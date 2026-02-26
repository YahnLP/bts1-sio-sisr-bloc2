---
author: YLP
title: 📝 DOCUMENTATION
---

# 📄 FICHE DE DOCUMENTATION TECHNIQUE — PROJET 1
## (À compléter en temps réel pendant le TP — Versée au Portfolio)

*Nom : _________________________ Prénom : _________________________ Date : _________*
*Groupe : _________________________ Version : 1.0*

---

## SECTION 1 — Plan d'Adressage VLSM

*(Reproduire le tableau complété de la Phase 1)*

| **VLAN** | **Nom** | **Besoin** | **Réseau /n** | **Masque** | **Passerelle** | **Plage DHCP** |
|---|---|---|---|---|---|---|
| 10 | RH | 25 | | | | |
| 20 | Informatique | 10 | | | | |
| 30 | Direction | 5 | | | | |
| 40 | Commercial | 40 | | | | |
| 50 | Comptabilité | 15 | | | | |
| 99 | Serveurs | 10 | | | Fixe | N/A |

**Justification des choix de masque :**
*(Expliquer en 4-6 lignes pourquoi ces tailles de sous-réseaux ont été choisies)*

---

## SECTION 2 — Configuration Réseau

### Switch SW-BatA

| **Port** | **VLAN** | **Mode** |
|---|---|---|
| Fa0/1 | | |
| Fa0/2 | | |
| Fa0/3 | | |
| Gig0/1 | Trunk | Trunk → Routeur |
| Gig0/2 | Trunk | Trunk → SW-BatB |

### Routeur — Sous-interfaces

| **Sous-interface** | **Encapsulation** | **Adresse IP** | **VLAN** |
|---|---|---|---|
| G0/0.10 | dot1Q 10 | | VLAN 10 RH |
| G0/0.20 | dot1Q 20 | | VLAN 20 IT |
| G0/0.30 | dot1Q 30 | | VLAN 30 Dir |
| G0/0.40 | dot1Q 40 | | VLAN 40 Com |
| G0/0.50 | dot1Q 50 | | VLAN 50 Compta |
| G0/0.99 | dot1Q 99 | | VLAN 99 Serv |

---

## SECTION 3 — Active Directory

### Structure des OUs

```
siosarl.local
    ├── RH
    │   ├── Utilisateurs
    │   └── Ordinateurs
    ├── Informatique
    │   ├── Utilisateurs
    │   └── Ordinateurs
    ├── Direction / Comptabilite / Commercial
    │   (même structure)
    └── [Autres OUs créées]
```

### GPO Déployées

| **Nom GPO** | **Liée à** | **Paramètre principal** | **Effet** |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

---

## SECTION 4 — DHCP

| **Étendue** | **Réseau** | **Plage distribuée** | **Options (GW/DNS)** | **Durée bail** |
|---|---|---|---|---|
| VLAN 10 | | | | |
| VLAN 20 | | | | |
| VLAN 30 | | | | |
| VLAN 40 | | | | |
| VLAN 50 | | | | |

---

## SECTION 5 — Serveur FTP/SFTP Linux

| **Paramètre** | **Valeur** |
|---|---|
| Adresse IP du serveur | 192.168.0.147 |
| Distribution | Debian 12 |
| Logiciel FTP | vsftpd |
| Répertoire FTP anonyme | /srv/ftp/public |
| Accès anonyme — Droits | Lecture seule |
| Répertoire SFTP | /srv/sftp/ |
| Groupe SFTP | sftpusers |
| Shell des utilisateurs SFTP | /usr/sbin/nologin |

### Utilisateurs SFTP Créés

| **Login** | **Service** | **Répertoire chroot** | **Peut se connecter SSH ?** |
|---|---|---|---|
| sftp_rh | RH | /srv/sftp/sftp_rh | Non |
| sftp_informatique | IT | /srv/sftp/sftp_informatique | Non |
| sftp_direction | Direction | /srv/sftp/sftp_direction | Non |

---

## SECTION 6 — Tests de Validation

| **Test** | **Commande / Méthode** | **Résultat** | **Capture** |
|---|---|---|---|
| Ping inter-VLAN (PC-RH → PC-Commercial) | `ping` | ✅ / ❌ | P1 |
| Structure AD visible dans ADUC | Console ADUC | ✅ / ❌ | P2 |
| GPO créées et liées | `gpmc.msc` | ✅ / ❌ | P3 |
| Partages accessibles | `\\siosarl.local\data` | ✅ / ❌ | P4 |
| FTP anonyme — lecture OK | `ftp [IP]` + `get` | ✅ / ❌ | P5 |
| SFTP authentifié — upload OK | `sftp sftp_rh@[IP]` + `put` | ✅ / ❌ | P6 |
| SSH refusé pour sftpusers | `ssh sftp_rh@[IP]` | ✅ / ❌ | P7 |

---

---
