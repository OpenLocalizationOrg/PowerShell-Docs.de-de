---
title: Das PowerShellTabCollection-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 81f4bf4a-83bf-415e-8378-1703792fbb58
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 4456b1e165130fd52249ffdbd7c22ff591061a8e

---

# Das PowerShellTabCollection-Objekt
  Das **PowerShellTab**-Sammlungsobjekt ist eine Sammlung von **PowerShellTab**-Objekten. Jedes **PowerShellTab**-Objekt fungiert als eine separate Laufzeitumgebung. Dies ist eine Instanz der Microsoft.PowerShell.Host.ISE.PowerShellTabs-Klasse. Ein Beispiel ist das **$psISE.PowerShellTabs**-Objekt.

## Methoden

### Add()
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Fügt eine neue PowerShell-Registerkarte zur Sammlung hinzu. Die neu hinzugefügte Registerkarte wird zurückgegeben.

```
$NewTab=$psISE.PowerShellTabs.Add()
$newTab.DisplayName="Brand New Tab"
```

### Remove(Microsoft.PowerShell.Host.ISE.PowerShellTab psTab)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Entfernt die Registerkarte, die durch den **psTab**-Parameter angegeben wird.

 **PsTab**
 die PowerShell-Registerkarte entfernen.

```

$newTab = $psISE.PowerShellTabs.Add()
Change the DisplayName of the new PowerShell tab. 
$newTab.DisplayName="This tab will go away in 5 seconds" 
sleep 5 
$psISE.PowerShellTabs.Remove($newTab)
```

### SetSelectedPowerShellTab(Microsoft.PowerShell.Host.ISE.PowerShellTab psTab)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Wählt die PowerShell-Registerkarte aus, die durch den **psTab**-Parameter angegeben wird, um diese als aktuell aktive PowerShell-Registerkarte festzulegen.

 **PsTab**
 die PowerShell-Registerkarte auswählen.

```
# Save the current tab in a variable and rename it
$OldTab = $psISE.CurrentPowerShellTab
$psISE.CurrentPowerShellTab.DisplayName="Old Tab"
# Create a new tab and give it a new display name
$newTab = $psISE.PowerShellTabs.Add()
$newTab.DisplayName="Brand New Tab" 
# Switch back to the original tab
$psISE.PowerShellTabs.SelectedPowerShellTab=$oldtab
```

## Weitere Informationen
 [Das PowerShellTab-Objekt](The-PowerShellTab-Object.md) 
 [Das Windows PowerShell ISE-Skriptobjektmodell](../ise/The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referenz zum Windows PowerShell ISE-Objektmodell](../ise/Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [Die ISE-Objektmodellhierarchie](../ise/The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Oct16_HO1-->


