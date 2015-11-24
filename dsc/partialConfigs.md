#Konfiguration des PowerShell gewünscht teilweise Konfigurationen

> Gilt für: WindowsPowerShell 5.0

In PowerShell 5.0 ermöglicht gewünscht State Configuration (DSC) in Fragmente und aus mehreren Quellen übermittelt werden. Die lokale Configuration Manager (LCM) auf dem Zielknoten setzt die Fragmente zusammen vor der Anwendung als eine einzelne Konfiguration. Diese Funktion ermöglicht die Steuerung der Konfiguration zwischen Teams oder Personen freigeben. Z. B., wenn zwei oder mehr Teams aus Entwicklern von einem Dienst zusammenarbeiten, können sie jede Konfigurationen zum Verwalten ihrer Teil des Diensts erstellen möchten. Jede dieser Konfigurationen von anderen Pull-Servern abgerufen werden konnte, und in verschiedenen Phasen der Entwicklung hinzugefügt werden konnte. Teilweise Konfigurationen können auch verschiedene Personen oder Teams verschiedene Aspekte der Knoten konfigurieren, ohne zu koordinieren, ein einzelnes Dokument bearbeiten. Ein Team kann z. B., verantwortlich für das Bereitstellen von virtuellen Computer und Betriebssystem, während ein anderes Team auch auf diesem virtuellen Computer bereitgestellt werden kann. Mit partiellen Konfigurationen kann jedes Team eine eigene Konfiguration, ohne diese unnötig kompliziert werden erstellen.

Sie können partielle Konfigurationen im Push-Modus, Pull-Modus oder eine Kombination aus beiden verwenden.

##Teilweise Konfigurationen im Push-Modus

