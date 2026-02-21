# Pack de Formation - Semaine 11 (S11) - BLOC 2
## 📦 Gestion des Actifs Logiciels · Licences · Conformité · Documentation Procédures

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S11 — Année 1 |
| **Bloc** | Bloc 2 — Administration des systèmes et des réseaux |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — onzième semaine |
| **Modalité** | Présentiel — salle de cours/TP |
| **Prérequis** | S2 (inventaire matériel), S5 (OCS Inventory), S6 (GLPI) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.1** | Recenser et identifier les ressources numériques | Maîtrise |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc | Maîtrise |
| **B1.2** | Exploiter des référentiels, normes et standards | Maîtrise |
| **B2.1** | Installer et configurer un service réseau pour une TPE ou une PME | Acquisition |
| **B3.3** | Participer à la gestion et au suivi d'un projet (documentation) | Maîtrise |

> 📌 **Note pédagogique :** S11 constitue une **semaine de transition** entre le Bloc 1 (support) et le Bloc 2 technique (infrastructure). Elle approfondit un angle crucial du Bloc 1 (la gestion des actifs logiciels et licences) tout en introduisant une compétence transversale du Bloc 2 (la rédaction de procédures techniques). Cette double casquette justifie son positionnement en S11 plutôt qu'en fin de Bloc 1.

---

## Objectifs Pédagogiques

**Gestion des actifs logiciels :**
- ✅ Distinguer les **7 types de licences** logicielles (OEM, Volume, SaaS, Open Source, Freeware, Shareware, Retail)
- ✅ Expliquer les **risques juridiques** et financiers du non-respect des licences
- ✅ Réaliser un **audit de conformité** logicielle avec GLPI/OCS
- ✅ Calculer le **coût total de possession** (TCO) d'un logiciel
- ✅ Gérer les **dates d'expiration** et renouvellements de licences
- ✅ Connaître les organismes de contrôle (BSA, SACEM, éditeurs)

**Documentation technique :**
- ✅ Rédiger une **procédure d'installation** structurée et exploitable
- ✅ Utiliser un **modèle standardisé** de documentation technique
- ✅ Distinguer une **procédure** (comment faire) d'un **guide utilisateur** (comment utiliser)
- ✅ Intégrer les **captures d'écran** de façon professionnelle
- ✅ Versionner et maintenir à jour la documentation technique

---

## ⭐ Spécificités Pédagogiques

### La Gestion des Licences : Un Sujet Sous-Estimé

La gestion des licences logicielles est souvent perçue comme "administrative" et donc peu valorisante par les apprenants techniques. Pourtant :

**Argument 1 — Le risque financier est massif.** Une PME de 80 personnes en sous-licensing Microsoft peut recevoir une amende de 50 000 à 200 000 € lors d'un audit BSA. C'est une responsabilité directe de la DSI.

**Argument 2 — C'est une compétence différenciante.** Un technicien qui maîtrise la gestion des licences (types, calcul TCO, audit) est immédiatement plus employable qu'un technicien qui ne fait que de la technique pure.

**Argument 3 — L'E5 l'évalue directement.** Le jury peut demander "comment gérez-vous les licences dans votre entreprise ?" — un apprenant qui répond "je ne sais pas, c'est l'admin qui s'en occupe" perd des points.

### La Documentation : Compétence Transversale Critique

La capacité à **documenter proprement** est la compétence la plus sous-estimée du BTS SIO. Un apprenant qui documente mal :
- Ne peut pas prouver son travail en E5
- Ne peut pas transmettre ses connaissances en entreprise
- Ne peut pas reprendre son propre travail 6 mois plus tard

S11 introduit la **procédure technique** comme format de documentation formel. Ce format sera réutilisé dans tous les projets du Bloc 2 (installation serveur S17, configuration réseau S13, scripting S16...).

### Lien avec S2 et S5

S11 est la **suite logique** de S2 (inventaire matériel) et S5 (OCS Inventory) : après avoir inventorié le matériel, on inventorie le logiciel. Après avoir compté les licences, on vérifie qu'on est conforme.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S10 | Feedback SPS #2 — retours positifs collectifs |
| H+0:10 | 20 min | 🎯 Découverte | Activité "L'Audit qui Tue" |
| H+0:30 | 50 min | 📖 Cours | Gestion des actifs logiciels : licences, conformité, risques, TCO |
| H+1:20 | **15 min** | ☕ **PAUSE** | — |
| H+1:35 | 30 min | 🖥️ TP Part. 1 | Audit de conformité GLPI — identifier les logiciels non conformes |
| H+2:05 | 35 min | 📖 Cours | Documentation technique : procédure, modèle, versioning |
| H+2:40 | 50 min | ✍️ **TP Part. 2** | Rédiger une procédure d'installation serveur (modèle fourni) |
| H+3:30 | 20 min | ✅ Peer review | Échange et validation croisée des procédures |
| H+3:50 | 10 min | 📁 Portfolio | Ces livrables dans le dossier E5 (procédure = preuve B2.1) |

