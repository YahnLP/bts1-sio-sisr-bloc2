---
author: YLP
title: 📚 FICHE DE COURS
---

# 📚 FICHE DE COURS ÉLÈVE
## "Routage Inter-VLAN & Scripting Bash"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 14*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.2** | Installer et configurer des éléments d'infrastructure réseau |
| **B2.4** | Exploiter un service en mode script |
| **B1.3** | Identifier les processus présents dans un système d'exploitation |
| **B3.3** | Participer à la gestion et au suivi d'un projet (automatisation) |

---

## PARTIE I — Routage Inter-VLAN

### I.A. Rappel : Le Problème de l'Isolation des VLANs

Un **VLAN** (Virtual Local Area Network) crée une **segmentation logique** du réseau : les machines de deux VLANs différents sont dans des sous-réseaux IP distincts et ne peuvent **pas se communiquer directement**, même si elles sont physiquement sur le même switch.

C'est voulu pour la sécurité, mais cela pose un problème pratique : la comptable du VLAN RH doit pouvoir envoyer un email au directeur dans le VLAN Direction !

**La solution : le routage inter-VLAN** — faire passer le trafic d'un VLAN à l'autre via un équipement de niveau 3 (routeur), qui agit comme un **point de contrôle centralisé**.

💡 **Lien ITIL — Gestion de la Disponibilité & Sécurité :** La segmentation en VLANs est une bonne pratique de sécurité (confinement des incidents, isolation des données sensibles). Le routage inter-VLAN permet de délivrer la **connectivité nécessaire aux métiers** tout en maintenant cette segmentation. C'est un équilibre entre disponibilité du service et sécurité.

---

### I.B. Les Trois Méthodes de Routage Inter-VLAN

| **Méthode** | **Description** | **Avantages** | **Inconvénients** |
|-------------|-----------------|--------------|------------------|
| **Un routeur par VLAN** | 1 interface physique du routeur par VLAN | Simple à comprendre | Très coûteux (N interfaces), peu scalable |
| **Router-on-a-Stick** ⭐ | 1 seule interface physique divisée en sous-interfaces logiques | Économique, simple à configurer | Goulot d'étranglement sur le lien trunk en charge |
| **Switch L3 (MLS)** | Routage effectué directement par le switch | Très performant, recommandé en production | Plus coûteux qu'un switch L2 |

*Au BTS SIO SISR, on étudie principalement le **Router-on-a-Stick** (méthode 2).*

---

### I.C. Architecture Router-on-a-Stick

#### Principe

Le routeur est connecté au switch par **un seul câble physique** configuré en mode **trunk** (il transporte plusieurs VLANs grâce au standard 802.1Q). Côté routeur, l'interface physique est subdivisée en **sous-interfaces logiques**, une par VLAN.

```
   PC-RH                   SWITCH                    ROUTEUR
  (VLAN 10)    Fa0/1       [TRUNK]       G0/0         G0/0.10  → Passerelle VLAN 10
192.168.10.10  ───────►  [=======]◄────────────────►  G0/0.20  → Passerelle VLAN 20
                                                       G0/0.30  → Passerelle VLAN 30
   PC-IT       Fa0/2
  (VLAN 20)    ───────►  [=======]
192.168.20.10

   PC-DIR      Fa0/3
  (VLAN 30)    ───────►  [=======]
192.168.30.10
```

*Légende : Schéma d'une architecture router-on-a-stick. Le switch à gauche est connecté au routeur par un seul lien physique trunk (ligne épaisse). Le routeur divise ce lien en 3 sous-interfaces logiques (G0/0.10, G0/0.20, G0/0.30), une par VLAN. Chaque sous-interface devient la passerelle par défaut des machines de son VLAN.*

---

#### Le Standard 802.1Q (Dot1Q)

Lorsqu'un paquet voyage sur un lien trunk, le switch lui ajoute un **tag 802.1Q** — une étiquette de 4 octets insérée dans l'en-tête Ethernet qui indique à quel VLAN appartient ce paquet.

```
   TRAME ETHERNET STANDARD (sans tag)
   ┌────────┬────────┬──────┬──────────┬─────┐
   │  Dest  │  Src   │ Type │  Données │ FCS │
   └────────┴────────┴──────┴──────────┴─────┘

   TRAME ETHERNET AVEC TAG 802.1Q (sur un trunk)
   ┌────────┬────────┬──────────┬──────┬──────────┬─────┐
   │  Dest  │  Src   │ Tag 802.1Q│ Type │  Données │ FCS │
   │        │        │(VLAN ID) │      │          │     │
   └────────┴────────┴──────────┴──────┴──────────┴─────┘
```

