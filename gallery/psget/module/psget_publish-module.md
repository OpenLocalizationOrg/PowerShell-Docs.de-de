
# Publish-Module

Veröffentlicht ein angegebenes Modul vom lokalen Computer auf einen Onlinekatalog.

## Beschreibung

Die **Veröffentlichen-Module** Cmdlet veröffentlicht ein Modul auf einen Onlinekatalog NuGet basierenden mithilfe eines API-Schlüssels als Teil eines Benutzerprofils im Katalog gespeichert. Sie können das Modul So veröffentlichen Sie den Modulnamen, oder der Pfad zu dem Ordner mit dem Modul angeben.

Wenn Sie den Namen ein Moduls angeben **Veröffentlichen-Module** veröffentlicht das erste Modul, das durch Ausführen finden würde `Get-Module -ListAvailable <Name>`. Wenn Sie angeben, dass mindestens ein Modul zu veröffentlichen, die Version **Veröffentlichen-Module** veröffentlicht das erste Modul mit einer Version, die größer oder gleich der minimalen Version, die Sie angegeben haben.

Veröffentlichen eines Moduls muss die Metadaten, die auf der Seite "Katalog" für das Modul angezeigt wird. Die erforderlichen Metadaten enthält den Modulnamen, Version, Beschreibung und Autor. Obwohl die meisten Metadaten aus dem modulmanifest durchgeführt wird, muss einige Metadaten angegeben werden, **Veröffentlichen-Module** Parameter, wie z. B. *Tag, ReleaseNote, IconUri, ProjectUri,* und *LicenseUri*, da diese Parameter Felder in einem Katalog NuGet basierenden.

Der RequiredVersion-Parameter können Sie die genaue Version eines Moduls zu veröffentlichenden angeben.
Path-Parameter unterstützt auch den Modul Basispfad mit den Ordner Version.
Der Force-Switch-Parameter auf Publish-Module-Cmdlet startet die NuGet.exe ohne Bestätigung.

## Cmdlet-syntax
```powershell
Get-Command -Name Publish-Module -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Veröffentlichen-Module](http://go.microsoft.com/fwlink/?LinkID=398575)

## Beispiele für Befehle

```powershell
ContosoServer module with different versions to be published.
PS C:\\windows\\system32> Get-Module -Name ContosoServer -ListAvailable
Directory: C:\\Program Files\\WindowsPowerShell\\Modules
ModuleType Version Name ExportedCommands
---------- ------- ---- ----------------
Manifest 2.8 ContosoServer Get-ContosoServer
Manifest 2.0 ContosoServer Get-ContosoServer
Manifest 1.5 ContosoServer Get-ContosoServer
Manifest 1.0 ContosoServer Get-ContosoServer
PS C:\\windows\\system32> Publish-Module -Name ContosoServer -RequiredVersion 1.0 -Repository LocalRepo -NuGetApiKey Local-Repo-NuGet-ApiKey
PS C:\\windows\\system32> Find-Module -Name ContosoServer -Repository LocalRepo
Version Name Repository Description
------- ---- ---------- -----------
1.0 ContosoServer LocalRepo ContosoServer module
PS C:\\windows\\system32> Publish-Module -Name ContosoServer -RequiredVersion 1.5 -Repository LocalRepo -NuGetApiKey Local-Repo-NuGet-ApiKey
PS C:\\windows\\system32> Find-Module -Name ContosoServer -Repository LocalRepo
Version Name Repository Description
------- ---- ---------- -----------
1.0 ContosoServer LocalRepo ContosoServer module
1.5 ContosoServer LocalRepo ContosoServer module
PS C:\\windows\\system32> Publish-Module -Path "C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\2.0" -Repository LocalRepo -NuGetApiKey Local-Repo-NuGet-ApiKey
PS C:\\windows\\system32> Find-Module -Name ContosoServer -Repository LocalRepo
Version Name Repository Description
_------ ---- ---------- -----------
1.0 ContosoServer LocalRepo ContosoServer module
1.5 ContosoServer LocalRepo ContosoServer module
2.0 ContosoServer LocalRepo ContosoServer module
```

## Veröffentlichen ein Modul mit Abhängigkeiten

### Erstellen Sie ein Modul mit Abhängigkeiten und Versionsbereich in RequiredModules-Eigenschaft des modulmanifest angegeben.

**Hinweis:**
  - \ * wird nur in MaximumVersion unterstützt und sollte auch am Ende der Zeichenfolge sein. 
  - \ * durch 999999999 in der Version des Objekts ersetzt.

```powershell
PS C:\windows\system32> $requiredModules = @( @{ModuleName = 'RequiredModule1'; ModuleVersion = '0.1'; MaximumVersion = '1.9'; }, @{ModuleName = 'RequiredModule2'; MaximumVersion = '1.*'; })

PS C:\windows\system32> cd C:\MyModules\ModuleWithDependencies

