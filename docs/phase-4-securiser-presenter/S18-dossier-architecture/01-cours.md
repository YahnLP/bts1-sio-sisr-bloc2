# Cours – Le dossier d'architecture technique (DAT)

*BTS SIO SISR 1re année – Bloc 2 – S18 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S17.
> - **Matériel :** Projet de S17, modèle de DAT.

## À la fin de ce cours, tu sauras

- [ ] **Corriger** les dysfonctionnements résiduels d'une infrastructure déployée ;
- [ ] **Dérouler** un plan de test et **conclure** sur la conformité au cahier des charges ;
- [ ] **Rédiger** un DAT : contexte, architecture, choix justifiés, procédures ;
- [ ] **Produire** des schémas exploitables par un tiers ;
- [ ] **Rédiger** une fiche descriptive de réalisation (modèle E6).

---

## FICHE DE COURS — LE DOSSIER D'ARCHITECTURE TECHNIQUE (DAT)

### I. Qu'est-ce qu'un DAT ?

Un **Dossier d'Architecture Technique** (DAT) est un document de référence décrivant l'**architecture complète d'un système d'information** : ses composants, ses choix de conception, ses configurations et ses procédures d'exploitation. Il est rédigé par les techniciens qui ont déployé l'infrastructure et doit être exploitable par n'importe quel technicien compétent — y compris un successeur qui ne connaît rien au projet.

>  **Lien ITIL — Gestion des Configurations et Gestion des Changements :** Le DAT est l'artéfact central de la **Gestion des Configurations** ITIL : il constitue la **CMDB documentaire** (Configuration Management Database) du projet. Sans DAT, chaque intervention est une redécouverte, chaque panne est une urgence. Avec un DAT à jour, le MTTR (Mean Time To Repair) est drastiquement réduit.

---

### II. Structure d'un DAT Professionnel

Un DAT complet comprend les sections suivantes :

```
DAT — Dossier d'Architecture Technique
│
├── 1. PAGE DE GARDE & GESTION DU DOCUMENT
│     Titre, version, auteurs, date, historique des révisions
│
├── 2. CONTEXTE ET OBJECTIFS
│     Présentation du client, besoins exprimés, périmètre du projet
│
├── 3. ARCHITECTURE GLOBALE
│     Schéma général de l'infrastructure
│     Inventaire des composants (matériel et logiciel)
│
├── 4. ARCHITECTURE RÉSEAU
│     Plan d'adressage, VLANs, routage
│     Schéma réseau détaillé et légende
│     Configurations des équipements réseau
│
├── 5. ARCHITECTURE SYSTÈME
│     Serveurs : rôles, configurations
│     Active Directory, DHCP, DNS, Partages
│     Services Linux
│
├── 6. SÉCURITÉ
│     GPO déployées, politiques de sécurité
│     Contrôle d'accès (NTFS, FTP, SSH)
│     Points de vigilance et préconisations
│
├── 7. PROCÉDURES D'EXPLOITATION
│     Démarrage/arrêt des services
│     Création d'un utilisateur (procédure pas-à-pas)
│     Sauvegarde et restauration
│
├── 8. PLAN DE TESTS ET RÉSULTATS
│     Tableau des tests réalisés avec résultats
│
└── 9. ANNEXES
      Configurations complètes (CLI Cisco, fichiers .conf)
      Scripts PowerShell / Bash
      Captures d'écran des tests
```

---

### III. Principes de Rédaction d'un DAT

| **Principe** | **Application concrète** |
|---|---|
| **Exploitable par un tiers** | Un technicien qui ne connaît pas le projet doit pouvoir reconfigurer un service depuis le DAT seul |
| **Décisions justifiées** | Chaque choix technique ("j'ai choisi /26 pour le VLAN Commercial") doit être expliqué |
| **Gestion des versions** | Chaque modification du document crée une nouvelle version (v1.0, v1.1, v2.0) avec date et auteur |
| **Captures d'écran numérotées** | Toutes les captures référencées dans le texte (ex : "voir Figure 3") |
| **Langage professionnel** | Pas de "j'ai fait", mais "le service DHCP a été configuré avec..." |
| **Cohérence interne** | Ce qui est dans les schémas correspond à ce qui est dans les tableaux |
| **À jour** | Un DAT obsolète est pire qu'aucun DAT (il induit en erreur) |

---

### IV. Gestion des Versions

Un tableau de gestion de version figure en première page du DAT :

| **Version** | **Date** | **Auteur(s)** | **Objet de la révision** |
|---|---|---|---|
| 0.1 | S17 — [date] | [Noms] | Version initiale — plan d'adressage et architecture réseau |
| 0.2 | S17 — [date] | [Noms] | Ajout déploiement Windows Server + Linux |
| 1.0 | S18 — [date] | [Noms] | Version finale — tests validés, DAT complet |

>  **Règle pratique :** Les versions `0.x` sont des versions de travail (en cours de rédaction). La version `1.0` est la première version **validée et livrée au client**.

---

### V. Schéma Réseau — Exigences Professionnelles

Un schéma réseau dans un DAT doit respecter les conventions suivantes :

| **Élément** | **Convention** |
|---|---|
| **Représentation des équipements** | Icônes standardisées (Cisco ou draw.io) — pas de rectangles génériques |
| **Identification des équipements** | Nom d'hôte + adresse IP de gestion |
| **VLANs** | Couleurs distinctes par VLAN, légende obligatoire |
| **Liens** | Type de lien indiqué (trunk, access, Ethernet, fibre) |
| **Sous-réseaux** | Adresse réseau et masque indiqués pour chaque segment |
| **Orientation** | Internet en haut, utilisateurs finaux en bas |
| **Légende** | En bas à droite — tous les éléments graphiques expliqués |

---
