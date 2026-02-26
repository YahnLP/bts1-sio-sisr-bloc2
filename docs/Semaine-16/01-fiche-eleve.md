---
author: YLP
title: 📚 FICHE DE COURS
---

# 📚 FICHE DE COURS ÉLÈVE
## "PowerShell • Active Directory • IPv6"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 16*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.4** | Exploiter un service en mode script (PowerShell) |
| **B2.3** | Assurer la sécurité des accès aux ressources |
| **B2.2** | Installer et configurer des éléments d'infrastructure (IPv6) |
| **B1.2** | Recenser et identifier les ressources et les besoins |

---

## PARTIE I — PowerShell : Philosophie et Fondamentaux

### I.A. PowerShell vs Bash — Ce qui Change Tout

PowerShell et Bash sont tous les deux des shells de ligne de commande permettant d'écrire des scripts. Mais ils reposent sur une **philosophie fondamentalement différente** :

| **Critère** | **Bash (Linux)** | **PowerShell (Windows)** |
|---|---|---|
| **Unité de base** | Texte (chaîne de caractères) | **Objet .NET** (avec propriétés et méthodes) |
| **Communication entre commandes** | Flux de texte via le pipe `\|` | **Objets structurés** via le pipeline `\|` |
| **Plateforme native** | Linux/Unix | Windows (+ multiplateforme depuis PS7) |
| **Accès aux services Microsoft** | Limité (outils tiers) | Natif (AD, Exchange, Azure, WMI...) |
| **Syntaxe des commandes** | Verbes courts (`ls`, `cp`, `rm`) | Verbe-Nom (`Get-ChildItem`, `Copy-Item`) |
| **Sensibilité à la casse** | Oui | Non (PowerShell ignore la casse) |

```
  BASH                              POWERSHELL
  ─────                             ──────────
  commande1 | commande2             Cmdlet1 | Cmdlet2
       │                                 │
       ▼                                 ▼
  Flux de TEXTE brut              Collection d'OBJETS .NET
  "root:x:0:0:root:/root:/bin/bash"  [Objet ProcessInfo]
                                      .Name = "chrome"
                                      .Id = 4872
                                      .CPU = 12.5
                                      .WorkingSet = 256MB
```

*Légende : Comparaison du pipeline Bash (à gauche) et du pipeline PowerShell (à droite). Bash transmet du texte brut entre les commandes — pour extraire une valeur, il faut parser le texte avec `cut`, `awk`, `grep`. PowerShell transmet des objets structurés avec des propriétés nommées directement accessibles — aucun parsing de texte nécessaire.*

💡 **Lien ITIL — Gestion des Actifs :** PowerShell permet d'inventorier, configurer et auditer l'infrastructure Windows de façon **programmable et reproductible**. C'est l'outil standard de la Gestion des Configurations dans les environnements Microsoft.

---

### I.B. La Nomenclature Verbe-Nom

Toutes les commandes PowerShell (appelées **cmdlets**) suivent le format `Verbe-Nom`, où :
- Le **Verbe** décrit l'action
- Le **Nom** désigne la cible

**Verbes les plus courants :**

| **Verbe** | **Action** | **Exemples** |
|---|---|---|
| `Get-` | Obtenir / Lire | `Get-Process`, `Get-Service`, `Get-ADUser` |
| `Set-` | Modifier une propriété | `Set-ADUser`, `Set-Service` |
| `New-` | Créer un nouvel objet | `New-ADUser`, `New-Item`, `New-ADGroup` |
| `Remove-` | Supprimer | `Remove-ADUser`, `Remove-Item` |
| `Start-` | Démarrer | `Start-Service`, `Start-Process` |
| `Stop-` | Arrêter | `Stop-Service`, `Stop-Process` |
| `Enable-` | Activer | `Enable-ADAccount` |
| `Disable-` | Désactiver | `Disable-ADAccount` |
| `Import-` | Importer des données | `Import-Csv`, `Import-Module` |
| `Export-` | Exporter des données | `Export-Csv`, `Export-Clixml` |
| `ConvertTo-` | Convertir le format | `ConvertTo-Html`, `ConvertTo-Json` |

