# S07 – Routage statique et service DHCP

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S07 (séance 7 sur 19) – Phase 2 |
| **Durée** | 4 heures |
| **Prérequis** | S04, S06 (adressage, VLAN) ; terminal Linux. |
| **Matériel** | Simulateur réseau ou Packet Tracer ; VM Debian |
| **Compétences travaillées** | B2.2 · B2.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Deux réseaux doivent se parler, et les postes doivent recevoir leur adresse automatiquement : que configurer, et où ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Lire** une table de routage Cisco et **distinguer** routes connectées et statiques ;
2. **Configurer** une interface de routeur et des **routes statiques** (dont la route par défaut) ;
3. **Tester** la connectivité avec `ping` et `show ip route` ;
4. **Expliquer** les quatre étapes de DHCP (DORA) ;
5. **Installer** et **configurer** un serveur DHCP sous Debian : étendue, options, réservation.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Routage statique et service DHCP](01-cours.md) | Cours |
| [TP – Relier deux réseaux par un routeur](02-tp.md) | TP |
| [TP – Serveur DHCP sous Linux](02b-tp-dhcp-linux.md) | TP (suite) |
| [Aide-mémoire – Routage statique et DHCP](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S06) et accroche : comment sortir de son réseau ? | Oral |
| 0 h 20 – 1 h 10 | **Cours** : routage statique, DHCP | Cours et questions |
| 1 h 10 – 2 h 20 | **TP** : relier deux réseaux par un routeur | Binôme |
| 2 h 20 – 3 h 30 | **TP** : serveur DHCP sous Linux | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S07-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.


</details>
