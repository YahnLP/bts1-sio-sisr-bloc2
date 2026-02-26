---
author: YLP
title: 📚 FICHE DE COURS
---

## 📚 FICHE DE COURS ÉLÈVE
### "Commutation, Cisco CLI et Linux : Commandes de Base"

*Version 1.0 - BTS SIO SISR - Semestre 1 - Semaine 6*

---

### 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|----------------|
| **B2.2** | Installer, tester et déployer une solution d'infrastructure réseau - Configurer un switch Cisco |
| **B2.3** | Exploiter, dépanner et superviser - Commandes show, diagnostic commutation |
| **B2.1** | Administrer un système d'exploitation - Commandes Linux fondamentales |

---

### I. La Commutation : Fonctionnement d'un Switch

#### A. Rôle du Switch (Rappel S4)

**Switch (Commutateur)** = Équipement de couche **2** (Liaison de données) du modèle OSI.

**Rôle :** Interconnecter des machines dans un **même réseau local (LAN)** en utilisant les **adresses MAC**.

**Analogie :** Un switch est comme un **central téléphonique intelligent** d'entreprise. Il sait sur quelle ligne se trouve chaque poste (MAC → Port) et établit des connexions directes, sans déranger les autres.

---

#### B. La Table MAC (CAM Table)

**Définition :** Table interne du switch qui **associe chaque adresse MAC au port physique** sur lequel elle a été détectée.

**Caractéristiques :**
- Construite **automatiquement** (self-learning = apprentissage automatique)
- Stockée en mémoire RAM (volatile)
- Chaque entrée a une **durée de vie** (aging time, défaut Cisco = 300 secondes)

**Commande Cisco pour l'afficher :**
```
Switch# show mac address-table
```

**Exemple de résultat :**
```
          Mac Address Table
-------------------------------------------
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    aaaa.aa00.0001    DYNAMIC     Fa0/1
   1    bbbb.bb00.0002    DYNAMIC     Fa0/2
   1    cccc.cc00.0003    DYNAMIC     Fa0/3
Total Mac Addresses for this criterion: 3
```

**Colonnes :**
- **Vlan** : VLAN auquel appartient l'entrée (VLAN 1 = défaut)
- **Mac Address** : Adresse MAC de la machine
- **Type** : DYNAMIC (apprise automatiquement) ou STATIC (configurée manuellement)
- **Ports** : Port physique du switch (Fa0/1 = FastEthernet port 1)

---

#### C. Processus d'Apprentissage et de Décision

Le switch traite chaque trame en **2 étapes systématiques** :

##### **Étape 1 : Apprentissage (Source MAC)**

1. Le switch reçoit une trame sur le **Port X**
2. Il lit l'**adresse MAC Source** de la trame
3. Il enregistre (ou met à jour) dans sa Table MAC :
   → *"MAC Source est joignable via Port X"*

##### **Étape 2 : Décision (Destination MAC)**

Le switch lit l'**adresse MAC Destination** et prend l'une de 3 décisions :

| **Situation** | **Décision** | **Description** |
|---------------|--------------|-----------------|
| MAC Destination **connue** dans la table | **FORWARDING** | Envoie la trame **uniquement** sur le port correspondant |
| MAC Destination **inconnue** dans la table | **FLOODING** | Envoie la trame sur **tous les ports** sauf celui de réception |
| MAC Destination = `FF:FF:FF:FF:FF:FF` (broadcast) | **FLOODING** | Envoie sur **tous les ports** sans exception (toujours) |

---

**Schéma du processus complet :**

```
Trame reçue sur Port 3
        │
        ▼
┌───────────────────────────────────────────────┐
│  APPRENTISSAGE                                │
│  Note MAC Source → Port 3 dans la table       │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
              MAC Destination =
              FF:FF:FF:FF:FF:FF ?
                /         \
              OUI          NON
               │             │
               │      MAC Destination
               │       connue dans table ?
               │          /      \
               │        OUI      NON
               │         │        │
               ▼         ▼        ▼
            FLOOD     FORWARD   FLOOD
          (tous ports)(1 port) (tous ports)
```

---

#### D. Évolution : Hub → Switch

