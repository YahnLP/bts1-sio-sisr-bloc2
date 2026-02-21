# Pack de Formation - Semaine 5 (S5) - BLOC 1
## 📦 OCS Inventory · Agent · Évaluation Diagnostique S1→S5

---

# 📋 FICHE ENSEIGNANT

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S5 — Année 1 |
| **Bloc** | Bloc 1 — Support et mise à disposition de services informatiques |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR — cinquième semaine |
| **Modalité** | Présentiel — salle TP (accès réseau, postes Windows/Linux) |
| **Prérequis** | S1→S4 : présentation BTS, gestion de parc, ITIL, gestion d'incidents |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B1.1** | Recenser et identifier les ressources numériques | Consolidation |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc | Acquisition |
| **B1.2** | Exploiter des référentiels, normes et standards | Consolidation |
| **B1.6** | Assurer le support des utilisateurs | Consolidation |

> 📌 **S5 a deux rôles distincts mais complémentaires.** La matinée introduit **OCS Inventory**, premier outil professionnel de gestion de parc automatisée — concrétisant la problématique de S2 ("comment inventorier 200 postes sans y passer 3 semaines ?"). L'après-midi est une **évaluation diagnostique** qui clôture le premier bloc de 5 séances et fournit à l'enseignant une cartographie précise des acquis et lacunes avant d'aborder les séances plus techniques du Bloc 2.

---

## Objectifs Pédagogiques

À l'issue de cette séance, l'apprenant sera capable de :

**OCS Inventory :**
- ✅ Expliquer l'architecture client/serveur d'OCS Inventory (agent → serveur → console web)
- ✅ Installer et configurer l'**agent OCS** sur un poste Windows
- ✅ Vérifier que le poste remonte ses informations dans la **console d'administration**
- ✅ Lire une fiche d'inventaire automatisée et la comparer à la fiche manuelle de S2
- ✅ Expliquer les **avantages et limites** de l'inventaire automatisé

**Évaluation diagnostique S1→S5 :**
- ✅ Mobiliser les compétences B1.1, B1.2, B1.3, B1.4, B1.6 dans un exercice intégré
- ✅ S'auto-évaluer honnêtement sur les 5 premières semaines
- ✅ Identifier ses lacunes prioritaires pour les séances suivantes

---

## ⭐ Spécificités Pédagogiques

### OCS Inventory : du Manuel à l'Automatisé

S2 a introduit l'inventaire de parc **manuel** (fiche technique remplie à la main). S5 répond à la question naturelle que les apprenants ont dû se poser : *"Et pour 500 postes ?"*

L'architecture OCS Inventory illustre parfaitement la logique client/serveur qui reviendra dans chaque séance technique du programme (Apache, AD DS, DHCP, DNS...). C'est une première exposition concrète à ce paradigme fondamental.

### L'Évaluation Diagnostique : Outil Pédagogique, Pas Sanction

L'évaluation S1→S5 est **diagnostique** au sens strict : elle sert à piloter l'enseignement, pas à classer les apprenants. Communiquer cela explicitement :
- Les résultats **ne comptent pas** dans la moyenne du trimestre
- Ils servent à l'enseignant pour **adapter les séances S6→S10**
- Ils servent à l'apprenant pour **construire son plan de révision personnel**
- Un résultat faible à ce stade est **normal et attendu** — 5 semaines sur 20 viennent de se terminer

**Ce que l'évaluation mesure par domaine :**

| **Domaine** | **Séances sources** | **Poids** |
|---|---|---|
| Gestion de parc (matériel, logiciel, licences) | S2 | 25% |
| ITIL — vocabulaire et concepts | S3 | 25% |
| Centre de services (niveaux, tickets, SLA) | S3-S4 | 25% |
| Diagnostic et résolution d'incidents | S4 | 25% |

### Organisation de la Journée

| **Bloc** | **Contenu** | **Durée** |
|---|---|---|
| **Matin** | OCS Inventory : cours + installation agent + vérification console | 2h |
| **Après-midi** | Évaluation diagnostique : QCM + mini-TP + auto-évaluation + bilan | 2h |

---

## Planning de Séance (4h)

| **Horaire** | **Durée** | **Phase** | **Contenu** |
|-------------|-----------|-----------|-------------|
| H+0:00 | 10 min | 🔄 Retour S4 | Bilan des 3 incidents — MTTR collectifs — fiches KB produites |
| H+0:10 | 30 min | 📖 Cours | OCS Inventory : architecture, agents, console, usages |
| H+0:40 | 50 min | 🖥️ TP OCS | Installation agent Windows + vérification console + comparaison S2 |
| H+1:30 | **10 min** | ☕ **PAUSE** | — |
| H+1:40 | 10 min | 📋 Briefing | Présentation de l'évaluation diagnostique — consignes |
| H+1:50 | 40 min | 📝 **QCM** | 30 questions — S1→S5 — individuel — sans support |
| H+2:30 | 30 min | 🔧 **Mini-TP** | Scénario intégré — résolution + ticket + fiche KB |
| H+3:00 | 30 min | ✅ Correction | Correction collective QCM + mini-TP |
| H+3:30 | 20 min | 📊 Bilan | Auto-évaluation + plan de révision personnalisé |
| H+3:50 | 10 min | 🔭 Projection | Annonce du programme S6→S10 (réseaux, Linux) |

