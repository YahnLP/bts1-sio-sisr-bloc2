---
author: YLP
title: 📚 FICHE DE COURS
---

## 📚 FICHE DE COURS ÉLÈVE
### "Routage Statique et Service DHCP"

*Version 1.0 – BTS SIO SISR – Semestre 1 – Semaine 8*

---

### I. Routage Statique

#### A. Le Problème du Routage Inter-Réseaux

Un **switch** relie des machines dans le **même réseau IP**. Mais deux réseaux IP distincts (ex. : `192.168.1.0/24` et `192.168.2.0/24`) ne peuvent pas communiquer directement — leurs adresses sont incompatibles au sens du masque.

Le **routeur** est l'équipement de **couche 3** chargé d'acheminer les paquets **entre** réseaux différents, en se basant sur les **adresses IP**.

**Règle :** Un PC envoie un paquet à sa **passerelle par défaut** dès que la destination n'est pas dans son propre réseau.

```
PC-A (192.168.1.10/24) veut joindre PC-B (192.168.2.20/24) :
  1. PC-A calcule : "192.168.2.20 ∉ mon réseau 192.168.1.0/24"
  2. PC-A envoie le paquet à sa passerelle : 192.168.1.1 (Fa0/0 du routeur)
  3. Le routeur lit l'IP destination, consulte sa table de routage
  4. Trouve la route vers 192.168.2.0/24 → envoie sur Fa0/1
  5. PC-B reçoit le paquet
```

---

#### B. La Table de Routage

La **table de routage** est le cerveau du routeur : une liste de **destinations connues** avec l'indication de **comment y aller** (via quelle interface ou quel prochain saut).

**Commande :**
```cisco
Router# show ip route
```

**Exemple de sortie :**
```
Codes: C - connected, S - static, R - RIP, O - OSPF, * - candidate default

Gateway of last resort is 10.0.0.2 to network 0.0.0.0

      10.0.0.0/30 is subnetted, 1 subnets
C        10.0.0.0 is directly connected, Serial0/0/0
C     192.168.1.0/24 is directly connected, FastEthernet0/0
S     192.168.2.0/24 [1/0] via 10.0.0.2
S*    0.0.0.0/0 [1/0] via 10.0.0.2
```

**Décryptage des codes :**

| **Code** | **Signification** | **Source** |
|----------|-------------------|------------|
| `C` | Connected – Directement connecté | Automatique (interface active avec IP) |
| `S` | Static – Route statique | Configurée manuellement par l'admin |
| `R` | RIP – Route apprise par le protocole RIP | Dynamique (S9+) |
| `O` | OSPF – Route apprise par OSPF | Dynamique (Année 2) |
| `S*` | Static candidate default | Route par défaut statique |

**Décryptage de `[1/0]` :**
- `1` = Distance administrative (fiabilité de la source ; 1 pour routes statiques)
- `0` = Métrique (coût du chemin ; 0 si pas de protocole dynamique)

**Décryptage de `via 10.0.0.2` :**
- Next-hop : adresse IP du prochain routeur à qui transmettre le paquet

---

#### C. Types de Routes

##### 🔹 Routes Directement Connectées (C – Connected)

Ajoutées **automatiquement** dès qu'une interface est configurée avec une IP et activée (`no shutdown`).

```cisco
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
! → Route C 192.168.1.0/24 ajoutée automatiquement dans la table
```

##### 🔹 Routes Statiques (S – Static)

Configurées **manuellement** par l'administrateur. Idéales pour les petits réseaux, ou pour des chemins fixes imposés.

**Syntaxe :**
```cisco
Router(config)# ip route <réseau_destination> <masque> <next_hop>
```

| **Paramètre** | **Description** | **Exemple** |
|---------------|-----------------|-------------|
| réseau_destination | Réseau à atteindre | `192.168.2.0` |
| masque | Masque de ce réseau | `255.255.255.0` |
| next_hop | IP du prochain routeur OU interface de sortie | `10.0.0.2` ou `Serial0/0/0` |

