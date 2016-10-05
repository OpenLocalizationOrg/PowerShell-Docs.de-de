# Install-Module

Installiert die PowerShell-Module von online-Repositorys auf dem lokalen Computer.

## Beschreibung

Install-Module-Cmdlet ein oder mehrere Module aus einer online-Katalog heruntergeladen, überprüft und installiert sie auf dem lokalen Computer auf den Bereich der angegebenen Installation.

Das Cmdlet "Install-Module" Ruft ein oder mehrere Module, die aus einem Onlinekatalog angegebene Kriterien erfüllen, überprüft, ob Suchergebnisse gültige Module und Kopien Modul Ordner an den Installationsort sind.

Wenn kein Bereich definiert ist oder wenn der Wert des Bereichsparameters AllUsers ist, wird das Modul %systemdrive%:\Program Files\WindowsPowerShell\Modules installiert. Wenn der Wert des Bereichs CurrentUser ist, wird das Modul $Home\Documents\WindowsPowerShell\Modules installiert.

Sie können die Ergebnisse auf der Basis Mindest- und genaue Versionen der angegebenen Module filtern.

- Unterstützung der gleichzeitigen Ausführung unterschiedlicher Versionen für Windows PowerShell 5.0 oder höher
- Unterstützung der Installation von Modulabhängigkeiten
- **Nicht vertrauenswürdige auffordern:**Benutzerakzeptanz ist erforderlich für die Installation der Module von einem nicht vertrauenswürdigen Repository aus.
- -Force neu installierte Modul installiert.
- RequiredVersion wird die angegebene Version in SxS mit vorhandenen Versionen in PowerShell, Version 5.0 oder höher installiert.

### Bereich
Gibt den Bereich der Installation des Moduls. Die zulässigen Werte für diesen Parameter sind: AllUsers und CurrentUser.

Der Standardbereich für die Installation ist AllUsers.

Die AllUsers-Bereich kann Module, die an einem Speicherort installiert werden, die für alle Benutzer des Computers zugänglich ist "$env: SystemDrive\Program Files\WindowsPowerShell\Modules".

Bereich CurrentUser kann Module, die nur auf "$Home\Documents\WindowsPowerShell\Modules", installiert werden, damit das Modul nur für den aktuellen Benutzer verfügbar ist.

## Hinweise

Mit diesem Cmdlet wird auf Windows PowerShell 3.0 oder spätere Versionen von Windows PowerShell unter Windows 7 oder Windows 2008 R2 und höheren Versionen von Windows ausgeführt werden.

Wenn ein installiertes Modul (d. h., verfügt es kein psm1, psd1 oder DLL-Datei mit dem gleichen Namen innerhalb des Ordners) importiert werden kann, schlägt die Installation fehl, wenn Sie den Force-Parameter auf den Befehl hinzufügen.

Wenn eine Version des Moduls auf dem Computer für den Name-Parameter angegebenen Wert entspricht, und Sie nicht den Parameter MinimumVersion oder RequiredVersion hinzugefügt haben, weiterhin installieren-Module im Hintergrund ohne dieses Modul installieren. Wenn das vorhandene Modul nicht der Werte in diesen Parameter entspricht MinimumVersion oder RequiredVersion Parameter angegeben werden, tritt ein Fehler auf. Genauer: Wenn die Version des installierten Moduls kleiner als der Wert des Parameters MinimumVersion oder nicht gleich dem Wert des Parameters RequiredVersion ist, wird ein Fehler auftritt. Ist die Version des installierten Modul größer als der Wert des Parameters MinimumVersion oder gleich dem Wert des Parameters RequiredVersion, weiterhin installieren-Module im Hintergrund ohne dieses Modul wird installiert.

Install-Modul gibt einen Fehler zurück, wenn kein Modul im Onlinekatalog vorhanden ist, die mit den angegebenen Namen übereinstimmt.

