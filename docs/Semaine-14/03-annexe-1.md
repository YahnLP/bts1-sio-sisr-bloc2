---
author: YLP
title: 📄 ANNEXE 1
---

# 📄 ANNEXE 1 — FICHE DE CONFIGURATION RÉSEAU INTER-VLAN
## (À compléter et verser au portfolio — Preuve E4/E5)

*Nom : _________________________ Prénom : _________________________ Date : _________*

---

## 1. Schéma de la Topologie

*Dessiner ou coller ici le schéma de la topologie Packet Tracer avec les équipements, les liens, les VLANs et les adresses IP.*

*(Zone réservée au schéma)*

---

## 2. Plan d'Adressage VLAN

| **VLAN** | **Nom** | **Réseau** | **Masque** | **Passerelle** | **Ports Switch** |
|---|---|---|---|---|---|
| 10 | | 192.168.10.0 | | | |
| 20 | | 192.168.20.0 | | | |
| 30 | | 192.168.30.0 | | | |

---

## 3. Configuration des Équipements

### Switch SW-CORE

| **Paramètre** | **Valeur** |
|---|---|
| Hostname | |
| Port trunk | |
| VLAN créés | 10, 20, 30 |
| Port Fa0/1 — VLAN | |
| Port Fa0/2 — VLAN | |
| Port Fa0/3 — VLAN | |

### Routeur R-SIOSARL

| **Sous-interface** | **Encapsulation** | **Adresse IP** | **Masque** |
|---|---|---|---|
| G0/0.10 | dot1Q 10 | | |
| G0/0.20 | dot1Q 20 | | |
| G0/0.30 | dot1Q 30 | | |

---

## 4. Table de Routage du Routeur

*Coller ici le résultat de la commande `show ip route` :*

```
(résultat de show ip route)
```

---

## 5. Tests de Connectivité

| **Source** | **Destination** | **Ping** | **Capture N°** |
|---|---|---|---|
| PC-RH (192.168.10.10) | Passerelle VLAN 10 (192.168.10.1) | ✅ / ❌ | |
| PC-RH (192.168.10.10) | Passerelle VLAN 20 (192.168.20.1) | ✅ / ❌ | |
| PC-RH (192.168.10.10) | Passerelle VLAN 30 (192.168.30.1) | ✅ / ❌ | |
| PC-RH (192.168.10.10) | PC-IT (192.168.20.10) | ✅ / ❌ | |
| PC-RH (192.168.10.10) | PC-DIR (192.168.30.10) | ✅ / ❌ | |

---

## 6. Justification de l'Architecture

*Expliquer en 4 à 6 lignes pourquoi vous avez choisi l'architecture router-on-a-stick plutôt qu'un routeur avec une interface physique par VLAN :*

---