---

## Différenciation Pédagogique

### Profil Avancé
- **Audit :** Réaliser un audit complet de l'instance GLPI/OCS avec rapport chiffré (nombre de licences manquantes, coût estimé de mise en conformité)
- **Documentation :** Rédiger une procédure complexe (installation serveur AD avec DNS + DHCP) avec schémas d'architecture
- **Extension :** Créer un **template de procédure réutilisable** pour l'établissement (avec logo, numérotation auto, table des versions)

### Profil Débutant
- **Audit :** Utiliser la grille de vérification fournie en Annexe 1 — cocher les conformités OK/KO
- **Documentation :** Compléter une procédure pré-rédigée (remplir les captures manquantes et les commandes)
- **Support :** Travailler en binôme avec un profil avancé qui relit et valide

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **Accès GLPI/OCS** | Pour consultation de l'inventaire logiciel |
| **Captures d'écran exemple** | Procédure installation Apache (fournie) |
| **Modèle de procédure** | Annexe 2 — au format Word/LibreOffice |
| **VM ou serveur de test** | Pour que les apprenants puissent capturer leurs propres étapes |

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "L'Audit qui Tue"

*Durée : 20 minutes — Collectif*

---

## Mise en Situation (7 min)

L'enseignant lit le scénario :

---

> **Contexte :** Vous êtes technicien dans une PME de 120 employés. Un lundi matin, le directeur reçoit une lettre recommandée de la **BSA** (Business Software Alliance) annonçant un **audit surprise de conformité logicielle** dans 15 jours.
>
> **Qu'est-ce que la BSA ?** Organisme qui représente les éditeurs de logiciels (Microsoft, Adobe, Autodesk...) et mène des audits dans les entreprises pour vérifier qu'elles utilisent uniquement des logiciels légalement acquis.
>
> **Le problème :** Personne dans la DSI n'a de vision claire du nombre de licences possédées vs installées. L'inventaire logiciel existe dans GLPI mais n'a jamais été analysé en détail.
>
> **La mission :** Vous avez 2 semaines pour :
> 1. Compter les logiciels installés sur tous les postes
> 2. Retrouver toutes les factures de licences
> 3. Identifier les écarts
> 4. Acheter en urgence les licences manquantes
> 5. Désinstaller les logiciels non autorisés
>
> **Le résultat de l'audit :**
> - Windows : OK — licences OEM avec chaque PC
> - Microsoft Office : **12 installations en trop** (90 postes équipés, 78 licences achetées)
> - Adobe Photoshop : **7 installations sans licence** (usage personnel non autorisé)
> - AutoCAD : OK — 5 licences pour 5 postes
> - WinRAR : **85 installations en version d'évaluation expirée** depuis 2 ans
>
> **Facture de mise en conformité :**
> - 12 × Office Standard 2021 : 12 × 300 € = **3 600 €**
> - 7 × Adobe Creative Cloud Pro : 7 × 60 €/mois × 12 mois = **5 040 €/an**
> - 85 × WinRAR : 85 × 29 € = **2 465 €**
> - **Total : 11 105 €** à dépenser en urgence
>
> **Amende BSA évitée de justesse :** Les auditeurs ne donnent pas d'amende cette fois car l'entreprise régularise immédiatement. Mais ils préviennent qu'un second audit aura lieu dans 6 mois.

---

## Questions Guidées (10 min)

| **Question** | **Concept visé** |
|---|---|
| "Pourquoi l'entreprise n'a-t-elle pas été sanctionnée cette fois ?" | Bonne foi + régularisation immédiate |
| "Quelle aurait été l'amende si refus de régulariser ?" | Peut atteindre 150 000 € + dommages et intérêts selon les éditeurs |
| "Est-ce que GLPI/OCS aurait pu éviter cette situation ?" | Oui — inventaire logiciel automatique + alertes |
| "WinRAR en version d'évaluation expirée — est-ce légal ?" | Non — la version d'évaluation est limitée à 40 jours |
| "Comment éviter ce problème de façon systématique ?" | Audit mensuel automatique, politique d'installation stricte |

## Conclusion (3 min)

L'enseignant écrit au tableau :

```
   LES 3 PILIERS DE LA GESTION DES LICENCES
   ─────────────────────────────────────────────────────────────
   ① INVENTAIRE — Savoir ce qui est installé (OCS, GLPI)
   ② CONFORMITÉ — Vérifier qu'on a le droit de l'utiliser (licences)
   ③ DOCUMENTATION — Prouver qu'on est conforme (factures, contrats)

   Sans ça → risque financier + risque juridique + risque réputationnel
```

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Gestion des Actifs Logiciels · Licences · Conformité"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 11*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.1** | Recenser et identifier les ressources numériques (logiciels) |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc (licences) |
| **B1.2** | Exploiter des référentiels (conformité légale) |

