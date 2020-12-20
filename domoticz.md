# Установка Domoticz

В репозитории OpenWrt 19.07 находится достаточно старая версия domoticz.
Чтобы установить свежую версию, воспользуйтесь готовым скриптом, который
нужно будет запустить в консоли

```sh
wget https://github.com/openlumi/xiaomi-gateway-openwrt/raw/master/files/domoticz.sh -O - | sh
````
Он установит все необходимые библиотеки, domoticz и плагин для работы с zigbee.

# Прошивка Zigate и подключение Zigate plugin к Domoticz

Для модуля jn5169, установленного на нашем шлюзе используется прошивка ZiGate.
Чтобы прошить её в модуль, воспользуетесь командой

```sh
wget https://github.com/openlumi/ZiGate/releases/download/snapshot-20201201/ZigbeeNodeControlBridge_JN5169_FULL_FUNC_DEVICE_31e_115200.bin -O /tmp/zigate.bin 
jnflash /tmp/zigate.bin
jntool erase_pdm
```
## Настройка плагина

Всё что нужно, это в разделе Setup > Hardware добавить элемент с типом
`Zigate plugin`, вписать имя в поле Name и нажать `Add`

![Добавление Zigate plugin](images/zigate_plugin.png)

Для настройки использовать

    Zigate Model: DIN
    Serial Port: /dev/ttymxc1


спустя несколько минут плагин проинициализируется и запустит панель управления
на порту 9440.


## Панель управления Zigate

![Панель управления](images/zigate_dashboard.png)

В данной панели вы можете включить режим сопряжения с дочерними устройствами и 
изменять настройки. 
Чтобы включить режим добавления устройств, включите переключатель 
`Accept new hardware` в верхнем правом углу.


Более подробно о панели вы можете прочитать на странице плагина
[https://github.com/pipiche38/Domoticz-Zigate-Wiki/blob/master/en-eng/WebUserInterfaceNavigation.md](https://github.com/pipiche38/Domoticz-Zigate-Wiki/blob/master/en-eng/WebUserInterfaceNavigation.md)

После добавления дочернего zigbee устройства оно появится в интерфейсе domoticz.
