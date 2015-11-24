#DSC Archiv-Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Archiv-Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Archiv (ZIP) Dateien zu entpacken.

##Syntax

```MOF
Archive [string] #ResourceName
{
    Destination = [string]
    Path = [string]
    [ Checksum = [string] { CreatedDate | ModifiedDate | SHA-1 | SHA-256 | SHA-512 }  ]
    [ DependsOn = [string[]] ]
    [ Ensure = [string] { Absent | Present }  ]
   [Force = [bool] ]
    [ Validate = [bool] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Destination| Gibt den Speicherort an, wobei Sie sicherstellen möchten, dass der Archiv-Inhalt extrahiert werden.|
| Pfad| Gibt den Quellpfad der Archivdatei.|
| __Prüfsumme__| Definiert den Typ zu verwenden, wenn Sie bestimmen, ob zwei Dateien identisch sind.Wenn __Checksum__ nicht angegeben ist, werden nur die Datei- oder Verzeichnisnamens für den Vergleich verwendet.Gültige Werte sind: SHA-1, SHA-256, SHA-512, CreatedDate, ModifiedDate, keine (Standard).Bei Angabe von __Checksum__ ohne __Validate__, die Konfiguration fehl.|
| Stellen Sie sicher| Bestimmt, ob zum Überprüfen, ob der Inhalt des Archivs auf vorhanden ist die __Ziel__.Legen Sie diese Eigenschaft auf __vorhanden__ um sicherzustellen, dass der Inhalt vorhanden sein.Legen Sie ihn auf __Abwesend__ um sicherzustellen, dass sie nicht vorhanden sind.Der Standardwert ist __vorhanden__.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Beispielsweise die ID des Ressource-Skriptblocks Konfiguration, die Sie ausführen möchten zuerst ResourceName und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| Überprüfen| Verwendet die Eigenschaft Prüfsumme um zu bestimmen, ob das Archiv die Signatur übereinstimmt.Bei Angabe von Prüfsumme, ohne zu überprüfen, schlägt die Konfiguration fehl.Wenn Sie überprüfen, ohne die Prüfsumme angeben, wird standardmäßig eine Prüfsumme SHA-256 verwendet.|
| Force| Bestimmte Dateioperationen (z. B. eine Datei überschreiben oder Löschen eines Verzeichnisses, das nicht leer ist), führt zu einem Fehler.Verwenden die Force-Eigenschaft überschreibt solche Fehler.Der Standardwert ist „False“.|

##Beispiel

Das folgende Beispiel zeigt, wie die Archiv-Ressource zu verwenden, um sicherzustellen, dass der Inhalt der Archivdatei namens Test.zip vorhanden und an ein bestimmtes Ziel extrahiert werden.

```
Archive ArchiveExample {
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Path = "C:\Users\Public\Documents\Test.zip"
    Destination = "C:\Users\Public\Documents\ExtractionPath"
} 
```




