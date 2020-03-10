---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON-und XML-Serialisierung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt die JSON-und XML-Formatierer in ASP.net-Web-API für ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449127"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON-und XML-Serialisierung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel werden die JSON-und XML-Formatierer in ASP.net-Web-API beschrieben.

In ASP.net-Web-API ist ein *Medientyp-Formatierer* ein Objekt, das Folgendes kann:

- Lesen von CLR-Objekten aus einem HTTP-Nachrichtentext
- Schreiben von CLR-Objekten in einen HTTP-Nachrichtentext

Die Web-API stellt Medientyp-Formatierer für JSON und XML bereit. Das Framework fügt diese Formatierer standardmäßig in die Pipeline ein. Clients können entweder JSON oder XML im Accept-Header der HTTP-Anforderung anfordern.

## <a name="contents"></a>Inhalt

- [JSON Media-Type-Formatierer](#json_media_type_formatter)

    - [Schreibgeschützte Eigenschaften](#json_readonly)
    - [Voraus](#json_dates)
    - [Einzug](#json_indenting)
    - [Kamel Schreibweise](#json_camelcasing)
    - [Anonyme und schwach typisierte Objekte](#json_anon)
- [XML-Medientyp Formatierer](#xml_media_type_formatter)

    - [Schreibgeschützte Eigenschaften](#xml_readonly)
    - [Voraus](#xml_dates)
    - [Einzug](#xml_indenting)
    - [Festlegen von XML-Serialisierern pro Typ](#xml_pertype)
- [Entfernen der JSON-oder XML-Formatierer](#removing_the_json_or_xml_formatter)
- [Behandeln von zirkulären Objekt verweisen](#handling_circular_object_references)
- [Testen der Objektserialisierung](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON Media-Type-Formatierer

Die JSON-Formatierung wird von der **jsonmediatypeer Formatter** -Klasse bereitgestellt. Standardmäßig verwendet **jsonmediatypeer Formatter** die [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) -Bibliothek, um die Serialisierung auszuführen. JSON.net ist ein Open Source-Projekt von Drittanbietern.

Wenn Sie möchten, können Sie die **jsonmediatypformatter** -Klasse so konfigurieren, dass **DataContractJsonSerializer** anstelle von JSON.NET verwendet wird. Legen Sie zu diesem Zweck die **usedatacontractjsonserializer** -Eigenschaft auf " **true**" fest:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON-Serialisierung

In diesem Abschnitt werden einige spezifische Verhalten des JSON-Formatierers mithilfe des standardmäßigen [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) -Serialisierungsprogramms beschrieben. Dies soll nicht als umfassende Dokumentation der JSON.NET-Bibliothek dienen. Weitere Informationen finden Sie in der [JSON.NET-Dokumentation](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Was wird serialisiert?

Standardmäßig sind alle öffentlichen Eigenschaften und Felder im serialisierten JSON-Code enthalten. Um eine Eigenschaft oder ein Feld auszulassen, versehen Sie Sie mit dem **jsonignore** -Attribut.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Wenn Sie einen &quot;Opt-in-&quot; Ansatz bevorzugen, müssen Sie die-Klasse mit dem **DataContract** -Attribut ergänzen. Wenn dieses Attribut vorhanden ist, werden Member ignoriert, es sei denn, Sie haben den **DataMember**. Sie können auch **DataMember** verwenden, um private Member zu serialisieren.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Schreibgeschützte Eigenschaften werden standardmäßig serialisiert.

<a id="json_dates"></a>
### <a name="dates"></a>Datumsangaben

Standardmäßig schreibt JSON.net Datumsangaben im [ISO 8601](http://www.w3.org/TR/NOTE-datetime) -Format. Datumsangaben in UTC (koordinierte Weltzeit) werden mit dem Suffix "Z" geschrieben. Datumsangaben in der lokalen Zeit umfassen einen Zeit Zonen Offset. Beispiel:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Standardmäßig behält JSON.net die Zeitzone bei. Sie können dies überschreiben, indem Sie die datetimezonehanding-Eigenschaft festlegen:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Wenn Sie lieber das [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anstelle von ISO 8601 verwenden möchten, legen Sie die **dateformathandge** -Eigenschaft in den serialisierungsprotokeneinstellungen fest:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Einzug

Legen Sie zum Schreiben von eingerückt-JSON die **Formatierungs** Einstellung auf " **Formatierung. eingezogen**" fest:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Kamel Schreibweise

Wenn sie JSON-Eigenschaftsnamen mit Kamel Schreibweise schreiben möchten, ohne das Datenmodell zu ändern, legen Sie den " **camelcasepropertynamesverhütungs tresolver** " für das Serialisierungsprogramm fest:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonyme und schwach typisierte Objekte

Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und in JSON Serialisieren. Beispiel:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Der Text der Antwortnachricht enthält den folgenden JSON-Code:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Wenn Ihre Web-API lose strukturierte JSON-Objekte von Clients empfängt, können Sie den Anforderungs Text in den Typ " **newtonsoft. JSON. Linq. jobject** " deserialisieren.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Es ist jedoch in der Regel besser, stark typisierte Datenobjekte zu verwenden. Dann müssen Sie die Daten nicht selbst analysieren, und Sie erhalten die Vorteile der Modell Validierung.

Der XML-Serialisierer unterstützt keine anonymen Typen oder **jobject** -Instanzen. Wenn Sie diese Features für Ihre JSON-Daten verwenden, sollten Sie den XML-Formatierer aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML-Medientyp Formatierer

Die XML-Formatierung wird von der **xmlmediatypeer Formatter** -Klasse bereitgestellt. Standardmäßig verwendet **xmlmediatypformatter** die **DataContractSerializer** -Klasse, um die Serialisierung auszuführen.

Wenn Sie möchten, können Sie den **xmlmediatypformatter** so konfigurieren, dass er anstelle von **DataContractSerializer**den **XmlSerializer** verwendet. Legen Sie zu diesem Zweck die **usexmlserializer** -Eigenschaft auf **true**fest:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Die **XmlSerializer** -Klasse unterstützt einen engeren Satz von Typen als **DataContractSerializer**, bietet jedoch mehr Kontrolle über den resultierenden XML-Code. Verwenden Sie ggf. **XmlSerializer** , wenn Sie ein vorhandenes XML-Schema zuordnen müssen.

### <a name="xml-serialization"></a>XML-Serialisierung

In diesem Abschnitt werden einige spezifische Verhalten des XML-Formatierers mithilfe des **standarddatacontractserializers**beschrieben.

Standardmäßig verhält sich DataContractSerializer wie folgt:

- Alle öffentlichen Lese-/Schreibeigenschaften und-Felder werden serialisiert. Um eine Eigenschaft oder ein Feld auszulassen, versehen Sie Sie mit dem **ignoredatamember** -Attribut.
- Private und geschützte Member werden nicht serialisiert.
- Schreibgeschützte Eigenschaften werden nicht serialisiert. (Der Inhalt einer schreibgeschützten Auflistungs Eigenschaft wird jedoch serialisiert.)
- Klassen-und Elementnamen werden in der XML-Datei genau so geschrieben, wie Sie in der Klassen Deklaration angezeigt werden.
- Ein XML-Standard Namespace wird verwendet.

Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie die Klasse mit dem **DataContract** -Attribut ergänzen. Wenn dieses Attribut vorhanden ist, wird die-Klasse wie folgt serialisiert:

- &quot;&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert. Um eine Eigenschaft oder ein Feld zu serialisieren, müssen Sie es mit dem **DataMember** -Attribut ergänzen.
- Um einen privaten oder geschützten Member zu serialisieren, versehen Sie ihn mit dem **DataMember** -Attribut.
- Schreibgeschützte Eigenschaften werden nicht serialisiert.
- Legen Sie den *Name* -Parameter im **DataContract** -Attribut fest, um zu ändern, wie der Klassenname im XML-Code angezeigt wird.
- Legen Sie den *Name* -Parameter im **DataMember** -Attribut fest, um zu ändern, wie ein Elementname im XML-Code angezeigt wird.
- Um den XML-Namespace zu ändern, legen Sie den *Namespace* -Parameter in der **DataContract** -Klasse fest.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Schreibgeschützte Eigenschaften werden nicht serialisiert. Wenn eine schreibgeschützte Eigenschaft ein privates Feld unterstützt, können Sie das private Feld mit dem **DataMember** -Attribut markieren. Dieser Ansatz erfordert das **DataContract** -Attribut für die-Klasse.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Datumsangaben

Datumsangaben werden im ISO 8601-Format geschrieben. Beispielsweise &quot;2012-05-23t20:21:37.9116538 z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Einzug

Legen Sie für die **Indent** -Eigenschaft den Wert " **true**" fest

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Festlegen von XML-Serialisierern pro Typ

Sie können verschiedene XML-Serialisierungstypen für verschiedene CLR-Typen festlegen. Beispielsweise könnten Sie über ein bestimmtes Datenobjekt verfügen, das **XmlSerializer** für die Abwärtskompatibilität benötigt. Sie können **XmlSerializer** für dieses Objekt verwenden und die Verwendung von **DataContractSerializer** für andere Typen fortsetzen.

Um ein XML-Serialisierungsprogramm für einen bestimmten Typ festzulegen, nennen Sie **setserializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Sie können ein **XmlSerializer** -Objekt oder ein beliebiges Objekt angeben, das von **XmlObjectSerializer**abgeleitet ist.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Entfernen der JSON-oder XML-Formatierer

Sie können das JSON-Formatierer oder den XML-Formatierer aus der Liste der Formatierer entfernen, wenn Sie Sie nicht verwenden möchten. Die Hauptgründe hierfür sind:

- Um Ihre Web-API-Antworten auf einen bestimmten Medientyp zu beschränken. Beispielsweise können Sie sich entscheiden, nur JSON-Antworten zu unterstützen und das XML-Formatierer zu entfernen.
- , Um das Standardformatierer durch einen benutzerdefinierten Formatierer zu ersetzen. Beispielsweise können Sie den JSON-Formatierer durch ihre eigene benutzerdefinierte Implementierung eines JSON-Formatierers ersetzen.

Der folgende Code zeigt, wie die Standard Formatierer entfernt werden. Nennen Sie dies in der **Anwendung\_Start** -Methode, die in "Global. asax" definiert ist.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Behandeln von zirkulären Objekt verweisen

Standardmäßig schreiben die JSON-und XML-Formatierer alle-Objekte als-Werte. Wenn zwei Eigenschaften auf das gleiche Objekt verweisen oder das gleiche Objekt zweimal in einer Auflistung angezeigt wird, serialisiert das formatierungprogramm das Objekt zweimal. Dies ist ein besonderes Problem, wenn das Objekt Diagramm Zyklen enthält, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn eine Schleife im Diagramm erkannt wird.

Beachten Sie die folgenden Objekt Modelle und den Controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Wenn diese Aktion aufgerufen wird, löst der Formatierer eine Ausnahme aus, die in die Antwortstatus Code 500 (interner Server Fehler) an den Client übersetzt.

Fügen Sie der **Anwendungs\_Start** -Methode in der Datei "Global. asax" den folgenden Code hinzu, um Objekt Verweise in JSON beizubehalten:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Nun gibt die Controller Aktion JSON zurück, das wie folgt aussieht:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Beachten Sie, dass das Serialisierungsprogramm beiden Objekten eine &quot;$ID&quot;-Eigenschaft hinzufügt. Außerdem wird erkannt, dass die Employee. Department-Eigenschaft eine-Schleife erstellt, sodass der Wert durch einen Objekt Verweis ersetzt wird: {&quot;$Ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Objekt Verweise sind in JSON nicht standardmäßig. Bevor Sie dieses Feature verwenden, sollten Sie berücksichtigen, ob die Ergebnisse von den Clients analysiert werden können. Es kann besser sein, nur Zyklen aus dem Diagramm zu entfernen. Beispielsweise ist die Verknüpfung von Employee zurück zur Abteilung in diesem Beispiel nicht unbedingt erforderlich.

Um Objekt Verweise in XML beizubehalten, haben Sie zwei Möglichkeiten. Die einfachere Option besteht darin, der Modell Klasse `[DataContract(IsReference=true)]` hinzuzufügen. Der *IsReference* -Parameter aktiviert Objekt Verweise. Beachten Sie, dass **DataContract** die Serialisierung deaktiviert, sodass Sie den Eigenschaften auch **DataMember** -Attribute hinzufügen müssen:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Nun erzeugt das Formatierer XML ähnlich wie folgt:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Wenn Sie Attribute in der Modell Klasse vermeiden möchten, gibt es eine weitere Option: Erstellen Sie eine neue typspezifische **DataContractSerializer** -Instanz, und legen Sie *preserveObjectReferences* im Konstruktor auf **true** fest. Legen Sie diese Instanz dann als Serialisierungsprogramm pro Typ für den XML-Medientyp-Formatierer fest. Der folgende Code zeigt, wie Sie dies tun:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testen der Objektserialisierung

Wenn Sie Ihre Web-API entwerfen, ist es hilfreich, zu testen, wie Ihre Datenobjekte serialisiert werden. Dies ist möglich, ohne einen Controller zu erstellen oder eine Controller Aktion aufzurufen.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
