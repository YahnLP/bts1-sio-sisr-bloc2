# 01 – Objectifs et ressources


---

## Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S14 — Année 1 |
| **Bloc** | Bloc 2 — Administrer les composants d'une infrastructure |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR (Bac Pro CIEL + autres bacs) |
| **Modalité** | Présentiel — Salle de TP réseau (Cisco Packet Tracer + Linux) |
| **Prérequis** | VLANs (S10), Adressage IP et sous-réseaux (S4-S5), bases Linux (S8-S9), DHCP S13 |

---

## Compétences RNCP Visées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B2.2** | Installer et configurer des éléments d'infrastructure réseau | Maîtrise opérationnelle |
| **B2.4** | Exploiter un service en mode script | Maîtrise opérationnelle |
| **B1.3** | Identifier les processus présents dans un système d'exploitation | Application |
| **B3.3** | Participer à la gestion et au suivi d'un projet (automatisation) | Application |

---

## Objectifs Pédagogiques

À l'issue de cette séance, l'apprenant sera capable de :

- ✅ Expliquer pourquoi les VLANs **isolent** le trafic et pourquoi le routage inter-VLAN est nécessaire
- ✅ Décrire et configurer l'architecture **router-on-a-stick** (routeur avec sous-interfaces)
- ✅ Configurer les **sous-interfaces** d'un routeur Cisco avec encapsulation 802.1Q
- ✅ Vérifier la connectivité inter-VLAN avec `ping` et `show ip route`
- ✅ Écrire un script Bash respectant la **syntaxe de base** (shebang, variables, `echo`, `read`)
- ✅ Implémenter des **structures conditionnelles** `if/then/elif/else/fi` dans un script Bash
- ✅ Produire un script complet de **création automatisée d'utilisateurs Linux** avec validation des entrées

## Prérequis

- Notion de VLAN (création, assignation de port, trunk) — S10
- Adressage IP, sous-réseaux, masques — S4-S5
- Navigation dans un terminal Linux, commandes de base (`ls`, `mkdir`, `useradd`, `passwd`) — S8-S9
- Utilisation de Cisco Packet Tracer (interface, configuration CLI Cisco) — S10-S11