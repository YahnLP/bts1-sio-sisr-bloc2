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

## Remarque

La séance s'intitule « DNS et STP » mais le TP ne porte que sur le DNS ; la partie STP est traitée dans le cours.
