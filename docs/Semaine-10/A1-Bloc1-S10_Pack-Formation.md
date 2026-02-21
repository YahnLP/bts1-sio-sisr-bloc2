# Pack de Formation - Semaine 10 (S10) - BLOC 1
## 🔧 Gestion des Configurations · Versioning · Retour Alternance · Clôture Bloc 1

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S10 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — dixième semaine |
| **Modalité** | Présentiel — salle de cours |
| **Prérequis** | S1 à S9 — Bloc 1 complet + 2 mois d'alternance en entreprise |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.2** | Exploiter des référentiels, normes et standards (ITIL — Configuration Management) | Maîtrise |
| **B2.3** | Gérer les accès et les services réseaux (préfiguration Bloc 2) | Découverte |
| **B3.3** | Participer à la gestion et au suivi d'un projet | Maîtrise |
| **E5 SPS** | Exploiter les situations professionnelles vécues en entreprise | Acquisition |

> 📌 **S10 est la séance charnière qui clôture le Bloc 1 et ouvre le Bloc 2.** Elle remplit trois fonctions stratégiques : (1) introduire la **gestion des configurations** comme dernière brique ITIL du Bloc 1, avec un volet technique qui préfigure le Bloc 2 ; (2) organiser le **premier retour d'alternance structuré** — transformer les expériences vécues en entreprise en SPS exploitables ; (3) formaliser la **clôture du Bloc 1** avec un bilan des 10 semaines et une annonce des changements du Bloc 2.

---

## Objectifs Pédagogiques

**Gestion des configurations :**
- ✅ Définir la **gestion des configurations** selon ITIL (Configuration Management)
- ✅ Distinguer une **baseline de configuration** d'une configuration active
- ✅ Expliquer le concept de **versioning** et pourquoi il est essentiel
- ✅ Sauvegarder et comparer des **configurations d'équipements réseau** (running-config vs startup-config)
- ✅ Documenter une configuration dans un **dépôt versionné** (Git ou fichiers horodatés)

**Retour alternance :**
- ✅ Partager en groupe les **situations professionnelles vécues** en entreprise
- ✅ Identifier les situations **valorisables en SPS** pour le dossier E5
- ✅ Formuler les **difficultés rencontrées** en entreprise et les solutions trouvées
- ✅ Relier les **compétences RNCP** aux missions réelles en entreprise

---

## ⭐ Spécificités Pédagogiques

### La Gestion des Configurations : Double Lecture

Le terme "gestion des configurations" a deux acceptions en IT :

**Sens ITIL (Configuration Management) :**
Gérer tous les **éléments de configuration** (CI) de l'infrastructure dans la CMDB : serveurs, postes, logiciels, licences, relations entre composants. Maintenir une vue fiable de "ce qui existe" et de ses versions.

**Sens technique réseau/système :**
Sauvegarder, versionner et restaurer les **fichiers de configuration** des équipements actifs (routeurs, switches, serveurs, firewalls) pour éviter la perte de configuration en cas de panne ou d'erreur.

S10 couvre les deux, car :
- Le sens ITIL clôture le Bloc 1 (dernière pratique ITIL non vue)
- Le sens technique ouvre le Bloc 2 (on manipule des configs réseau pour la première fois)

### Le Retour d'Alternance : Capitaliser sur 2 Mois

Les apprenants arrivent en S10 avec environ **2 mois d'expérience en entreprise** (septembre-novembre). Beaucoup ont déjà réalisé des missions exploitables — mais ne savent pas encore qu'elles le sont.

**Le piège classique** : l'apprenant qui dit "j'ai rien fait d'intéressant, j'ai juste installé des postes et réinitialisé des mots de passe". Or ces missions, bien documentées, sont parfaitement recevables en E5.

**La stratégie pédagogique de S10 :**
1. **Débriefing collectif** (tour de table structuré) pour désacraliser : tout le monde partage
2. **Exploitation individuelle guidée** : transformer 1 mission en brouillon de SPS #2
3. **Objectif explicite** : chaque apprenant repart avec une SPS #2 en cours de rédaction

### Clôture du Bloc 1 : Ritualiser la Transition

La clôture du Bloc 1 doit être **ritualisée** : c'est la fin d'une étape de 10 semaines. Les apprenants ont acquis une base solide (support, incidents, ITIL, GLPI, communication). Le Bloc 2 sera très différent (technique, infrastructure, scripting).

Verbaliser cette transition clairement : "Vous savez maintenant gérer le support IT. À partir de S11, vous allez apprendre à *construire* l'infrastructure qui supporte les services."

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S9 | Feedback SPS #1 — points forts et points d'amélioration collectifs |
| H+0:10 | 15 min | 🎯 Découverte | Activité "La Config Perdue" |
| H+0:25 | 35 min | 📖 Cours | Gestion des configurations : ITIL, baseline, versioning |
| H+1:00 | 25 min | 🔧 Démo | Sauvegarde de configs réseau (Cisco running/startup ou switch simulé) |
| H+1:25 | **15 min** | ☕ **PAUSE** | — |
| H+1:40 | 40 min | 🗣️ **Retour alternance** | Tour de table structuré — partage des missions en entreprise |
| H+2:20 | 50 min | ✍️ **TP** | Exploitation individuelle : brouillon SPS #2 à partir d'une mission entreprise |
| H+3:10 | 20 min | 🎓 **Bilan Bloc 1** | Synthèse des 10 semaines — quiz ludique — compétences acquises |
| H+3:30 | 20 min | 🔭 **Annonce Bloc 2** | Ce qui change : réseaux, systèmes, scripting — calendrier S11-S20 |
| H+3:50 | 10 min | 📅 **Calendrier** | Dates clés Bloc 2 — projets — examens blancs |

