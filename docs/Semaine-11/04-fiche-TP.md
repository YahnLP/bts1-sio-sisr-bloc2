# Semaine 11 (S11) - BLOC 2
## "Windows Server, Active Directory et Structure d'Annuaire"

---

## 🖥️ TP GUIDÉ PARTIE 1
### Installation et Configuration de Windows Server 2022

**Durée estimée : 45 minutes**

---

### Étape 1 : Créer la Machine Virtuelle (5 min)

Dans **VirtualBox** :

1. `Nouveau` → Nom : `WinSrv2022_TechPro` → Type : `Microsoft Windows` → Version : `Windows 2022 (64-bit)`
2. RAM : **2048 Mo** (2 Go) minimum, 4096 Mo recommandé
3. Disque dur : `Créer un disque virtuel maintenant` → `VDI` → `Dynamiquement alloué` → `50 Go`
4. Réseau : `Adaptateur 1` → `Réseau hôte uniquement (Host-only)`

**Monter l'ISO :**
- Paramètres → Stockage → Contrôleur IDE → Disque vide → Choisir le fichier ISO Windows Server 2022

---

### Étape 2 : Installer Windows Server 2022 (15 min)

Démarrer la VM :

1. **Langue** : Français (France), Français (France), Français
2. `Installer maintenant`
3. **Clé de produit** : `Ignorer` (évaluation 180 jours)
4. **Édition** : Sélectionner `Windows Server 2022 Standard (Desktop Experience)` ← Avec interface graphique
5. **Type d'installation** : `Personnalisée : installer uniquement Windows`
6. **Partition** : Sélectionner l'espace non alloué → `Suivant`
7. Attendre l'installation (~10-15 min)
8. Définir le mot de passe Administrateur local : `Admin@BTS2024`
9. Se connecter : `Ctrl+Alt+Suppr` → `Admin@BTS2024`

---

### Étape 3 : Configuration Post-Installation (10 min)

#### 3.1 – Paramètres régionaux

Vérifier que le clavier est bien AZERTY :
- `Paramètres` → `Heure et langue` → `Langue` → Vérifier `Français (France)`

#### 3.2 – Renommer le serveur

```
Clic droit sur le bouton Démarrer → Système
→ Renommer ce PC
→ Nom : SRV-AD-TECHPRO
→ Redémarrer plus tard
```

Ou via **Server Manager** → `Serveur local` → Cliquer sur le nom de l'ordinateur.

#### 3.3 – Configurer l'adresse IP fixe

```
Panneau de configuration → Centre Réseau et partage → Ethernet
→ Propriétés → Protocole Internet version 4 (TCP/IPv4) → Propriétés
→ Utiliser l'adresse IP suivante :
   Adresse IP : 192.168.50.1
   Masque : 255.255.255.0
   Passerelle : 192.168.50.1 (le serveur est lui-même la passerelle en TP)
   DNS préféré : 127.0.0.1 (il sera son propre DNS après promotion)
```

#### 3.4 – Redémarrer

Redémarrer le serveur pour appliquer le nouveau nom.

#### 3.5 – Vérification

Ouvrir `PowerShell` ou `Invite de commandes` :
```cmd
hostname          → doit afficher SRV-AD-TECHPRO
ipconfig /all     → vérifier IP 192.168.50.1, DNS 127.0.0.1
```

✅ **Checkpoint 1 :** Nom `SRV-AD-TECHPRO` confirmé, IP `192.168.50.1` configurée.

---

### Étape 4 : Installer le Rôle AD DS (10 min)

Depuis le **Gestionnaire de serveur** :

1. `Gérer` → `Ajouter des rôles et fonctionnalités`
2. `Avant de commencer` → `Suivant`
3. `Type d'installation` : **Installation basée sur un rôle ou une fonctionnalité** → `Suivant`
4. `Sélection du serveur` : `SRV-AD-TECHPRO` → `Suivant`
5. `Rôles de serveurs` : Cocher **Active Directory Domain Services**
   - Une fenêtre apparaît pour ajouter des fonctionnalités requises → `Ajouter des fonctionnalités`
6. `Fonctionnalités` → `Suivant`
7. `AD DS` → lire le résumé → `Suivant`
8. `Confirmation` → `Installer`
9. Attendre la fin de l'installation (~3-5 min)
10. **Ne pas redémarrer encore.** Laisser le gestionnaire ouvert.

