# Get-PSRepository

Ruft die registrierten Repositorys auf einem Computer ab.

## Beschreibung

Das Cmdlet "Get-PSRepository" ruft PowerShell-Modul-Repositorys, die registriert sind, für den aktuellen Benutzer auf einem Computer.

Für jeden registrierten Repository gibt Get-PSRepository ein PSRepository-Objekt, das optional geleitet werden kann, Unregister-PSRepository zum Aufheben der Registrierung einer registrierten Repository zurück.

## Cmdlet-syntax
```powershell
Get-Command -Name Get-PSRepository -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Get-PSRepository](http://go.microsoft.com/fwlink/?LinkID=517127)

## Beispiele für Befehle

```powershell

# Properties of Get-PSRepository returned object
Get-PSRepository PSGallery | Format-List * -Force

Name                      : PSGallery
SourceLocation            : https://www.powershellgallery.com/api/v2/
Trusted                   : False
Registered                : True
InstallationPolicy        : Untrusted
PackageManagementProvider : NuGet
PublishLocation           : https://www.powershellgallery.com/api/v2/package/
ScriptSourceLocation      : https://www.powershellgallery.com/api/v2/items/psscript/
ScriptPublishLocation     : https://www.powershellgallery.com/api/v2/package/
ProviderOptions           : {}

# Get all registered repositories
Get-PSRepository

# Get a specific registered repository
Get-PSRepository PSGallery

Name                      InstallationPolicy   SourceLocation
----                      ------------------   --------------
PSGallery                 Untrusted            https://www.powershellgallery.com/api/v2/

# Get registered repository with wildcards
Get-PSRepository *Gallery*

```

<!--HONumber=Oct16_HO1-->