---

## Différenciation Pédagogique

### Profil Avancé
- **Configs :** Manipuler directement un switch Cisco (physique ou Packet Tracer) — commandes `show running-config`, `copy running-config startup-config`, export TFTP
- **Versioning :** Initialiser un dépôt Git pour stocker les configs réseau, avec commit horodaté
- **SPS #2 :** Rédiger une SPS technique (installation serveur, configuration VLAN, script automatisation) si mission complexe disponible

### Profil Débutant
- **Configs :** Comprendre le concept par analogie (Word : brouillon vs document enregistré)
- **TP :** Utiliser le canevas SPS Annexe 2 de S9 pour structurer la mission entreprise
- **Binômage :** Les apprenants sans mission exploitable rédigent une SPS sur un TP de formation (S4-S6)

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **Switch Cisco ou simulateur** | Packet Tracer ou GNS3 — optionnel selon équipement disponible |
| **Captures de configs** | running-config et startup-config à distribuer pour analyse |
| **Grille de retour alternance** | Annexe 1 — imprimée pour chaque apprenant |
| **Canevas SPS #2** | Annexe 2 — réutilisation du S9 |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "La Config Perdue"

*Durée : 15 minutes — Collectif*

---

## Scénario (5 min)

L'enseignant lit la mise en situation :

---

> **Contexte :** Vous êtes technicien réseau dans une PME. Vendredi soir 18h, vous configurez le switch principal de l'entreprise pour ajouter un nouveau VLAN pour le service Marketing. Vous testez — tout fonctionne. Vous partez en weekend.
>
> **Lundi matin 8h :** Panne de courant pendant la nuit. Le switch redémarre. En arrivant, vous constatez que le nouveau VLAN Marketing a disparu. Les 12 postes du Marketing n'ont plus de réseau. Vous avez 45 minutes pour tout reconfigurer avant l'ouverture des bureaux.
>
> **Problème :** Vous n'avez pas sauvegardé la configuration. Vous ne vous souvenez plus de tous les paramètres (adresses IP, ports assignés, ACL...).

---

## Questions Guidées (7 min)

| **Question** | **Concept visé** |
|---|---|
| "Qu'est-ce qui a causé la perte de configuration ?" | Running-config (RAM) vs startup-config (NVRAM) |
| "Qu'aurait-il fallu faire vendredi soir ?" | Sauvegarder la config (`copy run start`) |
| "Si vous aviez sauvegardé, auriez-vous pu la retrouver 6 mois plus tard ?" | Versioning — garder des copies datées |
| "Est-ce que ce problème existe aussi pour les serveurs, pas seulement les switchs ?" | Oui — configs Apache, DHCP, AD... |
| "Comment éviter ce problème de façon systématique ?" | Procédure obligatoire + automatisation |

## Conclusion (3 min)

L'enseignant écrit au tableau :

```
   RÈGLE D'OR EN INFRASTRUCTURE IT

   ① Toute modification de configuration doit être sauvegardée
   ② Toute sauvegarde doit être versionnée (datée)
   ③ Toute version doit être documentée (quoi, quand, pourquoi, par qui)

   Sans ça → perte possible à chaque redémarrage / panne / erreur
```

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Gestion des Configurations · Versioning · ITIL Configuration Management"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 10*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.2** | Exploiter des référentiels (ITIL Configuration Management) |
| **B2.3** | Gérer les accès et services réseaux (configs équipements) |
| **B3.3** | Documenter les configurations |

---

## PARTIE I — Gestion des Configurations selon ITIL

### I.A. Configuration Management — Définition

En ITIL 4, la **gestion des configurations** (Configuration Management) est la pratique qui consiste à maintenir une information fiable et à jour sur tous les **éléments de configuration** (CI — Configuration Items) de l'infrastructure IT et leurs relations.

```
   CMDB (Configuration Management DataBase)
   ────────────────────────────────────────────────────────────
   Base de données centrale contenant tous les CI et leurs relations

   Exemples de CI :
   ├── Serveurs (physiques, virtuels)
   ├── Équipements réseau (switches, routeurs, firewalls)
   ├── Postes de travail (fixes, portables)
   ├── Logiciels (OS, applications, versions)
   ├── Licences (nombre, type, expiration)
   ├── Documents (DAT, procédures, schémas)
   └── Relations entre CI (ce serveur héberge cette application,
       cette application utilise cette base de données...)
```

---

### I.B. Baseline de Configuration

Une **baseline** (ou **référence de configuration**) est un instantané figé de la configuration d'un CI à un moment donné, qui sert de référence pour les changements futurs.

