---
title: Das ISEFileCollection-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0f86a427-ea38-4bce-85f8-06c98d30d508
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: c334a38d6686be45101f4569f38411e9703c8fea

---

# Das ISEFileCollection-Objekt
  Das **ISEFileCollection**-Objekt ist eine Sammlung von **ISEFile**-Objekten. Ein Beispiel ist die $psISE.CurrentPowerShellTab.Files-Sammlung.

## Methoden

### Add\( \[fullPath\] \)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Erstellt eine neue unbenannte Datei, gibt diese zurück und fügt sie der Sammlung hinzu. Die **IsUntitled** -Eigenschaft der neu erstellte Datei wird **$true**.

 **\[fullPath\]** – Optional Zeichenfolge den vollständigen Pfad der Datei. Eine Ausnahme wird erstellt, falls Sie den **fullPath**-Parameter und den relativen Pfad angeben oder falls Sie einen Dateinamen statt dem vollständigen Pfad verwenden.

```
# Adds a new untitled file to the collection of files in the current PowerShell tab.
$newFile = $psISE.CurrentPowerShellTab.Files.Add()

# Adds a file specified by its full path to the collection of files in the current PowerShell tab.
$psISE.CurrentPowerShellTab.Files.Add("$pshome\Examples\profile.ps1")

```

### Remove\( File, \[Force\] \)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Entfernt eine angegebene Datei aus der aktuellen PowerShell-Registerkarte.

 **Datei** -Zeichenfolge der ISEFile-Datei, die aus der Auflistung entfernt werden soll. Falls die Datei nicht gespeichert wurde, löst diese Methode eine Ausnahme aus. Verwenden Sie den Schalter **Force**, um das Entfernen einer ungespeicherten Datei zu erzwingen.

 **\[Force\]** – optionale Boolesche Wenn legen Sie **$true**, gewährt die Berechtigung, die Datei zu entfernen, selbst wenn sie nach der letzten Verwendung nicht gespeichert wurde. Der Standardwert ist **$false**.

```
# Removes the first opened file from the file collection associated with the current PowerShell tab.
# If the file has not yet been saved, then an exception is generated.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.Remove($firstfile)

# Removes the first opened file from the file collection associated with the current PowerShell tab, even if it has not been saved.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.Remove($firstfile, $true)
```

### SetSelectedFile\( selectedFile \)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Wählt die Datei aus, die durch den **selectedFile**-Parameter angegeben wird.

 **SelectedFile** – Microsoft.PowerShell.Host.ISE.ISEFile der ISEFile-Datei, die Sie auswählen möchten.

```

# Selects the specified file.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.SetSelectedFile($firstfile)

```

## Weitere Informationen
 [Das Objekt ISEFile](The-ISEFile-Object.md) 
 [der Windows PowerShell ISE-Skriptobjektmodell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Windows PowerShell ISE-Objektmodellreferenz](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [die ISE Objektmodellhierarchie](The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Oct16_HO1-->


