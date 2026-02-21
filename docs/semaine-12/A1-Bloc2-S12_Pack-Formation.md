# Pack de Formation - Semaine 12 (S12) - BLOC 2
## 💿 Déploiement d'Images Système · Clonage · WDS/MDT · TP Capture & Déploiement

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S12 — Année 1 |
| **Bloc** | Bloc 2 — Administration des systèmes et des réseaux |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — douzième semaine |
| **Modalité** | Présentiel — salle TP (postes physiques ou VMs) |
| **Prérequis** | S2 (inventaire matériel), S11 (gestion actifs logiciels), notions Windows/Linux |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B2.1** | Installer et configurer un service réseau pour une TPE ou une PME | Acquisition |
| **B2.2** | Installer et configurer des éléments d'infrastructure | Maîtrise |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc | Maîtrise |
| **B3.3** | Participer à la gestion et au suivi d'un projet | Acquisition |

> 📌 **S12 est une séance technique charnière** qui marque l'entrée réelle dans l'administration système du Bloc 2. Jusqu'ici, les apprenants ont inventorié, géré, documenté. À partir de S12, ils **construisent** : déployer 50 postes identiques en 2 heures plutôt qu'en 2 semaines est une compétence qui change radicalement la perception du métier SISR.

---

## Objectifs Pédagogiques

**Concepts de déploiement :**
- ✅ Distinguer **installation manuelle** vs **déploiement automatisé** d'un OS
- ✅ Expliquer le principe du **clonage de disque** (bit-à-bit)
- ✅ Décrire une **image système** et ses composantes (OS + pilotes + logiciels + config)
- ✅ Identifier les **cas d'usage** du déploiement d'images (parc homogène, disaster recovery, standardisation)
- ✅ Expliquer les notions de **Sysprep** et **généralisation** Windows
- ✅ Comparer les outils de clonage (Clonezilla, WDS/MDT, Fog Project)

**Pratique technique :**
- ✅ Créer une **machine de référence** (golden image) avec OS + logiciels
- ✅ Capturer une image système avec **Clonezilla**
- ✅ Déployer l'image capturée sur un nouveau poste
- ✅ Vérifier la **post-configuration** (SID, nom machine, activation)
- ✅ Documenter le processus dans une procédure technique (lien S11)

---

## ⭐ Spécificités Pédagogiques

### Le Déclic Pédagogique de S12

S12 provoque souvent un **déclic** chez les apprenants : c'est le moment où ils réalisent qu'un technicien SISR ne passe pas sa vie à installer Windows manuellement poste par poste. L'image système automatise ce qui prenait 3 heures par poste en 20 minutes — et peut être reproduite à l'infini.

**Verbaliser ce changement d'échelle** en début de séance :

> *"Un technicien junior qui ne connaît que l'installation manuelle peut gérer 10 postes par semaine. Un technicien qui maîtrise le déploiement d'images peut gérer 100 postes par semaine — avec moins d'erreurs et une qualité constante. C'est la différence entre un exécutant et un professionnel IT."*

### Choix Pédagogique : Clonezilla d'Abord, WDS/MDT Ensuite

Le pack S12 introduit **Clonezilla** comme outil principal du TP, puis présente **WDS/MDT** en cours théorique. Ce choix pédagogique est motivé par :

**Argument 1 — Simplicité.** Clonezilla est utilisable sans infrastructure serveur complexe — il fonctionne en standalone ou avec un serveur SSH basique. WDS nécessite Active Directory + serveur DHCP + PXE, ce qui est hors de portée en S12.

**Argument 2 — Universalité.** Clonezilla clone n'importe quel OS (Windows, Linux, macOS), alors que WDS est spécifique Windows. Les apprenants peuvent réutiliser Clonezilla en entreprise immédiatement.

**Argument 3 — Progression.** WDS/MDT sera approfondi en Projet SimIO (S17-S18) quand l'infrastructure réseau sera en place. S12 pose les bases conceptuelles.

### Matériel Nécessaire et Alternatives

**Configuration idéale :**
- 2 VMs par apprenant (1 machine de référence + 1 machine cible)
- Boot réseau PXE désactivé (sinon Clonezilla ISO en boot local)
- Stockage réseau partagé (NFS/SMB) pour centraliser les images

**Alternatives si contraintes :**
- **Disques externes USB** : chaque apprenant stocke son image sur une clé USB 16 Go
- **Poste physique + VM** : capturer l'image d'un poste physique, déployer sur VM (ou inverse)
- **Démonstration guidée** : si nombre de postes insuffisant, un apprenant pilote en mode vidéoprojecteur, les autres suivent la procédure sans capturer

### Lien avec l'Entreprise (Alternance)

