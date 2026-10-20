# Cours – Serveur LAMP, boucles Bash, SSH et cron

*BTS SIO SISR 1re année – Bloc 2 – S13 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S05, S06 (terminal et utilisateurs Linux).
> - **Matériel :** VM Debian (serveur) et une seconde VM (client SSH).

## À la fin de ce cours, tu sauras

- [ ] **Écrire** un script Bash : variables, saisie, conditions, boucles ;
- [ ] **Installer** et **vérifier** une pile LAMP (Apache, MariaDB, PHP) et **sécuriser** MariaDB ;
- [ ] **Configurer** SSH par clés et **désactiver** l'authentification par mot de passe ;
- [ ] **Écrire** un script de sauvegarde et **le planifier** avec cron.

---

## PARTIE I — Le Serveur LAMP

### I.A. Qu'est-ce qu'une Pile LAMP ?

**LAMP** est un acronyme désignant un ensemble de 4 logiciels open source qui, combinés, forment une plateforme complète pour héberger des **applications web dynamiques** :

| **Lettre** | **Composant** | **Rôle** |
|---|---|---|
| **L** | **Linux** | Système d'exploitation hôte |
| **A** | **Apache** | Serveur web — reçoit les requêtes HTTP et sert les pages |
| **M** | **MariaDB** (ou MySQL) | Système de gestion de base de données relationnelle |
| **P** | **PHP** | Langage de scripts côté serveur — génère les pages dynamiques |

 **Lien ITIL — Gestion des Services Applicatifs :** Une pile LAMP est un **service IT composite** : sa disponibilité dépend de la disponibilité de chacun de ses 4 composants. En ITIL 4, cela relève de la pratique de **Gestion de la Disponibilité** : il faut surveiller, maintenir et documenter chaque composant pour garantir la continuité du service applicatif.

---

### I.B. Architecture et Flux de Traitement

Voici ce qui se passe quand un utilisateur accède à une page web dynamique hébergée sur un serveur LAMP :

```
   NAVIGATEUR CLIENT
   (ex : Chrome)
         │
         │  1. Requête HTTP GET /index.php
         ▼
   ┌──────────────────────────────────────────────┐
   │              SERVEUR LINUX                   │
   │                                              │
   │  ┌─────────────────────────────────────┐     │
   │  │          APACHE (port 80/443)        │     │
   │  │  Reçoit la requête                  │     │
   │  │  Reconnaît l'extension .php          │     │
   │  │  Transmet à l'interpréteur PHP  ──► │     │
   │  └─────────────────────────────────────┘     │
   │                    │                         │
   │                    ▼                         │
   │  ┌─────────────────────────────────────┐     │
   │  │          PHP (interpréteur)          │     │
   │  │  Exécute le script PHP              │     │
   │  │  Si besoin de données ──────────►   │     │
   │  └─────────────────────────────────────┘     │
   │                    │                         │
   │                    ▼                         │
   │  ┌─────────────────────────────────────┐     │
   │  │          MARIADB (port 3306)         │     │
   │  │  Stocke et retourne les données     │     │
   │  └─────────────────────────────────────┘     │
   │                    │                         │
   │                    │  PHP génère la page HTML │
   │                    ▼                         │
   │  ┌─────────────────────────────────────┐     │
   │  │  Apache envoie la réponse HTTP      │     │
   └──────────────────────────────────────────────┘
         │
         │  2. Réponse HTTP 200 — Page HTML générée
         ▼
   NAVIGATEUR CLIENT (affiche la page)
```

*Légende : Flux de traitement d'une requête sur un serveur LAMP. Le navigateur envoie une requête HTTP à Apache. Apache délègue l'exécution du script PHP à l'interpréteur PHP, qui peut interroger MariaDB pour récupérer des données. PHP génère une page HTML que Apache retourne au navigateur. Le client ne voit jamais le code PHP ni la base de données — il reçoit uniquement du HTML.*

---

### I.C. Apache — Le Serveur Web

