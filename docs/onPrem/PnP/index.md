# Kusi's Knowledge Base

## OnPrem PnP

### Install PnP Module

```powershell
Install-Module SharePointPnPPowerShell2016
```

### Update PnP Module

```powershell
Update-Module SharePointPnPPowerShell*
```

### Get PnP Module

```powershell
Get-Module SharePointPnPPowerShell* -ListAvailable | Select-Object Name,Version | Sort-Object Version -Descending
```

### Connect with current credentials

```powershell
Connect-PnPOnline -Url https://[Url] -CurrentCredentials
```

### Connect with web login credentials

```powershell
Connect-PnPOnline -Url https://[Url] -UseWebLogin
```
