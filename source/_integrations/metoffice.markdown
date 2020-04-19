---
title: Met Office
description: Instructions on how to integrate Met Office weather conditions into Open Peer Power.
ha_category:
  - Weather
ha_release: 0.42
ha_iot_class: Cloud Polling
ha_domain: metoffice
excerpt: none
---

The `metoffice` weather platform uses the Met Office's [DataPoint API](https://www.metoffice.gov.uk/datapoint) for weather data.

## Configuration

To add the Met Office weather platform to your installation, you'll need to register for a free API key at the link above and then add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
weather:
  - platform: metoffice
    api_key: YOUR_API_KEY
{% endhighlight %}

<div class='note'>

This platform is an alternative to the [`metoffice`](/integrations/sensor.metoffice/) sensor.
The weather platform is easier to configure but less customizable.

</div>
