---
title: "Connection error"
description: "Connection error"
ha_category: Usage
---

It can happen that you get a traceback that notify you about connection issues while running Open Peer Power. Eg.

{% highlight bash %}
ConnectionRefusedError: [Errno 111] Connection refused
{% endhighlight %}

The chance is very high that this is not a bug but an issue with the service/daemon itself. Check your network (DNS, DHCP, uplink, etc.) first and make sure that Open Peer Power and the service are poperly configured. Keep in mind that webservices can be down.
