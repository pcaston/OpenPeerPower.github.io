---
title: Google Maps
description: Instructions how to use Google Maps Location Sharing to track devices in Open Peer Power.
ha_release: 0.67
ha_category:
  - Presence Detection
ha_iot_class: Cloud Polling
ha_domain: google_maps
excerpt: none
---

The `google_maps` platform allows you to detect presence using the unofficial API of [Google Maps Location Sharing](https://myaccount.google.com/locationsharing).

## Setup

You first need to create an additional Google account and share your location with that account. This integration will use that account to fetch the location of your device(s). 

1. You have to setup sharing through the Google Maps app on your mobile phone. You can find more information [here](https://support.google.com/accounts?p=location_sharing).
2. You need to use the cookies from that account after you have properly authenticated which you can retrieve with either [Export cookies](https://addons.mozilla.org/en-US/firefox/addon/export-cookies-txt/?src=search) for Firefox (make sure that "Prefix HttpOnly cookies" is unchecked) or [cookies.txt](https://chrome.google.com/webstore/detail/cookiestxt/njabckikapfpffapmjgojcnbfjonfjfg?hl=en-US) for Chrome/Chromium.
3. Save the cookie file to your Open Peer Power configuration directory with the following name: `.google_maps_location_sharing.cookies.` followed by the slugified username of the NEW Google account. Make sure to use the `.com` TLD (e.g., maps.google.com), otherwise the cookie won't be able to provide a valid session.
   - For example: If your email address was `location.tracker@gmail.com`, the filename would be: `.google_maps_location_sharing.cookies.location_tracker_gmail_com`.

## Configuration

To integrate Google Maps Location Sharing in Open Peer Power, add the following section to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
device_tracker:
  - platform: google_maps
    username: YOUR_USERNAME
{% endhighlight %}

Once enabled and you have rebooted devices discovered through this integration will be listed in the `known_devices.yaml` file within your configuration directory.

They will be created with indentifiers like `google_maps_<numeric_id>`. To be able to properly track entities you must set the `track` attribute to `true`. 

<div class='note'>
As of release 0.97 Google passwords are no longer required in your configuration. Users coming from earlier releases should only remove the password entry from their configuration file (username is still required) and restart Open Peer Power. The cookie file previously generated should still be valid and will allow the tracker to continue functioning normally until the cookie is invalidated.
</div>
