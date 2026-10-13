# S12 – DHCP Windows, partages de fichiers (NTFS) et DFS

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S12 (séance 12 sur 19) – Phase 3 |
| **Durée** | 4 heures |
| **Prérequis** | S10, S11 ; S07 (DHCP). |
| **Matériel** | VM Windows Server (domaine), VM cliente |
| **Compétences travaillées** | B2.2 · B2.3 · B3.3 · B1.5 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Chaque service a son dossier partagé, et seuls ses membres y accèdent : comment le construire, et que se passe-t-il quand droits NTFS et droits de partage se contredisent ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Installer** le rôle DHCP, **créer** une étendue, des options et une réservation ;
2. **Configurer** un relais DHCP pour plusieurs sous-réseaux ;
3. **Combiner** droits NTFS et droits de partage (le plus restrictif l'emporte) ;
4. **Construire** une arborescence par service avec des groupes de sécurité ;
5. **Créer** un espace de noms DFS.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – DHCP Windows Server, partages NTFS et DFS](01-cours.md) | Cours |
| [TP – DHCP, partages et DFS sous Windows Server](02-tp.md) | TP |
| [Annexe – Fiche de configuration DHCP](annexe-fiche-dhcp.md) | Annexe |
| [Annexe – Fiche de documentation des droits](annexe-fiche-droits.md) | Annexe |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S07, S10, S11) | Oral |
| 0 h 20 – 1 h 10 | **Cours** : DHCP Windows, NTFS et partage, DFS | Cours et questions |
| 1 h 10 – 3 h 30 | **TP** : DHCP, partages, DFS | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** ; fiches de configuration à rendre | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S12-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- Séance chargée : la partie DFS peut être traitée en démonstration.

</details>
