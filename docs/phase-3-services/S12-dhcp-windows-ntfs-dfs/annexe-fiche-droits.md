# Annexe – Fiche de documentation des droits

*BTS SIO SISR 1re année – Bloc 2 – S12*

## 1. Structure de l'Arborescence

| **Chemin complet** | **Nom du partage** | **Chemin UNC d'accès** |
|---|---|---|
| C:\Partages\RH | | |
| C:\Partages\Informatique | | |
| C:\Partages\Direction | | |
| C:\Partages\Commun | | |

---

## 2. Matrice des Droits NTFS

| **Dossier** | **Principal (groupe/utilisateur)** | **Droit NTFS accordé** |
|---|---|---|
| RH | GRP_RH | |
| RH | GRP_Direction | |
| RH | GRP_Informatique | |
| Informatique | GRP_Informatique | |
| Direction | GRP_Direction | |
| Direction | GRP_Informatique | |
| Commun | GRP_Tous_Salaries | |
| Commun | GRP_Informatique | |

---

## 3. Droits de Partage

| **Dossier partagé** | **Principal** | **Droit de partage** |
|---|---|---|
| RH | Tout le monde | |
| Informatique | Tout le monde | |
| Direction | Tout le monde | |
| Commun | Tout le monde | |

---

## 4. Calcul de l'Accès Effectif

Calculer l'accès effectif pour chaque combinaison :

| **Utilisateur** | **Dossier** | **Droit partage** | **Droit NTFS** | **Accès effectif** |
|---|---|---|---|---|
| alice.martin (GRP_RH) | RH | | | |
| alice.martin (GRP_RH) | Direction | | | |
| claire.dir (GRP_Direction) | RH | | | |
| claire.dir (GRP_Direction) | Direction | | | |
| bob.techno (GRP_Informatique) | RH | | | |
| bob.techno (GRP_Informatique) | Direction | | | |

---

## 5. Tests de Validation

| **Test** | **Utilisateur** | **Dossier testé** | **Résultat attendu** | **Résultat obtenu** | **Capture N°** |
|---|---|---|---|---|---|
| Accès lecture | alice.martin | RH | Autorisé | | |
| Création fichier | alice.martin | RH | Autorisé | | |
| Accès | alice.martin | Direction | Refusé | | |
| Accès lecture | claire.dir | RH | Autorisé | | |
| Création fichier | claire.dir | RH | Refusé | | |
| Accès | claire.dir | Direction | Autorisé | | |

---

## 6. Architecture DFS

| **Espace de noms** | **Chemin UNC DFS** | **Cible physique** |
|---|---|---|
| data | \\siosarl.local\data\RH | |
| data | \\siosarl.local\data\Informatique | |
| data | \\siosarl.local\data\Direction | |
| data | \\siosarl.local\data\Commun | |

---
