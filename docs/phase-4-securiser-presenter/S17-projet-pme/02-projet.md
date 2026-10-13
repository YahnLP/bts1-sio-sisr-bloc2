# Projet – Infrastructure de la PME SimIO

*BTS SIO SISR 1re année – Bloc 2 – S17*

> ##  Rappel du Cahier des Charges
> Bloc réseau : `192.168.0.0/22` — 6 VLANs — Routage inter-VLAN
> Services Windows Server : AD DS + GPO + DHCP + DNS + Partages
> Service Linux : FTP anonyme (lecture) + SFTP authentifié avec chroot

---

## PHASE 1 — Conception du Plan d'Adressage VLSM

*Durée estimée : 30 min — Support : papier ou draw.io*

### 1.1 — Calculer le Plan d'Adressage

Compléter le tableau suivant en appliquant la méthode VLSM (grand → petit) :

| **Priorité** | **VLAN** | **Besoin hôtes** | **Taille bloc (/n)** | **Réseau alloué** | **Masque** | **1ère IP hôte** | **Dernière IP hôte** | **Broadcast** | **Passerelle** |
|---|---|---|---|---|---|---|---|---|---|
| 1 | VLAN 40 — Commercial | 40 | | | | | | | |
| 2 | VLAN 10 — RH | 25 | | | | | | | |
| 3 | VLAN 50 — Comptabilité | 15 | | | | | | | |
| 4 | VLAN 20 — Informatique | 10 | | | | | | | |
| 5 | VLAN 99 — Serveurs | 10 | | | | | | | |
| 6 | VLAN 30 — Direction | 5 | | | | | | | |

>  **À faire valider par l'enseignant avant de passer à la phase 2.**

### 1.2 — Attribuer des Adresses Fixes aux Serveurs

Dans le VLAN 99, réserver manuellement :

| **Serveur** | **Rôle** | **Adresse IP** |
|---|---|---|
| SRV-AD01 | Contrôleur de domaine AD DS + DNS | 192.168.0.145 |
| SRV-DHCP | Serveur DHCP | 192.168.0.146 |
| SRV-FTP | Serveur FTP/SFTP Linux | 192.168.0.147 |

### 1.3 — Réaliser le Schéma Réseau

Dessiner (ou modéliser dans draw.io) le schéma complet de l'infrastructure incluant :
- Les deux switches (Bât. A et Bât. B) et le routeur inter-bâtiments
- Les VLANs avec leur numéro et leur plage d'adresses
- Le routeur de bord avec ses sous-interfaces (router-on-a-stick) ou switch L3
- Les serveurs dans le VLAN 99 avec leurs adresses IP
- Le lien trunk entre switch et routeur

---

## PHASE 2 — Déploiement Réseau Cisco Packet Tracer

*Durée estimée : 40 min*

### 2.1 — Créer la Topologie

Dans Packet Tracer, construire la topologie suivante :

```
  [PC-Commercial]──────────────────────────►
  [PC-RH]         [SW-BatA]        [SW-BatB]    [Plusieurs PCs]
  [PC-IT]    ────►          ◄────►          ──►
  [PC-Direction]                                (VLANs 40, 50)
  [SRV-AD01]
  [SRV-DHCP]
  [SRV-FTP]
       │                │
       └── VLANs 10-99  └── Lien trunk inter-switches
                            (Lien Bât.A ↔ Bât.B)
                                     │
                                  [ROUTEUR]
                               (Routage inter-VLAN)
```

### 2.2 — Configurer SW-BatA

```
! Créer tous les VLANs
SW-BatA(config)# vlan 10
SW-BatA(config-vlan)# name RH
[Répéter pour VLANs 20, 30, 40, 50, 99]

! Assigner les ports d'accès (adapter selon votre topologie)
SW-BatA(config)# interface range FastEthernet0/1-5
SW-BatA(config-if-range)# switchport mode access
SW-BatA(config-if-range)# switchport access vlan 10    ! (adapter par port)

! Configurer le trunk vers le routeur
SW-BatA(config)# interface GigabitEthernet0/1
SW-BatA(config-if)# switchport mode trunk

! Configurer le trunk vers SW-BatB (inter-bâtiments)
SW-BatA(config)# interface GigabitEthernet0/2
SW-BatA(config-if)# switchport mode trunk
SW-BatA(config-if)# switchport trunk allowed vlan 40,50
```