*Légende : Comparaison entre une trame Ethernet standard (sans tag, utilisée sur un port d'accès) et une trame Ethernet avec tag 802.1Q (utilisée sur un lien trunk). Le champ Tag 802.1Q de 4 octets contient notamment le VLAN ID (12 bits, permettant de distinguer jusqu'à 4094 VLANs).*

---

### I.D. Cheminement d'un Paquet Inter-VLAN

Traçons le chemin d'un paquet de **PC-RH (VLAN 10)** vers **PC-DIR (VLAN 30)** :

```
 1. PC-RH envoie le paquet vers sa passerelle (192.168.10.1 = G0/0.10 du routeur)
    │
    ▼
 2. Le switch reçoit la trame sur le port Fa0/1 (VLAN 10 access)
    Il ajoute le TAG 802.1Q (VLAN 10) et envoie vers le trunk
    │
    ▼
 3. Le routeur reçoit la trame taggée VLAN 10 sur G0/0.10
    Il "dé-tag" la trame, lit l'adresse IP destination (192.168.30.x)
    Il consulte sa table de routage → destination dans VLAN 30 → sortie G0/0.30
    │
    ▼
 4. Le routeur re-tagge la trame avec VLAN 30
    Il envoie la trame sur le trunk vers le switch
    │
    ▼
 5. Le switch reçoit la trame taggée VLAN 30
    Il retire le tag et envoie vers le port Fa0/3 (VLAN 30 access)
    │
    ▼
 6. PC-DIR reçoit le paquet
```

*Légende : Diagramme de flux montrant le cheminement étape par étape d'un paquet depuis PC-RH (VLAN 10) jusqu'à PC-DIR (VLAN 30). Chaque étape met en évidence le rôle du tag 802.1Q et l'action du routeur (consultation de la table de routage).*

---

### I.E. Configuration Cisco — Commandes Essentielles

#### Côté Switch : Créer les VLANs et configurer le trunk

```
Switch> enable
Switch# configure terminal

! Créer les VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name RH
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Informatique
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name Direction
Switch(config-vlan)# exit

! Assigner les ports d'accès
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# exit

Switch(config)# interface FastEthernet0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 30
Switch(config-if)# exit

! Configurer le port trunk vers le routeur
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q    ! (si requis par le modèle)
Switch(config-if)# exit

Switch(config)# end
Switch# write memory
```

#### Côté Routeur : Créer les sous-interfaces

```
Router> enable
Router# configure terminal

! Activer l'interface physique (sans IP sur l'interface physique elle-même)
Router(config)# interface GigabitEthernet0/0
Router(config-if)# no shutdown
Router(config-if)# exit

! Créer la sous-interface pour VLAN 10
Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

! Créer la sous-interface pour VLAN 20
Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

! Créer la sous-interface pour VLAN 30
Router(config)# interface GigabitEthernet0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# exit

Router(config)# end
Router# write memory
```

📌 **Règle importante :** Le numéro de sous-interface (`.10`, `.20`, `.30`) n'est pas obligatoirement identique au numéro de VLAN — mais c'est une **bonne pratique** de les aligner pour la lisibilité et la maintenance.

#### Commandes de Vérification Essentielles

| **Commande** | **Sur** | **Ce qu'elle affiche** |
|---|---|---|
| `show vlan brief` | Switch | Liste des VLANs et ports associés |
| `show interfaces trunk` | Switch | Interfaces en mode trunk et VLANs autorisés |
| `show ip interface brief` | Routeur | État de toutes les interfaces et sous-interfaces |
| `show ip route` | Routeur | Table de routage (doit contenir les 3 réseaux) |
| `ping 192.168.30.1` | PC source | Test de connectivité vers la passerelle du VLAN cible |

---

## PARTIE II — Scripting Bash

### II.A. Pourquoi Automatiser avec Bash ?

💡 **Lien ITIL — Gestion des Changements & Amélioration Continue :** Dans ITIL 4, l'automatisation des tâches répétitives est un pilier de l'amélioration continue. Créer manuellement 50 utilisateurs Linux prend du temps, génère des erreurs et n'est pas traçable. Un script Bash :
- **Élimine les erreurs humaines** (typos, oublis de paramètres)
- **Garantit la cohérence** (mêmes options pour tous les utilisateurs)
- **Est traçable et versionnable** (on peut auditer ce qui a été fait)
- **Est réutilisable** (on relance le script pour le prochain arrivant)

