---
author: YLP
title: 🖥️ FICHE TP
---

## 🖥️ TP GUIDÉ CISCO PACKET TRACER
### "3 VLANs, ports Access, Trunk inter-switches"

---

### Objectif

Configurer 3 VLANs sur 2 switches, affecter des ports Access, et créer un trunk inter-switches permettant aux machines du même VLAN de communiquer d'un switch à l'autre.

---

### Topologie

```
Réseau :
VLAN 10 (DIRECTION)     : 192.168.10.0/24
VLAN 20 (COMMERCIAL)    : 192.168.20.0/24
VLAN 30 (INFORMATIQUE)  : 192.168.30.0/24

SW-A (Switch A)                    SW-B (Switch B)
├─ Fa0/1 : PC-Dir-A  (192.168.10.11)    ├─ Fa0/1 : PC-Dir-B  (192.168.10.21)
├─ Fa0/2 : PC-Com-A  (192.168.20.11)    ├─ Fa0/2 : PC-Com-B  (192.168.20.21)
├─ Fa0/3 : PC-Info-A (192.168.30.11)    └─ Fa0/3 : PC-Info-B (192.168.30.21)
└─ Fa0/24: ──────────── Trunk ─────────── Fa0/24

Passerelle VLAN 10 : 192.168.10.1 (non simulée)
Passerelle VLAN 20 : 192.168.20.1 (non simulée)
Passerelle VLAN 30 : 192.168.30.1 (non simulée)
```

---

### Étape 1 : Créer la Topologie (8 min)

1. Ouvrir Packet Tracer → Nouveau fichier
2. Ajouter **2 switchs Cisco 2960** + **6 PC**
3. **Câbler :**
   - PC-Dir-A → SW-A Fa0/1 (câble droit)
   - PC-Com-A → SW-A Fa0/2
   - PC-Info-A → SW-A Fa0/3
   - PC-Dir-B → SW-B Fa0/1
   - PC-Com-B → SW-B Fa0/2
   - PC-Info-B → SW-B Fa0/3
   - SW-A Fa0/24 → SW-B Fa0/24 (câble droit)

4. **Configurer les IP des PC :**

| **PC** | **IP** | **Masque** | **Passerelle** |
|--------|--------|------------|----------------|
| PC-Dir-A | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC-Com-A | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC-Info-A | 192.168.30.11 | 255.255.255.0 | 192.168.30.1 |
| PC-Dir-B | 192.168.10.21 | 255.255.255.0 | 192.168.10.1 |
| PC-Com-B | 192.168.20.21 | 255.255.255.0 | 192.168.20.1 |
| PC-Info-B | 192.168.30.21 | 255.255.255.0 | 192.168.30.1 |

---

### Étape 2 : Test "Avant VLANs" (5 min)

Sur **PC-Dir-A** → Desktop → Command Prompt :
```
ping 192.168.20.11   (vers PC-Com-A, même switch)
ping 192.168.10.21   (vers PC-Dir-B, autre switch)
```

📝 **Résultats à noter :** Les pings fonctionnent-ils ? (Oui, pour les mêmes sous-réseaux si adresses cohérentes)

⚠️ Attention : PC-Dir-A (192.168.10.11) ne peut pas pinger PC-Com-A (192.168.20.11) car IPs dans réseaux différents, **même sans VLAN**. Les VLANs ajoutent l'isolation L2, en plus de l'isolation L3.

---

### Étape 3 : Configurer SW-A — VLANs et Ports Access (15 min)

```cisco
! Accéder au switch SW-A
SW-A> enable
SW-A# configure terminal

! Renommer le switch
SW-A(config)# hostname SW-A

! Désactiver résolution DNS
SW-A(config)# no ip domain-lookup

! Créer les 3 VLANs
SW-A(config)# vlan 10
SW-A(config-vlan)# name DIRECTION
SW-A(config-vlan)# exit

SW-A(config)# vlan 20
SW-A(config-vlan)# name COMMERCIAL
SW-A(config-vlan)# exit

SW-A(config)# vlan 30
SW-A(config-vlan)# name INFORMATIQUE
SW-A(config-vlan)# exit

! Affecter Fa0/1 → VLAN 10
SW-A(config)# interface FastEthernet0/1
SW-A(config-if)# switchport mode access
SW-A(config-if)# switchport access vlan 10
SW-A(config-if)# exit

! Affecter Fa0/2 → VLAN 20
SW-A(config)# interface FastEthernet0/2
SW-A(config-if)# switchport mode access
SW-A(config-if)# switchport access vlan 20
SW-A(config-if)# exit

! Affecter Fa0/3 → VLAN 30
SW-A(config)# interface FastEthernet0/3
SW-A(config-if)# switchport mode access
SW-A(config-if)# switchport access vlan 30
SW-A(config-if)# exit

! Vérifier
SW-A(config)# end
SW-A# show vlan brief
```

