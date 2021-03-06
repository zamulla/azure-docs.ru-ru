---
title: 'Azure Backup: подготовка к архивации виртуальных машин'
description: Убедитесь, что ваша среда подготовлена для архивации виртуальных машин в Azure.
services: backup
author: markgalioto
manager: carmonm
keywords: резервные копии; резервное копирование;
ms.service: backup
ms.topic: conceptual
ms.date: 6/21/2018
ms.author: markgal
ms.openlocfilehash: 06898877a4f13182230c6d5fb12544f90525d84d
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960174"
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Подготовка среды к архивации виртуальных машин, развернутых с помощью Resource Manager

В этой статье приведены шаги по подготовке среды к резервному копированию виртуальной машины, развернутой с помощью Azure Resource Manager. Шаги процедур выполняются на портале Azure. При создании резервной копии виртуальной машины данные резервного копирования или точки восстановления хранятся в хранилище служб восстановления. Хранилища служб восстановления содержат данные резервного копирования для виртуальных машин, развернутых с помощью классической модели и с помощью Resource Manager.

> [!NOTE]
> В Azure предусмотрены две модели развертывания, позволяющие создавать ресурсы и работать с ними: [модель Resource Manager и классическая модель](../azure-resource-manager/resource-manager-deployment-model.md).

Чтобы защитить виртуальную машину или создать ее резервную копию, развернутую с помощью Resource Manager, необходимо выполнить следующие предварительные требования:

