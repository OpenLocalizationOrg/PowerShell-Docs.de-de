---
title: "Verwalten von Prozessen mit „Process“-Cmdlets"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 5038f612-d149-4698-8bbb-999986959e31
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 5a635485387bb367f4e43982085f9d36765a95e5

---

# Verwalten von Prozessen mit „Process“-Cmdlets
Sie können die „Process“-Cmdlets in Windows PowerShell verwenden, um lokale und Remoteprozesse in Windows PowerShell zu verwalten.

## Abrufen von Prozessen (Get-Process)
Um die Prozesse abzurufen, die auf dem lokalen Computer ausgeführt werden, führen Sie **Get-Process** ohne Parameter aus.

Sie können bestimmte Prozesse abrufen, indem Sie deren Prozessnamen oder Prozess-IDs angeben. Der folgende Befehl ruft den Prozess „Idle“ ab:

```
PS> Get-Process -id 0
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
      0       0        0         16     0               0 Idle
```

Obwohl es für Cmdlets normal ist, dass sie in einigen Fällen keine Daten zurückgeben, generiert **Get-Process** einen Fehler, wenn Sie einen Prozess über seine Prozess-ID (ProcessId) angeben, das Cmdlet aber keine Übereinstimmung finden kann. Dies ist darin begründet, dass die eigentliche Absicht ist, dass ein bekannter aktiver Prozess abgerufen wird. Gibt es keinen Prozess mit dieser ID, ist es sehr wahrscheinlich, dass die ID falsch ist oder der Prozess bereits beendet wurde:

```
PS> Get-Process -Id 99
Get-Process : No process with process ID 99 was found.
At line:1 char:12
+ Get-Process  <<<< -Id 99
```

Sie können den „Name“-Parameter des Cmdlets „Get-Process“ verwenden, um anhand des Prozessnamens eine Teilmenge von Prozessen anzugeben. Der „Name“-Parameter akzeptiert mehrere Namen in einer durch Kommas getrennten Liste und unterstützt die Verwendung von Platzhaltern, sodass Sie Namensmuster eingeben können.

Beispielsweise ruft der folgende Befehl Prozesse ab, deren Namen mit „ex“ beginnen.

```
PS> Get-Process -Name ex*
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    234       7     5572      12484   134     2.98   1684 EXCEL
    555      15    34500      12384   134   105.25    728 explorer
```

Weil die .NET-Klasse „System.Diagnostics.Process“ die Grundlage für Windows PowerShell-Prozesse ist, gelten für diese einige der Konventionen, die von „System.Diagnostics.Process“ verwendet werden. Eine dieser Konventionen ist, dass der Prozessname für eine ausführbare Datei nie das „.exe“ am Ende des Namens der ausführbaren Datei enthält.

**Get-Process** akzeptiert auch mehrere Werte für den „Name“-Parameter.

```
PS> Get-Process -Name exp*,power* 
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    540      15    35172      48148   141    88.44    408 explorer
    605       9    30668      29800   155     7.11   3052 powershell
```

Sie können den „ComputerName“-Parameter von „Get-Process“ verwenden, um Prozesse auf Remotecomputern abzurufen. Beispielsweise ruft der folgende Befehl die PowerShell-Prozesse auf dem lokalen Computer (dargestellt durch „localhost“) und auf zwei Remotecomputern ab.

```
PS> Get-Process -Name PowerShell -ComputerName localhost, Server01, Server02
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    258       8    29772      38636   130            3700 powershell
    398      24    75988      76800   572            5816 powershell
    605       9    30668      29800   155     7.11   3052 powershell
```

Die Computernamen sind in dieser Anzeige nicht zu sehen, sie sind aber in der „MachineName“-Eigenschaft der Prozessobjekte gespeichert, die „Get-Process“ zurückgibt. Im folgenden Befehl wird das Cmdlet „Format-Table“ verwendet, um die Eigenschaften Prozess-„ID“, „ProcessName“ und „MachineName“ (ComputerName) der Prozessobjekte anzuzeigen.