**Apache HTTP Server** est le serveur web le plus utilisé au monde. Il écoute sur le port **80 (HTTP)** et/ou **443 (HTTPS)** et sert les fichiers du répertoire web aux clients.

**Fichiers et répertoires importants :**

| **Chemin** | **Rôle** |
|---|---|
| `/etc/apache2/apache2.conf` | Fichier de configuration principal |
| `/etc/apache2/sites-available/` | Fichiers de configuration des sites (VirtualHosts) |
| `/etc/apache2/sites-enabled/` | Liens symboliques vers les sites actifs |
| `/var/www/html/` | Répertoire racine web par défaut (document root) |
| `/var/log/apache2/access.log` | Journal des accès HTTP |
| `/var/log/apache2/error.log` | Journal des erreurs |

**Commandes de gestion Apache :**

```bash
sudo systemctl start apache2      # Démarrer Apache
sudo systemctl stop apache2       # Arrêter Apache
sudo systemctl restart apache2    # Redémarrer (recharge la config)
sudo systemctl reload apache2     # Recharger la config sans couper le service
sudo systemctl status apache2     # Vérifier l'état
sudo systemctl enable apache2     # Démarrage automatique au boot
sudo apache2ctl -t                # Vérifier la syntaxe de la configuration
```

---

### I.D. MariaDB — La Base de Données

**MariaDB** est un système de gestion de base de données relationnelle (SGBDR), fork communautaire de MySQL, 100% compatible. Elle stocke les données sous forme de **tables** dans des **bases de données**.

**Commandes essentielles en console MariaDB :**

```sql
-- Se connecter à MariaDB
sudo mysql -u root -p

-- Afficher les bases de données existantes
SHOW DATABASES;

-- Créer une nouvelle base de données
CREATE DATABASE siosarl_db;

-- Sélectionner une base de données
USE siosarl_db;

-- Créer un utilisateur dédié (bonne pratique : ne jamais utiliser root pour l'appli)
CREATE USER 'webuser'@'localhost' IDENTIFIED BY 'MotDePasse!2024';

-- Accorder les droits sur la base de données
GRANT ALL PRIVILEGES ON siosarl_db.* TO 'webuser'@'localhost';

-- Recharger les droits
FLUSH PRIVILEGES;

-- Afficher les utilisateurs
SELECT user, host FROM mysql.user;

-- Quitter
EXIT;
```

---

### I.E. PHP — Le Langage Côté Serveur

**PHP** (Hypertext Preprocessor) est un langage de script exécuté côté **serveur**. Il génère du HTML dynamique en réponse aux requêtes. Le client ne voit jamais le code PHP.

**Page PHP de test essentielle : `phpinfo()`**

```php
<?php
// Ce fichier sert uniquement à vérifier l'installation PHP
// À SUPPRIMER après validation (sécurité !)
phpinfo();
?>
```

**Exemple de page PHP se connectant à MariaDB :**

```php
<?php
$serveur = "localhost";
$utilisateur = "webuser";
$motdepasse = "MotDePasse!2024";
$base = "siosarl_db";

// Connexion via PDO (méthode recommandée)
try {
    $pdo = new PDO("mysql:host=$serveur;dbname=$base;charset=utf8",
                   $utilisateur, $motdepasse);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "<p style='color:green'>Connexion à MariaDB réussie !</p>";
} catch (PDOException $e) {
    echo "<p style='color:red'>Erreur : " . $e->getMessage() . "</p>";
}
?>
```

---

## PARTIE II — Bash Avancé : Les Boucles

### II.A. La Boucle `for`

La boucle `for` permet de **répéter un bloc d'instructions pour chaque élément d'une liste**.

#### Syntaxe 1 : Itération sur une liste de valeurs

```bash
#!/bin/bash

# ─── Boucle sur une liste statique ─────────────────
for SERVICE in apache2 mariadb php8.2
do
    echo "Vérification du service : $SERVICE"
    if systemctl is-active --quiet "$SERVICE"
    then
        echo "[OK] $SERVICE est actif."
    else
        echo "[ERREUR] $SERVICE est INACTIF !"
    fi
done
```

