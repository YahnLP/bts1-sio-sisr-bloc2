# Pack de Formation - Semaine 7 (S7) - BLOC 1
## 🚀 Mise à Disposition de Services · Qualité de Service · SLA · Disponibilité

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S7 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — septième semaine |
| **Modalité** | Présentiel — salle TP |
| **Prérequis** | S3 (ITIL, SLA notion), S6 (GLPI opérationnel) |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.5** | Mettre à disposition des utilisateurs un service informatique | Acquisition |
| **B1.2** | Exploiter des référentiels, normes et standards (ITIL) | Maîtrise |
| **B1.6** | Assurer le support des utilisateurs | Maîtrise |
| **B3.3** | Participer à la gestion et au suivi d'un projet | Acquisition |

> 📌 **S7 clôture le Bloc 1** avant le virage technique du Bloc 2. Elle ancre deux compétences transversales qui traverseront tout le programme : savoir **mettre à disposition un service** de façon structurée (pas juste "l'installer"), et savoir mesurer et garantir la **qualité de ce service** via le SLA et les indicateurs de disponibilité. Ces deux compétences sont systématiquement évaluées en E5.

---

## Objectifs Pédagogiques

À l'issue de cette séance, l'apprenant sera capable de :

**Mise à disposition d'un service :**
- ✅ Décrire les **5 étapes** de mise à disposition d'un service IT (analyse → installation → documentation → communication → validation)
- ✅ Distinguer "installer un service" de "mettre à disposition" un service
- ✅ Produire un **document de mise en service** (PV de mise en production)
- ✅ Rédiger une **communication utilisateur** pour l'annonce d'un nouveau service
- ✅ Définir les **critères d'acceptation** d'un service avant sa mise en production

**Qualité de service :**
- ✅ Calculer le **taux de disponibilité** d'un service (uptime/downtime)
- ✅ Convertir un pourcentage de disponibilité en **temps d'indisponibilité annuel**
- ✅ Distinguer **disponibilité planifiée** et **indisponibilité non planifiée**
- ✅ Identifier les composantes d'un **SLA complet**
- ✅ Expliquer les notions de **RTO** et **RPO** dans le contexte de la continuité de service

---

## ⭐ Spécificités Pédagogiques

### Clôture du Bloc 1 — Ce que S7 Consolide

S7 arrive à un moment charnière : les apprenants ont tous les outils (GLPI, OCS, tickets, incidents), mais ils n'ont pas encore réfléchi à ce que signifie **livrer un service** de façon professionnelle. C'est la différence entre un technicien qui installe et un professionnel IT qui met en production.

Ce changement de posture est fondamental pour le BTS SIO et pour la vie professionnelle. Il doit être verbalisé :

> *"Un développeur qui code une feature et dit 'ça marche sur ma machine' n'a pas livré un service. Un technicien qui installe Apache et dit 'c'est installé' n'a pas mis en production un service. Mettre à disposition un service, c'est s'assurer que les utilisateurs peuvent l'utiliser de façon fiable, qu'ils savent qu'il existe, qu'ils savent comment le demander, et qu'on est capable de le maintenir dans le temps."*

### La Disponibilité : Un Concept à Rendre Concret

Les pourcentages de disponibilité (99%, 99,9%, 99,99%) sont abstraits jusqu'à ce qu'on les convertisse en temps réel. Le moment où un apprenant réalise que "99,9% de disponibilité = 8h45 de panne par an" est souvent un déclic. Prévoir ce moment dans la séance — il est pédagogiquement très efficace.

### Lien avec le Projet SimIO (S17-S18)

Tout ce que les apprenants liront en S7 sur la mise à disposition de services, ils devront l'appliquer concrètement lors du Projet 1 (S17-S18) et pour chaque SPS de leur dossier E5. Signaler ce lien dès maintenant pour que S7 soit perçue comme une séance d'investissement, pas comme une séance théorique abstraite.

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S6 | Retour rapide sur le TP GLPI — questions en suspens |
| H+0:10 | 20 min | 🎯 Découverte | Activité "Le Service Fantôme" |
| H+0:30 | 45 min | 📖 Cours | Mise à disposition d'un service — 5 étapes, PV, communication |
| H+1:15 | **15 min** | ☕ **PAUSE** | — |
| H+1:30 | 40 min | 📖 Cours | Qualité de service — disponibilité, SLA complet, RTO/RPO |
| H+2:10 | 50 min | 🖥️ **TP** | Exercices : calculs disponibilité + rédaction SLA + PV de mise en service |
| H+3:00 | 30 min | ✅ Correction | Correction collective — calculs + SLA + PV |
| H+3:30 | 20 min | 🏁 **Bilan Bloc 1** | Synthèse des 7 semaines — compétences acquises — transition vers Bloc 2 |
| H+3:50 | 10 min | 🔭 Perspective | Présentation du Bloc 2 — réseaux, systèmes, scripting |

