---
title: "check_config"
description: "Script to perform a check of the current configuration"
---

Test any changes to your `configuration.yaml` file before launching Open Peer Power. This script allows you to test changes without the need to restart Open Peer Power.

{% highlight bash %}
$ hass --script check_config
{% endhighlight %}

The script has further options like checking configuration files which are not located in the default directory or showing your secrets for debugging.

{% highlight bash %}
$ hass --script check_config -h
usage: hass [-h] [--script {check_config}] [-c CONFIG] [-i [INFO]] [-f] [-s]

Check Open Peer Power configuration.

optional arguments:
  -h, --help            show this help message and exit
  --script {check_config}
  -c CONFIG, --config CONFIG
                        Directory that contains the Open Peer Power
                        configuration
  -i [INFO], --info [INFO]
                        Show a portion of the config
  -f, --files           Show used configuration files
  -s, --secrets         Show secret information
{% endhighlight %}

