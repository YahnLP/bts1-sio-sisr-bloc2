# Cours – Présenter une infrastructure à l'oral

*BTS SIO SISR 1re année – Bloc 2 – S19*

## PARTIE V — Présentation Technique d'une Infrastructure

### V.A. Structure d'une Présentation Orale SISR (format E4/E5)

Une présentation technique devant un jury suit toujours la même structure professionnelle :

| **Partie** | **Durée** | **Contenu** |
|---|---|---|
| **1. Contexte** | 30 s | "Qui est le client, quel était le besoin" |
| **2. Architecture** | 1 min | "Voici ce que j'ai mis en place" + schéma |
| **3. Choix techniques** | 1 min | "J'ai choisi X plutôt que Y parce que..." |
| **4. Démonstration / Tests** | 30 s | "Voici la preuve que ça fonctionne" |
| **5. Difficultés rencontrées** | 30 s | "J'ai rencontré... j'ai résolu en..." |
| **6. Perspectives** | 30 s | "Pour aller plus loin, on pourrait..." |

**Durée totale : 4 à 5 minutes**

### V.B. Vocabulaire Obligatoire pour la Présentation SimIO

Lors de la présentation du Projet 1 SimIO, ces termes **doivent** apparaître naturellement dans le discours :

| **Terme** | **Dans quel contexte l'utiliser** |
|---|---|
| VLSM | "J'ai découpé le bloc /22 en sous-réseaux à taille variable selon le besoin de chaque service" |
| Router-on-a-stick | "Le routage inter-VLAN est assuré par une interface trunk unique avec des sous-interfaces 802.1Q" |
| Principe du moindre privilège | "Chaque utilisateur SFTP est confiné dans son répertoire par chroot — il n'a accès qu'à ce dont il a besoin" |
| Gestion des Accès (ITIL) | "Les GPO centralisent la gestion des accès sans intervention poste par poste" |
| Authentification vs Autorisation | "L'AD authentifie l'utilisateur, les droits NTFS autorisent l'accès aux ressources" |
| Plan de tests | "La conformité au cahier des charges a été validée par 22 tests documentés" |

### V.C. Questions Typiques du Jury et Éléments de Réponse

| **Question du jury** | **Ce qu'il évalue** | **Éléments de réponse** |
|---|---|---|
| "Pourquoi un /27 pour le VLAN RH plutôt qu'un /26 ?" | Maîtrise du VLSM | "25 hôtes  /27 donne 30 hôtes utilisables, avec 5 de marge. Un /26 aurait gaspillé 37 adresses inutilement." |
| "Que se passe-t-il si le contrôleur de domaine tombe ?" | Notion de résilience | "Tous les services AD tombent (authentification, GPO, DHCP si colocalisé, DNS). En production, on déploierait un DC secondaire en réplication." |
| "Quelle est la différence entre les droits NTFS et les droits de partage ?" | Sécurité des fichiers | "Les droits de partage s'appliquent uniquement aux accès réseau. Les droits NTFS s'appliquent toujours, même en accès local. La règle la plus restrictive des deux s'applique." |
| "Pourquoi avez-vous désactivé l'authentification par mot de passe SSH ?" | Sécurité SSH | "Pour bloquer les attaques par force brute. Sans AuthPassword, même avec un scanner automatique, aucune tentative ne peut aboutir sans la clé privée." |
| "Comment ajouteriez-vous un 7e service — Logistique — dans cette infrastructure ?" | Extensibilité | "Créer un nouveau VLAN 60, allouer un sous-réseau dans l'espace libre du /22, configurer la sous-interface sur le routeur, créer l'OU dans AD, configurer l'étendue DHCP, créer le partage et les droits." |

---

## VI. Vocabulaire Clé à Maîtriser pour l'Examen

| **Terme** | **Définition** |
|-----------|---------------|
| **HTTP** | HyperText Transfer Protocol — échange de pages web entre client et serveur (port 80) |
| **HTTPS** | HTTP + TLS — communications web chiffrées (port 443) |
| **TLS** | Transport Layer Security — protocole de chiffrement de la couche transport |
| **Certificat X.509** | Document numérique prouvant l'identité d'un serveur et contenant sa clé publique |
| **CA** | Certificate Authority — organisme de confiance signant les certificats TLS |
| **Code de statut HTTP** | Code 3 chiffres indiquant le résultat d'une requête HTTP (200=OK, 404=Not Found...) |
| **SMTP** | Simple Mail Transfer Protocol — envoi de courriers électroniques (ports 25/587) |
| **IMAP** | Internet Message Access Protocol — lecture synchronisée des emails (port 143/993) |
| **POP3** | Post Office Protocol — téléchargement des emails (port 110/995) |
| **MTA** | Mail Transfer Agent — serveur gérant l'acheminement des emails |
| **Résolution récursive** | Le résolveur DNS prend en charge toute la résolution jusqu'à la réponse finale |
| **Résolution itérative** | Chaque serveur DNS renvoie vers le serveur suivant dans la hiérarchie |
| **Enregistrement A** | Enregistrement DNS associant un nom à une adresse IPv4 |
| **Enregistrement MX** | Enregistrement DNS indiquant le serveur de messagerie d'un domaine |
| **Enregistrement PTR** | Enregistrement DNS de résolution inverse (IP  nom) |
| **TTL DNS** | Time To Live — durée de validité d'un enregistrement DNS en cache |
| **DNSSEC** | Extension DNS ajoutant des signatures cryptographiques pour prévenir l'empoisonnement |

---

## Auto-évaluation : Suis-je Prêt ?

- [ ] Je décris le cycle complet d'une requête HTTP (méthode, URL, en-têtes, corps)
- [ ] Je connais les 5 familles de codes de statut HTTP et 2 exemples par famille
- [ ] J'explique le TLS handshake en 4 étapes
- [ ] Je distingue SMTP, IMAP et POP3 par leur rôle et leur port
- [ ] Je dessine le parcours complet d'un email de l'expéditeur au destinataire
- [ ] Je différencie résolution DNS récursive et itérative avec un schéma
- [ ] Je connais les 5 types d'enregistrements DNS les plus courants (A, CNAME, MX, PTR, NS)
- [ ] Je replace tous les protocoles de cette fiche dans le tableau OSI avec leurs ports
- [ ] Je suis capable de présenter le Projet SimIO en 4 min avec le vocabulaire technique approprié
- [ ] Je réponds aux 5 questions de jury types sans hésitation

---
