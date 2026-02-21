# Pack de Formation - Semaine 6 (S6) - BLOC 1
## 🎫 GLPI · Installation · Configuration · Lien OCS · TP Gestion de Tickets

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S6 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — sixième semaine |
| **Modalité** | Présentiel — salle TP (accès réseau, serveur GLPI disponible) |
| **Prérequis** | S3 (ITIL, tickets), S5 (OCS Inventory, inventaire automatisé) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.3** | Mettre en place et exploiter des outils de support et d'assistance | Maîtrise |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc | Maîtrise |
| **B1.6** | Assurer le support des utilisateurs | Acquisition |
| **B1.2** | Exploiter des référentiels, normes et standards (ITIL) | Maîtrise |

> 📌 **S6 est la séance de convergence du Bloc 1.** GLPI réunit tout ce qui a été vu depuis S2 : l'inventaire de parc (S2-S5), le vocabulaire ITIL (S3), le cycle de vie des incidents (S3-S4), et l'automatisation OCS (S5). C'est aussi l'outil que les apprenants utiliseront pour documenter les projets à venir. Il doit être installé et maîtrisé avant d'entrer dans le Bloc 2.

---

## Objectifs Pédagogiques

À l'issue de cette séance, l'apprenant sera capable de :

**Installation et configuration :**
- ✅ Décrire l'**architecture GLPI** (serveur web, base de données, PHP, agents)
- ✅ Naviguer dans l'interface GLPI et configurer les **catégories, priorités et SLA**
- ✅ Créer des **profils utilisateurs** (technicien, administrateur, utilisateur final)
- ✅ Configurer le **lien OCS → GLPI** via le plugin d'import

