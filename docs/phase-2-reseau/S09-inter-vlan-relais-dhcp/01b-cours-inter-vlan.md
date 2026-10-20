# Cours – Routage inter-VLAN (routeur sur un bras)

*BTS SIO SISR 1re année – Bloc 2 – S09*

## PARTIE I — Routage Inter-VLAN

### I.A. Rappel : Le Problème de l'Isolation des VLANs

Un **VLAN** (Virtual Local Area Network) crée une **segmentation logique** du réseau : les machines de deux VLANs différents sont dans des sous-réseaux IP distincts et ne peuvent **pas se communiquer directement**, même si elles sont physiquement sur le même switch.

C'est voulu pour la sécurité, mais cela pose un problème pratique : la comptable du VLAN RH doit pouvoir envoyer un email au directeur dans le VLAN Direction !

**La solution : le routage inter-VLAN** — faire passer le trafic d'un VLAN à l'autre via un équipement de niveau 3 (routeur), qui agit comme un **point de contrôle centralisé**.

 **Lien ITIL — Gestion de la Disponibilité & Sécurité :** La segmentation en VLANs est une bonne pratique de sécurité (confinement des incidents, isolation des données sensibles). Le routage inter-VLAN permet de délivrer la **connectivité nécessaire aux métiers** tout en maintenant cette segmentation. C'est un équilibre entre disponibilité du service et sécurité.

---

### I.B. Les Trois Méthodes de Routage Inter-VLAN

| **Méthode** | **Description** | **Avantages** | **Inconvénients** |
|-------------|-----------------|--------------|------------------|
| **Un routeur par VLAN** | 1 interface physique du routeur par VLAN | Simple à comprendre | Très coûteux (N interfaces), peu scalable |
| **Router-on-a-Stick**  | 1 seule interface physique divisée en sous-interfaces logiques | Économique, simple à configurer | Goulot d'étranglement sur le lien trunk en charge |
| **Switch L3 (MLS)** | Routage effectué directement par le switch | Très performant, recommandé en production | Plus coûteux qu'un switch L2 |

*Au BTS SIO SISR, on étudie principalement le **Router-on-a-Stick** (méthode 2).*

---

### I.C. Architecture Router-on-a-Stick

#### Principe

Le routeur est connecté au switch par **un seul câble physique** configuré en mode **trunk** (il transporte plusieurs VLANs grâce au standard 802.1Q). Côté routeur, l'interface physique est subdivisée en **sous-interfaces logiques**, une par VLAN.

<figure class="graphe diagramme"><svg viewBox="0 0 780 260" style="max-width:780px" role="img" aria-label="Routage inter-VLAN : trois postes sur un switch relié en trunk à un routeur à sous-interfaces" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="20" width="170" height="56" rx="8" class="dg-box"/>
<text x="95.0" y="44.0" text-anchor="middle" class="dg-t">PC-RH</text>
<text x="95.0" y="62.0" text-anchor="middle" class="dg-s">VLAN 10 - 192.168.10.10</text>
<line x1="180" y1="48" x2="330" y2="120" class="dg-line"/>
<text x="215" y="51.64" text-anchor="middle" class="dg-s">Fa0/1</text>
<rect x="10" y="96" width="170" height="56" rx="8" class="dg-ok"/>
<text x="95.0" y="120.0" text-anchor="middle" class="dg-t">PC-IT</text>
<text x="95.0" y="138.0" text-anchor="middle" class="dg-s">VLAN 20 - 192.168.20.10</text>
<line x1="180" y1="124" x2="330" y2="120" class="dg-line"/>
<text x="215" y="118.52" text-anchor="middle" class="dg-s">Fa0/2</text>
<rect x="10" y="172" width="170" height="56" rx="8" class="dg-warn"/>
<text x="95.0" y="196.0" text-anchor="middle" class="dg-t">PC-DIR</text>
<text x="95.0" y="214.0" text-anchor="middle" class="dg-s">VLAN 30 - 192.168.30.10</text>
<line x1="180" y1="200" x2="330" y2="120" class="dg-line"/>
<text x="215" y="185.4" text-anchor="middle" class="dg-s">Fa0/3</text>
<rect x="330" y="90" width="110" height="60" rx="8" class="dg-dark"/>
<text x="385.0" y="116.0" text-anchor="middle" class="dg-w">SWITCH</text>
<text x="385.0" y="134.0" text-anchor="middle" class="dg-s">[trunk]</text>
<line x1="440" y1="120" x2="520" y2="120" class="dg-line"/>
<text x="480" y="110" text-anchor="middle" class="dg-s">Gig0/1</text>
<rect x="520" y="20" width="250" height="200" rx="8" class="dg-soft"/>
<text x="645.0" y="98.0" text-anchor="middle" class="dg-t">ROUTEUR</text>
<text x="645.0" y="116.0" text-anchor="middle" class="dg-s">G0/0.10 : passerelle VLAN 10</text>
<text x="645.0" y="134.0" text-anchor="middle" class="dg-s">G0/0.20 : passerelle VLAN 20</text>
<text x="645.0" y="152.0" text-anchor="middle" class="dg-s">G0/0.30 : passerelle VLAN 30</text>
</svg>
</figure>