---

## Conseils de Différenciation

### Profil avancé — TP OCS

- Installer également l'**agent OCS Linux** (Debian) sur une VM
- Explorer l'**API REST OCS** pour récupérer l'inventaire d'un poste en JSON : `curl http://[serveur]/ocsapi/v1/computers`
- Comparer OCS Inventory avec **GLPI + FusionInventory** — quelles sont les différences ?
- Tenter de **déployer l'agent à distance** via PowerShell (PSExec ou Invoke-Command)

### Profil débutant — Évaluation

- Fournir une **fiche aide-mémoire** des vocabulaires clés S1-S5 (autoriser pendant le mini-TP uniquement, pas pendant le QCM)
- Lors de la correction, s'assurer que chaque question ratée est comprise, pas juste corrigée
- Proposer un **exercice de remédiation** à faire à la maison pour les lacunes identifiées

---

## Préparation Technique OCS Inventory

> ⚠️ **À préparer avant la séance** si l'environnement réseau le permet :

**Option A — Serveur OCS déjà déployé en salle :**
- Serveur OCS Inventory NG installé sur une VM (voir documentation officielle : ocsinventory-ng.org)
- Accessible depuis les postes de TP sur le réseau local
- URL console : `http://[IP_serveur]/ocsreports`

**Option B — Démonstration enseignant + agent seul :**
- L'enseignant montre la console OCS sur son poste (vidéoprojecteur)
- Les apprenants installent uniquement l'agent et pointent vers le serveur de l'enseignant
- Vérification collective sur la console projetée

**Option C — OCSInventory en cloud / demo :**
- Utiliser une instance de démonstration si disponible
- Se concentrer sur l'installation de l'agent et la lecture des logs locaux

---

## Lien avec le Référentiel Qualiopi

- ✅ Évaluation diagnostique formalisée — identification des acquis et lacunes
- ✅ Retour individualisé sur les résultats
- ✅ Plan de révision personnalisé co-construit avec l'apprenant
- ✅ Outil professionnel réel (OCS Inventory est utilisé dans les entreprises)

---

---

# 📚 FICHE DE COURS ÉLÈVE
## "OCS Inventory — Gestion de Parc Automatisée"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 5*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.1** | Recenser et identifier les ressources numériques |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc |

---

## PARTIE I — Pourquoi Automatiser l'Inventaire ?

En S2, vous avez rempli manuellement la fiche technique d'un seul poste — cela a pris 30 à 45 minutes. Projetons cette expérience à l'échelle :

| **Taille du parc** | **Inventaire manuel** | **Inventaire automatisé** |
|---|---|---|
| 1 poste | 45 min | 2 min (installation agent) |
| 50 postes | 37h30 (1 semaine) | 2h (déploiement agent en masse) |
| 200 postes | 150h (1 mois) | 4h (déploiement GPO ou script) |
| 1 000 postes | — (irréaliste) | ½ journée |

**Trois problèmes supplémentaires de l'inventaire manuel :**

1. **L'information vieillit dès qu'elle est écrite.** Une mise à jour Windows, un ajout de RAM, un changement de disque — la fiche manuelle est déjà obsolète.
2. **L'inventaire n'est jamais exhaustif.** On oublie des postes, des imprimantes réseau, des équipements dans des armoires.
3. **Aucune alerte sur les changements.** Si quelqu'un installe un logiciel non autorisé ou retire une barrette de RAM, on ne le sait pas.

La **gestion de parc automatisée** résout ces trois problèmes : les agents remontent les informations périodiquement, l'inventaire se met à jour sans intervention humaine, et les modifications sont traçables.

---

## PARTIE II — OCS Inventory NG

### II.A. Présentation

**OCS Inventory NG** (Open Computer and Software Inventory Next Generation) est un logiciel **open source** de gestion d'inventaire de parc informatique. Il est utilisé par des milliers d'organisations dans le monde, particulièrement en France où il est très répandu dans les collectivités et PME.

| **Paramètre** | **Valeur** |
|---|---|
| **Licence** | GPL v2 (open source — gratuit) |
| **Site officiel** | ocsinventory-ng.org |
| **Éditeur communautaire** | OCS Inventory Team |
| **Systèmes supportés (agent)** | Windows, Linux, macOS, Android, AIX, Solaris |
| **Technologies serveur** | Apache + PHP + MySQL/MariaDB |
| **Intégration** | GLPI (via plugin FusionInventory) |

---

### II.B. Architecture Client/Serveur

```
   ┌─────────────────────────────────────────────────────────────────┐
   │                    ARCHITECTURE OCS INVENTORY                    │
   │                                                                 │
   │   POSTES DU PARC                  SERVEUR OCS                  │
   │   ─────────────                  ───────────                   │
   │                                                                 │
   │  PC Windows ──────── HTTPS ──────►┌─────────────────┐          │
   │  PC Linux   ──────── HTTPS ──────►│  Serveur Apache │          │
   │  Mac        ──────── HTTPS ──────►│  PHP            │          │
   │  Laptop     ──────── HTTPS ──────►│  MySQL/MariaDB  │          │
   │                                   └────────┬────────┘          │
   │   ↑                                        │                   │
   │   Agent OCS                                ▼                   │
   │   installé sur                    ┌─────────────────┐          │
   │   chaque poste                    │  Console Web    │          │
   │                                   │  ocsreports     │          │
   │                                   │  (navigateur)   │          │
   │                                   └─────────────────┘          │
   │                                          ↑                     │
   │                                   Admin DSI                    │
   └─────────────────────────────────────────────────────────────────┘
```

