---
title: Deinstallieren von Windows PowerShell Web Access
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d54139714552943901f565a8525bb478ed308f09

---

#  Deinstallieren von Windows PowerShell Web Access

Aktualisiert: 24. Juni 2013

Gilt für: Windows Server 2012 R2, Windows Server 2012

Führen Sie die Schritte in diesem Thema aus, um die Windows PowerShell Web Access-Website und die Anwendung vom Gatewayserver zu löschen, auf dem entweder Windows Server 2012 R2 oder Windows Server 2012 ausgeführt wird. Benachrichtigen Sie zuvor die Benutzer der webbasierten Konsole, die Sie von der Website entfernen möchten.

<a href="" id="BKMK_uninstall"></a>

------------------------------------------------------------------------

Führen Sie vor dem Deinstallieren von Windows PowerShell Web Access vom Gatewayserver entweder das Cmdlet <span class="code">Uninstall-PswaWebApplication</span> aus, um die Website und die Windows PowerShell Web Access-Webanwendungen zu entfernen, oder verwenden Sie das IIS-Manager-Verfahren unter [So löschen Sie die Windows PowerShell Web Access-Website und -Webanwendungen mit IIS Manager](#BKMK_delsite).

Bei der Deinstallation von Windows PowerShell Web Access werden IIS oder andere Features, die automatisch installiert wurden, nicht deinstalliert, weil diese von Windows PowerShell Web Access zur Ausführung benötigt werden. Beim Deinstallationsvorgang bleiben die Features unangetastet, mit denen für Windows PowerShell Web Access eine Abhängigkeit besteht. Sie können diese Features bei Bedarf separat deinstallieren.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Empfohlene (schnellen)-Deinstallation</span></a>
<a href="/en-us/library/dn282396(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Mit den in diesem Abschnitt beschriebenen Verfahren können Sie die Windows PowerShell Web Access-Webanwendung und die Windows PowerShell Web Access-Funktion mit Windows PowerShell-Cmdlets deinstallieren.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 1: Löschen Sie die Webanwendung</span></a>

------------------------------------------------------------------------

Wenn Sie Ihre eigenen, benutzerdefinierten Websitenamen angegeben haben, fügen Sie dem Befehl den <span class="code">WebsiteName</span>-Parameter hinzu, und geben Sie den Namen der Website an. Wenn Sie eine benutzerdefinierte Webanwendung (und nicht die Standardanwendung **pswa**) verwendet haben, fügen Sie dem Befehl den <span class="code">WebApplicationName</span>-Parameter hinzu, und geben Sie den Namen der Webanwendung an.

#### So löschen Sie die Website und Webanwendungen mithilfe des Uninstall-PswaWebApplication-Cmdlets

1.  Öffnen Sie eine Windows PowerShell-Sitzung, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste auf der Taskleiste auf **Windows PowerShell**.

    -   Klicken Sie auf der Windows-Startseite** **auf **Windows PowerShell**.

2.  Geben Sie **Uninstall-PswaWebApplication** ein, und drücken Sie dann die **EINGABETASTE**.

3.  Fügen Sie dem Cmdlet den <span class="code">DeleteTestCertificate</span>-Parameter hinzu, falls Sie ein Testzertifikat verwenden. Dies ist im folgenden Beispiel dargestellt.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_28147344-ab2f-49e7-b1c2-6dbe649d4366'); "Copy to clipboard.")

        Uninstall-PswaWebApplication -DeleteTestCertificate

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 2: Deinstallieren Sie Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### So deinstallieren Sie Windows PowerShell Web Access mithilfe von Windows PowerShell-Cmdlets

1.  Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Benutzerrechten, indem Sie einen der folgenden Schritte durchführen. Wenn bereits eine Sitzung geöffnet ist, fahren Sie mit dem nächsten Schritt fort.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste in der Taskleiste auf **Windows PowerShell** und anschließend auf **Als Administrator ausführen**.

    -   Klicken Sie auf dem Windows-**Startbildschirm** mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie anschließend auf **Als Administrator ausführen**.

2.  Geben Sie Folgendes ein, und drücken Sie dann die **EINGABETASTE**, wobei *computer_name* für einen Remoteserver steht, von dem Sie Windows PowerShell Web Access entfernen möchten. Mit dem Parameter <span class="code">–Restart</span> werden Zielserver automatisch neu gestartet, falls dies für die Entfernung erforderlich ist.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_7b534520-f292-471f-89e3-a1079c03e369'); "Copy to clipboard.")

        Uninstall-WindowsFeature –Name WindowsPowerShellWebAccess -ComputerName <computer_name> -Restart

    Zum Entfernen von Rollen oder Features von einer Offline-VHD müssen Sie sowohl den <span class="code">ComputerName</span>-Parameter als auch den <span class="code">VHD</span>-Parameter hinzufügen. Der <span class="code">ComputerName</span>-Parameter enthält den Namen des Servers, auf dem die VHD eingebunden werden soll. Der <span class="code">VHD</span>-Parameter enthält den Pfad zur VHD-Datei auf dem angegebenen Server.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_5d8f91ee-b91a-4653-b7df-e745187fd72d'); "Copy to clipboard.")

        Uninstall-WindowsFeature –Name WindowsPowerShellWebAccess –VHD <path> -ComputerName <computer_name> -Restart

3.  Vergewissern Sie sich nach Abschluss der Entfernung, ob Windows PowerShell Web Access wirklich entfernt wurde. Öffnen Sie dazu im Server-Manager die Seite **Alle Server**, wählen Sie einen Server aus, von dem Sie das Feature entfernt haben, und zeigen Sie auf der Seite für den ausgewählten Server die Kachel **Rollen und Features** an. Sie können auch das Cmdlet <span class="code">Get-WindowsFeature</span> für den ausgewählten Server (Get-WindowsFeature -ComputerName &lt;*Computername*&gt;) ausführen, um eine Liste der Rollen und Features anzuzeigen, die auf dem Server installiert sind.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Benutzerdefinierte Deinstallation</span></a>
<a href="/en-us/library/dn282396(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Mit den in diesem Abschnitt beschriebenen Verfahren können Sie die Windows PowerShell Web Access-Webanwendung und die Windows PowerShell Web Access-Funktion mit dem Assistenten zum Entfernen von Rollen und Features im Server-Manager und der IIS Manager-Konsole deinstallieren.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 1: Löschen Sie die Webanwendung</span></a>

------------------------------------------------------------------------

#### So löschen Sie die Windows PowerShell Web Access-Website und -Webanwendungen mithilfe des IIS-Managers

1.  Öffnen Sie die IIS-Manager-Konsole, indem Sie eine der folgenden Aktionen ausführen: Wenn die Konsole bereits geöffnet ist, fahren Sie mit dem nächsten Schritt fort.

    -   Starten Sie auf dem Windows-Desktop den Server-Manager, indem Sie in der Windows-Taskleiste auf **Server-Manager** klicken. Klicken Sie im Menü **Tools** im Server-Manager auf **Internetinformationsdienste-Manager (IIS)**.

    -   Geben Sie auf dem Windows-**Startbildschirm** einen beliebigen Teil des Namens **Internetinformationsdienste-Manager (IIS)** ein. Klicken Sie auf die Verknüpfung, wenn diese in den Ergebnissen unter **Apps** angezeigt wird.

2.  Wählen Sie im IIS-Manager-Strukturbereich die Website aus, auf der die Windows PowerShell Web Access-Webanwendung ausgeführt wird.

3.  Klicken Sie im **Aktionsbereich** unter **Website verwalten** auf **Beenden**.

4.  Klicken Sie im Strukturbereich mit der rechten Maustaste auf die Webanwendung der Website, von der die Windows PowerShell Web Access-Webanwendung ausgeführt wird, und klicken Sie dann auf **Entfernen**.

5.  Wählen Sie im Strukturbereich die Option **Anwendungspools** und dann den Windows PowerShell Web Access-Anwendungsordner aus, klicken Sie im **Aktionsbereich** auf **Beenden**, und klicken Sie im Inhaltsbereich dann auf **Entfernen**.

6.  Schließen Sie den IIS-Manager.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Hinweis: </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Das Zertifikat wird während der Deinstallation nicht gelöscht. Wenn Sie ein selbstsigniertes Zertifikat erstellt oder ein Testzertifikat verwendet haben und dieses Zertifikat nun entfernen möchten, müssen Sie es im IIS-Manager löschen.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 2: Deinstallieren Sie Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### So deinstallieren Sie Windows PowerShell Web Access mithilfe des Assistenten zum Entfernen von Rollen und Features

1.  Wenn der Server-Manager bereits geöffnet ist, fahren Sie mit dem nächsten Schritt fort. Ist der Server-Manager noch nicht geöffnet, öffnen Sie ihn mit einer der folgenden Aktionen.

    -   Starten Sie auf dem Windows-Desktop den Server-Manager, indem Sie in der Windows-Taskleiste auf **Server-Manager** klicken.

    -   Klicken Sie auf der Windows-Startseite** **auf **Server-Manager**.

2.  Klicken Sie im Menü **Verwalten** auf **Rollen und Funktionen entfernen**.

3.  Wählen Sie auf der Seite **Zielserver auswählen** den Server oder die Offline-VHD aus, von dem bzw. der Sie das Feature entfernen möchten. Um eine Offline-VHD auszuwählen, müssen Sie zuerst den Server auswählen, auf dem die VHD eingebunden werden soll. Wählen Sie anschließend die VHD-Datei aus. Klicken Sie nach dem Auswählen des Zielservers auf **Weiter**.

4.  Klicken Sie erneut auf **Weiter**, um auf die Seite **Features entfernen** zu wechseln.

5.  Deaktivieren Sie das Kontrollkästchen unter **Windows PowerShell Web Access**, und klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Entfernungsauswahl bestätigen** auf **Entfernen**.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Siehe auch</span></a>
<a href="/en-us/library/dn282396(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Installieren und Verwenden von Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx)
[Hilfe zum IIS-Manager 7.0](https://technet.microsoft.com/library/cc732664.aspx)

<span>Anzeigen:</span> geschützten geerbt

<span class="stdr-votetitle">War diese Seite hilfreich?</span>
Ja Nein

Zusätzliches Feedback?

<span class="stdr-count"><span class="stdr-charcnt">1500</span> Verbleibende Zeichen</span> Senden diesen Schritt überspringen

<span class="stdr-thankyou">Danke!</span> <span class="stdr-appreciate">Wir schätzen Ihr Feedback.</span>

[Verwalten Sie Ihr Profil](https://social.technet.microsoft.com/profile)

|

<a href="javascript:void(0)" id="SiteFeedbackLinkOpener"><span id="FeedbackButton" class="FeedbackButton clip20x21"> <img src="https://i-technet.sec.s-msft.com/Areas/Epx/Content/Images/ImageSprite.png?v=635975720914499532" alt="Site Feedback" id="feedBackImg" class="cl_footer_feedback_icon" /> </span> Site Feedback</a> Site Feedback

<a href="javascript:void(0)" id="SiteFeedbackLinkCloser">x</a>

Teilen Sie uns Ihre Erfahrungen mit.

Wurde die Seite schnell geladen?

<span> Yes<span> </span></span> <span> No<span> </span></span>

Gefällt Ihnen das Design der Seite?

<span> Yes<span> </span></span> <span> No<span> </span></span>

Erzählen Sie uns mehr

-   [Flash-Newsletter](https://technet.microsoft.com/cc543196.aspx)
-   |
-   [Wenden Sie sich an uns](https://technet.microsoft.com/cc512759.aspx)
-   |
-   [Datenschutzrichtlinie](https://privacy.microsoft.com/privacystatement)
-   |
-   [Rechtliche Hinweise](https://technet.microsoft.com/cc300389.aspx)
-   |
-   [Marken](https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/)
-   |

© 2016 Microsoft

© 2016 Microsoft

Die Lizenz für Drittanbieterskripts oder Code, die mit dieser Website verlinkt oder über Verweise verbunden sind, wird Ihnen von den Codeeigentümern erteilt, nicht von Microsoft. Die ASP.NET Ajax CDN-Nutzungsbedingungen finden Sie unter http://www.asp.net/ajaxlibrary/CDN.ashx.
<img src="https://m.webtrends.com/dcsjwb9vb00000c932fd0rjc7_5p3t/njs.gif?dcsuri=/nojavascript&amp;WT.js=No" alt="DCSIMG" id="Img1" width="1" height="1" />




<!--HONumber=Oct16_HO1-->


