# Annexe – Fiche de documentation technique du projet

*BTS SIO SISR 1re année – Bloc 2 – S17*

## SECTION 1 — Plan d'Adressage VLSM

*(Reproduire le tableau complété de la Phase 1)*

| **VLAN** | **Nom** | **Besoin** | **Réseau /n** | **Masque** | **Passerelle** | **Plage DHCP** |
|---|---|---|---|---|---|---|
| 10 | RH | 25 | | | | |
| 20 | Informatique | 10 | | | | |
| 30 | Direction | 5 | | | | |
| 40 | Commercial | 40 | | | | |
| 50 | Comptabilité | 15 | | | | |
| 99 | Serveurs | 10 | | | Fixe | N/A |

**Justification des choix de masque :**
*(Expliquer en 4-6 lignes pourquoi ces tailles de sous-réseaux ont été choisies)*

---

## SECTION 2 — Configuration Réseau

### Switch SW-BatA

| **Port** | **VLAN** | **Mode** |
|---|---|---|
| Fa0/1 | | |
| Fa0/2 | | |
| Fa0/3 | | |
| Gig0/1 | Trunk | Trunk → Routeur |
| Gig0/2 | Trunk | Trunk → SW-BatB |

### Routeur — Sous-interfaces

| **Sous-interface** | **Encapsulation** | **Adresse IP** | **VLAN** |
|---|---|---|---|
| G0/0.10 | dot1Q 10 | | VLAN 10 RH |
| G0/0.20 | dot1Q 20 | | VLAN 20 IT |
| G0/0.30 | dot1Q 30 | | VLAN 30 Dir |
| G0/0.40 | dot1Q 40 | | VLAN 40 Com |
| G0/0.50 | dot1Q 50 | | VLAN 50 Compta |
| G0/0.99 | dot1Q 99 | | VLAN 99 Serv |

---

## SECTION 3 — Active Directory

### Structure des OUs

<figure class="graphe diagramme"><svg viewBox="0 0 484 314" style="max-width:484px" role="img" aria-label="Arborescence des unités d&#x27;organisation du domaine siosarl.local" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="157" class="dg-line"/>
<line x1="26" y1="157" x2="50" y2="157" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="259" class="dg-line"/>
<line x1="26" y1="259" x2="50" y2="259" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="293" class="dg-line"/>
<line x1="26" y1="293" x2="50" y2="293" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="123" class="dg-line"/>
<line x1="64" y1="123" x2="88" y2="123" class="dg-line"/>
<line x1="64" y1="170" x2="64" y2="191" class="dg-line"/>
<line x1="64" y1="191" x2="88" y2="191" class="dg-line"/>
<line x1="64" y1="170" x2="64" y2="225" class="dg-line"/>
<line x1="64" y1="225" x2="88" y2="225" class="dg-line"/>
<rect x="12" y="8" width="130" height="26" rx="6" class="dg-dark"/>
<text x="77.0" y="26" text-anchor="middle" class="dg-w">siosarl.local</text>
<rect x="50" y="42" width="42" height="26" rx="6" class="dg-box"/>
<text x="71.0" y="60" text-anchor="middle" class="dg-t">RH</text>
<rect x="88" y="76" width="105" height="26" rx="6" class="dg-soft"/>
<text x="140.6" y="94" text-anchor="middle" class="dg-s">Utilisateurs</text>
<rect x="88" y="110" width="99" height="26" rx="6" class="dg-soft"/>
<text x="137.3" y="128" text-anchor="middle" class="dg-s">Ordinateurs</text>
<rect x="50" y="144" width="122" height="26" rx="6" class="dg-box"/>
<text x="111.0" y="162" text-anchor="middle" class="dg-t">Informatique</text>
<rect x="88" y="178" width="105" height="26" rx="6" class="dg-soft"/>
<text x="140.6" y="196" text-anchor="middle" class="dg-s">Utilisateurs</text>
<rect x="88" y="212" width="99" height="26" rx="6" class="dg-soft"/>
<text x="137.3" y="230" text-anchor="middle" class="dg-s">Ordinateurs</text>
<rect x="50" y="246" width="322" height="26" rx="6" class="dg-box"/>
<text x="211.0" y="264" text-anchor="middle" class="dg-t">Direction / Comptabilite / Commercial</text>
<text x="382.0" y="264" text-anchor="start" class="dg-i">même structure</text>
<rect x="50" y="280" width="132" height="26" rx="6" class="dg-soft"/>
<text x="115.8" y="298" text-anchor="middle" class="dg-s">Autres OU créées</text>
</svg>
<figcaption>Chaque service possède une OU avec ses utilisateurs et ses ordinateurs.</figcaption>
</figure>

### GPO Déployées

| **Nom GPO** | **Liée à** | **Paramètre principal** | **Effet** |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

---

## SECTION 4 — DHCP

| **Étendue** | **Réseau** | **Plage distribuée** | **Options (GW/DNS)** | **Durée bail** |
|---|---|---|---|---|
| VLAN 10 | | | | |
| VLAN 20 | | | | |
| VLAN 30 | | | | |
| VLAN 40 | | | | |
| VLAN 50 | | | | |

---

## SECTION 5 — Serveur FTP/SFTP Linux

| **Paramètre** | **Valeur** |
|---|---|
| Adresse IP du serveur | 192.168.0.147 |
| Distribution | Debian 12 |
| Logiciel FTP | vsftpd |
| Répertoire FTP anonyme | /srv/ftp/public |
| Accès anonyme — Droits | Lecture seule |
| Répertoire SFTP | /srv/sftp/ |
| Groupe SFTP | sftpusers |
| Shell des utilisateurs SFTP | /usr/sbin/nologin |

### Utilisateurs SFTP Créés

| **Login** | **Service** | **Répertoire chroot** | **Peut se connecter SSH ?** |
|---|---|---|---|
| sftp_rh | RH | /srv/sftp/sftp_rh | Non |
| sftp_informatique | IT | /srv/sftp/sftp_informatique | Non |
| sftp_direction | Direction | /srv/sftp/sftp_direction | Non |

---

## SECTION 6 — Tests de Validation

| **Test** | **Commande / Méthode** | **Résultat** | **Capture** |
|---|---|---|---|
| Ping inter-VLAN (PC-RH → PC-Commercial) | `ping` | Oui / Non | P1 |
| Structure AD visible dans ADUC | Console ADUC | Oui / Non | P2 |
| GPO créées et liées | `gpmc.msc` | Oui / Non | P3 |
| Partages accessibles | `\\siosarl.local\data` | Oui / Non | P4 |
| FTP anonyme — lecture OK | `ftp [IP]` + `get` | Oui / Non | P5 |
| SFTP authentifié — upload OK | `sftp sftp_rh@[IP]` + `put` | Oui / Non | P6 |
| SSH refusé pour sftpusers | `ssh sftp_rh@[IP]` | Oui / Non | P7 |

---

---
