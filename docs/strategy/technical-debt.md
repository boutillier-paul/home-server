---
coverY: 0
layout:
  width: default
  cover:
    visible: false
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Technical debt

### Overview

The current platform includes several known limitations resulting from hardware constraints and incremental deployment strategy.

Tracking these items helps prioritize improvements without forcing unnecessary changes.

***

### Current Debt

* Single Proxmox node (single disk before ZFS mirror)
* Flat network topology
* No dedicated firewall & firewall not yet deployed
* No centralized identity provider
* Limited storage redundancy

***

### Planned Remediation

| Network    | High   | Deploy pfSense and VLAN segmentation |
| ---------- | ------ | ------------------------------------ |
| Storage    | High   | ZFS mirror and backup server         |
| Identity   | Medium | Deploy Authentik or Keycloak         |
| Automation | Low    | Introduce Terraform and Ansible      |

***

### Review Strategy

This register should be reviewed after major infrastructure changes or roadmap milestones.