**Exemples :**
```cisco
! Aller vers 192.168.2.0/24 en passant par le routeur 10.0.0.2
Router(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2

! Même chose mais via l'interface de sortie (équivalent)
Router(config)# ip route 192.168.2.0 255.255.255.0 Serial0/0/0

! Supprimer une route statique
Router(config)# no ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

##### 🔹 Route par Défaut (S* – Default Route)

**"Attrape-tout"** : utilisée quand aucune route spécifique ne correspond à la destination. Indispensable pour accéder à Internet (on ne peut pas avoir une route pour chaque IP publique !).

```cisco
! Tout ce qui ne correspond à aucune route → aller vers 10.0.0.2
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

**Analogie :** C'est le panneau *"Toutes directions"* à un carrefour — si vous ne savez pas exactement où aller, suivez cette direction générale.

---

#### D. Configuration Complète d'un Routeur Cisco (Référence)

```cisco
! ============================================
! ÉTAPE 1 : Nommer le routeur
! ============================================
Router> enable
Router# configure terminal
Router(config)# hostname R1_PARIS

! ============================================
! ÉTAPE 2 : Configurer l'interface LAN
! ============================================
R1_PARIS(config)# interface FastEthernet0/0
R1_PARIS(config-if)# description LAN Paris - 192.168.1.0/24
R1_PARIS(config-if)# ip address 192.168.1.1 255.255.255.0
R1_PARIS(config-if)# no shutdown         ! OBLIGATOIRE
R1_PARIS(config-if)# exit

! ============================================
! ÉTAPE 3 : Configurer l'interface WAN (liaison vers autre routeur)
! ============================================
R1_PARIS(config)# interface Serial0/0/0
R1_PARIS(config-if)# description WAN vers R2_LYON
R1_PARIS(config-if)# ip address 10.0.0.1 255.255.255.252
R1_PARIS(config-if)# clock rate 64000    ! Côté DCE uniquement (Packet Tracer)
R1_PARIS(config-if)# no shutdown
R1_PARIS(config-if)# exit

! ============================================
! ÉTAPE 4 : Ajouter les routes statiques
! ============================================
R1_PARIS(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
! Route par défaut vers Internet (si applicable)
R1_PARIS(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2

! ============================================
! ÉTAPE 5 : Vérifier et sauvegarder
! ============================================
R1_PARIS(config)# end
R1_PARIS# show ip route
R1_PARIS# show ip interface brief
R1_PARIS# ping 192.168.2.1          ! Tester la connectivité vers R2
R1_PARIS# copy running-config startup-config
```

---

#### E. Schéma Topologique de Référence

```
Réseau Paris            Liaison WAN             Réseau Lyon
192.168.1.0/24          10.0.0.0/30            192.168.2.0/24

PC-A ─────┐         ┌──────────────────┐         ┌───── PC-B
.10        │         │                  │         │  .20
           └── Fa0/0─┤     R1_PARIS     ├─Se──────┤
                .1   │                  │   .1  .2 │  Fa0/0─R2_LYON
                     └──────────────────┘          │          .1
                          10.0.0.1     10.0.0.2    └─────────────
```

**Table de routage de R1_PARIS :**

| **Réseau dest.** | **Via** | **Interface** | **Type** |
|------------------|---------|---------------|----------|
| 192.168.1.0/24 | Directement connecté | Fa0/0 | C |
| 10.0.0.0/30 | Directement connecté | Se0/0/0 | C |
| 192.168.2.0/24 | 10.0.0.2 | Se0/0/0 | S |

**Table de routage de R2_LYON :**

| **Réseau dest.** | **Via** | **Interface** | **Type** |
|------------------|---------|---------------|----------|
| 192.168.2.0/24 | Directement connecté | Fa0/0 | C |
| 10.0.0.0/30 | Directement connecté | Se0/0/0 | C |
| 192.168.1.0/24 | 10.0.0.1 | Se0/0/0 | S |

---

