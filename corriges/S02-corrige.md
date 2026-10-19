# Corrigé S02 – Comptes locaux et permissions NTFS

*Réservé à l'enseignant. Source : [TP S02](../docs/phase-1-fondamentaux/S02-systeme-comptes-ntfs/02-tp.md).*

## Partie 1 : comptes

| Contrôle | Attendu |
|---|---|
| `Get-LocalUser` | `mmartin`, `pdubois`, `sleroux`, `tpetit` présents |
| Groupe Administrateurs | `mmartin` + compte initial |
| Groupe Utilisateurs | `pdubois`, `sleroux`, `tpetit` |
| Test `pdubois` | `compmgmt.msc` demande une identification administrateur (UAC) |

**J'anticipe** : un utilisateur standard ne peut pas installer pour tous ; l'UAC demande un compte administrateur (moindre privilège).

## Partie 2 : matrice des permissions

| Dossier | Administrateurs | Direction | Comptabilité | Commercial | Technique |
|---|---|---|---|---|---|
| Direction | Contrôle total | Modification | – | – | – |
| Comptabilite | Contrôle total | Lecture | Modification | – | – |
| Commercial | Contrôle total | Lecture | – | Modification | – |
| Technique | Contrôle total | Lecture | – | – | Modification |
| Commun | Contrôle total | Modification | Lecture | Lecture | Lecture |

Résultats des tests : `pdubois` modifie Comptabilite, lit Commun, refusé ailleurs ; `mmartin` lit les trois dossiers métiers, modifie Direction et Commun ; `tpetit` modifie Commercial, lit Commun, refusé ailleurs.

**J'anticipe** : les droits s'additionnent (lecture + modification = modification) ; un **refus explicite l'emporte** (onglet *Accès effectif* pour vérifier).

## Questions de réflexion

| N° | Réponse attendue |
|---|---|
| 1 | Un logiciel malveillant ou une erreur s'exécute avec tous les droits ; moindre privilège |
| 2 | Désactiver d'abord (traçabilité, récupération des fichiers), supprimer plus tard selon la politique de l'entreprise (le délai de 3 à 6 mois de l'énoncé est un exemple, pas une règle) |
| 3 | Active Directory (comptes de domaine centralisés) |
| 4 | Gain de temps, cohérence, moins d'erreurs, évolution facile |
| 5 | **Lecture seule** : le refus d'écriture annule la modification |
| 6 | FAT32 ne gère pas les ACL |
| 7 | Le ransomware chiffre tout ce que l'utilisateur peut modifier ; d'où les sauvegardes hors ligne |

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Comptes et groupes corrects | 5 |
| Permissions conformes à la matrice (captures Sécurité) | 8 |
| Tests des trois comptes | 3 |
| Réflexion | 4 |

## Erreurs fréquentes

- Héritage désactivé mais permissions héritées conservées (au lieu de les supprimer).
- Oublier de laisser `Administrateurs` : le dossier devient inaccessible.
- Donner les droits à des utilisateurs plutôt qu'à des groupes.
- Confondre *Modification* et *Contrôle total*.

## Bilan

À rapprocher des compétences B2.1 à B2.3 et B1.1 du bilan du TP.
