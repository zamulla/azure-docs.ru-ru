## <a name="prerequisites"></a>предварительным требованиям

Для работы с этим руководством требуется активная подписка Azure.

> [!NOTE]
> Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в статье о [бесплатной пробной версии Azure][lnk-free-trial].

### <a name="required-software"></a>Необходимое программное обеспечение

На настольном компьютере необходимо установить клиент SSH, чтобы иметь удаленный доступ к командной строке Raspberry Pi.

- Windows не предоставляет клиент SSH. Мы советуем использовать [PuTTY](http://www.putty.org/).
- Большинство дистрибутивов Linux и Mac OS содержат служебную программу командной строки SSH. Дополнительные сведения см. в статье [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md) (Подключение по протоколу SSH с помощью Linux или Mac OS).

### <a name="required-hardware"></a>Необходимое оборудование

Настольный компьютер, чтобы иметь удаленный доступ к командной строке Raspberry Pi.

[Начальный набор Microsoft IoT для Raspberry Pi 3][lnk-starter-kits] или эквивалентные компоненты. В этом руководстве используются следующие компоненты из набора:

- Raspberry Pi 3;
- карта MicroSD (с NOOBS);
- кабель MiniUSB;
- кабель Ethernet;
- датчик BME280;
- монтажная плата;
- оптоволоконные кабеля с разъемами на обоих концах;
- резисторы;
- светодиодные индикаторы.

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/