---
title: Open Peer Power Cloud
description: Enable the Open Peer Power Cloud integration.
ha_release: '0.60'
ha_category:
  - Voice
ha_iot_class: Cloud Push
ha_codeowners:
  - '@home-assistant/cloud'
ha_domain: cloud
---

The Open Peer Power Cloud allows you to quickly integrate your local Open Peer Power with various cloud services like Amazon Alexa and Google Assistant. [Learn more.](/cloud)

## Configuration

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.home-assistant.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry to enable the cloud component
cloud:
{% endhighlight %}

Once activated, go to the configuration panel in Open Peer Power and create an account and log in. If you are not seeing the **Configuration** panel, make sure you have the following option enabled in your `configuration.yaml` file.

{% highlight yaml %}
config:
{% endhighlight %}