---

### II.B. Structure d'un Script Bash

Un script Bash est un **fichier texte** contenant une suite de commandes shell, exécutées séquentiellement.

```bash
#!/bin/bash
# ─────────────────────────────────────────────────
# Nom du script  : exemple.sh
# Description    : Démontre la structure d'un script
# Auteur         : Votre Nom
# Date           : 2024-09-01
# Version        : 1.0
# ─────────────────────────────────────────────────

# Ceci est un commentaire (ligne commençant par #)
# Les commentaires sont ignorés à l'exécution

echo "Bonjour, ce script est lancé !"
```

**Éléments fondamentaux :**

| **Élément** | **Syntaxe** | **Description** |
|---|---|---|
| **Shebang** | `#!/bin/bash` | Première ligne obligatoire — indique l'interpréteur à utiliser |
| **Commentaire** | `# texte` | Ligne ignorée à l'exécution — documentation du code |
| **Commande** | `echo "texte"` | Instruction exécutée par le shell |

**Rendre un script exécutable :**
```bash
chmod +x mon_script.sh   # Ajouter le droit d'exécution
./mon_script.sh          # Exécuter le script
```

---

### II.C. Les Variables

Une variable stocke une valeur pour la réutiliser dans le script.

```bash
#!/bin/bash

# ─── Déclaration et affectation ───────────────────
NOM="Alice"              # Pas d'espaces autour du =
AGE=25
REPERTOIRE="/home/alice"

# ─── Utilisation : $ devant le nom de la variable ─
echo "Bonjour $NOM !"
echo "Vous avez $AGE ans."
echo "Votre répertoire est : $REPERTOIRE"

# ─── Variables spéciales ──────────────────────────
echo "Nom du script : $0"
echo "1er argument  : $1"
echo "2ème argument : $2"
echo "Nombre d'args : $#"
echo "Code retour   : $?"   # 0 = succès, autre = erreur
```

**Règles des variables :**

| **Règle** | **Correct ✅** | **Incorrect ❌** |
|---|---|---|
| Pas d'espaces autour de `=` | `NOM="Alice"` | `NOM = "Alice"` |
| `$` pour utiliser une variable | `echo $NOM` | `echo NOM` |
| Guillemets pour les chaînes avec espaces | `PHRASE="Bonjour monde"` | `PHRASE=Bonjour monde` |
| Majuscules pour les variables d'environnement | `PATH`, `HOME` | (convention) |
| Minuscules pour les variables locales | `mon_login` | (convention) |

---

### II.D. Lecture de l'Entrée Utilisateur : `read`

La commande `read` permet de **demander une saisie à l'utilisateur** et de stocker la réponse dans une variable.

```bash
#!/bin/bash

# Syntaxe de base
echo "Entrez votre prénom :"
read PRENOM
echo "Bonjour, $PRENOM !"

# Syntaxe compacte avec -p (prompt)
read -p "Entrez votre nom de famille : " NOM
echo "Identité complète : $PRENOM $NOM"

# Lecture silencieuse (mot de passe) avec -s
read -s -p "Entrez votre mot de passe : " MOT_DE_PASSE
echo ""   # Retour à la ligne après la saisie silencieuse
echo "Mot de passe enregistré (longueur : ${#MOT_DE_PASSE} caractères)"
```

---

### II.E. Affichage : `echo`

```bash
#!/bin/bash

# Affichage simple
echo "Texte simple"

# Affichage avec variable
NOM="Bob"
echo "Bonjour $NOM"

# Affichage sans retour à la ligne (option -n)
echo -n "Chargement... "
echo "Terminé !"

# Affichage avec interprétation des séquences d'échappement (option -e)
echo -e "Ligne 1\nLigne 2\nLigne 3"
echo -e "\t→ Texte indenté"

# Affichage en couleur (codes ANSI)
echo -e "\e[32mTexte en vert\e[0m"
echo -e "\e[31mTexte en rouge\e[0m"
echo -e "\e[1mTexte en gras\e[0m"
```

---

### II.F. Structures Conditionnelles : `if / then / elif / else / fi`

Les conditions permettent d'exécuter des instructions **seulement si une condition est vraie**.

#### Syntaxe Générale

