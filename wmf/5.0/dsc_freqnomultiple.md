# Frequenzen für „RefreshMode“ und „ConfigurationMode“ müssen keine Vielfache des jeweils anderen sein

In der vorherigen Version von DSC behandelt der LCM `RefreshFrequencyMins` und `ConfigurationModeFrequencyMins` als Vielfaches des jeweils anderen. In WMF 5.0 RTM werden diese Eigenschaften unabhängig voneinander verarbeitet. 

Weitere Informationen finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](https://msdn.microsoft.com/powershell/dsc/metaconfig).

<!--HONumber=Oct16_HO1-->


