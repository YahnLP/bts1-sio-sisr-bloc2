# TP – Premiers scripts Bash

*BTS SIO SISR 1re année – Bloc 2 – S13*

> [!NOTE]
> **But.** Situation : Comment automatiser la création de 30 comptes, héberger un site web et administrer un serveur sans quitter son bureau ?
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le simulateur réseau ou Packet Tracer servent aux manipulations d'équipements réseau.


## PARTIE B — Exercices Bash Progressifs

### B1 — Premier Script : Variables et Echo

Créer un fichier `/home/user/scripts/bonjour.sh` :

```bash
#!/bin/bash
# ─────────────────────────────────────────────────────
# Script  : bonjour.sh
# Auteur  : [Votre nom]
# Date    : [Date]
# Rôle    : Démonstration variables et echo
# ─────────────────────────────────────────────────────

PRENOM="SimIO"
VERSION="1.0"
DATE_SCRIPT=$(date "+%d/%m/%Y à %H:%M")

echo "==================================="
echo "  Bienvenue dans SimIO Admin Tools"
echo "  Version : $VERSION"
echo "  Lancé le : $DATE_SCRIPT"
echo "==================================="
echo ""
echo "Bonjour, $PRENOM ! Ce script est fonctionnel."
```

Tester :
```bash
chmod +x bonjour.sh
./bonjour.sh
```

---

### B2 — Lecture et Conditions

Créer `/home/user/scripts/test_age.sh` :

```bash
#!/bin/bash
# ─────────────────────────────────────────────────────
# Script  : test_age.sh
# Rôle    : Démonstration read + conditions if/elif/else
# ─────────────────────────────────────────────────────

echo "=== Test d'âge ==="
read -p "Entrez votre âge : " AGE

# Vérifier que la saisie n'est pas vide
if [ -z "$AGE" ]
then
    echo "Erreur : aucune valeur saisie."
    exit 1
fi

# Vérifier que c'est un nombre positif
if [ $AGE -lt 0 ]
then
    echo "Erreur : l'âge ne peut pas être négatif."
    exit 1
fi

# Déterminer la catégorie
if [ $AGE -lt 18 ]
then
    echo "Vous êtes mineur ($AGE ans)."
elif [ $AGE -lt 65 ]
then
    echo "Vous êtes un adulte actif ($AGE ans)."
else
    echo "Vous êtes senior ($AGE ans)."
fi
```

---

### B3 — Tester l'Existence d'un Utilisateur

Créer `/home/user/scripts/check_user.sh` :

```bash
#!/bin/bash
# ─────────────────────────────────────────────────────
# Script  : check_user.sh
# Rôle    : Vérifier si un utilisateur Linux existe
# ─────────────────────────────────────────────────────

read -p "Entrez le nom d'utilisateur à vérifier : " LOGIN

if [ -z "$LOGIN" ]
then
    echo "Erreur : login vide."
    exit 1
fi

if id "$LOGIN" > /dev/null 2>&1
then
    echo "✅ L'utilisateur '$LOGIN' existe sur ce système."
    echo "   Détails : $(id $LOGIN)"
else
    echo "❌ L'utilisateur '$LOGIN' n'existe pas sur ce système."
fi
```

**J'anticipe.** Que vaut `$?` après une commande réussie ? et après une commande en échec ? À quoi cela sert-il dans un script ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

`0` si elle a réussi, une valeur **différente de 0** sinon. Un script s'en sert pour **décider** (continuer, afficher une erreur, quitter avec `exit 1`).

</details>


---

## PARTIE C — Script Complet de Création d'Utilisateurs Linux

### Contexte

SimIO SARL vient d'embaucher 5 nouveaux collaborateurs. Vous devez créer leurs comptes Linux sur le serveur de fichiers. Plutôt que de le faire manuellement, vous allez écrire un **script interactif** qui vous pose les questions et crée le compte en toute sécurité.

---

### C1 — Le Fichier d'Entrée

Créer le fichier `/home/user/nouveaux_employes.csv` :

```
prenom,nom,service,login
Alice,Martin,rh,a.martin
Bob,Dupont,informatique,b.dupont
Claire,Durand,direction,c.durand
David,Lemaire,informatique,d.lemaire
Emma,Bernard,rh,e.bernard
```

---

### C2 — Écrire le Script de Création

Créer `/home/user/scripts/creer_utilisateur.sh` :