```
   EXEMPLE — Baseline Serveur Web Production
   ─────────────────────────────────────────────────────────────
   Baseline v1.0 — 2024-09-15
   ├── Serveur : SRV-WEB-01 / Ubuntu 22.04.3 LTS
   ├── Apache 2.4.52
   ├── PHP 8.1.12
   ├── MariaDB 10.6.12
   ├── Sites hébergés : intranet.siosarl.local, catalogue.siosarl.local
   ├── Certificat SSL : Let's Encrypt — expire 2024-12-10
   ├── Configuration réseau : IP 192.168.10.50/24, GW .1, DNS .10
   ├── Fichiers de config : /etc/apache2/sites-available/*
   └── Date de création baseline : 2024-09-15

   → Tout changement par rapport à cette baseline doit être documenté
     et validé (Change Management)
```

> 💡 **Pourquoi une baseline ?** Sans baseline, on ne peut pas savoir si une configuration actuelle est conforme à ce qu'elle devrait être. C'est la référence pour les audits de conformité et les retours arrière (rollback) en cas de problème.

---

### I.C. Relations entre CI dans la CMDB

Un CI ne vit jamais seul. La CMDB enregistre les **dépendances** entre CI :

```
   Exemple de relations :
   ─────────────────────────────────────────────────────────────
   APPLICATION INTRANET
       │
       ├── Hébergée sur → SRV-WEB-01
       ├── Utilise → BASE-INTRANET (MariaDB sur SRV-DB-01)
       ├── Nécessite → Licence PHP (10 utilisateurs simultanés)
       └── Accessible via → Switch-CoreN1 (VLAN 20)

   Impact :
   Si SRV-WEB-01 tombe en panne → l'intranet est inaccessible
   Si Switch-CoreN1 redémarre → tous les services sur VLAN 20 coupés
   Si la licence PHP expire → l'intranet peut cesser de fonctionner
```

> 📌 **Utilité en gestion d'incidents :** Quand un incident P1 survient ("L'intranet est HS"), la CMDB permet de voir immédiatement tous les composants impliqués et de remonter la chaîne de dépendance pour identifier la cause.

---

## PARTIE II — Versioning des Configurations

### II.A. Pourquoi Versionner ?

Le **versioning** (ou **gestion de versions**) consiste à garder une trace de toutes les versions successives d'un fichier de configuration, avec l'horodatage et l'auteur de chaque modification.

| **Sans versioning** | **Avec versioning** |
|---|---|
| On écrase l'ancienne config à chaque modification | Chaque modification crée une nouvelle version datée |
| En cas d'erreur, on ne peut pas revenir en arrière | On peut restaurer n'importe quelle version antérieure |
| On ne sait pas qui a changé quoi ni quand | Chaque changement est tracé (auteur, date, raison) |
| Impossible de comparer deux états du système | Diff entre versions pour voir ce qui a changé |

---

### II.B. Méthodes de Versioning

| **Méthode** | **Principe** | **Avantages** | **Inconvénients** | **Usage** |
|---|---|---|---|---|
| **Fichiers horodatés** | Copier le fichier avec date dans le nom | Simple, universel | Pas de diff automatique, volume de stockage | Petite infra, configs manuelles |
| **Git / SVN** | Dépôt versionné avec historique complet | Diff, merge, rollback, commentaires | Nécessite apprentissage Git | Infra moyenne à grande |
| **Outils spécialisés** | Rancid, Oxidized (pour équipements réseau) | Automatisation, alertes sur changement non autorisé | Configuration initiale complexe | Datacenter, parc réseau important |
| **Backup CMDB** | Sauvegarde automatique de la CMDB GLPI | Intégré ITSM | Pas de granularité fichier | PME avec GLPI |

---

### II.C. Fichiers Horodatés — Convention de Nommage

Si vous gérez les configurations manuellement (sans Git), respecter une **convention de nommage stricte** :

```
   Format recommandé :
   [Type]_[Équipement]_[YYYYMMDD]_[Version].[extension]

   Exemples :
   config_Switch-Core1_20241015_v1.0.txt
   config_SRV-DHCP_20241022_v2.3.conf
   baseline_Firewall-PFSense_20241101_v1.0.xml

   Arborescence :
   /backup/configs/
   ├── switches/
   │   ├── Switch-Core1/
   │   │   ├── config_Switch-Core1_20241001_v1.0.txt
   │   │   ├── config_Switch-Core1_20241015_v1.1.txt
   │   │   └── config_Switch-Core1_20241101_v2.0.txt
   │   └── Switch-Distrib-RH/
   ├── serveurs/
   └── firewalls/

   + Un fichier CHANGELOG.md par équipement :
   Switch-Core1 — Historique des modifications
   v2.0 — 2024-11-01 — Ajout VLAN 30 pour Marketing — Auteur: [Nom]
   v1.1 — 2024-10-15 — Correction ACL port 22 — Auteur: [Nom]
   v1.0 — 2024-10-01 — Configuration initiale — Auteur: [Nom]
```

---

## PARTIE III — Configurations Réseau (Running vs Startup)

### III.A. Cisco IOS — Deux Configurations

Les équipements réseau Cisco (et la plupart des constructeurs) utilisent deux emplacements de stockage pour la configuration :

