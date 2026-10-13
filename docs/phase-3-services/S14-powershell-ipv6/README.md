# S14 – PowerShell, Active Directory en masse et IPv6

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S14 (séance 14 sur 19) – Phase 3 |
| **Durée** | 4 heures |
| **Prérequis** | S10 à S12 ; S13 (notion de script). |
| **Matériel** | VM Windows Server (domaine), VM cliente IPv6 |
| **Compétences travaillées** | B2.2 · B2.3 · B3.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Trente nouveaux comptes à créer avant lundi : comment les créer en une minute plutôt qu'en une journée ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Utiliser** les cmdlets fondamentales et le pipeline PowerShell ;
2. **Manipuler** des objets (`Select-Object`, `Where-Object`) ;
3. **Importer** un CSV et **créer** des utilisateurs AD en masse avec `New-ADUser` ;
4. **Gérer** les erreurs avec `Try/Catch` ;
5. **Décrire** une adresse IPv6, ses types (lien local, globale) et SLAAC.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – PowerShell, Active Directory et IPv6](01-cours.md) | Cours |
| [TP – PowerShell : utilisateurs AD en masse et IPv6](02-tp.md) | TP |
| [Annexe – Fiche de documentation](annexe-fiche-documentation.md) | Annexe |
| [Annexe – Fichier CSV des nouveaux employés](annexe-fichier-csv.md) | Annexe |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 15 | **Rappel d'amorçage** (S10, S13) | Oral |
| 0 h 15 – 1 h 15 | **Cours** : PowerShell, AD, IPv6 | Cours et questions |
| 1 h 15 – 3 h 30 | **TP** : PowerShell et création en masse ; exercices IPv6 | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S14-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).


</details>