📌 **Règle d'or :** Si vous cherchez une commande, pensez au verbe qui décrit ce que vous voulez faire + au nom de la ressource. `Get-Command -Verb Get -Noun *user*` vous trouvera toutes les cmdlets correspondantes.

---

### I.C. Cmdlets Essentielles de Navigation et d'Exploration

```powershell
# ─── Découvrir PowerShell ──────────────────────────────────────────────

# Lister toutes les cmdlets disponibles
Get-Command

# Chercher une cmdlet par verbe ou nom
Get-Command -Verb Get
Get-Command -Noun *user*
Get-Command -Name *AD*

# Obtenir l'aide complète d'une cmdlet
Get-Help Get-Process
Get-Help Get-Process -Examples       # Voir des exemples
Get-Help Get-Process -Detailed       # Aide détaillée
Get-Help Get-ADUser -Online          # Ouvrir la doc en ligne

# ─── Exploration des objets retournés ─────────────────────────────────

# Voir toutes les propriétés et méthodes d'un objet
Get-Process | Get-Member
Get-ADUser -Identity alice | Get-Member

# Lister les processus en cours
Get-Process

# Lister les services
Get-Service

# Obtenir des informations sur le système
Get-ComputerInfo

# Lister les fichiers et dossiers (équivalent de ls / dir)
Get-ChildItem C:\Windows
Get-ChildItem C:\Windows -Recurse -Filter "*.log"
```

---

### I.D. Le Pipeline et la Manipulation des Objets

Le **pipeline** (`|`) transmet l'objet retourné par la cmdlet de gauche comme entrée de la cmdlet de droite. Comme PowerShell transmet des objets (et non du texte), on peut accéder directement aux propriétés sans parsing.

```powershell
# ─── Pipeline simple ──────────────────────────────────────────────────

# Lister les processus → trier par consommation mémoire → prendre les 5 premiers
Get-Process | Sort-Object -Property WorkingSet -Descending | Select-Object -First 5

# ─── Where-Object : filtrer ───────────────────────────────────────────

# Ne garder que les services en cours d'exécution
Get-Service | Where-Object { $_.Status -eq "Running" }

# Ne garder que les processus consommant plus de 100 Mo
Get-Process | Where-Object { $_.WorkingSet -gt 100MB }

# ─── Select-Object : choisir les propriétés à afficher ────────────────

# Afficher uniquement le nom et le statut des services
Get-Service | Select-Object -Property Name, Status

# Afficher les 10 dernières lignes de l'historique PowerShell
Get-History | Select-Object -Last 10

# ─── ForEach-Object : traiter chaque objet ────────────────────────────

# Pour chaque service arrêté, afficher son nom
Get-Service | Where-Object { $_.Status -eq "Stopped" } | ForEach-Object {
    Write-Host "Service arrêté : $($_.Name)"
}

# ─── Chaînage complet ─────────────────────────────────────────────────

# Trouver les 3 processus les plus gourmands en CPU, afficher leur nom et CPU
Get-Process |
    Where-Object { $_.CPU -gt 0 } |
    Sort-Object -Property CPU -Descending |
    Select-Object -First 3 -Property Name, CPU, Id |
    Format-Table -AutoSize
```

---

### I.E. Variables, Conditions et Boucles en PowerShell

PowerShell supporte les mêmes structures de contrôle que Bash, avec une syntaxe différente.

**Tableau de correspondance Bash → PowerShell :**

| **Concept** | **Bash** | **PowerShell** |
|---|---|---|
| Déclarer une variable | `NOM="Alice"` | `$Nom = "Alice"` |
| Utiliser une variable | `echo $NOM` | `Write-Host $Nom` |
| Condition simple | `if [ $A -eq $B ]` | `if ($A -eq $B)` |
| Boucle sur une liste | `for x in liste` | `foreach ($x in $liste)` |
| Boucle numérique | `for (( i=0; i<10; i++ ))` | `for ($i=0; $i -lt 10; $i++)` |
| Boucle conditionnelle | `while [ condition ]` | `while (condition)` |
| Lire un fichier CSV | `while IFS=, read...` | `Import-Csv fichier.csv` |
| Afficher du texte | `echo "texte"` | `Write-Host "texte"` |
| Commentaire | `# commentaire` | `# commentaire` |

