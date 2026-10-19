# Cours – Supervision, sauvegarde et haute disponibilité

*BTS SIO SISR 1re année – Bloc 2 – S16 – à lire avant le [TP](02-tp.md)*

> [!NOTE] Ce que tu dois savoir déjà
> - **Prérequis :** planifier une tâche avec `cron` et se connecter en SSH (S13), filtrer des flux et lire une capture (S15).
> - **Mots déjà vus :** service, port, cron, SSH, DMZ.
> - **Expliqués dans ce cours :** supervision et alerte, sonde et seuil, sauvegarde 3-2-1, RPO et RTO, haute disponibilité, répartition de charge.
> - **Trois notions voisines, trois objectifs différents :** ne les confonds pas (§ 1).

## À la fin de ce cours, tu sauras

- [ ] distinguer **supervision**, **sauvegarde** et **haute disponibilité** ;
- [ ] expliquer ce que mesure une **sonde**, un **seuil**, une **alerte** ;
- [ ] appliquer la règle de sauvegarde **3-2-1** et expliquer pourquoi **tester la restauration** est indispensable ;
- [ ] **calculer** et **interpréter** un RPO et un RTO ;
- [ ] expliquer la **répartition de charge** et la **tolérance de panne**.

---

## 1. Trois notions, trois questions

| Notion | Question à laquelle elle répond | Exemple |
|---|---|---|
| **Supervision** | « Est-ce que ça marche **maintenant** ? Le saurai-je avant les utilisateurs ? » | Une alerte quand le site web ne répond plus |
| **Sauvegarde** | « Si je perds les données ou le serveur, **puis-je revenir en arrière** ? » | Une copie chiffrée du dossier des utilisateurs |
| **Haute disponibilité** | « Le service peut-il **continuer** malgré la panne d'un élément ? » | Deux serveurs web derrière un répartiteur |

> [!WARNING]
> **Le RAID n'est pas une sauvegarde.** Il protège de la panne d'un disque, pas d'une suppression par erreur, d'un rançongiciel ou d'un incendie : la suppression est copiée instantanément sur tous les disques.

## 2. La supervision

**Superviser**, c'est surveiller en continu l'état des équipements et des services, et **prévenir** quand quelque chose sort de la normale.

| Élément | Définition | Exemple |
|---|---|---|
| **Hôte** | Machine ou équipement surveillé | Serveur web, switch, imprimante |
| **Sonde** (ou *item*) | Une mesure collectée régulièrement | Utilisation du processeur, port 80 ouvert, espace disque |
| **Seuil** | Valeur à partir de laquelle la mesure devient anormale | Disque plein à plus de 90 % |
| **Déclencheur** (*trigger*) | Condition « seuil franchi » | `service web = arrêté pendant 2 minutes` |
| **Alerte** | Message envoyé à un humain | Courriel, SMS, notification |