**Hub (Concentrateur) - Obsolète :**

Le hub est un équipement de **couche 1** (physique). Il n'a aucune intelligence : il **répète le signal électrique sur tous les ports** sans exception, quelles que soient les adresses.

```
Hub - PC1 envoie à PC3 :
Port 1 [PC1] → Signal répété → Port 2 [PC2] ⚠️ (reçoit inutilement)
                             → Port 3 [PC3] ✅
                             → Port 4 [PC4] ⚠️ (reçoit inutilement)
```

**Switch - Moderne :**

```
Switch - PC1 envoie à PC3 (MAC connue) :
Port 1 [PC1] → FORWARDING → Port 3 [PC3] ✅
               (Port 2 et 4 ne reçoivent rien)
```

| **Critère** | **HUB** | **SWITCH** |
|-------------|---------|------------|
| Couche OSI | 1 (Physique) | 2 (Liaison) |
| Intelligence | ❌ Aucune | ✅ Table MAC |
| Sécurité | ❌ Toutes les trames visibles par tous | ✅ Forwarding ciblé |
| Performances | ❌ Dégradées avec le trafic | ✅ Débit dédié par port |
| Usage actuel | ❌ **Obsolète** | ✅ **Standard** |

---

#### E. Domaines de Collision et de Diffusion

##### 🔹 **Domaine de Collision (Collision Domain)**

**Définition :** Zone où deux transmissions simultanées provoquent une **collision** (corruption des signaux).

**Avec un Hub :**
```
[PC1]─┬─[PC2]─┬─[PC3]─┬─[PC4]
      └───── HUB ───────┘
→ 1 seul domaine de collision (tous les PC)
→ Si PC1 et PC2 émettent en même temps : COLLISION
→ Les deux doivent attendre un délai aléatoire et réémettre (CSMA/CD)
```

**Avec un Switch :**
```
[PC1]─Fa0/1─┐
[PC2]─Fa0/2─┤
[PC3]─Fa0/3─┤  [SWITCH]
[PC4]─Fa0/4─┘
→ 1 domaine de collision PAR PORT (full-duplex)
→ PC1 et PC2 peuvent émettre simultanément sans collision
```

💡 **Résumé :** Un switch avec **N ports** = **N domaines de collision** (1 par port).

---

##### 🔹 **Domaine de Diffusion (Broadcast Domain)**

**Définition :** Zone où une trame de diffusion (`FF:FF:FF:FF:FF:FF`) est **propagée et reçue par tous**.

**Avec un Switch seul :**
```
[PC1]─[PC2]─[SWITCH]─[PC3]─[PC4]
→ 1 seul domaine de diffusion
→ Un broadcast de PC1 est reçu par PC2, PC3, PC4
```

**Avec un Routeur :**
```
Réseau A: [PC1]─[PC2]─[SW1]─[ROUTEUR]─[SW2]─[PC3]─[PC4]
→ 2 domaines de diffusion séparés par le routeur
→ Broadcast de PC1 reste dans Réseau A (PC2 uniquement)
→ PC3 et PC4 ne reçoivent pas le broadcast
```

💡 **Règle à retenir :**
- **Switch** : Sépare les domaines de **collision** mais **PAS** les domaines de diffusion
- **Routeur** : Sépare les domaines de **diffusion** (et de collision)

| **Équipement** | **Sépare collisions ?** | **Sépare broadcasts ?** |
|----------------|-------------------------|-------------------------|
| Hub | ❌ Non | ❌ Non |
| Switch | ✅ Oui (1 par port) | ❌ Non |
| Routeur | ✅ Oui | ✅ Oui |

---

### II. La CLI Cisco IOS : Prise en Main

#### A. Accéder à un Équipement Cisco

##### 🔹 **Méthode 1 : Câble Console (accès physique - premier accès)**

**Matériel :**
- **Câble console** : RJ45 → USB (ou ancien DB9/RS-232)
- Logiciel terminal : **PuTTY**, Tera Term, SecureCRT

**Paramètres de connexion PuTTY :**

