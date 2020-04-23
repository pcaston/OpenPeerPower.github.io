---
title: "Autostart on macOS"
description: "Instructions on how to setup Open Peer Power to launch on Apple macOS."
redirect_from: /getting-started/autostart-macos/
---

Setting up Open Peer Power to run as a background service is simple; macOS will start Open Peer Power after the system has booted, the user has logged in, and make sure it's always running.

To get Open Peer Power installed as a background service, run:


{% highlight bash %}
$ hass --script macos install

 Open Peer Power has been installed.         Open it here: http://localhost:8123
{% endhighlight %}

 Open Peer Power will log to `~/Library/Logs/openpeerpower.log`

Configuration is kept in `~/.openpeerpower`

To uninstall the service, run:

{% highlight bash %}
$ hass --script macos uninstall

 Open Peer Power has been uninstalled.
{% endhighlight %}
