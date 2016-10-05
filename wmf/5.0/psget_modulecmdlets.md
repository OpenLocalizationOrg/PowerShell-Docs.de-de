# PowerShellGet-Cmdlets für die Modulverwaltung

- [Suchen-DscResource](https://technet.microsoft.com/en-us/library/mt654006.aspx)
- [Suchen-Modul](https://technet.microsoft.com/en-us/library/dn807167.aspx)
- [Suchen-Skript](https://technet.microsoft.com/en-us/library/mt654001.aspx)
- [Get-InstalledModule](https://technet.microsoft.com/en-us/library/mt653990.aspx)
- [Get-InstalledScript](https://technet.microsoft.com/en-us/library/mt653994.aspx)
- [Get-PSRepository](https://technet.microsoft.com/en-us/library/dn807170.aspx)
- [Install-Modul](https://technet.microsoft.com/en-us/library/dn807162.aspx)
- [Skript für die Installation](https://technet.microsoft.com/en-us/library/mt653998.aspx)
- [Neue ScriptFileInfo](https://technet.microsoft.com/en-us/library/mt653995.aspx)
- [Veröffentlichen-Module](https://technet.microsoft.com/en-us/library/dn807163.aspx)
- [Veröffentlichen von Skripts](https://technet.microsoft.com/en-us/library/mt654003.aspx)
- [Register-PSRepository](https://technet.microsoft.com/en-us/library/dn807168.aspx)
- [Save-Modul](https://technet.microsoft.com/en-us/library/mt653992.aspx)
- [Save-Skript](https://technet.microsoft.com/en-us/library/mt654004.aspx)
- [Set-PSRepository](https://technet.microsoft.com/en-us/library/dn807165.aspx)
- [Test-ScriptFileInfo](https://technet.microsoft.com/en-us/library/mt654005.aspx)
- [Deinstallieren Sie-Module](https://technet.microsoft.com/en-us/library/mt653996.aspx)
- [Deinstallieren Sie-Skript](https://technet.microsoft.com/en-us/library/mt653989.aspx)
- [Update-Modul](https://technet.microsoft.com/en-us/library/dn807166.aspx)
- [Update-ModuleManifest](https://technet.microsoft.com/en-us/library/mt654002.aspx)
- [Update-Skript](https://technet.microsoft.com/en-us/library/mt653997.aspx)
- [Update-ScriptFileInfo](https://technet.microsoft.com/en-us/library/mt653991.aspx)
- [Aufheben der Registrierung PSRepository](https://technet.microsoft.com/en-us/library/dn807161.aspx)

## Unterstützung der Installation von Modulabhängigkeiten wurde den Cmdlets „Get-InstalledModule“ und „Uninstall-Module“ hinzugefügt
- Dem Cmdlet „Publish-Module“ wurde die Auffüllung von Modulabhängigkeiten hinzugefügt. Die Listen „RequiredModules“ und „NestedModules“ von „PSModuleInfo“ werden bei der Vorbereitung der Abhängigkeitsliste eines Moduls verwendet, das veröffentlicht werden soll.
- Unterstützung der Installation von Abhängigkeiten in den Cmdlets „Install-Module“ und „Update-Module“. Modulabhängigkeiten werden standardmäßig installiert und aktualisiert.
- Der „-IncludeDependencies“-Parameter wurde dem Cmdlet „Find-Module“ hinzugefügt, um Modulabhängigkeiten in die Ergebnisse einzuschließen.
- Unterstützung von „-MaximumVersion“ wurde den Cmdlets „Find-Module“, „Install-Module“ und „Update-Module“ hinzugefügt.
- Die neuen Cmdlets „Get-InstalledModule“ und „Uninstall-Module“ wurden hinzugefügt.

## Demo von PowerShellGet-Cmdlets mit unterstützten Modulabhängigkeiten:

### Stellen Sie sicher, dass Modulabhängigkeiten im Repository verfügbar sind:
```powershell
Find-Module -Repository LocalRepo -Name RequiredModule1,RequiredModule2,RequiredModule3,NestedRequiredModule1,NestedRequiredModule2,NestedRequiredModule3 | Sort-Object -Property Name

Version    Name                     Repository    Description                  
-------    ----                     ----------    -----------                  
2.5        NestedRequiredModule1    LocalRepo     NestedRequiredModule1 module 
2.5        NestedRequiredModule2    LocalRepo     NestedRequiredModule2 module 
2.0        NestedRequiredModule3    LocalRepo     NestedRequiredModule3 module 
2.5        RequiredModule1          LocalRepo     RequiredModule1 module  
2.5        RequiredModule2          LocalRepo     RequiredModule2 module  
2.0        RequiredModule3          LocalRepo     RequiredModule3 module
```

### Erstellen Sie ein Modul mit Abhängigkeiten, die in den Eigenschaften „RequiredModules“ und „NestedModules“ des Modulmanifests angegeben sind.
```powershell
$RequiredModules = @('RequiredModule1',
                     @{ModuleName = 'RequiredModule2'; ModuleVersion = '1.5'; },
                     @{ModuleName = 'RequiredModule3'; RequiredVersion = '2.0'; })

$NestedRequiredModules = @('TestDepWithNestedRequiredModules1.psm1', 'NestedRequiredModule1',
                            @{ModuleName = 'NestedRequiredModule2'; ModuleVersion = '1.5'; },
                            @{ModuleName = 'NestedRequiredModule3'; RequiredVersion = '2.0'; })

New-ModuleManifest -Path 'C:\Program Files\WindowsPowerShell\Modules\TestDepWithNestedRequiredModules1\TestDepWithNestedRequiredModules1.psd1' `
-NestedModules $NestedRequiredModules -RequiredModules $RequiredModules -ModuleVersion "1.0" -Description "TestDepWithNestedRequiredModules1 module"
```

###  Veröffentlichen Sie zwei Versionen (**1.0** und **2.0**) des Moduls „TestDepWithNestedRequiredModules1“ mit Abhängigkeiten im Repository.
```powershell
Publish-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -NuGetApiKey "MyNuGet-ApiKey-For-LocalRepo"
```

###  Suchen Sie das Modul „TestDepWithNestedRequiredModules1“ mit seinen Abhängigkeiten, indem Sie „-IncludeDependencies“ angeben.
```powershell
Find-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo –IncludeDependencies -MaximumVersion "1.0"

Version    Name                                Repository  Description 
-------    ----                                ----------  -----------  
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module  
2.5        RequiredModule1                     LocalRepo   RequiredModule1 module      
2.5        RequiredModule2                     LocalRepo   RequiredModule2 module      
2.0        RequiredModule3                     LocalRepo   RequiredModule3 module      
2.5        NestedRequiredModule1               LocalRepo   NestedRequiredModule1 module
2.5        NestedRequiredModule2               LocalRepo   NestedRequiredModule2 module
2.0        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
``` 

### Verwenden Sie „Find-Module“-Metadaten zum Auffinden der Modulabhängigkeiten.
```powershell
$psgetModuleInfo = Find-Module -Repository MSPSGallery -Name ModuleWithDependencies2
$psgetModuleInfo.Dependencies.ModuleName

RequiredModule1
RequiredModule2
RequiredModule3
NestedRequiredModule1
NestedRequiredModule2
NestedRequiredModule3

$psgetModuleInfo.Dependencies

Name Value
---- -----
ModuleName RequiredModule1
CanonicalId PowerShellGet:RequiredModule1/#http://psget/psGallery/api/v2/

ModuleName RequiredModule2
ModuleVersion 2.0
CanonicalId PowerShellGet:RequiredModule2/2.0#http://psget/psGallery/api/v2/

ModuleName RequiredModule3
RequiredVersion 2.5
CanonicalId PowerShellGet:RequiredModule3/2.5#http://psget/psGallery/api/v2/

ModuleName NestedRequiredModule1
CanonicalId PowerShellGet:NestedRequiredModule1/#http://psget/psGallery/api/v2/

ModuleName NestedRequiredModule2
ModuleVersion 2.0
CanonicalId PowerShellGet:NestedRequiredModule2/2.0#http://psget/psGallery/api/v2/

ModuleName NestedRequiredModule3
RequiredVersion 2.5
CanonicalId PowerShellGet:NestedRequiredModule3/2.5#http://psget/psGallery/api/v2/
```

###  Installieren Sie das Modul „TestDepWithNestedRequiredModules1“ mit Abhängigkeiten.
```powershell
Install-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -RequiredVersion "1.0"
Get-InstalledModule

Version    Name                    Repository   Description                 
-------    ----                    ----------   -----------                 
1.0        NestedRequiredModule1   LocalRepo    NestedRequiredModule1 module
2.5        NestedRequiredModule2   LocalRepo    NestedRequiredModule2 module
2.0        NestedRequiredModule3   LocalRepo    NestedRequiredModule3 module
1.0        RequiredModule1         LocalRepo    RequiredModule1 module      
2.5        RequiredModule2                    LocalRepo    RequiredModule2 module 
2.0        RequiredModule3                    LocalRepo    RequiredModule3 module 
1.0        TestDepWithNestedRequiredModules1  LocalRepo    TestDepWithNestedRequiredModules1 module
```

###  Aktualisieren Sie das Modul „TestDepWithNestedRequiredModules1“ mit Abhängigkeiten.
```powershell
Find-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -AllVersions

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module

Update-Module -Name TestDepWithNestedRequiredModules1 -RequiredVersion 2.0
Get-InstalledModule

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        NestedRequiredModule1               LocalRepo   NestedRequiredModule1 module
2.5        NestedRequiredModule2               LocalRepo   NestedRequiredModule2 module
2.0        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
2.5        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
1.0        RequiredModule1                     LocalRepo   RequiredModule1 module
2.5        RequiredModule2                     LocalRepo   RequiredModule2 module
2.0        RequiredModule3                     LocalRepo   RequiredModule3 module
2.5        RequiredModule3                     LocalRepo   RequiredModule3 module
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
```

###  Führen Sie das Cmdlet „Uninstall-Module“ zum Deinstallieren eines Moduls aus, das Sie mit PowerShellGet installiert haben.
Wenn ein anderes Modul von dem Modul abhängt, das Sie löschen möchten, löst PowerShellGet einen Fehler aus.
```powershell
Get-InstalledModule -Name RequiredModule1 | Uninstall-Module

PackageManagement\Uninstall-Package : The module 'RequiredModule1' of version '2.5' in module base folder 'C:\Program Files\WindowsPowerShell\Modules\RequiredModule1\2.5' cannot be uninstalled, because one or more other modules 'ModuleWithDependencies2' are dependent on this module. Uninstall the modules that depend on this module before uninstalling module 'RequiredModule1'.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\PSGet.psm1:1303 char:25
+ ... $null = PackageManagement\\Uninstall-Package @PSBoundParameters
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : InvalidOperation: (Microsoft.Power...ninstallPackage:UninstallPackage) [Uninstall-Package], Exception
+ FullyQualifiedErrorId : UnableToUninstallAsOtherModulesNeedThisModule,Uninstall-Package,Microsoft.PowerShell.PackageManagement.Cmdlets.UninstallPackage
```

## Cmdlet „Save-Module“
```powershell
Save-Module -Repository MSPSGallery -Name ModuleWithDependencies2 -Path C:\MySavedModuleLocation
dir C:\MySavedModuleLocation

Directory: C:\MySavedModuleLocation

Mode LastWriteTime Length Name
---- ------------- ------ ----
d----- 4/21/2015 5:40 PM ModuleWithDependencies2
d----- 4/21/2015 5:40 PM NestedRequiredModule1
d----- 4/21/2015 5:40 PM NestedRequiredModule2
d----- 4/21/2015 5:40 PM NestedRequiredModule3
d----- 4/21/2015 5:40 PM RequiredModule1
d----- 4/21/2015 5:40 PM RequiredModule2
d----- 4/21/2015 5:40 PM RequiredModule3
```

## Cmdlet „Update-ModuleManifest“
Dieses neue Cmdlet wird verwendet, um die Manifestdatei mit eingegebenen Eigenschaftswerten zu aktualisieren. Es verwendet dieselben Parameter wie „Test-ModuleManifest“.

Wir stellen fest, dass viele Modulentwickler in exportierten Werten wie „FunctionsToExport“, „CmdletsToExport“ usw. „\*“ angeben möchten. Während der Veröffentlichung des Moduls im PowerShell-Katalog werden nicht angegebene Funktionen und Befehle nicht ordnungsgemäß im Katalog aufgefüllt. Deshalb sollten Modulentwickler ihre Manifeste mit ordnungsgemäßen Werten aktualisieren.

Wenn es Module mit exportierten Eigenschaften gibt, füllt „Update-ModuleManifest“ die angegebene Manifestdatei mit Informationen aus exportierten Funktionen, Cmdlets, Variablen usw. auf:
```powershell
Get-Content -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
@{
# Script module or binary module file associated with this manifest.
# RootModule = ''
# Version number of this module.
ModuleVersion = '2.5'
# ID used to uniquely identify this module
GUID = '610e5c5b-dc42-4eaa-8511-ebfb44066d5e'

#(Other properties removed here for Simplicity…)

# Functions to export from this module
FunctionsToExport = '*'
# Cmdlets to export from this module
CmdletsToExport = '*'
# Variables to export from this module
VariablesToExport = '*'
# Aliases to export from this module
AliasesToExport = '*'
}
```

Nach „Update-ModuleManifest“:
```powershell
Update-ModuleManifest -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
Get-Content -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
#
# Module manifest for module 'NewManifest'
#
# Generated by: author name
#
# Generated on: 11/13/2015
#
@{
# Script module or binary module file associated with this manifest.
# RootModule = ''
# Version number of this module.
ModuleVersion = '2.5'
# ID used to uniquely identify this module
GUID = '610e5c5b-dc42-4eaa-8511-ebfb44066d5e'
# Functions to export from this module
FunctionsToExport = 'Get-FooFn Get-FooWF'
# Cmdlets to export from this module
CmdletsToExport = 'Test-PSGetTestCmdlet'
}
```

Jedem Modul sind auch Metadatenfelder zugeordnet. Um die Metadaten im PowerShell-Katalog ordnungsgemäß anzuzeigen, können Sie mit „Update-ModuleManifest“ diese Felder unter „PrivateData“ auffüllen.
```powershell
Update-ModuleManifest -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1" -Tags "Tag1" -LicenseUri "http://license.com" -ProjectUri "http://project.com" -IconUri "http://icon.com" -ReleaseNotes "Test module"
```
Die Hashtable „PrivateData“ in der Vorlage der Manifestdatei hat die folgenden Eigenschaften:
```powershell
# Private data to pass to the module specified in RootModule/ModuleToProcess. This may also contain a PSData hashtable with additional module metadata used by PowerShell.
PrivateData = @{
    PSData = @{
        # Tags applied to this module. These help with module discovery in online galleries.
        # Tags = @()

        # A URL to the license for this module.
        # LicenseUri = ''
    
        # A URL to the main website for this project.
        # ProjectUri = ''
        
        # A URL to an icon representing this module.
        # IconUri = ''
        
        # ReleaseNotes of this module
        # ReleaseNotes = ''
        
        # External dependent modules of this module
        # ExternalModuleDependencies = ''
    } # End of PSData hashtable
} # End of PrivateData hashtable
```
***Hinweis:*** „DscResourcesToExport“ wird nur von der neuesten PowerShell-Version 5.0 unterstützt. Wenn Sie eine frühere Version von PowerShell ausführen, kann das Feld nicht aktualisiert werden.


<!--HONumber=Oct16_HO1-->


