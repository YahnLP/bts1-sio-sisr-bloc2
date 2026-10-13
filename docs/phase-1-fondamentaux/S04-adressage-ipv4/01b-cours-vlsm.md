# Cours – Plan d'adressage VLSM

*BTS SIO SISR 1re année – Bloc 2 – S04*

## II. Rappel Structuré — Plan d'Adressage VLSM

*(Rappel S4-S5 appliqué au projet S17)*

### II.A. Principe du VLSM

Le **VLSM** (Variable Length Subnet Masking) permet d'attribuer des sous-réseaux de **tailles différentes** à partir d'un même bloc d'adresses, en ajustant le masque selon le nombre réel d'hôtes nécessaires. Contrairement au découpage fixe (même masque pour tout), le VLSM **optimise l'utilisation des adresses**.

**Règle de calcul :** Pour héberger N hôtes, il faut un sous-réseau avec au moins N+2 adresses (une adresse réseau + une adresse broadcast + N hôtes). On prend la puissance de 2 supérieure ou égale à N+2.

```
Formule : 2^n ≥ N + 2  →  choisir la plus petite valeur de n satisfaisant la condition

Exemples :
  25 hôtes → 2^5 = 32 ≥ 27 → /27 (30 hôtes utilisables)
  40 hôtes → 2^6 = 64 ≥ 42 → /26 (62 hôtes utilisables)
  10 hôtes → 2^4 = 16 ≥ 12 → /28 (14 hôtes utilisables)
   5 hôtes → 2^3 =  8 ≥  7 → /29  (6 hôtes utilisables)
```

### II.B. Tableau de Référence des Masques

| **CIDR** | **Masque décimal** | **Hôtes utilisables** | **Adresses totales** |
|---|---|---|---|
| /24 | 255.255.255.0 | 254 | 256 |
| /25 | 255.255.255.128 | 126 | 128 |
| /26 | 255.255.255.192 | 62 | 64 |
| /27 | 255.255.255.224 | 30 | 32 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |

### II.C. Méthode de Découpage VLSM

**Règle d'or :** Toujours allouer les **plus grands sous-réseaux en premier** pour éviter le chevauchement.

Avec le bloc `192.168.0.0/22` (1024 adresses, de 192.168.0.0 à 192.168.3.255) :

| **Ordre** | **VLAN** | **Besoin** | **Taille bloc** | **Réseau alloué** | **Masque** | **Plage hôtes** | **Broadcast** |
|---|---|---|---|---|---|---|---|
| 1 | VLAN 40 Commercial | 40 h | /26 (64) | 192.168.0.0/26 | 255.255.255.192 | .1 → .62 | .63 |
| 2 | VLAN 10 RH | 25 h | /27 (32) | 192.168.0.64/27 | 255.255.255.224 | .65 → .94 | .95 |
| 3 | VLAN 50 Compta | 15 h | /27 (32) | 192.168.0.96/27 | 255.255.255.224 | .97 → .126 | .127 |
| 4 | VLAN 20 Info | 10 h | /28 (16) | 192.168.0.128/28 | 255.255.255.240 | .129 → .142 | .143 |
| 5 | VLAN 99 Serveurs | 10 h | /28 (16) | 192.168.0.144/28 | 255.255.255.240 | .145 → .158 | .159 |
| 6 | VLAN 30 Direction | 5 h | /29 (8) | 192.168.0.160/29 | 255.255.255.248 | .161 → .166 | .167 |

> *Adresses restantes du bloc /22 : disponibles pour extensions futures*

---