---

## Différenciation Pédagogique

### Profil Avancé
- Approfondir le concept de **haute disponibilité** (HA) : clustering, failover, load balancing (préfigure S26 Bloc 2 Année 2)
- Étudier les niveaux de **tolérance aux pannes** des datacenters : Tier I à Tier IV (Uptime Institute)
- Rédiger un **SLA complet et réaliste** pour le futur service DHCP de SimIO (S13) avec pénalités contractuelles
- Calculer l'impact financier d'une panne : coût/heure × durée × probabilité

### Profil Débutant
- Fournir la **table de référence de disponibilité** (les 5 niveaux de "nines") comme aide-mémoire
- Utiliser des analogies concrètes : disponibilité d'un supermarché, d'un distributeur de billets, d'un service SNCF
- Simplifier le TP : se concentrer sur les calculs de disponibilité et la rédaction de la communication utilisateur

---

## Matériel Nécessaire

| **Ressource** | **Détail** |
|---|---|
| **Calculatrice** | Pour les exercices de disponibilité |
| **Sujets TP** | Intégrés dans ce pack |
| **Modèle PV mise en service** | Annexe 1 |
| **Modèle SLA** | Annexe 2 |

---

## Lien avec le Référentiel Qualiopi

- ✅ Compétence B1.5 (mise à disposition) introduite et pratiquée
- ✅ Clôture formalisée du Bloc 1 avec synthèse des 7 semaines
- ✅ Lien explicite avec la suite du programme (Bloc 2, Projet SimIO, E5)
- ✅ Production de livrables professionnels (PV, SLA, communication) versables au portfolio

---

---

# 🎯 ACTIVITÉ DE DÉCOUVERTE
## "Le Service Fantôme"

*Durée : 20 minutes — Collectif*

---

## Mise en Situation (5 min)

L'enseignant lit le scénario :

---

> **Scénario :** Thomas est technicien dans une PME. Son responsable lui demande de "mettre en place un serveur de fichiers partagés pour le service RH".
>
> Thomas est consciencieux. Il passe une après-midi à installer et configurer le serveur. Le dossier partagé est créé, les droits NTFS sont corrects, le partage réseau fonctionne. Il teste depuis son propre poste : impeccable.
>
> Le lendemain matin, il croise sa responsable dans le couloir et lui dit : "C'est fait."
>
> Deux semaines plus tard, un incident : les fichiers RH sont devenus inaccessibles. En fouillant les tickets, le responsable DSI découvre que le partage était sur le disque C: du serveur — celui sur lequel Windows est installé. Ce disque a failli saturer, causant l'inaccessibilité. Personne ne le savait car le serveur n'était pas supervisé.
>
> Le responsable RH, lui, n'a découvert l'existence du serveur de fichiers que la veille de l'incident — son manager lui avait dit "les IT ont mis quelque chose en place". Aucune procédure d'utilisation n'avait été transmise. Résultat : certains collègues sauvegardaient encore leurs fichiers en local.

---

## Questions Guidées (10 min)

| **Question** | **Concept visé** |
|---|---|
| "Est-ce que Thomas a mal fait son travail techniquement ?" | Non — mais il a arrêté trop tôt |
| "Qu'est-ce qui manquait après l'installation ?" | Documentation, communication, supervision, critères d'acceptation |
| "Qui aurait dû être informé et de quoi ?" | Les utilisateurs RH — existence, chemin, procédure d'usage |
| "Qu'est-ce qui aurait évité l'incident disque ?" | Supervision, seuil d'alerte, SLA de stockage |
| "Le service a-t-il vraiment été 'mis à disposition' ?" | Non — installé ≠ mis à disposition |

## Conclusion (5 min)

L'enseignant écrit au tableau la distinction fondamentale de S7 :

```
   INSTALLER UN SERVICE          METTRE À DISPOSITION UN SERVICE
   ─────────────────────         ──────────────────────────────────
   Action technique               Démarche complète :
   Terminée quand ça marche       ① Analyse du besoin
   sur ma machine                 ② Installation et tests
                                  ③ Documentation technique
                                  ④ Communication utilisateurs
                                  ⑤ Validation et suivi
                                  ← Terminée quand les utilisateurs
                                    utilisent le service de façon
                                    fiable et autonome
```

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "Mise à Disposition · Qualité de Service · SLA · Disponibilité"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 7*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.5** | Mettre à disposition des utilisateurs un service informatique |
| **B1.2** | Exploiter des référentiels et standards (ITIL) |
| **B1.6** | Assurer le support des utilisateurs |

---

## PARTIE I — Mettre à Disposition un Service IT

### I.A. Les 5 Étapes de la Mise à Disposition

