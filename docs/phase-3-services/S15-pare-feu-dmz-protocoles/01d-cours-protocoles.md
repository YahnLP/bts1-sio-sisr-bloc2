# Cours – Protocoles applicatifs : HTTP, messagerie, DNS

*BTS SIO SISR 1re année – Bloc 2 – S15*

## PARTIE I — HTTP / HTTPS : Le Web Expliqué

### I.A. HTTP — HyperText Transfer Protocol

**HTTP** est le protocole de la couche Application qui permet l'échange de documents hypertexte (pages web, API, médias) entre un **client** (navigateur) et un **serveur web** (Apache, Nginx...). C'est le protocole le plus utilisé sur Internet.

- **Port :** TCP 80
- **Version courante :** HTTP/1.1 (texte), HTTP/2 (binaire, multiplexé), HTTP/3 (sur QUIC/UDP)
- **Sans état (stateless) :** chaque requête est indépendante — le serveur ne "se souvient" pas du client

**Fonctionnement — Requête/Réponse :**

<figure class="graphe diagramme"><svg viewBox="0 0 640 302" style="max-width:640px" role="img" aria-label="Échange HTTP requête-réponse : connexion TCP, requête GET, réponse 200, fermeture" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="155.0" y="30.0" text-anchor="middle" class="dg-w">Client (navigateur)</text>
<line x1="155.0" y1="42" x2="155.0" y2="294" class="dg-dash"/>
<rect x="400.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="485.0" y="30.0" text-anchor="middle" class="dg-w">Serveur web (Apache)</text>
<line x1="485.0" y1="42" x2="485.0" y2="294" class="dg-dash"/>
<text x="320.0" y="64" text-anchor="middle" class="dg-t">1. Connexion TCP</text>
<line x1="155.0" y1="72" x2="476.0" y2="72.0" class="dg-line"/>
<polygon points="485.0,72 476.0,67.0 476.0,77.0" class="dg-ah"/>
<text x="320.0" y="88" text-anchor="middle" class="dg-i">SYN, SYN-ACK, ACK</text>
<text x="320.0" y="114" text-anchor="middle" class="dg-t">2. Requête HTTP</text>
<line x1="155.0" y1="122" x2="476.0" y2="122.0" class="dg-line"/>
<polygon points="485.0,122 476.0,117.0 476.0,127.0" class="dg-ah"/>
<text x="320.0" y="138" text-anchor="middle" class="dg-i">GET /index.php HTTP/1.1</text>
<text x="320.0" y="154" text-anchor="middle" class="dg-i">Host: www.siosarl.local</text>
<text x="320.0" y="180" text-anchor="middle" class="dg-t">3. Réponse HTTP</text>
<line x1="485.0" y1="188" x2="164.0" y2="188.0" class="dg-dash"/>
<polygon points="155.0,188 164.0,193.0 164.0,183.0" class="dg-ah"/>
<text x="320.0" y="204" text-anchor="middle" class="dg-i">HTTP/1.1 200 OK</text>
<text x="320.0" y="220" text-anchor="middle" class="dg-i">Content-Type: text/html</text>
<text x="320.0" y="236" text-anchor="middle" class="dg-i">corps : &lt;html&gt;...&lt;/html&gt;</text>
<text x="320.0" y="262" text-anchor="middle" class="dg-t">4. Fermeture TCP (FIN)</text>
<line x1="155.0" y1="270" x2="476.0" y2="270.0" class="dg-line"/>
<polygon points="485.0,270 476.0,265.0 476.0,275.0" class="dg-ah"/>
</svg>
</figure>

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

>  **Dans le quotidien SISR :** GET et POST couvrent 95% des échanges web. GET pour afficher une page, POST pour soumettre un formulaire (connexion, recherche). Les méthodes PUT/DELETE/PATCH sont typiques des **API REST** que vous rencontrerez en développement web.

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

>  **Astuce de diagnostic :** Quand Apache retourne un `403` sur votre site LAMP, regardez en premier les **droits Linux** (`ls -la /var/www/html`) — Apache exécute les requêtes en tant qu'utilisateur `www-data` qui doit pouvoir lire les fichiers.

---

### I.D. HTTPS — HTTP Sécurisé

**HTTPS** = HTTP + **TLS** (Transport Layer Security). TLS chiffre intégralement la communication HTTP, protégeant la confidentialité et l'intégrité des données.

- **Port :** TCP 443
- **Certificat :** Le serveur présente un certificat X.509 signé par une **CA** (Certificate Authority) — il prouve son identité au client
- **Indicateur visuel :** Le cadenas dans la barre d'adresse du navigateur

**Le TLS Handshake simplifié :**

