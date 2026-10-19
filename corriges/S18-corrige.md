# Corrigé S18 – Tests de l'infrastructure et rédaction du DAT

*Réservé à l'enseignant. Source : [TP S18](../docs/phase-4-securiser-presenter/S18-dossier-architecture/02-tp.md) et [modèle de DAT](../docs/phase-4-securiser-presenter/S18-dossier-architecture/annexe-modele-dat.md).*

## Partie A : correction et tests

**J'anticipe** : on note le résultat réel (échec), on corrige, on refait le test et on note le nouveau résultat. Un plan honnête garde la trace des échecs.

Critères d'évaluation du plan de tests :

| Critère | Attendu |
|---|---|
| Exhaustivité | Les 22 tests du tableau 1.C exécutés, un par service ou flux du cahier des charges (réseau, DHCP, DNS, annuaire, GPO, partages, FTP, SFTP) |
| Preuves | Capture numérotée par test |
| Honnêteté | Échecs conservés, cause identifiée, correction et nouveau test |
| Cohérence | Résultats cohérents avec les captures P1 à P7 de S17 |

## Partie B : DAT

Vérifier la présence **dans l'ordre** :

| Section | Contrôle |
|---|---|
| Page de garde | Titre, auteurs, date, version 1.0, tableau des versions |
| Sommaire | Numéros de page |
| Contexte et objectifs | 10 à 15 lignes, périmètre inclus/exclu |
| Architecture globale | Schéma d'ensemble, inventaire matériel et logiciel |
| Architecture réseau | Plan VLSM cohérent avec S17, schéma légendé, configurations en annexe |
| Architecture système | Rôles Windows justifiés, AD/OU/groupes, DHCP, DNS, partages ; Linux FTP/SFTP |
| Sécurité | Tableau des GPO, politique d'accès aux fichiers, droits FTP/SFTP, 3 à 5 préconisations |
| Procédures | Création d'utilisateur (5 à 8 étapes), vérification des services, redémarrage après coupure |
| Plan de tests | Tableau complété avec résultats |
| Annexes | CLI Cisco, scripts PowerShell, fichiers Linux, captures numérotées |

La procédure EXP-01 du sujet est un modèle : elle doit se retrouver adaptée, avec prérequis, durée, étapes et vérification.

## Points d'attention pour l'enseignant

- Les références **1.A** (checklist), **1.B** (causes probables) et **1.C** (22 tests) sont dans le TP. La colonne « Résultat attendu » du tableau 1.C sert de clé de correction.
- Les adresses de 1.C sont celles du plan VLSM du [corrigé S17](S17-corrige.md) ; adapter si l'étudiant a un autre plan cohérent.
- Un DAT qui documente des limites connues vaut mieux qu'un DAT qui les cache.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Corrections et plan de tests exécuté | 5 |
| Structure et complétude du DAT | 5 |
| Schémas exploitables par un tiers | 3 |
| Procédures d'exploitation utilisables | 4 |
| Qualité rédactionnelle, version, annexes | 3 |

## Erreurs fréquentes

- Schéma sans légende ni adressage.
- Configurations collées sans explication des choix.
- Plan de tests sans résultat réel ni capture.
- Procédure qui suppose des connaissances non écrites.
- Incohérence entre le plan d'adressage du DAT et celui de S17.
