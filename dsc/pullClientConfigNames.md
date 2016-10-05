---
title: Einrichten eines Pullclients mithilfe von Konfigurationsnamen
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 02721f0f6f68cc78ae0430205d06f079e3e7465a

---

# Einrichten eines Pullclients mithilfe von Konfigurationsnamen

> Gilt für: Windows PowerShell 5.0

Jeder Zielknoten muss angewiesen werden, den Pullmodus zu verwenden. Außerdem muss die URL angegeben werden, unter der der Pullserver zum Abrufen von Konfigurationen kontaktiert werden kann. Hierzu müssen Sie den lokalen Konfigurations-Manager (LCM) mit den benötigten Informationen konfigurieren. Zum Konfigurieren des LCM erstellten Sie eine besondere Art von Konfiguration unter Angabe des **DSCLocalConfigurationManager**-Attributs. Weitere Informationen zum Konfigurieren des LCM finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md).

> **Hinweis**: Dieses Thema gilt für PowerShell 5.0. Informationen zum Einrichten eines Pullclients in PowerShell 4.0 finden Sie unter [Einrichten eines Pullclients mithilfe der Konfigurations-ID in PowerShell 4.0](pullClientConfigID4.md).

Das folgende Skript konfiguriert den LCM zum Abrufen von Konfigurationen von einem Pullserver namens „CONTOSO-PullSrv“:

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigNames
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
PullClientConfigNames
```

Im Skript definiert der **ConfigurationRepositoryWeb**-Block den Pullserver. Die **ServerURL**-Eigenschaft gibt den Endpunkt für den Pullserver an.

Die **RegistrationKey**-Eigenschaft ist ein freigegebener Schlüssel zwischen allen Clientknoten für einen Pullserver und dem jeweiligen Pullserver. Der gleiche Wert wird in einer Datei auf dem Pullserver gespeichert. 

Die **ConfigurationNames**-Eigenschaft ist ein Array, das die Namen der Konfigurationen angibt, die für den Clientknoten vorgesehen sind. Auf dem Pullserver muss die MOF-Konfigurationsdatei für diesen Clientknoten „*ConfigurationNames*.mof“ heißen, wobei *ConfigurationNames* dem Wert der **ConfigurationNames**-Eigenschaft entspricht, die Sie in dieser Metakonfiguration festlegen.

>**Hinweis:** Bei Angabe mehrerer Werte unter **ConfigurationNames** müssen Sie auch **PartialConfiguration**-Blöcke in Ihrer Konfiguration angeben. Informationen zu partiellen Konfigurationen finden Sie unter [PowerShell DSC – Teilkonfigurationen](partialConfigs.md).

Nachdem das Skript ausgeführt wurde, erstellt es einen neuen Ausgabeordner mit dem Namen **PullClientConfigNames**, der eine MOF-Datei mit der Metakonfiguration enthält. In diesem Fall heißt die MOF-Datei mit der Metakonfiguration `localhost.meta.mof`.

Rufen Sie zum Anwenden der Konfiguration das Cmdlet **Set DscLocalConfigurationManager** auf, wobei **Path** auf den Speicherort der MOF-Datei mit der Metakonfiguration festgelegt wird. Beispiel: `Set-DSCLocalConfigurationManager localhost –Path .\PullClientConfigNames –Verbose.`

> **Hinweis**: Registrierungsschlüssel funktionieren nur mit dem Webpullserver. Bei einem SMB-Pullserver müssen Sie **ConfigurationID** weiterhin verwenden. Informationen zum Konfigurieren eines Pullservers unter Verwendung von **ConfigurationID** finden Sie unter [Einrichten eines Pullclients mithilfe der Konfigurations-ID](PullClientConfigNames.md).

## Ressourcen und Berichtsserver

Wenn Sie in Ihrer LCM-Konfiguration nur einen **ConfigurationRepositoryWeb**- oder einen **ConfigurationRepositoryShare**-Block angeben (wie im vorherigen Beispiel), ruft der Pullclient Ressourcen per Pull vom angegebenen Server ab, sendet aber keine Berichte an den Server. Sie können für Konfigurationen, Ressourcen und Berichte einen einzigen Pullserver verwenden, allerdings müssen Sie einen **ReportRepositoryWeb**-Block erstellen, um die Berichterstattung einzurichten. Das folgenden Beispiel zeigt eine Metakonfiguration, mit der einen Client so eingerichtet wird, dass Konfigurationen und Ressourcen per Pull von einem Pullserver abgerufen und Berichtsdaten an denselben Pullserver gesendet werden.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigNames
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
        
        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigNames
```

Sie können für den Abruf von Ressourcen und die Berichterstattung auch unterschiedliche Pullserver angeben. Um einen Ressourcenserver anzugeben, verwenden Sie entweder einen **ResourceRepositoryWeb**-Block (für einen Webpullserver) oder einen **ResourceRepositoryShare**-Block (für einen SMB-Pullserver).
Um einen Berichtsserver anzugeben, verwenden Sie einen **ReportRepositoryWeb**-Block. Ein Berichtsserver kann kein SMB-Server sein.
Die folgende Metakonfiguration konfiguriert einen Pullclient zum Abrufen seiner Konfigurationen von **CONTOSO-PullSrv** und seiner Ressourcen von**CONTOSO-ResourceSrv** und Senden von Statusberichten an **CONTOSO-ReportSrv**.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigNames
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
            ServerURL = 'https://CONTOSO-ResourceSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '30ef9bd8-9acf-4e01-8374-4dc35710fc90'
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL = 'https://CONTOSO-ReportSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '6b392c6a-818c-4b24-bf38-47124f1e2f14'
        }
    }
}
PullClientConfigNames
```

## Weitere Informationen

* [Einrichten eines Pullclients mit Konfigurations-ID](PullClientConfigNames.md)
* [Einrichten eines DSC-Webpullservers](pullServer.md)




<!--HONumber=Oct16_HO1-->


