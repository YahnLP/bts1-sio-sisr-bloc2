# S15 – Pare-feu, NAT, DMZ et transfert de fichiers sécurisé

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S15 (séance 15 sur 19) – Phase 3 |
| **Durée** | 4 heures |
| **Prérequis** | S07 (routage), S09 (inter-VLAN), S13 (SSH). |
| **Matériel** | 3 VM : pare-feu/routeur Debian, serveur en DMZ, poste interne ; Wireshark |
| **Compétences travaillées** | B2.1 · B2.2 · B2.3 · B3.3 · B3.5A (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Un serveur web doit être joignable depuis Internet sans exposer le réseau interne : comment séparer les zones et filtrer ce qui entre ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Expliquer** le rôle d'un pare-feu et la différence entre filtrage sans état et avec état ;
2. **Concevoir** un réseau à trois zones (interne, DMZ, externe) ;
3. **Écrire** des règles de filtrage avec nftables et les **tester** ;
4. **Configurer** un NAT (partage d'accès et redirection de port) ;
5. **Déployer** un accès SFTP restreint à un dossier ;
6. **Vérifier** avec Wireshark qu'une règle fait ce qu'on croit.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Pare-feu, NAT et DMZ](01-cours.md) | Cours |
| [Cours – Transfert de fichiers : FTP et SFTP](01c-cours-ftp-sftp.md) | Cours complémentaire |
| [Cours – Protocoles applicatifs : HTTP, messagerie, DNS](01d-cours-protocoles.md) | Cours complémentaire |
| [TP – Un pare-feu à trois zones et un serveur web en DMZ](02-tp.md) | TP |
| [Aide-mémoire – Pare-feu, NAT et DMZ](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S07, S09, S13) et accroche : que voit-on d'Internet ? | Oral |
| 0 h 20 – 1 h 10 | **Cours** : pare-feu, NAT, DMZ | Cours et questions |
| 1 h 10 – 3 h 10 | **TP** : pare-feu et DMZ sous Linux | Binôme |
| 3 h 10 – 3 h 40 | **TP** : SFTP restreint | Binôme |
| 3 h 40 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S15-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- Séance **ajoutée par la refonte** (l'annexe II.E exige plusieurs périmètres de sécurité, un analyseur de trames, une DMZ). Contenu à valider.
- Les cours FTP/SFTP et protocoles applicatifs (HTTP, messagerie, DNS) sont conservés en cours complémentaires.

</details>
