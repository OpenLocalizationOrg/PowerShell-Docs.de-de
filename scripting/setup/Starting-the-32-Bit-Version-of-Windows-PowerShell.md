---
title: Starten der 32-Bit-Version von Windows PowerShell
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 12b31890-2609-4a76-8c24-0ebe78084f50
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 310f898a471ca60da4d9ebe5234ee86a636e44b5

---

# Starten der 32-Bit-Version von Windows PowerShell
Bei der Installation von Windows PowerShell auf einem 64-Bit-Computer wird **Windows PowerShell (x86)**, eine 32-Bit-Version von Windows PowerShell, zusätzlich zur 64-Bit-Version installiert. Wenn Sie Windows PowerShell ausführen, wird standardmäßig die 64-Bit-Version ausgeführt.

Es kann jedoch möglicherweise erforderlich sein, **Windows PowerShell (x86)** auszuführen, z.B. wenn Sie ein Modul verwenden, das die 32-Bit-Version verlangt, oder Sie eine Remoteverbindung mit einem 32-Bit-Computer herstellen.

Wählen Sie eines der folgenden Verfahren, um eine 32-Bit-Version von Windows PowerShell zu starten.

#### Windows Server® 2012 R2

-   Geben Sie im** **Startbildschirm **Windows PowerShell (x86)** ein. Klicken Sie auf die Kachel **Windows PowerShell x86**.

-   Wählen Sie im **Server-Manager** im Menü **Extras** den Eintrag **Windows PowerShell (x86)** aus.

-   Bewegen Sie auf dem Desktop den Cursor in die rechte obere Ecke, klicken Sie auf **Suchen**, geben Sie **PowerShell x86** ein, und klicken Sie dann auf **Windows PowerShell (x86)**.

-   Geben Sie über die Befehlszeile ein: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### Windows Server 2012

-   Klicken Sie auf **Start**, geben Sie **PowerShell** ein, und klicken Sie dann auf **Windows PowerShell (x86)**.

-   Wählen Sie im **Server-Manager** im Menü **Extras** den Eintrag **Windows PowerShell (x86)** aus.

-   Bewegen Sie auf dem Desktop den Cursor in die rechte obere Ecke, klicken Sie auf **Suchen**, geben Sie **PowerShell** ein, und klicken Sie dann auf **Windows PowerShell (x86)**.

-   Geben Sie über die Befehlszeile ein: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### Windows 8.1

-   Geben Sie im** **Startbildschirm **Windows PowerShell (x86)** ein. Klicken Sie auf die Kachel **Windows PowerShell x86**.

-   Falls Sie die [Remoteserver-Verwaltungstools](http://go.microsoft.com/fwlink/?LinkID=304145) für Windows 8.1 ausführen, können Sie Windows PowerShell x86 auch im Server-Manager-Menü **Extras** öffnen. Wählen Sie **Windows PowerShell (x86)** aus.

-   Bewegen Sie auf dem Desktop den Cursor in die rechte obere Ecke, klicken Sie auf **Suchen**, geben Sie **PowerShell x86** ein, und klicken Sie dann auf **Windows PowerShell (x86)**.
   
-   Geben Sie über die Befehlszeile ein: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### Windows 8

-   Bewegen Sie auf dem Bildschirm **Start** den Cursor in die rechte obere Ecke, klicken Sie auf **Einstellungen**, dann auf **Kacheln**, und bewegen Sie den Schieberegler **Verwaltungstools anzeigen** auf „Ja“. Geben Sie dann **PowerShell** ein, und klicken Sie auf **Windows PowerShell (x86)**.

-   Falls Sie die [Remoteserver-Verwaltungstools](http://www.microsoft.com/download/details.aspx?id=28972) für Windows 8 ausführen, können Sie Windows PowerShell x86 auch im Server-Manager-Menü **Extras** öffnen. Wählen Sie **Windows PowerShell (x86)** aus.

-   Geben Sie auf dem Bildschirm **Start** oder dem Desktop **PowerShell (x86)** ein, und klicken Sie dann auf **Windows PowerShell (x86)**.

-   Geben Sie über die Befehlszeile ein: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`



<!--HONumber=Oct16_HO1-->


