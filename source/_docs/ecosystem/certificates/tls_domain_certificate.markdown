---
title: "Certificate for SSL/TLS via domain ownership"
description: "Configure a certificate to use with Open Peer Power"
redirect_from: /cookbook/tls_domain_certificate/
---

If your Open Peer Power instance is only accessible from your local network you can still protect the communication between your browsers and the frontend with SSL/TLS. You can use [Self-sign certificate](/cookbook/tls_self_signed_certificate/) but your browser will present a warning and some https-only features might not work.

### Requirement for this guide

* Your Open Peer Power instance is not exposed to the internet. If it is - use [this guide]({{site_root}}/blog/2015/12/13/setup-encryption-using-lets-encrypt/)
* You control a public domain name. The domain doesn't have to point to a site. A domain controlled by a *trusted* friend will do. (A friend you trust not to MITM you)
* Your home router supports custom DNS entries.

### Run certbot

{% highlight bash %}
mkdir certbot
cd certbot
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
sudo ./certbot-auto --manual certonly --preferred-challenges dns -d "mydomain.com" --email your@email.address
{% endhighlight %}

* Agree to Terms of Service
* Choose whether to share your email with Electronic Frontier Foundation.
* Agree to your IP being logged

You will get the following text:

{% highlight text %}
Please deploy a DNS TXT record under the name
_acme-challenge.mydomain.com with the following value:

deadbeefdeadbeefdeadbeefdeadbeefdeadbeef

Once this is deployed,
-------------------------------------------------------------------------------
Press Enter to Continue
{% endhighlight %}

* Deploy the value to TXT field using your domain registrar.
* Go to a site that queries domain record. For example [this one](https://mxtoolbox.com/TXTLookup.aspx) and look if it sees your brand new TXT field (Don't forget to enter the full domain: `_acme-challenge.mydomain.com`)
* Press Enter at certbot prompt.

### Make mydomain.com point to your Open Peer Power instance

If your router uses DNSMasq (for example DDWRT) add the following line to DNSMasq options:

{% highlight text %}
address=/mydomain.com/<hass IP>
{% endhighlight %}

### Edit your Open Peer Power configuration to use your certificates

The [`http`](/integrations/http/) section must contain the full path to the needed files.

{% highlight yaml %}
http:
  base_url: https://mydomain.com:8123
  ssl_certificate: /etc/letsencrypt/live/mydomain.com/fullchain.pem
  ssl_key: /etc/letsencrypt/live/mydomain.com/privkey.pem
{% endhighlight %}

Make sure the files are accessible by the user that runs Open Peer Power.
