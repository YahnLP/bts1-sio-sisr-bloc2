# Annexe – Bilan de positionnement sur le bloc 2

*BTS SIO SISR 1re année – Bloc 2 – S19*

## Mode d'Emploi

Pour chaque compétence, évaluez-vous **honnêtement** selon 4 niveaux :

| **Niveau** | **Symbole** | **Ce que ça signifie** |
|---|---|---|
| Non acquis | ❌ | Je ne sais pas encore faire — j'aurais besoin d'un tutorat |
| En cours | ⚠ | Je comprends mais j'ai besoin d'aide pour réaliser |
| Acquis | ✅ | Je peux faire seul dans un contexte guidé (TP) |
| Maîtrisé |  | Je peux faire seul en contexte professionnel, expliquer et adapter |

**Règle d'or : être honnête est plus utile qu'être optimiste.** Un "en cours" identifié est un "acquis" en devenir. Un "maîtrisé" surestimé est une fausse sécurité.

---

## BLOC 2.1 — Conception d'Infrastructure

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve dans mon portfolio** |
|---|---|---|---|
| Analyser un cahier des charges | Je lis un CDC et identifie les contraintes techniques | | |
| Calculer un plan d'adressage VLSM | Je calcule masques et plages sans aide | | |
| Concevoir une segmentation VLAN | Je justifie le nombre et les noms de VLANs | | |
| Schématiser une infrastructure | Je produis un schéma exploitable par un tiers | | |
| Dimensionner les services (DHCP, DNS) | Je configure les étendues cohérentes avec l'adressage | | |

**Commentaire libre B2.1 :**
```
__________________________________________________________________
```

---

## BLOC 2.2 — Déploiement des Services

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve** |
|---|---|---|---|
| Configurer un switch (VLANs, trunk) | Je configure VLAN + trunk en CLI Cisco | | |
| Configurer un routage inter-VLAN | Je mets en place router-on-a-stick fonctionnel | | |
| Installer et configurer Apache | Je déploie un site web PHP fonctionnel | | |
| Installer et configurer MariaDB | Je crée BDD, utilisateur, droits | | |
| Configurer AD DS, DHCP, DNS Windows | Je déploie les 3 services en autonomie | | |
| Déployer un serveur FTP/SFTP Linux | vsftpd + chroot SFTP opérationnels | | |
| Configurer SSH par clés | Connexion sans mot de passe fonctionnelle | | |

**Commentaire libre B2.2 :**
```
__________________________________________________________________
```

---

## BLOC 2.3 — Sécurité des Accès

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve** |
|---|---|---|---|
| Structurer les OUs Active Directory | Hiérarchie logique, droits délégués | | |
| Créer et lier des GPO | 3 GPO fonctionnelles et justifiées | | |
| Appliquer les droits NTFS | Permissions cumulatives comprises et appliquées | | |
| Sécuriser SSH (clés, sshd_config) | Config sécurisée et testée | | |
| Mettre en place un chroot SFTP | Isolation fonctionnelle et testée | | |
| Comprendre les protocoles sécurisés | Je distingue HTTP/HTTPS, FTP/SFTP, LDAP/LDAPS | | |

**Commentaire libre B2.3 :**
```
__________________________________________________________________
```

---

## BLOC 2.4 — Scripting

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve** |
|---|---|---|---|
| Écrire un script Bash avec variables | Je déclare et utilise des variables | | |
| Utiliser les boucles Bash (for/while) | Je traite un fichier CSV en Bash | | |
| Écrire un script Bash de sauvegarde | Script avec tar, log, rotation autonome | | |
| Planifier avec cron | Expression cron lue et écrite sans aide | | |
| Utiliser le pipeline PowerShell | Chaîner 3+ cmdlets avec filtrage | | |
| Créer des utilisateurs AD via PowerShell | Script Import-Csv  New-ADUser fonctionnel | | |
| Gérer les erreurs (Try/Catch) | Script robuste qui ne plante pas sur un doublon | | |

**Commentaire libre B2.4 :**
```
__________________________________________________________________
```

---

## BLOC 2.5 — Continuité de Service

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve** |
|---|---|---|---|
| Mettre en place une sauvegarde automatique | Script cron opérationnel avec log | | |
| Tester la restauration | J'ai vérifié qu'une archive se restaure | | |
| Documenter un service (procédure) | Procédure exploitable par un collègue | | |
| Diagnostiquer un service défaillant | Je suis une méthode de diagnostic (log  config  test) | | |

**Commentaire libre B2.5 :**
```
__________________________________________________________________
```

---

## BLOC 3.2 — Sécurité Informatique

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve** |
|---|---|---|---|
| Expliquer les principes de sécurité réseau | Moindre privilège, séparation, chiffrement | | |
| Appliquer le principe du moindre privilège | Droits limités au strict nécessaire partout | | |
| Choisir entre FTP, FTPS, SFTP | Je justifie le choix selon le contexte | | |
| Configurer un pare-feu de base (ufw/iptables) | Règles entrantes/sortantes définies | | |

**Commentaire libre B3.2 :**
```
__________________________________________________________________
```

---

## BLOC 3.3 — Gestion de Projet et Communication

| **Compétence** | **Indicateur concret** | **Niveau** | **Preuve** |
|---|---|---|---|
| Rédiger un DAT professionnel | DAT exploitable par un tiers, versionné | | |
| Présenter une infrastructure à l'oral | Exposé structuré + questions sans panique | | |
| Utiliser le vocabulaire technique | Termes corrects, pas de paraphrases | | |
| Répondre à des questions imprévues | Je raisonne à voix haute même sur l'inconnu | | |
| Construire un plan de tests | 20+ tests documentés avec résultats | | |

**Commentaire libre B3.3 :**
```
__________________________________________________________________
```

---

## Synthèse Visuelle — Radar de Compétences

*Colorier chaque section proportionnellement à votre niveau (❌=0%, ⚠=40%, ✅=70%, =100%)*

```
                        B2.1 — Conception
                             ★
                            /|\
                           / | \
                          /  |  \
         B3.3 — Projet ──●   |   ●── B2.2 — Déploiement
                          \  |  /
                           \ | /
                            \|/
         B3.2 — Sécurité ────●────── B2.3 — Accès
                            /|\
                           / | \
                          /  |  \
         B2.5 — Continuité─●  |  ●── B2.4 — Scripting
                              |
                        [Centre = 0 / Pointe = 100%]
```

---

## Bilan Synthétique

**Mes 3 points forts du Bloc 2 (compétences les plus solides) :**

| **Compétence** | **Pourquoi c'est un point fort** |
|---|---|
| 1. | |
| 2. | |
| 3. | |

**Mes 3 priorités de progression pour l'Année 2 :**

| **Compétence à renforcer** | **Action concrète prévue** | **Échéance** |
|---|---|---|
| 1. | | S21-S25 |
| 2. | | S26-S30 |
| 3. | | S31-S35 |

**En entreprise, je vais chercher à pratiquer :**
*(Ce que vous allez activement demander à faire ou observer chez votre maître d'apprentissage)*

```
__________________________________________________________________
__________________________________________________________________
```

**Ce que j'aurais fait différemment dans le Projet 1 SimIO :**
*(Réflexivité professionnelle — réponse libre)*

```
__________________________________________________________________
__________________________________________________________________
```

---
