# Just Enough Administration (JEA)
Just Enough Administration ist ein neues WMF 5.0-Feature, das mithilfe von PowerShell-Remoting eine rollenbasierte Verwaltung ermöglicht.  Es erweitert die vorhandene eingeschränkte Endpunktinfrastruktur, indem Nicht-Administratoren das Ausführen bestimmter Befehle, Skripts und ausführbarer Dateien als Administrator erlaubt wird.  Dadurch können Sie die Anzahl der Volladministratoren in Ihrer Umgebung verringern und die Sicherheit erhöhen.  Mit JEA kann alles verwaltet werden, was mit PowerShell verwaltet werden kann, und zwar mit höherer Sicherheit.  In [dieser Anleitung](http://aka.ms/JEA) finden Sie eine detaillierte Erläuterung von Just Enough Administration.

Im Gegensatz zu alten eingeschränkten Endpunkten ist JEA sowohl leistungsfähig als auch einfach zu konfigurieren.  Benutzeroptionen in JEA können präzise gesteuert werden. Das geht soweit, dass die Parametersätze und Werte einschränkbar sind, die für einen bestimmten Befehl angegeben werden können. Die Aktionen des Benutzers werden im Kontext eines einmaligen virtuellen Kontos ausgeführt, das über die Rechte zum Ausführen der Administratoraktionen verfügt.  Die Befehle, die vom Benutzer aufgerufen werden, können für Sicherheitsüberwachungen protokolliert werden.

JEA basiert auf dem Erstellen speziell konfigurierter eingeschränkter Endpunkte.  Diese Endpunkte haben einige wichtige Merkmale:

1. Benutzer, die sich mit ihnen verbinden, nutzen auf Grundlage von „Ausführen als“ ein privilegiertes virtuelles Konto, das es nur für die Dauer dieser Remotesitzung gibt.  Standardmäßig ist dieses virtuelle Konto Mitglied der integrierten Gruppe „Administratoren“ und der Gruppe „Domänen-Admins“ auf Domänencontrollern (Hinweis: Diese Berechtigungen sind konfigurierbar). Durch Herstellen einer Verbindung als ein Benutzer und anschließendes Nutzen des Kontos eines anderen privilegierten Benutzers können Sie nicht privilegierten Benutzern erlauben, bestimmte Verwaltungsaufgaben auszuführen, ohne ihnen Administratorrechte für Ihr System zu erteilen.
2. Der Endpunkt ist gesperrt.  Dies bedeutet, dass PowerShell im Modus „Keine Sprache“ ausgeführt wird.  Nur bestimmte Befehle, Skripts und ausführbare Dateien sind für den Benutzer sichtbar.
3. Anderen Benutzern, die eine Verbindung herstellen, wird basierend auf der Gruppenmitgliedschaft eine andere Gruppe von Funktionen angezeigt.  Sie können am selben Endpunkt verschiedene Rollen mit verschiedenen Möglichkeiten einrichten.

<!--HONumber=Oct16_HO1-->


