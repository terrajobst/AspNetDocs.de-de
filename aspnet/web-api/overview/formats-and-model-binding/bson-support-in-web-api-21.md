---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Bson-Unterstützung in ASP.net-Web-API 2,1-ASP.NET 4. x
author: MikeWasson
description: zeigt, wie bson in einem Web-API-Controller (serverseitig) und in einer .NET-Client-App für ASP.NET 4. x verwendet wird.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504681"
---
# <a name="bson-support-in-aspnet-web-api-21"></a>Bson-Unterstützung in ASP.net-Web-API 2,1

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird gezeigt, wie Sie bson in Ihrem Web-API-Controller (serverseitig) und in einer .NET-Client-App verwenden. Die Web-API 2,1 führt die Unterstützung für bson ein. 

## <a name="what-is-bson"></a>Was ist bson?

[Bson](http://bsonspec.org/) ist ein binäres Serialisierungsformat. "Bson" steht für "Binary JSON", bson und JSON werden jedoch sehr unterschiedlich serialisiert. Bson ist "JSON-like", da Objekte ähnlich wie JSON als Name-Wert-Paare dargestellt werden. Im Gegensatz zu JSON werden numerische Datentypen als Bytes und nicht als Zeichen folgen gespeichert.

Bson war so konzipiert, dass es sich um eine einfache, leicht zu Scanende und schnelle Codierung/Decodierung handelt.

- Bson ist vergleichbar mit JSON. Je nach den Daten kann eine BSON-Nutzlast kleiner oder größer als eine JSON-Nutzlast sein. Zum Serialisieren von Binärdaten, wie z. b. einer Bilddatei, ist bson kleiner als JSON, da die Binärdaten nicht base64-codiert sind.
- Bson-Dokumente können leicht überprüft werden, da Elementen ein Längenfeld vorangestellt wird, sodass ein Parser Elemente überspringen kann, ohne Sie zu decodieren.
- Codierung und Decodierung sind effizient, da numerische Datentypen als Zahlen und nicht als Zeichen folgen gespeichert werden.

Native Clients, wie z. b. .NET-Client-apps, können von der Verwendung von bson anstelle von textbasierten Formaten wie JSON oder XML profitieren. Bei Browser Clients ist es wahrscheinlich, dass Sie sich auf JSON beschränken, da JavaScript die JSON-Nutzlast direkt konvertieren kann.

Glücklicherweise verwendet die Web-API [inhaltsaus](content-negotiation.md)Handlung, sodass Ihre API beide Formate unterstützen und den Client auswählen kann.

## <a name="enabling-bson-on-the-server"></a>Aktivieren von bson auf dem Server

Fügen Sie in Ihrer Web-API-Konfiguration den **bsonmediatyetformatter** der formatiererauflistung hinzu.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Wenn der Client nun "Application/bson" anfordert, verwendet die Web-API den bson-Formatierer.

Um bson anderen Medientypen zuzuordnen, fügen Sie Sie der supportedmediatypes-Auflistung hinzu. Mit dem folgenden Code wird "application/vnd. Configuration. config" zu den unterstützten Medientypen hinzugefügt:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>HTTP-Beispielsitzung

In diesem Beispiel verwenden wir die folgende Modell Klasse plus einen einfachen Web-API-Controller:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Ein Client sendet möglicherweise die folgende HTTP-Anforderung:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Hier ist die Antwort:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Hier habe ich die Binärdaten durch &quot;ersetzt.&quot; Zeichen. Der folgende Screenshot von "fddler" zeigt die unformatierten Hex-Werte.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Verwenden von bson mit httpclient

.NET-Client-Apps können den bson-Formatierer mit **HttpClient**verwenden. Weitere Informationen zu **HttpClient**finden Sie unter [Aufrufen einer Web-API von einem .NET-Client](../advanced/calling-a-web-api-from-a-net-client.md).

Der folgende Code sendet eine GET-Anforderung, die bson akzeptiert, und deserialisiert dann die bson-Nutzlast in der Antwort.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Um bson vom Server anzufordern, legen Sie den Accept-Header auf "Application/bson" fest:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Um den Antworttext zu deserialisieren, verwenden Sie den **bsonmediatypeer Formatter**. Dieser Formatierer ist nicht in der standardformatiererauflistung enthalten, daher müssen Sie ihn beim Lesen des Antwort Texts angeben:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Im nächsten Beispiel wird gezeigt, wie eine Post-Anforderung gesendet wird, die bson enthält.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Ein Großteil dieses Codes ist mit dem vorherigen Beispiel identisch. Geben Sie in der **postasync** -Methode jedoch **bsonmediatypeer Formatter** als Formatierer an:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serialisieren primitiver Typen der obersten Ebene

Jedes bson-Dokument ist eine Liste von Schlüssel-Wert-Paaren. Die bson-Spezifikation definiert keine Syntax zum Serialisieren eines einzelnen Rohwerts, wie z. b. eine ganze Zahl oder eine Zeichenfolge.

Um diese Einschränkung zu umgehen, behandelt **bsonmediatypformatter** primitive Typen als Sonderfall. Vor der Serialisierung konvertiert Sie den Wert in ein Schlüssel/Wert-Paar mit dem Schlüssel "Wert". Angenommen, Ihr API-Controller gibt eine ganze Zahl zurück:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Vor der Serialisierung konvertiert das bson-Formatierungs Programm dieses in das folgende Schlüssel-Wert-Paar:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Wenn Sie deserialisieren, konvertiert der Formatierer die Daten zurück in den ursprünglichen Wert. Clients, die einen anderen bson-Parser verwenden, müssen diesen Fall jedoch verarbeiten, wenn Ihre Web-API Rohwerte zurückgibt. Im Allgemeinen sollten Sie die Rückgabe strukturierter Daten anstelle von Rohwerten in Erwägung gezogen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Bson-Beispiel für Web-API](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[Medienformatierer](media-formatters.md)
