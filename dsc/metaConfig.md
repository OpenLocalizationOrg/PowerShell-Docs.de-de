#Konfigurieren des lokalen Konfigurations-Managers

> Gilt für: WindowsPowerShell 5.0

Die lokale Configuration Manager (LCM) ist das Modul von Windows PowerShell gewünscht State Configuration (DSC). LCM auf jedem Zielknoten ausgeführt wird, und ist verantwortlich für die Analyse und Umsetzung von Konfigurationen, die auf den Knoten gesendet werden. Es ist auch für eine Reihe von anderen Aspekten der DSC, einschließlich der folgenden verantwortlich.

* Bestimmen Aktualisierungsmodus (Push oder Pull).
* Angabe, wie oft ein Knoten abruft und Konfigurationen werden.
* Verknüpfen den Knoten mit Pull-Servern.
* Angeben von partiellen Konfigurationen.

Sie verwenden eine spezielle Konfiguration so konfigurieren Sie den LCM um jedes dieser Verhalten anzugeben. In den folgenden Abschnitten wird beschrieben, wie den LCM konfigurieren.

> **Hinweis**: Dieses Thema gilt für den LCM in Windows PowerShell 5.0 eingeführt. Informationen zum Konfigurieren der LCM in Windows PowerShell 4.0 finden Sie in der Windows PowerShell 4.0 Desired State Configuration lokale Configuration Manager.

##Das Schreiben und Umsetzung einer LCM-Konfigurations

Zum Konfigurieren des LCM erstellen und führen Sie eine spezielle Konfiguration. Um eine LCM Konfiguration anzugeben, verwenden Sie das DscLocalConfigurationManager-Attribut. Das folgende Beispiel zeigt eine einfache Konfiguration, die den LCM Push-Modus festgelegt.

```powershell
[DSCLocalConfigurationManager()]
configuration LCMConfig
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Push'
        }
    }
} 
```

Rufen Sie und führen Sie die Konfiguration zum Erstellen der Konfiguration MOF, genauso wie eine normale Konfiguration (Informationen zum Erstellen der MOF-Konfigurations finden Sie unter Erste Schritte mit Windows PowerShell gewünschten Konfiguration). Im Gegensatz zu normalen Konfigurationen, Sie sind nicht in Gang zu setzen eine LCM Konfiguration durch Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) Cmdlet. Rufen Sie stattdessen das Cmdlet Set-DscLocalConfigurationManager Angabe des Pfads zur Konfiguration MOF als Parameter. Nachdem Sie die Konfiguration umzusetzen, Sie sehen die Eigenschaften des LCM durch Aufrufen der [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx) Cmdlet.

Eine Konfiguration LCM kann Blöcke nur für eine begrenzte Anzahl von Ressourcen enthalten. Im vorherigen Beispiel ist die einzige Ressource namens **Settings**. Andere Ressourcen sind verfügbar:

* **ConfigurationRepositoryWeb**: Gibt einen HTTP-Pull-Server-Konfigurationen.
* **ConfigurationRepositoryShare**: Gibt einen SMB-Pull-Server-Konfigurationen.
* **ResourceRepositoryWeb**: Gibt einen HTTP-Pull-Server für Module.
* **ResourceRepositoryShare**: Gibt einen SMB-Pull-Server für Module.
* **ReportServerWeb**: Gibt einen HTTP-Pull-Server, die an die Berichte gesendet werden.
* **PartialConfiguration**: teilweise Konfigurationen angibt.

##Grundlegende Einstellungen

Als Pull-Servern und teilweise Konfigurationen angeben, werden alle Eigenschaften des LCM konfiguriert einem **Settings** Block. Die folgenden Eigenschaften stehen in einem **Settings** Block.

