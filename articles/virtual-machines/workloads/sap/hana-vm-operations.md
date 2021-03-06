---
title: Операции SAP HANA в Azure | Документация Майкрософт
description: Руководство по использованию систем SAP HANA, развернутых на виртуальных машинах Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: juergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2480ad464f2fc716cf68672387a189aeb92f5737
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918838"
---
# <a name="sap-hana-on-azure-operations-guide"></a>Руководство по использованию SAP HANA в Azure
Этот документ содержит рекомендации по работе с системами SAP HANA, развернутыми на виртуальных машинах Azure. Он не предназначен для замены стандартной документации SAP, к которой относятся следующие ресурсы:

- [SAP HANA Administration Guide](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html) (Руководство по администрированию SAP HANA);
- [SAP Installation Guides](https://service.sap.com/instguides) (Руководство по установке SAP);
- [Примечания к SAP](https://sservice.sap.com/notes).

## <a name="prerequisites"></a>предварительным требованиям
Чтобы воспользоваться приведенными в этом руководстве сведениями, необходимо иметь базовые знания об использовании следующих компонентов Azure:

- [Виртуальные машины Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Сети и виртуальные сети Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network).
- [Хранилище Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Дополнительные сведения по SAP NetWeaver и другим компонентам SAP на базе Azure см. в статье [Using Azure for hosting and running SAP workload scenarios](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) (Размещение и выполнение сценариев рабочей нагрузки SAP с помощью Azure) и [документации по Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Рекомендации по основной настройке
В следующих разделах описаны рекомендации по основной настройке развертывания систем SAP HANA на виртуальных машинах Azure.

### <a name="connect-into-azure-virtual-machines"></a>Подключение к виртуальным машинам Azure
Как описано в статье [SAP NetWeaver на виртуальных машинах Windows. Руководство по планированию и внедрению](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), есть два основных метода подключения к виртуальным машинам Azure:

- Подключение через Интернет и общедоступные конечные точки на виртуальной машине перехода или виртуальной машине, работающей под управлением SAP HANA.
- Подключение через [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) или Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Подключение "сеть — сеть" через VPN или ExpressRoute необходимо для производственных сценариев. Этот тип подключения также необходим для нерабочих сценариев, связанных с производственными сценариями, в которых используется программное обеспечение SAP. Пример межсайтового подключения приведен на следующем рисунке:

![Межсайтовые подключения](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Выбор типов виртуальных машин Azure
Типы виртуальных машин Azure, которые можно использовать для производственных сценариев, можно найти [в документации SAP для IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Для нерабочих сценариев доступен более широкий спектр собственных типов виртуальных машин Azure.

>[!NOTE]
> Используйте для этих сценариев типы виртуальных машин, которые указаны в [примечании к SAP № 1928533](https://launchpad.support.sap.com/#/notes/1928533). В рабочей среде можно использовать виртуальные машины Azure, сертифицированные для SAP HANA. Они указаны в опубликованном [списке сертифицированных платформ IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html) для SAP.

Разверните виртуальные машины в Azure с помощью:

- портала Azure;
- командлетов Azure PowerShell;
- Azure CLI.

Вы также можете развернуть полностью установленную платформу SAP HANA в службы виртуальных машин Azure через [облачную платформу SAP](https://cal.sap.com/). Сведения о процессе установки см. в статье [Развертывание SAP S/4HANA или BW/4HANA в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h), а сведения об автоматизации — на сайте [GitHub](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Выбор типа службы хранилища Azure
Azure предоставляет два типа хранилища, подходящих для виртуальных машин Azure, работающих под управлением SAP HANA:

- [Хранилище Azure уровня "Стандартный"](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Хранилище Azure Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure предлагает два метода развертывания виртуальных жестких дисков (VHD) в хранилище Azure класса "Стандартный" и хранилище Azure класса Premium. Рекомендуется использовать развертывания [управляемых дисков Azure](https://azure.microsoft.com/services/managed-disks/), если общий сценарий это допускает.

Список типов хранилища и их соглашения об уровне обслуживания в отношении операций ввода-вывода в секунду (IOPS) и пропускной способности хранилища см. на странице [Цены на управляемые диски](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Настройка хранилища для виртуальных машин Azure

Ранее вы покупали устройства SAP HANA для локальной среды и вам не нужно было беспокоиться о подсистемах операций ввода-вывода и их возможностях, потому что поставщик устройств должен сам убедиться в том, что для SAP HANA выполнены минимальные требования к хранилищу. Теперь вы создаете инфраструктуру Azure самостоятельно, поэтому вам необходимо ознакомиться с некоторыми из этих требований, чтобы также понимать требования к конфигурации, описанные в следующих разделах. Кроме того, это необходимо в случае настройки виртуальных машин, на которых будет выполняться SAP HANA. Для некоторых запрашиваемых характеристик необходимо выполнить следующее.

- Включите том чтения и записи в /hana/log с пропускной способностью 250 МБ/с и минимальным размером операций ввода-вывода в 1 МБ.
- Включите действие чтения с пропускной способностью не менее 400 МБ/с для /hana/data для следующих размеров операции ввода-вывода: 16 МБ и 64 МБ.
- Включите действие записи с пропускной способностью не менее 250 МБ/с для /hana/data со следующими размерами операций ввода-вывода: 16 МБ и 64 МБ.

Так как небольшая задержка хранилища для систем СУБД является критической, даже такие системы, как SAP HANA, хранят данные в памяти. Критический путь в хранилище обычно находится рядом с записями журналов транзакций систем СУБД. Кроме того, такие операции, как запись точек сохранения или загрузка данных в памяти после восстановления из-за сбоя, могут быть критическими. Таким образом обязательно необходимо использовать диски Azure уровня "Премиум" для томов /hana/data и /hana/log. Чтобы добиться минимальной пропускной способности /hana/log и /hana/data в соответствии с требованиями SAP, создайте RAID 0 с помощью MDADM или LVM на нескольких дисках хранилища Azure класса Premium и используйте тома RAID в качестве томов /hana/data и /hana/log. В качестве размеров блоков для RAID 0 рекомендуем использовать следующие:

- 64 или 128 КБ для /hana/data;
- 32 KБ для /hana/log.

> [!NOTE]
> Нет необходимости настраивать уровень избыточности с помощью томов RAID, так как хранилища Azure класса Premium и Standard хранят три образа виртуального жесткого диска. Использование тома RAID предназначено исключительно для настройки томов, которые предоставляют достаточную пропускную способность операций ввода-вывода.

Использование нескольких VHD Azure в RAID влияет на число операций ввода-вывода в секунду и пропускную способность хранилища. Таким образом, если вы добавите в RAID 0 3 диска P30 хранилища Azure класса Premium, число операций ввода-вывода в секунду и пропускная способность увеличатся в три раза, исходя из показателей одного диска P30 хранилища класса Premium.

Приведенные ниже рекомендации по кэшированию даны исходя из следующих характеристик ввода-вывода для SAP HANA:

- Практически отсутствует рабочая нагрузка чтения файлов данных HANA. Исключениями являются большие по размеру операции ввода-вывода после перезапуска экземпляра HANA или перезагрузки виртуальной машины Azure, когда данные загружаются в HANA. Еще одним случаем большего размера операций чтения файлов данных является резервное копирование базы данных HANA. В этом случае кэширование чтения практически не имеет смысла, поскольку в большинстве случаев все тома файлов данных необходимо считать полностью.
- Запись в файлы данных возникает скачкообразно в связи с созданием точек сохранения HANA и восстановления после сбоя. Запись точек сохранения выполняется асинхронно и не задерживает выполнение пользовательских транзакций. При записи данных во время восстановления после сбоя производительность является критически важной, поскольку необходимо восстановить работу системы как можно быстрее. Однако восстановление после сбоя происходит только в исключительных ситуациях
- Вряд ли возникнут другие ситуации, когда понадобится чтение из файлов повторяемых операций HANA. Исключениями являются большие операции ввода-вывода при выполнении резервных копий журналов транзакций, восстановления после сбоя или во время перезапуска экземпляра HANA.  
- Основной нагрузкой на файл журнала повторяемых операций SAP HANA являются операции записи. В зависимости от характера рабочей нагрузки можно иметь операции ввода-вывода размером 4 КБ, тогда как в других случаях размер может превышать 1 МБ. Задержки записи для журнала повторяемых операций SAP HANA являются критическими для производительности.
- Все операции записи необходимо сохранить на диске надежным образом

В зависимости от представленных шаблонов операций ввода-вывода SAP HANA кэширование для разных томов, использующих хранилища Azure класса Premium, должно устанавливаться следующим образом:

- /hana/data — без кэширования
- /hana/log — без кэширования, за исключением серии M (см. далее в этом документе)
- /hana/shared — кэширование чтения


Кроме того, при выборе виртуальной машины или изменении ее размера учитывайте пропускную способность ввода-вывода. Общие сведения о пропускной способности хранилища виртуальной машины см. в статье [Размеры виртуальных машин, оптимизированных для операций в памяти](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="cost-conscious-azure-storage-configuration"></a>Стоимость конфигурации хранилища Azure
В следующей таблице показана конфигурация типов виртуальных машин, которые клиенты часто используют для размещения SAP HANA на виртуальных машинах Azure. Некоторые типы виртуальных машин не соответствуют всем требованиям для SAP HANA. Но пока эти виртуальные машины отлично работали в нерабочих средах. 

> [!NOTE]
> Для рабочих сред проверьте [в документации SAP для IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), поддерживает ли SAP определенный тип виртуальной машины для SAP HANA.


| SKU виртуальной машины | ОЗУ | Макс. пропускная способность<br /> Throughput | /hana/data и /hana/log,<br /> чередующиеся с диспетчером логических томов или MDADM | /hana/shared | том /root | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 ГБ | 768 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 ГБ | 384 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 Гиб | 768 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 ГиБ | 1200 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 ГиБ | 2000 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 ГиБ | 500 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 Гиб | 500 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 ГБ | 1000 МБ/с | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 ГиБ | 1000 МБ/с | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 Гиб | 1000 МБ/с | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 ГиБ | 2000 МБ/с |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 Гиб | 2000 МБ/с | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Диски 3 x P20, рекомендуемые для типов виртуальных машин меньшего размера, превышают размер томов согласно рекомендациям относительно дискового пространства, указанным в [техническом документе для хранилища SAP TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Тем не менее этот вариант, как показано в таблице, был выбран, чтобы обеспечить необходимую пропускную способность диска для SAP HANA. При необходимости вы можете изменить размер тома /hana/backup, подобранный для хранения резервных копий, которые в два раза превышают объем памяти тома.   
Проверьте, будет ли пропускная способность хранилища для разных предлагаемых томов соответствовать рабочей нагрузке, которую вы хотите запустить. Если для рабочей нагрузки необходимы большие тома /hana/data и /hana/log, необходимо увеличить количество виртуальных жестких дисков хранилища Azure класса Premium. Изменение размера тома с большим количеством VHD, чем указано, увеличит пропускную способность операций ввода-вывода и число IOPS в пределах ограничений типа виртуальной машины Azure. 

> [!NOTE]
> В приведенных выше конфигурациях было бы невыгодно использовать [соглашение об уровне обслуживания для отдельной виртуальной машины Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), так как оно использует хранилища класса Standard и Premium одновременно. Однако выбор был сделан для оптимизации затрат.


#### <a name="azure-storage-configuration-to-benefit-for-meeting-single-vm-sla"></a>Конфигурация хранилища Azure для использования соглашения об уровне обслуживания для отдельной виртуальной машины
Если вы хотите воспользоваться преимуществами [соглашения об уровне обслуживания для отдельной виртуальной машины Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), необходимо использовать только VHD хранилища Azure класса Premium.

> [!NOTE]
> Для рабочих сред проверьте [в документации SAP для IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), поддерживает ли SAP определенный тип виртуальной машины для SAP HANA.

| SKU виртуальной машины | ОЗУ | Макс. пропускная способность<br /> Throughput | /hana/data и /hana/log,<br /> чередующиеся с диспетчером логических томов или MDADM | /hana/shared | том /root | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 ГБ | 768 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E16v3 | 128 ГБ | 384 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E32v3 | 256 Гиб | 768 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| E64v3 | 443 ГиБ | 1200 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| GS5 | 448 ГиБ | 2000 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M32ts | 192 ГиБ | 500 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M32ls | 256 Гиб | 500 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M64ls | 512 ГБ | 1000 МБ/с | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M64s | 1000 ГиБ | 1000 МБ/с | 2 x P30 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 Гиб | 1000 МБ/с | 3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 ГиБ | 2000 МБ/с |3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 Гиб | 2000 МБ/с | 5 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |


Диски 3 x P20, рекомендуемые для типов виртуальных машин меньшего размера, превышают размер томов согласно рекомендациям относительно дискового пространства, указанным в [техническом документе для хранилища SAP TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Тем не менее этот вариант, как показано в таблице, был выбран, чтобы обеспечить необходимую пропускную способность диска для SAP HANA. При необходимости вы можете изменить размер тома /hana/backup, подобранный для хранения резервных копий, которые в два раза превышают объем памяти тома.  
Проверьте, будет ли пропускная способность хранилища для разных предлагаемых томов соответствовать рабочей нагрузке, которую вы хотите запустить. Если для рабочей нагрузки необходимы большие тома /hana/data и /hana/log, необходимо увеличить количество виртуальных жестких дисков хранилища Azure класса Premium. Изменение размера тома с большим количеством VHD, чем указано, увеличит пропускную способность операций ввода-вывода и число IOPS в пределах ограничений типа виртуальной машины Azure. 



#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Решение хранилища с ускорителем записи Azure для виртуальных машин Azure серии M
Ускоритель записи Azure — это функция, которая предназначена исключительно для виртуальных машин серии M. Как очевидно из названия, целью функции является уменьшение задержки операций ввода-вывода записей в хранилище Azure класса Premium. В SAP HANA ускоритель записи нужно использовать только для тома /hana/log. Поэтому приведенные выше конфигурации необходимо изменить. Основное изменение — настроить использование ускорителя записи Azure только для тома /hana/log. 

> [!IMPORTANT]
> Сертификация SAP HANA для виртуальных машин Azure серии M предоставлена исключительно для конфигурации, где ускоритель записи Azure используется только для тома /hana/log. В результате ожидается, что в развертываниях производственного сценария SAP HANA на виртуальных машинах Azure серии M ускоритель записи Azure будет настроен для тома /hana/log.  

> [!NOTE]
> Для рабочих сред проверьте [в документации SAP для IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), поддерживает ли SAP определенный тип виртуальной машины для SAP HANA.

Рекомендуемые конфигурации выглядят следующим образом.

| SKU виртуальной машины | ОЗУ | Макс. пропускная способность<br /> Throughput | hana/data; | hana/log; | /hana/shared | том /root | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 ГиБ | 500 МБ/с | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 Гиб | 500 МБ/с | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 ГБ | 1000 МБ/с | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 ГиБ | 1000 МБ/с | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 Гиб | 1000 МБ/с | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 ГиБ | 2000 МБ/с |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 Гиб | 2000 МБ/с | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Проверьте, будет ли пропускная способность хранилища для разных предлагаемых томов соответствовать рабочей нагрузке, которую вы хотите запустить. Если для рабочей нагрузки необходимы большие тома /hana/data и /hana/log, необходимо увеличить количество виртуальных жестких дисков хранилища Azure класса Premium. Изменение размера тома с большим количеством VHD, чем указано, увеличит пропускную способность операций ввода-вывода и число IOPS в пределах ограничений типа виртуальной машины Azure.

Ускоритель записи Azure работает только с [управляемыми дисками Azure](https://azure.microsoft.com/services/managed-disks/). Поэтому в качестве управляемых дисков необходимо развернуть по крайней мере диски хранилища Azure класса Premium, формирующие том /hana/log.

Есть ограничения на число виртуальных жестких дисков хранилища Azure класса Premium для каждой виртуальной машины, которые могут поддерживаться ускорителем записи Azure. Сейчас действуют такие ограничения:

- 16 виртуальных жестких дисков для виртуальной машины серии M128xx;
- 8 виртуальных жестких дисков для виртуальной машины серии M64xx.
- 4 виртуальных жестких диска для виртуальной машины серии M32xx.

Подробные инструкции по включению ускорителя записи Azure см. в статье [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) (Ускоритель записи).

Подробные сведения и ограничения для ускорителя записи Azure можно найти в той же статье.


### <a name="set-up-azure-virtual-networks"></a>Настройка виртуальных сетей Azure
Если у вас есть подключение "сеть — сеть" в Azure через VPN или ExpressRoute, у вас должна быть как минимум одна [виртуальная сеть Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), которая подключается через виртуальный шлюз к сети VPN или ExpressRoute. Виртуальный шлюз находится в подсети виртуальной сети Azure. Чтобы установить SAP HANA, необходимо создать две дополнительные подсети в виртуальной сети. Одну подсеть, в которой размещаются виртуальные машины, на которых будут работать экземпляры SAP HANA, и другую подсеть, в которой будут работать виртуальная машина Jumpbox или виртуальная машина управления, для размещения SAP HANA Studio или другого программного обеспечения для управления.

При установке виртуальных машин для запуска SAP HANA они должны иметь следующее:

- Два виртуальных сетевых адаптера, один из которых подключается к подсети управления, а другой используется для подключения из локальной сети или других сетей к экземпляру SAP HANA на виртуальной машине Azure.
- Статические частные IP-адреса, развернутые для обоих виртуальных сетевых адаптеров.

Общие сведения о различных методах назначения IP-адресов см. в статье [Типы IP-адресов и методы распределения в Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Для виртуальных машин, работающих под управлением SAP HANA, необходимо назначить статические IP-адреса. Причиной является то, что некоторые атрибуты конфигурации для HANA ссылаются на IP-адреса.

С помощью [групп безопасности сети (NSG) Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) можно перенаправить трафик, поступающий к экземпляру SAP HANA или Jumpbox. Группы безопасности сети связаны с подсетью SAP HANA и подсетью управления.

На следующем рисунке представлен обзор приблизительной схемы развертывания SAP HANA:

![Приблизительная схема развертывания SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Чтобы развернуть SAP HANA в Azure без подключения "сеть — сеть", необходимо получить доступ к экземпляру SAP HANA с помощью общедоступного IP-адреса, назначенного виртуальной машине Azure, на которой работает виртуальная машина Jumpbox. В этом стандартном сценарии для развертывания используются встроенные DNS-службы Azure для разрешения имен узлов. Встроенные DNS-службы Azure особо важны при более сложном развертывании, в котором используются общедоступные IP-адреса. С помощью групп безопасности сети Azure можно ограничить открытые порты или диапазоны IP-адресов, которые могут подключаться к подсетям Azure с ресурсами с IP-адресами для общего доступа. На следующем рисунке показана предварительная схема для развертывания SAP HANA без подключения "сеть — сеть":
  
![Примерная схема развертывания SAP HANA без подключения "сеть — сеть"](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Операции развертывания SAP HANA на виртуальных машинах Azure
В следующих разделах описаны некоторые операции, связанные с развертыванием систем SAP HANA на виртуальных машинах Azure.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Операции архивации и восстановления на виртуальных машинах Azure
Следующие документы содержат сведения об архивации и восстановлении развертывания SAP HANA:

- [Обзор резервного копирования SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Резервное копирование SAP HANA на уровне файлов](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Резервное копирование SAP HANA на основе моментальных снимков хранилища](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Запуск и перезапуск виртуальных машин с SAP HANA
Отличительной чертой общедоступного облака Azure является то, что плата взимается только за время вычисления. Например, при завершении работы виртуальной машины под управлением SAP HANA будет выставлен счет только за расходы на хранение за это время. Другая функция доступна при указании статических IP-адресов для виртуальных машин в начальном развертывании. Если перезагрузить виртуальную машину с SAP HANA, она перезапустится с предыдущим IP-адресом. 


### <a name="use-saprouter-for-sap-remote-support"></a>Удаленная поддержка SAP с помощью SAPRouter
Если вы установили подключение "сеть — сеть" между локальными расположениями и Azure и используете компоненты SAP, вероятно, у вас уже есть SAProuter. В этом случае выполните следующие действия для включения удаленной поддержки:

- Сохраните частный и статический IP-адрес виртуальной машины, на которой размещается SAP HANA, в конфигурации SAProuter.
- Настройте группу безопасности сети подсети, в которой размещена виртуальная машина HANA, чтобы разрешить трафик через порт TCP/IP 3299.

Если вы подключаетесь к Azure через Интернет и у вас нет маршрутизатора SAP для виртуальной машины с SAP HANA, необходимо установить компонент. Установите SAProuter на отдельной виртуальной машине в подсети управления. На следующем рисунке показана примерная схема для развертывания SAP HANA с SAProuter и без подключения "сеть — сеть":

![Примерная схема развертывания SAP HANA без подключения "сеть — сеть" и SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

SAProuter следует установить на отдельной виртуальной машине, а не на виртуальной машине Jumpbox. Отдельная виртуальная машина должна иметь статический IP-адрес. Чтобы подключить ваш SAProuter к SAProuter, размещенный в SAP, обратитесь к SAP для получения IP-адреса. (SAProuter, размещенный в SAP, является аналогом экземпляра SAProuter, установленного на виртуальной машине.) С помощью IP-адреса SAP можно настроить экземпляр SAProuter. В параметрах конфигурации требуется только TCP-порт 3299.

Дополнительные сведения о том, как настроить и поддерживать подключения удаленной поддержки через SAProuter, см. [в документации по SAP](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Высокий уровень доступности SAP HANA на собственных виртуальных машинах Azure
Если вы используете SUSE Linux 12 SP1 или более поздней версии, можно установить кластер Pacemaker с устройствами STONITH. С помощью этих устройств можно настроить конфигурацию SAP HANA, которая использует синхронную репликацию с репликацией системы HANA и автоматическим переходом на другой ресурс. Дополнительные сведения об установке см. в статье [Руководство по обеспечению высокого уровня доступности SAP HANA на виртуальных машинах Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
