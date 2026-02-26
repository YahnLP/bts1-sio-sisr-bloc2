# Semaine 12 (S12) - BLOC 2
## "Stratégies de Groupe (GPO) et DNS Windows AD-Intégré"

---

## 🖥️ TP PARTIE 1 : STRATÉGIES DE GROUPE (GPO)

**Prérequis :** VM S11 opérationnelle — domaine `techpro.local`, OU et utilisateurs créés.

**Si VM cliente disponible :** Connecter la VM W10/11 au domaine `techpro.local` avant de commencer le TP (Paramètres → Système → Domaine → `techpro.local` → Administrateur / Admin@BTS2024 → Redémarrer).

---

### GPO 1 : Politique de Sécurité des Mots de Passe

**Objectif :** Appliquer une politique de mot de passe renforcée aux utilisateurs de `OU_UTILISATEURS`.

> **Rappel :** La politique de mot de passe pour les connexions au domaine ne peut techniquement être définie QUE dans la `Default Domain Policy`. En TP, on configure la politique de compte **locale** via GPO sur les machines, ce qui affecte les comptes locaux des PC. Pour les comptes du domaine, c'est la Default Domain Policy qui fait foi.

#### Étape 1 : Ouvrir la GPMC

```
Win+R → gpmc.msc → Entrée
```

#### Étape 2 : Créer la GPO

1. Dans l'arbre GPMC, déployer `techpro.local`
2. Clic droit sur `Objets de stratégie de groupe` → `Nouveau`
3. Nom : `GPO_Securite_Comptes` → `OK`

#### Étape 3 : Modifier la GPO

1. Clic droit sur `GPO_Securite_Comptes` → `Modifier`
2. Naviguer vers :
   ```
   Configuration Ordinateur
   └── Paramètres Windows
       └── Paramètres de sécurité
           └── Stratégies de compte
               └── Stratégie de mot de passe
   ```
3. Configurer les paramètres suivants :

| **Paramètre** | **Valeur** |
|---------------|------------|
| Appliquer l'historique des mots de passe | **5** mots de passe mémorisés |
| Durée de vie maximale du mot de passe | **90** jours |
| Durée de vie minimale du mot de passe | **1** jour |
| Longueur minimale du mot de passe | **10** caractères |
| Le mot de passe doit respecter des exigences de complexité | **Activé** |

4. Naviguer vers :
   ```
   Stratégies de compte
   └── Stratégie de verrouillage du compte
   ```
5. Configurer :

| **Paramètre** | **Valeur** |
|---------------|------------|
| Seuil de verrouillage du compte | **5** tentatives d'ouverture de session non valides |
| Durée de verrouillage du compte | **15** minutes |
| Réinitialiser le compteur de verrouillage | **15** minutes |

6. Fermer l'éditeur.

#### Étape 4 : Lier la GPO à OU_UTILISATEURS

1. Dans GPMC, clic droit sur `OU_UTILISATEURS` → `Lier un objet de stratégie de groupe existant`
2. Sélectionner `GPO_Securite_Comptes` → `OK`
3. La GPO apparaît sous `OU_UTILISATEURS` avec un lien.

#### Étape 5 : Forcer et Vérifier

```cmd
:: Sur le serveur (ou la VM cliente si disponible)
gpupdate /force

:: Vérifier l'application
gpresult /r
```

Chercher dans la sortie :
```
Les GPO suivantes ont été appliquées :
  GPO_Securite_Comptes    Lien : OU=OU_UTILISATEURS,DC=techpro,DC=local
```

✅ **Checkpoint GPO 1 :** GPO visible dans GPMC liée à OU_UTILISATEURS. `gpresult /r` la liste. Capture à fournir.

---

### GPO 2 : Fond d'Écran Imposé

**Objectif :** Imposer un fond d'écran d'entreprise à tous les utilisateurs de `OU Commercial`.

#### Étape 1 : Préparer l'image

L'image doit être stockée dans un chemin **accessible par tous les utilisateurs** (partage réseau ou chemin local SYSVOL).