```powershell
# ─── Variables et types ───────────────────────────────────────────────
$Prenom     = "Alice"
$Age        = 25
$EstActif   = $true          # Booléen
$Tableau    = @("RH", "IT", "Direction")   # Tableau

# ─── Conditions ───────────────────────────────────────────────────────
if ($Age -ge 18) {
    Write-Host "$Prenom est majeur(e)."
} elseif ($Age -lt 0) {
    Write-Host "Âge invalide."
} else {
    Write-Host "$Prenom est mineur(e)."
}

# ─── Opérateurs de comparaison ────────────────────────────────────────
# -eq   Égal à
# -ne   Différent de
# -gt   Supérieur à
# -lt   Inférieur à
# -ge   Supérieur ou égal
# -le   Inférieur ou égal
# -like Correspond à un motif (wildcards * ?)
# -match Correspond à une regex

# ─── Boucle foreach ───────────────────────────────────────────────────
$Services = @("apache2", "mariadb", "sshd")

foreach ($Service in $Services) {
    Write-Host "Traitement de : $Service"
}

# ─── Boucle ForEach-Object dans un pipeline ───────────────────────────
1..5 | ForEach-Object {
    Write-Host "Itération numéro : $_"
}
```

---

### I.F. Gestion des Erreurs avec Try/Catch

```powershell
# ─── Structure Try/Catch/Finally ──────────────────────────────────────
try {
    # Code susceptible de générer une erreur
    $Utilisateur = Get-ADUser -Identity "utilisateur_inexistant"
    Write-Host "Utilisateur trouvé : $($Utilisateur.Name)"
}
catch {
    # Exécuté si une erreur se produit
    Write-Host "❌ Erreur : $($_.Exception.Message)"
}
finally {
    # Toujours exécuté (nettoyage, fermeture de connexions...)
    Write-Host "Opération terminée."
}

# ─── ErrorAction : contrôler le comportement en cas d'erreur ──────────
# -ErrorAction Stop      → Transformer l'erreur en exception catchable
# -ErrorAction Continue  → Continuer malgré l'erreur (défaut)
# -ErrorAction SilentlyContinue → Ignorer l'erreur silencieusement

Get-ADUser -Identity "inconnu" -ErrorAction Stop   # Lance une exception catchable
```

---

## PARTIE II — PowerShell et Active Directory

### II.A. Le Module ActiveDirectory

Pour administrer Active Directory avec PowerShell, il faut charger le module dédié :

```powershell
# Charger le module (automatique sur un DC, manuel sur un poste RSAT)
Import-Module ActiveDirectory

# Vérifier que le module est chargé
Get-Module -Name ActiveDirectory

# Lister toutes les cmdlets du module AD
Get-Command -Module ActiveDirectory
```

---

### II.B. Cmdlets AD Essentielles

```powershell
# ─── UTILISATEURS ─────────────────────────────────────────────────────

# Lister tous les utilisateurs du domaine
Get-ADUser -Filter *

# Chercher un utilisateur spécifique
Get-ADUser -Identity "alice.martin"

# Chercher avec filtre et afficher des propriétés étendues
Get-ADUser -Filter { Department -eq "RH" } -Properties Department, Title, EmailAddress

# Chercher et afficher des colonnes choisies
Get-ADUser -Filter * | Select-Object Name, SamAccountName, Enabled

# Créer un utilisateur
New-ADUser `
    -Name            "Alice Martin" `
    -GivenName       "Alice" `
    -Surname         "Martin" `
    -SamAccountName  "a.martin" `
    -UserPrincipalName "a.martin@siosarl.local" `
    -Path            "OU=RH,DC=siosarl,DC=local" `
    -AccountPassword (ConvertTo-SecureString "Azerty!2024" -AsPlainText -Force) `
    -ChangePasswordAtLogon $true `
    -Enabled         $true `
    -Description     "Arrivée fusion Nexio"

# Activer / Désactiver un utilisateur
Enable-ADAccount  -Identity "a.martin"
Disable-ADAccount -Identity "a.martin"

# Modifier un utilisateur
Set-ADUser -Identity "a.martin" -Title "Responsable RH" -Department "RH"

# Supprimer un utilisateur
Remove-ADUser -Identity "a.martin" -Confirm:$false

# ─── GROUPES ──────────────────────────────────────────────────────────

# Lister les membres d'un groupe
Get-ADGroupMember -Identity "GRP_RH"

# Ajouter un utilisateur à un groupe
Add-ADGroupMember -Identity "GRP_RH" -Members "a.martin"

# ─── UNITÉS D'ORGANISATION ────────────────────────────────────────────

# Créer une OU
New-ADOrganizationalUnit -Name "RH" -Path "DC=siosarl,DC=local"

# Lister les OUs
Get-ADOrganizationalUnit -Filter *
```

