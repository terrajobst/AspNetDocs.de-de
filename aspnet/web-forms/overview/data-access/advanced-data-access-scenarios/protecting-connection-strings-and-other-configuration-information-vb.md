---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Schützen von Verbindungs Zeichenfolgen und anderen Konfigurationsinformationen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Eine ASP.NET-Anwendung speichert in der Regel Konfigurationsinformationen in einer Web. config-Datei. Einige dieser Informationen sind sensibel und garantieren den Schutz. Nach def...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 070e1dccb80ef9af21ea621357c5b23e2ada6f9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78420999"
---
# <a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Schützen von Verbindungszeichenfolgen und anderen Konfigurationsinformationen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) oder [PDF herunterladen](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Eine ASP.NET-Anwendung speichert in der Regel Konfigurationsinformationen in einer Web. config-Datei. Einige dieser Informationen sind sensibel und garantieren den Schutz. Diese Datei wird standardmäßig nicht für einen Besucher der Website bereitgestellt, aber ein Administrator oder ein Hacker erhält möglicherweise Zugriff auf das Dateisystem des Webservers und zeigt den Inhalt der Datei an. In diesem Tutorial erfahren Sie, dass Sie mit ASP.NET 2,0 vertrauliche Informationen schützen können, indem Sie Abschnitte der Web. config-Datei verschlüsseln.

## <a name="introduction"></a>Einführung

Konfigurationsinformationen für ASP.NET-Anwendungen werden in der Regel in einer XML-Datei mit dem Namen `Web.config`gespeichert. Im Verlauf dieser Tutorials haben wir die `Web.config` mehrmals aktualisiert. Beim Erstellen des `Northwind` typisierten Datasets im [ersten](../introduction/creating-a-data-access-layer-vb.md)Lernprogramm werden z. b. die Informationen zur Verbindungs Zeichenfolge automatisch zu `Web.config` im Abschnitt `<connectionStrings>` hinzugefügt. Zu einem späteren Zeitpunkt haben wir im Lernprogramm zu [Master Seiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-vb.md) manuell `Web.config`aktualisiert und ein `<pages>` Element hinzugefügt, das angibt, dass alle ASP.NET-Seiten in unserem Projekt das `DataWebControls` Design verwenden sollten.

Da `Web.config` sensible Daten wie z. b. Verbindungs Zeichenfolgen enthalten kann, ist es wichtig, dass die Inhalte von `Web.config` sicher und vor nicht autorisierten Viewern verborgen bleiben. Standardmäßig wird jede HTTP-Anforderung an eine Datei mit der `.config`-Erweiterung von der ASP.net-Engine verarbeitet, die den in Abbildung 1 dargestellten *Meldungstyp "nicht* verarbeitet" zurückgibt. Dies bedeutet, dass Besucher die Inhalte Ihrer `Web.config` Dateien nicht anzeigen können, indem Sie einfach http://www.YourServer.com/Web.config in die Adressleiste Ihres Browsers eingeben.

