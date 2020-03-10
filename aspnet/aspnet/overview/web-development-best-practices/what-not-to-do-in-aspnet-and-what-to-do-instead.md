---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Was ist in ASP.net nicht zu tun? Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Thema werden einige häufige Fehler beschrieben, die Benutzer innerhalb von ASP.NET-Webprojekten treffen. Hier finden Sie Empfehlungen dazu, was Sie tun sollten, um diese Kommas zu vermeiden...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500133"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Häufige Fehler bei ASP.NET und empfohlene Vorgehensweisen

> In diesem Thema werden einige häufige Fehler beschrieben, die Benutzer innerhalb von ASP.NET-Webprojekten treffen. Hier finden Sie Empfehlungen dazu, was Sie tun sollten, um diese häufigen Fehler zu vermeiden. Es basiert auf einer [Präsentation](http://vimeo.com/68390507) von **Damian Edwards** auf der norwegischen Entwicklerkonferenz.

## <a name="disclaimer"></a>Haftungsausschluss

Dieses Thema ist nicht als kompletter Leitfaden gedacht, um sicherzustellen, dass Ihre Anwendung sicher und effizient ist. Sie müssen weiterhin bewährte Methoden für Sicherheit und Leistung befolgen, die in diesem Thema nicht beschrieben werden. Es wird nur vorgeschlagen, häufige Fehler in Bezug auf .NET-Klassen und-Prozesse zu vermeiden.

## <a name="overview"></a>Übersicht

Dieses Thema enthält folgende Abschnitte:

- [Einhaltung von Standards](#standards)

    - [Steuerelement Adapter](#adapters)
    - [Stileigenschaften für Steuerelemente](#styleprop)
    - [Seiten-und Steuerelement Rückrufe](#callback)
    - [Erkennung von Browser Funktionen](#browsercap)
- [Sicherheit](#security)

    - [Anforderungs Validierung](#validation)
    - [Formular Authentifizierung und Sitzung ohne cookielose Authentifizierung](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Mittlere Vertrauensstellung](#medium)
    - [&lt;appSettings-&gt;](#appsettings)
    - [Urlpathcodieren](#urlpathencode)
- [Zuverlässigkeit und Leistung](#performance)

    - [PreSendRequestHeaders und PreSendRequestContent](#presend)
    - [Asynchrone Seiten Ereignisse mit Web Forms](#asyncevents)
    - [Fire-and-Forget-Arbeit](#fire)
    - [Anforderungs Entitäts Text](#requestentity)
    - [Response. Redirect und Response. End](#redirect)
    - [EnableViewState und ViewStateMode](#viewstatemode)
    - [Sqlmitgliedshipprovider](#sqlprovider)
    - [Anforderungen mit langer Ausführungszeit (> 110 Sekunden)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Einhaltung von Standards

<a id="adapters"></a>

### <a name="control-adapters"></a>Steuerelement Adapter

Empfehlung: Verwenden Sie für das adaptive Rendering nicht die Verwendung von Steuerelement Adaptern, und verwenden Sie stattdessen CSS-Medien Abfragen und standardkonforme HTML.

Steuerelement Adapter wurden in .NET 2,0 eingeführt, um Präsentationscode zu rendieren, der für verschiedene Geräte und Umgebungen angepasst wurde. Jetzt kann dieses adaptive Rendering mit CSS und HTML erreicht werden. Sie sollten die Verwendung von Steuerelement Adaptern nicht mehr unterbinden und vorhandene Adapter in CSS und HTML konvertieren.

Weitere Informationen finden Sie unter [Medien Abfragen](http://www.w3.org/TR/css3-mediaqueries/) und Gewusst [wie: Hinzufügen von mobilen Seiten zu Ihrer ASP.net Web Forms/MVC-Anwendung](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Stileigenschaften für Steuerelemente

Empfehlung: setzen Sie das Festlegen von Stilwerten im Steuerelement Markup aus, und legen Sie stattdessen Formatierungs Werte in CSS-Stylesheets fest.

Webserver Steuerelemente enthalten Dutzende von Eigenschaften, die verwendet werden können, um Eigenschaften im Linienstil festzulegen. Beispielsweise legt die ForeColor-Eigenschaft die Textfarbe für ein Steuerelement fest. Sie können diese Wirkung effizienter über CSS-Stylesheets erreichen. Stylesheets ermöglichen es Ihnen, Stilwerte zu zentralisieren und diese Werte nicht in der gesamten Anwendung festzulegen.

Das folgende Beispiel zeigt eine CSS-Klasse, die den Text auf "rot" setzt.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Im nächsten Beispiel wird gezeigt, wie die CSS-Klasse dynamisch angewendet wird.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Seiten-und Steuerelement Rückrufe

Empfehlung: Verwenden Sie die Seiten-und Steuerelement Rückrufe nicht mehr, und verwenden Sie stattdessen eine der folgenden Methoden: AJAX, UpdatePanel, MVC-Aktionsmethoden, Web-API oder signalr.

In früheren Versionen von ASP.net ermöglichten die Rückruf Methoden "page" und "Control" das Aktualisieren eines Teils der Webseite, ohne eine ganze Seite zu aktualisieren. Sie können jetzt partielle Seiten Aktualisierungen über [AJAX](../../../ajax/index.md), [Update Panel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web-API](../../../web-api/index.md) oder [signalr](../../../signalr/index.md)ausführen. Sie sollten die Rückruf Methoden nicht mehr verwenden, da Sie Probleme mit freundlichen URLs und Routing verursachen können. Standardmäßig aktivieren Steuerelemente keine Rückruf Methoden, aber wenn Sie diese Funktion in einem-Steuerelement aktiviert haben, sollten Sie Sie deaktivieren.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Erkennung von Browser Funktionen

Empfehlung: Verwenden Sie die statische Erkennung von Browserfunktionen nicht mehr, sondern verwenden Sie stattdessen die Erkennung dynamischer Features.

In früheren Versionen von ASP.net wurden die unterstützten Funktionen für jeden Browser in einer XML-Datei gespeichert. Das Erkennen von Funktions Unterstützung durch eine statische Suche ist nicht der beste Ansatz. Nun können Sie die unterstützten Funktionen eines Browsers dynamisch mithilfe eines Features zur Funktionserkennung wie [modernizr](http://modernizr.com/)erkennen. Die Funktionserkennung bestimmt die Unterstützung, indem versucht wird, eine Methode oder Eigenschaft zu verwenden und dann zu überprüfen, ob der Browser das gewünschte Ergebnis erzeugt hat. Modernizr ist standardmäßig in den Webanwendungs Vorlagen enthalten.

<a id="security"></a>

## <a name="security"></a>Sicherheit

<a id="validation"></a>

### <a name="request-validation"></a>Anforderungsvalidierung

Empfehlung: Überprüfen Sie die Benutzereingaben, und codieren Sie die Ausgabe von Benutzern.

Die Anforderungs Validierung ist eine Funktion von ASP.net, die jede Anforderung überprüft und die Anforderung beendet, wenn eine erkannte Bedrohung gefunden wurde. Richten Sie sich nicht nach der Anforderungs Überprüfung, um Ihre Anwendung vor Site übergreifenden Skript Angriffen zu schützen. Überprüfen Sie stattdessen alle Eingaben von Benutzern, und codieren Sie die Ausgabe. In einigen eingeschränkten Fällen können Sie reguläre Ausdrücke verwenden, um die Eingabe zu validieren, aber in komplizierteren Fällen sollten Sie die Benutzereingabe mithilfe von .NET-Klassen validieren, die bestimmen, ob der Wert mit zulässigen Werten übereinstimmt.

Im folgenden Beispiel wird gezeigt, wie eine statische Methode in der URI-Klasse verwendet wird, um zu bestimmen, ob der von einem Benutzer bereitgestellte URI gültig ist.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Um jedoch den URI ausreichend zu überprüfen, sollten Sie auch überprüfen, ob `http` oder `https`angegeben wird. Im folgenden Beispiel wird Instanzmethoden verwendet, um zu überprüfen, ob der URI gültig ist.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Bevor Sie Benutzereingaben als HTML-Code oder Benutzereingaben in eine SQL-Abfrage einschließen, codieren Sie die Werte, um sicherzustellen, dass bösartiger Code nicht eingeschlossen wird.

Sie können den Wert im Markup mit der &lt;%:%&gt; Syntax in HTML codieren, wie unten gezeigt.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

In Razor-Syntax können Sie die Codierung auch mit @ codieren, wie unten gezeigt.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

Im nächsten Beispiel wird gezeigt, wie Sie einen Wert im Code Behind in HTML codieren.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Verwenden Sie Befehlsparameter wie [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx), um einen Wert für SQL-Befehle sicher zu codieren. <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Formular Authentifizierung und Sitzung ohne cookielose Authentifizierung

Empfehlung: Cookies erforderlich.

Das übergeben von Authentifizierungsinformationen in der Abfrage Zeichenfolge ist nicht sicher. Daher sind Cookies erforderlich, wenn Ihre Anwendung die Authentifizierung einschließt. Wenn Ihr Cookie vertrauliche Informationen speichert, sollten Sie SSL für das Cookie benötigen.

Im folgenden Beispiel wird gezeigt, wie in der Datei "Web. config" angegeben wird, dass die Formular Authentifizierung ein Cookie erfordert, das über SSL übertragen wird.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Empfehlung: nie auf false festgelegt.

Standardmäßig ist enableviewstatuemac auf true festgelegt. Auch wenn Ihre Anwendung keinen Ansichts Zustand verwendet, legen Sie enableViewStateMac nicht auf false fest. Wenn Sie diesen Wert auf false festlegen, wird Ihre Anwendung anfällig für Website übergreifende Skripts.

Beginnend mit ASP.NET 4.5.2 erzwingt die Runtime **enableviewstatuemac = true**. Auch wenn Sie den Wert auf false festlegen, wird dieser Wert von der Laufzeit ignoriert, und der Wert wird auf true festgelegt. Weitere Informationen finden Sie unter [ASP.NET 4.5.2 and enableviewstatuemac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Im folgenden Beispiel wird gezeigt, wie "enableviewstatuemac" auf "true" festgelegt wird. Sie müssen diesen Wert nicht auf "true" festlegen, da er standardmäßig "true" ist. Wenn Sie jedoch auf einer beliebigen Seite in der Anwendung auf false festgelegt haben, müssen Sie diesen Wert sofort korrigieren.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Mittlere Vertrauensstellung

Empfehlung: Sie sind nicht von mittlerer Vertrauensstellung (oder einer anderen Vertrauens Ebene) als Sicherheitsgrenze abhängig.

Teilweise Vertrauenswürdigkeit schützt Ihre Anwendung nicht angemessen und sollte nicht verwendet werden. Verwenden Sie stattdessen die volle Vertrauenswürdigkeit, und isolieren Sie nicht vertrauenswürdige Anwendungen in separaten Anwendungs Pools. Führen Sie auch jeden Anwendungs Pool unter einer eindeutigen Identität aus. Weitere Informationen finden Sie unter [ASP.net partielle Vertrauenswürdigkeit garantiert keine Anwendungs Isolation](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Empfehlung: Deaktivieren Sie die Sicherheitseinstellungen in &lt;appSettings-&gt; Element nicht.

Das appSettings-Element enthält viele Werte, die für Sicherheitsupdates erforderlich sind. Diese Werte sollten nicht geändert oder deaktiviert werden. Wenn Sie diese Werte beim Bereitstellen eines Updates deaktivieren müssen, aktivieren Sie Sie nach Abschluss der Bereitstellung sofort erneut.

Weitere Informationen finden Sie unter [ASP.net appSettings-Element](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Empfehlung: Verwenden Sie stattdessen [urlencode](https://msdn.microsoft.com/library/zttxte6w.aspx) .

Die urlpathencode-Methode wurde der .NET Framework hinzugefügt, um ein sehr spezifisches Browser Kompatibilitätsproblem zu beheben. Sie codiert eine URL nicht ordnungsgemäß und schützt Ihre Anwendung nicht vor Cross-Site Scripting. Sie sollten Sie niemals in Ihrer Anwendung verwenden. Verwenden Sie stattdessen [urlencode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Im folgenden Beispiel wird gezeigt, wie eine codierte URL als Abfrage Zeichenfolgen-Parameter für ein Hyperlink-Steuerelement übergeben wird.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Zuverlässigkeit und Leistung

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders und PreSendRequestContent

Empfehlung: Verwenden Sie diese Ereignisse nicht mit verwalteten Modulen. Schreiben Sie stattdessen ein natives IIS-Modul, um die erforderliche Aufgabe auszuführen. Siehe [Erstellen von HTTP-Modulen](https://msdn.microsoft.com/library/ms693629.aspx)mit System eigenem Code.

Sie können die Ereignisse " [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) " und " [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) " mit nativen IIS-Modulen verwenden.
> [!WARNING]
> Verwenden Sie `PreSendRequestHeaders` und `PreSendRequestContent` nicht mit verwalteten Modulen, die `IHttpModule`implementieren. Durch Festlegen dieser Eigenschaften können Probleme mit asynchronen Anforderungen verursacht werden. Die Kombination aus dem Anwendungs angeforderten Routing (arr) und websockets kann zu Zugriffs Verletzungs Ausnahmen führen, die zu einem Absturz der w3wp führen können. Beispiel: iiscore! W3_CONTEXT_BASE:: getislastnotification + 68 in "iiscore. dll" hat eine Zugriffs Verletzungs Ausnahme verursacht (0xc0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Asynchrone Seiten Ereignisse mit Web Forms

Empfehlung: vermeiden Sie in Web Forms das Schreiben von asynchronen void-Methoden für Seiten Lebenszyklus Ereignisse, und verwenden Sie stattdessen [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) für asynchronen Code.

Wenn Sie ein Seiten Ereignis mit " **Async** " und " **void**" markieren, können Sie nicht ermitteln, wann der asynchrone Code abgeschlossen wurde. Verwenden Sie stattdessen Page. RegisterAsyncTask, um den asynchronen Code so auszuführen, dass Sie den Abschluss nachverfolgen können.

Das folgende Beispiel zeigt einen Click-Handler für Schaltflächen, der asynchronen Code enthält. Dieses Beispiel umfasst das asynchrone Lesen eines Zeichen folgen Werts, der nur als vereinfachtes Beispiel für eine asynchrone Aufgabe und nicht als empfohlene Vorgehensweise bereitgestellt wird.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Wenn Sie asynchrone Aufgaben verwenden, legen Sie das Ziel Framework für die HTTP-Laufzeit in der Datei "Web. config" auf 4,5 (oder höher) fest. Wenn Sie das Ziel Framework auf 4,5 festlegen, wird der neue Synchronisierungs Kontext eingeschaltet, der in .NET 4,5 hinzugefügt wurde. Dieser Wert wird standardmäßig in neuen Projekten in Visual Studio festgelegt, aber nicht festgelegt, wenn Sie mit einem vorhandenen Projekt arbeiten.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Fire-and-Forget-Arbeit

Empfehlung: vermeiden Sie bei der Verarbeitung einer Anforderung in ASP.NET das Starten von Fire-and-Forget-Arbeit (z. b. das Aufrufen der Thread Pool. QueueUserWorkItem-Methode oder das Erstellen eines Timers, der wiederholt einen Delegaten aufruft)

Wenn Ihre Anwendung Fire-and-Forget-Arbeit in ASP.NET ausführt, kann die Anwendung nicht synchronisiert werden. Die APP-Domäne kann jederzeit zerstört werden. Dies bedeutet, dass der laufende Prozess möglicherweise nicht mehr mit dem aktuellen Status der Anwendung identisch ist.

Sie sollten diese Art von Arbeit außerhalb von ASP.net verschieben. Sie können einen Webauftrag, einen Windows-Dienst oder eine workerrolle in Azure verwenden, um laufende Aufgaben auszuführen und den Code aus einem anderen Prozess auszuführen.

Wenn Sie diese Arbeit in ASP.net ausführen müssen, können Sie das nuget-Paket namens [webbackgroat](http://www.nuget.org/packages/webbackgrounder) hinzufügen, um den Code auszuführen.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Anforderungs Entitäts Text

Empfehlung: vermeiden Sie das Lesen von "Request. Form" oder "Request. InputStream" vor dem Ausführungs Ereignis des Handlers.

Der früheste, den Sie aus "Request. Form" oder "Request. InputStream" lesen sollten, ist während des Execute-Ereignisses des Handlers. In MVC ist der Controller der-Handler, und das Execute-Ereignis wird ausgeführt, wenn die Aktionsmethode ausgeführt wird. In Web Forms ist die Seite der-Handler, und das Execute-Ereignis ist, wenn das Page. Init-Ereignis ausgelöst wird. Wenn Sie den Anforderungs Entitäts Text vor dem Execute-Ereignis lesen, stören Sie die Verarbeitung der Anforderung.

Wenn Sie den Anforderungs Entitäts Text vor dem Execute-Ereignis lesen müssen, verwenden Sie entweder [Request. getbufferlessinputstream](https://msdn.microsoft.com/library/ff406798.aspx) oder [Request. getbufferedinputstream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Wenn Sie getbufferlessinputstream verwenden, erhalten Sie den unformatierten Datenstrom aus der Anforderung und übernehmen die Verantwortung für die Verarbeitung der gesamten Anforderung. Nach dem Aufrufen von getbufferlessinputstream sind Request. Form und Request. InputStream nicht verfügbar, da Sie nicht von ASP.net aufgefüllt wurden. Wenn Sie getbufferedinputstream verwenden, erhalten Sie eine Kopie des Streams aus der Anforderung. Request. Form und Request. InputStream sind später in der Anforderung weiterhin verfügbar, da ASP.net die andere Kopie auffüllt.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect und Response. End

Empfehlung: Beachten Sie die Unterschiede in der Behandlung von Threads nach dem Aufrufen von [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

Die [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) -Methode ruft die Response. End-Methode auf. Bei einem synchronen Prozess bewirkt das Aufrufen von Request. Redirect, dass der aktuelle Thread sofort abgebrochen wird. Bei einem asynchronen Prozess bricht das Aufrufen von Response. Redirect den aktuellen Thread jedoch nicht ab, sodass die Codeausführung für die Anforderung fortgesetzt wird. In einem asynchronen Prozess müssen Sie die-Aufgabe von der-Methode zurückgeben, um die Codeausführung zu beenden.

In einem MVC-Projekt sollte "Response. Redirect" nicht aufgerufen werden. Stattdessen wird ein redirectresult zurückgegeben.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState und ViewStateMode

Empfehlung: Verwenden Sie ViewStateMode anstelle von EnableViewState, um genau zu steuern, welche Steuerelemente den Ansichts Zustand verwenden.

Wenn Sie in der Page-Direktive EnableViewState auf false festlegen, wird der Ansichts Zustand für alle Steuerelemente innerhalb der Seite deaktiviert und kann nicht aktiviert werden. Wenn Sie den Ansichts Zustand nur für bestimmte Steuerelemente in der Seite aktivieren möchten, legen Sie für die Seite ViewStateMode auf deaktiviert fest.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Legen Sie dann ViewStateMode nur für die Steuerelemente, die tatsächlich den Ansichts Zustand benötigen, auf aktiviert fest.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Wenn Sie den Ansichts Zustand nur für die Steuerelemente aktivieren, die ihn benötigen, können Sie die Größe des Ansichts Zustands für Ihre Webseiten verkleinern.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>Sqlmitgliedshipprovider

Empfehlung: Verwenden Sie universelle Anbieter.

In den aktuellen Projektvorlagen wurde sqlmembership shipprovider durch [ASP.net-universelle Anbieter](http://www.nuget.org/packages/Microsoft.AspNet.Providers)ersetzt, das als nuget-Paket verfügbar ist. Wenn Sie sqlmembership shipprovider in einem Projekt verwenden, das mit einer früheren Version der Vorlagen erstellt wurde, sollten Sie zu universelle Anbieter wechseln. Die universelle Anbieter arbeiten mit allen Datenbanken, die von Entity Framework unterstützt werden.

Weitere Informationen finden Sie unter [Einführung in ASP.net-universelle Anbieter](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Lange Ausführungsanforderungen (> 110 Sekunden)

Empfehlung: Verwenden Sie [websockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) oder [signalr](../../../signalr/index.md) für verbundene Clients, und verwenden Sie asynchrone e/a-Vorgänge.

Anforderungen mit langer Ausführungszeit können unvorhersehbare Ergebnisse und eine schlechte Leistung in Ihrer Webanwendung verursachen. Die standardmäßige Timeout Einstellung für eine Anforderung beträgt 110 Sekunden. Wenn Sie den Sitzungszustand mit einer Anforderung mit langer Ausführungszeit verwenden, gibt ASP.net die Sperre für das Sitzungs Objekt nach 110 Sekunden frei. Die Anwendung befindet sich jedoch möglicherweise in der Mitte eines Vorgangs für das Sitzungs Objekt, wenn die Sperre aufgehoben wird, und der Vorgang wird möglicherweise nicht erfolgreich abgeschlossen. Wenn eine zweite Anforderung vom Benutzer blockiert wird, während die erste Anforderung ausgeführt wird, kann die zweite Anforderung auf das Sitzungs Objekt in einem inkonsistenten Zustand zugreifen.

Wenn Ihre Anwendung blockierende (oder synchrone) e/a-Vorgänge umfasst, reagiert die Anwendung nicht mehr.

Verwenden Sie die asynchronen e/a-Vorgänge in der .NET Framework, um die Leistung zu verbessern. Verwenden Sie auch websockets oder signalr zum Verbinden von Clients mit dem Server. Diese Features sind für die effiziente Verarbeitung von Anforderungen mit langer Ausführungszeit konzipiert.
