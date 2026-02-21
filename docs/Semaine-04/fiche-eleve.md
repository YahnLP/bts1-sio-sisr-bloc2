# Semaine 4 (S4) - BLOC 1
## 🎫 Gestion des Incidents · Cycle de Vie du Ticket · TP Résolution

---

# 📚 FICHE DE COURS ÉLÈVE
## "Cycle de Vie d'un Incident · Méthode de Diagnostic"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 4*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.3** | Exploiter des outils de support (tickets, base de connaissances) |
| **B1.6** | Assurer le support des utilisateurs |
| **B3.3** | Documenter et communiquer professionnellement |

---

## PARTIE I — Cycle de Vie Complet d'un Incident

### I.A. Les 7 Étapes

```
   ① SIGNALEMENT          ② ENREGISTREMENT        ③ QUALIFICATION
   ─────────────          ─────────────────        ───────────────
   Utilisateur            Ticket ouvert            Catégorie
   contacte le      →     dans l'outil       →     Priorité (P1-P4)
   support                ITSM                     Impact / Urgence
   (tel, mail,            Toutes les               Niveau attribué
   portail)               infos collectées         (N1/N2/N3)

        │
        ▼
   ④ DIAGNOSTIC           ⑤ RÉSOLUTION            ⑥ VALIDATION
   ────────────           ──────────────           ────────────
   Méthode                Solution                 Utilisateur
   structurée       →     appliquée          →     confirme
   Hypothèses             Testée                   que le service
   testées 1 à 1          Documentée               est restauré

        │
        ▼
   ⑦ CLÔTURE
   ─────────
   Ticket fermé
   MTTR calculé
   Satisfaction recueillie
   Alimentation base
   de connaissances
```

---

### I.B. Ce Qui Doit Figurer dans le Ticket à Chaque Étape

