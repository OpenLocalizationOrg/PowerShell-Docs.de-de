#DSC für Linux NxArchive Ressource

Die **NxArchive** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Entpacken (TAR, ZIP) Archivdateien an einen bestimmten Pfad auf einem Linux-Knoten.

##Syntax

```
nxArchive <string> #ResourceName
{
    SourcePath = <string>
    DestinationPath = <string>
    [ Checksum = <string> { ctime | mtime | md5 }  ]
    [ Force = <bool> ]
    [ DependsOn = <string[]> ]
    [ Ensure = <string> { Absent | Present }  ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Quellpfad| Gibt den Quellpfad der Archivdatei.Dies dürfte eine .tar ZIP- oder. weswegen-Datei.|
| Zielpfad| Gibt den Speicherort an, wobei Sie sicherstellen möchten, dass der Archiv-Inhalt extrahiert werden.|
| Prüfsumme| Definiert den Typ zu verwenden, wenn Sie feststellen, ob das Archiv Quelle aktualisiert wurde.Werte sind: "Ctime", "Mtime", oder "md5".Der Standardwert ist "md5".|
| Force| Bestimmte Dateioperationen (z. B. eine Datei überschreiben oder Löschen eines Verzeichnisses, das nicht leer ist), führt zu einem Fehler.Mithilfe der **Kraft** Eigenschaft überschreibt diese Fehler.Der Standardwert ist **$false**.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| Stellen Sie sicher| Bestimmt, ob zum Überprüfen, ob der Inhalt des Archivs auf vorhanden ist die **Ziel**.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass der Inhalt vorhanden sein.Bei Einstellung auf "Abwesend", um sicherzustellen, dass sie nicht vorhanden sind.Der Standardwert ist "Present".|

##Beispiel

Das folgende Beispiel zeigt, wie Sie die **NxArchive** Ressource, um sicherzustellen, dass der Inhalt einer Archivdatei aufgerufen `website.tar` vorhanden und werden an ein bestimmtes Ziel extrahiert.

```
Import-DSCResource -Module nx 

nxFile SyncArchiveFromWeb
{
   Ensure = "Present"
   SourcePath = “http://release.contoso.com/releases/website.tar”
   DestinationPath = "/usr/release/staging/website.tar"
   Type = "File"
   Checksum = “mtime”
}

nxArchive SyncWebDir
{
   SourcePath = “/usr/release/staging/website.tar”
   DestinationPath = “/usr/local/apache2/htdocs/”
   Force = $false
   DependsOn = "[nxFile]SyncArchiveFromWeb"
} 
```




