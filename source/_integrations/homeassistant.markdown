---
title: Open Peer Power Core Integration
description: Description of the openpeerpower integration.
ha_release: 0.0
ha_quality_scale: internal
ha_codeowners:
  - '@open-peer-power/core'
ha_domain: openpeerpower
---

The Open Peer Power integration provides generic implementations like the generic `openpeerpower.turn_on`.

## Services

The `openpeerpower` integration provides services for controlling Open Peer Power itself, as well as generic controls for any entity.

### Service `openpeerpower.check_config`

Reads the configuration files and checks them for correctness, but **does not** load them into Open Peer Power. Creates a persistent notification and log entry if errors are found.

### Service `openpeerpower.reload_core_config`

Loads the main configuration file (`configuration.yaml`) and all linked files. Once loaded the new configuration is applied.

### Service `openpeerpower.restart`

Restarts the Open Peer Power instance (also reloading the configuration on start).

### Service `openpeerpower.stop`

Stops the Open Peer Power instance. Open Peer Power must be restarted from the Host device to run again.

### Service `openpeerpower.set_location`

Update the location of the Open Peer Power default zone (usually "Home").

| Service data attribute    | Optional | Description                                           |
|---------------------------|----------|-------------------------------------------------------|
| `latitude`                |       no | Latitude of your location.                            |
| `longitude`               |       no | Longitude of your location.                           |

#### Example

{% highlight yaml %}
action:
  service: openpeerpower.set_location
  data:
    latitude: 32.87336
    longitude: 117.22743
{% endhighlight %}

### Service `openpeerpower.toggle` 

Generic service to toggle devices on/off under any domain. Same usage as the light.turn_on, switch.turn_on, etc. services.

| Service data attribute    | Optional | Description                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |       yes | The entity_id of the device to toggle on/off.         |

#### Example

{% highlight yaml %}
action:
  service: openpeerpower.toggle
  data:
    entity_id: light.living_room
{% endhighlight %}

#### Service `openpeerpower.turn_on` 

Generic service to turn devices on under any domain. Same usage as the light.turn_on, switch.turn_on, etc. services.

| Service data attribute    | Optional | Description                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |       yes | The entity_id of the device to turn on.               |

#### Example

{% highlight yaml %}
action:
  service: openpeerpower.turn_on
  data:
    entity_id: light.living_room
{% endhighlight %}

### Service `openpeerpower.turn_off` 

Generic service to turn devices off under any domain. Same usage as the light.turn_on, switch.turn_on, etc. services.

| Service data attribute    | Optional | Description                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |       yes | The entity_id of the device to turn off.              |

#### Example

{% highlight yaml %}
action:
  service: openpeerpower.turn_off
  data:
    entity_id: light.living_room
{% endhighlight %}

### Service `openpeerpower.update_entity` 

Force one or more entities to update its data rather than wait for the next scheduled update.

| Service data attribute    | Optional | Description                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |       no | One or multiple entity_ids to update. It can be a list.  |

#### Example

{% highlight yaml %}
action:
  service: openpeerpower.update_entity
  data:
    entity_id:
    - light.living_room
    - switch.coffe_pot
{% endhighlight %}