✅ **Validation :** Fa0/1 dans VLAN 10, Fa0/2 dans VLAN 20, Fa0/3 dans VLAN 30.

---

### Étape 4 : Configurer le Trunk sur SW-A (8 min)

```cisco
SW-A# configure terminal

SW-A(config)# interface FastEthernet0/24
SW-A(config-if)# switchport mode trunk
SW-A(config-if)# switchport trunk allowed vlan 10,20,30
SW-A(config-if)# exit

SW-A(config)# end
SW-A# show interfaces trunk
```

✅ **Validation :** Fa0/24 en mode trunking, VLANs 10,20,30 autorisés.

---

### Étape 5 : Configurer SW-B (15 min)

**Même configuration sur SW-B** (VLANs identiques, ports identiques, trunk sur Fa0/24).

```cisco
SW-B> enable
SW-B# configure terminal
SW-B(config)# hostname SW-B
SW-B(config)# no ip domain-lookup

SW-B(config)# vlan 10
SW-B(config-vlan)# name DIRECTION
SW-B(config-vlan)# exit
SW-B(config)# vlan 20
SW-B(config-vlan)# name COMMERCIAL
SW-B(config-vlan)# exit
SW-B(config)# vlan 30
SW-B(config-vlan)# name INFORMATIQUE
SW-B(config-vlan)# exit

SW-B(config)# interface FastEthernet0/1
SW-B(config-if)# switchport mode access
SW-B(config-if)# switchport access vlan 10
SW-B(config-if)# exit

SW-B(config)# interface FastEthernet0/2
SW-B(config-if)# switchport mode access
SW-B(config-if)# switchport access vlan 20
SW-B(config-if)# exit

SW-B(config)# interface FastEthernet0/3
SW-B(config-if)# switchport mode access
SW-B(config-if)# switchport access vlan 30
SW-B(config-if)# exit

SW-B(config)# interface FastEthernet0/24
SW-B(config-if)# switchport mode trunk
SW-B(config-if)# switchport trunk allowed vlan 10,20,30
SW-B(config-if)# exit

SW-B(config)# end
SW-B# copy run start
```

---

### Étape 6 : Sauvegarder SW-A (2 min)

```cisco
SW-A# copy running-config startup-config
```

---

### Étape 7 : Tests de Validation (10 min)

Réaliser les tests suivants et noter les résultats :

| **Test** | **Depuis** | **Vers** | **Ping** | **Résultat attendu** | **Résultat réel** |
|----------|------------|----------|----------|----------------------|-------------------|
| 1 | PC-Dir-A | PC-Dir-B (même VLAN, autre switch) | `ping 192.168.10.21` | ✅ Succès | |
| 2 | PC-Com-A | PC-Com-B (même VLAN, autre switch) | `ping 192.168.20.21` | ✅ Succès | |
| 3 | PC-Dir-A | PC-Com-A (VLANs différents, même switch) | `ping 192.168.20.11` | ❌ Échec attendu | |
| 4 | PC-Dir-A | PC-Info-B (VLANs différents, autre switch) | `ping 192.168.30.21` | ❌ Échec attendu | |

📝 **Analyse :**
- Tests 1 et 2 : même VLAN → communication possible même sur 2 switches différents (grâce au trunk)
- Tests 3 et 4 : VLANs différents → isolation garantie (même sans routeur)

---

### Étape 8 : Vérifications Finales (5 min)

```cisco
! Sur SW-A :
SW-A# show vlan brief
SW-A# show interfaces trunk
SW-A# show interfaces FastEthernet0/1 switchport
SW-A# show mac address-table vlan 10
```

📝 **Questions :**
1. Combien d'entrées dans la table MAC pour VLAN 10 après les tests ?
2. Le port Fa0/24 apparaît-il dans `show vlan brief` ? Pourquoi ?
3. La colonne "Native VLAN" dans `show interfaces trunk` indique quoi ?

---

### Étape 9 : Sauvegarde Packet Tracer

`File → Save As → NOM_Prenom_S7_TP_VLAN_Trunk.pkt`

---

## 🐧 TP LINUX
### "Utilisateurs, Groupes et Permissions"

---

### Objectif

Créer une organisation utilisateurs/groupes sur Debian (3 services, 6 utilisateurs), configurer des répertoires partagés avec les bonnes permissions.

---

### Scénario

