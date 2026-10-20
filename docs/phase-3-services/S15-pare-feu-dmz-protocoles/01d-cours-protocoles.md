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

<figure class="graphe diagramme"><svg viewBox="0 0 760 494" style="max-width:760px" role="img" aria-label="Dialogue SMTP entre un client et un serveur de messagerie" xmlns="http://www.w3.org/2000/svg">
<rect x="40.0" y="8" width="220" height="34" rx="8" class="dg-dark"/>
<text x="150.0" y="30.0" text-anchor="middle" class="dg-w">Client (siosarl.local)</text>
<line x1="150.0" y1="42" x2="150.0" y2="486" class="dg-dash"/>
<rect x="500.0" y="8" width="220" height="34" rx="8" class="dg-dark"/>
<text x="610.0" y="30.0" text-anchor="middle" class="dg-w">Serveur SMTP (nexio.fr)</text>
<line x1="610.0" y1="42" x2="610.0" y2="486" class="dg-dash"/>
<text x="380.0" y="64" text-anchor="middle" class="dg-t">EHLO siosarl.local</text>
<line x1="150.0" y1="72" x2="601.0" y2="72.0" class="dg-line"/>
<polygon points="610.0,72 601.0,67.0 601.0,77.0" class="dg-ah"/>
<text x="380.0" y="98" text-anchor="middle" class="dg-t">250 mail.nexio.fr Hello</text>
<line x1="610.0" y1="106" x2="159.0" y2="106.0" class="dg-dash"/>
<polygon points="150.0,106 159.0,111.0 159.0,101.0" class="dg-ah"/>
<text x="380.0" y="132" text-anchor="middle" class="dg-t">MAIL FROM: &lt;alice@siosarl.local&gt;</text>
<line x1="150.0" y1="140" x2="601.0" y2="140.0" class="dg-line"/>
<polygon points="610.0,140 601.0,135.0 601.0,145.0" class="dg-ah"/>
<text x="380.0" y="166" text-anchor="middle" class="dg-t">250 OK</text>
<line x1="610.0" y1="174" x2="159.0" y2="174.0" class="dg-dash"/>
<polygon points="150.0,174 159.0,179.0 159.0,169.0" class="dg-ah"/>
<text x="380.0" y="200" text-anchor="middle" class="dg-t">RCPT TO: &lt;bob@nexio.fr&gt;</text>
<line x1="150.0" y1="208" x2="601.0" y2="208.0" class="dg-line"/>
<polygon points="610.0,208 601.0,203.0 601.0,213.0" class="dg-ah"/>
<text x="380.0" y="234" text-anchor="middle" class="dg-t">250 OK</text>
<line x1="610.0" y1="242" x2="159.0" y2="242.0" class="dg-dash"/>
<polygon points="150.0,242 159.0,247.0 159.0,237.0" class="dg-ah"/>
<text x="380.0" y="268" text-anchor="middle" class="dg-t">DATA</text>
<line x1="150.0" y1="276" x2="601.0" y2="276.0" class="dg-line"/>
<polygon points="610.0,276 601.0,271.0 601.0,281.0" class="dg-ah"/>
<text x="380.0" y="302" text-anchor="middle" class="dg-t">354 Start mail input</text>
<line x1="610.0" y1="310" x2="159.0" y2="310.0" class="dg-dash"/>
<polygon points="150.0,310 159.0,315.0 159.0,305.0" class="dg-ah"/>
<text x="380.0" y="336" text-anchor="middle" class="dg-t">En-têtes et corps du message, terminé par une ligne contenant un point</text>
<line x1="150.0" y1="344" x2="601.0" y2="344.0" class="dg-line"/>
<polygon points="610.0,344 601.0,339.0 601.0,349.0" class="dg-ah"/>
<text x="380.0" y="360" text-anchor="middle" class="dg-i">From: Alice  |  To: Bob  |  Subject: Réunion vendredi  |  « Bonjour Bob, ... »</text>
<text x="380.0" y="386" text-anchor="middle" class="dg-t">250 Message queued</text>
<line x1="610.0" y1="394" x2="159.0" y2="394.0" class="dg-dash"/>
<polygon points="150.0,394 159.0,399.0 159.0,389.0" class="dg-ah"/>
<text x="380.0" y="420" text-anchor="middle" class="dg-t">QUIT</text>
<line x1="150.0" y1="428" x2="601.0" y2="428.0" class="dg-line"/>
<polygon points="610.0,428 601.0,423.0 601.0,433.0" class="dg-ah"/>
<text x="380.0" y="454" text-anchor="middle" class="dg-t">221 Bye</text>
<line x1="610.0" y1="462" x2="159.0" y2="462.0" class="dg-dash"/>
<polygon points="150.0,462 159.0,467.0 159.0,457.0" class="dg-ah"/>
</svg>
<figcaption>Les commandes du client sont en trait plein, les réponses du serveur en pointillé.</figcaption>
</figure>

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

