---
title: Open Peer Power Mobile App Support
description: The Mobile App integration allows a generic platform for integrating with mobile apps.
ha_category:
  - Other
ha_release: 0.89
ha_config_flow: true
ha_quality_scale: internal
ha_codeowners:
  - '@robbiet480'
ha_domain: mobile_app
---

The Mobile App integration allows Open Peer Power mobile apps to easily integrate with Open Peer Power.

If you are planning to use a mobile application that integrates with Open Peer Power, we recommend that you keep this integration enabled.

## Configuration

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry
mobile_app:
{% endhighlight %}

## Apps that use Mobile App

- [Open Peer Power for iOS](https://apps.apple.com/us/app/open-peer-power/id1099568401?ls=1) (official)
- [Open Peer Power for Android](https://play.google.com/store/apps/details?id=io.openpeerpower.companion.android) (official)

## Mobile App Documentation

- [Companion documentation](https://companion.openpeerpower.io/)
