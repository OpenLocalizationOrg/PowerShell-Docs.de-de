---
title: Erstellen von .NET- und COM-Objekten  New-Object
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 2057b113-efeb-465e-8b44-da2f20dbf603
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 4c0f405a46e16935211b3886a40c9f7d1afc7260

---

# Erstellen von .NET- und COM-Objekten (New-Object)
Es gibt Softwarekomponenten mit .NET Framework- und COM-Schnittstellen, mit denen Sie viele Systemverwaltungsaufgaben ausführen können. Mit Windows PowerShell können Sie diese Komponenten verwenden und sind somit nicht auf die Aufgaben beschränkt, die mithilfe von Cmdlets ausgeführt werden können. Viele der Cmdlets in der ursprünglichen Version von Windows PowerShell funktionieren nicht auf Remotecomputern. Wir zeigen, wie diese Einschränkung beim Verwalten von Ereignisprotokollen mithilfe der .NET Framework-Klasse **System.Diagnostics.EventLog** direkt aus Windows PowerShell heraus umgangen werden können.

### Verwenden von „New-Object“ für den Zugriff auf das Ereignisprotokoll
Die .NET Framework-Klassenbibliothek enthält eine Klasse namens **System.Diagnostics.EventLog**, die zum Verwalten von Ereignisprotokollen verwendet werden kann. Zum Erstellen einer neuen Instanz einer .NET Framework-Klasse können Sie das Cmdlet **New-Object** mit dem Parameter **TypeName** verwenden. Der folgende Befehl erstellt z. B. einen Ereignisprotokollverweis:

```
PS> New-Object -TypeName System.Diagnostics.EventLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
```

Obwohl der Befehl eine Instanz der EventLog-Klasse erstellt hat, enthält die Instanz keine Daten. Der Grund hierfür ist, dass wir kein bestimmtes Ereignisprotokoll angegeben haben. Wie erhalten Sie ein echtes Ereignisprotokoll?

#### Verwenden von Konstruktoren mit „New-Object“
Um auf ein bestimmtes Ereignisprotokoll zu verweisen, müssen Sie den Namen des Protokolls angeben. **New-Object** verfügt über einen **ArgumentList**-Parameter. Die Argumente, die Sie als Werte an diesen Parameter übergeben, werden von einer speziellen Startmethode des Objekts verwendet. Die Methode wird als *Konstruktor* bezeichnet, da sie zum Konstruieren des Objekts dient. Um beispielsweise einen Verweis auf das Anwendungsprotokoll zu erhalten, geben Sie die Zeichenfolge „Application“ als Argument ein:

```
PS> New-Object -TypeName System.Diagnostics.EventLog -ArgumentList Application

Max(K) Retain OverflowAction        Entries Name
------ ------ --------------        ------- ----
16,384      7 OverwriteOlder          2,160 Application
```

> [!NOTE]
> Da die meisten .NET Framework Core-Klassen im System-Namespace enthalten sind, versucht Windows PowerShell automatisch, Klassen zu finden, die Sie im System-Namespace angeben, wenn keine Übereinstimmung für den angegebenen Typnamen gefunden wird. Dies bedeutet, dass Sie „Diagnostics.EventLog“ anstelle von „System.Diagnostics.EventLog“ angeben können.

#### Speichern von Objekten in Variablen
Möglicherweise möchten Sie einen Verweis auf ein Objekt speichern, damit Sie es in der aktuellen Shell verwenden können. Obwohl Windows PowerShell Ihnen ermöglicht, viele Aufgaben mit Pipelines zu erledigen und damit die Notwendigkeit von Variablen zu verringern, erleichtert das Speichern von Verweisen auf Objekte in Variablen manchmal die Bearbeitung dieser Objekte.

Mit Windows PowerShell können Sie Variablen erstellen, die im Prinzip benannte Objekte sind. Die Ausgabe eines beliebigen gültigen Windows PowerShell-Befehls kann in einer Variablen gespeichert werden. Namen von Variablen beginnen immer mit $. Wenn Sie den Verweis auf ein Anwendungsprotokoll in einer Variablen namens „$AppLog“ speichern möchten, geben Sie den Namen der Variablen an, gefolgt von einem Gleichheitszeichen und dem Befehl zum Erstellen des Anwendungsprotokollobjekts:

```
PS> $AppLog = New-Object -TypeName System.Diagnostics.EventLog -ArgumentList Application
```

Wenn Sie dann „$AppLog“ eingeben, sehen Sie, dass es das Anwendungsprotokoll enthält:

```
PS> $AppLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
  16,384      7 OverwriteOlder          2,160 Application
```

