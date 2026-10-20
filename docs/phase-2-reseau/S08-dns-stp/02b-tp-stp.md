# TP – Observer le STP sur trois commutateurs

*BTS SIO SISR 1re année – Bloc 2 – S08*

> [!NOTE]
> **But.** Situation : Comment les utilisateurs joignent-ils un serveur par son nom, et pourquoi un câble de secours peut-il faire tomber tout le réseau ?
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le **simulateur réseau** sert aux manipulations d'équipements réseau.


> [!NOTE]
> **Matériel réel ou simulation.** Si le matériel est disponible, fais la section « Variante sur matériel réel » en fin de TP ; sinon travaille dans le **simulateur réseau** : le choix revient à l'enseignant. Cisco Packet Tracer reste possible en complément. Voir [Matériel réel ou simulation](../../ressources/materiel-reel.md).

## IV. TP Cisco – Observer le STP sur 3 Switches

> [!TIP]
> **Ce TP se fait dans le simulateur réseau** (dans le navigateur, sans installation ni Internet).
>
> 1. Télécharge [TP-S08-stp.json](tp/TP-S08-stp.json){ download }.
> 2. Ouvre le [simulateur](../../simulateur/simulateur-reseau.html), clique sur **Ouvrir** et choisis ce fichier : le triangle, les PC et le sujet s'affichent.
> 3. Suis les étapes ci-dessous. **Vérifier mon travail** contrôle le résultat ; **Charger la correction** montre une solution.
>
> **Complément.** Cisco Packet Tracer reste possible : les commandes sont identiques (encadrés « Complément Packet Tracer »). Mode d'emploi : [Utiliser les TP du simulateur](../../ressources/simulateur.md).

### Topologie

<figure class="graphe diagramme"><svg viewBox="0 0 520 260" style="max-width:520px" role="img" aria-label="Triangle de trois switches : une boucle" xmlns="http://www.w3.org/2000/svg">
<rect x="205.0" y="38" width="110" height="24" rx="4" class="dg-dark"/>
<rect x="213.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="225.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="237.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="249.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="261.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="273.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="285.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="297.0" y="46" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<text x="260" y="80" text-anchor="middle" class="dg-s">SW1</text>
<rect x="35.0" y="188" width="110" height="24" rx="4" class="dg-dark"/>
<rect x="43.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="55.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="67.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="79.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="91.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="103.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="115.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="127.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<text x="90" y="230" text-anchor="middle" class="dg-s">SW2</text>
<rect x="375.0" y="188" width="110" height="24" rx="4" class="dg-dark"/>
<rect x="383.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="395.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="407.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="419.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="431.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="443.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="455.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<rect x="467.0" y="196" width="8" height="8" rx="1" fill="#fff" opacity=".85"/>
<text x="430" y="230" text-anchor="middle" class="dg-s">SW3</text>
<line x1="225" y1="60" x2="120" y2="190" class="dg-line"/>
<line x1="295" y1="60" x2="400" y2="190" class="dg-line"/>
<line x1="145" y1="200" x2="375" y2="200" class="dg-line"/>
<text x="165" y="110" text-anchor="end" class="dg-i">Fa0/1</text>
<text x="355" y="110" text-anchor="start" class="dg-i">Fa0/2</text>
<text x="150" y="190" text-anchor="start" class="dg-i">Fa0/2</text>
<text x="370" y="190" text-anchor="end" class="dg-i">Fa0/2</text>
<text x="260" y="240" text-anchor="middle" class="dg-t">3 liens = 1 boucle : STP bloque 1 port</text>
</svg>
<figcaption>Trois liens forment une boucle : le STP doit bloquer un port.</figcaption>
</figure>

Trois switches reliés en **triangle** = 3 liens, donc **1 boucle** → STP doit bloquer 1 port.

---

### Étape 1 : Ouvrir la topologie (5 min)

