# Semaine 2 (S2) - BLOC 1
## 🖥️ Gestion de Parc · Inventaire · Documentation · Fiche Technique

---
# 📚 FICHE DE COURS ÉLÈVE
## "Gestion de Parc Informatique · Documentation Technique"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 2*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B1.1** | Recenser et identifier les ressources numériques |
| **B1.2** | Exploiter des référentiels, normes et standards |
| **B1.4** | Mettre en place et exploiter des outils de gestion de parc |

---

## PARTIE I — La Gestion de Parc : Enjeux et Définition

### I.A. Définition

La **gestion de parc informatique** (ou **asset management** en anglais) désigne l'ensemble des activités permettant de **recenser, suivre et administrer** tous les équipements informatiques et logiciels d'une organisation, tout au long de leur cycle de vie.

```
   CYCLE DE VIE D'UN ÉQUIPEMENT
   ─────────────────────────────────────────────────────────────

   ① Acquisition          ② Déploiement        ③ Exploitation
   ─────────────          ────────────          ──────────────
   Achat / leasing        Installation          Utilisation
   Réception              Configuration         Maintenance
   Vérification           Intégration parc      Mises à jour
   → Créer la fiche       → Compléter la        → Maintenir la
                            fiche                 fiche à jour

   ④ Maintenance          ⑤ Fin de vie
   ─────────────          ────────────
   Réparation             Décommission
   Remplacement pièce     Effacement données
   Mise à niveau          Recyclage / Don
   → Historique           → Clôturer la fiche
     dans la fiche
```

*Légende : Le cycle de vie d'un équipement informatique en 5 phases. La fiche d'inventaire naît lors de l'acquisition et est mise à jour à chaque étape. Elle est clôturée (et conservée en archive) lors du décommissionnement.*

---

### I.B. Pourquoi Inventorier ? Les 6 Raisons Fondamentales

**1. Assistance et Dépannage**
Sans inventaire, chaque incident demande une investigation préalable du matériel et des logiciels. Avec un inventaire, le technicien connaît l'environnement avant même d'arriver au bureau de l'utilisateur.

**2. Gestion des Licences**
Les logiciels professionnels sont vendus sous licence. Une organisation qui utilise 80 copies d'un logiciel pour lequel elle a 60 licences est en **infraction légale** — et s'expose à des amendes lors d'audits (Microsoft, Adobe, BSA). L'inventaire évite les sur-licenciements (gaspillage) et les sous-licenciements (risque juridique).

**3. Planification et Budget**
Savoir que 30% du parc a plus de 5 ans permet de planifier les renouvellements matériels, d'anticiper les budgets et d'éviter les pannes en série en période de surcharge.

**4. Sécurité**
Un équipement non inventorié est un équipement non géré : pas de mises à jour, pas d'antivirus, potentiellement compromis. L'inventaire est la première ligne de défense d'une politique de sécurité. On ne peut pas sécuriser ce qu'on ne connaît pas.

**5. Conformité et Audit**
La réglementation (RGPD, ISO 27001, NIS2) impose de connaître les actifs qui traitent des données. Un auditeur de conformité demande systématiquement l'inventaire du parc comme premier document.

**6. Continuité de Service (ITIL)**
En cas de panne ou de sinistre, l'inventaire permet de savoir exactement ce qui a été perdu ou endommagé, d'évaluer l'impact et de prioriser la restauration des équipements critiques.

---

### I.C. Les Trois Dimensions de l'Inventaire

Un inventaire de parc complet couvre trois dimensions distinctes :

```
   ┌──────────────────────────────────────────────────────────────┐
   │                    INVENTAIRE DE PARC                        │
   │                                                              │
   │   ┌──────────────┐  ┌──────────────┐  ┌────────────────┐    │
   │   │   MATÉRIEL   │  │   LOGICIEL   │  │    LICENCES    │    │
   │   │              │  │              │  │                │    │
   │   │ • PC/laptops │  │ • OS (W/L)   │  │ • Type (OEM,  │    │
   │   │ • Serveurs   │  │ • Bureautique│  │   Volume, Sub) │    │
   │   │ • Switches   │  │ • Métier     │  │ • Nombre       │    │
   │   │ • Routeurs   │  │ • Sécurité   │  │ • Expiration   │    │
   │   │ • Imprimantes│  │ • Agents IT  │  │ • Clé/serial   │    │
   │   │ • Téléphones │  │              │  │                │    │
   │   └──────────────┘  └──────────────┘  └────────────────┘    │
   │                                                              │
   │   + Localisation + Affectation utilisateur + Historique      │
   └──────────────────────────────────────────────────────────────┘
```

