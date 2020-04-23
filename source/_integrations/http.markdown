---
title: HTTP
description: Offers a web framework to serve files.
ha_category:
  - Other
  - Binary Sensor
  - Sensor
ha_release: pre 0.7
ha_iot_class: Local Push
ha_quality_scale: internal
ha_codeowners:
  - '@open-peer-power/core'
ha_domain: http
excerpt: none
---

The `http` integration serves all files and data required for the Open Peer Power frontend. You only need to add this to your configuration file if you want to change any of the default settings.

There is currently support for the following device types within Open Peer Power:

- [Binary Sensor](#binary-sensor)
- [Sensor](#sensor)

<div class='note'>

The option option `server_host` should only be used on a Open Peer Power Core installation!

</div>

{% highlight yaml %}
# Example configuration.yaml entry
http:
{% endhighlight %}

<div class='note'>

Configuring trusted_networks via the `http` integration will be deprecated and moved to `auth_providers` instead. For instructions, see <a href="/docs/authentication/providers/#trusted-networks">trusted networks</a>. In Open Peer Power 0.89.0 and 0.89.1, you need place the trusted network under both `http` and `auth_providers` if you still want to use trusted networks features. You can remove it from `http` section starting from 0.89.2.

</div>

The sample below shows a configuration entry with possible values:

{% highlight yaml %}
# Example configuration.yaml entry
http:
  server_port: 12345
  ssl_certificate: /etc/letsencrypt/live/hass.example.com/fullchain.pem
  ssl_key: /etc/letsencrypt/live/hass.example.com/privkey.pem
  cors_allowed_origins:
    - https://google.com
    - https://www.openpeerpower.io
  use_x_forwarded_for: true
  trusted_proxies:
    - 10.0.0.200
  ip_ban_enabled: true
  login_attempts_threshold: 5
{% endhighlight %}

The [Set up encryption using Let's Encrypt](/blog/2015/12/13/setup-encryption-using-lets-encrypt/) blog post gives you details about the encryption of your traffic using free certificates from [Let's Encrypt](https://letsencrypt.org/).

Or use a self signed certificate following the instructions here [Self-signed certificate for SSL/TLS](/docs/ecosystem/certificates/tls_self_signed_certificate/).

## HTTP sensors

To use those kind of [sensors](#sensor) or [binary sensors](#binary-sensor) in your installation no configuration in Open Peer Power is needed. All configuration is done on the devices themselves. This means that you must be able to edit the target URL or endpoint and the payload. The entity will be created after the first message has arrived.

## IP filtering and banning

If you want to apply additional IP filtering, and automatically ban brute force attempts, set `ip_ban_enabled` to `true` and the maximum number of attempts. After the first ban, an `ip_bans.yaml` file will be created in the root configuration folder. It will have the banned IP address and time in UTC when it was added:

{% highlight yaml %}
127.0.0.1:
  banned_at: '2016-11-16T19:20:03'
{% endhighlight %}

After a ban is added a Persistent Notification is populated to the Open Peer Power frontend.

<div class='note warning'>

Please note, that sources from `trusted_networks` won't be banned automatically.

</div>

## Hosting files

If you want to use Open Peer Power to host or serve static files then create a directory called `www` under the configuration path (`/config`). The static files in `www/` can be accessed by the following URL `http://your.domain:8123/local/`, for example `audio.mp3` would be accessed as `http://your.domain:8123/local/audio.mp3`.

<div class='note'>

  If you've had to create the `www/` folder for the first time, you'll need to restart Open Peer Power.

</div>

<div class='note warning'>

  Files served from the `www`/`local` folder, aren't protected by the Open Peer Power authentication. Files stored in this folder, if the URL is known, can be accessed by anybody without authentication.

</div>

## Binary Sensor

The HTTP binary sensor is dynamically created with the first request that is made to its URL. You don't have to define it in the configuration first.

The sensor will then exist as long as Open Peer Power is running. After a restart of Open Peer Power the sensor will be gone until it is triggered again.

The URL for a binary sensor looks like the example below:

{% highlight bash %}
http://IP_ADDRESS:8123/api/states/binary_sensor.DEVICE_NAME
{% endhighlight %}

<div class='note'>
You should choose a unique device name (DEVICE_NAME) to avoid clashes with other devices.
</div>

The JSON payload must contain the new state and can have a friendly name. The friendly name is used in the frontend to name the sensor.

{% highlight json %}
{"state": "on", "attributes": {"friendly_name": "Radio"}}
{% endhighlight %}

For a quick test `curl` can be useful to "simulate" a device.

{% highlight bash %}
$ curl -X POST -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"state": "off", "attributes": {"friendly_name": "Radio"}}' \
    http://localhost:8123/api/states/binary_sensor.radio
{% endhighlight %}

{% highlight bash %}
$ curl -X GET -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
       -H "Content-Type: application/json" \
       http://localhost:8123/api/states/binary_sensor.radio
{
    "attributes": {
        "friendly_name": "Radio"
    },
    "entity_id": "binary_sensor.radio",
    "last_changed": "16:45:51 05-02-2016",
    "last_updated": "16:45:51 05-02-2016",
    "state": "off"
}
{% endhighlight %}

### Examples

In this section you'll find some real-life examples of how to use this sensor, besides `curl`, which was shown earlier.

#### Using Python request module

{% highlight python %}
response = requests.post(
    "http://localhost:8123/api/states/binary_sensor.radio",
    headers={
        "Authorization": "Bearer LONG_LIVED_ACCESS_TOKEN",
        "content-type": "application/json",
    },
    data=json.dumps({"state": "on", "attributes": {"friendly_name": "Radio"}}),
)
print(response.text)
{% endhighlight %}

#### Using `httpie`

[`httpie`](https://github.com/jkbrzt/httpie) is a user-friendly CLI HTTP client.

{% highlight bash %}
$ http -v POST http://localhost:8123/api/states/binary_sensor.radio \
      'Authorization:Bearer LONG_LIVED_ACCESS_TOKEN' content-type:application/json state=off \
      attributes:='{"friendly_name": "Radio"}'
{% endhighlight %}

## Sensor

The HTTP sensor is dynamically created with the first request that is made to its URL. You don't have to define it in the configuration first.

The sensor will then exist as long as Open Peer Power is running. After a restart of Open Peer Power the sensor will be gone until it is triggered again.

The URL for a sensor looks like the example below:

{% highlight bash %}
http://IP_ADDRESS:8123/api/states/sensor.DEVICE_NAME
{% endhighlight %}

<div class='note'>
You should choose a unique device name (DEVICE_NAME) to avoid clashes with other devices.
</div>

 The JSON payload must contain the new state and should include the unit of measurement and a friendly name. The friendly name is used in the frontend to name the sensor.

{% highlight json %}
{"state": "20", "attributes": {"unit_of_measurement": "°C", "friendly_name": "Bathroom Temperature"}}
{% endhighlight %}

For a quick test, `curl` can be useful to "simulate" a device.

{% highlight bash %}
$ curl -X POST -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
       -H "Content-Type: application/json" \
       -d '{"state": "20", "attributes": {"unit_of_measurement": "°C", "friendly_name": "Bathroom Temp"}}' \
       http://localhost:8123/api/states/sensor.bathroom_temperature
{% endhighlight %}

{% highlight bash %}
$ curl -X GET -H "Authorization: Bearer LONG_LIVED_ACCESS_TOKEN" \
       -H "Content-Type: application/json" \
       http://localhost:8123/api/states/sensor.bathroom_temperature
{
    "attributes": {
        "friendly_name": "Bathroom Temp",
        "unit_of_measurement": "\u00b0C"
    },
    "entity_id": "sensor.bathroom_temperature",
    "last_changed": "09:46:17 06-02-2016",
    "last_updated": "09:48:46 06-02-2016",
    "state": "20"
}
{% endhighlight %}

For more examples please visit the [HTTP Binary Sensor](#examples) page.