Beaucoup d'apprenants seront confrontés au déploiement de postes en entreprise dans les semaines qui suivent S12. Insister sur le fait que **documenter le processus** (machine de référence, logiciels installés, version d'image) est aussi important que de savoir le faire techniquement.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S11 | Feedback procédures rédigées — points forts collectifs |
| H+0:10 | 15 min | 🎯 Découverte | Activité "50 Postes en 1 Jour" |
| H+0:25 | 45 min | 📖 Cours | Déploiement d'images : concepts, outils, Sysprep, cas d'usage |
| H+1:10 | **15 min** | ☕ **PAUSE** | — |
| H+1:25 | 20 min | 🔧 Démo | Démonstration Clonezilla (capture + déploiement) par l'enseignant |
| H+1:45 | 90 min | 🖥️ **TP** | Capture et déploiement d'une image système (par apprenant) |
| H+3:15 | 25 min | ✅ Validation | Tests post-déploiement + vérification SID + activation |
| H+3:40 | 20 min | 📝 Documentation | Rédaction procédure de déploiement (modèle S11 réutilisé) |

---

## Différenciation Pédagogique

### Profil Avancé
- **Image personnalisée** : Créer une image Windows 11 avec Office, 7-Zip, Firefox pré-installés + fond d'écran personnalisé + scripts de post-config
- **Déploiement réseau** : Configurer Clonezilla en mode serveur (drbl-winroll) pour déploiement multicast sur 3 machines simultanément
- **Sysprep manuel** : Exécuter Sysprep avec fichier de réponses (unattend.xml) pour automatiser la post-configuration
- **Extension WDS** : Si infrastructure AD disponible, tester WDS en parallèle de Clonezilla

### Profil Débutant
- **Image fournie** : Utiliser une image pré-capturée par l'enseignant, se concentrer sur le déploiement uniquement
- **Mode guidé** : Suivre la procédure pas-à-pas avec captures d'écran (fournie)
- **Binômage** : Un apprenant avancé guide sans faire à la place
- **Validation simplifiée** : Vérifier uniquement le boot et le nom de machine (pas le SID)

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **2 VMs par apprenant** | 1 machine de référence (Windows 10/11) + 1 machine cible vierge |
| **ISO Clonezilla** | Dernière version stable (téléchargée ou sur serveur local) |
| **Stockage images** | Serveur NFS/SMB ou clés USB 16 Go |
| **Checklist Sysprep** | Annexe 1 — vérifications avant capture |
| **Procédure déploiement** | Modèle vierge (Annexe 2) |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "50 Postes en 1 Jour"

*Durée : 15 minutes — Collectif*

---

## Mise en Situation (5 min)

L'enseignant pose le problème :

---

> **Contexte :** Une PME renouvelle son parc informatique : 50 nouveaux PC Dell arrivent lundi matin. Ils doivent être opérationnels vendredi pour le déménagement de bureaux. Chaque PC doit avoir :
> - Windows 11 Pro
> - Microsoft Office 2021
> - Adobe Acrobat Reader
> - 7-Zip, VLC, Firefox
> - Imprimantes réseau configurées
> - Fond d'écran de l'entreprise
> - Compte utilisateur local "Admin-Local"
>
> **Deux techniciens, deux approches :**

---

**Technicien A — Installation manuelle :**
```
Par poste :
- Installation Windows 11 : 45 min
- Mises à jour Windows : 30 min
- Installation Office : 20 min
- Installation des 4 autres logiciels : 15 min
- Configuration imprimantes : 10 min
- Personnalisation (fond d'écran, compte) : 10 min
───────────────────────────────────────────────────
Total par poste : 2h10

50 postes × 2h10 = 108 heures = 13,5 jours (à 8h/jour)
→ Impossible de tenir le délai avec 2 techniciens
```

**Technicien B — Déploiement d'image :**
```
Préparation (une fois) :
- Installation Windows 11 sur 1 PC de référence : 45 min
- Installation de tous les logiciels : 45 min
- Configuration et personnalisation : 30 min
- Capture de l'image avec Clonezilla : 20 min
───────────────────────────────────────────────────
Total préparation : 2h20 (une seule fois)

Déploiement (par poste) :
- Déployer l'image Clonezilla : 15 min
- Post-configuration (nom, activation) : 5 min
───────────────────────────────────────────────────
Total par poste : 20 min

50 postes × 20 min = 16,7 heures = 2 jours (à 8h/jour)
→ 2 techniciens peuvent déployer 50 postes en 1 jour
   en parallèle (25 postes chacun)
```

---

## Questions Guidées (7 min)

| **Question** | **Concept visé** |
|---|---|
| "Pourquoi l'approche B est-elle plus rapide ?" | Automatisation — le travail complexe n'est fait qu'une fois |
| "Quelle approche garantit que tous les postes sont identiques ?" | Image système — pas de risque d'oubli ou d'erreur |
| "Est-ce que l'approche B fonctionne si chaque poste doit avoir un nom différent ?" | Oui — la post-configuration personnalise après le déploiement |
| "Y a-t-il des inconvénients à l'approche B ?" | Investissement initial (temps de préparation), stockage de l'image |

## Conclusion (3 min)

