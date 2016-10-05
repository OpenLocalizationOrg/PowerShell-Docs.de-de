# Problembehandlung bei DSC222

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

In diesem Thema werden die Methoden zum fehlerfreien Ausführen Ihrer DSC-Skripts (Desired State Configuration, Konfiguration für den gewünschten Zustand) beschrieben. Die effektive Verwendung von Protokollen zum Auffinden von Fehlern und das Verständnis der Vorgehensweise zum Recyceln von Cache, um die unmittelbaren Ergebnisse Ihrer Ressourcenänderungen anzuzeigen, ermöglichen eine effizientere Problemhehandlung von DSC. Diese Techniken werden in zwei Abschnitten behandelt:

* Mein Skript wird nicht ausgeführt: **Verwenden von DSC-Protokollen für die Diagnose von Skriptfehlern**
* Meine Ressourcen werden nicht aktualisiert: **Zurücksetzen des Caches**

## Mein Skript wird nicht ausgeführt: Verwenden von DSC-Protokollen für die Diagnose von Skriptfehlern

Wie alle Windows-Softwareprogramme erfasst DSC Fehler und Ereignisse in [Protokollen](https://msdn.microsoft.com/library/windows/desktop/aa363632.aspx), die Sie in der [Ereignisanzeige](http://windows.microsoft.com/windows/what-information-event-logs-event-viewer) anzeigen können. Die Durchsicht dieser Protokolle kann Ihnen dabei helfen herauszufinden, warum ein bestimmter Vorgang fehlgeschlagen ist und wie Sie Fehler in Zukunft vermeiden. Das Schreiben von Konfigurationsskripts ist nicht ganz einfach. Sie sollten daher den Fortschritt der Konfiguration im Ereignisprotokoll der DSC-Analyse mithilfe der DSC-Protokollressource verfolgen, um Fehler bei der Erstellung leichter aufspüren zu können.

## Wo befinden sich die DSC-Ereignisprotokolle?

In der Ereignisanzeige werden DSC-Ereignisse unter **Anwendungs- und Dienstprotokolle/Microsoft/Windows/Desired State Configuration** angezeigt.

Die können auch das entsprechende PowerShell-Cmdlet [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) ausführen, um die Ereignisprotokolle anzuzeigen:

```
PS C:\Users> Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                                                                                  
-----------                     -- ---------------- -------                                                                                                  
11/17/2014 10:27:23 PM        4102 Information      Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
```

Wie oben gezeigt, lautet der primäre Protokollname von DSC **Microsoft -> Windows -> DSC** (andere Protokollnamen unter Windows werden hier aus Gründen der Übersichtlichkeit nicht dargestellt). Der primäre Name wird an den Namen des Kanals angefügt. Daraus ergibt sich der vollständige Protokollname. Das DSC-Modul schreibt hauptsächlich in drei Protokolltypen: [Betriebsprotokoll, analytisches Protokoll und Debugprotokoll](https://technet.microsoft.com/library/cc722404.aspx). Da die analytischen Protokolle und die Debugprotokolle standardmäßig deaktiviert sind, sollten Sie sie in der Ereignisanzeige aktivieren. Öffnen Sie dazu die Ereignisanzeige, indem Sie in Windows PowerShell „eventvwr“ eingeben. Oder klicken Sie auf die Schaltfläche **Start** und dann auf **Systemsteuerung**, **Verwaltung** und **Ereignisanzeige**. Klicken Sie in der Ereignisanzeige im Menü **Ansicht** auf **Analytische und Debugprotokolle einblenden**. Der Name des für den analytischen Kanal lautet **Microsoft-Windows-Dsc/Analytic**, und der Debugkanal heißt **Microsoft-Windows-Dsc/Debug**. Außerdem können Sie zum Aktivieren der Protokolle das Hilfsprogramm [wevtutil](https://technet.microsoft.com/library/cc732848.aspx) verwenden, wie im folgenden Beispiel gezeigt.

```powershell
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
```

## Was ist in den DSC-Protokollen enthalten?

DSC-Protokolle werden, basierend auf der Wichtigkeit der Meldung, auf die drei Protokollkanäle verteilt. Das Betriebsprotokoll in DSC enthält alle Fehlermeldungen und kann verwendet werden, um ein Problem zu identifizieren. Das analytische Protokoll enthält eine größere Anzahl von Ereignissen und dient dazu zu ermitteln, wo ein Fehler aufgetreten ist. Dieser Kanal enthält auch ausführliche Meldungen (falls vorhanden). Das Debugprotokoll enthält Protokolle, die Ihnen helfen zu verstehen, wie der Fehler entstanden ist. DSC-Ereignismeldungen sind so aufgebaut, dass jede Ereignismeldung mit eine Auftrags-ID beginnt, die einen DSC-Vorgang eindeutig identifiziert. Im folgenden Beispiel wird versucht, die Meldung über das erste Ereignis abzurufen, das im DSC-Betriebsprotokoll protokolliert wurde.

```powershell
PS C:\Users> $AllDscOpEvents=get-winevent -LogName "Microsoft-Windows-Dsc/Operational"
PS C:\Users> $FirstOperationalEvent=$AllDscOpEvents[0]
PS C:\Users> $FirstOperationalEvent.Message
Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
Consistency engine was run successfully. 
```

DSC-Ereignisse werden in einer bestimmten Struktur protokolliert, das dem Benutzer ermöglicht, Ereignisse eines DSC-Auftrags zusammenzufassen. Die Struktur sieht wie folgt aus:

**Auftrags-ID: \<Guid\>**
**\<ereignismeldung\>**

## Sammeln von Ereignissen zu einem einzelnen DSC-Vorgang

DSC-Ereignisprotokolle enthalten Ereignisse, die mithilfe verschiedener DSC-Vorgänge generiert wurden. Allerdings interessieren Sie sich in der Regel nur für die Details zu einem bestimmten Vorgang. Alle DSC-Protokolle können nach der Auftrags-ID-Eigenschaft gruppiert werden, die für jeden DSC-Vorgang eindeutig ist. Die Auftrags-ID wird in allen DSC-Ereignissen als erster Eigenschaftswert angezeigt. Die folgenden Schritte erläutern, wie Sie alle Ereignisse in einer gruppierten Arraystruktur sammeln.

```powershell
<##########################################################################
 Step 1 : Enable analytic and debug DSC channels (Operational channel is enabled by default)
###########################################################################>
 
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
wevtutil.exe set-log “Microsoft-Windows-Dsc/Debug” /q:True /e:true
 
<##########################################################################
 Step 2 : Perform the required DSC operation (Below is an example, you could run any DSC operation instead)
###########################################################################>
 
Get-DscLocalConfigurationManager
 
<##########################################################################
Step 3 : Collect all DSC Logs, from the Analytic, Debug and Operational channels
###########################################################################>
 
$DscEvents=[System.Array](Get-WinEvent "Microsoft-windows-dsc/operational") `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Analytic" -Oldest) `
         + [System.Array](Get-Winevent "Microsoft-Windows-Dsc/Debug" -Oldest)
 
 
<##########################################################################
 Step 4 : Group all logs based on the job ID
###########################################################################>
$SeparateDscOperations=$DscEvents | Group {$_.Properties[0].value}  
```

Hier enthält die Variable `$SeparateDscOperations` nach den Auftrags-IDs gruppierte Protokolle. Jedes Arrayelement dieser Variablen stellt eine Gruppe von Ereignissen dar, die von einem anderen DSC-Vorgang protokolliert wurden, und ermöglicht den Zugriff auf weitere Informationen zu den Protokollen .

```
PS C:\> $SeparateDscOperations
 
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   48 {1A776B6A-5BAC-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
   40 {E557E999-5BA8-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
PS C:\> $SeparateDscOperations[0].Group
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                               
-----------                     -- ---------------- -------                                               
12/2/2013 3:47:29 PM          4115 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4198 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4114 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4102 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4098 Warning          Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4098 Warning          Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4176 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...       
```

Sie können die Daten in der Variablen `$SeparateDscOperations` mit [Where-Object](https://technet.microsoft.com/library/ee177028.aspx) extrahieren. In den folgenden fünf Szenarien ist es beispielsweise sinnvoll, Daten für die Problembehandlung bei DSC zu extrahieren:

### 1: Fehler bei Vorgängen

Alle Ereignisse verfügen über [Schweregrade](https://msdn.microsoft.com/library/dd996917(v=vs.85)). Diese Informationen kann verwendet werden, um die Fehlerereignisse zu identifizieren:

```
PS C:\> $SeparateDscOperations  | Where-Object {$_.Group.LevelDisplayName -contains "Error"}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   38 {5BCA8BE7-5BB6-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
```

### 2: Details zu Vorgängen, die in der letzten halben Stunde ausgeführt wurden

`TimeCreated`, eine Eigenschaft jedes Windows-Ereignisses, gibt den Zeitpunkt an, zu dem das Ereignis erstellt wurde. Durch Vergleichen dieser Eigenschaft mit einem bestimmten Datum/Uhrzeit-Objekt können Sie alle Ereignisse filtern:

```powershell
PS C:\> $DateLatest=(Get-date).AddMinutes(-30)
PS C:\> $SeparateDscOperations  | Where-Object {$_.Group.TimeCreated -gt $DateLatest}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
    1 {6CEC5B09-5BB0-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord}   
```

### 3: Meldungen zum aktuellen Vorgang

Der aktuelle Vorgang wird im ersten Index der Arraygruppe `$SeparateDscOperations` gespeichert. Durch Abfragen der Meldungen der Gruppe für den Index 0 werden alle Meldungen für den aktuellen Vorgang zurückgegeben:

```powershelll
PS C:\> $SeparateDscOperations[0].Group.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
Running consistency engine.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Configuration is sent from computer NULL by user sid S-1-5-18.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Displaying messages from built-in DSC resources:
 WMI channel 1 
 ResourceID:  
 Message : [INCH-VM]:                            [] Starting consistency engine.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Displaying messages from built-in DSC resources:
 WMI channel 1 
 ResourceID:  
 Message : [INCH-VM]:                            [] Consistency check completed. 
```

### 4: Fehlermeldungen, die für die letzten fehlgeschlagenen Vorgänge protokolliert wurden

`$SeparateDscOperations[0].Group` enthält eine Reihe von Ereignissen für den aktuellen Vorgang. Führen Sie das Cmdlet `Where-Object` aus, um die Ereignisse basierend auf dem Schweregrad/Anzeigenamen zu filtern. Die Ergebnisse werden in der Variablen `$myFailedEvent` gespeichert. Diese kann weiter zerlegt werden, um die Ereignismeldung zu erhalten:

```powershell
PS C:\> $myFailedEvent=($SeparateDscOperations[0].Group | Where-Object {$_.LevelDisplayName -eq "Error"})
 
PS C:\> $myFailedEvent.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
DSC Engine Error : 
 Error Message Current configuration does not exist. Execute Start-DscConfiguration command with -Path pa
rameter to specify a configuration file and create a current configuration first. 
Error Code : 1 
```

### 5: Alle Ereignisse, die für eine bestimmte Auftrags-ID generiert wurden.

`$SeparateDscOperations` ist ein Array von Gruppen, die jeweils den Namen als eindeutige Auftrags-ID aufweisen. Durch Ausführen des Cmdlets `Where-Object` können Sie die Gruppen von Ereignissen mit einer bestimmten Auftrags-ID extrahieren:

```powershell
PS C:\> ($SeparateDscOperations | Where-Object {$_.Name -eq $jobX} ).Group

   ProviderName: Microsoft-Windows-DSC
 
TimeCreated                     Id LevelDisplayName Message                                               
-----------                     -- ---------------- -------                                               
12/2/2013 4:33:24 PM          4102 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4168 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4146 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4120 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...  
```

## Verwenden von „xDscDiagnostics“ zum Analysieren von DSC-Protokollen

**xDscDiagnostics** ist ein PowerShell-Modul, das aus zwei einfachen Vorgängen besteht, die bei der Analyse von DSC-Fehlern auf dem Computer helfen können: `Get-xDscOperation` und `Trace-xDscOperation`. Diese Funktionen können Ihnen helfen, alle lokalen Ereignisse der letzten DSC-Vorgänge oder DSC-Ereignisse auf Remotecomputern (mit gültigen Anmeldeinformationen) zu identifizieren. Hier wird der Begriff „DSC-Vorgang“ verwendet, um eine einzelne eindeutige DSC-Ausführung von Anfang bis Ende zu definieren. `Test-DscConfiguration` wäre z. B. ein separater DSC-Vorgang. Auf ähnliche Weise kann jedes andere Cmdlet in DSC (z. B. `Get-DscConfiguration`, `Start-DscConfiguration` usw.) jeweils als separater DSC-Vorgang identifiziert werden. Die beiden Cmdlets werden im PowerShell-Modul [xDscDiagnostics](https://powershellgallery.com/packages/xDscDiagnostics)(DSC Resource Kit) beschrieben und im Folgenden ausführlicher erläutert. Hilfe ist durch Ausführen von `Get-Help <cmdlet name>` verfügbar.

## Get-xDscOperation

Mit diesem Cmdlet können Sie die Ergebnisse der DSC-Vorgänge suchen, die auf einem oder mehreren Computern ausgeführt werden. Zurückgegeben wird ein Objekt, das die Sammlung der von den einzelnen DSC-Vorgängen erzeugten Ereignisse enthält. In der folgenden Ausgabe wurden beispielsweise drei Befehle ausgeführt. Der erste wurde erfolgreich ausgeführt, bei den beiden anderen sind Fehler aufgetreten. Die Ergebnisse sind in der Ausgabe von `Get-xDscOperation` zusammengefasst.

TODO: Diese Abbildung, die die Ausgabe von Get-xDscOperation darstellt, ersetzen

### Parameter

* **Newest**: Akzeptiert einen ganzzahligen Wert für die Anzahl der anzuzeigenden Vorgänge. Standardmäßig werden die 10 neuesten Vorgänge zurückgegeben. Beispiel: TODO: „Get-xDscOperation -Newest 5“ anzeigen
* **ComputerName**: Parameter, der ein Array von Zeichenfolgen akzeptiert, die jeweils den Namen eines Computers enthalten, von dem Sie DSC-Ereignisprotokolldaten sammeln möchten. Standardmäßig werden Daten vom Host-Computer gesammelt. Um dieses Feature zu aktivieren, müssen Sie den folgenden Befehl auf den Remotecomputern im erweiterten Modus ausführen, damit sie die Sammlung von Ereignissen zulassen.
```powershell
  New-NetFirewallRule -Name "Service RemoteAdmin" -Action Allow
```
* **Credential**: Parameter vom Typ „PSCredential“, der den Zugriff auf die im Parameter „ComputerName“ angegebenen Computer ermöglichen kann.

### Zurückgegebenes Objekt

Das Cmdlet gibt ein Array von Objekten jeweils Typ **Microsoft.PowerShell.xDscDiagnostics.GroupedEvents** zurück. Jedes Objekt in diesem Array bezieht sich auf einen anderen DSC-Vorgang. Die Standardanzeige für dieses Objekt weist die folgenden Eigenschaften auf:
* **SequenceID**: Gibt die dem DSC-Vorgang basierend auf der Zeit zugewiesene inkrementelle Nummer an. Die „SequenceID“ des zuletzt ausgeführten Vorgangs wäre beispielsweise „1“, die „SequenceID“ des vorletzten DSC-Vorgangs wäre „2“ und so weiter. Diese Nummer ist, ein weiterer Bezeichner für jedes Objekt im zurückgegebenen Array.
* **TimeCreated**: Ein DateTime-Wert, der angibt, wann der DSC-Vorgang begonnen hat.
* **ComputerName**: Der Name des Computers, von dem die Ergebnisse aggregiert werden.
* **Result**: Eine Zeichenfolge mit dem Wert **Fehler** oder **Erfolg** die angibt, ob bei dem DSC-Vorgang ein Fehler aufgetreten ist oder nicht.
* **AllEvents**: Ein Objekt, das eine Sammlung der vom DSC-Vorgang erzeugten Ereignisse darstellt.

Zum Beispiel die folgende Ausgabe zeigt die Ergebnisse des letzten Vorgangs von mehreren Computern: TODO: Bild ersetzen für Get-xDscOperation remotecomputerprotokolle anzeigen

## Trace-xDscOperation

Dieses Cmdlet gibt ein Objekt zurück, das eine Sammlung von Ereignissen, deren Ereignistypen und die von einem bestimmten DSC-Vorgang generierte Meldungsausgabe enthält. Wenn Sie mit `Get-xDscOperation` in einem der Vorgänge einen Fehler finden, würden Sie diesen Vorgang normalerweise verfolgen, um herauszufinden, welches Ereignis den Fehler verursacht hat.

### Parameter

* **SequenceID**: Dies ist der einem Vorgang zugewiesene ganzzahlige Wert, der sich auf einen bestimmten Computer bezieht. Indem Sie eine Sequenz-ID angeben, z. B. 4, wird der Trace für den viertletzten DSC-Vorgang ausgegeben.

Trace-xDscOperation mit angegebener Sequenz-ID
* **JobID**: Dies ist der GUID-Wert, der von LCM xDscOperation zugewiesen wurde, um einen Vorgang eindeutig zu identifizieren. Wird ein „JobID“-Wert angegeben, wird der Trace des entsprechenden DSC-Vorgangs ausgegeben.
  TODO: Abbildung für Trace-xDscOperation mit JobID als Parameter ersetzen
* **ComputerName** und **Credential**: Mit diesen Parametern kann der Trace von Remotecomputern gesammelt werden:
```powershell
New-NetFirewallRule -Name "Service RemoteAdmin" -Action Allow
```
  TODO: Abbildung für Trace-xDscOperation (auf einem anderen Computer ausgeführt) ersetzen

Beachten Sie Folgendes: Da `Trace-xDscOperation` Ereignisse aus den analytischen Protokollen, den Debugprotokollen und den Betriebsprotokollen aggregiert, werden Sie aufgefordert, diese Protokolle wie oben beschrieben zu aktivieren.

### Zurückgegebenes Objekt

Das Cmdlet gibt ein Array von Objekte vom Typ `Microsoft.PowerShell.xDscDiagnostics.TraceOutput` zurück. Jedes Objekt im Array enthält die folgenden Felder:
* **ComputerName**: Der Name des Computers, von dem die Protokolle gesammelt werden.
* **EventType**: Dies ist ein Feld vom Typ Enumerator, das Informationen über Ereignistyp enthält. Dabei kann es sich um folgende Werte handeln:
  - *Operational*: Das Ereignis stammt aus dem Betriebsprotokoll.
  - *Analytic*: Das Ereignis stammt aus dem analytischen Protokoll.
  - *Debug*: Das Ereignis stammt aus dem Debugprotokoll.
  - *Verbose*: Ereignisse, die während der Ausführung als ausführliche Meldungen ausgegeben werden. Ausführliche Meldungen erleichtern die Identifizierung der Abfolge von Ereignissen, die veröffentlicht werden.
  - *Error*: Fehlerereignisse. Anhand der Fehlerereignisse können Sie in der Regel schnell die Ursache eines Fehlers finden.
* **TimeCreated**: Ein DateTime-Wert, der angibt, wann das Ereignis von DSC protokolliert wurde.
* **Message**: Die Meldung, die von DSC in die Ereignisprotokolle geschrieben wurde.

Die folgenden Felder im Objekt können für weitere Informationen zum Ereignis enthalten, werden aber nicht standardmäßig angezeigt:

* **JobID**: Die für diesen DSC-Vorgang spezifische Auftrags-ID (GUID-Format).
* **SequenceID**: Die für diesen DSC-Vorgang auf diesem Computer eindeutige Sequenz-ID.
* **Event**: Dies ist das eigentliche, von DSC protokollierte Ereignis vom Typ `System.Diagnostics.Eventing.Reader.EventLogRecord`. Es kann auch durch Ausführen des Cmdlets `Get-WinEvent` abgerufen werden. Es enthält weitere Informationen, wie z. B. Task, Ereignis-ID und Ereignisstufe.

Informationen zu den Ereignissen können Sie alternativ auch sammeln, indem Sie die Ausgabe von `Trace-xDscOperation` in einer Variablen speichern. Verwenden Sie den folgenden Befehl, um alle Ereignisse für einen bestimmten DSC-Vorgang anzuzeigen:

```powershell
(Trace-xDscOperation -SequenceID 3).Event
```

Dadurch wird angezeigt, die gleichen Ergebnisse wie die `Get-WinEvent` -Cmdlet, wie die folgende Ausgabe: TODO: welche Ausgabe?

Im Idealfall würden Sie zuerst `Get-xDscOperations` verwenden, um die letzten DSC-Konfigurationsausführungen auf Ihren Computern aufzulisten. Im Anschluss können Sie jeden einzelnen Vorgang (anhand der Sequenz-ID oder der Auftrags-ID) mit `Trace-xDscOperation` untersuchen, um zu ermitteln, was im Hintergrund geschehen ist.

## Meine Ressourcen werden nicht aktualisiert: Zurücksetzen des Caches

Das DSC-Modul speichert Ressourcen zwischen, die aus Effizienzgründen als PowerShell-Modul implementiert wurden. Dies kann jedoch Probleme verursachen, wenn Sie eine Ressource erstellen und gleichzeitig testen, da DSC die zwischengespeicherte Version lädt, solange der Vorgang nicht neu gestartet wurde. Die einzige Möglichkeit, DSC zu veranlassen, die neuere Version zu laden, besteht darin, den Prozess, der das DSC-Modul hostet, explizit zu beenden.

Ähnliches gilt, wenn Sie `Start-DSCConfiguration` nach dem Hinzufügen und Ändern einer benutzerdefinierten Ressource ausführen. Die Änderung kann dann möglicherweise nicht ausgeführt werden, bis der Computer neu gestartet wird. Grund hierfür ist, dass DSC im WMI-Anbieterhostprozess (WmiPrvSE) ausgeführt wird, und in der Regel mehrere Instanzen von WmiPrvSE gleichzeitig ausgeführt werden. Beim Neustart wird der Hostprozess neu gestartet und der Cache geleert.

Um die Konfiguration erfolgreich zu recyceln und den Cache zu löschen, ohne einen Neustart auszuführen, müssen Sie den Hostprozess beenden und neu starten. Dazu können Sie den Prozess auf Instanzebene identifizieren, beenden und neu starten. Sie können auch `DebugMode` verwenden, wie nachfolgend gezeigt, um die PowerShell DSC-Ressource erneut zu laden.

Um zu identifizieren, welcher Prozess das DSC-Modul hostet, und diesen auf Instanzebene zu beenden, können Sie die Prozess-ID des WmiPrvSE-Prozesses auflisten, der das DSC-Modul hostet. Beenden Sie den WmiPrvSE-Prozess dann mithilfe der unten aufgeführten Befehle, um den Anbieter zu aktualisieren, und führen Sie anschließend **Start-DscConfiguration** erneut aus.

```powershell
###
### find the process that is hosting the DSC engine
###
$dscProcessID = Get-WmiObject msft_providers | 
Where-Object {$_.provider -like 'dsccore'} | 
Select-Object -ExpandProperty HostProcessIdentifier 

###
### Stop the process
###
Get-Process -Id $dscProcessID | Stop-Process
```

## Verwenden von DebugMode

Sie können den lokalen Konfigurations-Manager (LCM) von DSC für die Verwendung von `DebugMode` konfigurieren, damit der Cache bei jedem Neustart des Hostprozesses neu gestartet wird. Indem Sie den Debugmodus verwenden (auf **TRUE** festlegen), lädt das Modul die PowerShell DSC-Ressource immer neu. Sobald Sie mit dem Schreiben Ihrer Ressource fertig sind, können Sie den Debugmodus wieder deaktivieren (auf **FALSE** festlegen), um zum alten Verhalten zurückzukehren und die Module wieder zwischenzuspeichern.

In der folgenden Demonstration wird verdeutlicht, wie `DebugMode` den Cache automatisch aktualisieren kann. Betrachten wir zunächst die Standardkonfiguration:

```
PS C:\Users\WinVMAdmin\Desktop> Get-DscLocalConfigurationManager
 
 
AllowModuleOverwrite           : False
CertificateID                  : 
ConfigurationID                : 
ConfigurationMode              : ApplyAndMonitor
ConfigurationModeFrequencyMins : 30
Credential                     : 
DebugMode                      : False
DownloadManagerCustomData      : 
DownloadManagerName            : 
LocalConfigurationManagerState : Ready
RebootNodeIfNeeded             : False
RefreshFrequencyMins           : 15
RefreshMode                    : PUSH
PSComputerName                 :  
```

Sie sehen, dass `DebugMode` auf **FALSE** festgelegt ist.

Zum Einrichten der `DebugMode`-Demo verwenden Sie die folgenden PowerShell-Ressource:

```powershell
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return @{onlyProperty = Get-Content -path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    "1"|Out-File -PSPath "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"
}
function Test-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return $false
} 
```

Nun erstellen Sie eine Konfiguration mit der oben stehenden Ressource namens `TestProviderDebugMode`:

```powershell
Configuration ConfigTestDebugMode
{
    Import-DscResource -Name TestProviderDebugMode
    Node localhost
    {
        TestProviderDebugMode test
        {
            onlyProperty = "blah"
        }
    }
}
ConfigTestDebugMode
```

Sie erkennen, dass der Inhalt der Datei „**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**“ **1** ist.

Aktualisieren Sie nun den Anbietercode mithilfe des folgenden Skripts:

```powershell
$newResourceOutput = Get-Random -Minimum 5 -Maximum 30
$content = @"
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    return @{onlyProperty = Get-Content -path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    "$newResourceOutput"|Out-File -PSPath C:\OutputFromTestProviderDebugMode.txt
}
function Test-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    return `$false
}
"@ | Out-File -FilePath "C:\Program Files\WindowsPowerShell\Modules\MyPowerShellModules\DSCResources\TestProviderDebugMode\TestProviderDebugMode.psm1
```

Dieses Skript generiert eine Zufallszahl und aktualisiert den Anbietercode entsprechend. Ist `DebugMode` auf „false“ festgelegt, wird der Inhalt der Datei „**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**“ nie geändert.

Legen Sie `DebugMode` in Ihrem Konfigurationsskript jetzt auf **TRUE** fest:

```powershell
LocalConfigurationManager
{
    DebugMode = $true
} 
```

Wenn Sie das oben stehende Skript erneut ausführen, sehen Sie, dass der Inhalt der Datei jedes Mal anders ist. (Zum Überprüfen können Sie `Get-DscConfiguration` ausführen). Im Folgenden wird das Ergebnis von zwei weiteren Ausführungen angezeigt (Ihre Ergebnisse können sich nach Ausführen des Skripts von diesem Ergebnis unterscheiden):

```powershell
PS C:\Users\WinVMAdmin\Desktop> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
20                                      localhost                              
 
PS C:\Users\WinVMAdmin\Desktop> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
14 
```

## Weitere Informationen

### Verweis
* [DSC-Ressource „Log“](logResource.md)

### Konzepte
* [Erstellen von benutzerdefinierten Windows PowerShell DSC-Ressourcen](authoringResource.md)

### Weitere Ressourcen
* [Windows PowerShell DSC-Cmdlets](https://technet.microsoft.com/en-us/library/dn521624(v=wps.630).aspx)


<!--HONumber=Jun16_HO1-->


