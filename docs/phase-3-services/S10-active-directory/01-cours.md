# Cours – Windows Server et Active Directory

*BTS SIO SISR 1re année – Bloc 2 – S10 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S02 (comptes), S04 (adressage), S08 (DNS).
> - **Matériel :** VirtualBox, ISO Windows Server 2022, VM cliente Windows 10/11.

## À la fin de ce cours, tu sauras

- [ ] **Choisir** une édition de Windows Server et **l'installer** en VM ;
- [ ] **Configurer** nom, adresse IP fixe et mises à jour ;
- [ ] **Expliquer** la structure d'Active Directory (forêt, domaine, OU) et ses objets ;
- [ ] **Promouvoir** un contrôleur de domaine et **joindre** un poste ;
- [ ] **Concevoir** et **créer** une structure d'OU, des utilisateurs et des groupes.

---

## FICHE DE COURS ÉLÈVE
### "Windows Server, Active Directory et Structure d'Annuaire"

*Version 1.0 – BTS SIO SISR – *

---

### I. Windows Server : Vue d'Ensemble

#### A. Rôle d'un Serveur Windows

Un **serveur** est un ordinateur (physique ou virtuel) qui fournit des **services** à d'autres machines (les clients). Windows Server est le système d'exploitation de Microsoft conçu pour ce rôle.

**Services typiques d'un Windows Server :**

| **Service** | **Rôle Windows Server** | **Fonction** |
|-------------|------------------------|--------------|
| Annuaire réseau | Active Directory Domain Services (AD DS) | Centralise les comptes, authentification |
| Attribution d'IP | DHCP Server | Distribue les configurations réseau |
| Résolution de noms | DNS Server | Traduit noms ↔ IP |
| Partage de fichiers | File and Storage Services | Partages réseau |
| Impression réseau | Print and Document Services | Imprimantes partagées |
| Web intranet | IIS (Internet Information Services) | Sites web internes |
| VPN | Routing and Remote Access | Accès distant sécurisé |
| Mises à jour | WSUS | Distribution des MAJ Windows en interne |

---

#### B. Éditions de Windows Server

| **Édition** | **Cible** | **Particularité** |
|-------------|-----------|-------------------|
| **Essentials** | TPE (≤25 utilisateurs, ≤50 appareils) | AD intégré, pas de CAL requises |
| **Standard** | PME, environnements non intensifs | 2 VMs incluses avec virtualisation |
| **Datacenter** | Grandes entreprises, cloud, virtualisation intensive | VMs illimitées, toutes fonctionnalités |
| **Azure Edition** | Cloud hybride Microsoft Azure | Fonctionnalités cloud-native |

**Pour les TP BTS SIO :** Windows Server 2022 **Evaluation** (180 jours, gratuit, fonctionnellement identique à Standard).

---

#### C. Modes d'Installation : Core vs Desktop Experience

| **Mode** | **Interface** | **RAM** | **Usage** |
|----------|---------------|---------|-----------|
| **Server Core** | CLI uniquement (pas de bureau) | ~1 Go | Production (sécurité renforcée) |
| **Desktop Experience** | Avec interface graphique (bureau classique) | ~2 Go | Apprentissage, administration visuelle |

**En TP : Desktop Experience** — interface graphique, plus accessible.

---

#### D. Post-Installation Indispensable

Après installation, **avant** tout rôle, configurer :

1. **Nom d'hôte** : `SRV-AD-TECHPRO` (descriptif, court, sans espaces)
   - *Paramètres* → *Système* → *Renommer ce PC*

2. **Adresse IP fixe** : Obligatoire pour un serveur (AD, DNS, DHCP ne fonctionnent pas bien avec DHCP)
   - IPv4 statique, masque, passerelle, **DNS pointant sur lui-même**

3. **Mises à jour Windows** : Effectuer les MAJ critiques avant la promotion DC

4. **Paramètres régionaux** : Langue, fuseau horaire, clavier (souvent en QWERTY par défaut)

---

#### E. Le Gestionnaire de Serveur (Server Manager)

L'interface centrale d'administration de Windows Server. Lancé automatiquement au démarrage.

**Onglets principaux :**
- **Tableau de bord** : Vue d'ensemble de l'état des rôles
- **Serveur local** : Paramètres du serveur (nom, IP, pare-feu, WER…)
- **Tous les serveurs** : Gérer plusieurs serveurs depuis une seule console
- **Gérer → Ajouter des rôles et fonctionnalités** : Installer les rôles

---

### II. Active Directory : Concepts Fondamentaux

#### A. Pourquoi Active Directory ?

**Sans AD (groupe de travail / workgroup) :**
- Chaque PC a ses propres comptes locaux
- 50 PCs = 50 × comptes à créer/gérer
- Mot de passe changé sur un PC ≠ changé sur les autres
- Pas de politique centralisée (longueur MDP, verrouillage…)

**Avec AD (domaine) :**
- Un serveur centralisé gère **tous** les comptes
- Un compte unique pour tous les PCs du domaine
- Administration centralisée des politiques de sécurité
- Authentification unique (SSO – Single Sign-On)