<figure class="graphe diagramme"><svg viewBox="0 0 760 300" style="max-width:760px" role="img" aria-label="POP3 et IMAP : comportement avec plusieurs appareils" xmlns="http://www.w3.org/2000/svg">
<text x="190" y="24" text-anchor="middle" class="dg-t">POP3</text>
<text x="570" y="24" text-anchor="middle" class="dg-t">IMAP</text>
<line x1="380" y1="10" x2="380" y2="290" class="dg-dash"/>
<rect x="20" y="40" width="120" height="34" rx="6" class="dg-dark"/>
<text x="80.0" y="62.0" text-anchor="middle" class="dg-w">Serveur</text>
<rect x="20" y="100" width="110" height="34" rx="6" class="dg-ok"/>
<text x="75.0" y="122.0" text-anchor="middle" class="dg-t">PC Bureau</text>
<rect x="20" y="150" width="110" height="34" rx="6" class="dg-bad"/>
<text x="75.0" y="172.0" text-anchor="middle" class="dg-t">Smartphone</text>
<rect x="20" y="200" width="110" height="34" rx="6" class="dg-bad"/>
<text x="75.0" y="222.0" text-anchor="middle" class="dg-t">Tablette</text>
<line x1="132" y1="117" x2="221.0" y2="117.0" class="dg-line"/>
<polygon points="230,117 221.0,112.0 221.0,122.0" class="dg-ah"/>
<text x="136" y="110" text-anchor="start" class="dg-s">télécharge puis supprime</text>
<text x="150" y="167" text-anchor="start" class="dg-s">rien à récupérer</text>
<text x="150" y="217" text-anchor="start" class="dg-s">rien à récupérer</text>
<text x="20" y="270" text-anchor="start" class="dg-i">Les mails ne sont que sur le premier appareil.</text>
<rect x="400" y="40" width="120" height="34" rx="6" class="dg-dark"/>
<text x="460.0" y="62.0" text-anchor="middle" class="dg-w">Serveur</text>
<rect x="400" y="100" width="110" height="34" rx="6" class="dg-ok"/>
<text x="455.0" y="122.0" text-anchor="middle" class="dg-t">PC Bureau</text>
<line x1="512" y1="117" x2="591.0" y2="117.0" class="dg-line"/>
<polygon points="600,117 591.0,112.0 591.0,122.0" class="dg-ah"/>
<line x1="600" y1="117" x2="521.0" y2="117.0" class="dg-line"/>
<polygon points="512,117 521.0,122.0 521.0,112.0" class="dg-ah"/>
<text x="620" y="121" text-anchor="start" class="dg-s">synchronisé</text>
<rect x="400" y="150" width="110" height="34" rx="6" class="dg-ok"/>
<text x="455.0" y="172.0" text-anchor="middle" class="dg-t">Smartphone</text>
<line x1="512" y1="167" x2="591.0" y2="167.0" class="dg-line"/>
<polygon points="600,167 591.0,162.0 591.0,172.0" class="dg-ah"/>
<line x1="600" y1="167" x2="521.0" y2="167.0" class="dg-line"/>
<polygon points="512,167 521.0,172.0 521.0,162.0" class="dg-ah"/>
<text x="620" y="171" text-anchor="start" class="dg-s">synchronisé</text>
<rect x="400" y="200" width="110" height="34" rx="6" class="dg-ok"/>
<text x="455.0" y="222.0" text-anchor="middle" class="dg-t">Tablette</text>
<line x1="512" y1="217" x2="591.0" y2="217.0" class="dg-line"/>
<polygon points="600,217 591.0,212.0 591.0,222.0" class="dg-ah"/>
<line x1="600" y1="217" x2="521.0" y2="217.0" class="dg-line"/>
<polygon points="512,217 521.0,222.0 521.0,212.0" class="dg-ah"/>
<text x="620" y="221" text-anchor="start" class="dg-s">synchronisé</text>
<text x="400" y="270" text-anchor="start" class="dg-i">Le serveur est la source de vérité.</text>
</svg>
<figcaption>Avec POP3 le premier appareil vide la boîte ; avec IMAP tous les appareils restent synchronisés.</figcaption>
</figure>

