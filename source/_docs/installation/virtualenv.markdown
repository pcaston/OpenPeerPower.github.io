---
title: "Installation in Python virtual environment"
description: "How to install Open Peer Power in a Python virtual environment."
redirect_from: /getting-started/installation-virtualenv/
---

If you already have Python 3.7 or later installed, you can easily give Open Peer Power a spin.

It's recommended when installing Python packages that you use a [virtual environment](https://docs.python.org/3.7/library/venv.html#module-venv). This will make sure that your Python installation and Open Peer Power installation won't impact one another. The following steps will work on most UNIX like systems.

<div class='note'>

This is a generic guide for running Open Peer Power under Python. We recommend to use [our recommended installation guides](/docs/installation/#recommended). The steps below may be shorter but some users find difficulty when applying updates and may run into issues.

Before you begin the guide below, ensure that you have a *so-called standard* build environment that includes things like `make`, `gcc`, `python3`, including Python 3 `setuptools` and `pip` modules. Less obvious is the need to install `openssl-dev` (for opensslv.h) and `libffi-dev` (for cffi.h) for things to build later on.

</div>

{% comment %}
This page describes installation instructions for a pure Python installation.
It should not contain any OS specific instructions.
{% endcomment %}

### Install

 1. Create a virtual environment in your current directory:
    {% highlight bash %}
    python3 -m venv homeassistant
    {% endhighlight %}
 2. Open the virtual environment:
    {% highlight bash %}
    cd homeassistant
    {% endhighlight %}
 3. Activate the virtual environment:
    {% highlight bash %}
    source bin/activate
    {% endhighlight %}
 4. Install Open Peer Power:
    {% highlight bash %}
    python3 -m pip install homeassistant
    {% endhighlight %}    
 5. Run Open Peer Power:
    {% highlight bash %}
    hass --open-ui
    {% endhighlight %}
 6. You can now reach the web interface on `http://ipaddress:8123/` - the first start may take a couple of minutes before the web interface is available. This can take longer if you're using lower-end hardware like a Raspberry Pi Zero.
 
### Upgrade

 1. Stop Open Peer Power

 2. Open the directory where the virtual environment is located, activate the virtual environment, then upgrade Open Peer Power:
    {% highlight bash %}
    cd homeassistant
    source bin/activate
    python3 -m pip install --upgrade homeassistant
    {% endhighlight %}
 3. Start Open Peer Power
 4. You can now reach the web interface on `http://ipaddress:8123/` - the first start may take some time before the web interface is available, depending on how many integrations need to be upgraded.

### Run a specific version

In the event that a Open Peer Power version doesn't play well with your hardware setup, you can downgrade to a previous release. For example:

{% highlight bash %}
cd homeassistant
source bin/activate
pip3 install homeassistant==0.XX.X
{% endhighlight %}

#### Run the beta version

If you would like to test next release before anyone else, you can install the beta version, for example:

{% highlight bash %}
cd homeassistant
source bin/activate
pip3 install --pre --upgrade homeassistant
{% endhighlight %}

#### Run the development version

If you want to stay on the bleeding-edge Open Peer Power development branch, you can upgrade to `dev`.

<div class='note warning'>
  The "dev" branch is likely to be unstable. Potential consequences include loss of data and instance corruption.
</div>

For example:

{% highlight bash %}
cd homeassistant
source bin/activate
pip3 install --upgrade git+git://github.com/OpenPeerPower/Open-Peer-Power.git@dev
{% endhighlight %}

### Notes

- In the future, if you want to start Open Peer Power manually again, follow step 2, 3 and 5.
- It's recommended to run Open Peer Power as a dedicated user.
- If you want Open Peer Power to automatically start at boot, check the [autostart documentation](/docs/autostart/)

<div class='info'>
 
Looking for more advanced guides? Check our [Raspbian guide](/docs/installation/raspberry-pi/) or the [other installation guides](/docs/installation/).

</div>

### After upgrading Python

If you've upgraded Python (for example, you were running 3.7.1 and now you've installed 3.7.3) then you'll need to build a new virtual environment. Simply rename your existing virtual environment directory:

{% highlight bash %}
mv homeassistant homeassistant.old
{% endhighlight %}

Then follow the [Install](#install) steps again, being sure to use the newly installed version of Python.
