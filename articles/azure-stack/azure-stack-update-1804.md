---
title: Обновление 1804 для Azure Stack | Документация Майкрософт
description: Узнайте, что находится в обновлении 1804 для интегрированных систем Azure Stack, о каких проблемах известно и где можно скачать обновление.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 9fc58d971db18db63e4dc76123ff1311b77e0191
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316439"
---
# <a name="azure-stack-1804-update"></a>Обновление 1804 для Azure Stack

*Область применения: интегрированные системы Azure Stack*

В этой статье представлены улучшения и исправления в пакете обновления 1804, известные проблемы выпуска и сведения о том, где можно скачать обновление. Известные проблемы можно разделить на проблемы, которые непосредственно относятся к процессу обновления и проблемы со сборкой (после установки).

> [!IMPORTANT]        
> Этот пакет обновления предназначен только для интегрированных систем Azure Stack. Не применяйте этот пакет обновления к Пакету средств разработки Azure Stack.

## <a name="build-reference"></a>Указание сборки    
Номер сборки обновления 1804 для Azure Stack — **20180513.1**.   

### <a name="new-features"></a>новые функции;
Это обновление включает в себя следующие улучшения и исправления для Azure Stack.

- <!-- 15028744 - IS -->  **Поддержка отключенных развертываний Azure Stack с использованием AD FS в Visual Studio**. В Visual Studio можно добавить подписки и пройти проверку подлинности с учетными данными федеративного пользователя AD FS. 
 
- <!-- 1779474, 1779458 - IS --> **Использование виртуальных машин серий Av2 и F**. Теперь в Azure Stack можно использовать виртуальные машины на основе размеров виртуальных машин серий Av2 и F. Дополнительные сведения см. в статье [Поддерживаемые размеры виртуальных машин в Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). 

- <!-- 1759172 - IS, ASDK --> **Новые административные подписки**. Начиная с версии 1804, на портале доступны два новых типа подписки. Эти новые типы подписки доступны в дополнение к подписке поставщика по умолчанию и видимы в новых установках Azure Stack, начиная с версии 1804. *Не используйте эти новые типы подписки с данной версией Azure Stack*. Мы сообщим о доступности этих типов подписки с будущим обновлением. 

  При обновлении Azure Stack до версии 1804 не отображаются два новых типа подписки. Однако новые развертывания интегрированных систем Azure Stack и установок пакета средств разработки Azure Stack 1804 или более поздней версии имеют доступ ко всем трем типам подписки.  

  Эти новые типы подписки являются частью большего изменения, предназначенного, чтобы защитить подписку поставщика по умолчанию и упростить развертывание общих ресурсов (например, серверов размещения SQL). По мере добавления частей этого большего изменения с будущими обновлениями Azure Stack ресурсы, развертываемые в эти новые типы подписок, могут быть утеряны. 

  Сейчас доступно три типа подписок:  
  - Подписка поставщика по умолчанию. Продолжайте использовать этот тип подписки. 
  - Подписка для учета. *Не используйте этот тип подписки.*
  - Подписка для контроля потребления. *Не используйте этот тип подписки.*

  



## <a name="fixed-issues"></a>Исправленные проблемы

- <!-- IS, ASDK --> На портале администрирования больше не нужно обновлять плитку обновления до того, как она отобразит информацию.
 
- <!-- 2050709  --> Теперь можно использовать портал администрирования для изменения метрик хранилища службы BLOB-объектов, службы таблиц и службы очередей.
 
- <!-- IS, ASDK --> В разделе **Сети** при нажатии кнопки **Подключение** для настройки VPN-подключение **Сеть — сеть (IPsec)** — единственный доступный вариант.

- **Различные исправления** производительности, стабильности, безопасности и операционной системы, используемой службой Azure Stack.