**Gestion des tickets :**
- ✅ Créer un ticket d'incident et un ticket de demande dans GLPI
- ✅ **Catégoriser, affecter, suivre, résoudre et clôturer** un ticket
- ✅ Lier un ticket à un **actif inventorié** (CI importé d'OCS)
- ✅ Consulter les **statistiques et tableaux de bord** GLPI

---

## ⭐ Spécificités Pédagogiques

### La Convergence Pédagogique de S6

S6 est le moment où les apprenants voient le "puzzle complet" du Bloc 1 :

```
S2 Fiche technique manuelle
         ↓
S5 OCS Inventory automatise l'inventaire
         ↓
S6 GLPI importe l'inventaire OCS ET gère les tickets
         ↓
Résultat : quand un technicien ouvre un ticket dans GLPI,
il peut le lier au CI du poste concerné — historique complet
```

Ce moment de convergence est pédagogiquement puissant. Le prendre le temps de le verbaliser explicitement en début de séance.

### Accès à GLPI

Trois configurations possibles selon l'infrastructure de l'établissement :

**Configuration A — Serveur GLPI de l'établissement**
GLPI installé sur un serveur dédié, accessible par tous les apprenants. Option idéale. L'enseignant crée les comptes en avance.

**Configuration B — VM individuelle**
Chaque apprenant installe GLPI sur sa propre VM Debian/Ubuntu. Plus de temps de mise en place (~30 min) mais meilleure maîtrise. Utiliser le script d'installation automatisé fourni en Annexe.

**Configuration C — GLPI Demo en ligne**
`demo.glpi-project.org` — instance de démonstration publique. Utilisable pour la navigation mais pas pour le TP complet (données effacées périodiquement, pas de lien OCS).

> **Recommandation :** Configuration A pour les établissements équipés. Configuration B pour les groupes avancés ou les séances avec temps disponible.

### Entretiens Individuels (Annoncés en S5)

S6 est la séance où l'enseignant conduit les **entretiens individuels de 5 minutes** annoncés en S5 suite à l'évaluation diagnostique. Pendant que les binômes travaillent en autonomie sur le TP GLPI, l'enseignant voit les apprenants un par un sur la base de leur grille d'auto-positionnement. Ces entretiens sont rapides et bienveillants : 2 points forts identifiés, 1 point de progression prioritaire.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Bilan S5 | Retour sur l'évaluation diagnostique — résultats globaux (anonymisés) |
| H+0:10 | 35 min | 📖 Cours | GLPI : présentation, architecture, interface, modules |
| H+0:45 | 20 min | 🔧 Démo | Démo live de GLPI par l'enseignant — tour complet de l'interface |
| H+1:05 | **15 min** | ☕ **PAUSE** | — |
| H+1:20 | 30 min | 🖥️ TP Part. 1 | Installation/configuration GLPI + lien OCS (ou navigation sur instance fournie) |
| H+1:50 | 75 min | 🖥️ TP Part. 2 | Gestion de tickets : cycle complet sur 4 scénarios |
| H+3:05 | 25 min | 🎤 Entretiens | Entretiens individuels (5 min/apprenant, pendant que les autres finissent le TP) |
| H+3:30 | 20 min | ✅ Correction | Correction collective — erreurs fréquentes, bonnes pratiques |
| H+3:50 | 10 min | 📁 Portfolio | Bilan Bloc 1 — ce qui entre dans le portfolio, transition vers Bloc 2 |

---

## Préparation Avant la Séance

| **Tâche** | **Détail** |
|---|---|
| **Serveur GLPI opérationnel** | Tester l'accès depuis la salle avant la séance |
| **Comptes créés** | 1 compte `admin` pour l'enseignant + 1 compte `technicien_[prénom]` par apprenant |
| **Plugin OCS Import installé** | Si lien OCS prévu — tester l'import en avance |
| **4 scénarios TP imprimés** | Un jeu par apprenant (ou accès numérique) |
| **Grilles d'entretien S5** | Récupérer les grilles d'auto-positionnement de chaque apprenant |

---

## Différenciation Pédagogique

### Profil Avancé
- Installer GLPI sur VM Debian depuis la ligne de commande uniquement (script Annexe A)
- Configurer un **SLA personnalisé** avec règles d'escalade automatiques
- Explorer le module **Règles métier** de GLPI pour l'attribution automatique de tickets
- Créer un **rapport GLPI personnalisé** sur les statistiques d'incidents de la séance

### Profil Débutant
- Utiliser l'instance GLPI pré-configurée fournie par l'enseignant
- Se concentrer sur le TP Part. 2 (tickets) — la configuration est optionnelle
- Travailler avec la fiche de navigation GLPI fournie en Annexe B

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **Instance GLPI accessible** | Selon configuration A/B/C |
| **URL + identifiants** | À distribuer en début de séance |
| **Script d'installation** | Annexe A — pour Configuration B |
| **Fiche navigation GLPI** | Annexe B — pour les débutants |
| **4 scénarios TP** | Intégrés dans ce pack |

---

## Lien avec le Référentiel Qualiopi

- ✅ Convergence des apprentissages S2-S5 dans un outil professionnel réel
- ✅ Mise en situation professionnelle complète (tickets de bout en bout)
- ✅ Entretiens individuels formalisés suite à l'évaluation diagnostique S5
- ✅ Première trace documentée de la maîtrise B1.3 et B1.4 pour le portfolio

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "GLPI — Gestion Libre de Parc Informatique"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 6*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.2** | Exploiter des référentiels et standards (ITIL dans GLPI) |
| **B1.3** | Mettre en place et exploiter des outils de support |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc |
| **B1.6** | Assurer le support des utilisateurs |

---

## PARTIE I — Présentation de GLPI

### I.A. Qu'est-ce que GLPI ?

**GLPI** (Gestion Libre de Parc Informatique) est un logiciel **ITSM** (IT Service Management) open source développé en PHP, très répandu en France. Il intègre dans un seul outil :

- La **gestion des tickets** (incidents, demandes, problèmes, changements)
- La **CMDB** (inventaire des actifs IT)
- La **base de connaissances** (solutions aux incidents récurrents)
- La **gestion des licences** logicielles
- La **planification** des maintenances et interventions
- Les **statistiques et rapports** de performance (SLA, MTTR...)

| **Paramètre** | **Valeur** |
|---|---|
| **Éditeur** | Teclib (France) + communauté open source |
| **Licence** | GPL v2 (gratuit) |
| **Langage** | PHP + MySQL/MariaDB |
| **Interface** | Web (navigateur) |
| **Plateformes** | Tout serveur Linux/Windows avec Apache/Nginx + PHP |
| **Utilisateurs** | + de 300 000 organisations dans le monde |
| **Intégration** | OCS Inventory, FusionInventory, LDAP/AD, SSO |

> 💡 **GLPI dans le monde professionnel :** GLPI est l'outil ITSM le plus déployé dans les collectivités territoriales, établissements d'enseignement, administrations et PME françaises. Il est mentionné dans de très nombreuses fiches de poste technicien / admin système. Le maîtriser vous différencie immédiatement.

---

### I.B. Architecture GLPI

```
   UTILISATEURS FINAUX        TECHNICIENS              ADMINISTRATEURS
   (créent des tickets        (traitent les tickets,   (configurent GLPI,
   via portail ou email)      accèdent à la CMDB)      gèrent les profils)
         │                          │                          │
         └──────────────────────────┴──────────────────────────┘
                                    │
                             HTTP / HTTPS
                                    │
                    ┌───────────────▼──────────────────┐
                    │         SERVEUR WEB               │
                    │    Apache / Nginx + PHP 8.x       │
                    │                                   │
                    │    ┌─────────────────────────┐   │
                    │    │    APPLICATION GLPI      │   │
                    │    │   (interface, logique,   │   │
                    │    │    règles, workflows)    │   │
                    │    └────────────┬────────────┘   │
                    └─────────────────┼────────────────┘
                                      │ SQL
                    ┌─────────────────▼────────────────┐
                    │         BASE DE DONNÉES           │
                    │       MySQL / MariaDB             │
                    │  (tickets, actifs, utilisateurs,  │
                    │   connaissances, historique...)   │
                    └──────────────────────────────────┘

   ───── ALIMENTATION AUTOMATIQUE DE LA CMDB ─────

   OCS Inventory ──── Plugin OCS Import ────►  GLPI CMDB
   (inventaire)                                (actifs liés aux tickets)

   FusionInventory ─── Agent natif GLPI ────►  GLPI CMDB
   (alternative OCS)
```

---

### I.C. La Place de GLPI dans l'Écosystème ITSM

```
   ┌─────────────────────────────────────────────────────────┐
   │                      GLPI                               │
   │                                                         │
   │  ┌──────────────┐    ┌─────────────┐   ┌───────────┐  │
   │  │   TICKETS     │    │    CMDB     │   │    KB     │  │
   │  │ Incidents     │◄──►│ Computers   │◄──│ Solutions │  │
   │  │ Demandes      │    │ Printers    │   │ Procédures│  │
   │  │ Problèmes     │    │ Network     │   └───────────┘  │
   │  │ Changements   │    │ Software    │                   │
   │  └──────────────┘    └──────┬──────┘   ┌───────────┐  │
   │                             │           │ LICENCES  │  │
   │                    ┌────────▼────┐      │ Audit     │  │
   │                    │ OCS Import  │      │ Expiration│  │
   │                    │  (plugin)   │      └───────────┘  │
   │                    └────────┬────┘                     │
   └─────────────────────────────┼───────────────────────────┘
                                  │
                        ┌─────────▼──────────┐
                        │   OCS Inventory    │
                        │   (agents postes)  │
                        └────────────────────┘
```

---

## PARTIE II — Interface GLPI : Les Modules Essentiels

### II.A. Menu Principal

```
   GLPI — Barre de navigation principale
   ─────────────────────────────────────────────────────────────────
   Accueil   Parc   Assistance   Gestion   Outils   Admin   Config
      │        │        │           │         │        │        │
      │        │        │           │         │        │        └─ Profils, LDAP,
      │        │        │           │         │        │           Plugins, Règles
      │        │        │           │         │        └─ Utilisateurs, Entités
      │        │        │           │         └─ Prise de notes, Tâches, Rapports
      │        │        │           └─ Fournisseurs, Contrats, Documents, Licences
      │        │        └─ Tickets ★, Problèmes, Changements, Planification
      │        └─ Ordinateurs ★, Moniteurs, Logiciels, Réseau,
      │           Périphériques, Imprimantes, Téléphones
      └─ Tableau de bord — Vue d'ensemble des tickets ouverts
```

### II.B. La Fiche d'un Ticket GLPI

Un ticket dans GLPI contient tous les champs que vous remplissez manuellement depuis S3 — mais dans une interface structurée et traçable :

| **Champ** | **Équivalent cours** | **Options GLPI** |
|---|---|---|
| **Titre** | Titre du ticket | Texte libre |
| **Type** | Incident / Demande | Incident / Demande de service |
| **Catégorie** | Domaine technique | Réseau, Système, Matériel, Logiciel, Sécurité... |
| **Demandeur** | Utilisateur concerné | Lié à l'annuaire GLPI / LDAP / AD |
| **Technicien affecté** | Niveau N1/N2 | Utilisateur ou groupe technique |
| **Priorité** | P1 à P4 | 1-Très haute à 5-Très basse (calculée automatiquement) |
| **Urgence + Impact** | Matrice S3 | Saisie séparée → priorité calculée |
| **Statut** | Étape du cycle | Nouveau → En cours → En attente → Résolu → Clôturé |
| **Description** | Description incident | Texte riche (images, fichiers joints) |
| **Suivi** | Actions N1 dans le ticket | Fils de messages (internes ou publics) |
| **Solution** | Résolution | Texte + lien KB optionnel |
| **CI lié** | Équipement concerné | Lié à un actif de la CMDB |
| **SLA** | Délai contractuel | Calculé automatiquement, alerte si dépassement |

---

### II.C. Cycle de Vie d'un Ticket dans GLPI

```
   STATUT          ACTION                    QUI
   ──────────────────────────────────────────────────────────────
   [ Nouveau ]  ← Ticket créé (portail, email, téléphone)
        │
        ▼
   [ En cours   ← Technicien s'affecte ou est affecté
     (attribué)]
        │
        ▼
   [ En cours   ← Technicien travaille sur la résolution
     (planifié)]
        │
        ├──► [ En attente ] ← En attente d'info utilisateur / fournisseur
        │         │
        │         └──► Retour à En cours dès réponse reçue
        │
        ▼
   [ Résolu   ] ← Solution saisie par le technicien
        │
        ▼
   [ Clôturé  ] ← Validé par l'utilisateur OU clôture automatique (ex. 72h)
```

> ⚠️ **Différence importante :** "Résolu" signifie que le technicien a appliqué une solution. "Clôturé" signifie que l'utilisateur a confirmé que la solution fonctionne. Un ticket peut rester en "Résolu" plusieurs jours si l'utilisateur n'a pas encore confirmé. La clôture automatique après X jours est configurable.

---

### II.D. Les Profils Utilisateurs dans GLPI

GLPI gère des **profils** (rôles) qui définissent ce que chaque type d'utilisateur peut voir et faire :

| **Profil** | **Accès** | **Peut faire** |
|---|---|---|
| **Super-Admin** | Tout | Configuration complète, tous les modules |
| **Admin** | Tout sauf configuration système | Gérer utilisateurs, profils, entités |
| **Technicien** | Assistance + Parc | Créer/traiter tickets, consulter CMDB |
| **Responsable** | Supervision | Voir statistiques, SLA, rapports |
| **Utilisateur final** | Portail uniquement | Créer tickets pour soi-même, consulter ses tickets |
| **Observateur** | Lecture seule | Voir sans modifier |

---

### II.E. Les Catégories de Tickets

Les **catégories** permettent de router automatiquement les tickets vers les bons groupes techniques et d'alimenter les statistiques par domaine. Une bonne arborescence de catégories est essentielle :

```
Exemple d'arborescence de catégories SimIO SARL :

├── Matériel
│   ├── Ordinateur (poste fixe)
│   ├── Laptop
│   ├── Imprimante
│   └── Périphérique
├── Logiciel
│   ├── Système d'exploitation
│   ├── Bureautique (Office)
│   ├── Métier (ERP, CRM...)
│   └── Sécurité (antivirus)
├── Réseau
│   ├── Connectivité (pas d'accès)
│   ├── Lenteur réseau
│   ├── WiFi
│   └── VPN
├── Accès et Comptes
│   ├── Mot de passe oublié / expiré
│   ├── Droits insuffisants
│   └── Création de compte
└── Autre
```

---

## PARTIE III — Le Lien OCS → GLPI

### III.A. Pourquoi Lier OCS et GLPI ?

Sans lien OCS-GLPI, les deux outils fonctionnent en silos :
- OCS a l'inventaire des postes
- GLPI a les tickets d'incidents

Avec le lien OCS-GLPI (via le plugin **OCS Inventory NG**) :
- Les postes inventoriés par OCS apparaissent automatiquement dans la CMDB GLPI
- Un ticket peut être lié au CI du poste concerné
- L'historique matériel du poste est visible depuis le ticket
- Les changements matériels détectés par OCS sont visibles dans GLPI

### III.B. Configuration du Plugin OCS Import

**Depuis GLPI (Administration → Plugins → OCS Inventory NG) :**

```
Étapes de configuration :

1. Renseigner l'URL du serveur OCS
   Serveur OCS : http://[IP_OCS]/ocsreports

2. Compte de connexion à la base OCS
   Login : glpi (compte SQL dédié, créé sur le serveur OCS)
   Password : [mot de passe]

3. Options d'import :
   ☑ Synchroniser les ordinateurs      → importer les postes OCS
   ☑ Mettre à jour automatiquement     → synchro lors des scans OCS
   ☑ Importer les logiciels            → liste logiciels dans CMDB

4. Tester la connexion
   → "Test de connexion" → attendu : "Connexion réussie"

5. Lancer l'import initial
   → "Synchroniser GLPI avec OCS" → les postes OCS apparaissent
     dans Parc → Ordinateurs
```

### III.C. Résultat dans GLPI

Après import OCS, chaque poste inventorié par OCS devient un **CI (Configuration Item)** dans GLPI :

```
GLPI → Parc → Ordinateurs → [Poste importé depuis OCS]

Informations disponibles :
├── Matériel (CPU, RAM, disque) ← vient d'OCS
├── Logiciels installés         ← vient d'OCS
├── Réseau (IP, MAC)            ← vient d'OCS
├── Tickets liés                ← ajoutés par les techniciens dans GLPI
├── Historique des modifications ← suivi automatique
└── Utilisateur affecté         ← configuré dans GLPI
```

---

## PARTIE IV — Statistiques et Tableaux de Bord

GLPI génère automatiquement des statistiques exploitables pour le reporting DSI :

| **Rapport** | **Contenu** | **Usage** |
|---|---|---|
| **Tickets par statut** | Volume Nouveau / En cours / Résolu / Clôturé | État des files d'attente |
| **MTTR moyen** | Temps de résolution par catégorie | Mesure d'efficacité |
| **Tickets par technicien** | Charge de travail individuelle | Management d'équipe |
| **Respect SLA** | % tickets traités dans les délais | Contractuel |
| **Tickets par catégorie** | Volume par domaine technique | Identifier les incidents récurrents |
| **Évolution mensuelle** | Tendance sur 12 mois | Pilotage long terme |

> 📌 **Lien avec l'E5 :** Savoir lire et commenter un tableau de bord GLPI est une compétence valorisable devant le jury E5. Un apprenant qui dit "j'ai configuré les SLA et généré les rapports mensuels pour la DSI" démontre B1.2 et B1.3 au niveau Maîtrise.

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **GLPI** | Gestion Libre de Parc Informatique — outil ITSM open source |
| **ITSM** | IT Service Management — ensemble des pratiques de gestion des services IT |
| **Plugin** | Extension ajoutant des fonctionnalités à GLPI |
| **OCS Import** | Plugin GLPI permettant d'importer l'inventaire OCS dans la CMDB |
| **Entité** | Unité organisationnelle dans GLPI (département, site, filiale) |
| **Profil** | Rôle définissant les droits d'accès d'un utilisateur dans GLPI |
| **Catégorie de ticket** | Classification du ticket par domaine technique |
| **Suivi** | Message ajouté à un ticket pour documenter l'avancement |
| **Solution** | Réponse finale ajoutée à un ticket pour le passer en "Résolu" |
| **CI (Configuration Item)** | Actif géré dans la CMDB — ordinateur, imprimante, serveur... |
| **SLA GLPI** | Service Level Agreement configuré dans GLPI — déclenche des alertes |
| **Règle métier** | Automatisation dans GLPI (ex : si catégorie = Réseau → affecter au groupe Réseau) |
| **Portail utilisateur** | Interface simplifiée GLPI pour les utilisateurs finaux |
| **FusionInventory** | Alternative à OCS — agent d'inventaire intégré nativement à GLPI |
| **Clôture automatique** | Mécanisme GLPI pour clôturer automatiquement les tickets résolus après X jours |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] J'explique l'architecture GLPI (serveur web + PHP + MySQL)
- [ ] Je navigue dans les menus principaux de GLPI
- [ ] Je crée un ticket avec tous les champs obligatoires
- [ ] Je catégorise et affecte un ticket à un technicien
- [ ] Je fais avancer le statut d'un ticket jusqu'à la clôture
- [ ] J'explique la différence entre "Résolu" et "Clôturé"
- [ ] Je sais configurer le lien OCS → GLPI via le plugin
- [ ] Je consulte les statistiques GLPI (MTTR, SLA)

