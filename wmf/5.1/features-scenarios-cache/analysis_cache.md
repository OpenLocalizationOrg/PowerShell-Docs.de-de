# Die Datei „ModuleAnalysisCache“ #

Ab Version 5.1 bietet PowerShell die folgende Kontrolle über die Datei, die zum Zwischenspeichern von Daten zu einem Modul verwendet wird, z. B. der Befehle, die es exportiert.

Standardmäßig wird dieser Cache in der Datei `${env:LOCALAPPDATA}\Microsoft\Windows\PowerShell\ModuleAnalysisCache` gespeichert.
Der Cache wird in der Regel beim Start der Suche nach einem Befehl gelesen und einige Zeit nach dem Import des Moduls in einem Hintergrundthread geschrieben.

Um den Standardspeicherort des Caches zu ändern, legen Sie die Umgebungsvariable „PSModuleAnalysisCachePath“ vor dem Starten von PowerShell fest. Änderungen an dieser Umgebungsvariablen betreffen nur untergeordnete Prozesse.
Der Wert muss einen vollständigen Pfad (samt Dateiname) definieren, in dem PowerShell die Berechtigung zum Erstellen und Schreiben von Dateien hat.
Zum Deaktivieren des Dateicaches kann dieser Wert auf einen ungültigen Speicherort festgelegt werden. Beispiel:

```PowerShell
$env:PSModuleAnalysisCachePath = 'nul'
```

Hiermit wird der Pfad auf ein ungültiges Gerät festgelegt. Es erfolgt keine Fehlermeldung, wenn PowerShell nicht in den Pfad schreiben kann, obwohl Sie mithilfe eines Ablaufverfolgungstools einen Fehlerbericht anzeigen können:

```PowerShell
Trace-Command -PSHost -Name Modules -Expression { Import-Module Microsoft.PowerShell.Management -Force }
```

Bei der Ausgabe aus dem Cache sucht PowerShell nach Modulen, die nicht mehr vorhanden sind, um einen unnötig großen Cache zu vermeiden.
Mitunter sind diese Überprüfungen nicht wünschenswert, weshalb Sie sie deaktivieren können, indem Sie Folgendes festlegen:

```PowerShell
$env:PSDisableModuleAnalysisCacheCleanup = 1
```

Das Festlegen dieser Umgebungsvariablen wird im aktuellen Prozess sofort wirksam.

<!--HONumber=Oct16_HO1-->


