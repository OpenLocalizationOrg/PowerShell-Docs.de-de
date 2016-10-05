---
title: Arbeiten mit Softwareinstallationen
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 51a12fe9-95f6-4ffc-81a5-4fa72a5bada9
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 13fdac5369d70289d7a0b5115a04879f707e47fc

---

# Arbeiten mit Softwareinstallationen
Auf Anwendungen, die für die Verwendung von Windows Installer entwickelt wurden, kann über die WMI-Klasse **Win32_Product** zugegriffen werden, aber nicht alle heute verfügbaren Anwendungen verwenden den Windows Installer. Da Windows Installer eine breite Palette von Standardverfahren für die Arbeit mit installierbaren Anwendungen bereitstellt, konzentrieren wir uns in erster Linie auf diese Anwendungen. Anwendungen, die alternative Setuproutinen verwenden, werden im Allgemeinen nicht von Windows Installer verwaltet. Spezifische Verfahren für die Arbeit mit diesen Anwendungen hängen von der Installationssoftware und Entscheidungen des Anwendungsentwicklers ab.

> [!NOTE]
> Anwendungen, die durch Kopieren der Anwendungsdateien auf den Computer installiert werden, können in der Regel nicht mit den hier beschriebenen Verfahren verwaltet werden. Sie können diese Anwendungen als Dateien und Ordner mit den Verfahren verwalten, die im Abschnitt „Arbeiten mit Dateien und Ordnern“ erläutert werden.

### Auflisten von Windows Installer-Anwendungen
Verwenden Sie die folgende einfache WMI-Abfrage zum Auflisten von Anwendungen, die mithilfe von Windows Installer auf einem lokalen System oder Remotesystem installiert wurden:

```
PS> Get-WmiObject -Class Win32_Product -ComputerName .
IdentifyingNumber : {7131646D-CD3C-40F4-97B9-CD9E4E6262EF}
Name              : Microsoft .NET Framework 2.0
Vendor            : Microsoft Corporation
Version           : 2.0.50727
Caption           : Microsoft .NET Framework 2.0
```

Um alle Eigenschaften des Objekts Win32_Product auf dem Bildschirm anzuzeigen, verwenden Sie den Eigenschaften-Parameter der formatierungs-Cmdlets, z. B. das Format-List-Cmdlet mit einem Wert von \ * (alle).

```
PS> Get-WmiObject -Class Win32_Product -ComputerName . | Where-Object -FilterScript {$_.Name -eq "Microsoft .NET Framework 2.0"} | Format-List -Property *
Name              : Microsoft .NET Framework 2.0
Version           : 2.0.50727
InstallState      : 5
Caption           : Microsoft .NET Framework 2.0
Description       : Microsoft .NET Framework 2.0
IdentifyingNumber : {7131646D-CD3C-40F4-97B9-CD9E4E6262EF}
InstallDate       : 20060506
InstallDate2      : 20060506000000.000000-000
InstallLocation   :
PackageCache      : C:\WINDOWS\Installer\619ab2.msi
SKUNumber         :
Vendor            : Microsoft Corporation
```

Oder verwenden Sie den Parameter **Get-WmiObject Filter**, um nur Microsoft .NET Framework 2.0 auszuwählen. Da es sich bei dem in diesem Befehl verwendeten Filter um einen WMI-Filter handelt, verwendet er die Syntax der WMI Query Language (WQL) und nicht die Windows PowerShell-Syntax :

```
Get-WmiObject -Class Win32_Product -ComputerName . -Filter "Name='Microsoft .NET Framework 2.0'"| Format-List -Property *
```