### II. Le Service DHCP

#### A. Pourquoi le DHCP ?

**Sans DHCP :** Chaque machine doit être configurée manuellement avec une IP, un masque, une passerelle, un DNS. Sur 200 PC, c'est long, fastidieux, et source d'erreurs (conflits d'IP, mauvaises passerelles…).

**Avec DHCP :** Les machines obtiennent leur configuration réseau **automatiquement** auprès d'un serveur DHCP, en quelques secondes.

**DHCP** = *Dynamic Host Configuration Protocol* – Protocole de configuration dynamique des hôtes.

---

#### B. Le Processus DORA (4 étapes)

DORA est le mnémotechnique des 4 messages échangés entre un client et un serveur DHCP :

```
Client (PC)                          Serveur DHCP
    │                                     │
    │──── 1. DISCOVER ──────────────────>│
    │     Broadcast : "Y a-t-il un        │
    │     serveur DHCP sur le réseau ?"   │
    │                                     │
    │<─── 2. OFFER ──────────────────────│
    │     "Oui ! Je te propose            │
    │     192.168.1.50 pour 8 heures."    │
    │                                     │
    │──── 3. REQUEST ───────────────────>│
    │     "J'accepte l'offre de           │
    │     192.168.1.50."                  │
    │                                     │
    │<─── 4. ACK (Acknowledge) ──────────│
    │     "Confirmé ! L'IP est à toi      │
    │     jusqu'à [heure]."               │
    │                                     │
    ✅ Le client configure son IP
```

**Détail des 4 messages :**

| **Étape** | **Nom** | **Type** | **Qui envoie ?** | **Contenu** |
|-----------|---------|----------|------------------|-------------|
| 1 | **DISCOVER** | Broadcast | Client | "Cherche serveur DHCP" |
| 2 | **OFFER** | Unicast (ou broadcast) | Serveur | Propose une IP + config réseau |
| 3 | **REQUEST** | Broadcast | Client | "J'accepte l'offre du serveur X" |
| 4 | **ACK** | Unicast | Serveur | Confirme l'attribution + durée du bail |

**Pourquoi REQUEST est-il un broadcast ?**
Il peut y avoir plusieurs serveurs DHCP. Le REQUEST broadcast permet d'informer tous les serveurs que le client a choisi l'un d'eux, afin que les autres reprennent leurs IP proposées.

---

#### C. Le Bail DHCP (Lease)

L'IP n'est pas attribuée **définitivement** — elle est **louée** pour une durée limitée appelée **bail** (lease time).

- **Durée typique :** 8 heures, 1 jour, 7 jours selon configuration
- **Renouvellement automatique :** À la moitié du bail (T/2), le client essaie de renouveler
- **Libération :** Quand la machine se déconnecte proprement (`ipconfig /release`)
- **File des baux actifs :** `/var/lib/dhcp/dhcpd.leases` (Linux)

**Intérêt du bail limité :** Récupérer automatiquement les IP de machines déconnectées, éviter l'épuisement du pool.

---

#### D. Les Composants d'un Serveur DHCP

##### 🔹 L'Étendue (Scope)

Plage d'adresses IP que le serveur peut distribuer, associée à un réseau.

| **Paramètre d'étendue** | **Description** | **Exemple** |
|-------------------------|-----------------|-------------|
| **Réseau** | Sous-réseau concerné | `192.168.1.0/24` |
| **Plage d'attribution** (range) | Adresses distribuables | `192.168.1.100` → `192.168.1.200` |
| **Masque** | Masque de sous-réseau | `255.255.255.0` |
| **Passerelle** (routers) | Passerelle par défaut | `192.168.1.1` |
| **DNS** | Serveurs de noms | `8.8.8.8`, `8.8.4.4` |
| **Durée du bail** | Durée d'attribution | `86400` (24h en secondes) |

**Adresses à exclure de la plage :**
- L'adresse du routeur (ex. `.1`)
- Les serveurs à IP fixe (ex. `.10`, `.11`, `.12`)
- En pratique : réserver les 10 à 20 premières adresses pour les équipements fixes

**Exemple :** Réseau `192.168.1.0/24`, 50 PC :
- Adresses fixes réservées : `.1` (routeur), `.2` à `.19` (serveurs)
- Plage DHCP : `192.168.1.20` → `192.168.1.150`

##### 🔹 La Réservation (Reservation / Static Mapping)

Permet d'attribuer **toujours la même IP** à une machine identifiée par son **adresse MAC**.

**Cas d'usage :**
- Serveur d'impression (IP fixe pour les PC mais géré par DHCP)
- Caméra IP, NAS
- Tout équipement qui doit avoir une IP prévisible sans configuration manuelle

**Différence IP fixe vs réservation DHCP :**

| | **IP fixe (manuelle)** | **Réservation DHCP** |
|--|------------------------|----------------------|
| Configuration sur | La machine | Le serveur DHCP |
| Risque d'erreur | Oui (erreur humaine) | Non (centralisé) |
| Gestion centralisée | Non | Oui |
| Déménagement réseau | Reconfigurer la machine | Modifier le serveur |

---

### III. TP Linux : Serveur DHCP avec isc-dhcp-server

#### A. Installation du Service

```bash
# Mettre à jour la liste des paquets
sudo apt update

# Installer le serveur DHCP
sudo apt install isc-dhcp-server

# Vérifier l'installation
sudo systemctl status isc-dhcp-server
# → Active: failed (normal - pas encore configuré)
```

---

#### B. Déclarer l'Interface d'Écoute

Le fichier `/etc/default/isc-dhcp-server` déclare **sur quelle interface** le serveur écoute les requêtes DHCP.

```bash
sudo nano /etc/default/isc-dhcp-server
```

Trouver la ligne `INTERFACESv4` et modifier :

```
# Changer :
INTERFACESv4=""

# En (remplacer eth0 par le nom réel de votre interface) :
INTERFACESv4="eth0"
```

**Trouver le nom de l'interface :**
```bash
ip addr show
# Chercher l'interface avec une IP (souvent eth0, ens33, enp0s3...)
```

---

#### C. Configurer le Serveur – `/etc/dhcp/dhcpd.conf`

```bash
# Sauvegarder la config originale
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak

# Éditer la configuration
sudo nano /etc/dhcp/dhcpd.conf
```

**Vider le fichier et entrer la configuration suivante :**

```
# ============================================================
# Paramètres globaux
# ============================================================
option domain-name "bts-sio.local";
option domain-name-servers 8.8.8.8, 8.8.4.4;

default-lease-time 600;          # Bail par défaut : 10 minutes (TP)
max-lease-time 7200;             # Bail maximum : 2 heures

# Ce serveur fait autorité sur son réseau
# (répond aux requêtes même sans bail existant)
authoritative;

# ============================================================
# Étendue (Scope) pour le réseau 192.168.10.0/24
# ============================================================
subnet 192.168.10.0 netmask 255.255.255.0 {
  # Plage d'adresses distribuables
  range 192.168.10.100 192.168.10.150;

  # Options réseau transmises aux clients
  option routers 192.168.10.1;           # Passerelle par défaut
  option subnet-mask 255.255.255.0;      # Masque
  option domain-name-servers 8.8.8.8;   # DNS primaire

  # Durée de bail pour cette étendue (écrase le global)
  default-lease-time 600;
  max-lease-time 3600;
}

# ============================================================
# Réservation : toujours donner 192.168.10.10 à cette MAC
# ============================================================
host PC-Imprimante {
  hardware ethernet aa:bb:cc:dd:ee:ff;   # Remplacer par la vraie MAC
  fixed-address 192.168.10.10;
}
```

---

#### D. Démarrer et Vérifier le Service

```bash
# Vérifier la syntaxe AVANT de redémarrer (évite les erreurs silencieuses)
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
# Si OK : "Configuration file errors encountered -- exiting" est absent

# Redémarrer le service
sudo systemctl restart isc-dhcp-server

# Vérifier l'état du service
sudo systemctl status isc-dhcp-server
```

**Sortie attendue (`systemctl status`) si tout va bien :**
```
● isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled)
     Active: active (running) since ...
```

**Si erreur :** Lire les logs :
```bash
journalctl -u isc-dhcp-server -n 30
# Chercher les lignes "error" ou "warning"
```

**Activer au démarrage :**
```bash
sudo systemctl enable isc-dhcp-server
```

---

#### E. Tester le Serveur DHCP

##### Option 1 – Test depuis la VM elle-même (sans client séparé)

```bash
# Renouveler le bail DHCP de la VM elle-même (si configurée en DHCP)
sudo dhclient -r eth0    # Libérer l'IP actuelle
sudo dhclient eth0       # Demander une nouvelle IP

# Vérifier l'IP obtenue
ip addr show eth0
```

##### Option 2 – Test depuis une VM cliente (réseau interne VirtualBox)

1. Configurer **2 VM sur le même réseau interne** VirtualBox (Host-Only ou Internal Network)
2. VM Serveur : IP fixe `192.168.10.1/24` sur cette interface
3. VM Cliente : Interface configurée en DHCP (`auto eth0` / `iface eth0 inet dhcp`)
4. Démarrer la VM cliente → elle doit obtenir une IP dans la plage `192.168.10.100-150`

---

#### F. Consulter les Baux Actifs

```bash
# Afficher les baux actifs
cat /var/lib/dhcp/dhcpd.leases
```

**Exemple de sortie :**
```
lease 192.168.10.100 {
  starts 3 2024/11/20 10:15:30;
  ends   3 2024/11/20 10:25:30;
  binding state active;
  hardware ethernet aa:bb:cc:11:22:33;
  client-hostname "PC-Client-01";
}
```

---

#### G. Commandes de Diagnostic DHCP – Côté Client

```bash
# Linux : demander une IP DHCP manuellement
sudo dhclient eth0

# Linux : libérer l'IP DHCP
sudo dhclient -r eth0

# Linux : voir les baux côté client
cat /var/lib/dhcp/dhclient.leases

# Windows : voir la config IP (dont info DHCP)
ipconfig /all

# Windows : libérer et renouveler
ipconfig /release
ipconfig /renew
```

---

### IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **Routage** | Processus d'acheminement des paquets IP entre réseaux différents |
| **Table de routage** | Table interne du routeur listant les réseaux connus et comment les atteindre |
| **Passerelle par défaut** | Adresse IP du routeur vers lequel envoyer les paquets hors du réseau local |
| **Route statique** | Route configurée manuellement par l'administrateur (`ip route`) |
| **Route par défaut** | Route `0.0.0.0/0` couvrant toutes les destinations non connues explicitement |
| **Route directement connectée** | Route ajoutée automatiquement quand une interface est active avec une IP |
| **Next-hop** | Adresse IP du prochain routeur sur le chemin vers une destination |
| **Distance administrative** | Indice de fiabilité d'une source de routage (1 = statique, 120 = RIP) |
| **`no shutdown`** | Commande Cisco activant une interface (désactivée par défaut sur routeurs) |
| **DHCP** | Dynamic Host Configuration Protocol – attribution automatique de config réseau |
| **DORA** | Discover, Offer, Request, Acknowledge – les 4 étapes du processus DHCP |
| **Étendue (Scope)** | Plage d'adresses IP qu'un serveur DHCP peut distribuer sur un réseau |
| **Bail (Lease)** | Durée pendant laquelle une adresse IP est attribuée à un client |
| **Réservation** | Attribution d'une IP fixe à une machine identifiée par son adresse MAC |
| **`authoritative`** | Indique que le serveur DHCP fait autorité sur son réseau |
| **`range`** | Directive dhcpd.conf définissant la plage d'IP distribuables |
| **`option routers`** | Directive dhcpd.conf envoyant la passerelle par défaut aux clients |
| **isc-dhcp-server** | Implémentation du serveur DHCP sous Debian/Ubuntu |
| **dhcpd.conf** | Fichier de configuration du serveur isc-dhcp-server |
| **dhcpd.leases** | Fichier des baux DHCP actifs |

---

### V. Exercices d'Entraînement

#### Exercice 1 : Compléter les Tables de Routage

Topologie :
```
PC1 (192.168.10.10/24) ─Fa0/0─ R1 ─Se0/0/0─ R2 ─Fa0/0─ PC2 (192.168.20.20/24)
                      .1      10.0.0.1  10.0.0.2    .1
```

**a)** Compléter la table de routage de **R1** :

