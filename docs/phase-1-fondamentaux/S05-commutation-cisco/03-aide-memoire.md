# Aide-mémoire – Commutation et CLI Cisco

*BTS SIO SISR 1re année – Bloc 2 – S05*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Modes de la CLI

| Invite | Mode | Entrer |
|---|---|---|
| `Switch>` | Utilisateur | — |
| `Switch#` | Privilégié | `enable` |
| `Switch(config)#` | Configuration globale | `configure terminal` |
| `Switch(config-if)#` | Interface | `interface g0/1` |

## Configuration de base

```text
hostname SW1
banner motd #Accès réservé#
enable secret MotDePasse
line console 0
 password cisco
 login
copy running-config startup-config
```

## Vérifier

`show running-config` · `show mac address-table` · `show interfaces status` · `show version`

**Rappel :** tant qu'on n'a pas fait `copy running-config startup-config`, la configuration est perdue au redémarrage.