---

### I.D. Types de Licences Logicielles à Connaître

| **Type de licence** | **Signification** | **Exemple** |
|---|---|---|
| **OEM** (Original Equipment Manufacturer) | Liée au matériel — non transférable | Windows livré avec un PC |
| **Retail / Boîte** | Liée à un utilisateur — transférable entre machines | Achat en magasin |
| **Volume (VL)** | Bloc de licences pour une organisation | Microsoft Open License |
| **Abonnement (SaaS)** | Location mensuelle ou annuelle | Microsoft 365, Adobe CC |
| **Open Source / GPL** | Libre d'utilisation, de modification et de distribution | Linux, LibreOffice |
| **Freeware** | Gratuit, mais code non accessible et conditions restrictives | 7-Zip, VLC (gratuit mais pas open source) |
| **Shareware** | Essai gratuit limité — paiement pour les fonctions complètes | WinRAR |

> ⚠️ **Cas courant en entreprise :** Un logiciel "gratuit" en usage personnel peut être payant pour un usage commercial. Vérifier systématiquement les conditions de licence, même pour les logiciels réputés gratuits.

---

## PARTIE II — ITIL et Gestion des Actifs

### II.A. La Pratique ITIL "Gestion des Actifs IT"

Dans le référentiel **ITIL 4**, la **Gestion des Actifs IT** (IT Asset Management — ITAM) est une pratique dédiée à planifier et gérer le cycle de vie complet de tous les actifs IT. Elle s'appuie sur une base de données appelée **CMDB** (Configuration Management DataBase).

| **Terme ITIL** | **Signification** | **Équivalent concret** |
|---|---|---|
| **Actif IT** | Tout élément ayant une valeur pour l'organisation | Un PC, un serveur, une licence, un câble |
| **CMDB** | Base de données des configurations | L'inventaire de parc en base de données |
| **CI** | Configuration Item — élément géré dans la CMDB | Une fiche technique dans la CMDB |
| **Relation** | Lien entre deux CIs | "Ce PC est utilisé par cet utilisateur qui est dans ce service" |

> 💡 **Pour votre culture professionnelle :** Les outils de gestion de parc professionnels (GLPI, ManageEngine, Lansweeper, ServiceNow) sont des implémentations concrètes de la CMDB ITIL. Vous les rencontrerez dès votre premier poste en entreprise.

---

### II.B. Outils de Gestion de Parc

| **Outil** | **Type** | **Caractéristiques** |
|---|---|---|
| **GLPI** | Open source (PHP) | ITSM complet — tickets + inventaire + CMDB |
| **OCS Inventory** | Open source | Agent local pour inventaire automatique |
| **Lansweeper** | Freemium | Scan réseau automatique — très répandu en PME |
| **Microsoft SCCM / Intune** | Microsoft (payant) | Gestion centralisée parc Windows |
| **ManageEngine AssetExplorer** | Freemium | ITAM complet, très utilisé en France |
| **Excel / Google Sheets** | Universel (gratuit) | Simple mais non automatisé — PME sans outil dédié |

---

## PARTIE III — La Fiche Technique : Cœur de l'Inventaire

### III.A. Qu'est-ce qu'une Fiche Technique ?