| Eigenschaft| Typ| Beschreibung|
|--- |--- |---|
| ConfigurationModeFrequencyMins| UInt32| Häufigkeit in Minuten an, die aktuelle Konfiguration wird überprüft und angewendet.Diese Eigenschaft wird ignoriert, wenn die Eigenschaft ConfigurationMode auf ApplyOnly festgelegt ist.Der Standardwert ist 15.__Hinweis__: entweder der Wert dieser Eigenschaft muss ein Vielfaches des Werts der __RefreshFrequencyMins__ Eigenschaft oder den Wert des der __RefreshFrequencyMins__ -Eigenschaft muss ein Vielfaches des Werts dieser Eigenschaft sein.|
| RebootNodeIfNeeded| bool| Legen Sie den __$true__ automatisch den Knoten nach einer Konfiguration neu gestartet, die erforderlich sind, Neustart angewendet wird.Andernfalls müssen Sie den Knoten für jede Konfiguration manuell neu starten, die erforderlich sind.Der Standardwert ist __$false__.|
| ConfigurationMode| string| Gibt an, wie der LCM tatsächlich die Konfiguration der Zielknoten gilt.Dauert die folgenden Werte: __"ApplyOnly"__: DSC wendet die Konfiguration und keine weiteren Daten enthält, es sei denn, eine neue Konfiguration verschoben wird, auf dem Zielknoten, oder wenn eine neue Konfiguration von einem Server abgerufen werden.Nach der ersten Anwendung eine neue Konfiguration überprüft DSC Abweichung von einer zuvor konfigurierten Zustand nicht.__"ApplyAndMonitor"__: Dies ist der Standardwert.Der LCM gilt keine neuen Konfigurationen.Nach der ersten Anwendung einer neuen Konfiguration der Zielknoten aus den gewünschten Status, wandert DSC meldet die Abweichung in Protokollen __"ApplyAndAutoCorrect"__: DSC gilt keine neuen Konfigurationen.Nach der ersten Anwendung eine neue Konfiguration, wenn der Zielknoten aus den gewünschten Status, wandert DSC Berichte die Abweichung in Protokollen und wendet die aktuelle Konfiguration erneut.|
| ActionAfterReboot| string| Gibt an, was nach einem Neustart während der Anwendung von einer Konfiguration ausgeführt werden.Die möglichen Werte sind wie folgt: __"ContuinueConfiguration"__: Anwenden der aktuellen Konfiguration.__" StopConfiguraiton"__: Beenden Sie die aktuelle Konfiguration.|
| RefreshMode| string| Gibt an, wie der LCM Konfigurationen abruft.Die möglichen Werte sind wie folgt: __"Deaktiviert"__: DSC-Konfigurationen werden für diesen Knoten deaktiviert.__"Push"__: Konfigurationen werden durch Aufrufen des Start-DscConfiguration-Cmdlets initiiert.Die Konfiguration wird sofort auf den Knoten angewendet.Dies ist der Standardwert.__Pull:__ der Knoten ist so konfiguriert, dass Sie regelmäßig prüfen, ob Konfigurationen von einem Pull-Server.Wenn diese Eigenschaft auf Pull festgelegt ist, müssen Sie angeben, dass einen Pull-Server in einem __ConfigurationRepositoryWeb__ oder __ConfigurationRepositoryShare__ blockieren.Weitere Informationen zu Pull-Servern finden Sie unter [Einrichten eines DSC Pull-Servers](pullServer.md).|
| CertificateID| string| Eine GUID, die ein Zertifikat zum Sichern der Anmeldeinformationen für den Zugriff auf die Konfiguration angibt.Weitere Informationen finden Sie unter [Anmeldeinformationen in Windows PowerShell gewünschten Konfiguration sichern möchten](http://blogs.msdn.com/b/powershell/archive/2014/01/31/want-to-secure-credentials-in-windows-powershell-desired-state-configuration.aspx)?|
| ConfigurationID| string| Eine GUID, die Konfigurationsdatei beim Abrufen aus einem Pull-Server im Pull-Modus bezeichnet.Der Knoten mithilfe von pull-Konfigurationen auf die Pull-Server lautet der Name der MOF-Konfiguration ConfigurationID.mof ist.__Hinweis:__ wenn Sie diese Eigenschaft festlegen, können Sie den Knoten mit einem Pull-Server registrieren, mit __RegistryKeys__ und kann nicht verwendet werden.Weitere Informationen finden Sie unter [Einrichten eines Pull-Clients mit Konfigurationsnamen](pullClientConfigNames.md).|
| RefreshFrequencyMins| UInt32| Das Zeitintervall in Minuten an, an denen der LCM einen Pull-Server zum Abrufen der aktualisierter Konfigurationen überprüft.Dieser Wert wird ignoriert, wenn der LCM nicht im Pull-Modus konfiguriert ist.Der Standardwert ist 30.__Hinweis:__  entweder der Wert dieser Eigenschaft muss ein Vielfaches des Werts der __ConfigurationModeFrequencyMins__ Eigenschaft oder den Wert des der __ConfigurationModeFrequencyMins__ -Eigenschaft muss ein Vielfaches des Werts dieser Eigenschaft sein.|
| AllowModlueOverwrite| bool| __$TRUE__ Wenn neue Konfigurationen, die aus dem Konfigurationsserver heruntergeladene zulässig sind, überschreiben die alten auf dem Zielknoten.Andernfalls $FALSE.|
| Debug-Modus| bool| Wenn auf festgelegt __$TRUE__, dadurch LCM, alle Ressourcen DSC erneut zu laden, auch wenn diese bereits zwischengespeichert wurden.Auf $FALSE festgelegt, zwischengespeicherte Ressourcen verwenden.In der Regel würden Sie diese Eigenschaft festlegen, um __$TRUE__ während des Debuggens einer Ressource, und klicken Sie auf __$FALSE__ für die Produktion.Der Standardwert ist __$FALSE__.|
| ConfigurationDownloadManagers| CimInstance]| Veraltete.Verwendung __ConfigurationRepositoryWeb__ und __ConfigurationRepositoryShare__ Blöcke Pull Konfigurationsserver zu definieren.|
| ResourceModuleManagers| CimInstance]| Veraltete.Verwendung __ResourceRepositoryWeb__ und __ResourceRepositoryShare__ Server pull-Blöcke, um die Ressource zu definieren.|
| ReportManagers| CimInstance]| Veraltete.Verwendung __ReportServerWeb__ Server pull-Blöcke, um den Bericht zu definieren.|
| PartialConfigurations| CimInstance| Nicht implementiert.Verwenden Sie nicht.|
| StatusRetentionTimeInDays| UInt32| Die Anzahl der Tage, die der LCM den Status der aktuellen Konfiguration hält.|

