# Aide-mémoire – Adressage IPv4

*BTS SIO SISR 1re année – Bloc 2 – S04*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Calculs

- Adresses de l'hôte : `2^(bits hôte) − 2` (on retire l'adresse réseau et la diffusion).
- `/24` = `255.255.255.0` ; `/25` = `.128` ; `/26` = `.192` ; `/27` = `.224` ; `/28` = `.240` ; `/30` = `.252`.
- **Plages privées :** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
- `169.254.x.x` : pas de DHCP (APIPA). `127.0.0.1` : le poste lui-même.

## Configuration

| Système | Commande |
|---|---|
| Linux, voir | `ip -br a`, `ip route` |
| Linux, ajouter une adresse | `sudo ip addr add 192.168.1.10/24 dev enp0s3` |
| Linux, route par défaut | `sudo ip route add default via 192.168.1.1` |
| Windows | `ipconfig`, `netsh interface ip show config` |
| ARP | `ip neigh` (Linux), `arp -a` (Windows) |

## VLSM

On attribue d'abord les **plus grands** sous-réseaux, puis les plus petits, sans chevauchement.