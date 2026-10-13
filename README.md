# Bloc 2 · SISR – BTS SIO 1re année

Cours, TP et aide-mémoire du **bloc 2 (infrastructure, systèmes et réseaux)** du BTS SIO option SISR, 1re année : **19 séances de 4 h** pour 19 semaines d'alternance réparties sur l'année. Auteur : Yahn LE PRETTRE - Formateur – licence CC BY-NC-SA 4.0.

**Site (GitHub Pages) :** <https://yahnlp.github.io/bts1-sio-sisr-bloc2/>

## Contenu

| Dossier | Contenu |
|---|---|
| [`docs/phase-1-fondamentaux`](docs/phase-1-fondamentaux/README.md) | S01 à S05 : matériel, système d'exploitation, comptes et droits, modèle OSI, adressage IPv4, commutation Cisco |
| [`docs/phase-2-reseau`](docs/phase-2-reseau/README.md) | S06 à S09 : VLAN, routage statique et inter-VLAN, DHCP, DNS, STP, TP intégré |
| [`docs/phase-3-services`](docs/phase-3-services/README.md) | S10 à S15 : Active Directory, GPO, partages, Bash, LAMP, SSH, PowerShell, IPv6, pare-feu et DMZ |
| [`docs/phase-4-securiser-presenter`](docs/phase-4-securiser-presenter/README.md) | S16 à S19 : supervision, sauvegarde, haute disponibilité, projet de PME, DAT, épreuve blanche E6 |
| [`docs/ressources`](docs/ressources/README.md) | Référentiel, épreuve E6, fiche de réalisation, outils, glossaire, licences |
| [`docs/simulateur`](docs/simulateur/) | Le simulateur réseau hors ligne (un seul fichier HTML) |
| [`corriges`](corriges/) | Corrigés **réservés à l'enseignant** (non publiés sur le site, mais visibles dans ce dépôt) |
| [`tools`](tools/CONVENTIONS.md) | Conventions de rédaction du site |

## Principe pédagogique

Anticiper → manipuler → comprendre → vérifier → faire le bilan de ses compétences. Chaque TP demande ce que l'on **anticipe** avant de manipuler, explique chaque commande (« Je comprends »), pose des questions « pourquoi » et se termine par un **bilan de compétences** rattaché au référentiel du BTS.

## Cadre d'examen

Toutes les épreuves sont **ponctuelles** (pas de contrôle en cours de formation). Ce bloc prépare l'épreuve **E6** (administration des systèmes et des réseaux : préparation de 1 h 30, oral de 40 min, deux réalisations). Voir [`docs/ressources/epreuve-e6.md`](docs/ressources/epreuve-e6.md).

## Travailler en local

```bash
pip install -r requirements.txt
mkdocs serve        # http://127.0.0.1:8000
mkdocs build --strict
```

## Licence

Contenus : **CC BY-NC-SA 4.0**. Simulateur réseau : **EUPL 1.2**. Détail : [`docs/ressources/licences.md`](docs/ressources/licences.md).
