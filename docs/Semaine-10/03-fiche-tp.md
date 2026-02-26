---
author: YLP
title: 🖥️ FICHE DE TP
---


# 🖥️ TP 

### "Infrastructure Complète 2 VLANs + Routage + DHCP + DNS"

> **Note :** Ce TP est la **répétition générale** avant l'évaluation. L'enseignant peut guider et répondre aux questions. Les apprenants peuvent s'entraider. Durée : 60 minutes.

---

### Plan d'Adressage du TP Intégré

| **Réseau** | **VLAN** | **Adresse réseau** | **Passerelle** | **Plage DHCP** |
|------------|----------|-------------------|----------------|----------------|
| Informatique | VLAN 10 | 192.168.10.0/24 | 192.168.10.1 | .100 → .200 |
| Direction | VLAN 20 | 192.168.20.0/24 | 192.168.20.1 | .100 → .180 |
| Serveurs | VLAN 30 | 192.168.30.0/24 | 192.168.30.1 | (pas de DHCP) |
| Serveur Linux | – | 192.168.30.10 | 192.168.30.1 | IP fixe |

**Noms DNS (`infra.local`) :**
| **Nom** | **IP** | **Type** |
|---------|--------|---------|
| ns1.infra.local | 192.168.30.10 | A |
| srv-dhcp.infra.local | 192.168.30.10 | A (alias) |
| gw.infra.local | 192.168.30.1 | A |
| intranet.infra.local | 192.168.10.50 | A |

---

### Partie A – Configuration du Switch (Packet Tracer, ~15 min)

```cisco
! Accès CLI du switch
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW_ACCÈS

! Créer les VLANs
SW_ACCÈS(config)# vlan 10
SW_ACCÈS(config-vlan)# name Informatique
SW_ACCÈS(config-vlan)# exit

SW_ACCÈS(config)# vlan 20
SW_ACCÈS(config-vlan)# name Direction
SW_ACCÈS(config-vlan)# exit

SW_ACCÈS(config)# vlan 30
SW_ACCÈS(config-vlan)# name Serveurs
SW_ACCÈS(config-vlan)# exit

! Affecter les ports aux VLANs (mode access)
SW_ACCÈS(config)# interface range FastEthernet0/1 - 5
SW_ACCÈS(config-if-range)# switchport mode access
SW_ACCÈS(config-if-range)# switchport access vlan 10
SW_ACCÈS(config-if-range)# exit

SW_ACCÈS(config)# interface range FastEthernet0/6 - 10
SW_ACCÈS(config-if-range)# switchport mode access
SW_ACCÈS(config-if-range)# switchport access vlan 20
SW_ACCÈS(config-if-range)# exit

SW_ACCÈS(config)# interface range FastEthernet0/11 - 15
SW_ACCÈS(config-if-range)# switchport mode access
SW_ACCÈS(config-if-range)# switchport access vlan 30
SW_ACCÈS(config-if-range)# exit

! Configurer le port trunk vers le routeur
SW_ACCÈS(config)# interface FastEthernet0/24
SW_ACCÈS(config-if)# switchport mode trunk
SW_ACCÈS(config-if)# switchport trunk allowed vlan 10,20,30
SW_ACCÈS(config-if)# exit

! Sauvegarder
SW_ACCÈS(config)# end
SW_ACCÈS# copy running-config startup-config

! Vérifier
SW_ACCÈS# show vlan brief
SW_ACCÈS# show interfaces trunk
```

✅ **Validation Étape A :** `show vlan brief` montre les 3 VLANs avec leurs ports. `show interfaces trunk` montre Fa0/24 en trunk avec les 3 VLANs autorisés.

---

### Partie B – Configuration du Routeur (Packet Tracer, ~15 min)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R1_INFRA
R1_INFRA(config)# no ip domain-lookup

! Activer l'interface physique (sans IP)
R1_INFRA(config)# interface FastEthernet0/0
R1_INFRA(config-if)# no shutdown
R1_INFRA(config-if)# exit

! Sous-interface VLAN 10 (Informatique)
R1_INFRA(config)# interface FastEthernet0/0.10
R1_INFRA(config-subif)# encapsulation dot1Q 10
R1_INFRA(config-subif)# ip address 192.168.10.1 255.255.255.0
R1_INFRA(config-subif)# ip helper-address 192.168.30.10   ! Agent relais DHCP
R1_INFRA(config-subif)# exit

! Sous-interface VLAN 20 (Direction)
R1_INFRA(config)# interface FastEthernet0/0.20
R1_INFRA(config-subif)# encapsulation dot1Q 20
R1_INFRA(config-subif)# ip address 192.168.20.1 255.255.255.0
R1_INFRA(config-subif)# ip helper-address 192.168.30.10   ! Agent relais DHCP
R1_INFRA(config-subif)# exit

! Sous-interface VLAN 30 (Serveurs)
R1_INFRA(config)# interface FastEthernet0/0.30
R1_INFRA(config-subif)# encapsulation dot1Q 30
R1_INFRA(config-subif)# ip address 192.168.30.1 255.255.255.0
R1_INFRA(config-subif)# exit

