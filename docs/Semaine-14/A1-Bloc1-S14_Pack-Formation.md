# Pack de Formation - Semaine 14 (S14) - BLOC 1
## 🌐 Présence en Ligne · CMS · WordPress · LAMP Stack · TP Installation

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S14 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — quatorzième semaine |
| **Modalité** | Présentiel — salle TP (serveurs Linux ou VMs) |
| **Prérequis** | Notions Linux de base, S11 (documentation procédures), S13 (veille techno) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B2.1** | Installer et configurer un service réseau pour une TPE ou une PME | Maîtrise |
| **B1.5** | Mettre à disposition des utilisateurs un service informatique | Maîtrise |
| **B3.3** | Participer à la gestion et au suivi d'un projet | Acquisition |

> 📌 **S14 BLOC 1 est une séance charnière** qui fait le pont entre la gestion de services et l'administration système. Elle répond à un besoin métier concret (présence en ligne d'une organisation) par une réalisation technique complète (installation LAMP + WordPress). C'est souvent la **première fois** que les apprenants installent une stack complète de A à Z.

---

## Objectifs Pédagogiques

**Présence en ligne :**
- ✅ Expliquer pourquoi une organisation **doit** être présente en ligne (5 raisons business)
- ✅ Identifier les **types de présence** (site vitrine, e-commerce, blog, application web)
- ✅ Comparer les **coûts** (hébergement, développement, maintenance) et calculer le ROI
- ✅ Analyser des **exemples de sites** de PME

**CMS (Content Management Systems) :**
- ✅ Définir un **CMS** et expliquer ses avantages vs développement sur mesure
- ✅ Comparer les **principaux CMS** (WordPress, Joomla, Drupal, PrestaShop)
- ✅ Identifier les **cas d'usage** de chaque CMS
- ✅ Expliquer l'écosystème WordPress (thèmes, plugins, communauté)

**Stack LAMP :**
- ✅ Décrire l'architecture **LAMP** (Linux, Apache, MySQL, PHP)
- ✅ Expliquer le **rôle de chaque composant**
- ✅ Installer et configurer une **stack LAMP complète**
- ✅ Créer un **VirtualHost Apache** pour un site web
- ✅ Installer et configurer **WordPress** de A à Z
- ✅ Documenter la procédure d'installation (lien S11)

---

## ⭐ Spécificités Pédagogiques

### La Présence en Ligne : Un Besoin Métier Concret

**L'argument d'ouverture efficace :**

> *"En 2024, une PME sans site web est invisible. 87% des consommateurs recherchent une entreprise en ligne AVANT de la contacter. Si votre entreprise n'apparaît pas dans Google, elle n'existe pas pour la majorité de vos clients potentiels."*

### WordPress : Le CMS à Double Lecture

WordPress est perçu très différemment selon le public :
- **Pour les utilisateurs :** "C'est facile, avec des templates"
- **Pour les techniciens SISR :** "C'est une application PHP qui nécessite Apache, MySQL, des permissions, de la sécurisation..."

S14 BLOC 1 doit faire comprendre cette double lecture.

### Le TP LAMP : Une Première Complexe

Pour beaucoup d'apprenants, S14 est la **première installation multi-composants**. Il faut donc :
1. Fournir une procédure très détaillée
2. Prévoir du temps de débug (20-30% auront un blocage)
3. Avoir des solutions de contournement

### Lien avec l'Alternance

Beaucoup de PME clientes :
- N'ont pas de site web
- Ont un site obsolète
- Cherchent à moderniser leur présence

Un apprenant capable d'installer et de maintenir un WordPress est **immédiatement utile** en entreprise.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S13 | Feedback RFC et veille — partage info trouvée |
| H+0:10 | 15 min | 🎯 Découverte | Activité "La PME Invisible" |
| H+0:25 | 25 min | 📖 Cours | Présence en ligne : pourquoi, types, coûts, ROI |
| H+0:50 | 30 min | 📖 Cours | CMS : définition, comparatif, WordPress, LAMP |
| H+1:20 | **15 min** | ☕ **PAUSE** | — |
| H+1:35 | 10 min | 🔧 Démo | Démonstration installation LAMP par l'enseignant |
| H+1:45 | 90 min | 🖥️ **TP** | Installation complète LAMP + WordPress |
| H+3:15 | 15 min | ✅ Validation | Tests fonctionnels WordPress + sécurité de base |
| H+3:30 | 20 min | 📝 Documentation | Rédaction procédure (modèle S11) |

