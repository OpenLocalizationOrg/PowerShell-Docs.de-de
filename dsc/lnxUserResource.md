#DSC für Linux NxUser Ressource

Die **NxUser** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus, um lokale Benutzer auf einem Linux-Knoten zu verwalten.

##Syntax

```
nxUser <string> #ResourceName
{
    UserName = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ FullName = <string> ]
    [ Description = <string> ]
    [ Password = <string> ]
    [ Disabled = <bool> ]
    [ PasswordChangeRequired = <bool> ]
    [ HomeDirectory = <string> ]
    [ Mode = <string> ]
    [ GroupID = <string> ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Gibt den Kontonamen, der einen bestimmten Zustand sichergestellt werden soll.|
|---|---|
| UserName| Gibt den Speicherort an, um sicherzustellen, dass den Status für eine Datei oder ein Verzeichnis werden soll.|
| Stellen Sie sicher| Gibt an, ob das Konto vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass das Konto vorhanden ist, und legen Sie ihn auf "Abwesend", um sicherzustellen, dass das Konto nicht vorhanden ist.|
| "FullName"| Eine Zeichenfolge, die den vollständigen Namen für das Benutzerkonto zu verwenden.|
| Beschreibung| Die Beschreibung für das Benutzerkonto ein.|
| Kennwort| Der Hash des Benutzerkennworts im entsprechenden Format für die Linux-Computer.Dies ist normalerweise eine Salt erstellten SHA-256 oder Hash SHA-512.Debian und Ubuntu Linux kann dieser Wert mit dem Befehl Mkpasswd generiert werden.Für andere Linux-Distributionen kann die Methode Crypt Python Crypt Bibliothek zum Generieren des Hash verwendet werden.|
| Deaktiviert| Gibt an, ob das Konto aktiviert ist.Legen Sie diese Eigenschaft auf **$true** um sicherzustellen, dass dieses Konto deaktiviert ist, und legen Sie es auf **$false** um sicherzustellen, dass diese Option aktiviert ist.|
| PasswordChangeRequired| Gibt an, ob der Benutzer das Kennwort ändern kann.Legen Sie diese Eigenschaft auf **$true** um sicherzustellen, dass der Benutzer das Kennwort ändern nicht, und legen Sie ihn auf **$false** ermöglicht es den Benutzer, das Kennwort zu ändern.Der Standardwert ist **$false**.Diese Eigenschaft wird nur ausgewertet, wenn das Benutzerkonto nicht zuvor vorhanden war und erstellt wird.|
| HomeDirectory| Das Basisverzeichnis des Benutzers.|
| Gruppen-ID| Die primäre Gruppen-ID für den Benutzer.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Beispielsweise ist die ID des Ressource-Skriptblock Konfiguration, die Sie ausführen möchten, zuerst "Ressourcenname", und der Typ ist "ResourceType", die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Beispiel

Im folgende Beispiel wird sichergestellt, dass der Benutzer "Monuser" vorhanden ist und ein der Gruppe "DBusers Mitglied".

```
Import-DSCResource -Module nx 

Node $node {
nxUser UserExample{
   UserName = "monuser"
   Description = "Monitoring user"
   Password  =    '$6$fZAne/Qc$MZejMrOxDK0ogv9SLiBP5J5qZFBvXLnDu8HY1Oy7ycX.Y3C7mGPUfeQy3A82ev3zIabhDQnj2ayeuGn02CqE/0'
   Ensure = "Present"
   HomeDirectory = "/home/monuser"
}

nxGroup GroupExample{
   GroupName = "DBusers"
   Ensure = "Present"
   MembersToInclude = "monuser"
   DependsOn = "[nxUser]UserExample"            
}
}
```