```
PS> Get-Process -Name PowerShell -ComputerName localhost, Server01, Server01 | Format-Table -Property ID, ProcessName, MachineName
  Id ProcessName MachineName
  -- ----------- -----------
3700 powershell  Server01
3052 powershell  Server02
5816 powershell  localhost
```

In diesem komplexeren Befehl wird der standardmäßigen „Get-Process“-Anzeige die „MachineName“-Eigenschaft hinzugefügt. Das Graviszeichen (`) (ASCII 96) ist das Windows PowerShell-Fortsetzungszeichen.

```
get-process powershell -computername localhost, Server01, Server02 | format-table -property Handles, `
                    @{Label="NPM(K)";Expression={[int]($_.NPM/1024)}}, `
                    @{Label="PM(K)";Expression={[int]($_.PM/1024)}}, `
                    @{Label="WS(K)";Expression={[int]($_.WS/1024)}}, `
                    @{Label="VM(M)";Expression={[int]($_.VM/1MB)}}, `
                    @{Label="CPU(s)";Expression={if ($_.CPU -ne $()` 
                    {$_.CPU.ToString("N")}}}, `                                                                         
                    Id, ProcessName, MachineName -auto

Handles  NPM(K)  PM(K) WS(K) VM(M) CPU(s)  Id ProcessName  MachineName
-------  ------  ----- ----- ----- ------  -- -----------  -----------
    258       8  29772 38636   130         3700 powershell Server01
    398      24  75988 76800   572         5816 powershell localhost
    605       9  30668 29800   155 7.11    3052 powershell Server02
```

## Beenden von Prozessen (Stop-Process)
Windows PowerShell gibt Ihnen die Flexibilität zum Auflisten von Prozessen, aber wie sieht es mit dem Beenden eines Prozesses aus?

Das Cmdlet **Stop-Process** akzeptiert einen Namen oder eine ID als Angabe für den Prozess, den Sie beenden möchten. Inwieweit Sie Prozesse beenden können, hängt von Ihren Berechtigungen ab. Einige Prozesse können nicht beendet werden. Wenn Sie beispielsweise versuchen, den Prozess „idle“ zu beenden, erhalten Sie einen Fehler:

```
PS> Stop-Process -Name Idle
Stop-Process : Process 'Idle (0)' cannot be stopped due to the following error:
 Access is denied
At line:1 char:13
+ Stop-Process  <<<< -Name Idle
```

Mit dem **Force**-Parameter können Sie auch eine Eingabeaufforderung erzwingen. Dieser Parameter ist insbesondere nützlich, wenn Sie für die Angabe des Prozessnamens einen Platzhalter verwenden, denn möglicherweise ergibt sich versehentlich eine Übereinstimmung für einige Prozesse, die Sie nicht beenden möchten:

```
PS> Stop-Process -Name t*,e* -Confirm
Confirm
Are you sure you want to perform this action?
Performing operation "Stop-Process" on Target "explorer (408)".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):n
Confirm
Are you sure you want to perform this action?
Performing operation "Stop-Process" on Target "taskmgr (4072)".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):n
```

Komplexe Prozessverarbeitung ist möglich, indem Sie einige der Cmdlets zur Objektfilterung verwenden. Weil ein Prozessobjekt eine „Responding“-Eigenschaft hat, die gleich „false“ ist, wenn das Prozessobjekt nicht mehr reagiert, können Sie alle nicht reagierenden Anwendungen mit dem folgenden Befehl beenden:

```
Get-Process | Where-Object -FilterScript {$_.Responding -eq $false} | Stop-Process
```

Sie können die gleiche Vorgehensweise in anderen Situationen verwenden. Nehmen Sie beispielsweise an, dass automatisch eine sekundäre Anwendung für den Infobereich ausgeführt wird, wenn der Benutzer eine andere Anwendung startet. Sie stellen möglicherweise fest, dass dies in Terminaldienstesitzungen nicht ordnungsgemäß funktioniert, aber Sie möchten es in Sitzungen beibehalten, die in der Konsole des physischen Computers ausgeführt werden. Sitzungen, die mit dem Desktop eines physischen Computers verbunden sind, haben immer die Sitzungs-ID „0“, also können Sie alle Instanzen des Prozesses, die sich in anderen Sitzungen befinden, beenden, indem Sie **Where-Object** und die Prozesssitzungs-ID (**SessionId**) verwenden:

