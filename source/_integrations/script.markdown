---
title: Scripts
description: Instructions on how to setup scripts within Open Peer Power.
ha_category:
  - Automation
ha_release: 0.7
ha_quality_scale: internal
ha_codeowners:
  - '@open-peer-power/core'
ha_domain: script
excerpt: none
---

The `script` integration allows users to specify a sequence of actions to be executed by Open Peer Power. These are run when you turn the script on. The script integration will create an entity for each script and allow them to be controlled via services.

## Configuration

The sequence of actions is specified using the [Open Peer Power Script Syntax](/getting-started/scripts/).

{% highlight yaml %}
# Example configuration.yaml entry
script:
  message_temperature:
    sequence:
      # This is Open Peer Power Script Syntax
      - service: notify.notify
        data_template:
          message: Current temperature is {% raw %}{{ states('sensor.temperature') }}{% endraw %}
{% endhighlight %}

<div class='note'>

Script names (e.g., `message_temperature` in the example above) are not allowed to contain capital letters, or dash (minus) characters, i.e., `-`. The preferred way to separate words for better readability is to use underscore (`_`) characters.

</div>

### Full Configuration

{% raw %}

{% highlight yaml %}
script: 
  wakeup:
    alias: Wake Up
    icon: "mdi:party-popper"
    description: 'Turns on the bedroom lights and then the living room lights after a delay'
    fields:
      minutes:
        description: 'The amount of time to wait before turning on the living room lights'
        example: 1
    sequence:
      # This is Open Peer Power Script Syntax
      - event: LOGBOOK_ENTRY
        event_data:
          name: Paulus
          message: is waking up
          entity_id: device_tracker.paulus
          domain: light
      - alias: Bedroom lights on
        service: light.turn_on
        data:
          entity_id: group.bedroom
          brightness: 100
      - delay:
          # supports seconds, milliseconds, minutes, hours
          minutes: {{ minutes }}
      - alias: Living room lights on
        service: light.turn_on
        data:
          entity_id: group.living_room
{% endhighlight %}

{% endraw %}

### Passing variables to scripts

As part of the service, variables can be passed along to a script so they become available within templates in that script.

There are two ways to achieve this. One way is using the generic `script.turn_on` service. To pass variables to the script with this service, call it with the desired variables:

{% highlight yaml %}
# Example configuration.yaml entry
automation:
  trigger:
    platform: state
    entity_id: light.bedroom
    from: 'off'
    to: 'on'
  action:
    service: script.turn_on
    entity_id: script.notify_pushover
    data:
      variables:
        title: 'State change'
        message: 'The light is on!'
{% endhighlight %}

The other way is calling the script as a service directly. In this case, all service data will be made available as variables. If we apply this approach on the script above, it would look like this:

{% highlight yaml %}
# Example configuration.yaml entry
automation:
  trigger:
    platform: state
    entity_id: light.bedroom
    from: 'off'
    to: 'on'
  action:
    service: script.notify_pushover
    data:
      title: 'State change'
      message: 'The light is on!'
{% endhighlight %}

Using the variables in the script requires the use of `data_template`:

{% highlight yaml %}
# Example configuration.yaml entry
script:
  notify_pushover:
    description: 'Send a pushover notification'
    fields:
      title:
        description: 'The title of the notification'
        example: 'State change'
      message:
        description: 'The message content'
        example: 'The light is on!'
    sequence:
      - condition: state
        entity_id: switch.pushover_notifications
        state: 'on'
      - service: notify.pushover
        data_template:
          title: "{% raw %}{{ title }}{% endraw %}"
          message: "{% raw %}{{ message }}{% endraw %}"
{% endhighlight %}

### In the Overview

Scripts in the Overview panel will be displayed with an **EXECUTE** button if the device has no `delay:` or `wait:` statement, and as a toggle switch if it has either of those.

This is to enable you to stop a running script.