Une **fiche technique** (ou **fiche d'inventaire**) est le document qui recense toutes les informations pertinentes d'un équipement informatique à un moment donné. Elle est à la fois :

- Une **preuve d'existence** de l'équipement dans le parc
- Un **outil de diagnostic** pour les interventions futures
- Une **trace d'historique** des modifications apportées
- Un **élément de conformité** pour les audits

### III.B. Informations à Collecter pour un Poste de Travail

**Identification :**

| **Champ** | **Exemple** | **Où le trouver** |
|---|---|---|
| Numéro d'inventaire | PC-2024-042 | Étiquette sur le boîtier |
| Numéro de série | 5CG9382XKL | Étiquette, BIOS, `wmic bios get serialnumber` |
| Marque et modèle | Dell OptiPlex 7090 | Étiquette, boîtier |
| Type d'équipement | Poste fixe / Laptop / Tout-en-un | Visuel |
| Date d'achat | 14/03/2023 | Facture, bon de livraison |
| Fin de garantie | 14/03/2026 | Site constructeur, facture |
| Localisation | Bâtiment A — Bureau 214 | Relevé physique |
| Utilisateur affecté | Alice Martin — Service RH | RH ou AD |

**Composants Matériels :**

| **Composant** | **Détail à noter** | **Outil de collecte** |
|---|---|---|
| **Processeur (CPU)** | Marque, modèle, fréquence, nombre de cœurs | `msinfo32`, `wmic cpu get name`, `lscpu` |
| **Mémoire vive (RAM)** | Capacité totale (Go), fréquence, nombre de barrettes | `msinfo32`, `wmic memorychip get capacity` |
| **Stockage** | Type (HDD/SSD/NVMe), capacité, marque | `msinfo32`, gestionnaire de disques, `lsblk` |
| **Carte graphique (GPU)** | Marque, modèle, VRAM | Gestionnaire de périphériques |
| **Carte réseau (Ethernet)** | Marque, modèle, adresse MAC | `ipconfig /all`, `ip link` |
| **WiFi** | Marque, modèle, normes supportées (802.11ac, ax...) | Gestionnaire de périphériques |
| **Carte mère** | Marque, modèle, version BIOS | `msinfo32`, `dmidecode` |
| **Alimentation** | Puissance (W) — laptops : capacité batterie | Physique ou fiche constructeur |
| **Écran** | Taille, résolution, marque | Physique, affichage Windows |

**Système et Logiciels :**

| **Champ** | **Exemple** | **Outil de collecte** |
|---|---|---|
| Système d'exploitation | Windows 11 Pro 23H2 | `winver`, `msinfo32`, `uname -a` |
| Clé de licence OS | OEM / intégrée UEFI | `wmic path softwarelicensingservice get OA3xOriginalProductKey` |
| Domaine / Groupe de travail | `siosarl.local` | `msinfo32`, Propriétés système |
| Logiciels installés | Liste avec versions | Ajout/Suppression de programmes, `wmic product`, `Get-InstalledProgram` |
| Antivirus | Windows Defender 4.18.x | Centre de sécurité Windows |
| Dernière mise à jour OS | 12/10/2024 — KB5031455 | Windows Update |

**Réseau :**

| **Champ** | **Exemple** | **Outil de collecte** |
|---|---|---|
| Adresse IP | 192.168.0.52 (DHCP) ou fixe | `ipconfig`, `ip addr` |
| Masque de sous-réseau | 255.255.255.0 (/24) | `ipconfig`, `ip addr` |
| Passerelle par défaut | 192.168.0.1 | `ipconfig`, `ip route` |
| Serveur DNS | 192.168.0.145 | `ipconfig /all` |
| Adresse MAC | 00:1A:2B:3C:4D:5E | `ipconfig /all`, `ip link` |
| VLAN | VLAN 10 — RH | Switch/documentation réseau |

---

### III.C. Commandes de Collecte — Windows

```powershell
# ─── Informations système complètes ──────────────────────────────
msinfo32                    # Interface graphique complète
msinfo32 /report C:\info.txt  # Export en fichier texte

# ─── CPU ─────────────────────────────────────────────────────────
wmic cpu get name,NumberOfCores,MaxClockSpeed
# → Intel(R) Core(TM) i5-10400 @ 2.90GHz   6   2904

# ─── RAM ─────────────────────────────────────────────────────────
wmic memorychip get capacity,speed
# → 8589934592  3200   (8Go en octets, 3200 MHz)

# Version PowerShell plus lisible :
Get-CimInstance Win32_PhysicalMemory | Select-Object Capacity, Speed, Manufacturer

# ─── Stockage ────────────────────────────────────────────────────
wmic diskdrive get model,size,mediatype
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, Size

# ─── Numéro de série ─────────────────────────────────────────────
wmic bios get serialnumber
Get-CimInstance Win32_BIOS | Select-Object SerialNumber

# ─── Réseau ──────────────────────────────────────────────────────
ipconfig /all               # IP, masque, GW, DNS, MAC

# ─── Logiciels installés ─────────────────────────────────────────
wmic product get name,version
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select-Object DisplayName, DisplayVersion, Publisher |
    Sort-Object DisplayName
```

---

### III.D. Commandes de Collecte — Linux (Debian/Ubuntu)

```bash
# ─── CPU ─────────────────────────────────────────────────────────
lscpu
cat /proc/cpuinfo | grep "model name" | uniq

# ─── RAM ─────────────────────────────────────────────────────────
free -h                                # Mémoire totale et disponible
sudo dmidecode --type 17               # Détail des barrettes (root requis)

# ─── Stockage ────────────────────────────────────────────────────
lsblk                                  # Disques et partitions
df -h                                  # Espace utilisé par partition
sudo fdisk -l                          # Détail des disques

# ─── Matériel complet ────────────────────────────────────────────
sudo lshw                              # Inventaire matériel complet
sudo lshw -short                       # Version condensée
sudo lshw -html > inventaire.html      # Export HTML

# ─── Réseau ──────────────────────────────────────────────────────
ip addr show                           # Adresses IP + MACs
ip route show                          # Table de routage (passerelle)
cat /etc/resolv.conf                   # Serveurs DNS

# ─── Numéro de série ─────────────────────────────────────────────
sudo dmidecode -s system-serial-number
sudo dmidecode -s baseboard-product-name

# ─── Logiciels installés ─────────────────────────────────────────
dpkg --list                            # Tous les paquets installés
dpkg --list | grep "ii" | wc -l        # Compter les paquets
apt list --installed 2>/dev/null       # Version moderne
```

---

### III.E. Bonnes Pratiques de Documentation

| **Règle** | **Mauvaise pratique** | **Bonne pratique** |
|---|---|---|
| **Précision** | "RAM : 8 Go" | "RAM : 2 × 8 Go DDR4 3200 MHz Samsung" |
| **Complétude** | Laisser un champ vide | Indiquer "Inconnu" ou "À vérifier" |
| **Datation** | Pas de date | Chaque fiche porte une date de création et une date de dernière modification |
| **Versionning** | Écraser l'ancienne fiche | Conserver l'historique (v1.0 → v1.1 → v2.0) |
| **Objectivité** | "Vieux PC lent" | "Dell OptiPlex 3050, i3-7100, 4 Go RAM — obsolète selon politique renouvellement (> 5 ans)" |
| **Format standard** | Fiche libre sans structure | Modèle standardisé pour toute l'organisation |
| **Accessibilité** | Fiche sur le bureau du technicien | Fiche dans le GLPI ou espace partagé |

---

## PARTIE IV — Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|---------------|
| **Gestion de parc** | Ensemble des activités de recensement et de suivi des équipements informatiques |
| **Inventaire** | Liste exhaustive des actifs informatiques d'une organisation |
| **Fiche technique** | Document décrivant les caractéristiques complètes d'un équipement |
| **CMDB** | Configuration Management DataBase — base de données des CIs |
| **CI** | Configuration Item — tout actif géré dans une CMDB |
| **GLPI** | Gestion Libre de Parc Informatique — logiciel ITSM open source français |
| **Licence OEM** | Licence liée au matériel d'origine, non transférable |
| **Licence Volume** | Bloc de licences acheté en quantité pour une organisation |
| **SaaS** | Software as a Service — logiciel accessible par abonnement via internet |
| **BSA** | Business Software Alliance — organisme qui traque les logiciels piratés en entreprise |
| **`msinfo32`** | Outil Windows d'informations système (interface graphique) |
| **`wmic`** | Windows Management Instrumentation Command line |
| **`lshw`** | List Hardware — outil Linux de collecte d'informations matérielles |
| **`dmidecode`** | Outil Linux lisant les données DMI/SMBIOS du matériel |
| **EAN / Code-barres** | Identifiant physique apposé sur l'équipement pour le traçage |
| **Fin de garantie** | Date après laquelle le constructeur ne couvre plus les pannes matérielles |
| **EOL** | End Of Life — date à partir de laquelle un logiciel ne reçoit plus de mises à jour |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] Je cite 4 raisons pour lesquelles un inventaire de parc est indispensable
- [ ] Je distingue inventaire matériel, logiciel et licences
- [ ] Je connais les 3 types de licences les plus courants (OEM, Volume, SaaS)
- [ ] Je sais quelle commande Windows/Linux utiliser pour trouver le CPU, la RAM, le numéro de série
- [ ] J'ai rempli une fiche technique complète sans champ vide
- [ ] J'explique le lien entre gestion de parc et CMDB ITIL
- [ ] Je comprends pourquoi un logiciel "gratuit" peut créer un problème de licence en entreprise

---