---

#### B. La Hiérarchie AD : Forêt, Arbre, Domaine

<figure class="graphe diagramme"><svg viewBox="0 0 700 330" style="max-width:700px" role="img" aria-label="Forêt, arbre et domaines Active Directory" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="10" width="680" height="310" rx="12" class="dg-soft"/>
<text x="30" y="34" text-anchor="start" class="dg-t">FORÊT (Forest)</text>
<text x="30" y="52" text-anchor="start" class="dg-s">ensemble de domaines partageant le même schéma AD - périmètre de sécurité ultime</text>
<rect x="30" y="68" width="400" height="236" rx="10" class="dg-box"/>
<text x="46" y="92" text-anchor="start" class="dg-t">ARBRE (Tree)</text>
<text x="46" y="110" text-anchor="start" class="dg-s">domaines partageant un espace DNS</text>
<rect x="110" y="126" width="240" height="50" rx="8" class="dg-ok"/>
<text x="230.0" y="147.0" text-anchor="middle" class="dg-t">Domaine racine (Root Domain)</text>
<text x="230.0" y="165.0" text-anchor="middle" class="dg-s">techpro.local</text>
<rect x="110" y="226" width="240" height="50" rx="8" class="dg-ok"/>
<text x="230.0" y="247.0" text-anchor="middle" class="dg-t">Domaine enfant</text>
<text x="230.0" y="265.0" text-anchor="middle" class="dg-s">paris.techpro.local</text>
<line x1="230" y1="176" x2="230" y2="226" class="dg-line"/>
<rect x="460" y="126" width="210" height="70" rx="8" class="dg-warn"/>
<text x="565.0" y="148.0" text-anchor="middle" class="dg-t">Autre arbre</text>
<text x="565.0" y="166.0" text-anchor="middle" class="dg-s">filiale2.com</text>
<text x="565.0" y="184.0" text-anchor="middle" class="dg-s">même forêt, espace DNS différent</text>
</svg>
</figure>

**Pour 99% des PME : 1 forêt = 1 arbre = 1 domaine.** La complexité multi-domaines concerne les grandes organisations.

---

#### C. Les Unités Organisationnelles (OU)

Une **OU** (Organizational Unit) est un **conteneur** dans un domaine AD. Elle sert à :
- **Organiser** les objets (utilisateurs, ordinateurs, groupes) selon la structure de l'entreprise
- **Déléguer** l'administration (ex : le responsable RH administre l'OU RH)
- **Appliquer des GPO** (stratégies de groupe) à un périmètre précis

**Analogie :** Une OU est un **dossier** dans l'explorateur Windows. On peut imbriquer des OU dans des OU (sous-dossiers).

**Ce qu'une OU N'EST PAS :**
- Une OU n'est **pas** un groupe de sécurité : les membres d'une OU n'héritent pas automatiquement des mêmes droits
- Une OU ne peut **pas** être utilisée directement pour les permissions NTFS (seuls les groupes le peuvent)

**Structure OU recommandée :**