### 2.3 — Configurer le Routeur (Router-on-a-Stick)

```
! Une sous-interface par VLAN
Router(config)# interface GigabitEthernet0/0
Router(config-if)# no shutdown

Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address [passerelle VLAN 10] [masque]

[Répéter pour chaque VLAN]
```

### 2.4 — Tester le Routage Inter-VLAN

Depuis chaque PC, tester :
```
ping [passerelle de son VLAN]    → Doit répondre
ping [IP d'un PC d'un autre VLAN] → Doit répondre
```

✅ Capturer les `ping` réussis inter-VLAN : **Capture P1**.

---

## PHASE 3 — Déploiement Windows Server : AD DS + OUs + GPO

*Durée estimée : 30 min*

### 3.1 — Installer et Configurer AD DS

```powershell
# Installer le rôle AD DS
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

# Promouvoir en contrôleur de domaine
Install-ADDSForest `
    -DomainName "siosarl.local" `
    -DomainNetbiosName "SIOSARL" `
    -InstallDns:$true `
    -SafeModeAdministratorPassword (ConvertTo-SecureString "Admin!2024" -AsPlainText -Force) `
    -Force
```

### 3.2 — Créer la Structure des OUs

```powershell
Import-Module ActiveDirectory
$Dom = "DC=siosarl,DC=local"

# Créer les OUs de services
$Services = @("RH","Informatique","Direction","Commercial","Comptabilite")
foreach ($Service in $Services) {
    New-ADOrganizationalUnit -Name $Service -Path $Dom -ErrorAction SilentlyContinue
    New-ADOrganizationalUnit -Name "Ordinateurs" -Path "OU=$Service,$Dom" -ErrorAction SilentlyContinue
    New-ADOrganizationalUnit -Name "Utilisateurs" -Path "OU=$Service,$Dom" -ErrorAction SilentlyContinue
}

