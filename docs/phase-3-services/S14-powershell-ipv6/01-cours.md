# Cours – PowerShell, Active Directory et IPv6

*BTS SIO SISR 1re année – Bloc 2 – S14 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S10 à S12 ; S13 (notion de script).
> - **Matériel :** VM Windows Server (domaine), VM cliente IPv6.

## À la fin de ce cours, tu sauras

- [ ] **Utiliser** les cmdlets fondamentales et le pipeline PowerShell ;
- [ ] **Manipuler** des objets (`Select-Object`, `Where-Object`) ;
- [ ] **Importer** un CSV et **créer** des utilisateurs AD en masse avec `New-ADUser` ;
- [ ] **Gérer** les erreurs avec `Try/Catch` ;
- [ ] **Décrire** une adresse IPv6, ses types (lien local, globale) et SLAAC.

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

<figure class="graphe diagramme"><svg viewBox="0 0 760 260" style="max-width:760px" role="img" aria-label="Pipeline Bash (texte) et pipeline PowerShell (objets)" xmlns="http://www.w3.org/2000/svg">
<rect x="20" y="14" width="340" height="34" rx="6" class="dg-dark"/>
<text x="190.0" y="36.0" text-anchor="middle" class="dg-w">Bash</text>
<rect x="20" y="66" width="340" height="36" rx="6" class="dg-box"/>
<text x="190.0" y="89.0" text-anchor="middle" class="dg-t">commande1 | commande2</text>
<line x1="190" y1="102" x2="190.0" y2="117.0" class="dg-line"/>
<polygon points="190,126 195.0,117.0 185.0,117.0" class="dg-ah"/>
<rect x="20" y="126" width="340" height="60" rx="6" class="dg-soft"/>
<text x="190.0" y="152.0" text-anchor="middle" class="dg-t">Flux de TEXTE brut</text>
<text x="190.0" y="170.0" text-anchor="middle" class="dg-s">root:x:0:0:root:/root:/bin/bash</text>
<rect x="400" y="14" width="340" height="34" rx="6" class="dg-dark"/>
<text x="570.0" y="36.0" text-anchor="middle" class="dg-w">PowerShell</text>
<rect x="400" y="66" width="340" height="36" rx="6" class="dg-warn"/>
<text x="570.0" y="89.0" text-anchor="middle" class="dg-t">Cmdlet1 | Cmdlet2</text>
<line x1="570" y1="102" x2="570.0" y2="117.0" class="dg-line"/>
<polygon points="570,126 575.0,117.0 565.0,117.0" class="dg-ah"/>
<rect x="400" y="126" width="340" height="120" rx="6" class="dg-ok"/>
<text x="570.0" y="146.0" text-anchor="middle" class="dg-t">Collection d&#x27;OBJETS .NET</text>
<text x="570.0" y="164.0" text-anchor="middle" class="dg-s">Objet ProcessInfo</text>
<text x="570.0" y="182.0" text-anchor="middle" class="dg-s">.Name = &quot;chrome&quot;</text>
<text x="570.0" y="200.0" text-anchor="middle" class="dg-s">.Id = 4872</text>
<text x="570.0" y="218.0" text-anchor="middle" class="dg-s">.CPU = 12.5</text>
<text x="570.0" y="236.0" text-anchor="middle" class="dg-s">.WorkingSet = 256 Mo</text>
</svg>
<figcaption>Bash transmet du texte brut, PowerShell transmet des objets .NET dont on lit les propriétés.</figcaption>
</figure>

*Légende : Comparaison du pipeline Bash (à gauche) et du pipeline PowerShell (à droite). Bash transmet du texte brut entre les commandes — pour extraire une valeur, il faut parser le texte avec `cut`, `awk`, `grep`. PowerShell transmet des objets structurés avec des propriétés nommées directement accessibles — aucun parsing de texte nécessaire.*

 **Lien ITIL — Gestion des Actifs :** PowerShell permet d'inventorier, configurer et auditer l'infrastructure Windows de façon **programmable et reproductible**. C'est l'outil standard de la Gestion des Configurations dans les environnements Microsoft.

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

 **Règle d'or :** Si vous cherchez une commande, pensez au verbe qui décrit ce que vous voulez faire + au nom de la ressource. `Get-Command -Verb Get -Noun *user*` vous trouvera toutes les cmdlets correspondantes.

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
    Write-Host "[ERREUR] Erreur : $($_.Exception.Message)"
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

