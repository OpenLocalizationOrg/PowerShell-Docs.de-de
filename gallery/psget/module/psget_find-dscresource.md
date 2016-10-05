# Find-DscResource

DSC-Ressourcen ermittelt in Modulen.

## Beschreibung

Findet das Cmdlet suchen DscResource [Desired State Configuration (DSC)](https://msdn.microsoft.com/en-us/PowerShell/dsc/overview) in Modulen, die den angegebenen aus registrierten Repositorys Kriterien enthaltenen Ressourcen.
Für die einzelnen Module, die mit diesem Cmdlet findet, gibt suchen-DscResource ein PSGetDscResourceInfo-Objekt, das Sie übergeben werden können Install-Modul zum Installieren der Module, die mit den Ressourcen, die mit diesem Cmdlet zurückgegeben.

DSC ist eine neue Verwaltungsplattform in Windows PowerShell, die es ermöglicht, Konfigurationsdaten für Softwaredienste bereitzustellen und zu verwalten und die Umgebung zu verwalten, in der diese Dienste ausgeführt werden.

DSC-Ressourcen sind die Bausteine einer DSC-Konfiguration. Eine Ressource macht Eigenschaften verfügbar, die konfiguriert werden können (Schema), und enthält die PowerShell-Skriptfunktionen, die der lokale Konfigurations-Manager wie gewünscht ausführt.

Mit einer Ressource kann etwas Allgemeines wie eine Datei oder Spezifisches wie eine IIS-Servereinstellung modelliert werden. Gruppen ähnlicher Ressourcen werden in einem DSC-Modul kombiniert, das alle erforderlichen Dateien in einer Struktur organisiert, die portierbar ist und Metadaten zum Bestimmen des Zwecks der Ressourcen enthält.

- Suchen-DscResource kann mit Version-Parameter filtern: MinimumVersion, RequiredVersion, AllVersions.
  - Diese Parameter schließen sich gegenseitig aus.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Modul ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt suchen-DscResource die neueste Version des Moduls, der gleich oder größer als die angegebene minimale Version oder die neueste Version des Moduls ist, wenn keine Mindestversion angegeben wird.
  - Wenn der RequiredVersion-Parameter angegeben ist, gibt suchen-DscResource nur die Version des Moduls, das genau mit die angegebene Version übereinstimmt.
- Suchen-DscResource Filtern nach Metadaten des Moduls mit dem Tag-Parameter
- Suchen-DscResource Repository-spezifischen Suchsprache filtern kann mit dem Filter-Parameter.
- Suchen-DscResource kann Module aus allen oder nur wenige der registrierten Repositorys filtern.

## Cmdlet-syntax
```powershell
Get-Command -Name Find-DscResource -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Suchen-DscResource](http://go.microsoft.com/fwlink/?LinkId=517196)

## Beispiele für Befehle
```powershell

# Find a specific DSC Resource
Find-DscResource -Name xIisFeatureDelegation

Name                                Version    ModuleName                          Repository
----                                -------    ----------                          ----------
xIisFeatureDelegation               1.10.0.0   xWebAdministration                  PSGallery

# Find all available DSC Resources from all registered repositories
Find-DscResource

# Find a DSC resource by name
Find-DscResource -Name xWebsite

# Find multiple DSC Resources
Find-DscResource -Name xIisHandler, xFirewall

# Find all DSC resources contained within a specific module
Find-DscResource -ModuleName xNetworking
Find-DscResource -ModuleName xWebAdministration

# Find all DSC resources in modules with DSCResourceKit or DesiredStateConfiguration
Find-DscResource -Tag DesiredStateConfiguration, DSCResourceKit

# Find available DSC Resources from few registered repositories
Find-DscResource -Repository PSGallery,PrivatePSGallery

# Find all DSC Resources in a specified repository
Find-DscResource -Repository PSGallery

# Find DSC Resources from all versions of a module
Find-DscResource -ModuleName xNetworking -AllVersions

# Find DSC Resources with module name and MinimumVersion.
Find-DscResource -ModuleName xNetworking -MinimumVersion 1.1

# Find DSC Resources with module name and exact version
Find-DscResource -ModuleName xNetworking -RequiredVersion 2.1.1

# Find DSC Resources defined modules with -Filter based search. -Filter searches in description and module names
Find-DscResource -Filter Domain

# Find all DSC Resources with tags Azure or DSC in module metadata
Find-DscResource -Tag Azure, DSC

```


<!--HONumber=Oct16_HO1-->