*Légende : Comparaison POP3 (à gauche) et IMAP (à droite). Avec POP3, le premier appareil qui télécharge vide la boîte du serveur — les autres appareils ne voient plus les messages. Avec IMAP, le serveur est la source de vérité et tous les appareils synchronisent leur état avec lui.*

---

### II.D. Récapitulatif Messagerie

| **Protocole** | **Port** | **Usage** | **Chiffrement** |
|---|---|---|---|
| SMTP | 25 | Serveur vers serveur | STARTTLS (optionnel) |
| SMTP | 587 | Client vers serveur | STARTTLS (obligatoire en production) |
| SMTP | 465 | Client vers serveur | SSL/TLS (SMTPS, ancien) |
| IMAP | 143 | Le client récupère (synchronisation) | STARTTLS |
| IMAPS | 993 | Le client récupère (synchronisation) | SSL/TLS direct |
| POP3 | 110 | Le client télécharge | STARTTLS |
| POP3S | 995 | Le client télécharge | SSL/TLS direct |

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

<figure class="graphe diagramme"><svg viewBox="0 0 700 192" style="max-width:700px" role="img" aria-label="Résolution DNS récursive : le client délègue tout au résolveur" xmlns="http://www.w3.org/2000/svg">
<rect x="40.0" y="8" width="200" height="34" rx="8" class="dg-dark"/>
<text x="140.0" y="30.0" text-anchor="middle" class="dg-w">Client</text>
<line x1="140.0" y1="42" x2="140.0" y2="184" class="dg-dash"/>
<rect x="460.0" y="8" width="200" height="34" rx="8" class="dg-dark"/>
<text x="560.0" y="30.0" text-anchor="middle" class="dg-w">Résolveur (FAI / entreprise)</text>
<line x1="560.0" y1="42" x2="560.0" y2="184" class="dg-dash"/>
<text x="350.0" y="64" text-anchor="middle" class="dg-t">www.google.com ?</text>
<line x1="140.0" y1="72" x2="551.0" y2="72.0" class="dg-line"/>
<polygon points="560.0,72 551.0,67.0 551.0,77.0" class="dg-ah"/>
<rect x="437.1" y="90" width="245.8" height="42" rx="6" class="dg-soft"/>
<text x="560.0" y="107" text-anchor="middle" class="dg-s">Le résolveur fait tout le travail</text>
<text x="560.0" y="123" text-anchor="middle" class="dg-s">Serveurs racine, .com, DNS Google</text>
<text x="350.0" y="152" text-anchor="middle" class="dg-t">216.58.213.100</text>
<line x1="560.0" y1="160" x2="149.0" y2="160.0" class="dg-dash"/>
<polygon points="140.0,160 149.0,165.0 149.0,155.0" class="dg-ah"/>
</svg>
<figcaption>Le client pose une seule question et reçoit la réponse finale.</figcaption>
</figure>

