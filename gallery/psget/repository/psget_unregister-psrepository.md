# Unregister-PSRepository

Hebt die Registrierung eines Repositorys.

## Beschreibung

Das Cmdlet "Unregister-PSRepository" hebt die Registrierung eines Repositorys für den aktuellen Benutzer.
- Aufheben der Registrierung und erneute Registrierung des Repositorys PSGallery kann für ein Unternehmen und Szenarien getrennt.
- Benutzer können die PSGallery erneut registrieren, indem einfach ausführen. `Register-PSRepository -Default`
- Da PSGallery die Standardeinstellung ist Repository im Veröffentlichen-Module und Veröffentlichungsskript Cmdlets zu veröffentlichen, wird ein Fehler ausgelöst, wenn PSGallery nicht in der Liste registrierte Repository verfügbar ist.

## Cmdlet-syntax

```powershell
Get-Command -Name Unregister-PSRepository -Module PowerShellGet -Syntax
```
## Online-Hilfe-Cmdlet-Referenz

[Aufheben der Registrierung PSRepository](http://go.microsoft.com/fwlink/?LinkID=517130)

## Beispiele für Befehle

```powershell
Unregister-PSRepository -Name "MyPrivateGallery"

Get-PSRepository exp | Unregister-PSRepository
```

### Aufheben der Registrierung und erneute Registrierung des Repositorys PSGallery kann für ein Unternehmen und Szenarien getrennt.
```powershell

# Unregister PSGallery repository
Unregister-PSRepository PSGallery

# Publish-Module throws an error when PSGallery is not a registered repository
Publish-Module -Name MyModule
publish-module : Unable to find repository 'PSGallery'. Use Get-PSRepository to see all available repositories. Try again after specifying a valid repository name. You can use 'Register-PSRepository -Default' to register the PSGallery repository.
At line:1 char:1
+ publish-module -name mymodule
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (PSGallery:String) [Publish-Module], ArgumentException
    + FullyQualifiedErrorId : PSGalleryNotFound,Publish-Module

# Re-register PSGallery repository
Register-PSRepository -Default
```

<!--HONumber=Oct16_HO1-->


