# Aide-mémoire – Matériel et serveurs

*BTS SIO SISR 1re année – Bloc 2 – S01*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Composants

| Composant | Rôle | Repère |
|---|---|---|
| Carte mère | Relie tous les composants | Socket, slots DIMM et PCIe, pile CMOS |
| Processeur (CPU) | Exécute les instructions | Fréquence (GHz), cœurs, threads |
| Mémoire vive (RAM) | Mémoire de travail, volatile | Type (DDR4, DDR5), capacité, fréquence |
| Stockage | Données durables | HDD (mécanique), SSD (flash, SATA ou NVMe) |
| Alimentation (PSU) | Convertit le secteur | Puissance (W), certification 80 Plus |
| Carte graphique (GPU) | Calcule l'image | Intégrée ou dédiée (PCIe x16) |

## Serveur et poste

| Critère | Poste | Serveur |
|---|---|---|
| Alimentation | Une | Souvent deux, remplaçables à chaud |
| Disques | Un ou deux | Baie de plusieurs disques, RAID |
| Administration | Sur place | À distance (carte de gestion) |
| Format | Tour | Rack (hauteur en U) |

## Connectiques vidéo

| Connecteur | Signal | Repère |
|---|---|---|
| VGA | Analogique | Obsolète ; image seule, 15 broches bleues |
| DVI | Numérique (parfois analogique) | En voie de disparition ; image seule |
| **HDMI** | Numérique, image **et son** | Standard TV et multimédia ; HDMI 2.0 : 4K à 60 Hz, HDMI 2.1 : 8K à 60 Hz ; CEC, eARC ; norme à licence |
| **DisplayPort** | Numérique, image **et son** | Standard PC et professionnel ; plusieurs écrans chaînés (MST) ; norme ouverte (VESA) ; clip de verrouillage |
| USB-C (mode alternatif) | Numérique, vidéo + données + alimentation | Portables : sortie DisplayPort ou HDMI par un seul câble |

**Choisir :** HDMI pour un téléviseur ou un vidéoprojecteur, DisplayPort pour un écran de PC (surtout en multi-écrans). Vérifier que **le port, le câble et l'écran** prennent en charge la même version : le maillon le plus faible fixe la résolution et la fréquence. Détail : [cours, partie « Connectiques vidéo »](01b-cours-serveur.md).

## Démarrage

- **BIOS / UEFI** : micrologiciel qui démarre la machine avant le système ; on y règle l'**ordre de démarrage** et la **virtualisation** (VT-x / AMD-V).
- Accès : touche pendant le démarrage (souvent `Suppr`, `F2`, `F10`, `Échap`) : **elle varie selon le constructeur**.