| **Réseau destination** | **Masque** | **Via (next-hop)** | **Interface** | **Type** |
|------------------------|------------|---------------------|---------------|----------|
| 192.168.10.0 | 255.255.255.0 | Directement connecté | Fa0/0 | C |
| 10.0.0.0 | 255.255.255.252 | Directement connecté | Se0/0/0 | C |
| 192.168.20.0 | ? | ? | ? | ? |

**b)** Compléter la table de routage de **R2** :

| **Réseau destination** | **Masque** | **Via (next-hop)** | **Interface** | **Type** |
|------------------------|------------|---------------------|---------------|----------|
| 192.168.20.0 | 255.255.255.0 | Directement connecté | Fa0/0 | C |
| 10.0.0.0 | 255.255.255.252 | Directement connecté | Se0/0/0 | C |
| 192.168.10.0 | ? | ? | ? | ? |

**c)** Écrire les commandes `ip route` pour R1 et R2.

---

#### Exercice 2 : DORA

Remettre dans l'ordre ces événements DHCP et indiquer le type de chaque message (broadcast/unicast) :

- Le client envoie un message à tous les serveurs DHCP du réseau.
- Le serveur confirme l'attribution de l'IP et la durée du bail.
- Le client informe le serveur choisi qu'il accepte l'offre.
- Le serveur propose une adresse IP disponible au client.

