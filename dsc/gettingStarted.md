---
title: Erste Schritte mit Windows PowerShell DSC
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: c5ee7f7e7678b60700edb1ab1b66139791ea67c6

---

# Erste Schritte mit Windows PowerShell DSC #

In dieser Anleitung wird erläutert, wie PowerShell DSC-Dokumente erstellt und auf Computer angewendet werden. Grundkenntnisse von PowerShell-Cmdlets, Modulen und Funktionen werden vorausgesetzt. 


## Erstellen einer Konfiguration ##

[**Konfigurationen**](https://msdn.microsoft.com/en-us/powershell/dsc/configurations) sind Dokumente, die eine Umgebung beschreiben. Umgebungen bestehen aus **Knoten**, bei denen es sich um virtuelle oder physische Computer handelt. 

Konfigurationen können verschiedene Formen haben. Die einfachste Möglichkeit zum Erstellen einer neuen Konfiguration ist die Erstellung einer PS1-Datei (PowerShell-Skript). Öffnen Sie hierfür den Editor Ihrer Wahl. Die PowerShell ISE ist eine gute Wahl, da sie DSC nativ versteht. Speichern Sie Folgendes in einer PS1-Datei:

```powershell
configuration MyFirstConfiguration
{
    Import-DscResource -Name WindowsFeature

    Node localhost
    {
        WindowsFeature IIS
        {
            Name = "IIS"

        }
        
    }

}
```
## Teile einer Konfiguration ##
**Configuration** ist ein Schlüsselwort, das PowerShell 4.0 hinzugefügt wurde. Es bezeichnet eine spezielle Art von PowerShell-Funktion, die von DSC verwendet wird. In diesem Beispiel heißt die Funktion „myFirstConfiguration“. 

Die nächste Zeile ist eine Importanweisung wie zum Importieren eines Moduls. Sie wird weiter unten besprochen.

„Node“ definiert den Namen des Computers, auf den diese Konfiguration angewendet werden soll. Obwohl diese Konfiguration lokal bearbeitet wird, können Konfigurationen auch für Remoteknoten gelten. 

Knoten können Computernamen oder IP-Adressen haben. Ein einzelnes Konfigurationsdokument kann mehrere Knoten enthalten. Mithilfe von [Konfigurationsdaten](https://msdn.microsoft.com/en-us/powershell/dsc/configdata) können Sie die gleiche Konfiguration auch auf mehrere Knoten anwenden. In diesem Fall heißt der Knoten „localhost“, was sich auf den lokalen Computer bezieht. 

Das nächste Element ist eine [**Ressource**](https://msdn.microsoft.com/en-us/powershell/dsc/resources). Ressourcen sind Bausteine von Konfigurationen. Jede Ressource ist ein Modul, das die Implementierungslogik eines einzelnen Aspekts eines Computers definiert. Durch Ausführen von **Get-DscResource** in PowerShell können Sie alle Ressourcen auf Ihrem Computer anzeigen. Ressourcen müssen auf dem lokalen Computer vorhanden sein und importiert werden, ehe sie in einer Konfiguration mit **Import-DscResource** in der zweiten Zeile dieser Konfiguration verwendet werden können. 

**Umsetzung einer Konfigurations**

Wenn das obige Skript gespeichert und ausgeführt wird, erfolgt keine Ausgabe. Das liegt daran, dass eine Konfiguration bloß eine Funktion ist und dass das obige Skript die Funktion zwar definiert, sie aber noch nicht ausgeführt hat. Nachdem die Funktion definiert wurde, muss sie aufgerufen werden:
```powershell
myFirstConfiguration
```

Bei ihrer Ausführung überprüfen Konfigurationsfunktionen, ob die Konfiguration gültig ist. Sie darf keine Syntaxfehler aufweisen, für Ressourcen müssen alle Pflichtparameter definiert sein, und alle Ressourcen müssen vor der Ausführung importiert werden.

Nachdem die Konfiguration ausgeführt wurde, wird ein Ordner mit dem Namen der Konfiguration und einer **MOF-Datei** für jeden Knoten in der Konfiguration erstellt. Die MOF-Datei ist ein auf Standards basierendes Verwaltungsformat, das von PowerShell DSC für die Kommunikation über das Netzwerk verwendet wird.

So wenden Sie eine Konfiguration an:
```powershell
Start-DscConfiguration -Path ./myFirstConfiguration
```
Dadurch wird ein PowerShell-Auftrag erstellt, der auf den Knoten in der Konfiguration ausgeführt wird, um diese zu konfigurieren. Rufen Sie „-Wait“ auf, um die Ausgabe des Auftrags anzuzeigen. 
```powershell
Start-DscConfiguration -Path ./myFirstConfiguration -Wait
```




<!--HONumber=Oct16_HO1-->


