# Der PowerShell-Katalog

Der PowerShell-Katalog ist das zentrale Repository für PowerShell-Inhalte. Im Katalog finden Sie neue PowerShell-Befehle oder DSC-Ressourcen (Desired State Configuration).

# Übersicht über PowerShellGet

Das PowerShellGet-Modul enthält Cmdlets zum Ermitteln, Installieren, Aktualisieren und Veröffentlichen der PowerShell-Artefakte wie Module, DSC-Ressourcen, Rollenfunktionen und Skripts über https://www.PowerShellGallery.com und andere private Repositorys.

## Erste Schritte mit dem Katalog

Für die Installation von Elementen über den Katalog ist die neueste Version des PowerShellGet-Moduls erforderlich, die in Windows 10, im Windows Management Framework (WMF) 5.0 oder im MSI-basierten Installer (für PowerShell 3 und 4) verfügbar ist.

- [**Windows 10**](http://go.microsoft.com/fwlink/?LinkID=624830&clcid=0x409) abrufen
- [**WMF 5.0**](http://go.microsoft.com/fwlink/?LinkId=398175) abrufen
- [**Abrufen von MSI-Installationsprogramm**](http://go.microsoft.com/fwlink/?LinkID=746217&clcid=0x409)

Mit dem neuesten [PowerShellGet](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)-Modul haben Sie folgende Möglichkeiten:

-   Durchsuchen der Elemente im Katalog mit [**Find-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [**Find-Script**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)
-   Speichern von Elementen aus dem Katalog in Ihrem System mit [**Save-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [**Save-Script**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)
-   Installieren von Elementen aus dem Katalog mit [**Install-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [**Install-Script**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)
-   Hochladen von Elementen im Katalog mit [**Publish-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [**Publish-Script**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)
-   Hinzufügen Ihres eigenen benutzerdefinierten Repositorys mit [**Register-PSRepository**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)

Sehen Sie sich die Seite [Erste Schritte](psgallery/psgallery_gettingstarted.md) an, um weitere Informationen zur Verwendung von PowerShellGet-Befehlen mit dem Katalog zu erhalten. Sie können auch *Update-Help -Module PowerShellGet* ausführen, um die lokale Hilfe für diese Befehle zu installieren.

## Unterstützte Betriebssysteme

Für das **PowerShellGet**-Modul ist **PowerShell 3.0 oder neuer** erforderlich.

Aus diesem Grund erfordert **PowerShellGet** eines der folgenden Betriebssysteme:

- Windows 10
- Windows 8.1 Pro
- Windows 8.1 Enterprise
- Windows 7 SP1
- Windows Server 2016 TP5
- Windows Server 2012 R2
- Windows Server 2008 R2 SP1

Für **PowerShellGet** ist außerdem .NET Framework 4.5 oder höher erforderlich. Von [hier](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx) können Sie .NET Framework 4.5 oder höher installieren.


## Sie haben eine Frage? Haben Sie Feedback?

Weitere Informationen zum PowerShell-Katalog und zu PowerShellGet finden Sie auf der Seite [Erste Schritte](psgallery/psgallery_gettingstarted.md). Stellen Sie Feedback bereit, und melden Sie Probleme über [UserVoice](http://windowsserver.uservoice.com/forums/301869-powershell).



<!--HONumber=Oct16_HO1-->


