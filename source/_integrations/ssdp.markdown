---
title: Simple Service Discovery Protocol (SSDP)
description: Discover integrations on the network using the SSDP protocol.
ha_category:
  - Network
ha_release: 0.94
ha_domain: ssdp
---

The `ssdp` "Simple Service Discovery Protocol" (part of UPnP) integration will scan the network for supported devices and services. Discovered integrations will show up in the discovered section on the integrations page in the configuration panel.

## Configuration

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry
ssdp:
{% endhighlight %}

## Discovered Integrations

The following integrations are automatically discovered by the SSDP integration:

 - [deCONZ](../deconz/)
 - [DirecTV](/integrations/directv/)
 - [Huawei LTE](../huawei_lte/)
 - [Philips Hue](../hue/)
 - [Samsung TV](../samsungtv/)
