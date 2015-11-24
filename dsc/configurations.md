#DSC-Konfigurationen

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

DSC-Konfigurationen werden PowerShell-Skripts, die eine besondere Art von Funktion zu definieren. 
Um eine Konfiguration zu definieren, verwenden Sie das PowerShell-Schlüsselwort __Konfiguration__.

```powershell
Configuration MyDscConfiguration {

    Node “TEST-PC1” {
        WindowsFeature MyFeatureInstance {
            Ensure = “Present”
            Name =  “RSAT”
        }
        WindowsFeature My2ndFeatureInstance {
            Ensure = “Present”
            Name = “Bitlocker”
        }
    }
}
```

Speichern Sie das Skript als PS1-Datei.

##Configuration-syntax

Ein Skript besteht aus den folgenden Teilen:

- Die **Konfiguration** blockieren. Dies ist die äußerste Skriptblock. Definieren sie mithilfe der **Konfiguration** -Schlüsselwort und einen Namen angeben. In diesem Fall ist der Name der Konfiguration "MyDscConfiguration".
- Eine oder mehrere **Knoten** blockiert. Diese definieren die Knoten (Computer oder virtuelle Computer), die Sie konfigurieren. In der Konfiguration oben ist eine **Knoten** blockieren, die auf einen Computer mit dem Namen "TEST-PC1".
- Mindestens eine Ressource blockiert. Dies ist, in dem die Konfiguration die Eigenschaften für die Ressourcen festlegt, die ihm konfiguriert wird. In diesem Fall stehen zwei Ressource-Blöcken, von denen jeder die Ressource "WindowsFeature" aufrufen.

Innerhalb einer **Konfiguration** Block, führen Sie alle Elemente, die Sie normalerweise in einer PowerShell-Funktion konnte. Beispielsweise im vorherigen Beispiel, konnte Wenn Sie nicht den Namen des Zielcomputers in der Konfiguration fest codieren möchten Sie einen Parameter für den Knotennamen hinzufügen:

```powershell
Configuration MyDscConfiguration {

    param(
        [string[]]$computerName="localhost"
    )
    Node $computerName {
        WindowsFeature MyFeatureInstance {
            Ensure = “Present”
            Name =  “RSAT”
        }
        WindowsFeature My2ndFeatureInstance {
            Ensure = “Present”
            Name = “Bitlocker”
        }
    }
}
```

