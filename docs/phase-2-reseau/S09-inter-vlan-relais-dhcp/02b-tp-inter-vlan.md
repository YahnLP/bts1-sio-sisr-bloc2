# TP – Routage inter-VLAN avec un routeur Cisco

*BTS SIO SISR 1re année – Bloc 2 – S09*

> [!NOTE]
> **But.** Situation : Les VLAN sont isolés, mais le service informatique doit joindre tous les postes et un seul serveur DHCP doit servir tous les réseaux : comment ?
>
> **Comment ce TP fonctionne.** Avant de manipuler, tu écris ce que tu **anticipes** (« J'anticipe »). Puis tu testes et tu notes ce que tu **constates**. Tu **comprends** ce que fait chaque commande (« Je comprends »). Un **bilan de compétences** clôt la séance. Les parties de services (Linux, Windows Server) se font sur des **machines virtuelles réelles** ; le simulateur réseau ou Packet Tracer servent aux manipulations d'équipements réseau.


> [!NOTE]
> **Matériel réel ou simulation.** Si le matériel est disponible, fais la section « Variante sur matériel réel » en fin de TP ; sinon travaille en simulation (Packet Tracer ou simulateur réseau) : le choix revient à l'enseignant. Voir [Matériel réel ou simulation](../../ressources/materiel-reel.md).

## PARTIE A — Routage Inter-VLAN avec Cisco Packet Tracer

### Topologie à Reproduire

Créer dans Packet Tracer la topologie suivante :

```
   PC-RH                 SW-CORE                  R-SIOSARL
 VLAN 10               [Catalyst]                  [Router]
192.168.10.10 ──Fa0/1──►          ──Gig0/1──────► G0/0.10 → 192.168.10.1/24
                                                   G0/0.20 → 192.168.20.1/24
   PC-IT       ──Fa0/2──►                          G0/0.30 → 192.168.30.1/24
 VLAN 20
192.168.20.10

   PC-DIR      ──Fa0/3──►
 VLAN 30
192.168.30.10
```

---

### A1 — Créer la Topologie dans Packet Tracer

1. Ouvrir **Cisco Packet Tracer**
2. Placer les équipements :
   - 1 Switch **Catalyst 2960** (ou équivalent)
   - 1 Routeur **Cisco 1941** (ou équivalent)
   - 3 PC (PC-RH, PC-IT, PC-DIR)
3. Câbler :
   - PC-RH → SW-CORE Fa0/1 (câble droit)
   - PC-IT → SW-CORE Fa0/2 (câble droit)
   - PC-DIR → SW-CORE Fa0/3 (câble droit)
   - SW-CORE Gig0/1 → R-SIOSARL G0/0 (câble droit ou croisé selon les équipements)

---

### A2 — Configurer les Adresses IP des PCs

Configurer les PCs (Desktop → IP Configuration) :

| **PC** | **Adresse IP** | **Masque** | **Passerelle** |
|---|---|---|---|
| PC-RH | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC-IT | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC-DIR | 192.168.30.10 | 255.255.255.0 | 192.168.30.1 |

---

### A3 — Configurer le Switch SW-CORE

Accéder au CLI du switch (clic → CLI) et saisir :

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW-CORE

! ── Création des VLANs ──────────────────────────
SW-CORE(config)# vlan 10
SW-CORE(config-vlan)# name RH
SW-CORE(config-vlan)# exit
SW-CORE(config)# vlan 20
SW-CORE(config-vlan)# name Informatique
SW-CORE(config-vlan)# exit
SW-CORE(config)# vlan 30
SW-CORE(config-vlan)# name Direction
SW-CORE(config-vlan)# exit

! ── Ports d'accès ───────────────────────────────
SW-CORE(config)# interface Fa0/1
SW-CORE(config-if)# switchport mode access
SW-CORE(config-if)# switchport access vlan 10
SW-CORE(config-if)# exit

SW-CORE(config)# interface Fa0/2
SW-CORE(config-if)# switchport mode access
SW-CORE(config-if)# switchport access vlan 20
SW-CORE(config-if)# exit

SW-CORE(config)# interface Fa0/3
SW-CORE(config-if)# switchport mode access
SW-CORE(config-if)# switchport access vlan 30
SW-CORE(config-if)# exit

! ── Port trunk vers le routeur ───────────────────
SW-CORE(config)# interface GigabitEthernet0/1
SW-CORE(config-if)# switchport mode trunk
SW-CORE(config-if)# exit

SW-CORE(config)# end
SW-CORE# write memory
```

✅ **Vérification :** `show vlan brief` — les VLANs 10, 20, 30 doivent apparaître avec leurs ports respectifs.

---

### A4 — Configurer le Routeur R-SIOSARL

```
Router> enable
Router# configure terminal
Router(config)# hostname R-SIOSARL

! ── Interface physique (pas d'IP, juste no shutdown) ──
R-SIOSARL(config)# interface GigabitEthernet0/0
R-SIOSARL(config-if)# no shutdown
R-SIOSARL(config-if)# exit

! ── Sous-interface VLAN 10 ───────────────────────
R-SIOSARL(config)# interface GigabitEthernet0/0.10
R-SIOSARL(config-subif)# encapsulation dot1Q 10
R-SIOSARL(config-subif)# ip address 192.168.10.1 255.255.255.0
R-SIOSARL(config-subif)# exit

! ── Sous-interface VLAN 20 ───────────────────────
R-SIOSARL(config)# interface GigabitEthernet0/0.20
R-SIOSARL(config-subif)# encapsulation dot1Q 20
R-SIOSARL(config-subif)# ip address 192.168.20.1 255.255.255.0
R-SIOSARL(config-subif)# exit

! ── Sous-interface VLAN 30 ───────────────────────
R-SIOSARL(config)# interface GigabitEthernet0/0.30
R-SIOSARL(config-subif)# encapsulation dot1Q 30
R-SIOSARL(config-subif)# ip address 192.168.30.1 255.255.255.0
R-SIOSARL(config-subif)# exit

R-SIOSARL(config)# end
R-SIOSARL# write memory
```

✅ **Vérifications obligatoires :**

```
R-SIOSARL# show ip interface brief
! → G0/0, G0/0.10, G0/0.20, G0/0.30 doivent être "up/up"

R-SIOSARL# show ip route
! → 3 lignes "C" (Connected) pour 192.168.10.0, 192.168.20.0, 192.168.30.0
```

---

### A5 — Tester la Connectivité Inter-VLAN

Depuis **PC-RH** (Desktop → Command Prompt) :

```
C:\> ping 192.168.10.1     → Doit répondre (passerelle VLAN 10)
C:\> ping 192.168.20.1     → Doit répondre (passerelle VLAN 20, test routage)
C:\> ping 192.168.30.1     → Doit répondre (passerelle VLAN 30, test routage)
C:\> ping 192.168.20.10    → Doit répondre (PC-IT, VLAN 20)
C:\> ping 192.168.30.10    → Doit répondre (PC-DIR, VLAN 30)
```

✅ **Point de contrôle :** Capturer les `ping` réussis. Si un `ping` échoue, utiliser `tracert` pour identifier où le paquet est bloqué.

---

### A6 — Compléter la Fiche de Configuration Réseau

Remplir la **Fiche de Configuration Réseau** (voir Annexe 1) avec toutes les informations de votre configuration.

**J'anticipe.** Combien de câbles faut-il entre le switch et le routeur avec la méthode « routeur sur un bras » pour 3 VLAN ? ______________________

<details markdown="1">
<summary>Éléments de réponse</summary>

**Un seul** : un lien trunk, découpé en 3 sous-interfaces logiques côté routeur. Sans cette méthode, il faudrait un câble et une interface par VLAN.

</details>


---

## Variante sur matériel réel

**Matériel :** 1 commutateur (SW-CORE), 1 routeur, 3 PC, 1 câble console, câbles Ethernet.

1. Remettre commutateur et routeur à zéro (`erase startup-config`, `delete flash:vlan.dat`, `reload`).
2. Relever les noms réels des ports et des interfaces (`show interfaces status`, `show ip interface brief`) et adapter les commandes (`Gig0/1`, `G0/0`).
3. Le lien commutateur à routeur est un **trunk**. Sur certains modèles, `switchport trunk encapsulation dot1q` doit précéder `switchport mode trunk`.
4. Sur le routeur : `no shutdown` sur l'interface physique, puis une sous-interface par VLAN avec `encapsulation dot1Q <numéro>`. Si une sous-interface ne répond pas, vérifier que son numéro de VLAN correspond à celui du commutateur.
5. Régler les adresses des PC dans le système, avec la passerelle de chaque VLAN, et autoriser l'ICMP sur le pare-feu Windows.
6. A5 : tester les ping entre VLAN, puis `show ip route` et `show interfaces trunk`.
7. A6 : remplacer le schéma Packet Tracer par une photo du montage ou un schéma de câblage.
8. Remise à zéro en fin de séance.

## Bilan de la séance

Remplis ce bilan **seul**, honnêtement : il sert à décider ce qu'il faut reprendre. Niveau : **Non maîtrisé**, **Maîtrise partielle**, **Bonne maîtrise** ou **Excellente maîtrise** (le vocabulaire des grilles nationales d'évaluation) ; **Non évalué** si tu n'as pas travaillé la compétence aujourd'hui.

| Compétence du référentiel | Ce que j'ai fait dans cette séance | Niveau |
|---|---|---|
| **B2.1** – Concevoir une solution d'infrastructure réseau | | |
| **B2.2** – Installer, tester et déployer une solution d'infrastructure réseau | | |
| **B2.3** – Exploiter, dépanner et superviser une solution d'infrastructure réseau | | |

> [!NOTE]
> Les codes sont ceux du [référentiel](../../ressources/referentiel.md) (l'arrêté numérote des activités : les codes « B… » sont un usage courant).

**Je sais faire, sans aide :**

- [ ] Expliquer pourquoi les VLAN isolent le trafic et pourquoi il faut un routage inter-VLAN ;
- [ ] Configurer un routeur sur un bras (sous-interfaces 802.1Q) ;
- [ ] Configurer l'agent relais DHCP (`ip helper-address`) ;
- [ ] Déployer une infrastructure complète : VLAN, routage, DHCP, DNS ;
- [ ] Diagnostiquer méthodiquement une panne multi-services.

**Ce que je dois reprendre :** ______________________

**Ma question à poser à l'enseignant :** ______________________
