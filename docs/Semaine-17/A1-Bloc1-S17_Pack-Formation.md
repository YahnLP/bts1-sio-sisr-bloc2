# Pack de Formation - Semaine 17 (S17) - BLOC 1
## ⭐ PROJET 1 (Début) · GLPI · OCS Inventory · Catalogue de Services · Support

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S17 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — dix-septième semaine |
| **Modalité** | Présentiel — salle TP + travail en équipe |
| **Prérequis** | GLPI/OCS (S5-S6), ITIL (S3-S4), Wiki (S16), Active Directory (S11-S12 BLOC 2) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc | Maîtrise |
| **B1.5** | Mettre à disposition des utilisateurs un service informatique | Maîtrise |
| **B1.2** | Exploiter des référentiels, normes et standards (ITIL) | Maîtrise |
| **B3.3** | Participer à la gestion et au suivi d'un projet | Maîtrise |

> 📌 **S17-S18 BLOC 1 constituent le PROJET 1** — un projet de synthèse multi-blocs étalé sur 2 semaines. S17 BLOC 1 pose les fondations du système de support (GLPI, OCS, catalogue de services) qui seront intégrées à l'infrastructure technique déployée par les autres blocs. C'est la **première expérience projet** des apprenants en Année 1.

---

## Objectifs Pédagogiques

