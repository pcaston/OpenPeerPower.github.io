---
title: "Weather Forecast Card"
sidebar_label: Weather Forecast
description: "The Weather Forecast card displays the weather. Very useful to include on interfaces that people display on the wall."
excerpt: none
---

The Weather Forecast card displays the weather. Very useful to include on interfaces that people display on the wall.

<p class='img'>
<img src='/images/lovelace/lovelace_weather.png' alt='Screenshot of the weather card'>
Screenshot of the weather card.
</p>

{% configuration %}
type:
  required: true
  description: weather-forecast
  type: string
entity:
  required: true
  description: "The `entity_id` of the `weather` platform to use."
  type: string
name:
  required: false
  description: Overwrites the friendly name.
  type: string
  default: Entity Name
theme:
  required: false
  description: "Set to any theme within `themes.yaml`"
  type: string
{% endconfiguration %}

Example

{% highlight yaml %}
type: weather-forecast
entity: weather.dark_sky
{% endhighlight %}

<div class="note">

  This card works only with platforms that define a `weather` entity.
  
  E.g., it works with [Dark Sky](/integrations/weather.darksky/) but not [Dark Sky Sensor](/integrations/darksky)

</div>