*Légende : Schéma d'une architecture router-on-a-stick. Le switch à gauche est connecté au routeur par un seul lien physique trunk (ligne épaisse). Le routeur divise ce lien en 3 sous-interfaces logiques (G0/0.10, G0/0.20, G0/0.30), une par VLAN. Chaque sous-interface devient la passerelle par défaut des machines de son VLAN.*

---

#### Le Standard 802.1Q (Dot1Q)

Lorsqu'un paquet voyage sur un lien trunk, le switch lui ajoute un **tag 802.1Q** — une étiquette de 4 octets insérée dans l'en-tête Ethernet qui indique à quel VLAN appartient ce paquet.

<figure class="graphe diagramme"><svg viewBox="0 0 760 210" style="max-width:760px" role="img" aria-label="Trame Ethernet standard et trame avec tag 802.1Q" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="26" text-anchor="start" class="dg-t">Trame Ethernet standard (sans tag, port Access)</text>
<rect x="20" y="36" width="110" height="42" rx="5" class="dg-box"/>
<text x="75.0" y="62.0" text-anchor="middle" class="dg-t">Dest</text>
<rect x="134" y="36" width="110" height="42" rx="5" class="dg-box"/>
<text x="189.0" y="62.0" text-anchor="middle" class="dg-t">Src</text>
<rect x="248" y="36" width="90" height="42" rx="5" class="dg-box"/>
<text x="293.0" y="62.0" text-anchor="middle" class="dg-t">Type</text>
<rect x="342" y="36" width="230" height="42" rx="5" class="dg-soft"/>
<text x="457.0" y="62.0" text-anchor="middle" class="dg-t">Données</text>
<rect x="576" y="36" width="70" height="42" rx="5" class="dg-box"/>
<text x="611.0" y="62.0" text-anchor="middle" class="dg-t">FCS</text>
<text x="20" y="116" text-anchor="start" class="dg-t">Trame avec tag 802.1Q (lien trunk)</text>
<rect x="20" y="126" width="100" height="42" rx="5" class="dg-box"/>
<text x="70.0" y="152.0" text-anchor="middle" class="dg-t">Dest</text>
<rect x="124" y="126" width="100" height="42" rx="5" class="dg-box"/>
<text x="174.0" y="152.0" text-anchor="middle" class="dg-t">Src</text>
<rect x="228" y="126" width="110" height="42" rx="5" class="dg-warn"/>
<text x="283.0" y="152.0" text-anchor="middle" class="dg-t">Tag 802.1Q</text>
<rect x="342" y="126" width="80" height="42" rx="5" class="dg-box"/>
<text x="382.0" y="152.0" text-anchor="middle" class="dg-t">Type</text>
<rect x="426" y="126" width="190" height="42" rx="5" class="dg-soft"/>
<text x="521.0" y="152.0" text-anchor="middle" class="dg-t">Données</text>
<rect x="620" y="126" width="70" height="42" rx="5" class="dg-box"/>
<text x="655.0" y="152.0" text-anchor="middle" class="dg-t">FCS</text>
<text x="250" y="198" text-anchor="middle" class="dg-i">Tag : 4 octets, dont le VLAN ID</text>
</svg>
<figcaption>Le tag 802.1Q de 4 octets contient notamment le VLAN ID sur 12 bits (4094 VLAN).</figcaption>
</figure>

