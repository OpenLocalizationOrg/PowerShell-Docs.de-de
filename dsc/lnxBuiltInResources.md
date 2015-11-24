#Integrierte gewünschten Status Konfigurationsressourcen für Linux

Ressourcen sind die Bausteine, die Sie zum Schreiben eines Skripts PowerShell gewünscht State Configuration (DSC) verwenden können. DSC für Linux enthält eine Reihe von integrierten Funktionen zum Konfigurieren von Ressourcen wie Dateien und Ordner, Pakete, Umgebungsvariablen und Dienste und Prozesse.

##Integrierte Ressourcen

Die folgende Tabelle enthält eine Liste dieser Ressourcen und Links zu Themen, die ausführlich zu beschreiben.

* [NxArchive Resource](lnxArchiveResource.md)– bietet einen Mechanismus zum (TAR, ZIP) Archivdateien an einen bestimmten Pfad zu entpacken.
* [NxEnvironment Resource](lnxEnvironmentResource.md)-Umgebungsvariablen auf dem Zielknoten verwaltet.
* [NxFile Resource](lnxFileResource.md)– Linux verwaltet Dateien und Verzeichnisse.
* [NxFileLine Resource](lnxFileLineResource.md)– einzelne Zeilen in einer Linux-Datei verwaltet.
* [NxGroup Resource](lnxGroupResource.md)--lokale Linux-Gruppen verwaltet.
* [NxPackage Resource](lnxPackageResource.md)--verwaltet Pakete auf Linux-Knoten - [NxScript Resource](lnxScriptResource.md)--führt Skripts auf dem Zielknoten.
* [NxService Resource](lnxServiceResource.md)--verwaltet Linux-Dienste (Daemons).
* [NxSshAuthorizedKeys Resource](lnxSshAuthorizedKeysResource.md)--verwaltet öffentlichen ssh Keys für einen Linux-Benutzer.
* [NxUser Resource](lnxUserResource.md)– lokale Linux-Benutzer verwaltet.




