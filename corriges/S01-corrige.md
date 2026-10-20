# Corrigé S01 – Matériel, serveurs et machine virtuelle

*Réservé à l'enseignant. Source : [TP S01](../docs/phase-1-fondamentaux/S01-materiel-et-serveurs/02-tp.md).*

## Points de contrôle de l'installation

| Point | Attendu | Si ça échoue |
|---|---|---|
| Virtualisation | VT-x / AMD-V activée dans l'UEFI avant de ​créer la VM | VM très lente ou démarrage refusé : activer dans l'UEFI |
| Création de la VM | 4 Go de RAM, disque VDI dynamique (~40 Go), 2 cœurs, ISO montée | Ordre de démarrage : optique avant disque |
| Réseau | NAT ou accès par pont | Pas de réseau dans l'invité : ​vérifier le mode |
| Installation | Langue FR, édition Pro, installation personnalisée | Appuyer sur Entrée à l'invite « Appuyez sur une touche » |
| Compte local | Compte créé, mot de passe noté | Windows 11 : couper le réseau pour éviter le compte Microsoft |
| Additions invité | Installées, redémarrage fait, plein écran ​fonctionnel | Image CD des Additions non montée |

## « Je comprends » : pourquoi contrôler VT-x / AMD-V avant ?

L'hyperviseur s'appuie sur des fonctions du processeur. Désactivées dans le micrologiciel, la VM ne démarre pas ou très lentement : c'est le premier point à vérifier.

## Questions de réflexion

| N° | Éléments attendus | Attention |
|---|---|---|
| 1 | Composants redondés (alimentations, disques), RAM ECC, processeurs serveur, garantie et support ​étendus, gestion à distance | L'énoncé ne donne plus de multiple chiffré (il n'était pas sourcé) : accepter « nettement plus cher » |
| 2 | Techniquement oui ; pas de redondance, pas de gestion à distance, moins fiable en 24 h/24 | Accepter l'usage en test ou en labo |
| 3 | Ça fonctionne, à la vitesse USB 2.0 (rétrocompatibilité) | |
| 4 | Coût, équipements spécialisés, installation plus délicate | Ne pas conclure que le cuivre est « moins bon » : il ​suffit sur courte distance |
| 5 | UEFI qui émule un BIOS classique pour les anciens systèmes | |
| 6 | Windows charge un pilote selon le mode SATA ; en changeant le mode après installation, le pilote n'est pas chargé | |

## Barème indicatif (20 points)

| Critère | Points |
|---|---|
| VM fonctionnelle (captures ​datées) | 8 |
| Réponses « J'anticipe » et « Je comprends » | 4 |
| Questions de réflexion | 6 |
| Présentation du rendu | 2 |

## Erreurs fréquentes

- Oublier de monter l'ISO ou de régler l'ordre de démarrage.
- Disque alloué en taille fixe : l'espace est consommé d'emblée.
- Confondre mémoire vive de la VM et mémoire vidéo.

## Bilan

Compétences visées : B2.1, B2.2, B2.3, B1.1. Proposition de l'enseignant, à valider. Comparer l'auto-évaluation de l'étudiant aux captures.
