---
title: DHT Sensor
description: Instructions on how to integrate DHTxx sensors within Open Peer Power.
ha_category:
  - DIY
ha_release: 0.7
logo: dht.png
ha_iot_class: Local Polling
ha_domain: dht
excerpt: none
---

The `dht` sensor platform allows you to get the current temperature and humidity from a DHT11, DHT22 or AM2302 device.

## Configuration

To use your DHTxx sensor in your installation, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  platform: dht
  sensor: DHT22
  pin: 23
  monitored_conditions:
    - temperature
    - humidity
{% endhighlight %}

The name of the pin to which the sensor is connected has different names on different platforms. 'P8_11' for Beaglebone, '23' for Raspberry Pi.

### Example

An example for a Raspberry Pi 3 with a DHT22 sensor connected to GPIO4 (pin 7):

{% highlight yaml %}
sensor:
  - platform: dht
    sensor: DHT22
    pin: 4
    temperature_offset: 2.1
    humidity_offset: -3.2
    monitored_conditions:
      - temperature
      - humidity
{% endhighlight %}