| **Étape** | **Ce qu'on ajoute au ticket** |
|---|---|
| ① Signalement | — (avant l'ouverture du ticket) |
| ② Enregistrement | Utilisateur, description, date/heure, équipement concerné |
| ③ Qualification | Catégorie, priorité, niveau attribué, SLA applicable |
| ④ Diagnostic | Actions de diagnostic tentées + résultats observés |
| ⑤ Résolution | Solution complète appliquée, étapes détaillées |
| ⑥ Validation | Confirmation de l'utilisateur (date/heure, moyen) |
| ⑦ Clôture | MTTR, satisfaction si recueillie, flag "à mettre en KB" |

> 📌 **Règle professionnelle :** Le ticket doit être rempli **en temps réel**, pas reconstruit de mémoire après la résolution. Un ticket rédigé après coup perd la chronologie et les hypothèses infructueuses — qui sont pourtant précieuses pour les incidents futurs.

---

## PARTIE II — La Méthode de Diagnostic Structurée

### II.A. Le Principe des Couches OSI Appliqué au Diagnostic

La méthode la plus efficace pour diagnostiquer un incident réseau ou système consiste à remonter les couches du modèle OSI **du bas vers le haut** — de la couche physique (câble, alimentation) vers la couche application (logiciel, droits).

```
   COUCHE 7 — Application   ← Vérifier en dernier
   COUCHE 6 — Présentation
   COUCHE 5 — Session
   COUCHE 4 — Transport     ← Ports, firewall, service actif ?
   COUCHE 3 — Réseau        ← IP, routage, ping ?
   COUCHE 2 — Liaison       ← MAC, switch, VLAN ?
   COUCHE 1 — Physique      ← Vérifier en premier
                               Câble branché ?
                               Voyant allumé ?
                               Alimentation ?
```

**En pratique pour chaque incident :**

> *"Avant de toucher au logiciel, vérifie le câble. Avant de vérifier le câble, regarde si la machine est allumée."*

---

### II.B. Méthode de Diagnostic Générale — 5 Questions

Pour tout incident, se poser ces 5 questions dans l'ordre :

| **Question** | **Ce qu'elle révèle** | **Exemple** |
|---|---|---|
| **1. Est-ce que ça a déjà fonctionné ?** | Régression vs jamais configuré | "Ça marchait hier" → chercher ce qui a changé |
| **2. Qu'est-ce qui a changé récemment ?** | Cause probable immédiate | Mise à jour, déplacement, nouveau câble... |
| **3. Est-ce que c'est reproductible ?** | Incident ponctuel vs permanent | Toujours / parfois / une seule fois |
| **4. Le problème est-il isolé ou généralisé ?** | 1 utilisateur vs infrastructure | 1 PC → poste / tous les PC → réseau ou serveur |
| **5. Y a-t-il un message d'erreur ?** | Information diagnostique directe | Copier le message exact — ne pas paraphraser |

---

### II.C. Diagnostic de Chaque Type d'Incident

#### IMPRIMANTE

```
   NIVEAU 1 — PHYSIQUE
   ├── Imprimante allumée ? Voyant d'état normal ?
   ├── Câble alimentation branché ?
   ├── Câble USB ou réseau branché des deux côtés ?
   └── Papier présent ? Bourrage papier ?

   NIVEAU 2 — SYSTÈME
   ├── Imprimante visible dans "Périphériques et imprimantes" ?
   ├── État de l'imprimante : en ligne / hors ligne / en pause ?
   ├── File d'attente : travaux bloqués ? Vider la file.
   └── Imprimante définie comme "Par défaut" ?

   NIVEAU 3 — PILOTE / RÉSEAU
   ├── Pilote installé et à jour ?
   ├── Si réseau : ping vers l'IP de l'imprimante ?
   ├── Port d'impression correct (IP, port 9100 ou 515) ?
   └── Pare-feu bloquant le port d'impression ?

   NIVEAU 4 — APPLICATION
   ├── L'application peut-elle imprimer (test page Windows) ?
   ├── Problème avec un seul logiciel ou tous ?
   └── Droits d'impression pour l'utilisateur ?
```

#### ACCÈS DOSSIER REFUSÉ

```
   NIVEAU 1 — CONNECTIVITÉ
   ├── Le partage réseau est-il accessible ? (ping du serveur)
   ├── Le chemin UNC est-il correct ? (\\serveur\partage)
   └── Le lecteur réseau est-il connecté ?

   NIVEAU 2 — AUTHENTIFICATION
   ├── L'utilisateur est-il authentifié sur le domaine ?
   ├── Le compte est-il actif et non verrouillé ?
   └── Mot de passe expiré ?

   NIVEAU 3 — DROITS DE PARTAGE
   ├── L'utilisateur (ou son groupe) est-il dans les droits de partage ?
   └── Niveau de droits suffisant (Lecture / Modification / Contrôle total) ?

   NIVEAU 4 — DROITS NTFS
   ├── Droits NTFS sur le dossier pour l'utilisateur ou son groupe ?
   ├── Vérifier les "Permissions effectives" (onglet Sécurité → Avancé)
   ├── Un refus (Deny) explicite annule tout droit accordé
   └── Héritage activé ou désactivé sur ce dossier ?

   → Règle : c'est la permission LA PLUS RESTRICTIVE entre
     droits de partage et droits NTFS qui s'applique
```

#### POSTE LENT

```
   NIVEAU 1 — RESSOURCES SYSTÈME (Gestionnaire des tâches)
   ├── CPU : consommation anormale ? Quel processus ?
   ├── RAM : utilisation > 85% ? Fichier d'échange actif ?
   ├── Disque : activité à 100% ? Disque HDD saturé ?
   └── Réseau : activité suspecte en arrière-plan ?

   NIVEAU 2 — PROCESSUS ET SERVICES
   ├── Processus inconnus consommant des ressources → malware ?
   ├── Mises à jour Windows en cours silencieusement ?
   ├── Antivirus en scan complet ?
   └── Service défaillant en boucle ?

   NIVEAU 3 — DÉMARRAGE ET PERSISTANCE
   ├── Nombreux programmes au démarrage ? (msconfig / Démarrage)
   ├── Espace disque disponible < 10% → ralentissement swap
   └── Fragmentation disque HDD ? (SSD : non pertinent)

   NIVEAU 4 — MATÉRIEL
   ├── RAM insuffisante pour l'usage (< 4Go pour W11)
   ├── Température CPU élevée → throttling (HWMonitor)
   └── Disque dur défaillant ? (CrystalDiskInfo — état SMART)
```

---

## PARTIE III — La Base de Connaissances (Knowledge Base)

### III.A. Pourquoi une Base de Connaissances ?

La **base de connaissances** (KB) est le référentiel des solutions aux incidents déjà rencontrés et résolus. Elle transforme l'expérience individuelle d'un technicien en **capital collectif** de la DSI.

```
   SANS BASE DE CONNAISSANCES          AVEC BASE DE CONNAISSANCES
   ──────────────────────────          ──────────────────────────
   Incident résolu                →    Incident résolu
   Le technicien "sait"           →    Solution documentée dans KB
   3 mois plus tard, même         →    3 mois plus tard, même
   incident, autre technicien          incident, autre technicien
   → repart de zéro               →    → consulte KB → 5 min
   → 45 min                            → Utilisateur satisfait
   → Utilisateur frustré          →    → MTTR en baisse
```

### III.B. Structure d'une Fiche KB

| **Section** | **Contenu** |
|---|---|
| **Titre** | Description concise du symptôme |
| **Symptômes** | Comment se manifeste le problème (ce que voit l'utilisateur) |
| **Cause(s) connue(s)** | Pourquoi ça arrive (1 à 3 causes les plus fréquentes) |
| **Solution** | Étapes de résolution numérotées et précises |
| **Vérification** | Comment s'assurer que c'est résolu |
| **Escalade** | Quand escalader vers N2 (si non résolu après ces étapes) |
| **Mots-clés** | Pour faciliter la recherche |
| **Auteur / Date** | Traçabilité |

---

## IV. Commandes Utiles par Incident

### Imprimante — Windows

```cmd
:: Lister les imprimantes installées
wmic printer list brief

:: État de l'imprimante
wmic printer where name="Nom_Imprimante" get PrinterStatus, WorkOffline

:: Vider la file d'attente manuellement
net stop spooler
del /Q /F /S "%systemroot%\System32\spool\PRINTERS\*.*"
net start spooler

:: Ping vers une imprimante réseau
ping [IP_imprimante]
```

```powershell
# PowerShell — lister les imprimantes
Get-Printer | Select-Object Name, PrinterStatus, PortName

# Redémarrer le spooler
Restart-Service -Name Spooler

# Supprimer une imprimante
Remove-Printer -Name "Nom_Imprimante"
```

### Droits et Accès — Windows

```cmd
:: Droits NTFS d'un dossier
icacls "C:\Dossier\Cible"

:: Appliquer des droits NTFS
icacls "C:\Dossier\Cible" /grant "DOMAINE\Utilisateur:(R)"
icacls "C:\Dossier\Cible" /grant "DOMAINE\Utilisateur:(M)"  :: Modification
icacls "C:\Dossier\Cible" /grant "DOMAINE\GRP_RH:(F)"       :: Contrôle total

:: Partages disponibles sur le serveur
net share

:: Tester l'accès à un partage
net use \\serveur\partage
```

```powershell
# Vérifier les droits NTFS sur un dossier
Get-Acl "C:\Dossier\Cible" | Format-List

# Droits effectifs pour un utilisateur (GUI nécessaire pour "Permissions effectives")
# En PowerShell - vérifier l'appartenance aux groupes
Get-ADGroupMember -Identity "GRP_RH" | Where-Object { $_.Name -eq "alice.martin" }
```

### Performances Système — Windows

```cmd
:: Processus consommateurs (tri par CPU)
tasklist /fo table | sort /r /+65

:: Espace disque
wmic logicaldisk get name,freespace,size

:: Informations RAM
wmic computersystem get totalphysicalmemory
wmic OS get FreePhysicalMemory
```

```powershell
# Top 10 processus par CPU
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU, WorkingSet

# Top 10 processus par RAM
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10 Name, WorkingSet

# Espace disque disponible
Get-PSDrive -PSProvider FileSystem | Select-Object Name, Used, Free

# Services en échec
Get-Service | Where-Object { $_.Status -eq "Stopped" -and $_.StartType -eq "Automatic" }

# Programmes au démarrage
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location
```

---

## V. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Cycle de vie d'un incident** | Séquence complète : signalement → enregistrement → qualification → diagnostic → résolution → validation → clôture |
| **Base de connaissances (KB)** | Référentiel des solutions aux incidents résolus — capital collectif de la DSI |
| **Diagnostic différentiel** | Méthode consistant à tester et éliminer des hypothèses une par une |
| **Permissions effectives** | Résultat final des droits NTFS appliqués à un utilisateur, tenant compte de tous ses groupes |
| **Héritage NTFS** | Transmission automatique des droits d'un dossier parent à ses sous-dossiers |
| **Deny (Refus explicite)** | Droit NTFS qui annule tout droit accordé — prioritaire sur toute permission |
| **Spooler** | Service Windows gérant la file d'attente d'impression |
| **File d'attente** | Liste des travaux d'impression en attente d'être envoyés à l'imprimante |
| **Throttling** | Réduction automatique des performances du CPU en cas de surchauffe |
| **SMART** | Self-Monitoring, Analysis and Reporting Technology — système de surveillance des disques durs |
| **Fichier d'échange (swap)** | Espace disque utilisé comme mémoire virtuelle quand la RAM est saturée |
| **Permissions effectives** | Combinaison réelle des droits d'accès d'un utilisateur sur une ressource |
| **`icacls`** | Outil Windows en ligne de commande pour gérer les droits NTFS |
| **`net share`** | Commande Windows affichant les partages réseau du serveur |
| **`tasklist`** | Commande Windows listant les processus en cours |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Je décris les 7 étapes du cycle de vie d'un incident
- [ ] J'applique les 5 questions de diagnostic dans l'ordre
- [ ] Je diagnostique une imprimante en partant de la couche physique
- [ ] Je vérifie les droits NTFS avec `icacls` ou l'interface graphique
- [ ] J'identifie un processus qui sature le CPU avec le Gestionnaire des tâches
- [ ] Je remplis un ticket en temps réel pendant la résolution
- [ ] Je rédige une fiche de base de connaissances exploitable par un collègue

---