```
   ┌──────────────────────────────────────────────────────────────┐
   │               RUNNING-CONFIG                                  │
   │  Stockage : RAM (volatile — effacée au redémarrage)           │
   │  Fichier  : running-config                                    │
   │  Rôle     : Configuration ACTIVE, en cours d'utilisation       │
   │  Commande : show running-config                                │
   │                                                               │
   │  C'est la config actuellement appliquée sur l'équipement.     │
   │  Toute modification (ajout VLAN, changement IP...) modifie     │
   │  d'abord la running-config.                                    │
   └──────────────────────────────────────────────────────────────┘

   ┌──────────────────────────────────────────────────────────────┐
   │               STARTUP-CONFIG                                  │
   │  Stockage : NVRAM (non-volatile — survit au redémarrage)       │
   │  Fichier  : startup-config                                    │
   │  Rôle     : Configuration SAUVEGARDÉE, chargée au boot         │
   │  Commande : show startup-config                                │
   │                                                               │
   │  C'est la config que l'équipement chargera au prochain         │
   │  redémarrage. Si on ne sauvegarde pas la running-config        │
   │  vers la startup-config, les modifications sont perdues.       │
   └──────────────────────────────────────────────────────────────┘
```

---

### III.B. Commandes Cisco IOS Essentielles

```cisco
! ─── Voir la configuration active ───
Switch# show running-config
! Affiche toute la config en RAM (peut être long)

! ─── Voir la configuration de démarrage ───
Switch# show startup-config
! Affiche la config qui sera chargée au prochain boot

! ─── Comparer les deux configs ───
Switch# show archive config differences
! Montre les différences entre running et startup (si disponible)

! ─── Sauvegarder la running-config vers la startup-config ───
Switch# copy running-config startup-config
! ou raccourci :
Switch# write memory
Switch# wr

! ─── Exporter la config vers un serveur TFTP ───
Switch# copy running-config tftp:
! Puis entrer l'IP du serveur TFTP et le nom de fichier

! ─── Restaurer une config depuis TFTP ───
Switch# copy tftp: running-config
! Attention : fusionne avec la config existante, ne la remplace pas

! ─── Effacer la startup-config (reset factory) ───
Switch# erase startup-config
! Au prochain redémarrage, l'équipement démarre vierge
```

---

### III.C. Workflow Professionnel de Modification

```
   ÉTAPE 1 — SAUVEGARDE PRÉVENTIVE
   ──────────────────────────────────────────────────────────────
   Avant toute modification, sauvegarder la config actuelle :
   Switch# copy running-config tftp:
   Destination : backup_Switch-Core1_20241115_avant-modif.txt

   ÉTAPE 2 — MODIFICATION EN MODE CONFIG
   ──────────────────────────────────────────────────────────────
   Switch# configure terminal
   Switch(config)# [commandes de modification]
   Switch(config)# exit

   ÉTAPE 3 — TEST ET VALIDATION
   ──────────────────────────────────────────────────────────────
   Vérifier que la modification fonctionne (ping, accès, VLAN...)
   Si KO → annuler (reload sans sauvegarder)
   Si OK → passer à l'étape 4

   ÉTAPE 4 — SAUVEGARDE PERMANENTE
   ──────────────────────────────────────────────────────────────
   Switch# copy running-config startup-config
   → La modification survivra au redémarrage

   ÉTAPE 5 — DOCUMENTATION
   ──────────────────────────────────────────────────────────────
   Exporter la nouvelle config et mettre à jour le CHANGELOG :
   Switch# copy running-config tftp:
   Destination : config_Switch-Core1_20241115_v2.1.txt

   Fichier CHANGELOG.md :
   v2.1 — 2024-11-15 — Ajout VLAN 40 Commerce — Auteur: [Nom]
```

---

### III.D. Comparer Deux Versions de Configuration

Pour identifier ce qui a changé entre deux versions, utiliser un outil de diff :

**Sous Linux :**
```bash
diff -u config_Switch_20241001_v1.0.txt config_Switch_20241115_v2.1.txt

# Ou pour une sortie plus lisible :
diff -u config_Switch_20241001_v1.0.txt config_Switch_20241115_v2.1.txt | colordiff
```

**Sous Windows :**
- WinMerge (gratuit, interface graphique)
- Notepad++ avec plugin Compare
- Visual Studio Code avec extension GitLens

**En ligne :**
- diffchecker.com (copier-coller les deux configs)

---

## PARTIE IV — Versioning avec Git (Aperçu)

### IV.A. Pourquoi Git pour les Configs ?

**Git** n'est pas réservé au code source — il est parfait pour versionner des fichiers de configuration texte :

| **Avantage Git** | **Exemple sur configs réseau** |
|---|---|
| Historique complet | Voir tous les changements depuis 2 ans |
| Auteur et date | Savoir qui a modifié quoi et quand |
| Commentaire de commit | "Ajout VLAN 40 pour le service Commerce — Ticket GLPI #1234" |
| Diff automatique | `git diff` montre exactement les lignes modifiées |
| Rollback facile | Revenir à une version antérieure en 1 commande |
| Branches | Tester une modif dans une branche sans toucher à la prod |

---

### IV.B. Workflow Git pour Configs — Exemple