```bash
#!/bin/bash
# ═══════════════════════════════════════════════════════════════
# Script  : creer_utilisateur.sh
# Auteur  : [Votre nom]
# Date    : [Date]
# Version : 1.0
# Rôle    : Création interactive et sécurisée d'un utilisateur Linux
#           pour SimIO SARL
# Usage   : preuve pour une fiche de réalisation (E6) – B2.2, B2.3
# ═══════════════════════════════════════════════════════════════

# ─── Vérification des droits root ───────────────────────────────
if [ $EUID -ne 0 ]
then
    echo "❌ Erreur : ce script doit être exécuté en tant que root (sudo)."
    exit 1
fi

# ─── En-tête ────────────────────────────────────────────────────
echo "======================================================="
echo "     OUTIL DE CRÉATION D'UTILISATEUR — SimIO SARL"
echo "======================================================="
echo ""

# ─── Saisie des informations ─────────────────────────────────────
read -p "Prénom de l'utilisateur         : " PRENOM
read -p "Nom de famille                  : " NOM
read -p "Login (ex: p.nom)               : " LOGIN
read -p "Service (rh/informatique/direction/commun) : " SERVICE
read -s -p "Mot de passe initial            : " MDP
echo ""

# ─── Validation : champs vides ───────────────────────────────────
if [ -z "$PRENOM" ] || [ -z "$NOM" ] || [ -z "$LOGIN" ] || [ -z "$SERVICE" ] || [ -z "$MDP" ]
then
    echo "❌ Erreur : tous les champs sont obligatoires."
    exit 1
fi

# ─── Validation : le login ne doit pas déjà exister ──────────────
if id "$LOGIN" > /dev/null 2>&1
then
    echo "❌ Erreur : l'utilisateur '$LOGIN' existe déjà sur ce système."
    exit 1
fi

# ─── Validation : le groupe/service doit exister ─────────────────
if ! getent group "$SERVICE" > /dev/null 2>&1
then
    echo "⚠️  Le groupe '$SERVICE' n'existe pas. Création en cours..."
    groupadd "$SERVICE"
    if [ $? -eq 0 ]
    then
        echo "✅ Groupe '$SERVICE' créé."
    else
        echo "❌ Erreur lors de la création du groupe '$SERVICE'."
        exit 1
    fi
fi

# ─── Récapitulatif et confirmation ───────────────────────────────
echo ""
echo "┌──────────────────────────────────────────────┐"
echo "│          RÉCAPITULATIF DE CRÉATION           │"
echo "├──────────────────────────────────────────────┤"
echo "│  Nom complet  : $PRENOM $NOM"
echo "│  Login        : $LOGIN"
echo "│  Service      : $SERVICE"
echo "│  Répertoire   : /home/$LOGIN"
echo "│  Shell        : /bin/bash"
echo "└──────────────────────────────────────────────┘"
echo ""
read -p "Confirmer la création ? (o/n) : " CONFIRMATION

if [ "$CONFIRMATION" != "o" ] && [ "$CONFIRMATION" != "O" ]
then
    echo "❌ Création annulée."
    exit 0
fi

# ─── Création de l'utilisateur ───────────────────────────────────
echo ""
echo "⏳ Création en cours..."

useradd -m \
        -s /bin/bash \
        -c "$PRENOM $NOM" \
        -G "$SERVICE" \
        "$LOGIN"

if [ $? -ne 0 ]
then
    echo "❌ Erreur lors de la création de l'utilisateur."
    exit 1
fi

# ─── Définition du mot de passe ──────────────────────────────────
echo "$LOGIN:$MDP" | chpasswd

if [ $? -ne 0 ]
then
    echo "❌ Erreur lors de la définition du mot de passe."
    exit 1
fi

# ─── Forcer le changement de mot de passe à la 1ère connexion ────
chage -d 0 "$LOGIN"

# ─── Confirmation finale ─────────────────────────────────────────
echo ""
echo "✅ Utilisateur '$LOGIN' ($PRENOM $NOM) créé avec succès !"
echo "   Groupe      : $SERVICE"
echo "   Répertoire  : /home/$LOGIN"
echo "   Mot de passe temporaire défini. L'utilisateur devra le changer à la 1ère connexion."
echo ""
echo "📋 Vérification :"
id "$LOGIN"

# ─── Journalisation ──────────────────────────────────────────────
LOG_FILE="/var/log/creation_utilisateurs.log"
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Création : $LOGIN ($PRENOM $NOM) - Service : $SERVICE" >> "$LOG_FILE"
echo "📄 Événement enregistré dans $LOG_FILE"
```

---

### C3 — Tester le Script

```bash
# Rendre exécutable
chmod +x creer_utilisateur.sh

# Exécuter avec sudo
sudo ./creer_utilisateur.sh
```

**Tests à réaliser et captures à prendre :**

| **Test** | **Données de test** | **Résultat attendu** |
|---|---|---|
| Test 1 — Création normale | Alice Martin / a.martin / rh | ✅ Création réussie |
| Test 2 — Login déjà existant | Relancer avec a.martin | ❌ Message d'erreur "existe déjà" |
| Test 3 — Champ vide | Ne pas saisir le prénom | ❌ Message d'erreur "champs obligatoires" |
| Test 4 — Annulation | Répondre "n" à la confirmation | ❌ Message "Création annulée" |
| Test 5 — Groupe inexistant | Saisir un service "marketing" | ⚠ Groupe créé automatiquement |

---

### C4 — Vérifier les Créations

```bash
# Vérifier que l'utilisateur existe
id a.martin

# Voir le répertoire home créé
ls -la /home/a.martin

# Vérifier l'appartenance aux groupes
groups a.martin

# Consulter le journal de création
sudo cat /var/log/creation_utilisateurs.log
```

---

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |
| **B3.3** – Sécuriser les équipements et les usages des utilisateurs | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant).

**Je sais faire, sans aide :**

- [ ] Écrire un script Bash : variables, saisie, conditions, boucles ;
- [ ] Installer et vérifier une pile LAMP (Apache, MariaDB, PHP) et sécuriser MariaDB ;
- [ ] Configurer SSH par clés et désactiver l'authentification par mot de passe ;
- [ ] Écrire un script de sauvegarde et le planifier avec cron.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________
