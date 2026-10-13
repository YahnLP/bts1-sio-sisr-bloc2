# S08 – Service DNS (Bind9) et Spanning Tree Protocol

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S08 (séance 8 sur 19) – Phase 2 |
| **Durée** | 4 heures |
| **Prérequis** | S04 à S07. |
| **Matériel** | VM Debian ; simulateur réseau ou Packet Tracer |
| **Compétences travaillées** | B2.2 · B2.3 · B3.4 amorcée (disponibilité) (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Comment les utilisateurs joignent-ils un serveur par son nom, et pourquoi un câble de secours peut-il faire tomber tout le réseau ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Expliquer** la résolution de noms (récursive, itérative) ;
2. **Distinguer** zone directe et zone inverse, **lire** et **créer** des enregistrements (A, PTR, CNAME, NS, SOA) ;
3. **Installer** et **configurer** Bind9 ; **tester** avec `dig`, `nslookup`, `host` ;
4. **Expliquer** le danger des boucles de commutation et le rôle de STP ;
5. **Décrire** l'élection du root bridge et **observer** STP sur trois switchs.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Service DNS et Spanning Tree Protocol](01-cours.md) | Cours |
| [TP – Serveur DNS Bind9](02-tp.md) | TP |
| [TP – Observer le STP sur trois commutateurs](02b-tp-stp.md) | TP (suite) |
| [Aide-mémoire – DNS et STP](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S07) : à quoi sert un serveur DHCP ? et un nom ? | Oral |
| 0 h 20 – 1 h 10 | **Cours** : DNS, STP | Cours et questions |
| 1 h 10 – 2 h 40 | **TP** : serveur DNS Bind9 | Binôme |
| 2 h 40 – 3 h 30 | **TP** : observer STP | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S08-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).


</details>