Um mehrere Module zu installieren, geben Sie ein Array von den Modulnamen müssen durch Kommas getrennt. Sie können MinimumVersion oder RequiredVersion hinzufügen, wenn Sie mehrere Namen angeben.

Standardmäßig werden die Module in den Ordner "Programme", um Verwechslungen zu vermeiden, bei der Installation von Windows PowerShell Desired State Configuration (DSC) Ressourcen installiert. Sie können mehrere PSGetItemInfo Objekte auf Install-Modul übergeben; Dies ist eine andere Möglichkeit, mehrere Module anzugeben, in einen einzelnen Befehl zu installieren.

Zum Vermeiden von ausgeführten Module, die installierten böswilligen Code enthalten sind Module nicht automatisch durch Installation importiert. Aus Sicherheitsgründen empfiehlt es sich, Modulcode vor dem Ausführen alle Cmdlets und Funktionen in einem Modul zum ersten Mal ausgewertet.


## Cmdlet-syntax
```powershell
Get-Command -Name Install-Module -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Install-Modul](http://go.microsoft.com/fwlink/?LinkID=398573)

## Beispiele für Befehle

```powershell

# Install a module by name
Install-Module -Name MyDscModule

# Install multiple modules
Install-Module ContosoClient,ContosoServer

# Install a module using its minimum version
Install-Module -Name ContosoServer -MinimumVersion 1.0

# Install a specific version of a module
Install-Module -Name ContosoServer -RequiredVersion 1.1.3

# Install the latest version of a module to $home\Documents\WindowsPowerShell\Modules.
Install-Module -Name ContosoServer -Scope CurrentUser

# if a module is already available under $env:PSModulePath, below command fails with 'ModuleAlreadyInstalled,Install-Package,Microsoft.PowerShell.PackageManagement.Cmdlets.InstallPackage'
Install-Module ContosoServer -RequiredVersion 1.5

# if a module is already available under $env:PSModulePath, below command fails with 'ModuleAlreadyInstalled,Install-Package,Microsoft.PowerShell.PackageManagement.Cmdlets.InstallPackage'
Install-Module ContosoServer -MinimumVersion 2.5

# Install multiple modules from multiple registered repositories
Install-Module ContosoClient,ContosoServer -Repository PSGallery, PrivatePSGallery

# Install a module with -WhatIf
Install-Module ContosoClient -WhatIf

# Install a module with -Confirm. A prompt will be displayed to confirm the installation.
Install-Module ContosoClient -WhatIf

# -Force option reinstalls the installed module
Install-Module ContosoClient -Force

# Install a module with dependencies
Install-Module -Name 
```

## Install-Module-Cmdlet in der Pipeline-Vorgänge

```powershell

# Find a module and install it
Find-Module -Name "MyDSC*" | Install-Module

# Find a module and install it to the CurrentUser scope
Find-Module -Name "MyDSC*" | Install-Module -Scope CurrentUser

# Find commands by name and install them
# The first command finds the specified commands in the INT repository, and then uses the pipeline operator to pass them to Install-Module to install them.
# The second command uses Get-InstalledModule to verify the modules from the prior command are installed.
Find-Command -Repository "INT" -Name Get-ContosoClient,Get-ContosoServer | Install-Module
Get-InstalledModule

# This command finds the resource named MyResource and passes it to the Install-Module cmdlet by using the pipeline operator. The Install-Module cmdlet installs the module for the resource. 
# If you pipe multiple resources to the Install-Module cmdlet from the same module, Install-Module attempts to install the module only once. 
Find-DscResource -Name "MyResource" | Install-Module
Get-InstalledModule

# Find multiple role capabilities and install them
Find-RoleCapability -Name MyJeaRole, Maintenance | Install-Module
Get-InstalledModule

