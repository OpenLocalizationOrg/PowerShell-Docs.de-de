---
title: Konfigurieren des lokalen Konfigurations-Managers
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 5d37938869a71bea0d8a6349e680411b7d0200d9

---

# Konfigurieren des lokalen Konfigurations-Managers

> Gilt für: Windows PowerShell 5.0

Die lokale Konfigurations-Manager (Local Configuration Manager, LCM) ist das Modul von Windows PowerShell DSC (Desired State Configuration, Konfiguration des gewünschten Zustands). Der LCM wird auf allen Zielknoten ausgeführt und ist zuständig für das Analysieren und Anwenden von Konfigurationen, die zum Knoten gesendet werden. Dieses Modul ist auch für verschiedene andere DSC-Aspekte zuständig, wie z. B. die folgenden.

* Bestimmen des Aktualisierungsmodus (Push- oder Pullmodus).
* Angeben, wie oft ein Knoten Konfigurationen abruft und anwendet.
* Verknüpfen des Knotens mit Pullservern.
* Angeben von Teilkonfigurationen.

Sie verwenden eine spezielle Art von Konfiguration zum Konfigurieren des LCM, um jedes dieser Verhalten anzugeben. In den folgenden Abschnitten wird beschrieben, wie der LCM konfiguriert wird.

> **Hinweis**: Dieses Thema gilt für den in Windows PowerShell 5.0 eingeführten LCM. Informationen zum Konfigurieren des LCM in Windows PowerShell 4.0 finden Sie unter [Windows PowerShell 4.0 DSC – Lokaler Konfigurations-Manager (LCM)](metaconfig4.md).

## Schreiben und Anwenden einer LCM-Konfiguration

Zum Konfigurieren des LCM müssen Sie einen besonderen Typ von Konfiguration erstellen und ausführen. Zum Angeben der LCM-Konfiguration verwenden Sie das „DscLocalConfigurationManager“-Attribut. Das folgende Beispiel zeigt eine einfache Konfiguration, die den LCM auf den Pushmodus festgelegt.

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