Deux manières de collecter : avec un **agent** installé sur la machine (il remonte les mesures, comme l'agent Zabbix), ou **sans agent** via un protocole d'interrogation : **SNMP** pour les équipements réseau (switch, routeur, imprimante), ou un simple test de port.

> [!TIP]
> **Une bonne alerte est actionnable.** Trop d'alertes inutiles finissent ignorées. On règle les seuils pour qu'un message signifie « quelqu'un doit agir », et on prévoit à qui il est envoyé.

Outils courants : **Zabbix** (complet, avec agents), **Nagios** et ses dérivés, **Centreon**, **Prometheus** associé à Grafana, **Uptime Kuma** (surveillance de disponibilité simple). Le principe est le même partout.

**Je vérifie.** Un serveur de fichiers est « disponible » (il répond au ping) mais ses utilisateurs ne peuvent plus enregistrer. Quelle sonde aurait permis de le voir ?

<details markdown="1">
<summary>Voir la réponse</summary>

Une sonde sur l'**espace disque libre** (disque plein) : le serveur répond au ping alors que son service est dégradé. Une bonne supervision surveille le **service rendu** (port, espace, charge), pas seulement la présence de la machine.

</details>

## 3. La sauvegarde

### La règle 3-2-1

| Chiffre | Règle |
|---|---|
| **3** | Trois exemplaires des données (l'original et deux copies) |
| **2** | Sur deux types de supports différents (disque local, NAS, bande, nuage) |
| **1** | Dont une copie **hors site** (autre bâtiment, autre lieu, nuage) |

### Types de sauvegarde

| Type | Contenu | Avantage | Inconvénient |
|---|---|---|---|
| **Complète** | Tout | Restauration simple | Longue, volumineuse |
| **Différentielle** | Les changements depuis la dernière complète | Restauration en 2 étapes | Grossit au fil des jours |
| **Incrémentale** | Les changements depuis la dernière sauvegarde (quelle qu'elle soit) | Rapide, petite | Restauration en chaîne : plus longue |

### Une sauvegarde non testée n'existe pas

Une sauvegarde n'a de valeur que si on **sait restaurer**. On teste donc régulièrement : restaurer dans un dossier à part, comparer à l'original, **mesurer le temps** que cela prend.

> [!WARNING]
> Une copie qui reste connectée et modifiable (un dossier partagé en lecture-écriture, par exemple) peut être chiffrée en même temps que l'original par un rançongiciel. Au moins une copie doit être **hors ligne ou en lecture seule**.

## 4. RPO et RTO

Deux chiffres que le client **fixe** et que l'administrateur **tient** :

| Sigle | Nom | Question |
|---|---|---|
| **RPO** | *Recovery Point Objective* : perte de données maximale admise | « Combien de **temps de travail** puis-je perdre ? » |
| **RTO** | *Recovery Time Objective* : durée maximale d'interruption admise | « Combien de temps le service peut-il être **arrêté** ? » |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 279.10 276.60" style="max-width:372px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 265.8)">
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M183,-255C183,-255 12,-255 12,-255 6,-255 0,-249 0,-243 0,-243 0,-230 0,-230 0,-224 6,-218 12,-218 12,-218 183,-218 183,-218 189,-218 195,-224 195,-230 195,-230 195,-243 195,-243 195,-249 189,-255 183,-255"/>
<text text-anchor="start" x="19" y="-233" font-family="Helvetica,sans-Serif" font-size="15.00">Dernière sauvegarde</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M127.5,-146C127.5,-146 67.5,-146 67.5,-146 61.5,-146 55.5,-140 55.5,-134 55.5,-134 55.5,-121 55.5,-121 55.5,-115 61.5,-109 67.5,-109 67.5,-109 127.5,-109 127.5,-109 133.5,-109 139.5,-115 139.5,-121 139.5,-121 139.5,-134 139.5,-134 139.5,-140 133.5,-146 127.5,-146"/>
<text text-anchor="start" x="74.5" y="-124" font-family="Helvetica,sans-Serif" font-size="15.00">Panne</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M97.5,-217.83C97.5,-200.79 97.5,-174.78 97.5,-155.22"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="100.65,-155.2 97.5,-146.2 94.35,-155.2 100.65,-155.2"/>
<text text-anchor="start" x="97.5" y="-178.6" font-family="Helvetica,sans-Serif" font-size="13.00">perte de données (RPO)</text>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M159,-37C159,-37 36,-37 36,-37 30,-37 24,-31 24,-25 24,-25 24,-12 24,-12 24,-6 30,0 36,0 36,0 159,0 159,0 165,0 171,-6 171,-12 171,-12 171,-25 171,-25 171,-31 165,-37 159,-37"/>
<text text-anchor="start" x="43" y="-15" font-family="Helvetica,sans-Serif" font-size="15.00">Service rétabli</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M97.5,-108.83C97.5,-91.79 97.5,-65.78 97.5,-46.22"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="100.65,-46.2 97.5,-37.2 94.35,-46.2 100.65,-46.2"/>
<text text-anchor="start" x="97.5" y="-69.6" font-family="Helvetica,sans-Serif" font-size="13.00">interruption (RTO)</text>
</g>
</g>
</svg>
</figure>

**Exemple.** Une sauvegarde est faite chaque nuit à 2 h. La panne survient à 15 h : on perd **13 h** de travail. Au pire (panne juste avant 2 h), on perd **près de 24 h**. Le RPO réel de cette stratégie est donc d'environ **24 h**. Si le client exige un RPO d'une heure, il faut des sauvegardes plus fréquentes ou une réplication.

**Je vérifie.** Un client exige un RPO de 4 heures et un RTO de 1 heure. Une sauvegarde complète est faite chaque nuit ; la restauration complète prend 3 heures. La stratégie convient-elle ?

<details markdown="1">
<summary>Voir la réponse</summary>

**Non, sur les deux points.** Avec une sauvegarde quotidienne, on peut perdre jusqu'à 24 h (RPO de 4 h non tenu). La restauration dure 3 h (RTO de 1 h non tenu). Il faut des sauvegardes plus fréquentes (incrémentales toutes les 4 h, par exemple) et une restauration plus rapide, ou une solution de **haute disponibilité** qui évite la restauration.

</details>

## 5. La haute disponibilité

La **disponibilité** est la part du temps où le service est utilisable. On l'exprime en pourcentage :

| Disponibilité | Arrêt toléré par an (environ) |
|---|---|
| 99 % | 3 jours et 15 heures |
| 99,9 % | 8 heures 45 |
| 99,99 % | 52 minutes |

La **haute disponibilité** (HA) supprime les **points uniques de défaillance** : si un élément tombe, un autre prend le relais.

| Technique | Principe | Exemple |
|---|---|---|
| **Redondance** | Doubler un élément | Deux alimentations, deux cartes réseau, RAID |
| **Répartition de charge** (*load balancing*) | Un **répartiteur** distribue les demandes entre plusieurs serveurs identiques | Deux serveurs web derrière HAProxy |
| **Bascule** (*failover*) | Un serveur de secours prend le relais d'un serveur en panne | Adresse IP flottante entre deux machines (VRRP, *keepalived*) |
| **Test de santé** (*health check*) | Le répartiteur vérifie que chaque serveur répond et retire celui qui tombe | `check` dans la configuration HAProxy |

<figure class="graphe diagramme">
<svg viewBox="0.00 0.00 355.60 408.66" style="max-width:475px" role="img" aria-label="Schéma" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g class="graph" transform="scale(1 1) rotate(0) translate(10.8 397.86)">
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M216.5,-387.06C216.5,-387.06 116.5,-387.06 116.5,-387.06 110.5,-387.06 104.5,-381.06 104.5,-375.06 104.5,-375.06 104.5,-362.06 104.5,-362.06 104.5,-356.06 110.5,-350.06 116.5,-350.06 116.5,-350.06 216.5,-350.06 216.5,-350.06 222.5,-350.06 228.5,-356.06 228.5,-362.06 228.5,-362.06 228.5,-375.06 228.5,-375.06 228.5,-381.06 222.5,-387.06 216.5,-387.06"/>
<text text-anchor="start" x="123.5" y="-365.06" font-family="Helvetica,sans-Serif" font-size="15.00">Utilisateurs</text>
</g>
<g class="node n&#45;hex">
<polygon fill="none" stroke="black" stroke-width="1.8" points="332.5,-247.97 249.5,-292.15 83.5,-292.15 0.5,-247.97 83.5,-203.78 249.5,-203.78 332.5,-247.97"/>
<text text-anchor="start" x="84" y="-252.97" font-family="Helvetica,sans-Serif" font-size="15.00">Répartiteur de charge</text>
<text text-anchor="start" x="134.5" y="-235.97" font-family="Helvetica,sans-Serif" font-size="15.00">HAProxy</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M166.5,-349.8C166.5,-336.99 166.5,-318.98 166.5,-301.72"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="169.65,-301.24 166.5,-292.24 163.35,-301.24 169.65,-301.24"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M135,-145.88C135,-145.88 12,-145.88 12,-145.88 6,-145.88 0,-139.88 0,-133.88 0,-133.88 0,-120.88 0,-120.88 0,-114.88 6,-108.88 12,-108.88 12,-108.88 135,-108.88 135,-108.88 141,-108.88 147,-114.88 147,-120.88 147,-120.88 147,-133.88 147,-133.88 147,-139.88 141,-145.88 135,-145.88"/>
<text text-anchor="start" x="19" y="-123.88" font-family="Helvetica,sans-Serif" font-size="15.00">Serveur web 1</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M132.66,-203.82C119.31,-186.78 104.44,-167.83 92.95,-153.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="95.31,-151.08 87.28,-145.94 90.35,-154.97 95.31,-151.08"/>
</g>
<g class="node n&#45;rect">
<path fill="none" stroke="black" stroke-width="1.8" d="M322,-145.88C322,-145.88 199,-145.88 199,-145.88 193,-145.88 187,-139.88 187,-133.88 187,-133.88 187,-120.88 187,-120.88 187,-114.88 193,-108.88 199,-108.88 199,-108.88 322,-108.88 322,-108.88 328,-108.88 334,-114.88 334,-120.88 334,-120.88 334,-133.88 334,-133.88 334,-139.88 328,-145.88 322,-145.88"/>
<text text-anchor="start" x="206" y="-123.88" font-family="Helvetica,sans-Serif" font-size="15.00">Serveur web 2</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M200.7,-203.82C214.2,-186.78 229.23,-167.83 240.84,-153.18"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="243.45,-154.95 246.57,-145.94 238.51,-151.04 243.45,-154.95"/>
</g>
<g class="node n&#45;cyl">
<path fill="none" stroke="black" stroke-width="1.8" d="M258,-46.2C258,-48.74 216.99,-50.81 166.5,-50.81 116.01,-50.81 75,-48.74 75,-46.2 75,-46.2 75,-4.68 75,-4.68 75,-2.13 116.01,-0.06 166.5,-0.06 216.99,-0.06 258,-2.13 258,-4.68 258,-4.68 258,-46.2 258,-46.2"/>
<path fill="none" stroke="black" stroke-width="1.8" d="M258,-46.2C258,-43.65 216.99,-41.59 166.5,-41.59 116.01,-41.59 75,-43.65 75,-46.2"/>
<text text-anchor="start" x="94" y="-21.94" font-family="Helvetica,sans-Serif" font-size="15.00">Données partagées</text>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M90.13,-108.51C103.2,-94.46 121.74,-74.54 137.28,-57.84"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="139.9,-59.64 143.73,-50.91 135.29,-55.35 139.9,-59.64"/>
</g>
<g class="edge">
<path fill="none" stroke="black" stroke-width="1.8" d="M243.69,-108.51C230.48,-94.46 211.74,-74.54 196.04,-57.84"/>
<polygon fill="black" stroke="black" stroke-width="1.8" points="197.98,-55.3 189.52,-50.91 193.39,-59.62 197.98,-55.3"/>
</g>
</g>
</svg>
</figure>

> [!NOTE]
> **Le répartiteur est lui-même un point unique de défaillance** : pour une vraie haute disponibilité, il est doublé (deux répartiteurs partageant une adresse IP flottante). C'est le sujet de la deuxième année ; ici, on observe la bascule des **serveurs** derrière un répartiteur.

**Je vérifie.** Deux serveurs web derrière un répartiteur gardent chacun une copie **locale** des sessions des utilisateurs. Quel problème se pose quand un serveur tombe ?

<details markdown="1">
<summary>Voir la réponse</summary>

Les utilisateurs connectés au serveur tombé **perdent leur session** et doivent se reconnecter. Pour l'éviter, on partage les sessions (base de données, cache partagé) ou on les stocke côté client.

</details>

## À retenir

- **Supervision** (savoir), **sauvegarde** (revenir en arrière), **haute disponibilité** (continuer) : trois réponses à trois questions.
- **3-2-1** et **restauration testée** : sans test, pas de sauvegarde.
- **RPO** = perte de données tolérée ; **RTO** = interruption tolérée. Ils se **calculent** à partir de la stratégie réelle.
- La haute disponibilité **supprime les points uniques de défaillance**, y compris le répartiteur lui-même.
