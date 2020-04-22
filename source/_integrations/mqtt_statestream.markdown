---
title: MQTT Statestream
description: Instructions on how to setup MQTT Statestream within Open Peer Power.
logo: mqtt.png
ha_category:
  - Other
ha_release: 0.54
ha_iot_class: Configurable
ha_domain: mqtt_statestream
excerpt: none
---

The `mqtt_statestream` integration publishes state changes in Open Peer Power to individual MQTT topics.

## Configuration

To enable MQTT Statestream in Open Peer Power, add the following section to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
mqtt_statestream:
  base_topic: openpeerpower
  publish_attributes: true
  publish_timestamps: true
{% endhighlight %}

## Operation

When any Open Peer Power entity changes, this integration will publish that change to MQTT.

The topic for each entity is different, so you can easily subscribe other systems to just the entities you are interested in.
The topic will be in the form `base_topic/domain/entity/state`.

For example, with the example configuration above, if an entity called 'light.master_bedroom_dimmer' is turned on, this integration will publish `on` to `openpeerpower/light/master_bedroom_dimmer/state`.

If that entity also has an attribute called `brightness`, the integration will also publish the value of that attribute to `openpeerpower/light/master_bedroom_dimmer/brightness`.

All states and attributes are passed through JSON serialization before publishing. **Please note** that this causes strings to be quoted (e.g., the string 'on' will be published as '"on"'). You can access the JSON deserialized values (as well as unquoted strings) at many places by using `value_json` instead of `value`.

The last_updated and last_changed values for the entity will be published to `openpeerpower/light/master_bedroom_dimmer/last_updated` and `openpeerpower/light/master_bedroom_dimmer/last_changed`, respectively. The timestamps are in ISO 8601 format - for example, `2017-10-01T23:20:30.920969+00:00`.

## Include/exclude

The **exclude** and **include** configuration variables can be used to filter the items that are published to MQTT.

1\. If neither **exclude** or **include** are specified, all entities are published.

2\. If only **exclude** is specified, then all entities except the ones listed are published.

{% highlight yaml %}
# Example of excluding entities
mqtt_statestream:
  base_topic: openpeerpower
  exclude:
    domains:
      - switch
    entities:
      - sensor.nopublish
{% endhighlight %}
In the above example, all entities except for *switch.x* and *sensor.nopublish* will be published to MQTT.

3\. If only **include** is specified, then only the specified entries are published.

{% highlight yaml %}
# Example of excluding entities
mqtt_statestream:
  base_topic: openpeerpower
  include:
    domains:
      - sensor
    entities:
      - lock.important
{% endhighlight %}
In this example, only *sensor.x* and *lock.important* will be published.

4\. If both **include** and **exclude** are specified then all entities specified by **include** are published except for the ones
specified by **exclude**.

{% highlight yaml %}
# Example of excluding entities
mqtt_statestream:
  base_topic: openpeerpower
  include:
    domains:
      - sensor
  exclude:
    entities:
      - sensor.noshow
{% endhighlight %}
In this example, all sensors except for *sensor.noshow* will be published.
