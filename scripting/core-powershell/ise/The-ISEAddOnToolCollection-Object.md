---
title: Das ISEAddOnToolCollection-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 634eab89-0845-4016-974b-361b09bb8f7b
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 0a8f19693085b9b878fae60953a2cdc358ba8f4c

---

# Das ISEAddOnToolCollection-Objekt
  Das **ISEAddOnToolCollection**-Objekt ist eine Sammlung von **ISEAddOnTool**-Objekten. Ein Beispiel ist das **$psISE.CurrentPowerShellTab.VerticalAddOnTools**-Objekt.

## Methoden

### Add( Name, ControlType, [IsVisible] )
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Fügt der Sammlung ein neues Add-On-Tool hinzu. Das neu hinzugefügte Add-On-Tool wird zurückgegeben. Vor dem Ausführen dieses Befehls müssen Sie das Add-On-Tool auf dem lokalen Computer installieren und die Assembly laden.

 **Name**: Zeichenfolge – gibt den Anzeigenamen des Add-On-Tools an, das zu Windows PowerShell ISE hinzugefügt wird.

 **ControlType**: Typ – gibt das Steuerelement an, das hinzugefügt wird.

 **\[IsVisible\]** – optionale Boolesche Wenn legen Sie **$true**, das Add-on-Tool ist sofort im Bereich zugeordneten Tools angezeigt.

```PowerShell
# Load a DLL with an add-on and then add it to the ISE
[reflection.assembly]::LoadFile("c:\test\ISESimpleSolution\ISESimpleSolution.dll")
$psISE.CurrentPowerShellTab.VerticalAddOnTools.Add("Solutions", [ISESimpleSolution.Solution], $true)
```

### Remove( Item )
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Entfernt das angegebene Add-On-Tool aus der Sammlung.

 **Item**: Microsoft.PowerShell.Host.ISE.ISEAddOnTool – gibt das Objekt an, das aus Windows PowerShell ISE entfernt werden soll.

```PowerShell
# Load a DLL with an add-on and then add it to the ISE
[reflection.assembly]::LoadFile("c:\test\ISESimpleSolution\ISESimpleSolution.dll")
$psISE.CurrentPowerShellTab.VerticalAddOnTools.Add("Solutions", [ISESimpleSolution.Solution], $true)
```

### SetSelectedPowerShellTab( psTab )
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Wählt die PowerShell-Registerkarte aus, die vom **psTab**-Parameter angegeben wird.

 **psTab**: Microsoft.PowerShell.Host.ISE.PowerShellTab – die auszuwählende PowerShell-Registerkarte.

```PowerShell
      $newTab = $psISE.PowerShellTabs.Add()
# Change the DisplayName of the new PowerShell tab. 
$newTab.DisplayName="Brand New Tab"
```

### Remove( psTab )
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Entfernt die PowerShell Registerkarte, die vom **psTab**-Parameter angegeben wird.

 **psTab**: Microsoft.PowerShell.Host.ISE.PowerShellTab – die zu entfernende PowerShell-Registerkarte.

```PowerShell
$newTab = $psISE.PowerShellTabs.Add()
Change the DisplayName of the new PowerShell tab. 
$newTab.DisplayName="This tab will go away in 5 seconds" 
sleep 5 
$psISE.PowerShellTabs.Remove($newTab)
```

## Weitere Informationen
- [Das PowerShellTab-Objekt](The-PowerShellTab-Object.md) 
- [Die Windows PowerShell ISE-Skriptobjektmodell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
- [Windows PowerShell ISE-Objektmodellreferenz](Windows-PowerShell-ISE-Object-Model-Reference.md) 
- [Die ISE Objektmodellhierarchie](The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Oct16_HO1-->


