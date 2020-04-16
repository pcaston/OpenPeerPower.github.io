---
title: Default Config
description: The default configuration integration will initiate a default configuration for Open Peer Power.
ha_category:
  - Other
ha_release: 0.88
ha_domain: default_config
---

This integration is a meta-component and configures a default set of integrations for Open Peer Power to load. The integrations that will be loaded are:

- [Automation](/integrations/automation/)
- [Open Peer Power Cloud](/integrations/cloud/)
- [Configuration](/integrations/config/)
- [Frontend](/integrations/frontend/)
- [History](/integrations/history/)
- [Logbook](/integrations/logbook/)
- [Map](/integrations/map/)
- [Mobile App Support](/integrations/mobile_app/)
- [Person](/integrations/person/)
- [Scripts](/integrations/script/)
- [Simple Service Discovery Protocol (SSDP)](/integrations/ssdp/)
- [Sun](/integrations/sun/)
- [System Health](/integrations/system_health/)
- [Updater](/integrations/updater/)
- [Zero-configuration networking (zeroconf)](/integrations/zeroconf/)
- [Zone](/integrations/zone)

## Configuration

To integrate this into Open Peer Power, add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
default_config:
```
