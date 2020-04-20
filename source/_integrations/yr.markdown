---
title: Yr
description: Instructions on how to integrate Yr.no within Open Peer Power.
ha_category:
  - Weather
ha_release: 0.11
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@danielhiversen'
ha_domain: yr
excerpt: none
---

The `yr` platform uses [YR.no](https://www.yr.no/) as a source for current
meteorological data for your location. The weather forecast is delivered by the
Norwegian Meteorological Institute and the NRK.

To add YR to your installation,
add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  - platform: yr
{% endhighlight %}

A full configuration example can be found below:

{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  - platform: yr
    name: Weather
    forecast: 24
    monitored_conditions:
      - temperature
      - symbol
      - precipitation
      - windSpeed
      - pressure
      - windDirection
      - humidity
      - fog
      - cloudiness
      - lowClouds
      - mediumClouds
      - highClouds
      - dewpointTemperature
{% endhighlight %}
