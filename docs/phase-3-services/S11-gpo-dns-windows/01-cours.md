# Cours – Stratégies de groupe (GPO) et DNS intégré à AD

*BTS SIO SISR 1re année – Bloc 2 – S11 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** S10.
> - **Matériel :** VM Windows Server (domaine S10), VM cliente.

## À la fin de ce cours, tu sauras

- [ ] **Expliquer** le rôle des GPO et l'ordre d'application (LSDOU) ;
- [ ] **Créer** et **lier** une GPO avec la console de gestion ;
- [ ] **Configurer** une GPO de sécurité et une GPO d'environnement ;
- [ ] **Forcer** et **diagnostiquer** avec `gpupdate /force` et `gpresult /r` ;
- [ ] **Gérer** une zone DNS intégrée à AD (enregistrements A, CNAME, zone inverse).

---

## FICHE DE COURS ÉLÈVE
### "Stratégies de Groupe (GPO) et DNS Windows AD-Intégré"

*Version 1.0 – BTS SIO SISR – *

---

### I. Les Stratégies de Groupe (GPO)

#### A. Définition et Rôle

Une **GPO** (Group Policy Object – Objet de Stratégie de Groupe) est un ensemble de **paramètres de configuration** appliqués automatiquement à des ordinateurs ou des utilisateurs membres d'un domaine Active Directory.

**Ce qu'une GPO peut faire :**

| **Catégorie** | **Exemples de paramètres** |
|---------------|---------------------------|
| **Sécurité** | Longueur minimale du mot de passe, verrouillage de compte, audit des connexions |
| **Interface** | Fond d'écran imposé, désactiver l'écran de veille, masquer des icônes bureau |
| **Restrictions** | Interdire l'installation de logiciels, bloquer l'accès au Panneau de config. |
| **Réseau** | Mapper automatiquement des lecteurs réseau, configurer les imprimantes |
| **Scripts** | Exécuter un script à l'ouverture/fermeture de session ou au démarrage/arrêt |
| **Mises à jour** | Configurer Windows Update (WSUS) |
| **Logiciels** | Déployer automatiquement des logiciels MSI |
| **Internet Explorer / Edge** | Page d'accueil imposée, liste de sites autorisés/bloqués |

---

#### B. Architecture GPO : LSDOU

Les GPO s'appliquent dans un ordre précis, du **plus général au plus spécifique** :

<figure class="graphe diagramme"><svg viewBox="0 0 760 300" style="max-width:760px" role="img" aria-label="Ordre d&#x27;application des GPO : Local, Site, Domaine, OU" xmlns="http://www.w3.org/2000/svg">
<rect x="20" y="14" width="700" height="54" rx="8" class="dg-box"/>
<circle cx="48" cy="41" r="16" class="dg-dark"/>
<text x="48" y="47" text-anchor="middle" class="dg-w">L</text>
<text x="76" y="38" text-anchor="start" class="dg-t">Local</text>
<text x="76" y="56" text-anchor="start" class="dg-s">stratégie locale (gpedit.msc), sans domaine</text>
<line x1="40" y1="68" x2="40.0" y2="71.0" class="dg-line"/>
<polygon points="40,80 45.0,71.0 35.0,71.0" class="dg-ah"/>
<rect x="60" y="80" width="660" height="54" rx="8" class="dg-box"/>
<circle cx="88" cy="107" r="16" class="dg-dark"/>
<text x="88" y="113" text-anchor="middle" class="dg-w">S</text>
<text x="116" y="104" text-anchor="start" class="dg-t">Site</text>
<text x="116" y="122" text-anchor="start" class="dg-s">stratégies liées au site AD géographique</text>
<line x1="80" y1="134" x2="80.0" y2="137.0" class="dg-line"/>
<polygon points="80,146 85.0,137.0 75.0,137.0" class="dg-ah"/>
<rect x="100" y="146" width="620" height="54" rx="8" class="dg-soft"/>
<circle cx="128" cy="173" r="16" class="dg-dark"/>
<text x="128" y="179" text-anchor="middle" class="dg-w">D</text>
<text x="156" y="170" text-anchor="start" class="dg-t">Domaine</text>
<text x="156" y="188" text-anchor="start" class="dg-s">stratégies liées au domaine, pour tous</text>
<line x1="120" y1="200" x2="120.0" y2="203.0" class="dg-line"/>
<polygon points="120,212 125.0,203.0 115.0,203.0" class="dg-ah"/>
<rect x="140" y="212" width="580" height="54" rx="8" class="dg-warn"/>
<circle cx="168" cy="239" r="16" class="dg-dark"/>
<text x="168" y="245" text-anchor="middle" class="dg-w">O</text>
<text x="196" y="236" text-anchor="start" class="dg-t">OU</text>
<text x="196" y="254" text-anchor="start" class="dg-s">stratégies liées à l&#x27;OU de l&#x27;objet (OU enfant appliquée en dernier)</text>
</svg>
<figcaption>LSDOU : chaque niveau est appliqué après le précédent ; le plus spécifique l&#x27;emporte.</figcaption>
</figure>

