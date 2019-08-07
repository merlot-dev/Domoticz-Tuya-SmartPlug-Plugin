# Domoticz-Tuya-SmartPlug-Plugin

A Domoticz plugin to manage Tuya Smart Plug (single and multi socket device)
This is a modified plugin from the original by Tixi and sincze, The original plugin only supported an On/Off switch.
Now meters for Watt, Voltage, Ampere, kWh (calculated) are also created.

![devices](https://github.com/sincze/Domoticz-Tuya-SmartPlug-Plugin/blob/master/Tuya%20Smartplug.JPG)

Multi socket has not been tested yet by merlot. 
Plugin is provided on best-effort.

## ONLY TESTED FOR Raspberry Pi

With Python version 3.5 & Domoticz version V4.10717
## Prerequisites

This plugin is based on the pytuya Python library version 7.0.4, which includes support for Protocol version 3.3.
For the installation of this library, follow the Installation guide below.
See [`https://github.com/clach04/python-tuya/`](https://github.com/clach04/python-tuya/) for more information.

For the pytuya Python library, you need pycrypto. pycrypto can be installed with pip:
```
pip3 install pycrypto
```
See [`https://pypi.org/project/pycrypto/`](https://pypi.org/project/pycrypto/) for more information.

## Installation

Assuming that domoticz directory is installed in your home directory.
To be modified, meanwhile install as sincze and just copy my plugin.py version

```bash
cd ~/domoticz/plugins
git clone https://github.com/sincze/Domoticz-Tuya-SmartPlug-Plugin
cd Domoticz-Tuya-SmartPlug-Plugin
git clone https://github.com/clach04/python-tuya.git
ln -s ~/domoticz/plugins/Domoticz-Tuya-SmartPlug-Plugin/python-tuya/pytuya pytuya
# restart domoticz:
sudo /etc/init.d/domoticz.sh restart
```
## Known issues

1/ python environment

Domoticz may not have the path to the pycrypto library in its python environment.
In this case you will observe something starting like that in the log:
* failed to load 'plugin.py', Python Path used was 
* Module Import failed, exception: 'ImportError'

To find where pycrypto is installed, in a shell:
```bash
pip3 show pycrypto
```
The Crypto directory should be present in the directory indicated with Location.

when you have it, just add a symbolic link to it in Domoticz-Tuya-SmartPlug-Plugin directory with ```ln -s```.
Example:
```bash
cd ~/domoticz/plugins/Domoticz-Tuya-SmartPlug-Plugin
ln -s /home/pi/.local/lib/python3.5/site-packages/Crypto Crypto
```

2/ Tuya app

The tuya app must be close. This limitation is due to the tuya device itself that support only one connection.

3/ Alternative crypto libraries

PyCryptodome or pyaes can be used instead of pycrypto. 

## Updating

Like other plugins, in the Domoticz-Tuya-SmartPlug-Plugin directory:
```bash
git pull
sudo /etc/init.d/domoticz.sh restart
```

## Parameters

| Parameter | Value |
| :--- | :--- |
| **IP address** | IP of the Smart Plug eg. 192.168.1.231 |
| **Version** | version 3.1 or 3.3 of the Smart Plug |
| **DevID** | devID of the Smart Plug |
| **Local Key** | Local Key of the Smart Plug |
| **DPS** |	1 for single socket device and a list of dps separated by ';' for multisocket device eg. 1;2;3;7
| **DPS group** | None for single socket device and a list of list of dps separated by ':' for multisocket device eg. 1;2 : 3;7
| **ID Amp;Watt;Volt** | Enter the ID's of these specific devices separated by ; eg 4;5;6 (nt tested with Multisocket)
| **Debug** | default is 0 |

**DPS** should only includes values that correspond to plug's dps id. Be careful some devices also have timers in the dps state.

**DPS group** can be used to group multiple sockets in one Domoticz switch.

Helper scripts get_dps.py turnON.py and turnOFF.py can help:
* to determine the dps list
* to check that the needed information are valid (i.e. devID and Local Key) before using the plugin.

## DevID & Local Key Extraction

Recommanded method:
[`https://github.com/codetheweb/tuyapi/blob/master/docs/SETUP.md`](https://github.com/codetheweb/tuyapi/blob/master/docs/SETUP.md)

All the information can be found here:
[`https://github.com/clach04/python-tuya/`](https://github.com/clach04/python-tuya/)

## Acknowledgements

* Special thanks for all the hard work of [clach04](https://github.com/clach04), [codetheweb](https://github.com/codetheweb/) and all the other contributers on [python-tuya](https://github.com/clach04/python-tuya) and [tuyapi](https://github.com/codetheweb/tuyapi) who have made communicating to Tuya devices possible with open source code.
* Domoticz team
* Original Plugin by [Tixi](https://github.com/tixi/Domoticz-Tuya-SmartPlug-Plugin)