Mettre un service à disposition des utilisateurs est un **processus en 5 étapes**. Chaque étape produit un livrable documentaire.

```
  ①  ANALYSE DU BESOIN
  ───────────────────────────────────────────────────
  Comprendre ce que les utilisateurs ont besoin de faire
  (pas seulement ce qu'ils demandent).

  Livrables : cahier des charges, expression de besoins
  Questions clés :
    - Combien d'utilisateurs utiliseront ce service ?
    - Quand ont-ils besoin d'y accéder ? (horaires, mobilité)
    - Quelles données traitera ce service ? (sensibilité RGPD)
    - Quel niveau de disponibilité est requis ?
    - Quel est le délai de mise en place ?

  ───────────────────────────────────────────────────
  ②  INSTALLATION ET TESTS
  ───────────────────────────────────────────────────
  Déployer le service dans un environnement de test,
  puis en production après validation.

  Livrables : plan de tests, PV de recette (résultats des tests)
  Bonnes pratiques :
    - Tester AVANT la mise en production
    - Tester les cas normaux ET les cas limites
    - Tester la restauration (pas seulement la sauvegarde)
    - Faire valider les tests par quelqu'un d'autre que celui
      qui a installé le service

  ───────────────────────────────────────────────────
  ③  DOCUMENTATION TECHNIQUE
  ───────────────────────────────────────────────────
  Documenter le service pour permettre sa maintenance
  par n'importe quel technicien de l'équipe.

  Livrables : DAT (Dossier d'Architecture Technique),
              procédures d'exploitation, guide de dépannage
  Contenu minimal :
    - Architecture (schéma + description)
    - Configuration (paramètres, fichiers de conf)
    - Procédures : démarrage, arrêt, sauvegarde, restauration
    - Contacts (responsable technique, fournisseur, support N2)

  ───────────────────────────────────────────────────
  ④  COMMUNICATION AUX UTILISATEURS
  ───────────────────────────────────────────────────
  Informer les utilisateurs de l'existence du service,
  de comment y accéder et de comment obtenir du support.

  Livrables : email/note d'information, guide utilisateur,
              entrée dans la base de connaissances GLPI
  Contenu obligatoire :
    - Quoi : nom et description du service en termes métier
    - Pourquoi : bénéfice pour l'utilisateur
    - Comment accéder : procédure simple, pas de jargon
    - Qui contacter en cas de problème
    - Date de disponibilité

  ───────────────────────────────────────────────────
  ⑤  VALIDATION ET SUIVI (PV de Mise en Service)
  ───────────────────────────────────────────────────
  Formaliser la mise en production et définir
  les indicateurs de suivi.

  Livrables : PV de mise en service signé,
              SLA défini, supervision configurée
  Ce qui est validé :
    - Tests de recette passés avec succès
    - Documentation disponible et à jour
    - Utilisateurs informés
    - Supervision active (alertes configurées)
    - SLA formalisé et accepté par les parties
```

---

### I.B. Le PV de Mise en Service

Le **Procès-Verbal de Mise en Service** (ou **PV de recette**) est le document qui formalise qu'un service est prêt à être utilisé en production. C'est la signature qui marque le passage de "en test" à "en production".

Il contient systématiquement :