Um teilweise Konfigurationen im Push-Modus zu verwenden, konfigurieren Sie den LCM auf dem Zielknoten die partiellen Konfigurationen zu erhalten. Jede partielle Konfiguration muss an das Ziel mit dem Cmdlet veröffentlichen DSCConfiguration abgelegt werden. Der Zielknoten kombiniert dann die partielle Konfiguration in eine einzelne und wenden Sie die Konfiguration durch Aufrufen der [Start DscConfigurationxt](https://technet.microsoft.com/en-us/library/dn521623.aspx) Cmdlet.

###Konfigurieren den LCM für partielle Push-Modus-Konfigurationen

Um den LCM für partielle Konfigurationen im Push-Modus zu konfigurieren, erstellen Sie eine **DSCLocalConfigurationManager** Konfiguration mit einem **PartialConfiguration** Block für jede partielle Konfiguration. Weitere Informationen zum Konfigurieren von LCM, finden Sie unter [Windows konfigurieren den Konfigurations-Manager für lokale](https://technet.microsoft.com/en-us/library/mt421188.aspx). Das folgende Beispiel zeigt eine LCM-Konfiguration, die beiden partielle Konfigurationen erwartet – eine, die das Betriebssystem bereitgestellt wird und das Bereitstellen und Konfigurieren von SharePoint.

```powershell
[DSCLocalConfigurationManager()]
configuration PartialConfigDemo
{
    Node localhost
    {

           PartialConfiguration OSInstall
        {
            Description = 'Configuration for the Base OS'
            RefreshMode = 'Push'
        }
           PartialConfiguration SharePointConfig
        {
            Description = 'Configuration for the SharePoint server'
            RefreshMode = 'Push'
        }
    }
}
PartialConfigDemo 
```

Die **RefreshMode** für jede partielle Konfiguration auf "Push" festgelegt ist. Die Namen der **PartialConfiguration** Blöcke (in diesem Fall "OSInstall" und "SharePointConfig") müssen genau die Namen der Konfigurationen, die an den Zielknoten verschoben werden übereinstimmen.

###Veröffentlichen und teilweise Konfigurationen Push-Modus starten

![PartialConfig-Ordnerstruktur](./images/PartialConfig1.jpg)

Rufen Sie dann **Veröffentlichen DSCConfiguration** für jede Konfiguration, übergeben Sie die Ordner, die Konfigurationsdokumente als Path-Parameter enthalten. Nach der Veröffentlichung beide Konfigurationen, rufen Sie `Start DSCConfiguration – UseExisting` auf dem Zielknoten.

##Teilweise Konfigurationen im Pull-Modus

Teilweise Konfigurationen aus einem oder mehreren Pull-Servern abgerufen werden können (Weitere Informationen zu Pull-Servern finden Sie unter [Windows PowerShell gewünschten Status Pull Konfigurationsserver](pullServer.md). Zu diesem Zweck müssen Sie den LCM auf dem Zielknoten, ziehen Sie die teilweisen Konfigurationen, und benennen, und suchen Konfigurationsdokumente ordnungsgemäß auf die Pull-Server konfigurieren.

###Konfigurieren den LCM Pull-Knoten-Konfigurationen

Zum Konfigurieren des LCM teilweise Konfigurationen von einem Pull-Server abrufen definieren Sie den Pull-Server entweder ein **ConfigurationRepositoryWeb** (für eine HTTP-Pull-Server) oder **ConfigurationRepositoryShare** (für eine SMB-Pull-Server) blockieren. Sie erstellen **PartialConfiguration** Blöcke, die an den Pull-Server finden Sie unter Verwendung der **ConfigurationSource** Eigenschaft. Sie müssen auch zum Erstellen eines Blocks Einstellungen, um anzugeben, dass der LCM Pull-Modus verwendet, und die ConfigurationID angeben, die die Pull-Server und dem Zielknoten verwenden, um die Konfigurationen zu identifizieren. Die folgende Meta-Konfiguration definiert einen HTTP-Pull-Server mit dem Namen CONTOSO-PullSrv, und ziehen Sie zwei partielle Konfigurationen, die diesen Server.

```powershell
[DSCLocalConfigurationManager()]
configuration PartialConfigDemo
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

           PartialConfiguration OSInstall
        {
            Description = 'Configuration for the Base OS'
            ConfigurationSource = '[ConfigurationRepositoryWeb]CONTOSO-PullSrv'
            RefreshMode = 'Pull'
        }
           PartialConfiguration SharePointConfig
        {
            Description = 'Configuration for the Sharepoint Server'
            ConfigurationSource = '[ConfigurationRepositoryWeb]CONTOSO-PullSrv'
            DependsOn = [PartialConfiguration]OSInstall
            RefreshMode = 'Pull'
        }
    }
}
PartialConfigDemo 
```

Sie können partielle Konfigurationen von mehr als einem Pull-Server abrufen – Sie müssten einfach jede Pull-Server definieren und verweisen Sie dann auf den entsprechenden Pull-Server in jedem Block PartialConfiguration.

Nach dem Erstellen der Meta-Konfigurations, müssen Sie ausführen, um ein Dokument (eine MOF-Datei) erstellen, und rufen Sie dann [Set-DscLocalConfigurationManager] (https://technet.microsoft.com/en-us/library/dn521621 (v=wps.630).aspx) den LCM konfigurieren.

###Benennung und die Konfigurationsdokumente auf dem Server Pull platzieren

Teilweise Konfigurationsdokumente müssen in den Ordner, der als eingefügt werden die **ConfigurationPath** in den `web.config` Datei für den Pull-Server (in der Regel `c:\Programme\Microsoft Files\WindowsPowerShell\DscService\Configuration`). Die Konfigurationsdokumente müssen wie folgt benannt werden: _ConfigurationName_. _ConfigurationID_`MOF`, wobei _ConfigurationName_ ist der Name der partiellen Konfiguration und _ConfigurationID_ wird die Konfigurations-ID, die in den LCM auf dem Zielknoten definiert. In unserem Beispiel sollte die Konfigurationsdokumente Namen wie folgt aussehen.
![PartialConfig Namen auf Pull-server](images/PartialConfigPullServer.jpg)

###Teilweise Konfigurationen Ausführen von einem Pull-server

Nachdem auf dem Zielknoten LCM konfiguriert wurde und die Konfigurationsdokumente erstellt und auf dem Pull-Server ordnungsgemäß benannt wurden, der Zielknoten wird ziehen Sie die partiellen Konfigurationen kombinieren und wenden Sie die resultierende Konfiguration in regelmäßigen Abständen gemäß der **RefreshFrequencyMins** -Eigenschaft des LCM. Wenn Sie eine Aktualisierung erzwingen möchten, können Sie das Update-DscConfiguration-Cmdlet, um die Konfigurationen zu ziehen aufrufen und dann `Start DSCConfiguration – UseExisting` angewendet.

##Teilweise Konfigurationen im gemischten Modus für Push- und Pullabonnements

Sie können auch mischen Push und pull-Modi für partielle Konfigurationen. Das heißt, könnten Sie eine partielle Konfiguration verwenden, die von einem Pull-Server abgerufen werden, während eine andere partielle Konfiguration verschoben werden. Behandeln Sie jede partielle Konfiguration wie, je nach den Aktualisierungsmodus wie in den vorherigen Abschnitten beschrieben. Die folgende Meta-Konfiguration beschreibt z. B. das gleiche Beispiel, mit der Konfiguration des Betriebssystems teilweise im Pull-Modus und die partielle SharePoint-Konfiguration im Push-Modus.

```powershell
[DSCLocalConfigurationManager()]
configuration PartialConfigDemo
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

           PartialConfiguration OSInstall
        {
            Description = 'Configuration for the Base OS'
            ConfigurationSource = '[ConfigurationRepositoryWeb]CONTOSO-PullSrv'
            RefreshMode = 'Pull'
        }
           PartialConfiguration SharePointConfig
        {
            Description = 'Configuration for the Sharepoint Server'
            DependsOn = [PartialConfiguration]OSInstall
            RefreshMode = 'Push'
        }
    }
}
PartialConfigDemo 
```

Beachten Sie, dass die **RefreshMode** im Block Einstellungen angegeben ist "Pull", aber die **RefreshMode** für die OSInstall teilweise Konfigurationen ist "Push".

Sie benennen und suchen die Konfigurationsdokumente für ihre jeweiligen aktualisierungsmodi wie oben beschrieben. Rufen Sie **Veröffentlichen DSCConfiguration** der SharePointInstall veröffentlichen teilweise Konfigurationen und entweder warten, die OSInstall Konfiguration aus dem Pull-Server abgerufen werden, oder erzwingen eine Aktualisierung durch Aufrufen von [Update-DscConfiguration] (https://technet.microsoft.com/en-us/library/mt143541 (v=wps.630).aspx).

##Weitere Informationen

**Konzepte**
[WindowsPowerShell gewünschten Status Pull-Konfigurationsserver](pullServer.md) 
[Konfigurieren den lokalen Konfigurations-Manager Windows](https://technet.microsoft.com/en-us/library/mt421188.aspx)