*Légende : Architecture OCS Inventory. L'agent installé sur chaque poste collecte les informations matérielles et logicielles, puis les envoie au serveur OCS via HTTPS. Le serveur stocke les données dans MySQL. L'administrateur accède aux inventaires via la console web `ocsreports`. Le protocole HTTPS garantit la confidentialité des données de parc en transit.*

---

### II.C. Fonctionnement de l'Agent

L'**agent OCS** est un service (daemon) qui s'exécute en arrière-plan sur chaque poste. Ses actions :

```
   DÉMARRAGE DU POSTE
         │
         ▼
   Agent OCS démarre
   (service Windows ou cron Linux)
         │
         ▼
   Collecte des informations :
   • Matériel (CPU, RAM, disques, cartes réseau...)
   • OS (version, patches installés, clé de licence)
   • Logiciels (liste complète avec versions)
   • Réseau (IP, MAC, VLAN si disponible)
   • Périphériques connectés
         │
         ▼
   Comparaison avec le dernier inventaire envoyé
   (changements uniquement si "ipdiscover" ou delta)
         │
         ▼
   Envoi au serveur OCS via HTTPS (XML compressé)
   URL : http(s)://[serveur]/ocsinventory
         │
         ▼
   Serveur stocke en base de données
   Console web mise à jour
```

---

### II.D. Ce qu'OCS Inventory Collecte

| **Catégorie** | **Informations collectées** |
|---|---|
| **Matériel** | CPU (modèle, fréquence, cœurs), RAM (capacité, slots), Disques (modèle, taille, type), Carte mère, BIOS (version, date), Carte réseau (MAC, IP, type) |
| **Système** | OS (nom, version, build, langue), Clé de licence OS, Domaine/groupe de travail, Nom du poste, Uptime |
| **Logiciels** | Liste complète avec éditeur, version, date d'installation, chemin |
| **Réseau** | Toutes les interfaces (IP, masque, MAC, VLAN) |
| **Périphériques** | Moniteurs (marque, résolution), Imprimantes, Ports (USB, PCI...) |
| **Sécurité** | Antivirus détecté, pare-feu, mises à jour manquantes (optionnel) |

> 📌 **Point sécurité :** OCS Inventory collecte des informations potentiellement sensibles (configuration du réseau, logiciels installés, parfois clés de licence). Le serveur OCS doit être sécurisé (HTTPS, authentification forte, accès restreint) et les données traitées conformément au RGPD.

---

### II.E. Avantages et Limites

| **Avantages** | **Limites** |
|---|---|
| ✅ Inventaire automatique et périodique | ❌ Nécessite un agent sur chaque poste |
| ✅ Détection des changements | ❌ Agent = charge CPU/RAM (légère) |
| ✅ 100% open source et gratuit | ❌ Interface web vieillissante |
| ✅ Multi-OS (Windows, Linux, Mac) | ❌ Pas de gestion native des licences avancée |
| ✅ Intégration GLPI (via plugin) | ❌ Nécessite un serveur dédié |
| ✅ API REST disponible | ❌ Configuration initiale complexe |
| ✅ Très répandu en France | ❌ Alternatives plus modernes existent (Lansweeper, Rudder) |

---

### II.F. OCS et GLPI — L'Écosystème Complet

OCS Inventory et GLPI fonctionnent souvent ensemble dans les organisations françaises :

```
   OCS INVENTORY                      GLPI
   ─────────────                      ────
   Collecte automatique   ──────────► Reçoit l'inventaire
   des données matérielles            via plugin FusionInventory
   et logicielles                     ou import natif

                                       + Gestion des tickets
                                       + CMDB relationnelle
                                       + Gestion des licences
                                       + Base de connaissances
                                       + Planification
                                       + Rapports SLA
```

> 💡 **En entreprise :** On dit souvent "on est sous GLPI + OCS". GLPI est l'outil de gestion (tickets, actifs, CMDB), OCS est le collecteur automatique qui l'alimente. L'un sans l'autre est moins efficace.

---

### II.G. Commandes de l'Agent Windows

```cmd
:: Forcer un inventaire immédiat (lancer depuis le répertoire d'installation)
"C:\Program Files\OCS Inventory Agent\OCSInventory.exe" /np /server:[IP_SERVEUR]

:: Forcer un inventaire avec logs détaillés
"C:\Program Files\OCS Inventory Agent\OCSInventory.exe" /np /server:[IP_SERVEUR] /debug /logfile:C:\Temp\ocs_debug.log

:: Vérifier le service Windows OCS
sc query OCS_AGENT
Get-Service -Name "OCS_AGENT"

:: Voir les logs de l'agent
type "C:\ProgramData\OCS Inventory Agent\OCSInventory.log"
```