#### Syntaxe 2 : Itération sur une plage numérique

```bash
#!/bin/bash

# ─── Boucle de 1 à 10 ──────────────────────────────
for i in $(seq 1 10)
do
    echo "Itération numéro : $i"
done

# ─── Syntaxe C-style (plus compacte) ───────────────
for (( i=1; i<=10; i++ ))
do
    echo "i = $i"
done
```

#### Syntaxe 3 : Itération sur les fichiers d'un répertoire

```bash
#!/bin/bash

# ─── Traiter tous les fichiers .log ────────────────
for FICHIER in /var/log/*.log
do
    TAILLE=$(du -sh "$FICHIER" | cut -f1)
    echo "Fichier : $FICHIER — Taille : $TAILLE"
done
```

---

### II.B. La Boucle `while`

La boucle `while` répète un bloc d'instructions **tant qu'une condition est vraie**.

#### Syntaxe de base

```bash
#!/bin/bash

# ─── Compteur simple ───────────────────────────────
COMPTEUR=1

while [ $COMPTEUR -le 5 ]
do
    echo "Tour numéro : $COMPTEUR"
    COMPTEUR=$(( COMPTEUR + 1 ))   # Incrémenter le compteur
done
```

#### Lire un fichier ligne par ligne

```bash
#!/bin/bash

# ─── Lire le fichier /etc/passwd ligne par ligne ───
# C'est le cas d'usage le plus courant en administration système

while IFS= read -r LIGNE
do
    echo "Ligne lue : $LIGNE"
done < /etc/passwd
```

#### Boucle de saisie avec validation

```bash
#!/bin/bash

# ─── Demander jusqu'à obtenir "oui" ou "non" ───────
REPONSE=""

while [ "$REPONSE" != "oui" ] && [ "$REPONSE" != "non" ]
do
    read -p "Confirmez-vous ? (oui/non) : " REPONSE
    if [ "$REPONSE" != "oui" ] && [ "$REPONSE" != "non" ]
    then
        echo "[ATTENTION] Réponse invalide. Tapez 'oui' ou 'non'."
    fi
done

echo "Réponse enregistrée : $REPONSE"
```

---

### II.C. Comparatif `for` vs `while`

| **Situation** | **Boucle à utiliser** | **Pourquoi** |
|---|---|---|
| Traiter une liste connue à l'avance (fichiers, services, utilisateurs) | `for` | La liste est finie et définie |
| Répéter jusqu'à ce qu'une condition change | `while` | La fin dépend d'un état variable |
| Lire un fichier ligne par ligne | `while ... read` | Lecture séquentielle adaptée |
| Répéter N fois exactement | `for` avec `seq` ou C-style | Nombre de tours connu |
| Attendre qu'un service démarre | `while` | Condition inconnue à l'avance |

---

### II.D. Commandes Utiles dans les Scripts d'Administration

| **Commande** | **Usage** | **Exemple** |
|---|---|---|
| `date` | Afficher/formater la date courante | `date "+%Y-%m-%d_%H-%M-%S"` |
| `tar` | Archiver et compresser des fichiers | `tar czf archive.tar.gz /dossier/` |
| `find` | Rechercher des fichiers selon critères | `find /var/log -name "*.log" -mtime +30` |
| `du` | Afficher la taille d'un dossier | `du -sh /var/www/html` |
| `df` | Afficher l'espace disque | `df -h /` |
| `wc` | Compter lignes/mots/caractères | `wc -l /etc/passwd` |
| `cut` | Extraire une colonne d'un fichier | `cut -d: -f1 /etc/passwd` |
| `grep` | Filtrer du texte | `grep "error" /var/log/apache2/error.log` |
| `$(commande)` | Substitution de commande | `DATE=$(date "+%Y%m%d")` |
| `>>` | Redirection en ajout (logs) | `echo "message" >> fichier.log` |
| `2>&1` | Rediriger stderr vers stdout | `commande >> log.txt 2>&1` |