<figure class="graphe diagramme"><svg viewBox="0 0 292 552" style="max-width:292px" role="img" aria-label="Arborescence des unités d&#x27;organisation du domaine techpro.local" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="293" class="dg-line"/>
<line x1="26" y1="293" x2="50" y2="293" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="429" class="dg-line"/>
<line x1="26" y1="429" x2="50" y2="429" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="123" class="dg-line"/>
<line x1="64" y1="123" x2="88" y2="123" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="225" class="dg-line"/>
<line x1="64" y1="225" x2="88" y2="225" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="259" class="dg-line"/>
<line x1="64" y1="259" x2="88" y2="259" class="dg-line"/>
<line x1="102" y1="136" x2="102" y2="157" class="dg-line"/>
<line x1="102" y1="157" x2="126" y2="157" class="dg-line"/>
<line x1="102" y1="136" x2="102" y2="191" class="dg-line"/>
<line x1="102" y1="191" x2="126" y2="191" class="dg-line"/>
<line x1="64" y1="306" x2="64" y2="327" class="dg-line"/>
<line x1="64" y1="327" x2="88" y2="327" class="dg-line"/>
<line x1="64" y1="306" x2="64" y2="361" class="dg-line"/>
<line x1="64" y1="361" x2="88" y2="361" class="dg-line"/>
<line x1="64" y1="306" x2="64" y2="395" class="dg-line"/>
<line x1="64" y1="395" x2="88" y2="395" class="dg-line"/>
<line x1="64" y1="442" x2="64" y2="463" class="dg-line"/>
<line x1="64" y1="463" x2="88" y2="463" class="dg-line"/>
<line x1="64" y1="442" x2="64" y2="497" class="dg-line"/>
<line x1="64" y1="497" x2="88" y2="497" class="dg-line"/>
<line x1="64" y1="442" x2="64" y2="531" class="dg-line"/>
<line x1="64" y1="531" x2="88" y2="531" class="dg-line"/>
<rect x="12" y="8" width="130" height="26" rx="6" class="dg-dark"/>
<text x="77.0" y="26" text-anchor="middle" class="dg-w">techpro.local</text>
<rect x="50" y="42" width="146" height="26" rx="6" class="dg-box"/>
<text x="123.0" y="60" text-anchor="middle" class="dg-t">OU_UTILISATEURS</text>
<rect x="88" y="76" width="122" height="26" rx="6" class="dg-box"/>
<text x="149.0" y="94" text-anchor="middle" class="dg-t">OU Direction</text>
<rect x="88" y="110" width="146" height="26" rx="6" class="dg-box"/>
<text x="161.0" y="128" text-anchor="middle" class="dg-t">OU Informatique</text>
<rect x="126" y="144" width="98" height="26" rx="6" class="dg-box"/>
<text x="175.0" y="162" text-anchor="middle" class="dg-t">OU Réseau</text>
<rect x="126" y="178" width="154" height="26" rx="6" class="dg-box"/>
<text x="203.0" y="196" text-anchor="middle" class="dg-t">OU Développement</text>
<rect x="88" y="212" width="130" height="26" rx="6" class="dg-box"/>
<text x="153.0" y="230" text-anchor="middle" class="dg-t">OU Commercial</text>
<rect x="88" y="246" width="66" height="26" rx="6" class="dg-box"/>
<text x="121.0" y="264" text-anchor="middle" class="dg-t">OU RH</text>
<rect x="50" y="280" width="138" height="26" rx="6" class="dg-box"/>
<text x="119.0" y="298" text-anchor="middle" class="dg-t">OU_ORDINATEURS</text>
<rect x="88" y="314" width="151" height="26" rx="6" class="dg-soft"/>
<text x="163.7" y="332" text-anchor="middle" class="dg-s">OU Postes-Direction</text>
<rect x="88" y="348" width="171" height="26" rx="6" class="dg-soft"/>
<text x="173.6" y="366" text-anchor="middle" class="dg-s">OU Postes-Informatique</text>
<rect x="88" y="382" width="158" height="26" rx="6" class="dg-soft"/>
<text x="167.0" y="400" text-anchor="middle" class="dg-s">OU Postes-Commercial</text>
<rect x="50" y="416" width="106" height="26" rx="6" class="dg-box"/>
<text x="103.0" y="434" text-anchor="middle" class="dg-t">OU_GROUPES</text>
<rect x="88" y="450" width="130" height="26" rx="6" class="dg-ok"/>
<text x="153.0" y="468" text-anchor="middle" class="dg-t">GRP_Direction</text>
<rect x="88" y="484" width="154" height="26" rx="6" class="dg-ok"/>
<text x="165.0" y="502" text-anchor="middle" class="dg-t">GRP_Informatique</text>
<rect x="88" y="518" width="138" height="26" rx="6" class="dg-ok"/>
<text x="157.0" y="536" text-anchor="middle" class="dg-t">GRP_Commercial</text>
</svg>
<figcaption>Le domaine est découpé en trois OU principales : utilisateurs, ordinateurs et groupes.</figcaption>
</figure>

**Bonne pratique :** Séparer les utilisateurs, les ordinateurs et les groupes dans des OU distinctes, même si c'est plus verbeux. Cela facilite l'application des GPO.

---

#### D. Les Objets Active Directory

| **Objet** | **Icône MMC** | **Description** | **Attributs principaux** |
|-----------|---------------|-----------------|--------------------------|
| **Utilisateur** (User) |  | Compte d'une personne | SAMAccountName, UPN, mot de passe, service, email… |
| **Ordinateur** (Computer) |  | Poste ou serveur joint au domaine | Nom, OS, date de dernière connexion |
| **Groupe** (Group) |  | Regroupement d'utilisateurs et/ou d'ordinateurs | Type (sécurité/distribution), étendue (global/domain local/universel) |
| **OU** |  | Conteneur pour organiser les objets | Nom, GPO liées |
| **Contact** |  | Personne externe (pas de compte) | Email, téléphone |
| **Imprimante partagée** |  | Imprimante publiée dans l'annuaire | Nom, localisation |

---

#### E. Le Compte Utilisateur AD

Le compte AD d'un utilisateur contient :

