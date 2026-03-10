# ADR-0002 Network Segmentation & Firewall Strategy

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

<table><thead><tr><th valign="top">Option</th><th>Advantages</th><th valign="top">Limitations</th></tr></thead><tbody><tr><td valign="top"><strong>pfSense</strong></td><td><ul><li>Mature firewall platform</li><li>Strong documentation</li><li>Enterprise-grade features</li></ul></td><td valign="top"><ul><li>Slightly heavier resource footprint</li></ul></td></tr><tr><td valign="top"><strong>OPNsense</strong></td><td><ul><li>Modern interface, similar capabilities, open governance</li></ul></td><td valign="top"><ul><li>Smaller community compared to pfSense</li></ul></td></tr><tr><td valign="top"><strong>RouterOS (Mikrotik)</strong></td><td><ul><li>Powerful routing features</li></ul></td><td valign="top"><ul><li>Less aligned with open-source learning goals</li></ul></td></tr><tr><td valign="top"><strong>OpenWRT</strong></td><td><ul><li>Lightweight</li><li>Flexible</li></ul></td><td valign="top"><ul><li>Limited enterprise-grade firewall features</li></ul></td></tr><tr><td valign="top"><strong>Cloudflare Tunnel Only</strong></td><td><ul><li>Simplifies external exposure</li></ul></td><td valign="top"><ul><li>Does not provide internal segmentation</li></ul></td></tr><tr><td valign="top"><strong>No Segmentation</strong></td><td><ul><li>Simplicity</li></ul></td><td valign="top"><ul><li>Increased risk and lack of isolation</li></ul></td></tr></tbody></table>

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





