# Aide-mémoire – DNS et STP

*BTS SIO SISR 1re année – Bloc 2 – S08*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Enregistrements

| Type | Rôle |
|---|---|
| A | Nom → IPv4 |
| AAAA | Nom → IPv6 |
| CNAME | Alias vers un autre nom |
| PTR | IP → nom (zone inverse) |
| NS | Serveur de noms de la zone |
| MX | Serveur de messagerie |
| SOA | Début de zone (serveur principal, numéro de série) |

## Tests

`dig @serveur nom` · `dig -x adresse` · `nslookup nom serveur` · `named-checkconf` · `named-checkzone zone fichier`

**Penser à incrémenter le numéro de série** de la zone après chaque modification.

## STP

Élection du **root bridge** : plus petite priorité, puis plus petite adresse MAC. Ports : *root*, *designated*, *blocked*. `show spanning-tree`.