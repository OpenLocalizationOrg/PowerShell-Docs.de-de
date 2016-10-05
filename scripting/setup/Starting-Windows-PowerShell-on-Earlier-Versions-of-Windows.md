---
title: "Starten von Windows PowerShell unter früheren Versionen von Windows"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 57125436-3d1e-4e7f-b5c4-8f0ecb49d642
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 6cdb6bb5d901c9bc7d2b7f5051e372337bbb69f9

---

# Starten von Windows PowerShell unter früheren Versionen von Windows
In diesem Abschnitt wird das Starten von Windows PowerShell und Windows PowerShell Integrated Scripting Environment (ISE) unter Windows® 7, Windows Server® 2008 R2 und Windows Server 2008 erläutert. Außerdem wird beschrieben, wie das optionale Feature für Windows PowerShell ISE in Windows PowerShell 2.0 in Windows Server® 2008 R2 und Windows Server 2008 aktiviert wird.

Zum Installieren von Windows PowerShell 4.0 in unterstützten Systemen müssen Sie [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) herunterladen und installieren. Weitere Informationen finden Sie unter [Installieren von Windows PowerShell](Installing-Windows-PowerShell.md).

Zum Installieren von Windows PowerShell 3.0 in unterstützten Systemen müssen Sie [Windows Management Framework 3.0](http://go.microsoft.com/fwlink/?LinkID=240290) herunterladen und installieren. Weitere Informationen finden Sie unter [Installieren von Windows PowerShell](Installing-Windows-PowerShell.md).

## Starten von Windows PowerShell unter früheren Versionen von Windows
Mit den folgenden Methoden können Sie die installierte Version von Windows PowerShell 3.0 oder Windows PowerShell 4.0 installieren, sofern zutreffend.

#### Über das Startmenü

-   Klicken Sie auf **Start**, geben Sie **PowerShell** ein, und klicken Sie dann auf **Windows PowerShell**.

-   Klicken Sie im Menü ** ****Start** auf **Alle Programme**, danach auf **Zubehör**, anschließend auf den Ordner **Windows PowerShell** und schließlich auf **Windows PowerShell**.

#### An der Eingabeaufforderung

-   Geben Sie zum Starten von Windows PowerShell in Cmd.exe, Windows PowerShell oder Windows PowerShell ISE Folgendes ein:

    ```
    PowerShell
    ```

    Sie können auch die Parameter des Programms „PowerShell.exe“ verwenden, um die Sitzung anzupassen. Weitere Informationen finden Sie unter [PowerShell.exe – Befehlszeilenhilfe](../core-powershell/console/PowerShell.exe-Command-Line-Help.md).

#### Mit administrativen Berechtigungen (Als Administrator ausführen)

1.  Klicken Sie auf **Start**, geben Sie **PowerShell** ein, klicken Sie mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie dann auf **Als Administrator ausführen**.

## Starten von Windows PowerShell ISE unter früheren Versionen von Windows
Verwenden Sie eine der folgenden Methoden, um Windows PowerShell ISE zu starten:

#### Über das Startmenü

-   Klicken Sie auf **Start**, geben Sie **ISE** ein, und klicken Sie dann auf **Windows PowerShell ISE**.

-   Klicken Sie im Menü ** ****Start** auf **Alle Programme**, danach auf **Zubehör**, anschließend auf den Ordner **Windows PowerShell** und schließlich auf **Windows PowerShell ISE**.

#### An der Eingabeaufforderung

-   Geben Sie zum Starten von Windows PowerShell in Cmd.exe, Windows PowerShell oder Windows PowerShell ISE Folgendes ein:

    ```
    PowerShell_ISE
    ```

    oder

    ```
    ISE
    ```

#### Mit administrativen Berechtigungen (Als Administrator ausführen)

1.  Klicken Sie auf **Start**, geben Sie **ISE** ein, klicken Sie mit der rechten Maustaste auf **Windows PowerShell ISE**, und klicken Sie dann auf **Als Administrator ausführen**.

## Aktivieren von Windows PowerShell ISE unter früheren Versionen von Windows
In Windows PowerShell 4.0 und Windows PowerShell 3.0 ist Windows PowerShell ISE standardmäßig für alle Versionen von Windows aktiviert. Falls es nicht bereits aktiviert ist, erfolgt die Aktivierung mithilfe von Windows Management Framework 4.0 oder Windows Management Framework 3.0.

In Windows PowerShell 2.0 ist Windows PowerShell ISE standardmäßig für Windows 7 aktiviert. Allerdings ist es in Windows Server 2008 R2 und Windows Server 2008 ein optionales Feature.

Gehen Sie wie folgt vor, um Windows PowerShell ISE in Windows PowerShell 2.0 unter Windows Server 2008 R2 oder Windows Server 2008 zu aktivieren.

#### So aktivieren Sie Windows PowerShell Integrated Scripting Environment (ISE)

1.  Starten Sie den Server-Manager.

2.  Klicken Sie auf **Features** und dann auf **Features hinzufügen**.

3.  Klicken Sie unter „Features auswählen“ auf „Windows PowerShell Integrated Scripting Environment (ISE)“.




<!--HONumber=Oct16_HO1-->


