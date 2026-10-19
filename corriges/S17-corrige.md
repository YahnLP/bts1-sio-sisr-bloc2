# Corrigé S17 – Projet d'infrastructure de la PME SimIO

*Réservé à l'enseignant. Source : [projet S17](../docs/phase-4-securiser-presenter/S17-projet-pme/02-projet.md).*

## Phase 1 : plan d'adressage VLSM (bloc `192.168.0.0/22`)

| Priorité | VLAN | Besoin | Bloc | Réseau | Masque | Hôtes | Broadcast | Passerelle |
|---|---|---|---|---|---|---|---|---|
| 1 | 40 Commercial | 40 | /26 | `192.168.0.0` | `255.255.255.192` | .1 à .62 | `.63` | `.1` |
| 2 | 10 RH | 25 | /27 | `192.168.0.64` | `255.255.255.224` | .65 à .94 | `.95` | `.65` |
| 3 | 50 Comptabilité | 15 | /27 | `192.168.0.96` | `255.255.255.224` | .97 à .126 | `.127` | `.97` |
| 4 | 20 Informatique | 10 | /28 | `192.168.0.128` | `255.255.255.240` | .129 à .142 | `.143` | `.129` |
| 5 | 99 Serveurs | 10 | /28 | `192.168.0.144` | `255.255.255.240` | .145 à .158 | `.159` | `.158` (au choix, hors .145 à .147) |
| 6 | 30 Direction | 5 | /29 | `192.168.0.160` | `255.255.255.248` | .161 à .166 | `.167` | `.161` |

Justifications : Comptabilité (15 hôtes + passerelle) dépasse les 14 hôtes d'un /28, d'où un /27. Les serveurs du TP (`.145` à `.147`) tombent bien dans le VLAN 99. Il reste de la place pour l'extension dans le /22. **Accepter tout plan cohérent** (blocs alignés, sans chevauchement, marge de croissance justifiée).

**J'anticipe** : du plus grand au plus petit, pour éviter que de petits blocs fragmentent l'espace.

## Phase 2 : réseau Cisco

| Contrôle | Attendu |
|---|---|
| VLAN | 10, 20, 30, 40, 50, 99 sur les deux switchs |
| Trunk switch-routeur | Doit porter **tous** les VLAN |
| Trunk SW-BatA - SW-BatB | VLAN 40 et 50 (Bât. B) |
| Routeur | Sous-interfaces `.10`, `.20`, `.30`, `.40`, `.50`, `.99`, `encapsulation dot1Q`, adresse = passerelle du plan |
| DHCP | `ip helper-address` vers le serveur DHCP sur les VLAN clients |
| Tests | Passerelle de chaque VLAN, ping inter-VLAN (P1) |

**Je comprends** : ping inter-VLAN, passerelle de chaque VLAN, route par défaut, relais DHCP : on teste chaque couche avant d'ajouter des services.

## Phase 3 à 4 : Windows

| Étape | Attendu |
|---|---|
| AD | Forêt `siosarl.local`, OU par service avec `Utilisateurs` et `Ordinateurs`, groupes `GRP_<service>` |
| Utilisateurs | Au moins 2 par service, dans la bonne OU, membres du bon groupe |
| GPO | `GPO_Restriction_Standard` liée à RH, Commercial, Comptabilité, Direction (**pas** Informatique) ; `GPO_Fond_Ecran_SimIO` liée au domaine ; paramètres fins faits dans `gpmc.msc` |
| DHCP | Une étendue par VLAN, options routeur/DNS/domaine, autorisé dans l'AD |
| DNS | Zone `siosarl.local`, enregistrement `srv-ftp` → `192.168.0.147` |
| Partages | 6 dossiers, droits NTFS par groupe comme en S12 |

Remarques :
- `New-SmbShare -FullAccess "Tout le monde"` : bonne pratique si les droits NTFS sont restrictifs ; vérifier qu'ils le sont.
- Le DHCP est sur `SRV-DHCP` (`.146`) : l'autorisation se fait avec le nom et l'adresse de **ce** serveur (`srv-dhcp.siosarl.local`, `192.168.0.146`), et le routeur relaie vers `.146` (`ip helper-address`).
- Les mots de passe du sujet (`Admin!2024`, `Azerty!2024`, `Sftp!2024`) sont ceux du labo.

## Phase 5 : FTP et SFTP

| Point | Attendu |
|---|---|
| FTP anonyme | `anonymous_enable=YES`, `anon_root=/srv/ftp/public`, `anon_upload_enable=NO`, `local_enable=NO` ; `ls` et `get bienvenue.txt` réussissent ; `put` refusé |
| SFTP | 5 comptes `sftp_<service>` sans shell, racine de la cage `root:root 755`, dossier `upload` au compte ; bloc `Match Group sftpusers` à la **fin** de `sshd_config` |
| Tests | Dépôt dans `upload` réussi ; `ssh sftp_rh@...` refusé (service SFTP uniquement) |
| Journal | Accès visibles dans `/var/log/vsftpd.log` |

## Barème indicatif (60 points ou à ramener sur 20)

| Phase | Points |
|---|---|
| 1 : plan VLSM et schéma | 10 |
| 2 : réseau Cisco, ping inter-VLAN | 12 |
| 3 : AD, OU, utilisateurs, GPO | 12 |
| 4 : DHCP, DNS, partages | 12 |
| 5 : FTP anonyme et SFTP chroot | 10 |
| Captures P1 à P7 et suivi | 4 |

## Erreurs fréquentes

- Chevauchement de sous-réseaux ou bloc non aligné.
- Trunk vers le routeur qui ne porte pas tous les VLAN.
- Passerelle hors du sous-réseau.
- GPO liée à une OU qui ne contient pas les utilisateurs visés.
- Racine de la cage SFTP appartenant à l'utilisateur.
- FTP anonyme qui accepte l'écriture.
