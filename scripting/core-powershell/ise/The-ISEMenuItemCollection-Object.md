---
title: Das ISEMenuItemCollection-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0c0f5484-3320-408e-8534-5bd1c8e48512
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 563bfc58e545a9e67eb9dd89d8d28e1aa2a33f1c

---

# Das ISEMenuItemCollection-Objekt
  Ein **ISEMenuItemCollection**-Objekt ist eine Sammlung von **ISEMenuItem**-Objekten. Es ist eine Instanz der Microsoft.PowerShell.Host.ISE.ISEMenuItemCollection-Klasse. Ein Beispiel ist das **$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus**-Objekt, das verwendet wird, um das Menü **Add-On** in Windows PowerShell® Integrated Scripting Environment (ISE) anzupassen.

## Methode

### Add(string DisplayName, System.Management.Automation.ScriptBlock Action, System.Windows.Input.KeyGesture Shortcut )
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Fügt der Sammlung ein Menüelement hinzu.

 **DisplayName**
 Der Anzeigename des hinzuzufügenden Menüs.

 **Action**
 Das **System.Management.Automation.ScriptBlock**-Objekt, das die diesem Menüelement zugeordnete Aktion angibt.

 **Shortcut**
 Die Tastenkombination für diese Aktion.

 **Returns**
 Das ISEMenuItem-Objekt, das soeben hinzugefügt wurde.

```
# Create an Add-ons menu with an fast access key and a shortcut.
# Note the use of "_"  as opposed to the "&" for mapping to the fast access key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("_Process",{get-process},"Alt+P")
```

### Clear()
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Entfernt alle Untermenüs des Menüelements.

```
# Remove all custom submenu items from the AddOns menu
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()

```

## Weitere Informationen
 [Das ISEMenuItem-Objekt](The-ISEMenuItem-Object.md) 
 [Das Windows PowerShell ISE-Skriptobjektmodell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referenz zum Windows PowerShell ISE-Objektmodell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [Die ISE-Objektmodellhierarchie](The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Oct16_HO1-->