1. Ouvre le simulateur, clique sur **Ouvrir** et choisis `TP-S08-stp.json`.
2. Vérifie le câblage : SW1 Fa0/1 ↔ SW2 Fa0/1, SW1 Fa0/2 ↔ SW3 Fa0/1, SW2 Fa0/2 ↔ SW3 Fa0/2, et un PC sur Fa0/3 de chaque switch.
3. Laisse converger le STP (clique sur **+1 min**) : certains liens passent à l'orange (blocage), les autres au vert.

<details markdown="1">
<summary>Complément Packet Tracer : créer la topologie</summary>

1. Placer **3 switches Cisco 2960** (SW1, SW2, SW3).
2. Câbler SW1 Fa0/1 ↔ SW2 Fa0/1, SW1 Fa0/2 ↔ SW3 Fa0/1, SW2 Fa0/2 ↔ SW3 Fa0/2 (câbles droits).
3. Attendre la convergence STP (~30-50 secondes) : voyants orange (Blocking) et verts (Forwarding).

</details>

---

### Étape 2 : Identifier le Root Bridge (10 min)

```cisco
! Sur CHAQUE switch, taper :
SW1# show spanning-tree

! Chercher la ligne :
"This bridge is the root"   → ce switch EST le root bridge
OU
"Root ID ... Address xx:xx:xx..."  → adresse du root bridge (différente du switch)
```

 **Tableau à remplir :**

| **Switch** | **Bridge ID (MAC)** | **Priorité** | **Root Bridge ?** |
|------------|--------------------|--------------|--------------------|
| SW1 | | 32769 | |
| SW2 | | 32769 | |
| SW3 | | 32769 | |

**Question :** Comment le root bridge a-t-il été désigné ? (priorité égale → MAC la plus basse)

---

### Étape 3 : Observer les Rôles et États des Ports (10 min)

Sur **chaque switch**, taper `show spanning-tree` et relever :

```cisco
SW2# show spanning-tree

! Chercher la section "Interface" :
Interface        Role  Sts  Cost   Prio.Nbr Type
Fa0/1           Root  FWD  19     128.1    P2p
Fa0/2           Altn  BLK  19     128.2    P2p
```

 **Tableau à remplir :**

| **Switch** | **Interface** | **Rôle (Root/Desg/Altn)** | **État (FWD/BLK)** |
|------------|---------------|--------------------------|---------------------|
| SW1 | Fa0/1 | | |
| SW1 | Fa0/2 | | |
| SW2 | Fa0/1 | | |
| SW2 | Fa0/2 | | |
| SW3 | Fa0/1 | | |
| SW3 | Fa0/2 | | |

**Questions :**
- Quel port est en état BLK (Blocking) ?
- Sur quel switch se trouve-t-il ?
- Pourquoi STP a-t-il choisi CE port à bloquer ?

---

### Étape 4 : Tester la Connectivité (5 min)

Les PC sont déjà placés et adressés : PC1 sur SW1 (Fa0/3), PC2 sur SW2 (Fa0/3), PC3 sur SW3 (Fa0/3), avec `192.168.1.11/24`, `192.168.1.12/24` et `192.168.1.13/24`. Double-clique sur un PC pour ouvrir son terminal.

<details markdown="1">
<summary>Complément Packet Tracer : ajouter les PC</summary>

Ajouter 1 PC sur Fa0/3 de chaque switch et configurer les IP ci-dessus (Desktop → IP Configuration).

</details>

**Tests :**
```
PC1 → ping 192.168.1.12   OK : Doit répondre (via le chemin STP actif)
PC1 → ping 192.168.1.13   OK : Doit répondre
```

---

### Étape 5 : Observer la Reconvergence (15 min)

**Simuler une panne :** dans le simulateur, supprimer le câble entre SW1 et SW2 (clic sur le câble, puis **Supprimer** ; sur Packet Tracer, même geste). Ne pas oublier de **le rebrancher** à la fin de l'étape.

