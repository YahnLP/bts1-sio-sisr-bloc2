# TP – Observer le STP sur trois commutateurs

*BTS SIO SISR 1re année – Bloc 2 – S08*

> [!NOTE]
> **But.** Situation : Comment les utilisateurs joignent-ils un serveur par son nom, et pourquoi un câble de secours peut-il faire tomber tout le réseau ?
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le simulateur réseau ou Packet Tracer servent aux manipulations d'équipements réseau.


## IV. TP Cisco – Observer le STP sur 3 Switches

### Topologie

```
        [SW1]
       /     \
  Fa0/1       Fa0/2
     /           \
[SW2]─────────[SW3]
    Fa0/2   Fa0/2
```

Trois switches reliés en **triangle** = 3 liens, donc **1 boucle** → STP doit bloquer 1 port.

---

### Étape 1 : Créer la Topologie dans Packet Tracer (5 min)

1. Placer **3 switches Cisco 2960** (SW1, SW2, SW3)
2. Câbler :
   - SW1 Fa0/1 ↔ SW2 Fa0/1 (câble droit)
   - SW1 Fa0/2 ↔ SW3 Fa0/1 (câble droit)
   - SW2 Fa0/2 ↔ SW3 Fa0/2 (câble droit)
3. Attendre la convergence STP (~30-50 secondes)
4. Observer les voyants : certains doivent être **orange** (Blocking) d'autres **vert** (Forwarding)

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

**Question :** Comment Packet Tracer a-t-il désigné le root bridge ? (priorité égale → MAC la plus basse)

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

Ajouter **1 PC** sur chaque switch :
- PC1 sur SW1 (Fa0/3), PC2 sur SW2 (Fa0/3), PC3 sur SW3 (Fa0/3)
- Configurer les IP : `192.168.1.11/24`, `192.168.1.12/24`, `192.168.1.13/24`

**Tests :**
```
PC1 → ping 192.168.1.12   ✅ Doit répondre (via le chemin STP actif)
PC1 → ping 192.168.1.13   ✅ Doit répondre
```

---

### Étape 5 : Observer la Reconvergence (15 min)

**Simuler une panne :** Dans Packet Tracer, débrancher le câble entre SW1 et SW2 (clic sur le câble → supprimer).

**Observer :**
1. Les voyants changent (orange = STP en convergence)
2. Attendre ~30-50 secondes
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

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |
| **B3.4** – Garantir la disponibilité, l'intégrité et la confidentialité des services informatiques et des données de l'organisation face à des cyberattaques | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant). Le choix des compétences pour cette séance est une proposition de l'enseignant, **à valider**.

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

