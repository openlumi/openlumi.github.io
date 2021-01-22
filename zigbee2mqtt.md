# Flashing ZiGate and configuring Zigbee2mqtt

For the jn5169 chip, which is installed on our gateway, the Zigate firmware is used.
You need to flash it and reset it to default settings.

```shell
wget https://github.com/openlumi/ZiGate/releases/download/snapshot-20201201/ZigbeeNodeControlBridge_JN5169_FULL_FUNC_DEVICE_31e_115200.bin -O /tmp/zigate.bin 
jnflash /tmp/zigate.bin
jntool erase_pdm
```

Add the repository with packages for openlumi project

```shell
[ -f /lib/libustream-ssl.so ] && echo "libustream already installed" || opkg install libustream-mbedtls
wget https://openlumi.github.io/openwrt-packages/public.key
opkg-key add public.key
echo 'src/gz openlumi https://openlumi.github.io/openwrt-packages/packages/19.07/arm_cortex-a9_neon' >> /etc/opkg/customfeeds.conf
```

Then you should install the required libraries and zigbee2mqtt itself.

```shell
opkg update
opkg install mosquitto node node-zigbee2mqtt
sed -i 's/port: 8080/port: 8090/' /etc/zigbee2mqtt/configuration.yaml
sed -i 's/baudrate: 1000000/baudrate: 115200/' /etc/zigbee2mqtt/configuration.yaml
sed -i 's/homeassistant: false/homeassistant: true/' /etc/zigbee2mqtt/configuration.yaml
/etc/init.d/zigbee2mqtt restart
```

Now the zigbee2mqtt web interface will work on http://*GATEWAY-IP*:8090/.
It will send zigbee events to the local MQTT server.
If you need to configure the connection to the external MQTT broker, edit 
the configuration file `/etc/zigbee2mqtt/configuration.yaml` and restart
the `zigbee2mqtt` service

```shell
/etc/init.d/zigbee2mqtt restart
```
