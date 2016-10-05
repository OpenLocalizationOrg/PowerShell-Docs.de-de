# Get-InstalledModule

Ruft die Module auf einem Computer installiert.

## Beschreibung

Das Cmdlet "Get-InstalledModule" Ruft auf einem Computer installierten PowerShell-Module, die mithilfe von Install-Module-Cmdlet installiert wurden.

Für jedes installierte Modul gibt Get-InstalledModule ein PSRepositoryItemInfo-Objekt, das optional weitergeleitet werden kann, deinstallieren-Modul für die Deinstallation der installierten Module zurück.

- Get-InstalledModule kann installierten Module, die basierend auf den Namen, die Versionsparameter gefiltert werden.
- Get-InstalledModule kann mit Version-Parameter filtern: MinimumVersion, MaximumVersion, RequiredVersion, AllVersions.
  - Diese Parameter verwendet werden, mit Ausnahme von MinmimumVersion und MaximumVersion.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Modul ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt Get-InstalledModule die neueste Version des installierten Moduls, der gleich oder größer als die angegebene minimale Version oder die neueste Version des Moduls ist, wenn keine Mindestversion angegeben wird. 
  - Wenn der RequiredVersion-Parameter angegeben wird, gibt Get-InstalledModule nur die Version der installierten Module, die mit die angegebene Version übereinstimmt.

## Cmdlet-syntax
```powershell
Get-Command -Name Get-InstalledModule -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Get-InstalledModule](http://go.microsoft.com/fwlink/?LinkId=526863)

## Beispiele für Befehle

```powershell

# Get all modules installed using PowerShellGet cmdlets
Get-InstalledModule

# Get a specific installed module
Get-InstalledModule DJoin

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.0        DJoin                               PSGallery            This is a PowerShell frontend to the DJOIN.exe c...

# Get installed module with wildcards
Get-InstalledModule -Name AzureRM*

# Get all versions of an installed module
Get-InstalledModule -Name AzureRM.Automation -AllVersions

# Get installed module with MinimumVersion
Get-InstalledModule -Name AzureRM.Automation -MinimumVersion 1.0.0

# Get installed module with MaximumVersion
Get-InstalledModule -Name AzureRM.Automation -MaximumVersion 1.0.8

# Get installed module with version range
Get-InstalledModule -Name AzureRM.Automation -MinimumVersion 1.0.0 -MaximumVersion 1.0.8

# Get installed module with RequiredVersion
Get-InstalledScript -Name AzureRM.Automation -RequiredVersion 1.0.3

# Properties of Get-InstalledModule returned object
Get-InstalledModule DJoin | Format-List * -Force

Name                       : DJoin
Version                    : 1.0
Type                       : Module
Description                : This is a PowerShell frontend to the DJOIN.exe command which provides better
                             discoverability and usability.
Author                     : Jeffrey Snover
CompanyName                : jsnover
Copyright                  : (C) Microsoft Corporation. All rights reserved.
PublishedDate              : 2/15/2016 7:12:37 PM
InstalledDate              : 4/5/2016 4:13:39 PM
UpdatedDate                :
LicenseUri                 :
ProjectUri                 :
IconUri                    :
Tags                       : {Nano, PSModule}
Includes                   : {Function, RoleCapability, Command, DscResource...}
PowerShellGetFormatVersion :
ReleaseNotes               :
Dependencies               : {}
RepositorySourceLocation   : https://www.powershellgallery.com/api/v2/
Repository                 : PSGallery
PackageManagementProvider  : NuGet
AdditionalMetadata         : {description, installeddate, tags, PackageManagementProvider...}
InstalledLocation          : C:\Program Files\WindowsPowerShell\Modules\DJoin\1.0

```



## InstalledDate und UpdatedDate Eigenschaften im PSGetRepositoryItemInfo-Objekt

    During the install operation:
        InstalledDate: current DateTime (Get-Date) value
        UpdatedDate: null

    During the Update operation:
        InstalledDate: InstalledDate from the previous installation otherwise current DateTime (Get-Date) value
        UpdatedDate: current DateTime (Get-Date) value

```powershell
Install-Module -Name ContosoServer -RequiredVersion 1.0 -Repository INT
Get-InstalledModule -Name ContosoServer | Format-Table Name, InstalledDate, UpdatedDate

Name          InstalledDate         UpdatedDate
----          -------------         -----------
ContosoServer 2/29/2016 11:59:14 AM


\Update-Module -Name ContosoServer
Get-InstalledModule -Name ContosoServer | Format-Table Name, InstalledDate, UpdatedDate

Name          InstalledDate         UpdatedDate
----          -------------         -----------
ContosoServer 2/29/2016 11:59:14 AM 2/29/2016 12:00:15 PM
```

<!--HONumber=Oct16_HO1-->


