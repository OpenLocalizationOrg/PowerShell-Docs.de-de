---
title: Arbeiten mit Druckern
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 4f29ead3-f83b-4706-ac3e-f2154ff38dc5
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: c013124d12a551245152c1703e5f1d8a3f8f5f70

---

# Arbeiten mit Druckern
Sie können Windows PowerShell zum Verwalten von Druckern mit WMI und dem COM-Objekt „WScript.Network“ vom WSH verwenden. Wir werden eine Kombination beider Tools verwenden, um bestimmte Aufgaben zu veranschaulichen.

### Auflisten von Druckerverbindungen
Die einfachste Möglichkeit, die auf einem Computer installierten Drucker aufzulisten, ist die Verwendung der WMI-Klasse **Win32_Printer**:

```
Get-WmiObject -Class Win32_Printer -ComputerName
```

Sie können die Drucker auch mit dem COM-Objekt **WScript.Network** auflisten, das normalerweise in WSH-Skripts verwendet wird:

```
(New-Object -ComObject WScript.Network).EnumPrinterConnections()
```

Da dieser Befehl eine einfache Zeichenfolgenauflistung von Portnamen und Druckergerätenamen ohne unterscheidende Bezeichnungen zurückgibt, ist die Ausgabe nicht leicht zu interpretieren.

### Hinzufügen eines Netzwerkdruckers
Verwenden Sie zum Hinzufügen eines neuen Netzwerkdruckers **WScript.Network**:

```
(New-Object -ComObject WScript.Network).AddWindowsPrinterConnection("\\Printserver01\Xerox5")
```

### Festlegen eines Standarddruckers
Um mithilfe von WMI den Standarddrucker festzulegen, suchen Sie den Drucker in der **Win32_Printer**-Sammlung, und rufen Sie dann die Methode **SetDefaultPrinter** auf:

```
(Get-WmiObject -ComputerName . -Class Win32_Printer -Filter "Name='HP LaserJet 5Si'").SetDefaultPrinter()
```

**WScript.Network** ist etwas einfacher zu verwenden, da es eine **SetDefaultPrinter**-Methode besitzt, die nur den Druckernamen als Argument akzeptiert:

```
(New-Object -ComObject WScript.Network).SetDefaultPrinter('HP LaserJet 5Si')
```

### Entfernen einer Druckerverbindung
Um eine Druckerverbindung zu entfernen, verwenden Sie die Methode **WScript.Network RemovePrinterConnection**:

```
(New-Object -ComObject WScript.Network).RemovePrinterConnection("\\Printserver01\Xerox5")
```




<!--HONumber=Oct16_HO1-->