---

### II.H. Comparaison des Outils de Gestion de Parc

| **Outil** | **Type** | **Inventaire Auto** | **Tickets** | **CMDB** | **Coût** |
|---|---|---|---|---|---|
| **OCS Inventory** | Open source | ✅ (agent) | ❌ | ❌ | Gratuit |
| **GLPI seul** | Open source | ❌ (manuel) | ✅ | ✅ | Gratuit |
| **GLPI + OCS** | Open source | ✅ | ✅ | ✅ | Gratuit |
| **Lansweeper** | Freemium | ✅ (agentless) | ❌ | Limité | Free/<100 |
| **SCCM/Intune** | Microsoft | ✅ (agent) | ❌ | ✅ | Inclus M365 |
| **ServiceNow** | SaaS | ✅ | ✅ | ✅ | Très élevé |

---

## III. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Agent OCS** | Logiciel installé sur chaque poste qui collecte et envoie les données au serveur |
| **Serveur OCS** | Serveur central qui reçoit, stocke et expose les inventaires |
| **ocsreports** | Interface web d'administration d'OCS Inventory |
| **XML** | Format de données utilisé par l'agent pour envoyer l'inventaire |
| **ipdiscover** | Fonctionnalité OCS qui scanne le réseau pour détecter des équipements non inventoriés |
| **FusionInventory** | Plugin GLPI permettant l'intégration avec OCS Inventory |
| **Inventaire delta** | Envoi uniquement des modifications depuis le dernier inventaire (optimisation réseau) |
| **Agentless** | Inventaire sans agent — utilise des protocoles réseau (SNMP, WMI) à distance |
| **SNMP** | Protocole permettant l'inventaire à distance des équipements réseau |
| **WMI** | Windows Management Instrumentation — interface Windows pour l'administration distante |

---

---

# 🖥️ FICHE TP — DÉCOUVERTE OCS INVENTORY

*Durée : 50 minutes — En binôme*

---

## Étape 1 — Connexion à la Console OCS (5 min)

Ouvrir un navigateur et accéder à la console OCS :

```
URL : http://[IP_SERVEUR_OCS]/ocsreports
Login : admin
Mot de passe : admin (à changer en production !)
```

> ⚠️ **Sécurité :** En production, le mot de passe `admin/admin` est une faille critique. Tout serveur OCS accessible depuis Internet avec ces identifiants par défaut sera compromis en quelques heures.

Explorer l'interface 5 minutes :

| **Section** | **Ce que vous trouvez** |
|---|---|
| Inventaire → Tous les ordinateurs | |
| Inventaire → Softwares | |
| Configuration → Générale | |
| Rapports | |

---

## Étape 2 — Téléchargement et Installation de l'Agent Windows (20 min)

### 2.1 — Téléchargement

```
Site officiel : https://github.com/OCSInventory-NG/WindowsAgent/releases
Fichier à télécharger : OCS-NG-Windows-Agent-Setup-[version].exe
```

Si le réseau ne le permet pas, l'enseignant fournit le fichier sur le partage réseau de TP.

### 2.2 — Installation

Lancer l'installeur en **tant qu'administrateur** et noter les paramètres configurés :

| **Paramètre d'installation** | **Valeur saisie** |
|---|---|
| Adresse du serveur OCS | |
| Port (par défaut 80 ou 443) | |
| TAG (étiquette de groupe) | `TP-BTS-SIO` |
| Fréquence d'inventaire | |
| Service Windows créé ? | ☐ Oui / ☐ Non |

### 2.3 — Premier Inventaire Forcé

Après installation, forcer immédiatement un inventaire :

```cmd
:: Ouvrir CMD en administrateur
cd "C:\Program Files\OCS Inventory Agent"
OCSInventory.exe /np /server:[IP_SERVEUR] /debug /logfile:C:\Temp\ocs.log
```

Observer la sortie du terminal et noter :

| **Information dans les logs** | **Valeur** |
|---|---|
| Connexion au serveur réussie ? | ☐ Oui / ☐ Non |
| Nombre d'éléments matériels envoyés | |
| Nombre de logiciels détectés | |
| Durée de l'inventaire | s |
| Erreur éventuelle | |

---

## Étape 3 — Vérification dans la Console (10 min)

Retourner dans la console OCS et vérifier que le poste est apparu :

```
Inventaire → Tous les ordinateurs → Chercher votre poste (nom ou IP)
```

Comparer l'inventaire OCS avec la fiche manuelle réalisée en S2 :

| **Information** | **Fiche manuelle S2** | **OCS Inventory** | **Identique ?** |
|---|---|---|---|
| Nom du poste | | | ☐ Oui / ☐ Non |
| CPU | | | ☐ Oui / ☐ Non |
| RAM totale | | | ☐ Oui / ☐ Non |
| OS + version | | | ☐ Oui / ☐ Non |
| Adresse IP | | | ☐ Oui / ☐ Non |
| Adresse MAC | | | ☐ Oui / ☐ Non |
| Nb logiciels | | (OCS compte tout) | — |
| Numéro de série | | | ☐ Oui / ☐ Non |

**Observations :**

