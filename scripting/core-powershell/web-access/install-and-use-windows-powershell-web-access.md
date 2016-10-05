---
title: Installieren und Verwenden von Windows PowerShell Web Access
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 75e9b6525d74a8bc632b9fe2e64183717f59776f

---

#  Installieren und Verwenden von Windows PowerShell Web Access

Aktualisiert: 5. November 2013

Gilt für: Windows Server 2012 R2, Windows Server 2012

Windows PowerShell® Web Access (eingeführt in Windows Server® 2012) fungiert als Windows PowerShell-Gateway, indem eine webbasierte Windows PowerShell-Konsole bereitgestellt wird, die auf einen Remotecomputer ausgerichtet ist. Dadurch können IT-Spezialisten Windows PowerShell-Befehle und -Skripts über eine Windows PowerShell-Konsole in einem Webbrowser ausführen, ohne dass Windows PowerShell, Remoteverwaltungssoftware oder Browser-Plugins auf dem Clientgerät installiert sein müssen. Zur Ausführung der webbasierten Windows PowerShell-Konsole ist lediglich ein ordnungsgemäß konfiguriertes Windows PowerShell Web Access-Gateway und ein Browser auf dem Clientgerät erforderlich, der JavaScript® unterstützt und Cookies akzeptiert.

Beispiele für Clientgeräte umfassen Laptops, privat genutzte Personalcomputer, geliehene Computer, Tablet PCs, Webkiosks, Computer, auf denen kein Windows-basiertes Betriebssystem ausgeführt wird, sowie Browser auf Handys. IT-Professionals können über Geräte, die Zugriff auf eine Internetverbindung und einen Webbrowser haben, wichtige Verwaltungsaufgaben auf Windows-basierten Remoteservern ausführen.

Nach der erfolgreichen Einrichtung und Konfiguration des Gateways können Benutzer mithilfe eines Webbrowsers auf eine Windows PowerShell-Konsole zugreifen. Nachdem Benutzer die geschützte Windows PowerShell Web Access-Website geöffnet haben, können sie nach der erfolgreichen Authentifizierung eine webbasierte Windows PowerShell-Konsole ausführen.

Die Einrichtung und Konfiguration von Windows PowerShell Web Access umfasst drei Schritte:

1.  Installieren von Windows PowerShell Web Access

2.  Konfigurieren des Gateways

3.  Konfigurieren von Autorisierungsregeln, die Benutzern den Zugriff auf die webbasierte Windows PowerShell-Konsole ermöglichen

Vor der Installation und Konfiguration von Windows PowerShell Web Access sollten Sie das gesamte Handbuch lesen, das Anweisungen zum Installieren, Sichern und Deinstallieren von Windows PowerShell Web Access enthält. Im Thema [Verwendung der webbasierten Windows PowerShell-Konsole](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx) wird beschrieben, wie Benutzer sich bei der webbasierten Konsole anmelden, und es werden einige Beschränkungen und Unterschiede zwischen der webbasierten Windows PowerShell-Konsole und der **powershell.exe**-Konsole erläutert. Endbenutzer der webbasierten Konsole sollten sich das Thema [Verwendung der webbasierten Windows PowerShell-Konsole](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx) durchlesen, sie müssen den Rest dieses Handbuchs jedoch nicht lesen.

Dieses Thema enthält keine ausführliche Anleitung zum Webserverbetrieb (IIS), sondern es werden nur die Schritte beschrieben, die zum Konfigurieren des Windows PowerShell Web Access-Gateways erforderlich sind. Weitere Informationen zum Konfigurieren und Schützen von Websites in IIS finden Sie in den IIS-Dokumentationsressourcen im Abschnitt "Siehe auch".

Das folgende Diagramm zeigt die Funktionsweise von Windows PowerShell Web Access.

<span><img src="https://i-technet.sec.s-msft.com/dynimg/IC564303.jpeg" title="Windows PowerShell Web Access diagram" alt="Windows PowerShell Web Access diagram" id="ee15fa8f-ce13-49e5-933d-514f6d60a2b1" /></span>

Inhalte dieses Themas:

-   [Für den Betrieb von Windows PowerShell Web Access](#BKMK_reqs)

-   [Unterstützung für Browser und Client-Geräte](#BKMK_browser)

-   [Empfohlene (schnellen)-Bereitstellung](#BKMK_recm)

-   [Benutzerdefinierte Bereitstellung](#BKMK_custom)

-   [Konfigurieren eines echten Zertifikats](#BKMK_configcert)

<a href="" id="BKMK_reqs"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Für den Betrieb von Windows PowerShell Web Access</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_0" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Windows PowerShell Web Access erfordert die Ausführung von Webserver (IIS), .NET Framework 4.5 und Windows PowerShell 3.0 oder Windows PowerShell 4.0 auf dem Server, auf dem Sie das Gateway ausführen möchten. Sie können Windows PowerShell Web Access auf einem Server mit Windows Server 2012 R2 oder Windows Server 2012 installieren, indem Sie entweder den Assistenten zum Hinzufügen von Rollen und Features im Server-Manager oder Windows PowerShell-Bereitstellungs-Cmdlets für den Server-Manager verwenden. Wenn Sie Windows PowerShell Web Access mithilfe des Server-Managers oder seiner Bereitstellungs-Cmdlets installieren, werden die erforderlichen Rollen und Features im Rahmen des Installationsvorgangs automatisch hinzugefügt.

Mithilfe von Windows PowerShell Web Access können Remotebenutzer auf Computer der Organisation über Windows PowerShell in einem Webbrowser zugreifen. Obwohl Windows PowerShell Web Access ein benutzerfreundliches und leistungsfähiges Verwaltungstool ist, ist der webbasierte Zugriff mit Sicherheitsrisiken verbunden. Daher sollte eine möglichst sichere Konfiguration gewählt werden. Administratoren, die das Windows PowerShell Web Access-Gateway konfigurieren, wird die Verwendung der verfügbaren Sicherheitsebenen empfohlen. Damit sind die auf Cmdlets basierenden Autorisierungsregeln von Windows PowerShell Web Access sowie die Sicherheitsebenen gemeint, die im Webserver (IIS) und in Drittanbieteranwendungen verfügbar sind. In dieser Dokumentation wird sowohl auf nicht sichere Konfigurationen, die nur für Testumgebungen empfohlen werden, als auch auf Konfigurationen eingegangen, die für sichere Bereitstellungen empfohlen werden.

<a href="" id="BKMK_browser"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Unterstützung für Browser und Client-Geräte</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Windows PowerShell Web Access unterstützt die folgenden Internetbrowser. Obwohl es keine offizielle Unterstützung für mobile Browser gibt, kann die webbasierte Windows PowerShell-Konsole in vielen dieser Browser ausgeführt werden. Andere Browser, die Cookies zulassen, JavaScript ausführen und HTTPS-Websites ausführen, sollten ebenfalls funktionieren. Offizielle Tests wurden jedoch nicht durchgeführt.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Unterstützte desktopcomputerbrowser</span></a>

------------------------------------------------------------------------

-   Windows® Internet Explorer® für Microsoft Windows® 8.0, 9.0, 10.0 und 11.0

-   Mozilla Firefox® 10.0.2

-   Google Chrome™ 17.0.963.56m für Windows

-   Apple Safari® 5.1.2 für Windows

-   Apple Safari 5.1.2 für Mac OS®

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Mobile Geräte minimal getestet oder Browser</span></a>

------------------------------------------------------------------------

-   Windows Phone 7 und 7.5

-   Google Android WebKit 3.1 Browser Android 2.2.1 (Kernel 2.6)

-   Apple Safari für iPhone-Betriebssystem 5.0.1

-   Apple Safari für iPad 2-Betriebssystem 5.0.1

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Browseranforderungen</span></a>

------------------------------------------------------------------------

Browser müssen folgende Anforderungen erfüllen, um die webbasierte Windows PowerShell Web Access-Konsole verwenden zu können.

-   Zulassen von Cookies von der Website des Windows PowerShell Web Access-Gateways.

-   Öffnen und Lesen von HTTPS-Seiten.

-   Öffnen und Ausführen von Websites, die JavaScript verwenden.

<a href="" id="BKMK_recm"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Empfohlene (schnellen)-Bereitstellung</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Sie können das Windows PowerShell Web Access-Gateway auf einem Server mit Windows Server 2012 R2 oder Windows Server 2012 installieren, indem Sie entweder Windows PowerShell-Cmdlets oder den Assistenten zum Hinzufügen von Rollen und Features im Server-Manager verwenden. Verwenden Sie für schnelle Installation und Konfiguration Windows PowerShell-Cmdlets, wie in diesem Abschnitt beschrieben.

-   [Schritt 1: Installieren von Windows PowerShell Web Access](#BKMK_step1)

-   [Schritt 2: Konfigurieren des Gateways](#BKMK_step2)

-   [Schritt 3: Konfigurieren Sie eine restriktive Autorisierungsregel](#BKMK_step3)

<a href="" id="BKMK_step1"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 1: Installieren von Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### So installieren Sie Windows PowerShell Web Access mithilfe von Windows PowerShell-Cmdlets

1.  Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Benutzerrechten, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste in der Taskleiste auf **Windows PowerShell** und anschließend auf **Als Administrator ausführen**.

    -   Klicken Sie auf dem Windows-**Startbildschirm** mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie anschließend auf **Als Administrator ausführen**.

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
    <td><p>In Windows PowerShell 3.0 und 4.0 muss das Server-Manager-Cmdlet-Modul vor der Ausführung von Cmdlets, die zum Modul gehören, nicht in die Windows PowerShell-Sitzung importiert werden. Module werden automatisch importiert, wenn Sie ein zum Modul gehörendes Cmdlet zum ersten Mal ausführen. Außerdem wird die Groß- und Kleinschreibung bei Windows PowerShell-Cmdlets nicht berücksichtigt.</p></td>
    </tr>
    </tbody>
    </table>

2.  Geben Sie Folgendes ein, und drücken Sie dann die **EINGABETASTE**, wobei *Computername* für den Remotecomputer steht, auf dem Sie Windows PowerShell Web Access ggf. installieren möchten. Mit dem <span class="code">Restart</span>-Parameter werden Zielserver bei Bedarf automatisch neu gestartet.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_374a9c21-4f6e-471e-b957-bb190a594533'); "In Zwischenablage kopieren.")

        Install-WindowsFeature –Name WindowsPowerShellWebAccess -ComputerName <computer_name> -IncludeManagementTools -Restart

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
    <td><p>Bei der Installation von Windows PowerShell Web Access mithilfe von Windows PowerShell-Cmdlets werden Webserver (IIS)-Verwaltungstools nicht standardmäßig hinzugefügt. Wenn Sie die Verwaltungstools auf demselben Server wie das Windows PowerShell Web Access-Gateway installieren möchten, fügen Sie dem Installationsbefehl den <span class="code">IncludeManagementTools</span>-Parameter hinzu (wie in diesem Schritt angegeben). Wenn Sie die Windows PowerShell Web Access-Website über einen Remotecomputer verwalten, installieren Sie das IIS-Manager-Snap-In, indem Sie die <a href="http://go.microsoft.com/fwlink/?LinkID=304145">Remoteserver-Verwaltungstools für Windows 8.1</a> oder die <a href="http://go.microsoft.com/fwlink/p/?LinkID=238560">Remoteserver-Verwaltungstools für Windows 8</a> auf dem Computer installieren, von dem aus Sie das Gateway verwalten möchten.</p></td>
    </tr>
    </tbody>
    </table>

    Zum Installieren von Rollen oder Features auf einer Offline-VHD müssen Sie sowohl den <span class="code">ComputerName</span>- als auch den <span class="code">VHD</span>-Parameter hinzufügen. Der <span class="code">ComputerName</span>-Parameter enthält den Namen des Servers, auf dem die VHD eingebunden werden soll. Der <span class="code">VHD</span>-Parameter enthält den Pfad zur VHD-Datei auf dem angegebenen Server.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_d841d509-347e-49d0-bf54-8d1f306bece6'); "In Zwischenablage kopieren.")

        Install-WindowsFeature –Name WindowsPowerShellWebAccess –VHD <path> -ComputerName <computer_name> -IncludeManagementTools -Restart

3.  Stellen Sie nach Abschluss der Installation sicher, dass Windows PowerShell Web Access auf den Zielservern installiert wurde. Führen Sie dazu das Cmdlet **Get-WindowsFeature** auf einem Zielserver in einer Windows PowerShell-Konsole aus, die mit erhöhten Benutzerrechten geöffnet wurde. Sie können die Installation von Windows PowerShell Web Access auch in der Server-Manager-Konsole überprüfen, indem Sie auf der Seite **Alle Server** einen Zielserver auswählen und anschließend die Kachel **Rollen und Features** für den ausgewählten Server anzeigen. Sie können auch die Infodatei für Windows PowerShell Web Access anzeigen.

4.  Nachdem Windows PowerShell Web Access installiert wurde, werden Sie aufgefordert, die Infodatei zu lesen, in der grundlegende, erforderliche Setupanweisungen für das Gateway enthalten sind. Diese Setupanweisungen finden Sie auch im nächsten Abschnitt [Schritt 2: Konfigurieren des Gateways](#BKMK_step2). Die Infodatei befindet sich unter <span class="computerOutputInline">C:\\Windows\\Web\\PowerShellWebAccess\\wwwroot\\README.txt</span>.

<a href="" id="BKMK_step2"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 2: Konfigurieren des Gateways</span></a>

------------------------------------------------------------------------

Das Cmdlet **Install-PswaWebApplication** stellt eine schnelle Möglichkeit zum Konfigurieren von Windows PowerShell Web Access dar. Sie können den <span class="code">UseTestCertificate</span>-Parameter zwar dem Cmdlet <span class="code">Install-PswaWebApplication</span> hinzufügen, um zu Testzwecken ein selbstsigniertes SSL-Zertifikat zu installieren, aber dies ist kein sicheres Verfahren. Verwenden Sie für eine sichere Produktionsumgebung immer ein gültiges SSL-Zertifikat, das von einer Zertifizierungsstelle signiert wurde. Mithilfe der IIS-Manager-Konsole können Administratoren das Testzertifikat durch ein signiertes Zertifikat ihrer Wahl ersetzen.

Sie können die Konfiguration der Windows PowerShell Web Access-Webanwendung durchführen, indem Sie entweder das Cmdlet <span class="code">Install-PswaWebApplication</span> ausführen oder im IIS-Manager GUI-basierte Konfigurationsschritte ausführen. Standardmäßig wird die Webanwendung **pswa** (und der dazugehörige Anwendungspool **pswa_pool**) durch das Cmdlet im Container **Standardwebsite** installiert, wie im IIS-Manager gezeigt. Bei Bedarf können Sie das Cmdlet anweisen, den Container „Standardwebsite“ der Webanwendung zu ändern. Der IIS-Manager bietet Konfigurationsoptionen, die für Webanwendungen verfügbar sind, beispielsweise das Ändern der Portnummer oder des SSL-Zertifikats (Secure Sockets Layer).

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC17938.jpeg" title="System_CAPS_security" alt="System_CAPS_security" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-security" /></span><span class="alertTitle"> Sicherheitshinweis </span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Es wird nachdrücklich empfohlen, dass Administratoren das Gateway so konfigurieren, dass ein gültiges, von einer Zertifizierungsstelle signiertes Zertifikat verwendet wird.</p></td>
</tr>
</tbody>
</table>

-   [Das Windows PowerShell Web Access-Gateway mit einem Testzertifikat zu konfigurieren, mithilfe von Install-PswaWebApplication](#BKMK_testcert)

-   [Das Windows PowerShell Web Access-Gateway mit einem originalzertifikat zu konfigurieren, mithilfe von Install-PswaWebApplication und IIS-Manager](#BKMK_gencert)

#### So verwenden Sie das Install-PswaWebApplication-Cmdlet, um das Windows PowerShell Web Access-Gateway mit einem Testzertifikat zu konfigurieren

1.  Öffnen Sie eine Windows PowerShell-Sitzung, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste auf der Taskleiste auf **Windows PowerShell**.

    -   Klicken Sie auf der Windows-Startseite** **auf **Windows PowerShell**.

2.  Geben Sie Folgendes ein, und drücken Sie anschließend die **EINGABETASTE**.

    **Install-PswaWebApplication - UseTestCertificate**

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC17938.jpeg" title="System_CAPS_security" alt="System_CAPS_security" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-security" /></span><span class="alertTitle"> Sicherheitshinweis </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Der <span class="code">UseTestCertificate</span>-Parameter sollte nur in einer privaten Testumgebung verwendet werden. Für sichere Produktionsumgebungen empfiehlt sich die Verwendung eines gültigen Zertifikats, das von einer Zertifizierungsstelle signiert wurde.</p></td>
    </tr>
    </tbody>
    </table>

    Beim Ausführen des Cmdlets wird die Windows PowerShell Web Access-Webanwendung im Container „Standardwebsite“ von IIS installiert. Das Cmdlet erstellt die erforderliche Infrastruktur zum Ausführen von Windows PowerShell Web Access auf der Standardwebsite „https://&lt;Servername&gt;/pswa“. Geben Sie zum Installieren der Webanwendung auf einer anderen Website den Websitenamen an, indem Sie den <span class="code">WebSiteName</span>-Parameter hinzufügen. Fügen Sie den <span class="code">WebApplicationName</span>-Parameter hinzu, um den Namen der Webanwendung zu ändern (der Standardname lautet <span class="code">pswa</span>).

    Die folgenden Einstellungen werden durch die Ausführung des Cmdlets konfiguriert. Falls gewünscht, können Sie diese in der IIS-Manager-Konsole manuell ändern.

    -   Pfad: /pswa

    -   Anwendungspool: pswa_pool

    -   Aktivierte Protokolle: http

    -   Physischer Pfad: %*windir*%/Web/PowerShellWebAccess/wwwroot

    <span class="label">Beispiel:</span> <span class="code">Install-PswaWebApplication – Webanwendungsname MyWebApp – UseTestCertificate</span>

    In diesem Beispiel ergibt sich als Website für Windows PowerShell Web Access „https://&lt;*Servername*&gt;/myWebApp“.

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
    <td><p>Eine Anmeldung ist erst möglich, nachdem den Benutzern durch Hinzufügen von Autorisierungsregeln der Zugriff auf die Website gestattet wurde. Weitere Informationen finden Sie unter <a href="#BKMK_step3">Schritt 3: Konfigurieren einer restriktiven Autorisierungsregel</a> und <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access</a>.</p></td>
    </tr>
    </tbody>
    </table>

#### So verwenden Sie Install-PswaWebApplication und IIS-Manager, um das Windows PowerShell Web Access-Gateway mit einem Originalzertifikat zu konfigurieren

1.  Öffnen Sie eine Windows PowerShell-Sitzung, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste auf der Taskleiste auf **Windows PowerShell**.

    -   Klicken Sie auf der Windows-Startseite** **auf **Windows PowerShell**.

2.  Geben Sie Folgendes ein, und drücken Sie anschließend die **EINGABETASTE**.

    **Install-PswaWebApplication**

    Die folgenden Gatewayeinstellungen werden durch die Ausführung des Cmdlets konfiguriert. Falls gewünscht, können Sie diese in der IIS-Manager-Konsole manuell ändern. Sie können auch Werte für die Parameter <span class="code">WebsiteName</span> und <span class="code">WebApplicationName</span> des Cmdlets <span class="code">Install-PswaWebApplication</span> angeben.

    -   Pfad: /pswa

    -   Anwendungspool: pswa_pool

    -   Aktivierte Protokolle: http

    -   Physischer Pfad: %*windir*%/Web/PowerShellWebAccess/wwwroot

3.  Öffnen Sie die IIS-Manager-Konsole, indem Sie eine der folgenden Aktionen ausführen:

    -   Starten Sie auf dem Windows-Desktop den Server-Manager, indem Sie in der Windows-Taskleiste auf **Server-Manager** klicken. Klicken Sie im Menü **Tools** im Server-Manager auf **Internetinformationsdienste-Manager (IIS)**.

    -   Klicken Sie auf der Windows-Startseite** **auf **Server-Manager**.

4.  Erweitern Sie im IIS-Manager-Strukturbereich den Knoten für den Server, auf dem Windows PowerShell Web Access installiert ist, bis der Ordner **Sites** sichtbar ist. Erweitern Sie den Ordner **Sites**.

5.  Wählen Sie die Website aus, auf der Sie die Windows PowerShell Web Access-Webanwendung installiert haben. Klicken Sie im Bereich **Aktionen** auf **Bindungen**.

6.  Klicken Sie im Dialogfeld **Websitebindung** auf **Hinzufügen**.

7.  Wählen Sie im Dialogfeld **Websitebindung hinzufügen** im Feld **Typ** die Option **https** aus.

8.  Wählen Sie das signierte Zertifikat im Dropdownmenü des Felds **SSL-Zertifikat** aus. Klicken Sie auf **OK**. Weitere Informationen zum Anfordern eines Zertifikats finden Sie in diesem Thema unter [So konfigurieren Sie ein SSL-Zertifikat im IIS-Manager](#BKMK_cert).

    Die Windows PowerShell Web Access-Webanwendung ist jetzt für die Verwendung des signierten SSL-Zertifikats konfiguriert. Sie können auf Windows PowerShell Web Access zugreifen, indem Sie „https://&lt;Servername&gt;/pswa“ in einem Browserfenster öffnen.

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
    <td><p>Eine Anmeldung ist erst möglich, nachdem den Benutzern durch Hinzufügen von Autorisierungsregeln der Zugriff auf die Website gestattet wurde. Weitere Informationen finden Sie unter <a href="#BKMK_step3">Schritt 3: Konfigurieren einer restriktiven Autorisierungsregel</a> und <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access</a>.</p></td>
    </tr>
    </tbody>
    </table>

<a href="" id="BKMK_step3"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 3: Konfigurieren Sie eine restriktive Autorisierungsregel</span></a>

------------------------------------------------------------------------

Nach der Installation von Windows PowerShell Web Access und der Konfiguration des Gateways können Benutzer die Anmeldeseite in einem Browser öffnen. Sie können sich jedoch erst anmelden, nachdem der Windows PowerShell Web Access-Administrator ihnen ausdrücklich Zugriff gewährt hat. Die Windows PowerShell Web Access-Zugriffssteuerung wird mithilfe der Windows PowerShell-Cmdlets verwaltet, die in der folgenden Tabelle beschrieben sind. Eine vergleichbare GUI für das Hinzufügen und Verwalten von Autorisierungsregeln ist nicht verfügbar. Ausführlichere Informationen zu Windows PowerShell Web Access-Cmdlets finden Sie in den Cmdlet-Referenzthemen unter [Windows PowerShell Web Access-Cmdlets](https://technet.microsoft.com/library/hh918342.aspx).

Weitere Informationen zu Windows PowerShell Web Access-Autorisierungsregeln und -Sicherheit finden Sie unter [Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx).

#### So fügen Sie eine restriktive Autorisierungsregel hinzu

1.  Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Benutzerrechten, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste in der Taskleiste auf **Windows PowerShell** und anschließend auf **Als Administrator ausführen**.

    -   Klicken Sie auf dem Windows-**Startbildschirm** mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie anschließend auf **Als Administrator ausführen**.

2.  <span class="label">Optionaler Schritt zur Einschränkung des Benutzerzugriffs mithilfe von Sitzungskonfigurationen:</span> Stellen Sie sicher, dass Sitzungskonfigurationen, die Sie in Ihren Regeln verwenden, bereits möchten vorhanden. Falls diese noch nicht erstellt wurden, verwenden Sie die Anleitung zum Erstellen von Sitzungskonfigurationen in MSDN unter [about_Session_Configuration_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx).

3.  Geben Sie Folgendes ein, und drücken Sie anschließend die **EINGABETASTE**.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_f9e7959b-75d0-4d63-8f8e-02334a8dd09d'); "In Zwischenablage kopieren.")

        Add-PswaAuthorizationRule –UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Diese Autorisierungsregel erlaubt es einem bestimmten Benutzer, auf einen Computer im Netzwerk zuzugreifen, auf den er normalerweise zugreifen kann. Der Zugriff ist auf eine bestimmte Sitzungskonfiguration beschränkt, die die üblichen Anforderungen des Benutzers im Hinblick auf die Ausführung von Skripts und Cmdlets abdeckt. Im folgenden Beispiel wird dem Benutzer <span class="code">JSmith</span> in der Domäne <span class="code">Contoso</span> Zugriff auf die Verwaltung des Computers <span class="code">Contoso_214</span> und die Verwendung einer Sitzungskonfiguration mit dem Namen <span class="code">NewAdminsOnly</span> gewährt.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_ebd5bc5e-ec5d-4955-a86a-63843e480e37'); "In Zwischenablage kopieren.")

        Add-PswaAuthorizationRule –UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Stellen Sie sicher, dass die Regel erstellt wurde, indem Sie mit der **Get-PswaAuthorizationRule** -Cmdlets oder **Test-PswaAuthorizationRule - UserName &lt;Domäne\\Benutzer | Computer\\user&gt; - ComputerName** &lt;Computername&gt;. Z. B. **Test-PswaAuthorizationRule – UserName Contoso\\JSmith – ComputerName Contoso_214**.

Nachdem Sie eine Autorisierungsregel konfiguriert haben, können sich autorisierte Benutzer bei der webbasierten Konsole anmelden und mit der Nutzung von Windows PowerShell Web Access beginnen.

<a href="" id="BKMK_custom"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Benutzerdefinierte Bereitstellung</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Sie können das Windows PowerShell Web Access-Gateway auf einem Server mit Windows Server 2012 R2 oder Windows Server 2012 installieren, indem Sie den Assistenten zum Hinzufügen von Rollen und Features im Server-Manager verwenden. Nach der Installation von Windows PowerShell Web Access können Sie die Konfiguration des Gateways im IIS-Manager anpassen.

<a href="" id="BKMK_custom1"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 1: Installieren von Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### So installieren Sie Windows PowerShell Web Access mithilfe des Assistenten zum Hinzufügen von Rollen und Features

1.  Wenn der Server-Manager bereits geöffnet ist, fahren Sie mit dem nächsten Schritt fort. Ist der Server-Manager noch nicht geöffnet, öffnen Sie ihn mit einer der folgenden Aktionen.

    -   Starten Sie auf dem Windows-Desktop den Server-Manager, indem Sie in der Windows-Taskleiste auf **Server-Manager** klicken.

    -   Klicken Sie auf der Windows-Startseite** **auf **Server-Manager**.

2.  Klicken Sie im Menü **Verwalten** auf **Rollen und Funktionen hinzufügen**.

3.  Wählen Sie auf der Seite **Installationstyp auswählen** die Option **Rollenbasierte oder featurebasierte Installation** aus. Klicken Sie auf **Weiter**.

4.  Wählen Sie auf der Seite **Zielserver auswählen** einen Server aus dem Serverpool aus, oder wählen Sie eine Offline-VHD aus. Um eine Offline-VHD als Zielserver auszuwählen, müssen Sie zuerst den Server auswählen, auf dem die VHD eingebunden werden soll. Wählen Sie anschließend die VHD-Datei aus. Informationen zum Hinzufügen von Servern zu Ihrem Serverpool finden Sie in der Server-Manager-Hilfe. Klicken Sie nach dem Auswählen des Zielservers auf **Weiter**.

5.  Erweitern Sie im Assistenten auf der Seite **Features auswählen** die Option **Windows PowerShell**, und wählen Sie anschließend **Windows PowerShell Web Access** aus.

6.  Sie werden aufgefordert, erforderliche Features, wie beispielsweise .NET Framework 4.5, und Rollendienste des Webservers (IIS) hinzuzufügen. Fügen Sie die erforderlichen Features hinzu, und setzen Sie den Vorgang fort.

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
    <td><p>Bei der Installation von Windows PowerShell Web Access mithilfe des Assistenten zum Hinzufügen von Rollen und Features wird auch Webserver (IIS) einschließlich IIS-Manager-Snap-In installiert. Das Snap-In und andere IIS-Verwaltungstools werden standardmäßig installiert, wenn Sie den Assistenten zum Hinzufügen von Rollen und Features verwenden. Wenn Sie Windows PowerShell Web Access mithilfe von Windows PowerShell-Cmdlets wie im folgenden Verfahren beschrieben installieren, werden Verwaltungstools nicht standardmäßig hinzugefügt.</p></td>
    </tr>
    </tbody>
    </table>

7.  Falls die Featuredateien für Windows PowerShell Web Access nicht auf dem in Schritt 4 ausgewählten Zielserver gespeichert sind, klicken Sie auf der Seite **Installationsauswahl bestätigen** auf **Alternativen Quellpfad angeben**, und geben Sie den Pfad zu den Featuredateien an. Klicken Sie andernfalls auf **Installieren**.

8.  Nachdem Sie auf **Installieren** geklickt haben, werden auf der Seite **Installationsstatus** der Installationsstatus, Ergebnisse und Meldungen wie Warnungen, Fehler oder nach der Installation auszuführende Konfigurationsschritten angezeigt, die für Windows PowerShell Web Access erforderlich sind. Nachdem Windows PowerShell Web Access installiert wurde, werden Sie aufgefordert, die Infodatei zu lesen, in der grundlegende, erforderliche Setupanweisungen für das Gateway enthalten sind. Diese Anweisungen sind auch in diesem Thema enthalten. Die Infodatei befindet sich unter <span class="computerOutputInline">C:\\Windows\\Web\\PowerShellWebAccess\\wwwroot\\README.txt</span>.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 2: Konfigurieren des Gateways</span></a>

------------------------------------------------------------------------

Die Anweisungen in diesem Abschnitt gelten für die Installation der Windows PowerShell Web Access-Webanwendung in einem Unterverzeichnis der Website, und nicht im Stammverzeichnis. Dieses GUI-basierte Verfahren entspricht den Aktionen, die vom Cmdlet <span class="code">Install-PswaWebApplication</span> durchgeführt werden. Dieser Abschnitt enthält auch Anweisungen zur Verwendung des IIS-Managers zum Konfigurieren des Windows PowerShell Web Access-Gateways als Stammwebsite.

-   [IIS-Manager verwenden, um das Gateway auf einer vorhandenen Website konfigurieren](#BKMK_configman)

-   [IIS-Manager verwenden, um das Gateway als Stammwebsite mit einem Testzertifikat zu konfigurieren](#BKMK_configroot)

-   

#### So verwenden Sie den IIS-Manager, um das Gateway auf einer vorhandenen Website zu konfigurieren

1.  Öffnen Sie die IIS-Manager-Konsole, indem Sie eine der folgenden Aktionen ausführen:

    -   Starten Sie auf dem Windows-Desktop den Server-Manager, indem Sie in der Windows-Taskleiste auf **Server-Manager** klicken. Klicken Sie im Menü **Tools** im Server-Manager auf **Internetinformationsdienste-Manager (IIS)**.

    -   Geben Sie auf dem Windows-**Startbildschirm** einen beliebigen Teil des Namens **Internetinformationsdienste-Manager (IIS)** ein. Klicken Sie auf die Verknüpfung, wenn diese in den **Apps**-Ergebnissen angezeigt wird.

2.  Erstellen Sie einen neuen Anwendungspool für Windows PowerShell Web Access. Erweitern Sie den Knoten des Gatewayservers im IIS-Manager-Strukturbereich, wählen Sie die **Anwendungspools** aus, und klicken Sie im Bereich** Aktionen** auf **Anwendungspool hinzufügen**.

3.  Fügen Sie einen neuen Anwendungspool mit dem Namen **pswa_pool** hinzu, oder geben Sie einen anderen Namen an. Klicken Sie auf **OK**.

4.  Erweitern Sie im IIS-Manager-Strukturbereich den Knoten für den Server, auf dem Windows PowerShell Web Access installiert ist, bis der Ordner **Sites** sichtbar ist. Wählen Sie den Ordner **Sites** aus.

5.  Klicken Sie mit der rechten Maustaste auf die Website (z.B. **Standardwebsite**), der Sie die Windows PowerShell Web Access-Website hinzufügen möchten, und klicken Sie anschließend auf **Anwendung hinzufügen**.

6.  Geben Sie im Feld **Alias** „pswa“ ein, oder geben Sie einen anderen Alias an. Der Alias wird zum Namen des virtuellen Verzeichnisses. In der folgenden URL steht **pswa** z.B. für den Alias, der in diesem Schritt angegeben wurde: „https://&lt;Servername&gt;/pswa“.

7.  Wählen Sie im Feld **Anwendungspool** den Anwendungspool aus, den Sie in Schritt 3 erstellt haben.

8.  Suchen Sie im Feld **Physischer Pfad** nach dem Speicherort der Anwendung. Sie können den Standardspeicherort %%amp;quot;%windir%/Web/PowerShellWebAccess/wwwroot%%amp;quot;% verwenden. Klicken Sie auf **OK**.

9.  Führen Sie die Schritte des Verfahrens [So konfigurieren Sie ein SSL-Zertifikat im IIS-Manager](#BKMK_cert) aus, das in diesem Thema enthalten ist.

10. <span class="label">Optionaler Sicherheitsschritt:</span> der Website im Strukturbereich ausgewählt ist, und doppelklicken Sie auf **SSL-Einstellungen** im Inhaltsbereich. Wählen Sie die Option **SSL erforderlich** aus, und klicken Sie anschließend im Bereich **Aktionen** auf **Übernehmen**. Optional können Sie es im Bereich **SSL-Einstellungen** obligatorisch machen, dass Benutzer, die eine Verbindung mit der Windows PowerShell Web Access-Website herstellen, über Clientzertifikate verfügen. Clientzertifikate dienen dazu, die Identität des Benutzers eines Clientgeräts zu überprüfen. Weitere Informationen dazu, wie das Anfordern von Clientzertifikaten die Sicherheit von Windows PowerShell Web Access erhöhen kann, finden Sie unter [Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx) in diesem Handbuch.

11. Öffnen Sie eine Browsersitzung auf einem Clientgerät. Weitere Informationen zu unterstützten Browsern und Geräten finden Sie in diesem Thema unter [Unterstützung für Browser und Clientgeräte](#BKMK_browser).

12. Öffnen Sie die neue Windows PowerShell Web Access-Website https://&lt;*Gatewayservername*&gt;/pswa.

    Der Browser sollte die Anmeldeseite der Windows PowerShell Web Access-Konsole anzeigen.

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
    <td><p>Eine Anmeldung ist erst möglich, nachdem den Benutzern durch Hinzufügen von Autorisierungsregeln der Zugriff auf die Website gestattet wurde.</p></td>
    </tr>
    </tbody>
    </table>

13. Führen Sie in einer mit erhöhten Benutzerrechten (Als Administrator ausführen) geöffneten Windows PowerShell-Sitzung das folgende Skript aus (hierbei entspricht *Anwendungspoolname* dem Namen des in Schritt 3 erstellten Anwendungspools), um dem Anwendungspool Zugriffsrechte für die Autorisierungsdatei zu erteilen.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_c1a80a93-8fcf-4beb-a025-5f81bfb8bdae'); "In Zwischenablage kopieren.")

        $applicationPoolName = "<application_pool_name>"
        $authorizationFile = "C:\windows\web\powershellwebaccess\data\AuthorizationRules.xml"
        c:\windows\system32\icacls.exe $authorizationFile /grant ('"' + "IIS AppPool\$applicationPoolName" + '":R') > $null

    Führen Sie den folgenden Befehl aus, um die vorhandenen Zugriffsrechte für die Autorisierungsdatei anzuzeigen:

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_ac2179c2-9548-4a17-8663-267fdd105380'); "In Zwischenablage kopieren.")

        c:\windows\system32\icacls.exe $authorizationFile

#### So verwenden Sie den IIS-Manager, um das Gateway als Stammwebsite mit einem Testzertifikat zu konfigurieren

1.  Öffnen Sie die IIS-Manager-Konsole, indem Sie eine der folgenden Aktionen ausführen:

    -   Starten Sie auf dem Windows-Desktop den Server-Manager, indem Sie in der Windows-Taskleiste auf **Server-Manager** klicken. Klicken Sie im Menü **Tools** im Server-Manager auf **Internetinformationsdienste-Manager (IIS)**.

    -   Geben Sie auf dem Windows-**Startbildschirm** einen beliebigen Teil des Namens **Internetinformationsdienste-Manager (IIS)** ein. Klicken Sie auf die Verknüpfung, wenn diese in den **Apps**-Ergebnissen angezeigt wird.

2.  Erweitern Sie im IIS-Manager-Strukturbereich den Knoten für den Server, auf dem Windows PowerShell Web Access installiert ist, bis der Ordner **Sites** sichtbar ist. Wählen Sie den Ordner **Sites** aus.

3.  Klicken Sie im Bereich **Aktionen** auf **Website hinzufügen**.

4.  Geben Sie einen Namen für die Website ein, z.B. **Windows PowerShell Web Access**.

5.  Für die neue Website wird automatisch ein Anwendungspool erstellt. Wenn Sie einen anderen Anwendungspool verwenden möchten, klicken Sie auf **Auswählen**, um einen Anwendungspool zum Zuordnen zur neuen Website auszuwählen. Wählen Sie den alternativen Anwendungspool im Dialogfeld **Anwendungspool auswählen** aus, und klicken Sie anschließend auf **OK**.

6.  Navigieren Sie im Textfeld **Physischer Pfad** zu %*windir*%/Web/PowerShellWebAccess/wwwroot.

7.  Wählen Sie unter **Bindung** im Feld **Typ** die Option **https** aus.

8.  Weisen Sie der Website eine Portnummer zu, die noch nicht von einer anderen Website oder -anwendung verwendet wird. Sie können den **netstat**-Befehl in einem Eingabeaufforderungsfenster ausführen, um nach offenen Ports zu suchen. Die Standardportnummer ist 443.

    Ändern Sie den Standardport, falls Port 443 bereits von einer anderen Website verwendet wird oder wenn das Ändern der Portnummer aus anderen Sicherheitsgründen notwendig ist. Falls eine andere Website, die auf dem Gatewayserver ausgeführt wird, den ausgewählten Port verwendet, wird eine Warnung angezeigt, wenn Sie im Dialogfeld **Website hinzufügen** auf **OK** klicken. Sie müssen einen nicht verwendeten Port zum Ausführen von Windows PowerShell Web Access verwenden.

9.  Falls dies für Ihre Organisation erforderlich ist, können Sie optional einen Hostnamen angeben, der für die Organisation und Benutzer sinnvoll ist, z.B. **www.contoso.com**. Klicken Sie auf **OK**.

10. Zur Erhöhung der Sicherheit von Produktionsumgebungen wird dringend dazu geraten, ein gültiges Zertifikat bereitzustellen, das von einer Zertifizierungsstelle signiert wurde. Sie müssen ein SSL-Zertifikat bereitstellen, weil Benutzer die Verbindung mit Windows PowerShell Web Access nur über eine HTTPS-Website herstellen können. Weitere Informationen zum Anfordern eines Zertifikats finden Sie in diesem Thema unter [So konfigurieren Sie ein SSL-Zertifikat im IIS-Manager](#BKMK_cert).

11. Klicken Sie auf **OK**, um das Dialogfeld **Website hinzufügen** zu schließen.

12. Führen Sie in einer mit erhöhten Benutzerrechten (Als Administrator ausführen) geöffneten Windows PowerShell-Sitzung das folgende Skript aus (hierbei entspricht *Anwendungspoolname* dem Namen des in Schritt 4 erstellten Anwendungspools), um dem Anwendungspool Zugriffsrechte für die Autorisierungsdatei zu erteilen.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_35ae9944-ca44-4af7-9c96-616083b3e3db'); "In Zwischenablage kopieren.")

        $applicationPoolName = "<application_pool_name>"
        $authorizationFile = "C:\windows\web\powershellwebaccess\data\AuthorizationRules.xml"
        c:\windows\system32\icacls.exe $authorizationFile /grant ('"' + "IIS AppPool\$applicationPoolName" + '":R') > $null

    Führen Sie den folgenden Befehl aus, um die vorhandenen Zugriffsrechte für die Autorisierungsdatei anzuzeigen:

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_0eb6d70a-2807-498b-b088-fa5233ed68d5'); "In Zwischenablage kopieren.")

        c:\windows\system32\icacls.exe $authorizationFile

13. Klicken Sie, während die neue Website im IIS-Manager-Strukturbereich ausgewählt ist, im Bereich **Aktionen** auf **Start**, um die Website zu starten.

14. Öffnen Sie eine Browsersitzung auf einem Clientgerät. Weitere Informationen zu unterstützten Browsern und Geräten finden Sie in diesem Dokument unter [Unterstützung für Browser und Clientgeräte](#BKMK_browser).

15. Öffnen Sie die neue Windows PowerShell Web Access-Website.

    Da die Stammwebsite auf den Windows PowerShell Web Access-Ordner verweist, sollte im Browser die Anmeldeseite von Windows PowerShell Web Access angezeigt werden, wenn Sie „https://&lt; *Gatewayservername*&gt;“ öffnen. Es sollte nicht erforderlich sein, der URL **/pswa** hinzuzufügen.

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
    <td><p>Eine Anmeldung ist erst möglich, nachdem den Benutzern durch Hinzufügen von Autorisierungsregeln der Zugriff auf die Website gestattet wurde.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Schritt 3: Konfigurieren Sie eine restriktive Autorisierungsregel</span></a>

------------------------------------------------------------------------

Nach der Installation von Windows PowerShell Web Access und der Konfiguration des Gateways können Benutzer die Anmeldeseite in einem Browser öffnen. Sie können sich jedoch erst anmelden, nachdem der Windows PowerShell Web Access-Administrator ihnen ausdrücklich Zugriff gewährt hat. Die Windows PowerShell Web Access-Zugriffssteuerung wird mithilfe der Windows PowerShell-Cmdlets verwaltet, die in der folgenden Tabelle beschrieben sind. Eine vergleichbare GUI für das Hinzufügen und Verwalten von Autorisierungsregeln ist nicht verfügbar. Ausführlichere Informationen zu Windows PowerShell Web Access-Cmdlets finden Sie in den Cmdlet-Referenzthemen unter [Windows PowerShell Web Access-Cmdlets](https://technet.microsoft.com/library/hh918342.aspx).

Weitere Informationen zu Windows PowerShell Web Access-Autorisierungsregeln und -Sicherheit finden Sie unter [Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx).

#### So fügen Sie eine restriktive Autorisierungsregel hinzu

1.  Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Benutzerrechten, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste in der Taskleiste auf **Windows PowerShell** und anschließend auf **Als Administrator ausführen**.

    -   Klicken Sie auf dem Windows-**Startbildschirm** mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie anschließend auf **Als Administrator ausführen**.

2.  <span class="label">Optionaler Schritt zur Einschränkung des Benutzerzugriffs mithilfe von Sitzungskonfigurationen:</span> Stellen Sie sicher, dass Sitzungskonfigurationen, die Sie in Ihren Regeln verwenden, bereits möchten vorhanden. Falls diese noch nicht erstellt wurden, verwenden Sie die Anleitung zum Erstellen von Sitzungskonfigurationen in MSDN unter [about_Session_Configuration_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx).

3.  Geben Sie Folgendes ein, und drücken Sie anschließend die **EINGABETASTE**.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_4df22c91-f56f-4bb5-91e7-99f9b365ed5d'); "In Zwischenablage kopieren.")

        Add-PswaAuthorizationRule –UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Diese Autorisierungsregel erlaubt es einem bestimmten Benutzer, auf einen Computer im Netzwerk zuzugreifen, auf den er normalerweise zugreifen kann. Der Zugriff ist auf eine bestimmte Sitzungskonfiguration beschränkt, die die üblichen Anforderungen des Benutzers im Hinblick auf die Ausführung von Skripts und Cmdlets abdeckt. Im folgenden Beispiel wird dem Benutzer <span class="code">JSmith</span> in der Domäne <span class="code">Contoso</span> Zugriff auf die Verwaltung des Computers <span class="code">Contoso_214</span> und die Verwendung einer Sitzungskonfiguration mit dem Namen <span class="code">NewAdminsOnly</span> gewährt.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_efc3999a-2905-453f-86cd-014b41658ffc'); "In Zwischenablage kopieren.")

        Add-PswaAuthorizationRule –UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Stellen Sie sicher, dass die Regel erstellt wurde, indem Sie mit der **Get-PswaAuthorizationRule** -Cmdlets oder **Test-PswaAuthorizationRule - UserName &lt;Domäne\\Benutzer | Computer\\user&gt; - ComputerName** &lt;Computername&gt;. Z. B. **Test-PswaAuthorizationRule – UserName Contoso\\JSmith – ComputerName Contoso_214**.

Nachdem Sie eine Autorisierungsregel konfiguriert haben, können sich autorisierte Benutzer bei der webbasierten Konsole anmelden und mit der Nutzung von Windows PowerShell Web Access beginnen.

<a href="" id="BKMK_configcert"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Konfigurieren eines echten Zertifikats</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_4" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Für sichere Produktionsumgebungen sollten Sie stets ein gültiges, von einer Zertifizierungsstelle (ZS) signiertes SSL-Zertifikat verwenden. In diesem Abschnitt wird das Verfahren beschrieben, mit dem Sie ein gültiges SSL-Zertifikat von einer Zertifizierungsstelle beziehen und anwenden.

### So konfigurieren Sie ein SSL-Zertifikat im IIS-Manager

1.  Wählen Sie im IIS-Manager-Strukturbereich den Server aus, auf dem Windows PowerShell Web Access installiert ist.

2.  Doppelklicken Sie im Inhaltsbereich auf **Serverzertifikate**.

3.  Führen Sie im Bereich **Aktionen** eine der folgenden Aktionen aus. Weitere Informationen zur Konfiguration von Serverzertifikaten in IIS finden Sie unter [Konfigurieren von Serverzertifikaten in IIS 7](https://technet.microsoft.com/library/cc732230.aspx).

    -   Klicken Sie auf **Importieren**, um ein vorhandenes gültiges Zertifikat von einem Speicherort im Netzwerk zu importieren.

    -   Klicken Sie auf **Zertifikatanforderung erstellen**, um ein Zertifikat von einer Zertifizierungsstelle wie VeriSign™, Thawte oder GeoTrust® anzufordern. Der allgemeine Name des Zertifikats muss dem Hostheader in der Anforderung entsprechen. Wenn der Clientbrowser beispielsweise %%amp;quot;http://www.contoso.com/%%amp;quot; anfordert, muss der allgemeine Name ebenfalls %%amp;quot;http://www.contoso.com/%%amp;quot; lauten. Dies ist die sicherste und empfohlene Option, um das Windows PowerShell Web Access-Gateway mit einem Zertifikat zu versehen.

    -   Klicken Sie auf **Selbstsigniertes Zertifikat erstellen**, um ein Zertifikat zu erstellen, das Sie sofort verwenden und später bei Bedarf von einer Zertifizierungsstelle signieren lassen können. Geben Sie einen Anzeigenamen für das selbstsignierte Zertifikat an, z.B. **Windows PowerShell Web Access**. Diese Vorgehensweise ist als nicht sicher anzusehen und wird nur für eine private Testumgebung empfohlen.

4.  Wählen Sie nach der Erstellung bzw. Beschaffung eines Zertifikats die Website, auf die das Zertifikat angewendet werden soll (z.B. **Standardwebsite**), im IIS-Manager-Strukturbereich aus. Klicken Sie anschließend im Bereich **Aktionen** auf **Bindungen**.

5.  Fügen Sie im Dialogfeld **Websitebindung hinzufügen** eine Bindung vom Typ **https** für die Website hinzu, falls noch keine Bindung angezeigt wird. Wenn Sie kein selbstsigniertes Zertifikat verwenden, geben Sie den Hostnamen aus Schritt 3 dieses Verfahrens an. Wenn Sie ein selbstsigniertes Zertifikat verwenden, ist dieser Schritt nicht erforderlich.

6.  Wählen Sie das Zertifikat aus, das Sie in Schritt 3 dieses Verfahrens abgerufen oder erstellt haben, und klicken Sie anschließend auf **OK**.

<a href="" id="BKMK_using"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Mithilfe der webbasierten Windows PowerShell-Konsole</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_5" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Nachdem Windows PowerShell Web Access installiert und die Gatewaykonfiguration wie in diesem Thema beschrieben abgeschlossen wurde, ist die webbasierte Windows PowerShell-Konsole für die Verwendung bereit. Weitere Informationen zu den ersten Schritten mit der webbasierten Konsole finden Sie unter [Verwendung der webbasierten Windows PowerShell-Konsole](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx).

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Siehe auch</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_6" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Dokumentation zu Internetinformationsdienste (IIS) 7.0](https://technet.microsoft.com/library/cc753433.aspx)
[Hilfe zum IIS-Manager 7.0](https://technet.microsoft.com/library/cc732664.aspx)
[Konfigurieren der Webserversicherheit (IIS 7)](https://technet.microsoft.com/library/cc731278.aspx)
[Ressourcen zur IPsec-Bereitstellung](https://technet.microsoft.com/network/bb531150)

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


