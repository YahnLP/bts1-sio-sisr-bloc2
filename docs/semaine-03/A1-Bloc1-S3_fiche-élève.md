# Semaine 3 (S3) - BLOC 1
## 🔧 ITIL Introduction · Service IT · Centre de Services · Niveaux de Support

---

# 📚 FICHE DE COURS ÉLÈVE
## "ITIL 4 · Service IT · Centre de Services"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 3*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.2** | Exploiter des référentiels, normes et standards (ITIL) |
| **B1.3** | Mettre en place et exploiter des outils de support |
| **B3.3** | Gérer et suivre un projet — Communication professionnelle |

---

## PARTIE I — Qu'est-ce qu'ITIL ?

### I.A. Origine et Définition

**ITIL** (Information Technology Infrastructure Library) est un **référentiel de bonnes pratiques** pour la gestion des services informatiques. Il ne s'agit pas d'une norme imposée (comme ISO 27001) ni d'une technologie — c'est un ensemble de recommandations issues de l'expérience de milliers d'organisations dans le monde.

| **Paramètre** | **Valeur** |
|---|---|
| **Créé par** | Cabinet britannique OGC (Office of Government Commerce) — années 1980 |
| **Version actuelle** | ITIL 4 (2019) |
| **Éditeur** | Axelos (filiale du gouvernement britannique) |
| **Certification** | ITIL Foundation (entrée de gamme, très répandue en entreprise) |
| **Utilisation** | DSI de toutes tailles — de la startup aux multinationales |

> 💡 **Pourquoi l'apprendre au BTS ?** Parce que **90% des offres d'emploi pour des postes de support ou d'administration IT mentionnent ITIL**. Connaître le vocabulaire ITIL vous positionne immédiatement comme un profil structuré et professionnel, même sans expérience.

---

### I.B. La Notion de Service IT

En ITIL, la notion de **service** est centrale. Un service IT est :

> *"Un moyen de créer de la valeur pour les clients en facilitant les résultats qu'ils souhaitent atteindre, sans qu'ils aient à gérer des coûts et des risques spécifiques."*

En clair : **le service IT porte le résultat métier, pas la technologie**.

```
   CE QUE LE CLIENT VOIT            CE QUE LA DSI GÈRE
   ──────────────────────           ──────────────────────────────────
   "Je peux envoyer des emails"     Serveur Exchange/Microsoft 365
                                    Réseau LAN + Internet
                                    Pare-feu et antispam
                                    Licences Microsoft
                                    Sauvegardes quotidiennes
                                    Mises à jour de sécurité
                                    Supervision 24h/24

   ← La valeur perçue               La complexité cachée →
```

*Légende : Le client perçoit uniquement le service rendu ("je peux envoyer des emails"). La DSI gère l'ensemble des composants technologiques qui rendent ce service possible. ITIL appelle cela le "voile du service" — la DSI absorbe la complexité pour que le client n'ait pas à la gérer.*

---

### I.C. Les 4 Dimensions d'un Service ITIL 4

ITIL 4 stipule que tout service IT doit être analysé selon 4 dimensions complémentaires. Oublier l'une d'elles conduit à des échecs de projet :

```
   ┌────────────────────────────────────────────────────────────┐
   │                                                            │
   │    ①  ORGANISATIONS ET PERSONNES                           │
   │       Qui fait quoi ? Compétences, rôles, culture,         │
   │       structure organisationnelle, formation               │
   │                                                            │
   │    ②  INFORMATIONS ET TECHNOLOGIE                          │
   │       Outils, logiciels, matériel, données, IA,            │
   │       bases de données, cloud                              │
   │                                                            │
   │    ③  PARTENAIRES ET FOURNISSEURS                          │
   │       Prestataires externes, contrats, dépendances,        │
   │       sous-traitance (ex : hébergeur cloud, mainteneur)    │
   │                                                            │
   │    ④  FLUX DE VALEUR ET PROCESSUS                          │
   │       Comment le travail est réalisé, procédures,          │
   │       workflows, automatisation, amélioration continue     │
   │                                                            │
   └────────────────────────────────────────────────────────────┘

   Ces 4 dimensions s'appliquent à CHAQUE service IT.
   Exemple : "Service messagerie SimIO SARL"
   ① Qui gère la messagerie ? Quelles compétences ?
   ② Exchange / Microsoft 365 / serveur mail Postfix ?
   ③ Microsoft est-il un partenaire ? Y a-t-il un prestataire externe ?
   ④ Quelle procédure quand un email ne passe pas ?
```

