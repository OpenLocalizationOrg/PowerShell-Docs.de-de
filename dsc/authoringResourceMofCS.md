#Erstellen eine Ressource DSC in C

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

In der Regel wird eine benutzerdefinierte Windows PowerShell gewünscht State Configuration (DSC)-Ressource in einem Powershellskript implementiert. Allerdings können Sie die Funktionalität von DSC benutzerdefinierte Ressource implementieren, durch die Cmdlets in c# schreiben. Eine Einführung zum Schreiben von Cmdlets in c# finden Sie unter [ein Windows PowerShell-Cmdlet schreiben](https://technet.microsoft.com/en-us/library/dd878294.aspx).

Abgesehen davon, dass die Ressource im c# als Cmdlets implementieren, der Prozess der Erstellung des MOF-Schemas, die Ordnerstruktur erstellen, importieren und mithilfe der benutzerdefinierten DSC Ressource sind identisch mit den in beschriebenen [beim Schreiben einer benutzerdefinierten DSC-Ressource mit MOF-](authoringResourceMOF.md).

##Beim Schreiben einer Cmdlet-basierten Ressource

In diesem Beispiel implementieren wir eine einfache Ressource, die eine Textdatei und seinen Inhalt zu verwalten.

###Schreiben das MOF-schema

Im folgenden wird die Definition der MOF-Ressource.

```
[ClassVersion("1.0.0"), FriendlyName("xDemoFile")]
class MSFT_XDemoFile : OMI_BaseResource
{
                [Key, Description("path")] String Path;
                [Write, Description("Should the file be present"), ValueMap{"Present","Absent"}, Values{"Present","Absent"}] String Ensure;
                [Write, Description("Contentof file.")] String Content;                   
};
```

###Einrichten von Visual Studio-Projekt

####Einrichten eines Cmdlet-Projekts

1. Öffnen Sie Visual Studio.
1. Erstellen Sie ein C#-Projekt, und geben Sie den Namen.
1. Wählen Sie **-Klassenbibliothek** aus der verfügbaren Projektvorlagen.
1. Klicken Sie auf **Ok**.
1. Fügen Sie dem Projekt einen Assemblyverweis auf System.Automation.Management.dll.
1. Ändern Sie den Namen der Assembly mit dem Ressourcennamen übereinstimmen. In diesem Fall sollte die Assembly den Namen **MSFT_XDemoFile**.

###Den Cmdlet-Code schreiben

Der folgende C#-Code implementiert die **Get-TargetResource**, **Set TargetResource**, und **Test TargetResource** Cmdlets.

```C#
Get-TargetResource
       [OutputType(typeof(System.Collections.Hashtable))]
       [Cmdlet(VerbsCommon.Get, "TargetResource")]
       public class GetTargetResource : PSCmdlet
       {
              [Parameter(Mandatory = true)]
              public string Path { get; set; }

///<summary>
/// Implement the logic to write the current state of the resource as a 
/// Hash table with keys being the resource properties 
/// and the Values are the corresponding current values on the target machine.

///</summary>
              protected override void ProcessRecord()
              {
// Download the zip file at the end of this blog to see sample implementation.
 }

Set-TargetResouce
         [OutputType(typeof(void))]
    [Cmdlet(VerbsCommon.Set, "TargetResource")]
    public class SetTargetResource : PSCmdlet
    {
        privatestring _ensure;
        privatestring _content;

[Parameter(Mandatory = true)]
        public string Path { get; set; }

[Parameter(Mandatory = false)]      
       [ValidateSet("Present", "Absent", IgnoreCase = true)]
       public string Ensure {
            get
            {
                // set the default to present.
               return (this._ensure ?? "Present");
            }
            set
            {
                this._ensure = value;
            }
           } 
            public string Content {
            get { return (string.IsNullOrEmpty(this._content) ? "" : this._content); }
            set { this._content = value; }
        }

///<summary>
        /// Implement the logic to set the state of the machine to the desired state.
        ///</summary>
        protected override void ProcessRecord()
        {
//Implement the set method of the resource 
/* Uncomment this section if your resource needs a machine reboot.
PSVariable DscMachineStatus = new PSVariable("DSCMachineStatus", 1, ScopedItemOptions.AllScope);
                this.SessionState.PSVariable.Set(DscMachineStatus);
*/     
  }
    }
Test-TargetResource    
       [Cmdlet("Test", "TargetResource")]
    [OutputType(typeof(Boolean))]
    public class TestTargetResource : PSCmdlet
    {   

        private string _ensure;
        private string _content;

        [Parameter(Mandatory = true)]
        public string Path { get; set; }

        [Parameter(Mandatory = false)]
        [ValidateSet("Present", "Absent", IgnoreCase = true)]
        public string Ensure
        {
            get
            {
                // set the default to present.
                return (this._ensure ?? "Present");
            }
            set
            {
                this._ensure = value;
            }
        }

        [Parameter(Mandatory = false)]
        public string Content
        {
            get { return (string.IsNullOrEmpty(this._content) ? "“:this._content);}
            set { this._content = value; }
        }

///<summary>
/// Write a Boolean value which indicates whether the current machine is in    
/// desired state or not.
        ///</summary>
        protected override void ProcessRecord()
        {
                // Implement the test method of the resource.
        }
}
```

###Die Ressource wird bereitgestellt

Die kompilierte Dll-Datei sollte in einer Dateistruktur ähnelt einer Ressource skriptbasierte gespeichert werden. Im folgenden sehen die Ordnerstruktur für diese Ressource.

```
$env: psmodulepath (folder)
    |- MyDscResources (folder)
        |- DSCResources (folder)
            |- MSFT_XDemoFile (folder)
                |- MSFT_XDemoFile.psd1 (file, optional)
                |- MSFT_XDemoFile.dll (file, required)
                |- MSFT_XDemoFile.schema.mof (file, required)
```

###Weitere Informationen

####Konzepte

[Schreiben eine benutzerdefinierte DSC-Ressource mit MOF](authoringResourceMOF.md)
####Weitere Ressourcen

[Das Schreiben eines Windows PowerShell-Cmdlets](https://msdn.microsoft.com/en-us/library/dd878294.aspx)



