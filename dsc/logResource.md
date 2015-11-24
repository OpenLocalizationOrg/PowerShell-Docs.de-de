#DSC-Protokoll-Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die __Protokoll__ Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus, um Nachrichten in der Microsoft-Windows-Desired State Configuration/analytische Ereignisprotokoll zu schreiben.

```
Syntax

Log [string] #ResourceName
{
    Message = [string]
    [ DependsOn = [string[]] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Nachricht| Gibt die Meldung im Ereignisprotokoll Microsoft-Windows-Desired Zustand Configuration/analytische schreiben möchten.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor Sie dieses Protokoll-Nachricht ruft.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Das folgende Beispiel zeigt, wie eine Meldung im Ereignisprotokoll Microsoft-Windows-Desired Zustand Configuration/analytische eingeschlossen wird.

> **Hinweis**: Wenn das Ausführen [Test DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx) mit dieser Ressource konfiguriert ist, wird immer zurückgegeben **$false**.

```powershell 
Log LogExample
{
    Message = "This message will appear in the Microsoft-Windows-Desired State Configuration/Analytic event log."
} 
```