##Pull-Servern

Ein Pull-Server ist ein OData-Webdienst oder eine SMB-Freigabe, die als zentraler Speicherort für DSC-Dateien verwendet wird. LCM Konfiguration unterstützt, definieren die folgenden Typen von Pull-Servern:

* **Konfigurationsserver**: ein Repository für DSC-Konfigurationen. Definieren Konfiguration trennt mit **ConfigurationRepositoryWeb** (für Web-basierte Server) und **ConfigurationRepositoryShare** (für SMB-basierten Server) blockiert.
* Ressourcenserver – ein Repository für DSC-Ressourcen, die als PowerShell-Module verpackt. Definieren Sie Ressource trennt, mit **ResourceRepositoryWeb** (für Web-basierte Server) und **ResourceRepositoryShare** (für SMB-basierten Server) blockiert.
* Berichtsserver – ein Dienst, der DSC Berichtsdaten sendet. Definieren von Berichtsservern mit **ReportServerWeb** blockiert. Ein Berichtsserver muss es sich um einen Webdienst sein.

Informationen zum Einrichten und Verwenden von Pull-Servern finden Sie unter [Einrichten eines DSC Pull-Servers](pullServer.md).

##Konfiguration Server Blöcke

So definieren Sie eine webbasierte Konfiguration sever, erstellen Sie eine **ConfigurationRepositoryWeb** Block. Ein **ConfigurationRepositoryWeb** definiert die folgenden Eigenschaften.

| Eigenschaft| Typ| Beschreibung|
|---|---|---|
| AllowUnsecureConnection| bool| Legen Sie auf **$TRUE** um Verbindungen über den Knoten mit dem Server ohne Authentifizierung zu ermöglichen.Legen Sie auf **$FALSE** Authentifizierung erforderlich ist.|
| CertificateID| string| Eine GUID, die das Zertifikat zur Authentifizierung beim Server darstellt.|
| ConfigurationNames| String[]| Ein Array der Namen von Konfigurationen, die von den Zielknoten abgerufen werden.Diese werden nur verwendet, wenn der Knoten mit dem Pull-Server registriert ist ein **RegistrationKey**.Weitere Informationen finden Sie unter [Einrichten eines Pull-Clients mit Konfigurationsnamen](pullClientConfigNames.md).|
| RegistrationKey| string| Eine GUID, die den Knoten mit dem Pull-Server registriert.Weitere Informationen finden Sie unter [Einrichten eines Pull-Clients mit Konfigurationsnamen](pullClientConfigNames.md).|
| Server-URL| string| Die URL des Servers Konfiguration.|

Um eine SMB-basierten Konfigurationsserver zu definieren, erstellen Sie eine **ConfigurationRepositoryShare** blockieren. Ein **ConfigurationRepositoryShare** definiert die folgenden Eigenschaften.

| Eigenschaft| Typ| Beschreibung|
|---|---|---|
| Credential| MSFT_Credential| Die Anmeldeinformationen zum Authentifizieren der SMB-Freigabe verwendet wird.|
| Quellpfad| string| Der Pfad der SMB-Freigabe.|

##Ressource Server Blöcke

Definieren Sie eine webbasierte Ressource sever, erstellen Sie eine **ResourceRepositoryWeb** blockieren. Ein **ResourceRepositoryWeb** definiert die folgenden Eigenschaften.

| Eigenschaft| Typ| Beschreibung|
|---|---|---|
| AllowUnsecureConnection| bool| Legen Sie auf **$TRUE** um Verbindungen über den Knoten mit dem Server ohne Authentifizierung zu ermöglichen.Legen Sie auf **$FALSE** Authentifizierung erforderlich ist.|
| CertificateID| string| Eine GUID, die das Zertifikat zur Authentifizierung beim Server darstellt.|
| RegistrationKey| string| Eine GUID, die den Knoten mit dem Pull-Server identifiziert.Weitere Informationen finden Sie unter einem Knoten mit einem DSC Pull-Server registrieren.|
| Server-URL| string| Die URL des Servers Konfiguration.|

