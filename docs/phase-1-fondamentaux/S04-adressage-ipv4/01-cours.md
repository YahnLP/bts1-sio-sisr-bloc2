# Cours – Adressage IPv4, ARP et ICMP

*BTS SIO SISR 1re année – Bloc 2 – S04 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S03 (modèle OSI, adresse IP et masque) ; conversions binaire/décimal (mathématiques).
> - **Matériel :** VM Windows, VM Debian, Wireshark.

## À la fin de ce cours, tu sauras

- [ ] **Utiliser** la notation CIDR et **calculer** adresse réseau, plage d'hôtes et diffusion ;
- [ ] **Découper** un réseau en sous-réseaux de tailles différentes (VLSM) ;
- [ ] **Expliquer** ARP (adresse MAC ↔ adresse IP) et ICMP (ping, traceroute) ;
- [ ] **Configurer** une adresse IP sous Windows et sous Linux (`ip addr`, `ip route`) ;
- [ ] **Installer** Debian en machine virtuelle et se déplacer en ligne de commande.

---

## I. Adressage IPv4 – Approfondissement

### A. Rappel : Classes d'Adresses IPv4

Le premier octet d'une adresse IPv4 détermine sa **classe**.

| **Classe** | **Premier octet** | **Plage complète** | **Masque par défaut** | **Usage historique** |
|------------|-------------------|--------------------|----------------------|----------------------|
| **A** | 1 – 126 | 1.0.0.0 à 126.255.255.255 | /8 (255.0.0.0) | Très grands réseaux |
| **B** | 128 – 191 | 128.0.0.0 à 191.255.255.255 | /16 (255.255.0.0) | Grandes entreprises |
| **C** | 192 – 223 | 192.0.0.0 à 223.255.255.255 | /24 (255.255.255.0) | PME, petits réseaux |
| **D** | 224 – 239 | 224.0.0.0 à 239.255.255.255 | - | Multicast |
| **E** | 240 – 255 | 240.0.0.0 à 255.255.255.255 | - | Réservé expérimental |

**À retenir :** Classe A = 1–126, Classe B = 128–191, Classe C = 192–223

> [!NOTE]
> Les classes A, B et C servent à **adresser des machines** (une adresse = un équipement). Les classes D et E ont un rôle à part, expliqué ci-dessous. Précision : le réseau `127.0.0.0/8` (dont `127.0.0.1`, la boucle locale) est réservé aux tests de la machine elle-même ; c'est pourquoi la classe A s'arrête à 126.

#### Classe D : parler à un groupe (le multicast)

Il y a trois façons d'envoyer un message sur un réseau :

| **Mode** | **À qui ?** | **Image** | **Exemple** |
|----------|-------------|-----------|-------------|
| **Unicast** | Un seul destinataire | Un appel téléphonique | Votre PC demande une page à un serveur web |
| **Broadcast** | Tout le monde sur le réseau local | Une annonce au haut-parleur | Un PC qui cherche un serveur DHCP |
| **Multicast** | Un **groupe** d'abonnés | Une chaîne de radio ou de télévision | Un flux vidéo suivi par 20 postes |

Une adresse de classe D (de `224.0.0.0` à `239.255.255.255`) **n'identifie pas une machine** : elle désigne un **groupe**. Les équipements qui veulent recevoir le message **s'abonnent** à ce groupe ; les autres l'ignorent. L'émetteur n'envoie le flux **qu'une seule fois**, même s'il y a 50 abonnés (en unicast, il l'enverrait 50 fois).

**À quoi ça sert concrètement :**

| **Adresse** | **Utilisation** |
|-------------|-----------------|
| `224.0.0.1` | Tous les équipements du réseau local |
| `224.0.0.2` | Tous les routeurs du réseau local |
| `224.0.0.5` et `224.0.0.6` | Les routeurs qui échangent leurs routes avec **OSPF** |
| `224.0.0.9` | Les routeurs qui échangent leurs routes avec **RIP v2** |
| `224.0.0.251` | Découverte de services sur le réseau local (imprimante, Chromecast, mDNS) |
| `239.0.0.0` à `239.255.255.255` | Plage **à usage interne** d'une organisation (comme les adresses privées) : télévision sur IP, visioconférence, déploiement d'images système vers plusieurs postes en même temps |

