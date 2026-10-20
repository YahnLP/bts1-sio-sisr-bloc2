# Cours – Stratégies de groupe (GPO) et DNS intégré à AD

*BTS SIO SISR 1re année – Bloc 2 – S11 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S10.
> - **Matériel :** VM Windows Server (domaine S10), VM cliente.

## À la fin de ce cours, tu sauras

- [ ] **Expliquer** le rôle des GPO et l'ordre d'application (LSDOU) ;
- [ ] **Créer** et **lier** une GPO avec la console de gestion ;
- [ ] **Configurer** une GPO de sécurité et une GPO d'environnement ;
- [ ] **Forcer** et **diagnostiquer** avec `gpupdate /force` et `gpresult /r` ;
- [ ] **Gérer** une zone DNS intégrée à AD (enregistrements A, CNAME, zone inverse).

---

## FICHE DE COURS ÉLÈVE
### "Stratégies de Groupe (GPO) et DNS Windows AD-Intégré"

*Version 1.0 – BTS SIO SISR – *

---

### I. Les Stratégies de Groupe (GPO)

#### A. Définition et Rôle

Une **GPO** (Group Policy Object – Objet de Stratégie de Groupe) est un ensemble de **paramètres de configuration** appliqués automatiquement à des ordinateurs ou des utilisateurs membres d'un domaine Active Directory.

**Ce qu'une GPO peut faire :**

| **Catégorie** | **Exemples de paramètres** |
|---------------|---------------------------|
| **Sécurité** | Longueur minimale du mot de passe, verrouillage de compte, audit des connexions |
| **Interface** | Fond d'écran imposé, désactiver l'écran de veille, masquer des icônes bureau |
| **Restrictions** | Interdire l'installation de logiciels, bloquer l'accès au Panneau de config. |
| **Réseau** | Mapper automatiquement des lecteurs réseau, configurer les imprimantes |
| **Scripts** | Exécuter un script à l'ouverture/fermeture de session ou au démarrage/arrêt |
| **Mises à jour** | Configurer Windows Update (WSUS) |
| **Logiciels** | Déployer automatiquement des logiciels MSI |
| **Internet Explorer / Edge** | Page d'accueil imposée, liste de sites autorisés/bloqués |

---

#### B. Architecture GPO : LSDOU

Les GPO s'appliquent dans un ordre précis, du **plus général au plus spécifique** :

```
Ordre d'application :
1.  LOCAL       → Stratégie locale (gpedit.msc sur le PC, sans domaine)
2.  SITE        → Stratégies liées au site AD géographique
3.  DOMAINE     → Stratégies liées au domaine (s'appliquent à tous)
4.  OU          → Stratégies liées à l'OU contenant l'objet
         └──  OU enfant → (plus spécifique = dernier appliqué)

LSDOU = L ocal → S ite → D omaine → OU
```

**Règle de priorité :**
- En cas de **conflit** entre deux GPO, la **dernière appliquée** gagne
- OU enfant > OU parent > Domaine > Site > Local
- **Paramètre "Non configuré"** dans une GPO = la GPO ne touche pas ce paramètre, l'héritage s'applique
- **Paramètre "Désactivé"** = la GPO force ce paramètre à être désactivé

---

#### C. Héritage des GPO

Par défaut, les GPO liées à une OU parent s'appliquent aussi aux OU enfants (**héritage**).

```
Domaine techpro.local
  └── GPO_Domaine (fond d'écran entreprise)
        │
        ├── OU_UTILISATEURS
        │     └── GPO_Utilisateurs (MDP 10 caract.)
        │           │
        │           └── OU Commercial
        │                 └── GPO_Commercial (MDP 8 caract.)
        │
        └── OU Directeurs
              └── (Hérite GPO_Domaine + GPO_Utilisateurs)
```

**Mécanismes pour modifier l'héritage :**

