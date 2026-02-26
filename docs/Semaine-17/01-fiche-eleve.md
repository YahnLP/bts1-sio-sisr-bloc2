---
author: YLP
title: 📚 FICHE DE COURS
---

# 📚 FICHE DE COURS ÉLÈVE
## "FTP / SFTP — Transfert de Fichiers Sécurisé sur Linux"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 17*
*(Note : les parties réseau, AD, DHCP, DNS, partages sont couvertes dans les fiches S10-S16)*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.2** | Installer et configurer un service réseau (FTP/SFTP) |
| **B3.2** | Mettre en œuvre et maintenir la sécurité informatique |
| **B2.3** | Assurer la sécurité des accès aux ressources |

---

## I. FTP et SFTP — Deux Protocoles pour le Transfert de Fichiers

### I.A. FTP — File Transfer Protocol

**FTP** (RFC 959) est un protocole de transfert de fichiers entre un client et un serveur. Il utilise **deux connexions TCP** :

| **Connexion** | **Port** | **Rôle** |
|---|---|---|
| **Contrôle** | TCP 21 | Envoi des commandes (login, liste, navigation) |
| **Données** | TCP 20 (actif) ou aléatoire (passif) | Transfert effectif des fichiers |

```
   CLIENT FTP                          SERVEUR FTP
        │                                    │
        │─── Connexion contrôle (port 21) ──►│
        │◄─── Authentification demandée ──────│
        │─── USER alice / PASS secret ───────►│
        │◄─── 230 Login successful ──────────│
        │                                    │
        │─── LIST (lister les fichiers) ─────►│
        │◄─── Connexion données (port 20) ───│
        │◄─── Contenu du répertoire ─────────│
        │                                    │
        │─── RETR fichier.txt ────────────────►│
        │◄─── Données du fichier ─────────────│
```

*Légende : Fonctionnement FTP avec ses deux connexions. La connexion de contrôle (port 21) reste ouverte pendant toute la session et transporte les commandes textuelles. Pour chaque transfert de fichier ou listage, une connexion de données séparée est établie (port 20 en mode actif, ou port aléatoire en mode passif).*

> ⚠️ **Sécurité FTP :** Le FTP transmet les données **en clair**, y compris l'identifiant et le mot de passe. Il ne doit **jamais** être utilisé pour des données sensibles sur un réseau non maîtrisé. En production, on lui préfère systématiquement SFTP ou FTPS.

---

### I.B. SFTP — SSH File Transfer Protocol

**SFTP** n'est pas FTP sur SSL — c'est un protocole **entièrement distinct**, conçu pour fonctionner comme **sous-système de SSH**. Toutes les données transitent dans le tunnel SSH chiffré.

| **Critère** | **FTP** | **SFTP** |
|---|---|---|
| **Port** | 21 (contrôle) + 20 (données) | 22 (SSH uniquement) |
| **Chiffrement** | ❌ Aucun (texte clair) | ✅ Intégral (AES, ChaCha20) |
| **Authentification** | Login/Mot de passe (clair) | Mot de passe chiffré ou **clés SSH** |
| **Pare-feu** | Difficile (2 ports, passif = ports aléatoires) | Simple (1 seul port TCP 22) |
| **Intégrité des données** | Non garantie | Garantie (MAC SSH) |
| **Usage recommandé** | Réseau interne maîtrisé uniquement | Tout réseau |

💡 **Lien ITIL — Gestion de la Sécurité des Informations :** Le choix entre FTP et SFTP est une décision de sécurité. En ITIL 4, la pratique de **Gestion de la Sécurité des Informations** impose de choisir des protocoles conformes au niveau de sensibilité des données échangées. SFTP est le choix par défaut dès que les fichiers transférés sont liés à l'activité de l'entreprise.

---

### I.C. vsftpd — Le Serveur FTP/SFTP sur Linux

**vsftpd** (Very Secure FTP Daemon) est le serveur FTP le plus répandu sur les distributions Linux. Malgré son nom, il gère aussi le mode SFTP via SSH.

**Fichiers importants :**

| **Chemin** | **Rôle** |
|---|---|
| `/etc/vsftpd.conf` | Fichier de configuration principal |
| `/etc/ftpusers` | Liste des utilisateurs **interdits** de FTP |
| `/etc/vsftpd.userlist` | Liste des utilisateurs **autorisés** (si activé) |
| `/var/log/vsftpd.log` | Journal d'accès FTP |
| `/srv/ftp/` | Répertoire racine FTP anonyme (par convention) |

---

### I.D. Paramètres Clés de vsftpd