> Le cabinet **TechPro SARL** (client S6) demande de préparer son serveur Linux pour 3 services :
> - **Direction** (2 utilisateurs) : répertoire `/data/direction`
> - **Commercial** (2 utilisateurs) : répertoire `/data/commercial`
> - **Informatique** (2 utilisateurs) : répertoire `/data/informatique`
>
> Chaque service ne doit accéder qu'à son propre répertoire. L'Informatique doit pouvoir accéder à tous les répertoires (pour les sauvegardes).

---

### Étape 1 : Créer les Groupes

```bash
sudo groupadd direction
sudo groupadd commercial
sudo groupadd informatique

# Vérifier
cat /etc/group | grep -E "direction|commercial|informatique"
```

---

### Étape 2 : Créer les Utilisateurs

```bash
# Direction
sudo useradd -m -s /bin/bash -G direction alice
sudo passwd alice    # MDP : Alice@123

sudo useradd -m -s /bin/bash -G direction bob
sudo passwd bob      # MDP : Bob@123

# Commercial
sudo useradd -m -s /bin/bash -G commercial charlie
sudo passwd charlie  # MDP : Charlie@123

sudo useradd -m -s /bin/bash -G commercial diana
sudo passwd diana    # MDP : Diana@123

# Informatique
sudo useradd -m -s /bin/bash -G informatique eric
sudo passwd eric     # MDP : Eric@123

sudo useradd -m -s /bin/bash -G informatique fiona
sudo passwd fiona    # MDP : Fiona@123
```

**Vérifier :**
```bash
id alice       # uid=...(alice) gid=...(alice) groups=...(alice),...(direction)
cat /etc/passwd | tail -6    # Les 6 derniers utilisateurs créés
```

---

### Étape 3 : Créer les Répertoires et Définir les Permissions

```bash
# Créer les répertoires
sudo mkdir -p /data/direction /data/commercial /data/informatique

# Affecter les groupes
sudo chown root:direction /data/direction
sudo chown root:commercial /data/commercial
sudo chown root:informatique /data/informatique

# Permissions : groupe peut tout (rwx), autres rien
sudo chmod 770 /data/direction
sudo chmod 770 /data/commercial
sudo chmod 770 /data/informatique

# Vérifier
ls -ld /data/direction /data/commercial /data/informatique
```

**Résultat attendu :**
```
drwxrwx--- 2 root direction    4096 nov 15 /data/direction
drwxrwx--- 2 root commercial   4096 nov 15 /data/commercial
drwxrwx--- 2 root informatique 4096 nov 15 /data/informatique
```

---

### Étape 4 : Donner Accès Multi-Répertoires à l'Informatique

```bash
# Eric et Fiona (Informatique) doivent accéder à TOUS les répertoires
sudo usermod -aG direction eric
sudo usermod -aG commercial eric
sudo usermod -aG direction fiona
sudo usermod -aG commercial fiona

# Vérifier
groups eric    # eric informatique direction commercial
```

---

### Étape 5 : Tester les Accès

```bash
# Test 1 : alice (direction) accède à son répertoire
su - alice
ls /data/direction    # ✅ Doit fonctionner
ls /data/commercial   # ❌ Doit afficher "Permission non accordée"
exit

# Test 2 : charlie (commercial) accède à son répertoire
su - charlie
echo "Fichier commercial" > /data/commercial/test.txt   # ✅ Doit créer le fichier
ls /data/direction                                       # ❌ Permission refusée
exit

# Test 3 : eric (informatique) accède à tous
su - eric
ls /data/direction    # ✅ Doit fonctionner
ls /data/commercial   # ✅ Doit fonctionner
exit
```

---

### Étape 6 : Exercices de Permissions

```bash
# 1. Créer un fichier confidentiel accessible uniquement au propriétaire
touch /home/alice/confidentiel.txt
chmod 600 /home/alice/confidentiel.txt
ls -l /home/alice/confidentiel.txt
# → -rw------- alice alice ...

# 2. Créer un script exécutable par tous
touch /usr/local/bin/ping_test.sh
echo '#!/bin/bash' > /usr/local/bin/ping_test.sh
echo 'ping -c 1 8.8.8.8' >> /usr/local/bin/ping_test.sh
chmod 755 /usr/local/bin/ping_test.sh
ls -l /usr/local/bin/ping_test.sh
# → -rwxr-xr-x root root ...

# 3. Répertoire lisible par le groupe, interdit aux autres
mkdir /data/rapports
chown root:direction /data/rapports
chmod 750 /data/rapports
ls -ld /data/rapports
# → drwxr-x--- root direction ...
```

---
