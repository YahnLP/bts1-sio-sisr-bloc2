# Corrigé S03 – Diagnostiquer un réseau

*Réservé à l'enseignant. Source : [TP S03](../docs/phase-1-fondamentaux/S03-bases-reseau/02-tp.md).*

## Valeurs propres à chaque poste

Les tableaux des parties A à C dépendent du réseau de la salle : ​vérifier la **cohérence**, pas des valeurs fixes.

| Contrôle | Cohérence attendue |
|---|---|
| Passerelle | Dans le même réseau que l'IP du poste (masque appliqué) |
| IPv4 en `169.254.x.x` | Réponse APIPA : DHCP injoignable, à signaler |
| MAC | Format `xx-xx-xx-xx-xx-xx` relevé avec `ipconfig /all` |
| DNS / DHCP | Adresses présentes, durée du bail notée |

## Partie B : ping de proche en proche

| Étape | Teste | En cas d'échec |
|---|---|---|
| 1 `127.0.0.1` | Pile réseau du poste | Pilote ou système |
| 2 ​passerelle | Liaison locale | Câble, switch, adresse |
| 3 `8.8.8.8` | Sortie Internet sans DNS | Box ou fournisseur |
| 4 nom de domaine | Résolution DNS | Si 3 réussit : **DNS** |

Le TTL est un compteur décrémenté à chaque routeur ; le temps est la latence aller-retour.

## Partie C

- `* * *` : pas de réponse du routeur, pas forcément une panne.
- `LISTENING` : un service attend des ​connexions ; chaque port ouvert est une porte d'entrée.
- Connexion web : port distant 443, port local éphémère (haut numéro).

## Partie D : Wireshark

| Couche | Champ | Attendu |
|---|---|---|
| ​Ethernet II | MAC destination | **MAC de la passerelle** |
| IP | IP destination | IP de la cible du ping |
| ICMP | Type | 8 (requête), 0 (réponse) |

- ARP en **diffusion** (`ff:ff:ff:ff:ff:ff`) : l'IP est connue, pas la MAC.
- Couches : Ethernet = 2 ; IP, ICMP = 3 ; UDP = 4 ; DNS = 7.
- La requête DNS précède le trafic web : il faut l'adresse IP avant d'envoyer.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Parties A à C complètes et cohérentes | 8 |
| Captures datées requête/réponse ICMP et ARP | 6 |
| Réponses « J'anticipe » / « Je comprends » | 4 |
| Respect des consignes (capture limitée, ​rendu nommé) | 2 |

## Erreurs fréquentes

- Confondre MAC de la passerelle et MAC du serveur final.
- Conclure à une panne devant `* * *`.
- Capturer longtemps ou partager une capture avec des données personnelles.
- Lancer `arp -d` sans invite administrateur.
