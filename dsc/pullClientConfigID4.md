#Einrichten eines Pull-Clients mithilfe des Konfigurations-ID in PowerShell 4.0

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Jede Zielknoten hat angewiesen, Pull-Modus zu verwenden und erhält die URL, in dem den Pull-Server abzurufenden Konfigurationen kontaktieren kann. Zu diesem Zweck müssen Sie die lokale Configuration Manager (LCM) mit den erforderlichen Informationen zu konfigurieren. Um den LCM zu konfigurieren, erstellen Sie eine besondere Art der Konfiguration einer "Metaconfiguration" genannt. Weitere Informationen zum Konfigurieren von LCM, finden Sie unter [Windows PowerShell 4.0 Desired State Configuration lokale Configuration Manager](metaConfig4.md)

Das folgende Skript konfiguriert den LCM Pull-Konfigurationen auf einem Server mit dem Namen "PullServer".

```powershell
Configuration SimpleMetaConfigurationForPull 
{ 
    LocalConfigurationManager 
    { 
        ConfigurationID = "1C707B86-EF8E-4C29-B7C1-34DA2190AE24";
        RefreshMode = "PULL";
        DownloadManagerName = "WebDownloadManager";
        RebootNodeIfNeeded = $true;
        RefreshFrequencyMins = 15;
        ConfigurationModeFrequencyMins = 30; 
        ConfigurationMode = "ApplyAndAutoCorrect";
        DownloadManagerCustomData = @{ServerUrl = "http://PullServer:8080/PSDSCPullServer/PSDSCPullServer.svc"; AllowUnsecureConnection = “TRUE”}
    } 
} 
SimpleMetaConfigurationForPull -Output "."
```

Im Skript **DownloadManagerCustomData** übergibt die URL des Servers Pull und (für dieses Beispiel) ermöglicht eine unsichere Verbindung.

Nach der Ausführung dieses Skripts erstellt einen neuen Ausgabeordner namens **SimpleMetaConfigurationForPull** und Metaconfiguration MOF-Datei vorhanden.

Verwenden Sie zum Anwenden der Konfigurations **Set DscLocalConfigurationManager** mit Parametern für **ComputerName** (verwenden Sie "Localhost") und **Pfad** (der Pfad zum Speicherort der Datei für den Zielknoten localhost.meta.mof). Beispiel:
```powershell
Set-DSCLocalConfigurationManager –ComputerName localhost –Path . –Verbose.
```

##Konfigurations-ID

Im Skript wird das **ConfigurationID** -Eigenschaft des LCM auf eine GUID, die zuvor für diesen Zweck erstellten (erstellen Sie eine GUID, mit der **New Guid** Cmdlet). Die **ConfigurationID** wird der LCM verwendet wird, finden die entsprechende Konfiguration auf dem Server Pull. Die MOF-Konfigurationsdatei auf dem Pull-Server muss den Namen `ConfigurationID.mof`, wobei *ConfigurationID* ist der Wert der **ConfigurationID** Eigenschaft des Zielknotens LCM.




