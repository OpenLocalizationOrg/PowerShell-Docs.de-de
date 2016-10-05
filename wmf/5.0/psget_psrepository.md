# Registrieren eines PowerShell-Repositorys
Sie können PowerShellGet für interne Repositorys konfigurieren. Dies erfolgt mithilfe der folgenden Erweiterungen:
- Register-PSRepository: Registriert ein Repository für den aktuellen Benutzer.
- Unregister-PSRepository: Entfernt ein registriertes Repository für den aktuellen Benutzer.
- Set-PSRepository: Legt Werte für ein registriertes Repository fest.
- Get-PSRepository: Ruft alle registrierten Repositorys für den aktuellen Benutzer ab.

Nachdem ein Repository registriert wurde, können Sie „Find-Module“ und „Install-Module“ dafür verwenden.

```powershell
\#Register a default repository
Register-PSRepository –Name DemoRepo –SourceLocation “https://www.myget.org/F/powershellgetdemo/api/v2” –PublishLocation “<https://www.myget.org/F/powershellgetdemo/api/v2>/package” –InstallationPolicy –Trusted

\#Get all of the registered repositories
Get-PSRepository
Name SourceLocation
---- --------------
PSGallery https://msconfiggallery.cloudapp...
DemoRepo https://www.myget.org/F/powershe...

\#Search only the new repository for modules
Find-Module -Repository DemoRepo
Repository Version Name
---------- ------- ----
DemoRepo 1.0.1 xActiveDirectory
DemoRepo 1.1.1 SomeModule

\#By default, PowerShellGet operates against all registered repositories when none is specified. In this example, the “SomeModule” module is installed from the DemoRepo.
Install-Module SomeModule

\#Removing a repository
Unregister-PSRepository DemoRepo
```

<!--HONumber=Oct16_HO1-->


