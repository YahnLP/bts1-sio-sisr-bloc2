---
author: YLP
title: 🖥️ FICHE DE TP
---

# 🖥️ FICHE TP ÉLÈVE
## TP S13 — Serveur DHCP, Partages et DFS sous Windows Server

*Durée : ~2h30 de TP — À réaliser en binôme*

---

## Contexte

Vous êtes technicien systèmes et réseaux chez **SimIO SARL**. Votre DSI vous confie la mise en place des services suivants sur le serveur Windows Server de la salle :

- Un service **DHCP** pour attribuer automatiquement les adresses IP aux postes du réseau
- Des **dossiers partagés** avec des droits adaptés pour 3 services : RH, Informatique, Direction
- Un **espace de noms DFS** pour que les utilisateurs accèdent aux fichiers via un chemin stable

---

## Partie A — Installation et Configuration du Service DHCP

### Prérequis : Vérifier la configuration de la VM Serveur

Avant de commencer, assurez-vous que votre VM Serveur dispose d'une **IP statique** (le serveur DHCP lui-même ne doit jamais avoir une IP dynamique).

Notez la configuration IP de votre VM Serveur :
- Adresse IP : ___________________________
- Masque : ___________________________
- Passerelle : ___________________________

---

### A1 — Installer le rôle DHCP

1. Ouvrir le **Gestionnaire de serveur** → Cliquer sur **Ajouter des rôles et des fonctionnalités**
2. Type d'installation : **Installation basée sur un rôle ou une fonctionnalité** → Suivant
3. Sélectionner le serveur local → Suivant
4. Cocher le rôle **Serveur DHCP** → Ajouter les fonctionnalités → Suivant
5. Cliquer sur **Installer** et attendre la fin de l'installation
6. Cliquer sur **Terminer la configuration DHCP** dans la notification jaune du Gestionnaire de serveur
7. Dans l'assistant de post-installation, valider avec les droits administrateur → Valider → Fermer

✅ **Point de contrôle :** Dans le Gestionnaire de serveur → Outils → DHCP, votre serveur doit apparaître avec une icône verte.

---

### A2 — Créer une Étendue DHCP

1. Ouvrir la **Console DHCP** : Gestionnaire de serveur → Outils → DHCP
2. Développer le nœud de votre serveur → Clic droit sur **IPv4** → Nouvelle étendue
3. Renseigner les paramètres suivants :

| **Paramètre** | **Valeur à saisir** |
|---|---|
| Nom de l'étendue | `Réseau_Principal` |
| Plage de début | `192.168.10.20` |
| Plage de fin | `192.168.10.200` |
| Masque de sous-réseau | `255.255.255.0` |
| Durée du bail | `8 jours` |

4. Ajouter une exclusion pour les adresses `192.168.10.20` à `192.168.10.30` (réservées aux serveurs)
5. **Ne pas configurer les options pour l'instant** → Terminer l'assistant
6. Clic droit sur l'étendue → **Activer**

✅ **Point de contrôle :** L'étendue doit apparaître avec une icône verte (Active).

---

### A3 — Configurer les Options DHCP

1. Dans la console DHCP, développer votre étendue → Clic droit sur **Options d'étendue** → Configurer les options
2. Cocher l'option **003 Routeur** → Saisir l'adresse de la passerelle : `192.168.10.1` → Ajouter → OK
3. Cocher l'option **006 Serveurs DNS** → Saisir l'adresse du DNS : `192.168.10.10` → Ajouter → OK
4. Cocher l'option **015 Nom de domaine DNS** → Saisir : `siosarl.local` → OK

✅ **Point de contrôle :** Dans Options d'étendue, les 3 options (003, 006, 015) doivent être listées.

---

### A4 — Tester le DHCP depuis la VM Cliente

1. Sur la **VM Cliente Windows**, vérifier que la carte réseau est en **"Obtenir une adresse IP automatiquement"**
2. Ouvrir une **invite de commandes** et exécuter :
   ```
   ipconfig /release
   ipconfig /renew
   ipconfig /all
   ```
3. Vérifier que le client a bien obtenu :
   - Une adresse dans la plage 192.168.10.31 → 192.168.10.200
   - La passerelle 192.168.10.1
   - Le DNS 192.168.10.10

4. Dans la console DHCP du serveur, vérifier que le bail apparaît sous **Baux d'adresses**

✅ **Point de contrôle :** Capture d'écran de `ipconfig /all` montrant l'IP, la passerelle et le serveur DHCP obtenus.

---

### A5 — Créer une Réservation DHCP

