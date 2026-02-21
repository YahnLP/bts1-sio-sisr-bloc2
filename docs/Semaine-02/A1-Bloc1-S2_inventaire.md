# Semaine 2 (S2) - BLOC 1
## 🖥️ Gestion de Parc · Inventaire · Documentation · Fiche Technique

---

# 🖥️ FICHE TP — INVENTAIRE DU POSTE DE TRAVAIL

*Durée 🕑 : 90 minutes*

---

## Objectifs

- Collecter les informations techniques complètes d'un poste de travail réel
- Utiliser les outils système (graphique et ligne de commande) pour valider les informations
- Remplir une fiche technique exploitable et la verser au portfolio

---

## Étape 0 — Identification du Poste (5 min)

Avant d'allumer l'ordinateur, noter les informations **visibles physiquement** :

| **Information** | **Valeur** |
|---|---|
| Marque et modèle (étiquette boîtier) | |
| Numéro de série (étiquette dos/dessous) | |
| Type d'équipement (fixe / laptop / AIO) | |
| Présence d'une étiquette d'inventaire (numéro) | |
| Port(s) réseau visible(s) (RJ45, WiFi antenne ?) | |
| Ports disponibles (USB, HDMI, DisplayPort...) | |

---

## Étape 1 — Collecte Matérielle sous Windows (30 min)

### 1.1 — Avec msinfo32

Ouvrir `msinfo32` (Recherche Windows → "Informations système") et noter :

| **Information** | **Valeur trouvée** | **Chemin dans msinfo32** |
|---|---|---|
| Nom de l'ordinateur | | Résumé du système |
| Système d'exploitation | | Résumé du système |
| Version OS + numéro de build | | Résumé du système |
| Fabricant / Modèle | | Résumé du système |
| Processeur (CPU) | | Résumé du système |
| RAM totale installée | | Résumé du système |
| Type BIOS (Legacy/UEFI) | | Résumé du système |
| Version BIOS | | Résumé du système |

### 1.2 — Avec les commandes WMIC

Ouvrir un **invité de commandes** (`cmd`) et exécuter :

```cmd
:: CPU - noter le modèle exact
wmic cpu get name

:: RAM - noter la capacité (en octets, diviser par 1073741824 pour avoir les Go)
wmic memorychip get capacity,speed

:: Disque dur - noter modèle et taille
wmic diskdrive get model,size,mediatype

:: Numéro de série
wmic bios get serialnumber

:: Réseau - noter IP, masque, GW, DNS, MAC
ipconfig /all
```

Coller les résultats dans un bloc-notes pour les recopier sur la fiche.

### 1.3 — Avec le Gestionnaire de Périphériques

`Clic droit Poste de travail → Gestionnaire de périphériques` :

| **Composant** | **Valeur** |
|---|---|
| Carte réseau Ethernet | |
| Carte réseau WiFi | |
| Carte graphique | |
| Contrôleur de stockage | |

---

## Étape 2 — Inventaire Logiciel (20 min)

### 2.1 — Système d'exploitation

```cmd
winver
```

| **Information** | **Valeur** |
|---|---|
| OS complet avec édition | |
| Version et numéro de build | |
| Type de licence (OEM / Retail / Volume) | |
| Domaine ou groupe de travail | |

### 2.2 — Logiciels Installés

Dans `Paramètres → Applications` ou via `wmic product get name,version` :

Lister au moins **10 logiciels** avec leur version :

| **Logiciel** | **Version** | **Type de licence** |
|---|---|---|
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |

### 2.3 — Mises à jour

```cmd
:: Historique des mises à jour récentes
wmic qfe get hotfixid,installedon | sort /r
```

| **Information** | **Valeur** |
|---|---|
| Dernière mise à jour KB installée | |
| Date d'installation | |
| Antivirus installé | |
| Version de l'antivirus | |

---

## Étape 3 — Configuration Réseau Complète (10 min)

```cmd
ipconfig /all
```

| **Interface** | **Adresse IP** | **Masque** | **Passerelle** | **DNS** | **MAC** |
|---|---|---|---|---|---|
| Ethernet | | | | | |
| WiFi | | | | | |

---

## Étape 4 — (Optionnel) Collecte sous Linux (15 min)

*Si le poste est en dual-boot ou si des VMs Linux sont disponibles :*

```bash
# Lancer ces commandes et noter les résultats
lscpu | grep "Model name"
free -h | grep Mem
lsblk | grep disk
ip addr show | grep -E "inet |link/ether"
sudo dmidecode -s system-serial-number
```

---

## Étape 5 — Remplissage de la Fiche Technique Officielle (15 min)

Transférer toutes les informations collectées dans la **Fiche Technique Officielle** (Annexe 1).

---

## Questions de Réflexion (À répondre par écrit sur la fiche ou dans le portfolio)

**Q1.** Quelle information vous a été la plus difficile à trouver ? Pourquoi ?