---

## Différenciation Pédagogique

### Profil Avancé
- Installation LAMP avec config optimisée (PHP-FPM, cache OpCache)
- Configurer HTTPS avec Let's Encrypt
- Créer script Bash d'installation automatique
- Installer WooCommerce (e-commerce)

### Profil Débutant
- Utiliser script d'installation semi-automatique fourni
- Compléter procédure pré-rédigée
- Binômage avec profil avancé
- Vérifier uniquement que le site est accessible

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **VM Ubuntu Server 22.04** | 1 par apprenant |
| **Accès sudo** | Root ou utilisateur avec droits sudo |
| **Connexion Internet** | Pour téléchargements |
| **Navigateur web** | Pour accéder au site WordPress |
| **Procédure d'installation** | Annexe 1 — imprimée ou affichée |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "La PME Invisible"

*Durée : 15 minutes — Collectif*

---

## Mise en Situation (5 min)

L'enseignant présente deux PME fictives concurrentes :

**PME A — "Plomberie Martin"**
- 15 salariés, créée en 1985
- Spécialiste chauffage et plomberie
- **Pas de site web** (juste une page Facebook rarement mise à jour)
- Publicité : annuaire papier Pages Jaunes
- CA : stable mais -5% par an depuis 3 ans

**PME B — "AquaTech Solutions"**
- 12 salariés, créée en 2018
- Même secteur, même ville
- **Site web professionnel** : 
  - Présentation services
  - Galerie réalisations avant/après
  - Formulaire devis en ligne (réponse 24h)
  - Blog avec conseils
  - Avis clients (4,8/5 sur Google)
- CA : +25% par an

**Question à la classe :**
> *"Vous avez une fuite d'eau urgente. Quelle entreprise allez-vous appeler ? Pourquoi ?"*

---

## Discussion Guidée (7 min)

| **Question** | **Concept visé** |
|---|---|
| "Pourquoi Plomberie Martin perd des clients malgré 40 ans d'expérience ?" | Invisibilité en ligne = perte de parts de marché |
| "Chercheriez-vous un plombier dans les Pages Jaunes papier ?" | Les usages ont changé |
| "Qu'apporte le site web à AquaTech que Martin n'a pas ?" | Crédibilité, disponibilité 24/7, preuve du savoir-faire |
| "Combien coûterait un site comme celui d'AquaTech ?" | 500-1000 €/an (WordPress + hébergement) |
| "Le site génère-t-il du CA ou c'est juste de la com' ?" | ROI mesurable : devis en ligne = clients = CA |

## Conclusion (3 min)

> *"En 2024, une PME sans site web perd 60% de ses clients potentiels avant même qu'ils ne l'appellent. Installer un site web, c'est un acte métier, pas juste de la technique."*

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Présence en Ligne · Enjeux pour une Organisation"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 14*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.5** | Mettre à disposition un service informatique |
| **B2.1** | Installer et configurer un service réseau |

---

## PARTIE I — Pourquoi une Organisation Doit Être en Ligne

### I.A. Les 5 Raisons Business

```
   ① VISIBILITÉ ET DÉCOUVRABILITÉ
   ──────────────────────────────────────────────────────────────
   87% des consommateurs recherchent une entreprise en ligne avant
   de la contacter (étude BrightLocal 2023).

   Sans site web :
   • Invisible sur Google (92% des Français)
   • Absent des comparateurs
   • Pas de fiche Google My Business complète

   Avec site web :
   • Référencement naturel (SEO) → trafic gratuit
   • Publicité ciblée (Google Ads)
   • Présence locale (Google Maps, avis)


   ② CRÉDIBILITÉ ET CONFIANCE
   ──────────────────────────────────────────────────────────────
   75% des internautes jugent la crédibilité d'une entreprise
   sur la qualité de son site web (Stanford 2023).

   Site web professionnel = signal de sérieux
   Pas de site = doute sur la fiabilité


   ③ DISPONIBILITÉ 24/7
   ──────────────────────────────────────────────────────────────
   Un site web ne ferme jamais :
   • Consultation services à 22h le dimanche
   • Formulaire de contact/devis en continu
   • FAQ répond sans mobiliser le standard

   ROI : 30% des demandes arrivent hors horaires bureau


   ④ OUTIL COMMERCIAL ET MARKETING
   ──────────────────────────────────────────────────────────────
   Le site web est un commercial qui ne dort jamais :
   • Présentation produits/services avec photos
   • Témoignages clients
   • Call-to-action (devis, achat, contact)
   • Mesure du ROI (Google Analytics)


   ⑤ RECRUTEMENT ET RÉPUTATION EMPLOYEUR
   ──────────────────────────────────────────────────────────────
   86% des candidats recherchent une entreprise en ligne avant
   de postuler (LinkedIn 2023).

   Site web employeur :
   • Présentation culture d'entreprise
   • Offres d'emploi visibles
   • Attractivité RH
```

