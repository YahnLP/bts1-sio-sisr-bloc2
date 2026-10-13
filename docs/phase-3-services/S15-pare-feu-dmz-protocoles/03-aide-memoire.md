# Aide-mémoire – Pare-feu, NAT et DMZ

*BTS SIO SISR 1re année – Bloc 2 – S15*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

```text
ct state established,related accept
iifname "enp0s8" oifname "enp0s9" tcp dport { 22, 80 } accept
iifname "enp0s3" tcp dport 8080 dnat to 192.168.20.10:80
oifname "enp0s3" masquerade
```

| Besoin | Commande |
|---|---|
| Vérifier la syntaxe | `sudo nft -c -f /etc/nftables.conf` |
| Appliquer | `sudo nft -f /etc/nftables.conf` |
| Voir les règles | `sudo nft list ruleset` |
| Activer le routage | `sudo sysctl -w net.ipv4.ip_forward=1` |
| Voir un flux | `sudo tcpdump -i enp0s3 -n tcp port 80` |
| Tester un port | `nc -vz hôte 80` |

**Politique `drop` par défaut**, matrice des flux avant les règles, chaque règle testée.