---
title: Azure AD SSPR с компонентом обратной записи паролей | Документация Майкрософт
description: Использование Azure AD и Azure AD Connect для обратной записи паролей в локальный каталог.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: fce92845591f20f7f2e9cff1a856e0904629255b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054800"
---
# <a name="password-writeback-overview"></a>Обзор обратной записи паролей

Компонент обратной записи паролей позволяет настроить в Azure Active Directory (Azure AD) запись паролей в локальный каталог Active Directory. Это устраняет необходимость в настройке и управлении комплексным локальным решением для самостоятельного сброса пароля (SSPR), а также предоставляет пользователям удобный облачный способ сброса локальных паролей из любого расположения. Компонент обратной записи паролей  — это компонент [Azure Active Directory Connect](./../connect/active-directory-aadconnect.md), который могут включить и использовать текущие подписчики [Azure Active Directory](../fundamentals/active-directory-whatis.md) Premium.

Компонент обратной записи паролей предоставляет следующие возможности:

* **Обратная связь с нулевой задержкой.** Обратная запись паролей выполняется в синхронном режиме. Пользователи немедленно получают уведомление, если их пароль не соответствует политике или если его не удалось сбросить или изменить по какой-либо причине.
* **Поддержка сброса пароля для пользователей, использующих службы федерации Active Directory (AD FS) или другие технологии федерации.** Благодаря обратной записи паролей, при условии, что федеративные учетные записи пользователей синхронизируются в клиенте Azure AD, они смогут управлять локальными паролями Active Directory из облака.
* **Поддержка сброса пароля для пользователей, применяющих** [синхронизацию хэшей паролей](./../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). Если служба сброса пароля обнаруживает, что для синхронизированной учетной записи пользователя включена синхронизация хэшей паролей, то локальный и облачный пароли для этой учетной записи сбрасываются одновременно. Обратная запись паролей не зависит от синхронизация хэшей паролей.
* **Поддержка сброса пароля для пользователей, использующих сквозную аутентификацию**. Благодаря обратной записи паролей, при условии, что учетные записи с поддержкой сквозной аутентификации синхронизируются с клиентом Azure AD, они смогут управлять локальными паролями Active Directory из облака.
* **Поддержка изменения пароля с помощью панели доступа и Office 365**. При изменении просроченных или непросроченных паролей федеративными пользователями или пользователями с синхронизацией паролей эти пароли будут записываться обратно в локальную среду Active Directory.
* **Поддержка обратной записи паролей при их сбросе администратором на портале Azure.** Каждый раз, когда администратор сбрасывает пароль пользователя на [портале Azure](https://portal.azure.com), если этот пользователь является федеративным или для него включена синхронизация паролей, выбранный администратором пароль также будет устанавливаться в локальной службе Active Directory. В настоящее время эта функциональная возможность не поддерживается на портале администрирования Office.
* **Принудительное применение политик паролей локальной службы Active Directory**. Когда пользователь сбрасывает свой пароль, перед его фиксацией в соответствующем каталоге выполняется проверка соблюдения политики локальной службы Active Directory. Проверяется журнал, сложность, срок пользования, фильтры паролей и любые другие ограничения паролей, заданные в локальной службе Active Directory.
* **Не требуются какие-либо правила брандмауэра для входящего трафика**. Компонент обратной записи паролей использует ретранслятор служебной шины Azure в качестве базового коммуникационного канала. Чтобы эта функция работала, не требуется открывать какие-либо входящие порты в брандмауэре.
* **Отсутствие поддержки учетных записей пользователей, существующих в защищенных группах в локальной службе Active Directory**. Дополнительные сведения о защищенных группах см. в статье [Приложение В. Защищенные учетные записи и группы в Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Как работает обратная запись паролей

При сбросе или смене пароля в облаке федеративным пользователем или пользователем с синхронизацией хэшей происходит следующее:

1. Проверяется установленный у пользователя тип пароля. Если определено, что пароль управляется локально, выполняется следующее.
   * Проверяется, работает ли служба обратной записи. Если она работает, мы пользователю разрешается продолжить.
   * Если служба обратной записи не включена, пользователю сообщается о том, что сейчас невозможно сбросить пароль.
2. Затем пользователь проходит соответствующие этапы аутентификации и переходит на страницу **Сброс пароля**.
3. Пользователь выбирает новый пароль и подтверждает его.
4. После нажатия пользователем кнопки **Отправить** указанный в виде простого текста пароль шифруется с помощью симметричного ключа, который был создан в процессе установки компонента обратной записи.
5. После шифрования пароля он включается в полезные данные, которые передаются по каналу HTTPS на ретранслятор служебной шины для определенного клиента (который был также настроен в процессе установки обратной записи). Этот ретранслятор защищается случайно генерируемым паролем, который известен только локальной установленной системе.
6. После того как сообщение достигает служебной шины, конечная точка сброса пароля автоматически активируется и обнаруживает ожидающий запрос на сброс.
7. Затем служба выполняет поиск пользователя с помощью атрибута привязки облака. Для успешного поиска:

    * Объект пользователя должен существовать в пространстве соединителя Active Directory.
    * Объект пользователя должен быть связан с соответствующим объектом метавселенной (MV).
    * Объект пользователя должен быть связан с соответствующим объектом соединителя Azure Active Directory.
    * В ссылке из объекта соединителя Active Directory на MV должно быть правило синхронизации `Microsoft.InfromADUserAccountEnabled.xxx`. <br> <br>
    Когда поступает вызов из облака, модуль синхронизации использует атрибут **cloudAnchor**, чтобы найти объект пространства соединителя Azure Active Directory. Посл этого он переходит по ссылке к объекту MV, а затем — к объекту Active Directory. Так как для одного и того же пользователя может существовать несколько объектов Active Directory (несколько лесов), обработчик синхронизации полагается на ссылку `Microsoft.InfromADUserAccountEnabled.xxx` для выбора нужного из них.

    > [!Note]
    > В результате этой логики Azure AD Connect должен иметь возможность связаться с эмулятором основного контроллера домена, чтобы компонент обратной записи паролей работал. Если необходимо включить эту функцию вручную, можно подключить Azure AD Connect к эмулятору основного контроллера домена. Щелкните правой кнопкой мыши **Свойства** для соединителя синхронизации Active Directory, затем выберите **configure directory partitions** (Настройка разделов каталога). Здесь найдите раздел **domain controller connection settings** (Параметры подключения контроллера домена) и установите флажок **only use preferred domain controllers** (Использовать только предпочитаемые контроллеры домена). Если предпочитаемый контроллер домена не является эмулятором основного контроллера домена, Azure AD Connect все равно попытается подключиться к основному контроллеру домена для обратной записи паролей.

8. После нахождения учетной записи пользователя предпринимается попытка сброса пароля непосредственно в соответствующем лесу Active Directory.
9. Если операция установки пароля прошла успешно, пользователь уведомляется о том, что его пароль изменен.
    > [!NOTE]
    > Если пароль пользователя синхронизирован с Azure AD с помощью функции синхронизации паролей, то существует вероятность, что локальная политика паролей не такая строгая, как облачная. В этом случае мы по-прежнему принудительно применяем локальную политику, используя синхронизацию хэша паролей. Благодаря этому ваша локальная политика применяется в облаке независимо от того, обеспечиваете ли вы единый вход с помощью синхронизации паролей или федерации.

10. В случае сбоя операции установки пароля пользователю возвращается сообщение об ошибке и предлагается повторить попытку. Сбой может произойти по следующим причинам:
    * Служба была отключена.
    * Выбранный пароль не соответствует политикам организации.
    * Возможно, не удалось найти пользователя в локальном каталоге Active Directory.

    Для многих из таких случаев имеется конкретное сообщение, позволяющее указать пользователю возможности для устранения проблемы.

## <a name="configure-password-writeback"></a>Настройка компонента обратной записи паролей

Мы советуем использовать функцию автоматического обновления [Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md), если вы хотите использовать компонент обратной записи паролей.

Включение компонента обратной записи паролей с помощью DirSync и Azure AD Sync больше не поддерживается. Дополнительные сведения о переходе см. в разделе [Обновление Windows Azure Active Directory Sync и Azure Active Directory Sync](../connect/active-directory-aadconnect-dirsync-deprecated.md).

В инструкциях ниже предполагается, что вы уже настроили Azure AD Connect в своей среде с помощью параметров [Экспресс](./../connect/active-directory-aadconnect-get-started-express.md) или [Пользовательский](./../connect/active-directory-aadconnect-get-started-custom.md).

1. Чтобы настроить и включить компонент обратной записи паролей, войдите на сервер Azure AD Connect и запустите мастер настройки **Azure AD Connect**.
2. На странице **Приветствие** нажмите кнопку **Настроить**.
3. Выберите **Настроить параметры синхронизации** на странице **Дополнительные задачи** и нажмите кнопку **Далее**.
4. На странице **Подключение к Azure AD** введите учетные данные глобального администратора и нажмите кнопку **Далее**.
5. На страницах фильтров **Подключить каталоги** и **Домен или подразделение** нажмите кнопку **Далее**.
6. На странице **Дополнительные компоненты** установите флажок рядом с параметром **Обратная запись паролей** и нажмите кнопку **Далее**.
   ![Включение обратной записи паролей в Azure AD Connect][Writeback]
7. На странице **Готово к настройке** щелкните **Настроить** и дождитесь завершения процесса.
8. После завершения настройки нажмите кнопку **Выход**.

Действия по устранению неполадок, связанных с компонентом обратной записи паролей, описаны в разделе [Устранение неполадок с обратной записью паролей](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) соответствующей статьи.

## <a name="active-directory-permissions"></a>Разрешения Active Directory

Для учетной записи, указанной в служебной программе Azure AD Connect, должны быть заданы следующие элементы, чтобы можно было использовать SSPR:

* **Сброс пароля** 
* **Изменить пароль** 
* **разрешения на запись** для `lockoutTime`;  
* **разрешения на запись** для `pwdLastSet`;
* **расширенные права** для любого из перечисленных ниже объектов:
   * корневой объект *каждого домена* в этом лесу;
   * подразделения пользователя, которые должны находиться в области действия SSPR.

Если вы не уверены, к какой учетной записи относятся описанные выше учетные данные, откройте пользовательский интерфейс настройки Azure Active Directory Connect и щелкните **Просмотр текущей конфигурации**. Учетная запись, для которой необходимо добавить разрешение, указана в разделе **Синхронизированные каталоги**.

Установка этих разрешений позволит учетной записи службы MA для каждого леса управлять паролями от имени учетных записей пользователей в этом лесу. 

> [!IMPORTANT]
> Если не назначить эти разрешения, то даже при внешне правильной настройке обратной записи пользователи столкнутся с ошибками при попытке управления своими локальными паролями из облака.
>

> [!NOTE]
> Репликация этих разрешений по всем объектам вашего каталога может занять час или больше.
>

Чтобы задать соответствующие разрешения для компонента обратной записи паролей, сделайте следующее.

1. Откройте оснастку "Пользователи и компьютеры Active Directory" с учетной записью с правами администратора соответствующего домена.
2. В меню **Вид** включите параметр **Расширенные возможности**.
3. На панели слева щелкните правой кнопкой мыши объект, который представляет корень домена, и выберите **Свойства** > **Безопасность** > **Дополнительно**.
4. На вкладке **Разрешения** нажмите кнопку **Добавить**.
5. Выберите учетную запись, к которой необходимо применить разрешения (из настроек Azure AD Connect).
6. Из раскрывающегося списка **Применяется к** выберите **Descendent User objects** (Дочерние объекты-пользователи).
7. В разделе **Разрешения** установите флажки для следующих разрешений:
    * **Сброс пароля**
    * **Изменить пароль**
    * **Write lockoutTime** (Запись времени блокировки);
    * **Write pwdLastSet** (Запись времени последней установки пароля).
8. Щелкните **Применить** и "ОК", чтобы применить изменения, а затем закройте все открытые диалоговые окна.

## <a name="licensing-requirements-for-password-writeback"></a>Требования к лицензированию для обратной записи паролей

Сведения о лицензировании см. в разделе [Лицензии, необходимые для компонента обратной записи паролей](concept-sspr-licensing.md) или на следующих сайтах:

* [Сайт с ценами на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security).
* [Microsoft 365 корпоративный](https://www.microsoft.com/secure-productive-enterprise/default.aspx).

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>Локальные режимы аутентификации, поддерживаемые компонентом обратной записи паролей

Компонент обратной записи паролей поддерживает следующие типы паролей пользователей:

* **Пользователи облака.** В этом случае обратная запись паролей не применяется, так как локальный пароль отсутствует.
* **Пользователи с синхронизацией паролей.** Компонент обратной записи паролей поддерживается.
* **Федеративные пользователи.** Компонент обратной записи паролей поддерживается.
* **Пользователи со сквозной аутентификацией.** Компонент обратной записи паролей поддерживается.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Операции администрирования и операции пользователя, поддерживаемые компонентом обратной записи паролей

Обратная запись паролей осуществляется во всех следующих ситуациях:

* **Поддерживаемые операции пользователей:**
  * все самостоятельные добровольные операции изменения пароля конечного пользователя;
  * все самостоятельные принудительные операции изменения пароля пользователя (например, из-за окончания срока действия пароля);
  * все самостоятельные операции сброса пароля пользователя, выполняемые на [портале сброса паролей](https://passwordreset.microsoftonline.com).
* **Поддерживаемые операции администрирования:**
  * все самостоятельные добровольные операции изменения пароля администратора;
  * все самостоятельные принудительные операции изменения пароля администратора (например, из-за окончания срока действия пароля);
  * все самостоятельные операции сброса пароля администратора, выполняемые на [портале сброса пароля](https://passwordreset.microsoftonline.com);
  * все операции сброса пароля конечного пользователя, инициируемые администратором на [портале Azure](https://portal.azure.com).

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Операции администрирования и операции пользователя, не поддерживаемые компонентом обратной записи паролей

Обратная запись паролей *не осуществляется* во всех следующих ситуациях:

* **Неподдерживаемые операции пользователей:**
  * Все операции сброса пароля пользователя с помощью PowerShell версии 1, PowerShell версии 2 или API Graph Azure AD.
* **Неподдерживаемые операции администрирования:**
  * все операции сброса пароля конечного пользователя, инициируемые администратором на [портале управления Azure Office](https://portal.office.com);
  * все операции сброса пароля пользователя, инициируемые администратором с помощью PowerShell версии 1, PowerShell версии 2 или API Graph Azure AD.

Мы работаем над устранением этих ограничений, но точное время их снятия пока указать не можем.

## <a name="password-writeback-security-model"></a>Модель безопасности обратной записи паролей

Обратная запись паролей — это служба с высоким уровнем безопасности. Для защиты информации мы реализуем четырехуровневую модель безопасности, описанную ниже.

* **Ретранслятор служебной шины для определенного клиента**
  * При настройке службы настраивается ретранслятор служебной шины для определенного клиента, который защищается случайно генерируемым надежным паролем. У корпорации Майкрософт нет доступа к этому паролю.
* **Заблокированный криптографически надежный ключ шифрования пароля**
  * После создания ретранслятора служебной шины создается надежный симметричный ключ, который используется для шифрования пароля при передаче по сети. Этот ключ существует только в секретном хранилище компании в облаке, которое тщательно блокируется и контролируется, подобно любому паролю в каталоге.
* **Отраслевой стандартный протокол TLS**
  1. При выполнении сброса или изменения пароля в облаке мы берем пароль в виде простого текста и шифруем его, используя ваш открытый ключ.
  2. Затем он вставляется в сообщение HTTPS, которое передается по зашифрованному каналу с использованием SSL-сертификатов корпорации Майкрософт на ваш ретранслятор служебной шины.
  3. После поступления сообщения в служебную шину ваш локальный агент активируется и проходит аутентификацию в служебной шине с использованием ранее созданного надежного пароля.
  4. Локальный агент извлекает зашифрованное сообщение и расшифровывает его с помощью созданного закрытого ключа.
  5. Затем локальный агент пытается установить пароль через интерфейс AD DS SetPassword API. Именно этот этап позволяет принудительно применить политику паролей локальной службы Active Directory (например, сложность, срок пользования, историю, фильтры и т. д.) в облаке.
* **Политики срока действия сообщений** 
  * Если по какой-либо причине сообщение задержится в служебной шине из-за неработающей локальной службы, срок его ожидания истечет и оно будет удалено через несколько минут. Время ожидания и удаление сообщения повышают безопасность.

### <a name="password-writeback-encryption-details"></a>Сведения о шифровании компонента обратной записи паролей

После того, как пользователь отправит пароль, запрос на сброс пройдет несколько этапов шифрования, прежде чем поступит в вашу локальную среду. Эти этапы шифрования обеспечивают максимальную надежность и безопасность службы. Они описаны ниже.

* **Этап 1. Шифрование пароля с использованием 2048-разрядного ключа RSA.** Когда пользователь отправляет пароль для обратной записи в локальную среду, сначала этот пароль шифруется с использованием 2048-разрядного ключа RSA.
* **Этап 2. Шифрование на уровне пакета с использованием алгоритма AES-GCM.** Весь пакет (пароль и необходимые метаданные) шифруется с использованием алгоритма AES-GCM. Из-за этого пользователи с прямым доступом к базовому каналу служебной шины не могут просматривать или изменять содержимое.
* **Этап 3. Взаимодействие через канал SSL/TLS.** Весь обмен данными со служебной шиной осуществляется через канал SSL/TLS. Это позволяет защитить содержимое от несанкционированного стороннего доступа.
* **Автоматическая смена ключей каждые шесть месяцев**. Каждые шесть месяцев или при каждом отключении и последующем включении компонента обратной записи паролей в Azure AD Connect. Все ключи сменяются автоматически, чтобы обеспечить максимальную безопасность службы.

### <a name="password-writeback-bandwidth-usage"></a>Использование пропускной способности компонента обратной записи паролей

Компонент обратной записи паролей — это служба с низкой пропускной способностью, которая отправляет запросы в локальный агент только при следующих условиях:

* Два сообщения отправляются при включении или отключении компонента с помощью Azure AD Connect.
* Одно сообщение отправляется каждые 5 минут как сигнал пульса службы, если служба запущена.
* Два сообщения отправляются при каждой отправке нового пароля.
    * Первое сообщение является запросом на выполнение операции.
    * Второе сообщение содержит результат этой операции и отправляется в следующих случаях:
        * Во время каждой отправки нового пароля при самостоятельном сбросе пароля пользователем.
        * Во время каждой отправки нового пароля в ходе операции по смене пароля пользователя.
        * Во время каждой отправки нового пароля при сбросе пароля, инициированном администратором (только на портале администрирования Azure).

#### <a name="message-size-and-bandwidth-considerations"></a>Рекомендации по размеру сообщений и пропускной способности

Размер каждого сообщения, описанного выше, как правило, не превышает 1 КБ. Даже при экстремальных нагрузках служба обратной записи паролей будет потреблять несколько килобитов в секунду. Так как каждое сообщение отправляется в режиме реального времени, только если этого требует операция обновления пароля, и размер сообщения невелик, потребляемая компонентом обратной записи пропускная способность будет слишком незначительна, чтобы оказывать заметное влияние.

## <a name="next-steps"></a>Дополнительная информация

* [Как развернуть самостоятельный сброс пароля?](howto-sspr-deployment.md)
* [Сброс или изменение пароля](../user-help/active-directory-passwords-update-your-own-password.md)
* [Регистрация для самостоятельного сброса пароля](../user-help/active-directory-passwords-reset-register.md)
* [Требования к лицензированию самостоятельного сброса пароля в Azure AD](concept-sspr-licensing.md)
* [Какие данные используются для SSPR и какие сведения нужно указывать для пользователей](howto-sspr-authenticationdata.md)
* [Доступные пользователям методы проверки подлинности](concept-sspr-howitworks.md#authentication-methods)
* [Параметры политики для SSPR](concept-sspr-policy.md)
* [Как сообщать о действиях в SSPR](howto-sspr-reporting.md)
* [Обзор всех параметров SSPR и их значение](concept-sspr-howitworks.md)
* [Как устранить неполадки самостоятельного сброса пароля](active-directory-passwords-troubleshoot.md)
* [Вопросы, не вошедшие в другие статьи](active-directory-passwords-faq.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Включение обратной записи паролей в Azure AD Connect"
