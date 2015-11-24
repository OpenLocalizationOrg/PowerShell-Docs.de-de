#DSC Skriptressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die **Skript** Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Ausführen von Windows PowerShell-Skriptblöcken auf Zielknoten.

##Syntax

```
Script [string] #ResourceName
{
    GetScript = [string]
    SetScript = [string]
    TestScript = [string]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| GetScript| Stellt einen Block mit Windows PowerShell-Skript, das ausgeführt, beim Aufrufen wird der [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407379.aspx) Cmdlet.Dieser Block muss eine Hash-Tabelle zurückgeben.|
| SetScript| Stellt einen Block von Windows PowerShell-Skript.Beim Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) -Cmdlet der **TestScript** Block wird zuerst ausgeführt.Wenn die **TestScript** blockieren gibt **$false**,  **SetScript** Block ausgeführt wird.Wenn die **TestScript** blockieren gibt **$true**,  **SetScript** Block kann nicht ausgeführt werden.|
| TestScript| Stellt einen Block von Windows PowerShell-Skript.Beim Aufrufen der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) -Cmdlet wird dieser Block ausgeführt.Wenn zurückgegeben **$false**, der SetScript-Block ausgeführt wird.Wenn zurückgegeben **$true**, die SetScript Block wird nicht ausgeführt.Die **TestScript** Block ebenfalls ausgeführt wird, beim Aufrufen der [Test DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx) Cmdlet.In diesem Fall die **SetScript** Block wird nicht ausgeführt, blockieren Sie unabhängig davon, was den TestScript Wert zurück.Die **TestScript** Block muss gibt True zurück, wenn die tatsächliche Konfiguration entspricht der aktuelle der gewünschten Statuskonfiguration, und False, wenn er nicht übereinstimmt.(Die aktuelle Konfiguration für den gewünschten Status ist die letzte Konfiguration in Kraft gesetzt, auf den Knoten, der DSC verwendet.)|
| Credential| Gibt die Anmeldeinformationen zum Ausführen dieses Skripts verwenden, wenn Anmeldeinformationen erforderlich sind.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.

##Beispiel

```powershell
Script ScriptExample
{
    SetScript = { 
        $sw = New-Object System.IO.StreamWriter("C:\TempFolder\TestFile.txt")
        $sw.WriteLine("Some sample string")
        $sw.Close()
    }
    TestScript = { Test-Path "C:\TempFolder\TestFile.txt" }
    GetScript = { <# This must return a hash table #> }          
}
```





