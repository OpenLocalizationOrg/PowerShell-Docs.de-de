# Find-Module
Sucht ein Onlinekatalog Module, die bestimmte Kriterien erfüllen.

## Beschreibung
Suchen-Modul ermittelt die Modulen eingetragene Repositorys, die den angegebenen Kriterien entspricht.
Für jedes Modul gefunden wird gibt suchen-Modul ein PSRepositoryItemInfo-Objekt, das optional geleitet werden kann Install-Modul für die Installation der Module zurück.

- Suchen-Module kann Filter basierend auf Modul den Inhalt durch den - Befehl - DscResource, - RoleCapability und - Parameter enthält.
- Suchen-Module mit Version-Parameter filtern: MinimumVersion, MaximumVersion, RequiredVersion, AllVersions.
  - Diese Parameter verwendet werden, mit Ausnahme von MinmimumVersion und MaximumVersion.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Modul ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt suchen-Module die neueste Version des Moduls, der gleich oder größer als die angegebene minimale Version oder die neueste Version des Moduls ist, wenn keine Mindestversion angegeben wird. 
  - Wenn der RequiredVersion-Parameter angegeben ist, gibt suchen-Modul nur die Version des Moduls, die mit die angegebene Version übereinstimmt.
- „Find-Module“ kann Modulmetadaten anhand des „-Tag“-Parameters filtern.
- Suchen-Module kann eine Repository-spezifischen Suchsprache Filterung mit dem Filter-Parameter.
- Suchen-Module kann Module aus allen oder nur wenige der registrierten Repositorys filtern.

## Cmdlet-syntax
```powershell
Get-Command -Name Find-Module -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Suchen-Modul](http://go.microsoft.com/fwlink/?LinkID=398574)

## Beispiele für Befehle
```powershell
# Find a specific module
Find-Module Azure

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.3.2      Azure                               PSGallery            Microsoft Azure PowerShell - Service Management

# Find multiple modules
Find-Module Azure,AzureRM

# Find modules with wildcards in -Name
Find-Module -Name AzureRM*

# Find all versions of a module
Find-Module -Name PSReadline -AllVersions

# Find a module with -MinimumVersion. 
# With MinimumVersion we can find a module whose version is greate than or equal to the specified MinimumVersion value.
Find-Module -Name PSReadline -MinimumVersion 1.0.0.12

# Find a module with MaximumVersion
Find-Module -Name PSReadline -MaximumVersion 1.0.0.13

# Find a module with both MinimumVersion and MaximumVersion range.
Find-Module -Name PSReadline -MinimumVersion 1.0.0.12 -MaximumVersion 1.0.0.13

# Find a module with exact version
Find-Module -Name AzureRM -RequiredVersion 1.3.2

# Find a module from the specified repository
Find-Module -Name Contoso -Repository MyLocalRepo

# Find available modules from all registered repositories
Find-Module

# Find available modules from few registered repositories
Find-Module -Repository PSGallery,PrivatePSGallery

# Find a module along with its dependencies
Find-Module -Name AzureRM -IncludeDependencies

# Find all modules with Dsc resources
Find-Module -Includes DscResource

# Find modules with a specific DscResource
Find-Module -DscResource xFirewall

# Find all modules with cmdlets
Find-Module -Includes Cmdlet

# Find all modules with functions
Find-Module -Includes Function

# Find all modules with Role Capabilities
Find-Module -Includes RoleCapability

# Find all modules with the specified Role Capability name
Find-Module -RoleCapability RoleCap1
Find-Module -RoleCapability RoleCap2 -Includes RoleCapability

# Find modules with specific commands
Find-Module -Command Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer
Find-Module -Command Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer -Includes Cmdlet
Find-Module -Command Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer -Includes Function

# Find modules with -Filter based search. -Filter searches in description and names
Find-Module -Filter Cookbook
Find-Module -Filter RBAC
Find-Module -Filter 'App Domain' -Includes 'DscResource'

# Find all modules with tags Azure or DSC
Find-Module -Tag Azure, DSC

# Properties of Find-Module returned object
Find-Module AzureRM.Profile | Format-List * -Force

Name                       : AzureRM.profile
Version                    : 1.0.8
Type                       : Module
Description                : Microsoft Azure PowerShell - Profile credential management cmdlets for Azure Resource
                             Manager
Author                     : Microsoft Corporation
CompanyName                : {elogeel, azure-sdk}
Copyright                  : Microsoft Corporation. All rights reserved.
PublishedDate              : 5/4/2016 9:40:33 PM
InstalledDate              :
UpdatedDate                :
LicenseUri                 : https://raw.githubusercontent.com/Azure/azure-powershell/dev/LICENSE.txt
ProjectUri                 : https://github.com/Azure/azure-powershell
IconUri                    :
Tags                       : {PSModule}
Includes                   : {Function, RoleCapability, Command, DscResource...}
PowerShellGetFormatVersion :
ReleaseNotes               : https://github.com/Azure/azure-powershell/blob/dev/ChangeLog.md
Dependencies               : {}
RepositorySourceLocation   : https://www.powershellgallery.com/api/v2/
Repository                 : PSGallery
PackageManagementProvider  : NuGet
AdditionalMetadata         : {downloadCount, description, copyright, FileList...}

```


<!--HONumber=Oct16_HO1-->