En TP, utiliser le chemin SYSVOL (disponible automatiquement sur tous les DC) :

```
\\SRV-AD-TECHPRO\SYSVOL\techpro.local\scripts\
```

Copier une image `.jpg` dans ce chemin (depuis le serveur) :

```powershell
# Créer le dossier s'il n'existe pas
$sysvol = "C:\Windows\SYSVOL\sysvol\techpro.local\scripts"
New-Item -Path "$sysvol\Fond" -ItemType Directory -Force

# Copier une image (par exemple depuis le bureau)
Copy-Item "C:\Users\Administrateur\Desktop\fond_techpro.jpg" -Destination "$sysvol\Fond\"
```

Le chemin réseau UNC sera : `\\SRV-AD-TECHPRO\SYSVOL\techpro.local\scripts\Fond\fond_techpro.jpg`

#### Étape 2 : Créer la GPO

1. GPMC → `Objets de stratégie de groupe` → Clic droit → `Nouveau`
2. Nom : `GPO_Fond_Ecran_Commercial` → `OK`
3. Clic droit → `Modifier`

#### Étape 3 : Configurer le Fond d'Écran

Naviguer vers :
```
Configuration Utilisateur
└── Modèles d'administration
    └── Bureau
        └── Bureau
            └── Papier peint du Bureau
```

Double-clic sur `Papier peint du Bureau` :
- État : **Activé**
- Nom du papier peint : `\\SRV-AD-TECHPRO\SYSVOL\techpro.local\scripts\Fond\fond_techpro.jpg`
- Style du papier peint : **Ajuster** (ou Étirer)
- `OK`

#### Étape 4 : Empêcher le Changement (optionnel)

Pour que l'utilisateur ne puisse pas modifier le fond d'écran :

```
Configuration Utilisateur
└── Modèles d'administration
    └── Panneau de configuration
        └── Personnalisation
            └── Empêcher la modification du fond d'écran du Bureau → Activé
```

#### Étape 5 : Lier à OU Commercial

1. GPMC → Clic droit sur `Commercial` (dans `OU_UTILISATEURS`) → `Lier un objet de stratégie de groupe existant`
2. Sélectionner `GPO_Fond_Ecran_Commercial` → `OK`

#### Étape 6 : Tester

```cmd
gpupdate /force /logoff
:: Se reconnecter avec un compte de OU Commercial (s.leroy par exemple)
:: Le fond d'écran doit être imposé
```

✅ **Checkpoint GPO 2 :** Fond d'écran visible dans GPMC lié à OU Commercial. Si VM cliente disponible : s.leroy a le fond imposé.

---

### GPO 3 : Mappage de Lecteur Réseau

**Objectif :** Mapper automatiquement le lecteur `H:` sur le partage `\\SRV-AD-TECHPRO\Users\%LogonUser%` pour les utilisateurs de `OU_UTILISATEURS`.

#### Étape 1 : Créer le Partage sur le Serveur

```powershell
# Créer le dossier de base
New-Item -Path "C:\Partages\Users" -ItemType Directory -Force

# Créer des sous-dossiers pour chaque utilisateur
$utilisateurs = @("m.durand","p.bernard","l.martin","e.petit","s.leroy","d.morel")
foreach ($u in $utilisateurs) {
    New-Item -Path "C:\Partages\Users\$u" -ItemType Directory -Force
}

# Partager le dossier Users
New-SmbShare -Name "Users" -Path "C:\Partages\Users" `
             -FullAccess "TECHPRO\Administrateur" `
             -ChangeAccess "TECHPRO\Domain Users" `
             -ReadAccess "TECHPRO\Domain Users"
