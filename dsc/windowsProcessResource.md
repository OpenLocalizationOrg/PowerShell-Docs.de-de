#DSC WindowsProcess Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die **WindowsProcess** Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Konfigurieren von Prozessen auf einem Zielknoten.

##Syntax

```
WindowsProcess [string] #ResourceName
{
    Arguments = [string]
    Path = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ DependsOn = [string[]] ]
    [ StandardErrorPath = [string] ]
    [ StandardInputPath = [string] ]
    [ StandardOutputPath = [string] ]
    [ WorkingDirectory = [string] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Argumente| Gibt eine Zeichenfolge an den Prozess zu übergebenden Argumente-ist.Wenn Sie mehrere Argumente übergeben müssen, können platzieren sie alle in dieser Zeichenfolge.|
| Pfad| Gibt den Pfad zur ausführbaren Datei Prozess.Wenn Sie diese Eigenschaft auf den Namen der ausführbaren Datei festlegen, suchen DSC in der __Pfad__ Variable.Wenn Sie einen vollständig qualifizierten Domänennamen zuweisen, der Prozess muss vorhanden sein gibt es da DSC nicht überprüft die __Pfad__ Variable in diesem Fall.|
| Credential| Gibt die Anmeldeinformationen für das Starten des Prozesses an.|
| Stellen Sie sicher| Gibt an, ob der Prozess vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass der Prozess vorhanden ist.Legen Sie sie andernfalls auf "Abwesend".Der Standardwert ist "Present".|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist "DependsOn ="[ResourceType] ResourceName"".|
| StandardErrorPath| Gibt den Verzeichnispfad ein, um den Standardfehler zu schreiben.Gibt es eine vorhandene Datei wird überschrieben.|
| StandardInputPath| Gibt den Standardspeicherort für die Eingabe an.|
| StandardOutputPath| Gibt den Speicherort an die Standardausgabe zu schreiben.Gibt es eine vorhandene Datei wird überschrieben.|
| WorkingDirectory| Gibt den Speicherort an, der das aktuelle Arbeitsverzeichnis für den Prozess verwendet wird.|