```

## Unterstützung der gleichzeitigen Ausführung unterschiedlicher Versionen für PowerShell 5.0 oder höher

PowerShellGet unterstützt die Side-by-Side (SxS)-Modul versionsunterstützung in Install-Module, Update-Module und Publish-Module-Cmdlets, die in Windows PowerShell 5.0 oder höher ausgeführt.

### Beispiele für die Install-Modul

```powershell
# Install a version of the module
Install-Module -Name PSScriptAnalyzer -RequiredVersion 1.1.0 -Repository PSGallery
Get-Module -ListAvailable -Name PSScriptAnalyzer | Format-List Name,Version,ModuleBase

Name : PSScriptAnalyzer
Version : 1.1.0
ModuleBase : C:\Program Files\WindowsPowerShell\Modules\PSScriptAnalyzer\1.1.0

# Install another version of the module in Side-by-Side with already installed version.
Install-Module -Name PSScriptAnalyzer -RequiredVersion 1.1.1 -Repository PSGallery
Get-Module -ListAvailable -Name PSScriptAnalyzer | Format-List Name,Version,ModuleBase

Name       : PSScriptAnalyzer 
Version    : 1.1.1
ModuleBase : C:\Program Files\WindowsPowerShell\Modules\PSScriptAnalyzer\1.1.1
Name       : PSScriptAnalyzer
Version    : 1.1.0
ModuleBase : C:\Program Files\WindowsPowerShell\Modules\PSScriptAnalyzer\1.1.0

# Get all versions of an installed module
Get-InstalledModule -Name PSScriptAnalyzer -AllVersions
Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.1.0      PSScriptAnalyzer                    PSGallery            PSScriptAnalyzer provides script analysis... 
1.1.1      PSScriptAnalyzer                    PSGallery            PSScriptAnalyzer provides script analysis...


```

## Installieren des Moduls mit Abhängigkeiten

```powershell

# Find a module
Find-Module -Name TypePx -Repository PSGallery

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
2.0.1.20   TypePx                              PSGallery            The TypePx module adds properties and methods to the m...

# Find a module and its dependencies
Find-Module -Name TypePx -Repository PSGallery -IncludeDependencies

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
2.0.1.20   TypePx                              PSGallery            The TypePx module adds properties and methods to the m...
1.0.5.18   SnippetPx                           PSGallery            The SnippetPx module enhances the snippet experience i...

# Discover the dependencies list without adding -IncludeDependencies
$result = Find-Module -Name TypePx -Repository PSGallery
$result.Dependencies

Name                           Value
----                           -----
Name                           SnippetPx
CanonicalId                    powershellget:SnippetPx/#https://www.powershellgallery.com/api/v2/


# Now install the module along with its dependencies
Install-Module -Name TypePx -Repository PSGallery -Verbose

