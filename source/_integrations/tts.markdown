---
title: Text-to-Speech (TTS)
description: Instructions on how to set up Text-to-Speech (TTS) with Open Peer Power.
ha_release: 0.35
ha_codeowners:
  - '@pvizeli'
ha_domain: tts
excerpt: none
---

Text-to-Speech (TTS) enables Open Peer Power to speak to you.

## Configuring a `tts` platform

To get started, add the following lines to your `configuration.yaml` (example for Google):

{% highlight yaml %}
# Example configuration.yaml entry for Google TTS service
tts:
  - platform: google_translate
{% endhighlight %}

<div class='note'>

Depending on your setup, you might need to set a base URL (`base_url`) inside the [HTTP component](/integrations/http/) or in the parameters of this component.

</div>

The following optional parameters can be used with any platform. However, the TTS integration will only look for global settings under the configuration of the first configured platform:

The extended example from above would look like the following sample:

{% highlight yaml %}
# Example configuration.yaml entry for Google Translate TTS service
tts:
  - platform: google_translate
    cache: true
    cache_dir: /tmp/tts
    time_memory: 300
    base_url: http://192.168.0.10:8123
    service_name: google_say
{% endhighlight %}

<div class='note'>

In the above example, `base_url` is custom to this particular TTS platform configuration. It is not suggesting that you use the `base_url` that you have set for your core Open Peer Power configuration. The reason you might need to do this is outlined in the next section.

</div>

## When do you need to set `base_url` here?

The general answer is "whenever the global `base_url` set in [HTTP component](/integrations/http/) is not adequate to allow the `say` service to run". The `say` service operates by generating a media file that contains the speech corresponding to the text passed to the service. Then the `say` service sends a message to the media device with a URL pointing to the file. The device fetches the media file at the URL and plays the media. Some combinations of a media device, network configuration and Open Peer Power configuration can make it so that the device cannot fetch the media file.

The following sections describe some of the problems encountered with media devices.

### Self-signed certificates

This problem occurs when your Open Peer Power instance is configured to be accessed through SSL, and you are using a self-signed certificate.

The `tts` service will send an `https://` URL to the media device, which will check the certificate, and reject it. So it won't play your file. If you could make the device accept your certificate, it would play the file. However, many media devices do not allow changing settings to accept self-signed certificates. Ultimately, your option may be to serve files to the device as `http://` rather than `https://`. To do this, you *could* change the `base_url` setting in [HTTP component](/integrations/http/), but that would turn off SSL for all services that use `base_url`. Instead, setting a `base_url` for the `tts` service allows turning off SSL only for this component.

### Google cast devices

The Google cast devices (Google Home, Chromecast, etc.) present the following problems:

* They [reject self-signed certificates](#self-signed-certificates).

* They do not work with URLs that contain hostnames established by local naming means. Let's say your Open Peer Power instance is running on a machine made known locally as `ha`. All your machines on your local network are able to access it as `ha`. However, try as you may, your cast device won't download the media files from your `ha` machine. That's because your cast device ignores your local naming setup. In this example, the `say` service creates a URL like `http://ha/path/to/media.mp3` (or `https://...` if you are using SSL). Setting a `base_url` that contains the IP address of your server works around this issue. By using an IP address, the cast device does not have to resolve the hostname.

* An alternative way to force Google cast devices to use internal DNS is to block them from accessing Google DNS at the firewall/router level. This would be useful in the case, for example, where your internal IP of Open Peer Power is a private IP and you have your internal DNS server (quite often a split-brain DNS scenario). This method works on both Google Home Mini and Google Chromecasts.

## Service say

The `say` service support `language` and on some platforms also `options` for set, i.e., *voice, motion, speed, etc*. The text for speech is set with `message`. Since release 0.92, service name can be defined in configuration `service_name` option.

Say to all `media_player` device entities:

{% highlight yaml %}
# Replace google_translate_say with <platform>_say when you use a different platform.
service: tts.google_translate_say
entity_id: "all"
data:
  message: 'May the Force be with you.'
{% endhighlight %}

Say to the `media_player.floor` device entity:

{% highlight yaml %}
service: tts.google_translate_say
entity_id: media_player.floor
data:
  message: 'May the Force be with you.'
{% endhighlight %}

Say to the `media_player.floor` device entity in French:

{% highlight yaml %}
service: tts.google_translate_say
entity_id: media_player.floor
data:
  message: 'Que la force soit avec toi.'
  language: 'fr'
{% endhighlight %}

With a template:

{% highlight yaml %}
service: tts.google_translate_say
data_template:
  message: "Temperature is {% raw %}{{states('sensor.temperature')}}{% endraw %}."
  cache: false
{% endhighlight %}

## Cache

The integration has two caches. Both caches can be controlled with the `cache` option in the platform configuration or the service call `say`. A long time cache will be located on the file system. The in-memory cache for fast responses to media players will be auto-cleaned after a short period.

## REST API

### POST `/api/tts_get_url`

Returns a URL to the generated TTS file. Platform and message are required.

{% highlight json %}
{
    "platform": "amazon_polly",
    "message": "I am speaking now"
}
{% endhighlight %}

The return code is 200 if the file is generated. The message body will contain a JSON object with the URL.

{% highlight json %}
{
    "url": "http://127.0.0.1:8123/api/tts_proxy/265944c108cbb00b2a621be5930513e03a0bb2cd_en_-_demo.mp3"
}
{% endhighlight %}

Sample `curl` command:

{% highlight bash %}
$ curl -X POST -H "x-ha-access: YOUR_PASSWORD" \
       -H "Content-Type: application/json" \
       -d '{"message": "I am speaking now", "platform": "amazon_polly"}' \
       http://localhost:8123/api/tts_get_url
{% endhighlight %}