---

### I.B. Les Types de Présence en Ligne

| **Type** | **Objectif** | **Contenu** | **Coût/an** | **Exemple** |
|---|---|---|---|---|
| **Site vitrine** | Présenter l'entreprise | 5-10 pages | 500-1500 € | Cabinet comptable |
| **Blog** | Partager expertise, SEO | Articles réguliers | 300-800 € | Cabinet avocat |
| **E-commerce** | Vendre en ligne | Catalogue, panier, paiement | 2000-5000 € | Boutique vêtements |
| **Application web** | Fournir service en ligne | Fonctionnalités interactives | 5000-20000 € | SaaS |
| **Portail intranet** | Centraliser infos employés | Documents RH, annuaire | 1000-3000 € | PME > 50 salariés |

> 📌 **90% des PME françaises n'ont besoin que d'un site vitrine ou d'un blog.**

---

### I.C. Coûts et ROI

**Coût d'un site web professionnel (WordPress) :**

```
   INVESTISSEMENT INITIAL
   ──────────────────────────────────────────────────────────────
   Nom de domaine (.fr, .com)           : 10-20 €/an
   Hébergement web                       : 50-150 €/an
   Thème WordPress premium (optionnel)   : 0-60 € (unique)
   Développement/configuration           : 500-2000 €
                                          OU 0 € (alternant)
   ───────────────────────────────────────────────────────────────
   TOTAL première année                  : 560-2230 €

   COÛT ANNUEL RÉCURRENT
   ──────────────────────────────────────────────────────────────
   Hébergement + domaine                 : 60-170 €/an
   Maintenance                           : 200-600 €/an
                                          OU 0 € (interne)
   ───────────────────────────────────────────────────────────────
   TOTAL années suivantes                : 60-770 €/an
```

**Calcul du ROI (exemple PME plomberie) :**

```
   Coût site : 1 000 € première année, puis 300 €/an

   Clients gagnés via le site : 15/an (sur 80 clients totaux)
   CA moyen par client : 800 €
   CA généré par le site : 15 × 800 € = 12 000 €

   ROI = (12 000 - 1 000) / 1 000 × 100 = 1 100%
   → Le site est rentabilisé dès la première année
```

---

## PARTIE II — Les CMS (Content Management Systems)

### II.A. Définition

Un **CMS** est un logiciel qui permet de créer, gérer et modifier le contenu d'un site web **sans coder**.

```
   SITE WEB CODÉ À LA MAIN          vs          SITE WEB AVEC CMS
   ────────────────────────────                ─────────────────────
   Modifier un texte :                         Modifier un texte :
   1. Ouvrir fichier HTML                      1. Se connecter au CMS
   2. Trouver la ligne                         2. Cliquer "Modifier"
   3. Modifier le code                         3. Taper le texte
   4. Sauvegarder                              4. Cliquer "Publier"
   5. Uploader via FTP                         → Fini
   6. Vider le cache
   → Nécessite compétences tech                → Accessible non-technicien
```

**Avantages d'un CMS :**
- ✅ Pas besoin de coder
- ✅ Interface graphique intuitive (WYSIWYG)
- ✅ Gestion utilisateurs et droits
- ✅ Thèmes pré-faits (design professionnel)
- ✅ Plugins/extensions (fonctionnalités sans développement)
- ✅ SEO facilité
- ✅ Communauté et support

**Inconvénients :**
- ❌ Performance inférieure à un site codé optimisé
- ❌ Failles de sécurité potentielles (plugins non à jour)
- ❌ Moins de flexibilité
- ❌ Dépendance à l'écosystème du CMS

---

### II.B. Comparatif des Principaux CMS

