# S11 – Stratégies de groupe (GPO) et DNS intégré à Active Directory

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S11 (séance 11 sur 19) – Phase 3 |
| **Durée** | 4 heures |
| **Prérequis** | S10. |
| **Matériel** | VM Windows Server (domaine S10), VM cliente |
| **Compétences travaillées** | B2.2 · B2.3 · B3.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Comment imposer à tous les comptes une politique de mots de passe, un fond d'écran et un lecteur réseau, sans passer sur chaque poste ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Expliquer** le rôle des GPO et l'ordre d'application (LSDOU) ;
2. **Créer** et **lier** une GPO avec la console de gestion ;
3. **Configurer** une GPO de sécurité et une GPO d'environnement ;
4. **Forcer** et **diagnostiquer** avec `gpupdate /force` et `gpresult /r` ;
5. **Gérer** une zone DNS intégrée à AD (enregistrements A, CNAME, zone inverse).

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Stratégies de groupe (GPO) et DNS intégré à AD](01-cours.md) | Cours |
| [TP – GPO et DNS Windows](02-tp.md) | TP |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S10) | Oral |
| 0 h 20 – 1 h 10 | **Cours** : GPO, DNS intégré à AD | Cours et questions |
| 1 h 10 – 3 h 30 | **TP** : trois GPO puis DNS Windows | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S11-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).


</details>
