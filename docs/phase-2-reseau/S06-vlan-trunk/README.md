# S06 – VLAN, trunk 802.1Q et utilisateurs Linux

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S06 (séance 6 sur 19) – Phase 2 |
| **Durée** | 4 heures |
| **Prérequis** | S05 (CLI Cisco, table MAC). |
| **Matériel** | Simulateur réseau ou Packet Tracer ; VM Debian |
| **Compétences travaillées** | B2.1 · B2.2 · B2.3 · B3.3 (segmentation) (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Comment séparer les services d'une entreprise (direction, commercial, informatique) sur les mêmes switchs ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Expliquer** l'intérêt des VLAN pour la segmentation et la sécurité ;
2. **Distinguer** port access et port trunk, **expliquer** le marquage 802.1Q et le VLAN natif ;
3. **Créer** des VLAN, **affecter** des ports, **configurer** un trunk ;
4. **Vérifier** avec `show vlan` et `show interfaces trunk` ;
5. **Gérer** utilisateurs, groupes et permissions sous Linux.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – VLAN, trunk 802.1Q et utilisateurs Linux](01-cours.md) | Cours |
| [TP – VLAN et trunk entre deux commutateurs](02-tp.md) | TP |
| [TP – Utilisateurs, groupes et permissions sous Linux](02b-tp-linux.md) | TP (suite) |
| [Aide-mémoire – VLAN et trunk](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S05) : domaine de diffusion | Oral |
| 0 h 20 – 1 h 10 | **Cours** : VLAN, trunk, configuration Cisco | Cours et questions |
| 1 h 10 – 2 h 40 | **TP** : 3 VLAN et un trunk | Binôme |
| 2 h 40 – 3 h 30 | **TP** : utilisateurs, groupes, permissions Linux | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S06-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).


</details>
