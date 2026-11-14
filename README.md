# 🖥️ Mon cluster Proxmox haute disponibilité sur un seul PC portable

![Proxmox VE](https://img.shields.io/badge/Proxmox_VE-9.2-E57000?logo=proxmox&logoColor=white)
![Debian](https://img.shields.io/badge/Debian-12_Bookworm-A81D33?logo=debian&logoColor=white)
![VMware Workstation](https://img.shields.io/badge/VMware_Workstation-Pro-607078?logo=vmware&logoColor=white)
![Cluster](https://img.shields.io/badge/cluster-2_n%C5%93uds_%2B_QDevice-1F6FEB)
![Stockage](https://img.shields.io/badge/stockage-NFS_partag%C3%A9-6E40C9)
![Statut](https://img.shields.io/badge/statut-termin%C3%A9-brightgreen)

> **En une phrase :** sur mon PC portable, dans VMware Workstation, j'ai monté un **cluster Proxmox VE** de deux serveurs avec un **arbitre** (QDevice) et un **stockage partagé NFS**. J'y ai ensuite testé la migration à chaud, la haute disponibilité (en coupant un serveur), les snapshots, les clones et les droits d'accès.

Ce dépôt est écrit pour être compris **même sans rien connaître** à Proxmox : tous les mots techniques sont expliqués.<br>
👉 Le compte-rendu complet, étape par étape et avec toutes les captures, est dans [`RAPPORT.md`](RAPPORT.md).

Projet réalisé pendant la semaine PPE du 9 au 13 novembre 2026, module **Virtualisation (CFC 190)**, Geneva Institute of Technology, classe E1B.

## 🧱 Ce que j'ai construit

| Machine | Rôle | Adresses |
|---|---|---|
| **pve-01** | Nœud Proxmox (hyperviseur) | 10.10.10.21 / 10.10.30.21 |
| **pve-02** | Nœud Proxmox (hyperviseur) | 10.10.10.23 / 10.10.30.23 |
| **qdevice-01** | Arbitre du quorum (Debian 12) | 10.10.10.14 |
| **nas-01** | Stockage partagé NFS (Debian 12) : `ds01` et `ds02` | 10.10.10.50 |
| **vm-deb-pve-01** | VM de test (Debian 12, cloud-init, agent QEMU) | 10.10.20.31 |

Trois réseaux séparés, qui jouent le rôle de VLAN : **management** (10.10.10.0/24), **VM** (10.10.20.0/24) et **cluster et migration** (10.10.30.0/24).

## ✅ Ce qui a été testé, avec les résultats

| Test | Résultat |
|---|---|
| Cluster + quorum | `Quorate: Yes`, 3 votes (2 nœuds + QDevice) |
| Migration à chaud, aller | 9 s, 45 ms de gel, **0 % de perte** de ping |
| Migration à chaud, retour | 16 s, 38 ms de gel, **0 % de perte** de ping |
| Déplacement de disque à chaud (20 Go) | 21 min, 0 % de perte, pic de latence à 460 ms |
| Haute disponibilité (coupure brutale de pve-02) | VM redémarrée seule sur pve-01 en **≈ 3 min 10 s** |
| Snapshot + retour arrière | OK (gel propre du disque grâce à l'agent QEMU) |
| Clone complet / clone lié depuis un template | 24 s / moins d'1 s |
| Compte en lecture seule (PVEAuditor) | voit tout, ne peut rien modifier |

Les pannes rencontrées (kernel panic en émulation, carte réseau e1000 du NAS bloquée, cluster qui « clignote » sous forte charge…) sont documentées et expliquées dans le rapport, dans la section **Problèmes rencontrés**.

## 📁 Contenu du dépôt

- `RAPPORT.md` : le rapport complet (également publié sur HackMD)
- `captures/` : les captures d'écran du rapport

---

**Auteur :** Paul Giocanti ([@L4ami](https://github.com/L4ami)) · Novembre 2026<br>
CFC d'informaticien + Bachelor (double certification) · Geneva Institute of Technology
