# Aide-mémoire – Supervision, sauvegarde, HA

*BTS SIO SISR 1re année – Bloc 2 – S16*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Sauvegarde 3-2-1

3 copies · 2 supports · 1 hors site · **restauration testée**.

## RPO et RTO

- **RPO** = perte de données maximale admise (≈ intervalle entre deux sauvegardes).
- **RTO** = durée maximale d'interruption admise (≈ temps de bascule ou de restauration).

## Commandes

| Besoin | Commande |
|---|---|
| Archive | `tar czf sauv.tar.gz /var/www` |
| Extraire | `tar xzf sauv.tar.gz -C /tmp/test` |
| Copie distante | `rsync -a fichier user@hôte:/chemin/` |
| Syntaxe HAProxy | `sudo haproxy -c -f /etc/haproxy/haproxy.cfg` |
| État d'un service | `systemctl status apache2` |

**Supervision :** hôte → sonde → seuil → déclencheur → alerte.