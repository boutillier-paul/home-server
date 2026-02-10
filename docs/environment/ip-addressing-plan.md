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

# IP addressing plan

### Current Addressing

Currently managed by the ISP router DHCP range.

All hosts are part of a single subnet.

***

### Target Addressing Scheme

<table><thead><tr><th valign="middle">Network</th><th>Subnet</th><th>Usage</th></tr></thead><tbody><tr><td valign="middle">Management</td><td>10.10.10.0/24</td><td>Admin access</td></tr><tr><td valign="middle">Services</td><td>10.10.20.0/24</td><td>Internal services</td></tr><tr><td valign="middle">DMZ</td><td>10.10.30.0/24</td><td>Public exposure</td></tr><tr><td valign="middle">Lab</td><td>10.10.40.0/24</td><td>Experiments</td></tr><tr><td valign="middle">Storage</td><td>10.10.50.0/24</td><td>Backend communication</td></tr></tbody></table>

The goal is to maintain clear separation between administrative, user and exposed workloads.





