# Kusi's Knowledge Base

## SharePoint 2016 / 2019

### Close Connection to reconnect with another user

```url
https:<>/_layouts/closeConnection.aspx?loginasanotheruser=true
```

### App Launcher

```powershell
Enable-SPFeature -identity CustomTiles
```

Edit List on

```url
https:<>/Lists/Custom Tiles
```

### Mobile View

For a better mobile view feeling you can add in the Master Page in the Head Element.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

### WebService in WSP

Create a new Visual Studio Project **SharePoint Empty Project**

![Visual Studio Prject](./Assets/VSWebServiceNew.png)

In the Solution Explorer on the Project right click an choose **Add...** and then **Mapped Folder**

![Visual Studio Prject](./Assets/VSShareMappedFolder.png)

select the **ISAPI** Folder

![Visual Studio Prject](./Assets/VSFolderISAPI.png)

> IMPORTANT: take all folders and file names in lower case, otherwise the service will not working.

Add a Project folder and then 3 files and add it to the Projectfolder in the ISAPI Folder.

Create a file [servicename].svc with following content:

```xml
<%@ ServiceHost Language="C#" Debug="true"
    Service="[ServicenameInclNamespace], $SharePoint.Project.AssemblyFullName$"
    CodeBehind="[servicename].svc.cs"
    Factory="Microsoft.SharePoint.Client.Services.MultipleBaseAddressWebServiceHostFactory,
    Microsoft.SharePoint.Client.ServerRuntime, Version=16.0.0.0, Culture=neutral,
    PublicKeyToken=71e9bce111e9429c" %>
```

Create a file [servicename].svc.cs with following content:

```cs
namespace [Namespace]
{
    class [ServiceName] : [InterfaceServiceName]
    {
        public string [FunctionName](string [PropertyName])
        {
        }
    }
}
```

Create a file I[Servicename].cs with following content:

```cs
using System.ServiceModel;
using System.ServiceModel.Web;

namespace [Namespace]
{
    [ServiceContract]
    interface [IntefaceServiceName]
    {
        [OperationContract]
        [WebGet(UriTemplate = "[ServiceFunctionName]/{[PropertyName]}",
            ResponseFormat = WebMessageFormat.Json)]
        string [FunctionName](string [PropertyName]);
    }
}
```

Create a WSP with **Publish** the Project and deploy the WSP to the server. After deployment the service can called like:

```
https://[SharePointWebUrl]/_vti_bin/[FolderStructureBelowISAPI][ServiceName].svc/[ServiceFunctionName]/[PropertyName]
```

### Custom Url protocol handler

With a custom Url protocol handler you can open from the browser a custom application with parameter. 

Example call:
```html
<a href="myhandler:paramater">test</a>
```

Registry Entry:
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\myhandler]
"URL Protocol"="myhandler"

[HKEY_CLASSES_ROOT\myhandler\shell]

[HKEY_CLASSES_ROOT\myhandler\shell\open]

[HKEY_CLASSES_ROOT\myhandler\shell\open\command]
@="C:\\Programm Files\\myhandler\\myhandler.exe \"%1\""
```

c# Console or Forms App
```csharp
static void Main(string[] args)
{
    args[0] // returns myhandler:paramater
}
```
### Merge ULS Log

Merge the Log files from ULS Log from each Server in a time range to one file.

```powershell
Set-SPLogLevel -TraceSeverity Verbose
$start = get-date
 
# Make Test
 
$end = Get-Date
Set-SPLogLevel -TraceSeverity None

Merge-SPLogFile -StartTime $start -EndTime $end -Correlation <ID> -Path "c:\temp\mergedLogFile.log" -Overwrite
```

