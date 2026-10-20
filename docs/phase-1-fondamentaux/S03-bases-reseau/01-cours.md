# Cours – Les fondamentaux du réseau

*BTS SIO SISR 1re année – Bloc 2 – S03 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S01 (connectique RJ45).
> - **Matériel :** Postes Windows, simulateur réseau, Wireshark.

## À la fin de ce cours, tu sauras

- [ ] **Décrire** les composants d'un réseau (carte réseau, switch, routeur) et les **types** de réseaux (PAN, LAN, MAN, WAN) ;
- [ ] **Expliquer** le modèle OSI (7 couches) et le modèle TCP/IP (4 couches) ;
- [ ] **Lire** une adresse IPv4 et son masque ;
- [ ] **Utiliser** `ping`, `ipconfig`, `tracert`, `nslookup` pour diagnostiquer ;
- [ ] **Capturer** quelques trames avec Wireshark et **repérer** les adresses MAC et IP.

---

## I. Introduction : Qu'est-ce qu'un Réseau Informatique ?

### A. Définition

**Réseau informatique** = Ensemble d'**équipements interconnectés** permettant l'**échange de données** (fichiers, messages, vidéos...).

**Analogie :** Un réseau informatique est comme le **réseau routier** :
- Les **routes** = câbles réseau (RJ45, fibre)
- Les **voitures** = données (paquets)
- Les **carrefours** = switchs et routeurs
- Les **panneaux** = protocoles (règles de circulation)
- Les **adresses** = adresses IP

---

### B. Composants de Base d'un Réseau

| **Composant** | **Rôle** | **Analogie** | **Exemple** |
|---------------|----------|--------------|-------------|
| **Hôte** (Host) | Machine connectée (PC, serveur, smartphone, imprimante) | Maison avec adresse | PC portable, serveur web |
| **Câble réseau** | Support physique de transmission | Route | Câble RJ45 (cuivre), fibre optique |
| **Carte réseau** (NIC) | Interface permettant la connexion au réseau | Porte d'entrée de la maison | Carte Ethernet, Wi-Fi |
| **Switch** | Équipement reliant plusieurs machines dans un réseau local | Rond-point local (quartier) | Switch 24 ports Cisco |
| **Routeur** | Équipement reliant plusieurs réseaux entre eux | Autoroute inter-villes | Box Internet, routeur Cisco |
| **Point d'accès Wi-Fi** | Permet connexion sans fil | Route sans goudron (radio) | Borne Wi-Fi d'entreprise |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 603.09 289.60" style="max-width:804px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 278.8)">
<g class="node n&#45;ellipse">
<ellipse fill="none" stroke="black" stroke-width="1.8" cx="74.25" cy="-133.5" rx="74.49" ry="26.33"/>
<text text-anchor="start" x="43.75" y="-130" font-family="Helvetica,sans-Serif" font-size="15.00">Internet</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M292.49,-152C292.49,-152 218.49,-152 218.49,-152 212.49,-152 206.49,-146 206.49,-140 206.49,-140 206.49,-127 206.49,-127 206.49,-121 212.49,-115 218.49,-115 218.49,-115 292.49,-115 292.49,-115 298.49,-115 304.49,-121 304.49,-127 304.49,-127 304.49,-140 304.49,-140 304.49,-146 298.49,-152 292.49,-152"/>
<text text-anchor="start" x="225.49" y="-130" font-family="Helvetica,sans-Serif" font-size="15.00">Routeur</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M148.67,-133.5C168.1,-133.5 188.59,-133.5 206.3,-133.5"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M439.49,-152C439.49,-152 374.49,-152 374.49,-152 368.49,-152 362.49,-146 362.49,-140 362.49,-140 362.49,-127 362.49,-127 362.49,-121 368.49,-115 374.49,-115 374.49,-115 439.49,-115 439.49,-115 445.49,-115 451.49,-121 451.49,-127 451.49,-127 451.49,-140 451.49,-140 451.49,-146 445.49,-152 439.49,-152"/>
<text text-anchor="start" x="381.49" y="-130" font-family="Helvetica,sans-Serif" font-size="15.00">Switch</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M304.62,-133.5C323.06,-133.5 343.98,-133.5 362.01,-133.5"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M569.49,-268C569.49,-268 521.49,-268 521.49,-268 515.49,-268 509.49,-262 509.49,-256 509.49,-256 509.49,-243 509.49,-243 509.49,-237 515.49,-231 521.49,-231 521.49,-231 569.49,-231 569.49,-231 575.49,-231 581.49,-237 581.49,-243 581.49,-243 581.49,-256 581.49,-256 581.49,-262 575.49,-268 569.49,-268"/>
<text text-anchor="start" x="528.49" y="-246" font-family="Helvetica,sans-Serif" font-size="15.00">PC 1</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M429.92,-152.14C455.4,-173.78 497.17,-209.28 522.62,-230.91"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M569.49,-191C569.49,-191 521.49,-191 521.49,-191 515.49,-191 509.49,-185 509.49,-179 509.49,-179 509.49,-166 509.49,-166 509.49,-160 515.49,-154 521.49,-154 521.49,-154 569.49,-154 569.49,-154 575.49,-154 581.49,-160 581.49,-166 581.49,-166 581.49,-179 581.49,-179 581.49,-185 575.49,-191 569.49,-191"/>
<text text-anchor="start" x="528.49" y="-169" font-family="Helvetica,sans-Serif" font-size="15.00">PC 2</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M451.56,-145.95C470.13,-151.25 491.51,-157.36 509.09,-162.38"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M569.49,-114C569.49,-114 521.49,-114 521.49,-114 515.49,-114 509.49,-108 509.49,-102 509.49,-102 509.49,-89 509.49,-89 509.49,-83 515.49,-77 521.49,-77 521.49,-77 569.49,-77 569.49,-77 575.49,-77 581.49,-83 581.49,-89 581.49,-89 581.49,-102 581.49,-102 581.49,-108 575.49,-114 569.49,-114"/>
<text text-anchor="start" x="528.49" y="-92" font-family="Helvetica,sans-Serif" font-size="15.00">PC 3</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M451.56,-121.37C470.13,-116.2 491.51,-110.25 509.09,-105.36"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M569.49,-37C569.49,-37 521.49,-37 521.49,-37 515.49,-37 509.49,-31 509.49,-25 509.49,-25 509.49,-12 509.49,-12 509.49,-6 515.49,0 521.49,0 521.49,0 569.49,0 569.49,0 575.49,0 581.49,-6 581.49,-12 581.49,-12 581.49,-25 581.49,-25 581.49,-31 575.49,-37 569.49,-37"/>
<text text-anchor="start" x="528.49" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">PC 4</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M430.23,-114.77C455.61,-93.38 496.89,-58.61 522.27,-37.22"/>
</g>
</g>
</svg>
</figure>
*Un réseau local simple : quatre PC reliés à un switch, lui-même connecté à un routeur (accès Internet).*
---

### C. Pourquoi Mettre en Réseau ?

**Avantages :**

1. **Avantage :** **Partage de ressources**
   - Fichiers (serveur de fichiers)
   - Imprimantes (imprimante réseau)
   - Connexion Internet (routeur partagé)
   - Applications (logiciels métier centralisés)

2. **Avantage :** **Communication**
   - Emails, messagerie instantanée
   - Visioconférence
   - Partage d'informations en temps réel

3. **Avantage :** **Centralisation**
   - Données stockées sur serveurs (sauvegardes facilitées)
   - Administration centralisée (Active Directory vu plus tard)

4. **Avantage :** **Économies**
   - 1 imprimante pour 10 personnes (vs 10 imprimantes)
   - 1 connexion Internet partagée

**Inconvénients :**

1. **Inconvénient :** **Complexité** : Plus difficile à configurer et maintenir qu'un PC isolé
2. **Inconvénient :** **Sécurité** : Point d'entrée pour les attaques (malwares, intrusions)
3. **Inconvénient :** **Dépendance** : Si le réseau tombe, tous les utilisateurs sont bloqués
4. **Inconvénient :** **Coût initial** : Équipements réseau (switchs, routeurs, câblage)

---

## II. Le Modèle OSI : Les 7 Couches

### A. Pourquoi un Modèle en Couches ?

Pour communiquer, deux machines doivent respecter des **règles communes** (protocoles).

Plutôt que d'avoir un seul gros programme gérant tout (impossible à maintenir), on découpe en **7 couches indépendantes**.

