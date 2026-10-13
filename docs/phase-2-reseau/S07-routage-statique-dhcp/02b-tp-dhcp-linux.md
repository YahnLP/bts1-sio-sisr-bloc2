# TP – Serveur DHCP sous Linux

*BTS SIO SISR 1re année – Bloc 2 – S07*

> [!NOTE]
> **But.** Situation : Deux réseaux doivent se parler, et les postes doivent recevoir leur adresse automatiquement : que configurer, et où ?
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le simulateur réseau ou Packet Tracer servent aux manipulations d'équipements réseau.


## III. TP Linux : Serveur DHCP avec isc-dhcp-server

### A. Installation du Service

```bash
# Mettre à jour la liste des paquets
sudo apt update

# Installer le serveur DHCP
sudo apt install isc-dhcp-server

# Vérifier l'installation
sudo systemctl status isc-dhcp-server
# → Active: failed (normal - pas encore configuré)
```

---

### B. Déclarer l'Interface d'Écoute

Le fichier `/etc/default/isc-dhcp-server` déclare **sur quelle interface** le serveur écoute les requêtes DHCP.

```bash
sudo nano /etc/default/isc-dhcp-server
```

Trouver la ligne `INTERFACESv4` et modifier :

```
# Changer :
INTERFACESv4=""

# En (remplacer eth0 par le nom réel de votre interface) :
INTERFACESv4="eth0"
```

**Trouver le nom de l'interface :**
```bash
ip addr show
# Chercher l'interface avec une IP (souvent eth0, ens33, enp0s3...)
```

---

### C. Configurer le Serveur – `/etc/dhcp/dhcpd.conf`

```bash
# Sauvegarder la config originale
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak

# Éditer la configuration
sudo nano /etc/dhcp/dhcpd.conf
```

**Vider le fichier et entrer la configuration suivante :**

```
# ============================================================
# Paramètres globaux
# ============================================================
option domain-name "bts-sio.local";
option domain-name-servers 8.8.8.8, 8.8.4.4;

default-lease-time 600;          # Bail par défaut : 10 minutes (TP)
max-lease-time 7200;             # Bail maximum : 2 heures

# Ce serveur fait autorité sur son réseau
# (répond aux requêtes même sans bail existant)
authoritative;

# ============================================================
# Étendue (Scope) pour le réseau 192.168.10.0/24
# ============================================================
subnet 192.168.10.0 netmask 255.255.255.0 {
  # Plage d'adresses distribuables
  range 192.168.10.100 192.168.10.150;

  # Options réseau transmises aux clients
  option routers 192.168.10.1;           # Passerelle par défaut
  option subnet-mask 255.255.255.0;      # Masque
  option domain-name-servers 8.8.8.8;   # DNS primaire

  # Durée de bail pour cette étendue (écrase le global)
  default-lease-time 600;
  max-lease-time 3600;
}

# ============================================================
# Réservation : toujours donner 192.168.10.10 à cette MAC
# ============================================================
host PC-Imprimante {
  hardware ethernet aa:bb:cc:dd:ee:ff;   # Remplacer par la vraie MAC
  fixed-address 192.168.10.10;
}
```

---

### D. Démarrer et Vérifier le Service

```bash
# Vérifier la syntaxe AVANT de redémarrer (évite les erreurs silencieuses)
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
# Si OK : "Configuration file errors encountered -- exiting" est absent

# Redémarrer le service
sudo systemctl restart isc-dhcp-server

# Vérifier l'état du service
sudo systemctl status isc-dhcp-server
```

**Sortie attendue (`systemctl status`) si tout va bien :**
```
● isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled)
     Active: active (running) since ...
```

**Si erreur :** Lire les logs :
```bash
journalctl -u isc-dhcp-server -n 30
# Chercher les lignes "error" ou "warning"
```

**Activer au démarrage :**
```bash
sudo systemctl enable isc-dhcp-server
```

---

### E. Tester le Serveur DHCP

#### Option 1 – Test depuis la VM elle-même (sans client séparé)

```bash
# Renouveler le bail DHCP de la VM elle-même (si configurée en DHCP)
sudo dhclient -r eth0    # Libérer l'IP actuelle
sudo dhclient eth0       # Demander une nouvelle IP

# Vérifier l'IP obtenue
ip addr show eth0
```

#### Option 2 – Test depuis une VM cliente (réseau interne VirtualBox)

1. Configurer **2 VM sur le même réseau interne** VirtualBox (Host-Only ou Internal Network)
2. VM Serveur : IP fixe `192.168.10.1/24` sur cette interface
3. VM Cliente : Interface configurée en DHCP (`auto eth0` / `iface eth0 inet dhcp`)
4. Démarrer la VM cliente → elle doit obtenir une IP dans la plage `192.168.10.100-150`

---

### F. Consulter les Baux Actifs

```bash
# Afficher les baux actifs
cat /var/lib/dhcp/dhcpd.leases
```

**Exemple de sortie :**
```
lease 192.168.10.100 {
  starts 3 2024/11/20 10:15:30;
  ends   3 2024/11/20 10:25:30;
  binding state active;
  hardware ethernet aa:bb:cc:11:22:33;
  client-hostname "PC-Client-01";
}
```

---

### G. Commandes de Diagnostic DHCP – Côté Client

```bash
# Linux : demander une IP DHCP manuellement
sudo dhclient eth0

# Linux : libérer l'IP DHCP
sudo dhclient -r eth0

# Linux : voir les baux côté client
cat /var/lib/dhcp/dhclient.leases

# Windows : voir la config IP (dont info DHCP)
ipconfig /all

# Windows : libérer et renouveler
ipconfig /release
ipconfig /renew
```

**J'anticipe.** Si l'étendue ne contient pas l'option `routers`, un client obtient-il une adresse ? Pourra-t-il sortir de son réseau ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

Il obtient bien une adresse (et un masque), mais **sans passerelle** : il joint son réseau local mais pas Internet ni les autres réseaux. Chaque option DHCP (`routers`, `domain-name-servers`) apporte une partie de la configuration.

</details>


---

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant). Le choix des compétences pour cette séance est une proposition de l'enseignant, **à valider**.

**Je sais faire, sans aide :**

- [ ] Lire une table de routage Cisco et distinguer routes connectées et statiques ;
- [ ] Configurer une interface de routeur et des routes statiques (dont la route par défaut) ;
- [ ] Tester la connectivité avec `ping` et `show ip route` ;
- [ ] Expliquer les quatre étapes de DHCP (DORA) ;
- [ ] Installer et configurer un serveur DHCP sous Debian : étendue, options, réservation.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________
