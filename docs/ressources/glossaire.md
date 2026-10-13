# Glossaire

Tous les mots techniques du bloc 2, expliqués simplement. Dans les pages du site, **passe la souris sur un sigle** (VLAN, DHCP, GPO…) : sa définition s'affiche.

> [!NOTE]
> Un terme suivi de la mention **notion technique** dépasse le socle de première année : tu peux le laisser de côté.

La colonne « Vu en » indique la séance où le mot apparaît pour la première fois dans le cours.

| Terme | Sigle | Anglais | Définition | Vu en |
|---|---|---|---|---|
| **Adresse d'auto-configuration** | APIPA | *automatic private ip addressing* | Adresse en 169.254.x.x qu'une machine Windows s'attribue seule faute de serveur DHCP : signe d'un problème. | S04 |
| **Adresse de nouvelle génération** | IPv6 | *internet protocol version 6* | Adresses sur 128 bits écrites en hexadécimal, conçues pour remplacer l'IPv4. | S14 |
| **Adresse IP** | IP | *internet protocol* | Numéro qui identifie une machine sur un réseau (exemple : 192.168.1.20). | S03 |
| **Adresse MAC** | MAC | *media access control* | Numéro d'identification de la carte réseau (6 octets), utilisé sur le réseau local. | S03 |
| **Agent relais DHCP** |  | *DHCP relay* | Fonction d'un routeur qui transmet les demandes DHCP d'un VLAN vers un serveur situé dans un autre réseau (`ip helper-address`). | S09 |
| **Alimentation redondante** | PSU | *power supply unit* | Bloc d'alimentation d'un serveur ; un serveur en possède souvent deux pour résister à la panne de l'un. | S01 |
| **Analyseur de trames** |  | *packet sniffer* | Logiciel (Wireshark) qui capture et décode les trames qui passent sur le réseau. | S03 |
| **Annuaire** |  | *directory* | Base centralisée des comptes, groupes et machines d'une organisation. | S10 |
| **Annuaire de Microsoft** | AD | *Active Directory* | Annuaire utilisé dans les entreprises Windows. | S10 |
| **Applet de commande** |  | *cmdlet* | Commande PowerShell, nommée Verbe-Nom (exemple : Get-Process). | S14 |
| **Arbre recouvrant** | STP | *spanning tree protocol* | Protocole qui bloque certains liens redondants pour éviter les boucles entre commutateurs. | S08 |
| **Attribution automatique d'adresses** | DHCP | *dynamic host configuration protocol* | Service qui donne automatiquement une adresse IP aux machines qui se connectent. | S07 |
| **Baie de brassage** |  | *rack* | Armoire normalisée dans laquelle on monte serveurs et équipements réseau. | S01 |
| **Bail DHCP** |  | *lease* | Durée pendant laquelle une adresse est prêtée à une machine. | S07 |
| **Câble à paires torsadées** | RJ45 | *registered jack 45* | Connecteur à 8 broches des câbles Ethernet en cuivre. | S03 |
| **Clé publique et clé privée** |  | *key pair* | Paire de clés qui remplace le mot de passe : la privée reste chez toi, la publique est déposée sur le serveur. | S13 |
| **Commutateur** |  | *switch* | Équipement qui relie les machines d'un réseau local et envoie chaque trame vers le bon port grâce à sa table MAC. | S03 |
| **Concentrateur** |  | *hub* | Ancien équipement qui répète le signal sur tous ses ports ; remplacé par le commutateur. | S03 |
| **Connexion sécurisée à distance** | SSH | *secure shell* | Prendre le contrôle d'un terminal sur une autre machine, par une connexion chiffrée. | S13 |
| **Contrôle de compte d'utilisateur** | UAC | *user account control* | Fenêtre de Windows qui demande une confirmation avant une action d'administrateur. | S02 |
| **Contrôleur de domaine** | DC | *domain controller* | Serveur qui héberge l'annuaire Active Directory et authentifie les utilisateurs. | S10 |
| **Découpage en sous-réseaux de tailles variables** | VLSM | *variable length subnet masking* | Technique qui découpe un réseau en sous-réseaux de tailles différentes, adaptées aux besoins. | S04 |
| **Diffusion** |  | *broadcast* | Message envoyé à toutes les machines d'un même réseau local. | S03 |
| **Disque dur** | HDD | *hard disk drive* | Stockage à plateaux tournants : grand, peu cher, assez lent. | S01 |
| **Disque SSD** | SSD | *solid state drive* | Stockage sans pièce mobile (mémoire flash) : plus rapide et plus résistant aux chocs qu'un disque dur. | S01 |
| **Domaine de diffusion** |  | *broadcast domain* | L'ensemble des machines qui reçoivent une même diffusion ; un VLAN en forme un. | S03 |
| **Dossier d'architecture technique** | DAT | *technical architecture document* | Document qui décrit une infrastructure : besoins, schémas, choix, configuration, tests, exploitation. | S18 |
| **Durée maximale d'interruption admise** | RTO | *recovery time objective* | Le temps maximum acceptable pour remettre le service en marche après une panne. | S16 |
| **Enregistrement DNS** |  | *DNS record* | Ligne d'une zone : A (nom vers IPv4), AAAA (IPv6), CNAME (alias), MX (messagerie), PTR (adresse vers nom). | S08 |
| **Environnement de commande de Windows** |  | *PowerShell* | Interpréteur de commandes de Windows, qui manipule des objets plutôt que du texte. | S14 |
| **Étendue DHCP** |  | *scope* | Plage d'adresses qu'un serveur DHCP peut distribuer pour un réseau donné. | S07 |
| **Fiche descriptive de réalisation** |  |  | Document de deux pages qui présente une réalisation professionnelle présentée à l'épreuve E6. | S18 |
| **Grappe de disques** | RAID | *redundant array of independent disks* | Plusieurs disques utilisés ensemble pour gagner en vitesse et/ou résister à la panne d'un disque. Ce n'est pas une sauvegarde. | S01 |
| **Groupe de sécurité** |  | *security group* | Ensemble de comptes auquel on donne des droits : on gère les groupes plutôt que les personnes. | S12 |
| **Haute disponibilité** | HA | *high availability* | Conception qui permet à un service de continuer malgré la panne d'un élément. | S16 |
| **Interface en ligne de commande de Cisco** | IOS | *internetwork operating system* | Système des équipements Cisco, administré en ligne de commande. | S05 |
| **Interface graphique** | GUI | *graphical user interface* | On commande avec la souris, des fenêtres et des icônes. | S02 |
| **Interpréteur de commandes** | bash | *bourne again shell* | Programme qui lit les commandes tapées sous Linux et lance les scripts. | S13 |
| **Lien trunk** |  | *trunk* | Lien entre deux commutateurs qui transporte plusieurs VLAN, chaque trame portant une étiquette. | S06 |
| **Ligne de commande** | CLI | *command line interface* | On commande en tapant du texte dans un terminal. | S02 |
| **Liste de contrôle d'accès** | ACL | *access control list* | Liste des utilisateurs et groupes autorisés (ou refusés) sur un dossier ou un fichier, avec leurs droits. | S02 |
| **Liste de contrôle d'accès réseau (ACL)** |  | *access control list* | Liste de règles d'un routeur qui autorise ou refuse des paquets selon leur origine, leur destination ou leur port. | S15 |
| **Machine virtuelle** | VM | *virtual machine* | Un ordinateur simulé par un logiciel, qui tourne dans une fenêtre de l'ordinateur réel. | S01 |
| **Masque de sous-réseau** |  | *subnet mask* | Nombre qui sépare, dans une adresse IP, la partie réseau de la partie machine. | S04 |
| **Mémoire vive** | RAM | *random access memory* | Mémoire rapide où l'ordinateur place ce qu'il utilise en ce moment ; elle se vide quand on éteint. | S01 |
| **Micrologiciel de démarrage** | BIOS | *basic input output system* | Programme inscrit sur la carte mère qui démarre la machine avant le système ; ancienne génération. | S01 |
| **Micrologiciel de démarrage moderne** | UEFI | *unified extensible firmware interface* | Remplaçant moderne du BIOS : démarrage plus rapide, disques de grande taille, démarrage sécurisé. | S01 |
| **Modèle OSI** | OSI | *open systems interconnection* | Découpage théorique de la communication réseau en 7 couches, utilisé pour situer un problème. | S03 |
| **Norme d'étiquetage des VLAN** | 802.1Q | *IEEE 802.1Q* | Norme qui ajoute à la trame Ethernet une étiquette indiquant son VLAN. | S06 |
| **Notation de masque en nombre de bits** | CIDR | *classless inter-domain routing* | Écriture /24 qui indique combien de bits du début de l'adresse désignent le réseau. | S04 |
| **Paquet** |  | *packet* | Unité de données routée entre réseaux (couche 3) ; contient les adresses IP. | S03 |
| **Pare-feu** |  | *firewall* | Équipement ou logiciel qui autorise ou bloque les communications selon des règles. | S15 |
| **Partage de fichiers de Windows** | SMB | *server message block* | Protocole des partages de fichiers Windows (dossiers partagés, imprimantes). | S12 |
| **Passerelle par défaut** |  | *default gateway* | Adresse du routeur vers lequel une machine envoie tout ce qui sort de son réseau. | S04 |
| **Perte de données maximale admise** | RPO | *recovery point objective* | La quantité de données (exprimée en temps) que l'on accepte de perdre. | S16 |
| **Pile de services web** | LAMP | *Linux Apache MariaDB PHP* | Ensemble Linux, Apache, MariaDB et PHP qui permet d'héberger un site web dynamique. | S13 |
| **Plan d'adressage** |  | *addressing plan* | Tableau qui attribue à chaque réseau et à chaque équipement sa plage ou son adresse IP. | S17 |
| **Planificateur de tâches** | cron | *cron* | Service Linux qui lance des commandes à heures fixes. | S13 |
| **Principe de moindre privilège** |  | *least privilege* | Donner à chacun uniquement les droits dont il a besoin, pas plus. | S12 |
| **Processeur** | CPU | *central processing unit* | Le composant qui exécute les instructions des programmes. | S01 |
| **Protocole d'annuaire** | LDAP | *lightweight directory access protocol* | Langage pour interroger un annuaire. | S10 |
| **Protocole de communication réseau** | TCP/IP | *transmission control protocol / internet protocol* | L'ensemble de règles de communication utilisé par Internet. | S03 |
| **Protocole de messages de contrôle** | ICMP | *internet control message protocol* | Protocole des messages de diagnostic et d'erreur ; c'est lui qui transporte le `ping`. | S04 |
| **Protocole de supervision réseau** | SNMP | *simple network management protocol* | Permet d'interroger à distance un équipement réseau (imprimante, commutateur) sur son état. | S16 |
| **Règle de sauvegarde 3-2-1** |  | *3-2-1 backup rule* | Trois copies des données, sur deux supports différents, dont une hors site. | S16 |
| **Répartition de charge** |  | *load balancing* | Répartir les demandes entre plusieurs serveurs identiques. | S16 |
| **Réseau local virtuel** | VLAN | *virtual local area network* | Découpage logique d'un commutateur en plusieurs réseaux séparés. | S06 |
| **Réseau privé virtuel** | VPN | *virtual private network* | Tunnel chiffré qui relie deux réseaux ou un poste distant à un réseau en passant par Internet. | S15 |
| **Réservation DHCP** |  | *reservation* | Adresse toujours donnée à la même machine, repérée par son adresse MAC. | S07 |
| **Résolution d'adresses** | ARP | *address resolution protocol* | Protocole qui retrouve l'adresse MAC correspondant à une adresse IP du réseau local. | S04 |
| **Routage inter-VLAN** |  | *inter-VLAN routing* | Faire communiquer des VLAN entre eux en passant par un routeur ou un commutateur de niveau 3. | S09 |
| **Routage statique** |  | *static routing* | Routes écrites à la main par l'administrateur dans la table de routage. | S07 |
| **Route par défaut** |  | *default route* | Route utilisée quand aucune autre ne correspond (0.0.0.0/0), souvent vers Internet. | S07 |
| **Routeur** |  | *router* | Équipement qui relie des réseaux différents et choisit le chemin des paquets. | S03 |
| **Routeur sur un bras** |  | *router on a stick* | Un seul lien trunk entre routeur et commutateur, découpé en sous-interfaces, une par VLAN. | S09 |
| **Sécurité de port** |  | *port security* | Fonction du commutateur qui limite les adresses MAC autorisées sur un port. | S05 |
| **Services de domaine Active Directory** | AD DS | *Active Directory domain services* | Rôle Windows Server qui fournit l'annuaire et l'authentification du domaine. | S10 |
| **Stratégie de groupe** | GPO | *group policy object* | Règles appliquées automatiquement aux comptes et aux postes Windows d'un domaine. | S11 |
| **Supervision** |  | *monitoring* | Surveillance continue de l'état des équipements et des services, avec alertes en cas de panne. | S16 |
| **Système de détection d'intrusion** | IDS | *intrusion detection system* | Outil qui surveille le réseau ou une machine et signale les comportements suspects. | S16 |
| **Système de fichiers de Windows** | NTFS | *new technology file system* | Système de fichiers de Windows, qui gère des droits d'accès précis sur chaque dossier et fichier. | S02 |
| **Système de fichiers distribué** | DFS | *distributed file system* | Présente plusieurs partages répartis sur différents serveurs sous une arborescence unique. | S12 |
| **Système de noms de domaine** | DNS | *domain name system* | Annuaire d'Internet : il traduit un nom (www.exemple.fr) en adresse IP. | S08 |
| **Table d'adresses MAC** |  | *MAC address table* | Table du commutateur qui associe chaque adresse MAC au port où elle a été vue. | S05 |
| **Table de routage** |  | *routing table* | Liste des réseaux connus d'un routeur et du chemin à suivre pour chacun. | S07 |
| **Tolérance de panne** |  | *fault tolerance* | Capacité d'un système à continuer de fonctionner malgré une panne. | S16 |
| **Traduction d'adresses** | NAT | *network address translation* | Le routeur fait passer les machines du réseau privé sur Internet avec sa propre adresse. | S15 |
| **Trame** |  | *frame* | Unité de données transmise sur un lien local (couche 2) ; contient les adresses MAC. | S03 |
| **Transfert de fichiers** | FTP | *file transfer protocol* | Protocole de transfert de fichiers, non chiffré ; à remplacer par SFTP. | S15 |
| **Transfert de fichiers sécurisé** | SFTP | *SSH file transfer protocol* | Transfert de fichiers à l'intérieur d'une connexion SSH chiffrée. | S15 |
| **Unité d'organisation** | OU | *organizational unit* | Dossier de l'annuaire qui regroupe des comptes et des ordinateurs pour leur appliquer des règles. | S10 |
| **Unité de rack** |  | *rack unit* | Hauteur normalisée (4,45 cm) des équipements montés en baie. | S01 |
| **VLAN natif** |  | *native VLAN* | VLAN dont les trames circulent sans étiquette sur un trunk. | S06 |
| **Zone démilitarisée** | DMZ | *demilitarized zone* | Réseau intermédiaire qui héberge les serveurs accessibles depuis Internet, isolé du réseau interne. | S15 |
| **Zone DNS** |  | *DNS zone* | Partie de l'espace de noms dont un serveur DNS a la charge. | S08 |