Beachten Sie, dass WQL-Abfragen häufig Zeichen wie Leerzeichen oder Gleichheitszeichen verwenden, die in Windows PowerShell eine besondere Bedeutung haben. Aus diesem Grund ist es ratsam, den Wert des Filterparameters immer in Anführungszeichen anzugeben. Sie können auch das Windows PowerShell-Escapezeichen verwenden, ein Hochkomma ('), obwohl die Lesbarkeit damit möglicherweise nicht verbessert wird. Der folgende Befehl entspricht dem vorherigen Befehl und es werden dieselben Ergebnisse zurückgegeben, aber es wird das Hochkomma als Escapezeichen für Sonderzeichen verwendet, statt die gesamte Filterzeichenfolge wiederzugeben

```
Get-WmiObject -Class Win32_Product -ComputerName . -Filter Name`=`'Microsoft` .NET` Framework` 2.0`' | Format-List -Property *
```

Um nur die Eigenschaften von Interesse aufzulisten, verwenden Sie den Eigenschaftenparameter der Formatierungs-Cmdlets zur Auflistung der gewünschten Eigenschaften.

```
Get-WmiObject -Class Win32_Product -ComputerName . | Format-List -Property Name,InstallDate,InstallLocation,PackageCache,Vendor,Version,IdentifyingNumber
...
Name              : HighMAT Extension to Microsoft Windows XP CD Writing Wizard
InstallDate       : 20051022
InstallLocation   : C:\Program Files\HighMAT CD Writing Wizard\
PackageCache      : C:\WINDOWS\Installer\113b54.msi
Vendor            : Microsoft Corporation
Version           : 1.1.1905.1
IdentifyingNumber : {FCE65C4E-B0E8-4FBD-AD16-EDCBE6CD591F}
...
```

Um schließlich nur die Namen der installierten Anwendungen anzuzeigen, vereinfacht die **Format-Wide**-Anweisung die Ausgabe:

```
Get-WmiObject -Class Win32_Product -ComputerName .  | Format-Wide -Column 1
```

Obwohl es nun mehrere Möglichkeiten zum Anzeigen der Anwendungen gibt, die mit Windows Installer installiert wurden, wurden keine anderen Anwendungen berücksichtigt. Da die meisten Standardanwendungen ihr Deinstallationsprogramm bei Windows registrieren, können Sie mit diesen lokal arbeiten, indem Sie sie in der Windows-Registrierung suchen.

### Auflisten aller deinstallierbaren Anwendungen
Es gibt zwar keine sichere Methode, alle Anwendungen auf einem System zu finden, aber es ist möglich, alle Programme mit Auflistungen zu finden, die im Dialogfeld „Software“ angezeigt werden. „Software“ sucht diese Anwendungen im folgenden Registrierungsschlüssel:

**HKEY_LOCAL_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Uninstall**.

Dieser Schlüssel kann auch untersucht werden, um Anwendungen zu finden. Um die Anzeige des Deinstallationsschlüssel zu vereinfachen, kann diesem Registrierungsspeicherort ein Windows PowerShell-Laufwerk zugeordnet werden:

```
PS>    

Name       Provider      Root                                   CurrentLocation
----       --------      ----                                   ---------------
Uninstall  Registry      HKEY_LOCAL_MACHINE\SOFTWARE\Micr...
```

> [!NOTE]
> Das Laufwerk **HKLM:** ist dem Stammverzeichnis von **HKEY_LOCAL_MACHINE** zugeordnet, daher haben wir dieses Laufwerk im Pfad zum Deinstallationsschlüssel verwendet. Statt **HKLM:** hätten wir den Registrierungspfad entweder mit **HKLM** oder **HKEY_LOCAL_MACHINE** festlegen können. Der Vorteil der Verwendung eines vorhandenen Registrierungslaufwerks ist, dass wir die Befehlszeilenergänzung verwenden können, um die Namen der Schlüssel zu vervollständigen, ohne sie ganz eingeben zu müssen.

Wir haben nun ein Laufwerk mit dem Namen „Deinstallieren“, das für die schnelle und bequeme Suche nach Anwendungsinstallationen verwendet werden kann. Wir können die Anzahl der installierten Anwendungen durch Zählen der Anzahl der Registrierungsschlüssel im Laufwerk „Deinstallieren: Windows PowerShell“ suchen:

```
PS> (Get-ChildItem -Path Uninstall:).Count
459
```

Diese Liste mit Anwendungen kann nun anhand einer Vielzahl von Verfahren weiter durchsucht werden, beginnend mit **Get-ChildItem**. Um eine Liste der Anwendungen abzurufen und in der Variablen **$UninstallableApplications** zu speichern, verwenden Sie folgenden Befehl:

```
$UninstallableApplications = Get-ChildItem -Path Uninstall:
```

> [!NOTE]
> Wir verwenden an dieser Stelle einen langen Variablennamen aus Gründen der Übersichtlichkeit. Tatsächlich ist es nicht notwendig, lange Namen zu verwenden. Zwar können Sie die Befehlszeilenergänzung für Variablennamen verwenden, Sie können aber zur schnelleren Bearbeitung auch Namen mit 1 bis 2 Zeichen verwenden. Längere und aussagekräftige Namen sind besonders hilfreich, wenn Sie Code zur Wiederverwendung entwickeln.

Um die Werte der Registrierungseinträge in den Registrierungsschlüsseln unter „Deinstallieren“ anzuzeigen, verwenden Sie die Methode „GetValue“ der Registrierungsschlüssel. Der Wert der Methode ist der Name des Registrierungseintrags.

Um die Anzeigenamen der Anwendungen beispielsweise im Deinstallationsschlüssel zu suchen, verwenden Sie den folgenden Befehl:

```
PS> Get-ChildItem -Path Uninstall: | ForEach-Object -Process { $_.GetValue("DisplayName") }
```

Es gibt keine Garantie dafür, dass diese Werte eindeutig sind. Im folgenden Beispiel werden zwei installierte Elemente als „Windows Media Encoder 9-Reihe“ angezeigt:

```
PS> Get-ChildItem -Path Uninstall: | Where-Object -FilterScript { $_.GetValue("DisplayName") -eq "Windows Media Encoder 9 Series"}

   Hive: Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Micros
oft\Windows\CurrentVersion\Uninstall

SKC  VC Name                           Property
---  -- ----                           --------
  0   3 Windows Media Encoder 9        {DisplayName, DisplayIcon, UninstallS...
  0  24 {E38C00D0-A68B-4318-A8A6-F7... {AuthorizedCDFPrefix, Comments, Conta...
```

### Installieren von Anwendungen
Sie können die Klasse **Win32_Product** verwenden, um Windows Installer-Pakete remote oder lokal zu installieren.

> [!NOTE]
> Unter Windows Vista, Windows Server 2008 und höheren Versionen von Windows müssen Sie Windows PowerShell zum Installieren einer Anwendung mit der Option „Als Administrator ausführen“ starten.

Verwenden Sie bei der Remoteinstallation einen UNC-Netzwerkpfad (Universal Naming Convention), um den Pfad zum MSI-Paket anzugeben, da das WMI-Subsystem Windows PowerShell-Pfade nicht interpretieren kann. Geben Sie beispielsweise zum Installieren des Pakets „NewPackage.msi“ in der Netzwerkfreigabe „\\AppServ\dsp“ auf dem Remotecomputer PC01 den folgenden Befehl in der Windows PowerShell-Eingabeaufforderung ein:

```
(Get-WMIObject -ComputerName PC01 -List | Where-Object -FilterScript {$_.Name -eq "Win32_Product"}).Install(\\AppSrv\dsp\NewPackage.msi)
```

Anwendungen, die keine Windows Installer-Technologie verwenden, verfügen möglicherweise über anwendungsspezifische Methoden für die automatisierte Bereitstellung. Um zu bestimmen, ob es eine Methode für die automatische Bereitstellung gibt, überprüfen Sie die Dokumentation für die Anwendung, oder wenden Sie sich an den Support des Anwendungsherstellers. In einigen Fällen verfügt die Installationssoftware möglicherweise über Verfahren zur Automatisierung, auch wenn der Hersteller der Anwendung diese nicht speziell für die Automatisierung der Installation vorgesehen hat.

### Entfernen von Anwendungen
Das Entfernen eines Windows Installer-Pakets mithilfe von Windows PowerShell funktioniert ungefähr so wie die Installation eines Pakets. Im Folgenden ist ein Beispiel aufgeführt, in dem das zu deinstallierende Paket basierend auf seinem Namen ausgewählt wird. In einigen Fällen ist es möglicherweise einfacher, mit **IdentifyingNumber** zu filtern:

```
(Get-WmiObject -Class Win32_Product -Filter "Name='ILMerge'" -ComputerName . ).Uninstall()
```

Das Entfernen von anderen Anwendungen ist nicht ganz so einfach, auch wenn es lokal durchgeführt wird. Nach den Befehlszeilen-Deinstallationszeichenfolgen für diese Anwendungen wird durch Extrahieren der Eigenschaft **UninstallString** gesucht. Diese Methode funktioniert für Windows Installer-Anwendungen und ältere Programme, die unter dem Deinstallationsschlüssel angezeigt werden:

```
Get-ChildItem -Path Uninstall: | ForEach-Object -Process { $_.GetValue("UninstallString") }
```

Sie können die Ausgabe bei Bedarf nach dem Anzeigenamen filtern:

```
Get-ChildItem -Path Uninstall: | Where-Object -FilterScript { $_.GetValue("DisplayName") -like "Win*"} | ForEach-Object -Process { $_.GetValue("UninstallString") }
```

Diese Zeichenfolgen sind jedoch möglicherweise direkt über die Windows PowerShell-Eingabeaufforderung nur mit einigen Änderungen verwendbar.

### Aktualisieren von Windows Installer-Anwendungen
Um eine Anwendung zu aktualisieren, müssen Sie den Namen der Anwendung und den Pfad zum Aktualisierungspaket der Anwendung kennen. Mit diesen Informationen können Sie eine Anwendung mit einem einzigen Windows PowerShell-Befehl aktualisieren:

```
(Get-WmiObject -Class Win32_Product -ComputerName . -Filter "Name='OldAppName'").Upgrade(\\AppSrv\dsp\OldAppUpgrade.msi)
```




<!--HONumber=Oct16_HO1-->


