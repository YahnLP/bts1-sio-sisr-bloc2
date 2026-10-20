# Cours – Agent relais DHCP

*BTS SIO SISR 1re année – Bloc 2 – S09 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S06 à S08.
> - **Matériel :** Simulateur réseau ou Packet Tracer, ou matériel réel (au choix de l'enseignant) ; VM Debian (DHCP, DNS).

## À la fin de ce cours, tu sauras

- [ ] **Expliquer** pourquoi les VLAN isolent le trafic et pourquoi il faut un routage inter-VLAN ;
- [ ] **Configurer** un routeur sur un bras (sous-interfaces 802.1Q) ;
- [ ] **Configurer** l'agent relais DHCP (`ip helper-address`) ;
- [ ] **Déployer** une infrastructure complète : VLAN, routage, DHCP, DNS ;
- [ ] **Diagnostiquer** méthodiquement une panne multi-services.

---

## COURS : AGENT RELAIS DHCP

### A. Le Problème : DHCP et VLANs

**Rappel :** Le protocole DHCP fonctionne grâce à des messages **broadcast** (`255.255.255.255`). Quand un PC démarre et cherche un serveur DHCP, il crie :

> *"Y a-t-il un serveur DHCP quelque part ? DISCOVER !"*

**Le problème :** Les routeurs **ne transmettent pas les broadcasts** entre réseaux.

Dans une infrastructure avec VLANs :

<figure class="graphe diagramme"><svg viewBox="0 0 780 210" style="max-width:780px" role="img" aria-label="Un routeur ne transmet pas le broadcast DHCP d&#x27;un VLAN vers un autre" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="10" width="200" height="190" rx="10" class="dg-soft"/>
<text x="110" y="30" text-anchor="middle" class="dg-t">VLAN 10</text>
<text x="110" y="48" text-anchor="middle" class="dg-s">192.168.10.0/24</text>
<rect x="570" y="10" width="200" height="190" rx="10" class="dg-soft"/>
<text x="670" y="30" text-anchor="middle" class="dg-t">VLAN 30</text>
<text x="670" y="48" text-anchor="middle" class="dg-s">192.168.30.0/24</text>
<rect x="30" y="90" width="130" height="46" rx="8" class="dg-box"/>
<text x="95.0" y="118.0" text-anchor="middle" class="dg-t">PC-A</text>
<rect x="350" y="80" width="120" height="66" rx="8" class="dg-dark"/>
<text x="410.0" y="118.0" text-anchor="middle" class="dg-w">Routeur</text>
<rect x="595" y="90" width="150" height="46" rx="8" class="dg-box"/>
<text x="670.0" y="109.0" text-anchor="middle" class="dg-t">Serveur DHCP</text>
<text x="670.0" y="127.0" text-anchor="middle" class="dg-s">192.168.30.10</text>
<line x1="160" y1="113" x2="341.0" y2="113.0" class="dg-line"/>
<polygon points="350,113 341.0,108.0 341.0,118.0" class="dg-ah"/>
<text x="255.0" y="105.0" text-anchor="middle" class="dg-s">DHCP Discover (broadcast)</text>
<line x1="470" y1="113" x2="595" y2="113" class="dg-dash"/>
<rect x="485" y="130" width="100" height="34" rx="8" class="dg-bad"/>
<text x="535.0" y="152.0" text-anchor="middle" class="dg-t">Non transmis</text>
<text x="160" y="176" text-anchor="middle" class="dg-i">PC-A n&#x27;obtient jamais d&#x27;adresse</text>
</svg>
</figure>

**Le problème :** Faut-il installer un serveur DHCP dans chaque VLAN ? Non — c'est coûteux et difficile à maintenir.

---

### B. La Solution : L'Agent Relais DHCP (`ip helper-address`)

**Principe :** Le routeur intercepte les broadcasts DHCP dans chaque VLAN et les **transmet en unicast** vers le serveur DHCP centralisé.

```
VLAN 10                    ROUTEUR                    VLAN Serveurs
PC-A ──broadcast──→ [Interface Fa0/0.10] → unicast → [Serveur DHCP Linux]
                        ip helper-address             192.168.30.10
                        192.168.30.10
```

**Fonctionnement détaillé :**

<figure class="graphe diagramme"><svg viewBox="0 0 760 400" style="max-width:760px" role="img" aria-label="Relais DHCP : le routeur convertit le broadcast en unicast grâce à giaddr" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="155.0" y="30.0" text-anchor="middle" class="dg-w">PC-A (VLAN 10)</text>
<line x1="155.0" y1="42" x2="155.0" y2="392" class="dg-dash"/>
<rect x="295.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="380.0" y="30.0" text-anchor="middle" class="dg-w">Routeur (agent relais)</text>
<line x1="380.0" y1="42" x2="380.0" y2="392" class="dg-dash"/>
<rect x="520.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="605.0" y="30.0" text-anchor="middle" class="dg-w">Serveur DHCP (VLAN 30)</text>
<line x1="605.0" y1="42" x2="605.0" y2="392" class="dg-dash"/>
<text x="267.5" y="64" text-anchor="middle" class="dg-t">1. DHCP Discover (broadcast)</text>
<line x1="155.0" y1="72" x2="371.0" y2="72.0" class="dg-line"/>
<polygon points="380.0,72 371.0,67.0 371.0,77.0" class="dg-ah"/>
<text x="267.5" y="88" text-anchor="middle" class="dg-i">reçu sur Fa0/0.10</text>
<rect x="194.4" y="106" width="371.2" height="58" rx="6" class="dg-soft"/>
<text x="380.0" y="123" text-anchor="middle" class="dg-s">2. ip helper-address configuré sur l&#x27;interface : OUI</text>
<text x="380.0" y="139" text-anchor="middle" class="dg-s">le broadcast devient un unicast vers 192.168.30.10</text>
<text x="380.0" y="155" text-anchor="middle" class="dg-s">le routeur ajoute giaddr = 192.168.10.1</text>
<text x="492.5" y="184" text-anchor="middle" class="dg-t">Discover relayé (unicast)</text>
<line x1="380.0" y1="192" x2="596.0" y2="192.0" class="dg-line"/>
<polygon points="605.0,192 596.0,187.0 596.0,197.0" class="dg-ah"/>
<text x="492.5" y="208" text-anchor="middle" class="dg-i">giaddr = 192.168.10.1</text>
<rect x="442.5" y="226" width="325.0" height="42" rx="6" class="dg-soft"/>
<text x="605.0" y="243" text-anchor="middle" class="dg-s">3. giaddr = 192.168.10.1 : étendue du VLAN 10</text>
<text x="605.0" y="259" text-anchor="middle" class="dg-s">réponse en unicast vers le routeur</text>
<text x="492.5" y="288" text-anchor="middle" class="dg-t">Offer (unicast vers giaddr)</text>
<line x1="605.0" y1="296" x2="389.0" y2="296.0" class="dg-dash"/>
<polygon points="380.0,296 389.0,301.0 389.0,291.0" class="dg-ah"/>
<text x="267.5" y="322" text-anchor="middle" class="dg-t">4. Offer retransmis via le VLAN 10</text>
<line x1="380.0" y1="330" x2="164.0" y2="330.0" class="dg-dash"/>
<polygon points="155.0,330 164.0,335.0 164.0,325.0" class="dg-ah"/>
<rect x="4.0" y="348" width="331.6" height="26" rx="6" class="dg-soft"/>
<text x="169.79999999999998" y="365" text-anchor="middle" class="dg-s">5. PC-A obtient une adresse de 192.168.10.0/24</text>
</svg>
</figure>

**Le champ `giaddr` (Gateway IP Address)** est la clé : il permet au serveur DHCP de savoir dans quel réseau se trouve le client et donc quelle étendue (subnet) utiliser pour lui attribuer une IP.

---

### C. Configuration de l'Agent Relais

#### Côté Routeur Cisco

La commande `ip helper-address` se configure sur chaque **interface (ou sous-interface) côté VLAN client** :

```cisco
! Sur la sous-interface du VLAN 10 (côté clients VLAN 10)
Router(config)# interface FastEthernet0/0.10
Router(config-if)# ip helper-address 192.168.30.10    ! IP du serveur DHCP Linux

! Sur la sous-interface du VLAN 20 (côté clients VLAN 20)
Router(config)# interface FastEthernet0/0.20
Router(config-if)# ip helper-address 192.168.30.10    ! Même serveur DHCP
```

**Vérification :**
```cisco
Router# show running-config | section interface
! Vérifier que ip helper-address est bien sur les bonnes interfaces

Router# show ip helper-address           ! Sur certaines versions IOS
```

#### Côté Serveur DHCP Linux

Le serveur DHCP doit avoir **une étendue (subnet) pour chaque VLAN** qu'il sert :

```
# /etc/dhcp/dhcpd.conf

# Étendue pour le VLAN 10 (réseau Informatique)
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;              # IP du routeur côté VLAN 10
    option domain-name-servers 192.168.30.10; # IP du serveur DNS
    default-lease-time 86400;
}

# Étendue pour le VLAN 20 (réseau Direction)
subnet 192.168.20.0 netmask 255.255.255.0 {
    range 192.168.20.100 192.168.20.200;
    option routers 192.168.20.1;              # IP du routeur côté VLAN 20
    option domain-name-servers 192.168.30.10;
    default-lease-time 86400;
}

# Déclaration du réseau serveur (obligatoire même si pas de range DHCP ici)
# Bind9 doit connaître ce réseau pour accepter les requêtes relayées
subnet 192.168.30.0 netmask 255.255.255.0 {
    # Pas de range : les serveurs ont des IPs fixes
}
```

> **Important :** Le serveur DHCP Linux doit avoir une déclaration `subnet` pour **tous les réseaux** qu'il connaît, même ceux sans plage d'attribution. Sinon, il refuse de traiter les requêtes relayées.

---

### D. Schéma Complet de l'Infrastructure

<figure class="graphe diagramme"><svg viewBox="0 0 840 330" style="max-width:840px" role="img" aria-label="Infrastructure complète : switch d&#x27;accès, routeur avec relais DHCP, serveur Linux" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="20" width="230" height="200" rx="8" class="dg-soft"/>
<text x="125.0" y="98.0" text-anchor="middle" class="dg-t">SW_ACCÈS (Cisco 2960)</text>
<text x="125.0" y="116.0" text-anchor="middle" class="dg-s">VLAN 10 : Fa0/1 à Fa0/10</text>
<text x="125.0" y="134.0" text-anchor="middle" class="dg-s">VLAN 20 : Fa0/11 à Fa0/20</text>
<text x="125.0" y="152.0" text-anchor="middle" class="dg-s">Trunk : Fa0/24</text>
<rect x="380" y="20" width="300" height="200" rx="8" class="dg-box"/>
<text x="530.0" y="80.0" text-anchor="middle" class="dg-t">ROUTEUR R1</text>
<text x="530.0" y="98.0" text-anchor="middle" class="dg-s">Fa0/0.10 : 192.168.10.1/24</text>
<text x="530.0" y="116.0" text-anchor="middle" class="dg-s">ip helper-address 192.168.30.10</text>
<text x="530.0" y="134.0" text-anchor="middle" class="dg-s">Fa0/0.20 : 192.168.20.1/24</text>
<text x="530.0" y="152.0" text-anchor="middle" class="dg-s">ip helper-address 192.168.30.10</text>
<text x="530.0" y="170.0" text-anchor="middle" class="dg-s">Fa0/0.30 : 192.168.30.1/24</text>
<line x1="240" y1="120" x2="371.0" y2="120.0" class="dg-line"/>
<polygon points="380,120 371.0,115.0 371.0,125.0" class="dg-ah"/>
<text x="310.0" y="110.0" text-anchor="middle" class="dg-s">trunk</text>
<line x1="380" y1="120" x2="249.0" y2="120.0" class="dg-line"/>
<polygon points="240,120 249.0,125.0 249.0,115.0" class="dg-ah"/>
<line x1="530" y1="220" x2="530" y2="262" class="dg-line"/>
<text x="548" y="246" text-anchor="start" class="dg-s">Fa0/0</text>
<rect x="200" y="262" width="630" height="62" rx="10" class="dg-soft"/>
<text x="515" y="282" text-anchor="middle" class="dg-t">Serveur Linux (VM Debian) 192.168.30.10 - VLAN 30</text>
<rect x="214" y="290" width="290" height="28" rx="8" class="dg-ok"/>
<text x="359.0" y="309.0" text-anchor="middle" class="dg-t">isc-dhcp-server : 2 étendues</text>
<rect x="516" y="290" width="304" height="28" rx="8" class="dg-ok"/>
<text x="668.0" y="309.0" text-anchor="middle" class="dg-t">Bind9 : zone infra.local (A, PTR, CNAME)</text>
</svg>
</figure>
