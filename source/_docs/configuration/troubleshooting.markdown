---
title: "Troubleshooting your configuration"
description: "Common problems with tweaking your configuration and their solutions."
redirect_from: /getting-started/troubleshooting-configuration/
---

It can happen that you run into trouble while configuring Open Peer Power. Perhaps an integration is not showing up or is acting strangely. This page will discuss a few of the most common problems.

Before we dive into common issues, make sure you know where your configuration directory is. Open Peer Power will print out the configuration directory it is using when starting up.

Whenever an integration or configuration option results in a warning, it will be stored in `open-peer-power.log` in the configuration directory. This file is reset on start of Open Peer Power.

## My integration does not show up

When an integration does not show up, many different things can be the case. Before you try any of these steps, make sure to look at the `open-peer-power.log` file and see if there are any errors related to your integration you are trying to set up.

If you have incorrect entries in your configuration files you can use the [`check_config`](/docs/tools/check_config/) script to assist in identifying them: `hass --script check_config`. If you need to provide the path for your configuration you can do this using the `-c` argument like this: `hass --script check_config -c /path/to/your/config/dir`.

### Problems with the configuration

One of the most common problems with Open Peer Power is an invalid `configuration.yaml` or other configuration file.

- With Open Peer Power you can use the [`ha` command](/hassio/commandline/#open-peer-power): `ha core check`.
  - You can test your configuration with Open Peer Power Core using the command line with: `hass --script check_config`.
  - On Docker you can use `docker exec open-peer-power python -m openpeerpower --script check_config --config /config` - where `open-peer-power` is the name of the container.
- The configuration files, including `configuration.yaml` must be UTF-8 encoded. If you see error like `'utf-8' codec can't decode byte`, edit the offending configuration and re-save it as UTF-8.
- You can verify your configuration's YAML structure using [this online YAML parser](http://yaml-online-parser.appspot.com/) or [YAML Lint](http://www.yamllint.com/).
- To learn more about the quirks of YAML, read [YAML IDIOSYNCRASIES](https://docs.saltstack.com/en/latest/topics/troubleshooting/yaml_idiosyncrasies.html) by SaltStack (the examples there are specific to SaltStack, but do explain YAML issues well).

`configuration.yaml` does not allow multiple sections to have the same name. If you want to load multiple platforms for one component, you can append a [number or string](/getting-started/devices/#style-2-list-each-device-separately) to the name or nest them using [this style](/getting-started/devices/#style-1-collect-every-entity-under-the-parent):

{% highlight yaml %}
sensor:
  - platform: forecast
    ...
  - platform: bitcoin
    ...
{% endhighlight %}

Another common problem is that a required configuration setting is missing. If this is the case, the integration will report this to `open-peer-power.log`. You can have a look at [the various integration pages](/integrations/) for instructions on how to setup the components.

See the [logger](/integrations/logger/) integration for instructions on how to define the level of logging you require for specific modules.

If you find any errors or want to expand the documentation, please [let us know](https://github.com/OpenPeerPower/openpeerpower.io/issues).

#### Problems with dependencies

Almost all integrations have external dependencies to communicate with your devices and services. Sometimes Open Peer Power is unable to install the necessary dependencies. If this is the case, it should show up in `open-peer-power.log`.

The first step is trying to restart Open Peer Power and see if the problem persists. If it does, look at the log to see what the error is. If you can't figure it out, please [report it](https://github.com/OpenPeerPower/Open-Peer-Power/issues) so we can investigate what is going on.

#### Problems with integrations

It can happen that some integrations either do not work right away or stop working after Open Peer Power has been running for a while. If this happens to you, please [report it](https://github.com/OpenPeerPower/Open-Peer-Power/issues) so that we can have a look.

#### Multiple files

If you are using multiple files for your setup, make sure that the pointers are correct and the format of the files is valid.

{% highlight yaml %}
light: !include devices/lights.yaml
sensor: !include devices/sensors.yaml
{% endhighlight %}

Contents of `lights.yaml` (notice it does not contain `light:`):

{% highlight yaml %}
- platform: hyperion
  host: 192.168.1.98
  ...
{% endhighlight %}

Contents of `sensors.yaml`:

{% highlight yaml %}
- platform: mqtt
  name: "Room Humidity"
  state_topic: "room/humidity"
- platform: mqtt
  name: "Door Motion"
  state_topic: "door/motion"
  ...
{% endhighlight %}

<div class='note'>
Whenever you report an issue, be aware that we are volunteers who do not have access to every single device in the world nor unlimited time to fix every problem out there.
</div>

### Entity names

The only characters valid in entity names are:

- Lowercase letters
- Numbers
- Underscores

If you create an entity with other characters then Open Peer Power may not generate an error for that entity. However you will find that attempts to use that entity will generate errors (or possibly fail silently).
