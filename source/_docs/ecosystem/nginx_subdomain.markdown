---
title: "NGINX Configuration"
description: "Configure Nginx to work with Open Peer Power as a subdomain"
---

This example demonstrates how you can configure NGINX to act as a proxy for Open Peer Power.

This is useful if you want to have:

 * a subdomain redirecting to your Open Peer Power instance
 * several subdomain for several instance
 * HTTPS redirection

#### Subdomain

So you already have a working NGINX server available at example.org. Your Open Peer Power is correctly working on this web server and available at `http://localhost:8123`

To be able to access to your Open Peer Power instance by using `https://home.example.org`, create file `/etc/nginx/sites-enabled/openpeerpower` (or symlink via `/etc/nginx/sites-available`) and add the following:

{% highlight nginx %}
server {
    listen       443 ssl;
    server_name  home.example.org;
    
    ssl on;
    ssl_certificate /etc/nginx/ssl/home.example.org/home.example.org-bundle.crt;
    ssl_certificate_key /etc/nginx/ssl/home.example.org/home.example.org.key;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://localhost:8123;
        proxy_set_header Host $host;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /api/websocket {
        proxy_pass http://localhost:8123/api/websocket;
        proxy_set_header Host $host;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

    }
}
{% endhighlight %}

If you don't want HTTPS, you can change `listen 443 ssl` to `listen 80` or better, consider redirecting all HTTP to HTTPS. See further down.

#### Multiple Instance

You already have Open Peer Power running on `http://localhost:8123` and available at home.example.org as describe before. The configuration file for this Open Peer Power is available in `/home/alice/.openpeerpower/configuration.yaml`.

You want another instance available at `https://countryside.example.org`

You can either :
 * Create a new user, `bob`, to hold the configuration file in `/home/bob/.openpeerpower/configuration.yaml` and run Open Peer Power as this new user
 * Create another configuration directory in `/home/alice/.openpeerpower2/configuration.yaml` and run Open Peer Power using `hass --config /home/alice/.openpeerpower2/`

In both solution, change port number used by modifying `configuration.yaml` file.

{% highlight yaml %}
http:
  server_port: 8124
  ...
{% endhighlight %}

Start Open Peer Power: Now, you have another instance running on `http://localhost:8124`

To access this instance by using `https://countryside.example.org` create the file `/etc/nginx/sites-enabled/countryside.example.org` (or symlink via `/etc/nginx/sites-available`) and add the following:

{% highlight nginx %}
server {
    listen       443 ssl;
    server_name  countryside.example.org;
    
    ssl on;
    ssl_certificate /etc/nginx/ssl/countryside.example.org/countryside.example.org-bundle.crt;
    ssl_certificate_key /etc/nginx/ssl/countryside.example.org/countryside.example.org.key;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://localhost:8124;
        proxy_set_header Host $host;
        
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /api/websocket {
        proxy_pass http://localhost:8124/api/websocket;
        proxy_set_header Host $host;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

    }
}
{% endhighlight %}

#### HTTP to HTTPS redirection

Add to your `/etc/nginx/sites-enabled/default`

{% highlight nginx %}
server {
    listen       80 default_server;
    server_name  example.tld;

    return 301 https://$host$request_uri;
}
{% endhighlight %}

