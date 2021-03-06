pimatic shell execute plugin
=======================
This plugin let you define devices that execute shell commands. Additonal it allowes you
to execute shell commands in rule actions. So you can define rules of the form:

    if ... then execute "some command"

Configuration
-------------
You can load the plugin by editing your `config.json` to include:

    { 
       "plugin": "shell-execute"
    }


Devices can be defined by adding them to the `devices` section in the config file.
Set the `class` attribute to `ShellSwitch`. For example:

    { 
      "id": "light",
      "name": "Lamp",
      "class": "ShellSwitch", 
      "onCommand": "echo on",
      "offCommand": "echo off",
      "getStateCommand": "echo false"
      "interval": "0"
    }

Or you can define a sensor which attributes gets updated with the output of shell command:

    { 
      "id": "temperature",
      "name": "Room Temperature",
      "class": "ShellSensor", 
      "attributeName": "temperature",
      "attributeType": "number",
      "attributeUnit": "°C",
      "command": "echo 42.0"
    }

If you're running pimatic on a RaspberryPi, you can use the following sensors for a quick overview of your system health:

    {
      "id": "wlan-strength",
      "name": "WLAN Strength",
      "class": "ShellSensor",
      "attributeName": "wlan-strength",
      "attributeType": "number",
      "attributeUnit": "%",
      "command": "iwconfig wlan0 | grep Signal | sed -n -e 's/^.*Signal level.\\([0-9]*\\).*/\\1/gp'",
      "interval": 15000
    },
    {
      "id": "mem-usage",
      "name": "Memory Usage",
      "class": "ShellSensor",
      "attributeName": "mem-usage",
      "attributeType": "number",
      "attributeUnit": "MB",
      "command": "free -m | awk '$5 ~ /[0-9.]+/ { print $3 }'",
      "interval": 60000
    },
    {
      "id": "disk-usage",
      "name": "Disk Usage",
      "class": "ShellSensor",
      "attributeName": "disk-usage",
      "attributeType": "number",
      "attributeUnit": "%",
      "command": "df | awk '/^\\/dev\\/root/ { printf \"%.1f\", ($3/$2)*100 }'",
      "interval": 300000
    },
    {
      "id": "cpu-temp",
      "name": "CPU Temperature",
      "class": "ShellSensor",
      "attributeName": "cpu-temp",
      "attributeType": "number",
      "attributeUnit": "°C",
      "command": "/opt/vc/bin/vcgencmd measure_temp | cut -d \"=\" -f2 | cut -d \"'\" -f1",
      "interval": 60000
    }

For device configuration options see the [device-config-schema](device-config-schema.html) file.
