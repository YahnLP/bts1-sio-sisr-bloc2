# Pack de Formation - Semaine 16 (S16) - BLOC 1
## 👥 Travail Collaboratif · Wiki · Git · Partage Documentaire · TP Wiki Technique

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S16 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — seizième semaine |
| **Modalité** | Présentiel — salle TP |
| **Prérequis** | Notions Linux (S8-S14), documentation procédures (S11), veille techno (S13) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B3.3** | Participer à la gestion et au suivi d'un projet | Maîtrise |
| **B1.5** | Mettre à disposition des utilisateurs un service informatique | Acquisition |
| **B3.4** | Mettre en œuvre une démarche de veille technologique | Application |

> 📌 **S16 BLOC 1 marque l'entrée dans la Phase 4** (Projet de synthèse S16-S20). Elle pose les bases du travail collaboratif avant le Projet 1 (S17-S18) où les apprenants travailleront en équipe sur une infrastructure complète. Le wiki créé en S16 servira de documentation collective pour le projet.

---

## Objectifs Pédagogiques

**Travail collaboratif :**
- ✅ Expliquer les **enjeux** du travail collaboratif en IT
- ✅ Identifier les **outils** adaptés selon les besoins (wiki, Git, cloud, ticketing)
- ✅ Comprendre les **bonnes pratiques** de collaboration (nommage, versioning, communication)

**Wiki technique :**
- ✅ Définir ce qu'est un **wiki** et ses cas d'usage
- ✅ Comparer les solutions de wiki (MediaWiki, DokuWiki, BookStack, Confluence)
- ✅ Installer et configurer **DokuWiki**
- ✅ Créer une **structure documentaire** pour une équipe IT
- ✅ Rédiger des pages wiki en **syntaxe markdown/dokuwiki**

**Git (introduction) :**
- ✅ Comprendre le concept de **versioning** de fichiers
- ✅ Expliquer à quoi sert **Git** (suivi modifications, collaboration)
- ✅ Distinguer Git et GitHub
- ✅ Effectuer les **commandes de base** (init, add, commit, log)

**Partage documentaire :**
- ✅ Comparer les solutions de partage (NAS, cloud, serveur fichiers)
- ✅ Identifier les critères de choix (sécurité, coût, accessibilité)

---

## ⭐ Spécificités Pédagogiques

### Le Wiki : Un Outil Sous-Estimé

Beaucoup d'apprenants découvrent le concept de wiki en S16. Ils connaissent Wikipedia mais n'ont jamais pensé à créer un wiki pour leur équipe IT.

**L'argument d'ouverture :**

> *"Dans une DSI, la documentation est le premier outil de productivité. Un technicien qui documente ses procédures dans un wiki fait gagner 50% de temps à toute l'équipe. Sans wiki, chaque technicien réinvente la roue : 'Comment on fait déjà pour créer un utilisateur AD ?' → 10 minutes perdues à chercher dans les emails. Avec un wiki : recherche 'créer utilisateur AD' → procédure en 10 secondes."*

### Git : Sensibilisation, Pas Maîtrise

S16 BLOC 1 fait une **introduction à Git**, pas une formation complète. L'objectif est que les apprenants :
1. Comprennent **pourquoi** Git est utile (versioning, collaboration)
2. Sachent **exécuter** les 4 commandes de base (init, add, commit, log)
3. Aient **envie** d'approfondir Git plus tard

**Ce qui n'est PAS traité en S16 :**
- Les branches (branch, merge)
- Les dépôts distants (push, pull, clone)
- GitHub/GitLab

Ces sujets seront vus en Année 2 ou en autonomie.

### Le TP Wiki : Documentation Collective du Projet 1

Le wiki créé en S16 servira de **base documentaire** pour le Projet 1 (S17-S18). Les apprenants y documenteront :
- L'architecture réseau du projet
- Les procédures d'installation
- Les configurations serveurs
- Les comptes et mots de passe (chiffrés)
- Les incidents et résolutions

**Stratégie pédagogique :** Créer le wiki en S16, puis l'alimenter tout au long du projet en S17-S18.

### Lien avec l'Alternance

En entreprise, les apprenants sont confrontés à :
- Des documentations obsolètes ou inexistantes
- Des connaissances dispersées (emails, papiers, mémoire des anciens)
- Des outils collaboratifs imposés (SharePoint, Confluence, Notion...)

