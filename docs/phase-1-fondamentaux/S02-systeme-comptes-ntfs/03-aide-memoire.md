# Aide-mémoire – Système, comptes et NTFS

*BTS SIO SISR 1re année – Bloc 2 – S02*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Comptes (Windows)

| Action | Invite d'administrateur (cmd) |
|---|---|
| Créer un compte | `net user alice MotDePasse /add` |
| Ajouter à un groupe | `net localgroup Administrateurs alice /add` |
| Lister les comptes | `net user` |
| Désactiver | `net user alice /active:no` |

## Droits NTFS

| Droit | Permet |
|---|---|
| Lecture | Voir le contenu |
| Écriture | Créer, modifier |
| Modification | Lecture, écriture, suppression |
| Contrôle total | Tout, dont changer les droits |

- **Moindre privilège** : donner uniquement ce qui est nécessaire.
- Les droits se donnent à des **groupes**, pas à des personnes.
- Un **refus** l'emporte sur une autorisation.
- Onglet *Sécurité → Avancé → Accès effectif* pour vérifier.