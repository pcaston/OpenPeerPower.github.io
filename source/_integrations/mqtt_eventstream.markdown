---
title: MQTT Eventstream
description: Instructions on how to setup MQTT eventstream within Open Peer Power.
logo: mqtt.png
ha_category:
  - Other
ha_release: 0.11
ha_iot_class: Configurable
ha_domain: mqtt_eventstream
---

The `mqtt_eventstream` integration connects two Open Peer Power instances via MQTT.

## Configuration

To integrate MQTT Eventstream into Open Peer Power, add the following section to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
mqtt_eventstream:
  publish_topic: MyServerName
  subscribe_topic: OtherHaServerName
{% endhighlight %}

{% configuration %}
publish_topic:
  description: Topic for publishing local events.
  required: false
  type: string
subscribe_topic:
  description: Topic to receive events from the remote server.
  required: false
  type: string
ignore_event:
  description: Ignore sending these [events](/docs/configuration/events/) over mqtt.
  required: false
  type: list
{% endconfiguration %}

## Multiple Instances

Events from multiple instances can be aggregated to a single master instance by subscribing to a wildcard topic from the master instance.

{% highlight yaml %}
# Example master instance configuration.yaml entry
mqtt_eventstream:
  publish_topic: master/topic
  subscribe_topic: slaves/#
  ignore_event:
    - call_service
    - state_changed
{% endhighlight %}

For a multiple instance setup, each slave would publish to their own topic.

{% highlight yaml %}
# Example slave instance configuration.yaml entry
mqtt_eventstream:
  publish_topic: slaves/upstairs
  subscribe_topic: master/topic
{% endhighlight %}

{% highlight yaml %}
# Example slave instance configuration.yaml entry
mqtt_eventstream:
  publish_topic: slaves/downstairs
  subscribe_topic: master/topic
{% endhighlight %}
