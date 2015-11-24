#Einführung von Konfigurationen

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Es gibt zwei Möglichkeiten, PowerShell gewünscht State Configuration (DSC) Konfigurationen umzusetzen: push und Pull Modus.

##Push-Modus

![Push-Modus](images/Push.png "How push mode works")

Push-Modus bezieht sich auf einen Benutzer, die aktiv eine Konfiguration mit einem Zielknoten anwenden, durch Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) Cmdlet.

Nach dem Erstellen und Kompilieren eine Konfiguration, Sie können in Gang zu setzen im Push-Modus durch Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) festlegen-Cmdlet - Path-Parameter des Cmdlets der Pfad, in dem die MOF-Konfiguration. Die MOF-Konfiguration beispielsweise Locted auf `C:\DSC\Configurations\localhost.mof`, würden Sie sie anwenden, auf dem lokalen Computer mit den folgenden Befehl aus:
`Start DscConfiguration-Pfad 'C:\DSC\Configurations'`

> __Hinweis__: Standardmäßig wird DSC eine Konfiguration als Hintergrundauftrag ausgeführt. Rufen Sie zum interaktiven Ausführen die Konfiguration der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) mit der __-Warten__ Parameter.

##Pull-Modus

![Pull-Modus](images/Pull.png "How pull mode works")
Im Pull-Modus sind Pull-Clients erhalten die gewünschten Status Konfigurationen von einem remote-Pull-Server konfiguriert. Entsprechend der Pull-Server Dienst hosten die DSC eingerichtet wurde, und mit der Konfigurationen und Ressourcen, die von den Pull-Clients bereitgestellt wurde.
Jede Pull-Clients verfügt über eine geplante Aufgabe, die eine Überprüfung regelmäßig für die Konfiguration des Knotens ausführt. Beim Auslösen des Ereignisses beim ersten wird die lokale Configuration Manager (LCM) auf dem Pull-Client, um die Konfiguration zu überprüfen. Wenn der Pull-Client, als konfiguriert ist die gewünschte, geschieht nichts. Andernfalls wird die LCM Anforderung an dem Pull-Server zu eine bestimmte Konfiguration zu erhalten. Wenn diese Konfiguration auf die Pull-Server vorhanden ist, und anfängliche Überprüfung wird übergibt, wird die Konfiguration an den Client Pull übertragen, wo sie dann durch den LCM ausgeführt wird.

In den folgenden Themen wird erläutert, wie Pull-Servern und -Clients eingerichtet werden:

- [Ein Pull-Webserver einrichten](pullServer.md)
- [Ein SMB-Pull-Server einrichten](pullServerSMB.md)
- [Konfigurieren eines Pull-Clients](pullClientConfigID.md)



