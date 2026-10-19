# Cours – Pare-feu, NAT et DMZ

*BTS SIO SISR 1re année – Bloc 2 – S15 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** adressage IPv4 et passerelle (S04), routage entre deux réseaux (S07), routage inter-VLAN (S09), connexion SSH (S13).
> - **Mots déjà vus :** routeur, table de routage, port, SSH.
> - **Expliqués dans ce cours :** pare-feu avec ou sans état, politique par défaut, zones (interne, DMZ, externe), NAT source et NAT destination, règles `nftables`.
> - **Séance chargée :** le cours est long, mais la partie « règles nftables » (§ 5) peut être relue pendant le TP.

## À la fin de ce cours, tu sauras

- [ ] expliquer ce qu'est un **périmètre de sécurité** et pourquoi on en met plusieurs ;
- [ ] distinguer un filtrage **sans état** d'un filtrage **avec état** ;
- [ ] choisir une **politique par défaut** et la justifier ;
- [ ] dessiner un réseau à **trois zones** (interne, DMZ, externe) et dresser la **matrice des flux autorisés** ;
- [ ] expliquer le **NAT source** (masquerade) et le **NAT destination** (redirection de port) ;
- [ ] lire et écrire une règle **nftables** simple.

---

## 1. Pourquoi filtrer ?

Un réseau d'entreprise ne se protège pas avec une seule barrière. L'annexe II.E du référentiel du BTS cite d'ailleurs la présence de **plusieurs périmètres de sécurité** parmi les attendus d'une infrastructure SISR : on sépare les machines selon leur exposition, et on contrôle ce qui passe d'une zone à l'autre.

Un **pare-feu** (*firewall*) est un équipement ou un logiciel qui **autorise ou bloque** des communications selon des règles : origine, destination, protocole, port, et parfois état de la connexion.

> [!WARNING]
> Un pare-feu n'est qu'une couche. Il ne protège ni d'un mot de passe faible, ni d'un service non mis à jour, ni d'un poste interne infecté. On parle de **défense en profondeur** : filtrage réseau, droits, mises à jour, sauvegardes.

## 2. Sans état ou avec état

| | Sans état (*stateless*) | Avec état (*stateful*) |
|---|---|---|
| **Principe** | Chaque paquet est jugé seul, d'après ses adresses et ses ports | Le pare-feu **mémorise les connexions** et reconnaît les paquets de réponse |
| **Exemple** | Liste de contrôle d'accès d'un routeur | `nftables`, `iptables`, pare-feu des systèmes modernes |
| **Conséquence** | Il faut écrire la règle **aller et retour** | On écrit la règle de l'aller ; le retour est autorisé par `established` |

Avec un pare-feu à état, une connexion a un état : **new** (premier paquet), **established** (échange en cours), **related** (liée à une autre, par exemple une erreur ICMP) ou **invalid** (incohérente). La règle clé d'un pare-feu à état est :

```text
ct state established,related accept
```

**Je vérifie.** Un poste du LAN ouvre une page web. Pourquoi n'a-t-on pas besoin d'une règle « Internet → LAN, port source 443 » avec un pare-feu à état ?

<details markdown="1">
<summary>Voir la réponse</summary>

Parce que la connexion a été **ouverte depuis l'intérieur** : le pare-feu l'a enregistrée comme `new` puis `established`, et laisse passer les paquets de réponse sans règle spécifique. Un pare-feu sans état, lui, aurait besoin d'une règle explicite pour le retour.

</details>

## 3. La politique par défaut

Quand aucune règle ne correspond à un paquet, la **politique par défaut** s'applique :

- **`drop` (tout refuser sauf ce qui est autorisé)** : sûre. On ouvre uniquement ce dont on a besoin ; un oubli casse un service, mais ne crée pas de faille.
- **`accept` (tout autoriser sauf ce qui est interdit)** : pratique, mais un oubli ouvre une porte.

On choisit **`drop` par défaut** dans les chaînes `input` et `forward`, et on liste les flux légitimes. `drop` jette le paquet sans réponse (le client attend jusqu'au délai d'expiration) ; `reject` renvoie un refus explicite (plus rapide pour le client, mais le pare-feu révèle sa présence).

## 4. Zones et DMZ

On regroupe les machines en **zones** selon leur niveau de confiance :