<figure class="graphe diagramme"><svg viewBox="0 0 497 620" style="max-width:497px" role="img" aria-label="Attributs d&#x27;un objet utilisateur Active Directory" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="259" class="dg-line"/>
<line x1="26" y1="259" x2="50" y2="259" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="395" class="dg-line"/>
<line x1="26" y1="395" x2="50" y2="395" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="531" class="dg-line"/>
<line x1="26" y1="531" x2="50" y2="531" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="123" class="dg-line"/>
<line x1="64" y1="123" x2="88" y2="123" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="157" class="dg-line"/>
<line x1="64" y1="157" x2="88" y2="157" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="191" class="dg-line"/>
<line x1="64" y1="191" x2="88" y2="191" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="225" class="dg-line"/>
<line x1="64" y1="225" x2="88" y2="225" class="dg-line"/>
<line x1="64" y1="272" x2="64" y2="293" class="dg-line"/>
<line x1="64" y1="293" x2="88" y2="293" class="dg-line"/>
<line x1="64" y1="272" x2="64" y2="327" class="dg-line"/>
<line x1="64" y1="327" x2="88" y2="327" class="dg-line"/>
<line x1="64" y1="272" x2="64" y2="361" class="dg-line"/>
<line x1="64" y1="361" x2="88" y2="361" class="dg-line"/>
<line x1="64" y1="408" x2="64" y2="429" class="dg-line"/>
<line x1="64" y1="429" x2="88" y2="429" class="dg-line"/>
<line x1="64" y1="408" x2="64" y2="463" class="dg-line"/>
<line x1="64" y1="463" x2="88" y2="463" class="dg-line"/>
<line x1="64" y1="408" x2="64" y2="497" class="dg-line"/>
<line x1="64" y1="497" x2="88" y2="497" class="dg-line"/>
<line x1="64" y1="544" x2="64" y2="565" class="dg-line"/>
<line x1="64" y1="565" x2="88" y2="565" class="dg-line"/>
<line x1="64" y1="544" x2="64" y2="599" class="dg-line"/>
<line x1="64" y1="599" x2="88" y2="599" class="dg-line"/>
<rect x="12" y="8" width="274" height="26" rx="6" class="dg-dark"/>
<text x="149.0" y="26" text-anchor="middle" class="dg-w">Objet Utilisateur : Jean Martin</text>
<rect x="50" y="42" width="90" height="26" rx="6" class="dg-box"/>
<text x="95.0" y="60" text-anchor="middle" class="dg-t">Identité</text>
<rect x="88" y="76" width="112" height="26" rx="6" class="dg-soft"/>
<text x="143.9" y="94" text-anchor="middle" class="dg-s">Prénom : Jean</text>
<rect x="88" y="110" width="105" height="26" rx="6" class="dg-soft"/>
<text x="140.6" y="128" text-anchor="middle" class="dg-s">Nom : Martin</text>
<rect x="88" y="144" width="191" height="26" rx="6" class="dg-soft"/>
<text x="183.5" y="162" text-anchor="middle" class="dg-s">Nom complet : Jean Martin</text>
<rect x="88" y="178" width="191" height="26" rx="6" class="dg-soft"/>
<text x="183.5" y="196" text-anchor="middle" class="dg-s">SAMAccountName : j.martin</text>
<text x="289.0" y="196" text-anchor="start" class="dg-i">login réseau (TECHPRO\j.martin)</text>
<rect x="88" y="212" width="211" height="26" rx="6" class="dg-soft"/>
<text x="193.39999999999998" y="230" text-anchor="middle" class="dg-s">UPN : j.martin@techpro.local</text>
<text x="308.79999999999995" y="230" text-anchor="start" class="dg-i">login au format e-mail</text>
<rect x="50" y="246" width="154" height="26" rx="6" class="dg-box"/>
<text x="127.0" y="264" text-anchor="middle" class="dg-t">Authentification</text>
<rect x="88" y="280" width="270" height="26" rx="6" class="dg-soft"/>
<text x="223.1" y="298" text-anchor="middle" class="dg-s">Mot de passe (haché, jamais en clair)</text>
<rect x="88" y="314" width="270" height="26" rx="6" class="dg-soft"/>
<text x="223.1" y="332" text-anchor="middle" class="dg-s">Expiration du mot de passe : 90 jours</text>
<rect x="88" y="348" width="277" height="26" rx="6" class="dg-soft"/>
<text x="226.39999999999998" y="366" text-anchor="middle" class="dg-s">Compte activé / désactivé / verrouillé</text>
<rect x="50" y="382" width="74" height="26" rx="6" class="dg-box"/>
<text x="87.0" y="400" text-anchor="middle" class="dg-t">Profil</text>
<rect x="88" y="416" width="323" height="26" rx="6" class="dg-soft"/>
<text x="249.5" y="434" text-anchor="middle" class="dg-s">Chemin du profil : \\SRV-AD\Profiles\j.martin</text>
<rect x="88" y="450" width="231" height="26" rx="6" class="dg-soft"/>
<text x="203.3" y="468" text-anchor="middle" class="dg-s">Script de connexion : logon.bat</text>
<rect x="88" y="484" width="343" height="26" rx="6" class="dg-soft"/>
<text x="259.4" y="502" text-anchor="middle" class="dg-s">Lecteur réseau : H: vers \\SRV-AD\Users\j.martin</text>
<rect x="50" y="518" width="218" height="26" rx="6" class="dg-box"/>
<text x="159.0" y="536" text-anchor="middle" class="dg-t">Appartenance aux groupes</text>
<rect x="88" y="552" width="154" height="26" rx="6" class="dg-ok"/>
<text x="165.0" y="570" text-anchor="middle" class="dg-t">GRP_Informatique</text>
<rect x="88" y="586" width="130" height="26" rx="6" class="dg-ok"/>
<text x="153.0" y="604" text-anchor="middle" class="dg-t">GRP_All_Users</text>
</svg>
<figcaption>Un compte utilisateur regroupe identité, authentification, profil et appartenance aux groupes.</figcaption>
</figure>

