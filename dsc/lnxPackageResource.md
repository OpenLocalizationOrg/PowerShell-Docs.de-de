#DSC für Linux NxPackage Ressource

Die **NxPackage** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von Paketen auf einem Linux-Knoten.

##Syntax

```
nxPackage <string> #ResourceName
{
    Name = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ PackageManager = <string> { Yum | Apt | Zypper } ]
    [ PackageGroup = <bool>]
    [ Arguments = <string> ]
    [ ReturnCode = <uint32> ]
    [ LogPath = <string> ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Der Name des Pakets zu einen bestimmten Zustand sichergestellt werden soll.|
| Stellen Sie sicher| Bestimmt, ob zum Überprüfen, ob das Paket vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass das Paket vorhanden ist.Bei Einstellung auf "Abwesend", um sicherzustellen, dass das Paket nicht vorhanden ist.Der Standardwert ist "Present".|
| PackageManager| Unterstützte Werte sind "Yum", "apt" und "Zypper".Gibt den Paket-Manager beim Installieren von Paketen verwenden.Wenn **FilePath** angegeben ist, wird der angegebene Pfad zum Installieren des Pakets verwendet werden.Andernfalls wird ein Paket-Manager zum Installieren des Pakets aus einem vorab konfigurierten Repository verwendet.Wenn weder **PackageManager** noch **FilePath** werden bereitgestellt, die Standard-Paket-Manager für das System verwendet wird.|
| FilePath| Der Pfad, in dem das Paket gespeichert ist|
| PackageGroup| Wenn **$true**, den **Namen** wird erwartet, dass der Name einer Gruppe Paket für die Verwendung mit ist ein **PackageManager**.**PacakgeGroup** ist nicht gültig, wenn die Bereitstellung einer **FilePath**.|
| Argumente| Eine Zeichenfolge von Argumenten, die das Paket genau wie angegeben übergeben wird.|
| Rückgabecode| Der erwartete zurück Code.Wenn Sie den tatsächlichen Code zurück, entspricht nicht dem erwarteten Wert, finden Sie hier die Konfiguration einen Fehler zurück.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Im folgende Beispiel wird sichergestellt, dass das Paket mit dem Namen "Httpd" auf einem Linux-Computer mit dem "Yum" Paket-Manager installiert ist.

```
Import-DSCResource -Module nx 

Node $node {
nxPackage httpd
{
    Name = "httpd"
    Ensure = "Present"
    PackageManager = "Yum"
}
}
```




