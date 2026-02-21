# Semaine 4 (S4) - BLOC 1
## 🎫 Gestion des Incidents · Cycle de Vie du Ticket · TP Résolution

---

# 🖥️ FICHE TP ÉLÈVE
## Résolution de 3 Incidents — SimIO SARL

*Durée : 105 minutes (35 min par incident) — Binôme*

---

## Consignes Générales

1. **Ouvrir un ticket** avant de commencer chaque diagnostic (modèle S3)
2. **Remplir le ticket en temps réel** — noter chaque action et son résultat immédiatement
3. **1 action à la fois** — observer le résultat avant d'en faire une autre
4. **Commencer toujours par la couche physique** (câble, alimentation, voyants)
5. En fin d'incident : **rédiger la fiche KB** correspondante

---

## ─── INCIDENT 1 ─── IMPRIMANTE HORS SERVICE

### Contexte

*Marie Durand, service Comptabilité, appelle le centre de services :*

> *"Bonjour, c'est Marie, j'essaie d'imprimer mes relevés depuis ce matin mais l'imprimante ne fait rien. Le document disparaît juste après l'envoi et rien ne sort. J'ai besoin d'imprimer avant 14h pour la réunion avec le commissaire aux comptes."*

---

### Ticket — À Remplir Avant de Commencer

| **Champ** | **Votre saisie** |
|---|---|
| Titre | |
| Utilisateur / Service | |
| Description | |
| Impact métier | |
| Priorité | |
| Niveau initial | |

---

### Checklist de Diagnostic — Imprimante

Cocher chaque vérification effectuée et noter le résultat :

**🔌 Niveau Physique**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Imprimante allumée, voyant d'état normal | | |
| Câble alimentation branché | | |
| Câble USB / réseau branché des deux côtés | | |
| Pas de bourrage papier / papier présent | | |
| Pas de message d'erreur sur l'écran LCD de l'imprimante | | |

**🖥️ Niveau Système**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Imprimante visible dans "Périphériques et imprimantes" | | |
| Statut de l'imprimante : en ligne ou hors ligne ? | | |
| File d'attente : travaux bloqués ? | | |
| Imprimante définie comme imprimante par défaut ? | | |

**Commandes exécutées :**
```
Commande 1 : _________________________________ Résultat : _______________
Commande 2 : _________________________________ Résultat : _______________
Commande 3 : _________________________________ Résultat : _______________
```

**🔧 Niveau Pilote / Réseau**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Pilote installé et à jour | | |
| Si réseau : ping vers l'IP de l'imprimante | | |
| Port configuré correctement | | |

---

### Résolution

| **Champ** | **Votre saisie** |
|---|---|
| **Cause identifiée** | |
| **Solution appliquée (étapes)** | 1. 2. 3. |
| **Test de validation** | |
| **Résultat du test** | |
| **MTTR** | min |

---

### Fiche Base de Connaissances — Incident 1

| **Section** | **Contenu** |
|---|---|
| **Titre KB** | |
| **Symptômes** | |
| **Cause principale** | |
| **Solution (étapes)** | 1. 2. 3. |
| **Vérification** | |
| **Escalade si** | |
| **Mots-clés** | imprimante, |

---

---

## ─── INCIDENT 2 ─── ACCÈS DOSSIER REFUSÉ

### Contexte

*Ahmed Benali, service Ressources Humaines, écrit au centre de services :*

> *"Bonjour, depuis ce matin quand j'essaie d'ouvrir le dossier RH sur le serveur, Windows m'affiche 'Vous n'avez pas l'autorisation d'accéder à \\SIOSARL-SRV\Partages\RH. Contactez votre administrateur réseau pour demander l'accès.' Hier soir ça fonctionnait sans problème. Je dois mettre à jour des contrats ce matin."*

---

### Ticket — À Remplir Avant de Commencer

