# Cours – Service DNS et Spanning Tree Protocol

*BTS SIO SISR 1re année – Bloc 2 – S08 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S04 à S07.
> - **Matériel :** VM Debian ; simulateur réseau ou Packet Tracer, ou matériel réel (au choix de l'enseignant).

## À la fin de ce cours, tu sauras

- [ ] **Expliquer** la résolution de noms (récursive, itérative) ;
- [ ] **Distinguer** zone directe et zone inverse, **lire** et **créer** des enregistrements (A, PTR, CNAME, NS, SOA) ;
- [ ] **Installer** et **configurer** Bind9 ; **tester** avec `dig`, `nslookup`, `host` ;
- [ ] **Expliquer** le danger des boucles de commutation et le rôle de STP ;
- [ ] **Décrire** l'élection du root bridge et **observer** STP sur trois switchs.

---

## FICHE DE COURS ÉLÈVE
### "Service DNS, Bind9 et Spanning Tree Protocol"

*Version 1.0 – BTS SIO SISR – *

---

### I. Le Service DNS

#### A. Pourquoi le DNS ?

Les machines communiquent par **adresses IP** (192.168.10.50). Les humains retiennent des **noms** (www.techpro.local, google.fr).

**Le DNS** (Domain Name System) fait le lien : il traduit les noms de domaine en adresses IP, et inversement.

**Sans DNS :** Vous devriez mémoriser l'IP de chaque site web. `http://142.250.185.78` à la place de `http://www.google.fr`.

**Avec DNS :** Vous tapez un nom, le système trouve l'IP pour vous, de façon transparente.

---

#### B. Vocabulaire Essentiel

| **Terme** | **Définition** | **Exemple** |
|-----------|----------------|-------------|
| **Domaine** | Espace de noms hiérarchique | `techpro.local`, `google.fr` |
| **FQDN** | Fully Qualified Domain Name – nom complet avec le point final | `www.techpro.local.` |
| **Zone** | Portion de l'espace de noms gérée par un serveur DNS | Zone `techpro.local` gérée par `ns1.techpro.local` |
| **Zone directe** | Traduit noms → adresses IP | `www` → `192.168.10.50` |
| **Zone inverse** | Traduit adresses IP → noms | `192.168.10.50` → `www.techpro.local.` |
| **Résolveur** | Client DNS qui interroge les serveurs | Pile réseau du PC, box Internet |
| **Serveur autoritaire** | Possède les fichiers de zone et fait référence | Le serveur Bind9 qu'on configure |
| **Cache DNS** | Mémorisation temporaire des réponses (durée = TTL) | Cache de la box, du PC |
| **TTL** | Time To Live – durée de validité d'un enregistrement en cache | `3600` = 1 heure |
| **Forwarder** | Serveur DNS vers lequel on renvoie les requêtes non résolues localement | `8.8.8.8` (Google DNS) |

---

#### C. Les Types d'Enregistrements DNS

Chaque ligne dans un fichier de zone est un **enregistrement DNS (Resource Record)**.

| **Type** | **Nom** | **Rôle** | **Exemple** |
|----------|---------|----------|-------------|
| **A** | Address | Nom → IPv4 | `www IN A 192.168.10.50` |
| **AAAA** | Address IPv6 | Nom → IPv6 | `www IN AAAA 2001:db8::1` |
| **PTR** | Pointer | IPv4 → Nom (zone inverse) | `50 IN PTR www.techpro.local.` |
| **CNAME** | Canonical Name | Alias pointant vers un autre nom | `web IN CNAME www` |
| **NS** | Name Server | Serveur de noms autoritaire pour la zone | `@ IN NS ns1.techpro.local.` |
| **MX** | Mail Exchanger | Serveur de messagerie | `@ IN MX 10 mail.techpro.local.` |
| **SOA** | Start of Authority | Informations d'autorité de la zone | (voir ci-dessous) |
| **TXT** | Text | Enregistrement texte libre (SPF, DKIM...) | `@ IN TXT "v=spf1 mx -all"` |

---

#### D. Le Processus de Résolution DNS

Il existe deux types de résolution :

##### Résolution Récursive (vue du client)

Le client pose **une seule question** au résolveur et attend **une seule réponse complète**. Le résolveur fait tout le travail.

<figure class="graphe diagramme"><svg viewBox="0 0 760 342" style="max-width:760px" role="img" aria-label="Résolution DNS : le résolveur interroge la racine puis les serveurs autoritaires" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="130" height="34" rx="8" class="dg-dark"/>
<text x="135.0" y="30.0" text-anchor="middle" class="dg-w">Client</text>
<line x1="135.0" y1="42" x2="135.0" y2="334" class="dg-dash"/>
<rect x="233.33333333333337" y="8" width="130" height="34" rx="8" class="dg-dark"/>
<text x="298.33333333333337" y="30.0" text-anchor="middle" class="dg-w">Résolveur DNS</text>
<line x1="298.33333333333337" y1="42" x2="298.33333333333337" y2="334" class="dg-dash"/>
<rect x="396.6666666666667" y="8" width="130" height="34" rx="8" class="dg-dark"/>
<text x="461.6666666666667" y="30.0" text-anchor="middle" class="dg-w">Serveur racine</text>
<line x1="461.6666666666667" y1="42" x2="461.6666666666667" y2="334" class="dg-dash"/>
<rect x="560.0" y="8" width="130" height="34" rx="8" class="dg-dark"/>
<text x="625.0" y="30.0" text-anchor="middle" class="dg-w">Serveur autoritaire</text>
<line x1="625.0" y1="42" x2="625.0" y2="334" class="dg-dash"/>
<text x="216.66666666666669" y="64" text-anchor="middle" class="dg-t">www.google.fr ?</text>
<line x1="135.0" y1="72" x2="289.3" y2="72.0" class="dg-line"/>
<polygon points="298.33333333333337,72 289.3,67.0 289.3,77.0" class="dg-ah"/>
<text x="380.0" y="98" text-anchor="middle" class="dg-t">Qui gère .fr ?</text>
<line x1="298.33333333333337" y1="106" x2="452.7" y2="106.0" class="dg-line"/>
<polygon points="461.6666666666667,106 452.7,101.0 452.7,111.0" class="dg-ah"/>
<text x="380.0" y="132" text-anchor="middle" class="dg-t">ns1.nic.fr</text>
<line x1="461.6666666666667" y1="140" x2="307.3" y2="140.0" class="dg-dash"/>
<polygon points="298.33333333333337,140 307.3,145.0 307.3,135.0" class="dg-ah"/>
<text x="461.6666666666667" y="166" text-anchor="middle" class="dg-t">Qui gère google.fr ?</text>
<line x1="298.33333333333337" y1="174" x2="616.0" y2="174.0" class="dg-line"/>
<polygon points="625.0,174 616.0,169.0 616.0,179.0" class="dg-ah"/>
<text x="461.6666666666667" y="200" text-anchor="middle" class="dg-t">ns1.google.com</text>
<line x1="625.0" y1="208" x2="307.3" y2="208.0" class="dg-dash"/>
<polygon points="298.33333333333337,208 307.3,213.0 307.3,203.0" class="dg-ah"/>
<text x="461.6666666666667" y="234" text-anchor="middle" class="dg-t">IP de www.google.fr ?</text>
<line x1="298.33333333333337" y1="242" x2="616.0" y2="242.0" class="dg-line"/>
<polygon points="625.0,242 616.0,237.0 616.0,247.0" class="dg-ah"/>
<text x="461.6666666666667" y="268" text-anchor="middle" class="dg-t">142.250.185.78</text>
<line x1="625.0" y1="276" x2="307.3" y2="276.0" class="dg-dash"/>
<polygon points="298.33333333333337,276 307.3,281.0 307.3,271.0" class="dg-ah"/>
<text x="216.66666666666669" y="302" text-anchor="middle" class="dg-t">142.250.185.78</text>
<line x1="298.33333333333337" y1="310" x2="144.0" y2="310.0" class="dg-dash"/>
<polygon points="135.0,310 144.0,315.0 144.0,305.0" class="dg-ah"/>
</svg>
</figure>

##### Résolution Itérative (vue du résolveur)

Le serveur renvoie la **meilleure réponse** qu'il peut donner (pas une réponse complète). Le résolveur pose ensuite la question au serveur suivant. C'est la mécanique interne que le résolveur utilise.

---

#### E. Zones Directe et Inverse

##### Zone Directe (forward zone)

Traduit des **noms → adresses IP** via des enregistrements **A**.

**Fichier de zone directe `db.techpro.local` (exemple) :**
```
$TTL 3600                        ; TTL par défaut : 1 heure
@ IN SOA ns1.techpro.local. admin.techpro.local. (
                2024112001       ; Serial : YYYYMMDDNN
                3600             ; Refresh : 1h
                900              ; Retry : 15min
                604800           ; Expire : 1 semaine
                3600 )           ; Negative cache TTL : 1h

; Serveur(s) de noms
@ IN NS ns1.techpro.local.

; Enregistrements A (nom → IP)
ns1      IN A 192.168.10.1       ; Serveur DNS lui-même
www      IN A 192.168.10.50      ; Serveur web
mail     IN A 192.168.10.51      ; Serveur mail
srv-nas  IN A 192.168.10.10      ; NAS

; Alias CNAME
web      IN CNAME www            ; "web" pointe vers "www"
intranet IN CNAME www
```

**Lecture :**
- `@` = origine de la zone (= `techpro.local.`)
- `IN` = classe Internet (toujours IN)
- Un nom sans point final est **relatif** à la zone (`www` = `www.techpro.local.`)
- Un nom **avec point final** est **absolu** (FQDN) : `ns1.techpro.local.`

---

##### Zone Inverse (reverse zone)

Traduit des **adresses IP → noms** via des enregistrements **PTR**.

Le nom de la zone inverse pour `192.168.10.0/24` est : `10.168.192.in-addr.arpa`
*(les octets du réseau sont inversés, + `.in-addr.arpa`)*

**Fichier de zone inverse `db.192.168.10.rev` (exemple) :**
```
$TTL 3600
@ IN SOA ns1.techpro.local. admin.techpro.local. (
                2024112001
                3600
                900
                604800
                3600 )

@ IN NS ns1.techpro.local.

; Enregistrements PTR (dernier octet de l'IP → nom complet)
1  IN PTR ns1.techpro.local.
10 IN PTR srv-nas.techpro.local.
50 IN PTR www.techpro.local.
51 IN PTR mail.techpro.local.
```

**Lecture :**
- `1 IN PTR ns1.techpro.local.` → l'IP `192.168.10.1` (zone `10.168.192.in-addr.arpa`) correspond à `ns1.techpro.local.`
- On n'écrit que le **dernier octet** (le reste est dans le nom de la zone)
- Le nom cible d'un PTR doit **toujours** avoir un point final

---

#### F. L'Enregistrement SOA – Start of Authority

L'enregistrement SOA est le premier de chaque zone. Il contient les métadonnées de la zone.

```
@ IN SOA ns1.techpro.local. admin.techpro.local. (
          2024112001   ; Serial
          3600         ; Refresh
          900          ; Retry
          604800       ; Expire
          3600 )       ; Negative TTL
```

| **Champ** | **Rôle** |
|-----------|----------|
| `ns1.techpro.local.` | Serveur DNS primaire de la zone |
| `admin.techpro.local.` | Email de l'administrateur (le `.` remplace le `@`) |
| **Serial** | Numéro de version (convention : AAAAMMJJNN). Doit être incrémenté à chaque modif pour que les secondaires rechargent la zone |
| **Refresh** | Délai entre deux vérifications du secondaire (en secondes) |
| **Retry** | Délai avant nouvelle tentative si le secondaire ne peut pas contacter le primaire |
| **Expire** | Durée après laquelle le secondaire considère sa copie comme obsolète |
| **Negative TTL** | Durée de mise en cache d'une réponse NXDOMAIN (nom inexistant) |

---

### II. Spanning Tree Protocol (STP)

#### A. Le Problème des Boucles Réseau

En commuté (switches), on peut être tenté de relier plusieurs switches par plusieurs câbles pour la **redondance** (si un lien tombe, l'autre prend le relais). Mais cela crée des **boucles physiques**.

**Problème 1 : Tempête de broadcast (Broadcast Storm)**

<figure class="graphe diagramme"><svg viewBox="0 0 560 200" style="max-width:560px" role="img" aria-label="Boucle de commutation : un broadcast tourne indéfiniment entre trois switchs" xmlns="http://www.w3.org/2000/svg">
<rect x="150" y="40" width="70" height="34" rx="8" class="dg-dark"/>
<text x="185.0" y="62.0" text-anchor="middle" class="dg-w">SW1</text>
<rect x="320" y="40" width="70" height="34" rx="8" class="dg-dark"/>
<text x="355.0" y="62.0" text-anchor="middle" class="dg-w">SW2</text>
<rect x="235" y="130" width="70" height="34" rx="8" class="dg-dark"/>
<text x="270.0" y="152.0" text-anchor="middle" class="dg-w">SW3</text>
<line x1="220" y1="57" x2="320" y2="57" class="dg-line"/>
<line x1="185" y1="74" x2="245" y2="130" class="dg-line"/>
<line x1="355" y1="74" x2="295" y2="130" class="dg-line"/>
<text x="280" y="22" text-anchor="middle" class="dg-t">PC-A envoie un broadcast</text>
<text x="280" y="190" text-anchor="middle" class="dg-i">chaque switch retransmet vers les deux autres, sans fin</text>
</svg>
</figure>

- SW1 reçoit le broadcast et le retransmet vers SW2 et SW3.
- SW2 le reçoit de SW1 et le retransmet vers SW3 (et SW1).
- SW3 le reçoit de SW1 et de SW2 et le retransmet vers SW1 et SW2, indéfiniment.

**Problème 2 : Instabilité de la table MAC**

Un switch reçoit la même trame de plusieurs ports → la MAC Source change de port → la table MAC est mise à jour en boucle → tous les paquets sont floodés.

**Conséquence :** Sans protection, une boucle réseau rend le réseau totalement inutilisable en quelques secondes.

---

#### B. La Solution : STP (802.1d)

**STP** (Spanning Tree Protocol) est un protocole de couche 2 qui :
1. **Détecte** automatiquement les boucles
2. **Bloque** logiquement certains ports pour casser la boucle
3. **Réactive** les ports bloqués si un lien principal tombe (redondance)

**Principe :** STP construit un **arbre recouvrant** (spanning tree) — une topologie sans boucle — en désactivant logiquement les liens redondants.

<figure class="graphe diagramme"><svg viewBox="0 0 760 210" style="max-width:760px" role="img" aria-label="Avant STP : boucle ; après STP : arbre sans boucle, un lien est bloqué logiquement" xmlns="http://www.w3.org/2000/svg">
<text x="190" y="22" text-anchor="middle" class="dg-t">Avant STP (boucle)</text>
<text x="570" y="22" text-anchor="middle" class="dg-t">Après STP (arbre sans boucle)</text>
<rect x="95" y="50" width="70" height="34" rx="8" class="dg-dark"/>
<text x="130.0" y="72.0" text-anchor="middle" class="dg-w">SW1</text>
<rect x="265" y="50" width="70" height="34" rx="8" class="dg-dark"/>
<text x="300.0" y="72.0" text-anchor="middle" class="dg-w">SW2</text>
<rect x="180" y="140" width="70" height="34" rx="8" class="dg-dark"/>
<text x="215.0" y="162.0" text-anchor="middle" class="dg-w">SW3</text>
<line x1="165" y1="67" x2="265" y2="67" class="dg-line"/>
<line x1="130" y1="84" x2="190" y2="140" class="dg-line"/>
<line x1="300" y1="84" x2="240" y2="140" class="dg-line"/>
<rect x="475" y="50" width="70" height="34" rx="8" class="dg-dark"/>
<text x="510.0" y="72.0" text-anchor="middle" class="dg-w">SW1</text>
<rect x="645" y="50" width="70" height="34" rx="8" class="dg-dark"/>
<text x="680.0" y="72.0" text-anchor="middle" class="dg-w">SW2</text>
<rect x="560" y="140" width="70" height="34" rx="8" class="dg-dark"/>
<text x="595.0" y="162.0" text-anchor="middle" class="dg-w">SW3</text>
<line x1="545" y1="67" x2="645" y2="67" class="dg-line"/>
<line x1="510" y1="84" x2="570" y2="140" class="dg-line"/>
<line x1="680" y1="84" x2="620" y2="140" class="dg-dash"/>
<text x="665" y="132" text-anchor="start" class="dg-i">lien bloqué</text>
<line x1="380" y1="95" x2="431.0" y2="95.0" class="dg-line"/>
<polygon points="440,95 431.0,90.0 431.0,100.0" class="dg-ah"/>
</svg>
</figure>

---

#### C. Fonctionnement de STP : Les 4 Étapes

##### Étape 1 : Élection du Root Bridge (Pont Racine)

Tous les switches échangent des messages **BPDU** (Bridge Protocol Data Unit) contenant leur **Bridge ID**.

**Bridge ID = Priorité (16 bits) + Adresse MAC (48 bits)**

- **Priorité par défaut :** 32768 (sur tous les switches Cisco 2960)
- **Le switch avec le Bridge ID le plus petit devient Root Bridge**
- En cas d'égalité de priorité → le switch avec la **MAC la plus faible** gagne

**Exemple :**

| **Switch** | **Priorité** | **MAC** | **Bridge ID** | **Résultat** |
|------------|-------------|---------|---------------|--------------|
| SW1 | 32768 | 00:0A:00:00:00:01 | 32768 + 00:0A... | |
| SW2 | 32768 | 00:0A:00:00:00:02 | 32768 + 00:0A... | |
| SW3 | **4096** | 00:0A:00:00:00:03 | 4096 + 00:0A... | **ROOT BRIDGE** (priorité plus basse) |

**Mémo :** "Priorité la **plus basse** = Root Bridge" (comme à une enchère : on veut la mise la plus basse)

##### Étape 2 : Élection des Root Ports

Sur chaque switch **non-root**, on élit le port offrant le **chemin le moins coûteux** vers le Root Bridge : le **Root Port (RP)**.

**Coût de port STP (selon la vitesse du lien) :**

| **Vitesse** | **Coût STP** |
|-------------|-------------|
| 10 Mb/s | 100 |
| 100 Mb/s (Fast Ethernet) | 19 |
| 1 Gb/s | 4 |
| 10 Gb/s | 2 |

*Principe : plus le lien est rapide, plus le coût est faible = chemin préféré*

##### Étape 3 : Élection des Designated Ports

Sur chaque **segment réseau** (chaque lien entre deux switches), un seul port est élu **Designated Port (DP)** : le port qui transmet les trames sur ce segment. C'est toujours le port du côté ayant le meilleur chemin vers le Root Bridge.

##### Étape 4 : Blocage des Ports Restants

Tout port qui n'est ni Root Port ni Designated Port devient **Alternate Port** et est placé en état **Blocking** : il ne transmet plus de trames normales, mais surveille les BPDU pour réagir en cas de panne.

---

#### D. États des Ports STP

| **État** | **Durée** | **Reçoit BPDU ?** | **Envoie BPDU ?** | **Transmet trames ?** | **Apprend MAC ?** |
|----------|-----------|-------------------|-------------------|----------------------|-------------------|
| **Blocking** | Permanent (ports redondants) | Oui | Non | Non | Non |
| **Listening** | 15 secondes (Forward Delay) | Oui | Oui | Non | Non |
| **Learning** | 15 secondes (Forward Delay) | Oui | Oui | Non | Oui |
| **Forwarding** | Permanent (ports actifs) | Oui | Oui | Oui | Oui |
| **Disabled** | Si admin ou erreur | Non | Non | Non | Non |

**Temps de convergence STP classique (802.1d) :**
- Détection de panne : ~20 secondes (Max Age timer)
- Listening + Learning : 2 × 15 secondes = 30 secondes
- **Total : ~50 secondes** avant qu'un lien de secours soit actif

**Remarque :** Rapid STP (802.1w / Rapid PVST+) réduit ce délai à **quelques secondes** — à voir en Année 2.

---

#### E. Observer STP avec Cisco IOS

```cisco
! Afficher l'état STP complet (VLAN 1 par défaut)
Switch# show spanning-tree

! Exemple de sortie commentée :
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769                  ← Priorité + ID VLAN
             Address     0001.4300.0001         ← MAC du Root Bridge
             This bridge is the root            ← Ce switch EST le root bridge
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.4300.0001
             ...

Interface        Role  Sts  Cost   Prio.Nbr Type
Fa0/1           Desg  FWD  19     128.1    P2p  ← Designated, Forwarding
Fa0/2           Desg  FWD  19     128.2    P2p
Fa0/3           Desg  FWD  19     128.3    P2p
```

**Lecture des colonnes :**
- **Role :** Root (RP), Desg (Designated), Altn (Alternate/Blocked)
- **Sts :** FWD (Forwarding), BLK (Blocking), LIS (Listening), LRN (Learning)
- **Cost :** Coût cumulatif vers le Root Bridge
- **Prio.Nbr :** Priorité du port (valeur par défaut 128)

---

### V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **DNS** | Domain Name System – traduit noms de domaine ↔ adresses IP |
| **Zone directe** | Fichier de zone résolvant noms → IP (enregistrements A) |
| **Zone inverse** | Fichier de zone résolvant IP → noms (enregistrements PTR) |
| **Enregistrement A** | Associe un nom à une adresse IPv4 |
| **Enregistrement PTR** | Associe une adresse IP à un nom (zone inverse) |
| **Enregistrement CNAME** | Alias pointant vers un autre nom canonique |
| **Enregistrement NS** | Déclare les serveurs de noms d'une zone |
| **SOA** | Start of Authority – métadonnées de la zone (serial, refresh, TTL…) |
| **FQDN** | Fully Qualified Domain Name – nom absolu terminé par un `.` |
| **TTL** | Time To Live – durée de validité en cache d'un enregistrement DNS |
| **Résolveur** | Client DNS qui interroge les serveurs |
| **Serveur autoritaire** | Serveur possédant les fichiers de zone officiels |
| **Forwarder** | Serveur externe auquel on délègue les requêtes non résolues localement |
| **Bind9** | Implémentation de référence du serveur DNS sous Linux |
| **named** | Démon Bind9 (Named = Name Daemon) |
| **named.conf** | Fichier de configuration principal de Bind9 |
| **named-checkzone** | Commande de vérification syntaxique d'un fichier de zone |
| **dig** | Outil de diagnostic DNS en ligne de commande |
| **STP** | Spanning Tree Protocol (802.1d) – évite les boucles réseau en couche 2 |
| **Root Bridge** | Switch élu centre de l'arbre STP (Bridge ID le plus faible) |
| **Bridge ID** | Identifiant STP = Priorité (16 bits) + Adresse MAC (48 bits) |
| **BPDU** | Bridge Protocol Data Unit – messages échangés par STP |
| **Root Port** | Port du switch non-root offrant le meilleur chemin vers le Root Bridge |
| **Designated Port** | Port en Forwarding sur chaque segment réseau |
| **Alternate Port** | Port bloqué (logiquement désactivé) par STP |
| **Blocking** | État STP d'un port : reçoit les BPDU mais ne transmet pas de trames |
| **Forwarding** | État STP d'un port actif : transmet les trames normalement |
| **Broadcast Storm** | Saturation réseau causée par une boucle et une trame broadcast en boucle infinie |

---

### VI. Exercices d'Entraînement

#### Exercice 1 : Fichier de Zone

Écrire les enregistrements DNS pour le réseau `cabinet-archi.local` (192.168.50.0/24) :

| **Machine** | **Nom** | **IP** |
|-------------|---------|--------|
| Serveur DNS | ns1.cabinet-archi.local | 192.168.50.1 |
| Serveur web | www.cabinet-archi.local | 192.168.50.100 |
| Serveur impression | print.cabinet-archi.local | 192.168.50.200 |
| Alias | web.cabinet-archi.local | → www |

1. Écrire le fichier de zone directe complet (avec SOA)
2. Écrire les 4 enregistrements PTR correspondants
3. Quel est le nom de la zone inverse pour `192.168.50.0/24` ?

---

#### Exercice 2 : Analyse STP

Trois switches reliés en triangle. Les Bridge IDs sont :

- SW-A : Priorité 32768, MAC `0010.0000.0001`
- SW-B : Priorité 32768, MAC `0010.0000.0002`
- SW-C : Priorité **4096**, MAC `0010.0000.0003`

1. Quel switch devient Root Bridge ? Pourquoi ?
2. Sur SW-A : quel est son Root Port (liaison vers SW-B ou vers SW-C) ?
3. Sur SW-B : quel port sera probablement en état Blocking ?
4. Si SW-C tombe en panne, que se passe-t-il ?

---

#### Exercice 3 : Diagnostic DNS

Une machine cliente (`192.168.10.30`) n'arrive pas à résoudre `www.techpro.local`. Proposer une procédure de diagnostic de 5 étapes, de la plus simple à la plus complexe.

---

### VII. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer le rôle du DNS et les 4 étapes de la résolution récursive
- [ ] Distinguer zone directe (A) et zone inverse (PTR)
- [ ] Lire et interpréter un enregistrement A, PTR, CNAME, NS, SOA
- [ ] Installer Bind9 sous Debian et vérifier qu'il tourne
- [ ] Configurer `named.conf.options` (forwarders, allow-recursion)
- [ ] Créer un fichier de zone directe avec enregistrements A et CNAME
- [ ] Créer un fichier de zone inverse avec enregistrements PTR
- [ ] Vérifier la syntaxe avec `named-checkzone` avant de redémarrer
- [ ] Tester avec `dig` et `nslookup` les résolutions directe et inverse
- [ ] Expliquer pourquoi les boucles réseau sont dangereuses (broadcast storm)
- [ ] Décrire le processus d'élection du Root Bridge STP (Bridge ID le plus faible)
- [ ] Identifier les états des ports STP (Forwarding, Blocking, Listening, Learning)
- [ ] Lire `show spanning-tree` et identifier Root Port, Designated Port, Alternate Port
