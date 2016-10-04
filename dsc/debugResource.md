---
title: Debuggen von DSC-Ressourcen
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 461cd535bfbdda6cc90364aace9912f713431c21

---

# Debuggen von DSC-Ressourcen

> Gilt für: Windows PowerShell 5.0

In PowerShell 5.0 wurde ein neues Feature in DSC (Konfiguration für den gewünschten Zustand) eingeführt, mit dem Sie eine DSC-Ressource beim Anwenden einer Konfiguration debuggen können.

## Aktivieren des DSC-Debuggens
Bevor Sie eine Ressource debuggen können, müssen Sie das Cmdlet [Enable-DscDebug](https://technet.microsoft.com/en-us/library/mt517870.aspx) aufrufen, um das Debuggen zu aktivieren. Dieses Cmdlet verfügt über den Pflichtparameter **BreakAll**. 

Um zu überprüfen, ob das Debuggen aktiviert wurde, werfen Sie einen Blick auf das Ergebnis eines Aufrufs von [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx). Die folgende PowerShell-Ausgabe zeigt das Ergebnis der Aktivierung des Debuggens an:


```powershell
PS C:\DebugTest> $LCM = Get-DscLocalConfigurationManager

PS C:\DebugTest> $LCM.DebugMode
NONE

PS C:\DebugTest> Enable-DscDebug -BreakAll

PS C:\DebugTest> $LCM = Get-DscLocalConfigurationManager

PS C:\DebugTest> $LCM.DebugMode
ForceModuleImport
ResourceScriptBreakAll

PS C:\DebugTest>
```


## Starten einer Konfiguration mit aktiviertem Debuggen
Um eine DSC-Ressource zu debuggen, starten Sie eine Konfiguration, die diese Ressource aufruft. In diesem Beispiel betrachten wir eine einfache Konfiguration, die die Ressource [WindowsFeature](windowsfeatureResource.md) aufruft, um sicherzustellen, dass das Feature „WindowsPowerShellWebAccess“ installiert wurde:

```powershell
Configuration PSWebAccess
    {
    Import-DscResource -ModuleName 'PsDesiredStateConfiguration'
    Node localhost
        {
        WindowsFeature PSWA
            {
            Name = 'WindowsPowerShellWebAccess'
            Ensure = 'Present'
            }
        }
    }
PSWebAccess
```
Rufen Sie nach dem Kompilieren der Konfiguration [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) auf, um die Konfiguration zu starten. Die Konfiguration wird beendet, wenn der lokale Konfigurations-Manager die erste Ressource in der Konfiguration aufruft. Bei Verwendung der Parameter `-Verbose` und `-Wait` werden in der Ausgabe die Zeilen angezeigt, die Sie eingeben müssen, um das Debuggen zu starten.

```powershell
PS C:\DebugTest> Start-DscConfiguration .\PSWebAccess -Wait -Verbose
VERBOSE: Perform operation 'Invoke CimMethod' with following parameters, ''methodName' = SendConfigurationApply,'className' = MSFT_DSCLocalConfiguration
Manager,'namespaceName' = root/Microsoft/Windows/DesiredStateConfiguration'.
VERBOSE: An LCM method call arrived from computer TEST-SRV with user sid S-1-5-21-2127521184-1604012920-1887927527-108583.
VERBOSE: An LCM method call arrived from computer TEST-SRV with user sid S-1-5-21-2127521184-1604012920-1887927527-108583.
VERBOSE: [TEST-SRV]: LCM:  [ Start  Set      ]
WARNING: [TEST-SRV]:                            [DSCEngine] Warning LCM is in Debug 'ResourceScriptBreakAll' mode.  Resource script processing will 
be stopped to wait for PowerShell script debugger to attach.
VERBOSE: [TEST-SRV]:                            [DSCEngine] Importing the module C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateCo
nfiguration\DscResources\MSFT_RoleResource\MSFT_RoleResource.psm1 in force mode.
VERBOSE: [TEST-SRV]: LCM:  [ Start  Resource ]  [[WindowsFeature]PSWA]
VERBOSE: [TEST-SRV]: LCM:  [ Start  Test     ]  [[WindowsFeature]PSWA]
VERBOSE: [TEST-SRV]:                            [[WindowsFeature]PSWA] Importing the module MSFT_RoleResource in force mode.
WARNING: [TEST-SRV]:                            [[WindowsFeature]PSWA] Resource is waiting for PowerShell script debugger to attach.  Use the follow
ing commands to begin debugging this resource script:
Enter-PSSession -ComputerName TEST-SRV -Credential <credentials>
Enter-PSHostProcess -Id 9000 -AppDomainName DscPsPluginWkr_AppDomain
Debug-Runspace -Id 9
```
An diesem Punkt hat der lokale Konfigurations-Manager die Ressource aufgerufen und den ersten Haltepunkt erreicht. Die letzten drei Zeilen in der Ausgabe zeigen, wie das Ressourcenskript an den Prozess angefügt und das Debuggen gestartet wird.

## Debuggen des Ressourcenskripts

Starten Sie eine neue Instanz von PowerShell ISE. Geben Sie im Konsolenbereich die letzten drei Zeilen der `Start-DscConfiguration`-Ausgabe als Befehle ein, wobei Sie `<credentials>` durch gültige Benutzeranmeldeinformationen ersetzen. Jetzt sollte eine Eingabeaufforderung angezeigt werden, die in etwa so aussieht:

```powershell
[TEST-SRV]: [DBG]: [Process:9000]: [RemoteHost]: PS C:\DebugTest>>
```

Das Ressourcenskript wird im Skriptfenster geöffnet, und der Debugger wird in der ersten Zeile der Funktion **Test-TargetResource** angehalten (der **Test()**-Methode einer klassenbasierten Ressource).
Jetzt können Sie die Debugbefehle in der ISE verwenden, um das Ressourcenskript schrittweise zu durchlaufen, sich die Variablenwerte anzuschauen, die Aufrufliste anzuzeigen, usw. Weitere Informationen zum Debuggen in PowerShell ISE finden Sie unter [So wird's gemacht: Debuggen von Skripts in Windows PowerShell ISE](https://technet.microsoft.com/en-us/library/dd819480.aspx). Denken Sie daran, dass jede Zeile im Ressourcenskript (oder in der Klasse) als Haltepunkt festgelegt ist.

## Deaktivieren des DSC-Debuggens

Nach Aufrufen von [Enable-DscDebug](https://technet.microsoft.com/en-us/library/mt517870.aspx), führen alle Aufrufe von [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) dazu, dass die Konfiguration unterbrochen und der Debugger gestartet wird. Damit Konfigurationen wie gewohnt ausgeführt werden können, müssen Sie das Debuggen durch Aufrufen des Cmdlets [Disable-DscDebug](https://technet.microsoft.com/en-us/library/mt517872.aspx) deaktivieren.

>**Hinweis:** Ein Neustart ändert den Debugzustand des LCM nicht. Wenn das Debuggen aktiviert ist, wird nach einem Neustart weiterhin das Starten einer Konfiguration unterbrochen und der Debugger gestartet.


## Weitere Informationen
- [Schreiben einer benutzerdefinierten DSC-Ressource mit MOF](authoringResourceMOF.md) 
- [Schreiben einer benutzerdefinierten DSC-Ressource mit PowerShell-Klassen](authoringResourceClass.md)




<!--HONumber=Oct16_HO1-->


