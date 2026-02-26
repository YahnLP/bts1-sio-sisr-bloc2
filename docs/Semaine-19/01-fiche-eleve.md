---
author: YLP
title: 📚 FICHE DE COURS
---

# 📚 FICHE DE COURS ÉLÈVE
## "Protocoles Applicatifs • Présentation Technique"

*Version 1.0 — BTS SIO SISR — Année 1 — Semaine 19*

---

## 🎯 Compétences Travaillées

| **Code** | **Compétence** |
|----------|---------------|
| **B2.2** | Comprendre et exploiter les protocoles réseau (couche Application) |
| **B1.2** | Recenser et identifier les ressources et les besoins |
| **B3.3** | Présenter une infrastructure technique à l'oral |

---

## PARTIE I — HTTP / HTTPS : Le Web Expliqué

### I.A. HTTP — HyperText Transfer Protocol

**HTTP** est le protocole de la couche Application qui permet l'échange de documents hypertexte (pages web, API, médias) entre un **client** (navigateur) et un **serveur web** (Apache, Nginx...). C'est le protocole le plus utilisé sur Internet.

- **Port :** TCP 80
- **Version courante :** HTTP/1.1 (texte), HTTP/2 (binaire, multiplexé), HTTP/3 (sur QUIC/UDP)
- **Sans état (stateless) :** chaque requête est indépendante — le serveur ne "se souvient" pas du client

**Fonctionnement — Requête/Réponse :**

```
   CLIENT (navigateur)                       SERVEUR WEB (Apache)
         │                                          │
         │── 1. Connexion TCP (SYN/SYN-ACK/ACK) ──►│
         │                                          │
         │── 2. Requête HTTP ──────────────────────►│
         │   GET /index.php HTTP/1.1                │
         │   Host: www.siosarl.local                │
         │   Accept: text/html                      │
         │   User-Agent: Mozilla/5.0                │
         │                                          │
         │◄─ 3. Réponse HTTP ─────────────────────── │
         │   HTTP/1.1 200 OK                        │
         │   Content-Type: text/html; charset=utf-8 │
         │   Content-Length: 4823                   │
         │                                          │
         │   <html>...</html>          (le corps)   │
         │                                          │
         │── 4. Fermeture TCP (FIN) ───────────────►│
```

*Légende : Échange HTTP complet en 4 étapes. La connexion TCP est établie en premier (SYN/SYN-ACK/ACK — le 3-way handshake). La requête HTTP indique la méthode (GET), la ressource (/index.php) et la version du protocole. Le serveur répond avec un code de statut (200 OK) et les en-têtes décrivant la réponse, suivis du corps (le contenu HTML). La connexion TCP est ensuite fermée.*

---

### I.B. Les Méthodes HTTP

| **Méthode** | **Action** | **Analogie** |
|---|---|---|
| `GET` | Demander une ressource (lecture) | Consulter un document |
| `POST` | Envoyer des données au serveur | Remplir et soumettre un formulaire |
| `PUT` | Remplacer une ressource existante | Écraser un fichier |
| `PATCH` | Modifier partiellement une ressource | Corriger une ligne dans un document |
| `DELETE` | Supprimer une ressource | Supprimer un fichier |
| `HEAD` | Obtenir les en-têtes sans le corps | Vérifier si un fichier existe sans le télécharger |
| `OPTIONS` | Connaître les méthodes supportées | Demander ce qu'on peut faire |

> 📌 **Dans le quotidien SISR :** GET et POST couvrent 95% des échanges web. GET pour afficher une page, POST pour soumettre un formulaire (connexion, recherche). Les méthodes PUT/DELETE/PATCH sont typiques des **API REST** que vous rencontrerez en développement web.

---

### I.C. Les Codes de Statut HTTP

Les codes de statut HTTP sont regroupés en 5 familles selon leur premier chiffre :

