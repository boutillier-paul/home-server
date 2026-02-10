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

# ADR-0001 - Virtualization Platform Selection

### Status

Accepted

***

### Context

The platform requires a primary compute layer capable of:

* Running multiple workloads
* Supporting experimentation
* Providing flexibility for future cluster expansion

The environment is constrained by limited hardware resources and a single operator.

The solution must balance:

* Stability
* Learning value
* Operational simplicity

***

### Decision Drivers

* Ease of deployment
* Resource efficiency
* Community support
* Open-source availability
* Ability to evolve toward cluster architectures

***

### Considered Options

<table data-header-hidden><thead><tr><th valign="top">Option</th><th>Advantages</th><th valign="top">Limitations</th></tr></thead><tbody><tr><td valign="top"><strong>Proxmox VE</strong></td><td>Mature virtualization platform, supports VMs and containers, clustering support, efficient on modest hardware</td><td valign="top">Not cloud-native by default</td></tr><tr><td valign="top"><strong>OpenStack</strong></td><td>Industry-standard private cloud, highly scalable</td><td valign="top">Extremely heavy for a small environment, high operational complexity</td></tr><tr><td valign="top"><strong>Talos + Kubernetes Only</strong></td><td>Modern declarative infrastructure</td><td valign="top">Does not replace virtualization layer, adds early complexity</td></tr><tr><td valign="top"><strong>Bare Metal + Docker</strong></td><td>Minimal overhead</td><td valign="top">Limited isolation, poor multi-environment management</td></tr><tr><td valign="top"><strong>VMware ESXi</strong></td><td>Enterprise-grade</td><td valign="top">Licensing constraints, less aligned with open-source preference</td></tr></tbody></table>

***

### Decision

Proxmox VE was selected as the primary compute platform.

***

### Rationale

Proxmox provides the best balance between:

* Stability
* Resource efficiency
* Learning value
* Future cluster capability
* Ease of operation

It allows experimentation across multiple paradigms (VMs, containers, Kubernetes) without requiring excessive hardware.

***

### Consequences

#### Positive

* Flexible workload hosting
* Supports gradual evolution
* Good observability

***

#### Negative

* Requires manual infrastructure lifecycle management

***

#### Neutral / Future Work

* Kubernetes will be layered on top rather than replacing the compute platform.

***

### Related Documents

* [Current Architecture](../environment/current-architecture.md)
* [Platform Capabilities](../environment/platform-capabilities.md)