**Q2.** Si vous deviez inventorier 200 postes, quelle serait la méthode la plus efficace ? Pourriez-vous écrire un script pour automatiser la collecte ?

**Q3.** Votre poste de TP a-t-il des logiciels installés qui semblent sans rapport avec la formation (jeux, logiciels personnels...) ? Que suggère la bonne pratique de gestion de parc ?

**Q4.** La fiche que vous venez de remplir sera-t-elle encore exacte dans 6 mois ? Qu'est-ce qui peut changer et comment maintenir la fiche à jour ?

---

---

# 📋 ANNEXE 1 — FICHE TECHNIQUE OFFICIELLE (À REMPLIR)

*Modèle standardisé — BTS SIO SISR*

---

## EN-TÊTE

| **Champ** | **Valeur** |
|---|---|
| **N° d'inventaire** | |
| **Date de création** | |
| **Dernière mise à jour** | |
| **Rédigé par** | |
| **Version** | 1.0 |
| **Statut** | ☐ En service  ☐ En maintenance  ☐ Décommissionné |

---

## SECTION 1 — IDENTIFICATION

| **Champ** | **Valeur** |
|---|---|
| Marque | |
| Modèle | |
| Numéro de série | |
| Type | ☐ Poste fixe  ☐ Laptop  ☐ Tout-en-un  ☐ Serveur |
| Date d'achat | |
| Fin de garantie constructeur | |
| Valeur d'achat HT (si connu) | |

---

## SECTION 2 — LOCALISATION ET AFFECTATION

| **Champ** | **Valeur** |
|---|---|
| Localisation physique | |
| Utilisateur affecté | |
| Service | |
| Usage principal | ☐ Bureautique  ☐ Développement  ☐ Graphisme  ☐ Serveur  ☐ TP formation |

---

## SECTION 3 — COMPOSANTS MATÉRIELS

| **Composant** | **Marque / Modèle** | **Caractéristiques** |
|---|---|---|
| **Processeur (CPU)** | | Fréquence : _____ GHz  /  Cœurs : _____ |
| **Mémoire vive (RAM)** | | Capacité : _____ Go  /  Type : _____  /  Fréq. : _____ MHz |
| **Stockage principal** | | Capacité : _____ Go  /  Type : ☐ HDD  ☐ SSD  ☐ NVMe |
| **Stockage secondaire** | | Capacité : _____  /  Type : _____ |
| **Carte graphique (GPU)** | | VRAM : _____ |
| **Carte réseau Ethernet** | | Débit max : _____ Mbps |
| **Carte réseau WiFi** | | Normes : _____ |
| **Carte mère** | | Modèle : _____  /  BIOS v. : _____ |
| **Alimentation** | | Puissance : _____ W |
| **Écran** | | Taille : _____"  /  Résolution : _____ |

---

## SECTION 4 — SYSTÈME D'EXPLOITATION

| **Champ** | **Valeur** |
|---|---|
| OS | |
| Version / Build | |
| Architecture | ☐ 32 bits  ☐ 64 bits |
| Type de licence | ☐ OEM  ☐ Retail  ☐ Volume  ☐ Abonnement |
| Domaine / Groupe de travail | |
| Langue | |

---

## SECTION 5 — LOGICIELS PRINCIPAUX

| **Logiciel** | **Version** | **Éditeur** | **Type de licence** | **Date d'expiration** |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |

---

## SECTION 6 — CONFIGURATION RÉSEAU

| **Interface** | **Adresse IP** | **Masque** | **Passerelle** | **DNS** | **Adresse MAC** |
|---|---|---|---|---|---|
| Ethernet | | | | | |
| WiFi | | | | | |

| **Paramètre** | **Valeur** |
|---|---|
| Mode d'attribution IP | ☐ DHCP automatique  ☐ IP fixe |
| VLAN (si applicable) | |
| Nom DNS | |

---

## SECTION 7 — SÉCURITÉ

| **Champ** | **Valeur** |
|---|---|
| Antivirus | |
| Version antivirus | |
| Dernière mise à jour antivirus | |
| Dernière mise à jour OS | |
| Chiffrement disque | ☐ BitLocker  ☐ VeraCrypt  ☐ Aucun |
| Pare-feu actif | ☐ Oui  ☐ Non |

---

## SECTION 8 — HISTORIQUE DES INTERVENTIONS

| **Date** | **Intervention** | **Technicien** | **Résultat** |
|---|---|---|---|
| | Création de la fiche | | — |
| | | | |
| | | | |

---

## SECTION 9 — OBSERVATIONS

```
_____________________________________________________________________
_____________________________________________________________________
_____________________________________________________________________
```

---

---

# 📋 ANNEXE 2 — EXEMPLE DE FICHE TECHNIQUE COMPLÈTE (Poste Fictif)

*Référence pour la correction et les apprenants débutants*

---

## EN-TÊTE

