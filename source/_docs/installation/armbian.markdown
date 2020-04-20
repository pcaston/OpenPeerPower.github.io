---
title: "Installation on a Armbian system"
description: "Instructions to install Open Peer Power on an Armbian-powered systems."
---

[armbian](https://www.armbian.com) runs on a wide-variety of [ARM development boards](https://www.armbian.com/download/). Currently there are around 50 boards supported inclusive the OrangePi family, Cubieboard, Pine64, and ODROID.

Setup Python and `pip`:

{% highlight bash %}
sudo apt-get update
sudo apt-get install python3-dev python3-pip libffi-dev libssl-dev
{% endhighlight %}

Now that you installed python, there are two ways to install Open Peer Power:

1. It is recommended to install Open Peer Power in a virtual environment to avoid using `root`, using the [VirtualEnv instructions](/docs/installation/virtualenv/)
2. Alternatively, you can install Open Peer Power for the user you created when first booting Armbian:

{% highlight bash %}
sudo pip3 install homeassistant
hass --open-ui
{% endhighlight %}

Running these commands will:

- Install Open Peer Power
- Launch Open Peer Power and serve the web interface on `http://localhost:8123`
- The configuration files will be created in `/home/{user}/.homeassistant`
