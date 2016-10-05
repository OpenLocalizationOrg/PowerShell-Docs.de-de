---
title: Entfernen von Objekten aus der Pipeline  Where-Object
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 01df8b22-2d22-4e2c-a18d-c004cd3cc284
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 94117fcf337ecf550d6df1d167e608ba64582c03

---

# Entfernen von Objekten aus der Pipeline (Where-Object)
In Windows PowerShell geschieht es häufig, dass Sie mehr Objekte als gewünscht generieren und an eine Pipeline übergeben. Mithilfe der **Format**-Cmdlets können Sie die Eigenschaften bestimmter Objekte angeben, die angezeigt werden sollen, aber dies hilft dabei, ganze Objekte aus der Anzeige zu entfernen. Möglicherweise möchten Sie die Objekte vor dem Ende einer Pipeline filtern, um nur für eine Teilmenge der ursprünglich generierten Objekte bestimmte Aktionen auszuführen.

Windows PowerShell enthält ein **Where-Object**-Cmdlet, mit dem Sie jedes Objekt in der Pipeline testen können, um es nur dann an die Pipeline zu übergeben, wenn es eine bestimmte Testbedingung erfüllt. Objekte, die den Test nicht bestehen, werden aus der Pipeline entfernt. Sie geben die Testbedingung als Wert des Parameters **Where-ObjectFilterScript** an.

### Ausführen einfacher Tests mit „Where-Object“
Der Wert von **FilterScript** ist ein *Skriptblock* (d.h. ein oder mehrere, in geschweifte Klammern {} eingeschlossene Windows PowerShell-Befehle), der als „true“ oder „false“ ausgewertet wird. Diese Skriptblöcke können sehr einfach sein, aber ihre Erstellung erfordert Kenntnisse eines anderen Windows PowerShell-Konzepts: Vergleichsoperatoren. Mit einem Vergleichsoperator werden die Elemente auf den beiden Seiten des Operators verglichen. Vergleichsoperatoren beginnen mit einem Minuszeichen (-), gefolgt von einem Namen. Die grundlegenden Vergleichsoperatoren können für nahezu jede Art von Objekt verwendet werden. Die erweiterten Vergleichsoperatoren können möglicherweise nur mit Text oder Arrays verwendet werden.

> [!NOTE]
> Bei Windows PowerShell-Vergleichsoperatoren wird beim Arbeiten mit Text die Groß-/Kleinschreibung standardmäßig nicht beachtet.

Aufgrund von Analyseaspekten werden Symbole wie <, > und = nicht als Vergleichsoperatoren verwendet. Stattdessen bestehen Vergleichsoperatoren aus Buchstaben. In der folgenden Tabelle sind die grundlegenden Vergleichsoperatoren aufgeführt.

|Vergleichsoperator|Bedeutung|Beispiel (gibt „true“ zurück)|
|-----------------------|-----------|--------------------------|
|-eq|Ist gleich|1 -eq 1|
|-ne|Ist ungleich|1 -ne 2|
|-lt|Ist kleiner als|1 -lt 2|
|-le|Ist kleiner als oder gleich|1 -le 2|
|-gt|Ist größer als|2 -gt 1|
|-ge|Ist größer als oder gleich|2 -ge 1|
|-like|Entspricht (Platzhaltervergleich für Text)|"entweder"-wie "F\ * intensive?"|
|-notlike|Entspricht nicht (Platzhaltervergleich für Text)|"entweder"-notlike "P\ * .doc"|
|-contains|Enthält|1,2,3 -contains 1|
|-notcontains|Enthält nicht|1,2,3 -notcontains 4|

In „Where-Object“-Skriptblöcken dient die spezielle Variable „$_“ zum Verweisen auf das aktuelle Objekt in der Pipeline. Es folgt ein Beispiel für die Funktionsweise. Wenn Sie über eine Liste mit Zahlen verfügen und nur diejenigen zurückgegeben möchten, die kleiner als 3 sind, können Sie die Zahlen mit „Where-Object“ folgendermaßen filtern:

```
PS> 1,2,3,4 | Where-Object -FilterScript {$_ -lt 3}
1
2
```

### Filtern basierend auf Objekteigenschaften
Da „$_“ auf das aktuelle Pipelineobjekt verweist, können wir für unsere Tests auf seine Eigenschaften zugreifen.

Als Beispiel können wir uns die Klasse „Win32_SystemDriver“ in WMI anschauen. Möglicherweise gibt es in einem bestimmten System Hunderte von Systemtreibern, Sie sind aber vielleicht nur an einer bestimmten Auswahl von Systemtreibern interessiert, z. B. an den Treibern, die gerade ausgeführt werden. Wenn Sie „Get-Member“ verwenden, um „Win32_SystemDriver“-Elemente anzuzeigen (**Get-WmiObject -Class Win32_SystemDriver | Get-Member -MemberType Property**) sehen Sie, dass die relevante Eigenschaft „State“ ist und den Wert „Running“ aufweist, wenn der Treiber ausgeführt wird. Sie können die Systemtreiber so filtern, dass nur die ausgeführten Treiber ausgewählt werden. Geben Sie dazu Folgendes ein:

```
Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript {$_.State -eq "Running"}
```

Dies erzeugt immer noch eine lange Liste. Möglicherweise möchten Sie die Liste weiter filtern, um nur die Treiber auszuwählen, die automatisch gestartet werden. Verwenden Sie dazu außerdem den „StartMode“-Wert:

```
PS> Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript {$_.State -eq "Running"} | Where-Object -FilterScript {$_.StartMode -eq "Auto"}

DisplayName : RAS Asynchronous Media Driver
Name        : AsyncMac
State       : Running
Status      : OK
Started     : True

DisplayName : Audio Stub Driver
Name        : audstub
State       : Running
Status      : OK
Started     : True
```

Dadurch erhalten Sie eine Vielzahl von Informationen, die Sie nicht mehr benötigen, da bekannt ist, dass die Treiber ausgeführt werden. In der Tat sind die einzigen Informationen, die Sie an dieser Stelle wahrscheinlich benötigen, der Name und der Anzeigename. Der folgende Befehl enthält nur diese zwei Eigenschaften, wodurch eine wesentlich vereinfachte Ausgabe erzeugt wird:

```
PS> Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript {$_.State -eq "Running"} | Where-Object -FilterScript {$_.StartMode -eq "Manual"} | Format-Table -Property Name,DisplayName

Name                                    DisplayName
----                                    -----------
AsyncMac                                RAS Asynchronous Media Driver
Fdc                                     Floppy Disk Controller Driver
Flpydisk                                Floppy Disk Driver
Gpc                                     Generic Packet Classifier
IpNat                                   IP Network Address Translator
mouhid                                  Mouse HID Driver
MRxDAV                                  WebDav Client Redirector
mssmbios                                Microsoft System Management BIOS Driver
```

Der oben stehende Befehl enthält zwei „Where-Object“-Elemente, die aber mit Hilfe des logischen Operators „-and“ wie folgt in einem einzigen „Where-Object“-Element ausgedrückt werden können:

```
Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript { ($_.State -eq "Running") -and ($_.StartMode -eq "Manual") } | Format-Table -Property Name,DisplayName
```

In der folgenden Tabelle sind die standardmäßigen logischen Operatoren aufgeführt.

|Logischer Operator|Bedeutung|Beispiel (gibt „true“ zurück)|
|--------------------|-----------|--------------------------|
|-and|Logisches „Und“; „true“, wenn beide Seiten zutreffen|(1 -eq 1) -and (2 -eq 2)|
|-or|Logisches „Oder“; „true“, wenn eine der beiden Seiten zutrifft|(1 -eq 1) -or (1 -eq 2)|
|-not|Logisches „Nicht“; kehrt „true“ und „false“ um|-not (1 -eq 2)|
|\!|Logisches „Nicht“; kehrt „true“ und „false“ um|\! (1 - Eq 2)|




<!--HONumber=Oct16_HO1-->