Für die Erstellung der MOF-Konfigurationsdatei können Sie die Konfiguration wie eine normale Konfiguration aufrufen und ausführen. Weitere Informationen zum Erstellen der MOF-Konfigurationsdatei finden Sie unter [Kompilieren der Konfiguration](configurations.md#compiling-the-configuration). Im Gegensatz zu normalen Konfigurationen wird eine LCM-Konfiguration nicht durch Aufrufen des Cmdlets [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) angewendet. Stattdessen rufen Sie das Cmdlet [Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx) auf und geben dabei den Pfad zur MOF-Konfigurationsdatei als Parameter an. Nach dem Anwenden der Konfiguration können Sie die Eigenschaften des LCM durch Aufrufen des Cmdlets [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx) anzeigen.

Eine LCM-Konfiguration kann nur Blöcke für eine begrenzte Menge von Ressourcen enthalten. Im vorherigen Beispiel ist **Settings** die einzige aufgerufene Ressource. Es folgen die anderen verfügbaren Ressourcen:

* **ConfigurationRepositoryWeb**: Gibt einen HTTP-Pullserver für Konfigurationen an. 
* **ConfigurationRepositoryWeb**: Gibt einen SMB-Pullserver für Konfigurationen an.
* **ResourceRepositoryWeb**: Gibt einen HTTP-Pullserver für Module an.
* **ResourceRepositoryWeb**: Gibt einen SMB-Pullserver für Module an.
* **ReportServerWeb**: Gibt einen HTTP-Pullserver an, an den Berichte gesendet werden.
* **PartialConfiguration**: Gibt Teilkonfigurationen an.

## Grundlegende Einstellungen

Außer der Angabe von Pullservern und Teilkonfigurationen werden alle Eigenschaften des LCM in einem **Settings**-Block konfiguriert. Die folgenden Eigenschaften sind in einem **Settings**-Block verfügbar:

|  Eigenschaft  |  Typ  |  Beschreibung   | 
|----------- |------- |--------------- | 
| ConfigurationModeFrequencyMins| UInt32| Gibt (in Minuten) an, wie oft die aktuelle Konfiguration überprüft und angewendet wird. Diese Eigenschaft wird ignoriert, wenn die „ConfigurationMode“-Eigenschaft auf „ApplyOnly“ festgelegt ist. Der Standardwert ist 15. <br> __Hinweis__: Der Wert dieser Eigenschaft muss ein Vielfaches des Werts der __RefreshFrequencyMins__-Eigenschaft sein, oder der Wert der __RefreshFrequencyMins__-Eigenschaft muss ein Vielfaches des Werts dieser Eigenschaft sein.| 
| RebootNodeIfNeeded| bool| Legen Sie diese Einstellung auf __$true__ fest, um den Knoten automatisch neu zu starten, nachdem eine Konfiguration angewendet wurde, die einen Neustart erfordert. Andernfalls müssen Sie den Knoten für jede Konfiguration manuell neu starten, die dies erfordert. Der Standardwert ist __$false__.| 
| ConfigurationMode| string | Gibt an, wie der LCM die Konfiguration tatsächlich auf die Zielknoten anwendet. Mögliche Werte sind __ApplyOnly__, __ApplyandMonitor (Standard)__ und __ApplyandAutoCorrect__. <ul><li>__ApplyOnly__: DSC wendet die Konfiguration an und führt keine weiteren Schritte aus, es sei denn, eine neue Konfiguration wird per Push auf den Zielknoten übertragen oder per Pull von einem Server abgerufen. Nach der ersten Anwendung einer neuen Konfiguration überprüft DSC nicht auf Abweichungen von einem zuvor konfigurierten Status. Beachten Sie, dass DSC versucht, die Konfiguration anzuwenden, bis dies erfolgreich passiert ist, bevor __ApplyOnly__ wirksam wird. </li><li> __ApplyAndMonitor__: Dies ist der Standardwert. Der LCM wendet neue Konfigurationen an. Wenn der Zielknoten nach der ersten Anwendung einer neuen Konfiguration vom gewünschten Zustand abweicht, meldet DSC die Abweichung in Protokollen. Beachten Sie, dass DSC versucht, die Konfiguration anzuwenden, bis dies erfolgreich passiert ist, bevor __ApplyAndMonitor__ wirksam wird.</li><li>__ApplyAndAutoCorrect__: DSC wendet alle neuen Konfigurationen an. Wenn der Zielknoten nach der ersten Anwendung einer neuen Konfiguration vom gewünschten Zustand abweicht, meldet DSC die Abweichung in Protokollen und wendet dann die aktuelle Konfiguration an.</li></ul>| 
| ActionAfterReboot| string| Gibt an, was nach einem Neustart während der Anwendung einer Konfiguration passiert. Die möglichen Werte sind __ContinueConfiguration (Standard)__ und __StopConfiguration__. <ul><li> __ContinueConfiguration__: Nach dem Neustart des Computers wird das Anwenden der aktuellen Konfiguration fortgesetzt.</li><li>__StopConfiguration__: Nach dem Neustart des Computers wird die aktuelle Konfiguration beendet.</li></ul>| 
| RefreshMode| string| Gibt an, wie der LCM Konfigurationen abruft. Die möglichen Werte sind __Disabled__, __Push (Standard)__ und __Pull__. <ul><li>__Disabled__: DSC-Konfigurationen werden für diesen Knoten deaktiviert.</li><li> __Push__: Konfigurationen werden gestartet, indem das Cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) aufgerufen wird. Die Konfiguration wird sofort auf den Knoten angewendet. Dies ist der Standardwert.</li><li>__Pull:__ Der Knoten ist so konfiguriert, dass regelmäßig eine Überprüfung auf Konfigurationen von einem Pullserver erfolgt. Wenn diese Eigenschaft auf __Pull__ festgelegt ist, müssen Sie in einem __ConfigurationRepositoryWeb__- oder __ConfigurationRepositoryShare__-Block einen Pullserver angeben. Weitere Informationen zu Pullservern finden Sie unter [Einrichten eines DSC-Pullservers](pullServer.md).</li></ul>| 
| CertificateId| string| GUID, die ein Zertifikat zum Schützen der Anmeldeinformationen für den Zugriff auf die Konfiguration angibt. Weitere Informationen finden Sie unter [Möchten Sie Anmeldeinformationen in Windows PowerShell zum Konfigurieren des gewünschten Zustands schützen?](http://blogs.msdn.com/b/powershell/archive/2014/01/31/want-to-secure-credentials-in-windows-powershell-desired-state-configuration.aspx).| 
| ConfigurationID| string| GUID, die die Konfigurationsdatei bestimmt, die im Pullmodus von einem Pullserver abgerufen wird. Der Knoten ruft Konfigurationen vom Pullserver ab, wenn der Name der MOF-Konfigurationsdatei „ConfigurationID.mof“ ist.<br> __Hinweis:__ Wenn Sie diese Eigenschaft festlegen, funktioniert das Registrieren des Knotens mit einem Pullserver mithilfe von __RegistrationKey__ nicht. Weitere Informationen finden Sie unter [Einrichten eines Pullclients mit Konfigurationsnamen](pullClientConfigNames.md).| 
| RefreshFrequencyMins| UInt32| Zeitintervall (in Minuten), in dem der LCM einen Pullserver auf aktualisierte Konfigurationen abfragt. Dieser Wert wird ignoriert, wenn der LCM nicht im Pullmodus konfiguriert ist. Der Standardwert ist 30.<br> __Hinweis__: Der Wert dieser Eigenschaft muss ein Vielfaches des Werts der __ConfigurationModeFrequencyMins__-Eigenschaft sein, oder der Wert der __ConfigurationModeFrequencyMins__-Eigenschaft muss ein Vielfaches des Werts dieser Eigenschaft sein.| 
| AllowModuleOverwrite| bool| __$TRUE__, wenn neue vom Konfigurationsserver heruntergeladene Konfigurationen die alten Konfigurationen auf dem Zielknoten überschreiben dürfen. Andernfalls „$FALSE“.| 
| DebugMode| string| Mögliche Werte sind __None (Standard)__, __ForceModuleImport__ und __All__. <ul><li>Bei Festlegung auf __None__ werden zwischengespeicherte Ressourcen verwendet. Dies ist die Standardeinstellung, die in Produktionsszenarien verwendet werden sollte.</li><li>Das Festlegen auf __ForceModuleImport__ bewirkt, dass der LCM DSC-Ressourcenmodule erneut lädt, auch wenn sie zuvor bereits geladen und zwischengespeichert wurden. Dies beeinträchtigt die Leistung von DSC-Vorgängen, da jedes Modul bei Verwendung neu geladen wird. In der Regel wird dieser Wert beim Debuggen einer Ressource verwendet.</li><li>In dieser Version ist __All__ identisch mit __ForceModuleImport__.</li></ul> |
| ConfigurationDownloadManagers| CimInstance[]| Veraltet. Verwenden Sie die Blöcke __ConfigurationRepositoryWeb__ und __ConfigurationRepositoryShare__ zum Definieren von Pullservern für Konfigurationen.| 
| ResourceModuleManagers| CimInstance[]| Veraltet. Verwenden Sie die Blöcke __ResourceRepositoryWeb__ und __ResourceRepositoryShare__ zum Definieren von Pullservern für Ressourcen.| 
| ReportManagers| CimInstance[]| Veraltet. Verwenden Sie Blöcke des Typs __ReportServerWeb__ zum Definieren von Pullservern für Berichte.| 
| PartialConfigurations| CimInstance| Nicht implementiert. Nicht verwenden.| 
| StatusRetentionTimeInDays | UInt32| Anzahl der Tage, die der LCM den Status der aktuellen Konfiguration beibehält.| 

## Pullserver

Ein Pullserver ist entweder ein OData-Webdienst oder eine SMB-Freigabe, der/die als zentraler Speicherort für DSC-Dateien verwendet wird. Die LCM-Konfiguration unterstützt die folgenden Typen von Pullservern:

* **Konfigurationsserver**: Repository für DSC-Konfigurationen. Definieren Sie Konfigurationsserver mithilfe der Blöcke **ConfigurationRepositoryWeb** (für webbasierte Server) und **ConfigurationRepositoryShare** (für SMB-basierte Server).
* Ressourcenserver: Repository für DSC-Ressourcen, die als PowerShell-Module gepackt sind. Definieren Sie Ressourcenserver mithilfe der Blöcke **ResourceRepositoryWeb** (für webbasierte Server) und **ResourceRepositoryShare** (für SMB-basierte Server).
* Berichtsserver: Dienst, an den DSC Berichtsdaten sendet. Definieren Sie Berichtsserver mithilfe von **ReportServerWeb**-Blöcken. Ein Berichtsserver muss ein Webdienst sein.

Weitere Informationen zum Einrichten und Verwenden von Pullservern finden Sie unter [Einrichten eines DSC-Pullservers](pullServer.md).

## Konfigurationsserverblöcke

Zum Definieren eines webbasierten Konfigurationsservers erstellen Sie einen **ConfigurationRepositoryWeb**-Block. Ein **ConfigurationRepositoryWeb**-Block definiert die folgenden Eigenschaften.

|Eigenschaft|Typ|Beschreibung|
|---|---|---| 
|AllowUnsecureConnection|bool|Legen Sie diese Einstellung auf **$TRUE** fest, um Verbindungen zwischen Knoten und Server ohne Authentifizierung zu erlauben. Bei Festlegung auf **$FALSE** ist eine Authentifizierung erforderlich.|
|CertificateId|string|GUID, die das Zertifikat zur Authentifizierung beim Server darstellt.|
|ConfigurationNames|String[]|Array der Namen von Konfigurationen, die per Pull vom Zielknoten abgerufen werden. Diese werden nur verwendet, wenn der Knoten mit einem **RegistrationKey** beim Pullserver registriert ist. Weitere Informationen finden Sie unter [Einrichten eines Pullclients mit Konfigurationsnamen](pullClientConfigNames.md).|
|RegistrationKey|string|GUID, die den Knoten beim Pullserver registriert. Weitere Informationen finden Sie unter [Einrichten eines Pullclients mit Konfigurationsnamen](pullClientConfigNames.md).|
|ServerURL|string|URL des Konfigurationsservers.|

Zum Definieren eines SMB-basierten Konfigurationsservers erstellen Sie einen **ConfigurationRepositoryShare**-Block. Ein **ConfigurationRepositoryShare**-Block definiert die folgenden Eigenschaften.

|Eigenschaft|Typ|Beschreibung|
|---|---|---|
|Credential|MSFT_Credential|Anmeldeinformationen zum Authentifizieren bei der SMB-Freigabe.|
|SourcePath|string|Pfad der SMB-Freigabe.|

## Ressourcenserverblöcke

Zum Definieren eines webbasierten Ressourcenservers erstellen Sie einen **ResourceRepositoryWeb**-Block. Ein **ResourceRepositoryWeb**-Block definiert die folgenden Eigenschaften.

|Eigenschaft|Typ|Beschreibung|
|---|---|---|
|AllowUnsecureConnection|bool|Legen Sie diese Einstellung auf **$TRUE** fest, um Verbindungen zwischen Knoten und Server ohne Authentifizierung zu erlauben. Bei Festlegung auf **$FALSE** ist eine Authentifizierung erforderlich.|
|CertificateId|string|GUID, die das Zertifikat zur Authentifizierung beim Server darstellt.|
|RegistrationKey|string|GUID, die den Knoten beim Pullserver identifiziert. Weitere Informationen finden Sie unter „Registrieren eines Knotens bei einem DSC-Pullserver“.|
|ServerURL|string|URL des Konfigurationsservers.|
 
Zum Definieren eines SMB-basierten Ressourcenservers erstellen Sie einen **ResourceRepositoryShare**-Block. Ein **ResourceRepositoryShare**-Block definiert die folgenden Eigenschaften.

|Eigenschaft|Typ|Beschreibung|
|---|---|---|
|Credential|MSFT_Credential|Anmeldeinformationen zum Authentifizieren bei der SMB-Freigabe.|
|SourcePath|string|Pfad der SMB-Freigabe.|

## Berichtsserverblöcke

Ein Berichtsserver muss ein OData-Webdienst sein. Zum Definieren eines Berichtsservers erstellen Sie einen **ReportServerWeb**-Block. Ein **ReportServerWeb**-Block definiert die folgenden Eigenschaften.

|Eigenschaft|Typ|Beschreibung|
|---|---|---| 
|AllowUnsecureConnection|bool|Legen Sie diese Einstellung auf **$TRUE** fest, um Verbindungen zwischen Knoten und Server ohne Authentifizierung zu erlauben. Bei Festlegung auf **$FALSE** ist eine Authentifizierung erforderlich.|
|CertificateId|string|GUID, die das Zertifikat zur Authentifizierung beim Server darstellt.|
|RegistrationKey|string|GUID, die den Knoten beim Pullserver identifiziert. Weitere Informationen finden Sie unter „Registrieren eines Knotens bei einem DSC-Pullserver“.|
|ServerURL|string|URL des Konfigurationsservers.|

## Teilkonfigurationen

Zum Definieren von Teilkonfigurationen erstellen Sie einen **PartialConfiguration**-Block. Weitere Informationen zu Teilkonfigurationen finden Sie unter [DSC-Teilkonfigurationen](partialConfigs.md). Ein **PartialConfiguration**-Block definiert die folgenden Eigenschaften.

|Eigenschaft|Typ|Beschreibung|
|---|---|---| 
|ConfigurationSource|string[]|Array mit Namen von Konfigurationsservern, die zuvor in den Blöcken **ConfigurationRepositoryWeb** und **ConfigurationRepositoryShare** definiert wurden, aus denen die Teilkonfiguration per Pull abgerufen wird.|
|DependsOn|string{}|Eine Liste der Namen anderer Konfigurationen, die abgeschlossen sein müssen, bevor diese Teilkonfiguration angewendet wird.|
|Beschreibung|string|Text zum Beschreiben der Teilkonfiguration.|
|ExclusiveResources|string[]|Array von Ressourcen, die ausschließlich für diese Teilkonfiguration gelten.|
|RefreshMode|string|Gibt an, wie der LCM diese Teilkonfiguration abruft. Die möglichen Werte sind __Disabled__, __Push (Standard)__ und __Pull__. <ul><li>__Deaktiviert__: Diese Teilkonfiguration ist deaktiviert.</li><li> __Push__: Die Teilkonfiguration wird per Push auf den Knoten übertragen, indem das Cmdlet [Publish-DscConfiguration](https://technet.microsoft.com/en-us/library/mt517875.aspx) aufgerufen wird. Nachdem alle Teilkonfigurationen für den Knoten von einem Server per Push oder Pull abgerufen wurden, kann die Konfiguration durch Aufrufen von `Start-DscConfiguration –UseExisting` gestartet werden. Dies ist der Standardwert.</li><li>__Pull:__ Der Knoten ist so konfiguriert, dass regelmäßig eine Überprüfung auf Teilkonfigurationen auf einem Pullserver erfolgt. Wenn diese Eigenschaft auf __Pull__ festgelegt ist, müssen Sie einen Pullserver angeben, indem Sie die __ConfigurationSource__-Eigenschaft festlegen. Weitere Informationen zu Pullservern finden Sie unter [Einrichten eines DSC-Pullservers](pullServer.md).</li></ul>|
|ResourceModuleSource|string[]|Array der Namen von Ressourcenservern, von denen erforderliche Ressourcen für diese Teilkonfiguration heruntergeladen werden. Diese Namen müssen auf Ressourcenserver verweisen, die zuvor in den Blöcken **ResourceRepositoryWeb** und **ResourceRepositoryShare** definiert wurden.|

## Weitere Informationen 

### Konzepte
[Windows PowerShell DSC – Übersicht](overview.md)
 
[Einrichten eines DSC-Pullservers](pullServer.md) 

[WindowsPowerShell 4.0 DSC – Lokaler Konfigurations-Manager](metaConfig4.md) 

### Weitere Ressourcen
[Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx) 

[Einrichten eines Pullclients mit Konfigurationsnamen](pullClientConfigNames.md) 




<!--HONumber=Oct16_HO1-->