```bash
if [ condition ]
then
    # Instructions si condition vraie
elif [ autre_condition ]
then
    # Instructions si autre_condition vraie
else
    # Instructions si aucune condition n'est vraie
fi
```

> ⚠️ **Attention critique :** Des **espaces** sont **obligatoires** à l'intérieur des crochets `[ ]` : `[ condition ]` fonctionne, `[condition]` génère une erreur.

---

#### Opérateurs de Comparaison

**Comparaisons de chaînes :**

| **Opérateur** | **Signification** | **Exemple** |
|---|---|---|
| `=` | Égal à | `[ "$NOM" = "Alice" ]` |
| `!=` | Différent de | `[ "$NOM" != "Bob" ]` |
| `-z` | Chaîne vide | `[ -z "$NOM" ]` |
| `-n` | Chaîne non vide | `[ -n "$NOM" ]` |

**Comparaisons numériques :**

| **Opérateur** | **Signification** | **Exemple** |
|---|---|---|
| `-eq` | Equal (égal) | `[ $AGE -eq 18 ]` |
| `-ne` | Not Equal (différent) | `[ $AGE -ne 0 ]` |
| `-lt` | Less Than (inférieur) | `[ $AGE -lt 18 ]` |
| `-le` | Less or Equal (inf. ou égal) | `[ $AGE -le 18 ]` |
| `-gt` | Greater Than (supérieur) | `[ $AGE -gt 18 ]` |
| `-ge` | Greater or Equal (sup. ou égal) | `[ $AGE -ge 18 ]` |

**Tests sur les fichiers et répertoires :**

| **Opérateur** | **Signification** | **Exemple** |
|---|---|---|
| `-e` | Existe (fichier ou répertoire) | `[ -e "/etc/passwd" ]` |
| `-f` | Est un fichier régulier | `[ -f "/etc/passwd" ]` |
| `-d` | Est un répertoire | `[ -d "/home/alice" ]` |
| `-r` | Est lisible | `[ -r "fichier.txt" ]` |
| `-w` | Est accessible en écriture | `[ -w "fichier.txt" ]` |

---

#### Exemple Complet

```bash
#!/bin/bash

read -p "Entrez votre âge : " AGE

if [ -z "$AGE" ]
then
    echo "Erreur : vous n'avez pas saisi d'âge."
elif [ $AGE -lt 0 ] || [ $AGE -gt 120 ]
then
    echo "Erreur : âge invalide ($AGE)."
elif [ $AGE -lt 18 ]
then
    echo "Vous êtes mineur ($AGE ans)."
elif [ $AGE -ge 18 ] && [ $AGE -lt 65 ]
then
    echo "Vous êtes majeur actif ($AGE ans)."
else
    echo "Vous êtes senior ($AGE ans)."
fi
```

---

### II.G. Le Code de Retour `$?`

Chaque commande shell retourne un **code de retour** (exit code) :
- **0** = succès
- **Toute autre valeur** = erreur

```bash
#!/bin/bash

# Vérifier si un utilisateur existe déjà
id alice > /dev/null 2>&1

if [ $? -eq 0 ]
then
    echo "L'utilisateur alice existe déjà."
else
    echo "L'utilisateur alice n'existe pas."
fi

# Forme condensée (plus professionnelle)
if id alice > /dev/null 2>&1
then
    echo "L'utilisateur alice existe déjà."
fi
```

📌 **`/dev/null 2>&1` :** Redirige la sortie standard **et** les erreurs vers `/dev/null` (la "poubelle" Linux). On utilise cette redirection pour exécuter une commande silencieusement et n'utiliser que son code de retour.

---

### II.H. Commandes Linux Utiles pour la Gestion des Utilisateurs

| **Commande** | **Description** | **Exemple** |
|---|---|---|
| `useradd` | Créer un utilisateur | `useradd -m -s /bin/bash alice` |
| `passwd` | Définir/changer un mot de passe | `echo "mdp123" \| passwd --stdin alice` |
| `usermod` | Modifier un utilisateur existant | `usermod -aG sudo alice` |
| `userdel` | Supprimer un utilisateur | `userdel -r alice` |
| `groupadd` | Créer un groupe | `groupadd comptabilite` |
| `id` | Afficher l'identité d'un utilisateur | `id alice` |
| `getent passwd` | Lister les utilisateurs du système | `getent passwd` |
| `chown` | Changer le propriétaire d'un fichier | `chown alice:comptabilite /data/compta` |

**Options importantes de `useradd` :**

