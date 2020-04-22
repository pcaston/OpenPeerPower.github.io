---
title: "Security of Open Peer Power"
description: "Security of Open Peer Power."
---

As Open Peer Power is like every other service or daemon that is running on a computer system that allows access over a network connection, certain measures were taken to increase the overall security while still staying operational.

[Secure your installation](/docs/configuration/securing/) once you've finished with the installation process regardless of your use case.

 Open Peer Power is NOT able to change the configuration of your router or firewall. This means that you need to setup [port-forwarding](/docs/configuration/remote/) and adjusting firewall rules if you want to allow access from the internet. By default your frontend and your Open Peer Power add-ons like Mosquitto, SSH and your Samba shares are only accessible from your local network.

## Server banner

Further [details about the fingerprint/server banner](/docs/security/webserver/) of a Open Peer Power instance are available. 

## Porosity

The default port of Open Peer Power is 8123. This is the port where the [`frontend`](/integrations/frontend/) and the [`API`](/integrations/api/) is served. Both are depending on the [`http`](/integrations/http/) integration which contains the capability to adjust the settings like `server_host` or `server_port`.

## HTTP SSL/TLS

 Open Peer Power is following the [Mozilla's Operations Security team recommendations](https://wiki.mozilla.org/Security/Server_Side_TLS) for Server side SSL/TLS settings. Open Peer Power uses **Modern compatibility** by default. If an user wishes to use **Intermediate compatibility**, this is configurable in the [`http` integration](/integrations/http/).

## SSH

If SSH is used with the [SSH server add-on](/addons/ssh/) then the user is responsible for the configuration and security.

## Source code

Due to the lack of resources we are not able to review all of our dependencies and inspect them for malicious behavior, leakage of information or compliance with GDPR. But we have a keen interest in the development of our dependencies and try to work closely with the upstream developer.

