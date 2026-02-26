---
author: YLP
title: 📚 FICHE DE COURS
---

# 📚 FICHE DE COURS — RÉVISIONS CCNA 1

## I. Structure de l'Examen CCNA

Le **CCNA** (Cisco Certified Network Associate) est la certification réseau de référence pour les techniciens IT. L'examen actuel **CCNA 200-301** couvre un spectre large :

| **Domaine** | **Poids à l'examen** | **Couverture dans le cours BTS** |
|---|---|---|
| Network Fundamentals (Fondamentaux réseau) | ~20% | S4-S5, S8 |
| Network Access (Accès réseau, switching) | ~20% | S10, S14 |
| IP Connectivity (Routage IP) | ~25% | S4-S5, S14 |
| IP Services (DHCP, DNS, NAT, NTP) | ~10% | S11, S13 |
| Security Fundamentals | ~15% | S12, S15-S16 |
| Automation and Programmability | ~10% | S14-S16 |

**Format de l'examen :**
- Durée : 120 minutes
- ~100 questions (QCM, glisser-déposer, simulation Packet Tracer)
- Score de passage : 825/1000
- Langue : Anglais (quelques versions traduites disponibles)

---

## II. Les 7 Couches du Modèle OSI — Mémo Absolu

```
Couche 7 — APPLICATION   → HTTP, HTTPS, FTP, SFTP, SMTP, DNS, SSH, DHCP
Couche 6 — PRÉSENTATION  → Chiffrement, compression, encodage (SSL/TLS)
Couche 5 — SESSION       → Établissement et contrôle des sessions
Couche 4 — TRANSPORT     → TCP (fiable), UDP (rapide) — ports source/destination
Couche 3 — RÉSEAU        → IP, ICMP, routage — adresses IP
Couche 2 — LIAISON       → Ethernet, 802.1Q VLAN, MAC — trames
Couche 1 — PHYSIQUE      → Câbles, signaux électriques/optiques — bits

Moyen mémorisation (anglais) : "All People Seem To Need Data Processing"
                               (Application, Presentation, Session, Transport, Network, Data Link, Physical)
```

**Quel équipement à quelle couche ?**

| **Équipement** | **Couche OSI** | **Unité traitée** |
|---|---|---|
| Hub, câble, répéteur | Couche 1 | Bits |
| Switch L2 | Couche 2 | Trames (adresses MAC) |
| Switch L3, routeur | Couche 3 | Paquets (adresses IP) |
| Pare-feu, proxy, load balancer | Couche 4 à 7 | Segments, données |

---

## III. TCP vs UDP — Ce Qu'il Faut Retenir

| **Critère** | **TCP** | **UDP** |
|---|---|---|
| Connexion | Orienté connexion (3-way handshake) | Sans connexion |
| Fiabilité | Garantie (acquittements, retransmissions) | Non garantie |
| Ordre | Garanti | Non garanti |
| Vitesse | Plus lent (overhead) | Plus rapide |
| Usage | HTTP, HTTPS, FTP, SSH, SMTP, AD | DNS, DHCP, VoIP, streaming, jeux |

**Le 3-way handshake TCP :**
```
Client ──── SYN ───────────────► Serveur
Client ◄─── SYN-ACK ──────────── Serveur
Client ──── ACK ───────────────► Serveur
             ↑
    Connexion établie — échange de données
```

---

## IV. Ports à Connaître Par Cœur

| **Port** | **Protocole** | **Couche** | **TCP/UDP** |
|---|---|---|---|
| 20/21 | FTP | Application | TCP |
| 22 | SSH / SFTP | Application | TCP |
| 23 | Telnet | Application | TCP |
| 25 | SMTP | Application | TCP |
| 53 | DNS | Application | TCP + UDP |
| 67/68 | DHCP | Application | UDP |
| 80 | HTTP | Application | TCP |
| 110 | POP3 | Application | TCP |
| 143 | IMAP | Application | TCP |
| 389 | LDAP | Application | TCP |
| 443 | HTTPS | Application | TCP |
| 445 | SMB (partages Windows) | Application | TCP |
| 3306 | MySQL/MariaDB | Application | TCP |
| 3389 | RDP | Application | TCP |
| 5985/5986 | WinRM | Application | TCP |

