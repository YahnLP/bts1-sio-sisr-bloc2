# Cours – DHCP Windows Server, partages NTFS et DFS

*BTS SIO SISR 1re année – Bloc 2 – S12 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S10, S11 ; S07 (DHCP).
> - **Matériel :** VM Windows Server (domaine), VM cliente.

## À la fin de ce cours, tu sauras

- [ ] **Installer** le rôle DHCP, **créer** une étendue, des options et une réservation ;
- [ ] **Configurer** un relais DHCP pour plusieurs sous-réseaux ;
- [ ] **Combiner** droits NTFS et droits de partage (le plus restrictif l'emporte) ;
- [ ] **Construire** une arborescence par service avec des groupes de sécurité ;
- [ ] **Créer** un espace de noms DFS.

---

## I. Le Service DHCP : Automatiser l'attribution des adresses IP

### A. Pourquoi le DHCP ? Le problème de l'adressage manuel

Dans une entreprise, chaque équipement réseau (PC, imprimante, téléphone IP…) a besoin d'une adresse IP unique pour communiquer. Si chaque technicien configure les adresses manuellement, on obtient rapidement :

- Des **conflits d'adresses** (deux machines avec la même IP)
- Des **erreurs de configuration** (mauvaise passerelle, mauvais DNS)
- Un **temps de travail considérable** pour gérer des centaines de postes

 **Lien avec ITIL — Gestion des Configurations :** Dans le référentiel ITIL 4, le service DHCP relève de la pratique de **Gestion des actifs et de la configuration**. Il garantit que chaque élément de configuration (CI) reçoit les bons paramètres réseau de manière fiable, traçable et cohérente. Un serveur DHCP bien configuré est la base d'un parc informatique sain.

**Le DHCP (Dynamic Host Configuration Protocol)** résout ce problème : c'est un protocole qui permet à un serveur de distribuer **automatiquement** la configuration réseau à tous les clients qui en font la demande.

---

### B. Le Protocole DORA — Les 4 Étapes du Dialogue DHCP

Quand un client démarrage sans adresse IP, il engage un dialogue en **4 étapes** avec le serveur DHCP. On retient ces étapes avec le mot **DORA** :

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

*Légende : Schéma représentant les 4 échanges du protocole DHCP entre un client (à gauche) et un serveur (à droite). Les flèches pleines représentent des messages Broadcast (envoyés à toute la plage réseau). Les flèches en pointillés représentent des messages Unicast (adressés directement).*

| **Étape** | **Émetteur** | **Mode** | **Message** |
|-----------|-------------|---------|-------------|
| **D**ISCOVER | Client | Broadcast | "Quelqu'un peut me donner une IP ?" |
| **O**FFER | Serveur | Unicast ou Broadcast | "Je t'offre telle adresse, disponible 8 jours" |
| **R**EQUEST | Client | Broadcast | "J'accepte cette offre" |
| **A**CKNOWLEDGE | Serveur | Unicast | "C'est confirmé, voici ta configuration complète" |

 **Moyen mnémotechnique :** **D**iscuter → **O**btenir → **R**épondre → **A**ccuser réception

---

### C. L'Étendue DHCP (Scope)

Une **étendue** est la plage d'adresses IP qu'un serveur DHCP peut distribuer sur un réseau donné. C'est le "stock" d'adresses disponibles.

<figure class="graphe diagramme"><svg viewBox="0 0 760 160" style="max-width:760px" role="img" aria-label="Étendue 192.168.1.0/24 : plage exclue, réservations et plage distribuable" xmlns="http://www.w3.org/2000/svg">
<text x="380" y="22" text-anchor="middle" class="dg-t">Étendue DHCP - réseau 192.168.1.0/24 (plage .1 à .254)</text>
<rect x="10" y="40" width="170" height="70" rx="8" class="dg-bad"/>
<text x="95.0" y="62.0" text-anchor="middle" class="dg-t">Exclues</text>
<text x="95.0" y="80.0" text-anchor="middle" class="dg-s">.1 à .9</text>
<text x="95.0" y="98.0" text-anchor="middle" class="dg-s">routeur, serveurs</text>
<rect x="190" y="40" width="150" height="70" rx="8" class="dg-ok"/>
<text x="265.0" y="71.0" text-anchor="middle" class="dg-t">Distribuables</text>
<text x="265.0" y="89.0" text-anchor="middle" class="dg-s">.10 à .49</text>
<rect x="350" y="40" width="170" height="70" rx="8" class="dg-warn"/>
<text x="435.0" y="71.0" text-anchor="middle" class="dg-t">Réservées (par MAC)</text>
<text x="435.0" y="89.0" text-anchor="middle" class="dg-s">.50 à .60</text>
<rect x="530" y="40" width="220" height="70" rx="8" class="dg-ok"/>
<text x="640.0" y="62.0" text-anchor="middle" class="dg-t">Distribuables</text>
<text x="640.0" y="80.0" text-anchor="middle" class="dg-s">.61 à .200</text>
<text x="640.0" y="98.0" text-anchor="middle" class="dg-s">clients, à la demande</text>
<text x="380" y="140" text-anchor="middle" class="dg-i">adresses fixes en dehors de la distribution automatique ; réservations liées à l&#x27;adresse MAC</text>
</svg>
</figure>

*Légende : Schéma d'une étendue DHCP montrant la répartition entre les adresses exclues (réservées aux équipements fixes : serveurs, routeurs), les réservations (adresses attribuées à des machines spécifiques via leur adresse MAC) et la plage distribuable aux clients.*

| **Paramètre de l'étendue** | **Description** | **Exemple** |
|---------------------------|-----------------|-------------|
| **Plage d'adresses** | Premier et dernier IP distribuables | 192.168.1.10 → 192.168.1.200 |
| **Masque de sous-réseau** | Masque associé au réseau | 255.255.255.0 (/24) |
| **Durée du bail** | Temps pendant lequel le client "loue" l'IP | 8 jours |
| **Exclusions** | Adresses dans la plage NON distribuées | 192.168.1.10 → 192.168.1.20 (serveurs) |
| **Réservations** | IP fixe liée à une adresse MAC spécifique | MAC aa:bb:cc → toujours 192.168.1.51 |

---

### D. Les Options DHCP

En plus de l'adresse IP, le serveur DHCP peut transmettre d'autres paramètres de configuration réseau, appelés **options**. Elles sont identifiées par un numéro :

| **N° d'option** | **Nom** | **Description** | **Exemple** |
|-----------------|---------|-----------------|-------------|
| **003** | Routeur | Adresse de la passerelle par défaut | 192.168.1.1 |
| **006** | Serveurs DNS | Adresses des serveurs DNS | 192.168.1.10, 8.8.8.8 |
| **015** | Nom de domaine | Suffixe DNS du domaine | siosolutions.local |
| **044** | Serveurs WINS | Pour résolution NetBIOS (legacy) | 192.168.1.11 |

 **Niveaux d'options :** Les options peuvent être définies à 3 niveaux, avec la priorité suivante (du plus général au plus spécifique) :

```
Niveau SERVEUR (s'applique à toutes les étendues)
    └─► Niveau ÉTENDUE (s'applique à une étendue spécifique)
            └─► Niveau RÉSERVATION (s'applique à une machine spécifique)
```

*Légende : Diagramme montrant la hiérarchie des options DHCP. Une option définie au niveau Réservation écrase celle de l'Étendue, qui elle-même écrase celle du Serveur. Cela permet une grande flexibilité : des paramètres globaux avec des exceptions ciblées.*

---

### E. L'Agent Relais DHCP (DHCP Relay Agent)

#### Le problème : DHCP et les routeurs ne font pas bon ménage

Le protocole DHCP fonctionne par **broadcasts** (diffusions sur tout le réseau local). Or, **un routeur bloque par défaut les broadcasts** : ils ne traversent pas les frontières entre sous-réseaux.

**Conséquence :** Si une entreprise a 3 réseaux (ex : 192.168.1.0/24, 192.168.2.0/24, 192.168.3.0/24), il faudrait théoriquement **3 serveurs DHCP distincts**, un par réseau. C'est coûteux et difficile à maintenir.

#### La solution : l'Agent Relais

L'**agent relais DHCP** (RFC 3046) est un composant configuré sur un routeur ou un serveur dans chaque sous-réseau "distant". Son rôle est de :

1. **Intercepter** les broadcasts DISCOVER des clients locaux
2. **Les convertir** en messages unicast
3. **Les transmettre** au serveur DHCP centralisé (dans un autre sous-réseau)
4. **Relayer** la réponse du serveur au client

<figure class="graphe diagramme"><svg viewBox="0 0 780 320" style="max-width:780px" role="img" aria-label="Agent relais : un seul serveur DHCP central sert deux sous-réseaux" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="10" width="270" height="130" rx="10" class="dg-soft"/>
<text x="145" y="30" text-anchor="middle" class="dg-t">Sous-réseau A - 192.168.1.0/24</text>
<rect x="60" y="56" width="170" height="50" rx="8" class="dg-box"/>
<text x="145.0" y="77.0" text-anchor="middle" class="dg-t">Client A</text>
<text x="145.0" y="95.0" text-anchor="middle" class="dg-s">sans IP</text>
<rect x="500" y="10" width="270" height="130" rx="10" class="dg-soft"/>
<text x="635" y="30" text-anchor="middle" class="dg-t">Sous-réseau B - 192.168.2.0/24</text>
<rect x="550" y="56" width="170" height="50" rx="8" class="dg-box"/>
<text x="635.0" y="77.0" text-anchor="middle" class="dg-t">Client B</text>
<text x="635.0" y="95.0" text-anchor="middle" class="dg-s">sans IP</text>
<rect x="330" y="52" width="120" height="60" rx="8" class="dg-dark"/>
<text x="390.0" y="78.0" text-anchor="middle" class="dg-w">Routeur</text>
<text x="390.0" y="96.0" text-anchor="middle" class="dg-s">agent relais</text>
<line x1="230" y1="81" x2="321.0" y2="81.0" class="dg-line"/>
<polygon points="330,81 321.0,76.0 321.0,86.0" class="dg-ah"/>
<text x="280.0" y="75.0" text-anchor="middle" class="dg-s">broadcast</text>
<line x1="550" y1="81" x2="459.0" y2="81.0" class="dg-line"/>
<polygon points="450,81 459.0,86.0 459.0,76.0" class="dg-ah"/>
<text x="500" y="70" text-anchor="middle" class="dg-s">DISCOVER</text>
<rect x="280" y="220" width="220" height="70" rx="8" class="dg-ok"/>
<text x="390.0" y="242.0" text-anchor="middle" class="dg-t">Serveur DHCP central</text>
<text x="390.0" y="260.0" text-anchor="middle" class="dg-s">192.168.1.10</text>
<text x="390.0" y="278.0" text-anchor="middle" class="dg-s">gère toutes les plages</text>
<line x1="390" y1="112" x2="390.0" y2="211.0" class="dg-line"/>
<polygon points="390,220 395.0,211.0 385.0,211.0" class="dg-ah"/>
<text x="470" y="170" text-anchor="middle" class="dg-i">unicast relayé</text>
</svg>
</figure>

*Légende : Schéma montrant comment l'agent relais DHCP permet à un seul serveur DHCP centralisé (en bas) de servir deux sous-réseaux distincts séparés par un routeur. Sans l'agent relais, le broadcast du Client B ne dépasserait jamais la frontière du sous-réseau B.*

 **En résumé :** L'agent relais est la solution pour centraliser le service DHCP tout en desservant plusieurs sous-réseaux depuis un seul serveur. C'est une bonne pratique d'administration qui simplifie la gestion et réduit les coûts.

---

## II. Partage de Fichiers Windows : Droits NTFS vs Droits de Partage

### A. Pourquoi deux types de droits ?

Quand un utilisateur accède à un dossier partagé sur un serveur Windows, **deux filtres de sécurité** s'appliquent successivement :

1. **Les droits de partage** (Share Permissions) : contrôlent l'accès **via le réseau** uniquement
2. **Les droits NTFS** : contrôlent l'accès **au niveau du système de fichiers**, que l'on accède au dossier via le réseau ou localement

<figure class="graphe diagramme"><svg viewBox="0 0 560 300" style="max-width:560px" role="img" aria-label="Deux filtres successifs : droits de partage puis droits NTFS" xmlns="http://www.w3.org/2000/svg">
<rect x="160" y="8" width="240" height="40" rx="8" class="dg-dark"/>
<text x="280.0" y="33.0" text-anchor="middle" class="dg-w">Utilisateur réseau</text>
<line x1="280" y1="48" x2="280.0" y2="67.0" class="dg-line"/>
<polygon points="280,76 285.0,67.0 275.0,67.0" class="dg-ah"/>
<rect x="130" y="76" width="300" height="66" rx="8" class="dg-box"/>
<text x="280.0" y="96.0" text-anchor="middle" class="dg-t">Droits de partage</text>
<text x="280.0" y="114.0" text-anchor="middle" class="dg-s">Contrôle total / Modifier / Lecture</text>
<text x="280.0" y="132.0" text-anchor="middle" class="dg-s">premier filtre : accès réseau</text>
<line x1="280" y1="142" x2="280.0" y2="161.0" class="dg-line"/>
<polygon points="280,170 285.0,161.0 275.0,161.0" class="dg-ah"/>
<text x="300" y="160" text-anchor="start" class="dg-i">si accordé</text>
<rect x="130" y="170" width="300" height="66" rx="8" class="dg-box"/>
<text x="280.0" y="190.0" text-anchor="middle" class="dg-t">Droits NTFS</text>
<text x="280.0" y="208.0" text-anchor="middle" class="dg-s">Contrôle total / Modifier / Lecture / Écriture...</text>
<text x="280.0" y="226.0" text-anchor="middle" class="dg-s">deuxième filtre : système de fichiers</text>
<line x1="280" y1="236" x2="280.0" y2="249.0" class="dg-line"/>
<polygon points="280,258 285.0,249.0 275.0,249.0" class="dg-ah"/>
<rect x="130" y="258" width="300" height="34" rx="8" class="dg-warn"/>
<text x="280.0" y="280.0" text-anchor="middle" class="dg-t">Accès effectif : le plus restrictif des deux</text>
</svg>
</figure>

*Légende : Schéma illustrant les deux couches de sécurité appliquées lors d'un accès réseau à un dossier partagé Windows. L'accès effectif final est toujours le plus restrictif entre le droit de partage et le droit NTFS accordés.*

---

### B. La Règle d'Or : Le Plus Restrictif l'Emporte

Lorsqu'un utilisateur accède à un partage réseau, Windows calcule l'**accès effectif** en combinant les deux types de droits. La règle est simple mais absolue :

> **Accès effectif = MINIMUM entre (Droit de partage) et (Droit NTFS)**

**Exemples de calcul :**

| **Droit de partage** | **Droit NTFS** | **Accès effectif** | **Explication** |
|---------------------|----------------|-------------------|-----------------|
| Contrôle total | Lecture | **Lecture** | NTFS est plus restrictif |
| Lecture | Contrôle total | **Lecture** | Partage est plus restrictif |
| Modifier | Modifier | **Modifier** | Les deux sont identiques |
| Lecture | Aucun accès | **Aucun accès** | NTFS est plus restrictif |
| Contrôle total | Contrôle total | **Contrôle total** | Les deux accordent le max |

 **Bonne pratique professionnelle :** En entreprise, on définit généralement le droit de partage au niveau **"Contrôle total"** pour le groupe *Tout le monde* ou *Utilisateurs authentifiés*, puis on **gère toute la sécurité uniquement via les droits NTFS**. Cela évite la double gestion et simplifie la maintenance.

---

### C. Les Niveaux de Droits NTFS

Les droits NTFS sont plus **granulaires** que les droits de partage. Les permissions standards les plus utilisées sont :

| **Permission NTFS** | **Lire** | **Lister** | **Écrire** | **Modifier** | **Supprimer** | **Changer droits** |
|--------------------|:--------:|:----------:|:----------:|:------------:|:-------------:|:-----------------:|
| Lecture | Oui | Oui | Non | Non | Non | Non |
| Lecture et exécution | Oui | Oui | Non | Non | Non | Non |
| Listage du dossier | Non | Oui | Non | Non | Non | Non |
| Écriture | Oui | Oui | Oui | Non | Non | Non |
| Modifier | Oui | Oui | Oui | Oui | Oui | Non |
| Contrôle total | Oui | Oui | Oui | Oui | Oui | Oui |

 **Lien ITIL — Gestion des Accès :** Dans ITIL 4, la pratique de **Gestion des accès** vise à garantir que seules les personnes autorisées peuvent consulter ou modifier une information. Configurer correctement les droits NTFS par groupe d'utilisateurs (et non par utilisateur individuel) est une bonne pratique d'administration qui facilite la gestion des départs/arrivées dans l'entreprise.

---

### D. Créer une Arborescence par Service : Bonne Pratique

En entreprise, les dossiers partagés sont organisés **par service métier**. Chaque service a son dossier, et les droits sont accordés à des **groupes AD** (pas à des utilisateurs individuels) :

```
\\SERVEUR\Partages\
    ├── RH\               ──► Groupe AD : GRP_RH       (Modifier)
    │                    ──► Groupe AD : GRP_Direction  (Lecture)
    │                    ──► Groupe AD : GRP_IT         (Contrôle total)
    │
    ├── Comptabilite\     ──► Groupe AD : GRP_Compta    (Modifier)
    │                    ──► Groupe AD : GRP_Direction  (Lecture)
    │                    ──► Groupe AD : GRP_IT         (Contrôle total)
    │
    ├── Informatique\     ──► Groupe AD : GRP_IT        (Contrôle total)
    │
    ├── Direction\        ──► Groupe AD : GRP_Direction (Contrôle total)
    │                    ──► Groupe AD : GRP_IT         (Contrôle total)
    │
    └── Commun\           ──► Groupe AD : Utilisateurs  (Modifier)
                         ──► Groupe AD : GRP_IT         (Contrôle total)
```

*Légende : Exemple d'arborescence de dossiers partagés organisée par service dans une entreprise. Chaque dossier est associé à un ou plusieurs groupes Active Directory avec des niveaux de droits différents. Le dossier "Commun" est accessible à tous les utilisateurs authentifiés.*

 **Règle d'organisation :** Ne jamais accorder des droits directement à un utilisateur individuel. Toujours passer par des **groupes AD**. Ainsi, quand un collaborateur change de service ou quitte l'entreprise, il suffit de modifier son appartenance aux groupes, sans toucher aux droits des dossiers.

---

## III. DFS (Distributed File System) — Introduction

### A. Le Problème que DFS Résout

Imaginez que votre entreprise stocke ses fichiers sur le serveur `\\SERVEUR-FS01\Partages`. 120 utilisateurs ont ce chemin dans leurs raccourcis, scripts de connexion et favoris réseau.

Un jour, vous migrez les fichiers vers un nouveau serveur plus performant : `\\SERVEUR-FS02`. Résultat : **tous les 120 raccourcis sont cassés**. Il faut reconfigurer chaque poste — un travail colossal.

**DFS (Distributed File System)** résout ce problème en introduisant une **couche d'abstraction** entre les utilisateurs et les serveurs physiques.

---

### B. Le Principe des Espaces de Noms DFS

Avec DFS, les utilisateurs accèdent aux fichiers via un **espace de noms** (namespace) — un chemin "logique" qui ne change jamais — plutôt que directement via le chemin physique du serveur.

<figure class="graphe diagramme"><svg viewBox="0 0 780 230" style="max-width:780px" role="img" aria-label="Espace de noms DFS : un chemin logique stable pointe vers des dossiers physiques" xmlns="http://www.w3.org/2000/svg">
<text x="110" y="22" text-anchor="middle" class="dg-t">Utilisateurs</text>
<text x="390" y="22" text-anchor="middle" class="dg-t">Espace de noms DFS (chemin logique stable)</text>
<text x="660" y="22" text-anchor="middle" class="dg-t">Dossiers physiques</text>
<rect x="280" y="40" width="220" height="170" rx="8" class="dg-ok"/>
<text x="390.0" y="112.0" text-anchor="middle" class="dg-t">\\domaine\partages\</text>
<text x="390.0" y="130.0" text-anchor="middle" class="dg-s">serveur DFS</text>
<text x="390.0" y="148.0" text-anchor="middle" class="dg-s">point d&#x27;entrée unique</text>
<rect x="10" y="44" width="200" height="34" rx="8" class="dg-box"/>
<text x="110.0" y="66.0" text-anchor="middle" class="dg-t">\\domaine\partages\RH</text>
<line x1="210" y1="61" x2="271.0" y2="61.0" class="dg-line"/>
<polygon points="280,61 271.0,56.0 271.0,66.0" class="dg-ah"/>
<rect x="560" y="44" width="210" height="34" rx="8" class="dg-soft"/>
<text x="665.0" y="66.0" text-anchor="middle" class="dg-t">\\SERVEUR-FS01\RH</text>
<line x1="500" y1="61" x2="551.0" y2="61.0" class="dg-line"/>
<polygon points="560,61 551.0,56.0 551.0,66.0" class="dg-ah"/>
<rect x="10" y="86" width="200" height="34" rx="8" class="dg-box"/>
<text x="110.0" y="108.0" text-anchor="middle" class="dg-t">\\domaine\partages\Compta</text>
<line x1="210" y1="103" x2="271.0" y2="103.0" class="dg-line"/>
<polygon points="280,103 271.0,98.0 271.0,108.0" class="dg-ah"/>
<rect x="560" y="86" width="210" height="34" rx="8" class="dg-soft"/>
<text x="665.0" y="108.0" text-anchor="middle" class="dg-t">\\SERVEUR-FS01\Compta</text>
<line x1="500" y1="103" x2="551.0" y2="103.0" class="dg-line"/>
<polygon points="560,103 551.0,98.0 551.0,108.0" class="dg-ah"/>
<rect x="10" y="128" width="200" height="34" rx="8" class="dg-box"/>
<text x="110.0" y="150.0" text-anchor="middle" class="dg-t">\\domaine\partages\IT</text>
<line x1="210" y1="145" x2="271.0" y2="145.0" class="dg-line"/>
<polygon points="280,145 271.0,140.0 271.0,150.0" class="dg-ah"/>
<rect x="560" y="128" width="210" height="34" rx="8" class="dg-soft"/>
<text x="665.0" y="150.0" text-anchor="middle" class="dg-t">\\SERVEUR-FS02\IT</text>
<line x1="500" y1="145" x2="551.0" y2="145.0" class="dg-line"/>
<polygon points="560,145 551.0,140.0 551.0,150.0" class="dg-ah"/>
<rect x="560" y="170" width="210" height="34" rx="8" class="dg-soft"/>
<text x="665.0" y="192.0" text-anchor="middle" class="dg-t">\\NAS-01\Backups</text>
<line x1="500" y1="187" x2="551.0" y2="187.0" class="dg-line"/>
<polygon points="560,187 551.0,182.0 551.0,192.0" class="dg-ah"/>
</svg>
</figure>

*Légende : Schéma montrant le fonctionnement de DFS Namespaces. À gauche, les utilisateurs accèdent à un chemin logique unique (l'espace de noms DFS). À droite, ce chemin pointe vers des serveurs et dossiers physiques réels, qui peuvent changer sans que le chemin utilisateur soit impacté.*

**Vocabulaire DFS à connaître :**

| **Terme** | **Définition** |
|-----------|---------------|
| **Espace de noms (Namespace)** | Le chemin logique stable que voient les utilisateurs (ex : `\\siosarl.local\data`) |
| **Racine DFS (DFS Root)** | Le point d'entrée de l'espace de noms (ex : `\\siosarl.local\data`) |
| **Dossier DFS (DFS Folder)** | Un sous-chemin dans l'espace de noms (ex : `\\siosarl.local\data\RH`) |
| **Cible (Target)** | Le dossier physique réel pointé par un dossier DFS (ex : `\\SERVEUR-FS01\RH`) |
| **Référence (Referral)** | La liste des cibles retournée au client quand il demande l'accès à un dossier DFS |

---

### C. Les Bénéfices du DFS — Vision ITIL

| **Bénéfice** | **Pratique ITIL associée** | **Explication** |
|-------------|--------------------------|----------------|
| **Indépendance physique** | Gestion de la disponibilité | Si FS01 tombe, on repointe vers FS02 sans toucher aux postes utilisateurs |
| **Migration transparente** | Gestion des changements | Migrer un serveur de fichiers sans interrompre le service ni reconfigurer les postes |
| **Simplification des UNC** | Gestion des configurations | Un seul chemin à mémoriser et à documenter |
| **Réplication DFS** (avancé) | Continuité de service | Synchronisation automatique des fichiers entre plusieurs sites |

 **Lien ITIL — Gestion de la Disponibilité :** Le DFS Namespaces est un excellent exemple de la pratique ITIL de gestion de la disponibilité : on conçoit le service de fichiers pour qu'il soit **résilient aux changements d'infrastructure** sans impacter l'expérience des utilisateurs.

---

## IV. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|---------------|
| **DHCP** | Dynamic Host Configuration Protocol — protocole d'attribution automatique de la configuration réseau |
| **DORA** | Séquence des 4 étapes DHCP : Discover, Offer, Request, Acknowledge |
| **Étendue (Scope)** | Plage d'adresses IP qu'un serveur DHCP peut distribuer sur un réseau |
| **Bail (Lease)** | Durée pendant laquelle un client "loue" une adresse IP obtenue via DHCP |
| **Exclusion** | Plage d'adresses dans l'étendue que le serveur DHCP ne distribuera jamais |
| **Réservation** | Association fixe entre une adresse MAC et une adresse IP dans une étendue DHCP |
| **Option DHCP** | Paramètre réseau supplémentaire transmis au client (passerelle, DNS, etc.) |
| **Agent relais** | Composant qui relaie les broadcasts DHCP entre sous-réseaux, vers un serveur DHCP centralisé |
| **Droits de partage** | Permissions contrôlant l'accès à un dossier **via le réseau** uniquement (3 niveaux) |
| **Droits NTFS** | Permissions du système de fichiers applicables localement ET en réseau (plus granulaires) |
| **Règle du plus restrictif** | Lors d'un accès réseau, l'accès effectif = le minimum entre droits de partage et droits NTFS |
| **Chemin UNC** | Universal Naming Convention — chemin réseau du type `\\SERVEUR\Partage\Dossier` |
| **DFS** | Distributed File System — système de fichiers distribué offrant des chemins d'accès logiques stables |
| **Espace de noms DFS** | Chemin logique stable utilisé par les utilisateurs, indépendant de l'emplacement physique des fichiers |
| **Cible DFS** | Dossier physique réel pointé par un dossier dans l'espace de noms DFS |
| **Groupe AD** | Objet Active Directory regroupant des utilisateurs pour simplifier la gestion des droits |

---

## V. Questions de Réflexion (Pour aller plus loin)

1. **Pourquoi un message DISCOVER est-il envoyé en broadcast et pas directement au serveur DHCP ?**
   - *Piste : À l'instant du DISCOVER, le client ne connaît pas encore l'adresse IP du serveur DHCP...*

2. **Qu'est-ce qui se passe si la durée du bail DHCP expire et que le client n'a pas pu renouveler son adresse ?**
   - *Piste : À mi-bail, le client essaie de renouveler. Si le serveur est injoignable à l'expiration complète, que fait le client ?*

3. **Dans quel scénario est-il dangereux de mettre "Contrôle total" côté NTFS et "Lecture" côté partage ?**
   - *Piste : Et si l'utilisateur se connecte localement au serveur ? Les droits de partage s'appliquent-ils ?*

4. **Pourquoi vaut-il mieux gérer les droits via des groupes AD plutôt que directement par utilisateur ?**
   - *Piste : Pensez à ce qui se passe quand un collaborateur change de service ou quitte l'entreprise.*

5. **En quoi le DFS peut-il être considéré comme une mesure de "continuité de service" au sens ITIL ?**
   - *Piste : Pensez à la situation où le serveur hébergeant les fichiers tombe en panne. Que se passe-t-il côté utilisateurs ?*

---

## VI. Ressources pour Approfondir

**Documentation Microsoft officielle :**
- [Rôle DHCP Windows Server — docs.microsoft.com](https://docs.microsoft.com/fr-fr/windows-server/networking/technologies/dhcp/dhcp-top)
- [DFS Namespaces — Vue d'ensemble](https://docs.microsoft.com/fr-fr/windows-server/storage/dfs-namespaces/dfs-overview)
- [Contrôle des accès aux partages de fichiers](https://docs.microsoft.com/fr-fr/windows-server/storage/file-server/)

**Outils pratiques :**
- `ipconfig /all` — Voir la configuration IP et l'adresse du serveur DHCP
- `ipconfig /release` puis `ipconfig /renew` — Libérer et renouveler un bail DHCP
- `net use * \\serveur\partage` — Connecter un lecteur réseau en ligne de commande

---

## Auto-évaluation : Suis-je Prêt ?

Après avoir terminé cette séance, je suis capable de :

- [ ] Expliquer le rôle du DHCP et les 4 étapes du protocole DORA
- [ ] Identifier les paramètres d'une étendue DHCP (plage, bail, exclusions, options)
- [ ] Expliquer à quoi sert un agent relais DHCP et dans quel contexte il est nécessaire
- [ ] Distinguer les droits de partage et les droits NTFS et expliquer la règle du plus restrictif
- [ ] Calculer l'accès effectif dans un tableau de droits croisés
- [ ] Créer une arborescence de dossiers partagés et affecter des droits par groupe AD
- [ ] Expliquer le principe des espaces de noms DFS et les avantages pour l'entreprise
- [ ] Compléter une fiche de configuration DHCP et une fiche de documentation des droits

---
