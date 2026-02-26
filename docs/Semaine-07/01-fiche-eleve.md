---
author: YLP
title: 📚 FICHE DE COURS
---

## 📚 FICHE DE COURS ÉLÈVE
### "VLANs, Trunk 802.1Q et Linux : Utilisateurs, Groupes, Permissions"

*Version 1.0 - BTS SIO SISR - Semestre 1 - Semaine 7*

---

### 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|----------------|
| **B2.2** | Configurer des VLANs et un trunk sur switch Cisco |
| **B2.3** | Vérifier et diagnostiquer la segmentation réseau |
| **B2.1** | Gérer les utilisateurs, groupes et permissions Linux |

---

### I. VLANs : Segmentation Logique du Réseau

#### A. Définition et Problème Résolu

**VLAN (Virtual Local Area Network)** = Réseau local **logique** créé à l'intérieur d'un switch physique. Un VLAN regroupe des ports de switch qui forment un **domaine de diffusion isolé**, indépendamment du câblage physique.

**Problème sans VLAN :**
- Tous les ports d'un switch = 1 domaine de diffusion unique
- Tout le monde reçoit les broadcasts
- Pas d'isolation entre services → risque de sécurité, performances dégradées

**Solution avec VLANs :**
```
Switch physique unique                Switches logiques virtuels

Ports 1-4 : Direction   ──────────►  [VLAN 10 - Direction]   ┐
Ports 5-8 : Commercial  ──────────►  [VLAN 20 - Commercial]  ┤ Isolés entre eux
Ports 9-12: Informatique ─────────►  [VLAN 30 - Informatique]┘
```

---

#### B. Avantages des VLANs

| **Avantage** | **Explication** |
|--------------|-----------------|
| **Sécurité** | Isolation logique : Commercial ne voit pas les trames de la Direction |
| **Performance** | Réduction des broadcasts (chaque VLAN = 1 domaine de diffusion distinct) |
| **Flexibilité** | Regrouper des utilisateurs par fonction, pas par emplacement physique |
| **Économie** | Un seul équipement physique pour plusieurs réseaux logiques |
| **Administration** | Changer de service = changer l'affectation du port, pas re-câbler |

---

#### C. Types de Ports Switch : Access vs Trunk

##### 🔹 **Port Access (Port d'Accès)**

- Appartient à **un seul VLAN**
- Connecte les **équipements terminaux** (PC, imprimante, téléphone IP)
- La trame qui sort du port Access est **sans tag VLAN** (l'équipement terminal ne sait pas qu'il est dans un VLAN)

```
PC (Direction)─────[Access VLAN 10]─── Switch
                    Trame sans tag
                    Switch sait : ce port = VLAN 10
```

---

##### 🔹 **Port Trunk (Port de Liaison)**

- Transporte **plusieurs VLANs simultanément**
- Connecte les **switches entre eux** ou un switch à un routeur
- Chaque trame est **taguée** avec le numéro de VLAN (sauf VLAN natif)

```
Switch A────[Trunk 802.1Q]────Switch B
           Trames taguées :
           [VLAN 10 | données]
           [VLAN 20 | données]
           [VLAN 30 | données]
```

---

#### D. Le Protocole 802.1Q (Dot1Q) : Marquage des Trames

**Principe :** Quand une trame traverse un trunk, le switch y insère un **tag de 4 octets** (32 bits) entre l'en-tête Ethernet et les données.

**Structure du tag 802.1Q :**
```
Trame normale Ethernet :
[MAC Dest (6o)] [MAC Src (6o)] [EtherType (2o)] [Données] [FCS]

Trame 802.1Q taguée :
[MAC Dest (6o)] [MAC Src (6o)] [802.1Q Tag (4o)] [EtherType (2o)] [Données] [FCS]
                                │
                     ┌──────────┴──────────┐
                     │  TPID (2o) = 0x8100 │  ← Identifie une trame 802.1Q
                     │  PCP (3 bits)        │  ← Priorité (QoS)
                     │  DEI (1 bit)         │  ← Eligible au rejet
                     │  VID (12 bits)       │  ← Numéro de VLAN (0 à 4094)
                     └─────────────────────┘
```

