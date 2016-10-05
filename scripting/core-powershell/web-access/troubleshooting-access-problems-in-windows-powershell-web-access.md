---
title: Behandeln von Zugriffsproblemen in Windows PowerShell Web Access
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: df935247806f108705c9528e4c5454ce499becef

---

#  Behandeln von Zugriffsproblemen in Windows PowerShell Web Access

Aktualisiert: 24. Juni 2013

Gilt für: Windows Server 2012 R2, Windows Server 2012

<a href="" id="BKMK_trouble"></a>

------------------------------------------------------------------------

In der folgenden Tabelle sind einige allgemeine Probleme aufgeführt, die möglicherweise auftreten, wenn Benutzer über Windows PowerShell Web Access eine Verbindung mit einem Remotecomputer herstellen möchten. Außerdem enthält die Tabelle Vorschläge zur Behebung der Probleme.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Problem</p></th>
<th><p>Mögliche Ursache und Lösung</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Fehler bei der Anmeldung.</p></td>
<td><p>Ein Fehler kann aufgrund einer der folgenden Bedingungen auftreten.</p>
<ul>
<li><p>Es gibt keine Autorisierungsregel, die dem Benutzer den Zugriff auf den Computer oder eine bestimmte Sitzungskonfiguration auf dem Remotecomputer erlaubt. Die Windows PowerShell Web Access-Sicherheit ist restriktiv. Benutzern muss der Zugriff auf Remotecomputer mithilfe von Autorisierungsregeln explizit gewährt werden. Weitere Informationen zum Erstellen von Autorisierungsregeln finden Sie unter <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access</a> in diesem Handbuch.</p></li>
<li><p>Der Benutzer hat keinen autorisierten Zugriff auf den Zielcomputer. Dies wird durch Zugriffssteuerungslisten bestimmt. Weitere Informationen finden Sie unter „Anmelden bei Windows PowerShell Web Access“ in <a href="https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx">Verwenden der webbasierten Windows PowerShell-Konsole</a> oder im <a href="https://msdn.microsoft.com/library/windows/desktop/ee706585.aspx">Windows PowerShell-Teamblog</a>.</p>
<ul>
<li><p>Die Windows PowerShell-Remoteverwaltung ist auf dem Zielcomputer möglicherweise nicht aktiviert. Stellen Sie sicher, dass dieses Feature auf dem Computer aktiviert ist, mit dem der Benutzer eine Verbindung herstellen möchte. Weitere Informationen finden Sie unter „So konfigurieren Sie den Computer für Remoting“ im Abschnitt <a href="https://technet.microsoft.com/library/dd315349.aspx">about_Remote_Requirements</a> der Windows PowerShell-Hilfethemen.</p></li>
</ul></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Wenn Benutzer versuchen, sich in einem Internet Explorer-Fenster bei Windows PowerShell Web Access anzumelden, wird die Seite <strong>Interner Serverfehler</strong> angezeigt, oder der Internet Explorer reagiert nicht mehr. Dieses Problem tritt speziell in Internet Explorer auf.</p></td>
<td><p>Dies kann bei Benutzern der Fall sein, die sich mit einem Domänennamen angemeldet haben, der chinesische Zeichen enthält, oder wenn der Gatewayservername mindestens ein chinesisches Zeichen aufweist. Benutzer können dieses Problem umgehen, indem sie <a href="http://ie.microsoft.com/testdrive/info/downloads/Default.html">Internet Explorer 10 installieren und ausführen</a> und dann die folgenden Schritte ausführen.</p>
<ol>
<li><p>Ändern Sie für Internet Explorer die Einstellung <strong>Dokumentmodus</strong> in <strong>IE10-Standards</strong>.</p>
<ol>
<li><p>Drücken Sie die Taste <strong>F12</strong>, um die Konsole mit den Entwicklertools zu öffnen.</p></li>
<li><p>Klicken Sie in Internet Explorer 10 auf <strong>Browsermodus</strong>, und wählen Sie die Option <strong>Internet Explorer 10</strong> aus.</p></li>
<li><p>Klicken Sie auf <strong>Dokumentmodus</strong> und dann auf <strong>IE10-Standards</strong>.</p></li>
<li><p>Drücken Sie die Taste <strong>F12</strong> erneut, um die Konsole mit den Entwicklertools zu schließen.</p></li>
</ol></li>
<li><p>Deaktivieren Sie die automatische Proxykonfiguration.</p>
<ol>
<li><p>Klicken Sie in Internet Explorer 10 auf <strong>Extras</strong> und dann auf <strong>Internetoptionen</strong>.</p></li>
<li><p>Klicken Sie im Dialogfeld <strong>Internetoptionen</strong> auf der Registerkarte <strong>Verbindungen</strong> auf <strong>LAN-Einstellungen</strong>.</p></li>
<li><p>Deaktivieren Sie das Kontrollkästchen <strong>Automatische Suche der Einstellungen</strong>. Klicken Sie auf <strong>OK</strong> und dann erneut auf <strong>OK</strong>, um das Dialogfeld <strong>Internetoptionen</strong> zu schließen.</p></li>
</ol></li>
</ol></td>
</tr>
<tr class="odd">
<td><p>Es kann keine Verbindung mit einem Remotecomputer in einer Arbeitsgruppe hergestellt werden.</p></td>
<td><p>Wenn der Computer Mitglied einer Arbeitsgruppe ist, verwenden Sie die folgende Syntax, um den Benutzernamen anzugeben, und melden Sie sich an den Computer: &lt;<em>Arbeitsgruppenname</em>&gt;\&lt;<em>User_name</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>Die Verwaltungstools des Webservers (IIS) sind nicht verfügbar, obwohl die Rolle installiert wurde.</p></td>
<td><p>Bei der Installation von Windows PowerShell Web Access mit dem <span class="code">Install-WindowsFeature</span>-Cmdlet werden die Verwaltungstools nur installiert, wenn der <span class="code">IncludeManagementTools</span>-Parameter zum Cmdlet hinzugefügt wird. Ein Beispiel finden Sie in diesem Thema unter „So installieren Sie Windows PowerShell Web Access mithilfe von Windows PowerShell-Cmdlets“ in <a href="https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx">Installieren und Verwenden von Windows PowerShell Web Access</a>. Sie können die IIS-Manager-Konsole und andere benötigte IIS-Verwaltungstools hinzufügen, indem Sie die Tools in einer Sitzung des Assistenten zum Hinzufügen von Rollen und Features auswählen, die auf den Gatewayserver ausgerichtet ist. Der Assistent zum Hinzufügen von Rollen und Features wird über den Server-Manager geöffnet.</p></td>
</tr>
<tr class="odd">
<td><p>Auf die Windows PowerShell Web Access-Website kann nicht zugegriffen werden</p></td>
<td><p>Wenn die verstärkte Sicherheitskonfiguration in Internet Explorer (IE ESC) aktiviert ist, können Sie die Windows PowerShell Web Access-Website der Liste der vertrauenswürdigen Websites hinzufügen oder IE ESC deaktivieren. Sie können IE ESC auf der Kachel <strong>Eigenschaften</strong> der Seite <strong>Lokaler Server</strong> im Server-Manager deaktivieren.</p></td>
</tr>
<tr class="even">
<td><p>Die folgende Fehlermeldung wird beim Versuch angezeigt, eine Verbindung herstellen, wenn der Gatewayserver der Zielcomputer ist und sich auch in einer Arbeitsgruppe befindet: <strong>Fehler bei der Autorisierung. Stellen Sie sicher, dass Sie zum Herstellen einer Verbindung mit dem Zielcomputer autorisiert sind.</strong></p></td>
<td><p>Wenn der Gatewayserver auch der Zielserver ist und sich in einer Arbeitsgruppe befindet, geben Sie den Benutzernamen, den Computernamen und den Benutzergruppennamen wie in der folgenden Tabelle dargestellt an. Verwenden Sie nicht nur einen Punkt (.) als Computernamen.</p>
<div>
<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Szenario</p></th>
<th><p>UserName-Parameter</p></th>
<th><p>UserGroup-Parameter</p></th>
<th><p>ComputerName-Parameter</p></th>
<th><p>ComputerGroup-Parameter</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Gatewayserver befindet sich in Domäne</p></td>
<td><p><em>Servername</em>\<em>Benutzername</em>, Localhost\<em>Benutzername</em>, oder. \<em>User_name</em></p></td>
<td><p><em>Servername</em>\<em>Benutzergruppe</em>, Localhost\<em>Benutzergruppe</em>, oder. \<em>Benutzergruppe</em></p></td>
<td><p>Vollqualifizierter Name des Gatewayservers oder Localhost</p></td>
<td><p><em>Servername</em>\<em>Computergruppe</em>, Localhost\<em>Computergruppe</em>, oder. \<em>Computergruppe</em></p></td>
</tr>
<tr class="even">
<td><p>Gatewayserver befindet sich in Arbeitsgruppe</p></td>
<td><p><em>Servername</em>\<em>Benutzername</em>, Localhost\<em>Benutzername</em>, oder. \<em>User_name</em></p></td>
<td><p><em>Servername</em>\<em>Benutzergruppe</em>, Localhost\<em>Benutzergruppe</em> oder. \<em>Benutzergruppe</em></p></td>
<td><p>Servername</p></td>
<td><p><em>Servername</em>\<em>Computergruppe</em>, Localhost\<em>Computergruppe</em> oder. \<em>Computergruppe</em></p></td>
</tr>
</tbody>
</table>
</div>
<p>Melden Sie sich bei einem Gatewayserver als Zielcomputer an, indem Sie Anmeldeinformationen in einem der folgenden Formate verwenden.</p>
<ul>
<li><p><em>Servername</em>\<em>User_name</em></p></li>
<li><p>Localhost\<em>Benutzername</em></p></li>
<li><p>.\<em>user_name</em></p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Eine Sicherheits-ID (SID) wird angezeigt, in einer Autorisierungsregel anstelle der Syntax <em>User_name</em>/<em>Computername</em> </p></td>
<td><p>Entweder ist die Regel nicht mehr gültig, oder bei der Active Directory-Domänendienste-Abfrage ist ein Fehler aufgetreten. Eine Autorisierungsregel ist normalerweise nicht in Fällen gültig, in denen der Gatewayserver zuerst einer Arbeitsgruppe angehörte, dann jedoch einer Domäne beigetreten ist.</p></td>
</tr>
<tr class="even">
<td><p>Anmeldung an einem Zielcomputer, der in Autorisierungsregeln als IPv6-Adresse mit einer Domäne angegeben wurde, nicht möglich</p></td>
<td><p>Autorisierungsregeln unterstützen keine IPv6-Adresse in Form eines Domänennamens. Verwenden Sie zum Angeben eines Zielcomputers mithilfe einer IPv6-Adresse die ursprüngliche IPv6-Adresse (mit Doppelpunkten) in der Autorisierungsregel. Sowohl domänenbezogene als auch numerische IPv6-Adressen (mit Doppelpunkten) werden auf der Anmeldeseite von Windows PowerShell Web Access als Zielcomputername unterstützt. Dies gilt jedoch nicht für Autorisierungsregeln. Weitere Informationen zu IPv6-Adressen finden Sie unter <a href="https://technet.microsoft.com/library/cc781672.aspx">Funktionsweise von IPv6</a>.</p></td>
</tr>
</tbody>
</table>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Siehe auch</span></a>
<a href="/en-us/library/dn282395(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx)
[Verwendung der webbasierten Windows PowerShell-Konsole](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx)
[about_Remote_Requirements](https://technet.microsoft.com/library/dd315349.aspx)

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