**Avantages du découpage en couches :**
- **Avantage :** **Modularité** : Changer une couche sans toucher aux autres (ex: passer d'Ethernet à Wi-Fi)
- **Avantage :** **Standardisation** : Différents constructeurs peuvent fabriquer des équipements compatibles
- **Avantage :** **Simplicité** : Chaque couche a un rôle précis et limité

---

### B. Les 7 Couches OSI (Open Systems Interconnection)

**Mnémotechnique pour retenir l'ordre (de bas en haut) :**
> **"P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way"

| **N°** | **Couche** | **Rôle** | **Protocoles / Technologies** | **Unité de Données** |
|--------|------------|----------|-------------------------------|---------------------|
| **7** | **Application** | Interface avec l'utilisateur (applications) | HTTP, FTP, SMTP, DNS, DHCP | **Données** (Data) |
| **6** | **Présentation** | Formatage, encodage, chiffrement, compression | JPEG, MP3, SSL/TLS, ASCII | **Données** |
| **5** | **Session** | Gestion des sessions (ouverture, maintien, fermeture) | NetBIOS, RPC | **Données** |
| **4** | **Transport** | Fiabilité, découpage en segments, contrôle de flux | **TCP**, **UDP** | **Segment** (TCP) / **Datagramme** (UDP) |
| **3** | **Réseau** | Adressage logique, routage | **IP**, ICMP, ARP | **Paquet** (Packet) |
| **2** | **Liaison** | Adressage physique, détection d'erreurs | **Ethernet**, Wi-Fi, PPP | **Trame** (Frame) |
| **1** | **Physique** | Transmission des bits (signaux électriques/optiques) | Câbles RJ45, fibre, ondes radio | **Bit** |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 378.60 628.60" style="max-width:505px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 617.8)">
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M331,-607C331,-607 26,-607 26,-607 20,-607 14,-601 14,-595 14,-595 14,-582 14,-582 14,-576 20,-570 26,-570 26,-570 331,-570 331,-570 337,-570 343,-576 343,-582 343,-582 343,-595 343,-595 343,-601 337,-607 331,-607"/>
<text text-anchor="start" x="33" y="-585" font-family="Helvetica,sans-Serif" font-size="15.00">7 Application : navigateur, messagerie</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M324,-512C324,-512 33,-512 33,-512 27,-512 21,-506 21,-500 21,-500 21,-487 21,-487 21,-481 27,-475 33,-475 33,-475 324,-475 324,-475 330,-475 336,-481 336,-487 336,-487 336,-500 336,-500 336,-506 330,-512 324,-512"/>
<text text-anchor="start" x="40" y="-490" font-family="Helvetica,sans-Serif" font-size="15.00">6 Présentation : formats, chiffrement</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M178.5,-569.57C178.5,-555.84 178.5,-536.7 178.5,-521.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="181.65,-521.06 178.5,-512.06 175.35,-521.06 181.65,-521.06"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M345,-417C345,-417 12,-417 12,-417 6,-417 0,-411 0,-405 0,-405 0,-392 0,-392 0,-386 6,-380 12,-380 12,-380 345,-380 345,-380 351,-380 357,-386 357,-392 357,-392 357,-405 357,-405 357,-411 351,-417 345,-417"/>
<text text-anchor="start" x="19" y="-395" font-family="Helvetica,sans-Serif" font-size="15.00">5 Session : ouverture, maintien, fermeture</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M178.5,-474.57C178.5,-460.84 178.5,-441.7 178.5,-426.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="181.65,-426.06 178.5,-417.06 175.35,-426.06 181.65,-426.06"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M292.5,-322C292.5,-322 64.5,-322 64.5,-322 58.5,-322 52.5,-316 52.5,-310 52.5,-310 52.5,-297 52.5,-297 52.5,-291 58.5,-285 64.5,-285 64.5,-285 292.5,-285 292.5,-285 298.5,-285 304.5,-291 304.5,-297 304.5,-297 304.5,-310 304.5,-310 304.5,-316 298.5,-322 292.5,-322"/>
<text text-anchor="start" x="71.5" y="-300" font-family="Helvetica,sans-Serif" font-size="15.00">4 Transport : TCP, UDP, ports</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M178.5,-379.57C178.5,-365.84 178.5,-346.7 178.5,-331.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="181.65,-331.06 178.5,-322.06 175.35,-331.06 181.65,-331.06"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M267.5,-227C267.5,-227 89.5,-227 89.5,-227 83.5,-227 77.5,-221 77.5,-215 77.5,-215 77.5,-202 77.5,-202 77.5,-196 83.5,-190 89.5,-190 89.5,-190 267.5,-190 267.5,-190 273.5,-190 279.5,-196 279.5,-202 279.5,-202 279.5,-215 279.5,-215 279.5,-221 273.5,-227 267.5,-227"/>
<text text-anchor="start" x="96.5" y="-205" font-family="Helvetica,sans-Serif" font-size="15.00">3 Réseau : IP, routage</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M178.5,-284.57C178.5,-270.84 178.5,-251.7 178.5,-236.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="181.65,-236.06 178.5,-227.06 175.35,-236.06 181.65,-236.06"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M315,-132C315,-132 42,-132 42,-132 36,-132 30,-126 30,-120 30,-120 30,-107 30,-107 30,-101 36,-95 42,-95 42,-95 315,-95 315,-95 321,-95 327,-101 327,-107 327,-107 327,-120 327,-120 327,-126 321,-132 315,-132"/>
<text text-anchor="start" x="49" y="-110" font-family="Helvetica,sans-Serif" font-size="15.00">2 Liaison : Ethernet, adresses MAC</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M178.5,-189.57C178.5,-175.84 178.5,-156.7 178.5,-141.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="181.65,-141.06 178.5,-132.06 175.35,-141.06 181.65,-141.06"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M310.5,-37C310.5,-37 46.5,-37 46.5,-37 40.5,-37 34.5,-31 34.5,-25 34.5,-25 34.5,-12 34.5,-12 34.5,-6 40.5,0 46.5,0 46.5,0 310.5,0 310.5,0 316.5,0 322.5,-6 322.5,-12 322.5,-12 322.5,-25 322.5,-25 322.5,-31 316.5,-37 310.5,-37"/>
<text text-anchor="start" x="53.5" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">1 Physique : câbles, signaux, bits</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M178.5,-94.57C178.5,-80.84 178.5,-61.7 178.5,-46.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="181.65,-46.06 178.5,-37.06 175.35,-46.06 181.65,-46.06"/>
</g>
</g>
</svg>
</figure>
*Les 7 couches du modèle OSI, de l'application (en haut) au câble (en bas).*
---

### C. Détail de Chaque Couche

#### **Couche 1 - Physique**

**Rôle :** Transmission des **bits** (0 et 1) sous forme de signaux physiques.

**Caractéristiques :**
- Type de signal : Électrique (cuivre), lumineux (fibre), radio (Wi-Fi)
- Vitesse de transmission : 10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps...
- Type de câble : RJ45 Cat5e/Cat6, fibre monomode/multimode
- Connecteurs : RJ45, LC, SC (fibre)

**Équipements :** 
- Câbles (RJ45, fibre optique)
- Hub (répéteur) - obsolète
- Répéteur

**Exemple concret :** Le câble Ethernet RJ45 que vous branchez dans votre PC.

---

#### **Couche 2 - Liaison de Données (Data Link)**

**Rôle :** Organiser les bits en **trames**, détecter les erreurs, gérer l'accès au média (qui parle quand).

**Adressage :** **Adresse MAC** (Media Access Control)
- Format : 6 octets en hexadécimal
- Exemple : `00:1A:2B:3C:4D:5E`
- Unique au monde (gravée dans la carte réseau)
- Les 3 premiers octets = constructeur (OUI - Organizationally Unique Identifier)

**Protocoles :**
- **Ethernet** (réseau filaire)
- **Wi-Fi (802.11)** (réseau sans fil)
- **PPP** (Point-to-Point Protocol)

**Équipements :** 
- **Switch** (commutateur) - Relie les machines dans un réseau local (LAN)
- Pont (bridge)

**Exemple concret :** Un switch qui reçoit une trame destinée à l'adresse MAC `AA:BB:CC:DD:EE:FF` et l'envoie uniquement au port où se trouve cette machine.

---

#### **Couche 3 - Réseau (Network)**

**Rôle :** **Routage** des paquets entre différents réseaux. Adressage logique.

**Adressage :** **Adresse IP** (Internet Protocol)
- IPv4 : 4 octets → Exemple : `192.168.1.10`
- IPv6 : 16 octets → Exemple : `2001:0db8:85a3::8a2e:0370:7334`

**Protocoles :**
- **IP** (Internet Protocol) - Transport des paquets
- **ICMP** (Internet Control Message Protocol) - Messages d'erreur, ping
- **ARP** (Address Resolution Protocol) - Correspondance IP ↔ MAC

**Équipements :** 
- **Routeur** - Relie plusieurs réseaux (LAN, WAN, Internet)

**Exemple concret :** Votre box Internet (routeur) qui reçoit un paquet destiné à `8.8.8.8` (Google DNS) et le route vers Internet.

---

#### **Couche 4 - Transport**

**Rôle :** Assurer la **fiabilité** de la transmission (ou pas, selon le protocole). Découpage et réassemblage des données.

**Protocoles :**

| **Protocole** | **Type** | **Fiable ?** | **Usage** |
|---------------|----------|--------------|-----------|
| **TCP** | Orienté connexion | Oui (accusés réception, retransmission) | Email, Web (HTTP), transfert fichiers (FTP) |
| **UDP** | Sans connexion | Non (pas de vérification) | Streaming vidéo, VoIP, DNS, jeux en ligne |

**TCP (Transmission Control Protocol) :**
- Établit une connexion (handshake 3 étapes : SYN, SYN-ACK, ACK)
- Garantit l'ordre d'arrivée des segments
- Retransmet les segments perdus
- Plus **lent** mais **fiable**

**UDP (User Datagram Protocol) :**
- Envoie directement sans établir de connexion
- Pas de garantie de livraison
- Plus **rapide** mais **non fiable**

**Numéro de port :** Identifie l'application destinataire (0-65535)

| **Port** | **Service** | **Protocole** |
|----------|-------------|---------------|
| 20/21 | FTP (transfert fichiers) | TCP |
| 22 | SSH (connexion sécurisée) | TCP |
| 25 | SMTP (envoi email) | TCP |
| 53 | DNS (résolution noms) | UDP (et TCP) |
| 80 | HTTP (web non sécurisé) | TCP |
| 443 | HTTPS (web sécurisé) | TCP |
| 3389 | RDP (bureau à distance Windows) | TCP |

**Exemple concret :** Quand vous visitez `https://www.google.fr`, votre navigateur se connecte au port **443** du serveur Google en utilisant **TCP**.

---

#### **Couche 5 - Session**

**Rôle :** Gestion des **sessions** de communication (ouverture, maintien, fermeture).

**Protocoles :**
- NetBIOS
- RPC (Remote Procedure Call)

**Exemple concret :** Quand vous vous connectez à un site web, une session est ouverte. Si vous fermez le navigateur, la session est fermée.

 **Note :** Cette couche est souvent intégrée dans la couche Application (modèle TCP/IP simplifié).

---

#### **Couche 6 - Présentation**

**Rôle :** **Formatage**, **encodage**, **chiffrement**, **compression** des données.

**Fonctions :**
- Conversion de formats (JPEG, MP3, MPEG, ASCII, EBCDIC)
- Chiffrement/déchiffrement (SSL/TLS)
- Compression/décompression

**Exemple concret :** Quand vous accédez à un site en HTTPS, la couche Présentation chiffre les données avec SSL/TLS avant l'envoi.

 **Note :** Souvent intégrée dans la couche Application (modèle TCP/IP).

---

#### **Couche 7 - Application**

**Rôle :** Interface avec l'**utilisateur final**. Services réseau directement utilisables.

**Protocoles :**
- **HTTP/HTTPS** : Navigation web
- **FTP** : Transfert de fichiers
- **SMTP** : Envoi d'emails
- **POP3/IMAP** : Réception d'emails
- **DNS** : Résolution de noms de domaine
- **DHCP** : Attribution automatique d'adresses IP
- **Telnet/SSH** : Accès à distance

**Applications :**
- Navigateur web (Chrome, Firefox)
- Client email (Outlook, Thunderbird)
- Client FTP (FileZilla)
- Messagerie (Slack, Teams, WhatsApp)

**Exemple concret :** Quand vous tapez `www.google.fr` dans votre navigateur, c'est la couche Application qui utilise le protocole **HTTP** pour récupérer la page web.

---

### D. Processus d'Encapsulation (Émission)

Quand Alice envoie un message à Bob :