**VID (VLAN Identifier) :** 12 bits → 4096 VLANs possibles (0 et 4095 réservés → 4094 utilisables).

---

#### E. Le VLAN Natif

**Définition :** Le VLAN natif est le seul VLAN dont les trames traversent le trunk **sans tag**.

- Valeur par défaut Cisco : **VLAN 1**
- Les deux extrémités du trunk doivent avoir le **même VLAN natif**
- Si mismatch : CDP (Cisco Discovery Protocol) affiche une erreur

**Bonne pratique :** Changer le VLAN natif (pas VLAN 1) et n'y mettre aucun utilisateur.

```
Switch A                    Trunk 802.1Q                   Switch B
                  ┌─────────────────────────┐
Port VLAN 10 ──►  │ [Tag VLAN 10] données   │ ──► Port VLAN 10
Port VLAN 20 ──►  │ [Tag VLAN 20] données   │ ──► Port VLAN 20
Port VLAN 1  ──►  │ données (sans tag)      │ ──► Port VLAN 1 (natif)
                  └─────────────────────────┘
```

---

#### F. Fonctionnement Complet : Trame VLAN de A à Z

**Topologie :**
```
PC1 (VLAN 10)─Access─[SW_A]─Trunk─[SW_B]─Access─PC3 (VLAN 10)
PC2 (VLAN 20)─Access─[SW_A]                  ─Access─PC4 (VLAN 20)
```

**PC1 envoie une trame à PC3 :**

```
Étape 1 : PC1 envoie une trame normale (sans tag) sur son port Access
         → SW_A reçoit sur port Access VLAN 10

Étape 2 : SW_A consulte sa table MAC pour VLAN 10
         → Si PC3 connu : port Trunk
         → SW_A ajoute le tag VLAN 10 à la trame

Étape 3 : Trame taguée [VLAN 10] traverse le Trunk

Étape 4 : SW_B reçoit la trame taguée
         → Lit le tag : VLAN 10
         → Retire le tag
         → Envoie la trame (sans tag) sur le port Access VLAN 10 de PC3

Étape 5 : PC3 reçoit la trame normale (ne sait pas qu'elle était taguée)
```

**PC2 (VLAN 20) essaie d'envoyer à PC3 (VLAN 10) :**
- Impossible au niveau L2 — ils sont dans des VLANs différents
- SW_A ne transmettra jamais une trame VLAN 20 sur un port VLAN 10

---

### II. Configuration VLANs sur Switch Cisco

#### A. Créer des VLANs

**Mode :** Global Configuration

```cisco
Switch# configure terminal

! Créer VLAN 10 et le nommer
Switch(config)# vlan 10
Switch(config-vlan)# name DIRECTION
Switch(config-vlan)# exit

! Créer VLAN 20
Switch(config)# vlan 20
Switch(config-vlan)# name COMMERCIAL
Switch(config-vlan)# exit

! Créer VLAN 30
Switch(config)# vlan 30
Switch(config-vlan)# name INFORMATIQUE
Switch(config-vlan)# exit
```

**Vérification :**
```cisco
Switch# show vlan brief
```

**Résultat :**
```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, ...  (tous les ports non affectés)
10   DIRECTION                        active
20   COMMERCIAL                       active
30   INFORMATIQUE                     active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

💡 **À noter :** Les VLANs 10, 20, 30 n'ont pas encore de ports affectés (colonne Ports vide).

---

#### B. Affecter des Ports en Mode Access

**Mode :** Interface Configuration

```cisco
! Port 1 → VLAN 10 (Direction)
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

! Port 2 → VLAN 10 (Direction)
Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

! Ports 3-4 → VLAN 20 (Commercial) - commande de plage
Switch(config)# interface range FastEthernet0/3 - 4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

