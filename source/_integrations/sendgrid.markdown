---
title: SendGrid
description: Instructions on how to add email notifications via SendGrid to Open Peer Power.
logo: sendgrid.png
ha_category:
  - Notifications
ha_release: 0.14
ha_domain: sendgrid
excerpt: none
---

The `sendgrid` notification platform sends email notifications via [SendGrid](https://sendgrid.com/), a proven cloud-based email platform.

## Setup

You need an [API key](https://app.sendgrid.com/settings/api_keys) from SendGrid.

## Configuration

To enable notification emails via SendGrid in your installation, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
notify:
  - name: NOTIFIER_NAME
    platform: sendgrid
    api_key: YOUR_API_KEY
    sender: SENDER_EMAIL_ADDRESS
    recipient: YOUR_RECIPIENT
{% endhighlight %}

To use notifications, please see the [getting started with automation page](/getting-started/automation/).
