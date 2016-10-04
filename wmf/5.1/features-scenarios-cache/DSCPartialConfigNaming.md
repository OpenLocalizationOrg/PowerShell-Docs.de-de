---
title: "Übertragen einer Namenskonvention für eine partielle Konfiguration mithilfe von Pull"
author: jaimeo
contributor: berheabrha
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 368c26766961e760fd2de8c99057121bea076158

---

##Übertragen einer Namenskonvention für eine partielle Konfiguration mithilfe von Pull
In früheren Versionen war die Namenskonvention für eine teilweise Konfiguration der Mof-Dateiname in der Pull-Server-Installationsdetails teilweise den Namen der in den lokalen Configuration Manager-Einstellungen angegeben entsprechen soll, die wiederum entsprechen den Namen der in der MOF-Datei eingebettet. Sehen Sie sich die nachfolgenden Momentaufnahmen an: •   lokale Konfigurationseinstellungen, die eine partielle Konfiguration definieren, die von einem Knoten empfangen werden darf.

![Beispiel einer Metakonfiguration](../../images/MetaConfigPartialOne.png)

•   Beispieldefinition einer partiellen Konfiguration 

```Powershell
Configuration PartialOne
{
    Node('localhost')
    {
        File test 
        {
            DestinationPath = "$env:TEMP\partialconfigexample.txt"
            Contents = 'Partial Config Example'
        }
    }
}
PartialOne
```

•   ConfigurationName, eingebettet in der generierten MOF-Datei

![Beispiel einer generierten MOF-Datei](../../images/PartialGeneratedMof.png)

•   FileName im Repository der Pull-Konfiguration 

![FileName im Repository der Konfiguration](../../images/PartialInConfigRepository.png)

Azure Automation-Dienst generierte Name Mof-Dateien als ``<ConfigurationName>.<NodeName>.mof``. Daher wurde die nachstehende Konfiguration in „PartialOne.Localhost.mof“ kompiliert.  
Dies machte es unmöglich, ziehen Sie eine partielle Konfiguration von Azure Automation-Dienst.

```Powershell
Configuration PartialOne
{
    Node('localhost')
    {
        File test 
        {
            DestinationPath = "$env:TEMP\partialconfigexample.txt"
            Contents = 'Partial Config Example'
        }
    }
}
PartialOne
```

In WMF 5.1 kann die Teilkonfiguration im Pullserver/-dienst nach dem Schema „``<ConfigurationName>.<NodeName>.mof``“ benannt werden. Darüber hinaus können ein Computer aus ein Pull-Vorgang eine einzige Konfiguration stammen Server-Dienst dann Konfigurationsdokument auf Pull-Server-Konfigurations-Repository einen beliebigen Namen haben. Diese Benennungskonvention Flexibilität ermöglicht die Verwaltung von partiellen Konfiguration eines Knotens mit lokal und Azure Automation Pull-Dienst. Beispielsweise haben Sie "base" teilweise Konfigurationen, die lokal abgelegten ruft und eine andere partielle Konfiguration, die aus Azure Automation-Dienst abgerufen ruft.

Die nachfolgende Metakonfiguration richtet einen Knoten ein, der sowohl lokal als auch über den Azure Automation-Dienst verwaltet wird.

```Powershell
  [DscLocalConfigurationManager()]
   Configuration RegistrationMetaConfig
   {
        Settings
        {
            RefreshFrequencyMins = 30;
            RefreshMode = "PULL";            
        }

        ConfigurationRepositoryWeb web
        {
            ServerURL =  $endPoint
            RegistrationKey = $registrationKey
            ConfigurationNames = $configurationName
        }

        # Partial configuration managed by Azure Automation Service.
        PartialConfiguration PartialCOnfigurationManagedByAzureAutomation
        {
            ConfigurationSource = "[ConfigurationRepositoryWeb]Web"   
        }
    
        # This partial configuration is managed locally.
        PartialConfiguration OnPremConfig
        {
            RefreshMode = "PUSH"
            ExclusiveResources = @("Script")
        }

   }

   RegistrationMetaConfig
   slcm -Path .\RegistrationMetaConfig -Verbose
 ```





<!--HONumber=Oct16_HO1-->


