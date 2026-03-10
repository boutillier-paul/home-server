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

<table><thead><tr><th valign="top">Option</th><th>Advantages</th><th valign="top">Limitations</th></tr></thead><tbody><tr><td valign="top"><strong>Proxmox VE</strong></td><td><ul><li>Mature virtualization platform</li><li>Supports VMs and containers</li><li>Clustering support</li><li>Efficient on modest hardware</li></ul></td><td valign="top"><ul><li>Not cloud-native by default</li></ul></td></tr><tr><td valign="top"><strong>OpenStack</strong></td><td><ul><li>Industry-standard private cloud</li><li>Highly scalable</li></ul></td><td valign="top"><ul><li>Extremely heavy for a small environment</li><li>High operational complexity</li></ul></td></tr><tr><td valign="top"><strong>Talos + Kubernetes Only</strong></td><td><ul><li>Modern declarative infrastructure</li></ul></td><td valign="top"><ul><li>Does not replace virtualization layer</li><li>Adds early complexity</li></ul></td></tr><tr><td valign="top"><strong>Bare Metal + Docker</strong></td><td><ul><li>Minimal overhead</li></ul></td><td valign="top"><ul><li>Limited isolation</li><li>Poor multi-environment management</li></ul></td></tr><tr><td valign="top"><strong>VMware ESXi</strong></td><td><ul><li>Enterprise-grade</li></ul></td><td valign="top"><ul><li>Licensing constraints</li><li>Less aligned with open-source preference</li></ul></td></tr></tbody></table>

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