**SAMAccountName :** Limité à 20 caractères. Convention courante : `p.nom` (première lettre prénom + point + nom). Doit être unique dans le domaine.

**UPN (User Principal Name) :** Format email (`login@domaine`). Utilisé pour l'authentification dans les applications modernes.

---

#### F. Les Groupes Active Directory

Les groupes AD servent à **regrouper des utilisateurs** pour leur attribuer des droits collectivement.

##### Type de Groupe

| **Type** | **Usage** |
|----------|-----------|
| **Sécurité** | Attribuer des droits/permissions (NTFS, partages, GPO) |
| **Distribution** | Listes de diffusion email (Exchange/Outlook) uniquement |

**→ En BTS SIO SISR : presque toujours des groupes de sécurité.**

##### Étendue du Groupe

| **Étendue** | **Membres possibles** | **Peut être utilisé pour les droits dans…** | **Cas d'usage** |
|-------------|----------------------|----------------------------------------------|-----------------|
| **Domaine Local** | Utilisateurs, groupes globaux, groupes universels du domaine ou d'autres domaines | Le domaine local uniquement | Attribuer des droits sur des ressources locales |
| **Global** | Utilisateurs du même domaine uniquement | N'importe quel domaine de la forêt | Regrouper des utilisateurs par rôle/service |
| **Universel** | Utilisateurs et groupes de n'importe quel domaine | N'importe quel domaine de la forêt | Multi-domaines (grandes organisations) |

**Règle simple pour un domaine unique (99% des cas PME) :**
- Créer des **groupes de sécurité globaux** (`GRP_Informatique`, `GRP_Commercial`…)
- Ajouter les utilisateurs dans ces groupes
- Attribuer les droits aux groupes (et non aux utilisateurs directement)

---

#### G. Nommer les Objets AD : Conventions

Une bonne convention de nommage évite les confusions et facilite l'administration.

**Conventions courantes :**

| **Type d'objet** | **Préfixe** | **Exemple** |
|-----------------|-------------|-------------|
| OU Utilisateurs | `OU_` | `OU_Informatique` |
| OU Ordinateurs | `PC_` | `PC_Informatique` |
| Groupes de sécurité | `GRP_` | `GRP_Informatique` |
| Groupes de distribution | `DL_` | `DL_Commercial` |
| SAMAccountName utilisateur | `p.nom` | `j.martin`, `e.petit` |
| Nom d'hôte serveur | `SRV-` | `SRV-AD-01`, `SRV-FICHIER` |
| Nom d'hôte poste client | `PC-` | `PC-COM-01`, `PC-INFO-05` |

---

#### H. La Console ADUC (Active Directory Users and Computers)

La console MMC principale pour administrer AD.

**Accès :**
- `Démarrer` → `Outils d'administration Windows` → `Utilisateurs et ordinateurs Active Directory`
- Ou `dsa.msc` dans Exécuter

**Actions principales (clic droit dans ADUC) :**

| **Action** | **Clic droit sur…** | **Menu** |
|------------|---------------------|----------|
| Créer une OU | Le domaine ou une OU | Nouveau → Unité d'organisation |
| Créer un utilisateur | Une OU | Nouveau → Utilisateur |
| Créer un groupe | Une OU | Nouveau → Groupe |
| Déplacer un objet | L'objet | Déplacer… |
| Réinitialiser MDP | Un utilisateur | Réinitialiser le mot de passe |
| Désactiver un compte | Un utilisateur | Désactiver le compte |
| Ajouter au groupe | Un utilisateur | Ajouter à un groupe |
| Propriétés complètes | N'importe quel objet | Propriétés |

---

### III. Tableau de Correspondance Complet

| **Concept entreprise** | **Objet AD** | **Commande / Console** |
|------------------------|--------------|------------------------|
| Entreprise | Domaine | `dcpromo` / Server Manager |
| Service/Département | OU | ADUC → Nouveau → OU |
| Employé | Utilisateur | ADUC → Nouveau → Utilisateur |
| PC professionnel | Ordinateur | Joint au domaine depuis le PC |
| Équipe/rôle | Groupe (global, sécurité) | ADUC → Nouveau → Groupe |
| Règle de sécurité | GPO (Group Policy Object) | GPMC – S11 |
| Administrateur délégué | Délégation de contrôle | ADUC → Déléguer le contrôle |

---

### IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **Active Directory (AD)** | Service d'annuaire de Microsoft – centralise l'authentification et la gestion des ressources réseau |
| **Contrôleur de domaine (DC)** | Serveur Windows hébergeant la base de données AD et gérant l'authentification |
| **Domaine** | Unité administrative AD identifiée par un nom DNS (ex: techpro.local) |
| **Forêt (Forest)** | Ensemble de domaines partageant un schéma AD commun – périmètre de sécurité ultime |
| **Arbre (Tree)** | Ensemble de domaines partageant un espace de noms DNS continu |
| **OU** | Unité Organisationnelle – conteneur AD pour organiser les objets et appliquer des GPO |
| **Utilisateur (User)** | Objet AD représentant le compte d'une personne |
| **Ordinateur (Computer)** | Objet AD représentant un PC ou serveur joint au domaine |
| **Groupe (Group)** | Objet AD regroupant des utilisateurs/ordinateurs pour leur attribuer des droits collectivement |
| **GPO** | Group Policy Object – stratégie de groupe appliquée à une OU (S11) |
| **SAMAccountName** | Identifiant court de connexion réseau (ex: j.martin) – max 20 caractères |
| **UPN** | User Principal Name – identifiant format email (j.martin@techpro.local) |
| **DSRM** | Directory Services Restore Mode – mode de récupération du DC, protégé par un mot de passe spécifique |
| **Domaine local** | Étendue de groupe : droits applicables dans le domaine local uniquement |
| **Groupe global** | Étendue de groupe : membres du domaine local, droits dans toute la forêt |
| **Promotion DC** | Opération qui transforme un Windows Server en contrôleur de domaine |
| **AD DS** | Active Directory Domain Services – rôle Windows Server hébergeant l'annuaire |
| **ADUC** | Active Directory Users and Computers – console MMC principale d'administration AD |
| **dsa.msc** | Commande pour ouvrir la console ADUC |
| **Niveau fonctionnel** | Version des fonctionnalités AD disponibles (lié à la version des DC du domaine) |

---

### V. Exercices d'Entraînement

#### Exercice 1 : Identifier les Objets AD

Pour chaque élément, dire s'il s'agit d'un Utilisateur, Ordinateur, Groupe, OU ou Domaine :

**1.** `GRP_Comptabilité` → ?

<details markdown="1">
<summary>Correction de la question 1</summary>

**Groupe** (préfixe `GRP_`) : il sert à regrouper des utilisateurs pour leur attribuer des droits.

</details>

**2.** `PC-FIN-08` (machine jointe au domaine) → ?

<details markdown="1">
<summary>Correction de la question 2</summary>

**Ordinateur** : une machine jointe au domaine a son propre compte dans l'annuaire.

</details>

**3.** `Service Financier` (conteneur dans AD) → ?

<details markdown="1">
<summary>Correction de la question 3</summary>

**OU** (unité d'organisation) : un conteneur qui range des objets et reçoit des GPO.

</details>

**4.** `marie.dupont` (login réseau) → ?

<details markdown="1">
<summary>Correction de la question 4</summary>

**Utilisateur** : c'est son identifiant de connexion (SAMAccountName).

</details>

**5.** `finance.entreprise.local` → ?

<details markdown="1">
<summary>Correction de la question 5</summary>

**Domaine** (ici un sous-domaine de `entreprise.local`).

</details>

**6.** `DL_Newsletter_Clients` → ?

<details markdown="1">
<summary>Correction de la question 6</summary>

**Groupe de distribution** : selon la convention de nommage du cours, `DL_` désigne un groupe de distribution, c'est-à-dire une liste de diffusion de courriels, sans droits d'accès.

</details>

**7.** `SRV-FILE-01` (serveur de fichiers joint) → ?

<details markdown="1">
<summary>Correction de la question 7</summary>

**Ordinateur** : un serveur membre du domaine est un compte ordinateur comme un poste de travail.

</details>

---

#### Exercice 2 : Concevoir une Structure OU

L'entreprise **BioMed SARL** (cabinet médical) a la structure suivante :

<figure class="graphe diagramme"><svg viewBox="0 0 398 416" style="max-width:398px" role="img" aria-label="Organigramme de BioMed SARL" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="157" class="dg-line"/>
<line x1="26" y1="157" x2="50" y2="157" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="259" class="dg-line"/>
<line x1="26" y1="259" x2="50" y2="259" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="361" class="dg-line"/>
<line x1="26" y1="361" x2="50" y2="361" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="123" class="dg-line"/>
<line x1="64" y1="123" x2="88" y2="123" class="dg-line"/>
<line x1="64" y1="170" x2="64" y2="191" class="dg-line"/>
<line x1="64" y1="191" x2="88" y2="191" class="dg-line"/>
<line x1="64" y1="170" x2="64" y2="225" class="dg-line"/>
<line x1="64" y1="225" x2="88" y2="225" class="dg-line"/>
<line x1="64" y1="272" x2="64" y2="293" class="dg-line"/>
<line x1="64" y1="293" x2="88" y2="293" class="dg-line"/>
<line x1="64" y1="272" x2="64" y2="327" class="dg-line"/>
<line x1="64" y1="327" x2="88" y2="327" class="dg-line"/>
<line x1="64" y1="374" x2="64" y2="395" class="dg-line"/>
<line x1="64" y1="395" x2="88" y2="395" class="dg-line"/>
<rect x="12" y="8" width="234" height="26" rx="6" class="dg-dark"/>
<text x="129.0" y="26" text-anchor="middle" class="dg-w">BioMed SARL (50 personnes)</text>
<rect x="50" y="42" width="170" height="26" rx="6" class="dg-box"/>
<text x="135.0" y="60" text-anchor="middle" class="dg-t">Direction Médicale</text>
<rect x="88" y="76" width="298" height="26" rx="6" class="dg-warn"/>
<text x="237.0" y="94" text-anchor="middle" class="dg-t">Dr. Lefebvre (médecin généraliste)</text>
<rect x="88" y="110" width="234" height="26" rx="6" class="dg-warn"/>
<text x="205.0" y="128" text-anchor="middle" class="dg-t">Dr. Rousseau (cardiologue)</text>
<rect x="50" y="144" width="138" height="26" rx="6" class="dg-box"/>
<text x="119.0" y="162" text-anchor="middle" class="dg-t">Administration</text>
<rect x="88" y="178" width="186" height="26" rx="6" class="dg-warn"/>
<text x="181.0" y="196" text-anchor="middle" class="dg-t">Claire (secrétariat)</text>
<rect x="88" y="212" width="178" height="26" rx="6" class="dg-warn"/>
<text x="177.0" y="230" text-anchor="middle" class="dg-t">Marc (comptabilité)</text>
<rect x="50" y="246" width="106" height="26" rx="6" class="dg-box"/>
<text x="103.0" y="264" text-anchor="middle" class="dg-t">Infirmerie</text>
<rect x="88" y="280" width="162" height="26" rx="6" class="dg-warn"/>
<text x="169.0" y="298" text-anchor="middle" class="dg-t">Anne (infirmière)</text>
<rect x="88" y="314" width="170" height="26" rx="6" class="dg-warn"/>
<text x="173.0" y="332" text-anchor="middle" class="dg-t">Pierre (infirmier)</text>
<rect x="50" y="348" width="122" height="26" rx="6" class="dg-box"/>
<text x="111.0" y="366" text-anchor="middle" class="dg-t">Informatique</text>
<rect x="88" y="382" width="202" height="26" rx="6" class="dg-warn"/>
<text x="189.0" y="400" text-anchor="middle" class="dg-t">Thomas (informaticien)</text>
</svg>
<figcaption>Les services de l&#x27;entreprise serviront de base aux unités d&#x27;organisation.</figcaption>
</figure>

**1.** Proposer la structure OU complète pour ce domaine

<details markdown="1">
<summary>Correction de la question 1</summary>

Une OU par service, avec à l'intérieur des sous-OU qui séparent utilisateurs, ordinateurs et groupes (pratique qui facilite l'application des GPO) :