```

#### Étape 2 : Créer la GPO

1. GPMC → Nouvelle GPO : `GPO_Lecteur_H`
2. Modifier → Naviguer vers :
   ```
   Configuration Utilisateur
   └── Préférences Windows
       └── Paramètres Windows
           └── Mappages de lecteurs
   ```
3. Clic droit dans le panneau droit → `Nouveau` → `Lecteur mappé`

#### Étape 3 : Configurer le Lecteur

- **Action :** Mettre à jour
- **Emplacement :** `\\SRV-AD-TECHPRO\Users\%LogonUser%`
- **Lettre de lecteur :** `H`
- **Reconnecter :** Coché
- **Étiquette :** `Mon espace personnel`
- `OK`

#### Étape 4 : Lier à OU_UTILISATEURS

GPMC → Clic droit sur `OU_UTILISATEURS` → Lier `GPO_Lecteur_H`.

#### Étape 5 : Tester

```cmd
gpupdate /force /logoff
:: Reconnecter en tant que l.martin
:: Le lecteur H: doit apparaître dans l'Explorateur
net use   :: Lister les lecteurs mappés
```

✅ **Checkpoint GPO 3 :** Lecteur H: visible. Capture Explorateur Windows ou `net use`.

---

### Vérifications Finales GPO

```powershell
# Lister toutes les GPO du domaine
Get-GPO -All | Select-Object DisplayName, GpoStatus, CreationTime | Format-Table

# Vérifier les liens de chaque GPO
$gpos = @("GPO_Securite_Comptes","GPO_Fond_Ecran_Commercial","GPO_Lecteur_H")
foreach ($gpo in $gpos) {
    Write-Host "=== Liens de : $gpo ===" -ForegroundColor Cyan
    (Get-GPO -Name $gpo).GenerateReportToFile("HTML","C:\GPO_$gpo.html")
    Write-Host "Rapport : C:\GPO_$gpo.html"
}
```

**Captures à fournir pour le livrable :**
1. GPMC avec les 3 GPO liées aux OU correspondantes
2. Sortie `gpresult /r` montrant les GPO appliquées
3. (Si VM cliente) Fond d'écran imposé sur `s.leroy`
4. (Si VM cliente) Lecteur H: visible dans l'Explorateur

---

## 🖥️ TP PARTIE 2 : DNS WINDOWS ET ZONES AD-INTÉGRÉES

**Prérequis :** La zone `techpro.local` a été créée automatiquement lors de la promotion DC en S11.

---

### Étape 1 : Explorer la Zone DNS Existante

```
Win+R → dnsmgmt.msc
```

1. Déployer `SRV-AD-TECHPRO` → `Zones de recherche directe` → `techpro.local`
2. Observer les enregistrements déjà présents :
   - Enregistrements `A` pour `SRV-AD-TECHPRO` (créés automatiquement)
   - Enregistrements `SRV` dans les sous-dossiers `_msdcs`, `_tcp`, `_udp`

**Vérifier que la zone est AD-intégrée :**
- Clic droit sur `techpro.local` → `Propriétés`
- `Type` → doit afficher `Intégré Active Directory`
- `Mises à jour dynamiques` → doit être `Sécurisées uniquement`

---

### Étape 2 : Ajouter des Enregistrements A

Nous allons simuler que le réseau TechPro dispose de plusieurs serveurs.

**Via la console DNS (méthode graphique) :**

1. Clic droit sur `techpro.local` → `Nouvel hôte (A ou AAAA)...`
2. Remplir et `Ajouter un hôte` pour chacun :

| **Nom** | **IP** | **Créer PTR** |
|---------|--------|---------------|
| `www` | `192.168.50.100` | Coché (si zone inverse existe) |
| `intranet-srv` | `192.168.50.101` | Coché |
| `nas` | `192.168.50.10` | Coché |
| `srv-fichier` | `192.168.50.20` | Coché |

**Via PowerShell :**

```powershell
Add-DnsServerResourceRecordA -ZoneName "techpro.local" -Name "www"          -IPv4Address "192.168.50.100" -CreatePtr
Add-DnsServerResourceRecordA -ZoneName "techpro.local" -Name "intranet-srv" -IPv4Address "192.168.50.101" -CreatePtr
Add-DnsServerResourceRecordA -ZoneName "techpro.local" -Name "nas"          -IPv4Address "192.168.50.10"  -CreatePtr
Add-DnsServerResourceRecordA -ZoneName "techpro.local" -Name "srv-fichier"  -IPv4Address "192.168.50.20"  -CreatePtr
```

---

### Étape 3 : Ajouter un Enregistrement CNAME

```powershell
# Alias "intranet" vers "www.techpro.local."
Add-DnsServerResourceRecordCName -ZoneName "techpro.local" `
                                  -Name "intranet" `
                                  -HostNameAlias "www.techpro.local."