VERBOSE: Repository details, Name = 'PSGallery', Location = 'https://www.powershellgallery.com/api/v2/'; IsTrusted =
'False'; IsRegistered = 'True'.
VERBOSE: Using the provider 'PowerShellGet' for searching packages.
VERBOSE: Using the specified source names : 'PSGallery'.
VERBOSE: Getting the provider object for the PackageManagement Provider 'NuGet'.
VERBOSE: The specified Location is 'https://www.powershellgallery.com/api/v2/' and PackageManagementProvider is
'NuGet'.
VERBOSE: Searching repository 'https://www.powershellgallery.com/api/v2/FindPackagesById()?id='TypePx'' for ''.
VERBOSE: Total package yield:'1' for the specified package 'TypePx'.
VERBOSE: Performing the operation "Install-Module" on target "Version '2.0.1.20' of module 'TypePx'".

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
VERBOSE: The installation scope is specified to be 'AllUsers'.
VERBOSE: The specified module will be installed in 'C:\Program Files\WindowsPowerShell\Modules'.
VERBOSE: The specified Location is 'NuGet' and PackageManagementProvider is 'NuGet'.
VERBOSE: Downloading module 'TypePx' with version '2.0.1.20' from the repository
'https://www.powershellgallery.com/api/v2/'.
VERBOSE: Searching repository 'https://www.powershellgallery.com/api/v2/FindPackagesById()?id='TypePx'' for ''.
VERBOSE: Searching repository 'https://www.powershellgallery.com/api/v2/FindPackagesById()?id='SnippetPx'' for ''.
VERBOSE: InstallPackage' - name='SnippetPx',
version='1.0.5.18',destination='C:\Users\manikb\AppData\Local\Temp\1027042896'
VERBOSE: DownloadPackage' - name='SnippetPx',
version='1.0.5.18',destination='C:\Users\manikb\AppData\Local\Temp\1027042896\SnippetPx\SnippetPx.nupkg',
uri='https://www.powershellgallery.com/api/v2/package/SnippetPx/1.0.5.18'
VERBOSE: Downloading 'https://www.powershellgallery.com/api/v2/package/SnippetPx/1.0.5.18'.
VERBOSE: Completed downloading 'https://www.powershellgallery.com/api/v2/package/SnippetPx/1.0.5.18'.
VERBOSE: Completed downloading 'SnippetPx'.
VERBOSE: Hash for package 'SnippetPx' does not match hash provided from the server.
VERBOSE: InstallPackageLocal' - name='SnippetPx',
version='1.0.5.18',destination='C:\Users\manikb\AppData\Local\Temp\1027042896'
VERBOSE: InstallPackage' - name='TypePx',
version='2.0.1.20',destination='C:\Users\manikb\AppData\Local\Temp\1027042896'
VERBOSE: DownloadPackage' - name='TypePx',
version='2.0.1.20',destination='C:\Users\manikb\AppData\Local\Temp\1027042896\TypePx\TypePx.nupkg',
uri='https://www.powershellgallery.com/api/v2/package/TypePx/2.0.1.20'
VERBOSE: Downloading 'https://www.powershellgallery.com/api/v2/package/TypePx/2.0.1.20'.
VERBOSE: Completed downloading 'https://www.powershellgallery.com/api/v2/package/TypePx/2.0.1.20'.
VERBOSE: Completed downloading 'TypePx'.
VERBOSE: Hash for package 'TypePx' does not match hash provided from the server.
VERBOSE: InstallPackageLocal' - name='TypePx',
version='2.0.1.20',destination='C:\Users\manikb\AppData\Local\Temp\1027042896'
VERBOSE: Installing the dependency module 'SnippetPx' with version '1.0.5.18' for the module 'TypePx'.
VERBOSE: Module 'SnippetPx' was installed successfully to path 'C:\Program
Files\WindowsPowerShell\Modules\SnippetPx\1.0.5.18'.
VERBOSE: Module 'TypePx' was installed successfully to path 'C:\Program
Files\WindowsPowerShell\Modules\TypePx\2.0.1.20'.


# Get the installed modules
Get-InstalledModule

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.0.5.18   SnippetPx                           PSGallery            The SnippetPx module enhances the snippet experience i...
2.0.1.20   TypePx                              PSGallery            The TypePx module adds properties and methods to the m...

```

## Fehlerszenarien

```powershell

# Below command fails with 'NameShouldNotContainWildcardCharacters,Install-Module'
Install-Module ContosoServe*

# Below command fails with 'VersionRangeAndRequiredVersionCannotBeSpecifiedTogether,Install-Module'
Install-Module ContosoServer -MinimumVersion 1.0 -RequiredVersion 5.0

# Below command fails with 'VersionParametersAreAllowedOnlyWithSingleName,Install-Module'
Install-Module ContosoClient,ContosoServer -RequiredVersion 2.0

# Below command fails with 'VersionParametersAreAllowedOnlyWithSingleName,Install-Module'
Install-Module ContosoClient,ContosoServer -MinimumVersion 2.0

```


<!--HONumber=Oct16_HO1-->


