# 01 – Objectifs et ressources

## Informations Générales

| **Élément** | **Détails** |
|-------------|-------------|
| **Semaine** | S9 - Année 1 |
| **Bloc** | Bloc 2 - Infrastructure, Systèmes & Réseaux |
| **Durée** | 4 heures (1 séance) |
| **Phase** | Phase 2 - Services réseau, administration & RGPD |
| **Public** | Apprentis BTS SIO SISR (hétérogène : Bac Pro CIEL + débutants) |
| **Prérequis** | S5 (IPv4, ARP, ICMP, ping, Debian) – S6 (CLI Cisco, switch, table MAC) – S8 (routage statique, DHCP, isc-dhcp-server) 

---

## Compétences RNCP Visées

| **Code** | **Libellé** | **Niveau** |
|----------|-------------|------------|
| **B2.2** | Installer, tester et déployer une solution d'infrastructure – DNS, topologie commutée | **Application** |
| **B2.3** | Exploiter, dépanner et superviser – résolution DNS, diagnostic STP | Application |
| **B2.1** | Administrer les services d'un système d'exploitation serveur – Bind9 | Application |

---

## Objectifs Pédagogiques

**À l'issue de cette séance, l'apprenant sera capable de :**

1. **Expliquer** le rôle du DNS et le processus de résolution de noms (récursif, itératif)
2. **Distinguer** zone directe (nom → IP) et zone inverse (IP → nom)
3. **Lire et créer** des enregistrements DNS (A, PTR, CNAME, NS, SOA)
4. **Installer et configurer** le serveur DNS Bind9 sous Debian
5. **Tester** la résolution DNS avec `dig`, `nslookup`, `host`
6. **Expliquer** pourquoi les boucles réseau sont dangereuses et comment STP les évite
7. **Décrire** l'élection du root bridge (priorité, MAC) et les états STP des ports
8. **Observer** le comportement STP sur 3 switches dans Packet Tracer
