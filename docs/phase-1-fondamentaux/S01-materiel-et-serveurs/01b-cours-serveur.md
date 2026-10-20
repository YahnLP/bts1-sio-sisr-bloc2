# Cours – Serveur, connectique et BIOS/UEFI

*BTS SIO SISR 1re année – Bloc 2 – S01*

## I. PC vs Serveur : Deux Mondes, Deux Philosophies

### A. Rappel : Le PC de Bureau (Desktop Computer)

**Définition :** Ordinateur personnel conçu pour un **usage individuel**, généralement utilisé 8 heures par jour en moyenne.

**Caractéristiques principales :**
- **Format** : Tour (ATX, Micro-ATX) ou All-in-One
- **Utilisateur** : 1 personne
- **Durée de vie** : 3-5 ans
- **Priorités** : Coût, polyvalence, confort utilisateur (silence, esthétique)

---

### B. Le Serveur : Définition et Rôle

**Définition :** Machine informatique **dédiée** à fournir des **services** à d'autres ordinateurs (clients) via un réseau, fonctionnant **en continu** (24h/24, 7j/7).

**Analogie :** Un serveur est comme un **restaurant** : il reste ouvert toute la journée pour servir de nombreux clients, alors qu'un PC est comme une **cuisine personnelle** utilisée quelques heures par jour.

 **Lien avec ITIL :** Le serveur est un **actif de configuration (CI)** critique dans la CMDB. Sa disponibilité conditionne la **continuité de service** de toute l'organisation !

---

### C. Tableau Comparatif Détaillé

| **Critère** | **PC de Bureau** | **Serveur Professionnel** |
|-------------|------------------|---------------------------|
| **Usage principal** | Bureautique, jeux, création | Hébergement de services (web, fichiers, bases de données, mails...) |
| **Nombre d'utilisateurs** | 1 | De 10 à plusieurs milliers simultanément |
| **Temps de fonctionnement** | 8h/jour en moyenne | **24h/24, 7j/7, 365j/an** |
| **Format** | Tour (verticale), All-in-One | **Rack** (horizontal 1U/2U/4U) ou Tour |
| **Emplacement** | Bureau, domicile | **Datacenter**, salle serveur climatisée |
| **Processeur** | Intel Core i3/i5/i7, AMD Ryzen | Intel Xeon, AMD EPYC (multi-socket possibles) |
| **RAM** | 8-32 Go | 32 Go à plusieurs **To** (ECC pour fiabilité) |
| **Stockage** | 500 Go - 2 To | Plusieurs To à plusieurs dizaines de To (RAID) |
| **Alimentation** | 1 bloc (450-750W) | **2 blocs redondants** (hot-swap) |
| **Refroidissement** | 2-3 ventilateurs | **Ventilateurs redondants** + climatisation dédiée |
| **Cartes réseau** | 1 port Gigabit | **2-4 ports Gigabit ou 10 Gbps** (redondance) |
| **Administration** | Locale (écran+clavier branchés) | **À distance** (SSH, RDP, iLO, iDRAC) |
| **Système d'exploitation** | Windows 10/11, Linux Desktop | Windows Server, Linux Server (CentOS, Ubuntu Server, Debian...) |
| **Disponibilité attendue** | Pas d'engagement : une panne gêne une personne | **99,9 % à 99,999 %** (≈ 8 h 45 min à ≈ 5 min d'indisponibilité par an) |
| **Garantie** | 1-3 ans | 3-5 ans (support 24/7 possible) |
| **Prix** | 400 - 2 000 € | 2 000 - 50 000 €+ |

---

### D. Technologies Spécifiques aux Serveurs

#### **Redondance des Composants**

**Définition :** Duplication des composants critiques pour qu'en cas de panne de l'un, l'autre prenne automatiquement le relais **sans interruption de service**.

**Composants redondés :**

| Composant | Risque si panne | Solution serveur |
|-----------|----------------|------------------|
| **Alimentation** | Arrêt complet du serveur | **2 blocs d'alimentation** (dual PSU) |
| **Disques durs** | Perte de données | **RAID** (plusieurs disques avec redondance) |
| **Cartes réseau** | Perte de connexion | **2 cartes réseau** (bonding/teaming) |
| **Ventilateurs** | Surchauffe → arrêt | **Ventilateurs multiples** + alerte |

---

#### **Hot-Swap (Échange à Chaud)**

**Définition :** Capacité à **remplacer un composant défectueux sans éteindre le serveur**.

