---
title: HDInsight — присоединенные к домену кластеры — Azure
description: Дополнительные сведения
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 3fd3a4b8982fe2170726df03bdc884e658d0b0c2
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019494"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Основные сведения о безопасности Hadoop в присоединенных к домену кластерах HDInsight

До сегодняшнего дня служба Azure HDInsight поддерживала локального администратора, работающего только с одним пользователем. Такой сценарий отлично подходил для небольших отделов по работе с приложениями. По мере набора популярности рабочих нагрузок Hadoop в корпоративном секторе увеличилась потребность в возможностях корпоративного уровня, таких как проверка подлинности на основе Active Directory, поддержка нескольких пользователей и управление доступом на основе ролей. С помощью присоединенных к домену кластеров HDInsight можно создать кластер HDInsight, присоединенный к домену Active Directory, настроить список сотрудников компании, которые могут пройти проверку подлинности в Azure Active Directory для входа в кластер HDInsight. Люди, которые не являются сотрудниками организации, не могут войти в кластер HDInsight или получить к нему доступ. Корпоративный администратор может настроить управление доступом на основе ролей для обеспечения защиты Hive с помощью [Apache Ranger](http://hortonworks.com/apache/ranger/) и ограничить доступ к данным до требуемого уровня. Наконец, администратор может провести аудит доступа к данным по сотрудникам, а также аудит любых изменений в политиках контроля доступа, обеспечивая высокий уровень контроля над корпоративными ресурсами.

> [!NOTE]
> Новые возможности, описанные в этой статье, доступны в режиме предварительной версии только для следующих типов кластера: Hadoop, Spark и Interactive Query. Диспетчер Oozie теперь работает в присоединенных к домену кластерах. Чтобы получить доступ к пользовательскому веб-интерфейсу Oozie, пользователям необходимо включить [туннелирование](../hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="benefits"></a>Преимущества
Безопасность корпоративного уровня разделяется на четыре обширных составляющих — безопасность периметра, аутентификация, авторизация и шифрование.

![Преимущества, предоставляемые основными составляющими присоединенного к домену кластера HDInsight](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Безопасность периметра
Безопасность периметра в HDInsight обеспечивается за счет виртуальных сетей и службы шлюза. Сейчас корпоративный администратор может создать кластер HDInsight в виртуальной сети и ограничить доступ к ней с помощью групп безопасности сети (правила брандмауэра). Для обмена данными с кластером HDInsight будут использоваться только IP-адреса, определенные в правилах исходящего трафика брандмауэра, тем самым обеспечивая безопасность периметра. Еще один уровень безопасности периметра обеспечивает шлюз. Шлюз — это служба, выступающая в качестве первой линии обороны для любого входящего запроса к кластеру HDInsight. Он принимает запрос, проверяет его и только после этого позволяет его передачу на другие узлы в кластере, обеспечивая таким образом безопасность периметра для других узлов имен и данных в кластере.

### <a name="authentication"></a>Authentication
Корпоративный администратор может создать присоединенный к домену кластер HDInsight в [виртуальной сети](https://azure.microsoft.com/services/virtual-network/). Узлы кластера HDInsight присоединяются к домену, управление которым осуществляется на корпоративном уровне. Для этого нужно использовать [доменные службы Azure Active Directory](../../active-directory-domain-services/active-directory-ds-overview.md). Все узлы в кластере присоединяются к домену, управление которым осуществляется на корпоративном уровне. При такой конфигурации сотрудники компании могут войти в узлы кластера с использованием своих учетных данных домена. Кроме того, с помощью этих учетных данные можно проверять подлинность в других утвержденных конечных точках, например представлениях Ambari, ODBC, JDBC, PowerShell и интерфейсах REST API, а также обеспечить взаимодействие с кластером. Администратор полностью управляет ограничением числа пользователей, взаимодействующих с кластером через эти конечные точки.

### <a name="authorization"></a>Авторизация
В большинстве компаний следуют такой рекомендации: доступ ко всем корпоративным ресурсам предоставляется не всем сотрудникам. Аналогичным образом в этом выпуске администратор может определить политики управления доступом на основе ролей для ресурсов кластера. Например, администратор может настроить [Apache Ranger](http://hortonworks.com/apache/ranger/), чтобы настроить политики контроля доступа для Hive. Эта возможность гарантирует, что сотрудники будут иметь доступ только к тем данным, которые им нужны для успешного выполнения задач. Только администраторы могут получить доступ к кластеру по протоколу SSH.

### <a name="auditing"></a>Аудит
Кроме защиты ресурсов кластера HDInsight от неавторизованных пользователей и защиты данных, для отслеживания неавторизованного или случайного доступа к ресурсам также требуется аудит доступа к ресурсам кластера и отслеживание данных. Администратор может просмотреть все случаи получения доступа к ресурсам и данным кластера HDInsight и сообщить о них. Кроме того, администратор может просмотреть все изменения в политиках контроля доступа, внесенные в поддерживаемых конечных точках Apache Ranger, и сообщить о них. Для поиска журналов аудита присоединенный к домену кластер HDInsight использует знакомый пользовательский интерфейс Apache Ranger. В серверной части Ranger использует [Apache Solr](http://hortonworks.com/apache/solr/) для хранения и поиска журналов.

### <a name="encryption"></a>Шифрование
Защита данных важна для соответствия требованиям корпоративной безопасности и обеспечения соответствия требованиям. Для защиты данных нужно не только ограничивать доступ к данным для несанкционированных сотрудников, но и шифровать их. Хранилища данных для кластеров HDInsight, хранилище BLOB-объектов Azure и Azure Data Lake Store поддерживают [шифрование неактивных данных](../../storage/common/storage-service-encryption.md) на стороне сервера. Защищенные кластеры HDInsight полностью совместимы с возможностью шифрования неактивных данных на стороне сервера.

## <a name="next-steps"></a>Дополнительная информация
* [Планирование архитектуры присоединенных к домену кластеров Hadoop в Azure HDInsight](apache-domain-joined-architecture.md)
* [Настройка присоединенных к домену кластеров HDInsight с помощью доменных служб Azure Active Directory](apache-domain-joined-configure.md)
* [Управление присоединенными к домену кластерами HDInsight](apache-domain-joined-manage.md)
* [Настройка политик Hive в присоединенном к домену кластере HDInsight](apache-domain-joined-run-hive.md)
* [Проверка подлинности при использовании присоединенного к домену кластера HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)

