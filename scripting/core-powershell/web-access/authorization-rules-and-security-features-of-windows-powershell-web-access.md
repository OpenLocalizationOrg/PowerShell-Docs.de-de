---
title: Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: dc50a729855a71d61c187da9d698bd294f740546

---

# Autorisierungsregeln und Sicherheitsfeatures von Windows PowerShell Web Access

Aktualisiert: 24. Juni 2013

Gilt für: Windows Server 2012 R2, Windows Server 2012

Windows PowerShell® Web Access in Windows Server® 2012 R2 und in Windows Server® 2012 verfügt über ein restriktives Sicherheitsmodell. Benutzern muss explizit der Zugriff gewährt werden, bevor sie sich am Windows PowerShell Web Access-Gateway anmelden und die webbasierte Windows PowerShell-Konsole verwenden können.

-   [Konfigurieren von Autorisierungsregeln und Websitesicherheit](#BKMK_auth)

-   [Sitzungsverwaltung](#BKMK_sesmgmt)


Nach der Installation von Windows PowerShell Web Access und der Konfiguration des Gateways können Benutzer die Anmeldeseite in einem Browser öffnen. Sie können sich jedoch erst anmelden, nachdem der Windows PowerShell Web Access-Administrator ihnen ausdrücklich Zugriff gewährt hat. Die Windows PowerShell Web Access-Zugriffssteuerung wird mithilfe der Windows PowerShell-Cmdlets verwaltet, die in der folgenden Tabelle beschrieben sind. Eine vergleichbare GUI für das Hinzufügen und Verwalten von Autorisierungsregeln ist nicht verfügbar. Ausführlichere Informationen zu Windows PowerShell Web Access-Cmdlets finden Sie in den Cmdlet-Referenzthemen unter [Windows PowerShell Web Access-Cmdlets](https://technet.microsoft.com/library/hh918342.aspx).

Administratoren können 0-*n*-Authentifizierungsregeln für Windows PowerShell Web Access definieren. Die Standardsicherheit ist nicht %%amp;quot;tolerant%%amp;quot;, sondern restriktiv: Wenn keine Authentifizierungsregel vorhanden ist, darf kein Benutzer auf irgendetwas zugreifen.

Die Cmdlets „Add-PswaAuthorizationRule“ und „Test-PswaAuthorizationRule“ enthalten nun unter Windows Server 2012 R2 einen „Credential“-Parameter, mit dem Sie Windows PowerShell Web Access-Autorisierungsregeln von einem Remotecomputer aus oder innerhalb einer aktiven Windows PowerShell Web Access-Sitzung hinzufügen und testen können. Wie bei anderen Windows PowerShell-Cmdlets, die über einen „Credential“-Parameter verfügen, können Sie ein „PSCredential“-Objekt als Wert des Parameters angeben. Führen Sie das Cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) aus, um ein „PSCredential“-Objekt zu erstellen, das Anmeldeinformationen enthält, die Sie an einen Remotecomputer übergeben möchten.

Windows PowerShell Web Access-Authentifizierungsregeln sind Whitelist-Regeln. Jede Regel ist eine Definition einer zulässigen Verbindung zwischen Benutzern, Zielcomputern und bestimmten Windows PowerShell-[Sitzungskonfigurationen](https://technet.microsoft.com/library/dd819508.aspx) (auch als Endpunkte oder Runspaces bezeichnet) auf angegebenen Zielcomputern.

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
<td><p>Für einen Benutzer muss nur eine Regel zutreffen, damit er Zugriff erhält. Wenn einem Benutzer der Zugriff auf einen Computer mit vollem Sprachzugriff oder ausschließlichem Zugriff auf Windows PowerShell-Remoteverwaltungs-Cmdlets gewährt wird, kann sich der Benutzer über die webbasierte Konsole an anderen Computern anmelden, die mit dem ersten Zielcomputer verbunden sind (bzw. per Hop zu diesen Computern wechseln). Die sicherste Möglichkeit, Windows PowerShell Web Access zu konfigurieren, besteht darin, Benutzern nur den Zugriff auf eingeschränkte Sitzungskonfigurationen zu gewähren (auch als Endpunkte oder Runspaces bezeichnet), mit denen die Erledigung bestimmter Aufgaben möglich ist, die andernfalls remote ausgeführt werden müssten.</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Name</p></th>
<th><p>Beschreibung</p></th>
<th><p>Parameter</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="https://technet.microsoft.com/library/jj592890.aspx">Add-PswaAuthorizationRule</a></p></td>
<td><p>Fügt dem Windows PowerShell Web Access-Autorisierungsregelsatz eine neue Autorisierungsregel hinzu.</p></td>
<td><ul>
<li><p>ComputerGroupName</p></li>
<li><p>ComputerName</p></li>
<li><p>ConfigurationName</p></li>
<li><p>RuleName</p></li>
<li><p>UserGroupName</p></li>
<li><p>UserName</p></li>
<li><p>Credential (Windows Server 2012 R2 und höher)</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><a href="https://technet.microsoft.com/library/jj592893.aspx">Remove-PswaAuthorizationRule</a></p></td>
<td><p>Entfernt eine angegebene Autorisierungsregel aus Windows PowerShell Web Access.</p></td>
<td><ul>
<li><p>Id</p></li>
<li><p>RuleName</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><a href="https://technet.microsoft.com/library/jj592891.aspx">Get-PswaAuthorizationRule</a></p></td>
<td><p>Gibt einen Satz von Windows PowerShell Web Access-Autorisierungsregeln zurück. Wird das Cmdlet ohne Parameter verwendet, gibt es alle Regeln zurück.</p></td>
<td><ul>
<li><p>Id</p></li>
<li><p>RuleName</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><a href="https://technet.microsoft.com/library/jj592892.aspx">Test-PswaAuthorizationRule</a></p></td>
<td><p>Wertet Autorisierungsregeln aus, um festzustellen, ob eine bestimmte Benutzer-, Computer- oder Sitzungskonfigurations-Zugriffsanforderung autorisiert ist. Wenn keine Parameter hinzugefügt werden, wertet das Cmdlet standardmäßig alle Autorisierungsregeln aus. Durch das Hinzufügen von Parametern können Administratoren eine zu testende Autorisierungsregel oder eine Teilmenge von zu testenden Autorisierungsregeln angeben.</p></td>
<td><ul>
<li><p>ComputerName</p></li>
<li><p>ConfigurationName</p></li>
<li><p>RuleName</p></li>
<li><p>UserName</p></li>
<li><p>Credential (Windows Server 2012 R2 und höher)</p></li>
</ul></td>
</tr>
</tbody>
</table>

Mit den oben angegebenen Cmdlets wird ein Satz von Zugriffsregeln erstellt, die zum Autorisieren eines Benutzers auf dem Windows PowerShell Web Access-Gateway verwendet werden. Die Regeln unterscheiden sich von Zugriffssteuerungslisten auf dem Zielcomputer und bieten eine zusätzliche Ebene der Sicherheit für den Webzugriff. Weitere Informationen zur Sicherheit finden Sie im folgenden Abschnitt.

Falls ein Benutzer keine der vorherigen Sicherheitsebenen passieren kann, wird im Browserfenster eine allgemeine Meldung des Typs "Zugriff verweigert" angezeigt. Obwohl die Sicherheitsdetails auf dem Gatewayserver protokolliert werden, erhalten die Benutzern keine Informationen darüber, wie viele Sicherheitsebenen sie passiert haben oder auf welcher Ebene der Anmelde- oder Authentifizierungsfehler aufgetreten ist.

Weitere Informationen zur Konfiguration von Autorisierungsregeln finden Sie unter [Konfigurieren von Autorisierungsregeln](#BKMK_configrules) in diesem Thema.

<a href="" id="BKMK_sec"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Sicherheit</span></a>

------------------------------------------------------------------------

Das Windows PowerShell Web Access-Sicherheitsmodell verfügt über vier Ebenen zwischen einem Endbenutzer der webbasierten Konsole und einem Zielcomputer. Windows PowerShell Web Access-Administratoren können Sicherheitsebenen mithilfe zusätzlicher Konfigurationsschritte in der IIS-Manager-Konsole hinzufügen. Weitere Informationen zum Schützen von Websites in der IIS-Manager-Konsole finden Sie unter [Konfigurieren der Webserversicherheit (IIS 7)](https://technet.microsoft.com/library/cc731278(v=ws.10).aspx). Weitere Informationen zu den bewährten IIS-Methoden und zur Verhinderung von Denial-of-Service-Angriffen finden Sie unter [Best Practices for Preventing DoS/Denial of Service Attacks](https://technet.microsoft.com/library/cc750213.aspx) (Bewährte Methoden zur Verhinderung von Denial-of-Service-Angriffen (DoS)). Ein Administrator kann außerdem zusätzliche, im Handel verfügbare Authentifizierungssoftware erwerben und installieren.

Die folgende Tabelle enthält eine Beschreibung der vier Sicherheitsebenen zwischen Endbenutzern und Zielcomputern.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Reihenfolge</p></th>
<th><p>Ebene</p></th>
<th><p>Beschreibung</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>1</p></td>
<td><p>Sicherheitsfeatures des Webservers (IIS), beispielsweise die Clientzertifikatauthentifizierung</p></td>
<td><p>Benutzer von Windows PowerShell Web Access müssen immer einen Benutzernamen und ein Kennwort angeben, um ihre Konten auf dem Gateway zu authentifizieren. Windows PowerShell Web Access-Administratoren können die optionale Authentifizierung per Clientzertifikat jedoch auch aktivieren oder deaktivieren (siehe Schritt 10 unter „To use IIS Manager to configure the gateway in an existing website“ (So konfigurieren Sie das Gateway auf einer vorhandenen Website mithilfe von IIS-Manager) in <a href="https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx">Install and Use Windows PowerShell Web Access</a> (Installieren und Verwenden von Windows PowerShell Web Access)). Für die optionale Clientzertifikatauthentifizierung müssen Endbenutzer zusätzlich zu ihren Benutzernamen und Kennwörtern auch über ein gültiges Clientzertifikat verfügen. Das Feature ist Teil der Konfiguration des Webservers (IIS). Wenn die Clientzertifikatebene aktiviert ist, werden Benutzer auf der Windows PowerShell Web Access-Anmeldeseite zum Angeben gültiger Zertifikate aufgefordert, bevor ihre Anmeldeinformationen ausgewertet werden. Die Clientzertifikatauthentifizierung bewirkt, dass automatisch das Vorhandensein des Clientzertifikats überprüft wird.</p>
<p>Falls kein gültiges Zertifikat gefunden wird, werden die Benutzer von Windows PowerShell Web Access darüber informiert, damit sie das Zertifikat bereitstellen können. Wenn ein gültiges Clientzertifikat gefunden wird, öffnet Windows PowerShell Web Access die Anmeldeseite, auf der Benutzer ihre Benutzernamen und Kennwörter angeben können.</p>
<p>Dies ist nur ein Beispiel für die zusätzlichen Sicherheitseinstellungen, die vom Webserver (IIS) zur Verfügung gestellt werden. Weitere Informationen zu anderen IIS-Sicherheitsfeatures finden Sie unter <a href="https://technet.microsoft.com/library/cc731278(ws.10).aspx">Konfigurieren der Webserversicherheit (IIS 7)</a>.</p></td>
</tr>
<tr class="even">
<td><p>2</p></td>
<td><p>Formularbasierte Gatewayauthentifizierung von Windows PowerShell Web Access</p></td>
<td><p>Die Anmeldeseite von Windows PowerShell Web Access erfordert einen Satz von Anmeldeinformationen (Benutzername und Kennwort) und bietet Benutzern die Möglichkeit, unterschiedliche Anmeldeinformationen für den Zielcomputer anzugeben. Wenn der Benutzer keine alternativen Anmeldeinformationen angibt, werden die primären Benutzernamen- und Kennwortinformationen, die für die Verbindung zum Gateway verwendet werden, auch genutzt, um eine Verbindung mit dem Zielcomputer herzustellen.</p>
<p>Die erforderlichen Anmeldeinformationen werden auf dem Windows PowerShell Web Access-Gateway authentifiziert. Bei diesen Anmeldeinformationen muss es sich um gültige Benutzerkonten auf entweder dem lokalen Windows PowerShell Web Access-Gatewayserver oder in Active Directory® handeln.</p>
<p>Nachdem ein Benutzer am Gateway authentifiziert wurde, werden die Autorisierungsregeln von Windows PowerShell Web Access überprüft, um sicherzustellen, dass der Benutzer zum Zugriff auf den angeforderten Zielcomputer berechtigt ist. Nach der erfolgreichen Autorisierung werden die Anmeldeinformationen des Benutzers an den Zielcomputer übergeben.</p></td>
</tr>
<tr class="odd">
<td><p>3</p></td>
<td><p>Windows PowerShell Web Access-Autorisierungsregeln</p></td>
<td><p>Nachdem ein Benutzer am Gateway authentifiziert wurde, werden die Autorisierungsregeln von Windows PowerShell Web Access überprüft, um sicherzustellen, dass der Benutzer zum Zugriff auf den angeforderten Zielcomputer berechtigt ist. Nach der erfolgreichen Autorisierung werden die Anmeldeinformationen des Benutzers an den Zielcomputer übergeben.</p>
<p>Diese Regeln werden erst ausgewertet, nachdem ein Benutzer vom Gateway authentifiziert wurde und bevor ein Benutzer auf einem Zielcomputer authentifiziert werden kann.</p></td>
</tr>
<tr class="even">
<td><p>4</p></td>
<td><p>Zielauthentifizierung und Autorisierungsregeln</p></td>
<td><p>Die letzte Sicherheitsebene für Windows PowerShell Web Access ist die eigene Sicherheitskonfiguration des Zielcomputers. Für Benutzer müssen auf dem Zielcomputer die richtigen Zugriffsrechte konfiguriert sein. Dies gilt auch für die Windows PowerShell Web Access-Autorisierungsregeln zum Ausführen einer webbasierten Windows PowerShell Web Access-Konsole, die für einen Zielcomputer über Windows PowerShell Web Access verwendet wird.</p>
<p>Diese Ebene bietet die gleichen Sicherheitsmechanismen, die auch Verbindungsversuche auswerten würden, falls Benutzer versuchen, eine Windows PowerShell-Remotesitzung zu einem Zielcomputer mithilfe der Cmdlets <strong>Enter-PSSession</strong> oder <strong>New-PSSession</strong> über Windows PowerShell einzurichten.</p>
<p>Standardmäßig verwendet Windows PowerShell Web Access den primären Benutzernamen und das dazugehörige Kennwort für die Authentifizierung auf dem Gateway und dem Zielcomputer. Die webbasierte Anmeldeseite enthält im Abschnitt <strong>Optionale Verbindungseinstellungen</strong> eine Option, bei der Benutzer bei Bedarf andere Anmeldeinformationen für den Zielcomputer angeben können. Wenn der Benutzer keine alternativen Anmeldeinformationen angibt, werden die primären Benutzernamen- und Kennwortinformationen, die für die Verbindung zum Gateway verwendet werden, auch genutzt, um eine Verbindung mit dem Zielcomputer herzustellen.</p>
<p>Mithilfe von Autorisierungsregeln kann Benutzern der Zugriff auf eine bestimmte Sitzungskonfiguration gestattet werden. Sie können eingeschränkte Runspaces oder Sitzungskonfigurationen für Windows PowerShell Web Access erstellen und für bestimmte Benutzer nur die Verbindung mit speziellen Sitzungskonfigurationen zulassen, wenn diese sich bei Windows PowerShell Web Access anmelden. Mithilfe von Zugriffssteuerungslisten können Sie bestimmen, welche Benutzer auf bestimmte Endpunkte zugreifen können, und den Zugriff auf den Endpunkt für eine bestimmte Gruppe von Benutzern anhand von Autorisierungsregeln weiter einschränken, wie in diesem Abschnitt beschrieben. Weitere Informationen zu eingeschränkten Runspaces finden Sie auf MSDN unter <a href="https://msdn.microsoft.com/library/windows/desktop/ee706589.aspx">Constrained Runspaces</a> (Eingeschränkte Runspaces).</p></td>
</tr>
</tbody>
</table>

<a href="" id="BKMK_configrules"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Konfigurieren von Autorisierungsregeln</span></a>

------------------------------------------------------------------------

Administratoren streben für Windows PowerShell Web Access-Benutzer meist die Verwendung derjenigen Autorisierungsregel an, die in ihrer Umgebung bereits für die Windows PowerShell-Remoteverwaltung definiert ist. Im ersten Verfahren dieses Abschnitts wird beschrieben, wie Sie eine sichere Autorisierungsregel hinzufügen können, die es einem einzelnen Benutzer erlaubt, sich für die Verwaltung eines Computers bei Verwendung einer einzigen Sitzungskonfiguration anzumelden. Das zweite Verfahren beschreibt, wie Sie eine Autorisierungsregel entfernen, die nicht mehr benötigt wird.

Wenn Sie die Verwendung von benutzerdefinierten Sitzungskonfigurationen planen, um für bestimmte Benutzer nur die Arbeit in eingeschränkten Runspaces von Windows PowerShell Web Access zuzulassen, erstellen Sie die benutzerdefinierten Sitzungskonfigurationen vor dem Hinzufügen der Autorisierungsregeln, die darauf verweisen. Sie können die Windows PowerShell Web Access-Cmdlets nicht zum Erstellen von benutzerdefinierten Sitzungskonfigurationen verwenden. Weitere Informationen zur Erstellung von benutzerdefinierten Sitzungskonfigurationen finden Sie auf MSDN unter [about_Session_Configuration_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx).

Für Windows PowerShell Web Access-Cmdlets wird ein Platzhalterzeichen unterstützt, und zwar das Sternchen (\*). Platzhalterzeichen innerhalb von Zeichenfolgen werden nicht unterstützt. Verwenden Sie ein einzelnes Sternchen pro Eigenschaft (Benutzer, Computer oder Sitzungskonfigurationen).

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Hinweis </span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Weitere Möglichkeiten der Verwendung von Autorisierungsregeln, um Benutzern Zugriff zu gewähren, und Hilfe zum Schutz der Windows PowerShell Web Access-Umgebung finden Sie unter <a href="#BKMK_others">Weitere Beispiele für Autorisierungsregelszenarios</a> in diesem Thema.</p></td>
</tr>
</tbody>
</table>

#### So fügen Sie eine restriktive Autorisierungsregel hinzu

1.  Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Benutzerrechten, indem Sie einen der folgenden Schritte durchführen.

    -   Klicken Sie auf dem Windows-Desktop mit der rechten Maustaste in der Taskleiste auf **Windows PowerShell** und anschließend auf **Als Administrator ausführen**.

    -   Klicken Sie auf dem Windows-**Startbildschirm** mit der rechten Maustaste auf **Windows PowerShell**, und klicken Sie anschließend auf **Als Administrator ausführen**.

2.  <span class="label">Optionaler Schritt zur Einschränkung des Benutzerzugriffs mithilfe von Sitzungskonfigurationen:</span> Stellen Sie sicher, dass Sitzungskonfigurationen, die Sie in Ihren Regeln verwenden, bereits möchten vorhanden. Falls diese noch nicht erstellt wurden, verwenden Sie die Anleitung zum Erstellen von Sitzungskonfigurationen in MSDN unter [about_Session_Configuration_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx).

3.  Geben Sie Folgendes ein, und drücken Sie anschließend die **EINGABETASTE**.

    [Kopieren] (javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_1079478f-cd51-4d35-8022-4b532a9d57a4'); „In Zwischenablage kopieren.“)

        Add-PswaAuthorizationRule –UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Diese Autorisierungsregel erlaubt es einem bestimmten Benutzer, auf einen Computer im Netzwerk zuzugreifen, auf den er normalerweise zugreifen kann. Der Zugriff ist auf eine bestimmte Sitzungskonfiguration beschränkt, die die üblichen Anforderungen des Benutzers im Hinblick auf die Ausführung von Skripts und Cmdlets abdeckt. Im folgenden Beispiel wird dem Benutzer <span class="code">JSmith</span> in der Domäne <span class="code">Contoso</span> Zugriff auf die Verwaltung des Computers <span class="code">Contoso_214</span> und die Verwendung einer Sitzungskonfiguration mit dem Namen <span class="code">NewAdminsOnly</span> gewährt.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_4e760377-e401-4ef4-988f-7a0aec1b2a90'); „In Zwischenablage kopieren.“)

        Add-PswaAuthorizationRule –UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Stellen Sie sicher, dass die Regel erstellt wurde, indem Sie mit der **Get-PswaAuthorizationRule** -Cmdlets oder **Test-PswaAuthorizationRule - UserName &lt;Domäne\\Benutzer | Computer\\user&gt; - ComputerName** &lt;Computername&gt;. Z. B. **Test-PswaAuthorizationRule – UserName Contoso\\JSmith – ComputerName Contoso_214**.

#### So entfernen Sie eine Autorisierungsregel

1.  Falls noch keine Windows PowerShell-Sitzung geöffnet wurde, finden Sie die entsprechende Vorgehensweise in Schritt 1 unter [So fügen Sie eine restriktive Autorisierungsregel hinzu](#BKMK_arar) in diesem Abschnitt.

2.  Geben Sie Folgendes ein, und drücken Sie anschließend die **EINGABETASTE**, wobei *Regel-ID* für die eindeutige ID-Nummer der Regel steht, die Sie entfernen möchten.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_0daef66d-0ecf-47fb-a8a0-d4dbceb8409d'); „In Zwischenablage kopieren.“)

        Remove-PswaAuthorizationRule -ID <rule ID>

    Falls Ihnen die ID-Nummer nicht bekannt ist, dafür jedoch der Anzeigename der zu entfernenden Regel, können Sie alternativ dazu den Namen der Regel abrufen und diesen an das Cmdlet <span class="code">Remove-PswaAuthorizationRule</span> weiterreichen, um die Regel zu entfernen. Dies wird im folgenden Beispiel veranschaulicht: Get-PswaAuthorizationRule -RuleName &lt;*Regelname*&gt; | Remove-PswaAuthorizationRule.

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
    <td><p>Sie werden nicht aufgefordert zu bestätigen, dass die angegebene Autorisierungsregel gelöscht werden soll. Die Regel wird gelöscht, wenn Sie die <strong>EINGABETASTE</strong> drücken. Vergewissern Sie sich, ob die Autorisierungsregel wirklich entfernt werden soll, bevor Sie das Cmdlet <strong>Remove-PswaAuthorizationRule</strong> ausführen.</p></td>
    </tr>
    </tbody>
    </table>

<a href="" id="BKMK_others"></a>
####

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Andere autorisierungsregelszenarios</span></a>

------------------------------------------------------------------------

Bei jeder Windows PowerShell-Sitzung wird eine Sitzungskonfiguration verwendet. Falls für eine Sitzung keine Konfiguration angegeben ist, wird von Windows PowerShell die standardmäßige integrierte Windows PowerShell-Sitzungskonfiguration mit dem Namen „Microsoft.PowerShell“ verwendet. Die Standardsitzungskonfiguration schließt alle auf einem Computer verfügbaren Cmdlets ein. Administratoren können den Zugriff auf alle Computer einschränken, indem sie eine Sitzungskonfiguration mit eingeschränktem Runspace (ein begrenzter Bereich von Cmdlets und Aufgaben, die die Endbenutzer ausführen können) definieren. Ein Benutzer, dem der Zugriff auf einen Computer gestattet wurde (entweder mit vollem Sprachzugriff oder nur mit Zugriff auf die Windows PowerShell-Remoteverwaltungs-Cmdlets), kann Verbindungen mit anderen Computern herstellen, die mit dem ersten Computer verbunden sind. Durch die Definition eines eingeschränkten Runspace kann verhindert werden, dass Benutzer von ihrem zulässigen Windows PowerShell-Runspace aus auf andere Computer zugreifen. Außerdem wird dadurch die Sicherheit der Windows PowerShell Web Access-Umgebung verbessert. Die Sitzungskonfiguration kann (per Gruppenrichtlinie) an alle Computer verteilt werden, für die Administratoren den Zugriff über Windows PowerShell Web Access gewähren möchten. Weitere Informationen zu Sitzungskonfigurationen finden Sie unter [about_Session_Configurations](https://technet.microsoft.com/library/dd819508.aspx). Im Folgenden sind einige Beispiele für die Verwendung von Sitzungskonfigurationen aufgeführt.

-   Ein Administrator erstellt einen Endpunkt namens **PswaEndpoint** mit einem eingeschränkten Runspace. Anschließend erstellt der Administrator die Regel **\*,\*,PswaEndpoint** und verteilt den Endpunkt an andere Computer. Mithilfe der Regel können alle Benutzer auf alle Computer mit dem Endpunkt **PswaEndpoint** zugreifen. Falls es sich um die einzige Autorisierungsregel handelt, die in dem Regelsatz definiert ist, ist der Zugriff auf Computer ohne diesen Endpunkt nicht möglich.

-   Der Administrator hat einen Endpunkt mit dem eingeschränkten Runspace **PswaEndpoint** erstellt und möchte den Zugriff auf bestimmte Benutzer beschränken. Der Administrator erstellt die Benutzergruppe **Level1Support** und definiert die folgende Regel: **Level1Support,\*,PswaEndpoint**. Mit der Regel wird allen Benutzern der Gruppe **Level1Support** Zugriff auf alle Computer mit der Konfiguration **PswaEndpoint** gewährt. Ebenso ist es möglich, den Zugriff auf eine bestimmte Gruppe von Computern zu beschränken.

-   Einige Administratoren gewähren bestimmten Benutzern mehr Zugriff als anderen. Beispielsweise erstellt ein Administrator die beiden Benutzergruppen **Admins** und **BasicSupport**. Ferner erstellt der Administrator einen Endpunkt mit dem eingeschränkten Runspace **PswaEndpoint** und definiert die folgenden beiden Regeln: **Admins,\*,\*** und **BasicSupport,\*,PswaEndpoint**. Mit der ersten Regel wird allen Benutzern der Gruppe **Admin** Zugriff auf alle Computer gewährt, und mit der zweiten Regel wird allen Benutzern der Gruppe **BasicSupport** nur Zugriff auf Computer mit **PswaEndpoint** gewährt.

-   Ein Administrator hat eine private Testumgebung eingerichtet und möchte nun allen autorisierten Netzwerkbenutzern den Zugriff auf alle Computer im Netzwerk ermöglichen, auf die sie normalerweise zugreifen können, und zwar mit Zugriff auf alle Sitzungskonfigurationen, auf die sie normalerweise zugreifen können. Da es sich um eine private Testumgebung handelt, erstellt der Administrator eine Autorisierungsregel, die nicht sicher ist. Der Administrator führt das Cmdlet <span class="code">Add-PswaAuthorizationRule \* \* \*</span> aus. Dabei wird das Platzhalterzeichen **\*** verwendet, um alle Benutzer, alle Computer und alle Konfigurationen anzugeben. Diese Regel entspricht der folgenden: <span class="code">Add-PswaAuthorizationRule -UserName \* -ComputerName \* -ConfigurationName \*</span>.

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
    <td><p>Diese Regel empfiehlt sich nicht für eine sichere Umgebung und umgeht die von Windows PowerShell Web Access bereitgestellte Autorisierungsregel-Sicherheitsebene.</p></td>
    </tr>
    </tbody>
    </table>

-   Ein Administrator muss für Benutzer in einer Umgebung, in der sowohl Arbeitsgruppen als auch Domänen enthalten sind, das Herstellen einer Verbindung mit Zielcomputern zulassen. Dabei werden Arbeitsgruppencomputer gelegentlich verwendet, um eine Verbindung mit Zielcomputern in Domänen herzustellen, und Computer in Domänen werden gelegentlich verwendet, um eine Verbindung mit Zielcomputern in Arbeitsgruppen herzustellen. Der Administrator verfügt über einen Gatewayserver (*PswaServer*) in einer Arbeitsgruppe, und der Zielcomputer *srv1.contoso.com* befindet sich in einer Domäne. Der Benutzer *Chris* ist ein autorisierter lokaler Benutzer auf dem Arbeitsgruppen-Gatewayserver und auf dem Zielcomputer. Sein Benutzername auf dem Arbeitsgruppenserver lautet *chrisLocal*, und sein Benutzername auf dem Zielcomputer lautet *contoso\\chris*. Der Administrator fügt die folgende Regel hinzu, um den Zugriff auf %%amp;quot;srv1.contoso.com%%amp;quot; für Chris zu autorisieren.

    [Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_8d183d3d-1c19-44b8-9297-530b0efc7c79'); „In Zwischenablage kopieren.“)

        Add-PswaAuthorizationRule –userName PswaServer\chrisLocal –computerName srv1.contoso.com –configurationName Microsoft.PowerShell

    Im vorigen Regelbeispiel wird Chris auf dem Gatewayserver authentifiziert und anschließend sein Zugriff auf *srv1* autorisiert. Auf der Anmeldeseite muss Chris unter **Optionale Verbindungseinstellungen** (*contoso\\chris*) einen zweiten Satz von Anmeldeinformationen angeben. Der Gatewayserver verwendet den zusätzlichen Satz von Anmeldeinformationen für die Authentifizierung auf dem Zielcomputer *srv1.contoso.com*.

    Im obigen Szenario richtet Windows PowerShell Web Access eine Verbindung mit dem Zielcomputer erst ein, nachdem Folgendes erfolgreich durchgeführt und von mindestens einer Autorisierungsregel zugelassen wurde.

    1.  Authentifizierung des Arbeitsgruppen-Gatewayservers durch Hinzufügen eines Benutzernamens im Format *server_name*\\*user_name* zur Autorisierungsregel

    2.  Authentifizierung auf dem Zielcomputer mithilfe von alternativen Anmeldeinformationen, die auf der Anmeldeseite unter **Optionale Verbindungseinstellungen** angegeben wurden

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
    <td><p>Wenn sich Gateway- und Zielcomputer in unterschiedlichen Arbeitsgruppen oder Domänen befinden, muss eine Vertrauensstellung zwischen den beiden Arbeitsgruppencomputern, den beiden Domänen oder der Arbeitsgruppe und der Domäne eingerichtet werden. Diese Vertrauensstellung kann nicht mithilfe von Windows PowerShell Web Access-Autorisierungsregel-Cmdlets konfiguriert werden. Mit den Autorisierungsregeln wird keine Vertrauensstellung zwischen Computern definiert. Damit kann für Benutzer nur die Verbindung mit bestimmten Zielcomputern und Sitzungskonfigurationen konfiguriert werden. Weitere Informationen zur Konfiguration einer Vertrauensstellung zwischen unterschiedlichen Domänen finden Sie unter <a href="https://technet.microsoft.com/library/cc794775.aspx">Creating Domain and Forest Trusts</a> (Erstellen von Domänen- und Gesamtstruktur-Vertrauensstellungen). Weitere Informationen zum Hinzufügen von Arbeitsgruppencomputern zu einer Liste mit vertrauenswürdigen Hosts finden Sie unter <a href="https://technet.microsoft.com/library/dd759202.aspx">Remoteverwaltung mit dem Server-Manager</a>.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Verwenden eine einzelne Gruppe von Autorisierungsregeln für mehrere Standorte</span></a>

------------------------------------------------------------------------

Autorisierungsregeln werden in einer XML-Datei gespeichert. Standardmäßig wird die XML-Datei unter dem Pfad „%windir%\\Web\\PowershellWebAccess\\data\\AuthorizationRules.xml“ gespeichert.

Der Pfad zur XML-Datei mit den Autorisierungsregeln ist in der Datei **powwa.config** gespeichert, die im Ordner „%windir%\\Web\\PowershellWebAccess\\data“ enthalten ist. Der Administrator kann den Verweis auf den Standardpfad in **powwa.config** flexibel ändern, falls Präferenzen oder Anforderungen dies erfordern. Da der Administrator den Speicherort der Datei ändern kann, können mehrere Windows PowerShell Web Access-Gateways dieselben Autorisierungsregeln verwenden, falls eine Konfiguration dieser Art gewünscht wird.

<a href="" id="BKMK_sesmgmt"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Sitzungsverwaltung</span></a>
<a href="/en-us/library/dn282394(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Standardmäßig begrenzt Windows PowerShell Web Access einen Benutzer auf drei zeitgleiche Sitzungen. Sie können die Datei **web.config** der Webanwendung im IIS-Manager bearbeiten, um eine andere Anzahl von Sitzungen pro Benutzer zu unterstützen. Der Pfad zur Datei **web.config** lautet „$Env:Windir\\Web\\PowerShellWebAccess\\wwwroot\\Web.config“.

Standardmäßig ist der Webserver (IIS) so konfiguriert, dass der Anwendungspool neu gestartet wird, wenn Einstellungen bearbeitet werden. Beispielsweise wird der Anwendungspool neu gestartet, wenn Änderungen an der Datei **web.config** vorgenommen werden. Da von Windows PowerShell Web Access speicherinterne Sitzungszustände verwendet werden, verlieren bei Windows PowerShell Web Access-Sitzungen angemeldete Benutzer ihre Sitzungen, wenn der Anwendungspool neu gestartet wird.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Festlegen der Standardparameter auf der Anmeldeseite</span></a>

------------------------------------------------------------------------

Wenn Ihr Windows PowerShell Web Access-Gateway unter Windows Server 2012 R2 ausgeführt wird, können Sie die Standardwerte für die Einstellungen konfigurieren, die auf der Windows PowerShell Web Access-Anmeldeseite angezeigt werden. Sie können Werte in der Datei **web.config** konfigurieren, die im vorherigen Abschnitt beschrieben wurde. Standardwerte für die Anmeldeseiteneinstellungen finden Sie im Abschnitt **appSettings** der Datei „web.config“. Es folgt ein Beispiel des Abschnitts **appSettings**. Gültige Werte für viele dieser Einstellungen entsprechen denen der entsprechenden Parameter des Cmdlets [New-PSSession](https://technet.microsoft.com/library/hh849717.aspx) in Windows PowerShell. Der im folgenden Codeblock enthaltene Schlüssel <span class="code">defaultApplicationName</span> ist z.B. der Wert der Einstellungsvariablen **$PSSessionApplicationName** auf dem Zielcomputer.

[Kopieren](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_6ccfd0a1-485a-4ac5-9636-89ebab501bef'); „In Zwischenablage kopieren.“)

    <appSettings>
            <add key="maxSessionsAllowedPerUser" value="3"/>
            <add key="defaultPortNumber" value="5985"/>
            <add key="defaultSSLPortNumber" value="5986"/>
            <add key="defaultApplicationName" value="WSMAN"/>
            <add key="defaultUseSslSelection" value="0"/>
            <add key="defaultAuthenticationType" value="0"/>
            <add key="defaultAllowRedirection" value="0"/>
            <add key="defaultConfigurationName" value="Microsoft.PowerShell"/>
    </appSettings>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Timeouts und verbindungstrennungen ungeplanten</span></a>

------------------------------------------------------------------------

Windows PowerShell Web Access-Sitzungstimeout In Windows PowerShell Web Access unter Windows Server 2012 wird nach 15-minütiger Inaktivität angemeldeten Benutzern eine Timeoutmeldung angezeigt. Wenn der Benutzer nach dem Anzeigen der Timeoutmeldung nicht innerhalb von fünf Minuten reagiert, wird die Sitzung beendet, und der Benutzer wird abgemeldet. Sie können die Zeitspanne für den Sitzungstimeout in den Websiteeinstellungen im IIS-Manager ändern.

In Windows PowerShell Web Access unter Windows Server 2012 R2 wird standardmäßig nach 20-minütiger Inaktivität ein Sitzungstimeout angezeigt. Wenn Benutzer aufgrund von Netzwerkfehlern oder anderen ungeplanten Herunterfahrvorgängen oder Ausfällen von Sitzungen in der webbasierten Konsole getrennt werden, werden die Windows PowerShell Web Access-Sitzungen weiter ausgeführt und bleiben mit den Zielcomputern verbunden, bis die Timeoutperiode auf dem Client überschritten wird. Dies geschieht nicht, wenn Benutzer die Sitzungen selbst beenden. Die Sitzung wird entweder standardmäßig nach 20 Minuten oder nach der vom Gatewayadministrator festgelegten Timeoutperiode getrennt, je nachdem, was kürzer ist.

Wenn der Gatewayserver unter Windows Server 2012 R2 ausgeführt wird, ermöglicht Windows PowerShell Web Access Benutzern das erneute Verbinden mit gespeicherten Sitzungen zu einem späteren Zeitpunkt. Wenn Sitzungen jedoch aufgrund von Netzwerkfehlern, ungeplanten Herunterfahrvorgängen oder Ausfällen getrennt werden, können Benutzer gespeicherte Sitzungen erst sehen bzw. sich wieder mit diesen verbinden, nachdem die vom Gatewayadministrator festgelegte Timeoutperiode abgelaufen ist.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Siehe auch</span></a>
<a href="/en-us/library/dn282394(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Installieren und Verwenden von Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx)
[about_Session_Configurations](https://technet.microsoft.com/library/dd819508.aspx)
[Windows PowerShell Web Access-Cmdlets](https://technet.microsoft.com/library/hh918342.aspx)

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


