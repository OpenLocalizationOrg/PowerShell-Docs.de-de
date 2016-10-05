---
title: "Beispielvorlage für das Hochschreiben eines Features"
contributor: KeithB
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 025565404b60cebefac27e51c70d70edb5e47bc9

---

>Hinweis: Vorschlag für einen aussagekräftigen Namen und eine Kurzbeschreibung bereitstellen

## PowerShellGet-Erweiterungen ##
Die Cmdlets im Modul PowerShellGet wurden erheblich in WMF 5.1 aktualisiert. Die folgenden neuen Szenarien werden jetzt unterstützt:

- **Proxy-Unterstützung:** mithilfe der PowerShellGet-Cmdlets mit einem internen-Proxy wird jetzt unterstützt, durch das Hinzufügen der Proxy- und ProxyCredential Parameter aller des suchen - Install-, speichern- und Publish-Cmdlets im Modul PowerShellGet (z. B.: Publish-Module suchen-Modul installieren-Modul speichern-Module). 
- **Erzwingen der Katalog Signierung:** Alle PowerShellGet-Cmdlets nun überprüfen und Aktualisieren von signierten Modulen zu erzwingen. Module, die mit den neuen Katalog Cmdlets signiert werden durch die PowerShellGet-Cmdlets sicherstellen, dass das Modul während der Übertragung nicht geändert wurde, und Updates für das Modul nur stammen aus dem ursprünglichen Verleger können überprüft werden. Dies wirkt sich auf Install-Module und Update-Module-Cmdlets. 
- **Funktionen für die Freigabe und zum Erwerb von Rolle:** Rolle Funktionen sind die Endpunktdefinitionen verwendet, um einfach genug Verwaltung verwendet, um einzuschränken, zu konfigurieren und wird über die PowerShell-Galerie in PowerShell-Modulen gemeinsam genutzt werden. Cmdlets sind suchen RoleCapability und neu PSRoleCapabilityFile. 
- **Suchen:** Suchen-Befehl können Benutzer ein Modul in der PowerShell-Galerie zu suchen, die einen Befehl enthält, sie suchen. 
- **Authentifizierte Repositories:** Visual Studio-Paket-Management-Feed erfordert eine Authentifizierung, etwas, das nun über die Cmdlets PowerShellGet unterstützt wird.

Benutzer-Feedback identifiziert weitere Bereiche für die Verbesserung, die in 5.1 und behoben werden:

- **Skript für die Installation Änderungen:** verwendet, die vom Skript für die Installation verwendet nicht den Pfad automatisch in 5.1 hinzugefügt. Diese Änderung wurde in der eigenständigen Version von PowerShellGet und befindet sich jetzt im WMF 5.1.
- **Hinzufügen von Metadatenfeldern zu vorhandenen Skripts:** Update ScriptFileInfo kann auf vorhandenen Skripts verwendet werden, die Metadaten erforderlich, um die Veröffentlichung in der PowerShellGallery Felder hinzufügen. Früher mussten Benutzer manuell dies ein vorhandenes Skript zusammenführen.
- **Ein niedrigerer Versionsangabe Modul im Katalog veröffentlichen:** Es ist jetzt möglich, die dem Katalog mithilfe der veröffentlichen-Module ein Modul mit einer niedrigeren Versionsnummer als die aktuelle Version des vorherigen höchsten veröffentlichen. Wenn ein Verleger Version 2.0.0 ihr Modul mit wichtige Änderungen von den Versionen 1.x veröffentlicht, konnte sie nicht einfach ein Update auf Version 1.5.0 freigegeben. Sie können jetzt eine 1.5.1 veröffentlichen die Unterstützung für die Verwaltung von Modulen in der PowerShell-Galerie verbessert wird. 
- **Bei der Installation von Skripts und Module Ausgabekontext Befehl überprüfen:** Installieren-Module und Installationsskript wird nun überprüft, wenn sie Befehle enthalten, die bereits auf dem System vorhanden sind, und wird von standardmäßig Fehler Out, wenn das geschieht. 
- **Bootstrapping Nuget veröffentlichen-Cmdlets:** zuvor gab es keine Möglichkeit, den NuGet-Anbieter automatisch installiert wird, bei Verwendung von Publish-Module oder veröffentlichen-Skript, das bestimmte Automatisierungsaufgaben schwer vorgenommen. Benutzer können jetzt hinzufügen - Erzwingen des Veröffentlichen-Befehle, die Meldung zu umgehen. 

>Hinweis: Zusätzliche Details über neue Konzepte, Implementierung, Beispiele usw. sollten mit der kanonischen technischen Dokumentation verknüpft werden

**Weitere Informationen zur Verwendung von PowerShellGet**
- [Informationen zu CatalogSigning]()
- []()
- [Filtern der Ergebnisse von Get-Module von CompatiblePSEditions]()
- [Verhindern, dass Ausführung des Skripts, wenn bei einer Comaptible-Edition von PowerShell ausführen.]()






<!--HONumber=Oct16_HO1-->


