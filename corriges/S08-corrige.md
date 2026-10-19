# Corrigé S08 – Serveur DNS Bind9

*Réservé à l'enseignant. Source : [TP S08](../docs/phase-2-reseau/S08-dns-stp/02-tp.md).*

## Points de contrôle

| Étape | Attendu | Si ça échoue |
|---|---|---|
| Installation | `systemctl status bind9` : active (running) ; port 53 en écoute (`ss -tlnup \| grep 53`) | Paquet absent, service arrêté |
| `named-checkconf` | Aucune sortie | Point-virgule ou accolade manquant dans `named.conf.*` |
| `named-checkzone` | `loaded serial 2024112001` puis `OK` | Nom de zone ou FQDN sans point final |
| Zone inverse | Nom de zone `10.168.192.in-addr.arpa` (octets inversés) | Zone écrite à l'endroit |
| Client | `resolv.conf` pointe sur `127.0.0.1` | Fichier régénéré automatiquement |

## Résultats des tests

| Commande | Réponse attendue |
|---|---|
| `dig @127.0.0.1 www.techpro.local A` | `192.168.10.50`, flag `aa` |
| `dig @127.0.0.1 web.techpro.local` | CNAME vers `www`, puis `192.168.10.50` |
| `dig @127.0.0.1 -x 192.168.10.50` | `www.techpro.local.` (PTR) |
| `dig @127.0.0.1 www.google.fr` | Réponse via les forwarders, flags `ra` sans `aa` |

Flags : `qr` = réponse ; `aa` = le serveur fait autorité ; `rd` = récursion demandée ; `ra` = récursion disponible.

## Questions

- **J'anticipe** (série non incrémenté) : le secondaire compare les numéros de série ; identiques, il ne recopie pas la modification.
- **Je comprends** : `dig nom` interroge la zone directe (enregistrement A) ; `dig -x adresse` interroge la zone inverse (enregistrement PTR).

## TP complémentaire : observer le STP sur trois commutateurs

*Source : [TP 02b](../docs/phase-2-reseau/S08-dns-stp/02b-tp-stp.md).*

| Étape | Attendu |
|---|---|
| Câblage | SW1 Fa0/1-SW2 Fa0/1, SW1 Fa0/2-SW3 Fa0/1, SW2 Fa0/2-SW3 Fa0/2 (le schéma du TP est cohérent avec cette liste) |
| Priorité | 32769 sur les trois (32768 + numéro de VLAN 1) |
| Root bridge | Le switch à la **plus petite adresse MAC** (priorités égales) : tous ses ports sont *Desg / FWD* |
| Switchs non racines | Un *Root / FWD* vers le root bridge |
| Lien entre les deux non racines | Un seul port bloqué (*Altn / BLK*), sur le switch de **plus grand** identifiant de pont |
| Ping | PC1 vers PC2 et PC3 répondent par le chemin actif |

Reconvergence : l'ordre de grandeur est de 30 à 50 s (écoute puis apprentissage). Le port bloqué ne passe en *Root / FWD* que si le lien retiré faisait partie du chemin actif ; sinon rien ne change : faire argumenter l'étudiant à partir de son propre tableau.

Étape 6 : `spanning-tree vlan 1 priority 4096` sur SW3 ; `show spanning-tree` affiche « This bridge is the root ». La priorité est un multiple de 4096 (le reste est occupé par le numéro de VLAN dans l'identifiant de pont).

**J'anticipe** : le switch à la plus petite MAC est le root bridge ; **un seul** lien est bloqué dans un triangle.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Installation et options globales | 3 |
| Zone directe valide (`named-checkzone`) | 5 |
| Zone inverse valide | 4 |
| Tests `dig`/`nslookup`/`host` commentés | 5 |
| Réponses de réflexion | 3 |

## Erreurs fréquentes

- FQDN sans point final : `ns1.techpro.local` est interprété comme `ns1.techpro.local.techpro.local.`.
- Série non incrémentée après modification.
- Redémarrer Bind sans avoir validé les zones.
- Confondre CNAME (alias d'un nom) et A (adresse).
- `allow-recursion` trop ouvert : un résolveur public peut servir à des attaques.