! Sécurité de base
R1_INFRA(config)# enable secret Infra@2024
R1_INFRA(config)# banner motd #INFRA - Acces restreint#

R1_INFRA(config)# end
R1_INFRA# copy running-config startup-config

! Vérifications
R1_INFRA# show ip interface brief
R1_INFRA# show running-config | section interface
```

✅ **Validation Étape B :** `show ip interface brief` montre Fa0/0.10, Fa0/0.20, Fa0/0.30 en **up/up** avec leurs IPs. `show running-config` montre `ip helper-address` sur .10 et .20.

---

### Partie C – Configuration du Serveur DHCP Linux (VM Debian, ~15 min)

**Configurer l'IP fixe du serveur Linux (si pas déjà fait) :**

```bash
# Vérifier l'interface connectée au réseau VLAN 30
ip addr show

# Configurer une IP fixe si nécessaire (selon l'interface)
sudo nano /etc/network/interfaces
```

```
# /etc/network/interfaces
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 192.168.30.10
    netmask 255.255.255.0
    gateway 192.168.30.1
    dns-nameservers 127.0.0.1 8.8.8.8
```

```bash
sudo systemctl restart networking
ip addr show eth0   # Vérifier : 192.168.30.10/24
```

**Configurer dhcpd.conf :**

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

```
option domain-name "infra.local";
option domain-name-servers 192.168.30.10;
default-lease-time 3600;
max-lease-time 86400;
authoritative;

# Réseau VLAN 10 - Informatique
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option domain-name-servers 192.168.30.10;
}

# Réseau VLAN 20 - Direction
subnet 192.168.20.0 netmask 255.255.255.0 {
    range 192.168.20.100 192.168.20.180;
    option routers 192.168.20.1;
    option domain-name-servers 192.168.30.10;
}

# Réseau VLAN 30 - Serveurs (déclaré mais sans plage)
subnet 192.168.30.0 netmask 255.255.255.0 {
    # Pas de range : IP fixes uniquement
}
```

```bash
# Vérifier la syntaxe
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf

# Démarrer/redémarrer
sudo systemctl restart isc-dhcp-server
sudo systemctl status isc-dhcp-server   # Doit être active (running)
```

✅ **Validation Étape C :** `systemctl status isc-dhcp-server` : active. 3 subnets déclarés dans dhcpd.conf.

---

### Partie D – Configuration du Serveur DNS Linux (VM Debian, ~10 min)

```bash
sudo nano /etc/bind/named.conf.local
```

Ajouter :
```
zone "infra.local" {
    type master;
    file "/etc/bind/db.infra.local";
};

zone "30.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192.168.30.rev";
};
```

```bash
sudo nano /etc/bind/db.infra.local
```

```
$TTL 3600
@   IN  SOA ns1.infra.local. admin.infra.local. (
                2024112001 3600 900 604800 3600 )

@           IN  NS  ns1.infra.local.

ns1         IN  A   192.168.30.10
srv-dhcp    IN  A   192.168.30.10
gw          IN  A   192.168.30.1
intranet    IN  A   192.168.10.50
serveur     IN  CNAME   ns1
```

```bash
sudo nano /etc/bind/db.192.168.30.rev
```

```
$TTL 3600
@   IN  SOA ns1.infra.local. admin.infra.local. (
                2024112001 3600 900 604800 3600 )
@   IN  NS  ns1.infra.local.

1   IN  PTR gw.infra.local.
10  IN  PTR ns1.infra.local.
```

```bash
sudo named-checkzone infra.local /etc/bind/db.infra.local
sudo systemctl restart bind9
dig @127.0.0.1 ns1.infra.local         # Test résolution directe
dig @127.0.0.1 -x 192.168.30.10        # Test résolution inverse
```

✅ **Validation Étape D :** Les deux `dig` retournent les bonnes réponses avec le flag `aa`.

---

### Partie E – Tests Finaux (5 min)

**Depuis un PC Cisco dans Packet Tracer (VLAN 10, configuré en DHCP) :**

1. Le PC doit obtenir une IP dans la plage `.100-.200`
2. La passerelle doit être `192.168.10.1`
3. Le DNS doit être `192.168.30.10`

```
PC1> ipconfig /all           # Vérifier IP, masque, GW, DNS
PC1> ping 192.168.20.101     # Ping un PC VLAN 20 (inter-VLAN)
PC1> ping 192.168.30.10      # Ping le serveur Linux
```

**Tableau de validation TP intégré :**

| **Test** | **Commande** | **Résultat attendu** | **✅/❌** |
|----------|--------------|----------------------|----------|
| PC VLAN10 obtient IP DHCP | `ipconfig` | IP dans .10.100-.200 | |
| PC VLAN20 obtient IP DHCP | `ipconfig` | IP dans .20.100-.180 | |
| Ping inter-VLAN | PC VLAN10 → ping .20.101 | 4/4 réponses | |
| Ping vers serveur | PC VLAN10 → ping 192.168.30.10 | 4/4 réponses | |
| DNS résout A | `dig @192.168.30.10 ns1.infra.local` | 192.168.30.10 | |
| Baux DHCP actifs | `cat /var/lib/dhcp/dhcpd.leases` | Entrées actives | |