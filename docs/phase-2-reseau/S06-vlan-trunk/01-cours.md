# Cours – VLAN, trunk 802.1Q et utilisateurs Linux

*BTS SIO SISR 1re année – Bloc 2 – S06 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S05 (CLI Cisco, table MAC).
> - **Matériel :** Simulateur réseau ou Packet Tracer, ou matériel réel (au choix de l'enseignant) ; VM Debian.

## À la fin de ce cours, tu sauras

- [ ] **Expliquer** l'intérêt des VLAN pour la segmentation et la sécurité ;
- [ ] **Distinguer** port access et port trunk, **expliquer** le marquage 802.1Q et le VLAN natif ;
- [ ] **Créer** des VLAN, **affecter** des ports, **configurer** un trunk ;
- [ ] **Vérifier** avec `show vlan` et `show interfaces trunk` ;
- [ ] **Gérer** utilisateurs, groupes et permissions sous Linux.

---

## I. VLANs : Segmentation Logique du Réseau

### A. Définition et Problème Résolu

**VLAN (Virtual Local Area Network)** = Réseau local **logique** créé à l'intérieur d'un switch physique. Un VLAN regroupe des ports de switch qui forment un **domaine de diffusion isolé**, indépendamment du câblage physique.

**Problème sans VLAN :**
- Tous les ports d'un switch = 1 domaine de diffusion unique
- Tout le monde reçoit les broadcasts
- Pas d'isolation entre services → risque de sécurité, performances dégradées

**Solution avec VLANs :**
<figure class="graphe diagramme"><svg viewBox="0 0 680 200" style="max-width:680px" role="img" aria-label="Un switch physique découpé en trois switches logiques (VLAN)" xmlns="http://www.w3.org/2000/svg">
<text x="120" y="22" text-anchor="middle" class="dg-t">Switch physique unique</text>
<text x="440" y="22" text-anchor="middle" class="dg-t">Switches logiques (VLAN)</text>
<rect x="20" y="40" width="200" height="40" rx="8" class="dg-soft"/>
<text x="120.0" y="56.0" text-anchor="middle" class="dg-t">Ports 1 à 4</text>
<text x="120.0" y="74.0" text-anchor="middle" class="dg-s">Direction</text>
<line x1="220" y1="60" x2="311.0" y2="60.0" class="dg-line"/>
<polygon points="320,60 311.0,55.0 311.0,65.0" class="dg-ah"/>
<rect x="320" y="40" width="240" height="40" rx="8" class="dg-box"/>
<text x="440.0" y="65.0" text-anchor="middle" class="dg-t">VLAN 10 - Direction</text>
<rect x="20" y="92" width="200" height="40" rx="8" class="dg-soft"/>
<text x="120.0" y="108.0" text-anchor="middle" class="dg-t">Ports 5 à 8</text>
<text x="120.0" y="126.0" text-anchor="middle" class="dg-s">Commercial</text>
<line x1="220" y1="112" x2="311.0" y2="112.0" class="dg-line"/>
<polygon points="320,112 311.0,107.0 311.0,117.0" class="dg-ah"/>
<rect x="320" y="92" width="240" height="40" rx="8" class="dg-ok"/>
<text x="440.0" y="117.0" text-anchor="middle" class="dg-t">VLAN 20 - Commercial</text>
<rect x="20" y="144" width="200" height="40" rx="8" class="dg-soft"/>
<text x="120.0" y="160.0" text-anchor="middle" class="dg-t">Ports 9 à 12</text>
<text x="120.0" y="178.0" text-anchor="middle" class="dg-s">Informatique</text>
<line x1="220" y1="164" x2="311.0" y2="164.0" class="dg-line"/>
<polygon points="320,164 311.0,159.0 311.0,169.0" class="dg-ah"/>
<rect x="320" y="144" width="240" height="40" rx="8" class="dg-warn"/>
<text x="440.0" y="169.0" text-anchor="middle" class="dg-t">VLAN 30 - Informatique</text>
<path d="M572 44 L582 44 L582 184 L572 184" class="dg-line"/>
<text x="626" y="118" text-anchor="middle" class="dg-t">Isolés</text>
<text x="626" y="136" text-anchor="middle" class="dg-t">entre eux</text>
</svg>
</figure>

---

### B. Avantages des VLANs

| **Avantage** | **Explication** |
|--------------|-----------------|
| **Sécurité** | Isolation logique : Commercial ne voit pas les trames de la Direction |
| **Performance** | Réduction des broadcasts (chaque VLAN = 1 domaine de diffusion distinct) |
| **Flexibilité** | Regrouper des utilisateurs par fonction, pas par emplacement physique |
| **Économie** | Un seul équipement physique pour plusieurs réseaux logiques |
| **Administration** | Changer de service = changer l'affectation du port, pas re-câbler |

---

### C. Types de Ports Switch : Access vs Trunk

#### **Port Access (Port d'Accès)**

- Appartient à **un seul VLAN**
- Connecte les **équipements terminaux** (PC, imprimante, téléphone IP)
- La trame qui sort du port Access est **sans tag VLAN** (l'équipement terminal ne sait pas qu'il est dans un VLAN)

<figure class="graphe diagramme"><svg viewBox="0 0 640 150" style="max-width:640px" role="img" aria-label="Port Access VLAN 10 : la trame sort sans tag" xmlns="http://www.w3.org/2000/svg">
<rect x="50" y="40" width="40" height="28" rx="4" class="dg-box"/>
<rect x="55" y="45" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="67" y="68" width="6" height="6" class="dg-box"/>
<rect x="58" y="74" width="24" height="4" rx="2" class="dg-box"/>
<text x="70" y="94" text-anchor="middle" class="dg-s">PC Direction</text>
<rect x="460" y="34" width="120" height="56" rx="8" class="dg-dark"/>
<text x="520.0" y="67.0" text-anchor="middle" class="dg-w">Switch</text>
<line x1="92" y1="56" x2="460" y2="62" class="dg-line"/>
<rect x="250" y="22" width="140" height="28" rx="6" class="dg-ok"/>
<text x="320.0" y="41.0" text-anchor="middle" class="dg-t">Port Access</text>
<text x="320" y="78" text-anchor="middle" class="dg-t">trame sans tag</text>
<text x="320" y="96" text-anchor="middle" class="dg-i">le PC ignore l&#x27;existence du VLAN</text>
<text x="520" y="112" text-anchor="middle" class="dg-i">ce port = VLAN 10</text>
<text x="520" y="128" text-anchor="middle" class="dg-i">(le switch le sait)</text>
</svg>
<figcaption>Un port Access appartient à un seul VLAN ; le tag n&#x27;existe que sur les liens trunk.</figcaption>
</figure>

---

#### **Port Trunk (Port de Liaison)**

- Transporte **plusieurs VLANs simultanément**
- Connecte les **switches entre eux** ou un switch à un routeur
- Chaque trame est **taguée** avec le numéro de VLAN (sauf VLAN natif)

<figure class="graphe diagramme"><svg viewBox="0 0 720 210" style="max-width:720px" role="img" aria-label="Liaison trunk 802.1Q : les trames portent une étiquette de VLAN, sauf le VLAN natif" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="20" width="90" height="170" rx="8" class="dg-dark"/>
<text x="55.0" y="110.0" text-anchor="middle" class="dg-w">Switch A</text>
<rect x="620" y="20" width="90" height="170" rx="8" class="dg-dark"/>
<text x="665.0" y="110.0" text-anchor="middle" class="dg-w">Switch B</text>
<rect x="230" y="14" width="260" height="182" rx="10" class="dg-soft"/>
<text x="360" y="34" text-anchor="middle" class="dg-t">Trunk 802.1Q</text>
<text x="166" y="63" text-anchor="middle" class="dg-s">Port VLAN 10</text>
<line x1="102" y1="69" x2="229.0" y2="69.0" class="dg-line"/>
<polygon points="238,69 229.0,64.0 229.0,74.0" class="dg-ah"/>
<rect x="238" y="52" width="244" height="34" rx="8" class="dg-box"/>
<text x="360.0" y="74.0" text-anchor="middle" class="dg-t">[Tag VLAN 10] données</text>
<line x1="482" y1="69" x2="609.0" y2="69.0" class="dg-line"/>
<polygon points="618,69 609.0,64.0 609.0,74.0" class="dg-ah"/>
<text x="166" y="111" text-anchor="middle" class="dg-s">Port VLAN 20</text>
<line x1="102" y1="117" x2="229.0" y2="117.0" class="dg-line"/>
<polygon points="238,117 229.0,112.0 229.0,122.0" class="dg-ah"/>
<rect x="238" y="100" width="244" height="34" rx="8" class="dg-ok"/>
<text x="360.0" y="122.0" text-anchor="middle" class="dg-t">[Tag VLAN 20] données</text>
<line x1="482" y1="117" x2="609.0" y2="117.0" class="dg-line"/>
<polygon points="618,117 609.0,112.0 609.0,122.0" class="dg-ah"/>
<text x="166" y="159" text-anchor="middle" class="dg-s">Port VLAN 1</text>
<line x1="102" y1="165" x2="229.0" y2="165.0" class="dg-line"/>
<polygon points="238,165 229.0,160.0 229.0,170.0" class="dg-ah"/>
<rect x="238" y="148" width="244" height="34" rx="8" class="dg-warn"/>
<text x="360.0" y="170.0" text-anchor="middle" class="dg-t">sans tag (VLAN natif)</text>
<line x1="482" y1="165" x2="609.0" y2="165.0" class="dg-line"/>
<polygon points="618,165 609.0,160.0 609.0,170.0" class="dg-ah"/>
</svg>
</figure>

---

### D. Le Protocole 802.1Q (Dot1Q) : Marquage des Trames

**Principe :** Quand une trame traverse un trunk, le switch y insère un **tag de 4 octets** (32 bits) entre l'en-tête Ethernet et les données.

**Structure du tag 802.1Q :**
<figure class="graphe diagramme"><svg viewBox="0 0 760 330" style="max-width:760px" role="img" aria-label="Trame Ethernet normale et trame 802.1Q" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="28" text-anchor="start" class="dg-t">Trame Ethernet normale</text>
<rect x="20" y="40" width="100" height="40" rx="5" class="dg-box"/>
<text x="70.0" y="56.0" text-anchor="middle" class="dg-t">MAC dest.</text>
<text x="70.0" y="74.0" text-anchor="middle" class="dg-s">6 octets</text>
<rect x="124" y="40" width="100" height="40" rx="5" class="dg-box"/>
<text x="174.0" y="56.0" text-anchor="middle" class="dg-t">MAC src.</text>
<text x="174.0" y="74.0" text-anchor="middle" class="dg-s">6 octets</text>
<rect x="228" y="40" width="90" height="40" rx="5" class="dg-box"/>
<text x="273.0" y="56.0" text-anchor="middle" class="dg-t">EtherType</text>
<text x="273.0" y="74.0" text-anchor="middle" class="dg-s">2 octets</text>
<rect x="322" y="40" width="220" height="40" rx="5" class="dg-soft"/>
<text x="432.0" y="56.0" text-anchor="middle" class="dg-t">Données</text>
<text x="432.0" y="74.0" text-anchor="middle" class="dg-s">46 à 1500 octets</text>
<rect x="546" y="40" width="70" height="40" rx="5" class="dg-box"/>
<text x="581.0" y="56.0" text-anchor="middle" class="dg-t">FCS</text>
<text x="581.0" y="74.0" text-anchor="middle" class="dg-s">4 octets</text>
<text x="20" y="118" text-anchor="start" class="dg-t">Trame 802.1Q (sur un trunk)</text>
<rect x="20" y="130" width="100" height="40" rx="5" class="dg-box"/>
<text x="70.0" y="146.0" text-anchor="middle" class="dg-t">MAC dest.</text>
<text x="70.0" y="164.0" text-anchor="middle" class="dg-s">6 octets</text>
<rect x="124" y="130" width="100" height="40" rx="5" class="dg-box"/>
<text x="174.0" y="146.0" text-anchor="middle" class="dg-t">MAC src.</text>
<text x="174.0" y="164.0" text-anchor="middle" class="dg-s">6 octets</text>
<rect x="228" y="130" width="110" height="40" rx="5" class="dg-warn"/>
<text x="283.0" y="146.0" text-anchor="middle" class="dg-t">Tag 802.1Q</text>
<text x="283.0" y="164.0" text-anchor="middle" class="dg-s">4 octets</text>
<rect x="342" y="130" width="90" height="40" rx="5" class="dg-box"/>
<text x="387.0" y="146.0" text-anchor="middle" class="dg-t">EtherType</text>
<text x="387.0" y="164.0" text-anchor="middle" class="dg-s">2 octets</text>
<rect x="436" y="130" width="170" height="40" rx="5" class="dg-soft"/>
<text x="521.0" y="146.0" text-anchor="middle" class="dg-t">Données</text>
<text x="521.0" y="164.0" text-anchor="middle" class="dg-s">46 à 1500 octets</text>
<rect x="610" y="130" width="70" height="40" rx="5" class="dg-box"/>
<text x="645.0" y="146.0" text-anchor="middle" class="dg-t">FCS</text>
<text x="645.0" y="164.0" text-anchor="middle" class="dg-s">4 octets</text>
<line x1="224" y1="172" x2="224" y2="196" class="dg-dash"/>
<line x1="334" y1="172" x2="334" y2="196" class="dg-dash"/>
<line x1="224" y1="196" x2="150" y2="214" class="dg-dash"/>
<line x1="334" y1="196" x2="610" y2="214" class="dg-dash"/>
<rect x="150" y="214" width="130" height="58" rx="5" class="dg-box"/>
<text x="215.0" y="230.0" text-anchor="middle" class="dg-t">TPID</text>
<text x="215.0" y="248.0" text-anchor="middle" class="dg-s">0x8100</text>
<text x="215.0" y="266.0" text-anchor="middle" class="dg-s">identifie le tag</text>
<rect x="284" y="214" width="100" height="58" rx="5" class="dg-box"/>
<text x="334.0" y="230.0" text-anchor="middle" class="dg-t">PCP</text>
<text x="334.0" y="248.0" text-anchor="middle" class="dg-s">3 bits</text>
<text x="334.0" y="266.0" text-anchor="middle" class="dg-s">priorité (QoS)</text>
<rect x="388" y="214" width="100" height="58" rx="5" class="dg-box"/>
<text x="438.0" y="230.0" text-anchor="middle" class="dg-t">DEI</text>
<text x="438.0" y="248.0" text-anchor="middle" class="dg-s">1 bit</text>
<text x="438.0" y="266.0" text-anchor="middle" class="dg-s">éligible au rejet</text>
<rect x="492" y="214" width="130" height="58" rx="5" class="dg-warn"/>
<text x="557.0" y="230.0" text-anchor="middle" class="dg-t">VID</text>
<text x="557.0" y="248.0" text-anchor="middle" class="dg-s">12 bits</text>
<text x="557.0" y="266.0" text-anchor="middle" class="dg-s">n° de VLAN (1 à 4094)</text>
<text x="380" y="298" text-anchor="middle" class="dg-i">12 bits : 4096 valeurs, 0 et 4095 réservés, donc 4094 VLAN utilisables</text>
</svg>
<figcaption>Le tag 802.1Q ajoute 4 octets entre l&#x27;adresse source et l&#x27;EtherType.</figcaption>
</figure>

**VID (VLAN Identifier) :** 12 bits → 4096 VLANs possibles (0 et 4095 réservés → 4094 utilisables).

---

### E. Le VLAN Natif

**Définition :** Le VLAN natif est le seul VLAN dont les trames traversent le trunk **sans tag**.

- Valeur par défaut Cisco : **VLAN 1**
- Les deux extrémités du trunk doivent avoir le **même VLAN natif**
- Si mismatch : CDP (Cisco Discovery Protocol) affiche une erreur

**Bonne pratique :** Changer le VLAN natif (pas VLAN 1) et n'y mettre aucun utilisateur.

<figure class="graphe diagramme"><svg viewBox="0 0 760 240" style="max-width:760px" role="img" aria-label="Trunk 802.1Q entre deux switches : tags par VLAN" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="30" width="90" height="170" rx="8" class="dg-dark"/>
<text x="55.0" y="120.0" text-anchor="middle" class="dg-w">Switch A</text>
<rect x="660" y="30" width="90" height="170" rx="8" class="dg-dark"/>
<text x="705.0" y="120.0" text-anchor="middle" class="dg-w">Switch B</text>
<rect x="215" y="14" width="330" height="200" rx="10" class="dg-soft"/>
<text x="380" y="36" text-anchor="middle" class="dg-t">Lien trunk 802.1Q</text>
<rect x="235" y="56" width="290" height="38" rx="5" class="dg-warn"/>
<text x="380.0" y="80.0" text-anchor="middle" class="dg-t">Tag VLAN 10 + données</text>
<line x1="102" y1="75" x2="224.0" y2="75.0" class="dg-line"/>
<polygon points="233,75 224.0,70.0 224.0,80.0" class="dg-ah"/>
<line x1="527" y1="75" x2="649.0" y2="75.0" class="dg-line"/>
<polygon points="658,75 649.0,70.0 649.0,80.0" class="dg-ah"/>
<text x="110" y="68" text-anchor="start" class="dg-s">Port VLAN 10</text>
<text x="650" y="68" text-anchor="end" class="dg-s">Port VLAN 10</text>
<rect x="235" y="108" width="290" height="38" rx="5" class="dg-ok"/>
<text x="380.0" y="132.0" text-anchor="middle" class="dg-t">Tag VLAN 20 + données</text>
<line x1="102" y1="127" x2="224.0" y2="127.0" class="dg-line"/>
<polygon points="233,127 224.0,122.0 224.0,132.0" class="dg-ah"/>
<line x1="527" y1="127" x2="649.0" y2="127.0" class="dg-line"/>
<polygon points="658,127 649.0,122.0 649.0,132.0" class="dg-ah"/>
<text x="110" y="120" text-anchor="start" class="dg-s">Port VLAN 20</text>
<text x="650" y="120" text-anchor="end" class="dg-s">Port VLAN 20</text>
<rect x="235" y="160" width="290" height="38" rx="5" class="dg-box"/>
<text x="380.0" y="184.0" text-anchor="middle" class="dg-t">Données sans tag</text>
<line x1="102" y1="179" x2="224.0" y2="179.0" class="dg-line"/>
<polygon points="233,179 224.0,174.0 224.0,184.0" class="dg-ah"/>
<line x1="527" y1="179" x2="649.0" y2="179.0" class="dg-line"/>
<polygon points="658,179 649.0,174.0 649.0,184.0" class="dg-ah"/>
<text x="110" y="172" text-anchor="start" class="dg-s">Port VLAN 1 (natif)</text>
<text x="650" y="172" text-anchor="end" class="dg-s">Port VLAN 1 (natif)</text>
</svg>
<figcaption>Seul le VLAN natif circule sans tag sur le trunk.</figcaption>
</figure>

---

### F. Fonctionnement Complet : Trame VLAN de A à Z

**Topologie :**
<figure class="graphe diagramme"><svg viewBox="0 0 760 230" style="max-width:760px" role="img" aria-label="Deux VLAN sur deux switches reliés par un trunk" xmlns="http://www.w3.org/2000/svg">
<rect x="200" y="80" width="110" height="50" rx="8" class="dg-dark"/>
<text x="255.0" y="110.0" text-anchor="middle" class="dg-w">SW_A</text>
<rect x="450" y="80" width="110" height="50" rx="8" class="dg-dark"/>
<text x="505.0" y="110.0" text-anchor="middle" class="dg-w">SW_B</text>
<line x1="310" y1="105" x2="450" y2="105" class="dg-line"/>
<text x="380" y="95" text-anchor="middle" class="dg-t">Trunk</text>
<text x="380" y="124" text-anchor="middle" class="dg-i">VLAN 10 et 20</text>
<rect x="30" y="26" width="40" height="28" rx="4" class="dg-warn"/>
<rect x="35" y="31" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="47" y="54" width="6" height="6" class="dg-box"/>
<rect x="38" y="60" width="24" height="4" rx="2" class="dg-box"/>
<text x="50" y="80" text-anchor="middle" class="dg-s">PC1</text>
<rect x="30" y="146" width="40" height="28" rx="4" class="dg-ok"/>
<rect x="35" y="151" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="47" y="174" width="6" height="6" class="dg-box"/>
<rect x="38" y="180" width="24" height="4" rx="2" class="dg-box"/>
<text x="50" y="200" text-anchor="middle" class="dg-s">PC2</text>
<line x1="72" y1="44" x2="200" y2="92" class="dg-line"/>
<line x1="72" y1="164" x2="200" y2="118" class="dg-line"/>
<text x="136.0" y="58" text-anchor="middle" class="dg-i">Access 10</text>
<text x="136.0" y="158" text-anchor="middle" class="dg-i">Access 20</text>
<rect x="690" y="26" width="40" height="28" rx="4" class="dg-warn"/>
<rect x="695" y="31" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="707" y="54" width="6" height="6" class="dg-box"/>
<rect x="698" y="60" width="24" height="4" rx="2" class="dg-box"/>
<text x="710" y="80" text-anchor="middle" class="dg-s">PC3</text>
<rect x="690" y="146" width="40" height="28" rx="4" class="dg-ok"/>
<rect x="695" y="151" width="30" height="18" rx="2" class="dg-soft"/>
<rect x="707" y="174" width="6" height="6" class="dg-box"/>
<rect x="698" y="180" width="24" height="4" rx="2" class="dg-box"/>
<text x="710" y="200" text-anchor="middle" class="dg-s">PC4</text>
<line x1="688" y1="44" x2="560" y2="92" class="dg-line"/>
<line x1="688" y1="164" x2="560" y2="118" class="dg-line"/>
<text x="624.0" y="58" text-anchor="middle" class="dg-i">Access 10</text>
<text x="624.0" y="158" text-anchor="middle" class="dg-i">Access 20</text>
</svg>
<figcaption>PC1 et PC3 sont dans le VLAN 10, PC2 et PC4 dans le VLAN 20.</figcaption>
</figure>

**PC1 envoie une trame à PC3 :**

1. **PC1** envoie une trame normale (sans tag) sur son port Access ; **SW_A** la reçoit sur un port Access du VLAN 10.
2. **SW_A** consulte sa table MAC pour le VLAN 10 : si PC3 est connu, la sortie est le port Trunk, et SW_A **ajoute le tag VLAN 10**.
3. La trame taguée traverse le **trunk**.
4. **SW_B** lit le tag (VLAN 10), le **retire**, puis envoie la trame sans tag sur le port Access VLAN 10 de PC3.
5. **PC3** reçoit une trame normale : il ne sait pas qu'elle a été taguée.

**PC2 (VLAN 20) essaie d'envoyer à PC3 (VLAN 10) :**
- Impossible au niveau L2 — ils sont dans des VLANs différents
- SW_A ne transmettra jamais une trame VLAN 20 sur un port VLAN 10

---

## II. Configuration VLANs sur Switch Cisco

### A. Créer des VLANs

**Mode :** Global Configuration

```cisco
Switch# configure terminal

! Créer VLAN 10 et le nommer
Switch(config)# vlan 10
Switch(config-vlan)# name DIRECTION
Switch(config-vlan)# exit

! Créer VLAN 20
Switch(config)# vlan 20
Switch(config-vlan)# name COMMERCIAL
Switch(config-vlan)# exit

! Créer VLAN 30
Switch(config)# vlan 30
Switch(config-vlan)# name INFORMATIQUE
Switch(config-vlan)# exit
```

**Vérification :**
```cisco
Switch# show vlan brief
```

**Résultat :**
```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, ...  (tous les ports non affectés)
10   DIRECTION                        active
20   COMMERCIAL                       active
30   INFORMATIQUE                     active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

 **À noter :** Les VLANs 10, 20, 30 n'ont pas encore de ports affectés (colonne Ports vide).

---

### B. Affecter des Ports en Mode Access

**Mode :** Interface Configuration

```cisco
! Port 1 → VLAN 10 (Direction)
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

! Port 2 → VLAN 10 (Direction)
Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

! Ports 3-4 → VLAN 20 (Commercial) - commande de plage
Switch(config)# interface range FastEthernet0/3 - 4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

! Ports 5-6 → VLAN 30 (Informatique)
Switch(config)# interface range FastEthernet0/5 - 6
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit
```

**Vérification :**
```cisco
Switch# show vlan brief
```

**Résultat attendu :**
```
VLAN Name                   Status    Ports
---- ---------------         --------- --------
1    default                 active    Fa0/7, Fa0/8 ... (ports non affectés)
10   DIRECTION               active    Fa0/1, Fa0/2
20   COMMERCIAL              active    Fa0/3, Fa0/4
30   INFORMATIQUE            active    Fa0/5, Fa0/6
```

---

### C. Configurer un Port Trunk

**Mode :** Interface Configuration (sur le port reliant 2 switches)

```cisco
! Port 24 = liaison inter-switches → Trunk
Switch(config)# interface FastEthernet0/24
Switch(config-if)# switchport mode trunk

! (Optionnel - Bonne pratique) Restreindre les VLANs autorisés
Switch(config-if)# switchport trunk allowed vlan 10,20,30

! (Optionnel - Bonne pratique) Changer le VLAN natif
Switch(config-if)# switchport trunk native vlan 99

Switch(config-if)# exit
Switch(config)# end
Switch# copy run start
```

**Vérification :**
```cisco
Switch# show interfaces trunk
```

**Résultat :**
```
Port      Mode         Encapsulation  Status    Native vlan
Fa0/24    on           802.1q         trunking  99

Port      Vlans allowed on trunk
Fa0/24    10,20,30

Port      Vlans allowed and active in management domain
Fa0/24    10,20,30

Port      Vlans in spanning tree forwarding state and not pruned
Fa0/24    10,20,30
```

**Commande utile pour inspecter un port :**
```cisco
Switch# show interfaces FastEthernet0/1 switchport
```
```
Name: Fa0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: dot1q
Access Mode VLAN: 10 (DIRECTION)
Trunking Native Mode VLAN: 1 (default)
...
```

---

## III. Linux : Utilisateurs, Groupes et Permissions

### A. Notion d'Utilisateurs sous Linux

Linux est un système **multi-utilisateurs**. Chaque fichier et processus appartient à un utilisateur.

**Types de comptes :**

| **Type** | **UID** | **Exemples** | **Usage** |
|----------|---------|--------------|-----------|
| **root** | 0 | `root` | Super-administrateur, accès total |
| **Utilisateur système** | 1-999 | `www-data`, `syslog` | Services/démons système, pas de connexion interactive |
| **Utilisateur normal** | ≥1000 | `alice`, `etudiant` | Utilisateurs humains |

**Fichiers clés :**
```bash
cat /etc/passwd     # Liste des comptes : login:x:UID:GID:info:home:shell
cat /etc/group      # Liste des groupes : nom:x:GID:membres
sudo cat /etc/shadow  # Mots de passe hashés (root uniquement)
```

**Exemple de ligne `/etc/passwd` :**
<figure class="graphe diagramme"><svg viewBox="0 0 820 150" style="max-width:820px" role="img" aria-label="Anatomie d&#x27;une ligne de /etc/passwd" xmlns="http://www.w3.org/2000/svg">
<rect x="20" y="14" width="90" height="34" rx="5" class="dg-warn"/>
<text x="65.0" y="36.0" text-anchor="middle" class="dg-t">alice</text>
<line x1="65.0" y1="48" x2="65.0" y2="70" class="dg-dash"/>
<text x="65.0" y="82" text-anchor="middle" class="dg-s">Nom de login</text>
<text x="117" y="36" text-anchor="middle" class="dg-t">:</text>
<rect x="124" y="14" width="50" height="34" rx="5" class="dg-box"/>
<text x="149.0" y="36.0" text-anchor="middle" class="dg-t">x</text>
<line x1="149.0" y1="48" x2="149.0" y2="96" class="dg-dash"/>
<text x="149.0" y="108" text-anchor="middle" class="dg-s">Mot de passe</text>
<text x="181" y="36" text-anchor="middle" class="dg-t">:</text>
<rect x="188" y="14" width="70" height="34" rx="5" class="dg-ok"/>
<text x="223.0" y="36.0" text-anchor="middle" class="dg-t">1001</text>
<line x1="223.0" y1="48" x2="223.0" y2="70" class="dg-dash"/>
<text x="223.0" y="82" text-anchor="middle" class="dg-s">UID</text>
<text x="265" y="36" text-anchor="middle" class="dg-t">:</text>
<rect x="272" y="14" width="70" height="34" rx="5" class="dg-box"/>
<text x="307.0" y="36.0" text-anchor="middle" class="dg-t">1001</text>
<line x1="307.0" y1="48" x2="307.0" y2="96" class="dg-dash"/>
<text x="307.0" y="108" text-anchor="middle" class="dg-s">GID principal</text>
<text x="349" y="36" text-anchor="middle" class="dg-t">:</text>
<rect x="356" y="14" width="140" height="34" rx="5" class="dg-soft"/>
<text x="426.0" y="36.0" text-anchor="middle" class="dg-t">Alice Dupont</text>
<line x1="426.0" y1="48" x2="426.0" y2="70" class="dg-dash"/>
<text x="426.0" y="82" text-anchor="middle" class="dg-s">Info (GECOS)</text>
<text x="503" y="36" text-anchor="middle" class="dg-t">:</text>
<rect x="510" y="14" width="130" height="34" rx="5" class="dg-ok"/>
<text x="575.0" y="36.0" text-anchor="middle" class="dg-t">/home/alice</text>
<line x1="575.0" y1="48" x2="575.0" y2="96" class="dg-dash"/>
<text x="575.0" y="108" text-anchor="middle" class="dg-s">Répertoire home</text>
<text x="647" y="36" text-anchor="middle" class="dg-t">:</text>
<rect x="654" y="14" width="120" height="34" rx="5" class="dg-warn"/>
<text x="714.0" y="36.0" text-anchor="middle" class="dg-t">/bin/bash</text>
<line x1="714.0" y1="48" x2="714.0" y2="70" class="dg-dash"/>
<text x="714.0" y="82" text-anchor="middle" class="dg-s">Shell par défaut</text>
<text x="410" y="142" text-anchor="middle" class="dg-i">x : le hachage du mot de passe est stocké dans /etc/shadow</text>
</svg>
<figcaption>Sept champs séparés par deux-points.</figcaption>
</figure>

---

### B. Créer et Gérer des Utilisateurs

```bash
# Créer un utilisateur avec home directory et shell bash
sudo useradd -m -s /bin/bash alice
#           -m : créer /home/alice automatiquement
#           -s : définir le shell (/bin/bash = shell interactif)

# Définir le mot de passe
sudo passwd alice
# → Demande le nouveau MDP (2 fois, ne s'affiche pas)

# Créer un utilisateur et l'affecter directement à un groupe
sudo useradd -m -s /bin/bash -G direction alice
#           -G : groupe(s) supplémentaires (en plus du groupe principal)

# Modifier un utilisateur existant
sudo usermod -aG direction alice    # Ajouter alice au groupe direction (-a = append, sinon remplace)
sudo usermod -aG sudo alice         # Donner les droits sudo à alice
sudo usermod -s /bin/sh alice       # Changer le shell
sudo usermod -l nouveau_nom alice   # Renommer le compte

# Supprimer un utilisateur
sudo userdel alice                  # Supprime le compte (garde /home/alice)
sudo userdel -r alice               # Supprime compte + /home/alice + mail spool
```

**`usermod -aG` vs `usermod -G` :**
- `-G groupe` seul → **remplace** tous les groupes supplémentaires par `groupe`
- `-aG groupe` → **ajoute** `groupe` aux groupes existants (append)
- Toujours utiliser `-aG` pour ne pas accidentellement retirer des accès !

---

### C. Créer et Gérer des Groupes

```bash
# Créer un groupe
sudo groupadd direction
sudo groupadd commercial
sudo groupadd informatique

# Supprimer un groupe
sudo groupdel direction

# Afficher les groupes d'un utilisateur
groups alice          # Sortie : alice direction sudo
id alice              # Sortie : uid=1001(alice) gid=1001(alice) groups=1001(alice),4(adm),27(sudo),1002(direction)

# Lister tous les groupes
cat /etc/group
```

---

### D. Comprendre les Permissions Linux

Chaque fichier/répertoire a **3 niveaux de permissions** pour **3 catégories** :

<figure class="graphe diagramme"><svg viewBox="0 0 780 250" style="max-width:780px" role="img" aria-label="Lecture de la commande ls -l" xmlns="http://www.w3.org/2000/svg">
<text x="20" y="26" text-anchor="start" class="dg-i">$ ls -l fichier.txt</text>
<rect x="20" y="36" width="150" height="34" rx="5" class="dg-warn"/>
<text x="95.0" y="58.0" text-anchor="middle" class="dg-t">-rw-r--r--</text>
<line x1="95.0" y1="70" x2="95.0" y2="100" class="dg-dash"/>
<text x="101.0" y="104" text-anchor="start" class="dg-s">Type + permissions</text>
<rect x="180" y="36" width="34" height="34" rx="5" class="dg-box"/>
<text x="197.0" y="58.0" text-anchor="middle" class="dg-t">1</text>
<line x1="197.0" y1="70" x2="197.0" y2="132" class="dg-dash"/>
<text x="203.0" y="136" text-anchor="start" class="dg-s">Liens</text>
<rect x="224" y="36" width="70" height="34" rx="5" class="dg-ok"/>
<text x="259.0" y="58.0" text-anchor="middle" class="dg-t">alice</text>
<line x1="259.0" y1="70" x2="259.0" y2="164" class="dg-dash"/>
<text x="265.0" y="168" text-anchor="start" class="dg-s">Utilisateur</text>
<rect x="304" y="36" width="100" height="34" rx="5" class="dg-ok"/>
<text x="354.0" y="58.0" text-anchor="middle" class="dg-t">direction</text>
<line x1="354.0" y1="70" x2="354.0" y2="196" class="dg-dash"/>
<text x="360.0" y="200" text-anchor="start" class="dg-s">Groupe</text>
<rect x="414" y="36" width="54" height="34" rx="5" class="dg-box"/>
<text x="441.0" y="58.0" text-anchor="middle" class="dg-t">1024</text>
<line x1="441.0" y1="70" x2="441.0" y2="100" class="dg-dash"/>
<text x="447.0" y="104" text-anchor="start" class="dg-s">Taille</text>
<rect x="478" y="36" width="110" height="34" rx="5" class="dg-box"/>
<text x="533.0" y="58.0" text-anchor="middle" class="dg-t">nov 15 10:30</text>
<line x1="533.0" y1="70" x2="533.0" y2="132" class="dg-dash"/>
<text x="539.0" y="136" text-anchor="start" class="dg-s">Date</text>
<rect x="598" y="36" width="110" height="34" rx="5" class="dg-soft"/>
<text x="653.0" y="58.0" text-anchor="middle" class="dg-t">fichier.txt</text>
<line x1="653.0" y1="70" x2="653.0" y2="164" class="dg-dash"/>
<text x="659.0" y="168" text-anchor="end" class="dg-s">Nom</text>
<text x="20" y="238" text-anchor="start" class="dg-i">Type : - fichier, d répertoire, l lien symbolique. Puis 9 caractères de permissions.</text>
</svg>
<figcaption>Chaque colonne de la sortie de ls -l a une signification précise.</figcaption>
</figure>

**Décomposition des 9 caractères de permissions :**

<figure class="graphe diagramme"><svg viewBox="0 0 560 170" style="max-width:560px" role="img" aria-label="Les 9 caractères de permissions : propriétaire, groupe, autres" xmlns="http://www.w3.org/2000/svg">
<rect x="30" y="14" width="150" height="40" rx="6" class="dg-warn"/>
<text x="105.0" y="39.0" text-anchor="middle" class="dg-t">rw-</text>
<line x1="105" y1="54" x2="105" y2="84" class="dg-dash"/>
<text x="105" y="102" text-anchor="middle" class="dg-t">Propriétaire (user)</text>
<text x="105" y="122" text-anchor="middle" class="dg-s">lecture + écriture</text>
<rect x="200" y="14" width="150" height="40" rx="6" class="dg-ok"/>
<text x="275.0" y="39.0" text-anchor="middle" class="dg-t">r--</text>
<line x1="275" y1="54" x2="275" y2="84" class="dg-dash"/>
<text x="275" y="102" text-anchor="middle" class="dg-t">Groupe (group)</text>
<text x="275" y="122" text-anchor="middle" class="dg-s">lecture seule</text>
<rect x="370" y="14" width="150" height="40" rx="6" class="dg-box"/>
<text x="445.0" y="39.0" text-anchor="middle" class="dg-t">r--</text>
<line x1="445" y1="54" x2="445" y2="84" class="dg-dash"/>
<text x="445" y="102" text-anchor="middle" class="dg-t">Autres (others)</text>
<text x="445" y="122" text-anchor="middle" class="dg-s">lecture seule</text>
<text x="280" y="156" text-anchor="middle" class="dg-i">r = lire (4)   w = écrire (2)   x = exécuter (1)</text>
</svg>
<figcaption>Trois triplets rwx pour trois catégories.</figcaption>
</figure>

**Signification de chaque permission :**

| **Permission** | **Symbole** | **Valeur** | **Sur un fichier** | **Sur un répertoire** |
|----------------|-------------|------------|--------------------|-----------------------|
| **Read** | `r` | 4 | Lire le contenu | Lister le contenu (ls) |
| **Write** | `w` | 2 | Modifier/écrire | Créer/supprimer des fichiers dedans |
| **Execute** | `x` | 1 | Exécuter (script, programme) | Traverser (cd dans ce répertoire) |
| **Aucune** | `-` | 0 | Accès refusé | Accès refusé |

---

### E. Modifier les Permissions : `chmod`

**Deux syntaxes :** numérique (rapide) et symbolique (lisible).

#### **Syntaxe Numérique**

Calculer la valeur pour chaque catégorie (u/g/o) : r=4, w=2, x=1.

**Exemples courants :**

| **Notation** | **Permissions** | **Usage typique** |
|--------------|-----------------|-------------------|
| `chmod 777` | `rwxrwxrwx` | Dangereux - tout le monde peut tout faire |
| `chmod 755` | `rwxr-xr-x` | Script/exécutable : proprio tout, groupe+autres exécutent |
| `chmod 644` | `rw-r--r--` | Fichier normal : proprio modifie, groupe+autres lisent |
| `chmod 640` | `rw-r-----` | Fichier sensible : groupe lit, autres rien |
| `chmod 600` | `rw-------` | Fichier privé : proprio seul modifie |
| `chmod 700` | `rwx------` | Répertoire privé : proprio seul accède |

**Calcul rapide `chmod 754` :**
```
7 = r+w+x = 4+2+1 = rwx  (propriétaire)
5 = r+x   = 4+0+1 = r-x  (groupe)
4 = r     = 4+0+0 = r--  (autres)
→ rwxr-xr--
```

---

#### **Syntaxe Symbolique**

Format : `chmod [qui][opération][permission] fichier`

| **Qui ?** | **Opération** | **Permission** |
|-----------|---------------|----------------|
| `u` = user (propriétaire) | `+` = ajouter | `r` = lecture |
| `g` = group | `-` = retirer | `w` = écriture |
| `o` = others | `=` = définir exactement | `x` = exécution |
| `a` = all (tous) | | |

```bash
chmod u+x script.sh          # Ajouter l'exécution au propriétaire
chmod g-w fichier.txt         # Retirer l'écriture au groupe
chmod o-rwx confidentiel.txt  # Retirer tout aux autres
chmod a+r rapport.pdf         # Ajouter lecture à tous
chmod u=rwx,g=rx,o= dossier/  # Définir précisément : proprio=rwx, groupe=r-x, autres=---
chmod -R 755 /var/www/html/   # Appliquer récursivement (-R)
```

---

### F. Modifier le Propriétaire : `chown`

```bash
# Changer le propriétaire
chown alice fichier.txt               # Fichier appartient maintenant à alice
sudo chown alice fichier.txt          # Souvent besoin de sudo

# Changer propriétaire ET groupe simultanément
chown alice:direction fichier.txt     # alice = proprio, direction = groupe
chown :direction fichier.txt          # Changer seulement le groupe (équivalent chgrp)

# Récursif (tout un dossier)
sudo chown -R alice:direction /var/data/direction/
```

---

### G. Scénario Complet : Créer des Utilisateurs par Service

**Objectif :** Préparer un système Linux pour 3 services (Direction, Commercial, Informatique) avec isolation des données.

```bash
# 1. Créer les groupes
sudo groupadd direction
sudo groupadd commercial
sudo groupadd informatique

# 2. Créer les utilisateurs
sudo useradd -m -s /bin/bash -G direction alice
sudo passwd alice         # Définir MDP pour alice

sudo useradd -m -s /bin/bash -G direction bob
sudo passwd bob

sudo useradd -m -s /bin/bash -G commercial charlie
sudo passwd charlie

sudo useradd -m -s /bin/bash -G informatique diana
sudo passwd diana

# 3. Créer les répertoires partagés par service
sudo mkdir -p /data/direction
sudo mkdir -p /data/commercial
sudo mkdir -p /data/informatique

# 4. Affecter propriétaire et groupe
sudo chown root:direction /data/direction
sudo chown root:commercial /data/commercial
sudo chown root:informatique /data/informatique

# 5. Permissions : groupe peut lire/écrire/traverser, autres rien
sudo chmod 770 /data/direction
sudo chmod 770 /data/commercial
sudo chmod 770 /data/informatique
# → rwxrwx--- : proprio(root)=tout, groupe=tout, autres=rien

# 6. Vérification
ls -ld /data/direction
# drwxrwx--- 2 root direction 4096 nov 15 10:30 /data/direction

# Test : alice (direction) peut-elle accéder à /data/commercial ?
su - alice
ls /data/commercial    # Doit afficher "Permission non accordée"
ls /data/direction     # Doit fonctionner
```

---

## IV. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **VLAN** | Virtual Local Area Network - réseau logique sur un switch physique |
| **Segmentation logique** | Découpage d'un réseau en sous-réseaux isolés sans modifier le câblage |
| **Port Access** | Port switch appartenant à un seul VLAN (pour équipements terminaux) |
| **Port Trunk** | Port switch transportant plusieurs VLANs (liaisons inter-switches) |
| **Tag 802.1Q** | En-tête de 4 octets inséré dans la trame pour identifier le VLAN |
| **VID** | VLAN Identifier - 12 bits dans le tag 802.1Q (4094 VLANs max) |
| **VLAN natif** | VLAN dont les trames traversent le trunk sans tag (défaut = VLAN 1) |
| **Domaine de diffusion VLAN** | Un VLAN = un domaine de diffusion distinct |
| **Routage inter-VLAN** | Communication entre VLANs via un routeur ou switch L3 |
| **`vlan <id>`** | Mode de configuration d'un VLAN dans la CLI Cisco |
| **`switchport mode access`** | Configurer un port en mode Access |
| **`switchport mode trunk`** | Configurer un port en mode Trunk |
| **`show vlan brief`** | Afficher résumé des VLANs et leurs ports |
| **`show interfaces trunk`** | Afficher les ports trunk et les VLANs autorisés |
| **UID** | User ID - identifiant numérique d'un utilisateur Linux |
| **GID** | Group ID - identifiant numérique d'un groupe Linux |
| **`useradd`** | Commande de création d'utilisateur Linux |
| **`passwd`** | Définir ou changer un mot de passe utilisateur |
| **`usermod -aG`** | Ajouter un utilisateur à un groupe (sans retirer les autres) |
| **`groupadd`** | Créer un groupe Linux |
| **Permissions rwx** | Read (4), Write (2), eXecute (1) sur fichier ou répertoire |
| **`chmod`** | Modifier les permissions d'un fichier/répertoire |
| **`chown`** | Changer le propriétaire (et éventuellement le groupe) |
| **`ls -l`** | Afficher les permissions, propriétaire, taille d'un fichier |

---

## V. Exercices d'Entraînement

### Exercice 1 : Calcul de Permissions

Convertir en notation symbolique (rwx) :

| **chmod** | **Propriétaire** | **Groupe** | **Autres** | **Notation symbolique** |
|-----------|-----------------|------------|------------|-------------------------|
| `chmod 755` | | | | |
| `chmod 644` | | | | |
| `chmod 700` | | | | |
| `chmod 640` | | | | |
| `chmod 777` | | | | |

<details markdown="1">
<summary>Correction : valeur numérique vers symbolique</summary>

Chaque chiffre est la somme de **r = 4**, **w = 2**, **x = 1**.

| **chmod** | **Propriétaire** | **Groupe** | **Autres** | **Notation symbolique** |
|-----------|:----------------:|:----------:|:----------:|-------------------------|
| `chmod 755` | 7 = rwx | 5 = r-x | 5 = r-x | `rwxr-xr-x` |
| `chmod 644` | 6 = rw- | 4 = r-- | 4 = r-- | `rw-r--r--` |
| `chmod 700` | 7 = rwx | 0 = --- | 0 = --- | `rwx------` |
| `chmod 640` | 6 = rw- | 4 = r-- | 0 = --- | `rw-r-----` |
| `chmod 777` | 7 = rwx | 7 = rwx | 7 = rwx | `rwxrwxrwx` |

</details>

Et l'inverse, convertir en valeur numérique :

| **Notation symbolique** | **Valeur numérique** |
|-------------------------|----------------------|
| `rwxr-xr-x` | |
| `rw-r--r--` | |
| `rwx------` | |
| `rw-rw-r--` | |

<details markdown="1">
<summary>Correction : symbolique vers valeur numérique</summary>

On additionne les droits de chaque groupe de trois caractères.

| **Notation symbolique** | **Calcul** | **Valeur numérique** |
|-------------------------|------------|----------------------|
| `rwxr-xr-x` | rwx = 4+2+1 = 7 ; r-x = 4+1 = 5 ; r-x = 5 | **755** |
| `rw-r--r--` | rw- = 4+2 = 6 ; r-- = 4 ; r-- = 4 | **644** |
| `rwx------` | rwx = 7 ; --- = 0 ; --- = 0 | **700** |
| `rw-rw-r--` | rw- = 6 ; rw- = 6 ; r-- = 4 | **664** |

</details>

---

### Exercice 2 : Analyse `ls -l`

```
-rw-r----- 1 alice direction  2048 nov 15 rapport.pdf
drwxrwx--- 2 root  commercial 4096 nov 15 /data/commercial
-rwxr-xr-x 1 root  root        512 nov 15 script.sh
```

**1.** Qui peut **lire** `rapport.pdf` ? Qui ne peut pas ?

<details markdown="1">
<summary>Correction de la question 1</summary>

Le propriétaire `alice` (`rw-`) et les membres du groupe `direction` (`r--`) peuvent le lire. Les **autres** (`---`) ne peuvent pas, y compris les membres du groupe `commercial`. L'administrateur `root` passe outre les permissions.

</details>

**2.** Qui peut **créer des fichiers** dans `/data/commercial` ?

<details markdown="1">
<summary>Correction de la question 2</summary>

Pour créer un fichier dans un dossier, il faut les droits **w** (écriture) **et x** (traversée) sur ce dossier. Le propriétaire `root` et le groupe `commercial` ont `rwx` : ils peuvent créer des fichiers. Les autres n'ont aucun droit (`---`).

</details>

**3.** L'utilisateur `charlie` (groupe `commercial`) peut-il exécuter `script.sh` ?

<details markdown="1">
<summary>Correction de la question 3</summary>

**Oui.** `charlie` n'est ni `root` ni dans le groupe `root` : ce sont les droits des **autres** qui s'appliquent, soit `r-x`. Il a donc le droit de lire et d'exécuter le script.

</details>

**4.** Donner la commande pour que `bob` (groupe `direction`) puisse lire `rapport.pdf`.

<details markdown="1">
<summary>Correction de la question 4</summary>

**Aucune commande n'est nécessaire** : le groupe `direction` a déjà le droit de lecture (`r--`), et `bob` en fait partie. Si `bob` n'était pas dans ce groupe : `sudo usermod -aG direction bob` (il doit se reconnecter), ou, pour ne donner l'accès qu'à lui, `setfacl -m u:bob:r rapport.pdf`.

</details>

---

### Exercice 3 : VLANs

Un switch a la configuration suivante :
```
VLAN 10 (RH) : Ports 1-3
VLAN 20 (IT) : Ports 4-6
VLAN 30 (DIR) : Ports 7-8
Trunk : Port 24
```

**Questions :**

**1.** PC branché en port 2 peut-il communiquer avec PC en port 5 ? Pourquoi ?

<details markdown="1">
<summary>Correction de la question 1</summary>

**Non.** Le port 2 est dans le VLAN 10 et le port 5 dans le VLAN 20 : deux VLAN sont deux domaines de diffusion séparés, le switch ne fait jamais passer une trame de l'un à l'autre. Il faudrait un routeur.

</details>

**2.** PC en port 7 envoie un broadcast. Qui le reçoit ?

<details markdown="1">
<summary>Correction de la question 2</summary>

Uniquement les équipements du **VLAN 30** : le PC du port 8, et, par le trunk du port 24 (la trame y est étiquetée VLAN 30), les membres du VLAN 30 du second switch. Les VLAN 10 et 20 ne le reçoivent pas.

</details>

**3.** Quelle commande affiche les ports affectés à chaque VLAN ?

<details markdown="1">
<summary>Correction de la question 3</summary>

`show vlan brief`.

</details>

**4.** Que faut-il pour que RH et IT puissent s'envoyer des fichiers ?

<details markdown="1">
<summary>Correction de la question 4</summary>

Un équipement de **couche 3** : un routeur (sous-interfaces 802.1Q, « router on a stick ») ou un switch L3. Chaque VLAN a son propre réseau IP avec une passerelle, et les PC sont configurés avec cette passerelle. Le partage de fichiers doit en plus être autorisé par les règles de filtrage éventuelles (ACL, pare-feu).

</details>

**5.** Si le port 24 est en trunk, quels VLANs traversent-ils la liaison vers le 2e switch ?

<details markdown="1">
<summary>Correction de la question 5</summary>

Par défaut, **tous les VLAN** existants sur le switch (1, 10, 20, 30). On restreint en pratique avec `switchport trunk allowed vlan 10,20,30` pour ne transporter que les VLAN utiles.

</details>

---

## VI. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer l'intérêt des VLANs (sécurité, performance, flexibilité)
- [ ] Distinguer port Access (1 VLAN) et port Trunk (N VLANs)
- [ ] Expliquer le rôle du tag 802.1Q et du VLAN natif
- [ ] Créer des VLANs sur un switch Cisco et les nommer
- [ ] Affecter des ports en mode Access à un VLAN
- [ ] Configurer un port Trunk entre deux switches
- [ ] Utiliser `show vlan brief` et `show interfaces trunk`
- [ ] Créer des utilisateurs et groupes Linux (useradd, groupadd, passwd)
- [ ] Ajouter un utilisateur à un groupe (usermod -aG)
- [ ] Lire les permissions Linux (rwxr-xr-x → 755)
- [ ] Calculer la valeur chmod numérique (r=4, w=2, x=1)
- [ ] Modifier les permissions avec chmod (numérique et symbolique)
- [ ] Changer le propriétaire d'un fichier avec chown

---
