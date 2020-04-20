---
title: Generic Thermostat
description: Turn Open Peer Power into a thermostat
ha_category:
  - Climate
ha_release: pre 0.7
ha_iot_class: Local Polling
ha_domain: generic_thermostat
excerpt: none
---

The `generic_thermostat` climate platform is a thermostat implemented in Open Peer Power. It uses a sensor and a switch connected to a heater or air conditioning under the hood. When in heater mode, if the measured temperature is cooler than the target temperature, the heater will be turned on and turned off when the required temperature is reached. When in air conditioning mode, if the measured temperature is hotter than the target temperature, the air conditioning will be turned on and turned off when required temperature is reached. One Generic Thermostat entity can only control one switch. If you need to activate two switches, one for a heater and one for an air conditioner, you will need two Generic Thermostat entities.

{% highlight yaml %}
# Example configuration.yaml entry
climate:
  - platform: generic_thermostat
    name: Study
    heater: switch.study_heater
    target_sensor: sensor.study_temperature
{% endhighlight %}

Time for `min_cycle_duration` and `keep_alive` must be set as "hh:mm:ss" or it must contain at least one of the following entries: `days:`, `hours:`, `minutes:`, `seconds:` or `milliseconds:`. Alternatively, it can be an integer that represents time in seconds.

Currently the `generic_thermostat` climate platform supports 'heat', 'cool' and 'off' HVAC modes. You can force your `generic_thermostat` to avoid starting by setting HVAC mode to 'off'.

Please note that when changing the preset mode to away, you will force a target temperature change as well that will get restored once the preset mode is set to none again.

## Full configuration example

{% highlight yaml %}
climate:
  - platform: generic_thermostat
    name: Study
    heater: switch.study_heater
    target_sensor: sensor.study_temperature
    min_temp: 15
    max_temp: 21
    ac_mode: false
    target_temp: 17
    cold_tolerance: 0.3
    hot_tolerance: 0
    min_cycle_duration:
      seconds: 5
    keep_alive:
      minutes: 3
    initial_hvac_mode: "off"
    away_temp: 16
    precision: 0.1
{% endhighlight %}
