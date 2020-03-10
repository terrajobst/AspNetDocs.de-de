---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Verwenden von asynchronen Methoden in ASP.NET 4,5 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer asynchronen ASP.net Web Forms Anwendung mit Visual Studio Express 2012 für das Web, die kostenlos ist...
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 7abc3d7acc60d7d868958f2a313bc408f96c95a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507165"
---
# <a name="using-asynchronous-methods-in-aspnet-45"></a>Verwenden asynchroner Methoden in ASP.NET 4.5

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer asynchronen ASP.net Web Forms-Anwendung mit [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11), eine kostenlose Version von Microsoft Visual Studio. Sie können auch [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)verwenden. Die folgenden Abschnitte sind in diesem Tutorial enthalten.
> 
> - [Verarbeitung von Anforderungen durch den Thread Pool](#HowRequestsProcessedByTP)
> - [Auswählen von synchronen oder asynchronen Methoden](#ChoosingSyncVasync)
> - [Die Beispielanwendung](#SampleApp)
> - [Die synchrone Gizmos-Seite](#GizmosSynch)
> - [Erstellen einer asynchronen Gizmos-Seite](#CreatingAsynchGizmos)
> - [Paralleles Ausführen mehrerer Vorgänge](#Parallel)
> - [Verwenden eines Abbruch Tokens](#CancelToken)
> - [Server Konfiguration für Webdienst Aufrufe mit hoher Parallelität/hoher Latenz](#ServerConfig)
> 
> Ein umfassendes Beispiel für dieses Tutorial finden Sie unter  
> [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) auf der [GitHub](https://github.com/) -Website.

ASP.NET 4,5 Web Pages in Kombination mit [.NET 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) können Sie asynchrone Methoden registrieren, die ein Objekt des Typs " [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)" zurückgeben. In .NET Framework 4 wurde ein asynchrones Programmier Konzept eingeführt, das als [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) bezeichnet wird, und ASP.NET 4,5 unterstützt [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Tasks werden durch den **Tasktyp und** verwandte Typen im [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) -Namespace dargestellt. Der .NET Framework 4,5 baut auf dieser asynchronen Unterstützung mit den Schlüsselwörtern " [Erwartung](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) " und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) " auf, die das Arbeiten mit [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Objekten wesentlich weniger komplex machen als vorherige asynchrone Ansätze. Das [Wait](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort ist syntaktische Kurzwort, um anzugeben, dass ein Code Element asynchron auf einen anderen Code Abschnitt warten soll. Das [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort stellt einen Hinweis dar, den Sie verwenden können, um Methoden als aufgabenbasierte asynchrone Methoden zu markieren. Die Kombination aus " **warten**", " **Async**" und " **Task** " erleichtert Ihnen das Schreiben von asynchronem Code in .NET 4,5. Das neue Modell für asynchrone Methoden wird als *Task basiertes asynchrones Muster* (**Tap**) bezeichnet. In diesem Tutorial wird davon ausgegangen, dass Sie mit der asynchronen Programmierung mit den Schlüsselwörtern " [Erwartung](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) " und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) " und dem [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace vertraut sind

Weitere Informationen [zum Verwenden der](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwörter "References" und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) " und des [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace finden Sie in den folgenden verweisen.

- [Whitepaper: Asynchronität in .net](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Häufig gestellte Fragen zu Async/warten](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchrone Programmierung in Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Verarbeitung von Anforderungen durch den Thread Pool

Auf dem Webserver verwaltet das .NET Framework einen Thread Pool, der zum Service ASP.NET-Anforderungen verwendet wird. Wenn eine Anforderung eingeht, wird ein Thread aus dem Pool verteilt, um diese Anforderung zu verarbeiten. Wenn die Anforderung synchron verarbeitet wird, ist der Thread, der die Anforderung verarbeitet, ausgelastet, während die Anforderung verarbeitet wird, und dieser Thread kann keine andere Anforderung verarbeiten.   
  
Dies stellt möglicherweise kein Problem dar, da der Thread Pool groß genug gemacht werden kann, um viele ausgelastete Threads aufnehmen zu können. Die Anzahl der Threads im Thread Pool ist jedoch begrenzt (der Standardwert für .NET 4,5 ist 5.000). In großen Anwendungen mit hoher Parallelität von Anforderungen mit langer Ausführungszeit sind möglicherweise alle verfügbaren Threads ausgelastet. Diese Bedingung wird als Thread Hunger bezeichnet. Wenn diese Bedingung erreicht wird, werden Anforderungen vom Webserver in die Warteschlange eingereiht. Wenn die Anforderungs Warteschlange voll ist, lehnt der Webserver Anforderungen mit einem HTTP 503-Status (Server ist zu stark ausgelastet) ab. Der CLR-Thread Pool weist Einschränkungen bei neuen Thread Spritzen auf. Wenn die Parallelität burstig ist (d. h., Ihre Website kann plötzlich eine große Anzahl von Anforderungen erhalten), und alle verfügbaren Anforderungs Threads sind aufgrund von Back-End-aufrufen mit hoher Latenzzeit ausgelastet. die eingeschränkte Thread einschleusungs Rate kann dazu führen, dass Ihre Anwendung sehr schlecht reagiert. Außerdem hat jeder neue dem Thread Pool hinzugefügte Thread mehr Aufwand (z. b. 1 MB Stapel Arbeitsspeicher). Eine Webanwendung, die synchrone Methoden verwendet, um Aufrufe mit hoher Latenz zu verarbeiten, bei denen der Thread Pool auf den maximalen .NET 4,5-Standardwert von 5 000 Threads ansteigt, beansprucht ungefähr 5 GB mehr Arbeitsspeicher als eine Anwendung, die die gleichen Anforderungen verwendet. asynchrone Methoden und nur 50-Threads. Wenn Sie asynchrone Aufgaben durcharbeiten, verwenden Sie nicht immer einen Thread. Wenn Sie z. b. einen asynchronen webService Request erstellen, verwendet ASP.NET keine Threads zwischen dem asynchronen **Methoden** Aufrufvorgang und dem **warten**. Die Verwendung des Thread Pools für den Einsatz von Anforderungen mit hoher Latenzzeit kann zu einem hohen Speicherbedarf und unzureichender Auslastung der Server Hardware führen.

## <a name="processing-asynchronous-requests"></a>Verarbeiten von asynchronen Anforderungen

In Webanwendungen, die eine große Anzahl gleichzeitiger Anforderungen beim Start anzeigen oder eine bursty-Auslastung haben (bei der die Parallelität plötzlich zunimmt), erhöht sich die Reaktionsfähigkeit Ihrer Anwendung, indem Webdienst Aufrufe asynchron durchführt werden. Die Verarbeitung einer asynchronen Anforderung dauert denselben Zeitraum wie eine synchrone Anforderung. Wenn z. b. eine Anforderung einen Webdienst aufzurufen, der zwei Sekunden benötigt, dauert die Anforderung zwei Sekunden, unabhängig davon, ob Sie synchron oder asynchron ausgeführt wird. Während eines asynchronen Aufrufes wird jedoch verhindert, dass ein Thread auf andere Anforderungen antwortet, während er darauf wartet, dass die erste Anforderung beendet wird. Daher verhindern asynchrone Anforderungen das Anforderungswarteschlangen-und Thread Pool Wachstum, wenn es viele gleichzeitige Anforderungen gibt, die Vorgänge mit langer Laufzeit aufrufen.

## <a id="ChoosingSyncVasync"></a>Auswählen von synchronen oder asynchronen Methoden

In diesem Abschnitt werden die Richtlinien für den Einsatz von synchronen oder asynchronen Methoden aufgeführt. Dies sind nur Richtlinien. untersuchen Sie jede Anwendung einzeln, um zu bestimmen, ob asynchrone Methoden die Leistung unterstützen.

Verwenden Sie in der Regel synchrone Methoden für die folgenden Bedingungen:

- Die Vorgänge sind einfach oder kurz ausgeführt.
- Einfachheit ist wichtiger als die Effizienz.
- Bei den Vorgängen handelt es sich hauptsächlich um CPU-Vorgänge anstelle von Vorgängen, die einen umfangreichen Datenträger Die Verwendung von asynchronen Methoden für CPU-gebundene Vorgänge bietet keine Vorteile und führt zu einem höheren Verwaltungsaufwand.

Im Allgemeinen verwenden Sie asynchrone Methoden für die folgenden Bedingungen:

- Sie rufen Dienste auf, die durch asynchrone Methoden genutzt werden können, und Sie verwenden .NET 4,5 oder höher.
- Die Vorgänge sind Netzwerk gebunden oder e/a-gebunden anstatt CPU-gebunden.
- Parallelität ist wichtiger als die Einfachheit des Codes.
- Sie möchten einen Mechanismus bereitstellen, mit dem Benutzer eine Anforderung mit langer Ausführungszeit abbrechen können.
- Wenn der Vorteil der Umstellung von Threads die Kosten des Kontext Schalters übersteigt. Im Allgemeinen sollten Sie eine Methode asynchron erstellen, wenn die synchrone Methode den ASP.net-Anforderungs Thread blockiert, während keine Arbeit ausgeführt wird. Durch die asynchrone Ausführung des Aufrufes wird der ASP.net Request-Thread nicht blockiert, während er darauf wartet, dass der webService Request beendet wird.
- Das Testen zeigt, dass die blockierenden Vorgänge einen Engpass bei der Standort Leistung darstellen und dass IIS mehr Anforderungen mithilfe von asynchronen Methoden für diese blockierenden Aufrufe bedienen kann.

  Das herunterladbare Beispiel zeigt, wie asynchrone Methoden effektiv verwendet werden. Das bereitgestellte Beispiel wurde entworfen, um eine einfache Demonstration der asynchronen Programmierung in ASP.NET 4,5 zu ermöglichen. Das Beispiel soll keine Referenzarchitektur für die asynchrone Programmierung in ASP.net sein. Das Beispielprogramm ruft [ASP.net-Web-API](../../../web-api/index.md) Methoden auf, die wiederum [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) aufrufen, um lange Ausführungs-Webdienst Aufrufe zu simulieren. In den meisten Produktionsanwendungen werden solche offensichtlichen Vorteile bei der Verwendung von asynchronen Methoden nicht angezeigt.   
  
Bei wenigen Anwendungen müssen alle Methoden asynchron sein. Häufig bietet die typverarbeitung einiger synchroner Methoden in asynchrone Methoden die beste Effizienzsteigerung für die erforderliche Arbeitsmenge.

## <a id="SampleApp"></a>Die Beispielanwendung

Sie können die Beispielanwendung von [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) auf der [GitHub](https://github.com/) -Website herunterladen. Das Repository besteht aus drei Projekten:

- *Webappasync*: das ASP.net-Web Forms Projekt, das den Web-API- **webapipwg** -Dienst nutzt. Der größte Teil des Codes für dieses Tutorial ist das Projekt.
- *Webapipgw*: das ASP.NET MVC 4-Web-API-Projekt, das die `Products, Gizmos and Widgets` Controller implementiert. Es stellt die Daten für das *webappasync* -Projekt und das *Mvc4Async* -Projekt bereit.
- *Mvc4Async*: das ASP.NET MVC 4-Projekt, das den Code enthält, der in einem anderen Tutorial verwendet wird. Es werden Web-API-Aufrufe an den **webapipwg** -Dienst durchführt.

## <a id="GizmosSynch"></a>Die synchrone Gizmos-Seite

 Der folgende Code zeigt die `Page_Load` synchrone-Methode, die verwendet wird, um eine Liste von Gizmos anzuzeigen. (In diesem Artikel ist ein Gizmo ein fiktives mechanisches Gerät.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Der folgende Code zeigt die `GetGizmos`-Methode des Gizmo-Dienstanbieter.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Die `GizmoService GetGizmos`-Methode übergibt einen URI an einen ASP.net-Web-API HTTP-Dienst, der eine Liste der Gizmos-Daten zurückgibt. Das *webapipgw* -Projekt enthält die Implementierung der Web-API-`gizmos, widget` und `product` Controller.  
Die folgende Abbildung zeigt die Gizmos-Seite aus dem Beispiel Projekt.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Erstellen einer asynchronen Gizmos-Seite

Das Beispiel verwendet die [neuen Schlüsselwörter](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) und erwarnung (verfügbar in .NET 4,5 und Visual Studio 2012), damit der Compiler für die Verwaltung der komplizierten Transformationen verantwortlich ist, die für die asynchrone Programmierung erforderlich sind. Der Compiler ermöglicht das Schreiben von Code mithilfe C#der synchronen Ablaufsteuerungskonstrukte, und der Compiler wendet automatisch die für die Verwendung von Rückrufen erforderlichen Transformationen an, um das Blockieren von Threads zu vermeiden.

ASP.NET asynchrone Seiten müssen die [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive mit dem `Async`-Attribut enthalten, das auf "true" festgelegt ist. Der folgende Code zeigt die [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive, bei der das `Async`-Attribut für die Seite *gizmosasync. aspx* auf "true" festgelegt ist.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Der folgende Code zeigt die `Gizmos` synchrone `Page_Load`-Methode und die `GizmosAsync` asynchrone Seite. Wenn Ihr Browser die [HTML 5-&lt;Markierung&gt; Elements](http://www.w3.org/wiki/HTML/Elements/mark)unterstützt, werden die Änderungen in `GizmosAsync` gelb hervorgehoben angezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Die asynchrone Version:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Die folgenden Änderungen wurden angewendet, um zuzulassen, dass die `GizmosAsync` Seite asynchron ist.

- Für die [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive muss das `Async`-Attribut auf "true" festgelegt sein.
- Die `RegisterAsyncTask`-Methode wird verwendet, um eine asynchrone Aufgabe zu registrieren, die den Code enthält, der asynchron ausgeführt wird.
- Die neue `GetGizmosSvcAsync`-Methode ist mit dem [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort gekennzeichnet, das den Compiler anweist, Rückrufe für Teile des Texts zu generieren und automatisch eine `Task` zu erstellen, die zurückgegeben wird.
- &quot;Async-&quot; wurde an den asynchronen Methodennamen angehängt. Das Anfügen von "Async" ist nicht erforderlich, ist jedoch die Konvention beim Schreiben von asynchronen Methoden.
- Der Rückgabetyp der neuen `GetGizmosSvcAsync` Methode ist `Task`. Der Rückgabetyp von `Task` stellt laufende Arbeit dar und stellt Aufrufern der Methode ein Handle zur Verfügung, über das auf den Abschluss des asynchronen Vorgangs gewartet werden soll.
- Das [Erwartungs Wort für den](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Webdienst wurde angewendet.
- Die asynchrone Webdienst-API wurde (`GetGizmosAsync`) aufgerufen.

Innerhalb des `GetGizmosSvcAsync` Methoden Texts eine andere asynchrone Methode, wird `GetGizmosAsync` aufgerufen. `GetGizmosAsync` sofort eine `Task<List<Gizmo>>` zurück, die letztendlich beendet wird, wenn die Daten verfügbar sind. Da Sie nichts anderes tun möchten, bis Sie über die Gizmo-Daten verfügen, erwartet der Code die Aufgabe (mit dem **Erwartungs Wort "** erwartungsgemäß"). Sie können das **warte** Zeit Schlüsselwort nur in Methoden verwenden, die mit dem **Async** -Schlüsselwort kommentiert werden.

Mit dem **Erwartungs Wort wird der Thread** nicht blockiert, bis die Aufgabe beendet ist. Er registriert den Rest der Methode als Rückruf für die Aufgabe und gibt sofort zurück. Wenn die erwartete Aufgabe schließlich abgeschlossen ist, ruft Sie diesen Rückruf auf und setzt somit die Ausführung der Methode an der Stelle fort, an der Sie aufgehört hat. Weitere Informationen zur Verwendung der Schlüsselwörter "References" und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) [" und des](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace finden Sie unter " [Async-Verweise](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)".

Der folgende Code zeigt die `GetGizmos`- und `GetGizmosAsync`-Methoden:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Die asynchronen Änderungen ähneln den oben beschriebenen **gizmosasync** -Vorgängen. 

- Die Methoden Signatur wurde mit dem [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort versehen, der Rückgabetyp wurde in "`Task<List<Gizmo>>`" geändert, und " *Async* " wurde an den Methodennamen angehängt.
- Die asynchrone [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) -Klasse wird anstelle der synchronen [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) -Klasse verwendet.
- Das [Erwartungs Wort wurde auf die](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) asynchrone Methode " [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[getasync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) " angewendet.

Die folgende Abbildung zeigt die asynchrone Gizmo-Sicht.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Die Browser Darstellung der Gizmos-Daten ist mit der vom synchronen-Befehl erstellten Sicht identisch. Der einzige Unterschied besteht darin, dass die asynchrone Version bei starker Auslastung möglicherweise leistungsfähiger ist.

## <a name="registerasynctask-notes"></a>RegisterAsyncTask-Notizen

Methoden, die mit `RegisterAsyncTask` verknüpft sind, werden unmittelbar nach der [vorab](https://msdn.microsoft.com/library/ms178472.aspx)Version ausgeführt. Sie können auch asynchrone Seiten Ereignisse direkt verwenden, wie im folgenden Code gezeigt:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Der Nachteil von asynchronen void-Ereignissen besteht darin, dass Entwickler nicht mehr die volle Kontrolle darüber haben, wann Ereignisse ausgeführt werden. Beispielsweise, wenn sowohl eine ASPX-als auch eine-. Master definieren `Page_Load`-Ereignissen, und eine oder beide sind asynchron, die Ausführungsreihenfolge kann nicht garantiert werden. Die gleiche indeterminiate-Reihenfolge für nichtereignis Handler (z. b. `async void Button_Click`) ist gültig. Für die meisten Entwickler sollte dies akzeptabel sein, aber diejenigen, die die vollständige Kontrolle über die Ausführungsreihenfolge benötigen, sollten nur APIs wie `RegisterAsyncTask` verwenden, die Methoden verwenden, die ein Task-Objekt zurückgeben.

## <a id="Parallel"></a>Paralleles Ausführen mehrerer Vorgänge

Asynchrone Methoden haben gegenüber synchronen Methoden einen erheblichen Vorteil, wenn eine Aktion mehrere unabhängige Vorgänge ausführen muss. Im bereitgestellten Beispiel zeigt die synchrone Seite *PWG. aspx*(für Produkte, Widgets und Gizmos) die Ergebnisse von drei Webdienst aufrufen an, um eine Liste der Produkte, Widgets und Gizmos zu erhalten. Das [ASP.net-Web-API](../../../web-api/index.md) Projekt, das diese Dienste bereitstellt, verwendet [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) zum Simulieren von Latenzzeiten oder langsamen Netzwerk aufrufen. Wenn die Verzögerung auf 500 Millisekunden festgelegt ist, dauert die asynchrone *pwgasync. aspx* -Seite etwas mehr als 500 Millisekunden, während die synchrone `PWG` Version mehr als 1.500 Millisekunden benötigt. Die synchrone *PWG. aspx* -Seite wird im folgenden Code dargestellt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Der asynchrone `PWGasync` Code Behind ist unten dargestellt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Die folgende Abbildung zeigt die von der asynchronen *pwgasync. aspx* -Seite zurückgegebene Sicht.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Verwenden eines Abbruch Tokens

Asynchrone Methoden, die `Task`zurückgeben, können abgebrochen werden, d. h., Sie akzeptieren einen [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) -Parameter, wenn eine mit dem Attribut `AsyncTimeout` der [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive angegeben wird. Der folgende Code zeigt die Seite *gizmuscancelasync. aspx* mit einem Timeout von auf der zweiten Seite.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Der folgende Code zeigt die Datei *GizmosCancelAsync.aspx.cs* .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

In der bereitgestellten Beispielanwendung wird durch Auswählen des Links *gizmuscancelasync* die Seite *gizmuscancelasync. aspx* aufgerufen und der Abbruch (durch Timeout) des asynchronen Aufrufs veranschaulicht. Da die Verzögerungszeit innerhalb eines zufälligen Bereichs liegt, müssen Sie die Seite möglicherweise mehrmals aktualisieren, um die Timeout Fehlermeldung zu erhalten.

## <a id="ServerConfig"></a>Server Konfiguration für Webdienst Aufrufe mit hoher Parallelität/hoher Latenz

Um die Vorteile einer asynchronen Webanwendung zu nutzen, müssen Sie möglicherweise einige Änderungen an der Standard Serverkonfiguration vornehmen. Beachten Sie beim Konfigurieren von und Belastungstests für Ihre asynchrone Webanwendung Folgendes:

- Windows 7, Windows Vista, Windows 8 und alle Windows-Client Betriebssysteme haben maximal 10 gleichzeitige Anforderungen. Sie benötigen ein Windows Server-Betriebssystem, um die Vorteile von asynchronen Methoden unter hoher Auslastung anzuzeigen.
- Registrieren Sie .NET 4,5 mit IIS über eine Eingabeaufforderung mit erhöhten Rechten, indem Sie den folgenden Befehl verwenden:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  Weitere Informationen finden Sie unter [ASP.NET IIS Registration Tool (ASPNET\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Möglicherweise müssen Sie das Limit für die [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) -Warteschlange vom Standardwert 1.000 auf 5.000 erhöhen. Wenn die Einstellung zu niedrig ist, werden möglicherweise [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) -Anforderungen mit dem HTTP 503-Status abgelehnt. So ändern Sie das Warteschlangen Limit von http. sys:

    - Öffnen Sie den IIS-Manager, und navigieren Sie zum Bereich Anwendungs Pools.
    - Klicken Sie mit der rechten Maustaste auf den Ziel Anwendungs Pool, und wählen Sie **Erweiterte Einstellungen**.  
        Erweiterte](using-asynchronous-methods-in-aspnet-45/_static/image4.png) ![
    - Ändern Sie im Dialogfeld **Erweiterte Einstellungen** die *Warteschlangen Länge* von 1.000 in 5.000.  
        ![Warteschlangen Länge](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Beachten Sie in den obigen Abbildungen, dass .NET Framework als v 4.0 aufgeführt ist, auch wenn der Anwendungs Pool .NET 4,5 verwendet. Informationen zu dieser Abweichung finden Sie in den folgenden Bereichen:

- [.Net-Versionsverwaltung und mehrere Ziel Versionen: .NET 4,5 ist ein direktes Upgrade auf .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [Festlegen einer IIS-Anwendung oder eines Anwendungs Pools für die Verwendung von ASP.NET 3,5 anstelle von 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [.NET Framework-Versionen und -Abhängigkeiten](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- Wenn Ihre Anwendung Webdienste oder System.NET verwendet, um über HTTP mit einem Back-End zu kommunizieren, müssen Sie möglicherweise das [connectionManagement/maxConnection-](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) Element erhöhen. Bei ASP.NET-Anwendungen wird dies durch die Funktion "autoConfig" auf das 12-fache der Anzahl der CPUs beschränkt. Dies bedeutet, dass Sie bei einer Quad-proc-Methode höchstens 12 \* 4 = 48 gleichzeitige Verbindungen mit einem IP-Endpunkt haben können. Da diese an [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)gebunden ist, ist die einfachste Möglichkeit, `maxconnection` in einer ASP.NET-Anwendung zu erhöhen, [System .net. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) in der from `Application_Start`-Methode in der Datei *Global. asax* Programm gesteuert festzulegen. Ein Beispiel finden Sie im Beispiel für das herunterladen.
- In .NET 4,5 sollte der Standardwert 5000 für [maxconcurrentrequestspercpu](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) in Ordnung sein.

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [Hongmei ge](https://blogs.msdn.com/b/hongmeig/)