**Composants hot-swap courants :**
- Disques durs (baies hot-swap)
- Alimentations
- Ventilateurs
- Cartes réseau (sur certains modèles haut de gamme)

**Avantage :** Maintenance **sans interruption de service** → continuité d'activité préservée.

---

#### **Format Rack**

**Définition :** Format standardisé permettant de monter les serveurs dans une **armoire (baie) de 19 pouces**.

**Unité de mesure :** Le **"U"** (Rack Unit)
- 1U = 1.75 pouce = **44.45 mm de hauteur**
- Les armoires font généralement **42U** de haut (environ 2 mètres)

**Formats courants :**
- **1U** : Serveur très plat, haute densité (ex: serveur web, firewall)
- **2U** : Format standard (bon compromis densité/performance)
- **4U** : Serveur puissant avec beaucoup de disques

**Avantages du format rack :**
- **Avantage :** **Gain de place** : densité maximale
- **Avantage :** **Gestion des câbles** optimisée
- **Avantage :** **Climatisation centralisée** de l'armoire
- **Avantage :** **Sécurité physique** (armoire verrouillable)

---

#### **Administration à Distance**

**Pourquoi ?** Un datacenter peut contenir des **centaines de serveurs**. Impossible d'avoir un écran/clavier branché sur chacun !

**Technologies d'administration à distance :**

| **Technologie** | **Type** | **Description** |
|-----------------|----------|-----------------|
| **SSH** (Secure Shell) | Logiciel | Accès en ligne de commande sécurisé (Linux/Unix) |
| **RDP** (Remote Desktop Protocol) | Logiciel | Bureau à distance (Windows Server) |
| **iLO** (HP) / **iDRAC** (Dell) / **IPMI** | Matériel | Carte d'administration intégrée au serveur, accessible même serveur éteint |

**Carte d'administration (iLO/iDRAC) :**
- Petit ordinateur **indépendant** intégré au serveur
- Possède sa **propre carte réseau** et son **IP dédiée**
- Permet de :
  - Allumer/éteindre le serveur à distance
  - Accéder au BIOS à distance
  - Voir l'écran de démarrage (KVM over IP)
  - Monter une ISO virtuelle pour installer un OS
  - Surveiller la santé du serveur (températures, pannes disques...)

---

### E. Cas d'Usage : Quand Utiliser un Serveur ?

**On utilise un serveur quand :**

1. **Plusieurs utilisateurs** doivent accéder aux **mêmes données** (ex: serveur de fichiers)
2. Un **service doit être disponible 24/7** (ex: site web, serveur mail)
3. On a besoin de **centraliser la gestion** (ex: Active Directory pour gérer les comptes utilisateurs)
4. La **sécurité des données** est critique (sauvegardes, RAID, accès contrôlés)
5. La **puissance de calcul** nécessaire dépasse un PC standard (ex: serveur de bases de données, serveur de rendu 3D)

**On peut utiliser un PC (ou mini-PC) quand :**