```bash
# ─── Initialisation du dépôt (une seule fois) ───
cd /backup/configs
git init
git config user.name "Technicien Réseau"
git config user.email "technicien@siosarl.local"

# ─── Ajouter une première config ───
# (après avoir exporté depuis le switch)
cp ~/downloads/config_Switch-Core1.txt switches/Switch-Core1.txt
git add switches/Switch-Core1.txt
git commit -m "Config initiale Switch-Core1 — v1.0"

# ─── 2 semaines plus tard : modification du switch ───
# (exporter la nouvelle config)
cp ~/downloads/config_Switch-Core1_nouvelle.txt switches/Switch-Core1.txt
git add switches/Switch-Core1.txt
git commit -m "Ajout VLAN 40 Commerce — Ticket GLPI #1234"

# ─── Voir l'historique ───
git log --oneline
# Affiche :
# a3f82c1 Ajout VLAN 40 Commerce — Ticket GLPI #1234
# e7b12f4 Config initiale Switch-Core1 — v1.0

# ─── Voir ce qui a changé entre deux commits ───
git diff e7b12f4 a3f82c1

# ─── Revenir à une version antérieure (rollback) ───
git checkout e7b12f4 -- switches/Switch-Core1.txt
# Le fichier est restauré à la version initiale
# Il faut ensuite le réappliquer sur le switch
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Configuration Management** | Pratique ITIL de gestion de tous les CI et leurs relations dans la CMDB |
| **CI (Configuration Item)** | Tout élément de l'infrastructure géré dans la CMDB (serveur, switch, logiciel...) |
| **Baseline** | Référence de configuration figée à un instant T, servant de base pour les changements |
| **Versioning** | Gestion des versions successives d'un fichier avec horodatage et traçabilité |
| **Running-config** | Configuration active d'un équipement réseau (stockée en RAM, volatile) |
| **Startup-config** | Configuration de démarrage d'un équipement réseau (stockée en NVRAM, persistante) |
| **NVRAM** | Non-Volatile RAM — mémoire qui conserve les données sans alimentation |
| **TFTP** | Trivial File Transfer Protocol — protocole simple pour transférer des fichiers (configs) |
| **Diff** | Comparaison de deux fichiers pour identifier les lignes modifiées |
| **Rollback** | Retour à une version antérieure d'une configuration |
| **Commit** | Enregistrement d'une version dans un système de versioning (Git) |
| **CHANGELOG** | Fichier documentant l'historique des modifications d'une configuration |
| **Copy run start** | Commande Cisco pour sauvegarder la running-config vers la startup-config |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Je définis Configuration Management selon ITIL
- [ ] J'explique ce qu'est une baseline et pourquoi elle est utile
- [ ] Je distingue running-config (RAM) et startup-config (NVRAM)
- [ ] Je sais sauvegarder une running-config vers startup-config
- [ ] J'applique une convention de nommage cohérente aux fichiers de config
- [ ] J'explique pourquoi versionner les configs est essentiel
- [ ] Je documente un changement de configuration (qui, quoi, quand, pourquoi)
- [ ] Je peux comparer deux versions de configuration avec diff

---

---

# 🗣️ RETOUR ALTERNANCE — GRILLE DE PARTAGE

*Durée : 40 minutes — Tour de table structuré*

---

## Objectif

Chaque apprenant partage **1 à 2 situations professionnelles vécues** en entreprise depuis septembre. L'enseignant guide le partage et aide le groupe à identifier les situations exploitables en SPS.

---

## Règles du Tour de Table

1. **Temps de parole** : 3 minutes maximum par apprenant
2. **Pas de jugement** : Toutes les missions sont valorisables si bien documentées
3. **Questions du groupe** : Après chaque partage, 2 questions rapides sont autorisées
4. **L'enseignant note** : Les missions potentiellement riches en SPS

---

## Grille de Partage Individuel

*Chaque apprenant remplit cette grille AVANT le tour de table (10 min de préparation)*

**Prénom :** _________________________ **Entreprise :** _______________________

---

### Mission 1 — La Plus Fréquente

| **Champ** | **Votre réponse** |
|---|---|
| **Quel type de mission faites-vous le plus souvent ?** | ☐ Helpdesk / tickets<br>☐ Installation de postes<br>☐ Gestion de comptes utilisateurs<br>☐ Maintenance matérielle<br>☐ Configuration réseau<br>☐ Autre : _________ |
| **Exemple concret (1 phrase)** | |
| **Compétence RNCP mobilisée** | B1.__ ou B2.__ |
| **Avez-vous des preuves ?** | ☐ Tickets GLPI<br>☐ Captures d'écran<br>☐ Emails / procédures<br>☐ Aucune pour l'instant |

---

### Mission 2 — La Plus Intéressante

| **Champ** | **Votre réponse** |
|---|---|
| **Quelle mission vous a semblé la plus formatrice ou intéressante ?** | |
| **Pourquoi ?** | |
| **Qu'avez-vous appris ?** | |
| **Compétence RNCP** | B1.__ ou B2.__ |

---

### Difficultés Rencontrées

| **Difficulté** | **☑** | **Comment vous l'avez surmontée (ou pas encore)** |
|---|---|---|
| Manque de connaissances techniques | ☐ | |
| Manque de procédures / documentation | ☐ | |
| Communication avec les utilisateurs | ☐ | |
| Gestion du stress / de l'urgence | ☐ | |
| Autre : _________________ | ☐ | |

---

### Besoin de Support / Conseil

> *Y a-t-il une situation en entreprise sur laquelle vous aimeriez avoir l'avis du groupe ou de l'enseignant ?*

```
___________________________________________________________________
___________________________________________________________________
```

---

---

# ✍️ TP — EXPLOITATION MISSION ENTREPRISE → SPS #2

*Durée : 50 minutes — Individuel*

---

## Consignes

Choisir **1 mission vécue en entreprise** depuis septembre et rédiger le **brouillon de la SPS #2** en suivant la structure vue en S9.

Si vous n'avez pas encore de mission exploitable en entreprise, rédiger la SPS #2 sur une mission de formation (TP S4-S6).

---

## Étape 1 — Choix de la Mission (5 min)

**Mission choisie :** ___________________________________________________

**Pourquoi cette mission ?** (Cocher les critères qui s'appliquent)
- ☐ J'ai des preuves concrètes (tickets, captures, documents)
- ☐ J'ai pris des décisions techniques justifiables
- ☐ J'ai rencontré une difficulté que j'ai surmontée
- ☐ Ça mobilise une compétence RNCP que je veux valoriser
- ☐ C'est représentatif de mon rôle en entreprise

---

## Étape 2 — Structure SPS #2 (40 min)

*Utiliser le canevas Annexe 2 si besoin. Rédiger directement sur cette fiche ou sur ordinateur.*

### ① CONTEXTE (5-8 lignes)

```
Organisation : _________________________________________________________
Infrastructure IT : ____________________________________________________
Votre rôle : ___________________________________________________________
Période : ______________________________________________________________

