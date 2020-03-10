---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profile, Designs und Webparts | Microsoft-Dokumentation
author: microsoft
description: In ASP.NET 2,0 sind wesentliche Änderungen an der Konfiguration und Instrumentation vorhanden. Die neue ASP.NET-Konfigurations-API ermöglicht die Änderung von Konfigurationsänderungen...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: cf5c45781be6d003d28c6aa27efa08032579a6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474639"
---
# <a name="profiles-themes-and-web-parts"></a>Profile, Designs und Webparts

von [Microsoft](https://github.com/microsoft)

> In ASP.NET 2,0 sind wesentliche Änderungen an der Konfiguration und Instrumentation vorhanden. Die neue ASP.NET-Konfigurations-API ermöglicht die programmgesteuerte Konfiguration von Konfigurationsänderungen. Außerdem sind viele neue Konfigurationseinstellungen für neue Konfigurationen und Instrumentation vorhanden.

ASP.NET 2,0 stellt eine beträchtliche Verbesserung im Bereich personalisierter Websites dar. Zusätzlich zu den bereits behandelten Mitgliedschafts Features haben ASP.NET Profile, Designs und Webparts eine deutliche Verbesserung der Personalisierung auf Websites.

## <a name="aspnet-profiles"></a>ASP.net profile

ASP.NET-Profile ähneln Sitzungen. Der Unterschied besteht darin, dass ein Profil persistent ist, während eine Sitzung verloren geht, wenn der Browser geschlossen wird. Ein weiterer großer Unterschied zwischen Sitzungen und Profilen besteht darin, dass Profile stark typisiert sind und damit IntelliSense während des Entwicklungsprozesses bereitgestellt werden.

Ein Profil wird entweder in der Computer Konfigurationsdatei oder in der Datei "Web. config" für die Anwendung definiert. (Sie können kein Profil in einer Datei "Web. config" Unterordner definieren.) Der folgende Code definiert ein Profil zum Speichern der Website Besucher vor-und Nachname.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Der Standard Datentyp für eine Profil Eigenschaft ist System. String. Im obigen Beispiel wurde kein Datentyp angegeben. Daher sind die Eigenschaften "FirstName" und "LastName" beide vom Typ "String". Wie bereits erwähnt, sind Profil Eigenschaften stark typisiert. Der folgende Code fügt eine neue Eigenschaft für das Alter mit dem Typ "Int32" hinzu.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profile werden im Allgemeinen mit der ASP.NET-Formular Authentifizierung verwendet. Bei Verwendung in Verbindung mit der Formular Authentifizierung verfügt jeder Benutzer über ein separates Profil, das mit der Benutzer-ID verknüpft ist. Allerdings ist es auch möglich, die Verwendung von Profilen in einer anonymen Anwendung mithilfe des &lt;anonymousidentifitifi&gt;-Elements in der Konfigurationsdatei zu ermöglichen, zusammen mit dem **zugewiesenen** Attribut, wie unten dargestellt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Wenn ein anonymer Benutzer die Website durchsucht, erstellt ASP.net eine Instanz von " **ProfileCommon** " für den Benutzer. Dieses Profil verwendet eine eindeutige ID, die in einem Cookie im Browser gespeichert ist, um den Benutzer als eindeutigen Besucher zu identifizieren. Auf diese Weise können Sie Profilinformationen für Benutzer speichern, die anonym durchsuchen.

## <a name="profile-groups"></a>Profil Gruppen

Es ist möglich, die Eigenschaften von Profilen zu gruppieren. Durch das Gruppieren von Eigenschaften ist es möglich, mehrere Profile für eine bestimmte Anwendung zu simulieren.

Die folgende Konfiguration konfiguriert eine FirstName-und LastName-Eigenschaft für zwei Gruppen. Käufer und Chancen.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Es ist dann möglich, Eigenschaften für eine bestimmte Gruppe wie folgt festzulegen:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Speichern komplexer Objekte

Bisher haben die von uns behandelten Beispiele einfache Datentypen in einem Profil gespeichert. Es ist auch möglich, komplexe Datentypen in einem Profil zu speichern, indem Sie die Serialisierungsmethode wie folgt mithilfe des Attributs **serializeAs** angeben:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In diesem Fall ist der Typ purchaseinvoice. Die purchaseingevoice-Klasse muss als serialisierbar gekennzeichnet werden und kann eine beliebige Anzahl von Eigenschaften enthalten. Wenn purchaseingevoice beispielsweise eine Eigenschaft namens **numitemspurgejagt**hat, können Sie diese Eigenschaft im Code wie folgt aufrufen:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil Vererbung

Es ist möglich, ein Profil für die Verwendung in mehreren Anwendungen zu erstellen. Indem Sie eine Profilklasse erstellen, die von ProfileBase abgeleitet ist, können Sie ein Profil in mehreren Anwendungen wieder verwenden, indem Sie das **erbt** -Attribut wie unten gezeigt verwenden:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In diesem Fall sieht die Klasse " **purchasingprofile** " wie folgt aus:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Profil Anbieter

ASP.NET Profile verwenden das Anbieter Modell. Der Standardanbieter speichert die Informationen in einer SQL Server Express Datenbank im App-\_Datenordner der Webanwendung, die den SqlProfileProvider-Anbieter verwendet. Wenn die Datenbank nicht vorhanden ist, wird Sie von ASP.NET automatisch erstellt, wenn das Profil versucht, Informationen zu speichern.

In einigen Fällen möchten Sie jedoch möglicherweise einen eigenen Profil Anbieter entwickeln. Mit der ASP.NET Profile-Funktion können Sie problemlos verschiedene Anbieter verwenden.

In folgenden Aktionen erstellen Sie einen benutzerdefinierten Profil Anbieter:

- Sie müssen Profilinformationen in einer Datenquelle speichern, z. b. in einer FoxPro-Datenbank oder in einer Oracle-Datenbank, die von den in der .NET Framework enthaltenen Profil Anbietern nicht unterstützt wird.
- Sie müssen Profilinformationen mithilfe eines Datenbankschemas verwalten, das sich von dem Datenbankschema unterscheidet, das von den in der .NET Framework enthaltenen Anbietern verwendet wird. Ein gängiges Beispiel ist, dass Sie Profilinformationen in eine vorhandene SQL Server Datenbank mit Benutzerdaten integrieren möchten.

### <a name="required-classes"></a>Erforderliche Klassen

Um einen Profil Anbieter zu implementieren, erstellen Sie eine Klasse, die die abstrakte Klasse System. Web. profile. ProfileProvider erbt. Die abstrakte Klasse **ProfileProvider** erbt wiederum die abstrakte System. Configuration. SettingsProvider-Klasse, die die abstrakte Klasse System. Configuration. Provider. ProviderBase erbt. Aufgrund dieser Vererbungs Kette müssen Sie zusätzlich zu den erforderlichen Elementen der **ProfileProvider** -Klasse die erforderlichen Member der **SettingsProvider** -Klasse und der **ProviderBase** -Klasse implementieren.

In den folgenden Tabellen werden die Eigenschaften und Methoden beschrieben, die Sie in den abstrakten Klassen **ProviderBase**, **SettingsProvider**und **ProfileProvider** implementieren müssen.

### <a name="providerbase-members"></a>ProviderBase-Member

| **Member** | **Beschreibung** |
| --- | --- |
| Initialize-Methode | Übernimmt als Eingabe den Namen der Anbieter Instanz und eine NameValueCollection der Konfigurationseinstellungen. Wird verwendet, um Optionen und Eigenschaftswerte für die Anbieter Instanz festzulegen, einschließlich der Implementierungs spezifischen Werte und Optionen, die in der Computerkonfiguration oder der Datei "Web. config" angegeben sind. |

### <a name="settingsprovider-members"></a>SettingsProvider-Member

| **Member** | **Beschreibung** |
| --- | --- |
| ApplicationName-Eigenschaft | Der Anwendungsname, der mit jedem Profil gespeichert wird. Der Profil Anbieter verwendet den Anwendungsnamen, um Profilinformationen für jede Anwendung separat zu speichern. Dadurch können mehrere ASP.NET-Anwendungen dieselbe Datenquelle verwenden, ohne dass ein Konflikt vorliegt, wenn derselbe Benutzername in verschiedenen Anwendungen erstellt wird. Alternativ können mehrere ASP.NET-Anwendungen eine Profildaten Quelle gemeinsam nutzen, indem Sie denselben Anwendungsnamen angeben. |
| GetPropertyValues-Methode | Nimmt als Eingabe einen SettingsContext und ein SettingsPropertyCollection-Objekt an. Der **SettingsContext** stellt Informationen über den Benutzer bereit. Sie können die Informationen als Primärschlüssel verwenden, um Profil Eigenschafts Informationen für den Benutzer abzurufen. Verwenden Sie das **SettingsContext** -Objekt, um den Benutzernamen zu erhalten und zu übernehmen, ob der Benutzer authentifiziert oder anonym ist. **SettingsPropertyCollection** enthält eine Auflistung von SettingsProperty-Objekten. Jedes **SettingsProperty** -Objekt stellt den Namen und den Typ der Eigenschaft sowie weitere Informationen bereit, z. b. den Standardwert für die Eigenschaft und ob die Eigenschaft schreibgeschützt ist. Die **GetPropertyValues** -Methode füllt eine SettingsPropertyValueCollection mit SettingsPropertyValue-Objekten auf der Grundlage der **SettingsProperty** -Objekte auf, die als Eingabe bereitgestellt werden. Die Werte aus der Datenquelle für den angegebenen Benutzer werden den PropertyValue-Eigenschaften für jedes **SettingsPropertyValue** -Objekt zugewiesen, und die gesamte Auflistung wird zurückgegeben. Durch den Aufruf der-Methode wird auch der LastActivityDate-Wert für das angegebene Benutzerprofil auf das aktuelle Datum und die aktuelle Uhrzeit aktualisiert. |
| SetPropertyValues-Methode | Nimmt als Eingabe einen **SettingsContext** und ein **SettingsPropertyValueCollection** -Objekt an. Der **SettingsContext** stellt Informationen über den Benutzer bereit. Sie können die Informationen als Primärschlüssel verwenden, um Profil Eigenschafts Informationen für den Benutzer abzurufen. Verwenden Sie das **SettingsContext** -Objekt, um den Benutzernamen zu erhalten und zu übernehmen, ob der Benutzer authentifiziert oder anonym ist. **SettingsPropertyValueCollection** enthält eine Auflistung von **SettingsPropertyValue** -Objekten. Jedes **SettingsPropertyValue** -Objekt stellt den Namen, den Typ und den Wert der Eigenschaft sowie weitere Informationen bereit, z. b. den Standardwert für die Eigenschaft und ob die Eigenschaft schreibgeschützt ist. Die **SetPropertyValues** -Methode aktualisiert die Profil Eigenschaftswerte in der Datenquelle für den angegebenen Benutzer. Durch den Aufruf der-Methode werden auch die Werte **LastActivityDate** und LastUpdatedDate für das angegebene Benutzerprofil auf das aktuelle Datum und die aktuelle Uhrzeit aktualisiert. |

### <a name="profileprovider-members"></a>ProfileProvider-Member

| **Member** | **Beschreibung** |
| --- | --- |
| DeleteProfiles-Methode | Übernimmt als Eingabe ein Zeichen folgen Array mit Benutzernamen und Lösch Vorgängen aus der Datenquelle alle Profilinformationen und Eigenschaftswerte für die angegebenen Namen, wobei der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Wenn Ihre Datenquelle Transaktionen unterstützt, sollten Sie alle Löschvorgänge in eine Transaktion einschließen und ein Rollback der Transaktion ausführen und eine Ausnahme auslösen, wenn ein Löschvorgang fehlschlägt. |
| DeleteProfiles-Methode | Nimmt als Eingabe eine Auflistung von ProfileInfo-Objekten an und löscht aus der Datenquelle alle Profilinformationen und Eigenschaftswerte für jedes Profil, bei dem der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Wenn Ihre Datenquelle Transaktionen unterstützt, sollten Sie alle Löschvorgänge in eine Transaktion einschließen und ein Rollback für die Transaktion ausführen und eine Ausnahme auslösen, wenn ein Löschvorgang fehlschlägt. |
| Delta einactiveprofiles-Methode | Nimmt als Eingabe einen ProfileAuthenticationOption-Wert und ein DateTime-Objekt an und löscht aus der Datenquelle alle Profilinformationen und Eigenschaftswerte, bei denen das Datum der letzten Aktivität kleiner oder gleich dem angegebenen Datum und der angegebenen Uhrzeit ist und der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Der Parameter " **ProfileAuthenticationOption** " gibt an, ob nur anonyme Profile, nur authentifizierte Profile oder alle Profile gelöscht werden sollen. Wenn Ihre Datenquelle Transaktionen unterstützt, sollten Sie alle Löschvorgänge in eine Transaktion einschließen und ein Rollback für die Transaktion ausführen und eine Ausnahme auslösen, wenn ein Löschvorgang fehlschlägt. |
| GetAllProfiles-Methode | Verwendet als Eingabe einen Wert vom Typ " **ProfileAuthenticationOption** ", eine ganze Zahl, die den Seitenindex angibt, eine ganze Zahl, die die Seitengröße angibt, und einen Verweis auf eine Ganzzahl, die auf die Gesamtzahl der Profile festgelegt wird. Gibt eine ProfileInfoCollection zurück, die **ProfileInfo** -Objekte für alle Profile in der Datenquelle enthält, wobei der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Der Parameter " **ProfileAuthenticationOption** " gibt an, ob nur anonyme Profile, nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Die von der **GetAllProfiles** -Methode zurückgegebenen Ergebnisse werden durch die Werte für den Seitenindex und die Seitengröße eingeschränkt. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** -Objekten an, die in der **ProfileInfoCollection**zurückgegeben werden. Der Seiten Indexwert gibt an, welche Ergebnisseite zurückgegeben werden soll, wobei 1 die erste Seite identifiziert. Der-Parameter für die Gesamtanzahl der Datensätze ist ein out-Parameter (Sie können **ByRef** in Visual Basic verwenden), der auf die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher z. b. 13 Profile für die Anwendung enthält und der Seitenindex Wert 2 mit einer Seitengröße von 5 ist, enthält die zurückgegebene **ProfileInfoCollection** das sechste bis zehnte Profil. Der Total Records-Wert wird auf 13 festgelegt, wenn die-Methode zurückgibt. |
| GetAllInactiveProfiles-Methode | Verwendet als Eingabe einen **ProfileAuthenticationOption** -Wert, ein **DateTime** -Objekt, eine ganze Zahl, die den Seitenindex angibt, eine Ganzzahl, die die Seitengröße angibt, und einen Verweis auf eine Ganzzahl, die auf die Gesamtzahl der Profile festgelegt wird. Gibt eine **ProfileInfoCollection** zurück, die **ProfileInfo** -Objekte für alle Profile in der Datenquelle enthält, bei denen das Datum der letzten Aktivität kleiner oder gleich dem angegebenen **DateTime** -Wert ist und der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Der Parameter " **ProfileAuthenticationOption** " gibt an, ob nur anonyme Profile, nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Die Ergebnisse, die von der **GetAllInactiveProfiles** -Methode zurückgegeben werden, werden durch die Werte für den Seitenindex und die Seitengröße eingeschränkt. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** -Objekten an, die in der **ProfileInfoCollection**zurückgegeben werden. Der Seiten Indexwert gibt an, welche Ergebnisseite zurückgegeben werden soll, wobei 1 die erste Seite identifiziert. Der-Parameter für die Gesamtanzahl der Datensätze ist ein out-Parameter (Sie können **ByRef** in Visual Basic verwenden), der auf die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher z. b. 13 Profile für die Anwendung enthält und der Seitenindex Wert 2 mit einer Seitengröße von 5 ist, enthält die zurückgegebene **ProfileInfoCollection** das sechste bis zehnte Profil. Der Total Records-Wert wird auf 13 festgelegt, wenn die-Methode zurückgibt. |
| FindProfilesByUserName-Methode | Verwendet als Eingabe einen **ProfileAuthenticationOption** -Wert, eine Zeichenfolge mit einem Benutzernamen, eine ganze Zahl, die den Seitenindex angibt, eine Ganzzahl, die die Seitengröße angibt, und einen Verweis auf eine Ganzzahl, die auf die Gesamtzahl der Profile festgelegt wird. Gibt eine **ProfileInfoCollection** zurück, die **ProfileInfo** -Objekte für alle Profile in der Datenquelle enthält, wobei der Benutzername mit dem angegebenen Benutzernamen übereinstimmt und der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Der Parameter " **ProfileAuthenticationOption** " gibt an, ob nur anonyme Profile, nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Wenn Ihre Datenquelle zusätzliche Suchfunktionen unterstützt, wie z. b. Platzhalter Zeichen, können Sie ausführlichere Suchfunktionen für Benutzernamen bereitstellen. Die von der **FindProfilesByUserName** -Methode zurückgegebenen Ergebnisse werden durch die Werte für den Seitenindex und die Seitengröße eingeschränkt. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** -Objekten an, die in der **ProfileInfoCollection**zurückgegeben werden. Der Seiten Indexwert gibt an, welche Ergebnisseite zurückgegeben werden soll, wobei 1 die erste Seite identifiziert. Der-Parameter für die Gesamtanzahl der Datensätze ist ein out-Parameter (Sie können **ByRef** in Visual Basic verwenden), der auf die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher z. b. 13 Profile für die Anwendung enthält und der Seitenindex Wert 2 mit einer Seitengröße von 5 ist, enthält die zurückgegebene **ProfileInfoCollection** das sechste bis zehnte Profil. Der Total Records-Wert wird auf 13 festgelegt, wenn die-Methode zurückgibt. |
| FindInactiveProfilesByUserName-Methode | Verwendet als Eingabe einen **ProfileAuthenticationOption** -Wert, eine Zeichenfolge mit einem Benutzernamen, ein **DateTime** -Objekt, eine ganze Zahl, die den Seitenindex angibt, eine Ganzzahl, die die Seitengröße angibt, und einen Verweis auf eine Ganzzahl, die auf die Gesamtzahl der Profile festgelegt wird. Gibt eine **ProfileInfoCollection** zurück, die **ProfileInfo** -Objekte für alle Profile in der Datenquelle enthält, wobei der Benutzername mit dem angegebenen Benutzernamen übereinstimmt, wobei das Datum der letzten Aktivität kleiner oder gleich dem angegebenen **DateTime**-Wert ist und der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Der Parameter " **ProfileAuthenticationOption** " gibt an, ob nur anonyme Profile, nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Wenn Ihre Datenquelle zusätzliche Suchfunktionen unterstützt, wie z. b. Platzhalter Zeichen, können Sie ausführlichere Suchfunktionen für Benutzernamen bereitstellen. Die Ergebnisse, die von der **FindInactiveProfilesByUserName** -Methode zurückgegeben werden, werden durch die Werte für den Seitenindex und die Seitengröße eingeschränkt. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** -Objekten an, die in der **ProfileInfoCollection**zurückgegeben werden. Der Seiten Indexwert gibt an, welche Ergebnisseite zurückgegeben werden soll, wobei 1 die erste Seite identifiziert. Der-Parameter für die Gesamtanzahl der Datensätze ist ein out-Parameter (Sie können **ByRef** in Visual Basic verwenden), der auf die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher z. b. 13 Profile für die Anwendung enthält und der Seitenindex Wert 2 mit einer Seitengröße von 5 ist, enthält die zurückgegebene **ProfileInfoCollection** das sechste bis zehnte Profil. Der Total Records-Wert wird auf 13 festgelegt, wenn die-Methode zurückgibt. |
| Getnumofinactiveprofiles-Methode | Nimmt als Eingabe einen **ProfileAuthenticationOption** -Wert und ein **DateTime** -Objekt an und gibt die Anzahl aller Profile in der Datenquelle zurück, bei denen das Datum der letzten Aktivität kleiner oder gleich dem angegebenen **DateTime** -Wert ist und der Anwendungsname mit dem Wert der **ApplicationName** -Eigenschaft übereinstimmt. Der Parameter " **ProfileAuthenticationOption** " gibt an, ob nur anonyme Profile, nur authentifizierte Profile oder alle Profile gezählt werden sollen. |

### <a name="applicationname"></a>ApplicationName

Da Profil Anbieter Profilinformationen für jede Anwendung separat speichern, müssen Sie sicherstellen, dass das Datenschema den Anwendungsnamen enthält und dass Abfragen und Updates auch den Anwendungsnamen enthalten. Beispielsweise wird der folgende Befehl verwendet, um einen Eigenschafts Wert aus einer Datenbank basierend auf dem Benutzernamen abzurufen, und ob das Profil anonym ist, und stellt sicher, dass der Wert **ApplicationName** in der Abfrage enthalten ist.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.net-Themen

## <a name="what-are-aspnet-20-themes"></a>Was sind ASP.NET 2,0-Designs?

Einer der wichtigsten Aspekte einer Webanwendung ist ein konsistentes Erscheinungsbild auf der Website. ASP.NET 1. x-Entwickler verwenden in der Regel Cascading Stylesheets (CSS), um ein konsistentes Erscheinungsbild zu implementieren. ASP.NET 2,0-Designs verbessern bei CSS erheblich, da Sie dem ASP.NET-Entwickler die Möglichkeit einräumen, die Darstellung von ASP.NET-Server Steuerelementen und HTML-Elementen zu definieren. ASP.net Themes können auf einzelne Steuerelemente, eine bestimmte Webseite oder eine gesamte Webanwendung angewendet werden. Themen verwenden eine Kombination aus CSS-Dateien, einer optionalen Skin-Datei und einem optionalen Image-Verzeichnis, wenn Bilder benötigt werden. Die Skin-Datei steuert die visuelle Darstellung von ASP.NET-Server Steuerelementen.

## <a name="where-are-themes-stored"></a>Wo werden Designs gespeichert?

Der Speicherort, an dem die Designs gespeichert werden, unterscheidet sich je nach Bereich. Designs, die auf eine beliebige Anwendung angewendet werden können, werden im folgenden Ordner gespeichert:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Ein Design, das spezifisch für eine bestimmte Anwendung ist, wird in einem `App\_Themes\<Theme\_Name>` Verzeichnis im Stammverzeichnis der Website gespeichert.

> [!NOTE]
> Eine Skin-Datei sollte nur Server Steuerelement Eigenschaften ändern, die sich auf die Darstellung auswirken.

Ein globales Design ist ein Design, das auf eine beliebige Anwendung oder Website angewendet werden kann, die auf dem Webserver ausgeführt wird. Diese Themen werden standardmäßig im Verzeichnis "ASP. netclientfiles\designs" gespeichert, das sich im Verzeichnis "v2. x. xxxxx" befindet. Alternativ können Sie die Designdateien in den Ordner ASPNET\_Client/System\_Web/[Version]/Themes/[Design\_Name] im Stammverzeichnis Ihrer Website verschieben.

Anwendungsspezifische Designs können nur auf die Anwendung angewendet werden, in der sich die Dateien befinden. Diese Dateien werden im `App\_Themes/<theme\_name>` Verzeichnis im Stammverzeichnis der Website gespeichert.

## <a name="the-components-of-a-theme"></a>Die Komponenten eines Designs

Ein Design besteht aus einer oder mehreren CSS-Dateien, einer optionalen Skin-Datei und einem optionalen Bilder Ordner. Die CSS-Dateien können einen beliebigen Namen haben (z. b. "default. CSS" oder "Theme. CSS" usw.), und Sie müssen sich im Stammverzeichnis des Designs-Ordners befinden. Die CSS-Dateien werden verwendet, um gewöhnliche CSS-Klassen und Attribute für bestimmte Selektoren zu definieren. Um eine der CSS-Klassen auf ein Page-Element anzuwenden, wird die **CssClass** -Eigenschaft verwendet.

Die Skin-Datei ist eine XML-Datei, die Eigenschafts Definitionen für ASP.NET-Server Steuerelemente enthält. Der unten aufgeführte Code ist eine Beispiel-Skin-Datei.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Abbildung 1** unten zeigt eine kleine ASP.NET-Seite, die ohne angewendetes Design durchsucht wurde. **Abbildung 2** zeigt die gleiche Datei mit angewendetem Design. Die Hintergrundfarbe und die Textfarbe werden über eine CSS-Datei konfiguriert. Die Darstellung der Schaltfläche und des Textfelds wird mithilfe der oben aufgeführten Skin-Datei konfiguriert.

![Kein Design](profiles-themes-and-web-parts/_static/image1.gif)

**Abbildung 1**: kein Design

![Design angewendet](profiles-themes-and-web-parts/_static/image2.gif)

**Abbildung 2**: Design angewendet

Die oben aufgeführte Skin-Datei definiert eine Standard Skin für alle TextBox-Steuerelemente und Schaltflächen-Steuerelemente. Dies bedeutet, dass alle TextBox-Steuerelemente und Schaltflächen Steuerelemente, die auf einer Seite eingefügt werden, diese Darstellung annehmen. Sie können auch eine Skin definieren, die auf bestimmte Instanzen dieser Steuerelemente angewendet werden kann, indem Sie die **SkinID** -Eigenschaft des Steuer Elements verwenden.

Der folgende Code definiert eine Skin für ein Schaltflächen-Steuerelement. Nur Schaltflächen-Steuerelemente mit der Eigenschaft " **SkinID** " von " **goButton** " übernehmen die Darstellung der Skin.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Sie können nur eine Standard Skin pro Server Steuerelement eingeben. Wenn Sie zusätzliche Skins benötigen, sollten Sie die Eigenschaft "SkinID" verwenden.

## <a name="applying-themes-to-pages"></a>Anwenden von Designs auf Seiten

Ein Design kann mit einer der folgenden Methoden angewendet werden:

- In den &lt;Seiten&gt; Element der Datei "Web. config"
- In der @Page-Direktive einer Seite
- Programmgesteuert

## <a name="applying-a-theme-in-the-configuration-file"></a>Anwenden eines Designs in der Konfigurationsdatei

Verwenden Sie die folgende Syntax, um ein Design in der Anwendungs Konfigurationsdatei anzuwenden:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Der hier angegebene Design Name muss mit dem Namen des Designs-Ordners identisch sein. Dieser Ordner kann an einem der zuvor erwähnten Speicherorte vorhanden sein. Wenn Sie versuchen, ein Design anzuwenden, das nicht vorhanden ist, tritt ein Konfigurationsfehler auf.

## <a name="applying-a-theme-in-the-page-directive"></a>Anwenden eines Designs in der Page-Direktive

Sie können auch ein Design in der @ Page-Direktive anwenden. Mit dieser Methode können Sie ein Design für eine bestimmte Seite verwenden.

Verwenden Sie die folgende Syntax, um ein Design in der @Page-Direktive anzuwenden:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Erneut muss das hier angegebene Design dem Design Ordner entsprechen, wie bereits erwähnt. Wenn Sie versuchen, ein Design anzuwenden, das nicht vorhanden ist, tritt ein Buildfehler auf. Visual Studio hebt auch das Attribut hervor und benachrichtigt Sie, dass kein solches Design vorhanden ist.

## <a name="applying-a-theme-programmatically"></a>Programm gesteuertes Anwenden eines Designs

Wenn Sie ein Design Programm gesteuert anwenden möchten, müssen Sie die **Theme** -Eigenschaft für die Seite in der **Seite\_PreInit** -Methode angeben.

Verwenden Sie die folgende Syntax, um ein Design Programm gesteuert anzuwenden:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Das Design muss in der PreInit-Methode aufgrund des Lebenszyklus der Seite angewendet werden. Wenn Sie Sie nach diesem Punkt anwenden, wurde das Seitendesign bereits von der Laufzeit übernommen, und eine Änderung an diesem Punkt ist im Lebenszyklus zu spät. Wenn Sie ein Design anwenden, das nicht vorhanden ist, tritt eine **HttpException** auf. Wenn ein Design Programm gesteuert angewendet wird, tritt eine Buildwarnung auf, wenn für ein Server Steuerelement eine "SkinID"-Eigenschaft angegeben ist. Diese Warnung soll Sie darüber informieren, dass kein Design deklarativ angewendet wird, und es kann ignoriert werden.

## <a name="exercise-1--applying-a-theme"></a>Übung 1: Anwenden eines Designs

In dieser Übung wenden Sie ein ASP.net-Design auf eine Website an.

> [!IMPORTANT]
> Wenn Sie Microsoft Word verwenden, um Informationen in eine Skin-Datei einzugeben, stellen Sie sicher, dass Sie keine regulären Anführungszeichen durch intelligente Anführungszeichen ersetzen. Intelligente Anführungszeichen verursachen Probleme mit Skin-Dateien.

1. Erstellen Sie eine neue ASP.NET-Website.
2. Klicken Sie mit der rechten Maustaste auf das Projekt in Projektmappen-Explorer und wählen Sie neues Element hinzufügen aus.
3. Wählen Sie Webkonfigurationsdatei aus der Liste der Dateien aus, und klicken Sie auf Hinzufügen
4. Klicken Sie mit der rechten Maustaste auf das Projekt in Projektmappen-Explorer und wählen Sie neues Element hinzufügen aus.
5. Wählen Sie Skin File aus, und klicken Sie auf Hinzufügen
6. Klicken Sie auf Ja, wenn Sie gefragt werden, ob Sie die Datei in der APP\_Theme-Ordner platzieren möchten.
7. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner "skinfile" im Ordner "App\_Designs", und wählen Sie neues Element hinzufügen aus.
8. Wählen Sie in der Liste der Dateien Stylesheet aus, und klicken Sie auf hinzufügen. Sie verfügen jetzt über alle Dateien, die zum Implementieren des neuen Designs erforderlich sind. Allerdings hat Visual Studio Ihren Designs-Ordner "skinfile" benannt. Klicken Sie mit der rechten Maustaste auf diesen Ordner, und ändern Sie den Namen in cooltheme.
9. Öffnen Sie die Datei "skinfile. Skin", und fügen Sie den folgenden Code am Ende der Datei ein: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Speichern Sie die Datei "skinfile. Skin".
11. Öffnen Sie die Stylesheet. CSS.
12. Ersetzen Sie den gesamten Text in der Datei durch Folgendes: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Speichern Sie die Datei "Stylesheet. CSS".
14. Öffnen Sie die Seite Default. aspx.
15. Hinzufügen eines TextBox-Steuer Elements und eines Schaltflächen-Steuer Elements
16. Speichern Sie die Seite. Navigieren Sie jetzt zur Seite "default. aspx". Es sollte als normales Webformular angezeigt werden.
17. Öffnen Sie die Datei "Web. config".
18. Fügen Sie direkt unterhalb des öffnenden `<system.web>` Tags Folgendes hinzu: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Speichern Sie die Datei "Web. config". Navigieren Sie jetzt zur Seite "default. aspx". Es sollte angezeigt werden, wenn das Design angewendet wird.
20. Wenn Sie nicht bereits geöffnet ist, öffnen Sie die Seite "default. aspx" in Visual Studio.
21. Wählen Sie die Schaltfläche aus.
22. Ändern Sie die Eigenschaft **SkinID** in goButton. Beachten Sie, dass Visual Studio eine Dropdown Liste mit gültigen SkinID-Werten für ein Schaltflächen-Steuerelement enthält.
23. Speichern Sie die Seite. Nun wird die Seite in Ihrem Browser erneut in der Vorschau angezeigt. Die Schaltfläche sollte jetzt "Go" lauten und sollte in der Darstellung breiter aussehen.

Mithilfe der Eigenschaft " **SkinID** " können Sie problemlos verschiedene Skins für verschiedene Instanzen eines bestimmten Typs von Server Steuerelementen konfigurieren.

## <a name="the-stylesheettheme-property"></a>Die StyleSheetTheme-Eigenschaft

Bisher haben wir nur über das Anwenden von Designs mithilfe der Theme-Eigenschaft gesprochen. Wenn die Design-Eigenschaft verwendet wird, werden alle deklarativen Einstellungen für Server Steuerelemente von der Skin-Datei überschrieben. In Übung 1 haben Sie z. b. für das Schaltflächen-Steuerelement eine SkinID "goButton" angegeben und den Text der Schaltfläche in "Go" geändert. Möglicherweise haben Sie bemerkt, dass die Text-Eigenschaft der Schaltfläche im Designer auf "Button" festgelegt wurde, aber das Design hat dies überschritten. Das Design überschreibt immer alle Eigenschafts Einstellungen im Designer.

Wenn Sie in der Lage sein möchten, die Eigenschaften zu überschreiben, die in der Skin-Datei des Designs mit den im Designer angegebenen Eigenschaften definiert sind, können Sie die **StyleSheetTheme** -Eigenschaft anstelle der Theme-Eigenschaft verwenden. Die StyleSheetTheme-Eigenschaft ist identisch mit der Design-Eigenschaft, mit der Ausnahme, dass Sie nicht alle expliziten Eigenschafts Einstellungen außer Kraft setzt, wie die Design-Eigenschaft tut.

Um dies in Aktion zu sehen, öffnen Sie die Datei Web. config aus dem Projekt in Übung 1, und ändern Sie das `<pages>`-Element wie folgt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Navigieren Sie jetzt zur Seite "default. aspx", und Sie sehen, dass das Schaltflächen-Steuerelement erneut die Text-Eigenschaft "Button" hat. Dies liegt daran, dass die explizite Eigenschafts Einstellung im Designer die Text Eigenschaft überschreibt, die von der goButton-SkinID festgelegt wird.

## <a name="overriding-themes"></a>Überschreiben von Designs

Ein globales Design kann überschrieben werden, indem ein Design mit dem gleichen Namen im App-\_Theme-Ordner der Anwendung angewendet wird. Das Design wird jedoch nicht in einem echten Überschreibungs Szenario angewendet. Wenn die Laufzeit Designdateien im Ordner App\_Designs findet, wird das Design mithilfe dieser Dateien angewendet, und das globale Design wird ignoriert.

Die StyleSheetTheme-Eigenschaft ist über schreibbar und kann im Code wie folgt überschrieben werden:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Webparts

ASP.NET Webparts ist ein integrierter Satz von Steuerelementen zum Erstellen von Websites, mit denen Endbenutzer den Inhalt, die Darstellung und das Verhalten von Webseiten direkt in einem Browser ändern können. Die Änderungen können auf alle Benutzer der Website oder einzelner Benutzer angewendet werden. Wenn Benutzer Seiten und Steuerelemente ändern, können die Einstellungen gespeichert werden, um die persönlichen Einstellungen eines Benutzers in zukünftigen Browsersitzungen beizubehalten, einem Feature namens Personalization. Diese Webparts Funktionen bedeuten, dass Entwickler den Endbenutzern ermöglichen können, eine Webanwendung dynamisch zu personalisieren, ohne dass Entwickler oder Administrator eingreifen müssen.

Mithilfe des Webparts-Steuerelement Satzes können Sie als Entwickler Endbenutzern folgende Aktionen ermöglichen:

- Personalisieren Sie den Seiten Inhalt. Benutzer können einer Seite neue Webparts Steuerelemente hinzufügen, Sie entfernen, Sie ausblenden oder Sie wie normale Fenster minimieren.
- Personalisieren Sie das Seitenlayout. Benutzer können ein Webparts-Steuerelement in eine andere Zone auf einer Seite ziehen oder seine Darstellung, Eigenschaften und ihr Verhalten ändern.
- Exportieren und Importieren von Steuerelementen. Benutzer können Webparts Steuerungseinstellungen für die Verwendung auf anderen Seiten oder Websites importieren oder exportieren, wobei die Eigenschaften, die Darstellung und sogar die Daten in den Steuerelementen beibehalten werden. Dadurch werden die Dateneingabe-und Konfigurations Anforderungen für Endbenutzer reduziert.
- Erstellen Sie Verbindungen. Benutzer können Verbindungen zwischen Steuerelementen einrichten, sodass z. b. ein Diagramm Steuerelement ein Diagramm für die Daten in einem Stock Ticker-Steuerelement anzeigen kann. Benutzer können nicht nur die Verbindung selbst personalisieren, sondern auch das Aussehen und die Details der Anzeige der Daten durch das Diagramm Steuerelement.
- Verwalten und personalisieren Sie Einstellungen auf Website Ebene. Autorisierte Benutzer können Einstellungen auf Website Ebene konfigurieren, feststellen, wer auf eine Website oder Seite zugreifen kann, rollenbasierten Zugriff auf Steuerelemente festlegen usw. Beispielsweise könnte ein Benutzer in einer Administrator Rolle ein Webparts Steuerelement festlegen, das von allen Benutzern gemeinsam genutzt werden soll, und verhindern, dass Benutzer, die keine Administratoren sind, das freigegebene Steuerelement personalisieren.

In der Regel arbeiten Sie mit Webparts auf eine von drei Arten: das Erstellen von Seiten, die Webparts Steuerelemente verwenden, das Erstellen einzelner Webparts Steuerelemente oder das Erstellen von benutzerspezifischen Webanwendungen wie z. b. einem Portal.

## <a name="page-development"></a>Seiten Entwicklung

Seiten Entwickler können mit visuellen Entwurfs Tools wie Microsoft Visual Studio 2005 Seiten erstellen, die Webparts verwenden. Ein Vorteil bei der Verwendung eines Tools wie z. b. Visual Studio besteht darin, dass der Webparts Steuerelement Satz Features für die Drag & Drop-Erstellung und-Konfiguration von Webparts Steuerelementen in einem visuellen Designer bereitstellt. Beispielsweise können Sie den Designer verwenden, um eine Webparts Zone oder ein Webparts Editor-Steuerelement auf die Entwurfs Oberfläche zu ziehen. Anschließend können Sie das Steuerelement direkt im Designer mithilfe der Benutzeroberfläche konfigurieren, die vom Webparts Steuerelement Satz bereitgestellt wird. Dies kann die Entwicklung Webparts Anwendungen beschleunigen und die Menge des Codes reduzieren, den Sie schreiben müssen.

## <a name="control-development"></a>Steuern der Entwicklung

Sie können jedes vorhandene ASP.NET-Steuerelement als Webparts-Steuerelement verwenden, einschließlich Standard-Webserver Steuerelementen, benutzerdefinierter Server Steuerelemente und Benutzer Steuerelemente. Für eine maximale programmgesteuerte Steuerung der Umgebung können Sie auch benutzerdefinierte Webparts Steuerelemente erstellen, die von der WebPart-Klasse abgeleitet werden. Bei der Entwicklung einzelner Webparts Steuerelemente erstellen Sie in der Regel ein Benutzer Steuerelement und verwenden es als Webparts Steuerelement oder entwickeln ein benutzerdefiniertes Webparts Steuerelement.

Als Beispiel für die Entwicklung eines benutzerdefinierten Webparts Steuer Elements können Sie ein Steuerelement erstellen, das die von anderen ASP.NET-Server Steuerelementen bereitgestellten Features bereitstellt, die für das Verpacken als personalisierbares Webparts Steuerelement nützlich sein können: Kalender, Listen, Finanzinformationen, Nachrichten, Rechner, Rich-Text-Steuerelemente zum Aktualisieren von Inhalten, bearbeitbare Raster, die Verbindungen mit Datenbanken herstellen, Diagramme, die Ihre Anzeige dynamisch aktualisieren, oder Wetter-und Reiseinformationen. Wenn Sie dem Steuerelement einen visuellen Designer bereitstellen, kann jeder Seiten Entwickler, der Visual Studio verwendet, das Steuerelement einfach in eine Webparts Zone ziehen und zur Entwurfszeit konfigurieren, ohne zusätzlichen Code schreiben zu müssen.

Personalisierung ist die Grundlage des Webparts Features. Dadurch können Benutzer das Layout, die Darstellung und das Verhalten von Webparts Steuerelementen auf einer Seite ändern oder personalisieren. Die personalisierten Einstellungen sind langlebig: Sie werden nicht nur während der aktuellen Browsersitzung (wie z. b. dem Ansichts Zustand) beibehalten, sondern auch in langfristiger Speicherung, sodass die Einstellungen eines Benutzers auch für zukünftige Browsersitzungen gespeichert werden. Die Personalisierung ist für Webparts Seiten standardmäßig aktiviert.

Die strukturellen Komponenten der UI basieren auf Personalisierung und stellen die Kernstruktur und die Dienste bereit, die von allen Webparts Steuerelementen benötigt werden. Eine strukturelle Benutzeroberfläche, die auf jeder Webparts Seite erforderlich ist, ist das WebPartManager-Steuerelement. Obwohl das Steuerelement nie sichtbar ist, besteht die entscheidende Aufgabe, alle Webparts Steuerelemente auf einer Seite zu koordinieren. Beispielsweise werden alle einzelnen Webparts Steuerelemente nachverfolgt. Sie verwaltet Webparts Zonen (Regionen, die Webparts Steuerelemente auf einer Seite enthalten) und welche Steuerelemente in welchen Zonen enthalten sind. Außerdem werden die verschiedenen Anzeigemodi, in denen eine Seite angezeigt werden kann, nachverfolgt und gesteuert, z. b. Durchsuchen, verbinden, bearbeiten oder Katalog Modus, und ob Personalisierungs Änderungen für alle Benutzer oder für einzelne Benutzer gelten. Schließlich werden Verbindungen und die Kommunikation zwischen Webparts-Steuerelementen initiiert und nachverfolgt.

Die zweite Art der strukturellen Strukturkomponente ist die Zone. Zonen fungieren als Layout-Manager auf einer Webparts Seite. Sie enthalten und organisieren Steuerelemente, die von der Part-Klasse (Teil Steuerelemente) abgeleitet sind, und bieten die Möglichkeit, ein modulares Seitenlayout entweder in horizontaler oder vertikaler Ausrichtung durchzuführen. Zonen bieten außerdem allgemeine und konsistente Benutzeroberflächen Elemente (z. b. Kopf-und Fußzeilen Stil, Titel, Rahmenart, Aktions Schaltflächen usw.) für jedes Steuerelement, das Sie enthalten. Diese allgemeinen Elemente werden als Chrome eines-Steuer Elements bezeichnet. Mehrere spezialisierte Zonen Typen werden in den verschiedenen Anzeigemodi und mit verschiedenen Steuerelementen verwendet. Die verschiedenen Typen von Zonen werden im folgenden Abschnitt Webparts Essentials-Steuerelemente beschrieben.

Die Webparts UI-Steuerelemente, die alle von der **Part** -Klasse abgeleitet sind, bilden die primäre Benutzeroberfläche auf einer Webparts Seite. Der Webparts-Steuerelement Satz ist flexibel und inklusiv in den Optionen, die Sie zum Erstellen von Teil Steuerelementen erhalten. Zusätzlich zum Erstellen eigener benutzerdefinierter Webparts Steuerelemente können Sie auch vorhandene ASP.NET-Server Steuerelemente, Benutzer Steuerelemente oder benutzerdefinierte Server Steuerelemente als Webparts Steuerelemente verwenden. Die wesentlichen Steuerelemente, die am häufigsten für das Erstellen von Webparts Seiten verwendet werden, werden im nächsten Abschnitt beschrieben.

## <a name="web-parts-essential-controls"></a>Webparts wichtige Steuerelemente

Der Webparts-Steuerelement Satz ist umfangreich, aber einige Steuerelemente sind von entscheidender Bedeutung, da Sie für Webparts erforderlich sind, oder weil es sich um die Steuerelemente handelt, die am häufigsten auf Webparts Seiten verwendet werden. Wenn Sie mit der Verwendung von Webparts beginnen und grundlegende Webparts Seiten erstellen, ist es hilfreich, sich mit den wesentlichen Webparts Steuerelementen vertraut zu machen, die in der folgenden Tabelle beschrieben werden.

| **Webparts-Steuerelement** | **Beschreibung** |
| --- | --- |
| WebPartManager | Verwaltet alle Webparts Steuerelemente auf einer Seite. Für jede Webparts Seite ist nur ein **WebPartManager** -Steuerelement erforderlich. |
| CatalogZone | Enthält CatalogPart-Steuerelemente. Verwenden Sie diese Zone, um einen Katalog mit Webparts-Steuerelementen zu erstellen, aus denen Benutzer Steuerelemente auswählen können, die einer Seite hinzugefügt werden |
| Editor Zone | Enthält Editor Part-Steuerelemente. Verwenden Sie diese Zone, um es Benutzern zu ermöglichen, Webparts Steuerelemente auf einer Seite zu bearbeiten und zu personalisieren. |
| WebPartZone | Enthält und stellt das allgemeine Layout für die WebPart-Steuerelemente bereit, die die Hauptbenutzer Oberfläche einer Seite bilden. Verwenden Sie diese Zone immer dann, wenn Sie Seiten mit Webparts Steuerelementen erstellen. Seiten können eine oder mehrere Zonen enthalten. |
| ConnectionsZone | Enthält WebPartConnection-Steuerelemente und stellt eine Benutzeroberfläche zum Verwalten von Verbindungen bereit. |
| Webpart (GenericWebPart) | Rendert die primäre Benutzeroberfläche. die meisten Webparts UI-Steuerelemente fallen in diese Kategorie. Für eine maximale programmgesteuerte Steuerung können Sie benutzerdefinierte Webparts Steuerelemente erstellen, die vom Basis- **Webpart** -Steuerelement abgeleitet werden. Sie können auch vorhandene Server Steuerelemente, Benutzer Steuerelemente oder benutzerdefinierte Steuerelemente als Webparts Steuerelemente verwenden. Wenn eines dieser Steuerelemente in einer Zone platziert wird, umschließt das **WebPartManager** -Steuerelement diese automatisch zur Laufzeit mit **GenericWebPart** -Steuerelementen, sodass Sie Sie mit Webparts-Funktionalität verwenden können. |
| CatalogPart | Enthält eine Liste der verfügbaren Webparts-Steuerelemente, die Benutzer zur Seite hinzufügen können. |
| WebPartConnection | Erstellt eine Verbindung zwischen zwei Webparts-Steuerelementen auf einer Seite. Die Verbindung definiert eines der Webparts Steuerelemente als Anbieter (von Daten) und das andere als Consumer. |
| EditorPart | Dient als Basisklasse für die spezialisierten Editor-Steuerelemente. |
| Editor Part-Steuerelemente ("augenanceeditor Part", "Layouteditor Part", "verhalteeditor Part" und "propertygrideditor Part") | Benutzern das Personalisieren verschiedener Aspekte Webparts UI-Steuerelemente auf einer Seite gestatten |

## <a name="lab-create-a-web-part-page"></a>Lab: Erstellen einer Webpartseite

In dieser Übungseinheit erstellen Sie eine Webpartseite, die Informationen über ASP.NET-Profile persistent speichert.

### <a name="creating-a-simple-page-with-web-parts"></a>Erstellen einer einfachen Seite mit Webparts

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie eine Seite, die Webparts Steuerelemente verwendet, um statischen Inhalt anzuzeigen. Der erste Schritt beim Arbeiten mit Webparts besteht darin, eine Seite mit zwei erforderlichen strukturellen Elementen zu erstellen. Zuerst benötigt eine Webparts-Seite ein WebPartManager-Steuerelement, um alle Webparts Steuerelemente zu verfolgen und zu koordinieren. Zweitens benötigt eine Webparts Seite eine oder mehrere Zonen, d. & # 160; a. zusammengesetzte Steuerelemente, die WebPart-Steuerelemente oder andere Server Steuerelemente enthalten und einen angegebenen Bereich einer Seite belegen.

> [!NOTE]
> Sie müssen nichts tun, um die Webparts Personalisierung zu aktivieren. Sie ist standardmäßig für die Webparts-Steuerelement Gruppe aktiviert. Wenn Sie zum ersten Mal eine Webparts Seite auf einer Site ausführen, richtet ASP.net einen Standard Personalisierungs Anbieter ein, um die Benutzer Personalisierungs Einstellungen zu speichern. Weitere Informationen zur Personalisierung finden Sie unter Übersicht über die Webparts Personalisierung.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>So erstellen Sie eine Seite für enthaltende Webparts Steuerelemente

1. Schließen Sie die Standardseite, und fügen Sie der Website eine neue Seite mit dem Namen WebPartsDemo. aspx hinzu.
2. Wechseln Sie zur **Entwurfs** Ansicht.
3. Vergewissern Sie sich, dass im Menü **Ansicht** die Option **nicht visuelle Steuerelemente** und **Details** ausgewählt ist, damit Sie Layouttags und Steuerelemente sehen können, die keine Benutzeroberfläche haben.
4. Platzieren Sie die Einfügemarke vor den `<div>` Tags auf der Entwurfs Oberfläche, und drücken Sie die EINGABETASTE, um eine neue Zeile hinzuzufügen. Positionieren Sie die Einfügemarke vor dem neuen Zeilenzeichen, klicken Sie im Menü auf das Dropdown Listen-Steuerelement **Block Format** , und wählen Sie die Option über **Schrift 1** aus. Fügen Sie in der Überschrift den Text **Webparts Demonstrations Seite**hinzu.
5. Ziehen Sie auf der Registerkarte **Webparts** der Toolbox ein **WebPartManager** -Steuerelement auf die Seite, und positionieren Sie es direkt hinter das neue Zeilenzeichen und vor den `<div>`Tags.   
  
   Das **WebPartManager** -Steuerelement renbt keine Ausgabe, sodass es als graues Feld auf der Designer Oberfläche angezeigt wird.
6. Positionieren Sie die Einfügemarke innerhalb der `<div>` Tags.
7. Klicken Sie im Menü **Layout** auf **Tabelle einfügen**, und erstellen Sie eine neue Tabelle, die eine Zeile und drei Spalten enthält. Klicken Sie auf die Schaltfläche **Zell Eigenschaften** , wählen Sie **oben** in der Dropdown Liste **vertikale Ausrichtung** aus, klicken Sie auf **OK**, und klicken Sie erneut auf **OK** , um die Tabelle zu erstellen.
8. Ziehen Sie ein webparser-Steuerelement in die linke Tabellenspalte. Klicken Sie mit der rechten Maustaste auf das Steuerelement **webparser** , wählen Sie **Eigenschaften**aus, und legen Sie die folgenden Eigenschaften fest:   
  
   ID: SidebarZone   
  
   HeaderText: Rand Leiste
9. Ziehen Sie ein zweites **webparser** -Steuerelement in die Spalte der mittleren Tabelle, und legen Sie die folgenden Eigenschaften fest:   
  
   ID: MainZone   
  
   HeaderText: Main
10. Speichern Sie die Datei .

Ihre Seite verfügt jetzt über zwei verschiedene Zonen, die Sie separat steuern können. Allerdings hat keines der Zonen Inhalte, sodass das Erstellen von Inhalt der nächste Schritt ist. In dieser exemplarischen Vorgehensweise arbeiten Sie mit Webparts-Steuerelementen, die nur statischen Inhalt anzeigen.

Das Layout einer Webparts Zone wird durch ein &lt;ZoneTemplate-&gt;-Element angegeben. Innerhalb der Zonen Vorlage können Sie jedes beliebige ASP.NET-Steuerelement hinzufügen, egal ob es sich um ein benutzerdefiniertes Webparts Steuerelement, ein Benutzer Steuerelement oder ein vorhandenes Server Steuerelement handelt Beachten Sie, dass hier das Label-Steuerelement verwendet wird und dass Sie einfach statischen Text hinzufügen. Wenn Sie ein reguläres Server Steuerelement in einer **webparser-One** -Zone platzieren, behandelt ASP.NET das-Steuerelement zur Laufzeit als Webparts-Steuerelement, das Webparts-Funktionen für das Steuerelement aktiviert.

**So erstellen Sie Inhalte für die Hauptzone**

1. Ziehen Sie in der **Entwurfs** Ansicht ein **Label** -Steuerelement von der Registerkarte **Standard** der Toolbox in den Inhalts Bereich der Zone, deren **ID** -Eigenschaft auf MainZone festgelegt ist.
2. Wechseln Sie zur **Quell** Ansicht. Beachten Sie, dass ein &lt;ZoneTemplate-&gt; Element hinzugefügt wurde, um das **Label** -Steuerelement in der MainZone zu schließen.
3. Fügen Sie dem &lt;ASP: Label-&gt; Element ein Attribut mit dem Namen **Titel** hinzu, und legen Sie dessen Wert auf "Content" fest. Entfernen Sie das Attribut Text = "Label" aus dem &lt;ASP: Label-&gt; Element. Fügen Sie zwischen dem öffnenden und dem schließenden Tag des &lt;ASP: Label&gt;-Elements in einem Paar von &lt;H2-&gt; Element Tags Text hinzu, wie z. b. **Willkommen auf der Startseite** . Der Code sollte wie folgt aussehen. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Speichern Sie die Datei .

Erstellen Sie als nächstes ein Benutzer Steuerelement, das der Seite auch als Webparts-Steuerelement hinzugefügt werden kann.

### <a name="to-create-a-user-control"></a>So erstellen Sie ein benutzerdefiniertes Steuerelement

1. Fügen Sie Ihrer Website ein neues Webbenutzer Steuerelement hinzu, das als Such Steuerelement dient. Deaktivieren Sie die Option zum **Platzieren des Quellcodes in einer separaten Datei**. Fügen Sie Sie im gleichen Verzeichnis wie die WebPartsDemo. aspx-Seite hinzu, und nennen Sie Sie "SearchUserControl. ascx".   
  
    > [!NOTE]
    > Das Benutzer Steuerelement für diese exemplarische Vorgehensweise implementiert die tatsächliche Suchfunktion nicht. Sie wird nur zur Veranschaulichung Webparts Funktionen verwendet.
2. Wechseln Sie zur **Entwurfs** Ansicht. Ziehen Sie auf der Registerkarte **Standard** der Toolbox ein TextBox-Steuerelement auf die Seite.
3. Platzieren Sie die Einfügemarke nach dem soeben hinzugefügten Textfeld, und drücken Sie die EINGABETASTE, um eine neue Zeile hinzuzufügen
4. Ziehen Sie ein Schaltflächen-Steuerelement auf die Seite in der neuen Zeile unterhalb des soeben hinzugefügten Textfelds.
5. Wechseln Sie zur **Quell** Ansicht. Stellen Sie sicher, dass der Quellcode für das Benutzer Steuerelement wie im folgenden Beispiel aussieht. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Speichern und schließen Sie die Datei.

Nun können Sie der Sidebar-Zone Webparts-Steuerelemente hinzufügen. Sie fügen der Sidebar-Zone zwei-Steuerelemente hinzu, eine mit einer Liste von Links und eine andere, die das Benutzer Steuerelement ist, das Sie im vorherigen Verfahren erstellt haben. Die Links werden **als Standard** Bezeichnungs Server-Steuerelement hinzugefügt, ähnlich der Art, wie Sie den statischen Text für die Hauptzone erstellt haben. Obwohl die einzelnen im Benutzer Steuerelement enthaltenen Server Steuerelemente direkt in der Zone enthalten sein könnten (z. b. das Label-Steuerelement), ist dies in diesem Fall nicht der Fall. Stattdessen sind Sie Teil des Benutzer Steuer Elements, das Sie im vorherigen Verfahren erstellt haben. Dies veranschaulicht eine gängige Methode, beliebige Steuerelemente und zusätzliche Funktionen in einem Benutzer Steuerelement zu verpacken und dann auf dieses Steuerelement in einer Zone als Webparts-Steuerelement zu verweisen.

Zur Laufzeit umschließt der Webparts-Steuerelement Satz beide Steuerelemente mit GenericWebPart-Steuerelementen. Wenn ein **GenericWebPart** -Steuerelement ein Webserver Steuerelement umschließt, ist das generische Teil Steuerelement das übergeordnete Steuerelement, und Sie können über die ChildControl-Eigenschaft des übergeordneten Steuer Elements auf das Server Steuerelement zugreifen. Diese Verwendung generischer Teil Steuerelemente ermöglicht Standard-Webserver Steuerelementen das gleiche grundlegende Verhalten und dieselben Attribute wie Webparts Steuerelemente, die von der **Webpart** -Klasse abgeleitet werden.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>So fügen Sie der Sidebar-Zone Webparts Steuerelemente hinzu

1. Öffnen Sie die Seite WebPartsDemo. aspx.
2. Wechseln Sie zur **Entwurfs** Ansicht.
3. Ziehen Sie die von Ihnen erstellte Benutzer Steuerelement Seite, SearchUserControl. ascx, von **Projektmappen-Explorer** in die Zone, deren **ID** -Eigenschaft auf SidebarZone festgelegt ist, und legen Sie Sie dort ab.
4. Speichern Sie die WebPartsDemo. aspx-Seite.
5. Wechseln Sie zur **Quell** Ansicht.
6. Fügen Sie im &lt;ASP: webparametzone&gt;-Element für die SidebarZone direkt oberhalb des Verweises auf das Benutzer Steuerelement ein &lt;ASP: Label-&gt; Element mit enthaltenen Links hinzu, wie im folgenden Beispiel gezeigt. Fügen Sie dem Benutzer Steuer Elementtag außerdem ein **Titel** Attribut mit dem Wert **Suchen**hinzu, wie hier gezeigt. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Speichern und schließen Sie die Datei.

Nun können Sie Ihre Seite testen, indem Sie Sie in Ihrem Browser aufrufen. Die Seite zeigt die beiden Zonen an. Der folgende Screenshot zeigt die Seite.

**Webparts Demo Seite mit zwei Zonen**

![Screenshot der exemplarischen Vorgehensweise zu Webparts vs 1](profiles-themes-and-web-parts/_static/image3.gif)

**Abbildung 3**: Webparts vs Exemplarische Vorgehensweise 1-Bildschirmfoto

In der Titelleiste jedes Steuer Elements ist ein abwärts Pfeil, der den Zugriff auf ein Verbenmenü der verfügbaren Aktionen ermöglicht, die Sie für ein Steuerelement ausführen können. Klicken Sie für eines der Steuerelemente auf das Verbenmenü, und klicken Sie dann auf das Verb minimieren, um das Steuerelement zu **minimieren** . Klicken Sie im Verbenmenü auf **Wiederherstellen**, und das Steuerelement wird auf seine normale Größe zurückgegeben.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Benutzer können Seiten bearbeiten und Layout ändern

Webparts bietet Benutzern die Möglichkeit, das Layout Webparts Steuerelemente zu ändern, indem Sie Sie aus einer Zone in eine andere ziehen. Zusätzlich zu den Benutzern das Verschieben von **Webpart** -Steuerelementen aus einer Zone in eine andere ermöglicht, können Sie es Benutzern ermöglichen, verschiedene Eigenschaften der Steuerelemente zu bearbeiten, einschließlich ihrer Darstellung, des Layouts und des Verhaltens. Der Webparts-Steuerelement Satz stellt grundlegende Bearbeitungsfunktionen für **Webpart** -Steuerelemente bereit. Obwohl dies in dieser exemplarischen Vorgehensweise nicht der Fall ist, können Sie auch benutzerdefinierte Editor-Steuerelemente erstellen, die es Benutzern ermöglichen, die Funktionen von **Webpart** -Steuerelementen zu bearbeiten. Wie beim Ändern des Speicher Orts eines **Webpart** -Steuer Elements basiert das Bearbeiten der Eigenschaften eines Steuer Elements auf der ASP.NET-Personalisierung, um die Änderungen zu speichern, die Benutzer vornehmen.

In diesem Teil der exemplarischen Vorgehensweise fügen Sie Benutzern die Möglichkeit hinzu, die grundlegenden Merkmale eines beliebigen **Webpart** -Steuer Elements auf der Seite zu bearbeiten. Um diese Funktionen zu aktivieren, fügen Sie der Seite ein weiteres benutzerdefiniertes Benutzer Steuerelement hinzu, zusammen mit einem &lt;ASP: editorizone&gt; Element und zwei Bearbeitungs Steuerelementen.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>So erstellen Sie ein Benutzer Steuerelement, das das Ändern des Seitenlayouts ermöglicht

1. Wählen Sie in Visual Studio im Menü **Datei** das **neue** Untermenü aus, und klicken Sie auf die Option **Datei** .
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Webbenutzer Steuer**Element aus. Nennen Sie die neue Datei "Displaymodemenu. ascx". Deaktivieren Sie die Option zum **Platzieren des Quellcodes in einer separaten Datei**.
3. Klicken Sie zum Erstellen des neuen Steuer Elements auf hinzufügen.
4. Wechseln Sie zur **Quell** Ansicht.
5. Entfernen Sie den gesamten vorhandenen Code in der neuen Datei, und fügen Sie den folgenden Code ein. Dieser Benutzer Steuerelement Code verwendet Funktionen des Webparts Steuerelement Satzes, mit denen eine Seite die Anzeige bzw. den Anzeigemodus ändern kann, und ermöglicht es Ihnen, die physische Darstellung und das Layout der Seite zu ändern, während Sie sich in bestimmten Anzeigemodi befinden. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Speichern Sie die Datei, indem Sie auf der Symbolleiste auf das Symbol Speichern klicken, oder wählen Sie im Menü **Datei** die Option **Speichern** aus.

### <a name="to-enable-users-to-change-the-layout"></a>So aktivieren Sie die Benutzer, um das Layout zu ändern

1. Öffnen Sie die WebPartsDemo. aspx-Seite, und wechseln Sie zur **Entwurfs** Ansicht.
2. Positionieren Sie die Einfügemarke direkt nach dem zuvor hinzugefügten Steuerelement **WebPartManager** in der **Entwurfs** Ansicht. Fügen Sie nach dem Text eine harte Rückgabe hinzu, sodass nach dem **WebPartManager** -Steuerelement eine leere Zeile vorhanden ist. Platzieren Sie die Einfügemarke in der leeren Zeile.
3. Ziehen Sie das Benutzer Steuerelement, das Sie soeben erstellt haben (die Datei mit dem Namen Displaymodemenu. ascx), auf die Seite WebPartsDemo. aspx, und legen Sie es in der leeren Zeile ab.
4. Ziehen Sie ein editorizone-Steuerelement aus dem Abschnitt **Webparts** der Toolbox in die Zelle verbleibende Tabelle öffnen auf der Seite WebPartsDemo. aspx.
5. Ziehen Sie aus dem Abschnitt **Webparts** der Toolbox ein Element "looanceeditor Part" und ein Layouteditor Part-Steuerelement in das Steuerelement " **Editor Zone** ".
6. Wechseln Sie zur **Quell** Ansicht. Der resultierende Code in der Tabellenzelle sollte in etwa wie der folgende Code aussehen: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Speichern Sie die WebPartsDemo. aspx-Datei. Sie haben ein Benutzer Steuerelement erstellt, das es Ihnen ermöglicht, Anzeigemodi zu ändern und das Seitenlayout zu ändern, und Sie haben auf der primären Webseite auf das-Steuerelement verwiesen.

Nun können Sie die Funktion zum Bearbeiten von Seiten und zum Ändern des Layouts testen.

### <a name="to-test-layout-changes"></a>So testen Sie Layoutänderungen

1. Lädt die Seite in einem Browser.
2. Klicken Sie auf das Dropdown Menü **Anzeigemodus** , und wählen Sie **Bearbeiten**aus. Die Zonen Titel werden angezeigt.
3. Ziehen Sie das Steuerelement **Meine Verknüpfungen** von der Titelleiste aus der Rand Leiste an den unteren Rand der Haupt Zone. Die Seite sollte wie im folgenden Screenshot aussehen:

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Webparts Demoseite mit dem Verschieben des Link Steuer Elements

![Screenshot der exemplarischen Vorgehensweise zu Webparts vs 2](profiles-themes-and-web-parts/_static/image4.gif)

**Abbildung 4**: Screenshot der exemplarischen Vorgehensweise für Webparts vs 2

1. Klicken Sie auf das Dropdown Menü **Anzeigemodus** , und wählen Sie **Durchsuchen**aus. Die Seite wird aktualisiert, die Zonen Namen werden ausgeblendet, und das Steuerelement **Meine Verknüpfungen** bleibt dort, wo es positioniert ist.
2. Um zu veranschaulichen, dass die Personalisierung funktioniert, schließen Sie den Browser, und laden Sie die Seite dann erneut. Die von Ihnen vorgenommenen Änderungen werden für zukünftige Browsersitzungen gespeichert.
3. Wählen Sie im Menü **Anzeigemodus** die Option **Bearbeiten**aus.   
  
   Jedes Steuerelement auf der Seite wird nun mit einem abwärts Pfeil in der Titelleiste angezeigt, der das Dropdown Menü Verben enthält.
4. Klicken Sie auf den Pfeil, um das Verbenmenü im Steuerelement **Meine Verknüpfungen** anzuzeigen. Klicken Sie auf das **Bearbeitungs** Verb.   
  
   Das **editorizone** -Steuerelement wird angezeigt und zeigt die hinzugefügten Editor Part-Steuerelemente an.
5. Ändern Sie **im Abschnitt Darstellung** des Bearbeitungs Steuer Elements den **Titel** in meine Favoriten, verwenden Sie die Dropdown Liste **Chrome-Typ** , um **nur Titel**auszuwählen, und klicken **Sie dann auf über**nehmen. Der folgende Screenshot zeigt die Seite im Bearbeitungsmodus.

### <a name="web-parts-demo-page-in-edit-mode"></a>Webparts Demoseite im Bearbeitungsmodus

![Screenshot der exemplarischen Vorgehensweise zu Webparts vs 3](profiles-themes-and-web-parts/_static/image5.gif)

**Abbildung 5**: Screenshot der exemplarischen Vorgehensweise von Webparts vs 3

1. Klicken Sie auf das Menü **Anzeigemodus** , und wählen Sie **Durchsuchen** aus, um zum Suchmodus zurückzukehren.
2. Das-Steuerelement verfügt nun über einen aktualisierten Titel und keinen Rahmen, wie im folgenden Screenshot zu sehen.

### <a name="edited-web-parts-demo-page"></a>Webparts DemoPage bearbeitet

![Screenshot der exemplarischen Vorgehensweise zu Webparts vs 4](profiles-themes-and-web-parts/_static/image6.gif)

**Abbildung 4**: Webparts vs Exemplarische Vorgehensweise 4-Bildschirmfoto

### <a name="adding-web-parts-at-run-time"></a>Hinzufügen von Webparts zur Laufzeit

Außerdem können Sie es Benutzern ermöglichen, ihrer Seite zur Laufzeit Webparts Steuerelemente hinzuzufügen. Konfigurieren Sie hierzu die Seite mit einem Webparts Katalog, der eine Liste der Webparts Steuerelemente enthält, die Sie Benutzern zur Verfügung stellen möchten.

**So ermöglichen Sie Benutzern das Hinzufügen von Webparts zur Laufzeit**

1. Öffnen Sie die WebPartsDemo. aspx-Seite, und wechseln Sie zur **Entwurfs** Ansicht.
2. Ziehen Sie von der Registerkarte **Webparts** der Toolbox ein CatalogZone-Steuerelement in die Rechte Spalte der Tabelle unterhalb des **Editor Zone** -Steuer Elements.   
  
   Beide Steuerelemente können sich in derselben Tabellenzelle befinden, da Sie nicht gleichzeitig angezeigt werden.
3. Weisen Sie im Bereich Eigenschaften die Zeichenfolge **Add Webparts** der Header Text-Eigenschaft des **CatalogZone** -Steuer Elements zu.
4. Ziehen Sie im Abschnitt **Webparts** der Toolbox ein deklarativecatalogpart-Steuerelement in den Inhalts Bereich des **CatalogZone** -Steuer Elements.
5. Klicken Sie auf den Pfeil in der oberen rechten Ecke des **deklarativecatalogpart** -Steuer Elements, um das Aufgaben Menü anzuzeigen, und wählen Sie dann **Vorlagen bearbeiten**aus.
6. Ziehen Sie im Abschnitt **Standard** der Toolbox ein **FileUpload** -Steuerelement und ein **Calendar** -Steuerelement in den Abschnitt **webpartstemplate** des **deklarativecatalogpart** -Steuer Elements.
7. Wechseln Sie zur **Quell** Ansicht. Überprüfen Sie den Quellcode des &lt;ASP: CatalogZone-&gt; Elements. Beachten Sie, dass das **DeclarativeCatalogPart** -Steuerelement ein &lt;webpartstemplate-&gt;-Element mit den beiden eingeschlossenen Server Steuerelementen enthält, die Sie der Seite aus dem Katalog hinzufügen können.
8. Fügen Sie jedem der Steuerelemente, die Sie dem Katalog hinzugefügt haben, eine **Title** -Eigenschaft hinzu. verwenden Sie dazu den Zeichen folgen Wert, der für jeden Titel im folgenden Codebeispiel angezeigt wird Obwohl es sich bei dem Titel nicht um eine Eigenschaft handelt, die Sie normalerweise zur Entwurfszeit auf diesen beiden Server Steuerelementen festlegen können, werden diese Steuerelemente, wenn ein Benutzer diese Steuerelemente zur Laufzeit einer **webprotzone** -Zone aus dem Katalog hinzufügt, jeweils mit einem **GenericWebPart** -Steuerelement umfänden. Auf diese Weise können Sie als Webparts Steuerelemente fungieren, sodass Sie Titel anzeigen können.   
  
   Der Code für die beiden Steuerelemente, die im **deklarativecatalogpart** -Steuerelement enthalten sind, sollte wie folgt aussehen. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Speichern Sie die Seite.

Sie können den Katalog jetzt testen.

### <a name="to-test-the-web-parts-catalog"></a>So testen Sie den Webparts-Katalog

1. Lädt die Seite in einem Browser.
2. Klicken Sie auf das Dropdown Menü **Anzeigemodus** , und wählen Sie **catalog**aus.   
  
   Der Katalog mit dem Namen **Add Webparts** wird angezeigt.
3. Ziehen Sie das Steuerelement " **Meine Favoriten** " aus der Hauptzone zurück an den oberen Rand der Rand Leiste, und legen Sie es dort ab.
4. Aktivieren Sie im **Webparts Katalog hinzufügen** beide Kontrollkästchen, und wählen Sie dann in der Dropdown Liste die Option **Main** aus, die die verfügbaren Zonen enthält.
5. Klicken Sie im Katalog auf **Hinzufügen** . Die Steuerelemente werden der Haupt Zone hinzugefügt. Wenn Sie möchten, können Sie der Seite mehrere Instanzen von Steuerelementen aus dem Katalog hinzufügen.   
  
   Der folgende Screenshot zeigt die Seite mit dem Dateiuploadsteuerelement und den Kalender in der Hauptzone. 

![Der Haupt Zone hinzugefügte Steuerelemente aus dem Katalog](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Klicken Sie auf das Dropdown Menü **Anzeigemodus** , und wählen Sie **Durchsuchen**aus. Der Katalog wird nicht mehr angezeigt, und die Seite wird aktualisiert.
7. Schließen Sie den Browser. Laden Sie die Seite erneut. Die von Ihnen vorgenommenen Änderungen bleiben erhalten.