! Ports 5-6 → VLAN 30 (Informatique)
Switch(config)# interface range FastEthernet0/5 - 6
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit
```

**Vérification :**
```cisco
Switch# show vlan brief
```

**Résultat attendu :**
```
VLAN Name                   Status    Ports
---- ---------------         --------- --------
1    default                 active    Fa0/7, Fa0/8 ... (ports non affectés)
10   DIRECTION               active    Fa0/1, Fa0/2
20   COMMERCIAL              active    Fa0/3, Fa0/4
30   INFORMATIQUE            active    Fa0/5, Fa0/6
```

---

#### C. Configurer un Port Trunk

**Mode :** Interface Configuration (sur le port reliant 2 switches)

```cisco
! Port 24 = liaison inter-switches → Trunk
Switch(config)# interface FastEthernet0/24
Switch(config-if)# switchport mode trunk

! (Optionnel - Bonne pratique) Restreindre les VLANs autorisés
Switch(config-if)# switchport trunk allowed vlan 10,20,30

! (Optionnel - Bonne pratique) Changer le VLAN natif
Switch(config-if)# switchport trunk native vlan 99

Switch(config-if)# exit
Switch(config)# end
Switch# copy run start
```

**Vérification :**
```cisco
Switch# show interfaces trunk
```

**Résultat :**
```
Port      Mode         Encapsulation  Status    Native vlan
Fa0/24    on           802.1q         trunking  99

Port      Vlans allowed on trunk
Fa0/24    10,20,30

Port      Vlans allowed and active in management domain
Fa0/24    10,20,30

Port      Vlans in spanning tree forwarding state and not pruned
Fa0/24    10,20,30
```

**Commande utile pour inspecter un port :**
```cisco
Switch# show interfaces FastEthernet0/1 switchport
```
```
Name: Fa0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: dot1q
Access Mode VLAN: 10 (DIRECTION)
Trunking Native Mode VLAN: 1 (default)
...
```

---

### III. Linux : Utilisateurs, Groupes et Permissions

#### A. Notion d'Utilisateurs sous Linux

Linux est un système **multi-utilisateurs**. Chaque fichier et processus appartient à un utilisateur.

**Types de comptes :**

| **Type** | **UID** | **Exemples** | **Usage** |
|----------|---------|--------------|-----------|
| **root** | 0 | `root` | Super-administrateur, accès total |
| **Utilisateur système** | 1-999 | `www-data`, `syslog` | Services/démons système, pas de connexion interactive |
| **Utilisateur normal** | ≥1000 | `alice`, `etudiant` | Utilisateurs humains |

**Fichiers clés :**
```bash
cat /etc/passwd     # Liste des comptes : login:x:UID:GID:info:home:shell
cat /etc/group      # Liste des groupes : nom:x:GID:membres
sudo cat /etc/shadow  # Mots de passe hashés (root uniquement)
```

**Exemple de ligne `/etc/passwd` :**
```
alice:x:1001:1001:Alice Dupont:/home/alice:/bin/bash
  │   │  │    │    │            │            └─ Shell par défaut
  │   │  │    │    │            └─ Répertoire home
  │   │  │    │    └─ Info (GECOS)
  │   │  │    └─ GID principal
  │   │  └─ UID
  │   └─ Mot de passe (x = stocké dans /etc/shadow)
  └─ Nom de login
```

---

#### B. Créer et Gérer des Utilisateurs

```bash
# Créer un utilisateur avec home directory et shell bash
sudo useradd -m -s /bin/bash alice
#           -m : créer /home/alice automatiquement
#           -s : définir le shell (/bin/bash = shell interactif)

# Définir le mot de passe
sudo passwd alice
# → Demande le nouveau MDP (2 fois, ne s'affiche pas)

# Créer un utilisateur et l'affecter directement à un groupe
sudo useradd -m -s /bin/bash -G direction alice
#           -G : groupe(s) supplémentaires (en plus du groupe principal)

# Modifier un utilisateur existant
sudo usermod -aG direction alice    # Ajouter alice au groupe direction (-a = append, sinon remplace)
sudo usermod -aG sudo alice         # Donner les droits sudo à alice
sudo usermod -s /bin/sh alice       # Changer le shell
sudo usermod -l nouveau_nom alice   # Renommer le compte

