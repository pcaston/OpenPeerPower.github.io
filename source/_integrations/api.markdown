---
title: Open Peer Power API
description: Instructions on how to setup the RESTful API within Open Peer Power.
ha_category:
  - Other
ha_release: 0.7
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
ha_domain: api
---

The `api` integration exposes a RESTful API and allows one to interact with a Open Peer Power instance that is running headless. This integration depends on the [`http` integration](/integrations/http/).

```yaml
# Example configuration.yaml entry
api:
```

For details to use the API, please refer to the [REST API](/developers/rest_api/) in the "Developer" section.
