# CMS-Cmdlets (Cryptographic Message Syntax, Syntax verschlüsselter Nachrichten)

Der CMS-Cmdlets unterstützen die Ver- und Entschlüsselung von Inhalten mithilfe des IETF-Standardformats für kryptografisch geschützte Nachrichten, wie unter [RFC5652](http://tools.ietf.org/html/rfc5652) dokumentiert.

```powershell
Get-CmsMessage [-Content] <string>
Get-CmsMessage [-Path] <string>
Get-CmsMessage [-LiteralPath] <string>
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-Content] <string> [[-OutFile] <string>]
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-Path] <string> [[-OutFile] <string>]
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-LiteralPath] <string> [[-OutFile] <string>]
Unprotect-CmsMessage [-EventLogRecord] <EventLogRecord> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
Unprotect-CmsMessage [-Content] <string> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
Unprotect-CmsMessage [-Path] <string> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
Unprotect-CmsMessage [-LiteralPath] <string> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
```

Der CMS-Verschlüsselungsstandard implementiert die Verschlüsselung mit öffentlichem Schlüssel, bei der die Schlüssel zum Verschlüsseln von Inhalten (der *öffentliche Schlüssel*) und zum Entschlüsseln von Inhalten (der *private Schlüssel*) getrennt sind.

Ihr öffentlicher Schlüssel kann umfassend freigegeben werden, da seine Daten nicht vertraulich sind. Wenn Inhalte mit diesem öffentlichen Schlüssel verschlüsselt sind, können sie nur mit Ihrem privaten Schlüssel entschlüsselt werden. Weitere Informationen über asymmetrische Kryptosysteme finden Sie unter <https://de.wikipedia.org/wiki/Asymmetrisches_Kryptosystem>.

Um in PowerShell erkannt zu werden, benötigen Verschlüsselungszertifikate einen eindeutigen Schlüsselverwendungsbezeichner zum Kennzeichnen als Datenverschlüsselungszertifikate (wie die Bezeichner für „Codesignatur“ und „Verschlüsselte E-Mail“).

Hier ein Beispiel zum Erstellen eines Zertifikats, das sich gut für die Verschlüsselung von Dokumenten eignet:

```powershell
(Change the text in **Subject** to your name, email, or other identifier), and put in a file (i.e.: DocumentEncryption.inf):
[Version]
Signature = "$Windows NT$"
[Strings]
szOID\_ENHANCED\_KEY\_USAGE = "2.5.29.37"
szOID\_DOCUMENT\_ENCRYPTION = "1.3.6.1.4.1.311.80.1"
[NewRequest]
Subject = "<cn=me@somewhere.com>"
MachineKeySet = false
KeyLength = 2048
KeySpec = AT\_KEYEXCHANGE
HashAlgorithm = Sha1
Exportable = true
RequestType = Cert
KeyUsage = "CERT\_KEY\_ENCIPHERMENT\_KEY\_USAGE | CERT\_DATA\_ENCIPHERMENT\_KEY\_USAGE"
ValidityPeriod = "Years"
ValidityPeriodUnits = "1000"
[Extensions]
%szOID\_ENHANCED\_KEY\_USAGE% = "{text}%szOID\_DOCUMENT\_ENCRYPTION%"
```

Führen Sie dann Folgendes aus:
```powershell
certreq -new DocumentEncryption.inf DocumentEncryption.cer
```

Nun können Sie Inhalte ver- und entschlüsseln:

```powershell
$protected = "Hello World" | Protect-CmsMessage -To "\*me@somewhere.com\*[](mailto:*leeholm@microsoft.com*)"
$protected
-----BEGIN CMS-----
MIIBqAYJKoZIhvcNAQcDoIIBmTCCAZUCAQAxggFQMIIBTAIBADA0MCAxHjAcBgNVBAMMFWxlZWhv
bG1AbWljcm9zb2Z0LmNvbQIQQYHsbcXnjIJCtH+OhGmc1DANBgkqhkiG9w0BAQcwAASCAQAnkFHM
proJnFy4geFGfyNmxH3yeoPvwEYzdnsoVqqDPAd8D3wao77z7OhJEXwz9GeFLnxD6djKV/tF4PxR
E27aduKSLbnxfpf/sepZ4fUkuGibnwWFrxGE3B1G26MCenHWjYQiqv+Nq32Gc97qEAERrhLv6S4R
G+2dJEnesW8A+z9QPo+DwYU5FzD0Td0ExrkswVckpLNR6j17Yaags3ltNVmbdEXekhi6Psf2MLMP
TSO79lv2L0KeXFGuPOrdzPAwCkV0vNEqTEBeDnZGrjv/5766bM3GW34FXApod9u+VSFpBnqVOCBA
DVDraA6k+xwBt66cV84OHLkh0kT02SIHMDwGCSqGSIb3DQEHATAdBglghkgBZQMEASoEEJbJaiRl
KMnBoD1dkb/FzSWAEBaL8xkFwCu0e1ZtDj7nSJc=
-----END CMS-----

$protected | Unprotect-CmsMessage
Hello World
```

Parameter des Typs **CMSMessageRecipient** unterstützen Bezeichner in den folgenden Formaten:
- Tatsächliches Zertifikat (wie vom Zertifikatanbieter abgerufen)
- Pfad zu einer Datei mit dem Zertifikat
- Pfad zu einem Verzeichnis mit dem Zertifikat
- Fingerabdruck des Zertifikats (dient zum Nachschlagen im Zertifikatspeicher)
- Name des Antragstellers des Zertifikats (dient zum Nachschlagen im Zertifikatspeicher)

Um Verschlüsselungszertifikate für Dokumente beim Zertifikatanbieter anzuzeigen, können Sie den dynamischen Parameter **-DocumentEncryptionCert** verwenden:

```powershell
dir -DocumentEncryptionCert
```

<!--HONumber=Oct16_HO1-->