---

#### Exercice 3 : Analyser un fichier dhcpd.conf

```
subnet 172.16.5.0 netmask 255.255.255.0 {
  range 172.16.5.50 172.16.5.100;
  option routers 172.16.5.1;
  option domain-name-servers 1.1.1.1;
  default-lease-time 3600;
  max-lease-time 86400;
}
```

1. Quel est le réseau configuré ?
2. Combien d'adresses IP peuvent être distribuées ?
3. Quelle est la passerelle envoyée aux clients ?
4. Un client obtient un bail à 9h00 et ne renouvelle pas. À quelle heure son IP sera-t-elle libérable ?
5. Peut-on configurer une réservation avec `fixed-address 172.16.5.5` ? Est-ce une bonne pratique (l'IP est-elle dans la plage range) ?

---

#### Exercice 4 : Diagnostic

**Scénario :** PC1 (réseau 192.168.1.0/24) ne peut pas pinger PC2 (réseau 192.168.2.0/24). Le routeur est en place.

Donner 5 causes possibles et la commande permettant de vérifier/corriger chacune.

---

### VI. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer pourquoi un switch ne peut pas relier deux réseaux IP différents
- [ ] Décrire le rôle de la passerelle par défaut côté client
- [ ] Lire une table de routage Cisco et identifier les types C, S, S*
- [ ] Écrire la commande `ip route` pour une route statique et une route par défaut
- [ ] Configurer une interface routeur Cisco (ip address + no shutdown)
- [ ] Expliquer les 4 étapes DORA du protocole DHCP
- [ ] Distinguer étendue DHCP et réservation DHCP
- [ ] Installer isc-dhcp-server sous Debian
- [ ] Configurer `/etc/dhcp/dhcpd.conf` avec une étendue et une réservation
- [ ] Démarrer/arrêter/vérifier un service avec `systemctl`
- [ ] Lire les baux actifs dans `/var/lib/dhcp/dhcpd.leases`

---