| **CMS** | **Part marché** | **Cas d'usage** | **Difficulté** | **Points forts** |
|---|---|---|---|---|
| **WordPress** | 43% | Site vitrine, blog, e-commerce | ★☆☆ | Facilité, communauté énorme |
| **Shopify** | E-commerce | E-commerce clé en main | ★☆☆ | Spécialisé vente, hébergement inclus |
| **Joomla** | 2,5% | Sites communautaires | ★★☆ | Flexibilité, gestion utilisateurs |
| **Drupal** | 1,5% | Sites entreprise complexes | ★★★ | Sécurité, performance |
| **PrestaShop** | E-commerce | E-commerce PME | ★★☆ | Gratuit, français, modules |
| **Wix** | Site builder | Sites très simples | ★☆☆ | Tout-en-un, drag & drop |

> 📌 **Choix S14 BLOC 1 :** WordPress représente 43% du web, il est open source, gratuit, très documenté.

---

### II.C. WordPress — Écosystème

**Chiffres clés (2024) :**
- 43% de tous les sites web utilisent WordPress
- 810 millions de sites WordPress actifs
- 60 000+ plugins gratuits
- 11 000+ thèmes gratuits
- Utilisé par : Microsoft News, Sony Music, NASA...

**Composants :**

```
   WORDPRESS.ORG (open source, auto-hébergé — ce qu'on installe)
   ──────────────────────────────────────────────────────────────
   Logiciel gratuit à télécharger et installer sur son serveur
   Contrôle total, personnalisation illimitée
   Nécessite : serveur, base de données, maintenance

   WORDPRESS.COM (SaaS, hébergé par Automattic)
   ──────────────────────────────────────────────────────────────
   Service en ligne clé en main
   Hébergement inclus, pas de maintenance technique
   Limité en personnalisation (sauf plan payant)

   THÈMES (apparence visuelle)
   ──────────────────────────────────────────────────────────────
   Gratuits : 11 000+ sur wordpress.org/themes
   Payants : ThemeForest (60 $)
   Installation : Apparence → Thèmes → Ajouter

   PLUGINS (extensions de fonctionnalités)
   ──────────────────────────────────────────────────────────────
   Exemples populaires :
   • Yoast SEO (référencement)
   • WooCommerce (e-commerce)
   • Contact Form 7 (formulaires)
   • Wordfence (sécurité)
   • Jetpack (backups, performance)
```

---

## PARTIE III — La Stack LAMP

### III.A. Architecture LAMP

**LAMP** = **Linux, Apache, MySQL, PHP** — stack d'hébergement web open source la plus répandue.

```
   ARCHITECTURE LAMP
   ──────────────────────────────────────────────────────────────

   CLIENT (Navigateur)
       │
       │ HTTP Request (http://monsite.local)
       ↓
   ┌──────────────────────────────────────────────────────────┐
   │  SERVEUR LINUX (Ubuntu Server 22.04)                     │
   │                                                           │
   │   ┌──────────────────────────────────────────────────┐   │
   │   │  APACHE (Serveur Web)                           │   │
   │   │  • Écoute port 80 (HTTP)                        │   │
   │   │  • Reçoit requête HTTP                          │   │
   │   │  • Identifie le VirtualHost                     │   │
   │   │  • Lit le fichier (.html, .php...)              │   │
   │   └────────────────┬─────────────────────────────┘   │
   │                    │                                  │
   │                    │ Si fichier .php                  │
   │                    ↓                                  │
   │   ┌──────────────────────────────────────────────────┐   │
   │   │  PHP (Langage de programmation)                 │   │
   │   │  • Interprète le code PHP                       │   │
   │   │  • Exécute les instructions                     │   │
   │   │  • Génère HTML dynamique                        │   │
   │   └────────────────┬─────────────────────────────┘   │
   │                    │                                  │
   │                    │ Si requête SQL (données)         │
   │                    ↓                                  │
   │   ┌──────────────────────────────────────────────────┐   │
   │   │  MySQL / MariaDB (Base de données)              │   │
   │   │  • Stocke les données (articles, users...)      │   │
   │   │  • Exécute requêtes SQL                         │   │
   │   │  • Renvoie résultats à PHP                      │   │
   │   └──────────────────────────────────────────────────┘   │
   │                                                           │
   └──────────────────────────────────────────────────────────┘
       │
       │ HTTP Response (HTML généré)
       ↓
   CLIENT (Affichage)
```

