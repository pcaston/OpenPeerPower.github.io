---
title: Stream
description: Instructions on how to integrate live streams within Open Peer Power.
ha_category:
  - Other
ha_release: '0.90'
ha_iot_class: Local Push
ha_quality_scale: internal
ha_codeowners:
  - '@hunterjm'
ha_domain: stream
---

The `stream` integration provides a way to proxy live streams through Open Peer Power. The integration currently only supports proxying H.264 source streams to the HLS format and requires at least FFmpeg >= 3.2.

## Configuration

To enable this component, add the following lines to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
stream:
{% endhighlight %}

### Services

Once loaded, the `stream` platform will expose services that can be called to perform various actions.

#### Service `record`

Make a `.mp4` recording from a provided stream.  While this service can be called directly, it is used internally by the [`camera.record`](/integrations/camera#service-record) service.

Both `duration` and `lookback` options are suggestions, but should be consistent per stream.  The actual length of the recording may vary. It is suggested that you tweak these settings to fit your needs.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `stream_source`        |      no  | The input source for the stream, e.g., `rtsp://my.stream.feed:554`. |
| `filename`             |      no  | The file name string. e.g., `/tmp/my_stream.mp4`. |
| `duration`             |      yes | Target recording length (in seconds). Default: 30 |
| `lookback`             |      yes | Target lookback period (in seconds) to include in addition to duration.  Only available if there is currently an active HLS stream for `stream_source`. Default: 0 |

The path part of `filename` must be an entry in the `whitelist_external_dirs` in your [`openpeerpower:`](/docs/configuration/basic/) section of your `configuration.yaml` file.

For example, the following action in an automation would take a recording from `rtsp://my.stream.feed:554` and save it to `/config/www`.

{% highlight yaml %}
action:
  service: camera.record
  data:
    entity_id: camera.quintal
    filename: '/config/www/my_stream.mp4'
    duration: 30
{% endhighlight %}

## Streaming in Lovelace

As of Open Peer Power version 0.92 you can now live-stream a camera feed directly in lovelace.
To do this add either [picture-entity](/lovelace/picture-entity/), [picture-glance](/lovelace/picture-glance/) or [picture-elements](/lovelace/picture-elements/), set `camera_image` to a stream-ready camera entity and set `camera_view` to `live` in one of your Lovelace views.

## Troubleshooting

Some users on manual installs may see the following error in their logs after restarting:

{% highlight text %}
2019-03-12 08:49:59 ERROR (SyncWorker_5) [openpeerpower.util.package] Unable to install package av==6.1.2: Command "/home/pi/open-peer-power/bin/python3 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-udfl2b3t/av/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-ftn5zmh2/install-record.txt --single-version-externally-managed --compile --install-headers /home/pi/open-peer-power/include/site/python3.6/av" failed with error code 1 in /tmp/pip-install-udfl2b3t/av/
2019-03-12 08:49:59 ERROR (MainThread) [openpeerpower.requirements] Not initializing stream because could not install requirement av==6.1.2
2019-03-12 08:49:59 ERROR (MainThread) [openpeerpower.setup] Setup failed for stream: Could not install all requirements.
{% endhighlight %}

If you see this error you can solve it by running the following commands and restarting Open Peer Power (commands do not need to be ran as the `openpeerpower` user):

{% highlight text %}
sudo apt-get install -y python-dev pkg-config libavformat-dev libavcodec-dev libavdevice-dev libavutil-dev libswscale-dev libavresample-dev libavfilter-dev
{% endhighlight %}