___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
```

---

### ② MISSION (3-5 lignes)

```
Objectif : _____________________________________________________________
Assigné par : __________________________________________________________
Contraintes : __________________________________________________________

___________________________________________________________________
___________________________________________________________________
```

---

### ③ RÉALISATION (15+ lignes — section principale)

**Ce que vous avez fait, étape par étape :**
```
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
```

**Décisions prises et justifications :**
```
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
```

**Difficultés et résolutions :**
```
___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
```

**Outils / Commandes / Méthodes utilisés :**
```
___________________________________________________________________
___________________________________________________________________
```

---

### ④ COMPÉTENCES MOBILISÉES

| **Code RNCP** | **Intitulé** | **Action concrète justifiant** |
|---|---|---|
| | | |
| | | |

---

### ⑤ RÉSULTATS ET VALIDATION

```
Comment avez-vous vérifié la réussite ? Qui a validé ?
___________________________________________________________________
___________________________________________________________________
```

---

### ⑥ RÉFLEXIVITÉ

**Ce que vous feriez différemment :**
```
___________________________________________________________________
___________________________________________________________________
```

**Ce que cette mission vous a appris :**
```
___________________________________________________________________
___________________________________________________________________
```

---

### ⑦ PREUVES DISPONIBLES

| **N°** | **Type de preuve** | **Description** | **Disponible ?** |
|---|---|---|---|
| P1 | | | ☐ Oui ☐ À récupérer |
| P2 | | | ☐ Oui ☐ À récupérer |
| P3 | | | ☐ Oui ☐ À récupérer |

---

## Étape 3 — Actions à Mener (5 min)

**Pour finaliser cette SPS #2, je dois :**

- ☐ Récupérer les captures d'écran manquantes
- ☐ Demander à mon tuteur une validation écrite
- ☐ Exporter les tickets GLPI correspondants
- ☐ Relire et enrichir la section Réflexivité
- ☐ Autre : __________________________________________________________

**Date cible de finalisation :** _______________________________

---

---

# 🎓 BILAN BLOC 1 — SYNTHÈSE DES 10 SEMAINES

*Durée : 20 minutes — Collectif*

---

## Quiz Ludique de Clôture (10 min)

L'enseignant pose 10 questions rapides (réponses à main levée) pour réviser le Bloc 1 :

| **Question** | **Réponse attendue** |
|---|---|
| Quelle commande Windows liste les logiciels installés ? | `wmic product get name,version` |
| Qu'est-ce qu'une licence OEM ? | Liée au matériel, non transférable |
| Que signifie MTTR ? | Mean Time To Repair |
| Différence entre incident et problème ITIL ? | Incident = interruption, Problème = cause racine |
| Disponibilité 99,9% = combien d'heures de panne/an ? | ~8h45 |
| Dans GLPI, différence suivi Privé vs Public ? | Privé = techniciens seulement, Public = utilisateur aussi |
| Agent OCS envoie les données via quel protocole ? | HTTP(S) |
| Que signifie RPO ? | Recovery Point Objective — perte de données max acceptable |
| Running-config vs startup-config ? | Running = RAM (volatile), Startup = NVRAM (persistante) |
| Combien de SPS minimum pour E5 ? | 3 minimum, 5 recommandées |

---

## Le Bloc 1 en Carte Mentale (5 min)

L'enseignant dessine au tableau (ou projette) la synthèse visuelle du Bloc 1 :

```
                         BLOC 1
        Support et Mise à Disposition de Services
                           │
           ┌───────────────┼───────────────┐
           │               │               │
       INVENTAIRE      SUPPORT IT     DOCUMENTATION
           │               │               │
    ┌──────┴──────┐   ┌───┴────┐     ┌────┴─────┐
    │             │   │        │     │          │
   S2 Fiche    S5 OCS  S3 ITIL  S4     S7 SLA   S9 Catalogue
   technique         S6 GLPI  Incidents  Mise à     services
                              KB         dispo    S10 Configs
                                                  versioning
