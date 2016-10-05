# Ermittlung, Installation und Inventur von PowerShell-Modulen mit PowerShellGet
 
PowerShellGet ist in dieser Version von WMF enthalten:
-   „Find-Module“ kann Modulmetadaten anhand des „-Tag“-Parameters filtern.
-   „Find-Module“ kann mit dem „-Filter“-Parameter nach einer repositoryspezifischen Suchsprache filtern.
-   „Find-Module“ kann mit den Parametern „-Command“, „-DscResource“ und „-Includes“ den Modulinhalt filtern.
-   „Find-DscResource“ ermöglicht die Ermittlung einzelner DSC-Ressourcen in Repositorys.
-   Unterstützung für die Installation über und Veröffentlichung in Dateifreigaben mit NuGet

## Beispiele für Befehle
```powershell
\# Find all modules with tags Azure or DSC
Find-Module -Tag Azure, DSC

\# Find modules with a specific DscResource
Find-Module -DscResource xFirewall

\#Find modules with specific commands
Find-Module -Command Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer

\# Find all modules with Dsc resources
Find-Module -Includes DscResource

\# Find all modules with cmdlets
Find-Module -Includes Cmdlet

\# Find all modules with functions
Find-Module -Includes Function

\# Find all DSC resources
Find-DscResource

\# Find all DSC resources contained within a specific module
Find-DscResource -ModuleName xNetworking

\# Find all DSC resources in modules with DSCResourceKit or DesiredStateConfiguration
Find-DscResource -Tag DesiredStateConfiguration, DSCResourceKit

\# Find modules using -Filter parameter
\# Specified filter value is searched in Name and Description properties
Find-Module -Filter Cookbook -Repository PSGallery
Find-Module -Filter RBAC -Repository PSGallery
```

## Neue Features in Windows PowerShellGet
-   Unterstützung der gleichzeitigen Ausführung unterschiedlicher Versionen für Windows PowerShell 5.0 oder höher
-   Unterstützung der Installation von Modulabhängigkeiten
-   Drei neue Cmdlets
    -   Get-InstalledModule
    -   Uninstall-Module
    -   Save-Module
    

<!--HONumber=Oct16_HO1-->


