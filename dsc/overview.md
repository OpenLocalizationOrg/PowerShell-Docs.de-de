---
title: "Windows PowerShell DSC – Übersicht"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1a796658eb30bdf5c37ea3677f94767260a34b45

---

# Windows PowerShell DSC – Übersicht 

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

In diesem Thema wird das Feature DSC (Desired State Configuration, Konfiguration des gewünschten Zustands) in Windows PowerShell beschrieben. Dieses Thema bietet einen Überblick über DSC und die Dokumentationsressourcen, die Sie benötigen, um DSC zu verstehen und zu nutzen.

## Featurebeschreibung
DSC ist eine neue Verwaltungsplattform in Windows PowerShell, die es ermöglicht, Konfigurationsdaten für Softwaredienste bereitzustellen und zu verwalten und die Umgebung zu verwalten, in der diese Dienste ausgeführt werden.

DSC stellt eine Gruppe von Windows PowerShell-Spracherweiterungen, neue Windows PowerShell-Cmdlets und Ressourcen bereit, mit denen Sie deklarativ angeben, wie Ihre Software-Umgebung konfiguriert werden soll. DSC bietet außerdem eine Möglichkeit zum Warten und Verwalten vorhandener Konfigurationen.

## Praktische Anwendung
Es folgen einige Beispielszenarien, in denen Sie integrierte DSC-Ressourcen verwenden können, um eine Gruppe von Computern (auch Zielknoten genannt) automatisiert zu konfigurieren und zu verwalten:

* Aktivieren und Deaktivieren von Serverrollen und Features
* Verwalten von Registrierungseinstellungen
* Verwalten von Dateien und Verzeichnissen
* Starten, Beenden und Verwalten von Prozessen und Diensten
* Verwalten von Gruppen und Benutzerkonten
* Bereitstellen neuer Software
* Verwalten von Umgebungsvariablen
* Ausführen von Windows PowerShell-Skripts
* Korrigieren einer Konfiguration, die vom gewünschten Zustand abweicht
* Ermitteln des tatsächlichen Konfigurationsstatus auf einem bestimmten Knoten

## Wichtige Konzepte
DSC ist eine deklarative Plattform für die Konfiguration, Bereitstellung und Verwaltung von Systemen, die aus drei Hauptkomponenten besteht:

* [Konfigurationen](configurations.md) sind deklarative PowerShell-Skripts, mit denen Instanzen von Ressourcen definiert und konfiguriert werden. Bei Ausführung der Konfiguration wird der gewünschte Zustand entsprechend den Vorgaben in der Konfiguration von DSC eingerichtet. DSC-Konfigurationen sind auch idempotent, d. h. vom lokalen Konfigurations-Manager (LCM) wird weiter sicherstellt, dass Computer entsprechend der Deklaration in der Konfiguration konfiguriert werden.
* Ressourcen sind die zwingend erforderlichen Bausteine von DSC, die geschrieben werden, um die verschiedenen Komponenten eines Teilsystems zu modellieren und die Ablaufsteuerung ihrer sich ändernden Zustände zu implementieren. Sie befinden sich in PowerShell-Modulen und können geschrieben werden, um etwas Allgemeines wie eine Datei oder einen Windows-Prozess oder etwas Spezifisches wie einen IIS-Server oder eine in Azure ausgeführte VM zu modellieren.
* Der lokale Konfigurations-Manager (Local Configuration Manager, LCM) ist das Modul, dass DSC die Interaktion zwischen Ressourcen und Konfigurationen erleichtert. Der LCM fragt das System mithilfe der von Ressourcen implementierten Ablaufsteuerung ab, um sicherzustellen, dass der von einer Konfiguration vorgegebene Zustand beibehalten wird. Ist dies nicht der Fall, verwendete der LCM weitere Logik innerhalb der Ressourcen, um eine Korrektur entsprechend der Deklaration der Konfiguration vorzunehmen. 

DSC bietet auch eine Reihe von neuen Programmiersprachen-Schlüsselworten, Cmdlets und Tools, die das Erstellen von Konfigurationen, Entwickeln von DSC-Ressourcen, Aufrufen von Konfigurationen und Verwalten des LCM ermöglichen. Viele dieser Cmdlets sind in Windows 8.1 als Teil des Moduls „PSDesiredStateConfiguration“ enthalten (einschließlich `Start-DscConfiguration`, `Set-DscLocalConfigurationManager` und `Get-DscResource`). Der (im [PowerShell-Katalog](https://www.powershellgallery.com/packages/xDSCResourceDesigner/) enthaltene) „xDscResourceDesigner“ ist eine Sammlung von Cmdlets, die die Entwicklung von DSC-Ressourcen vereinfachen.

## Weitere Informationen
* [DSC-Konfigurationen](configurations.md)
* [DSC-Ressourcen](resources.md)
* [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md)




<!--HONumber=Oct16_HO1-->


