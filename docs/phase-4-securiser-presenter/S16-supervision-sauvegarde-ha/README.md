# S16 – Supervision, sauvegarde et haute disponibilité

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S16 (séance 16 sur 19) – Phase 4 |
| **Durée** | 4 heures |
| **Prérequis** | S13 (cron, SSH), S15. |
| **Matériel** | 3 VM Debian (serveur, serveur de secours, superviseur) |
| **Compétences travaillées** | B2.3 · B3.4 · B3.5A (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Un serveur tombe un lundi matin : comment l'avoir su avant les utilisateurs, comment le remettre en route, comment éviter que ça s'arrête ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Distinguer** supervision, sauvegarde et haute disponibilité ;
2. **Superviser** un service avec alertes (sondes et seuils) ;
3. **Mettre en place** une sauvegarde 3-2-1 et **tester une restauration** ;
4. **Calculer** RPO et RTO pour une situation donnée ;
5. **Répartir** la charge entre deux serveurs web et **observer** la bascule en cas de panne.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Supervision, sauvegarde et haute disponibilité](01-cours.md) | Cours |
| [TP – Superviser, sauvegarder et rendre un site web tolérant aux pannes](02-tp.md) | TP |
| [Aide-mémoire – Supervision, sauvegarde, HA](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S13, S15) et accroche : la panne du lundi | Oral |
| 0 h 20 – 1 h 10 | **Cours** : supervision, sauvegarde, RPO/RTO, haute disponibilité | Cours et questions |
| 1 h 10 – 3 h 30 | **TP** : superviser, sauvegarder/restaurer, répartir la charge | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S16-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- Séance **ajoutée par la refonte** (supervision avec alertes, tolérance de panne et répartition de charge : annexe II.E). Contenu à valider.

</details>
