---
title: Das PowerShellTab-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: a9b58556-951b-4f48-b3ae-b351b7564360
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: f3f2d27c3c82406f8e8967fd1784a6e07579c1fa

---

# Das PowerShellTab-Objekt
  Das **PowerShellTab**-Objekt stellt eine Windows PowerShell-Laufzeitumgebung dar.

## Methoden

###  <a name="invoke"></a> Aufrufen\ (Skript \)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Führt das angegebene Skript auf der PowerShell-Registerkarte aus.

> [!NOTE]
>  Diese Methode kann nur für andere PowerShell-Registerkarten verwendet werden, nicht für die PowerShell-Registerkarte, auf der sie ausgeführt wird. Sie gibt keine Objekte oder Werte zurück. Wenn durch den Code eine Variable geändert wird, bleiben diese Änderungen auf der Registerkarte erhalten, für die der Befehl aufgerufen wurde.

 **Script**: System.Management.Automation.ScriptBlock oder Zeichenfolge – der auszuführende Skriptblock.

```
# Manually create a second PowerShell tab before running this script.
# Return to the first PowerShell tab and type the following command
$psise.PowerShellTabs[1].Invoke({dir})
```

### InvokeSynchronous( Script, [useNewScope], millisecondsTimeout )
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Führt das angegebene Skript auf der PowerShell-Registerkarte aus.

> [!NOTE]
>  Diese Methode kann nur für andere PowerShell-Registerkarten verwendet werden, nicht für die PowerShell-Registerkarte, auf der sie ausgeführt wird. Der Skriptblock wird ausgeführt, und alle vom Skript zurückgegebenen Werte werden an die Laufzeitumgebung zurückgegeben, in der der Befehl aufgerufen wurde. Wenn die Ausführung des Befehls länger dauert, als vom Wert **millesecondsTimeout** angegeben, tritt durch den Befehl ein Fehler mit folgender Ausnahme auf: „Timeout für Vorgang überschritten.“

 **Script**: System.Management.Automation.ScriptBlock oder Zeichenfolge – der auszuführende Skriptblock.

 **\[useNewScope\]** -Optionaler boolescher Wert, der standardmäßig **$true**
 Wenn festgelegt **$true**, und klicken Sie dann ein neuer Bereich in der zum Ausführen des Befehls erstellt wird. Die Laufzeitumgebung der vom Befehl angegebenen PowerShell-Registerkarte wird nicht geändert.

 **\[millisecondsTimeout\]** -optionale ganze Zahl, die standardmäßig **500**.
Wenn der Befehl nicht innerhalb der angegebenen Zeit abgeschlossen wird, generiert der Befehl eine **TimeoutException** mit der Meldung „Timeout für Vorgang überschritten“.

```
# create a new PowerShell tab and then switch back to the first
$PSise.PowerShellTabs.Add()
$psISE.PowerShellTabs.SetSelectedPowerShellTab($psISE.PowerShellTabs[0]) 

# Invoke a simple command on the other tab, in its own scope
$psISE.PowerShellTabs[1].InvokeSynchronous('$x=1',$false)
# You can switch to the other tab and type “$x” to see that the value is saved there.

# This example sets a value in the other tab (in a different scope) 
# and returns it through the pipeline to this tab to store in $a
$a=$psISE.PowerShellTabs[1].InvokeSynchronous('$z=3;$z') 
$a

# This example runs a command that takes longer than the allowed timeout value
# and measures how long it runs so that you can see the impact
measure-command {$psISE.PowerShellTabs[1].InvokeSynchronous("sleep 10",$false,5000)}

```

## Eigenschaften

###  <a name="AddOnsMenu"></a> AddOnsMenu
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die das Add-On-Menü für die PowerShell-Registerkarte abruft.

```
# Clear the Add-ons menu if one exists.
$psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Clear()
# Create an AddOns menu with an accessor.
# Note the use of "_"  as opposed to the "&" for mapping to the fast key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("_Process",{get-process},"Alt+P") 
# Add a nested menu. 
$parentAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("Parent",$null,$null) 
$parentAdded.SubMenus.Add("_Dir",{dir},"Alt+D")
# Show the Add-ons menu on the current PowerShell tab.
$psISE.CurrentPowerShellTab.AddOnsMenu
```

