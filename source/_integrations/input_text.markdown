---
title: Input Text
description: Instructions on how to integrate the Input Text integration into Open Peer Power.
ha_category:
  - Automation
ha_release: 0.53
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
ha_domain: input_text
excerpt: none
---

The `input_text` integration allows the user to define values that can be controlled via the frontend and can be used within conditions of automation. Changes to the value stored in the text box generate state events. These state events can be utilized as `automation` triggers as well. It can also be configured in password mode (obscured text).

{% highlight yaml %}
# Example configuration.yaml entries
input_text:
  text1:
    name: Text 1
    initial: Some Text
  text2:
    name: Text 2
    min: 8
    max: 40
  text3:
    name: Text 3
    pattern: '[a-fA-F0-9]*'
  text4:
    name: Text 4
    mode: password
{% endhighlight %}

### Services

This integration provides a service to modify the state of the `input_text` and a service to reload the `input_text` configuration without restarting Open Peer Power itself.

| Service | Data | Description |
| ------- | ---- | ----------- |
| `set_value` | `value`<br>`entity_id(s)` | Set the value for specific `input_text` entities.
| `reload` | | Reload `input_text` configuration |

### Restore State

If you set a valid value for `initial` this integration will start with state set to that value. Otherwise, it will restore the state it had prior to Open Peer Power stopping.

### Scenes

To set the state of the input_text in a [Scene](/integrations/scene/):

{% highlight yaml %}
# Example configuration.yaml entry
scene:
  - name: Example1
    entities:
      input_text.example: Hello!
{% endhighlight %}

## Automation Examples

Here's an example using `input_text` in an action in an automation.

{% raw %}
{% highlight yaml %}
# Example configuration.yaml entry using 'input_text' in an action in an automation
input_select:
  scene_bedroom:
    name: Scene
    options:
      - Select
      - Concentrate
      - Energize
      - Reading
      - Relax
      - 'OFF'
    initial: 'Select'
input_text:
  bedroom:
    name: Brightness
    
automation:
  - alias: Bedroom Light - Custom
    trigger:
      platform: state
      entity_id: input_select.scene_bedroom
    action:
      - service: input_text.set_value
        # Again, note the use of 'data_template:' rather than the normal 'data:' if you weren't using an input variable.
        data_template:
          entity_id: input_text.bedroom
          value: "{{ states('input_select.scene_bedroom') }}"
{% endhighlight %}
{% endraw %}