---

### Étape 5 : Promouvoir le Serveur en Contrôleur de Domaine (15 min)

Après installation du rôle, une notification apparaît dans le Gestionnaire de serveur :
`⚠️ Configuration post-déploiement requise`

1. Cliquer sur le drapeau jaune → `Promouvoir ce serveur en contrôleur de domaine`

2. **Configuration du déploiement :**
   - Sélectionner : `Ajouter une nouvelle forêt`
   - Nom de domaine racine : `techpro.local`
   - `Suivant`

3. **Options du contrôleur de domaine :**
   - Niveau fonctionnel de la forêt : `Windows Server 2016`
   - Niveau fonctionnel du domaine : `Windows Server 2016`
   - Cocher : `Serveur DNS` et `Catalogue global (GC)`
   - Mot de passe DSRM : `Dsrm@BTS2024` ← **NOTER ce mot de passe !**
   - `Suivant`

4. **Options DNS :**
   - Ignorer l'avertissement sur la délégation DNS
   - `Suivant`

5. **Options supplémentaires :**
   - Nom NetBIOS du domaine : `TECHPRO` (auto-rempli)
   - `Suivant`

6. **Chemins d'accès :**
   - Laisser les chemins par défaut
   - `Suivant`

7. **Vérifier les options** → `Suivant`

8. **Vérification des prérequis** → Des avertissements peuvent apparaître (normal) → `Installer`

9. Le serveur redémarre automatiquement (~5 min).

---

### Étape 6 : Première Connexion au Domaine (5 min)

Après redémarrage :
- Ecran de connexion : `TECHPRO\Administrateur` (et non plus `Administrateur` seul)
- Mot de passe : `Admin@BTS2024`

**Vérifications post-promotion :**

```powershell
# Vérifier que le domaine est créé
Get-ADDomain

# Vérifier que le DNS est fonctionnel
Resolve-DnsName techpro.local

# Vérifier les contrôleurs de domaine
Get-ADDomainController -Filter *
```

Ou via **Server Manager** → Le rôle `AD DS` doit apparaître en vert.

✅ **Checkpoint 2 :** Connexion avec `TECHPRO\Administrateur`, domaine `techpro.local` visible.

---

## 🖥️ TP GUIDÉ PARTIE 2
### Création du Domaine, Structure OU et Objets Active Directory

**Durée estimée : 70 minutes**

