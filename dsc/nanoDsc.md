---
title: Verwenden von DSC unter Nano Server
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 962941ba946a67256baf141bd195361c94a68f90

---

# Verwenden von DSC unter Nano Server

> Gilt für: Windows PowerShell 5.0

**DSC unter Nano Server** ist ein optionales Paket im Ordner `NanoServer\Packages` des Windows Server 2016-Mediums. Das Paket kann installiert werden, wenn Sie eine VHD (virtuelle Festplatte) für einen Nano Server erstellen, indem Sie **Microsoft-NanoServer-DSC-Package** als Wert des Parameters **Packages** der Funktion **New-NanoServerImage** angeben. Wenn Sie beispielsweise eine virtuelle Festplatte für einen virtuellen Computer erstellen, würde der Befehl wie folgt aussehen:

```powershell
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1 -Packages Microsoft-NanoServer-DSC-Package
```

Informationen zum Installieren und Verwenden von Nano Server sowie zum Verwalten von Nano Server mit PowerShell-Remoting finden Sie unter [Erste Schritte mit Nano Server](https://technet.microsoft.com/en-us/library/mt126167.aspx).


## Unter Nano Server verfügbare DSC-Funktionen

 Da Nano Server nur eine begrenzte Anzahl von APIs, die im Vergleich zu einer Vollversion von Windows Server unterstützt, muss DSC auf Nano Server nicht vollständig funktionelle Parität mit DSC auf vollständige SKUs, die zurzeit ausgeführt wird. DSC unter Nano Server befindet sich in der aktiven Entwicklung, und der Funktionsumfang ist noch nicht vollständig.
 
 Die folgenden DSC-Funktionen sind zurzeit unter Nano Server verfügbar: 


* Sowohl Push- als auch Pull-Modus

* Alle DSC-Cmdlets, die eine Vollversion von Windows Server beinhaltet, einschließlich der folgenden: 
  * [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx)
  * [Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx)   
  * [Enable-DscDebug](https://technet.microsoft.com/en-us/library/mt517870.aspx)
  * [Disable-DscDebug](https://technet.microsoft.com/en-us/library/mt517872.aspx)       
  * [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx)
  * [Stop-DscConfiguration](https://technet.microsoft.com/en-us/library/mt143542.aspx)
  * [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407379.aspx)
  * [Test-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx)      
  * [Veröffentlichen von DscConfiguraiton](https://technet.microsoft.com/en-us/library/mt517875.aspx) 
  * [Update-DscConfiguration](https://technet.microsoft.com/en-us/library/mt143541.aspx)
  * [Restore-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407383.aspx)
  * [Remove-DscConfigurationDocument](https://technet.microsoft.com/en-us/library/mt143544.aspx)
  * [Get-DscConfigurationStatus](https://technet.microsoft.com/en-us/library/mt517868.aspx)
  * [Invoke-DscResource](https://technet.microsoft.com/en-us/library/mt517869.aspx)
  * [Suchen-DscResource](https://technet.microsoft.com/en-us/library/mt517874.aspx)
  * [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx)
  * [Neue DscChecksum](https://technet.microsoft.com/en-us/library/dn521622.aspx)    

* Kompilieren von Konfigurationen (siehe [DSC-Konfigurationen](configurations.md))

  **Problem:** Die Kennwortverschlüsselung (siehe [Schützen der MOF-Datei](securemof.md)) während der Konfigurationskompilierung funktioniert nicht.

* Kompilieren von Metakonfigurationen (siehe [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md)

* Ausführen einer Ressource unter dem Benutzerkontext (siehe [Ausführen von DSC mit Benutzeranmeldeinformationen („Ausführen als“, RunAs)](runAsUser.md))

* Klassenbasierte Ressourcen (siehe [Schreiben einer benutzerdefinierten DSC-Ressource mit PowerShell-Klassen](authoringResourceClass.md))

* Debuggen von DSC-Ressourcen (siehe [Debuggen von DSC-Ressourcen](debugresource.md))
  
  **Problem:** Funktioniert nicht, wenn eine Ressource PsDscRunAsCredential verwendet (siehe [Ausführen von DSC mit Benutzeranmeldeinformationen](runAsUser.md))

* [Die knotenübergreifende Abhängigkeiten angeben](crossNodeDependencies.md) 

* [Ressource-versioning](sxsResource.md)

* Pullclient (Konfigurationen und Ressourcen) (siehe [Einrichten eines Pullclients mithilfe von Konfigurationsnamen](pullClientConfigNames.md))

* [Teilweise Konfigurationen (Pull- und Push)](partialConfigs.md)

* [Berichterstattung zur Pull-server](reportServer.md) 

* MOF-Verschlüsselung

* Protokollierung

* Azure Automation-DSC-Berichte

* Ressourcen, die voll funktionsfähig sind
  * [Archiv](archiveResource.md)
  * [Umgebung](environmentResource.md)
  * [Datei](fileResource.md)
  * [Protokoll](logResource.md)
  * ProcessSet
  * [Registrierung](registryResource.md)
  * [Skript](scriptResource.md)
  * WindowsPackageCab
  * [WindowsProcess](windowsProcessResource.md)
  * WaitForAll (siehe [Angeben knotenübergreifender Abhängigkeiten](crossNodeDependencies.md))
  * WaitForAny (siehe [Angeben knotenübergreifender Abhängigkeiten](crossNodeDependencies.md))
  * WaitForSome (siehe [Angeben knotenübergreifender Abhängigkeiten](crossNodeDependencies.md))

* Ressourcen, die teilweise funktionsfähig sind
  * [Gruppe](groupResource.md)
  * GroupSet
  
  **Problem:** Bei den oben genannten Ressourcen tritt ein Fehler auf, wenn eine bestimmte Instanz zweimal aufgerufen wird (bzw. wenn die gleiche Konfiguration zweimal ausgeführt wird)
  
  * [Dienst](serviceResource.md)
  * ServiceSet
  
  **Problem:** Funktioniert nur beim Starten/Beenden (Status) des Diensts. Schlägt fehl, wenn versucht wird, andere Attribute des Diensts zu ändern, z. B. Starttyp, Anmeldeinformationen, Beschreibung usw. Ein Fehler mit etwa folgendem Wortlaut wird ausgelöst:
  
  *Typ [management.managementobject] wurde nicht gefunden: Stellen Sie sicher, dass die Assembly mit diesem Typ geladen wird.*
  
* Ressourcen, die nicht funktionsfähig sind
  * [Benutzer](userResource.md)
  

## Unter Nano Server nicht verfügbare DSC-Funktionen

Die folgenden DSC-Funktionen sind zurzeit unter Nano Server nicht verfügbar:

* Entschlüsseln von MOF-Dokumenten mit verschlüsselten Kennwörtern 
* Pullserver: Sie können zurzeit keinen Pullserver unter Nano Server einrichten.
* Alle Funktionen, die nicht in der Liste der Funktionen aufgeführt sind, funktionieren.

## Verwenden benutzerdefinierter DSC-Ressourcen unter Nano Server
 
Aufgrund eingeschränkter Sätze von Windows-APIs und CLR-Bibliotheken, die unter Nano Server verfügbar sind, funktionieren DSC-Ressourcen, die in der vollständigen CLR-Version von Windows funktionieren, nicht notwendigerweise auch unter Nano Server. Führen Sie zuerst End-to-End-Tests durch, bevor Sie benutzerdefinierte DSC-Ressourcen in einer Produktionsumgebung bereitstellen.

## Weitere Informationen
- [Erste Schritte mit Nano Server](https://technet.microsoft.com/en-us/library/mt126167.aspx)




<!--HONumber=Oct16_HO1-->


