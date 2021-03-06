## <a name="configure-the-nodejs-simulated-device"></a>Настройка имитации устройства Node.js
1. На панели удаленного мониторинга щелкните **+ Добавить устройство**, а затем добавьте *пользовательское устройство*. Запишите имя узла Центра Интернета вещей, идентификатор устройства и ключ устройства. Они потребуются позже, при подготовке клиентского приложения устройства remote_monitoring.js.
2. Убедитесь, что на компьютере разработки установлен компонент Node.js 0.12.x или более поздняя версия. Выполните команду `node --version` в командной строке или оболочке, чтобы проверить версию. Сведения об использовании диспетчера пакетов для установки Node.js в Linux см. в разделе [Installing Node.js via package manage][node-linux] (Установка Node.js с помощью диспетчера пакетов).
3. После установки Node.js клонируйте последнюю версию репозитория [azure-iot-sdks-node][lnk-github-repo] на свой компьютер разработки. Всегда используйте ветвь **master** , чтобы получать последние версии библиотек и примеров.
4. Из папки node/device/samples в локальной копии репозитория [azure-iot-sdks-node][lnk-github-repo] скопируйте в пустую папку на компьютере разработки следующие два файла:
   
   * packages.json
   * remote_monitoring.js
5. Откройте файл remote_monitoring.js и найдите следующее определение переменной.
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Замените **[IoT Hub device connection string]** строкой подключения устройства. Используйте значения имени узла Центра Интернета вещей, идентификатора устройства и ключа устройства, которые вы записали на шаге 1. Строка подключения имеет следующий формат:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Если имя узла Центра Интернета вещей — **contoso**, а идентификатор устройства — **mydevice**, то строка подключения будет выглядеть как следующий фрагмент кода:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Сохраните файл. Выполните следующие команды в оболочке или командной строке в папке, содержащей эти файлы, для установки необходимых пакетов, а затем запустите пример приложения.
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Наблюдение динамической телеметрии в действии
На панели мониторинга отображаются данные телеметрии температуры и влажности из существующих имитаций устройств.

![Панель мониторинга по умолчанию][image1]

Если выбрать имитацию устройства Node.js, которая запускалась в предыдущем разделе, отобразятся данные телеметрии температуры, влажности и внешней температуры.

![Добавление значения внешней температуры на панель мониторинга][image2]

Решение для удаленного мониторинга автоматически определяет тип дополнительных данных телеметрии внешней температуры и добавляет их на диаграмму на панели мониторинга.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-v1-send-external-temperature/image1.png
[image2]: media/iot-suite-v1-send-external-temperature/image2.png