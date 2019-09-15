---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parameter Bindung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt, wie die Web-API Parameter bindet und wie der Bindungsprozess in ASP.NET 4. x angepasst wird.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5386532ab581e023d93d16a5d4107e07f40b986f
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985826"
---
# <a name="parameter-binding-in-aspnet-web-api"></a>Parameter Bindung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

In diesem Artikel wird beschrieben, wie die Web-API Parameter bindet und wie Sie den Bindungsprozess anpassen können. Wenn die Web-API eine Methode auf einem Controller aufruft, muss Sie Werte für die Parameter festlegen, einen Prozess namens " *Binding*".

Standardmäßig verwendet die Web-API die folgenden Regeln, um Parameter zu binden:

- Wenn es sich bei dem Parameter um einen einfachen Typ handelt, versucht die Web-API, den Wert aus dem URI zu erhalten. Einfache Typen umfassen die [primitiven](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .NET-Typen (**int**, **bool**, **Double**usw.) sowie **TimeSpan**, **DateTime**, **GUID**, **Decimal**und **String** *sowie* einen beliebigen Typ mit einem Typ. Konverter, der von einer Zeichenfolge konvertieren kann. (Weitere Informationen zu Typkonvertern später.)
- Bei komplexen Typen versucht die Web-API, den Wert aus dem Nachrichtentext mit einem [Medientyp-Formatierer](media-formatters.md)zu lesen.

Hier ist z. b. eine typische Web-API-Controller Methode:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Der *ID* -Parameter ist &quot;ein&quot; einfacher Typ, daher versucht die Web-API, den Wert aus dem Anforderungs-URI zu erhalten. Der *Element* Parameter ist ein komplexer Typ, sodass die Web-API einen Medientyp-Formatierer verwendet, um den Wert aus dem Anforderungs Text zu lesen.

Um einen Wert aus dem URI zu erhalten, sucht die Web-API in den Routendaten und in der URI-Abfrage Zeichenfolge. Die Routendaten werden aufgefüllt, wenn das Routing System den URI analysiert und mit einer Route übereinstimmt. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md).

Im weiteren Verlauf dieses Artikels zeige ich Ihnen, wie Sie den Modell Bindungsprozess anpassen können. Bei komplexen Typen empfiehlt es sich jedoch, nach Möglichkeit Medientyp-Formatierer zu verwenden. Ein Hauptprinzip von http ist, dass Ressourcen im Nachrichtentext gesendet werden, wobei die Inhaltsaushandlung zum Angeben der Darstellung der Ressource verwendet wird. Medientyp-Formatierer wurden für genau diesen Zweck entwickelt.

## <a name="using-fromuri"></a>Verwenden [fromuri]

Fügen Sie dem-Parameter das **[fromuri]** -Attribut hinzu, um zu erzwingen, dass die Web-API einen komplexen Typ aus dem URI liest. Im folgenden Beispiel wird ein `GeoPoint` -Typ zusammen mit einer Controller Methode definiert, die `GeoPoint` den aus dem URI abruft.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Der Client kann die Werte für breiten-und Längengrade in der Abfrage Zeichenfolge ablegen, und die Web- `GeoPoint`API verwendet diese, um ein zu erstellen. Beispiel:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Verwenden von [frombody]

Wenn Sie erzwingen möchten, dass die Web-API einen einfachen Typ aus dem Anforderungs Text liest, fügen Sie das Attribut **[frombody]** dem Parameter hinzu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

In diesem Beispiel verwendet die Web-API einen Medientyp-Formatierer, um den Wert von *Name* aus dem Anforderungs Text zu lesen. Im folgenden finden Sie ein Beispiel für eine Client Anforderung.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Wenn ein Parameter [frombody] aufweist, verwendet die Web-API den Content-Type-Header, um einen Formatierer auszuwählen. In diesem Beispiel ist &quot;der Inhaltstyp "Application/JSON&quot; ", und der Anforderungs Text ist eine unformatierte JSON-Zeichenfolge (kein JSON-Objekt).

Höchstens ein Parameter darf aus dem Nachrichtentext gelesen werden. Dies funktioniert nicht:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Der Grund für diese Regel besteht darin, dass der Anforderungs Text möglicherweise in einem nicht gepufferten Stream gespeichert wird, der nur einmal gelesen werden kann.

## <a name="type-converters"></a>Typkonverter

Sie können festlegen, dass die Web-API eine Klasse als einfachen Typ behandelt (sodass die Web-API versucht, Sie aus dem URI zu binden), indem Sie einen **TypeConverter** erstellen und eine Zeichen folgen Konvertierung bereitstellen.