---

## PARTIE III — Administration à Distance Sécurisée

### III.A. SSH — Secure Shell

**SSH (Secure Shell)** est un protocole réseau permettant d'établir une **connexion chiffrée** à un système distant pour l'administrer en ligne de commande. Il remplace les anciens protocoles non sécurisés (Telnet, rsh) en chiffrant intégralement la communication.

- **Port par défaut :** TCP 22
- **Chiffrement :** AES, ChaCha20 (données), RSA/ECDSA/Ed25519 (authentification)
- **Fichier de configuration serveur :** `/etc/ssh/sshd_config`

#### Authentification par Mot de Passe vs Clés

```
   ─── AUTHENTIFICATION PAR MOT DE PASSE (méthode basique) ─────────
   Client ──[connexion]──► Serveur
   Serveur ──[demande MDP]──► Client
   Client ──[envoie MDP chiffré]──► Serveur
   Serveur vérifie le MDP ──► OK ou Refus

   Risques : force brute, phishing, MDP faible ou compromis

   ─── AUTHENTIFICATION PAR CLÉS (méthode recommandée) ─────────────
                    ┌──────────────────┐
                    │   PAIRE DE CLÉS  │
                    │  Générée 1 fois  │
                    └────────┬─────────┘
                             │
              ┌──────────────┴──────────────┐
              ▼                             ▼
    CLÉ PRIVÉE                   CLÉ PUBLIQUE
   Reste sur le CLIENT            Copiée sur le SERVEUR
   NE JAMAIS PARTAGER             dans ~/.ssh/authorized_keys
   (~/.ssh/id_rsa ou id_ed25519)

   Connexion :
   Client ──[je veux me connecter]──► Serveur
   Serveur ──[défi chiffré avec clé publique]──► Client
   Client déchiffre avec sa clé privée ──[réponse]──► Serveur
   Serveur valide ──► Connexion accordée SANS mot de passe
```

*Légende : Comparaison des deux méthodes d'authentification SSH. À gauche, l'authentification par mot de passe — simple mais vulnérable à la force brute. À droite, l'authentification par clés — la clé privée (cadenas fermé) reste sur le client, la clé publique (cadenas ouvert) est déposée sur le serveur. La connexion repose sur un échange cryptographique sans jamais transmettre la clé privée.*

---

#### Commandes SSH Essentielles

```bash
# ─── Côté CLIENT ───────────────────────────────────────────────────

# Générer une paire de clés Ed25519 (algorithme moderne recommandé)
ssh-keygen -t ed25519 -C "utilisateur@siosarl.local"
# → Génère ~/.ssh/id_ed25519 (privée) et ~/.ssh/id_ed25519.pub (publique)

# Copier la clé publique sur le serveur (méthode automatique)
ssh-copy-id utilisateur@192.168.1.10
# → Ajoute la clé dans ~/.ssh/authorized_keys sur le serveur

# Se connecter via SSH
ssh utilisateur@192.168.1.10

# Se connecter sur un port non standard
ssh -p 2222 utilisateur@192.168.1.10

# Exécuter une commande sur le serveur sans ouvrir de shell interactif
ssh utilisateur@192.168.1.10 "df -h"

# ─── Côté SERVEUR — Sécuriser sshd_config ─────────────────────────
# Fichier : /etc/ssh/sshd_config
# Paramètres de sécurité recommandés :

PermitRootLogin no               # Interdire la connexion directe en root
PasswordAuthentication no        # Interdire les connexions par mot de passe
PubkeyAuthentication yes         # Autoriser les clés uniquement
AuthorizedKeysFile .ssh/authorized_keys
MaxAuthTries 3                   # Maximum 3 tentatives
Port 22                          # (optionnel : changer pour un port non standard)

# Redémarrer SSH après modification
sudo systemctl restart ssh
```

 **Règle de sécurité absolue :** La **clé privée** (`id_ed25519`) ne quitte **jamais** le poste client. Si elle est compromise, toutes les connexions sécurisées utilisant cette clé le sont aussi. La clé publique (`id_ed25519.pub`) peut être distribuée librement.