* Создайте или укажите хранилище служб восстановления *в том же регионе, в котором размещена ваша виртуальная машина*.
* Выберите сценарий, определите политику архивации, а также элементы, которые необходимо защитить.
* Проверьте, установлен ли на виртуальной машине соответствующий агент (расширение).
* Проверьте сетевое подключение.
* Если для виртуальных машин Linux требуется настроить среду архивации для создания резервных копий с согласованием приложений, следуйте [инструкциям по настройке сценариев, выполняемых перед созданием моментального снимка и после него](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Если эти условия в вашей среде уже выполнены, переходите к статье [Архивация виртуальных машин Azure в хранилище служб восстановления](backup-azure-arm-vms.md). Если нужно выполнить любые из этих предварительных требований или проверить их соблюдение, в этой статье описаны соответствующие шаги.

## <a name="supported-operating-systems-for-backup"></a>Поддерживаемые версии операционных систем для резервного копирования

 * **Linux**: служба Azure Backup поддерживает весь [список дистрибутивов, рекомендованных для использования в Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), за исключением CoreOS Linux. Список операционных систем Linux, которые поддерживают восстановление файлов, см. в разделе [Восстановление файлов из резервной копии виртуальной машины](backup-azure-restore-files-from-vm.md#for-linux-os).

    > [!NOTE] 
    > Другие собственные дистрибутивы Linux также должны работать, если агент виртуальной машины доступен на виртуальной машине и есть поддержка Python. Но эти дистрибутивы не поддерживаются.
    >
 * **Windows Server**, **клиент Windows**: версии до Windows Server 2008 R2 или Windows 7 не поддерживаются.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Ограничения при резервном копировании и восстановлении виртуальной машины
Перед подготовкой среды следует учитывать такие ограничения:

* Архивация виртуальных машин, к которым подключено более 16 дисков данных, не поддерживается.
* Архивация виртуальных машин с зарезервированным IP-адресом и без заданной конечной точки не поддерживается.
* Резервное копирование виртуальных машин Linux, зашифрованных с помощью шифрования LUKS, не поддерживается.
* Мы не рекомендуем создавать резервные копии виртуальных машин, которые содержат общие тома кластера (CSV) или конфигурацию масштабируемого файлового сервера. В противном случае возможен сбой модулей записи CSV. Для этих компонентов предполагается задействование всех виртуальных машин, включенных в конфигурацию кластера при выполнении задачи создания моментального снимка. Служба Azure Backup не поддерживает согласованность нескольких виртуальных машин. 
* Данные резервных копий не содержат установленные в сети диски, подключенные к виртуальной машине.
* Замена существующей виртуальной машины во время восстановления не поддерживается. При попытке восстановить имеющуюся виртуальную машину произойдет сбой операции.
* Резервное копирование и восстановление между регионами не поддерживаются.
* При настройке резервного копирования убедитесь, что настройки **брандмауэров и виртуальных сетей** учетной записи хранения разрешают доступ из всех сетей.
* Для выбранных сетей после настройки брандмауэра и параметров виртуальной сети для вашей учетной записи хранения выберите **Разрешить доверенным службам Майкрософт доступ к этой учетной записи хранения** как исключение, чтобы разрешить доступ службы Azure Backup к ограниченной сетью учетной записи хранения. Для ограниченных сетью учетных записей хранения не поддерживается восстановление на уровне элементов.
* Резервные копии виртуальных машин можно создавать в любых общедоступных регионах Azure. (См. [список](https://azure.microsoft.com/regions/#services) поддерживаемых регионов.) Если искомый регион в настоящее время не поддерживается, он будет отсутствовать в раскрывающемся списке при создании хранилища.
* Восстановление контроллера домена виртуальной машины, которая входит в конфигурацию с несколькими контроллерами домена, поддерживается только с помощью PowerShell. Дополнительные сведения см. в разделе о [восстановлении контроллера домена в конфигурации с несколькими контроллерами домена](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Моментальный снимок диска c включенным ускорителем записи не поддерживается. Это ограничение блокирует возможность службы Azure Backup создавать моментальный снимок, согласованный с приложением, всех дисков виртуальной машины.
* Восстановление виртуальных машин с описанными ниже особыми конфигурациями сети поддерживается только в PowerShell. Когда процедура восстановления будет завершена, виртуальные машины, созданные в пользовательском интерфейсе с помощью рабочего процесса восстановления, не будут включать в себя эти конфигурации сети. Дополнительные сведения см. в разделе [Восстановление виртуальных машин с помощью специальных конфигураций сети](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Виртуальные машины в конфигурации балансировщика нагрузки (внутренняя и внешняя)
  * Виртуальные машины с несколькими зарезервированными IP-адресами
  * Виртуальные машины с несколькими сетевыми адаптерами

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Создание хранилища служб восстановления для виртуальной машины
Хранилище служб восстановления — это сущность, в которой хранятся созданные резервные копии и точки восстановления. В нем также содержатся политики архивации, связанные с защищенными виртуальными машинами.

Чтобы создать хранилище служб восстановления, сделайте следующее:

1. Войдите на [портале Azure](https://portal.azure.com/).
2. В меню **Центр** выберите **Обзор**, а затем введите **Службы восстановления**. По мере ввода данных фильтруется список ресурсов. Выберите **Хранилища служб восстановления**.

    ![Введите в поле и выберите пункт "Хранилища служб восстановления" в результатах](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    После этого отобразится список хранилищ служб восстановления,
3. В меню **Хранилища служб восстановления** выберите **Добавить**.

    ![Создание хранилища служб восстановления — шаг 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Откроется область **Хранилища служб восстановления**. Вам будет предложено ввести сведения об **имени**, **подписке**, **группе ресурсов** и **расположении**.

    ![Область хранилищ служб восстановления](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. В поле **Имя**введите понятное имя хранилища. Имя должно быть уникальным в пределах подписки Azure. Введите имя, которое содержит от 2 до 50 символов. Имя должно начинаться с буквы, оно может содержать только буквы, цифры и дефисы.
5. Выберите **Подписка**, чтобы просмотреть список доступных подписок. Если неизвестно, какую подписку нужно использовать, оставьте подписку по умолчанию (или предлагаемую подписку). Вариантов будет несколько только в том случае, если рабочая или учебная учетная запись связана с несколькими подписками Azure.
6. Выберите **Группа ресурсов**, чтобы просмотреть список доступных групп ресурсов, или создайте группу ресурсов, выбрав **Создать**. Дополнительные сведения о группах ресурсов см. в [обзоре Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Щелкните **Расположение**, чтобы выбрать географический регион, в котором будет находиться хранилище. Хранилище *должно* находиться в том же регионе, что и виртуальные машины, которые необходимо защитить.

   > [!IMPORTANT]
   > Если вы не уверены, в каком расположении находится ваша виртуальная машина, закройте диалоговое окно создания хранилища и перейдите к списку виртуальных машин на портале. Если у вас есть виртуальные машины в нескольких регионах, вам необходимо создать хранилище служб восстановления в каждом из них. Прежде чем переходить к следующему расположению, необходимо создать хранилище в первом расположении. Не нужно указывать учетные записи хранения для хранения данных резервного копирования. Хранилище служб восстановления и служба Azure Backup обрабатывают это автоматически.
   >
   >

8. Нажмите кнопку **Создать**. Для создания хранилища служб восстановления может потребоваться некоторое время. Следите за уведомлениями о состоянии на портале в верхней правой области. После создания хранилище появится в списке хранилищ служб восстановления. Если вы не видите хранилища, выберите **Обновить**.

    ![Список хранилищ службы архивации](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Теперь, когда вы создали хранилище, можно приступить к настройке репликации.

## <a name="set-storage-replication"></a>Настройка репликации хранилища
При настройке репликации хранилища можно выбирать между геоизбыточным хранилищем и локально избыточным хранилищем. По умолчанию это геоизбыточное хранилище. Если резервная копия является основной, оставьте значение параметра по умолчанию. Если вам нужно более дешевое и не такое надежное решение, выберите локально избыточное хранилище.

Чтобы изменить параметр репликации хранилища:

1. В области **Хранилища служб восстановления** выберите хранилище.
    При выборе хранилища открывается область **Параметры** (с именем хранилища вверху), а также область со сведениями о хранилище.

   ![Выбор хранилища из списка резервных хранилищ](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. В области **Параметры** используйте вертикальный ползунок, чтобы прокрутить вниз к разделу **Управление**, и выберите **Инфраструктура резервного копирования**. В разделе **Общее** выберите **Конфигурация архивации**. В области **Конфигурация архивации** выберите вариант репликации для своего хранилища. По умолчанию это геоизбыточное хранилище.

   ![Список хранилищ службы архивации](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Если в качестве конечной точки основного хранилища резервных копий используется Azure, продолжайте использовать геоизбыточное хранилище. Если Azure используется в качестве конечной точки вторичного хранилища резервных копий, выберите локально избыточное хранилище. Дополнительные сведения о вариантах хранения см. в [обзоре репликации службы хранилища Azure](../storage/common/storage-redundancy.md).

3. Если вы изменили тип репликации хранилища, выберите **Сохранить**.
    
Выбрав параметры хранилища, можно приступать к связыванию виртуальной машины с хранилищем. Перед началом связывания нужно обнаружить и зарегистрировать виртуальные машины Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Выбор цели резервного копирования, настройка политики и определение объектов для защиты
Прежде чем регистрировать виртуальную машину в хранилище служб восстановления, запустите процесс обнаружения, чтобы определить все недавно добавленные в подписку виртуальные машины. Процесс обнаружения запрашивает в Azure список виртуальных машин в подписке. При обнаружении новых виртуальных машин на портале отображаются имя облачной службы и соответствующий регион. На портале Azure *сценарием* является то, что вы поместите в хранилище служб восстановления. *Политика*. Это расписание, которое определяет частоту и время создания точек восстановления. Политика также включает диапазон хранения для точек восстановления.

1. Если хранилище служб восстановления уже открыто, перейдите к шагу 2. Если у вас нет открытого хранилища служб восстановления, откройте [портал Azure](https://portal.azure.com/). В меню **Центр** выберите **Больше служб**.

   a. В списке ресурсов введите **Службы восстановления**. Список будет отфильтрован по мере ввода данных. Когда вы увидите пункт **Хранилища служб восстановления**, щелкните его.

      ![Введите в поле и выберите пункт "Хранилища служб восстановления" в результатах](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      После этого отобразится список хранилищ служб восстановления, Если в подписке нет хранилищ, список останется пустым.

      ![Просмотр списка хранилищ служб восстановления](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   Б. в котором нужно выбрать хранилище.

      Для выбранного хранилища откроются область **Параметры** и область мониторинга хранилища.

      ![Область параметров и область мониторинга хранилища](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. В меню панели мониторинга хранилища выберите **Архивация**.

   ![Кнопка "Архивация"](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Откроются области **Архивация** и **Цель резервного копирования**.

3. В области **Цель резервного копирования** для параметра **Where is your workload running?** (Где выполняется рабочая нагрузка?) установите значение **Azure**, а для параметра **What do you want to backup?** (Для чего необходимо создать резервную копию?) — значение **Виртуальная машина**. Нажмите кнопку **ОК**.

   ![Области "Архивация" и "Цель резервного копирования"](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   При этом расширение виртуальной машины зарегистрируется в хранилище. После этого область **Цель резервного копирования** закроется и откроется область **Политика архивации**.

   ![Области "Архивация" и "Политика архивации"](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. В области **Политика архивации** выберите политику архивации для своего хранилища.

   ![Выбор политики резервного копирования](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   Подробные сведения о политике по умолчанию указаны в раскрывающемся меню. Если вы хотите создать политику, в раскрывающемся меню щелкните **Создать**. Инструкции по определению политики резервного копирования см. в разделе [Определение политики резервного копирования](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Чтобы связать политику архивации с хранилищем, нажмите кнопку **ОК**.

   После этого область **Цель резервного копирования** закроется и откроется область **Выбор виртуальных машин**.
5. Выберите виртуальные машины в области **Выбор виртуальных машин**, чтобы связать их с указанной политикой, и нажмите кнопку **ОК**.

   ![Область "Выбор виртуальных машин"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   Выбранная виртуальная машина пройдет проверку. Если ожидаемые виртуальные машины, не отобразятся проверьте, находятся ли они в том же регионе Azure, что и хранилище служб восстановления. Если виртуальные машины по-прежнему не отображаются, проверьте, не защищены ли они с помощью другого хранилища. Регион хранилища служб восстановления отображается на панели мониторинга хранилища.

6. Теперь, когда все параметры для хранилища заданы, в области **Архивация** выберите **Включить резервное копирование**. В результате выполнения этого шага для хранилища и виртуальных машин будет развернута политика. На этом шаге создается начальная точка восстановления для виртуальной машины.

   ![Кнопка "Включить резервное копирование"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

После успешного включения резервного копирования политика архивации будет выполняться по расписанию. Если вы хотите создать задание резервного копирования по запросу, чтобы выполнить архивацию виртуальных машин, см. раздел [Активация задания архивации](./backup-azure-vms-first-look-arm.md#initial-backup).

Если у вас возникли проблемы с регистрацией виртуальной машины, см. сведения об установке агента виртуальной машины и сетевом подключении. Если защита виртуальных машин, созданных в Azure, включена, следующие сведения можно пропустить. Однако если вы перенесли виртуальные машины в Azure, убедитесь, что агент виртуальной машины установлен должным образом и что виртуальная машина может подключиться к виртуальной сети.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Установка агента виртуальной машины на виртуальной машине
[Агент виртуальной машины](../virtual-machines/extensions/agent-windows.md) Azure необходимо установить на виртуальной машине Azure, чтобы обеспечить работоспособность модуля резервного копирования. Если виртуальная машина создана из Azure Marketplace, агент уже существует на виртуальной машине. 

Следующая информация приведена для случаев, когда *не* используется виртуальная машина, созданная в Azure Marketplace. **Например, вы перенесли виртуальную машину из локального центра обработки данных. В таком случае необходимо установить агент виртуальной машины, чтобы защитить ее.**

**Примечание**. После установки агента виртуальной машины необходимо также использовать Azure PowerShell для обновления свойства ProvisionGuestAgent, чтобы сообщить Azure, что на виртуальной машине установлен агент. 

Если у вас возникли сложности с архивацией виртуальной машины Azure, убедитесь, что на ней правильно установлен агент виртуальной машины Azure (см. таблицу ниже). В таблице приведены дополнительные сведения об агенте для виртуальных машин Windows и Linux.

| **Операция** | **Windows** | **Linux** |
| --- | --- | --- |
| Установка агента VM |Скачайте и установите [MSI-файл агента](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Чтобы выполнить установку, необходимо иметь права администратора. |<li> Установите последнюю версию [агента Linux](../virtual-machines/extensions/agent-linux.md). Чтобы выполнить установку, необходимо иметь права администратора. Мы рекомендуем устанавливать агент из репозитория дистрибутива. Мы **не рекомендуем** устанавливать агент виртуальной машины Linux непосредственно из Github.  |
| Обновление агента виртуальной машины |Обновление агента виртуальной машины — это простая процедура, схожая с переустановкой [двоичных файлов агента виртуальной машины](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Необходимо убедиться, что во время обновления агента виртуальной машины не выполняются операции архивации. |Следуйте указаниям по [обновлению агента виртуальной машины Linux ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Мы рекомендуем обновлять агент из репозитория дистрибутива. Мы **не рекомендуем** обновлять агент виртуальной машины Linux непосредственно из Github.<br>Необходимо убедиться, что во время обновления агента виртуальной машины не выполняются операции резервного копирования. |
| Проверка установки агента VM |<li>На виртуальной машине Azure перейдите в папку *C:\WindowsAzure\Packages*. <li>В ней должен находиться файл WaAppAgent.exe.<li> Щелкните правой кнопкой мыши этот файл, выберите пункт **Свойства** и перейдите на вкладку **Подробно**. В поле "Версия продукта" должно отображаться значение 2.6.1198.718 или выше. |Недоступно |

### <a name="backup-extension"></a>Расширение резервного копирования
Когда агент будет установлен на виртуальной машине, служба Azure Backup установит для агента модуль резервного копирования. Служба Azure Backup легко обновляет и применяет исправления к расширению резервного копирования.

Расширение резервного копирования устанавливается службой Azure Backup независимо от того, запущена ли виртуальная машина. На запущенной виртуальной машине проще получить точку восстановления, согласованную с приложениями. При этом служба Azure Backup продолжает архивацию виртуальной машины, даже если она выключена и нельзя установить расширение. Это называется *автономной виртуальной машиной*. В этом случае точка восстановления будет *отказоустойчивой*.

## <a name="establish-network-connectivity"></a>Установка сетевого подключения
Чтобы управлять моментальными снимками виртуальной машины, для расширения резервного копирования требуется подключение к общедоступным IP-адресам Azure. Без правильного подключения к Интернету время ожидания HTTP-запросов от виртуальной машины истекает, а архивация завершается сбоем. Если для развертывания установлены ограничения доступа на месте, например через группу безопасности сети (NSG), выберите один из следующих вариантов, чтобы открыть путь для трафика резервного копирования:

* [Добавьте диапазоны IP-адресов центра обработки данных Azure в список разрешений.](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
* Разверните прокси-сервер HTTP для маршрутизации трафика.

Решая, какой вариант использовать, обратите внимание на соотношение управляемости, детального управления и затрат.

| Параметр | Преимущества | Недостатки |
| --- | --- | --- |
| Разрешенные диапазоны IP-адресов |Нет дополнительных затрат.<br><br>Нет дополнительных затрат. Открыть доступ в NSG можно с помощью командлета **Set-AzureNetworkSecurityRule**. |Сложность управления, так как задействованные диапазоны IP-адресов со временем меняются.<br><br>Доступ ко всей службе Azure, а не только к хранилищу. |
| Использование прокси-сервера HTTP |Разрешено точное управление разрешенными URL-адресами хранилища в прокси-сервере.<br><br>Единая точка доступа к виртуальным машинам через Интернет.<br><br>Не подвергается влиянию изменений IP-адресов Azure. |Дополнительные затраты для работы виртуальной машины с программным обеспечением прокси-сервера. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Добавьте диапазоны IP-адресов центра обработки данных Azure в список разрешений.
Чтобы добавить диапазоны IP-адресов центра обработки данных Azure в список разрешений, ознакомьтесь с дополнительными сведениями о диапазонах IP-адресов и указаниями на [веб-сайте Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653).

Для разрешения подключений к хранилищу из определенного региона можно использовать [теги службы](../virtual-network/security-overview.md#service-tags). Убедитесь, что правило, которое позволяет получить доступ к учетной записи хранения, имеет более высокий приоритет, чем правило, которое блокирует доступ к Интернету. 

![NSG с тегами хранилища для региона](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Приведенное ниже видео ознакомит вас с пошаговой процедурой настройки тегов службы. 

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!WARNING]
> Теги службы хранилища доступны только в определенных регионах и находятся на этапе предварительной версии. Список регионов см. в разделе [Теги служб](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>Использование прокси-сервера HTTP для резервного копирования виртуальных машин
Во время резервного копирования виртуальных машин команды управления моментальными снимками отправляются из расширения резервного копирования в службу хранилища Azure с помощью API HTTPS. Направьте трафик расширения резервного копирования через прокси-сервер HTTP, потому что это единственный компонент, который настроен для общего доступа в Интернете.

> [!NOTE]
> Мы не рекомендуем использовать какое-либо специальное программное обеспечение прокси-сервера. Убедитесь, что прокси-сервер подходит для выполнения описанных ниже настроек.
>
>

На приведенном ниже изображении показан пример трех этапов настройки, которые необходимо выполнить, чтобы использовать прокси-сервер HTTP:

* Виртуальная машина приложения направляет весь HTTP-трафик для общего доступа в Интернете через виртуальную машину прокси-сервера.
* В виртуальной машине прокси-сервера разрешен входящий трафик от виртуальных машин в виртуальной сети.
* Для группы безопасности сети с именем NSF-lockdown требуется правило безопасности, которое разрешает передачу интернет-трафика из виртуальной машины прокси-сервера.

Чтобы использовать прокси-сервер HTTP для взаимодействия с общедоступным Интернетом, выполните такие действия.

> [!NOTE]
> Имена и значения на этих шагах указаны только для примера. Используйте имена и значения своего развертывания, вводя или вставляя сведения в свой код.

#### <a name="step-1-configure-outgoing-network-connections"></a>Шаг 1. Настройка исходящих сетевых подключений
###### <a name="for-windows-machines"></a>Для компьютеров Windows
Эта процедура настраивает конфигурацию прокси-сервера для учетной записи локальной системы.

1. Скачайте [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Откройте Internet Explorer, выполнив следующую команду с повышенными привилегиями:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

3. Откройте Internet Explorer и последовательно выберите **Сервис** > **Свойства браузера** > **Подключения** > **Настройка параметров локальной сети**.
4. Проверьте параметры прокси-сервера для системной учетной записи. Задайте IP-адрес прокси-сервера и порт.
5. Закройте Internet Explorer.

Следующий скрипт настраивает для компьютера прокси-сервер, который будет использоваться для любого исходящего трафика HTTP или HTTPS. Если вы настроили прокси-сервер из текущей учетной записи пользователя, а не учетной записи Local System, используйте скрипт для применения к SYSTEMACCOUNT.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Если в журнале прокси-сервера отобразится сообщение "407 — Требуется проверка подлинности прокси", проверьте правильность настройки проверки подлинности.
>
>

###### <a name="for-linux-machines"></a>Для компьютеров Linux
Добавьте следующую строку в файл ```/etc/environment``` :

```
http_proxy=http://<proxy IP>:<proxy port>
```

Добавьте следующие строки в файл ```/etc/waagent.conf``` :

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Шаг 2. Разрешение входящих подключений на прокси-сервере
1. Откройте брандмауэр Windows на прокси-сервере. Самый простой способ доступа к брандмауэру — это поиск **брандмауэра Windows в режиме повышенной безопасности**.
2. В диалоговом окне **Брандмауэр Windows в режиме повышенной безопасности** щелкните правой кнопкой мыши **Правила для входящего трафика** и выберите **Новое правило**.
3. В мастере новых правил для входящего трафика на странице **Тип правила** выберите параметр **Настраиваемый** и нажмите кнопку **Далее**.
4. На странице **Программа** выберите **Все программы** и нажмите кнопку **Далее**.
5. На странице **Протокол и порты** введите следующие сведения и нажмите кнопку **Далее**:
   * В поле **Тип протокола** выберите **TCP**.
   * В поле **Локальный порт** выберите **Определенные порты**. В поле ниже укажите номер порта прокси-сервера, который был настроен.
   * В поле **Удаленный порт** выберите **Все порты**.

В оставшейся части мастера принимайте параметры по умолчанию, пока не дойдете до конца. Затем дайте этому правилу имя. 

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Шаг 3. Добавление правила исключения к группе NSG
Она добавляет исключение к группе NSG. Это исключение разрешает TCP-трафик из любого порта на сервере 10.0.0.5 на любой адрес в Интернете по порту 80 (HTTP) или 443 (HTTPS). Если вам необходимо разрешить общий доступ из Интернета к какому-либо порту, добавьте этот порт в параметр ```-DestinationPortRange```.

Введите следующую команду в командной строке Azure PowerShell:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Вопросы?
Если вы хотите задать вопрос или предложить добавить какие-либо функции, [отправьте нам свой отзыв](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Дополнительная информация
Следующий шаг после подготовки среды для резервного копирования виртуальной машины — это создание резервной копии. Более подробные сведения о резервном копировании виртуальных машин см. в статье по планированию.

* [Настройка виртуальных машин](backup-azure-arm-vms.md)
* [Планирование инфраструктуры резервного копирования виртуальных машин в Azure](backup-azure-vms-introduction.md)
* [Управление резервными копиями виртуальной машины](backup-azure-manage-vms.md)
