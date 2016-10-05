# Get-InstalledScript

Ruft die Skripts auf einem Computer installiert.

## Beschreibung

Das Cmdlet "Get-InstalledScript" installiert PowerShell-Skripts auf einem Computer abgerufen.

Für jedes installierte Skript gibt Get-InstalledScript ein PSRepositoryItemInfo-Objekt, das optional geleitet werden kann deinstallieren-Skript zum Deinstallieren der installierten Skripts zurück.

- Get-InstalledScript kann installierten Skripts basierend auf Name, Versionsparameter filtern.
- Get-InstalledScript kann mit Version-Parameter filtern: MinimumVersion, MaximumVersion, RequiredVersion, AllVersions.
  - Diese Parameter verwendet werden, mit Ausnahme von MinmimumVersion und MaximumVersion.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Skripts ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt Get-InstalledScript die neueste Version des installierten Skripts, die gleich oder größer als die angegebene minimale Version oder die neueste Version des Skripts ist, wenn keine Mindestversion angegeben wird. 
  - Wenn der RequiredVersion-Parameter angegeben wird, gibt Get-InstalledScript nur die Version des installierten Skripts, die mit die angegebene Version übereinstimmt.

## Cmdlet-syntax

```powershell
Get-Command -Name Get-InstalledScript -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Get-InstalledScript](http://go.microsoft.com/fwlink/?LinkId=619790)

## Beispiele für Befehle

```powershell

# Get all scripts installed using PowerShellGet cmdlets
Get-InstalledScript

# Get a specific installed script
Get-InstalledScript Show-Tree

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.0.0      Show-Tree                           PSGallery            Script to show the layout of PowerShell namespaces (Tr...

# Get installed script with wildcards
Get-InstalledScript -Name *Azure*

# Get all versions of an installed script
Get-InstalledScript -Name Connect-O365 -AllVersions

# Get installed script with MinimumVersion
Get-InstalledScript -Name Connect-O365 -MinimumVersion 1.1

# Get installed script with MaximumVersion
Get-InstalledScript -Name Connect-O365 -MaximumVersion 1.6.3

# Get installed script with version range
Get-InstalledScript -Name Connect-O365 -MinimumVersion 1.1 -MaximumVersion 1.6.3

# Get installed script with RequiredVersion
Get-InstalledScript -Name Connect-O365 -RequiredVersion 1.4

# Properties of Get-InstalledScript returned object
Get-InstalledScript Show-Tree | Format-List * -Force

Name                       : Show-Tree
Version                    : 1.0.0
Type                       : Script
Description                : Script to show the layout of PowerShell namespaces (Trees) using ASCII
Author                     : Jeffrey Snover
CompanyName                : jsnover
Copyright                  : (C) Microsoft Corporation. All rights reserved.
PublishedDate              : 2/15/2016 10:15:35 PM
InstalledDate              : 5/4/2016 11:44:13 PM
UpdatedDate                :
LicenseUri                 :
ProjectUri                 :
IconUri                    :
Tags                       : {Nano, PSScript}
Includes                   : {Function, RoleCapability, Command, DscResource...}
PowerShellGetFormatVersion :
ReleaseNotes               :
Dependencies               : {}
RepositorySourceLocation   : https://www.powershellgallery.com/api/v2/
Repository                 : PSGallery
PackageManagementProvider  : NuGet
AdditionalMetadata         : {description, installeddate, tags, PackageManagementProvider...}
InstalledLocation          : C:\Program Files\WindowsPowerShell\Scripts


```

<!--HONumber=Oct16_HO1-->


