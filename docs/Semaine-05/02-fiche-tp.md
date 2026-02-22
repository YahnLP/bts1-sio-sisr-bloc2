---
author: YLP
title: 🖥️ FICHE TP
---

### IV. TP Guidé Partie 1 : Configuration IP Manuelle sous Windows

#### Objectif

Configurer manuellement une adresse IPv4 statique sous Windows 10/11 et vérifier la connectivité par les commandes `ping`, `tracert` et `arp -a`.

---

#### Scénario

Vous devez configurer votre PC avec l'adresse suivante :
- **Adresse IP :** 192.168.50.XX (XX = votre numéro de poste, ex: poste 5 → 192.168.50.5)
- **Masque :** 255.255.255.0 (/24)
- **Passerelle :** 192.168.50.1
- **DNS :** 8.8.8.8

---

#### Étape 1 : Configurer l'IP via l'interface graphique (5 min)

1. Clic droit sur **l'icône réseau** (barre des tâches) → **Paramètres réseau et Internet**
2. **Modifier les options de l'adaptateur**
3. Clic droit sur la carte réseau active → **Propriétés**
4. Double-clic sur **Protocole Internet version 4 (TCP/IPv4)**
5. Sélectionner **"Utiliser l'adresse IP suivante"**
6. Remplir :
   - Adresse IP : `192.168.50.XX`
   - Masque de sous-réseau : `255.255.255.0`
   - Passerelle par défaut : `192.168.50.1`
7. Sélectionner **"Utiliser l'adresse de serveur DNS suivante"**
   - Serveur DNS préféré : `8.8.8.8`
8. **OK → OK** pour valider

---

#### Étape 2 : Vérifier avec ipconfig (2 min)

1. Ouvrir **Invite de commandes** (`cmd`)
2. Taper :
```cmd
ipconfig /all
```
3. ✅ Vérifier que l'adresse configurée est bien affichée
4. **Noter** l'adresse MAC (Adresse physique) de votre carte : __________________

---

#### Étape 3 : Tests de Connectivité (10 min)

**Test 1 - Boucle locale :**
```cmd
ping 127.0.0.1
```
✅ Doit fonctionner (teste la pile TCP/IP locale)

**Test 2 - Propre IP :**
```cmd
ping 192.168.50.XX
```
✅ Doit fonctionner (la machine se répond à elle-même)

