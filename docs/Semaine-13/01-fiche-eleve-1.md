---
author: YLP
title: 📚 FICHE DE COURS
---

# 📚 FICHE DE COURS ÉLÈVE
## "DHCP Windows Server, Partage de Fichiers et DFS"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 13*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.2** | Installer et configurer un service réseau (DHCP) |
| **B2.3** | Assurer la sécurité des accès aux ressources partagées |
| **B1.2** | Recenser et identifier les ressources et les besoins |
| **B1.4** | Mettre en place et vérifier les niveaux d'habilitation associés à un service |

---

## I. Le Service DHCP : Automatiser l'attribution des adresses IP

### A. Pourquoi le DHCP ? Le problème de l'adressage manuel

Dans une entreprise, chaque équipement réseau (PC, imprimante, téléphone IP…) a besoin d'une adresse IP unique pour communiquer. Si chaque technicien configure les adresses manuellement, on obtient rapidement :

- Des **conflits d'adresses** (deux machines avec la même IP)
- Des **erreurs de configuration** (mauvaise passerelle, mauvais DNS)
- Un **temps de travail considérable** pour gérer des centaines de postes

💡 **Lien avec ITIL — Gestion des Configurations :** Dans le référentiel ITIL 4, le service DHCP relève de la pratique de **Gestion des actifs et de la configuration**. Il garantit que chaque élément de configuration (CI) reçoit les bons paramètres réseau de manière fiable, traçable et cohérente. Un serveur DHCP bien configuré est la base d'un parc informatique sain.

**Le DHCP (Dynamic Host Configuration Protocol)** résout ce problème : c'est un protocole qui permet à un serveur de distribuer **automatiquement** la configuration réseau à tous les clients qui en font la demande.

---

### B. Le Protocole DORA — Les 4 Étapes du Dialogue DHCP

Quand un client démarrage sans adresse IP, il engage un dialogue en **4 étapes** avec le serveur DHCP. On retient ces étapes avec le mot **DORA** :

```
CLIENT (sans IP)                              SERVEUR DHCP
     │                                              │
     │  1. DISCOVER  ─────────────────────────────► │
     │  (Broadcast - "Y a-t-il un serveur DHCP ?")  │
     │                                              │
     │  ◄──────────────────────────── 2. OFFER      │
     │  (Unicast - "Je te propose 192.168.1.50")    │
     │                                              │
     │  3. REQUEST  ──────────────────────────────► │
     │  (Broadcast - "J'accepte 192.168.1.50 !")    │
     │                                              │
     │  ◄───────────────────────── 4. ACKNOWLEDGE   │
     │  (Unicast - "C'est confirmé. Bail : 8 jours")│
     │                                              │
```

*Légende : Schéma représentant les 4 échanges du protocole DHCP entre un client (à gauche) et un serveur (à droite). Les flèches pleines représentent des messages Broadcast (envoyés à toute la plage réseau). Les flèches en pointillés représentent des messages Unicast (adressés directement).*

| **Étape** | **Émetteur** | **Mode** | **Message** |
|-----------|-------------|---------|-------------|
| **D**ISCOVER | Client | Broadcast | "Quelqu'un peut me donner une IP ?" |
| **O**FFER | Serveur | Unicast ou Broadcast | "Je t'offre telle adresse, disponible 8 jours" |
| **R**EQUEST | Client | Broadcast | "J'accepte cette offre" |
| **A**CKNOWLEDGE | Serveur | Unicast | "C'est confirmé, voici ta configuration complète" |

📌 **Moyen mnémotechnique :** **D**iscuter → **O**btenir → **R**épondre → **A**ccuser réception

---

### C. L'Étendue DHCP (Scope)

Une **étendue** est la plage d'adresses IP qu'un serveur DHCP peut distribuer sur un réseau donné. C'est le "stock" d'adresses disponibles.

```
┌──────────────────────────────────────────────────────────────────┐
│              ÉTENDUE DHCP — Réseau 192.168.1.0/24                │
│                                                                  │
│  Plage totale : 192.168.1.1 → 192.168.1.254                      │
│                                                                  │
│  [EXCLUES] [RÉSERVÉES] [DISPONIBLES POUR DISTRIBUTION]           │
│  .1 à .9    .50 à .60   .10 à .49  et  .61 à .200               │
│  (Routeur,  (Serveurs  (Distribuées aux clients à la demande)    │
│   Serveurs)  par MAC)                                            │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

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

💡 **Niveaux d'options :** Les options peuvent être définies à 3 niveaux, avec la priorité suivante (du plus général au plus spécifique) :

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

```
   Sous-réseau A (192.168.1.0/24)         Sous-réseau B (192.168.2.0/24)
   ┌────────────────────────┐              ┌────────────────────────────┐
   │  Client A              │              │  Client B                  │
   │  (sans IP)             │              │  (sans IP)                 │
   │       │                │    ROUTEUR   │      │                     │
   │  Broadcast DISCOVER ──►│◄────────────►│ Agent Relais ─────────►   │
   │                        │              │  (Unicast vers DHCP Srv)   │
   └────────────────────────┘              └────────────────────────────┘
                                                          │
                                                          ▼
                                          ┌──────────────────────────┐
                                          │   SERVEUR DHCP CENTRALISÉ│
                                          │   (192.168.1.10)         │
                                          │   Gère TOUTES les plages │
                                          └──────────────────────────┘