<figure class="graphe diagramme"><svg viewBox="0 0 760 260" style="max-width:760px" role="img" aria-label="Import-Csv transforme chaque ligne d&#x27;un CSV en objet" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="24" text-anchor="start" class="dg-t">Fichier CSV (texte brut)</text>
<rect x="20" y="36" width="230" height="100" rx="6" class="dg-soft"/>
<text x="135.0" y="73.0" text-anchor="middle" class="dg-s">Prenom,Nom,Service</text>
<text x="135.0" y="91.0" text-anchor="middle" class="dg-s">Alice,Martin,RH</text>
<text x="135.0" y="109.0" text-anchor="middle" class="dg-s">Bob,Dupont,IT</text>
<line x1="256" y1="86" x2="351.0" y2="86.0" class="dg-line"/>
<polygon points="360,86 351.0,81.0 351.0,91.0" class="dg-ah"/>
<text x="308" y="74" text-anchor="middle" class="dg-t">Import-Csv</text>
<text x="372" y="24" text-anchor="start" class="dg-t">Objets PowerShell</text>
<rect x="372" y="36" width="380" height="96" rx="6" class="dg-ok"/>
<text x="562.0" y="62.0" text-anchor="middle" class="dg-t">Objet 1</text>
<text x="562.0" y="80.0" text-anchor="middle" class="dg-s">.Prenom = &quot;Alice&quot;</text>
<text x="562.0" y="98.0" text-anchor="middle" class="dg-s">.Nom = &quot;Martin&quot;</text>
<text x="562.0" y="116.0" text-anchor="middle" class="dg-s">.Service = &quot;RH&quot;</text>
<rect x="372" y="146" width="380" height="96" rx="6" class="dg-ok"/>
<text x="562.0" y="172.0" text-anchor="middle" class="dg-t">Objet 2</text>
<text x="562.0" y="190.0" text-anchor="middle" class="dg-s">.Prenom = &quot;Bob&quot;</text>
<text x="562.0" y="208.0" text-anchor="middle" class="dg-s">.Nom = &quot;Dupont&quot;</text>
<text x="562.0" y="226.0" text-anchor="middle" class="dg-s">.Service = &quot;IT&quot;</text>
</svg>
<figcaption>Chaque ligne du fichier devient un objet dont les colonnes sont des propriétés.</figcaption>
</figure>

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