# Supprimer un utilisateur
sudo userdel alice                  # Supprime le compte (garde /home/alice)
sudo userdel -r alice               # Supprime compte + /home/alice + mail spool
```

⚠️ **`usermod -aG` vs `usermod -G` :**
- `-G groupe` seul → **remplace** tous les groupes supplémentaires par `groupe`
- `-aG groupe` → **ajoute** `groupe` aux groupes existants (append)
- Toujours utiliser `-aG` pour ne pas accidentellement retirer des accès !

---

#### C. Créer et Gérer des Groupes

```bash
# Créer un groupe
sudo groupadd direction
sudo groupadd commercial
sudo groupadd informatique

# Supprimer un groupe
sudo groupdel direction

# Afficher les groupes d'un utilisateur
groups alice          # Sortie : alice direction sudo
id alice              # Sortie : uid=1001(alice) gid=1001(alice) groups=1001(alice),4(adm),27(sudo),1002(direction)

# Lister tous les groupes
cat /etc/group
```

---

#### D. Comprendre les Permissions Linux

Chaque fichier/répertoire a **3 niveaux de permissions** pour **3 catégories** :

```
ls -l fichier.txt
-rw-r--r-- 1 alice direction 1024 nov 15 10:30 fichier.txt
│││││││││ │ │     │
│││││││││ │ │     └─ Groupe propriétaire
│││││││││ │ └─ Utilisateur propriétaire
│││││││││ └─ Nombre de liens
│└┴┴┴┴┴┴┴─ Permissions (9 caractères)
└─ Type : - = fichier, d = répertoire, l = lien symbolique
```

**Décomposition des 9 caractères de permissions :**

```
rw-r--r--
│││││││││
│││││││└┴─ Autres (others) : r-- = lecture seule
│││└┴┴─── Groupe : r-- = lecture seule
└┴┴─────── Propriétaire (user) : rw- = lecture + écriture
```

**Signification de chaque permission :**

| **Permission** | **Symbole** | **Valeur** | **Sur un fichier** | **Sur un répertoire** |
|----------------|-------------|------------|--------------------|-----------------------|
| **Read** | `r` | 4 | Lire le contenu | Lister le contenu (ls) |
| **Write** | `w` | 2 | Modifier/écrire | Créer/supprimer des fichiers dedans |
| **Execute** | `x` | 1 | Exécuter (script, programme) | Traverser (cd dans ce répertoire) |
| **Aucune** | `-` | 0 | Accès refusé | Accès refusé |

---

#### E. Modifier les Permissions : `chmod`

**Deux syntaxes :** numérique (rapide) et symbolique (lisible).

##### 🔹 **Syntaxe Numérique**

Calculer la valeur pour chaque catégorie (u/g/o) : r=4, w=2, x=1.

**Exemples courants :**

| **Notation** | **Permissions** | **Usage typique** |
|--------------|-----------------|-------------------|
| `chmod 777` | `rwxrwxrwx` | ⚠️ Dangereux - tout le monde peut tout faire |
| `chmod 755` | `rwxr-xr-x` | Script/exécutable : proprio tout, groupe+autres exécutent |
| `chmod 644` | `rw-r--r--` | Fichier normal : proprio modifie, groupe+autres lisent |
| `chmod 640` | `rw-r-----` | Fichier sensible : groupe lit, autres rien |
| `chmod 600` | `rw-------` | Fichier privé : proprio seul modifie |
| `chmod 700` | `rwx------` | Répertoire privé : proprio seul accède |

**Calcul rapide `chmod 754` :**
```
7 = r+w+x = 4+2+1 = rwx  (propriétaire)
5 = r+x   = 4+0+1 = r-x  (groupe)
4 = r     = 4+0+0 = r--  (autres)
→ rwxr-xr--
```

---

##### 🔹 **Syntaxe Symbolique**

Format : `chmod [qui][opération][permission] fichier`

| **Qui ?** | **Opération** | **Permission** |
|-----------|---------------|----------------|
| `u` = user (propriétaire) | `+` = ajouter | `r` = lecture |
| `g` = group | `-` = retirer | `w` = écriture |
| `o` = others | `=` = définir exactement | `x` = exécution |
| `a` = all (tous) | | |

```bash
chmod u+x script.sh          # Ajouter l'exécution au propriétaire
chmod g-w fichier.txt         # Retirer l'écriture au groupe
chmod o-rwx confidentiel.txt  # Retirer tout aux autres
chmod a+r rapport.pdf         # Ajouter lecture à tous
chmod u=rwx,g=rx,o= dossier/  # Définir précisément : proprio=rwx, groupe=r-x, autres=---
chmod -R 755 /var/www/html/   # Appliquer récursivement (-R)
```

---

#### F. Modifier le Propriétaire : `chown`

```bash
# Changer le propriétaire
chown alice fichier.txt               # Fichier appartient maintenant à alice
sudo chown alice fichier.txt          # Souvent besoin de sudo