<figure class="graphe diagramme"><svg viewBox="0 0 900 590" style="max-width:900px" role="img" aria-label="Encapsulation chez Alice puis désencapsulation chez Bob : chaque couche ajoute ou retire son en-tête autour du message" xmlns="http://www.w3.org/2000/svg">
<text x="180" y="26" text-anchor="middle" class="dg-t">Alice envoie : on ENCAPSULE</text>
<text x="730" y="26" text-anchor="middle" class="dg-t">Bob reçoit : on DÉSENCAPSULE</text>
<line x1="340" y1="473.0" x2="560" y2="473.0" class="dg-dash"/>
<rect x="360" y="44" width="160" height="58" rx="8" class="dg-box"/>
<text x="440.0" y="60.0" text-anchor="middle" class="dg-t">Couches 7, 6, 5</text>
<text x="440.0" y="78.0" text-anchor="middle" class="dg-s">Application</text>
<text x="440.0" y="96.0" text-anchor="middle" class="dg-s">Présentation, Session</text>
<rect x="360" y="144" width="160" height="58" rx="8" class="dg-ok"/>
<text x="440.0" y="169.0" text-anchor="middle" class="dg-t">Couche 4</text>
<text x="440.0" y="187.0" text-anchor="middle" class="dg-s">Transport</text>
<rect x="360" y="244" width="160" height="58" rx="8" class="dg-warn"/>
<text x="440.0" y="269.0" text-anchor="middle" class="dg-t">Couche 3</text>
<text x="440.0" y="287.0" text-anchor="middle" class="dg-s">Réseau</text>
<rect x="360" y="344" width="160" height="58" rx="8" class="dg-bad"/>
<text x="440.0" y="369.0" text-anchor="middle" class="dg-t">Couche 2</text>
<text x="440.0" y="387.0" text-anchor="middle" class="dg-s">Liaison</text>
<rect x="360" y="444" width="160" height="58" rx="8" class="dg-dark"/>
<text x="440.0" y="469.0" text-anchor="middle" class="dg-w">Couche 1</text>
<text x="440.0" y="487.0" text-anchor="middle" class="dg-s">Physique</text>
<rect x="14" y="50" width="110" height="46" rx="4" class="dg-box"/>
<text x="69.0" y="78.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="14" y="150" width="46" height="46" rx="4" class="dg-ok"/>
<text x="37.0" y="178.0" text-anchor="middle" class="dg-t">TCP</text>
<rect x="60" y="150" width="110" height="46" rx="4" class="dg-box"/>
<text x="115.0" y="178.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="14" y="250" width="40" height="46" rx="4" class="dg-warn"/>
<text x="34.0" y="278.0" text-anchor="middle" class="dg-t">IP</text>
<rect x="54" y="250" width="46" height="46" rx="4" class="dg-ok"/>
<text x="77.0" y="278.0" text-anchor="middle" class="dg-t">TCP</text>
<rect x="100" y="250" width="110" height="46" rx="4" class="dg-box"/>
<text x="155.0" y="278.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="14" y="350" width="46" height="46" rx="4" class="dg-bad"/>
<text x="37.0" y="378.0" text-anchor="middle" class="dg-t">ETH</text>
<rect x="60" y="350" width="40" height="46" rx="4" class="dg-warn"/>
<text x="80.0" y="378.0" text-anchor="middle" class="dg-t">IP</text>
<rect x="100" y="350" width="46" height="46" rx="4" class="dg-ok"/>
<text x="123.0" y="378.0" text-anchor="middle" class="dg-t">TCP</text>
<rect x="146" y="350" width="110" height="46" rx="4" class="dg-box"/>
<text x="201.0" y="378.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="256" y="350" width="44" height="46" rx="4" class="dg-bad"/>
<text x="278.0" y="378.0" text-anchor="middle" class="dg-t">FCS</text>
<rect x="14" y="450" width="326" height="46" rx="8" class="dg-dark"/>
<text x="177.0" y="478.0" text-anchor="middle" class="dg-w">0 1 0 1 1 0 0 1 0 1 1 0 …</text>
<rect x="560" y="50" width="110" height="46" rx="4" class="dg-box"/>
<text x="615.0" y="78.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="560" y="150" width="46" height="46" rx="4" class="dg-ok"/>
<text x="583.0" y="178.0" text-anchor="middle" class="dg-t">TCP</text>
<rect x="606" y="150" width="110" height="46" rx="4" class="dg-box"/>
<text x="661.0" y="178.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="560" y="250" width="40" height="46" rx="4" class="dg-warn"/>
<text x="580.0" y="278.0" text-anchor="middle" class="dg-t">IP</text>
<rect x="600" y="250" width="46" height="46" rx="4" class="dg-ok"/>
<text x="623.0" y="278.0" text-anchor="middle" class="dg-t">TCP</text>
<rect x="646" y="250" width="110" height="46" rx="4" class="dg-box"/>
<text x="701.0" y="278.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="560" y="350" width="46" height="46" rx="4" class="dg-bad"/>
<text x="583.0" y="378.0" text-anchor="middle" class="dg-t">ETH</text>
<rect x="606" y="350" width="40" height="46" rx="4" class="dg-warn"/>
<text x="626.0" y="378.0" text-anchor="middle" class="dg-t">IP</text>
<rect x="646" y="350" width="46" height="46" rx="4" class="dg-ok"/>
<text x="669.0" y="378.0" text-anchor="middle" class="dg-t">TCP</text>
<rect x="692" y="350" width="110" height="46" rx="4" class="dg-box"/>
<text x="747.0" y="378.0" text-anchor="middle" class="dg-t">Message</text>
<rect x="802" y="350" width="44" height="46" rx="4" class="dg-bad"/>
<text x="824.0" y="378.0" text-anchor="middle" class="dg-t">FCS</text>
<rect x="560" y="450" width="326" height="46" rx="8" class="dg-dark"/>
<text x="723.0" y="478.0" text-anchor="middle" class="dg-w">0 1 0 1 1 0 0 1 0 1 1 0 …</text>
<text x="132" y="78.0" text-anchor="start" class="dg-s">Données</text>
<text x="678" y="78.0" text-anchor="start" class="dg-s">Données</text>
<text x="178" y="178.0" text-anchor="start" class="dg-s">Segment</text>
<text x="724" y="178.0" text-anchor="start" class="dg-s">Segment</text>
<text x="218" y="278.0" text-anchor="start" class="dg-s">Paquet</text>
<text x="764" y="278.0" text-anchor="start" class="dg-s">Paquet</text>
<text x="308" y="378.0" text-anchor="start" class="dg-s">Trame</text>
<text x="854" y="378.0" text-anchor="start" class="dg-s">Trame</text>
<line x1="60" y1="100" x2="60.0" y2="137.0" class="dg-line"/>
<polygon points="60,146 65.0,137.0 55.0,137.0" class="dg-ah"/>
<text x="72" y="128.0" text-anchor="start" class="dg-i">+ en-tête TCP (ports)</text>
<line x1="60" y1="200" x2="60.0" y2="237.0" class="dg-line"/>
<polygon points="60,246 65.0,237.0 55.0,237.0" class="dg-ah"/>
<text x="72" y="228.0" text-anchor="start" class="dg-i">+ en-tête IP (adresses IP)</text>
<line x1="60" y1="300" x2="60.0" y2="337.0" class="dg-line"/>
<polygon points="60,346 65.0,337.0 55.0,337.0" class="dg-ah"/>
<text x="72" y="328.0" text-anchor="start" class="dg-i">+ en-tête et fin de trame (adresses MAC)</text>
<line x1="606" y1="146" x2="606.0" y2="109.0" class="dg-line"/>
<polygon points="606,100 601.0,109.0 611.0,109.0" class="dg-ah"/>
<text x="618" y="128.0" text-anchor="start" class="dg-i">− en-tête TCP</text>
<line x1="606" y1="246" x2="606.0" y2="209.0" class="dg-line"/>
<polygon points="606,200 601.0,209.0 611.0,209.0" class="dg-ah"/>
<text x="618" y="228.0" text-anchor="start" class="dg-i">− en-tête IP</text>
<line x1="606" y1="346" x2="606.0" y2="309.0" class="dg-line"/>
<polygon points="606,300 601.0,309.0 611.0,309.0" class="dg-ah"/>
<text x="618" y="328.0" text-anchor="start" class="dg-i">− en-tête et fin de trame</text>
<text x="450" y="530" text-anchor="middle" class="dg-i">transmission sur le câble RJ45 ou en Wi-Fi</text>
<rect x="90" y="552" width="16" height="16" rx="3" class="dg-box"/>
<text x="112" y="565" text-anchor="start" class="dg-s">Message : données</text>
<rect x="240" y="552" width="16" height="16" rx="3" class="dg-ok"/>
<text x="262" y="565" text-anchor="start" class="dg-s">TCP : ports</text>
<rect x="370" y="552" width="16" height="16" rx="3" class="dg-warn"/>
<text x="392" y="565" text-anchor="start" class="dg-s">IP : adresses IP</text>
<rect x="530" y="552" width="16" height="16" rx="3" class="dg-bad"/>
<text x="552" y="565" text-anchor="start" class="dg-s">ETH / FCS : adresses MAC, contrôle d&#x27;erreur</text>
</svg>
</figure>

| **Couche** | **Unité de données (PDU)** | **Ce que la couche ajoute** |
|------------|----------------------------|-----------------------------|
| 7, 6, 5 | **Données** | le message lui-même, mis en forme (chiffrement, encodage, session) |
| 4 - Transport | **Segment** | en-tête **TCP** : ports source et destination |
| 3 - Réseau | **Paquet** | en-tête **IP** : adresses IP source et destination |
| 2 - Liaison | **Trame** | en-tête **Ethernet** (adresses MAC) et contrôle d'erreur **FCS** |
| 1 - Physique | **Bits** | conversion en signaux électriques, lumineux ou radio |

 **Analogie Poupées Russes :** Chaque couche ajoute une "enveloppe" (en-tête). À l'arrivée, on retire les enveloppes une par une jusqu'au message original.

---

## III. Le Modèle TCP/IP (Modèle Internet)

### A. Différence avec OSI

Le **modèle TCP/IP** est le modèle **réellement utilisé** sur Internet. Il simplifie OSI en **4 couches**.

