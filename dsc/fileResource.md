#DSC Dateiressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von Dateien und Ordnern auf dem Zielknoten.

##Syntax

```
File [string] #ResourceName
{
    DestinationPath = [string]
    [ Attributes = [string[]] { Archive | Hidden | ReadOnly | System }]
    [ Checksum = [string] { CreatedDate | ModifiedDate | SHA-1 | SHA-256 | SHA-512 } ]
    [ Contents = [string] ]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present } ] 
    [ Force = [bool] ]
    [ Recurse = [bool] ]
    [ DependsOn = [string[]] ]
    [ SourcePath = [string] ]
    [ Type = [string] { Directory | File } ] 
    [ MatchSource = [bool] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Zielpfad| Gibt den Speicherort an, um sicherzustellen, dass den Status für eine Datei oder ein Verzeichnis werden soll.|
| Attribute| Gibt den gewünschten Status der Attribute für die Datei oder das Verzeichnis an.|
| Prüfsumme| Gibt die Prüfsumme zu verwenden, wenn Sie bestimmen, ob zwei Dateien identisch sind.Wenn __Checksum__ nicht angegeben ist, werden nur die Datei- oder Verzeichnisnamens für den Vergleich verwendet.Gültige Werte sind: SHA-1, SHA-256, SHA-512, CreatedDate, ModifiedDate.|
| Inhalt| Gibt den Inhalt einer Datei, z. B. eine bestimmte Zeichenfolge.|
| Credential| Gibt die Anmeldeinformationen, die erforderlich, um Zugriff auf Ressourcen, wie z. B. Quelldateien sind an, wenn ein solcher Zugriff erforderlich ist.|
| Stellen Sie sicher| Gibt an, ob die Datei oder das Verzeichnis vorhanden ist.Legen Sie diese Eigenschaft auf "Abwesend", um sicherzustellen, dass die Datei oder das Verzeichnis nicht vorhanden ist.Bei Einstellung auf "Present", um sicherzustellen, dass die Datei oder das Verzeichnis vorhanden ist.Der Standardwert ist "Present".|
| Force| Bestimmte Dateioperationen (z. B. eine Datei überschreiben oder Löschen eines Verzeichnisses, das nicht leer ist), führt zu einem Fehler.Verwenden die Force-Eigenschaft überschreibt solche Fehler.Der Standardwert ist __$false__.|
| Rekursion| Gibt an, ob Unterverzeichnisse enthalten sind.Legen Sie diese Eigenschaft auf __$true__ um anzugeben, dass die Unterverzeichnisse enthalten sein soll.Der Standardwert ist __$false__.**Hinweis**: Diese Eigenschaft ist nur gültig, wenn die Type-Eigenschaft auf Verzeichnis festgelegt ist.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| Quellpfad| Gibt den Pfad, aus dem die Ressource Datei oder einen Ordner kopiert.|
| Typ| Gibt an, ob die Ressource konfiguriert wird ein Verzeichnis oder eine Datei ist.Legen Sie diese Eigenschaft auf "Verzeichnis", um anzugeben, dass die Ressource ein Verzeichnis ist.Legen sie auf "Datei", um anzugeben, dass es sich bei der Ressource um eine Datei handelt.Der Standardwert ist "File".|
| MatchSource| Wenn der Standardwert von __$false__, und klicken Sie dann alle Dateien in der Quelle (z. B. die Dateien A, B und C) an das Ziel beim ersten hinzugefügt werden die Konfiguration angewendet wird.Wenn die Quelle eine neue Datei (D) hinzugefügt wird, wird es nicht an das Ziel hinzugefügt werden, selbst wenn die Konfiguration später erneut angewendet wird.Wenn der Wert __$true__, und klicken Sie dann jedes Mal die Konfiguration angewendet wird, neue Dateien anschließend die Quelle (z. B. Datei D in diesem Beispiel) an das Ziel hinzugefügt werden.|

##Beispiel

Das folgende Beispiel zeigt, wie die Ressource verwendet wird, um sicherzustellen, dass ein Verzeichnis mit dem Pfad `C:\Users\Public\Documents\DSCDemo\DemoSource` für eine Datenquelle (z. B. dem Server "Pull") steht außerdem vorhanden ist (und alle Unterverzeichnisse) auf dem Zielknoten. Außerdem schreibt eine bestätigendes Meldung in das Protokoll, wenn dies abgeschlossen ist, und enthält eine Anweisung aus, um sicherzustellen, dass der Datei überprüfen-Vorgang ausgeführt, bevor der Protokollierung wird.

```powershell
Configuration FileResourceDemo
{
    Node "localhost"
    {
        File DirectoryCopy
        {
            Ensure = "Present"  # You can also set Ensure to "Absent"
            Type = "Directory" # Default is "File".
            Recurse = $true # Ensure presence of subdirectories, too
            SourcePath = "C:\Users\Public\Documents\DSCDemo\DemoSource"
            DestinationPath = "C:\Users\Public\Documents\DSCDemo\DemoDestination"    
        }

        Log AfterDirectoryCopy
        {
            # The message below gets written to the Microsoft-Windows-Desired State Configuration/Analytic log
            Message = "Finished running the file resource with ID DirectoryCopy"
            DependsOn = "[File]DirectoryCopy" # This means run "DirectoryCopy" first.
        }
    }
}
```