---

## V. Adressage IP — Récapitulatif Complet

### Classes IPv4 (connaissance historique)

| **Classe** | **1er octet** | **Plage** | **Masque par défaut** | **Adresses privées** |
|---|---|---|---|---|
| A | 1–126 | 1.0.0.0 – 126.255.255.255 | /8 | 10.0.0.0/8 |
| B | 128–191 | 128.0.0.0 – 191.255.255.255 | /16 | 172.16.0.0/12 |
| C | 192–223 | 192.0.0.0 – 223.255.255.255 | /24 | 192.168.0.0/16 |
| D | 224–239 | Multicast | — | — |
| E | 240–255 | Expérimental | — | — |

### Adresses Spéciales à Retenir

| **Adresse** | **Signification** |
|---|---|
| `127.0.0.1` | Loopback (interface locale) |
| `0.0.0.0` | Route par défaut / adresse non spécifiée |
| `255.255.255.255` | Broadcast limité (tout le réseau local) |
| `169.254.x.x` | APIPA (pas de DHCP disponible) |

### Calcul Rapide de Sous-Réseau

```
Masque /n → 2^(32-n) adresses totales → 2^(32-n) - 2 hôtes

/24 → 256 adresses → 254 hôtes
/25 → 128 adresses → 126 hôtes
/26 → 64 adresses  → 62 hôtes
/27 → 32 adresses  → 30 hôtes
/28 → 16 adresses  → 14 hôtes
/29 → 8 adresses   → 6 hôtes
/30 → 4 adresses   → 2 hôtes (liaison point à point)
/31 → 2 adresses   → 0 hôtes (RFC 3021 : liaisons P2P spéciales)
/32 → 1 adresse    → hôte unique
```

---

## VI. Protocoles de Routage — Vue d'Ensemble

| **Type** | **Protocole** | **Algorithme** | **Usage** |
|---|---|---|---|
| Statique | Routes statiques (`ip route`) | Aucun (manuel) | Petits réseaux, routes par défaut |
| Dynamique IGP | **OSPF** | Dijkstra (état des liens) | Entreprises, interopérable |
| Dynamique IGP | **EIGRP** (Cisco propriétaire) | DUAL | Réseaux Cisco homogènes |
| Dynamique EGP | **BGP** | Path vector | Connexion opérateurs Internet |

```
Routage statique :
Router(config)# ip route [réseau] [masque] [next-hop ou interface]
Route par défaut :
Router(config)# ip route 0.0.0.0 0.0.0.0 [passerelle Internet]
```

---

## VII. Protocoles Essentiels — Mémo

| **Protocole** | **Rôle** | **Couche** | **Port/Type** |
|---|---|---|---|
| **ARP** | Résout IP → MAC sur le réseau local | 2/3 | Broadcast Ethernet |
| **ICMP** | Messages d'erreur et diagnostic (`ping`, `traceroute`) | 3 | Protocole IP type 1 |
| **DHCP** | Attribution automatique d'adresses IP | Application | UDP 67/68 |
| **DNS** | Résolution de noms en adresses IP | Application | UDP/TCP 53 |
| **NAT** | Traduction d'adresses privées ↔ publiques | 3 | — |
| **STP** | Prévention des boucles de niveau 2 | 2 | — |
| **VTP** | Propagation des VLANs entre switches Cisco | 2 | — |
| **CDP** | Découverte des voisins Cisco (propriétaire) | 2 | — |
| **LLDP** | Découverte des voisins (standard IEEE) | 2 | — |
| **802.1Q** | Encapsulation VLAN sur les trunks | 2 | — |

---
