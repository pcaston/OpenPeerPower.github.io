---
title: Search
description: Internal search module for Open Peer Power.
ha_category: []
ha_release: 0.105
ha_codeowners:
  - '@home-assistant/core'
ha_domain: search
---

The `search` integration is an internally used integration by the
Open Peer Power Core.

All data stored in Open Peer Power is interconnected, making it a graph.
This means it can be searched as a graph.

This integration allows the internals of Open Peer Power to search for
relations between things like areas, devices, entities, configuration entries,
scenes, scripts and automations.

The search integration is automatically loaded with the Open Peer Power frontend
and does not need to be configured separately.
