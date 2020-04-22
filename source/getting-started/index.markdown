---
title: "Install Open Peer Power"
description: "Getting started: How to install Open Peer Power."
---

{% comment %}

Note for contributors:

The getting started guide aims at getting new users get Open Peer Power up and
running as fast as possible. Nothing else. All other things should not be
written down, as it creates a spaghetti of links and the user will lose focus.

So here are guidelines:

 - Focus on the bare necessities. No remote port, no securing installation. The
   defaults are good enough to get a system up and running for the first guide.
 - Avoid or explain technical terms.
 - Do not talk about YAML if it can be partially/fully done in UI.
 - Do not tell people about stuff they can do later. This can be added to a
   2nd tier guide.
 - The first page of the guide is for installation, hence Open Peer Power specific.
   Other pages should not refer to it except for the page introducing the last
   page that introduces `configuration.yaml`.

{% endcomment %}

This guide will help you get Open Peer Power running on a Raspberry Pi, to create an efficient power management hub.

Follow this guide if you want to get started with Open Peer Power easily or if you have little to no Linux experience. For advanced users (or if you don't have a [device that is supported by this guide][supported]), check out our [alternative installation methods](/docs/installation/). Once you finish your alternative installation, you can continue at the [next step][next-step].

[supported]: /hassio/installation/

### Suggested hardware

The Raspberry Pi 4 Model B is a good, affordable option for the power management server. 

- Raspberry Pi 4 Model B (2GB) + Power Supply (at least 2.5A)
- Micro SD Card. Ideally get one that is Application Class 2 as they handle small I/O much more consistently than cards not optimized to host applications. A 32 GB or bigger card is recommended.
- SD Card reader. This is already part of most laptops, but you can purchase a standalone USB adapter if you don't have one. The brand doesn't matter, just pick the cheapest.
- Ethernet cable. Open Peer Power can work with Wi-Fi, but an Ethernet connection would be more reliable.

### Software requirements

- Download and extract the Open Peer Power image for [your device](/hassio/installation/)
- Download [balenaEtcher] to write the image to an SD card

[balenaEtcher]: https://www.balena.io/etcher

### Installation

1. Put the SD card in your card reader.
2. Open balenaEtcher, select the Open Peer Power image and flash it to the SD card.
3. Unmount the SD card and remove it from your card reader.
4. Follow this step if you want to configure Wi-Fi or a static IP address (this step requires a USB stick). Otherwise, move to step 5.
   - Format a USB stick to FAT32 with the volume name `CONFIG`.
   - Create a folder named `network` in the root of the newly-formatted USB stick.
   - Within that folder, create a file named `my-network` without a file extension.
   - Copy one of [the examples] to the `my-network` file and adjust accordingly.
   - Plug the USB stick into the Raspberry Pi.

5. Insert the SD card into your Raspberry Pi. If you are going to use an Ethernet cable, connect that too.
6. Connect your power supply to the Raspberry Pi.
7. The Raspberry Pi will now boot up, connect to the Internet and download the latest version of Open Peer Power. This will take about 20 minutes.
8. Open Peer Power will be available at `http://openpeerpower.local:8123`. If you are running an older Windows version or have a stricter network configuration, you might need to access Open Peer Power at `http://openpeerpower:8123` or `http://X.X.X.X:8123` (replace `X.X.X.X` with your Pi's IP address).
9. If you used a USB stick for configuring the network, you can now remove it.

[the examples]: https://github.com/OpenPeerPower/hassos/blob/dev/Documentation/network.md

### [Next step: Onboarding &raquo;][next-step]

[next-step]: /getting-started/onboarding/
