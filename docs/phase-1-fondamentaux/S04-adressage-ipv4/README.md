# S04 – Adressage IPv4, ARP, ICMP, configuration IP et premiers pas sous Linux

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S04 (séance 4 sur 19) – Phase 1 |
| **Durée** | 4 heures |
| **Parcours** | **Cœur** – Adressage IPv4 et VLSM : prérequis de S05, S07, S08, S10 et du projet S17. Voir le [parcours cœur et complément](../../ressources/parcours-coeur-complement.md). |
| **Prérequis** | S03 (modèle OSI, adresse IP et masque) ; conversions binaire/décimal (mathématiques). |
| **Matériel** | VM Windows, VM Debian, Wireshark |
| **Compétences travaillées** | B2.1 · B2.2 · B2.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Comment une machine sait-elle à qui envoyer un paquet, et comment attribuer à chaque poste une adresse cohérente avec son réseau ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Utiliser** la notation CIDR et **calculer** adresse réseau, plage d'hôtes et diffusion ;
2. **Découper** un réseau en sous-réseaux de tailles différentes (VLSM) ;
3. **Expliquer** ARP (adresse MAC ↔ adresse IP) et ICMP (ping, traceroute) ;
4. **Configurer** une adresse IP sous Windows et sous Linux (`ip addr`, `ip route`) ;
5. **Installer** Debian en machine virtuelle et se déplacer en ligne de commande.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Adressage IPv4, ARP et ICMP](01-cours.md) | Cours |
| [Cours – Plan d'adressage VLSM](01b-cours-vlsm.md) | Cours (suite) |
| [TP – Configuration IP sous Windows et sous Linux](02-tp.md) | TP |
| [Aide-mémoire – Adressage IPv4](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S03) : une adresse, un masque, une passerelle | Oral |
| 0 h 20 – 1 h 15 | **Cours** : CIDR, ARP, ICMP, VLSM | Cours et questions |
| 1 h 15 – 3 h 30 | **TP** : IP sous Windows, installation de Debian, IP sous Linux | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S04-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- Le calcul de sous-réseaux est enseigné en mathématiques ; ici, on l'applique à un plan d'adressage.

</details>
