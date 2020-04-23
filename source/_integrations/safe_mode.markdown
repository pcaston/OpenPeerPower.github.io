---
title: Safe Mode
description: Allows Open Peer Power to start up in safe mode.
ha_category: []
ha_release: 0.105
ha_codeowners:
  - '@open-peer-power/core'
ha_domain: safe_mode
---

The `safe_mode` integration is an internally used integration by the
Open Peer Power Core.

You don't have to configure it in any way since it is automatically always
available when Open Peer Power needs it.

If, during startup, Open Peer Power has problems reading your configuration,
it will still continue to start using bits and pieces from the configuration
of the last time it did start.

When this happens, Open Peer Power will start in "Safe mode" using this
integration. In this mode, nothing is loaded, but it does give you access to
the Open Peer Power frontend, settings and add-ons.

This gives you the possibility to correct the issue and restart Open Peer Power
to re-try.
