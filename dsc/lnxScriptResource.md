#DSC für Linux NxScript Ressource

Die **NxScript** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Ausführen von Linux-Skripts auf einem Linux-Knoten.

##Syntax

```
nxScript <string> #ResourceName
{
    GetScript = <string>
    SetScript = <string>
    TestScript = <string>
    [ User = <string> ]
    { Group = <string> ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| GetScript| Stellt ein Skript, das ausgeführt, beim Aufrufen wird der [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521625.aspx) Cmdlet.Das Skript muss mit einem Shebang, z. B. # beginnen! / bin/bash.|
| SetScript| Bietet ein Skript.Beim Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) -Cmdlet der **TestScript** zuerst ausgeführt.Wenn die **TestScript** Block gibt einen Exitcode ungleich 0, die **SetScript** Block ausgeführt wird.Wenn die **TestScript** gibt einen Exitcode von 0, die **SetScript** kann nicht ausgeführt werden.Das Skript muss mit einem Shebang beginnen, wie z. B. `#! / bin/bash`.|
| TestScript| Bietet ein Skript.Beim Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) -Cmdlet dieses Skript ausgeführt wird.Wenn sie einen Exitcode ungleich 0 zurückgibt, wird der SetScript ausgeführt.Wenn es einen Exitcode von 0 ist, gibt die **SetScript** kann nicht ausgeführt werden.Die **TestScript** führt auch beim Aufrufen der [Test DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx) Cmdlet.In diesem Fall die **SetScript** wird nicht ausgeführt, unabhängig davon, welche Beendigungscode aus der **TestScript**.Die **TestScript** müssen einen Exitcode von 0 zurückgegeben, wenn die tatsächliche Konfiguration entspricht der aktuellen Konfigurations für den gewünschten Status, einen Exitcode als 0, wenn er nicht übereinstimmt.(Die aktuelle Konfiguration für den gewünschten Status ist die letzte Konfiguration in Kraft gesetzt, auf den Knoten, der DSC verwendet).Das Skript muss mit einem Shebang, wie z. B. 1#!/bin/bash.1 beginnen.|
| Benutzer| Der Benutzer zum Ausführen des Skripts als.|
| Gruppe| Zum Ausführen des Skripts als Gruppe.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Das folgende Beispiel zeigt die Verwendung der **NxScript** Ressource zusätzliche Konfigurationsschritte ausführen.

```
Import-DSCResource -Module nx 

Node $node {
nxScript KeepDirEmpty{

    GetScript = @"
#!/bin/bash
ls /tmp/mydir/ | wc -l
"@

    SetScript = @"
#!/bin/bash
rm -rf /tmp/mydir/*
"@

    TestScript = @'
#!/bin/bash
filecount=`ls /tmp/mydir | wc -l`
if [ $filecount -gt 0 ]
then
    exit 1
else
    exit 0
fi
'@
} 
}
```




