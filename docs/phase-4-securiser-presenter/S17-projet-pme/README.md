# S17 – Projet : l'infrastructure de la PME SimIO

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S17 (séance 17 sur 19) – Phase 4 |
| **Durée** | 4 heures |
| **Parcours** | **Cœur** – Projet PME : assemble S04 à S15 ; prépare l'épreuve E6. Voir le [parcours cœur et complément](../../ressources/parcours-coeur-complement.md). |
| **Prérequis** | S04 à S15. |
| **Matériel** | Simulateur réseau ou Packet Tracer, ou matériel réel (au choix de l'enseignant) pour le réseau ; VM Windows Server et Debian (services) |
| **Compétences travaillées** | B2.1 · B2.2 · B2.3 · B3.3 · B1.4 (mode projet) (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Une PME de 6 services vous confie son infrastructure : de l'adressage à l'annuaire, livrez un réseau qui fonctionne et que quelqu'un d'autre saura exploiter. »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Concevoir** un plan d'adressage VLSM et une architecture VLAN par service ;
2. **Déployer** le routage inter-VLAN, l'annuaire, les GPO, DHCP, DNS et les partages ;
3. **Sécuriser** l'accès d'un serveur de fichiers (SFTP) ;
4. **Documenter** en continu la configuration réalisée.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Projet – Infrastructure de la PME SimIO](02-projet.md) | Projet |
| [Aide-mémoire – Projet PME](03-aide-memoire.md) | Aide-mémoire |
| [Annexe – Fiche de documentation technique du projet](annexe-documentation.md) | Annexe |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 30 | **Lancement** : cahier des charges, répartition en binômes | Collectif |
| 0 h 30 – 3 h 30 | **Projet** : phases 1 à 4 (adressage, réseau, Windows, services) | Binôme ou trinôme |
| 3 h 30 – 4 h 00 | **Bilan d'étape** : ce qui marche, ce qui reste | Collectif |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S17-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- Le projet se poursuit en S18 (tests et DAT). Le TP GLPI/OCS de l'ancienne semaine 17 relève du bloc 1 et n'est plus dans ce dépôt.

</details>
