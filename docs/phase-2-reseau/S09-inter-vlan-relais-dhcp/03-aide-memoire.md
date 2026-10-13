# Aide-mémoire – Inter-VLAN et relais DHCP

*BTS SIO SISR 1re année – Bloc 2 – S09*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

```text
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.30.10
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.30.10
interface g0/0
 no shutdown
```

- Le lien routeur-switch est un **trunk** ; la passerelle de chaque VLAN est la sous-interface.
- `ip helper-address` : relaie la diffusion DHCP en **unicast** vers le serveur ; le serveur doit avoir **une étendue par VLAN**.
- Diagnostic : `show ip interface brief` · `show ip route` · `show interfaces trunk`.

**Méthode de panne :** poste → passerelle → autre VLAN → serveur → nom.