---

## PARTIE II — Le Vocabulaire Fondamental ITIL

### II.A. Les 4 Types de Sollicitations

Toute interaction entre un utilisateur et la DSI appartient à l'un de ces 4 types. Les connaître est **indispensable** pour rédiger correctement un ticket :

```
   ┌─────────────────────────────────────────────────────────────────┐
   │  TYPE          │  DÉFINITION                  │  EXEMPLE        │
   ├─────────────────────────────────────────────────────────────────┤
   │  INCIDENT      │  Interruption non planifiée  │  "Mon PC ne     │
   │                │  ou dégradation d'un service  │  démarre plus" │
   │                │  IT                           │                │
   ├─────────────────────────────────────────────────────────────────┤
   │  PROBLÈME      │  Cause racine d'un ou         │  "Pourquoi     │
   │                │  plusieurs incidents récurrents│  5 PC ont eu  │
   │                │  (peut être ouvert avant l'   │  le même crash │
   │                │  incident suivant)             │  ce mois ?"   │
   ├─────────────────────────────────────────────────────────────────┤
   │  CHANGEMENT    │  Ajout, modification ou       │  "Installer une│
   │                │  retrait d'un composant dans  │  nouvelle      │
   │                │  l'infrastructure             │  application"  │
   ├─────────────────────────────────────────────────────────────────┤
   │  DEMANDE DE    │  Demande standard d'accès     │  "Créer un     │
   │  SERVICE       │  ou d'information (non-panne) │  compte VPN    │
   │                │                               │  pour un nouvel│
   │                │                               │  employé"      │
   └─────────────────────────────────────────────────────────────────┘
```

> ⚠️ **Erreur fréquente :** Confondre **incident** et **problème**. Un incident est un **événement** ("Excel plante") — un problème est une **investigation de cause** ("pourquoi Excel plante-t-il sur tous les postes du VLAN 10 ?"). Un incident peut être résolu (contournement) sans que le problème sous-jacent soit traité.

---

### II.B. Cycle de vie d'un Ticket

```
   OUVERTURE          QUALIFICATION          TRAITEMENT
   ─────────          ─────────────          ──────────
   Utilisateur        Catégorie              N1 tente
   contacte le   →    Priorité          →    résolution
   centre             Impact/Urgence         → si KO :
   de services        Attribution            escalade N2/N3

                                                  │
   CLÔTURE            VALIDATION             RÉSOLUTION
   ─────────          ──────────             ──────────
   Ticket         ←   Utilisateur        ←   Solution
   fermé              confirme               appliquée,
   MTTR calculé       la résolution          testée
   Satisfaction ?
```

---

### II.C. Les Indicateurs Clés (KPIs)

| **Indicateur** | **Signification** | **Ce qu'il mesure** |
|---|---|---|
| **MTTR** | Mean Time To Repair | Temps moyen de résolution d'un incident |
| **MTBF** | Mean Time Between Failures | Durée moyenne entre deux pannes (fiabilité) |
| **MTTA** | Mean Time To Acknowledge | Temps moyen avant première prise en charge |
| **Taux de résolution N1** | % d'incidents résolus sans escalade | Efficacité du support de premier niveau |
| **SLA %** | % de tickets traités dans les délais contractuels | Respect des engagements de service |
| **FCR** | First Contact Resolution | % de tickets résolus dès le premier appel |
| **Backlog** | File d'attente des tickets ouverts non résolus | Volume de travail en attente |

---

### II.D. Le SLA — Service Level Agreement

Un **SLA** est un contrat formel entre un fournisseur de services IT (la DSI) et un client (un département ou une entreprise cliente). Il définit **le niveau de service attendu** et les conséquences si ce niveau n'est pas atteint.

