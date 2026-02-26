---
author: YLP
title: 🖥️ FICHE TP
---

## 🖥️ TP CISCO PACKET TRACER
### "Relier 2 Réseaux par un Routeur – Routes Statiques"

---

### Objectif

Créer une topologie avec deux réseaux IP reliés par un routeur, configurer les interfaces et les routes statiques, et valider la connectivité inter-réseaux.

---

### Topologie à Réaliser

```
Réseau INFORMATIQUE              WAN              Réseau DIRECTION
192.168.10.0/24               10.0.0.0/30        192.168.20.0/24

PC-INFO-01 (192.168.10.11) ─Fa0/1─┐         ┌─Fa0/1─ PC-DIR-01 (192.168.20.11)
PC-INFO-02 (192.168.10.12) ─Fa0/2─┤         ├─Fa0/2─ PC-DIR-02 (192.168.20.12)
                                   │         │
                        [SW-INFO]  │         │  [SW-DIR]
                                   │         │
                              Fa0/0─┤R1_INFO├─Fa0/0
                              .1   │         │.1
                         Se0/0/0───┘         └───Se0/0/0
                         10.0.0.1               10.0.0.2
                                  [LIAISON WAN]
```

**Plan d'adressage :**

| **Équipement** | **Interface** | **Adresse IP** | **Masque** |
|----------------|---------------|----------------|------------|
| R1_INFO | Fa0/0 (LAN INFO) | 192.168.10.1 | 255.255.255.0 |
| R1_INFO | Se0/0/0 (WAN) | 10.0.0.1 | 255.255.255.252 |
| R2_DIR | Fa0/0 (LAN DIR) | 192.168.20.1 | 255.255.255.0 |
| R2_DIR | Se0/0/0 (WAN) | 10.0.0.2 | 255.255.255.252 |
| PC-INFO-01 | – | 192.168.10.11 | 255.255.255.0 (GW: .10.1) |
| PC-INFO-02 | – | 192.168.10.12 | 255.255.255.0 (GW: .10.1) |
| PC-DIR-01 | – | 192.168.20.11 | 255.255.255.0 (GW: .20.1) |
| PC-DIR-02 | – | 192.168.20.12 | 255.255.255.0 (GW: .20.1) |

---

### Étape 1 : Placer les Équipements (5 min)

Dans Packet Tracer :
1. **2 Routeurs** Cisco 1841 (Network Devices → Routers)
2. **2 Switches** Cisco 2960 (Network Devices → Switches)
3. **4 PC** (End Devices)
4. Renommer : R1_INFO, R2_DIR, SW-INFO, SW-DIR, PC-INFO-01, PC-INFO-02, PC-DIR-01, PC-DIR-02

---

### Étape 2 : Câbler la Topologie (5 min)

| **De** | **Port** | **Vers** | **Port** | **Câble** |
|--------|----------|----------|----------|-----------|
| PC-INFO-01 | Fa0 | SW-INFO | Fa0/1 | Droit |
| PC-INFO-02 | Fa0 | SW-INFO | Fa0/2 | Droit |
| SW-INFO | Fa0/24 | R1_INFO | Fa0/0 | Droit |
| R1_INFO | Se0/0/0 | R2_DIR | Se0/0/0 | **Série (DCE-DTE)** |
| R2_DIR | Fa0/0 | SW-DIR | Fa0/24 | Droit |
| SW-DIR | Fa0/1 | PC-DIR-01 | Fa0 | Droit |
| SW-DIR | Fa0/2 | PC-DIR-02 | Fa0 | Droit |

⚠️ **Câble série :** Dans Packet Tracer, utiliser **Serial DTE** ou **Serial DCE** (dans Connections). Identifier quel côté est DCE avec `show controllers serial 0/0/0`.

---

### Étape 3 : Configurer les PC (5 min)

Pour chaque PC → Onglet **Desktop** → **IP Configuration** :

| **PC** | **IP** | **Masque** | **Passerelle** |
|--------|--------|------------|----------------|
| PC-INFO-01 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC-INFO-02 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |
| PC-DIR-01 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC-DIR-02 | 192.168.20.12 | 255.255.255.0 | 192.168.20.1 |

---

### Étape 4 : Configurer R1_INFO (15 min)