1. Dans la console DHCP → Votre étendue → Clic droit sur **Réservations** → Nouvelle réservation
2. Renseigner :
   - Nom : `PC-Directeur`
   - Adresse IP : `192.168.10.50`
   - Adresse MAC : *(récupérer l'adresse MAC de la VM cliente via `ipconfig /all`, champ "Adresse physique")*
3. Cliquer sur **Ajouter** puis **Fermer**
4. Sur la VM Cliente : `ipconfig /release` puis `ipconfig /renew`

✅ **Point de contrôle :** La VM cliente doit maintenant obtenir l'IP 192.168.10.50.

---

### A6 — Compléter la Fiche de Configuration DHCP

Remplir la **Fiche de Configuration DHCP** (voir annexe du pack) avec toutes les informations de votre configuration.

---

## Partie B — Arborescence de Dossiers Partagés avec Droits

### B1 — Créer les Groupes AD

*(Si ce n'est pas déjà fait depuis la S12)*

1. Ouvrir **Utilisateurs et ordinateurs Active Directory**
2. Créer une OU `Groupes` dans votre domaine
3. Créer les groupes globaux de sécurité suivants :

| **Groupe** | **Membres à ajouter** |
|---|---|
| `GRP_RH` | Utilisateur `alice.martin` (à créer si inexistant) |
| `GRP_Informatique` | Utilisateur `bob.techno` (à créer si inexistant) |
| `GRP_Direction` | Utilisateur `claire.dir` (à créer si inexistant) |
| `GRP_Tous_Salaries` | Les 3 utilisateurs ci-dessus |

---

### B2 — Créer l'Arborescence de Dossiers

1. Ouvrir l'**Explorateur de fichiers** sur le serveur
2. Sur le disque `C:` (ou `D:` si disponible), créer le dossier `C:\Partages`
3. Dans ce dossier, créer les sous-dossiers suivants :
   - `C:\Partages\RH`
   - `C:\Partages\Informatique`
   - `C:\Partages\Direction`
   - `C:\Partages\Commun`

---

### B3 — Configurer les Droits NTFS

Pour chaque dossier, appliquer les droits NTFS suivants (Propriétés → Sécurité → Modifier) :

> **Important :** Désactiver d'abord l'héritage sur chaque sous-dossier (Avancé → Désactiver l'héritage → Convertir) puis retirer les entrées non nécessaires.

**Dossier `RH` :**

| **Principal** | **Droit NTFS** |
|---|---|
| SYSTEM | Contrôle total |
| Administrateurs | Contrôle total |
| GRP_RH | Modifier |
| GRP_Direction | Lecture et exécution |
| GRP_Informatique | Contrôle total |

**Dossier `Informatique` :**

| **Principal** | **Droit NTFS** |
|---|---|
| SYSTEM | Contrôle total |
| Administrateurs | Contrôle total |
| GRP_Informatique | Contrôle total |

**Dossier `Direction` :**

| **Principal** | **Droit NTFS** |
|---|---|
| SYSTEM | Contrôle total |
| Administrateurs | Contrôle total |
| GRP_Direction | Contrôle total |
| GRP_Informatique | Contrôle total |

**Dossier `Commun` :**

| **Principal** | **Droit NTFS** |
|---|---|
| SYSTEM | Contrôle total |
| Administrateurs | Contrôle total |
| GRP_Tous_Salaries | Modifier |
| GRP_Informatique | Contrôle total |

---

### B4 — Partager les Dossiers

1. Pour chaque dossier : Propriétés → **Partage** → Partage avancé → Cocher "Partager ce dossier"
2. Donner un nom de partage cohérent (ex : `RH$` avec `$` pour partage caché, ou `RH`)
3. Cliquer sur **Autorisations** → Ajouter `Tout le monde` → Droit : **Contrôle total**

> 💡 Cette configuration (Contrôle total côté partage, droits granulaires côté NTFS) est la bonne pratique recommandée.

---

### B5 — Tester les Accès depuis la VM Cliente

1. Sur la VM cliente (connectée en tant que `alice.martin`) :
   - Ouvrir l'Explorateur → Dans la barre d'adresse : `\\NomDuServeur\RH` → Vérifier l'accès
   - Essayer `\\NomDuServeur\Direction` → Doit être **refusé**

2. Se déconnecter, se reconnecter en tant que `claire.dir` :
   - Vérifier l'accès à `\\NomDuServeur\Direction` → **Doit fonctionner**
   - Vérifier l'accès à `\\NomDuServeur\RH` → **Lecture seule** (essayer de créer un fichier → refusé)

✅ **Point de contrôle :** Captures d'écran des tests d'accès pour chaque utilisateur.

---

### B6 — Compléter la Fiche de Documentation des Droits

Remplir la **Fiche de Documentation des Droits** (voir annexe du pack) avec la matrice complète des droits.

---

## Partie C — Introduction DFS : Créer un Espace de Noms

### C1 — Installer le Rôle DFS

1. Gestionnaire de serveur → Ajouter des rôles → **Services de fichiers et de stockage** → **Espaces de noms DFS**
2. Installer le rôle

---

### C2 — Créer un Espace de Noms DFS

1. Gestionnaire de serveur → Outils → **Gestion DFS**
2. Clic droit sur **Espaces de noms** → Nouvel espace de noms
3. Sélectionner votre serveur comme hôte de l'espace de noms
4. Nommer l'espace de noms : `data`
5. Choisir : **Espace de noms basé sur un domaine** (si AD configuré) ou **Espace de noms autonome**
6. Terminer l'assistant

L'espace de noms est maintenant accessible via : `\\siosarl.local\data` (ou `\\NomServeur\data`)

---

### C3 — Ajouter des Dossiers DFS pointant vers les Partages Existants

1. Clic droit sur l'espace de noms → **Nouveau dossier**
2. Créer les entrées suivantes :

| **Nom du dossier DFS** | **Cible (partage physique)** |
|---|---|
| `RH` | `\\NomServeur\RH` |
| `Informatique` | `\\NomServeur\Informatique` |
| `Direction` | `\\NomServeur\Direction` |
| `Commun` | `\\NomServeur\Commun` |

---

### C4 — Tester l'Accès via le Chemin DFS

Depuis la VM Cliente, tester l'accès via le chemin DFS :

```
\\siosarl.local\data\RH
\\siosarl.local\data\Commun
```

✅ **Point de contrôle :** Les mêmes fichiers sont accessibles, mais via un chemin qui ne dépend plus du nom physique du serveur.

---
