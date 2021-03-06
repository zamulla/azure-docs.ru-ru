---
title: Архитектура присоединенного к домену кластера Azure HDInsight | Документация Майкрософт
description: Сведения о планировании архитектуры присоединенного к домену кластера HDInsight.
services: hdinsight
documentationcenter: ''
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 8503534031dc5774e64c58edd3e158162a5a6aee
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110461"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Планирование архитектуры присоединенных к домену кластеров Hadoop в Azure HDInsight

Стандартный кластер HDInsight рассчитан на одного пользователя. Он подходит для большинства организаций с небольшими отделами по работе с приложениями, создающими объемные рабочие нагрузки данных. У каждого пользователя может быть отдельный кластер, выделенный по требованию, который можно уничтожить, если он больше не требуется. Однако многие организации переходят на новый режим работы. В этом режиме ИТ-специалисты управляют кластерами, которые совместно используются несколькими отделами разработки приложений. Таким образом, в Azure HDInsight этим более крупным предприятиям требуется многопользовательский доступ к кластеру.

HDInsight использует наиболее популярный поставщик удостоверений — управляемый Active Directory (AD). С помощью интеграции HDInsight с [доменными службами Azure Active Directory (AAD DS)](../../active-directory-domain-services/active-directory-ds-overview.md) можно получить доступ к кластерам путем использования учетных данных домена. Виртуальная машина в HDInsight присоединяется к предоставленному домену. Благодаря этому аутентифицированный пользователь может легко переключаться между всеми пользовательскими службами, запущенными в HDInsight (Ambari, сервер Hive, Ranger, сервер Thrift Spark и т. д.). Администраторы могут настраивать строгие политики авторизации на основе Apache Ranger, чтобы применять управление доступом на основе ролей для ресурсов в кластер.


## <a name="integrate-hdinsight-with-active-directory"></a>Интеграция HDInsight с Active Directory

Hadoop с открытым кодом использует Kerberos для предоставления аутентификации и обеспечения безопасности. Таким образом, узлы кластера HDInsight присоединены к домену под управлением доменных служб AAD-DS. Для включенных в кластер компонентов Hadoop настраиваются параметры безопасности Kerberos. Для каждого компонента Hadoop в субъект-служба создается автоматически. Также субъект создается для каждого компьютера, присоединенного к домену. Для хранения этих субъектов служб и компьютеров необходимо предоставить подразделение (OU) в контроллере домена (AAD-DS), в которых эти субъекты располагаются. 

Кратко перечислим, что вам нужно создать в сетевом окружении:

- Домен Active Directory (управляется AAD-DS).
- Защищенный LDAP (LDAPS), включенный в AAD-DS.
- Правильное сетевое подключение из виртуальной сети HDInsight к виртуальной сети AAD DS, если выбрать отдельные виртуальные сети для них. Виртуальная машина внутри виртуальной сети HDI должна напрямую обращаться к AAD-DS с помощью пиринга виртуальной сети. Если HDI и AAD-DS развернуты в одной виртуальной сети, возможность подключения автоматически предоставляется и никаких дальнейших действий не требуется.
- Подразделение (OU), [созданное в AAD-DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).
- учетную запись службы со следующими разрешениями:
    - на создание субъектов-служб в подразделении;
    - на присоединение компьютеров к домену и создание субъектов компьютеров в подразделении.

На приведенном ниже снимке экрана представлено такое подразделение в домене contoso.com. Также вы видите здесь несколько субъектов-служб и субъектов компьютеров.

![Подразделение в кластере HDInsight, присоединенном к домену](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="different-domain-controllers-setup"></a>Настройка отличающихся контроллеров доменов:
HDInsight в настоящее время поддерживает только AAD-DS в качестве основного контроллера домена, с которым кластер Kerberise обменивается данными. Однако также возможны другие сложные настройки AD, при условии, что это приведет к включению AAD-DS для доступа к HDI.

- **[Доменные службы Azure Active Directory (AAD-DS).](../../active-directory-domain-services/active-directory-ds-overview.md)** Эта служба предоставляет управляемый домен, который полностью совместим с Windows Server Active Directory. Корпорация Майкрософт управляет доменом, применяет к нему исправления и осуществляет его мониторинг домена в конфигурации высокой доступности. Вы можете развернуть свой кластер, не беспокоясь о поддержке контроллеров домена. Пользователи, группы и пароли синхронизируются из Azure Active Directory [односторонняя синхронизация AAD с AAD-DS]. Это позволяет пользователям входить в кластер с теми же корпоративными учетными данными. Дополнительные сведения см. в статье [Настройка присоединенных к домену кластеров HDInsight с помощью доменных служб Azure Active Directory](./apache-domain-joined-configure-using-azure-adds.md).
- **Локальная служба AD или служба AD на виртуальных машинах IaaS**. При наличии локальной службы AD или других более сложных установок AD для домена, вы можете синхронизировать эти удостоверения с AAD, используя AD Connect, а затем включить AAD-DS в этом клиенте AD. Так как Kerberos использует хэши паролей, необходимо будет [включить синхронизацию хэша пароля на AAD-DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). При использовании федерации со службами федерации доменных служб (ADFS) можно при необходимости установить синхронизацию хэша пароля в качестве запасного варианта в случае сбоя инфраструктуры ADFS. Дополнительные сведения см. в статье [Реализация синхронизации хэшированных паролей в службе синхронизации Azure AD Connect](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). Использование локальной службы AD или отдельной службы AD на виртуальных машинах IaaS (без AAD и AAD-DS) не поддерживается для присоединения кластера HDI к домену.

## <a name="next-steps"></a>Дополнительная информация
* [Настройка присоединенных к домену кластеров HDInsight с помощью доменных служб Azure Active Directory](apache-domain-joined-configure-using-azure-adds.md)
* [Настройка политик Hive в присоединенном к домену кластере HDInsight](apache-domain-joined-run-hive.md)
* [Управление присоединенными к домену кластерами HDInsight](apache-domain-joined-manage.md) 
