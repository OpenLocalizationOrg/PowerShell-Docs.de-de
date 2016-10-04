# MOF-Dokumente standardmäßig verschlüsselt

Konfigurationsdokumente enthalten vertrauliche Informationen. In früheren Versionen von DSC mussten Sie zum Schützten von Anmeldeinformationen innerhalb einer Konfiguration Zertifikate verteilen und verwalten. Dies brachte häufig einen erheblichen Verwaltungsaufwand mit sich, wobei es aber immer noch Konfigurationsinformationen gab, die nicht geschützt waren bzw. werden konnten. 

Dies ist nicht mehr der Fall, da **alle MOF-Konfigurationsdateien standardmäßig geschützt sind**. Keine Zertifikate oder Metakonfigurationseinstellungen erforderlich Jedes Mal, wenn eine MOF-Konfigurationsdatei auf einem Zielknoten vom lokalen Konfigurations-Manager (LCM) auf einen Datenträger gespeichert wird, wird sie verschlüsselt. Die MOF-Dateien werden mit [DPAPI](https://msdn.microsoft.com/en-us/library/ms995355.aspx) verschlüsselt. **Hinweis:** Von einem Konfigurationsskript generierte MOF-Dateien werden nicht verschlüsselt.

**Beispiel:** Verschlüsselung im Push-Modus ![MOF-Verschlüsselung](../images/MOF_Encryption.jpg)

Wenn Sie bereits die Zertifikatmethode zum Verschlüsseln von Kennwörtern verwenden oder zusätzliche Sicherheit für Ihre Kennwörter benötigen, funktioniert das [bestehende Verfahren zur zertifikatbasierten Verschlüsselung](https://msdn.microsoft.com/en-us/powershell/dsc/securemof) weiterhin. Das Ergebnis ist ein MOF-Dokument, das mit den DPAPIs vollständig verschlüsselt ist und zusätzlich über darin enthaltene verschlüsselte Kennwörter verfügt.

Diese Verschlüsselung gilt nur für MOF-Konfigurationsdokumente (pending.mof, current.mof, previous.mof, MOF-Teilkonfigurationen). MOF-Dateien mit Metakonfigurationen werden weiterhin unverschlüsselt gespeichert, da sie meist keine geheimen Schlüssel enthalten.


<!--HONumber=Oct16_HO1-->


