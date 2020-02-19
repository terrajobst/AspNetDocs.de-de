---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Bewährte Methoden für die Webentwicklung (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457088"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Bewährte Methoden für die Webentwicklung (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Bei den ersten drei Mustern ging es um das Einrichten eines Agile-Entwicklungsprozesses. der Rest geht über die Architektur und den Code. Dabei handelt es sich um eine Sammlung von bewährten Methoden für die Webentwicklung:

- Zustands [lose Webserver](#stateless) hinter einem intelligenten Load Balancer.
- [Vermeiden Sie den Sitzungszustand](#sessionstate) (oder wenn Sie ihn nicht vermeiden können, verwenden Sie den verteilten Cache anstelle einer Datenbank).
- [Verwenden Sie ein CDN](#cdn) , um statische Datei Ressourcen (Bilder, Skripts) per Edge zwischenzuspeichern.
- [Verwenden Sie asynchrone Unterstützung von .NET 4.5](#async) , um blockierende Aufrufe zu vermeiden.

Diese Vorgehensweisen sind für die Webentwicklung und nicht nur für Cloud-apps gültig, Sie sind jedoch besonders wichtig für Cloud-apps. Sie arbeiten zusammen, um Sie bei der optimalen Nutzung der hochgradig flexiblen Skalierung zu unterstützen, die von der cloudumgebung geboten wird. Wenn Sie diese Vorgehensweisen nicht befolgen, treten beim Versuch, Ihre Anwendung zu skalieren, Beschränkungen auf.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Zustands lose webeebene hinter einem intelligenten Load Balancer

Zustands *lose webetier* bedeutet, dass Sie keine Anwendungsdaten im Webserver Speicher oder im Dateisystem speichern. Wenn Sie Ihre webeebene zustandslos halten, können Sie eine bessere Kundenfreundlichkeit bereitstellen und Geld sparen:

- Wenn die webeebene zustandslos ist und sich hinter einem Load Balancer befindet, können Sie schnell auf Änderungen im Anwendungs Datenverkehr reagieren, indem Sie Server dynamisch hinzufügen oder entfernen. In der cloudumgebung, in der Sie nur für Server Ressourcen bezahlen, solange Sie Sie tatsächlich verwenden, kann die Möglichkeit, auf Bedarfs gesteuerte Änderungen zu reagieren, zu enormen Einsparungen führen.
- Eine Zustands lose webeebene ist architektonisch sehr viel einfacher, um die Anwendung horizontal hochzuskalieren. Außerdem können Sie schneller auf Skalierungs Anforderungen reagieren und weniger Geld für die Entwicklung und das Testen im Prozess aufwenden.
- Cloudserver, wie lokale Server, müssen gelegentlich gepatcht und neu gestartet werden. Wenn die webeebene zustandslos ist, führt das erneute Routing von Datenverkehr, wenn ein Server vorübergehend ausfällt, nicht zu Fehlern oder unerwartetem Verhalten.

Die meisten realen Anwendungen müssen den Zustand für eine Websitzung speichern. der wichtigste Punkt darin besteht darin, ihn nicht auf dem Webserver zu speichern. Sie können den Zustand auf andere Weise speichern, z. b. auf dem Client in Cookies oder außerhalb des Prozesses, der serverseitig in ASP.NET Session State mithilfe eines Cache Anbieters. Sie können Dateien im [Windows Azure-BLOB-Speicher](unstructured-blob-storage.md) anstelle des lokalen Dateisystems speichern.

Ein Beispiel dafür, wie einfach es ist, eine Anwendung in Windows Azure-Websites zu skalieren, wenn Ihre webeebene zustandslos ist, finden Sie auf der Registerkarte **skalieren** für eine Windows Azure-Website im Verwaltungs Portal:

![Registerkarte "Skalieren"](web-development-best-practices/_static/image1.png)

Wenn Sie Webserver hinzufügen möchten, können Sie einfach den Schieberegler für die Instanzen Anzahl nach rechts ziehen. Legen Sie den Wert auf 5 fest, und klicken Sie auf **Speichern**. innerhalb von Sekunden verfügen Sie über fünf Webserver in Windows Azure, die den Datenverkehr Ihrer Website verarbeiten.

![Fünf Instanzen](web-development-best-practices/_static/image2.png)

Sie können den instanzzähler genauso einfach auf den Wert 3 oder wieder auf den Wert 1 festlegen. Beim horizontalen Herunterskalieren beginnen Sie sofort damit, Geld zu sparen, da Windows Azure pro Minute und nicht nach der Stunde abgerechnet wird.

Sie können Windows Azure auch mitteilen, dass die Anzahl von Webservern basierend auf der CPU-Auslastung automatisch erhöht oder verringert werden soll. Im folgenden Beispiel wird die Anzahl von Webservern auf mindestens 2 reduziert, wenn die CPU-Auslastung unter 60% sinkt. wenn die CPU-Auslastung über 80% liegt, wird die Anzahl der Webserver auf maximal 4 heraufgesetzt.

![Skalieren nach CPU-Auslastung](web-development-best-practices/_static/image3.png)

Oder was ist, wenn Sie wissen, dass Ihre Website nur während der Arbeitszeit ausgelastet ist? Sie können Windows Azure anweisen, während des Tages mehrere Server auszuführen und auf einen einzelnen Server, nachts und am Wochenende zu verkürzen. In der folgenden Bildschirm Abbildung wird gezeigt, wie Sie die Website so einrichten, dass Sie einen Server innerhalb von Stunden und 4 Server während der Arbeitszeit zwischen 8 und 5 Uhr ausgeführt wird.

![Nach Zeitplan skalieren](web-development-best-practices/_static/image4.png)

![Zeit Plan Zeiten festlegen](web-development-best-practices/_static/image5.png)

![Tagzeitplan](web-development-best-practices/_static/image6.png)

![Wochentag](web-development-best-practices/_static/image7.png)

![Zeitplan für Wochenende](web-development-best-practices/_static/image8.png)

Natürlich können all dies auch in Skripts und im Portal erfolgen.

Die Fähigkeit Ihrer Anwendung, horizontal hochskalieren, ist fast unbegrenzt in Windows Azure, solange Sie keine Hindernisse für das dynamische Hinzufügen oder Entfernen von Server-VMS vermeiden, indem Sie die webeebene zustandslos halten.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Vermeiden des Sitzungs Zustands

Im praktischen Einsatz von Cloud-Apps lässt sich das Speichern von Zustandsinformationen für eine Benutzersitzung oft nicht vermeiden. Allerdings beeinträchtigen einige Herangehensweisen die Leistung und Skalierbarkeit stärker als andere. Wenn Sie einen Sitzungszustand speichern müssen, ist es am besten, die Menge der Zustandsinformationen niedrig zu halten und sie in Cookies zu speichern. Wenn dies nicht möglich ist, besteht die nächstbeste Lösung darin, den ASP.NET-Sitzungszustand mit einem Anbieter für [verteilten, in-Memory-Cache](distributed-caching.md#sessionstate)zu verwenden. Die schlechteste Lösung im Hinblick auf Leistung und Skalierbarkeit wäre es, einen datenbankbasierten Sitzungszustandsanbieter zu verwenden.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Verwenden eines CDN zum Zwischenspeichern statischer Datei Ressourcen

CDN ist ein Akronym für Content Delivery Network. Sie stellen statische Datei Ressourcen wie Bilder und Skriptdateien für einen CDN-Anbieter bereit, und der Anbieter speichert diese Dateien in Daten Centern auf der ganzen Welt zwischen, sodass der Benutzer, der auf Ihre Anwendung zugreift, relativ schnelle Antworten und geringe Latenzzeit für den zwischengespeicherten Werten. Dadurch wird die Gesamt Ladezeit der Site beschleunigt und die Last auf ihren Webservern reduziert. CDNs sind besonders wichtig, wenn Sie eine Zielgruppe erreichen, die geografisch weit verbreitet ist.

Windows Azure verfügt über ein CDN, und Sie können andere CDNs in einer Anwendung verwenden, die in Windows Azure oder in einer beliebigen Webhostingumgebung ausgeführt wird.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Verwenden der asynchronen Unterstützung von .NET 4.5 zum Vermeiden von blockierenden aufrufen

.NET 4,5 hat die C# Programmiersprachen und VB erweitert, um das asynchrone verarbeiten von Aufgaben zu vereinfachen. Der Vorteil der asynchronen Programmierung ist nicht nur für parallele Verarbeitungs Situationen, z. b. Wenn Sie mehrere Webdienst Aufrufe gleichzeitig starten möchten. Außerdem ermöglicht es Ihrem Webserver, effizienter und zuverlässiger unter hohen Ladebedingungen auszuführen. Auf einem Webserver ist nur eine begrenzte Anzahl von Threads verfügbar, und bei hohen Ladezeiten, wenn alle Threads verwendet werden, müssen eingehende Anforderungen warten, bis Threads freigegeben werden. Wenn Ihr Anwendungscode keine Aufgaben wie Datenbankabfragen und Webdienst Aufrufe asynchron verarbeitet, werden viele Threads unnötig gebunden, während der Server auf eine e/a-Antwort wartet. Dadurch wird die Menge des Datenverkehrs eingeschränkt, den der Server unter hohen Ladebedingungen verarbeiten kann. Bei der asynchronen Programmierung werden Threads, die auf die Rückgabe von Daten durch einen Webdienst oder eine Datenbank warten, bis zum Dienst neuer Anforderungen freigegeben, bis die Daten empfangen werden. Bei einem ausgelasteten Webserver können Hunderte oder Tausende von Anforderungen umgehend verarbeitet werden, die andernfalls darauf warten, dass Threads freigegeben werden.

Wie Sie bereits gesehen haben, ist es so einfach, die Anzahl von Webservern zu verringern, die Ihre Website verarbeiten. Wenn ein Server also einen höheren Durchsatz erzielen kann, benötigen Sie nicht so viele, und Sie können Ihre Kosten senken, da Sie für ein bestimmtes Verkehrsaufkommen weniger Server benötigen, als andernfalls.

Die Unterstützung für das asynchrone Programmiermodell von .NET 4,5 ist in ASP.NET 4,5 für Web Forms, MVC und die Web-API enthalten. in Entity Framework 6 und in der [Windows Azure Storage-API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Async-Unterstützung in ASP.NET 4,5

In ASP.NET 4,5 wurde die Unterstützung für asynchrone Programmierung nicht nur der Sprache hinzugefügt, sondern auch den MVC-, Web Forms-und Web-API-Frameworks. Eine ASP.NET MVC-Controller Aktionsmethode empfängt z. b. Daten von einer Webanforderung und übergibt die Daten an eine Ansicht, die dann den HTML-Code erstellt, der an den Browser gesendet wird. Häufig muss die Aktionsmethode Daten aus einer Datenbank oder einem Webdienst aufrufen, um Sie auf einer Webseite anzuzeigen oder die Daten zu speichern, die auf einer Webseite eingegeben werden. In diesen Szenarien ist es ganz einfach, die Aktionsmethode asynchron zu machen: statt ein *Action result* -Objekt zurückzugeben, geben Sie *Task&lt;"action result* "-&gt;und markieren die Methode mit dem " *Async* "-Schlüsselwort. Wenn in der-Methode eine Codezeile einen Vorgang startet, der die Wartezeit einschließt, markieren Sie Sie mit dem Wait-Schlüsselwort.

Im folgenden finden Sie eine einfache Aktionsmethode, die eine Repository-Methode für eine Datenbankabfrage aufruft:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Und hier ist dieselbe Methode, die den Daten Bank Aufrufe asynchron behandelt:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Im unter Abschnitt generiert der Compiler den entsprechenden asynchronen Code. Wenn die Anwendung `FindTaskByIdAsync`aufruft, führt ASP.net die `FindTask` Anforderung aus und entlädt dann den Arbeits Thread und stellt ihn zur Verfügung, um eine andere Anforderung zu verarbeiten. Wenn die `FindTask` Anforderung ausgeführt wird, wird ein Thread neu gestartet, um die Verarbeitung des Codes fortzusetzen, der nach diesem-Aufrufvorgang erfolgt. Während der Zwischenzeit zwischen dem Zeitpunkt, zu dem die `FindTask` Anforderung initiiert wird, und dem Zeitpunkt der Rückgabe der Daten steht ein Thread zur Verfügung, um nützliche Arbeiten durchzuführen, die andernfalls an die Antwort gesendet werden.

Es gibt zwar einen gewissen Aufwand für asynchronen Code, aber unter Bedingungen mit geringem Lade Aufwand ist dieser mehr Aufwand unerheblich, während Sie unter hoch Ladebedingungen Anforderungen verarbeiten können, die andernfalls auf verfügbare Threads warten.

Diese Art der asynchronen Programmierung war seit ASP.NET 1,1 möglich, aber es war schwierig, zu schreiben, fehleranfällig und schwer zu debuggen. Nun, da wir die Codierung für die IT in ASP.NET 4,5 vereinfacht haben, gibt es keinen Grund dafür, dies nicht mehr zu tun.

### <a name="async-support-in-entity-framework-6"></a>Async-Unterstützung in Entity Framework 6

Als Teil der async-Unterstützung in 4,5 haben wir die asynchrone Unterstützung für Webdienst Aufrufe, Sockets und Dateisystem-e/a-Vorgänge ausgeliefert, aber das gängigste Muster für Webanwendungen besteht darin, eine Datenbank zu erreichen, und unsere Daten Bibliotheken haben Async nicht unterstützt. Nun fügt Entity Framework 6 asynchrone Unterstützung für den Datenbankzugriff hinzu.

In Entity Framework 6 verfügen alle Methoden, die bewirken, dass eine Abfrage oder ein Befehl an die Datenbank gesendet wird, über asynchrone Versionen. Das Beispiel zeigt die asynchrone Version der *Find* -Methode.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Diese async-Unterstützung funktioniert nicht nur für Einfügungen, Löschungen, Aktualisierungen und einfache Suchvorgänge, sondern auch für LINQ-Abfragen:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Es gibt eine `Async` Version der `ToList`-Methode, da in diesem Code die Methode ist, die bewirkt, dass eine Abfrage an die Datenbank gesendet wird. Die Methoden `Where` und `OrderByDescending` konfigurieren nur die Abfrage, während die `ToListAsync`-Methode die Abfrage ausführt und die Antwort in der `result` Variablen speichert.

## <a name="summary"></a>Zusammenfassung

Sie können die hier beschriebenen bewährten Methoden für die Webentwicklung in allen webprogrammier-und cloudumgebungen implementieren, aber wir haben Tools in ASP.net und Windows Azure, um dies zu vereinfachen. Wenn Sie diesen Mustern folgen, können Sie Ihre webeebene problemlos skalieren, und Sie können Ihre Ausgaben minimieren, da jeder Server mehr Datenverkehr verarbeiten kann.

Im [nächsten Kapitel](single-sign-on.md) wird erläutert, wie die Cloud Single Sign-on Szenarien ermöglicht.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Zustands lose Webserver:

- [Microsoft Patterns and Practices: Leitfaden für die automatische Skalierung](https://msdn.microsoft.com/library/dn589774.aspx).
- [Deaktivieren der arr-instanzaffinität in Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)-Websites. Der Blog Beitrag von Erez Benari erläutert die Sitzungs Affinität in Windows Azure-Websites.

CDN:

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Videoreihe von Ulrich Homann, Marc Mercuri und Mark Simms. Weitere Informationen finden Sie in der CDN-Diskussion in Episode 3 ab 1:34:00.
- [Microsoft Patterns and Practices-hostingmuster für statisches Content](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN-Reviews](http://www.cdnreviews.com/). Übersicht über viele CDNs.

Asynchrone Programmierung:

- [Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial von Rick Anderson.
- [Asynchrone Programmierung mit Async und wartet (C# und Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN-Whitepaper, in dem die Begründung für asynchrone Programmierung, die Funktionsweise in ASP.NET 4,5 und das Schreiben von Code für die Implementierung erläutert werden.
- [Asynchrone Abfrage Entity Framework und speichern](https://msdn.microsoft.com/data/jj819165)
- [Erstellen von ASP.NET-Webanwendungen mithilfe von Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Video Präsentation von Rowan Miller. Enthält eine Grafik, die veranschaulicht, wie die asynchrone Programmierung bei hohen Lade Vorbedingungen dramatische Steigerungen des Webserver Durchsatzes vereinfachen kann.
- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Videoreihe von Ulrich Homann, Marc Mercuri und Mark Simms. Diskussionen zu den Auswirkungen der asynchronen Programmierung auf die Skalierbarkeit finden Sie in der Episode 4 und Episode 8.
- [Die Magie der Verwendung von asynchronen Methoden in ASP.NET 4,5 plus einem wichtigen Problem](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blog Beitrag von Scott Hanselman, hauptsächlich zur Verwendung von Async in ASP.net Web Forms-Anwendungen.

Weitere bewährte Methoden für die Webentwicklung finden Sie in den folgenden Ressourcen:

- [Die Lösung zum Beheben von IT-Beispielen: bewährte Methoden](the-fix-it-sample-application.md#bestpractices). Im Anhang zu diesem e-Book finden Sie eine Reihe bewährter Methoden, die in der Lösung zum Beheben von IT-Anwendungen implementiert wurden.
- [Webentwickler Checkliste](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Zurück](continuous-integration-and-continuous-delivery.md)
> [Weiter](single-sign-on.md)
