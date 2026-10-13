# Aide-mémoire – DHCP Windows, NTFS et DFS

*BTS SIO SISR 1re année – Bloc 2 – S12*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## DHCP

```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
Add-DhcpServerv4Scope -Name "LAN" -StartRange 192.168.10.100 -EndRange 192.168.10.200 -SubnetMask 255.255.255.0
Set-DhcpServerv4OptionValue -Router 192.168.10.1 -DnsServer 192.168.10.2
```

## Droits de partage et droits NTFS

- Droits **effectifs** = le **plus restrictif** des deux.
- Bonne pratique : partage large (Utilisateurs authentifiés : modifier) et **droits NTFS précis par groupe**.
- Schéma de nommage : `GG_` (groupe global, comptes) membres de `DL_` (domaine local, droits sur un dossier).

**DFS :** un espace de noms (`\\techpro.local\partages`) qui masque l'emplacement réel des dossiers.