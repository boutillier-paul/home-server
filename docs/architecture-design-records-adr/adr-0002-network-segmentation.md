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

# ADR-0002-network-segmentation

### Status

Accepted

***

### Context

The platform initially operates on a flat LAN provided by the ISP router.

As services expand and external exposure increases, segmentation becomes necessary to:

* Reduce attack surface
* Isolate workloads
* Separate administrative traffic
* Enable more realistic infrastructure scenarios

A routing and firewall solution must be selected.

***

### Decision Drivers

* Security improvement
* VLAN support
* Learning value
* Operational simplicity
* Compatibility with existing hardware

***

### Considered Options

<table data-header-hidden><thead><tr><th valign="top">Option</th><th>Advantages</th><th valign="top">Limitations</th></tr></thead><tbody><tr><td valign="top"><strong>pfSense</strong></td><td>Mature firewall platform, strong documentation, enterprise-grade features</td><td valign="top">Slightly heavier resource footprint</td></tr><tr><td valign="top"><strong>OPNsense</strong></td><td>Modern interface, similar capabilities, open governance</td><td valign="top">Smaller community compared to pfSense</td></tr><tr><td valign="top"><strong>RouterOS (Mikrotik)</strong></td><td>Powerful routing features</td><td valign="top">Less aligned with open-source learning goals</td></tr><tr><td valign="top"><strong>OpenWRT</strong></td><td>Lightweight, flexible</td><td valign="top">Limited enterprise-grade firewall features</td></tr><tr><td valign="top"><strong>Cloudflare Tunnel Only</strong></td><td>Simplifies external exposure</td><td valign="top">Does not provide internal segmentation</td></tr><tr><td valign="top"><strong>No Segmentation</strong></td><td>Simplicity</td><td valign="top">Increased risk and lack of isolation</td></tr></tbody></table>

***

### Decision

Implement VLAN-based segmentation with pfSense (or OPNsense if operational constraints require).

***

### Rationale

A dedicated firewall layer enables:

* Inter-VLAN routing
* Access control
* Traffic visibility

pfSense and OPNsense provide the best balance between:

* Feature completeness
* Learning value
* Long-term maintainability

***

### Consequences

#### Positive

* Improved security posture
* Better traffic isolation
* Ability to simulate enterprise network design

#### Negative

* Additional operational complexity
* Requires additional configuration effort

#### Neutral / Future Work

* Potential migration from VM-based firewall to dedicated hardware appliance.

***

### Related Documents

* [Current architecture](../environment/current-architecture.md)
* [Network topology](../environment/network-topology.md)
* [IP addressing plan](../environment/ip-addressing-plan.md)
* [Platform capabilities](../environment/platform-capabilities.md)
* [Roadmap & evolution](../strategy/roadmap-and-evolution.md)
* [Technical debt](../strategy/technical-debt.md)





