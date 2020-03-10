---
uid: config-builder
title: Konfigurations-Generatoren für ASP.net
author: rick-anderson
description: Erfahren Sie, wie Sie Konfigurationsdaten aus anderen Quellen als Web. config-Werte aus externen Quellen erhalten.
ms.author: riande
ms.date: 10/29/2018
msc.type: content
ms.openlocfilehash: 5299d9ab057c3096773955a7461e77a80673ebfe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472305"
---
# <a name="configuration-builders-for-aspnet"></a>Konfigurations-Generatoren für ASP.net

Von [Stephen Molloy](https://github.com/StephenMolloy) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Konfigurations-Generatoren stellen einen modernen und agilen Mechanismus für ASP.net-apps bereit, um Konfigurationswerte aus externen Quellen zu erhalten.

Konfigurations-Generatoren:

* Sind in .NET Framework 4.7.1 und höher verfügbar.
* Stellen Sie einen flexiblen Mechanismus zum Lesen von Konfigurations Werten bereit.
* Berücksichtigen Sie einige der grundlegenden Anforderungen von apps, die in einen Container und eine in der Cloud fokussierte Umgebung verschoben werden.
* Kann verwendet werden, um den Schutz von Konfigurationsdaten durch das Zeichnen aus Quellen zu verbessern, die zuvor nicht verfügbar waren (z. b. Azure Key Vault und Umgebungsvariablen) im .NET-Konfigurationssystem.

## <a name="keyvalue-configuration-builders"></a>Schlüssel-Wert-Konfigurations-Generatoren

Ein häufiges Szenario, das von Configuration Builder behandelt werden kann, ist die Bereitstellung eines grundlegenden Schlüssel-Wert-ersatzmechanismus für Konfigurations Abschnitte, die einem Schlüssel-Wert-Muster folgen. Das .NET Framework Konzept von configurationbuilder ist nicht auf bestimmte Konfigurations Abschnitte oder Muster beschränkt. Viele der Konfigurations-Generatoren in `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [nuget](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funktionieren jedoch innerhalb des Schlüssel-Wert-Musters.

## <a name="keyvalue-configuration-builders-settings"></a>Einstellungen für die Schlüssel-/wertkonfigurationsbuilder

Die folgenden Einstellungen gelten für alle Schlüssel-Wert-Konfigurations-Generatoren in `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modus

Die Konfigurations-Generatoren verwenden eine externe Quelle von Schlüssel-Wert-Informationen, um die ausgewählten Schlüssel-Wert-Elemente des Konfigurations Systems aufzufüllen. Insbesondere die Abschnitte "`<appSettings/>`" und "`<connectionStrings/>`" werden von den Konfigurations-Generatoren besonders behandelt. Die Generatoren funktionieren in drei Modi:

* `Strict`: der Standardmodus. In diesem Modus arbeitet der Konfigurations-Generator nur mit bekannten Schlüssel/Wert-zentrierten Konfigurations Abschnitten. im `Strict` Modus werden die einzelnen Schlüssel im-Abschnitt aufgelistet. Wenn ein übereinstimmender Schlüssel in der externen Quelle gefunden wird:

   * Die Konfigurations-Generatoren ersetzen den Wert im resultierenden Konfigurations Abschnitt durch den Wert aus der externen Quelle.
* `Greedy`: Dieser Modus ist eng mit `Strict` Modus verknüpft. Anstatt auf Schlüssel beschränkt zu sein, die in der ursprünglichen Konfiguration bereits vorhanden sind:

  * Die Konfigurations-Generatoren fügen alle Schlüssel-Wert-Paare aus der externen Quelle dem sich ergebenden Konfigurations Abschnitt hinzu.

* `Expand`-für die unformatierte XML-Datei vor dem analysieren in ein Konfigurations Abschnitts Objekt. Dies kann sich als Erweiterung von Token in einer Zeichenfolge vorstellen. Jeder Teil der unformatierten XML-Zeichenfolge, die dem Muster `${token}` entspricht, ist ein Kandidat für die Tokenerweiterung. Wenn kein entsprechender Wert in der externen Quelle gefunden wird, wird das Token nicht geändert. Generatoren in diesem Modus sind nicht auf die Abschnitte `<appSettings/>` und `<connectionStrings/>` beschränkt.

Mit dem folgenden Markup aus *Web. config* wird der [Umgebungs](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) -Generator im `Strict` Modus aktiviert:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Der folgende Code liest die `<appSettings/>` und `<connectionStrings/>`, die in der vorherigen *Web. config* -Datei angezeigt werden:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Im vorangehenden Code werden die Eigenschaftswerte auf festgelegt:

* Die Werte in der Datei " *Web. config* ", wenn die Schlüssel nicht in Umgebungsvariablen festgelegt sind.
* Die Werte der Umgebungsvariablen, sofern festgelegt.

`ServiceID` enthalten beispielsweise Folgendes:

* "Serviceid value from Web. config", wenn die Umgebungsvariable `ServiceID` nicht festgelegt ist.
* Der Wert der `ServiceID` Umgebungsvariablen, sofern festgelegt.

Die folgende Abbildung zeigt die `<appSettings/>` Schlüssel/Werte aus der vorherigen Datei *Web. config* , die im Umgebungs-Editor festgelegt wurde:

![Umgebungs-Editor](config-builder/static/env.png)

Hinweis: möglicherweise müssen Sie Visual Studio beenden und neu starten, um Änderungen in Umgebungsvariablen anzuzeigen.

### <a name="prefix-handling"></a>Präfix Behandlung

Wichtige Präfixe können das Festlegen von Schlüsseln vereinfachen:

* Die .NET Framework Konfiguration ist komplex und wird nicht eingefügt.
* Externe Schlüssel-/Wertquellen sind im allgemeinen grundlegend und flach. Umgebungsvariablen sind z. b. nicht vollständig.

Verwenden Sie einen der folgenden Ansätze, um mithilfe von Umgebungsvariablen sowohl `<appSettings/>` als auch `<connectionStrings/>` in die Konfiguration einzufügen:

* Mit dem `EnvironmentConfigBuilder` im Standard `Strict` Modus und den entsprechenden Schlüsselnamen in der Konfigurationsdatei. Der vorangehende Code und das Markup haben diese Vorgehensweise. Wenn Sie diesen Ansatz verwenden, können Sie in `<appSettings/>` und `<connectionStrings/>`**nicht** identisch benannte Schlüssel haben.
* Verwenden Sie im `Greedy` Modus zwei `EnvironmentConfigBuilder`s mit unterschiedlichen Präfixen und `stripPrefix`. Bei diesem Ansatz kann die APP `<appSettings/>` und `<connectionStrings/>` lesen, ohne die Konfigurationsdatei aktualisieren zu müssen. Der nächste Abschnitt, [stripprefix](#stripprefix), zeigt, wie dies geschieht.
* Verwenden Sie im `Greedy` Modus zwei `EnvironmentConfigBuilder`s mit unterschiedlichen Präfixen. Bei dieser Vorgehensweise können keine doppelten Schlüsselnamen vorhanden sein, da Schlüsselnamen sich durch das Präfix unterscheiden müssen.  Beispiel:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Mit dem vorangehenden Markup kann die gleiche flatkey/Value-Quelle verwendet werden, um die Konfiguration für zwei verschiedene Abschnitte aufzufüllen.

In der folgenden Abbildung werden die `<appSettings/>`-und `<connectionStrings/>` Schlüssel/Werte aus der vorherigen Datei *Web. config* im Umgebungs-Editor angezeigt:

![Umgebungs-Editor](config-builder/static/prefix.png)

Der folgende Code liest die `<appSettings/>`-und `<connectionStrings/>` Schlüssel/-Werte, die in der vorherigen *Web. config* -Datei enthalten sind:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Im vorangehenden Code werden die Eigenschaftswerte auf festgelegt:

* Die Werte in der Datei " *Web. config* ", wenn die Schlüssel nicht in Umgebungsvariablen festgelegt sind.
* Die Werte der Umgebungsvariablen, sofern festgelegt.

Beispielsweise werden die folgenden Werte festgelegt, indem Sie die vorherige *Web. config* -Datei, die Schlüssel/Werte im vorherigen Bild des Umgebungs-Editors und den vorherigen Code verwenden:

|  Key              | value |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID von "dev"-Variablen|
|    AppSetting_default            | AppSetting_default Wert aus der enumerationsdatei |
|       ConnStr_default         | ConnStr_default Val von der Gesamt Version|

### <a name="stripprefix"></a>stripprefix

`stripPrefix`: Boolescher Wert ist standardmäßig `false`. 

Das vorangehende XML-Markup trennt App-Einstellungen von Verbindungs Zeichenfolgen, erfordert jedoch, dass alle Schlüssel in der Datei " *Web. config* " das angegebene Präfix verwenden. Beispielsweise muss das Präfix `AppSetting` dem `ServiceID` Schlüssel ("AppSetting_ServiceID") hinzugefügt werden. Bei `stripPrefix`wird das Präfix nicht in der Datei " *Web. config* " verwendet. Das Präfix ist in der Configuration Builder-Quelle erforderlich (z. b. in der-Umgebung). Wir erwarten, dass die meisten Entwickler `stripPrefix`verwenden.

Anwendungen entfernen in der Regel das Präfix. Mit der folgenden *Web. config* -Datei wird das Präfix entfernt:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

In der vorherigen *Web. config* -Datei befindet sich der `default` Schlüssel sowohl in der `<appSettings/>` als auch in der `<connectionStrings/>`.

In der folgenden Abbildung werden die `<appSettings/>`-und `<connectionStrings/>` Schlüssel/Werte aus der vorherigen Datei *Web. config* im Umgebungs-Editor angezeigt:

![Umgebungs-Editor](config-builder/static/prefix.png)

Der folgende Code liest die `<appSettings/>`-und `<connectionStrings/>` Schlüssel/-Werte, die in der vorherigen *Web. config* -Datei enthalten sind:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Im vorangehenden Code werden die Eigenschaftswerte auf festgelegt:

* Die Werte in der Datei " *Web. config* ", wenn die Schlüssel nicht in Umgebungsvariablen festgelegt sind.
* Die Werte der Umgebungsvariablen, sofern festgelegt.

Beispielsweise werden die folgenden Werte festgelegt, indem Sie die vorherige *Web. config* -Datei, die Schlüssel/Werte im vorherigen Bild des Umgebungs-Editors und den vorherigen Code verwenden:

|  Key              | value |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID von "dev"-Variablen|
|    default            | AppSetting_default Wert aus der enumerationsdatei |
|    default         | ConnStr_default Val von der Gesamt Version|

### <a name="tokenpattern"></a>tokenpattern

`tokenPattern`: String, standardmäßig `@"\$\{(\w+)\}"`

Das `Expand` Verhalten der Generatoren durchsucht die unformatierten XML-Daten nach Token, die wie `${token}`aussehen. Die Suche erfolgt mit dem standardmäßigen regulären Ausdruck `@"\$\{(\w+)\}"`. Der Zeichensatz, der `\w` entspricht, ist strenger als XML, und viele Konfigurations Quellen erlauben. Verwenden Sie `tokenPattern`, wenn im Tokennamen mehr Zeichen als `@"\$\{(\w+)\}"` erforderlich sind.

`tokenPattern`: Zeichenfolge:

* Ermöglicht es Entwicklern, den Regex zu ändern, der für die Tokenübereinstimmung verwendet wird.
* Es wird keine Validierung durchgeführt, um sicherzustellen, dass es sich um einen wohlgeformten, nicht gefährlichen Regex handelt.
* Es muss eine Erfassungs Gruppe enthalten. Der gesamte regex muss mit dem gesamten Token identisch sein. Die erste Erfassung muss der Tokenname sein, der in der Konfigurations Quelle gesucht werden soll.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Configuration Builder in Microsoft. Configuration. configurationbuilder

### <a name="environmentconfigbuilder"></a>Umgebungs-Generator

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[Umgebungs](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/)Konfigurations Generator:

* Ist die einfachste der Configuration Builder.
* Liest Werte aus der Umgebung.
* Verfügt über keine zusätzlichen Konfigurationsoptionen.
* Der `name` Attribut Wert ist willkürlich.

**Hinweis:** In einer Windows-Container Umgebung werden Variablen, die zur Laufzeit festgelegt werden, nur in die EntryPoint-Prozessumgebung eingefügt. Apps, die als Dienst oder Non-EntryPoint-Prozess ausgeführt werden, nehmen diese Variablen nur dann entgegen, wenn Sie anderweitig über einen Mechanismus im Container eingefügt werden. Bei [IIS](https://github.com/Microsoft/iis-docker/pull/41) -/[ASP.net](https://github.com/Microsoft/aspnet-docker)-basierten Containern behandelt die aktuelle Version von [Servicemonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) dies nur im *DefaultAppPool* . Andere Windows-basierte containervarianten müssen möglicherweise Ihren eigenen einschleusungs Mechanismus für nicht-EntryPoint-Prozesse entwickeln.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Speichern Sie niemals Kenn Wörter, sensible Verbindungs Zeichenfolgen oder andere sensible Daten im Quellcode. Produktionsgeheimnisse sollten nicht für Entwicklungs-oder Testzwecke verwendet werden.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Dieser Konfigurations-Generator stellt eine ähnliche Funktion wie [ASP.net Core Secret Manager](/aspnet/core/security/app-secrets)bereit.

[Usersecretyconfigbuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) kann in .NET Framework Projekten verwendet werden, aber es muss eine geheime Datei angegeben werden. Alternativ können Sie die `UserSecretsId`-Eigenschaft in der Projektdatei definieren und die Datei mit den unformatierten Geheimnissen am richtigen Speicherort für den Lesevorgang erstellen. Um externe Abhängigkeiten aus dem Projekt herauszuhalten, ist die geheime Datei XML-formatiert. Die XML-Formatierung ist ein Implementierungsdetail, und das Format sollte nicht verlassen werden. Wenn Sie eine Datei " *Secrets. JSON* " für .net Core-Projekte freigeben müssen, sollten Sie die Verwendung von " [simplejsonconfigbuilder](#simplejsonconfigbuilder)" in Erwägung gezogen. Das `SimpleJsonConfigBuilder`-Format für .net Core sollte auch als Implementierungsdetails angesehen werden, die geändert werden können.

Konfigurations Attribute für `UserSecretsConfigBuilder`:

* `userSecretsId`: Dies ist die bevorzugte Methode zum Identifizieren einer XML-Geheimnis Datei. Es funktioniert ähnlich wie .net Core, das eine `UserSecretsId` Project-Eigenschaft verwendet, um diesen Bezeichner zu speichern. Die Zeichenfolge muss eindeutig sein, es muss sich nicht um eine GUID handeln. Mit diesem Attribut untersuchen die `UserSecretsConfigBuilder` einen bekannten lokalen Speicherort (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) für eine geheime Datei, die zu diesem Bezeichner gehört.
* `userSecretsFile`: ein optionales Attribut, das die Datei mit den geheimen Schlüsseln angibt. Das `~` Zeichen kann am Anfang verwendet werden, um auf den Anwendungs Stamm zu verweisen. Entweder dieses Attribut oder das `userSecretsId` Attribut ist erforderlich. Wenn beide angegeben sind, hat `userSecretsFile` Vorrang.
* `optional`: Boolescher Wert, der Standardwert `true`-verhindert eine Ausnahme, wenn die geheime Datei nicht gefunden werden kann. 
* Der `name` Attribut Wert ist willkürlich.

Die Geheimnis Datei hat das folgende Format:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[Azurekeyvaultconfigbuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) liest Werte, die in der [Azure Key Vault](/azure/key-vault/key-vault-whatis)gespeichert sind.

`vaultName` ist erforderlich (entweder der Name des Tresors oder ein URI für den Tresor). Mit den anderen Attributen können Sie steuern, mit welchem Tresor eine Verbindung hergestellt werden soll. Dies ist jedoch nur erforderlich, wenn die Anwendung nicht in einer Umgebung ausgeführt wird, die mit `Microsoft.Azure.Services.AppAuthentication`arbeitet. Die Authentifizierungs Bibliothek für Azure-Dienste wird verwendet, um nach Möglichkeit automatisch Verbindungsinformationen aus der Ausführungsumgebung auszuwählen. Sie können die automatische Erfassung von Verbindungsinformationen überschreiben, indem Sie eine Verbindungs Zeichenfolge angeben.

* `vaultName`: erforderlich, wenn `uri` nicht angegeben ist. Gibt den Namen des Tresors in Ihrem Azure-Abonnement an, aus dem Schlüssel-Wert-Paare gelesen werden sollen.
* `connectionString`-eine Verbindungs Zeichenfolge, die von [azureservicestokenprovider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support) verwendet wird.
* `uri`: stellt eine Verbindung mit anderen Key Vault Anbietern mit dem angegebenen `uri` Wert her. Wenn nicht angegeben, ist Azure (`vaultName`) der Tresor Anbieter.
* `version`-Azure Key Vault bietet eine Versions Verwaltungsfunktion für Geheimnisse. Wenn `version` angegeben wird, ruft der Generator nur die geheimen Schlüssel ab, die mit dieser Version übereinstimmen.
* `preloadSecretNames`: Standardmäßig fragt dieser Generator **alle** Schlüsselnamen im Schlüssel Tresor ab, wenn er initialisiert wird. Um zu verhindern, dass alle Schlüsselwerte gelesen werden, legen Sie dieses Attribut auf `false`fest. Wenn diese Einstellung auf `false` festgelegt wird, werden die geheimen Schlüssel einzeln gelesen. Das Lesen von geheimen Schlüsseln kann nützlich sein, wenn der Tresor den Zugriff auf "abrufen", aber nicht auf "auflisten" zulässt. **Hinweis:** Wenn Sie `Greedy` Modus verwenden, müssen `preloadSecretNames` `true` (Standard) sein.

### <a name="keyperfileconfigbuilder"></a>Keyperfileconfigbuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[Keyperfileconfigbuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) ist ein grundlegender Konfigurations-Generator, der die Dateien eines Verzeichnisses als Quelle von Werten verwendet. Der Name einer Datei ist der Schlüssel, und der Inhalt ist der Wert. Dieser Konfigurations-Generator kann nützlich sein, wenn er in einer orchestrierten Container Umgebung ausgeführt wird. Systeme wie docker Swarm und Kubernetes bieten `secrets` ihren orchestrierten Windows-Containern in dieser Schlüssel-pro-Datei-Weise.

Attribut Details:

* `directoryPath` – Erforderlich. Gibt einen Pfad an, in dem nach Werten gesucht werden soll. Docker für Windows geheimen Schlüssel werden standardmäßig im Verzeichnis " *c:\programdata\docker\secrets* " gespeichert.
* `ignorePrefix` Dateien, die mit diesem Präfix beginnen, werden ausgeschlossen. Der Standardwert ist „ignore“ (ignorieren).
* `keyDelimiter`-Standardwert ist `null`. Wenn dieser Wert angegeben ist, durchläuft der Konfigurations-Generator mehrere Ebenen des Verzeichnisses und erstellt Schlüsselnamen mit diesem Trennzeichen. Wenn dieser Wert `null`ist, untersucht der Konfigurations-Generator nur die oberste Verzeichnisebene.
* `optional`-Standardwert ist `false`. Gibt an, ob der Konfigurations-Generator Fehler verursachen soll, wenn das Quellverzeichnis nicht vorhanden ist.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Speichern Sie niemals Kenn Wörter, sensible Verbindungs Zeichenfolgen oder andere sensible Daten im Quellcode. Produktionsgeheimnisse sollten nicht für Entwicklungs-oder Testzwecke verwendet werden.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

.Net Core-Projekte verwenden häufig JSON-Dateien für die Konfiguration. Der [simplejsonconfigbuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) -Generator ermöglicht die Verwendung von .net Core-JSON-Dateien in der .NET Framework. Dieser Konfigurations-Generator stellt eine einfache Zuordnung von einer flachen Schlüssel-/Wert-Quelle zu bestimmten Schlüssel-Wert-Bereichen .NET Framework Konfiguration bereit. Dieser Konfigurations-Generator bietet **keine** hierarchischen Konfigurationen. Die JSON-Unterstützungs Datei ähnelt einem Wörterbuch, nicht einem komplexen hierarchischen Objekt. Eine hierarchische Datei mit mehreren Ebenen kann verwendet werden. Dieser Anbieter `flatten`die Tiefe, indem er den Eigenschaftsnamen auf jeder Ebene anfügt, wobei `:` als Trennzeichen verwendet wird.

Attribut Details:

* `jsonFile` – Erforderlich. Gibt die JSON-Datei an, aus der gelesen werden soll. Das `~` Zeichen kann am Anfang verwendet werden, um auf den Stamm der APP zu verweisen.
* `optional` boolescher Wert, der Standardwert ist `true`. Verhindert das Auslösen von Ausnahmen, wenn die JSON-Datei nicht gefunden werden kann.
* `jsonMode` - `[Flat|Sectional]`. `Flat` ist die Standardeinstellung. Wenn `jsonMode` `Flat`ist, ist die JSON-Datei eine einzelne flatkey/Wert-Quelle. Die `EnvironmentConfigBuilder` und `AzureKeyVaultConfigBuilder` sind auch einzelne flatkey-/Wert-Quellen. Wenn die `SimpleJsonConfigBuilder` im `Sectional` Modus konfiguriert ist:

  * Die JSON-Datei wird konzeptionell direkt auf der obersten Ebene in mehrere Wörterbücher aufgeteilt.
  * Jedes der Wörterbücher wird nur auf den Konfigurations Abschnitt angewendet, der mit dem an Sie angefügten Eigenschaftsnamen der obersten Ebene übereinstimmt. Beispiel:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementieren eines benutzerdefinierten Schlüssel-Wert-Konfigurations-Generators

Wenn die Konfigurations-Generatoren Ihren Anforderungen nicht entsprechen, können Sie ein benutzerdefiniertes schreiben. Die `KeyValueConfigBuilder` Basisklasse behandelt Ersetzungs Modi und die meisten Präfix Probleme. Ein Implementierungsprojekt benötigt nur:

* Erben Sie von der Basisklasse, und implementieren Sie mithilfe der `GetValue` und `GetAllValues`eine grundlegende Quelle von Schlüssel-Wert-Paaren:
* Fügen Sie [Microsoft. Configuration. configurationbuilder. Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) dem Projekt hinzu.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

Die `KeyValueConfigBuilder`-Basisklasse stellt einen Großteil der Arbeit und das konsistente Verhalten für Schlüssel-Wert-Konfigurations-Generatoren bereit.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [GitHub-Repository für Konfigurations-Generatoren](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Dienst-zu-Dienst-Authentifizierung für Azure Key Vault mithilfe von .net](/azure/key-vault/service-to-service-authentication#connection-string-support)
