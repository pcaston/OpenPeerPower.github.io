---
title: Home Asssitant WebSocket API
description: Instructions on how to setup the WebSocket API within Open Peer Power.
ha_category:
  - Other
ha_release: 0.34
ha_quality_scale: internal
ha_codeowners:
  - '@open-peer-power/core'
ha_domain: websocket_api
---

The `websocket_api` integration set up a WebSocket API and allows one to interact with a Open Peer Power instance that is running headless. This integration depends on the [`http` component](/integrations/http/).

<div class='note warning'>

It is HIGHLY recommended that you set the `api_password`, especially if you are planning to expose your installation to the internet.

</div>

## Configuration

{% highlight yaml %}
# Example configuration.yaml entry
websocket_api:
{% endhighlight %}

## Track current connections

The websocket API provides a sensor that will keep track of the number of current connected clients. You can add it by adding the following to your configuration:

{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  platform: websocket_api
{% endhighlight %}
