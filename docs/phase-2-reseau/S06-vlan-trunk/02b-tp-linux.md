# TP – Utilisateurs, groupes et permissions sous Linux

*BTS SIO SISR 1re année – Bloc 2 – S06*

> [!NOTE]
> **But.** Situation : Comment séparer les services d'une entreprise (direction, commercial, informatique) sur les mêmes switchs ?
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le simulateur réseau ou Packet Tracer servent aux manipulations d'équipements réseau.


## Objectif

Créer une organisation utilisateurs/groupes sur Debian (3 services, 6 utilisateurs), configurer des répertoires partagés avec les bonnes permissions.

---

## Scénario

> Le cabinet **TechPro SARL** (client des TP de la phase 2) demande de préparer son serveur Linux pour 3 services :
> - **Direction** (2 utilisateurs) : répertoire `/data/direction`
> - **Commercial** (2 utilisateurs) : répertoire `/data/commercial`
> - **Informatique** (2 utilisateurs) : répertoire `/data/informatique`
>
> Chaque service ne doit accéder qu'à son propre répertoire. L'Informatique doit pouvoir accéder à tous les répertoires (pour les sauvegardes).

---

## Étape 1 : Créer les Groupes

```bash
sudo groupadd direction
sudo groupadd commercial
sudo groupadd informatique

# Vérifier
cat /etc/group | grep -E "direction|commercial|informatique"
```

---

## Étape 2 : Créer les Utilisateurs

```bash
# Direction
sudo useradd -m -s /bin/bash -G direction alice
sudo passwd alice    # MDP : Alice@123

sudo useradd -m -s /bin/bash -G direction bob
sudo passwd bob      # MDP : Bob@123

# Commercial
sudo useradd -m -s /bin/bash -G commercial charlie
sudo passwd charlie  # MDP : Charlie@123

sudo useradd -m -s /bin/bash -G commercial diana
sudo passwd diana    # MDP : Diana@123

# Informatique
sudo useradd -m -s /bin/bash -G informatique eric
sudo passwd eric     # MDP : Eric@123

sudo useradd -m -s /bin/bash -G informatique fiona
sudo passwd fiona    # MDP : Fiona@123
```

**Vérifier :**
```bash
id alice       # uid=...(alice) gid=...(alice) groups=...(alice),...(direction)
cat /etc/passwd | tail -6    # Les 6 derniers utilisateurs créés
```

---

## Étape 3 : Créer les Répertoires et Définir les Permissions

```bash
# Créer les répertoires
sudo mkdir -p /data/direction /data/commercial /data/informatique

# Affecter les groupes
sudo chown root:direction /data/direction
sudo chown root:commercial /data/commercial
sudo chown root:informatique /data/informatique

# Permissions : groupe peut tout (rwx), autres rien
sudo chmod 770 /data/direction
sudo chmod 770 /data/commercial
sudo chmod 770 /data/informatique

# Vérifier
ls -ld /data/direction /data/commercial /data/informatique
```

**Résultat attendu :**
```
drwxrwx--- 2 root direction    4096 nov 15 /data/direction
drwxrwx--- 2 root commercial   4096 nov 15 /data/commercial
drwxrwx--- 2 root informatique 4096 nov 15 /data/informatique
```

**J'anticipe.** Un dossier a les droits `770` (`rwxrwx---`), propriétaire `root`, groupe `direction`. Qui peut y écrire ? Qui peut y entrer ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

`root` et les membres du groupe `direction` ont tous les droits (lecture, écriture, traversée). **Les autres n'ont aucun accès** : pas de lecture, pas d'écriture, pas même d'entrée.

</details>


---

## Étape 4 : Donner Accès Multi-Répertoires à l'Informatique

```bash
# Eric et Fiona (Informatique) doivent accéder à TOUS les répertoires
sudo usermod -aG direction eric
sudo usermod -aG commercial eric
sudo usermod -aG direction fiona
sudo usermod -aG commercial fiona

# Vérifier
groups eric    # eric informatique direction commercial
```

---

## Étape 5 : Tester les Accès

```bash
# Test 1 : alice (direction) accède à son répertoire
su - alice
ls /data/direction    # ✅ Doit fonctionner
ls /data/commercial   # ❌ Doit afficher "Permission non accordée"
exit

# Test 2 : charlie (commercial) accède à son répertoire
su - charlie
echo "Fichier commercial" > /data/commercial/test.txt   # ✅ Doit créer le fichier
ls /data/direction                                       # ❌ Permission refusée
exit

# Test 3 : eric (informatique) accède à tous
su - eric
ls /data/direction    # ✅ Doit fonctionner
ls /data/commercial   # ✅ Doit fonctionner
exit
```

**Je comprends.** Lorsqu'un utilisateur reçoit « Permission denied » sur un dossier, quelles sont les trois choses à vérifier ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

1) le **propriétaire et le groupe** du dossier (`ls -ld`) ; 2) les **droits** pour l'utilisateur, le groupe, les autres ; 3) l'**appartenance de l'utilisateur au groupe** (`id utilisateur`) : après un `usermod -aG`, la session doit être rouverte.

</details>


---

## Étape 6 : Exercices de Permissions

```bash
# 1. Créer un fichier confidentiel accessible uniquement au propriétaire
touch /home/alice/confidentiel.txt
chmod 600 /home/alice/confidentiel.txt
ls -l /home/alice/confidentiel.txt
# → -rw------- alice alice ...

# 2. Créer un script exécutable par tous
touch /usr/local/bin/ping_test.sh
echo '#!/bin/bash' > /usr/local/bin/ping_test.sh
echo 'ping -c 1 8.8.8.8' >> /usr/local/bin/ping_test.sh
chmod 755 /usr/local/bin/ping_test.sh
ls -l /usr/local/bin/ping_test.sh
# → -rwxr-xr-x root root ...

# 3. Répertoire lisible par le groupe, interdit aux autres
mkdir /data/rapports
chown root:direction /data/rapports
chmod 750 /data/rapports
ls -ld /data/rapports
# → drwxr-x--- root direction ...
```

---

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.1** – Concevoir une solution d'infrastructure réseau | | |
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |
| **B3.3** – Sécuriser les équipements et les usages des utilisateurs | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant).

**Je sais faire, sans aide :**

- [ ] Expliquer l'intérêt des VLAN pour la segmentation et la sécurité ;
- [ ] Distinguer port access et port trunk, expliquer le marquage 802.1Q et le VLAN natif ;
- [ ] Créer des VLAN, affecter des ports, configurer un trunk ;
- [ ] Vérifier avec `show vlan` et `show interfaces trunk` ;
- [ ] Gérer utilisateurs, groupes et permissions sous Linux.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________
