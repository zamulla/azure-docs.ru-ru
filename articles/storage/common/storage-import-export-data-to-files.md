---
title: Использование службы "Импорт и экспорт Azure" для передачи данных в службу "Файлы Azure" | Документация Майкрософт
description: Сведения о создании заданий импорта на портале Azure для передачи данных в службу "Файлы Azure".
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 4349b471f960e7844511c473bffcd2177a34e055
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660331"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Использование службы "Импорт и экспорт Azure" для импорта данных в службу "Файлы Azure"

В этой статье содержатся пошаговые инструкции по использованию службы "Импорт и экспорт Azure" для безопасного импорта больших объемов данных в службу "Файлы Azure". Чтобы служба осуществила импорт данных, нужно отправить в центр обработки данных Azure поддерживаемые диски с данными.  

Служба "Импорт и экспорт Azure" поддерживает только импорт файлов Azure в службу хранилища Azure. Экспорт файлов Azure не поддерживается.

## <a name="prerequisites"></a>предварительным требованиям

Перед созданием задания импорта для передачи данных в службу "Файлы Azure" внимательно просмотрите и выполните описанные ниже предварительные требования. Необходимо следующее:

- Активная подписка Azure для использования со службой импорта и экспорта.
- Как минимум одна учетная запись хранения Azure. Просмотрите список [поддерживаемых типов хранилища и учетных записей хранения для службы "Импорт и экспорт"](storage-import-export-requirements.md). Сведения о создании учетной записи хранения см. в разделе [Создание учетной записи хранения](storage-create-storage-account.md#create-a-storage-account).
- Соответствующее количество дисков [поддерживаемых типов](storage-import-export-requirements.md#supported-disks). 
- Система с ОС Windows [поддерживаемой версии](storage-import-export-requirements.md#supported-operating-systems).
- [Скачать WAImportExport версии 2](https://www.microsoft.com/download/details.aspx?id=55280) в систему Windows. Распакуйте содержимое в папку по умолчанию: `waimportexport`. Например, `C:\WaImportExport`.


## <a name="step-1-prepare-the-drives"></a>Шаг 1. Подготовка дисков

На этом шаге создается файл журнала. В файле журнала хранятся основные сведения, например серийный номер диска, ключ шифрования и сведения об учетной записи хранения.

Выполните следующие действия для подготовки дисков.

1. Подключите наши диски к системе Windows через соединители SATA.
2. Создайте один том NTFS на каждом диске. Присвойте ему букву диска. Не используйте точки подключения.
3. Измените файл *dataset.csv* в корневой папке, в которой находится средство. В зависимости от того, что импортируется, — файл, папка или и то и другое, добавьте записи в файл *dataset.csv*, как показано в следующем примере.  

    - **Чтобы импортировать файл**. В следующем примере данные для копирования находятся на диске С. Файл *MyFile1.txt* копируется в корень папки *MyAzureFileshare1*. Если папка *MyAzureFileshare1* не существует, она создается в учетной записи хранения Azure. Структура папок сохраняется.

        ```
            BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
    
        ```
    - **Чтобы импортировать папку**. Все файлы и папки, расположенные в *MyFolder2_original*, будут рекурсивно скопированы в общую папку. Структура папок сохраняется.

        ```
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            
        ```
    В одном файле можно сделать несколько записей, соответствующих импортируемым папкам или файлам. 

        ```
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            "F:\MyFolder3\MyFile3.txt","MyAzureFileshare2/",file,rename,"None",None 
            
        ```
    Дополнительные сведения о подготовке CSV-файла набора данных см. в [этом разделе](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    

4. Измените файл *driveset.csv* в корневой папке, где находится средство. Добавьте записи в файл *driveset.csv*, как показано в следующих примерах. Файл с набором дисков содержит список дисков и соответствующих букв дисков, чтобы средство могло правильно выбрать список дисков, которые будут подготовлены.

    В этом примере предполагается, что подключено два диска, а базовые тома NTFS G:\ и H:\ уже созданы. H:\ не зашифрован, а G: уже зашифрован. Средство форматирует и шифрует диск, который содержит только H:\ (а не G:\).

    - **Для диска, который не зашифрован:** укажите *Encrypt*, чтобы включить шифрование BitLocker на диске.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        H,Format,SilentMode,Encrypt,
        ```
    
    - **Для диска, который уже зашифрован:** укажите *AlreadyEncrypted* и введите ключ BitLocker.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
        ```

    В тот же файл можно внести несколько записей, соответствующих нескольким дискам. Дополнительные сведения о подготовке CSV-файла набора дисков см. в [этом разделе](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file). 

5.  Используйте параметр `PrepImport`, чтобы скопировать и подготовить данные для диска. Для первого сеанса копирования каталогов и файлов в рамках нового сеанса копирования выполните следующую команду:

        ```
        .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
        ```

    Ниже приведен пример импорта такого файла.
  
        ```
        .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
        ```
 
6. Файл журнала с именем, указанным с параметром `/j:`, создается при каждом запуске командной строки. Каждый подготовленный диск имеет файл журнала, который должен передаваться при создании задания импорта. Диски без файлов журналов обрабатываться не будут.

    > [!IMPORTANT]
    > - Не изменяйте данные на дисках или в файлах журналов после завершения подготовки этих дисков.

Дополнительные примеры см. в [примерах для файлов журнала](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>Шаг 2. Создание задания импорта 

Чтобы создать задание импорта на портале Azure, выполните следующие шаги.
1. Войдите в систему по адресу https://portal.azure.com/.
2. Выберите пункты **Все службы > Хранилище > Задания импорта и экспорта**. 

    ![Перейдите к импорту и экспорту](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Нажмите кнопку **Create Import/export Job** (Создать задание импорта или экспорта).

    ![Щелкните "Задание импорта и экспорта"](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. В разделе **Основные сведения**:

    - Выберите **Импорт в Azure**.
    - Введите описательное имя для задания импорта. Это имя будет использоваться для отслеживания выполняемых заданий, а также после их завершения.
        -  Это имя может содержать только буквы в нижнем регистре, цифры, дефисы и символы подчеркивания.
        -  Имя должно начинаться с буквы и не может содержать пробелы. 
    - Выберите подписку.
    - Выберите группу ресурсов. 

        ![Создание задания импорта — шаг 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. В разделе **Сведения о задании** сделайте следующее:
    
    - Отправьте файлы журнала, созданные на [шаге 1: подготовка дисков](#step-1-prepare-the-drives). 
    - Выберите учетную запись хранения, в которую будут импортированы данные. 
    - Расположение места назначения автоматически заполняется с учетом выбранного региона учетной записи хранения.
   
       ![Создание задания импорта — шаг 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. В разделе **Сведения о возврате** сделайте следующее:

    - В раскрывающемся списке выберите перевозчика.
    - Введите допустимый номер учетной записи, созданный в системе этого перевозчика. Корпорация Майкрософт использует эту учетную запись для отправки дисков обратно после завершения задания импорта. 
    - Укажите полное и допустимое имя контактного лица, телефон, адрес электронной почты, адрес, город, почтовый индекс, страну (провинцию) и страну (регион).

       ![Создание задание импорта – шаг 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

   
5. В разделе **Сводка** сделайте следующее:

    - Укажите адрес центра обработки данных Azure для доставки дисков обратно в Azure. Убедитесь, что в метке доставки упомянуто имя задания и полный адрес.
    - Нажмите кнопку **ОК** для завершения создания задания импорта.

        ![Создание задания импорта — шаг 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>Шаг 3. Отправка дисков в центр обработки данных Azure 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Шаг 4. Указание данных об отслеживании для задания

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="samples-for-journal-files"></a>Примеры файлов журнала

Чтобы **добавить дополнительные диски**, создайте файл с набором дисков и выполните приведенную ниже команду. 

Для последующих сеансов копирования на диски, указанные в файле *InitialDriveset.csv*, создайте еще один *CSV-файл* с набором дисков и укажите его в качестве значения параметра `AdditionalDriveSet`. Используйте **то же имя файла журнала** и укажите **новый идентификатор сеанса**. Формат CSV-файлов AdditionalDriveset и InitialDriveSet совпадает.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

Ниже приведен пример импорта такого файла.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Чтобы добавить дополнительные данные в тот же файл с набором дисков, используйте команду PrepImport для последующих сеансов копирования и скопируйте дополнительные файлы или папки.

Для последующих сеансов копирования на те же жесткие диски, указанные в файле *InitialDriveset.csv*, укажите имя **того же файла журнала** и **новый идентификатор сеанса**. Указывать ключ учетной записи хранения не требуется.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

Ниже приведен пример импорта такого файла.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Дополнительная информация

* [Просмотр состояния задания и диска](storage-import-export-view-drive-status.md)
* [Сведения о требованиях службы "Импорт и экспорт"](storage-import-export-requirements.md)


