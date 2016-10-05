# [(Übersicht)](overview.md)

# [Konfigurationen](configurations.md)
## [Inkraftsetzung von Konfigurationen](enactingConfigurations.md)
## [Verwenden von Ressourcen mit mehreren Versionen](sxsResource.md)
## [Ausführen von DSC mit Benutzeranmeldeinformationen](runAsUser.md)
## [Die knotenübergreifende Abhängigkeiten angeben](crossNodeDependencies.md)
## [Konfigurationsdaten](configData.md)
### [Optionen für Anmeldeinformationen in der Konfigurationsdaten](configDataCredentials.md)
## [Sichern die MOF-Konfigurationsdatei](secureMOF.md)
## [Teilweise Konfigurationen](partialConfigs.md)
## [Das Schreiben von Hilfe für DSC-Konfigurationen](configHelp.md)

# [Ressourcen](resources.md)
## [Integrierte Ressourcen](builtInResource.md)
### [Archive-Ressource](archiveResource.md)
### [Environment-Ressource](environmentResource.md)
### [File-Ressource](fileResource.md)
### [Group-Ressource](groupResource.md)
### [Log-Ressource](logResource.md)
### [Package-Ressource](packageResource.md)
### [Registry-Ressource](registryResource.md)
### [Script-Ressource](scriptResource.md)
### [Service-Ressource](serviceResource.md)
### [User-Ressource](userResource.md)
### [WindowsFeature-Ressource](windowsfeatureResource.md)
### [WindowsProcess-Ressourcen](windowsProcessResource.md)
## [Erstellen von benutzerdefinierten Ressourcen](authoringResource.md) 
### [MOF-basierte benutzerdefinierte Ressourcen](authoringResourceMOF.md)
#### [MOF-basierte Ressourcen in c#](authoringResourceMofCS.md)
### [Auf Klassen basierende benutzerdefinierte Ressourcen](authoringResourceClass.md)
### [Zusammengesetzte Ressourcen](authoringResourceComposite.md)
### [Schreiben eine Einzelinstanz-DSC-Ressource (empfohlen)](singleInstance.md)
### [Checkliste für die Ressource erstellen](resourceAuthoringChecklist.md)
## [Debuggen von DSC-Ressourcen](debugResource.md)
## [DSC-Ressourcen-Methoden aufrufen (direkt)](directCallResource.md)

# [Konfigurieren des lokalen Konfigurations-Managers (KGV)](metaConfig.md)
## [Konfigurieren die KGV in PowerShell 4.0](metaConfig4.md)

# Das DSC-Pullmodell
## [Einrichten eines Webpullservers](pullServer.md)
## [Einrichten einer SMB DSC-Pull-server](pullServerSMB.md)
## [Einrichten eines Pull-Clients](pullClient.md)
### [Einrichten eines Pullclients mithilfe von Konfigurationsnamen](pullClientConfigNames.md)
### [Einrichten eines Pullclients mithilfe einer Konfigurations-ID](pullClientConfigID.md)
## [Verwenden einen Berichtsserver DSC](reportServer.md)
## [Ziehen Sie best Practices für server](secureServer.md)

# [Problembehandlung bei DSC](troubleshooting.md)

# [Verwenden von DSC auf Nano Server](nanoDsc.md)

# DSC für Linux
## [Erste Schritte mit DSC für Linux](lnxGettingStarted.md)
## [Integrierte Ressourcen für Linux](lnxBuiltInResources.md)
### [NxArchive Ressourcen](lnxArchiveResource.md)
### [NxEnvironment Ressourcen](lnxEnvironmentResource.md)
### [NxFile Ressourcen](lnxFileResource.md)
### [NxFileLine Ressourcen](lnxFileLineResource.md)
### [NxGroup Ressourcen](lnxGroupResource.md)
### [NxPackage Ressourcen](lnxPackageResource.md)
### [NxService Ressourcen](lnxServiceResource.md)
### [NxSshAuthorizedKeys Ressourcen](lnxSshAuthorizedKeysResource.md)
### [NxUser Ressourcen](lnxUserResource.md)

# DSC MOF-Referenz
## [MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager.md)
### [ApplyConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-applyconfiguration.md)
### [DisableDebugConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-disabledebugconfiguration.md)
### [EnableDebugConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-enabledebugconfiguration.md)
### [GetConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-getconfiguration.md)
### [GetConfigurationResultOutput-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-getconfigurationresultoutput.md)
### [GetConfigurationStatus-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-getconfigurationstatus.md)
### [GetMetaConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-getmetaconfiguration.md)
### [PerformRequiredConfigurationChecks-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-performrequiredconfigurationchecks.md)
### [RemoveConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-removeconfiguration.md)
### [ResourceGet-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-resourceget.md)
### [ResourceSet-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-resourceset.md)
### [ResourceTest-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-resourcetest.md)
### [RollBack-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-rollback.md)
### [SendConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-sendconfiguration.md)
### [SendConfigurationApply-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-sendconfigurationapply.md)
### [SendConfigurationApplyAsync-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-sendconfigurationapplyasync.md)
### [SendMetaConfigurationApply-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-sendmetaconfigurationapply.md)
### [StopConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-stopconfiguration.md)
### [TestConfiguration-Methode der MSFT_DSCLocalConfigurationManager-Klasse](msft-dsclocalconfigurationmanager-testconfiguration.md)

# Weitere Ressourcen
## [Whitepapers](whitepapers.md)



<!--HONumber=Oct16_HO1-->