---

---

# 🖥️ FICHE TP — GLPI : GESTION COMPLÈTE DE TICKETS

*Durée : 75 minutes — Individuel (ou binôme selon la configuration)*

---

## Connexion à GLPI

| **URL** | `http://[IP_SERVEUR]/glpi` |
|---|---|
| **Votre identifiant** | `technicien_[votre_prénom]` |
| **Mot de passe** | `Glpi2024!` (à changer dès la connexion) |

---

## PARTIE A — Prise en Main de l'Interface (10 min)

Avant de traiter les tickets, explorez GLPI pour vous repérer :

| **Tâche d'exploration** | **Menu** | **Fait ?** |
|---|---|---|
| Trouver la liste de tous les ordinateurs importés d'OCS | Parc → Ordinateurs | ☐ |
| Ouvrir la fiche d'un ordinateur et noter les informations matérielles | Parc → Ordinateurs → [Cliquer sur un poste] | ☐ |
| Consulter la liste des tickets ouverts | Assistance → Tickets | ☐ |
| Trouver les catégories de tickets configurées | Config → Intitulés → Catégories des tickets | ☐ |
| Localiser votre profil utilisateur | Prénom en haut à droite → Mon profil | ☐ |

**Note :** Combien d'ordinateurs sont importés d'OCS dans cette instance GLPI ? _______

