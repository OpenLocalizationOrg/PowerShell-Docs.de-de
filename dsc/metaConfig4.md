#WindowsPowerShell 4.0 gewünscht State Configuration lokale Configuration Manager (LCM)

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Lokale Configuration Manager ist das Modul für Windows PowerShell gewünscht State Configuration (DSC). Es alle Zielknoten ausgeführt wird, und ist verantwortlich für das Aufrufen der Configuration-Ressourcen, die in ein Konfigurationsskript DSC enthalten sind. In diesem Thema werden die Eigenschaften von lokalen Configuration Manager aufgelistet und beschreibt, wie Sie die lokale Configuration Manager-Einstellungen auf einem Zielknoten ändern können.

##Lokale Configuration Manager-Eigenschaften

Im folgenden werden die lokale Configuration Manager-Eigenschaften, die festgelegt oder abgerufen werden kann.

* **AllowModuleOverwrite**: Steuerelemente, ob neue Konfigurationen aus dem Konfigurationsserver heruntergeladen sind, überschreiben die alten auf dem Zielknoten zulässig. Mögliche Werte sind True und False.
* **CertificateID**: GUID ein Zertifikat verwendet, um die Anmeldeinformationen für den Zugriff auf die Konfiguration zu sichern. Weitere Informationen finden Sie unter [Anmeldeinformationen in Windows PowerShell gewünschten Konfiguration sichern möchten?](http://blogs.msdn.com/b/powershell/archive/2014/01/31/want-to-secure-credentials-in-windows-powershell-desired-state-configuration.aspx).
* **ConfigurationID**: Gibt eine GUID, die zum Abrufen einer bestimmten Konfigurationsdatei von einem Server, der als "Pull"-Server einrichten. Die GUID wird sichergestellt, dass die richtige Konfigurationsdatei zugegriffen wird.
* **ConfigurationMode**: Gibt an, wie lokale Configuration Manager tatsächlich die Konfiguration der Zielknoten gilt. Sie können die folgenden Werte annehmen:
   - **ApplyOnly**: mit dieser Option DSC wendet die Konfiguration und keine weiteren Daten enthält, es sei denn, eine neue Konfiguration, entweder erkannt wird indem Sie eine neue Konfiguration direkt an das Ziel senden Knoten ("Push"), oder wenn Sie einen Server "pull" und die DSC konfiguriert haben ermittelt eine neue Konfiguration mit dem "Pull"-Server prüft. Wenn die Konfiguration des Zielknotens wandert ab, wird keine Aktion ausgeführt.
   - **ApplyAndMonitor**: Diese Option (die Standardeinstellung), DSC gilt jede neuen Konfigurationen, ob Sie direkt an den Zielknoten gesendet oder auf einem "Pull"-Server ermittelt. Wenn die Konfiguration des Zielknotens aus der Konfigurationsdatei wandert ab, meldet DSC danach die Abweichung in Protokollen. Weitere Informationen zur Protokollierung von DSC finden Sie unter [mithilfe von Ereignisprotokollen zum Diagnostizieren von Fehlern in der gewünschten Konfiguration](http://blogs.msdn.com/b/powershell/archive/2014/01/03/using-event-logs-to-diagnose-errors-in-desired-state-configuration.aspx).
   - **ApplyAndAutoCorrect**: mit dieser Option gilt DSC neuen Konfigurationen, ob Sie direkt an den Zielknoten gesendet oder auf einem "Pull"-Server ermittelt. Danach, wenn die Konfiguration des Zielknotens aus der Konfigurationsdatei wandert ab, DSC Berichte die Abweichung in Protokollen und versucht dann, passen Sie die Knoten Zielkonfiguration, in Übereinstimmung mit der Konfigurationsdatei anzuzeigen.
* **ConfigurationModeFrequencyMins**: Gibt die Frequenz (in Minuten), an dem die hintergrundanwendung im DSC versucht, die aktuelle Konfiguration auf dem Zielknoten zu implementieren. Der Standardwert ist 15. Dieser Wert kann in Verbindung mit RefreshMode festgelegt werden. Wenn RefreshMode PULL festgelegt ist, der Zielknoten aus den Konfigurationsserver in einem Intervall festlegen, indem Sie RefreshFrequencyMins und downloads für die aktuelle Konfiguration. Unabhängig vom Wert RefreshMode wendet das Modul Konsistenz in dem Intervall von ConfigurationModeFrequencyMins, die aktuelle Konfiguration, die auf dem Zielknoten heruntergeladen wurde. RefreshFrequencyMins sollte festgelegt werden, um eine ganze Zahl Vielfaches von ConfigurationModeFrequencyMins.
* **Anmeldeinformationen**: (mit Get-Credential) steht für Anmeldeinformationen erforderlich sind, remote Zugriff auf Ressourcen, wie z. B. den Konfigurationsserver zu kontaktieren.
* **DownloadManagerCustomData**: Stellt ein Array mit benutzerdefinierten Daten für den Download-Manager.
* **DownloadManagerName**: Gibt den Namen der Konfiguration und Modul Download-Manager.
* **RebootNodeIfNeeded**: bestimmte Änderungen auf dem Zielknoten erforderlich sein, neu gestartet werden, damit die Änderungen übernommen werden. Mit dem Wert **True**, diese Eigenschaft wird den Knoten neu gestartet, sobald die Konfiguration wurde vollständig ohne Warnung gilt. Wenn **False** (Standardwert), wird die Konfiguration abgeschlossen, aber der Knoten muss manuell neu gestartet werden damit die Änderungen wirksam werden.
* **RefreshFrequencyMins**: verwendet, wenn Sie eine "Pull"-Server eingerichtet haben. Gibt die Frequenz (in Minuten) an der lokalen Konfigurations-Manager einen "Pull"-Server zum Herunterladen der aktuellen Konfigurations beauftragt. Dieser Wert kann in Verbindung mit ConfigurationModeFrequencyMins festgelegt werden. Wenn RefreshMode PULL festgelegt ist, wird der Zielknoten kontaktiert den "Pull"-Server in einem Intervall festlegen, indem Sie RefreshFrequencyMins und downloads für die aktuelle Konfiguration. In dem Intervall ConfigurationModeFrequencyMins gilt das Modul Konsistenz die aktuelle Konfiguration, die auf dem Zielknoten heruntergeladen wurde. Wenn RefreshFrequencyMins nicht, in eine ganze Zahl festgelegt ist Vielfaches von ConfigurationModeFrequencyMins, das System wird aufgerundet werden kann. Der Standardwert ist 30.
* **RefreshMode**: Mögliche Werte sind **Push** (Standard) und **abrufen**. In der Konfiguration "Push" müssen Sie eine Konfigurationsdatei auf jedem Zielknoten Platzieren von jedem Clientcomputer. Im Modus "Pull" müssen Sie festlegen, einen "Pull"-Server für lokale Configuration Manager für die Kontaktaufnahme und die Konfigurationsdateien zugreifen.

###Beispiel für lokale Configuration Manager-Einstellungen aktualisieren

Können Sie die lokale Configuration Manager-Einstellungen der Zielknoten aktualisieren, indem Sie z. B. eine **LocalConfigurationManager** innerhalb des Blocks auf Knoten in einem Konfigurationsskript zu blockieren, wie im folgenden Beispiel gezeigt.

```powershell
Configuration ExampleConfig
{
    Node “Server001”
    {
        LocalConfigurationManager
        {
            ConfigurationID = "646e48cb-3082-4a12-9fd9-f71b9a562d4e"
            ConfigurationModeFrequencyMins = 45
            ConfigurationMode = "ApplyAndAutocorrect"
            RefreshMode = "Pull"
            RefreshFrequencyMins = 90
            DownloadManagerName = "WebDownloadManager"
            DownloadManagerCustomData = (@{ServerUrl="https://$PullServer/psdscpullserver.svc"})
            CertificateID = "71AA68562316FE3F73536F1096B85D66289ED60E"
            Credential = $cred
            RebootNodeIfNeeded = $true
            AllowModuleOverwrite = $false
        }
# One or more resource blocks can be added here
    }
}

# The following line invokes the configuration and creates a file called Server001.meta.mof at the specified path
ExampleConfig -OutputPath "c:\users\public\dsc"  
```

Ausführen des Skripts im vorherigen Beispiel generiert eine MOF-Datei, die angibt, und speichert die gewünschten Einstellungen. Um die Einstellungen zu übernehmen, können Sie die **Set DscLocalConfigurationManager** Cmdlets, wie im folgenden Beispiel gezeigt.

```powershell
Set-DscLocalConfigurationManager -Path "c:\users\public\dsc"
```

> **Hinweis**: für die **Pfad** Parameter, geben Sie denselben Pfad, die Sie für die **OutputPath** Parameter, wenn Sie die Konfiguration im vorherigen Beispiel aufgerufen.

Um die aktuelle lokale Configuration Manager-Einstellungen anzuzeigen, können Sie die **Get-DscLocalConfigurationManager** Cmdlet. Wenn Sie dieses Cmdlet ohne Parameter aufrufen, wird standardmäßig es die lokale Configuration Manager-Einstellungen für den Knoten erhalten auf dem er ausgeführt wird. Um einen anderen Knoten anzugeben, verwenden Sie die **CimSession** Parameter mit diesem Cmdlet.



