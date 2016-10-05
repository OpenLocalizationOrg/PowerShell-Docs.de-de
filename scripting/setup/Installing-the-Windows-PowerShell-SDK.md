---
title: Installieren das Windows PowerShell SDK
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: c3636b45-61aa-4720-85f0-58312c4fc8f9
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 7af27dc9bd8e93d1df5258b0d8df8af12726f568

---

# Installieren das Windows PowerShell SDK

Das folgende Thema beschreibt, wie das PowerShell SDK in verschiedenen Versionen von Windows installiert wird.

## Installieren des Windows PowerShell 3.0 SDK für Windows 8 und Windows Server 2012

Windows PowerShell 3.0 wird automatisch mit Windows 8 und Windows Server 2012 installiert.
Darüber hinaus können Sie die Verweisassemblys für Windows PowerShell 3.0 als Teil des Windows 8 SDKs herunterladen und installieren.
Diese Assemblys ermöglichen Ihnen das Schreiben von Cmdlets, Anbietern und Host-Programmen für Windows PowerShell 3.0.
Wenn Sie das Windows SDK für Windows 8 installieren, werden die Windows PowerShell-Assemblys im Ordner „Verweisassembly“ unter „\Programme (x86)\Reference Assemblies\Microsoft\WindowsPowerShell\3.0“ automatisch installiert.
Weitere Informationen finden Sie auf der [Windows 8 SDK-Downloadwebsite](http://msdn.microsoft.com/windows/hardware/hh852363.aspx).
Codebeispiele für Windows PowerShell stehen ebenfalls im Dev Center zur Verfügung.
Weitere Informationen finden Sie auf der Seite mit Desktop-Codebeispielen auf der [Dev Center-Website](http://code.msdn.microsoft.com/windowsdesktop/).

Darüber hinaus ist Windows PowerShell 3.0 abwärtskompatibel mit dem Windows PowerShell 2.0 SDK, das eine Reihe von Codebeispielen enthält.
Weitere Informationen zum Herunterladen des Windows PowerShell 2.0 SDKs finden Sie nachstehend.
(Beachten Sie, dass, während die 2.0-Codebeispiele mit Windows 8 und Windows PowerShell 3.0 kompatibel sind, Sie Windows PowerShell 2.0 auf einer Windows 8-Plattform nicht installieren können.)

##Installieren des Windows PowerShell 3.0 SDK für Windows 7 und Windows Server 2008 R2

Bei Windows 7 und Windows Server 2008 R2 wird PowerShell 2.0 automatisch installiert.
Darüber hinaus können Sie PowerShell 3.0 auf diesen Systemen installieren.
(Weitere Informationen finden Sie unter [Installieren von Windows PowerShell](Installing-Windows-PowerShell.md).)
Wie oben beschrieben, können Sie das Windows 8 SDK auch unter Windows 7 und Windows Server 2008 R2 installieren.

## Installieren des Windows PowerShell 2.0 SDK für Windows 7, Vista, XP, Server 2003 und Server 2008

Das Windows PowerShell 2.0 SDK stellt die Verweisassemblys bereit, die zum Schreiben von Cmdlets, Anbietern und Hostinganwendungen erforderlich sind, und bietet C#-Beispielcode, der als Ausgangspunkt dienen kann, wenn Sie mit dem Schreiben von Code beginnen.

Informationen zur Installation dieses SDK finden Sie unter [Windows PowerShell 2.0 SDK](http://go.microsoft.com/fwlink/?LinkId=184611).

## Verweisassemblys

Verweisassemblys werden standardmäßig an folgendem Speicherort installiert: `c:\Program Files\Reference Assemblies\Microsoft\WindowsPowerShell\V1.0`.

> **Hinweis**: Code, der für die Windows PowerShell 2.0-Assemblys kompiliert wird, kann nicht in Windows PowerShell 1.0-Installationen geladen werden.
>Jedoch kann Code, der für die Windows PowerShell 1.0-Assemblys kompiliert wird, in Windows PowerShell 2.0-Installationen geladen werden.

## Beispiele

Codebeispiele werden standardmäßig an folgendem Speicherort installiert: `C:\Program Files\Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\`.

Die folgenden Abschnitte enthalten eine kurze Beschreibung der einzelnen Beispiele.

## Cmdlet-Beispiele
**GetProcessSample01**

Zeigt, wie Sie ein einfaches Cmdlet schreiben, das alle Prozesse auf dem lokalen Computer abruft.

**GetProcessSample02**

Zeigt, wie dem Cmdlet Parameter hinzugefügt werden.
Das Cmdlet akzeptiert einen oder mehrere Prozessnamen und gibt die entsprechenden Prozesse zurück.

**GetProcessSample03**

Zeigt, wie Sie Parameter hinzufügen, die Eingaben aus der Pipeline akzeptieren.

**GetProcessSample04**

Zeigt, wie Fehler ohne Abbruch behandelt werden.

**GetProcessSample05**

Zeigt, wie eine Liste der angegebenen Prozesse angezeigt wird.

**SelectObject**

Zeigt das Schreiben eines Filters, um nur bestimmte Objekte auszuwählen.

**SelectString**

Zeigt, wie Dateien nach angegebenen Mustern durchsucht werden.

**StopProcessSample01**

Zeigt das Implementieren eines *PassThru*-Parameters und das Anfordern von Benutzerfeedback durch Aufrufe der Methoden [ShouldProcess](https://technet.microsoft.com/library/system.management.automation.cmdlet.shouldprocess.aspx) und [ShouldContinue](https://technet.microsoft.com/library/system.management.automation.cmdlet.shouldcontinue.aspx).
Benutzer geben den Parameter *PassThru* an, wenn sie erzwingen möchten, dass das Cmdlet ein Objekt zurückgibt.

**StopProcessSample02**

Zeigt, wie ein bestimmter Prozess beendet wird.

**StopProcessSample03**

Zeigt, wie Aliase für Parameter deklariert und Platzhalter unterstützt werden.

**StopProcessSample04**

Zeigt die Deklaration von Parametersätzen, das Objekt, das vom Cmdlet als Eingabe akzeptiert wird, und die Angabe des zu verwendenden Standardparametersatzes.

## Remotingbeispiele

**RemoteRunspace01**

Zeigt, wie Sie einen Remoterunspace erstellen, der zum Herstellen einer Remoteverbindung verwendet wird.

**RemoteRunspacePool01**

Zeigt, wie Sie einen Remoterunspace-Pool erstellen und anhand dieses Pools mehrere Befehle gleichzeitig ausführen.

**Serialization01**

Zeigt, wie Sie eine vorhandene .NET-Klasse anzeigen und sicherstellen, dass Informationen von ausgewählten öffentlichen Eigenschaften dieser Klasse über die Serialisierung/Deserialisierung hinweg beibehalten werden.

**Serialization02**

Zeigt, wie Sie eine vorhandene .NET-Klasse anzeigen und sicherstellen, dass Informationen von Instanzen aus dieser Klasse über die Serialisierung/Deserialisierung hinweg beibehalten werden, wenn die Informationen nicht in öffentlichen Eigenschaften dieser Klasse verfügbar sind.

**Serialization03**

Zeigt, wie Sie eine vorhandene .NET-Klasse anzeigen und sicherstellen, dass Instanzen dieser Klasse und von abgeleiteten Klassen in aktive .NET-Objekte deserialisiert (aktiviert) werden.

## Ereignisbeispiele

**Event01**

Zeigt, wie Sie ein Cmdlet für die Ereignisregistrierung durch Ableiten von ObjectEventRegistrationBase erstellen.

**Event02**

Zeigt, wie Sie Benachrichtigungen von Windows PowerShell-Ereignissen empfangen, die auf Remotecomputern generiert werden.
Verwendet das über die [Runspace](https://technet.microsoft.com/library/system.management.automation.runspaces.runspace.aspx)-Klasse offen gelegte PSEventReceived-Ereignis.

## Hosten von Anwendungsbeispielen

**Runspace01**

Zeigt, wie Sie die [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Klasse zum synchronen Ausführen des Cmdlets [Get-Process](http://go.microsoft.com/fwlink/?LinkId=113324) verwenden.
Das Cmdlet [Get-Process](http://go.microsoft.com/fwlink/?LinkId=113324) gibt [Process](https://technet.microsoft.com/library/system.diagnostics.process.aspx)-Objekte für jeden Prozess zurück, der auf dem lokalen Computer ausgeführt wird.

**Runspace02**

Zeigt, wie Sie die [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Klasse zum synchronen Ausführen der Cmdlets [Get-Process](http://go.microsoft.com/fwlink/?LinkId=113324) und [Sort-Object](http://go.microsoft.com/fwlink/?LinkID=113403) verwenden.
Das Cmdlet [Get-Process](http://go.microsoft.com/fwlink/?LinkId=113324) gibt [Process](https://technet.microsoft.com/library/system.diagnostics.process.aspx)-Objekte für jeden Prozess zurück, der auf dem lokalen Computer ausgeführt wird, und das Cmdlet „Sort-Object“ sortiert die Objekte basierend auf ihrer [Id](https://technet.microsoft.com/library/system.diagnostics.process.id.aspx)-Eigenschaft.
Die Ergebnisse dieser Befehle werden anhand des Steuerelements [DataGridView](https://technet.microsoft.com/library/system.windows.forms.datagridview.aspx) angezeigt.

**Runspace03**

Zeigt, wie Sie die [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Klasse zum synchronen Ausführen eines Skripts verwenden und wie Sie Fehler ohne Abbruch behandeln.
Das Skript empfängt eine Liste von Prozessnamen und ruft diese Prozesse anschließend ab.
Die Ergebnisse des Skripts, einschließlich Fehler ohne Abbruch, die beim Ausführen des Skripts generiert wurden, werden in einem Konsolenfenster angezeigt.

**Runspace04**

Zeigt, wie die [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Klasse zum Ausführen von Befehlen verwendet wird und wie Fehler mit Abbruch abgefangen werden, die beim Ausführen der Befehle ausgelöst werden.
Zwei Befehle werden ausgeführt. An den letzten Befehl wird ein ungültiges Parameterargument übergeben.
Daher werden keine Objekte zurückgegeben, und ein Fehler mit Abbruch wird ausgelöst.

**Runspace05**

Zeigt, wie Sie einem [InitialSessionState](https://technet.microsoft.com/library/system.management.automation.runspaces.initialsessionstate.aspx)-Objekt ein Snap-In hinzufügen, sodass das Cmdlet des Snap-Ins bei Öffnen des Runspace verfügbar ist.
Das Snap-In stellt ein Get-Proc-Cmdlet bereit (definiert durch das [GetProcessSample01-Beispiel](https://technet.microsoft.com/library/ff602028.aspx)), das synchron mithilfe eines [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Objekts ausgeführt wird.

**Runspace06**

Zeigt, wie Sie einem [InitialSessionState](https://technet.microsoft.com/library/system.management.automation.runspaces.initialsessionstate.aspx)-Objekt ein Modul hinzufügen, sodass das Modul beim Öffnen des Runspace geladen wird.
Das Modul stellt ein Get-Proc-Cmdlet bereit (definiert durch das [GetProcessSample02-Beispiel](https://technet.microsoft.com/library/ff602027.aspx)), das synchron mithilfe eines [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Objekts ausgeführt wird.

**Runspace07**

Zeigt, wie Sie einen Runspace erstellen und dann diesen Runspace zum synchronen Ausführen von zwei Cmdlets anhand eines [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Objekts verwenden.

**Runspace08**

Zeigt, wie Sie der Pipeline eines [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Objekts Befehle und Argumente hinzufügen und die Befehle synchron ausführen.

**Runspace09**

Zeigt, wie Sie der Pipeline eines [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Objekts ein Skript hinzufügen und das Skript asynchron ausführen.
Ereignisse werden verwendet, um die Ausgabe des Skripts zu verarbeiten.

**Runspace10**

Zeigt, wie Sie einen anfänglichen Standardsitzungsstatus erstellen, dem [InitialSessionState](https://technet.microsoft.com/library/system.management.automation.runspaces.initialsessionstate.aspx) ein Cmdlet hinzufügen, einen Runspace erstellen, der den anfänglichen Sitzungsstatus verwendet, und wie Sie den Befehl anhand eines [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-Objekts ausführen.

**Runspace11**

Zeigt, wie Sie mithilfe der [ProxyCommand](https://technet.microsoft.com/library/system.management.automation.proxycommand.aspx)-Klasse einen Proxybefehl erstellen, der ein vorhandenes Cmdlet aufruft, aber den Satz der verfügbaren Parameter einschränkt.
Der Proxybefehl wird anschließend zu einem anfänglichen Sitzungsstatus hinzugefügt, der dazu verwendet wird, einen eingeschränkten Runspace zu erstellen.
Dies bedeutet, dass der Benutzer nur über den Proxybefehl auf die Funktionalität des Cmdlets zugreifen kann.

**PowerShell01**

Zeigt, wie Sie über ein [InitialSessionState](https://technet.microsoft.com/library/system.management.automation.runspaces.initialsessionstate.aspx)-Objekt einen eingeschränkten Runspace erstellen.

**PowerShell02**

Zeigt, wie Sie einen Runspacepool verwenden, um mehrere Befehle gleichzeitig auszuführen.

## Hostbeispiele

**Host01**

Zeigt, wie eine Hostanwendung implementiert wird, die einen benutzerdefinierten Host verwendet.
In diesem Beispiel wird ein Runspace erstellt, der den benutzerdefinierten Host verwendet. Anschließend wird die [PowerShell](https://technet.microsoft.com/library/system.management.automation.powershell.aspx)-API zum Ausführen eines Skripts verwendet, das „exit“ aufruft.
Die Hostanwendung untersucht anschließend die Ausgabe des Skripts und gibt die Ergebnisse aus.

**Host02**

Zeigt, wie Sie eine Hostanwendung schreiben, die die Windows PowerShell-Laufzeit zusammen mit einer benutzerdefinierten Hostimplementierung verwendet.
Die Hostanwendung legt die Hostkultur auf Deutsch fest, führt das Cmdlet [Get-Process](http://go.microsoft.com/fwlink/?LinkId=113324) aus und zeigt die Ergebnisse so an, wie sie über „pwrsh.exe“ dargestellt würden, und gibt dann das aktuelle Datum und die Uhrzeit auf Deutsch aus.

**Host03**

Zeigt, wie eine interaktive, konsolenbasierte Hostanwendung erstellt wird, die Befehle von der Befehlszeile aus liest, die Befehle ausführt und die Ergebnisse anschließend in der Konsole anzeigt.

**Host04**

Zeigt, wie eine interaktive, konsolenbasierte Hostanwendung erstellt wird, die Befehle von der Befehlszeile aus liest, die Befehle ausführt und die Ergebnisse anschließend in der Konsole anzeigt.
Diese Hostanwendung unterstützt auch das Anzeigen von Eingabeaufforderungen, die es den Benutzern ermöglichen, mehrere Auswahlmöglichkeiten anzugeben.

**Host05**

Zeigt, wie eine interaktive, konsolenbasierte Hostanwendung erstellt wird, die Befehle von der Befehlszeile aus liest, die Befehle ausführt und die Ergebnisse anschließend in der Konsole anzeigt.
Diese Anwendung unterstützt auch Aufrufe von Remotecomputern mithilfe der Cmdlets [Enter-PsSession](http://go.microsoft.com/fwlink/?LinkId=135210) und [Exit-PsSession](http://go.microsoft.com/fwlink/?LinkId=135212).

**Host06**

Zeigt, wie eine interaktive, konsolenbasierte Hostanwendung erstellt wird, die Befehle von der Befehlszeile aus liest, die Befehle ausführt und die Ergebnisse anschließend in der Konsole anzeigt.
Darüber hinaus verwendet dieses Beispiel die Tokenizer-APIs, um die Farbe des vom Benutzer eingegebenen Texts anzugeben.

## Anbieterbeispiele

**AccessDBProviderSample01**

Zeigt, wie Sie eine Anbieterklasse deklarieren, die direkt von der [CmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.cmdletprovider.aspx)-Klasse abgeleitet wird.
Wird hier nur aus Gründen der Vollständigkeit aufgeführt.

**AccessDBProviderSample02**

Zeigt, wie Sie die Methoden [NewDrive](https://technet.microsoft.com/library/system.management.automation.provider.drivecmdletprovider.newdrive.aspx) und [RemoveDrive](https://technet.microsoft.com/library/system.management.automation.provider.drivecmdletprovider.removedrive.aspx) überschreiben, um Aufrufe an die Cmdlets „New-PSDrive“ und „Remove-PSDrive“ zu unterstützen.
Die Anbieterklasse in diesem Beispiel wird von der [DriveCmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.drivecmdletprovider.aspx)-Klasse abgeleitet.

**AccessDBProviderSample03**

Zeigt, wie Sie die Methoden [GetItem](https://technet.microsoft.com/library/system.management.automation.provider.itemcmdletprovider.getitem.aspx) und [SetItem](https://technet.microsoft.com/library/system.management.automation.provider.itemcmdletprovider.setitem.aspx) überschreiben, um Aufrufe an die Cmdlets „Get-Item“ und „Set-Item“ zu unterstützen.
Die Anbieterklasse in diesem Beispiel wird von der [ItemCmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.itemcmdletprovider.aspx)-Klasse abgeleitet.

**AccessDBProviderSample04**

Zeigt, wie Sie Containermethoden überschreiben, um Aufrufe der Cmdlets „Copy-Item“, „Get-ChildItem“, „New-Item“ und „Remove-Item“ zu unterstützen.
Diese Methoden sollten implementiert werden, wenn der Datenspeicher Elemente enthält, die Container sind.
Ein Container ist eine Gruppe von untergeordneten Elementen unter einem gemeinsamen übergeordneten Element.
Die Anbieterklasse in diesem Beispiel wird von der [ItemCmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.itemcmdletprovider.aspx)-Klasse abgeleitet.

**AccessDBProviderSample05**

Zeigt, wie Sie Containermethoden überschreiben, um Aufrufe der Cmdlets „Move-Item“ und „Join-Path“ zu unterstützen.
Diese Methoden sollten implementiert werden, wenn der Benutzer Elemente innerhalb eines Containers verschieben muss, und wenn der Datenspeicher geschachtelte Container enthält.
Die Anbieterklasse in diesem Beispiel wird von der [NavigationCmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.navigationcmdletprovider.aspx)-Klasse abgeleitet.

**AccessDBProviderSample06**

Zeigt, wie Sie Inhaltsmethoden überschreiben, um Aufrufe der Cmdlets „Clear-Content“, „Get-Content“ und „Set-Content“ zu unterstützen.
Diese Methoden sollten implementiert werden, wenn der Benutzer den Inhalt der Elemente im Datenspeicher verwaltet muss.
Die Anbieterklasse in diesem Beispiel wird von der [NavigationCmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.navigationcmdletprovider.aspx)-Klasse abgeleitet und implementiert die [IContentCmdletProvider](https://technet.microsoft.com/library/system.management.automation.provider.icontentcmdletprovider.aspx)-Schnittstelle.



<!--HONumber=Oct16_HO1-->