**Contexte :** TechPro SARL (organigramme de l'activité découverte) doit être créée dans Active Directory. 6 employés, 3 services principaux, sous-divisions.

---

### Étape 1 : Ouvrir la Console ADUC (2 min)

```
Démarrer → Outils d'administration Windows → Utilisateurs et ordinateurs Active Directory
```
*Ou :* `Win+R` → `dsa.msc` → `Entrée`

**Vue initiale :**
```
Active Directory Users and Computers
└── techpro.local
    ├── Builtin          (conteneurs système)
    ├── Computers        (conteneur par défaut ordinateurs)
    ├── Domain Controllers
    ├── ForeignSecurityPrincipals
    └── Users            (conteneur par défaut utilisateurs)
```

> **Important :** Les conteneurs `Users` et `Computers` ne sont **pas** des OU (pas d'icône dossier jaune). On ne peut pas y lier des GPO. C'est pourquoi on crée nos propres OU.

---

### Étape 2 : Créer la Structure OU (20 min)

Reproduire la structure suivante dans ADUC :

```
techpro.local
├── OU_UTILISATEURS
│   ├── Direction
│   ├── Informatique
│   │   ├── Réseau
│   │   └── Développement
│   ├── Commercial
│   └── RH
│
├── OU_ORDINATEURS
│   ├── Postes-Direction
│   ├── Postes-Informatique
│   └── Postes-Commercial
│
└── OU_GROUPES
```

**Procédure pour créer une OU (méthode graphique) :**

1. Clic droit sur `techpro.local` → `Nouveau` → `Unité d'organisation`
2. Nom : `OU_UTILISATEURS` → laisser cochée `Protéger contre la suppression accidentelle` → `OK`
3. Clic droit sur `OU_UTILISATEURS` → `Nouveau` → `Unité d'organisation` → Nom : `Direction`
4. Répéter pour toutes les OU

**Via PowerShell (plus rapide, copier-coller) :**

```powershell
# Structure principale
New-ADOrganizationalUnit -Name "OU_UTILISATEURS" -Path "DC=techpro,DC=local" -ProtectedFromAccidentalDeletion $true
New-ADOrganizationalUnit -Name "OU_ORDINATEURS" -Path "DC=techpro,DC=local" -ProtectedFromAccidentalDeletion $true
New-ADOrganizationalUnit -Name "OU_GROUPES" -Path "DC=techpro,DC=local" -ProtectedFromAccidentalDeletion $true

# Sous-OU Utilisateurs
New-ADOrganizationalUnit -Name "Direction" -Path "OU=OU_UTILISATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "Informatique" -Path "OU=OU_UTILISATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "Commercial" -Path "OU=OU_UTILISATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "RH" -Path "OU=OU_UTILISATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "Réseau" -Path "OU=Informatique,OU=OU_UTILISATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "Développement" -Path "OU=Informatique,OU=OU_UTILISATEURS,DC=techpro,DC=local"

# Sous-OU Ordinateurs
New-ADOrganizationalUnit -Name "Postes-Direction" -Path "OU=OU_ORDINATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "Postes-Informatique" -Path "OU=OU_ORDINATEURS,DC=techpro,DC=local"
New-ADOrganizationalUnit -Name "Postes-Commercial" -Path "OU=OU_ORDINATEURS,DC=techpro,DC=local"
```

✅ **Checkpoint 3 :** Dans ADUC, la structure arborescente est visible et correspond au schéma.

---

### Étape 3 : Créer les Utilisateurs (25 min)

**Employés à créer** (référence : tableau organigramme) :

| **Nom complet** | **SAMAccountName** | **OU cible** | **Groupe** |
|-----------------|--------------------|--------------|------------|
| Marie Durand | m.durand | Direction | GRP_Direction |
| Paul Bernard | p.bernard | RH | GRP_RH |
| Lucas Martin | l.martin | Réseau | GRP_Informatique |
| Emma Petit | e.petit | Développement | GRP_Informatique |
| Sophie Leroy | s.leroy | Commercial | GRP_Commercial |
| David Morel | d.morel | Commercial | GRP_Commercial |

**Procédure (méthode graphique) :**

1. Clic droit sur `OU Direction` → `Nouveau` → `Utilisateur`
2. Remplir :
   - Prénom : `Marie`
   - Nom : `DURAND`
   - Nom d'ouverture de session de l'utilisateur : `m.durand`
3. `Suivant`
4. Mot de passe : `Azerty@2024`
5. Cocher : `L'utilisateur doit changer le mot de passe à la prochaine ouverture de session`
6. `Suivant` → `Terminer`

**Via PowerShell (rapide pour tous les utilisateurs) :**

```powershell
# Définir le mot de passe commun
$MDP = ConvertTo-SecureString "Azerty@2024" -AsPlainText -Force

# Marie Durand - Direction
New-ADUser -Name "Marie Durand" -GivenName "Marie" -Surname "Durand" `
           -SamAccountName "m.durand" `
           -UserPrincipalName "m.durand@techpro.local" `
           -Path "OU=Direction,OU=OU_UTILISATEURS,DC=techpro,DC=local" `
           -AccountPassword $MDP -Enabled $true -ChangePasswordAtLogon $true

# Paul Bernard - RH
New-ADUser -Name "Paul Bernard" -GivenName "Paul" -Surname "Bernard" `
           -SamAccountName "p.bernard" `
           -UserPrincipalName "p.bernard@techpro.local" `
           -Path "OU=RH,OU=OU_UTILISATEURS,DC=techpro,DC=local" `
           -AccountPassword $MDP -Enabled $true -ChangePasswordAtLogon $true

# Lucas Martin - Réseau
New-ADUser -Name "Lucas Martin" -GivenName "Lucas" -Surname "Martin" `
           -SamAccountName "l.martin" `
           -UserPrincipalName "l.martin@techpro.local" `
           -Path "OU=Réseau,OU=Informatique,OU=OU_UTILISATEURS,DC=techpro,DC=local" `
           -AccountPassword $MDP -Enabled $true -ChangePasswordAtLogon $true

# Emma Petit - Développement
New-ADUser -Name "Emma Petit" -GivenName "Emma" -Surname "Petit" `
           -SamAccountName "e.petit" `
           -UserPrincipalName "e.petit@techpro.local" `
           -Path "OU=Développement,OU=Informatique,OU=OU_UTILISATEURS,DC=techpro,DC=local" `
           -AccountPassword $MDP -Enabled $true -ChangePasswordAtLogon $true

# Sophie Leroy - Commercial
New-ADUser -Name "Sophie Leroy" -GivenName "Sophie" -Surname "Leroy" `
           -SamAccountName "s.leroy" `
           -UserPrincipalName "s.leroy@techpro.local" `
           -Path "OU=Commercial,OU=OU_UTILISATEURS,DC=techpro,DC=local" `
           -AccountPassword $MDP -Enabled $true -ChangePasswordAtLogon $true

# David Morel - Commercial
New-ADUser -Name "David Morel" -GivenName "David" -Surname "Morel" `
           -SamAccountName "d.morel" `
           -UserPrincipalName "d.morel@techpro.local" `
           -Path "OU=Commercial,OU=OU_UTILISATEURS,DC=techpro,DC=local" `
           -AccountPassword $MDP -Enabled $true -ChangePasswordAtLogon $true

# Vérifier
Get-ADUser -Filter * | Select-Object Name, SamAccountName, DistinguishedName
```

✅ **Checkpoint 4 :** 6 utilisateurs visibles dans ADUC dans les bonnes OU.

---

### Étape 4 : Créer les Groupes et Ajouter les Membres (15 min)

```powershell
# Créer les groupes dans OU_GROUPES
New-ADGroup -Name "GRP_Direction" `
            -GroupScope Global -GroupCategory Security `
            -Path "OU=OU_GROUPES,DC=techpro,DC=local" `
            -Description "Membres de la Direction"

New-ADGroup -Name "GRP_Informatique" `
            -GroupScope Global -GroupCategory Security `
            -Path "OU=OU_GROUPES,DC=techpro,DC=local" `
            -Description "Membres du service Informatique"

New-ADGroup -Name "GRP_Commercial" `
            -GroupScope Global -GroupCategory Security `
            -Path "OU=OU_GROUPES,DC=techpro,DC=local" `
            -Description "Membres du service Commercial"

New-ADGroup -Name "GRP_RH" `
            -GroupScope Global -GroupCategory Security `
            -Path "OU=OU_GROUPES,DC=techpro,DC=local" `
            -Description "Membres des Ressources Humaines"

# Ajouter les membres
Add-ADGroupMember -Identity "GRP_Direction"    -Members "m.durand"
Add-ADGroupMember -Identity "GRP_RH"           -Members "p.bernard"
Add-ADGroupMember -Identity "GRP_Informatique" -Members "l.martin","e.petit"
Add-ADGroupMember -Identity "GRP_Commercial"   -Members "s.leroy","d.morel"

# Vérifier
Get-ADGroupMember -Identity "GRP_Informatique" | Select Name
Get-ADGroupMember -Identity "GRP_Commercial"   | Select Name
```

**Via ADUC (méthode graphique) :**
1. Clic droit sur `OU_GROUPES` → `Nouveau` → `Groupe`
2. Nom : `GRP_Direction` | Étendue : `Global` | Type : `Sécurité` → `OK`
3. Double-clic sur le groupe → Onglet `Membres` → `Ajouter` → Taper `m.durand` → `Vérifier les noms` → `OK`

---

### Étape 5 : Vérifications Finales (8 min)

```powershell
# Vue d'ensemble de la structure
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName | Sort-Object DistinguishedName

# Tous les utilisateurs avec leur OU
Get-ADUser -Filter * -Properties DistinguishedName | Select-Object Name, SamAccountName, DistinguishedName

# Membres de chaque groupe
foreach ($grp in @("GRP_Direction","GRP_RH","GRP_Informatique","GRP_Commercial")) {
    Write-Host "`n=== $grp ===" -ForegroundColor Cyan
    Get-ADGroupMember -Identity $grp | Select Name
}
```

**Capture obligatoire pour le livrable :**
1. Vue ADUC complète arborescente (toutes les OU développées)
2. Propriétés d'un utilisateur (onglet Général + onglet Membre de)
3. Propriétés d'un groupe (onglet Membres)
4. Sortie PowerShell `Get-ADUser -Filter *`