> *"Le déploiement d'images est la compétence qui fait passer d'une gestion artisanale à une gestion industrielle du parc informatique. Vous apprendrez cette semaine à créer, capturer et déployer des images — une compétence que vous utiliserez dans 90% des postes SISR."*

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Déploiement d'Images Système · Clonage · Automatisation"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 12*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.1** | Installer et configurer un service réseau |
| **B2.2** | Installer et configurer des éléments d'infrastructure |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc |

---

## PARTIE I — Concepts Fondamentaux

### I.A. Qu'est-ce qu'une Image Système ?

Une **image système** (ou **image disque**) est une copie exacte du contenu d'un disque dur ou d'une partition, incluant :

```
   IMAGE SYSTÈME = SNAPSHOT COMPLET D'UN DISQUE
   ─────────────────────────────────────────────────────────────
   ├── Système d'exploitation (Windows, Linux...)
   ├── Pilotes matériels (carte réseau, graphique, son...)
   ├── Logiciels pré-installés (Office, navigateurs, outils...)
   ├── Configuration système (services, registre Windows...)
   ├── Personnalisation (fond d'écran, icônes, raccourcis...)
   ├── Comptes utilisateurs locaux
   └── Données (si incluses — généralement non recommandé)

   Format de l'image :
   ├── Fichier unique compressé (.img, .wim, .gz...)
   ├── Taille : 5-15 Go selon contenu (compressé)
   └── Peut être stockée sur serveur, NAS, disque externe
```

> 💡 **Analogie cuisine :** Une image système, c'est comme une recette de gâteau déjà cuite et congelée. Au lieu de refaire toute la recette à chaque fois (installer Windows, puis Office, puis...), on décongèle le gâteau déjà prêt et on l'adapte (nom de la machine, utilisateur).

---

### I.B. Clonage vs Image vs Installation

| **Méthode** | **Principe** | **Avantages** | **Inconvénients** | **Usage** |
|---|---|---|---|---|
| **Installation manuelle** | Installer l'OS puis chaque logiciel un par un | Contrôle total, personnalisation | Très long, risque d'erreur | 1-5 postes uniques |
| **Clonage disque à disque** | Copier bit-à-bit disque A → disque B | Très rapide | Nécessite 2 disques de même taille minimum | Migration 1:1 |
| **Image système** | Capturer → stocker → déployer sur N postes | Rapide, reproductible, centralisé | Préparation initiale | Parc homogène 10+ postes |
| **Déploiement réseau PXE** | Déployer image via réseau (WDS, Fog) | Pas de clé USB, multicast | Infrastructure serveur nécessaire | Parc > 50 postes |

---

### I.C. Les Cas d'Usage du Déploiement d'Images

| **Cas d'usage** | **Description** | **Gain de temps** |
|---|---|---|
| **Déploiement de parc neuf** | 50-200 nouveaux postes identiques à configurer | 80-90% vs installation manuelle |
| **Renouvellement de parc** | Remplacer tous les postes tous les 4-5 ans | 85% vs installation manuelle |
| **Disaster Recovery** | Restaurer un poste défaillant en 20 min | 95% vs réinstallation complète |
| **Standardisation** | Garantir que tous les postes sont identiques | Qualité constante + conformité |
| **Formation / Salles TP** | Remettre à neuf 30 postes entre deux sessions | 98% vs réinstallation manuelle |
| **Migration OS** | Passer 100 postes de Windows 10 à Windows 11 | 70% vs migration manuelle |

---

## PARTIE II — La Machine de Référence (Golden Image)

### II.A. Définition

La **machine de référence** (ou **golden image** / **master image**) est le poste modèle qui sera cloné pour tous les autres postes du parc. Sa qualité détermine la qualité de tous les déploiements.

```
   CONSTRUCTION D'UNE MACHINE DE RÉFÉRENCE — CHECKLIST
   ─────────────────────────────────────────────────────────────

   ☐ ÉTAPE 1 — Installation OS propre
      • OS original (ISO officielle Microsoft ou distribution Linux)
      • Partition unique (C:\ pour Windows, / pour Linux)
      • Pas de données personnelles

   ☐ ÉTAPE 2 — Mises à jour complètes
      • Windows Update jusqu'à 0 mise à jour en attente
      • Redémarrage pour finaliser

   ☐ ÉTAPE 3 — Pilotes matériels
      • Pilotes génériques Microsoft (suffisent pour matériel récent)
      • OU pilotes spécifiques si parc homogène (Dell, HP...)

   ☐ ÉTAPE 4 — Logiciels standards
      • Office, Adobe Reader, 7-Zip, navigateurs...
      • Installer en version silencieuse si possible (déploiement sans GUI)

   ☐ ÉTAPE 5 — Configuration système
      • Services Windows optimisés (désactiver services inutiles)
      • Stratégies de sécurité (pare-feu, UAC, updates auto)
      • Fond d'écran corporate
      • Raccourcis bureautiques standards

   ☐ ÉTAPE 6 — Nettoyage pré-capture
      • Supprimer fichiers temporaires (Disk Cleanup)
      • Vider la corbeille
      • Supprimer les logs de setup
      • Défragmenter le disque (HDD uniquement, pas SSD)

   ☐ ÉTAPE 7 — Sysprep (Windows uniquement — voir II.B)
      • Généralisation pour retirer l'identité unique
```