```
Différences constatées : ____________________________________________
__________________________________________________________________
```

---

## Étape 4 — Questions de Réflexion (15 min)

**Q1.** OCS Inventory a détecté _____ logiciels sur votre poste. Lors de la fiche manuelle S2, vous en aviez listé _____. Comment expliquez-vous la différence ?

```
Réponse : ___________________________________________________________
```

**Q2.** Le service Windows OCS_AGENT est configuré pour démarrer automatiquement. Cela signifie que l'inventaire sera mis à jour à chaque démarrage du PC. Citez **2 situations** où cette mise à jour automatique est particulièrement utile pour la DSI.

```
Situation 1 : _______________________________________________________
Situation 2 : _______________________________________________________
```

**Q3.** Un utilisateur découvre qu'OCS Inventory surveille les logiciels installés sur son PC et s'y oppose au nom de sa vie privée. Que lui répondez-vous ? Comment la DSI doit-elle encadrer cet outil ?

```
Réponse : ___________________________________________________________
__________________________________________________________________
```

**Éléments de réponse Q3 :** L'inventaire de parc d'entreprise ne surveille pas l'activité personnelle — il recense uniquement les logiciels installés et la configuration matérielle pour des raisons légitimes (licences, sécurité, conformité). Il doit être mentionné dans la charte informatique de l'entreprise que les utilisateurs signent. En France, le RGPD impose une information préalable des personnes concernées par tout traitement de leurs données — un inventaire de parc sur un équipement professionnel est généralement couvert par la charte SI.

**Q4.** Votre entreprise d'alternance utilise-t-elle un outil d'inventaire de parc ? Lequel ? Comment les données sont-elles exploitées ?

```
Réponse : ___________________________________________________________
__________________________________________________________________
```

---

---

# 📝 ÉVALUATION DIAGNOSTIQUE S1→S5

*Document distribué en début d'après-midi*

---

## Consignes

**Durée totale :** 70 minutes (40 min QCM + 30 min mini-TP)

**Supports autorisés :** Aucun pendant le QCM. La fiche de cours S4 (liste des commandes) est autorisée pendant le mini-TP uniquement.

**Ce n'est pas une évaluation notée.** Son unique objectif est de vous aider à identifier vos points forts et vos lacunes pour les 15 semaines à venir. Répondez honnêtement — une réponse "je ne sais pas" est plus utile qu'une réponse inventée.

---

## PARTIE 1 — QCM (40 points, 30 questions)

*Durée : 40 minutes*

---

### DOMAINE A — Gestion de Parc et Documentation (S2) — 10 points

**A1.** La gestion de parc informatique désigne :
- A. La gestion des parkings des employés IT
- B. L'ensemble des activités de recensement et de suivi des équipements et logiciels
- C. La maintenance uniquement des serveurs
- D. La gestion des abonnements cloud

**A2.** Lequel de ces éléments N'EST PAS une composante d'un inventaire de parc complet ?
- A. Inventaire matériel
- B. Inventaire logiciel
- C. Inventaire des licences
- D. Inventaire des courriels des utilisateurs

**A3.** Une licence OEM est :
- A. Transférable d'un PC à un autre
- B. Liée au matériel d'origine — non transférable
- C. Un abonnement mensuel
- D. Une licence open source

**A4.** Pour afficher l'adresse IP, le masque, la passerelle et l'adresse MAC d'un PC Windows, on utilise :
- A. `netstat -a`
- B. `ping localhost`
- C. `ipconfig /all`
- D. `tracert`

**A5.** L'outil Windows `msinfo32` permet de :
- A. Formater le disque dur
- B. Obtenir des informations système complètes (CPU, RAM, OS...)
- C. Configurer le réseau WiFi
- D. Créer des utilisateurs Windows

**A6.** Qu'est-ce qu'une licence SaaS ?
- A. Un logiciel installé une fois, à vie
- B. Un logiciel accessible via abonnement par Internet
- C. Un logiciel open source
- D. Un logiciel fourni avec le matériel

**A7.** La commande Linux pour obtenir un inventaire matériel complet est :
- A. `cat /proc/hardware`
- B. `lshw -short`
- C. `hardware-list`
- D. `sysinfo`

**A8.** Dans un inventaire, si vous ne pouvez pas trouver une information, vous devez :
- A. Laisser le champ vide
- B. Inventer une valeur plausible
- C. Indiquer "Inconnu" ou "À vérifier"
- D. Supprimer le champ

**A9.** Le cycle de vie d'un équipement commence à quelle étape ?
- A. Déploiement
- B. Acquisition
- C. Maintenance
- D. Fin de vie

**A10.** La commande `wmic bios get serialnumber` retourne :
- A. La version du BIOS
- B. L'adresse MAC de la carte réseau
- C. Le numéro de série de l'équipement
- D. La clé de licence Windows

---

### DOMAINE B — ITIL et Vocabulaire (S3) — 10 points

**B1.** ITIL est :
- A. Un système d'exploitation
- B. Un référentiel de bonnes pratiques pour la gestion des services IT
- C. Un protocole réseau
- D. Un outil de gestion de parc

**B2.** Un utilisateur signale que son imprimante ne répond plus. C'est :
- A. Un problème
- B. Un changement
- C. Un incident
- D. Une demande de service

