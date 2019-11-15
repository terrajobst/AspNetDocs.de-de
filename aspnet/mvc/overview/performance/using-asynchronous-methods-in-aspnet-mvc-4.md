---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Verwenden von asynchronen Methoden in ASP.NET MVC 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer asynchronen ASP.NET MVC-Webanwendung mithilfe von Visual Studio Express 2012 für das Web.
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5df6a9c136b1934b3afd731eb0ceac1e0faa483e
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115084"
---
# <a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Verwenden asynchroner Methoden in ASP.NET MVC 4

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer asynchronen ASP.NET MVC-Webanwendung mithilfe von [Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/11), eine kostenlose Version von Microsoft Visual Studio. Sie können auch [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)verwenden.
> 
> Ein umfassendes Beispiel für dieses Tutorial finden Sie auf GitHub [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)

Mit der ASP.NET MVC 4- [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) Klasse in Kombination von [.NET 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) können Sie asynchrone Aktionsmethoden schreiben, die ein Objekt des Typs [Task&lt;Action result&gt;](https://msdn.microsoft.com/library/dd321424(VS.110).aspx)zurückgeben. In .NET Framework 4 wurde ein asynchrones Programmier Konzept eingeführt, das als [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) bezeichnet wird, und ASP.NET MVC 4 unterstützt [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Tasks werden durch den **Tasktyp und** verwandte Typen im [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) -Namespace dargestellt. Der .NET Framework 4,5 baut auf dieser asynchronen Unterstützung mit den Schlüsselwörtern " [Erwartung](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) " und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) " auf, die das Arbeiten mit [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Objekten wesentlich weniger komplex machen als vorherige asynchrone Ansätze. Das [Wait](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort ist syntaktische Kurzwort, um anzugeben, dass ein Code Element asynchron auf einen anderen Code Abschnitt warten soll. Das [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort stellt einen Hinweis dar, den Sie verwenden können, um Methoden als aufgabenbasierte asynchrone Methoden zu markieren. Die Kombination aus " **warten**", " **Async**" und " **Task** " erleichtert Ihnen das Schreiben von asynchronem Code in .NET 4,5. Das neue Modell für asynchrone Methoden wird als *Task basiertes asynchrones Muster* (**Tap**) bezeichnet. In diesem Tutorial wird davon ausgegangen, dass Sie mit der asynchronen Programmierung mit den Schlüsselwörtern " [Erwartung](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) " und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) " und dem [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace vertraut sind

Weitere Informationen [zum Verwenden der](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwörter "References" und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) " und des [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace finden Sie in den folgenden verweisen.

- [Whitepaper: Asynchronität in .net](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Häufig gestellte Fragen zu Async/warten](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchrone Programmierung in Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Verarbeitung von Anforderungen durch den Thread Pool

Auf dem Webserver verwaltet das .NET Framework einen Thread Pool, der zum Service ASP.NET-Anforderungen verwendet wird. Wenn eine Anforderung eingeht, wird ein Thread aus dem Pool verteilt, um diese Anforderung zu verarbeiten. Wenn die Anforderung synchron verarbeitet wird, ist der Thread, der die Anforderung verarbeitet, ausgelastet, während die Anforderung verarbeitet wird, und dieser Thread kann keine andere Anforderung verarbeiten.   
  
Dies stellt möglicherweise kein Problem dar, da der Thread Pool groß genug gemacht werden kann, um viele ausgelastete Threads aufnehmen zu können. Die Anzahl der Threads im Thread Pool ist jedoch begrenzt (der Standardwert für .NET 4,5 ist 5.000). In großen Anwendungen mit hoher Parallelität von Anforderungen mit langer Ausführungszeit sind möglicherweise alle verfügbaren Threads ausgelastet. Diese Bedingung wird als Thread Hunger bezeichnet. Wenn diese Bedingung erreicht wird, werden Anforderungen vom Webserver in die Warteschlange eingereiht. Wenn die Anforderungs Warteschlange voll ist, lehnt der Webserver Anforderungen mit einem HTTP 503-Status (Server ist zu stark ausgelastet) ab. Der CLR-Thread Pool weist Einschränkungen bei neuen Thread Spritzen auf. Wenn die Parallelität burstig ist (d. h., Ihre Website kann plötzlich eine große Anzahl von Anforderungen erhalten), und alle verfügbaren Anforderungs Threads sind aufgrund von Back-End-aufrufen mit hoher Latenzzeit ausgelastet. die eingeschränkte Thread einschleusungs Rate kann dazu führen, dass Ihre Anwendung sehr schlecht reagiert. Außerdem hat jeder neue dem Thread Pool hinzugefügte Thread mehr Aufwand (z. b. 1 MB Stapel Arbeitsspeicher). Eine Webanwendung, die synchrone Methoden verwendet, um Aufrufe mit hoher Latenz zu verarbeiten, bei denen der Thread Pool auf den maximalen .NET 4,5-Standardwert von 5 000 Threads ansteigt, beansprucht ungefähr 5 GB mehr Arbeitsspeicher als eine Anwendung, die die gleichen Anforderungen verwendet. asynchrone Methoden und nur 50-Threads. Wenn Sie asynchrone Aufgaben durcharbeiten, verwenden Sie nicht immer einen Thread. Wenn Sie z. b. einen asynchronen webService Request erstellen, verwendet ASP.NET keine Threads zwischen dem asynchronen **Methoden** Aufrufvorgang und dem **warten**. Die Verwendung des Thread Pools für den Einsatz von Anforderungen mit hoher Latenzzeit kann zu einem hohen Speicherbedarf und unzureichender Auslastung der Server Hardware führen.

## <a name="processing-asynchronous-requests"></a>Verarbeiten von asynchronen Anforderungen

In einer Web-App, die eine große Anzahl gleichzeitiger Anforderungen beim Start erkennt oder eine bursty Load (bei der die Parallelität plötzlich zunimmt), erhöht sich die Reaktionsfähigkeit der app durch das Ausführen von Webdienst aufrufen. Die Verarbeitung einer asynchronen Anforderung dauert denselben Zeitraum wie eine synchrone Anforderung. Wenn eine Anforderung einen Webdienst aufruft, der zwei Sekunden benötigt, dauert die Anforderung zwei Sekunden, unabhängig davon, ob Sie synchron oder asynchron ausgeführt wird. Während eines asynchronen Aufrufes wird jedoch verhindert, dass ein Thread auf andere Anforderungen antwortet, während er auf den Abschluss der ersten Anforderung wartet. Daher verhindern asynchrone Anforderungen das Anforderungswarteschlangen-und Thread Pool Wachstum, wenn es viele gleichzeitige Anforderungen gibt, die Vorgänge mit langer Laufzeit aufrufen.

## <a id="ChoosingSyncVasync"></a>Auswählen von synchronen oder asynchronen Aktionsmethoden

In diesem Abschnitt werden die Richtlinien für den Einsatz von synchronen oder asynchronen Aktionsmethoden aufgeführt. Dies sind nur Richtlinien. untersuchen Sie jede Anwendung einzeln, um zu bestimmen, ob asynchrone Methoden die Leistung unterstützen.

Verwenden Sie in der Regel synchrone Methoden für die folgenden Bedingungen:

- Die Vorgänge sind einfach oder kurz ausgeführt.
- Einfachheit ist wichtiger als die Effizienz.
- Bei den Vorgängen handelt es sich hauptsächlich um CPU-Vorgänge anstelle von Vorgängen, die einen umfangreichen Datenträger Die Verwendung von asynchronen Aktionsmethoden für CPU-gebundene Vorgänge bietet keine Vorteile und führt zu einem höheren Verwaltungsaufwand.

Im Allgemeinen verwenden Sie asynchrone Methoden für die folgenden Bedingungen:

- Sie rufen Dienste auf, die durch asynchrone Methoden genutzt werden können, und Sie verwenden .NET 4,5 oder höher.
- Die Vorgänge sind Netzwerk gebunden oder e/a-gebunden anstatt CPU-gebunden.
- Parallelität ist wichtiger als die Einfachheit des Codes.
- Sie möchten einen Mechanismus bereitstellen, mit dem Benutzer eine Anforderung mit langer Ausführungszeit abbrechen können.
- Wenn der Vorteil der Umstellung von Threads die Kosten des Kontext Schalters übersteigt. Im Allgemeinen sollten Sie eine Methode asynchron erstellen, wenn die synchrone Methode auf den ASP.net Request-Thread wartet, während keine Arbeit ausgeführt wird. Durch die asynchrone Ausführung des Aufrufes wird der ASP.net-Anforderungs Thread nicht angehalten und funktioniert nicht mehr, während er auf den Abschluss des webService Request wartet.
- Das Testen zeigt, dass die blockierenden Vorgänge einen Engpass bei der Standort Leistung darstellen und dass IIS mehr Anforderungen mithilfe von asynchronen Methoden für diese blockierenden Aufrufe bedienen kann.

Das herunterladbare Beispiel zeigt, wie asynchrone Aktionsmethoden effektiv verwendet werden. Das bereitgestellte Beispiel wurde entworfen, um eine einfache Demonstration der asynchronen Programmierung in ASP.NET MVC 4 mithilfe von .NET 4,5 zu ermöglichen. Das Beispiel ist nicht als Referenzarchitektur für die asynchrone Programmierung in ASP.NET MVC gedacht. Das Beispielprogramm ruft [ASP.net-Web-API](../../../web-api/index.md) Methoden auf, die wiederum [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) aufrufen, um lange Ausführungs-Webdienst Aufrufe zu simulieren. Die meisten Produktionsanwendungen zeigen diese offensichtlichen Vorteile für die Verwendung von asynchronen Aktionsmethoden nicht an.   
  
Bei wenigen Anwendungen müssen alle Aktionsmethoden asynchron sein. Häufig bietet die typverarbeitung einiger synchroner Aktionsmethoden in asynchrone Methoden die beste Effizienzsteigerung für die erforderliche Arbeitsmenge.

## <a id="SampleApp"></a>Die Beispielanwendung

Sie können die Beispielanwendung von [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET) auf der [GitHub](https://github.com/) -Website herunterladen. Das Repository besteht aus drei Projekten:

- *Mvc4Async*: das ASP.NET MVC 4-Projekt, das den in diesem Tutorial verwendeten Code enthält. Es werden Web-API-Aufrufe an den **webapipgw** -Dienst durchführt.
- *Webapipgw*: das ASP.NET MVC 4-Web-API-Projekt, das die `Products, Gizmos and Widgets` Controller implementiert. Es stellt die Daten für das *webappasync* -Projekt und das *Mvc4Async* -Projekt bereit.
- *Webappasync*: das ASP.net-Web Forms Projekt, das in einem anderen Tutorial verwendet wird.

## <a id="GizmosSynch"></a>Die synchrone Gizmos-Aktionsmethode

 Der folgende Code zeigt die `Gizmos` synchrone Aktionsmethode, die verwendet wird, um eine Liste von Gizmos anzuzeigen. (In diesem Artikel ist ein Gizmo ein fiktives mechanisches Gerät.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Der folgende Code zeigt die `GetGizmos`-Methode des Gizmo-Dienstanbieter.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

Die `GizmoService GetGizmos`-Methode übergibt einen URI an einen ASP.net-Web-API HTTP-Dienst, der eine Liste der Gizmos-Daten zurückgibt. Das *webapipgw* -Projekt enthält die Implementierung der Web-API-`gizmos, widget` und `product` Controller.  
Die folgende Abbildung zeigt die Gizmos-Ansicht aus dem Beispiel Projekt.

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Erstellen einer asynchronen Gizmos-Aktionsmethode

Das Beispiel verwendet die [neuen Schlüsselwörter](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) und erwarnung (verfügbar in .NET 4,5 und Visual Studio 2012), damit der Compiler für die Verwaltung der komplizierten Transformationen verantwortlich ist, die für die asynchrone Programmierung erforderlich sind. Der Compiler ermöglicht das Schreiben von Code mithilfe C#der synchronen Ablaufsteuerungskonstrukte, und der Compiler wendet automatisch die für die Verwendung von Rückrufen erforderlichen Transformationen an, um das Blockieren von Threads zu vermeiden.

Der folgende Code zeigt die `Gizmos` synchrone-Methode und die `GizmosAsync` asynchrone-Methode. Wenn Ihr Browser das [HTML 5-`<mark>` Element](http://www.w3.org/wiki/HTML/Elements/mark)unterstützt, werden die Änderungen in `GizmosAsync` gelb hervorgehoben angezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Die folgenden Änderungen wurden angewendet, damit das `GizmosAsync` asynchron sein kann.

- Die-Methode wird mit dem [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort gekennzeichnet, das den Compiler anweist, Rückrufe für Teile des Texts zu generieren und automatisch eine `Task<ActionResult>` zu erstellen, die zurückgegeben wird.
- &quot;Async-&quot; wurde an den Methodennamen angehängt. Das Anfügen von "Async" ist nicht erforderlich, ist jedoch die Konvention beim Schreiben von asynchronen Methoden.
- Der Rückgabetyp wurde von `ActionResult` in `Task<ActionResult>`geändert. Der Rückgabetyp von `Task<ActionResult>` stellt laufende Arbeit dar und stellt Aufrufern der Methode ein Handle zur Verfügung, über das auf den Abschluss des asynchronen Vorgangs gewartet werden soll. In diesem Fall ist der Aufrufer der Webdienst. `Task<ActionResult>` stellt laufende Arbeit mit dem Ergebnis `ActionResult.`
- Das [Erwartungs Wort für den](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Webdienst wurde angewendet.
- Die asynchrone Webdienst-API wurde (`GetGizmosAsync`) aufgerufen.

Innerhalb des `GetGizmosAsync` Methoden Texts eine andere asynchrone Methode, wird `GetGizmosAsync` aufgerufen. `GetGizmosAsync` sofort eine `Task<List<Gizmo>>` zurück, die letztendlich beendet wird, wenn die Daten verfügbar sind. Da Sie nichts anderes tun möchten, bis Sie über die Gizmo-Daten verfügen, erwartet der Code die Aufgabe (mit dem **Erwartungs Wort "** erwartungsgemäß"). Sie können das **warte** Zeit Schlüsselwort nur in Methoden verwenden, die mit dem **Async** -Schlüsselwort kommentiert werden.

Mit dem **Erwartungs Wort wird der Thread** nicht blockiert, bis die Aufgabe beendet ist. Er registriert den Rest der Methode als Rückruf für die Aufgabe und gibt sofort zurück. Wenn die erwartete Aufgabe schließlich abgeschlossen ist, ruft Sie diesen Rückruf auf und setzt somit die Ausführung der Methode an der Stelle fort, an der Sie aufgehört hat. Weitere Informationen zur Verwendung der Schlüsselwörter "References" und " [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) [" und des](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) [Aufgaben](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace finden Sie unter " [Async-Verweise](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)".

Der folgende Code zeigt die Methoden `GetGizmos` und `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Die asynchronen Änderungen ähneln den oben beschriebenen **gizmosasync** -Vorgängen. 

- Die Methoden Signatur wurde mit dem [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort versehen, der Rückgabetyp wurde in "`Task<List<Gizmo>>`" geändert, und " *Async* " wurde an den Methodennamen angehängt.
- Die asynchrone [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) -Klasse wird anstelle der [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) -Klasse verwendet.
- Das [Erwartungs Wort wurde auf die](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) asynchronen [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) -Methoden angewendet.

Die folgende Abbildung zeigt die asynchrone Gizmo-Sicht.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

Die Browser Darstellung der Gizmos-Daten ist mit der vom synchronen-Befehl erstellten Sicht identisch. Der einzige Unterschied besteht darin, dass die asynchrone Version bei starker Auslastung möglicherweise leistungsfähiger ist.

## <a id="Parallel"></a>Paralleles Ausführen mehrerer Vorgänge

Asynchrone Aktionsmethoden haben gegenüber synchronen Methoden einen erheblichen Vorteil, wenn eine Aktion mehrere unabhängige Vorgänge ausführen muss. Im bereitgestellten Beispiel zeigt die synchrone Methode `PWG`(für Produkte, Widgets und Gizmos) die Ergebnisse von drei Webdienst aufrufen an, um eine Liste von Produkten, Widgets und Gizmos zu erhalten. Das [ASP.net-Web-API](../../../web-api/index.md) Projekt, das diese Dienste bereitstellt, verwendet [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) zum Simulieren von Latenzzeiten oder langsamen Netzwerk aufrufen. Wenn die Verzögerung auf 500 Millisekunden festgelegt ist, dauert die Ausführung der asynchronen `PWGasync`-Methode etwas mehr als 500 Millisekunden, während die synchrone `PWG` Version mehr als 1.500 Millisekunden benötigt. Die synchrone `PWG`-Methode wird im folgenden Code dargestellt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Die asynchrone `PWGasync`-Methode wird im folgenden Code dargestellt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

Die folgende Abbildung zeigt die Ansicht, die von der **pwgasync** -Methode zurückgegeben wird.

![pwgasync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>Verwenden eines Abbruch Tokens

Asynchrone Aktionsmethoden, die `Task<ActionResult>`zurückgeben, können abgebrochen werden, d. h., Sie akzeptieren einen [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) -Parameter, wenn eine mit dem [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) -Attribut angegeben wird. Der folgende Code zeigt die `GizmosCancelAsync`-Methode mit einem Timeout von 150 Millisekunden.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Der folgende Code zeigt die getgizmosasync-Überladung, die einen [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) -Parameter annimmt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

In der bereitgestellten Beispielanwendung wird durch Auswählen des Links zum *Abbruch Token-Demo* die `GizmosCancelAsync`-Methode aufgerufen und der Abbruch des asynchronen Aufrufs veranschaulicht.

## <a id="ServerConfig"></a>Server Konfiguration für Webdienst Aufrufe mit hoher Parallelität/hoher Latenz

Um die Vorteile einer asynchronen Webanwendung zu nutzen, müssen Sie möglicherweise einige Änderungen an der Standard Serverkonfiguration vornehmen. Beachten Sie beim Konfigurieren von und Belastungstests für Ihre asynchrone Webanwendung Folgendes:

- Windows 7, Windows Vista und alle Windows-Client Betriebssysteme haben maximal 10 gleichzeitige Anforderungen. Sie benötigen ein Windows Server-Betriebssystem, um die Vorteile von asynchronen Methoden unter hoher Auslastung anzuzeigen.
- Registrieren Sie .NET 4,5 bei IIS über eine Eingabeaufforderung mit erhöhten Rechten:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis-i  
  Weitere Informationen finden Sie unter [ASP.NET IIS Registration Tool (ASPNET\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Möglicherweise müssen Sie das Limit für die [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) -Warteschlange vom Standardwert 1.000 auf 5.000 erhöhen. Wenn die Einstellung zu niedrig ist, werden möglicherweise [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) -Anforderungen mit dem HTTP 503-Status abgelehnt. So ändern Sie das Warteschlangen Limit von http. sys:

    - Öffnen Sie den IIS-Manager, und navigieren Sie zum Bereich Anwendungs Pools.
    - Klicken Sie mit der rechten Maustaste auf den Ziel Anwendungs Pool, und wählen Sie **Erweiterte Einstellungen**.  
        Erweiterte](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png) ![
    - Ändern Sie im Dialogfeld **Erweiterte Einstellungen** die *Warteschlangen Länge* von 1.000 in 5.000.  
        ![Warteschlangen Länge](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Beachten Sie in den obigen Abbildungen, dass .NET Framework als v 4.0 aufgeführt ist, auch wenn der Anwendungs Pool .NET 4,5 verwendet. Informationen zu dieser Abweichung finden Sie in den folgenden Bereichen:

    - [.Net-Versionsverwaltung und mehrere Ziel Versionen: .NET 4,5 ist ein direktes Upgrade auf .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Festlegen einer IIS-Anwendung oder eines Anwendungs Pools für die Verwendung von ASP.NET 3,5 anstelle von 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [.NET Framework-Versionen und -Abhängigkeiten](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Wenn Ihre Anwendung Webdienste oder System.NET verwendet, um über HTTP mit einem Back-End zu kommunizieren, müssen Sie möglicherweise das [connectionManagement/maxConnection-](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) Element erhöhen. Bei ASP.NET-Anwendungen wird dies durch die Funktion "autoConfig" auf das 12-fache der Anzahl der CPUs beschränkt. Dies bedeutet, dass Sie bei einer Quad-proc-Methode höchstens 12 \* 4 = 48 gleichzeitige Verbindungen mit einem IP-Endpunkt haben können. Da diese an [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)gebunden ist, ist die einfachste Möglichkeit, `maxconnection` in einer ASP.NET-Anwendung zu erhöhen, [System .net. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) in der from `Application_Start`-Methode in der Datei *Global. asax* Programm gesteuert festzulegen. Ein Beispiel finden Sie im Beispiel für das herunterladen.
- In .NET 4,5 sollte der Standardwert 5000 für [maxconcurrentrequestspercpu](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) in Ordnung sein.
