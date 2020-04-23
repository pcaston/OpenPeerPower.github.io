---
title: History Stats
description: Instructions about how to integrate historical statistics into Open Peer Power.
ha_category:
  - Utility
ha_iot_class: Local Polling
ha_release: 0.39
ha_quality_scale: internal
ha_domain: history_stats
excerpt: none
---

The `history_stats` sensor platform provides quick statistics about another integration or platforms, using data from the [history](/integrations/history/).

It can track how long the integration has been in a specific state, in a custom time period.

Examples of what you can track:

- How long you were at home this week
- How long the lights were ON yesterday
- How long you watched TV today

## Configuration

To enable the history statistics sensor, add the following lines to your `configuration.yaml`:

{% raw %}
{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  - platform: history_stats
    name: Lamp ON today
    entity_id: light.my_lamp
    state: 'on'
    type: time
    start: '{{ now().replace(hour=0, minute=0, second=0) }}'
    end: '{{ now() }}'
{% endhighlight %}
{% endraw %}

<div class='note'>

  You have to provide **exactly 2** of `start`, `end` and `duration`.
<br/>
  You can use [template extensions](/topics/templating/#open-peer-power-template-extensions) such as `now()` or `as_timestamp()` to handle dynamic dates, as shown in the examples below.

</div>

## Sensor type

Depending on the sensor type you choose, the `history_stats` integration can show different values:

- **time**: The default value, which is the tracked time, in hours
- **ratio**: The tracked time divided by the length of your period, as a percentage
- **count**: How many times the integration you track was changed to the state you track

## Time periods

The `history_stats` integration will execute a measure within a precise time period. You should always provide 2 of the following :
- When the period starts (`start` variable)
- When the period ends (`end` variable)
- How long is the period (`duration` variable)

As `start` and `end` variables can be either datetimes or timestamps, you can configure almost any period you want.

### Duration

The duration variable is used when the time period is fixed. Different syntaxes for the duration are supported, as shown below.

{% highlight yaml %}
# 6 hours
duration: 06:00
{% endhighlight %}

{% highlight yaml %}
# 1 minute, 30 seconds
duration: 00:01:30
{% endhighlight %}

{% highlight yaml %}
# 2 hours and 30 minutes
duration:
  # supports seconds, minutes, hours, days
  hours: 2
  minutes: 30
{% endhighlight %}

<div class='note'>

  If the duration exceeds the number of days of history stored by the `recorder` component (`purge_keep_days`), the history statistics sensor will not have all the information it needs to look at the entire duration. For example, if `purge_keep_days` is set to 7, a history statistics sensor with a duration of 30 days will only report a value based on the last 7 days of history.

</div>

### Examples

Here are some examples of periods you could work with, and what to write in your `configuration.yaml`:

**Today**: starts at 00:00 of the current day and ends right now.

{% raw %}
{% highlight yaml %}
    start: '{{ now().replace(hour=0, minute=0, second=0) }}'
    end: '{{ now() }}'
{% endhighlight %}
{% endraw %}

**Yesterday**: ends today at 00:00, lasts 24 hours.

{% raw %}
{% highlight yaml %}
    end: '{{ now().replace(hour=0, minute=0, second=0) }}'
    duration:
      hours: 24
{% endhighlight %}
{% endraw %}

**This morning (6AM - 11AM)**: starts today at 6, lasts 5 hours.

{% raw %}
{% highlight yaml %}
    start: '{{ now().replace(hour=6, minute=0, second=0) }}'
    duration:
      hours: 5
{% endhighlight %}
{% endraw %}

**Current week**: starts last Monday at 00:00, ends right now.

Here, last Monday is _today_ as a timestamp, minus 86400 times the current weekday (86400 is the number of seconds in one day, the weekday is 0 on Monday, 6 on Sunday).

{% raw %}
{% highlight yaml %}
    start: '{{ as_timestamp( now().replace(hour=0, minute=0, second=0) ) - now().weekday() * 86400 }}'
    end: '{{ now() }}'
{% endhighlight %}
{% endraw %}

**Last 30 days**: ends today at 00:00, lasts 30 days. Easy one.

{% raw %}
{% highlight yaml %}
    end: '{{ now().replace(hour=0, minute=0, second=0) }}'
    duration:
      days: 30
{% endhighlight %}
{% endraw %}

**All your history** starts at timestamp = 0, and ends right now.

{% raw %}
{% highlight yaml %}
    start: '{{ 0 }}'
    end: '{{ now() }}'
{% endhighlight %}
{% endraw %}

<div class='note'>

  The `/developer-tools/template` page of your Open Peer Power UI can help you check if the values for `start`, `end` or `duration` are correct. If you want to check if your period is right, just click on your component, the `from` and `to` attributes will show the start and end of the period, nicely formatted.

</div>
