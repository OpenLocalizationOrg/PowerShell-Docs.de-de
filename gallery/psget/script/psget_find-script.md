# Find-Script

Sucht die PowerShell-Skriptdateien, aus einer online-Katalog, die bestimmte Kriterien erfüllen.

## Beschreibung

Suchen-Skript ermittelt die Skriptdateien aus registrierten Repositorys, die den angegebenen Kriterien entspricht.
Für jedes Skript gefunden wird gibt suchen-Skript ein PSRepositoryItemInfo-Objekt, das optional geleitet werden kann, Skript für die Installation für die Installation der Skripts.
Das Cmdlet „Find-Script“ ermöglicht das Ermitteln der Skriptdateien mithilfe verschiedener Suchkriterien wie Name, Tag, Filter, Befehlsname, Versionsbereich, genaue Version, alle Versionen einschließlich Abhängigkeiten und Ursprung aus einem bestimmten oder allen registrierten Repositorys.

- Suchen-Skript kann Filter basierend auf Skript den Inhalt durch den - Befehl und - Parameter enthält.
- Suchen-Skript kann mit Version-Parameter filtern: MinimumVersion, MaximumVersion, RequiredVersion, AllVersions.
  - Diese Parameter verwendet werden, mit Ausnahme von MinmimumVersion und MaximumVersion.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Skripts ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt suchen-Skript die neueste Version des Skripts, die gleich oder größer als die angegebene minimale Version oder die neueste Version des Skripts ist, wenn keine Mindestversion angegeben wird. 
  - Wenn der RequiredVersion-Parameter angegeben ist, gibt suchen-Skript nur die Version des Skripts, die mit die angegebene Version übereinstimmt.
- Suchen-Skript auf Skript Metadaten filtern kann mit dem Tag-Parameter.
- Suchen-Skript auf Repository-spezifischen Suchsprache filtern kann mit dem Filter-Parameter.
- Suchen-Skripts kann Skripts aus allen oder nur wenige der registrierten Repositorys filtern.

**Hinweis:** registrierten PSRepository müssen eine gültige ScriptSourceLocation. Die Set-PSRepository können ScriptSourceLocation Wert festgelegt.

## Cmdlet-syntax

```powershell
Get-Command -Name Find-Script -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Suchen-Skript](http://go.microsoft.com/fwlink/?LinkId=619785)

## Beispiele für Befehle

```powershell
# Find a script from the registered repository with ScriptSourceLocation
Find-Script Connect-AzureVM

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.0        Connect-AzureVM                     PSGallery            This runbook sets up a connection to an Azure vi...

# Find multiple scripts
Find-Script -Name Connect-AzureVM, Show-Tree, Connect-O365

# Find scripts with wildcards in -Name
Find-Script -Name *Azure*

# Find all versions of a script
Find-Script -Name Connect-O365 -AllVersions

# Find a script with -MinimumVersion. 
# With MinimumVersion we can find a script whose version is greate than or equal to the specified MinimumVersion value.
Find-Script Connect-O365 -MinimumVersion 1.4

# Find a script with MaximumVersion
Find-Script -Name Connect-O365 -MaximumVersion 1.6.2

# Find a script with both MinimumVersion and MaximumVersion range.
Find-Script -Name Connect-O365 -MinimumVersion 1.1 -MaximumVersion 1.6.2

# Find a script with exact version
Find-Script -Name Connect-O365 -RequiredVersion 1.5.7

# Find a script from the specified repository
Find-Script -Name Fabrikam-ServerScript -Repository MyLocalRepo

# Find available scripts from all registered repositories
Find-Script

# Find available scripts from few registered repositories
Find-Script -Repository PSGallery, PrivatePSGallery

# Find a script along with its dependent modules and scripts
Find-Script -Name Connect-AzureVM -IncludeDependencies

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.0        Connect-AzureVM                     PSGallery            This runbook sets up a connection to an Azure vi...
1.4.0      Azure                               PSGallery            Microsoft Azure PowerShell - Service Management
1.1.2      Azure.Storage                       PSGallery            Microsoft Azure PowerShell - Storage service cmd...
1.0.8      AzureRM.profile                     PSGallery            Microsoft Azure PowerShell - Profile credential ...

# Find all scripts with workflows
Find-Script -Includes Workflow

# Find all scripts with functions
Find-Script -Includes Function

# Find scripts with specific commands
Find-Script -Command Log-Message
Find-Script -Command Log-Message, Show-Tree -Includes Function
Find-Script -Command Connect-AzureVM -Includes Workflow

# Find scripts with -Filter based search. -Filter searches in description and names
Find-Script -Filter Windows
Find-Script -Filter Azure

# Find all scripts with tags O365 or Nano
Find-Script -Tag O365, Nano

# Properties of Find-Script returned object
Find-Script Show-Tree | Format-List * -Force
Name                       : Show-Tree
Version                    : 1.0.0
Type                       : Script
Description                : Script to show the layout of PowerShell namespaces (Trees) using ASCII
Author                     : Jeffrey Snover
CompanyName                : jsnover
Copyright                  : (C) Microsoft Corporation. All rights reserved.
PublishedDate              : 2/15/2016 10:15:35 PM
InstalledDate              :
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
AdditionalMetadata         : {versionDownloadCount, ItemType, copyright, PackageManagementProvider...}


# Includes property on PSRepositoryItemInfo object
$t = Find-Script -Includes Workflow -Repository INT -Name Fabrikam-ClientScript
$t.Includes

Name                           Value
----                           -----
Function                       {Test-FunctionFromScript_Fabrikam-ClientScript}
RoleCapability                 {}
Command                        {Test-FunctionFromScript_Fabrikam-ClientScript, Test-WorkflowFromScript_Fabrikam-Clie...
DscResource                    {}
Workflow                       {Test-WorkflowFromScript_Fabrikam-ClientScript}
Cmdlet                         {}


```


<!--HONumber=Oct16_HO1-->


