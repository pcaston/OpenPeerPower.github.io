---
title: "MQTT Logging"
description: "Instructions on how to setup MQTT Logging within Open Peer Power."
logo: mqtt.png
---

The [logger](/integrations/logger/) integration allows the logging of received MQTT messages.

{% highlight yaml %}
# Example configuration.yaml entry
logger:
  default: warning
  logs:
    openpeerpower.components.mqtt: debug
{% endhighlight %}

