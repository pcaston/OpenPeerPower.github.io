---
title: "MQTT Testing"
description: "Instructions on how to test your MQTT setup."
logo: mqtt.png
---

The `mosquitto` broker package ships commandline tools (often as `*-clients` package) to send and receive MQTT messages. As an alternative have a look at [hbmqtt_pub](http://hbmqtt.readthedocs.org/en/latest/references/hbmqtt_pub.html) and [hbmqtt_sub](http://hbmqtt.readthedocs.org/en/latest/references/hbmqtt_sub.html) which are provided by HBMQTT. For sending test messages to a broker running on localhost check the example below:

{% highlight bash %}
$ mosquitto_pub -h 127.0.0.1 -t open-peer-power/switch/1/on -m "Switch is ON"
{% endhighlight %}

If you are using the embedded MQTT broker, the command looks a little different because you need to add the MQTT protocol version and your [broker credentials](/docs/mqtt/broker#embedded-broker).

{% highlight bash %}
$ mosquitto_pub -V mqttv311 -u openpeerpower -P <broker password> -t "hello" -m world
{% endhighlight %}

Another way to send MQTT messages by hand is to use the "Developer Tools" in the Frontend. Choose the "MQTT" tab. Enter something similar to the example below into the "Topic" field.

{% highlight bash %}
   open-peer-power/switch/1/power
 {% endhighlight %}
 and in the Payload field
 {% highlight bash %}
   ON
{% endhighlight %}
In the "Listen to a topic" field, type # to see everything, or "open-peer-power/switch/#" to just follow the published topic. Press "Start Listening" and then press "Publish". The result should appear similar to the text below 

{% highlight bash %}
Message 23 received on open-peer-power/switch/1/power/stat/POWER at 12:16 PM:
ON
QoS: 0 - Retain: false
Message 22 received on open-peer-power/switch/1/power/stat/RESULT at 12:16 PM:
{
    "POWER": "ON"
}
QoS: 0 - Retain: false
{% endhighlight %}

For reading all messages sent on the topic `open-peer-power` to a broker running on localhost:

{% highlight bash %}
$ mosquitto_sub -h 127.0.0.1 -v -t "open-peer-power/#"
{% endhighlight %}

For the embedded MQTT broker the command looks like:

{% highlight bash %}
$ mosquitto_sub -v -V mqttv311 -u openpeerpower -P <broker password> -t "#"
{% endhighlight %}

