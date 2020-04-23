---
title: Zero-configuration networking (zeroconf)
description: Exposes Open Peer Power using the Zeroconf protocol.
ha_category:
  - Network
ha_release: 0.18
ha_quality_scale: internal
ha_codeowners:
  - '@robbiet480'
  - '@Kane610'
ha_domain: zeroconf
---

The `zeroconf` integration will scan the network for supported devices and services. Discovered integrations will show up in the discovered section on the integrations page in the configuration panel. It will also make Open Peer Power discoverable for other services in the network. Zeroconf is also sometimes known as Bonjour, Rendezvous, and Avahi.

## Configuration

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, and you wish to have Open Peer Power scan for integrations using zeroconf and HomeKit, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry
zeroconf:
{% endhighlight %}
