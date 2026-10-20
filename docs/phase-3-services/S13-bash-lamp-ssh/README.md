# S13 – Scripts Bash, serveur LAMP, administration à distance (SSH) et cron

| Élément | Détail |
|---|---|
| **Niveau** | BTS SIO option SISR, 1re année – bloc 2 |
| **Séance** | S13 (séance 13 sur 19) – Phase 3 |
| **Durée** | 4 heures |
| **Prérequis** | S05, S06 (terminal et utilisateurs Linux). |
| **Matériel** | VM Debian (serveur) et une seconde VM (client SSH) |
| **Compétences travaillées** | B2.2 · B2.3 · B3.3 (administration sécurisée) (voir [le référentiel](../../ressources/referentiel.md)) |

> [!NOTE]
> **Question qui guide la séance :** *« Comment automatiser la création de 30 comptes, héberger un site web et administrer un serveur sans quitter son bureau ? »*

## Objectifs

À la fin de la séance, l'étudiant est capable de :

1. **Écrire** un script Bash : variables, saisie, conditions, boucles ;
2. **Installer** et **vérifier** une pile LAMP (Apache, MariaDB, PHP) et **sécuriser** MariaDB ;
3. **Configurer** SSH par clés et **désactiver** l'authentification par mot de passe ;
4. **Écrire** un script de sauvegarde et **le planifier** avec cron.

## Contenu de la séance

| Document | Rôle |
|---|---|
| [Cours – Serveur LAMP, boucles Bash, SSH et cron](01-cours.md) | Cours |
| [Cours – Scripts Bash : les bases](01b-cours-bash-bases.md) | Cours (suite) |
| [TP – LAMP, SSH par clés et sauvegarde avec cron](02-tp.md) | TP |
| [TP – Premiers scripts Bash](02b-tp-bash.md) | TP (suite) |
| [Aide-mémoire – Bash, LAMP, SSH, cron](03-aide-memoire.md) | Aide-mémoire |

## Déroulé conseillé (4 h)

| Durée | Activité | Format |
|---|---|---|
| 0 h 00 – 0 h 15 | **Rappel d'amorçage** (S05, S06) | Oral |
| 0 h 15 – 1 h 15 | **Cours** : bases du scripting, LAMP, SSH, cron | Cours et questions |
| 1 h 15 – 2 h 15 | **TP** : premiers scripts Bash | Binôme |
| 2 h 15 – 3 h 40 | **TP** : LAMP, SSH par clés, sauvegarde cron | Binôme |
| 3 h 40 – 4 h 00 | **Bilan de compétences** | Individuel |

## Retour en entreprise

Si ta situation le permet : **observer** comment ton entreprise traite le sujet de la séance, **documenter** en une demi-page l'écart avec le TP, ou **réaliser** une partie après accord de ton tuteur. Voir [Transfert en entreprise](../../ressources/transfert-entreprise.md). Le travail réalisé peut alimenter une [fiche de réalisation](../../ressources/fiche-realisation-professionnelle.md).

## Évaluation

- Compte rendu avec captures d'écran datées, schéma et explications, nommé `RENDU-B2-S13-NOM.pdf`.
- Bilan de compétences rempli à la fin de la séance (individuel).

<details markdown="1">
<summary>Notes pour l'enseignant</summary>

- **Compétences :** codes relus sur le référentiel (page [Référentiel](../../ressources/referentiel.md)) ; le choix des compétences pour cette séance est une proposition à valider.
- Séance très chargée : fusion de contenus de deux anciennes semaines. Les scripts sont à valider sur une VM propre avant la séance.

</details>
