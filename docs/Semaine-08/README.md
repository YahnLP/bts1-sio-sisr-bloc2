# 01 – Objectifs et ressources


---

## Informations Générales

| **Élément** | **Détails** |
|-------------|-------------|
| **Semaine** | S8 - Année 1 |
| **Bloc** | Bloc 2 - Infrastructure, Systèmes & Réseaux |
| **Durée** | 4 heures (1 séance) |
| **Phase** | Phase 2 - Services réseau, administration & RGPD |
| **Public** | Apprentis BTS SIO SISR (hétérogène : Bac Pro CIEL + débutants) |
| **Prérequis** | S5 (IPv4, masques, calculs réseau) - S6 (CLI Cisco, modes, commandes show) - S7 (VLANs, Linux utilisateurs/permissions) |

---

## Compétences RNCP Visées

| **Code** | **Libellé** | **Niveau** |
|----------|-------------|------------|
| **B2.2** | Installer, tester et déployer une solution d'infrastructure réseau – routage statique, DHCP | **Application** |
| **B2.3** | Exploiter, dépanner et superviser – diagnostic routes, baux DHCP | Application |
| **B2.1** | Administrer les services d'un système d'exploitation serveur – isc-dhcp-server | Application |

---

## Objectifs Pédagogiques

**À l'issue de cette séance, l'apprenant sera capable de :**

1. **Expliquer** le rôle de la passerelle par défaut et de la table de routage
2. **Lire** une table de routage Cisco et identifier les types de routes (C, S, S*)
3. **Configurer** une interface de routeur Cisco (ip address, no shutdown)
4. **Ajouter** des routes statiques avec `ip route` (réseau distant et route par défaut)
5. **Tester** la connectivité inter-réseaux avec ping et `show ip route`
6. **Expliquer** le fonctionnement du DHCP et les 4 étapes DORA
7. **Installer et configurer** le serveur isc-dhcp-server sous Debian
8. **Créer** une étendue DHCP avec plage, masque, passerelle, DNS
9. **Configurer** une réservation DHCP (IP fixe par adresse MAC)
