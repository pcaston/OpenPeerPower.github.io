---
title: One-Time Password (OTP)
description: Instructions on how to add One-Time Password (OTP) sensors into Open Peer Power.
ha_category:
  - Utility
ha_iot_class: Local Polling
ha_release: 0.49
ha_quality_scale: internal
ha_domain: otp
excerpt: none
---

The `otp` sensor generates One-Time Passwords according to [RFC6238](https://tools.ietf.org/html/rfc6238) that is compatible with most OTP generators available, including Google Authenticator. You can use this when building custom security solutions and want to use "rolling codes", that change every 30 seconds.

## Configuration

To enable the OTP sensor, add the following lines to your `configuration.yaml`:

{% highlight yaml %}
# Example configuration.yaml entry
sensor:
  - platform: otp
    token: SHARED_SECRET_TOKEN
{% endhighlight %}

## Generating a token

A simple way to generate a `token` for a new sensor is to run this snippet of Python code in your Open Peer Power virtual environment:

{% highlight shell %}
$ pip3 install pyotp
$ python3 -c 'import pyotp; print("Token:", pyotp.random_base32())'
Token: IHEDPEBEVA2WVHB7
{% endhighlight %}

To run in a Docker container:

{% highlight shell %}
$ docker exec -it home-assistant python -c 'import pyotp; print("Token:", pyotp.random_base32())'
Token: IHEDPEBEVA2WVHB7
{% endhighlight %}

Copy and paste the token into your Open Peer Power configuration and add it to your OTP generator. Verify that they generate the same code.

<div class='note warning'>
It is vital that your system clock is correct both on your Open Peer Power server and on your OTP generator device (e.g., your phone). If not, the generated codes will not match! Make sure NTP is running and syncing your time correctly before creating an issue.
</div>