| **Couche TCP/IP** | **Correspond aux couches OSI** | **Protocoles Principaux** |
|-------------------|--------------------------------|---------------------------|
| **4 - Application** | 5, 6, 7 (Session, Présentation, Application) | HTTP, FTP, SMTP, DNS, DHCP |
| **3 - Transport** | 4 (Transport) | TCP, UDP |
| **2 - Internet** | 3 (Réseau) | IP, ICMP, ARP |
| **1 - Accès Réseau** | 1, 2 (Physique, Liaison) | Ethernet, Wi-Fi |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 410.60 592.60" style="max-width:548px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 581.8)">
<g class="cluster cl">
<path fill="transparent" stroke="black" d="M20,-8C20,-8 175,-8 175,-8 181,-8 187,-14 187,-20 187,-20 187,-551 187,-551 187,-557 181,-563 175,-563 175,-563 20,-563 20,-563 14,-563 8,-557 8,-551 8,-551 8,-20 8,-20 8,-14 14,-8 20,-8"/>
<text text-anchor="start" x="16" y="-547.8" font-family="Helvetica,sans-Serif" font-size="14.00">OSI</text>
</g>
<g class="cluster cl">
<path fill="transparent" stroke="black" d="M225,-85C225,-85 369,-85 369,-85 375,-85 381,-91 381,-97 381,-97 381,-397 381,-397 381,-403 375,-409 369,-409 369,-409 225,-409 225,-409 219,-409 213,-403 213,-397 213,-397 213,-97 213,-97 213,-91 219,-85 225,-85"/>
<text text-anchor="start" x="221" y="-393.8" font-family="Helvetica,sans-Serif" font-size="14.00">TCP/IP</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M154.5,-523.5C154.5,-523.5 40.5,-523.5 40.5,-523.5 34.5,-523.5 28.5,-517.5 28.5,-511.5 28.5,-511.5 28.5,-498.5 28.5,-498.5 28.5,-492.5 34.5,-486.5 40.5,-486.5 40.5,-486.5 154.5,-486.5 154.5,-486.5 160.5,-486.5 166.5,-492.5 166.5,-498.5 166.5,-498.5 166.5,-511.5 166.5,-511.5 166.5,-517.5 160.5,-523.5 154.5,-523.5"/>
<text text-anchor="start" x="47.5" y="-501.5" font-family="Helvetica,sans-Serif" font-size="15.00">7 Application</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M347,-369.5C347,-369.5 247,-369.5 247,-369.5 241,-369.5 235,-363.5 235,-357.5 235,-357.5 235,-344.5 235,-344.5 235,-338.5 241,-332.5 247,-332.5 247,-332.5 347,-332.5 347,-332.5 353,-332.5 359,-338.5 359,-344.5 359,-344.5 359,-357.5 359,-357.5 359,-363.5 353,-369.5 347,-369.5"/>
<text text-anchor="start" x="254" y="-347.5" font-family="Helvetica,sans-Serif" font-size="15.00">Application</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M151.4,-486.49C163.61,-481.1 176.18,-474.58 187,-467 226.84,-439.1 263.42,-394.73 282.39,-369.73"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M159,-446.5C159,-446.5 36,-446.5 36,-446.5 30,-446.5 24,-440.5 24,-434.5 24,-434.5 24,-421.5 24,-421.5 24,-415.5 30,-409.5 36,-409.5 36,-409.5 159,-409.5 159,-409.5 165,-409.5 171,-415.5 171,-421.5 171,-421.5 171,-434.5 171,-434.5 171,-440.5 165,-446.5 159,-446.5"/>
<text text-anchor="start" x="43" y="-424.5" font-family="Helvetica,sans-Serif" font-size="15.00">6 Présentation</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M146.06,-409.46C177.1,-397.35 217.48,-381.61 248.51,-369.51"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M140.5,-369.5C140.5,-369.5 54.5,-369.5 54.5,-369.5 48.5,-369.5 42.5,-363.5 42.5,-357.5 42.5,-357.5 42.5,-344.5 42.5,-344.5 42.5,-338.5 48.5,-332.5 54.5,-332.5 54.5,-332.5 140.5,-332.5 140.5,-332.5 146.5,-332.5 152.5,-338.5 152.5,-344.5 152.5,-344.5 152.5,-357.5 152.5,-357.5 152.5,-363.5 146.5,-369.5 140.5,-369.5"/>
<text text-anchor="start" x="61.5" y="-347.5" font-family="Helvetica,sans-Serif" font-size="15.00">5 Session</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M152.73,-351C178.19,-351 208.66,-351 234.93,-351"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M147,-292.5C147,-292.5 48,-292.5 48,-292.5 42,-292.5 36,-286.5 36,-280.5 36,-280.5 36,-267.5 36,-267.5 36,-261.5 42,-255.5 48,-255.5 48,-255.5 147,-255.5 147,-255.5 153,-255.5 159,-261.5 159,-267.5 159,-267.5 159,-280.5 159,-280.5 159,-286.5 153,-292.5 147,-292.5"/>
<text text-anchor="start" x="55" y="-270.5" font-family="Helvetica,sans-Serif" font-size="15.00">4 Transport</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M339,-292.5C339,-292.5 255,-292.5 255,-292.5 249,-292.5 243,-286.5 243,-280.5 243,-280.5 243,-267.5 243,-267.5 243,-261.5 249,-255.5 255,-255.5 255,-255.5 339,-255.5 339,-255.5 345,-255.5 351,-261.5 351,-267.5 351,-267.5 351,-280.5 351,-280.5 351,-286.5 345,-292.5 339,-292.5"/>
<text text-anchor="start" x="262" y="-270.5" font-family="Helvetica,sans-Serif" font-size="15.00">Transport</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M159.1,-274C185.86,-274 217.04,-274 242.87,-274"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M139,-215.5C139,-215.5 56,-215.5 56,-215.5 50,-215.5 44,-209.5 44,-203.5 44,-203.5 44,-190.5 44,-190.5 44,-184.5 50,-178.5 56,-178.5 56,-178.5 139,-178.5 139,-178.5 145,-178.5 151,-184.5 151,-190.5 151,-190.5 151,-203.5 151,-203.5 151,-209.5 145,-215.5 139,-215.5"/>
<text text-anchor="start" x="63" y="-193.5" font-family="Helvetica,sans-Serif" font-size="15.00">3 Réseau</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M334.5,-215.5C334.5,-215.5 259.5,-215.5 259.5,-215.5 253.5,-215.5 247.5,-209.5 247.5,-203.5 247.5,-203.5 247.5,-190.5 247.5,-190.5 247.5,-184.5 253.5,-178.5 259.5,-178.5 259.5,-178.5 334.5,-178.5 334.5,-178.5 340.5,-178.5 346.5,-184.5 346.5,-190.5 346.5,-190.5 346.5,-203.5 346.5,-203.5 346.5,-209.5 340.5,-215.5 334.5,-215.5"/>
<text text-anchor="start" x="266.5" y="-193.5" font-family="Helvetica,sans-Serif" font-size="15.00">Internet</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M151.17,-197C180.99,-197 218.05,-197 247.21,-197"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M138,-138.5C138,-138.5 57,-138.5 57,-138.5 51,-138.5 45,-132.5 45,-126.5 45,-126.5 45,-113.5 45,-113.5 45,-107.5 51,-101.5 57,-101.5 57,-101.5 138,-101.5 138,-101.5 144,-101.5 150,-107.5 150,-113.5 150,-113.5 150,-126.5 150,-126.5 150,-132.5 144,-138.5 138,-138.5"/>
<text text-anchor="start" x="64" y="-116.5" font-family="Helvetica,sans-Serif" font-size="15.00">2 Liaison</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M353,-138.5C353,-138.5 241,-138.5 241,-138.5 235,-138.5 229,-132.5 229,-126.5 229,-126.5 229,-113.5 229,-113.5 229,-107.5 235,-101.5 241,-101.5 241,-101.5 353,-101.5 353,-101.5 359,-101.5 365,-107.5 365,-113.5 365,-113.5 365,-126.5 365,-126.5 365,-132.5 359,-138.5 353,-138.5"/>
<text text-anchor="start" x="248" y="-116.5" font-family="Helvetica,sans-Serif" font-size="15.00">Accès réseau</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M150.13,-120C174.13,-120 202.99,-120 228.67,-120"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M146,-61.5C146,-61.5 49,-61.5 49,-61.5 43,-61.5 37,-55.5 37,-49.5 37,-49.5 37,-36.5 37,-36.5 37,-30.5 43,-24.5 49,-24.5 49,-24.5 146,-24.5 146,-24.5 152,-24.5 158,-30.5 158,-36.5 158,-36.5 158,-49.5 158,-49.5 158,-55.5 152,-61.5 146,-61.5"/>
<text text-anchor="start" x="56" y="-39.5" font-family="Helvetica,sans-Serif" font-size="15.00">1 Physique</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M146.06,-61.54C177.1,-73.65 217.48,-89.39 248.51,-101.49"/>
</g>
</g>
</svg>
</figure>
*Correspondance entre le modèle OSI (7 couches) et le modèle TCP/IP (4 couches).*
**Pourquoi deux modèles ?**
- **OSI** : Modèle théorique, pédagogique (ISO, 1984)
- **TCP/IP** : Modèle pratique, utilisé sur Internet (ARPANET, 1970s)

 **À retenir :** On enseigne OSI pour **comprendre**, on utilise TCP/IP en **pratique**.

---

## IV. Types de Réseaux

### A. Classification par Taille (Portée Géographique)

| **Type** | **Nom Complet** | **Portée** | **Exemples** |
|----------|-----------------|------------|--------------|
| **PAN** | Personal Area Network | 1-10 mètres | Bluetooth (casque ↔ smartphone), USB |
| **LAN** | Local Area Network | Bâtiment, campus (100m-1km) | Réseau d'entreprise, école, domicile |
| **MAN** | Metropolitan Area Network | Ville (10-100 km) | Réseau universitaire multi-sites, réseau municipal |
| **WAN** | Wide Area Network | Pays, continent, monde | Internet, réseau d'entreprise multi-pays |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 240.60 411.60" style="max-width:321px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 400.8)">
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M201.5,-390C201.5,-390 17.5,-390 17.5,-390 11.5,-390 5.5,-384 5.5,-378 5.5,-378 5.5,-348 5.5,-348 5.5,-342 11.5,-336 17.5,-336 17.5,-336 201.5,-336 201.5,-336 207.5,-336 213.5,-342 213.5,-348 213.5,-348 213.5,-378 213.5,-378 213.5,-384 207.5,-390 201.5,-390"/>
<text text-anchor="start" x="24.5" y="-368" font-family="Helvetica,sans-Serif" font-size="15.00">PAN : quelques mètres</text>
<text text-anchor="start" x="52" y="-351" font-family="Helvetica,sans-Serif" font-size="15.00">Bluetooth, USB</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M207,-278C207,-278 12,-278 12,-278 6,-278 0,-272 0,-266 0,-266 0,-236 0,-236 0,-230 6,-224 12,-224 12,-224 207,-224 207,-224 213,-224 219,-230 219,-236 219,-236 219,-266 219,-266 219,-272 213,-278 207,-278"/>
<text text-anchor="start" x="19" y="-256" font-family="Helvetica,sans-Serif" font-size="15.00">LAN : bâtiment, campus</text>
<text text-anchor="start" x="37" y="-239" font-family="Helvetica,sans-Serif" font-size="15.00">réseau d&#39;entreprise</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M109.5,-335.93C109.5,-321.52 109.5,-303.39 109.5,-287.68"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="112.65,-287.26 109.5,-278.26 106.35,-287.26 112.65,-287.26"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M181.5,-166C181.5,-166 37.5,-166 37.5,-166 31.5,-166 25.5,-160 25.5,-154 25.5,-154 25.5,-124 25.5,-124 25.5,-118 31.5,-112 37.5,-112 37.5,-112 181.5,-112 181.5,-112 187.5,-112 193.5,-118 193.5,-124 193.5,-124 193.5,-154 193.5,-154 193.5,-160 187.5,-166 181.5,-166"/>
<text text-anchor="start" x="69" y="-144" font-family="Helvetica,sans-Serif" font-size="15.00">MAN : ville</text>
<text text-anchor="start" x="44.5" y="-127" font-family="Helvetica,sans-Serif" font-size="15.00">réseau municipal</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M109.5,-223.93C109.5,-209.52 109.5,-191.39 109.5,-175.68"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="112.65,-175.26 109.5,-166.26 106.35,-175.26 112.65,-175.26"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M190,-54C190,-54 29,-54 29,-54 23,-54 17,-48 17,-42 17,-42 17,-12 17,-12 17,-6 23,0 29,0 29,0 190,0 190,0 196,0 202,-6 202,-12 202,-12 202,-42 202,-42 202,-48 196,-54 190,-54"/>
<text text-anchor="start" x="36" y="-32" font-family="Helvetica,sans-Serif" font-size="15.00">WAN : pays, monde</text>
<text text-anchor="start" x="79" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">Internet</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M109.5,-111.93C109.5,-97.52 109.5,-79.39 109.5,-63.68"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="112.65,-63.26 109.5,-54.26 106.35,-63.26 112.65,-63.26"/>
</g>
</g>
</svg>
</figure>
*Les échelles de réseaux, du PAN (le plus petit) au WAN (le plus grand).*
---

### B. Classification par Topologie (Architecture Physique)

**Topologie** = Manière dont les équipements sont **physiquement reliés**.