# Changer propriétaire ET groupe simultanément
chown alice:direction fichier.txt     # alice = proprio, direction = groupe
chown :direction fichier.txt          # Changer seulement le groupe (équivalent chgrp)

# Récursif (tout un dossier)
sudo chown -R alice:direction /var/data/direction/
```

---

#### G. Scénario Complet : Créer des Utilisateurs par Service

**Objectif :** Préparer un système Linux pour 3 services (Direction, Commercial, Informatique) avec isolation des données.

```bash
# 1. Créer les groupes
sudo groupadd direction
sudo groupadd commercial
sudo groupadd informatique

# 2. Créer les utilisateurs
sudo useradd -m -s /bin/bash -G direction alice
sudo passwd alice         # Définir MDP pour alice

sudo useradd -m -s /bin/bash -G direction bob
sudo passwd bob

sudo useradd -m -s /bin/bash -G commercial charlie
sudo passwd charlie

sudo useradd -m -s /bin/bash -G informatique diana
sudo passwd diana

# 3. Créer les répertoires partagés par service
sudo mkdir -p /data/direction
sudo mkdir -p /data/commercial
sudo mkdir -p /data/informatique

# 4. Affecter propriétaire et groupe
sudo chown root:direction /data/direction
sudo chown root:commercial /data/commercial
sudo chown root:informatique /data/informatique

# 5. Permissions : groupe peut lire/écrire/traverser, autres rien
sudo chmod 770 /data/direction
sudo chmod 770 /data/commercial
sudo chmod 770 /data/informatique
# → rwxrwx--- : proprio(root)=tout, groupe=tout, autres=rien

# 6. Vérification
ls -ld /data/direction
# drwxrwx--- 2 root direction 4096 nov 15 10:30 /data/direction

