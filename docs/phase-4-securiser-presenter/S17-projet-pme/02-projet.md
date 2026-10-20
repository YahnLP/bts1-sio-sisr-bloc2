# Projet – Infrastructure de la PME SimIO

*BTS SIO SISR 1re année – Bloc 2 – S17*

> [!NOTE]
> **But.** Situation : Une PME de 6 services vous confie son infrastructure : de l'adressage à l'annuaire, livrez un réseau qui fonctionne et que quelqu'un d'autre saura exploiter.
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le simulateur réseau ou Packet Tracer servent aux manipulations d'équipements réseau.


> ##  Rappel du Cahier des Charges
> Bloc réseau : `192.168.0.0/22` — 6 VLANs — Routage inter-VLAN
> Services Windows Server : AD DS + GPO + DHCP + DNS + Partages
> Service Linux : FTP anonyme (lecture) + SFTP authentifié avec chroot

---

> [!NOTE]
> **Matériel réel ou simulation.** Si le matériel est disponible, fais la phase 2 avec la section 2.5 « Variante sur matériel réel » ; sinon travaille en simulation (Packet Tracer ou simulateur réseau) : le choix revient à l'enseignant. Voir [Matériel réel ou simulation](../../ressources/materiel-reel.md).

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

**J'anticipe.** Dans quel ordre attribues-tu les sous-réseaux VLSM (du plus grand au plus petit ou l'inverse) ? Pourquoi ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

Du **plus grand au plus petit** : on évite que de petits blocs fragmentent l'espace et empêchent de placer un grand sous-réseau aligné. On laisse de la **marge de croissance** à chaque VLAN.

</details>


---

## PHASE 2 — Déploiement Réseau Cisco Packet Tracer

*Durée estimée : 40 min*

### 2.1 — Créer la Topologie