| Zone | Contenu | Confiance |
|---|---|---|
| **Interne (LAN)** | Postes, serveurs de fichiers, annuaire | Élevée |
| **DMZ** (*zone démilitarisée*) | Serveurs **accessibles de l'extérieur** : web, messagerie, relais | Moyenne : exposée, donc surveillée |
| **Externe (WAN)** | Internet | Aucune |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 395.60 356.39" style="max-width:528px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 345.59)">
<g class="node n&#45;ellipse">
<ellipse fill="none" stroke="black" stroke-width="1.8" cx="170" cy="-296.6" rx="100.32" ry="38.37"/>
<text text-anchor="start" x="139.5" y="-301.6" font-family="Helvetica,sans-Serif" font-size="15.00">Internet</text>
<text text-anchor="start" x="121" y="-284.6" font-family="Helvetica,sans-Serif" font-size="15.00">zone externe</text>
</g>
<g class="node n&#45;hex">
<polygon fill="none" stroke="black" stroke-width="1.8" points="308.61,-156.21 239.3,-186.63 100.7,-186.63 31.39,-156.21 100.7,-125.79 239.3,-125.79 308.61,-156.21"/>
<text text-anchor="start" x="104" y="-152.71" font-family="Helvetica,sans-Serif" font-size="15.00">Pare&#45;feu / routeur</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M170,-258.15C170,-235.74 170,-207.65 170,-186.8"/>
<text text-anchor="start" x="170" y="-219.02" font-family="Helvetica,sans-Serif" font-size="13.00">WAN 203.0.113.1/24</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M138,-54C138,-54 12,-54 12,-54 6,-54 0,-48 0,-42 0,-42 0,-12 0,-12 0,-6 6,0 12,0 12,0 138,0 138,0 144,0 150,-6 150,-12 150,-12 150,-42 150,-42 150,-48 144,-54 138,-54"/>
<text text-anchor="start" x="25.5" y="-32" font-family="Helvetica,sans-Serif" font-size="15.00">Poste interne</text>
<text text-anchor="start" x="19" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">192.168.10.10</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M101.95,-125.71C91.14,-117.96 81.43,-108.48 75,-97 67.8,-84.14 67.52,-67.71 69.17,-54.11"/>
<text text-anchor="start" x="75" y="-86.6" font-family="Helvetica,sans-Serif" font-size="13.00">LAN 192.168.10.1/24</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M328,-54C328,-54 202,-54 202,-54 196,-54 190,-48 190,-42 190,-42 190,-12 190,-12 190,-6 196,0 202,0 202,0 328,0 328,0 334,0 340,-6 340,-12 340,-12 340,-42 340,-42 340,-48 334,-54 328,-54"/>
<text text-anchor="start" x="218" y="-32" font-family="Helvetica,sans-Serif" font-size="15.00">Serveur web</text>
<text text-anchor="start" x="209" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">192.168.20.10</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M197.37,-125.73C205.29,-116.74 213.74,-106.67 221,-97 231.23,-83.36 241.54,-67.35 249.6,-54.18"/>
<text text-anchor="start" x="230" y="-86.6" font-family="Helvetica,sans-Serif" font-size="13.00">DMZ 192.168.20.1/24</text>
</g>
</g>
</svg>
</figure>

**Le principe de la DMZ :** si le serveur web est piraté, l'attaquant se retrouve dans la DMZ, pas dans le LAN. Le pare-feu **interdit à la DMZ de joindre le LAN** (ou seulement pour un flux précis, vers un seul serveur).

### La matrice des flux

C'est le document qui précède toute règle : qui peut joindre qui, sur quel port.

| Source ↓ / Destination → | Interne | DMZ | Externe |
|---|---|---|---|
| **Interne** | autorisé | web (80, 443), administration (22) | web, DNS (80, 443, 53) |
| **DMZ** | **refusé** | autorisé | mises à jour seulement |
| **Externe** | **refusé** | web (80 ou 443) uniquement | sans objet |

**Je vérifie.** Le serveur de la DMZ a besoin de se synchroniser avec un serveur de temps sur Internet. Que dit la matrice, et que faut-il faire si le besoin est réel ?

<details markdown="1">
<summary>Voir la réponse</summary>

La matrice autorise « DMZ → Externe : mises à jour seulement ». Un besoin de synchronisation horaire (NTP, port UDP 123) n'y figure pas : il faut l'**ajouter explicitement** à la matrice, avec la destination la plus précise possible, puis écrire la règle correspondante. On ne desserre jamais la politique par défaut.

</details>