# Test : alice (direction) peut-elle accéder à /data/commercial ?
su - alice
ls /data/commercial    # Doit afficher "Permission non accordée"
ls /data/direction     # Doit fonctionner
```

---

### IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **VLAN** | Virtual Local Area Network - réseau logique sur un switch physique |
| **Segmentation logique** | Découpage d'un réseau en sous-réseaux isolés sans modifier le câblage |
| **Port Access** | Port switch appartenant à un seul VLAN (pour équipements terminaux) |
| **Port Trunk** | Port switch transportant plusieurs VLANs (liaisons inter-switches) |
| **Tag 802.1Q** | En-tête de 4 octets inséré dans la trame pour identifier le VLAN |
| **VID** | VLAN Identifier - 12 bits dans le tag 802.1Q (4094 VLANs max) |
| **VLAN natif** | VLAN dont les trames traversent le trunk sans tag (défaut = VLAN 1) |
| **Domaine de diffusion VLAN** | Un VLAN = un domaine de diffusion distinct |
| **Routage inter-VLAN** | Communication entre VLANs via un routeur ou switch L3 |
| **`vlan <id>`** | Mode de configuration d'un VLAN dans la CLI Cisco |
| **`switchport mode access`** | Configurer un port en mode Access |
| **`switchport mode trunk`** | Configurer un port en mode Trunk |
| **`show vlan brief`** | Afficher résumé des VLANs et leurs ports |
| **`show interfaces trunk`** | Afficher les ports trunk et les VLANs autorisés |
| **UID** | User ID - identifiant numérique d'un utilisateur Linux |
| **GID** | Group ID - identifiant numérique d'un groupe Linux |
| **`useradd`** | Commande de création d'utilisateur Linux |
| **`passwd`** | Définir ou changer un mot de passe utilisateur |
| **`usermod -aG`** | Ajouter un utilisateur à un groupe (sans retirer les autres) |
| **`groupadd`** | Créer un groupe Linux |
| **Permissions rwx** | Read (4), Write (2), eXecute (1) sur fichier ou répertoire |
| **`chmod`** | Modifier les permissions d'un fichier/répertoire |
| **`chown`** | Changer le propriétaire (et éventuellement le groupe) |
| **`ls -l`** | Afficher les permissions, propriétaire, taille d'un fichier |

---

### V. Exercices d'Entraînement

#### Exercice 1 : Calcul de Permissions

Convertir en notation symbolique (rwx) :

| **chmod** | **Propriétaire** | **Groupe** | **Autres** | **Notation symbolique** |
|-----------|-----------------|------------|------------|-------------------------|
| `chmod 755` | | | | |
| `chmod 644` | | | | |
| `chmod 700` | | | | |
| `chmod 640` | | | | |
| `chmod 777` | | | | |

Et l'inverse, convertir en valeur numérique :

| **Notation symbolique** | **Valeur numérique** |
|-------------------------|----------------------|
| `rwxr-xr-x` | |
| `rw-r--r--` | |
| `rwx------` | |
| `rw-rw-r--` | |

---

#### Exercice 2 : Analyse `ls -l`

```
-rw-r----- 1 alice direction  2048 nov 15 rapport.pdf
drwxrwx--- 2 root  commercial 4096 nov 15 /data/commercial
-rwxr-xr-x 1 root  root        512 nov 15 script.sh
```

1. Qui peut **lire** `rapport.pdf` ? Qui ne peut pas ?
2. Qui peut **créer des fichiers** dans `/data/commercial` ?
3. L'utilisateur `charlie` (groupe `commercial`) peut-il exécuter `script.sh` ?
4. Donner la commande pour que `bob` (groupe `direction`) puisse lire `rapport.pdf`.

---

#### Exercice 3 : VLANs

Un switch a la configuration suivante :
```
VLAN 10 (RH) : Ports 1-3
VLAN 20 (IT) : Ports 4-6
VLAN 30 (DIR) : Ports 7-8
Trunk : Port 24
```

**Questions :**
1. PC branché en port 2 peut-il communiquer avec PC en port 5 ? Pourquoi ?
2. PC en port 7 envoie un broadcast. Qui le reçoit ?
3. Quelle commande affiche les ports affectés à chaque VLAN ?
4. Que faut-il pour que RH et IT puissent s'envoyer des fichiers ?
5. Si le port 24 est en trunk, quels VLANs traversent-ils la liaison vers le 2e switch ?

---

### VI. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer l'intérêt des VLANs (sécurité, performance, flexibilité)
- [ ] Distinguer port Access (1 VLAN) et port Trunk (N VLANs)
- [ ] Expliquer le rôle du tag 802.1Q et du VLAN natif
- [ ] Créer des VLANs sur un switch Cisco et les nommer
- [ ] Affecter des ports en mode Access à un VLAN
- [ ] Configurer un port Trunk entre deux switches
- [ ] Utiliser `show vlan brief` et `show interfaces trunk`
- [ ] Créer des utilisateurs et groupes Linux (useradd, groupadd, passwd)
- [ ] Ajouter un utilisateur à un groupe (usermod -aG)
- [ ] Lire les permissions Linux (rwxr-xr-x → 755)
- [ ] Calculer la valeur chmod numérique (r=4, w=2, x=1)
- [ ] Modifier les permissions avec chmod (numérique et symbolique)
- [ ] Changer le propriétaire d'un fichier avec chown

---