# Aide-mémoire – Bash, LAMP, SSH, cron

*BTS SIO SISR 1re année – Bloc 2 – S13*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

```bash
#!/bin/bash
read -p "Nom : " nom
if [ -z "$nom" ]; then echo "Vide"; exit 1; fi
for i in 1 2 3; do echo "$nom $i"; done
```

| Besoin | Syntaxe |
|---|---|
| Tester un fichier | `[ -f fichier ]` |
| Comparer des nombres | `[ $a -gt $b ]` |
| Code de retour | `$?` |
| Boucle lecture | `while read ligne; do ...; done < fichier` |

## SSH par clés

`ssh-keygen -t ed25519` · `ssh-copy-id user@hôte` · dans `/etc/ssh/sshd_config` : `PasswordAuthentication no` puis `sudo sshd -t && sudo systemctl reload ssh`.

## cron

`crontab -e` — `minute heure jour mois jour_semaine commande` — `0 2 * * * /usr/local/bin/sauvegarde.sh`.