```

---

## Compétences Acquises — Auto-positionnement (5 min)

Chaque apprenant remplit rapidement sa grille :

| **Compétence** | **Non acquis** | **En cours** | **Acquis** |
|---|---|---|---|
| B1.1 — Recenser les ressources | ☐ | ☐ | ☐ |
| B1.2 — Exploiter référentiels (ITIL) | ☐ | ☐ | ☐ |
| B1.3 — Outils de support (GLPI, tickets) | ☐ | ☐ | ☐ |
| B1.4 — Outils de gestion de parc (OCS) | ☐ | ☐ | ☐ |
| B1.5 — Mettre à disposition un service | ☐ | ☐ | ☐ |
| B1.6 — Assurer le support utilisateurs | ☐ | ☐ | ☐ |

**Ma compétence la plus forte dans le Bloc 1 :** ________________________

**Ma compétence à renforcer :** ______________________________________

---

---

# 🔭 ANNONCE DU BLOC 2

*Durée : 20 minutes — Présentation enseignant*

---

## Ce qui Change à Partir de S11

```
   BLOC 1 (S1-S10)              →         BLOC 2 (S11-S20)
   ────────────────                       ─────────────────
   Support et services                     Infrastructure et réseaux
   Utilisateur = client                    L'infrastructure = client
   Helpdesk, tickets, ITIL                 Configuration, installation
   Outils : GLPI, OCS                      Outils : CLI, scripts, Packet Tracer
   SISR en tant qu'utilisateur             SISR en tant que bâtisseur
   Communication, support                   Technique, architecture
```

---

## Programme S11-S20 — Aperçu

| **Semaines** | **Thème** | **Sujets principaux** |
|---|---|---|
| **S11-S13** | Réseaux 1 | Modèle OSI, TCP/IP, adressage IPv4, sous-réseaux, DHCP |
| **S14-S16** | Systèmes 1 | Linux administration, utilisateurs, services, scripting Bash |
| **S17-S18** | **Projet SimIO** | Installation infrastructure réseau et serveurs — évaluation projet |
| **S19-S20** | Approfondissement | Configuration avancée, scripting PowerShell, préparation CCNA |

---

## Les Projets du Bloc 2

**Projet 1 — S17-S18 : SimIO Infrastructure**
Installation complète de l'infrastructure réseau et serveurs de SimIO SARL :
- Configuration routeur et switch (VLANs)
- Serveur DHCP + DNS
- Serveur de fichiers Windows Server
- Documentation complète (DAT, PV, catalogue de services)
- **→ SPS #3 garantie pour le dossier E5**

**Projet 2 — S25-S26 (Année 1 Bloc 3) : SimIO Sécurité**
Sécurisation de l'infrastructure SimIO (pare-feu, VPN, monitoring)

---

## Évaluations du Bloc 2

| **Évaluation** | **Semaine** | **Format** | **Coefficient indicatif** |
|---|---|---|---|
| TP pratique réseau | S13 | Configuration switch, adressage | CC |
| Script Bash | S16 | Automatisation tâche système | CC |
| **Projet SimIO** | S18 | Soutenance + DAT | **×2** |
| Éval. formative 2 | S20 | QCM + mini-TP S11-S20 | Diagnostic |

---

## Conseils pour Réussir le Bloc 2

```
✅ À FAIRE                              ❌ À ÉVITER
───────────────────────────────────    ───────────────────────────────
Pratiquer en CLI (pas seulement GUI)   Se limiter aux clics d'interface
Scripter dès que possible              Répéter manuellement les tâches
Documenter chaque config               "Je me souviendrai"
Poser des questions technique          Rester bloqué sans demander
Lier Bloc 2 avec l'entreprise          Séparer formation et alternance
Préparer le CCNA dès maintenant        Attendre l'année 2
```

---

## Calendrier Bloc 2 — Dates Clés

| **Date** | **Événement** |
|---|---|
| S11 (sem. du ___) | Début Bloc 2 — Réseaux |
| S13 (sem. du ___) | TP pratique réseau évalué |
| S16 (sem. du ___) | Script Bash évalué |
| **S17-S18** (sem. du ___) | **Projet SimIO — soutenance** |
| S20 (sem. du ___) | Évaluation formative 2 |
| Vacances (__-__) | Pause — préparation SPS entreprise |

---

## Message de Clôture

> *"Le Bloc 1 vous a donné la posture et le vocabulaire du professionnel IT. Le Bloc 2 va vous donner les compétences techniques pour construire ce que vous supportez. Dans 10 semaines, vous saurez installer un réseau complet, configurer des serveurs, et automatiser des tâches par script. C'est une montée en technicité — mais vous avez la base solide pour y arriver."*

---

---

# 📄 ANNEXE 1 — MODÈLE DE CHANGELOG

*Pour documenter les modifications de configuration — Réutilisable pour tous les équipements*

---

```
═════════════════════════════════════════════════════════════════════
                      CHANGELOG — [NOM ÉQUIPEMENT]
