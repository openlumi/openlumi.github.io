# Установка Zesp32 на прошивку с openwrt

Zesp будет работать так же как и на стоковой прошивке, но ввиду отличия в системных
библиотеках, требуются дополнительные шаги

### 1. Установка nodejs и zesp32

Установите пакеты на шлюз

```shell script
wget https://github.com/devbis/xiaomi-gateway-openwrt/raw/master/files/node_v10.22.0-1_arm_cortex-a9_neon.ipk
wget https://github.com/devbis/xiaomi-gateway-openwrt/raw/master/files/node-npm_v10.22.0-1_arm_cortex-a9_neon.ipk
wget https://github.com/devbis/xiaomi-gateway-openwrt/raw/master/files/node-zesp32_20200928-1_arm_cortex-a9_neon.ipk
opkg install node_v10.22.0-1_arm_cortex-a9_neon.ipk
opkg install node-npm_v10.22.0-1_arm_cortex-a9_neon.ipk
opkg install node-zesp32_20200928-1_arm_cortex-a9_neon.ipk
```

### 2. Zigbee прошивка.

Если у вас уже прошита версия для zesp, можно пропустить этот шаг.

Остановите zesp32

```shell script
killall node
```

Чтобы прошить на openwrt введите команду

```shell script
sh /root/flash.sh /opt/zesp32/util/Zigbee.bin --erasepdm
```

Это обновит прошивку в модуле jn5169
Теперь можно запустить zesp32 заново

```shell script
/etc/init.d/zesp32 restart
```

Интерфейс zesp32 будет доступен на `http://*GATEWAY_IP*:8081/`