---

## PARTIE I — Les Types de Licences Logicielles

### I.A. Les 7 Familles de Licences

| **Type** | **Définition** | **Droit d'usage** | **Exemple** | **Coût** |
|---|---|---|---|---|
| **OEM** | Original Equipment Manufacturer | Liée au matériel d'origine, non transférable | Windows préinstallé sur un PC Dell | Inclus dans le prix du PC |
| **Retail / Box** | Achat en magasin | Utilisable sur un seul poste à la fois, transférable entre machines | Office 2021 acheté en grande surface | 100-400 € |
| **Volume / VL** | Licence en volume pour organisations | Nombre défini de postes, gestion centralisée | Microsoft Open License (50 licences Office) | 150-250 €/licence |
| **SaaS** | Software as a Service | Abonnement mensuel/annuel, accès cloud | Microsoft 365, Adobe Creative Cloud | 10-60 €/mois/utilisateur |
| **Open Source** | Logiciel libre | Usage, modification, distribution libres | Linux, LibreOffice, GIMP, Firefox | Gratuit |
| **Freeware** | Gratuit mais propriétaire | Usage gratuit, code fermé | 7-Zip, VLC, Acrobat Reader | Gratuit |
| **Shareware** | Essai gratuit limité | Gratuit en version bridée ou limitée dans le temps | WinRAR (40 jours d'essai), WinZip | Payant après essai |

---

### I.B. Les Pièges Classiques

```
   PIÈGE N°1 — "C'est gratuit pour un usage personnel"
   ──────────────────────────────────────────────────────────────
   Beaucoup de logiciels sont gratuits en usage personnel mais
   PAYANTS en usage commercial.

   Exemples :
   • WinRAR : gratuit en usage perso, 29 € en entreprise
   • Certaines polices de caractères : gratuites en perso, licence
     commerciale nécessaire pour les logos d'entreprise
   • Slack : gratuit jusqu'à 10 000 messages, payant au-delà

   ⚠️ Installer un logiciel "gratuit perso" sur un PC d'entreprise
      = violation de licence


   PIÈGE N°2 — "On a acheté X licences il y a 5 ans"
   ──────────────────────────────────────────────────────────────
   Certaines licences sont PERPÉTUELLES (Office 2019 standalone),
   d'autres sont TEMPORAIRES (Office 365 — 1 an).

   • Licence perpétuelle : payée une fois, utilisable indéfiniment
     (mais les mises à jour majeures sont payantes)
   • Licence temporaire (subscription) : payée chaque mois/an,
     arrêt du paiement = arrêt du logiciel

   ⚠️ Ne pas renouveler une licence temporaire = utilisation illégale


   PIÈGE N°3 — "On a désinstallé, donc on peut réutiliser la licence"
   ──────────────────────────────────────────────────────────────
   Certaines licences sont NOMINATIVES (liées à un utilisateur),
   d'autres sont FLOTTANTES (pool partagé).

   • Licence nominative (Named User) : achetée pour "Alice Martin",
     utilisable uniquement par elle — même si désinstallée sur son
     ancien PC, elle reste "sa" licence
   • Licence flottante (Concurrent) : pool de N licences utilisables
     par N utilisateurs simultanés maximum

   ⚠️ Réattribuer une licence nominative sans en informer l'éditeur
      peut violer le contrat
```

---

## PARTIE II — La Conformité Logicielle

### II.A. Qu'est-ce que la Conformité ?

Une organisation est **conforme** si :
1. Tous les logiciels installés sont couverts par une licence valide
2. Le nombre d'installations ne dépasse pas le nombre de licences
3. Les preuves d'achat sont conservées et accessibles
4. Les renouvellements sont effectués avant expiration

---

### II.B. Les Risques du Non-Respect

| **Risque** | **Conséquence** | **Exemple chiffré** |
|---|---|---|
| **Amende BSA** | L'organisme BSA peut exiger des dommages-intérêts | 150 000 € pour une PME de 80 personnes (jurisprudence française) |
| **Poursuite éditeur** | Microsoft, Adobe, Autodesk peuvent attaquer en justice | Oracle a obtenu 1,3 milliard $ contre Google (cas extrême) |
| **Réputation** | Publication de l'amende, impact image | "Entreprise X condamnée pour piratage logiciel" |
| **Blocage logiciel** | Certains éditeurs peuvent désactiver les licences à distance | Adobe CC cesse de fonctionner si l'abonnement expire |
| **Audit informatique bloquant** | Lors d'un audit ISO 27001 ou NIS2, la non-conformité bloque la certification | Perte d'un marché public exigeant ISO 27001 |

---

### II.C. Les Organismes de Contrôle

| **Organisme** | **Rôle** | **Secteur** |
|---|---|---|
| **BSA** | Business Software Alliance — représente les grands éditeurs (Microsoft, Adobe, Autodesk...) | Logiciels professionnels |
| **SACEM** | Société des Auteurs, Compositeurs et Éditeurs de Musique | Musique (si diffusée en entreprise) |
| **APP** | Alliance for Intellectual Property — UK, mais influence européenne | Propriété intellectuelle globale |
| **Éditeurs directs** | Microsoft, Adobe, Oracle mènent leurs propres audits | Leurs logiciels uniquement |

> 📌 **Comment sont déclenchés les audits ?** Plusieurs déclencheurs :
> - Dénonciation anonyme (ancien employé mécontent, concurrent...)
> - Campagne d'audit aléatoire de la BSA dans un secteur
> - Détection automatique par les éditeurs (certains logiciels "appellent à la maison")
> - Lors d'une fusion/acquisition (due diligence)

---

## PARTIE III — L'Audit de Conformité

### III.A. Méthodologie d'Audit Interne

Un audit de conformité se déroule en 6 étapes :

```
   ① INVENTAIRE LOGICIEL
   ──────────────────────────────────────────────────────────────
   Lister tous les logiciels installés sur tous les postes
   Outils : OCS Inventory, GLPI, Lansweeper, SCCM
   Export : Fichier CSV avec Poste / Logiciel / Version / Éditeur

   ② INVENTAIRE DES LICENCES
   ──────────────────────────────────────────────────────────────
   Lister toutes les licences achetées
   Sources : factures, contrats, emails de confirmation, clés de licence
   Base : Fichier Excel ou module Licences de GLPI

   ③ RAPPROCHEMENT (RECONCILIATION)
   ──────────────────────────────────────────────────────────────
   Comparer les deux listes :
   Installations > Licences → SOUS-LICENSING (illégal)
   Installations < Licences → SUR-LICENSING (gaspillage)

   ④ IDENTIFICATION DES ÉCARTS
   ──────────────────────────────────────────────────────────────
   Produire 3 listes :
   • Logiciels sans licence (à acheter ou désinstaller)
   • Licences inutilisées (renégocier ou réaffecter)
   • Licences expirées (renouveler ou migrer)

   ⑤ PLAN DE REMÉDIATION
   ──────────────────────────────────────────────────────────────
   Décider pour chaque écart :
   • Acheter les licences manquantes
   • Désinstaller les logiciels non autorisés
   • Migrer vers des alternatives (LibreOffice au lieu d'Office)
   • Renégocier avec l'éditeur (volume discount)

   ⑥ SUIVI ET REPORTING
   ──────────────────────────────────────────────────────────────
   Produire un rapport de conformité mensuel/trimestriel
   Dashboard GLPI : % de conformité, nombre d'alertes, coût
```

---

### III.B. Formule de Conformité

```
   Taux de conformité = (Licences valides / Logiciels installés) × 100

   Exemple :
   Logiciels installés : 450 (tous postes confondus)
   Licences valides    : 380
   Conformité          : (380 / 450) × 100 = 84,4%

   Seuil acceptable : ≥ 98% (marge de tolérance pour renouvellements en cours)
   Seuil critique   : < 90% → audit externe probable
```

---

## PARTIE IV — Le Coût Total de Possession (TCO)

### IV.A. Définition

Le **TCO** (Total Cost of Ownership) est le coût réel d'un logiciel sur toute sa durée de vie, incluant :

| **Composante** | **Exemple** |
|---|---|
| **Achat initial** | 300 € pour Office 2021 |
| **Support et maintenance** | 50 €/an (updates, hotline) |
| **Formation** | 200 € pour former 1 utilisateur |
| **Coût de déploiement** | 2h technicien × 50 €/h = 100 € |
| **Coût de migration** | Passage de Office 2016 → 2021 : 4h × 50 € = 200 € |
| **Coût d'opportunité** | Temps perdu si le logiciel est lent ou inadapté |

```
   TCO sur 5 ans — Office 2021 (1 utilisateur)

   Achat initial          : 300 €
   Support (5 × 50 €)     : 250 €
   Formation              : 200 €
   Déploiement            : 100 €
   Migrations (× 2)       : 400 €
   ─────────────────────────────
   TCO 5 ans              : 1 250 €
   Coût annualisé         : 250 €/an
```

**Comparaison SaaS vs Perpétuel :**

| **Critère** | **Office 2021 (perpétuel)** | **Microsoft 365 (SaaS)** |
|---|---|---|
| Achat initial | 300 € | 0 € |
| Coût annuel | 50 € (support) | 120 €/an (abonnement) |
| Mises à jour majeures | Payantes (nouvelle version) | Incluses |
| TCO 5 ans | 1 250 € | 600 € (abonnement) + 300 € (support/formation) = 900 € |
| **Bilan** | Plus cher sur 5 ans | Moins cher si mises à jour fréquentes |

> 💡 **Enseignement :** Le SaaS est souvent plus économique sur le long terme si on valorise les mises à jour continues et la flexibilité (ajout/retrait d'utilisateurs). Le perpétuel est préférable si l'organisation veut contrôler les montées de version.

---

## PARTIE V — Gestion dans GLPI

### V.A. Module Licences GLPI

GLPI dispose d'un module dédié à la gestion des licences :

```
   GLPI → Parc → Licences
   ──────────────────────────────────────────────────────────────

   Informations à renseigner pour chaque licence :
   ├── Nom (ex : Microsoft Office Standard 2021)
   ├── Éditeur (Microsoft Corporation)
   ├── Type (OEM, Volume, SaaS, Open Source...)
   ├── Nombre acheté (ex : 80 licences)
   ├── Date d'achat
   ├── Date d'expiration (si temporaire)
   ├── Coût total
   ├── Numéro de commande / facture
   ├── Clé de licence (si applicable)
   └── Association avec les logiciels inventoriés (lien vers le logiciel)

   Vue consolidée :
   • Licences expirées dans les 30 jours (alerte)
   • Licences surexploitées (plus d'installations que de licences)
   • Licences sous-utilisées (licences achetées mais non déployées)
```

---

### V.B. Alertes Automatiques

Configurer GLPI pour envoyer des alertes :

| **Alerte** | **Condition** | **Action recommandée** |
|---|---|---|
| Expiration proche | Licence expire dans < 30 jours | Prévoir renouvellement |
| Sur-utilisation | Installations > Licences | Acheter ou désinstaller |
| Sous-utilisation | Licences inutilisées depuis 90 jours | Réattribuer ou annuler |
| Licence non attribuée | Achetée mais jamais déployée | Déployer ou annuler |

---

## VI. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Actif logiciel** | Tout logiciel déployé dans l'organisation, qu'il soit licencié ou non |
| **Conformité** | État où tous les logiciels installés sont couverts par des licences valides |
| **BSA** | Business Software Alliance — organisme représentant les éditeurs pour les audits |
| **Sous-licensing** | Situation où le nombre d'installations dépasse le nombre de licences (illégal) |
| **Sur-licensing** | Situation où des licences payées ne sont pas utilisées (gaspillage) |
| **TCO** | Total Cost of Ownership — coût total d'un logiciel sur sa durée de vie |
| **Named User** | Licence nominative attribuée à une personne précise |
| **Concurrent License** | Licence flottante utilisable par N utilisateurs simultanés (pool) |
| **Volume License** | Licence achetée en volume pour une organisation (tarif dégressif) |
| **Perpetual License** | Licence perpétuelle payée une fois, utilisable indéfiniment |
| **Subscription** | Licence par abonnement payée mensuellement ou annuellement |
| **Audit trail** | Trace documentaire prouvant la légalité des licences (factures, contrats) |
| **Réconciliation** | Processus de rapprochement entre licences achetées et logiciels installés |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Je distingue les 7 types de licences logicielles
- [ ] J'explique le risque juridique et financier du non-respect des licences
- [ ] Je calcule un taux de conformité à partir d'un inventaire
- [ ] Je calcule le TCO d'un logiciel sur 5 ans
- [ ] Je configure une alerte d'expiration dans GLPI
- [ ] Je produis un rapport de conformité à partir d'un inventaire OCS/GLPI
- [ ] J'explique la différence entre Named User et Concurrent License

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Documentation Technique · Rédaction de Procédures"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 11*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.1** | Installer et configurer un service (documentation) |
| **B3.3** | Documenter professionnellement |

---

## PARTIE I — Procédure vs Guide vs DAT

### I.A. Les 3 Types de Documentation Technique

| **Type** | **Public** | **Objectif** | **Contenu** | **Exemple** |
|---|---|---|---|---|
| **Procédure** | Technicien qui exécute | Reproduire une tâche précise | Étapes numérotées, commandes, captures | "Procédure d'installation Apache sur Ubuntu 22.04" |
| **Guide utilisateur** | Utilisateur final | Utiliser un service | Fonctionnalités, astuces, FAQ | "Guide d'utilisation de la messagerie" |
| **DAT** | DSI / auditeur | Comprendre l'architecture | Schémas, choix techniques, justifications | "Dossier d'Architecture Technique SimIO" |

> 📌 **Erreur courante :** Mélanger les trois dans un seul document. Une procédure d'installation qui explique aussi l'architecture et comment utiliser le service devient illisible.

---

### I.B. Caractéristiques d'une Bonne Procédure

| **Critère** | **Bonne pratique** | **Mauvaise pratique** |
|---|---|---|
| **Reproductibilité** | N'importe quel technicien peut suivre sans aide | "Configurer le serveur comme d'habitude" |
| **Précision** | Commandes exactes, chemins complets | "Modifier le fichier de config" (lequel ?) |
| **Captures d'écran** | À chaque étape clé, annotées | Captures floues ou sans légende |
| **Numérotation** | Étapes numérotées hiérarchiquement (1, 1.1, 1.2, 2, 2.1...) | Puces désordonnées |
| **Prérequis** | Clairement listés en début | Découverts en cours de route |
| **Résultat attendu** | "À la fin, vous devez voir X" | Pas de validation |

---

## PARTIE II — Structure d'une Procédure Technique

### II.A. Les 10 Sections Obligatoires

```
╔══════════════════════════════════════════════════════════════════════╗
║              STRUCTURE STANDARD D'UNE PROCÉDURE                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  ① EN-TÊTE                                                           ║
║     Titre, version, date, auteur, statut (brouillon/validé)          ║
║                                                                      ║
║  ② OBJECTIF                                                          ║
║     Ce que cette procédure permet d'accomplir (1-2 phrases)          ║
║                                                                      ║
║  ③ PÉRIMÈTRE                                                         ║
║     Ce qui est couvert / ce qui ne l'est pas                         ║
║                                                                      ║
║  ④ PRÉREQUIS                                                         ║
║     Matériel, logiciels, accès, compétences nécessaires              ║
║                                                                      ║
║  ⑤ DURÉE ESTIMÉE                                                     ║
║     Temps nécessaire pour exécuter la procédure                      ║
║                                                                      ║
║  ⑥ ÉTAPES D'INSTALLATION                                             ║
║     Numérotation hiérarchique, commandes, captures                   ║
║                                                                      ║
║  ⑦ VÉRIFICATION                                                      ║
║     Tests à effectuer pour valider que tout fonctionne               ║
║                                                                      ║
║  ⑧ DÉPANNAGE (TROUBLESHOOTING)                                       ║
║     Erreurs courantes + résolutions                                  ║
║                                                                      ║
║  ⑨ RÉFÉRENCES                                                        ║
║     Documentation officielle, liens utiles, tickets liés             ║
║                                                                      ║
║  ⑩ HISTORIQUE DES VERSIONS                                           ║
║     v1.0 — Date — Auteur — Modification                              ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

### II.B. Exemple d'En-Tête Professionnel

```
═══════════════════════════════════════════════════════════════════════
                          PROCÉDURE TECHNIQUE
                   Installation Serveur Apache 2.4
                          Ubuntu Server 22.04 LTS
═══════════════════════════════════════════════════════════════════════
Version      : 1.2
Date         : 2024-11-20
Auteur       : [Votre nom]
Réviseur     : [Nom du superviseur]
Statut       : ☑ Validé  ☐ Brouillon  ☐ Obsolète
Référence    : PROC-SRV-APACHE-001
═══════════════════════════════════════════════════════════════════════
```

---

## PARTIE III — Les Captures d'Écran Professionnelles

### III.A. Quand Capturer ?

| **Situation** | **Capturer ?** | **Raison** |
|---|---|---|
| Commande à exécuter | ✅ Oui | Montrer la syntaxe exacte |
| Résultat de la commande | ✅ Oui | Montrer ce qui est attendu |
| Menu de configuration | ✅ Oui | Éviter l'ambiguïté ("onglet Avancé" — lequel ?) |
| Message d'erreur | ✅ Oui | Aider au dépannage |
| Fenêtre standard Windows | ⚠️ Optionnel | Si c'est évident (ex : "Suivant" dans un wizard), peut être omis |
| Longue sortie texte | ❌ Non | Copier-coller le texte dans un bloc code |

---

### III.B. Règles de Capture

```
   RÈGLE 1 — RÉSOLUTION ADAPTÉE
   ──────────────────────────────────────────────────────────────
   Pas trop grande (fichier lourd), pas trop petite (illisible)
   Recommandation : 1280×720 ou 1920×1080, rognée sur la zone utile

   RÈGLE 2 — ANNOTATIONS
   ──────────────────────────────────────────────────────────────
   Flécher les boutons à cliquer, encadrer les champs à remplir
   Outils : ShareX, Greenshot, Snagit, Paint.NET

   RÈGLE 3 — NUMÉROTATION COHÉRENTE
   ──────────────────────────────────────────────────────────────
   Nommer les captures selon l'étape :
   01_installation_apache_apt.png
   02_verification_version.png
   03_configuration_vhost.png

   RÈGLE 4 — LÉGENDE OBLIGATOIRE
   ──────────────────────────────────────────────────────────────
   Chaque capture doit avoir une légende :
   "Figure 1.2 — Installation d'Apache via apt"
   "Figure 2.1 — Vérification du statut du service"

   RÈGLE 5 — FORMAT
   ──────────────────────────────────────────────────────────────
   PNG pour les captures d'interface (sans compression)
   JPEG uniquement si photo réelle (pas de capture d'écran)
```

---

## PARTIE IV — Versioning de la Documentation

### IV.A. Pourquoi Versionner ?

Une procédure évolue : changement d'OS, nouvelle version du logiciel, correction d'erreurs. Sans versioning, impossible de savoir quelle version est la bonne.

```
   SANS VERSIONING                  AVEC VERSIONING
   ───────────────                  ───────────────
   Procédure_Apache.docx       →    Procédure_Apache_v1.0.docx
   Procédure_Apache_NEW.docx   →    Procédure_Apache_v1.1.docx
   Procédure_Apache_FINAL.docx →    Procédure_Apache_v2.0.docx
   Procédure_Apache_FINAL2.docx     (pas de "FINAL2")

   Historique intégré au document :
   v2.0 — 2024-11-15 — Migration vers Ubuntu 24.04
   v1.1 — 2024-08-10 — Ajout section SSL
   v1.0 — 2024-05-20 — Version initiale
```

---

### IV.B. Numérotation Sémantique

```
   VERSION   X.Y.Z
            │ │ │
            │ │ └─ Patch (correction mineure, typo, capture mise à jour)
            │ └─── Minor (ajout d'une section, amélioration sans changement majeur)
            └───── Major (changement de version d'OS, refonte complète)

   Exemples :
   1.0.0 → Version initiale
   1.1.0 → Ajout section "Troubleshooting"
   1.1.1 → Correction typo + mise à jour capture étape 3
   2.0.0 → Migration Ubuntu 22.04 → 24.04 (procédure adaptée)
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Procédure** | Document décrivant les étapes pour accomplir une tâche technique reproductible |
| **Guide utilisateur** | Document décrivant comment utiliser un service (destiné aux non-techniciens) |
| **DAT** | Dossier d'Architecture Technique — document décrivant l'infrastructure |
| **Capture annotée** | Image d'écran avec flèches, encadrés ou texte ajouté pour guider |
| **Versioning** | Gestion des versions successives d'un document avec numérotation sémantique |
| **Troubleshooting** | Section d'une procédure listant les erreurs courantes et leurs résolutions |
| **Prérequis** | Conditions devant être remplies avant d'exécuter une procédure |
| **Résultat attendu** | État ou sortie du système après exécution réussie de la procédure |
| **Validation** | Tests confirmant que la procédure a été correctement suivie |

---

---

# 🖥️ TP PARTIE 1 — AUDIT DE CONFORMITÉ GLPI

*Durée : 30 minutes — Binôme*

---

## Objectif

Analyser l'inventaire logiciel d'une instance GLPI/OCS et produire un rapport de conformité identifiant les écarts.

---

## Étape 1 — Export de l'Inventaire Logiciel (10 min)

**Depuis GLPI :**
1. Aller dans : Parc → Logiciels
2. Filtrer : Tous les logiciels
3. Exporter en CSV : Logiciels installés sur tous les ordinateurs

**Colonnes attendues dans le CSV :**
- Nom du logiciel
- Version
- Éditeur
- Nombre d'installations

---

## Étape 2 — Analyse des Licences (10 min)

Pour chaque logiciel listé, vérifier dans GLPI (Parc → Licences) :

| **Logiciel** | **Installations** | **Licences** | **Écart** | **Action** |
|---|---|---|---|---|
| Microsoft Office Standard 2021 | 85 | 78 | **-7** | ❌ Sous-licensing |
| Adobe Acrobat Reader DC | 120 | — | Freeware | ✅ OK |
| WinRAR | 90 | 0 | **-90** | ❌ Non licencié |
| AutoCAD 2024 | 5 | 5 | 0 | ✅ Conforme |
| 7-Zip | 100 | — | Open Source | ✅ OK |

---

## Étape 3 — Calcul du Taux de Conformité (5 min)

```
Nombre total de logiciels commerciaux installés : _______
Nombre de logiciels commerciaux conformes       : _______

Taux de conformité = (Conformes / Total) × 100 = _______%
```

---

## Étape 4 — Plan de Remédiation (5 min)

Pour chaque logiciel non conforme, proposer une action :

| **Logiciel** | **Écart** | **Action recommandée** | **Coût estimé** |
|---|---|---|---|
| Office Standard | -7 | Acheter 7 licences | 7 × 300 € = 2 100 € |
| WinRAR | -90 | Migrer vers 7-Zip (gratuit) | 0 € |

**Coût total de mise en conformité :** _______________ €

---

---

# ✍️ TP PARTIE 2 — RÉDIGER UNE PROCÉDURE

*Durée : 50 minutes — Individuel*

---

## Sujet

Rédiger une procédure complète d'installation d'un serveur DHCP sur Ubuntu Server 22.04.

**Contrainte :** Utiliser le modèle fourni en Annexe 2.

---

## Ressources Fournies

- Modèle de procédure vierge (Annexe 2)
- VM Ubuntu Server 22.04 (ou captures d'écran pré-faites si VM indisponible)
- Documentation officielle : `man isc-dhcp-server`

---

## Étapes à Documenter

Votre procédure doit couvrir :

1. Installation du paquet `isc-dhcp-server`
2. Configuration du fichier `/etc/dhcp/dhcpd.conf` avec un pool DHCP 192.168.10.100-200
3. Définition de l'interface réseau dans `/etc/default/isc-dhcp-server`
4. Démarrage et activation du service
5. Vérification des logs (`/var/log/syslog`)
6. Test avec un client DHCP

---

## Livrables

- Procédure complète au format PDF ou DOCX
- Minimum 3 captures d'écran annotées
- Section Troubleshooting avec au moins 2 erreurs courantes
- Historique des versions rempli

---

---

# 📄 ANNEXE 1 — GRILLE D'AUDIT DE CONFORMITÉ

*Pour les débutants — Cocher OK/KO pour chaque logiciel*

---

| **N°** | **Logiciel** | **Éditeur** | **Installations** | **Licence ?** | **Nombre licences** | **Conforme ?** |
|---|---|---|---|---|---|---|
| 1 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 2 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 3 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 4 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 5 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 6 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 7 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 8 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 9 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |
| 10 | | | | ☐ Oui ☐ Non | | ☐ OK ☐ KO |

**Synthèse :**
- Nombre total de logiciels commerciaux : _______
- Conformes : _______
- Non conformes : _______
- **Taux de conformité : _______%**

---

# 📄 ANNEXE 2 — MODÈLE DE PROCÉDURE VIERGE

*Format Word/LibreOffice — À compléter*

---

```
═══════════════════════════════════════════════════════════════════════
                          PROCÉDURE TECHNIQUE
                   [TITRE DE LA PROCÉDURE]
                          [SYSTÈME / VERSION]
═══════════════════════════════════════════════════════════════════════
Version      : 1.0
Date         : ____________________
Auteur       : ____________________
Réviseur     : ____________________
Statut       : ☐ Validé  ☑ Brouillon  ☐ Obsolète
Référence    : PROC-___-___-___
═══════════════════════════════════════════════════════════════════════

1. OBJECTIF
───────────────────────────────────────────────────────────────────────
Cette procédure permet de :
___________________________________________________________________
___________________________________________________________________


2. PÉRIMÈTRE
───────────────────────────────────────────────────────────────────────
✅ Inclus dans cette procédure :
• _________________________________________________________________
• _________________________________________________________________

❌ Exclu de cette procédure :
• _________________________________________________________________


3. PRÉREQUIS
───────────────────────────────────────────────────────────────────────
☐ Matériel : _______________________________________________________
☐ Logiciels : ______________________________________________________
☐ Accès : __________________________________________________________
☐ Compétences : ____________________________________________________


4. DURÉE ESTIMÉE
───────────────────────────────────────────────────────────────────────
Temps d'exécution : __________ minutes


5. ÉTAPES D'INSTALLATION
───────────────────────────────────────────────────────────────────────

5.1. [Titre de l'étape]
───────────────────────────────────────────────────────────────────────
Description :
___________________________________________________________________

Commande à exécuter :
```
___________________________________________________________________
```

Résultat attendu :
___________________________________________________________________

[CAPTURE D'ÉCRAN — Figure 5.1]


5.2. [Titre de l'étape]
───────────────────────────────────────────────────────────────────────
Description :
___________________________________________________________________

Commande à exécuter :
```
___________________________________________________________________
```

Résultat attendu :
___________________________________________________________________

[CAPTURE D'ÉCRAN — Figure 5.2]


[Répéter pour chaque étape]


6. VÉRIFICATION
───────────────────────────────────────────────────────────────────────
Tests à effectuer pour valider l'installation :

☐ Test 1 : _________________________________________________________
   Commande : ______________________________________________________
   Résultat attendu : ______________________________________________

☐ Test 2 : _________________________________________________________
   Commande : ______________________________________________________
   Résultat attendu : ______________________________________________


7. DÉPANNAGE (TROUBLESHOOTING)
───────────────────────────────────────────────────────────────────────

Erreur 1 : _________________________________________________________
Cause    : _________________________________________________________
Solution : _________________________________________________________

Erreur 2 : _________________________________________________________
Cause    : _________________________________________________________
Solution : _________________________________________________________


8. RÉFÉRENCES
───────────────────────────────────────────────────────────────────────
• Documentation officielle : _______________________________________
• Tickets GLPI liés : ______________________________________________
• Autres procédures liées : ________________________________________


9. HISTORIQUE DES VERSIONS
───────────────────────────────────────────────────────────────────────
Version │ Date       │ Auteur      │ Modification
────────┼────────────┼─────────────┼──────────────────────────────────
 1.0    │            │             │ Création initiale
────────┼────────────┼─────────────┼──────────────────────────────────
        │            │             │
════════════════════════════════════════════════════════════════════════
```

---

*Pack S11 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.1, B1.2, B1.4, B2.1, B3.3*
*Gestion actifs logiciels · Licences · Conformité · TCO · Documentation procédures*
