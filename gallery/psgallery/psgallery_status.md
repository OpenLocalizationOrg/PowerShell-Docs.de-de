PowerShell-Katalogstatus
=========================


## 8/10/2016 - aufgelöst: kann nicht zum Senden von e-Mails an cgadmin@microsoft.com

__Zusammenfassung der Auswirkungen__: zwischen 8/5/2016 und 10/8/2016, konnten Kunden zum Senden von e-Mails an cgadmin@microsoft.com, oder der Kontakt-Funktion zu verwenden.  
__Ursache__: Als Ursache haben Techniker eine Konfigurationsänderung des E-Mail-Kontos ausgemacht.  
__Lösung__: Techniker haben das Konfigurationsproblem behoben.  
__Nächste Schritte__: Wenn Sie gesendeten e-Mail-Nachrichten an oder den Link Kontakt verwendet cgadmin@microsoft.com während dieser Zeit, und wir haben nicht geantwortet, versuchen Sie es erneut. Vielen Dank für Ihre Geduld.



## 13.07.2016 – Herunterladen von Elementen nicht möglich

__Zusammenfassung der Auswirkungen__: Vom 11.7.2016 bis 13.7.2016 hatten Kunden teilweise Probleme beim Herunterladen von Elementen aus dem PowerShell-Katalog. Das Problem manifestierte sich in der folgenden Fehlermeldung, die von „Install-Module/Install-Script“ und „Save-Module/Save-Script“ zurückgegeben wurde:

```PowerShell
PS C:\> Install-Module xStorage 
PackageManagement\Install-Package : Package 'xStorage' failed to be installed because: 
End of Central Directory record could not be found. At C:\Program 
Files\WindowsPowerShell\Modules\PowerShellGet\1.0.0.1\PSModule.psm1:1375 char:21 + ... 
$null = PackageManagement\Install-Package @PSBoundParameters + 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ + CategoryInfo : InvalidResult: 
(xStorage:String) [Install-Package], Exception + FullyQualifiedErrorId : Package '{0}' 
failed to be installed because: {1},Microsoft.PowerShell.PackageManagement.Cmdlets.InstallPackage 
```

__Vorläufige Ursache__: Techniker haben ein Problem mit dem Azure Content Delivery Network (CDN) ausgemacht, das am 11.7.2016 im PowerShell-Katalog bereitgestellt wurde.  
__Lösung__: Techniker haben Azure CDN im PowerShell-Katalog deaktiviert.  
__Nächste Schritte__: Untersuchen der Ursache und Entwickeln einer Lösung, um zu verhindern, dass dies künftig erneut vorkommt.


## 19.05.2016 – Herunterladen von Elementen nicht möglich
__Zusammenfassung der Auswirkungen__: Vom 17.5.2016 bis 19.5.2016 hatten Kunden teilweise Probleme beim Herunterladen von Elementen aus dem PowerShell-Katalog. Das Problem manifestierte sich in der folgenden Fehlermeldung, die von „Install-Module/Install-Script“ und „Save-Module/Save-Script“ zurückgegeben wurde:

```PowerShell
VERBOSE: Hash for package 'AzureRM.OperationalInsights' does not match hash provided from the server.
VERBOSE: InstallPackageLocal' - name='AzureRM.OperationalInsights', version='1.0.8',
destination='C:\Users\jbritt\AppData\Local\Temp\2\1741355729'
WARNING: Package 'AzureRM.OperationalInsights' failed to be installed because: 
End of Central Directory record could not be found. 
WARNING: Dependent Package 'AzureRM.OperationalInsights' failed to install. 
WARNING: Package 'AzureRM' failed to install. 
VERBOSE: Module 'AzureRM.Network' was saved successfully. 
VERBOSE: Saving the dependency module 'AzureRM.NotificationHubs' with version '1.0.8' for the 
module 'AzureRM'. 
VERBOSE: Module 'AzureRM.NotificationHubs' was saved successfully. 
PackageManagement\Save-Package : Unable to save the module 'AzureRM'. At C:\Program 
Files\WindowsPowerShell\Modules\PowerShellGet\1.0.0.1\PSModule.psm1:1187 char:21 + 
$null = PackageManagement\Save-Package @PSBoundParameters + 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ + 
CategoryInfo : InvalidOperation: (Microsoft.Power...ets.SavePackage:SavePackage) 
[Save-Package], Exception + FullyQualifiedErrorId : ProviderFailToDownloadFile,
Microsoft.PowerShell.PackageManagement.Cmdlets.SavePackage 
```

__Vorläufige Ursache__: Techniker haben ein Problem mit dem Azure Content Delivery Network (CDN) ausgemacht, das am 17.5.2016 im PowerShell-Katalog bereitgestellt wurde.  
__Lösung__: Techniker haben Azure CDN im PowerShell-Katalog deaktiviert.  
__Nächste Schritte__: Untersuchen der Ursache und Entwickeln einer Lösung, um zu verhindern, dass dies künftig erneut vorkommt.


<!--HONumber=Oct16_HO1-->