---

### II.C. Import-Csv : Traiter des Données Structurées

`Import-Csv` est la cmdlet qui transforme un fichier CSV en **collection d'objets PowerShell**. Chaque ligne devient un objet dont les propriétés correspondent aux en-têtes de colonnes.

```powershell
# ─── Exemple de fichier CSV (employes.csv) ────────────────────────────
# Prenom,Nom,Service,Login,Email
# Alice,Martin,RH,a.martin,a.martin@siosarl.local
# Bob,Dupont,Informatique,b.dupont,b.dupont@siosarl.local

# ─── Lecture du CSV ───────────────────────────────────────────────────
$Employes = Import-Csv -Path "C:\Scripts\employes.csv" -Delimiter ","
# → $Employes est maintenant un tableau d'objets

# Accéder à la première ligne
$Employes[0]
$Employes[0].Prenom    # → "Alice"
$Employes[0].Service   # → "RH"

# Itérer sur chaque ligne
foreach ($Employe in $Employes) {
    Write-Host "Traitement : $($Employe.Prenom) $($Employe.Nom) — Service : $($Employe.Service)"
}

# ─── Version pipeline ─────────────────────────────────────────────────
Import-Csv "C:\Scripts\employes.csv" | ForEach-Object {
    Write-Host "Login : $($_.Login) — Email : $($_.Email)"
}
```

```
   CSV (texte brut)                Import-Csv (objets PowerShell)
   ─────────────────              ─────────────────────────────────
   Prenom,Nom,Service             ┌─ Objet 1 ──────────────────────┐
   Alice,Martin,RH       ──────►  │  .Prenom  = "Alice"            │
   Bob,Dupont,IT                  │  .Nom     = "Martin"           │
                                  │  .Service = "RH"               │
                                  └────────────────────────────────┘
                                  ┌─ Objet 2 ──────────────────────┐
                                  │  .Prenom  = "Bob"              │
                                  │  .Nom     = "Dupont"           │
                                  │  .Service = "IT"               │
                                  └────────────────────────────────┘
```

*Légende : Transformation d'un fichier CSV (texte brut à gauche) en collection d'objets PowerShell (à droite) par la cmdlet Import-Csv. Chaque ligne du CSV devient un objet structuré dont les propriétés correspondent aux en-têtes de colonnes. On accède ensuite aux valeurs par leur nom (.Prenom, .Nom) sans avoir à parser le texte manuellement.*

---

## PARTIE III — IPv6 : Le Successeur d'IPv4

### III.A. Pourquoi IPv6 ?

IPv4 utilise des adresses de **32 bits** → 4,3 milliards d'adresses possibles. En 2011, le pool d'adresses IPv4 publiques de l'IANA était officiellement épuisé. La solution à long terme : **IPv6**, avec des adresses de **128 bits** → 340 sextillions d'adresses (3,4 × 10³⁸).

**Comparaison IPv4 / IPv6 :**

