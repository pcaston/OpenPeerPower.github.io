---
title: "MQTT"
description: "Details about the MQTT support of Open Peer Power."
---

MQTT (aka MQ Telemetry Transport) is a machine-to-machine or "Internet of Things" connectivity protocol on top of TCP/IP. It allows extremely lightweight publish/subscribe messaging transport.

To integrate MQTT into Open Peer Power, add the following section to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
mqtt:
  broker: IP_ADDRESS
{% endhighlight %}

For detailed setup instructions, please refer to the [MQTT broker](/docs/mqtt/broker) documentation.

## Additional features

- [Certificate](/docs/mqtt/certificate/)
- [Discovery](/docs/mqtt/discovery/)
- [Publish service](/docs/mqtt/service/)
- [Birth and last will messages](/docs/mqtt/birth_will/)
- [Testing your setup](/docs/mqtt/testing/)
- [Logging](/docs/mqtt/logging/)
- [Processing JSON](/docs/mqtt/processing_json/)

See the [MQTT example component](/cookbook/python_component_mqtt_basic/) how to integrate your own component.