*Légende : Comparaison entre une trame Ethernet standard (sans tag, utilisée sur un port d'accès) et une trame Ethernet avec tag 802.1Q (utilisée sur un lien trunk). Le champ Tag 802.1Q de 4 octets contient notamment le VLAN ID (12 bits, permettant de distinguer jusqu'à 4094 VLANs).*

---

### I.D. Cheminement d'un Paquet Inter-VLAN

Traçons le chemin d'un paquet de **PC-RH (VLAN 10)** vers **PC-DIR (VLAN 30)** :

1. **PC-RH** envoie le paquet vers sa passerelle (192.168.10.1 = G0/0.10 du routeur).
2. Le **switch** reçoit la trame sur Fa0/1 (VLAN 10 access), ajoute le **tag 802.1Q (VLAN 10)** et l'envoie sur le trunk.
3. Le **routeur** reçoit la trame taguée VLAN 10 sur G0/0.10, retire le tag, lit l'adresse IP de destination (192.168.30.x) et consulte sa table de routage : sortie par G0/0.30.
4. Le routeur **re-tague** la trame avec le VLAN 30 et l'envoie sur le trunk vers le switch.
5. Le switch retire le tag et émet la trame sur Fa0/3 (VLAN 30 access).
6. **PC-DIR** reçoit le paquet.

*Légende : Diagramme de flux montrant le cheminement étape par étape d'un paquet depuis PC-RH (VLAN 10) jusqu'à PC-DIR (VLAN 30). Chaque étape met en évidence le rôle du tag 802.1Q et l'action du routeur (consultation de la table de routage).*

---

### I.E. Configuration Cisco — Commandes Essentielles

#### Côté Switch : Créer les VLANs et configurer le trunk

```
Switch> enable
Switch# configure terminal

! Créer les VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name RH
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Informatique
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name Direction
Switch(config-vlan)# exit

! Assigner les ports d'accès
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# exit

Switch(config)# interface FastEthernet0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 30
Switch(config-if)# exit

! Configurer le port trunk vers le routeur
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q    ! (si requis par le modèle)
Switch(config-if)# exit

Switch(config)# end
Switch# write memory
```

#### Côté Routeur : Créer les sous-interfaces

```
Router> enable
Router# configure terminal

! Activer l'interface physique (sans IP sur l'interface physique elle-même)
Router(config)# interface GigabitEthernet0/0
Router(config-if)# no shutdown
Router(config-if)# exit

! Créer la sous-interface pour VLAN 10
Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

! Créer la sous-interface pour VLAN 20
Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

! Créer la sous-interface pour VLAN 30
Router(config)# interface GigabitEthernet0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# exit

Router(config)# end
Router# write memory
```

 **Règle importante :** Le numéro de sous-interface (`.10`, `.20`, `.30`) n'est pas obligatoirement identique au numéro de VLAN — mais c'est une **bonne pratique** de les aligner pour la lisibilité et la maintenance.

#### Commandes de Vérification Essentielles

| **Commande** | **Sur** | **Ce qu'elle affiche** |
|---|---|---|
| `show vlan brief` | Switch | Liste des VLANs et ports associés |
| `show interfaces trunk` | Switch | Interfaces en mode trunk et VLANs autorisés |
| `show ip interface brief` | Routeur | État de toutes les interfaces et sous-interfaces |
| `show ip route` | Routeur | Table de routage (doit contenir les 3 réseaux) |
| `ping 192.168.30.1` | PC source | Test de connectivité vers la passerelle du VLAN cible |

---
