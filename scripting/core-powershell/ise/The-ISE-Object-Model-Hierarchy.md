---
title: Die ISE-Objektmodellhierarchie
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: bc3300e4-9c17-4f00-a621-c8867126e3b3
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 12a47e57d461f1e57cd9c7b20365627378d7e87a

---

# Die ISE-Objektmodellhierarchie
  In diesem Thema wird die Hierarchie der Objekte beschrieben, die Teil von Windows PowerShell Integrated Scripting Environment (ISE) sind. Windows PowerShell ISE ist in Windows PowerShell 3.0 und in Windows PowerShell 4.0 enthalten. Klicken Sie auf ein Objekt, um die Dokumentation für die Klasse aufzurufen, die das Objekt definiert.

##  <a name="psISE"></a> **$psISE-Objekt**
 Das **$psISE**-Objekt ist das [Stammobjekt](The-ObjectModelRoot-Object.md) der Windows PowerShell ISE-Objekthierarchie. Es befindet sich auf der obersten Ebene und macht die folgenden Objekte für Skripts verfügbar:

-   **[$psISE.CurrentFile](#currentfile)**

-   **[$psISE.CurrentPowerShellTab](#currentpowershelltab)**

-   **[$psISE.CurrentVisibleHorizontalTool](#CurrentVisibleHorizontalTool)**

-   **[$psISE.CurrentVisibleVerticalTool](#CurrentVisibleVerticalTool)**

-   **[$psISE.Options](#options)**

-   **[$psISE.PowerShellTabs](#powershelltabs)**

##  <a name="CurrentFile"></a> **[$psISE.CurrentFile](The-ISEFile-Object.md)**
 Das **$psISE.CurrentFile**-Objekt ist eine Instanz der [ISEFile](The-ISEFile-Object.md)-Klasse und macht die folgenden Objekte für Skripts verfügbar:

-   **[$psISE.CurrentFile.DisplayName](The-ISEFile-Object.md#Displayname)**

-   **[$psISE.CurrentFile.Editor](The-ISEEditor-Object.md)**: Dieses Objekt ist eine Instanz der [ISEEditor](The-ISEEditor-Object.md)-Klasse und macht die folgenden Objekte für Skripts verfügbar:

    -   **[$psISE.CurrentFile.Editor.CanGoToMatch](The-ISEEditor-Object.md#cangotomatch)**

    -   **[CaretColumn](The-ISEEditor-Object.md#CaretColumn)**

    -   **[CaretLine](The-ISEEditor-Object.md#CaretLine)**

    -   **[$psISE.CurrentFile.Editor.CaretLineText](The-ISEEditor-Object.md#CaretLineText)**

    -   **[LineCount](The-ISEEditor-Object.md#LineCount)**

    -   **[SelectedText](The-ISEEditor-Object.md#SelectedText)**

    -   **[Text](The-ISEEditor-Object.md#Text)**

-   **[Codierung](The-ISEFile-Object.md#Encoding)**

-   **[Vollständiger Pfad](The-ISEFile-Object.md#FullPath)**

-   **[IsSaved](The-ISEFile-Object.md#IsSaved)**

-   **[IsUntitled](The-ISEFile-Object.md#IsUntitled)**

##  <a name="CurrentPowerShellTab"></a> **[$psISE.CurrentPowerShellTab](The-PowerShellTab-Object.md)**
 Das **$psISE.CurrentPowerShellTab**-Objekt ist eine Instanz der [PowerShellTab](The-PowerShellTab-Object.md)-Klasse und macht die folgenden Objekte für Skripts verfügbar:

-   **[$psISE.CurrentPowerShellTab.AddOnsMenu](The-ISEMenuItem-Object.md)**: Dieses Objekt ist eine Instanz der [ISEMenuItem](The-ISEMenuItem-Object.md)-Klasse und macht die folgenden Objekte für Skripts verfügbar:

    -   **[$psISE.CurrentPowerShellTab.AddOnsMenu.Action](The-ISEMenuItem-Object.md#Action)**

    -   **[$psISE.CurrentPowerShellTab.AddOnsMenu.DisplayName](The-ISEMenuItem-Object.md#DisplayName)**

    -   **[$psISE.CurrentPowerShellTab.AddOnsMenu.Shortcut](The-ISEMenuItem-Object.md#Shortcut)**

    -   **[$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus](The-ISEMenuItemCollection-Object.md)**

-   **[$psISE.CurrentPowerShellTab.CanInvoke](The-PowerShellTab-Object.md#CanExecute)**

-   **[$psISE.CurrentPowerShellTab.ConsolePane](The-ISEEditor-Object.md)**: Dieses Objekt ist eine Instanz der [ISEEditor](The-ISEEditor-Object.md)-Klasse und macht die folgenden Objekte für Skripts verfügbar:

    -   **[$psISE.CurrentPowerShellTab.ConsolePane.CanGoToMatch](The-ISEEditor-Object.md#cangotomatch)**

    -   **[CaretColumn](The-ISEEditor-Object.md#CaretColumn)**

    -   **[CaretLine](The-ISEEditor-Object.md#CaretLine)**

    -   **[$psISE.CurrentPowerShellTab.ConsolePane.CaretLineText](The-ISEEditor-Object.md#CaretLineText)**

    -   **[LineCount](The-ISEEditor-Object.md#LineCount)**

    -   **[SelectedText](The-ISEEditor-Object.md#SelectedText)**

    -   **[Text](The-ISEEditor-Object.md#Text)**

-   **[$psISE.CurrentPowerShellTab.DisplayName](The-PowerShellTab-Object.md#Displayname)**

-   **[$psISE.CurrentPowerShellTab.ExpandedScript](The-PowerShellTab-Object.md#ExpandedScript)**

-   **[$psISE.CurrentPowerShellTab.Files](The-ISEFileCollection-Object.md)**

-   **[$psISE.CurrentPowerShellTab.HorizontalAddOnTools](The-ISEAddOnToolCollection-Object.md)**

-   **[$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened](The-PowerShellTab-Object.md#HorizontalAddOnToolsPaneOpened)**

-   **[$psISE.CurrentPowerShellTab.Prompt](The-PowerShellTab-Object.md#Prompt)**

-   **[$psISE.CurrentPowerShellTab.ShowCommands](The-PowerShellTab-Object.md#ShowCommands)**

-   **[$psISE.CurrentPowerShellTab.Snippets](The-ISESnippetCollection-Object.md)**

-   **[$psISE.CurrentPowerShellTab.StatusText](The-PowerShellTab-Object.md#StatusText)**

-   **[$psISE.CurrentPowerShellTab.VerticalAddOnTools](The-ISEAddOnToolCollection-Object.md)**

-   **[$psISE.CurrentPowerShellTab.VerticalAddOnToolsPaneOpened](The-PowerShellTab-Object.md#VerticalAddOnToolsPaneOpened)**

-   **[$psISE.CurrentPowerShellTab.VisibleHorizontalAddOnTools](The-ISEAddOnToolCollection-Object.md)**

-   **[$psISE.CurrentPowerShellTab.VisibleVerticalAddOnTools](The-ISEAddOnToolCollection-Object.md)**

##  <a name="CurrentVisibleHorizontalTool"></a> **$psISE.CurrentVisibleHorizontalTool**
 Das **$psISE.CurrentVisibleHorizontalTool**-Objekt ist eine Instanz der [ISEAddOnTool](The-ISEAddOnTool-Object.md)-Klasse. Es stellt das installierte Add-On-Tool dar, das derzeit am oberen Rand des Windows PowerShell ISE-Fensters angedockt ist. Das Objekt macht die folgenden Objekte für Skripts verfügbar:

-   **[$psISE.CurrentVisibleHorizontalTool.Control](The-ISEAddOnTool-Object.md#control)**

-   **[$psISE.CurrentVisibleHorizontalTool.IsVisible](The-ISEAddOnTool-Object.md#isvisible)**

-   **[$psISE.CurrentVisibleHorizontalTool.Name](The-ISEAddOnTool-Object.md#name)**

##  <a name="CurrentVisibleVerticalTool"></a> **$psISE.CurrentVisibleVerticalTool**
 Das **$psISE.CurrentVisibleHorizontalTool**-Objekt ist eine Instanz der [ISEAddOnTool](The-ISEAddOnTool-Object.md)-Klasse. Es stellt das installierte Add-On-Tool dar, das derzeit am rechten Rand des Windows PowerShell ISE-Fensters angedockt ist. Das Objekt macht die folgenden Objekte für Skripts verfügbar:

-   **[$psISE.CurrentVisibleHorizontalTool.Control](The-ISEAddOnTool-Object.md#control)**

-   **[$psISE.CurrentVisibleHorizontalTool.IsVisible](The-ISEAddOnTool-Object.md#isvisible)**

-   **[$psISE.CurrentVisibleHorizontalTool.Name](The-ISEAddOnTool-Object.md#name)**

##  <a name="Options"></a> **$psISE.Options**
 Das **$psISE.Options**-Objekt macht die folgenden Objekte für Skripts verfügbar:

-   **[$psISE.Options.AutoSaveMinuteInterval](The-ISEOptions-Object.md#asmi)**

-   **[$psISE.Options.ConsolePaneBackgroundColor](The-ISEOptions-Object.md#cpbc)**

-   **[$psISE.Options.ConsolePaneTextForegroundColor](The-ISEOptions-Object.md#conpfc)**

-   **[$psISE.Options.ConsolePaneTextBackgroundColor](The-ISEOptions-Object.md#conptbc)**

-   **[$psISE.Options.ConsoleTokenColors](The-ISEOptions-Object.md#contc)**

-   **[$psISE.Options.DebugBackgroundColor](The-ISEOptions-Object.md#dbc)**

-   **[$psISE.Options.DebugForegroundColor](The-ISEOptions-Object.md#dfc)**

-   **[$psISE.Options.DefaultOptions](The-ISEOptions-Object.md)**

-   **[$psISE.Options.ErrorBackgroundColor](The-ISEOptions-Object.md#ebc)**

-   **[$psISE.Options.ErrorForegroundColor](The-ISEOptions-Object.md#efc)**

-   **[$psISE.Options.FontName](The-ISEOptions-Object.md#fn)**

-   **[$psISE.Options.Fontsize](The-ISEOptions-Object.md#fs)**

-   **[$psISE.Options.IntellisenseTimeoutInSeconds](The-ISEOptions-Object.md#itis)**

-   **[$psISE.Options.MRUCount](The-ISEOptions-Object.md#mc)**

-   **[$psISE.Options.ScriptPaneBackgroundColor](The-ISEOptions-Object.md#spbc)**

-   **[$psISE.Options.ScriptPaneForegroundColor](The-ISEOptions-Object.md#spfc)**

-   **[$psISE.Options.SelectedScriptPaneState](The-ISEOptions-Object.md#ssps)**

-   **[$psISE.Options.ShowDefaultSnippets](The-ISEOptions-Object.md#sds)**

-   **[$psISE.Options.ShowIntellisenseInConsolePane](The-ISEOptions-Object.md#siicp)**

-   **[$psISE.Options.ShowIntellisenseInScriptPane](The-ISEOptions-Object.md#siisp)**

-   **[$psISE.Options.ShowLineNumbers](The-ISEOptions-Object.md#sln)**

-   **[$psISE.Options.ShowOutlining](The-ISEOptions-Object.md#so)**

-   **[$psISE.Options.ShowToolBar](The-ISEOptions-Object.md#stb)**

-   **[$psISE.Options.ShowWarningBeforeSavingOnRun](The-ISEOptions-Object.md#swbsor)**

-   **[$psISE.Options.ShowWarningForDuplicateFiles](The-ISEOptions-Object.md#swfdf)**

-   **[$psISE.Options.TokenColors](The-ISEOptions-Object.md#tc)**

-   **[$psISE.Options.UseEnterToSelectConsolePaneIntellisense](The-ISEOptions-Object.md#uetsicpi)**

-   **[$psISE.Options. UseEnterToSelectScriptPaneIntellisense](The-ISEOptions-Object.md#uetsispi)**

-   **[$psISE.Options.UseLocalHelp](The-ISEOptions-Object.md#ulh)**

-   **[$psISE.Options.VerboseBackgroundColor](The-ISEOptions-Object.md#vbc)**

-   **[$psISE.Options.VerboseForegroundColor](The-ISEOptions-Object.md#vfc)**

-   **[$psISE.Options.WarningBackgroundColor](The-ISEOptions-Object.md#wbc)**

-   **[$psISE.Options.WarningForegroundColor](The-ISEOptions-Object.md#wfc)**

-   **[$psISE.Options.XmlTokenColors](The-ISEOptions-Object.md#xtc)**

-   **[$psISE.Options.Zoom](The-ISEOptions-Object.md#z)**

##  <a name="PowerShellTabs"></a> **[$psISE.PowerShellTabs](The-PowerShellTabCollection-Object.md)**
 Das **$psISE.PowerShellTabs**-Objekt ist eine Instanz der [PowerShellTabCollection](The-PowerShellTabCollection-Object.md)-Klasse. Es ist eine Sammlung aller zurzeit geöffneten PowerShell-Registerkarten, die die verfügbaren Windows PowerShell-Ausführungsumgebungen auf dem lokalen Computer oder auf verbundenen Remotecomputern darstellen. Jedes Element in der Sammlung ist eine Instanz der [PowerShellTab](The-PowerShellTab-Object.md)-Klasse.

## Weitere Informationen
- [Die Windows PowerShell ISE-Skriptobjektmodell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [Windows PowerShell ISE-Objektmodellreferenz](Windows-PowerShell-ISE-Object-Model-Reference.md)




<!--HONumber=Oct16_HO1-->


