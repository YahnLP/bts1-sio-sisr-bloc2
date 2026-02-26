# Pack de Formation - Semaine 10 (S10) - BLOC 2
## "Agent Relais DHCP, TP Intégré et Bilan Phase 2"

---

## 📚 COURS : AGENT RELAIS DHCP

### A. Le Problème : DHCP et VLANs

**Rappel :** Le protocole DHCP fonctionne grâce à des messages **broadcast** (`255.255.255.255`). Quand un PC démarre et cherche un serveur DHCP, il crie :

> *"Y a-t-il un serveur DHCP quelque part ? DISCOVER !"*

**Le problème :** Les routeurs **ne transmettent pas les broadcasts** entre réseaux.

Dans une infrastructure avec VLANs :

```
┌──────────────────────────────────────────────────────┐
│ VLAN 10 (192.168.10.0/24)                            │
│   PC-A démarre → envoie DHCP Discover (broadcast)    │
│   Le broadcast reste dans VLAN 10                    │
│   Le serveur DHCP est dans VLAN 30 (192.168.30.0/24) │
│   Le routeur ne transmet PAS le broadcast vers VLAN30 │
│   → PC-A n'obtient jamais de réponse DHCP ❌          │
└──────────────────────────────────────────────────────┘
```

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

```
1. PC-A (VLAN 10) : DHCP Discover (broadcast 255.255.255.255)
       ↓
2. Routeur reçoit sur Fa0/0.10 (VLAN 10)
   → Vérifie si ip helper-address est configuré sur cette interface
   → OUI : transforme le broadcast en unicast vers 192.168.30.10
   → Ajoute son adresse IP (192.168.10.1) dans le champ "giaddr" (Gateway IP)
       ↓
3. Serveur DHCP reçoit la requête relayée en unicast
   → Voit giaddr = 192.168.10.1 → sait que c'est pour la plage du VLAN 10
   → Répond en unicast vers 192.168.10.1 (le routeur)
       ↓
4. Routeur reçoit la réponse DHCP
   → La retransmet vers PC-A (via VLAN 10)
       ↓
5. PC-A obtient une IP dans la plage 192.168.10.0/24
```

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

```
┌─────────────────────────────────────────────────────────────────────┐
│                    INFRASTRUCTURE COMPLÈTE                          │
│                                                                     │
│  ┌──────────────┐    trunk    ┌──────────────────────────────────┐  │
│  │   SW_ACCÈS   │────────────│  ROUTEUR R1                      │  │
│  │  (Cisco 2960)│            │                                  │  │
│  │              │            │  Fa0/0.10  192.168.10.1/24       │  │
│  │ VLAN 10:     │            │    ip helper-address 192.168.30.10│  │
│  │  Fa0/1-Fa0/10│            │                                  │  │
│  │ VLAN 20:     │            │  Fa0/0.20  192.168.20.1/24       │  │
│  │  Fa0/11-Fa0/20│           │    ip helper-address 192.168.30.10│  │
│  │ Trunk:       │            │                                  │  │
│  │  Fa0/24 ─────┤            │  Fa0/0.30  192.168.30.1/24       │  │
│  └──────────────┘            └──────────┬───────────────────────┘  │
│                                         │ Fa0/0                     │
│  ┌──────────────────────────────────────┘                           │
│  │                                                                  │
│  │  ┌───────────────────────────────────────────────────────────┐  │
│  │  │  SERVEUR LINUX (VM Debian) – 192.168.30.10                │  │
│  │  │  ┌─────────────────────┐  ┌──────────────────────────────┐│  │
│  │  │  │  isc-dhcp-server    │  │  Bind9 (DNS)                 ││  │
│  │  │  │  2 étendues :       │  │  Zone : infra.local          ││  │
│  │  │  │  ▸ 192.168.10.0/24  │  │  A, PTR, CNAME               ││  │
│  │  │  │  ▸ 192.168.20.0/24  │  │                              ││  │
│  │  │  └─────────────────────┘  └──────────────────────────────┘│  │
│  │  └───────────────────────────────────────────────────────────┘  │
│  │   [VLAN 30 - Serveurs]                                           │
│  │   SW_ACCÈS Fa0/24 → R1 → Fa0/1 (accès) → Srv Linux             │
│  └──────────────────────────────────────────────────────────────────│
└─────────────────────────────────────────────────────────────────────┘
```