<figure class="graphe diagramme"><svg viewBox="0 0 760 150" style="max-width:760px" role="img" aria-label="Adresse IPv6 : 8 groupes de 16 bits" xmlns="http://www.w3.org/2000/svg">
<rect x="16" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="54.0" y="55.0" text-anchor="middle" class="dg-t">2001</text>
<rect x="102" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="140.0" y="55.0" text-anchor="middle" class="dg-t">0db8</text>
<rect x="188" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="226.0" y="55.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="274" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="312.0" y="55.0" text-anchor="middle" class="dg-t">0042</text>
<rect x="360" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="398.0" y="55.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="446" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="484.0" y="55.0" text-anchor="middle" class="dg-t">8a2e</text>
<rect x="532" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="570.0" y="55.0" text-anchor="middle" class="dg-t">0370</text>
<rect x="618" y="34" width="76" height="32" rx="5" class="dg-box"/>
<text x="656.0" y="55.0" text-anchor="middle" class="dg-t">7334</text>
<text x="54" y="24" text-anchor="middle" class="dg-t">G1</text>
<text x="54" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="97" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="140" y="24" text-anchor="middle" class="dg-t">G2</text>
<text x="140" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="183" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="226" y="24" text-anchor="middle" class="dg-t">G3</text>
<text x="226" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="269" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="312" y="24" text-anchor="middle" class="dg-t">G4</text>
<text x="312" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="355" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="398" y="24" text-anchor="middle" class="dg-t">G5</text>
<text x="398" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="441" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="484" y="24" text-anchor="middle" class="dg-t">G6</text>
<text x="484" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="527" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="570" y="24" text-anchor="middle" class="dg-t">G7</text>
<text x="570" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<text x="613" y="56" text-anchor="middle" class="dg-t">:</text>
<text x="656" y="24" text-anchor="middle" class="dg-t">G8</text>
<text x="656" y="86" text-anchor="middle" class="dg-s">16 bits</text>
<line x1="16" y1="104" x2="694" y2="104" class="dg-line"/>
<line x1="16" y1="98" x2="16" y2="110" class="dg-line"/>
<line x1="694" y1="98" x2="694" y2="110" class="dg-line"/>
<text x="380" y="128" text-anchor="middle" class="dg-t">128 bits au total</text>
</svg>
<figcaption>Huit groupes de quatre chiffres hexadécimaux séparés par des deux-points.</figcaption>
</figure>

*Légende : Structure d'une adresse IPv6. Les 128 bits sont divisés en 8 groupes de 16 bits (appelés hextets). Chaque groupe est représenté par 4 chiffres hexadécimaux (0-9, a-f), séparés par des deux-points. Contrairement à IPv4 (4 × 8 bits en décimal), IPv6 utilise la notation hexadécimale qui permet de représenter de grandes valeurs de façon compacte.*

---

### III.C. Règles de Compression des Adresses IPv6

Les adresses IPv6 complètes sont longues. Deux règles permettent de les abréger :

**Règle 1 — Supprimer les zéros de tête dans chaque groupe**

<figure class="graphe diagramme"><svg viewBox="0 0 760 190" style="max-width:760px" role="img" aria-label="Règle 1 : supprimer les zéros de tête de chaque groupe" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="28" text-anchor="start" class="dg-t">Avant</text>
<rect x="20" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="58.0" y="59.0" text-anchor="middle" class="dg-t">2001</text>
<rect x="106" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="144.0" y="59.0" text-anchor="middle" class="dg-t">0db8</text>
<rect x="192" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="230.0" y="59.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="278" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="316.0" y="59.0" text-anchor="middle" class="dg-t">0042</text>
<rect x="364" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="402.0" y="59.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="450" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="488.0" y="59.0" text-anchor="middle" class="dg-t">8a2e</text>
<rect x="536" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="574.0" y="59.0" text-anchor="middle" class="dg-t">0370</text>
<rect x="622" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="660.0" y="59.0" text-anchor="middle" class="dg-t">7334</text>
<line x1="144" y1="72" x2="144.0" y2="99.0" class="dg-line"/>
<polygon points="144,108 149.0,99.0 139.0,99.0" class="dg-ah"/>
<line x1="230" y1="72" x2="230.0" y2="99.0" class="dg-line"/>
<polygon points="230,108 235.0,99.0 225.0,99.0" class="dg-ah"/>
<line x1="316" y1="72" x2="316.0" y2="99.0" class="dg-line"/>
<polygon points="316,108 321.0,99.0 311.0,99.0" class="dg-ah"/>
<line x1="402" y1="72" x2="402.0" y2="99.0" class="dg-line"/>
<polygon points="402,108 407.0,99.0 397.0,99.0" class="dg-ah"/>
<line x1="574" y1="72" x2="574.0" y2="99.0" class="dg-line"/>
<polygon points="574,108 579.0,99.0 569.0,99.0" class="dg-ah"/>
<text x="20" y="98" text-anchor="middle" class="dg-s"></text>
<text x="20" y="128" text-anchor="start" class="dg-t">Après</text>
<rect x="20" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="58.0" y="159.0" text-anchor="middle" class="dg-t">2001</text>
<rect x="106" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="144.0" y="159.0" text-anchor="middle" class="dg-t">db8</text>
<rect x="192" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="230.0" y="159.0" text-anchor="middle" class="dg-t">0</text>
<rect x="278" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="316.0" y="159.0" text-anchor="middle" class="dg-t">42</text>
<rect x="364" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="402.0" y="159.0" text-anchor="middle" class="dg-t">0</text>
<rect x="450" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="488.0" y="159.0" text-anchor="middle" class="dg-t">8a2e</text>
<rect x="536" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="574.0" y="159.0" text-anchor="middle" class="dg-t">370</text>
<rect x="622" y="138" width="76" height="32" rx="5" class="dg-ok"/>
<text x="660.0" y="159.0" text-anchor="middle" class="dg-t">7334</text>
</svg>
<figcaption>Dans chaque groupe, on retire les zéros situés au début.</figcaption>
</figure>