═════════════════════════════════════════════════════════════════════
Équipement   : _______________________________________________________
Type         : ☐ Switch  ☐ Routeur  ☐ Serveur  ☐ Firewall  ☐ Autre
Localisation : _______________________________________________________
Responsable  : _______________________________________________________
═════════════════════════════════════════════════════════════════════

VERSION │ DATE       │ AUTEUR      │ MODIFICATION         │ TICKET/REF
────────┼────────────┼─────────────┼──────────────────────┼───────────
 1.0    │ YYYY-MM-DD │             │ Configuration        │
        │            │             │ initiale             │
────────┼────────────┼─────────────┼──────────────────────┼───────────
        │            │             │                      │
────────┼────────────┼─────────────┼──────────────────────┼───────────
        │            │             │                      │
────────┼────────────┼─────────────┼──────────────────────┼───────────
        │            │             │                      │
────────┼────────────┼─────────────┼──────────────────────┼───────────

═════════════════════════════════════════════════════════════════════
NOTES IMPORTANTES :
• Toujours sauvegarder la config AVANT modification (backup préventif)
• Tester en environnement non-production si possible
• Documenter POURQUOI la modification a été faite (pas seulement quoi)
• Référencer le ticket GLPI ou la demande de changement associée
═════════════════════════════════════════════════════════════════════
```

---

# 📄 ANNEXE 2 — SCRIPT DE SAUVEGARDE AUTOMATIQUE (EXEMPLE BASH)

*Script d'exemple pour sauvegarder automatiquement les configs — À adapter*

---

```bash
#!/bin/bash
# ═══════════════════════════════════════════════════════════════════
# Script : backup_configs.sh
# Auteur : [Votre nom]
# Date   : 2024-11-15
# Rôle   : Sauvegarde automatique des configurations réseau
# Usage  : ./backup_configs.sh
# Cron   : 0 2 * * * /backup/scripts/backup_configs.sh (tous les jours 2h)
# ═══════════════════════════════════════════════════════════════════

# ─── Configuration ───
BACKUP_DIR="/backup/configs"
DATE=$(date +%Y%m%d_%H%M%S)
LOG_FILE="$BACKUP_DIR/backup.log"

# Liste des équipements à sauvegarder (IP + nom)
declare -A EQUIPEMENTS=(
    ["192.168.1.1"]="Switch-Core1"
    ["192.168.1.2"]="Switch-Distrib-RH"
    ["192.168.1.254"]="Routeur-Principal"
)

# Identifiants (À SÉCURISER — utiliser vault ou clé SSH)
USERNAME="admin"
PASSWORD="password"  # ⚠️ NE PAS stocker en clair en production

# ─── Fonction de sauvegarde ───
backup_device() {
    local IP=$1
    local NAME=$2
    local FILENAME="${BACKUP_DIR}/${NAME}/config_${NAME}_${DATE}.txt"
    
    echo "[$(date)] Sauvegarde de ${NAME} (${IP})..." | tee -a "$LOG_FILE"
    
    # Créer le dossier si inexistant
    mkdir -p "${BACKUP_DIR}/${NAME}"
    
    # Méthode 1 : TFTP (si le switch supporte)
    # (nécessite un serveur TFTP configuré)
    
    # Méthode 2 : SSH avec expect (exemple Cisco)
    expect << EOF
        spawn ssh ${USERNAME}@${IP}
        expect "Password:"
        send "${PASSWORD}\r"
        expect "#"
        send "terminal length 0\r"
        expect "#"
        send "show running-config\r"
        expect "#"
        send "exit\r"
        expect eof
EOF > "$FILENAME"
    
    # Vérifier que le fichier a été créé
    if [ -f "$FILENAME" ]; then
        echo "[$(date)] ✅ ${NAME} sauvegardé : ${FILENAME}" | tee -a "$LOG_FILE"
    else
        echo "[$(date)] ❌ Erreur sauvegarde ${NAME}" | tee -a "$LOG_FILE"
    fi
}

# ─── Boucle sur tous les équipements ───
echo "═══════════════════════════════════════════════════" | tee -a "$LOG_FILE"
echo "Début de sauvegarde automatique — ${DATE}" | tee -a "$LOG_FILE"
echo "═══════════════════════════════════════════════════" | tee -a "$LOG_FILE"

for IP in "${!EQUIPEMENTS[@]}"; do
    backup_device "$IP" "${EQUIPEMENTS[$IP]}"
done

# ─── Nettoyage (garder seulement les 30 dernières sauvegardes) ───
find "$BACKUP_DIR" -name "config_*.txt" -mtime +30 -delete

echo "[$(date)] Nettoyage effectué (configs > 30 jours supprimées)" | tee -a "$LOG_FILE"
echo "═══════════════════════════════════════════════════" | tee -a "$LOG_FILE"
```

---

*Pack S10 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.2, B2.3, B3.3 + Exploitation alternance*
*Configuration Management · Running/Startup · Versioning · Retour alternance · Clôture Bloc 1*
