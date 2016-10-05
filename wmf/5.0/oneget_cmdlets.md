# PackageManagement-Cmdlets
Dies ist der Kern von PackageManagement zum Unterstützung der Ermittlung, Installation und Inventur von Software. Testen Sie die Cmdlets für diese Vorgänge:
-   Find-Package
-   Find-PackageProvider
-   Get-Package
-   Get-PackageProvider
-   Get-PackageSource
-   Import-PackageProvider
-   Install-Package
-   Install-PackageProvider
-   Register-PackageSource
-   Save-Package
-   Set-PackageSource
-   Uninstall-Package
-   Unregister-PackageSource

Da PackageManagement ein PowerShell-Modul ist, können Sie Folgendes tun, um PackageManagement selbst zu aktualisieren:
```powershell
PS C:\> Install-Module PackageManagement –Force
```
In diesem Fall müssen Sie die PowerShell-Sitzung erneut starten, um zur neuen Version von PackageManagement zu wechseln.

## [Suchen-Package-Cmdlet](https://technet.microsoft.com/en-us/library/dn890709.aspx)
Dieses Cmdlet ermöglicht mithilfe geladener Paketanbieter die Ermittlung von Softwarepaketen in verfügbaren Paketquellen.
```powershell
# Find all available Windows PowerShell module packages from galleries registered
# with PowerShellGet provider
Find-Package -Provider PowerShellGet -Source PSGallery

# Find a package from a provider that is not yet installed
# This will bootstrap NuGet provider and then search for jquery package using NuGet
# with <http://www.nuget.org/api/v2/> as source
Find-Package -Name jquery –Provider NuGet -Source http://www.nuget.org/api/v2/

# Find package with name and version
# Here we are assuming that the user already registered nuget.org using
# Register-PackageSource. You can specify either the provider or the source, or
# neither. For the latter, performance may be less optimal as it searches through all
# the providers and registered sources.
Find-Package -Name jquery –Provider NuGet –RequiredVersion 2.1.4 -Source nuget.org
```

## [Suchen-PackageProvider-Cmdlet](https://technet.microsoft.com/en-us/library/mt676544.aspx)
Das Cmdlet „Find-PackageProvider“ dient zum Auffinden übereinstimmender PackageManagement-Anbieter, die in mit „PowerShellGet“ registrierten Paketquellen verfügbar sind. Dies sind Paketanbieter, die für die Installation mit dem Cmdlet „Install-PackageProvider“ verfügbar sind. Standardmäßig schließt dies Module ein, die im PowerShell-Katalog mit den Tags „PackageManagement“ und „Provider“ verfügbar sind. 

„Find-PackageProvider“ findet auch übereinstimmende PackageManagement-Anbieter, die im Azure-Blobspeicher von PackageManagement verfügbar sind. Zu deren Auffinden und Installation wird der PackageManagement-Bootstrapper-Anbieter verwendet.
```powershell
#Find all available package providers in PackageManagement azure blob store as well as in PowerShellGallery.com
Find-PackageProvider

#Find all versions of a provider
Find-PackageProvider -Name "Nuget" -AllVersions

#Find a provider from a specified source
Find-PackageProvider -Name "Gistprovider" -Source "PSGallery"
```

## [Cmdlet "Get-Package"](https://technet.microsoft.com/en-us/library/dn890704.aspx)
Dieses Cmdlet gibt eine Liste aller Softwarepakete zurück, die mit PackageManagement installiert wurden.
```powershell
# Get all the packages installed by Programs provider
Get-Package –Provider Programs

# Get all the packages installed by NuGet provider at c:\test using the dynamic
# parameter destination
Get-Package –Provider NuGet -Destination c:\test
```

## [Cmdlet "Get-PackageProvider"](https://technet.microsoft.com/en-us/library/dn890703.aspx)
Paketanbieter, die geladen sind und auf dem lokalen Computer verwendet werden können, lassen sich mit diesem Cmdlet inventarisieren.
```powershell
# Get all currently loaded package providers
Get-PackageProvider

# The following cmdlet will show all the package providers available on the machine (including those that are not loaded):
Get-PackageProvider -ListAvailable
```

## [Cmdlet "Get-PackageSource"](https://technet.microsoft.com/en-us/library/dn890705.aspx)
Dieses Cmdlet ruft eine Liste der Paketquellen ab, die für einen Paketanbieter registriert sind.
```powershelll
# Get all package sources
Get-PackageSource

# Get all package sources for a specific provider
Get-PackageSource –ProviderName PowerShellGet
```