| **Champ** | **Valeur** |
|---|---|
| **N° d'inventaire** | PC-2024-007 |
| **Date de création** | 15/09/2024 |
| **Dernière mise à jour** | 15/09/2024 |
| **Rédigé par** | J. Dupont — Technicien SI |
| **Version** | 1.0 |
| **Statut** | ✅ En service |

---

## SECTION 1 — IDENTIFICATION

| **Champ** | **Valeur** |
|---|---|
| Marque | Dell |
| Modèle | OptiPlex 7090 |
| Numéro de série | 5CG93BXKL2 |
| Type | ✅ Poste fixe |
| Date d'achat | 14/03/2023 |
| Fin de garantie constructeur | 14/03/2026 |
| Valeur d'achat HT | 720 € |

---

## SECTION 2 — LOCALISATION ET AFFECTATION

| **Champ** | **Valeur** |
|---|---|
| Localisation physique | Bâtiment A — Bureau 214 |
| Utilisateur affecté | Alice Martin |
| Service | Ressources Humaines |
| Usage principal | ✅ Bureautique |

---

## SECTION 3 — COMPOSANTS MATÉRIELS

| **Composant** | **Marque / Modèle** | **Caractéristiques** |
|---|---|---|
| **CPU** | Intel Core i5-10400 | 2,90 GHz (boost 4,30 GHz) / 6 cœurs / 12 threads |
| **RAM** | Samsung DDR4 | 2 × 8 Go = 16 Go total / 2666 MHz |
| **Stockage principal** | Samsung 870 EVO | 512 Go / SSD SATA |
| **Stockage secondaire** | — | — |
| **GPU** | Intel UHD Graphics 630 | Intégré / 1 Go partagé |
| **Carte réseau Eth.** | Intel I219-LM | 1 Gbps |
| **WiFi** | — | Non installé |
| **Carte mère** | Dell 0GDG8Y | BIOS v1.11.0 |
| **Alimentation** | Dell | 260 W |
| **Écran** | Dell P2422H | 24" / 1920×1080 |

---

## SECTION 4 — SYSTÈME D'EXPLOITATION

| **Champ** | **Valeur** |
|---|---|
| OS | Windows 11 Professionnel |
| Version / Build | 23H2 — Build 22631.3737 |
| Architecture | ✅ 64 bits |
| Type de licence | ✅ Volume (Microsoft Open License) |
| Domaine | siosarl.local |
| Langue | Français |

---

## SECTION 5 — LOGICIELS PRINCIPAUX

| **Logiciel** | **Version** | **Éditeur** | **Type licence** | **Expiration** |
|---|---|---|---|---|
| Microsoft 365 Apps | 2308 (build 16731) | Microsoft | SaaS abonnement | 31/08/2025 |
| Adobe Acrobat Reader | 23.006 | Adobe | Freeware | — |
| 7-Zip | 23.01 | Igor Pavlov | Open Source (LGPL) | — |
| Google Chrome | 128.0.6613 | Google | Freeware | — |
| GLPI Agent | 1.5 | GLPI Project | Open Source | — |
| Windows Defender | 4.18.24050 | Microsoft | Inclus Windows | — |

---

## SECTION 6 — CONFIGURATION RÉSEAU

| **Interface** | **Adresse IP** | **Masque** | **Passerelle** | **DNS** | **MAC** |
|---|---|---|---|---|---|
| Ethernet | 192.168.0.52 (DHCP) | /27 | 192.168.0.65 | 192.168.0.145 | 00:1A:2B:3C:4D:5E |

| **Paramètre** | **Valeur** |
|---|---|
| Mode d'attribution IP | ✅ DHCP automatique |
| VLAN | VLAN 10 — RH |
| Nom DNS | pc-alice-rh.siosarl.local |

---

## SECTION 7 — SÉCURITÉ

| **Champ** | **Valeur** |
|---|---|
| Antivirus | Windows Defender |
| Version | 4.18.24050.9 |
| Dernière mise à jour | 15/09/2024 |
| Dernière MAJ OS | 12/09/2024 — KB5043076 |
| Chiffrement disque | ✅ BitLocker activé |
| Pare-feu actif | ✅ Oui (Windows Defender Firewall) |

---

## SECTION 8 — HISTORIQUE

| **Date** | **Intervention** | **Technicien** | **Résultat** |
|---|---|---|---|
| 14/03/2023 | Réception et déploiement initial | J. Dupont | OK |
| 14/03/2023 | Jonction au domaine siosarl.local | J. Dupont | OK |
| 12/09/2024 | Mise à jour Windows KB5043076 | Automatique | OK |
| 15/09/2024 | Création de la fiche technique | J. Dupont | — |

---

*S2 — BTS SIO SISR — Année 1 — Version 1.0*
*Compétences couvertes : B1.1, B1.2, B1.4*
*Première SPS possible du portfolio — Fiche technique du poste de TP*
