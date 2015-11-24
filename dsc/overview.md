#WindowsPowerShell gewünschten Status (Übersicht)

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Dieses Thema beschreibt die Funktion der Windows PowerShell gewünscht State Configuration (DSC) in Windows PowerShell. Sie können in diesem Thema verwenden, um einen Überblick über die DSC abzurufen und die Dokumentationsressourcen müssen Sie verstehen und Verwenden von DSC.

##Funktionsbeschreibung

DSC ist eine neue Management-Plattform in Windows PowerShell, die es ermöglicht, bereitstellen und Verwalten von Konfigurationsdaten für die Softwaredienste und Verwaltung der Umgebung, in der diese Dienste ausgeführt werden.

DSC bietet eine Reihe von Windows PowerShell-spracherweiterungen, neue Windows PowerShell-Cmdlets und Ressourcen, mit denen Sie deklarativ angeben, wie Ihre softwareumgebung konfiguriert werden soll. Darüber hinaus steht eine Möglichkeit zum Verwalten und vorhandene Konfigurationen zu verwalten.

##In der Praxis

Es folgen einige Beispielszenarien, in denen Sie integrierte DSC-Ressourcen konfigurieren und verwalten eine Gruppe von Computern (auch bekannt als Zielknoten) automatisiert verwenden können:

* Aktivieren oder Deaktivieren von Serverrollen und features
* Verwalten von Registrierungseinträgen
* Verwalten von Dateien und Verzeichnissen
* Starten, Anhalten und Verwalten von Prozessen und Diensten
* Verwalten von Gruppen und Benutzerkonten
* Neue Software bereitstellen
* Verwalten von Umgebungsvariablen
* Ausführen von Windows PowerShell-Skripts
* Beheben von einer Konfigurations hat den gewünschten Status verschwunden.
* Ermitteln den Konfigurationsstatus der tatsächlichen auf einem bestimmten Knoten

##Wichtige Konzepte

DSC ist eine deklarative Plattform für die Konfiguration, Bereitstellung und Verwaltung von Systemen verwendet. Es besteht aus drei Hauptkomponenten:

* [Konfigurationen](configurations.md) sind deklarative PowerShell-Skripts definieren und Konfigurieren von Instanzen von Ressourcen. Beim Ausführen der Konfiguration, DSC (und die Ressourcen, die von der Konfiguration des aufgerufenen) wird sicherstellen einfach "müssen sie also",, dass das System in den Zustand angeordnet, die durch die Konfiguration vorhanden ist. DSC Konfigurationen auch Idempotent sind: lokale Configuration Manager (LCM) wird fortgesetzt, um sicherzustellen, dass der Computer konfiguriert sind, in den Inhalt die Konfiguration Zustand deklariert.
* Ressourcen sind die imperative Bausteine von DSC die geschrieben werden, um die verschiedenen Komponenten des ein Teilsystem modellieren und Implementieren der Kontrollfluss, der ihren Status ändern. Sie befinden sich in PowerShell-Module und etwas als generische als eine Datei oder ein Windows-Prozess oder wie ein IIS-Server oder einen virtuellen Computer in Azure ausgeführte Modell geschrieben werden können.
* Die lokale Configuration Manager (LCM) ist das Modul mit dem DSC erleichtert die Interaktion zwischen Ressourcen und Konfigurationen. Der LCM fragt regelmäßig das System Kontrollfluss implementiert, indem Ressourcen verwenden, um sicherzustellen, dass der Status von einer Konfiguration angeordnet werden. Wenn das System aus-Zustand ist, verwendet LCM mehr Logik in den Ressourcen "dies entsprechend der Konfiguration Deklaration machen".

DSC enthält auch eine Reihe von neuen Programmiersprachen-Schlüsselwörter, Cmdlets und Tools, die Erstellung von Konfigurationen, Hilfe Build DSC Ressourcen zulassen, rufen Sie die Konfigurationen und LCM verwalten. Viele von diesen Cmdlets finden Sie in Windows 8.1 als Teil des Moduls PsDesiredStateConfig (einschließlich `Start DscConfiguration`, `Set DscLocalConfigurationManager`, und `Get-DscResource`). Die xDscResourceDesigner (finden Sie in der [PowerShell Gallery](https://www.powershellgallery.com/packages/xDSCResourceDesigner/)) ist eine Sammlung von Cmdlets, die die Entwicklung von DSC-Ressourcen zu vereinfachen.

##Weitere Informationen

* [DSC-Konfigurationen](configurations.md)
* [DSC-Ressourcen](resources.md)
* [Konfigurieren des lokalen Konfigurations-Managers](metaconfig.md)





