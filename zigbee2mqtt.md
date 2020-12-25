# Прошивка Zigate и настройка Zigbee2mqtt

Для модуля jn5169, установленного на нашем шлюзе используется прошивка ZiGate.
Требуется прошить её и сбросить на начальные настройки

```shell script
wget https://github.com/openlumi/ZiGate/releases/download/snapshot-20201201/ZigbeeNodeControlBridge_JN5169_FULL_FUNC_DEVICE_31e_115200.bin -O /tmp/zigate.bin 
jnflash /tmp/zigate.bin
jntool erase_pdm
```

Добавьте репозиторий с пакетами для проекта openlumi

```shell
[ -f /lib/libustream-ssl.so ] && echo "libustream already installed" || opkg install libustream-mbedtls
wget https://openlumi.github.io/openwrt-packages/public.key
opkg-key add public.key
echo 'src/gz openlumi https://openlumi.github.io/openwrt-packages/packages/19.07/arm_cortex-a9_neon' >> /etc/opkg/customfeeds.conf
```

Далее потребуется установить необходимые библиотеки и сам zigbee2mqtt

```shell
opkg update
opkg install mosquitto node node-zigbee2mqt
sed -i 's/port: 8080/port: 8090/' /etc/zigbee2mqtt/configuration.yaml
sed -i 's/baudrate: 1000000/baudrate: 115200/' /etc/zigbee2mqtt/configuration.yaml
sed -i 's/homeassistant: false/homeassistant: true/' /etc/zigbee2mqtt/configuration.yaml
/etc/init.d/zigbee2mqtt restart
```

Теперь по адресу http://*YOUR-IP*:8090/ будет работать веб-интерфейс zigbee2mqtt,
который будет отсылать данные в локальный MQTT сервер. 
Если вам требуется настроить подключение к внешнему MQTT серверу,
отредактируйте файл настроек `/etc/zigbee2mqtt/configuration.yaml` и перезапустите
сервис `zigbee2mqtt`

```shell
/etc/init.d/zigbee2mqtt restart
```