## 5. nftables : écrire des règles

`nftables` est le pare-feu du noyau Linux (il remplace `iptables`). Sa structure : des **tables**, qui contiennent des **chaînes**, qui contiennent des **règles**.

| Élément | Rôle |
|---|---|
| **Table** `inet filtre` | Regroupe les règles IPv4 et IPv6 |
| **Chaîne** `input` | Paquets à destination **du pare-feu lui-même** |
| **Chaîne** `forward` | Paquets qui **traversent** le pare-feu (LAN ↔ DMZ ↔ WAN) |
| **Chaîne** `output` | Paquets émis par le pare-feu |
| **Règle** | Un critère et un verdict (`accept`, `drop`, `reject`) |

Exemple de règles, lues de haut en bas, **la première qui correspond s'applique** :

```text
table inet filtre {
  chain forward {
    type filter hook forward priority 0; policy drop;
    ct state established,related accept
    ct state invalid drop
    iifname "enp0s8" oifname "enp0s3" accept
    iifname "enp0s8" oifname "enp0s9" tcp dport { 22, 80, 443 } accept
  }
}
```

| Fragment | Signification |
|---|---|
| `policy drop` | Tout ce qui ne correspond à aucune règle est jeté |
| `ct state established,related accept` | Les réponses aux connexions déjà autorisées passent |
| `iifname` / `oifname` | Interface d'entrée / interface de sortie |
| `tcp dport { 22, 80, 443 }` | Protocole TCP, port de destination dans la liste |

L'**ordre** compte : une règle `accept` placée avant une règle `drop` plus précise la rend inutile.

## 6. Le NAT

Le **NAT** (*network address translation*) modifie les adresses des paquets au passage du pare-feu.

| | NAT source (masquerade) | NAT destination (redirection) |
|---|---|---|
| **Rôle** | Les machines du réseau privé sortent avec l'adresse publique du pare-feu | Un service interne est joint par l'adresse publique, sur un port choisi |
| **Sens** | LAN → Internet | Internet → serveur de la DMZ |
| **Chaîne** | `postrouting` | `prerouting` |
| **Règle** | `oifname "enp0s3" masquerade` | `tcp dport 8080 dnat to 192.168.20.10:80` |

> [!TIP]
> **Le NAT n'est pas un pare-feu.** Il masque les adresses internes, mais c'est le filtrage (`forward`) qui décide de ce qui passe. Une redirection de port sans règle `forward` correspondante ne laisse rien passer, et c'est voulu.

Le routage doit être activé sur le pare-feu pour que les paquets le traversent : `net.ipv4.ip_forward=1`.

## 7. Autres moyens de filtrer

- **Liste de contrôle d'accès (ACL) d'un routeur Cisco** : filtrage sans état, appliqué à une interface (`ip access-group`). On écrit les règles du plus précis au plus général ; une règle `deny ip any any` implicite termine la liste.
- **Pare-feu applicatif** (WAF, proxy) : filtre au niveau du protocole (par exemple HTTP).
- **Pare-feu des postes** (Windows Defender Firewall, géré par GPO) : dernière barrière.

## 8. Vérifier qu'une règle fait ce qu'on croit

Une règle écrite n'est pas une règle **prouvée**. On vérifie :

1. **depuis le bon poste** : `curl`, `ping`, test de port avec `nc -vz hôte port` ;
2. **côté pare-feu** : `sudo nft list ruleset` et les compteurs (`counter`) ;
3. **sur le réseau** : `tcpdump` ou Wireshark, pour voir si le paquet part, arrive, est jeté ou refusé.

**Je vérifie.** Avec Wireshark, comment distingue-t-on un `drop` d'un `reject` ?

<details markdown="1">
<summary>Voir la réponse</summary>

Avec un `drop`, le client envoie un `SYN` qui reste **sans réponse** (il le réémet à plusieurs reprises, puis abandonne). Avec un `reject`, le client reçoit immédiatement un `RST` (TCP) ou un message ICMP « destination injoignable ».

</details>

## À retenir

- On **sépare en zones** et on dresse la **matrice des flux** avant d'écrire une règle.
- **Politique par défaut `drop`**, puis on ouvre le strict nécessaire.
- Un pare-feu **à état** autorise automatiquement le retour des connexions établies.
- **NAT source** pour sortir, **NAT destination** pour publier : le filtrage reste indispensable.
- On **teste** chaque règle, côté client et sur le réseau.