```

**Via console :** Clic droit → `Nouvel alias (CNAME)...` → Nom : `intranet` → FQDN cible : `www.techpro.local.`

---

### Étape 4 : Créer la Zone Inverse AD-Intégrée

```
Console DNS → Clic droit sur "Zones de recherche inversée"
→ "Nouvelle zone..."
```

1. **Type de zone :** `Zone principale` → Cocher `Stocker la zone dans Active Directory`
2. **Étendue de réplication :** `Vers tous les serveurs DNS exécutant sur les contrôleurs de domaine dans ce domaine : techpro.local`
3. **Zone de recherche inversée IPv4**
4. **ID réseau :** `192.168.50` (l'assistant complète le reste)
5. **Mise à jour dynamique :** `Sécurisées uniquement`
6. Terminer.

La zone `50.168.192.in-addr.arpa` est créée.

**Ajouter l'enregistrement PTR du serveur :**

```powershell
Add-DnsServerResourceRecordPtr -ZoneName "50.168.192.in-addr.arpa" `
                                -Name "1" `
                                -PtrDomainName "srv-ad-techpro.techpro.local."
```

---

### Étape 5 : Tester la Résolution DNS

**Depuis le serveur :**

```powershell
# Test résolution directe
Resolve-DnsName www.techpro.local
Resolve-DnsName intranet.techpro.local        # CNAME → doit retourner IP de www

# Test résolution inverse
Resolve-DnsName 192.168.50.100

# Tester avec nslookup
nslookup www.techpro.local 127.0.0.1
nslookup 192.168.50.100 127.0.0.1
```

```cmd
:: Via nslookup interactif
nslookup
> server 127.0.0.1
> set type=A
> www.techpro.local
> set type=CNAME
> intranet.techpro.local
> set type=PTR
> 192.168.50.100
> exit
```

**Lister tous les enregistrements :**

```powershell
# Zone directe
Get-DnsServerResourceRecord -ZoneName "techpro.local" | Format-Table HostName, RecordType, RecordData

# Zone inverse
Get-DnsServerResourceRecord -ZoneName "50.168.192.in-addr.arpa" | Format-Table HostName, RecordType, RecordData
```

---

### Grille de Validation TP Complet

| **Critère** | **Vérification** | **Résultat attendu** | **✅/❌** |
|-------------|------------------|----------------------|----------|
| GPO_Securite_Comptes créée | GPMC | Visible dans Objets de stratégie | |
| GPO_Securite_Comptes liée | GPMC → OU_UTILISATEURS | Lien visible | |
| Paramètre MDP = 10 caract. | Éditeur GPO → Stratégie MDP | Longueur min = 10 | |
| Verrouillage = 5 tentatives | Éditeur GPO → Verrouillage | Seuil = 5 | |
| GPO_Fond_Ecran liée | GPMC → OU Commercial | Lien visible | |
| Chemin fond d'écran configuré | Éditeur GPO → Bureau | Chemin UNC correct | |
| GPO_Lecteur_H liée | GPMC → OU_UTILISATEURS | Lien visible | |
| gpresult /r OK | Terminal | 3 GPO listées appliquées | |
| Zone directe AD-intégrée | `dnsmgmt.msc` | Propriétés : Intégré AD | |
| Enregistrements A créés | Console DNS | www, intranet-srv, nas, srv-fichier | |
| CNAME intranet → www | Console DNS | CNAME visible | |
| Zone inverse créée | Console DNS | 50.168.192.in-addr.arpa | |
| Résolution www réussie | `Resolve-DnsName` | IP 192.168.50.100 | |
| Résolution inverse réussie | `Resolve-DnsName` (IP) | Nom FQDN retourné | |

