# S09 – Routage inter-VLAN, relais DHCP et TP intégré

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S09 (séance 9 sur 19) – Phase 2 |
| **Durée** | 4 heures |
| **Prérequis** | S06 à S08. |
| **Matériel** | Simulateur réseau ou Packet Tracer ; VM Debian (DHCP, DNS) |
| **Compétences travaillées** | B2.1 · B2.2 · B2.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Les VLAN sont isolés, mais le service informatique doit joindre tous les postes et un seul serveur DHCP doit servir tous les réseaux : comment ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Expliquer** pourquoi les VLAN isolent le trafic et pourquoi il faut un routage inter-VLAN ;
2. **Configurer** un routeur sur un bras (sous-interfaces 802.1Q) ;
3. **Configurer** l'agent relais DHCP (`ip helper-address`) ;
4. **Déployer** une infrastructure complète : VLAN, routage, DHCP, DNS ;
5. **Diagnostiquer** méthodiquement une panne multi-services.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Agent relais DHCP](01-cours.md) | Cours |
| [Cours – Routage inter-VLAN (routeur sur un bras)](01b-cours-inter-vlan.md) | Cours (suite) |
| [TP intégré – Deux VLAN, routage, DHCP et DNS](02-tp.md) | TP |
| [TP – Routage inter-VLAN avec un routeur Cisco](02b-tp-inter-vlan.md) | TP (suite) |
| [Annexe – Fiche de configuration inter-VLAN](annexe-fiche-inter-vlan.md) | Annexe |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S06 à S08) | Oral |
| 0 h 20 – 1 h 00 | **Cours** : routage inter-VLAN, relais DHCP | Cours et questions |
| 1 h 00 – 4 h 00 | **TP intégré** (évaluation formative de la phase 2) | Binôme ou trinôme |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S09-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- Le TP intégré peut être noté (évaluation formative) : la grille est à construire à partir des points de contrôle.
- Le TP « routage inter-VLAN » est issu de l'ancienne semaine 14 ; le cours sur le relais DHCP de l'ancienne semaine 10.

</details>
