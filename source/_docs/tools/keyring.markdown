---
title: "keyring"
description: "Script to store secrets in a keyring"
---

Using [Keyring](https://github.com/jaraco/keyring) is an alternative way to `secrets.yaml`. The secrets can be managed from the command line via the `keyring` script.

{% highlight bash %}
$ hass --script keyring --help
{% endhighlight %}

To store a password in keyring, replace your password or API key with `!secret` and an identifier in `configuration.yaml` file.

{% highlight yaml %}
integration1:
  api_key: !secret integration1_key
{% endhighlight %}

Create an entry in your keyring.

{% highlight bash %}
$ hass --script keyring set integration1_key
{% endhighlight %}

If you launch Open Peer Power now, you will be prompted for the keyring password to unlock your keyring.

{% highlight bash %}
$ hass
Config directory: /home/openpeerpower/.openpeerpower
Please enter password for encrypted keyring:
{% endhighlight %}

<div class='note warning'>

  If you are using the Python Keyring, [autostarting](/getting-started/autostart/) of Open Peer Power will no longer work.

</div>