**Observer :**
1. Les voyants changent (orange = STP en convergence)
2. Attendre la reconvergence (**+1 min** dans le simulateur)
3. Retaper `show spanning-tree` sur SW2 et SW3
4. Le port précédemment en BLK doit passer en FWD !

```cisco
SW2# show spanning-tree
! Le port Altn/BLK doit maintenant être Root/FWD
```

 **Questions :**
- Quel port était BLK avant la panne ?
- Quel état a-t-il pris après la panne ?
- Combien de secondes a pris la reconvergence (estimer en observant les voyants) ?

---

### Étape 6 : Forcer le Root Bridge (10 min)

Avant de commencer, vérifie que le câble SW1 ↔ SW2 est bien rebranché.

Modifier la priorité STP pour forcer **SW3** à devenir root bridge :

```cisco
SW3# configure terminal
SW3(config)# spanning-tree vlan 1 priority 4096
SW3(config)# end
```

Attendre la reconvergence, puis vérifier :
```cisco
SW3# show spanning-tree
! "This bridge is the root" doit apparaître sur SW3
```

 **Question :** Pourquoi utilise-t-on une valeur comme 4096 plutôt qu'une valeur quelconque ?
(Réponse : La priorité STP doit être un multiple de 4096)

---

### Étape 7 : Vérifier et enregistrer ton travail

1. Clique sur **Vérifier mon travail** : SW3 doit être root avec la priorité 4096 et les PC doivent se joindre.
2. Clique sur **Enregistrer** et nomme le fichier `NOM_Prenom_S08_TP_STP.json`.

---

## Variante sur matériel réel

**Matériel :** 3 commutateurs, 3 PC, 1 câble console, câbles Ethernet.

> [!WARNING]
> Le triangle de commutateurs forme une **boucle**. Ne relier aucun des trois commutateurs au réseau de l'établissement.

1. Remettre les commutateurs à zéro. Relever les noms réels des ports (`show interfaces status`) et adapter les commandes.
2. Étape 1 : câbler le triangle. Sur du matériel ancien, relier des commutateurs entre eux avec un câble croisé. Les voyants passent à l'orange le temps de la convergence, puis au vert, sauf pour le port bloqué.
3. Étapes 2 à 4 : mêmes commandes (`show spanning-tree`). Comparer les adresses MAC des trois commutateurs pour vérifier l'élection du root bridge.
4. Étape 5 : débrancher réellement le câble entre SW1 et SW2. Lancer avant un `ping` continu entre deux PC et **chronométrer** le temps d'interruption, au lieu de l'estimer d'après les voyants. Avec le STP classique, de l'ordre de la trentaine de secondes ; une valeur très inférieure indique un STP rapide sur ce modèle.
5. Étape 6 : `spanning-tree vlan 1 priority 4096` sur le commutateur choisi, puis relire `show spanning-tree`.
6. Remise à zéro en fin de séance.

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |
| **B3.4** – Garantir la disponibilité, l'intégrité et la confidentialité des services informatiques et des données de l'organisation face à des cyberattaques | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant).

**Je sais faire, sans aide :**

- [ ] Expliquer la résolution de noms (récursive, itérative) ;
- [ ] Distinguer zone directe et zone inverse, lire et créer des enregistrements (A, PTR, CNAME, NS, SOA) ;
- [ ] Installer et configurer Bind9 ; tester avec `dig`, `nslookup`, `host` ;
- [ ] Expliquer le danger des boucles de commutation et le rôle de STP ;
- [ ] Décrire l'élection du root bridge et observer STP sur trois switchs.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________

**J'anticipe.** Parmi trois switchs reliés en triangle, lequel devient le *root bridge* si aucune priorité n'est modifiée ? Combien de liens seront bloqués ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

Celui qui a la **plus petite adresse MAC** (à priorité égale). Dans un triangle, **un seul lien** est bloqué (côté du switch le plus éloigné du root) : un arbre sans boucle relie trois switchs avec deux liens actifs.

</details>