| **Paramètre** | **Valeur** |
|---------------|------------|
| Type de connexion | **Serial** |
| Port COM | COM3, COM4... (Gestionnaire de périphériques Windows) |
| Vitesse (baud rate) | **9600** |
| Bits de données | 8 |
| Parité | Aucune |
| Bits d'arrêt | 1 |
| Contrôle de flux | Aucun |

**Procédure :**
1. Brancher câble console PC ↔ Switch
2. Ouvrir PuTTY → Serial → COM3 → 9600
3. Appuyer sur **Entrée** → Prompt du switch apparaît

##### 🔹 **Méthode 2 : Packet Tracer (simulation)**

1. Clic sur le switch dans la topologie
2. Onglet **CLI**
3. Appuyer sur **Entrée**
4. Répondre **No** à "Initial configuration dialog?"

---

#### B. Les Modes de la CLI Cisco IOS

La CLI Cisco est organisée en **hiérarchie de modes**. Chaque mode a un **prompt** (invite) distinctif et permet des commandes spécifiques.

```
Switch>                     ← Mode User EXEC
    │  enable
    ▼
Switch#                     ← Mode Privileged EXEC (Enable Mode)
    │  configure terminal (ou conf t)
    ▼
Switch(config)#             ← Mode Global Configuration
    │  interface FastEthernet0/1
    ▼
Switch(config-if)#          ← Mode Interface Configuration
    │  exit (revenir d'un niveau)
    │  end (revenir directement en Privileged)
```

---

| **Mode** | **Prompt** | **Accès** | **Ce qu'on peut faire** |
|----------|------------|-----------|-------------------------|
| **User EXEC** | `Switch>` | Automatique à la connexion | `ping`, `show` limités, `enable` |
| **Privileged EXEC** | `Switch#` | `enable` (+ mot de passe si configuré) | Tous les `show`, `debug`, `copy`, `reload` |
| **Global Config** | `Switch(config)#` | `configure terminal` depuis Privileged | Modifier la config globale (hostname, passwords, banner...) |
| **Interface Config** | `Switch(config-if)#` | `interface <nom>` depuis Global Config | Configurer une interface spécifique |
| **Line Config** | `Switch(config-line)#` | `line console 0` ou `line vty 0 4` | Configurer les accès console et réseau |

---

**Commandes de navigation entre modes :**

```cisco
Switch> enable                ! User → Privileged
Switch# disable               ! Revenir en User EXEC
Switch# configure terminal    ! Privileged → Global Config
Switch(config)# exit          ! Remonter d'UN niveau
Switch(config)# end           ! Retour direct en Privileged (ou Ctrl+Z)
Switch(config-if)# exit       ! Interface Config → Global Config
Switch(config-if)# end        ! Interface Config → Privileged (sauter tous niveaux)
```

---

#### C. Astuces de Productivité CLI