#### **1. Topologie en Bus**

<figure class="graphe diagramme"><svg viewBox="0 0 680 230" style="max-width:680px" role="img" aria-label="Topologie en bus : un câble unique relie tous les postes" xmlns="http://www.w3.org/2000/svg">
<rect x="30" y="100" width="620" height="10" rx="5" class="dg-dark"/>
<rect x="24" y="92" width="10" height="26" rx="2" class="dg-bad"/>
<rect x="646" y="92" width="10" height="26" rx="2" class="dg-bad"/>
<text x="40" y="84" text-anchor="start" class="dg-i">terminateur</text>
<text x="640" y="84" text-anchor="end" class="dg-i">terminateur</text>
<line x1="130" y1="110" x2="130" y2="140" class="dg-line"/>
<rect x="110" y="142" width="40" height="28" rx="4" class="dg-box"/>
<rect x="115" y="147" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="127" y="170" width="6" height="6" class="dg-box"/>
<rect x="118" y="176" width="24" height="4" rx="2" class="dg-box"/>
<text x="130" y="196" text-anchor="middle" class="dg-s">PC1</text>
<line x1="270" y1="110" x2="270" y2="140" class="dg-line"/>
<rect x="250" y="142" width="40" height="28" rx="4" class="dg-box"/>
<rect x="255" y="147" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="267" y="170" width="6" height="6" class="dg-box"/>
<rect x="258" y="176" width="24" height="4" rx="2" class="dg-box"/>
<text x="270" y="196" text-anchor="middle" class="dg-s">PC2</text>
<line x1="410" y1="110" x2="410" y2="140" class="dg-line"/>
<rect x="390" y="142" width="40" height="28" rx="4" class="dg-box"/>
<rect x="395" y="147" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="407" y="170" width="6" height="6" class="dg-box"/>
<rect x="398" y="176" width="24" height="4" rx="2" class="dg-box"/>
<text x="410" y="196" text-anchor="middle" class="dg-s">PC3</text>
<line x1="550" y1="110" x2="550" y2="140" class="dg-line"/>
<rect x="530" y="142" width="40" height="28" rx="4" class="dg-box"/>
<rect x="535" y="147" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="547" y="170" width="6" height="6" class="dg-box"/>
<rect x="538" y="176" width="24" height="4" rx="2" class="dg-box"/>
<text x="550" y="196" text-anchor="middle" class="dg-s">PC4</text>
<text x="340" y="92" text-anchor="middle" class="dg-t">câble coaxial unique (le « bus »)</text>
</svg>
</figure>

**Caractéristiques :**
- **Avantage :** Simple, économique (1 seul câble)
- **Inconvénient :** Si câble coupé → tout le réseau tombe
- **Inconvénient :** Collisions fréquentes (une seule machine parle à la fois)

**Usage :** Obsolète (années 1980-1990)

---

#### **2. Topologie en Étoile**

<figure class="graphe diagramme"><svg viewBox="0 0 680 330" style="max-width:680px" role="img" aria-label="Topologie en étoile : chaque poste est relié à un équipement central" xmlns="http://www.w3.org/2000/svg">
<rect x="298.0" y="148" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="306.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="318.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="330.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="342.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="354.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="366.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<line x1="320.1370685858735" y1="142.54146123136834" x2="153.4743734894222" y2="63.178273090201046" class="dg-line"/>
<line x1="340.0" y1="130.0" x2="340.0" y2="58.0" class="dg-line"/>
<line x1="359.8629314141265" y1="142.54146123136834" x2="526.5256265105778" y2="63.178273090201046" class="dg-line"/>
<line x1="319.77880933960273" y1="160.66622456874168" x2="153.8977707804695" y2="231.75809823694163" class="dg-line"/>
<line x1="340.0" y1="174.0" x2="340.0" y2="236.0" class="dg-line"/>
<line x1="360.22119066039727" y1="160.66622456874168" x2="526.1022292195305" y2="231.75809823694163" class="dg-line"/>
<rect x="110" y="36" width="40" height="28" rx="4" class="dg-box"/>
<rect x="115" y="41" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="127" y="64" width="6" height="6" class="dg-box"/>
<rect x="118" y="70" width="24" height="4" rx="2" class="dg-box"/>
<text x="130" y="30" text-anchor="middle" class="dg-s">PC1</text>
<rect x="320" y="16" width="40" height="28" rx="4" class="dg-box"/>
<rect x="325" y="21" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="337" y="44" width="6" height="6" class="dg-box"/>
<rect x="328" y="50" width="24" height="4" rx="2" class="dg-box"/>
<text x="340" y="10" text-anchor="middle" class="dg-s">PC2</text>
<rect x="530" y="36" width="40" height="28" rx="4" class="dg-box"/>
<rect x="535" y="41" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="547" y="64" width="6" height="6" class="dg-box"/>
<rect x="538" y="70" width="24" height="4" rx="2" class="dg-box"/>
<text x="550" y="30" text-anchor="middle" class="dg-s">PC3</text>
<rect x="110" y="226" width="40" height="28" rx="4" class="dg-box"/>
<rect x="115" y="231" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="127" y="254" width="6" height="6" class="dg-box"/>
<rect x="118" y="260" width="24" height="4" rx="2" class="dg-box"/>
<text x="130" y="280" text-anchor="middle" class="dg-s">PC4</text>
<rect x="320" y="246" width="40" height="28" rx="4" class="dg-box"/>
<rect x="325" y="251" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="337" y="274" width="6" height="6" class="dg-box"/>
<rect x="328" y="280" width="24" height="4" rx="2" class="dg-box"/>
<text x="340" y="300" text-anchor="middle" class="dg-s">PC5</text>
<rect x="530" y="226" width="40" height="28" rx="4" class="dg-box"/>
<rect x="535" y="231" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="547" y="254" width="6" height="6" class="dg-box"/>
<rect x="538" y="260" width="24" height="4" rx="2" class="dg-box"/>
<text x="550" y="280" text-anchor="middle" class="dg-s">PC6</text>
<rect x="298.0" y="148" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="306.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="318.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="330.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="342.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="354.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="366.0" y="156" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<text x="340" y="190" text-anchor="middle" class="dg-s">Switch</text>
</svg>
</figure>

**Caractéristiques :**
- **Avantage :** Si un câble casse → seul ce PC est déconnecté
- **Avantage :** Facilité de dépannage (isolation des pannes)
- **Avantage :** Ajout/retrait de PC simple
- **Inconvénient :** Si le switch tombe → tout le réseau tombe
- **Inconvénient :** Plus de câble nécessaire

**Usage :** **Standard de fait** des réseaux locaux filaires

---

#### **3. Topologie en Anneau**

<figure class="graphe diagramme"><svg viewBox="0 0 560 330" style="max-width:560px" role="img" aria-label="Topologie en anneau : chaque poste est relié à deux voisins, les données tournent dans un sens" xmlns="http://www.w3.org/2000/svg">
<line x1="303.90559321444823" y1="52.22363013148102" x2="384.6991792475409" y2="86.77636986851898" class="dg-line"/>
<polygon points="350.7,72.3 337.7,72.1 341.7,62.9" class="dg-ah"/>
<line x1="408.6047724619891" y1="123.0" x2="408.60477246198917" y2="180.99999999999997" class="dg-line"/>
<polygon points="408.6,159.0 403.6,147.0 413.6,147.0" class="dg-ah"/>
<line x1="384.69917924754094" y1="217.223630131481" x2="303.90559321444823" y2="251.776369868519" class="dg-line"/>
<polygon points="337.9,237.3 346.9,227.9 350.9,237.1" class="dg-ah"/>
<line x1="256.09440678555177" y1="251.776369868519" x2="175.30082075245912" y2="217.22363013148106" class="dg-line"/>
<polygon points="209.3,231.7 222.3,231.9 218.3,241.1" class="dg-ah"/>
<line x1="151.3952275380109" y1="181.00000000000003" x2="151.39522753801086" y2="123.0" class="dg-line"/>
<polygon points="151.4,145.0 156.4,157.0 146.4,157.0" class="dg-ah"/>
<line x1="175.3008207524591" y1="86.77636986851897" x2="256.09440678555177" y2="52.223630131481016" class="dg-line"/>
<polygon points="222.1,66.7 213.1,76.1 209.1,66.9" class="dg-ah"/>
<rect x="260.0" y="26.0" width="40" height="28" rx="4" class="dg-box"/>
<rect x="265.0" y="31.0" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="277.0" y="54.0" width="6" height="6" class="dg-box"/>
<rect x="268.0" y="60.0" width="24" height="4" rx="2" class="dg-box"/>
<text x="280.0" y="20.0" text-anchor="middle" class="dg-s">PC1</text>
<rect x="388.6047724619891" y="81.0" width="40" height="28" rx="4" class="dg-box"/>
<rect x="393.6047724619891" y="86.0" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="405.6047724619891" y="109.0" width="6" height="6" class="dg-box"/>
<rect x="396.6047724619891" y="115.0" width="24" height="4" rx="2" class="dg-box"/>
<text x="408.6047724619891" y="75.0" text-anchor="middle" class="dg-s">PC2</text>
<rect x="388.60477246198917" y="190.99999999999997" width="40" height="28" rx="4" class="dg-box"/>
<rect x="393.60477246198917" y="195.99999999999997" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="405.60477246198917" y="218.99999999999997" width="6" height="6" class="dg-box"/>
<rect x="396.60477246198917" y="224.99999999999997" width="24" height="4" rx="2" class="dg-box"/>
<text x="408.60477246198917" y="244.99999999999997" text-anchor="middle" class="dg-s">PC3</text>
<rect x="260.0" y="246.0" width="40" height="28" rx="4" class="dg-box"/>
<rect x="265.0" y="251.0" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="277.0" y="274.0" width="6" height="6" class="dg-box"/>
<rect x="268.0" y="280.0" width="24" height="4" rx="2" class="dg-box"/>
<text x="280.0" y="300.0" text-anchor="middle" class="dg-s">PC4</text>
<rect x="131.3952275380109" y="191.00000000000003" width="40" height="28" rx="4" class="dg-box"/>
<rect x="136.3952275380109" y="196.00000000000003" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="148.3952275380109" y="219.00000000000003" width="6" height="6" class="dg-box"/>
<rect x="139.3952275380109" y="225.00000000000003" width="24" height="4" rx="2" class="dg-box"/>
<text x="151.3952275380109" y="245.00000000000003" text-anchor="middle" class="dg-s">PC5</text>
<rect x="131.39522753801086" y="80.99999999999999" width="40" height="28" rx="4" class="dg-box"/>
<rect x="136.39522753801086" y="85.99999999999999" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="148.39522753801086" y="108.99999999999999" width="6" height="6" class="dg-box"/>
<rect x="139.39522753801086" y="114.99999999999999" width="24" height="4" rx="2" class="dg-box"/>
<text x="151.39522753801086" y="74.99999999999999" text-anchor="middle" class="dg-s">PC6</text>
<text x="280" y="164" text-anchor="middle" class="dg-i">sens de circulation</text>
</svg>
</figure>

**Caractéristiques :**
- Chaque PC est relié à deux autres (forme un anneau)
- Les données circulent dans un sens
- **Inconvénient :** Si un PC tombe → tout le réseau tombe (sauf anneau double)

