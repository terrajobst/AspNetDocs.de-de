---
uid: web-api/overview/advanced/httpclient-message-handlers
title: HttpClient-Meldungs Handler in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Erstellen von benutzerdefinierten Meldungs Handlern für ASP.net-Web-API in ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449277"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a>HttpClient-Meldungs Handler in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Ein *Nachrichten Handler* ist eine Klasse, die eine HTTP-Anforderung empfängt und eine HTTP-Antwort zurückgibt.

In der Regel werden eine Reihe von Meldungs Handlern verkettet. Der erste Handler empfängt eine HTTP-Anforderung, verarbeitet einige Verarbeitungsschritte und übergibt die Anforderung an den nächsten Handler. An einem bestimmten Punkt wird die Antwort erstellt, und die Kette wird wieder hergestellt. Dieses Muster wird als *delegier ender* Handler bezeichnet.

![](httpclient-message-handlers/_static/image1.png)

Auf der Clientseite verwendet die **HttpClient** -Klasse einen Meldungs Handler zum Verarbeiten von Anforderungen. Der Standard Handler ist **httpclienthandler**, der die Anforderung über das Netzwerk sendet und die Antwort vom Server erhält. Sie können benutzerdefinierte Meldungs Handler in die Client Pipeline einfügen:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.net-Web-API verwendet auch Nachrichten Handler auf der Serverseite. Weitere Informationen finden Sie unter [http-Nachrichten Handler](http-message-handlers.md).

## <a name="custom-message-handlers"></a>Benutzerdefinierte Meldungs Handler

Zum Schreiben eines benutzerdefinierten Nachrichten Handlers leiten Sie von **System .net. http. delegatinghandler** ab und überschreiben die **SendAsync** -Methode. Hier die Signatur der Methode:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Die Methode nimmt eine **httprequestmessage** als Eingabe an und gibt asynchron eine **httpresponsmessage**zurück. Eine typische-Implementierung führt Folgendes aus:

1. Verarbeiten Sie die Anforderungs Nachricht.
2. Ruft `base.SendAsync` auf, um die Anforderung an den inneren Handler zu senden.
3. Der innere Handler gibt eine Antwortnachricht zurück. (Dieser Schritt ist asynchron.)
4. Verarbeiten Sie die Antwort und geben Sie Sie an den Aufrufer zurück.

Das folgende Beispiel zeigt einen Nachrichten Handler, der der ausgehenden Anforderung einen benutzerdefinierten Header hinzufügt:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Der Aufruf von `base.SendAsync` ist asynchron. Wenn der Handler nach diesem-Befehl eine beliebige Aufgabe ausführt, verwenden Sie das **Erwartungs Wort, um die Ausführung** nach Abschluss der-Methode fortzusetzen. Das folgende Beispiel zeigt einen Handler, der Fehlercodes protokolliert. Die Protokollierung selbst ist nicht sehr interessant, aber das Beispiel zeigt, wie Sie die Antwort innerhalb des Handlers erhalten.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Hinzufügen von Meldungs Handlern zur Client Pipeline

Verwenden Sie die **httpclientfactory. Create** -Methode, um **HttpClient**benutzerdefinierte Handler hinzuzufügen:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Meldungs Handler werden in der Reihenfolge aufgerufen, in der Sie Sie an die **Create** -Methode übergeben. Da Handler Schaltflächen sind, wird die Antwortnachricht in der anderen Richtung bewegt. Das heißt, der letzte Handler ist der erste, der die Antwortnachricht erhält.
