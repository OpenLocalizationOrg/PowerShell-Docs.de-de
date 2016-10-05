---
title: Installieren des Windows PowerShell 2.0-Moduls
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 82928f2b-f96a-4ae6-a0d0-6e7b181da308
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: b6189e43fc9579a29d598deb705bb8e4e32e4a4f

---

# Installieren des Windows PowerShell 2.0-Moduls
In diesem Thema wird die Installation von Windows PowerShell 2.0 Engine erläutert.

Windows PowerShell 3.0 ist mit Windows PowerShell 2.0 abwärtskompatibel. Cmdlets, Anbieter, Snap-Ins, Module und Skripts, die für Windows PowerShell 2.0 geschrieben wurden, werden unverändert in Windows PowerShell 3.0 und Windows PowerShell 4.0 ausgeführt. Doch aufgrund einer Änderung der Richtlinie für die Laufzeitaktivierung in Microsoft .NET Framework 4 können Windows PowerShell-Hostprogramme, die für Windows PowerShell 2.0 geschrieben und mit Common Language Runtime (CLR) 2.0 kompiliert wurden, nicht unverändert in späteren Versionen von Windows PowerShell ausgeführt werden, die mit CLR 4.0 kompiliert wurden.

Die Module Windows PowerShell 2.0, Windows PowerShell 3.0 und Windows PowerShell 4.0 können parallel ausgeführt werden, um die Abwärtskompatibilität mit Befehlen und Hostprogrammen zu erhalten, die von diesen Änderungen betroffen sind. Außerdem ist Windows PowerShell 2.0 Engine in Windows Server 2012 R2, Windows 8.1, Windows 8, Windows Server 2012 und Windows Management Framework 3.0 enthalten. Windows PowerShell 2.0 Engine soll ausschließlich verwendet werden, wenn ein vorhandenes Skript oder Hostprogramm nicht ausgeführt werden kann, da es nicht mit Windows PowerShell 3.0, Windows PowerShell 4.0 oder Microsoft .NET Framework 4 kompatibel ist. Solche Fälle sind allerdings eher selten.

Windows PowerShell 2.0 Engine ist eine optionale Funktion von Windows Server 2012 R2, Windows 8.1, Windows® 8 und Windows-Server® 2012. In früheren Versionen von Windows ersetzt die Installation von Windows PowerShell 3.0 komplett die Installation von Windows PowerShell 2.0 im Installationsverzeichnis von Windows PowerShell, wenn Sie Windows Management Framework 3.0 installieren. Windows PowerShell 2.0 Engine wird jedoch beibehalten.

Informationen zum Starten des Windows PowerShell 2.0-Moduls finden Sie unter [Starten des Windows PowerShell 2.0-Moduls](Starting-the-Windows-PowerShell-2.0-Engine.md).

## Unter Windows 8.1 und Windows 8
Unter Windows 8.1 und Windows 8 ist die Funktion Windows PowerShell 2.0 Engine standardmäßig aktiviert. Um es verwenden zu können, müssen Sie jedoch die Option für Microsoft .NET Framework 3.5 aktivieren, das erforderlich ist. In diesem Abschnitt wird auch erläutert, wie die Funktion Windows PowerShell 2.0 Engine aktiviert und deaktiviert wird.

#### So aktivieren Sie .NET Framework 3.5

1.  Geben Sie **Windows-Features** auf dem** **Startbildschirm ein.

2.  Klicken Sie auf der Leiste **Apps** auf **Einstellungen**, und klicken Sie dann auf **Windows-Features aktivieren oder deaktivieren**.

3.  Klicken Sie im Feld **Windows-Features** auf **.NET Framework 3.5 (enthält .NET 2.0 und 3.0)**, um es auszuwählen.

    Wenn Sie die Option **NET Framework 3.5 (enthält .NET 2.0 und 3.0)** auswählen, wird das Feld aufgefüllt, um anzugeben, dass nur ein Teil des Features ausgewählt ist. Dies ist jedoch für Windows PowerShell 2.0 Engine ausreichend.