<figure class="graphe diagramme"><svg viewBox="0 0 760 274" style="max-width:760px" role="img" aria-label="Résolution DNS itérative : le résolveur interroge chaque serveur" xmlns="http://www.w3.org/2000/svg">
<rect x="20.0" y="8" width="150" height="34" rx="8" class="dg-dark"/>
<text x="95.0" y="30.0" text-anchor="middle" class="dg-w">Résolveur</text>
<line x1="95.0" y1="42" x2="95.0" y2="266" class="dg-dash"/>
<rect x="210.0" y="8" width="150" height="34" rx="8" class="dg-dark"/>
<text x="285.0" y="30.0" text-anchor="middle" class="dg-w">Serveur racine</text>
<line x1="285.0" y1="42" x2="285.0" y2="266" class="dg-dash"/>
<rect x="400.0" y="8" width="150" height="34" rx="8" class="dg-dark"/>
<text x="475.0" y="30.0" text-anchor="middle" class="dg-w">Serveur .com</text>
<line x1="475.0" y1="42" x2="475.0" y2="266" class="dg-dash"/>
<rect x="590.0" y="8" width="150" height="34" rx="8" class="dg-dark"/>
<text x="665.0" y="30.0" text-anchor="middle" class="dg-w">Serveur de Google</text>
<line x1="665.0" y1="42" x2="665.0" y2="266" class="dg-dash"/>
<text x="190.0" y="64" text-anchor="middle" class="dg-t">google.com ?</text>
<line x1="95.0" y1="72" x2="276.0" y2="72.0" class="dg-line"/>
<polygon points="285.0,72 276.0,67.0 276.0,77.0" class="dg-ah"/>
<text x="190.0" y="98" text-anchor="middle" class="dg-t">Demandez au serveur .com</text>
<line x1="285.0" y1="106" x2="104.0" y2="106.0" class="dg-dash"/>
<polygon points="95.0,106 104.0,111.0 104.0,101.0" class="dg-ah"/>
<text x="285.0" y="132" text-anchor="middle" class="dg-t">google.com ?</text>
<line x1="95.0" y1="140" x2="466.0" y2="140.0" class="dg-line"/>
<polygon points="475.0,140 466.0,135.0 466.0,145.0" class="dg-ah"/>
<text x="285.0" y="166" text-anchor="middle" class="dg-t">Demandez au serveur de Google</text>
<line x1="475.0" y1="174" x2="104.0" y2="174.0" class="dg-dash"/>
<polygon points="95.0,174 104.0,179.0 104.0,169.0" class="dg-ah"/>
<text x="380.0" y="200" text-anchor="middle" class="dg-t">www.google.com ?</text>
<line x1="95.0" y1="208" x2="656.0" y2="208.0" class="dg-line"/>
<polygon points="665.0,208 656.0,203.0 656.0,213.0" class="dg-ah"/>
<text x="380.0" y="234" text-anchor="middle" class="dg-t">216.58.213.100</text>
<line x1="665.0" y1="242" x2="104.0" y2="242.0" class="dg-dash"/>
<polygon points="95.0,242 104.0,247.0 104.0,237.0" class="dg-ah"/>
</svg>
<figcaption>Chaque serveur renvoie une référence vers le niveau suivant jusqu&#x27;à la réponse finale.</figcaption>
</figure>

*Légende : Les deux modes de résolution DNS. Dans la résolution récursive (en haut), le client délègue entièrement la recherche au résolveur — c'est ce que fait votre navigateur quand vous tapez une URL. Dans la résolution itérative (en bas), le résolveur interroge successivement les serveurs de la hiérarchie DNS (racine → TLD → autorité) en récupérant à chaque fois une référence vers le niveau suivant, jusqu'à obtenir la réponse finale. Dans la pratique, le client utilise la récursion avec son résolveur, et ce résolveur utilise l'itération avec les serveurs d'autorité.*

---

### III.D. Hiérarchie DNS

