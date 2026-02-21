# Semaine 3 (S3) - BLOC 1
## 🔧 ITIL Introduction · Service IT · Centre de Services · Niveaux de Support

---


# 🖥️ FICHE TP — TICKETS ET NIVEAUX DE SUPPORT

*Durée : 50 minutes — En binôme*

---

## Exercice 1 — Classification des Sollicitations (10 min)

Pour chacune des 12 sollicitations suivantes, indiquer le **type** (Incident / Problème / Changement / Demande de service), la **priorité** (P1 à P4) et le **niveau de support initial** (N1 / N2) :

| **N°** | **Sollicitation** | **Type** | **Priorité** | **Niveau** |
|---|---|---|---|---|
| 1 | "Mon mot de passe Active Directory a expiré, je ne peux plus me connecter" | | | |
| 2 | "Le serveur de fichiers partagés est inaccessible pour les 80 utilisateurs" | | | |
| 3 | "Peut-on installer Adobe Reader sur mon poste ?" | | | |
| 4 | "Mon PC fait un bruit bizarre depuis 2 jours" | | | |
| 5 | "La connexion WiFi est lente depuis une semaine pour tout le service Commercial" | | | |
| 6 | "Je voudrais créer un compte VPN pour un nouveau collaborateur qui arrive lundi" | | | |
| 7 | "Le site web de l'entreprise est inaccessible depuis Internet" | | | |
| 8 | "Mon imprimante n'est plus reconnue après la mise à jour Windows d'hier" | | | |
| 9 | "3 postes du service RH ont planté avec le même écran bleu ce mois-ci" | | | |
| 10 | "Je n'arrive pas à ouvrir mes emails depuis mon téléphone" | | | |
| 11 | "Nous souhaitons migrer notre serveur de messagerie vers Microsoft 365" | | | |
| 12 | "Le logiciel de paie plante lors du calcul mensuel — la paie de 300 employés bloquée" | | | |

**Corrigé :**

| **N°** | **Type** | **Priorité** | **Niveau** | **Justification** |
|---|---|---|---|---|
| 1 | Incident | P3 | N1 | 1 utilisateur bloqué — procédure standard (réinitialisation mot de passe) |
| 2 | Incident | P1 | N2 | Impact total — 80 utilisateurs bloqués — accès serveur nécessaire |
| 3 | Demande de service | P4 | N1 | Standard, pas urgent |
| 4 | Incident | P3 | N1/N2 | Possible défaillance matérielle imminente — diagnostic matériel requis |
| 5 | Incident | P2 | N2 | Groupe d'utilisateurs impacté, problème réseau/infrastructure |
| 6 | Demande de service | P3 | N1/N2 | Standard mais nécessite accès VPN (droits N2 si infra externe) |
| 7 | Incident | P1 | N2 | Service externe en panne — impact image entreprise |
| 8 | Incident | P3 | N1 | 1 utilisateur, pilote imprimante — procédure connue |
| 9 | Problème | P2 | N2 | Cause racine à identifier (BSOD récurrent) |
| 10 | Incident | P3 | N1 | 1 utilisateur, config mobile — procédure IMAP/Exchange |
| 11 | Changement | P4 | N2/Chef de projet | Migration — CAB (Change Advisory Board) requis |
| 12 | Incident | P1 | N2/N3 | Impact critique sur processus métier vital |

---

## Exercice 2 — Rédiger un Ticket Professionnel (20 min)

### Scénario A

Vous recevez l'appel suivant :

> *"Allo ? Oui c'est Pierre de la direction. Euh… mon ordinateur il veut plus démarrer depuis ce matin. J'ai des trucs importants à présenter au CODIR dans 2 heures. C'est vraiment urgent, j'ai essayé de l'allumer plusieurs fois mais ça s'éteint tout de suite, y'a un écran bleu qui passe vite. Hier soir ça marchait bien."*

Rédiger le ticket complet dans le tableau suivant :

| **Champ** | **Votre rédaction** |
|---|---|
| **Titre du ticket** | |
| **Utilisateur / Service / Contact** | |
| **Description complète** | |
| **Impact métier** | |
| **Actions déjà tentées** | |
| **Priorité (P1-P4)** | |
| **Niveau de support** | |
| **Pièces jointes recommandées** | |