**Projet 1 (vue d'ensemble) :**
- ✅ Comprendre la structure d'un **projet multi-blocs** (infrastructure + support + sécurité)
- ✅ Collaborer en équipe sur un projet commun
- ✅ Documenter son travail dans le wiki d'équipe (S16)

**GLPI et OCS Inventory :**
- ✅ Installer et configurer **GLPI** (version récente)
- ✅ Installer et configurer **OCS Inventory Server**
- ✅ Déployer l'**agent OCS** sur des postes clients
- ✅ Synchroniser **OCS avec GLPI** (remontée automatique inventaire)
- ✅ Vérifier la **remontée d'inventaire** matériel et logiciel

**Catalogue de services :**
- ✅ Créer un **catalogue de services IT** structuré dans GLPI
- ✅ Définir des **catégories de services** (support, infrastructure, applications)
- ✅ Rédiger des **fiches de service** (description, SLA, procédure de demande)

**Support et tickets :**
- ✅ Simuler la **création de tickets** d'incidents réalistes
- ✅ Appliquer le **cycle de vie d'un ticket** (ouverture → diagnostic → résolution → clôture)
- ✅ Documenter les **résolutions** dans la base de connaissances GLPI

---

## ⭐ Spécificités Pédagogiques

### Le Projet 1 : Une Approche Multi-Blocs

**Structure du Projet 1 (S17-S18) :**

```
PROJET 1 — INFRASTRUCTURE PME SIMIO SARL
═══════════════════════════════════════════════════════════════

BLOC 2 (Infrastructure) — S17-S18
├─ Plan d'adressage VLSM
├─ Configuration VLANs
├─ Routage inter-VLAN
├─ Déploiement Active Directory (OU, utilisateurs, GPO)
├─ Services réseau (DHCP, DNS)
├─ Serveur fichiers + partages
└─ Serveur FTP/SFTP

BLOC 1 (Support) — S17-S18
├─ Installation GLPI + OCS Inventory
├─ Synchronisation OCS → GLPI
├─ Catalogue de services
├─ Résolution de tickets d'incidents
└─ Documentation dans le wiki d'équipe

BLOC 3 (Sécurité) — S17-S18
├─ Règles firewall Stormshield
├─ Segmentation LAN/DMZ/WAN
├─ GPO de sécurité (mots de passe, verrouillage)
├─ Configuration sauvegardes
└─ HTTPS sur services web

BLOC TRANSVERSAL
├─ Documentation complète dans le wiki (S16)
├─ Schémas réseau (architecture, adressage)
└─ Procédures techniques
```

**Coordination inter-blocs :** Les apprenants doivent communiquer entre eux pour que le système de support (BLOC 1) s'intègre à l'infrastructure (BLOC 2) sécurisée (BLOC 3).

### GLPI : Révision + Approfondissement

GLPI a été vu en S6 (installation de base + OCS). S17 approfondit :
- Configuration avancée (catégories, SLA, notifications)
- Catalogue de services structuré
- Base de connaissances
- Workflow complet des tickets

**Stratégie pédagogique :** Si les apprenants ont déjà une instance GLPI de S6, la réutiliser et l'enrichir. Sinon, réinstaller proprement.

### Le Wiki : Outil de Documentation Projet

Le wiki créé en S16 devient l'**outil central de documentation** du Projet 1. Les apprenants doivent y consigner :
- L'architecture technique du projet
- Les procédures d'installation
- Les configurations (serveurs, réseau, sécurité)
- Les incidents rencontrés et leurs résolutions
- Les comptes et mots de passe (chiffrés ou dans un gestionnaire)

**Évaluation du wiki :** En S18, la qualité de la documentation wiki sera évaluée (complétude, clarté, structure).

### Les 3 Tickets d'Incidents : Scénarios Réalistes

Les 3 tickets à résoudre en S17 sont des **incidents réels** typiques d'une PME :
1. **Incident réseau** : Un utilisateur ne peut plus accéder au serveur fichiers
2. **Incident logiciel** : Une application métier ne se lance plus
3. **Incident compte** : Un utilisateur a oublié son mot de passe et son compte est verrouillé

Ces incidents permettent de **mobiliser les compétences** acquises en S1-S16 (diagnostic, Active Directory, droits NTFS, support utilisateur).

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 15 min | 🚀 Lancement | Présentation Projet 1 — contexte, objectifs, livrables |
| H+0:15 | 20 min | 📖 Cours | Rappel GLPI/OCS + Catalogue de services |
| H+0:35 | 30 min | 🖥️ **TP Part. 1** | Installation/Configuration GLPI + OCS Server |
| H+1:05 | **15 min** | ☕ **PAUSE** | — |
| H+1:20 | 30 min | 🖥️ **TP Part. 2** | Déploiement agent OCS + synchronisation GLPI |
| H+1:50 | 30 min | 🖥️ **TP Part. 3** | Création catalogue de services dans GLPI |
| H+2:20 | 60 min | 🎭 **TP Part. 4** | Simulation résolution 3 tickets d'incidents |
| H+3:20 | 30 min | 📝 Documentation | Documentation dans le wiki d'équipe |
| H+3:50 | 10 min | 📅 Projection | Préparation S18 — suite du projet |

---

## Différenciation Pédagogique

### Profil Avancé
- **GLPI :** Configurer authentification LDAP (connexion AD)
- **OCS :** Déployer agent OCS en masse via GPO
- **Catalogue :** Créer des SLA différenciés par criticité
- **Tickets :** Résoudre un 4ème ticket complexe (corruption BDD)

### Profil Débutant
- **Installation :** Utiliser script d'installation semi-automatique
- **Catalogue :** Modèle de catalogue pré-rempli à compléter
- **Tickets :** Résoudre 2 tickets sur 3 (les plus simples)
- **Binômage :** Travailler avec un profil avancé

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **VM Ubuntu Server** | Pour GLPI + OCS Server |
| **VM Windows clients** | 2-3 postes pour déployer agent OCS |
| **Serveur AD** | Déjà déployé en BLOC 2 (S11-S12) |
| **Wiki d'équipe** | Créé en S16 |
| **Scénarios tickets** | Annexe 1 — 3 incidents détaillés |
| **Modèle catalogue** | Annexe 2 — structure type |

---

---

# 🚀 PRÉSENTATION DU PROJET 1

*Durée : 15 minutes — Collectif*

---

## Contexte du Projet

**Entreprise : SimIO SARL**
- Secteur : Commerce de proximité (vente de matériel informatique)
- Effectif : 80 employés
- 4 services : Direction, Commercial, Comptabilité, Informatique
- Besoin : Déployer une infrastructure IT complète et sécurisée

**Votre rôle :**
- Équipe IT de 3 personnes (binômes ou trinômes selon effectif)
- Mission : Installer et configurer l'infrastructure + système de support
- Durée : 2 semaines (S17-S18)

---

## Livrables du Projet 1 (BLOC 1)

| **Livrable** | **Description** | **Semaine** |
|---|---|---|
| **GLPI + OCS opérationnels** | Installation, configuration, synchronisation | S17 |
| **Inventaire automatique** | Agents OCS déployés, remontée inventaire | S17 |
| **Catalogue de services** | Structuré par catégories, fiches complètes | S17 |
| **Base incidents résolus** | 3 tickets traités et documentés | S17 |
| **Documentation wiki** | Architecture, procédures, configurations | S17-S18 |
| **Présentation orale** | Restitution projet devant le client (enseignant) | S18 |

---

## Critères d'Évaluation (BLOC 1)

| **Critère** | **Points** |
|---|---|
| Fonctionnement GLPI + OCS | /20 |
| Qualité du catalogue de services | /15 |
| Résolution des tickets | /25 |
| Documentation wiki | /20 |
| Présentation orale | /10 |
| Travail d'équipe | /10 |
| **TOTAL** | **/100** |

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "GLPI + OCS Inventory · Catalogue de Services · Support IT"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 17*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc |
| **B1.5** | Mettre à disposition un service informatique |
| **B1.2** | Exploiter des référentiels ITIL |

---

## PARTIE I — GLPI (Rappels + Approfondissement)

### I.A. Rappel : Qu'est-ce que GLPI ?

**GLPI** (Gestion Libre de Parc Informatique) est un logiciel open source de :
- **Gestion de parc** (inventaire matériel et logiciel)
- **Helpdesk** (gestion des tickets d'incidents et demandes)
- **Gestion des actifs IT** (licences, contrats, fournisseurs)

```
   GLPI — FONCTIONNALITÉS PRINCIPALES
   ─────────────────────────────────────────────────────────────

   ① INVENTAIRE
      • Matériel (PC, serveurs, imprimantes, téléphones)
      • Logiciels (licences, versions)
      • Réseau (switches, routeurs, IP)

   ② HELPDESK
      • Tickets d'incidents
      • Tickets de demandes de service
      • Suivi et historique

   ③ GESTION ADMINISTRATIVE
      • Contrats (maintenance, support)
      • Fournisseurs
      • Budgets

   ④ BASE DE CONNAISSANCES
      • Articles de résolution
      • FAQ
      • Procédures
```

**Chiffres clés :**
- 15 000+ organisations utilisent GLPI
- Disponible en 45 langues
- 500+ plugins disponibles

---

### I.B. OCS Inventory — Inventaire Automatique

**OCS Inventory** est un outil d'inventaire automatique qui remonte les informations vers GLPI.

```
   FONCTIONNEMENT OCS + GLPI
   ─────────────────────────────────────────────────────────────

   ① AGENT OCS (sur chaque poste client)
      • S'exécute en tâche planifiée (ex : tous les jours à 9h)
      • Scanne le matériel (CPU, RAM, disques, réseau)
      • Scanne les logiciels installés
      • Envoie les données au serveur OCS

   ② SERVEUR OCS INVENTORY
      • Reçoit les données des agents
      • Stocke dans sa base de données
      • Met à disposition via API

   ③ GLPI
      • Se connecte au serveur OCS via plugin
      • Importe automatiquement les nouveaux ordinateurs
      • Met à jour l'inventaire existant
      • Affiche tout dans son interface
```

**Avantages :**
- ✅ Inventaire automatique (pas de saisie manuelle)
- ✅ Mise à jour en temps réel
- ✅ Détection des changements matériels/logiciels
- ✅ Conformité licences (détecter les logiciels non autorisés)

---

## PARTIE II — Le Catalogue de Services

### II.A. Définition

Un **catalogue de services IT** est la liste structurée de tous les services proposés par la DSI aux utilisateurs.

```
   CATALOGUE DE SERVICES = MENU DE LA DSI

   Comme un menu de restaurant :
   ────────────────────────────────────────────────────────────
   • Liste tous les plats disponibles (services)
   • Indique les ingrédients (prérequis)
   • Précise les délais de préparation (SLA)
   • Affiche les prix (coûts si applicable)

   Catalogue de services IT :
   ────────────────────────────────────────────────────────────
   • Liste tous les services IT disponibles
   • Décrit comment les demander
   • Indique les délais de livraison (SLA)
   • Précise les conditions d'accès
```

---

### II.B. Structure d'un Catalogue

**Organisation en 3 niveaux :**

```
NIVEAU 1 — CATÉGORIES PRINCIPALES
├── Support Utilisateur
├── Infrastructure & Réseau
├── Applications Métier
└── Sécurité

NIVEAU 2 — SOUS-CATÉGORIES
└── Support Utilisateur
    ├── Compte et Authentification
    ├── Poste de Travail
    ├── Messagerie
    └── Impression

NIVEAU 3 — SERVICES DÉTAILLÉS
└── Compte et Authentification
    ├── Création de compte utilisateur
    ├── Réinitialisation mot de passe
    ├── Déblocage de compte
    └── Modification droits d'accès
```

---

### II.C. Fiche de Service Type

Chaque service du catalogue doit avoir une **fiche descriptive** :

```
╔══════════════════════════════════════════════════════════════╗
║             FICHE DE SERVICE                                 ║
╠══════════════════════════════════════════════════════════════╣
║  Nom du service     : Réinitialisation mot de passe         ║
║  Catégorie          : Support > Compte et Authentification  ║
║  Type               : Demande de service                     ║
╠══════════════════════════════════════════════════════════════╣
║  Description :                                               ║
║  Réinitialiser le mot de passe d'un utilisateur bloqué ou   ║
║  oublié, conformément à la politique de sécurité.           ║
╠══════════════════════════════════════════════════════════════╣
║  Bénéficiaires      : Tous les employés                     ║
║  Prérequis          : Validation identité (manager ou RH)   ║
╠══════════════════════════════════════════════════════════════╣
║  SLA (Délai)        : 2 heures ouvrées                      ║
║  Disponibilité      : Lun-Ven 8h-18h                        ║
╠══════════════════════════════════════════════════════════════╣
║  Procédure de demande :                                      ║
║  1. Créer un ticket dans GLPI                               ║
║  2. Catégorie : Support > Compte                            ║
║  3. Fournir : nom, prénom, service                          ║
║  4. Validation manager (par email)                          ║
╠══════════════════════════════════════════════════════════════╣
║  Contact support    : support@simio.local                   ║
╚══════════════════════════════════════════════════════════════╝
```

---

### II.D. Avantages d'un Catalogue

**Pour les utilisateurs :**
- ✅ Savent quels services sont disponibles
- ✅ Connaissent les délais de livraison
- ✅ Savent comment demander un service
- ✅ Autonomie (self-service)

**Pour la DSI :**
- ✅ Standardisation des demandes
- ✅ Priorisation facilitée (SLA définis)
- ✅ Mesure de la qualité de service
- ✅ Réduction des demandes hors périmètre

---

## PARTIE III — Le Cycle de Vie d'un Ticket

### III.A. Les États d'un Ticket

```
   CYCLE DE VIE D'UN TICKET DANS GLPI
   ─────────────────────────────────────────────────────────────

   ① NOUVEAU
      • Ticket vient d'être créé
      • En attente d'affectation à un technicien

   ② EN COURS (ATTRIBUÉ)
      • Ticket affecté à un technicien
      • Technicien travaille sur la résolution

   ③ EN ATTENTE
      • Ticket mis en pause (en attente info utilisateur, commande...)
      • Timer SLA en pause

   ④ RÉSOLU
      • Technicien a trouvé et appliqué une solution
      • En attente validation utilisateur

   ⑤ CLOS
      • Utilisateur confirme que le problème est résolu
      • Ticket archivé

   ⑥ ANNULÉ (optionnel)
      • Demande retirée par l'utilisateur
      • Ou doublon d'un autre ticket
```

---

### III.B. Les Champs Essentiels d'un Ticket

| **Champ** | **Description** | **Exemple** |
|---|---|---|
| **Titre** | Résumé court du problème | "Impossible d'accéder au serveur fichiers" |
| **Demandeur** | Utilisateur ayant créé le ticket | Julie Dupont (Comptabilité) |
| **Catégorie** | Classification du problème | Réseau > Accès serveur |
| **Priorité** | Urgence × Impact | 3 (Moyenne) |
| **Description** | Détails du problème | "Depuis ce matin 9h, message d'erreur..." |
| **Technicien** | Personne assignée à la résolution | Marc Technician |
| **Statut** | État actuel | En cours |
| **Solution** | Description de la résolution | "Droits NTFS manquants → ajoutés" |

---

### III.C. Diagnostic d'un Incident (Méthodologie)

**Méthode en 5 étapes :**

```
ÉTAPE 1 — COLLECTER LES INFORMATIONS
─────────────────────────────────────────────────────────────
Questions à poser :
• Quand le problème est-il apparu ?
• Quel est le message d'erreur exact ?
• Le problème affecte-t-il d'autres utilisateurs ?
• Des changements récents ont-ils été effectués ?


ÉTAPE 2 — REPRODUIRE LE PROBLÈME
─────────────────────────────────────────────────────────────
• Tenter de reproduire le problème sur le poste de l'utilisateur
• Tester sur un autre poste (problème local ou global ?)


ÉTAPE 3 — FORMULER DES HYPOTHÈSES
─────────────────────────────────────────────────────────────
Exemples :
• H1 : Droits d'accès insuffisants
• H2 : Problème réseau (câble, switch)
• H3 : Service Windows arrêté


ÉTAPE 4 — TESTER LES HYPOTHÈSES
─────────────────────────────────────────────────────────────
• Vérifier les droits NTFS → OK, droits corrects
• Tester ping vers serveur → KO, pas de réponse
  → H2 confirmée : problème réseau


ÉTAPE 5 — APPLIQUER LA SOLUTION
─────────────────────────────────────────────────────────────
• Vérifier câble réseau → débranché
• Rebrancher le câble
• Tester ping → OK
• Utilisateur peut accéder au serveur → Résolu
```

---

## IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **GLPI** | Gestion Libre de Parc Informatique — logiciel de gestion IT open source |
| **OCS Inventory** | Outil d'inventaire automatique de parc informatique |
| **Agent OCS** | Logiciel installé sur chaque poste qui remonte l'inventaire |
| **Catalogue de services** | Liste structurée des services IT proposés aux utilisateurs |
| **Fiche de service** | Description détaillée d'un service (SLA, procédure, prérequis) |
| **Ticket** | Demande de support ou signalement d'incident dans GLPI |
| **SLA** | Service Level Agreement — engagement sur délai de résolution |
| **Helpdesk** | Service d'assistance aux utilisateurs (synonyme : support) |
| **Base de connaissances** | Bibliothèque de solutions et procédures dans GLPI |
| **Inventaire** | Liste complète du matériel et logiciels du parc |

---

---

# 🖥️ TP PARTIE 1 — INSTALLATION GLPI + OCS SERVER

*Durée : 30 minutes — Individuel ou binôme*

---

## Objectif

Installer GLPI et OCS Inventory Server sur Ubuntu Server.

---

## ÉTAPE 1 — Installer les Prérequis (10 min)

**Si LAMP déjà installé (S14) :**

```bash
apache2 -v && mysql --version && php -v
# Si tout fonctionne, passer à l'étape 2
```

**Si LAMP non installé :**

```bash
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-ldap php-imap php-apcu -y

# Sécuriser MySQL
sudo mysql_secure_installation
# Mot de passe root : MySQL2024!
```

---

## ÉTAPE 2 — Télécharger et Installer GLPI (10 min)

```bash
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/10.0.12/glpi-10.0.12.tgz
tar -xzf glpi-10.0.12.tgz
sudo mv glpi /var/www/

# Permissions
sudo chown -R www-data:www-data /var/www/glpi
sudo chmod -R 755 /var/www/glpi
```

**Créer la base de données GLPI :**

```bash
sudo mysql -u root -p
# Mot de passe : MySQL2024!
```

```sql
CREATE DATABASE glpi_db;
CREATE USER 'glpi_user'@'localhost' IDENTIFIED BY 'GlpiPass2024!';
GRANT ALL PRIVILEGES ON glpi_db.* TO 'glpi_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

**Configurer VirtualHost Apache :**

```bash
sudo nano /etc/apache2/sites-available/glpi.conf
```

**Contenu :**

```apache
<VirtualHost *:80>
    ServerName glpi.local
    DocumentRoot /var/www/glpi/public

    <Directory /var/www/glpi/public>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/glpi_error.log
    CustomLog ${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
```

**Activer :**

```bash
sudo a2ensite glpi.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```

**Configurer /etc/hosts (poste client) :**

```
192.168.X.X    glpi.local
```

---

## ÉTAPE 3 — Installation Web de GLPI (10 min)

**Navigateur :** http://glpi.local

**Étape 1 — Langue :**
- Français → **OK**

**Étape 2 — Licence :**
- Accepter la licence GPL → **Continuer**

**Étape 3 — Choix installation :**
- **Installer**

**Étape 4 — Base de données :**
- Serveur SQL : `localhost`
- Utilisateur SQL : `glpi_user`
- Mot de passe SQL : `GlpiPass2024!`
- **Continuer**

**Étape 5 — Sélection BDD :**
- Sélectionner : `glpi_db`
- **Continuer**

**Étape 6 — Initialisation :**
- L'installation crée les tables
- **Continuer**

**Étape 7 — Fin :**
- Comptes par défaut créés
- **Utiliser GLPI**

**Connexion :**
- Identifiant : `glpi`
- Mot de passe : `glpi`

**⚠️ Changer le mot de passe immédiatement :**
- Administration → Utilisateurs → glpi → Mot de passe : `AdminGlpi2024!`

---

---

# 🖥️ TP PARTIE 2 — OCS INVENTORY + SYNCHRONISATION

*Durée : 30 minutes — Individuel ou binôme*

---

## Objectif

Installer OCS Inventory Server, déployer l'agent sur un client, synchroniser avec GLPI.

---

## ÉTAPE 1 — Installer OCS Inventory Server (15 min)

```bash
# Installer les dépendances
sudo apt install make gcc perl libxml-simple-perl libcompress-zlib-perl libdbi-perl libdbd-mysql-perl libnet-ip-perl libsoap-lite-perl libapache-dbi-perl libapache2-mod-perl2 libio-compress-perl -y

# Télécharger OCS
cd /tmp
wget https://github.com/OCSInventory-NG/OCSInventory-ocsreports/releases/download/2.12.1/OCSNG_UNIX_SERVER-2.12.1.tar.gz
tar -xzf OCSNG_UNIX_SERVER-2.12.1.tar.gz
cd OCSNG_UNIX_SERVER-2.12.1

# Installer
sudo ./setup.sh
```

**Réponses aux questions :**
```
Which host is running database server [localhost] → Entrée
On which port is running database server [3306] → Entrée
Where is Apache daemon binary [/usr/sbin/apache2] → Entrée
Apache user [www-data] → Entrée
Apache group [www-data] → Entrée

Do you wish to setup Communication server on this computer [y/n] → y
Do you wish to setup Administration server on this computer [y/n] → y

MySQL root password → MySQL2024!
```

**Relancer Apache :**

```bash
sudo systemctl restart apache2
```

**Accéder à OCS :**
- URL : http://glpi.local/ocsreports
- Identifiant : `admin`
- Mot de passe : `admin`

**⚠️ Changer le mot de passe :**
- Configuration → Users → admin → Password : `AdminOCS2024!`

---

## ÉTAPE 2 — Déployer Agent OCS sur Client Windows (10 min)

**Sur un poste client Windows :**

1. Télécharger l'agent : https://github.com/OCSInventory-NG/WindowsAgent/releases
2. Exécuter `OCS-NG-Windows-Agent-Setup.exe`
3. **Configuration agent :**
   - URL serveur : `http://192.168.X.X/ocsinventory`
   - TAG : `Projet1`
   - Lancer inventaire immédiatement : ☑ Oui
4. Installer

**Vérifier la remontée :**
- OCS Server : http://glpi.local/ocsreports
- All computers → le PC doit apparaître

---

## ÉTAPE 3 — Synchroniser OCS avec GLPI (5 min)

**Dans GLPI :**

1. Configuration → Plugins → **OCS Inventory NG**
2. Si pas installé : télécharger depuis https://plugins.glpi-project.org
3. Installer le plugin
4. Configuration du plugin :
   - URL serveur OCS : `http://localhost/ocsinventory`
   - Login : `admin`
   - Password : `AdminOCS2024!`
5. **Tester la connexion** → OK
6. **Lancer la synchronisation**
7. Parc → Ordinateurs → le PC remonté par OCS apparaît dans GLPI

---

---

# 🖥️ TP PARTIE 3 — CATALOGUE DE SERVICES

*Durée : 30 minutes — Individuel*

---

## Objectif

Créer un catalogue de services IT structuré dans GLPI.

---

## ÉTAPE 1 — Activer le Catalogue de Services (5 min)

**Dans GLPI :**

1. Configuration → Assistance → Catégories de tickets
2. Créer l'arborescence de catégories (voir Annexe 2)

---

## ÉTAPE 2 — Créer les Catégories Principales (10 min)

**Créer les catégories suivantes :**

| **Catégorie** | **Description** |
|---|---|
| Support Utilisateur | Assistance aux utilisateurs finaux |
| Infrastructure & Réseau | Services liés aux serveurs et au réseau |
| Applications Métier | Support sur les applications spécifiques |
| Sécurité | Demandes liées à la sécurité IT |

**Procédure :**
1. Configuration → Intitulés → Catégories de tickets
2. Cliquer **+** (Ajouter)
3. Nom : `Support Utilisateur`
4. **Ajouter**
5. Répéter pour les 3 autres catégories

---

## ÉTAPE 3 — Créer les Sous-Catégories (15 min)

**Sous Support Utilisateur, créer :**

| **Sous-catégorie** | **Parent** |
|---|---|
| Compte et Authentification | Support Utilisateur |
| Poste de Travail | Support Utilisateur |
| Messagerie | Support Utilisateur |
| Impression | Support Utilisateur |

**Sous Infrastructure & Réseau, créer :**

| **Sous-catégorie** | **Parent** |
|---|---|
| Serveur | Infrastructure & Réseau |
| Réseau | Infrastructure & Réseau |
| Stockage | Infrastructure & Réseau |

**Procédure :**
1. Cliquer sur **Support Utilisateur**
2. Cliquer **Ajouter un élément fils**
3. Nom : `Compte et Authentification`
4. **Ajouter**
5. Répéter pour les autres

---

---

# 🎭 TP PARTIE 4 — RÉSOLUTION DE 3 TICKETS

*Durée : 60 minutes — Individuel*

---

## Objectif

Simuler la création et la résolution de 3 tickets d'incidents réalistes.

---

## INCIDENT 1 — Accès Serveur Refusé (20 min)

**Contexte :**
Julie Dupont (Comptabilité) ne peut plus accéder au dossier partagé `\\SRV-FILES\Comptabilite` depuis ce matin. Message d'erreur : "Accès refusé".

**Créer le ticket dans GLPI :**

1. Assistance → Tickets → **Créer un ticket**
2. Titre : `Accès refusé au dossier Comptabilité`
3. Demandeur : Julie Dupont
4. Catégorie : `Support Utilisateur > Poste de Travail`
5. Description :
   ```
   Depuis ce matin 9h, je ne peux plus ouvrir le dossier Comptabilité
   sur le serveur. Message : "Accès refusé".
   
   Hier soir, tout fonctionnait normalement.
   ```
6. Priorité : Haute
7. **Créer**

**Diagnostic :**

```
HYPOTHÈSES
──────────────────────────────────────────────────────────────
H1 : Mot de passe expiré → Test connexion AD : OK
H2 : Droits NTFS retirés → À vérifier
H3 : Serveur inaccessible → Test ping : OK

TESTS
──────────────────────────────────────────────────────────────
1. Se connecter sur SRV-FILES
2. Vérifier les droits NTFS du dossier Comptabilité
   → Propriétés → Sécurité
   → Le groupe "Comptabilite" est absent !

CAUSE IDENTIFIÉE
──────────────────────────────────────────────────────────────
Les droits NTFS du groupe Comptabilite ont été supprimés
(probablement lors d'une manipulation hier soir)
```

**Résolution :**

```bash
# Sur SRV-FILES
# Clic droit sur C:\Partages\Comptabilite → Propriétés → Sécurité
# Ajouter → Groupe "Comptabilite" → Modification
# Appliquer
```

**Dans GLPI :**

1. Ouvrir le ticket
2. Solution :
   ```
   DIAGNOSTIC :
   Les droits NTFS du groupe Comptabilite avaient été supprimés.
   
   RÉSOLUTION :
   Ajout du groupe Comptabilite avec droits "Modification" sur
   le dossier \\SRV-FILES\Comptabilite
   
   VALIDATION :
   Utilisateur peut de nouveau accéder au dossier.
   ```
3. Statut : **Résolu**
4. Temps passé : 15 minutes

---

## INCIDENT 2 — Application Métier ne se Lance Pas (20 min)

**Contexte :**
Pierre Martin (Commercial) signale que le logiciel de gestion commerciale "GestCom" affiche une erreur au lancement : "Erreur de connexion à la base de données".

**Créer le ticket dans GLPI :**

1. Titre : `GestCom : Erreur connexion base de données`
2. Demandeur : Pierre Martin
3. Catégorie : `Applications Métier`
4. Description :
   ```
   Depuis 10h, GestCom ne se lance plus. Message d'erreur :
   "Erreur de connexion à la base de données SQL Server".
   
   Mes collègues du service commercial ont le même problème.
   ```
5. Priorité : Critique (impact : tout le service commercial)

**Diagnostic :**

```
HYPOTHÈSES
──────────────────────────────────────────────────────────────
H1 : Application corrompue → Tester sur autre poste : même erreur
     → H1 rejetée (problème global)
H2 : Serveur SQL inaccessible → À vérifier
H3 : Service SQL Server arrêté → À vérifier

TESTS
──────────────────────────────────────────────────────────────
1. Ping vers serveur SQL (SRV-SQL01) → OK
2. Se connecter sur SRV-SQL01
3. Vérifier services Windows
   → Service "SQL Server (MSSQLSERVER)" : Arrêté

CAUSE IDENTIFIÉE
──────────────────────────────────────────────────────────────
Le service SQL Server s'est arrêté (probablement suite à une
mise à jour Windows cette nuit)
```

**Résolution :**

```powershell
# Sur SRV-SQL01
# Services.msc
# SQL Server (MSSQLSERVER) → Démarrer
# Démarrage automatique → Configurer
```

**Dans GLPI :**

1. Solution :
   ```
   DIAGNOSTIC :
   Le service SQL Server était arrêté sur SRV-SQL01.
   
   RÉSOLUTION :
   - Redémarrage du service SQL Server
   - Configuration en démarrage automatique
   
   CAUSE :
   Probable redémarrage suite aux mises à jour Windows
   
   VALIDATION :
   GestCom fonctionne de nouveau. 12 utilisateurs validés.
   
   ACTION PRÉVENTIVE :
   Configurer monitoring du service SQL Server dans Nagios.
   ```
2. Statut : **Résolu**
3. Temps passé : 20 minutes

---

## INCIDENT 3 — Compte Utilisateur Verrouillé (20 min)

**Contexte :**
Sophie Bernard (Direction) ne peut plus se connecter à son PC. Message : "Votre compte a été verrouillé. Contactez l'administrateur."

**Créer le ticket dans GLPI :**

1. Titre : `Compte AD verrouillé - Sophie Bernard`
2. Demandeur : Sophie Bernard
3. Catégorie : `Support Utilisateur > Compte et Authentification`
4. Description :
   ```
   Je ne peux plus me connecter sur mon PC.
   Message : "Compte verrouillé".
   
   J'ai peut-être fait une erreur en tapant mon mot de passe
   plusieurs fois ce matin.
   ```
5. Priorité : Haute (Direction)

**Diagnostic :**

```
HYPOTHÈSES
──────────────────────────────────────────────────────────────
H1 : Tentatives de connexion échouées → À vérifier dans AD
H2 : Mot de passe expiré → À vérifier

TESTS
──────────────────────────────────────────────────────────────
1. Ouvrir Active Directory sur SRV-DC01
2. Utilisateurs et ordinateurs AD
3. Rechercher : Sophie Bernard (sbernard)
4. Propriétés du compte
   → ☑ Le compte est verrouillé
   
CAUSE IDENTIFIÉE
──────────────────────────────────────────────────────────────
Compte verrouillé suite à 5 tentatives de connexion échouées
(politique de sécurité AD)
```

**Résolution :**

```
# Sur SRV-DC01
# Active Directory → Utilisateurs
# Clic droit sur sbernard → Propriétés
# Compte → Déverrouiller le compte
# Appliquer
```

**Communication avec l'utilisateur :**

```
Email ou appel téléphonique :

"Bonjour Sophie,

Votre compte a été déverrouillé. Vous pouvez de nouveau vous
connecter.

IMPORTANT : Votre mot de passe actuel fonctionne. Cependant,
pour éviter un nouveau verrouillage, je vous recommande de :
1. Vérifier que le Caps Lock n'est pas activé
2. Taper lentement votre mot de passe

Si vous avez oublié votre mot de passe, nous pouvons le
réinitialiser.

Cordialement,
Support IT"
```

**Dans GLPI :**

1. Solution :
   ```
   DIAGNOSTIC :
   Compte AD verrouillé suite à tentatives de connexion échouées.
   
   RÉSOLUTION :
   - Déverrouillage du compte via Active Directory
   - Utilisateur informé par téléphone
   - Validation : utilisateur peut se reconnecter
   
   RECOMMANDATION :
   Rappeler à l'utilisateur la politique de mots de passe.
   ```
2. Statut : **Résolu**
3. Temps passé : 10 minutes

---

---

# 📝 DOCUMENTATION DANS LE WIKI

*Durée : 30 minutes — Collectif*

---

## Objectif

Documenter le Projet 1 (partie S17) dans le wiki d'équipe créé en S16.

---

## Pages à Créer

### 1. Page "Projet 1 — Architecture Globale"

```wiki
====== Projet 1 — Infrastructure SimIO SARL ======

**Date de réalisation :** S17-S18 (Février 2025)
**Équipe :** [Noms des membres]

===== Architecture Générale =====

[Insérer schéma réseau]

===== Composants Déployés =====

^ Composant ^ Serveur ^ IP ^ Rôle ^
| Active Directory | SRV-DC01 | 192.168.10.20 | Contrôleur de domaine |
| GLPI | SRV-GLPI | 192.168.10.30 | Gestion parc + Helpdesk |
| OCS Inventory | SRV-GLPI | 192.168.10.30 | Inventaire automatique |
| Serveur Fichiers | SRV-FILES | 192.168.10.40 | Partages réseau |

===== Documentation Technique =====

  * [[projet_1:installation_glpi|Installation GLPI + OCS]]
  * [[projet_1:catalogue_services|Catalogue de Services]]
  * [[projet_1:incidents_resolus|Incidents Résolus]]
```

---

### 2. Page "Installation GLPI + OCS"

```wiki
====== Procédure : Installation GLPI + OCS Inventory ======

**Auteur :** [Nom]
**Date :** 2025-02-XX
**Version :** 1.0

===== Prérequis =====

  * Ubuntu Server 22.04
  * LAMP installé (Apache, MySQL, PHP 8.1+)
  * Accès sudo

===== Installation GLPI =====

==== 1. Télécharger GLPI ====

<code bash>
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/10.0.12/glpi-10.0.12.tgz
tar -xzf glpi-10.0.12.tgz
sudo mv glpi /var/www/
</code>

[Suite de la procédure...]

===== Installation OCS Inventory =====

[Procédure détaillée...]

===== Synchronisation OCS → GLPI =====

[Procédure détaillée...]
```

---

### 3. Page "Incidents Résolus"

```wiki
====== Base d'Incidents Résolus — Projet 1 ======

===== Incident #1 : Accès Serveur Refusé =====

**Date :** 2025-02-XX
**Utilisateur :** Julie Dupont (Comptabilité)
**Symptôme :** Accès refusé au dossier \\SRV-FILES\Comptabilite

**Diagnostic :**
Droits NTFS du groupe Comptabilite supprimés.

**Résolution :**
Ajout du groupe Comptabilite avec droits "Modification".

**Temps de résolution :** 15 minutes

===== Incident #2 : Application GestCom =====

[Idem pour les 2 autres incidents...]
```

---

---

# 📄 ANNEXE 1 — SCÉNARIOS DÉTAILLÉS DES 3 TICKETS

*(Fournis dans la section TP Partie 4)*

---

# 📄 ANNEXE 2 — MODÈLE CATALOGUE DE SERVICES

```
CATALOGUE DE SERVICES IT — SIMIO SARL
═══════════════════════════════════════════════════════════════

📁 SUPPORT UTILISATEUR
   ├─ Compte et Authentification
   │  ├─ Création de compte utilisateur (SLA : 4h)
   │  ├─ Réinitialisation mot de passe (SLA : 2h)
   │  ├─ Déblocage de compte (SLA : 1h)
   │  └─ Modification droits d'accès (SLA : 4h)
   │
   ├─ Poste de Travail
   │  ├─ Installation logiciel standard (SLA : 24h)
   │  ├─ Résolution problème matériel (SLA : 8h)
   │  └─ Configuration poste neuf (SLA : 48h)
   │
   ├─ Messagerie
   │  ├─ Création boîte mail (SLA : 4h)
   │  ├─ Configuration Outlook (SLA : 2h)
   │  └─ Problème envoi/réception (SLA : 4h)
   │
   └─ Impression
      ├─ Installation imprimante réseau (SLA : 4h)
      └─ Dépannage impression (SLA : 2h)

📁 INFRASTRUCTURE & RÉSEAU
   ├─ Serveur
   │  ├─ Demande accès serveur fichiers (SLA : 8h)
   │  └─ Incident serveur (SLA : 1h si critique)
   │
   ├─ Réseau
   │  ├─ Problème connexion réseau (SLA : 2h)
   │  └─ Demande ouverture port firewall (SLA : 24h)
   │
   └─ Stockage
      └─ Augmentation quota stockage (SLA : 48h)

📁 APPLICATIONS MÉTIER
   ├─ GestCom (Gestion Commerciale)
   ├─ Paie & Compta
   └─ CRM

📁 SÉCURITÉ
   ├─ Signalement incident sécurité (SLA : immédiat)
   └─ Demande accès privilégiés (SLA : 24h + validation)
```

---

*Pack S17 BLOC 1 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences : B1.4, B1.5, B1.2, B3.3*
*PROJET 1 · GLPI · OCS Inventory · Catalogue de services · Support · Tickets*
