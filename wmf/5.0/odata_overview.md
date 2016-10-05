# Generieren von PowerShell-Cmdlets basierend auf einem OData-Endpunkt
Generieren von Windows PowerShell-Cmdlets basierend auf einem OData-Endpunkt
--------------------------------------------------------------

**Export-ODataEndpointProxy** ist ein Cmdlet, das basierend auf der von einem bestimmten OData-Endpunkt verfügbar gemachten Funktionalität verschiedene Windows PowerShell-Cmdlets generiert.

Im folgenden Beispiel wird gezeigt, wie dieses neue Cmdlet verwendet wird:

\# Einfacher Anwendungsfall von „Export-ODataEndpointProxy“

```powershell
Export-ODataEndpointProxy -Uri 'http://services.odata.org/v3/(S(snyobsk1hhutkb2yulwldgf1))/odata/odata.svc' -OutputModule C:\Users\user\Generated.psd1

ipmo 'C:\Users\user\Generated.psd1'
# Cmdlets are created based on the following heuristics
# New-<EntityType> -<Key> [-<Other Attributes>]
#
# Get-<EntityType> [-<Key> -Top –Skip –Filter -OrderBy]
# # If there is a complex key, the keys will actually be -<Key1> -<Key2>…
# # Note this rule applies to any other instances of the key
#
# Set-<EntityType> -<Key> [-<Other Attributes>]
#
# Remove-<EntityType> -<Key>
#
# Invoke-<EntityType><Action> [-<Key> -<Other Parameters>]
#
#
# Cmdlets from associations (Note: Get and Remove get additional parameter sets)
# Get-<EntityType> -<AssociatedEntity>
# New-<EntityType> -<AssociatedEntity> -<Key>
# Remove-<EntityType> -<AssociatedEntity> -<Key>
#
#
# Note: Every cmdlet has the –ConnectionURI parameter for explicitly setting the URI of the endpoint. This normally uses the same address that you gave the Export-ODataEndpointProxy cmdlet, but can be overridden in this fashion for the sake of similar endpoints.
#
```

Teile wichtiger Anwendungsfälle dieser Funktionalität befinden sich noch in der Entwicklung, so z. B.:
-   Zuordnungen
-   Übergeben von Datenströmen

Generieren von Windows PowerShell-Cmdlets basierend auf einem OData-Endpunkt mit ODataUtils
------------------------------------------------------------------------------
Das „ODataUtils“-Modul ermöglicht die Generierung von Windows PowerShell-Cmdlets anhand von REST-Endpunkten, die OData unterstützen. Das Windows PowerShell-Modul „Microsoft.PowerShell.ODataUtils“ weist die folgenden inkrementellen Verbesserungen auf.
-   Übertragen zusätzlicher Informationen vom serverseitigen Endpunkt zur Clientseite
-   Unterstützung einer clientseitigen Auslagerung
-   Serverseitige Filterung mithilfe des „-Select“-Parameters
-   Unterstützung für Webanforderungsheader

Die vom Cmdlet „Export-ODataEndPointProxy“ generierten „Proxy“-Cmdlets bieten zusätzliche Informationen (die nicht in den während der clientseitigen Proxygenerierung verwendeten „$metadata“ enthalten sind) vom serverseitigen OData-Endpunkt zum „Information“-Datenstrom (einem neuen Windows PowerShell 5.0-Feature). Hier ein Beispiel des Abrufs dieser Informationen.
```powershell
Import-Module Microsoft.PowerShell.ODataUtils -Force
$generatedProxyModuleDir = Join-Path -Path $env:SystemDrive -ChildPath 'ODataDemoProxy'
$uri = "http://services.odata.org/V3/(S(fhleiief23wrm5a5nhf542q5))/OData/OData.svc/"
Export-ODataEndpointProxy -Uri $uri -OutputModule $generatedProxyModuleDir -Force -AllowUnSecureConnection -Verbose -AllowClobber
Import-Module $generatedProxyModuleDir -Force

# In the below command, we are retrieving top 1 product.
# By specifying -IncludeTotalResponseCount parameter,
# we are getting the total count of all the Product records
# available on the server side. This information
# is surfaced on the client side through the Information stream.
$product = Get-Product -Top 1 -AllowUnsecureConnection -AllowAdditionalData -IncludeTotalResponseCount -InformationVariable infoStream
# The Information stream contains the additional
# information sent from the server side.
$additionalInfo = $infoStream.GetEnumerator() | % MessageData
# 'Odata.Count' indicates the total product records
# available on the server side Odata endpoint.
$additionalInfo['odata.count']
```

Sie können die Datensätze von der Serverseite dank Unterstützung der clientseitigen Auslagerung in Batches abrufen. Dies ist hilfreich, wenn Sie eine große Datenmenge vom Server über das Netzwerk abrufen müssen.
```powershell
$skipCount = 0
$batchSize = 3
# Client-Side Paging Support: The records from the server side
# are retrieved in batches of $batchSize
while($skipCount -le $additionalInfo['odata.count'])
{
Get-Product -AllowUnsecureConnection -AllowAdditionalData -Top $batchSize -Skip $skipCount
$skipCount += $batchSize
}
```

Die generierten „Proxy“-Cmdlets unterstützen den „–Select“-Parameter, den Sie als Filter verwenden können, um nur die Datensatzeigenschaften zu empfangen, die der Client benötigt. Dies reduziert die Menge der Daten, die über das Netzwerk übertragen werden, da die Filterung auf dem Server erfolgt.
```powershell
# In the below example only the Name property of the
# Product record is retrieved from the server side.
Get-Product -Top 2 -AllowUnsecureConnection -AllowAdditionalData -Select Name
```

Das Cmdlet „Export-ODataEndpointProxy“ und die von ihm generierten „Proxy“-Cmdlets unterstützen nun den „Headers“-Parameter (geben Sie Werte als Hashtabelle an). Sie können ihn verwenden, um zusätzliche Informationen zu übertragen, die vom serverseitigen OData-Endpunkt erwartet werden. Im folgenden Beispiel können Sie einen „Subscription“-Schlüssel mithilfe des „Headers“-Parameters für Dienste übertragen, die einen „Subscription“-Schlüssel für die Authentifizierung erwarten.
```powershell
# As an example, in the below command 'XXXX' is the authentication used by the
# Export-ODataEndpointProxy cmdlet to interact with the server-side
# OData endpoint accessed through $endPointUri.

Export-ODataEndpointProxy -Uri $endPointUri -OutputModule $generatedProxyModuleDir -Force -AllowUnSecureConnection -Verbose -Headers @{'subscription-key'='XXXX'}
```


<!--HONumber=Oct16_HO1-->