---

### III.B. RDP — Remote Desktop Protocol

**RDP** (Remote Desktop Protocol) est un protocole Microsoft permettant de prendre le **contrôle graphique complet** d'un bureau Windows à distance.

| **Paramètre** | **Valeur** |
|---|---|
| Port par défaut | TCP 3389 |
| Système concerné | Windows (serveur et poste de travail) |
| Activation | Panneau de configuration → Système → Bureau à distance |
| Client Linux | `remmina`, `xrdp` |
| Client Windows | `mstsc.exe` (Remote Desktop Connection) |

**Activer RDP sur Windows Server (PowerShell) :**
```powershell
# Activer le bureau à distance
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' `
    -Name "fDenyTSConnections" -Value 0

# Autoriser RDP dans le pare-feu Windows
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

# Vérifier l'état
Get-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' `
    -Name "fDenyTSConnections"
```

---

### III.C. WinRM — Windows Remote Management

**WinRM** est le protocole Microsoft d'administration à distance **en ligne de commande** (l'équivalent Windows de SSH). Il permet d'exécuter des commandes PowerShell sur des machines Windows distantes.

| **Paramètre** | **Valeur** |
|---|---|
| Ports par défaut | HTTP : TCP 5985 / HTTPS : TCP 5986 |
| Protocole sous-jacent | SOAP/WS-Management |
| Usage principal | Exécution de scripts PowerShell à distance, gestion de parc |
| Outil client | `Enter-PSSession`, `Invoke-Command` (PowerShell) |

**Configurer WinRM (PowerShell administrateur) :**
```powershell
# Activer WinRM sur la machine cible
Enable-PSRemoting -Force

# Se connecter à distance (depuis un autre poste)
Enter-PSSession -ComputerName SERVEUR-WIN -Credential (Get-Credential)

# Exécuter une commande distante sans ouvrir de session interactive
Invoke-Command -ComputerName SERVEUR-WIN -ScriptBlock { Get-Service | Where-Object {$_.Status -eq "Stopped"} }

# Terminer une session distante
Exit-PSSession
```

---

### III.D. Tableau Comparatif des Outils d'Administration Distante

| **Critère** | **SSH** | **RDP** | **WinRM** |
|---|---|---|---|
| **OS cible** | Linux/Unix/Windows | Windows | Windows |
| **Interface** | Ligne de commande | Graphique (bureau complet) | Ligne de commande (PowerShell) |
| **Port** | TCP 22 | TCP 3389 | TCP 5985/5986 |
| **Usage typique** | Admin serveurs Linux, transferts de fichiers (scp/sftp) | Support utilisateur, admin visuelle | Automatisation PowerShell, gestion de parc Windows |
| **Consommation réseau** | Très faible | Élevée (vidéo du bureau) | Faible |
| **Sécurité** | Très bonne (clés) | Bonne (chiffrement TLS) | Bonne (Kerberos/NTLM) |

---

## PARTIE IV — Automatisation avec Cron

### IV.A. Qu'est-ce que Cron ?

**Cron** est le **planificateur de tâches** des systèmes Unix/Linux. Il permet d'exécuter automatiquement des commandes ou des scripts à des moments précis ou à intervalles réguliers.

 **Lien ITIL — Gestion de la Disponibilité :** Cron est l'outil qui permet d'implémenter automatiquement des pratiques essentielles de continuité de service : **sauvegardes nocturnes**, **rotation des logs**, **vérification de santé des services**, **nettoyage automatique de l'espace disque**. Sans cron (ou équivalent), ces tâches reposent sur la mémoire et la disponibilité des techniciens.

---

### IV.B. La Syntaxe Crontab

Chaque ligne d'un fichier crontab définit une tâche planifiée selon la syntaxe :

```
MIN  HEURE  JOUR_MOIS  MOIS  JOUR_SEMAINE  COMMANDE
 │     │        │        │        │
 │     │        │        │        └─ 0=Dimanche, 1=Lundi ... 6=Samedi
 │     │        │        └───────── 1 à 12
 │     │        └────────────────── 1 à 31
 │     └─────────────────────────── 0 à 23
 └───────────────────────────────── 0 à 59
