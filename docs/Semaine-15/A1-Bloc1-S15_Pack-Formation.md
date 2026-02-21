# Pack de Formation - Semaine 15 (S15) - BLOC 1
## 🔒 HTTPS · SEO Basique · Portfolio E4 #2 · Évaluation Formative 2

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S15 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — quinzième semaine |
| **Modalité** | Présentiel — salle TP + évaluation |
| **Prérequis** | S14 BLOC 1 (WordPress + LAMP), notions Active Directory, DHCP, DNS |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.5** | Mettre à disposition des utilisateurs un service informatique | Maîtrise |
| **B2.1** | Installer et configurer un service réseau | Maîtrise |
| **B3.3** | Participer à la gestion et au suivi d'un projet (portfolio) | Maîtrise |
| **B2.2** | Installer et configurer des éléments d'infrastructure (éval) | Évaluation |

> 📌 **S15 BLOC 1 est une séance triple** : (1) complète le sujet présence en ligne avec sécurisation HTTPS et visibilité SEO ; (2) formalise une deuxième situation professionnelle pour le portfolio E4 ; (3) évalue les acquis techniques via un TP intégré.

---

## Objectifs Pédagogiques

**Présence en ligne (suite) :**
- ✅ Expliquer l'importance du **HTTPS** (sécurité, confiance, SEO)
- ✅ Installer un **certificat SSL/TLS** (Let's Encrypt)
- ✅ Configurer Apache pour **forcer HTTPS** (redirection HTTP → HTTPS)
- ✅ Comprendre les **bases du référencement** (SEO on-page)
- ✅ Configurer les **méta-données** WordPress (titre, description, sitemap)
- ✅ Utiliser **Google Search Console** pour indexer son site

**Portfolio E4 #2 :**
- ✅ Identifier une **situation professionnelle** vécue en entreprise
- ✅ Rédiger une **fiche descriptive** structurée (contexte, mission, réalisation, résultats)
- ✅ Sélectionner les **preuves** appropriées (captures, documents, schémas)
- ✅ Respecter la **méthodologie de formalisation** pour l'épreuve E4

**Évaluation formative 2 :**
- ✅ Mettre en œuvre une infrastructure complète (AD, DNS, DHCP, droits)
- ✅ Diagnostiquer et résoudre des problèmes techniques
- ✅ Documenter ses actions

---

## ⭐ Spécificités Pédagogiques

### La Séance Triple : Gestion du Temps

S15 BLOC 1 traite de **trois sujets différents** en 4 heures. C'est ambitieux. Organisation recommandée :
- **1h15** : HTTPS + SEO (cours + TP guidé)
- **1h** : Portfolio E4 #2 (atelier guidé)
- **1h30** : Évaluation formative 2 (TP noté individuel)
- **15 min** : Pause

### HTTPS : Certificat Auto-signé vs Let's Encrypt

En environnement de TP local (sans nom de domaine public), on ne peut pas utiliser Let's Encrypt. Deux options :
1. **Certificat auto-signé** : rapide, mais navigateur affiche avertissement de sécurité
2. **Démonstration Let's Encrypt** : l'enseignant montre sur un vrai domaine, les apprenants suivent la théorie

**Choix pédagogique S15 :** Certificat auto-signé pour le TP (les apprenants font), puis explication Let's Encrypt (théorie).

### Portfolio E4 : Accompagnement Indispensable

Beaucoup d'apprenants ont du mal à formaliser leurs situations professionnelles. Ils ne savent pas :
- Quelle situation choisir
- Comment la décrire
- Quelles preuves joindre

**Solution :** Atelier guidé avec :
1. Grille de sélection des situations
2. Modèle de fiche à remplir
3. Exemples de bonnes et mauvaises fiches
4. Relecture croisée entre pairs

### Évaluation Formative 2 : Positionnement Mi-Année

S15 marque la **fin de la Phase 3** (S11-S15). L'évaluation formative 2 permet de vérifier les acquis avant d'entrer en Phase 4 (projet de synthèse S16-S20).

**Ce qui est évalué :**
- Compétences BLOC 2 : AD, DNS, DHCP, droits (vues en S11-S13 dans le BLOC 2)
- Compétences transversales : documentation, diagnostic, autonomie

**Format :** TP pratique 1h30, noté, individuel, sur infrastructure Windows Server.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S14 | Feedback installation WordPress — sites fonctionnels |
| H+0:10 | 15 min | 🎯 Découverte | Activité "Le Site Non Sécurisé" |
| H+0:25 | 20 min | 📖 Cours | HTTPS : pourquoi, certificat SSL/TLS, Let's Encrypt |
| H+0:45 | 30 min | 🖥️ **TP Part. 1** | Installer certificat auto-signé + forcer HTTPS |
| H+1:15 | 15 min | 📖 Cours | SEO basique : balises meta, sitemap, Search Console |
| H+1:30 | 15 min | 🖥️ **TP Part. 2** | Configurer SEO WordPress (Yoast SEO) |
| H+1:45 | **15 min** | ☕ **PAUSE** | — |
| H+2:00 | 15 min | 📖 Méthodo | Portfolio E4 : structure, sélection situations |
| H+2:15 | 45 min | ✍️ **Atelier** | Formalisation guidée situation professionnelle #2 |
| H+3:00 | **90 min** | 📝 **ÉVAL** | Évaluation formative 2 — TP intégré (noté) |

---

## Différenciation Pédagogique

### Profil Avancé
- **HTTPS :** Configurer HSTS, Perfect Forward Secrecy
- **SEO :** Audit complet avec outils (Screaming Frog, GTmetrix)
- **Portfolio :** Rédiger 2 situations professionnelles
- **Éval :** Exercices bonus (réplication AD, zones DNS conditionnelles)

### Profil Débutant
- **HTTPS :** Suivre procédure pas-à-pas fournie
- **SEO :** Remplir uniquement titre et description
- **Portfolio :** Utiliser modèle pré-rempli à compléter
- **Éval :** Temps supplémentaire si nécessaire, aide documentation

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **VM Ubuntu Server** | Avec WordPress installé (S14) |
| **Modèle fiche E4** | Annexe 1 — format Word/PDF |
| **Grille sélection situations** | Annexe 2 |
| **Sujet évaluation formative 2** | Annexe 3 — énoncé + grille notation |
| **Infrastructure Windows** | Pour évaluation : Windows Server + clients |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "Le Site Non Sécurisé"

*Durée : 15 minutes — Collectif*

---

## Démonstration (8 min)

L'enseignant ouvre deux sites web en parallèle sur vidéoprojecteur :

**Site A — HTTP (non sécurisé) :**
- URL : http://exemple-non-securise.com
- Le navigateur affiche : **"Non sécurisé"** dans la barre d'adresse
- Icône : 🔓 (cadenas ouvert ou point d'exclamation)

**Site B — HTTPS (sécurisé) :**
- URL : https://exemple-securise.com
- Le navigateur affiche : **🔒** (cadenas fermé vert)
- Message : "Connexion sécurisée"

**L'enseignant montre :**
1. Clic sur le cadenas → informations certificat
2. "Le site est sûr. Votre connexion à ce site est chiffrée."

---

## Questions Guidées (5 min)

| **Question** | **Concept visé** |
|---|---|
| "Quelle différence voyez-vous entre les deux sites ?" | HTTP vs HTTPS dans la barre d'adresse |
| "Sur lequel saisiriez-vous vos coordonnées bancaires ?" | HTTPS = confiance pour données sensibles |
| "Que signifie le 'S' de HTTPS ?" | Secure — connexion chiffrée |
| "Pensez-vous que les internautes font attention au cadenas ?" | 84% des utilisateurs quittent un site non HTTPS (GlobalSign 2023) |

## Conclusion (2 min)

**L'enseignant écrit au tableau :**

```
   POURQUOI PASSER EN HTTPS ?

   ① SÉCURITÉ : Données chiffrées (impossible à intercepter)
   ② CONFIANCE : 84% des utilisateurs quittent un site HTTP
   ③ SEO : Google favorise les sites HTTPS dans les résultats
   ④ OBLIGATOIRE : Pour e-commerce, formulaires, connexion
```

> *"Aujourd'hui, un site web professionnel DOIT être en HTTPS. C'est gratuit (Let's Encrypt), rapide à installer (30 min), et indispensable."*

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "HTTPS · Certificat SSL/TLS · Sécurisation du Site"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 15*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.5** | Mettre à disposition un service informatique sécurisé |
| **B2.1** | Installer et configurer un service réseau (HTTPS) |

---

## PARTIE I — Qu'est-ce que HTTPS ?

### I.A. Définition

**HTTPS** = **HTTP Secure** = protocole HTTP + couche de chiffrement SSL/TLS.

```
   HTTP (non sécurisé)              vs         HTTPS (sécurisé)
   ───────────────────────                    ─────────────────────
   http://monsite.com                         https://monsite.com
   Port 80                                    Port 443
   Données en clair                           Données chiffrées
   🔓 Non sécurisé                            🔒 Connexion sécurisée

   RISQUE HTTP :
   ───────────────────────────────────────────────────────────────
   Attaque "Man-in-the-Middle" (MITM) :
   • L'attaquant intercepte la communication
   • Il peut lire les données (mots de passe, CB...)
   • Il peut modifier les données

   PROTECTION HTTPS :
   ───────────────────────────────────────────────────────────────
   • Chiffrement de bout en bout
   • Impossible de lire ou modifier les données
   • Authentification du serveur (certificat)
```

---

### I.B. Les 3 Raisons d'Activer HTTPS

**① SÉCURITÉ DES DONNÉES**

Toutes les données échangées entre le navigateur et le serveur sont **chiffrées** :
- Mots de passe
- Coordonnées bancaires
- Données personnelles
- Contenu des formulaires

> 💡 **Sans HTTPS :** Un attaquant sur le même réseau WiFi (café, gare) peut intercepter toutes les données.

**② CONFIANCE DES UTILISATEURS**

```
   STATISTIQUES (GlobalSign 2023)
   ───────────────────────────────────────────────────────────────
   • 84% des utilisateurs quittent un site non HTTPS
   • 77% craignent que leurs données soient volées sur HTTP
   • 65% ne font pas confiance à un site avec avertissement sécurité
```

Le **cadenas vert** 🔒 est devenu un symbole de confiance. Son absence = perte de crédibilité.

**③ RÉFÉRENCEMENT (SEO)**

Depuis 2014, Google favorise les sites HTTPS dans ses résultats de recherche.

```
   IMPACT SEO
   ───────────────────────────────────────────────────────────────
   Site HTTP : Pénalité de classement
   Site HTTPS : Bonus de classement

   Exemple :
   • Deux sites identiques (contenu, qualité)
   • Site A en HTTP → Position 8 sur Google
   • Site B en HTTPS → Position 4 sur Google
```

Depuis 2018, Chrome affiche **"Non sécurisé"** pour tous les sites HTTP → impact direct sur le taux de clic.

---

## PARTIE II — Le Certificat SSL/TLS

### II.A. Qu'est-ce qu'un Certificat ?

Un **certificat SSL/TLS** est un fichier électronique qui :
1. Prouve l'**identité** du site web (authentification)
2. Contient la **clé publique** permettant le chiffrement

```
   ANALOGIE : CARTE D'IDENTITÉ
   ───────────────────────────────────────────────────────────────
   Certificat SSL = Carte d'identité du site web

   Contient :
   • Nom du site (monsite.com)
   • Nom de l'organisation (Entreprise ABC)
   • Période de validité (du 01/01/2024 au 01/01/2025)
   • Clé publique (pour chiffrer les échanges)
   • Signature de l'autorité de certification (CA)

   Délivré par :
   • Autorité de Certification (CA) reconnue
   • Ex : Let's Encrypt, DigiCert, Sectigo...
```

---

### II.B. Les Types de Certificats

| **Type** | **Validation** | **Affichage** | **Usage** | **Prix** |
|---|---|---|---|---|
| **DV** (Domain Validation) | Automatique (propriété domaine) | 🔒 + nom domaine | Blog, site vitrine PME | 0-50 €/an |
| **OV** (Organization Validation) | Vérification entreprise | 🔒 + nom entreprise | Site corporate | 100-300 €/an |
| **EV** (Extended Validation) | Vérification approfondie | 🔒 + barre verte + nom entreprise | E-commerce, banques | 300-1000 €/an |

> 📌 **Pour 90% des sites web :** Un certificat **DV gratuit** (Let's Encrypt) suffit largement.

---

### II.C. Let's Encrypt — Le Certificat Gratuit

**Let's Encrypt** est une autorité de certification (CA) qui délivre des certificats SSL/TLS **gratuits** depuis 2015.

```
   CARACTÉRISTIQUES
   ───────────────────────────────────────────────────────────────
   ✅ Gratuit (100%)
   ✅ Automatisé (installation en 5 minutes)
   ✅ Reconnu par tous les navigateurs
   ✅ Renouvellement automatique tous les 90 jours
   ✅ Utilisé par 400 millions de sites web

   FONCTIONNEMENT
   ───────────────────────────────────────────────────────────────
   1. Installation de Certbot (client Let's Encrypt)
   2. Certbot contacte Let's Encrypt
   3. Let's Encrypt vérifie que vous possédez le domaine
   4. Délivrance du certificat
   5. Configuration automatique d'Apache/Nginx
   6. Renouvellement auto tous les 90 jours
```

**Commande d'installation (production) :**

```bash
# Ubuntu avec Apache
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d monsite.com -d www.monsite.com
```

> ⚠️ **Limitation :** Let's Encrypt nécessite un **nom de domaine public** accessible depuis Internet. En TP local (monsite.local), on utilise un **certificat auto-signé**.

---

## PARTIE III — Certificat Auto-signé (TP Local)

### III.A. Pourquoi Auto-signé en TP ?

En environnement de TP avec un domaine local (`monsite.local`), Let's Encrypt ne fonctionne pas car :
- Le domaine n'existe pas publiquement
- Let's Encrypt ne peut pas vérifier la propriété

**Solution :** Créer un **certificat auto-signé** — un certificat que l'on génère soi-même.

```
   CERTIFICAT AUTO-SIGNÉ
   ───────────────────────────────────────────────────────────────
   Avantages :
   ✅ Fonctionne en local
   ✅ Chiffrement actif (données protégées)
   ✅ Gratuit, rapide (5 min)

   Inconvénients :
   ❌ Navigateur affiche un avertissement de sécurité
   ❌ "Connexion non privée" / "Certificat non valide"
   ❌ Inutilisable en production

   Usage :
   • Tests locaux
   • Développement
   • Environnement de TP
```

---

### III.B. Créer un Certificat Auto-signé

**Commande OpenSSL :**

```bash
# Générer une clé privée + certificat auto-signé valide 365 jours
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/monsite.key \
  -out /etc/ssl/certs/monsite.crt
```

**Réponses aux questions :**
```
Country Name (2 letter code) [AU]: FR
State or Province Name [Some-State]: Occitanie
Locality Name []: Toulouse
Organization Name []: Mon Entreprise
Organizational Unit Name []: IT
Common Name (FQDN) []: monsite.local
Email Address []: admin@monsite.local
```

> 📌 **Common Name** = nom de domaine exact du site (`monsite.local`).

---

## PARTIE IV — Référencement Basique (SEO)

### IV.A. Qu'est-ce que le SEO ?

**SEO** = **Search Engine Optimization** = ensemble de techniques pour améliorer la visibilité d'un site dans les résultats de recherche (Google, Bing...).

```
   OBJECTIF SEO
   ───────────────────────────────────────────────────────────────
   Être en 1ère page de Google sur les mots-clés stratégiques

   Exemple : PME plomberie à Toulouse
   ───────────────────────────────────────────────────────────────
   Mots-clés cibles :
   • "plombier Toulouse"
   • "dépannage plomberie Toulouse"
   • "chauffagiste Toulouse urgence"

   Sans SEO : Position 50+ → 0 visiteur
   Avec SEO : Position 1-5 → 100+ visiteurs/mois → clients
```

**Les 3 Piliers du SEO :**

| **Pilier** | **Description** | **Exemples** |
|---|---|---|
| **SEO On-Page** | Optimisation du contenu et du code du site | Titres, méta-descriptions, balises H1/H2, images |
| **SEO Off-Page** | Liens externes pointant vers le site (backlinks) | Articles invités, annuaires, réseaux sociaux |
| **SEO Technique** | Performance et structure technique | Vitesse, mobile-friendly, sitemap, HTTPS |

> 📌 **S15 BLOC 1 se concentre sur le SEO On-Page** (le plus accessible pour un débutant).

---

### IV.B. SEO On-Page — Les Bases

**① Balise TITLE (titre de la page)**

```html
<head>
  <title>Plombier Toulouse | Dépannage 24/7 | AquaTech</title>
</head>
```

- Apparaît dans l'onglet du navigateur
- Apparaît dans les résultats Google (en bleu, cliquable)
- **50-60 caractères max**
- Doit contenir les **mots-clés principaux**

**② Meta DESCRIPTION**

```html
<meta name="description" content="Plombier à Toulouse disponible 24/7. Dépannage urgent, installation chaudière, débouchage. Devis gratuit en 2h. ☎ 05 XX XX XX XX">
```

- Apparaît dans les résultats Google (texte gris sous le titre)
- **150-160 caractères max**
- Doit donner **envie de cliquer**
- Contient les mots-clés + appel à l'action

**③ Balises H1, H2, H3 (titres hiérarchiques)**

```html
<h1>Plombier à Toulouse — Intervention Rapide 24/7</h1>

<h2>Nos Services de Plomberie</h2>
<h3>Dépannage d'urgence</h3>
<h3>Installation de chaudière</h3>

<h2>Pourquoi Nous Choisir ?</h2>
```

- **H1** : titre principal de la page (1 seul par page)
- **H2** : sections principales
- **H3** : sous-sections

> 💡 Google utilise ces balises pour comprendre la structure du contenu.

**④ Attribut ALT des images**

```html
<img src="plombier-toulouse.jpg" alt="Plombier intervenant sur une fuite d'eau à Toulouse">
```

- Décrit l'image pour Google (qui ne "voit" pas les images)
- Améliore l'accessibilité (lecteurs d'écran pour malvoyants)
- Mots-clés pertinents

---

### IV.C. Sitemap XML

Un **sitemap** est un fichier XML listant toutes les pages du site pour faciliter l'indexation par Google.

**Exemple sitemap.xml :**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://monsite.com/</loc>
    <lastmod>2024-02-16</lastmod>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://monsite.com/services</loc>
    <lastmod>2024-02-15</lastmod>
    <priority>0.8</priority>
  </url>
</urlset>
```

**WordPress génère automatiquement un sitemap :** `https://monsite.com/sitemap.xml`

**Soumettre le sitemap à Google :**
1. Aller sur **Google Search Console**
2. Ajouter son site
3. Indexation → Sitemaps → Ajouter sitemap.xml

---

### IV.D. Google Search Console

**Google Search Console** est un outil gratuit de Google pour suivre le référencement de son site.

**Fonctionnalités :**
- Soumettre son sitemap
- Voir les mots-clés qui amènent des visiteurs
- Identifier les erreurs d'indexation
- Vérifier la compatibilité mobile
- Suivre les backlinks

**Inscription :**
1. Aller sur https://search.google.com/search-console
2. Ajouter son site (propriété)
3. Vérifier la propriété (ajout balise HTML ou fichier)

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **HTTPS** | HTTP Secure — protocole HTTP avec chiffrement SSL/TLS |
| **SSL/TLS** | Protocoles de chiffrement des communications web |
| **Certificat SSL** | Fichier prouvant l'identité d'un site et permettant le chiffrement |
| **CA** | Certificate Authority — autorité délivrant des certificats |
| **Let's Encrypt** | CA gratuite et automatisée (400M de sites) |
| **Certificat auto-signé** | Certificat généré soi-même (usage TP/dev uniquement) |
| **SEO** | Search Engine Optimization — optimisation pour moteurs de recherche |
| **SEO On-Page** | Optimisation du contenu et du code du site |
| **Balise Title** | Titre de la page (50-60 caractères) |
| **Meta Description** | Description de la page (150-160 caractères) |
| **Sitemap** | Fichier XML listant toutes les pages du site |
| **Google Search Console** | Outil Google de suivi du référencement |

---

---

# 🖥️ TP PARTIE 1 — INSTALLER HTTPS (CERTIFICAT AUTO-SIGNÉ)

*Durée : 30 minutes — Individuel*

---

## Objectif

Installer un certificat SSL auto-signé sur le serveur WordPress (S14 BLOC 1) et forcer la redirection HTTP → HTTPS.

---

## Prérequis

- VM Ubuntu Server avec WordPress fonctionnel (S14 BLOC 1)
- Site accessible en HTTP sur http://monsite.local

---

## ÉTAPE 1 — Générer le Certificat (5 min)

```bash
# Générer clé privée + certificat auto-signé valide 365 jours
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/monsite.key \
  -out /etc/ssl/certs/monsite.crt
```

**Répondre aux questions :**
```
Country Name: FR
State: Occitanie
Locality: Toulouse
Organization: Mon Entreprise TP
Common Name: monsite.local  ← IMPORTANT : mettre le nom exact
Email: admin@monsite.local
```

**Vérifier les fichiers créés :**

```bash
ls -l /etc/ssl/private/monsite.key
ls -l /etc/ssl/certs/monsite.crt
```

---

## ÉTAPE 2 — Configurer Apache pour HTTPS (10 min)

```bash
# Créer le VirtualHost HTTPS
sudo nano /etc/apache2/sites-available/monsite-ssl.conf
```

**Contenu du fichier :**

```apache
<VirtualHost *:443>
    ServerName monsite.local
    DocumentRoot /var/www/monsite

    # SSL Configuration
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/monsite.crt
    SSLCertificateKeyFile /etc/ssl/private/monsite.key

    <Directory /var/www/monsite/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/monsite_ssl_error.log
    CustomLog ${APACHE_LOG_DIR}/monsite_ssl_access.log combined
</VirtualHost>
```

**Sauvegarder :** Ctrl+O → Entrée → Ctrl+X

---

## ÉTAPE 3 — Activer SSL et le VirtualHost (5 min)

```bash
# Activer le module SSL d'Apache
sudo a2enmod ssl

# Activer le VirtualHost HTTPS
sudo a2ensite monsite-ssl.conf

# Vérifier la configuration
sudo apache2ctl configtest
# Résultat attendu : Syntax OK

# Recharger Apache
sudo systemctl reload apache2
```

---

## ÉTAPE 4 — Forcer la Redirection HTTP → HTTPS (5 min)

```bash
# Éditer le VirtualHost HTTP (port 80)
sudo nano /etc/apache2/sites-available/monsite.conf
```

**Ajouter avant `</VirtualHost>` :**

```apache
    # Redirection HTTP → HTTPS
    Redirect permanent / https://monsite.local/
```

**Fichier complet :**

```apache
<VirtualHost *:80>
    ServerName monsite.local
    ServerAdmin admin@monsite.local
    DocumentRoot /var/www/monsite

    # Redirection HTTP → HTTPS
    Redirect permanent / https://monsite.local/

    ErrorLog ${APACHE_LOG_DIR}/monsite_error.log
    CustomLog ${APACHE_LOG_DIR}/monsite_access.log combined
</VirtualHost>
```

**Recharger Apache :**

```bash
sudo systemctl reload apache2
```

---

## ÉTAPE 5 — Test et Validation (5 min)

**Test dans le navigateur :**

1. Aller sur http://monsite.local
2. Le site doit **automatiquement rediriger** vers https://monsite.local
3. Le navigateur affiche un **avertissement de sécurité** :
   - Chrome : "Votre connexion n'est pas privée"
   - Firefox : "Avertissement : risque probable de sécurité"

**Accepter le certificat :**
- Chrome : Cliquer "Paramètres avancés" → "Accéder au site"
- Firefox : Cliquer "Avancé" → "Accepter le risque et poursuivre"

4. Le site s'affiche en HTTPS avec 🔒 (cadenas) + avertissement

**Vérifier le certificat :**
- Cliquer sur le cadenas 🔒
- "Le certificat n'est pas valide"
- Afficher le certificat → vérifier les informations (Common Name = monsite.local)

---

## ÉTAPE 6 — Configurer WordPress pour HTTPS (5 min)

**Dans le tableau de bord WordPress :**

1. Réglages → Général
2. **Adresse web de WordPress** : `https://monsite.local`
3. **Adresse web du site** : `https://monsite.local`
4. **Enregistrer les modifications**

**Vérifier :**
- Toutes les pages du site sont maintenant en HTTPS
- Les liens internes utilisent HTTPS

---

---

# 🖥️ TP PARTIE 2 — CONFIGURER LE SEO (YOAST SEO)

*Durée : 15 minutes — Individuel*

---

## Objectif

Installer et configurer le plugin Yoast SEO pour optimiser le référencement du site WordPress.

---

## ÉTAPE 1 — Installer Yoast SEO (3 min)

**Dans WordPress :**

1. Extensions → Ajouter
2. Rechercher : **Yoast SEO**
3. Cliquer **Installer** (sur Yoast SEO by Team Yoast)
4. Cliquer **Activer**

---

## ÉTAPE 2 — Configuration Initiale (5 min)

1. SEO → Général
2. **Assistant de configuration** → Démarrer
3. Suivre les étapes :

**Étape 1 — Type de site :**
- Sélectionner : **Blog** (ou Entreprise locale si approprié)

**Étape 2 — Organisation ou personne :**
- Sélectionner : **Organisation**
- Nom : Mon Entreprise TP
- Logo : (laisser vide ou uploader un logo)

**Étape 3 — Profils sociaux :**
- (Laisser vide pour le TP)

**Étape 4 — Indexation :**
- Tout cocher (par défaut)

**Étape 5 — Plusieurs auteurs :**
- Non (site géré par 1 personne)

**Étape 6 — Terminé**

---

## ÉTAPE 3 — Optimiser une Page (7 min)

**Éditer la page "Bienvenue" (créée en S14) :**

1. Pages → Toutes les pages
2. Éditer "Bienvenue"
3. Descendre jusqu'à la section **Yoast SEO**

**Phrase clé cible :**
- Saisir : `WordPress LAMP tutorial`

**Optimiser le titre SEO :**
- Titre actuel : "Bienvenue"
- Nouveau titre : `Installation WordPress sur LAMP - Tutoriel Complet`
- Yoast affiche un aperçu Google

**Optimiser la méta-description :**
- Cliquer "Modifier l'extrait"
- Saisir : `Guide complet pour installer WordPress sur une stack LAMP (Linux, Apache, MySQL, PHP). Installation pas à pas avec captures d'écran.`

**Score SEO Yoast :**
- Yoast analyse la page
- Affiche des suggestions (points verts ✅ / orange 🟠 / rouges ❌)
- Exemples de suggestions :
  - "La phrase clé apparaît dans le titre" ✅
  - "La méta-description fait 155 caractères" ✅
  - "La phrase clé apparaît dans l'URL" 🟠

**Améliorer l'URL (slug) :**
- Dans le bloc "Permalien" (en haut de la page)
- Modifier : `wordpress-lamp-tutorial`

**Mettre à jour la page**

---

## ÉTAPE 4 — Vérifier le Sitemap (5 min)

Yoast SEO génère automatiquement un sitemap XML.

**Accéder au sitemap :**
- URL : https://monsite.local/sitemap_index.xml
- Le navigateur affiche le sitemap XML

**Contenu du sitemap :**
- Liste des pages du site
- Liste des articles
- Dates de dernière modification

> 📌 **En production :** Soumettre ce sitemap à Google Search Console.

---

---

# 📝 ATELIER — PORTFOLIO E4 #2

*Durée : 45 minutes — Guidé*

---

## Objectif

Formaliser une **deuxième situation professionnelle** vécue en entreprise pour le portfolio E4.

---

## Rappel : Qu'est-ce que l'Épreuve E4 ?

**E4 — Support et mise à disposition de services informatiques**
- Épreuve orale : **40 minutes** (20 min présentation + 20 min questions)
- S'appuie sur un **portfolio** de situations professionnelles
- L'étudiant présente **2 situations** de son choix

**Critères d'évaluation :**
- Qualité de la réalisation technique
- Capacité à analyser et résoudre un problème
- Communication et argumentation

---

## PHASE 1 — Sélection de la Situation (10 min)

**Utiliser la grille de sélection (Annexe 2) :**

| **Critère** | **Oui** | **Non** |
|---|---|---|
| Situation vécue **personnellement** en entreprise ? | | |
| Situation **technique** (pas administrative) ? | | |
| Situation **significative** (> 2 heures de travail) ? | | |
| Compétences BLOC 1 mises en œuvre ? | | |
| Des **preuves** disponibles (captures, docs) ? | | |
| **Différente** de la situation #1 ? | | |

**Si 6/6 "Oui" → Situation valable pour E4**

**Exemples de bonnes situations pour BLOC 1 :**
- Déploiement d'un nouveau service (site web, serveur fichiers...)
- Résolution d'un incident complexe (panne serveur, problème réseau...)
- Mise en place d'un outil de gestion (GLPI, inventaire...)
- Formation utilisateurs à un nouveau logiciel
- Migration infrastructure (passage Windows 10 → 11, changement de serveur...)

**Exemples de mauvaises situations :**
- "J'ai réinitialisé 50 mots de passe" (trop répétitif, pas technique)
- "J'ai classé des documents" (pas technique)
- "J'ai observé mon tuteur faire..." (pas personnel)

---

## PHASE 2 — Rédaction de la Fiche (25 min)

**Utiliser le modèle fourni (Annexe 1).**

### Section 1 — Identification (3 min)

```
SITUATION PROFESSIONNELLE N°2
─────────────────────────────────────────────────────────────

Titre de la situation :
(5-10 mots décrivant l'action principale)
Ex : "Installation d'un serveur WordPress pour le site vitrine"

Date de réalisation : 
Durée totale : 
Entreprise : 
Tuteur/Référent :
```

### Section 2 — Contexte (5 min)

**Décrire le contexte en 5-10 lignes :**
- Quelle était la situation initiale ?
- Quel besoin ou problème existait ?
- Qui a demandé cette action ?

**Exemple :**
> *"L'entreprise ABC (PME 30 salariés, commerce de proximité) souhaitait moderniser sa présence en ligne. L'ancien site, créé en 2010, n'était plus maintenu et n'était pas responsive mobile. Le directeur m'a demandé d'installer un nouveau site WordPress pour présenter les produits et permettre la prise de rendez-vous en ligne."*

### Section 3 — Mission et Objectifs (5 min)

**Décrire la mission confiée :**
- Qu'ai-je dû faire concrètement ?
- Quels étaient les objectifs à atteindre ?

**Exemple :**
> *"Ma mission était d'installer et configurer un serveur LAMP (Linux, Apache, MySQL, PHP) sur un VPS loué chez OVH, puis d'y déployer WordPress avec un thème adapté au commerce de proximité. Objectifs : site accessible en HTTPS, responsive mobile, formulaire de contact fonctionnel, sauvegarde automatique."*

### Section 4 — Réalisation (7 min)

**Décrire les étapes de la réalisation :**

**Format recommandé :**
```
1. Préparation
   - Choix du VPS (OVH, 4 Go RAM, Ubuntu Server 22.04)
   - Installation du système d'exploitation

2. Installation de la stack LAMP
   - Installation Apache 2.4
   - Installation MySQL 8.0 et création base de données
   - Installation PHP 8.1 + modules

3. Déploiement WordPress
   - Téléchargement WordPress 6.4 (version française)
   - Configuration VirtualHost Apache
   - Installation via interface web

4. Sécurisation
   - Installation certificat SSL Let's Encrypt
   - Configuration force HTTPS
   - Installation plugin de sécurité Wordfence

5. Tests et mise en production
   - Tests de navigation multi-navigateurs
   - Tests responsive mobile
   - Validation formulaire de contact
```

### Section 5 — Difficultés Rencontrées (3 min)

**Décrire 1-2 difficultés et comment elles ont été résolues :**

**Exemple :**
> *"Difficulté 1 : Le certificat Let's Encrypt ne s'installait pas. Cause : port 80 fermé par le firewall OVH. Résolution : ouverture du port 80 via l'interface OVH + nouvelle tentative → succès."*
>
> *"Difficulté 2 : Les permaliens WordPress généraient des erreurs 404. Cause : module Apache rewrite non activé. Résolution : `sudo a2enmod rewrite` + redémarrage Apache."*

### Section 6 — Résultats (2 min)

**Décrire les résultats obtenus :**
- Le site est-il en ligne et fonctionnel ?
- Les objectifs ont-ils été atteints ?
- Retours utilisateurs/client ?

**Exemple :**
> *"Le site a été mis en production le 15/12/2024. Il est accessible en HTTPS, entièrement responsive, et le formulaire de contact fonctionne. Le directeur est satisfait : 25 demandes de contact reçues via le site durant le premier mois."*

---

## PHASE 3 — Sélection des Preuves (10 min)

**Joindre 3-5 preuves au portfolio :**

| **Type de preuve** | **Exemple** |
|---|---|
| **Captures d'écran** | Interface WordPress, site en HTTPS, tableau de bord |
| **Fichiers de config** | VirtualHost Apache, wp-config.php (masquer les MDP !) |
| **Schémas** | Architecture LAMP, schéma réseau |
| **Logs / Rapports** | Logs d'installation, rapport de tests |
| **Attestations** | Email du tuteur validant la mise en production |

**Nommer les fichiers :**
- `E4_Situation2_Preuve1_Site_HTTPS.png`
- `E4_Situation2_Preuve2_Config_Apache.txt`
- `E4_Situation2_Preuve3_Schema_LAMP.pdf`

---

---

# 📝 ÉVALUATION FORMATIVE 2 — TP INTÉGRÉ

*Durée : 90 minutes — Individuel — Noté*

---

## Contexte

L'entreprise **TechCorp** (50 salariés) souhaite déployer une nouvelle infrastructure Active Directory pour centraliser la gestion des utilisateurs et des ressources.

Vous êtes chargé de mettre en place cette infrastructure.

---

## Infrastructure Fournie

- **Serveur Windows Server 2022** : SRV-DC01 (contrôleur de domaine)
- **2 postes clients Windows 11** : PC-CLIENT01, PC-CLIENT02
- **Réseau** : 192.168.10.0/24

---

## MISSION 1 — Active Directory (30 min / 40 points)

### 1.1. Installer Active Directory (10 points)

1. Installer le rôle **Active Directory Domain Services**
2. Promouvoir le serveur en contrôleur de domaine
3. Créer une nouvelle forêt : `techcorp.local`

**Livrable :** Capture d'écran de la console "Utilisateurs et ordinateurs Active Directory" montrant le domaine créé.

### 1.2. Créer la Structure Organisationnelle (15 points)

Créer l'arborescence d'Unités Organisationnelles (OU) suivante :

```
TECHCORP
├── Utilisateurs
│   ├── Direction
│   ├── Comptabilite
│   └── Informatique
└── Ordinateurs
    ├── Postes-Fixes
    └── Portables
```

**Livrable :** Capture d'écran de la console AD montrant toutes les OU créées.

### 1.3. Créer les Utilisateurs (15 points)

Créer les comptes utilisateurs suivants :

| **Nom** | **Prénom** | **Login** | **OU** | **Mot de passe** |
|---|---|---|---|---|
| MARTIN | Sophie | smartin | Direction | Pass@2024! |
| DURAND | Pierre | pdurand | Comptabilite | Pass@2024! |
| BERNARD | Julie | jbernard | Informatique | Pass@2024! |

**Livrable :** Capture d'écran montrant les 3 utilisateurs créés dans leurs OU respectives.

---

## MISSION 2 — DNS (20 min / 20 points)

### 2.1. Configurer le DNS (10 points)

Le DNS est installé automatiquement avec AD. Vérifier sa configuration :

1. Ouvrir la console **Gestionnaire DNS**
2. Vérifier que la zone `techcorp.local` existe
3. Vérifier l'enregistrement A du serveur SRV-DC01

**Livrable :** Capture d'écran de la console DNS montrant la zone `techcorp.local`.

### 2.2. Ajouter des Enregistrements (10 points)

Créer les enregistrements DNS suivants :

| **Type** | **Nom** | **Cible/Adresse** |
|---|---|---|
| A | www | 192.168.10.10 |
| A | ftp | 192.168.10.11 |
| CNAME | intranet | www.techcorp.local |

**Livrable :** Capture d'écran montrant les 3 enregistrements créés.

---

## MISSION 3 — DHCP (15 min / 20 points)

### 3.1. Installer et Configurer DHCP (15 points)

1. Installer le rôle **DHCP**
2. Créer une étendue avec les paramètres suivants :

```
Nom de l'étendue : Réseau-Principal
Plage d'adresses : 192.168.10.100 - 192.168.10.200
Masque : 255.255.255.0
Passerelle par défaut : 192.168.10.1
Serveurs DNS : 192.168.10.10 (SRV-DC01)
Durée du bail : 8 heures
```

3. Exclure les adresses 192.168.10.100 à 192.168.10.110 (réservées serveurs)

**Livrable :** Capture d'écran de la console DHCP montrant l'étendue configurée.

### 3.2. Tester DHCP (5 points)

1. Sur PC-CLIENT01, configurer la carte réseau en **DHCP**
2. Vérifier qu'une adresse IP est obtenue
3. Exécuter `ipconfig /all` et vérifier les paramètres

**Livrable :** Capture d'écran du résultat de `ipconfig /all` sur PC-CLIENT01.

---

## MISSION 4 — Droits NTFS et Partages (25 min / 20 points)

### 4.1. Créer une Arborescence de Dossiers (5 points)

Sur SRV-DC01, créer la structure suivante dans `C:\Partages\` :

```
C:\Partages\
├── Direction
├── Comptabilite
└── Commun
```

**Livrable :** Capture d'écran de l'explorateur montrant les dossiers créés.

### 4.2. Configurer les Droits NTFS (10 points)

Appliquer les droits NTFS suivants :

| **Dossier** | **Groupe/Utilisateur** | **Droits NTFS** |
|---|---|---|
| Direction | smartin | Contrôle total |
| Direction | Tous les autres | Aucun accès |
| Comptabilite | pdurand | Modification |
| Comptabilite | Tous les autres | Lecture seule |
| Commun | Utilisateurs du domaine | Modification |

**Livrable :** Captures d'écran des propriétés de sécurité (onglet Sécurité) des 3 dossiers.

### 4.3. Créer les Partages Réseau (5 points)

Partager les dossiers avec les noms suivants :

| **Dossier** | **Nom de partage** | **Droits de partage** |
|---|---|---|
| C:\Partages\Direction | Direction$ | Contrôle total pour smartin |
| C:\Partages\Comptabilite | Comptabilite | Lecture pour Tout le monde |
| C:\Partages\Commun | Commun | Modification pour Tout le monde |

**Livrable :** Capture d'écran de la console "Gestion de l'ordinateur → Dossiers partagés" montrant les 3 partages.

---

## Grille de Notation

| **Mission** | **Points** |
|---|---|
| Mission 1 — Active Directory | /40 |
| Mission 2 — DNS | /20 |
| Mission 3 — DHCP | /20 |
| Mission 4 — Droits et Partages | /20 |
| **TOTAL** | **/100** |

**Critères transversaux :**
- Autonomie : /10 (bonus)
- Documentation des actions : /10 (bonus si procédure rédigée)

---

---

# 📄 ANNEXE 1 — MODÈLE FICHE PORTFOLIO E4

```
═══════════════════════════════════════════════════════════════
                   PORTFOLIO E4 — SITUATION N°2
═══════════════════════════════════════════════════════════════

┌─────────────────────────────────────────────────────────────┐
│ 1. IDENTIFICATION                                           │
└─────────────────────────────────────────────────────────────┘

Titre de la situation :
___________________________________________________________________

Date de réalisation : _______________  Durée totale : ___________

Entreprise : __________________________________________________________

Tuteur/Référent : _____________________________________________________


┌─────────────────────────────────────────────────────────────┐
│ 2. CONTEXTE                                                 │
└─────────────────────────────────────────────────────────────┘

(Décrire la situation initiale, le besoin ou problème — 5-10 lignes)

___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________


┌─────────────────────────────────────────────────────────────┐
│ 3. MISSION ET OBJECTIFS                                     │
└─────────────────────────────────────────────────────────────┘

(Décrire la mission confiée et les objectifs à atteindre)

___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________


┌─────────────────────────────────────────────────────────────┐
│ 4. RÉALISATION                                              │
└─────────────────────────────────────────────────────────────┘

(Décrire les étapes de la réalisation — format liste numérotée)

1. _______________________________________________________________
2. _______________________________________________________________
3. _______________________________________________________________
4. _______________________________________________________________
5. _______________________________________________________________


┌─────────────────────────────────────────────────────────────┐
│ 5. DIFFICULTÉS RENCONTRÉES                                  │
└─────────────────────────────────────────────────────────────┘

Difficulté 1 : ________________________________________________________
Résolution : __________________________________________________________

Difficulté 2 : ________________________________________________________
Résolution : __________________________________________________________


┌─────────────────────────────────────────────────────────────┐
│ 6. RÉSULTATS                                                │
└─────────────────────────────────────────────────────────────┘

(Décrire les résultats obtenus et leur impact)

___________________________________________________________________
___________________________________________________________________
___________________________________________________________________


┌─────────────────────────────────────────────────────────────┐
│ 7. COMPÉTENCES MOBILISÉES                                   │
└─────────────────────────────────────────────────────────────┘

☐ B1.1 — Recenser et identifier les ressources
☐ B1.2 — Exploiter des référentiels et normes
☐ B1.3 — Identifier les processus présents
☐ B1.4 — Mettre en place des outils de gestion de parc
☐ B1.5 — Mettre à disposition un service
☐ B1.6 — Accompagner les utilisateurs


┌─────────────────────────────────────────────────────────────┐
│ 8. PREUVES JOINTES                                          │
└─────────────────────────────────────────────────────────────┘

1. _______________________________________________________________
2. _______________________________________________________________
3. _______________________________________________________________
4. _______________________________________________________________
5. _______________________________________________________________

═══════════════════════════════════════════════════════════════
```

---

# 📄 ANNEXE 2 — GRILLE DE SÉLECTION DES SITUATIONS

| **Critère** | **Oui** | **Non** |
|---|---|---|
| **1. Situation personnelle** : Ai-je réalisé cette mission moi-même ? | ☐ | ☐ |
| **2. Situation technique** : S'agit-il d'une tâche technique (pas administrative) ? | ☐ | ☐ |
| **3. Situation significative** : Ai-je passé plus de 2 heures dessus ? | ☐ | ☐ |
| **4. Compétences BLOC 1** : Ai-je mis en œuvre des compétences du BLOC 1 ? | ☐ | ☐ |
| **5. Preuves disponibles** : Ai-je des captures, documents, schémas ? | ☐ | ☐ |
| **6. Différente de la situation #1** : Est-elle suffisamment différente ? | ☐ | ☐ |

**Si 6/6 "Oui" → Situation valable pour E4**

**Si 4-5 "Oui" → Situation acceptable mais à améliorer**

**Si < 4 "Oui" → Chercher une autre situation**

---

# 📄 ANNEXE 3 — SUJET ÉVALUATION FORMATIVE 2

*(Sujet complet fourni dans la section "Évaluation Formative 2" ci-dessus)*

---

*Pack S15 BLOC 1 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences : B1.5, B2.1, B3.3, B2.2 (éval)*
*HTTPS · SSL/TLS · SEO · Portfolio E4 · Évaluation Formative*
