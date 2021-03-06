---
title: 'Устранение сбоя службы Azure Backup: состояние гостевого агента "Недоступно"'
description: Симптомы, причины и способы устранения проблем в работе службы Azure Backup, связанных с агентом, расширением и дисками.
services: backup
author: genlin
manager: cshepard
keywords: Azure backup; VM agent; Network connectivity;
ms.service: backup
ms.topic: troubleshooting
ms.date: 06/25/2018
ms.author: genli
ms.openlocfilehash: 09cfda3c2c790297b0961ecac92cba61c9e6de6f
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754331"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Устранение неполадок службы Azure Backup. Проблемы с агентом или расширением

В этой статье описано, как устранять ошибки службы Azure Backup, связанные с установлением связи с агентом виртуальной машины и расширением.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>Агенту виртуальной машины не удается установить связь со службой Azure Backup

Сообщение об ошибке: "Агенту виртуальной машины не удается установить связь со службой Azure Backup".<br>
Код ошибки: UserErrorGuestAgentStatusUnavailable

После регистрации виртуальной машины в службе Backup и добавления ее в расписание служба Backup инициирует задание, взаимодействуя с агентом виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:

**Причина 1. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.  
**Причина 2. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  .  
**Причина 3. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.  
**Причина 4. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  .  
**Причина 5. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Сбой операции моментального снимка из-за отсутствия сетевого подключения у виртуальной машины

Сообщение об ошибке: "Сбой операции моментального снимка, отсутствует сетевое подключение у виртуальной машины".<br>
Код ошибки: ExtensionSnapshotFailedNoNetwork

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:    
**Причина 1. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.  
**Причина 2. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**.  
**Причина 3. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  

## <a name="vmsnapshot-extension-operation-failed"></a>Операция с расширением VMSnapshot завершается сбоем

Сообщение об ошибке: "Сбой в ходе работы расширения VMSnapshot".<br>
Код ошибки: ExtentionOperationFailed

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:  
**Причина 1. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**.  
**Причина 2. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  
**Причина 3. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**.  
**Причина 4. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Архивация завершается ошибкой, так как агент виртуальной машины не отвечает

Сообщение об ошибке "Не удалось запросить состояние моментального снимка в агенте виртуальной машины" <br>
Код ошибки: GuestAgentSnapshotTaskStatusError

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:  
**Причина 1. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**.  
**Причина 2. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.  
**Причина 3. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.  

## <a name="backup-fails-with-an-internal-error"></a>Внутренняя ошибка службы Backup

