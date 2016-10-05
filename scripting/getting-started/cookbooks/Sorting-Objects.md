---
title: Sortieren von Objekten
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 8530caa8-3ed4-4c56-aed7-1295dd9ba199
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 88753c43124cfce3581af2259449be5f01579ae8

---

# Sortieren von Objekten
Damit sich angezeigte Daten einfacher auswerten lassen, können sie mit dem Cmdlet **Sort-Object** geordnet werden. **Sort-Object** erhält den Namen von mindestens einer Eigenschaft, nach der sortiert werden soll, und gibt Daten zurück, die nach den Werten dieser Eigenschaften sortiert sind.

Nehmen Sie das Problem des Auflistens von „Win32_SystemDriver“-Instanzen. Wenn Sie nach Staus (**State**) und dann nach **Name** sortieren möchten, geben Sie Folgendes ein:

```
Get-WmiObject -Class Win32_SystemDriver | Sort-Object -Property State,Name | Format-Table -Property Name,State,Started,DisplayName -AutoSize -Wrap
```

Dies ist zwar eine ziemlich lange Anzeige, aber Sie sehen die Elemente mit demselben Status in Gruppen zusammengefasst:

```
Name           State   Started DisplayName
----           -----   ------- -----------
ACPI           Running    True Microsoft ACPI Driver
AFD            Running    True AFD
AmdK7          Running    True AMD K7 Processor Driver
AsyncMac       Running    True RAS Asynchronous Media Driver
...
Abiosdsk       Stopped   False Abiosdsk
ACPIEC         Stopped   False ACPIEC
aec            Stopped   False Microsoft Kernel Acoustic Echo Canceller
...
```

Sie können die Objekte auch in umgekehrter Reihenfolge sortieren, indem Sie den **Descending**-Parameter angeben. Dadurch wird die Sortierreihenfolge umgekehrt, sodass Namen in umgekehrter alphabetischer Reihenfolge und Zahlen nach absteigender Größe sortiert werden.

```
PS> Get-WmiObject -Class Win32_SystemDriver | Sort-Object -Property State,Name -Descending | Format-Table -Property Name,State,Started,DisplayName -AutoSize -Wrap

Name           State   Started DisplayName
----           -----   ------- -----------
WS2IFSL        Stopped   False Windows Socket 2.0 Non-IFS Service Provider Supp
                               ort Environment
wceusbsh       Stopped   False Windows CE USB Serial Host Driver...
...
wdmaud         Running    True Microsoft WINMM WDM Audio Compatibility Driver
Wanarp         Running    True Remote Access IP ARP Driver
...
```




<!--HONumber=Oct16_HO1-->