```
Get-Process -Name BadApp | Where-Object -FilterScript {$_.SessionId -neq 0} | Stop-Process
```

Das Cmdlet „Stop-Process“ hat keinen ComputerName-Parameter. Daher müssen Sie, wenn Sie auf einem Remotecomputer einen Befehl zum Beenden eines Prozesses ausführen möchten, das Cmdlet „Invoke-Command“ verwenden. Wenn Sie beispielsweise den PowerShell-Prozess auf dem Remotecomputer „Server01“ beenden möchten, geben Sie Folgendes ein:

```
Invoke-Command -ComputerName Server01 {Stop-Process Powershell}
```

## Beenden alle anderen Windows PowerShell-Sitzungen
Gelegentlich kann es nützlich sein, alle aktiven Windows PowerShell-Sitzungen mit Ausnahme der aktuellen Sitzung zu beenden. Wenn eine Sitzung zu viele Ressourcen verwendet, oder wenn nicht mehr auf sie zugegriffen werden kann (sie wird möglicherweise remote oder in einer anderen Desktopsitzung ausgeführt), kann es sein, dass Sie sie nicht direkt beenden können. Wenn Sie aber versuchen, alle aktiven Sitzungen zu beenden, wird möglicherweise stattdessen die aktuelle Sitzung beendet.

Jede Windows PowerShell-Sitzung hat die Umgebungsvariable PID, die die ID des jeweiligen Windows PowerShell-Prozesses enthält. Sie können die $PID mit der ID jeder Sitzung abgleichen und nur die Windows PowerShell-Sitzungen beenden, die eine andere ID haben. Im folgenden Pipelinebefehl wird dies ausgeführt und die Liste der beendeten Sitzungen zurückgegeben (weil der **PassThru**-Parameter verwendet wird):

```
PS> Get-Process -Name powershell | Where-Object -FilterScript {$_.Id -ne $PID} | Stop-Process -
PassThru
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    334       9    23348      29136   143     1.03    388 powershell
    304       9    23152      29040   143     1.03    632 powershell
    302       9    20916      26804   143     1.03   1116 powershell
    335       9    25656      31412   143     1.09   3452 powershell
    303       9    23156      29044   143     1.05   3608 powershell
    287       9    21044      26928   143     1.02   3672 powershell
```

## Starten und Debuggen von Prozessen sowie Warten auf Prozesse
Zu Windows PowerShell gehören auch Cmdlets zum Starten (oder Neustarten) eines Prozesses, zum Debuggen eines Prozesses sowie zum Warten, bis ein Prozess abgeschlossen ist, bevor ein Befehl ausgeführt wird. Informationen zu diesen Cmdlets finden Sie im Cmdlet-Hilfethema für jedes Cmdlet.

## Weitere Informationen
[Get-Process [m2]](https://technet.microsoft.com/en-us/library/27a05dbd-4b69-48a3-8d55-b295f6225f15)
[Stop-Process [m2]](https://technet.microsoft.com/en-us/library/12454238-9881-457a-bde4-fb6cd124deec)
[Start-Process](https://technet.microsoft.com/en-us/library/41a7e43c-9bb3-4dc2-8b0c-f6c32962e72c)
[Wait-Process](https://technet.microsoft.com/en-us/library/9222af7a-789d-4a09-aa90-09d7c256c799)
[Debug-Process](https://technet.microsoft.com/en-us/library/eea1dace-3913-4dbd-b659-5a94a610eee1)
[Invoke-Command](https://technet.microsoft.com/en-us/library/22fd98ba-1874-492e-95a5-c069467b8462)




<!--HONumber=Oct16_HO1-->


