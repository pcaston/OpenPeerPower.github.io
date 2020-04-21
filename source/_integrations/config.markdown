---
title: Configuration
description: Instructions on how to setup the configuration panel for Open Peer Power.
ha_category:
  - Front End
ha_release: 0.39
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
ha_domain: config
---

The `config` integration is designed to display panels in the frontend to configure and manage parts of Open Peer Power.

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
# Example configuration.yaml entry
config:
{% endhighlight %}

### Integrations

This section enables you to manage integrations for devices such as Philips Hue and Sonos from within Open Peer Power.

### Users

This section enables you to manage your Open Peer Power users.

### General

This section enables you to manage the name, location, and unit system of your Open Peer Power installation.

### Server Control

This section enables you to control Open Peer Power from within Open Peer Power. Check your configuration, reload the core, groups, scripts, automations, and the Open Peer Power process itself with a single mouse click.

<p class='img'>
  <img src='{{site_root}}/images/screenshots/server-management.png' />
</p>

### Persons

This section enables you to associate users with their device tracker entities using the person component.

### Entity Registry

This section enables you to override the name, change the entity ID or disable an entity in Open Peer Power.

### Area Registry

This section enables you to organize entities to physical areas of your home.

### Automation

This section enables you to create and modify automations from within Open Peer Power, without needing to write out the YAML code.

### Script

Similar to the automation editor, this section enables you to create and modify scripts from within Open Peer Power, without needing to write out the YAML code.

### Z-Wave

This section enables you to control your Z-Wave network and devices from within Open Peer Power. You can add and remove devices, as well as change device specific configuration variables.

### Customization

This section enables you to customize entities within Open Peer Power. Use this to set friendly names, change icons, hide entities, and modify other attributes.
