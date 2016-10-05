# Suchen-Befehl

Sucht von PowerShell-Befehle in Modulen.

## Beschreibung
Die suchen-Command-Cmdlet findet PowerShell-Befehle wie z. B. Cmdlets, Aliasen, Funktionen und Workflows. Suchen-Befehl sucht Module registrierten Repositorys.
Für jeden Befehl, die mit diesem Cmdlet findet, wird ein PSGetCommandInfo-Objekt zurückgegeben. Sie können ein PSGetCommandInfo-Objekt an das Cmdlet Modul installieren, installieren Sie das Modul mit dem Befehl übergeben.

- Suchen-Befehl kann mit Version-Parameter filtern: MinimumVersion, RequiredVersion, AllVersions.
  - Diese Parameter schließen sich gegenseitig aus.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Modul ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt Suchen-Befehl die neueste Version des Moduls, der gleich oder größer als die angegebene minimale Version oder die neueste Version des Moduls ist, wenn keine Mindestversion angegeben wird.
  - Wenn der RequiredVersion-Parameter angegeben ist, gibt Suchen-Befehl nur die Version des Moduls, das genau mit die angegebene Version übereinstimmt.
- Suchen-Befehl Filtern nach Metadaten des Moduls mit dem Tag-Parameter
- Suchen-Befehl kann eine Repository-spezifischen Suchsprache Filterung mit dem Filter-Parameter.
- Suchen-Befehl kann Module aus allen oder nur wenige der registrierten Repositorys filtern.

## Cmdlet-syntax
```powershell
Get-Command -Name Find-Command -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Suchen-Befehl](http://go.microsoft.com/fwlink/?LinkId=733636)

## Beispiele für Befehle
```powershell

# Find a specific command
Find-Command -Name Get-ScriptAnalyzerRule

Name                                Version    ModuleName                          Repository
----                                -------    ----------                          ----------
Get-ScriptAnalyzerRule              1.5.0      PSScriptAnalyzer                    PSGallery

# Find multiple commands
Find-Command -Name Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer

# Find all available commands from all registered repositories
Find-Command

# Find available commands from few registered repositories
Find-Command -Repository PSGallery,PrivatePSGallery

# Find all commands in a specified repository
Find-Command -Repository PSGallery

# Find a command defined in a specific module
Find-Command -Name Get-ScriptAnalyzerRule -Module PSScriptAnalyzer

# Find commands from all versions of a module
Find-Command -ModuleName PSReadline -AllVersions

# Find commands with module name and MinimumVersion.
Find-Command -ModuleName PSReadline -MinimumVersion 1.1

# Find commands with module name and exact version
Find-Command -ModuleName AzureRM -RequiredVersion 1.4.0

# Find commands defined modules with -Filter based search. -Filter searches in description and module names
Find-Command -Filter Cookbook
Find-Command -Filter RBAC

# Find all commands with tags Azure or DSC in module metadata
Find-Command -Tag Azure, DSC

```

<!--HONumber=Oct16_HO1-->


