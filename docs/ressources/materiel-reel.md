# Matériel réel ou simulation

Les TP réseau de ce bloc (S05 à S09 et le projet S17) sont écrits pour **Packet Tracer** ou pour le **simulateur réseau** du site. Chacun contient aussi une section **« Variante sur matériel réel »**.

> [!NOTE]
> **Règle de choix.** Si le matériel est disponible, on fait le TP sur matériel réel. Sinon, on le fait en simulation, avec Packet Tracer ou le simulateur : **c'est l'enseignant qui décide**, séance par séance. Les deux versions visent les mêmes compétences ; seul le montage change.

> [!WARNING]
> **Non testé sur matériel.** Les variantes ont été rédigées à partir des commandes déjà présentes dans les TP et des usages courants des équipements Cisco. Elles n'ont pas été essayées sur des équipements réels : les noms de ports, les modules et les versions d'IOS varient selon le modèle. À valider par l'enseignant sur le matériel de la salle.

## Pourquoi proposer les deux

- Une simulation sert à **comprendre et s'entraîner** ; elle ne suffit pas pour l'épreuve E6 (voir [Outils et simulateurs](outils.md) et la [fiche de réalisation](fiche-realisation-professionnelle.md)).
- Le matériel réel apporte ce que le simulateur ne montre pas : câbles et voyants, accès console, remise à zéro d'un équipement, temps réels de convergence, pare-feu des postes, erreurs de câblage.
- Sans matériel, la simulation permet de faire le TP en classe entière, sans réservation de salle.

## Matériel par TP

| Séance | TP | Équipements | Postes |
|---|---|---|---|
| S05 | Configurer un commutateur | 1 commutateur Cisco administrable | 3 PC |
| S06 | VLAN et trunk | 2 commutateurs | 6 PC |
| S07 | Relier deux réseaux par un routeur | 2 routeurs, 2 commutateurs | 4 PC |
| S08 | Observer le STP | 3 commutateurs | 3 PC |
| S09 | TP intégré (VLAN, routage, DHCP, DNS) | 1 commutateur, 1 routeur | 1 PC hôte d'une VM Debian, 2 PC clients |
| S09 | Routage inter-VLAN | 1 commutateur, 1 routeur | 3 PC |
| S17 | Projet PME, phase 2 | 1 routeur, 2 commutateurs | selon le plan retenu |

Pour tous : un **câble console** par équipement (avec adaptateur USB-série si les PC n'ont pas de port série), des câbles Ethernet, un logiciel de terminal sur les PC.

## Accès à l'équipement

- On se connecte par la **console** : le câble relie le port console de l'équipement au PC, et un logiciel de terminal (PuTTY ou équivalent) ouvre la session. Les réglages usuels des équipements Cisco sont 9600 bauds, 8 bits, sans parité, 1 bit d'arrêt, sans contrôle de flux ; la notice du modèle fait foi.
- L'invite « Initial configuration dialog? » s'affiche sur un équipement sans configuration : répondre `no`.

## Remise à zéro avant chaque TP

Un équipement de salle garde la configuration du groupe précédent. Avant de commencer :

```
Switch> enable
Switch# erase startup-config
Switch# delete flash:vlan.dat
Switch# reload
```

Sur un routeur, `erase startup-config` puis `reload` suffisent. Refaire la même remise à zéro **en fin de séance**, pour effacer les mots de passe configurés pendant le TP.

> [!NOTE]
> Les VLAN d'un commutateur sont enregistrés dans le fichier `vlan.dat`, pas dans la configuration de démarrage : sans `delete flash:vlan.dat`, ils survivent à l'effacement. Sur du matériel d'occasion, vérifier aussi `show vtp status` : un domaine VTP resté configuré peut écraser les VLAN créés. En cas de doute, passer en `vtp mode transparent`.

## Ce qui change par rapport à la simulation

| Sujet | Simulation | Matériel réel |
|---|---|---|
| Noms de ports | `Fa0/1`, `Fa0/24`, `Se0/0/0` | Selon le modèle : `Gi0/1`, `Fa0/48`, etc. Adapter les commandes `interface`. |
| Liaison série | Câble série DCE/DTE | Nécessite un module série. Sans module, utiliser un lien Ethernet routeur à routeur (sans `clock rate`). |
| Câbles | Droit ou croisé choisi dans la bibliothèque | Les équipements récents détectent le câble (auto-MDIX) ; sur du matériel ancien, relier deux équipements de même type avec un câble croisé. |
| Ports de routeur | Souvent actifs | Désactivés par défaut : `no shutdown`. |
| Pare-feu des PC | Absent | Un PC Windows bloque le ping entrant par défaut : autoriser l'ICMP pour le TP, ou le désactiver sur le réseau de TP, puis le rétablir. |
| Configuration IP des PC | Onglet *IP Configuration* | Réglages de la carte réseau du système. |
| Sauvegarde | Fichier Packet Tracer | `copy running-config startup-config`, puis enregistrer la sortie de `show running-config` dans un fichier texte. |
| Temps | Instantané ou accéléré | Réels : le STP met de l'ordre de la trentaine de secondes à converger. |

## Règles de sécurité du laboratoire

- **Isoler le montage** : aucun câble du TP ne doit rejoindre le réseau de l'établissement. Un triangle de commutateurs (S08) crée une boucle, et un serveur DHCP de TP (S09) perturbe les postes qui le trouvent.
- Repérer les câbles (étiquettes) et le plan de câblage avant de brancher.
- Ne pas laisser de mot de passe de TP sur un équipement : remise à zéro en fin de séance.

## Garder une preuve

Une photo du montage, un schéma, la sortie de `show running-config` et des tests réussis (voir la [fiche de réalisation](fiche-realisation-professionnelle.md)) alimentent le dossier. Ne pas y inclure de mot de passe réel.
