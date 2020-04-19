---
title: MQTT Room Presence
description: Instructions on how to track room presence within Open Peer Power.
logo: mqtt.png
ha_category:
  - Presence Detection
ha_release: 0.27
ha_iot_class: Configurable
ha_domain: mqtt_room
excerpt: none
---

The `mqtt_room` sensor platform allows you to detect the indoor location of devices using MQTT clients.

## Configuration

To use this device tracker in your installation, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  - platform: mqtt_room
    device_id: 123testid
{% endhighlight %}

## Usage

Example JSON that should be published to the room topics:

{% highlight json %}
{
  "id": "123testid",
  "name": "Test Device",
  "distance": 5.678
}
{% endhighlight %}

### Setting up clients

This integration works with any software that is sending data in the given format. Each client should post the discovered devices in its own subtopic of the configured topic.
Instead of developing your own application, you can also use any of these already existing clients:

- [**room-assistant**](https://github.com/mKeRix/room-assistant): looks for Bluetooth LE beacons, based on Node.js
- [**Happy Bubbles Presence Server**](https://github.com/happy-bubbles/presence): presence detection server for Happy Bubbles BLE-scanning devices, based on Go
- [**ESP32-MQTT-room**](https://jptrsn.github.io/ESP32-mqtt-room/): runs on an ESP32, and looks for Bluetooth LE devices, based on C++/Arduino
