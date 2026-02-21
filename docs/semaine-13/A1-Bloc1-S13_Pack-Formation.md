# Pack de Formation - Semaine 13 (S13) - BLOC 1
## 🔄 Gestion des Changements · Veille Technologique

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S13 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — treizième semaine |
| **Modalité** | Présentiel — salle de cours |
| **Prérequis** | S3 (ITIL fondamentaux), S10 (gestion des configurations), S12 (déploiement) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.2** | Exploiter des référentiels, normes et standards (ITIL) | Maîtrise |
| **B3.3** | Participer à la gestion et au suivi d'un projet | Maîtrise |
| **B3.4** | Mettre en œuvre une démarche de veille technologique | Acquisition |

> 📌 **S13 BLOC 1 remplit deux fonctions complémentaires :** (1) clôturer le triptyque ITIL (Incident Management S3-S4, Configuration Management S10, **Change Management S13**) en l'appliquant à la gestion de l'infrastructure ; (2) lancer la **veille technologique** comme pratique professionnelle continue, indispensable pour rester employable dans un secteur qui évolue constamment.

---

## Objectifs Pédagogiques

**Gestion des changements :**
- ✅ Définir un **changement** selon ITIL et le distinguer d'un incident ou d'une demande
- ✅ Expliquer le rôle du **CAB** (Change Advisory Board) et du Change Manager
- ✅ Décrire le **cycle de vie d'un changement** en 7 étapes
- ✅ Rédiger une **RFC** (Request For Change) complète et professionnelle
- ✅ Identifier les **3 types de changements** (standard, normal, urgent)
- ✅ Appliquer l'**analyse de risque** d'un changement

**Veille technologique :**
- ✅ Expliquer pourquoi la veille est **obligatoire** dans les métiers IT
- ✅ Identifier les **sources fiables** selon le domaine
- ✅ Configurer un **agrégateur de flux RSS** (Feedly)
- ✅ Mettre en place des **alertes Google** ciblées
- ✅ Distinguer information de qualité et clickbait technique
- ✅ Organiser sa veille en **routine hebdomadaire**

---

## ⭐ Spécificités Pédagogiques

### La Gestion des Changements : Entre Théorie et Réalité

Le Change Management ITIL est souvent perçu comme "bureaucratique" par les apprenants. Il faut contrer cette perception avec des exemples concrets de changements mal gérés.

**L'anecdote pédagogique qui fonctionne :**

> *"En 2017, un technicien de British Airways a redémarré un serveur de base de données sans suivre la procédure de changement. Il a coupé l'alimentation électrique au lieu de faire un shutdown propre. Résultat : corruption de la base de données, 75 000 passagers bloqués pendant 3 jours, perte estimée à 80 millions de livres."*

Cette histoire réelle fait immédiatement comprendre que le Change Management n'est pas de la paperasse — c'est de la gestion de risque.

### La Veille Technologique : Compétence de Survie

La veille technologique est la **seule compétence** qui garantit l'employabilité à long terme.

**Argument choc à utiliser en début de séance :**

> *"Dans les métiers IT, 50% de vos compétences techniques seront périmées dans 5 ans. Les technologies que vous apprenez aujourd'hui seront remplacées ou évoluées d'ici 2029. La seule compétence qui ne périme jamais, c'est votre capacité à apprendre en continu."*

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S12 | Feedback déploiements d'images — succès et difficultés |
| H+0:10 | 20 min | 🎯 Découverte | Activité "Le Serveur du Vendredi Soir" |
| H+0:30 | 50 min | 📖 Cours | Change Management : cycle de vie, types, CAB, RFC, risques |
| H+1:20 | **15 min** | ☕ **PAUSE** | — |
| H+1:35 | 25 min | ✍️ TP Part. 1 | Rédiger une RFC pour un changement infrastructure |
| H+2:00 | 40 min | 📖 Cours | Veille technologique : pourquoi, sources, outils, méthode |
| H+2:40 | 40 min | 🖥️ **TP Part. 2** | Configuration Feedly + alertes Google + sources par domaine |
| H+3:20 | 20 min | 📊 Partage | Tour de table — chacun partage 1 info de veille trouvée |
| H+3:40 | 20 min | 📅 Ritualisation | Planifier sa routine de veille hebdomadaire |