| **Astuce** | **Usage** |
|------------|-----------|
| **`?`** | Aide contextuelle (affiche les commandes disponibles dans ce mode) |
| **`Tab`** | Complétion automatique de la commande |
| **`sh run`** au lieu de `show running-config` | Abréviation (fonctionne tant qu'unique) |
| **`conf t`** au lieu de `configure terminal` | Abréviation |
| **Flèche Haut** | Rappeler la commande précédente |
| **Ctrl+C** | Interrompre une opération |
| **Ctrl+Z** | Équivalent de `end` (retour Privileged) |
| **`no <commande>`** | Annuler/supprimer une configuration |
| **`show history`** | Afficher les dernières commandes tapées |

---

#### D. Les Commandes `show` Essentielles

Les commandes `show` s'utilisent en **mode Privileged EXEC** (ou User EXEC pour certaines).

```cisco
Switch# show running-config       ! Config active en RAM (tout ce qui est configuré)
Switch# show startup-config       ! Config sauvegardée en Flash (au démarrage)
Switch# show version              ! Version IOS, temps de fonctionnement, RAM, Flash
Switch# show interfaces           ! Détails de toutes les interfaces (compteurs d'erreurs...)
Switch# show ip interface brief   ! Résumé rapide : interface, IP, état (up/down)
Switch# show mac address-table    ! Table MAC apprise
Switch# show flash                ! Fichiers stockés en mémoire Flash
Switch# show history              ! 10 dernières commandes tapées
```

**Exemple de `show ip interface brief` :**
```
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/1        unassigned      YES unset  up                    up
FastEthernet0/2        unassigned      YES unset  down                  down
...
Vlan1                  unassigned      YES unset  administratively down down
```

**Lecture :**
- **up/up** : Interface active (câble connecté + activée)
- **down/down** : Pas de câble ou problème physique
- **administratively down** : Désactivée volontairement (`shutdown`)

---

### III. Configurer un Switch Cisco : Séquence Complète

#### A. Logique de la Configuration Initiale

Quand on reçoit un switch neuf ou réinitialisé, il faut :

1. **Nommer** l'équipement (`hostname`)
2. **Afficher un message d'avertissement** (`banner motd`)
3. **Sécuriser** l'accès mode privilégié (`enable secret`)
4. **Sécuriser** l'accès console (`line console 0`)
5. **Sécuriser** l'accès réseau / Telnet (`line vty 0 15`)
6. **Chiffrer** tous les mots de passe (`service password-encryption`)
7. **Sauvegarder** la configuration (`copy run start`)

---

#### B. Séquence de Configuration Complète (Avec Explications)

```cisco
! ============================================================
! ÉTAPE 0 : Accès en mode privilégié
! ============================================================
Switch> enable                   ! Pas de mot de passe au 1er accès
Switch#

! ============================================================
! ÉTAPE 1 : Entrer en mode configuration globale
! ============================================================
Switch# configure terminal
Switch(config)#

! ============================================================
! ÉTAPE 2 : Nommer le switch
! ============================================================
Switch(config)# hostname SW_BUREAU_01
SW_BUREAU_01(config)#            ! Le prompt change immédiatement !

! ============================================================
! ÉTAPE 3 : Désactiver la résolution DNS (astuce de confort)
! Évite que les fautes de frappe soient interprétées comme
! des noms d'hôtes à résoudre (pauses de 30 secondes !)
! ============================================================
SW_BUREAU_01(config)# no ip domain-lookup

! ============================================================
! ÉTAPE 4 : Configurer le message du jour (Banner MOTD)
! Le '#' est le délimiteur (doit être absent du message)
! ============================================================
SW_BUREAU_01(config)# banner motd #
Entrer le message du jour :
*****************************************************
* Acces INTERDIT aux personnes non autorisees.      *
* Toute connexion est enregistree et tracee.        *
* Contacter le DSI : dsi@entreprise.fr              *
*****************************************************
#

! ============================================================
! ÉTAPE 5 : Sécuriser le mode privilégié
! "enable secret" = chiffrement MD5 (toujours préférer à
! "enable password" qui stocke en clair)
! ============================================================
SW_BUREAU_01(config)# enable secret Cisco@2024

! ============================================================
! ÉTAPE 6 : Sécuriser la ligne console (accès physique)
! ============================================================
SW_BUREAU_01(config)# line console 0
SW_BUREAU_01(config-line)# password Console@2024
SW_BUREAU_01(config-line)# login
SW_BUREAU_01(config-line)# exec-timeout 5 0    ! Déconnexion après 5min inactivité
SW_BUREAU_01(config-line)# exit

! ============================================================
! ÉTAPE 7 : Sécuriser les lignes VTY (accès Telnet/SSH)
! vty 0 15 = 16 sessions simultanées possibles
! ============================================================
SW_BUREAU_01(config)# line vty 0 15
SW_BUREAU_01(config-line)# password Vty@2024
SW_BUREAU_01(config-line)# login
SW_BUREAU_01(config-line)# exec-timeout 5 0
SW_BUREAU_01(config-line)# exit

! ============================================================
! ÉTAPE 8 : Chiffrer TOUS les mots de passe en clair
! (line console, line vty sont en clair sans ça !)
! ============================================================
SW_BUREAU_01(config)# service password-encryption

! ============================================================
! ÉTAPE 9 : Revenir en mode privilégié
! ============================================================
SW_BUREAU_01(config)# end
SW_BUREAU_01#

! ============================================================
! ÉTAPE 10 : Vérifier la configuration
! ============================================================
SW_BUREAU_01# show running-config

! ============================================================
! ÉTAPE 11 : SAUVEGARDER ! (CRITIQUE - sinon perdu au reboot)
! ============================================================
SW_BUREAU_01# copy running-config startup-config
Destination filename [startup-config]?     ! Appuyer sur Entrée
Building configuration...
[OK]
```

---

#### C. Vérifier la Configuration : `show running-config`

Après configuration, taper `show running-config` permet de **vérifier** tout ce qui a été fait.

**Extrait typique :**
```
SW_BUREAU_01# show running-config
Building configuration...

Current configuration : 1024 bytes
!
version 15.0
...
!
hostname SW_BUREAU_01
!
no ip domain-lookup
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0   ← Mot de passe chiffré MD5
!
banner motd ^C
*****************************************************
* Acces INTERDIT aux personnes non autorisees.      *
*****************************************************
^C
!
...
line con 0
 password 7 0822455D0A16                            ← Chiffré par service pwd-enc
 login
 exec-timeout 5 0
!
line vty 0 4
 password 7 0822455D0A16
 login
 exec-timeout 5 0
!
end
```

💡 **`enable secret 5 $1$...`** : Le `5` indique chiffrement MD5. Impossible de retrouver le mot de passe à partir de ce hash.

💡 **`password 7 ...`** : Service password-encryption → chiffrement faible (type 7), déchiffrable en ligne, mais dissuasif.

---

#### D. Différence Running-Config vs Startup-Config

```
Mémoire RAM                      Mémoire Flash (NVRAM)
┌─────────────────────┐           ┌─────────────────────┐
│   running-config    │           │   startup-config    │
│   (config active)   │  copy     │   (config sauvegardée)│
│   Modifiée en temps │──run───→  │   Chargée au         │
│   réel              │  start    │   démarrage          │
│   PERDUE au reboot  │           │   Persistante        │
└─────────────────────┘           └─────────────────────┘
```

⚠️ **Règle d'or :** Toujours `copy run start` après modification !

**Autres commandes de sauvegarde :**
```cisco
Switch# write memory             ! Équivalent de "copy run start" (ancienne syntaxe)
Switch# write                    ! Version abrégée (valide sur certains IOS)
Switch# erase startup-config     ! Effacer la config sauvegardée (reset usine)
Switch# reload                   ! Redémarrer (attention : sans save = config perdue !)
```

---

### IV. Linux : Commandes de Base (Suite S5)

**Rappel S5 :** Navigation (pwd, ls, cd), création (mkdir, touch), lecture (cat, less), édition (nano), suppression (rm), copie (cp), déplacement (mv).

Cette séance approfondit et consolide ces commandes avec plus d'options et de cas pratiques.

---

#### A. Navigation et Exploration

```bash
# Connaître son emplacement
pwd                         # /home/etudiant

# Lister le contenu
ls                          # Liste simple
ls -l                       # Liste longue (permissions, taille, date)
ls -a                       # Inclut les fichiers cachés (commençant par .)
ls -la                      # Longue + cachés (le plus utilisé)
ls -lh                      # Tailles lisibles (Ko, Mo, Go)
ls -lt                      # Triés par date (plus récent en premier)
ls /etc                     # Lister un répertoire sans s'y déplacer

# Se déplacer
cd /home/etudiant            # Chemin absolu (depuis la racine /)
cd BTS_SIO                   # Chemin relatif (depuis le répertoire courant)
cd ..                        # Remonter d'un niveau
cd ../..                     # Remonter de 2 niveaux
cd ~                         # Aller dans son répertoire home
cd -                         # Revenir au répertoire précédent
```

**Comprendre la sortie de `ls -l` :**
```
-rw-r--r--  1  etudiant  etudiant  1024  nov  15 10:30  fichier.txt
│           │  │          │         │     │              │
│           │  │          │         │     │              └─ Nom du fichier
│           │  │          │         │     └─ Date dernière modification
│           │  │          │         └─ Taille en octets
│           │  │          └─ Groupe propriétaire
│           │  └─ Utilisateur propriétaire
│           └─ Nombre de liens physiques
└─ Permissions (type + rwx propriétaire + rwx groupe + rwx autres)
```

---

#### B. Gestion des Répertoires et Fichiers

```bash
# Créer des répertoires
mkdir MonDossier                    # Créer un dossier
mkdir -p Projet/Reseau/Config       # Créer toute l'arborescence d'un coup
                                    # (-p = parents, crée chaque niveau manquant)

# Créer des fichiers
touch fichier.txt                   # Créer un fichier vide
touch a.txt b.txt c.txt             # Créer plusieurs fichiers d'un coup

# Copier
cp source.txt destination.txt       # Copier un fichier (renomme à la destination)
cp source.txt /tmp/                  # Copier dans un répertoire (garde le nom)
cp -r DossierSource/ DossierDest/   # Copier un dossier entier (-r = récursif)
cp -p source.txt dest.txt           # Copier en préservant les métadonnées (date, permissions)

# Déplacer / Renommer
mv ancien.txt nouveau.txt           # Renommer un fichier
mv fichier.txt /tmp/                # Déplacer dans /tmp
mv DossierA/ /home/etudiant/        # Déplacer un dossier

# Supprimer
rm fichier.txt                      # Supprimer un fichier
rm -i fichier.txt                   # Supprimer avec confirmation (-i = interactif)
rm -f fichier.txt                   # Forcer la suppression sans confirmation
rm -r MonDossier/                   # Supprimer un dossier et son contenu
rm -rf MonDossier/                  # Forcer la suppression récursive (DANGEREUX)
```

⚠️ **AVERTISSEMENT `rm -rf` :**
- Il n'y a **pas de corbeille** dans un terminal Linux : la suppression est **définitive**
- TOUJOURS vérifier le chemin avant `rm -rf`
- En TP : préférer `rm -ri` (confirmation pour chaque fichier) pour s'entraîner

---

#### C. Lecture et Affichage du Contenu

```bash
# Afficher le contenu d'un fichier
cat fichier.txt                     # Afficher tout le fichier d'un coup
cat -n fichier.txt                  # Avec numéros de lignes
cat fichier1.txt fichier2.txt       # Afficher plusieurs fichiers à la suite

# Navigation dans les fichiers longs
less fichier.txt                    # Affichage page par page
                                    # (Espace = page suivante, q = quitter,
                                    # / = rechercher, n = occurrence suivante)
more fichier.txt                    # Similaire à less (moins de fonctions)

# Extraits
head fichier.txt                    # 10 premières lignes (défaut)
head -20 fichier.txt                # 20 premières lignes
tail fichier.txt                    # 10 dernières lignes (défaut)
tail -5 fichier.txt                 # 5 dernières lignes
tail -f /var/log/syslog             # Suivi en temps réel (pratique pour logs)
```

---

#### D. Édition avec Nano

**Nano** est l'éditeur de texte en ligne de commande le plus accessible pour les débutants.

**Ouvrir/créer un fichier :**
```bash
nano monFichier.txt         # Ouvrir ou créer
nano /etc/hosts             # Ouvrir un fichier système (ajouter sudo si besoin)
sudo nano /etc/hosts        # Avec droits admin
```

**Interface Nano :**
```
  GNU nano 5.4                 monFichier.txt

Voici le contenu du fichier.
On peut taper ici librement.
_

^G Aide    ^O Enreg.  ^W Chercher ^K Couper  ^T Vérif.
^X Quitter ^R Insérer ^\ Remplacer ^U Coller  ^J Justifier
```

**Raccourcis clavier essentiels (`^` = touche Ctrl) :**

| **Raccourci** | **Action** |
|---------------|------------|
| `Ctrl+O` puis `Entrée` | **Enregistrer** le fichier (O comme ecrirO) |
| `Ctrl+X` | **Quitter** (demande de sauvegarder si modifications) |
| `Ctrl+K` | **Couper** la ligne courante (dans un presse-papiers) |
| `Ctrl+U` | **Coller** la ligne coupée |
| `Ctrl+W` | **Chercher** un texte dans le fichier |
| `Ctrl+G` | **Aide** complète |
| `Ctrl+\` | **Remplacer** texte |
| `Ctrl+C` | Afficher le **numéro de ligne** actuel |
| `Flèches` | Déplacer le curseur |

**Procédure typique :**
1. `nano monfichier.txt` → Le fichier s'ouvre
2. Taper le contenu souhaité
3. `Ctrl+O` → Appuyer sur `Entrée` pour confirmer le nom
4. `Ctrl+X` pour quitter

---

#### E. Exercices Pratiques Linux

##### **Exercice 1 : Créer une Arborescence de Projet**

Créer **entièrement depuis le terminal** la structure suivante :

```
/home/etudiant/
└── BTS_SIO_SISR/
    ├── Bloc2_Reseau/
    │   ├── Semaine5/
    │   │   └── config_ip.txt     → contenu : "IP: 192.168.5.20 - Masque: 255.255.255.0"
    │   └── Semaine6/
    │       └── commutation.txt   → contenu : "Switch = couche 2 - Table MAC"
    ├── Bloc2_Linux/
    │   ├── commandes.txt         → contenu : "ls, cd, cp, mv, rm, mkdir, cat, nano"
    │   └── notes.txt             → contenu vide (juste créer le fichier)
    └── README.txt                → contenu : "Portfolio BTS SIO SISR - Année 1"
```

**Commandes à utiliser :** `mkdir -p`, `nano`, `touch`, `cat`

---

##### **Exercice 2 : Manipulation de Fichiers**

À partir de l'arborescence créée :

1. **Copier** `commandes.txt` dans `/tmp/sauvegarde_commandes.txt`
2. **Déplacer** `notes.txt` dans `Semaine6/` (et le renommer `notes_s6.txt`)
3. **Afficher** le contenu de `config_ip.txt` avec numéros de lignes
4. **Ouvrir** `README.txt` avec nano et ajouter la ligne : "Semaine 6 - Commutation et CLI Cisco"
5. **Lister** le contenu de `BTS_SIO_SISR/` avec les tailles lisibles (`-lh`)

---

##### **Exercice 3 : Comprendre `ls -l`**

Taper `ls -la /home/etudiant/` et répondre :
1. Quels fichiers commencent par un `.` ? À quoi servent-ils ?
2. Quel est le propriétaire du répertoire `BTS_SIO_SISR` ?
3. Quelle est la taille du fichier `commandes.txt` ?
4. Quelle est la date de dernière modification de `README.txt` ?

---

### V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **Commutation** | Processus de transfert de trames entre ports d'un switch selon la table MAC |
| **Table MAC (CAM)** | Table d'un switch associant adresses MAC aux ports physiques |
| **Forwarding** | Switch envoie la trame uniquement sur le port du destinataire connu |
| **Flooding** | Switch envoie la trame sur tous les ports (MAC inconnue ou broadcast) |
| **Self-Learning** | Apprentissage automatique des MAC par le switch à partir des trames |
| **Aging Time** | Durée de vie d'une entrée dans la table MAC (défaut 300s sur Cisco) |
| **Domaine de collision** | Zone où deux émissions simultanées causent une collision |
| **Domaine de diffusion** | Zone où un broadcast est propagé |
| **Hub** | Équipement couche 1, répète le signal sur tous les ports (obsolète) |
| **IOS** | Internetwork Operating System - Système d'exploitation des équipements Cisco |
| **CLI** | Command Line Interface - Interface en ligne de commande Cisco |
| **User EXEC** | Mode CLI initial (prompt `>`) - accès limité |
| **Privileged EXEC** | Mode CLI administrateur (prompt `#`) - accès complet |
| **Global Config** | Mode configuration globale (prompt `(config)#`) |
| **hostname** | Commande Cisco changeant le nom de l'équipement |
| **banner motd** | Message affiché à chaque connexion à l'équipement |
| **enable secret** | Mot de passe chiffré MD5 pour le mode privilégié |
| **service password-encryption** | Chiffre tous les mots de passe en clair dans la config |
| **running-config** | Configuration active en RAM (perdue au redémarrage) |
| **startup-config** | Configuration sauvegardée en Flash (chargée au démarrage) |
| **copy run start** | Commande Cisco sauvegardant la config RAM → Flash |
| **exec-timeout** | Délai d'inactivité avant déconnexion automatique |
| **Chemin absolu** | Chemin depuis la racine `/` (ex: `/home/etudiant/fichier.txt`) |
| **Chemin relatif** | Chemin depuis le répertoire courant (ex: `../fichier.txt`) |
| **`cat`** | Afficher le contenu d'un fichier dans le terminal |
| **`nano`** | Éditeur de texte interactif dans le terminal |
| **`less`** | Afficher un fichier long page par page |

---

### VI. Exercices d'Entraînement

#### Exercice 1 : Simulation Table MAC

Un switch démarre à vide (table MAC vide). Il possède 4 ports avec :
- Port 1 : PC-A (MAC : `AAAA.0001`)
- Port 2 : PC-B (MAC : `BBBB.0002`)
- Port 3 : PC-C (MAC : `CCCC.0003`)
- Port 4 : PC-D (MAC : `DDDD.0004`)

Voici les trames reçues dans l'ordre :
1. PC-A envoie à PC-C
2. PC-C répond à PC-A
3. PC-B envoie à PC-D
4. PC-D répond à PC-B
5. PC-A envoie un broadcast

Pour chaque trame, indiquer :
- L'action du switch (FLOODING ou FORWARDING)
- Le contenu de la table MAC après chaque trame
- Sur quels ports la trame est envoyée

---

#### Exercice 2 : Navigation CLI

Répondre sans Packet Tracer (à partir du cours) :

1. On est en `Switch>`. On tape `show running-config`. Que se passe-t-il ?
2. On est en `Switch(config-if)#`. On veut aller directement en Privileged EXEC. Quelle commande ?
3. On tape `hostname MonSwitch` en mode User EXEC. Que se passe-t-il ?
4. Quelle est la différence entre `enable password` et `enable secret` ?
5. On a configuré le switch mais pas tapé `copy run start`. On redémarre. Qu'arrive-t-il ?

---

#### Exercice 3 : Domaines

Pour le réseau suivant :
```
[PC1]─[PC2]─[HUB_A]─[SWITCH]─[HUB_B]─[PC3]─[PC4]
```

1. Combien de domaines de collision ?
2. Combien de domaines de diffusion ?
3. Si PC1 envoie un ping à PC3, qui reçoit la trame au niveau du HUB_A ?
4. Si on remplace HUB_A par un switch, que change-t-il ?

---

#### Exercice 4 : Commandes Linux

Écrire la commande Linux pour :

1. Créer l'arborescence `/opt/apache/config/sites/` en une seule commande
2. Copier tous les fichiers `.txt` du répertoire courant dans `/tmp/sauvegarde/`
3. Afficher les 20 dernières lignes du fichier `/var/log/syslog`
4. Renommer le fichier `config_old.conf` en `config.conf`
5. Supprimer le dossier `/tmp/test/` et tout son contenu sans confirmation

---

### VII. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer le processus d'apprentissage d'un switch (self-learning, table MAC)
- [ ] Distinguer forwarding et flooding (quand et pourquoi chaque cas)
- [ ] Définir domaine de collision et domaine de diffusion
- [ ] Indiquer quel équipement sépare les domaines de diffusion (routeur !)
- [ ] Naviguer entre les 3 modes CLI Cisco (User, Privileged, Global Config)
- [ ] Configurer le hostname, banner, enable secret d'un switch
- [ ] Configurer les mots de passe console et vty
- [ ] Utiliser `show running-config` pour vérifier la config
- [ ] Sauvegarder une config avec `copy run start`
- [ ] Expliquer la différence running-config / startup-config
- [ ] Naviguer dans l'arborescence Linux (pwd, ls -la, cd)
- [ ] Créer une arborescence de dossiers avec `mkdir -p`
- [ ] Copier, déplacer, renommer des fichiers (cp, mv)
- [ ] Afficher le contenu d'un fichier (cat, less, head, tail)
- [ ] Créer et éditer un fichier avec nano (Ctrl+O, Ctrl+X)

---
