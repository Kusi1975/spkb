# Kusi's Knowledge Base

## OnPrem PowerShell

### Update Content Database

```powershell
Get-SPContentDatabase |% { Upgrade-SPContentDatabase -Identity $_.Id -Confirm:$false }
```

### Update CA Content Database

```powershell
$wa = Get-SPWebApplication -IncludeCentralAdministration | Where { $_.DefaultServerComment -eq "SharePoint Central Administration v4"}

Get-SPContentDatabase -WebApplication $wa |% { Upgrade-SPContentDatabase -Identity $_.Id -Confirm:$false }
```

### New Developer Certificate

```powershell
Import-Module WebAdministration
Set-Location IIS:\SslBindings
New-WebBinding -Name "Default Web Site" -IP "*" -Port 443 -Protocol https
$c = New-SelfSignedCertificate -DnsName *.myexample.com,*.my.com -CertStoreLocation cert:\LocalMachine\My
```

Open mmc.exe and add Server Certificate

Copy New Certificate from Personal to Trusted Root Certifiaction Authorities

Open IIS and change the Binding Certificate

### Error: cannot be loaded because running scripts is disabled on this System

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

### Get all loaded Assemblies in Powershell

```powershell
[System.AppDomain]::CurrentDomain.GetAssemblies() | Where-Object Location | Sort-Object -Property FullName | Select-Object -Property Name, Location, Version | Out-GridView
```
