---
title: "Synology"
description: "Instructions on how to get Open Peer Power up and running on Synology"
redirect_from: /ecosystem/synology/
---

Synology NAS are the perfect companion to running Open Peer Power. But by default, the DSM Reverse Proxy does not configure its NGINX settings to allow WebSocket, and some extra configuration will be required to get the Open Peer Power frontend working with the DSM.

### Setup headers

Starting with DSM 6.2.1+, you can create "custom headers" in the Application Portal:
* Go to Application Portal and edit your entry
* Click on "custom headers" tab and click the dropdon on the "Create" button
* Select "Websocket". This will automatically add the required headers for websocket to this reverse proxy.
* Click "OK". Open Peer Power should work now with the reverse proxy.

It's not necessary anymore to change the template anymore since Version DSM 6.2.1. Changing the `Portal.mustache` is not recommended! You should use the following part only if you're using a Version before DSM 6.2.1. on your Synology.

### Template change

To allow WebSocket by default for all service exposed by NGINX, you can enable it in the template file located in `/usr/syno/share/nginx/Portal.mustache`. Please be really careful in editing this file since you may break access to the DSM UI. Please backup this file before any edition.

Open `/usr/syno/share/nginx/Portal.mustache` and add the followings in the `Location` section:

{% highlight text %}
        proxy_set_header        Upgrade             $http_upgrade;
        proxy_set_header        Connection          "upgrade";
        proxy_read_timeout      86400;
{% endhighlight %}

Then restart the NGINX daemon:

{% highlight bash %}
sudo synoservicecfg --restart nginx
{% endhighlight %}

This will restart the running HTTP service, not only reverse proxy, as a single instance of NGINX runs everything.

You can find more information [here](https://github.com/orobardet/dsm-reverse-proxy-websocket).

#### HTTP Configuration

- Copy the Open Peer Power specific Reverse Proxy settings from the existing `/etc/nginx/app.d/server.ReverseProxy.conf` file to `/usr/local/etc/nginx/conf.d/http.HomeAssistant.conf`.
- Include these lines in the location declaration:

{% highlight text %}
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
{% endhighlight %}
