---
title: Neuerungen in Windows PowerShell 5.0
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 1476722e-947e-425d-a86c-50037488dc6e
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 666590df32157a7477d385961dd5665094275868

---

# Neuerungen in Windows PowerShell
Windows PowerShell® 5.0 bietet wichtige neue Features, die die Verwendungsmöglichkeiten erweitern, die Benutzerfreundlichkeit verbessern und es Ihnen ermöglichen, die Steuerung und Verwaltung von Windows-basierten Umgebungen leichter und umfassender zu bewältigen.

Windows PowerShell 5.0 ist abwärtskompatibel. Cmdlets, Anbieter, Module, Snap-Ins, Skripts, Funktionen und Profile, die für Windows PowerShell 4.0, Windows PowerShell 3.0 und Windows PowerShell 2.0 entwickelt wurden, funktionieren im Allgemeinen ohne Änderungen in Windows PowerShell 5.0.

Windows PowerShell 5.0 wird standardmäßig unter Windows Server ® 2016 Technical Preview und Windows 10 ® installiert. Zum Installieren von Windows PowerShell 5.0 unter Windows Server 2012 R2, Windows 8.1 Enterprise oder Windows 8.1 Pro müssen Sie [Windows Management Framework 5.0](https://aka.ms/wmf5download) herunterladen und installieren. Achten Sie darauf, dass Sie die Details für das Herunterladen lesen und alle Systemanforderungen erfüllen, bevor Sie Windows Management Framework 5.0 installieren.

## Inhalt dieses Themas

-   [Windows PowerShell 4.0 DSC-Updates in KB 3000850](#BKMK_3000850)

-   [Neue Funktionen in Windows PowerShell 5.0](#BKMK_new50)

-   [Neue Funktionen in Windows PowerShell 4.0](#BKMK_wps4)

-   [Neue Funktionen in Windows PowerShell 3.0](#BKMK_wps3)

## <a name="BKMK_3000850"></a>Windows PowerShell 4.0-Updates im Updaterollup vom November 2014 (KB 3000850)
Viele Updates und Verbesserungen für Windows PowerShell DSC (Desired State Configuration, Konfiguration für den gewünschten Zustand) in Windows PowerShell 4.0 stehen im [Updaterollup vom November 2014 für Windows RT 8.1, Windows 8.1 und Windows Server 2012 R2](https://support.microsoft.com/kb/3000850/) (KB 3000850) zur Verfügung. Sie können feststellen, ob KB 3000850 auf Ihrem System installiert ist, indem Sie `Get-Hotfix -Id KB3000850` in Windows PowerShell ausführen.

-   Aktualisierungen vorhandener Cmdlets im Modul [PSDesiredStateConfiguration](https://technet.microsoft.com/library/dn391651(v=wps.640).aspx)

    -   [Get-DscResource](https://technet.microsoft.com/library/dn521625.aspx) ist schneller (insbesondere in der ISE).

    -   [Start-DscConfiguration](https://technet.microsoft.com/library/dn521623.aspx) hat den neuen Parameter „– UseExisting“, der die zuletzt angewendete Konfiguration erneut anwendet.

    -   Der [Start-DscConfiguration](https://technet.microsoft.com/library/dn521623.aspx)-Parameter „-Force“ wurde korrigiert.

    -   [Get-DscLocalConfigurationManager](https://technet.microsoft.com/library/dn407378.aspx) zeigt nützlichere Informationen zum Modulzustand.

    -   [Test-DscConfiguration](https://technet.microsoft.com/library/dn407382.aspx) gibt nun den Namen des Computers zusammen mit „True“ oder „False“ zurück.

    -   [New-DscChecksum](https://technet.microsoft.com/library/dn521622.aspx) unterstützt jetzt UNC-Pfade.

-   Neue Cmdlets im Modul [PSDesiredStateConfiguration](https://technet.microsoft.com/library/dn391651(v=wps.640).aspx)

    -   [Update-DscConfiguration](https://technet.microsoft.com/library/mt143541(v=wps.630).aspx): Führt eine bedarfsgesteuerte Überprüfung des Pullservers durch.

    -   [Stop-DscConfiguration](https://technet.microsoft.com/library/mt143542(v=wps.630).aspx): Beendet eine Konfiguration, die bereits ausgeführt wird.

    -   [Remove-DscConfigurationDocument](https://technet.microsoft.com/library/mt143544(v=wps.630).aspx): Ermöglicht das Entfernen von Konfigurationsdokumenten in verschiedenen Phasen (ausstehend, vorherig oder aktuell).

-   Spracherweiterungen

    -   „DependsOn“ unterstützt jetzt zusammengesetzte Ressourcen.

    -   „DependsOn“ unterstützt jetzt Zahlen in Namen von Ressourceninstanzen.

    -   Knotenausdrücke, die als leer ausgewertet werden, lösen keine Fehler mehr aus.

    -   Ein Fehler, der auftritt, wenn ein Knotenausdruck als leer ausgewertet wird, wurde behoben.

    -   Konfigurationen, die Konfigurationen aufrufen, funktionieren nun in der Windows PowerShell-Konsole.

-   Verbesserungen beim Pullmodus

    -   Der Pullmodus unterstützt jetzt alle ZIP-Dateien.

    -   **AllowModuleOverwrite** funktioniert jetzt ordnungsgemäß.

-   Verbesserungen der Resilienz

    -   Der neue **DebugMode** ermöglicht das erneute Laden von Ressourcenmodulen.

    -   Wenn ein Konfigurationsfehler auftritt, wird die Datei „pending.mof“ nicht gelöscht.

    -   Der lokale Konfigurations-Manager (Local Configuration Manager, LCM) arbeitet jetzt stabiler, wenn Metakonfigurationseinstellungen beschädigt wurden.

-   Verbesserungen bei der Diagnose

    -   Eine Warnung wird angezeigt, wenn der LCM den Zeitgeber auf andere Einstellungen festlegt, als von Ihnen angegeben wurden.

    -   Fehlerprotokolldateien enthalten jetzt die Aufrufliste für Windows PowerShell-Ressourcen.

-   Verbesserungen bei der Flexibilität

    -   Die Ressource „LocalConfigurationManager“ hat die neue Eigenschaft **ActionAfterReboot**.

        -   ContinueConfiguration (Standardwert): Setzt eine Konfiguration automatisch fort, nachdem ein Zielknoten neu gestartet wurde.

        -   StopConfiguration: Setzt eine Konfiguration nicht automatisch fort, nachdem ein Knoten neu gestartet wurde.

    -   Eine Konsistenzprüfung kann nun häufiger als ein Pullvorgang erfolgen oder umgekehrt.

    -   Unterstützung der Versionsverwaltung: DSC kann nun ein Dokument erkennen, das auf einem neueren Client generiert wurde (enthalten in [WMF 5.0](https://aka.ms/wmf5download)).

-   Verbesserungen bei der Fehlervermeidung

    -   Die Version des Moduls wird nun erzwungen, bevor eine Konfiguration angewendet wird.

    -   **DebugPreference** ist jetzt ordnungsgemäß für Aufrufe von „Get-, Set- oder Test-TargetResource“ festgelegt.

-   Verbesserungen bei der Behandlung von Anmeldeinformationen

    -   Ein Zertifikat wird nun verwendet, wenn sowohl **Certificate** als auch **PSDscAllowPlainTextPassword** angegeben werden.

    -   Anmeldeinformationen werden auch für „Get-TargetResource“ entschlüsselt.

    -   Anmeldeinformationen für Metakonfigurationen werden ver- und entschlüsselt.

    -   „PSCredentials“ werden nun entschlüsselt, wenn sie in einem eingebetteten Objekt enthalten sind.

-   Verbesserungen bei integrierten Ressourcen

    -   Die Ressource „Package“

        -   Installiert nicht mehr das falsche Paket (entweder aus lokalen oder Webquellen).

        -   Unterstützt nun HTTPS.

    -   Die [Ressource „Package“](https://technet.microsoft.com/library/dn282132.aspx) unterstützt jetzt HTTPS.

    -   Die [Ressource „Archive“](https://technet.microsoft.com/library/dn249917.aspx) unterstützt jetzt Anmeldeinformationen.

## <a name="BKMK_new50"></a>Neue Features in Windows PowerShell 5.0

-   [Neue Funktionen in Windows PowerShell](#BKMK_newcore)

-   [Neue Funktionen in Windows PowerShell DSC](#BKMK_newDSC)

-   [Neue Funktionen in Windows PowerShell ISE](#BKMK_newISE)

-   [Neue Funktionen in Windows PowerShell-Webdienste](#BKMK_newOData)

-   [Wichtige Fehlerbehebungen in Windows PowerShell 5.0](#BKMK_5bugfix)

### <a name="BKMK_newcore"></a>Neue Features in Windows PowerShell

-   Ab Windows PowerShell 5.0 ist eine Entwicklung mithilfe von Klassen möglich, indem eine mit anderen objektorientierten Programmiersprachen vergleichbare formale Syntax und Semantik verwendet wird. **Class**, **Enum** und andere Schlüsselwörter wurden der Windows PowerShell-Sprache hinzugefügt, um das neue Feature zu unterstützen. Weitere Informationen zum Arbeiten mit Klassen finden Sie unter „about_Classes“.

-   Windows PowerShell 5.0 führt einen neuen, strukturierten Informationsdatenstrom ein, mit dessen Hilfe Sie strukturierte Daten zwischen einem Skript und seinen Aufrufern (oder seiner Hostingumgebung) übertragen können. Sie können nun „Write-Host“ zum Übergeben der Ausgabe an den Informationsdatenstrom nutzen. Informationsdatenströme funktionieren auch für „PowerShell.Streams“, Aufträge, geplante Aufträge und Workflows. Die folgenden Features unterstützen den Informationsdatenstrom.

    -   Das neue Cmdlet „Write-Information“ ermöglicht Ihnen das Angeben, wie Windows PowerShell Daten in einem Informationsdatenstrom für einen Befehl verarbeitet. „Write-Host“ ist ein Wrapper für „Write-Informationen“. „Write-Information“ ist auch eine unterstützte Workflowaktivität.

    -   Mit den beiden neuen allgemeinen Parametern, „InformationVariable“ und „InformationAction“, können Sie festlegen, wie Informationsdatenströme von einem Befehl angezeigt werden. Gültige Werte für „InformationAction“ sind „SilentlyContinue“, „Stop“, „Continue“, „Inquire“, „Ignore“ oder „Suspend“. „SilentlyContinue“ ist der Standardwert. „InformationVariable“ gibt eine Zeichenfolge als Namen einer Variablen an, in der die „Write-Host“-Daten von einem Befehl gespeichert werden sollen.

    -   Die neue Einstellungsvariable „InformationPreference“ gibt Ihre Standardeinstellung für Daten in einem Informationsdatenstrom in einer Windows PowerShell-Sitzung an. Der Standardwert ist „SilentlyContinue“.

    -   Die beiden neuen allgemeinen Workflowparameter „PSInformation“ und „InformationAction“ wurden hinzugefügt.

    -   Wenn Sie den Befehl „Format-Table“ verwenden, werden Spalten jetzt automatisch formatiert, indem die ersten 300 ms Daten ausgewertet werden, die den Datenstrom durchlaufen.

-   In Zusammenarbeit mit [Microsoft Research](https://research.microsoft.com/) wurde das neue Cmdlet „ConvertFrom-String“ hinzugefügt. „ConvertFrom-String“ ermöglicht das Extrahieren und Analysieren strukturierter Objekte aus dem Inhalt von Textzeichenfolgen. Weitere Informationen finden Sie unter „ConvertFrom-String“.

-   Das neue Cmdlet „Convert-String“ formatiert Text automatisch basierend auf einem Beispiel, das Sie in einem „-Example“-Parameter angeben.

-   Das neue Modul „Microsoft.PowerShell.Archive“ bietet Cmdlets, mit denen Sie Dateien und Ordner in Archivdateien (.zip) komprimieren, Dateien aus vorhandenen ZIP-Dateien extrahieren und ZIP-Dateien mit neueren Versionen von Dateien aktualisieren können, die in ihnen komprimiert sind.

-   Mit dem neuen Modul „PackageManagement“ können Sie Softwarepakete im Internet ermitteln und installieren. Das Modul „PackageManagement“ (zuvor als OneGet bezeichnet) dient zur Verwaltung vorhandener Paket-Manager (die auch als Paketanbieter bezeichnet werden) und zur Vereinheitlichung der Verwaltung von Windows-Paketen mithilfe einer einzigen Windows PowerShell-Schnittstelle.

-   Mit dem neuen Modul „PowerShellGet“ können Sie Module und DSC-Ressourcen im [PowerShell-Katalog](https://www.powershellgallery.com/) finden, installieren, veröffentlichen und aktualisieren. Dies ist auch in einem internen Modulrepository möglich, das Sie durch Ausführen des Cmdlets „Register-PSRepository“ einrichten können.

-   Das neue Schlüsselwort **Hidden** wurde hinzugefügt, um anzugeben, dass ein Member (eine Eigenschaft oder Methode) nicht standardmäßig in Ergebnissen von „Get-Member“ angezeigt wird (es sei denn, Sie fügen den Parameter „-Force“ hinzu). Eigenschaften oder Methoden, die als ausgeblendet markiert wurden, werden auch nicht in IntelliSense-Ergebnissen angezeigt. Dies gilt nur, wenn Sie sich in einem Kontext befinden, in dem der Member sichtbar sein soll. Die automatische Variable „$This“ soll beispielsweise ausgeblendete Member zeigen, wenn sie in der Klassenmethode enthalten ist.

-   „New-Item“, „Remove-Item“ und „Get-ChildItem“ wurden verbessert, um das Erstellen und Verwalten [symbolischer Verknüpfungen](https://en.wikipedia.org/wiki/Symbolic_link) zu unterstützen. Der **ItemType**-Parameter für „New-Item“ akzeptiert den neuen Wert **SymbolicLink**. Nun können Sie symbolische Verknüpfungen in einer einzigen Zeile erstellen, indem Sie das Cmdlet „New-Item“ ausführen.

-   „Get-ChildItem“ hat auch den neuen „–Depth“-Parameter, den Sie mit dem „–Recurse“-Parameter verwenden, um die Rekursion zu beschränken. Beispiel: „Get-ChildItem –Recurse –Depth 2“ gibt Ergebnisse aus dem aktuellen Ordner, alle untergeordneten Ordner im aktuellen Ordner und alle Ordner innerhalb der untergeordneten Ordner zurück.

-   „Copy-Item“ ermöglicht Ihnen nun das Kopieren von Dateien oder Ordnern aus einer Windows PowerShell-Sitzung in eine andere. Das bedeutet, dass Sie Dateien in Sitzungen kopieren können, die mit Remotecomputern verbunden sind (einschließlich Computern mit [Nano Server](https://blogs.technet.com/b/windowsserver/archive/2015/04/08/microsoft-announces-nano-server-for-modern-apps-and-cloud.aspx), die keine andere Schnittstelle haben). Zum Kopieren von Dateien geben Sie „PSSession“-IDs als Wert für die neuen Parameter „FromSession“ und „ToSession“ ein. Fügen Sie „–Path“ und „–Destination“ hinzu, um den Ursprungspfad und das Ziel anzugeben. Z. B. Copy-Item-Pfad c:\\myFile.txt - ToSession $s-Ziel d:\\destinationFolder.

-   Die Windows PowerShell-Aufzeichnung wurde verbessert und gilt nun für alle Hostinganwendungen (z. B. Windows PowerShell ISE) anstatt nur für den Konsolenhost (**powershell.exe**). Aufzeichnungsoptionen (einschließlich einer systemweiten Aufzeichnung) können durch Aktivieren der Gruppenrichtlinieneinstellung **PowerShell-Aufzeichnung aktivieren** („Administrative Vorlagen“/„Windows-Komponenten“/„Windows PowerShell“) konfiguriert werden.

-   Ein neues Feature zur detaillierten Ablaufverfolgung von Skripts ermöglicht eine detaillierte Nachverfolgung und Analyse der Verwendung von Windows PowerShell-Skripts auf einem System. Nachdem Sie die detaillierte Ablaufverfolgung von Skripts aktiviert haben, protokolliert Windows PowerShell alle Skriptblöcke im ETW-Ereignisprotokoll **Microsoft-Windows-PowerShell/Operational**.

-   Ab Windows PowerShell 5.0 unterstützen die neuen CMS-Cmdlets (Cryptographic Message Syntax) die Ver- und Entschlüsselung von Inhalten mithilfe des IETF-Standardformats für kryptografisch geschützte Nachrichten, wie unter [RFC5652](https://tools.ietf.org/html/rfc5652) dokumentiert. Die Cmdlets „Get-CmsMessage“, „Protect-CmsMessage“ und „Unprotect-CmsMessage“ wurden dem Modul [Microsoft.PowerShell.Security](https://technet.microsoft.com/library/hh849807.aspx) hinzugefügt.

-   Neue Cmdlets im Modul [Microsoft.PowerShell.Utility](https://technet.microsoft.com/library/hh849958.aspx) (Get-Runspace, Debug-Runspace, Get-RunspaceDebug, Enable-RunspaceDebug, Disable-RunspaceDebug), ermöglichen das Festlegen von Debugoptionen für einen Runspace sowie das Starten und Beenden des Debuggens in einem Runspace. Für das Debuggen beliebiger Runspaces, d.h. Runspaces, die nicht der Standardrunspace für eine Windows PowerShell-Konsole oder Windows PowerShell-ISE-Sitzung sind, ermöglicht Windows PowerShell, dass Sie Haltepunkte setzen und veranlassen, dass hinzugefügte Haltepunkte die Ausführung des Skripts bewirken, bis Sie einen Debugger zum Debuggen des Runspaceskripts anfügen können. Dem Windows PowerShell-Skriptdebugger für Runspaces wurde Unterstützung für das geschachtelte Debugging für beliebige Runspaces hinzugefügt.

-   Das Cmdlet „Format-Hex“ wurde dem Modul [Microsoft.PowerShell.Utility](https://technet.microsoft.com/library/hh849958.aspx) hinzugefügt. Mit Format-Hex- können Sie Text- und Binärdaten im Hexadezimalformat anzeigen.

-   Die Cmdlets „Get-Clipboard“ und „Set-Clipboard“ wurden dem Modul [Microsoft.PowerShell.Utility](https://technet.microsoft.com/library/hh849958.aspx) hinzugefügt. Sie vereinfachen die Übertragung von Inhalten in und aus einer Windows PowerShell-Sitzung. Die „Clipboard“-Cmdlets unterstützen Bilder, Audiodateien, Dateilisten und Text.

-   Das Cmdlet „Clear-RecycleBin“ wurde dem Modul [Microsoft.PowerShell.Utility](https://technet.microsoft.com/library/hh849827(v=wps.640).aspx) hinzugefügt. Es dient zum Leeren des Papierkorbs für ein Festplattenlaufwerk, wozu auch externe Laufwerke zählen. Standardmäßig werden Sie aufgefordert, einen „Clear-RecycleBin“-Befehl zu bestätigen, da die „ConfirmImpact“-Eigenschaft des Cmdlets auf „ConfirmImpact.High“ festgelegt ist.

-   Das neue Cmdlet „New-TemporaryFile“ ermöglicht Ihnen während der Skripterstellung das Anlegen einer temporären Datei. Standardmäßig wird der neue temporäre Datei in ```C:\Users\<user name>\AppData\Local\Temp``` erstellt.

-   Die Cmdlets „Out-File“, „Add-Content“ und „Set-Content“ haben nun den neuen Parameter „–NoNewline“, der eine neue Zeile hinter der Ausgabe weglässt.

-   Das Cmdlet „New-Guid“ nutzt die .NET Framework-Klasse „Guid“ zum Generieren einer GUID, die beim Schreiben von Skripts oder DSC-Ressourcen nützlich ist.

-   Da Dateiversionsinformationen irreführend sein können, insbesondere, wenn eine Datei mit einem Patch versehen wurde, sind die neuen Skripteigenschaften „FileVersionRaw“ und „ProductVersionRaw“ für „FileInfo“-Objekte verfügbar. Beispielsweise führen Sie den folgenden Befehl zur Anzeige der Werte dieser Eigenschaften für powershell.exe $pid enthält, in denen die Prozess-ID für eine ausgeführte Windows PowerShell-Sitzung:  ```Get-Process -Id $pid -FileVersionInfo | Format-List *version* -Force```

-   Die neuen Cmdlets „Enter-PSHostProcess“ und „Exit-PSHostProcess“ ermöglichen das Debuggen von Windows PowerShell-Skripts in vom aktuellen Prozess getrennten Prozessen, der in der Windows PowerShell-Konsole ausgeführt wird. Führen Sie „Enter-PSHostProcess“ zum Eingeben einer oder Anfügen an eine bestimmte Prozess-ID und dann „Get-Runspace“ zum Zurückgeben der aktiven Runspaces innerhalb des Prozesses aus. Führen Sie „Exit-PSHostProcess“ zum Trennen vom Prozess aus, wenn Sie mit dem Debuggen des Skripts innerhalb des Prozesses fertig sind.

-   Das Cmdlet „Wait-Debugger“ wurde dem Modul [Microsoft.PowerShell.Utility](https://technet.microsoft.com/library/hh849958.aspx) hinzugefügt. Sie können „Wait-Debugger“ ausführen, um ein Skript im Debugger zu beenden, bevor die nächste Anweisung im Skript ausgeführt wird.

-   Der Windows PowerShell-Workflow-Debugger unterstützt nun die Vervollständigung mit der TAB-TASTE und das Debuggen geschachtelter Workflowfunktionen. Sie können jetzt **STRG+PAUSE** drücken, um den Debugger in einem ausgeführten Skript, in lokalen und Remotesitzungen und in einem Workflowskript zu öffnen.

-   Das Cmdlet „Debug-Job“ wurde dem Modul [Microsoft.PowerShell.Core](https://technet.microsoft.com/library/hh849695.aspx) hinzugefügt, um ausgeführte Auftragsskripts zu debuggen, die für Windows PowerShell-Workflowaufträge, Hintergrundaufträge und in Remotesitzungen ausgeführte Aufträge ausgeführt werden.

-   Der neue Status „AtBreakpoint“ wurde für Windows PowerShell-Aufträge hinzugefügt. Der Status „AtBreakpoint“ gilt, wenn ein Auftrag in einem Skript ausgeführt wird, das gesetzte Haltepunkte enthält, und das Skript einen Haltepunkt erreicht hat. Wenn ein Auftrag an einem Haltepunkt angehalten wird, müssen Sie den Auftrag durch Ausführen des Cmdlets „Debug-Job“ debuggen.

-   Windows PowerShell 5.0 implementiert in „$PSModulePath“ Unterstützung für mehrere Versionen eines einzelnen Windows PowerShell-Moduls in demselben Ordner. Die „RequiredVersion“-Eigenschaft wurde der „ModuleSpecification“-Klasse hinzugefügt, um Sie beim Abrufen der gewünschten Version zu unterstützen. Diese Eigenschaft und die „ModuleVersion“-Eigenschaft schließen sich gegenseitig aus. „RequiredVersion“ wird nun als Teil des Werts des „FullyQualifiedName“-Parameters der Cmdlets „Get-Module“, „Import-Module“ und „Remove-Module“ unterstützt.

-   Durch Ausführen des Cmdlets „Test-ModuleManifest“ können Sie jetzt die Gültigkeit der Modulversion überprüfen.

-   Ergebnisse des Cmdlets „Get-Command“ enthalten nun die Spalte „Version“. Der „CommandInfo“-Klasse wurde die neue „Version“-Eigenschaft hinzugefügt. „Get-Command“ zeigt Befehle aus mehreren Versionen des gleichen Moduls. Die „Version“-Eigenschaft ist ebenfalls Teil der abgeleiteten Klassen von „CmdletInfo“ und „ApplicationInfo“.

-   „Get-Command“ hat den neuen Parameter „-ShowCommandInfo“, der „ShowCommand“-Informationen als „PSObjects“ zurückgibt. Dies ist besonders nützlich für die Ausführung von „Show-Command“ in der Windows PowerShell-ISE mithilfe von Windows PowerShell-Remoting. Der Parameter „–ShowCommandInfo“ ersetzt die vorhandene „Get-SerializedCommand“-Funktion im Modul „Microsoft.PowerShell.Utility“. Doch das „Get-SerializedCommand“-Skript ist weiterhin zur Unterstützung von Skripts unter einer Vorgängerversion verfügbar.

-   Das neue Cmdlet „Get-ItemPropertyValue“ ermöglicht das Abrufen des Werts einer Eigenschaft ohne Verwendung der Punktnotation. Führen Sie z. B. in älteren Versionen von Windows PowerShell den folgenden Befehl aus, um den Wert der Eigenschaft Anwendungsbasis der Registrierungsschlüssel "powershellengine" abzurufen: **(Get-ItemProperty-Pfad HKLM:\\SOFTWARE\\Microsoft\\PowerShell\\3\\PowerShellEngine-Namen ApplicationBase). ApplicationBase**. Ab Windows PowerShell 5.0, Sie können ausführen **Get-ItemPropertyValue-Pfad HKLM:\\SOFTWARE\\Microsoft\\PowerShell\\3\\PowerShellEngine-Namen ApplicationBase**.

-   In der Windows PowerShell-Konsole werden jetzt wie in der Windows PowerShell ISE Syntaxfarben verwendet.

-   Das neue NetworkSwitch-Modul enthält Cmdlets, die es Ihnen ermöglichen, Netzwerkswitches, die mit dem Windows Server 2012 R2-Logo zertifiziert sind, mit einer Switch-, VLAN- und grundlegenden Schicht-2-Netzwerkswitch-Konfiguration zu versehen.

-   Der „FullyQualifiedName“-Parameter wurde den Cmdlets „Import-Module“ und „Remove-Module“ hinzugefügt, um die Speicherung mehrerer Versionen eines einzelnen Moduls zu unterstützen.

-   „Save-Help“, „Update-Help“, „Import-PSSession“, „Export-PSSession“ und „Get-Command“ haben den neuen Parameter „FullyQualifiedModule“ des Typs „ModuleSpecification“. Fügen Sie diesen Parameter hinzu, um ein Modul mit seinem vollqualifizierten Namen anzugeben.

-   Der Wert von **$PSVersionTable.PSVersion** wurde auf 5.0 aktualisiert.

### <a name="BKMK_newDSC"></a>Neue Features in Windows PowerShell DSC

-   Verbesserungen der Windows PowerShell-Sprache ermöglichen Ihnen das Definieren von Windows PowerShell-DSC-Ressourcen mithilfe von Klassen. Import-DscResource ist jetzt ein echtes dynamisches Schlüsselwort. Windows PowerShell analysiert das Stammmodul des angegebenen Moduls und sucht Klassen, die das DscResource-Attribut enthalten. Sie können jetzt Klassen verwenden, um DSC-Ressourcen zu definieren, bei denen weder eine MOF-Datei noch ein Unterordner des Typs „DSCResource“ im Modulordner erforderlich ist. Eine Windows PowerShell-Moduldatei kann mehrere DSC-Ressourcenklassen enthalten.

-   Den folgenden Cmdlets im Modul „PSDesiredStateConfiguration“ wurde der neue Parameter „ThrottleLimit“ hinzugefügt. Fügen Sie den „ThrottleLimit“-Parameter zum Angeben der Anzahl der Zielcomputer oder -geräte hinzu, auf die der Befehl gleichzeitig angewendet werden soll.

    -   Get-DscConfiguration

    -   Get-DscConfigurationStatus

    -   Get-DscLocalConfigurationManager

    -   Restore-DscConfiguration

    -   Test-DscConfiguration

    -   Compare-DscConfiguration

    -   Publish-DscConfiguration

    -   Set-DscLocalConfigurationManager

    -   Start-DscConfiguration

    -   Update-DscConfiguration

-   Bei der zentralen DSC-Fehlerberichterstattung werden ausführliche Fehlerinformationen nicht nur im Ereignisprotokoll protokolliert, sondern können auch zur späteren Analyse an einen zentralen Speicherort gesendet werden. Sie können diesen zentralen Speicherort verwenden, um DSC-Konfigurationsfehler zu speichern, die für alle Server in ihrer Umgebung aufgetreten sind. Nachdem der Berichtsserver in der Metakonfiguration definiert wurde, werden alle Fehler an den Berichtsserver gesendet und anschließend in einer Datenbank gespeichert. Sie können diese Funktion unabhängig davon einrichten, ob der Zielknoten zum Abrufen von Konfigurationen von einem Pullserver konfiguriert ist.

-   Verbesserungen an der Windows PowerShell-ISE vereinfachen das Erstellen von DSC-Ressourcen. Sie haben nun die folgenden Möglichkeiten:

    -   Auflisten aller DSC-Ressourcen in einem **configuration**- oder **node**-Block durch Drücken von **STRG+LEERTASTE** in einer leeren Zeile innerhalb des Blocks.

    -   Automatische Vervollständigung für Ressourceneigenschaften mit dem **enumeration**-Typ.

    -   Automatische Vervollständigung für die **DependsOn**-Eigenschaft von DSC-Ressourcen basierend auf anderen Ressourceninstanzen in der Konfiguration.

    -   Bessere Vervollständigung mit der TAB-TASTE von Eigenschaftswerten von Ressourcen.

-   Benutzer können jetzt eine Ressource unter einem angegebenen Satz von Anmeldeinformationen ausführen, indem das **PSDscRunAsCredential**-Attribut einem „Node“-Block hinzugefügt wird. Z. B. PSDscRunAsCredential = Get-Credential-Contoso\\DscUser. Diese Funktion ist nützlich zum Erstellen von Konfigurationen, die Windows Installer und ausführbare Installationsprogramme ausführen, auf die benutzerbezogene Registrierungsstruktur zugreifen oder andere Aufgaben außerhalb des aktuellen Benutzerkontexts ausführen.

-   x86-basierte 32-Bit-Unterstützung wurde für das **Configuration**-Schlüsselwort hinzugefügt.

-   Windows PowerShell bietet nun Unterstützung für benutzerdefinierte Hilfe für DSC-Konfigurationen, die durch Hinzufügen von „[CmdletBinding()]“ zur generierten Konfigurationsfunktion definiert wird.

-   Das neue **DscLocalConfigurationManager**-Attribut kennzeichnet einen Konfigurationsblock als Metakonfiguration, die zum Konfigurieren des lokalen Konfigurations-Managers von DSC verwendet wird. Dieses Attribut beschränkt eine Konfiguration auf ausschließlich Elemente, die zum Konfigurieren des lokalen Konfigurations-Managers von DSC dienen. Diese Konfiguration während der Verarbeitung generiert eine \*.meta.mof-Datei, die dann an die entsprechenden Knoten durch Ausführen des Cmdlets Set-DscLocalConfigurationManager gesendet wird.

-   Teilkonfigurationen sind jetzt in Windows PowerShell 5.0 zulässig. Sie können Konfigurationsdokumente in Fragmenten an einen Knoten übermitteln. Damit ein Knoten mehrere Fragmente eines Konfigurationsdokuments empfangen kann, muss der lokale Konfigurations-Manager des Knotens für das Angeben der erwarteten Fragmente eingerichtet sein.

-   Die computerübergreifende Synchronisierung in DSC in Windows PowerShell 5.0 ist neu. Mithilfe der integrierten WaitFor\ * Ressourcen (**WaitForAll**, **WaitForAny**, und **WaitForSome**), können Sie jetzt Abhängigkeiten auf Computern während der Konfiguration ausgeführt wird, ohne externe Orchestrierungen angeben. Diese Ressourcen ermöglichen eine Synchronisierung zwischen Knoten mithilfe von CIM-Verbindungen über das Protokoll „WS-Man“. Eine Konfiguration kann warten, bis sich der Zustand einer bestimmten Ressource eines anderen Computers geändert hat.

-   Just Enough Administration (JEA), ein neues Delegierungssicherheitsfeature, nutzt DSC und von Windows PowerShell eingeschränkte Runspaces, um Unternehmen vor (absichtlichen oder unabsichtlichen) Datenverlusten oder Gefährdungen durch Mitarbeiter zu schützen. Weitere Informationen zu JEA, so z. B. zum Herunterladen der xJEA DSC-Ressource, finden Sie unter [Just Enough Administration, Step by Step](https://blogs.technet.com/b/privatecloud/archive/2014/05/14/just-enough-administration-step-by-step.aspx) (Just Enough Administration, Schritt für Schritt).

-   Dem Modul „PSDesiredStateConfiguration“ wurden die folgenden neuen Cmdlets hinzugefügt.

    -   Das neue Cmdlet „Get-DscConfigurationStatus“ ruft allgemeine Informationen zum Konfigurationsstatus von einem Knoten ab. Sie erhalten den Status der letzten oder aller Konfigurationen.

    -   Das neue Cmdlet „Compare-DscConfiguration“ vergleicht eine angegebene Konfiguration mit dem tatsächlichen Status eines oder mehrerer Zielknoten.

    -   Das neue Cmdlet „Publish-DscConfiguration“ kopiert eine MOF-Konfigurationsdatei auf einen Zielknoten, ohne die Konfiguration anzuwenden. Diese Konfiguration wird während des nächsten Konsistenzdurchlaufs oder bei Ausführen des Cmdlets „Update-DscConfiguration“ angewendet.

    -   Mit dem neuen Cmdlet „Test-DscConfiguration“ können Sie überprüfen, ob eine resultierende Konfiguration der gewünschten Konfiguration entspricht. Zurückgegeben wird entweder „true“, wenn die Konfiguration der gewünschten Konfiguration entspricht, oder „false“, wenn die tatsächliche Konfiguration nicht mit der gewünschten Konfiguration übereinstimmt.

    -   Das neue Cmdlet „Update-DscConfiguration“ erzwingt die Verarbeitung einer Konfiguration. Wenn sich der lokale Konfigurations-Managers im Pullmodus befindet, ruft das Cmdlet die Konfiguration vom Pullserver ab, bevor diese angewendet wird.

### <a name="BKMK_newISE"></a>Neue Features in der Windows PowerShell ISE

-   Sie können jetzt Windows PowerShell-Remoteskripts und Dateien in einer lokalen Kopie der Windows PowerShell-ISE bearbeiten, indem Sie „Enter-PSSession“ ausführen, um eine Remotesitzung auf dem Computer zu starten, auf dem die Dateien gespeichert sind, die Sie bearbeiten möchten, und dann **PSEdit<path and file name on the remote computer>** ausführen. Dieses Feature erleichtert die Bearbeitung von Windows PowerShell-Dateien, für die Server Core-Installationsoption von Windows Server gespeichert sind, bei der Windows PowerShell-ISE nicht ausgeführt werden kann.

-   Das Cmdlet „Start-Transcript“ wird jetzt in der Windows PowerShell-ISE unterstützt.

-   Sie können Remoteskripts nun in der Windows PowerShell-ISE debuggen.

-   Der neue Menübefehl **Alle unterbrechen** (STRG+B) unterbricht den Debugger bei lokal und remote ausgeführten Skripts.

### <a name="BKMK_newOData"></a>Neue Features in Windows PowerShell-Webdienste (Verwaltung der OData-IIS-Erweiterung)

-   Ab Windows PowerShell 5.0 können Sie einen Satz von Windows PowerShell-Cmdlets basierend auf den Funktionen generieren, die von einem bestimmten OData-Endpunkt verfügbar gemacht werden, indem Sie das Cmdlet „Export-ODataEndpointProxy“ im neuen Modul [Microsoft.PowerShell.OdataUtils](https://technet.microsoft.com/library/dn818507(v=wps.640).aspx) ausführen.

### <a name="BKMK_5bugfix"></a>Wichtige Fehlerbehebungen in Windows PowerShell 5.0

-   Windows PowerShell 5.0 bietet eine neue COM-Implementierung, die bei der Arbeit mit COM-Objekten erhebliche Leistungssteigerungen bietet. Eine dazugehörige Videodemonstration finden Sie unter [Com_Perf_Improvements](https://1drv.ms/1qu3UPZ).

-   Beträchtliche Leistungssteigerungen wurden bei der ersten Vervollständigung mit der TAB-TASTE in einer Windows PowerShell-Sitzung erreicht, deren Dauer um knapp 500 ms verkürzt wurde.

## <a name="BKMK_wps4"></a>Neue Features in Windows PowerShell 4.0
Windows PowerShell 4.0 ist abwärtskompatibel. Cmdlets, Anbieter, Module, Snap-Ins, Skripts, Funktionen und Profile, die für Windows PowerShell 3.0 und Windows PowerShell 2.0 entwickelt wurden, funktionieren ohne Änderungen in Windows PowerShell 4.0.

Windows PowerShell 4.0 wird standardmäßig unter Windows® 8.1 und Windows Server 2012 R2 installiert. Zum Installieren von Windows PowerShell 4.0 unter Windows 7 mit SP1 oder Windows Server 2008 R2 müssen Sie [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) herunterladen und installieren. Achten Sie darauf, dass Sie die Details für das Herunterladen lesen und alle Systemanforderungen erfüllen, bevor Sie Windows Management Framework 4.0 installieren.

-   [Neue Funktionen in Windows PowerShell](#BKMK_core)

-   [Neue Funktionen in Windows PowerShell Integrated Scripting Environment (ISE)](#BKMK_ise)

-   [Neue Funktionen in Windows PowerShell-Workflow](#BKMK_workflow)

-   [Neue Funktionen in Windows PowerShell-Webdienste](#BKMK_psws)

-   [Neue Funktionen in Windows PowerShell Web Access](#BKMK_powwa)

-   [Wichtige Fehlerbehebungen in Windows PowerShell 4.0](#BKMK_bugs)

Windows PowerShell 4.0 bietet die folgenden neuen Features.

### <a name="BKMK_core"></a>Neue Features in Windows PowerShell

-   **Windows PowerShell DSC** (Desired State Configuration, Konfiguration für den gewünschten Zustand) ist ein neues Verwaltungssystem in Windows PowerShell 4.0, mit dem die Bereitstellung und Verwaltung von Konfigurationsdaten für Softwaredienste und die Umgebung ermöglicht wird, in der diese Dienste ausgeführt werden. Weitere Informationen zu DSC finden Sie unter [Erste Schritte mit Windows PowerShell DSC](https://technet.microsoft.com/en-us/library/c134aa32-b085-4656-9a89-955d8ff768d0).

-   Sie können nun mit **Save-Help** die Hilfe für Module speichern, die auf Remotecomputern installiert sind. Sie können Save-Help verwenden, um die Modulhilfe auf einen mit dem Internet verbundenen Client (auf dem möglicherweise nicht alle Module installiert sind, für die Sie Hilfe benötigen) herunterzuladen, und anschließend die gespeicherte Hilfe in einen freigegebenen Remoteordner oder auf einen Remotecomputer, der nicht über einen Internetzugang verfügt, zu kopieren.

-   Der Windows PowerShell-Debugger wurde insofern verbessert, dass das Debuggen von Windows PowerShell-Workflows und von Skripts ermöglicht wird, die auf Remotecomputern ausgeführt werden. Windows PowerShell-Workflows können nun auf Skriptebene über die Windows PowerShell-Befehlszeile oder über Windows PowerShell ISE gedebuggt werden. Windows PowerShell-Skripts, darunter auch Skriptworkflows, können nun über Remotesitzungen gedebuggt werden. Remotedebugsitzungen werden bei Windows PowerShell-Remotesitzungen beibehalten, die getrennt werden und später erneut eine Verbindung aufbauen.

-   Durch einen **RunNow**-Parameter für **Register-ScheduledJob** und **Set-ScheduledJob** entfällt das Festlegen eines sofortigen Startdatums und einer Uhrzeit für Aufträge, indem der **Trigger**-Parameter verwendet wird.

-   Mit **Invoke-RestMethod** und **Invoke-WebRequest** können Sie nun alle Header festlegen, indem Sie den „Headers“-Parameter verwenden. Obwohl dieser Parameter schon immer vorhanden war, handelte es sich dabei um einen von vielen Parametern für die Web-Cmdlets, die Ausnahmen oder Fehler verursacht haben.

-   **Get-Module** wurde einen neuen Parameter **FullyQualifiedName**, des Typs **ModuleSpecification\ []**. Mit dem **FullyQualifiedName**-Parameter von „Get-Module“ können Sie nun ein Modul angeben, indem Sie den Modulnamen, die Version und optional die GUID angeben.

-   Die Standard-Ausführungsrichtlinieneinstellung in Windows Server 2012 R2 lautet **RemoteSigned**. Unter Windows 8.1 gibt es keine Änderung der Standardeinstellung.

-   Ab Windows PowerShell 4.0 wird der Methodenaufruf mithilfe dynamischer Methodennamen unterstützt. Sie können eine Variable zum Speichern eines Methodennamens verwenden, und anschließend die Methode dynamisch aufrufen, indem die Variable aufgerufen wird.

-   Asynchrone Workflowaufträge werden nicht mehr gelöscht, wenn das Timeoutintervall abgelaufen ist, das vom allgemeinen **PSElapsedTimeoutSec**-Workflowparameter angegeben wird.

-   Der neue Parameter **RepeatIndefinitely** wurde den Cmdlets **New-JobTrigger** und **Set-JobTrigger** hinzugefügt. Dadurch entfällt die Angabe eines **TimeSpan.MaxValue**-Werts für den **RepetitionDuration**-Parameter, damit ein geplanter Auftrag für eine unbestimmte Zeit wiederholt ausgeführt werden kann.

-   Der **Passthru**-Parameter wurde den Cmdlets **Enable-JobTrigger** und **Disable-JobTrigger** hinzugefügt. Der „Passthru“-Parameter zeigt alle Objekte an, die durch Ihren Befehl erstellt oder geändert wurden.

-   Die Parameternamen für das Angeben einer Arbeitsgruppe in den Cmdlets **Add-Computer** und **Remove-Computer** sind nun konsistent. Beide Cmdlets verwenden nun den Parameter **WorkgroupName**.

-   Der neue allgemeine Parameter **PipelineVariable** wurde hinzugefügt. Mit „PipelineVariable“ können Sie die Ergebnisse eines weitergeleiteten Befehls (oder Teil eines weitergeleiteten Befehls) als Variable speichern, die über den Rest der Pipeline übergeben werden kann.

-   Das Filtern einer Sammlung mithilfe einer Methodensyntax wird nun unterstützt. Dies bedeutet, Sie können eine Objektsammlung mithilfe einer vereinfachten Syntax filtern, ähnlich wie bei „Where()“ oder „Where-Object“, formatiert als Methodenaufruf. Es folgt ein Beispiel: (Get-Process).where({$_.Name -match 'powershell'})

-   Das **Get-Process**-Cmdlet hat den neuen Switch-Parameter **IncludeUserName**.

-   Das neue Cmdlet **Get-FileHash** wurde hinzugefügt, das für eine angegebene Datei einen Dateihash in einem von mehreren Formaten zurückgibt.

-   Wenn in Windows PowerShell 4.0 ein Modul den **DefaultCommandPrefix**-Schlüssel in seinem Manifest verwendet oder der Benutzer ein Modul mit dem **Prefix**-Parameter importiert, zeigt die **ExportedCommands**-Eigenschaft des Moduls die Befehle im Modul mit dem Präfix an. Wenn Sie die Befehle ausführen, mit der qualifizierten modulsyntax ModuleName\\CommandName, müssen die Befehlsnamen das Präfix enthalten.

-   Der Wert von **$PSVersionTable.PSVersion** wurde auf 4.0 aktualisiert.

-   Das Verhalten des **Where()**-Operators hat sich geändert. Dass `Collection.Where('property –match name')` einen Zeichenfolgeausdruck im Format `"Property –CompareOperator Value"` akzeptiert, wird nicht mehr unterstützt. Der **Where()**-Operator akzeptiert jedoch Zeichenfolgeausdrücke im Format eines Skriptblocks. Dies wird weiterhin unterstützt.

### <a name="BKMK_ise"></a>Neue Features in der Windows PowerShell Integrated Scripting Environment (ISE)

-   Windows PowerShell ISE unterstützt das Windows PowerShell Workflow-Debuggen und das Remoteskript-Debuggen.

-   Die IntelliSense-Unterstützung wurde für Windows PowerShell-DSC-Anbieter und -Konfigurationen hinzugefügt.

### <a name="BKMK_workflow"></a>Neue Features in Windows PowerShell Workflow

-   Für einen neuen allgemeinen **PipelineVariable**-Parameter wurde die Unterstützung im Kontext von iterativen Pipelines hinzugefügt, wie z.B. bei denen, die vom System Center Orchestrator verwendet werden. Dies sind Pipelines, die Befehle einfach von links nach rechts ausführen, im Gegensatz zur kombinierten Ausführung mithilfe von Streaming.

-   Die Parameterbindung wurde so verbessert, dass sie auch außerhalb von Szenarios zur Vervollständigung mit der TAB-TASTE funktioniert, wie z. B. bei Befehlen, die im aktuellen Runspace nicht vorhanden sind.

-   Die Unterstützung benutzerdefinierter Containeraktivitäten wurde Windows PowerShell Workflow hinzugefügt. Wenn ein Aktivitätsparameter Typen ist **Aktivität**, **aktivität\ []**– oder eine generische Auflistung von Aktivitäten wird – der Benutzer einen Skriptblock als Argument angegeben wurde, und Windows PowerShell Workflow konvertiert den Skriptblock in XAML wie bei normalen Windows PowerShell-Skript zum Workflow-Kompilierung.

-   Nach einem Absturz stellt Windows PowerShell Workflow automatisch eine neue Verbindung mit verwalteten Knoten her.

-   Sie können nun **Foreach -Parallel**-Aktivitätsanweisungen mithilfe der **ThrottleLimit**-Eigenschaft drosseln.

-   Der allgemeine **ErrorAction**-Parameter weist den neuen gültigen Wert **Suspend** auf, der für Workflows exklusiv ist.

-   Ein Workflowendpunkt schließt sich nun automatisch, wenn keine aktiven Sitzungen ausgeführt werden, keine in Bearbeitung befindlichen Aufträge und keine ausstehenden Aufträge vorhanden sind. Dieses Feature behält Ressourcen auf dem Computer bei, der als Workflowserver agiert, wenn die automatischen Schließbedingungen erfüllt wurden.

### <a name="BKMK_psws"></a>Neue Features in Windows PowerShell-Webdienste

-   Wenn in Windows PowerShell-Webdienste (PSWS, auch als Management OData IIS-Erweiterung bezeichnet) ein Fehler auftritt, während ein Cmdlet ausgeführt wird, werden detailliertere Fehlermeldungen an den Aufrufer zurückgegeben. Außerdem befolgen Fehlercodes [Richtlinien für Fehlercodes für die Microsoft Azure-REST-API](https://msdn.microsoft.com/library/windowsazure/dd179357.aspx).

-   Ein Endpunkt kann nun die API-Version definieren, sowie die Nutzung einer bestimmten API-Version erzwingen. Wenn Versionskonflikte zwischen Client und Server auftreten, werden auf dem Client und auf dem Server Fehler angezeigt.

-   Die Verwaltung des Dispatchschemas wurde vereinfacht, indem Werte für im Schema fehlende Felder automatisch generiert werden. Die Generierung erfolgt als ein hilfreicher Ausgangspunkt, selbst wenn das Dispatchschema nicht vorhanden ist.

-   Die Typenverarbeitung in PSWS wurde so verbessert, dass auch Typen unterstützt werden, die einen anderen als den Standardkonstruktor verwenden, indem sie sich ähnlich wie der **PSTypeConverter** in Windows PowerShell verhält. Auf diese Weise können Sie mit PSWS komplexe Typen verwenden.

-   Mit PSWS können Sie eine zugewiesene Instanz nun während der Ausführung einer Abfrage erweitern. Bei größeren binären Inhalten (z. B. Bildern, Audio oder Videos) sind die Übertragungskosten erheblich, und es ist besser, binäre Daten ohne Codierung zu übertragen. PSWS verwendet benannte Ressourcenstreams für die Übertragung ohne Codierung. Der benannte Ressourcendatenstrom ist eine Eigenschaft einer Entität des **Edm.Stream**-Typs. Jeder benannte Ressourcenstream verfügt über einen separaten URI für GET- oder UPDATE-Vorgänge.

-   OData -Aktionen bieten nun einen Mechanismus zum Aufrufen von nicht-CRUD-Methoden (Erstellen, Lesen, Aktualisieren und Löschen) für eine Ressource. Sie können eine Aktion aufrufen, indem Sie eine HTTP POST-Anforderung an den URI senden, die für die Aktion definiert ist. Die Parameter für die Aktion werden im Text der POST-Anforderung definiert.

-   Um eine Konsistenz mit Windows Azure-Richtlinien herzustellen, sollten alle URLs vereinfacht werden. Durch eine in **Key As Segment** enthaltene Änderung können einzelne Schlüssel als Segmente dargestellt werden. Beachten Sie, dass Verweise, die mehrere Schlüsselwerte enthalten, wie zuvor durch Kommas getrennte Werte in einer eingeklammerten Notation benötigen.

-   Vor dieser Version von PSWS war die einzige Möglichkeit für die Ausführung von Erstell-, Aktualisierungs- und Löschvorgängen das Aufrufen von Post, Put, oder Delete zu einer Ressource auf oberster Ebene. Neu in dieser Version von PSWS: mit enthaltenen Ressourcenvorgängen können Benutzer die gleichen Ergebnisse erzielen und dabei die gleiche Ressource weniger direkt erreichen, als wären die Ressourcen enthalten.

### <a name="BKMK_powwa"></a>Neue Features in Windows PowerShell Web Access

-   Sie können die Verbindung mit vorhandenen Sitzungen in der webbasierten Windows PowerShell Web Access-Konsole trennen und erneut herstellen. Über die Schaltfläche **Speichern** in der webbasierten Konsole können Sie die Verbindung mit einer Sitzung trennen, ohne sie zu löschen und zu einem anderen Zeitpunkt eine neue Verbindung mit der Sitzung herstellen.

-   Standardparameter können auf der Anmeldeseite angezeigt werden. Um Standardparameter anzuzeigen, müssen Sie Werte für alle Einstellungen konfigurieren, die im Bereich **Optionale Verbindungseinstellungen** auf der Anmeldeseite in einer Datei namens **web.config** angezeigt werden. Sie können die Datei **web.config** verwenden, um alle optionalen Verbindungseinstellungen abgesehen von einem zweiten oder alternativen Anmeldeinformationssatz zu konfigurieren.

-   In Windows Server 2012 R2 können Sie die Autorisierungsregeln für Windows PowerShell Web Access remote verwalten. Die Cmdlets **Add-PswaAuthorizationRule** und **Test-PswaAuthorizationRule** enthalten nun einen Credential-Parameter, mit dem Administratoren Autorisierungsregeln von einem Remotecomputer oder in einer Windows PowerShell Web Access-Sitzung verwalten können.

-   Sie können nun mehrere Windows PowerShell Web Access-Sitzungen in einer einzelnen Browsersitzung ausführen, indem Sie für jede Sitzung eine neue Browserregisterkarte verwenden. Sie müssen nicht mehr eine neue Browsersitzung öffnen, um in der webbasierten Windows PowerShell-Konsole eine Verbindung mit einer neuen Sitzung herzustellen.

### <a name="BKMK_bugs"></a>Wichtige Fehlerbehebungen in Windows PowerShell 4.0

-   **Get-Counter** kann nun Zähler zurückgeben, die in französischen Editionen von Windows ein Apostroph-Zeichen enthalten.

-   Sie können nun die **GetType**-Methode zu deserialisierten Objekten anzeigen.

-   Mit **#Requires**-Anweisungen können Benutzer nun bei Bedarf Administratorzugriffsrechte anfordern.

-   Das Cmdlet **Import-Csv** ignoriert nun leere Zeilen.

-   Ein Problem wurde behoben, wenn Windows PowerShell ISE bei der Ausführung eines **Invoke-WebRequest**-Befehls zu viel Speicher verwendet.

-   **Get-Module** zeigt nun Modulversionen in einer **Version**-Spalte an.

-   Remove-Element – Recurse entfernt nun wie erwartet Elemente aus Unterordnern.

-   Eine **UserName**-Eigenschaft wurde zu **Get-Process**-Ausgabeobjekten hinzugefügt.

-   Das Cmdlet **Invoke-RestMethod** gibt nun alle verfügbaren Ergebnisse wieder.

-   **Add-Member** wirkt sich jetzt auf Hashtabellen aus, selbst wenn darauf noch nicht zugegriffen wurde.

-   **Select-Object –Expand** verursacht keinen Fehler mehr oder generiert eine Ausnahme, wenn der Wert der Eigenschaft NULL oder leer ist.

-   **Get-Process** kann nun in einer Pipeline mit anderen Befehlen verwendet werden, die die **ComputerName**-Eigenschaft aus Objekten abruft.

-   **ConvertTo-Json** und **ConvertFrom-Json** können nun Begriffe in doppelten Anführungszeichen akzeptieren, und die dazugehörigen Fehlermeldungen sind nun lokalisierbar.

-   **Get-Job** gibt nun alle abgeschlossenen geplanten Aufträge zurück, sogar in neuen Sitzungen.

-   Probleme mit der Bereitstellung von VHDs und deren Aufhebung mithilfe des **FileSystem**-Anbieters in Windows PowerShell 4.0 wurden behoben. Windows PowerShell kann nun neue Laufwerke erkennen, wenn sie in derselben Sitzung bereitgestellt werden.

-   Sie müssen nicht mehr explizit Module des Typs **ScheduledJob** oder **Workflow** laden, um mit ihren Auftragstypen zu arbeiten.

-   Am Prozess für den Import von Workflows wurden Leistungsverbesserungen vorgenommen, die geschachtelte Workflows definieren; dieser Prozess ist nun schneller.

## <a name="BKMK_wps3"></a>Neue Features in Windows PowerShell 3.0
Windows PowerShell 3.0 bietet die folgenden neuen Features.

-   [Windows PowerShell-Workflow](#BKMK_Workflow)

-   [Windows PowerShell Web Access](#BKMK_WebAccess)

-   [Neue Windows PowerShell ISE-Features](#BKMK_ISE)

-   [Unterstützung für Microsoft .NET Framework 4.0](#BKMK_NET4)

-   [Unterstützung für Windows Preinstallation Environment](#BKMK_WinPE)

-   [Sitzungen](#BKMK_Disconnected)

-   [Sitzungskonnektivität](#BKMK_Robust)

-   [Aktualisierbares Hilfesystem](#BKMK_UpHelp)

-   [Verbesserte Onlinehilfe](#BKMK_Online)

-   [CIM-integration](#BKMK_CIM)

-   [Sitzungskonfigurationsdateien](#BKMK_ConfigFile)

-   [Geplante Aufträge und Task Scheduler-Integration](#BKMK_ScheduledJob)

-   [Erweiterungen für Windows PowerShell-Sprache](#BKMK_Lang)

-   [Neue Kern-Cmdlets](#BKMK_Core)

-   [Verbesserungen an vorhandenen Core-Cmdlets und Anbieter](#BKMK_Prov)

-   [Remote-Modul importieren und Ermittlung](#BKMK_REM)

-   [Verbesserte Befehlszeilenergänzung](#BKMK_TAB)

-   [Auto-Laden von Modulen](#BKMK_AutoLoad)

-   [Der Moduloberfläche](#BKMK_MOD)

-   [Vereinfachte Ermittlung von Befehlen](#BKMK_SIMPLE)

-   [Verbesserte Protokollierung, Diagnose und Unterstützung von Gruppenrichtlinien](#BKMK_LOG)

-   [Formatierung und Ausgabeverbesserungen](#BKMK_OUT)

-   [Verbesserte Konsolenhosterfahrung](#BKMK_HOST)

-   [Neue Cmdlets und Hosting-APIs](#BKMK_API)

-   [Verbesserung der Leistung](#BKMK_PERF)

-   [RunAs und Unterstützung freigegebener Hosts](#BKMK_RUNAS)

-   [Sonderzeichen Behandlung Verbesserungen](#BKMK_CHAR)

### <a name="BKMK_Workflow"></a>Windows PowerShell-Workflow
Durch Windows PowerShell® Workflow kann Windows PowerShell auf die Leistungsfähigkeit von Windows Workflow Foundation zurückgreifen. Sie können Workflows in XAML oder in der Windows PowerShell-Sprache schreiben und sie genau wie ein Cmdlet ausführen. Das Cmdlet [Get-Command](https://technet.microsoft.com/en-us/library/59c6d302-6e8c-48b7-a6f6-f0172df936ad) ruft Workflowbefehle ab, das Cmdlet [Get-Help](https://technet.microsoft.com/en-us/library/1f46eeb4-49d7-4bec-bb29-395d9b42f54a) ruft Hilfe für Workflows ab.

Workflows sind Sequenzen von Verwaltungsaktivitäten mehrerer Computer, die lange ausgeführt werden, wiederholbar, parallelisierbar, unterbrechbar und neu startbar sind. Workflows können durch eine beabsichtigte oder unbeabsichtigte Unterbrechung fortgesetzt werden, z. B. nach einem Netzwerkausfall, einem Neustart von Windows oder einem Stromausfall.

Workflows sind außerdem portierbar; sie können als XAML-Dateien exportiert oder aus ihnen importiert werden. Sie können benutzerdefinierte Sitzungskonfigurationen schreiben, mit denen Workflows oder Aktivitäten durch delegierte oder untergeordnete Benutzer in einem Workflow ausgeführt werden können.

Im Folgenden finden Sie die Vorteile von Windows PowerShell Workflow.

-   **Automatisierung sequenzierter, langer Aufgaben.**

-   **Remoteüberwachung lange ausgeführter Aufgaben**. Status und Fortschritt von Aktivitäten sind jederzeit sichtbar.

-   **Verwaltung mehrerer Computer.** Gleichzeitiges Ausführen von Aufgaben als Workflows zu Hundertern von verwalteten Knoten. Windows PowerShell Workflow enthält eine integrierte Bibliothek allgemeiner Verwaltungsparameter, z.B. **PSComputerName**, mit denen Verwaltungsszenarios für mehrere Computer umgesetzt werden können.

-   **Einzelne aufgabenausführung komplexer Prozesse.** Sie können ähnliche Skripts kombinieren, die ein vollständiges End-to-End-Szenario in einen einzelnen Workflow implementiert.

-   **Persistenz**: Ein Workflow wird an bestimmten Punkten gespeichert (oder zwischengespeichert), die vom Autor definiert wurden, sodass der Workflow bei der letzten persistent gespeicherten Aufgabe (oder dem Prüfpunkt) fortgesetzt werden kann, statt den Workflow vom Anfang erneut starten zu müssen.

-   **Stabilität.** Automatisierte Wiederherstellung nach einem Fehler. Workflows überstehen geplante und nicht geplante Neustarts. Sie können die Workflowausführung unterbrechen und den Workflow anschließend vom letzten permanenten Punkt fortsetzen. Workflowautoren können festlegen, dass bestimmte Aktivitäten bei einem Fehler für einen oder mehrere verwaltete Knoten erneut ausgeführt werden.

-   **Zu trennen, erneut eine Verbindung herstellen und Ausführen in getrennten Sitzungen.** Benutzer können eine Verbindung zum Workflowserver herstellen und sie trennen, der Workflow wird jedoch kontinuierlich ausgeführt. Sie können sich vom Clientcomputer abmelden oder den Clientcomputer neu starten und die Workflowausführung von einem anderen Computer überwachen, ohne den Workflow zu unterbrechen.

-   **Bei der Planung.** Workflowaufgaben können wie jedes Windows PowerShell-Cmdlet oder -Skript geplant werden.

-   **Workflow- und Verbindungsdrosselung.** Die Workflowausführung und Verbindungen zu Knoten können gedrosselt werden, sodass eine Skalierung und Hochverfügbarkeitsszenarios möglich sind.

### <a name="BKMK_WebAccess"></a>Windows PowerShell Web Access
Windows PowerShell® Web Access ist ein Windows Server 2012-Feature, mit dem Benutzer Windows PowerShell-Befehle und -Skripts in einer webbasierten Konsole ausführen können. Geräte mit webbasierter Konsole benötigen Windows PowerShell, Remoteverwaltungssoftware oder Browser-Plug-In-Installationen nicht. Erforderlich ist lediglich ein ordnungsgemäß konfiguriertes Windows PowerShell Web Access-Gateway und ein Browser auf dem Clientgerät, der JavaScript® unterstützt und Cookies akzeptiert.

Weitere Informationen finden Sie unter [Bereitstellen von Windows PowerShell Web Access](https://go.microsoft.com/fwlink/p/?LinkID=221050).

### <a name="BKMK_ISE"></a>Neue Windows PowerShell ISE-Features
Für Windows PowerShell 3.0 bietet Windows PowerShell® Integrated Scripting Environment (ISE) viele neue Features, darunter IntelliSense, das Fenster „Befehl anzeigen“ und ein vereinheitlichter Konsolenbereich, Snippets, übereinstimmende Klammern, Bereich erweitern/reduzieren, automatisches Speichern, Liste zuletzt verwendeter Elemente, umfassende Kopie, Blockkopie und vollständige Unterstützung für das Schreiben von Windows PowerShell-Skriptworkflows. Weitere Informationen finden Sie unter [about_Windows_PowerShell_ISE [v3]](https://technet.microsoft.com/en-us/library/dfa54d47-60c6-4fff-8197-c747e8d411bb).

### <a name="BKMK_NET4"></a>Unterstützung für Microsoft .NET Framework 4
Windows PowerShell wurde während der Common Language Runtime 4.0 erstellt. Cmdlet-, Skript- und Workflowersteller können die neuen Microsoft .NET Framework 4-Klassen in Windows PowerShell mit Features verwenden, zu denen unter anderen die Anwendungskompatibilität und Bereitstellung, Managed Extensibility Framework, paralleles Computing, Netzwerk, Windows Communication Foundation und Windows Workflow Foundation gehören.

### <a name="BKMK_WinPE"></a>Unterstützung für Windows Preinstallation Environment
Windows PowerShell 3.0 ist eine optionale Komponente des Windows Preinstallation Environment (Windows PE) 4.0 für Windows 8. Windows PE ist ein minimales Betriebssystem, mit dem ein Computer ohne Betriebssystem gestartet und für die Windows-Installation vorbereitet werden kann. Windows PE kann zum Partitionieren und Formatieren von Festplatten, zum Kopieren von Datenträgerimages auf einen Computer und zum Auslösen der Windows-Einrichtung über eine Netzwerkfreigabe verwendet werden. Windows PowerShell 3.0 kann unter Windows PE zum Verwalten von Bereitstellungs-, Diagnose- und Wiederherstellungsszenarien verwendet werden.

### <a name="BKMK_Disconnected"></a>Getrennte Sitzungen
Ab Windows PowerShell 3.0 werden permanente durch den Benutzer verwaltete Sitzungen ("PSSessions"), die Sie mithilfe des New-PSSession-Cmdlets erstellen, auf dem Remotecomputer gespeichert. Sie sind nicht mehr von der Sitzung abhängig, in der sie erstellt wurden.

Sie können jetzt eine Sitzung trennen, ohne die Befehle zu unterbrechen, die in der Sitzung ausgeführt werden. Sie können die Sitzung schließen und den Computer herunterfahren. Später können Sie über eine andere Sitzung auf demselben oder einem anderen Computer erneut eine Verbindung zu der Sitzung herstellen.

Der **ComputerName**-Parameter des Cmdlets [Get-PSSession](https://technet.microsoft.com/en-us/library/b2b10531-d0df-4746-b877-e75c09955cb6) ruft nun alle Benutzersitzungen ab, die eine Verbindung mit dem Computer aufbauen, sogar wenn sie in einer anderen Sitzung auf einem anderen Computer gestartet wurden. Sie können eine Verbindung zu den Sitzungen herstellen, die Ergebnisse der Befehle abrufen, neue Befehle starten, und anschließend die Verbindung zu der Sitzung trennen.

Neue Cmdlets wurden hinzugefügt, damit das Feature „Getrennte Sitzungen“ unterstützt wird. Dazu gehören [Disconnect-PSSession](https://technet.microsoft.com/en-us/library/f8f95111-612f-4cba-9098-77904b0473d8), [Connect-PSSession](https://technet.microsoft.com/en-us/library/b803dd29-f208-4079-80d4-db04d778f060) und „Receive-PSSession“ sowie neue Parameter, die den Cmdlets hinzugefügt wurden, die „PSSessions“ verwalten, z.B. der **InDisconnectedSession**-Parameter des Cmdlets [Invoke-Command](https://technet.microsoft.com/en-us/library/906b4b41-7da8-4330-9363-e7164e5e6970).

Die Funktion für getrennte Sitzungen wird nur unterstützt, wenn die Computer am startenden ("Client") und beendenden ("Server") Ende der Verbindung Windows PowerShell 3.0 ausführen.

### <a name="BKMK_Robust"></a>Zuverlässige Sitzungskonnektivität
Windows PowerShell 3.0 erkennt unerwartete Verbindungsunterbrechungen zwischen dem Client und dem Server und versucht, die Verbindung neu aufzubauen und die Ausführung automatisch fortzusetzen. Wenn die Client/Server-Verbindung nicht erneut in der geplanten Zeit hergestellt werden kann, erhält der Benutzer eine Benachrichtigung und die Sitzung wird getrennt. Während des Versuchs eine neue Verbindung herzustellen, gibt Windows PowerShell dem Benutzer kontinuierliches Feedback.

Wenn die getrennte Sitzung mithilfe des InvokeCommand gestartet wurde, erstellt Windows PowerShell einen Auftrag für die getrennte Sitzung, damit ein erneuter Verbindungsaufbau schneller erfolgen und die Ausführung fortgesetzt werden kann.

Diese Features bieten eine zuverlässigere und wiederherstellbare Remoting-Erfahrung, mit der Benutzer lange Aufgaben ausführen können, für die zuverlässige Sitzungen wie Workflows erforderlich sind.

### <a name="BKMK_UpHelp"></a>Aktualisierbares Hilfesystem
Sie können aktualisierte Hilfedateien für die Cmdlets in Ihren Modulen herunterladen. Das Cmdlet [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545) ermittelt die neuesten Hilfedateien, lädt sie aus dem Internet herunter, entpackt sie, überprüft sie auf Gültigkeit und installiert sie im ordnungsgemäßen sprachspezifischen Verzeichnis für das Modul.

Um die aktualisierten Hilfedateien zu verwenden, geben Sie einfach `Get-Help` ein. Windows oder Windows PowerShell müssen nicht neu gestartet werden. Starten Sie Windows PowerShell mit der Option „Als Administrator ausführen“, um Hilfe für Module im Verzeichnis „$pshome“ zu aktualisieren.

Um Benutzer zu unterstützen, die keinen Internetzugriff haben und sich hinter Firewalls befinden, lädt das neue Cmdlet [Save-Help](https://technet.microsoft.com/en-us/library/aed94f90-b73f-4e25-a25d-7c18d9f161fa) Hilfedateien in ein Dateisystemverzeichnis (z. B. eine Dateifreigabe) herunter. Benutzer können anschließend das Cmdlet [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545) verwenden, um aktualisierte Hilfedateien aus der Dateifreigabe abzurufen.

Sie können das Cmdlet [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545) verwenden, um Hilfedateien für alle oder bestimmte Module in allen unterstützten Benutzeroberflächenkulturen zu aktualisieren. Sie können sogar einen [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545)-Befehl in Ihrem Windows PowerShell-Profil ablegen. Standardmäßig lädt Windows PowerShell die Hilfedateien für ein Modul nicht öfter als einmal täglich herunter.

Windows 8- und Windows Server 2012-Module enthalten keine Hilfedateien. Um die neuesten Hilfedateien herunterzuladen, geben Sie `Update-Help` ein. Geben Sie `Get-Help` (ohne Parameter) ein, um weitere Informationen zu erhalten, oder lesen Sie [about_Updatable_Help](https://technet.microsoft.com/en-us/library/10bba75c-f4ac-4ca1-bbf3-8f34dd521ffe).

Wenn die Hilfedateien für ein Cmdlet nicht auf dem Computer installiert sind, zeigt das Cmdlet [Get-Help](https://technet.microsoft.com/en-us/library/1f46eeb4-49d7-4bec-bb29-395d9b42f54a) nun automatisch generierte Hilfe an. Die automatisch generierte Hilfe umfasst die Befehlssyntax und Anweisungen für die Nutzung des Cmdlets [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545) zum Herunterladen der Hilfedateien.

Jeder Modulautor kann eine aktualisierbare Hilfe für das Modul unterstützen. Sie können Hilfedateien in das Modul einschließen und die aktualisierbare Hilfe verwenden, um sie zu aktualisieren oder Hilfedateien wegzulassen und die aktualisierbare Hilfe zu verwenden, um sie zu installieren. Weitere Informationen zur Unterstützung der aktualisierbaren Hilfe finden Sie unter [Unterstützen der aktualisierbaren Hilfe](https://go.microsoft.com/FWLink/?LinkID=242129) in MSDN.

### <a name="BKMK_Online"></a>Verbesserte Onlinehilfe
Die Windows PowerShell-Onlinehilfe ist eine nützliche Ressource für alle Benutzer. Sie ist jedoch besonders wichtig für Benutzer, die aktualisierte Hilfedateien nicht installieren bzw. nicht installieren können.

Um die Onlinehilfe für jedes Windows PowerShell-Cmdlet zu erhalten, geben Sie Folgendes ein:

```
Get-Help <cmdlet-name> -Online
```

Windows PowerShell öffnet die Onlineversion des Hilfethemas in Ihrem Standardinternetbrowser.

Das **Get-Help -Online**-Feature in Windows PowerShell 3.0 ist nun noch leistungsfähiger, da es sogar dann funktioniert, wenn keine Hilfedateien für das Cmdlet auf dem Computer installiert sind. Das Feature **Get-Help -Online** ruft den URI für das Onlinehilfethema aus der „HelpUri“-Eigenschaft von Cmdlets und erweiterten Funktionen ab.

```
PS C:\>(Get-Command Get-ScheduledJob).HelpUri
https://go.microsoft.com/fwlink/?LinkID=223923
```

Ab Windows PowerShell 3.0 können Autoren von C#-Cmdlets die **HelpUri**-Eigenschaft auffüllen, indem sie ein **HelpUri**-Attribut in der Cmdlet-Klasse erstellen. Entwickler erweiterter Funktionen können eine **HelpUri**-Eigenschaft für das **CmdletBinding**-Attribut definieren. Der Wert der **HelpUri**-Eigenschaft muss mit „http“ oder „https“ beginnen.

Sie können auch dem ersten dazugehörigen Link einer XML-basierten Cmdlet-Hilfedatei einen **HelpUri**-Wert oder eine „.Link“-Direktive der kommentarbasierten Hilfe in einer Funktion hinzufügen.

Weitere Informationen zur Unterstützung der Onlinehilfe finden Sie unter [Unterstützen der Onlinehilfe](https://go.microsoft.com/fwlink/?LinkId=242132) in MSDN.

### <a name="BKMK_CIM"></a>CIM-Integration
Windows PowerShell 3.0 bietet Unterstützung für das Common Information Model (CIM), das allgemeine Definitionen der Verwaltungsinformationen für Systeme, Netzwerke, Anwendungen und Dienste bereitstellt, mit denen Verwaltungsinformationen zwischen heterogenen Systemen ausgetauscht werden können. Unterstützung für CIM in Windows PowerShell 3.0, einschließlich der Möglichkeit zum Erstellen von Windows PowerShell-Cmdlets auf Grundlage neuer oder vorhandener CIM-Klassen, von Befehlen auf Grundlage der Cmdlet-Definition von XML-Dateien und der Unterstützung für CIM .NET Framework. API, CIM-Verwaltungs-Cmdlets und WMI 2.0-Anbieter.

### <a name="BKMK_ConfigFile"></a>Sitzungskonfigurationsdateien
Ab Windows PowerShell 3.0 können Sie mithilfe einer Datei eine benutzerdefinierte Sitzungskonfiguration entwerfen. Mit der neuen Sitzungskonfigurationsdatei können Sie die Umgebung der Sitzungen ermitteln, die die Sitzungskonfiguration verwendet, dazu gehören die Module, Skripts, und Formatdateien werden in Sitzungen geladen, die Cmdlets und Sprachelemente, die Benutzer verwenden können, und Skripts, die sie ausführen können sowie die Variablen, die sie verwenden können.

Sie können eine Sitzung entwerfen, in der Benutzer Cmdlets nur von einem bestimmten Modul ausführen können, oder eine Sitzung, in der Benutzer denen vollständigen Sprachzugriff auf alle Module und Zugriff auf Skripts haben, die erweiterte Aufgaben ausführen.

In vorherigen Versionen von Windows PowerShell stand das Steuerelement auf dieser Ebene nur den Benutzern zur Verfügung, die ein C#-Programm oder ein komplexes Startskript schreiben können. Nun kann jedes Mitglied der Administratorengruppe auf dem Computer eine Sitzungskonfiguration mithilfe einer Konfigurationsdatei anpassen.

Um eine Sitzungskonfigurationsdatei zu erstellen, müssen Sie das Cmdlet [New-PSSessionConfigurationFile](https://technet.microsoft.com/en-us/library/5f3e3633-6e90-479c-aea9-ba45a1954866) verwenden. Um die Sitzungskonfigurationsdatei auf eine Sitzungskonfiguration anzuwenden, verwenden Sie das Cmdlet [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) oder [Set-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/b21fbad3-1759-4260-b206-dcb8431cd6ea).

Weitere Informationen finden Sie unter [about_Session_Configuration_Files](https://technet.microsoft.com/en-us/library/c7217447-1ebf-477b-a8ef-4dbe9a1473b8) und [New-PSSessionConfigurationFile](https://technet.microsoft.com/en-us/library/5f3e3633-6e90-479c-aea9-ba45a1954866).

### <a name="BKMK_ScheduledJob"></a>Integration geplanter Aufträge und der Aufgabenplanung
Sie können jetzt Windows PowerShell-Hintergrundaufträge planen und in Windows PowerShell und in der Aufgabenplanung verwalten.

In Windows PowerShell geplante Aufträge sind eine nützliche Hybridlösung von Windows PowerShell-Hintergrundaufträgen und Aufgabenplanungsaufträgen.

Genau wie Windows PowerShell-Hintergrundaufträge werden geplante Aufträge asynchron im Hintergrund ausgeführt. Instanzen geplanter, abgeschlossener Aufträge können mithilfe von „Job“-Cmdlets verwaltet werden, wie z. B. [Start-Job](https://technet.microsoft.com/en-us/library/2bc04935-0deb-4ec0-b856-d7290cca6442) und [Get-Job](https://technet.microsoft.com/en-us/library/1352c534-7193-46ca-9ab1-0c5219a661ad).

Genau wie Task Scheduler-Aufgaben können Sie geplante Aufträge für einen einmaligen oder wiederkehrenden Plan, oder als Antwort auf eine Aktion oder ein Ereignis ausführen. Sie können geplante Aufträge in der Aufgabenplanung bei Bedarf anzeigen, aktivieren und deaktivieren, sie ausführen oder als Vorlagen verwenden, und Bedingungen festlegen, unter denen der Auftrag gestartet wird.

Außerdem enthalten geplante Aufträge einen angepassten Cmdlets-Satz für die Verwaltung. Mit den Cmdlets können Sie geplante Aufträge erstellen, bearbeiten, verwalten, deaktivieren und geplante Aufträge erneut aktivieren, geplante Auftragsauslöser erstellen und geplante Auftragsoptionen festlegen.

Weitere Informationen zu geplanten Aufträgen finden Sie unter [about_Scheduled_Jobs](https://technet.microsoft.com/en-us/library/3b546629-703c-4939-b44f-52dd567bce92).

### <a name="BKMK_Lang"></a>Verbesserungen der Windows PowerShell-Sprache
Windows PowerShell 3.0 umfasst viele Features, die darauf ausgelegt sind, die Sprache einfacher und benutzerfreundlicher zu gestalten und häufige Fehler zu vermeiden. Die Verbesserungen umfassen die Enumeration der Eigenschaft, Anzahl und Längeneigenschaften von skalaren Objekten, neue Umleitungsoperatoren, der $Using-Bereichsmodifikator, automatische PSItem-Variable, flexible Skriptformatierung, Attribute von Variablen, vereinfachte Attributargumente, numerische Befehlsnamen, den Stop-Parsing-Operator, verbesserte Array-Verteilung, neue Bit-Operatoren, sortierte Wörterbücher, PSCustomObject-Umwandlung und eine verbesserte kommentar-basierte Hilfe.

### <a name="BKMK_Core"></a>Neue Kern-Cmdlets
Zur Windows PowerShell-Kerninstallation wurden neue Cmdlets hinzugefügt, dazu gehören Cmdlets zur Verwaltung geplanter Aufträge, getrennte Sitzungen, die CIM-Integration und das aktualisierbare Hilfesystem.

|||
|-|-|
|Add-JobTrigger|New-JobTrigger|
|Connect-PSSession|New-PSSessionConfigurationFile|
|ConvertFrom-Json|New-PSTransportOption|
|ConvertTo-Json|New-PSWorkflowExecutionOption|
|Disable-JobTrigger|New-PSWorkflowSession|
|Disable-ScheduledJob|New-ScheduledJobOption|
|Disconnect-PSSession|New-WinEvent|
|Enable-JobTrigger|Receive-PSSession|
|Enable-ScheduledJob|Register-CimIndicationEvent|
|Get-CimAssociatedInstance|Register-ScheduledJob|
|Get-CimClass|Remove-CimInstance|
|Get-CimInstance|Remove-CimSession|
|Get-CimSession|Remove-TypeData|
|Get-ControlPanelItem|Rename-Computer|
|Get-IseSnippet|Resume-Job|
|Get-JobTrigger|Save-Help|
|Get-ScheduledJob|Set-CimInstance|
|Get-ScheduledJobOption|Set-JobTrigger|
|Get-TypeData|Set-ScheduledJob|
|Import-IseSnippet|Set-ScheduledJobOption|
|Invoke-AsWorkflow|Show-Command|
|Invoke-CimMethod|Show-ControlPanelItem|
|Invoke-RestMethod|Suspend-Job|
|Invoke-WebRequest|Test-PSSessionConfigurationFile|
|New-CimInstance|Unblock-File|
|New-CimSession|Unregister-ScheduledJob|
|New-CimSessionOption|Update-Help|
|New-IseSnippet||

### <a name="BKMK_Prov"></a>Verbesserungen an vorhandenen Kern-Cmdlets und Anbietern
Windows PowerShell 3.0 umfasst neue Features für vorhandene Cmdlets. Dazu gehören die vereinfachte Syntax und neue Parameter für folgende Cmdlets: Computer-Cmdlets, CSV-Cmdlets, Get-ChildItem, Get-Command, Get-Content, Get-History, Measure-Object, Security-Cmdlets, Select-Object, Select-String, Split-Path, Start-Process, Tee-Object, Test-Connection, Add-Member und WMI-Cmdlets.

Die Windows PowerShell-Anbieter wurden auch erheblich verbessert, dazu gehört die Unterstützung von Zertifikatanbietern zur Verwaltung von Secure Socket Layer (SSL)-Zertifikaten für das Web-Hosting, die Unterstützung von Anmeldeinformationen, permanente Netzwerklaufwerke, und alternative Datenstreams auf Dateisystemlaufwerken.

### <a name="BKMK_REM"></a>Import und Erkennung des Remotemoduls
Windows PowerShell 3.0 erweitert die Modulerkennung, die Importfunktion und implizite Remotefunktionen auf Remotecomputern. Die Module-Cmdlets rufen Module auf den Remotecomputern ab und importieren die Module mithilfe von Windows PowerShell-Remoting auf die Remote- oder lokalen Computer. Mit der neuen Unterstützung für CIM-Sitzungen können Sie CIM und WMI verwenden, um nicht-Windows-Computer zu verwalten, indem Sie die Befehle auf dem lokalen Computer importieren, die implizit auf dem Remotecomputer ausgeführt werden.

Weitere Informationen finden Sie in den Hilfethemen für die Cmdlets [Get-Module](https://technet.microsoft.com/en-us/library/2cccd4c4-9a21-4c77-b691-984ee57242e1) und [Import-Module](https://technet.microsoft.com/en-us/library/af616c24-e122-4098-930e-1e3ea2080ade).

### <a name="BKMK_TAB"></a>Verbesserte Vervollständigung mit der TAB-TASTE
Durch die Vervollständigung mit der TAB-TASTE in der Windows PowerShell-Konsole werden nun die Namen von Cmdlets, Parametern, Parameterwerten, Enumerationen, .NET Framework-Typen, COM-Objekten, versteckten Verzeichnissen und vielem mehr vervollständigt. Das Feature zum Fertigstellen von Registerkarten wurde auf Grundlage einer neuen Analyse und einer abstrakten Syntaxstruktur komplett neu geschrieben, damit weitere Szenarios unterstützt werden, dazu gehören speicherinterne Analysestrukturen und die Midline-Registerkartenfertigstellung.

### <a name="BKMK_AutoLoad"></a>Auto-Laden von Modulen
Das Cmdlet [Get-Command](https://technet.microsoft.com/en-us/library/59c6d302-6e8c-48b7-a6f6-f0172df936ad) ruft nun alle Cmdlets und Funktionen aus allen Modulen ab, die auf dem Computer installiert sind, selbst wenn das Modul nicht in die aktuelle Sitzung importiert wird.

Wenn Sie das erforderliche Cmdlet abrufen, können Sie es sofort nutzen, ohne Module importieren zu müssen. Windows PowerShell-Module werden nun automatisch importiert, wenn Sie ein beliebiges Cmdlet im Modul verwenden. Sie müssen nicht mehr nach dem Modul suchen und es importieren, um die Cmdlets zu nutzen.

Der automatische Import von Modulen wird durch Verwenden des Cmdlets in einem Befehl oder durch Ausführen von **Get-Command** für ein Cmdlet ohne Platzhalter oder von [Get-Help](https://technet.microsoft.com/en-us/library/1f46eeb4-49d7-4bec-bb29-395d9b42f54a) für ein Cmdlet ohne Platzhalter ausgelöst.

Sie können mithilfe der Einstellungsvariablen **$PSModuleAutoLoadingPreference** das automatische Importieren von Modulen aktivieren, deaktivieren und konfigurieren.

Weitere Informationen finden Sie unter [about_Modules [v4]](https://technet.microsoft.com/en-us/library/94f57429-a539-4aee-bb0d-205cd7e801f9), [about_Preference_Variables [v4]](https://technet.microsoft.com/en-us/library/31344314-be29-4286-b039-afa5460cbe8b) und in den Hilfethemen für die Cmdlets [Get-Command](https://technet.microsoft.com/en-us/library/59c6d302-6e8c-48b7-a6f6-f0172df936ad) und [Import-Module](https://technet.microsoft.com/en-us/library/af616c24-e122-4098-930e-1e3ea2080ade).

### <a name="BKMK_MOD"></a>Verbesserungen der Modulumgebung
Mit Windows PowerShell 3.0 erhalten Sie die erweiterte Feature-Unterstützung für Module, dazu gehören auch folgende neue Features.

1.  Die Modulprotokollierung für einzelne Module (LogPipelineExecutionDetails) und die neue „Modulprotokollierung aktivieren“-Gruppenrichtlinieneinstellung

2.  Erweiterte Modulobjekte, die die Werte aus dem Modulmanifest verfügbar machen

3.  Die neue **ExportedCommands**-Eigenschaft von Modulen, einschließlich geschachtelter Module, die Befehle aller Typen kombinieren.

4.  Verbesserte Erkennung verfügbarer (nicht importierter) Module, einschließlich Zulassen der Parameter **Path** und **ListAvailable** im selben Befehl.

5.  Der neue **DefaultCommandPrefix**-Schlüssel in Modulemanifesten, der Namenskonflikte vermeidet, ohne den Modulcode zu ändern.

6.  Verbesserte Modulvoraussetzungen, einschließlich voll qualifizierter, erforderlicher Module mit Version und GUID sowie dem automatischen Importieren erforderlicher Module

7.  Ruhigerer, optimierter Ablauf des Cmdlets [New-ModuleManifest](https://technet.microsoft.com/en-us/library/512adced-f42f-4e88-ba7c-834fc9e5d047).

8.  Neuer **Module**-Parameter für „#Requires“

9. Verbessertes Cmdlet [Import-Module](https://technet.microsoft.com/en-us/library/af616c24-e122-4098-930e-1e3ea2080ade) mit den Parametern **MinimumVersion** und **RequiredVersion**.

### <a name="BKMK_SIMPLE"></a>Vereinfachte Ermittlung von Befehlen
Sie müssen nicht mehr alle Module importieren, um die für Ihre Sitzung verfügbaren Befehle zu erkennen. In Windows PowerShell 3.0 ruft das Cmdlet [Get-Command](https://technet.microsoft.com/en-us/library/59c6d302-6e8c-48b7-a6f6-f0172df936ad) alle Befehle aus allen installierten Modulen ab. Und wenn Sie einen Befehl verwenden, wird das Modul, das den Befehl exportiert, automatisch in Ihre Sitzung importiert.

Das neue Cmdlet [Show-Command](https://technet.microsoft.com/en-us/library/65bba50b-91a8-49d5-80a2-a30fc684ba41) ist speziell für Einsteiger konzipiert. Sie können in einem Fenster nach Befehlen suchen. Sie können alle Befehle anzeigen oder nach Modul filtern, ein Modul importieren, indem Sie auf eine Schaltfläche klicken, Textfelder und Dropdownlisten verwenden, um einen gültigen Befehl zu konstruieren, und anschließend den Befehl zu kopieren und auszuführen, ohne das Fenster zu verlassen.

### <a name="BKMK_LOG"></a>Verbesserte Protokollierung, Diagnose und Unterstützung von Gruppenrichtlinien
Windows PowerShell 3.0 verbessert die Protokollierungs- und Nachverfolgungsunterstützung für Befehle und Module mit Unterstützung von ETW-Protokollen (Event Tracing for Windows, Ereignisablaufverfolgung für Windows), einer bearbeitbaren **LogPipelineExecutionDetails**-Eigenschaft von Modulen und der Gruppenrichtlinieneinstellung „Modulprotokollierung aktivieren“. Sie können nun Parameterwerte aus Protokolldetails abrufen, indem Sie die Protokolleigenschaften anzeigen.

### <a name="BKMK_OUT"></a>Formatierungs- und Ausgabeverbesserungen
Neue Formatierungs- und Ausgabeverbesserungen steigern die Effizienz aller Windows PowerShell-Benutzer. Die Verbesserungen umfassen die Umleitung der Ausgabe für alle Datenströme, ein verbessertes Cmdlet „Update-Type“, das Typen dynamisch ohne „Format.ps1xml“-Dateien hinzufügt, Zeilenumbruch in der Ausgabe, Standardformatierungseigenschaften benutzerdefinierter Objekte, den **PSCustomObject**-Typ, verbesserte Formatierung für WMI-Objekte und heterogene Objekte und Unterstützung der Erkennung von Methodenüberladungen.

### <a name="BKMK_HOST"></a>Verbesserte Konsolenhostumgebung
Das Windows PowerShell-Konsolenhostprogramm hat in Windows PowerShell 3.0 neue Features. Dazu gehört auch standardmäßig das Singlethread-Apartment. Mit der neuen „Mit PowerShell ausführen“-Option im Datei-Explorer können Sie Skripts in einer nicht eingeschränkten Sitzung durch einen einfachen Rechtsklick ausführen. Die neue Konsolenhostlogik startet Windows PowerShell schneller und mit den neuen Schriftarten können Sie die bekannte Konsolenfensteroberfläche personalisieren.

Weitere Informationen finden Sie unter [about_Run_With_PowerShell](https://technet.microsoft.com/en-us/library/c9d9ca5f-eff9-4409-be9d-e43b5b4087eb).

### <a name="BKMK_API"></a>Neue Cmdlet- und Hosting-APIs
Die neuen Cmdlet-API und Hosting-API umfassen öffentliche, erweiterte Syntaxstruktur (AST)-APIs und APIs für die Pipeline-Nummerierung, geschachtelte Pipelines, Runspacepool-Registerkartenfertigstellung, Windows RT, das obsolete Cmdlet-Attribut, und Verb- und Substantiveigenschaften des FunctionInfo-Objekts.

### <a name="BKMK_PERF"></a>Leistungsverbesserungen
Die erheblichen Leistungsverbesserungen in Windows PowerShell stammen aus der neuen Sprachanalyse, die auf DLR (Dynamic Runtime Language) in .NET Framework 4. basiert, sowie der Laufzeitskriptkompilierung, Modulzuverlässigkeitsverbesserungen und Änderungen am Algorithmus von [Get-ChildItem](https://technet.microsoft.com/en-us/library/75cf79bb-4db6-4a67-8c36-3d20754e2190), die die Leistung verbessern, besonders bei der Suche nach Netzwerkfreigaben.

### <a name="BKMK_RUNAS"></a>RunAs und Unterstützung freigegebener Hosts
Windows PowerShell 3.0 bietet Unterstützung der Features „RunAs“ und „SharedHost“.

Das Feature *RunAs* ist für Windows PowerShell Workflow konzipiert. Benutzer einer Sitzungskonfiguration können damit Sitzungen erstellen, die mit der Berechtigung eines freigegebenen Benutzerkontos ausgeführt werden. Dadurch können Benutzer mit weniger Privilegien bestimmte Befehle und Skripts mit Administratorberechtigungen ausführen, außerdem entfällt das Hinzufügen von weniger erfahrenen Benutzern zur Administratorengruppe.

Mit dem Feature **SharedHost** können mehrere Benutzer auf mehreren Computern gleichzeitig eine Verbindung mit einer Workflowsitzung herstellen und den Status eines Workflows überwachen. Benutzer können einen Workflow auf einem Computer starten und dann auf einem anderen Computer eine Verbindung zur Workflowsitzung herstellen, ohne die Sitzung auf dem ursprünglichen Computer trennen zu müssen. Benutzer müssen über die gleichen Berechtigungen verfügen und die gleiche Sitzungskonfiguration verwenden. Weitere Informationen finden Sie unter „Ausführen eines Windows PowerShell Workflows“ in den ersten Schritten mit Windows PowerShell Workflow.

### <a name="BKMK_CHAR"></a>Verbesserungen bei der Verarbeitung von Sonderzeichen
Damit Windows PowerShell 3.0 Sonderzeichen besser interpretieren und ordnungsgemäß verarbeiten kann, ist der **LiteralPath**-Parameter, der Sonderzeichen in Pfaden verarbeitet, für fast alle Cmdlets gültig, die über einen **Path**-Parameter verfügen. Dazu zählen die neuen Cmdlets [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545) und [Save-Help](https://technet.microsoft.com/en-us/library/aed94f90-b73f-4e25-a25d-7c18d9f161fa). Die Analyse bietet außerdem eine spezielle Logik zur Verbesserung der Verarbeitung von Akzentzeichen (`) und eckigen Klammern in Dateinamen und -pfaden.

## Weitere Informationen
[about_Windows_PowerShell_4.0](https://technet.microsoft.com/en-us/library/hh847833(v=wps.630).aspx)
[about_Windows_PowerShell_5.0](https://technet.microsoft.com/en-us/library/6d56fa88-371e-40c9-b2de-64a2a0cd49da)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)




<!--HONumber=Oct16_HO1-->