```bash
# /etc/vsftpd.conf — Paramètres essentiels et leur signification

# ─── FTP anonyme ──────────────────────────────────────────────────────
anonymous_enable=YES          # Autoriser les connexions sans authentification
anon_root=/srv/ftp/public     # Répertoire racine de l'utilisateur anonyme
no_anon_password=YES          # Pas de mot de passe pour l'anonyme
anon_upload_enable=NO         # Interdire l'upload anonyme
anon_mkdir_write_enable=NO    # Interdire la création de dossiers en anonyme

# ─── FTP authentifié ──────────────────────────────────────────────────
local_enable=YES              # Autoriser les utilisateurs locaux Linux
write_enable=YES              # Autoriser l'écriture (upload, mkdir...)
local_umask=022               # Masque de permissions pour les fichiers créés

# ─── Chroot (isolation de l'utilisateur) ─────────────────────────────
chroot_local_user=YES         # Confiner chaque utilisateur dans son home
allow_writeable_chroot=YES    # Permettre l'écriture dans le répertoire confiné
                              # (ou mieux : créer un sous-répertoire writable)

# ─── Mode passif (recommandé pour les pare-feux) ──────────────────────
pasv_enable=YES
pasv_min_port=10000
pasv_max_port=10100

# ─── Sécurité ─────────────────────────────────────────────────────────
userlist_enable=YES           # Activer la liste blanche d'utilisateurs
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO              # NO = la liste est une liste BLANCHE (autorisés)
                              # YES = la liste est une liste NOIRE (interdits)
xferlog_enable=YES            # Activer le journal de transfert
log_ftp_protocol=YES          # Journaliser le protocole complet
```

---

### I.E. SFTP avec Restriction Chroot via SSH

Le moyen le plus sécurisé de proposer un accès SFTP est de l'intégrer directement dans OpenSSH avec un **chroot jail** : l'utilisateur est **enfermé dans son répertoire** et ne peut pas naviguer dans le reste du système de fichiers.

Configuration dans `/etc/ssh/sshd_config` :

```
# ─── À la FIN du fichier sshd_config ──────────────────────────────────

# Groupe dont les membres auront accès SFTP uniquement (pas SSH)
Match Group sftpusers
    # Forcer l'utilisation du sous-système SFTP interne
    ForceCommand internal-sftp
    # Confiner l'utilisateur dans son répertoire (chroot)
    ChrootDirectory /srv/sftp/%u
    # Interdire le transfert X11 et le port forwarding
    X11Forwarding no
    AllowTcpForwarding no
```

**Structure des répertoires pour le chroot SFTP :**

```
/srv/sftp/
    ├── alice/                   ← Répertoire racine (chroot) de alice
    │   │                           DOIT appartenir à root:root, chmod 755
    │   └── upload/              ← Répertoire où alice peut écrire
    │                                Propriétaire : alice:alice, chmod 755
    │
    ├── bob/                     ← Répertoire racine (chroot) de bob
    │   └── upload/
    │
    └── commun/                  ← Pour un groupe partagé
        └── partage/
```

