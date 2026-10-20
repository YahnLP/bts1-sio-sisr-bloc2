# Corrigé S06 – VLAN et trunk entre deux commutateurs

*Réservé à l'enseignant. Source : [TP S06](../docs/phase-2-reseau/S06-vlan-trunk/02-tp.md).*

## Configuration attendue (identique sur SW-A et SW-B)

```cisco
vlan 10
 name DIRECTION
vlan 20
 name COMMERCIAL
vlan 30
 name INFORMATIQUE
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 30
interface FastEthernet0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

## Vérifications

| Commande | Attendu |
|---|---|
| `show vlan brief` | Fa0/1 en VLAN 10, Fa0/2 en VLAN 20, Fa0/3 en VLAN 30 ; Fa0/24 **absent** |
| `show interfaces trunk` | Fa0/24 en mode `on`, encapsulation 802.1q, VLAN autorisés 10,20,30 |

## Tests de validation

| Test | Résultat | Explication |
|---|---|---|
| 1 Dir-A → Dir-B | ​Succès | Même VLAN, trunk entre les switchs (le premier ping peut échouer : ARP) |
| 2 Com-A → Com-B | Succès | Idem |
| 3 Dir-A → Com-A | Échec | Réseaux IP différents, passerelle non simulée, VLAN différents |
| 4 Dir-A → Info-B | Échec | Idem |

## Questions

1. **Table MAC du VLAN 10** : au moins 2 entrées (PC-Dir-A sur Fa0/1, PC-Dir-B apprise sur Fa0/24).
2. **Fa0/24 dans `show vlan brief`** : non, la commande liste les ports *access* ; les trunks ​apparaissent dans `show interfaces trunk`.
3. **Native VLAN** : VLAN dont les trames circulent **sans étiquette** sur le trunk (VLAN 1 par défaut).

**J'anticipe** (avant VLAN) : oui, tous les ports sont dans le VLAN 1. **J'anticipe** (VLAN 10 / VLAN 20) : non, domaines de diffusion séparés ; il faut un routeur ou un switch de niveau 3 pour les relier.
**Je comprends** : avec un lien *access* VLAN 10, seul ce VLAN passe ; le trunk transporte plusieurs VLAN grâce à l'étiquette 802.1Q.

## TP complémentaire : utilisateurs, groupes et permissions sous Linux

*Source : [TP 02b](../docs/phase-2-reseau/S06-vlan-trunk/02b-tp-linux.md).*

| Étape | Attendu |
|---|---|
| Groupes et comptes | 3 groupes (`direction`, `commercial`, `informatique`), 6 comptes ; `id alice` ​montre le groupe `direction` |
| Répertoires | `/data/direction`, `/data/commercial`, `/data/informatique` en `drwxrwx---`, propriétaire `root`, groupe du service |
| Informatique | `eric` et `fiona` ajoutés à `direction` et `commercial` avec `usermod -aG` ; `groups eric` liste les trois groupes |
| Test `alice` | `ls /data/direction` réussit, `ls /data/commercial` refusé |
| Test `charlie` | Crée un fichier dans `/data/commercial`, refusé sur `/data/direction` |
| Test `eric` | Accède à `direction` et `commercial` |
| Exercices | `600` donne `-rw-------`, `755` donne `-rwxr-xr-x`, `750` donne `drwxr-x---` |

**J'anticipe** (`770`, `root:direction`) : `root` et les membres du groupe ont tous les droits ; les ​autres n'ont aucun accès, pas même d'entrer.
**Je comprends** (« Permission denied ») : 1) propriétaire et groupe du dossier (`ls -ld`) ; 2) droits pour propriétaire, groupe, autres ; 3) appartenance de l'utilisateur au groupe (`id`) : après `usermod -aG`, une nouvelle session est nécessaire.

Points d'attention : le message d'erreur dépend de la langue du système (« Permission non accordée » en ​français). Dans l'exercice 1, le fichier de `/home/alice` appartient à `alice` seulement s'il est créé par `alice` ; créé avec `sudo`, il appartient à `root`. Le TP indique les mots de passe des comptes (labo).

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Topologie et adressage | 3 |
| VLAN et ports access (SW-A et SW-B) | 6 |
| ​Trunk fonctionnel | 4 |
| Tests et analyse | 4 |
| Questions, sauvegardes | 3 |

## Erreurs fréquentes

- VLAN non créés sur **les deux** switchs, ou noms différents.
- Trunk configuré d'un seul côté ; VLAN absent de la ​liste `allowed`.
- Port laissé en mode dynamique (pas de `switchport mode access`).
- Conclure que l'échec du test 3 prouve l'isolation VLAN : il est aussi dû aux sous-réseaux différents (l'énoncé le signale).
