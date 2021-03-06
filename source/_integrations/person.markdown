---
title: Person
description: Instructions on how to set up people tracking within Open Peer Power.
ha_category:
  - Presence Detection
ha_release: 0.88
ha_quality_scale: internal
ha_domain: person
excerpt: none
---

The person integration allows connecting [device tracker](/integrations/device_tracker/) entities to one or more person entities. The state updates of a connected device tracker will set the state of the person. When multiple device trackers are used, the state of person will be determined in this order:

1. If there are stationary trackers (non-GPS trackers, i.e., a router or Bluetooth 'device_trackers') presenting the status 'home', the tracker most recently updated will be used.
2. If there are trackers of type 'gps', then the most recently updated tracker will be used.
3. Otherwise, the latest tracker with status 'not_home' will be used.

Let's say, for example, that you have 3 trackers: 'tracker_gps', 'tracker_router' and 'tracker_ble'.

1. You're at home, all 3 devices show status 'home' - status of your Person entity will be 'home' with source 'tracker_router' or 'tracker_ble', whichever was most recently updated.
2. You just left home. 'tracker_gps' shows status 'not_home', but the other two trackers show status 'home' (they may not have yet updated due to their 'consider_home' setting see [device_tracker](/integrations/device_tracker/#configuring-a-device_tracker-platform)). Since the stationary trackers have priority, you are considered 'home'.
3. After some time, both stationary trackers show status 'not_home'. Now your Person entity has status 'not_home' with source 'tracker_gps'.
4. While you are away from home, your Open Peer Power is restarted. Until 'tracker_gps' receives an update, your status will be determined by the stationary trackers, since they will have the most recent update after a restart. Obviously, the status will be 'not_home'.
5. Then you're going into a zone you have defined as 'zone1', 'tracker_gps' sends an update, and now your status is 'zone1' with source 'tracker_gps'.
6. You've returned home and your mobile device has connected to the router, but 'tracker_gps' hasn't updated yet. Your status will be 'home' with source 'tracker_router'.
7. After the 'tracker_gps' update occurs, your status will still be 'home' with source 'tracker_router' or 'tracker_ble', whichever has the most recent update.

TL;DR: When you're at home, your position is determined first by stationary trackers (if any) and then by GPS. When you're outside your home, your position is determined firstly by GPS and then by stationary trackers.

**Hint**: When you use multiple device trackers together, especially stationary and GPS trackers, it's advisable to set `consider_home` for stationary trackers as low as possible see [device_tracker](/integrations/device_tracker/#configuring-a-device_tracker-platform)).

You can manage persons via the UI from the person page inside the configuration panel or via `YAML` in your `configuration.yaml` file.

## Configuring the `person` integration via the Open Peer Power configuration panel

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.openpeerpower.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

{% highlight yaml %}
person:
{% endhighlight %}

## Configuring the `person` integration via YAML

If you prefer YAML, you can also configure your persons via `configuration.yaml`:

{% highlight yaml %}
# Example configuration.yaml entry
person:
  - name: Ada
    id: ada6789
    device_trackers:
      - device_tracker.ada
{% endhighlight %}

An extended example would look like the following sample:

{% highlight yaml %}
# Example configuration.yaml entry
person:
  - name: Ada
    id: ada6789
    device_trackers:
      - device_tracker.ada
  - name: Stacey
    id: stacey12345
    user_id: 12345678912345678912345678912345
    device_trackers:
      - device_tracker.stacey
      - device_tracker.beacon
{% endhighlight %}

If you change the YAML, you can reload it by calling the `person.reload` service.

### Customizing the picture for a person

By following the instructions on the [customizing entities](/docs/configuration/customizing-devices#entity_picture) page, you can customize the picture used for a person entity in the `customize:` section of your configuration. For example:

{% highlight yaml %}
customize:
  person.ada:
    entity_picture: "/local/ada.jpg"
{% endhighlight %}

See the documentation about [hosting files](/integrations/http/#hosting-files) for more information about the `www` folder.
