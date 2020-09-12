# Установка Zesp32 на прошивку с openwrt

Zesp будет работать так же как и на стоковой прошивке, но ввиду отличия в системных
библиотеках, требуются дополнительные шаги

### 1. Zigbee прошивка.

Если у вас уже прошита версия для zesp, можно пропустить этот шаг.
Чтобы прошить на openwrt введите команду

```shell script
sh /root/flash.sh /opt/app/util/Zigbee.bin
```

Это обновит прошивку в модуле jn5169

### Установка nodejs

Удалите файлы nodejs, которые поставляются в архиве и установите готовый пакет

```shell script
rm -rf /opt/node
rm -f /usr/bin/npm
rm -f /usr/bin/node
```

Установите пакеты на шлюз

```shell script
wget https://github.com/devbis/xiaomi-gateway-openwrt/raw/master/files/node_v10.22.0-1_arm_cortex-a9_neon.ipk
wget https://github.com/devbis/xiaomi-gateway-openwrt/raw/master/files/node-npm_v10.22.0-1_arm_cortex-a9_neon.ipk
opkg install node_v10.22.0-1_arm_cortex-a9_neon.ipk
opkg install node-npm_v10.22.0-1_arm_cortex-a9_neon.ipk
```

Дальше запускайте zesp как обычно, командой `./start.sh`
или добавьте запуск в /etc/rc.local

```shell script
cd /opt/app
/opt/app/start.sh &
```

