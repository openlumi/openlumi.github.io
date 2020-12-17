# Прошивка Zigate и настройка Zigbee2mqtt

Для модуля jn5169, установленного на нашем шлюзе используется прошивка ZiGate.
Требуется прошить её и сбросить на начальные настройки

```shell script
wget https://github.com/openlumi/ZiGate/releases/download/snapshot-20201008/ZiGate_3.1_cd_fix_leak__JN5169_COORDINATOR_115200.bin -O /tmp/zigate.bin
jnflash /tmp/zigate.bin
jntool erase_pdm
```

Далее потребуется установить необходимые библиотеки и сам zigbee2mqtt

```shell
cd /tmp
wget https://openlumi.github.io/openwrt-packages/packages/arm_cortex-a9_neon/node/node_v12.19.0-1_arm_cortex-a9_neon.ipk
wget https://openlumi.github.io/openwrt-packages/packages/arm_cortex-a9_neon/node/node-zigbee2mqtt_1.16.1-1_arm_cortex-a9_neon.ipk
opkg update
opkg install mosquitto
opkg install /tmp/node_v12.19.0-1_arm_cortex-a9_neon.ipk
opkg install /tmp/node-zigbee2mqtt_1.16.1-1_arm_cortex-a9_neon.ipk
mkdir -p /etc/zigbee2mqtt/
wget https://raw.githubusercontent.com/openlumi/openwrt-node-packages/master/node-zigbee2mqtt/files/zigbee2mqtt.init -O /etc/init.d/zigbee2mqtt
chmod +x /etc/init.d/zigbee2mqtt
wget https://raw.githubusercontent.com/openlumi/openwrt-node-packages/master/node-zigbee2mqtt/files/configuration.yaml -O /etc/zigbee2mqtt/configuration.yaml
sed -i 's/baudrate: 1000000/baudrate: 115200/' /etc/zigbee2mqtt/configuration.yaml
sed -i 's/homeassistant: false/homeassistant: true/' /etc/zigbee2mqtt/configuration.yaml
/etc/init.d/zigbee2mqtt restart
```

Теперь по адресу http://*YOUR-IP*:8080/ будет работать веб-интерфейс zigbee2mqtt,
который будет отсылать данные в локальный MQTT сервер. 
Если вам требуется настроить подключение к внешнему MQTT серверу,
отредактируйте файл настроек `/etc/zigbee2mqtt/configuration.yaml` и перезапустите
сервис `zigbee2mqtt`

```shell
/etc/init.d/zigbee2mqtt restart
```