Dans Packet Tracer, construire la topologie suivante :

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 1453.60 459.10" style="max-width:1939px;min-width:900px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 448.3)">
<g class="cluster cl">
<path fill="transparent" stroke="black" d="M281,-320.5C281,-320.5 1019,-320.5 1019,-320.5 1025,-320.5 1031,-326.5 1031,-332.5 1031,-332.5 1031,-417.5 1031,-417.5 1031,-423.5 1025,-429.5 1019,-429.5 1019,-429.5 281,-429.5 281,-429.5 275,-429.5 269,-423.5 269,-417.5 269,-417.5 269,-332.5 269,-332.5 269,-326.5 275,-320.5 281,-320.5"/>
<text text-anchor="start" x="277" y="-414.3" font-family="Helvetica,sans-Serif" font-size="14.00">Bâtiment A : ports d&#39;accès</text>
</g>
<g class="cluster cl">
<path fill="transparent" stroke="black" d="M1052,-320.5C1052,-320.5 1412,-320.5 1412,-320.5 1418,-320.5 1424,-326.5 1424,-332.5 1424,-332.5 1424,-417.5 1424,-417.5 1424,-423.5 1418,-429.5 1412,-429.5 1412,-429.5 1052,-429.5 1052,-429.5 1046,-429.5 1040,-423.5 1040,-417.5 1040,-417.5 1040,-332.5 1040,-332.5 1040,-326.5 1046,-320.5 1052,-320.5"/>
<text text-anchor="start" x="1048" y="-414.3" font-family="Helvetica,sans-Serif" font-size="14.00">Bâtiment B : ports d&#39;accès</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M232.5,-399C232.5,-399 41.5,-399 41.5,-399 35.5,-399 29.5,-393 29.5,-387 29.5,-387 29.5,-340 29.5,-340 29.5,-334 35.5,-328 41.5,-328 41.5,-328 232.5,-328 232.5,-328 238.5,-328 244.5,-334 244.5,-340 244.5,-340 244.5,-387 244.5,-387 244.5,-393 238.5,-399 232.5,-399"/>
<text text-anchor="start" x="99.5" y="-377" font-family="Helvetica,sans-Serif" font-size="15.00">ROUTEUR</text>
<text text-anchor="start" x="65" y="-360" font-family="Helvetica,sans-Serif" font-size="15.00">routage inter&#45;VLAN</text>
<text text-anchor="start" x="48.5" y="-343" font-family="Helvetica,sans-Serif" font-size="15.00">(sous&#45;interfaces dot1Q)</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M177,-256C177,-256 97,-256 97,-256 91,-256 85,-250 85,-244 85,-244 85,-231 85,-231 85,-225 91,-219 97,-219 97,-219 177,-219 177,-219 183,-219 189,-225 189,-231 189,-231 189,-244 189,-244 189,-250 183,-256 177,-256"/>
<text text-anchor="start" x="104" y="-234" font-family="Helvetica,sans-Serif" font-size="15.00">SW&#45;BatA</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M137,-327.96C137,-304.6 137,-274.72 137,-256.09"/>
<text text-anchor="start" x="137" y="-288.6" font-family="Helvetica,sans-Serif" font-size="13.00">trunk (Gi0/1)</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M92,-133C92,-133 12,-133 12,-133 6,-133 0,-127 0,-121 0,-121 0,-108 0,-108 0,-102 6,-96 12,-96 12,-96 92,-96 92,-96 98,-96 104,-102 104,-108 104,-108 104,-121 104,-121 104,-127 98,-133 92,-133"/>
<text text-anchor="start" x="19" y="-111" font-family="Helvetica,sans-Serif" font-size="15.00">SW&#45;BatB</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M84.82,-221.17C70.02,-214.14 55.6,-204.12 47,-190 36.55,-172.84 40.59,-149.15 45.4,-133.12"/>
<text text-anchor="start" x="53.5" y="-179.6" font-family="Helvetica,sans-Serif" font-size="13.00">trunk Bât.A ↔ Bât.B</text>
<text text-anchor="start" x="47" y="-165.6" font-family="Helvetica,sans-Serif" font-size="13.00">VLAN 40 et 50 (Gi0/2)</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M248.5,-133C248.5,-133 197.5,-133 197.5,-133 191.5,-133 185.5,-127 185.5,-121 185.5,-121 185.5,-108 185.5,-108 185.5,-102 191.5,-96 197.5,-96 197.5,-96 248.5,-96 248.5,-96 254.5,-96 260.5,-102 260.5,-108 260.5,-108 260.5,-121 260.5,-121 260.5,-127 254.5,-133 248.5,-133"/>
<text text-anchor="middle" x="223" y="-111" font-family="Helvetica,sans-Serif" font-size="15.00">BatA</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M167.13,-218.91C178.13,-211.23 189.86,-201.37 198,-190 210.31,-172.81 216.88,-149.12 220.15,-133.11"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M77.5,-37C77.5,-37 26.5,-37 26.5,-37 20.5,-37 14.5,-31 14.5,-25 14.5,-25 14.5,-12 14.5,-12 14.5,-6 20.5,0 26.5,0 26.5,0 77.5,0 77.5,0 83.5,0 89.5,-6 89.5,-12 89.5,-12 89.5,-25 89.5,-25 89.5,-31 83.5,-37 77.5,-37"/>
<text text-anchor="middle" x="52" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">BatB</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M52,-95.82C52,-79.01 52,-53.88 52,-37.1"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M1003,-390.5C1003,-390.5 923,-390.5 923,-390.5 917,-390.5 911,-384.5 911,-378.5 911,-378.5 911,-348.5 911,-348.5 911,-342.5 917,-336.5 923,-336.5 923,-336.5 1003,-336.5 1003,-336.5 1009,-336.5 1015,-342.5 1015,-348.5 1015,-348.5 1015,-378.5 1015,-378.5 1015,-384.5 1009,-390.5 1003,-390.5"/>
<text text-anchor="start" x="939.5" y="-368.5" font-family="Helvetica,sans-Serif" font-size="15.00">PC&#45;RH</text>
<text text-anchor="start" x="930" y="-351.5" font-family="Helvetica,sans-Serif" font-size="15.00">VLAN 10</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M859,-390.5C859,-390.5 779,-390.5 779,-390.5 773,-390.5 767,-384.5 767,-378.5 767,-378.5 767,-348.5 767,-348.5 767,-342.5 773,-336.5 779,-336.5 779,-336.5 859,-336.5 859,-336.5 865,-336.5 871,-342.5 871,-348.5 871,-348.5 871,-378.5 871,-378.5 871,-384.5 865,-390.5 859,-390.5"/>
<text text-anchor="start" x="799.5" y="-368.5" font-family="Helvetica,sans-Serif" font-size="15.00">PC&#45;IT</text>
<text text-anchor="start" x="786" y="-351.5" font-family="Helvetica,sans-Serif" font-size="15.00">VLAN 20</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M715,-390.5C715,-390.5 607,-390.5 607,-390.5 601,-390.5 595,-384.5 595,-378.5 595,-378.5 595,-348.5 595,-348.5 595,-342.5 601,-336.5 607,-336.5 607,-336.5 715,-336.5 715,-336.5 721,-336.5 727,-342.5 727,-348.5 727,-348.5 727,-378.5 727,-378.5 727,-384.5 721,-390.5 715,-390.5"/>
<text text-anchor="start" x="614" y="-368.5" font-family="Helvetica,sans-Serif" font-size="15.00">PC&#45;Direction</text>
<text text-anchor="start" x="628" y="-351.5" font-family="Helvetica,sans-Serif" font-size="15.00">VLAN 30</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M543,-390.5C543,-390.5 297,-390.5 297,-390.5 291,-390.5 285,-384.5 285,-378.5 285,-378.5 285,-348.5 285,-348.5 285,-342.5 291,-336.5 297,-336.5 297,-336.5 543,-336.5 543,-336.5 549,-336.5 555,-342.5 555,-348.5 555,-348.5 555,-378.5 555,-378.5 555,-384.5 549,-390.5 543,-390.5"/>
<text text-anchor="start" x="304" y="-368.5" font-family="Helvetica,sans-Serif" font-size="15.00">SRV&#45;AD01, SRV&#45;DHCP, SRV&#45;FTP</text>
<text text-anchor="start" x="387" y="-351.5" font-family="Helvetica,sans-Serif" font-size="15.00">VLAN 99</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M1395.5,-390.5C1395.5,-390.5 1266.5,-390.5 1266.5,-390.5 1260.5,-390.5 1254.5,-384.5 1254.5,-378.5 1254.5,-378.5 1254.5,-348.5 1254.5,-348.5 1254.5,-342.5 1260.5,-336.5 1266.5,-336.5 1266.5,-336.5 1395.5,-336.5 1395.5,-336.5 1401.5,-336.5 1407.5,-342.5 1407.5,-348.5 1407.5,-348.5 1407.5,-378.5 1407.5,-378.5 1407.5,-384.5 1401.5,-390.5 1395.5,-390.5"/>
<text text-anchor="start" x="1273.5" y="-368.5" font-family="Helvetica,sans-Serif" font-size="15.00">PC&#45;Commercial</text>
<text text-anchor="start" x="1298" y="-351.5" font-family="Helvetica,sans-Serif" font-size="15.00">VLAN 40</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M1202.5,-390.5C1202.5,-390.5 1067.5,-390.5 1067.5,-390.5 1061.5,-390.5 1055.5,-384.5 1055.5,-378.5 1055.5,-378.5 1055.5,-348.5 1055.5,-348.5 1055.5,-342.5 1061.5,-336.5 1067.5,-336.5 1067.5,-336.5 1202.5,-336.5 1202.5,-336.5 1208.5,-336.5 1214.5,-342.5 1214.5,-348.5 1214.5,-348.5 1214.5,-378.5 1214.5,-378.5 1214.5,-384.5 1208.5,-390.5 1202.5,-390.5"/>
<text text-anchor="start" x="1074.5" y="-368.5" font-family="Helvetica,sans-Serif" font-size="15.00">PC Comptabilité</text>
<text text-anchor="start" x="1102" y="-351.5" font-family="Helvetica,sans-Serif" font-size="15.00">VLAN 50</text>
</g>
</g>
</svg>
</figure>

> [!NOTE]
> Les numéros de VLAN et le placement des postes sont ceux du plan d'adressage de la phase 1 : **adapte-les** si ton plan diffère. Le schéma montre le principe : deux commutateurs reliés par un trunk limité aux VLAN 40 et 50, et un routeur qui assure le routage entre VLAN.

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
Router(config-subif)# ip helper-address 192.168.0.146    ! relais DHCP vers SRV-DHCP (VLAN clients, pas le VLAN 99)

[Répéter pour chaque VLAN]
```

### 2.4 — Tester le Routage Inter-VLAN

Depuis chaque PC, tester :
```
ping [passerelle de son VLAN]    → Doit répondre
ping [IP d'un PC d'un autre VLAN] → Doit répondre
```

Capturer les `ping` réussis inter-VLAN : **Capture P1**.

**Je comprends.** Avant de passer à Windows Server, quels tests prouvent que la phase réseau est terminée ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

Un **ping** entre VLAN différents, la **passerelle** de chaque VLAN, la **route par défaut**, le **relais DHCP** qui fonctionne. On teste chaque couche avant de la charger de services.

</details>


---

### 2.5 — Variante sur matériel réel

Si le matériel est disponible, la phase 2 peut être réalisée sur des équipements réels plutôt que dans Packet Tracer : c'est le choix de l'enseignant.

**Matériel :** 1 routeur, 2 commutateurs (un par bâtiment), des PC ou des VM reliées par pont aux ports d'accès, 1 câble console, câbles Ethernet.

1. Remettre chaque équipement à zéro (voir [Matériel réel ou simulation](../../ressources/materiel-reel.md)) et relever les noms réels des ports.
2. Reprendre les commandes des étapes 2.2 et 2.3 en adaptant les noms de ports. Sur certains commutateurs, `switchport trunk encapsulation dot1q` précède `switchport mode trunk`.
3. Les interfaces du routeur sont désactivées par défaut : `no shutdown` sur l'interface physique.
4. Isoler le montage du réseau de l'établissement : il contient un serveur DHCP.
5. Garder des preuves : photo du montage, schéma, `show running-config` de chaque équipement (sans mot de passe réel), tests inter-VLAN réussis. Une réalisation sur matériel réel est plus facile à défendre à l'épreuve E6 qu'une simulation (voir [Outils et simulateurs](../../ressources/outils.md)).

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

Capturer la structure AD dans la console ADUC : **Capture P2**.

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

Capturer les GPO créées et liées dans `gpmc.msc` : **Capture P3**.

---

## PHASE 4 — DHCP + DNS + Partages de Fichiers

*Durée estimée : 30 min*

### 4.1 — Configurer le Serveur DHCP

```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
# À lancer sur SRV-DHCP : le serveur déclare SA PROPRE identité (nom et adresse)
Add-DhcpServerInDC -DnsName "srv-dhcp.siosarl.local" -IPAddress 192.168.0.146

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

Capturer les partages créés dans `fsmgmt.msc` : **Capture P4**.

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

Capturer la session FTP anonyme réussie : **Capture P5**.

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

    echo "[OK] Utilisateur SFTP créé : sftp_${SERVICE}"
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

**Autorisé :** Capturer la session SFTP réussie (upload de fichier) : **Capture P6**.
**Autorisé :** Capturer le refus de connexion SSH : **Capture P7**.

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
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant).

**Je sais faire, sans aide :**

- [ ] Concevoir un plan d'adressage VLSM et une architecture VLAN par service ;
- [ ] Déployer le routage inter-VLAN, l'annuaire, les GPO, DHCP, DNS et les partages ;
- [ ] Sécuriser l'accès d'un serveur de fichiers (SFTP) ;
- [ ] Documenter en continu la configuration réalisée.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________
