# Corrigé S09 – Deux VLAN, routage, DHCP et DNS

*Réservé à l'enseignant. Source : [TP S09](../docs/phase-2-reseau/S09-inter-vlan-relais-dhcp/02-tp.md).*

## Points de contrôle

| Partie | Commande | Attendu |
|---|---|---|
| A switch | `show vlan brief` | VLAN 10 (Fa0/1-5), 20 (Fa0/6-10), 30 (Fa0/11-15) |
| A switch | `show interfaces trunk` | Fa0/24 trunk, VLAN 10,20,30 autorisés |
| B routeur | `show ip interface brief` | Fa0/0, Fa0/0.10, .20, .30 en up/up avec leurs IP |
| B routeur | `show running-config` | `encapsulation dot1Q` et `ip helper-address 192.168.30.10` sur .10 et .20, **pas** sur .30 |
| C DHCP | `dhcpd -t -cf ...` puis `systemctl status isc-dhcp-server` | Syntaxe valide, service actif, 3 `subnet` |
| D DNS | `dig @127.0.0.1 ns1.infra.local` et `dig -x 192.168.30.10` | Bonnes réponses, flag `aa` |

## Tableau de validation

| Test | Attendu |
|---|---|
| PC VLAN 10 | IP `192.168.10.100-200`, passerelle `.10.1`, DNS `192.168.30.10` |
| PC VLAN 20 | IP `192.168.20.100-180` |
| Ping inter-VLAN | 4/4 grâce au routage sur sous-interfaces |
| Baux | Entrées dans `/var/lib/dhcp/dhcpd.leases` |

## Questions

- **Je comprends** : la demande DHCP est une diffusion que le routeur ne transmet pas ; `ip helper-address` la convertit en unicast vers le serveur, sur **chaque** sous-interface cliente.
- **J'anticipe** (pas d'adresse) : 1) port client en access VLAN 10 ; 2) trunk vers le routeur autorisant le VLAN 10 ; 3) sous-interface avec `encapsulation dot1Q 10` et `ip helper-address` ; 4) serveur DHCP avec une étendue pour `192.168.10.0/24` et service actif.

## Points d'attention pour l'enseignant

- Le service `isc-dhcp-server` n'écoute que sur les interfaces déclarées (`INTERFACESv4` dans `/etc/default/isc-dhcp-server`) : le TP n'en parle pas, c'est la panne la plus courante.
- Le nom d'interface réseau (`eth0`) varie : vérifier avec `ip a`.
- Le serveur DHCP est une VM et les clients sont dans Packet Tracer : prévoir la liaison entre les deux (ou un DHCP du simulateur). Sans cela, la partie E ne peut pas aboutir.
- Le routeur doit être sauvegardé (`copy run start`).

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Switch : VLAN et trunk | 4 |
| Routeur : sous-interfaces et relais | 5 |
| Serveur DHCP | 4 |
| Serveur DNS | 3 |
| Tests finaux, tableau complété | 4 |

## Erreurs fréquentes

- `no shutdown` oublié sur l'interface physique.
- `encapsulation dot1Q` manquante ou numéro de VLAN faux.
- `ip helper-address` sur une seule sous-interface.
- Plage DHCP qui chevauche des adresses fixes.
- Passerelle du serveur absente : les réponses ne reviennent pas.
