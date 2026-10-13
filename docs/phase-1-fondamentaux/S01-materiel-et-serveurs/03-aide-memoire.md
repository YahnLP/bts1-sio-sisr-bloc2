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

## Démarrage

- **BIOS / UEFI** : micrologiciel qui démarre la machine avant le système ; on y règle l'**ordre de démarrage** et la **virtualisation** (VT-x / AMD-V).
- Accès : touche pendant le démarrage (souvent `Suppr`, `F2`, `F10`, `Échap`) : **elle varie selon le constructeur**.