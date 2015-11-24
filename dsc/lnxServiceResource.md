#DSC für Linux NxService Ressource

Die **NxService** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von Diensten auf einem Linux-Knoten.

##Syntax

```
nxService <string> #ResourceName
{
    Name = <string>
    [ Controller = <string> { init | upstart | system }  ]
    [ Enambled = <bool> ]
    [ State = <string> { Running | Stopped } ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Der Name der Service/Daemon konfigurieren.|
| Controller| Der Typ des Dienst-Controllers beim Konfigurieren des Diensts verwendet werden soll.|
| Aktiviert| Gibt an, ob der Dienst beim Systemstart gestartet wird.|
| Status| Gibt an, ob der Dienst ausgeführt wird.Legen Sie diese Eigenschaft auf "Beendet", um sicherzustellen, dass der Dienst nicht ausgeführt wird.Legen sie auf "Ausführen", um sicherzustellen, dass der Dienst nicht ausgeführt wird.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|


##Weitere Informationen

Die **NxService** Ressource wird nicht erstellt eine Dienstdefinition oder ein Skript für den Dienst ist nicht vorhanden. Können Sie die Konfiguration des PowerShell gewünscht **NxFile** Resource-Ressource, um die Existenz oder den Inhalt der Datei oder des Skripts verwalten.

##Beispiel

Das folgende Beispiel zeigt die Konfiguration des Dienstes "Httpd" (für Apache HTTP Server), registriert das **SystemD** Dienstcontroller.

```
Import-DSCResource -Module nx 

Node $node {
#Apache Service
nxService ApacheService 
{
Name = "httpd"
State = "running"
Enabled = $true
Controller = "systemd"
}
}
```