**Règle 2 — Remplacer une (et une seule) suite de groupes `0000` consécutifs par `::`**

<figure class="graphe diagramme"><svg viewBox="0 0 760 190" style="max-width:760px" role="img" aria-label="Règle 2 : remplacer une suite de groupes nuls par ::" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="28" text-anchor="start" class="dg-t">Avant</text>
<rect x="20" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="58.0" y="59.0" text-anchor="middle" class="dg-t">FE80</text>
<rect x="106" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="144.0" y="59.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="192" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="230.0" y="59.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="278" y="38" width="76" height="32" rx="5" class="dg-warn"/>
<text x="316.0" y="59.0" text-anchor="middle" class="dg-t">0000</text>
<rect x="364" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="402.0" y="59.0" text-anchor="middle" class="dg-t">0200</text>
<rect x="450" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="488.0" y="59.0" text-anchor="middle" class="dg-t">F8FF</text>
<rect x="536" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="574.0" y="59.0" text-anchor="middle" class="dg-t">FE21</text>
<rect x="622" y="38" width="76" height="32" rx="5" class="dg-box"/>
<text x="660.0" y="59.0" text-anchor="middle" class="dg-t">67CF</text>
<text x="20" y="98" text-anchor="start" class="dg-i">Les trois groupes nuls consécutifs sont remplacés par ::</text>
<text x="20" y="128" text-anchor="start" class="dg-t">Après</text>
<rect x="20" y="138" width="430" height="32" rx="5" class="dg-ok"/>
<text x="235.0" y="159.0" text-anchor="middle" class="dg-t">FE80::200:F8FF:FE21:67CF</text>
</svg>
<figcaption>Exemple : FE80:0000:0000:0000:0200:F8FF:FE21:67CF devient FE80::200:F8FF:FE21:67CF.</figcaption>
</figure>

> **Attention :** `::` ne peut apparaître **qu'une seule fois** dans une adresse. Sinon, il serait impossible de savoir combien de groupes nuls il représente.

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

<figure class="graphe diagramme"><svg viewBox="0 0 760 330" style="max-width:760px" role="img" aria-label="Portée des adresses IPv6 : lien local, réseau local, Internet" xmlns="http://www.w3.org/2000/svg">
<rect x="130" y="10" width="500" height="70" rx="12" class="dg-ok"/>
<text x="380" y="38" text-anchor="middle" class="dg-t">INTERNET</text>
<text x="380" y="60" text-anchor="middle" class="dg-s">Global Unicast (2000::/3) : adresses publiques routables</text>
<rect x="310" y="96" width="140" height="36" rx="8" class="dg-dark"/>
<text x="380.0" y="119.0" text-anchor="middle" class="dg-w">ROUTEUR</text>
<line x1="380" y1="80" x2="380" y2="96" class="dg-line"/>
<rect x="130" y="146" width="500" height="176" rx="12" class="dg-warn"/>
<text x="380" y="172" text-anchor="middle" class="dg-t">RÉSEAU LOCAL</text>
<text x="380" y="192" text-anchor="middle" class="dg-s">Unique Local (FC00::/7) : adresses privées routables en interne</text>
<line x1="380" y1="132" x2="380" y2="146" class="dg-line"/>
<rect x="190" y="212" width="380" height="94" rx="12" class="dg-soft"/>
<text x="380" y="240" text-anchor="middle" class="dg-t">LIEN LOCAL</text>
<text x="380" y="260" text-anchor="middle" class="dg-s">Link-Local (FE80::/10)</text>
<text x="380" y="282" text-anchor="middle" class="dg-i">ne traverse pas le routeur</text>
</svg>
<figcaption>Link-Local ne franchit pas le routeur ; Unique Local reste dans le réseau privé ; Global Unicast est routable sur Internet.</figcaption>
</figure>