---

### III.B. Rôle de Chaque Composant

| **Composant** | **Rôle** | **Équivalent quotidien** |
|---|---|---|
| **Linux** | Système d'exploitation du serveur | Le bâtiment qui héberge le restaurant |
| **Apache** | Serveur web (traite requêtes HTTP) | Le serveur qui prend commandes et sert |
| **MySQL** | Base de données (stocke contenus) | La cuisine où sont stockés les ingrédients |
| **PHP** | Langage de programmation | Le cuisinier qui prépare les plats |

---

### III.C. Alternatives à LAMP

| **Stack** | **Composants** | **Usage** |
|---|---|---|
| **LEMP** | Linux + **Nginx** + MySQL + PHP | Alternative moderne (meilleure perf) |
| **WAMP** | **Windows** + Apache + MySQL + PHP | Développement Windows |
| **MEAN** | MongoDB + Express + Angular + **Node.js** | Stack JavaScript full-stack |

---

## IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Présence en ligne** | Ensemble des canaux numériques de visibilité d'une organisation |
| **CMS** | Content Management System — logiciel de gestion de contenu web |
| **WordPress** | CMS open source le plus utilisé (43% des sites web) |
| **LAMP** | Linux + Apache + MySQL + PHP — stack d'hébergement web |
| **Apache** | Serveur web open source |
| **MySQL** | Système de gestion de base de données relationnelle |
| **PHP** | Langage de programmation côté serveur |
| **VirtualHost** | Configuration Apache pour héberger plusieurs sites |
| **Thème** | Template définissant l'apparence d'un site WordPress |
| **Plugin** | Extension ajoutant des fonctionnalités à WordPress |
| **SEO** | Search Engine Optimization — optimisation pour moteurs |
| **ROI** | Return On Investment — retour sur investissement |

---

## ✅ Auto-évaluation

- [ ] J'explique 3 raisons pour lesquelles une PME doit avoir un site web
- [ ] Je compare site vitrine, blog et e-commerce
- [ ] Je calcule le ROI d'un site web
- [ ] Je définis un CMS et cite ses avantages
- [ ] Je compare WordPress, Joomla, Drupal
- [ ] J'explique l'architecture LAMP et le rôle de chaque composant
- [ ] J'installe une stack LAMP complète
- [ ] Je configure un VirtualHost Apache
- [ ] J'installe WordPress et le configure

---

---

# 🖥️ TP — INSTALLATION WORDPRESS SUR LAMP

*Durée : 90 minutes — Individuel*

---

## Objectif

Installer et configurer une stack LAMP (Linux Apache MySQL PHP) sur Ubuntu Server 22.04, puis installer WordPress.

---

## Prérequis

- VM Ubuntu Server 22.04 LTS installée
- Accès sudo
- Connexion Internet
- Accès SSH ou console directe

---

## PHASE 1 — Installation de la Stack LAMP (30 min)

### 1.1. Mise à Jour du Système (3 min)

```bash
sudo apt update
sudo apt upgrade -y
```

---

### 1.2. Installation d'Apache (5 min)

```bash
# Installer Apache2
sudo apt install apache2 -y

# Vérifier l'installation
apache2 -v

# Démarrer et activer Apache
sudo systemctl start apache2
sudo systemctl enable apache2

# Vérifier le statut
sudo systemctl status apache2
```

**Test navigateur :**
- Ouvrir http://[IP_DU_SERVEUR]
- Page **"Apache2 Ubuntu Default Page"** doit s'afficher

> 💡 **Trouver l'IP :** `ip addr show` ou `hostname -I`

---

### 1.3. Installation de MySQL (8 min)

```bash
# Installer MySQL Server
sudo apt install mysql-server -y

# Vérifier
mysql --version

# Sécuriser MySQL
sudo mysql_secure_installation
```

**Assistant mysql_secure_installation :**

```
VALIDATE PASSWORD COMPONENT ? [y/N]
→ N (pour simplifier en TP)

New password: 
→ MotDePasseMySQL2024!

Remove anonymous users? [Y/n] → Y
Disallow root login remotely? [Y/n] → Y
Remove test database? [Y/n] → Y
Reload privilege tables? [Y/n] → Y
```

**Créer la base WordPress :**

```bash
sudo mysql -u root -p
# Entrer : MotDePasseMySQL2024!
```

**Dans MySQL :**