Сообщение об ошибке: "Произошла внутренняя ошибка архивации. Повторите операцию через несколько минут". <br>
Код ошибки: BackUpOperationFailed или BackUpOperationFailedV2

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:  
**Причина 1. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.  
**Причина 2. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**.  
**Причина 3. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.  
**Причина 4. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**.  
**Причина 5. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  
**Причина 6. [Служба Backup не имеет разрешения на удаление старых точек восстановления из-за блокировки группы ресурсов](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**.

## <a name="causes-and-solutions"></a>Причины и решения

### <a name="the-vm-has-no-internet-access"></a>Виртуальная машина не подключена к Интернету
Согласно требованиям к развертыванию виртуальная машина не имеет доступа к Интернету. Или могут быть ограничения на доступ к инфраструктуре Azure.

Чтобы расширение службы Backup работало правильно, требуется подключение к общедоступным IP-адресам Azure. Для управления моментальными снимками виртуальной машины это расширение отправляет команды к конечной точке службы хранилища Azure (URL-адрес HTTP). Если расширение не имеет доступа к общедоступному Интернету, резервное копирование завершится сбоем.

Чтобы маршрутизировать трафик виртуальных машин, можно развернуть прокси-сервер.
##### <a name="create-a-path-for-https-traffic"></a>Создание пути для трафика HTTP

1. При наличии каких-либо ограничений сети (например, группы безопасности сети) разверните прокси-сервер HTTP для маршрутизации трафика.
2. Если вы используете группу безопасности сети, добавьте в нее правила, чтобы разрешить доступ к Интернету с прокси-сервера HTTP.

Чтобы узнать, как настроить прокси-сервер HTTP для резервного копирования виртуальных машин, ознакомьтесь с разделом [Подготовка среды для резервного копирования виртуальных машин Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Архивной виртуальной машине или прокси-серверу, через которые маршрутизируется трафик, требуется доступ к общедоступным IP-адресам Azure.

####  <a name="solution"></a>Решение
Для устранения этой проблемы воспользуйтесь одним из указанных ниже способов.

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Разрешение доступа к хранилищу Azure, которое соответствует региону

Для разрешения подключений к хранилищу из определенного региона можно использовать [теги службы](../virtual-network/security-overview.md#service-tags). Убедитесь, что правило, которое позволяет получить доступ к учетной записи хранения, имеет более высокий приоритет, чем правило, которое блокирует доступ к Интернету. 

![Группа безопасности сети с тегами хранилища для региона](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Пошаговые инструкции по настройке тегов службы см. в [этом видео](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Теги службы хранилища находятся на этапе предварительной версии. Они доступны только в определенных регионах. Список регионов см. в разделе [Теги служб](../virtual-network/security-overview.md#service-tags).

Если вы используете "Управляемые диски Azure", может потребоваться открыть в брандмауэрах дополнительный порт (8443).

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)

#### <a name="solution"></a>Решение
Возможно, агент виртуальной машины может быть поврежден или служба остановлена. Повторная установка агента виртуальной машины поможет получить последнюю версию и возобновить обмен данными со службой.

1. Определите, запущена ли служба гостевого агента Windows в службах компьютера (services.msc) на виртуальной машине. Попробуйте перезапустить службу гостевого агента Windows и запустите резервное копирование.    
2. Если она не отображается в списке служб, на панели управления выберите **Программы и компоненты**, чтобы определить, установлена ли служба гостевого агента Windows.
4. Если гостевой агент Windows отображается в разделе **Программы и компоненты**, удалите его.
5. Скачайте и установите [последнюю версию файла MSI агента](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Чтобы выполнить установку, необходимо иметь права администратора.
6. Убедитесь, что службы гостевого агента Windows отображаются в списке служб.
7. Выполните резервное копирование по запросу: 
    * На портале выберите **Моментальная архивация**.

Кроме того, убедитесь, что [Microsoft .NET 4.5 установлен](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) на виртуальной машине. Компонент .NET 4.5 необходим для взаимодействия агента виртуальной машины со службой.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)

#### <a name="solution"></a>Решение
Большая часть неполадок, связанных с агентом или расширением на виртуальных машинах Linux, вызваны проблемами с устаревшим агентом виртуальной машины. Чтобы устранить эту проблему, следуйте приведенным ниже общим рекомендациям.

1. Выполните указания по [обновлению агента виртуальной машины Linux ](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Мы *настоятельно рекомендуем* обновлять агент только посредством репозитория дистрибутивов. Мы не рекомендуем скачивать код агента непосредственно с портала GitHub и обновлять его. Если последняя версия агента для вашего дистрибутива недоступна, обратитесь в службу поддержки дистрибутива, чтобы получить инструкции по установке последней версии агента. Чтобы проверить наличие последней версии агента, перейдите на страницу [агента Linux для Microsoft Azure](https://github.com/Azure/WALinuxAgent/releases) в репозитории GitHub.

2. Убедитесь, что на виртуальной машине запущен агент Azure, выполнив команду `ps -e`.

 Если нужный процесс не запущен, выполните следующие команды для его перезапуска:

 * Для Ubuntu: `service walinuxagent start`.
 * Для других дистрибутивов: `service waagent start`.

3. [Настройте автоматический перезапуск агента](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Снова запустите резервное копирование для проверки. Если ошибка не исчезла, извлеките следующие журналы из виртуальной машины:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Если нам потребуется подробное ведение журнала для waagent, выполните следующие действия.

1. В файле /etc/waagent.conf найдите такую строку: **Enable verbose logging (y|n)**.
2. Для параметра **Logs.Verbose** измените значение с *n* на *y*.
3. Сохраните изменения и перезапустите waagent, выполнив шаги, описанные выше в этом разделе.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Не удалось получить состояние моментального снимка или создать моментальный снимок
Архивация виртуальных машин зависит от команды создания моментального снимка в базовой учетной записи хранения. Сбой службы Backup может произойти, если она не имеет доступа к учетной записи хранения или выполнение задачи создания моментального снимка задерживается.

#### <a name="solution"></a>Решение
Сбой задачи создания снимка может быть вызван следующими условиями.

| Причина: | Решение |
| --- | --- |
| Состояние виртуальной машины отображается неправильно из-за того, что ее работа была завершена в протоколе удаленного рабочего стола (RDP). | Когда вы завершаете работу виртуальной машины в RDP, проверьте, правильно ли отображается состояние виртуальной машины на портале. Если это не так, завершите работу виртуальной машины на портале с помощью операции **Завершение работы** на панели мониторинга виртуальной машины. |
| Виртуальная машина не может получить адрес узла или структуры по протоколу DHCP. | Чтобы архивация виртуальной машины IaaS работала, в гостевой учетной записи нужно включить протокол DHCP. Если виртуальная машина не может получить адрес узла или структуры в виде ответа DHCP 245, то она не сможет скачать или запустить какие-либо расширения. Если вам нужен статический частный IP-адрес, настройте его через платформу. DHCP для виртуальной машины следует оставить включенным. Дополнительные сведения см. в статье [Как задать статический внутренний частный IP-адрес с помощью PowerShell (классическая модель)](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Не удалось обновить или загрузить расширение резервного копирования
Если не удается загрузить расширения, получить моментальный снимок состояния невозможно, и архивация завершится сбоем.

#### <a name="solution"></a>Решение

Удалите расширение, чтобы принудительно перезагрузить расширение VMSnapshot. Расширение перезагрузится при следующей попытке резервного копирования.

Чтобы удалить расширение, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) перейдите к виртуальной машине, с архивацией которой возникли проблемы.
2. Выберите элемент **Параметры**.
3. Выберите **Расширения**.
4. Выберите **Vmsnapshot Extension** (Расширение Vmsnapshot).
5. Выберите **Удалить**.

Если расширение VMSnapshot для виртуальной машины Linux не отображается на портале Azure, [обновите агент Azure для Linux](../virtual-machines/linux/update-agent.md), а затем выполните резервное копирование. 

После этого расширение будет повторно установлено при следующей архивации.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Служба Backup не имеет разрешения на удаление старых точек восстановления из-за блокировки группы ресурсов
Эта проблема характерна для управляемых машин, в которых пользователь блокирует группу ресурсов. В этом случае служба Backup не может удалить устаревшие точки восстановления. Так как максимальное число точек восстановления не превышает 18, новые операции резервного копирования завершаются сбоем.

#### <a name="solution"></a>Решение

Чтобы устранить проблему, снимите блокировку группы ресурсов и удалите коллекции точек восстановления, выполнив следующие шаги: 
 
1. Удалите блокировку в группе ресурсов, в которой находится виртуальная машина. 
2. Установите ARMClient с помощью Chocolatey: <br>
   https://github.com/projectkudu/ARMClient
3. Войдите в ARMClient: <br>
    `.\armclient.exe login`
4. Получите коллекцию точек восстановления, соответствующую виртуальной машине: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Пример: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Удалите коллекцию точек восстановления: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. При следующем запланированном резервном копировании автоматически создается коллекция точек восстановления и новые точки восстановления.

Затем можно возобновить блокировку группы ресурсов виртуальной машины. 
