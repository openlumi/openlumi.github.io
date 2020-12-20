# Установка Zesp32 на прошивку с openwrt

Zesp будет работать так же как и на стоковой прошивке, но ввиду отличия в системных
библиотеках, требуются дополнительные шаги

### 1. Установка nodejs и zesp32

Установите пакеты на шлюз

```shell script
cd /tmp
wget https://openlumi.github.io/openwrt-packages/packages/arm_cortex-a9_neon/node/node_v12.19.0-1_arm_cortex-a9_neon.ipk
wget https://openlumi.github.io/openwrt-packages/packages/arm_cortex-a9_neon/node/node-npm_v12.19.0-1_arm_cortex-a9_neon.ipk
opkg update
opkg install /tmp/node_v12.19.0-1_arm_cortex-a9_neon.ipk
opkg install /tmp/node-npm_v12.19.0-1_arm_cortex-a9_neon.ipk
wget http://82.146.46.112/fw/ZESPowrt.tar.gz
tar -xzvf ZESPowrt.tar.gz -C /
wget https://raw.githubusercontent.com/openlumi/xiaomi-gateway-openwrt/master/files/zesp32.init -O /etc/init.d/zesp32
chmod +x /etc/init.d/zesp32
wget https://raw.githubusercontent.com/openlumi/xiaomi-gateway-openwrt/master/files/zesp32-package.json -O /opt/app/package.json
/etc/init.d/zesp32 enable
/etc/init.d/zesp32 start
```

### 2. Zigbee прошивка.

Если у вас уже прошита версия для zesp, можно пропустить этот шаг.

Остановите zesp32

```shell script
killall node
```

Чтобы прошить на openwrt введите команду

```shell script
jnflash /opt/app/util/Zigbee.bin
jntool erase_pdm
```

Это обновит прошивку в модуле jn5169
Теперь можно запустить zesp32 заново

```shell script
/etc/init.d/zesp32 restart
```

Интерфейс zesp32 будет доступен на `http://*GATEWAY_IP*:8081/`
