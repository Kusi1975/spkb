# Kusi's Knowledge Base

## M365 PnP

### Install PnP PowerShell

```powershell
Install-Module -Name PnP.PowerShell -Force
```

### Connect to Site Collection

```powershell
Connect-PnPOnline -Url https://<url> -Interactive
```

### Query more than 5000 Elements

```powershell
Connect-PnPOnline ...
$ctx = Get-PnPContext
$ctx.Load($listTitle)
$ctx.ExecuteQuery()

$query = New-Object Microsoft.SharePoint.Client.CamlQuery
$query.ViewXml = "<View Scope='RecursiveAll'><Query><Where></Where></Query><RowLimit Paged='TRUE'>500</RowLimit></View>"
$query.AllowIncrementalResults = $true

Do
{
    $listItems = $list.GetItems($query)
    $ctx.Load($listItems)
    $ctx.ExecuteQuery()
    if ($listItems.count -gt 0) {
        $listItems |% {
            Write-Host $_.FieldValues.FileRef 
        }
    }
    $query.ListItemCollectionPosition = $listItems.ListItemCollectionPosition			
}
While($query.ListItemCollectionPosition -ne $null)
```