```

**Caractères spéciaux :**

| **Caractère** | **Signification** | **Exemple** |
|---|---|---|
| `*` | Toutes les valeurs | `* * * * *` = chaque minute |
| `,` | Liste de valeurs | `0,30 * * * *` = à H:00 et H:30 |
| `-` | Intervalle | `1-5 * * * *` = minutes 1, 2, 3, 4, 5 |
| `/` | Pas (every) | `*/15 * * * *` = toutes les 15 min |

**Exemples de planifications courantes :**

| **Expression Cron** | **Signification** |
|---|---|
| `0 2 * * *` | Tous les jours à 2h00 du matin |
| `30 23 * * 5` | Tous les vendredis à 23h30 |
| `0 */6 * * *` | Toutes les 6 heures (0h, 6h, 12h, 18h) |
| `0 0 1 * *` | Le 1er de chaque mois à minuit |
| `*/5 * * * *` | Toutes les 5 minutes |
| `0 2 * * 1-5` | Du lundi au vendredi à 2h00 |

```
   EXEMPLES VISUELS
   ─────────────────────────────────────────────
   0 2 * * *    →   [MIN=0] [H=2] [Tj] [Tous mois] [Tous jours semaine]
                    = Tous les jours à 02:00

   */15 * * * * →   [MIN=0,15,30,45] [Toutes les H] [Tj] [Tous mois] [...]
                    = Toutes les 15 minutes

   0 0 1 1 *   →    [MIN=0] [H=0] [J=1] [M=1] [...]
                    = Le 1er janvier à 00:00
   ─────────────────────────────────────────────
```

*Légende : Décomposition visuelle de 3 expressions cron courantes. Chaque champ correspond à une unité de temps (de gauche à droite : minute, heure, jour du mois, mois, jour de la semaine). L'astérisque signifie "toutes les valeurs possibles".*

---

### IV.C. Gérer le Fichier Crontab

```bash
# Éditer le crontab de l'utilisateur courant
crontab -e

# Lister les tâches planifiées de l'utilisateur courant
crontab -l

# Supprimer toutes les tâches de l'utilisateur courant
crontab -r

# Éditer le crontab d'un autre utilisateur (root uniquement)
sudo crontab -u alice -e