<figure class="graphe diagramme"><svg viewBox="0 0 680 324" style="max-width:680px" role="img" aria-label="Poignée de main TLS : Client Hello, certificat, clé de session, tunnel chiffré" xmlns="http://www.w3.org/2000/svg">
<rect x="70.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="155.0" y="30.0" text-anchor="middle" class="dg-w">Client</text>
<line x1="155.0" y1="42" x2="155.0" y2="316" class="dg-dash"/>
<rect x="440.0" y="8" width="170" height="34" rx="8" class="dg-dark"/>
<text x="525.0" y="30.0" text-anchor="middle" class="dg-w">Serveur</text>
<line x1="525.0" y1="42" x2="525.0" y2="316" class="dg-dash"/>
<text x="340.0" y="64" text-anchor="middle" class="dg-t">1. Client Hello</text>
<line x1="155.0" y1="72" x2="516.0" y2="72.0" class="dg-line"/>
<polygon points="525.0,72 516.0,67.0 516.0,77.0" class="dg-ah"/>
<text x="340.0" y="88" text-anchor="middle" class="dg-i">versions TLS, algorithmes proposés, aléatoire</text>
<text x="340.0" y="114" text-anchor="middle" class="dg-t">2. Server Hello + certificat</text>
<line x1="525.0" y1="122" x2="164.0" y2="122.0" class="dg-dash"/>
<polygon points="155.0,122 164.0,127.0 164.0,117.0" class="dg-ah"/>
<text x="340.0" y="138" text-anchor="middle" class="dg-i">version choisie, certificat avec la clé publique</text>
<rect x="4.0" y="156" width="338.2" height="42" rx="6" class="dg-soft"/>
<text x="173.1" y="173" text-anchor="middle" class="dg-s">3. Vérification du certificat</text>
<text x="173.1" y="189" text-anchor="middle" class="dg-s">(chaîne de confiance jusqu&#x27;à l&#x27;autorité racine)</text>
<text x="340.0" y="218" text-anchor="middle" class="dg-t">4. Échange de la clé de session</text>
<line x1="155.0" y1="226" x2="516.0" y2="226.0" class="dg-line"/>
<polygon points="525.0,226 516.0,221.0 516.0,231.0" class="dg-ah"/>
<text x="340.0" y="242" text-anchor="middle" class="dg-i">chiffrée avec la clé publique du serveur</text>
<text x="340.0" y="268" text-anchor="middle" class="dg-t">5. Tunnel TLS établi</text>
<line x1="525.0" y1="276" x2="164.0" y2="276.0" class="dg-dash"/>
<polygon points="155.0,276 164.0,281.0 164.0,271.0" class="dg-ah"/>
<text x="340.0" y="292" text-anchor="middle" class="dg-i">les données HTTP sont désormais chiffrées</text>
</svg>
</figure>

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

<figure class="graphe diagramme"><svg viewBox="0 0 780 260" style="max-width:780px" role="img" aria-label="Acheminement d&#x27;un courriel : client, serveur d&#x27;envoi, serveur du destinataire, client" xmlns="http://www.w3.org/2000/svg">
<rect x="10" y="20" width="190" height="50" rx="8" class="dg-box"/>
<text x="105.0" y="41.0" text-anchor="middle" class="dg-t">Alice</text>
<text x="105.0" y="59.0" text-anchor="middle" class="dg-s">alice@siosarl.local</text>
<rect x="580" y="20" width="190" height="50" rx="8" class="dg-box"/>
<text x="675.0" y="41.0" text-anchor="middle" class="dg-t">Bob</text>
<text x="675.0" y="59.0" text-anchor="middle" class="dg-s">bob@nexio.fr</text>
<rect x="10" y="150" width="190" height="56" rx="8" class="dg-dark"/>
<text x="105.0" y="174.0" text-anchor="middle" class="dg-w">MTA SimIO</text>
<text x="105.0" y="192.0" text-anchor="middle" class="dg-s">mail.siosarl.local</text>
<rect x="580" y="150" width="190" height="56" rx="8" class="dg-dark"/>
<text x="675.0" y="174.0" text-anchor="middle" class="dg-w">MTA Nexio</text>
<text x="675.0" y="192.0" text-anchor="middle" class="dg-s">mail.nexio.fr</text>
<line x1="105" y1="70" x2="105.0" y2="141.0" class="dg-line"/>
<polygon points="105,150 110.0,141.0 100.0,141.0" class="dg-ah"/>
<text x="120" y="116" text-anchor="start" class="dg-s">1. envoi SMTP, port 587</text>
<line x1="200" y1="178" x2="571.0" y2="178.0" class="dg-line"/>
<polygon points="580,178 571.0,173.0 571.0,183.0" class="dg-ah"/>
<text x="390" y="168" text-anchor="middle" class="dg-s">2. transmission SMTP, port 25</text>
<line x1="675" y1="150" x2="675.0" y2="79.0" class="dg-line"/>
<polygon points="675,70 670.0,79.0 680.0,79.0" class="dg-ah"/>
<text x="660" y="116" text-anchor="end" class="dg-s">3. lecture : IMAP 143 (synchronisé)</text>
<text x="660" y="134" text-anchor="end" class="dg-s">ou POP3 110 (téléchargement)</text>
</svg>
</figure>

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

> **Sécurité SMTP :** SMTP sans authentification est exploitable pour le **spam relay** (utiliser votre serveur pour envoyer du spam). En production, toujours configurer `smtpd_recipient_restrictions` (Postfix) ou l'équivalent pour exiger une authentification.

---

### II.C. IMAP vs POP3 — Récupération des Emails

| **Critère** | **IMAP** (RFC 3501) | **POP3** (RFC 1939) |
|---|---|---|
| **Port** | 143 (STARTTLS) / 993 (SSL) | 110 (STARTTLS) / 995 (SSL) |
| **Stockage des emails** | Sur le **serveur** (synchronisation) | Téléchargés et **supprimés du serveur** |
| **Accès multi-appareils** | Oui — même boîte sur téléphone + PC + webmail | Non — les mails disparaissent du serveur après téléchargement |
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

>  **L'enregistrement SRV** est particulièrement important en SISR : c'est grâce aux enregistrements `_ldap._tcp.siosarl.local` et `_kerberos._tcp.siosarl.local` qu'un poste Windows trouve automatiquement son contrôleur de domaine lors de la jonction au domaine. Sans DNS correct, AD ne fonctionne pas.

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