Clic sur R1_INFO → Onglet **CLI** → **Entrée** → **no** au dialog.

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R1_INFO
R1_INFO(config)# no ip domain-lookup

! Interface LAN côté Informatique
R1_INFO(config)# interface FastEthernet0/0
R1_INFO(config-if)# description LAN Informatique 192.168.10.0/24
R1_INFO(config-if)# ip address 192.168.10.1 255.255.255.0
R1_INFO(config-if)# no shutdown
R1_INFO(config-if)# exit

! Interface WAN vers R2
R1_INFO(config)# interface Serial0/0/0
R1_INFO(config-if)# description WAN vers R2_DIR
R1_INFO(config-if)# ip address 10.0.0.1 255.255.255.252
R1_INFO(config-if)# clock rate 64000    ! Si ce côté est DCE
R1_INFO(config-if)# no shutdown
R1_INFO(config-if)# exit

! Route statique vers le réseau Direction
R1_INFO(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2

R1_INFO(config)# end
R1_INFO# copy running-config startup-config
```

**Vérification :**
```cisco
R1_INFO# show ip interface brief
R1_INFO# show ip route
```

✅ **Attendu :** Fa0/0 et Se0/0/0 en **up/up**, route S vers 192.168.20.0/24 présente.

---

### Étape 5 : Configurer R2_DIR (10 min)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R2_DIR
R2_DIR(config)# no ip domain-lookup

R2_DIR(config)# interface FastEthernet0/0
R2_DIR(config-if)# description LAN Direction 192.168.20.0/24
R2_DIR(config-if)# ip address 192.168.20.1 255.255.255.0
R2_DIR(config-if)# no shutdown
R2_DIR(config-if)# exit

R2_DIR(config)# interface Serial0/0/0
R2_DIR(config-if)# description WAN vers R1_INFO
R2_DIR(config-if)# ip address 10.0.0.2 255.255.255.252
R2_DIR(config-if)# no shutdown        ! clock rate NON nécessaire côté DTE
R2_DIR(config-if)# exit

! Route statique vers le réseau Informatique
R2_DIR(config)# ip route 192.168.10.0 255.255.255.0 10.0.0.1

R2_DIR(config)# end
R2_DIR# copy running-config startup-config
```

---

### Étape 6 : Tests de Connectivité (10 min)

Depuis **PC-INFO-01** → Desktop → **Command Prompt** :

```
Test 1 : Ping local (même réseau)
> ping 192.168.10.12
Attendu : 4 réponses ✅

Test 2 : Ping passerelle
> ping 192.168.10.1
Attendu : 4 réponses ✅

Test 3 : Ping inter-réseaux (LE TEST CLÉ)
> ping 192.168.20.11
Attendu : 4 réponses ✅
(1er ping souvent KO = ARP, les 3 suivants OK)

Test 4 : Ping autre PC côté Direction
> ping 192.168.20.12
Attendu : 4 réponses ✅
```

Depuis **R1_INFO** :
```cisco
R1_INFO# ping 192.168.20.1     ! Ping routeur distant
R1_INFO# ping 192.168.20.11    ! Ping PC côté Direction
R1_INFO# show ip route          ! Vérifier toutes les routes
R1_INFO# traceroute 192.168.20.11   ! Voir le chemin
```

---

### Grille de Validation TP Cisco

| **Critère** | **Commande de vérification** | **Résultat attendu** | **✅/❌** |
|-------------|------------------------------|----------------------|----------|
| R1_INFO hostname | `show running-config` | hostname R1_INFO | |
| Fa0/0 R1 up/up | `show ip interface brief` | FastEthernet0/0 up up | |
| Se0/0/0 R1 up/up | `show ip interface brief` | Serial0/0/0 up up | |
| Route S vers 192.168.20.0 dans R1 | `show ip route` | S 192.168.20.0/24 via 10.0.0.2 | |
| Route S vers 192.168.10.0 dans R2 | `show ip route` | S 192.168.10.0/24 via 10.0.0.1 | |
| Ping local INFO | PC-INFO-01 ping .12 | 4/4 succès | |
| Ping inter-réseaux | PC-INFO-01 ping 192.168.20.11 | 4/4 succès | |
| Config sauvegardée | `show startup-config` | Identique à running | |

---

### Étape 7 : Sauvegarder le Fichier

**File** → **Save As** → `NOM_Prenom_S8_TP_Routage.pkt`

---
