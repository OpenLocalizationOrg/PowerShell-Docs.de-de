# Protokollierung des Softwarebestands (Software Inventory Logging, SIL)

**WICHTIG: ** *Bei der Installation von WMF 5.0 auf einem Server mit Windows Server 2012 R2 Server, auf dem die Protokollierung des Softwarebestands (SIL) bereits ausgeführt wird, muss das Cmdlet „Start-SilLogging“ nach der Installation von WMF einmal ausgeführt werden, da der Installationsvorgang das Feature „Protokollierung des Softwarebestands“ fälschlicherweise beendet.*

Durch die Protokollierung des Softwarebestands sollen die Betriebskosten für das Abrufen genauer Informationen zu der lokal auf einem Server bereitgestellten Microsoft-Software reduziert werden, vor allem aber für das Abrufen dieser Informationen von zahlreichen Servern in einer IT-Umgebung (sofern die Software in der gesamten IT-Umgebung installiert ist und ausgeführt wird). Sofern eingerichtet, können Sie diese Daten an einen Aggregationsserver weiterleiten und die Protokolldaten mittels eines einheitlichen, automatischen Prozesses zentral sammeln.

Sie können Daten zum Softwarebestand auch über ein direktes Abfragen aller Computer protokollieren. Doch mit der Protokollierung des Softwarebestands können (durch die Nutzung einer von jedem Server ausgelösten Weiterleitung) Probleme bei der Serverermittlung überwunden werden, die für viele Software-Inventur- und Ressourcenverwaltungsszenarien typisch sind. Die Software-Inventurprotokollierung verwendet SSL, um Daten zu schützen, die über HTTPS an einen Aggregationsserver weitergeleitet werden. Da die Daten an zentraler Stelle gespeichert werden, können sie einfacher analysiert, bearbeitet und freigegeben werden.

Im Rahmen der Funktionalität des Features werden keine Daten an Microsoft gesendet. Die Daten und die Funktionalität der Softwareinventurprotokollierung sind ausschließlich zur Verwendung durch den lizenzierten Besitzer der Serversoftware und durch Administratoren gedacht.

Weitere Informationen und Dokumentation zu Cmdlets zur Protokollierung des Softwarebestands (Software Inventory Logging; SIL) finden Sie in den Windows Server 2012 R2-Onlineressourcen unter <http://technet.microsoft.com/library/dn383584.aspx>.


<!--HONumber=Oct16_HO1-->


