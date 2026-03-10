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
  tags:
    visible: true
---

# Current state 2 - Matrix/Element Call

#### État actuel

**Infra & accès**

* Proxmox installé et accessible en admin via **Tailscale** (accès distant “clean”, sans NAT/port-forward).
* Politique Tailscale **restrictive** (grants/ACL) : accès admin limité aux ports nécessaires.
* Pare-feu Proxmox activé et règles d’accès minimales (SSH/UI).
* Stockage :
  * NVMe : OS + infra critique (local/local-lvm).
  * SSD SATA 1To : pool LVM-thin `vmdata` pour les workloads (VM/CT).

**Service “Discord-like” (phase test)**

* Un conteneur LXC dédié (ex : `matrix01`) héberge :
  * **Matrix Synapse** (homeserver)
  * Base de données **Postgres** (tu as précisé que tu l’utilises)
* Accès client fonctionnel depuis Element (connexion au homeserver OK).
* Exposition **Tailscale-only** (MagicDNS / IP Tailscale).
* Tailscale Serve utilisé pour HTTPS/proxy sur l’URL Tailscale du service.
* Début de mise en place de `.well-known` (et résolution des conflits de port 443 avec tailscaled).

***

#### Ce que ça implique (et pourquoi)

**Pourquoi Matrix**

Matrix (Synapse) apporte :

* identité, comptes, gestion des rooms, historique
* clients matures (Element desktop/mobile/web)
* contrôle des données côté serveur

Limites connues :

* l’expérience appels/vocal “Discord-like” dépend beaucoup de la couche WebRTC (TURN/SFU).

**Pourquoi Element Call + LiveKit**

* **Element Call** = l’interface web d’appel moderne (remplace/complète les appels “classiques”).
* **LiveKit** = SFU WebRTC (relai média intelligent), améliore fortement :
  * stabilité audio/vidéo
  * partage d’écran
  * appels multi-participants
  * comportements NAT/mobile

***

### Cible : une “belle stack” Tailscale-only pour 10 amis

#### Option recommandée (Discord-like moderne)

* **Synapse (Matrix)** : chat, rooms, auth
* **Postgres** : DB
* **LiveKit** : média temps réel (SFU)
* **Element Call** : UI appels (webapp servie en HTTPS)
* **Tailscale Serve** : reverse proxy + TLS (zéro ouverture Internet)
* (Optionnel) **coturn** : seulement si cas réseau difficiles ou fallback nécessaire

#### Pourquoi garder Matrix même avec LiveKit

LiveKit ne remplace pas Matrix :

* LiveKit = média (audio/vidéo/screen)
* Matrix/Synapse = messagerie + identité + rooms + permissions

Donc “Discord-like” complet = **Matrix + LiveKit + Element Call**.

***

### Sécurité : modèle d’accès Tailscale-only

#### Principes

* Aucune redirection de ports sur la box pour le service.
* Accès amis : uniquement vers le(s) service(s) nécessaires (ex : `matrix01` et `call01`), ports stricts (443).
* Pas d’Exit node, pas de subnet routing pour les amis → **tu ne transportes pas leur trafic Internet**.

#### À documenter (sans secrets)

* Liste des hôtes Tailscale déclarés (`hosts`) : `pve01`, `matrix01`, `call01`…
* Règles “grants” par rôle :
  * Admin (toi) : admin proxmox + admin services
  * Groupe “friends” : accès **uniquement** à `https://matrix…` et `https://call…` en 443

***

### Ce qui reste à faire (plan clair)

#### 1) Stabiliser l’existant (Matrix)

* Vérifier que Synapse est bien “propre” :
  * Postgres OK (backup/restores testés)
  * config `server_name` cohérente avec usage Tailscale-only
  * politique de rétention média (voir section dédiée ci-dessous)
  * caddy n'est plus présent
  * un trunk a été fait avec coturn, prévoir de le supprimer ?&#x20;
* Décider si l’instance est :
  * “privée” (non-fédérée) pour le groupe
  * “fédération off” (souvent recommandé pour débuter)

#### 2) Déployer Element Call (Tailscale-only)

* Servir la webapp Element Call via HTTPS sur une URL Tailscale :
  * ex : `https://call01.<tailnet>.ts.net`
* Protéger par ACL Tailscale : uniquement le groupe “friends” en `tcp:443`.

#### 3) Déployer LiveKit (Tailscale-only)

* Déployer LiveKit (service séparé) :
  * idéalement dans un conteneur dédié (isolation + maintenance)
* Générer et stocker les secrets (API key/secret) de manière sûre (hors Git en clair).
* Configurer Element Call ↔ LiveKit (JWT / keys).
* ACL Tailscale : exposer seulement ce qui est nécessaire (souvent 443 via proxy).



***

### Gestion de la rétention des médias (images/fichiers)

Objectif : “stocker le moins possible”.\
À mettre en place :

* Politique de purge du media store (Synapse a des mécanismes / jobs pour limiter/purger selon règles).
* Limites côté upload (taille max).
* Rotation + sauvegarde sélective si nécessaire.

***
