#DSC für Linux NxFileLine Ressource

Die **NxFileLine** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus, um Zeilen in einer Konfigurationsdatei auf einem Linux-Knoten zu verwalten.

##Syntax

```
nxFileLine <string> #ResourceName
{
    FilePath = <string>
    ContainsLine = <string>
    [ DoesNotContainPattern = <string> ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| FilePath| Der vollständige Pfad zur Datei, die Zeilen auf dem Zielknoten zu verwalten.|
| ContainsLine| Eine Kommentarzeile, um sicherzustellen, die in der Datei vorhanden ist.Diese Zeile wird an die Datei angefügt werden, wenn sie in der Datei nicht vorhanden ist.**ContainsLine** ist obligatorisch, jedoch kann auf eine leere Zeichenfolge festgelegt werden ('ContainsLine = '''), wenn er nicht benötigt wird.|
| DoesNotContainPattern| Muster eines regulären Ausdrucks für Zeilen, die in der Datei nicht vorhanden ist.Für alle Zeilen, die in der Datei vorhanden sind, die diesem regulären Ausdruck übereinstimmen, wird die Zeile aus der Datei entfernt werden.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Dieses Beispiel veranschaulicht die Verwendung der **NxFileLine** Ressource so konfigurieren Sie die `/Etc/sudoer` Datei, die sicherstellen, dass des Benutzers: Monuser nicht Requiretty konfiguriert ist.

```
Import-DSCResource -Module nx 

nxFileLine DoNotRequireTTY
{
   FilePath = “/etc/sudoers”
   ContainsLine = 'Defaults:monuser !requiretty'
   DoesNotContainPattern = "Defaults:monuser[ ]+requiretty"
} 
```