# Créer les groupes de sécurité
foreach ($Service in $Services) {
    New-ADGroup -Name "GRP_$Service" -GroupScope Global -GroupCategory Security `
        -Path "OU=$Service,$Dom" -ErrorAction SilentlyContinue
}
```

✅ Capturer la structure AD dans la console ADUC : **Capture P2**.

### 3.3 — Créer les Utilisateurs AD

Utiliser le script PowerShell de S14 (adapté) ou créer manuellement 2 utilisateurs par service.

```powershell
# Exemple pour RH
New-ADUser `
    -Name "Alice Martin" -GivenName "Alice" -Surname "Martin" `
    -SamAccountName "a.martin" -UserPrincipalName "a.martin@siosarl.local" `
    -Path "OU=Utilisateurs,OU=RH,$Dom" `
    -AccountPassword (ConvertTo-SecureString "Azerty!2024" -AsPlainText -Force) `
    -ChangePasswordAtLogon $true -Enabled $true

Add-ADGroupMember -Identity "GRP_RH" -Members "a.martin"
```

### 3.4 — Créer et Lier les GPO

```powershell
Import-Module GroupPolicy

# GPO restriction Panneau de configuration
New-GPO -Name "GPO_Restriction_Standard" | Out-Null

# Lier aux OUs standards (pas Informatique)
foreach ($OU in @("RH","Commercial","Comptabilite","Direction")) {
    New-GPLink -Name "GPO_Restriction_Standard" -Target "OU=$OU,$Dom"
}

# GPO fond d'écran (lier au domaine entier)
New-GPO -Name "GPO_Fond_Ecran_SimIO" | Out-Null
New-GPLink -Name "GPO_Fond_Ecran_SimIO" -Target $Dom
```

>  **Les paramètres fins des GPO** (chemin vers le fond d'écran, paramètre exact de restriction) se configurent dans l'éditeur graphique **Gestion des stratégies de groupe** (`gpmc.msc`). Documenter chaque paramètre modifié dans la fiche de documentation.

✅ Capturer les GPO créées et liées dans `gpmc.msc` : **Capture P3**.

---

## PHASE 4 — DHCP + DNS + Partages de Fichiers

*Durée estimée : 30 min*

### 4.1 — Configurer le Serveur DHCP

```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
Add-DhcpServerInDC -DnsName "srv-ad01.siosarl.local" -IPAddress 192.168.0.145

# Créer une étendue par VLAN (adapter avec votre plan d'adressage)
# Exemple VLAN 10 RH :
Add-DhcpServerv4Scope -Name "VLAN10-RH" `
    -StartRange 192.168.0.66 -EndRange 192.168.0.93 `
    -SubnetMask 255.255.255.224 -LeaseDuration "8.00:00:00"

Set-DhcpServerv4OptionValue -ScopeId 192.168.0.64 `
    -Router 192.168.0.65 -DnsServer 192.168.0.145 -DnsDomain "siosarl.local"

# [Répéter pour chaque VLAN]
```

### 4.2 — Vérifier la Zone DNS

```powershell
# La zone siosarl.local a été créée avec AD DS
# Vérifier les enregistrements
Get-DnsServerResourceRecord -ZoneName "siosarl.local" -RRType "A"

# Créer un enregistrement pour le serveur FTP
Add-DnsServerResourceRecordA -ZoneName "siosarl.local" `
    -Name "srv-ftp" -IPv4Address "192.168.0.147"
```

### 4.3 — Créer les Partages de Fichiers

```powershell
# Créer l'arborescence (comme en S12)
$Dossiers = @("RH","Informatique","Direction","Comptabilite","Commercial","Commun")
foreach ($D in $Dossiers) {
    New-Item -Path "C:\Partages\$D" -ItemType Directory -Force | Out-Null
    # Partager le dossier
    New-SmbShare -Name $D -Path "C:\Partages\$D" -FullAccess "Tout le monde" -ErrorAction SilentlyContinue
}
```

>  Configurer les droits NTFS par groupe AD comme en S12 (voir la fiche de droits de S12).

✅ Capturer les partages créés dans `fsmgmt.msc` : **Capture P4**.

---

## PHASE 5 — Serveur FTP/SFTP Linux (Debian)

*Durée estimée : 35 min*

### 5.1 — Installer vsftpd

```bash
sudo apt update
sudo apt install vsftpd -y
sudo systemctl enable --now vsftpd
sudo systemctl status vsftpd
```

---

### 5.2 — Configurer le FTP Anonyme en Lecture Seule

```bash
# Créer le répertoire public
sudo mkdir -p /srv/ftp/public
sudo chown nobody:nogroup /srv/ftp/public
sudo chmod 755 /srv/ftp/public

# Créer un fichier de test
echo "Bienvenue sur le serveur FTP SimIO SARL" | sudo tee /srv/ftp/public/bienvenue.txt

# Sauvegarder la config originale
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.backup

# Configurer vsftpd
sudo nano /etc/vsftpd.conf
```

Modifier/vérifier les paramètres suivants :

```bash
# ─── FTP anonyme ──────────────────────────────────────────────────────
anonymous_enable=YES
anon_root=/srv/ftp/public
no_anon_password=YES
anon_upload_enable=NO
anon_mkdir_write_enable=NO

# ─── FTP local (désactivé pour l'instant — SFTP prend le relais) ──────
local_enable=NO

# ─── Sécurité ─────────────────────────────────────────────────────────
xferlog_enable=YES
listen=YES
listen_ipv6=NO

# ─── Mode passif ──────────────────────────────────────────────────────
pasv_enable=YES
pasv_min_port=10000
pasv_max_port=10100
```

```bash
# Redémarrer vsftpd
sudo systemctl restart vsftpd

# Tester en FTP anonyme depuis la VM cliente
ftp [IP_SERVEUR_FTP]
# Login : anonymous
# Password : (entrée vide)
# Commande : ls  → doit lister bienvenue.txt
# Commande : get bienvenue.txt
```

✅ Capturer la session FTP anonyme réussie : **Capture P5**.

---

### 5.3 — Configurer le SFTP Authentifié avec Chroot

```bash
# ─── Créer le groupe et les utilisateurs SFTP ──────────────────────────
sudo groupadd sftpusers

# Créer les utilisateurs SFTP (un par service)
for SERVICE in rh informatique direction commercial comptabilite
do
    # Créer l'utilisateur sans shell interactif
    sudo useradd -m -s /usr/sbin/nologin -G sftpusers "sftp_${SERVICE}"
    echo "sftp_${SERVICE}:Sftp!2024" | sudo chpasswd

    # Créer la structure de répertoires pour le chroot
    sudo mkdir -p "/srv/sftp/sftp_${SERVICE}/upload"

    # Répertoire chroot : propriétaire root, non writable par l'user
    sudo chown root:root "/srv/sftp/sftp_${SERVICE}"
    sudo chmod 755 "/srv/sftp/sftp_${SERVICE}"

    # Répertoire upload : propriétaire de l'utilisateur
    sudo chown "sftp_${SERVICE}:sftpusers" "/srv/sftp/sftp_${SERVICE}/upload"
    sudo chmod 755 "/srv/sftp/sftp_${SERVICE}/upload"

    echo "✅ Utilisateur SFTP créé : sftp_${SERVICE}"
done
```

```bash
# ─── Configurer SSH pour le SFTP chroot ────────────────────────────────
sudo nano /etc/ssh/sshd_config
```

Ajouter à la **fin** du fichier :

```
# ─── Configuration SFTP avec chroot pour le groupe sftpusers ──────────
Match Group sftpusers
    ForceCommand internal-sftp
    ChrootDirectory /srv/sftp/%u
    X11Forwarding no
    AllowTcpForwarding no
    PasswordAuthentication yes
```

```bash
# Tester la configuration SSH avant de redémarrer
sudo sshd -t

# Si pas d'erreur, redémarrer SSH
sudo systemctl restart ssh
```

---

### 5.4 — Tester le SFTP depuis la VM Cliente

```bash
# Test depuis la VM cliente Linux
sftp sftp_rh@[IP_SERVEUR_FTP]
# Password : Sftp!2024

# Commandes SFTP
sftp> ls           # Doit afficher : upload
sftp> cd upload
sftp> put /etc/hostname   # Upload d'un fichier test
sftp> ls           # Doit afficher : hostname
sftp> exit
```

```bash
# Vérifier que l'utilisateur SFTP ne peut PAS se connecter en SSH
ssh sftp_rh@[IP_SERVEUR_FTP]
# → Doit afficher "This service allows sftp connections only." ou se déconnecter
```

✅ Capturer la session SFTP réussie (upload de fichier) : **Capture P6**.
✅ Capturer le refus de connexion SSH : **Capture P7**.

---

### 5.5 — Vérifier les Logs FTP

```bash
# Journal des accès FTP
sudo cat /var/log/vsftpd.log

# Tester le refus d'upload en anonyme
ftp [IP_SERVEUR_FTP]
ftp> put /etc/hostname   # Doit être refusé
```

---

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.1** – Concevoir une solution d'infrastructure réseau | | |
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |
| **B3.3** – Sécuriser les équipements et les usages des utilisateurs | | |
| **B1.4** – Travailler en mode projet | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant). Le choix des compétences pour cette séance est une proposition de l'enseignant, **à valider**.

**Je sais faire, sans aide :**

- [ ] Concevoir un plan d'adressage VLSM et une architecture VLAN par service ;
- [ ] Déployer le routage inter-VLAN, l'annuaire, les GPO, DHCP, DNS et les partages ;
- [ ] Sécuriser l'accès d'un serveur de fichiers (SFTP) ;
- [ ] Documenter en continu la configuration réalisée.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________
