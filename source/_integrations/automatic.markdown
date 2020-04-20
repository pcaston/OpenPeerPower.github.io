---
title: Automatic
description: Instructions for how to integrate Automatic ODB readers into Open Peer Power.
logo: automatic.png
ha_category:
  - Car
ha_release: 0.28
ha_iot_class: Cloud Push
ha_codeowners:
  - '@armills'
ha_domain: automatic
excerpt: none
---

The `automatic` device tracker platform offers presence detection by retrieving your car's information from the [Automatic](https://automatic.com/) cloud service.

## Setup

To use Automatic with Open Peer Power, first you must [create a free development account](https://developer.automatic.com/). Automatic will generate a Client ID and Secret for you to use in your Open Peer Power configuration. You will need to update your Event Delivery preferences to ensure Open Peer Power can receive updates. On the developer page, under App Settings / Event Delivery, select "Websocket" for Event Delivery Preference. Next, specify the OAuth Redirect URL in the developer page. This should be configured to `<home-assistant-url>/api/automatic/callback`. (Example: `http://homeassistant.local:8123/api/automatic/callback`) Note that this URL only needs to be accessible from the browser you use to perform the authentication.

Open Peer Power can also take advantage of `scope:current_location` if available. This will allow Open Peer Power to receive periodic location updates during a trip. In order to use this functionality, you must request the scope for your application from Automatic. Once `scope:current_location` is available, change `current_location` to `true` in your `configuration.yaml`.

## Configuration

Once your developer account is created, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
device_tracker:
  - platform: automatic
    client_id: 1234567
    secret: 0987654321
    devices:
      - 2007 Honda Element
      - 2004 Subaru Impreza
{% endhighlight %}

<div class='note'>
  
The device name must be the name given by Automatic automatically. This is typically the model year, make and model. This is *not* the nickname you have to give the vehicle in the `vehicles` section of the app's settings.
  
</div>  

Open Peer Power will also fire events when an update is received from Automatic. These can be used to trigger automations, as shown in the example below. A list of available event types can be found in the [Automatic Real-Time Events documentation](https://developer.automatic.com/api-reference/#real-time-events).

{% highlight yaml %}
# Example automatic event automation
automation:
  - trigger:
      - platform: event
        event_type: automatic_update
        event_data:
          type: "ignition:on"
          vehicle:
            id: "C_1234567890abcdefc"
    action:
      - service: light.turn_off
{% endhighlight %}

<div class='note'>
You can obtain the correct ID for your vehicle from your known_devices.yaml file. Be sure to lower-case any letters contained in your vehicle's ID when using it in an automation trigger.
</div>

See the [device tracker integration page](/integrations/device_tracker/) for instructions how to configure the cars to be tracked.