**À retenir :** une adresse de classe D est **toujours une adresse de destination**, jamais l'adresse d'une carte réseau ni l'adresse source d'un paquet. Elle n'a donc ni masque ni notion de « réseau de machines ».

#### Classe E : une réserve jamais utilisée

La classe E (de `240.0.0.0` à `255.255.255.255`) a été **mise de côté pour l'expérimentation** et les besoins futurs lors de la création d'IPv4. En pratique, elle n'a **jamais été attribuée** :
- elle ne s'utilise pas pour configurer un poste ni un serveur ;
- les routeurs d'Internet ne la transmettent pas ;
- une seule adresse de cette plage a un usage courant : `255.255.255.255`, l'adresse de **broadcast limité** (« à tout le réseau local »), que l'on retrouve dans le DHCP Discover.

Quand on a manqué d'adresses IPv4, des propositions ont été faites pour récupérer cette plage, mais de nombreux systèmes la refusent. La solution retenue est **IPv6** (S14).

> [!TIP]
> Ces « classes » sont l'ancienne façon de découper l'espace d'adresses. Depuis les années 1990, on raisonne en **notation CIDR** (`/24`, `/16`...) : la classe permet surtout de comprendre l'histoire et les masques « par défaut », alors que D et E, elles, existent toujours dans les équipements.

---

### B. Adresses Privées (RFC 1918)

Les adresses **privées** sont réservées aux réseaux locaux (LAN). Elles ne sont **pas routables sur Internet**.

