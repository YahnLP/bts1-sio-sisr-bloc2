# S03 – Les fondamentaux du réseau : modèle OSI, TCP/IP, diagnostic

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S03 (séance 3 sur 19) – Phase 1 |
| **Durée** | 4 heures |
| **Prérequis** | S01 (connectique RJ45). |
| **Matériel** | Postes Windows, simulateur réseau, Wireshark |
| **Compétences travaillées** | B2.1 amorcée · B2.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Un poste n'accède plus au serveur : par où commence-t-on, et comment parler d'un problème réseau sans se tromper de couche ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Décrire** les composants d'un réseau (carte réseau, switch, routeur) et les **types** de réseaux (PAN, LAN, MAN, WAN) ;
2. **Expliquer** le modèle OSI (7 couches) et le modèle TCP/IP (4 couches) ;
3. **Lire** une adresse IPv4 et son masque ;
4. **Utiliser** `ping`, `ipconfig`, `tracert`, `nslookup` pour diagnostiquer ;
5. **Capturer** quelques trames avec Wireshark et **repérer** les adresses MAC et IP.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Les fondamentaux du réseau](01-cours.md) | Cours |
| [TP – Diagnostiquer un réseau : commandes et analyse de trames](02-tp.md) | TP |
| [Aide-mémoire – Bases du réseau](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 15 | **Accroche** : « le réseau ne marche pas » | Oral |
| 0 h 15 – 1 h 30 | **Cours** : OSI, TCP/IP, types de réseaux, adresse IPv4 | Cours et questions |
| 1 h 30 – 3 h 30 | **TP** : commandes de diagnostic et première capture Wireshark | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S03-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- La partie Wireshark du TP est ajoutée par la refonte (l'analyseur de trames fait partie des attendus de l'annexe II.E).

</details>