| **Mécanisme** | **Effet** | **Qui peut l'appliquer ?** |
|---------------|-----------|--------------------------|
| **Enforced** (Forcer) | La GPO s'applique quoi qu'il arrive, même si une OU enfant dit le contraire | Administrateur GPO |
| **Block Inheritance** (Bloquer l'héritage) | L'OU n'hérite plus des GPO parentes | Admin de l'OU |
| **Enforced > Block Inheritance** | Si une GPO est "Enforced", le "Block Inheritance" ne peut pas la bloquer | – |

---

#### D. Filtrage des GPO

Par défaut, une GPO liée à une OU s'applique à **tous les objets** de cette OU. On peut **filtrer** son application :

##### Filtrage par groupe de sécurité

Dans les propriétés de la GPO → onglet **Délégation** / **Sécurité** :
- La GPO s'applique par défaut aux objets ayant les droits `Lire` + `Appliquer la stratégie de groupe`
- Retirer `Appliquer` sur un groupe = les membres de ce groupe ne reçoivent pas la GPO
- Ajouter un groupe avec `Appliquer` = filtrer la GPO uniquement sur ce groupe

**Exemple :** GPO `GPO_Bureau_Restreint` liée à `OU_UTILISATEURS`, mais on veut qu'elle ne s'applique qu'aux commerciaux :
1. Retirer `Authenticated Users` de la liste
2. Ajouter `GRP_Commercial` avec `Lire` + `Appliquer`

---

#### E. Structure d'une GPO

Chaque GPO est divisée en deux parties :

```
GPO
├── Configuration Ordinateur
│   → S'applique à l'objet Ordinateur au démarrage du PC
│   ├── Paramètres logiciels (déploiement de logiciels MSI)
│   ├── Paramètres Windows
│   │   ├── Scripts (démarrage/arrêt)
│   │   └── Paramètres de sécurité
│   │       ├── Stratégies de comptes (MDP, verrouillage)
│   │       ├── Stratégies locales (audit, droits utilisateurs)
│   │       └── Pare-feu Windows
│   └── Modèles d'administration
│       ├── Composants Windows
│       ├── Bureau
│       └── Réseau
│
└── Configuration Utilisateur
    → S'applique à l'objet Utilisateur à l'ouverture de session
    ├── Paramètres logiciels
    ├── Paramètres Windows
    │   ├── Scripts (connexion/déconnexion)
    │   └── Paramètres Internet Explorer
    └── Modèles d'administration
        ├── Bureau
        ├── Panneau de configuration
        └── Menu Démarrer et barre des tâches
```

---

#### F. La Console GPMC (Group Policy Management Console)

La console centrale pour gérer les GPO dans un domaine.

**Accès :**
- Gestionnaire de serveur → Outils → Gestion des stratégies de groupe
- Ou `Win+R` → `gpmc.msc`

**Interface GPMC :**
```
Gestion des stratégies de groupe
└── Forêt : techpro.local
    └── Domaines
        └── techpro.local
            ├── Default Domain Policy     ← Ne pas modifier !
            ├── Default DC Policy         ← Ne pas modifier !
            ├── OU_UTILISATEURS
            │   └── [GPO liées à cette OU]
            └── Objets de stratégie de groupe
                └── [Toutes les GPO du domaine]
```

**Création d'une GPO (bonne pratique) :**
1. Clic droit sur `Objets de stratégie de groupe` → `Nouveau` → nommer la GPO
2. Modifier la GPO (clic droit → Modifier) → Configurer les paramètres dans l'éditeur
3. Lier la GPO à une OU (clic droit sur l'OU → `Lier un objet de stratégie de groupe existant`)

**Avantage :** La GPO est créée indépendamment du lien, et peut être liée à plusieurs OU.

---

#### G. Diagnostiquer les GPO

**Depuis un poste client (ou le DC) :**

```cmd
:: Forcer l'application immédiate des GPO
gpupdate /force

:: Forcer + déconnexion (pour scripts de connexion, profils)
gpupdate /force /logoff

:: Afficher les GPO appliquées à l'utilisateur ET l'ordinateur courant
gpresult /r

:: Rapport HTML détaillé (recommandé pour le diagnostic)
gpresult /h C:\rapport_gpo.html
```

**Lecture de `gpresult /r` :**
```
Résultats du jeu de stratégies résultant pour :
  NomOrdinateur\Utilisateur
  
RÉSULTATS DE LA STRATÉGIE POUR L'ORDINATEUR
  Nom de site       : Default-First-Site-Name
  CN                : SRV-AD-TECHPRO
  ...
  Les GPO suivantes ont été appliquées :
    Default Domain Policy          Lien : techpro.local
    GPO_Securite_MDP               Lien : OU_UTILISATEURS

  Les GPO suivantes n'ont pas été appliquées :
    GPO_Bureau_Direction  Raison : Le filtre de sécurité est absent...
```

**Via GPMC — Modélisation :**
GPMC → clic droit sur `Modélisation de stratégie de groupe` → simuler l'application des GPO sur un utilisateur/PC sans les appliquer réellement (idéal pour tester avant déploiement).

---

### II. DNS sous Windows Server – Zones AD-Intégrées

#### A. DNS Windows : Rappel et Nouveautés

Nous avons vu le DNS avec **Bind9** sous Linux (S08). Windows Server dispose de son propre serveur DNS intégré au rôle **DNS Server**.

Lors de la **promotion en contrôleur de domaine** (S10), le rôle DNS est installé et configuré automatiquement pour le domaine AD (ex : `techpro.local`).

**Différence clé avec Bind9 :**
- Bind9 : zones stockées dans des **fichiers texte** (`/etc/bind/db.techpro.local`)
- DNS Windows : zones stockées soit en **fichiers** (comme Bind9), soit dans **Active Directory** (zones AD-intégrées)

---

#### B. Types de Zones DNS Windows

| **Type** | **Stockage** | **Avantages** | **Inconvénients** |
|----------|-------------|---------------|-------------------|
| **Zone primaire** (standard) | Fichier `.dns` sur le serveur | Simple, compatible tout DNS | Un seul maître d'écriture, pas de réplication AD |
| **Zone secondaire** (standard) | Fichier en lecture seule | Redondance, lecture seule | Transfert de zone à configurer manuellement |
| **Zone de stub** | Pointeurs NS uniquement | Résolution inter-domaines | Limité |
| **Zone AD-intégrée** | Base de données Active Directory | Réplication automatique, sécurité renforcée, mises à jour dynamiques sécurisées | Nécessite un DC |

---

#### C. Zones AD-Intégrées : Avantages

**1. Réplication automatique**
Les enregistrements DNS sont stockés dans la base AD et répliqués automatiquement sur **tous les contrôleurs de domaine** du domaine ou de la forêt. Plus besoin de configurer les transferts de zone manuellement.

**2. Mises à jour dynamiques sécurisées (Secure Dynamic Update)**
Les clients Windows peuvent enregistrer automatiquement leur IP dans le DNS (au démarrage). En mode sécurisé, seule la machine elle-même peut mettre à jour son enregistrement DNS — évite les usurpations.

**3. Résistance à la panne**
Si un DC tombe, les autres DC ont une copie complète de la zone DNS. Le DNS continue de fonctionner.

**4. Gestion via MMC**
La console DNS intégrée à Windows Server est graphique, accessible depuis `dnsmgmt.msc`.

---

#### D. Étendue de Réplication AD-Intégrée

Quand on crée une zone AD-intégrée, on choisit l'étendue de réplication :

| **Étendue** | **Réplication vers…** | **Usage recommandé** |
|-------------|----------------------|----------------------|
| **Tous les contrôleurs DNS du domaine** | Tous les DC qui ont le rôle DNS dans le domaine | Recommandé (standard) |
| **Tous les contrôleurs du domaine** | Tous les DC du domaine, même sans rôle DNS | Rarement utile |
| **Tous les contrôleurs DNS de la forêt** | Tous les DC DNS de toute la forêt | Multi-domaines |

---

#### E. Gérer le DNS Windows : Console DNS

**Accès :**
- `Win+R` → `dnsmgmt.msc`
- Gestionnaire de serveur → Outils → DNS

**Structure de la console :**
```
DNS
└── SRV-AD-TECHPRO
    ├── Zones de recherche directe
    │   ├── techpro.local (zone AD-intégrée)
    │   │   ├── (Enregistrements SOA, NS)
    │   │   ├── (Enregistrements A créés automatiquement par AD)
    │   │   ├── www      → A → 192.168.50.100
    │   │   └── intranet → CNAME → www
    │   └── _msdcs.techpro.local (zone interne AD, ne pas toucher)
    │
    └── Zones de recherche inversée
        └── 50.168.192.in-addr.arpa (à créer)
```

---

#### F. Enregistrements DNS Automatiques (AD)

Quand AD est installé, Windows crée automatiquement des **enregistrements SRV** dans la zone DNS. Ces enregistrements permettent aux clients de **localiser les services AD** (contrôleurs de domaine, Kerberos, LDAP…).

**Exemples d'enregistrements SRV auto-créés :**
```
_kerberos._tcp.techpro.local    SRV 0 100 88  SRV-AD-TECHPRO.techpro.local.
_ldap._tcp.techpro.local        SRV 0 100 389 SRV-AD-TECHPRO.techpro.local.
_gc._tcp.techpro.local          SRV 0 100 3268 SRV-AD-TECHPRO.techpro.local.
```

**Ne jamais supprimer ces enregistrements SRV** — le domaine AD ne fonctionnerait plus.

---

#### G. Mises à Jour Dynamiques

Le DNS Windows peut accepter que les clients enregistrent automatiquement leur IP :

- **Non sécurisées** : N'importe qui peut ajouter/modifier un enregistrement (dangereux)
- **Sécurisées** : Seuls les ordinateurs membres du domaine peuvent mettre à jour leur propre enregistrement (recommandé)
- **Aucune** : Pas de mise à jour dynamique (enregistrements manuels uniquement)

**Configuration :** Clic droit sur la zone → `Propriétés` → `Mises à jour dynamiques` → `Sécurisées uniquement`

---

### III. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **GPO** | Group Policy Object – ensemble de paramètres appliqués automatiquement à des ordinateurs ou utilisateurs du domaine |
| **GPMC** | Group Policy Management Console – console MMC de gestion des GPO |
| **gpedit.msc** | Éditeur de stratégie de groupe locale (sans domaine) |
| **gpmc.msc** | Console GPMC (avec domaine) |
| **LSDOU** | Ordre d'application des GPO : Local → Site → Domaine → OU |
| **Héritage GPO** | Les GPO liées à une OU parent s'appliquent automatiquement aux OU enfants |
| **Enforced** | Option sur une GPO : force son application même si une OU enfant la contredit |
| **Block Inheritance** | Option sur une OU : empêche l'héritage des GPO parentes |
| **gpupdate /force** | Commande Windows forçant l'application immédiate des GPO |
| **gpresult /r** | Commande affichant les GPO appliquées à l'utilisateur et l'ordinateur courant |
| **Non configuré** | Paramètre GPO neutre : la GPO ne touche pas ce paramètre |
| **Désactivé** | Paramètre GPO forçant la désactivation d'une fonction |
| **Configuration Ordinateur** | Section d'une GPO appliquée au démarrage du PC |
| **Configuration Utilisateur** | Section d'une GPO appliquée à l'ouverture de session |
| **Filtrage GPO** | Limitation de l'application d'une GPO à certains objets/groupes |
| **Fine-Grained Password Policy** | Politique de mot de passe affinée (différente par groupe d'utilisateurs) |
| **Zone AD-intégrée** | Zone DNS stockée dans la base AD, répliquée automatiquement entre DC |
| **Mise à jour dynamique sécurisée** | Enregistrement automatique d'IP par les clients membres du domaine uniquement |
| **dnsmgmt.msc** | Console de gestion DNS Windows |
| **Enregistrement SRV** | Enregistrement DNS indiquant l'emplacement d'un service réseau (LDAP, Kerberos…) |

---

### IV. Exercices d'Entraînement

#### Exercice 1 : Ordre d'Application LSDOU

Un PC `PC-INFO-01` appartient à `OU Informatique` dans `OU_UTILISATEURS`. L'utilisateur `l.martin` se connecte.

Les GPO suivantes sont liées :

| **Niveau** | **GPO** | **Paramètre** | **Valeur** |
|------------|---------|---------------|------------|
| Domaine | GPO_Domaine | MDP min | 8 caract. |
| Domaine | GPO_Domaine | Fond d'écran | logo.jpg |
| OU_UTILISATEURS | GPO_Users | MDP min | 10 caract. |
| OU_UTILISATEURS | GPO_Users | Lecteur H: | \\SRV-AD\Users\%username% |
| OU Informatique | GPO_Info | MDP min | 12 caract. |
| OU Informatique | GPO_Info | Installation logiciels | Autorisée |

1. Quelle valeur finale de MDP minimum s'applique ? Pourquoi ?
2. L'utilisateur a-t-il le lecteur H: ? Pourquoi ?
3. L'utilisateur peut-il installer des logiciels ? Pourquoi ?
4. Quel fond d'écran s'applique ?
5. Si la GPO_Domaine est marquée "Enforced" sur le paramètre MDP, quelle valeur s'applique ?

---

#### Exercice 2 : Identifier le Problème

Un administrateur crée une GPO `GPO_Fond_Ecran` et la lie à `OU Commercial`. Après `gpupdate /force`, l'utilisateur `s.leroy` n'a toujours pas le nouveau fond d'écran.

Proposer **5 causes possibles** et la vérification à faire pour chacune.

---

#### Exercice 3 : Zone DNS

1. Quelle est la différence entre une zone DNS AD-intégrée et une zone DNS standard (fichier) ?
2. Pourquoi Windows crée-t-il automatiquement des enregistrements SRV dans la zone DNS du domaine ?
3. Un client W10 joint au domaine devrait enregistrer automatiquement son IP dans le DNS. Quel type de mise à jour dynamique activer ? Pourquoi ?

---

### V. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer l'ordre LSDOU et pourquoi l'OU gagne sur le domaine
- [ ] Distinguer "Non configuré", "Activé" et "Désactivé" dans une GPO
- [ ] Expliquer le principe de l'héritage GPO
- [ ] Décrire les effets de "Enforced" et "Block Inheritance"
- [ ] Ouvrir la console GPMC (`gpmc.msc`)
- [ ] Créer une GPO et la lier à une OU
- [ ] Configurer une politique de mots de passe dans une GPO
- [ ] Configurer un fond d'écran imposé dans une GPO
- [ ] Mapper un lecteur réseau via GPO
- [ ] Utiliser `gpupdate /force` et interpréter `gpresult /r`
- [ ] Expliquer l'avantage d'une zone DNS AD-intégrée
- [ ] Créer une zone DNS AD-intégrée dans la console DNS
- [ ] Ajouter des enregistrements A, CNAME dans la console DNS Windows
- [ ] Tester la résolution avec `nslookup` et `Resolve-DnsName`