[!["Web. config" über einen Browser zu besuchen, wird die Meldung "dieser Typ wird nicht verarbeitet" zurückgegeben.](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Abbildung 1**: der Besuch von `Web.config` über einen Browser gibt die Meldung diese Art von Seite wird nicht verarbeitet zurück ([Klicken Sie, um das Bild in voller Größe anzuzeigen](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))

Doch was geschieht, wenn ein Angreifer einen anderen Exploit finden kann, der es Ihnen ermöglicht, die Inhalte Ihrer `Web.config` Dateien anzuzeigen? Was könnte ein Angreifer mit diesen Informationen tun, und welche Schritte können durchgeführt werden, um die sensiblen Informationen in `Web.config`weiter zu schützen? Glücklicherweise enthalten die meisten Abschnitte in `Web.config` keine vertraulichen Informationen. Welche Schäden können Angreifer verursachen, wenn Sie den Namen des von Ihren ASP.net Pages verwendeten Standarddesigns kennen?

Bestimmte `Web.config` Abschnitte enthalten jedoch vertrauliche Informationen, die Verbindungs Zeichenfolgen, Benutzernamen, Kenn Wörter, Servernamen, Verschlüsselungsschlüssel usw. enthalten können. Diese Informationen sind in der Regel in den folgenden `Web.config` Abschnitten zu finden:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

In diesem Tutorial werden die Verfahren zum Schutz solcher sensibler Konfigurationsinformationen erläutert. Wie wir sehen werden, enthält die .NET Framework Version 2,0 ein geschütztes Konfigurationssystem, das das programmgesteuerte verschlüsseln und entschlüsseln ausgewählter Konfigurations Abschnitte zu einem Kinderspiel macht.

> [!NOTE]
> Dieses Tutorial schließt die Microsoft s-Empfehlungen für das Herstellen einer Verbindung mit einer Datenbank aus einer ASP.NET-Anwendung ab. Zusätzlich zur Verschlüsselung der Verbindungs Zeichenfolgen können Sie das System unterstützen, indem Sie sicherstellen, dass eine sichere Verbindung mit der Datenbank hergestellt wird.

## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Schritt 1: Untersuchen der geschützten Konfigurationsoptionen ASP.NET 2,0 s

ASP.NET 2,0 enthält ein geschütztes Konfigurationssystem zum Verschlüsseln und Entschlüsseln von Konfigurationsinformationen. Dies schließt Methoden in der .NET Framework ein, die verwendet werden können, um Konfigurationsinformationen Programm gesteuert zu verschlüsseln oder zu entschlüsseln. Das geschützte Konfigurationssystem verwendet das [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), das es Entwicklern ermöglicht, die verwendete kryptografieimplementierung auszuwählen.

Die .NET Framework wird mit zwei geschützten Konfigurations Anbietern ausgeliefert:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) : verwendet den asymmetrischen [RSA-Algorithmus](http://en.wikipedia.org/wiki/Rsa) für die Verschlüsselung und Entschlüsselung.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) : verwendet die Windows- [Datenschutz-API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) für die Verschlüsselung und Entschlüsselung.

Da das geschützte Konfigurationssystem das Provider-Entwurfsmuster implementiert, können Sie einen eigenen geschützten Konfigurations Anbieter erstellen und in Ihre Anwendung einbinden. Weitere Informationen zu diesem Prozess finden Sie unter [Implementieren eines geschützten Konfigurations Anbieters](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) .

Der RSA-und der DPAPI-Anbieter verwenden Schlüssel für Ihre Verschlüsselungs-und Entschlüsselungs Routinen. diese Schlüssel können auf der Computer-oder Benutzerebene gespeichert werden. Schlüssel auf Computer Ebene eignen sich ideal für Szenarien, in denen die Webanwendung auf einem eigenen dedizierten Server ausgeführt wird oder wenn mehrere Anwendungen auf einem Server vorhanden sind, die verschlüsselte Informationen freigeben müssen. Schlüssel auf Benutzerebene sind sicherer in freigegebenen Hostingumgebungen, in denen andere Anwendungen auf demselben Server nicht in der Lage sein sollen, die geschützten Konfigurations Abschnitte Ihrer Anwendung zu entschlüsseln.

In diesem Tutorial werden in unseren Beispielen der DPAPI-Anbieter und die Schlüssel auf Computer Ebene verwendet. Insbesondere wird die Verschlüsselung des `<connectionStrings>` Abschnitts in `Web.config`erläutert, obwohl das geschützte Konfigurationssystem zum Verschlüsseln der meisten `Web.config` Abschnitts verwendet werden kann. Weitere Informationen zur Verwendung von Schlüssel auf Benutzerebene oder zum Verwenden des RSA-Anbieters finden Sie im Abschnitt Weitere Messwerte am Ende dieses Tutorials.

> [!NOTE]
> Die `RSAProtectedConfigurationProvider`-und `DPAPIProtectedConfigurationProvider` Anbieter werden in der `machine.config`-Datei mit den Anbieter Namen `RsaProtectedConfigurationProvider` bzw. `DataProtectionConfigurationProvider`registriert. Beim Verschlüsseln oder Entschlüsseln von Konfigurationsinformationen müssen wir anstelle des tatsächlichen Typnamens (`RSAProtectedConfigurationProvider` und `DPAPIProtectedConfigurationProvider`) den entsprechenden Anbieter Namen (`RsaProtectedConfigurationProvider` oder `DataProtectionConfigurationProvider`) angeben. Sie finden die `machine.config` Datei im Ordner `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`.

## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Schritt 2: Programm gesteuertes verschlüsseln und Entschlüsseln von Konfigurations Abschnitten

Mit einigen wenigen Codezeilen können wir einen bestimmten Konfigurations Abschnitt mithilfe eines angegebenen Anbieters verschlüsseln oder entschlüsseln. Der Code, wie wir in Kürze sehen werden, muss nur Programm gesteuert auf den entsprechenden Konfigurations Abschnitt verweisen, seinen `ProtectSection` oder `UnprotectSection` Methode aufzurufen und dann die `Save`-Methode aufzurufen, um die Änderungen beizubehalten. Darüber hinaus enthält die .NET Framework ein nützliches Befehlszeilen-Hilfsprogramm, mit dem Konfigurationsinformationen verschlüsselt und entschlüsselt werden können. Wir werden dieses Befehlszeilen-Hilfsprogramm in Schritt 3 untersuchen.

Um den programmgesteuerten Schutz von Konfigurationsinformationen zu veranschaulichen, erstellen Sie eine ASP.NET-Seite, die Schaltflächen zum Verschlüsseln und Entschlüsseln des `<connectionStrings>` Abschnitts in `Web.config`enthält.

Öffnen Sie zunächst die Seite `EncryptingConfigSections.aspx` im Ordner `AdvancedDAL`. Ziehen Sie ein TextBox-Steuerelement aus der Toolbox auf den Designer, und legen Sie dessen `ID`-Eigenschaft auf `WebConfigContents`, seine `TextMode`-Eigenschaft auf `MultiLine`und dessen `Width`-und `Rows`-Eigenschaften auf 95% bzw. 15 fest. Dieses TextBox-Steuerelement zeigt den Inhalt `Web.config` an, mit dem wir schnell feststellen können, ob der Inhalt verschlüsselt ist oder nicht. Natürlich wäre es in einer realen Anwendung nie, den Inhalt `Web.config`anzuzeigen.

Fügen Sie unterhalb des Textfelds zwei Schaltflächen-Steuerelemente mit dem Namen `EncryptConnStrings` und `DecryptConnStrings`hinzu. Legen Sie die entsprechenden Text Eigenschaften fest, um Verbindungs Zeichenfolgen zu verschlüsseln und Verbindungs Zeichenfolgen

An diesem Punkt sollte der Bildschirm in etwa wie in Abbildung 2 aussehen.

[![der Seite ein Textfeld und zwei Schaltflächen-websteuer Elemente hinzufügen](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen eines Textfelds und der zwei Schaltflächen-websteuer Elemente zur Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))

Als nächstes müssen Sie Code schreiben, der beim ersten Laden der Seite den Inhalt der `Web.config` im Textfeld `WebConfigContents` lädt und anzeigt. Fügen Sie den folgenden Code zur Code Behind-Klasse der Page s hinzu. Mit diesem Code wird eine Methode mit dem Namen `DisplayWebConfig` hinzugefügt und vom `Page_Load`-Ereignishandler aufgerufen, wenn `Page.IsPostBack` `False`ist:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

Die `DisplayWebConfig`-Methode verwendet die [`File`-Klasse](https://msdn.microsoft.com/library/system.io.file.aspx) , um die `Web.config` Datei der Anwendung zu öffnen, die`StreamReader`- [Klasse](https://msdn.microsoft.com/library/system.io.streamreader.aspx) , um ihren Inhalt in eine Zeichenfolge zu lesen, und die`Path`- [Klasse](https://msdn.microsoft.com/library/system.io.path.aspx) , um den physischen Pfad zur `Web.config` Datei zu generieren. Diese drei Klassen befinden sich alle in der [`System.IO`-Namespace](https://msdn.microsoft.com/library/system.io.aspx). Folglich müssen Sie am Anfang der Code-Behind-Klasse eine `Imports``System.IO`-Anweisung hinzufügen oder diese Klassennamen als Präfix hinzufügen `System.IO.`

Als nächstes müssen wir Ereignishandler für die beiden Schaltflächen-Steuerelemente `Click` Ereignissen hinzufügen und den erforderlichen Code hinzufügen, um den `<connectionStrings>` Abschnitt mithilfe eines Schlüssels auf Computer Ebene mit dem DPAPI-Anbieter zu verschlüsseln und zu entschlüsseln. Doppelklicken Sie im Designer auf die einzelnen Schaltflächen, um einen `Click` Ereignishandler in der Code Behind-Klasse hinzuzufügen, und fügen Sie dann den folgenden Code hinzu:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Der Code, der in den beiden Ereignis Handlern verwendet wird, ist nahezu identisch. Beide werden zunächst über die [`WebConfigurationManager` Class](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [`OpenWebConfiguration`-Methode](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)Informationen über die aktuelle Anwendung `Web.config` Datei erhalten. Diese Methode gibt die Webkonfigurationsdatei für den angegebenen virtuellen Pfad zurück. Im nächsten Schritt wird auf den Abschnitt `Web.config` Datei s `<connectionStrings>` über die`GetSection(sectionName)`- [Methode](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)der [`Configuration`-Klasse](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) zugegriffen, die ein [`ConfigurationSection`](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) Objekt zurückgibt.

Das `ConfigurationSection`-Objekt enthält eine [`SectionInformation`-Eigenschaft](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , die zusätzliche Informationen und Funktionen im Zusammenhang mit dem Konfigurations Abschnitt bereitstellt. Wie der obige Code zeigt, können wir bestimmen, ob der Konfigurations Abschnitt verschlüsselt ist, indem wir die `SectionInformation` Eigenschaft s `IsProtected` Eigenschaft überprüfen. Darüber hinaus kann der Abschnitt über die `SectionInformation` Eigenschaft s `ProtectSection(provider)` und `UnprotectSection` Methoden verschlüsselt oder entschlüsselt werden.

Die `ProtectSection(provider)`-Methode akzeptiert als Eingabe eine Zeichenfolge, die den Namen des für die Verschlüsselung zu verwendenden geschützten Konfigurations Anbieters angibt. Im `EncryptConnString` Button s-Ereignishandler übergeben wir dataschutzconfigurationprovider an die `ProtectSection(provider)`-Methode, sodass der DPAPI-Anbieter verwendet wird. Mit der `UnprotectSection`-Methode kann der Anbieter bestimmt werden, der zum Verschlüsseln des Konfigurations Abschnitts verwendet wurde. Daher sind keine Eingabeparameter erforderlich.

Nachdem Sie die `ProtectSection(provider)`-oder `UnprotectSection`-Methode aufgerufen haben, müssen Sie das `Configuration`-Objekt s [`Save`-Methode](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) aufrufen, um die Änderungen beizubehalten. Nachdem die Konfigurationsinformationen verschlüsselt oder entschlüsselt wurden und die Änderungen gespeichert wurden, wird `DisplayWebConfig` aufgerufen, um den aktualisierten `Web.config` Inhalt in das TextBox-Steuerelement zu laden.

Nachdem Sie den obigen Code eingegeben haben, testen Sie ihn, indem Sie die `EncryptingConfigSections.aspx` Seite über einen Browser aufrufen. Anfänglich wird eine Seite angezeigt, auf der der Inhalt `Web.config` aufgeführt ist, wobei der `<connectionStrings>` Abschnitt im Klartext angezeigt wird (siehe Abbildung 3).

[![der Seite ein Textfeld und zwei Schaltflächen-websteuer Elemente hinzufügen](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen eines Textfelds und der zwei Schaltflächen-websteuer Elemente zur Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))

Klicken Sie jetzt auf die Schaltfläche Verbindungs Zeichenfolgen verschlüsseln Wenn die Anforderungs Validierung aktiviert ist, erzeugt das von dem `WebConfigContents` Textfeld zurück gestellte Markup eine `HttpRequestValidationException`, die die Meldung anzeigt, dass ein potenziell gefährlicher `Request.Form` Wert vom Client erkannt wurde. Die Anforderungs Überprüfung, die standardmäßig in ASP.NET 2,0 aktiviert ist, verbietet Postbacks, die nicht codiertes HTML enthalten, und ist darauf ausgelegt, Skript einschleusungs Angriffe zu verhindern. Diese Überprüfung kann auf Seiten-oder Anwendungsebene deaktiviert werden. Um diese Seite zu deaktivieren, legen Sie die `ValidateRequest` Einstellung in der `@Page`-Direktive auf `False` fest. Die `@Page`-Direktive befindet sich am oberen Rand des deklarativen Markups der Seite.

[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Weitere Informationen zur Anforderungs Validierung, zu deren Zweck, zur Deaktivierung auf Seiten-und Anwendungsebene sowie zum HTML-codieren von Markup finden Sie unter [Anforderungs Validierung-verhindern von Skript Angriffen](../../../../whitepapers/request-validation.md).

Nachdem Sie die Anforderungs Validierung für die Seite deaktiviert haben, klicken Sie erneut auf die Schaltfläche Verbindungs Zeichenfolgen verschlüsseln. Beim Postback wird auf die Konfigurationsdatei zugegriffen, und der `<connectionStrings>` Abschnitt wird mithilfe des DPAPI-Anbieters verschlüsselt. Das Textfeld wird dann aktualisiert, um den neuen `Web.config` Inhalt anzuzeigen. Wie in Abbildung 4 gezeigt, werden die `<connectionStrings>` Informationen jetzt verschlüsselt.

[![klicken auf die Schaltfläche Verbindungs Zeichenfolgen verschlüsseln wird der Abschnitt &lt;ConnectionString&gt; verschlüsselt.](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Abbildung 4**: durch Klicken auf die Schaltfläche Verbindungs Zeichenfolgen verschlüsseln wird der `<connectionString>` Abschnitt verschlüsselt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))

Der auf meinem computergenerierte verschlüsselte `<connectionStrings>` Abschnitt folgt, obwohl der Inhalt des `<CipherData>`-Elements aus Gründen der Kürze entfernt wurde:

[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> Das `<connectionStrings>`-Element gibt den Anbieter an, der zum Ausführen der Verschlüsselung (`DataProtectionConfigurationProvider`) verwendet wird. Diese Informationen werden von der `UnprotectSection`-Methode verwendet, wenn auf die Schaltfläche zum Entschlüsseln der Verbindungs Zeichenfolgen geklickt wird.

Wenn der Zugriff auf die Verbindungs Zeichenfolgen-Informationen von `Web.config` erfolgt, entweder durch den Code, den wir schreiben, von einem SqlDataSource-Steuerelement oder durch den automatisch generierten Code aus den TableAdapters in den typisierten Datasets, wird er automatisch entschlüsselt. Kurz gesagt, es ist nicht erforderlich, zusätzlichen Code oder Logik hinzuzufügen, um den verschlüsselten `<connectionString>` Abschnitt zu entschlüsseln. Um dies zu veranschaulichen, besuchen Sie zu diesem Zeitpunkt eines der früheren Tutorials, z.b. das Lernprogramm "einfache Anzeige" aus dem Abschnitt "grundlegende Berichterstellung" (`~/BasicReporting/SimpleDisplay.aspx`). Wie in Abbildung 5 zu sehen ist, funktioniert das Tutorial genau so, wie es erwartet wird, und gibt an, dass die Informationen zur verschlüsselten Verbindungs Zeichenfolge automatisch von der ASP.NET-Seite entschlüsselt werden.

[![die Datenzugriffs Ebene die Informationen zur Verbindungs Zeichenfolge automatisch entschlüsselt.](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Abbildung 5**: die Datenzugriffs Ebene entschlüsselt automatisch die Informationen der Verbindungs Zeichenfolge ([Klicken Sie, um das Bild in voller Größe anzuzeigen](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))

Wenn Sie den `<connectionStrings>` Abschnitt wieder in seine Klartext-Darstellung zurücksetzen möchten, klicken Sie auf die Schaltfläche Verbindungs Zeichenfolgen entschlüsseln. Beim Postback sollten die Verbindungs Zeichenfolgen in `Web.config` in Klartext angezeigt werden. An diesem Punkt sollte der Bildschirm wie beim ersten Besuch dieser Seite aussehen (siehe Abbildung 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnet_regiisexe"></a>Schritt 3: Verschlüsseln von Konfigurations Abschnitten mithilfe von`aspnet_regiis.exe`

Die .NET Framework umfasst eine Reihe von Befehlszeilen Tools im Ordner `$WINDOWS$\Microsoft.NET\Framework\version\`. Im Tutorial [Verwenden von SQL-Cache Abhängigkeiten](../caching-data/using-sql-cache-dependencies-vb.md) haben wir uns beispielsweise mit der Verwendung des Befehlszeilen Tools `aspnet_regsql.exe` beschäftigt, um die erforderliche Infrastruktur für SQL-Cache Abhängigkeiten hinzuzufügen. Ein weiteres nützliches Befehlszeilen Tool in diesem Ordner ist das [ASP.NET IIS-Registrierungs Tool (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Wie der Name schon sagt, wird das ASP.NET IIS-Registrierungs Tool hauptsächlich verwendet, um eine ASP.NET 2,0-Anwendung bei Microsoft s Professional-Grade-Webserver (IIS) zu registrieren. Zusätzlich zu den IIS-bezogenen Features kann das ASP.NET IIS-Registrierungs Tool auch verwendet werden, um bestimmte Konfigurations Abschnitte in `Web.config`zu verschlüsseln oder zu entschlüsseln.

Die folgende Anweisung zeigt die allgemeine Syntax, die zum Verschlüsseln eines Konfigurations Abschnitts mit dem `aspnet_regiis.exe`-Befehlszeilen Tool verwendet wird:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

der *Abschnitt* ist der zu verschlüsselnde Konfigurations Abschnitt (z. b. ConnectionStrings), das *physische\_Verzeichnis* ist der vollständige physische Pfad zum Stammverzeichnis der Webanwendung, und *Provider* ist der Name des zu verwendenden geschützten Konfigurations Anbieters (z. b. dataschutzconfigurationprovider). Wenn die Webanwendung in IIS registriert ist, können Sie alternativ den virtuellen Pfad anstelle des physischen Pfads mit der folgenden Syntax eingeben:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Im folgenden `aspnet_regiis.exe` Beispiel wird der `<connectionStrings>` Abschnitt mit dem DPAPI-Anbieter mit einem Schlüssel auf Computer Ebene verschlüsselt:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Ebenso kann das Befehlszeilen Tool `aspnet_regiis.exe` zum Entschlüsseln von Konfigurations Abschnitten verwendet werden. Verwenden Sie anstelle des `-pef` Schalters `-pdf` (oder anstelle von `-pe``-pd`). Beachten Sie auch, dass der Anbieter Name beim Entschlüsseln nicht erforderlich ist.

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Da wir den DPAPI-Anbieter verwenden, der für den Computer spezifische Schlüssel verwendet, müssen Sie `aspnet_regiis.exe` vom gleichen Computer ausführen, von dem die Webseiten bedient werden. Wenn Sie z. b. dieses Befehlszeilenprogramm von Ihrem lokalen Entwicklungs Computer ausführen und dann die verschlüsselte Datei "Web. config" auf den Produktionsserver hochladen, kann der Produktionsserver die Informationen zur Verbindungs Zeichenfolge nicht entschlüsseln, da er verschlüsselt wurde. Verwendung von Schlüsseln, die für ihren Entwicklungs Computer spezifisch sind. Der RSA-Anbieter hat diese Einschränkung nicht, da es möglich ist, die RSA-Schlüssel auf einen anderen Computer zu exportieren.

## <a name="understanding-database-authentication-options"></a>Grundlegendes zu Daten Bank Authentifizierungs Optionen

Bevor eine Anwendung `SELECT`-, `INSERT`-, `UPDATE`-oder `DELETE`-Abfragen an eine Microsoft SQL Server Datenbank ausgeben kann, muss die Datenbank zuerst den Anforderer identifizieren. Dieser Prozess wird als *Authentifizierung* bezeichnet und SQL Server zwei Authentifizierungsmethoden bereitstellen:

- **Windows-Authentifizierung** : der Prozess, unter dem die Anwendung ausgeführt wird, wird für die Kommunikation mit der Datenbank verwendet. Wenn eine ASP.NET-Anwendung über Visual Studio 2005 s ASP.NET Development Server ausgeführt wird, nimmt die ASP.NET-Anwendung die Identität des aktuell angemeldeten Benutzers an. Bei ASP.NET-Anwendungen auf Microsoft Internet Information Server (IIS) wird für ASP.NET-Anwendungen in der Regel die Identität von `domainName``\MachineName` oder `domainName``\NETWORK SERVICE`angenommen, obwohl dies angepasst werden kann.
- **SQL-Authentifizierung** : eine Benutzer-ID und Kenn Wort Werte werden als Anmelde Informationen für die Authentifizierung bereitgestellt. Bei der SQL-Authentifizierung werden die Benutzer-ID und das Kennwort in der Verbindungs Zeichenfolge bereitgestellt.

Die Windows-Authentifizierung wird vor der SQL-Authentifizierung bevorzugt, da Sie sicherer ist. Bei der Windows-Authentifizierung ist die Verbindungs Zeichenfolge von einem Benutzernamen und Kennwort frei, und wenn sich der Webserver und der Datenbankserver auf zwei verschiedenen Computern befinden, werden die Anmelde Informationen nicht im Klartext über das Netzwerk gesendet. Bei der SQL-Authentifizierung werden die Anmelde Informationen für die Authentifizierung jedoch in der Verbindungs Zeichenfolge hart codiert und vom Webserver als Klartext an den Datenbankserver übertragen.

In diesen Tutorials wurde die Windows-Authentifizierung verwendet. Mithilfe der Verbindungs Zeichenfolge können Sie erkennen, welcher Authentifizierungsmodus verwendet wird. Die Verbindungs Zeichenfolge in `Web.config` für unsere Tutorials lautet:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Die integrierte Sicherheit = true und der Mangel an Benutzername und Kennwort geben an, dass die Windows-Authentifizierung verwendet wird. In manchen Verbindungs Zeichenfolgen wird der Begriff vertrauenswürdige Verbindung = yes oder Integrated Security = SSPI anstelle von Integrated Security = true verwendet. alle drei weisen auf die Verwendung der Windows-Authentifizierung hin.

Das folgende Beispiel zeigt eine Verbindungs Zeichenfolge, die die SQL-Authentifizierung verwendet. Notieren Sie sich die in der Verbindungs Zeichenfolge eingebetteten Anmelde Informationen:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Stellen Sie sich vor, dass ein Angreifer die `Web.config` Datei Ihrer Anwendung anzeigen kann. Wenn Sie die SQL-Authentifizierung verwenden, um eine Verbindung mit einer Datenbank herzustellen, auf die über das Internet zugegriffen werden kann, kann der Angreifer diese Verbindungs Zeichenfolge zum Herstellen einer Verbindung mit Ihrer Datenbank über SQL Management Studio oder von ASP.NET Seiten auf der eigenen Website verwenden. Um diese Bedrohung zu mindern, verschlüsseln Sie die Verbindungs Zeichenfolgen-Informationen in `Web.config` mithilfe des geschützten Konfigurations Systems.

> [!NOTE]
> Weitere Informationen zu den verschiedenen Arten der Authentifizierung, die in SQL Server verfügbar sind, finden Sie unter [Building Secure ASP.NET Applications: Authentication, Authorization, and Secure Communication](https://msdn.microsoft.com/library/aa302392.aspx). Weitere Beispiele für Verbindungs Zeichenfolgen, die die Unterschiede zwischen Windows-und SQL-Authentifizierungs Syntax veranschaulichen, finden Sie unter [connectionStrings.com](http://www.connectionstrings.com/).

## <a name="summary"></a>Zusammenfassung

Standardmäßig kann auf Dateien mit einer `.config`-Erweiterung in einer ASP.NET-Anwendung nicht über einen Browser zugegriffen werden. Diese Dateitypen werden nicht zurückgegeben, da Sie möglicherweise vertrauliche Informationen enthalten, wie z. b. Daten bankverbindungs Zeichenfolgen, Benutzernamen und Kenn Wörter, usw. Das geschützte Konfigurationssystem in .NET 2,0 unterstützt den weiteren Schutz sensibler Informationen, da bestimmte Konfigurations Abschnitte verschlüsselt werden können. Es gibt zwei integrierte geschützte Konfigurations Anbieter: einen, der den RSA-Algorithmus verwendet, und einen, der die Windows-Datenschutz-API (DPAPI) verwendet.

In diesem Tutorial wurde beschrieben, wie Konfigurationseinstellungen mithilfe des DPAPI-Anbieters verschlüsselt und entschlüsselt werden. Dies kann sowohl Programm gesteuert, wie in Schritt 2 beschrieben, als auch über das Befehlszeilen Tool `aspnet_regiis.exe` erfolgen, das in Schritt 3 beschrieben wurde. Weitere Informationen zur Verwendung von Schlüssel auf Benutzerebene oder zum Verwenden des RSA-Anbieters finden Sie in den Ressourcen im Abschnitt Weitere Informationen.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Aufbauen einer sicheren ASP.NET-Anwendung: Authentifizierung, Autorisierung und sichere Kommunikation](https://msdn.microsoft.com/library/aa302392.aspx)
- [Verschlüsseln von Konfigurationsinformationen in ASP.NET 2,0-Anwendungen](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Verschlüsseln von `Web.config` Werten in ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Gewusst wie: Verschlüsseln von Konfigurations Abschnitten in ASP.NET 2,0 mithilfe von DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Gewusst wie: Verschlüsseln von Konfigurations Abschnitten in ASP.NET 2,0 mit RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Die Konfigurations-API in .NET 2,0](http://www.odetocode.com/Articles/418.aspx)
- [Windows-Datenschutz](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Teresa Murphy und Randy Schmidt. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Weiter](debugging-stored-procedures-vb.md)
