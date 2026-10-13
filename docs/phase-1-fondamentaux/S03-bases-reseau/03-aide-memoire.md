# Aide-mémoire – Bases du réseau

*BTS SIO SISR 1re année – Bloc 2 – S03*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Modèle OSI

| N° | Couche | Unité | Exemples |
|---|---|---|---|
| 7 | Application | Données | HTTP, DNS, DHCP |
| 4 | Transport | Segment | TCP, UDP |
| 3 | Réseau | Paquet | IP, ICMP |
| 2 | Liaison | Trame | Ethernet, adresse MAC |
| 1 | Physique | Bit | Câble, fibre, Wi-Fi |

(Couches 5 et 6 : session et présentation.)

## Commandes de diagnostic

| Commande | Rôle |
|---|---|
| `ipconfig /all` | Configuration IP, MAC, DNS, DHCP |
| `ping hôte` | Joignabilité et latence |
| `tracert hôte` | Chemin et routeurs traversés |
| `nslookup nom` | Résolution DNS |
| `netstat -an` | Connexions et ports en écoute |

**Dépanner de proche en proche** : `127.0.0.1` → passerelle → `8.8.8.8` → un nom.