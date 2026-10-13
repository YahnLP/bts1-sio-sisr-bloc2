# Aide-mémoire – VLAN et trunk

*BTS SIO SISR 1re année – Bloc 2 – S06*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

```text
vlan 10
 name INFO
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface g0/1
 switchport mode trunk
```

| Vérification | Commande |
|---|---|
| VLAN et ports | `show vlan brief` |
| Trunks | `show interfaces trunk` |
| Mode d'un port | `show interfaces fa0/1 switchport` |

- **Access** : un seul VLAN, trames sans étiquette. **Trunk** : plusieurs VLAN, étiquette 802.1Q.
- Un VLAN = un domaine de diffusion = un sous-réseau IP.

## Linux : utilisateurs et droits

| Action | Commande |
|---|---|
| Créer un utilisateur | `sudo useradd -m -s /bin/bash alice` |
| Mot de passe | `sudo passwd alice` |
| Groupe | `sudo groupadd compta` · `sudo usermod -aG compta alice` |
| Propriétaire | `sudo chown alice:compta fichier` |
| Droits | `chmod 770 dossier` (rwx pour propriétaire et groupe) |
