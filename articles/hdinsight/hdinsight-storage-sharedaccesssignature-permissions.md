---
title: Ограничение доступа с помощью подписанных URL-адресов в Azure HDInsight | Документация Майкрософт
description: Узнайте, как использовать подписанные URL-адреса для ограничения доступа HDInsight к данным, хранящимся в хранилищах BLOB-объектов.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: larryfr
ms.openlocfilehash: 328f052e1ccad3ac26cce62c858e050253192ae8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2018
ms.locfileid: "32179987"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>Использование подписанных URL-адресов хранилища Azure для ограничения доступа к данным в HDInsight

HDInsight имеет полный доступ к данным в учетных записях хранения Azure, связанных с кластером. Чтобы ограничить этот доступ, вы можете применить подписанные URL-адреса для контейнера больших двоичных объектов. Подписанные URL-адреса (SAS) — это функция учетных записей хранения Azure, позволяющая ограничивать доступ к данным, например предоставлять доступ к данным только для чтения.

> [!IMPORTANT]
> Чтобы создать решение на основе Apache Ranger, попробуйте использовать HDInsight, присоединенный к домену. Дополнительные сведения см. в статье [Настройка присоединенных к домену кластеров HDInsight (предварительная версия)](./domain-joined/apache-domain-joined-configure.md).

> [!WARNING]
> HDInsight следует предоставить полный доступ к хранилищу по умолчанию для кластера.

## <a name="requirements"></a>Требования

* Подписка Azure
* C# или Python. Пример кода на C# предоставлен в решении Visual Studio.

  * Требуется Visual Studio версии 2013, 2015 или 2017.
  * Требуется Python версии 2.7 или более новой.