PS C:\MyModules\ModuleWithDependencies> New-ModuleManifest -Path .\ModuleWithDependencies.psd1 -ModuleVersion 1.0 -RequiredModules $requiredModules -Description 'ModuleWithDependencies demo module'
```

### Veröffentlichen Sie ModuleWithDependencies-Modul mit Abhängigkeiten im Repository.

```powershell
PS C:\MyModules\ModuleWithDependencies> Publish-Module -Path C:\MyModules\ModuleWithDependencies -Repository LocalRepo
```

### Suchen Sie ModuleWithDependencies-Modul mit Abhängigkeiten durch Angabe IncludeDependencies-

```powershell
PS C:\MyModules\ModuleWithDependencies> Find-Module -Name ModuleWithDependencies -Repository LocalRepo -IncludeDependencies

Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
1.0        ModuleWithDependencies              Module     localrepo            ModuleWithDependencies demo module
1.5        RequiredModule1                     Module     localrepo            RequiredModule1 module
1.5        RequiredModule2                     Module     localrepo            RequiredModule2 module
```

### Installieren Sie das Modul ModuleWithDependencies mit Abhängigkeiten.
Beachten Sie, dass während der Installation Abhängigkeit Versionsbereiche berücksichtigt werden.

```powershell
PS C:\windows\system32> Get-InstalledModule
PS C:\windows\system32>
PS C:\windows\system32> Install-Module -Name ModuleWithDependencies -Repository LocalRepo
PS C:\windows\system32>
PS C:\windows\system32> Get-InstalledModule

Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
1.0        ModuleWithDependencies              Module     localrepo            ModuleWithDependencies demo module
1.5        RequiredModule1                     Module     localrepo            RequiredModule1 module
1.5        RequiredModule2                     Module     localrepo            RequiredModule2 module
```

### Inhalt der ModuleWithDependencies2 Modul Manifestdatei

```powershell
@{
# Version number of this module.
ModuleVersion = '2.0'
# ID used to uniquely identify this module
GUID = '0eae34da-99dd-4608-8d28-c614fe7b0841'
# Author of this module
Author = 'manikb'
# Company or vendor of this module
CompanyName = 'Unknown'
# Copyright statement for this module
Copyright = '(c) 2015 manikb. All rights reserved.'
# Description of the functionality provided by this module
Description = 'ModuleWithDependencies2 module'
# Modules that must be imported into the global environment prior to importing this module
RequiredModules = @('RequiredModule1',
@{ModuleName = 'RequiredModule2'; ModuleVersion = '2.0'; },
@{ModuleName = 'RequiredModule3'; RequiredVersion = '2.5'; },
@{ModuleName = 'RequiredModule4'; ModuleVersion = '1.1'; MaximumVersion = '2.0'; },
@{ModuleName = 'RequiredModule5'; MaximumVersion = '1.5'; })
# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
NestedModules = @('NestedRequiredModule1',
@{ModuleName = 'NestedRequiredModule2'; ModuleVersion = '2.0'; },
@{ModuleName = 'NestedRequiredModule3'; RequiredVersion = '2.5'; },
@{ModuleName = 'NestedRequiredModule4'; ModuleVersion = '0.7'; MaximumVersion = '2.4'; },
@{ModuleName = 'NestedRequiredModule5'; MaximumVersion = '1.6'; },'ModuleWithDependencies2.psm1')
# Functions to export from this module
FunctionsToExport = '\*'
# Cmdlets to export from this module
CmdletsToExport = '\*'
# Variables to export from this module
VariablesToExport = '\*'
# Aliases to export from this module
AliasesToExport = '\*'
# Private data to pass to the module specified in RootModule/ModuleToProcess. This may also contain a PSData hashtable with additional module metadata used by PowerShell.
PrivateData = @{
    PSData = @{
      # Tags applied to this module. These help with module discovery in online galleries.
      Tags = 'Tag1', 'Tag2', 'Tag-ModuleWithDependencies2-2.0'
      # A URL to the license for this module.
      LicenseUri = 'http://modulewithdependencies2.com/license'
      # A URL to the main website for this project.
      ProjectUri = 'http://modulewithdependencies2.com/'
      # A URL to an icon representing this module.
      IconUri = 'http://modulewithdependencies2.com/icon'
      # ReleaseNotes of this module
      ReleaseNotes = 'ModuleWithDependencies2 release notes'
    } # End of PSData hashtable
} # End of PrivateData hashtable
}
```


### Externe Abhängigkeiten
Einige modulabhängigkeiten extern verwaltet werden können, in diesem Fall sollten sie zu der ExternalModuleDependencies-Eintrag im Abschnitt PSData des modulmanifests hinzugefügt werden.

Wenn 'SnippetPx' im Repository nicht verfügbar ist, wird unter Fehler ausgelöst.
```powershell
Publish-PSArtifactUtility : PowerShellGet cannot resolve the module dependency 'SnippetPx' of the module 'TypePx' on the repository 'LocalRepo'. Verify that the dependent module 'SnippetPx' is available in the repository 'LocalRepo'. If this dependent 'SnippetPx' is managed externally, add it to the ExternalModuleDependencies entry in the PSData section of the module manifest.
```


<!--HONumber=Oct16_HO1-->