---

## PARTIE B — Quatre Scénarios de Tickets (65 min)

*Traiter les 4 scénarios dans l'ordre. Chaque scénario simule une situation réelle.*

---

### 🎫 TICKET 1 — Création et Affectation (15 min)

**Rôle :** Vous jouez le technicien N1 qui reçoit un appel et crée le ticket.

**Appel reçu :**
> *"Bonjour, ici Sylvie Mercier du service Comptabilité. J'essaie d'imprimer ma déclaration TVA depuis ce matin mais l'imprimante réseau affiche 'Hors ligne' dans Windows. J'ai essayé de redémarrer l'imprimante et mon PC — ça ne change rien. J'ai besoin d'imprimer avant 11h pour la réunion."*

**Instructions :**

**Étape 1 — Créer le ticket**
- Aller dans : Assistance → Créer un ticket
- Remplir **tous** les champs obligatoires :

| **Champ** | **Valeur à saisir** |
|---|---|
| Type | Incident |
| Titre | |
| Catégorie | |
| Demandeur | Sylvie Mercier (ou créer l'utilisateur si absent) |
| Description | |
| Urgence | |
| Impact | |
| (Priorité calculée automatiquement) | |

**Étape 2 — Lier le ticket à un CI**
- Dans le ticket créé, aller dans l'onglet **Eléments**
- Cliquer sur "Ajouter un élément" → sélectionner un ordinateur ou une imprimante depuis la CMDB

**Étape 3 — Affecter le ticket**
- Onglet **Acteurs** du ticket
- Affecter à vous-même (technicien_[prénom])

**Étape 4 — Changer le statut**
- Passer le ticket de "Nouveau" à "En cours (attribué)"
- Ajouter un **suivi** (message interne) : "Prise en charge — diagnostic en cours"

**N° du ticket créé :** _______

---

### 🎫 TICKET 2 — Suivi et Escalade (15 min)

**Contexte :** Vous reprenez le Ticket 1. Votre diagnostic N1 indique que le problème vient du serveur d'impression — c'est hors de votre périmètre N1.

**Instructions :**

**Étape 1 — Documenter le diagnostic N1 dans le suivi**
- Ouvrir le Ticket 1
- Ajouter un **suivi public** (visible par l'utilisateur) avec :
  - Ce que vous avez vérifié (physique, état Windows, tentatives)
  - Pourquoi vous escaladez

**Étape 2 — Escalader vers N2**
- Dans l'onglet Acteurs → changer le technicien affecté pour le groupe "Techniciens N2 Système" (ou un autre technicien si le groupe n'existe pas)
- Ajouter un **suivi interne** (non visible utilisateur) : motif de l'escalade

**Étape 3 — Informer l'utilisateur**
- Ajouter un **suivi public** : "Votre incident est pris en charge et en cours de résolution par notre équipe spécialisée. Vous serez informé(e) dès la résolution."

**Étape 4 — Vérifier le SLA**
- Dans l'onglet du ticket, le temps restant avant dépassement SLA est-il affiché ?
- Valeur observée : _______

---

### 🎫 TICKET 3 — Demande de Service + Résolution (20 min)

**Rôle :** Vous créez ET résolvez un ticket de demande de service du début à la fin.

**Demande reçue par email :**
> *"Bonjour équipe IT, je suis Karim Benali, nouveau dans le service Marketing depuis lundi. Mon manager m'a dit de contacter le service IT pour avoir accès au dossier partagé Marketing sur le serveur. J'ai essayé d'y accéder hier mais j'obtiens 'Accès refusé'. Merci d'avance."*

**Instructions :**

**Étape 1 — Créer le ticket**

| **Champ** | **Valeur** |
|---|---|
| Type | Demande de service |
| Titre | |
| Catégorie | Accès et Comptes → Droits insuffisants |
| Demandeur | Karim Benali |
| Description | (reformulation professionnelle de l'email) |
| Urgence / Impact | |

**Étape 2 — Traiter la demande (simulation)**

Simuler les actions suivantes et les documenter dans le suivi :
1. Vérifier que Karim Benali a bien un compte AD actif
2. Vérifier son appartenance au groupe `GRP_MARKETING`
3. Ajouter Karim au groupe si absent (ou documenter l'action à effectuer)
4. Vérifier les droits NTFS du dossier Marketing pour `GRP_MARKETING`

Ajouter un **suivi interne** pour chacune des étapes ci-dessus.

**Étape 3 — Rédiger la solution et passer en Résolu**
- Onglet **Solution** du ticket
- Rédiger la solution complète en 3 à 5 lignes
- Passer le statut à **Résolu**

**Étape 4 — Lier à la base de connaissances**
- Si votre GLPI dispose de la KB, créer une **fiche KB** depuis la solution
- Titre KB : "Accès refusé dossier partagé — utilisateur non membre du groupe AD"

**N° du ticket :** _______ **Heure de clôture :** _______

---

### 🎫 TICKET 4 — Clôture + Tableau de Bord (15 min)

**Contexte :** L'utilisateur du Ticket 3 a rappelé pour confirmer que l'accès fonctionne. Vous clôturez le ticket et consultez les statistiques.

**Étape 1 — Clôturer le ticket**
- Ajouter un **suivi public** de confirmation : "L'accès au dossier partagé Marketing a été rétabli. N'hésitez pas à nous contacter si le problème réapparaît."
- Passer le statut de "Résolu" à **Clôturé**

**Étape 2 — Calculer le MTTR**

| **Information** | **Valeur** |
|---|---|
| Date/heure d'ouverture du ticket | |
| Date/heure de clôture | |
| MTTR calculé | min |

**Étape 3 — Consulter les statistiques**
- Aller dans : Assistance → Statistiques → Vue globale

Remplir le tableau :

| **Statistique** | **Valeur** |
|---|---|
| Nombre total de tickets ouverts sur l'instance | |
| Nombre de tickets en statut "Nouveau" | |
| Nombre de tickets en statut "Résolu" | |
| Catégorie avec le plus de tickets | |

**Étape 4 — Créer un rapport**
- Assistance → Statistiques → Tickets
- Filtrer par votre nom de technicien
- Quelle est votre MTTR moyen sur les tickets traités aujourd'hui ? _______

---

## Bilan du TP

| **Ticket** | **N°** | **Type** | **Statut final** | **MTTR** |
|---|---|---|---|---|
| Imprimante hors ligne | | Incident | | min |
| Escalade N2 (ticket 1) | même | Incident | Escaladé | — |
| Accès dossier Karim | | Demande | Clôturé | min |
| Clôture confirmée | même | Demande | Clôturé | — |

---

## Questions de Réflexion

**Q1.** Quelle différence avez-vous observée entre la création d'un ticket à la main (S3) et la création dans GLPI ? Citez 2 avantages concrets de GLPI.
```
Avantage 1 : ___________________________________________________________
Avantage 2 : ___________________________________________________________
```

**Q2.** Un ticket est passé en "Résolu" depuis 3 jours mais l'utilisateur n'a pas confirmé. Que devrait faire GLPI automatiquement ? Comment configurer ce comportement ?
```
_______________________________________________________________________
_______________________________________________________________________
```

**Q3.** Pourquoi est-il important de distinguer un suivi "interne" d'un suivi "public" dans GLPI ?
```
_______________________________________________________________________
_______________________________________________________________________
```

**Q4.** Vous êtes responsable IT d'une PME de 80 personnes. En regardant les statistiques GLPI, vous constatez que 40% des tickets concernent "Mot de passe oublié". Quelles solutions proposez-vous pour réduire ce volume ?
```
Solution 1 : ___________________________________________________________
Solution 2 : ___________________________________________________________
```

---

---

# 📁 ANNEXE A — SCRIPT D'INSTALLATION GLPI SUR DEBIAN

*Pour les apprenants profil avancé — Configuration B*

*Testé sur Debian 12 Bookworm / Ubuntu 22.04 LTS*

```bash
#!/bin/bash
# ─── INSTALLATION GLPI 10.x SUR DEBIAN/UBUNTU ────────────────────────
# Usage : sudo bash install_glpi.sh
# Durée : ~15 minutes selon la connexion

set -e  # Arrêt en cas d'erreur

echo "=== [1/6] Mise à jour du système ==="
apt update && apt upgrade -y

echo "=== [2/6] Installation Apache, PHP et extensions ==="
apt install -y apache2 php php-mysql php-curl php-gd php-intl \
  php-ldap php-mbstring php-xml php-zip php-bz2 php-imap \
  libapache2-mod-php mariadb-server

echo "=== [3/6] Sécurisation MariaDB ==="
# Répondre aux questions : root pw, suppr anonymous, désactiver root remote, etc.
mysql_secure_installation

echo "=== [4/6] Création de la base de données GLPI ==="
mysql -u root -p <<EOF
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'GlpiPass2024!';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
EOF

echo "=== [5/6] Téléchargement et décompression de GLPI ==="
cd /var/www/html
GLPI_VERSION="10.0.15"
wget -q "https://github.com/glpi-project/glpi/releases/download/${GLPI_VERSION}/glpi-${GLPI_VERSION}.tgz"
tar -xzf "glpi-${GLPI_VERSION}.tgz"
rm "glpi-${GLPI_VERSION}.tgz"
chown -R www-data:www-data glpi/
chmod -R 755 glpi/

echo "=== [6/6] Configuration Apache ==="
cat > /etc/apache2/sites-available/glpi.conf <<'APACHECONF'
<VirtualHost *:80>
    ServerName glpi.local
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Require all granted
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/glpi_error.log
    CustomLog ${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
APACHECONF

a2ensite glpi.conf
a2enmod rewrite
a2dissite 000-default.conf
systemctl restart apache2

echo ""
echo "════════════════════════════════════════════════════════"
echo " GLPI installé ! Finaliser via le navigateur :"
echo " http://[IP_DU_SERVEUR]/glpi"
echo " Base de données : glpi / GlpiPass2024! / localhost"
echo " Identifiants par défaut : glpi / glpi"
echo "════════════════════════════════════════════════════════"
```

**Post-installation (dans le navigateur) :**
1. Aller sur `http://[IP]/glpi`
2. Choisir la langue → Suivant
3. Accepter la licence → Continuer
4. Cliquer "Installer" (pas "Mettre à jour")
5. Renseigner les paramètres de base de données : `localhost` / `glpi` / `GlpiPass2024!`
6. Finaliser — noter les identifiants affichés
7. **IMPORTANT :** supprimer le dossier d'installation : `rm -rf /var/www/html/glpi/install`

---

# 📄 ANNEXE B — FICHE DE NAVIGATION RAPIDE GLPI

*Pour les apprenants débutants — À conserver*

```
╔══════════════════════════════════════════════════════════════╗
║              NAVIGATION RAPIDE GLPI                         ║
╠══════════════════════════════════════════════════════════════╣
║  CRÉER UN TICKET                                            ║
║  Assistance → + (bouton vert) → Créer un ticket             ║
║                                                              ║
║  VOIR MES TICKETS                                           ║
║  Assistance → Tickets → (filtre : Technicien = moi)         ║
║                                                              ║
║  CHANGER LE STATUT D'UN TICKET                              ║
║  Ouvrir le ticket → En-tête → Statut → Sélectionner         ║
║                                                              ║
║  AJOUTER UN SUIVI (commentaire)                             ║
║  Ouvrir le ticket → Onglet "Suivi" → Ajouter un suivi       ║
║  ☑ Privé = interne (non visible utilisateur)                 ║
║  ☐ Privé = public (visible utilisateur)                      ║
║                                                              ║
║  RÉDIGER LA SOLUTION (passer en Résolu)                     ║
║  Ouvrir le ticket → Onglet "Solution" → Saisir + Valider     ║
║                                                              ║
║  CLÔTURER UN TICKET                                         ║
║  Ouvrir un ticket Résolu → Statut → Clôturé                  ║
║                                                              ║
║  LIER UN CI (équipement) À UN TICKET                        ║
║  Ouvrir le ticket → Onglet "Eléments" → Ajouter             ║
║                                                              ║
║  VOIR LES STATISTIQUES                                      ║
║  Assistance → Statistiques → Vue globale                     ║
╚══════════════════════════════════════════════════════════════╝
```

---

# 📊 BILAN BLOC 1 — CE QUI ENTRE DANS LE PORTFOLIO

*À compléter individuellement en fin de S6*

Le Bloc 1 (S1-S6) vous a permis de produire plusieurs livrables qui peuvent entrer dans votre portfolio E5 :

| **Livrable** | **Produit en** | **Compétence** | **Dans mon portfolio ?** |
|---|---|---|---|
| Fiche technique du poste de TP | S2 | B1.1 | ☐ Oui / ☐ À améliorer |
| Rapport OCS comparé à la fiche | S5 | B1.4 | ☐ Oui / ☐ À améliorer |
| 3 tickets d'incidents résolus (S4) | S4 | B1.6 | ☐ Oui / ☐ À améliorer |
| 3 fiches KB produites (S4) | S4 | B1.3 | ☐ Oui / ☐ À améliorer |
| Tickets GLPI traités (S6) | S6 | B1.3, B1.4 | ☐ Oui / ☐ À améliorer |

**Pour transformer ces livrables en SPS E5 :**
La SPS doit contenir : contexte + mission + réalisation + preuves (captures) + compétences mobilisées + ce que j'ai appris. Si vous avez ces 6 éléments, votre livrable est une SPS exploitable.

**Mon plan pour la SPS Bloc 1 :**
```
Titre envisagé : _______________________________________________________
Compétences couvertes : ________________________________________________
Preuves disponibles : __________________________________________________
Ce que j'ai appris : ___________________________________________________
```

---

*Pack S6 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.2, B1.3, B1.4, B1.6*
*GLPI · Architecture · Tickets · Lien OCS · Statistiques · Bilan Bloc 1*
