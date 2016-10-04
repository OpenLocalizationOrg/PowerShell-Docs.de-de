# Vereinheitlichung und Konsistenz von Zustands- und Statusdarstellung

Für diese Version sind eine Reihe von Verbesserungen beim automatisch erstellten LCM-Zustand und DSC-Status erfolgt. Dazu gehören vereinheitlichte und konsistente Zustands- und Statusdarstellungen, verwaltbare „datetime“-Eigenschaft von Statusobjekten, die vom Cmdlet „Get-DscConfigurationStatus“ zurückgegeben werden, und eine verbesserte LCM-Statusdetaileigenschaft, die vom Cmdlet „Get-DscLocalConfigurationManager“ zurückgegeben wird.

Die Darstellung des LCM-Zustands und DSC-Betriebsstatus wird gemäß den folgenden Regeln überprüft und vereinheitlicht:
1.  Die Ressource „NotProcessed“ wirkt sich nicht auf den LCM-Zustand und DSC-Status aus.
2.  Der LCM beendet die Verarbeitung weiterer Ressourcen, sobald er auf eine Ressource trifft, die einen Neustart anfordert.
3.  Eine Ressource, die einen Neustart anfordert, ist erst im gewünschten Zustand, nach der Neustart tatsächlich erfolgt ist.
4.  Wenn der LCM auf eine fehlerhafte Ressource trifft, setzt er die Verarbeitung weiterer Ressourcen fort, sofern diese nicht von der fehlerhaften Ressource abhängig sind.
5.  Der vom Cmdlet „Get-DscConfigurationStatus“ zurückgegebene Gesamtstatus ist die Obermenge der Status aller Ressourcen.
6.  Der Status „PendingReboot“ ist eine Obermenge des Status „PendingConfiguration“.

Die nachfolgende Tabelle veranschaulicht die resultierenden auf Zustand und Status bezogenen Eigenschaften in einigen gängigen Szenarien.

| **Szenario**                    | **LCMState\ ***       | **Status** | **Ein Neustart angefordert**  | **ResourcesInDesiredState**  | **ResourcesNotInDesiredState** |
|---------------------------------|----------------------|------------|---------------|------------------------------|--------------------------------|
| S**^**                          | Idle                 | Erfolg    | $false        | E                            | $null                          |
| F**^**                          | PendingConfiguration | Fehler    | $false        | $null                        | F                              |
| E,F                             | PendingConfiguration | Fehler    | $false        | E                            | F                              |
| E, S                             | PendingConfiguration | Fehler    | $false        | E                            | F                              |
| E<sub>1</sub>, F, E<sub>2</sub> | PendingConfiguration | Fehler    | $false        | E<sub>1</sub>, E<sub>2</sub> | F                              |
| F<sub>1</sub>, E, F<sub>2</sub> | PendingConfiguration | Fehler    | $false        | E                            | F<sub>1</sub>, F<sub>2</sub>   |
| E, N                            | PendingReboot        | Erfolg    | $True         | E                            | N                              |
| F, N                            | PendingReboot        | Fehler    | $True         | $null                        | F, N                           |
| R, E                            | PendingReboot        | Erfolg    | $True         | $null                        | r                              |
| N, F                            | PendingReboot        | Erfolg    | $True         | $null                        | r                              |

^
S<sub>ich</sub>: eine Reihe von Ressourcen, die erfolgreich F angewendet<sub>ich</sub>: eine Reihe von Ressourcen, die nicht erfolgreich r: eine Ressource angewendet werden, die neu gestartet werden muss \*

```powershell
$LCMState = (Get-DscLocalConfigurationManager).LCMState
$Status = (Get-DscConfigurationStatus).Status

$RebootRequested = (Get-DscConfigurationStatus).RebootRequested

$ResourcesInDesiredState = (Get-DscConfigurationStatus).ResourcesInDesiredState

$ResourcesNotInDesiredState = (Get-DscConfigurationStatus).ResourcesNotInDesiredState
```
## Verbesserungen beim Cmdlet „Get-DscConfigurationStatus“

Für diese Version wurden einige Verbesserungen am Cmdlet „Get-DscConfigurationStatus“ vorgenommen. Zuvor hatte die „StartDate“-Eigenschaft von Objekten, die vom Cmdlet zurückgegeben wurden, den Typ „String“. Jetzt hat sie den Typ „Datetime“, der komplexe Auswahl- und Filtervorgänge basierend auf den inhärenten Eigenschaften eines „Datetime“-Objekts erleichtert.
```powershell
(Get-DscConfigurationStatus).StartDate | fl \*
DateTime : Friday, November 13, 2015 1:39:44 PM
Date : 11/13/2015 12:00:00 AM
Day : 13
DayOfWeek : Friday
DayOfYear : 317
Hour : 13
Kind : Local
Millisecond : 886
Minute : 39
Month : 11
Second : 44
Ticks : 635830187848860000
TimeOfDay : 13:39:44.8860000
Year : 2015
```

Es folgt ein Beispiel, das alle Datensätze von DSC-Vorgängen zurückgibt, die am selben Tag der Woche wie heute erfolgt sind.
```powershell
(Get-DscConfigurationStatus –All) | where { $\_.startdate.dayofweek -eq (Get-Date).DayOfWeek }
```

Datensätze von Vorgängen, die keine Änderungen an der Konfiguration des Knotens vornehmen (z. B. Lesevorgänge), werden entfernt. Deshalb werden die Vorgänge „Test-DscConfiguration“ und „Get-DscConfiguration“ nicht mehr mit Objekten verfälscht, die vom Cmdlet „Get-DscConfigurationStatus“ zurückgegeben werden.
Der Rückgabe des Cmdlets „Get-DscConfigurationStatus“ werden Datensätze zum Vorgang der Einstellung der Metakonfiguration hinzugefügt.

Es folgt ein Beispiel der Rückgabe des Cmdlets „Get-DscConfigurationStatus –All“.
```powershell
All configuration operations:

Status StartDate Type RebootRequested
------ --------- ---- ---------------
Success 11/13/2015 11:38:16 AM Consistency False
Success 11/13/2015 11:23:16 AM Reboot False
Success 11/13/2015 11:21:43 AM Reboot True
Success 11/13/2015 11:20:44 AM Initial True
Success 11/13/2015 11:20:44 AM LocalConfigurationManager False
```

## Verbesserungen beim Cmdlet „Get-DscLocalConfigurationManager“
Das neue Feld „LCMStateDetail“ wird dem Objekt hinzugefügt, das vom Cmdlet „Get-DscLocalConfigurationManager“ zurückgegeben wird. Dieses Feld wird aufgefüllt, wenn „LCMState = Busy". Es kann mit dem folgenden Cmdlet abgerufen werden:
```powershell
(Get-DscLocalConfigurationManager).LCMStateDetail
```

Es folgt eine Beispielausgabe einer kontinuierlichen Überwachung einer Konfiguration, die zwei Neustarts auf einem Remoteknoten erfordert.
```powershell
Start a configuration that requires two reboots

Monitor LCM State:
LCM State: Busy, LCM is applying a new configuration.
LCM State: PendingReboot,
Machine is rebooting...
LCM State: Busy, LCM is continuing applying configuration after last reboot.
LCM State: PendingReboot,
Machine is rebooting...
LCM State: Busy, LCM is continuing applying configuration after last reboot.
LCM State: Idle,
LCM State: Busy, LCM is performing a consistency check.
LCM State: Idle,
```


<!--HONumber=Oct16_HO1-->