```

*Légende : Schéma montrant comment l'agent relais DHCP permet à un seul serveur DHCP centralisé (en bas) de servir deux sous-réseaux distincts séparés par un routeur. Sans l'agent relais, le broadcast du Client B ne dépasserait jamais la frontière du sous-réseau B.*

📌 **En résumé :** L'agent relais est la solution pour centraliser le service DHCP tout en desservant plusieurs sous-réseaux depuis un seul serveur. C'est une bonne pratique d'administration qui simplifie la gestion et réduit les coûts.

---

## II. Partage de Fichiers Windows : Droits NTFS vs Droits de Partage

### A. Pourquoi deux types de droits ?

Quand un utilisateur accède à un dossier partagé sur un serveur Windows, **deux filtres de sécurité** s'appliquent successivement :

1. **Les droits de partage** (Share Permissions) : contrôlent l'accès **via le réseau** uniquement
2. **Les droits NTFS** : contrôlent l'accès **au niveau du système de fichiers**, que l'on accède au dossier via le réseau ou localement

```
        UTILISATEUR RÉSEAU
               │
               ▼
   ┌─────────────────────────┐
   │   DROITS DE PARTAGE     │  ◄── Premier filtre (accès réseau)
   │  (Contrôle Total /      │
   │   Modifier / Lecture)   │
   └────────────┬────────────┘
                │  Si l'accès est accordé, le 2e filtre s'applique
                ▼
   ┌─────────────────────────┐
   │     DROITS NTFS         │  ◄── Deuxième filtre (système de fichiers)
   │  (Contrôle Total /      │
   │   Modifier / Lecture /  │
   │   Écriture / Exécuter…) │
   └────────────┬────────────┘
                │
                ▼
      ACCÈS EFFECTIF = Le plus restrictif des deux
```

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

📌 **Bonne pratique professionnelle :** En entreprise, on définit généralement le droit de partage au niveau **"Contrôle total"** pour le groupe *Tout le monde* ou *Utilisateurs authentifiés*, puis on **gère toute la sécurité uniquement via les droits NTFS**. Cela évite la double gestion et simplifie la maintenance.

---

### C. Les Niveaux de Droits NTFS

Les droits NTFS sont plus **granulaires** que les droits de partage. Les permissions standards les plus utilisées sont :

| **Permission NTFS** | **Lire** | **Lister** | **Écrire** | **Modifier** | **Supprimer** | **Changer droits** |
|--------------------|:--------:|:----------:|:----------:|:------------:|:-------------:|:-----------------:|
| Lecture | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Lecture et exécution | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Listage du dossier | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Écriture | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Modifier | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Contrôle total | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

💡 **Lien ITIL — Gestion des Accès :** Dans ITIL 4, la pratique de **Gestion des accès** vise à garantir que seules les personnes autorisées peuvent consulter ou modifier une information. Configurer correctement les droits NTFS par groupe d'utilisateurs (et non par utilisateur individuel) est une bonne pratique d'administration qui facilite la gestion des départs/arrivées dans l'entreprise.

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

📌 **Règle d'organisation :** Ne jamais accorder des droits directement à un utilisateur individuel. Toujours passer par des **groupes AD**. Ainsi, quand un collaborateur change de service ou quitte l'entreprise, il suffit de modifier son appartenance aux groupes, sans toucher aux droits des dossiers.

---

## III. DFS (Distributed File System) — Introduction

### A. Le Problème que DFS Résout

Imaginez que votre entreprise stocke ses fichiers sur le serveur `\\SERVEUR-FS01\Partages`. 120 utilisateurs ont ce chemin dans leurs raccourcis, scripts de connexion et favoris réseau.

Un jour, vous migrez les fichiers vers un nouveau serveur plus performant : `\\SERVEUR-FS02`. Résultat : **tous les 120 raccourcis sont cassés**. Il faut reconfigurer chaque poste — un travail colossal.

**DFS (Distributed File System)** résout ce problème en introduisant une **couche d'abstraction** entre les utilisateurs et les serveurs physiques.

---

### B. Le Principe des Espaces de Noms DFS

Avec DFS, les utilisateurs accèdent aux fichiers via un **espace de noms** (namespace) — un chemin "logique" qui ne change jamais — plutôt que directement via le chemin physique du serveur.

```
   UTILISATEURS                    ESPACE DE NOMS DFS
                                  (chemin logique stable)
   \\domaine\partages\RH    ──►   \\domaine\partages\      ──►  \\SERVEUR-FS01\RH
   \\domaine\partages\Compta ──►  (Serveur DFS = point    ──►  \\SERVEUR-FS01\Compta
   \\domaine\partages\IT     ──►   d'entrée unique)       ──►  \\SERVEUR-FS02\IT
                                                          ──►  \\NAS-01\Backups
```

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

💡 **Lien ITIL — Gestion de la Disponibilité :** Le DFS Namespaces est un excellent exemple de la pratique ITIL de gestion de la disponibilité : on conçoit le service de fichiers pour qu'il soit **résilient aux changements d'infrastructure** sans impacter l'expérience des utilisateurs.

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

## ✅ Auto-évaluation : Suis-je Prêt ?

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