Um eine SMB-basierten Server zu definieren, erstellen Sie eine **ResourceRepositoryShare** blockieren. **ResourceRepositoryShare** definiert die folgenden Eigenschaften.

| Eigenschaft| Typ| Beschreibung|
|---|---|---|
| Credential| MSFT_Credential| Die Anmeldeinformationen zum Authentifizieren der SMB-Freigabe verwendet wird.|
| Quellpfad| string| Der Pfad der SMB-Freigabe.|

##Report Server-Blöcke

Ein Berichtsserver muss es sich um einen OData-Webdienst werden. Um einen Berichtsserver zu definieren, erstellen Sie eine **ReportServerWeb** blockieren. **ReportServerWeb** definiert die folgenden Eigenschaften.

| Eigenschaft| Typ| Beschreibung|
|---|---|---|
| AllowUnsecureConnection| bool| Legen Sie auf **$TRUE** um Verbindungen über den Knoten mit dem Server ohne Authentifizierung zu ermöglichen.Legen Sie auf **$FALSE** Authentifizierung erforderlich ist.|
| CertificateID| string| Eine GUID, die das Zertifikat zur Authentifizierung beim Server darstellt.|
| RegistrationKey| string| Eine GUID, die den Knoten mit dem Pull-Server identifiziert.Weitere Informationen finden Sie unter einem Knoten mit einem DSC Pull-Server registrieren.|
| Server-URL| string| Die URL des Servers Konfiguration.|

##Teilweise Konfigurationen

Um eine teilweise Konfigurationen zu definieren, erstellen Sie eine **PartialConfiguration** blockieren. Weitere Informationen zu partiellen Konfigurationen finden Sie unter [DSC teilweise Konfigurationen](partialConfigs.md). **PartialConfiguration** definiert die folgenden Eigenschaften.

| Eigenschaft| Typ| Beschreibung|
|---|---|---|
| ConfigurationSource| string[]| Ein Array von Namen der Konfiguration, zuvor definierten **ConfiguratoinRepositoryWeb** und **ConfigurationRepositoryShare** -Blöcken, in denen die partielle Konfiguration abgerufen wird, aus.|
| DependsOn| Zeichenfolge {}| Eine Liste der Namen von anderen Konfigurationen, die abgeschlossen sein müssen, bevor diese partiellen Konfiguration angewendet wird.|
| Beschreibung| string| Text für die partielle Konfiguration beschrieben.|
| ExclusiveResources| string[]| Eine Reihe von Ressourcen, die ausschließlich für diese partiellen Konfiguration.|
| RefreshMode| string| Gibt an, wie die DCS diese partiellen Konfiguration abruft.Die möglichen Werte sind wie folgt: **deaktiviert**: diese partiellen Konfiguration deaktiviert ist.**Push**: teilweise Konfigurationen wird der Knoten verschoben, durch Aufrufen der [Veröffentlichen DscConfiguration](https://technet.microsoft.com/en-us/library/mt517875.aspx) Cmdlet.Nachdem alle partielle Konfigurationen für den Knoten entweder verschoben oder von einem Server angefordert, kann die Konfiguration durch den Aufruf gestartet werden `Start DscConfiguration – UseExisting`.Dies ist der Standardwert.**Pull**: der Knoten ist so konfiguriert, dass Sie regelmäßig prüfen, ob die partielle Konfiguration von einem Pull-Server.Wenn diese Eigenschaft auf "Pull" festgelegt ist, müssen Sie einen Pull-Server angeben, durch Festlegen der **ConfigurationSource** Eigenschaft.Weitere Informationen zu Pull-Servern finden Sie unter [Einrichten eines DSC Pull-Servers](pullServer.md).|
| ResourceModlueSource| string[]| Ein Array der Namen von Ressourcenservern, von dem die erforderliche Ressourcen für diese Konfiguration teilweise heruntergeladen.Diese Namen müssen finden Sie in der zuvor im definierten Ressourcenserver **ResourceRepositoryWeb** und **ResourceRepositoryShare** blockiert.|

##Weitere Informationen

###Konzepte

Erste Schritte mit Windows PowerShell gewünschten Status-Konfiguration 
[Einrichten eines DSC Pull-Servers](pullServer.md) 
[WindowsPowerShell 4.0 gewünscht State Configuration lokale Configuration Manager](metaConfig4.md)

###Weitere Ressourcen

[Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx) 
[Ein Pull-Client mit Konfigurationsnamen einrichten](pullClientConfigNames.md)