#### Zugreifen auf ein Remoteereignisprotokoll mit „New-Object“
Die im vorherigen Abschnitt verwendeten Befehle hatten den lokalen Computer als Ziel. Dazu kann auch das Cmdlet **Get-EventLog** verwendet werden. Um auf das Anwendungsprotokoll auf einem Remotecomputer zuzugreifen, müssen Sie sowohl den Namen des Protokolls als auch einen Computernamen (oder eine IP-Adresse) als Argumente bereitstellen.

```
PS> $RemoteAppLog = New-Object -TypeName System.Diagnostics.EventLog Application,192.168.1.81
PS> $RemoteAppLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
     512      7 OverwriteOlder            262 Application
```

Welche Aufgaben können wir nun mit dem Verweis auf ein in der Variablen $RemoteAppLog gespeichertes Ereignisprotokoll ausführen?

#### Löschen eines Ereignisprotokolls mit Objektmethoden
Objekte verfügen oft über Methoden, die aufgerufen werden können, um Aufgaben auszuführen. Sie können **Get-Member** verwenden, um die einem Objekt zugeordneten Methoden anzuzeigen. Mit dem folgenden Befehl und der ausgewählten Ausgabe werden einige der Methoden der EventLog-Klasse angezeigt:

```
PS> $RemoteAppLog | Get-Member -MemberType Method

   TypeName: System.Diagnostics.EventLog

Name                      MemberType Definition
----                      ---------- ----------
...
Clear                     Method     System.Void Clear()
Close                     Method     System.Void Close()
...
GetType                   Method     System.Type GetType()
...
ModifyOverflowPolicy      Method     System.Void ModifyOverflowPolicy(Overfl...
RegisterDisplayName       Method     System.Void RegisterDisplayName(String ...
...
ToString                  Method     System.String ToString()
WriteEntry                Method     System.Void WriteEntry(String message),...
WriteEvent                Method     System.Void WriteEvent(EventInstance in...
```

Die Methode **Clear()** kann zum Löschen des Ereignisprotokolls verwendet werden. Beim Aufrufen einer Methode müssen dem Methodennamen immer Klammern folgen, auch wenn die Methode keine Argumente erfordert. So kann Windows PowerShell zwischen der Methode und einer möglichen Eigenschaft mit gleichem Namen unterscheiden. Geben Sie Folgendes ein, um die Methode **Clear** aufzurufen:

```
PS> $RemoteAppLog.Clear()
```

Geben Sie Folgendes ein, um das Protokoll anzuzeigen. Sie können erkennen, dass das Ereignisprotokoll gelöscht wurde und jetzt statt 262 Einträgen 0 Einträge enthält:

```
PS> $RemoteAppLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
     512      7 OverwriteOlder              0 Application
```

### Erstellen von COM-Objekten mit „New-Object“
Sie können **New-Object** verwenden, um mit Component Object Model-Komponenten (COM) zu arbeiten. Die Komponenten reichen von den verschiedenen in Windows Script Host (WSH) enthaltenen Bibliotheken bis hin zu ActiveX-Anwendungen wie Internet Explorer, die auf den meisten Systemen installiert sind.

**New-Object** verwendet zum Erstellen von COM-Objekten .NET Framework Runtime Callable Wrapper, es gelten somit die gleichen Einschränkungen wie für .NET Framework beim Aufrufen von COM-Objekten. Zum Erstellen eines COM-Objekts müssen Sie den Parameter **ComObject** mit dem programmatischen Bezeichner bzw. der *ProgId* der COM-Klasse angeben, die Sie verwenden möchten. Eine vollständige Erläuterung der Einschränkungen der COM-Verwendung und Informationen zu den auf einem System verfügbaren ProgIDs würden den Rahmen dieses Handbuchs sprengen, aber die meisten bekannten Objekte aus Umgebungen wie WSH können in Windows PowerShell verwendet werden.

Sie können die WSH-Objekte erstellen, indem Sie diese ProgIDs angeben: **WScript.Shell**, **WScript.Network**, **Scripting.Dictionary** und **Scripting.FileSystemObject**. Mit den folgenden Befehlen werden diese Objekte erstellt:

```
New-Object -ComObject WScript.Shell
New-Object -ComObject WScript.Network
New-Object -ComObject Scripting.Dictionary
New-Object -ComObject Scripting.FileSystemObject
```

Die meisten Funktionen dieser Klassen werden zwar auch auf andere Weise in Windows PowerShell bereitgestellt, aber einige Aufgaben wie das Erstellen einer Verknüpfung lassen sich mit den WSH-Klassen einfacher erledigen.

### Erstellen einer Desktopverknüpfung mit „WScript.Shell“
Eine Aufgabe, die mit einem COM-Objekt schnell und einfach ausgeführt werden kann, ist das Erstellen einer Verknüpfung. Angenommen, Sie möchten auf dem Desktop eine Verknüpfung zum Basisordner für Windows PowerShell erstellen. Zuerst müssen Sie einen Verweis auf **WScript.Shell** erstellen, den wir in einer Variablen namens **$WshShell** speichern:

```
$WshShell = New-Object -ComObject WScript.Shell
```

„Get-Member“ funktioniert mit COM-Objekten, sodass Sie die Elemente des Objekts untersuchen können, indem Sie folgendes eingeben:

```
PS> $WshShell | Get-Member

   TypeName: System.__ComObject#{41904400-be18-11d3-a28b-00104bd35090}

Name                     MemberType            Definition
----                     ----------            ----------
AppActivate              Method                bool AppActivate (Variant, Va...
CreateShortcut           Method                IDispatch CreateShortcut (str...
...
```

**Get-Member** verfügt über einen optionalen **InputObject**-Parameter, den Sie anstelle von Piping als Eingabe für **Get-Member** verwenden können. Dieselbe Ausgabe wie oben hätten Sie auch über den Befehl **Get-Member -InputObject $WshShell** erhalten. Wenn Sie **InputObject** verwenden, wird das Argument als ein einzelnes Element behandelt. Dies bedeutet, dass bei mehreren Objekte in einer Variablen diese von **Get-Member** als ein Array von Objekten behandelt werden. Beispiel:

```
PS> $a = 1,2,"three"
PS> Get-Member -InputObject $a
TypeName: System.Object[]
Name               MemberType    Definition
----               ----------    ----------
Count              AliasProperty Count = Length
...
```

Die Methode **WScript.Shell CreateShortcut** akzeptiert ein einziges Argument, und zwar, den Pfad zur zu erstellenden Verknüpfungsdatei. Wir könnten den vollständigen Pfad zum Desktop eingeben, aber es gibt einen einfacheren Weg. Der Desktop wird normalerweise durch einen Ordner namens „Desktop“ im Basisordner des aktuellen Benutzers dargestellt. Windows PowerShell verfügt über eine Variable **$Home**, die den Pfad zu diesem Ordner enthält. Sie können den Pfad zum Basisordner mithilfe dieser Variablen angeben und dann den Namen des Ordners „Desktop“ sowie den Namen für die zu erstellende Verknüpfung hinzufügen. Geben Sie dazu Folgendes ein:

```
$lnk = $WshShell.CreateShortcut("$Home\Desktop\PSHome.lnk")
```

Wenn Sie ein Element, das wie ein Variablenname aussieht, in doppelten Anführungszeichen angeben, versucht Windows PowerShell, dieses Element durch einen entsprechenden Wert zu ersetzen. Wenn Sie einzelne Anführungszeichen verwenden, versucht Windows PowerShell nicht, den Variablenwert zu ersetzen. Geben Sie beispielsweise die folgenden Befehle ein:

```
PS> "$Home\Desktop\PSHome.lnk"
C:\Documents and Settings\aka\Desktop\PSHome.lnk
PS> '$Home\Desktop\PSHome.lnk'
$Home\Desktop\PSHome.lnk
```

Wir verfügen jetzt über eine Variable namens **$lnk**, die einen neuen Verknüpfungsverweis enthält. Wenn Sie die zugehörigen Elemente anzeigen möchten, können Sie sie über die Pipeline an **Get-Member** übergeben. In der folgenden Ausgabe werden die zum Abschließen der Erstellung unserer Verknüpfung zu verwendenden Elemente angezeigt:

<pre>PS> $lnk | Get-Member TypeName: System.__ComObject#{f935dc23-1cf0-11d0-adb9-00c04fd58a0b} Name             MemberType   Definition ----             ----------   ---------- ... Save             Method       void Save () ... TargetPath       Property     string TargetPath () {get} {set} ...</pre>

Sie müssen den **TargetPath** angeben, d.h. den Anwendungsordner für Windows PowerShell, und dann die Verknüpfung **$lnk** durch Aufrufen der Methode **Save** speichern. Der Pfad des Windows PowerShell-Anwendungsordners ist in der Variablen **$PSHome** gespeichert, also geben Sie Folgendes ein:

<pre>$lnk.TargetPath = $PSHome $lnk.Save()</pre>

### Verwenden von Internet Explorer in Windows PowerShell
Viele Anwendungen (einschließlich der Microsoft Office-Anwendungen und Internet Explorer) können mithilfe von COM automatisiert werden. Internet Explorer verdeutlicht einige der typischen Techniken und Probleme im Zusammenhang mit COM-basierten Anwendungen.

Sie erstellen eine Internet Explorer-Instanz, indem Sie die ProgID von Internet Explorer angeben, **InternetExplorer.Application**:

```
$ie = New-Object -ComObject InternetExplorer.Application
```

