[![Code size](https://img.shields.io/github/languages/code-size/DaKarlii/Shelly_MQTT_Domoticz)](https://github.com/DaKarlii/Shelly_MQTT_Domoticz) [![Last commit](https://img.shields.io/github/last-commit/DaKarlii/Shelly_MQTT_Domoticz)](https://github.com/DaKarlii/Shelly_MQTT_Domoticz/commits)

## Private fork

This is a private, unofficial fork of [enesbcs/Shelly_MQTT](https://github.com/enesbcs/Shelly_MQTT). It is maintained for personal use and comes without official support or guarantees. The original plugin is no longer being developed.

The original author is developing a separate script for Domoticz MQTT Autodiscovery: https://github.com/enesbcs/shellyteacher4domo.

# ShellyMQTT - Domoticz Python Plugin
Python plugin for Shelly relay devices using MQTT protocol

## Local extension: newer Shelly switches

This fork adds relay support for newer Shelly devices, including Shelly 1 Mini Gen3 and Shelly 2PM Gen4 in **switch mode**. Each `switch:<id>` is created as a Domoticz switch; its state comes from `status/switch:<id>` or `NotifyStatus` / `NotifyFullStatus` events. Commands use MQTT Control. When the corresponding fields are reported, separate devices show power and cumulative energy (W and Wh), voltage, current, power factor, frequency and internal temperature per switch channel. Existing Gen1 devices keep their previous device IDs and behavior. Cover mode is not implemented. BTHome gateways can discover a combined temperature/humidity device when exactly one temperature and one humidity component are found for a BLE address; existing dummy devices are not modified. MQTT RPC and status notifications must be enabled.

Tested with real devices on Domoticz 2026.4: automatic creation, relay control from Domoticz and status reporting for Shelly 1 Mini Gen3 and Shelly 2PM Gen4, including PM measurements and a Shelly BLU H&T sensor through the 2PM gateway. Relay changes made directly or through external apps are reflected in Domoticz.

- Set each newer device's MQTT prefix to `shellies/<device-name>` (unique per device), for example `shellies/shelly2pmg4-terra`. The default device ID also works.
- Enable MQTT, **MQTT Control**, **RPC over MQTT**, **Generic status update over MQTT**, and **RPC status notifications over MQTT** in the device settings.
- The MQTT prefix forms part of each Domoticz DeviceID. Changing it creates new Domoticz devices rather than renaming the existing ones; existing IDX, history and automation references are not migrated.
- In Domoticz, allow new devices (the five-minute permission also works) and start or restart the plugin. If no switch appears, toggle the relay so it publishes its status. A failed creation is retried on a later status message, at most once every 15 seconds per switch. The older plugin startup message about hardware creation reflects saved settings and may not reflect the five-minute permission.
- Back up your existing plugin directory and Domoticz database before replacing the plugin file. This is an unofficial fork, not an upstream release; test on a non-critical relay first.

## Prerequisites

The original plugin was tested with Domoticz v4.x. This fork has also been tested with Domoticz 2026.4 as described above.

If you do not have a working Python >=3.5 installation, please install it first! ( https://www.domoticz.com/wiki/Using_Python_plugins )
(if Shelly_MQTT does not appear in HW list after installation, read again the above article!)

Setup and run MQTT broker and an MQTT capable Shelly device. (http://shelly-api-docs.shelly.cloud/#mqtt-support-beta)
If you do not have an MQTT server yet, install Mosquitto for example:
http://mosquitto.org/blog/2013/01/mosquitto-debian-repository/

> [!WARNING]
> **Older Shelly devices:** Do not enable **Use custom MQTT prefix**; the plugin may not detect the device. Newer devices must use `shellies/<device-name>` as described above.

> [!NOTE]
> **Shelly Plus H&T:** Set the MQTT prefix to `shellies/` followed by the device ID so it publishes under `shellies/<device-id>`.

## Installation

1. Clone this fork into your Domoticz plugins folder:
```bash
cd domoticz/plugins
git clone https://github.com/DaKarlii/Shelly_MQTT_Domoticz.git
```
2. Restart domoticz
3. Go to "Hardware" page and add new item with type "ShellyMQTT"
4. Set your MQTT server address and port to plugin settings
5. Remember to allow new devices discovery in Domoticz settings

Once plugin receive any MQTT message from Shelly it will try to create appropriate device.

## Plugin install&update with plugin manager

[Domoticz Plugins Manager](https://github.com/stas-demydiuk/domoticz-plugins-manager) can manage plugins through the Domoticz GUI. Check that it is configured to use this fork; an upstream entry may install or update the original plugin instead.

## Plugin manual update

Warning: if you use this method, Domoticz may duplicate devices after it!

1. Stop domoticz
2. Go to plugin folder and pull new version
```
cd domoticz/plugins/Shelly_MQTT_Domoticz
git pull
```
3. Start domoticz

## Supported devices

Tested in this fork on Domoticz 2026.4:
 - Shelly 1 Mini Gen3 (device discovery, relay control and status reporting)
 - Shelly 2PM Gen4 in switch mode (device discovery, relay control, status reporting and PM measurements)
 - Shelly BLU H&T sensor via the 2PM Gen4 BTHome gateway (temperature, humidity and battery)

The original project lists the following devices as tested and working:
 - Shelly 1 Open Source (relay)
 - Shelly 1PM (relay)*
 - Shelly Plug (relay)*
 - Shelly Plug S (relay)*
 - Shelly2 and 2.5 Switch (relay and roller shutter mode, positioning)
 - Shelly4 Pro (relay)*
 - Shelly H&T
 - Shelly RGBW2
 - Shelly Flood
 - Shelly Door and Window sensor
 - Shelly 2LED
 - Shelly Dimmer/Shelly Dimmer2
 - Shelly Bulb RGBW
 - Shelly EM/3EM
 - Shelly Button1
 - Shelly Door Window 2
 - Shelly i3
 - Shelly Bulb Duo
 - Shelly UNI
 - Shelly 1L (relay)
 - Shelly Gas
 - Shelly Motion
 - Shelly Plus H&T (by setting MQTT prefix to `shellies`)

*Power consumption can be enabled in the plugin settings page manually, it's an optional feature without any further support

## Contributing

Issues and pull requests are welcome at [this fork](https://github.com/DaKarlii/Shelly_MQTT_Domoticz). This is a personal project, so there is no guaranteed support or response time.

## Acknowledgements

Thanks to the original author and contributors of [Shelly_MQTT](https://github.com/enesbcs/Shelly_MQTT). Its MQTT implementation was heavily based on the [Domoticz Zigbee2MQTT plugin](https://github.com/stas-demydiuk/domoticz-zigbee2mqtt-plugin).

The original author also thanked [Allterco Robotics](https://allterco.com/en/Shelly) for providing Shelly device samples that supported development of the original plugin.
