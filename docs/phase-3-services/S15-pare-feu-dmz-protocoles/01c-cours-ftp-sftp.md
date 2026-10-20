# Cours – Transfert de fichiers : FTP et SFTP

*BTS SIO SISR 1re année – Bloc 2 – S15*

## I. FTP et SFTP — Deux Protocoles pour le Transfert de Fichiers

### I.A. FTP — File Transfer Protocol

**FTP** (RFC 959) est un protocole de transfert de fichiers entre un client et un serveur. Il utilise **deux connexions TCP** :

| **Connexion** | **Port** | **Rôle** |
|---|---|---|
| **Contrôle** | TCP 21 | Envoi des commandes (login, liste, navigation) |
| **Données** | TCP 20 (actif) ou aléatoire (passif) | Transfert effectif des fichiers |

<figure class="graphe diagramme"><svg viewBox="0 0 640 376" style="max-width:640px" role="img" aria-label="Session FTP : connexion de contrôle puis connexion de données" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="155.0" y="30.0" text-anchor="middle" class="dg-w">Client FTP</text>
<line x1="155.0" y1="42" x2="155.0" y2="368" class="dg-dash"/>
<rect x="400.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="485.0" y="30.0" text-anchor="middle" class="dg-w">Serveur FTP</text>
<line x1="485.0" y1="42" x2="485.0" y2="368" class="dg-dash"/>
<text x="320.0" y="64" text-anchor="middle" class="dg-t">Connexion de contrôle (port 21)</text>
<line x1="155.0" y1="72" x2="476.0" y2="72.0" class="dg-line"/>
<polygon points="485.0,72 476.0,67.0 476.0,77.0" class="dg-ah"/>
<text x="320.0" y="98" text-anchor="middle" class="dg-t">Authentification demandée</text>
<line x1="485.0" y1="106" x2="164.0" y2="106.0" class="dg-dash"/>
<polygon points="155.0,106 164.0,111.0 164.0,101.0" class="dg-ah"/>
<text x="320.0" y="132" text-anchor="middle" class="dg-t">USER alice / PASS ...</text>
<line x1="155.0" y1="140" x2="476.0" y2="140.0" class="dg-line"/>
<polygon points="485.0,140 476.0,135.0 476.0,145.0" class="dg-ah"/>
<text x="320.0" y="166" text-anchor="middle" class="dg-t">230 Login successful</text>
<line x1="485.0" y1="174" x2="164.0" y2="174.0" class="dg-dash"/>
<polygon points="155.0,174 164.0,179.0 164.0,169.0" class="dg-ah"/>
<text x="320.0" y="200" text-anchor="middle" class="dg-t">LIST (lister les fichiers)</text>
<line x1="155.0" y1="208" x2="476.0" y2="208.0" class="dg-line"/>
<polygon points="485.0,208 476.0,203.0 476.0,213.0" class="dg-ah"/>
<text x="320.0" y="234" text-anchor="middle" class="dg-t">Connexion de données (port 20)</text>
<line x1="485.0" y1="242" x2="164.0" y2="242.0" class="dg-dash"/>
<polygon points="155.0,242 164.0,247.0 164.0,237.0" class="dg-ah"/>
<text x="320.0" y="268" text-anchor="middle" class="dg-t">Contenu du répertoire</text>
<line x1="485.0" y1="276" x2="164.0" y2="276.0" class="dg-dash"/>
<polygon points="155.0,276 164.0,281.0 164.0,271.0" class="dg-ah"/>
<text x="320.0" y="302" text-anchor="middle" class="dg-t">RETR fichier.txt</text>
<line x1="155.0" y1="310" x2="476.0" y2="310.0" class="dg-line"/>
<polygon points="485.0,310 476.0,305.0 476.0,315.0" class="dg-ah"/>
<text x="320.0" y="336" text-anchor="middle" class="dg-t">Données du fichier</text>
<line x1="485.0" y1="344" x2="164.0" y2="344.0" class="dg-dash"/>
<polygon points="155.0,344 164.0,349.0 164.0,339.0" class="dg-ah"/>
</svg>
</figure>

*Légende : Fonctionnement FTP avec ses deux connexions. La connexion de contrôle (port 21) reste ouverte pendant toute la session et transporte les commandes textuelles. Pour chaque transfert de fichier ou listage, une connexion de données séparée est établie (port 20 en mode actif, ou port aléatoire en mode passif).*

> **Sécurité FTP :** Le FTP transmet les données **en clair**, y compris l'identifiant et le mot de passe. Il ne doit **jamais** être utilisé pour des données sensibles sur un réseau non maîtrisé. En production, on lui préfère systématiquement SFTP ou FTPS.

---

### I.B. SFTP — SSH File Transfer Protocol

**SFTP** n'est pas FTP sur SSL — c'est un protocole **entièrement distinct**, conçu pour fonctionner comme **sous-système de SSH**. Toutes les données transitent dans le tunnel SSH chiffré.

| **Critère** | **FTP** | **SFTP** |
|---|---|---|
| **Port** | 21 (contrôle) + 20 (données) | 22 (SSH uniquement) |
| **Chiffrement** | Aucun (texte clair) | Intégral (AES, ChaCha20) |
| **Authentification** | Login/Mot de passe (clair) | Mot de passe chiffré ou **clés SSH** |
| **Pare-feu** | Difficile (2 ports, passif = ports aléatoires) | Simple (1 seul port TCP 22) |
| **Intégrité des données** | Non garantie | Garantie (MAC SSH) |
| **Usage recommandé** | Réseau interne maîtrisé uniquement | Tout réseau |

 **Lien ITIL — Gestion de la Sécurité des Informations :** Le choix entre FTP et SFTP est une décision de sécurité. En ITIL 4, la pratique de **Gestion de la Sécurité des Informations** impose de choisir des protocoles conformes au niveau de sensibilité des données échangées. SFTP est le choix par défaut dès que les fichiers transférés sont liés à l'activité de l'entreprise.

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

> **Contrainte critique du chroot :** Le répertoire de chroot (racine de l'utilisateur) **doit appartenir à root** et ne pas être accessible en écriture à l'utilisateur lui-même. C'est une exigence de sécurité OpenSSH. L'utilisateur écrit dans un **sous-répertoire** du chroot.

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
