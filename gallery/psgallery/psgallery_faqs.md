# Häufig gestellte Fragen

## Was ist ein PowerShell-Modul?

Ein PowerShell-Modul ist wieder verwendbaren Paket enthält einige PowerShell-Funktionen. Alles in PowerShell (Funktionen, Variablen, DSC-Ressourcen usw.) kann in Module gepackt werden. Module sind in der Regel Ordner, die bestimmte Arten von Dateien auf einen bestimmten Pfad enthalten. Es gibt verschiedene Typen von PowerShell-Module gibt.

## Was ist ein PowerShell-Skript?

Ein PowerShell-Skript ist eine Reihe von Befehlen, die in einer PS1-Datei wiederverwenden und Freigeben von aktivieren gespeichert sind. PowerShell-Workflows sind auch PowerShell-Skripts zeigen eine Reihe von Aufgaben und Sequenzierung für diese Aufgaben. Weitere Informationen finden Sie unter [Erste Schritte mit PowerShell-Workflow](https://technet.microsoft.com/en-us/library/jj134242.aspx).

## Worin unterscheiden sich PowerShell-Skripts von PowerShell-Module?

Module sind im Allgemeinen besser für die Freigabe, aber wir aktivieren Skript freigeben, um Sie Workflows und Skripts an der Community beteiligen erleichtern. Weitere Informationen finden Sie unter den folgenden Blogs:

- [Schreiben von Skripts, Schreiben von PowerShell-Module nicht](http://blogs.technet.com/b/heyscriptingguy/archive/2011/06/27/don-t-write-scripts-write-powershell-modules.aspx)
- [Grundlegendes zur PowerShell-Module](http://blogs.technet.com/b/heyscriptingguy/archive/2015/07/10/understanding-powershell-modules.aspx)

## Wie kann ich die PowerShell-Katalog veröffentlicht?

Sie müssen ein Konto in der PowerShell-Galerie registrieren, bevor Sie Elemente im Katalog veröffentlichen können. Dies ist da Veröffentlichen von Elementen eine NuGetApiKey, die bei der Registrierung bereitgestellt wird. Zum Registrieren Ihrer persönliche, Arbeit, oder schulkonto Anmelden der PowerShell-Galerie. Ein einmalige Registrierungsprozess ist erforderlich, wenn Sie sich zum ersten Mal anmelden. Danach ist die NuGetApiKey auf Ihrer Profilseite verfügbar.

Verwenden, wenn Sie im Katalog registriert haben, die [Veröffentlichen-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) oder [Veröffentlichungsskript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlets für Ihr Angebot im Katalog veröffentlichen. Weitere Informationen zum Ausführen dieser Cmdlets finden Sie auf der Registerkarte "Veröffentlichen", oder Lesen der [Veröffentlichen-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [Veröffentlichungsskript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Dokumentation.

**Sie müssen sich nicht registrieren oder anmelden, um den Katalog zu installieren oder Elemente zu speichern.**

## Ich habe "Fehler beim Verarbeiten der Anforderung. 'Der angegebene API-Schlüssel ist ungültig oder verfügt nicht über die Berechtigung zum Zugriff auf das angegebene Paket'. Der Remoteserver hat einen Fehler zurückgegeben: (403) verboten. " Fehler beim versuch, ein Element in der PowerShell-Galerie zu veröffentlichen. Was bedeutet das?

Dieser Fehler kann aus den folgenden Gründen auftreten:

- **Die angegebene API-Schlüssel ist ungültig.**
     Stellen Sie sicher, dass Sie den gültigen API-Schlüssel aus Ihrem Konto angegeben haben. Zeigen Sie Seite Ihres Profils, um Ihren API-Schlüssel zu erhalten.
- **Der angegebene Elementname ist nicht Besitzer.**
     Wenn Sie sich vergewissert haben Ihren API-Schlüssel richtig ist, und es existiert bereits eine Element mit demselben Namen wie die, die Sie verwenden möchten. Das Element wurde möglicherweise durch den Besitzer nicht aufgeführte, in diesem Fall wird es nicht in Suchergebnisse angezeigt. Um festzustellen, ob eine Element mit dem gleichen Namen bereits vorhanden ist, öffnen Sie einen Browser, und navigieren Sie zur Seite "Details" des Artikels: `https://www.powershellgallery.com/packages/<itemName>`. Navigieren direkt `https://www.powershellgallery.com/packages/pester` gelangen Sie zur Detailseite des Pester-Moduls, ob es nicht aufgeführte oder nicht handelt. Wenn eine Element mit einem in Konflikt stehenden Namen bereits vorhanden ist und nicht aufgeführte, können Sie:
    - Wählen Sie einen anderen Namen für das Element.
    - Wenden Sie sich an den Besitzer des vorhandenen Elements.

## Warum ich mich mit meinen persönlichen Konto anmelden kann nicht, aber ich konnte gestern anmelden?

Bedenken Sie dabei, dass Ihr Katalog-Konto nicht als Ihre primäre e-Mail-Alias integriert ist. Weitere Informationen finden Sie unter [Microsoft e-Mail-Aliasnamen](http://windows.microsoft.com/en-us/windows/outlook/add-alias-account).

## Warum die werden alle Galerieelemente nicht angezeigt, wenn Sie die Kategorie Kontrollkästchen auf der Registerkarte Elemente auswählen?

Wenn Sie das Kontrollkästchen Kategorie auswählen, geben Sie an "Ich möchte alle Elemente in dieser Kategorie angezeigt werden." Nur die Elemente in den ausgewählten Kategorien werden angezeigt. Also auch, wenn Sie alle Kontrollkästchen für die Kategorie auswählen, geben Sie an "Ich möchte alle Elemente in einer beliebigen Kategorie angezeigt werden." Aber einige Elemente in der Galerie gehören nicht zu den Kategorien aufgeführt, damit sie nicht in den Ergebnissen angezeigt werden. Um alle Elemente im Katalog angezeigt werden, deaktivieren Sie alle Kategorien, oder wählen Sie die Registerkarte "Elemente" erneut aus.

## Was sind die Anforderungen für ein Modul in der PowerShell-Gallery veröffentlichen?

Jede Art von PowerShell-Modul (Skriptmodule, binäre Module oder manifest Module) kann im Katalog veröffentlicht werden. Zum Veröffentlichen eines Moduls PowerShellGet muss wissen, einige Informationen über sie - Version, Beschreibung, Autor und wie es lizenziert ist. Diese Informationen werden als Teil der Veröffentlichungsprozess von gelesen der *modulmanifest* (psd1)-Datei oder aus dem Wert der [**Veröffentlichen-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet **LicenseUri** Parameter. Alle Module, die im Katalog veröffentlicht müssen modulmanifeste verfügen. Jedes Modul, das die folgende Informationen in das Manifest enthält, kann im Katalog veröffentlicht werden:

- Version
- Beschreibung
- Autor
- Ein URI, dem Lizenzvertrag des Moduls, entweder als Teil von der **PrivateData** -Abschnitt des Manifests oder in der **LicenseUri** Parameter von der [**Veröffentlichen-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

## Wie erstelle ich eine korrekt formatierte modulmanifest?

Die einfachste Möglichkeit zum Erstellen eines modulmanifests ist die Ausführung der [**New-ModuleManifest**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet. In PowerShell 5.0 oder höher, generiert New-ModuleManifest eine korrekt formatierte modulmanifest mit leeren Feldern für nützlich, Metadaten wie **ProjectUri**, **LicenseUri**, und **Tags**. Einfach Text Geben Sie ein, oder verwenden Sie das generierte Manifest als Beispiel für die richtige Formatierung.

Um sicherzustellen, dass alle erforderlichen Metadatenfelder korrekt gefüllt wurde, verwenden Sie die [**Test-ModuleManifest**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

Um das Modul Manifestdatei Felder aktualisieren, verwenden Sie die [**Update-ModuleManifest**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

## Was sind die Anforderungen für ein Skript im Katalog veröffentlichen?

Jede Art von PowerShell-Skript (Skripts oder Workflows) kann im Katalog veröffentlicht werden. Um ein Skript zu veröffentlichen, PowerShellGet muss wissen, einige Informationen über sie - Version, Beschreibung, Autor und wie es lizenziert ist. Diese Informationen werden als Teil des Veröffentlichungsvorgangs aus der Skriptdatei gelesen *PSScriptInfo* Abschnitt oder aus dem Wert der [**Veröffentlichen-Skript**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet **LicenseUri** Parameter. Alle Skripts im Katalog veröffentlicht müssen die Metadateninformationen. Jedes Skript, das die folgenden Informationen im Abschnitt für die PSScriptInfo enthält, kann im Katalog veröffentlicht werden:

- Version
- Beschreibung
- Autor
- Ein URI, dem Lizenzvertrag des Skripts, entweder als Teil von der **PSScriptInfo** Abschnitt des Skripts oder in der **LicenseUri** Parameter von der [**Veröffentlichen-Skript**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

## Wie Suche ich?

Geben Sie an, was Sie in das Textfeld Suchen. Z. B. Wenn Sie Module finden möchten, die auf SQL Azure beziehen, geben Sie einfach "Sql Azure". Suchmaschine sucht nach diesen Schlüsselwörtern in alle veröffentlichten Elemente, einschließlich Titel, Beschreibung und in Metadaten. Anschließend wird basierend auf einem gewichteten Qualitätsergebnis, entspricht der nächsten angezeigt. Sie können auch nach bestimmten Feld Suchen: "Wert" Syntax in der Abfrage für die folgenden Felder:

- Tags
- Funktionen
- Cmdlets
- DscResources
- PowerShellVersion

In diesem Fall z. B. bei der Suche nach PowerShellVersion: "2.0" nur Ergebnisse, die kompatibel mit PowerShellVersion 2.0 (auf der Grundlage der Modul-Skript Manifest) werden angezeigt.

## Wie erstelle ich eine korrekt formatierte Skriptdatei?

Am einfachsten erstellen Sie eine korrekt formatierte Skriptdatei ist die Ausführung der [**neu ScriptFileInfo**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet. In PowerShell 5.0 neu ScriptFileInfo generiert eine korrekt formatierte Skriptdatei mit leeren Feldern für nützlich, Metadaten wie **ProjectUri**, **LicenseUri**, und **Tags**. Einfach Text Geben Sie ein, oder verwenden Sie die generierte Skriptdatei als Beispiel für die richtige Formatierung.

Um sicherzustellen, dass alle erforderlichen Metadatenfelder korrekt gefüllt wurde, verwenden Sie die [**Test ScriptFileInfo**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

Um die Metadatenfelder Skript zu aktualisieren, verwenden die [**Update ScriptFileInfo**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

## Wie andere Typen von PowerShell-Module sind vorhanden?

Das PowerShell-Modul Begriff bezieht sich auch auf die Dateien, die eigentliche Funktionalität zu implementieren. Modul-Skriptdateien (. psm1) enthalten die PowerShell-Code. Binärer Moduldateien (.dll) enthalten kompilierten Code.

Hier ist eine Möglichkeit vorstellen: der Ordner, der das Modul kapselt ist das Modul. Die Modulordner kann ein modulmanifest (psd1) enthalten, die den Inhalt des Ordners beschreibt. Die Dateien, die die Arbeit ausführen sind die Modul-Skriptdateien (. psm1) und das binäre Modul-Dateien (.dll). DSC-Ressourcen befinden sich in einem bestimmten Unterordner und als Skript-Modul oder binäre Moduldateien implementiert werden.

Alle Module in der Galerie modulmanifeste enthalten, und die meisten dieser Module enthalten, Skript-Modul oder binäre Modul-Dateien. Das Begriff Modul kann aufgrund dieser unterschiedliche Bedeutung verwirrend sein. Sofern nicht ausdrücklich anders angegeben, finden Sie alle Verwendungen des Moduls Word auf dieser Seite in den Modulordner mit diesen Dateien.

## In welcher Beziehung steht PackageManagement zum PowerShellGet? (Hohe Ebene Antwort)

PackageManagement ist eine allgemeine Schnittstelle für die Arbeit mit Paket-Manager. Schließlich, ob Sie mit PowerShell-Module, MSIs, Ruby Gems, NuGet-Pakete oder Perl-Module arbeiten, sollten Sie möglicherweise PackageManagement Befehle (Find-Paket und Install-Package) zum Suchen und installieren sie. PackageManagement wird mit einer Paket-Anbieter für jedes Paket-Manager, die in PackageManagement integriert wird. Anbieter müssen die eigentliche Arbeit; Sie Inhalte von Repositorys abrufen und die Inhalte lokal zu installieren. Häufig herumfließen Paket Anbieter einfach die vorhandenen Paket-Manager-Tools für einen gegebenen Paket.

PowerShellGet ist die Paket-Manager für PowerShell-Elemente. Es ist ein PSModule-Paket-Anbieter, der Funktionen für die PowerShellGet über PackageManagement verfügbar macht. Aus diesem Grund können Sie entweder [Install-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) oder Install-Package-Anbieter PSModule zum Installieren eines Moduls aus der PowerShell-Galerie. Bestimmte Funktionen PowerShellGet einschließlich [Update-Modul](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [Veröffentlichen-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409), PackageManagement Befehle zugreifen kann.

Zusammenfassend PowerShellGet ausschließlich konzentriert sich auf eine Premium-Paket-Verwaltungsfunktionen für PowerShell-Inhalte. PackageManagement konzentriert sich Paket Management Erfahrungen über einen allgemeinen Satz Tools bereitstellen. Wenn Sie diese Antwort unsatisfying finden, ist eine lange Antwort am Ende dieses Dokuments, der **wie PackageManagement tatsächlich bezieht sich auf PowerShellGet?** Abschnitt.

Weitere Informationen finden Sie auf der [PackageManagement-Projektseite](http://oneget.org/).

## In welcher Beziehung steht NuGet auf PowerShellGet?

Der PowerShell-Katalog ist eine geänderte Version der [NuGet-Katalog](http://www.nuget.org/). NuGet-Anbieter wird von PowerShellGet zur Arbeit mit NuGet-basierten Repositorys wie der PowerShell-Galerie verwendet.

Sie können PowerShellGet für eine beliebige gültige NuGet-Repository oder die Dateifreigabe verwenden. Müssen Sie einfach das Repository mit Hinzufügen der [**Registrieren PSRepository**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.

## Bedeutet dies, dass ich zum Arbeiten mit der Galerie NuGet.exe verwenden können?

Ja.

## Wie bezieht sich PackageManagement tatsächlich auf PowerShellGet? (Technische Details)

Hinter den Kulissen nutzt PowerShellGet stark PackageManagement Infrastruktur.

Auf der PowerShell-Cmdlet-Ebene [Install-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) ist tatsächlich ein einfacher Wrapper für die Install-Package-Anbieter PSModule.

Auf der Ebene der PackageManagement Paket-Anbieter ruft der PSModule-Paket-Anbieter in anderen PackageManagement-Paket-Anbietern. Bei der Arbeit mit NuGet basierenden Galerien (z. B. den PowerShell-Katalog) verwendet der PSModule-Paket-Anbieter der NuGet-Paket-Anbieter z. B. arbeiten mit dem Repository.

![PowerShellGet-Architektur](Images/powershellgetArchitecture.png)

Abbildung 1: PowerShellGet-Architektur

## Was ist erforderlich, um PowerShellGet auszuführen?

Im Allgemeinen wird empfohlen, die neueste Version der PowerShellGet-Modul (Beachten Sie, dass sie .NET 4.5) auswählen.

Für das **PowerShellGet**-Modul ist **PowerShell 3.0 oder neuer** erforderlich.

Aus diesem Grund erfordert **PowerShellGet** eines der folgenden Betriebssysteme:

- Windows 10
- Windows 8.1 Pro
- Windows 8.1 Enterprise
- Windows 7 SP1
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2008 R2 SP1

Für **PowerShellGet** ist außerdem .NET Framework 4.5 oder höher erforderlich. Von [hier](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx) können Sie .NET Framework 4.5 oder höher installieren.

## Ist es möglich, Reservieren von Namen für Elemente, die in Zukunft veröffentlicht wird?

Es ist nicht möglich, Hock Elementnamen. Wenn Sie glauben, dass ein vorhandenes Element übernommen hat dem Namen, der Ihr Angebot am besten entspricht mehr Try [wenden Sie sich an den Besitzer des Elements](psgallery_contacting_item_owners.md). Wenn Sie in ein paar Wochen Antwort erhalten haben, erhalten Sie beim Support und das PowerShell-Galerie-Team sieht zu.

## Wie mache ich den Besitzer für Objekte geltend?

Auschecken [PowerShellGallery.com Element Besitzer verwalten](Managing-Item-Owners.md) Details.

## Wie gehe ich mit dem Element Besitzer um, die meine Lizenz Element verletzt?

Wir empfehlen die PowerShell-Community zusammenarbeiten, um alle Rechtsstreitigkeiten zu beheben, die zwischen dem Element und der Besitzer der anderen Elemente auftreten können.  Wir haben gestalteten eine [bezweifeln Auflösungsvorgang](psgallery_dispute_resolution.md) die wir Sie bitten zu folgen, bevor PowerShellGallery.com Administratoren eingreifen.

<!--HONumber=Oct16_HO1-->


