# Corrigé S16 – Supervision, sauvegarde et haute disponibilité

*Réservé à l'enseignant. Source : [TP S16](../docs/phase-4-securiser-presenter/S16-supervision-sauvegarde-ha/02-tp.md).*

## Partie A à B : répartiteur HAProxy

| Point | Attendu |
|---|---|
| Pages | `curl` sur WEB1 puis WEB2 : chacun renvoie son nom |
| ​Contrôle | `haproxy -c -f /etc/haproxy/haproxy.cfg` valide avant le redémarrage |
| 6 requêtes via LB | WEB1, WEB2 en alternance (`roundrobin`) |
| Page `:8404/stats` | Deux serveurs en vert |
| Panne de WEB1 | Toutes les réponses viennent de WEB2 après quelques secondes |

Par défaut, HAProxy teste toutes les 2 s et retire un serveur après 3 échecs : prévoir une interruption ​perçue de l'ordre de quelques secondes (quelques lignes `ECHEC` sont normales). Le retour du serveur demande quelques tests réussis. Les valeurs sont **à ​mesurer**, pas à imposer.

**Je comprends** :
- `option httpchk GET /` : HAProxy envoie lui-même un `GET /` à intervalle régulier ; un serveur qui ne répond pas correctement sort de la rotation.
- Deux serveurs arrêtés : réponse `503 Service Unavailable`.
- Point unique de défaillance : **LB** (à doubler avec une adresse IP flottante, VRRP/keepalived) ; aussi SUP pour la supervision et les sauvegardes.

## Partie C : Zabbix

| Étape | Attendu |
|---|---|
| Base | `zabbix` ​créée en `utf8mb4`, schéma importé, `log_bin_trust_function_creators` remis à 0 |
| Serveur | `DBPassword` renseigné, services redémarrés et activés |
| Interface | Assistant terminé ; mot de ​passe du compte `Admin` **changé** |
| Agents | `Server`, `ServerActive`, `Hostname` identiques au nom de l'hôte déclaré |
| Hôtes | LB, WEB1, WEB2 avec le modèle *Linux by Zabbix agent* |
| Sonde | `net.tcp.service[http,,80]` toutes les 10 s : `1` si le service répond, `0` sinon |
| Déclencheur | `last(/web1/net.tcp.service[http,,80])=0`, gravité Haute |
| Panne | Problème `Apache arrêté sur web1` visible dans Surveillance > Problèmes, puis résolu au redémarrage |

**J'anticipe** (agent ​arrêté) : le modèle contient un déclencheur du type « Zabbix agent is not available », qui se déclenche après quelques minutes sans réponse (délai paramétrable). Les commandes d'installation dépendent de la version : se référer à la page officielle.

**Je comprends** :
- Élément = mesure ​collectée ; déclencheur = condition d'anomalie ; action = qui est prévenu et comment.
- Un serveur répond au ping alors que son service est arrêté : seule la sonde du service reflète ce que vivent les utilisateurs.
- Le délai évite les fausses ​alertes, qui finissent ignorées.

## Partie D : sauvegarde et restauration

| Point | Attendu |
|---|---|
| Clé SSH | WEB1 accède à SUP sans mot de passe |
| Script | Archive de `/var/www` et `/etc/apache2`, copie par `rsync`, suppression locale, journal `logger` |
| Cron | `*/15 * * * *` |
| Sinistre | `rm -rf /var/www/html/*` : le site ne répond plus |
| Restauration | Archive extraite dans `/tmp/restauration`, contenu vérifié (`var/www/html/...`), puis copie en place ; heures notées |

**J'anticipe** : on perd 10 minutes de modifications ; au pire 15 ​minutes, donc **RPO garanti = 15 minutes**.

**Je comprends** :
- Restaurer d'abord dans un dossier de test : vérifier que l'archive est lisible et complète sans écraser la production, et mesurer le temps.
- 3-2-1 : non ; une seule copie, sur un seul support, sans copie hors site.
- Pas de montage en lecture-écriture : un rançongiciel chiffrerait aussi la sauvegarde.

## Partie E : synthèse (RPO 1 h, RTO 15 min)

Réponse type attendue :
- La haute disponibilité des ​serveurs web tient le RTO (quelques secondes).
- Le RPO de 15 min tient l'exigence de 1 h.
- Le RTO d'une **restauration complète** doit être mesuré : s'il dépasse 15 min, le client n'est pas servi.
- Au moins deux changements parmi : doubler LB (keepalived), ​copie hors site, sauvegarde de la base de données, alertes par courriel, test de restauration régulier.

## Points d'attention pour l'enseignant

- Le script est lancé par `sudo crontab -e` donc **par root** : la clé SSH doit être celle de root (`sudo ssh-keygen`, `sudo ssh-copy-id`), comme le précise maintenant le TP. La restauration utilise `sudo scp` pour la même raison.
- Mots de passe `MotDePasseTP` : voulus (labo).

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Haute disponibilité (configuration, mesure) | 5 |
| Supervision (hôtes, sonde, déclencheur, ​preuve) | 5 |
| Sauvegarde et restauration prouvée | 6 |
| Tableau de synthèse et réponses | 4 |

## Erreurs fréquentes

- Hostname de l'agent différent du nom de l'hôte dans Zabbix.
- Pare-feu ou service Zabbix non démarré.
- Sauvegarde jamais restaurée, donc non comptée.
- Clé SSH d'un utilisateur ordinaire ​alors que cron s'exécute en root : le `rsync` réussit à la main et échoue en cron.
- Archive restaurée directement sur `/` au lieu d'un dossier de test.