**Composantes d'un SLA :**

| **Élément** | **Exemple concret** |
|---|---|
| **Disponibilité du service** | "Le service messagerie sera disponible 99,5% du temps (hors maintenance planifiée)" |
| **Délai de prise en charge** | "Tout incident Priorité 1 sera pris en charge en moins de 15 minutes" |
| **Délai de résolution** | "Tout incident P1 sera résolu en moins de 4 heures" |
| **Plage horaire** | "Le support est disponible du lundi au vendredi de 8h à 18h" |
| **Exclusions** | "Les incidents dus à une erreur utilisateur ne sont pas couverts par le SLA" |
| **Pénalités** | "En cas de non-respect du SLA P1, une réduction de 10% de la facture est appliquée" |

**Matrice de Priorité — Ce qu'utilise toute DSI :**

```
                            IMPACT
                    Élevé    Moyen    Faible
                 ┌────────┬────────┬────────┐
           Élevée│  P1    │  P2    │  P3    │
   URGENCE       │ CRIT.  │ HAUTE  │ MOYENNE│
           Faible│  P2    │  P3    │  P4    │
                 │ HAUTE  │MOYENNE │ BASSE  │
                 └────────┴────────┴────────┘

   P1 (Critique) : Service principal en panne — impact total
                   Ex : serveur AD planté, tous les utilisateurs bloqués
                   SLA : prise en charge < 15 min, résolution < 4h

   P2 (Haute)    : Service dégradé ou groupe d'utilisateurs impactés
                   Ex : imprimante partagée HS dans un service
                   SLA : prise en charge < 1h, résolution < 8h

   P3 (Moyenne)  : Un seul utilisateur impacté, contournement disponible
                   Ex : logiciel secondaire qui plante
                   SLA : prise en charge < 4h, résolution < 24h

   P4 (Basse)    : Demande ou incident mineur
                   Ex : changement de fond d'écran
                   SLA : prise en charge < 1 jour ouvré, résolution < 5 jours
```

---

### II.E. Les Pratiques ITIL 4 à Connaître

ITIL 4 définit **34 pratiques**. En voici les 10 incontournables pour un technicien SISR junior :

| **Pratique** | **Ce qu'elle couvre** | **Vue dans le cours** |
|---|---|---|
| **Gestion des incidents** | Restaurer le service le plus vite possible | S3, tous les TP |
| **Gestion des problèmes** | Éliminer les causes racines des incidents récurrents | S3 |
| **Gestion des demandes de service** | Traiter les demandes standard (comptes, accès...) | S3 |
| **Gestion des actifs IT** | Inventaire et cycle de vie des équipements | S2 |
| **Gestion des configurations** | CMDB — état de l'infrastructure | S2 |
| **Gestion des changements** | Contrôler les modifications de l'infrastructure | S17-S18 |
| **Gestion des niveaux de service** | SLA et suivi de la qualité | S3 |
| **Centre de services** | Point de contact unique utilisateurs / DSI | S3 |
| **Supervision et gestion des événements** | Détecter proactivement les anomalies | S9 |
| **Amélioration continue** | Mesurer et améliorer les services en permanence | Transversal |

---

## PARTIE III — Le Centre de Services

### III.A. Définition et Rôle

Le **centre de services** (ou **Service Desk**) est le **point de contact unique** entre les utilisateurs et la DSI. Il reçoit toutes les sollicitations (incidents, demandes, questions), les qualifie et les oriente vers les équipes appropriées.

```
   UTILISATEURS                   CENTRE DE SERVICES              ÉQUIPES TECHNIQUES
   ────────────                   ──────────────────              ──────────────────
   Marie (Compta)  ─── Tel ───►  ┌────────────────┐
   Pierre (RH)     ─── Mail ──►  │                │ ────────────► Équipe N2 Système
   Paul (Direction)─── Chat ──►  │   SERVICE      │ ────────────► Équipe N2 Réseau
   Nouveau PC ?    ─── GLPI ──►  │   DESK N1      │ ────────────► Équipe N3 Éditeur
   VPN KO ?        ───────────►  │                │ ────────────► Équipe N3 Sécurité
                                 └────────────────┘
                                         │
                                   Triage, tickets,
                                   résolution simple,
                                   escalade si nécessaire
```

