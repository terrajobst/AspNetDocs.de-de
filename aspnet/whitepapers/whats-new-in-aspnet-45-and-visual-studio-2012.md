---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Neuerungen in ASP.NET 4,5 und Visual Studio 2012 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument werden die neuen Features und Verbesserungen beschrieben, die in ASP.NET 4,5 eingeführt werden. Außerdem werden Verbesserungen für die Webentwicklung beschrieben...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422733"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Neue Funktionen in ASP.NET 4.5 und Visual Studio 2012

> In diesem Dokument werden die neuen Features und Verbesserungen beschrieben, die in ASP.NET 4,5 eingeführt werden. Außerdem werden Verbesserungen für die Webentwicklung in Visual Studio 2012 beschrieben. Dieses Dokument wurde ursprünglich am 29. Februar 2012 veröffentlicht.

- [ASP.net Core Laufzeit und Framework](#_Toc318097372)

    - [Asynchrones Lesen und Schreiben von HTTP-Anforderungen und-Antworten](#_Toc318097373)
    - [Verbesserungen bei der HttpRequest-Behandlung](#_Toc318097374)
    - [Asynchrones leeren einer Antwort](#_Toc318097375)
    - [Unterstützung für *warte* -und *Aufgaben*basierte asynchrone Module und Handler](#_Toc318097376)
    - [Asynchrone HTTP-Module](#_Toc318097377)
    - [Asynchrone HTTP-Handler](#_Toc318097378)
    - [Neue ASP.net-Anforderungs Überprüfungs Features](#_Toc318097379)
    - [Verzögerte (verzögerte) Anforderungs Validierung](#_Toc318097380)
    - [Unterstützung für nicht validierte Anforderungen](#_Toc318097381)
    - [Antixss-Bibliothek](#_Toc318097382)
    - [Unterstützung für das websockets-Protokoll](#_Toc318097383)
    - [Bündelung und Minimierung](#_Toc318097384)
    - [Leistungsverbesserungen für Webhosting](#_Toc_perf)

        - [Wichtige Leistungsfaktoren](#_Toc_perf_1)
        - [Anforderungen für neue Leistungs Features](#_Toc_perf_2)
        - [Gemeinsame Assemblys](#_Toc_perf_3)
        - [Verwenden der Multi-Core-JIT-Kompilierung für schnellere Starts](#_Toc_perf_4)
        - [Optimieren Garbage Collection, um den Arbeitsspeicher zu optimieren](#_Toc_perf_5)
        - [Vorabruf für Webanwendungen](#_Toc_perf_6)
- [ASP.net Web Forms](#_Toc318097385)

    - [Stark typisierte Datensteuerelemente](#_Toc318097386)
    - [Modellbindung](#_Toc318097387)

        - [Auswählen von Daten](#_Toc318097388)
        - [Wert Anbieter](#_Toc318097389)
        - [Filtern nach Werten eines Steuer Elements](#_Toc318097390)
    - [HTML-codierte Daten Bindungs Ausdrücke](#_Toc318097391)
    - [Unaufdringliche Validierung](#_Toc318097392)
    - [HTML5-Updates](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.net Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projektfreigabe zwischen Visual Studio 2010 und Visual Studio 2012 Release Candidate (Projekt Kompatibilität)](#project-compatibility)
    - [Konfigurationsänderungen in ASP.NET 4,5-Website Vorlagen](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Native Unterstützung in IIS 7 für ASP.NET Routing](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML-Editor](#_Toc318097397)

        - [Intelligente Aufgaben](#_Toc318097398)
        - [Unterstützung von WAI-ARIA](#_Toc318097399)
        - [Neue HTML5-Ausschnitte](#_Toc318097400)
        - [In Benutzer Steuerelement extrahieren](#_Toc318097401)
        - [IntelliSense für Code-Nuggets in Attributen](#_Toc318097402)
        - [Automatisches Umbenennen des passenden Tags beim Umbenennen eines öffnenden oder Endtags](#_Toc318097403)
        - [Generierung von Ereignis Handlern](#_Toc318097404)
        - [Intelligenter Einzug](#_Toc318097405)
        - [Automatische Reduzierung der Anweisungs Vervollständigung](#_Toc318097406)
    - [JavaScript-Editor](#_Toc318097407)

        - [Code Gliederung](#_Toc318097408)
        - [Klammer Zuordnung](#_Toc318097409)
        - [Gehe zu Definition](#_Toc318097410)
        - [ECMAScript5-Unterstützung](#_Toc318097411)
        - [Dom-IntelliSense](#_Toc318097412)
        - [Vsdoc-Signatur Überladungen](#_Toc318097413)
        - [Implizite Verweise](#_Toc318097414)
    - [CSS-Editor](#_Toc318097415)

        - [Automatische Reduzierung der Anweisungs Vervollständigung](#_Toc318097416)
        - [Hierarchischer Einzug.](#_Toc318097417)
        - [CSS-Unterstützung](#_Toc318097418)
        - [Anbieterspezifische Schemas (--,-WebKit)](#_Toc318097419)
        - [Unterstützung für Kommentare und unkommentare](#_Toc318097420)
        - [Farbauswahl](#_Toc318097421)
        - [Snippets (Ausschnitte)](#_Toc318097422)
        - [Benutzerdefinierte Regionen](#_Toc318097423)
    - [Seitenprüfung](#_Toc318097424)
    - [Veröffentlichen](#_Toc318097425)

        - [Veröffentlichungsprofile](#_Toc318097426)
        - [ASP.NET Vorkompilierung und Zusammenführen](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [ERS](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.net Core Laufzeit und Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchrones Lesen und Schreiben von HTTP-Anforderungen und-Antworten

In ASP.NET 4 wurde die Möglichkeit eingeführt, eine HTTP-Anforderungs Entität mithilfe der *HttpRequest. getbufferlessinputstream* -Methode als Datenstrom zu lesen. Diese Methode hat Streamingzugriff auf die Anforderungs Entität bereitgestellt. Es wurde jedoch synchron ausgeführt, wodurch ein Thread für die Dauer einer Anforderung gebunden wurde.

ASP.NET 4,5 unterstützt die Möglichkeit, Streams in einer HTTP-Anforderungs Entität asynchron zu lesen und asynchron zu leeren. ASP.NET 4,5 bietet Ihnen außerdem die Möglichkeit, eine HTTP-Anforderungs Entität zu verdoppeln, die eine einfachere Integration in downstreamhttp-Handler wie ASPX-Seiten Handler und ASP.NET MVC-Controller ermöglicht.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Verbesserungen bei der HttpRequest-Behandlung

Der von ASP.NET 4,5 von *HttpRequest. getbufferlessinputstream* zurückgegebene Datenstrom Verweis unterstützt sowohl synchrone als auch asynchrone Lesemethoden. Das von *getbufferlessinputstream* zurückgegebene *Stream* -Objekt implementiert jetzt sowohl die BeginRead-als auch die EndRead-Methode. Mit den asynchronen Daten *Strom* Methoden können Sie die Anforderungs Entität asynchron in Blöcken lesen, während ASP.NET den aktuellen Thread zwischen den einzelnen Iterationen einer asynchronen Lese Schleife freigibt.

ASP.NET 4,5 hat auch eine Begleit Methode zum Lesen der Anforderungs Entität auf eine gepufferte Weise hinzugefügt: *HttpRequest. getbufferedinputstream*. Diese neue Überladung funktioniert wie *getbufferlessinputstream*und unterstützt synchrone und asynchrone Lesevorgänge. Beim Lesen von Daten kopiert *getbufferedinputstream* jedoch auch die Entitäts Bytes in interne ASP.net-Puffer, sodass downstreammodule und Handler weiterhin auf die Anforderungs Entität zugreifen können. Wenn beispielsweise ein upstreamcode in der Pipeline die Anforderungs Entität bereits mithilfe von *getbufferedinputstream*gelesen hat, können Sie weiterhin *HttpRequest. Form* oder *HttpRequest. Files*verwenden. Auf diese Weise können Sie eine asynchrone Verarbeitung für eine Anforderung ausführen (z. b. das Streaming eines großen Datei Uploads in eine Datenbank), aber nach wie vor die ASPX-Seiten und MVC-ASP.net-Controller ausführen.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchrones leeren einer Antwort

Das Senden von Antworten an einen HTTP-Client kann viel Zeit in Anspruch nehmen, wenn der Client weit entfernt ist oder eine Verbindung mit geringer Bandbreite aufweist. Normalerweise ASP.net puffert die Antwort bytes, wenn Sie von einer Anwendung erstellt werden. ASP.NET führt dann am Ende der Anforderungs Verarbeitung einen einzelnen Sendevorgang der angewendeten Puffer aus.

Wenn die gepufferte Antwort groß ist (z. b. das Streaming einer großen Datei auf einen Client), müssen Sie in regelmäßigen Abständen *HttpResponse. Flush* aufrufen, um die gepufferte Ausgabe an den Client zu senden und die Speicherauslastung unter Kontrolle zu halten. Da *Flush* jedoch ein synchroner Aufruf ist, verbraucht das iterative Aufrufen von *Flush* weiterhin einen Thread für die Dauer von potenziell langen Ausführungsanforderungen.

ASP.NET 4,5 bietet Unterstützung für das asynchrone Ausführen von Leerungen mithilfe der *beginflush* -Methode und der *endflush* -Methode der *HttpResponse* -Klasse. Mit diesen Methoden können Sie asynchrone Module und asynchrone Handler erstellen, die inkrementell Daten an einen Client senden, ohne Betriebssystemthreads zu binden. Zwischen *beginflush* -und *endflush* -aufrufen gibt ASP.NET den aktuellen Thread frei. Dadurch wird die Gesamtzahl aktiver Threads erheblich reduziert, die benötigt werden, um http-Downloads mit langer Laufzeit zu unterstützen.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Unterstützung für *warte* -und *Aufgaben* basierte asynchrone Module und Handler

In .NET Framework 4 wurde ein asynchrones Programmier Konzept eingeführt, das als *Aufgabe*bezeichnet wird. Tasks werden durch den *Tasktyp und* verwandte Typen im *System. Threading. Tasks* -Namespace dargestellt. Der .NET Framework 4,5 baut auf diesem mit compilererweiterungen auf, die das Arbeiten mit *Task* Objekten einfach machen. Im .NET Framework 4,5 unterstützen die Compiler zwei neue Schlüsselwörter: " *warten* " und " *Async*". Das *Wait* -Schlüsselwort ist syntaktische Kurzwort, um anzugeben, dass ein Code Element asynchron auf einen anderen Code Abschnitt warten soll. Das *Async* -Schlüsselwort stellt einen Hinweis dar, den Sie verwenden können, um Methoden als aufgabenbasierte asynchrone Methoden zu markieren.

Die Kombination aus " *warten*", " *Async*" und " *Task* " erleichtert Ihnen das Schreiben von asynchronem Code in .NET 4,5. ASP.NET 4,5 unterstützt diese Vereinfachungen mit neuen APIs, mit denen Sie asynchrone HTTP-Module und asynchrone HTTP-Handler mithilfe der neuen compilererweiterungen schreiben können.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchrone HTTP-Module

Angenommen, Sie möchten asynchrone Arbeit innerhalb einer Methode ausführen, die ein *Task* -Objekt zurückgibt. Im folgenden Codebeispiel wird eine asynchrone Methode definiert, die einen asynchronen aufrufsbefehl zum Herunterladen der Microsoft-Startseite ausführt. Beachten Sie die Verwendung des *Async* -Schlüssel Worts in der Methoden Signatur und des Erwartungs Aufrufes *downloadstringtaskasync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Das ist alles, was Sie schreiben müssen – die .NET Framework behandelt automatisch die aufruppe der aufruppe, während auf den Abschluss des Downloads gewartet wird, sowie die automatische Wiederherstellung der Rückruf Stapel, nachdem der Download abgeschlossen ist.

Nehmen Sie nun an, dass Sie diese asynchrone Methode in einem asynchronen ASP.net-http-Modul verwenden möchten. ASP.NET 4,5 enthält eine Hilfsmethode (*eventhandlertaskasynchelper*) und einen neuen Delegattyp (*taskeventhandler*), den Sie verwenden können, um aufgabenbasierte asynchrone Methoden mit dem älteren asynchronen Programmiermodell zu integrieren, das von der HTTP-Pipeline ASP.net verfügbar gemacht wird. Dieses Beispiel zeigt Folgendes:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchrone HTTP-Handler

Der herkömmliche Ansatz zum Schreiben von asynchronen Handlern in ASP.net ist die Implementierung der *IHttpAsyncHandler* -Schnittstelle. ASP.NET 4,5 führt den asynchronen *httptaskasynchandler* -Basistyp ein, von dem Sie ableiten können, wodurch das Schreiben von asynchronen Handlern erheblich vereinfacht wird.

Der *httptaskasynchandler* -Typ ist abstrakt und erfordert, dass Sie die *processrequestasync* -Methode überschreiben. Intern ASP.net übernimmt die Integration der Rückgabe Signatur (ein *Task* -Objekt) von *processrequestasync* in das ältere asynchrone Programmiermodell, das von der ASP.NET-Pipeline verwendet wird.

Im folgenden Beispiel wird gezeigt, wie Sie *Aufgaben* verwenden und als Teil der Implementierung eines asynchronen HTTP-Handlers *warten* können:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Neue ASP.net-Anforderungs Überprüfungs Features

ASP.NET führt standardmäßig eine Anforderungs Überprüfung durch – Sie prüft Anforderungen, um in Feldern, Headern, Cookies usw. nach Markup oder Skripts zu suchen. Wenn ein beliebiges erkannt wird, löst ASP.net eine Ausnahme aus. Dies fungiert als erste Verteidigungslinie gegen mögliche Site übergreifende Skript Angriffe.

Mit ASP.NET 4,5 ist es leicht, nicht validierte Anforderungs Daten selektiv zu lesen. ASP.NET 4,5 integriert auch die beliebte antixss-Bibliothek, die zuvor eine externe Bibliothek war.

Entwickler haben häufig gefragt, ob Sie die Anforderungs Validierung für Ihre Anwendungen selektiv deaktivieren möchten. Wenn es sich bei Ihrer Anwendung beispielsweise um eine Forumssoftware handelt, können Sie es Benutzern ermöglichen, im HTML-Format Beiträge und Kommentare zu senden, aber trotzdem sicherzustellen, dass die Anforderungs Validierung alles andere überprüft.

ASP.NET 4,5 führt zwei Features ein, die Ihnen das selektive arbeiten mit nicht validierten Eingaben erleichtern: verzögerte (verzögerte) Anforderungs Validierung und Zugriff auf nicht validierte Anforderungs Daten.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Verzögerte (verzögerte) Anforderungs Validierung

In ASP.NET 4,5 unterliegen alle Anforderungs Datenstandard mäßig der Anforderungs Validierung. Allerdings können Sie die Anwendung so konfigurieren, dass die Anforderungs Validierung verzögert wird, bis Sie tatsächlich auf Anforderungs Daten zugreifen. (Dies wird mitunter als verzögerte Anforderungs Validierung bezeichnet, die auf Begriffen wie Lazy Loading für bestimmte Daten Szenarien basiert.) Sie können die Anwendung so konfigurieren, dass die verzögerte Validierung in der Datei "Web. config" verwendet wird, indem Sie das *requestvalidationmode* -Attribut im *HttpRuntime* -Element auf 4,5 festlegen, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Wenn der Anforderungs Validierungs Modus auf 4,5 festgelegt ist, wird die Anforderungs Validierung nur für einen bestimmten Anforderungs Wert und nur dann ausgelöst, wenn Ihr Code auf diesen Wert zugreift. Wenn Ihr Code z. b. den Wert von Request. Form ["Forum\_Post"] abruft, wird die Anforderungs Validierung nur für dieses Element in der Formular Auflistung aufgerufen. Keines der anderen Elemente in der *Formular* Auflistung wird überprüft. In früheren Versionen von ASP.net wurde die Anforderungs Validierung für die gesamte Anforderungs Auflistung ausgelöst, als auf ein Element in der Auflistung zugegriffen wurde. Durch das neue Verhalten können unterschiedliche Anwendungskomponenten verschiedene Teile von Anforderungs Daten untersuchen, ohne dass die Anforderungs Validierung für andere Teile ausgelöst wird.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Unterstützung für nicht validierte Anforderungen

Die verzögerte Anforderungs Validierung allein löst nicht das Problem der selektiven Umgehung der Anforderungs Validierung. Der callrequest. Form ["Forum\_Post"] löst weiterhin die Anforderungs Validierung für diesen spezifischen Anforderungs Wert aus. Möglicherweise möchten Sie jedoch auf dieses Feld zugreifen, ohne die Validierung auszulösen, da Sie Markup in diesem Feld zulassen möchten.

Um dies zuzulassen, unterstützt ASP.NET 4,5 jetzt nicht validierten Zugriff auf Anforderungs Daten. ASP.NET 4,5 enthält eine neue *nicht validierte* Sammlungs Eigenschaft in der *HttpRequest* -Klasse. Diese Auflistung bietet Zugriff auf alle allgemeinen Werte von Anforderungs Daten, wie z. b. *Form*, *QueryString*, *Cookies*und *URL*.

Mithilfe des Forums Beispiels, um nicht validierte Anforderungs Daten lesen zu können, müssen Sie zunächst die Anwendung so konfigurieren, dass Sie den neuen Anforderungs Validierungs Modus verwendet:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Sie können dann die *HttpRequest. unvalivalidierte* Eigenschaft verwenden, um den nicht validierten Formular Wert zu lesen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Sicherheit: *verwendet nicht validierte Anforderungs Daten mit Bedacht!* ASP.NET 4,5 hat die nicht validierten Anforderungs Eigenschaften und-Sammlungen hinzugefügt, um Ihnen den Zugriff auf sehr spezifische nicht validierte Anforderungs Daten zu erleichtern. Allerdings müssen Sie weiterhin eine benutzerdefinierte Validierung der unformatierten Anforderungs Daten durchführen, um sicherzustellen, dass gefährliche Text nicht für Benutzer gerendert wird.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Antixss-Bibliothek

Aufgrund der Beliebtheit der Microsoft antixss-Bibliothek enthält ASP.NET 4,5 jetzt Kern Codierungs Routinen aus Version 4,0 dieser Bibliothek.

Die Codierungs Routinen werden vom *antixssencoder* -Typ im neuen *System. Web. Security. antixss* -Namespace implementiert. Sie können den *antixssencoder* -Typ direkt verwenden, indem Sie eine der statischen Codierungs Methoden aufrufen, die im-Typ implementiert werden. Der einfachste Ansatz für die Verwendung der neuen Anti-XSS-Routinen besteht jedoch darin, eine ASP.NET-Anwendung so zu konfigurieren, dass die *antixssencoder* -Klasse standardmäßig verwendet wird. Fügen Sie zu diesem Zweck der Datei "Web. config" das folgende Attribut hinzu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Wenn das *encodertype* -Attribut auf den *antixssencoder* -Typ festgelegt ist, verwendet die gesamte Ausgabe Codierung in ASP.NET automatisch die neuen Codierungs Routinen.

Dabei handelt es sich um die Teile der externen antixss-Bibliothek, die in ASP.NET 4,5 integriert wurden:

- *HtmlEncode*, *htmlformurlencode*und *HtmlAttributeEncode*
- *Xmlattributeencode* und *xmlencode*
- *Urlencode* und *urlpathencode* (neu)
- *Cssencode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Unterstützung für das websockets-Protokoll

Das websockets-Protokoll ist ein Standard basiertes Netzwerkprotokoll, das definiert, wie eine sichere bidirektionale Echtzeitkommunikation zwischen einem Client und einem Server über HTTP hergestellt wird. Microsoft hat sowohl den IETF-als auch den W3C-Standard Textteilen zum Definieren des Protokolls eingesetzt. Das websockets-Protokoll wird von jedem Client (nicht nur von Browsern) unterstützt, und Microsoft investiert beträchtliche Ressourcen, die das websockets-Protokoll sowohl auf Client-als auch auf mobilen Betriebssystemen unterstützen.

Das websockets-Protokoll vereinfacht das Erstellen von Datenübertragungen mit langer Laufzeit zwischen einem Client und einem Server. Beispielsweise ist es viel einfacher, eine Chat-Anwendung zu schreiben, da Sie eine echte lang andauernden Verbindung zwischen einem Client und einem Server herstellen können. Sie müssen nicht auf Problem Umgehungen wie periodische Abruf Vorgänge oder http-Abfragen mit langer Ausführungszeit zurückgreifen, um das Verhalten eines Sockets zu simulieren.

ASP.NET 4,5 und IIS 8 enthalten websockets-Unterstützung auf niedriger Ebene, sodass ASP.NET-Entwickler verwaltete APIs für das asynchrone Lesen und Schreiben von Zeichen folgen-und Binärdaten in einem websockets-Objekt verwenden können. Für ASP.NET 4,5 gibt es einen neuen *System. Web. websockets* -Namespace, der Typen für die Arbeit mit dem websockets-Protokoll enthält.

Ein Browser Client richtet eine websockets-Verbindung ein, indem er ein DOM- *WebSocket* -Objekt erstellt, das auf eine URL in einer ASP.NET-Anwendung zeigt, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Sie können websockets-Endpunkte in ASP.NET erstellen, indem Sie eine beliebige Art von Modul oder Handler verwenden. Im vorherigen Beispiel wurde eine ASHX-Datei verwendet, da ASHX-Dateien eine schnelle Möglichkeit zum Erstellen eines Handlers sind.

Gemäß dem websockets-Protokoll akzeptiert eine ASP.NET-Anwendung die websockets-Anforderung eines Clients, indem angegeben wird, dass die Anforderung von einer HTTP GET-Anforderung auf eine websockets-Anforderung aktualisiert werden soll. Im Folgenden ein Beispiel:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Die Methode " *akzeptwebsocketrequest* " akzeptiert einen Funktions Delegaten, da ASP.net die aktuelle HTTP-Anforderung entlädt und dann die Steuerung an den Funktions Delegaten überträgt. Konzeptionell ist dieser Ansatz vergleichbar mit der Verwendung von " *System. Threading. Thread*", bei dem Sie einen Thread Start-Delegaten definieren, in dem Hintergrundarbeit durchgeführt wird.

Nachdem ASP.net und der Client einen websockets-Handshake erfolgreich abgeschlossen haben, ruft ASP.net ihren Delegaten auf, und die websockets-Anwendung wird gestartet. Das folgende Codebeispiel zeigt eine einfache Echo Anwendung, die die integrierte websockets-Unterstützung in ASP.NET verwendet:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Die Unterstützung in .NET 4,5 für *das Erwartungs* Wort und asynchrone aufgabenbasierte Vorgänge eignet sich für das Schreiben von websockets-Anwendungen. Das Codebeispiel zeigt, dass eine websockets-Anforderung vollständig asynchron in ASP.NET ausgeführt wird. Die Anwendung wartet asynchron auf eine Nachricht, die von einem Client gesendet werden soll, indem Sie den "warten"- *Socket aufrufen. ReceiveAsync*. Auf ähnliche Weise können Sie eine asynchrone Nachricht an einen Client senden, indem Sie den "warten"- *Socket aufrufen. SendAsync*.

Im Browser empfängt eine Anwendung websockets-Nachrichten über eine *onMessage* -Funktion. Zum Senden einer Nachricht von einem Browser aus wird die *Send* -Methode des *WebSocket* -DOM-Typs aufgerufen, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In Zukunft werden wir möglicherweise Updates für diese Funktionalität veröffentlichen, die einige der auf niedriger Ebene erforderlichen Codierungen, die in dieser Version für websockets-Anwendungen erforderlich sind, abstrahieren.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bündelung und Minimierung

Durch das bündeln können Sie einzelne JavaScript-und CSS-Dateien in einem Bündel kombinieren, das wie eine einzelne Datei behandelt werden kann. Bei der minifizierung werden JavaScript-und CSS-Dateien durch Entfernen von Leerzeichen und anderen Zeichen, die nicht erforderlich sind, verdichtet. Diese Features funktionieren mit Web Forms, ASP.NET MVC und Web Pages.

Bündel werden mithilfe der Bundle-Klasse oder einer ihrer untergeordneten Klassen, scriptbundle und stylebundle, erstellt. Nach dem Konfigurieren einer Instanz eines Pakets wird das Paket für eingehende Anforderungen bereitgestellt, indem es einfach einer globalen bundlecollection-Instanz hinzugefügt wird. In den Standardvorlagen wird die Paketkonfiguration in einer bundleconfig-Datei ausgeführt. Diese Standardkonfiguration erstellt Bündel für alle wichtigsten Skripts und CSS-Dateien, die von den Vorlagen verwendet werden.

Auf Pakete wird in Sichten mithilfe einer der paar möglichen Hilfsmethoden verwiesen. Um das Rendern verschiedener Markup für ein Paket im Debugmodus und im Releasemodus zu unterstützen, verfügen die Klassen scriptbundle und stylebundle über die Hilfsmethode rendern. Im Debugmodus generiert Rendering Markup für jede Ressource im Paket. Im Releasemodus generiert Rendering ein einzelnes Markup Element für das gesamte Paket. Das Umschalten zwischen Debug-und Releasemodus kann durch Ändern des Debug-Attributs des Elements "Compilation" in der Datei "Web. config" erreicht werden, wie unten dargestellt:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Außerdem kann die Aktivierung oder Deaktivierung der Optimierung direkt über die bundletable. enableoptimization-Eigenschaft festgelegt werden.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Wenn Dateien gebündelt werden, werden Sie zuerst alphabetisch sortiert (wie Sie in **Projektmappen-Explorer**angezeigt werden). Sie werden dann so organisiert, dass bekannte Bibliotheken und Ihre benutzerdefinierten Erweiterungen (z. b. jQuery, mootools und Dojo) zuerst geladen werden. Die abschließende Reihenfolge für die Bündelung des Skript Ordners, wie oben gezeigt, lautet beispielsweise wie folgt:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a. js

CSS-Dateien werden ebenfalls alphabetisch sortiert und dann neu organisiert, sodass "Reset. CSS" und "normalize. CSS" vor allen anderen Dateien stehen. Die abschließende Sortierung des oben gezeigten Stile-Ordners lautet wie folgt:

1. reset.css
2. Content. CSS
3. forms.css
4. globals.css
5. Menu. CSS
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Leistungsverbesserungen für Webhosting

Mit den .NET Framework 4,5 und Windows 8 werden Features eingeführt, mit denen Sie eine beträchtliche Leistungssteigerung für Web-Server-Workloads erreichen können. Dies schließt eine Reduzierung ein (bis zu 35%). sowohl in der Startzeit als auch im Speicherbedarf von Webhosting-Websites, die ASP.NET verwenden.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Wichtige Leistungsfaktoren

Im Idealfall sollten alle Websites aktiv sein und sich im Arbeitsspeicher befinden, um eine schnelle Reaktion auf die nächste Anforderung zu gewährleisten, wenn dies der Fall ist. Folgende Faktoren können sich auf die Reaktionsfähigkeit der Site auswirken:

- Die Zeit, die für das Neustarten eines Standorts nach dem erneuten Starten eines App-Pools benötigt wird. Dies ist die Zeit, die benötigt wird, um einen Webserver Prozess für die Website zu starten, wenn sich die Website-Assemblys nicht mehr im Arbeitsspeicher befinden. (Die Plattformassemblys befinden sich weiterhin im Arbeitsspeicher, da Sie von anderen Websites verwendet werden.) Diese Situation wird als "kalte Website, Start des warmen Frameworks" oder einfach als "kalte Standort Start" bezeichnet.
- Wie viel Arbeitsspeicher die Site einnimmt. Die Begriffe hierfür sind "Arbeitsspeicher Nutzung pro Standort" oder "nicht frei gegebener Workingset".

Die neuen Leistungsverbesserungen konzentrieren sich auf beide Faktoren.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Anforderungen für neue Leistungs Features

Die Anforderungen für die neuen Features können in folgende Kategorien unterteilt werden:

- Verbesserungen, die auf dem .NET Framework 4 ausgeführt werden.
- Verbesserungen, die die .NET Framework 4,5 erfordern, aber unter einer beliebigen Version von Windows ausgeführt werden können.
- Verbesserungen, die nur mit .NET Framework 4,5 unter Windows 8 verfügbar sind.

Die Leistung wird mit jeder Stufe der Verbesserung erhöht, die Sie aktivieren können.

Einige der .NET Framework 4,5-Verbesserungen profitieren von umfassenderen Leistungs Features, die auch für andere Szenarien gelten.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Gemeinsame Assemblys

**Anforderung**: .NET Framework 4 und Visual Studio 11 Developer Preview SDK

Verschiedene Websites auf einem Server verwenden häufig dieselben hilfssoftys (z. b. Assemblys aus einem Starter Kit oder einer Beispielanwendung). Jeder Standort verfügt über eine eigene Kopie dieser Assemblys im Verzeichnis "bin". Obwohl der Objektcode für die Assemblys identisch ist, sind Sie physisch getrennte Assemblys, sodass jede Assembly getrennt beim Start des kalten Standorts gelesen und separat im Arbeitsspeicher aufbewahrt werden muss.

Die neue Internalisierung-Funktionalität löst diese Ineffizienz und verringert sowohl die RAM-Anforderungen als auch die Ladezeit. Durch das zusammensetzen kann Windows eine einzelne Kopie der einzelnen Assemblys im Dateisystem beibehalten, und die einzelnen Assemblys in den Standort-bin-Ordnern werden durch symbolische Verknüpfungen zur einzelnen Kopie ersetzt. Wenn für eine einzelne Website eine eindeutige Version der Assembly erforderlich ist, wird die symbolische Verknüpfung durch die neue Version der Assembly ersetzt, und nur diese Website ist betroffen.

Das Freigeben von Assemblys mithilfe von symbolischen Verknüpfungen erfordert ein neues Tool namens ASPNET\_intern. exe, mit dem Sie den Speicher für interbenannte Assemblys erstellen und verwalten können. Sie wird als Teil von Visual Studio 11 Developer Preview SDK bereitgestellt. (Es funktioniert jedoch auf einem System, auf dem nur die .NET Framework 4 installiert ist, vorausgesetzt, dass Sie das neueste [Update](https://support.microsoft.com/kb/2468871)installiert haben.)

Um sicherzustellen, dass alle berechtigten Assemblys interniert wurden, führen Sie ASPNET\_intern. exe regelmäßig aus (z. b. einmal pro Woche als geplante Aufgabe). Eine typische Verwendung sieht wie folgt aus:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Wenn Sie alle Optionen anzeigen möchten, führen Sie das Tool ohne Argumente aus.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Verwenden der Multi-Core-JIT-Kompilierung für schnellere Starts

**Anforderung**: .NET Framework 4,5

Beim Start eines kalten Standorts müssen Assemblys nicht nur vom Datenträger gelesen, sondern auch JIT-kompiliert werden. Bei einem komplexen Standort kann dies zu erheblichen Verzögerungen führen. Eine neue allgemeine Methode im .NET Framework 4,5 reduziert diese Verzögerungen durch die Verteilung der JIT-Kompilierung auf verfügbare Prozessorkerne. Dies erfolgt so viel und so früh wie möglich durch die Verwendung von Informationen, die bei vorherigen Starts der Website gesammelt wurden. Diese Funktionalität wird von der [System. Runtime. profileoptimization. StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) -Methode implementiert.

Die JIT-Kompilierung unter Verwendung mehrerer Kerne ist standardmäßig in ASP.NET aktiviert, sodass Sie keine Maßnahmen ergreifen müssen, um dieses Feature zu nutzen. Wenn Sie diese Funktion deaktivieren möchten, legen Sie die folgende Einstellung in der Datei "Web. config" fest:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Optimieren Garbage Collection, um den Arbeitsspeicher zu optimieren

**Anforderung**: .NET Framework 4,5

Sobald eine Website ausgeführt wird, kann der Garbage Collector-Heap (GC) ein bedeutender Faktor für den Arbeitsspeicher Verbrauch sein. Wie alle Garbage Collector macht die .NET Framework GC Kompromisse zwischen der CPU-Zeit (Häufigkeit und Bedeutung von Auflistungen) und der Arbeitsspeicher Nutzung (zusätzlicher Speicherplatz, der für neue, freigegebene oder freisetzbare Objekte verwendet wird). In früheren Versionen haben wir Anleitungen zum Konfigurieren der GC bereitgestellt, um den richtigen Saldo zu erzielen (z. b. siehe [ASP.NET 2.0/3.5 Shared Hosting Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Für den .NET Framework 4,5 statt mehrerer eigenständiger Einstellungen ist eine von der Arbeitsauslastung definierte Konfigurationseinstellung verfügbar, die alle zuvor empfohlenen GC-Einstellungen sowie die neue Optimierung ermöglicht, die eine zusätzliche Leistung pro Standort bietet. Workingset.

Fügen Sie der Windows\Microsoft.Net\Framework\v4.0.30319\aspnet.config-Datei die folgende Einstellung hinzu, um die GC-Speicher Optimierung zu aktivieren:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Wenn Sie mit der vorherigen Anleitung für Änderungen an ASPNET. config vertraut sind, beachten Sie, dass diese Einstellung die alten Einstellungen ersetzt – beispielsweise ist es nicht erforderlich, gcserver, gcConcurrent usw. festzulegen. Sie müssen die alten Einstellungen nicht entfernen.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Vorabruf für Webanwendungen

**Anforderung**: .NET Framework 4,5 unter Windows 8

Bei mehreren Releases enthält Windows eine Technologie, die als " [Prefetcher](http://en.wikipedia.org/wiki/Prefetcher) " bezeichnet wird und die Kosten für den Datenträger Lesevorgang für den Anwendungsstart reduziert. Da der Kaltstart hauptsächlich bei Client Anwendungen ein Problem darstellt, ist diese Technologie nicht in Windows Server enthalten, was nur Komponenten umfasst, die für einen Server unverzichtbar sind. Der Vorabruf ist nun in der neuesten Version von Windows Server verfügbar, wo der Start einzelner Websites optimiert werden kann.

Für Windows Server ist der Prefetcher standardmäßig nicht aktiviert. Führen Sie die folgenden Befehle in der Befehlszeile aus, um den Prefetcher für Webhosting mit hoher Dichte zu aktivieren und zu konfigurieren:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Fügen Sie dann der Datei "Web. config" Folgendes hinzu, um den Prefetcher in ASP.NET-Anwendungen zu integrieren:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Stark typisierte Datensteuerelemente

In ASP.NET 4,5 umfasst Web Forms einige Verbesserungen für die Arbeit mit Daten. Die erste Verbesserung sind stark typisierte Daten Steuerelemente. Für Web Forms-Steuerelemente in früheren Versionen von ASP.net zeigen Sie einen Daten gebundenen Wert mithilfe von *eval* und einen Daten Bindungs Ausdruck an:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Für die bidirektionale Datenbindung verwenden Sie *Bind*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Zur Laufzeit verwenden diese Aufrufe Reflektion, um den Wert des angegebenen Members zu lesen und das Ergebnis dann im Markup anzuzeigen. Diese Vorgehensweise erleichtert das Binden von Daten an beliebige, unformatierte Daten.

Daten Bindungs Ausdrücke wie diese unterstützen jedoch keine Features wie IntelliSense für Elementnamen, Navigation (z. b. "Gehe zu Definition") oder die Überprüfung der Kompilierzeit für diese Namen.

Um dieses Problem zu beheben, bietet ASP.NET 4,5 die Möglichkeit, den Datentyp der Daten zu deklarieren, an die ein Steuerelement gebunden ist. Dazu verwenden Sie die neue *ItemType* -Eigenschaft. Wenn Sie diese Eigenschaft festlegen, sind zwei neue typisierte Variablen im Bereich der Daten Bindungs Ausdrücke verfügbar: *Item* und *binditem*. Da die Variablen stark typisiert sind, profitieren Sie von der Entwicklungsfunktion von Visual Studio.

Verwenden Sie für bidirektionale Daten Bindungs Ausdrücke die *binditem* -Variable:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

Die meisten Steuerelemente im ASP.net Web Forms Framework, die die Datenbindung unterstützen, wurden aktualisiert, um die *ItemType* -Eigenschaft zu unterstützen.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Modellbindung

Die Modell Bindung erweitert die Datenbindung in ASP.net-Web Forms-Steuerelementen, um mit Code zentriertem Datenzugriff zu arbeiten. Sie enthält Konzepte aus dem *ObjectDataSource* -Steuerelement und aus der Modell Bindung in ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Auswählen von Daten

Wenn Sie ein Daten Steuerelement so konfigurieren möchten, dass die Modell Bindung verwendet wird, um Daten auszuwählen, legen Sie die *SelectMethod* -Eigenschaft des Steuer Elements auf den Namen einer Methode im Code der Seite fest. Das Daten Steuerelement ruft die Methode zum entsprechenden Zeitpunkt im Lebenszyklus der Seite auf und bindet die zurückgegebenen Daten automatisch. Es ist nicht erforderlich, die *DataBind* -Methode explizit aufzurufen.

Im folgenden Beispiel wird das *GridView* -Steuerelement für die Verwendung einer Methode namens *GetCategories*konfiguriert:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Sie erstellen die *GetCategories* -Methode im Code der Seite. Für einen einfachen SELECT-Vorgang benötigt die Methode keine Parameter und sollte ein *IEnumerable* -oder *iquerable* -Objekt zurückgeben. Wenn die neue *ItemType* -Eigenschaft festgelegt ist (wodurch stark typisierte Daten Bindungs Ausdrücke aktiviert werden, wie bereits unter [stark typisierte Daten Steuerelemente](#_Toc318097386) erläutert), sollten die generischen Versionen dieser Schnittstellen – *IEnumerable&lt;t&gt;* oder *iquervable&lt;t&gt;* zurückgegeben werden, wobei der *t* -Parameter mit dem Typ der *ItemType* -Eigenschaft übereinstimmt (z. b. *iquervable&lt;Category&gt;* ).

Das folgende Beispiel zeigt den Code für eine *GetCategories* -Methode. In diesem Beispiel wird das Entity Framework Code First-Modell mit der Beispieldatenbank Northwind verwendet. Der Code stellt sicher, dass die Abfrage anhand der *include* -Methode Details der zugehörigen Produkte für jede Kategorie zurückgibt. (Dadurch wird sichergestellt, dass das *TemplateField* -Element im Markup die Anzahl der Produkte in jeder Kategorie anzeigt, ohne dass eine SELECT-Taste ( [n + 1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)) erforderlich ist.)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Wenn die Seite ausgeführt wird, ruft das *GridView* -Steuerelement die *GetCategories* -Methode automatisch auf und rendert die zurückgegebenen Daten mithilfe der konfigurierten Felder:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Da die Select-Methode ein *iquerable* -Objekt zurückgibt, kann das *GridView* -Steuerelement die Abfrage vor der Ausführung weiter bearbeiten. Das *GridView* -Steuerelement kann z. b. Abfrage Ausdrücke zum Sortieren und Paging zum zurückgegebenen *iquerable* -Objekt hinzufügen, bevor es ausgeführt wird, sodass diese Vorgänge vom zugrunde liegenden LINQ-Anbieter ausgeführt werden. In diesem Fall wird Entity Framework sicherstellen, dass diese Vorgänge in der Datenbank ausgeführt werden.

Das folgende Beispiel zeigt das *GridView* -Steuerelement, das zum Sortieren und Paging geändert wurde:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Wenn nun die Seite ausgeführt wird, kann das Steuerelement sicherstellen, dass nur die aktuelle Seite der Daten angezeigt wird, und dass Sie nach der ausgewählten Spalte geordnet ist:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Um die zurückgegebenen Daten zu filtern, müssen die Parameter der Select-Methode hinzugefügt werden. Diese Parameter werden zur Laufzeit von der Modell Bindung aufgefüllt, und Sie können Sie verwenden, um die Abfrage vor der Rückgabe der Daten zu ändern.

Nehmen Sie beispielsweise an, dass Sie Benutzern das Filtern von Produkten gestatten möchten, indem Sie ein Schlüsselwort in der Abfrage Zeichenfolge eingeben. Sie können der Methode einen Parameter hinzufügen und den Code aktualisieren, um den Parameterwert zu verwenden:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Dieser Code enthält einen *Where* -Ausdruck, wenn ein Wert für das *Schlüsselwort* bereitgestellt wird, und gibt dann die Abfrageergebnisse zurück.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Wert Anbieter

Das vorherige Beispiel war nicht spezifisch dafür, wo der Wert für den *Schlüsselwort* Parameter stammt. Um diese Informationen anzuzeigen, können Sie ein Parameter Attribut verwenden. In diesem Beispiel können Sie die *querystringattribute* -Klasse verwenden, die sich im *System. Web. modelbinding* -Namespace befindet:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Dies weist die Modell Bindung an, einen Wert aus der Abfrage Zeichenfolge zur Laufzeit an den *Schlüsselwort* Parameter zu binden. (Dies kann das Ausführen von Typkonvertierungen einschließen, obwohl dies in diesem Fall nicht der Fall ist.) Wenn kein Wert bereitgestellt werden kann und der Typ keine NULL-Werte zulässt, wird eine Ausnahme ausgelöst.

Die Quellen der Werte für diese Methoden werden als Wert Anbieter bezeichnet, und die Parameter Attribute, die angeben, welcher Wert Anbieter verwendet werden soll, werden als Wert Anbieter Attribute bezeichnet. Web Forms enthalten Wert Anbieter und zugehörige Attribute für alle typischen Quellen der Benutzereingaben in einer Web Forms Anwendung, wie z. b. Abfrage Zeichenfolge, Cookies, Formular Werte, Steuerelemente, Ansichts Zustand, Sitzungszustand und Profil Eigenschaften. Sie können auch benutzerdefinierte Wert Anbieter schreiben.

Standardmäßig wird der Parameter Name als Schlüssel verwendet, um einen Wert in der Wert Anbieter Auflistung zu finden. Im Beispiel sucht der Code nach einem Abfrage Zeichenfolgen-Wert mit dem Namen "Keyword" (z. b. ~/default.aspx? Keywords = Chef). Sie können einen benutzerdefinierten Schlüssel angeben, indem Sie ihn als Argument an das Parameter Attribut übergeben. Wenn Sie z. b. den Wert der Abfrage Zeichenfolgen-Variablen mit dem Namen q verwenden möchten, können Sie Folgendes tun:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Wenn sich diese Methode im Code der Seite befindet, können Benutzer die Ergebnisse filtern, indem Sie ein Schlüsselwort mithilfe der Abfrage Zeichenfolge übergeben:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Die Modell Bindung führt viele Aufgaben aus, die Sie andernfalls per Hand codieren müssten: das Lesen des Werts, das Überprüfen auf einen NULL-Wert, das Konvertieren des Werts in den entsprechenden Typ, das überprüfen, ob die Konvertierung erfolgreich war, und schließlich das Verwenden des Werts im Such. Die Modell Bindung ergibt weitaus weniger Code und kann die Funktionalität in der gesamten Anwendung wieder verwenden.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtern nach Werten eines Steuer Elements

Angenommen, Sie möchten das Beispiel erweitern, damit der Benutzer einen Filter Wert aus einer Dropdown Liste auswählen lässt. Fügen Sie die folgende Dropdown Liste zum Markup hinzu, und konfigurieren Sie Sie so, dass die Daten von einer anderen Methode mithilfe der *SelectMethod* -Eigenschaft erhalten werden:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

In der Regel würden Sie dem *GridView* -Steuerelement auch ein *EmptyDataTemplate* -Element hinzufügen, damit das Steuerelement eine Meldung anzeigt, wenn keine übereinstimmenden Produkte gefunden werden:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Fügen Sie im Seitencode die neue Select-Methode für die Dropdown Liste hinzu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Aktualisieren Sie abschließend die Methode " *GetProducts* Select", um einen neuen Parameter zu verwenden, der die ID der ausgewählten Kategorie aus der Dropdown Liste enthält:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Wenn die Seite ausgeführt wird, können Benutzer nun eine Kategorie aus der Dropdown Liste auswählen, und das *GridView* -Steuerelement wird automatisch neu gebunden, um die gefilterten Daten anzuzeigen. Dies ist möglich, da die Modell Bindung die Werte von Parametern für SELECT-Methoden nachverfolgt und erkennt, ob ein Parameterwert nach einem Postback geändert wurde. Wenn dies der Fall ist, erzwingt die Modell Bindung das zugeordnete Daten Steuerelement, die Daten erneut zu binden.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML-codierte Daten Bindungs Ausdrücke

Sie können jetzt das Ergebnis von Daten Bindungs Ausdrücken HTML-codieren. Doppelpunkt hinzufügen (:) bis zum Ende des &lt;% #-Präfix, das den Daten Bindungs Ausdruck kennzeichnet:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Unaufdringliche Validierung

Nun können Sie die integrierten Validierungs Steuerelement-Steuerelemente so konfigurieren, dass unaufdringliches JavaScript für die Client seitige Validierungs Logik verwendet wird. Dadurch wird die Menge an JavaScript, die Inline im Seiten Markup gerendert wird, erheblich reduziert, und die Gesamt Seitengröße wird reduziert. Sie können auf eine der folgenden Arten unaufdringliches JavaScript für Validierungs Steuerelemente konfigurieren:

- Global durch Hinzufügen der folgenden Einstellung zum *&lt;appSettings-&gt;* Element in der Datei "Web. config": 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Global durch Festlegen der statischen *System. Web. UI. ValidationSettings. unaufsivevalidationmode* -Eigenschaft auf " *unaufsivevalidationmode. WebForms* " (in der Regel in der *Anwendung\_Start* -Methode in der Datei "Global. asax").
- Einzeln für eine Seite durch Festlegen der neuen Eigenschaft " *unauffällig-evalidationmode* " der Klasse " *Page* " auf " *unaufsivevalidationmode. WebForms*".

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5-Updates

Es wurden einige Verbesserungen an Web Forms Server-Steuerelementen vorgenommen, um die Vorteile der neuen Features von HTML5 zu nutzen:

- Die *TextMode* -Eigenschaft des *TextBox* -Steuer Elements wurde aktualisiert, um die neuen HTML5-Eingabetypen wie z. b. *e-Mail*, *DateTime*usw. zu unterstützen.
- Das *FileUpload* -Steuerelement unterstützt jetzt mehrere Datei Uploads von Browsern, die diese HTML5-Funktion unterstützen.
- Validator-Steuerelemente unterstützen jetzt die Validierung von HTML5-Eingabe Elementen.
- Neue HTML5-Elemente, die über Attribute verfügen, die eine URL darstellen, unterstützen jetzt runat = "Server". Daher können Sie ASP.net-Konventionen in URL-Pfaden verwenden, wie z. b. den ~-Operator, um den Anwendungs Stamm darzustellen (z. b. &lt;Video runat = "Server" src = "~/MyVideo.wmv"/&gt;).
- Das *Update Panel* -Steuerelement wurde korrigiert, um das Veröffentlichen von HTML5-Eingabefeldern zu unterstützen.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta ist nun in Visual Studio 11 Beta enthalten. ASP.NET MVC ist ein Framework für die Entwicklung hochgradig testbarer und verwaltbarer Webanwendungen, indem das Model-View-Controller (MVC)-Muster genutzt wird. ASP.NET MVC 4 vereinfacht das Erstellen von Anwendungen für das Mobile Web und umfasst ASP.net-Web-API, das Sie bei der Erstellung von http-Diensten unterstützt, die jedes Gerät erreichen können. Weitere Informationen finden Sie in den [Anmerkungen zu dieser Version von ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET-Webseiten 2

Zu den neuen Features zählen die folgenden:

- Neue und aktualisierte Website Vorlagen.
- Hinzufügen einer serverseitigen und Client seitigen Validierung mit dem *Validierungs* Hilfsprogramm.
- Die Möglichkeit zum Registrieren von Skripts mit einem Assets-Manager.
- Aktivieren von Anmeldungen von Facebook und anderen Websites mithilfe von OAuth und OpenID.
- Hinzufügen von Zuordnungen mithilfe des *Maps* -Hilfsprogramms.
- Parallele Ausführung von Web Pages-Anwendungen.
- Rendern von Seiten für mobile Geräte.

Weitere Informationen zu diesen Features und Codebeispielen für vollständige Seiten finden Sie [in den Top-Funktionen in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Dieser Abschnitt enthält Informationen zu Verbesserungen für die Webentwicklung in Visual Web Developer 11 Beta und Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projektfreigabe zwischen Visual Studio 2010 und Visual Studio 2012 Release Candidate (Projekt Kompatibilität)

Bis Visual Studio 2012 Release Candidate hat das Öffnen eines vorhandenen Projekts in einer neueren Version von Visual Studio den Konvertierungs-Assistenten gestartet. Dadurch wurde ein Upgrade des Inhalts (Assets) eines Projekts und einer Projekt Mappe auf neue Formate erzwungen, die nicht abwärts kompatibel waren. Daher konnten Sie nach der Konvertierung das Projekt nicht in der älteren Version von Visual Studio öffnen.

Viele Kunden haben uns darüber informiert, dass dies nicht der richtige Ansatz war. In Visual Studio 11 Beta wird nun die Freigabe von Projekten und Projektmappen mit Visual Studio 2010 SP1 unterstützt. Wenn Sie also ein 2010-Projekt in Visual Studio 2012 Release Candidate öffnen, können Sie das Projekt weiterhin in Visual Studio 2010 SP1 öffnen.

> [!NOTE]
> Einige Projekttypen können nicht von Visual Studio 2010 SP1 und Visual Studio 2012 Release Candidate gemeinsam genutzt werden. Hierzu gehören einige ältere Projekte (z. b. ASP.NET MVC 2-Projekte) oder Projekte für besondere Zwecke (z. b. Setup Projekte).

Wenn Sie ein Visual Studio 2010 SP1-Webprojekt zum ersten Mal in Visual Studio 11 Beta öffnen, werden der Projektdatei die folgenden Eigenschaften hinzugefügt:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

Fileupgradeflags, upgradebackuplocation und oldtoolsversion werden von dem Prozess verwendet, der die Projektdatei aktualisiert. Sie haben keine Auswirkung auf das Arbeiten mit dem Projekt in Visual Studio 2010.

Visualstudioversion ist eine neue Eigenschaft, die von MSBuild 4,5 verwendet wird und die Version von Visual Studio für das aktuelle Projekt angibt. Da diese Eigenschaft in MSBuild 4,0 nicht vorhanden war (die Version von MSBuild, die von Visual Studio 2010 SP1 verwendet wird), wird ein Standardwert in die Projektdatei eingefügt.

Die vstoolspath-Eigenschaft wird verwendet, um die richtige targets-Datei zu bestimmen, die aus dem durch die MSBuildExtensionsPath32-Einstellung dargestellten Pfad importiert werden soll.

Es gibt auch einige Änderungen im Zusammenhang mit Import-Elementen. Diese Änderungen sind erforderlich, um die Kompatibilität zwischen beiden Versionen von Visual Studio zu unterstützen.

> [!NOTE]
> Wenn ein Projekt von Visual Studio 2010 SP1 und Visual Studio 11 Beta auf zwei verschiedenen Computern gemeinsam genutzt wird und das Projekt eine lokale Datenbank in den App-\_Datenordner enthält, müssen Sie sicherstellen, dass die von der Datenbank verwendete Version von SQL Server auf beiden Computern installiert ist.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Konfigurationsänderungen in ASP.NET 4,5-Website Vorlagen

Die folgenden Änderungen wurden an der standardmäßigen *Web. config* -Datei für die Website vorgenommen, die mithilfe von Website Vorlagen in Visual Studio 2012 Release Candidate erstellt werden:

- Im `<httpRuntime>`-Element ist das `encoderType`-Attribut jetzt standardmäßig festgelegt, um die antixss-Typen zu verwenden, die ASP.net hinzugefügt wurden. Weitere Informationen finden Sie unter [antixss-Bibliothek](#_Toc318097382).
- Außerdem wird im `<httpRuntime>`-Element das `requestValidationMode`-Attribut auf "4,5" festgelegt. Dies bedeutet, dass die Anforderungs Überprüfung standardmäßig für die Verwendung der verzögerten ("Lazy") Validierung konfiguriert ist. Weitere Informationen finden Sie unter [neue ASP.net Request Validation Features](#_Toc318097379).
- Das `<modules>`-Element des `<system.webServer>` Abschnitts enthält kein `runAllManagedModulesForAllRequests`-Attribut. (Der Standardwert ist false.) Dies bedeutet, dass bei Verwendung einer Version von IIS 7, die noch nicht auf SP1 aktualisiert wurde, Probleme mit dem Routing an einem neuen-Standort auftreten können. Weitere Informationen finden Sie unter systemeigene [Unterstützung in IIS 7 für ASP.NET Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).

Diese Änderungen haben keine Auswirkungen auf vorhandene Anwendungen. Sie können jedoch einen Unterschied im Verhalten zwischen vorhandenen Websites und neuen Websites darstellen, die Sie für ASP.NET 4,5 mithilfe der neuen Vorlagen erstellen.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Native Unterstützung in IIS 7 für ASP.NET Routing

Dies ist keine Änderung an ASP.net, sondern eine Änderung in Vorlagen für neue Website Projekte, die sich auf Sie auswirken können, wenn Sie eine Version von IIS 7 arbeiten, auf die das SP1-Update nicht angewendet wurde.

In ASP.net können Sie der Anwendung die folgende Konfigurationseinstellung hinzufügen, um das Routing zu unterstützen:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Wenn **runAllManagedModulesForAllRequests** den Wert true hat, wird eine URL wie `http://mysite/myapp/home` an ASP.NET weitergeleitet, auch wenn für die URL keine *aspx*-, *MVC*-oder ähnliche Erweiterung vorhanden ist.

Bei einem Update, das an IIS 7 vorgenommen wurde, ist die Einstellung **runAllManagedModulesForAllRequests** unnötig, und das ASP.NET-Routing wird System intern unterstützt. (Informationen zum Update finden Sie im Microsoft-Support Artikel [ein Update verfügbar ist, das bestimmten IIS 7,0-oder IIS 7,5-Handlern ermöglicht, Anforderungen zu verarbeiten, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).)

Wenn Ihre Website unter IIS 7 ausgeführt wird und IIS aktualisiert wurde, ist es nicht erforderlich, **runAllManagedModulesForAllRequests** auf "true" festzulegen. Das Festlegen der Einstellung auf "true" wird nicht empfohlen, da dadurch der Anforderungs Aufwand unnötige Verarbeitungsaufwand hinzugefügt wird. Wenn diese Einstellung auf "true" festgelegt ist, werden alle Anforderungen, einschließlich derjenigen für *htm*-, *JPG*-und andere statische Dateien, auch über die Pipeline "ASP.net Request" durchlaufen.

Wenn Sie eine neue ASP.NET 4,5-Website mit den Vorlagen erstellen, die in Visual Studio 2012 RC bereitgestellt werden, enthält die Konfiguration für die Website keine **runAllManagedModulesForAllRequests** -Einstellung. Dies bedeutet, dass die Einstellung standardmäßig auf false festgelegt ist.

Wenn Sie die Website dann unter Windows 7 ausführen, ohne dass SP1 installiert ist, enthält IIS 7 das erforderliche Update nicht. Das Routing funktioniert daher nicht, und es werden Fehler angezeigt. Wenn Sie ein Problem haben, bei dem das Routing nicht funktioniert, können Sie eine der folgenden Aktionen ausführen:

- Aktualisieren Sie Windows 7 auf SP1, wodurch das Update zu IIS 7 hinzugefügt wird.
- Installieren Sie das Update, das in dem zuvor aufgeführten Microsoft-Support Artikel beschrieben wird.
- Legen Sie **runAllManagedModulesForAllRequests** in der Datei "Web. config" der Website auf "true" fest. Beachten Sie, dass dies zu einem gewissen Verwaltungsaufwand führt.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML-Editor

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Intelligente Aufgaben

In Designansicht sind komplexe Eigenschaften von Server Steuerelementen häufig mit Dialogfeldern und Assistenten verknüpft, damit Sie leicht festgelegt werden können. Beispielsweise können Sie ein spezielles Dialogfeld zum Hinzufügen einer Datenquelle zu einem *Repeater* -Steuerelement oder zum Hinzufügen von Spalten zu einem *GridView* -Steuerelement verwenden.

Diese Art von UI-Hilfe für komplexe Eigenschaften ist jedoch nicht in der Quell Ansicht verfügbar. Aus diesem Grund führt Visual Studio 11 intelligente Aufgaben für die Quell Ansicht ein. Bei intelligenten Aufgaben handelt es sich um kontextabhängige Verknüpfungen für häufig C# verwendete Features im-und-Visual Basic-Editoren.

Für ASP.net-Web Forms-Steuerelemente werden intelligente Aufgaben auf Server Tags als kleines Symbol angezeigt, wenn sich die Einfügemarke innerhalb des Elements befindet:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Der Smarttask wird erweitert, wenn Sie auf das Symbol klicken, oder drücken Sie STRG +. (Punkt), genau wie in den Code-Editoren. Anschließend werden Verknüpfungen angezeigt, die den intelligenten Aufgaben in Designansicht ähneln.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Das Smarttask in der vorherigen Abbildung zeigt z. b. die Optionen für GridView-Aufgaben. Wenn Sie Spalten bearbeiten auswählen, wird das folgende Dialogfeld angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Wenn Sie das Dialogfeld ausfüllen, werden die Eigenschaften festgelegt, die Sie in Designansicht festlegen können. Wenn Sie auf OK klicken, wird das Markup für das-Steuerelement mit den neuen Einstellungen aktualisiert:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Unterstützung von WAI-ARIA

Das Schreiben von zugänglichen Websites wird immer wichtiger. Der " [WAI-ARIA"-Barrierefreiheits Standard](http://www.w3.org/WAI/intro/aria) definiert, wie Entwickler barrierefreie Websites schreiben sollten. Dieser Standard wird jetzt vollständig in Visual Studio unterstützt.

Beispielsweise verfügt das *Role* -Attribut jetzt über vollständiges IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Der WAI-ARIA-Standard führt außerdem Attribute mit dem Präfix " *Aria* " ein, mit denen Sie Semantik zu einem HTML5-Dokument hinzufügen können. Visual Studio unterstützt auch die folgenden *Aria-* Attribute vollständig:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Neue HTML5-Ausschnitte

Um das Schreiben häufig verwendeter HTML5-Markup zu beschleunigen und zu vereinfachen, enthält Visual Studio eine Reihe von Code Ausschnitten. Ein Beispiel hierfür ist der Videoausschnitt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Um den Code Ausschnitt aufzurufen, drücken Sie zweimal die Tab-Taste, wenn das Element in IntelliSense ausgewählt wird:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Dies erzeugt einen Code Ausschnitt, den Sie anpassen können.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>In Benutzer Steuerelement extrahieren

In großen Webseiten kann es ratsam sein, einzelne Teile in Benutzer Steuerelemente zu verschieben. Diese Art der Umgestaltung kann dazu beitragen, die Lesbarkeit der Seite zu erhöhen und die Seitenstruktur zu vereinfachen.

Um dies zu vereinfachen, können Sie, wenn Sie Web Forms Seiten in der Quell Ansicht bearbeiten, nun Text auf einer Seite auswählen, mit der rechten Maustaste darauf klicken und dann zu Benutzer Steuerelement extrahieren auswählen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense für Code-Nuggets in Attributen

Visual Studio hat immer IntelliSense für serverseitige Code-Nuggets in einer beliebigen Seite oder einem beliebigen Steuerelement bereitgestellt. Visual Studio enthält jetzt auch IntelliSense für Code-Nuggets in HTML-Attributen.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Dadurch wird das Erstellen von Daten Bindungs Ausdrücken vereinfacht:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatisches Umbenennen des passenden Tags beim Umbenennen eines öffnenden oder Endtags

Wenn Sie ein HTML-Element umbenennen (z. b. Ändern eines *div* -Tags in ein *Header* -Tag), ändert sich auch das entsprechende öffnende oder schließende Tag in Echtzeit.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Dies hilft, den Fehler zu vermeiden, wenn Sie vergessen, ein Schließ Endes Tag zu ändern oder das falsche zu ändern.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generierung von Ereignis Handlern

Visual Studio enthält nun Funktionen in der Quell Ansicht, die Sie beim Schreiben von Ereignis Handlern und beim manuellen Binden der Ereignishandler unterstützen. Wenn Sie einen Ereignis Namen in der Quell Ansicht bearbeiten, zeigt IntelliSense &lt;Create New Event&gt;an. Dadurch wird ein Ereignishandler im Code der Seite erstellt, der über die richtige Signatur verfügt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Standardmäßig verwendet der Ereignishandler die ID des Steuer Elements für den Namen der Ereignis Behandlungsmethode:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Der resultierende Ereignishandler sieht wie folgt aus (in diesem Fall in C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Intelligenter Einzug

Wenn Sie die EINGABETASTE in einem leeren HTML-Element drücken, wird die Einfügemarke im Editor an der rechten Stelle abgelegt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Wenn Sie an dieser Stelle die EINGABETASTE drücken, wird das Endtag nach unten und eingerückt, um das öffnende Tag abzugleichen. Die Einfügemarke wird ebenfalls eingezogen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Automatische Reduzierung der Anweisungs Vervollständigung

Die IntelliSense-Liste in Visual Studio filtert nun basierend auf der Art der Art und Weise, dass nur relevante Optionen angezeigt werden:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense filtert auch basierend auf dem Titel Fall der einzelnen Wörter in der IntelliSense-Liste. Wenn Sie z. b. "DL" eingeben, werden sowohl DL als auch ASP: DataList angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Diese Funktion ermöglicht es, die Anweisungs Vervollständigung für bekannte Elemente schneller zu erhalten.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript-Editor

Der JavaScript-Editor in Visual Studio 2012 Release Candidate ist vollständig neu und verbessert die Erfahrung bei der Arbeit mit JavaScript in Visual Studio erheblich.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Code outlining

Gliederungs Bereiche werden nun automatisch für alle Funktionen erstellt, sodass Sie Teile der Datei reduzieren können, die für den aktuellen Fokus nicht relevant sind.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Klammernabgleich (zugehörige Klammer)

Wenn Sie die Einfügemarke in eine öffnende oder schließende geschweifte Klammer einfügen, hebt der Editor die entsprechende hervor.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Gehe zu Definition

Mit dem Befehl Gehe zu Definition können Sie zur Quelle für eine Funktion oder Variable springen.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5-Unterstützung

Der Editor unterstützt die neue Syntax und APIs in ECMAScript5, die neueste Version des Standards, die die JavaScript-Sprache beschreibt.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>Dom-IntelliSense

IntelliSense für DOM-APIs wurde verbessert und bietet Unterstützung für viele neue HTML5-APIs, einschließlich *queryselector*, Dom-Speicher, Dokument übergreifendes Messaging und *Canvas*. Dom IntelliSense wird jetzt von einer einzelnen einfachen JavaScript-Datei und nicht von einer systemeigenen Typbibliotheks Definition gesteuert. Dies erleichtert das Erweitern oder ersetzen.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Vsdoc-Signatur Überladungen

Ausführliche IntelliSense-Kommentare können jetzt für separate über Ladungen von JavaScript-Funktionen mit dem neuen *&lt;Signature&gt;* -Element deklariert werden, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Implizite Verweise

Sie können jetzt JavaScript-Dateien zu einer zentralen Liste hinzufügen, die implizit in der Liste der Dateien enthalten ist, auf die eine beliebige JavaScript-Datei oder einen Block verweist. Dies bedeutet, dass Sie IntelliSense für Ihren Inhalt erhalten. Beispielsweise können Sie jQuery-Dateien der zentralen Liste von Dateien hinzufügen, und Sie erhalten IntelliSense für jQuery-Funktionen in jedem JavaScript-Dateiblock, unabhängig davon, ob Sie explizit referenziert wurden (mit///&lt;Verweis/&gt;).

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS-Editor

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Automatische Reduzierung der Anweisungs Vervollständigung

Die IntelliSense-Liste für CSS filtert nun basierend auf den CSS-Eigenschaften und-Werten, die vom ausgewählten Schema unterstützt werden.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense unterstützt auch die Suche nach Titel Fällen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hierarchischer Einzug

Der CSS-Editor verwendet den Einzug, um hierarchische Regeln anzuzeigen. Dadurch erhalten Sie einen Überblick darüber, wie die kaskadierenden Regeln logisch organisiert werden. Im folgenden Beispiel ist die #List eine Auswahl ein untergeordnetes Element von List, das daher eingezogen ist.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Das folgende Beispiel zeigt eine komplexere Vererbung:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Der Einzug einer Regel wird durch die übergeordneten Regeln bestimmt. Hierarchischer Einzug ist standardmäßig aktiviert, aber Sie können das Dialogfeld "Optionen" (Extras, Optionen in der Menüleiste) deaktivieren:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS-Unterstützung

Die Analyse von Hunderten von realen CSS-Dateien zeigt, dass CSS-Hacks sehr häufig vorkommen, und Visual Studio unterstützt jetzt die am häufigsten verwendeten. Diese Unterstützung umfasst IntelliSense und die Validierung des Stern (\*) und der Unterstriche (\_)-Eigenschafts-Hacks:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Typische Auswahl-Hacks werden ebenfalls unterstützt, sodass der hierarchische Einzug auch dann beibehalten wird, wenn Sie angewendet werden. Ein typischer Auswahl-Hack, der für Internet Explorer 7 verwendet wird, besteht darin, einen Selektor mit\*vorangestellt *: First-Child + HTML*. Wenn Sie diese Regel verwenden, wird der hierarchische Einzug beibehalten:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Anbieterspezifische Schemas (--,-WebKit)

CSS3 führt viele Eigenschaften ein, die von verschiedenen Browsern zu unterschiedlichen Zeiten implementiert wurden. Dadurch mussten Entwickler mit Hersteller spezifischer Syntax für bestimmte Browser programmieren. Diese browserspezifischen Eigenschaften sind jetzt in IntelliSense enthalten.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Unterstützung für Kommentare und unkommentare

Mit den gleichen Tastenkombinationen, die Sie im Code-Editor verwenden (STRG + k, C zum kommentieren und STRG + k, Sie können die Auskommentierung aufheben), können Sie nun CSS-Regeln kommentieren und entfernen.

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Farbauswahl

In früheren Versionen von Visual Studio bestand IntelliSense für Farb bezogene Attribute aus einer Dropdown Liste benannter Farbwerte. Diese Liste wurde durch eine Farbauswahl mit vollem Funktionsumfang ersetzt.

Wenn Sie einen Farbwert eingeben, wird die Farbauswahl automatisch angezeigt, und es wird eine Liste der zuvor verwendeten Farben angezeigt, gefolgt von einer Standardfarbpalette. Sie können eine Farbe mit der Maus oder der Tastatur auswählen.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Die Liste kann in eine komplette Farbauswahl erweitert werden. Mithilfe der Auswahl können Sie den Alphakanal steuern, indem Sie beim Verschieben des Schiebereglers die Farbe automatisch in RGBA umrechnen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Codeausschnitte

Mit Code Ausschnitten im CSS-Editor können Browser übergreifende Stile einfacher und schneller erstellt werden. Viele CSS3-Eigenschaften, für die browserspezifische Einstellungen erforderlich sind, wurden nun in Code Ausschnitte eingeführt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS-Ausschnitte unterstützen erweiterte Szenarien (z. b. CSS3-Medien Abfragen), indem Sie das at-Symbol (@) eingeben, das die IntelliSense-Liste anzeigt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Wenn Sie @media Wert auswählen und die Tab-Taste drücken, fügt der CSS-Editor den folgenden Code Ausschnitt ein:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Wie bei Code Ausschnitten können Sie eigene CSS-Ausschnitte erstellen.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Benutzerdefinierte Regionen

Benannte Code Bereiche, die bereits im Code-Editor verfügbar sind, sind jetzt für die CSS-Bearbeitung verfügbar. Auf diese Weise können Sie verwandte Stil Blöcke problemlos gruppieren.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Wenn ein Bereich reduziert wird, wird der Name der Region angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Seitenprüfung

Seitenprüfung ist ein Tool, das eine Webseite (HTML, Web Forms, ASP.NET MVC oder Webseiten) in der Visual Studio-IDE rendert und Ihnen ermöglicht, den Quellcode und die resultierende Ausgabe zu untersuchen. Für ASP.NET-Seiten können Sie mit Seitenprüfung bestimmen, welcher serverseitiger Code das HTML-Markup erzeugt hat, das im Browser gerendert wird.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Weitere Informationen zu Seitenprüfung finden Sie in den folgenden Tutorials:

- Verwenden von Seitenprüfung in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Verwenden von Seitenprüfung in [ASP.net Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Veröffentlichen

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Veröffentlichungsprofile

In Visual Studio 2010 werden Veröffentlichungsinformationen für Webanwendungs Projekte nicht in der Versionskontrolle gespeichert und sind nicht für die Freigabe mit anderen konzipiert. In Visual Studio 2012 Release Candidate wurde das Format des Veröffentlichungs Profils geändert. Es wurde zu einem Team Element gemacht und kann nun problemlos von Builds basierend auf MSBuild genutzt werden. Buildkonfigurationsinformationen befinden sich im Dialogfeld veröffentlichen, sodass Sie Buildkonfigurationen vor dem Veröffentlichen problemlos wechseln können.

Veröffentlichungs Profile werden im Ordner publishprofiles gespeichert. Der Speicherort des Ordners hängt davon ab, welche Programmiersprache Sie verwenden:

- C#: Properties\publishprofiles
- Visual Basic: "My project\publishprofiles"

Jedes Profil ist eine MSBuild-Datei. Während der Veröffentlichung wird diese Datei in die MSBuild-Datei des Projekts importiert. Wenn Sie in Visual Studio 2010 Änderungen am Veröffentlichungs-oder Paket Prozess vornehmen möchten, müssen Sie Ihre Anpassungen in einer Datei namens **ProjectName**. WPP. targets ablegen. Dies wird weiterhin unterstützt, aber Sie können Ihre Anpassungen jetzt im Veröffentlichungs Profil selbst vornehmen. Auf diese Weise werden die Anpassungen nur für dieses Profil verwendet.

Sie können jetzt auch Veröffentlichungs Profile von MSBuild nutzen. Verwenden Sie hierzu den folgenden Befehl, wenn Sie das Projekt erstellen:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Der "Project. csproj"-Wert ist der Pfad des Projekts, und "Profile Name" ist der Name des Profils, das veröffentlicht werden soll. Anstatt den Profilnamen für die *publishprofile* -Eigenschaft zu übergeben, können Sie alternativ den vollständigen Pfad zum Veröffentlichungs Profil übergeben.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET Vorkompilierung und Zusammenführen

Für Webanwendungs Projekte fügt Visual Studio 2012 Release Candidate eine Option auf der Seite Eigenschaften für das Paket/veröffentlichen hinzu, mit der Sie den Inhalt Ihrer Website bei der Veröffentlichung oder dem Packen des Projekts vorkompilieren und zusammenführen können. Um diese Optionen anzuzeigen, klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie Eigenschaften aus, und wählen Sie dann die Eigenschaften Seite für das Paket/veröffentlichen aus. In der folgenden Abbildung wird die Option Diese Anwendung vor dem Veröffentlichen Vorkompilieren angezeigt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Wenn diese Option ausgewählt ist, kompiliert Visual Studio die Anwendung immer dann, wenn Sie die Webanwendung veröffentlichen oder packen. Wenn Sie steuern möchten, wie der Standort vorkompiliert wird oder Assemblys zusammengeführt werden, klicken Sie auf die Schaltfläche Erweitert, um diese Optionen zu konfigurieren.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Der Standardweb Server zum Testen von Webprojekten in Visual Studio ist jetzt IIS Express. Der Visual Studio Development Server ist während der Entwicklung weiterhin eine Option für den lokalen Webserver, aber IIS Express ist nun der empfohlene Server. Die Verwendung von IIS Express in Visual Studio 11 Beta ähnelt der Verwendung in Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor der kommerziellen Veröffentlichung der beschriebenen Software ggf. erheblich geändert wird.

Die in diesem Dokument enthaltenen Informationen stellen die Sicht der Microsoft Corporation der hier diskutierten Themen zum Zeitpunkt der Veröffentlichung dar. Da Microsoft auf wechselnde Marktbedingungen reagieren muss, sollten sie nicht als Verpflichtung seitens Microsoft interpretiert werden, und Microsoft kann die Genauigkeit der dargelegten Informationen nach dem Zeitpunkt der Veröffentlichung nicht garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf kein Teil dieses Dokuments ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, sind die hier dargestellten Beispiel Unternehmen, Organisationen, Produkte, Domänen Namen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse fiktiv und keine Zuordnung zu echten Unternehmen, Organisationen, Produkten, Domänen Namen, e-Mail-Adressen. Adresse, Logo, Person, Ort oder Ereignis ist beabsichtigt oder sollte abgeleitet werden.

© 2012 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
