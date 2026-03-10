# Current state

## Infrastructure Baseline – Phase 1 (Foundation & Secure Access)

### Objectif de la phase

Mettre en place une base :

* Stable
* Sécurisée
* Structurée
* Évolutive
* Documentée

Cette phase couvre :

* Installation propre de Proxmox
* Sécurisation des accès
* Mise en place du stockage structuré
* Préparation aux étapes réseau et services futurs

***

## Infrastructure matérielle actuelle

### Serveur principal

* **Machine** : Dell Optiplex 3080 SFF
* **CPU** : i5-10505
* **RAM** : 32GB
* **NIC** : Intel I350-T2 (dual port)
* **Switch** : Netgear GS308E

### Stockage

| Disque       | Rôle                | Technologie |
| ------------ | ------------------- | ----------- |
| NVMe 256GB   | OS + Infra critique | LVM         |
| SATA SSD 1TB | Workloads VM        | LVM-thin    |

***

## Installation Proxmox

* Mode UEFI
* Secure Boot désactivé
* PXE / Network boot désactivé
* Installation sur NVMe
* Filesystem : EXT4 + LVM

### IOMMU

Activé via :

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
```

Validation :

```bash
dmesg | grep -e DMAR -e IOMMU
```

***

## Sécurisation des accès

### Exposition Internet

* Aucun port forward actif sur Freebox
* UPnP désactivé
* Pas d’exposition publique

***

### Accès distant

Utilisation de **Tailscale** :

* Accès via IP 100.x.x.x
* ACL strictes configurées via `grants`
* Seul l’utilisateur principal peut accéder à :
  * TCP 22
  * TCP 8006

Architecture :

```
Internet
   ↓
Tailscale (ACL identity-based)
   ↓
Proxmox Host
```

***

### SSH

* Root SSH désactivé
* Password SSH désactivé
* Authentification par clé uniquement
* User Linux dédié (`paulb`)
* Accès sudo validé

***

### UI Proxmox

* Compte root désactivé pour l’UI
* Compte admin dédié (`paul@pve`)
* TOTP activé (Aegis)
* 2FA Google actif (Tailscale)

***

### Firewall Proxmox

Configuration stricte :

#### Policy

* Datacenter → Input = DROP
* Output = ACCEPT

#### Règles autorisées

* SSH LAN (192.168.1.0/24)
* UI LAN
* SSH Tailscale (100.64.0.0/10)
* UI Tailscale
* ESTABLISHED,RELATED

Tout le reste est DROP.

***

## Architecture stockage

### NVMe (VG: pve)

Contient :

* `root`
* `swap`
* `local-lvm` (thin pool)

Usage :

* OS
* Services critiques futurs

***

### SATA 1TB (VG: vg\_vmdata)

Création :

```bash
pvcreate /dev/sda
vgcreate vg_vmdata /dev/sda
lvcreate -l 95%VG -T vg_vmdata/vmdata
```

Ajouté dans Proxmox comme LVM-thin.

Usage :

* VMs workloads
* Containers
* Templates

***

### Séparation logique

| Storage   | Usage                |
| --------- | -------------------- |
| local     | ISO / backups        |
| local-lvm | Infra critique       |
| vmdata    | Workloads principaux |

***

## État actuel de l’infrastructure

### Sécurité

* Accès distant sécurisé
* Zero Trust overlay
* SSH hardenisé
* 2FA actif
* Pas d’exposition publique

### Stockage

* Séparé OS / workloads
* Thin provisioning activé
* Prêt pour migration future ZFS mirror

### Virtualisation

* IOMMU activé
* Prêt pour passthrough futur (pfSense, etc.)

***

## Décisions d’architecture (ADR implicites)

1. Pas de ZFS mono-disk (complexité prématurée)
2. Séparation OS / workload
3. Overlay VPN plutôt que port forwarding
4. Authentification basée sur identité
5. Root non utilisé en production

***

## Prochaines étapes (Phase 2)

### Priorité 1 – Infrastructure réseau

* Déployer pfSense en VM
* Segmenter WAN / LAN
* Préparer VLAN sur switch
* Mettre la Freebox en bridge

***

### Priorité 2 – Templates & Workloads

* Créer template Debian propre
* Mettre en place CI/CD VM provisioning
* Déployer premier service réel (Vaultwarden ou monitoring)

***

### Priorité 3 – Observabilité

* Prometheus
* Grafana
* Loki

***

### Priorité 4 – Backup Strategy

* Backup local
* Test restauration
* Stratégie off-site

***

## Position actuelle dans la roadmap

```
[✓] Stabilité
[✓] Sécurité d’accès
[✓] Structuration stockage
[ ] Segmentation réseau
[ ] Observabilité
[ ] Backup mature
[ ] Automation
[ ] Kubernetes
```
