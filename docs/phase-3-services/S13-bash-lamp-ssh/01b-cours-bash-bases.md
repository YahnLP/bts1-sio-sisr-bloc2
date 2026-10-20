# Cours – Scripts Bash : les bases

*BTS SIO SISR 1re année – Bloc 2 – S13*

## PARTIE II — Scripting Bash

### II.A. Pourquoi Automatiser avec Bash ?

 **Lien ITIL — Gestion des Changements & Amélioration Continue :** Dans ITIL 4, l'automatisation des tâches répétitives est un pilier de l'amélioration continue. Créer manuellement 50 utilisateurs Linux prend du temps, génère des erreurs et n'est pas traçable. Un script Bash :
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

| **Règle** | **Correct** | **Incorrect** |
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

> **Attention critique :** Des **espaces** sont **obligatoires** à l'intérieur des crochets `[ ]` : `[ condition ]` fonctionne, `[condition]` génère une erreur.

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

 **`/dev/null 2>&1` :** Redirige la sortie standard **et** les erreurs vers `/dev/null` (la "poubelle" Linux). On utilise cette redirection pour exécuter une commande silencieusement et n'utiliser que son code de retour.

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

Réfléchissez à chaque question avant d'ouvrir la réponse.

**1. Pourquoi l'interface physique G0/0 du routeur ne doit-elle pas avoir d'adresse IP dans une configuration router-on-a-stick ?**

*Piste : C'est la sous-interface qui porte l'IP. Que se passerait-il si l'interface physique avait aussi une IP ?*

<details markdown="1">
<summary>Réponse</summary>

Elle ne porte **aucun réseau** : chaque VLAN a sa propre sous-interface avec son adresse de passerelle. Une adresse IP sur l'interface physique ferait écouter le routeur sur les trames **non étiquetées** (VLAN natif) en plus des sous-interfaces : on crée un réseau supplémentaire, sans isolation, avec un risque de conflit d'adressage. L'interface physique doit seulement être activée (`no shutdown`).

</details>

**2. Quelle est la principale limite du router-on-a-stick en production dans une grande entreprise ?**

*Piste : Pensez à ce qui se passe si 500 machines de 3 VLANs échangent des données simultanément via un seul lien trunk...*

<details markdown="1">
<summary>Réponse</summary>

**Un seul lien trunk** transporte tout le trafic inter-VLAN, dans les deux sens : c'est un goulot d'étranglement (500 machines de 3 VLAN saturent vite 1 Gbit/s) et un **point de défaillance unique**. Le routeur traite en plus tous les paquets en logiciel. En production on utilise un **commutateur de niveau 3** (interfaces VLAN, SVI) qui route à la vitesse du matériel.

</details>

**3. Dans un script Bash, pourquoi est-il important de tester si une variable est vide avant de l'utiliser dans une commande `useradd` ?**

*Piste : Que se passe-t-il si on exécute `useradd` avec un login vide ?*

<details markdown="1">
<summary>Réponse</summary>

Avec une variable vide, `useradd` s'exécute sans identifiant et échoue, ou pire, le script poursuit avec un état incohérent (mauvais dossier, fichier inattendu). Un test `[ -z "$LOGIN" ]` permet de **s'arrêter proprement** avec un message clair avant toute modification du système. Entourer les variables de guillemets évite aussi les mauvaises surprises.

</details>

**4. Pourquoi préférer `id $LOGIN > /dev/null 2>&1` plutôt que simplement `id $LOGIN` pour tester si un utilisateur existe ?**

*Piste : Pensez à l'expérience utilisateur et à la lisibilité des sorties du script.*

<details markdown="1">
<summary>Réponse</summary>

On ne s'intéresse qu'au **code retour** de `id` (0 si l'utilisateur existe), pas à son affichage. Rediriger la sortie standard et les erreurs vers `/dev/null` évite d'encombrer l'écran de messages (`uid=1001(...)` ou `no such user`) qui rendraient le script illisible pour l'utilisateur.

</details>

**5. En quoi un script de création d'utilisateurs automatisé est-il préférable à la création manuelle, du point de vue ITIL ?**

*Piste : Pensez à la traçabilité, la cohérence, la gestion des changements...*

<details markdown="1">
<summary>Réponse</summary>

Le script est **reproductible** (même résultat à chaque exécution), **rapide**, **moins sujet aux erreurs de saisie** et **traçable** (journal, versionnage) : c'est ce qu'ITIL attend d'une demande de service standard. Les comptes respectent la convention de nommage, et l'opération est documentée, auditable et facile à confier à un autre technicien.

</details>

---

## Auto-évaluation : Suis-je Prêt ?

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