```sql
CREATE DATABASE wordpress_db;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'WpPass2024!';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

**Vérification :**

```bash
mysql -u wp_user -p
# Entrer : WpPass2024!

SHOW DATABASES;
# wordpress_db doit apparaître

EXIT;
```

---

### 1.4. Installation de PHP (5 min)

```bash
# Installer PHP et modules WordPress
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y

# Vérifier
php -v
```

**Tester PHP :**

```bash
sudo nano /var/www/html/info.php
```

**Contenu :**

```php
<?php
phpinfo();
?>
```

**Sauvegarder :** Ctrl+O → Entrée → Ctrl+X

**Test navigateur :**
- http://[IP_DU_SERVEUR]/info.php
- Page **PHP Version 8.1.XX** doit s'afficher

> ⚠️ **Supprimer après test :**
> ```bash
> sudo rm /var/www/html/info.php
> ```

---

## PHASE 2 — Installation de WordPress (20 min)

### 2.1. Télécharger WordPress (3 min)

```bash
cd /tmp
wget https://fr.wordpress.org/latest-fr_FR.tar.gz
tar -xzvf latest-fr_FR.tar.gz
```

---

### 2.2. Déplacer WordPress (5 min)

```bash
sudo mkdir -p /var/www/monsite
sudo cp -r /tmp/wordpress/* /var/www/monsite/

# Permissions
sudo chown -R www-data:www-data /var/www/monsite
sudo chmod -R 755 /var/www/monsite
```

---

### 2.3. Configurer VirtualHost Apache (8 min)

```bash
sudo nano /etc/apache2/sites-available/monsite.conf
```

**Contenu :**

```apache
<VirtualHost *:80>
    ServerName monsite.local
    ServerAdmin admin@monsite.local
    DocumentRoot /var/www/monsite

    <Directory /var/www/monsite/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/monsite_error.log
    CustomLog ${APACHE_LOG_DIR}/monsite_access.log combined
</VirtualHost>
```

**Activer :**

```bash
sudo a2dissite 000-default.conf
sudo a2ensite monsite.conf
sudo a2enmod rewrite
sudo apache2ctl configtest
# Résultat : Syntax OK
sudo systemctl reload apache2
```

---

### 2.4. Configuration Fichier hosts (Poste Client)

**Windows :**
```cmd
# Notepad en Administrateur
# Ouvrir : C:\Windows\System32\drivers\etc\hosts

# Ajouter :
192.168.X.X    monsite.local
```

**Linux / macOS :**
```bash
sudo nano /etc/hosts

# Ajouter :
192.168.X.X    monsite.local
```

---

## PHASE 3 — Configuration WordPress (25 min)

### 3.1. Installation Web (15 min)

**Navigateur :** http://monsite.local

**Étape 1 — Langue :**
- Français → **Continuer**

**Étape 2 — Base de données :**

| **Champ** | **Valeur** |
|---|---|
| Nom base | `wordpress_db` |
| Identifiant | `wp_user` |
| Mot de passe | `WpPass2024!` |
| Adresse | `localhost` |
| Préfixe | `wp_` |

- **Valider** → **Lancer l'installation**

**Étape 3 — Informations site :**

| **Champ** | **Valeur** |
|---|---|
| Titre | Mon Site WordPress |
| Identifiant | admin |
| Mot de passe | AdminWP2024! |
| Email | admin@monsite.local |
| Moteurs recherche | ☐ Décocher |

- **Installer WordPress**

**Résultat :** "Bravo ! WordPress a été installé."

---

### 3.2. Connexion Tableau de Bord (3 min)

- **Se connecter**
- Identifiant : `admin`
- Mot de passe : `AdminWP2024!`

---

### 3.3. Configuration de Base (7 min)

**Réglages → Général :**
- Slogan : "Site de démonstration"
- **Enregistrer**

**Réglages → Permaliens :**
- ☑ **Titre de la publication**
- **Enregistrer**

**Apparence → Thèmes :**
- Activer un thème (ex : Twenty Twenty-Four)

**Créer page :**
- Pages → Ajouter
- Titre : "Bienvenue"
- Contenu : "Premier site WordPress sur LAMP"
- **Publier**

**Créer article :**
- Articles → Ajouter
- Titre : "Premier article"
- Contenu : "Installation réussie"
- **Publier**

---

## PHASE 4 — Validation (15 min)

### 4.1. Tests Fonctionnels

| **Test** | **Procédure** | **Résultat attendu** |
|---|---|---|
| **Page d'accueil** | http://monsite.local | Site s'affiche |
| **Article** | Cliquer "Premier article" | Article s'affiche |
| **Page** | http://monsite.local/bienvenue | Page s'affiche |
| **Admin** | http://monsite.local/wp-admin | Tableau de bord |
| **Upload** | Médias → Ajouter image | Image dans bibliothèque |

---

### 4.2. Vérifications Système

```bash
# Logs Apache
sudo tail -n 50 /var/log/apache2/monsite_error.log

# MySQL actif
sudo systemctl status mysql

# Apache actif
sudo systemctl status apache2

# Espace disque
df -h
```

---

### 4.3. Sécurité de Base

```bash
sudo nano /var/www/monsite/wp-config.php
```

**Ajouter avant `/* C'est tout */` :**

```php
// Désactiver l'éditeur de fichiers
define('DISALLOW_FILE_EDIT', true);
```

**Vérification :** Dans WordPress, Apparence → l'éditeur a disparu.

---

## PHASE 5 — Documentation (Lien S11)

Rédiger une **procédure d'installation** selon modèle S11 :

1. **Objectif** : Installer WordPress sur Ubuntu avec LAMP
2. **Prérequis** : VM Ubuntu, sudo, Internet
3. **Étapes** : Phases 1 à 4 avec commandes clés
4. **Troubleshooting** : 2 erreurs courantes
5. **Résultat** : Captures site fonctionnel

---

---

# 📄 ANNEXE 1 — TROUBLESHOOTING

---

## Erreur 1 — "Impossible d'établir connexion BDD"

**Symptôme :** Message d'erreur WordPress.

**Causes :**
1. MySQL n'est pas démarré
2. Identifiants incorrects
3. Base non créée

**Solutions :**

```bash
# Vérifier MySQL
sudo systemctl status mysql
sudo systemctl start mysql