# Crontab système (pour les scripts d'administration root)
# Fichier : /etc/crontab (avec un champ UTILISATEUR supplémentaire)
# Format : MIN HEURE JM MOIS JS UTILISATEUR COMMANDE
0 2 * * * root /usr/local/bin/sauvegarde.sh >> /var/log/sauvegarde.log 2>&1
```

---

### IV.D. Bonnes Pratiques Cron

| **Pratique** | **Pourquoi** | **Comment** |
|---|---|---|
| **Toujours utiliser les chemins absolus** | Cron n'a pas le même `$PATH` que votre shell interactif | `/usr/bin/tar` au lieu de `tar` |
| **Rediriger les sorties vers un log** | Par défaut, cron envoie les sorties par email (souvent non configuré) | `>> /var/log/script.log 2>&1` |
| **Horodater les logs** | Savoir quand chaque exécution a eu lieu | `echo "[$(date)] Début..." >> log.txt` |
| **Tester le script manuellement avant** | Un script qui fonctionne en interactif peut échouer dans cron | `sudo /chemin/script.sh` |
| **Ne pas planifier trop tôt après l'heure** | Éviter la charge simultanée si plusieurs serveurs sont planifiés à H:00 | Décaler de quelques minutes : `5 2 * * *` |

---

## V. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|---------------|
| **LAMP** | Pile logicielle Linux + Apache + MariaDB + PHP pour héberger des applications web |
| **Apache** | Serveur web open source — sert les pages HTTP/HTTPS aux navigateurs clients |
| **MariaDB** | Système de gestion de base de données relationnelle (fork de MySQL) |
| **PHP** | Langage de script côté serveur générant des pages HTML dynamiques |
| **VirtualHost** | Configuration Apache permettant d'héberger plusieurs sites sur un seul serveur |
| **PDO** | PHP Data Objects — interface PHP sécurisée pour accéder à une base de données |
| **Boucle `for`** | Structure Bash itérant sur une liste d'éléments connus |
| **Boucle `while`** | Structure Bash répétant un bloc tant qu'une condition est vraie |
| **SSH** | Secure Shell — protocole chiffré d'administration à distance en ligne de commande |
| **Clé privée** | Fichier secret conservé sur le client SSH, ne jamais partager |
| **Clé publique** | Fichier partagé déposé dans `~/.ssh/authorized_keys` sur le serveur SSH |
| **`authorized_keys`** | Fichier sur le serveur SSH listant les clés publiques autorisées à se connecter |
| **RDP** | Remote Desktop Protocol — contrôle graphique distant d'un bureau Windows |
| **WinRM** | Windows Remote Management — exécution de commandes PowerShell sur Windows distant |
| **Cron** | Planificateur de tâches Unix/Linux |
| **Crontab** | Fichier de configuration des tâches planifiées d'un utilisateur |
| **`tar czf`** | Commande de création d'une archive compressée (c=créer, z=gzip, f=fichier) |
| **Rotation de sauvegardes** | Suppression automatique des sauvegardes les plus anciennes pour libérer de l'espace |

---

## VI. Questions de Réflexion

1. **Pourquoi ne faut-il jamais laisser le fichier `phpinfo.php` accessible en production ?**
   - *Piste : Qu'est-ce que cette page révèle sur la configuration du serveur ?*

2. **Pourquoi créer un utilisateur MariaDB dédié pour l'application web plutôt d'utiliser root ?**
   - *Piste : Que se passe-t-il si l'application web est compromise et que la connexion BDD utilise root ?*

3. **Pourquoi l'authentification SSH par clés est-elle plus sécurisée que l'authentification par mot de passe ?**
   - *Piste : Qu'est-ce qu'une attaque par force brute ? Peut-elle fonctionner avec des clés ?*

4. **Dans quel scénario professionnel préférerait-on WinRM à RDP pour administrer un serveur Windows ?**
   - *Piste : Pensez à l'automatisation, à la consommation de bande passante, à la gestion de 50 serveurs simultanément...*

5. **Qu'arrive-t-il si un script de sauvegarde planifié via cron produit une erreur mais ne redirige pas ses sorties vers un log ?**
   - *Piste : Comment sauriez-vous que la sauvegarde a échoué ?*

---

## Auto-évaluation : Suis-je Prêt ?

- [ ] J'installe les 4 composants LAMP et je vérifie que chacun fonctionne
- [ ] Je teste mon installation PHP avec `phpinfo()` et je supprime le fichier ensuite
- [ ] Je me connecte à MariaDB, crée une base de données et un utilisateur dédié
- [ ] J'écris une boucle `for` itérant sur une liste de services et une boucle `for` avec `seq`
- [ ] J'écris une boucle `while` lisant un fichier ligne par ligne
- [ ] Je génère une paire de clés SSH Ed25519 et je comprends la différence privée/publique
- [ ] Je copie une clé publique sur un serveur et je me connecte sans mot de passe
- [ ] J'explique la différence entre RDP et WinRM et leurs cas d'usage respectifs
- [ ] Je déchiffre une expression cron (ex : `0 3 * * 1`)
- [ ] J'écris un script de sauvegarde avec `tar`, horodatage et journalisation
- [ ] Je planifie ce script avec `crontab -e`

---