Der folgende Code zeigt eine `GeoPoint` Klasse, die einen geografischen Punkt darstellt, sowie einen **TypeConverter** , der von Zeichen `GeoPoint` folgen in-Instanzen konvertiert. Die `GeoPoint` Klasse wird mit einem **[TypeConverter]** -Attribut versehen, um den Typkonverter anzugeben. (Dieses Beispiel wurde vom Blogbeitrag von Mike Stall [zum Binden an benutzerdefinierte Objekte in Aktions Signaturen in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)inspiriert.)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Nun wird die Web- `GeoPoint` API als einfacher Typ behandelt, d. h., `GeoPoint` es wird versucht, Parameter aus dem URI zu binden. Sie müssen **[fromuri]** nicht für den Parameter einschließen.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Der Client kann die-Methode mit einem URI wie dem folgenden aufrufen:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Modellbinder

Eine flexiblere Option als ein Typkonverter besteht darin, einen benutzerdefinierten Modell Binder zu erstellen. Mit einem Modell Binder haben Sie Zugriff auf Dinge wie die HTTP-Anforderung, die Aktions Beschreibung und die Rohwerte aus den Routendaten.

Um einen Modell Binder zu erstellen, implementieren Sie die **IModelBinder** -Schnittstelle. Diese Schnittstelle definiert eine einzige Methode, " **BindModel**":

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Hier ist ein Modell Binder für `GeoPoint` -Objekte.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Ein Modell Binder ruft roheingabe Werte von einem *Wert Anbieter*ab. Dieser Entwurf trennt zwei unterschiedliche Funktionen:

- Der Wert Anbieter nimmt die HTTP-Anforderung an und füllt ein Wörterbuch von Schlüssel-Wert-Paaren.
- Der Modell Binder verwendet dieses Wörterbuch zum Auffüllen des Modells.

Der Standardwert Anbieter in der Web-API ruft Werte aus den Routendaten und der Abfrage Zeichenfolge ab. Wenn der URI beispielsweise ist `http://localhost/api/values/1?location=48,-122`, erstellt der Wert Anbieter die folgenden Schlüssel-Wert-Paare:

- id = &quot;1&quot;
- Speicherort &quot;= 48.122&quot;

(Ich gehe von der Standardrouten Vorlage aus, die &quot;API/{Controller}/{ID}&quot;ist.)

Der Name des Parameters, der gebunden werden soll, wird in der **modelbindingcontext. modelname** -Eigenschaft gespeichert. Der Modell Binder sucht einen Schlüssel mit diesem Wert im Wörterbuch. Wenn der Wert vorhanden ist und in einen `GeoPoint`konvertiert werden kann, weist der Modell Binder den gebundenen Wert der **modelbindingcontext. Model** -Eigenschaft zu.

Beachten Sie, dass der Modell Binder nicht auf eine einfache Typkonvertierung beschränkt ist. In diesem Beispiel sucht der Modell Binder zuerst in einer Tabelle bekannter Speicherorte, und wenn dies nicht der Fall ist, wird die Typkonvertierung verwendet.

**Festlegen der Modell Bindung**

Es gibt mehrere Möglichkeiten, einen Modell Binder festzulegen. Zuerst können Sie dem-Parameter ein **[Modelbinder]** -Attribut hinzufügen.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Sie können dem Typ auch ein **[Modelbinder]** -Attribut hinzufügen. Die Web-API verwendet den angegebenen Modell Binder für alle Parameter dieses Typs.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Schließlich können Sie der **httpconfiguration**einen Modell Binder Anbieter hinzufügen. Ein Modell Binder Anbieter ist einfach eine Factoryklasse, die einen Modell Binder erstellt. Sie können einen Anbieter erstellen, indem Sie von der [modelbinderprovider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) -Klasse ableiten. Wenn Ihr Modell Binder jedoch einen einzelnen Typ behandelt, ist es einfacher, den integrierten **simplemodelbinderprovider**zu verwenden, der für diesen Zweck entwickelt wurde. Dies wird im folgenden Code veranschaulicht.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Bei einem Modell Bindungs Anbieter müssen Sie dem Parameter immer noch das Attribut **[Modelbinder]** hinzufügen, um der Web-API mitzuteilen, dass Sie einen Modell Binder und keinen Medientyp Formatierer verwenden soll. Jetzt müssen Sie jedoch nicht den Typ des Modell Binders im Attribut angeben:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Wert Anbieter

Ich habe erwähnt, dass ein Modell Binder Werte von einem Wert Anbieter abruft. Um einen benutzerdefinierten Wert Anbieter zu schreiben, implementieren Sie die **IValueProvider** -Schnittstelle. Es folgt ein Beispiel, das Werte aus den Cookies in der Anforderung abruft:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Sie müssen auch eine wertanbieterfactory erstellen, indem Sie von der **valueproviderfactory** -Klasse ableiten.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Fügen Sie der **httpconfiguration** die wertanbieterfactory wie folgt hinzu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Die Web-API verfasst alle Wert Anbieter, d. h., wenn ein Modell Binder " **valueprovider. GetValue**" aufruft, empfängt der Modell Binder den Wert des ersten Wert Anbieters, der ihn liefern kann.

