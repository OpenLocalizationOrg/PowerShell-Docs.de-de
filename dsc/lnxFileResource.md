#DSC für Linux NxFile Ressource

Die **NxFile** Ressource in PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus, um Dateien und Verzeichnisse auf einem Linux-Knoten zu verwalten.

##Syntax

```
nxFile <string> #ResourceName
{
    DestinationPath = <string>
    [ SourcePath = <string> ]
    [ Ensure = <string> { Absent | Present }  ]
    [ Type = <string> { directory | file | link } ]
    [ Contents = <string> ]
    [ Checksum = <string> { ctime | mtime | md5 }  ]
    [ Recurse = <bool> ]
    [ Force = <bool> ]
    [ Links = <string> { follow | manage } ]
    [ Group = <string> ]
    [ Mode = <string> ]
    [ Owner = <string> ]
    [ DependsOn = <string[]> ]

}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Zielpfad| Gibt den Speicherort an, um sicherzustellen, dass den Status für eine Datei oder ein Verzeichnis werden soll.|
| Quellpfad| Gibt den Pfad, aus dem die Ressource Datei oder einen Ordner kopiert.Dieser Pfad ist möglicherweise einen lokalen Pfad oder ein `http/Https/FTP-` URL.Remote `http/Https/FTP-` URLs sind nur unterstützt, wenn der Wert der **Typ** Eigenschaft ist die Datei.|
| Stellen Sie sicher| Bestimmt, ob zum Überprüfen, ob die Datei vorhanden ist.Legen Sie diese Eigenschaft auf "Present", um sicherzustellen, dass die Datei vorhanden ist.Bei Einstellung auf "Abwesend", um sicherzustellen, dass die Datei nicht vorhanden ist.Der Standardwert ist "Present".|
| Typ| Gibt an, ob die Ressource konfiguriert wird, ein Verzeichnis oder eine Datei ist.Legen Sie diese Eigenschaft auf "Verzeichnis", um anzugeben, dass die Ressource ein Verzeichnis ist.Legen sie "file", um anzugeben, dass es sich bei der Ressource um eine Datei handelt.Der Standardwert ist "file"|
| Inhalt| Gibt den Inhalt einer Datei, z. B. eine bestimmte Zeichenfolge.|
| Prüfsumme| Definiert den Typ zu verwenden, wenn Sie bestimmen, ob zwei Dateien identisch sind.Wenn **Checksum** nicht angegeben ist, werden nur die Datei- oder Verzeichnisnamens für den Vergleich verwendet.Werte sind: "Ctime", "Mtime", oder "md5".|
| Rekursion| Gibt an, ob Unterverzeichnisse enthalten sind.Legen Sie diese Eigenschaft auf **$true** um anzugeben, dass die Unterverzeichnisse enthalten sein soll.Der Standardwert ist **$false**.**Hinweis:** dieser Eigenschaft ist nur gültig, wenn die **Typ** -Eigenschaft auf das Verzeichnis festgelegt ist.|
| Force| Bestimmte Dateioperationen (z. B. eine Datei überschreiben oder Löschen eines Verzeichnisses, das nicht leer ist), führt zu einem Fehler.Mithilfe der **Kraft** Eigenschaft überschreibt diese Fehler.Der Standardwert ist **$false**.|
| Links| Gibt das gewünschte Verhalten für symbolische Verknüpfungen.Legen Sie diese Eigenschaft auf "folgen" symbolische Verknüpfungen und fungiert für das Ziel des Links (z. b.Kopieren Sie die Datei anstelle des).Legen Sie diese Eigenschaft auf "verwalten" auf den Link fungieren, (z. b.Kopieren Sie den Link selbst).Legen Sie diese Eigenschaft auf "ignorieren", um die symbolische Verknüpfungen zu ignorieren.|
| Gruppe| Der Name des der **Gruppe** für die Datei oder das Verzeichnis zu ändern.|
| Modus| Gibt die gewünschten Berechtigungen für die Ressource im Oktal- oder symbolische Notation an.(z. B. 777 oder Rwxrwxrwx).Bei Verwendung von symbolischen Notation bieten Sie keine das erste Zeichen, das Verzeichnis oder eine Datei angibt.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Z. B. wenn die **ID** der Ressource ist Configuration-Skriptblock, der ausgeführt werden soll zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist `DependsOn = "[ResourceType] ResourceName"`.|

##Weitere Informationen

Linux und Windows verschiedene Zeilenumbruchzeichen in Textdateien wird standardmäßig verwendet, und dies kann zu unerwarteten Ergebnissen führen, wenn einige Dateien auf einem Linux-Computer mit konfigurieren __NxFile__. Es gibt mehrere Möglichkeiten, den Inhalt einer Datei Linux zu verwalten, während durch unerwartete Zeilenumbruchzeichen verursacht Probleme vermieden werden:

Schritt 1: Kopieren Sie die Datei aus einer Remotequelle (http, Https oder ftp): eine Datei auf Linux mit dem gewünschten Inhalt zu erstellen und Bereitstellen er die Knoten, Sie konfigurieren, auf einem Web- oder FTP-Server zugegriffen werden kann. Definieren der __SourcePath__ -Eigenschaft in der __NxFile__ Ressource mit der Website oder FTP-URL für die Datei.

```
Import-DSCResource -Module nx
Node $Node
{
nxFile resolvConf
{
    SourcePath = "http://10.185.85.11/conf/resolv.conf"
    DestinationPath = "/etc/resolv.conf"
    Mode = "644"        
    Type = "file"

}

}
```


Schritt 2: Lesen Sie den Inhalt der Datei in der PowerShell-Skript mit [Get-Content](https://technet.microsoft.com/en-us/library/hh849787.aspx) nach dem Festlegen der __$OFS__ Eigenschaft, die das Linux Zeilenumbruch Zeichen verwendet.


```
Import-DSCResource -Module nx
Node $Node
{
$OFS = "`n"
$Contents = Get-Content C:\temp\resolv.conf

nxFile resolvConf
{
    DestinationPath = "/etc/resolv.conf"
    Mode = "644"        
    Type = "file"
    Contents = "$Contents"
}

}
```


Schritt 3: Verwenden einer PowerShell-Funktion, um Zeilenumbrüche für Windows mit Linux-Zeilenumbruchzeichen zu ersetzen.

```
Function LinuxString($inputStr){
    $outputStr = $inputStr.Replace("`r`n","`n")
    $ouputStr += "`n"
    Return $outputStr
}

Import-DSCResource -Module nx
Node $Node
{

$Contents = @'
search contoso.com
domain contoso.com
nameserver 10.185.85.11
'@

$Contents = LinuxString $Contents

nxFile resolvConf
{
    DestinationPath = "/etc/resolv.conf"
    Mode = "644"        
    Type = "file"
    Contents = $Contents

}
}
```

##Beispiel

Im folgende Beispiel wird sichergestellt, dass das Verzeichnis `/opt/Mydir` vorhanden ist, und eine Datei mit dem angegebenen Inhalt dieses Verzeichnis vorhanden ist.

```
Import-DSCResource -Module nx 

Node $node {
nxFile DirectoryExample
{
   Ensure = "Present"
   DestinationPath = "/opt/mydir"
   Type = "Directory"
}

nxFile FileExample
{
    Ensure = "Present"
    Destinationpath = "/opt/mydir/myfile"
    Contents=@"
#!/bin/bash`necho "hello world"`n
"@ 
    Mode = “755”
    DependsOn = "[nxFile]DirectoryExample"
} 
}
```





