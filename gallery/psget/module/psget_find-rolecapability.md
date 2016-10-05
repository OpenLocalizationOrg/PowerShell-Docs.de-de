# Suchen RoleCapability

Sucht nach Rolle Funktionen in Modulen.

## Beschreibung
Das Cmdlet suchen RoleCapability findet PowerShell Rolle Funktionen in Modulen. Suchen nach RoleCapability sucht Module registrierten Repositorys. Für jede Rolle-Funktion, die mit diesem Cmdlet findet, wird ein PSGetRoleCapabilityInfo-Objekt zurückgegeben. Sie können ein PSGetRoleCapabilityInfo-Objekt übergeben, an das Cmdlet "Install-Module" Installieren des Moduls, das die Funktion enthält.
PowerShell Rolle Funktionen definieren, welche Befehle, Programme usw. sind für einen Benutzer an einem Endpunkt gerade genug Administration (JEA) verfügbar. Rolle Funktionen werden von Dateien mit der Erweiterung .psrc definiert.

- Suchen-RoleCapability kann mit Version-Parameter filtern: MinimumVersion, RequiredVersion, AllVersions.
  - Diese Parameter schließen sich gegenseitig aus.
  - Diese Versionsparameter dürfen nur mit den Namen der einzelnen Modul ohne Platzhalter.
  - Wenn der RequiredVersion-Parameter nicht angegeben wird, gibt suchen RoleCapability die neueste Version des Moduls, der gleich oder größer als die angegebene minimale Version oder die neueste Version des Moduls ist, wenn keine Mindestversion angegeben wird.
  - Wenn der RequiredVersion-Parameter angegeben ist, gibt suchen RoleCapability nur die Version des Moduls, das genau mit die angegebene Version übereinstimmt.
- Suchen-RoleCapability Filtern nach Metadaten des Moduls mit dem Tag-Parameter
- Repository-spezifischen Suchsprache suchen RoleCapability filtern kann mit dem Filter-Parameter.
- Suchen-RoleCapability kann Module aus allen oder nur wenige der registrierten Repositorys filtern.

## Cmdlet-syntax
```powershell
Get-Command -Name Find-RoleCapability -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Suchen RoleCapability](http://go.microsoft.com/fwlink/?LinkId=718029)

## Beispiele für Befehle
```powershell

# Find a specific role capability
Find-RoleCapability -Name Maintenance

Name                                Version    ModuleName                          Repository
----                                -------    ----------                          ----------
Maintenance                         1.5.0      RoleCapModule                       PrivatePSGallery

# Find multiple role capabilities
Find-RoleCapability -Name MyJeaRole, Maintenance

# Find all available role capabilities from all registered repositories
Find-RoleCapability

# Find available role capabilities from few registered repositories
Find-RoleCapability -Repository PSGallery,PrivatePSGallery

# Find all role capabilities in a specified repository
Find-RoleCapability -Repository PSGallery

# Find a role capability defined in a specific module
Find-RoleCapability -Name Maintenance -ModuleName RoleCapModule

# Find role capabilities from all versions of a module
Find-RoleCapability -ModuleName RoleCapModule -AllVersions

# Find role capabilities with module name and MinimumVersion.
Find-RoleCapability -ModuleName RoleCapModule -MinimumVersion 1.1

# Find role capabilities with module name and exact version
Find-RoleCapability -ModuleName RoleCapModule -RequiredVersion 1.4.0

# Find role capabilities defined modules with -Filter based search. -Filter searches in description and module names
Find-RoleCapability -Filter Cookbook
Find-RoleCapability -Filter RBAC

# Find all role capabilities with tags Azure or DSC in module metadata
Find-RoleCapability -Tag Azure, DSC

```

<!--HONumber=Oct16_HO1-->