*Légende : Représentation des portées des adresses IPv6. Les adresses Link-Local (FE80::/10) sont limitées au segment réseau physique et ne peuvent pas être routées. Les adresses Unique Local (FC00::/7) sont routables à l'intérieur d'un réseau privé. Les adresses Global Unicast (2000::/3) sont publiques et routables sur Internet.*

---

### III.E. L'Adresse Link-Local — Toujours Présente

**Toute interface réseau IPv6 se configure automatiquement une adresse link-local**, sans aucune configuration manuelle. Elle est calculée à partir de l'adresse MAC de l'interface.

**Format d'une adresse link-local :**

<figure class="graphe diagramme"><svg viewBox="0 0 760 330" style="max-width:760px" role="img" aria-label="Construction d&#x27;une adresse link-local à partir de l&#x27;adresse MAC (EUI-64)" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="26" text-anchor="start" class="dg-t">1. Adresse MAC (48 bits) coupée en deux moitiés</text>
<rect x="20" y="36" width="160" height="32" rx="5" class="dg-box"/>
<text x="100.0" y="57.0" text-anchor="middle" class="dg-t">00 04 61</text>
<rect x="190" y="36" width="160" height="32" rx="5" class="dg-box"/>
<text x="270.0" y="57.0" text-anchor="middle" class="dg-t">9D F1 56</text>
<text x="20" y="100" text-anchor="start" class="dg-t">2. Insertion de FF:FE au milieu</text>
<rect x="20" y="110" width="160" height="32" rx="5" class="dg-box"/>
<text x="100.0" y="131.0" text-anchor="middle" class="dg-t">00 04 61</text>
<rect x="190" y="110" width="100" height="32" rx="5" class="dg-warn"/>
<text x="240.0" y="131.0" text-anchor="middle" class="dg-t">FF FE</text>
<rect x="300" y="110" width="160" height="32" rx="5" class="dg-box"/>
<text x="380.0" y="131.0" text-anchor="middle" class="dg-t">9D F1 56</text>
<text x="20" y="174" text-anchor="start" class="dg-t">3. Inversion du 7e bit (universel/local) : 00 devient 02</text>
<rect x="20" y="184" width="160" height="32" rx="5" class="dg-warn"/>
<text x="100.0" y="205.0" text-anchor="middle" class="dg-t">02 04 61</text>
<rect x="190" y="184" width="100" height="32" rx="5" class="dg-box"/>
<text x="240.0" y="205.0" text-anchor="middle" class="dg-t">FF FE</text>
<rect x="300" y="184" width="160" height="32" rx="5" class="dg-box"/>
<text x="380.0" y="205.0" text-anchor="middle" class="dg-t">9D F1 56</text>
<text x="20" y="248" text-anchor="start" class="dg-t">4. Préfixe FE80::/10 + identifiant d&#x27;interface (64 bits)</text>
<rect x="20" y="258" width="150" height="32" rx="5" class="dg-dark"/>
<text x="95.0" y="279.0" text-anchor="middle" class="dg-w">FE80::</text>
<rect x="180" y="258" width="290" height="32" rx="5" class="dg-ok"/>
<text x="325.0" y="279.0" text-anchor="middle" class="dg-t">0204:61FF:FE9D:F156</text>
<text x="20" y="316" text-anchor="start" class="dg-t">Résultat compressé : FE80::204:61FF:FE9D:F156</text>
</svg>
<figcaption>MAC 00:04:61:9D:F1:56 donne l&#x27;adresse link-local FE80::204:61FF:FE9D:F156.</figcaption>
</figure>