| **Classe** | **Plage Privée** | **Notation CIDR** | **Usage typique** |
|------------|------------------|-------------------|-------------------|
| **A** | 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 | Grandes infras (datacenter, réseau d'entreprise) |
| **B** | 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 | Réseaux moyens |
| **C** | 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | Domicile, PME |

**Adresses spéciales importantes :**

| **Adresse** | **Nom** | **Usage** | **À retenir** |
|-------------|---------|-----------|---------------|
| `127.0.0.1` | **Loopback** | Tester la pile TCP/IP locale | "Ping de soi-même" |
| `169.254.x.x` | **APIPA** | Attribution automatique si pas de DHCP |  Signe de problème réseau |
| `0.0.0.0` | Non défini | Absence d'adresse (ex: route par défaut) | - |
| `255.255.255.255` | Broadcast limité | Diffusion locale | Requêtes DHCP initiales |

---

### C. Notation CIDR (Classless Inter-Domain Routing)

La notation **CIDR** (prononcé "sider") remplace l'écriture longue du masque par un simple **suffixe `/xx`** indiquant le nombre de bits à 1 dans le masque.

**Exemples :**
- `192.168.1.0 /24` = réseau `192.168.1.0` avec masque `255.255.255.0`
- `10.0.0.0 /8` = réseau `10.0.0.0` avec masque `255.0.0.0`

**Tableau des CIDR courants :**

| **CIDR** | **Masque décimal** | **Bits hôte** | **Hôtes utilisables** | **Usage** |
|----------|--------------------|---------------|-----------------------|-----------|
| **/8** | 255.0.0.0 | 24 | 16 777 214 | Très grand réseau (classe A) |
| **/16** | 255.255.0.0 | 16 | 65 534 | Grand réseau (classe B) |
| **/24** | 255.255.255.0 | 8 | **254** | Réseau PME standard (le plus courant) |
| **/25** | 255.255.255.128 | 7 | 126 | Division d'un /24 en 2 |
| **/26** | 255.255.255.192 | 6 | 62 | Division en 4 |
| **/27** | 255.255.255.224 | 5 | 30 | Division en 8 |
| **/28** | 255.255.255.240 | 4 | 14 | Petit réseau |
| **/30** | 255.255.255.252 | 2 | **2** | Liaison point-à-point (routeurs) |
| **/32** | 255.255.255.255 | 0 | 1 | Hôte unique (loopback, routes statiques) |

**Formule :** Nombre d'hôtes utilisables = 2<sup>bits hôte</sup> − 2

**Exemple de calcul complet :**

Soit le réseau `192.168.10.0/26` :

| **Étape** | **Calcul** | **Résultat** |
|---|---|---|
| Masque | /26 | 255.255.255.192 |
| Bits hôte | 32 − 26 | 6 bits |
| Nombre d'adresses | 2^6^ | 64 |
| Hôtes utilisables | 64 − 2 | 62 |
| Adresse réseau | | 192.168.10.0 |
| Première adresse hôte | | 192.168.10.1 |
| Dernière adresse hôte | | 192.168.10.62 |
| Adresse de broadcast | | 192.168.10.63 |

---

## II. Le Protocole ARP

### A. Pourquoi ARP Existe-t-il ?

**Problème :** Quand un PC veut envoyer un paquet IP à `192.168.1.20`, il a besoin de l'**adresse MAC** de destination pour créer la trame Ethernet (couche 2).

Mais il ne connaît que l'**adresse IP** de la machine.

**Solution :** Le protocole **ARP** (Address Resolution Protocol) résout ce problème.

**ARP = Traduction IP → MAC**

---

### B. Fonctionnement d'ARP

**Étape par étape :**

<figure class="graphe diagramme"><svg viewBox="0 0 760 316" style="max-width:760px" role="img" aria-label="Résolution ARP : PC1 cherche l&#x27;adresse MAC de PC2" xmlns="http://www.w3.org/2000/svg">
<rect x="20.0" y="8" width="260" height="34" rx="8" class="dg-dark"/>
<text x="150.0" y="30.0" text-anchor="middle" class="dg-w">PC1 (192.168.1.10 - AA:BB:11)</text>
<line x1="150.0" y1="42" x2="150.0" y2="308" class="dg-dash"/>
<rect x="480.0" y="8" width="260" height="34" rx="8" class="dg-dark"/>
<text x="610.0" y="30.0" text-anchor="middle" class="dg-w">Réseau local et PC2 (192.168.1.20 - CC:DD:22)</text>
<line x1="610.0" y1="42" x2="610.0" y2="308" class="dg-dash"/>
<rect x="4.0" y="56" width="404.2" height="26" rx="6" class="dg-soft"/>
<text x="206.1" y="73" text-anchor="middle" class="dg-s">1. Consulte sa table ARP : pas d&#x27;entrée pour 192.168.1.20</text>
<text x="380.0" y="102" text-anchor="middle" class="dg-t">2. ARP Request (broadcast FF:FF:FF:FF:FF:FF)</text>
<line x1="150.0" y1="110" x2="601.0" y2="110.0" class="dg-line"/>
<polygon points="610.0,110 601.0,105.0 601.0,115.0" class="dg-ah"/>
<text x="380.0" y="126" text-anchor="middle" class="dg-i">« Qui a l&#x27;IP 192.168.1.20 ? Répondez à AA:BB:11 »</text>
<rect x="447.5" y="144" width="325.0" height="42" rx="6" class="dg-soft"/>
<text x="610.0" y="161" text-anchor="middle" class="dg-s">3. Tous les PC du réseau reçoivent la requête</text>
<text x="610.0" y="177" text-anchor="middle" class="dg-s">Seul PC2 reconnaît son adresse IP</text>
<text x="380.0" y="206" text-anchor="middle" class="dg-t">4. ARP Reply (unicast vers AA:BB:11)</text>
<line x1="610.0" y1="214" x2="159.0" y2="214.0" class="dg-dash"/>
<polygon points="150.0,214 159.0,219.0 159.0,209.0" class="dg-ah"/>
<text x="380.0" y="230" text-anchor="middle" class="dg-i">« 192.168.1.20 a pour MAC CC:DD:22 »</text>
<rect x="4.0" y="248" width="377.8" height="42" rx="6" class="dg-soft"/>
<text x="192.89999999999998" y="265" text-anchor="middle" class="dg-s">5. Met à jour sa table ARP : 192.168.1.20 -&gt; CC:DD:22</text>
<text x="192.89999999999998" y="281" text-anchor="middle" class="dg-s">6. Construit la trame Ethernet et envoie le paquet IP</text>
</svg>
<figcaption>PC1 ne connaît que l&#x27;adresse IP de PC2 : ARP lui fournit l&#x27;adresse MAC.</figcaption>
</figure>

**Schéma ARP :**

<figure class="graphe diagramme"><svg viewBox="0 0 780 350" style="max-width:780px" role="img" aria-label="Résolution ARP : requête diffusée, réponse unicast de PC2" xmlns="http://www.w3.org/2000/svg">
<rect x="40.0" y="8" width="100" height="34" rx="8" class="dg-dark"/>
<text x="90.0" y="30.0" text-anchor="middle" class="dg-w">PC1</text>
<line x1="90.0" y1="42" x2="90.0" y2="342" class="dg-dash"/>
<rect x="190.0" y="8" width="100" height="34" rx="8" class="dg-dark"/>
<text x="240.0" y="30.0" text-anchor="middle" class="dg-w">Switch</text>
<line x1="240.0" y1="42" x2="240.0" y2="342" class="dg-dash"/>
<rect x="340.0" y="8" width="100" height="34" rx="8" class="dg-dark"/>
<text x="390.0" y="30.0" text-anchor="middle" class="dg-w">PC2</text>
<line x1="390.0" y1="42" x2="390.0" y2="342" class="dg-dash"/>
<rect x="490.0" y="8" width="100" height="34" rx="8" class="dg-dark"/>
<text x="540.0" y="30.0" text-anchor="middle" class="dg-w">PC3</text>
<line x1="540.0" y1="42" x2="540.0" y2="342" class="dg-dash"/>
<rect x="640.0" y="8" width="100" height="34" rx="8" class="dg-dark"/>
<text x="690.0" y="30.0" text-anchor="middle" class="dg-w">PC4</text>
<line x1="690.0" y1="42" x2="690.0" y2="342" class="dg-dash"/>
<text x="165.0" y="64" text-anchor="middle" class="dg-t">ARP Request (broadcast)</text>
<line x1="90.0" y1="72" x2="231.0" y2="72.0" class="dg-line"/>
<polygon points="240.0,72 231.0,67.0 231.0,77.0" class="dg-ah"/>
<text x="315.0" y="98" text-anchor="middle" class="dg-t">ARP Request</text>
<line x1="240.0" y1="106" x2="381.0" y2="106.0" class="dg-line"/>
<polygon points="390.0,106 381.0,101.0 381.0,111.0" class="dg-ah"/>
<text x="390.0" y="132" text-anchor="middle" class="dg-t">ARP Request</text>
<line x1="240.0" y1="140" x2="531.0" y2="140.0" class="dg-line"/>
<polygon points="540.0,140 531.0,135.0 531.0,145.0" class="dg-ah"/>
<text x="465.0" y="166" text-anchor="middle" class="dg-t">ARP Request</text>
<line x1="240.0" y1="174" x2="681.0" y2="174.0" class="dg-line"/>
<polygon points="690.0,174 681.0,169.0 681.0,179.0" class="dg-ah"/>
<text x="315.0" y="200" text-anchor="middle" class="dg-t">ARP Reply (unicast)</text>
<line x1="390.0" y1="208" x2="249.0" y2="208.0" class="dg-dash"/>
<polygon points="240.0,208 249.0,213.0 249.0,203.0" class="dg-ah"/>
<text x="165.0" y="234" text-anchor="middle" class="dg-t">ARP Reply</text>
<line x1="240.0" y1="242" x2="99.0" y2="242.0" class="dg-dash"/>
<polygon points="90.0,242 99.0,247.0 99.0,237.0" class="dg-ah"/>
<rect x="427.0" y="260" width="226.0" height="26" rx="6" class="dg-soft"/>
<text x="540.0" y="277" text-anchor="middle" class="dg-s">PC3 et PC4 ignorent la requête</text>
<rect x="4.0" y="298" width="331.6" height="26" rx="6" class="dg-soft"/>
<text x="169.79999999999998" y="315" text-anchor="middle" class="dg-s">Table ARP de PC1 : 192.168.1.20 est à CC:DD:22</text>
</svg>
</figure>

---

### C. La Table ARP

La table ARP est une **mémoire cache temporaire** qui stocke les correspondances IP ↔ MAC récemment résolues.

**Afficher la table ARP (Windows) :**
```cmd
arp -a
```

**Résultat typique :**
```
Interface : 192.168.1.10 --- 0xc
  Adresse Internet      Adresse physique      Type
  192.168.1.1           00-1a-2b-3c-4d-5e     dynamique   ← Passerelle (routeur/box)
  192.168.1.20          00-cc-dd-22-33-44     dynamique   ← PC2 (récemment contacté)
  192.168.1.255         ff-ff-ff-ff-ff-ff     statique    ← Broadcast
```

**Durée de vie :** Les entrées ARP expirent automatiquement (Windows : ~2 minutes, Linux : ~60 secondes).

**ARP et les réseaux distants :**

**Attention :** Quand PC1 veut joindre un PC sur un **autre réseau** (ex: Internet), il n'envoie pas d'ARP vers la machine distante !

**Pourquoi ?** ARP est limité au réseau local (broadcast).

**Ce qui se passe :**
1. PC1 détecte que `8.8.8.8` n'est pas sur son réseau local
2. PC1 envoie un ARP vers sa **passerelle par défaut** (ex: 192.168.1.1)
3. La passerelle (routeur) répond avec sa MAC
4. PC1 envoie la trame à la MAC de la **passerelle** qui se charge du routage

---

### D. Cas Particuliers ARP

**Gratuitous ARP :**
- Un PC envoie un ARP en annonçant **sa propre IP** (sans question)
- Usage : Mise à jour des tables ARP voisines lors d'un changement IP ou redémarrage
- Détection de conflits IP (si quelqu'un répond à son propre ARP, il y a un doublon)

**ARP Spoofing (Usurpation ARP) :**
- **Attention :** Attaque : Un attaquant envoie de fausses réponses ARP pour associer sa MAC à l'IP de la passerelle
- Résultat : Les paquets sont envoyés à l'attaquant (man-in-the-middle)
- Défense : `arp -s` (ARP statique), détection par logiciel (ARPwatch)

---

## III. Le Protocole ICMP

### A. Définition et Rôle

**ICMP** (Internet Control Message Protocol) est un **protocole de signalisation** fonctionnant au niveau de la couche 3 (Réseau).

**Il ne transporte pas de données applicatives**, mais **des messages d'erreur et de diagnostic**.

**Analogie :** ICMP est comme le **service réclamations de la Poste** : si votre colis (paquet IP) ne peut pas être livré, la Poste vous envoie un avis de problème (message ICMP) expliquant pourquoi.

---

### B. Messages ICMP Courants

| **Type** | **Code** | **Message** | **Signification** |
|----------|----------|-------------|-------------------|
| 0 | 0 | **Echo Reply** | Réponse à un ping |
| 3 | 0 | **Destination Unreachable (Network)** | Réseau inaccessible |
| 3 | 1 | **Destination Unreachable (Host)** | Hôte inaccessible |
| 3 | 3 | **Destination Unreachable (Port)** | Port inaccessible (UDP) |
| 8 | 0 | **Echo Request** | Requête ping |
| 11 | 0 | **Time Exceeded (TTL)** | TTL arrivé à 0 (traceroute) |

---

### C. La Commande `ping`

**Ping** utilise ICMP Echo Request (Type 8) et Echo Reply (Type 0) pour **tester la connectivité**.

**Syntaxe :**
```cmd
ping <ip_ou_nom>              (Windows : 4 paquets par défaut)
ping -c 4 <ip_ou_nom>         (Linux : -c = count, obligatoire sinon infini)
```

**Exemple Windows :**
```
C:\> ping 192.168.1.1

Envoi d'une requête 'Ping' sur 192.168.1.1 avec 32 octets de données :
Réponse de 192.168.1.1 : octets=32 temps=2 ms TTL=64
Réponse de 192.168.1.1 : octets=32 temps=1 ms TTL=64
Réponse de 192.168.1.1 : octets=32 temps=2 ms TTL=64
Réponse de 192.168.1.1 : octets=32 temps=1 ms TTL=64

Statistiques Ping pour 192.168.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 2ms, Moyenne = 1ms
```

**Interprétation :**

| **Résultat** | **Signification** | **Action** |
|--------------|-------------------|------------|
| Réponse reçue, temps < 1ms | Machine locale ou même LAN | Connexion OK |
| Réponse reçue, temps 1-50ms | Réseau local ou proche | Connexion OK |
| Réponse reçue, temps > 200ms | Connexion lente/saturée | Latence élevée |
| "Délai d'attente dépassé" | Pas de réponse (pare-feu ou machine HS) | Problème à diagnostiquer |
| "Hôte de destination inaccessible" | Pas de route vers la machine | Problème routage/passerelle |
| TTL = 64 | Linux ou routeur Cisco (TTL initial = 64) | Info système |
| TTL = 128 | Windows (TTL initial = 128) | Info système |
| TTL = 255 | Cisco IOS ou certains Unix | Info système |

---

### D. Le TTL (Time To Live)

**TTL** = compteur décrémenté de 1 à chaque routeur traversé. Quand TTL = 0, le paquet est abandonné et un message ICMP "Time Exceeded" est renvoyé à l'expéditeur.

**Pourquoi TTL existe-t-il ?**
Éviter que des paquets "tournent en boucle" indéfiniment sur Internet en cas de problème de routage.

**Utilisation pratique :**
- TTL dans un ping peut indiquer le nombre de **sauts traversés** :
  - TTL reçu = 117 → TTL initial 128 (Windows) → 128 - 117 = **11 routeurs traversés**
  - TTL reçu = 53 → TTL initial 64 (Linux) → 64 - 53 = **11 routeurs traversés**

---

### E. La Commande `tracert` / `traceroute`

**Tracert/Traceroute** exploite les messages ICMP **Time Exceeded** pour découvrir le **chemin** emprunté par les paquets.

**Principe :**
1. Envoie un paquet avec TTL=1 → Premier routeur le rejette et renvoie "Time Exceeded" → on note son IP
2. Envoie TTL=2 → Deuxième routeur → on note son IP
3. Continue jusqu'à atteindre la destination

**Syntaxe :**
```cmd
tracert 8.8.8.8                 (Windows)
traceroute 8.8.8.8              (Linux)
```

**Exemple de résultat :**
```
C:\> tracert www.google.fr

Détermination de l'itinéraire vers www.google.fr [142.250.185.78]
avec un maximum de 30 sauts :

  1     2 ms     1 ms     1 ms  192.168.1.1        ← Votre box (passerelle)
  2    11 ms     9 ms    10 ms  10.100.200.1       ← 1er routeur FAI
  3    12 ms    11 ms    12 ms  80.10.250.5        ← Réseau FAI
  4     *        *        *     Délai d'attente    ← Routeur ne répond pas ICMP
  5    14 ms    13 ms    15 ms  72.14.202.76       ← Réseau Google
  6    15 ms    14 ms    16 ms  142.250.185.78     ← Destination Google
```

**Interprétation :**
- **`* * *`** : Le routeur ne répond pas aux ICMP (normal, certains bloquent pour sécurité)
- Temps **qui augmente brutalement** : Goulot d'étranglement ou liaison lente
- Dernier saut atteint = destination

**Différence Windows vs Linux :**
- `tracert` (Windows) : Utilise ICMP
- `traceroute` (Linux) : Utilise **UDP** par défaut (ports élevés), plus difficile à bloquer par les pare-feux
