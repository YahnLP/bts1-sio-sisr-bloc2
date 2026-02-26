# 01 – Objectifs et ressources


---

## Informations Générales

| **Élément** | **Détails** |
|-------------|-------------|
| **Semaine** | S12 - Année 1 |
| **Bloc** | Bloc 2 - Infrastructure, Systèmes & Réseaux |
| **Durée** | 4 heures (1 séance) |
| **Phase** | Phase 3 - Services d'annuaire, GPO et sécurité |
| **Public** | Apprentis BTS SIO SISR (hétérogène) |
| **Prérequis** | S11 (Windows Server installé, domaine `techpro.local` créé, structure OU + utilisateurs en place) |

---

## Compétences RNCP Visées

| **Code** | **Libellé** | **Niveau** |
|----------|-------------|------------|
| **B2.1** | Administrer les services d'un système d'exploitation serveur — GPO, DNS Windows | **Application** |
| **B2.2** | Installer, tester et déployer — stratégies de sécurité, zones DNS AD-intégrées | Application |
| **B2.3** | Exploiter, dépanner, superviser — `gpresult`, `gpupdate`, `nslookup`, journal événements | Application |

---

## Objectifs Pédagogiques

**À l'issue de cette séance, l'apprenant sera capable de :**

1. **Expliquer** le rôle des GPO et leur lien avec Active Directory
2. **Décrire** l'ordre d'application des GPO (LSDOU) et le mécanisme d'héritage
3. **Créer** une GPO avec la console GPMC et l'éditeur de stratégie de groupe
4. **Configurer** une GPO de sécurité (politique de mots de passe, verrouillage de compte)
5. **Configurer** une GPO d'environnement (fond d'écran imposé, lecteur réseau mappé)
6. **Lier** une GPO à une OU et tester son application
7. **Utiliser** `gpupdate /force` et `gpresult /r` pour forcer et diagnostiquer les GPO
8. **Expliquer** la différence entre zones DNS standard et zones DNS AD-intégrées
9. **Créer** une zone DNS AD-intégrée et ajouter des enregistrements depuis Windows Server