| **Section** | **Contenu** |
|---|---|
| **Identification** | Nom du service, version, date, technicien responsable |
| **Périmètre** | Ce qui est mis en service (et ce qui ne l'est pas encore) |
| **Tests réalisés** | Liste des tests avec résultat attendu / résultat obtenu |
| **Anomalies constatées** | Problèmes identifiés (même mineurs) avec leur statut |
| **Conditions de mise en service** | Réserves éventuelles (ex : "sous réserve de la sauvegarde quotidienne") |
| **Validation** | Signature du technicien + du responsable DSI (+ client si externe) |
| **SLA applicable** | Référence au SLA en vigueur pour ce service |

> 📌 **Lien avec l'E5 :** Le jury E5 peut demander "comment avez-vous validé que votre service était prêt à être mis en production ?" Un PV de mise en service est la réponse professionnelle à cette question.

---

### I.C. Communication Utilisateur — Bonnes Pratiques

La communication aux utilisateurs est souvent négligée par les techniciens. Pourtant, un service inconnu ou mal expliqué n'est pas utilisé — et un service pas utilisé n'a aucune valeur.

**Les 5 erreurs courantes dans une communication IT :**

| **Erreur** | **Exemple** | **Bonne pratique** |
|---|---|---|
| **Jargon technique** | "Le serveur SFTP est accessible sur le port 22" | "Vos fichiers sont maintenant accessibles depuis n'importe où" |
| **Oublier le bénéfice** | "Un partage réseau a été créé" | "Fini les clés USB : vos fichiers sont maintenant partagés et sauvegardés automatiquement" |
| **Procédure trop complexe** | 2 pages de configuration | 3 étapes illustrées |
| **Pas de contact support** | — | "En cas de problème : ticket GLPI, catégorie Réseau > Partage de fichiers" |
| **Pas de date** | "Disponible prochainement" | "Disponible à partir du lundi 15 mars à 8h" |

---

## PARTIE II — Qualité de Service

### II.A. La Disponibilité

La **disponibilité** d'un service IT est le pourcentage de temps pendant lequel ce service est accessible et fonctionnel. C'est l'indicateur de qualité de service le plus fondamental.

```
   Disponibilité (%) =  Temps de fonctionnement  × 100
                        ──────────────────────────────
                         Temps total de la période
```

**Les "nines" — Référence universelle en DSI :**

| **Disponibilité** | **Indisponibilité / an** | **Indisponibilité / mois** | **Usage typique** |
|---|---|---|---|
| **90%** ("one nine") | 36 jours 12h | 73 heures | Service non critique |
| **99%** ("two nines") | 3 jours 15h | 7h 18 min | Service standard |
| **99,5%** | 1 jour 19h | 3h 39 min | SLA PME typique |
| **99,9%** ("three nines") | 8h 45 min | 43 min | Service professionnel |
| **99,99%** ("four nines") | 52 min | 4 min 22s | Service critique (banque, santé) |
| **99,999%** ("five nines") | 5 min 15s | 26s | Téléphonie, bloc opératoire |

> 💡 **Le déclic pédagogique :** "99% de disponibilité ça semble très bien... jusqu'à ce qu'on réalise que ça représente 3 jours et demi de panne par an. Si votre service de messagerie est en panne 3 jours et demi, que se passe-t-il dans l'entreprise ?"

---

### II.B. Calcul de Disponibilité — Formules

**Formule de base :**
```
   Disponibilité = (Temps total - Temps d'indisponibilité) / Temps total × 100

   Exemple :
   Service disponible 8 715 heures sur 8 760 heures (1 an)
   → Disponibilité = 8 715 / 8 760 × 100 = 99,49%
   → Indisponibilité = 8 760 - 8 715 = 45 heures dans l'année
```

**Indisponibilité tolérable selon un SLA :**
```
   Indisponibilité max = Temps total × (1 - Disponibilité contractuelle)

   Exemple : SLA 99,9% sur 1 an (8 760 heures)
   → Indisponibilité max = 8 760 × (1 - 0,999) = 8,76 heures/an
   → Soit environ 8 heures 45 minutes de panne autorisées sur l'année
```

**Disponibilité sur une période personnalisée :**
```
   Heures disponibles dans 1 an  = 365 × 24 = 8 760 h
   Heures disponibles en 1 mois  = 730 h (moyenne)
   Heures disponibles en 1 semaine = 168 h

   Disponibilité 99,9% mensuelle :
   → 730 × (1 - 0,999) = 0,73 heure = 43 minutes 48 secondes
```

---

### II.C. Disponibilité Planifiée vs Non Planifiée

Toute indisponibilité n'est pas équivalente. Une DSI mature distingue deux types :

| **Type** | **Définition** | **Exemple** | **Comptabilisé dans le SLA ?** |
|---|---|---|---|
| **Maintenance planifiée** | Interruption programmée, annoncée à l'avance | Mise à jour Windows, sauvegarde hebdomadaire | Souvent **exclu** du SLA |
| **Indisponibilité non planifiée** | Panne ou interruption imprévue | Disque dur défaillant, panne réseau | Toujours **inclus** dans le SLA |

> 📌 **Pourquoi cette distinction est-elle cruciale ?** Un SLA à 99,9% qui exclut les maintenances planifiées (30 minutes/semaine = 26h/an) est très différent d'un SLA à 99,9% sur le temps total. La **plage horaire** et les **exclusions** d'un SLA sont aussi importantes que le pourcentage lui-même.

---

### II.D. Le SLA Complet — Toutes les Composantes

En S3, vous avez vu les composantes de base d'un SLA. En S7, voici le SLA complet tel qu'il est rédigé en contexte professionnel :

```
╔══════════════════════════════════════════════════════════════════╗
║              SERVICE LEVEL AGREEMENT — STRUCTURE COMPLÈTE        ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  1. PARTIES ET OBJET                                             ║
║     Prestataire de service (DSI), client (département/entité)   ║
║     Service concerné, durée du SLA, conditions de révision      ║
║                                                                  ║
║  2. DESCRIPTION DU SERVICE                                       ║
║     Ce que le service fait (en termes métier, pas technique)    ║
║     Ce qu'il ne fait PAS (périmètre explicite)                   ║
║     Conditions d'utilisation nominale                            ║
║                                                                  ║
║  3. DISPONIBILITÉ                                                ║
║     Taux de disponibilité contractuel (ex : 99,5%)              ║
║     Plage horaire couverte (ex : lundi-vendredi 8h-19h)         ║
║     Maintenance planifiée : fenêtre et préavis                   ║
║     Méthode de calcul et de mesure                               ║
║                                                                  ║
║  4. DÉLAIS DE SUPPORT                                            ║
║     Priorité P1 : prise en charge < 15 min, résolution < 4h     ║
║     Priorité P2 : prise en charge < 1h, résolution < 8h         ║
║     Priorité P3 : prise en charge < 4h, résolution < 24h        ║
║     Priorité P4 : résolution < 5 jours ouvrés                   ║
║                                                                  ║
║  5. CONTINUITÉ ET REPRISE (RTO / RPO)                           ║
║     RTO : délai maximum de reprise après incident majeur        ║
║     RPO : perte de données maximale acceptable                  ║
║                                                                  ║
║  6. EXCLUSIONS ET LIMITATIONS                                    ║
║     Cas non couverts (erreur utilisateur, cas de force majeure) ║
║     Conditions d'annulation du SLA                              ║
║                                                                  ║
║  7. INDICATEURS ET REPORTING                                     ║
║     KPIs mesurés, fréquence des rapports, responsable           ║
║                                                                  ║
║  8. PÉNALITÉS ET COMPENSATIONS                                   ║
║     Conséquences en cas de non-respect (réduction facture,      ║
║     crédits de service...)                                       ║
║                                                                  ║
║  9. RÉVISION ET RÉSILIATION                                      ║
║     Fréquence de révision du SLA, conditions de résiliation     ║
╚══════════════════════════════════════════════════════════════════╝
```

---

### II.E. RTO et RPO — Continuité de Service

Ces deux indicateurs définissent les **objectifs de reprise** après un incident majeur (panne grave, sinistre, cyberattaque) :

```
   INCIDENT MAJEUR
   ──────────────────────────────────────────────────────────────────

   Dernière        Incident         Reprise du        Retour
   sauvegarde      survient         service           à la normale
   propre          │                │                 │
       │           │                │                 │
       ├───────────┼────────────────┼─────────────────┤
       │           │                │
       ◄── RPO ───►◄───── RTO ──────►

   RPO (Recovery Point Objective)
   ────────────────────────────────
   Perte de données maximale acceptable.
   "Jusqu'où peut-on remonter dans le temps ?"
   Ex : RPO = 4h → on accepte de perdre au maximum 4h de données
   → Sauvegarde toutes les 4h minimum

   RTO (Recovery Time Objective)
   ────────────────────────────────
   Durée maximale acceptable pour reprendre le service.
   "En combien de temps maximum faut-il être rétabli ?"
   Ex : RTO = 2h → le service doit être restauré en moins de 2h
   → Nécessite un serveur de secours, des procédures de bascule...
```

**Les RTO/RPO varient selon la criticité du service :**

| **Type de service** | **RTO typique** | **RPO typique** |
|---|---|---|
| Service critique (messagerie, ERP) | < 2 heures | < 1 heure |
| Service important (partage de fichiers) | < 8 heures | < 4 heures |
| Service standard (intranet) | < 24 heures | < 24 heures |
| Service non critique (outil interne) | < 72 heures | < 24 heures |

> 💡 **Relation RTO/RPO avec la sauvegarde :** Un RPO de 4h impose une sauvegarde au moins toutes les 4h. Un RTO de 2h impose de disposer d'un environnement de secours capable de prendre le relais en moins de 2h. Ces exigences ont un **coût** direct — c'est pourquoi chaque service n'a pas les mêmes objectifs.

---

### II.F. Supervision et Alertes — Mesurer la Disponibilité

Un SLA ne peut pas être tenu sans **supervision** : il faut mesurer la disponibilité réelle pour la comparer à la disponibilité contractuelle.

```
   OUTILS DE SUPERVISION (aperçu — détaillés en S9)
   ─────────────────────────────────────────────────
   Nagios / Centreon     → Supervision infrastructure (ping, ports, services)
   Zabbix                → Supervision avancée (métriques, graphiques, alertes)
   PRTG                  → Supervision réseau et système (Windows/Linux)
   Uptime Robot          → Supervision web simple (gratuit, SaaS)
   Windows SNMP / WMI    → Intégré Windows Server pour la supervision locale

   Ce que la supervision mesure pour calculer la disponibilité :
   ├── Le service répond-il ? (ping, port TCP ouvert, HTTP 200)
   ├── Depuis quand est-il injoignable ? (horodatage de la panne)
   ├── Durée totale d'indisponibilité sur la période
   └── → Calcul automatique du taux de disponibilité réel
```

---

## PARTIE III — Synthèse Bloc 1 — Vue Complète

Le Bloc 1 (S1-S7) vous a donné une vision complète du **support et de la mise à disposition des services IT** :

```
   INVENTORIER               QUALIFIER                 TRAITER
   ──────────                ─────────                 ───────
   S2 Fiche technique   →    S3 ITIL, tickets,    →    S4 Incidents :
   S5 OCS Inventory          SLA, niveaux N1/2/3       diagnostic,
   S6 GLPI CMDB                                        résolution,
                                                       documentation

                                    ↓

   OUTILLER                  METTRE À DISPOSITION      MESURER
   ────────                  ────────────────────       ───────
   S6 GLPI tickets,     →    S7 Les 5 étapes,      →   S7 Disponibilité,
   base de connaissances,     PV de mise en service,    SLA complet,
   statistiques               communication users       RTO / RPO
```

---

## IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Mise à disposition** | Processus complet rendant un service accessible, documenté et utilisable par les utilisateurs |
| **PV de mise en service** | Document formalisant la mise en production d'un service après validation des tests |
| **Disponibilité** | Pourcentage de temps pendant lequel un service est accessible et fonctionnel |
| **Uptime** | Temps de fonctionnement d'un service (anglais pour "disponibilité") |
| **Downtime** | Temps d'indisponibilité d'un service |
| **"Nines"** | Convention de notation de la disponibilité : 99% = "two nines", 99,9% = "three nines"... |
| **Maintenance planifiée** | Interruption programmée et annoncée à l'avance (souvent exclue du SLA) |
| **RTO** | Recovery Time Objective — délai maximum pour reprendre un service après incident |
| **RPO** | Recovery Point Objective — perte de données maximale acceptable |
| **SLA** | Service Level Agreement — contrat définissant le niveau de service attendu |
| **Supervision** | Surveillance automatisée de l'état des services pour détecter les pannes |
| **Taux de disponibilité** | Pourcentage calculé : temps de fonctionnement / temps total × 100 |
| **Plage horaire** | Période durant laquelle le SLA s'applique (ex : 8h-18h jours ouvrés) |
| **Fenêtre de maintenance** | Créneau prévu et communiqué pour les interventions planifiées |
| **Continuité de service** | Capacité à maintenir ou reprendre rapidement un service après incident |
| **Critère d'acceptation** | Condition qui doit être remplie pour valider la mise en production |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Je décris les 5 étapes de mise à disposition d'un service
- [ ] J'explique la différence entre "installer" et "mettre à disposition"
- [ ] Je calcule un taux de disponibilité à partir d'un temps d'indisponibilité
- [ ] Je convertis un % de disponibilité en heures de panne annuelles
- [ ] Je distingue maintenance planifiée et indisponibilité non planifiée
- [ ] J'explique RTO et RPO avec un exemple concret
- [ ] Je rédige les sections essentielles d'un SLA
- [ ] Je rédige une communication utilisateur pour un nouveau service

---

---

# 🖥️ FICHE TP — DISPONIBILITÉ · SLA · PV DE MISE EN SERVICE

*Durée : 50 minutes — Individuel*

---

## MODULE 1 — Calculs de Disponibilité (15 min)

### Exercice 1.1 — Calcul de base

Un service de messagerie a été en panne pendant les durées suivantes en 2024 :
- 12 février : 2 heures 30 minutes (panne réseau)
- 15 mai : 45 minutes (mise à jour planifiée)
- 3 août : 6 heures (disque défaillant)
- 19 novembre : 1 heure 15 minutes (coupure électrique)

2024 est une **année bissextile** (366 jours).

| **Question** | **Calcul** | **Résultat** |
|---|---|---|
| Durée totale d'indisponibilité | | h min |
| Temps total de l'année (en heures) | | h |
| Taux de disponibilité | | % |
| Le SLA de 99,5% est-il respecté ? | | ☐ Oui ☐ Non |

---

### Exercice 1.2 — Objectif de disponibilité → temps de panne

Calculer l'indisponibilité maximale tolérable par an et par mois pour chaque SLA :

| **SLA contractuel** | **Indisponibilité max / an** | **Indisponibilité max / mois** |
|---|---|---|
| 99% | | |
| 99,5% | | |
| 99,9% | | |
| 99,99% | | |

*(Utiliser : 1 an = 8 760h, 1 mois = 730h)*

---

### Exercice 1.3 — Analyse d'un rapport mensuel

Le rapport de supervision du mois d'octobre (744 heures) indique :

| **Service** | **Temps d'indisponibilité** | **Dont planifié** | **Disponibilité réelle** | **SLA contractuel** | **Respecté ?** |
|---|---|---|---|---|---|
| Messagerie | 2h 15min | 30min | | 99,9% | |
| Partage de fichiers | 5h 00min | 1h 00min | | 99,5% | |
| Intranet | 0h 45min | 0h | | 99,5% | |
| VPN | 12h 30min | 2h | | 99% | |

> **Question :** Pour le service VPN, en distinguant indisponibilité planifiée et non planifiée, quel est le taux de disponibilité "hors maintenance" ? Le SLA est-il respecté sur cette base ?

```
Calcul : _______________________________________________________________
Réponse : ______________________________________________________________
```

---

## MODULE 2 — Rédiger un SLA (20 min)

### Contexte

SimIO SARL (80 employés) vient de déployer un **serveur de fichiers partagés** sur Windows Server. Vous êtes technicien DSI et vous devez rédiger le SLA de ce service pour les 6 prochains mois.

Informations à votre disposition :
- Le service est utilisé par tous les employés du lundi au vendredi, de 7h30 à 19h
- La maintenance est possible chaque dimanche entre 22h et 6h
- Le serveur est sauvegardé toutes les nuits à 23h
- Une panne du serveur de fichiers bloquerait le travail de tous les services
- L'équipe DSI (2 techniciens) travaille de 8h à 18h en semaine

Rédiger un SLA en complétant le modèle ci-dessous :

---

**SLA — Service de Partage de Fichiers — SimIO SARL**

| **Section** | **Contenu à rédiger** |
|---|---|
| **1. Parties** | Prestataire : DSI SimIO SARL / Bénéficiaire : |
| **2. Service couvert** | |
| **3. Ce qui est EXCLU du service** | |
| **4. Plage horaire du SLA** | |
| **5. Disponibilité contractuelle** | % |
| **6. Maintenance planifiée** | Fenêtre : ___ Préavis : ___ |
| **7. Délais de support P1** | Prise en charge < ___ Résolution < ___ |
| **8. Délais de support P2** | Prise en charge < ___ Résolution < ___ |
| **9. Délais de support P3** | Prise en charge < ___ Résolution < ___ |
| **10. RPO (perte données max)** | |
| **11. RTO (reprise max après incident majeur)** | |
| **12. KPI mesurés** | |
| **13. Rapport mensuel** | Fourni le : ___ Par : ___ |
| **14. Exclusions** | |

---

**Question de réflexion :** Vous avez défini un RTO de 4h pour ce service. Quelle infrastructure technique est nécessaire pour tenir cet objectif en cas de panne du serveur principal ? Citez au moins 2 éléments.

```
Élément 1 : ____________________________________________________________
Élément 2 : ____________________________________________________________
```

---

## MODULE 3 — PV de Mise en Service + Communication (15 min)

### Contexte

Vous venez de déployer le serveur de fichiers partagés de SimIO SARL. Tous les tests sont passés. Vous devez maintenant :
1. Rédiger le **PV de mise en service** (version simplifiée)
2. Rédiger la **communication** envoyée aux 80 employés

---

### 3.1 — PV de Mise en Service (simplifié)

| **Section** | **Contenu** |
|---|---|
| **Service** | Partage de fichiers SimIO SARL |
| **Technicien** | [Votre nom] |
| **Date de mise en service** | |
| **Version** | 1.0 |
| **Tests réalisés** | |
| Test 1 | Accès depuis un poste VLAN RH → Résultat : |
| Test 2 | Accès refusé pour un utilisateur sans droits → Résultat : |
| Test 3 | Sauvegarde nocturne → Résultat : |
| Test 4 | Restauration d'un fichier sauvegardé → Résultat : |
| Test 5 | Accès depuis un poste hors domaine → Résultat : |
| **Anomalies constatées** | |
| **Conditions de mise en service** | |
| **SLA applicable** | Référence : SLA-SimIO-Fichiers-v1.0 |
| **Signature technicien** | |
| **Signature responsable DSI** | |

---

### 3.2 — Communication aux Utilisateurs

Rédiger l'email envoyé aux 80 employés de SimIO SARL pour leur annoncer la mise en service du partage de fichiers. Respecter les bonnes pratiques du cours (pas de jargon, bénéfice clair, procédure simple, contact support).

```
DE      : dsi@siosarl.local
À       : tous@siosarl.local
OBJET   : _______________________________________________________________

Bonjour à toutes et tous,

________________________________________________________________________
________________________________________________________________________
________________________________________________________________________
________________________________________________________________________
________________________________________________________________________
________________________________________________________________________
________________________________________________________________________
________________________________________________________________________

Cordialement,
L'équipe DSI SimIO SARL
```

---

---

# 📄 ANNEXE 1 — MODÈLE DE PV DE MISE EN SERVICE

*Modèle complet — Réutilisable pour le Projet SimIO (S17-S18) et le portfolio E5*

---

```
╔══════════════════════════════════════════════════════════════════════╗
║           PROCÈS-VERBAL DE MISE EN SERVICE                          ║
║                  [NOM DU SERVICE]                                    ║
╠══════════════════════════════════════════════════════════════════════╣
║  N° document : PV-[SERVICE]-[DATE]    Version : 1.0                  ║
║  Date de rédaction : _______________  Technicien : ________________  ║
║  Environnement : ☐ Test  ☐ Préproduction  ☐ Production              ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  1. IDENTIFICATION DU SERVICE                                        ║
║  ─────────────────────────────────────────────────────────────────  ║
║  Nom du service : _______________________________________________    ║
║  Description    : _______________________________________________    ║
║  Version logicielle : ___________________________________________    ║
║  Serveur hébergeur : ____________________________________________    ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  2. PÉRIMÈTRE DE LA MISE EN SERVICE                                  ║
║  ─────────────────────────────────────────────────────────────────  ║
║  Inclus dans cette mise en service :                                 ║
║  ___________________________________________________________________  ║
║  EXCLU de cette mise en service (livraison ultérieure) :             ║
║  ___________________________________________________________________  ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  3. RÉSULTATS DES TESTS DE RECETTE                                   ║
║  ─────────────────────────────────────────────────────────────────  ║
║  N° │ Description du test        │ Résultat attendu │ Résultat │ ✅❌ ║
║  ────┼───────────────────────────┼──────────────────┼──────────┼──── ║
║   1  │                           │                  │          │     ║
║   2  │                           │                  │          │     ║
║   3  │                           │                  │          │     ║
║   4  │                           │                  │          │     ║
║   5  │                           │                  │          │     ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  4. ANOMALIES CONSTATÉES                                             ║
║  ─────────────────────────────────────────────────────────────────  ║
║  N° │ Description               │ Sévérité │ Statut   │ Responsable  ║
║  ────┼───────────────────────────┼──────────┼──────────┼────────────  ║
║      │                           │          │          │              ║
║      │                           │          │          │              ║
║  ☐ Aucune anomalie constatée                                         ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  5. CONDITIONS ET RÉSERVES DE MISE EN SERVICE                        ║
║  ─────────────────────────────────────────────────────────────────  ║
║  ☐ Mise en service sans réserve                                      ║
║  ☐ Mise en service avec réserve(s) :                                 ║
║  ___________________________________________________________________  ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  6. LIVRABLES ASSOCIÉS                                               ║
║  ─────────────────────────────────────────────────────────────────  ║
║  ☐ DAT (Dossier Architecture Technique)   Référence : ___________   ║
║  ☐ Procédures d'exploitation             Référence : ___________   ║
║  ☐ SLA                                   Référence : ___________   ║
║  ☐ Communication utilisateurs            Envoyée le : __________   ║
║  ☐ Supervision configurée                Outil : ________________  ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  7. VALIDATIONS                                                      ║
║  ─────────────────────────────────────────────────────────────────  ║
║  Technicien responsable    : __________________ Date : _________    ║
║  Responsable DSI           : __________________ Date : _________    ║
║  Représentant utilisateurs : __________________ Date : _________    ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

# 📄 ANNEXE 2 — TABLE DE RÉFÉRENCE DISPONIBILITÉ

*À conserver — Référence rapide pour tous les calculs futurs*

```
╔═══════════════════════════════════════════════════════════════════════╗
║              TABLE DE RÉFÉRENCE — DISPONIBILITÉ                      ║
╠════════════╦════════════════╦═══════════════════╦════════════════════╣
║  SLA (%)   ║ Indispo / an   ║ Indispo / mois    ║ Indispo / semaine  ║
╠════════════╬════════════════╬═══════════════════╬════════════════════╣
║  90,0%     ║ 876h 00min     ║ 73h 00min         ║ 16h 48min          ║
║  95,0%     ║ 438h 00min     ║ 36h 30min         ║ 8h 24min           ║
║  99,0%     ║ 87h 36min      ║ 7h 18min          ║ 1h 41min           ║
║  99,5%     ║ 43h 48min      ║ 3h 39min          ║ 50min 24s          ║
║  99,9%     ║ 8h 45min       ║ 43min 48s         ║ 10min 05s          ║
║  99,95%    ║ 4h 23min       ║ 21min 54s         ║ 5min 02s           ║
║  99,99%    ║ 52min 34s      ║ 4min 22s          ║ 1min 00s           ║
║  99,999%   ║ 5min 15s       ║ 26s               ║ 6s                 ║
╠════════════╩════════════════╩═══════════════════╩════════════════════╣
║  Base de calcul : 1 an = 8 760h / 1 mois = 730h / 1 semaine = 168h  ║
╚═══════════════════════════════════════════════════════════════════════╝
```

---

*Pack S7 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.2, B1.5, B1.6, B3.3*
*Clôture du Bloc 1 : Mise à disposition · SLA complet · Disponibilité · RTO/RPO · PV de mise en service*