**Les missions du centre de services :**

1. **Réception** de toutes les sollicitations (téléphone, email, portail self-service, chat)
2. **Qualification** : catégoriser, prioriser, enregistrer dans l'outil ITSM
3. **Résolution N1** : traiter les incidents simples (réinitialisations, accès, configurations basiques)
4. **Escalade** : transmettre aux niveaux supérieurs ce qui dépasse le N1
5. **Suivi** : informer l'utilisateur de l'avancement jusqu'à clôture
6. **Reporting** : produire les tableaux de bord (MTTR, SLA, volume...)

---

### III.B. Les Niveaux de Support N1 / N2 / N3

```
   ┌─────────────────────────────────────────────────────────────────┐
   │                      N3 — EXPERTISE                             │
   │  Éditeurs logiciels, constructeurs matériels, consultants       │
   │  spécialisés. Escalade extrêmement rare.                        │
   │  Exemples : bug dans le code de Microsoft, firmware d'un        │
   │  équipement réseau, faille de sécurité zero-day                 │
   ├─────────────────────────────────────────────────────────────────┤
   │                      N2 — SPÉCIALISTES                          │
   │  Techniciens expérimentés, spécialisés par domaine              │
   │  (réseau, système, sécurité, applicatif métier).                │
   │  Traitent les incidents complexes escaladés depuis le N1.       │
   │  Exemples : panne serveur, règle pare-feu, AD, base de données  │
   ├─────────────────────────────────────────────────────────────────┤
   │                N1 — CENTRE DE SERVICES (Helpdesk)               │
   │  Techniciens polyvalents (c'est votre futur poste immédiat !).  │
   │  Traitent ~70% des incidents sans escalade.                     │
   │  Exemples : réinitialisation de mot de passe, problème réseau   │
   │  simple, logiciel qui ne s'ouvre pas, imprimante HS             │
   └─────────────────────────────────────────────────────────────────┘
```

---

### III.C. Comparatif des Niveaux

| **Critère** | **N1** | **N2** | **N3** |
|---|---|---|---|
| **Profil** | Technicien junior / polyvalent | Technicien expérimenté / spécialisé | Expert / Éditeur |
| **Formation typique** | BTS SIO, Licence Pro | BTS SIO + expérience, LP, Bachelor | Ingénieur, CCIE, certif. expert |
| **Incidents traités** | Simples, procédurés | Complexes, sans procédure établie | Inconnus, non reproductibles |
| **Accès systèmes** | Restreint (AD, postes) | Étendu (serveurs, infra) | Total (production, code source) |
| **Délai de réponse** | Immédiat (SLA court) | Quelques heures | Jours, semaines |
| **Taux de résolution** | 60–80% des tickets entrants | 15–35% des tickets reçus | < 5% |
| **Outil** | GLPI, ServiceNow, Jira | GLPI, accès serveurs, CLI | Éditeur / lab de test |

---

### III.D. Critères d'Escalade N1 → N2

Un technicien N1 doit escalader vers N2 lorsqu'il rencontre l'une des situations suivantes :

| **Situation** | **Exemple** |
|---|---|
| **Hors compétence / accès** | L'incident nécessite des droits d'accès aux serveurs |
| **Dépassement du délai SLA** | Le ticket P2 risque de dépasser les 8h de délai |
| **Incident récurrent** | Le même utilisateur a eu 3 incidents similaires ce mois |
| **Impact élargi** | Le diagnostic révèle que plusieurs utilisateurs sont affectés |
| **Absence de procédure** | Aucune solution connue dans la base de connaissances |
| **Impact critique potentiel** | Le problème pourrait toucher un système de production |

> 📌 **Règle professionnelle :** Escalader n'est **pas un aveu d'échec** — c'est une **compétence professionnelle**. Un technicien N1 qui escalade au bon moment protège le SLA et la satisfaction utilisateur. Un N1 qui s'acharne sans escalader fait perdre du temps à tout le monde.