| **Famille** | **Signification** | **Exemples clés** |
|---|---|---|
| **1xx** | Information — traitement en cours | `100 Continue` |
| **2xx** | Succès | `200 OK`, `201 Created`, `204 No Content` |
| **3xx** | Redirection | `301 Moved Permanently`, `302 Found`, `304 Not Modified` |
| **4xx** | Erreur **client** | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found` |
| **5xx** | Erreur **serveur** | `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable` |

**Les 6 codes les plus rencontrés en administration :**

```
200 OK              → Tout va bien, ressource retournée
301 Moved Permanently → L'URL a changé définitivement (ex : HTTP → HTTPS)
400 Bad Request     → La requête du client est mal formée
401 Unauthorized    → Authentification requise (pas encore connecté)
403 Forbidden       → Authentifié mais pas les droits (ex : droits NTFS insuffisants)
404 Not Found       → La ressource n'existe pas sur ce serveur
500 Internal Server → Le serveur a planté (bug PHP, erreur Apache)
```

> 💡 **Astuce de diagnostic :** Quand Apache retourne un `403` sur votre site LAMP, regardez en premier les **droits Linux** (`ls -la /var/www/html`) — Apache exécute les requêtes en tant qu'utilisateur `www-data` qui doit pouvoir lire les fichiers.

---

### I.D. HTTPS — HTTP Sécurisé

**HTTPS** = HTTP + **TLS** (Transport Layer Security). TLS chiffre intégralement la communication HTTP, protégeant la confidentialité et l'intégrité des données.

- **Port :** TCP 443
- **Certificat :** Le serveur présente un certificat X.509 signé par une **CA** (Certificate Authority) — il prouve son identité au client
- **Indicateur visuel :** Le cadenas dans la barre d'adresse du navigateur

**Le TLS Handshake simplifié :**

```
   CLIENT                                   SERVEUR
      │                                        │
      │── 1. Client Hello ──────────────────►  │
      │   (versions TLS supportées,            │
      │    algorithmes proposés, aléatoire)    │
      │                                        │
      │  ◄─ 2. Server Hello + Certificat ──── │
      │   (version TLS choisie,                │
      │    certificat avec clé publique)       │
      │                                        │
      │── 3. Vérification du certificat       │
      │   (chaîne de confiance → CA racine)   │
      │                                        │
      │── 4. Échange de clé de session ──────►│
      │   (chiffré avec la clé publique        │
      │    du serveur)                         │
      │                                        │
      │◄──────────────────────── 5. Tunnel TLS établi ──
      │         Toutes les données HTTP        │
      │         sont maintenant chiffrées      │
```

*Légende : Le TLS handshake en 5 étapes. Le client annonce ses capacités. Le serveur répond avec son certificat contenant sa clé publique. Le client vérifie que le certificat est signé par une CA de confiance. Le client génère une clé de session et la chiffre avec la clé publique du serveur. À partir de ce moment, toute la communication HTTP est chiffrée de bout en bout — un intercepteur ne voit que des données chiffrées.*

**Différence entre HTTP et HTTPS pour l'administrateur :**

| **Aspect** | **HTTP** | **HTTPS** |
|---|---|---|
| Contenu visible sur le réseau | Oui (texte clair) | Non (chiffré) |
| Mots de passe des formulaires | Visibles en clair ! | Chiffrés |
| Certificat requis | Non | Oui (Let's Encrypt = gratuit) |
| Configuration Apache | `VirtualHost *:80` | `VirtualHost *:443` + `SSLCertificateFile` |
| SEO (référencement) | Pénalisé | Favorisé |

---

## PARTIE II — La Messagerie : SMTP, IMAP, POP3

### II.A. Architecture de la Messagerie Email

Pour comprendre SMTP, IMAP et POP3, il faut visualiser le **parcours complet d'un email** de l'expéditeur au destinataire :

```
   ALICE (alice@siosarl.local)            BOB (bob@nexio.fr)
   Client de messagerie                   Client de messagerie
   (Outlook, Thunderbird)                 (Outlook, Gmail)
          │                                       │
          │  1. Alice écrit et envoie             │
          │     SMTP port 587                     │
          ▼                                       │
   ┌──────────────┐                               │
   │  MTA SimIO   │  2. Transmission SMTP         │
   │ mail.siosarl │──── port 25 ────────────────► │
   │  .local      │                               ▼
   └──────────────┘                      ┌──────────────┐
                                         │  MTA Nexio   │
                                         │ mail.nexio.fr│
                                         └──────────────┘
                                                  │
                                    3. Bob récupère son mail
                                       IMAP port 143 (sync)
                                       POP3 port 110 (télécharg.)
