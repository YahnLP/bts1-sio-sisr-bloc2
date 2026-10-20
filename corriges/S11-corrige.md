# Corrigé S11 – GPO et DNS Windows

*Réservé à l'enseignant. Source : [TP S11](../docs/phase-3-services/S11-gpo-dns-windows/02-tp.md).*

## Partie 1 : GPO

| GPO | Liée à | Réglages attendus | Vérification |
|---|---|---|---|
| `GPO_Securite_Comptes` | `OU_UTILISATEURS` | ​Historique 5, durée max 90 j, min 1 j, longueur 10, complexité activée ; verrouillage 5 essais / 15 min | `gpresult /r` |
| `GPO_Fond_Ecran_Commercial` | `Commercial` | Papier peint activé, chemin UNC SYSVOL, style Ajuster | Fond imposé sur `s.leroy` |
| `GPO_Lecteur_H` | `OU_UTILISATEURS` | Préférences > Lecteur mappé H:, `\\SRV-AD-TECHPRO\Users\%LogonUser%`, action Mettre à jour | `net use` sur `l.martin` |

**J'anticipe** (mot de ​passe) : la règle de mot de passe de **domaine** se définit au niveau du domaine (`Default Domain Policy`), pas sur une OU ; la GPO liée à l'OU n'a donc pas l'effet attendu sur les comptes du domaine. Les GPO sont aussi appliquées par cycle ; `gpupdate /force` les applique immédiatement.
**Je comprends** (fond d'écran non reçu) : `gpresult /r` (ou `/h rapport.html`) sur le poste, `gpupdate /force`, puis déconnexion/connexion ; vérifier le lien, le filtrage et l'accès au chemin UNC.

## Partie 2 : DNS Windows

| Point | ​Attendu |
|---|---|
| Zone `techpro.local` | Type Intégré Active Directory ; mises à jour dynamiques sécurisées uniquement |
| Enregistrements A | `www` .100, `intranet-srv` .101, `nas` .10, `srv-fichier` .20 |
| CNAME | `intranet` → `www.techpro.local.` |
| Zone inverse | `50.168.192.in-addr.arpa`, PTR pour le serveur |
| `Resolve-DnsName intranet.techpro.local` | Montre le CNAME puis l'adresse de `www` |
| ​Résolution inverse | `192.168.50.100` → `www.techpro.local` |

**J'anticipe** (zone inverse) : elle permet de retrouver un nom à partir d'une adresse (journaux, outils de diagnostic, certains services) ; sans elle, `nslookup adresse` échoue ou ​renvoie une erreur.

## Points d'attention pour l'enseignant

- La mise en pratique de la GPO de mot de passe sert à faire constater la limite : demander à l'étudiant de comparer avec la `Default Domain Policy`.
- Le partage `Users` ajoute à la fois `-ChangeAccess` et `-ReadAccess` pour `Domain Users` : l'accès effectif est la Modification.
- `%LogonUser%` ​fonctionne dans les préférences de lecteur mappé ; une variable différente ne sera pas résolue.
- Sans VM cliente, les captures `gpresult` sont celles du serveur : barème adapté.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| GPO mot de passe correcte et liée | 4 |
| GPO fond d'écran | 3 |
| GPO lecteur réseau (partage + préférence) | 4 |
| DNS : A, CNAME, zone inverse | 5 |
| Tests, captures, réponses | 4 |

## Erreurs fréquentes

- GPO ​créée mais non liée.
- GPO utilisateur liée à une OU contenant des ordinateurs (ou l'inverse).
- Image du fond d'écran inaccessible pour l'utilisateur (chemin local).
- CNAME sans point final dans la cible.
- Zone inverse avec un ID réseau mal saisi.
