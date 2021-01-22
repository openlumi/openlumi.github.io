# Installing Domoticz

The OpenWrt 10.07 repository contains a rather old version of Domoticz.
To install the recent version you should use the script, that you have to run
in console

```sh
wget https://github.com/openlumi/xiaomi-gateway-openwrt/raw/master/files/domoticz.sh -O - | sh
````

It will install all requirements, Domoticz, and a plugin to work with zigbee.
Domoticz interface will run on 8080 port.

# Flash Zigate firmware and install Zigate plugin to Domoticz

For the jn5169 chip, which is installed on our gateway, the Zigate firmware is used.
To flash it run the following command

```sh
wget https://github.com/openlumi/ZiGate/releases/download/snapshot-20201201/ZigbeeNodeControlBridge_JN5169_FULL_FUNC_DEVICE_31e_115200.bin -O /tmp/zigate.bin 
jnflash /tmp/zigate.bin
jntool erase_pdm
```
## Plugin configuration

All you need to do is add a hardware with the type
`Zigate plugin`, enter a name in the Name field and click` Add`

![Adding Zigate plugin](images/zigate_plugin.png)

To configure use the following parameters

    Zigate Model: DIN
    Serial Port: /dev/ttymxc1


the plugin will be initialized and launch the control panel on the 9440 port 
after a few minutes.

## Zigate Control Panel


![Zigate dashboard](images/zigate_dashboard.png)

On this panel, you can switch on the pairing mode with child devices and
change settings.
To enable receiving of new devices, turn on the switch
"Accept new hardware" in the upper right corner.

You can read more about the dashboard on the plugin's page
[https://github.com/pipiche38/Domoticz-Zigate-Wiki/blob/master/en-eng/WebUserInterfaceNavigation.md](https://github.com/pipiche38/Domoticz-Zigate-Wiki/blob/master/en-eng/WebUserInterfaceNavigation.md)

After adding a child device, it will appear in the Domoticz interface.
