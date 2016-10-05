---
title: Verwenden eines DSC-Berichtsservers
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 70e0256691416427538ec7e03aa13b739d70bec7

---

# Verwenden eines DSC-Berichtsservers

> Gilt für: Windows PowerShell 5.0

>**Hinweis:** Der in diesem Thema beschriebene Berichtsserver ist in PowerShell 4.0 nicht verfügbar.

Die lokale Configuration Manager (LCM) eines Knotens kann so konfiguriert werden, dass Berichte zu seinem Konfigurationsstatus an einen Pullserver gesendet werden, der zum Abrufen dieser Daten abgefragt werden kann. Jedes Mal, wenn der Knoten eine Konfiguration überprüft und anwendet, wird ein Bericht zum Berichtsserver gesendet. Diese Berichte werden in einer Datenbank auf dem Server gespeichert und können durch Aufrufen des Webdiensts für die Berichtserstellung abgerufen werden. Jeder Bericht enthält Informationen zu den angewendeten Konfigurationen samt Zeitpunkt, verwendeten Ressourcen, ausgelösten Fehlern sowie Start- und Endzeiten.

## Konfigurieren eines Knotens zum Senden von Berichten

Sie weisen einen Knoten im **ReportServerWeb**-Block der LCM-Konfiguration des Knotens an, Berichte zu einem Server zu senden (weitere Informationen zum Konfigurieren des LCM finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md)). Der Server, an den der Knoten Berichte sendet, muss als Webpullserver eingerichtet werden (Sie können keine Berichte an eine SMB-Freigabe senden). Weitere Informationen zum Einrichten von Pullservern finden Sie unter [Einrichten eines DSC-Webpullservers](pullServer.md). Der Berichtsserver kann vom selben Dienst unterstützt werden, von dem der Knoten Konfigurationen und Ressourcen abruft. Es kann sich aber auch um einen anderen Dienst handeln.
 
Im **ReportServerWeb**-Block geben Sie die URL des Pulldiensts und einen Registrierungsschlüssel an, der dem Server bekannt ist.
 
Die folgende Konfiguration konfiguriert einen Knoten für das Abrufen von Konfiguration per Pull von einem Dienst und Senden von Berichten an einen Dienst auf einem anderen Server. 
 
```powershell
[DSCLocalConfigurationManager()]
configuration ReportClientConfig
{
    Node localhost
    {
        Settings
        {
            RefreshMode          = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded   = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL          = 'https://CONTOSO-PULL:8080/PSDSCPullServer.svc'
            RegistrationKey    = 'bbb9778f-43f2-47de-b61e-a0daff474c6d'
            ConfigurationNames = @('ClientConfig')
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL               = 'http://CONTOSO-REPORT:8080/PSDSCReportServer.svc'
            RegistrationKey         = 'ba39daaa-96c5-4f2f-9149-f95c46460faa'
            AllowUnsecureConnection = $true
        }
    }
}
ReportClientConfig
```

Mit der folgenden Konfiguration wird ein Knoten so eingerichtet, dass er einen einzigen Server für Konfigurationen, Ressourcen und Berichterstattung verwendet.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfig
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
        
        

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfig
```

>**Hinweis:** Sie können den Webdienst beim Einrichten eines Pullservers beliebig benennen, aber die Eigenschaft **ServerURL** muss mit dem Dienstnamen übereinstimmen.

## Abrufen von Berichtsdaten

Berichte, die zum Pullserver gesendet werden, gelangen in eine Datenbank auf dem Server. Die Berichte stehen über Aufrufe des Webdiensts zur Verfügung. Zum Abrufen von Berichten für einen bestimmten Knoten senden Sie eine HTTP-Anforderung an den Berichtswebdienst in der folgenden Form: `http://CONTOSO-REPORT:8080/PSDSCReportServer.svc/Nodes(AgentID = MyNodeAgentId)/Reports`, wobei `MyNodeAgentId` die Agent-ID des Knotens ist, für den Sie Berichte erhalten möchten. Durch Aufrufen von [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx) auf einem Knoten können Sie die Agent-ID dieses Knotens abrufen.

Die Berichte werden als ein Array von JSON-Objekten zurückgegeben.

Das folgende Skript gibt die Berichte für den Knoten zurück, auf dem es ausgeführt wird:

```powershell
function GetReport
{
    param($AgentId = "$((glcm).AgentId)", $serviceURL = "http://CONTOSO-REPORT:8080/PSDSCReportServer.svc")
    $requestUri = "$serviceURL/Nodes(AgentId= '$AgentId')/Reports"
    $request = Invoke-WebRequest -Uri $requestUri  -ContentType "application/json;odata=minimalmetadata;streaming=true;charset=utf-8" `
               -UseBasicParsing -Headers @{Accept = "application/json";ProtocolVersion = "2.0"} `
               -ErrorAction SilentlyContinue -ErrorVariable ev
    $object = ConvertFrom-Json $request.content
    return $object.value
}
```
    
## Anzeigen von Berichtsdaten

