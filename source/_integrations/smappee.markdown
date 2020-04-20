---
title: Smappee
description: Instructions on how to setup Smappee within Open Peer Power.
ha_release: 0.64
ha_category:
  - Hub
  - Energy
  - Sensor
  - Switch
ha_iot_class: Local Push
ha_domain: smappee
excerpt: none
---

The `smappee` integration adds support for the [Smappee](https://www.smappee.com/) controller for energy monitoring and Comport plug switches.

There is currently support for the following device types within Open Peer Power:

- Sensor
- Switch

Will be automatically added when you connect to the Smappee controller.

The smappee integration gets information from [Smappee API](https://smappee.atlassian.net/wiki/spaces/DEVAPI/overview). Note: their cloud API now requires a subscription fee of €2.50 per month for Smappee Energy/Solar or €3 per month for Smappee Plus.

## Configuration

Info on how to get API access is described in the [smappy wiki](https://github.com/EnergieID/smappy/wiki).

To use the `smappee` integration in your installation, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
smappee:
  host: 10.0.0.5
  client_id: YOUR_CLIENT_ID
  client_secret: YOUR_CLIENT_SECRET
  username: YOUR_MYSMAPPEE_USERNAME
  password: YOUR_MYSMAPPEE_PASSWORD
{% endhighlight %}

{% highlight yaml %}
# Minimal example configuration.yaml entry
smappee:
  host: 10.0.0.5
{% endhighlight %}

{% highlight yaml %}
# Cloud only example configuration.yaml entry
smappee:
  client_id: YOUR_CLIENT_ID
  client_secret: YOUR_CLIENT_SECRET
  username: YOUR_MYSMAPPEE_USERNAME
  password: YOUR_MYSMAPPEE_PASSWORD
{% endhighlight %}