# Tester connexion
mysql -u wp_user -p
SHOW DATABASES;
# wordpress_db doit apparaître
```

---

## Erreur 2 — "403 Forbidden"

**Symptôme :** Erreur 403 ou page blanche.

**Causes :**
1. Permissions incorrectes
2. VirtualHost mal configuré

**Solutions :**

```bash
# Permissions
sudo chown -R www-data:www-data /var/www/monsite
sudo chmod -R 755 /var/www/monsite

# Vérifier VirtualHost
sudo apache2ctl -S

# Logs
sudo tail -f /var/log/apache2/monsite_error.log
```

---

## Erreur 3 — "404 Not Found" permaliens

**Symptôme :** Page d'accueil OK, mais articles 404.

**Cause :** Module rewrite non activé.

**Solution :**

```bash
sudo a2enmod rewrite
sudo systemctl reload apache2
```

---

## Erreur 4 — PHP non interprété

**Symptôme :** Code PHP affiché en texte.

**Solution :**

```bash
# Vérifier module PHP
apachectl -M | grep php

# Réinstaller si absent
sudo apt install libapache2-mod-php -y
sudo systemctl restart apache2
```

---

# 📄 ANNEXE 2 — CHECKLIST SÉCURITÉ

---

| **Mesure** | **Procédure** | **✓** |
|---|---|---|
| **MDP admin fort** | 12+ caractères | ☐ |
| **Utilisateur != "admin"** | Créer nouvel admin, supprimer "admin" | ☐ |
| **Désactiver éditeur** | `DISALLOW_FILE_EDIT` dans wp-config.php | ☐ |
| **Supprimer thèmes/plugins inutilisés** | Apparence/Extensions → Supprimer | ☐ |
| **Mises à jour** | Tableau de bord → Mises à jour | ☐ |
| **Wordfence** | Extensions → Ajouter → Wordfence | ☐ |
| **Limiter tentatives connexion** | Plugin : Limit Login Attempts | ☐ |
| **Sauvegardes auto** | Plugin : UpdraftPlus | ☐ |
| **HTTPS** | Let's Encrypt (certbot) | ☐ |
| **Cacher version WP** | `remove_action('wp_head', 'wp_generator')` | ☐ |

---

*Pack S14 BLOC 1 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences : B2.1, B1.5, B3.3*
*Présence en ligne · CMS · WordPress · LAMP · Apache · MySQL · PHP*
