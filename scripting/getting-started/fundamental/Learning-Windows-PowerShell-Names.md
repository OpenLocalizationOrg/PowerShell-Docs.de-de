---
title: Erlernen von Windows PowerShell-Namen
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: b4d0fd22-8298-4ee6-82ae-9b6f2907c986
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1dd7dade688b56e3ac544c0562ad6bad4a4284a7

---

# Erlernen von Windows PowerShell-Namen
Das Erlernen der Namen von Befehlen und Befehlsparametern ist bei den meisten Befehlszeilenschnittstellen mit einem hohen Zeitaufwand verbunden. Das Problem ist, dass es nur sehr wenige Muster gibt. Deshalb ist die einzige Lernmöglichkeit das Auswendiglernen sämtlicher Befehle und Parameter, die Sie regelmäßig verwenden müssen.

Wenn Sie mit einem neuen Befehl oder Parameter arbeiten, können Sie meist nicht darauf zurückgreifen, was Sie bereits wissen. Sie müssen einen neuen Namen finden und erlernen. Wenn Sie sich ansehen, wie Schnittstellen ausgehend von einer kleinen Gruppe von Tools mit schrittweisen Ergänzungen der Funktionalität wachsen, ist es nicht schwer zu verstehen, warum die Struktur keinem Standard entspricht. Insbesondere bei Befehlsnamen mag dies logisch klingen, da jeder Befehl ein eigenes Tool ist, doch es gibt eine bessere Möglichkeit, mit Befehlsnamen zurecht zu kommen.

Die meisten Befehle werden erstellt, um Elemente des Betriebssystems oder von Anwendungen, z. B. Dienste oder Prozesse, zu verwalten. Die Befehle haben eine Vielzahl von Namen, die zu einer Familie gehören oder auch nicht. Auf Windows-Systemen können Sie z. B. die Befehle **net start** und **net stop** zum Starten und Beenden eines Diensts verwenden. Es gibt ein anderes allgemeineres Dienststeuerungstool für Windows mit dem völlig anderen Namen **sc**, der nicht zum Namensmuster der **net**-Befehle für Dienste passt. Für die Verwaltung von Prozessen bietet Windows den Befehl **tasklist** zum Auflisten von Prozessen und den Befehl **taskkill** zum Beenden von Prozessen.

Befehle, die Parameter verwenden, weisen unregelmäßige Parameterangaben auf. Sie können den Befehl **net start** ausführen, um einen Dienst auf einem Remotecomputer zu starten. Der Befehl **sc** Befehl startet einen Dienst auf einem Remotecomputer. Doch zum Angeben des Remotecomputers müssen Sie vor dessen Namen einen doppelten umgekehrten Schrägstrich setzen. Um z. B. den Spoolerdienst auf einem Remotecomputer mit dem Namen DC01 zu starten, geben Sie **sc \\DC01 start spooler** ein. Zum Auflisten von Aufgaben, die auf DC01 ausgeführt werden, müssen Sie den Parameter **/S** (für „system“) und den Namen DC01 ohne umgekehrte Schrägstriche angeben, z.B. so: **tasklist /S DC01**.

Obwohl es wichtige technische Unterschiede zwischen einem Dienst und einem Prozess gibt, sind beide Beispiele verwaltbarer Elemente auf einem Computer, die einen überlegt definierten Lebenszyklus haben. Sie können einen Dienst oder Prozess starten oder beenden oder eine Liste aller derzeit ausgeführten Dienste und Prozesse abrufen. Obgleich also ein Dienst und ein Prozess nicht dasselbe sind, lassen sich die Aktionen, die wir auf einen Dienst oder Prozess anwenden, häufig konzeptionell vergleichen. Darüber hinaus können auch Entscheidungen, die wir zum Anpassen einer Aktion durch das Angeben von Parametern treffen, konzeptionell vergleichbar sein.

Windows PowerShell nutzt diese Ähnlichkeiten zum Verringern der Anzahl unterschiedlicher Namen, die Sie kennen müssen, um Cmdlets zu verstehen und zu nutzen.

### Cmdlets bestehen aus einer Verb- und einer Nomenkomponente, damit Befehlsnamen einprägsamer sind
Windows PowerShell arbeitet mit einem Benennungssystem nach dem Muster „Verb-Nomen“, bei dem jeder Cmdlet-Name aus einem Standardverb besteht, das mittels Bindestrich mit einem bestimmten Nomen verbunden ist. Windows PowerShell-Verben sind nicht immer englische Verben, drücken aber bestimmte Aktionen in Windows PowerShell aus. Nomen sind sehr ähnlich wie Nomen in einer beliebigen Sprache. Sie beschreiben bestimmte Arten von Objekten, die bei der Systemadministration wichtig sind. Es ist einfach zu veranschaulichen, wie zweiteilige Namen den Lernaufwand verringern, indem wir uns einige Beispiele von Verben und Nomen ansehen.

Nomen sind weniger eingeschränkt, sollten aber stets beschreiben, auf was ein Befehl angewendet wird. Windows PowerShell bietet Befehle wie **Get-Process**, **Stop-Process**, **Get-Service** und **Stop-Service**.

Im Fall von zwei Nomen und zwei Verben, erleichtert Konsistenz das Erlernen nicht wesentlich. Doch wenn Sie sich einen Standardsatz von 10 Verben und 10 Nomen anschauen, müssen Sie nur 20 Wörter verstehen, die aber genutzt werden können, um 100 verschiedene Befehlsnamen zu bilden.