| **Option** | **Description** | **Exemple** |
|---|---|---|
| `-m` | Créer le répertoire home | `useradd -m alice` |
| `-d` | Spécifier le chemin du home | `useradd -d /data/users/alice alice` |
| `-s` | Spécifier le shell | `useradd -s /bin/bash alice` |
| `-G` | Ajouter à des groupes supplémentaires | `useradd -G sudo,rh alice` |
| `-c` | Commentaire (nom complet) | `useradd -c "Alice Martin" alice` |

---

## III. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|---------------|
| **VLAN** | Virtual LAN — segmentation logique d'un réseau physique |
| **Routage inter-VLAN** | Mécanisme permettant la communication entre machines de VLANs différents via un routeur |
| **Router-on-a-Stick** | Architecture utilisant un seul lien physique trunk et des sous-interfaces logiques pour router entre VLANs |
| **Trunk** | Lien réseau transportant le trafic de plusieurs VLANs simultanément grâce au tagging 802.1Q |
| **802.1Q (Dot1Q)** | Standard IEEE définissant l'encapsulation des trames Ethernet avec un tag VLAN sur un lien trunk |
| **Sous-interface** | Interface logique créée sur une interface physique d'un routeur Cisco (ex : G0/0.10) |
| **Encapsulation** | Action d'ajouter un en-tête (tag 802.1Q) à une trame pour identifier son VLAN |
| **Table de routage** | Table du routeur listant les réseaux connus et l'interface de sortie pour les atteindre |
| **Shebang** | Première ligne d'un script (`#!/bin/bash`) indiquant l'interpréteur à utiliser |
| **Variable Bash** | Espace mémoire nommé stockant une valeur dans un script (`NOM="Alice"`) |
| **`read`** | Commande Bash lisant une entrée clavier et la stockant dans une variable |
| **`echo`** | Commande Bash affichant du texte ou la valeur d'une variable |
| **Structure conditionnelle** | Construction `if/then/else/fi` exécutant des instructions selon une condition |
| **Code de retour (`$?`)** | Valeur numérique retournée par une commande (0 = succès, autre = erreur) |
| **`useradd`** | Commande Linux créant un nouvel utilisateur sur le système |
| **`/dev/null`** | Périphérique spécial Linux servant de "poubelle" pour les sorties non désirées |

---

## IV. Questions de Réflexion

1. **Pourquoi l'interface physique G0/0 du routeur ne doit-elle pas avoir d'adresse IP dans une configuration router-on-a-stick ?**
   - *Piste : C'est la sous-interface qui porte l'IP. Que se passerait-il si l'interface physique avait aussi une IP ?*

2. **Quelle est la principale limite du router-on-a-stick en production dans une grande entreprise ?**
   - *Piste : Pensez à ce qui se passe si 500 machines de 3 VLANs échangent des données simultanément via un seul lien trunk...*

3. **Dans un script Bash, pourquoi est-il important de tester si une variable est vide avant de l'utiliser dans une commande `useradd` ?**
   - *Piste : Que se passe-t-il si on exécute `useradd` avec un login vide ?*

4. **Pourquoi préférer `id $LOGIN > /dev/null 2>&1` plutôt que simplement `id $LOGIN` pour tester si un utilisateur existe ?**
   - *Piste : Pensez à l'expérience utilisateur et à la lisibilité des sorties du script.*

5. **En quoi un script de création d'utilisateurs automatisé est-il préférable à la création manuelle, du point de vue ITIL ?**
   - *Piste : Pensez à la traçabilité, la cohérence, la gestion des changements...*

---

## ✅ Auto-évaluation : Suis-je Prêt ?

- [ ] J'explique pourquoi deux VLANs différents ne peuvent pas communiquer sans routeur
- [ ] Je décris l'architecture router-on-a-stick avec ses composants
- [ ] Je cite les 4 commandes Cisco pour configurer une sous-interface (interface, encapsulation, ip address, exit)
- [ ] Je vérifie ma configuration avec `show ip route` et j'interprète la sortie
- [ ] Je crée un script Bash avec shebang, commentaires, variables et `echo`
- [ ] J'utilise `read -p` pour saisir des données et je les affiche
- [ ] J'écris une condition `if/elif/else/fi` avec des opérateurs corrects
- [ ] Je teste si une variable est vide avec `-z`
- [ ] J'utilise `useradd` avec les options `-m`, `-s`, `-c`, `-G`
- [ ] Je vérife si un utilisateur existe avec `id` et `$?`

---