<figure class="graphe diagramme"><svg viewBox="0 0 394 348" style="max-width:394px" role="img" aria-label="Hiérarchie du système DNS : racine, TLD, domaines" xmlns="http://www.w3.org/2000/svg">
<line x1="26" y1="34" x2="26" y2="55" class="dg-line"/>
<line x1="26" y1="55" x2="50" y2="55" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="157" class="dg-line"/>
<line x1="26" y1="157" x2="50" y2="157" class="dg-line"/>
<line x1="26" y1="34" x2="26" y2="225" class="dg-line"/>
<line x1="26" y1="225" x2="50" y2="225" class="dg-line"/>
<line x1="64" y1="68" x2="64" y2="89" class="dg-line"/>
<line x1="64" y1="89" x2="88" y2="89" class="dg-line"/>
<line x1="102" y1="102" x2="102" y2="123" class="dg-line"/>
<line x1="102" y1="123" x2="126" y2="123" class="dg-line"/>
<line x1="64" y1="170" x2="64" y2="191" class="dg-line"/>
<line x1="64" y1="191" x2="88" y2="191" class="dg-line"/>
<line x1="64" y1="238" x2="64" y2="259" class="dg-line"/>
<line x1="64" y1="259" x2="88" y2="259" class="dg-line"/>
<line x1="102" y1="272" x2="102" y2="293" class="dg-line"/>
<line x1="102" y1="293" x2="126" y2="293" class="dg-line"/>
<line x1="102" y1="272" x2="102" y2="327" class="dg-line"/>
<line x1="102" y1="327" x2="126" y2="327" class="dg-line"/>
<rect x="12" y="8" width="370" height="26" rx="6" class="dg-dark"/>
<text x="197.0" y="26" text-anchor="middle" class="dg-w">. (racine : 13 clusters de serveurs racine)</text>
<rect x="50" y="42" width="58" height="26" rx="6" class="dg-box"/>
<text x="79.0" y="60" text-anchor="middle" class="dg-t">.com</text>
<rect x="88" y="76" width="106" height="26" rx="6" class="dg-ok"/>
<text x="141.0" y="94" text-anchor="middle" class="dg-t">google.com</text>
<rect x="126" y="110" width="118" height="26" rx="6" class="dg-soft"/>
<text x="185.2" y="128" text-anchor="middle" class="dg-s">www.google.com</text>
<rect x="50" y="144" width="50" height="26" rx="6" class="dg-box"/>
<text x="75.0" y="162" text-anchor="middle" class="dg-t">.fr</text>
<rect x="88" y="178" width="82" height="26" rx="6" class="dg-ok"/>
<text x="129.0" y="196" text-anchor="middle" class="dg-t">free.fr</text>
<rect x="50" y="212" width="74" height="26" rx="6" class="dg-box"/>
<text x="87.0" y="230" text-anchor="middle" class="dg-t">.local</text>
<rect x="88" y="246" width="130" height="26" rx="6" class="dg-ok"/>
<text x="153.0" y="264" text-anchor="middle" class="dg-t">siosarl.local</text>
<rect x="126" y="280" width="171" height="26" rx="6" class="dg-soft"/>
<text x="211.6" y="298" text-anchor="middle" class="dg-s">srv-ad01.siosarl.local</text>
<rect x="126" y="314" width="165" height="26" rx="6" class="dg-soft"/>
<text x="208.3" y="332" text-anchor="middle" class="dg-s">srv-ftp.siosarl.local</text>
</svg>
<figcaption>La racine est au sommet ; chaque domaine délègue ses sous-domaines à ses propres serveurs.</figcaption>
</figure>

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

**Couche 7 - Application**

| **Protocole** | **Port(s)** | **Transport** | **Rôle** |
|---|---|---|---|
| HTTP | 80 | TCP | Pages web (texte clair) |
| HTTPS | 443 | TCP | Pages web (chiffré TLS) |
| FTP | 21/20 | TCP | Transfert de fichiers (clair) |
| SFTP | 22 | TCP | Transfert de fichiers (SSH) |
| FTPS | 990 | TCP | Transfert de fichiers (TLS) |
| SMTP | 25/587 | TCP | Envoi de mail |
| IMAP | 143/993 | TCP | Lecture de mail (synchronisée avec le serveur) |
| POP3 | 110/995 | TCP | Lecture de mail (téléchargement) |
| DNS | 53 | UDP + TCP | Résolution de noms |
| DHCP | 67/68 | UDP | Attribution IP automatique |
| SSH | 22 | TCP | Shell distant sécurisé |
| Telnet | 23 | TCP | Shell distant en clair (obsolète) |
| LDAP | 389 | TCP | Annuaire (Active Directory) |
| LDAPS | 636 | TCP | Annuaire chiffré (AD) |
| RDP | 3389 | TCP | Bureau distant Windows |
| SMB/CIFS | 445 | TCP | Partages de fichiers Windows |
| NTP | 123 | UDP | Synchronisation horaire |
| SNMP | 161/162 | UDP | Supervision réseau |

**Couche 4 - Transport** : TCP (connexion fiable, acquittements, retransmissions) ; UDP (sans connexion, rapide, sans garantie).

**Couche 3 - Réseau** : IP (IPv4 / IPv6), ICMP, ARP.

---
