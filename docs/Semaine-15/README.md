# 01 – Informations Générales

| **Champ** | **Détail** |
|-----------|-----------|
| **Semaine** | S15 — Année 1 |
| **Bloc** | Bloc 2 — Administrer les composants d'une infrastructure |
| **Durée totale** | 4 heures |
| **Public** | Apprentis BTS SIO SISR (Bac Pro CIEL + autres bacs) |
| **Modalité** | Présentiel — Salle de TP Linux/Réseau (VMs Debian/Ubuntu) |
| **Prérequis** | Linux bases (S8-S9), Bash variables/conditions (S14), notions réseau TCP/IP, SSH introductif |

---

## 🧠 Compétences travaillées

| **Code** | **Intitulé de la compétence** | **Niveau visé** |
|----------|-------------------------------|-----------------|
| **B2.2** | Installer et configurer un service réseau (LAMP, SSH) | Maîtrise opérationnelle |
| **B2.4** | Exploiter un service en mode script (Bash boucles + cron) | Maîtrise opérationnelle |
| **B2.5** | Assurer la maintenance et la continuité des services (sauvegarde) | Application |
| **B3.2** | Mettre en œuvre et maintenir la sécurité informatique (SSH clés, RDP/WinRM) | Application |

---

## 🎯 Objectifs

À l'issue de cette séance, l'apprenant sera capable de :

- ✅ Installer et configurer un serveur **LAMP** complet (Apache, MariaDB, PHP) sur Debian/Ubuntu
- ✅ Vérifier le bon fonctionnement de chaque composant LAMP
- ✅ Sécuriser une installation MariaDB avec `mysql_secure_installation`
- ✅ Écrire des **boucles `for`** et **`while`** en Bash pour automatiser des traitements répétitifs
- ✅ Configurer une authentification **SSH par clés** (paire publique/privée) et désactiver l'authentification par mot de passe
- ✅ Expliquer les principes et cas d'usage de **RDP** et **WinRM** pour l'administration distante Windows
- ✅ Écrire un **script de sauvegarde automatisée** avec compression et rotation
- ✅ Planifier l'exécution automatique d'un script via **cron**

---

## Prérequis

- Navigation dans un terminal Linux, droits sudo, éditeur `nano`
- Variables Bash, `read`, `echo`, structures `if/then/else` (S14)
- Commandes de base : `cp`, `mkdir`, `chmod`, `chown`, `ls`, `find`
- Notion de ports réseau (HTTP=80, HTTPS=443, SSH=22)
- Adressage IP, connectivité réseau entre VMs

---