<figure class="graphe diagramme"><svg viewBox="0 0 425 212" style="max-width:425px" role="img" aria-label="Structure Active Directory proposée pour biomed.local" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="89" class="dg-line"/>
<line x1="26" y1="89" x2="50" y2="89" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="123" class="dg-line"/>
<line x1="26" y1="123" x2="50" y2="123" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="157" class="dg-line"/>
<line x1="26" y1="157" x2="50" y2="157" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="191" class="dg-line"/>
<line x1="26" y1="191" x2="50" y2="191" class="dg-line"/>
<rect x="12" y="8" width="122" height="26" rx="6" class="dg-dark"/>
<text x="73.0" y="26" text-anchor="middle" class="dg-w">biomed.local</text>
<rect x="50" y="42" width="194" height="26" rx="6" class="dg-box"/>
<text x="147.0" y="60" text-anchor="middle" class="dg-t">OU_Direction_Medicale</text>
<text x="254.0" y="60" text-anchor="start" class="dg-i">Utilisateurs, Ordinateurs</text>
<rect x="50" y="76" width="162" height="26" rx="6" class="dg-box"/>
<text x="131.0" y="94" text-anchor="middle" class="dg-t">OU_Administration</text>
<text x="222.0" y="94" text-anchor="start" class="dg-i">Utilisateurs, Ordinateurs</text>
<rect x="50" y="110" width="130" height="26" rx="6" class="dg-box"/>
<text x="115.0" y="128" text-anchor="middle" class="dg-t">OU_Infirmerie</text>
<text x="190.0" y="128" text-anchor="start" class="dg-i">Utilisateurs, Ordinateurs</text>
<rect x="50" y="144" width="146" height="26" rx="6" class="dg-box"/>
<text x="123.0" y="162" text-anchor="middle" class="dg-t">OU_Informatique</text>
<text x="206.0" y="162" text-anchor="start" class="dg-i">Utilisateurs, Ordinateurs</text>
<rect x="50" y="178" width="106" height="26" rx="6" class="dg-ok"/>
<text x="103.0" y="196" text-anchor="middle" class="dg-t">OU_Groupes</text>
<text x="166.0" y="196" text-anchor="start" class="dg-i">groupes de sécurité et de distribution</text>
</svg>
<figcaption>Une OU par service, plus une OU pour les groupes.</figcaption>
</figure>

La structure reprend l'**organisation de l'entreprise** : elle sert à déléguer l'administration et à appliquer des GPO par service. Elle est distincte des groupes, qui servent aux droits d'accès.

</details>

**2.** Proposer les SAMAccountName pour chaque utilisateur

<details markdown="1">
<summary>Correction de la question 2</summary>