**Usage :** Rare (Token Ring, FDDI - obsolète)

---

#### **4. Topologie Maillée (Mesh)**

<figure class="graphe diagramme"><svg viewBox="0 0 560 340" style="max-width:560px" role="img" aria-label="Topologie maillée : chaque poste est relié à tous les autres (ici 5 postes, 10 liens)" xmlns="http://www.w3.org/2000/svg">
<line x1="302.6959956778647" y1="54.68431236568811" x2="405.6688208641793" y2="112.23364830931821" class="dg-line"/>
<line x1="290.1167972476068" y1="65.95100025992265" x2="361.577702110019" y2="235.1310390650711" class="dg-line"/>
<line x1="269.8832027523932" y1="65.95100025992265" x2="198.42229788998105" y2="235.13103906507106" class="dg-line"/>
<line x1="257.3040043221353" y1="54.68431236568811" x2="154.3311791358207" y2="112.23364830931821" class="dg-line"/>
<line x1="418.24801929443714" y1="148.868960934929" x2="381.81129660523266" y2="235.13103906507106" class="dg-line"/>
<line x1="405.6688208641793" y1="137.60227304069443" x2="211.00149632023889" y2="246.39772695930563" class="dg-line"/>
<line x1="402.36481654204397" y1="124.91796067500633" x2="157.63518345795603" y2="124.91796067500633" class="dg-line"/>
<line x1="345.69449935762583" y1="259.08203932499373" x2="214.30550064237423" y2="259.08203932499373" class="dg-line"/>
<line x1="348.99850367976114" y1="246.39772695930563" x2="154.3311791358207" y2="137.60227304069443" class="dg-line"/>
<line x1="178.1887033947674" y1="235.1310390650711" x2="141.75198070556286" y2="148.86896093492896" class="dg-line"/>
<rect x="260.0" y="26.0" width="40" height="28" rx="4" class="dg-box"/>
<rect x="265.0" y="31.0" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="277.0" y="54.0" width="6" height="6" class="dg-box"/>
<rect x="268.0" y="60.0" width="24" height="4" rx="2" class="dg-box"/>
<text x="280.0" y="20.0" text-anchor="middle" class="dg-s">PC1</text>
<rect x="408.36481654204397" y="108.91796067500633" width="40" height="28" rx="4" class="dg-box"/>
<rect x="413.36481654204397" y="113.91796067500633" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="425.36481654204397" y="136.91796067500633" width="6" height="6" class="dg-box"/>
<rect x="416.36481654204397" y="142.91796067500633" width="24" height="4" rx="2" class="dg-box"/>
<text x="428.36481654204397" y="162.91796067500633" text-anchor="middle" class="dg-s">PC2</text>
<rect x="351.69449935762583" y="243.08203932499373" width="40" height="28" rx="4" class="dg-box"/>
<rect x="356.69449935762583" y="248.08203932499373" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="368.69449935762583" y="271.08203932499373" width="6" height="6" class="dg-box"/>
<rect x="359.69449935762583" y="277.08203932499373" width="24" height="4" rx="2" class="dg-box"/>
<text x="371.69449935762583" y="297.08203932499373" text-anchor="middle" class="dg-s">PC3</text>
<rect x="168.30550064237423" y="243.08203932499373" width="40" height="28" rx="4" class="dg-box"/>
<rect x="173.30550064237423" y="248.08203932499373" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="185.30550064237423" y="271.08203932499373" width="6" height="6" class="dg-box"/>
<rect x="176.30550064237423" y="277.08203932499373" width="24" height="4" rx="2" class="dg-box"/>
<text x="188.30550064237423" y="297.08203932499373" text-anchor="middle" class="dg-s">PC4</text>
<rect x="111.63518345795603" y="108.91796067500633" width="40" height="28" rx="4" class="dg-box"/>
<rect x="116.63518345795603" y="113.91796067500633" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="128.63518345795603" y="136.91796067500633" width="6" height="6" class="dg-box"/>
<rect x="119.63518345795603" y="142.91796067500633" width="24" height="4" rx="2" class="dg-box"/>
<text x="131.63518345795603" y="162.91796067500633" text-anchor="middle" class="dg-s">PC5</text>
</svg>
</figure>

**Caractéristiques :**
- **Avantage :** Redondance maximale (plusieurs chemins possibles)
- **Avantage :** Si un lien tombe → routage automatique par un autre chemin
- **Inconvénient :** Coût élevé (beaucoup de câbles)
- **Inconvénient :** Complexité de configuration

**Usage :** Réseaux critiques (datacenters, opérateurs télécom), Internet (maillage partiel)

---

#### **5. Topologie en Arbre**

<figure class="graphe diagramme"><svg viewBox="0 0 700 360" style="max-width:700px" role="img" aria-label="Topologie en arbre : des étoiles reliées hiérarchiquement" xmlns="http://www.w3.org/2000/svg">
<line x1="335.95367083336987" y1="49.66163409088917" x2="144.04632916663016" y2="154.33836590911082" class="dg-line"/>
<line x1="350.0" y1="58.0" x2="350.0" y2="146.0" class="dg-line"/>
<line x1="364.04632916663013" y1="49.66163409088917" x2="555.9536708333699" y2="154.33836590911082" class="dg-line"/>
<rect x="308.0" y="38" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="316.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="328.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="340.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="352.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="364.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="376.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<line x1="121.12479686039633" y1="175.3128047094055" x2="64.42220510185595" y2="260.3666923472161" class="dg-line"/>
<line x1="130.0" y1="178.0" x2="130.0" y2="256.0" class="dg-line"/>
<line x1="138.87520313960366" y1="175.3128047094055" x2="195.57779489814405" y2="260.3666923472161" class="dg-line"/>
<line x1="341.1247968603963" y1="175.3128047094055" x2="284.422205101856" y2="260.3666923472161" class="dg-line"/>
<line x1="350.0" y1="178.0" x2="350.0" y2="256.0" class="dg-line"/>
<line x1="358.8752031396037" y1="175.3128047094055" x2="415.577794898144" y2="260.3666923472161" class="dg-line"/>
<line x1="561.1247968603964" y1="175.3128047094055" x2="504.422205101856" y2="260.3666923472161" class="dg-line"/>
<line x1="570.0" y1="178.0" x2="570.0" y2="256.0" class="dg-line"/>
<line x1="578.8752031396036" y1="175.3128047094055" x2="635.577794898144" y2="260.3666923472161" class="dg-line"/>
<rect x="88.0" y="158" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="96.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="108.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="120.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="132.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="144.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="156.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="30" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="35" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="47" y="294" width="6" height="6" class="dg-box"/>
<rect x="38" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="50" y="320" text-anchor="middle" class="dg-s">PC1</text>
<rect x="110" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="115" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="127" y="294" width="6" height="6" class="dg-box"/>
<rect x="118" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="130" y="320" text-anchor="middle" class="dg-s">PC2</text>
<rect x="190" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="195" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="207" y="294" width="6" height="6" class="dg-box"/>
<rect x="198" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="210" y="320" text-anchor="middle" class="dg-s">PC3</text>
<rect x="308.0" y="158" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="316.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="328.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="340.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="352.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="364.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="376.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="250" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="255" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="267" y="294" width="6" height="6" class="dg-box"/>
<rect x="258" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="270" y="320" text-anchor="middle" class="dg-s">PC4</text>
<rect x="330" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="335" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="347" y="294" width="6" height="6" class="dg-box"/>
<rect x="338" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="350" y="320" text-anchor="middle" class="dg-s">PC5</text>
<rect x="410" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="415" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="427" y="294" width="6" height="6" class="dg-box"/>
<rect x="418" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="430" y="320" text-anchor="middle" class="dg-s">PC6</text>
<rect x="528.0" y="158" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="536.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="548.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="560.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="572.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="584.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="596.0" y="166" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="470" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="475" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="487" y="294" width="6" height="6" class="dg-box"/>
<rect x="478" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="490" y="320" text-anchor="middle" class="dg-s">PC7</text>
<rect x="550" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="555" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="567" y="294" width="6" height="6" class="dg-box"/>
<rect x="558" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="570" y="320" text-anchor="middle" class="dg-s">PC8</text>
<rect x="630" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="635" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="647" y="294" width="6" height="6" class="dg-box"/>
<rect x="638" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="650" y="320" text-anchor="middle" class="dg-s">PC9</text>
<text x="350" y="28" text-anchor="middle" class="dg-s">Switch cœur (racine)</text>
<text x="130" y="150" text-anchor="middle" class="dg-s">Switch d&#x27;étage</text>
<text x="350" y="150" text-anchor="middle" class="dg-s">Switch d&#x27;étage</text>
<text x="570" y="150" text-anchor="middle" class="dg-s">Switch d&#x27;étage</text>
</svg>
</figure>

**Caractéristiques :**
- Organisation **hiérarchique** : un équipement racine (le cœur), des équipements intermédiaires, puis les postes. C'est une étoile d'étoiles.
- **Avantage :** réseau facile à étendre : on ajoute un switch et ses postes sur une branche.
- **Avantage :** on **isole** les pannes et on organise le réseau par étage, bâtiment ou service.
- **Inconvénient :** les liens entre switchs concentrent le trafic ; si le switch cœur tombe, toutes les branches sont coupées.

**Usage :** le plan de nombreux **réseaux d'entreprise** : cœur, distribution, accès.

---

#### **6. Topologie Hybride**

