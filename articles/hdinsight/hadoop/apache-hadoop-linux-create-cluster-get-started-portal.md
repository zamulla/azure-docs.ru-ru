---
title: Краткое руководство по началу работы с Hadoop и Hive в Azure HDInsight с помощью портала Azure | Документы Майкрософт
description: Узнайте, как создать кластеры HDInsight на портале Azure и запросить данные с помощью Hive.
keywords: начало работы с hadoop, hadoop под управлением linux, краткое руководство по hadoop, начало работы с hive, краткое руководство по hive
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc
ms.devlang: na
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: nitinme
ms.openlocfilehash: dd803fed4bc6a25904b2ea00a5b21af606ad7153
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097902"
---
# <a name="quickstart-get-started-with-hadoop-and-hive-in-azure-hdinsight-using-the-azure-portal"></a>Краткое руководство по началу работы с Hadoop и Hive в Azure HDInsight с помощью портала Azure

В этой статье вы узнаете, как создать кластеры [Hadoop](http://hadoop.apache.org/) в HDInsight с помощью портала Azure, а затем запустить задания Hive в HDInsight. Большинство заданий Hadoop — пакетные. Вы создаете кластер, выполняете несколько заданий, а затем удаляете его. В этой статье будут выполнены все три задачи.

В этом кратком руководстве для создания кластера Hadoop в HDInsight используется портал Azure. Создать кластер можно также с помощью [шаблона Azure Resource Manager](apache-hadoop-linux-tutorial-get-started.md).

В настоящее время в HDInsight доступно [семь типов кластеров](./apache-hadoop-introduction.md#cluster-types-in-hdinsight). Каждый тип кластера поддерживает свой набор компонентов. Все типы кластеров поддерживают инфраструктуру Hive. Дополнительные сведения о поддерживаемых компонентах в HDInsight см. в статье [Что представляют собой различные компоненты Hadoop, доступные в HDInsight?](../hdinsight-component-versioning.md)  

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="create-a-hadoop-cluster"></a>Создание кластера Hadoop

В этом разделе вы создадите кластер Hadoop в HDInsight, используя портал Azure. 

1. Войдите на [портал Azure](https://portal.azure.com).

1. На портале Azure выберите **Создать ресурс** > **Данные и аналитика** > **HDInsight**. 

    ![Databricks на портале Azure](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-hdinsight-on-portal.png "Databricks on Azure portal")

2. Выбрав **HDInsight** > **Быстрое создание** > **Основы**, укажите значения, как предложено на следующем снимке экрана.

    ![Ввод базовых значений для начала работы с кластером HDInsight на платформе Linux](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-provide-basic-values.png "Ввод базовых значений для создания кластера HDInsight")

    Введите или выберите следующие значения:
    
    |Свойство  |Описание  |
    |---------|---------|
    |**Имя кластера**     | Введите имя кластера Hadoop. Так как все кластеры в HDInsight используют одно пространство имен DNS, это имя должно быть уникальным. Имя может содержать до 59 символов, включая буквы, цифры и дефисы. Первый и последний знаки в имени не могут быть дефисами. |
    |**Подписка**     |  Выберите подписку Azure. |
    |**Тип кластера**     | Пока что пропустите этот параметр. Вы укажите его на следующем шаге процедуры.|
    |**Cluster login username and password** (Имя для входа и пароль для кластера)     | Имя для входа по умолчанию — **admin**. Длина пароля должна составлять не менее 10 символов. Пароль должен содержать по меньшей мере одну цифру, одну прописную и одну строчную буквы, а также один специальный символ (кроме ' " ` \)). Ни в коем случае **не вводите** распространенные пароли, например Pass@word1.|
    |**Имя пользователя SSH** | Имя пользователя по умолчанию — **sshuser**.  Можно указать другое имя пользователя SSH. |
    | **Использовать тот же пароль в учетных данных кластера** | Установите этот флажок, чтобы использовать одинаковый пароль для пользователя SSH и имени для входа для кластера.|
    |**Группа ресурсов**     | Создайте новую группу ресурсов или выберите имеющуюся группу ресурсов.  Группа ресурсов — это контейнер компонентов Azure.  В этом случае группа ресурсов содержит кластер HDInsight и зависимую учетную запись службы хранилища Azure. |
    |**Местоположение.**     | Выберите расположение Azure для создания кластера.  Выберите ближайшее к себе расположение для повышения производительности. |
        
3. Выберите **тип кластера** и укажите значения параметров, как показано на снимке экрана ниже.

    ![Ввод базовых значений для начала работы с кластером HDInsight на платформе Linux](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-provide-cluster-type.png "Ввод базовых значений для создания кластера HDInsight")

    Выберите следующие значения:
    
    |Свойство  |Описание  |
    |---------|---------|
    |**Тип кластера**     | Выберите **Hadoop**. |
    |**Операционная система**     |  Выберите подписку Azure. |
    |**Версия**     | Выберите **Hadoop 2.7.3 (HDI 3.6)**.|

    Щелкните **Выбрать** и нажмите кнопку **Далее**.

4. На вкладке **Хранилище** укажите значения параметров, как показано на снимке экрана ниже.

    ![Ввод значений хранилища кластера для начала работы с кластером HDInsight на платформе Linux](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-select-storage.png "Ввод значений хранилища для создания кластера HDInsight")

    Выберите следующие значения:
    
    |Свойство  |Описание  |
    |---------|---------|
    |**Тип первичного хранилища**     | В рамках этой статьи выберите **службу хранилища Azure**, чтобы использовать Azure Storage Blob в качестве учетной записи хранения по умолчанию. Можно также указать Azure Data Lake Storage в качестве хранилища по умолчанию. |
    |**Метод выбора**     |  В рамках этой статьи выберите **Мои подписки**, чтобы использовать учетную запись хранения из своей подписки Azure. Чтобы использовать учетную запись хранения из другой подписки, выберите **Ключ доступа** и укажите ключ доступа этой учетной записи. |
    |**Создание учетной записи хранения**     | Укажите имя учетной записи хранения.|

    Примите значения по умолчанию для остальных параметров и нажмите кнопку **Далее**.

5. На вкладке **Сводка** проверьте все значения, выбранные ранее.

    ![Сводка конфигурации для начала работы с кластером HDInsight на платформе Linux](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-summary.png "HDInsight Linux get started cluster summary")
      
4. Нажмите кнопку **Создать**. Вы увидите новый элемент **Submitting deployment for HDInsight** (Отправляется развертывание для HDInsight) на панели мониторинга портала. Процесс создания кластера занимает около 20 минут.

    ![Группа ресурсов при начале работы с HDInsight под управлением Linux](./media/apache-hadoop-linux-create-cluster-get-started-portal/deployment-progress-tile.png "Группа ресурсов кластера Azure HDInsight")

4. После создания кластера на портале Azure отобразится страница с общими сведениями об этом кластере.
   
    ![Параметры кластера при начале работы с HDInsight под управлением Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png "Свойства кластера HDInsight")    
    
    У каждого кластера есть зависимость [учетной записи хранения Azure](../hdinsight-hadoop-use-blob-storage.md) или [учетной записи Azure Data Lake](../hdinsight-hadoop-use-data-lake-store.md). Она называется учетной записью хранения по умолчанию. Кластер HDInsight должен находиться в том же регионе Azure, что и его учетная запись хранения, используемая по умолчанию. Удаление кластеров не приведет к удалению учетной записи хранения.

    > [!NOTE]
    > Сведения о других способах создания кластеров, а также о свойствах, используемых в этом руководстве, см. в статье [Создание кластеров Hadoop под управлением Linux в HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).       
    > 
    >

## <a name="run-hive-queries"></a>Выполнение запросов Hive

[Apache Hive](hdinsight-use-hive.md) — это самый популярный компонент службы HDInsight. Существует множество способов выполнения заданий Hive в HDInsight. В этом руководстве используется представление Hive Ambari на портале. Другие способы отправки заданий Hive описаны в статье [Использование Hive в HDInsight](hdinsight-use-hive.md).

1. Чтобы открыть Ambari, на предыдущем экране выберите **Панель мониторинга кластера**.  Вы можете также перейти по ссылке **https://&lt;ClusterName>.azurehdinsight.net**, где &lt;ClusterName> — это кластер, созданный в предыдущем разделе.

    ![Панель мониторинга кластера для начала работы с кластером HDInsight на платформе Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux get started cluster dashboard")

2. Введите имя пользователя Hadoop и пароль, указанные при создании кластера. Имя пользователя по умолчанию — **admin**.

3. Откройте **представление Hive** , как показано на снимке экрана ниже:
   
    ![Выбор представлений Ambari](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "Меню средства просмотра Hive в HDInsight")

4. На вкладке **Запрос** вставьте в лист следующие инструкции HiveQL:
   
        SHOW TABLES;

    ![Представление Hive в HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "Редактор запросов представления Hive в HDInsight")
   
   > [!NOTE]
   > Для Hive требуется точка с запятой.       
   > 
   > 

5. Нажмите кнопку **Execute**(Выполнить). Вкладка **Результаты** отобразится под вкладкой **Запрос** с информацией о задании. 
   
    Когда запрос будет выполнен, на вкладке **Запрос** появятся результаты этой операции. Вы увидите одну таблицу с именем **hivesampletable**. Этот пример таблицы Hive входит в состав всех кластеров HDInsight.
   
    ![Представление Hive в HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "Редактор запросов представления Hive в HDInsight")

6. Повторите шаги 4 и 5 и выполните следующий запрос.
   
        SELECT * FROM hivesampletable;
   
7. Вы также можете сохранить результаты запроса. Нажмите кнопку меню справа и укажите, как это следует сделать: скачать результаты в качестве CSF-файла или сохранить их в учетной записи хранения, связанной с кластером.

    ![Save result of Hive query](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Сохранение результата запроса Hive")

Когда задание Hive будет завершено, вы сможете [экспортировать результаты в базу данных SQL Azure или базу данных SQL Server](apache-hadoop-use-sqoop-mac-linux.md) либо [визуализировать их с помощью Excel](apache-hadoop-connect-excel-power-query.md). Дополнительные сведения об использовании Hive в HDInsight см. в статье [Использование Hive и HiveQL с Hadoop в HDInsight для анализа примера файла Apache log4j](hdinsight-use-hive.md).

## <a name="troubleshoot"></a>Устранение неполадок

Если при создании кластеров HDInsight возникли проблемы, см. раздел [Создание кластеров](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="clean-up-resources"></a>Очистка ресурсов
После завершения работы с этим руководством кластер можно удалить. В случае с HDInsight ваши данные хранятся в службе хранилища Azure, что позволяет безопасно удалить неиспользуемый кластер. Плата за кластеры HDInsight взимается, даже когда они не используются. Поскольку стоимость кластера во много раз превышает стоимость хранилища, экономически целесообразно удалять неиспользуемые кластеры. 

> [!NOTE]
> Если вы *сразу же* перейдете к следующему руководству, чтобы узнать, как выполнять операции извлечения, преобразования и загрузки, то можете не прерывать работу кластера. Дело в том, что в этом руководстве вам придется повторно создать кластер. Но если вы не собираетесь немедленно приступать к изучению следующего руководства, то нужно удалить кластер.
> 
>  

**Удаление кластера и (или) учетной записи хранения по умолчанию**

1. Вернитесь на вкладку браузера, где открыт портал Azure. Откройте страницу обзора кластера. Если требуется удалить кластер и сохранить учетную запись хранения по умолчанию, щелкните **Удалить**.

    ![Удаление кластера HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "Удаление кластера HDInsight")

2. Если вам нужно удалить кластер и учетную запись хранения по умолчанию, выберите имя группы ресурсов (выделено на предыдущем снимке экрана), чтобы открыть страницу группы ресурсов.

3. Выберите **Удалить группу ресурсов**, чтобы удалить группу ресурсов, которая содержит кластер и учетную запись хранения по умолчанию. Обратите внимание, что удаление группы ресурсов приводит к удалению учетной записи хранения. Если вы хотите сохранить учетную запись хранения, удалите только кластер.

## <a name="next-steps"></a>Дальнейшие действия
В этом руководстве вы узнали, как с помощью шаблона Resource Manager создать кластер HDInsight под управлением Linux и как выполнять базовые запросы Hive. В следующей статье вы узнаете, как выполнять операции извлечения, преобразования и загрузки с помощью Hadoop в HDInsight.

> [!div class="nextstepaction"]
>[Извлечение, преобразование и загрузка данных с помощью Apache Hive в HDInsight](../hdinsight-analyze-flight-delay-data-linux.md)

Если вы готовы приступить к работе с собственными данными и хотите узнать больше о том, как HDInsight сохраняет данные или как получать данные в HDInsight, см. сведения в статьях ниже:

* Сведения о том, как HDInsight использует службу хранилища Azure, см. в статье [Использование HDFS-совместимой службы хранилища с Hadoop в HDInsight](../hdinsight-hadoop-use-blob-storage.md).
* Сведения о том, как создать кластер HDInsight с Data Lake Storage, см. в статье [Краткое руководство. Настройка кластеров в HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).
* Сведения об отправке данных в HDInsight см. в статье [Отправка данных для заданий Hadoop в HDInsight](../hdinsight-upload-data.md).

Дополнительные сведения об анализе данных с помощью HDInsight см. в следующих статьях.

* Дополнительные сведения об использовании Hive с HDInsight, включая выполнение запросов Hive из Visual Studio, см. в [этой статье](hdinsight-use-hive.md).
* Дополнительные сведения о Pig — языке, который используется для преобразования данных, — см. в статье [Использование Pig с Hadoop в HDInsight](hdinsight-use-pig.md).
* Дополнительные сведения о MapReduce — способе написания программ, которые обрабатывают данные в Hadoop, — см. в статье [Использование MapReduce в Hadoop в HDInsight](hdinsight-use-mapreduce.md).
* Дополнительные сведения об анализе данных в HDInsight с помощью средств HDInsight для Visual Studio см. в статье [Приступая к работе с инструментами Hadoop в Visual Studio для HDInsight для выполнения запроса Hive](apache-hadoop-visual-studio-tools-get-started.md).


Дополнительные сведения о создании кластера HDInsight и управлении этим кластером см. в следующих статьях:

* Сведения об управлении кластером HDInsight под управлением Linux см. в статье [Управление кластерами HDInsight с помощью веб-интерфейса Ambari](../hdinsight-hadoop-manage-ambari.md).
* Дополнительные сведения о параметрах, которые можно выбрать при создании кластера HDInsight, см. в статье [Создание кластеров Hadoop под управлением Linux в HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).


[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


