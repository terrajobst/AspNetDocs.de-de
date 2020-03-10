---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Signalr 1. x Hubs-API-Handbuch-JavaScript-Client | Microsoft-Dokumentation
author: bradygaster
description: Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für die signalr-Version 1,1 in JavaScript-Clients, z. b. Browser und Windows Store (winjs).
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431103"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a>Leitfaden für die SignalR 1.x-Hubs-API – JavaScript-Client

von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Dieses Dokument enthält eine Einführung in die Verwendung der Hubs-API für die signalr-Version 1,1 in JavaScript-Clients, z. b. Browser und Windows Store-Anwendungen (winjs).
> 
> Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server. In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden. Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden. Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.
> 
> Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird. Eine Einführung in signalr, Hubs und persistente Verbindungen oder ein Tutorial, das zeigt, wie Sie eine komplette signalr-Anwendung erstellen, finden Sie unter [signalr-Getting Started](../getting-started/index.md).

## <a name="overview"></a>Übersicht

Dieses Dokument enthält folgende Abschnitte:

- [Der generierte Proxy und dessen Funktionsweise](#genproxy)

    - [Verwendungszwecke des generierten Proxys](#cantusegenproxy)
- [Client Einrichtung](#clientsetup)

    - [Verweisen auf den dynamisch generierten Proxy](#dynamicproxy)
    - [Erstellen einer physischen Datei für den von signalr generierten Proxy](#manualproxy)
- [Einrichten einer Verbindung](#establishconnection)

    - [$. Connection. Hub ist das gleiche Objekt, das von $. hubconnection () erstellt wird.](#connequivalence)
    - [Asynchrone Ausführung der Start Methode](#asyncstart)
- [Einrichten einer Domänen übergreifenden Verbindung](#crossdomain)
- [Konfigurieren der Verbindung](#configureconnection)

    - [Angeben von Abfrage Zeichen folgen Parametern](#querystring)
    - [Angeben der Transportmethode](#transport)
- [Vorgehensweise beim erhalten eines Proxys für eine Hub-Klasse](#getproxy)
- [Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann](#callclient)
- [Vorgehensweise beim Abrufen von Server Methoden vom Client](#callserver)
- [Behandeln von Ereignissen der Verbindungs Lebensdauer](#connectionlifetime)
- [Behandeln von Fehlern](#handleerrors)
- [Aktivieren der Client seitigen Protokollierung](#logging)

Dokumentation zum Programmieren der Server-oder .NET-Clients finden Sie in den folgenden Ressourcen:

- [Leitfaden für signalr-Hubs-API-Server](../guide-to-the-api/hubs-api-guide-server.md)
- [Leitfaden für signalr-Hubs-API: .NET-Client](../guide-to-the-api/hubs-api-guide-net-client.md)

Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API. Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Der generierte Proxy und dessen Funktionsweise

Sie können einen JavaScript-Client programmieren, um mit einem signalr-Dienst mit oder ohne einen Proxy zu kommunizieren, den signalr für Sie generiert. Der Proxy für Sie vereinfacht die Syntax des Codes, den Sie verwenden, um eine Verbindung herzustellen, Methoden zu schreiben, die vom Server aufgerufen werden, und Methoden auf dem Server aufzurufen.

Wenn Sie Code zum Abrufen von Server Methoden schreiben, können Sie mit dem generierten Proxy die Syntax verwenden, die so aussieht, als ob Sie eine lokale Funktion ausgeführt haben: Sie können `serverMethod(arg1, arg2)` statt `invoke('serverMethod', arg1, arg2)`schreiben. Die generierte Proxy Syntax ermöglicht außerdem einen sofortigen und verständlichen Client seitigen Fehler, wenn Sie einen Server Methodennamen falsch formatieren. Wenn Sie die Datei, die die Proxys definiert, manuell erstellen, können Sie auch IntelliSense-Unterstützung für das Schreiben von Code zum Aufrufen von Server Methoden erhalten.

Angenommen, Sie haben die folgende Hub-Klasse auf dem Server:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

In den folgenden Codebeispielen wird veranschaulicht, wie JavaScript-Code zum Aufrufen der `NewContosoChatMessage`-Methode auf dem Server und zum Empfangen von Aufrufen der `addContosoChatMessageToPage` Methode vom Server aussieht.

**Mit dem generierten Proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Ohne den generierten Proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Verwendungszwecke des generierten Proxys

Wenn Sie mehrere Ereignishandler für eine Client Methode registrieren möchten, die vom Server aufgerufen wird, können Sie den generierten Proxy nicht verwenden. Andernfalls können Sie auswählen, ob der generierte Proxy verwendet werden soll oder nicht, basierend auf der Codierungs Einstellung. Wenn Sie die Option nicht verwenden möchten, müssen Sie nicht auf die "signalr/Hubs"-URL in einem `script`-Element in Ihrem Client Code verweisen.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Clientsetup

Ein JavaScript-Client erfordert Verweise auf jQuery und die signalr Core JavaScript-Datei. Die jQuery-Version muss 1.6.4 oder größere spätere Versionen sein, z. b. 1.7.2, 1.8.2 oder 1.9.1. Wenn Sie sich für die Verwendung des generierten Proxys entscheiden, benötigen Sie auch einen Verweis auf die JavaScript-Datei des signalr-generierten Proxys. Im folgenden Beispiel wird gezeigt, wie die Verweise in einer HTML-Seite aussehen können, die den generierten Proxy verwendet.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Diese Verweise müssen in dieser Reihenfolge enthalten sein: zuerst jQuery, signalr Core und schließlich signalr-Proxys.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Verweisen auf den dynamisch generierten Proxy

Im vorangehenden Beispiel besteht der Verweis auf den von signalr generierten Proxy in der dynamischen Generierung von JavaScript-Code und nicht in einer physischen Datei. Signalr erstellt den JavaScript-Code für den Proxy im Handumdrehen und bedient ihn als Reaktion auf die URL "/signalr/Hubs" an den Client. Wenn Sie in der `MapHubs`-Methode eine andere Basis-URL für signalr-Verbindungen auf dem Server angegeben haben, ist die URL für die dynamisch generierte Proxy Datei Ihre benutzerdefinierte URL, an die "/Hubs" angehängt ist.

> [!NOTE]
> Verwenden Sie für Windows 8-JavaScript-Clients (Windows Store) die physische Proxy Datei anstelle der dynamisch generierten. Weitere Informationen finden Sie weiter unten in diesem Thema unter Vorgehens [Weise beim Erstellen einer physischen Datei für den signalr-generierten Proxy](#manualproxy) .

Verwenden Sie in einer ASP.NET MVC 4 Razor-Ansicht die Tilde, um auf den Anwendungs Stamm in der Proxy Dateireferenz zu verweisen:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Weitere Informationen zur Verwendung von signalr in MVC 4 finden Sie unter Einführung in [signalr und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Verwenden Sie in einer ASP.NET MVC 3 Razor-Ansicht `Url.Content` für den Proxy Datei Verweis:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Verwenden Sie in einer ASP.net-Web Forms Anwendung `ResolveClientUrl` für Ihren proxydateiverweis, oder registrieren Sie ihn über den ScriptManager mithilfe eines relativen Pfads (beginnend mit einer Tilde):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Verwenden Sie als allgemeine Regel dieselbe Methode zum Angeben der URL "/signalr/Hubs", die Sie für CSS-oder JavaScript-Dateien verwenden. Wenn Sie eine URL ohne eine Tilde angeben, funktioniert die Anwendung in einigen Szenarios ordnungsgemäß, wenn Sie in Visual Studio mit IIS Express testen, schlägt jedoch mit einem 404-Fehler fehl, wenn Sie die Bereitstellung für vollständiges IIS ausführen. Weitere Informationen finden Sie unter **Auflösen von Verweisen auf Ressourcen** auf Stamm Ebene in [Webservern in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx) auf der MSDN-Website.

Wenn Sie ein Webprojekt in Visual Studio 2012 im Debugmodus ausführen und Internet Explorer als Browser verwenden, können Sie die Proxy Datei in **Projektmappen-Explorer** unter **Skript Dokumenten**sehen, wie in der folgenden Abbildung dargestellt.

![Von JavaScript generierte Proxy Datei in Projektmappen-Explorer](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Um den Inhalt der Datei anzuzeigen, doppelklicken Sie auf **Hubs**. Wenn Sie nicht Visual Studio 2012 und Internet Explorer verwenden oder sich nicht im Debugmodus befinden, können Sie auch den Inhalt der Datei durch Navigieren zur URL "/signalR/Hubs" erhalten. Wenn Ihre Website beispielsweise auf `http://localhost:56699`ausgeführt wird, navigieren Sie in Ihrem Browser zu `http://localhost:56699/SignalR/hubs`.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Erstellen einer physischen Datei für den von signalr generierten Proxy

Als Alternative zum dynamisch generierten Proxy können Sie eine physische Datei mit dem Proxycode erstellen und auf diese Datei verweisen. Dies kann zum Steuern der Zwischenspeicherung oder zum Bündeln von Verhalten oder zum Aufrufen von IntelliSense bei der Codierung von Aufrufen von Server Methoden verwendet werden.

Führen Sie die folgenden Schritte aus, um eine Proxy Datei zu erstellen:

1. Installieren Sie das nuget-Paket [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner *Tools* , der die Datei signalr. exe enthält. Der Ordner Tools befindet sich an folgendem Speicherort:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Geben Sie folgenden Befehl ein:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Der Pfad zu Ihrer *dll* ist in der Regel der Ordner " *bin* " im Projektordner.

    Dieser Befehl erstellt eine Datei namens " *Server. js* " im gleichen Ordner wie " *signalr. exe*".
4. Platzieren Sie die Datei " *Server. js* " in einem entsprechenden Ordner in Ihrem Projekt, benennen Sie Sie entsprechend Ihrer Anwendung um, und fügen Sie anstelle der Referenz "signalr/Hubs" einen Verweis darauf ein.

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Einrichten einer Verbindung

Bevor Sie eine Verbindung herstellen können, müssen Sie ein Verbindungs Objekt erstellen, einen Proxy erstellen und Ereignishandler für Methoden registrieren, die vom Server aufgerufen werden können. Stellen Sie beim Einrichten des Proxy-und Ereignishandler die Verbindung her, indem Sie die `start`-Methode aufrufen.

Wenn Sie den generierten Proxy verwenden, müssen Sie das Verbindungs Objekt nicht in Ihrem eigenen Code erstellen, da der generierte Proxy Code dies für Sie erledigt.

<a id="nogenconnection"></a>

**Herstellen einer Verbindung (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Herstellen einer Verbindung (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen. Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Normalerweise registrieren Sie Ereignishandler, bevor Sie die `start`-Methode aufrufen, um die Verbindung herzustellen. Wenn Sie einige Ereignishandler registrieren möchten, nachdem Sie die Verbindung hergestellt haben, können Sie dies tun. Sie müssen jedoch mindestens einen ihrer Ereignishandler registrieren, bevor Sie die `start`-Methode aufrufen. Ein Grund hierfür ist, dass es möglicherweise viele Hubs in einer Anwendung gibt, aber Sie sollten das `OnConnected`-Ereignis nicht auf jedem Hub auslassen, wenn Sie nur eine von Ihnen verwenden möchten. Wenn die Verbindung hergestellt wird, weist das vorhanden sein einer Client Methode auf dem Proxy eines Hubs an, dass signalr das `OnConnected` Ereignis auslöst. Wenn Sie keine Ereignishandler registrieren, bevor Sie die `start`-Methode aufrufen, können Sie Methoden für den Hub aufrufen, aber die `OnConnected`-Methode des Hubs wird nicht aufgerufen, und es werden keine Client Methoden vom Server aufgerufen.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub ist das gleiche Objekt, das von $. hubconnection () erstellt wird.

Wie Sie in den Beispielen sehen können, verweist `$.connection.hub` auf das Verbindungs Objekt, wenn Sie den generierten Proxy verwenden. Dabei handelt es sich um dasselbe Objekt, das Sie durch Aufrufen von `$.hubConnection()` erhalten, wenn Sie den generierten Proxy nicht verwenden. Der generierte Proxy Code erstellt die Verbindung für Sie durch Ausführen der folgenden Anweisung:

![Erstellen einer Verbindung in der generierten Proxy Datei](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Wenn Sie den generierten Proxy verwenden, können Sie mit `$.connection.hub` arbeiten, die Sie mit einem Verbindungs Objekt ausführen können, wenn Sie den generierten Proxy nicht verwenden.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Asynchrone Ausführung der Start Methode

Die `start`-Methode wird asynchron ausgeführt. Sie gibt ein [Verzögertes jQuery-Objekt](http://api.jquery.com/category/deferred-object/)zurück. Dies bedeutet, dass Sie Rückruf Funktionen hinzufügen können, indem Sie Methoden wie `pipe`, `done`und `fail`aufrufen. Wenn Sie über Code verfügen, der nach dem Herstellen der Verbindung ausgeführt werden soll, z. b. einen Server Methodenaufrufe, fügen Sie diesen Code in eine Rückruffunktion ein, oder rufen Sie ihn über eine Rückruffunktion auf. Die `.done` Rückruf Methode wird ausgeführt, nachdem die Verbindung hergestellt wurde, und nach dem Ausführen von Code, den Sie in ihrer `OnConnected` Ereignishandlermethode auf dem Server haben, wird die Ausführung beendet.

Wenn Sie die "jetzt verbundene"-Anweisung aus dem vorherigen Beispiel als nächste Codezeile nach dem `start`-Methoden Aufrufs (nicht in einem `.done`-Rückruf) ablegen, wird die `console.log` Zeile vor dem Herstellen der Verbindung ausgeführt, wie im folgenden Beispiel gezeigt:

![Falsche Methode zum Schreiben von Code, der nach dem Herstellen der Verbindung ausgeführt wird.](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Einrichten einer Domänen übergreifenden Verbindung

Wenn der Browser eine Seite aus `http://contoso.com`lädt, befindet sich die signalr-Verbindung in der Regel in der gleichen Domäne `http://contoso.com/signalr`. Wenn die Seite aus `http://contoso.com` eine Verbindung mit `http://fabrikam.com/signalr`herstellt, handelt es sich um eine Domänen übergreifende Verbindung. Aus Sicherheitsgründen sind Domänen übergreifende Verbindungen standardmäßig deaktiviert. Stellen Sie sicher, dass Domänen übergreifende Verbindungen auf dem Server aktiviert sind, und geben Sie die Verbindungs-URL an, wenn Sie das Verbindungs Objekt erstellen, um eine Domänen übergreifende Verbindung herzustellen. Signalr verwendet die geeignete Technologie für Domänen übergreifende Verbindungen, z. b. [JSONP](http://en.wikipedia.org/wiki/JSONP) oder [cors](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Aktivieren Sie auf dem-Server Domänen übergreifende Verbindungen, indem Sie diese Option auswählen, wenn Sie die `MapHubs`-Methode aufgerufen haben.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

Geben Sie auf dem Client die URL an, wenn Sie das Verbindungs Objekt (ohne den generierten Proxy) erstellen oder bevor Sie die Start Methode (mit dem generierten Proxy) aufzurufen.

**Client Code, der eine Domänen übergreifende Verbindung (mit dem generierten Proxy) angibt**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Client Code, der eine Domänen übergreifende Verbindung angibt (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Wenn Sie den `$.hubConnection`-Konstruktor verwenden, müssen Sie `signalr` nicht in die URL einschließen, da er automatisch hinzugefügt wird (es sei denn, Sie geben `useDefaultUrl` als `false`an).

Sie können mehrere Verbindungen mit unterschiedlichen Endpunkten erstellen.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Legen Sie im Code nicht `jQuery.support.cors` auf "true" fest.
> 
>     ![Legen Sie jQuery. Support. cors nicht auf "true" fest.](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     Signalr übernimmt die Verwendung von JSONP oder cors. Wenn Sie `jQuery.support.cors` auf true festlegen, wird JSONP deaktiviert, da signalr davon ausgeht, dass der Browser cors unterstützt.
> - Wenn Sie eine Verbindung mit einer localhost-URL herstellen, wird Sie von Internet Explorer 10 nicht als Domänen übergreifende Verbindung betrachtet, sodass die Anwendung lokal mit IE 10 funktioniert, auch wenn Sie keine Domänen übergreifenden Verbindungen auf dem Server aktiviert haben.
> - Informationen zur Verwendung von Domänen übergreifenden Verbindungen mit Internet Explorer 9 finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Weitere Informationen zur Verwendung Domänen übergreifender Verbindungen mit Chrome finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen. Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Konfigurieren der Verbindung

Bevor Sie eine Verbindung herstellen, können Sie Abfrage Zeichen folgen Parameter angeben oder die Transportmethode angeben.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Angeben von Abfrage Zeichen folgen Parametern

Wenn Sie Daten an den Server senden möchten, wenn der Client eine Verbindung herstellt, können Sie dem Verbindungs Objekt Abfrage Zeichenfolgen-Parameter hinzufügen. In den folgenden Beispielen wird gezeigt, wie Sie einen Abfrage Zeichen folgen Parameter im Client Code festlegen.

**Legen Sie vor dem Aufrufen der Start Methode (mit dem generierten Proxy) einen Abfrage Zeichen folgen Wert fest.**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Legen Sie vor dem Aufrufen der Start Methode (ohne den generierten Proxy) einen Abfrage Zeichen folgen Wert fest.**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Servercode gelesen wird.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Angeben der Transportmethode

Im Rahmen der Verbindungs Herstellung aushandiert ein signalr-Client normalerweise mit dem Server, um den optimalen Transport zu ermitteln, der von Server und Client unterstützt wird. Wenn Sie bereits wissen, welcher Transport Sie verwenden möchten, können Sie diesen Aushandlungs Prozess umgehen, indem Sie die Transportmethode angeben, wenn Sie die `start`-Methode aufzurufen.

**Client Code, der die Transportmethode angibt (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Client Code, der die Transportmethode angibt (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Als Alternative können Sie mehrere Transportmethoden in der Reihenfolge angeben, in der Sie von signalr ausprobiert werden sollen:

**Client Code, der ein benutzerdefiniertes Transport Fall Back Schema (mit dem generierten Proxy) angibt**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Client Code, der ein benutzerdefiniertes Transport Fall Back Schema (ohne den generierten Proxy) angibt**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Sie können die folgenden Werte verwenden, um die Transportmethode anzugeben:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longabruf"

In den folgenden Beispielen wird gezeigt, wie Sie herausfinden, welche Transportmethode von einer Verbindung verwendet wird.

**Client Code, der die Transportmethode anzeigt, die von einer Verbindung verwendet wird (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Client Code, der die Transportmethode anzeigt, die von einer Verbindung verwendet wird (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Informationen zum Überprüfen der Transportmethode im Servercode finden Sie unter [ASP.net signalr Hubs API Guide-Server-How to Get Information about the Client from the context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Weitere Informationen zu Transporten und Fallbacks finden Sie unter [Einführung in signalr-Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Vorgehensweise beim erhalten eines Proxys für eine Hub-Klasse

Jedes von Ihnen erstellte Verbindungs Objekt kapselt Informationen zu einer Verbindung mit einem signalr-Dienst, der eine oder mehrere Hub-Klassen enthält. Um mit einer Hub-Klasse zu kommunizieren, verwenden Sie ein Proxy Objekt, das Sie selbst erstellen (wenn Sie den generierten Proxy nicht verwenden) oder das für Sie generiert wird.

Auf dem Client ist der Name des Proxys eine Camel-/schreibversion des Namens der Hub-Klasse. Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.

**Hub-Klasse auf dem Server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Beziehen eines Verweises auf den generierten Client Proxy für den Hub**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Erstellen eines Client Proxys für die Hub-Klasse (ohne generierten Proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Wenn Sie Ihre Hub-Klasse mit einem `HubName` Attribut ergänzen, verwenden Sie den genauen Namen, ohne den Fall zu ändern.

**Hubklasse auf Server mit hubname-Attribut**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Beziehen eines Verweises auf den generierten Client Proxy für den Hub**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Erstellen eines Client Proxys für die Hub-Klasse (ohne generierten Proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann

Um eine Methode zu definieren, die vom Server von einem Hub aufgerufen werden kann, fügen Sie dem Hub-Proxy einen Ereignishandler hinzu, indem Sie die `client`-Eigenschaft des generierten Proxys verwenden oder die `on`-Methode aufzurufen, wenn Sie den generierten Proxy nicht verwenden. Die Parameter können komplexe Objekte sein.

Fügen Sie den-Ereignishandler hinzu, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen. (Wenn Sie Ereignishandler hinzufügen möchten, nachdem Sie die `start`-Methode aufgerufen haben, lesen Sie den Hinweis unter [Herstellen einer Verbindung weiter](#establishconnection) oben in diesem Dokument, und verwenden Sie die Syntax, die zum Definieren einer Methode ohne Verwendung des generierten Proxys angezeigt wird.)

Beim Methodennamen wird die Groß-/Kleinschreibung beachtet. Beispielsweise werden auf dem-Server `Clients.All.addContosoChatMessageToPage` auf dem-Server `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`oder `addcontosochatmessagetopage` auf dem Client ausgeführt.

**Methode auf dem Client definieren (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Alternative Methode zum Definieren der Methode auf dem Client (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Methode auf dem Client definieren (ohne den generierten Proxy oder beim Hinzufügen nach dem Aufruf der Start-Methode)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Server Code, der die Client Methode aufruft**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Die folgenden Beispiele enthalten ein komplexes Objekt als Methoden Parameter.

**Methode auf dem Client definieren, die ein komplexes Objekt annimmt (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Methode auf dem Client definieren, die ein komplexes Objekt annimmt (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Server Code, der das komplexe Objekt definiert**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Server Code, der die Client Methode mithilfe eines komplexen Objekts aufruft**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Vorgehensweise beim Abrufen von Server Methoden vom Client

Um eine Server Methode vom Client aufzurufen, verwenden Sie die `server`-Eigenschaft des generierten Proxys oder die `invoke`-Methode des Hub-Proxys, wenn Sie den generierten Proxy nicht verwenden. Der Rückgabewert oder die Parameter können komplexe Objekte sein.

Übergeben Sie eine Camel-Case-Version des Methoden namens auf dem Hub. Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.

In den folgenden Beispielen wird veranschaulicht, wie eine Server Methode aufgerufen wird, die keinen Rückgabewert hat, und wie eine Server Methode aufgerufen wird, die über einen Rückgabewert verfügt.

**Server Methode ohne hubmethodname-Attribut**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Server Code, der das komplexe Objekt definiert, das in einem Parameter übergeben wird**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Wenn Sie die Hub-Methode mit einem `HubMethodName`-Attribut versehen haben, verwenden Sie diesen Namen, ohne den Fall zu ändern.

**Server Methode** mit einem hubmethodname-Attribut

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

In den vorangehenden Beispielen wird gezeigt, wie eine Server Methode aufgerufen wird, die über keinen Rückgabewert verfügt. In den folgenden Beispielen wird gezeigt, wie eine Server Methode aufgerufen wird, die über einen Rückgabewert verfügt.

**Server Code für eine Methode, die über einen Rückgabewert verfügt**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Die für den Rückgabewert verwendete Aktienklasse** .

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Behandeln von Ereignissen der Verbindungs Lebensdauer

Signalr stellt die folgenden Ereignisse für die Verbindungs Lebensdauer bereit, die Sie behandeln können:

- `starting`: wird ausgelöst, bevor Daten über die Verbindung gesendet werden.
- `received`: wird ausgelöst, wenn Daten über die Verbindung empfangen werden. Stellt die empfangenen Daten bereit.
- `connectionSlow`: wird ausgelöst, wenn der Client eine langsame oder häufig gelöschter Verbindung erkennt.
- `reconnecting`: wird ausgelöst, wenn der zugrunde liegende Transport mit dem erneuten Verbinden beginnt.
- `reconnected`: wird ausgelöst, wenn der zugrunde liegende Transport die Verbindung wieder hergestellt hat.
- `stateChanged`: wird ausgelöst, wenn sich der Verbindungsstatus ändert. Stellt den alten und den neuen Status bereit (Verbindung, verbunden, wiederherstellen oder Verbindung getrennt).
- `disconnected`: wird ausgelöst, wenn die Verbindung getrennt wurde.

Wenn Sie z. b. Warnmeldungen anzeigen möchten, wenn Verbindungsprobleme auftreten, die zu spürbaren Verzögerungen führen können, behandeln Sie das `connectionSlow`-Ereignis.

**Behandeln des connectionslow-Ereignisses (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Behandeln des connectionslow-Ereignisses (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Behandeln von Fehlern

Der signalr-JavaScript-Client stellt ein `error` Ereignis bereit, für das Sie einen Handler hinzufügen können. Sie können auch die Fail-Methode verwenden, um einen Handler für Fehler hinzuzufügen, die sich aus einem Aufruf der Server Methode ergeben.

Wenn Sie auf dem Server nicht explizit detaillierte Fehlermeldungen aktivieren, enthält das Ausnahme Objekt, das signalr nach einem Fehler zurückgibt, nur minimale Informationen über den Fehler. Wenn beispielsweise ein `newContosoChatMessage` Fehler auftritt, enthält die Fehlermeldung im Fehler Objekt den Wert "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`", dass detaillierte Fehlermeldungen an Clients in der Produktionsumgebung nicht gesendet werden sollen. Wenn Sie jedoch ausführliche Fehlermeldungen zu Problem Behandlungszwecken aktivieren möchten, verwenden Sie den folgenden Code auf dem Server.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Im folgenden Beispiel wird gezeigt, wie Sie einen Handler für das Fehler Ereignis hinzufügen.

**Hinzufügen eines Fehler Handlers (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Hinzufügen eines Fehler Handlers (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Im folgenden Beispiel wird gezeigt, wie ein Fehler von einem Methodenaufruf behandelt wird.

**Behandeln eines Fehlers von einem Methodenaufruf (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Behandeln eines Fehlers von einem Methodenaufruf (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Wenn ein Methodenaufruf fehlschlägt, wird das `error`-Ereignis ebenfalls ausgelöst, sodass der Code im `error` Methoden Handler und im `.fail`-Methoden Rückruf ausgeführt wird.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Aktivieren der Client seitigen Protokollierung

Um die Client seitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging`-Eigenschaft für das Verbindungs Objekt fest, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen.

**Aktivieren der Protokollierung (mit dem generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Aktivieren der Protokollierung (ohne den generierten Proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Um die Protokolle anzuzeigen, öffnen Sie die Entwicklertools Ihres Browsers, und navigieren Sie zur Registerkarte Konsole. Ein Tutorial mit Schritt-für-Schritt-Anweisungen und Screenshots, die die Vorgehensweise veranschaulichen, finden Sie unter [Server Broadcast mit ASP.net signalr-enable Logging](index.md).
