#DSC Registrierung Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die **Registrierung** Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von Registrierungsschlüsseln und-Werten auf dem Zielknoten.

##Syntax

```
Registry [string] #ResourceName
{
    Key = [string]
    ValueName = [string]
    [ Ensure = [string] { Absent | Present }  ]
    [ Force =  [bool]   ]
    [ Hex = [bool] ]
    [ DependsOn = [string[]] ]
    [ ValueData = [string[]] ]
    [ ValueType = [string] { Binary | Dword | ExpandString | MultiString | Qword | String }  ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Schlüssel| Gibt den Pfad des Registrierungsschlüssels, der einen bestimmten Zustand sichergestellt werden soll.Dieser Pfad muss die Struktur enthalten.|
| Wertname| Gibt den Namen des Registrierungswerts.|
| Stellen Sie sicher| Gibt an, ob es sich bei dem Schlüssel und Wert vorhanden ist.Um sicherzustellen, dass dies der Fall ist, legen Sie diese Eigenschaft auf "Present".Um sicherzustellen, dass sie nicht vorhanden sind, legen Sie die Eigenschaft auf "Abwesend".Der Standardwert ist "Present".|
| Force| Wenn der angegebene Registrierungsschlüssel vorhanden ist __Kraft__ mit dem neuen Wert überschrieben.|
| Hex| Gibt an, ob Daten im Hexadezimalformat angegeben werden.Wenn angegeben, werden die Daten des DWORD/QWORD-Wert im Hexadezimalformat angezeigt.Für andere Typen nicht gültig.Der Standardwert ist __$false__.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| ValueData| Die Daten für den Registrierungswert.|
| ValueType| Gibt den Typ des Werts.Die unterstützten Typen sind:

<ul>
            <li>Zeichenfolge (REG_SZ)</li>
<li>Binär (REG-BINARY)</li>
<li>DWORD 32-Bit (REG_DWORD)</li>
<li>QWORD 64-Bit (REG_QWORD)</li>
<li>Mehrteilige Zeichenfolge (REG_mit mehreren_SZ)</li>
<li>Erweiterbare Zeichenfolge (REG_EXPAND_SZ)</li></ul>

##Beispiel

```powershell
Registry RegistryExample
{
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Key = "HKEY_LOCAL_MACHINE\SOFTWARE\ExampleKey"
    ValueName = "TestValue"
    ValueData = "TestData"
}
```