```

*Légende : Le parcours d'un email de A à Z. SMTP est utilisé pour deux choses distinctes : le client envoie son email à son serveur de messagerie (MTA) via SMTP port 587 (soumission). Les serveurs MTA se transmettent l'email entre eux via SMTP port 25. Le destinataire récupère ses emails depuis son serveur avec IMAP (synchronisation, mail reste sur le serveur) ou POP3 (téléchargement, mail supprimé du serveur).*

---

### II.B. SMTP — Simple Mail Transfer Protocol

**SMTP** (RFC 5321) est le protocole d'**envoi** de courrier électronique. Il est utilisé dans deux contextes distincts :

| **Usage** | **Port** | **Chiffrement** | **Qui l'utilise** |
|---|---|---|---|
| Client → Serveur (soumission) | **587** (STARTTLS) ou **465** (SSL) | Oui (recommandé) | Outlook, Thunderbird, apps |
| Serveur → Serveur (relais) | **25** | Optionnel (STARTTLS) | Serveurs MTA entre eux |

**Dialogue SMTP (simplifié) :**

```
Client ──► EHLO siosarl.local
Serveur ◄── 250 mail.nexio.fr Hello
Client ──► MAIL FROM: <alice@siosarl.local>
Serveur ◄── 250 OK
Client ──► RCPT TO: <bob@nexio.fr>
Serveur ◄── 250 OK
Client ──► DATA
Serveur ◄── 354 Start mail input
Client ──► From: Alice <alice@siosarl.local>
            To: Bob <bob@nexio.fr>
            Subject: Réunion vendredi
            Date: [date]
            
            Bonjour Bob, ...
            .
Serveur ◄── 250 Message queued
Client ──► QUIT
Serveur ◄── 221 Bye
```

> ⚠️ **Sécurité SMTP :** SMTP sans authentification est exploitable pour le **spam relay** (utiliser votre serveur pour envoyer du spam). En production, toujours configurer `smtpd_recipient_restrictions` (Postfix) ou l'équivalent pour exiger une authentification.

---

### II.C. IMAP vs POP3 — Récupération des Emails

| **Critère** | **IMAP** (RFC 3501) | **POP3** (RFC 1939) |
|---|---|---|
| **Port** | 143 (STARTTLS) / 993 (SSL) | 110 (STARTTLS) / 995 (SSL) |
| **Stockage des emails** | Sur le **serveur** (synchronisation) | Téléchargés et **supprimés du serveur** |
| **Accès multi-appareils** | ✅ Oui — même boîte sur téléphone + PC + webmail | ❌ Non — les mails disparaissent du serveur après téléchargement |
| **Organisation dossiers** | Synchronisée entre client et serveur | Locale uniquement |
| **Fonctionnement hors-ligne** | Partiel (cache local) | Total (mails stockés localement) |
| **Usage recommandé** | Usage professionnel moderne | Archivage local, connexion intermittente |

**Pourquoi IMAP est-il devenu le standard ?**

```
   AVEC POP3 :                           AVEC IMAP :
   
   PC Bureau    →  télécharge mail       PC Bureau    ←──► [SERVEUR]
                   mail supprimé               ↑              ↑
   Smartphone   →  mail déjà parti !     Smartphone  ←──►  même
                                                             boîte
   Tablet       →  idem                  Tablet      ←──►  sync
   
   Résultat : les mails ne sont que      Résultat : cohérence parfaite
   sur le premier appareil qui a         entre tous les appareils
   relevé la boîte.
