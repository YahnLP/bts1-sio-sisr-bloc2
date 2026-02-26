---
author: YLP
title: 🖥️ FICHE DE TP
---

# 🖥️ FICHE TP ÉLÈVE
## TP S14 — Routage Inter-VLAN Cisco + Script Bash de création d'utilisateurs

*Durée : ~2h30 — En binôme*

---

## Contexte

Vous continuez votre mission chez **SimIO SARL**. Ce TP est en deux parties indépendantes :
- **Partie A** : Configurer le routage inter-VLAN sur l'infrastructure Cisco (Packet Tracer)
- **Parties B & C** : Automatiser la création d'utilisateurs Linux avec un script Bash

---

## PARTIE A — Routage Inter-VLAN avec Cisco Packet Tracer

### Topologie à Reproduire

Créer dans Packet Tracer la topologie suivante :

```
   PC-RH                 SW-CORE                  R-SIOSARL
 VLAN 10               [Catalyst]                  [Router]
192.168.10.10 ──Fa0/1──►          ──Gig0/1──────► G0/0.10 → 192.168.10.1/24
                                                   G0/0.20 → 192.168.20.1/24
   PC-IT       ──Fa0/2──►                          G0/0.30 → 192.168.30.1/24
 VLAN 20
192.168.20.10

   PC-DIR      ──Fa0/3──►
 VLAN 30
192.168.30.10
```

---

### A1 — Créer la Topologie dans Packet Tracer

1. Ouvrir **Cisco Packet Tracer**
2. Placer les équipements :
   - 1 Switch **Catalyst 2960** (ou équivalent)
   - 1 Routeur **Cisco 1941** (ou équivalent)
   - 3 PC (PC-RH, PC-IT, PC-DIR)
3. Câbler :
   - PC-RH → SW-CORE Fa0/1 (câble droit)
   - PC-IT → SW-CORE Fa0/2 (câble droit)
   - PC-DIR → SW-CORE Fa0/3 (câble droit)
   - SW-CORE Gig0/1 → R-SIOSARL G0/0 (câble droit ou croisé selon les équipements)

---

### A2 — Configurer les Adresses IP des PCs

Configurer les PCs (Desktop → IP Configuration) :

| **PC** | **Adresse IP** | **Masque** | **Passerelle** |
|---|---|---|---|
| PC-RH | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC-IT | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC-DIR | 192.168.30.10 | 255.255.255.0 | 192.168.30.1 |

---

### A3 — Configurer le Switch SW-CORE

Accéder au CLI du switch (clic → CLI) et saisir :

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW-CORE

! ── Création des VLANs ──────────────────────────
SW-CORE(config)# vlan 10
SW-CORE(config-vlan)# name RH
SW-CORE(config-vlan)# exit
SW-CORE(config)# vlan 20
SW-CORE(config-vlan)# name Informatique
SW-CORE(config-vlan)# exit
SW-CORE(config)# vlan 30
SW-CORE(config-vlan)# name Direction
SW-CORE(config-vlan)# exit

! ── Ports d'accès ───────────────────────────────
SW-CORE(config)# interface Fa0/1
SW-CORE(config-if)# switchport mode access
SW-CORE(config-if)# switchport access vlan 10
SW-CORE(config-if)# exit

SW-CORE(config)# interface Fa0/2
SW-CORE(config-if)# switchport mode access
SW-CORE(config-if)# switchport access vlan 20
SW-CORE(config-if)# exit

SW-CORE(config)# interface Fa0/3
SW-CORE(config-if)# switchport mode access
SW-CORE(config-if)# switchport access vlan 30
SW-CORE(config-if)# exit

! ── Port trunk vers le routeur ───────────────────
SW-CORE(config)# interface GigabitEthernet0/1
SW-CORE(config-if)# switchport mode trunk
SW-CORE(config-if)# exit

SW-CORE(config)# end
SW-CORE# write memory
```

✅ **Vérification :** `show vlan brief` — les VLANs 10, 20, 30 doivent apparaître avec leurs ports respectifs.

---

### A4 — Configurer le Routeur R-SIOSARL

```
Router> enable
Router# configure terminal
Router(config)# hostname R-SIOSARL

