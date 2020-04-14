---
title: "Installation on macOS"
description: "Installation of Open Peer Power on your macOS system."
---

[macOS](http://www.apple.com/macos/) is available by default on Apple computer. If you run a different operating system, please refer to the other section of the documentation.

To run Open Peer Power on macOS, you need to install Python first. Download Python 3.7 or later from [https://www.python.org/downloads/mac-osx/](https://www.python.org/downloads/mac-osx/) and follow the instructions of the installer.

Open a terminal and install Open Peer Power in a virtual environment:

```bash
python3 -m venv homeassistant
source homeassistant/bin/activate
pip3 install homeassistant
```

You can then configure Open Peer Power to autostart by following [this guide](/docs/autostart/macos/).
