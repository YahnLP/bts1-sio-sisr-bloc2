# S05 – Commutation, ligne de commande Cisco et commandes Linux

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S05 (séance 5 sur 19) – Phase 1 |
| **Durée** | 4 heures |
| **Prérequis** | S04 (adressage, ARP) ; terminal Linux de base. |
| **Matériel** | Simulateur réseau ou Packet Tracer ; VM Debian |
| **Compétences travaillées** | B2.2 · B2.3 (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Comment un switch sait-il vers quel port envoyer une trame, et comment le configurer proprement sans souris ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Expliquer** l'apprentissage de la table MAC, le renvoi et l'inondation ;
2. **Distinguer** domaine de collision et domaine de diffusion, switch et hub ;
3. **Naviguer** dans les modes de la ligne de commande Cisco ;
4. **Configurer** un switch (nom, bannière, mots de passe, sauvegarde) et **vérifier** avec `show` ;
5. **Utiliser** les commandes Linux de gestion de fichiers.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Commutation, CLI Cisco et commandes Linux](01-cours.md) | Cours |
| [TP – Configurer un commutateur Cisco](02-tp.md) | TP |
| [Aide-mémoire – Commutation et CLI Cisco](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 20 | **Rappel d'amorçage** (S04) et accroche : le switch est-il intelligent ? | Oral |
| 0 h 20 – 1 h 20 | **Cours** : commutation, CLI Cisco | Cours et questions |
| 1 h 20 – 3 h 30 | **TP** : configurer un switch ; commandes Linux | Binôme |
| 3 h 30 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S05-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- **Corrigé :** dans `corriges/` à la racine du dépôt (hors du site publié).
- Packet Tracer n'est pas redistribuable : le simulateur réseau du site peut le remplacer pour les manipulations de base.

</details>