* Кластер HDInsight под управлением Linux ИЛИ [Azure PowerShell][powershell]. Если у вас есть кластер под управлением Linux, можно добавить подписанный URL-адрес в кластер с помощью Ambari. Если нет, создайте кластер с помощью Azure PowerShell и добавьте подписанный URL-адрес в процессе создания кластера.

    > [!IMPORTANT]
    > Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе [Приближается дата прекращения сопровождения HDI версии 3.3](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Примеры файлов взяты из статьи на странице [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Этот репозиторий содержит указанные далее элементы.

  * Проект Visual Studio, способный создать контейнер хранилища, хранимую политику и SAS для использования с HDInsight.
  * Сценарий Python, способный создать контейнер хранилища, хранимую политику и SAS для использования с HDInsight.
  * Сценарий PowerShell, способный создать кластер HDInsight и настроить его для использования SAS.

## <a name="shared-access-signatures"></a>Подписи коллективного доступа

Существуют две формы подписанных URL-адресов:

* Нерегламентированные. Время начала, время окончания срока действия и разрешения для SAS указываются в универсальном коде ресурса (URI) SAS.

* Хранимая политика доступа. Хранимая политика доступа определяется в контейнере ресурсов, например в контейнере больших двоичных объектов. Она может использоваться для управления ограничениями одного или нескольких подписанных URL-адресов. При сопоставлении подписи общего доступа с хранимой политикой доступа эта подпись наследует ограничения (время начала, время окончания и разрешения), определенные для данной хранимой политики доступа.

Различие между этими двумя формами важно для одного ключевого сценария — отзыва. Подпись общего доступа — это URL-адрес, этой подписью может воспользоваться любой человек, который ее получил, независимо от того, кто изначально запрашивал такую подпись. Размещенный в свободном доступе подписанный URL-адрес сможет использовать любой человек. Распространяемая подпись общего доступа действительна до тех пор, пока не произойдет одно из четырех возможных событий:

1. Наступает время истечения срока действия, указанное для данной подписи общего доступа.

2. Истек срок действия, указанный для хранимой политики доступа, на которую ссылается SAS. Следующие сценарии приводят к достижению времени окончания срока действия:

    * Истек интервал времени.
    * Хранимая политика доступа изменилась, чтобы время окончания срока действия оказалось в прошлом. При изменении времени окончания срока действия отменяется подписанный URL-адрес.

3. Удаляется хранимая политика доступа, на которую ссылается подпись, что является другим способом отозвать подпись общего доступа. Если вы снова создадите хранимую политику доступа с таким же именем, к ней будут применены все маркеры SAS предыдущей политики (если не истек срок действия соответствующих SAS). Если вы намерены отозвать SAS, обязательно используйте другое имя при повторном создании политики доступа, срок действия которой еще не истек.

4. Повторно создается ключ учетной записи, который был использован для создания подписи общего доступа. Это приведет к тому, что все приложения, использующие прежний ключ, не смогут пройти аутентификацию. Предоставьте всем компонентам новый ключ.

> [!IMPORTANT]
> URI подписанного URL-адреса связан с ключом учетной записи, который использовался для создания подписи и соответствующей хранимой политики доступа (при наличии таковой). Если хранимая политика доступа не указана, то единственный способ отменить подписанный URL-адрес — изменить ключ учетной записи.

Рекомендуется всегда использовать хранимые политики доступа. При использовании хранимой политики можно отменить подписи или продлить дату окончания срока действия при необходимости. В инструкциях, приведенных в этом документе, для формирования подписанных URL-адресов используются хранимые политики доступа.

Дополнительные сведения о подписанных URL-адресах см. в статье [Общие сведения о модели SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Создание хранимой политики и SAS с использованием C\#

1. Откройте решение в Visual Studio.

2. В обозревателе решений щелкните проект **SASToken** правой кнопкой мыши и выберите пункт **Свойства**.

3. Выберите **Параметры** и добавьте значения для следующих записей:

   * StorageConnectionString: строка подключения к учетной записи хранения, для которой необходимо создать хранимую политику и SAS. Требуется формат: `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey`, где `myaccount` — это имя вашей учетной записи хранения, а `mykey` — ключ к учетной записи хранения.

   * ContainerName: контейнер в учетной записи хранения, доступ к которому необходимо ограничить.

   * SASPolicyName: имя, которое будет использоваться для создаваемой хранимой политики.

   * FileToUpload: путь к файлу для отправки в контейнер.

4. Запустите проект. После создания SAS отобразится похожая информация:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Сохраните маркер политики SAS, имя учетной записи хранения и имя контейнера. Эти значения используются для связи учетной записи хранения с кластером HDInsight.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Создание хранимой политики и SAS с использованием Python

1. Откройте файл SASToken.py и измените следующие значения:

   * policy\_name: имя, которое будет использоваться для создаваемой хранимой политики.

   * storage\_account\_name: имя вашей учетной записи хранения.

   * storage\_account\_key: ключ вашей учетной записи хранения.

   * storage\_container\_name: контейнер в учетной записи хранения, доступ к которому необходимо ограничить.

   * example\_file\_path: путь к файлу, который передается в контейнер.

2. Выполните скрипт. После выполнения сценария отобразится маркер SAS следующего вида:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Сохраните маркер политики SAS, имя учетной записи хранения и имя контейнера. Эти значения используются для связи учетной записи хранения с кластером HDInsight.

## <a name="use-the-sas-with-hdinsight"></a>Использование SAS с HDInsight

При создании кластера HDInsight необходимо указать основную учетную запись хранения; дополнительные учетные записи хранения указываются по желанию. Оба способа добавления хранилища требуют полного доступа к соответствующим учетным записям хранения и контейнерам.

Чтобы использовать подписанный URL-адрес для ограничения доступа к контейнеру, добавьте пользовательскую запись в файл конфигурации **core-site** для кластера.

* Для кластеров HDInsight **под управлением Windows** или **Linux** это можно сделать на этапе создания кластера с помощью PowerShell.
* При создании кластеров HDInsight **под управлением Linux** с помощью Ambari конфигурация корректируется позднее.

### <a name="create-a-cluster-that-uses-the-sas"></a>Создание кластера, использующего SAS

Пример создания кластера HDInsight, использующего SAS, включен в каталог `CreateCluster` репозитория. Чтобы воспользоваться этим примером, выполните указанные ниже действия.

1. Откройте файл `CreateCluster\HDInsightSAS.ps1` в текстовом редакторе и измените указанные ниже значения в начале документа.

    ```powershell
    # Replace 'mycluster' with the name of the cluster to be created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with the name of the group to be created
    $resourceGroupName = 'myresourcegroup'
    # Replace with the Azure data center you want to the cluster to live in
    $location = 'North Europe'
    # Replace with the name of the default storage account to be created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with the name of the SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with the name of the SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with the SAS token generated earlier
    $SASToken = 'sastoken'
    # Set the number of worker nodes in the cluster
    $clusterSizeInNodes = 3
    ```

    Например, измените `'mycluster'` в имени кластера, который требуется создать. Значения SAS должны совпадать со значениями, которые использовались при создании учетной записи хранения и маркера SAS.

    Изменив значения, сохраните файл.

2. Откройте командную строку Azure PowerShell. Если вы не знакомы с модулем Azure PowerShell или он у вас не установлен, см. статью [Установка и настройка служб Azure PowerShell][powershell].

1. В командной строке выполните следующую команду для проверки подлинности в подписке Azure:

    ```powershell
    Connect-AzureRmAccount
    ```

    При появлении запроса войдите в систему, используя учетную запись своей подписки Azure.

    Если ваша учетная запись связана с несколькими подписками Azure, выберите нужную подписку с помощью командлета `Select-AzureRmSubscription`.

4. В командной строке измените каталоги на каталог `CreateCluster` , содержащий файл HDInsightSAS.ps1. Затем выполните следующую команду для запуска сценария:

    ```powershell
    .\HDInsightSAS.ps1
    ```

    По мере выполнения сценария информация о создании группы ресурсов и учетных записей хранения появляется в командной строке PowerShell. Затем вам будет предложено указать пользователя HTTP для кластера HDInsight. Эта учетная запись используется для безопасного доступа к кластеру по HTTP/s.

    При создании кластера под управлением Linux запрашивается имя учетной записи пользователя SSH и пароль. Эта учетная запись используется для удаленного входа в кластер.

   > [!IMPORTANT]
   > При появлении запроса на имя и пароль пользователя HTTP/s или SSH необходимо указать пароль, который отвечает следующим условиям:
   >
   > * содержит не меньше 10 символов;
   > * содержит хотя бы одну цифру;
   > * содержит хотя бы один специальный символ;
   > * содержит хотя бы одну букву в верхнем или нижнем регистре.

Обычно выполнение сценария занимает около 15 минут. Если сценарий выполнен без ошибок, значит, кластер создан.

### <a name="use-the-sas-with-an-existing-cluster"></a>Использование SAS в существующем кластере

Если у вас уже есть кластер под управлением Linux, SAS можно добавить в конфигурацию файла **core-site** , выполнив описанные ниже действия.

1. Откройте веб-интерфейс Ambari для вашего кластера. Адрес этой страницы: https://YOURCLUSTERNAME.azurehdinsight.net. При появлении запроса пройдите проверку подлинности, указав имя пользователя и пароль администратора, которые использовались при создании кластера.

2. В левой части экрана пользовательского веб-интерфейса Ambari выберите **HDFS**, затем вкладку щелкните **Configs** (Конфигурации) посередине страницы.

3. Откройте вкладку **Advanced** (Дополнительно) и прокрутите экран до раздела **Custom core-site** (Настройка файла core-site).

4. Разверните раздел **Custom core-site** (Настройка файла core-site), прокрутите экран до конца и щелкните ссылку **Add property...** (Добавить свойство...). В полях **Key** (Ключ) и **Value** (Значение) укажите следующие значения.

   * **Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Value**: SAS, выданный выполненным ранее приложением C# или Python.

     Замените **CONTAINERNAME** на имя контейнера, используемого в приложении C# или SAS. Замените **STORAGEACCOUNTNAME** на имя используемой учетной записи хранения.

5. Нажмите кнопку **Add** (Добавить), чтобы сохранить этот ключ и значение, а затем кнопку **Save** (Сохранить), чтобы сохранить изменения в конфигурации. При появлении запроса добавьте описание внесенного изменения (например, "Добавление доступа к хранилищу SAS") и нажмите кнопку **Сохранить**.

    Закончив вносить изменения, нажмите кнопку **ОК** .

   > [!IMPORTANT]
   > Чтобы изменения вступили в силу, необходимо перезапустить несколько служб.

6. В пользовательском веб-интерфейсе Ambari выберите **HDFS** в списке слева, а затем щелкните **Restart All Affected** (Перезапустить все затронутые) в раскрывающемся списке **Service Actions** (Действия службы) справа. Когда появится запрос, выберите __Conform Restart All__ (Подтвердить перезапуск всех).

    Повторите эту процедуру для MapReduce2 и YARN.

7. Перезапустив службы, выберите одну из них и отключите режим обслуживания в раскрывающемся списке **Учетные записи обслуживания**.

## <a name="test-restricted-access"></a>Тестирование ограниченного доступа

Чтобы убедиться в том, что доступ ограничен, используйте следующие методы:

* Для подключения к кластерам HDInsight **под управлением Windows** используйте удаленный рабочий стол. Дополнительные сведения см. в статье [Подключение к HDInsight с использованием RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    Установив подключение, щелкните значок **командной строки Hadoop** на рабочем столе, чтобы открыть командную строку.

* Для подключения к кластерам HDInsight **под управлением Linux** используйте протокол SSH. Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Установив подключение к кластеру, выполните описанные ниже действия, чтобы убедиться в том, что элементы в учетной записи хранения SAS доступны только для чтения и включения в списки.

1. Чтобы вывести содержимое контейнера, выполните следующую команду в командной строке: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Замените **SASCONTAINER** именем контейнера, созданного для учетной записи хранения SAS. Замените **SASACCOUNTNAME** именем учетной записи хранения, используемой для SAS.

    Список включает файл, отправленный при создании контейнера и SAS.

2. Проверьте, можете ли вы прочитать содержимое файла, выполнив следующую команду: Замените **SASCONTAINER** и **SASACCOUNTNAME**, как на предыдущем шаге. Замените **FILENAME** именем файла, отображенным в предыдущей команде.

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Эта команда выводит содержимое файла.

3. Загрузите файл в локальную файловую систему, выполнив следующую команду:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Эта команда загружает на локальный компьютер файл с именем **testfile.txt**.

4. Передайте локальный файл в новый файл с именем **testupload.txt** в хранилище SAS, выполнив следующую команду:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Должно появиться сообщение следующего вида:

        put: java.io.IOException

    Эта ошибка возникает из-за того, что расположение хранилища доступно только для чтения и включения в списки. Разместите данные в хранилище по умолчанию доступного для записи кластера, выполнив следующую команду:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    На этот раз операция должна завершиться успешно.

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="a-task-was-canceled"></a>Задача была отменена

**Проблема**. При создании кластера с помощью сценария PowerShell может появиться следующее сообщение об ошибке:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Причина**. Эта ошибка может возникать при использовании пароля для администратора кластера, пользователя HTTP кластера либо (в случае кластеров под управлением Linux) пользователя SSH.

**Решение**. Используйте пароль, отвечающий следующим требованиям:

* содержит не меньше 10 символов;
* содержит хотя бы одну цифру;
* содержит хотя бы один специальный символ;
* содержит хотя бы одну букву в верхнем или нижнем регистре.

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда вы узнали, как добавить хранилище с ограниченным доступом в кластер HDInsight, ознакомьтесь с другими способами работы с данными в вашем кластере:

* [Использование Hive с HDInsight](hadoop/hdinsight-use-hive.md)
* [Использование Pig с HDInsight](hadoop/hdinsight-use-pig.md)
* [Использование MapReduce с HDInsight](hadoop/hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