| **Champ** | **Votre saisie** |
|---|---|
| Titre | |
| Utilisateur / Service | |
| Description | |
| Impact métier | |
| Priorité | |
| Niveau initial | |

---

### Checklist de Diagnostic — Accès Refusé

**🌐 Niveau Connectivité**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Ping vers le serveur de fichiers | | |
| Accès à d'autres partages sur le même serveur | | |
| Chemin UNC correct : `\\SIOSARL-SRV\Partages\RH` | | |

**🔐 Niveau Authentification**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Utilisateur connecté au domaine siosarl.local | | |
| Compte actif dans Active Directory | | |
| Mot de passe non expiré | | |
| Compte non verrouillé | | |

**📁 Niveau Droits de Partage**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Accéder aux propriétés du partage → onglet Partage | | |
| L'utilisateur ou son groupe (GRP_RH) figure-t-il dans les droits de partage ? | | |
| Niveau de droits accordés | | |

**🔒 Niveau Droits NTFS**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Clic droit dossier → Propriétés → Sécurité | | |
| L'utilisateur ou GRP_RH figure-t-il dans la liste ? | | |
| Y a-t-il un refus (Deny) explicite ? | | |
| Vérifier les "Permissions effectives" | | |

**Commandes exécutées :**
```
Commande 1 : _________________________________ Résultat : _______________
Commande 2 : _________________________________ Résultat : _______________
Commande 3 : _________________________________ Résultat : _______________
```

---

### Résolution

| **Champ** | **Votre saisie** |
|---|---|
| **Cause identifiée** | |
| **Solution appliquée** | 1. 2. 3. |
| **Droit NTFS ajouté** | Utilisateur/Groupe : _____ / Niveau : _____ |
| **Test de validation** | |
| **MTTR** | min |

---

### Question de Réflexion

> "Ahmed appartient au groupe GRP_RH qui a les droits en Modification sur le partage. Mais en NTFS, GRP_RH n'a que Lecture. Quelle permission s'applique effectivement ? Pourquoi ?"

```
Réponse : ___________________________________________________________
___________________________________________________________________
```

