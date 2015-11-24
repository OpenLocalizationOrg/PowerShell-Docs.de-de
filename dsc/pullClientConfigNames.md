#Einrichten eines Konfigurationsnamen mit Pull-Clients

> Gilt für: WindowsPowerShell 5.0

Jede Zielknoten hat angewiesen, Pull-Modus zu verwenden und erhält die URL, in dem den Pull-Server abzurufenden Konfigurationen kontaktieren kann. Zu diesem Zweck müssen Sie die lokale Configuration Manager (LCM) mit den erforderlichen Informationen zu konfigurieren. Um den LCM zu konfigurieren, erstellen Sie eine besondere Art von Konfiguration, versehen mit dem **DSCLocalConfigurationManager** Attribut. Weitere Informationen zum Konfigurieren von LCM, finden Sie unter [konfigurieren den Konfigurations-Manager für lokale](metaConfig.md).

> **Hinweis**: Dieses Thema gilt für PowerShell 5.0. Informationen zum Einrichten von einem Pull-Client in PowerShell 4.0 finden Sie unter [Einrichten eines Pull-Clients mithilfe des Konfigurations-ID in PowerShell 4.0](pullClientConfigID4.md)

Das folgende Skript konfiguriert den LCM Pull-Konfigurationen auf einem Server mit dem Namen "CONTOSO-PullSrv":

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
            ConfigurationNames = @('ClientConfig')

        }      
    }
}
PullClientConfigID
```

In das Skript die **ConfigurationRepositoryWeb** Block definiert den Pull-Server. Die **ServerURL** -Eigenschaft gibt den Endpunkt für den Pull-Server.

Die **RegistrationKey** -Eigenschaft ist ein gemeinsam verwendeten Schlüssel zwischen allen Clientknoten für einen Pull-Server und dem Pull-Server. Der gleiche Wert wird in einer Datei auf dem Pull-Server gespeichert. Die **ConfigurationNames** -Eigenschaft gibt den Namen der Konfiguration für den Clientknoten vorgesehen. Auf dem Pull-Server, die MOF Konfigurationsdatei für diesen Clientknoten genannt werden, muss *ConfigurationNames*MOF, in denen *ConfigurationNames* entspricht dem Wert der **ConfigurationNames** Eigenschaftensatz in dieser Metaconfiguration.

Nachdem dieses Skript ausgeführt wird, erstellt Sie einen neuen Ausgabeordner mit dem Namen **PullClientConfigID** und Metaconfiguration MOF-Datei vorhanden. In diesem Fall wird die Metaconfiguration MOF-Datei den Namen `localhost.meta.mof`.

Aufrufen, um die Konfiguration zu übernehmen, die **Set DscLocalConfigurationManager** -Cmdlet mit der **Pfad** auf den Speicherort der Metaconfiguration MOF-Datei festgelegt. Beispiel: `Set DSCLocalConfigurationManager Localhost – Pfad. \PullClientConfigID – Verbose.`

> **Hinweis**: Registrierungsschlüssel funktionieren nur mit Pull-Webservern. Sie müssen nach wie vor verwenden, **ConfigurationID** mit einem SMB-Pull-Server. Informationen zum Konfigurieren von einem Pull-Server mithilfe von **ConfigurationID**, finden Sie unter [Einrichten eines Pull-Clients mithilfe des Konfigurations-ID](pullClientConfigID.md)

##Ressourcen und Berichtsservern

Standardmäßig ruft der Clientknoten erforderlichen Ressourcen von und Berichten Status zum Konfigurationsserver Pull. Allerdings können Sie verschiedene Pull-Server für Ressourcen und Berichte angeben.
Um einen Ressourcenserver anzugeben, verwenden Sie entweder eine **ResourceRepositoryWeb** (für einen Pull-Webserver) oder einem **ResourceRepositoryShare** Block (für eine SMB-Pull-Server).
Um einen Berichtsserver anzugeben, verwenden Sie eine **ReportRepositoryWeb** blockieren. Ein Berichtsserver kann keinem SMB-Server sein.
Die folgenden Metaconfiguration konfiguriert einen Pull-Client, um die Konfigurationen zu erhalten **CONTOSO-PullSrv** und seine Ressourcen aus **CONTOSO-ResourceSrv**, und senden Berichte zum Status **CONTOSO-ReportSrv**:

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = 'fbc6ef09-ad98-4aad-a062-92b0e0327562'
        }

        ResourceRepositoryWeb CONTOSO-ResourceSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '30ef9bd8-9acf-4e01-8374-4dc35710fc90'
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

* [Einrichten eines Pull-Clients mit dem Konfigurations-ID](pullClientConfigID.md)
* [Das Einrichten eines DSC Pull-Webservers](pullServer.md)




