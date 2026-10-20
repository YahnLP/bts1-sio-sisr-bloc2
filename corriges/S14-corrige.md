# Corrigé S14 – PowerShell et IPv6

*Réservé à l'enseignant. Source : [TP S14](../docs/phase-3-services/S14-powershell-ipv6/02-tp.md).*

## Partie A : pipeline

| Question | Commande attendue |
|---|---|
| Services ​arrêtés | `(Get-Service \| Where-Object Status -eq 'Stopped').Count` ou `... \| Measure-Object` |
| Processus le plus gourmand en RAM | `Get-Process \| Sort-Object WorkingSet -Descending \| Select-Object -First 1` |
| Comptes AD désactivés | `Get-ADUser -Filter 'Enabled -eq $false'` ou `Search-ADAccount -AccountDisabled -UsersOnly` |

**J'anticipe** : les 5 processus ayant consommé le plus de processeur. PowerShell transmet des **objets** ; on trie ou filtre sur une propriété sans analyser du texte.

## Partie B : création en masse

| Point | Attendu |
|---|---|
| Module | `ActiveDirectory` disponible, plusieurs ​dizaines de cmdlets |
| OU | RH, Informatique, Comptabilite, Direction, Commercial |
| CSV | 10 lignes, accents corrects (`Chargée RH`) |
| Premier passage | 10 comptes ​créés dans les bonnes OU, activés, changement de mot de passe obligatoire |
| Second passage | 10 comptes **ignorés** (doublons), rapport final cohérent |
| Journal | `C:\Scripts\log_creation_ad.txt` horodaté |

**Je comprends** : une erreur sur un compte (doublon, mot de passe refusé) ne doit ni arrêter les ​suivants ni passer inaperçue ; le `catch` la journalise et le script continue.

## Partie C : IPv6

| Adresse compressée | Forme complète |
|---|---|
| `::1` | `0000:0000:0000:0000:0000:0000:0000:0001` |
| `FE80::1` | `FE80:0000:0000:0000:0000:0000:0000:0001` |
| `2001:db8::1` | `2001:0DB8:0000:0000:0000:0000:0000:0001` |
| `2001:db8:0:1::` | `2001:0DB8:0000:0001:0000:0000:0000:0000` |
| `FF02::1` | `FF02:0000:0000:0000:0000:0000:0000:0001` |

| Adresse complète | Forme la plus courte |
|---|---|
| `2001:0DB8:0000:0000:0000:0000:0000:0001` | `2001:db8::1` |
| `FE80:0000:0000:0000:0204:61FF:FE9D:F156` | `fe80::204:61ff:fe9d:f156` |
| `0000:0000:0000:0000:0000:0000:0000:0001` | `::1` |
| `2001:0DB8:ABCD:0012:0000:0000:0000:0001` | `2001:db8:abcd:12::1` |
| `FF02:0000:0000:0000:0000:0000:0000:0001` | `ff02::1` |

| Adresse | Type | Routable sur ​Internet |
|---|---|---|
| `FE80::1` | Lien local (link-local) | Non |
| `::1` | Bouclage (loopback) | Non |
| `2001:db8::1234` | Documentation (`2001:db8::/32`) | Non (réservée) |
| `FC00::1` | Locale unique (ULA, `fc00::/7`) | Non |
| `FF02::1` | Multidiffusion, tous les nœuds du lien | Non |
| `::` | Non spécifiée | Non |

**C4 (EUI-64)** : MAC `00:50:56:AB:CD:EF` → insertion de `FF:FE` → inversion du bit U/L (`00` devient `02`) → identifiant `0250:56FF:FEAB:CDEF` → adresse link-local **`FE80::250:56FF:FEAB:CDEF`**.

## Points d'attention pour l'enseignant

- L'appel du ​script s'écrit `& "C:\Scripts\creer_utilisateurs_ad.ps1"` (l'ancienne forme `.\C:\...` était invalide et a été corrigée).
- Enregistrer le CSV en UTF-8 (avec BOM sous Windows PowerShell 5.1) sinon les accents sont altérés.
- Mot de passe `Nexio!2024` : voulu (labo).
- Plusieurs OU sont créées à la ​racine du domaine, contrairement à l'arborescence de S10 : accepter.

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Partie A : exploration et pipeline | 4 |
| Script fonctionnel (création, gestion d'erreurs, journal) | 8 |
| Test des doublons | 2 |
| IPv6 (décompression, compression, ​types, EUI-64) | 5 |
| Captures et réponses | 1 |

## Erreurs fréquentes

- Politique d'exécution non modifiée : le script ne se lance pas.
- Délimiteur du CSV (`;` en configuration française) différent de celui du script.
- Compression IPv6 : deux `::` dans la même adresse.
- Oublier le bit U/L en EUI-64.
- ​Confondre adresse locale unique (ULA) et lien local.
