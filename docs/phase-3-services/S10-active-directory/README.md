# S10 – Windows Server et Active Directory

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S10 (séance 10 sur 19) – Phase 3 |
| **Durée** | 4 heures |
| **Prérequis** | S02 (comptes), S04 (adressage), S08 (DNS). |
| **Matériel** | VirtualBox, ISO Windows Server 2022, VM cliente Windows 10/11 |
| **Compétences travaillées** | B2.1 · B2.2 · B2.3 · B3.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« 50 salariés, 50 postes : comment centraliser les comptes pour ne pas créer 50 comptes locaux sur chaque machine ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Choisir** une édition de Windows Server et **l'installer** en VM ;
2. **Configurer** nom, adresse IP fixe et mises à jour ;
3. **Expliquer** la structure d'Active Directory (forêt, domaine, OU) et ses objets ;
4. **Promouvoir** un contrôleur de domaine et **joindre** un poste ;
5. **Concevoir** et **créer** une structure d'OU, des utilisateurs et des groupes.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Windows Server et Active Directory](01-cours.md) | Cours |
| [TP – Installer un domaine Active Directory](02-tp.md) | TP |
| [Aide-mémoire – Active Directory](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S02, S08) et accroche : l'annuaire | Oral |
| 0 h 20 – 1 h 10 | **Cours** : Windows Server, Active Directory | Cours et questions |
| 1 h 10 – 3 h 30 | **TP** : installer un domaine, créer les OU et les comptes | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** et première fiche de réalisation (R1) | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S10-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- Les mots de passe des comptes du TP sont des identifiants de laboratoire communs : choix pédagogique assumé en environnement contrôlé. Ne pas les reprendre dans une réalisation présentée à l'épreuve.

</details>