---

### II.B. Sysprep — La Généralisation Windows

**Sysprep** (System Preparation Tool) est un utilitaire Microsoft qui **généralise** une installation Windows pour qu'elle puisse être clonée sans conflit.

```
   PROBLÈME SANS SYSPREP
   ─────────────────────────────────────────────────────────────
   Chaque installation Windows génère un SID (Security Identifier)
   unique pour la machine. Si on clone sans Sysprep :

   PC-REF    → SID : S-1-5-21-123456789-...
   Clone 1   → SID : S-1-5-21-123456789-...  ← IDENTIQUE !
   Clone 2   → SID : S-1-5-21-123456789-...  ← IDENTIQUE !

   Conséquence :
   • Conflits dans Active Directory (même SID = même machine)
   • Problèmes de licences Windows (activation refusée)
   • Erreurs réseau (NetBIOS, WINS...)


   SOLUTION : SYSPREP
   ─────────────────────────────────────────────────────────────
   Sysprep retire l'identité unique de Windows et prépare l'image
   pour être redéployée. À chaque déploiement, Windows génère :
   • Un nouveau SID
   • Un nouveau nom de machine
   • Une nouvelle demande d'activation (si pas de clé générique)
```

**Commande Sysprep (Windows) :**

```cmd
C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown

Options :
/generalize → Retire le SID et les infos spécifiques
/oobe       → Au prochain boot, affiche l'assistant de configuration initiale
/shutdown   → Éteint la machine après Sysprep (prêt à capturer l'image)
```

> ⚠️ **IMPORTANT :** Sysprep **doit** être exécuté AVANT de capturer l'image Windows. Si oublié, tous les clones auront le même SID → problèmes garantis.

---

### II.C. Linux et Généralisation

Sous Linux, il n'y a pas de SID, mais il faut quand même **nettoyer** avant de capturer l'image :

```bash
# Supprimer l'historique des commandes
history -c
rm ~/.bash_history

# Supprimer les clés SSH uniques (si présentes)
rm -rf /etc/ssh/ssh_host_*

# Vider les logs système
> /var/log/syslog
> /var/log/auth.log

# Supprimer les règles réseau persistantes (udev)
rm /etc/udev/rules.d/70-persistent-net.rules

# Réinitialiser le hostname (sera reconfiguré au déploiement)
echo "localhost" > /etc/hostname

# Nettoyer le cache APT
apt-get clean
```

---

## PARTIE III — Outils de Clonage et Déploiement

### III.A. Comparatif des Outils

| **Outil** | **OS supportés** | **Type** | **Complexité** | **Usage typique** |
|---|---|---|---|---|
| **Clonezilla** | Windows, Linux, macOS | Live CD/USB, standalone ou serveur | ★☆☆ | PME, salle TP, technicien autonome |
| **WDS/MDT** | Windows uniquement | Serveur Windows, déploiement PXE réseau | ★★★ | Entreprise avec AD, parc > 50 postes |
| **Fog Project** | Windows, Linux | Serveur Linux, déploiement PXE réseau | ★★☆ | Alternative open source à WDS |
| **Ghost (Symantec)** | Windows, Linux | Payant, entreprise | ★★☆ | Grandes DSI (déclin face au gratuit) |
| **dd** | Linux (tout) | Ligne de commande | ★★★ | Experts Linux, serveurs |
| **Acronis True Image** | Windows, macOS | Payant, GUI simple | ★☆☆ | Particuliers, petites structures |

> 📌 **Choix pédagogique S12 :** Clonezilla est retenu car il est gratuit, universel, ne nécessite pas d'infrastructure complexe, et est très utilisé en PME. WDS/MDT sera vu en S17-S18 (Projet SimIO) quand l'AD sera en place.

---

### III.B. Clonezilla — Présentation

**Clonezilla** est un outil open source de clonage et déploiement basé sur **Partclone** et **dd**. Il existe en deux versions :

```
   CLONEZILLA LIVE (S12)
   ─────────────────────────────────────────────────────────────
   • Boot depuis USB ou CD
   • Mode standalone : 1 machine à la fois
   • Image stockée sur disque externe, serveur SSH, NFS, Samba
   • Interface texte (menus guidés)
   • Usage : technicien mobile, disaster recovery, petit parc

   CLONEZILLA SERVER (SE — Server Edition)
   ─────────────────────────────────────────────────────────────
   • Serveur DRBL (Diskless Remote Boot in Linux)
   • Déploiement multicast : 1 image → N machines simultanément
   • Boot PXE réseau
   • Usage : salle TP, déploiement de masse (50+ postes)
```

---

### III.C. WDS/MDT — Aperçu (Approfondi en S17-S18)

**WDS** (Windows Deployment Services) et **MDT** (Microsoft Deployment Toolkit) sont les outils Microsoft pour le déploiement automatisé de Windows en environnement d'entreprise.

