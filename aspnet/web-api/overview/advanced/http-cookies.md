---
uid: web-api/overview/advanced/http-cookies
title: HTTP-Cookies in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt das Senden und empfangen von HTTP-Cookies in der Web-API für ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449313"
---
# <a name="http-cookies-in-aspnet-web-api"></a>HTTP-Cookies in der ASP.NET-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird beschrieben, wie http-Cookies in der Web-API gesendet und empfangen werden.

## <a name="background-on-http-cookies"></a>Hintergrundinformationen zu http-Cookies

In diesem Abschnitt erhalten Sie einen kurzen Überblick darüber, wie Cookies auf der HTTP-Ebene implementiert werden. Weitere Informationen finden Sie in [RFC 6265](http://tools.ietf.org/html/rfc6265).

Ein Cookie ist ein Datenelement, das von einem Server in der HTTP-Antwort gesendet wird. Der Client (optional) speichert das Cookie und gibt es bei nachfolgenden Anforderungen zurück. Dies ermöglicht dem Client und dem Server das Freigeben des Zustands. Zum Festlegen eines Cookies enthält der Server einen Set-Cookie-Header in der Antwort. Das Format eines Cookies ist ein Name-Wert-Paar mit optionalen Attributen. Beispiel:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Im folgenden finden Sie ein Beispiel mit Attributen:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Zum Zurückgeben eines Cookies an den Server schließt der Client in späteren Anforderungen einen Cookie-Header ein.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Eine HTTP-Antwort kann mehrere Set-Cookie-Header enthalten.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Der Client gibt mehrere Cookies mithilfe eines einzelnen Cookieheaders zurück.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Der Gültigkeitsbereich und die Dauer eines Cookies werden durch folgende Attribute im Set-Cookie-Header gesteuert:

- **Domäne**: weist den Client an, welche Domäne das Cookie erhalten soll. Wenn die Domäne z. b. "example.com" ist, gibt der Client das Cookie an jede Unterdomäne von example.com zurück. Wenn nicht angegeben, ist die Domäne der Ursprungsserver.
- **Path**: schränkt das Cookie auf den angegebenen Pfad innerhalb der Domäne ein. Wenn nicht angegeben, wird der Pfad des Anforderungs-URI verwendet.
- **Läuft**ab: legt ein Ablaufdatum für das Cookie fest. Der Client löscht das Cookie, wenn es abläuft.
- **Max-age**: legt das maximale Alter für das Cookie fest. Der Client löscht das Cookie, wenn es das maximale Alter erreicht.

Wenn sowohl `Expires` als auch `Max-Age` festgelegt sind, hat `Max-Age` Vorrang. Wenn keines der beiden festgelegt ist, löscht der Client das Cookie, wenn die aktuelle Sitzung beendet wird. (Die genaue Bedeutung von "Session" wird vom Benutzer-Agent bestimmt.)

Beachten Sie jedoch, dass Clients Cookies ignorieren können. Beispielsweise kann ein Benutzer Cookies aus Datenschutzgründen deaktivieren. Clients können Cookies löschen, bevor Sie ablaufen, oder die Anzahl der gespeicherten Cookies einschränken. Aus Datenschutzgründen werden von Clients häufig Cookies von Drittanbietern abgelehnt, bei denen die Domäne nicht dem Ursprungsserver entspricht. Kurz gesagt, sollte der Server nicht darauf zurückgreifen, dass die von ihm festgelegt werden.

## <a name="cookies-in-web-api"></a>Cookies in der Web-API

Zum Hinzufügen eines Cookies zu einer HTTP-Antwort erstellen Sie eine **cookieheadervalue** -Instanz, die das Cookie darstellt. Dann rufe Sie die **addcookies** -Erweiterungsmethode auf, die im **System .net. http definiert ist. Httpresponsheadersextensions** -Klasse, um das Cookie hinzuzufügen.

Der folgende Code fügt z. b. ein Cookie innerhalb einer Controller Aktion hinzu:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Beachten Sie, dass **addcookies** ein Array von **cookieheadervalue** -Instanzen annimmt.

Um die Cookies aus einer Client Anforderung zu extrahieren, müssen Sie die **GetCookies** -Methode aufrufen:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Ein **cookieheadervalue** enthält eine Sammlung von **cookiestate** -Instanzen. Jede **cookiestate** stellt ein Cookie dar. Verwenden Sie die Indexer-Methode, um einen **cookiestate** nach dem Namen zu erhalten, wie hier gezeigt.

## <a name="structured-cookie-data"></a>Strukturierte Cookie-Daten

Viele Browser beschränken, wie viele Cookies sowohl die&#8212;Gesamtzahl als auch die Anzahl pro Domäne speichern werden. Daher kann es hilfreich sein, strukturierte Daten in einem einzigen Cookie zu platzieren, anstatt mehrere Cookies festzulegen.

> [!NOTE]
> RFC 6265 definiert nicht die Struktur von Cookie-Daten.

Mithilfe der **cookieheadervalue** -Klasse können Sie eine Liste von Name-Wert-Paaren für die Cookiedaten übergeben. Diese Name-Wert-Paare werden als URL-codierte Formulardaten im Set-Cookie-Header codiert:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Der vorherige Code erzeugt den folgenden Set-Cookie-Header:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

Die **cookiestate** -Klasse stellt eine Indexer-Methode bereit, um die untergeordneten Werte aus einem Cookie in der Anforderungs Nachricht zu lesen:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Beispiel: festlegen und Abrufen von Cookies in einem Nachrichten Handler

In den vorherigen Beispielen wurde gezeigt, wie Cookies in einem Web-API-Controller verwendet werden. Eine andere Möglichkeit ist die Verwendung von [Nachrichten Handlern](http-message-handlers.md). Meldungs Handler werden zuvor in der Pipeline aufgerufen als Controller. Ein Nachrichten Handler kann Cookies aus der Anforderung lesen, bevor die Anforderung den Controller erreicht, oder der Antwort Cookies hinzufügen, nachdem der Controller die Antwort generiert hat.

![](http-cookies/_static/image2.png)

Der folgende Code zeigt einen Nachrichten Handler zum Erstellen von Sitzungs-IDs. Die Sitzungs-ID wird in einem Cookie gespeichert. Der Handler überprüft die Anforderung für das Sitzungs Cookie. Wenn das Cookie nicht in der Anforderung enthalten ist, generiert der Handler eine neue Sitzungs-ID. In beiden Fällen speichert der Handler die Sitzungs-ID im **httprequestmessage. Properties-Eigenschaften** Behälter. Außerdem wird der HTTP-Antwort das Sitzungs Cookie hinzugefügt.

Diese Implementierung überprüft nicht, ob die Sitzungs-ID des Clients tatsächlich vom Server ausgegeben wurde. Verwenden Sie es nicht als Form der Authentifizierung. Der Punkt des Beispiels besteht darin, die HTTP-Cookie-Verwaltung anzuzeigen.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Ein Controller kann die Sitzungs-ID aus dem **httprequestmessage. Properties-Eigenschaften** Behälter erhalten.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
