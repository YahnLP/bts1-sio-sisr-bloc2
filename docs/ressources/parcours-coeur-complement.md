# Parcours « cœur » et « complément »

Les 19 séances de 4 h représentent **76 h** par an. Le volume prévu pour ce bloc en première année dans la grille sous statut scolaire est de **90 h**, concentrées sur le second semestre (annexe III.A du référentiel) : le parcours en apprentissage en offre environ 84 %. Quand le temps manque, ce parcours indique **ce qu'on peut alléger**.

> [!NOTE]
> Ce classement est une **proposition de l'auteur, à valider** par l'équipe pédagogique. Il ne retire rien du site : chaque séance reste disponible, et son statut est rappelé dans sa fiche.

## Résultat : aucune séance complément

Les séances forment une **chaîne** : l'adressage (S04) alimente S05, S07, S08, S10 et le projet ; la commutation (S05, S06) alimente S09, S13 et S16 ; Active Directory (S10 à S12) alimente S14 et S17 ; le projet S17 assemble les séances S04 à S15 ; S16 est la seule séance sur la supervision, la sauvegarde et la haute disponibilité (annexe II.E). J'ai relu les prérequis et les renvois entre séances : **aucune séance ne peut être retirée sans casser une autre**.

- **19 séances cœur** (76 h) ;
- **0 séance complément**.

| Séance | Thème | Parcours | Pourquoi |
|---|---|---|---|
| S01 | Matériel du PC, serveurs, connectique et BIOS/UEFI | **Cœur** | Matériel et serveurs, VM Windows : prérequis de S02 et S03. |
| S02 | Système d'exploitation, comptes utilisateurs et permissions NTFS | **Cœur** | Système, comptes et NTFS : prérequis de S10. |
| S03 | Les fondamentaux du réseau : modèle OSI, TCP/IP, diagnostic | **Cœur** | Bases du réseau : prérequis de S04 ; seule séance sur l'analyseur de trames. |
| S04 | Adressage IPv4, ARP, ICMP, configuration IP et premiers pas sous Linux | **Cœur** | Adressage IPv4 et VLSM : prérequis de S05, S07, S08, S10 et du projet S17. |
| S05 | Commutation, ligne de commande Cisco et commandes Linux | **Cœur** | Commutation et CLI Cisco : prérequis de S06, S13 et S16. |
| S06 | VLAN, trunk 802.1Q et utilisateurs Linux | **Cœur** | VLAN, trunk et utilisateurs Linux : prérequis de S07, S09 et S13. |
| S07 | Routage statique et service DHCP | **Cœur** | Routage statique et DHCP : prérequis de S08, S09, S12 et S15. |
| S08 | Service DNS (Bind9) et Spanning Tree Protocol | **Cœur** | DNS et STP : le DNS est prérequis de S09 et S10 ; le STP n'est repris nulle part (voir les parties allégeables). |
| S09 | Routage inter-VLAN, relais DHCP et TP intégré | **Cœur** | Inter-VLAN et relais DHCP : prérequis de S15 ; TP intégré de la phase 2. |
| S10 | Windows Server et Active Directory | **Cœur** | Active Directory : prérequis de S11, S12 et S14. |
| S11 | Stratégies de groupe (GPO) et DNS intégré à Active Directory | **Cœur** | GPO et DNS Windows : prérequis de S12. |
| S12 | DHCP Windows, partages de fichiers (NTFS) et DFS | **Cœur** | DHCP Windows, NTFS et DFS : prérequis de S14 et du projet S17. |
| S13 | Scripts Bash, serveur LAMP, administration à distance (SSH) et cron | **Cœur** | Bash, LAMP, SSH : prérequis de S14, S15 et S16. |
| S14 | PowerShell, Active Directory en masse et IPv6 | **Cœur** | PowerShell et IPv6 : le projet S17 réutilise les scripts PowerShell (OU, groupes, utilisateurs). |
| S15 | Pare-feu, NAT, DMZ et transfert de fichiers sécurisé | **Cœur** | Pare-feu, NAT, DMZ : prérequis de S16 et du projet S17 ; seule séance sur les périmètres de sécurité. |
| S16 | Supervision, sauvegarde et haute disponibilité | **Cœur** | Supervision, sauvegarde et haute disponibilité : seule séance sur ces éléments de l'annexe II.E. |
| S17 | Projet : l'infrastructure de la PME SimIO | **Cœur** | Projet PME : assemble S04 à S15 ; prépare l'épreuve E6. |
| S18 | Tests, dossier d'architecture technique (DAT) et fiche de réalisation | **Cœur** | Dossier d'architecture technique : prépare directement l'épreuve E6. |
| S19 | Épreuve blanche E6 et bilan de fin d'année | **Cœur** | Épreuve blanche E6. |

## Parties que l'on peut alléger

Les séances sont déjà denses. Faute de séance entière à retirer, voici les parties dont le contenu n'est repris nulle part ailleurs dans la progression.

| Où | Partie | Durée | Pourquoi on peut l'alléger |
|---|---|---|---|
| S08 | TP « Observer le STP sur trois commutateurs » | 50 min | Le STP n'est repris dans aucune séance suivante. Le remplacer par une démonstration de 15 à 20 minutes avec le simulateur ; garder le DNS, prérequis de S09 et S10. |
| S09 | TP « Routage inter-VLAN » (fichier 02b) | hors déroulé | Le TP intégré de la séance (parties A et B) couvre déjà la configuration du commutateur et du routeur ; le déroulé conseillé n'utilise pas ce TP. À réserver aux étudiants en difficulté ou à une révision. |
| S15 | Cours complémentaire « Protocoles applicatifs » (fichier 01d) | hors déroulé | Déjà présenté comme cours complémentaire et absent du déroulé. À donner à lire. Le cours FTP/SFTP (01c) reste utile : le projet S17 en a besoin. |

## Si le temps manque encore

Dans le déroulé conseillé, seul le TP sur le STP libère du temps (environ 30 minutes) ; les deux autres parties sont déjà hors déroulé. Au-delà, il faut alléger **à l'intérieur** de séances cœur (par exemple, ne faire que certaines étapes d'un TP) ou fusionner des séances ; à décider avec l'enseignant, en particulier pour S17 et S18 qui préparent l'épreuve E6.

> [!WARNING]
> Ne pas retirer S16 : c'est la seule séance qui porte la supervision, la sauvegarde et la haute disponibilité, éléments attendus de l'annexe II.E. Ne pas retirer non plus S14 : le projet S17 réutilise ses scripts PowerShell.

## Utilisation

- Si **toutes** les séances sont faites, rien ne change.
- Si le temps manque, alléger d'abord les parties du tableau ci-dessus.
- Les prérequis écrits dans chaque fiche ont été relus.
