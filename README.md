# PPE — Déploiement d'une solution Proxmox VE (cluster, HA, migration)

Projet de la semaine PPE du 9 au 13 novembre 2026, module Virtualisation (CFC 190), Geneva Institute of Technology, classe E1B.

Mise en place d'un cluster **Proxmox VE 9** à deux nœuds avec un **QDevice** pour le quorum, un **stockage partagé NFS** (Debian 12), puis tests de haute disponibilité, de migration à chaud, de déplacement de disque, de snapshots, de clones et de permissions. Tout le lab tourne dans **VMware Workstation** sur un PC portable, sans serveur physique.

| Machine | Rôle | Adresse |
|---|---|---|
| pve-01 | Nœud Proxmox | 10.10.10.21 / 10.10.30.21 |
| pve-02 | Nœud Proxmox | 10.10.10.23 / 10.10.30.23 |
| qdevice-01 | Arbitre du quorum (Debian 12) | 10.10.10.14 |
| nas-01 | Serveur NFS (Debian 12) : `/srv/nfs/ds01`, `/srv/nfs/ds02` | 10.10.10.50 |

## Contenu du dépôt

- `RAPPORT.md` : le rapport complet (également publié sur HackMD)
- `captures/` : les captures d'écran du rapport

## Auteur

Paul Giocanti — [GitHub L4ami](https://github.com/L4ami)