## <a name="additional-releases-timed-with-this-update"></a>Дополнительные выпуски, приуроченные к этому обновлению  
Ниже приведены компоненты, которые теперь доступны, но не требуют установки обновления 1804 для Azure Stack.
- **Обновление для пакета мониторинга System Center Operations Manager для Microsoft Azure Stack**. Новая версия (1.0.3.0) пакета мониторинга System Center Operations Manager для Microsoft Azure Stack доступна для [скачивания](https://www.microsoft.com/download/details.aspx?id=55184). С этой версией можно использовать субъекты-службы при добавлении подключенного к сети развертывания Azure Stack. Эта версия также включает в себя функции управления обновлениями, которые дают возможность выполнять действия по исправлению непосредственно из Operations Manager. В нее добавлены новые панели мониторинга, отображающие поставщики ресурсов, единицы масштабирования и узлы единиц масштабирования.

- **Новая версия модуля PowerShell для ресурсов администратора Azure Stack (1.3.0)**.  Модуль PowerShell для Azure Stack версии 1.3.0 теперь доступен для установки. Эта версия включает в себя команды для всех поставщиков ресурсов администратора, предназначенных для управления Azure Stack.  В этом выпуске будет считаться устаревшим некоторое содержимое [репозитория](https://github.com/Azure/AzureStack-Tools) инструментов GitHub для Azure Stack. 

   Сведения об установке приведены в [указаниях](azure-stack-powershell-install.md) или [справке](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) для модуля Azure Stack 1.3.0. 

- **Первоначальный выпуск справочника по REST API Azure Stack**. Опубликован [справочник по API для всех поставщиков ресурсов администратора Azure Stack](https://docs.microsoft.com/rest/api/azure-stack/). 


## <a name="before-you-begin"></a>Перед началом работы    

### <a name="prerequisites"></a>предварительным требованиям
- Прежде чем применить обновление 1804 для Azure Stack, установите [обновление 1803](azure-stack-update-1803.md) для Azure Stack.    

### <a name="known-issues-with-the-update-process"></a>Известные проблемы с процессом обновления   
- Во время установки обновления 1804 могут отображаться оповещения с заголовком *Error – Template for FaultType UserAccounts.New is missing* (Ошибка: отсутствует шаблон для FaultType UserAccounts.New).  Эти оповещения можно игнорировать. Они будут закрыты автоматически после завершения обновления до версии 1804.   
 
- <!-- TBD - IS --> Не пытайтесь создавать виртуальные машины во время установки этого обновления. Дополнительные сведения об управлении обновлениями в Azure Stack см. в [этой статье](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Действия после обновления
После установки обновления 1804 установите все применимые исправления. Дополнительные сведения см. в статьях базы знаний по ссылке ниже, а также в статье о нашей [политике обслуживания](azure-stack-servicing-policy.md).  
 - [KB 4341392 — исправление Azure Stack 1.0.180523.11](https://support.microsoft.com/en-us/help/4341392).




### <a name="known-issues-post-installation"></a>Известные проблемы (после установки)
Ниже перечислены известные проблемы после установки сборки **20180513.1**.

#### <a name="portal"></a>Microsoft Azure
- <!-- 1272111 - IS --> Установка этой версии или обновление до нее могут привести к тому, что вы не сможете просматривать единицы масштабирования Azure Stack на портале администрирования.  
  Решение. Просмотрите сведения о единицах масштабирования, используя PowerShell. Дополнительные сведения см. в содержимом раздела [справки](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) по модулю Azure Stack версии 1.3.0. 

- <!-- 2332636 - IS --> При использовании AD FS для системы идентификации Azure Stack и обновлении до этой версии Azure Stack владелец подписки по умолчанию сбрасывается до встроенного пользователя **CloudAdmin**.  
  Решение. Чтобы устранить эту проблему после установки этого обновления, выполните шаг 3 из процедуры [Активация службы автоматизации для настройки отношений доверия с поставщиком утверждений в Azure Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1), чтобы сбросить владельца подписки поставщика по умолчанию.   

- <!-- TBD - IS ASDK --> Некоторые типы административных подписок недоступны.  При обновлении Azure Stack до этой версии два типа подписки, [введенные с версией 1804 ](#new-features), не отображаются в консоли. Это ожидаемое поведение. Доступные типы подписки: *Подписка для учета* и *Подписка для контроля потребления*. Эти типы подписок еще не готовы к использованию, но их уже можно увидеть в новых средах Azure Stack, начиная с версии 1804. Продолжайте использовать тип подписки *Поставщик по умолчанию*.  


- <!-- TBD -  IS ASDK --> Возможность [открыть новый запрос на поддержку из раскрывающегося списка](azure-stack-manage-portals.md#quick-access-to-help-and-support) на портале администрирования недоступна. Вместо этого перейдите по следующей ссылке:     
    - Для интегрированных систем Azure Stack используйте https://aka.ms/newsupportrequest.

- <!-- 2403291 - IS ASDK --> Возможно, вы не используете горизонтальную полосу прокрутки в нижней части административного и пользовательских порталов. Если не удается получить доступ к горизонтальной полосе прокрутки, с помощью строки навигации перейдите в предыдущую колонку портала, выбрав имя колонки, которую нужно просмотреть, в списке навигации в верхней левой части портала.
  ![Элемент навигации](media/azure-stack-update-1804/breadcrumb.png) 

- <!-- TBD - IS --> Просмотр вычислений или ресурсов хранилища на портале администрирования может оказаться невозможным. Причиной является ошибка, которая произошла во время установки обновления. Из-за нее об обновлении было неверно сообщено (как об успешном). При возникновении этой ошибки обратитесь за помощью в службу поддержки корпорации Майкрософт.

- <!-- TBD - IS --> На портале может появиться пустая панель мониторинга. Чтобы восстановить панель мониторинга, щелкните значок шестеренки в правом верхнем углу портала и выберите **Восстановление параметров по умолчанию**.

- <!-- TBD - IS ASDK --> Удаление подписки пользователя приводит к появлению потерянных ресурсов. Чтобы избежать этого, следует сначала удалить ресурсы пользователя или всю группу ресурсов и лишь затем удалять подписки пользователя.

- <!-- TBD - IS ASDK --> Вы не можете просматривать разрешения для подписки на порталах Azure Stack. Используйте PowerShell, чтобы проверить разрешения.

- <!-- TBD - IS ASDK --> На портале администрирования может появиться критическое оповещение о компоненте *Microsoft.Update.Admin*. В полях "Имя оповещения", "Описание" и "Исправление" отображается следующее сообщение:  
    - *ERROR - Template for FaultType ResourceProviderTimeout is missing.* (Ошибка. Отсутствует шаблон FaultType ResourceProviderTimeout).

  Это оповещение можно проигнорировать. 


#### <a name="health-and-monitoring"></a>Работоспособность и мониторинг
- <!-- 1264761 - IS ASDK --> Вы можете просмотреть предупреждения компонента *Контроллер работоспособности* со следующими сведениями:  

   Предупреждение № 1:
   - НАЗВАНИЕ. Роль инфраструктуры неработоспособна.
   - СЕРЬЕЗНОСТЬ. Предупреждение.
   - КОМПОНЕНТ. Контроллер работоспособности.
   - ОПИСАНИЕ. Контроллер работоспособности сканера пульса недоступен. Это может повлиять на отчеты о работоспособности и метрики.  

  Предупреждение №2:
   - НАЗВАНИЕ. Роль инфраструктуры неработоспособна.
   - СЕРЬЕЗНОСТЬ. Предупреждение.
   - КОМПОНЕНТ. Контроллер работоспособности.
   - ОПИСАНИЕ. Контроллер работоспособности сканера ошибок недоступен. Это может повлиять на отчеты о работоспособности и метрики.

  Оба эти предупреждения можно проигнорировать. Они автоматически исчезнут через некоторое время.  
 

#### <a name="compute"></a>Службы вычислений
- <!-- TBD - IS --> При выборе размера виртуальной машины для развертывания некоторые размеры виртуальных машин серии F не отображаются в селекторе размера во время создания. Следующие размеры виртуальных машин не отображаются в селекторе: *F8s_v2*, *F16s_v2*, *F32s_v2* и *F64s_v2*.  
  Чтобы обойти эту проблему, используйте один из следующих методов для развертывания виртуальной машины. В каждом методе необходимо указать размер виртуальной машины, который вы хотите использовать.

  - **Шаблон Azure Resource Manager:** при использовании шаблона задайте в нем значение *vmSize*, равное желаемому размеру виртуальной машины. Например, приведенное ниже значение используется для развертывания виртуальной машины размера *F32s_v2*.  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** можно использовать команду [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) и указать размер виртуальной машины в качестве параметра. Пример: `--size "Standard_F32s_v2"`.

  - **PowerShell:** в PowerShell можно использовать командлет [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) с параметром, который указывает размер виртуальной машины. Пример: `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Параметры масштабирования для масштабируемых наборов виртуальной машины на портале недоступны. В качестве обходного решения используйте [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Из-за различий между версиями PowerShell используйте параметр `-Name` вместо параметра `-VMScaleSetName`.

- <!-- TBD - IS -->При создании группы доступности на портале (**Создать** > **Вычисления** > **Группа доступности**) вы можете создать ее только с одним доменом сбоя и одним доменом обновления. В качестве обходного решения при создании виртуальной машины создайте группу доступности с помощью PowerShell, CLI или на портале.

- <!-- TBD - IS ASDK --> При создании виртуальных машин на пользовательском портале Azure Stack отображается неверное число дисков данных, которые можно подключить к виртуальной машине серии D. Ко всем поддерживаемым виртуальным машинам серии D можно подключить такое же количество дисков данных, которое доступно в конфигурации Azure.

- <!-- TBD - IS ASDK --> При сбое создания образа виртуальной машины неисправный элемент, который вы не смогли удалить, может быть добавлен в колонку вычислений образов виртуальной машины.

  В качестве обходного пути создайте образ виртуальной машины с пустым VHD. Его можно создать с помощью Hyper-V (New-VHD -Path C:\dummy.vhd -Fixed -SizeBytes 1 GB). Этот процесс должен устранить проблему, которая не дает удалить неисправный элемент. Затем через 15 минут после создания фиктивного образа вы сможете удалить его.

  После этого вы можете попытаться повторно загрузить образ виртуальной машины, который ранее загрузить не удалось.

- <!-- TBD - IS ASDK --> Если подготовка расширения для развертывания виртуальной машины занимает слишком много времени, следует дождаться истечения времени ожидания, а не пытаться остановить процесс освобождения или удаления виртуальной машины.  

- <!-- 1662991 IS ASDK --> Диагностика виртуальных машин Linux не поддерживается в Azure Stack. При развертывании виртуальной машины Linux с включенной диагностикой оно завершается со сбоем. Развертывание также не выполняется, если базовые метрики виртуальной машины Linux включены через параметры диагностики.  


#### <a name="networking"></a>Сеть
- <!-- 1766332 - IS ASDK --> Если в разделе **Сеть** выбрать **Create VPN Gateway** (Создание VPN-шлюза) для настройки VPN-подключения, то в качестве типа VPN будет указан параметр **Policy Based** (На основе политики). Не выбирайте это подключение. В Azure Stack поддерживается только параметр **Route Based** (На основе маршрутов).

- <!-- 2388980 - IS ASDK --> После создания виртуальной машины и ее связывания с общедоступным IP-адресом вы не можете отменить эту связь. Будет выглядеть так, будто вам удалось его отменить, но ранее присвоенный общедоступный IP-адрес останется связанным с исходной виртуальной машиной.

  Сейчас для создания виртуальной машины необходимо использовать только новые общедоступные IP-адреса.

  Это происходит, даже если присвоить этот IP-адрес новой виртуальной машине (это часто называют *переключением виртуального IP-адреса*). Все последующие попытки подключиться через этот IP-адрес к новой виртуальной машине приведут к подключению к исходной виртуальной машине, к которой он был привязан.

- <!-- 2292271 - IS ASDK --> Если повысить квоту для ресурсов сети, которые являются частью предложения и плана, связанного с подпиской клиента, то новое ограничение не применяется к этой подписке. Тем не менее оно распространяется на новые подписки, созданные после увеличения квоты. 

  Чтобы обойти эту проблему, используйте план надстройки для повышения квоты сети, если план уже связан с подпиской. Чтобы получить дополнительные сведения, узнайте, как [сделать доступным план надстройки](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Невозможно удалить подписку, с которой связаны ресурсы зоны DNS или таблицы маршрутов. Чтобы успешно удалить подписку, необходимо сначала удалить из подписки клиента ресурсы зоны DNS и таблицы маршрутов. 
  

- <!-- 1902460 - IS ASDK --> Azure Stack поддерживает один *локальный сетевой шлюз* на IP-адрес. Это относится ко всем подпискам клиентов. После создания первого подключения к локальному сетевому шлюзу последующие попытки создать ресурс локального сетевого шлюза с тем же IP-адресом блокируются.

- <!-- 16309153 - IS ASDK --> В виртуальной сети, созданной с помощью параметра DNS-сервера *Автоматический*, попытка изменить на пользовательский DNS-сервер завершается с ошибкой. Обновленные параметры не передаются на виртуальные машины в этой виртуальной сети.

- <!-- TBD - IS ASDK --> Azure Stack не поддерживает добавление дополнительных сетевых интерфейсов к экземпляру виртуальной машины после ее развертывания. Если для виртуальной машины требуется несколько сетевых интерфейсов, их нужно определить во время развертывания.

- <!-- 2096388 IS --> Портал администрирования нельзя использовать для обновления правил для сетевой группы безопасности. 

    Обходной путь для службы приложений. Если вам нужно подключение с помощью удаленного рабочего стола к экземплярам контроллера, измените правила безопасности в группах безопасности сети с помощью PowerShell.  Ниже приведены примеры того, как *разрешить* конфигурацию, а затем восстановить для нее состояние *запрета*:  
    
    - *Разрешить:*
 
      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
      
      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
      
      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
      
      ##This doesn’t work. Need to set properties again even in case of edit
      
      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
      
      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
      
      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Запретить:*

        ```powershell
        
        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
        
        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
        
        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
        
        ##This doesn’t work. Need to set properties again even in case of edit
    
        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
    
        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
          
        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg 
        ```

#### <a name="sql-and-mysql"></a>SQL и MySQL

- <!-- TBD - IS --> Создание элементов на серверах размещения SQL и MySQL, если его выполняет не поставщик ресурсов, не поддерживается. и может привести к несоответствию состояний.  

- <!-- IS, ASDK --> При создании номера SKU для поставщиков ресурсов SQL и MySQL в именах **семейства** и **уровня** не поддерживаются специальные знаки, включая пробелы и точки.


> [!NOTE]  
> <!-- TBD - IS --> После обновления до Azure Stack 1804 вы можете продолжить использование ранее развернутых поставщиков ресурсов SQL и MySQL.  Рекомендуем обновлять SQL и MySQL при выходе нового выпуска. Применяйте обновления для поставщиков ресурсов SQL и MySQL последовательно (как и для Azure Stack).  Например, если вы используете версию 1802, сначала примените версию 1803, а затем обновите версию до 1804.      
>   
> Установка обновления 1804 не влияет на текущее использование поставщиков ресурсов SQL или MySQL пользователями.
> Независимо от версии используемых поставщиков ресурсов, данные пользователей в базах данных не затрагиваются и остаются доступными.    



#### <a name="app-service"></a>Служба приложений Azure
- <!-- 2352906 - IS ASDK --> Прежде чем создавать первую функцию Azure в подписке, пользователь должен зарегистрировать поставщик ресурсов хранилища.

- <!-- TBD - IS ASDK -->Чтобы масштабировать инфраструктуру (рабочие роли, роли управления, внешние роли), используйте PowerShell, как описано в заметках о выпуске служб вычислений.

- <!-- TBD - IS ASDK --> Сейчас службу приложений можно развернуть только в **подписке поставщика по умолчанию**.  В будущем обновлении службу приложений можно будет развернуть в новую подписку для контроля использования (представленную в Azure Stack 1804), в которую будут перенесены все имеющиеся развертывания.

#### <a name="usage"></a>Использование  
- <!-- TBD - IS ASDK --> Вместо метки *TimeDate*, которая показывает время создания записи, в данных инструмента для отслеживания использования общедоступного IP-адреса отображается одно значение *EventDateTime* для каждой записи. В настоящее время эти данные непригодны для выполнения точного расчета и получения данных об использовании общедоступного IP-адреса.


<!-- #### Identity -->
<!-- #### Marketplace --> 


## <a name="download-the-update"></a>Скачивание обновления
Пакет обновления 1804 для Azure Stack можно скачать [здесь](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>См. также
- Сведения об использовании привилегированной конечной точки (PEP) для отслеживания и возобновления обновлений см. в статье [Мониторинг обновлений в Azure Stack с помощью привилегированной конечной точки](azure-stack-monitor-update.md).
- [Общие сведения об управлении обновлениями в Azure Stack](azure-stack-updates.md).
- [Применение обновлений в Azure Stack](azure-stack-apply-updates.md).
