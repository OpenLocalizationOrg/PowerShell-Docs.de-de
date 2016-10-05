# Update-Module

Heruntergeladen und installiert die neueste Version der angegebenen Module aus einer online-Katalog auf dem lokalen Computer.

## Beschreibung

Das Cmdlet Update-Modul installiert eine neuere Version von Windows PowerShell-Moduls, die aus dem Onlinekatalog durch das Ausführen von Install-Module auf dem lokalen Computer installiert wurde.

Standardmäßig ist die neueste Version des angegebenen Moduls Onlinekatalog installiert, wenn Sie eine erforderliche Version angeben. Sie können eine vorhandene, installierte Modul aktualisieren, den Namen des Moduls angeben. Update-Modul durchsucht $env: PSModulePath für das Modul, das Sie aktualisieren möchten.

Ausführen von Update-Modul ohne den Name-Parameter aktualisiert alle Module, die auf dem lokalen Computer aktualisiert werden können.

### Hinweise

- Mit diesem Cmdlet wird auf Windows PowerShell 3.0 oder spätere Versionen von Windows PowerShell unter Windows 7 oder Windows 2008 R2 und höheren Versionen von Windows ausgeführt werden.
- Wenn das Modul, das Sie mit dem Name-Parameter angeben, nicht mithilfe von Install-Modul installiert wurde, tritt ein Fehler auf. Sie können nur Update-Modul ausführen, die auf Module, die Sie aus dem Onlinekatalog Ausführen von Install-Module installiert.
- Wenn Update-Modul versucht, Binärdateien zu aktualisieren, die verwendet werden, gibt Update-Modul einen Fehler, der die Prozesse Problem identifiziert und informiert den Benutzer zum Wiederholen der Update-Modul nach dem Beenden der Prozesse, zurück.
- Auf PowerShell 5.0 oder neuere Versionen sowie beim Update-Module ein Modul aktualisiert hinzugefügt wird die aktuelle (oder angegebene) Version des Moduls, damit die älteren und neueren Versionen nebeneinander im selben Verzeichnis jetzt sind. Es wäre hilfreich, also z. B. und ein Beispiel für die Ausgabe dieser Befehle angezeigt.


## Cmdlet-syntax
```powershell
Get-Command -Name Update-Module -Module PowerShellGet -Syntax
```

## Online-Hilfe-Cmdlet-Referenz

[Update-Modul](http://go.microsoft.com/fwlink/?LinkID=398576)


## Beispiele für Befehle

```powershell
PS C:\\windows\\system32> Update-Module -Name ContosoServer -RequiredVersion 1.5
PS C:\\windows\\system32> Get-Module -ListAvailable -Name ContosoServer | Format-List Name,Version,ModuleBase
Name : ContosoServer
Version : 2.0
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\2.0
Name : ContosoServer
Version : 1.5
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\1.5
Name : ContosoServer
Version : 1.0
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\1.0
PS C:\\windows\\system32> Get-InstalledModule
Version Name Repository Description
------- ---- ---------- -----------
1.0 ContosoServer MSPSGallery ContosoServer module
1.5 ContosoServer MSPSGallery ContosoServer module
2.0 ContosoServer MSPSGallery ContosoServer module
PS C:\\windows\\system32> Update-Module -Name ContosoServer
PS C:\\windows\\system32> Get-Module -ListAvailable -Name ContosoServer | Format-List Name,Version,ModuleBase
Name : ContosoServer
Version : 2.8.1
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\2.8.1
Name : ContosoServer
Version : 2.0
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\2.0
Name : ContosoServer
Version : 1.5
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\1.5
Name : ContosoServer
Version : 1.0
ModuleBase : C:\\Program Files\\WindowsPowerShell\\Modules\\ContosoServer\\1.0
PS C:\\windows\\system32> Get-InstalledModule
Version Name Repository Description
------- ---- ---------- -----------
1.0 ContosoServer MSPSGallery ContosoServer module
1.5 ContosoServer MSPSGallery ContosoServer module
2.0 ContosoServer MSPSGallery ContosoServer module
2.8.1 ContosoServer MSPSGallery ContosoServer module
```


###  Aktualisieren Sie das Modul „TestDepWithNestedRequiredModules1“ mit Abhängigkeiten.
```powershell
Find-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -AllVersions

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module

Update-Module -Name TestDepWithNestedRequiredModules1 -RequiredVersion 2.0
Get-InstalledModule

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        NestedRequiredModule1               LocalRepo   NestedRequiredModule1 module
2.5        NestedRequiredModule2               LocalRepo   NestedRequiredModule2 module
2.0        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
2.5        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
1.0        RequiredModule1                     LocalRepo   RequiredModule1 module
2.5        RequiredModule2                     LocalRepo   RequiredModule2 module
2.0        RequiredModule3                     LocalRepo   RequiredModule3 module
2.5        RequiredModule3                     LocalRepo   RequiredModule3 module
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
```

<!--HONumber=Oct16_HO1-->