---

### III.E. Rédiger un Bon Ticket d'Incident

Un ticket mal rédigé ralentit la résolution et crée de la frustration. Un ticket bien rédigé permet au N2 (ou au N3) de comprendre la situation sans avoir à rappeler l'utilisateur.

**Structure obligatoire d'un ticket :**

| **Champ** | **Contenu attendu** | **Mauvais exemple** | **Bon exemple** |
|---|---|---|---|
| **Titre** | Résumé en 10 mots max | "Problème ordi" | "Excel 365 ne s'ouvre plus — PC-RH-042 — Alice Martin" |
| **Utilisateur** | Nom, service, contact | — | "Alice Martin — RH — ext. 214 — a.martin@siosarl.local" |
| **Description** | Quoi, quand, comment reproduire | "ça marche pas" | "Depuis ce matin 8h, Excel affiche 'Application non disponible' au démarrage. Le problème se reproduit à chaque tentative d'ouverture. Les autres applis Office fonctionnent." |
| **Impact** | Qui est bloqué, quel processus | — | "Bloquée pour le bilan mensuel à remettre à 10h" |
| **Actions déjà tentées** | Ce qui a été fait avant d'appeler | — | "Redémarrage du PC effectué — problème persistant" |
| **Priorité** | P1 à P4 selon la matrice | — | P2 (impact = 1 utilisateur critique, urgence = deadline métier) |
| **Pièces jointes** | Captures d'écran du message d'erreur | — | `capture_erreur_excel_09h15.png` |

---

## PARTIE IV — Les Outils ITSM

### IV.A. GLPI — Gestion Libre de Parc Informatique

**GLPI** est l'outil ITSM open source le plus répandu en France. Il gère à la fois les tickets et l'inventaire de parc (lien direct avec S2).

**Fonctions principales de GLPI :**

| **Module** | **Fonction** |
|---|---|
| **Tickets** | Création, suivi, escalade, clôture d'incidents et demandes |
| **Actifs** | Inventaire matériel et logiciel (lié à OCS Inventory) |
| **CMDB** | Relations entre les CIs (un ticket lié à un actif) |
| **Base de connaissances** | Articles de résolution pour les incidents récurrents |
| **Planification** | Calendrier de maintenance, interventions |
| **Rapports** | Tableaux de bord, statistiques SLA, MTTR... |

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **ITIL** | Information Technology Infrastructure Library — référentiel de bonnes pratiques IT |
| **Service IT** | Moyen de créer de la valeur pour un client en prenant en charge la complexité technologique |
| **Incident** | Interruption non planifiée ou dégradation d'un service IT |
| **Problème** | Cause racine d'un ou plusieurs incidents |
| **Changement** | Ajout, modification ou retrait d'un composant de l'infrastructure |
| **Demande de service** | Demande standard d'accès ou d'information (non-panne) |
| **SLA** | Service Level Agreement — contrat définissant le niveau de service attendu |
| **MTTR** | Mean Time To Repair — temps moyen de résolution d'un incident |
| **MTBF** | Mean Time Between Failures — temps moyen entre deux pannes |
| **FCR** | First Contact Resolution — résolution dès le premier appel |
| **Centre de services** | Point de contact unique utilisateurs / DSI (Service Desk) |
| **N1 / N2 / N3** | Niveaux de support du plus généraliste (N1) au plus expert (N3) |
| **Escalade** | Transfert d'un ticket vers un niveau de support supérieur |
| **Priorité** | Combinaison de l'impact et de l'urgence d'un incident (P1 à P4) |
| **GLPI** | Gestion Libre de Parc Informatique — outil ITSM open source français |
| **Base de connaissances** | Référentiel de solutions pour les incidents récurrents |
| **Backlog** | File d'attente des tickets ouverts non résolus |
| **CMDB** | Configuration Management DataBase — base de données des actifs IT |
| **KPI** | Key Performance Indicator — indicateur clé de performance |
| **DSI** | Direction des Systèmes d'Information |

---
