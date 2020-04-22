---
title: Input Datetime
description: Instructions on how to integrate the Input Datetime integration into Open Peer Power.
ha_category:
  - Automation
ha_release: 0.55
ha_quality_scale: internal
ha_codeowners:
  - '@open-peer-power/core'
ha_domain: input_datetime
excerpt: none
---

The `input_datetime` integration allows the user to define date and time values
that can be controlled via the frontend and can be used within automations and
templates.

To add three datetime inputs to your installation,
one with both date and time, and one with date or time each,
add the following lines to your `configuration.yaml`:

{% highlight yaml %}
# Example configuration.yaml entry
input_datetime:
  both_date_and_time:
    name: Input with both date and time
    has_date: true
    has_time: true
  only_date:
    name: Input with only date
    has_date: true
    has_time: false
  only_time:
    name: Input with only time
    has_date: false
    has_time: true
{% endhighlight %}

### Attributes

A datetime input entity's state exports several attributes that can be useful in
automations and templates.

| Attribute | Description |
| ----- | ----- |
| `has_time` | `true` if this entity has a time.
| `has_date` | `true` if this entity has a date.
| `year`<br>`month`<br>`day` | The year, month and day of the date.<br>(only available if `has_date: true`)
| `timestamp` | A timestamp representing the time held in the input.<br>(only available if `has_time: true`)

### Restore State

If you set a valid value for `initial` this integration will start with state set to that value. Otherwise, it will restore the state it had prior to Open Peer Power stopping.

### Services

Available service: `input_datetime.set_datetime` and `input_datetime.reload`.

Service data attribute | Format String | Description
-|-|-
`date` | `%Y-%m-%d` | This can be used to dynamically set the date.
`time` | `%H:%M:%S` | This can be used to dynamically set the time.
`datetime` | `%Y-%m-%d %H:%M:%S` | This can be used to dynamically set both the date & time.

To set both the date and time in the same call, use `date` and `time` together, or use `datetime` by itself. Using `datetime` has the advantage that both can be set using one template.

`input_dateteime.reload` service allows one to reload `input_datetime`'s configuration without restarting Open Peer Power itself.

## Automation Examples

The following example shows the usage of the `input_datetime` as a trigger in an
automation (note that you will need a
[time sensor](/integrations/time_date) elsewhere in your configuration):

{% raw %}
{% highlight yaml %}
# Example configuration.yaml entry
# Turns on bedroom light at the time specified.
automation:
  trigger:
    platform: template
    value_template: "{{ states('sensor.time') == (state_attr('input_datetime.bedroom_alarm_clock_time', 'timestamp') | int | timestamp_custom('%H:%M', True)) }}"
  action:
    service: light.turn_on
    entity_id: light.bedroom
{% endhighlight %}
{% endraw %}

To dynamically set the `input_datetime` you can call
`input_datetime.set_datetime`. The values for `date` and `time` must be in a certain format for the call to be successful. (See service description above.)
If you have a `datetime` object you can use its `strftime` method. Of if you have a timestamp you can use the `timestamp_custom` filter.
The following example shows the different methods in an automation rule:

{% raw %}
{% highlight yaml %}
# Example configuration.yaml entry
# Shows different ways to set input_datetime when an input_boolean is turned on
automation:
  trigger:
    platform: state
    entity_id: input_boolean.example
    to: 'on'
  action:
  # Sets time to '05:30:00
  - service: input_datetime.set_datetime
    entity_id: input_datetime.bedroom_alarm_clock_time
    data:
      time: '05:30:00'
   # Sets time to time from datetime object (current time in this example)
  - service: input_datetime.set_datetime
    entity_id: input_datetime.another_time
    data_template:
      time: "{{ now().strftime('%H:%M:%S') }}"
  # Sets date to date from timestamp (current date in this example)
  - service: input_datetime.set_datetime
    entity_id: input_datetime.another_date
    data_template:
      date: "{{ as_timestamp(now())|timestamp_custom('%Y-%m-%d') }}"
  # Sets date and time to date and time from datetime object (current date and time in this example)
  - service: input_datetime.set_datetime
    entity_id: input_datetime.date_and_time
    data_template:
      datetime: "{{ now().strftime('%Y-%m-%d %H:%M:%S') }}"
  # Sets date and time to date and time from timestamp (current date and time in this example)
  - service: input_datetime.set_datetime
    data_template:
      entity_id: input_datetime.date_and_time
      date: >
        {{ now().timestamp() | timestamp_custom("%Y-%m-%d", true) }}
      time: >
        {{ now().timestamp() | timestamp_custom("%H:%M:%S", true) }}
{% endhighlight %}
{% endraw %}