```

*Légende : Comparaison POP3 (à gauche) et IMAP (à droite). Avec POP3, le premier appareil qui télécharge vide la boîte du serveur — les autres appareils ne voient plus les messages. Avec IMAP, le serveur est la source de vérité et tous les appareils synchronisent leur état avec lui.*

---

### II.D. Récapitulatif Messagerie

```
PROTOCOLE   PORT     USAGE                   CHIFFREMENT
─────────────────────────────────────────────────────────
SMTP        25       Serveur ↔ Serveur       STARTTLS (optionnel)
SMTP        587      Client → Serveur        STARTTLS (obligatoire en prod)
SMTP        465      Client → Serveur        SSL/TLS (legacy SMTPS)
IMAP        143      Client récupère (sync)  STARTTLS
IMAPS       993      Client récupère (sync)  SSL/TLS (direct)
POP3        110      Client télécharge       STARTTLS
POP3S       995      Client télécharge       SSL/TLS (direct)
```

---

## PARTIE III — DNS en Profondeur

### III.A. Rappel : La Résolution DNS

Le DNS (Domain Name System) transforme un **nom de domaine** lisible (`www.siosarl.local`) en une **adresse IP** utilisable (`192.168.0.145`). Aucune communication IP ne peut avoir lieu sans cette résolution préalable.

- **Port :** UDP 53 (requêtes standard) / TCP 53 (réponses volumineuses, transferts de zone)
- **Cache TTL :** Chaque enregistrement DNS a une durée de vie (Time To Live) après laquelle il doit être rafraîchi

---

### III.B. Types d'Enregistrements DNS

| **Type** | **Signification** | **Exemple** | **Usage** |
|---|---|---|---|
| **A** | Address — IPv4 | `srv-ad01.siosarl.local → 192.168.0.145` | Résolution nom → IP |
| **AAAA** | Address IPv6 | `srv-ad01.siosarl.local → FE80::1` | Résolution nom → IPv6 |
| **CNAME** | Canonical Name — alias | `www → srv-ad01` | Alias d'un enregistrement A |
| **MX** | Mail Exchanger | `siosarl.local → mail.siosarl.local (10)` | Serveur de messagerie du domaine |
| **PTR** | Pointer — résolution inverse | `192.168.0.145 → srv-ad01.siosarl.local` | IP → nom (zone inverse) |
| **NS** | Name Server | `siosarl.local → ns1.siosarl.local` | Serveur DNS faisant autorité |
| **SOA** | Start Of Authority | — | Informations sur la zone (admin, TTL...) |
| **TXT** | Texte libre | `v=spf1 mx ~all` | SPF, DKIM, validation de domaine |
| **SRV** | Service | `_ldap._tcp.siosarl.local` | Localisation des services AD (Kerberos, LDAP) |

> 📌 **L'enregistrement SRV** est particulièrement important en SISR : c'est grâce aux enregistrements `_ldap._tcp.siosarl.local` et `_kerberos._tcp.siosarl.local` qu'un poste Windows trouve automatiquement son contrôleur de domaine lors de la jonction au domaine. Sans DNS correct, AD ne fonctionne pas.

---

### III.C. Résolution Récursive vs Itérative

Il existe deux modes de résolution DNS, selon l'acteur qui porte la charge de la recherche :

```
   ─── RÉSOLUTION RÉCURSIVE (client → résolveur) ────────────────────────────
   Le client demande "Donne-moi l'IP de www.google.com".
   Le résolveur prend TOUT en charge et retourne la réponse finale.

   CLIENT                    RÉSOLVEUR (ISP/Entreprise)
      │                              │
      │── "www.google.com ?" ───────►│
      │                              │  (fait tout le travail)
      │                              │  ← → Serveurs racine
      │                              │  ← → Serveurs .com
      │                              │  ← → DNS Google
      │◄── "216.58.213.100" ─────────│


   ─── RÉSOLUTION ITÉRATIVE (résolveur → serveurs d'autorité) ──────────────
   Le résolveur demande à chaque serveur "Qui sait?" jusqu'à la réponse.

   RÉSOLVEUR         RACINE           .COM NS         GOOGLE NS
      │                 │                │                │
      │── "google.com?"──►│                │                │
      │◄── "Demandez .com"─│               │                │
      │─────── "google.com?" ────────────►│                │
      │◄─────── "Demandez Google" ─────────│               │
      │────────────────── "www.google.com?" ───────────────►│
      │◄────────────────── "216.58.213.100" ────────────────│
```

*Légende : Les deux modes de résolution DNS. Dans la résolution récursive (en haut), le client délègue entièrement la recherche au résolveur — c'est ce que fait votre navigateur quand vous tapez une URL. Dans la résolution itérative (en bas), le résolveur interroge successivement les serveurs de la hiérarchie DNS (racine → TLD → autorité) en récupérant à chaque fois une référence vers le niveau suivant, jusqu'à obtenir la réponse finale. Dans la pratique, le client utilise la récursion avec son résolveur, et ce résolveur utilise l'itération avec les serveurs d'autorité.*

---

### III.D. Hiérarchie DNS

```
                        . (Racine — 13 serveurs racine)
                        │
            ┌──────────┼──────────┐
           .com       .fr        .local
            │          │           │
        google.com  free.fr   siosarl.local
            │                      │
       www.google.com          srv-ad01.siosarl.local
                                srv-ftp.siosarl.local
```

*Légende : Hiérarchie du système DNS. La racine (`.`) est au sommet, gérée par 13 clusters de serveurs racine répartis mondialement (en réalité plusieurs centaines de serveurs via anycast). Les TLD (Top Level Domains) comme `.com`, `.fr` ou `.local` sont en dessous. Chaque domaine délègue ensuite ses sous-domaines à ses propres serveurs DNS.*

---

### III.E. Les Attaques DNS (Culture Sécurité)

| **Attaque** | **Mécanisme** | **Protection** |
|---|---|---|
| **DNS Poisoning** (empoisonnement) | Injecter de faux enregistrements dans le cache d'un résolveur | DNSSEC, requêtes randomisées |
| **DNS Hijacking** (détournement) | Modifier les serveurs DNS d'une victime (malware, routeur compromis) | HTTPS, DNSSEC, surveillance |
| **DNS Amplification** (DDoS) | Utiliser des serveurs DNS ouverts pour amplifier une attaque DDoS | Fermer les résolveurs ouverts |
| **DNS Tunneling** | Exfiltrer des données dans des requêtes DNS (contournement de pare-feu) | Analyse comportementale DNS |

---

## PARTIE IV — Récapitulatif : Les Protocoles Applicatifs dans le Modèle OSI

```
COUCHE 7 — APPLICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  PROTOCOLE   PORT(S)   TRANSPORT   RÔLE
  ─────────────────────────────────────────────────────────────
  HTTP        80        TCP         Pages web (texte clair)
  HTTPS       443       TCP         Pages web (chiffré TLS)
  FTP         21/20     TCP         Transfert fichiers (clair)
  SFTP        22        TCP         Transfert fichiers (SSH)
  FTPS        990       TCP         Transfert fichiers (TLS)
  SMTP        25/587    TCP         Envoi mail
  IMAP        143/993   TCP         Lecture mail (sync serveur)
  POP3        110/995   TCP         Lecture mail (télécharg.)
  DNS         53        UDP+TCP     Résolution de noms
  DHCP        67/68     UDP         Attribution IP automatique
  SSH         22        TCP         Shell distant sécurisé
  Telnet      23        TCP         Shell distant (clair — obsolète)
  LDAP        389       TCP         Annuaire (Active Directory)
  LDAPS       636       TCP         Annuaire chiffré (AD)
  RDP         3389      TCP         Bureau distant Windows
  SMB/CIFS    445       TCP         Partages fichiers Windows
  NTP         123       UDP         Synchronisation horaire
  SNMP        161/162   UDP         Supervision réseau

COUCHE 4 — TRANSPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  TCP : connexion fiable, acquittements, retransmissions
  UDP : sans connexion, rapide, pas de garantie

COUCHE 3 — RÉSEAU
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  IP (IPv4 / IPv6) — ICMP — ARP
```

---

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
| "Pourquoi un /27 pour le VLAN RH plutôt qu'un /26 ?" | Maîtrise du VLSM | "25 hôtes → /27 donne 30 hôtes utilisables, avec 5 de marge. Un /26 aurait gaspillé 37 adresses inutilement." |
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
| **Enregistrement PTR** | Enregistrement DNS de résolution inverse (IP → nom) |
| **TTL DNS** | Time To Live — durée de validité d'un enregistrement DNS en cache |
| **DNSSEC** | Extension DNS ajoutant des signatures cryptographiques pour prévenir l'empoisonnement |

---

## ✅ Auto-évaluation : Suis-je Prêt ?

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