```
   ARCHITECTURE WDS/MDT
   ─────────────────────────────────────────────────────────────

   ① SERVEUR WDS (rôle Windows Server)
      • Héberge les images de déploiement (.wim)
      • Boot PXE réseau (pas de clé USB nécessaire)
      • Intégré Active Directory

   ② SERVEUR DHCP
      • Fournit les options DHCP 66 et 67 (boot PXE)
      • Indique au client où trouver le serveur WDS

   ③ CLIENT (poste à déployer)
      • Boot réseau PXE (option dans le BIOS)
      • Télécharge l'image depuis le serveur WDS
      • Déploiement automatique avec fichier de réponses (unattend.xml)

   Avantages :
   ✅ Déploiement sans intervention (zero-touch)
   ✅ Injection automatique de pilotes selon modèle matériel
   ✅ Intégration domaine automatique
   ✅ Post-configuration via scripts PowerShell

   Inconvénients :
   ❌ Infrastructure complexe (AD + DHCP + WDS)
   ❌ Windows uniquement
   ❌ Courbe d'apprentissage élevée
```

---

## PARTIE IV — Workflow de Déploiement avec Clonezilla

### IV.A. Les 4 Phases

```
   PHASE 1 — PRÉPARATION
   ─────────────────────────────────────────────────────────────
   1. Créer la machine de référence (golden image)
   2. Installer OS + logiciels + configuration
   3. Nettoyer et Sysprep (Windows) ou nettoyage manuel (Linux)
   4. Éteindre la machine


   PHASE 2 — CAPTURE DE L'IMAGE
   ─────────────────────────────────────────────────────────────
   1. Booter la machine de référence sur Clonezilla Live (USB/CD)
   2. Choisir "device-image" (sauvegarder vers fichier image)
   3. Sélectionner la destination (serveur SSH, disque externe...)
   4. Choisir "savedisk" (sauvegarder le disque entier)
   5. Nommer l'image (ex : Win11-Office-2024-11-20)
   6. Lancer la capture → durée : 10-30 min selon taille
   7. Image générée : fichier .img compressé


   PHASE 3 — DÉPLOIEMENT DE L'IMAGE
   ─────────────────────────────────────────────────────────────
   1. Booter le poste cible vierge sur Clonezilla Live
   2. Choisir "device-image" (restaurer depuis fichier image)
   3. Sélectionner la source (même emplacement que la capture)
   4. Choisir "restoredisk" (restaurer le disque entier)
   5. Sélectionner l'image à déployer
   6. Confirmer (ATTENTION : écrase tout le disque cible)
   7. Lancer le déploiement → durée : 5-15 min
   8. Redémarrer


   PHASE 4 — POST-CONFIGURATION
   ─────────────────────────────────────────────────────────────
   Windows :
   1. Assistant OOBE s'affiche (si Sysprep exécuté)
   2. Configurer : région, clavier, nom de machine, utilisateur
   3. Activer Windows (clé de produit si nécessaire)
   4. Joindre le domaine AD (si applicable)
   5. Vérifier le SID : ouvrir cmd et taper `whoami /user`
      → Le SID doit être différent de la machine de référence

   Linux :
   1. Changer le hostname : `hostnamectl set-hostname PC-XX`
   2. Regénérer clés SSH : `dpkg-reconfigure openssh-server`
   3. Configurer IP statique ou DHCP selon besoin
   4. Tester la connectivité réseau
```

---

### IV.B. Commandes Clonezilla Utiles

Clonezilla s'utilise via une interface texte en mode menu, mais voici les choix à effectuer :

