# Annexe – Modèle de DAT

*BTS SIO SISR 1re année – Bloc 2 – S18*

## PAGE DE GARDE

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│         DOSSIER D'ARCHITECTURE TECHNIQUE (DAT)                  │
│                                                                 │
│         Projet : Infrastructure informatique SimIO SARL         │
│                                                                 │
│         Client    : SimIO SARL                                  │
│         Prestataire : [Vos noms]                                │
│                                                                 │
│         Version : 1.0                                           │
│         Date    : [Date de S18]                                 │
│         Statut  : LIVRÉ                                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Tableau de Gestion des Versions

| **Version** | **Date** | **Auteur(s)** | **Modifications** |
|---|---|---|---|
| 0.1 | S17 | [Noms] | Création initiale — plan d'adressage et schéma réseau |
| 0.2 | S17 | [Noms] | Ajout Windows Server, Linux FTP/SFTP |
| 1.0 | S18 | [Noms] | Version finale — tests validés, document complet |

---

## Sommaire

1. Contexte et Objectifs
2. Architecture Globale
3. Architecture Réseau
   - 3.1 Plan d'Adressage VLSM
   - 3.2 Schéma Réseau
   - 3.3 Configuration des Équipements Réseau
4. Architecture Système
   - 4.1 Windows Server — AD DS, DHCP, DNS, Partages
   - 4.2 Linux Debian — FTP/SFTP
5. Sécurité
6. Procédures d'Exploitation
7. Plan de Tests et Résultats
8. Annexes

---

## 1. Contexte et Objectifs

SimIO SARL est une PME de 80 collaborateurs répartis sur 2 bâtiments. [Compléter avec la présentation du contexte du projet].

**Objectifs du projet :**
- Segmenter le réseau en VLANs par service pour améliorer la sécurité et les performances
- Déployer une infrastructure Active Directory centralisée
- Mettre en place des services réseaux (DHCP, DNS) et des partages de fichiers
- Fournir un accès FTP anonyme pour les ressources internes et un accès SFTP sécurisé par service

**Périmètre :**
- Inclus : réseau local, AD DS, DHCP, DNS, partages SMB, FTP/SFTP
- Exclu : accès Internet, messagerie, téléphonie, infrastructure de sauvegarde

---

## 2. Architecture Globale

### 2.1 Inventaire Matériel

| **Équipement** | **Rôle** | **Localisation** | **Adresse IP** |
|---|---|---|---|
| SW-BatA | Switch principal Bât. A | Bât. A — salle réseau | — (adresse de gestion si configurée) |
| SW-BatB | Switch Bât. B | Bât. B — armoire réseau | — |
| RTR-SIOSARL | Routeur inter-VLAN | Bât. A — salle réseau | Voir sous-interfaces |
| SRV-AD01 | Contrôleur de domaine | Bât. A — salle serveurs | 192.168.0.145 |
| SRV-FTP | Serveur FTP/SFTP Linux | Bât. A — salle serveurs | 192.168.0.147 |

### 2.2 Inventaire Logiciel

| **Logiciel** | **Version** | **Rôle** | **Serveur** |
|---|---|---|---|
| Windows Server | 2022 | AD DS, DNS, DHCP, Partages | SRV-AD01 |
| Debian | 12 | FTP/SFTP | SRV-FTP |
| vsftpd | [version] | Serveur FTP anonyme | SRV-FTP |
| OpenSSH | [version] | Accès SFTP chiffré | SRV-FTP |

---

*(Les sections 3 à 8 sont à compléter par le binôme en reprenant la documentation réalisée pendant les TP S17 et S18)*

---
