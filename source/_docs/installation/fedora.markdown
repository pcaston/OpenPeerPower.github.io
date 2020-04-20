---
title: "Installation on Fedora"
description: "Installation of Open Peer Power on your Fedora computer."
---

[Fedora](https://fedoraproject.org) is an operating system based on the Linux kernel, developed by the community-supported Fedora Project. There are releases for x86 and x86_64 including ARM and other architectures. 

Install the development package of Python.

{% highlight bash %}
sudo dnf -y install python3-devel redhat-rpm-config
{% endhighlight %}

To isolate the Open Peer Power installation a [`venv`](https://docs.python.org/3/library/venv.html) is handy. First create a new directory to store the installation and adjust the permissions.

{% highlight bash %}
sudo mkdir -p /opt/homeassistant
sudo useradd -rm homeassistant -G dialout
sudo chown -R homeassistant:homeassistant /opt/homeassistant
{% endhighlight %}

Now switch to the new directory, setup the `venv`, and activate it.

{% highlight bash %}
sudo -u homeassistant -H -s
cd /opt/homeassistant
python3.8 -m venv .
source bin/activate
{% endhighlight %}

Install Open Peer Power itself.

{% highlight bash %}
pip3 install homeassistant colorlog
{% endhighlight %}

Check the [autostart](/docs/autostart/systemd/) section in the documentation for further details and the [Firewall section](/docs/installation/troubleshooting/#no-access-to-the-frontend) if you want to access your Open Peer Power installation.