> ⚠️ **Contrainte critique du chroot :** Le répertoire de chroot (racine de l'utilisateur) **doit appartenir à root** et ne pas être accessible en écriture à l'utilisateur lui-même. C'est une exigence de sécurité OpenSSH. L'utilisateur écrit dans un **sous-répertoire** du chroot.

---

## II. Rappel Structuré — Plan d'Adressage VLSM

*(Rappel S4-S5 appliqué au projet S17)*

### II.A. Principe du VLSM

Le **VLSM** (Variable Length Subnet Masking) permet d'attribuer des sous-réseaux de **tailles différentes** à partir d'un même bloc d'adresses, en ajustant le masque selon le nombre réel d'hôtes nécessaires. Contrairement au découpage fixe (même masque pour tout), le VLSM **optimise l'utilisation des adresses**.

**Règle de calcul :** Pour héberger N hôtes, il faut un sous-réseau avec au moins N+2 adresses (une adresse réseau + une adresse broadcast + N hôtes). On prend la puissance de 2 supérieure ou égale à N+2.

```
Formule : 2^n ≥ N + 2  →  choisir la plus petite valeur de n satisfaisant la condition

Exemples :
  25 hôtes → 2^5 = 32 ≥ 27 → /27 (30 hôtes utilisables)
  40 hôtes → 2^6 = 64 ≥ 42 → /26 (62 hôtes utilisables)
  10 hôtes → 2^4 = 16 ≥ 12 → /28 (14 hôtes utilisables)
   5 hôtes → 2^3 =  8 ≥  7 → /29  (6 hôtes utilisables)
```

### II.B. Tableau de Référence des Masques

| **CIDR** | **Masque décimal** | **Hôtes utilisables** | **Adresses totales** |
|---|---|---|---|
| /24 | 255.255.255.0 | 254 | 256 |
| /25 | 255.255.255.128 | 126 | 128 |
| /26 | 255.255.255.192 | 62 | 64 |
| /27 | 255.255.255.224 | 30 | 32 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |

### II.C. Méthode de Découpage VLSM

**Règle d'or :** Toujours allouer les **plus grands sous-réseaux en premier** pour éviter le chevauchement.

Avec le bloc `192.168.0.0/22` (1024 adresses, de 192.168.0.0 à 192.168.3.255) :

| **Ordre** | **VLAN** | **Besoin** | **Taille bloc** | **Réseau alloué** | **Masque** | **Plage hôtes** | **Broadcast** |
|---|---|---|---|---|---|---|---|
| 1 | VLAN 40 Commercial | 40 h | /26 (64) | 192.168.0.0/26 | 255.255.255.192 | .1 → .62 | .63 |
| 2 | VLAN 10 RH | 25 h | /27 (32) | 192.168.0.64/27 | 255.255.255.224 | .65 → .94 | .95 |
| 3 | VLAN 50 Compta | 15 h | /27 (32) | 192.168.0.96/27 | 255.255.255.224 | .97 → .126 | .127 |
| 4 | VLAN 20 Info | 10 h | /28 (16) | 192.168.0.128/28 | 255.255.255.240 | .129 → .142 | .143 |
| 5 | VLAN 99 Serveurs | 10 h | /28 (16) | 192.168.0.144/28 | 255.255.255.240 | .145 → .158 | .159 |
| 6 | VLAN 30 Direction | 5 h | /29 (8) | 192.168.0.160/29 | 255.255.255.248 | .161 → .166 | .167 |

> *Adresses restantes du bloc /22 : disponibles pour extensions futures*

---

## III. Rappel Structuré — GPO (Group Policy Objects)

*(Rappel S12 appliqué au projet S17)*

### III.A. Qu'est-ce qu'une GPO ?

Une **GPO** (Group Policy Object) est un ensemble de paramètres de configuration appliqués automatiquement aux ordinateurs et/ou utilisateurs d'une OU Active Directory. Elles permettent de **standardiser et contrôler l'environnement de travail** sans intervention sur chaque poste.

**Types de paramètres GPO :**

| **Catégorie** | **Exemples** |
|---|---|
| **Configuration ordinateur** | Scripts de démarrage, politiques de mot de passe, pare-feu, mises à jour |
| **Configuration utilisateur** | Scripts de connexion, redirection de dossiers, mappages lecteurs réseau |
| **Restrictions utilisateur** | Interdire le panneau de configuration, masquer des lecteurs, bloquer PowerShell |

### III.B. Commandes PowerShell pour les GPO

```powershell
# Importer le module de gestion des GPO
Import-Module GroupPolicy

# Lister toutes les GPO du domaine
Get-GPO -All

# Créer une nouvelle GPO
New-GPO -Name "Restriction_Utilisateurs_Standards"

# Lier une GPO à une OU
New-GPLink -Name "Restriction_Utilisateurs_Standards" -Target "OU=RH,DC=siosarl,DC=local"

# Vérifier les GPO appliquées (depuis un poste client)
gpresult /r
gpresult /h rapport_gpo.html    # Rapport HTML détaillé

# Forcer la mise à jour des GPO sur le poste courant
gpupdate /force
```

### III.C. GPO du Projet SimIO — Paramètres Requis

| **GPO** | **Liée à** | **Paramètre** | **Chemin dans l'éditeur GPO** |
|---|---|---|---|
| `GPO_Restriction_Standard` | OU RH, Commercial, Compta | Interdire le panneau de configuration | Config. utilisateur → Modèles d'administration → Panneau de configuration → Interdire l'accès... |
| `GPO_Lecteurs_Reseau` | Toutes OUs utilisateurs | Mapper `Z:` vers `\\siosarl.local\data\[Service]` | Config. utilisateur → Préférences Windows → Paramètres Windows → Mappages de lecteurs |
| `GPO_Fond_Ecran` | OU Domaine complet | Fond d'écran SimIO | Config. utilisateur → Modèles d'admin → Bureau → Bureau Active Desktop |
| `GPO_Securite_Mdp` | Domaine entier | Longueur min 10, complexité activée | Config. ordinateur → Paramètres Windows → Paramètres de sécurité → Stratégies de compte |

---

## IV. Vocabulaire Clé — FTP/SFTP

| **Terme** | **Définition** |
|-----------|---------------|
| **FTP** | File Transfer Protocol — protocole de transfert de fichiers non chiffré (ports 21/20) |
| **SFTP** | SSH File Transfer Protocol — transfert de fichiers via tunnel SSH chiffré (port 22) |
| **FTPS** | FTP Secure — FTP avec chiffrement TLS/SSL (ne pas confondre avec SFTP) |
| **Mode actif** | Le serveur FTP initie la connexion de données vers le client (port 20) |
| **Mode passif** | Le client initie la connexion de données (port aléatoire côté serveur) |
| **vsftpd** | Very Secure FTP Daemon — serveur FTP/FTPS open source pour Linux |
| **Chroot jail** | Mécanisme enfermant un utilisateur dans un répertoire racine virtuel |
| **Utilisateur anonyme** | Connexion FTP sans authentification (login "anonymous" ou "ftp") |
| **`ForceCommand internal-sftp`** | Directive SSH forçant l'utilisateur à utiliser SFTP (interdisant le shell SSH) |
| **ChrootDirectory** | Directive SSH définissant le répertoire racine pour le chroot SFTP |
| **`sftpusers`** | Groupe Linux regroupant les utilisateurs ayant accès SFTP uniquement |

---
