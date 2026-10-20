# Cours – Routage statique et service DHCP

*BTS SIO SISR 1re année – Bloc 2 – S07 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S04, S06 (adressage, VLAN) ; terminal Linux.
> - **Matériel :** Simulateur réseau ou Packet Tracer, ou matériel réel (au choix de l'enseignant) ; VM Debian.

## À la fin de ce cours, tu sauras

- [ ] **Lire** une table de routage Cisco et **distinguer** routes connectées et statiques ;
- [ ] **Configurer** une interface de routeur et des **routes statiques** (dont la route par défaut) ;
- [ ] **Tester** la connectivité avec `ping` et `show ip route` ;
- [ ] **Expliquer** les quatre étapes de DHCP (DORA) ;
- [ ] **Installer** et **configurer** un serveur DHCP sous Debian : étendue, options, réservation.

---

## I. Routage Statique

### A. Le Problème du Routage Inter-Réseaux

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

### B. La Table de Routage

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
| `R` | RIP – Route apprise par le protocole RIP | Dynamique (vu en deuxième année) |
| `O` | OSPF – Route apprise par OSPF | Dynamique (Année 2) |
| `S*` | Static candidate default | Route par défaut statique |

**Décryptage de `[1/0]` :**
- `1` = Distance administrative (fiabilité de la source ; 1 pour routes statiques)
- `0` = Métrique (coût du chemin ; 0 si pas de protocole dynamique)

**Décryptage de `via 10.0.0.2` :**
- Next-hop : adresse IP du prochain routeur à qui transmettre le paquet

---

### C. Types de Routes

#### Routes Directement Connectées (C – Connected)

Ajoutées **automatiquement** dès qu'une interface est configurée avec une IP et activée (`no shutdown`).

```cisco
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
! → Route C 192.168.1.0/24 ajoutée automatiquement dans la table
```

#### Routes Statiques (S – Static)

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

#### Route par Défaut (S* – Default Route)

**"Attrape-tout"** : utilisée quand aucune route spécifique ne correspond à la destination. Indispensable pour accéder à Internet (on ne peut pas avoir une route pour chaque IP publique !).

```cisco
! Tout ce qui ne correspond à aucune route → aller vers 10.0.0.2
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

**Analogie :** C'est le panneau *"Toutes directions"* à un carrefour — si vous ne savez pas exactement où aller, suivez cette direction générale.

---

### D. Configuration Complète d'un Routeur Cisco (Référence)

<figure class="graphe diagramme"><svg viewBox="0 0 760 250" style="max-width:760px" role="img" aria-label="Deux réseaux locaux reliés par une liaison série entre deux routeurs" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="10" width="200" height="226" rx="10" class="dg-soft"/>
<rect x="550" y="10" width="200" height="226" rx="10" class="dg-soft"/>
<text x="110" y="30" text-anchor="middle" class="dg-t">Réseau Paris</text>
<text x="110" y="48" text-anchor="middle" class="dg-s">192.168.1.0/24</text>
<text x="650" y="30" text-anchor="middle" class="dg-t">Réseau Lyon</text>
<text x="650" y="48" text-anchor="middle" class="dg-s">192.168.2.0/24</text>
<rect x="35" y="90" width="150" height="46" rx="8" class="dg-box"/>
<text x="110.0" y="109.0" text-anchor="middle" class="dg-t">PC-A</text>
<text x="110.0" y="127.0" text-anchor="middle" class="dg-s">192.168.1.10</text>
<rect x="575" y="90" width="150" height="46" rx="8" class="dg-box"/>
<text x="650.0" y="109.0" text-anchor="middle" class="dg-t">PC-B</text>
<text x="650.0" y="127.0" text-anchor="middle" class="dg-s">192.168.2.20</text>
<rect x="240" y="80" width="110" height="66" rx="8" class="dg-dark"/>
<text x="295.0" y="118.0" text-anchor="middle" class="dg-w">R1_PARIS</text>
<rect x="410" y="80" width="110" height="66" rx="8" class="dg-dark"/>
<text x="465.0" y="118.0" text-anchor="middle" class="dg-w">R2_LYON</text>
<line x1="185" y1="113" x2="240" y2="113" class="dg-line"/>
<text x="212" y="104" text-anchor="middle" class="dg-s">Fa0/0</text>
<text x="212" y="130" text-anchor="middle" class="dg-s">.1</text>
<line x1="520" y1="113" x2="575" y2="113" class="dg-line"/>
<text x="548" y="104" text-anchor="middle" class="dg-s">Fa0/0</text>
<text x="548" y="130" text-anchor="middle" class="dg-s">.1</text>
<line x1="350" y1="113" x2="410" y2="113" class="dg-line"/>
<text x="380" y="100" text-anchor="middle" class="dg-s">Se</text>
<text x="364" y="134" text-anchor="middle" class="dg-s">.1</text>
<text x="396" y="134" text-anchor="middle" class="dg-s">.2</text>
<rect x="300" y="176" width="160" height="38" rx="8" class="dg-warn"/>
<text x="380.0" y="191.0" text-anchor="middle" class="dg-t">Liaison WAN</text>
<text x="380.0" y="209.0" text-anchor="middle" class="dg-s">10.0.0.0/30</text>
<text x="380" y="164" text-anchor="middle" class="dg-i">10.0.0.1  -  10.0.0.2</text>
</svg>
</figure>

---

### E. Schéma Topologique de Référence

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

## II. Le Service DHCP

### A. Pourquoi le DHCP ?

**Sans DHCP :** Chaque machine doit être configurée manuellement avec une IP, un masque, une passerelle, un DNS. Sur 200 PC, c'est long, fastidieux, et source d'erreurs (conflits d'IP, mauvaises passerelles…).

**Avec DHCP :** Les machines obtiennent leur configuration réseau **automatiquement** auprès d'un serveur DHCP, en quelques secondes.

**DHCP** = *Dynamic Host Configuration Protocol* – Protocole de configuration dynamique des hôtes.

---

### B. Le Processus DORA (4 étapes)

DORA est le mnémotechnique des 4 messages échangés entre un client et un serveur DHCP :

<figure class="graphe diagramme"><svg viewBox="0 0 640 308" style="max-width:640px" role="img" aria-label="Échange DHCP en quatre messages : Discover, Offer, Request, Acknowledge" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="155.0" y="30.0" text-anchor="middle" class="dg-w">Client DHCP (PC)</text>
<line x1="155.0" y1="42" x2="155.0" y2="300" class="dg-dash"/>
<rect x="400.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="485.0" y="30.0" text-anchor="middle" class="dg-w">Serveur DHCP</text>
<line x1="485.0" y1="42" x2="485.0" y2="300" class="dg-dash"/>
<text x="320.0" y="64" text-anchor="middle" class="dg-t">1. DISCOVER (broadcast)</text>
<line x1="155.0" y1="72" x2="476.0" y2="72.0" class="dg-line"/>
<polygon points="485.0,72 476.0,67.0 476.0,77.0" class="dg-ah"/>
<text x="320.0" y="88" text-anchor="middle" class="dg-i">« Y a-t-il un serveur DHCP sur le réseau ? »</text>
<text x="320.0" y="114" text-anchor="middle" class="dg-t">2. OFFER</text>
<line x1="485.0" y1="122" x2="164.0" y2="122.0" class="dg-line"/>
<polygon points="155.0,122 164.0,127.0 164.0,117.0" class="dg-ah"/>
<text x="320.0" y="138" text-anchor="middle" class="dg-i">« Je te propose 192.168.1.50 pour 8 heures. »</text>
<text x="320.0" y="164" text-anchor="middle" class="dg-t">3. REQUEST (broadcast)</text>
<line x1="155.0" y1="172" x2="476.0" y2="172.0" class="dg-line"/>
<polygon points="485.0,172 476.0,167.0 476.0,177.0" class="dg-ah"/>
<text x="320.0" y="188" text-anchor="middle" class="dg-i">« J&#x27;accepte l&#x27;offre de 192.168.1.50. »</text>
<text x="320.0" y="214" text-anchor="middle" class="dg-t">4. ACK</text>
<line x1="485.0" y1="222" x2="164.0" y2="222.0" class="dg-line"/>
<polygon points="155.0,222 164.0,227.0 164.0,217.0" class="dg-ah"/>
<text x="320.0" y="238" text-anchor="middle" class="dg-i">« Confirmé : l&#x27;adresse est à toi jusqu&#x27;à l&#x27;échéance du bail. »</text>
<rect x="28.8" y="256" width="252.4" height="26" rx="6" class="dg-soft"/>
<text x="155.0" y="273" text-anchor="middle" class="dg-s">Le client configure son adresse IP</text>
</svg>
</figure>

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

### C. Le Bail DHCP (Lease)

L'IP n'est pas attribuée **définitivement** — elle est **louée** pour une durée limitée appelée **bail** (lease time).

- **Durée typique :** 8 heures, 1 jour, 7 jours selon configuration
- **Renouvellement automatique :** À la moitié du bail (T/2), le client essaie de renouveler
- **Libération :** Quand la machine se déconnecte proprement (`ipconfig /release`)
- **File des baux actifs :** `/var/lib/dhcp/dhcpd.leases` (Linux)

**Intérêt du bail limité :** Récupérer automatiquement les IP de machines déconnectées, éviter l'épuisement du pool.

---

### D. Les Composants d'un Serveur DHCP

#### L'Étendue (Scope)

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

#### La Réservation (Reservation / Static Mapping)

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

## IV. Vocabulaire Clé

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

## V. Exercices d'Entraînement

### Exercice 1 : Compléter les Tables de Routage

Topologie :

<figure class="graphe diagramme"><svg viewBox="0 0 780 230" style="max-width:780px" role="img" aria-label="Topologie de l&#x27;exercice 1 : PC1, routeur R1, routeur R2, PC2" xmlns="http://www.w3.org/2000/svg">
<rect x="40" y="66" width="40" height="28" rx="4" class="dg-box"/>
<rect x="45" y="71" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="57" y="94" width="6" height="6" class="dg-box"/>
<rect x="48" y="100" width="24" height="4" rx="2" class="dg-box"/>
<text x="60" y="120" text-anchor="middle" class="dg-s">PC1</text>
<rect x="236" y="68" width="68" height="44" rx="8" class="dg-dark"/>
<text x="270" y="95" text-anchor="middle" class="dg-w">R1</text>
<rect x="476" y="68" width="68" height="44" rx="8" class="dg-dark"/>
<text x="510" y="95" text-anchor="middle" class="dg-w">R2</text>
<rect x="700" y="66" width="40" height="28" rx="4" class="dg-box"/>
<rect x="705" y="71" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="717" y="94" width="6" height="6" class="dg-box"/>
<rect x="708" y="100" width="24" height="4" rx="2" class="dg-box"/>
<text x="720" y="120" text-anchor="middle" class="dg-s">PC2</text>
<line x1="82" y1="82" x2="236" y2="82" class="dg-line"/>
<line x1="304" y1="82" x2="476" y2="82" class="dg-line"/>
<line x1="544" y1="82" x2="698" y2="82" class="dg-line"/>
<text x="60" y="138" text-anchor="middle" class="dg-s">192.168.10.10/24</text>
<text x="720" y="138" text-anchor="middle" class="dg-s">192.168.20.20/24</text>
<text x="159" y="70" text-anchor="middle" class="dg-i">Fa0/0 (.1)</text>
<text x="159" y="104" text-anchor="middle" class="dg-i">LAN 192.168.10.0/24</text>
<text x="312" y="70" text-anchor="start" class="dg-s">Se0/0/0 : 10.0.0.1</text>
<text x="468" y="104" text-anchor="end" class="dg-s">Se0/0/0 : 10.0.0.2</text>
<text x="621" y="70" text-anchor="middle" class="dg-i">Fa0/0 (.1)</text>
<text x="621" y="104" text-anchor="middle" class="dg-i">LAN 192.168.20.0/24</text>
<rect x="290" y="150" width="200" height="50" rx="8" class="dg-warn"/>
<text x="390" y="171" text-anchor="middle" class="dg-t">Liaison WAN</text>
<text x="390" y="189" text-anchor="middle" class="dg-s">10.0.0.0/30</text>
</svg>
</figure>
**a)** Compléter la table de routage de **R1** :

| **Réseau destination** | **Masque** | **Via (next-hop)** | **Interface** | **Type** |
|------------------------|------------|---------------------|---------------|----------|
| 192.168.10.0 | 255.255.255.0 | Directement connecté | Fa0/0 | C |
| 10.0.0.0 | 255.255.255.252 | Directement connecté | Se0/0/0 | C |
| 192.168.20.0 | ? | ? | ? | ? |

<details markdown="1">
<summary>Correction a) : table de R1</summary>

| **Réseau destination** | **Masque** | **Via (next-hop)** | **Interface** | **Type** |
|------------------------|------------|---------------------|---------------|----------|
| 192.168.20.0 | 255.255.255.0 | 10.0.0.2 (Se0/0/0 de R2) | Se0/0/0 | **S** (statique) |

Le réseau `192.168.20.0` n'est pas connecté à R1 : il faut lui indiquer par quelle adresse l'atteindre. Le next-hop est l'adresse de R2 **sur le lien commun** : `10.0.0.2`.

</details>

**b)** Compléter la table de routage de **R2** :

| **Réseau destination** | **Masque** | **Via (next-hop)** | **Interface** | **Type** |
|------------------------|------------|---------------------|---------------|----------|
| 192.168.20.0 | 255.255.255.0 | Directement connecté | Fa0/0 | C |
| 10.0.0.0 | 255.255.255.252 | Directement connecté | Se0/0/0 | C |
| 192.168.10.0 | ? | ? | ? | ? |

<details markdown="1">
<summary>Correction b) : table de R2</summary>

| **Réseau destination** | **Masque** | **Via (next-hop)** | **Interface** | **Type** |
|------------------------|------------|---------------------|---------------|----------|
| 192.168.10.0 | 255.255.255.0 | 10.0.0.1 (Se0/0/0 de R1) | Se0/0/0 | **S** (statique) |

</details>

**c)** Écrire les commandes `ip route` pour R1 et R2.

<details markdown="1">
<summary>Correction c) : commandes</summary>

```cisco
R1(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2
R2(config)# ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

Il faut une route **dans chaque sens** : sans la route de R2 vers `192.168.10.0`, le ping de PC1 atteint PC2 mais la réponse ne revient jamais. Vérification : `show ip route` (lignes `S`).

</details>

---

### Exercice 2 : DORA

Remettre dans l'ordre ces événements DHCP et indiquer le type de chaque message (broadcast/unicast) :

- Le client envoie un message à tous les serveurs DHCP du réseau.
- Le serveur confirme l'attribution de l'IP et la durée du bail.
- Le client informe le serveur choisi qu'il accepte l'offre.
- Le serveur propose une adresse IP disponible au client.

<details markdown="1">
<summary>Correction de l'exercice 2</summary>

| **Ordre** | **Message** | **Événement** | **Type** |
|:---------:|-------------|---------------|----------|
| 1 | **DISCOVER** | Le client envoie un message à tous les serveurs DHCP du réseau | **Broadcast** (le client n'a pas encore d'adresse) |
| 2 | **OFFER** | Le serveur propose une adresse IP disponible au client | **Unicast** (ou broadcast selon le client) |
| 3 | **REQUEST** | Le client informe le serveur choisi qu'il accepte l'offre | **Broadcast** (les autres serveurs apprennent qu'ils n'ont pas été retenus) |
| 4 | **ACK** | Le serveur confirme l'attribution de l'IP et la durée du bail | **Unicast** |

Moyen mnémotechnique : **DORA** (Discover, Offer, Request, Acknowledge).

</details>

---

### Exercice 3 : Analyser un fichier dhcpd.conf

```
subnet 172.16.5.0 netmask 255.255.255.0 {
  range 172.16.5.50 172.16.5.100;
  option routers 172.16.5.1;
  option domain-name-servers 1.1.1.1;
  default-lease-time 3600;
  max-lease-time 86400;
}
```

**1.** Quel est le réseau configuré ?

<details markdown="1">
<summary>Correction de la question 1</summary>

`172.16.5.0/24` (masque `255.255.255.0`).

</details>

**2.** Combien d'adresses IP peuvent être distribuées ?

<details markdown="1">
<summary>Correction de la question 2</summary>

De `.50` à `.100` inclus : 100 − 50 + 1 = **51 adresses**.

</details>

**3.** Quelle est la passerelle envoyée aux clients ?

<details markdown="1">
<summary>Correction de la question 3</summary>

`172.16.5.1` (`option routers`).

</details>

**4.** Un client obtient un bail à 9h00 et ne renouvelle pas. À quelle heure son IP sera-t-elle libérable ?

<details markdown="1">
<summary>Correction de la question 4</summary>

Le bail par défaut est de 3600 s = 1 h : l'adresse redevient libérable à **10h00**. (Le client peut demander une durée plus longue, au plus `max-lease-time` = 86400 s soit 24 h.) Un client normal tente de renouveler à la moitié du bail, soit 9h30.

</details>

**5.** Peut-on configurer une réservation avec `fixed-address 172.16.5.5` ? Est-ce une bonne pratique (l'IP est-elle dans la plage range) ?

<details markdown="1">
<summary>Correction de la question 5</summary>

**Oui**, avec un bloc `host` qui associe l'adresse MAC du client à `fixed-address 172.16.5.5`. C'est une bonne pratique ici : `172.16.5.5` est **en dehors** de la plage dynamique (`.50` à `.100`), donc aucun autre client ne peut recevoir cette adresse par erreur.

</details>

---

### Exercice 4 : Diagnostic

**Scénario :** PC1 (réseau 192.168.1.0/24) ne peut pas pinger PC2 (réseau 192.168.2.0/24). Le routeur est en place.

Donner 5 causes possibles et la commande permettant de vérifier/corriger chacune.

<details markdown="1">
<summary>Correction de l'exercice 4</summary>

Méthode : tester de proche en proche (`ping 127.0.0.1`, sa propre adresse, la passerelle, l'autre côté du routeur, puis PC2).

| **Cause possible** | **Vérification** | **Correction** |
|--------------------|------------------|----------------|
| Passerelle par défaut absente ou fausse sur PC1 (ou PC2) | `ipconfig` / `ip route` | Renseigner la bonne passerelle |
| Interface du routeur éteinte (`shutdown`) ou câble débranché | `show ip interface brief` (état `up/up` ?) | `no shutdown` |
| Route manquante, dans un sens ou dans l'autre | `show ip route` | `ip route 192.168.2.0 255.255.255.0 <next-hop>` |
| Mauvaise adresse ou mauvais masque sur une interface ou un PC | `show ip interface brief`, `ipconfig` | Corriger l'adresse ou le masque |
| Pare-feu ou ACL qui bloque l'ICMP (ex. pare-feu Windows de PC2) | `show access-lists`, règles du pare-feu du poste | Autoriser l'ICMP ou tester avec un autre protocole |

</details>
---

## VI. Auto-évaluation : Suis-je Prêt ?

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