#### Aktivieren und Deaktivieren von Windows PowerShell 2.0 Engine

1.  Geben Sie **Windows-Features** auf dem** **Startbildschirm ein.

2.  Klicken Sie auf der Leiste **Apps** auf **Einstellungen**, und klicken Sie dann auf **Windows-Features aktivieren oder deaktivieren**.

3.  Erweitern Sie im Feld **Windows-Features** den Knoten **Windows PowerShell 2.0**, und klicken Sie auf das Feld **Windows PowerShell 2.0 Engine**, um es zu aktivieren oder zu deaktivieren.

## Unter Windows Server 2012 R2 und Windows Server 2012
Mithilfe der folgenden Verfahren können Sie Windows PowerShell 2.0 Engine und Microsoft .NET Framework 3.5-Features hinzufügen. Windows PowerShell 2.0 Engine erfordert mindestens Microsoft .NET Framework 2.0.50727. Diese Anforderung wird durch Microsoft .NET Framework 3.5 erfüllt.

#### So fügen Sie das Feature „.NET Framework 3.5“ hinzu

1.  Klicken Sie im **Server-Manager** im Menü **Verwalten** auf **Rollen und Features hinzufügen**.

    Oder klicken Sie im **Server-Manager** auf **Alle Server**, klicken Sie mit der rechten Maustaste auf einen Servernamen, und wählen Sie anschließend **Rollen und Features hinzufügen** aus.

2.  Wählen Sie auf der Seite **Installationstyp auswählen** **Rollenbasierte oder featurebasierte Installation** aus.

3.  Klappen Sie auf der Seite **Features** den Knoten **Features von .NET Framework 3.5** auf, und wählen **NET Framework 3.5 (enthält .NET 2.0 und 3.0)** aus.

    Die anderen Optionen unter diesem Knoten sind für Windows PowerShell 2.0 Engine nicht erforderlich.

#### So fügen Sie das Feature „Windows PowerShell 2.0 Engine“ hinzu

-   Klicken Sie im **Server-Manager** im Menü **Verwalten** auf **Rollen und Features hinzufügen**.

    Klicken Sie im **Server-Manager** auf **Alle Server**, klicken Sie mit der rechten Maustaste auf einen Servernamen, und wählen Sie anschließend **Rollen und Features hinzufügen** aus.

-   Wählen Sie auf der Seite **Installationstyp auswählen** **Rollenbasierte oder featurebasierte Installation** aus.

-   Auf der Seite **Features**, erweitern Sie den Knoten **Windows PowerShell (installiert)**, und wählen Sie **Windows PowerShell 2.0 Engine** aus.

Informationen zum Starten des Windows PowerShell 2.0-Moduls finden Sie unter [Starten des Windows PowerShell 2.0-Moduls](Starting-the-Windows-PowerShell-2.0-Engine.md).

## Auf älteren Systemen
Das [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881)-Paket, das Windows PowerShell 4.0 auf Windows 7, Windows Server 2008 R2 und Windows Server 2012 installiert, enthält Windows PowerShell 2.0 Engine. Windows PowerShell 2.0 Engine ist aktiviert und einsatzbereit, falls erforderlich, ohne zusätzliche Installation, Einrichtung und Konfiguration.

Das Windows Management Framework 3.0-Paket, das Windows PowerShell 3.0 auf Windows 7, Windows Server 2008 R2 und Windows Server 2008 installiert, enthält Windows PowerShell 2.0 Engine. Windows PowerShell 2.0 Engine ist aktiviert und einsatzbereit, falls erforderlich, ohne zusätzliche Installation, Einrichtung und Konfiguration.

## Weitere Informationen
[Windows PowerShell-Systemanforderungen](Windows-PowerShell-System-Requirements.md)
[Installieren von Windows PowerShell](Installing-Windows-PowerShell.md)
[Starten von Windows PowerShell [ps]](https://technet.microsoft.com/en-us/library/8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)
[Starten des Windows PowerShell 2.0-Moduls](Starting-the-Windows-PowerShell-2.0-Engine.md)




<!--HONumber=Oct16_HO1-->