! ── Interface physique (pas d'IP, juste no shutdown) ──
R-SIOSARL(config)# interface GigabitEthernet0/0
R-SIOSARL(config-if)# no shutdown
R-SIOSARL(config-if)# exit

! ── Sous-interface VLAN 10 ───────────────────────
R-SIOSARL(config)# interface GigabitEthernet0/0.10
R-SIOSARL(config-subif)# encapsulation dot1Q 10
R-SIOSARL(config-subif)# ip address 192.168.10.1 255.255.255.0
R-SIOSARL(config-subif)# exit

! ── Sous-interface VLAN 20 ───────────────────────
R-SIOSARL(config)# interface GigabitEthernet0/0.20
R-SIOSARL(config-subif)# encapsulation dot1Q 20
R-SIOSARL(config-subif)# ip address 192.168.20.1 255.255.255.0
R-SIOSARL(config-subif)# exit

! ── Sous-interface VLAN 30 ───────────────────────
R-SIOSARL(config)# interface GigabitEthernet0/0.30
R-SIOSARL(config-subif)# encapsulation dot1Q 30
R-SIOSARL(config-subif)# ip address 192.168.30.1 255.255.255.0
R-SIOSARL(config-subif)# exit

R-SIOSARL(config)# end
R-SIOSARL# write memory
```

✅ **Vérifications obligatoires :**

```
R-SIOSARL# show ip interface brief
! → G0/0, G0/0.10, G0/0.20, G0/0.30 doivent être "up/up"

R-SIOSARL# show ip route
! → 3 lignes "C" (Connected) pour 192.168.10.0, 192.168.20.0, 192.168.30.0
```

---

### A5 — Tester la Connectivité Inter-VLAN

Depuis **PC-RH** (Desktop → Command Prompt) :

```
C:\> ping 192.168.10.1     → Doit répondre (passerelle VLAN 10)
C:\> ping 192.168.20.1     → Doit répondre (passerelle VLAN 20, test routage)
C:\> ping 192.168.30.1     → Doit répondre (passerelle VLAN 30, test routage)
C:\> ping 192.168.20.10    → Doit répondre (PC-IT, VLAN 20)
C:\> ping 192.168.30.10    → Doit répondre (PC-DIR, VLAN 30)
```

✅ **Point de contrôle :** Capturer les `ping` réussis. Si un `ping` échoue, utiliser `tracert` pour identifier où le paquet est bloqué.

---

### A6 — Compléter la Fiche de Configuration Réseau

Remplir la **Fiche de Configuration Réseau** (voir Annexe 1) avec toutes les informations de votre configuration.

---

## PARTIE B — Exercices Bash Progressifs

### B1 — Premier Script : Variables et Echo

Créer un fichier `/home/user/scripts/bonjour.sh` :

```bash
#!/bin/bash
# ─────────────────────────────────────────────────────
# Script  : bonjour.sh
# Auteur  : [Votre nom]
# Date    : [Date]
# Rôle    : Démonstration variables et echo
# ─────────────────────────────────────────────────────

PRENOM="SimIO"
VERSION="1.0"
DATE_SCRIPT=$(date "+%d/%m/%Y à %H:%M")

echo "==================================="
echo "  Bienvenue dans SimIO Admin Tools"
echo "  Version : $VERSION"
echo "  Lancé le : $DATE_SCRIPT"
echo "==================================="
echo ""
echo "Bonjour, $PRENOM ! Ce script est fonctionnel."
```

Tester :
```bash
chmod +x bonjour.sh
./bonjour.sh
```

---

### B2 — Lecture et Conditions

Créer `/home/user/scripts/test_age.sh` :

```bash
#!/bin/bash
# ─────────────────────────────────────────────────────
# Script  : test_age.sh
# Rôle    : Démonstration read + conditions if/elif/else
# ─────────────────────────────────────────────────────

echo "=== Test d'âge ==="
read -p "Entrez votre âge : " AGE

# Vérifier que la saisie n'est pas vide
if [ -z "$AGE" ]
then
    echo "Erreur : aucune valeur saisie."
    exit 1
fi

# Vérifier que c'est un nombre positif
if [ $AGE -lt 0 ]
then
    echo "Erreur : l'âge ne peut pas être négatif."
    exit 1
fi

# Déterminer la catégorie
if [ $AGE -lt 18 ]
then
    echo "Vous êtes mineur ($AGE ans)."
elif [ $AGE -lt 65 ]
then
    echo "Vous êtes un adulte actif ($AGE ans)."
else
    echo "Vous êtes senior ($AGE ans)."
fi
```

---

### B3 — Tester l'Existence d'un Utilisateur

Créer `/home/user/scripts/check_user.sh` :

```bash
#!/bin/bash
# ─────────────────────────────────────────────────────
# Script  : check_user.sh
# Rôle    : Vérifier si un utilisateur Linux existe
# ─────────────────────────────────────────────────────

read -p "Entrez le nom d'utilisateur à vérifier : " LOGIN

if [ -z "$LOGIN" ]
then
    echo "Erreur : login vide."
    exit 1
fi

if id "$LOGIN" > /dev/null 2>&1
then
    echo "✅ L'utilisateur '$LOGIN' existe sur ce système."
    echo "   Détails : $(id $LOGIN)"
else
    echo "❌ L'utilisateur '$LOGIN' n'existe pas sur ce système."
fi
```

---

## PARTIE C — Script Complet de Création d'Utilisateurs Linux

### Contexte

SimIO SARL vient d'embaucher 5 nouveaux collaborateurs. Vous devez créer leurs comptes Linux sur le serveur de fichiers. Plutôt que de le faire manuellement, vous allez écrire un **script interactif** qui vous pose les questions et crée le compte en toute sécurité.

---

### C1 — Le Fichier d'Entrée

Créer le fichier `/home/user/nouveaux_employes.csv` :

```
prenom,nom,service,login
Alice,Martin,rh,a.martin
Bob,Dupont,informatique,b.dupont
Claire,Durand,direction,c.durand
David,Lemaire,informatique,d.lemaire
Emma,Bernard,rh,e.bernard
```

---

### C2 — Écrire le Script de Création

Créer `/home/user/scripts/creer_utilisateur.sh` :

```bash
#!/bin/bash
# ═══════════════════════════════════════════════════════════════
# Script  : creer_utilisateur.sh
# Auteur  : [Votre nom]
# Date    : [Date]
# Version : 1.0
# Rôle    : Création interactive et sécurisée d'un utilisateur Linux
#           pour SimIO SARL
# Épreuve : Portfolio BTS SIO SISR — Compétence B2.4
# ═══════════════════════════════════════════════════════════════

# ─── Vérification des droits root ───────────────────────────────
if [ $EUID -ne 0 ]
then
    echo "❌ Erreur : ce script doit être exécuté en tant que root (sudo)."
    exit 1
fi

# ─── En-tête ────────────────────────────────────────────────────
echo "======================================================="
echo "     OUTIL DE CRÉATION D'UTILISATEUR — SimIO SARL"
echo "======================================================="
echo ""

# ─── Saisie des informations ─────────────────────────────────────
read -p "Prénom de l'utilisateur         : " PRENOM
read -p "Nom de famille                  : " NOM
read -p "Login (ex: p.nom)               : " LOGIN
read -p "Service (rh/informatique/direction/commun) : " SERVICE
read -s -p "Mot de passe initial            : " MDP
echo ""

# ─── Validation : champs vides ───────────────────────────────────
if [ -z "$PRENOM" ] || [ -z "$NOM" ] || [ -z "$LOGIN" ] || [ -z "$SERVICE" ] || [ -z "$MDP" ]
then
    echo "❌ Erreur : tous les champs sont obligatoires."
    exit 1
fi

# ─── Validation : le login ne doit pas déjà exister ──────────────
if id "$LOGIN" > /dev/null 2>&1
then
    echo "❌ Erreur : l'utilisateur '$LOGIN' existe déjà sur ce système."
    exit 1
fi

# ─── Validation : le groupe/service doit exister ─────────────────
if ! getent group "$SERVICE" > /dev/null 2>&1
then
    echo "⚠️  Le groupe '$SERVICE' n'existe pas. Création en cours..."
    groupadd "$SERVICE"
    if [ $? -eq 0 ]
    then
        echo "✅ Groupe '$SERVICE' créé."
    else
        echo "❌ Erreur lors de la création du groupe '$SERVICE'."
        exit 1
    fi
fi

# ─── Récapitulatif et confirmation ───────────────────────────────
echo ""
echo "┌──────────────────────────────────────────────┐"
echo "│          RÉCAPITULATIF DE CRÉATION           │"
echo "├──────────────────────────────────────────────┤"
echo "│  Nom complet  : $PRENOM $NOM"
echo "│  Login        : $LOGIN"
echo "│  Service      : $SERVICE"
echo "│  Répertoire   : /home/$LOGIN"
echo "│  Shell        : /bin/bash"
echo "└──────────────────────────────────────────────┘"
echo ""
read -p "Confirmer la création ? (o/n) : " CONFIRMATION

if [ "$CONFIRMATION" != "o" ] && [ "$CONFIRMATION" != "O" ]
then
    echo "❌ Création annulée."
    exit 0
fi

# ─── Création de l'utilisateur ───────────────────────────────────
echo ""
echo "⏳ Création en cours..."

useradd -m \
        -s /bin/bash \
        -c "$PRENOM $NOM" \
        -G "$SERVICE" \
        "$LOGIN"

if [ $? -ne 0 ]
then
    echo "❌ Erreur lors de la création de l'utilisateur."
    exit 1
fi

# ─── Définition du mot de passe ──────────────────────────────────
echo "$LOGIN:$MDP" | chpasswd

if [ $? -ne 0 ]
then
    echo "❌ Erreur lors de la définition du mot de passe."
    exit 1
fi

# ─── Forcer le changement de mot de passe à la 1ère connexion ────
chage -d 0 "$LOGIN"

# ─── Confirmation finale ─────────────────────────────────────────
echo ""
echo "✅ Utilisateur '$LOGIN' ($PRENOM $NOM) créé avec succès !"
echo "   Groupe      : $SERVICE"
echo "   Répertoire  : /home/$LOGIN"
echo "   Mot de passe temporaire défini. L'utilisateur devra le changer à la 1ère connexion."
echo ""
echo "📋 Vérification :"
id "$LOGIN"

# ─── Journalisation ──────────────────────────────────────────────
LOG_FILE="/var/log/creation_utilisateurs.log"
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Création : $LOGIN ($PRENOM $NOM) - Service : $SERVICE" >> "$LOG_FILE"
echo "📄 Événement enregistré dans $LOG_FILE"
```

---

### C3 — Tester le Script

```bash
# Rendre exécutable
chmod +x creer_utilisateur.sh

# Exécuter avec sudo
sudo ./creer_utilisateur.sh
```

**Tests à réaliser et captures à prendre :**

| **Test** | **Données de test** | **Résultat attendu** |
|---|---|---|
| Test 1 — Création normale | Alice Martin / a.martin / rh | ✅ Création réussie |
| Test 2 — Login déjà existant | Relancer avec a.martin | ❌ Message d'erreur "existe déjà" |
| Test 3 — Champ vide | Ne pas saisir le prénom | ❌ Message d'erreur "champs obligatoires" |
| Test 4 — Annulation | Répondre "n" à la confirmation | ❌ Message "Création annulée" |
| Test 5 — Groupe inexistant | Saisir un service "marketing" | ⚠️ Groupe créé automatiquement |

---

### C4 — Vérifier les Créations

```bash
# Vérifier que l'utilisateur existe
id a.martin

# Voir le répertoire home créé
ls -la /home/a.martin

# Vérifier l'appartenance aux groupes
groups a.martin

# Consulter le journal de création
sudo cat /var/log/creation_utilisateurs.log
```

---