**Correction :** La permission la plus restrictive entre droits de partage et droits NTFS s'applique. Ici : Modification (partage) ET Lecture (NTFS) → **Lecture** s'applique. Ahmed peut lire les fichiers mais pas les modifier. Pour lui donner la modification, il faut ajouter GRP_RH avec les droits Modification en NTFS (ou Contrôle total si on accorde tout via le partage et qu'on gère via NTFS uniquement).

---

### Fiche Base de Connaissances — Incident 2

| **Section** | **Contenu** |
|---|---|
| **Titre KB** | |
| **Symptômes** | |
| **Cause principale** | |
| **Solution (étapes)** | 1. 2. 3. |
| **Vérification** | |
| **Escalade si** | |
| **Mots-clés** | accès refusé, NTFS, |

---

---

## ─── INCIDENT 3 ─── POSTE LENT

### Contexte

*Julien Morel, service Commercial, appelle en fin de matinée :*

> *"Bonjour, mon PC est devenu vraiment très lent depuis ce matin, tout rame, même pour ouvrir le Bloc-Notes. J'ai redémarré et c'est redevenu normal 5 minutes, et puis ça a recommencé à ramer. J'ai un rendez-vous client par visio dans une heure et j'ai peur que ça plante."*

---

### Ticket — À Remplir Avant de Commencer

| **Champ** | **Votre saisie** |
|---|---|
| Titre | |
| Utilisateur / Service | |
| Description | |
| Impact métier | |
| Priorité | |
| Niveau initial | |

---

### Checklist de Diagnostic — Poste Lent

**📊 Niveau Ressources (Gestionnaire des tâches — Ctrl+Shift+Échap)**

| **Vérification** | **Valeur constatée** | **Normal ?** |
|---|---|---|
| % CPU global (onglet Performances) | % | < 80% au repos |
| Processus consommant le plus de CPU | | |
| % RAM utilisée | % | < 85% |
| RAM disponible (Mo ou Go) | | > 500 Mo |
| % Disque (activité) | % | < 80% au repos |
| Processus consommant le plus de RAM | | |

**Commandes exécutées :**
```
Commande 1 : _________________________________ Résultat : _______________
Commande 2 : _________________________________ Résultat : _______________
Commande 3 : _________________________________ Résultat : _______________
```

**🔍 Niveau Processus et Services**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Processus inconnu consommant des ressources | | |
| Windows Update en cours en arrière-plan | | |
| Antivirus en scan complet | | |
| Service en état d'erreur (Gestionnaire des services) | | |

**💾 Niveau Disque et Démarrage**

| **Vérification** | **✅/❌** | **Observation** |
|---|---|---|
| Espace disque C: disponible (> 10%) | | |
| Programmes lancés au démarrage (nombreux ?) | | |
| Fragmentation disque HDD (si applicable) | | |

---

### Résolution

| **Champ** | **Votre saisie** |
|---|---|
| **Cause identifiée** | |
| **Solution appliquée** | 1. 2. 3. |
| **Mesure CPU/RAM après correction** | CPU : ___% / RAM : ___% |
| **Test de validation** | |
| **MTTR** | min |

---

### Question de Réflexion

> "Après résolution, que conseillez-vous à Julien pour éviter que ce problème se reproduise ? Donnez 2 recommandations concrètes."

```
Recommandation 1 : __________________________________________________
Recommandation 2 : __________________________________________________
```

---

### Fiche Base de Connaissances — Incident 3

| **Section** | **Contenu** |
|---|---|
| **Titre KB** | |
| **Symptômes** | |
| **Cause principale** | |
| **Solution (étapes)** | 1. 2. 3. |
| **Vérification** | |
| **Escalade si** | |
| **Mots-clés** | poste lent, CPU, RAM, |

---

---

## Bilan des 3 Incidents — Tableau Récapitulatif

*À compléter en fin de TP*

| **Incident** | **Cause racine** | **Solution** | **MTTR** | **Niveau** | **En KB ?** |
|---|---|---|---|---|---|
| Imprimante | | | min | | ☐ |
| Accès dossier | | | min | | ☐ |
| Poste lent | | | min | | ☐ |

**MTTR moyen :** _____ min (total / 3)

**Incident le plus difficile à diagnostiquer :** _________________________

**Raison :** _____________________________________________________________

---

---

# 📄 ANNEXE — CHECKLIST RAPIDE DE DIAGNOSTIC N1

*À conserver dans son portfolio — Référence pour toutes les interventions futures*

---

```
╔══════════════════════════════════════════════════════════════════╗
║         CHECKLIST TECHNICIEN N1 — AVANT TOUTE INTERVENTION       ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  □  1. J'ai ouvert un ticket AVANT de commencer                  ║
║  □  2. J'ai collecté : Qui / Quoi / Quand / Impact               ║
║  □  3. J'ai demandé "qu'est-ce qui a changé récemment ?"         ║
║  □  4. Je commence par la couche physique                         ║
║  □  5. Je fais 1 action à la fois et j'observe                   ║
║  □  6. Je note chaque action dans le ticket en temps réel        ║
║  □  7. Je teste la résolution avec l'utilisateur                  ║
║  □  8. J'ai validé la clôture avec l'utilisateur                 ║
║  □  9. J'ai calculé et noté le MTTR                              ║
║  □  10. J'ai créé ou mis à jour la fiche KB si nécessaire        ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║  ESCALADER VERS N2 SI :                                          ║
║  □  Dépassement du délai SLA prévu                               ║
║  □  Incident nécessite des droits serveur / infra                ║
║  □  Cause racine inconnue après 3 hypothèses testées             ║
║  □  Impact élargi détecté (plusieurs utilisateurs)               ║
║  □  Incident récurrent (3e occurrence ou plus)                   ║
╚══════════════════════════════════════════════════════════════════╝
```