Dieser Befehl startet Internet Explorer, zeigt die Anwendung macht aber nicht an. Wenn Sie „Get-Process“ eingeben, können Sie sehen, dass ein Prozess mit dem Namen „iexplore“ ausgeführt wird. Auch wenn Sie Windows PowerShell beenden, wird der Prozess weiterhin ausgeführt. Sie müssen den Computer neu starten oder ein Tool wie Task-Manager verwenden, um den Prozess „iexplore“ zu beenden.

> [!NOTE]
> Für COM-Objekte, die als separate Prozesse gestartet werden, so genannte *ActiveX-EXE-Dateien*, kann beim Start ein Benutzeroberflächenfenster angezeigt werden, muss aber nicht. Wenn ein Objekt ein Fenster erstellt, dieses aber nicht anzeigt, wie Internet Explorer, geht der Fokus in der Regel an den Windows-Desktop, und Sie müssen das Fenster einblenden, um mit ihm zu interagieren.

Durch Eingabe von **$ie | Get-Member** können Sie Eigenschaften und Methoden für Internet Explorer anzeigen. Um das Internet Explorer-Fenster anzuzeigen, legen Sie die Eigenschaft „Visible“ auf „$true“ fest. Geben Sie dazu Folgendes ein:

```
$ie.Visible = $true
```

Anschließend können Sie mithilfe der Methode „Navigate“ zu einer bestimmten Webadresse navigieren:

```
$ie.Navigate("http://www.microsoft.com/technet/scriptcenter/default.mspx")
```

Unter Verwendung anderer Elemente des Internet Explorer-Objektmodells können Sie Text-Inhalt von der Website abrufen. Mit dem folgenden Befehl können Sie den HTML-Text im Hauptteil der aktuellen Webseite anzeigen:

```
$ie.Document.Body.InnerText
```

Rufen Sie die Methode „Quit()“ auf, um Internet Explorer aus PowerShell heraus zu schließen:

```
$ie.Quit()
```

Dadurch wird das Schließen von Internet Explorer erzwungen. Die Variable „$ie“ enthält keinen gültigen Verweis mehr, obwohl es sich noch um ein COM-Objekt zu handeln scheint. Wenn Sie die Variable verwenden, erhalten Sie einen Automatisierungsfehler:

```
PS> $ie | Get-Member
Get-Member : Exception retrieving the string representation for property "Appli
cation" : "The object invoked has disconnected from its clients. (Exception fro
m HRESULT: 0x80010108 (RPC_E_DISCONNECTED))"
At line:1 char:16
+ $ie | Get-Member <<<<
```

Sie können entweder den verbleibenden Verweis mit einem Befehl wie „$ie = $null“ oder die Variable durch folgende Eingabe entfernen:

```
Remove-Variable ie
```

> [!NOTE]
> Es gibt keinen allgemeinen Standard, der festlegt, ob ActiveX-EXE-Dateien beendet oder weiterhin ausgeführt werden, wenn Sie einen Verweis darauf entfernen. Je nach Situation, also z. B. abhängig davon, ob die Anwendung angezeigt, ein bearbeitetes Dokument darin ausgeführt oder Windows PowerShell noch ausgeführt wird, wird die Anwendung beendet oder auch nicht. Aus diesem Grund sollten Sie das Abbruchverhalten für jede ActiveX-EXE-Datei testen, die Sie in Windows PowerShell verwenden möchten.

### Abrufen von Warnungen zu von .NET Framework umschlossenen COM-Objekten
In einigen Fällen verfügt ein COM-Objekt möglicherweise über einen zugeordneten .NET Framework *Runtime-Callable Wrapper* oder RCW, und dieser wird von **New-Object** verwendet. Da das Verhalten des RCW möglicherweise vom Verhalten des normalen COM-Objekts abweicht, verfügt **New-Object** über einen **Strict**-Parameter, der Sie auf den RCW-Zugriff hinweist. Wenn Sie den Parameter **Strict** angeben und dann ein COM-Objekt erstellen, das einen RCW verwendet, wird eine Warnung angezeigt:

```
PS> $xl = New-Object -ComObject Excel.Application -Strict
New-Object : The object written to the pipeline is an instance of the type "Mic
rosoft.Office.Interop.Excel.ApplicationClass" from the component's primary inte
rop assembly. If this type exposes different members than the IDispatch members
, scripts written to work with this object might not work if the primary intero
p assembly is not installed.
At line:1 char:17
+ $xl = New-Object  <<<< -ComObject Excel.Application -Strict
```

Das Objekt wird zwar erstellt, Sie werden jedoch darauf hingewiesen, dass es sich nicht um ein Standard-COM-Objekt handelt.




<!--HONumber=Oct16_HO1-->