Convention courante : `prénom.nom` en minuscules, sans accent ni espace, 20 caractères au maximum. L'énoncé ne donne pas tous les noms de famille : avec les informations disponibles, `lefebvre`, `rousseau`, `claire`, `marc`, `anne`, `pierre`, `thomas`. Avec les noms complets on aurait par exemple `claire.martin`. L'UPN correspondant serait `claire.martin@biomed.local`.

</details>

**3.** Proposer 3 groupes pertinents avec leur type et étendue

<details markdown="1">
<summary>Correction de la question 3</summary>

| **Groupe** | **Type** | **Étendue** | **Rôle** |
|------------|----------|-------------|----------|
| `GG_Medical` | Sécurité | Globale | Regroupe les médecins et le personnel infirmier |
| `DL_Dossiers_Patients_RW` | Sécurité | Domaine local | Porte le droit de modification sur le partage des dossiers patients ; contient `GG_Medical` |
| `DL_Personnel` | Distribution | Globale | Liste de diffusion des courriels destinés à tout le personnel |

Principe **AGDLP** : les comptes (**A**) vont dans des groupes globaux (**G**), qui vont dans des groupes de domaine local (**DL**), auxquels on donne les droits (**P**) sur la ressource.

</details>

**4.** Quel nom de domaine suggérer ? (justifier)

<details markdown="1">
<summary>Correction de la question 4</summary>

Pour le TP, **`biomed.local`** (nom NetBIOS `BIOMED`) : simple et cohérent avec le reste du cours. En production, on préfère un sous-domaine d'un nom de domaine que l'entreprise possède, par exemple `ad.biomed-sarl.fr` : `.local` est réservé à la résolution multicast (mDNS) et peut provoquer des conflits, alors qu'un nom public évite les collisions et permet d'obtenir des certificats.

</details>

---

#### Exercice 3 : Questions de Cours

**1.** Quelle est la différence entre un groupe de sécurité et un groupe de distribution ?

<details markdown="1">
<summary>Correction de la question 1</summary>

Un groupe de **sécurité** peut recevoir des permissions (droits sur des dossiers, filtrage de GPO) et servir de liste de diffusion. Un groupe de **distribution** ne sert qu'à envoyer des courriels : il n'est pas utilisable dans les permissions.

</details>

**2.** Pourquoi faut-il une IP fixe sur le serveur AVANT la promotion DC ?

<details markdown="1">
<summary>Correction de la question 2</summary>

Le contrôleur de domaine est aussi le **serveur DNS** : les clients le trouvent par son adresse et par les enregistrements SRV qu'il publie. Si son adresse changeait (DHCP), ils ne le joindraient plus. L'assistant de promotion refuse ou avertit d'une configuration en DHCP. Le DC doit aussi utiliser lui-même son DNS.

</details>

**3.** Peut-on appliquer une GPO directement sur un groupe de sécurité ? Et sur une OU ?

<details markdown="1">
<summary>Correction de la question 3</summary>

Une GPO se **lie** à un site, un domaine ou une **OU**, pas à un groupe. On peut en revanche **filtrer** son application avec un groupe de sécurité (filtrage de sécurité) : seuls les membres du groupe reçoivent la GPO liée à l'OU.

</details>

**4.** Quelle est la différence entre SAMAccountName et UPN ?

<details markdown="1">
<summary>Correction de la question 4</summary>

Le **SAMAccountName** est l'ancien identifiant de connexion (`BIOMED\claire.martin`), limité à 20 caractères et unique dans le domaine. L'**UPN** (User Principal Name) a la forme d'une adresse (`claire.martin@biomed.local`), est unique dans la forêt et peut être différent du nom de domaine, par exemple l'adresse de messagerie.

</details>

**5.** Qu'est-ce que le mot de passe DSRM et à quoi sert-il ?

<details markdown="1">
<summary>Correction de la question 5</summary>

DSRM (*Directory Services Restore Mode*) est le mot de passe de l'**administrateur local du contrôleur de domaine**, défini lors de la promotion. Il permet de démarrer le DC en mode restauration pour réparer ou restaurer la base Active Directory. Il doit être conservé en lieu sûr.

</details>

---

### VI. Auto-évaluation : Suis-je Prêt ?

- [ ] Nommer les 3 éditions principales de Windows Server et leur cible
- [ ] Expliquer la différence Server Core vs Desktop Experience
- [ ] Lister les 4 paramètres à configurer après installation (nom, IP, MAJ, région)
- [ ] Expliquer la hiérarchie Forêt → Arbre → Domaine → OU
- [ ] Différencier OU et Groupe (rôle de chacun)
- [ ] Expliquer SAMAccountName vs UPN
- [ ] Différencier groupe de sécurité vs groupe de distribution
- [ ] Différencier groupe global vs domaine local
- [ ] Installer Windows Server 2022 dans une VM
- [ ] Configurer IP fixe + nom d'hôte post-installation
- [ ] Promouvoir le serveur en contrôleur de domaine (AD DS)
- [ ] Créer des OU dans ADUC
- [ ] Créer des utilisateurs dans une OU avec mot de passe
- [ ] Créer des groupes et y ajouter des utilisateurs