In vielen Fällen können Sie erkennen, was ein Befehl bewirkt, indem Sie seinen Namen lesen. Deshalb ist es meist offensichtlich, welcher Name für einen neuen Befehl verwendet werden sollte. Ein Befehl zum Herunterfahren eines Computers kann z.B. **Stop-Computer** lauten. Ein Befehl zum Abrufen aller Computer in einem Netzwerk kann **Get-Computer** heißen. Der Befehl zum Abrufen des Systemdatums heißt **Get-Date**.

Mit dem **-Verb**-Parameter für **Get-Command** können Sie alle Befehle auflisten, die ein bestimmtes Verb enthalten. (**Get-Command** wird im nächsten Abschnitt ausführlich behandelt.) Um beispielsweise alle Cmdlets anzuzeigen, die das Verb **Get** enthalten, geben Sie Folgendes ein:

```
PS> Get-Command -Verb Get
CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Get-Acl                         Get-Acl [[-Path] <String[]>]...
Cmdlet          Get-Alias                       Get-Alias [[-Name] <String[]...
Cmdlet          Get-AuthenticodeSignature       Get-AuthenticodeSignature [-...
Cmdlet          Get-ChildItem                   Get-ChildItem [[-Path] <Stri...
...
```

Der **-Noun**-Parameter ist noch nützlicher, da Sie eine Familie von Befehlen anzeigen können, die sich auf den gleichen Objekttyp auswirken. Wenn Sie z. B. anzeigen möchten, welche Befehle zum Verwalten von Diensten verfügbar sind, geben Sie folgenden Befehl ein:

```
PS> Get-Command -Noun Service
CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Get-Service                     Get-Service [[-Name] <String...
Cmdlet          New-Service                     New-Service [-Name] <String>...
Cmdlet          Restart-Service                 Restart-Service [-Name] <Str...
Cmdlet          Resume-Service                  Resume-Service [-Name] <Stri...
Cmdlet          Set-Service                     Set-Service [-Name] <String>...
Cmdlet          Start-Service                   Start-Service [-Name] <Strin...
Cmdlet          Stop-Service                    Stop-Service [-Name] <String...
Cmdlet          Suspend-Service                 Suspend-Service [-Name] <Str... 
...
```

Ein Befehl ist nicht unbedingt ein Cmdlet, nur weil er das Benennungsschema „Verb-Nomen“ hat. Ein Beispiel eines nativen Windows PowerShell-Befehls, der kein Cmdlet ist, aber einen „Verb-Nomen“-Namen hat, ist der Befehl zum Leeren eines Konsolenfensters, „Clear-Host“. Der Befehl „Clear-Host“ ist eigentlich eine interne Funktion, was Sie sehen können, wenn Sie „Get-Command“ dafür aufrufen:

```
PS> Get-Command -Name Clear-Host

CommandType     Name                            Definition
-----------     ----                            ----------
Function        Clear-Host                      $spaceType = [System.Managem...
```

### Cmdlets verwenden Standardparameter
Wie bereits erwähnt, weisen Befehle, die bei herkömmlichen Befehlsschnittstellen verwendet werden, in der Regel keine konsistenten Parameternamen auf. Mitunter haben Parameter überhaupt keine Namen. Falls doch, handelt es sich oft um einzelne Zeichen oder abgekürzte Wörter, die schnell eingegeben werden können, aber für neue Benutzern nicht leicht verständlich sind.

Im Gegensatz zu den meisten anderen herkömmlichen Befehlszeilenschnittstellen verarbeitet Windows PowerShell Parameter direkt und nutzt diesen direkten Zugriff auf die Parameter (zusammen mit Anleitungen für Entwickler), um Parameternamen zu standardisieren. Obwohl dies nicht garantiert, dass jedes Cmdlet immer den Standards entspricht, wird dies angeregt.

> [!NOTE]
> Parameternamen ist stets ein „-“ vorangestellt, damit Windows PowerShell diese eindeutig als Parameter identifizieren kann. Beim Beispiel **Get-Command -Name Clear-Host** lautet der Name des Parameters **Name**, der jedoch als **-Name** eingegeben wird.

Es folgen einige allgemeine Merkmale der Standardparameternamen und ihre Verwendungen.

#### Der Parameter „Hilfe“ (?)
Wenn Sie den Parameter **-?** für ein Cmdlet angeben, wird das Cmdlet nicht ausgeführt. Stattdessen zeigt Windows PowerShell Hilfe für das Cmdlet an.

#### Allgemeine Parameter
Windows PowerShell verfügt über mehrere Parameter, die als *allgemeine Parameter* bezeichnet werden. Da diese Parameter vom Windows PowerShell-Modul gesteuert werden, weisen Sie bei Implementierung durch ein Cmdlet stets dasselbe Verhalten auf. Die allgemeinen Parameter heißen **WhatIf**, **Confirm**, **Verbose**, **Debug**, **Warn**, **ErrorAction**, **ErrorVariable**, **OutVariable** und **OutBuffer**.

#### Empfohlene Parameter
Die Windows PowerShell-Kern-Cmdlets verwenden standardmäßige Namen für ähnliche Parameter. Wenngleich die Verwendung von Parameternamen nicht erzwungen wird, gibt es zur Förderung der Standardisierung explizite Anleitungen für die Verwendung.

In den Anleitungen wird beispielsweise empfohlen, für einen Parameter, der anhand des Namens auf einen Computer verweist, die Bezeichnung **ComputerName** anstatt „Server“, „Host“, „System“, „Node“ oder ein anderes gängiges alternatives Wort zu wählen. Zu den empfohlenen Parameternamen gehören **Force**, **Exclude**, **Include**, **PassThru**, **Path** und **CaseSensitive**.




<!--HONumber=Oct16_HO1-->


