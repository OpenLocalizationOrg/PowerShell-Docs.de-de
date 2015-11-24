#DSC User-Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0


Die __Benutzer__ Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Verwalten von lokalen Benutzerkonten auf dem Zielknoten.


##Syntax

```
User [string] #ResourceName
{
    UserName = [string]
    [ Description = [string] ]
    [ Disabled = [bool] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ FullName = [string] ]
    [ Password = [PSCredential] ]
    [ PasswordChangeNotAllowed = [bool] ]
    [ PasswordChangeRequired = [bool] ]
    [ PasswordNeverExpires = [bool] ]
    [ DependsOn = [string[]] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| UserName| Gibt den Kontonamen, der einen bestimmten Zustand sichergestellt werden soll.|
| Beschreibung| Gibt die Beschreibung, die Sie für das Benutzerkonto verwenden möchten.|
| Deaktiviert| Gibt an, ob das Konto aktiviert ist.Legen Sie diese Eigenschaft auf __$true__ um sicherzustellen, dass dieses Konto deaktiviert ist, und legen Sie es auf __$false__ um sicherzustellen, dass diese Option aktiviert ist.|
| Stellen Sie sicher| Gibt an, ob das Konto vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass das Konto vorhanden ist, und legen Sie ihn auf "Abwesend", um sicherzustellen, dass das Konto nicht vorhanden ist.|
| "FullName"| Stellt eine Zeichenfolge mit dem vollständigen Namen, die, den Sie für das Benutzerkonto verwenden möchten.|
| Kennwort| Gibt das Kennwort für dieses Konto verwenden möchten.|
| PasswordChangeNotAllowed| Gibt an, ob der Benutzer das Kennwort ändern kann.Legen Sie diese Eigenschaft auf __$true__ um sicherzustellen, dass der Benutzer das Kennwort ändern nicht, und legen Sie ihn auf __$false__ ermöglicht es den Benutzer, das Kennwort zu ändern.Der Standardwert ist __$false__.|
| PasswordChangeRequired| Gibt an, ob der Benutzer das Kennwort bei der nächsten Anmeldung ändern muss.Legen Sie diese Eigenschaft auf __$true__ wenn der Benutzer das Kennwort ändern muss.Der Standardwert ist __$true__.|
| PasswordNeverExpires| Gibt an, ob das Kennwort abläuft.Um sicherzustellen, dass das Kennwort für dieses Konto nie abläuft, wird diese Eigenschaft festgelegt __$true__, und legen Sie es auf __$false__ wenn das Kennwort abläuft.Der Standardwert ist __$false__.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst __ResourceName__ und der Typ ist __ResourceType__, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

```powershell
User UserExample
{
    Ensure = "Present"  # To ensure the user account does not exist, set Ensure to "Absent"
    UserName = "SomeName"
    Password = $passwordCred # This needs to be a credential object
    DependsOn = “[Group]GroupExample" # Configures GroupExample first
}
```




