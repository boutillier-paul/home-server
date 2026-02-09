---
layout:
  width: default
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

# Current architecture

### Overview

The platform currently consists of a single Proxmox node acting as the primary compute host.

Network access is provided by an ISP router with a flat LAN topology.

***

### Current Components

* Proxmox VE single node
* NVMe system disk
* SSH secured access
* Initial documentation repository
* Cloudflare-based external access (planned)

***

### Limitations

* No network segmentation
* No centralized identity provider
* No backup server deployed yet

***

(Attach: Current infrastructure topology diagram)