**Test 3 - Voisin de classe :**
```cmd
ping 192.168.50.YY
```
*(YY = numéro de poste d'un voisin)*
✅ Doit fonctionner si votre voisin a configuré la même plage réseau

**Test 4 - Table ARP (Exercice d'observation):**
```cmd
arp -a
```
📌 **Observation :** Après le ping du voisin, sa MAC doit apparaître dans la table ARP.
- IP de votre voisin dans la table : _______________
- MAC de votre voisin dans la table : _______________
- Type (dynamique) : _______________

**Test 5 - Passerelle (si connectée au réseau de la salle) :**
```cmd
ping 192.168.50.1
```
*(Si la passerelle n'existe pas, essayer la passerelle réelle de la salle, indiquée par l'enseignant)*

**Test 6 - Résolution DNS :**
```cmd
ping www.google.fr
```
*(Si Internet accessible depuis la salle)*

**Test 7 - Tracert :**
```cmd
tracert 8.8.8.8
```
📌 **Observation :** Combien de sauts (routeurs) jusqu'à Google ? ___________

---

#### Étape 4 : Configuration via netsh (Ligne de Commande Windows) - Bac Pro CIEL

Pour les apprenants avancés : configuration IP **sans interface graphique** (utile pour scripts batch) :

```cmd
REM Configurer l'IP (remplacer "Ethernet0" par le nom de votre interface)
netsh interface ip set address "Ethernet0" static 192.168.50.XX 255.255.255.0 192.168.50.1

REM Configurer le DNS
netsh interface ip set dns "Ethernet0" static 8.8.8.8

REM Vérifier
netsh interface ip show config "Ethernet0"

REM Revenir en DHCP
netsh interface ip set address "Ethernet0" dhcp
```

---

### V. TP Guidé Partie 2 : Installation de Debian en Machine Virtuelle

#### Objectif

Installer **Debian 12 "Bookworm"** dans une machine virtuelle VirtualBox, démarrer et se connecter pour la première fois.

---

#### Pourquoi Linux pour un technicien SISR ?

| **Stat** | **Chiffre** |
|----------|-------------|
| Serveurs web (Linux vs Windows) | **Linux : ~80%** (Apache, Nginx) |
| Serveurs cloud (AWS, Azure, GCP) | Linux : 75%+ |
| Routeurs/Switches (firmware) | Basé Linux ou Unix |
| Smartphones Android | **Noyau Linux** |
| Salaire technicien Linux vs Windows only | +15-25% (source : études sectorielles) |

> *"En BTS SIO SISR, vous aurez systématiquement besoin de Linux. L'épreuve E5 peut inclure un serveur Linux. Autant s'y mettre maintenant."*

---

#### Étape 1 : Créer la Machine Virtuelle (5 min)

1. Ouvrir **VirtualBox**
2. Cliquer sur **Nouveau**
3. Renseigner :
   - **Nom :** `Debian-S5`
   - **Type :** Linux
   - **Version :** Debian (64-bit)
4. **RAM :** 1024 Mo (1 Go minimum, 2 Go recommandé)
5. **Disque dur :** Créer un disque virtuel VDI de **10 Go** (dynamiquement alloué)
6. **Processeur :** 1 ou 2 vCPU
7. Cliquer sur **Créer**

---

#### Étape 2 : Monter l'ISO Debian

1. Sélectionner la VM `Debian-S5` → **Configuration** (engrenage)
2. Onglet **Stockage**
3. Cliquer sur le lecteur CD (Vide)
4. Icône de disque à droite → **Choisir un fichier de disque**
5. Sélectionner le fichier ISO Debian (fourni par l'enseignant)
6. **OK**

---

#### Étape 3 : Installation Debian (20 min)

1. **Démarrer** la VM (bouton Start vert)
2. Choisir **Install** (pas "Graphical Install" pour garder la VM légère)
3. Suivre les étapes :

| **Étape** | **Choix** |
|-----------|-----------|
| Langue | **French** |
| Pays | **France** |
| Clavier | **fr** (Azerty) |
| Hostname | **debian-tp** |
| Domaine | laisser vide |
| **Mot de passe root** | `Azerty123!` (noter précieusement !) |
| **Nouveau utilisateur** | Votre prénom (ex: `pierre`) |
| **Mot de passe utilisateur** | `Azerty123!` |
| Partitionnement | **Tout dans une seule partition** |
| Disque | sélectionner le disque virtuel (sda) |
| Confirmation partitions | **Oui** (écraser) |
| Miroir apt | **france** → **deb.debian.org** |
| Proxy | laisser vide |
| Sondage popularité | **Non** |
| **Logiciels à installer** | ⚠️ **DÉCOCHER tout** sauf `utilitaires usuels du système` |
| GRUB | **Oui** → sélectionner `/dev/sda` |

4. **Redémarrage** → ✅ Retirer l'ISO automatiquement (VirtualBox le fait)

---

#### Étape 4 : Premier Démarrage et Connexion

1. La VM démarre → Écran texte noir avec `debian-tp login:`
2. Saisir votre **nom d'utilisateur** (ex: `pierre`) → **Entrée**
3. Saisir le **mot de passe** (les caractères ne s'affichent pas, c'est normal !) → **Entrée**
4. ✅ Vous êtes connecté ! Le prompt affiche : `pierre@debian-tp:~$`

**Qu'est-ce que ce prompt ?**
```
pierre     @    debian-tp    :    ~         $
Utilisateur     Nom machine       Répertoire    Niveau non-root
                                  courant (~=
                                  home)
```

---

### VI. TP Guidé Partie 3 : Commandes Linux de Base et Configuration IP

#### A. Les 10 Commandes Linux Essentielles (à connaître par cœur)

**Pour naviguer dans l'arborescence :**

```bash
pwd                 # Print Working Directory - afficher le répertoire actuel
ls                  # List - lister le contenu du répertoire
ls -la              # List détaillé avec droits, taille, fichiers cachés
cd /etc             # Change Directory - aller dans /etc
cd ..               # Remonter d'un niveau
cd ~                # Aller dans son dossier home
```

**Pour gérer des fichiers :**

```bash
cat /etc/hostname   # Afficher le contenu d'un fichier
mkdir test          # Créer un répertoire
rm -r test          # Supprimer un répertoire et son contenu
cp fichier.txt /tmp # Copier un fichier
mv fichier.txt doc.txt  # Renommer/déplacer
```

**Pour s'informer :**

```bash
whoami              # Afficher l'utilisateur courant
uname -a            # Infos sur le système (version noyau, architecture)
df -h               # Espace disque (human readable)
free -h             # Mémoire RAM disponible
man ls              # Manuel d'utilisation de la commande ls (q pour quitter)
```

---

**Exercice 1 :** Répondre aux questions en utilisant uniquement la ligne de commande :

| **Question** | **Commande** | **Réponse** |
|--------------|--------------|-------------|
| Dans quel répertoire suis-je ? | `pwd` | ____________ |
| Quel est mon nom d'utilisateur ? | `whoami` | ____________ |
| Quelle version du noyau Linux ? | `uname -r` | ____________ |
| Quelle est la RAM disponible ? | `free -h` | ____________ |
| Combien d'espace disque libre ? | `df -h` | ____________ |
| Contenu du fichier /etc/hostname ? | `cat /etc/hostname` | ____________ |

---

#### B. Configuration IP sous Linux

**Afficher les interfaces réseau actuelles :**

```bash
ip addr show
# ou en abrégé :
ip a
```

**Résultat typique :**
```
1: lo: <LOOPBACK,UP,LOWER_UP>
    link/loopback 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo

2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP>
    link/ether 00:0c:29:ab:cd:ef brd ff:ff:ff:ff:ff:ff
    inet 192.168.75.128/24 brd 192.168.75.255 scope global dynamic ens33
```

**Éléments à identifier :**
- `lo` : Interface de **loopback** (toujours présente, 127.0.0.1)
- `ens33` (ou `eth0`, `enp0s3`...) : Interface **réseau principale**
- `link/ether` : Adresse **MAC**
- `inet` : Adresse **IPv4** avec masque CIDR
- `dynamic` : IP assignée par DHCP

---

**Configurer une IP statique (temporaire - perdue au redémarrage) :**

```bash
# Ajouter une IP (en root ou avec sudo)
sudo ip addr add 192.168.50.200/24 dev ens33

# Vérifier
ip addr show ens33

# Ajouter la passerelle par défaut
sudo ip route add default via 192.168.50.1

# Vérifier la table de routage
ip route show
```

⚠️ **Cette configuration est temporaire** (perdue au redémarrage). Pour la rendre permanente, il faut modifier le fichier `/etc/network/interfaces`.

---

**Configurer une IP permanente sous Debian :**

```bash
# Se connecter en root (ou utiliser sudo)
su -
# Entrer le mot de passe root

# Éditer le fichier de configuration réseau
nano /etc/network/interfaces
```

**Contenu actuel (DHCP) :**
```
# This file describes the network interfaces available on your system

auto lo
iface lo inet loopback

allow-hotplug ens33
iface ens33 inet dhcp
```

**Modifier pour IP statique :**
```
auto lo
iface lo inet loopback

auto ens33
iface ens33 inet static
    address 192.168.50.200
    netmask 255.255.255.0
    gateway 192.168.50.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

**Appliquer les changements :**
```bash
# Redémarrer le service réseau
systemctl restart networking

# Vérifier
ip addr show ens33
```

---

**Exercice 2 :**

1. Afficher les interfaces réseau de votre VM Debian : `ip a`
2. Noter le nom de l'interface réseau principale : ___________
3. Quelle adresse IP votre VM a-t-elle actuellement ? ___________
4. Ajouter temporairement l'IP `192.168.50.200/24` : `sudo ip addr add 192.168.50.200/24 dev <nom_interface>`
5. Vérifier : `ip addr show`
6. Tester la connectivité vers votre propre VM : `ping -c 4 192.168.50.200`

---

#### C. Tableau Comparatif Windows / Linux

| **Opération** | **Windows** | **Linux** |
|---------------|-------------|-----------|
| Afficher config IP | `ipconfig /all` | `ip addr show` |
| Configurer IP (GUI) | Paramètres réseau → adaptateur | *(pas de GUI en mode serveur)* |
| Configurer IP (CLI) | `netsh interface ip set address...` | `ip addr add .../24 dev ens33` |
| Config IP permanente | Paramètres réseau | `/etc/network/interfaces` |
| Afficher table routage | `route print` | `ip route show` |
| Ajouter passerelle | `route add` | `ip route add default via ...` |
| Ping | `ping 8.8.8.8` (4 paquets puis stop) | `ping -c 4 8.8.8.8` (-c obligatoire !) |
| Traceroute | `tracert 8.8.8.8` | `traceroute 8.8.8.8` |
| Table ARP | `arp -a` | `arp -n` ou `ip neigh show` |
| DNS | `nslookup www.google.fr` | `nslookup www.google.fr` ou `dig www.google.fr` |
| Nom d'hôte | `hostname` | `hostname` ou `cat /etc/hostname` |

---

### VII. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|----------------|
| **CIDR** | Classless Inter-Domain Routing - Notation du masque par suffixe `/xx` |
| **Classe A** | Adresses 1.x.x.x à 126.x.x.x - Très grands réseaux |
| **Classe B** | Adresses 128.x.x.x à 191.x.x.x - Grands réseaux |
| **Classe C** | Adresses 192.x.x.x à 223.x.x.x - Petits réseaux (PME) |
| **Adresse privée** | Adresse non routable sur Internet (RFC 1918) |
| **Adresse publique** | Adresse routable sur Internet, unique au monde |
| **APIPA** | Automatic Private IP Addressing - 169.254.x.x, si pas de DHCP |
| **Loopback** | 127.0.0.1 - La machine elle-même, jamais sur un réseau physique |
| **ARP** | Address Resolution Protocol - Résolution IP → MAC |
| **ARP Request** | Message broadcast demandant "Qui a l'IP X ?" |
| **ARP Reply** | Réponse unicast "C'est moi, ma MAC est..." |
| **Table ARP** | Cache local des correspondances IP ↔ MAC |
| **ARP Spoofing** | Attaque falsifiant les réponses ARP (man-in-the-middle) |
| **ICMP** | Internet Control Message Protocol - Messages d'erreur et diagnostic |
| **TTL** | Time To Live - Compteur décrémenté à chaque routeur |
| **ping** | Test de connectivité utilisant ICMP Echo Request/Reply |
| **tracert/traceroute** | Trace le chemin des paquets routeur par routeur |
| **Debian** | Distribution Linux stable, base de nombreux serveurs |
| **Terminal / shell** | Interface en ligne de commande Linux |
| **Root** | Super-utilisateur Linux (équivalent Administrateur Windows) |
| **Prompt** | Invite de commande (ex: `user@hostname:~$`) |
| **Chemin absolu** | Chemin depuis la racine (ex: `/etc/network/interfaces`) |
| **Chemin relatif** | Chemin depuis le répertoire courant (ex: `../dossier`) |

---

### VIII. Exercices de Fin de Séance

#### Exercice 1 : Identification Rapide (5 questions)

Pour chaque adresse, donner : classe, privée ou publique, CIDR de son réseau par défaut.

| **Adresse** | **Classe** | **Privée/Publique** | **Réseau (CIDR défaut)** |
|-------------|------------|---------------------|--------------------------|
| `172.20.5.10` | | | |
| `10.0.0.254` | | | |
| `201.45.2.1` | | | |
| `169.254.1.5` | | | |
| `192.168.200.50` | | | |

---

#### Exercice 2 : Calcul CIDR

Soit `10.10.10.0/27` :
1. Masque décimal : ___________
2. Adresse broadcast : ___________
3. Nombre d'hôtes utilisables : ___________
4. Plage d'hôtes : ___________ à ___________

---

#### Exercice 3 : Lecture ARP

Vous exécutez `arp -a` sur votre PC (`192.168.1.100`) et obtenez :
```
192.168.1.1     aa-bb-cc-dd-ee-01   dynamique
192.168.1.25    aa-bb-cc-dd-ee-25   dynamique
192.168.1.255   ff-ff-ff-ff-ff-ff   statique
```

1. Quelle est l'adresse MAC de votre passerelle ? ___________
2. Vous voulez pinger `192.168.1.50`, quelle trame ARP sera envoyée ?
3. Vous voulez accéder à `www.google.fr`, vers quelle MAC la trame sera-t-elle envoyée ?

---

#### Exercice 4 : Diagnostic Tracert

Vous effectuez `tracert 8.8.8.8` et obtenez :
```
  1     2ms    192.168.1.1
  2    12ms    10.10.0.1
  3     *  *  *
  4    14ms    72.14.202.76
  5    15ms    8.8.8.8
```

1. Qu'est-ce que `192.168.1.1` ? ___________
2. Pourquoi le saut 3 affiche-t-il `* * *` ? ___________
3. Combien de sauts (routeurs traversés) jusqu'à `8.8.8.8` ? ___________
4. Le TTL initial du ping était 128 (Windows). Quelle valeur de TTL recevrez-vous de `8.8.8.8` ? ___________

---

### IX. Ressources pour Approfondir

**Documentation :**
- RFC 826 (ARP) : Standard officiel du protocole ARP
- RFC 792 (ICMP) : Standard officiel ICMP
- Documentation Debian : https://www.debian.org/doc/manuals/debian-reference/

**Vidéos YouTube :**
- "Comprendre ARP en 5 minutes" - Animation du processus
- "Installer Debian en VM VirtualBox pas à pas" - Tutoriel complet
- "Les bases de Linux pour débutants" - Commandes essentielles

**Pratique Linux :**
- **OverTheWire Bandit** : Wargame pour apprendre Linux par la pratique (gratuit)
- **TryHackMe - Pre-Security** : Parcours interactif avec VM Linux en ligne (gratuit)
- **Linux Survival** : Site web avec émulateur terminal (linuxsurvival.com)

---

### ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Identifier la classe (A/B/C) d'une adresse IP au premier regard
- [ ] Distinguer adresse privée et adresse publique
- [ ] Reconnaître une adresse APIPA (169.254.x.x) et comprendre sa signification
- [ ] Lire et utiliser la notation CIDR (/8, /16, /24...)
- [ ] Calculer adresse réseau, broadcast, plage hôtes à partir d'une IP et d'un masque
- [ ] Expliquer le fonctionnement d'ARP (request/reply, table ARP)
- [ ] Expliquer le rôle d'ICMP et interpréter la sortie d'un ping
- [ ] Utiliser `tracert` et interpréter les sauts et les `* * *`
- [ ] Configurer une IP statique sous Windows (interface graphique)
- [ ] Installer Debian en VM et se connecter en ligne de commande
- [ ] Utiliser les commandes Linux de base (ls, cd, pwd, ip addr, ping)
- [ ] Comparer la gestion réseau Windows vs Linux

---
