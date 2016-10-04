---
title: "Ändern des Computerstatus"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 8093268b-27f8-4a49-8871-142c5cc33f01
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1779b9de13a30a43236e24793e5196261a7db77f

---

# Ändern des Computerstatus
Verwenden Sie zum Zurücksetzen eines Computers in Windows PowerShell entweder ein Standardbefehlszeilentool oder ein WMI-Klasse. Sie verwenden Windows PowerShell zwar nur zum Ausführen des Tools, dennoch erfahren Sie einige wichtige Details über die Arbeit mit externen Tools in Windows PowerShell, indem Sie lernen, wie Sie den Energiezustand eines Computers in Windows PowerShell ändern.

### Sperren eines Computers
Die einzige Möglichkeit, einen Computer unmittelbar mit den standardmäßig verfügbaren Tools zu sperren, besteht im Aufrufen der Funktion **LockWorkstation()** in **user32.dll**:

```
rundll32.exe user32.dll,LockWorkStation
```

Mit diesem Befehl wird die Arbeitsstation sofort gesperrt. Hierbei wird *rundll32.exe* zum Ausführen von Windows-DLLs (und Speichern ihrer Bibliotheken für die wiederholte Verwendung) verwendet, um „user32.dll“, eine Bibliothek von Windows-Verwaltungsfunktionen, auszuführen.

Wenn Sie eine Arbeitsstation sperren, während die schnelle Benutzerumschaltung aktiviert ist, z. B. bei Computern unter Windows XP, wird der Benutzeranmeldebildschirm anstelle des Bildschirmschoners des aktuellen Benutzers angezeigt.

Um bestimmte Sitzungen auf einem Terminalserver herunterzufahren, verwenden Sie das Befehlszeilentool **tsshutdn.exe**.

### Abmelden der aktuellen Sitzung
Es sind mehrere verschiedene Verfahren verfügbar, um eine Sitzung auf dem lokalen System abzumelden. Die einfachste Möglichkeit besteht in der Verwendung des Remotedesktop-/Terminaldienste-Befehlszeilentools **logoff.exe**. (Weitere Informationen erhalten Sie, indem Sie an der Windows PowerShell-Eingabeaufforderung **logoff /?** eingeben). Zum Abmelden der aktuellen aktiven Sitzung geben Sie **logoff** ohne Argumente ein.

Sie können auch das Tool **shutdown.exe** mit der zugehörigen Abmeldeoption verwenden:

```
shutdown.exe -l
```

Eine dritte Möglichkeit ist die Verwendung von WMI. Die Klasse „Win32_OperatingSystem“ besitzt eine Win32Shutdown-Methode. Durch Aufrufen der Methode mit dem 0-Flag wird die Abmeldung initiiert:

```
(Get-WmiObject -Class Win32_OperatingSystem -ComputerName .).Win32Shutdown(0)
```

Weitere Informationen und andere Funktionen der Win32Shutdown-Methode finden Sie unter „Win32Shutdown-Methode der Win32_OperatingSystem-Klasse“ in MSDN.

### Herunterfahren oder Neustarten eines Computers
Das Herunterfahren und Neustarten von Computern wird im Allgemeinen vom gleichen Tasktyp ausgeführt. Mit Tools, die einen Computer herunterfahren, kann normalerweise auch ein Neustart ausgeführt werden, und umgekehrt. Es gibt zwei einfache Optionen für den Neustart eines Computers aus Windows PowerShell. Verwenden Sie entweder „Tsshutdn.exe“ oder „Shutdown.exe“ mit den entsprechenden Argumenten. Ausführliche Informationen zur Verwendung erhalten Sie mit **tsshutdn.exe /?** oder **shutdown.exe /?**.

Sie können auch **Win32_OperatingSystem** direkt in Windows PowerShell zum Herunterfahren und Neustarten verwenden.

Verwenden Sie zum Herunterfahren des Computers die Win32Shutdown-Methode mit dem Flag **1**.

```
(Get-WmiObject -Class Win32_OperatingSystem -ComputerName .).Win32Shutdown(1)
```

Verwenden Sie zum Neustarten des Betriebssystems die Win32Shutdown-Methode mit dem Flag **2**.

```
(Get-WmiObject -Class Win32_OperatingSystem -ComputerName .).Win32Shutdown(2)
```




<!--HONumber=Oct16_HO1-->


