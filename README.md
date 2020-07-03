Note: Fix the bug when domoticz-zigbee2mqtt-plugin skips addition of new device due to incomplete model information on join zigbee network event with the message: <b>doesn't have "model" attribute, skipped</b>.

<b>Insert after the following code</b> (line 166, file plugin.py)

```
            if message['type'] == 'device_connected' or message['type'] == 'device_removed':
                self.publishToMqtt('bridge/config/devices', '')
```                
 <b>this code:</b>
``` 
            elif message['type'] == 'pairing' and message == 'interview_successful':
                self.publishToMqtt('bridge/config/devices', '')
```

# Zigbee2MQTT - Domoticz Python Plugin
Python plugin for Domoticz to add integration with [zigbee2mqtt](https://github.com/Koenkk/zigbee2mqtt) project

![image](https://user-images.githubusercontent.com/2734836/79760084-363b2e00-8328-11ea-994f-32b7b58ea6be.png)

## Prerequisites

- Setup and run zigbee2mqtt server (https://www.zigbee2mqtt.io).
- Make sure that your Domoticz supports Python plugins (https://www.domoticz.com/wiki/Using_Python_plugins)

## Installation

You can use [Plugins Manager](https://github.com/stas-demydiuk/domoticz-plugins-manager) for automatic installation or follow manual steps:

1. Clone repository into your domoticz plugins folder
```
cd domoticz/plugins
git clone https://github.com/stas-demydiuk/domoticz-zigbee2mqtt-plugin.git zigbee2mqtt
```
2. Restart domoticz
3. Make sure that "Accept new Hardware Devices" is enabled in Domoticz settings
4. Go to "Hardware" page and add new item with type "Zigbee2MQTT"
5. Set your MQTT server address and port to plugin settings

Once plugin receive device list from zigbee2mqtt server it will create appropriate domoticz devices. You will find these devices on `Setup -> Devices` page.

## Plugin update

1. Go to plugin folder and pull new version
```
cd domoticz/plugins/zigbee2mqtt
git pull
```
2. Restart domoticz

## Zigbee groups support

Plugin supports [zigbee groups](https://www.zigbee2mqtt.io/information/groups.html). It is impossible to automatically identify what device should represent the group (OnOff switch, dimmer, etc.), so plugin relies on suffixes in zigbee group name. Currently following suffixes are supported:

| Suffix  | Device Type                           | Example        |
|---------|---------------------------------------|----------------|
| _dimmer | Dimmer Switch                         | mygroup_dimmer |
| _ct     | Dimmer Switch with Color Temperature  | mygroup_ct     |
| _rgb    | RGB Switch                            | mygroup_rgb    |
| _rgbw   | RGBW Switch                           | mygroup_rgbw   |

if no suffix will be found then On/Off switch will be created by default

## Supported zigbee devices

see the full list [here](SUPPORTED_DEVICES.md)