###  <a name="CanExecute"></a> CanInvoke
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte boolesche Eigenschaft, die den Wert **$true** zurückgibt, wenn ein Skript mit der [Invoke( Script )](#invoke)-Methode aufgerufen werden kann.

```
# CanInvoke will be false if the PowerShell
# tab is running a script that takes a while, and you
# check its properties from another PowerShell tab. It is
# always false if checked on the current PowerShell tab. 
# Manually create a second PowerShell tab before running this script.
# Return to the first tab and type
$secondTab = $psise.PowerShellTabs[1] 
$secondTab.CanInvoke 
$secondTab.Invoke({sleep 20})
$secondTab.CanInvoke

```

###  <a name="Commandpane"></a> Consolepane
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.  In Windows PowerShell ISE 2.0 lautete der Name hierfür **CommandPane**.

 Die schreibgeschützte Eigenschaft, die das [editor](../ise/The-ISEEditor-Object.md)-Objekt des Konsolenbereichs abruft.

```
# Gets the Console Pane editor.
$psISE.CurrentPowerShellTab.ConsolePane

```

###  <a name="Displayname"></a> DisplayName
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die Lese-/Schreibeigenschaft Eigenschaft, die den auf der PowerShell-Registerkarte angezeigten Text abruft oder festlegt. Standardmäßig lautet der Name von Registerkarten „PowerShell #“, wobei # eine Zahl darstellt.

```
$newTab = $psise.PowerShellTabs.Add()
# Change the DisplayName of the new PowerShell tab. 
$newTab.DisplayName="Brand New Tab"
```

###  <a name="ExpandedScript"></a> ExpandedScript
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die Lese-/Schreibeigenschaft, die bestimmt, ob der Skript-Bereich erweitert oder ausgeblendet wird.

```
# Toggle the expanded script property to see its effect.
$PSise.CurrentPowerShellTab.ExpandedScript=!$PSise.CurrentPowerShellTab.ExpandedScript

```

###  <a name="Files"></a> Dateien
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die die [Sammlung von Skriptdateien](../ise/The-ISEFileCollection-Object.md) abruft, die auf der PowerShell-Registerkarte geöffnet sind.

```
$newFile = $psISE.CurrentPowerShellTab.Files.Add()
$newFile.Editor.Text = "a`r`nb" 
# Gets the line count
$newFile.Editor.LineCount
```

###  <a name="Output"></a> Ausgabe
  Dieses Feature ist in Windows PowerShell ISE 2.0 enthalten, wurde in höheren Versionen von ISE aber entfernt oder umbenannt.  In höheren Versionen von Windows PowerShell ISE können Sie das **ConsolePane**-Objekt für den gleichen Zweck verwenden.

 Die schreibgeschützte Eigenschaft, die den Ausgabebereich des aktuellen [editor](../ise/The-ISEEditor-Object.md) abruft.

```
# Clears the text in the Output pane.
$psise.CurrentPowerShellTab.output.clear()
```

###  <a name="Prompt"></a> Auffordern
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die den aktuellen Aufforderungstext abruft. Hinweis: Die **Prompt**-Funktion kann durch das Profil des Benutzers überschrieben werden. Wenn das Ergebnis von einer einfachen Zeichenfolge abweicht, gibt diese Eigenschaft nichts zurück.

```
# Gets the current prompt text.
$psISE.CurrentPowerShellTab.Prompt
```

###  <a name="ShowCommands"></a> ShowCommands
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Die Lese-/Schreibeigenschaft, die angibt, ob der Befehlsbereich derzeit angezeigt wird.

```
# Gets the current status of the Commands pane and stores it in the $a variable
$a = $psISE.CurrentPowerShellTab.ShowCommands
# if $a is $false, then turn the Commands pane on by changing the value to $True
if (!$a) {$psISE.CurrentPowerShellTab.ShowCommands=$True}
```

###  <a name="StatusText"></a> StatusText
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die den **PowerShellTab**-Statustext abruft.

```
# Gets the current status text,
$psISE.CurrentPowerShellTab.StatusText
```

###  <a name="HorizontalAddOnToolsPaneOpened"></a> HorizontalAddOnToolsPaneOpened
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Die schreibgeschützte Eigenschaft, die angibt, ob der horizontale Add-On-Toolbereich derzeit geöffnet ist.

```
# Gets the current state of the horizontal Add-ons tool pane. 
$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened
```

###  <a name="VerticalAddOnToolsPaneOpened"></a> **VerticalAddOnToolsPaneOpened**
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Die schreibgeschützte Eigenschaft, die angibt, ob der vertikale Add-On-Toolbereich derzeit geöffnet ist.

```
# Turns on the Commands pane
$psISE.CurrentPowerShellTab.ShowCommands=$True
# Gets the current state of the vertical Add-ons tool pane. 
$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened
```

## Weitere Informationen
 [Das PowerShellTabCollection-Objekt](The-PowerShellTabCollection-Object.md) 
 [Das Windows PowerShell ISE-Skriptobjektmodell](../ise/The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referenz zum Windows PowerShell ISE-Objektmodell](../ise/Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [Die ISE-Objektmodellhierarchie](../ise/The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Oct16_HO1-->


