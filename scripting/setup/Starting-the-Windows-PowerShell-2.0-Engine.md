---
title: Starten des Windows PowerShell 2.0-Moduls
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: edafc2fa-7576-49c2-bbba-9336f4bcfc28
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 094c3c9f240457fc884031e7d82dcdc1e81e582d

---

# Starten des Windows PowerShell 2.0-Moduls
In diesem Abschnitt wird erklärt, wie Sie Windows PowerShell 2.0 Engine unter Windows 8.1, Windows Server 2012 R2, Windows 8 und Windows Server 2012 starten. Bei diesen ist Windows PowerShell 2.0 Engine bereits installiert. Sie erfahren darüber hinaus, wie Sie Windows PowerShell 2.0 Engine auf anderen Systemen starten, auf denen Windows PowerShell 2.0, Windows PowerShell 3.0, und Windows PowerShell 4.0 installiert sind.

Windows PowerShell 4.0 und Windows PowerShell 3.0 sind mit Windows PowerShell 2.0 abwärtskompatibel. Cmdlets, Anbieter, Snap-Ins, Module und Skripts, die für Windows PowerShell 2.0 geschrieben wurden, können unverändert in Windows PowerShell 4.0 und in Windows PowerShell 3.0 ausgeführt werden. Doch aufgrund einer Änderung der Richtlinie für die Laufzeitaktivierung in Microsoft .NET Framework 4 können Windows PowerShell-Hostprogramme, die für Windows PowerShell 2.0 geschrieben und mit Common Language Runtime (CLR) 2.0 kompiliert wurden, nicht unverändert in Windows PowerShell 3.0 oder Windows PowerShell 4.0 ausgeführt werden, da die mit CLR 4.0 kompiliert wurden. Windows PowerShell 2.0 Engine sollte nur verwendet werden, wenn ein vorhandenes Skript oder Hostprogramm nicht ausgeführt werden kann, da es inkompatibel mit Windows PowerShell 4.0, Windows PowerShell 3.0, oder Microsoft .NET Framework 4 ist. Solche Fälle sind allerdings eher selten.

Viele Programme, die Windows PowerShell 2.0 Engine benötigen, starten sie automatisch. Diese Anweisungen gelten für die seltenen Fälle, in denen Sie das Modul manuell starten müssen.

## Installieren und Aktivieren erforderlicher Programme
Aktivieren Sie Windows PowerShell 2.0 Engine und Microsoft .NET Framework 3.5 mit Service Pack 1, bevor Sie Windows PowerShell 2.0 Engine starten. Anweisungen hierzu finden Sie unter [Installieren von Windows PowerShell](Installing-Windows-PowerShell.md).

Systeme, auf denen [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) oder Windows Management Framework 3.0 installiert sind, verfügen über alle erforderlichen Komponenten. Es ist keine weitere Konfiguration erforderlich. Informationen zum Installieren von [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) oder Windows Management Framework 3.0 finden Sie unter [Installieren von Windows PowerShell](Installing-Windows-PowerShell.md).

## So starten Sie Windows PowerShell 2.0 Engine
Beim Starten von Windows PowerShell wird die neueste Version standardmäßig gestartet. Verwenden Sie den „Version“-Parameter von „PowerShell.exe“, um Windows PowerShell mit Windows PowerShell 2.0 Engine zu starten. Sie können den Befehl an jeder Eingabeaufforderung einschließlich Windows PowerShell und „Cmd.exe“ ausführen.

```
PowerShell.exe -Version 2
```

## So starten Sie eine Remotesitzung mit Windows PowerShell 2.0 Engine
Erstellen Sie auf dem Remotecomputer eine Sitzungskonfiguration (auch „Endpunkt“ genannt), die Windows PowerShell 2.0 Engine lädt, um Windows PowerShell 2.0 Engine in einer Remotesitzung auszuführen. Die Sitzungskonfiguration wird auf dem Remotecomputer gespeichert und kann von jedem autorisierten Benutzer verwendet werden, um Sitzungen zu erstellen, die Windows PowerShell 2.0 Engine verwenden.