**Correction attendue :**

| **Champ** | **Correction** |
|---|---|
| **Titre** | "PC ne démarre pas — BSOD — Pierre [Nom] — Direction — CODIR dans 2h" |
| **Utilisateur** | "Pierre [Nom] — Direction — ext./email à collecter" |
| **Description** | "Depuis ce matin, le PC de M. [Nom] s'éteint immédiatement après allumage avec un écran bleu de courte durée. Le problème est apparu aujourd'hui — la veille le poste fonctionnait normalement. Tentatives d'allumage multiples, échec à chaque fois." |
| **Impact** | "M. [Nom] doit présenter un dossier au CODIR dans 2 heures — bloqué sur ses fichiers de présentation" |
| **Actions** | "Plusieurs tentatives de redémarrage — problème persistant" |
| **Priorité** | P1 — Impact élevé (direction, présentation imminente) + Urgence élevée (2h) |
| **Niveau** | N2 — BSOD suspect (matériel ou pilote) + utilisateur de direction |
| **Pièces jointes** | Photo de l'écran bleu si accessible, numéro de série du PC |

---

### Scénario B

> *"Bonjour, ici Sophie du marketing. Je travaille sur une présentation pour demain et je voudrais que vous m'installiez Photoshop, PowerPoint et aussi ce logiciel de montage vidéo que mes collègues ont. Ah et aussi, mon souris est un peu lente, vous pourriez regarder ?"*

Rédiger un ticket **ou** expliquer pourquoi ce sont en réalité **deux tickets distincts** à créer.

**Correction attendue :** Deux tickets distincts :
- **Ticket 1 (Demande de service P4)** : Installation de logiciels — Photoshop (licence à vérifier), PowerPoint (déjà dans Microsoft 365 ?), logiciel de montage vidéo (lequel ? licence ?)
- **Ticket 2 (Incident P4)** : Souris lente — diagnostic (filaire ? sans fil ? batterie ? pilote ?)

> 💡 **Règle professionnelle :** 1 ticket = 1 sujet. Mélanger plusieurs demandes dans un ticket empêche le suivi individuel et fausse les statistiques SLA.

---

## Exercice 3 — Jeu de Rôle N1/N2 (20 min)

*En binôme — alternez les rôles*

**Scénario :** L'apprenant A joue le technicien N1 au centre de services. L'apprenant B joue un utilisateur qui appelle avec un incident. Le technicien N1 doit :
1. Accueillir professionnellement l'utilisateur
2. Collecter toutes les informations pour rédiger le ticket
3. Tenter une résolution N1
4. Décider si l'escalade vers N2 est nécessaire et la justifier

**Scénarios à jouer :**

**Scénario 3.1 :** L'utilisateur ne retrouve plus ses fichiers dans son dossier personnel réseau. Il est stressé, pense avoir tout perdu. En réalité, il s'est connecté hier avec un compte temporaire et ses fichiers sont dans son dossier habituel.

**Scénario 3.2 :** L'utilisateur signale que depuis la mise à jour de ce matin, son deuxième écran n'est plus détecté. Il a besoin des deux écrans pour son travail. Le problème concerne uniquement son poste.

**Scénario 3.3 :** L'utilisateur est en déplacement et ne peut pas se connecter au VPN de l'entreprise. Il doit accéder à des documents confidentiels. L'accès VPN est configuré sur son PC par le service IT — il n'a aucune documentation.

**Grille d'observation (à remplir par l'observateur pendant le jeu de rôle) :**

| **Critère** | **Observé ?** | **Note** |
|---|---|---|
| Accueil professionnel (se présente, note le nom de l'utilisateur) | ☐ Oui / ☐ Non | |
| Collecte toutes les infos pour le ticket (quoi, quand, impact) | ☐ Oui / ☐ Non | |
| Utilise le vocabulaire ITIL (incident, ticket, priorité...) | ☐ Oui / ☐ Non | |
| Tente une résolution N1 avant d'escalader | ☐ Oui / ☐ Non | |
| Informe l'utilisateur du suivi (délai, escalade si applicable) | ☐ Oui / ☐ Non | |
| Reste calme même si l'utilisateur est stressé | ☐ Oui / ☐ Non | |

---
