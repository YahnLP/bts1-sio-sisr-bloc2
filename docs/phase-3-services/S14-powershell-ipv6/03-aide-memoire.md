# Aide-mémoire – PowerShell et IPv6

*BTS SIO SISR 1re année – Bloc 2 – S14*

> Un mot ou un sigle inconnu ? Passe la souris dessus, ou ouvre le [glossaire](../../ressources/glossaire.md).

```powershell
Get-Command *user*
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
Import-Csv employes.csv | ForEach-Object { New-ADUser -Name $_.Nom -SamAccountName $_.Login -Enabled $true }
try { ... } catch { Write-Host $_.Exception.Message }
```

- Nommage **Verbe-Nom** ; `Get-Help commande` ; `| Get-Member` pour voir les propriétés d'un objet.
- **IPv6 :** 128 bits en hexadécimal ; `::` remplace une suite de groupes nuls (une seule fois) ; lien local `fe80::/10` ; globale `2000::/3` ; SLAAC : le poste construit son adresse d'après le préfixe annoncé par le routeur.