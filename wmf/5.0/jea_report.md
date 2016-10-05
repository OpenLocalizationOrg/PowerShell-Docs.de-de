# Berichterstellung zu JEA
Um einen Bericht zum Zustand Ihrer JEA-Konfiguration zu erhalten, können Sie die folgenden Cmdlets verwenden:
1.  **Get-PSSessionConfiguration**, um eine Liste aller registrierten Endpunkte auf einem bestimmten Computer zurückzugeben.
2.  **Get-PSSessionCapability**, um die Optionen zu melden, die ein Benutzer auf einem bestimmten Endpunkt hat.

Hier ein Beispiel von **Get-PSSessionCapability**:
```powershell
Get-PSSessionCapability -ConfigurationName Maintenance -Username "CONTOSO\JohnDoe"

CommandType     Name                                               Version    Source           
-----------     ----                                               -------    ------           
Alias           clear -> Clear-Host                                                            
Alias           cls -> Clear-Host                                                              
Alias           exsn -> Exit-PSSession                                                         
Alias           gcm -> Get-Command                                                             
Alias           measure -> Measure-Object                                                      
Alias           select -> Select-Object                                                        
Function        Clear-Host                                                                     
Function        Exit-PSSession                                                                 
Function        Get-Command                                                                    
Function        Get-FormatData                                                                 
Function        Get-Help                                                                       
Function        Get-UserInfo                                                                   
Function        Measure-Object                                                                 
Function        Out-Default                                                                    
Function        Select-Object                                                                  
Cmdlet          Restart-Service                                    3.0.0.0 Microsof...


```

Zum Erstellen eines Berichts zu den _Aktionen_, die ein Benutzer in einer JEA-Sitzung ausgeführt hat, haben Sie diese Möglichkeiten:
1. Aktivieren Sie die „Über die Schulter“-Aufzeichnungen für den gewünschten Endpunkt, und fragen Sie das Verzeichnis mit den Aufzeichnungen auf ein vollständiges Protokoll aller Aktionen des Benutzers ab.
2. Aktivieren Sie die PowerShell-Modulprotokollierung, und überprüfen Sie die PowerShell-Ereignisprotokolle.

<!--HONumber=Oct16_HO1-->