| **Critère** | **IPv4** | **IPv6** |
|---|---|---|
| Taille de l'adresse | 32 bits | 128 bits |
| Nombre d'adresses | ~4,3 milliards | ~3,4 × 10³⁸ |
| Notation | Décimale pointée (`192.168.1.1`) | Hexadécimale avec deux-points (`2001:db8::1`) |
| Configuration | Manuelle ou DHCP | Manuelle, DHCPv6 ou **SLAAC (auto)** |
| NAT obligatoire | Souvent | Non (assez d'adresses publiques) |
| Sécurité intégrée | Optionnelle (IPsec) | IPsec natif |
| En-tête | Variable, complexe | Simplifié, taille fixe |

---

### III.B. Format d'une Adresse IPv6

Une adresse IPv6 est composée de **128 bits** représentés en **8 groupes de 16 bits**, chaque groupe étant écrit en **hexadécimal** et séparé par des **deux-points** (`:`) :

```
   Adresse IPv6 complète :
   2001 : 0db8 : 0000 : 0042 : 0000 : 8a2e : 0370 : 7334
   ─────   ────   ────   ────   ────   ────   ────   ────
   G1      G2     G3     G4     G5     G6     G7     G8
   (16b)  (16b)  (16b)  (16b)  (16b)  (16b)  (16b)  (16b)
                         ←───── 128 bits au total ──────────►
```

*Légende : Structure d'une adresse IPv6. Les 128 bits sont divisés en 8 groupes de 16 bits (appelés hextets). Chaque groupe est représenté par 4 chiffres hexadécimaux (0-9, a-f), séparés par des deux-points. Contrairement à IPv4 (4 × 8 bits en décimal), IPv6 utilise la notation hexadécimale qui permet de représenter de grandes valeurs de façon compacte.*

---

### III.C. Règles de Compression des Adresses IPv6

Les adresses IPv6 complètes sont longues. Deux règles permettent de les abréger :

**Règle 1 — Supprimer les zéros de tête dans chaque groupe**

```
Avant : 2001:0db8:0000:0042:0000:8a2e:0370:7334
         ↓     ↓    ↓    ↓    ↓    ↓    ↓    ↓
Après : 2001:db8:0:42:0:8a2e:370:7334
```

**Règle 2 — Remplacer une (et une seule) suite de groupes `0000` consécutifs par `::`**

```
Avant : 2001:db8:0:42:0:8a2e:370:7334
              ↓   ↓  ↓ ↓
Identifier la plus longue suite de groupes nuls consécutifs → groupe 3 et 5 isolés

2001:db8:0:42:0:8a2e:370:7334   → pas de suite assez longue ici

Exemple avec suite longue :
FE80:0000:0000:0000:0200:F8FF:FE21:67CF
→ FE80::200:F8FF:FE21:67CF
      ↑↑
      :: remplace 0000:0000:0000
```

> ⚠️ `::` ne peut apparaître **qu'une seule fois** dans une adresse. Sinon, il serait impossible de savoir combien de groupes nuls il représente.

**Exercices de compression :**

| **Adresse complète** | **Adresse compressée** |
|---|---|
| `2001:0db8:0000:0000:0000:0000:0000:0001` | `2001:db8::1` |
| `FE80:0000:0000:0000:0204:61FF:FE9D:F156` | `FE80::204:61FF:FE9D:F156` |
| `0000:0000:0000:0000:0000:0000:0000:0001` | `::1` (adresse de loopback IPv6) |
| `0000:0000:0000:0000:0000:0000:0000:0000` | `::` (adresse non spécifiée) |

---

### III.D. Types d'Adresses IPv6 Principaux

| **Type** | **Préfixe** | **Description** | **Analogie IPv4** |
|---|---|---|---|
| **Loopback** | `::1/128` | Adresse de bouclage locale | `127.0.0.1` |
| **Non spécifiée** | `::/128` | Adresse "vide" (pendant config) | `0.0.0.0` |
| **Link-Local** | `FE80::/10` | Valide uniquement sur le lien local (ne traverse pas les routeurs) | `169.254.0.0/16` (APIPA) |
| **Unique Local** | `FC00::/7` | Adresses privées (réseau interne), routables en interne | `10.0.0.0/8`, `192.168.0.0/16` |
| **Global Unicast** | `2000::/3` | Adresses publiques routables sur Internet | Adresses IPv4 publiques |
| **Multicast** | `FF00::/8` | Envoi à un groupe d'équipements | `224.0.0.0/4` (multicast IPv4) |

```
   ─── PORTÉE DES ADRESSES IPv6 ─────────────────────────────────────
                                           ┌───────────────────────────────────────┐
                                           │           INTERNET                    │
                                           │   Global Unicast (2000::/3)           │
   ─────────────────────────────────────────────────────────────────────
                               ROUTEUR
   ─────────────────────────────────────────────────────────────────────
         ┌─────────────────────────────────────────────────────────┐
         │               RÉSEAU LOCAL                              │
         │   Unique Local (FC00::/7)                               │
         │                                                         │
         │  ┌────────────────────────────────────┐                │
         │  │           LIEN LOCAL               │                │
         │  │   Link-Local (FE80::/10)            │                │
         │  │   (ne traverse PAS le routeur)      │                │
         │  └────────────────────────────────────┘                │
         └─────────────────────────────────────────────────────────┘
```

*Légende : Représentation des portées des adresses IPv6. Les adresses Link-Local (FE80::/10) sont limitées au segment réseau physique et ne peuvent pas être routées. Les adresses Unique Local (FC00::/7) sont routables à l'intérieur d'un réseau privé. Les adresses Global Unicast (2000::/3) sont publiques et routables sur Internet.*

---

### III.E. L'Adresse Link-Local — Toujours Présente

**Toute interface réseau IPv6 se configure automatiquement une adresse link-local**, sans aucune configuration manuelle. Elle est calculée à partir de l'adresse MAC de l'interface.

**Format d'une adresse link-local :**

```
Préfixe fixe : FE80::/10
   └── FE80:0000:0000:0000: + Identifiant d'interface (64 bits)

Exemple avec MAC : 00:04:61:FE:9D:F1
Identifiant d'interface (EUI-64) :
   MAC découpée : 0004:61   +   FE:9D:F1
   FF:FE inséré au milieu → 0004:61FF:FE9D:F1
   Bit universel/local inversé → 0204:61FF:FE9D:F1

Adresse link-local résultante : FE80::204:61FF:FE9D:F1
```

> 📌 **Utilité pratique :** Les adresses link-local permettent la communication locale même en l'absence de routeur ou de serveur DHCPv6. Elles sont utilisées notamment pour la découverte de routeurs (Router Discovery) et le protocole NDP (Neighbor Discovery Protocol, l'équivalent IPv6 d'ARP).

---

### III.F. Auto-configuration SLAAC

**SLAAC** (Stateless Address Autoconfiguration, RFC 4862) est le mécanisme permettant à un hôte IPv6 de se configurer **automatiquement une adresse globale sans serveur DHCPv6**.

**Mécanisme en 4 étapes :**

```
   HÔTE (vient de démarrer)           ROUTEUR IPv6
         │                                  │
         │  1. Router Solicitation ─────────►│
         │     (Multicast : "Y a-t-il        │
         │      un routeur sur ce lien ?")   │
         │                                  │
         │  ◄──────────── 2. Router Advertisement
         │     (Préfixe : 2001:db8:1::/64    │
         │      Durée de vie, options...)    │
         │                                  │
         │  3. L'hôte construit son adresse  │
         │     Préfixe (64 bits du RA)       │
         │   + Identifiant d'interface (EUI-64 ou aléatoire)
         │   = 2001:db8:1::204:61FF:FE9D:F1 │
         │                                  │
         │  4. DAD (Duplicate Address Detection)
         │     "Cette adresse est-elle déjà utilisée ?"
         │     (Multicast vers l'adresse candidate)
         │     → Si pas de réponse : adresse attribuée ✅
```

*Légende : Les 4 étapes de SLAAC. L'hôte demande un préfixe réseau au routeur (Router Solicitation). Le routeur répond avec sa publicité (Router Advertisement) contenant le préfixe /64. L'hôte construit l'adresse complète en combinant le préfixe (64 bits) et son identifiant d'interface (64 bits). La détection de doublon (DAD) vérifie que l'adresse n'est pas déjà utilisée.*

---

## IV. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|---------------|
| **Cmdlet** | Commande PowerShell au format Verbe-Nom (ex : `Get-Process`) |
| **Pipeline PowerShell** | Mécanisme transmettant des objets (et non du texte) entre cmdlets via `\|` |
| **Objet .NET** | Structure de données avec propriétés et méthodes, unité de base manipulée par PowerShell |
| **`$_`** | Variable automatique représentant l'objet courant dans un pipeline ou une boucle |
| **`Where-Object`** | Cmdlet filtrant les objets d'un pipeline selon une condition |
| **`Select-Object`** | Cmdlet sélectionnant les propriétés à afficher ou les N premiers/derniers objets |
| **`ForEach-Object`** | Cmdlet exécutant un bloc de script pour chaque objet du pipeline |
| **`Import-Csv`** | Cmdlet transformant un fichier CSV en collection d'objets PowerShell |
| **`New-ADUser`** | Cmdlet créant un utilisateur dans Active Directory |
| **`-ErrorAction Stop`** | Paramètre transformant une erreur en exception catchable par `Try/Catch` |
| **`ConvertTo-SecureString`** | Cmdlet convertissant une chaîne de texte en objet sécurisé (pour les mots de passe) |
| **IPv6** | Protocole Internet version 6 — adresses 128 bits en notation hexadécimale |
| **Hextet** | Groupe de 16 bits dans une adresse IPv6 (8 hextets par adresse) |
| **`::` (double-colon)** | Abréviation d'une ou plusieurs suites de groupes nuls consécutifs (une seule occurrence) |
| **Link-Local** | Adresse IPv6 `FE80::/10` valide uniquement sur le lien local, non routable |
| **SLAAC** | Stateless Address Autoconfiguration — auto-configuration d'adresse IPv6 via les RA du routeur |
| **Router Advertisement (RA)** | Message ICMPv6 envoyé par le routeur contenant le préfixe réseau pour SLAAC |
| **DAD** | Duplicate Address Detection — vérification qu'une adresse IPv6 n'est pas déjà utilisée |
| **EUI-64** | Format de l'identifiant d'interface 64 bits dérivé de l'adresse MAC (utilisé dans SLAAC) |

---

## V. Questions de Réflexion

1. **En Bash, `ls /var/log | grep ".log"` fonctionne par filtrage de texte. Quel est l'équivalent PowerShell et en quoi est-il fondamentalement différent ?**
   - *Piste : `Get-ChildItem /var/log | Where-Object { $_.Extension -eq ".log" }` — quelle propriété est utilisée ? Est-ce du texte ?*

2. **Pourquoi utilise-t-on `ConvertTo-SecureString` lors de la création d'un utilisateur AD en PowerShell, plutôt que de passer directement le mot de passe en clair ?**
   - *Piste : Pensez à la journalisation des commandes PowerShell et à ce qui se passerait si le mot de passe apparaissait en clair dans un log.*

3. **Que se passe-t-il si l'on n'utilise pas `-ErrorAction Stop` dans un script de création d'utilisateurs AD en masse et qu'un utilisateur existe déjà ?**
   - *Piste : Le script continue-t-il ? Comment sauriez-vous qu'une erreur s'est produite ?*

4. **Pourquoi l'adresse link-local est-elle indispensable même dans un réseau entièrement configuré avec des adresses Global Unicast ?**
   - *Piste : Pensez au protocole NDP (équivalent IPv6 d'ARP) et à la découverte de routeurs.*

5. **Pourquoi SLAAC peut-il être préféré à DHCPv6 dans certains environnements, et dans quels cas DHCPv6 reste-t-il nécessaire ?**
   - *Piste : SLAAC distribue un préfixe mais ne distribue pas le serveur DNS — qui s'en charge alors ?*

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Je comprends la différence fondamentale entre le pipeline Bash (texte) et le pipeline PowerShell (objets)
- [ ] Je construis une cmdlet PowerShell à partir du format Verbe-Nom
- [ ] J'utilise `Where-Object`, `Select-Object` et `ForEach-Object` dans un pipeline
- [ ] Je lis les propriétés d'un objet avec `$_.NomDeLaPropriété` et `Get-Member`
- [ ] J'importe un CSV avec `Import-Csv` et j'accède aux colonnes par leur nom
- [ ] J'écris un script PowerShell créant des utilisateurs AD depuis un CSV
- [ ] J'utilise `Try/Catch` pour gérer les erreurs dans un script
- [ ] Je lis et écris une adresse IPv6 en notation complète et compressée
- [ ] J'identifie les types d'adresses (Link-Local, Unique Local, Global Unicast)
- [ ] J'explique les 4 étapes de SLAAC

---
