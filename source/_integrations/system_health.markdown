---
title: System Health
description: System Health integration will report system info and allow to run system diagnostics.
ha_category:
  - Other
ha_release: 0.87
ha_quality_scale: internal
ha_domain: system_health
---

The System Health integration provides an API to offer information on the system and its components. It also allows to run diagnostic tools to diagnose problems.

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry
system_health:
{% endhighlight %}

System Health integration data can be viewed in Developer Tools on the "Info" tab.
