# Aide-mémoire – Routage statique et DHCP

*BTS SIO SISR 1re année – Bloc 2 – S07*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

```text
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
ip route 192.168.2.0 255.255.255.0 10.0.0.2
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

`show ip route` · `show ip interface brief` · codes : **C** connecté, **S** statique.

## DHCP : DORA

**D**iscover → **O**ffer → **R**equest → **A**cknowledge.

```text
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
  option domain-name-servers 192.168.10.2;
}
host imprimante { hardware ethernet AA:BB:CC:DD:EE:FF; fixed-address 192.168.10.20; }
```

`sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf` (tester la syntaxe) · `sudo systemctl restart isc-dhcp-server` · baux : `/var/lib/dhcp/dhcpd.leases`