**B3.** Un technicien enquête pour comprendre pourquoi 6 PCs ont planté avec le même écran bleu en une semaine. Il gère :
- A. Un incident
- B. Un problème
- C. Un changement
- D. Une escalade

**B4.** "Installer un logiciel métier sur 30 postes" est :
- A. Un incident
- B. Un problème
- C. Un changement
- D. Une demande de service

**B5.** Le MTTR désigne :
- A. Maximum Time To Respond
- B. Mean Time To Repair
- C. Monthly Time To Review
- D. Minimum Ticket Resolution Time

**B6.** Un SLA (Service Level Agreement) définit :
- A. La liste des logiciels autorisés
- B. Le niveau de service attendu et les délais de résolution
- C. Le budget annuel de la DSI
- D. Les mots de passe des serveurs

**B7.** Dans la matrice de priorité ITIL, P1 désigne :
- A. Un incident mineur à faible urgence
- B. Un incident critique à impact total
- C. Une demande de service prioritaire
- D. Un problème récurrent

**B8.** La pratique ITIL qui gère l'inventaire des équipements s'appelle :
- A. Gestion des incidents
- B. Gestion des niveaux de service
- C. Gestion des actifs IT
- D. Centre de services

**B9.** Le "FCR" (First Contact Resolution) mesure :
- A. Le temps moyen entre deux pannes
- B. Le pourcentage de tickets résolus dès le premier contact
- C. Le coût moyen par incident
- D. La satisfaction des utilisateurs

**B10.** Les 4 dimensions d'un service ITIL 4 sont :
- A. Coût, Qualité, Délai, Périmètre
- B. Organisations/Personnes, Informations/Technologie, Partenaires/Fournisseurs, Flux/Processus
- C. N1, N2, N3, DSI
- D. Incident, Problème, Changement, Demande

---

### DOMAINE C — Centre de Services et Niveaux de Support (S3-S4) — 10 points

**C1.** Le centre de services (Service Desk) est :
- A. Le département qui achète les équipements
- B. Le point de contact unique entre les utilisateurs et la DSI
- C. Le service qui gère les serveurs
- D. L'équipe qui développe les applications

**C2.** Un technicien N1 doit escalader vers N2 dans quelle situation ?
- A. Dès que l'incident dure plus de 5 minutes
- B. Quand l'incident dépasse ses compétences ou les délais SLA
- C. Uniquement si le manager le demande
- D. Jamais — le N1 doit tout traiter

**C3.** Quel profil correspond au technicien N3 ?
- A. Technicien helpdesk junior, résout les incidents simples
- B. Spécialiste expérimenté, traite les incidents complexes
- C. Expert ou éditeur, traite les incidents inconnus et non reproductibles
- D. Manager de l'équipe IT

**C4.** Un bon ticket d'incident doit contenir en priorité :
- A. Uniquement le nom de l'utilisateur
- B. Qui, quoi, quand, impact, actions déjà tentées
- C. Le numéro de téléphone de l'éditeur du logiciel
- D. La liste de tous les logiciels installés

**C5.** "1 ticket = 1 sujet" signifie :
- A. Un ticket ne peut pas être modifié
- B. On ne peut pas mélanger plusieurs problèmes dans un seul ticket
- C. Un seul technicien peut traiter chaque ticket
- D. Le ticket doit tenir sur une seule page

**C6.** Le "backlog" dans un outil ITSM représente :
- A. Les tickets clôturés du mois
- B. La file d'attente des tickets ouverts non résolus
- C. Les mises à jour logicielles en attente
- D. L'historique des incidents résolus

**C7.** Laquelle de ces actions N'est PAS une mission du centre de services N1 ?
- A. Réception et enregistrement des incidents
- B. Résolution des incidents simples et procédurés
- C. Modification de la configuration des serveurs de production
- D. Escalade vers N2 si dépassement de compétences

**C8.** Un utilisateur appelle pour trois problèmes différents en même temps. Que fait le technicien N1 ?
- A. Traite les trois en même temps dans un seul ticket
- B. Choisit le plus urgent et ignore les deux autres
- C. Ouvre trois tickets distincts, un par sujet
- D. Escalade immédiatement vers N2

**C9.** GLPI est utilisé principalement pour :
- A. Configurer les switches réseau
- B. Gérer les tickets et l'inventaire de parc (ITSM)
- C. Développer des applications web
- D. Surveiller les performances des serveurs

**C10.** La base de connaissances (Knowledge Base) sert à :
- A. Stocker les mots de passe des serveurs
- B. Référencer les solutions aux incidents résolus pour éviter de repartir de zéro
- C. Lister les employés de la DSI
- D. Archiver les anciennes versions des logiciels

---

### DOMAINE D — Diagnostic et Résolution d'Incidents (S4) — 10 points

**D1.** La méthode de diagnostic "du général au particulier" appliquée au modèle OSI consiste à :
- A. Commencer par la couche Application (logiciel) et descendre
- B. Commencer par la couche Physique (câble, alimentation) et monter
- C. Vérifier toutes les couches en même temps
- D. Toujours commencer par redémarrer

