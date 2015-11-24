#DSC-Dienstressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0


Die **Service** Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von Diensten auf dem Zielknoten.

##Syntax

```
Service [string] #ResourceName
{
    Name = [string]
    [ BuiltInAccount = [string] { LocalService | LocalSystem | NetworkService }  ]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
    [ StartupType = [string] { Automatic | Disabled | Manual }  ]
    [ State = [string] { Running | Stopped }  ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Gibt den Dienstnamen an.Beachten Sie, dass manchmal dies anders als der angezeigte Name ist.Sie erhalten eine Liste der Dienste und deren aktuellem Status mit dem Cmdlet Get-Service.|
| BuiltInAccount| Gibt das Konto für den Dienst verwenden.Die Werte für diese Eigenschaft zulässig sind: **LocalService**, **"LocalSystem"**, und **NetworkService**.|
| Credential| Gibt die Anmeldeinformationen für das Konto, dem der Dienst, klicken Sie unter ausgeführt wird.Diese Eigenschaft und die __BuiltinAccount__ Eigenschaft kann nicht zusammen verwendet werden.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|
| Des Starttyps für den| Gibt den Starttyp für den Dienst an.Die Werte für diese Eigenschaft zulässig sind: **automatische**, **deaktiviert**, und **manuell**|
| Status| Gibt den Status für den Dienst sicherstellen möchten.|

##Beispiel

```powershell
Service ServiceExample
{
    Name = "TermService"
    StartupType = "Manual"
    State = "Running"
} 
```




