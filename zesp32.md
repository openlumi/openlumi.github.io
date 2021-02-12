# Installing Zesp32

Zesp will work the same as on the stock firmware, but due to the difference
in system libraries, additional steps are required

### 1. Installing nodejs and zesp32

Add the package repository for the openlumi project

```shell
[ -f /lib/libustream-ssl.so ] && echo "libustream already installed" || opkg install libustream-mbedtls
wget https://openlumi.github.io/openwrt-packages/public.key
opkg-key add public.key
echo 'src/gz openlumi https://openlumi.github.io/openwrt-packages/packages/19.07/arm_cortex-a9_neon' >> /etc/opkg/customfeeds.conf
```

Install packages on the gateway

```shell
opkg update
opkg install node node-npm
cd /tmp
wget http://82.146.46.112/fw/ZESPowrt.tar.gz
wget http://82.146.46.112/fw/update.tar.gz
tar -xzvf ZESPowrt.tar.gz -C /
tar -xzvf update.tar.gz -C /
wget https://raw.githubusercontent.com/openlumi/openlumi.github.io/master/files/zesp32.init -O /etc/init.d/zesp32
chmod +x /etc/init.d/zesp32
wget https://raw.githubusercontent.com/openlumi/openlumi.github.io/master/files/zesp32-package.json -O /opt/app/package.json
/etc/init.d/zesp32 enable
/etc/init.d/zesp32 start
```

### 2. Zigbee firmware.

You can skip this step if you already have a Zesp zigbee firmware flashed.

Stop zesp32

```shell
killall node
```

To flash the zigbee firmware enter the following command

```shell
jnflash /opt/app/util/Zigbee.bin
jntool erase_pdm
```

It will update the firmware in the jn5169 module.
Now you can start zesp32 again

```shell
/etc/init.d/zesp32 restart
```

Zesp32 interface will be available on `http://*GATEWAY_IP*:8081/`
