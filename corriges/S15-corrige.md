# Corrigé S15 – Pare-feu, DMZ et protocoles

*Réservé à l'enseignant. Source : [TP S15](../docs/phase-3-services/S15-pare-feu-dmz-protocoles/02-tp.md).*

## Partie A à B : réseau et serveur web

| Point | Attendu |
|---|---|
| Adressage | FW : `203.0.113.1`, `192.168.10.1`, `192.168.20.1` ; EXT, PC-LAN, SRV-DMZ avec leur passerelle |
| Routage | `net.ipv4.ip_forward=1` (commande **et** fichier `/etc/sysctl.d/`) |
| Sans règle | PC-LAN et SRV-DMZ se joignent : le pare-feu est un routeur qui transmet tout |
| Apache | `curl http://192.168.20.10` depuis PC-LAN affiche la page |

**Je comprends** (`sysctl.d`) : `sysctl -w` ne vaut que pour la session ; le fichier rend le réglage permanent.

## Partie C : matrice des flux

| Source ↓ / Destination → | FW | LAN | DMZ | WAN |
|---|---|---|---|---|
| **LAN** | SSH (22) | – | SSH (22), web (80) | web (80, 443), DNS (53) |
| **DMZ** | rien | **rien** | – | rien |
| **WAN** | rien | rien | web via 8080 (redirigé vers 80) | – |

**J'anticipe** : politique `drop` partout. Sans `ct state established,related accept`, la demande passe mais la réponse est jetée.

## Partie D : règles

Le fichier `nftables.conf` du sujet est la solution. Points à vérifier :

| Élément | Attendu |
|---|---|
| Politique | `policy drop` sur `input` et `forward` |
| États | `established,related accept` avant les autres règles, `invalid drop` |
| SSH vers FW | uniquement depuis l'interface LAN |
| DMZ → LAN | aucune règle d'autorisation |
| NAT | `dnat` en `prerouting`, `masquerade` en `postrouting` |
| Contrôle | `sudo nft -c -f /etc/nftables.conf` silencieux, puis `nft list ruleset` |

Les noms d'interface (`enp0s3`, `enp0s8`, `enp0s9`) sont à adapter à chaque VM.

**Je comprends** :
- `ct status dnat accept` : n'accepte que les paquets redirigés par la règle de NAT ; un paquet direct d'Internet vers le port 80 du serveur reste refusé.
- `policy drop` dans `forward` : tout ce qui traverse sans autorisation explicite est jeté.
- `masquerade` : remplace l'adresse source par celle du pare-feu côté WAN ; les réponses reviennent au pare-feu.
- `dnat to 192.168.20.10:80` : réécrit destination et port ; l'extérieur utilise 8080, le serveur reçoit sur 80.

## Partie E : tests

| N° | Test | Résultat |
|---|---|---|
| 1 | PC-LAN → web de EXT | Autorisé ; EXT voit l'adresse source `203.0.113.1` |
| 2 | PC-LAN → SSH SRV-DMZ | Autorisé |
| 3 | PC-LAN → web SRV-DMZ | Autorisé |
| 4 | EXT → `203.0.113.1:8080` | Autorisé (redirection) |
| 5 | EXT → SRV-DMZ direct | Refusé (délai dépassé) |
| 6 | EXT → PC-LAN (ping) | Refusé |
| 7 | SRV-DMZ → PC-LAN (ping) | Refusé |
| 8 | SRV-DMZ → SSH PC-LAN | Refusé |
| 9 | EXT → SSH du pare-feu | Refusé |

Captures : source vue par EXT = pare-feu (masquerade) ; destination vue par le serveur = `192.168.20.10:80` (dnat) ; le test 7 n'atteint pas le LAN (rien sur `tcpdump -i enp0s8 icmp`).

## Partie F : SFTP restreint

| Point | Attendu |
|---|---|
| Racine de la cage | `/srv/sftp/presta` appartient à `root`, droits `755` ; `depot` appartient à `presta` |
| `sshd_config` | Bloc `Match Group sftpusers` à la **fin** du fichier, `ForceCommand internal-sftp` |
| Contrôle | `sshd -t` silencieux avant `reload` |
| Tests | `put` dans `depot/` réussit ; `cd ..` reste dans la cage ; `ssh presta@...` est refusé (service SFTP uniquement) |

**J'anticipe** : `sshd` refuse une racine modifiable par l'utilisateur (risque d'évasion) ; `ForceCommand internal-sftp` impose le sous-système SFTP, sans shell.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Réseau et routage (A, B) | 3 |
| Matrice des flux (C) | 3 |
| Règles nftables correctes et commentées (D) | 5 |
| Tests prévus puis constatés, captures (E) | 6 |
| SFTP restreint (F) | 3 |

## Erreurs fréquentes

- Appliquer `policy drop` sans règle SSH : perte d'accès à FW.
- Oublier `established,related` : aucune réponse ne revient.
- Noms d'interface copiés tels quels alors qu'ils diffèrent.
- Oublier la route sur EXT pour les tests 5 et 6, ou la laisser et conclure à tort.
- Racine de la cage SFTP modifiable : `sshd` refuse la connexion.
- Mot de passe de `presta` : voulu pour le labo.