Wenn Sie eine Variable auf das Ergebnis der **GetReport**-Funktion festlegen, können Sie die einzelnen Felder in einem Element des Arrays anzeigen, das zurückgegeben wird:

```powershell
$reports = GetReport
$reports[1]


JobId                : 019dfbe5-f99f-11e5-80c6-001dd8b8065c
OperationType        : Consistency
RefreshMode          : Pull
Status               : Success
ReportFormatVersion  : 2.0
ConfigurationVersion : 2.0.0
StartTime            : 04/03/2016 06:21:43
EndTime              : 04/03/2016 06:22:04
RebootRequested      : False
Errors               : {}
StatusData           : {{"StartDate":"2016-04-03T06:21:43.7220000-07:00","IPV6Addresses":["2001:4898:d8:f2f2:852b:b255:b071:283b","fe80::852b:b255:b071
                       :283b%12","::2000:0:0:0","::1","::2000:0:0:0"],"DurationInSeconds":"21","JobID":"{019DFBE5-F99F-11E5-80C6-001DD8B8065C}","Curren
                       tChecksum":"A7797571CB9C3AF4D74C39A0FDA11DAF33273349E1182385528FFC1E47151F7F","MetaData":"Author: configAuthor; Name: 
                       Sample_ArchiveFirewall; Version: 2.0.0; GenerationDate: 04/01/2016 15:23:30; GenerationHost: CONTOSO-PullSrv;","RebootRequested":"False
                       ","Status":"Success","IPV4Addresses":["10.240.179.151","127.0.0.1"],"LCMVersion":"2.0","ResourcesNotInDesiredState":[{"SourceInf
                       o":"C:\\ReportTest\\Sample_xFirewall_AddFirewallRule.ps1::23::9::xFirewall","ModuleName":"xNetworking","DurationInSeconds":"8.785",
                       "InstanceName":"Firewall","StartDate":"2016-04-03T06:21:56.4650000-07:00","ResourceName":"xFirewall","ModuleVersion":"2.7.0.0","
                       RebootRequested":"False","ResourceId":"[xFirewall]Firewall","ConfigurationName":"Sample_ArchiveFirewall","InDesiredState":"False
                       "}],"NumberOfResources":"2","Type":"Consistency","HostName":"CONTOSO-PULLCLI","ResourcesInDesiredState":[{"SourceInfo":"C:\\ReportTest\\Sample_xFirewall_AddFirewallRule.ps1::16::9::Archive","ModuleName":"PSDesiredStateConfiguration","DurationInSeconds":"1.848",
                       "InstanceName":"ArchiveExample","StartDate":"2016-04-03T06:21:56.4650000-07:00","ResourceName":"Archive","ModuleVersion":"1.1","
                       RebootRequested":"False","ResourceId":"[Archive]ArchiveExample","ConfigurationName":"Sample_ArchiveFirewall","InDesiredState":"T
                       rue"}],"MACAddresses":["00-1D-D8-B8-06-5C","00-00-00-00-00-00-00-E0"],"MetaConfiguration":{"AgentId":"52DA826D-00DE-4166-8ACB-73F2B46A7E00",
                       "ConfigurationDownloadManagers":[{"SourceInfo":"C:\\ReportTest\\LCMConfig.ps1::14::9::ConfigurationRepositoryWeb","A
                       llowUnsecureConnection":"True","ServerURL":"http://CONTOSO-PullSrv:8080/PSDSCPullServer.svc","RegistrationKey":"","ResourceId":"[Config
                       urationRepositoryWeb]CONTOSO-PullSrv","ConfigurationNames":["ClientConfig"]}],"ActionAfterReboot":"ContinueConfiguration","LCMCo
                       mpatibleVersions":["1.0","2.0"],"LCMState":"Idle","ResourceModuleManagers":[],"ReportManagers":[{"AllowUnsecureConnection":"True
                       ","RegistrationKey":"","ServerURL":"http://CONTOSO-PullSrv:8080/PSDSCPullServer.svc","ResourceId":"[ReportServerWeb]CONTOSO-PullSrv","S
                       ourceInfo":"C:\\ReportTest\\LCMConfig.ps1::24::9::ReportServerWeb"}],"StatusRetentionTimeInDays":"10","LCMVersion":"2.0","Config
                       urationMode":"ApplyAndMonitor","RefreshFrequencyMins":"30","RebootNodeIfNeeded":"True","RefreshMode":"Pull","DebugMode":["NONE"]
                       ,"LCMStateDetail":"","AllowModuleOverwrite":"False","ConfigurationModeFrequencyMins":"15"},"Locale":"en-US","Mode":"Pull"}}
AdditionalData       : {}
```

Standardmäßig werden die Berichte nach **JobID** sortiert. Zum Abrufen des neuesten Berichts können Sie die Berichte absteigend nach der Eigenschaft **StartTime** sortieren, und dann das erste Element des Arrays abrufen:

```powershell
$reportsByStartTime = $reports | Sort-Object {$_."StartTime" -as [DateTime] } -Descending
$reportMostRecent = $reportsByStartTime[0]
```