**D2.** Avant tout diagnostic, quelle est la première question à poser à l'utilisateur ?
- A. "Avez-vous essayé de redémarrer ?"
- B. "Quel est votre mot de passe ?"
- C. "Est-ce que ça a déjà fonctionné ? Qu'est-ce qui a changé récemment ?"
- D. "Quand avez-vous acheté ce PC ?"

**D3.** Pour une imprimante réseau qui ne répond plus, quelle est la première vérification logique ?
- A. Désinstaller et réinstaller le pilote
- B. Vérifier que l'imprimante est allumée et le câble réseau branché
- C. Appeler l'éditeur du pilote
- D. Remplacer l'imprimante

**D4.** Un utilisateur obtient "Accès refusé" sur un dossier réseau. Après vérification, son groupe a les droits "Contrôle total" en droits de partage, mais "Lecture" en droits NTFS. Quelle permission s'applique ?
- A. Contrôle total (le plus permissif l'emporte)
- B. Lecture (le plus restrictif l'emporte)
- C. Aucune permission (annulation)
- D. Cela dépend du serveur

**D5.** Le Gestionnaire des tâches Windows affiche 98% d'utilisation CPU en permanence. Quelle est votre première action ?
- A. Redémarrer le PC immédiatement
- B. Identifier quel processus consomme le plus de CPU
- C. Augmenter la fréquence du processeur
- D. Désactiver l'antivirus

**D6.** La commande pour vider la file d'attente d'impression sous Windows est :
- A. `net stop spooler` puis supprimer les fichiers dans `\spool\PRINTERS`
- B. `flush printer queue`
- C. `clear-printqueue`
- D. `wmic printer reset`

**D7.** Pourquoi ne faut-il faire qu'une seule action à la fois lors d'un diagnostic ?
- A. Pour aller plus vite
- B. Pour éviter de saturer le CPU
- C. Pour savoir quelle action a résolu le problème et pouvoir le documenter
- D. C'est une règle arbitraire sans justification

**D8.** `icacls C:\Partages\RH` affiche les droits NTFS du dossier RH. Pour accorder les droits de modification au groupe GRP_RH, la commande est :
- A. `icacls C:\Partages\RH /add GRP_RH:M`
- B. `icacls C:\Partages\RH /grant GRP_RH:(M)`
- C. `chmod 660 C:\Partages\RH`
- D. `setacl C:\Partages\RH GRP_RH modify`

**D9.** Après avoir résolu un incident, quelle est l'étape OBLIGATOIRE avant de clôturer le ticket ?
- A. Envoyer un rapport au DSI
- B. Faire valider la résolution par l'utilisateur
- C. Calculer le coût de l'intervention
- D. Archiver le poste de l'utilisateur

**D10.** Une fiche de base de connaissances doit contenir au minimum :
- A. Le nom de l'utilisateur concerné
- B. Symptômes + cause + solution + vérification
- C. Le prix de la pièce remplacée
- D. Le nombre d'heures passées sur l'incident

---

**CORRIGÉ QCM :**
A1-B, A2-D, A3-B, A4-C, A5-B, A6-B, A7-B, A8-C, A9-B, A10-C
B1-B, B2-C, B3-B, B4-C, B5-B, B6-B, B7-B, B8-C, B9-B, B10-B
C1-B, C2-B, C3-C, C4-B, C5-B, C6-B, C7-C, C8-C, C9-B, C10-B
D1-B, D2-C, D3-B, D4-B, D5-B, D6-A, D7-C, D8-B, D9-B, D10-B

---

## PARTIE 2 — MINI-TP INTÉGRÉ (30 points)

*Durée : 30 minutes — La fiche commandes S4 est autorisée*

---

### Contexte

Vous êtes technicien N1 au centre de services de SimIO SARL. Il est 10h15, un lundi. Vous recevez le message suivant dans votre outil ITSM :

---

> **[TICKET AUTO-GÉNÉRÉ — PORTAIL UTILISATEUR]**
>
> *"Bonjour, j'essaie d'accéder au dossier 'Direction_Confidentiel' sur le serveur depuis vendredi dernier et j'ai un message d'accès refusé. Avant vendredi ça marchait. J'ai des documents urgents à consulter pour préparer le CODIR de demain matin. Merci de m'aider rapidement."*
>
> **Utilisateur :** Catherine Lefebvre
> **Service :** Direction Générale
> **Équipement déclaré :** LAPTOP-DG-001
> **Heure de soumission :** 10h08

---

### Travail Demandé

**Question 1 — Compléter le ticket (10 pts)**

Le ticket auto-généré est incomplet. Compléter les champs manquants :

| **Champ** | **Valeur** |
|---|---|
| **Titre professionnel du ticket** | |
| **Priorité (P1→P4) et justification** | |
| **Niveau initial (N1/N2) et justification** | |
| **Informations manquantes à collecter auprès de l'utilisateur** | 1. 2. 3. |
| **SLA applicable (délai de prise en charge + délai résolution)** | |

**Question 2 — Plan de diagnostic (10 pts)**

Décrire votre démarche de diagnostic en 6 étapes ordonnées. Pour chaque étape, préciser l'action et l'outil/commande utilisé :

| **Étape** | **Action** | **Outil / Commande** |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |
| 6 | | |

**Question 3 — Fiche KB (10 pts)**

Après résolution (hypothèse : la cause était la suppression accidentelle du groupe `GRP_Direction` des droits NTFS du dossier lors d'une mise à jour de sécurité vendredi), rédiger la fiche de base de connaissances :

| **Section** | **Contenu** |
|---|---|
| **Titre KB** | |
| **Symptômes** | |
| **Cause identifiée** | |
| **Solution (étapes numérotées)** | 1. 2. 3. 4. |
| **Commande de vérification** | |
| **Escalade si** | |
| **Mots-clés** | |

---

### Correction du Mini-TP

**Q1 — Éléments attendus :**
- Titre : "Accès refusé — dossier Direction_Confidentiel — C. Lefebvre — DG — CODIR demain" (titre explicite, urgence visible)
- Priorité : **P2** — 1 utilisateur (impact moyen) + CODIR demain (urgence élevée) → P2. Justification attendue
- Niveau : N1 → escalade probable N2 (droits NTFS sur serveur → droits serveur nécessaires)
- Informations manquantes : contact direct, n° de poste, chemin exact du dossier, message d'erreur exact, qui d'autre est impacté
- SLA P2 : prise en charge < 1h / résolution < 8h (selon politique standard)

**Q2 — Plan de diagnostic attendu :**
1. Rappeler l'utilisateur pour collecter les infos manquantes et obtenir le message d'erreur exact
2. Vérifier la connectivité réseau (ping du serveur de fichiers)
3. Vérifier que le compte est actif et non verrouillé dans AD
4. Vérifier les droits de partage sur `Direction_Confidentiel`
5. Vérifier les droits NTFS — permissions effectives pour C. Lefebvre (onglet Sécurité → Avancé → Permissions effectives)
6. Si droits manquants : ajouter `GRP_Direction` avec droits Modification en NTFS → tester avec l'utilisateur

**Q3 — Fiche KB attendue :**
- Titre : "Accès refusé sur dossier partagé suite à modification des droits NTFS"
- Symptômes : "Message 'Accès refusé' sur un dossier réseau qui fonctionnait auparavant"
- Cause : "Suppression accidentelle d'un groupe dans les droits NTFS du dossier (suite à mise à jour, erreur de manipulation ou réinitialisation des droits hérités)"
- Solution : 1. Vérifier permissions effectives (Sécurité → Avancé), 2. Identifier le groupe/utilisateur manquant, 3. `icacls [chemin] /grant [Groupe]:(M)` ou via interface graphique, 4. Tester l'accès avec l'utilisateur
- Vérification : `icacls [chemin]` ou ouvrir le dossier avec le compte de l'utilisateur
- Escalade si : permissions effectives correctes mais accès toujours refusé → problème de profil itinérant ou GPO → N2

---

---

# 📊 GRILLE D'AUTO-ÉVALUATION S1→S5

*À compléter après la correction — Document conservé dans le portfolio*

---

## Résultats par Domaine

| **Domaine** | **Score** | **Seuil recommandé** | **Statut** |
|---|---|---|---|
| A — Gestion de parc (S2) | /10 | ≥ 7 | ☐ Acquis ☐ À retravailler |
| B — ITIL vocabulaire (S3) | /10 | ≥ 7 | ☐ Acquis ☐ À retravailler |
| C — Centre de services (S3-S4) | /10 | ≥ 7 | ☐ Acquis ☐ À retravailler |
| D — Diagnostic incidents (S4) | /10 | ≥ 7 | ☐ Acquis ☐ À retravailler |
| **Mini-TP intégré** | /30 | ≥ 20 | ☐ Acquis ☐ À retravailler |
| **TOTAL** | /70 | ≥ 50 | |

---

## Analyse Personnelle

**Ma question la plus difficile et pourquoi :**
```
___________________________________________________________________
```

**Le concept que je maîtrise le mieux :**
```
___________________________________________________________________
```

**Les 2 notions que je dois retravailler avant les prochaines séances :**
```
1. ________________________________________________________________
2. ________________________________________________________________
```

---

## Plan de Révision Personnalisé

| **Notion à retravailler** | **Action concrète** | **Ressource** | **Délai** |
|---|---|---|---|
| | | Fiche cours S___ | Avant S6 |
| | | Fiche cours S___ | Avant S6 |
| | | | Avant S7 |

---

## Ce Que Je Retiens de OCS Inventory

```
En 3 phrases, expliquez à quelqu'un qui n'a pas suivi S5 ce qu'est OCS
Inventory et à quoi ça sert :

___________________________________________________________________
___________________________________________________________________
___________________________________________________________________
```

---

## Note de l'Enseignant

*Rempli lors de l'entretien rapide de bilan*

```
Points forts identifiés : ___________________________________________
Axe de progression prioritaire : ____________________________________
Adaptation pédagogique prévue en S6 : ______________________________
```

---

*Pack S5 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.1, B1.2, B1.4, B1.6*
*OCS Inventory — Installation Agent — Évaluation diagnostique S1→S5*
*Clôture du premier bloc pédagogique — Passerelle vers Bloc 2 (réseaux et systèmes)*
