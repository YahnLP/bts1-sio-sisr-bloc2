# Corrigé S19 – Épreuve blanche E6

*Réservé à l'enseignant. Source : [sujet S19](../docs/phase-4-securiser-presenter/S19-epreuve-blanche-e6/02-sujet.md). L'épreuve n'a pas de réponse ​unique : ce document guide l'observation.*

## Cadre

Épreuve d'entraînement, **non officielle**. La grille ci-dessous reprend celle du sujet (outil de formation) ; les grilles nationales restent celles de la circulaire d'organisation. Niveaux : non maîtrisé, maîtrise partielle, bonne maîtrise, excellente maîtrise.

## Ce qu'on attend pour chaque critère

| Critère | Bonne ​maîtrise | Signaux d'alerte |
|---|---|---|
| Réalité de la réalisation | VM ou situation d'entreprise, preuves datées, démonstration en direct | Captures non datées, schéma sans machine derrière |
| Conception | Besoin reformulé, adressage cohérent, ​choix justifiés (« pourquoi pas autre chose ? ») | Choix non argumenté, plan qui ne tient pas la croissance |
| Mise en œuvre | Configuration expliquée ligne par ligne, tests menés, démonstration qui réussit | Lecture des diapositives, démonstration non répétée |
| ​Exploitation et sécurité | Moindre privilège, filtrage, sauvegarde testée, supervision, documentation | Mots de passe par défaut, sauvegarde jamais restaurée |
| Démarche | Méthode de ​diagnostic, erreurs assumées, limites reconnues | Aucune limite citée |
| Communication | Plan clair, schéma lisible, temps tenu (15 min + 5 min de démonstration) | Dépassement, lecture du support |

## Éléments de réponse aux questions types

| Question | Éléments attendus |
|---|---|
| Plan d'adressage, doublement des postes | ​Marge dans chaque bloc, réserve dans le /22, possibilité de réattribuer les blocs |
| Preuve du fonctionnement | Plan de test, captures datées, démonstration |
| Panne DHCP / DNS / contrôleur de domaine | Conséquences par service, redondance possible, procédure de rétablissement |
| Flux entre zones | Matrice des flux, politique par ​défaut `drop`, justification de chaque ouverture |
| Erreurs de manipulation, compte compromis | Moindre privilège, groupes, GPO, complexité, verrouillage, journalisation |
| Part personnelle | ​Tâches précises et traçables (dépôt de configurations, journal de bord) |
| Améliorations | Haute disponibilité, supervision, sauvegarde hors site, sécurisation des accès |

## Conduite de l'épreuve

- Préparation : 1 h 30, seul, avec dossier et poste de démonstration.
- Oral : 40 ​minutes (15 de présentation, environ 5 de démonstration, 20 d'échange).
- Relever les **preuves** vues et les **questions** posées ; remplir ensuite le [bilan de positionnement](../docs/phase-4-securiser-presenter/S19-epreuve-blanche-e6/annexe-bilan-competences.md) avec l'étudiant : les niveaux sont comparés à vos observations.

## Barème indicatif (20 points, usage formatif)

| Critère | Points |
|---|---|
| Réalité et preuves | 4 |
| Conception | 4 |
| Mise en œuvre et démonstration | 4 |
| Exploitation et sécurité | 3 |
| ​Démarche | 2 |
| Communication | 3 |

Cette note est un outil de formation et ne préjuge pas de la note de l'épreuve nationale.
