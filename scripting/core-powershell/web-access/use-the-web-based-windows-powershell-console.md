---
title: Verwendung der webbasierten Windows PowerShell-Konsole
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 9c633d457db9d15621285b2662244c4190550f63

---

#  Verwendung der webbasierten Windows PowerShell-Konsole

Aktualisiert: 24. Juni 2013

Gilt für: Windows Server 2012 R2, Windows Server 2012

Windows PowerShell® Web Access ermöglicht Windows PowerShell®-Benutzern die Anmeldung bei einer SSL (Secure Sockets Layer )-gesicherten Website zur Verwendung von Windows PowerShell-Sitzungen, Cmdlets und Skripts zur Verwaltung eines Remotecomputers. Da die Windows PowerShell-Konsole in einem Webbrowser ausgeführt wird, kann sie auf verschiedensten Clientgeräten geöffnet werden, etwa auf Handys, Tabletcomputern, öffentlichen Kioskcomputern, Laptopcomputern und gemeinsam genutzten oder ausgeliehenen Computern. Die webbasierte Windows PowerShell-Konsole steuert einen Remotecomputer, der von Benutzern im Zuge der Anmeldung angegeben wird. Dieser Artikel behandelt die Anmeldung für die ersten Schritte mit der webbasierten Windows PowerShell Web Access-Konsole.

Der Artikel beschreibt nicht die Verwendung von Windows PowerShell oder das Ausführen von Cmdlets und Skripts. Informationen zur Verwendung von Windows PowerShell und Skriptressourcen erhalten Sie im Abschnitt „Siehe auch“ am Ende des Artikels.

Inhalte dieses Themas:

-   [Unterstützte Browser und Clientgeräte](#BKMK_browser)

-   [Anmelden bei Windows PowerShell Web Access](#BKMK_sign)

-   [Abmelden und Timeout](#BKMK_timeout)

-   [Unterschiede in der webbasierten Windows PowerShell-Konsole](#BKMK_web)

<a href="" id="BKMK_browser"></a>
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

<a href="" id="BKMK_sign"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Anmelden bei Windows PowerShell Web Access</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Ihr Windows PowerShell Web Access-Administrator sollten Ihnen eine URL bereitstellen, die die Adresse der Website des Windows PowerShell Web Access-Gateways Ihrer Organisation ist. Standardmäßig ist die Adresse dieser Website „https://&lt;server_name&gt;/pswa“. Bevor Sie sich bei Windows PowerShell Web Access anmelden, sollten Sie sichergehen, dass Sie über den Namen oder die IP-Adresse des zu verwaltenden Remotecomputers verfügen. Sie müssen auf dem Remotecomputer ein autorisierter Benutzer sein. Außerdem müssen Sie darauf die Remoteverwaltung zulassen. Weitere Informationen dazu, wie Sie Ihren Computer so konfigurieren, dass eine Remoteverwaltung möglich ist, finden Sie unter [Aktivieren und Verwenden von Remotebefehlen in Windows PowerShell](https://technet.microsoft.com/magazine/ff700227.aspx). Die einfachste Möglichkeit, die Remoteverwaltung auf Ihrem Computer zu konfigurieren, besteht in der Ausführung des Cmdlets **Enable-PSRemoting -force** auf dem Computer in einer Windows PowerShell-Sitzung, die mit erhöhten Benutzerrechten (**Als Administrator ausführen**) geöffnet wurde.

### So melden Sie sich bei Windows PowerShell Web Access an

1.  Öffnen Sie die Windows PowerShell Web Access-Website in einem Fenster oder auf einer Registerkarte des Internetbrowsers.

2.  Geben Sie auf der Windows PowerShell Web Access-Anmeldeseite Ihren Netzwerkbenutzernamen und das dazugehörige Kennwort an, außerdem den Namen des Computers, den Sie verwalten möchten (und auf dem Sie ein autorisierter Benutzer sind). Wenn Sie der Windows PowerShell Web Access-Administrator angewiesen hat, einen URI zu einer benutzerdefinierten Website oder einem Proxyserver statt eines Computernamens zu verwenden, wählen Sie **Verbindungs-URI** im Feld **Verbindungstyp** und geben Sie den URI an.

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
    <td><ul>
    <li><p>Wenn der Computer in einer Arbeitsgruppe ist, verwenden Sie die folgende Syntax, geben Sie Ihren Benutzernamen ein, und melden Sie sich bei dem Computer:&lt;<em>Arbeitsgruppenname</em>&gt;\&lt;<em>User_name</em>&gt;.</p></li>
    <li><p>Wenn es sich beim Zielcomputer um den Gatewayserver handelt, können Sie <strong>localhost</strong> im Feld <strong>Computername</strong> angeben.</p></li>
    <li><p>Wenn der Zielcomputer der Gateway-Server ist und der Gateway-Server in einer Arbeitsgruppe ist, können Sie <strong>Localhost</strong> in die <strong>Computername</strong> Feld, jedoch nicht Localhost\&lt;<em>User_name</em>&gt; in den <strong>Benutzername</strong> Feld. Verwenden Sie &lt;<em>Arbeitsgruppenname</em>&gt;\&lt;<em>User_name</em>&gt;.</p></li>
    </ul></td>
    </tr>
    </tbody>
    </table>

3.  Der Abschnitt **Optionale Verbindungseinstellungen** bezieht sich auf die Autorisierungsanforderungen des zu verwaltenden Remotecomputers. Weitere Informationen zu den Parametern, die den optionalen Verbindungseinstellungen entsprechen, finden Sie in der [Hilfe zum Cmdlet „Enter-PSSession“](https://technet.microsoft.com/library/dd315384.aspx).

    In der Regel sind die Anmeldeinformationen zum Passieren des Windows PowerShell Web Access-Gateways dieselben, die vom zu verwaltenden Remotecomputer erkannt werden. Wenn Sie jedoch andere Anmeldeinformationen zur Verwaltung des in Schritt 2 angegebenen Remotecomputers verwenden möchten, erweitern Sie den Abschnitt **Optionale Verbindungseinstellungen**, und geben Sie die abweichenden Anmeldeinformationen an. Fahren Sie ansonsten mit Schritt 6 fort.

4.  Wenn der Windows PowerShell Web Access-Administrator eine benutzerdefinierte Konfiguration für Windows PowerShell Web Access-Benutzer erstellt hat, geben Sie den Namen der Sitzungskonfiguration in das Feld **Konfigurationsname** ein. Weitere Informationen zu Sitzungskonfigurationen finden Sie unter [about_Session_Configurations](https://technet.microsoft.com/library/dd819508.aspx) auf der Microsoft-Website.

5.  Belassen Sie den **Authentifizierungstyp** als **Standard**, sofern Sie nicht vom Windows PowerShell Web Access-Administrator gegenteilige Anweisungen erhalten haben.

6.  Klicken Sie auf **Anmelden**.

<a href="" id="BKMK_timeout"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Abmelden und Timeout</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Sie werden in den folgenden Situationen von einer webbasierten Windows PowerShell-Sitzung abgemeldet.

-   Wenn Sie unten rechts in der Konsole auf **Abmelden** klicken. (Nur Windows Server 2012)

-   Wenn Sie unten rechts in der Konsole auf **Speichern** oder **Beenden** klicken (nur Windows Server 2012 R2). Durch Klicken auf **Speichern** wird Ihre Windows PowerShell Web Access-Sitzung gespeichert und geschlossen. Sie können zu einem späteren Zeitpunkt die Verbindung mit der Sitzung wiederherstellen. Wenn Sie sich wieder bei Windows PowerShell Web Access anmelden, zeigt Windows PowerShell Web Access eine Liste der gespeicherten Sitzungen an. Sie können entweder eine gespeicherte Sitzung auswählen und die Verbindung erneut herstellen oder eine neue Sitzung starten. Die maximale Anzahl geöffneter (sowohl gespeicherter als auch aktiver Sitzungen), die für Benutzer zulässig sind, wird vom Gatewayadministrator konfiguriert.

    Wenn Sie auf **Beenden** klicken, werden Sie von der Windows PowerShell Web Access-Sitzung abgemeldet, ohne dass diese gespeichert wird.

-   Wenn Sie sich zur Verwaltung eines anderen Remotecomputers in derselben Browsersitzung oder einer neuen Registerkarte desselben Browsers anmelden. (Dies gilt nicht, wenn Windows Server 2012 R2 auf dem Gatewayserver ausgeführt wird. Windows PowerShell Web Access unter Windows Server 2012 R2 lässt mehrere Benutzersitzungen auf neuen Registerkarten in derselben Browsersitzung zu.) Weitere Informationen dazu, wie Sie mehr als eine aktive Sitzung auf demselben Computer starten, finden Sie unter „Aufbauen von Verbindungen zu mehreren Zielcomputern gleichzeitig“ im Abschnitt [Einschränkungen der webbasierten Konsole](#BKMK_limits) dieses Artikels.

-   Wenn Sie mehr als 20 Minuten inaktiv sind. Der Gatewayadministrator kann den Timeoutzeitraum für Inaktivität anpassen. Weitere Informationen finden Sie unter [Sitzungsverwaltung](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx#BKMK_sesmgmt).

    -   Wenn Sie aufgrund von Netzwerkfehlern oder anderen ungeplanten Herunterfahrvorgängen oder Ausfällen in der webbasierten Konsole von Sitzungen getrennt werden und nicht weil Sie die Sitzungen selbst geschlossen haben, wird die Windows PowerShell Web Access-Sitzung weiter ausgeführt. Die Verbindung mit dem Zielcomputer bleibt bestehen, bis das Timeout auf dem Client überschritten wird. In der Standardeinstellung beträgt dieses Zeitlimit 20 Minuten, was vom Gatewayadministrator angepasst werden kann. Die Sitzung wird entweder standardmäßig nach 20 Minuten oder nach der vom Gatewayadministrator festgelegten Timeoutperiode getrennt, je nachdem, was kürzer ist.

        Wenn der Gatewayserver unter Windows Server 2012 R2 ausgeführt wird, ermöglicht Windows PowerShell Web Access Benutzern das erneute Verbinden mit gespeicherten Sitzungen zu einem späteren Zeitpunkt. Die Benutzer können jedoch gespeicherte Sitzungen erst anzeigen bzw. die Verbindung erneut herstellen, nachdem das vom Gatewayadministrator festgelegte Timeout abgelaufen ist.

-   Wenn Sie das Browserfenster oder die Browserregisterkarte schließen.

-   Wenn Sie das Clientgerät ausschalten, auf dem der Browser ausgeführt wird, oder es vom Netzwerk trennen.

-   Wenn Sie den **Beenden**-Befehl in der Webkonsole ausführen. Der Befehl funktioniert nicht, wenn die Sitzungskonfiguration, mit der Sie verbunden sind, zur Unterstützung des [NoLanguage](https://msdn.microsoft.com/library/windows/desktop/system.management.automation.pslanguagemode.aspx)-Modus konfiguriert ist oder sich in einem eingeschränkten Runspace befindet.

Falls Sie sich wieder anmelden möchten, öffnen Sie die Windows PowerShell Web Access-Website erneut, und melden Sie sich anhand der Schritte in [So melden Sie sich bei Windows PowerShell Web Access an](#BKMK_signin) in diesem Artikel an.

<a href="" id="BKMK_web"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Unterschiede in der webbasierten Windows PowerShell-Konsole</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Nach der Anmeldung bei Windows PowerShell Web Access wird im Fenster oder auf der Registerkarte des Browsers eine webbasierte Windows PowerShell-Konsole geöffnet. Die Konsole ist mit dem Remotecomputer verbunden, den Sie während der Anmeldung angegeben haben. Daher können nur die Windows PowerShell-Cmdlets und -Skripts in der Konsole verwendet werden, die auf dem Remotecomputer verfügbar sind. In diesem Abschnitt werden weitere Einschränkungen von Windows PowerShell Web Access-Konsolen sowie die Unterschiede zwischen Windows PowerShell Web Access-Konsolen und der installierten **PowerShell.exe**-Konsole beschrieben.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Funktionelle Unterschiede von PowerShell.exe</span></a>

------------------------------------------------------------------------

Die Mehrzahl der Windows PowerShell-Hostfunktionen steht in der webbasierten Windows PowerShell Web Access-Konsole zur Verfügung, einige jedoch nicht.

-   <span class="label">Geschachtelte Fortschritt angezeigt.</span>  Windows PowerShell Web Access zeigt eine Status-GUI für Cmdlets an, die einen Status ausgeben, es werden jedoch nur Informationen der höchsten Statusebene angezeigt.

-   <span class="label">Geben Sie die Farbe ändern.</span>  Die Eingabefarbe (Vordergrund und Hintergrund) kann nicht geändert werden. Der Stil von Ausgabe-, Warn-, ausführlichen und Fehlermeldungen können alle mithilfe eines Skripts geändert werden.

-   <span class="label">PSHostRawUserInterface.</span>  Windows PowerShell Web Access wird per Windows PowerShell-Remoteverwaltung implementiert und verwendet einen Remoterunspace. Windows PowerShell Web Access implementiert einige Methoden dieser Benutzeroberfläche nicht, z. B. Befehle, die in die Windows-Konsole schreiben. Befehle wie **PowerTab** funktionieren in Windows PowerShell Web Access nicht.

-   <span class="label">Funktionstasten.</span>  Windows PowerShell Web Access unterstützt einige Funktionstasten nicht. In vielen Fällen liegt dies daran, dass die Befehle vom Browser reserviert sind.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Nicht unterstützte Funktionstasten</p></th>
<th><p>Aktion</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>STRG+C</p></td>
<td><p>In Windows PowerShell Web Access wird <strong>STRG+C</strong> vom Browser verwendet, um Inhalte zu kopieren. Die Konsole stellt eine <strong>Abbrechen</strong>-Schaltfläche bereit, darüber hinaus ist der Befehl <strong>STRG+Q</strong> zum Abbrechen von Befehlen verfügbar.</p></td>
</tr>
<tr class="even">
<td><p>ALT+Leertaste, E, L</p></td>
<td><p>Durch den Bildschirmpuffer scrollen</p></td>
</tr>
<tr class="odd">
<td><p>ALT+Leertaste, E, F</p></td>
<td><p>Nach Text im Bildschirmpuffer suchen</p></td>
</tr>
<tr class="even">
<td><p>ALT+Leertaste, E, K</p></td>
<td><p>Vom Bildschirmpuffer zu kopierenden Text auswählen</p></td>
</tr>
<tr class="odd">
<td><p>ALT+Leertaste, E, P</p></td>
<td><p>Daten aus der Zwischenablage in die Windows PowerShell-Konsole einfügen</p></td>
</tr>
<tr class="even">
<td><p>ALT+Leertaste, C</p></td>
<td><p>Schließen der Windows PowerShell-Konsole</p></td>
</tr>
<tr class="odd">
<td><p>STRG+PAUSE</p></td>
<td><p>Schließen des Windows PowerShell-Fensters erzwingen</p></td>
</tr>
<tr class="even">
<td><p>STRG+POS1</p></td>
<td><p>Löschvorgang vom Anfang der aktuellen Befehlszeile durchführen</p></td>
</tr>
<tr class="odd">
<td><p>STRG+ENDE</p></td>
<td><p>Löschvorgang bis zum Ende der aktuellen Befehlszeile durchführen</p></td>
</tr>
<tr class="even">
<td><p>F1</p></td>
<td><p>Cursor in der Befehlszeile ein Zeichen nach rechts verschieben</p></td>
</tr>
<tr class="odd">
<td><p>F2</p></td>
<td><p>Neuen Befehl durch Kopieren des letzten Befehls bis zum getippten Zeichen erstellen</p></td>
</tr>
<tr class="even">
<td><p>F3</p></td>
<td><p>Befehlszeile mit Inhalt der letzten Befehlszeile vervollständigen</p></td>
</tr>
<tr class="odd">
<td><p>F4</p></td>
<td><p>Zeichen von Cursorposition löschen</p></td>
</tr>
<tr class="even">
<td><p>F5</p></td>
<td><p>Befehlsverlauf rückwärts durchsuchen. Greifen sie auf Befehle im Befehlsverlauf in Windows PowerShell Web Access zu, indem Sie in der webbasierten Konsole auf die <strong>Verlauf</strong>-Bildlaufschaltflächen klicken.</p></td>
</tr>
<tr class="odd">
<td><p>F7</p></td>
<td><p>Einen Befehl interaktiv aus dem Befehlsverlauf auswählen</p></td>
</tr>
<tr class="even">
<td><p>F8</p></td>
<td><p>Verlauf durchsuchen und Befehle aufrufen, die dem aktuellen Text entsprechen</p></td>
</tr>
<tr class="odd">
<td><p>F9</p></td>
<td><p>Einen bestimmten, nummerierten Befehl aus dem Verlauf ausführen</p></td>
</tr>
<tr class="even">
<td><p>BILD-AUF</p></td>
<td><p>Den ersten Befehl im Verlauf ausführen</p></td>
</tr>
<tr class="odd">
<td><p>BILD-AB</p></td>
<td><p>Den letzten Befehl im Verlauf ausführen</p></td>
</tr>
<tr class="even">
<td><p>ALT+F7</p></td>
<td><p>Befehlsverlauf löschen</p></td>
</tr>
</tbody>
</table>

<a href="" id="BKMK_limits"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Einschränkungen der webbasierten Konsole</span></a>

------------------------------------------------------------------------

-   <span class="label">Doppel-Hop.</span>   Auf die Doppel-Hop-Einschränkung (oder die Verbindung mit einem zweiten Computer über die erste Verbindung) stoßen Sie, wenn Sie versuchen, eine neue Sitzung zu erstellen oder mit einer neuen Sitzung zu arbeiten, indem Sie Windows PowerShell Web Access verwenden. Windows PowerShell Web Access verwendet einen Remoterunspace, und **PowerShell.exe** unterstützt das Herstellen einer Remoteverbindungen mit einem zweiten Computer von einem Remoterunspace derzeit nicht. Wenn Sie beispielsweise versuchen, mithilfe des Cmdlets **Enter-PSSession** von einer bestehenden Verbindung aus eine Verbindung mit einem zweiten Remotecomputer herzustellen, können verschiedene Fehler gemeldet werden, z. B. „Netzwerkressourcen nicht verfügbar“.

    Derlei Fehler können vermieden werden, indem Ihr Administrator die CredSSP-Authentifizierung in der Netzwerkumgebung Ihrer Organisation einrichtet. Weitere Informationen zur Konfiguration der CredSSP-Authentifizierung finden Sie unter [CredSSP für Zweit-Hop-Remoting](http://blogs.msdn.com/b/powershell/archive/2008/06/05/credssp-for-second-hop-remoting-part-i-domain-account.aspx) auf der Microsoft-Website. Sie können auch explizite Anmeldeinformationen zur Verwaltung eines zweiten Remotecomputers angeben, denn mit impliziten Daten ist es unwahrscheinlich, dass der zweite Hop zugelassen wird.

-   Bei Windows PowerShell Web Access gelten dieselben Einschränkungen wie bei Windows PowerShell-Remotesitzungen. Befehle, die Windows-Konsolen-APIs direkt aufrufen, etwa solche für konsolenbasierte Editoren oder textbasierte Menüprogramme, funktionieren nicht, da die Befehle nicht an die standardmäßigen Eingabe-, Ausgabe- und Fehlerpipes lesen und schreiben. Aus diesem Grund funktionieren Befehle nicht, die ausführbare Dateien wie **notepad.exe** starten oder GUI wie <span class="code">OpenGridView</span> oder <span class="code">ogv</span> anzeigen. Ihre Erfahrung wird durch dieses Verhalten beeinträchtigt, denn für Sie scheint es so, als würde Windows PowerShell Web Access nicht auf Ihre Befehlseingaben reagieren.

-   Die Registerkartenvervollständigung funktioniert in einer Sitzungskonfiguration mit einem beschränkten Runspace oder einer Sitzungskonfiguration im **NoLanguage**-Modus nicht. Obwohl Administratoren eine Sitzung so konfigurieren können, dass die Registerkartenvervollständigung unterstützt wird, raten wir aus Sicherheitsgründen davon ab, da sie für nicht autorisierte Benutzer die folgenden Informationen verfügbar machen kann.

    -   Interne Pfade des Dateisystems

    -   Freigegebene Ordner und interne Computer

    -   Variable im Runspace

    -   Geladene Typen oder .NET Framework-Namespaces

    -   Umgebungsvariablen

-   Benutzer, die bei einer **NoLanguage**-Sitzungskonfiguration oder einem eingeschränkten Runspace in Windows PowerShell Web Access angemeldet sind, können den **Beenden**-Befehl nicht zum Beenden der Sitzung ausführen. Zum Abmelden müssen Benutzer auf der Konsolenseite auf **Abmelden** klicken.

-   <span class="label">Beim Verbinden mit mehreren Zielcomputern gleichzeitig.</span>   Wenn Windows Server 2012 auf dem Gatewayserver ausgeführt wird, lässt Windows PowerShell Web Access nur eine Remotecomputerverbindung pro Browsersitzung zu. Die einmalige Anmeldung und anschließende Verbindung mit mehreren Remotecomputern mithilfe verschiedener Browserregisterkarten ist nicht zulässig. Beim Öffnen eines neuen Browserfensters oder einer neuen Browserregisterkarte werden Sie von Windows PowerShell Web Access dazu aufgefordert, Ihre aktuelle Sitzung zu beenden und eine neue Sitzung zu starten, damit Sie eine Verbindung zu einem neuen (oder demselben) Remotecomputer herstellen können. Falls Sie mehrere separate Sitzungen mit verschiedenen Remotecomputern verwenden möchten, können Sie mithilfe einer Funktion in Internet Explorer eine neue Sitzung erstellen. Starten Sie eine neue Browsersitzung in Internet Explorer, indem Sie **ALT** drücken, das Menü **Datei** öffnen und **Neue Sitzung** auswählen. Öffnen Sie in dieser neuen Sitzung anschließend die Windows PowerShell Web Access-Website, und melden Sie sich an, um auf einen anderen Remotecomputer zuzugreifen.

    Wenn das Windows PowerShell Web Access-Gateway unter Windows Server 2012 R2 ausgeführt wird, können Benutzer mehrere Verbindungen mit Remotecomputern auf verschiedenen Browserregisterkarten öffnen. Wenn Sie mehr als eine Verbindung mit einem Remotecomputer über die webbasierte Windows PowerShell-Konsole öffnen möchten, erkundigen Sie sich bei Ihrem Windows PowerShell Web Access-Gatewayadministrator, ob dieses Feature vom Gatewayserver unterstützt wird.

-   <span class="label">Permanente Windows PowerShell-Sitzungen (erneute Verbindung).</span>   Nachdem die Sitzung mit dem Windows PowerShell Web Access-Gateway abgelaufen ist, wird die Remoteverbindung zwischen dem Gateway und dem Zielcomputer getrennt. Dabei werden alle Cmdlets und Skripts, die aktuell laufen, beendet. Es ist empfehlenswert, bei lang andauernden Aufgaben die Windows PowerShell-**-Job**-Infrastruktur zu verwenden, damit Sie Aufträge starten, die Verbindung mit dem Computer trennen und später wieder herstellen können, ohne dass der Auftrag abgebrochen wird. Ein weiterer Vorteil der Verwendung von **-Job**-Cmdlets ist, dass Sie sie mithilfe von Windows PowerShell Web Access starten, sich abmelden und später wieder anmelden können, indem Sie entweder Windows PowerShell Web Access oder einen anderen Host ausführen (z. B. Windows PowerShell® Integrated Scripting Environment (ISE)).

-   <span class="label">Ändern der Größe der Konsole.</span>   Es stehen die folgenden drei Möglichkeiten zur Änderung der Größe des **PowerShell.exe**-Konsolenfensters zur Verfügung.

    -   Ändern der Konsolenfenstergröße per Ziehen des Mauszeigers

    -   Ändern von Höhe und Breite per GUI für Konsoleneigenschaften

    -   Ändern von Höhe und Breite des Konsolenfensters per Cmdlet

        Das Konsolenfenster von Windows PowerShell Web Access kann wie folgt mithilfe von Cmdlets konfiguriert werden. Im folgenden Beispiel ändert ein Benutzer die Breite der Windows PowerShell Web Access-Konsole auf **20**.

        [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_778d5e55-9195-4bd7-b313-d1fbca7876e4'); "Copy to clipboard.")

            $newSize = $Host.UI.RawUI.WindowSize
            $newSize.Width = $newSize.Width - 20

            $oldSize = $Host.UI.RawUI.WindowSize

            $Host.UI.RawUI.WindowSize = $newSize

        Die Höhe der Konsole lässt sich auf eine ähnliche Weise ändern.

        Weitere Beispiele zur Anpassung der Konsolenansicht stehen im [Windows PowerShell-Teamblog](http://blogs.msdn.com/b/powershell/) zur Verfügung.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Siehe auch</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_4" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Windows PowerShell-Cmdlet-Referenz](https://technet.microsoft.com/library/ee407531(ws.10).aspx)
[Windows PowerShell in Microsoft TechNet](https://technet.microsoft.com/library/bb978526.aspx)
[TechNet-Skriptcenterrepository](http://gallery.technet.microsoft.com/scriptcenter)
[Skriptcenter – Hey, Scripting Guy!](https://technet.microsoft.com/scriptcenter)
[Windows PowerShell-Teamblog](http://blogs.msdn.com/b/powershell/)

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


