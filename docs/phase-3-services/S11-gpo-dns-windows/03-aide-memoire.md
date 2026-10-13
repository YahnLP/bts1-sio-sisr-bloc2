# Aide-mémoire – GPO et DNS Windows

*BTS SIO SISR 1re année – Bloc 2 – S11*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

- Ordre d'application : **L**ocal, **S**ite, **D**omaine, **OU** (la dernière gagne).
- Console : `gpmc.msc`. Lier une GPO à une **OU**, jamais à un poste.

| Commande | Rôle |
|---|---|
| `gpupdate /force` | Forcer l'application |
| `gpresult /r` | Voir les GPO appliquées au poste |
| `Get-GPO -All` | Lister les GPO (PowerShell) |

## DNS Windows

Zone **intégrée à AD** : réplication par l'annuaire. `Add-DnsServerResourceRecordA` · `nslookup` · `Resolve-DnsName`.