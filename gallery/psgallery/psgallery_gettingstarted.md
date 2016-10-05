# Erste Schritte mit der PowerShell-Galerie

## Was ist der PowerShell-Galerie?

Der PowerShell-Katalog ist das zentrale Repository für PowerShell-Inhalte.
Darin finden Sie nützliche PowerShell-Module, die mit PowerShell-Befehlen und Desired State Configuration (DSC)-Ressourcen. Sie finden auch PowerShell-Skripts, von die einige PowerShell-Workflows und die Aufgaben beschreiben enthalten und Sequenzierung für diese Vorgänge bereitstellen kann.
Einige dieser Elemente werden von Microsoft erstellt, und andere werden von der PowerShell-Community erstellt.

## Anforderungen

Herunterladen von Elementen aus der Galerie PowerShell auf Ihrem System muss die [PowerShellGet](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Modul. Sie finden das PowerShellGet-Modul in eine der folgenden. Sie müssen nicht anmelden, um Elemente aus dem PowerShell-Katalog herunterzuladen.

-   [Windows 10](http://go.microsoft.com/fwlink/?LinkID=624830&clcid=0x409)
-   [Windows Management Framework 5.0](http://go.microsoft.com/fwlink/?LinkId=398175)
-   [MSI-Installationsprogramm (für PowerShell, 3 und 4)](http://go.microsoft.com/fwlink/?LinkID=746217&clcid=0x409)

PowerShellGet erfordert außerdem die [NuGet Anbieter](http://go.microsoft.com/fwlink/?LinkId=722208) mit der PowerShell-Galerie arbeiten. Sie werden aufgefordert, den NuGet-Anbieter automatisch bei der ersten Verwendung von PowerShellGet zu installieren, ist der NuGet-Anbieter nicht in einem der folgenden Speicherorte:

-   $env: ProgramFiles\\PackageManagement\\ProviderAssemblies
-   $env: LOCALAPPDATA\\PackageManagement\\ProviderAssemblies

Oder führen Sie **Installation PackageProvider-Namen NuGet-Force** zum Download und Installation des NuGet-Anbieters zu automatisieren.

  
Wenn Sie mit eine älteren Version als 2.8.5.201 NuGet haben, müssen Sie rufen die folgenden PowerShell-Cmdlets zum Installieren, und wechseln Sie auf die neueste Version von NuGet.

1.  Install-PackageProvider NuGet - MinimumVersion '2.8.5.201'-Force
2.  Import-PackageProvider NuGet - MinimumVersion '2.8.5.201'-Force
3.  Löschen Sie die ältere Version von NuGet im obigen Installationsverzeichnis.

Weitere Informationen finden Sie unter <http://oneget.org/> .

  
Hinweis: Aufgrund von Änderungen in Formate Verpacken, wird empfohlen, zu aktualisieren, um die neueste Version der PowerShellGet und PackageManagement, um Elemente zu installieren, die kürzlich aktualisiert wurden. PowerShellGet befindet sich auf Windows 10, erfahren Sie mehr über können [hier](http://go.microsoft.com/fwlink/?LinkID=624830&clcid=0x409).
PowerShellGet ist auch Teil von Windows Management Framework (WMF) 5.0, die Sie herunterladen können [hier](http://go.microsoft.com/fwlink/?LinkId=398175).

## Ermittlung von Elementen aus der PowerShell-Galerie

Sie können Elemente in der PowerShell-Galerie suchen, mit der **Suche** -Steuerelement auf dieser Website oder durch Navigieren durch die Seiten und Skripts. Sie finden auch Objekte aus der Galerie PowerShell durch Ausführen der [**Suchen-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [**Suchen-Skript**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlets, die je nach Elementtyp, mit **-Repository PSGallery**.

Filtern von Ergebnissen aus der Galerie kann erfolgen, indem mit den folgenden Parametern von [Suchen-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [Suchen-Skript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409)

- Name
- AllVersions
- MinimumVersion
- RequiredVersion
- Tag
- Enthält
- DscResource
- RoleCapability
- Befehl
- Filter

Wenn Sie nur spezifische DSC-Ressourcen in der Galerie ermitteln möchten, können Sie Ausführen den [**Suchen-DscResource**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet.
[**Suchen-DscResource**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) auf DSC-Ressourcen, die im Katalog enthaltenen Daten zurückgegeben. Da DSC-Ressourcen immer als Teil eines Moduls geliefert werden, müssen dennoch ausführen [Install-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) diese DSC-Ressourcen zu installieren.

## Erfahren Sie mehr über die Elemente in der PowerShell-Galerie

Wenn Sie ein Element, die Sie interessiert sind ermittelt haben, sollten Sie weitere Informationen. Dazu können Sie untersuchen des Artikels bestimmte Seite in der Sammlung. Auf dieser Seite werden Sie sehen alle Metadaten, die mit dem Element hochgeladen. Diese Metadaten für ein Element wird durch den Autor des Objekts bereitgestellt und von Microsoft nicht bestätigt. Der Besitzer des Elements eng an das Katalog-Konto verwendet, um das Element zu veröffentlichen, ist, und mehr als das Feld Autor vertrauenswürdig ist.

Wenn Sie erkennen ein Element, das Sie der Meinung ist nicht veröffentlicht, in gutem Glauben, klicken Sie auf **Missbrauch** auf Seite des Elements.

Wenn Sie ausführen, [Suchen-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) oder [Suchen-Skript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409), können Sie diese Daten im zurückgegebenen PSGetModuleInfo Objekt anzeigen. Zum Beispiel ausgeführt [**Suchen-Module-Name PSReadLine-Repository PSGallery | Get-Member**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Daten auf das PSReadLine-Modul in der Galerie zurückgegeben.

## Herunterladen von Elementen aus der PowerShell-Galerie

Wir empfehlen den folgenden Prozess, beim Herunterladen von Elementen aus der PowerShell-Galerie:

### Überprüfen

Führen Sie zum Herunterladen eines Elements aus dem Katalog für die Überprüfung entweder die [**Speichern-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) oder [**Speichern-Skript**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) -Cmdlet, je nach den Elementtyp. Dadurch können Sie das Element lokal speichern, ohne es zu installieren, und überprüfen den Inhalt. Denken Sie daran, das gespeicherte Element manuell löschen.

Einige dieser Elemente werden von Microsoft erstellt, und andere werden von der PowerShell-Community erstellt. Microsoft empfiehlt, dass Sie den Inhalt und den Code der Elemente in diesem Katalog vor der Installation überprüfen.

Wenn Sie erkennen ein Element, das Sie der Meinung ist nicht veröffentlicht, in gutem Glauben, klicken Sie auf **Missbrauch** auf Seite des Elements.

### Installation

Um ein Element aus dem Katalog für die Verwendung zu installieren, führen Sie entweder die [**Install-Module**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) oder [**Installationsskript**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) -Cmdlet, je nach den Elementtyp.

[Install-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) installiert das Modul zum $env: ProgramFiles\\WindowsPowerShell\\Modules standardmäßig. Dies erfordert ein Administratorkonto. Wenn Sie beim Hinzufügen der **-Bereich CurrentUser** $env Parameter, die das Modul installiert ist: USERPROFILE\\Documents\\WindowsPowerShell\\Modules.

[Skript für die Installation](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) installiert das Skript $env: ProgramFiles\\WindowsPowerShell\\Scripts standardmäßig. Dies erfordert ein Administratorkonto. Wenn Sie beim Hinzufügen der **-Bereich CurrentUser** $env Parameter, das Skript installiert ist: USERPROFILE\\Documents\\WindowsPowerShell\\Scripts.

In der Standardeinstellung [Install-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) und [-Installationsskript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) installiert die aktuelle Version eines Elements. Fügen Sie zum Installieren von einer älteren Version des Elements, das **- RequiredVersion** Parameter.

### Stellen Sie

Wenn ein Element aus der PowerShell-Galerie in Azure Automation bereitstellen möchten, klicken Sie auf **Bereitstellen in Azure Automation** auf der Detailseite des Elements. Sie werden zum Azure-Verwaltungsportal weitergeleitet, wo Sie melden Sie sich mit Ihren Azure-Konto-Anmeldeinformationen. Beachten Sie, dass Elemente mit Abhängigkeiten Bereitstellen aller Abhängigkeiten in Azure Automation bereitgestellt werden. Die Schaltfläche "Bereitstellen in Azure Automation" deaktiviert werden, indem das Hinzufügen der **AzureAutomationNotSupported** Tag, um die Metadaten des Elements.

Weitere Informationen zu Azure Automation finden Sie unter der [Azure Automation-Website](http://azure.microsoft.com/en-us/services/automation/).

## Aktualisieren von Elementen aus der PowerShell-Galerie

Führen Sie zum Aktualisieren aus der Galerie PowerShell installierte Elemente entweder die [Update-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) oder [-Updateskript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet. Bei Ausführung ohne zusätzliche Parameter [Update-Modul](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) versucht, die einzelnen Module durch Ausführen von installiert aktualisieren [Install-Module](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409).
Um Module einzeln zu aktualisieren, fügen die **-Namen** Parameter.

Auf ähnliche Weise die Ausführung ohne zusätzliche Parameter [Update-Skript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) versucht auch, aktualisieren Sie jedes Skript, das durch Ausführen von installiert [Installationsskript](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409).
Um Skripts selektiv zu aktualisieren, fügen die **-Namen** Parameter.

## Listenelemente, die Sie aus dem Katalog PowerShell installiert haben

Um herauszufinden, welche Module Sie aus dem Katalog PowerShell installiert haben, führen Sie die [**Get-InstalledModule**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet. Dieser Befehl listet alle Module, die auf Ihrem System installiert, die direkt aus dem Katalog PowerShell installiert wurden.

Auf ähnliche Weise, um herauszufinden, welche Skripts aus dem Katalog PowerShell installiert, führen Sie die [**Get-InstalledScript**](http://go.microsoft.com/fwlink/?LinkID=760387&clcid=0x409) Cmdlet. Dieser Befehl listet alle Skripts, die auf Ihrem System installiert, die direkt aus dem Katalog PowerShell installiert wurden.


<!--HONumber=Oct16_HO1-->