```
   CAPTURE D'IMAGE (savedisk)
   ─────────────────────────────────────────────────────────────
   Clonezilla live → device-image → local_dev (disque externe)
   ou ssh_server (serveur distant) ou samba_server

   → Beginner mode (recommandé)
   → savedisk (sauvegarder le disque entier)
   → Nom de l'image : Win11-Office-2024-11-20
   → Disque source : sda (disque à capturer)
   → Options de compression : -z1p (compression gzip, parallèle)
   → Confirmer et lancer


   DÉPLOIEMENT D'IMAGE (restoredisk)
   ─────────────────────────────────────────────────────────────
   Clonezilla live → device-image → local_dev ou ssh_server

   → Beginner mode
   → restoredisk (restaurer le disque entier)
   → Sélectionner l'image : Win11-Office-2024-11-20
   → Disque cible : sda (disque à écraser)
   → ⚠️ ATTENTION : toutes les données du disque cible seront perdues
   → Confirmer et lancer
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Image système** | Copie exacte d'un disque incluant OS, logiciels et configuration |
| **Machine de référence** | Poste modèle configuré parfaitement, qui sera cloné pour créer les autres postes |
| **Golden image** | Synonyme de machine de référence — l'image "parfaite" du parc |
| **Clonage** | Copie bit-à-bit d'un disque vers un autre disque |
| **Sysprep** | Outil Microsoft de généralisation Windows (retire le SID unique) |
| **SID** | Security Identifier — identifiant unique d'une machine Windows |
| **Généralisation** | Processus de retrait des informations spécifiques avant clonage |
| **OOBE** | Out-Of-Box Experience — assistant de première configuration Windows |
| **Clonezilla** | Outil open source de clonage et déploiement d'images système |
| **WDS** | Windows Deployment Services — rôle Windows Server pour déploiement réseau PXE |
| **MDT** | Microsoft Deployment Toolkit — surcouche WDS pour automatisation avancée |
| **PXE** | Preboot eXecution Environment — boot réseau sans disque local ni USB |
| **Multicast** | Déploiement simultané d'une image vers plusieurs machines |
| **Unattend.xml** | Fichier de réponses Windows pour automatiser la configuration post-déploiement |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] J'explique la différence entre installation manuelle, clonage et image système
- [ ] Je liste les 7 étapes de création d'une machine de référence
- [ ] J'explique pourquoi Sysprep est obligatoire sous Windows avant capture
- [ ] Je décris le workflow complet de déploiement avec Clonezilla
- [ ] Je sais vérifier que le SID est différent après déploiement
- [ ] Je compare Clonezilla et WDS/MDT (avantages/inconvénients)
- [ ] J'identifie 3 cas d'usage du déploiement d'images en entreprise

---

---

# 🖥️ TP — CAPTURE ET DÉPLOIEMENT D'IMAGE SYSTÈME

*Durée : 90 minutes — Individuel (ou binôme selon matériel)*

---

## Objectif

Créer une machine de référence Windows 10/11, capturer son image avec Clonezilla, puis déployer cette image sur une seconde machine vierge.

---

## Matériel Fourni

- **VM 1** : Machine de référence (Windows 10/11 installé)
- **VM 2** : Machine cible vierge (disque vide)
- **ISO Clonezilla Live** : Disponible sur le serveur ou à télécharger
- **Stockage image** : Clé USB 16 Go OU serveur SSH/NFS/Samba

---

## PHASE 1 — Préparation de la Machine de Référence (20 min)

### 1.1. Installation des Logiciels Standards

Sur la **VM 1** (machine de référence), installer :

| **Logiciel** | **Version** | **URL** |
|---|---|---|
| 7-Zip | Dernière stable | https://www.7-zip.org |
| VLC Media Player | Dernière stable | https://www.videolan.org |
| Mozilla Firefox | Dernière stable | https://www.mozilla.org/firefox |
| Adobe Acrobat Reader DC | Dernière | https://get.adobe.com/reader |

> 💡 **Astuce :** Utiliser Chocolatey pour installer en batch :
> ```powershell
> choco install 7zip vlc firefox adobereader -y
> ```

### 1.2. Personnalisation

- [ ] Changer le fond d'écran (logo de votre établissement ou image neutre)
- [ ] Créer un raccourci "Mes Applications" sur le bureau pointant vers `C:\Program Files`
- [ ] Désactiver les notifications Windows (Paramètres → Système → Notifications)

### 1.3. Nettoyage Pré-Capture

```cmd
# Nettoyage des fichiers temporaires
cleanmgr.exe

# Vider la corbeille
```

Décocher "Fichiers de mise à jour Windows" si vous voulez conserver les updates.

### 1.4. Sysprep (OBLIGATOIRE sous Windows)

**⚠️ SAUVEGARDER VOTRE TRAVAIL AVANT SYSPREP — La machine va s'éteindre**

```cmd
# Ouvrir une invite de commandes en Administrateur
cd C:\Windows\System32\Sysprep
sysprep.exe /generalize /oobe /shutdown
```

**Options choisies dans la GUI Sysprep (si vous préférez l'interface graphique) :**
- Action de nettoyage : **Généraliser le système**
- Options d'arrêt : **Arrêter le système**
- Cocher : ☑ **Généraliser**

La machine va redémarrer, exécuter Sysprep, puis **s'éteindre automatiquement**.

> 🛑 **NE PAS REDÉMARRER LA MACHINE APRÈS SYSPREP** — Elle est prête à être capturée.

---

## PHASE 2 — Capture de l'Image avec Clonezilla (25 min)

### 2.1. Boot sur Clonezilla Live

1. Attacher l'ISO Clonezilla à la **VM 1**
2. Configurer le boot pour démarrer sur le CD/ISO
3. Redémarrer la VM → Clonezilla Live démarre

### 2.2. Choix des Options Clonezilla

**Interface Clonezilla :**

```
Clonezilla live (Default settings, VGA 800x600)
→ [Entrée]

Choose language : fr_FR.UTF-8 French | Français
→ [Entrée]

Ne pas toucher au mappage du clavier
→ [Entrée]

Start Clonezilla
→ [Entrée]

device-image   (Travailler avec disques ou partitions en utilisant des images)
→ [Entrée]

local_dev   (Monter un périphérique local)
→ [Entrée]

