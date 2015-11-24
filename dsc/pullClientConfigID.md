#Einrichten eines Pull-Clients mithilfe des Konfigurations-ID

> Gilt für: WindowsPowerShell 5.0

Jede Zielknoten hat angewiesen, Pull-Modus zu verwenden und erhält die URL, in dem den Pull-Server abzurufenden Konfigurationen kontaktieren kann. Zu diesem Zweck müssen Sie die lokale Configuration Manager (LCM) mit den erforderlichen Informationen zu konfigurieren. Um LCM konfigurieren zu können, erstellen Sie eine besondere Art von Konfiguration gedrosselt mit der **DSCLocalConfigurationManager** Attribut. Weitere Informationen zum Konfigurieren von LCM, finden Sie unter [konfigurieren den Konfigurations-Manager für lokale](metaConfig.md).

> **Hinweis**: Dieses Thema gilt für PowerShell 5.0. Informationen zum Einrichten von einem Pull-Client in PowerShell 4.0 finden Sie unter [Einrichten eines Pull-Clients mithilfe des Konfigurations-ID in PowerShell 4.0](pullClientConfigID4.md)

Das folgende Skript konfiguriert den LCM Pull-Konfigurationen auf einem Server mit dem Namen "CONTOSO-PullSrv".

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = 1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'

        }      
    }
}
PullClientConfigID
```

In das Skript die **ConfigurationRepositoryWeb** Block definiert den Pull-Server. Die **ServerURL**

Nachdem dieses Skript ausgeführt wird, erstellt Sie einen neuen Ausgabeordner mit dem Namen **PullClientConfigID** und Metaconfiguration MOF-Datei vorhanden. In diesem Fall wird die Metaconfiguration MOF-Datei den Namen `localhost.meta.mof`.

Aufrufen, um die Konfiguration zu übernehmen, die **Set DscLocalConfigurationManager** -Cmdlet mit der **Pfad** auf den Speicherort der Metaconfiguration MOF-Datei festgelegt. Beispiel: `Set DSCLocalConfigurationManager Localhost – Pfad. \PullClientConfigID – Verbose.`

##Konfigurations-ID

Im Skript wird das **ConfigurationID** -Eigenschaft des LCM auf eine GUID, die zuvor für diesen Zweck erstellten (erstellen Sie eine GUID, mit der **New Guid** Cmdlet). Die **ConfigurationID** wird der LCM verwendet wird, finden die entsprechende Konfiguration auf dem Server Pull. Die MOF-Konfigurationsdatei auf dem Pull-Server muss den Namen _ConfigurationID_MOF, in denen _ConfigurationID_ ist der Wert der **ConfigurationID** Eigenschaft des Zielknotens LCM.

##SMB-Pull-server

Richten Sie einen Client Pull-Konfigurationen aus einem SMB-Server verwenden Sie zum Festlegen einer **ConfigurationRepositoryShare** blockieren. In einem **ConfigurationRepositoryShare** Block, Sie geben Sie den Pfad auf dem Server durch Festlegen der **SourcePath** Eigenschaft. Die folgenden Metaconfiguration konfiguriert den Zielknoten aus einem SMB-Pull-Server mit dem Namen abrufen **SMBPullServer**.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb SMBPullServer
        {
            SourcePath = '\\SMBPullServer\PullSource'

        }     
    }
}
PullClientConfigID
```

##Ressourcen und Berichtsservern

Standardmäßig ruft der Clientknoten erforderlichen Ressourcen von und Berichten Status zum Konfigurationsserver Pull. Allerdings können Sie verschiedene Pull-Server für Ressourcen und Berichte angeben.
Um einen Ressourcenserver anzugeben, verwenden Sie entweder eine **ResourceRepositoryWeb** (für einen Pull-Webserver) oder einem **ResourceRepositoryShare** Block (für eine SMB-Pull-Server).
Um einen Berichtsserver anzugeben, verwenden Sie eine **ReportRepositoryWeb** blockieren. Ein Berichtsserver kann keinem SMB-Server sein.
Die folgenden Metaconfiguration konfiguriert einen Pull-Client, um die Konfigurationen zu erhalten **CONTOSO-PullSrv** und seine Ressourcen aus **CONTOSO-ResourceSrv**, und senden Berichte zum Status **CONTOSO-ReportSrv**.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'

        }

        ResourceRepositoryWeb CONTOSO-ResourceSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

##Weitere Informationen

* [Ein Pull-Client mit Konfigurationsnamen einrichten](pullClientConfigNames.md)



