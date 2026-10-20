# Corrigé S10 – Installer un domaine Active Directory

*Réservé à l'enseignant. Source : [TP S10](../docs/phase-3-services/S10-active-directory/02-tp.md).*

## Partie 1 : serveur et promotion

| Point | Attendu | Si ça échoue |
|---|---|---|
| VM | 2 à 4 Go de RAM, ​disque 50 Go, réseau **hôte uniquement** | Pas de lien avec les clients |
| Nom et IP | `SRV-AD-TECHPRO`, `192.168.50.1/24`, DNS `127.0.0.1` | Nom changé sans redémarrage |
| Rôle AD DS | Installé, drapeau jaune « Configuration post-déploiement » | Rôle non installé |
| Promotion | Nouvelle forêt `techpro.local`, DNS et GC cochés, DSRM noté | Avertissement de délégation DNS : normal en TP |
| Connexion | `TECHPRO\Administrateur` | Mot de passe non conforme à la complexité |
| Vérifications | `Get-ADDomain`, `Resolve-DnsName techpro.local`, `Get-ADDomainController -Filter *` | DNS du ​serveur pas sur `127.0.0.1` |

**J'anticipe** : adresse fixe (tous les clients en dépendent) ; DNS du contrôleur sur lui-même (ou un autre DC), car l'annuaire publie ses enregistrements SRV dans le DNS.
**Je comprends** (« domaine introuvable ») : vérifier d'abord que le DNS du poste est celui du contrôleur de domaine, puis `ping`, l'heure du poste, le nom saisi.

## Partie 2 : OU, utilisateurs, groupes

| Contrôle | Attendu |
|---|---|
| Structure | `OU_UTILISATEURS` (Direction, ​Informatique > Réseau / Développement, Commercial, RH), `OU_ORDINATEURS` (3 OU), `OU_GROUPES` |
| Utilisateurs (6) | m.durand (Direction), p.bernard (RH), l.martin (Réseau), e.petit (Développement), s.leroy et d.morel (Commercial) |
| Groupes | GRP_Direction, GRP_RH, GRP_Informatique (l.martin, e.petit), GRP_Commercial (s.leroy, d.morel) |
| Mot de passe | Changement obligatoire à la première connexion |

`Get-ADUser -Filter * | Select Name,SamAccountName,DistinguishedName` doit montrer les bons chemins d'OU.

Les conteneurs `Users` et `Computers` ne sont pas des OU : on ne peut pas y lier de GPO d'où les OU personnalisées.

## Remarques pour l'enseignant

- Mots de passe du TP (`Admin@BTS2024`, `Dsrm@BTS2024`, `Azerty@2024`) : voulus pour le labo.
- Le serveur sert de passerelle dans l'énoncé : simplification de TP, sans routage réel.

## Barème indicatif (20 points)

| Critère | ​Points |
|---|---|
| Serveur préparé (nom, IP, DNS) | 3 |
| Rôle installé et domaine créé | 5 |
| Structure d'OU correcte | 4 |
| Utilisateurs et groupes | 5 |
| Questions et captures | 3 |

## Erreurs fréquentes

- DNS du serveur sur 8.8.8.8 : la promotion ou les vérifications échouent.
- Nom du ​serveur modifié après la promotion.
- Utilisateurs créés dans le conteneur `Users` au lieu des OU.
- Oublier les groupes (créés mais sans membres).