[Insérer votre clé USB ou configurer le serveur SSH]
→ [Entrée] après détection

Sélectionner le périphérique de destination pour l'image
→ Choisir votre clé USB (ex : sdb1)
→ [Entrée]

Mode Beginner   (Mode débutant: Accepter les options par défaut)
→ [Entrée]

savedisk   (Sauvegarder_disque_local_en_image)
→ [Entrée]
```

### 2.3. Nom de l'Image et Lancement

```
Nom de l'image : 2024-11-20-Win11-Office-img
→ [Entrée]

Disque source à sauvegarder : sda (disque de la VM)
→ Espace pour sélectionner, [Entrée]

Vérifier/réparer le système de fichiers avant de sauvegarder : -fsck
→ [Entrée]

Vérifier l'image sauvegardée : -scs (Skip checking)
→ [Entrée] (recommandé pour gagner du temps en TP)

Choisir si vous voulez chiffrer l'image : -senc (Skip)
→ [Entrée]

Action à effectuer après avoir terminé : poweroff
→ [Entrée]

Appuyez sur [Entrée] pour continuer...
→ [Entrée]

Êtes-vous sûr de vouloir continuer ? (y/n)
→ y [Entrée]
```

**La capture démarre.** Durée estimée : **10-20 minutes** selon la taille du disque et la vitesse USB.

**État d'avancement affiché :**
```
Cloning /dev/sda to /home/partimag/2024-11-20-Win11-Office-img
Rate: 2.1 GB/min, Estimated time remaining: 00:03:45
```

Une fois terminé, la VM s'éteint automatiquement.

### 2.4. Vérification de l'Image

Sur votre clé USB (ou serveur), vous devez trouver :

```
/home/partimag/2024-11-20-Win11-Office-img/
├── sda-chs.sf          (Géométrie du disque)
├── sda-pt.sf           (Table de partitions)
├── sda1.ntfs-ptcl-img.gz.aa   (Partition système compressée)
├── sda1.ntfs-ptcl-img.gz.ab
├── ...
├── Info-dmi.txt        (Infos matériel)
└── blkdev.list         (Liste des périphériques)
```

---

## PHASE 3 — Déploiement de l'Image (20 min)

### 3.1. Boot Clonezilla sur la Machine Cible

1. Attacher l'ISO Clonezilla à la **VM 2** (machine cible vierge)
2. Boot sur Clonezilla Live
3. Même choix de langue/clavier qu'à l'étape 2.1

### 3.2. Restauration de l'Image

```
Start Clonezilla
→ [Entrée]

device-image
→ [Entrée]

local_dev
→ [Entrée]