**Règle de priorité :**
- En cas de **conflit** entre deux GPO, la **dernière appliquée** gagne
- OU enfant > OU parent > Domaine > Site > Local
- **Paramètre "Non configuré"** dans une GPO = la GPO ne touche pas ce paramètre, l'héritage s'applique
- **Paramètre "Désactivé"** = la GPO force ce paramètre à être désactivé

---

#### C. Héritage des GPO

Par défaut, les GPO liées à une OU parent s'appliquent aussi aux OU enfants (**héritage**).

<figure class="graphe diagramme"><svg viewBox="0 0 521 246" style="max-width:521px" role="img" aria-label="Héritage des GPO dans le domaine techpro.local" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="225" class="dg-line"/>
<line x1="64" y1="225" x2="88" y2="225" class="dg-line"/>
<line x1="102" y1="102" x2="102" y2="123" class="dg-line"/>
<line x1="102" y1="123" x2="126" y2="123" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="157" class="dg-line"/>
<line x1="140" y1="157" x2="164" y2="157" class="dg-line"/>
<line x1="178" y1="170" x2="178" y2="191" class="dg-line"/>
<line x1="178" y1="191" x2="202" y2="191" class="dg-line"/>
<rect x="12" y="8" width="194" height="26" rx="6" class="dg-dark"/>
<text x="109.0" y="26" text-anchor="middle" class="dg-w">Domaine techpro.local</text>
<rect x="50" y="42" width="114" height="26" rx="6" class="dg-warn"/>
<text x="107.0" y="60" text-anchor="middle" class="dg-t">GPO_Domaine</text>
<text x="174.0" y="60" text-anchor="start" class="dg-i">fond d&#x27;écran entreprise</text>
<rect x="88" y="76" width="146" height="26" rx="6" class="dg-box"/>
<text x="161.0" y="94" text-anchor="middle" class="dg-t">OU_UTILISATEURS</text>
<rect x="126" y="110" width="154" height="26" rx="6" class="dg-warn"/>
<text x="203.0" y="128" text-anchor="middle" class="dg-t">GPO_Utilisateurs</text>
<text x="290.0" y="128" text-anchor="start" class="dg-i">mot de passe 10 caractères</text>
<rect x="164" y="144" width="130" height="26" rx="6" class="dg-box"/>
<text x="229.0" y="162" text-anchor="middle" class="dg-t">OU Commercial</text>
<rect x="202" y="178" width="138" height="26" rx="6" class="dg-warn"/>
<text x="271.0" y="196" text-anchor="middle" class="dg-t">GPO_Commercial</text>
<text x="350.0" y="196" text-anchor="start" class="dg-i">mot de passe 8 caractères</text>
<rect x="88" y="212" width="130" height="26" rx="6" class="dg-box"/>
<text x="153.0" y="230" text-anchor="middle" class="dg-t">OU Directeurs</text>
<text x="228.0" y="230" text-anchor="start" class="dg-i">hérite de GPO_Domaine et GPO_Utilisateurs</text>
</svg>
<figcaption>Une GPO liée à un conteneur s&#x27;applique à tous ses enfants, sauf si une GPO plus proche la remplace.</figcaption>
</figure>

**Mécanismes pour modifier l'héritage :**

