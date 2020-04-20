---
title: Heatmiser
description: Instructions on how to integrate Heatmiser thermostats within Open Peer Power.
logo: heatmiser.png
ha_category:
  - Climate
ha_release: '0.10'
ha_iot_class: Local Polling
ha_codeowners:
  - '@andylockran'
ha_domain: heatmiser
excerpt: none
---

The `heatmiser` climate platform let you control [Heatmiser DT/DT-E/PRT/PRT-E](https://www.heatmisershop.co.uk/room-thermostats/) thermostats from Heatmiser. The module itself is currently setup to work over a RS232 -> RS485 converter, therefore it connects over IP.

Further work would be required to get this setup to connect over Wi-Fi, but the HeatmiserV3 Python module being used is a full implementation of the V3 protocol.

To set it up, add the following information to your `configuration.yaml` file:

{% highlight yaml %}
climate:
  - platform: heatmiser
    host: YOUR_IP_ADDRESS
    port: YOUR_PORT
    tstats:
      - id: THERMOSTAT_ID
        name: THERMOSTAT_NAME
{% endhighlight %}

A single interface can handle up to 32 connected devices.