[Insérer la même clé USB avec l'image]
→ Sélectionner sdb1

Mode Beginner
→ [Entrée]

restoredisk   (Restaurer_image_vers_disque_local)
→ [Entrée]

Sélectionner l'image à restaurer : 2024-11-20-Win11-Office-img
→ [Entrée]

Disque cible : sda (disque de la VM 2)
→ Espace puis [Entrée]

⚠️ AVERTISSEMENT : Toutes les données sur sda seront EFFACÉES
Êtes-vous sûr de vouloir continuer ? (y/n)
→ y [Entrée]

Le nom de périphérique du disque cible est sda.
Êtes-vous sûr de vouloir continuer ? (y/n)
→ y [Entrée]
```

**Le déploiement démarre.** Durée : **5-15 minutes**.

Une fois terminé :
```
Action à effectuer : reboot
→ [Entrée]
```

Retirer l'ISO Clonezilla et laisser la VM 2 redémarrer normalement.

---

## PHASE 4 — Post-Configuration et Validation (25 min)

### 4.1. Première Configuration Windows (OOBE)

Si Sysprep a été exécuté correctement, l'assistant de configuration Windows s'affiche :

| **Étape** | **Choix recommandé** |
|---|---|
| Région | France |
| Disposition clavier | Français (France) |
| Nom de la machine | `PC-CLONE-01` (ou selon convention) |
| Compte utilisateur | `Admin-TP` (compte local) |
| Mot de passe | `MotDePasse123!` |
| Questions de sécurité | Remplir 3 questions |

Terminer la configuration → Windows démarre sur le bureau.

### 4.2. Vérification du SID

**Pourquoi vérifier le SID ?** Pour s'assurer que Sysprep a bien généralisé l'image et que chaque clone a un SID unique.

```cmd
# Ouvrir cmd en Administrateur
whoami /user
```

**Résultat attendu :**
```
INFORMATIONS SUR L'UTILISATEUR
------------------------------

Nom d'utilisateur          SID
========================== ========================================
PC-CLONE-01\Admin-TP       S-1-5-21-XXXXXXXXXX-YYYYYYYYYY-ZZZZZZZZZZ-1001
```

Comparer le SID avec celui de la machine de référence (si disponible). Ils doivent être **différents**.

### 4.3. Tests de Validation

| **Test** | **Procédure** | **Résultat attendu** |
|---|---|---|
| **Logiciels installés** | Vérifier que 7-Zip, VLC, Firefox, Adobe sont présents | ✅ Tous présents |
| **Fond d'écran** | Vérifier la personnalisation | ✅ Fond d'écran personnalisé affiché |
| **Raccourci bureau** | Ouvrir "Mes Applications" | ✅ Pointe vers C:\Program Files |
| **Connexion réseau** | `ping 8.8.8.8` | ✅ Réponse reçue |
| **Activation Windows** | Paramètres → Mise à jour et sécurité → Activation | État visible (activé ou non selon clé) |

### 4.4. Tableau de Comparaison

| **Élément** | **Machine de référence (VM 1)** | **Clone déployé (VM 2)** | **Identique ?** |
|---|---|---|---|
| Nom de machine | | PC-CLONE-01 | ❌ (normal) |
| SID | S-1-5-21-123... | S-1-5-21-456... | ❌ (normal) |
| Logiciels installés | 7-Zip, VLC, Firefox, Adobe | | ✅ |
| Fond d'écran | Personnalisé | | ✅ |
| Espace disque utilisé | ~15 Go | | ✅ |

---

## PHASE 5 — Documentation (Lien S11)

Rédiger une **procédure de déploiement** selon le modèle S11 (Annexe 2) incluant :

1. **Objectif** : Déployer une image Windows 11 standardisée
2. **Prérequis** : Clonezilla Live, clé USB 16 Go, 2 VMs
3. **Étapes** : Phase 1 à 4 (résumées avec captures clés)
4. **Troubleshooting** : 2 erreurs courantes (voir Annexe 1)
5. **Références** : Lien vers documentation Clonezilla officielle

---

---

# 📄 ANNEXE 1 — TROUBLESHOOTING CLONEZILLA

*Erreurs courantes et résolutions*

---

## Erreur 1 — "No valid image found"

**Symptôme :** Lors de la restauration, Clonezilla ne trouve pas l'image capturée.

**Causes possibles :**
1. Mauvais périphérique sélectionné (mauvaise clé USB ou partition)
2. Image corrompue ou incomplète
3. Nom d'image avec caractères spéciaux

**Solutions :**
1. Vérifier que le bon périphérique est monté (même clé USB que pour la capture)
2. Lister les images disponibles manuellement :
   ```bash
   ls /home/partimag/
   ```
3. Si l'image est corrompue, refaire la capture

---

## Erreur 2 — "Target disk is too small"

**Symptôme :** Le disque cible est plus petit que le disque source.

**Causes :**
- Disque cible de 120 Go, image capturée depuis un disque de 500 Go

**Solutions :**
1. Utiliser un disque cible au moins aussi grand que le disque source
2. Avant la capture, réduire la partition source pour qu'elle tienne sur le disque cible :
   - Windows : Gestion des disques → Réduire le volume
   - Clonezilla : Utiliser "saveparts" (partition) au lieu de "savedisk" (disque entier)

---

## Erreur 3 — Après déploiement, Windows affiche "Preparing Automatic Repair"

**Symptôme :** Windows ne démarre pas normalement après déploiement.

**Causes possibles :**
1. Sysprep mal exécuté ou non exécuté
2. Corruption de l'image lors du transfert

**Solutions :**
1. Refaire la capture en s'assurant que Sysprep a bien été exécuté avec `/generalize /oobe`
2. Vérifier l'intégrité de l'image après capture (option `-sck` dans Clonezilla)

---

## Erreur 4 — Les deux clones ont le même SID

**Symptôme :** `whoami /user` affiche le même SID sur deux machines différentes.

**Cause :** Sysprep n'a pas été exécuté avant la capture.

**Solution :**
1. Recréer la machine de référence proprement
2. **Exécuter Sysprep AVANT de capturer l'image**
3. Redéployer l'image

---

# 📄 ANNEXE 2 — CHECKLIST SYSPREP

*À cocher AVANT de capturer l'image Windows*

---

| **Vérification** | **✓** |
|---|---|
| Tous les logiciels sont installés | ☐ |
| Toutes les mises à jour Windows sont appliquées | ☐ |
| Les fichiers temporaires ont été nettoyés | ☐ |
| La corbeille est vide | ☐ |
| Aucun compte utilisateur personnel n'est présent | ☐ |
| Aucune donnée personnelle (Documents, Images...) | ☐ |
| Le fond d'écran corporate est configuré | ☐ |
| Les paramètres système sont optimisés | ☐ |
| **SYSPREP A ÉTÉ EXÉCUTÉ** | ☐ |
| La machine s'est éteinte automatiquement après Sysprep | ☐ |
| **LA MACHINE N'A PAS ÉTÉ REDÉMARRÉE APRÈS SYSPREP** | ☐ |

> ⚠️ **Si un seul de ces points n'est pas coché, NE PAS CAPTURER L'IMAGE.**

---

*Pack S12 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B2.1, B2.2, B1.4, B3.3*
*Déploiement d'images · Clonage · Clonezilla · Sysprep · Golden image · WDS/MDT (aperçu)*