| **Mécanisme** | **Effet** | **Qui peut l'appliquer ?** |
|---------------|-----------|--------------------------|
| **Enforced** (Forcer) | La GPO s'applique quoi qu'il arrive, même si une OU enfant dit le contraire | Administrateur GPO |
| **Block Inheritance** (Bloquer l'héritage) | L'OU n'hérite plus des GPO parentes | Admin de l'OU |
| **Enforced > Block Inheritance** | Si une GPO est "Enforced", le "Block Inheritance" ne peut pas la bloquer | – |

---

#### D. Filtrage des GPO

Par défaut, une GPO liée à une OU s'applique à **tous les objets** de cette OU. On peut **filtrer** son application :

##### Filtrage par groupe de sécurité

Dans les propriétés de la GPO → onglet **Délégation** / **Sécurité** :
- La GPO s'applique par défaut aux objets ayant les droits `Lire` + `Appliquer la stratégie de groupe`
- Retirer `Appliquer` sur un groupe = les membres de ce groupe ne reçoivent pas la GPO
- Ajouter un groupe avec `Appliquer` = filtrer la GPO uniquement sur ce groupe

**Exemple :** GPO `GPO_Bureau_Restreint` liée à `OU_UTILISATEURS`, mais on veut qu'elle ne s'applique qu'aux commerciaux :
1. Retirer `Authenticated Users` de la liste
2. Ajouter `GRP_Commercial` avec `Lire` + `Appliquer`

---

#### E. Structure d'une GPO

Chaque GPO est divisée en deux parties :

<figure class="graphe diagramme"><svg viewBox="0 0 612 756" style="max-width:612px" role="img" aria-label="Structure d&#x27;un objet de stratégie de groupe" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="463" class="dg-line"/>
<line x1="26" y1="463" x2="50" y2="463" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="123" class="dg-line"/>
<line x1="64" y1="123" x2="88" y2="123" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="327" class="dg-line"/>
<line x1="64" y1="327" x2="88" y2="327" class="dg-line"/>
<line x1="102" y1="136" x2="102" y2="157" class="dg-line"/>
<line x1="102" y1="157" x2="126" y2="157" class="dg-line"/>
<line x1="102" y1="136" x2="102" y2="191" class="dg-line"/>
<line x1="102" y1="191" x2="126" y2="191" class="dg-line"/>
<line x1="140" y1="204" x2="140" y2="225" class="dg-line"/>
<line x1="140" y1="225" x2="164" y2="225" class="dg-line"/>
<line x1="140" y1="204" x2="140" y2="259" class="dg-line"/>
<line x1="140" y1="259" x2="164" y2="259" class="dg-line"/>
<line x1="140" y1="204" x2="140" y2="293" class="dg-line"/>
<line x1="140" y1="293" x2="164" y2="293" class="dg-line"/>
<line x1="102" y1="340" x2="102" y2="361" class="dg-line"/>
<line x1="102" y1="361" x2="126" y2="361" class="dg-line"/>
<line x1="102" y1="340" x2="102" y2="395" class="dg-line"/>
<line x1="102" y1="395" x2="126" y2="395" class="dg-line"/>
<line x1="102" y1="340" x2="102" y2="429" class="dg-line"/>
<line x1="102" y1="429" x2="126" y2="429" class="dg-line"/>
<line x1="64" y1="476" x2="64" y2="497" class="dg-line"/>
<line x1="64" y1="497" x2="88" y2="497" class="dg-line"/>
<line x1="64" y1="476" x2="64" y2="531" class="dg-line"/>
<line x1="64" y1="531" x2="88" y2="531" class="dg-line"/>
<line x1="64" y1="476" x2="64" y2="633" class="dg-line"/>
<line x1="64" y1="633" x2="88" y2="633" class="dg-line"/>
<line x1="102" y1="544" x2="102" y2="565" class="dg-line"/>
<line x1="102" y1="565" x2="126" y2="565" class="dg-line"/>
<line x1="102" y1="544" x2="102" y2="599" class="dg-line"/>
<line x1="102" y1="599" x2="126" y2="599" class="dg-line"/>
<line x1="102" y1="646" x2="102" y2="667" class="dg-line"/>
<line x1="102" y1="667" x2="126" y2="667" class="dg-line"/>
<line x1="102" y1="646" x2="102" y2="701" class="dg-line"/>
<line x1="102" y1="701" x2="126" y2="701" class="dg-line"/>
<line x1="102" y1="646" x2="102" y2="735" class="dg-line"/>
<line x1="102" y1="735" x2="126" y2="735" class="dg-line"/>
<rect x="12" y="8" width="50" height="26" rx="6" class="dg-dark"/>
<text x="37.0" y="26" text-anchor="middle" class="dg-w">GPO</text>
<rect x="50" y="42" width="218" height="26" rx="6" class="dg-warn"/>
<text x="159.0" y="60" text-anchor="middle" class="dg-t">Configuration Ordinateur</text>
<text x="278.0" y="60" text-anchor="start" class="dg-i">appliquée à l&#x27;ordinateur au démarrage</text>
<rect x="88" y="76" width="158" height="26" rx="6" class="dg-soft"/>
<text x="167.0" y="94" text-anchor="middle" class="dg-s">Paramètres logiciels</text>
<text x="256.0" y="94" text-anchor="start" class="dg-i">déploiement MSI</text>
<rect x="88" y="110" width="170" height="26" rx="6" class="dg-box"/>
<text x="173.0" y="128" text-anchor="middle" class="dg-t">Paramètres Windows</text>
<rect x="126" y="144" width="72" height="26" rx="6" class="dg-soft"/>
<text x="162.1" y="162" text-anchor="middle" class="dg-s">Scripts</text>
<text x="208.2" y="162" text-anchor="start" class="dg-i">démarrage / arrêt</text>
<rect x="126" y="178" width="202" height="26" rx="6" class="dg-box"/>
<text x="227.0" y="196" text-anchor="middle" class="dg-t">Paramètres de sécurité</text>
<rect x="164" y="212" width="165" height="26" rx="6" class="dg-soft"/>
<text x="246.3" y="230" text-anchor="middle" class="dg-s">Stratégies de comptes</text>
<text x="338.6" y="230" text-anchor="start" class="dg-i">MDP, verrouillage</text>
<rect x="164" y="246" width="145" height="26" rx="6" class="dg-soft"/>
<text x="236.4" y="264" text-anchor="middle" class="dg-s">Stratégies locales</text>
<text x="318.8" y="264" text-anchor="start" class="dg-i">audit, droits</text>
<rect x="164" y="280" width="132" height="26" rx="6" class="dg-soft"/>
<text x="229.8" y="298" text-anchor="middle" class="dg-s">Pare-feu Windows</text>
<rect x="88" y="314" width="218" height="26" rx="6" class="dg-box"/>
<text x="197.0" y="332" text-anchor="middle" class="dg-t">Modèles d&#x27;administration</text>
<rect x="126" y="348" width="145" height="26" rx="6" class="dg-soft"/>
<text x="198.4" y="366" text-anchor="middle" class="dg-s">Composants Windows</text>
<rect x="126" y="382" width="66" height="26" rx="6" class="dg-soft"/>
<text x="158.8" y="400" text-anchor="middle" class="dg-s">Bureau</text>
<rect x="126" y="416" width="66" height="26" rx="6" class="dg-soft"/>
<text x="158.8" y="434" text-anchor="middle" class="dg-s">Réseau</text>
<rect x="50" y="450" width="226" height="26" rx="6" class="dg-ok"/>
<text x="163.0" y="468" text-anchor="middle" class="dg-t">Configuration Utilisateur</text>
<text x="286.0" y="468" text-anchor="start" class="dg-i">appliquée à l&#x27;utilisateur à l&#x27;ouverture de session</text>
<rect x="88" y="484" width="158" height="26" rx="6" class="dg-soft"/>
<text x="167.0" y="502" text-anchor="middle" class="dg-s">Paramètres logiciels</text>
<rect x="88" y="518" width="170" height="26" rx="6" class="dg-box"/>
<text x="173.0" y="536" text-anchor="middle" class="dg-t">Paramètres Windows</text>
<rect x="126" y="552" width="72" height="26" rx="6" class="dg-soft"/>
<text x="162.1" y="570" text-anchor="middle" class="dg-s">Scripts</text>
<text x="208.2" y="570" text-anchor="start" class="dg-i">connexion / déconnexion</text>
<rect x="126" y="586" width="211" height="26" rx="6" class="dg-soft"/>
<text x="231.39999999999998" y="604" text-anchor="middle" class="dg-s">Paramètres Internet Explorer</text>
<rect x="88" y="620" width="218" height="26" rx="6" class="dg-box"/>
<text x="197.0" y="638" text-anchor="middle" class="dg-t">Modèles d&#x27;administration</text>
<rect x="126" y="654" width="66" height="26" rx="6" class="dg-soft"/>
<text x="158.8" y="672" text-anchor="middle" class="dg-s">Bureau</text>
<rect x="126" y="688" width="184" height="26" rx="6" class="dg-soft"/>
<text x="218.2" y="706" text-anchor="middle" class="dg-s">Panneau de configuration</text>
<rect x="126" y="722" width="244" height="26" rx="6" class="dg-soft"/>
<text x="247.89999999999998" y="740" text-anchor="middle" class="dg-s">Menu Démarrer et barre des tâches</text>
</svg>
<figcaption>Une GPO contient une configuration ordinateur et une configuration utilisateur.</figcaption>
</figure>

---

#### F. La Console GPMC (Group Policy Management Console)

La console centrale pour gérer les GPO dans un domaine.

**Accès :**
- Gestionnaire de serveur → Outils → Gestion des stratégies de groupe
- Ou `Win+R` → `gpmc.msc`

**Interface GPMC :**
<figure class="graphe diagramme"><svg viewBox="0 0 477 348" style="max-width:477px" role="img" aria-label="Arborescence de la console GPMC" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="102" y1="102" x2="102" y2="123" class="dg-line"/>
<line x1="102" y1="123" x2="126" y2="123" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="157" class="dg-line"/>
<line x1="140" y1="157" x2="164" y2="157" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="191" class="dg-line"/>
<line x1="140" y1="191" x2="164" y2="191" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="225" class="dg-line"/>
<line x1="140" y1="225" x2="164" y2="225" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="293" class="dg-line"/>
<line x1="140" y1="293" x2="164" y2="293" class="dg-line"/>
<line x1="178" y1="238" x2="178" y2="259" class="dg-line"/>
<line x1="178" y1="259" x2="202" y2="259" class="dg-line"/>
<line x1="178" y1="306" x2="178" y2="327" class="dg-line"/>
<line x1="178" y1="327" x2="202" y2="327" class="dg-line"/>
<rect x="12" y="8" width="282" height="26" rx="6" class="dg-dark"/>
<text x="153.0" y="26" text-anchor="middle" class="dg-w">Gestion des stratégies de groupe</text>
<rect x="50" y="42" width="194" height="26" rx="6" class="dg-box"/>
<text x="147.0" y="60" text-anchor="middle" class="dg-t">Forêt : techpro.local</text>
<rect x="88" y="76" width="90" height="26" rx="6" class="dg-box"/>
<text x="133.0" y="94" text-anchor="middle" class="dg-t">Domaines</text>
<rect x="126" y="110" width="130" height="26" rx="6" class="dg-box"/>
<text x="191.0" y="128" text-anchor="middle" class="dg-t">techpro.local</text>
<rect x="164" y="144" width="194" height="26" rx="6" class="dg-bad"/>
<text x="261.0" y="162" text-anchor="middle" class="dg-t">Default Domain Policy</text>
<text x="368.0" y="162" text-anchor="start" class="dg-i">ne pas modifier</text>
<rect x="164" y="178" width="162" height="26" rx="6" class="dg-bad"/>
<text x="245.0" y="196" text-anchor="middle" class="dg-t">Default DC Policy</text>
<text x="336.0" y="196" text-anchor="start" class="dg-i">ne pas modifier</text>
<rect x="164" y="212" width="146" height="26" rx="6" class="dg-box"/>
<text x="237.0" y="230" text-anchor="middle" class="dg-t">OU_UTILISATEURS</text>
<rect x="202" y="246" width="158" height="26" rx="6" class="dg-soft"/>
<text x="281.0" y="264" text-anchor="middle" class="dg-s">GPO liées à cette OU</text>
<rect x="164" y="280" width="258" height="26" rx="6" class="dg-box"/>
<text x="293.0" y="298" text-anchor="middle" class="dg-t">Objets de stratégie de groupe</text>
<rect x="202" y="314" width="191" height="26" rx="6" class="dg-soft"/>
<text x="297.5" y="332" text-anchor="middle" class="dg-s">Toutes les GPO du domaine</text>
</svg>
<figcaption>Les GPO par défaut ne se modifient pas : on crée ses propres GPO.</figcaption>
</figure>

**Création d'une GPO (bonne pratique) :**
1. Clic droit sur `Objets de stratégie de groupe` → `Nouveau` → nommer la GPO
2. Modifier la GPO (clic droit → Modifier) → Configurer les paramètres dans l'éditeur
3. Lier la GPO à une OU (clic droit sur l'OU → `Lier un objet de stratégie de groupe existant`)

**Avantage :** La GPO est créée indépendamment du lien, et peut être liée à plusieurs OU.

---

#### G. Diagnostiquer les GPO

**Depuis un poste client (ou le DC) :**

```cmd
:: Forcer l'application immédiate des GPO
gpupdate /force

:: Forcer + déconnexion (pour scripts de connexion, profils)
gpupdate /force /logoff

:: Afficher les GPO appliquées à l'utilisateur ET l'ordinateur courant
gpresult /r

:: Rapport HTML détaillé (recommandé pour le diagnostic)
gpresult /h C:\rapport_gpo.html
```

**Lecture de `gpresult /r` :**
```
Résultats du jeu de stratégies résultant pour :
  NomOrdinateur\Utilisateur
  
RÉSULTATS DE LA STRATÉGIE POUR L'ORDINATEUR
  Nom de site       : Default-First-Site-Name
  CN                : SRV-AD-TECHPRO
  ...
  Les GPO suivantes ont été appliquées :
    Default Domain Policy          Lien : techpro.local
    GPO_Securite_MDP               Lien : OU_UTILISATEURS

  Les GPO suivantes n'ont pas été appliquées :
    GPO_Bureau_Direction  Raison : Le filtre de sécurité est absent...
```

**Via GPMC — Modélisation :**
GPMC → clic droit sur `Modélisation de stratégie de groupe` → simuler l'application des GPO sur un utilisateur/PC sans les appliquer réellement (idéal pour tester avant déploiement).

---

### II. DNS sous Windows Server – Zones AD-Intégrées

#### A. DNS Windows : Rappel et Nouveautés

Nous avons vu le DNS avec **Bind9** sous Linux (S08). Windows Server dispose de son propre serveur DNS intégré au rôle **DNS Server**.

Lors de la **promotion en contrôleur de domaine** (S10), le rôle DNS est installé et configuré automatiquement pour le domaine AD (ex : `techpro.local`).

**Différence clé avec Bind9 :**
- Bind9 : zones stockées dans des **fichiers texte** (`/etc/bind/db.techpro.local`)
- DNS Windows : zones stockées soit en **fichiers** (comme Bind9), soit dans **Active Directory** (zones AD-intégrées)

---

#### B. Types de Zones DNS Windows

| **Type** | **Stockage** | **Avantages** | **Inconvénients** |
|----------|-------------|---------------|-------------------|
| **Zone primaire** (standard) | Fichier `.dns` sur le serveur | Simple, compatible tout DNS | Un seul maître d'écriture, pas de réplication AD |
| **Zone secondaire** (standard) | Fichier en lecture seule | Redondance, lecture seule | Transfert de zone à configurer manuellement |
| **Zone de stub** | Pointeurs NS uniquement | Résolution inter-domaines | Limité |
| **Zone AD-intégrée** | Base de données Active Directory | Réplication automatique, sécurité renforcée, mises à jour dynamiques sécurisées | Nécessite un DC |

---

#### C. Zones AD-Intégrées : Avantages

**1. Réplication automatique**
Les enregistrements DNS sont stockés dans la base AD et répliqués automatiquement sur **tous les contrôleurs de domaine** du domaine ou de la forêt. Plus besoin de configurer les transferts de zone manuellement.

**2. Mises à jour dynamiques sécurisées (Secure Dynamic Update)**
Les clients Windows peuvent enregistrer automatiquement leur IP dans le DNS (au démarrage). En mode sécurisé, seule la machine elle-même peut mettre à jour son enregistrement DNS — évite les usurpations.

**3. Résistance à la panne**
Si un DC tombe, les autres DC ont une copie complète de la zone DNS. Le DNS continue de fonctionner.

**4. Gestion via MMC**
La console DNS intégrée à Windows Server est graphique, accessible depuis `dnsmgmt.msc`.

---

#### D. Étendue de Réplication AD-Intégrée

Quand on crée une zone AD-intégrée, on choisit l'étendue de réplication :

| **Étendue** | **Réplication vers…** | **Usage recommandé** |
|-------------|----------------------|----------------------|
| **Tous les contrôleurs DNS du domaine** | Tous les DC qui ont le rôle DNS dans le domaine | Recommandé (standard) |
| **Tous les contrôleurs du domaine** | Tous les DC du domaine, même sans rôle DNS | Rarement utile |
| **Tous les contrôleurs DNS de la forêt** | Tous les DC DNS de toute la forêt | Multi-domaines |

---

#### E. Gérer le DNS Windows : Console DNS

**Accès :**
- `Win+R` → `dnsmgmt.msc`
- Gestionnaire de serveur → Outils → DNS

**Structure de la console :**
<figure class="graphe diagramme"><svg viewBox="0 0 505 382" style="max-width:505px" role="img" aria-label="Structure de la console DNS de Windows Server" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="327" class="dg-line"/>
<line x1="64" y1="327" x2="88" y2="327" class="dg-line"/>
<line x1="102" y1="102" x2="102" y2="123" class="dg-line"/>
<line x1="102" y1="123" x2="126" y2="123" class="dg-line"/>
<line x1="102" y1="102" x2="102" y2="293" class="dg-line"/>
<line x1="102" y1="293" x2="126" y2="293" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="157" class="dg-line"/>
<line x1="140" y1="157" x2="164" y2="157" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="191" class="dg-line"/>
<line x1="140" y1="191" x2="164" y2="191" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="225" class="dg-line"/>
<line x1="140" y1="225" x2="164" y2="225" class="dg-line"/>
<line x1="140" y1="136" x2="140" y2="259" class="dg-line"/>
<line x1="140" y1="259" x2="164" y2="259" class="dg-line"/>
<line x1="102" y1="340" x2="102" y2="361" class="dg-line"/>
<line x1="102" y1="361" x2="126" y2="361" class="dg-line"/>
<rect x="12" y="8" width="50" height="26" rx="6" class="dg-dark"/>
<text x="37.0" y="26" text-anchor="middle" class="dg-w">DNS</text>
<rect x="50" y="42" width="138" height="26" rx="6" class="dg-box"/>
<text x="119.0" y="60" text-anchor="middle" class="dg-t">SRV-AD-TECHPRO</text>
<rect x="88" y="76" width="234" height="26" rx="6" class="dg-box"/>
<text x="205.0" y="94" text-anchor="middle" class="dg-t">Zones de recherche directe</text>
<rect x="126" y="110" width="130" height="26" rx="6" class="dg-ok"/>
<text x="191.0" y="128" text-anchor="middle" class="dg-t">techpro.local</text>
<text x="266.0" y="128" text-anchor="start" class="dg-i">zone AD-intégrée</text>
<rect x="164" y="144" width="72" height="26" rx="6" class="dg-soft"/>
<text x="200.1" y="162" text-anchor="middle" class="dg-s">SOA, NS</text>
<rect x="164" y="178" width="330" height="26" rx="6" class="dg-soft"/>
<text x="328.79999999999995" y="196" text-anchor="middle" class="dg-s">Enregistrements A créés automatiquement par AD</text>
<rect x="164" y="212" width="46" height="26" rx="6" class="dg-soft"/>
<text x="186.9" y="230" text-anchor="middle" class="dg-s">www</text>
<text x="219.8" y="230" text-anchor="start" class="dg-i">A : 192.168.50.100</text>
<rect x="164" y="246" width="79" height="26" rx="6" class="dg-soft"/>
<text x="203.4" y="264" text-anchor="middle" class="dg-s">intranet</text>
<text x="252.8" y="264" text-anchor="start" class="dg-i">CNAME : www</text>
<rect x="126" y="280" width="158" height="26" rx="6" class="dg-soft"/>
<text x="205.0" y="298" text-anchor="middle" class="dg-s">_msdcs.techpro.local</text>
<text x="294.0" y="298" text-anchor="start" class="dg-i">zone interne AD, ne pas toucher</text>
<rect x="88" y="314" width="242" height="26" rx="6" class="dg-box"/>
<text x="209.0" y="332" text-anchor="middle" class="dg-t">Zones de recherche inversée</text>
<rect x="126" y="348" width="210" height="26" rx="6" class="dg-warn"/>
<text x="231.0" y="366" text-anchor="middle" class="dg-t">50.168.192.in-addr.arpa</text>
<text x="346.0" y="366" text-anchor="start" class="dg-i">à créer</text>
</svg>
<figcaption>Zones directes pour nom vers IP, zones inversées pour IP vers nom.</figcaption>
</figure>

---

#### F. Enregistrements DNS Automatiques (AD)

Quand AD est installé, Windows crée automatiquement des **enregistrements SRV** dans la zone DNS. Ces enregistrements permettent aux clients de **localiser les services AD** (contrôleurs de domaine, Kerberos, LDAP…).

**Exemples d'enregistrements SRV auto-créés :**
```
_kerberos._tcp.techpro.local    SRV 0 100 88  SRV-AD-TECHPRO.techpro.local.
_ldap._tcp.techpro.local        SRV 0 100 389 SRV-AD-TECHPRO.techpro.local.
_gc._tcp.techpro.local          SRV 0 100 3268 SRV-AD-TECHPRO.techpro.local.
```

**Ne jamais supprimer ces enregistrements SRV** — le domaine AD ne fonctionnerait plus.

---

#### G. Mises à Jour Dynamiques

Le DNS Windows peut accepter que les clients enregistrent automatiquement leur IP :

- **Non sécurisées** : N'importe qui peut ajouter/modifier un enregistrement (dangereux)
- **Sécurisées** : Seuls les ordinateurs membres du domaine peuvent mettre à jour leur propre enregistrement (recommandé)
- **Aucune** : Pas de mise à jour dynamique (enregistrements manuels uniquement)

**Configuration :** Clic droit sur la zone → `Propriétés` → `Mises à jour dynamiques` → `Sécurisées uniquement`

---

### III. Vocabulaire Clé

| **Terme** | **Définition** |
|-----------|----------------|
| **GPO** | Group Policy Object – ensemble de paramètres appliqués automatiquement à des ordinateurs ou utilisateurs du domaine |
| **GPMC** | Group Policy Management Console – console MMC de gestion des GPO |
| **gpedit.msc** | Éditeur de stratégie de groupe locale (sans domaine) |
| **gpmc.msc** | Console GPMC (avec domaine) |
| **LSDOU** | Ordre d'application des GPO : Local → Site → Domaine → OU |
| **Héritage GPO** | Les GPO liées à une OU parent s'appliquent automatiquement aux OU enfants |
| **Enforced** | Option sur une GPO : force son application même si une OU enfant la contredit |
| **Block Inheritance** | Option sur une OU : empêche l'héritage des GPO parentes |
| **gpupdate /force** | Commande Windows forçant l'application immédiate des GPO |
| **gpresult /r** | Commande affichant les GPO appliquées à l'utilisateur et l'ordinateur courant |
| **Non configuré** | Paramètre GPO neutre : la GPO ne touche pas ce paramètre |
| **Désactivé** | Paramètre GPO forçant la désactivation d'une fonction |
| **Configuration Ordinateur** | Section d'une GPO appliquée au démarrage du PC |
| **Configuration Utilisateur** | Section d'une GPO appliquée à l'ouverture de session |
| **Filtrage GPO** | Limitation de l'application d'une GPO à certains objets/groupes |
| **Fine-Grained Password Policy** | Politique de mot de passe affinée (différente par groupe d'utilisateurs) |
| **Zone AD-intégrée** | Zone DNS stockée dans la base AD, répliquée automatiquement entre DC |
| **Mise à jour dynamique sécurisée** | Enregistrement automatique d'IP par les clients membres du domaine uniquement |
| **dnsmgmt.msc** | Console de gestion DNS Windows |
| **Enregistrement SRV** | Enregistrement DNS indiquant l'emplacement d'un service réseau (LDAP, Kerberos…) |

---

### IV. Exercices d'Entraînement

#### Exercice 1 : Ordre d'Application LSDOU

Un PC `PC-INFO-01` appartient à `OU Informatique` dans `OU_UTILISATEURS`. L'utilisateur `l.martin` se connecte.

Les GPO suivantes sont liées :

| **Niveau** | **GPO** | **Paramètre** | **Valeur** |
|------------|---------|---------------|------------|
| Domaine | GPO_Domaine | MDP min | 8 caract. |
| Domaine | GPO_Domaine | Fond d'écran | logo.jpg |
| OU_UTILISATEURS | GPO_Users | MDP min | 10 caract. |
| OU_UTILISATEURS | GPO_Users | Lecteur H: | \\SRV-AD\Users\%username% |
| OU Informatique | GPO_Info | MDP min | 12 caract. |
| OU Informatique | GPO_Info | Installation logiciels | Autorisée |

**1.** Quelle valeur finale de MDP minimum s'applique ? Pourquoi ?

<details markdown="1">
<summary>Correction de la question 1</summary>

**12 caractères.** Les GPO s'appliquent dans l'ordre **L**ocal, **S**ite, **D**omaine, **OU** : la dernière appliquée l'emporte en cas de conflit. Ici `GPO_Domaine` (8), puis `GPO_Users` (10), puis `GPO_Info` (12) : l'OU la plus proche de l'objet gagne. (En pratique, la politique de mot de passe des comptes du domaine se règle au niveau du domaine ; l'exercice illustre ici l'ordre d'application.)

</details>

**2.** L'utilisateur a-t-il le lecteur H: ? Pourquoi ?

<details markdown="1">
<summary>Correction de la question 2</summary>

**Oui.** `GPO_Users` est liée à `OU_UTILISATEURS` et `OU Informatique` est dans cette OU : la GPO est **héritée** par les OU enfants. Aucune autre GPO ne contredit ce paramètre.

</details>

**3.** L'utilisateur peut-il installer des logiciels ? Pourquoi ?

<details markdown="1">
<summary>Correction de la question 3</summary>

**Oui.** `GPO_Info` est liée directement à `OU Informatique`, et l'autorise. Personne ne l'interdit à un niveau plus proche.

</details>

**4.** Quel fond d'écran s'applique ?

<details markdown="1">
<summary>Correction de la question 4</summary>

`logo.jpg`, défini par `GPO_Domaine`. Il est hérité jusqu'à l'OU, puisque aucune GPO plus proche ne définit un autre fond d'écran.

</details>

**5.** Si la GPO_Domaine est marquée "Enforced" sur le paramètre MDP, quelle valeur s'applique ?

<details markdown="1">
<summary>Correction de la question 5</summary>

**8 caractères.** Une GPO *Enforced* (appliquée de force) ne peut plus être écrasée par les GPO des niveaux inférieurs : l'ordre est inversé, la GPO du domaine prime sur celles des OU.

</details>

---

#### Exercice 2 : Identifier le Problème

Un administrateur crée une GPO `GPO_Fond_Ecran` et la lie à `OU Commercial`. Après `gpupdate /force`, l'utilisateur `s.leroy` n'a toujours pas le nouveau fond d'écran.

Proposer **5 causes possibles** et la vérification à faire pour chacune.

<details markdown="1">
<summary>Correction de l'exercice 2</summary>

Outils de base : `gpresult /r` (GPO appliquées et refusées, avec le motif), `gpresult /h rapport.html`, ou « Résultats de stratégie de groupe » dans GPMC.

| **Cause possible** | **Vérification** |
|--------------------|------------------|
| L'utilisateur `s.leroy` n'est **pas dans `OU Commercial`** (ou le compte est ailleurs) | Position du compte dans ADUC |
| Le lien est **désactivé**, ou la GPO est elle-même désactivée | GPMC : lien activé ? État de la GPO |
| **Filtrage de sécurité** : l'utilisateur n'a pas les droits *Lecture* et *Appliquer la stratégie de groupe* | Onglet Délégation / Étendue de la GPO |
| Une autre GPO ou un **blocage d'héritage / Enforced** impose un paramètre contraire (ordre LSDOU) | `gpresult /r` : section « GPO refusées » |
| Le paramètre est dans la **configuration ordinateur** alors que la GPO ne cible que l'utilisateur (ou l'inverse) | Où le paramètre est défini dans l'éditeur de GPO |
| Les paramètres **utilisateur** ne s'appliquent qu'à l'ouverture de session : il faut se déconnecter puis se reconnecter | Fermer la session et se reconnecter |
| Le poste ne joint pas le **contrôleur de domaine** (DNS faux, réseau) | `nslookup` du domaine, `nltest /dsgetdc:` |

</details>

---

#### Exercice 3 : Zone DNS

**1.** Quelle est la différence entre une zone DNS AD-intégrée et une zone DNS standard (fichier) ?

<details markdown="1">
<summary>Correction de la question 1</summary>

Une zone **AD-intégrée** est stockée dans la base Active Directory et répliquée avec elle entre tous les contrôleurs de domaine (plusieurs serveurs peuvent la modifier, mises à jour dynamiques sécurisées possibles). Une zone **standard** est un fichier `.dns` sur un seul serveur principal, copié vers des secondaires par transfert de zone.

</details>

**2.** Pourquoi Windows crée-t-il automatiquement des enregistrements SRV dans la zone DNS du domaine ?

<details markdown="1">
<summary>Correction de la question 2</summary>

Les enregistrements **SRV** indiquent aux clients **où se trouvent les services** du domaine (LDAP, Kerberos, catalogue global, contrôleur de domaine le plus proche). C'est grâce à eux qu'un poste trouve un DC pour joindre le domaine et ouvrir une session. Le service Netlogon les enregistre à la promotion et à chaque démarrage.

</details>

**3.** Un client W10 joint au domaine devrait enregistrer automatiquement son IP dans le DNS. Quel type de mise à jour dynamique activer ? Pourquoi ?

<details markdown="1">
<summary>Correction de la question 3</summary>

**Sécurisée uniquement** (disponible pour les zones AD-intégrées). Seuls les ordinateurs authentifiés du domaine peuvent créer ou modifier leurs enregistrements : un poste non autorisé ne peut pas usurper le nom d'une machine ni polluer la zone.

</details>

---

### V. Auto-évaluation : Suis-je Prêt ?

- [ ] Expliquer l'ordre LSDOU et pourquoi l'OU gagne sur le domaine
- [ ] Distinguer "Non configuré", "Activé" et "Désactivé" dans une GPO
- [ ] Expliquer le principe de l'héritage GPO
- [ ] Décrire les effets de "Enforced" et "Block Inheritance"
- [ ] Ouvrir la console GPMC (`gpmc.msc`)
- [ ] Créer une GPO et la lier à une OU
- [ ] Configurer une politique de mots de passe dans une GPO
- [ ] Configurer un fond d'écran imposé dans une GPO
- [ ] Mapper un lecteur réseau via GPO
- [ ] Utiliser `gpupdate /force` et interpréter `gpresult /r`
- [ ] Expliquer l'avantage d'une zone DNS AD-intégrée
- [ ] Créer une zone DNS AD-intégrée dans la console DNS
- [ ] Ajouter des enregistrements A, CNAME dans la console DNS Windows
- [ ] Tester la résolution avec `nslookup` et `Resolve-DnsName`