In diesem Beispiel geben Sie den Namen des Knotens, indem Sie es als $computerName-Parameter übergeben wenn Sie [Kompilieren der Konfigurationsdateiunterschiede] (#, Kompilieren die Konfiguration). Der Name der Standardwert ist "Localhost".

##Kompilieren die Konfiguration

Bevor Sie eine Konfiguration in Gang zu setzen können, müssen Sie es in eine MOF-Dokument kompiliert. Hierzu müssen Sie die Konfiguration aufrufen, wie Sie eine PowerShell-Funktion.
> __Hinweis:__ zum Aufrufen einer Konfigurations die Funktion im globalen Bereich (wie bei jedem anderen PowerShell-Funktion) werden muss. Können Sie diese geschieht entweder durch "Punkt-sourcing" des Skripts vornehmen oder durch Ausführen des Konfigurationsskripts mithilfe von F5, oder klicken auf die __Skript ausführen__ Schaltfläche in der ISE. Punkt-Quelle das Skript, führen Sie den Befehl ". .\myConfig.ps1`  `myConfig.ps1 "ist der Name der Skriptdatei, die die Konfiguration enthält.

Beim Aufrufen der Konfigurations erstellt:

- Ein Ordner im aktuellen Verzeichnis mit dem gleichen Namen wie die Konfiguration.
- Eine Datei namens _NodeName_MOF in das neue Verzeichnis, in dem _NodeName_ ist der Name des Zielknotens der Konfiguration. Wenn mehrere Knoten vorhanden sind, wird eine MOF-Datei für jeden Knoten erstellt.

> __Hinweis__: das MOF-Datei enthält alle Konfigurationsinformationen für den Zielknoten. Aus diesem Grund ist es wichtig, um es zu schützen. Weitere Informationen finden Sie unter [Sichern die MOF-Datei](secureMOF.md).

Kompilieren die erste Konfiguration oberhalb der Ergebnisse in die folgende Ordnerstruktur:

```powershell
PS C:\users\default\Documents\DSC Configurations> . .\MyDscConfiguration.ps1
PS C:\users\default\Documents\DSC Configurations> MyDscConfiguration
    Directory: C:\users\default\Documents\DSC Configurations\MyDscConfiguration
Mode                LastWriteTime         Length Name                                                                                              
----                -------------         ------ ----                                                                                         
-a----       10/23/2015   4:32 PM           2842 TEST-PC1.mof
```

Wenn die Konfiguration einen Parameter, wie im zweiten Beispiel, die zum Zeitpunkt der Kompilierung angegeben werden akzeptiert. Hier ist wie die, die aussehen würde:

```powershell
PS C:\users\default\Documents\DSC Configurations> . .\MyDscConfiguration.ps1
PS C:\users\default\Documents\DSC Configurations> MyDscConfiguration -computerName 'MyTestNode'
    Directory: C:\users\default\Documents\DSC Configurations\MyDscConfiguration
Mode                LastWriteTime         Length Name                                                                                              
----                -------------         ------ ----                                                                                         
-a----       10/23/2015   4:32 PM           2842 MyTestNode.mof
```

##DependsOn verwenden

Eine nützliche DSC-Schlüsselwort ist __DependsOn__. In der Regel (jedoch nicht immer unbedingt), DSC angewendet wird, die Ressourcen in der Reihenfolge, die sie in der Konfiguration angezeigt werden. Allerdings __DependsOn__ gibt an, welche Ressourcen von anderen Ressourcen abhängig sind und LCM wird sichergestellt, dass sie in der richtigen Reihenfolge, unabhängig von der Reihenfolge angewendet werden, in denen Ressourcen Instanzen definiert sind. Z. B. eine Konfiguration gibt möglicherweise an, die einer Instanz von der __Benutzer__ Ressource abhängig ist, auf das Vorhandensein einer __Gruppe__ Instanz:

```powershell
Configuration DependsOnExample {
    Node Test-PC1 {
        Group GroupExample {
            Ensure = “Present”
            GroupName = “TestGroup”
        }
User UserExample {
Ensure = “Present”
FullName = “TestUser”
DependsOn = “GroupExample”
}
    }
}
```

##Neue Ressourcen verwenden in Ihrer Konfiguration

Wenn Sie die vorherigen Beispielen ausgeführt haben, vielleicht Sie bemerkt, dass Sie darauf hingewiesen wurden, dass Sie eine Ressource verwendet haben, ohne diesen explizit importieren.
Heute ist im Lieferumfang DSC 12 Ressourcen als Teil des PSDesiredStateConfiguration-Moduls. Andere Ressourcen in externen Modulen müssen eingefügt werden, `$env: PSModulePath` um LCM erkannt zu werden. Neue Cmdlet [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx), kann verwendet werden, um zu bestimmen, welche Ressourcen von den LCM auf dem System installiert und für die Verwendung verfügbar sind. 
Sobald diese Module in platziert wurden `$env: PSModulePath` von ordnungsgemäß erkannt [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx), dennoch müssen Sie in Ihrer Konfiguration geladen werden. __Import DscResource__ ist eine dynamic-Schlüsselwort, das nur in erkannt wird ein __Konfiguration__ Block (d. h. es ist kein Cmdlet). __Import DscResource__ unterstützt zwei Parameter:
* __ModuleName__ ist die empfohlene Verwendung __Import DscResource__. Er akzeptiert den Namen des Moduls, das die Ressourcen (sowie ein Zeichenfolgenarray Modulnamen) importiert werden.
* __Name__ ist der Name der Ressource zu importieren. Dies ist nicht der Anzeigename, der als "Name" zurückgegeben werden, indem [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx), aber der Klassennamen verwendet werden, wenn das Ressourcenschema definiert (als zurückgegeben __ResourceType__ von [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx)).

##Weitere Informationen

* [WindowsPowerShell gewünschten Status (Übersicht)](overview.md)
* [DSC-Ressourcen](resources.md)
* [Konfigurieren des lokalen Konfigurations-Managers](metaconfig.md)