>  **Utilité pratique :** Les adresses link-local permettent la communication locale même en l'absence de routeur ou de serveur DHCPv6. Elles sont utilisées notamment pour la découverte de routeurs (Router Discovery) et le protocole NDP (Neighbor Discovery Protocol, l'équivalent IPv6 d'ARP).

---

### III.F. Auto-configuration SLAAC

**SLAAC** (Stateless Address Autoconfiguration, RFC 4862) est le mécanisme permettant à un hôte IPv6 de se configurer **automatiquement une adresse globale sans serveur DHCPv6**.

**Mécanisme en 4 étapes :**

<figure class="graphe diagramme"><svg viewBox="0 0 720 322" style="max-width:720px" role="img" aria-label="Autoconfiguration IPv6 SLAAC : sollicitation, annonce, construction de l&#x27;adresse, détection de doublon" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="155.0" y="30.0" text-anchor="middle" class="dg-w">Hôte (vient de démarrer)</text>
<line x1="155.0" y1="42" x2="155.0" y2="314" class="dg-dash"/>
<rect x="480.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="565.0" y="30.0" text-anchor="middle" class="dg-w">Routeur IPv6</text>
<line x1="565.0" y1="42" x2="565.0" y2="314" class="dg-dash"/>
<text x="360.0" y="64" text-anchor="middle" class="dg-t">1. Router Solicitation (multicast)</text>
<line x1="155.0" y1="72" x2="556.0" y2="72.0" class="dg-line"/>
<polygon points="565.0,72 556.0,67.0 556.0,77.0" class="dg-ah"/>
<text x="360.0" y="88" text-anchor="middle" class="dg-i">« Y a-t-il un routeur sur ce lien ? »</text>
<text x="360.0" y="114" text-anchor="middle" class="dg-t">2. Router Advertisement</text>
<line x1="565.0" y1="122" x2="164.0" y2="122.0" class="dg-dash"/>
<polygon points="155.0,122 164.0,127.0 164.0,117.0" class="dg-ah"/>
<text x="360.0" y="138" text-anchor="middle" class="dg-i">Préfixe 2001:db8:1::/64, durée de vie, options</text>
<rect x="4.0" y="156" width="351.4" height="74" rx="6" class="dg-soft"/>
<text x="179.7" y="173" text-anchor="middle" class="dg-s">3. Construction de l&#x27;adresse :</text>
<text x="179.7" y="189" text-anchor="middle" class="dg-s">préfixe (64 bits du RA) + identifiant d&#x27;interface</text>
<text x="179.7" y="205" text-anchor="middle" class="dg-s">(EUI-64 ou aléatoire)</text>
<text x="179.7" y="221" text-anchor="middle" class="dg-s">= 2001:db8:1::204:61FF:FE9D:F1</text>
<text x="360.0" y="250" text-anchor="middle" class="dg-t">4. DAD (multicast vers l&#x27;adresse candidate)</text>
<line x1="155.0" y1="258" x2="556.0" y2="258.0" class="dg-line"/>
<polygon points="565.0,258 556.0,253.0 556.0,263.0" class="dg-ah"/>
<text x="360.0" y="274" text-anchor="middle" class="dg-i">« Cette adresse est-elle déjà utilisée ? »</text>
<text x="360.0" y="290" text-anchor="middle" class="dg-i">Pas de réponse : l&#x27;adresse est attribuée</text>
</svg>
</figure>

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

Réfléchissez à chaque question avant d'ouvrir la réponse.

**1. En Bash, `ls /var/log | grep ".log"` fonctionne par filtrage de texte. Quel est l'équivalent PowerShell et en quoi est-il fondamentalement différent ?**

*Piste : `Get-ChildItem /var/log | Where-Object { $_.Extension -eq ".log" }` — quelle propriété est utilisée ? Est-ce du texte ?*

<details markdown="1">
<summary>Réponse</summary>

`Get-ChildItem` renvoie des **objets** (fichiers .NET) et non du texte. `Where-Object { $_.Extension -eq ".log" }` teste la **propriété** `Extension` de chaque objet. Avec Bash, `grep` cherche une chaîne dans du texte, ce qui est fragile (un fichier `app.log.bak` ou un nom contenant `.log` en milieu de chaîne correspond aussi). En PowerShell, le pipeline transmet des objets avec leurs propriétés et méthodes : pas d'analyse de texte par colonnes.

</details>

**2. Pourquoi utilise-t-on `ConvertTo-SecureString` lors de la création d'un utilisateur AD en PowerShell, plutôt que de passer directement le mot de passe en clair ?**

*Piste : Pensez à la journalisation des commandes PowerShell et à ce qui se passerait si le mot de passe apparaissait en clair dans un log.*

<details markdown="1">
<summary>Réponse</summary>

Un `SecureString` est conservé **chiffré en mémoire** et n'apparaît pas en clair à l'écran, dans l'historique ni dans les journaux PowerShell (transcription, journalisation des blocs de script). Un mot de passe en clair dans une commande peut être lu par quiconque consulte les journaux. Remarque : `-AsPlainText -Force` écrit encore le mot de passe en clair dans le script ; en production, on le saisit avec `Read-Host -AsSecureString` ou on le lit depuis un coffre.

</details>

**3. Que se passe-t-il si l'on n'utilise pas `-ErrorAction Stop` dans un script de création d'utilisateurs AD en masse et qu'un utilisateur existe déjà ?**

*Piste : Le script continue-t-il ? Comment sauriez-vous qu'une erreur s'est produite ?*

<details markdown="1">
<summary>Réponse</summary>

L'erreur (utilisateur déjà existant) est dite **non bloquante** : PowerShell affiche un message rouge et **le script continue**. Un `Try/Catch` n'intercepte pas cette erreur, et dans une longue boucle on peut ne pas voir qu'un compte n'a pas été créé. Avec `-ErrorAction Stop`, l'erreur devient bloquante, le `Catch` la traite : on la consigne dans un journal et on décide de continuer ou d'arrêter.

</details>

**4. Pourquoi l'adresse link-local est-elle indispensable même dans un réseau entièrement configuré avec des adresses Global Unicast ?**

*Piste : Pensez au protocole NDP (équivalent IPv6 d'ARP) et à la découverte de routeurs.*

<details markdown="1">
<summary>Réponse</summary>

L'adresse `FE80::/10` est créée **automatiquement** sur toute interface IPv6 et fonctionne sans routeur ni DHCP. Le protocole **NDP** (qui remplace ARP et gère la découverte des routeurs) s'en sert : les annonces de routeur sont émises depuis l'adresse link-local du routeur, et les routes pointent vers elle comme prochain saut. Sans link-local, l'interface ne pourrait pas obtenir sa configuration.

</details>

**5. Pourquoi SLAAC peut-il être préféré à DHCPv6 dans certains environnements, et dans quels cas DHCPv6 reste-t-il nécessaire ?**

*Piste : SLAAC distribue un préfixe mais ne distribue pas le serveur DNS — qui s'en charge alors ?*

<details markdown="1">
<summary>Réponse</summary>

SLAAC est **sans serveur à maintenir** : le routeur annonce le préfixe et chaque machine construit son adresse. C'est simple et léger. **DHCPv6** reste nécessaire pour **contrôler et tracer** les adresses attribuées (inventaire, réservations, journaux) ou pour distribuer des options. Le serveur DNS est transmis soit par l'option **RDNSS** des annonces de routeur, soit par un DHCPv6 *sans état* (qui ne distribue que les options, pas les adresses).

</details>

---

## Auto-évaluation : Suis-je Prêt ?

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