---

## Différenciation Pédagogique

### Profil Avancé
- **Change Management :** Rédiger une RFC complexe avec analyse de risque complète et plan de rollback détaillé
- **Veille :** Créer un dashboard de veille avec agrégation multi-sources
- **Extension :** Rédiger un article de synthèse technique

### Profil Débutant
- **Change Management :** Utiliser le modèle RFC pré-rempli
- **Veille :** Configuration guidée pas-à-pas avec 5 sources imposées
- **Support :** Liste de sources fiables fournie

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **Modèle RFC** | Annexe 1 — au format Word/PDF |
| **Liste sources veille** | Annexe 2 — par domaine technique |
| **Accès internet** | Pour configuration Feedly et alertes Google |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "Le Serveur du Vendredi Soir"

*Durée : 20 minutes — Collectif*

---

## Scénario (8 min)

L'enseignant lit l'histoire :

> **Contexte :** Vous êtes technicien dans une PME de 150 employés. Vendredi 17h, votre responsable vous dit : "On a un problème de lenteur sur le serveur de fichiers depuis 2 semaines. J'ai trouvé la cause : il faut augmenter la RAM de 16 Go à 32 Go. J'ai acheté les barrettes, elles arrivent demain matin. Peux-tu venir samedi matin les installer ?"
>
> **Samedi 9h — Votre intervention :**
> 1. Vous éteignez le serveur (150 employés n'ont plus accès aux fichiers)
> 2. Vous installez les 2 barrettes de 16 Go
> 3. Vous redémarrez le serveur
> 4. **Problème** : Le serveur ne démarre plus — écran noir, bip répétitif
>
> **Diagnostic :** Les barrettes RAM achetées sont incompatibles (DDR4 3200 MHz au lieu de DDR4 2666 MHz).
>
> **Actions tentées :**
> - Retirer les nouvelles barrettes, remettre les anciennes
> - Mais... vous avez cassé un clip de fixation lors de la manipulation
> - Le serveur redémarre mais n'a plus que 8 Go de RAM au lieu de 16 Go
>
> **Conséquences :**
> - Lundi 8h : Les employés se plaignent que le serveur est encore plus lent
> - La Direction découvre que le serveur a été modifié sans autorisation
> - Coût : 800 € de RAM jetée + 1 200 € technicien certifié + perte productivité

---

## Questions Guidées (9 min)

| **Question** | **Concept visé** |
|---|---|
| "Qu'aurait dû être fait AVANT d'acheter la RAM ?" | Vérifier la compatibilité matérielle |
| "Qu'aurait dû être fait AVANT d'intervenir samedi ?" | Planifier le changement, tester, informer |
| "Pourquoi intervenir le samedi était-il une mauvaise idée ?" | Pas de fenêtre de maintenance formalisée |
| "Si le changement avait été formalisé, qu'est-ce qui aurait été différent ?" | Validation, plan de rollback, communication |

## Conclusion (3 min)

```
   CHANGEMENT NON GÉRÉ = INCIDENT GARANTI

   Un changement bien géré :
   ① Est demandé formellement (RFC)
   ② Est évalué techniquement
   ③ Est approuvé par le CAB
   ④ Est planifié dans une fenêtre de maintenance
   ⑤ A un plan de rollback testé
   ⑥ Est communiqué aux utilisateurs
   ⑦ Est documenté après réalisation
```

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Gestion des Changements · Change Management ITIL"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 13*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.2** | Exploiter des référentiels ITIL (Change Management) |
| **B3.3** | Documenter et formaliser un changement |

---

## PARTIE I — Définition et Périmètre

### I.A. Qu'est-ce qu'un Changement ?

En ITIL 4, un **changement** est défini comme :

> *"L'ajout, la modification ou la suppression de tout élément susceptible d'avoir un effet sur les services IT."*

```
   EXEMPLES DE CHANGEMENTS
   ─────────────────────────────────────────────────────────────
   ✅ Ajout d'un serveur
   ✅ Mise à jour OS (Windows Server 2019 → 2022)
   ✅ Modification firewall (nouvelle règle)
   ✅ Ajout d'un VLAN
   ✅ Migration application vers le cloud

   ❌ PAS DES CHANGEMENTS
   ─────────────────────────────────────────────────────────────
   ❌ Réinitialisation mot de passe (demande standard)
   ❌ Remplacement disque défaillant à l'identique (incident)
   ❌ Redémarrage service bloqué (incident)
```

---

### I.B. Changement vs Incident vs Demande

| **Type** | **Déclencheur** | **Objectif** | **Validation** |
|---|---|---|---|
| **Incident** | Interruption non planifiée | Restaurer le service | Technicien N1/N2 |
| **Demande de service** | Besoin utilisateur standard | Fournir un service | Automatisé ou N1 |
| **Changement** | Évolution planifiée | Modifier l'infrastructure | **CAB** |

---

## PARTIE II — Le Cycle de Vie d'un Changement

### II.A. Les 7 Étapes

```
   ① DEMANDE (RFC — Request For Change)
   ──────────────────────────────────────────────────────────────
   Un demandeur crée une RFC documentant le changement souhaité

   ② ÉVALUATION TECHNIQUE
   ──────────────────────────────────────────────────────────────
   Le Change Manager évalue :
   • Faisabilité technique
   • Impact sur les services
   • Ressources nécessaires
   • Risques

   ③ APPROBATION ou REJET
   ──────────────────────────────────────────────────────────────
   Le CAB décide : approuvé / rejeté / reporté

   ④ PLANIFICATION
   ──────────────────────────────────────────────────────────────
   • Date de la fenêtre de maintenance
   • Runbook (ordre des tâches)
   • Plan de rollback
   • Communication utilisateurs

   ⑤ IMPLÉMENTATION
   ──────────────────────────────────────────────────────────────
   Exécution du changement
   Documentation en temps réel

   ⑥ VALIDATION
   ──────────────────────────────────────────────────────────────
   Vérifier que le changement a atteint ses objectifs

   ⑦ CLÔTURE
   ──────────────────────────────────────────────────────────────
   Mise à jour CMDB
   Archivage de la RFC
```

---

### II.B. Le CAB (Change Advisory Board)

Le **CAB** est un comité qui évalue et approuve les changements.

| **Membre** | **Rôle** | **Apporte** |
|---|---|---|
| **Change Manager** | Président du CAB | Vue d'ensemble |
| **Responsable DSI** | Valide l'alignement stratégique | Budget, priorités |
| **Technicien expert** | Évalue la faisabilité | Compétence technique |
| **Responsable métier** | Représente les utilisateurs | Impact métier |
| **Responsable sécurité** | Évalue les risques | Conformité |

---

### II.C. Analyse de Risque

Chaque changement est évalué selon **impact** × **probabilité** :

```
   MATRICE DE RISQUE
   ──────────────────────────────────────────────────────────────

                        PROBABILITÉ D'ÉCHEC
                     Faible    Moyenne    Élevée
                   ┌─────────┬──────────┬──────────┐
   IMPACT    Faible│  FAIBLE │  FAIBLE  │  MOYEN   │
             Moyen │  FAIBLE │  MOYEN   │  ÉLEVÉ   │
             Élevé │  MOYEN  │  ÉLEVÉ   │ CRITIQUE │
                   └─────────┴──────────┴──────────┘

   Actions selon le risque :
   FAIBLE    → Approbation simplifiée
   MOYEN     → Approbation CAB + plan rollback
   ÉLEVÉ     → CAB + tests préalables
   CRITIQUE  → Direction + simulation + équipe standby
```

---

## PARTIE III — Les 3 Types de Changements

### III.A. Changement Standard

Changement **pré-approuvé**, faible risque, procédure documentée.

```
   EXEMPLES
   ──────────────────────────────────────────────────────────────
   • Redémarrage mensuel planifié serveurs
   • Déploiement mises à jour sécurité Microsoft
   • Ajout utilisateur à Active Directory

   PROCESSUS
   ──────────────────────────────────────────────────────────────
   1. Procédure approuvée une fois par le CAB
   2. Exécution sans nouvelle approbation
   3. Documentation dans GLPI
```

---

### III.B. Changement Normal

Nécessite **évaluation et approbation** du CAB.

```
   EXEMPLES
   ──────────────────────────────────────────────────────────────
   • Ajout d'un nouveau serveur
   • Modification topologie réseau (VLAN)
   • Migration application vers nouvelle version

   PROCESSUS
   ──────────────────────────────────────────────────────────────
   1. RFC soumise
   2. Évaluation technique
   3. Présentation au CAB
   4. Décision → planification → implémentation
```

---

### III.C. Changement Urgent (Emergency)

Application **immédiate** pour incident critique.

```
   EXEMPLES
   ──────────────────────────────────────────────────────────────
   • Patch sécurité critique (CVE exploit actif)
   • Restauration serveur critique en panne
   • Blocage attaque en cours

   PROCESSUS ACCÉLÉRÉ
   ──────────────────────────────────────────────────────────────
   1. RFC urgente
   2. Approbation E-CAB (Emergency CAB)
   3. Implémentation immédiate
   4. Documentation a posteriori
```

---

## PARTIE IV — La RFC (Request For Change)

### IV.A. Structure Complète

```
╔══════════════════════════════════════════════════════════════════════╗
║                    REQUEST FOR CHANGE (RFC)                          ║
╠══════════════════════════════════════════════════════════════════════╣
║  N° RFC         : RFC-2024-___          Date : __________           ║
║  Type           : ☐ Standard  ☐ Normal  ☐ Urgent                   ║
╠══════════════════════════════════════════════════════════════════════╣
║  1. IDENTIFICATION                                                   ║
║  Demandeur      : _________________________________________         ║
║  Service        : _________________________________________         ║
║  Date souhaitée : _________________________________________         ║
╠══════════════════════════════════════════════════════════════════════╣
║  2. DESCRIPTION DU CHANGEMENT                                        ║
║  Titre          : _________________________________________         ║
║  Description détaillée :                                            ║
║  _______________________________________________________________     ║
╠══════════════════════════════════════════════════════════════════════╣
║  3. JUSTIFICATION                                                    ║
║  Pourquoi ce changement :                                           ║
║  _______________________________________________________________     ║
║  Bénéfices attendus :                                               ║
║  _______________________________________________________________     ║
╠══════════════════════════════════════════════════════════════════════╣
║  4. IMPACT ET RISQUE                                                 ║
║  Services impactés  : _____________________________________         ║
║  Nombre utilisateurs : _____________________________________        ║
║  Niveau de risque   : ☐ Faible ☐ Moyen ☐ Élevé ☐ Critique         ║
╠══════════════════════════════════════════════════════════════════════╣
║  5. PLAN D'IMPLÉMENTATION                                            ║
║  Fenêtre maintenance : Du __________ au __________                  ║
║  Étapes prévues :                                                    ║
║  1. _____________________________________________________________    ║
║  2. _____________________________________________________________    ║
╠══════════════════════════════════════════════════════════════════════╣
║  6. PLAN DE ROLLBACK                                                 ║
║  Procédure de retour arrière :                                      ║
║  _______________________________________________________________     ║
║  Rollback testé : ☐ Oui  ☐ Non                                     ║
╠══════════════════════════════════════════════════════════════════════╣
║  7. COMMUNICATION                                                    ║
║  Utilisateurs à informer : _____________________________________     ║
║  Moyen : ☐ Email  ☐ Intranet  ☐ Autre                              ║
╠══════════════════════════════════════════════════════════════════════╣
║  8. VALIDATION                                                       ║
║  Critères de succès :                                               ║
║  _______________________________________________________________     ║
╠══════════════════════════════════════════════════════════════════════╣
║  9. DÉCISION CAB                                                     ║
║  Date réunion   : _________________________________________         ║
║  Décision       : ☐ Approuvée  ☐ Rejetée  ☐ Reportée              ║
║  Signature      : _________________________________________         ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Change** | Modification d'un élément de l'infrastructure IT |
| **RFC** | Request For Change — demande formelle de changement |
| **CAB** | Change Advisory Board — comité décisionnel |
| **E-CAB** | Emergency CAB — CAB restreint pour urgences |
| **Change Manager** | Responsable du processus de gestion des changements |
| **Runbook** | Document détaillant l'implémentation étape par étape |
| **Rollback** | Retour à l'état antérieur en cas d'échec |
| **Baseline** | État de référence avant changement |

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Veille Technologique · Méthode · Outils"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 13*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B3.4** | Mettre en œuvre une démarche de veille technologique |

---

## PARTIE I — Pourquoi la Veille est Obligatoire

### I.A. L'Obsolescence des Compétences IT

```
   CYCLE DE VIE D'UNE TECHNOLOGIE IT
   ─────────────────────────────────────────────────────────────

   Année 0   → Technologie émergente
   Année 2   → Adoption croissante (vous l'apprenez)
   Année 5   → Technologie mature (vous la maîtrisez)
   Année 8   → Début du déclin
   Année 12  → Legacy (maintenance uniquement)
   Année 15+ → Obsolète

   Exemple : Windows Server 2008
   • 2008 : Lancement
   • 2012 : Très répandu
   • 2020 : Fin de support Microsoft
   • 2024 : Obsolète — vulnérabilités non patchées
```

**Le problème :** Si vous ne vous mettez pas à jour, vous devenez obsolète avec la technologie.

**La solution :** Veille technologique continue = apprentissage permanent.

---

### I.B. Les Chiffres

| **Secteur IT** | **Obsolescence** |
|---|---|
| Langages de programmation | 5-7 ans |
| OS serveur | 8-10 ans |
| Équipements réseau | 5-7 ans |
| Protocoles réseau | 10-15 ans |

> 💡 **50% de vos compétences seront périmées dans 5 ans** si vous ne faites pas de veille.

---

## PARTIE II — Les Sources de Veille

### II.A. Typologie des Sources

| **Type** | **Fiabilité** | **Mise à jour** | **Exemple** |
|---|---|---|---|
| **Documentation officielle** | ★★★★★ | Lente | Microsoft Docs, Cisco.com |
| **Blogs éditeurs** | ★★★★☆ | Rapide | Blog VMware, Red Hat |
| **Communautés** | ★★★☆☆ | Très rapide | Stack Overflow, Reddit |
| **Presse spécialisée** | ★★★★☆ | Quotidienne | ZDNet, ITespresso |

---

### II.B. Sources par Domaine

**SYSTÈMES WINDOWS :**
- Documentation : https://learn.microsoft.com
- Blog : https://techcommunity.microsoft.com
- Communauté : r/sysadmin (Reddit)

**SYSTÈMES LINUX :**
- Documentation : https://www.kernel.org
- Blogs : Red Hat Developer
- Communauté : r/linuxadmin

**RÉSEAUX :**
- Documentation : Cisco Learning Network
- Blogs : Packet Pushers
- Communauté : r/networking

**SÉCURITÉ :**
- Alertes : CERT-FR, ANSSI, CVE Database
- Blogs : Krebs on Security
- Communauté : r/netsec

---

### II.C. Détecter les Sources Fiables

```
   ✅ SOURCE FIABLE
   ──────────────────────────────────────────────────────────────
   ✅ Auteur identifié
   ✅ Sources citées
   ✅ Date récente (< 1 an)
   ✅ Pas de sensationnalisme
   ✅ Cohérence technique

   ❌ SOURCE NON FIABLE
   ──────────────────────────────────────────────────────────────
   ❌ Auteur anonyme
   ❌ Clickbait ("Ce hack CHOQUANT...")
   ❌ Aucune source
   ❌ Date ancienne
   ❌ Erreurs techniques
```

---

## PARTIE III — Outils de Veille

### III.A. Agrégateur RSS — Feedly

**RSS** permet de s'abonner aux mises à jour d'un site.

**Feedly** centralise tous vos flux :

```
   AVANTAGES
   ──────────────────────────────────────────────────────────────
   ✅ Gratuit
   ✅ Centralise 100+ sources
   ✅ Lecture rapide (5 min)
   ✅ Sauvegarde articles
   ✅ Catégorisation par thème

   CONFIGURATION
   ──────────────────────────────────────────────────────────────
   Catégories :
   ├── Systèmes (Windows, Linux, virtualisation)
   ├── Réseaux (Cisco, protocoles)
   ├── Sécurité (CVE, ANSSI)
   └── Cloud (Azure, AWS)

   20-30 sources recommandées
```

**Tutoriel Feedly :**
1. Créer compte sur https://feedly.com
2. Cliquer "+ Add Content"
3. Rechercher "Microsoft Tech Community" → Ajouter
4. Créer catégorie "Systèmes"

---

### III.B. Alertes Google

**Google Alerts** envoie email quand un mot-clé apparaît.

```
   ALERTES RECOMMANDÉES
   ──────────────────────────────────────────────────────────────
   1. "Windows Server 2022" + "nouvelle fonctionnalité"
   2. "CVE" + "critique" + "Windows"
   3. "Cisco" + "vulnérabilité"
   4. "ANSSI"
   5. [Ville] + "offre emploi" + "technicien réseau"

   Fréquence : digest quotidien
```

**Créer une alerte :**
1. https://www.google.com/alerts
2. Saisir terme
3. Fréquence : quotidien
4. Créer l'alerte

---

### III.C. Réseaux Sociaux Techniques

| **Plateforme** | **Usage** | **À suivre** |
|---|---|---|
| **LinkedIn** | Suivre experts et entreprises | Microsoft, Cisco |
| **Twitter/X** | Alertes temps réel | #infosec, #sysadmin |
| **Reddit** | Discussions techniques | r/sysadmin, r/networking |
| **YouTube** | Tutoriels | NetworkChuck |

---

## PARTIE IV — Méthode de Veille

### IV.A. Routine 30 Minutes / Semaine

```
   LUNDI MATIN — 30 MINUTES
   ──────────────────────────────────────────────────────────────

   10 MIN — Feedly (scan titres)
   10 MIN — Lecture approfondie (2-3 articles)
   5 MIN  — Alertes Google
   5 MIN  — Réseaux sociaux

   TOTAL : 30 min → Compétence à jour
```

---

### IV.B. Documentation

```
   CARNET DE VEILLE
   ──────────────────────────────────────────────────────────────
   Outil : OneNote, Notion, Markdown

   Structure :
   ├── 2024
   │   ├── Novembre
   │   │   ├── Semaine 46
   │   │   │   ├── Windows Server 2025 Preview
   │   │   │   ├── CVE-2024-XXXXX critique
   │   │   ├── Semaine 47

   Par entrée :
   ├── Titre
   ├── Date
   ├── Source (URL)
   ├── Résumé
   ├── Pertinence
   └── Action
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Veille technologique** | Surveillance continue des évolutions techniques |
| **RSS** | Really Simple Syndication — format de flux web |
| **Agrégateur** | Outil centralisant plusieurs flux RSS |
| **CVE** | Common Vulnerabilities and Exposures |
| **CERT** | Computer Emergency Response Team |
| **ANSSI** | Agence Nationale Sécurité SI (France) |
| **Clickbait** | Contenu sensationnaliste peu fiable |

---

---

# ✍️ TP PARTIE 1 — RÉDIGER UNE RFC

*Durée : 25 minutes — Individuel*

---

## Scénario

**Contexte :** SimIO SARL (80 employés) souhaite ajouter un VLAN pour séparer le réseau Marketing du réseau général.

**Changement demandé :**
- Créer VLAN 20 (Marketing)
- Configurer switch principal (Cisco Catalyst 2960)
- Attribuer ports 10-15 au VLAN 20
- Configurer DHCP pour VLAN 20 (plage 192.168.20.100-200)

**Mission :** Rédiger la RFC complète.

---

## Consignes

Remplir toutes les sections :
1. Identification
2. Description
3. Justification
4. Impact et risque
5. Plan d'implémentation (5-7 étapes)
6. Plan de rollback
7. Communication
8. Validation

---

## Critères d'Évaluation

| **Critère** | **Points** |
|---|---|
| RFC complète | /5 |
| Description technique précise | /3 |
| Plan de rollback réaliste | /3 |
| Analyse de risque pertinente | /3 |
| Communication prévue | /2 |
| **TOTAL** | **/16** |

---

---

# 🖥️ TP PARTIE 2 — CONFIGURATION VEILLE

*Durée : 40 minutes — Individuel*

---

## Objectif

Configurer votre écosystème de veille personnel.

---

## ÉTAPE 1 — Créer Compte Feedly (5 min)

1. Aller sur https://feedly.com
2. Créer un compte

---

## ÉTAPE 2 — Ajouter 10 Sources RSS (15 min)

Ajouter **2 sources par catégorie** :

| **Catégorie** | **Sources recommandées** |
|---|---|
| **Systèmes** | Microsoft Tech Community, Red Hat Blog |
| **Réseaux** | Cisco Blog, Packet Pushers |
| **Sécurité** | ANSSI, Krebs on Security |
| **Cloud** | Azure Blog, AWS News Blog |
| **Général** | ZDNet France, Les Numériques Pro |

**Procédure :**
1. "+ Add Content"
2. Rechercher "Microsoft Tech Community"
3. "Follow"
4. Créer catégorie "Systèmes"
5. Répéter pour les 9 autres

---

## ÉTAPE 3 — Configurer 5 Alertes Google (10 min)

Créer sur https://www.google.com/alerts :

| **N°** | **Mots-clés** | **Fréquence** |
|---|---|---|
| 1 | "Windows Server" + "nouvelle version" | Quotidien |
| 2 | "CVE" + "critique" + "Microsoft" | Quotidien |
| 3 | "ANSSI" + "bulletin" | Quotidien |
| 4 | [Ville] + "offre emploi technicien" | Hebdomadaire |
| 5 | "Cisco" + "fin de support" | Hebdomadaire |

---

## ÉTAPE 4 — Première Lecture (10 min)

1. Parcourir les 20-30 premiers articles Feedly
2. Sélectionner **3 articles pertinents**
3. Noter :

| **Article** | **Source** | **Titre** | **Pertinence** |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

## ÉTAPE 5 — Planifier Routine (Livrable)

```
MA ROUTINE DE VEILLE HEBDOMADAIRE
─────────────────────────────────────────────────────────────

Jour : ☐ Lundi  ☐ Mercredi  ☐ Vendredi
Heure : __________ (ex : 8h-8h30)

Outils :
☐ Feedly
☐ Alertes Google
☐ LinkedIn
☐ Reddit

Documentation :
☐ OneNote
☐ Markdown
☐ Google Keep

Engagement : ______ minutes/semaine
```

---

---

# 📄 ANNEXE 1 — MODÈLE RFC VIERGE

*(Modèle complet fourni en section IV.A du cours)*

---

# 📄 ANNEXE 2 — SOURCES FIABLES PAR DOMAINE

---

## SYSTÈMES WINDOWS

**Documentation officielle :**
- https://learn.microsoft.com/windows-server
- https://learn.microsoft.com/powershell

**Blogs :**
- https://techcommunity.microsoft.com
- https://www.petri.com

**Communautés :**
- r/sysadmin (Reddit)
- https://community.spiceworks.com

---

## SYSTÈMES LINUX

**Documentation :**
- https://www.kernel.org
- https://wiki.archlinux.org
- https://ubuntu.com/blog

**Blogs :**
- https://developers.redhat.com
- https://www.linux.com

**Communautés :**
- r/linuxadmin
- https://unix.stackexchange.com

---

## RÉSEAUX

**Documentation :**
- https://learningnetwork.cisco.com
- https://www.juniper.net/documentation

**Blogs :**
- https://packetpushers.net
- https://www.networkworld.com

**Communautés :**
- r/networking
- https://community.cisco.com

---

## SÉCURITÉ

**Alertes officielles :**
- https://www.cert.ssi.gouv.fr (CERT-FR)
- https://www.ssi.gouv.fr (ANSSI)
- https://cve.mitre.org

**Blogs :**
- https://krebsonsecurity.com
- https://www.schneier.com

**Communautés :**
- r/netsec
- r/cybersecurity

---

## CLOUD

**Documentation :**
- https://learn.microsoft.com/azure
- https://aws.amazon.com/blogs
- https://cloud.google.com/blog

**Blogs :**
- https://cloudacademy.com/blog
- https://acloudguru.com/blog

---

*Pack S13 BLOC 1 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.2, B3.3, B3.4*
*Change Management · RFC · CAB · Veille technologique · Feedly · Sources fiables*
