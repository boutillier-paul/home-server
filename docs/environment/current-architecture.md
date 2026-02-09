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

The platform currently relies on a single Proxmox VE node acting as the primary compute host.

At this stage, the infrastructure is intentionally simple in order to establish stable foundations before introducing additional layers such as segmentation, centralized identity or automation.

External connectivity is provided by the ISP router, while internal services are hosted locally.

***

### Current Components

* Proxmox VE (single node)
* NVMe system disk
* Managed switch (Layer 2)
* Intel dual-port Ethernet NIC
* Auxiliary Orange Pi node used for lightweight workloads

Most services are currently deployed directly within the local network without strict segmentation.

***

### Design Intent

The current architecture prioritizes:

* Stability
* Observability of system behaviour
* Ease of recovery

More advanced capabilities such as VLAN isolation, dedicated firewalling and cluster expansion are planned but not yet fully implemented.

***

### High-Level Overview

<figure><img src="../.gitbook/assets/simple-network-diagram.png" alt=""><figcaption></figcaption></figure>