<figure class="graphe diagramme"><svg viewBox="0 0 740 390" style="max-width:740px" role="img" aria-label="Topologie hybride : des étoiles reliées par un cœur maillé" xmlns="http://www.w3.org/2000/svg">
<line x1="335.0813201031557" y1="138.77098792091397" x2="254.9186798968443" y2="200.22901207908603" class="dg-line"/>
<line x1="404.9186798968443" y1="138.77098792091397" x2="485.0813201031557" y2="200.22901207908603" class="dg-line"/>
<line x1="264.0" y1="227.0" x2="476.0" y2="227.0" class="dg-line"/>
<line x1="356.4320271359186" y1="103.52001695994912" x2="272.04795590413227" y2="50.77997244008268" class="dg-line"/>
<rect x="230" y="21" width="40" height="28" rx="4" class="dg-box"/>
<rect x="235" y="26" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="247" y="49" width="6" height="6" class="dg-box"/>
<rect x="238" y="55" width="24" height="4" rx="2" class="dg-box"/>
<text x="250" y="15" text-anchor="middle" class="dg-s">PC1</text>
<line x1="383.5679728640814" y1="103.52001695994912" x2="467.95204409586773" y2="50.77997244008268" class="dg-line"/>
<rect x="470" y="21" width="40" height="28" rx="4" class="dg-box"/>
<rect x="475" y="26" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="487" y="49" width="6" height="6" class="dg-box"/>
<rect x="478" y="55" width="24" height="4" rx="2" class="dg-box"/>
<text x="490" y="15" text-anchor="middle" class="dg-s">PC2</text>
<line x1="205.3191099950412" y1="220.63828099785118" x2="93.85644625805803" y2="172.33779337849182" class="dg-line"/>
<rect x="50" y="146" width="40" height="28" rx="4" class="dg-box"/>
<rect x="55" y="151" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="67" y="174" width="6" height="6" class="dg-box"/>
<rect x="58" y="180" width="24" height="4" rx="2" class="dg-box"/>
<text x="70" y="200" text-anchor="middle" class="dg-s">PC3</text>
<line x1="204.97797894581427" y1="232.50807438653476" x2="94.41078421305183" y2="273.049379121881" class="dg-line"/>
<rect x="50" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="55" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="67" y="294" width="6" height="6" class="dg-box"/>
<rect x="58" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="70" y="320" text-anchor="middle" class="dg-s">PC4</text>
<line x1="217.13783298880026" y1="242.7419185615985" x2="204.65102139319956" y2="311.4193823374024" class="dg-line"/>
<rect x="180" y="321" width="40" height="28" rx="4" class="dg-box"/>
<rect x="185" y="326" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="197" y="349" width="6" height="6" class="dg-box"/>
<rect x="188" y="355" width="24" height="4" rx="2" class="dg-box"/>
<text x="200" y="375" text-anchor="middle" class="dg-s">PC5</text>
<line x1="534.6808900049588" y1="220.63828099785118" x2="646.143553741942" y2="172.33779337849182" class="dg-line"/>
<rect x="650" y="146" width="40" height="28" rx="4" class="dg-box"/>
<rect x="655" y="151" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="667" y="174" width="6" height="6" class="dg-box"/>
<rect x="658" y="180" width="24" height="4" rx="2" class="dg-box"/>
<text x="670" y="200" text-anchor="middle" class="dg-s">PC6</text>
<line x1="535.0220210541858" y1="232.50807438653476" x2="645.5892157869482" y2="273.049379121881" class="dg-line"/>
<rect x="650" y="266" width="40" height="28" rx="4" class="dg-box"/>
<rect x="655" y="271" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="667" y="294" width="6" height="6" class="dg-box"/>
<rect x="658" y="300" width="24" height="4" rx="2" class="dg-box"/>
<text x="670" y="320" text-anchor="middle" class="dg-s">PC7</text>
<line x1="522.8621670111997" y1="242.7419185615985" x2="535.3489786068004" y2="311.4193823374024" class="dg-line"/>
<rect x="520" y="321" width="40" height="28" rx="4" class="dg-box"/>
<rect x="525" y="326" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="537" y="349" width="6" height="6" class="dg-box"/>
<rect x="528" y="355" width="24" height="4" rx="2" class="dg-box"/>
<text x="540" y="375" text-anchor="middle" class="dg-s">PC8</text>
<rect x="328.0" y="108" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="336.0" y="116" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="348.0" y="116" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="360.0" y="116" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="372.0" y="116" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="384.0" y="116" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="396.0" y="116" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="178.0" y="223" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="186.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="198.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="210.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="222.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="234.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="246.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="478.0" y="223" width="84" height="24" rx="4" class="dg-dark"/>
<rect x="486.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="498.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="510.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="522.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="534.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="546.0" y="231" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<text x="370" y="214" text-anchor="middle" class="dg-t">cœur maillé</text>
<text x="370" y="378" text-anchor="middle" class="dg-i">étoiles à l&#x27;accès + maillage au cœur</text>
</svg>
</figure>

**Caractéristiques :**
- **Combinaison** de plusieurs topologies : ici des étoiles (les postes) reliées par un cœur **maillé** (les switchs).
- **Avantage :** on profite de la simplicité de l'étoile à l'accès et de la **redondance** du maillage là où une panne coûterait cher.
- **Inconvénient :** plus complexe à concevoir, à documenter et à dépanner.

**Usage :** la **réalité** de presque tous les réseaux d'entreprise.

---

#### **Pour comparer**

| **Topologie** | **Principe** | **Point fort** | **Point faible** | **Usage actuel** |
|---------------|--------------|----------------|------------------|------------------|
| **Bus** | Un câble unique | Économique | Une coupure arrête tout | Obsolète |
| **Étoile** | Un équipement central | Pannes isolées, dépannage simple | Le centre est un point unique de défaillance | Standard des LAN filaires |
| **Anneau** | Chaque poste relié à deux voisins | Accès régulier au réseau | Une panne peut tout bloquer | Rare (Token Ring, FDDI) |
| **Maillée** | Tous reliés à tous | Redondance maximale | Coût et complexité | Datacenters, opérateurs |
| **Arbre** | Étoiles hiérarchisées | Extensible, organisé | Dépend du cœur | Réseaux d'entreprise |
| **Hybride** | Combinaison | S'adapte aux besoins | Complexe à gérer | Presque tous les réseaux réels |

---

## V. Adressage IPv4

### A. Structure d'une Adresse IPv4

**Adresse IPv4** = Identifiant **unique** d'une machine sur un réseau IP.

**Format :** 4 octets séparés par des points (notation décimale pointée)

**Exemple :** `192.168.1.10`

**En binaire :**
```
192      .  168      .  1        .  10
11000000 .  10101000 .  00000001 .  00001010
```

**Valeurs possibles par octet :** 0 à 255 (= 2^8 = 256 valeurs)

**Nombre total d'adresses IPv4 :** 2^32 = **4 294 967 296 adresses** (~4,3 milliards)

