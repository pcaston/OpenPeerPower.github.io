---
title: Alert
description: Instructions on how to setup automatic alerts within Open Peer Power.
ha_category:
  - Automation
ha_release: 0.38
ha_quality_scale: internal
ha_domain: alert
excerpt: none
---

The `alert` integration is designed to notify you when problematic issues arise.
For example, if the garage door is left open, the `alert` integration can be used
remind you of this by sending you repeating notifications at customizable
intervals. This is also used for low battery sensors,
water leak sensors, or any condition that may need your attention.

Alerts will add an entity to the front end only when they are firing.
This entity allows you to silence an alert until it is resolved.

### Basic Example

The `alert` integration makes use of any of the `notifications` integrations. To
setup the `alert` integration, first, you must setup a `notification` integration.
Then, add the following to your configuration file:

{% highlight yaml %}
# Example configuration.yaml entry
alert:
  garage_door:
    name: Garage is open
    done_message: Garage is closed
    entity_id: input_boolean.garage_door
    state: 'on'
    repeat: 30
    can_acknowledge: true
    skip_first: true
    notifiers:
      - ryans_phone
      - kristens_phone
{% endhighlight %}


In this example, the garage door status (`input_boolean.garage_door`) is watched
and this alert will be triggered when its status is equal to `on`.
This indicates that the door has been opened. Because the `skip_first` option
was set to `true`, the first notification will not be delivered immediately.
However, every 30 minutes, a notification will be delivered until either
`input_boolean.garage_door` no longer has a state of `on` or until the alert is
acknowledged using the Open Peer Power frontend.

For notifiers that require other parameters (such as `twilio_sms` which requires
you specify a `target` parameter when sending the notification), you can use the
`group` notification to wrap them for an alert.
Simply create a `group` notification type with a single notification member
(such as `twilio_sms`) specifying the required parameters other than `message`
provided by the `alert` component:

{% highlight yaml %}
- platform: group
  name: john_phone_sms
  services:
    - service: twilio_sms
      data:
        target: !secret john_phone
{% endhighlight %}

{% highlight yaml %}
alert:
  freshwater_temp_alert:
    name: "Warning: I have detected a problem with the freshwater tank temperature"
    entity_id: binary_sensor.freshwater_temperature_status
    state: 'on'
    repeat: 5
    can_acknowledge: true
    skip_first: false
    notifiers:
      - john_phone_sms
{% endhighlight %}

### Complex Alert Criteria

By design, the `alert` integration only handles very simple criteria for firing.
That is, it only checks if a single entity's state is equal to a value. At some
point, it may be desirable to have an alert with a more complex criteria.
Possibly, when a battery percentage falls below a threshold. Maybe you want to
disable the alert on certain days. Maybe the alert firing should depend on more
than one input. For all of these situations, it is best to use the alert in
conjunction with a `Template Binary Sensor`. The following example does that.

{% raw %}
{% highlight yaml %}
binary_sensor:
  - platform: template
    sensors:
      motion_battery_low:
        value_template: "{{ state_attr('sensor.motion', 'battery') < 15 }}"
        friendly_name: 'Motion battery is low'

alert:
  motion_battery:
    name: Motion Battery is Low
    entity_id: binary_sensor.motion_battery_low
    repeat: 30
    notifiers:
      - ryans_phone
      - kristens_phone
{% endhighlight %}
{% endraw %}

This example will begin firing as soon as the entity `sensor.motion`'s `battery`
attribute falls below 15. It will continue to fire until the battery attribute
raises above 15 or the alert is acknowledged on the frontend.

### Dynamic Notification Delay Times

It may be desirable to have the delays between alert notifications dynamically
change as the alert continues to fire. This can be done by setting the `repeat`
configuration key to a list of numbers rather than a single number.
Altering the first example would look like the following.

{% highlight yaml %}
# Example configuration.yaml entry
alert:
  garage_door:
    name: Garage is open
    entity_id: input_boolean.garage_door
    state: 'on'   # Optional, 'on' is the default value
    repeat:
      - 15
      - 30
      - 60
    can_acknowledge: true  # Optional, default is true
    skip_first: true  # Optional, false is the default
    notifiers:
      - ryans_phone
      - kristens_phone
{% endhighlight %}

Now the first message will be sent after a 15 minute delay, the second will be
sent 30 minutes after that, and a 60 minute delay will fall between every
following notification.
For example, if the garage door opens at 2:00, a notification will be
sent at 2:15, 2:45, 3:45, 4:45, etc., continuing every 60 minutes.

### Message Templates

It may be desirable to have the alert notifications include information
about the state of the entity. [Templates][template]
can be used in the message or name of the alert to make it more relevant.
The following will show for a plant how to include the problem `attribute`
of the entity.

{% raw %}
{% highlight yaml %}
# Example configuration.yaml entry
alert:
  office_plant:
    name: Plant in office needs help
    entity_id: plant.plant_office
    state: 'problem'
    repeat: 30
    can_acknowledge: true
    skip_first: true
    message: "Plant {{ states.plant.plant_office }} needs help ({{ state_attr('plant.plant_office', 'problem') }})"
    done_message: Plant in office is fine
    notifiers:
      - ryans_phone
      - kristens_phone
{% endhighlight %}
{% endraw %}

The resulting message could be `Plant Officeplant needs help (moisture low)`.

### Additional parameters for notifiers 

Some notifiers support more parameters (e.g., to set text color or action
  buttons). These can be supplied via the `data` parameter:

{% highlight yaml %}
# Example configuration.yaml entry
alert:
  garage_door:
    name: Garage is open
    entity_id: input_boolean.garage_door
    state: 'on'   # Optional, 'on' is the default value
    repeat:
      - 15
      - 30
      - 60
    can_acknowledge: true  # Optional, default is true
    skip_first: true  # Optional, false is the default
    data:
      inline_keyboard:
        - 'Close garage:/close_garage, Acknowledge:/garage_acknowledge'
    notifiers:
      - frank_telegram
{% endhighlight %}
This particular example relies on the `inline_keyboard` functionality of
Telegram, where the user is presented with buttons to execute certain actions.

[template]: /docs/configuration/templating/
