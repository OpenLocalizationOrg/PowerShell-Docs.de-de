# Erstellen eines JEA-Endpunkts und Herstellen einer Verbindung damit
Um einen JEA-Endpunkt zu erstellen, müssen Sie eine besonders konfigurierte PowerShell-Sitzungskonfigurationsdatei erstellen und registrieren, die mit dem Cmdlet **New-PSSessionConfigurationFile** generiert werden kann.

```powershell
New-PSSessionConfigurationFile -SessionType RestrictedRemoteServer -TranscriptDirectory "C:\ProgramData\JEATranscripts" -RunAsVirtualAccount -RoleDefinitions @{ 'CONTOSO\NonAdmin_Operators' = @{ RoleCapabilities = 'Maintenance' }} -Path "$env:ProgramData\JEAConfiguration\Demo.pssc" 
```

Diese Sitzungskonfigurationsdatei sieht folgendermaßen aus: 
```powershell
@{

# Version number of the schema used for this document
SchemaVersion = '2.0.0.0'

# ID used to uniquely identify this document
GUID = 'a384fdd3-5830-4a2c-ac86-cdd1822c3afd'

# Author of this document
Author = 'Administrator'

# Description of the functionality provided by these settings
# Description = ''

# Session type defaults to apply for this session configuration. Can be 'RestrictedRemoteServer' (recommended), 'Empty', or 'Default'
SessionType = 'RestrictedRemoteServer'

# Directory to place session transcripts for this session configuration
TranscriptDirectory = 'C:\ProgramData\JEATranscripts'

# Whether to run this session configuration as the machine's (virtual) administrator account
RunAsVirtualAccount = $true

# Groups associated with machine's (virtual) administrator account
# RunAsVirtualAccountGroups = 'Remote Desktop Users', 'Remote Management Users'

# Scripts to run when applied to a session
# ScriptsToProcess = 'C:\ConfigData\InitScript1.ps1', 'C:\ConfigData\InitScript2.ps1'

# User roles (security groups), and the Role Capabilities that should be applied to them when applied to a session
RoleDefinitions = @{
    'CONTOSO\NonAdmin_Operators' = @{
        'RoleCapabilities' = 'Maintenance' } }

} 
```
Beim Erstellen eines JEA-Endpunkts müssen die folgenden Parameter des Befehls (und entsprechenden Schlüssel in der Datei) festgelegt werden:
1.  „SessionType“ auf „RestrictedRemoteServer“.
2.  „RunAsVirtualAccount“ auf **$true**.
3.  „TranscriptPath“ auf das Verzeichnis, in dem „Über die Schulter“-Aufzeichnungen nach jeder Sitzung gespeichert werden.
4.  „RoleDefinitions“ auf eine Hashtabelle, die definiert, welche Gruppen Zugriff auf bestimmte „Rollenfunktionen“ haben.  Dieses Feld bestimmt, **wer** auf diesen Endpunkt **was** anwenden darf.   „Rollenfunktionen“ sind spezielle Dateien, die in Kürze erläutert werden.


Das Feld „RoleDefinitions“ definiert, welche Gruppen Zugriff auf bestimmte Rollenfunktionen haben.  Eine Rollenfunktion ist eine Datei, die verschiedene Funktionen definiert, die Benutzern, die eine Verbindung herstellen, verfügbar gemacht werden.  Sie können Rollenfunktionen mit dem Befehl **New-PSRoleCapabilityFile** erstellen.

```powershell
New-PSRoleCapabilityFile -Path "$env:ProgramFiles\WindowsPowerShell\Modules\DemoModule\RoleCapabilities\Maintenance.psrc" 
```

