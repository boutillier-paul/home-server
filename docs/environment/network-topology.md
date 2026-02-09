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

# Network topology

### Current State

Flat LAN network provided by ISP router.

***

### Target Segmentation

* Management VLAN
* Services VLAN
* DMZ VLAN
* Lab VLAN
* Storage VLAN

***

### Routing Strategy

* Short term: ISP router
* Mid term: pfSense VM
* Long term: Dedicated firewall

***

(Attach: Network segmentation diagram)
