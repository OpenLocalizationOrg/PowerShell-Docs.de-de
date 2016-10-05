---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Wichtige Konzepte in diesem Leitfaden
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 178fea44987b0c457b8e5d23fbe851ee12f03b31

---

# Wichtige Konzepte in diesem Leitfaden
**Was genau ist JEA?**

JEA ist eine Erweiterung von [eingeschränkten PowerShell-Endpunkten](http://blogs.technet.com/b/heyscriptingguy/archive/2014/03/31/introduction-to-powershell-endpoints.aspx), die Rollendefinitionen, virtuelle Konten sowie viele weitere Verbesserungen hinzufügt, mit denen Sie Ihre Verwaltungsendpunkte noch besser sichern können.
Ein JEA-Endpunkt besteht aus einer PowerShell-Sitzungskonfigurationsdatei und mindestens einer Rollenfunktionsdatei.

**Was sind die Sitzungskonfiguration und die Funktion?**

PowerShell-Sitzungskonfigurationsdateien (PSSC) definieren, *wer* eine Verbindung zu einem PowerShell-Endpunkt herstellen darf und *wie* diese Sitzung konfiguriert wird.
In diesen Dateien ordnen Sie Benutzer und Sicherheitsgruppen zu bestimmten Verwaltungsrollen zu und konfigurieren globale Einstellungen wie virtuelle Konten und Aufzeichnungsrichtlinien.
Sitzungskonfigurationsdateien sind computerspezifisch, sodass Sie den Zugriff bei Bedarf für jeden Computer einzeln steuern können.

PowerShell-Rollenfunktionsdateien (PSRC) definieren, *was* Benutzer, die dieser Rolle angehören, auf dem System tun können.
Hier können Sie festlegen, welche Cmdlets, Funktionen, Anbieter und externen Programme ein Benutzer während einer JEA-Sitzung verwenden darf.
Rollenfunktionsdateien gelten häufig allgemein für die jeweilige Rolle (DNS-Administrator, Level-1-Helpdesk, schreibgeschützte Bestandsüberwachung usw.) und gehören zu PowerShell-Modulen, sodass sie problemlos in Ihrer Umgebung und für andere Benutzer freigegeben werden können.

**Wie nutzt JEA virtuelle Konten?**

In der PowerShell-Sitzungskonfigurationsdatei können Sie JEA-Sitzungen so konfigurieren, dass sie virtuelle ausführende Konten verwenden.
Virtuelle Konten sind einmalige privilegierte Konten. Sie werden nur für genau den Benutzer angelegt, der in genau der Sitzung eine Verbindung herstellt, in deren Kontext die Befehle des Benutzers ausgeführt werden.
Virtuelle Konten gehören standardmäßig zur Sicherheitsgruppe „Administratoren“, können optional jedoch so konfiguriert werden, dass sie nur den von Ihnen angegebenen Sicherheitsgruppen angehören.

**PowerShell-Remoting**: Mithilfe von PowerShell-Remoting können Sie PowerShell-Befehle auf Remotecomputern ausführen.
Sie können einen oder mehrere Computer sowie temporäre oder dauerhafte Verbindungen verwenden.
In dieser Demo haben Sie in einer interaktiven Sitzung eine Remoteverbindung mit Ihrem lokalen Computer hergestellt.
JEA schränkt die Funktionalität ein, die über PowerShell-Remoting verfügbar ist.
Um weitere Informationen zu PowerShell zu erhalten, führen Sie `Get-Help about_Remote` aus.

**Ausführender Benutzer (RunAs)**: Bei der Verwendung von JEA führt ein Benutzer ohne Administratorrechte ein privilegiertes virtuelles Konto im Modus „Ausführen als“ aus.
Das virtuelle Konto besteht nur für die Dauer der Remotesitzung.
Das heißt: Das Konto wird erstellt, wenn ein Benutzer eine Verbindung mit dem Endpunkt herstellt, und wieder gelöscht, wenn der Benutzer die Sitzung beendet.
Standardmäßig ist das virtuelle Konto Mitglied der lokalen Administratorengruppe.
Auf einem Domänencontroller ist es Mitglied der Gruppe „Domänen-Admins“.
Virtuelle Konten bestehen nur lokal auf dem Computer, auf dem sie erstellt wurden, und verfügen über keine Berechtigungen außerhalb dieses Computers.
Dies bedeutet, dass sie nicht in Active Directory registriert sind (ihnen ist keine RID zugewiesen).
Außerdem gilt: Wenn ein zulässiger Befehl bzw. ein zulässiges Skript versucht, auf Ressourcen außerhalb des lokalen Computers zuzugreifen, erfolgt der Zugriff auf diese Ressourcen unter der Identität des Computers, nicht der Identität des virtuellen Kontos.

**Verbundener Benutzer (Connected)**: Der Benutzer ohne Administratorrechte, der eine Verbindung mit dem JEA-Endpunkt herstellt und dem Rollen zugewiesen sind.
Alle Befehle, die dieser Benutzer ausführt, werden im Kontext des ausführenden Benutzers oder des virtuellen ausführenden Kontos ausgeführt.




<!--HONumber=Oct16_HO1-->