**Problème :** Épuisement des adresses IPv4 (8 milliards d'humains, des milliards d'objets connectés) → Solution : **IPv6** (340 sextillions d'adresses !)

---

### B. Classes d'Adresses IPv4 (Historique)

Les adresses IPv4 étaient autrefois divisées en **5 classes** (A, B, C, D, E).

| **Classe** | **Plage** | **Masque par défaut** | **Usage** | **Nb d'hôtes** |
|------------|-----------|----------------------|-----------|----------------|
| **A** | 1.0.0.0 à 126.255.255.255 | 255.0.0.0 (/8) | Très grands réseaux (pays, ISP) | ~16 millions |
| **B** | 128.0.0.0 à 191.255.255.255 | 255.255.0.0 (/16) | Grandes entreprises | ~65 000 |
| **C** | 192.0.0.0 à 223.255.255.255 | 255.255.255.0 (/24) | PME, petits réseaux | 254 |
| **D** | 224.0.0.0 à 239.255.255.255 | - | **Multicast** (diffusion à groupe) | - |
| **E** | 240.0.0.0 à 255.255.255.255 | - | **Réservé** (expérimental) | - |

**Comment reconnaître la classe ?**

Regarder le **premier octet** :
- Classe A : 1-126
- Classe B : 128-191
- Classe C : 192-223
- Classe D : 224-239
- Classe E : 240-255

**Exemples :**
- `10.0.0.1` → Classe A (premier octet = 10)
- `172.16.0.1` → Classe B (premier octet = 172)
- `192.168.1.1` → Classe C (premier octet = 192)

 **Note :** Aujourd'hui, les classes sont obsolètes. On utilise **CIDR** (Classless Inter-Domain Routing) avec notation `/xx` (ex: `192.168.1.0/24`).

---

### C. Adresses Privées vs Publiques

#### **Adresses Publiques**

**Définition :** Adresses **routables sur Internet**, **uniques au monde**.

**Attribution :** Distribuées par les **RIR** (Regional Internet Registries) via les FAI (Fournisseurs d'Accès Internet).

**Exemple :** `8.8.8.8` (Google DNS), `142.250.185.78` (serveur Google)

**Coût :** Payant (ressource rare depuis épuisement IPv4)

---

#### **Adresses Privées (RFC 1918)**

**Définition :** Adresses **non routables sur Internet**, utilisables dans les réseaux **locaux** (LAN).

**Plages réservées :**

| **Classe** | **Plage Privée** | **Masque** | **Nombre d'adresses** |
|------------|------------------|------------|-----------------------|
| **A** | 10.0.0.0 à 10.255.255.255 | 255.0.0.0 (/8) | ~16 millions |
| **B** | 172.16.0.0 à 172.31.255.255 | 255.240.0.0 (/12) | ~1 million |
| **C** | 192.168.0.0 à 192.168.255.255 | 255.255.0.0 (/16) | 65 536 |

**Avantages :**
- **Avantage :** **Gratuites** (pas besoin de payer un FAI)
- **Avantage :** **Réutilisables** : Chaque entreprise peut utiliser `192.168.1.0/24` dans son LAN
- **Avantage :** **Sécurité** : Pas d'accès direct depuis Internet (protection naturelle)

**Fonctionnement :**
- Les machines en réseau privé utilisent des adresses comme `192.168.1.10`
- Pour accéder à Internet, elles passent par un **routeur NAT** (Network Address Translation)
- Le routeur traduit l'adresse privée en adresse publique (celle de la box)

**Exemple :**
- Votre PC : `192.168.1.50` (privé)
- Votre box : `90.34.12.78` (public, fourni par le FAI)
- Quand vous allez sur Google, votre requête part avec l'IP `90.34.12.78` (celle de la box)
- Google ne voit jamais votre `192.168.1.50`

---

#### **Adresses Spéciales**

| **Adresse** | **Nom** | **Usage** |
|-------------|---------|-----------|
| **0.0.0.0** | Adresse invalide | Toutes les interfaces (bind serveur) |
| **127.0.0.1** | Localhost / Loopback | Boucle locale (machine elle-même) |
| **255.255.255.255** | Broadcast | Diffusion à toutes les machines du réseau |
| **169.254.x.x** | APIPA (Automatic Private IP Addressing) | Auto-assignée si pas de DHCP |

**Exemple localhost :**
- Si vous faites `ping 127.0.0.1`, vous pingez **votre propre machine** (utile pour tester la pile TCP/IP)

---

### D. Masque de Sous-Réseau (Subnet Mask)

**Définition :** Le masque indique quelle partie de l'adresse IP représente le **réseau** et quelle partie représente l'**hôte** (machine).

**Format :** 4 octets comme une adresse IP

**Exemple :** `255.255.255.0`

**En binaire :**
```
255      .  255      .  255      .  0
11111111 .  11111111 .  11111111 .  00000000

Partie réseau (1) ────────────────┘ └── Partie hôte (0)
```

**Règle :** 
- Les bits à **1** dans le masque = partie **réseau**
- Les bits à **0** dans le masque = partie **hôte**

---

**Exemple concret :**

**Adresse IP :** `192.168.1.10`
**Masque :** `255.255.255.0` (ou `/24`)

```
IP :     192.168.1.10    → 11000000.10101000.00000001.00001010
Masque : 255.255.255.0   → 11111111.11111111.11111111.00000000
         ─────────────────   ───────────────────────── ────────
         Partie RÉSEAU                                Partie HÔTE

Adresse du RÉSEAU :  192.168.1.0   (tous les bits hôte à 0)
Adresse BROADCAST : 192.168.1.255  (tous les bits hôte à 1)
Plage utilisable :   192.168.1.1 à 192.168.1.254
```

**Nombre d'hôtes utilisables :** 2^8 - 2 = **254 machines**
- `-2` car on retire l'adresse réseau (`.0`) et l'adresse broadcast (`.255`)

---

**Masques courants :**

| **Masque (décimal)** | **Notation CIDR** | **Bits Réseau** | **Bits Hôte** | **Nb Hôtes** |
|----------------------|-------------------|-----------------|---------------|--------------|
| 255.0.0.0 | /8 | 8 | 24 | ~16 millions |
| 255.255.0.0 | /16 | 16 | 16 | ~65 000 |
| 255.255.255.0 | /24 | 24 | 8 | 254 |
| 255.255.255.128 | /25 | 25 | 7 | 126 |
| 255.255.255.192 | /26 | 26 | 6 | 62 |
| 255.255.255.224 | /27 | 27 | 5 | 30 |
| 255.255.255.252 | /30 | 30 | 2 | 2 (liaison point-à-point) |

---

### E. Notion de Passerelle par Défaut (Gateway)

**Définition :** La **passerelle par défaut** (default gateway) est l'adresse IP du **routeur** permettant de sortir du réseau local pour accéder à d'autres réseaux (Internet).

**Analogie :** Si votre réseau local est un village, la passerelle est le **portail de sortie** vers l'autoroute (Internet).

**Exemple :**

Votre configuration réseau :
- **Adresse IP** : `192.168.1.50`
- **Masque** : `255.255.255.0`
- **Passerelle** : `192.168.1.1` (adresse du routeur/box)

**Fonctionnement :**
1. Vous voulez accéder à `www.google.fr` (IP `142.250.185.78`)
2. Votre PC compare : `142.250.185.78` est-il dans mon réseau `192.168.1.0/24` ?
   - Non ! (premier octet déjà différent : 142 ≠ 192)
3. Le PC envoie le paquet à la **passerelle** `192.168.1.1`
4. Le routeur route le paquet vers Internet

**Sans passerelle configurée :** Impossible d'accéder à Internet (seulement au réseau local).

---

### F. Rôle du DNS (Domain Name System)

**Problème :** Les humains retiennent mal les adresses IP (`142.250.185.78`), mais bien les noms (`www.google.fr`).

**Solution :** Le **DNS** = "Annuaire téléphonique d'Internet" qui traduit les noms en adresses IP.

**Fonctionnement :**

1. Vous tapez `www.google.fr` dans votre navigateur
2. Votre PC envoie une requête DNS au serveur DNS configuré (ex: `8.8.8.8` - Google DNS)
3. Le serveur DNS répond : `www.google.fr` = `142.250.185.78`
4. Votre PC contacte l'IP `142.250.185.78` pour récupérer la page web

**Serveurs DNS courants :**

| **Adresse IP** | **Fournisseur** |
|----------------|-----------------|
| 8.8.8.8 / 8.8.4.4 | Google Public DNS |
| 1.1.1.1 / 1.0.0.1 | Cloudflare DNS |
| 9.9.9.9 | Quad9 DNS (sécurité) |
| 208.67.222.222 / 208.67.220.220 | OpenDNS |

**Configuration :** Généralement fournie automatiquement par **DHCP**, ou configurable manuellement dans les paramètres réseau.

---

## VII. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|----------------|
| **Réseau informatique** | Ensemble d'équipements interconnectés permettant l'échange de données |
| **LAN** | Local Area Network - Réseau local (bâtiment, campus) |
| **WAN** | Wide Area Network - Réseau étendu (pays, monde, Internet) |
| **Switch** | Équipement reliant plusieurs machines dans un réseau local (couche 2) |
| **Routeur** | Équipement reliant plusieurs réseaux entre eux (couche 3) |
| **Protocole** | Ensemble de règles permettant la communication entre équipements |
| **Modèle OSI** | Modèle théorique en 7 couches décrivant les communications réseau |
| **Modèle TCP/IP** | Modèle pratique en 4 couches utilisé sur Internet |
| **Encapsulation** | Processus d'ajout d'en-têtes par chaque couche lors de l'envoi |
| **Désencapsulation** | Processus de retrait des en-têtes à la réception |
| **Trame** | Unité de données de la couche 2 (Liaison) |
| **Paquet** | Unité de données de la couche 3 (Réseau) |
| **Segment** | Unité de données de la couche 4 (Transport - TCP) |
| **Adresse MAC** | Adresse physique unique gravée dans une carte réseau (couche 2) |
| **Adresse IP** | Adresse logique identifiant une machine sur un réseau IP (couche 3) |
| **IPv4** | Protocole IP version 4 (4 octets, 4,3 milliards d'adresses) |
| **Masque de sous-réseau** | Indique la partie réseau et la partie hôte d'une adresse IP |
| **Passerelle par défaut** | Adresse IP du routeur permettant de sortir du réseau local |
| **DNS** | Domain Name System - Service de résolution de noms en adresses IP |
| **DHCP** | Dynamic Host Configuration Protocol - Attribution automatique d'adresses IP |
| **TCP** | Transmission Control Protocol - Protocole orienté connexion, fiable |
| **UDP** | User Datagram Protocol - Protocole non connecté, rapide mais non fiable |
| **Port** | Numéro identifiant une application (0-65535) |
| **ICMP** | Internet Control Message Protocol - Messages d'erreur et ping |
| **Ping** | Commande testant la connectivité réseau (ICMP Echo Request/Reply) |
| **Traceroute / Tracert** | Commande traçant le chemin des paquets vers une destination |
| **Loopback** | Adresse 127.0.0.1 pointant vers la machine locale |
| **Broadcast** | Adresse de diffusion vers toutes les machines du réseau |
| **Unicast** | Communication d'une machine vers une seule autre |
| **Multicast** | Communication d'une machine vers un groupe |
| **NAT** | Network Address Translation - Traduction d'adresses privées en publiques |

---

## VIII. Exercices d'Entraînement

### Exercice 1 : Identification de Classe et Masque

Pour chaque adresse IP, indiquer :
- La classe (A, B, C, D, E)
- Le masque par défaut
- Si c'est une adresse privée ou publique

| **Adresse IP** | **Classe** | **Masque par défaut** | **Privée / Publique** |
|----------------|------------|-----------------------|-----------------------|
| 10.0.0.1 | | | |
| 172.16.5.10 | | | |
| 192.168.100.50 | | | |
| 8.8.8.8 | | | |
| 172.32.1.1 | | | |
| 127.0.0.1 | | | |

---

### Exercice 2 : Calcul de Réseau

Soit l'adresse IP `192.168.10.45` avec le masque `255.255.255.0`.

1. Quelle est l'adresse du réseau ? __________
2. Quelle est l'adresse de broadcast ? __________
3. Combien d'hôtes utilisables dans ce réseau ? __________
4. Quelle est la plage d'adresses utilisables ? __________ à __________
5. Cette machine peut-elle communiquer directement (sans routeur) avec `192.168.10.100` ? Oui / Non
6. Cette machine peut-elle communiquer directement avec `192.168.11.10` ? Oui / Non

---

### Exercice 3 : Diagnostic de Panne

**Scénario :** Un utilisateur vous appelle : *"Je ne peux plus accéder à Internet !"*

Vous exécutez les commandes suivantes :

```
C:\> ping 127.0.0.1
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Fonctionne

C:\> ping 192.168.1.1  (passerelle)
Délai d'attente de la demande dépassé.
Ne fonctionne pas

C:\> ipconfig
Adresse IPv4. . . . . . . . . . . . . .: 192.168.1.50
Masque de sous-réseau. . . . . . . . . : 255.255.255.0
Passerelle par défaut. . . . . . . . . : 192.168.1.1
```

**Questions :**
1. La pile TCP/IP fonctionne-t-elle ? Oui / Non
2. Quel est le problème probable ?
   - ☐ Problème DNS
   - ☐ Problème de connexion locale (câble, switch)
   - ☐ Problème de configuration IP
   - ☐ Problème Internet (FAI)
3. Quelle serait votre prochaine action de diagnostic ?

---

### Exercice 4 : Matching Couches OSI

Associer chaque élément à sa couche OSI :

| **Élément** | **Couche OSI** |
|-------------|----------------|
| Navigateur Chrome | |
| Adresse MAC | |
| Câble RJ45 | |
| Protocole TCP | |
| Adresse IP | |
| Chiffrement SSL | |
| Routeur | |
| Switch | |
| Port 443 (HTTPS) | |

---

## IX. Questions de Réflexion (Pour aller plus loin)

1. **Pourquoi a-t-on besoin de deux adresses (IP et MAC) ?**
   - *Piste : IP = logique (changeable, routage), MAC = physique (gravée, local)*

2. **Que se passe-t-il si deux machines ont la même adresse IP sur un réseau ?**
   - *Réponse : Conflit d'adresses IP → les deux machines ont des problèmes de connectivité*

3. **Pourquoi le modèle OSI a-t-il 7 couches et pas 3 ou 10 ?**
   - *Réponse : Compromis entre modularité (séparation des fonctions) et simplicité*

4. **Pourquoi UDP est-il utilisé pour la visioconférence alors qu'il n'est pas fiable ?**
   - *Réponse : Vitesse prioritaire. Mieux vaut perdre quelques images que tout bloquer en attendant retransmission*

5. **Comment fonctionne le NAT pour permettre à 10 machines avec IP privée de partager 1 IP publique ?**
   - *Piste : Table de correspondance IP:Port interne ↔ IP:Port externe*

6. **Pourquoi y a-t-il épuisement des adresses IPv4 mais pas IPv6 ?**
   - *Réponse : IPv4 = 2^32 (~4 milliards) vs IPv6 = 2^128 (340 sextillions !)*

---

## X. Ressources pour Approfondir

**Sites Web :**
- [Wikipedia - Modèle OSI](https://fr.wikipedia.org/wiki/Mod%C3%A8le_OSI)
- [Wikipedia - Suite des protocoles Internet (TCP/IP)](https://fr.wikipedia.org/wiki/Suite_des_protocoles_Internet)
- [Subnet Calculator](https://www.subnet-calculator.com/) - Calculateur de sous-réseaux en ligne

**Vidéos YouTube :**
- "Le modèle OSI expliqué simplement" - Vulgarisation complète
- "Comprendre les adresses IP et les masques de sous-réseau" - Tutoriel
- "Commandes réseau Windows : ping, ipconfig, tracert" - Démo pratique

**Simulateur :**
- **Packet Tracer** (Cisco) - Gratuit, permet de créer et simuler des réseaux virtuels

**Documentation RFC :**
- RFC 791 : Internet Protocol (IPv4)
- RFC 1918 : Address Allocation for Private Internets
- RFC 793 : Transmission Control Protocol (TCP)

---

## Auto-évaluation : Suis-je Prêt ?

Après avoir terminé cette séance, je suis capable de :

- [ ] Expliquer le rôle d'un réseau informatique et ses avantages
- [ ] Nommer les 7 couches du modèle OSI dans l'ordre
- [ ] Expliquer la différence entre modèle OSI et TCP/IP
- [ ] Différencier LAN, MAN, WAN, PAN
- [ ] Lire une adresse IPv4 et son masque de sous-réseau
- [ ] Identifier la classe d'une adresse IP (A, B, C)
- [ ] Différencier adresse privée et adresse publique
- [ ] Calculer l'adresse réseau et broadcast à partir d'une IP et d'un masque
- [ ] Utiliser la commande `ipconfig` pour voir ma configuration IP
- [ ] Utiliser `ping` pour tester une connectivité
- [ ] Utiliser `tracert` pour tracer le chemin vers une destination
- [ ] Expliquer le rôle du DNS et de la passerelle par défaut
- [ ] Diagnostiquer une panne réseau simple avec les commandes de base