S16 leur donne les clés pour **structurer et partager** la connaissance technique.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S15 | Feedback évaluation formative 2 — correction collective |
| H+0:10 | 15 min | 🎯 Découverte | Activité "La Documentation Perdue" |
| H+0:25 | 25 min | 📖 Cours | Travail collaboratif : enjeux, outils, bonnes pratiques |
| H+0:50 | 20 min | 📖 Cours | Wiki : définition, comparatif solutions, structure |
| H+1:10 | 10 min | 📖 Cours | Git : versioning, commandes de base (introduction) |
| H+1:20 | **15 min** | ☕ **PAUSE** | — |
| H+1:35 | 15 min | 🔧 Démo | Démonstration installation DokuWiki par l'enseignant |
| H+1:50 | 60 min | 🖥️ **TP Part. 1** | Installation et configuration DokuWiki |
| H+2:50 | 40 min | 🖥️ **TP Part. 2** | Création structure wiki + pages de documentation |
| H+3:30 | 20 min | 👥 Collaboratif | Travail en binôme : relecture croisée des wikis |
| H+3:50 | 10 min | 📅 Projection | Préparation Projet 1 (S17-S18) — rôle du wiki |

---

## Différenciation Pédagogique

### Profil Avancé
- **Wiki :** Installer MediaWiki (plus complexe) au lieu de DokuWiki
- **Git :** Créer un dépôt GitHub, effectuer push/pull
- **Extension :** Configurer authentification LDAP sur le wiki
- **Automatisation :** Script Bash de backup automatique du wiki

### Profil Débutant
- **Wiki :** Utiliser une installation DokuWiki pré-configurée
- **Structure :** Modèle de structure wiki fourni à copier
- **Git :** Démo uniquement (pas de TP Git pour eux)
- **Binômage :** Travailler avec un profil avancé

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **VM Ubuntu Server** | 1 par apprenant (ou réutiliser celle de S14-S15) |
| **Accès sudo** | Root ou utilisateur sudo |
| **Navigateur web** | Pour accéder au wiki |
| **Modèle structure wiki** | Annexe 1 — arborescence type |
| **Cheat sheet Git** | Annexe 2 — commandes de base |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "La Documentation Perdue"

*Durée : 15 minutes — Collectif*

---

## Mise en Situation (8 min)

L'enseignant raconte l'histoire :

