---
title: Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: f6569220fbafdba49bac9ac9dca3e6036a7aad08

---

# Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID

> Gilt für: Windows PowerShell 5.0

Jeder Zielknoten muss angewiesen werden, den Pullmodus zu verwenden. Außerdem muss die URL angegeben werden, unter der der Pullserver zum Abrufen von Konfigurationen kontaktiert werden kann. Hierzu müssen Sie den lokalen Konfigurations-Manager (LCM) mit den benötigten Informationen konfigurieren. Zum Konfigurieren des LCM erstellten Sie eine besondere Art von Konfiguration unter Angabe des **DSCLocalConfigurationManager**-Attributs. Weitere Informationen zum Konfigurieren des LCM finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md).

> **Hinweis**: Dieses Thema gilt für PowerShell 5.0. Informationen zum Einrichten eines Pullclients in PowerShell 4.0 finden Sie unter [Einrichten eines Pullclients mithilfe der Konfigurations-ID in PowerShell 4.0](pullClientConfigID4.md).

Das folgende Skript konfiguriert den LCM zum Abrufen von Konfigurationen von einem Pullserver namens „CONTOSO-PullSrv“.

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
    }
}
PullClientConfigID
```

Im Skript definiert der **ConfigurationRepositoryWeb**-Block den Pullserver. Die **ServerURL**

Nachdem das Skript ausgeführt wurde, erstellt es einen neuen Ausgabeordner mit dem Namen **PullClientConfigID**, der eine MOF-Datei mit der Metakonfiguration enthält. In diesem Fall heißt die MOF-Datei mit der Metakonfiguration `localhost.meta.mof`.

Rufen Sie zum Anwenden der Konfiguration das Cmdlet **Set DscLocalConfigurationManager** auf, wobei **Path** auf den Speicherort der MOF-Datei mit der Metakonfiguration festgelegt wird. Beispiel: `Set-DSCLocalConfigurationManager localhost –Path .\PullClientConfigID –Verbose.`

## Konfigurations-ID

Das Skript legt die **ConfigurationID**-Eigenschaft des LCM auf eine GUID fest, die zuvor für diesen Zweck erstellt wurde (Sie können eine GUID mit dem Cmdlet **New-Guid** erstellen). Der LCM nutzt die **ConfigurationID** zum Auffinden der entsprechenden Konfiguration auf dem Pullserver. Die MOF-Konfigurationsdatei auf dem Pullserver muss den Namen _ConfigurationID.mof_ haben, wobei _ConfigurationID_ der Wert der **ConfigurationID**-Eigenschaft des LCM des Zielknotens ist.

## SMB-Pullserver

Um einen Client zum Abrufen von Konfigurationen per Pull von einem SMB-Server einzurichten, verwenden Sie einen **ConfigurationRepositoryShare**-Block. Im **ConfigurationRepositoryShare**-Block geben Sie den Pfad zum Server durch Festlegen der **SourcePath**-Eigenschaft an. Die folgende Metakonfiguration konfiguriert den Zielknoten zum Abrufen von Daten von einem SMB-Pullserver mit dem Namen **SMBPullServer**.

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

## Ressourcen und Berichtsserver

Wenn Sie in Ihrer LCM-Konfiguration nur einen **ConfigurationRepositoryWeb**- oder einen **ConfigurationRepositoryShare**-Block angeben (wie im vorherigen Beispiel), ruft der Pullclient Ressourcen per Pull vom angegebenen Server ab, sendet aber keine Berichte an den Server. Sie können für Konfigurationen, Ressourcen und Berichte einen einzigen Pullserver verwenden, allerdings müssen Sie einen **ReportRepositoryWeb**-Block erstellen, um die Berichterstattung einzurichten. 

Das folgenden Beispiel zeigt eine Metakonfiguration, mit der einen Client so eingerichtet wird, dass Konfigurationen und Ressourcen per Pull von einem Pullserver abgerufen und Berichtsdaten an denselben Pullserver gesendet werden.

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
        
        
        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

Sie können für den Abruf von Ressourcen und die Berichterstattung auch unterschiedliche Pullserver angeben. Um einen Ressourcenserver anzugeben, verwenden Sie entweder einen **ResourceRepositoryWeb**-Block (für einen Webpullserver) oder einen **ResourceRepositoryShare**-Block (für einen SMB-Pullserver).
Um einen Berichtsserver anzugeben, verwenden Sie einen **ReportRepositoryWeb**-Block. Ein Berichtsserver kann kein SMB-Server sein.
Die folgende Metakonfiguration konfiguriert einen Pullclient zum Abrufen seiner Konfigurationen von **CONTOSO-PullSrv** und seiner Ressourcen von**CONTOSO-ResourceSrv** und Senden von Statusberichten an **CONTOSO-ReportSrv**.

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

## Weitere Informationen

* [Einrichten eines Pullclients mit Konfigurationsnamen](pullClientConfigNames.md)




<!--HONumber=Oct16_HO1-->


