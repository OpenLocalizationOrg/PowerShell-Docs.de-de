---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Berichterstellung zu JEA
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d867a6462e9fa8b6e16c8c2103899c72b380116c

---

# Berichterstellung zu JEA
Da JEA es nicht privilegierten Benutzern erlaubt, in einem privilegierten Kontext zu arbeiten, sind Protokollierung und Überprüfung extrem wichtig.
In diesem Abschnitt geht es um die Tools, die Sie für die Protokollierung und Berichterstellung verwenden können.

## Berichterstellung zu JEA-Aktionen
### Mitschnitt einer Aufzeichnung
Eine der schnellsten Möglichkeiten, sich einen Überblick über die Aktivitäten in einer PowerShell-Sitzung zu verschaffen, ist es, der Person, die Daten eingibt, über die Schulter zu schauen.
So sehen Sie die eingegebenen Befehle, die Ausgabe dieser Befehle, und alles ist gut.
Vielleicht ist auch nicht alles gut, aber zumindest wissen Sie dann Bescheid.
PowerShell-Aufzeichnungen bieten eine ähnliche Ansicht nach Abschluss der Aktivitäten.

Wenn Sie das Feld „TranscriptDirectory“ in Ihrer Sitzungskonfiguration verwenden, zeichnet PowerShell automatisch alle Aktionen in einer bestimmten Sitzung auf.
Sie finden die Aufzeichnungen Ihrer Sitzungen in diesem Dokument: $env:ProgramData\JEAConfiguration\Transcripts.

Wie Sie sehen, werden Informationen über den verbundenen Benutzer, den ausführenden Benutzer, die in dieser Sitzung ausgeführten Befehle und vieles mehr aufgezeichnet.
Weitere Informationen über PowerShell-Aufzeichnungen finden Sie in diesem [Blogbeitrag](http://blogs.msdn.com/b/powershell/archive/2015/06/09/powershell-the-blue-team.aspx).

### PowerShell-Ereignisprotokolle
Wenn die Modulprotokollierung aktiviert ist, werden alle PowerShell-Aktionen auch in regulären Windows-Ereignisprotokollen aufgezeichnet.
Diese sind im Vergleich zu Aufzeichnungen etwas schwieriger zu lesen, aber die Detailgenauigkeit kann sehr hilfreich sein.

Im Betriebsprotokoll „PowerShell“ wird unter der Ereignis-ID 4104 jeder aufgerufene Befehl aufgezeichnet, wenn die Modulprotokollierung aktiviert ist.

### Weitere Ereignisprotokolle
Im Gegensatz zu PowerShell-Protokollen und -Aufzeichnungen erfassen andere Protokollierungsmechanismen nicht den „verbundenen Benutzer“.
Sie müssen PowerShell-Protokolle und andere Protokolle korrelieren, um durchgeführte Aktionen zu vergleichen.

Im Betriebsprotokoll „Windows Remote Management“ werden unter der Ereignis-ID 193 die SID und der Name des Benutzers aufgezeichnet, der die Verbindung herstellt. Zur Unterstützung der Korrelation wird auch die SID des virtuellen ausführenden Kontos aufgezeichnet.
Sie haben sicherlich auch bemerkt, dass der Name des virtuellen ausführenden Kontos am Ende die Domäne und den Benutzernamen des Benutzers enthält, der die Verbindung herstellt.

## Berichterstellung für die JEA-Konfiguration
### Get-PSSessionConfiguration
Um einen exakten Bericht über den Status Ihrer Umgebung zu erhalten, müssen Sie wissen, wie viele JEA-Endpunkte Sie auf Ihrem Computer eingerichtet haben.
`Get-PSSessionConfiguration` ist genau das.

### Get-PSSessionCapability
Die manuelle Berichterstellung zu den Funktionen eines bestimmten Benutzers über einen JEA-Endpunkt kann ziemlich komplex sein.
Sie müssen wahrscheinlich mehrere verschiedene Rollenfunktionen überprüfen.
Glücklicherweise macht das Cmdlet „Get-PSSessionCapability“ genau das.

Führen Sie zum Testen den folgenden Befehl von einer PowerShell-Administratoreingabeaufforderung aus:
```PowerShell
Get-PSSessionCapability -Username 'CONTOSO\OperatorUser' -ConfigurationName JEADemo
```




<!--HONumber=Oct16_HO1-->