> **Contexte :** DSI d'une PME de 80 employés. Équipe IT de 3 personnes :
> - **Marc** (responsable IT, 55 ans, 15 ans d'ancienneté)
> - **Julie** (technicienne, 30 ans, 3 ans d'ancienneté)
> - **Vous** (alternant, arrivé il y a 2 mois)
>
> **Lundi 9h :** Marc ne vient pas travailler. Il a eu un accident de voiture ce weekend (bénin, mais hospitalisé 3 semaines).
>
> **Lundi 10h :** Le serveur de messagerie tombe en panne. 80 employés ne peuvent plus envoyer/recevoir d'emails.
>
> **Julie :** "Marc s'occupait toujours du serveur mail. Je ne sais pas où sont les mots de passe admin ni comment le redémarrer."
>
> **Vous cherchez la documentation :**
> - Aucun wiki d'équipe
> - Quelques procédures Word éparpillées sur le serveur fichiers (non datées, non organisées)
> - Des emails de Marc avec des notes ("mot de passe mail : dans le fichier Excel sur mon bureau")
> - Le fichier Excel n'existe pas ou est sur le PC de Marc (éteint, chiffré)
>
> **Résultat :**
> - 4 heures pour retrouver le mot de passe admin (trouvé dans un Post-it sous le clavier de Marc)
> - 6 heures pour comprendre comment redémarrer le service (tentatives multiples)
> - Messagerie rétablie à 18h → 80 employés sans email pendant 8 heures
> - Coût estimé de la panne : 15 000 € de productivité perdue
>
> **Si un wiki d'équipe avait existé :**
> - Page "Serveur Messagerie" avec procédure de redémarrage : 10 minutes
> - Mot de passe stocké dans un gestionnaire d'équipe : 2 minutes
> - Messagerie rétablie en 15 minutes → perte < 1 000 €

---

## Questions Guidées (5 min)

| **Question** | **Concept visé** |
|---|---|
| "Quelle est la cause principale du problème ?" | Documentation inexistante ou non centralisée |
| "Marc est-il coupable de ne pas avoir documenté ?" | Oui, mais c'est un problème d'équipe (pas d'outil imposé) |
| "Que se serait-il passé si Marc avait démissionné ?" | Perte de connaissance = catastrophe |
| "Quels outils auraient pu éviter ce problème ?" | Wiki d'équipe, gestionnaire de mots de passe, procédures centralisées |

## Conclusion (2 min)

**L'enseignant écrit au tableau :**

```
   DOCUMENTATION = ASSURANCE CONTRE LA PERTE DE CONNAISSANCE

   Sans documentation centralisée :
   ❌ Dépendance aux personnes
   ❌ Perte de temps (recherche infos)
   ❌ Risque d'erreur (mémoire défaillante)
   ❌ Coût des pannes

   Avec wiki d'équipe :
   ✅ Autonomie de l'équipe
   ✅ Gain de temps (recherche instantanée)
   ✅ Qualité constante
   ✅ Continuité de service
```

> *"Un wiki d'équipe n'est pas un luxe — c'est un outil de survie pour une DSI."*

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Travail Collaboratif · Outils · Bonnes Pratiques"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 16*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B3.3** | Participer à la gestion et au suivi d'un projet |
| **B1.5** | Mettre à disposition un service informatique |

---

## PARTIE I — Les Enjeux du Travail Collaboratif

### I.A. Pourquoi Collaborer ?

En IT, **personne ne travaille seul** :
- Les projets impliquent plusieurs personnes (techniciens, responsables, utilisateurs)
- Les connaissances sont dispersées (chacun a son expertise)
- Les outils évoluent (veille collective plus efficace)

```
   TRAVAIL ISOLÉ                    vs        TRAVAIL COLLABORATIF
   ─────────────────────                     ────────────────────────
   • Chacun dans son coin                    • Équipe synchronisée
   • Documentation personnelle               • Documentation partagée
   • Connaissances perdues si départ         • Connaissances capitalisées
   • Réinventer la roue                      • Réutiliser l'existant
   • Erreurs répétées                        • Apprentissage collectif
```

**Statistiques (Atlassian 2023) :**
- 86% des DSI considèrent la collaboration comme critique
- 75% des échecs de projets IT sont dus à une mauvaise communication
- 60% du temps d'un technicien est perdu à chercher de l'information

---

### I.B. Les 4 Piliers de la Collaboration IT

**① DOCUMENTATION PARTAGÉE**

Centraliser la connaissance technique dans un espace accessible à tous.

**Outils :** Wiki (DokuWiki, MediaWiki, Confluence), Base de connaissances (SharePoint, Notion)

**② VERSIONING ET GESTION DE CODE**

Suivre les modifications des fichiers (scripts, configurations, code).

**Outils :** Git, SVN, GitLab, GitHub

**③ COMMUNICATION ASYNCHRONE**

Communiquer sans nécessiter de réponse immédiate (vs téléphone, réunion).

**Outils :** Slack, Microsoft Teams, Mattermost, email structuré

**④ GESTION DE TÂCHES**

Organiser le travail, attribuer des tâches, suivre l'avancement.

**Outils :** Jira, Trello, Monday, GLPI (tickets)

---

### I.C. Bonnes Pratiques

**① NOMMER LES FICHIERS CORRECTEMENT**

```
❌ MAUVAIS EXEMPLES
─────────────────────────────────────────────────────────────
• doc.txt (quoi comme doc ?)
• nouveau.docx (nouveau quoi ?)
• Copie de Copie de rapport.pdf (quelle version ?)
• IMG_3847.jpg (contenu ?)

✅ BONS EXEMPLES
─────────────────────────────────────────────────────────────
• 2024-02-16_Procedure_Installation_Apache.pdf (date + description)
• Config_Switch_Principal_v2.3.txt (nom + version)
• Schema_Reseau_Projet1_Final.png (projet + état)
```

**Convention de nommage recommandée :**
```
[Date]_[Type]_[Description]_[Version].[ext]

Exemples :
• 2024-02-16_Procedure_Backup_v1.0.pdf
• 2024-02-15_Schema_Infra_SimIO.png
• 2024-02-14_Config_Firewall_v2.1.txt
```

**② VERSIONNER LES DOCUMENTS**

Utiliser un système de versions explicites :
- v1.0 → version initiale
- v1.1 → corrections mineures
- v2.0 → refonte majeure

**③ TOUJOURS DATER**

Un document sans date est un document mort (on ne sait pas s'il est à jour).

**④ UTILISER UN SEUL OUTIL PAR USAGE**

Ne pas multiplier les outils pour le même besoin :
- ❌ Documentation dans : emails + Word + Google Docs + papier
- ✅ Documentation dans : wiki uniquement

---

## PARTIE II — Le Wiki Technique

### II.A. Qu'est-ce qu'un Wiki ?

Un **wiki** est un site web collaboratif où chaque page peut être modifiée par plusieurs utilisateurs.

```
   CARACTÉRISTIQUES D'UN WIKI
   ─────────────────────────────────────────────────────────────
   ✅ Éditable par plusieurs personnes
   ✅ Historique des modifications (qui, quand, quoi)
   ✅ Recherche intégrée
   ✅ Liens entre pages (navigation fluide)
   ✅ Syntaxe simple (markdown ou équivalent)
   ✅ Pas besoin de coder HTML
```

**Exemples de wikis connus :**
- **Wikipedia** : encyclopédie collaborative mondiale
- **ArchWiki** : documentation Linux Arch (référence dans la communauté)
- **Wiki interne** : documentation d'équipe (IT, projets, procédures)

---

### II.B. Cas d'Usage d'un Wiki IT

| **Usage** | **Contenu typique** | **Exemple** |
|---|---|---|
| **Base de connaissances** | Procédures techniques, FAQ | "Comment créer un utilisateur AD" |
| **Documentation projet** | Architecture, schémas, décisions | "Projet SimIO — Architecture réseau" |
| **Onboarding** | Guide pour nouveaux arrivants | "Bienvenue dans l'équipe IT" |
| **Troubleshooting** | Incidents connus et résolutions | "Serveur mail ne démarre pas → solution" |
| **Inventaire** | Liste des serveurs, IP, comptes | "Serveurs production — tableau récapitulatif" |
| **Veille techno** | Synthèses de veille, nouveautés | "Nouveautés Windows Server 2025" |

---

### II.C. Comparatif des Solutions de Wiki

| **Wiki** | **Technicité** | **Hébergement** | **Coût** | **Points forts** | **Usage type** |
|---|---|---|---|---|---|
| **DokuWiki** | ★☆☆ | Auto-hébergé | Gratuit | Pas de BDD, fichiers texte, plugins | PME, labo, école |
| **MediaWiki** | ★★☆ | Auto-hébergé | Gratuit | Même moteur que Wikipedia, puissant | DSI, projets complexes |
| **BookStack** | ★☆☆ | Auto-hébergé | Gratuit | Interface moderne, organisation livres/chapitres | Équipes < 20 personnes |
| **Confluence** | ★☆☆ | Cloud ou auto | 10 users = 10 $/mois | Intégration Jira, professionnel | Entreprises structurées |
| **Notion** | ★☆☆ | Cloud | Gratuit/payant | Moderne, tout-en-un (wiki+tâches+bases) | Startups, petites équipes |
| **Wiki.js** | ★★☆ | Auto-hébergé | Gratuit | Moderne, markdown natif, open source | Équipes techniques |

> 📌 **Choix S16 BLOC 1 :** DokuWiki — simple, sans base de données, parfait pour apprendre.

---

### II.D. DokuWiki — Présentation

**DokuWiki** est un wiki open source créé en 2004, très utilisé dans les environnements IT.

**Caractéristiques :**
- ✅ Pas de base de données (tout en fichiers texte)
- ✅ Installation en 5 minutes
- ✅ Syntaxe wiki simple
- ✅ 1000+ plugins disponibles
- ✅ Gestion des droits (ACL)
- ✅ Historique des modifications
- ✅ Recherche full-text

**Stockage :**
```
/var/www/dokuwiki/
├── data/
│   ├── pages/           ← Contenu des pages (fichiers .txt)
│   ├── media/           ← Images, fichiers joints
│   └── attic/           ← Historique des versions
├── conf/                ← Configuration
└── lib/                 ← Plugins
```

---

## PARTIE III — Git (Introduction)

### III.A. Le Problème du Versioning

**Situation classique sans Git :**

```
Mon_Projet/
├── script.sh
├── script_v2.sh
├── script_v2_final.sh
├── script_v2_final_VRAI.sh
├── script_v2_final_VRAI_corrigé.sh
└── script_OK.sh            ← Lequel est le bon ?
```

**Problèmes :**
- On ne sait plus quelle est la bonne version
- Impossible de savoir ce qui a changé entre les versions
- Si erreur, difficile de revenir en arrière
- Impossible de travailler à plusieurs sur le même fichier

---

### III.B. Git — La Solution

**Git** est un système de **gestion de versions** (version control system — VCS).

```
   GIT EN UNE PHRASE
   ─────────────────────────────────────────────────────────────
   Git enregistre l'historique complet de toutes les modifications
   d'un projet, permettant de :
   • Revenir à n'importe quelle version
   • Voir qui a modifié quoi et quand
   • Travailler en parallèle sur le même projet
```

**Avantages :**
- ✅ Historique complet de tous les changements
- ✅ Chaque modification est datée et attribuée à son auteur
- ✅ Possibilité de revenir en arrière à n'importe quel moment
- ✅ Branches pour travailler sur des fonctionnalités séparées
- ✅ Collaboration sans conflit

---

### III.C. Git vs GitHub

**Confusion fréquente :** Git ≠ GitHub

| **Aspect** | **Git** | **GitHub** |
|---|---|---|
| **Nature** | Logiciel (installé sur votre PC) | Site web (service en ligne) |
| **Fonction** | Gérer les versions localement | Héberger le code en ligne |
| **Utilisation** | Ligne de commande | Interface web + Git |
| **Coût** | Gratuit | Gratuit (public) / payant (privé) |

**Analogie :**
- **Git** = Word (logiciel pour écrire)
- **GitHub** = Google Drive (endroit pour stocker et partager)

**Alternatives à GitHub :** GitLab, Bitbucket, Gitea

---

### III.D. Les 4 Commandes de Base

**① git init** — Initialiser un dépôt Git

```bash
cd /home/user/mon-projet
git init
# Résultat : Création d'un dossier caché .git
```

**② git add** — Ajouter des fichiers au suivi

```bash
git add script.sh          # Ajouter un fichier
git add .                  # Ajouter tous les fichiers modifiés
```

**③ git commit** — Enregistrer une version

```bash
git commit -m "Ajout de la fonction de backup automatique"
# -m = message décrivant les modifications
```

**④ git log** — Voir l'historique

```bash
git log
# Affiche la liste des commits avec date, auteur, message
```

**Workflow typique :**

```bash
# 1. Créer un projet
mkdir mon-script
cd mon-script
git init

# 2. Créer un fichier
echo "#!/bin/bash" > backup.sh
echo "echo 'Backup en cours...'" >> backup.sh

# 3. Ajouter au suivi Git
git add backup.sh

# 4. Enregistrer la version
git commit -m "Version initiale du script de backup"

# 5. Modifier le fichier
echo "tar -czf backup.tar.gz /data" >> backup.sh

# 6. Enregistrer la modification
git add backup.sh
git commit -m "Ajout de la commande tar pour compresser"

# 7. Voir l'historique
git log
```

---

## PARTIE IV — Partage Documentaire

### IV.A. Solutions de Partage

| **Solution** | **Principe** | **Avantages** | **Inconvénients** | **Usage** |
|---|---|---|---|---|
| **Serveur fichiers (SMB/NFS)** | Dossiers partagés sur serveur local | Contrôle total, rapide, local | Pas d'accès distant facile | PME, réseau local |
| **NAS** | Boîtier dédié partage fichiers | Simple, fiable, RAID | Coût initial | PME/TPE |
| **Cloud public** | Google Drive, Dropbox, OneDrive | Accessible partout, facile | Dépendance, confidentialité | Petites équipes |
| **Cloud privé** | Nextcloud, ownCloud auto-hébergé | Contrôle total, souveraineté | Installation, maintenance | DSI structurées |
| **SharePoint** | Solution Microsoft intégrée Office 365 | Intégration MS, workflow | Complexité, coût | Grandes entreprises |

---

### IV.B. Critères de Choix

**① SÉCURITÉ ET CONFIDENTIALITÉ**

```
   DONNÉES SENSIBLES                     → Hébergement local ou cloud privé
   DONNÉES PUBLIQUES OU PEU SENSIBLES    → Cloud public acceptable
```

**② ACCESSIBILITÉ**

```
   ÉQUIPE NOMADE / TÉLÉTRAVAIL           → Cloud impératif
   ÉQUIPE SUR SITE UNIQUEMENT            → Serveur local suffisant
```

**③ COÛT**

```
   Budget limité                          → Serveur local (coût matériel)
   Budget flexible                        → Cloud public (abonnement)
```

**④ VOLUMÉTRIE**

```
   < 100 Go                               → Cloud public
   100 Go - 1 To                          → NAS ou cloud privé
   > 1 To                                 → Serveur fichiers dédié
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Travail collaboratif** | Méthodes et outils permettant à une équipe de travailler ensemble efficacement |
| **Wiki** | Site web collaboratif éditable par plusieurs utilisateurs |
| **DokuWiki** | Solution de wiki open source sans base de données |
| **Git** | Système de gestion de versions (VCS) |
| **Commit** | Enregistrement d'une version dans Git avec un message descriptif |
| **Dépôt (repository)** | Projet suivi par Git (contient l'historique des versions) |
| **GitHub** | Service d'hébergement de code en ligne utilisant Git |
| **Versioning** | Suivi des modifications successives d'un fichier |
| **NAS** | Network Attached Storage — boîtier de stockage réseau |
| **Cloud privé** | Service cloud hébergé et contrôlé par l'organisation |
| **Nextcloud** | Solution open source de cloud privé (équivalent Dropbox auto-hébergé) |

---

## ✅ Auto-évaluation

- [ ] J'explique les enjeux du travail collaboratif en IT
- [ ] J'identifie les 4 piliers de la collaboration (documentation, versioning, communication, tâches)
- [ ] Je définis ce qu'est un wiki et liste 3 cas d'usage
- [ ] Je compare DokuWiki, MediaWiki, Confluence
- [ ] J'installe DokuWiki sur Ubuntu Server
- [ ] Je crée une structure documentaire cohérente dans un wiki
- [ ] J'explique à quoi sert Git (versioning, collaboration)
- [ ] Je distingue Git et GitHub
- [ ] J'exécute les 4 commandes de base (init, add, commit, log)
- [ ] Je compare les solutions de partage documentaire

---

---

# 🖥️ TP PARTIE 1 — INSTALLER DOKUWIKI

*Durée : 60 minutes — Individuel*

---

## Objectif

Installer et configurer DokuWiki sur Ubuntu Server pour créer un wiki d'équipe IT.

---

## Prérequis

- VM Ubuntu Server 22.04 (peut réutiliser celle de S14-S15)
- Accès sudo
- Apache et PHP déjà installés (si LAMP installé en S14)

---

## ÉTAPE 1 — Vérifier/Installer Apache et PHP (10 min)

**Si Apache/PHP déjà installés (S14) :**

```bash
apache2 -v
php -v
# Si les deux fonctionnent, passer à l'étape 2
```

**Si Apache/PHP non installés :**

```bash
sudo apt update
sudo apt install apache2 php libapache2-mod-php php-xml php-gd php-zip -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

---

## ÉTAPE 2 — Télécharger DokuWiki (5 min)

```bash
cd /tmp
wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
tar -xzf dokuwiki-stable.tgz
```

**Identifier le dossier extrait :**

```bash
ls -l /tmp
# Vous verrez un dossier dokuwiki-2024-02-06 (la date varie)
```

---

## ÉTAPE 3 — Déplacer DokuWiki vers DocumentRoot (5 min)

```bash
# Remplacer 2024-02-06 par la date réelle du dossier
sudo mv /tmp/dokuwiki-2024-02-06 /var/www/dokuwiki

# Permissions
sudo chown -R www-data:www-data /var/www/dokuwiki
sudo chmod -R 755 /var/www/dokuwiki
```

---

## ÉTAPE 4 — Configurer VirtualHost Apache (10 min)

```bash
sudo nano /etc/apache2/sites-available/wiki.conf
```

**Contenu :**

```apache
<VirtualHost *:80>
    ServerName wiki.local
    DocumentRoot /var/www/dokuwiki

    <Directory /var/www/dokuwiki/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/wiki_error.log
    CustomLog ${APACHE_LOG_DIR}/wiki_access.log combined
</VirtualHost>
```

**Activer le site :**

```bash
sudo a2ensite wiki.conf
sudo systemctl reload apache2
```

---

## ÉTAPE 5 — Configurer /etc/hosts (Poste Client) (3 min)

**Sur votre poste client (pas le serveur) :**

**Windows :**
```cmd
# Notepad en Administrateur
# C:\Windows\System32\drivers\etc\hosts

192.168.X.X    wiki.local
```

**Linux/macOS :**
```bash
sudo nano /etc/hosts

192.168.X.X    wiki.local
```

---

## ÉTAPE 6 — Installation Web de DokuWiki (15 min)

**Ouvrir le navigateur :** http://wiki.local/install.php

**Configuration de DokuWiki :**

| **Champ** | **Valeur** |
|---|---|
| Nom du wiki | Wiki Technique IT |
| Compte superutilisateur | admin |
| Nom complet | Administrateur Wiki |
| Email | admin@wiki.local |
| Mot de passe | AdminWiki2024! |
| Politique ACL | Public (lecture publique, écriture utilisateurs) |
| Licence | CC BY-SA (Creative Commons) |

**Cliquer "Sauvegarder"**

**Résultat :** "DokuWiki est maintenant configuré."

---

## ÉTAPE 7 — Sécuriser l'Installation (5 min)

```bash
# Supprimer le fichier d'installation (obligatoire)
sudo rm /var/www/dokuwiki/install.php

# Vérifier
ls /var/www/dokuwiki/install.php
# Résultat : No such file or directory
```

---

## ÉTAPE 8 — Première Connexion (7 min)

**Navigateur :** http://wiki.local

**Se connecter :**
- Cliquer "Connexion" (en haut à droite)
- Identifiant : `admin`
- Mot de passe : `AdminWiki2024!`

**Explorer l'interface :**
- **Éditer** : Cliquer sur l'icône crayon pour éditer la page d'accueil
- **Créer une page** : Taper `[[:nouvelle_page]]` dans l'éditeur → enregistrer → cliquer sur le lien rouge

---

---

# 🖥️ TP PARTIE 2 — CRÉER LA STRUCTURE DU WIKI

*Durée : 40 minutes — Individuel*

---

## Objectif

Créer une structure documentaire cohérente pour un wiki d'équipe IT.

---

## ÉTAPE 1 — Créer l'Arborescence (15 min)

**Structure recommandée pour un wiki IT :**

```
Accueil
├── Serveurs
│   ├── Serveur Web (Apache)
│   ├── Serveur Base de Données (MySQL)
│   └── Serveur Fichiers
├── Réseau
│   ├── Architecture Réseau
│   ├── Configuration Switches
│   └── Configuration Firewall
├── Procédures
│   ├── Création Utilisateur AD
│   ├── Backup Serveurs
│   └── Déploiement WordPress
├── Incidents
│   └── Base d'Incidents Connus
└── Projet_1
    ├── Architecture
    ├── Planning
    └── Documentation Technique
```

**Créer les pages :**

1. Sur la page d'accueil, cliquer **Éditer**
2. Remplacer le contenu par :

```wiki
====== Wiki Technique IT ======

Bienvenue sur le wiki de l'équipe IT.

===== Navigation =====

  * [[serveurs:accueil|Serveurs]]
  * [[reseau:accueil|Réseau]]
  * [[procedures:accueil|Procédures]]
  * [[incidents:accueil|Incidents]]
  * [[projet_1:accueil|Projet 1]]

===== Guide d'utilisation =====

Pour créer une nouvelle page :
  - Tapez le lien entre doubles crochets : ''<nowiki>[[nom_de_la_page]]</nowiki>''
  - Cliquez sur le lien rouge
  - Rédigez le contenu
  - Enregistrez

===== Règles de rédaction =====

  * **Toujours dater** les procédures
  * **Toujours indiquer l'auteur**
  * **Utiliser des captures d'écran** pour les procédures techniques
  * **Mettre à jour** les procédures obsolètes
```

3. **Enregistrer**

---

## ÉTAPE 2 — Créer une Page "Serveurs" (10 min)

Cliquer sur le lien **Serveurs** (rouge) → Vous arrivez sur la page vide.

**Contenu de la page serveurs:accueil :**

```wiki
====== Serveurs ======

Liste des serveurs de l'infrastructure.

===== Serveurs de Production =====

^ Nom       ^ IP            ^ Rôle           ^ OS                  ^ Responsable ^
| SRV-WEB01 | 192.168.10.10 | Serveur Web    | Ubuntu Server 22.04 | Admin       |
| SRV-DB01  | 192.168.10.11 | Base de données| Ubuntu Server 22.04 | Admin       |
| SRV-DC01  | 192.168.10.20 | Contrôleur AD  | Windows Server 2022 | Admin       |

===== Documentation par Serveur =====

  * [[serveurs:srv-web01|SRV-WEB01 — Serveur Web Apache]]
  * [[serveurs:srv-db01|SRV-DB01 — Serveur Base de Données MySQL]]
  * [[serveurs:srv-dc01|SRV-DC01 — Contrôleur de Domaine AD]]
```

**Enregistrer**

---

## ÉTAPE 3 — Créer une Procédure Technique (15 min)

Aller sur la page d'accueil → cliquer **Procédures** → cliquer **Création Utilisateur AD**

**Contenu de la page procedures:creation_utilisateur_ad :**

```wiki
====== Procédure : Création d'un Utilisateur Active Directory ======

**Auteur :** Admin \\
**Date de création :** 2024-02-16 \\
**Dernière mise à jour :** 2024-02-16 \\
**Version :** 1.0

===== Objectif =====

Créer un compte utilisateur dans Active Directory et l'affecter à la bonne Unité Organisationnelle (OU).

===== Prérequis =====

  * Accès au serveur contrôleur de domaine (SRV-DC01)
  * Droits d'administration sur Active Directory
  * Informations de l'utilisateur (nom, prénom, service)

===== Durée Estimée =====

5 minutes

===== Procédure =====

==== 1. Ouvrir la console Active Directory ====

  - Sur SRV-DC01, ouvrir **Utilisateurs et ordinateurs Active Directory**
  - Menu Démarrer → Outils d'administration Windows → Utilisateurs et ordinateurs AD

==== 2. Sélectionner l'Unité Organisationnelle ====

  - Dérouler l'arborescence : **techcorp.local** → **Utilisateurs** → **[Service]**
  - Exemple : **techcorp.local/Utilisateurs/Comptabilite**

==== 3. Créer l'utilisateur ====

  - Clic droit sur l'OU → **Nouveau** → **Utilisateur**
  - Remplir les champs :
    * Prénom : Pierre
    * Nom : Durand
    * Nom d'ouverture de session : pdurand
  - Cliquer **Suivant**

==== 4. Définir le mot de passe ====

  - Mot de passe : **Pass@2024!** (temporaire)
  - Cocher : ☑ L'utilisateur doit changer le mot de passe à la prochaine ouverture de session
  - Cliquer **Suivant** → **Terminer**

==== 5. Validation ====

  - L'utilisateur apparaît dans l'OU sélectionnée
  - Double-cliquer sur l'utilisateur → vérifier les propriétés

===== Vérification =====

  - Tenter une connexion avec le compte créé depuis un poste client
  - L'utilisateur doit être invité à changer son mot de passe

===== Troubleshooting =====

^ Problème ^ Cause Probable ^ Solution ^
| "Le nom d'utilisateur existe déjà" | Login déjà utilisé | Choisir un login différent (ex : pdurand2) |
| "Mot de passe ne respecte pas les exigences" | Politique de MDP trop stricte | Utiliser 12+ caractères, maj+min+chiffres+symboles |

===== Historique des Versions =====

^ Version ^ Date       ^ Auteur ^ Modifications ^
| 1.0     | 2024-02-16 | Admin  | Création initiale |
```

**Enregistrer**

---

---

# 🖥️ BONUS — INTRODUCTION À GIT (DÉMO OPTIONNELLE)

*Durée : 10-15 minutes — Démonstration enseignant ou exercice avancé*

---

## Objectif

Comprendre les commandes de base de Git par la pratique.

---

## ÉTAPE 1 — Installer Git (2 min)

```bash
sudo apt install git -y
git --version
```

---

## ÉTAPE 2 — Configurer Git (3 min)

```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@exemple.com"
```

---

## ÉTAPE 3 — Créer un Projet et Initialiser Git (3 min)

```bash
mkdir mon-wiki-scripts
cd mon-wiki-scripts
git init
# Résultat : "Initialized empty Git repository"
```

---

## ÉTAPE 4 — Créer un Fichier et Faire un Commit (5 min)

```bash
# Créer un fichier
echo "# Scripts pour le Wiki IT" > README.md

# Vérifier l'état
git status
# Résultat : README.md en "Untracked files"

# Ajouter au suivi
git add README.md

# Vérifier l'état
git status
# Résultat : README.md en "Changes to be committed"

# Faire le commit
git commit -m "Ajout du fichier README"

# Voir l'historique
git log
```

---

## ÉTAPE 5 — Modifier et Faire un Second Commit (5 min)

```bash
# Modifier le fichier
echo "" >> README.md
echo "## Liste des scripts" >> README.md
echo "- backup.sh : Script de sauvegarde" >> README.md

# Voir les modifications
git diff README.md

# Ajouter et commiter
git add README.md
git commit -m "Ajout de la section liste des scripts"

# Voir l'historique complet
git log
# On voit maintenant 2 commits
```

---

---

# 📄 ANNEXE 1 — MODÈLE STRUCTURE WIKI IT

```
WIKI TECHNIQUE IT — STRUCTURE TYPE
═══════════════════════════════════════════════════════════════

📁 Accueil
   └─ Page d'accueil avec navigation principale

📁 Serveurs
   ├─ Accueil (tableau récapitulatif des serveurs)
   ├─ Serveur Web
   ├─ Serveur Base de Données
   ├─ Serveur Fichiers
   ├─ Serveur Active Directory
   └─ Serveur DHCP/DNS

📁 Réseau
   ├─ Architecture Réseau (schémas, plan d'adressage)
   ├─ Configuration Switches
   ├─ Configuration Firewall
   ├─ VLANs
   └─ Routage

📁 Procédures
   ├─ Active Directory
   │  ├─ Création utilisateur
   │  ├─ Création groupe
   │  └─ Gestion des GPO
   ├─ Serveurs
   │  ├─ Installation Apache
   │  ├─ Installation MySQL
   │  └─ Configuration DHCP
   └─ Réseau
      ├─ Configuration VLAN Cisco
      └─ Configuration VPN

📁 Incidents
   ├─ Base d'Incidents Connus (tableau)
   └─ Fiches incidents par catégorie

📁 Inventaire
   ├─ Serveurs (détails techniques)
   ├─ Équipements Réseau (switches, routeurs)
   └─ Postes Clients

📁 Projets
   ├─ Projet 1
   │  ├─ Architecture
   │  ├─ Planning
   │  ├─ Documentation Technique
   │  └─ Incidents et Résolutions
   └─ [Futurs projets]

📁 Veille Technologique
   ├─ Nouveautés 2024
   ├─ Articles Intéressants
   └─ Formations Recommandées
```

---

# 📄 ANNEXE 2 — CHEAT SHEET GIT (COMMANDES DE BASE)

```
═══════════════════════════════════════════════════════════════
                    GIT CHEAT SHEET — BASES
═══════════════════════════════════════════════════════════════

INITIALISATION
──────────────────────────────────────────────────────────────
git init                    # Créer un nouveau dépôt Git
git config --global user.name "Nom"     # Configurer nom
git config --global user.email "email"  # Configurer email


COMMANDES ESSENTIELLES
──────────────────────────────────────────────────────────────
git status                  # Voir l'état des fichiers
git add fichier.txt         # Ajouter un fichier au suivi
git add .                   # Ajouter tous les fichiers modifiés
git commit -m "Message"     # Enregistrer une version
git log                     # Voir l'historique des commits
git log --oneline           # Historique compact


VOIR LES MODIFICATIONS
──────────────────────────────────────────────────────────────
git diff                    # Voir les modifications non ajoutées
git diff fichier.txt        # Voir modifs d'un fichier précis
git show <commit-id>        # Voir le contenu d'un commit


ANNULER DES MODIFICATIONS
──────────────────────────────────────────────────────────────
git checkout fichier.txt    # Annuler les modifs d'un fichier
git reset HEAD fichier.txt  # Retirer un fichier du staging
git revert <commit-id>      # Annuler un commit (crée un nouveau)


WORKFLOW TYPIQUE
──────────────────────────────────────────────────────────────
1. Modifier des fichiers
2. git status               # Voir ce qui a changé
3. git add .                # Ajouter les modifications
4. git commit -m "..."      # Enregistrer la version
5. git log                  # Vérifier que le commit est là

═══════════════════════════════════════════════════════════════
```

---

*Pack S16 BLOC 1 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences : B3.3, B1.5, B3.4*
*Travail collaboratif · Wiki · DokuWiki · Git · Versioning · Partage documentaire*