Dadurch wird eine Rollenfunktionsvorlage erstellt, die wie folgt aussieht:
```
@{

# ID used to uniquely identify this document
GUID = '9287a34f-3f0e-4fbe-9dd7-f1361ba9fd65'

# Author of this document
Author = 'Administrator'

# Description of the functionality provided by these settings
# Description = ''

# Company associated with this document
CompanyName = 'Unknown'

# Copyright statement for this document
Copyright = '(c) 2015 Administrator. All rights reserved.'

# Modules to import when applied to a session
# ModulesToImport = 'MyCustomModule', @{ ModuleName = 'MyCustomModule'; ModuleVersion = '1.0.0.0'; GUID = '4d30d5f0-cb16-4898-812d-f20a6c596bdf' }

# Aliases to make visible when applied to a session
# VisibleAliases = 'Item1', 'Item2'

# Cmdlets to make visible when applied to a session
# VisibleCmdlets = 'Invoke-Cmdlet1', @{ Name = 'Invoke-Cmdlet2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }

# Functions to make visible when applied to a session
# VisibleFunctions = 'Invoke-Function1', @{ Name = 'Invoke-Function2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }

# External commands (scripts and applications) to make visible when applied to a session
# VisibleExternalCommands = 'Item1', 'Item2'

# Providers to make visible when applied to a session
# VisibleProviders = 'Item1', 'Item2'

# Scripts to run when applied to a session
# ScriptsToProcess = 'C:\ConfigData\InitScript1.ps1', 'C:\ConfigData\InitScript2.ps1'

# Aliases to be defined when applied to a session
# AliasDefinitions = @{ Name = 'Alias1'; Value = 'Invoke-Alias1'}, @{ Name = 'Alias2'; Value = 'Invoke-Alias2'}

# Functions to define when applied to a session
# FunctionDefinitions = @{ Name = 'MyFunction'; ScriptBlock = { param($MyInput) $MyInput } }

# Variables to define when applied to a session
# VariableDefinitions = @{ Name = 'Variable1'; Value = { 'Dynamic' + 'InitialValue' } }, @{ Name = 'Variable2'; Value = 'StaticInitialValue' }

# Environment variables to define when applied to a session
# EnvironmentVariables = @{ Variable1 = 'Value1'; Variable2 = 'Value2' }

# Type files (.ps1xml) to load when applied to a session
# TypesToProcess = 'C:\ConfigData\MyTypes.ps1xml', 'C:\ConfigData\OtherTypes.ps1xml'

# Format files (.ps1xml) to load when applied to a session
# FormatsToProcess = 'C:\ConfigData\MyFormats.ps1xml', 'C:\ConfigData\OtherFormats.ps1xml'

# Assemblies to load when applied to a session
# AssembliesToLoad = 'System.Web', 'System.OtherAssembly, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

} 

```
Um in einer JEA-Sitzungskonfiguration verwendet zu werden, müssen Rollenfunktionen als gültige PowerShell-Module im Verzeichnis „RoleCapabilities“ gespeichert werden. Ein Modul kann, falls gewünscht, mehrere Rollenfunktionsdateien enthalten.

Zum Starten der Festlegung, auf welche Cmdlets, Funktionen, Aliase und Skripts ein Benutzer zugreifen darf, wenn eine Verbindung mit einer JEA-Sitzung hergestellt wird, fügen Sie der Datei mit den Rollenfunktionen hinter den auskommentierten Vorlagen eigene Regeln hinzu. Um sich einen tieferen Einblick in die Konfiguration von Rollenfunktionen zu verschaffen, konsultieren Sie das vollständige [Just Enough Administration-Handbuch](http://aka.ms/JEA).

Nachdem Sie die Anpassung Ihrer Sitzungskonfiguration und dazugehörigen Rollenfunktionen abgeschlossen haben, registrieren Sie diese Sitzungskonfiguration. Erstellen Sie den Endpunkt durch Ausführen von **Register-PSSessionConfiguration**.

```powershell
Register-PSSessionConfiguration -Name Maintenance -Path "C:\ProgramData\JEAConfiguration\Demo.pssc" 
```

## Herstellen einer Verbindung mit einem JEA-Endpunkt
Das Herstellen einer Verbindung mit einem JEA-Endpunkt erfolgt wie das Verbinden mit einem beliebigen anderen PowerShell-Endpunkt.  Sie müssen einfach den Namen Ihres JEA-Endpunkts als „ConfigurationName“-Parameter für **New-PSSession**, **Invoke-Command** oder **Enter-PSSession** eingeben.

```powershell
Enter-PSSession -ConfigurationName Maintenance -ComputerName localhost
```
Sobald Sie mit der JEA-Sitzung verbunden sind, können Sie nur noch die Befehle ausführen, die in der Positivliste unter den Rollenfunktionen angegeben sind. Wenn Sie versuchen, einen für Ihre Rolle nicht zulässigen Befehl auszuführen, tritt ein Fehler auf.

<!--HONumber=Oct16_HO1-->