## [Import-PackageProvider-Cmdlet](https://technet.microsoft.com/en-us/library/mt676545.aspx)
Dieses Cmdlet fügt der aktuellen Sitzung PackageManagement-Paketanbieter hinzu.
```powershell
# Import a package provider from the local machine
Import-PackageProvider –Name MyProvider

#The -Name parameter can be either the name of the provider or the full path to the provider. Currently, we support .dll, .exe and.psm1 for the full path case. If the name of the provider is used for the -Name parameter, then additional version parameters such as -RequiredVersion, -MinimumVersion and -MaximumVersion may be specified. Otherwise, the latest version of the provider will be imported.

#If a package provider is not yet loaded to your system, we can discover and install on-demand. You can use explicit discovery and install cmdlets to do so:
 Find-PackageProvider
 Install-PackageProvider –Name MyProvider

#After installed, follow the Import-PackageProvider to load it to your system.

# Import a specific version of a package provider. PackageManagement supports installations of multiple versions of a package provider using PackageProvider cmdlets (not by bootstrapper provider). You can install another version of a package provider given that you already have one up running by:
Find-PackageProvider –Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force
Get-PackageProvider –ListAvailable
Import-PackageProvider –Name "Nuget" -RequiredVersion "2.8.5.201" -Verbose
Import-PackageProvider –Name MyProvider –RequiredVersion xxxx -force
```

##[Cmdlet „Install-Package“](https://technet.microsoft.com/en-us/library/dn890711.aspx)

Dieses Cmdlet ermöglicht mithilfe geladener Paketanbieter die Installation von Softwarepaketen in verfügbaren Paketquellen.
```powershell
# Install a package by name.
# NuGet provider requires us to provide the dynamic parameter destination path
# when we use this provider to install. Not all providers will require you to supply
# dynamic parameters for PackageManagement cmdlets.
Install-Package -Name jquery -Source nuget.org -Destination c:\test

# Install a package by piping.
Find-Package -Name jquery –Provider NuGet | Install-Package -Destination c:\test
```

## [Install-PackageProvider-Cmdlet](https://technet.microsoft.com/en-us/library/mt676543.aspx)
Mit diesem Cmdlet werden ein oder mehrere PackageManagement-Paketanbieter installiert.
```powershell
# Install a package provider from the PowerShell Gallery
Install-PackageProvider –Name "Gistprovider" -Verbose

# Install a specified version of a package provider
Find-PackageProvider –Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force

# Find a provider and install it
Find-PackageProvider –Name "Gistprovider" | Install-PackageProvider -Verbose

# Install a provider to the current user’s module folder
Install-PackageProvider –Name Gistprovider –Verbose –Scope CurrentUser
```

## [Register-PackageSource-Cmdlet](https://technet.microsoft.com/en-us/library/dn890701.aspx)
Mit diesem Cmdlet wird eine Paketquelle für einen angegebenen Paketanbieter hinzugefügt.
Jeder PackageManagement-Anbieter hat möglicherweise ein oder mehrere Softwarequellen oder Repositorys. PackageManagement bietet PowerShell-Cmdlets zum Hinzufügen/Entfernen/Abfrage der Quelle. Beispielsweise können Sie eine Paketquelle für den NuGet-Anbieter registrieren:
```powershell
Register-PackageSource -Name "NugetSource" -Location "http://www.nuget.org/api/v2" –ProviderName nuget
```

## [Save-Package-Cmdlet](https://technet.microsoft.com/en-us/library/dn890708.aspx)
Dieses Cmdlet dient zum Speichern von Paketen auf dem lokalen Computer, ohne sie zu installieren.
```powershell
# Saves jquery package to c:\test using NuGetProvider
# Notes that the -Path parameter must point to an existing location
Save-Package -Name jquery –Provider NuGet -Path c:\test

# Save a package by piping.
Find-Package -Name jquery -Source http://www.nuget.org/api/v2/ | Save-Package -Path c:\test
Find-Package -source c:\test
```

## [Set-PackageSource-Cmdlet](https://technet.microsoft.com/en-us/library/dn890710.aspx)
Dieses Cmdlet ändert die Informationen zu einer vorhandenen Paketquelle. 
```powershell
#Set-PackageSource changes the values for a source that has already been registered by running the Register-PackageSource cmdlet. By #running Set-PackageSource, you can change the source name and location.
Set-PackageSource  -Name nuget.org -Location  http://www.nuget.org/api/v2 -NewName nuget2 -NewLocation https://www.nuget.org/api/v2 
```

## [Cmdlet "Uninstall-Package"](https://technet.microsoft.com/en-us/library/dn890702.aspx)
Dieses Cmdlet deinstalliert Pakete, die auf dem lokalen Computer installiert sind.
```powershell
# Uninstall jquery using nuget
Uninstall-Package -Name jquery –Provider NuGet -Destination c:\test

# Uninstall a package with by piping with Get-Package
Get-Package -Name jquery –Provider NuGet -Destination c:\test | Uninstall-Package
```

## [Aufheben der Registrierung PackageSource-Cmdlet](https://technet.microsoft.com/en-us/library/dn890707.aspx)
```powershell
# Unregister a package source for the NuGet provider. You can use command Unregister-PackageSource, to disconnect with a repository, and Get-PackageSource, to discover what the repositories are associated with that provider.
Unregister-PackageSource  -Name "NugetSource"
```


<!--HONumber=Oct16_HO1-->