Alternativ können Sie die wertanbieterfactory auf Parameter Ebene festlegen, indem Sie das **valueprovider** -Attribut wie folgt verwenden:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Dies weist die Web-API an, die Modell Bindung mit der angegebenen wertanbieterfactory zu verwenden und keinen der anderen registrierten Wert Anbieter zu verwenden.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Modell Binder sind eine bestimmte Instanz eines allgemeineren Mechanismus. Wenn Sie sich das **[Modelbinder]** -Attribut ansehen, sehen Sie, dass es von der abstrakten **parameterbindingattribute** -Klasse abgeleitet wird. Diese Klasse definiert eine einzelne Methode, **GetBinding**, die ein **httpparameterbinding** -Objekt zurückgibt:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Eine **httpparameterbinding** ist dafür verantwortlich, einen Parameter an einen Wert zu binden. Im Fall von **[Modelbinder]** gibt das Attribut eine **httpparameterbinding** -Implementierung zurück, die einen **IModelBinder** verwendet, um die tatsächliche Bindung auszuführen. Sie können auch Ihre eigene **httpparameterbinding**implementieren.

Nehmen Sie beispielsweise an, dass Sie Etags aus `if-match` - `if-none-match` und-Headern in der Anforderung erhalten möchten. Wir beginnen mit der Definition einer Klasse zur Darstellung von Etags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Wir definieren außerdem eine Enumeration, die angibt, ob das ETag aus dem `if-match` Header oder dem `if-none-match` Header erhalten werden soll.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Im folgenden finden Sie eine **httpparameterbinding** , die das ETag aus dem gewünschten Header abruft und an einen Parameter des Typs ETag bindet:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Die **executebindingasync** -Methode führt die Bindung aus. Fügen Sie in dieser Methode den gebundenen Parameterwert dem **Aktions Argument** -Wörterbuch in **httpactioncontext**hinzu.

> [!NOTE]
> Wenn die **executebindingasync** -Methode den Text der Anforderungs Nachricht liest, überschreiben Sie die **willread Body** -Eigenschaft, um true zurückzugeben. Bei dem Anforderungs Text kann es sich um einen nicht gepufferten Datenstrom handeln, der nur einmal gelesen werden kann, sodass die Web-API eine Regel erzwingt, dass höchstens eine Bindung den Nachrichtentext lesen kann.

Um ein benutzerdefiniertes **httpparameterbinding**-Attribut anzuwenden, können Sie ein Attribut definieren, das von **parameterbindingattribute**abgeleitet wird. Für `ETagParameterBinding`werden zwei Attribute definiert, eine für `if-match` Header und eine für `if-none-match` Header. Beide werden von einer abstrakten Basisklasse abgeleitet.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Im folgenden finden Sie eine Controller Methode, `[IfNoneMatch]` die das-Attribut verwendet.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Neben **parameterbindingattribute**gibt es einen anderen Hook zum Hinzufügen eines benutzerdefinierten **httpparameterbinding**-Objekts. Im **httpconfiguration** -Objekt ist die **parameterbindingrules** -Eigenschaft eine Auflistung anonymer Funktionen vom Typ (**httpparameterdescriptor**  - &gt; **httpparameterbinding**). Beispielsweise können Sie eine Regel hinzufügen, die von jedem ETag-Parameter einer get `ETagParameterBinding` - `if-none-match`Methode mit verwendet wird:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Die Funktion sollte für `null` Parameter zurückgeben, bei denen die Bindung nicht anwendbar ist.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Der gesamte Parameter Bindungsprozess wird durch einen austauschbaren Dienst ( **iaktionvaluebinder**) gesteuert. Die Standard Implementierung von **iaktionvaluebinder** führt Folgendes aus:

1. Suchen Sie nach einem **parameterbindingattribute** -Parameter. Dies umfasst **[frombody]** , **[fromuri]** und **[Modelbinder]** oder benutzerdefinierte Attribute.
2. Suchen Sie andernfalls in **httpconfiguration. parameterbindingrules** nach einer Funktion, die einen **httpparameterbinding**-Wert zurückgibt, der nicht NULL ist.
3. Verwenden Sie andernfalls die Standardregeln, die Sie zuvor beschrieben haben. 

    - Wenn der Parametertyp "Simple" oder ein Typkonverter ist, binden Sie den URI aus dem URI. Dies entspricht dem Platzieren des **[fromuri]** -Attributs für den Parameter.
    - Versuchen Sie andernfalls, den-Parameter aus dem Nachrichtentext zu lesen. Dies entspricht dem Einfügen von **[frombody]** für den Parameter.

Wenn Sie möchten, können Sie den gesamten **iaktionvaluebinder** -Dienst durch eine benutzerdefinierte Implementierung ersetzen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Beispiel für benutzerdefinierte Parameter Bindung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall hat eine gute Reihe von Blogbeiträgen zur Web-API-Parameter Bindung verfasst:

- [Funktionsweise der Parameter Bindung durch die Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [MVC-Stil Parameter Bindung für Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Binden an benutzerdefinierte Objekte in Aktions Signaturen in MVC/Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Erstellen eines benutzerdefinierten Wert Anbieters in der Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Web-API-Parameter Bindung im Hintergrund](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
