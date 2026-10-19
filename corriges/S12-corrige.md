# Corrigé S12 – DHCP Windows, droits NTFS et DFS

*Réservé à l'enseignant. Source : [TP S12](../docs/phase-3-services/S12-dhcp-windows-ntfs-dfs/02-tp.md).*

## Partie A : DHCP

| Point | Attendu |
|---|---|
| Serveur | IP statique, rôle DHCP installé, post-installation terminée |
| Étendue `Réseau_Principal` | `192.168.10.20` à `.200`, masque `255.255.255.0`, bail 8 jours |
| Exclusion | `.20` à `.30` : la plage distribuée commence à `.31` |
| Options | 003 routeur `192.168.10.1`, 006 DNS `192.168.10.10`, 015 domaine `siosarl.local` |
| Client | Après `ipconfig /release` puis `/renew` : IP entre `.31` et `.200`, passerelle, DNS corrects, bail visible dans **Baux d'adresses** |
| Réservation | `PC-Directeur` → `192.168.10.50` (MAC de la VM cliente) |

**J'anticipe** : dans un domaine Active Directory, le serveur DHCP doit être **autorisé** dans l'annuaire ; sinon il reste inactif.

## Partie B : arborescence et droits

| Dossier | Droits NTFS attendus |
|---|---|
| RH | SYSTEM et Administrateurs : contrôle total ; GRP_RH : modifier ; GRP_Direction : lecture et exécution ; GRP_Informatique : contrôle total |
| Informatique | SYSTEM, Administrateurs, GRP_Informatique : contrôle total |
| Direction | SYSTEM, Administrateurs, GRP_Direction, GRP_Informatique : contrôle total |
| Commun | SYSTEM, Administrateurs, GRP_Informatique : contrôle total ; GRP_Tous_Salaries : modifier |

Partage : `Tout le monde` en contrôle total ; les droits fins sont portés par NTFS.

Tests : `alice.martin` accède à RH, refusée sur Direction ; `claire.dir` accède à Direction, lit RH sans pouvoir y créer de fichier.

**J'anticipe** (partage en lecture, NTFS en modification) : non, le droit effectif sur le réseau est le **plus restrictif** des deux : lecture seule. En local sur le serveur, la modification serait possible.

## Partie C : DFS

| Point | Attendu |
|---|---|
| Espace de noms | `data`, basé sur le domaine si l'AD est présent |
| Dossiers DFS | RH, Informatique, Direction, Commun, avec leur cible `\\Serveur\Partage` |
| Test | `\\siosarl.local\data\RH` et `\Commun` affichent les mêmes fichiers que le partage direct |

**Je comprends** : le chemin DFS ne change pas si l'on déplace le dossier vers un autre serveur ; on modifie la cible et les utilisateurs ne voient rien. DFS peut aussi proposer plusieurs cibles.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| DHCP : étendue, exclusion, options, réservation | 6 |
| Groupes, arborescence, droits NTFS | 6 |
| Partages et tests d'accès | 4 |
| DFS | 3 |
| Fiches et réponses | 1 |

## Erreurs fréquentes

- Étendue créée mais non activée ; serveur DHCP non autorisé dans AD.
- Exclusion mal saisie : le client reçoit `.20` ou `.21`.
- Héritage désactivé avec suppression au lieu de « Convertir » : perte d'accès pour les administrateurs.
- Droits donnés à des utilisateurs plutôt qu'à des groupes.
- Test depuis le serveur et non depuis le client : le droit local masque la restriction du partage.

## Remarque

Le bilan mentionne un relais DHCP pour plusieurs sous-réseaux, notion vue en S09 et non manipulée dans ce TP.
