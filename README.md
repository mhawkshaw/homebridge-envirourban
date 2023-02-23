
<p align="center">

<img src="https://github.com/homebridge/branding/raw/master/logos/homebridge-wordmark-logo-vertical.png" width="150">

</p>


# Pimoroni Enviro Urban Homebridge Plug-in

This Homebridge plug-in allows you to make the data available from the [Pimoroni Enviro Urban](https://learn.pimoroni.com/article/getting-started-with-enviro) device.

## Setup MQTT broker

This Homebridge plug-in reads the data from an MQTT broker providing the JSON information, for example:

* {"readings": {"pressure": 997.99, "pm1": 0, "pm2_5": 4, "noise": 0.015, "humidity": 47.19, "temperature": 22.26, "pm10": 4, "voltage": 0.0}, "nickname": "my-air-quality", "model": "urban", "uid": "xxxxxxxxxxxxxx", "timestamp": "2023-02-23T21:45:09Z"}

You can view the entries on your computer using an MQTT viewer, for example [MQTT Explorer](http://mqtt-explorer.com/)

You need to install an [MQTT broker](http://mosquitto.org/) on your machine, this can be any machine in your network, including the machine running Homebridge. Here are some instructions for popular distributions:

### Raspberry Pi / Ubuntu

In short, you just need to do the following:

    sudo apt-get update
    sudo apt-get install -y mosquitto mosquitto-clients
    sudo systemctl enable mosquitto.service

### macOS

Use [Homebrew](https://brew.sh/)

    brew install mosquitto

### Windows

Go to the (Mosquitto Download Page)[https://mosquitto.org/download/] and choose the right installer for your system.

### Enable authentication

A quick search online will provide you with information on how to secure your installation. To help you, I've found the following links for the 
[Raspberry Pi](https://randomnerdtutorials.com/how-to-install-mosquitto-broker-on-raspberry-pi/) and [Ubuntu](https://www.vultr.com/docs/install-mosquitto-mqtt-broker-on-ubuntu-20-04-server/)

## Plug-in Installation

Follow the [homebridge installation instructions](https://www.npmjs.com/package/homebridge) if you haven't already.

Install this plugin globally:

    npm install -g homebridge-envirourban

Add platform to `config.json`, for configuration see below.

## Plug-in Configuration

The plug-in needs to know where to find the MQTT broker providing the JSON data (e.g. mqtt://127.0.0.1:1883) along with the serial number of the device to uniquely identify it (you can also use your Raspberry Pi identifier).

```json
{
  "platforms": [
    {
      "platform": "EnviroUrbanAirQuality",
      "mqttbroker": "mqtt://127.0.0.1:1883",
      "username": "",
      "password": "",
      "devices": [
        {
          "displayName": "My Enviro Urban Sensor",
          "serial": "1234567890",
          "topic": "enviro/my-air-quality"
        }
      ],
      "excellent": 10,
      "good": 20,
      "fair": 25,
      "inferior": 50,
      "poor": 50
    }
  ]
}

```

The following settings are optional:

- `username`: the MQTT broker username
- `password`: the MQTT broker password
- `excellent`: the upper value for PM2.5 air quality considered to be excellent (in micrograms per cubic metre). Default is 10.
- `good`: the upper value for PM2.5 air quality considered to be good (in micrograms per cubic metre). Default is 20.
- `fair`: the upper value for PM2.5 air quality considered to be fair (in micrograms per cubic metre). Default is 25.
- `inferior`: the upper value for PM2.5 air quality considered to be inferior (in micrograms per cubic metre). Default is 50.
- `poor`: the lowest value for PM2.5 air quality considered to be poor (in micrograms per cubic metre). Anything above this is considered to be poor. Default is 50.

If you have multiple Enviro Urban devices, then you can list them all in the config giving each one a unique name, MQTT topic and serial number.