Dies ist eine komplexe Aufgabe, die in der Regel von einem Systemadministrator ausgeführt wird.

Im folgenden Verfahren wird der **PSVersion**-Parameter des Cmdlets [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) zum Erstellen einer Sitzungskonfiguration verwendet, die Windows PowerShell 2.0 Engine nutzt. Sie können auch den **PowerShellVersion**-Parameter des Cmdlets [New-PSSessionConfigurationFile](https://technet.microsoft.com/en-us/library/5f3e3633-6e90-479c-aea9-ba45a1954866) verwenden, um eine Sitzungskonfigurationsdatei für eine Sitzung zu erstellen, die Windows PowerShell 2.0 Engine lädt. Sie können auch den **PSVersion**-Parameter des Parameters [Set-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/b21fbad3-1759-4260-b206-dcb8431cd6ea) verwenden, um eine Sitzungskonfiguration so zu ändern, dass Windows PowerShell 2.0 Engine verwendet wird.

Weitere Informationen zu Sitzungskonfigurationsdateien finden Sie unter [about_Session_Configuration_Files](https://technet.microsoft.com/en-us/library/c7217447-1ebf-477b-a8ef-4dbe9a1473b8). Informationen zu Sitzungskonfigurationen, einschließlich Einrichtung und Sicherheit, finden Sie unter [about_Session_Configurations [v4]](https://technet.microsoft.com/en-us/library/a2fbe12a-350c-4d04-be50-24102824e3ab).

#### Gehen Sie folgendermaßen vor, um eine Remotesitzung von Windows PowerShell 2.0 zu starten

1.  Verwenden Sie den **PSVersion**-Parameter des Cmdlets [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) mit dem Wert „2.0“, um eine Sitzungskonfiguration zu erstellen, die Windows PowerShell 2.0 Engine erfordert. Führen Sie diesen Befehl auf dem Computer auf der Serverseite oder dem empfangenden Ende der Verbindung aus.

    Der folgende Befehl erstellt die PS2-Sitzungskonfiguration auf dem Computer „Server01“. Starten Sie Windows PowerShell 4.0 oder Windows PowerShell 3.0 mit der Option **Als Administrator ausführen**, um diesen Befehl auszuführen.

    ```
    Register-PSSessionConfiguration -Name PS2 -PSVersion 2.0
    ```

2.  Zum Erstellen einer Sitzung auf dem Computer „Server01“, der die PS2-Sitzungskonfiguration nutzt, verwenden Sie den **ConfigurationName**-Parameter von Cmdlets, die eine Remotesitzung aufbauen, wie z. B. das Cmdlet [New-PSSession](https://technet.microsoft.com/en-us/library/76f6628c-054c-4eda-ba7a-a6f28daaa26f).

    Wenn eine Sitzung gestartet wird, die die Sitzungskonfiguration verwendet, wird Windows PowerShell 2.0 Engine automatisch in die Sitzung geladen.

    Der folgende Befehl erstellt eine Sitzung auf dem Computer „Server01“, die die PS2-Sitzungskonfiguration verwendet. Der Befehl speichert die Sitzung in der $s-Variablen.

    ```
    $s = New-PSSession -ComputerName Server01 -ConfigurationName PS2
    ```

## So starten Sie einen Hintergrundauftrags mit Windows PowerShell 2.0 Engine
Verwenden Sie den **PSVersion**-Parameter des Cmdlets [Start-Job](https://technet.microsoft.com/en-us/library/2bc04935-0deb-4ec0-b856-d7290cca6442) zum Starten eines Hintergrundauftrags mit Windows PowerShell 2.0 Engine.

Der folgende Befehl startet einen Hintergrundauftrag mit Windows PowerShell 2.0 Engine

```
Start-Job {Get-Process} -PSVersion 2.0
```

Weitere Informationen zu Hintergrundaufträgen finden Sie unter [about_Jobs [v4]](https://technet.microsoft.com/en-us/library/7362512a-8a4e-4575-b2ea-a740e5c4f002).




<!--HONumber=Oct16_HO1-->


