# Corrigé S13 – Bash, pile LAMP et SSH

*Réservé à l'enseignant. Source : [TP S13](../docs/phase-3-services/S13-bash-lamp-ssh/02-tp.md).*

## Partie A : pile LAMP

| Étape | Attendu |
|---|---|
| Apache | `systemctl status apache2` actif ; page par défaut dans le navigateur |
| MariaDB | `mysql_secure_installation` répondu : mot de passe root, utilisateurs anonymes retirés, root distant interdit, base de test supprimée, privilèges rechargés |
| PHP | `php -v` ; `test.php` affiché puis **supprimé** |
| Base | `siosarl_db`, utilisateur `webuser`@`localhost` avec droits sur cette base uniquement |
| `connexion_bdd.php` | Message de succès vert |

**J'anticipe** (`mysql_secure_installation`) : durcit l'installation par défaut, par exemple mot de passe root, suppression des comptes anonymes, interdiction de la connexion root à distance, suppression de la base de test.

## Partie B : scripts

- `verif_lamp.sh` : `apache2` et `mariadb` actifs ; `php8.2-fpm` affiche `[N/A]` puisque PHP est servi par `libapache2-mod-php` ; le script reste correct.
- `liste_users.sh` : **bogue dans l'énoncé** : `UID` est une variable en lecture seule de bash. `read ... UID ...` affiche `UID: readonly variable`. Faire renommer la variable (`ID_USER`), et idéalement `HOME` et `SHELL` (`REP_HOME`, `SHELL_USER`) pour ne pas écraser des variables d'environnement.

## Partie C : SSH

| Point | Attendu |
|---|---|
| Clés | `id_ed25519` (privée, droits 600) et `id_ed25519.pub` |
| Copie | `ssh-copy-id` puis connexion sans mot de passe |
| `sshd_config` | `PermitRootLogin no`, `PasswordAuthentication no`, `PubkeyAuthentication yes`, `MaxAuthTries 3` |
| Contrôle | `sudo sshd -t` silencieux avant `systemctl restart ssh` |

**J'anticipe** : on risque de se verrouiller dehors. On teste la clé dans une seconde session et on garde la première ouverte.

## Partie D : sauvegarde et cron

| Exigence | Où la vérifier |
|---|---|
| Archive horodatée `.tar.gz` | `ls -lh /var/backups/siosarl/` |
| Rotation à 7 archives | Après plus de 7 exécutions, `ls` en montre 7 |
| Journal horodaté | `/var/log/sauvegarde_siosarl.log` |
| Planification | `sudo crontab -l` : `0 2 * * * /usr/local/bin/siosarl/sauvegarde.sh ...` |
| Test cron | Entrée du journal à l'heure planifiée, puis retour à `0 2` |

**Je comprends** : `30 2 * * 1-5` signifie 2 h 30, du lundi au vendredi. Cron a un environnement minimal (ni répertoire courant ni `PATH`) : il faut des chemins absolus.

## Points d'attention pour l'enseignant

- Le script écrit déjà dans le journal avec `tee -a`, et la ligne cron redirige aussi la sortie vers le même fichier : chaque message apparaît deux fois en exécution planifiée. Accepter, ou retirer la redirection.
- `tar` affiche « Removing leading `/` » : normal, ce n'est pas une erreur.
- Le mot de passe `SioSarl!Web2024` du TP est voulu (labo).
- Le fichier `test.php` doit bien être supprimé (`phpinfo()` expose la configuration).

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| LAMP fonctionnelle (captures A1 à A5) | 5 |
| Scripts Bash corrects | 4 |
| SSH par clés et durcissement | 4 |
| Script de sauvegarde et rotation | 4 |
| Cron et documentation | 3 |

## Erreurs fréquentes

- `PasswordAuthentication no` avant d'avoir testé la clé.
- Droits de `~/.ssh` trop ouverts : la clé est ignorée.
- Utiliser `root` dans l'application au lieu de `webuser`.
- Script non exécutable ou chemin relatif dans le crontab.
- Heure du test cron mal calculée.
