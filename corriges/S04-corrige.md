# Corrigé S04 – Configuration IP sous Windows et Linux

*Réservé à l'enseignant. Source : [TP S04](../docs/phase-1-fondamentaux/S04-adressage-ipv4/02-tp.md).*

## Partie 1 : Windows

| Test | Résultat attendu |
|---|---|
| `ipconfig /all` | IP `192.168.50.XX`, masque `255.255.255.0`, passerelle `192.168.50.1`, DNS `8.8.8.8` |
| `ping 127.0.0.1` et `ping` de sa propre IP | Réussissent (pile locale) |
| `ping` voisin | Réussit si même plage et pare-feu Windows autorisant l'ICMP |
| `arp -a` | IP du voisin, MAC du voisin, type **dynamique** |
| `ping` passerelle / Internet | Échec normal si la passerelle `.50.1` n'existe pas dans la salle |

**J'anticipe** (IP `192.168.1.20/24`, passerelle `192.168.2.1`) : la passerelle n'est pas dans le réseau du poste ; le poste ne sort pas de `192.168.1.0/24`.

**Piège** : un `ping` voisin qui échoue alors que l'IP est correcte vient souvent du pare-feu Windows (profil réseau public), pas de l'adressage.

## Partie 2 : Debian

| Point | Attendu |
|---|---|
| VM | 1 à 2 Go de RAM, disque 10 Go, hostname `debian-tp` |
| Logiciels | Tout décoché sauf utilitaires usuels (pas d'interface graphique) |
| Connexion | Prompt `utilisateur@debian-tp:~$` (le `$` signale un compte non root) |

## Partie 3 : commandes

| Question | Commande | Valeur |
|---|---|---|
| Répertoire courant | `pwd` | `/home/<utilisateur>` |
| Utilisateur | `whoami` | nom du compte créé |
| Noyau | `uname -r` | propre à la VM (série 6.1 pour Debian 12) |
| RAM / disque | `free -h`, `df -h` | propres à la VM |
| Hostname | `cat /etc/hostname` | `debian-tp` |

**Exercice 2** : nom d'interface variable (`enp0s3` sous VirtualBox, `ens33` ailleurs). Si la commande `sudo` est refusée, l'utilisateur n'est pas dans le groupe `sudo` : utiliser `su -`. Le `ping` vers sa propre adresse fonctionne même sans réseau.

**Je comprends** : `ip addr add` ajoute l'adresse pour la session en cours ; elle disparaît au redémarrage. Pour la rendre permanente : `/etc/network/interfaces` (Debian) puis `systemctl restart networking`.

## Remarques pour l'enseignant

- Le tableau « Pourquoi Linux » du TP contient des pourcentages **sans source** : ne pas les faire apprendre.
- Les mots de passe `Azerty123!` sont voulus (labo).

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Configuration IP Windows et tests | 6 |
| Installation Debian et connexion | 5 |
| Exercices 1 et 2 | 5 |
| « J'anticipe » / « Je comprends » | 4 |

## Erreurs fréquentes

- Passerelle hors du réseau du poste.
- Confondre `ip addr add` (temporaire) et le fichier de configuration (permanent).
- Oublier `-c 4` avec `ping` sous Linux : il tourne sans fin (`Ctrl+C`).
- Mot de passe saisi « dans le vide » sous Linux : normal, rien ne s'affiche.