1. Usage très limité (1-5 utilisateurs occasionnels)
2. Budget serré (TPE, association, particulier)
3. Service non critique (tolérance à l'indisponibilité)
4. Apprentissage / maquettage (comme en BTS SIO !)

 **Note :** Dans un contexte professionnel d'entreprise, on privilégie **toujours un serveur** pour les services critiques.

### F. Aperçu : ce que coûte un choix de matériel

Choisir entre un PC et un serveur ne se résume pas au prix d'achat. Le **coût total de possession** (TCO, *total cost of ownership*) additionne ce qu'on paie **pendant toute la vie** du matériel : achat, électricité, maintenance, licences, temps d'administration, remplacement.

**Exemple chiffré (fictif).** Un petit serveur coûte 2 400 € à l'achat, consomme en moyenne 150 W en continu et reste en service 5 ans. Le kWh est facturé 0,20 €.

- Énergie par an : 0,150 kW × 24 h × 365 j = 1 314 kWh, soit 1 314 × 0,20 = **262,80 €** ;
- Énergie sur 5 ans : 262,80 × 5 = **1 314 €** ;
- Maintenance supposée : 120 € par an, soit 600 € sur 5 ans ;
- TCO sur 5 ans : 2 400 + 1 314 + 600 = **4 314 €**, soit environ 863 € par an.

L'électricité dépasse ici la moitié du prix d'achat : un matériel moins cher mais plus gourmand peut revenir plus cher. À l'échelle d'une organisation, la fin de vie compte aussi : le matériel électronique se **recycle** (déchets d'équipements électriques et électroniques, DEEE) et ne se jette pas.

En 2e année, la séance [Coût, éthique et environnement](https://yahnlp.github.io/bts2-sio-sisr-bloc2/phase-1-reprendre-concevoir/S03-cout-ethique-environnement/) reprend la méthode complète. Ici, retiens le réflexe : **comparer des coûts sur la durée**.

**Je vérifie.** Même serveur, mais consommation moyenne de 100 W. Quelle est l'économie d'énergie sur 5 ans ?

<details markdown="1">
<summary>Éléments de réponse</summary>

0,100 kW × 24 × 365 = 876 kWh par an, soit 175,20 € ; sur 5 ans 876 €. Économie : 1 314 − 876 = **438 €**.

</details>

---

## II. Périphériques et Connectique : Le Grand Tour

### A. Pourquoi Connaître les Connectiques ?

En tant que **technicien systèmes et réseaux**, vous serez amené à :

- **Brancher** des équipements (serveurs, switchs, écrans, périphériques...)
- **Diagnostiquer** des problèmes de connexion (mauvais câble, port défectueux...)
- **Conseiller** les utilisateurs sur le bon câble à utiliser
- **Auditer** l'infrastructure existante (inventaire des câblages)

 **Retour d'expérience :** devant un « ça ne marche plus », le **câble** (mal branché, défectueux, mauvais type) est l'une des premières choses à contrôler : c'est gratuit et rapide.

---

### B. Les Connectiques USB : L'Universel

**USB = Universal Serial Bus** (Bus série universel)

#### Les Différents Formats (Types de Connecteurs)

| **Type** | **Forme** | **Usage Principal** | **Image** |
|----------|-----------|---------------------|-----------|
| **USB Type-A** | Rectangle plat | Côté ordinateur (hôte) | Standard depuis 1996 |
| **USB Type-B** | Carré/trapèze | Côté périphérique (imprimante, scanner) | Moins courant aujourd'hui |
| **USB Type-C** | Ovale symétrique | **Standard moderne** (réversible) | Remplace progressivement A et B |
| **Micro-USB** | Petit trapèze | Anciens smartphones, petits appareils | En voie de disparition |
| **Mini-USB** | Plus gros que micro | Vieux appareils photo, GPS | Obsolète |

---

#### Les Versions USB (Vitesses)

| **Version** | **Débit Max** | **Année** | **Reconnaissable à** |
|-------------|---------------|-----------|----------------------|
| **USB 1.0/1.1** | 12 Mb/s | 1996 | Port **blanc** ou sans couleur |
| **USB 2.0** | 480 Mb/s (60 Mo/s) | 2000 | Port **noir** |
| **USB 3.0 / 3.1 Gen 1** | 5 Gb/s (625 Mo/s) | 2008 | Port **bleu** |
| **USB 3.1 Gen 2** | 10 Gb/s (1.25 Go/s) | 2013 | Port bleu ou **bleu-vert** |
| **USB 3.2** | 20 Gb/s | 2017 | Port USB-C (parfois marqué SS20) |
| **USB4** | 40 Gb/s | 2019 | Port USB-C (compatible Thunderbolt 3) |

**Rétrocompatibilité :** Un périphérique USB 2.0 fonctionne sur un port USB 3.0 (mais à vitesse USB 2.0). Un câble USB 3.0 est **nécessaire** pour atteindre les vitesses USB 3.0+.

---

#### USB Type-C : La Révolution

**Avantages :**
- **Avantage :** **Réversible** : se branche dans les 2 sens (enfin !)
- **Avantage :** **Universel** : même connecteur pour smartphones, PC, tablettes, écrans...
- **Avantage :** **Multifonction** : données + vidéo + alimentation (jusqu'à 100W)
- **Avantage :** **Rapide** : jusqu'à 40 Gb/s (USB4)

**Attention piège :** Tous les câbles USB-C ne se valent pas !
- Certains sont **charge uniquement** (pas de données)
- Certains sont USB 2.0 (lents) même s'ils ont un connecteur Type-C
- Il faut vérifier les **spécifications** du câble !

---

### C. Les Connectiques Vidéo : VGA, HDMI, DisplayPort

#### Tableau Comparatif

| **Connecteur** | **Type** | **Résolution Max** | **Audio** | **État** |
|----------------|----------|-------------------|-----------|----------|
| **VGA** | Analogique | 1920×1200 | Non | Obsolète |
| **DVI** | Numérique | 2560×1600 | Non | En voie de disparition |
| **HDMI** | Numérique | 8K à 60 Hz (v2.1) | Oui | Standard TV/multimédia |
| **DisplayPort** | Numérique | 8K à 60 Hz (v1.4) | Oui | Standard PC/professionnel |
| **USB-C (alt mode)** | Numérique | 8K à 60 Hz | Oui | Futur standard |
| **GPMI** | Numérique | 8K et au-delà (annoncé) | Oui | Émergent (norme chinoise annoncée en 2025) |

---

#### **VGA (Video Graphics Array)** - Obsolète

**Reconnaissable à :** Connecteur **trapézoïdal bleu à 15 broches**

**Caractéristiques :**
- Technologie **analogique** (années 1980)
- Signal dégradé sur longues distances (>5m)
- Qualité d'image inférieure au numérique

**Usage actuel :** Encore présent sur vieux projecteurs ou écrans, mais **à éviter**.

---

#### **HDMI (High-Definition Multimedia Interface)**

**Reconnaissable à :** Connecteur **rectangulaire noir/doré plat**

**Caractéristiques :**
- Transporte **vidéo + audio** sur un seul câble
- Standard sur les **TV, consoles de jeu, lecteurs Blu-ray**
- Plusieurs versions (1.4, 2.0, 2.1) avec résolutions/fréquences croissantes
- Longueur maximale : **10-15 m** (au-delà, signal dégradé ou nécessité d'amplificateur)

**Versions courantes :**
- **HDMI 1.4** : Full HD 1080p à 60 Hz
- **HDMI 2.0** : 4K à 60 Hz
- **HDMI 2.1** : 8K à 60 Hz, 4K à 120 Hz (gaming)

---

#### **DisplayPort**

**Reconnaissable à :** Connecteur **rectangulaire noir avec un coin coupé**

**Caractéristiques :**
- Concurrent de HDMI, privilégié dans le **monde professionnel**
- Supporte le **Multi-Stream Transport (MST)** : plusieurs écrans sur un seul câble (daisy-chaining)
- Débits supérieurs à HDMI à version équivalente
- Pas de royalties (HDMI est soumis à licence)

**Avantage pro :** Idéal pour setups multi-écrans (trading, design, développement)

#### **HDMI ou DisplayPort : que choisir ?**

C'est une question récurrente. Les deux transportent image et son numériques ; ils n'ont pas été conçus pour le même monde.

| **Critère** | **HDMI** | **DisplayPort** |
|-------------|----------|-----------------|
| **Origine** | Consortium de fabricants d'électronique grand public (2002) | VESA, association de l'industrie informatique (2006) |
| **Usage typique** | Téléviseurs, consoles, lecteurs Blu-ray, vidéoprojecteurs | Cartes graphiques, écrans de PC, postes de travail professionnels |
| **Licence** | Payante pour les fabricants (HDMI Licensing Administrator) | Norme ouverte de la VESA, sans redevance par port |
| **Plusieurs écrans sur un câble** | Non : une sortie, un écran | Oui, avec le **MST** (chaînage d'écrans, DisplayPort 1.2 et suivants) |
| **Commande des appareils** | **CEC** (une télécommande pilote TV et lecteur), **ARC/eARC** (retour du son vers une barre de son) | Pas d'équivalent ; en revanche mode alternatif sur **USB-C** très répandu sur les portables |
| **Fréquence variable (anti-saccade)** | VRR à partir de HDMI 2.1 | Adaptive-Sync (FreeSync, G-Sync Compatible) depuis longtemps |
| **Verrouillage du connecteur** | Non, il peut se débrancher | Souvent un **clip de verrouillage**, utile en salle serveur ou sur un poste fixe |
| **Débit maximal (exemples)** | HDMI 2.1 : 48 Gb/s | DisplayPort 1.4 : 32,4 Gb/s ; DisplayPort 2.1 : jusqu'à 80 Gb/s |

> [!NOTE]
> Le débit dépend de la **version** et non du nom de la norme : un port HDMI 2.1 peut dépasser un DisplayPort 1.4, et inversement avec DisplayPort 2.1. Pour savoir ce qu'on peut afficher (résolution, fréquence), on lit la version du port **et** celle du câble.

**Règle pratique :**
- Une **TV** ou une **console** : HDMI.
- Un **PC** et des **écrans de bureau**, surtout à plusieurs écrans : DisplayPort.
- Brancher un port DisplayPort sur un écran HDMI est possible avec un **adaptateur** ou un câble DisplayPort vers HDMI (simple et peu coûteux dans ce sens). Le sens inverse (HDMI vers DisplayPort) demande un adaptateur **actif**, plus cher.

---

#### **GPMI (General Purpose Media Interface) : à surveiller**

**GPMI** est une interface annoncée en 2025 par une alliance de plus de 50 entreprises chinoises. Elle vise à remplacer à la fois HDMI et DisplayPort par **un seul câble** qui transporte vidéo, audio, données, **alimentation** et commandes. Deux variantes ont été présentées :

| **Variante** | **Connecteur** | **Débit annoncé** | **Alimentation annoncée** | **Cible** |
|--------------|----------------|-------------------|---------------------------|-----------|
| **GPMI Type-C** | Celui de l'**USB-C** | 96 Gb/s | jusqu'à 240 W | Portables, écrans, accessoires |
| **GPMI Type-B** | Connecteur dédié, plus grand | 192 Gb/s | jusqu'à 480 W | Usages professionnels (production vidéo, imagerie médicale) |

> [!WARNING]
> Ces valeurs sont celles **annoncées** par ses promoteurs. GPMI est d'abord une norme chinoise : fin 2025, les grands constructeurs mondiaux (PC, consoles, téléphones) ne l'ont pas adoptée. Dans une entreprise, on rencontre aujourd'hui HDMI, DisplayPort et USB-C. À retenir surtout comme tendance : la **convergence vers un câble unique** (comme l'USB-C et le Thunderbolt), et comme exemple de **veille technologique** (voir S14), avec le risque d'une norme concurrente de plus.

---

### D. Les Connectiques Réseau : RJ45 et Fibre Optique

#### **RJ45 (Ethernet sur Câble Cuivre)**

**Reconnaissable à :** Connecteur **transparent/translucide avec 8 contacts dorés**

**Caractéristiques :**
- Standard pour les **réseaux locaux (LAN)**
- Câble appelé **paire torsadée** (Twisted Pair)
- Plusieurs catégories selon la vitesse supportée

| **Catégorie** | **Débit Max** | **Distance Max** | **Usage** |
|---------------|---------------|------------------|-----------|
| **Cat 5** | 100 Mb/s | 100 m | Obsolète |
| **Cat 5e** | 1 Gb/s | 100 m | Standard actuel maisons/bureaux |
| **Cat 6** | 10 Gb/s (55 m) | 100 m (1 Gb/s) | Recommandé pour nouvelles installations |
| **Cat 6a** | 10 Gb/s | 100 m | Datacenters |
| **Cat 7 / Cat 8** | 40 Gb/s | 30 m | Très haut débit (rare) |

**Types de câbles RJ45 :**
- **Câble droit** (Straight-through) : PC ↔ Switch, Switch ↔ Routeur
- **Câble croisé** (Crossover) : PC ↔ PC direct (obsolète grâce à l'Auto-MDIX)

**Limitation :** Distance maximale de **100 mètres**. Au-delà, il faut un équipement intermédiaire (switch) ou passer à la fibre.

---

#### **Fibre Optique**

**Principe :** Transmission de données par **lumière** dans un câble en verre ou plastique.

**Avantages par rapport au cuivre (RJ45) :**
- **Avantage :** **Débits très élevés** : 10 Gb/s, 40 Gb/s, 100 Gb/s, voire plus
- **Avantage :** **Distances énormes** : jusqu'à **plusieurs kilomètres** sans perte de signal
- **Avantage :** **Immunité aux interférences électromagnétiques** (EMI)
- **Avantage :** **Légèreté et finesse** du câble
- **Avantage :** **Sécurité** : impossible d'écouter la ligne par induction électromagnétique

**Inconvénients :**
- **Inconvénient :** Plus **coûteuse** que le cuivre
- **Inconvénient :** Nécessite des équipements spécialisés (switches fibre, transceivers)
- **Inconvénient :** Plus **fragile** (ne pas plier à moins de 3 cm de rayon)

---

**Types de Fibre Optique :**

| **Type** | **Cœur** | **Distance** | **Usage** | **Connecteurs Courants** |
|----------|----------|--------------|-----------|--------------------------|
| **Multimode (MMF)** | 50 ou 62.5 µm | 300 m - 2 km | LAN (campus, datacenters) | LC, SC, ST |
| **Monomode (SMF)** | 9 µm | 10 - 100 km | WAN (liaisons longue distance) | LC, SC |

**Code couleur des câbles fibre :**
- **Jaune** : Monomode (SMF)
- **Orange / Aqua** : Multimode (MMF)

 **À retenir :** 
- **Cuivre (RJ45)** : bon marché, 1 Gb/s, max 100m → LANs bureaux
- **Fibre Optique** : cher, très haut débit, longues distances → Datacenters, liaisons inter-sites

---

## III. Le BIOS/UEFI : Le Gardien du Démarrage

### A. Qu'est-ce que le BIOS/UEFI ?

#### **BIOS (Basic Input/Output System)**

**Définition :** Programme **firmware** (logiciel intégré au matériel) stocké dans une **puce mémoire flash** sur la carte mère, s'exécutant **avant le système d'exploitation**.

**Analogie :** Le BIOS est le **gardien de nuit** de l'immeuble (PC). Avant que l'immeuble ouvre officiellement (démarrage de Windows), le gardien vérifie que tout fonctionne (électricité, portes, alarmes...).

**Rôle principal :**
1. **POST (Power-On Self Test)** : Test automatique de tous les composants au démarrage
2. **Initialisation du matériel** : Configuration des composants (CPU, RAM, disques...)
3. **Lancement du système d'exploitation** : Chargement du bootloader (programme de démarrage de l'OS)

---

#### **UEFI (Unified Extensible Firmware Interface)**

**Définition :** Successeur moderne du BIOS, développé par Intel dans les années 2000.

**Différences BIOS vs UEFI :**

| **Critère** | **BIOS (Legacy)** | **UEFI** |
|-------------|-------------------|----------|
| **Année** | 1975-2000 | 2005-Aujourd'hui |
| **Interface** | Texte uniquement, navigation clavier | **Graphique**, souris supportée |
| **Taille disque max** | 2.2 To (MBR) | >2 To (GPT) |
| **Sécurité** | Faible (pas de vérification) | **Secure Boot** (protection contre rootkits) |
| **Vitesse de démarrage** | Lente | **Rapide** |
| **Support 32/64 bits** | 16 bits | 32 et 64 bits |
| **Réseau** | Non | Oui (PXE boot amélioré) |

 **À retenir :** Tous les PC récents (depuis ~2012) utilisent **UEFI**, mais beaucoup conservent un **mode Legacy** (compatibilité BIOS) pour installer de vieux OS.

---

### B. Accéder au BIOS/UEFI

#### Touches d'Accès selon les Constructeurs

| **Constructeur** | **Touche(s)** | **Alternative** |
|------------------|---------------|-----------------|
| **Dell** | F2 | DEL |
| **HP** | F10 | ESC, puis F10 |
| **Lenovo** | F2 ou F1 | Bouton "Novo" (petits PC portables) |
| **Asus** | DEL | F2 |
| **Acer** | F2 | DEL |
| **MSI** | DEL | F2 |
| **Gigabyte** | DEL | F2 |
| **Autres** | ESC, F12 (menu de boot) | Consulter manuel |

**Astuce :** **Marteler la touche dès l'allumage** du PC (ne pas attendre le logo Windows) !

---

#### Méthode Alternative (Windows 10/11)

**Si le PC démarre trop vite pour intercepter la touche :**

1. Windows → **Paramètres** ()
2. **Mise à jour et sécurité** → **Récupération**
3. **Démarrage avancé** → **Redémarrer maintenant**
4. **Dépannage** → **Options avancées** → **Paramètres du microprogramme UEFI**
5. Clic sur **Redémarrer** → Le PC démarre directement dans l'UEFI

---

### C. Navigation dans le BIOS/UEFI

#### Menus Principaux (Exemple UEFI Typique)

| **Menu** | **Fonction Principale** |
|----------|------------------------|
| **Main / Information** | Informations système (CPU, RAM, version BIOS, date/heure) |
| **Boot** | **Ordre de démarrage** (priorité des périphériques de boot) |
| **Advanced** | Paramètres avancés (virtualisation, overclocking, gestion énergie) |
| **Security** | Mots de passe BIOS, Secure Boot, TPM |
| **Exit** | Sauvegarder et quitter / Quitter sans sauvegarder / Charger les défauts |

---

#### Paramètres Essentiels à Connaître

**1. Date et Heure**
- Impact : Windows et Linux nécessitent une date correcte pour les certificats SSL/TLS
- Réglage : Menu **Main** ou **Advanced → System Time**

**2. Ordre de Boot (Boot Priority)**
- Impact : Définit sur quel périphérique le PC cherche l'OS en premier
- Ordre recommandé classique :
  1. SSD/HDD interne (OS principal)
  2. USB (pour installer un OS ou dépanner)
  3. CD/DVD (rare aujourd'hui)
  4. Réseau (PXE boot - pour installations centralisées)

**3. Virtualization Technology (Intel VT-x / AMD-V)**
- Impact : **Obligatoire** pour utiliser des machines virtuelles (VirtualBox, VMware, Hyper-V)
- Réglage : Menu **Advanced → CPU Configuration**
- **À activer systématiquement** en BTS SIO !

**4. Secure Boot**
- Impact : Empêche le démarrage de logiciels non signés (protection contre rootkits)
- Problème : Peut **bloquer l'installation de Linux** ou de Windows non officiels
- Réglage : Menu **Security → Secure Boot**
- Conseil : **Désactiver** si problème d'installation d'OS

**5. SATA Mode (IDE / AHCI / RAID)**
- Impact : Mode de fonctionnement des disques SATA
- Réglage recommandé : **AHCI** (Advanced Host Controller Interface) pour performances optimales
- **Attention :** Changer ce paramètre sur un Windows déjà installé peut causer un **écran bleu au démarrage** !

---

### D. La Séquence de Démarrage (Boot Sequence)

#### Étapes Détaillées du Démarrage

<figure class="graphe diagramme"><svg viewBox="0 0 760 692" style="max-width:760px" role="img" aria-label="Séquence de démarrage d&#x27;un PC, du bouton Power à l&#x27;écran de connexion" xmlns="http://www.w3.org/2000/svg">
<rect x="70" y="10" width="540" height="30" rx="8" class="dg-box"/>
<circle cx="92" cy="25.0" r="13" class="dg-dark"/>
<text x="92" y="30.0" text-anchor="middle" class="dg-w">1</text>
<text x="118" y="31.0" text-anchor="start" class="dg-t">Appui sur le bouton Power</text>
<line x1="340.0" y1="40" x2="340.0" y2="53.0" class="dg-line"/>
<polygon points="340.0,62 345.0,53.0 335.0,53.0" class="dg-ah"/>
<rect x="70" y="62" width="540" height="56" rx="8" class="dg-box"/>
<circle cx="92" cy="90.0" r="13" class="dg-dark"/>
<text x="92" y="95.0" text-anchor="middle" class="dg-w">2</text>
<text x="118" y="87.0" text-anchor="start" class="dg-t">L&#x27;alimentation envoie le signal « POWER GOOD »</text>
<text x="118" y="107.0" text-anchor="start" class="dg-s">Le CPU démarre</text>
<line x1="340.0" y1="118" x2="340.0" y2="131.0" class="dg-line"/>
<polygon points="340.0,140 345.0,131.0 335.0,131.0" class="dg-ah"/>
<rect x="70" y="140" width="540" height="56" rx="8" class="dg-warn"/>
<circle cx="92" cy="168.0" r="13" class="dg-dark"/>
<text x="92" y="173.0" text-anchor="middle" class="dg-w">3</text>
<text x="118" y="165.0" text-anchor="start" class="dg-t">Le CPU exécute sa première instruction</text>
<text x="118" y="185.0" text-anchor="start" class="dg-s">Chargement du BIOS/UEFI depuis la puce ROM</text>
<line x1="340.0" y1="196" x2="340.0" y2="209.0" class="dg-line"/>
<polygon points="340.0,218 345.0,209.0 335.0,209.0" class="dg-ah"/>
<rect x="70" y="218" width="540" height="74" rx="8" class="dg-warn"/>
<circle cx="92" cy="255.0" r="13" class="dg-dark"/>
<text x="92" y="260.0" text-anchor="middle" class="dg-w">4</text>
<text x="118" y="243.0" text-anchor="start" class="dg-t">POST (Power-On Self Test)</text>
<text x="118" y="263.0" text-anchor="start" class="dg-s">Tests RAM, CPU, clavier ; détection des disques et de la carte graphique</text>
<text x="118" y="281.0" text-anchor="start" class="dg-s">En cas d&#x27;erreur : bips sonores ou message</text>
<line x1="340.0" y1="292" x2="340.0" y2="305.0" class="dg-line"/>
<polygon points="340.0,314 345.0,305.0 335.0,305.0" class="dg-ah"/>
<rect x="70" y="314" width="540" height="56" rx="8" class="dg-warn"/>
<circle cx="92" cy="342.0" r="13" class="dg-dark"/>
<text x="92" y="347.0" text-anchor="middle" class="dg-w">5</text>
<text x="118" y="339.0" text-anchor="start" class="dg-t">Initialisation des périphériques</text>
<text x="118" y="359.0" text-anchor="start" class="dg-s">Carte graphique, clavier, souris...</text>
<line x1="340.0" y1="370" x2="340.0" y2="383.0" class="dg-line"/>
<polygon points="340.0,392 345.0,383.0 335.0,383.0" class="dg-ah"/>
<rect x="70" y="392" width="540" height="56" rx="8" class="dg-warn"/>
<circle cx="92" cy="420.0" r="13" class="dg-dark"/>
<text x="92" y="425.0" text-anchor="middle" class="dg-w">6</text>
<text x="118" y="417.0" text-anchor="start" class="dg-t">Recherche du périphérique bootable</text>
<text x="118" y="437.0" text-anchor="start" class="dg-s">Selon l&#x27;ordre de boot : 1. SSD, 2. USB, 3. CD/DVD, 4. Réseau</text>
<line x1="340.0" y1="448" x2="340.0" y2="461.0" class="dg-line"/>
<polygon points="340.0,470 345.0,461.0 335.0,461.0" class="dg-ah"/>
<rect x="70" y="470" width="540" height="56" rx="8" class="dg-ok"/>
<circle cx="92" cy="498.0" r="13" class="dg-dark"/>
<text x="92" y="503.0" text-anchor="middle" class="dg-w">7</text>
<text x="118" y="495.0" text-anchor="start" class="dg-t">Lecture du MBR ou de la GPT du disque</text>
<text x="118" y="515.0" text-anchor="start" class="dg-s">Chargement du bootloader (Windows Boot Manager, GRUB...)</text>
<line x1="340.0" y1="526" x2="340.0" y2="539.0" class="dg-line"/>
<polygon points="340.0,548 345.0,539.0 335.0,539.0" class="dg-ah"/>
<rect x="70" y="548" width="540" height="56" rx="8" class="dg-ok"/>
<circle cx="92" cy="576.0" r="13" class="dg-dark"/>
<text x="92" y="581.0" text-anchor="middle" class="dg-w">8</text>
<text x="118" y="573.0" text-anchor="start" class="dg-t">Le bootloader charge le noyau de l&#x27;OS</text>
<text x="118" y="593.0" text-anchor="start" class="dg-s">ntoskrnl.exe (Windows), vmlinuz (Linux)</text>
<line x1="340.0" y1="604" x2="340.0" y2="617.0" class="dg-line"/>
<polygon points="340.0,626 345.0,617.0 335.0,617.0" class="dg-ah"/>
<rect x="70" y="626" width="540" height="56" rx="8" class="dg-ok"/>
<circle cx="92" cy="654.0" r="13" class="dg-dark"/>
<text x="92" y="659.0" text-anchor="middle" class="dg-w">9</text>
<text x="118" y="651.0" text-anchor="start" class="dg-t">Chargement de l&#x27;OS complet</text>
<text x="118" y="671.0" text-anchor="start" class="dg-s">Écran de connexion Windows / Linux</text>
<line x1="624" y1="140" x2="624" y2="448" class="dg-line"/>
<line x1="624" y1="140" x2="616" y2="140" class="dg-line"/>
<line x1="624" y1="448" x2="616" y2="448" class="dg-line"/>
<text x="634" y="290.0" text-anchor="start" class="dg-t">BIOS / UEFI</text>
<text x="634" y="308.0" text-anchor="start" class="dg-i">étapes 3 à 6</text>
</svg>
<figcaption>Le BIOS/UEFI intervient dans les étapes 3 à 6 (en orange).</figcaption>
</figure>

*Légende : Séquence complète de démarrage d'un PC, du bouton Power jusqu'à l'écran de connexion de l'OS. Le BIOS/UEFI intervient dans les étapes 3 à 6.*

---

#### Codes BIP (Beep Codes)

Si un problème est détecté durant le POST, le BIOS émet des **bips sonores** pour indiquer l'erreur.

| **Nombre de Bips** | **Signification (Award BIOS)** | **Signification (AMI BIOS)** |
|--------------------|--------------------------------|------------------------------|
| **1 bip court** | POST réussi (normal) | POST réussi |
| **1 bip long** | Problème mémoire (RAM) | Problème mémoire |
| **2 bips courts** | Erreur POST (général) | Erreur RAM |
| **3 bips courts** | Erreur RAM (64 premiers Ko) | Erreur RAM |
| **Bips continus** | Problème alimentation ou carte mère | Problème alimentation |
| **Aucun bip** | Problème haut-parleur, CPU ou carte mère | Problème carte mère ou CPU |

**Important :** Les codes varient selon le fabricant du BIOS (Award, AMI, Phoenix...). Consulter le manuel de la carte mère !
