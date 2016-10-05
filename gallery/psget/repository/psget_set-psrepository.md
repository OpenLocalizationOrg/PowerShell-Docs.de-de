# Set-PSRepository

Set-PSRepository werden die Werte für eine registrierte Repository festgelegt.

## Beschreibung

Das Cmdlet "Set-PSRepository" werden die Werte für ein Repository für registrierte Modul festgelegt.

## Cmdlet-syntax

```powershell
Get-Command -Name Set-PSRepository -Module PowerShellGet -Syntax
```
## Online-Hilfe-Cmdlet-Referenz

[Set-PSRepository](http://go.microsoft.com/fwlink/?LinkID=517128)

## Beispiele für Befehle

```powershell
PS C:\> Register-PSRepository -Name myRepository -SourceLocation "https://www.myget.org/F/powershellgetdemo/api/v2" -InstallationPolicy Trusted
PS C:\> Get-PSRepository
Name                      InstallationPolicy   SourceLocation
----                      ------------------   --------------
myRepository              Trusted              https://www.myget.org/F/powershellgetdemo/api/v2

# Set installation Policy for a repository
PS C:\> Set-PSRepository -Name myRepository -InstallationPolicy Untrusted
PS C:\> Get-PSRepository

Name                      InstallationPolicy   SourceLocation
----                      ------------------   --------------
myRepository              Untrusted            https://www.myget.org/F/powershellgetdemo/api/v2
```


### Set-PSRepository-Cmdlet mit Skript Support freigeben

Verwenden Sie Set-PSRepository-Cmdlets Hinzufügen der **ScriptSourceLocation** und **ScriptPublishLocation** auf der PSRepository.
```powershell
# Add script sharing locations to an existing PSRepository using Set-PSRepository object.
Set-PSRepository -Name MyGallery `
                 -ScriptSourceLocation https://MyGallery.com/api/v2/items/psscript/ `
                 -ScriptPublishLocation https://MyGallery.com/api/v2/package/

Get-PSRepository -Name GalleryINT | Format-List * -Force

Name : GalleryINT
SourceLocation : https://MyGallery.com/api/v2/
Trusted : True
Registered : True
InstallationPolicy : Trusted
PackageManagementProvider : NuGet
PublishLocation : https://MyGallery.com/api/v2/package/
ScriptSourceLocation : https://MyGallery.com/api/v2/items/psscript/
ScriptPublishLocation : https://MyGallery.com/api/v2/package/
ProviderOptions : {}

```


<!--HONumber=Oct16_HO1-->


