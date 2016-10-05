---
title: Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID in PowerShell 4.0
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 730f2f26e2811996e79cf0073a4ef65cad390687

---

# Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID in PowerShell 4.0

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Jeder Zielknoten muss angewiesen werden, den Pullmodus zu verwenden. Außerdem muss die URL angegeben werden, unter der der Pullserver zum Abrufen von Konfigurationen kontaktiert werden kann. Hierzu müssen Sie den lokalen Konfigurations-Manager (LCM) mit den benötigten Informationen konfigurieren. Zum Konfigurieren des LCM müssen Sie einen besonderen Typ von Konfiguration erstellen, die als „Metakonfiguration“ bezeichnet wird. Weitere Informationen zum Konfigurieren des LCM finden Sie unter [Windows PowerShell 4.0 DSC – Lokaler Konfigurations-Manager](metaConfig4.md).

Das folgende Skript konfiguriert den LCM zum Abrufen von Konfigurationen von einem Pullserver namens „PullServer“:

```powershell
Configuration SimpleMetaConfigurationForPull 
{ 
    LocalConfigurationManager 
    { 
        ConfigurationID = "1C707B86-EF8E-4C29-B7C1-34DA2190AE24";
        RefreshMode = "PULL";
        DownloadManagerName = "WebDownloadManager";
        RebootNodeIfNeeded = $true;
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 30; 
        ConfigurationMode = "ApplyAndAutoCorrect";
        DownloadManagerCustomData = @{ServerUrl = "http://PullServer:8080/PSDSCPullServer/PSDSCPullServer.svc"; AllowUnsecureConnection = “TRUE”}
    } 
} 
SimpleMetaConfigurationForPull -Output "."
```

Im Skript übergibt **DownloadManagerCustomData** die URL des Pullservers und lässt (bei diesem Beispiel) eine unsichere Verbindung zu. 

Nachdem das Skript ausgeführt wurde, erstellt es einen neuen Ausgabeordner mit dem Namen **SimpleMetaConfigurationForPull**, der eine MOF-Datei mit der Metakonfiguration enthält.

Um die Konfiguration anzuwenden, verwenden Sie das Cmdlet **Set DscLocalConfigurationManager** mit Parametern für **ComputerName** (verwenden Sie „localhost“) und **Path** (der Pfad zum Speicherort der Datei „localhost.meta.mof“ für den Zielknoten). Beispiel: 
```powershell
Set-DSCLocalConfigurationManager –ComputerName localhost –Path . –Verbose.
```

## Konfigurations-ID
Das Skript legt die **ConfigurationID**-Eigenschaft des LCM auf eine GUID fest, die zuvor für diesen Zweck erstellt wurde (Sie können eine GUID mit dem Cmdlet **New-Guid** erstellen). Der LCM nutzt die **ConfigurationID** zum Auffinden der entsprechenden Konfiguration auf dem Pullserver. Die MOF-Konfigurationsdatei auf dem Pullserver muss den Namen `ConfigurationID.mof` haben, wobei *ConfigurationID* der Wert der **ConfigurationID**-Eigenschaft des LCM des Zielknotens ist.

## Abrufen per Pull von einem SMB-Server

Wenn der Pullserver nicht als Webdienst, sondern als SMB-Dateifreigabe konfiguriert wurde, legen Sie **DscFileDownloadManager** anstelle von **WebDownLoadManager** fest.
**DscFileDownloadManager** verwendet eine **SourcePath**-Eigenschaft anstelle von **ServerUrl**. Das folgende Skript konfiguriert den LCM zum Abrufen von Konfigurationen von einer SMB-Freigabe namens „SmbDscShare“ auf einem Server namens „CONTOSO-SERVER“:

```powershell
Configuration SimpleMetaConfigurationForPull 
{ 
    LocalConfigurationManager 
    { 
        ConfigurationID = "1C707B86-EF8E-4C29-B7C1-34DA2190AE24";
        RefreshMode = "PULL";
        DownloadManagerName = "DscFileDownloadManager";
        RebootNodeIfNeeded = $true;
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 30; 
        ConfigurationMode = "ApplyAndAutoCorrect";
        DownloadManagerCustomData = @{ServerUrl = "\\CONTOSO-SERVER\SmbDscShare"}
    } 
} 
SimpleMetaConfigurationForPull -Output "."
```

## Weitere Informationen

- [Einrichten eines DSC-Webpullservers](pullServer.md)
- [Einrichten einer SMB DSC-Pull-server](pullServerSMB.md)




<!--HONumber=Oct16_HO1-->


