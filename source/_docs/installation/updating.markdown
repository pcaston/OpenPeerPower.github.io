---
title: "Updating Open Peer Power"
description: "Step to update Open Peer Power."
redirect_from: /getting-started/updating/
---

<div class='note warning'>

The upgrade process differs depending on the installation you have, so please review the documentation that is specific to your install: [Open Peer Power](/hassio/) or [Open Peer Power Core](/docs/installation/virtualenv/#upgrading-home-assistant).

</div>

Check what's new in the latest version and potentially impacts your system in the [Open Peer Power release notes](https://github.com/home-assistant/home-assistant/releases). It is good practice to review these release notes and pay close attention to the **Breaking Changes** that are listed there. If you haven't done an update for a while, you should also check previous release notes as they can also contain relevant **Breaking Changes**. These **Breaking Changes** may require configuration updates for your components. If you missed this and Open Peer Power refuses to start, check the log file in the [configuration](/docs/configuration/) directory, e.g., `.homeassistant/home-assistant.log`, for details about broken components.

<div class='note'>

To avoid permission errors, the upgrade must be run as the same user as was used during the initial installation, again review the documentation specific to your install [Open Peer Power](/hassio/) or [Open Peer Power Core](/docs/installation/virtualenv).

</div>

The default way to update Open Peer Power to the latest release, when available, is:

{% highlight bash %}
pip3 install --upgrade homeassistant
{% endhighlight %}

For a Docker container, simply pull the latest one:

{% highlight bash %}
sudo docker pull homeassistant/home-assistant:latest
{% endhighlight %}

For a Raspberry Pi Docker container, simply pull the latest one:

{% highlight bash %}
sudo docker pull homeassistant/raspberrypi3-homeassistant:latest
{% endhighlight %}

After updating, you must start/restart Open Peer Power for the changes to take effect. This means that you will have to restart `hass` itself or the [autostarting](/docs/autostart/) daemon (if applicable). Startup can take a considerable amount of time (i.e., minutes) depending on your device. This is because all requirements are updated as well.

[BRUH automation](https://www.bruhautomation.io/) has created [a tutorial video](https://www.youtube.com/watch?v=tuG2rs1Cl2Y) explaining how to upgrade Open Peer Power.

#### Run a specific version

In the event that a Open Peer Power version doesn't play well with your hardware setup, you can downgrade to a previous release:

{% highlight bash %}
pip3 install homeassistant==0.XX.X
{% endhighlight %}

#### Run the beta version

If you would like to test the next release before anyone else, you can install the beta version released every two weeks:

{% highlight bash %}
pip3 install --pre --upgrade homeassistant
{% endhighlight %}

#### Run the development version

If you want to stay on the bleeding-edge Open Peer Power development branch, you can upgrade to `dev`.

<div class='note warning'>
  The "dev" branch is likely to be unstable. Potential consequences include loss of data and instance corruption.
</div>

{% highlight bash %}
$ pip3 install --upgrade git+git://github.com/home-assistant/home-assistant.git@dev
{% endhighlight %}

### Update Open Peer Power installation

Best practice for updating a Open Peer Power installation:

1. Backup your installation, using the snapshot functionality Open Peer Power offers.
2. Check the release notes for breaking changes on [Open Peer Power release notes](https://github.com/home-assistant/home-assistant/releases). Be sure to check all release notes between the version you are running and the one you are upgrading to. Use the search function in your browser (`CTRL + f`) and search for **Breaking Changes**.
3. Check your configuration using the [Check Open Peer Power configuration](/addons/check_config/) add-on.
4. If the check passes, you can safely update. If not, update your configuration accordingly.
5. Update Open Peer Power.