Beachten Sie, dass das Feld **StatusData** ein Objekt mit mehreren Eigenschaften ist. Hier befindet sich ein Großteil der Berichtsdaten. Sehen wir uns die einzelnen Felder der Eigenschaft **StatusData** für den aktuellen Bericht an:

```powershell
$statusData = $reportMostRecent.StatusData | ConvertFrom-Json
$statusData

StartDate                  : 2016-04-04T11:21:41.2990000-07:00
IPV6Addresses              : {2001:4898:d8:f2f2:852b:b255:b071:283b, fe80::852b:b255:b071:283b%12, ::2000:0:0:0, ::1...}
DurationInSeconds          : 25
JobID                      : {135D230E-FA92-11E5-80C6-001DD8B8065C}
CurrentChecksum            : A7797571CB9C3AF4D74C39A0FDA11DAF33273349E1182385528FFC1E47151F7F
MetaData                   : Author: configAuthor; Name: Sample_ArchiveFirewall; Version: 2.0.0; GenerationDate: 04/01/2016 15:23:30; GenerationHost: 
                             CONTOSO-PullSrv;
RebootRequested            : False
Status                     : Success
IPV4Addresses              : {10.240.179.151, 127.0.0.1}
LCMVersion                 : 2.0
ResourcesNotInDesiredState : {@{SourceInfo=C:\ReportTest\Sample_xFirewall_AddFirewallRule.ps1::23::9::xFirewall; ModuleName=xNetworking; 
                             DurationInSeconds=10.725; InstanceName=Firewall; StartDate=2016-04-04T11:21:55.7200000-07:00; ResourceName=xFirewall; 
                             ModuleVersion=2.7.0.0; RebootRequested=False; ResourceId=[xFirewall]Firewall; ConfigurationName=Sample_ArchiveFirewall; 
                             InDesiredState=False}}
NumberOfResources          : 2
Type                       : Consistency
HostName                   : CONTOSO-PULLCLI
ResourcesInDesiredState    : {@{SourceInfo=C:\ReportTest\Sample_xFirewall_AddFirewallRule.ps1::16::9::Archive; ModuleName=PSDesiredStateConfiguration; 
                             DurationInSeconds=2.672; InstanceName=ArchiveExample; StartDate=2016-04-04T11:21:55.7200000-07:00; ResourceName=Archive; 
                             ModuleVersion=1.1; RebootRequested=False; ResourceId=[Archive]ArchiveExample; ConfigurationName=Sample_ArchiveFirewall; 
                             InDesiredState=True}}
MACAddresses               : {00-1D-D8-B8-06-5C, 00-00-00-00-00-00-00-E0}
MetaConfiguration          : @{AgentId=52DA826D-00DE-4166-8ACB-73F2B46A7E00; ConfigurationDownloadManagers=System.Object[]; 
                             ActionAfterReboot=ContinueConfiguration; LCMCompatibleVersions=System.Object[]; LCMState=Idle; 
                             ResourceModuleManagers=System.Object[]; ReportManagers=System.Object[]; StatusRetentionTimeInDays=10; LCMVersion=2.0; 
                             ConfigurationMode=ApplyAndMonitor; RefreshFrequencyMins=30; RebootNodeIfNeeded=True; RefreshMode=Pull; 
                             DebugMode=System.Object[]; LCMStateDetail=; AllowModuleOverwrite=False; ConfigurationModeFrequencyMins=15}
Locale                     : en-US
Mode                       : Pull
```

Unter anderem wird hier deutlich, dass die neueste Konfiguration zwei Ressourcen aufgerufen hat, von denen sich die eine im gewünschten Zustand befand und die andere nicht. So können Sie eine besser lesbare Ausgabe der Eigenschaft **ResourcesNotInDesiredState** erzeugen:

```powershell
$statusData.ResourcesInDesiredState

SourceInfo        : C:\ReportTest\Sample_xFirewall_AddFirewallRule.ps1::16::9::Archive
ModuleName        : PSDesiredStateConfiguration
DurationInSeconds : 2.672
InstanceName      : ArchiveExample
StartDate         : 2016-04-04T11:21:55.7200000-07:00
ResourceName      : Archive
ModuleVersion     : 1.1
RebootRequested   : False
ResourceId        : [Archive]ArchiveExample
ConfigurationName : Sample_ArchiveFirewall
InDesiredState    : True
```

Beachten Sie, dass diese Beispiele dazu dienen, Ihnen eine Vorstellung der Möglichkeiten von Berichtsdaten zu geben. Eine Einführung in das Arbeiten mit JSON in PowerShell finden Sie unter [Arbeiten mit JSON und PowerShell](https://blogs.technet.microsoft.com/heyscriptingguy/2015/10/08/playing-with-json-and-powershell/).

## Weitere Informationen
- [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md)
- [Einrichten eines DSC-Webpullservers](pullServer.md)
- [Einrichten eines Pullclients mithilfe von Konfigurationsnamen](pullClientConfigNames.md)




<!--HONumber=Oct16_HO1-->


