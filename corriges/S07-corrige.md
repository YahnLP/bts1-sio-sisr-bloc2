# Corrigé S07 – Relier deux réseaux par un routeur

*Réservé à l'enseignant. Source : [TP S07](../docs/phase-2-reseau/S07-routage-statique-dhcp/02-tp.md).*

## Configuration attendue

```cisco
! R1_INFO
interface FastEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
interface Serial0/0/0
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000      ! côté DCE seulement
 no shutdown
ip route 192.168.20.0 255.255.255.0 10.0.0.2

! R2_DIR
interface FastEthernet0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown
interface Serial0/0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

Le côté DCE est identifié par `show controllers serial 0/0/0`. Le câble est connecté sur le routeur qui reçoit `clock rate`.

## Grille de validation

| Critère | ​Attendu |
|---|---|
| `show ip interface brief` | Fa0/0 et Se0/0/0 en **up/up** sur les deux routeurs |
| `show ip route` R1 | `C` pour 192.168.10.0/24 et 10.0.0.0/30, `S 192.168.20.0/24 via 10.0.0.2` |
| `show ip route` R2 | `S 192.168.10.0/24 via 10.0.0.1` |
| Ping PC-INFO-01 → PC-DIR-01 | 4/4, le premier peut échouer (ARP) |
| `traceroute 192.168.20.11` depuis R1 | 2 sauts : `10.0.0.2` puis le PC |

## Questions

- **J'anticipe** : seules les routes connectées (`C`) ; le réseau derrière R2 est inconnu sans route statique (`S`).
- **Je comprends** (« ​délai dépassé » malgré la route de R1) : la route **de retour** manque sur R2, ou la passerelle du PC est fausse ; un ping est un aller-retour.

## Diagnostic des pannes simulées

| Symptôme | Cause probable |
|---|---|
| Se0/0/0 « down/down » | Câble série absent ou `no shutdown` oublié |
| Se0/0/0 « up/down » | `clock rate` manquant côté DCE |
| Ping PC → ​passerelle KO | Mauvaise passerelle ou masque sur le PC |
| Ping PC local OK, distant KO | Route statique absente d'un des deux routeurs |

## TP complémentaire : serveur DHCP sous Linux

*Source : [TP 02b](../docs/phase-2-reseau/S07-routage-statique-dhcp/02b-tp-dhcp-linux.md).*

| Étape | Attendu |
|---|---|
| Installation | `isc-dhcp-server` installé ; le premier `status` affiche `failed` (normal, pas encore configuré) |
| Interface | `INTERFACESv4="<nom réel>"` dans `/etc/default/isc-dhcp-server` (nom relevé avec `ip a`) |
| `dhcpd.conf` | `authoritative`, sous-réseau `192.168.10.0/24`, plage `.100` à `.150`, `routers 192.168.10.1`, bail 600 s, réservation `PC-Imprimante` en `192.168.10.10` |
| Contrôle | `dhcpd -t -cf /etc/dhcp/dhcpd.conf` sans erreur, puis `active (running)` |
| Client | Adresse ​entre `.100` et `.150`, passerelle `.1` ; bail visible dans `/var/lib/dhcp/dhcpd.leases` |

**J'anticipe** (pas d'option `routers`) : le client obtient adresse et masque mais **pas de passerelle** : il joint son réseau local, pas les autres.

Pannes fréquentes : `INTERFACESv4` vide ou erroné (le service ne démarre pas) ; serveur sans adresse fixe dans le sous-réseau servi (`192.168.10.1/24`) ; MAC de la réservation non remplacée ; client et serveur sur des ​réseaux VirtualBox différents.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Câblage et adressage | 4 |
| Configuration des deux routeurs | 7 |
| Routes statiques dans les deux sens | 3 |
| Tests et grille de validation | 4 |
| Sauvegarde et réponses | 2 |

## Erreurs fréquentes

- Route ​statique sur un seul routeur.
- Prochain saut égal à sa propre adresse.
- Masque `/30` mal posé (`255.255.255.252`).
