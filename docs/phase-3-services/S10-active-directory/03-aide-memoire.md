# Aide-mémoire – Active Directory

*BTS SIO SISR 1re année – Bloc 2 – S10*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

## Installer le domaine (PowerShell, rôle AD DS)

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName "techpro.local"
```

| Objet | Rôle |
|---|---|
| Domaine | Frontière d'administration et de sécurité |
| OU | Dossier de l'annuaire pour ranger et déléguer |
| Groupe de sécurité | Ensemble de comptes auquel on donne des droits |
| Contrôleur de domaine | Serveur qui héberge l'annuaire |

`Get-ADDomain` · `Get-ADUser -Filter *` · `New-ADOrganizationalUnit` · poste : `sysdm.cpl` pour joindre le domaine.

**Le DNS du poste doit pointer vers le contrôleur de domaine.**