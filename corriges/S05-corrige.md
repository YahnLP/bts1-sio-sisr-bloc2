# Corrigé S05 – Configurer un commutateur Cisco

*Réservé à l'enseignant. Source : [TP S05](../docs/phase-1-fondamentaux/S05-commutation-cisco/02-tp.md).*

## Configuration attendue

```cisco
hostname SW_BUREAU_01
no ip domain-lookup
banner motd #
 ACCÈS RÉSERVÉ AU PERSONNEL AUTORISÉ ...
#
enable secret Cisco@2024
service password-encryption
line con 0
 password Console@2024
 login
 exec-timeout 5 0
line vty 0 15
 password Vty@2024
 login
 exec-timeout 5 0
```

Les mots de passe sont ceux du TP (labo).

## Vérification de `show running-config`

| Ligne | Remarque |
|---|---|
| `enable secret 5 $1$...` | Haché ; selon la version de Packet Tracer, le type peut différer |
| `password 7 ...` sur con et vty | Chiffrement réversible faible, d'où l'intérêt de `enable secret` |
| `line vty 0 4` **et** `line vty 5 15` | Le TP écrit `line vty 0 15` : la sortie peut afficher deux blocs ; les deux doivent contenir `login` |
| Banner entre `^C` | Visible à la reconnexion |

## Questions

- **Modes** : le mode Privileged EXEC offre `configure terminal`, `show running-config`, `copy`, etc. ; le mode User n'a que des commandes de consultation limitées.
- **`no ip domain-lookup`** : évite l'attente DNS après une faute de frappe.
- **`enable password` / `enable secret`** : clair (ou faible) contre haché ; utiliser `enable secret`.
- **Table MAC** : vide au départ, puis 2 entrées après le ping Alice → Bob (apprentissage par adresse source) ; Clara n'a rien émis donc n'apparaît pas ; type **DYNAMIC** ; vieillissement par défaut de **300 s**.
- **`running-config` / `startup-config`** : mémoire vive contre mémoire non volatile ; sans `copy running-config startup-config`, la configuration est perdue au redémarrage.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Topologie et IP des PC | 3 |
| Configuration complète (hostname, banner, secrets, lignes) | 8 |
| Vérification par `show` | 3 |
| Table MAC et réponses | 3 |
| Sauvegarde (`copy`) et fichier `.pkt` nommé | 3 |

## Erreurs fréquentes

- Oublier `login` : le mot de passe n'est pas demandé.
- Oublier `copy running-config startup-config`.
- Banner mal terminé (caractère `#` manquant).
