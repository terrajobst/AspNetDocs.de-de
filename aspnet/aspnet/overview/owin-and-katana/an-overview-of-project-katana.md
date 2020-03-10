---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Übersicht über Project Katana | Microsoft-Dokumentation
author: howarddierking
description: Das ASP.NET-Framework ist seit mehr als zehn Jahren verfügbar, und die Plattform hat die Entwicklung von zahllosen Websites und Diensten ermöglicht. Als Webanwendung...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 1f28db822930cdfd2ebf4cf9bb27d173f4aa4201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500343"
---
# <a name="an-overview-of-project-katana"></a>Übersicht über das Katana-Projekt

von [Howard Dierking](https://github.com/howarddierking)

> Das ASP.NET-Framework ist seit mehr als zehn Jahren verfügbar, und die Plattform hat die Entwicklung von zahllosen Websites und Diensten ermöglicht. Da sich Webanwendungs-Entwicklungsstrategien entwickelt haben, ist das Framework in der Lage, sich mit Technologien wie ASP.NET MVC und ASP.net-Web-API zu entwickeln. Da die Entwicklung von Webanwendungen den nächsten evolutionären Schritt in die Welt der Cloud Computing übernimmt, stellt Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) den zugrunde liegenden Satz von Komponenten für ASP.NET-Anwendungen bereit, sodass Sie flexibel, portabel und einfach zu gestalten sind. – können Sie auf andere Weise Ihre ASP.NET-Anwendungen mit Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) Cloud optimieren.

## <a name="why-katana--why-now"></a>Warum Katana – warum jetzt?

 Unabhängig davon, ob ein Entwickler-oder Endbenutzer Produkt diskutiert wird, ist es wichtig, sich mit den zugrunde liegenden Beweggründen für die Erstellung des Produkts – vertraut zu machen. Außerdem ist es wichtig, dass Sie wissen, wofür das Produkt erstellt wurde. ASP.net wurde ursprünglich mit zwei Kunden erstellt.   
  
**Die erste Kundengruppe waren klassische ASP-Entwickler.** Zu diesem Zeitpunkt war ASP eine der primären Technologien zum Erstellen dynamischer, datengesteuerte Websites und Anwendungen durch das Zusammenwirken von Markup und Server seitigem Skript. Die ASP-Laufzeit hat serverseitiges Skript mit einer Reihe von Objekten bereitgestellt, die die Kernaspekte des zugrunde liegenden http-Protokolls und des Webservers abstrahiert haben und Zugriff auf zusätzliche Dienste wie Sitzungs-und Anwendungs Zustands Verwaltung, Cache usw. bieten. Leistungsstarke, klassische ASP-Anwendungen wurden bei der Bewältigung der Größe und Komplexität zu einer Herausforderung. Dies war größtenteils auf die fehlende Struktur in Skript Umgebungen zurückzuführen, die mit der Duplizierung von Code zusammenhängen, der sich aus dem Überschreiben von Code und Markup ergibt. Um die Stärken von klassischem ASP bei der Bewältigung einiger Herausforderungen zu nutzen, hat ASP.net die von den objektorientierten Sprachen der .NET Framework bereitgestellte Code Organisation genutzt, während gleichzeitig auch das serverseitige Programmiermodell beibehalten wird. an welche klassischen ASP-Entwickler gewöhnt waren.

**Die zweite Gruppe von Zielkunden für ASP.net waren Entwickler von Windows-Geschäftsanwendungen.** Im Gegensatz zu klassischen ASP-Entwicklern, die an das Schreiben von HTML-Markup und den Code zum Generieren von mehr HTML-Markup gewöhnt waren, waren WinForms-Entwickler (wie die von Ihnen vorgenommenen VB6-Entwickler) an eine Entwurfszeit Oberfläche gewöhnt, die eine Canvas und eine Vielzahl von Benutzern enthielt. Schnittstellen Steuerelemente. Die erste Version von ASP.net – wird auch als "Web Forms" bezeichnet und bietet eine ähnliche Entwurfszeit Oberfläche sowie ein serverseitiges Ereignis Modell für Benutzeroberflächen Komponenten und einen Satz von Infrastruktur Features (z. b. ViewState), um eine nahtlose Entwickler Oberfläche zu erstellen. zwischen Client-und serverseitiger Programmierung. Web Forms die Zustands lose Struktur des webzustands unter einem Zustands behafteten Ereignis Modell, das WinForms-Entwickler vertraut war, praktisch verborgen.

### <a name="challenges-raised-by-the-historical-model"></a>Herausforderungen, die vom historischen Modell ausgelöst wurden

**Das Endergebnis war ein ausgereiftes, Funktions Reiches Lauf Zeit-und Entwickler Programmiermodell.** Mit dieser Funktion gab es jedoch einige wichtige Herausforderungen. Erstens war das Framework **monolithischer**, mit logisch unterschiedlichen Funktionseinheiten, die eng in derselben System. Web. dll-Assembly gekoppelt waren (z. b. die http-Kern Objekte mit dem Web Forms-Framework). Zweitens war ASP.net als Teil des größeren .NET Framework enthalten, das bedeutete, dass die **Zeit zwischen den Releases in der Reihenfolge der Jahre lag.** Dies machte es für ASP.net schwierig, alle Änderungen in der schnell entwickelnden Webentwicklung mit allen Änderungen zu übernehmen. Schließlich wurde "System. Web. dll" selbst auf verschiedene Arten an eine bestimmte webhostingoption gekoppelt: Internetinformationsdienste (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Evolutionäre Schritte: ASP.NET MVC und ASP.net-Web-API

Und viele Änderungen an der Webentwicklung. Webanwendungen wurden zunehmend als eine Reihe kleiner, fokussierte Komponenten und nicht als große Frameworks entwickelt. Die Anzahl der Komponenten und die Häufigkeit, mit der Sie freigegeben wurden, stieg zu einer immer schnelleren Rate. Es ist klar, dass die Beibehaltung des webins mit dem Web Frameworks erfordern würde, dass Frameworks kleiner, entkoppelter und eher schwer zu finden sind, anstatt größer und funktionsreicher zu sein. daher **hat das ASP.net-Team mehrere evolutionäre Schritte ausgeführt, um ASP.net als eine Familie austauschbarer Webkomponenten anstelle eines einzelnen Frameworks zu ermöglichen**.

Eine der frühen Änderungen war der Anstieg der Beliebtheit des bekannten Model-View-Controller-Entwurfs Musters (MVC) dank Webentwicklungs-Frameworks wie Ruby on Rails. Diese Art der Entwicklung von Webanwendungen verschafft dem Entwickler eine bessere Kontrolle über das Markup Ihrer Anwendung, während gleichzeitig die Trennung von Markup und Geschäftslogik beibehalten wurde. Dies war einer der ersten Verkaufspunkte für ASP.net. Um die Nachfrage nach diesem Stil der Webanwendungs Entwicklung zu erfüllen, hat Microsoft die Gelegenheit genommen, sich für die Zukunft besser zu positionieren, indem **ASP.NET MVC out-of-Band entwickelt** wird (und nicht in die .NET Framework). ASP.NET MVC wurde als unabhängiger Download veröffentlicht. Dadurch hat das Engineering-Team die Flexibilität, Updates viel häufiger bereitzustellen, als zuvor möglich waren.

Ein weiterer wichtiger Wandel bei der Entwicklung von Webanwendungen war der Wechsel von dynamischen, vom Server generierten Webseiten zu statischem anfangs Markup mit dynamischen Abschnitten der Seite, die aus Client seitigem Skript für die Kommunikation mit Back-End- **Web-APIs über AJAX-Anforderungen**generiert wurden. Diese Architektur Schicht hat das Aufkommen von Web-APIs und die Entwicklung des ASP.net-Web-API Frameworks geholfen. Wie im Fall von ASP.NET MVC bietet das Release von ASP.net-Web-API eine weitere Möglichkeit zur Weiterentwicklung von ASP.net als modulareres Framework. Das Engineering-Team hat die Gelegenheit genutzt und **ASP.net-Web-API so erstellt, dass es keine Abhängigkeiten von den in System. Web. dll gefundenen Kern Frameworktypen gab**. Dies ermöglichte zwei Dinge: Erstens bedeutete dies, dass sich ASP.net-Web-API in einer vollständig eigenständigen Weise entwickeln konnte (und die Iteration weiterhin schnell durchgeführt werden kann, da Sie über nuget geliefert wird). Zweitens: da es keine externen Abhängigkeiten von "System. Web. dll" gab und daher keine Abhängigkeiten von IIS vorhanden waren, ASP.net-Web-API die Funktion zur Durchführung auf einem benutzerdefinierten Host (z. b. eine Konsolenanwendung, ein Windows-Dienst usw.) enthalten.

### <a name="the-future-a-nimble-framework"></a>Die Zukunft: ein Bare-Framework

Durch das Entkoppeln von frameworkkomponenten untereinander und deren Freigabe auf nuget könnten Frameworks nun **unabhängig und schneller iterieren**. Außerdem erwies sich die Leistungsfähigkeit und Flexibilität der Self-Hosting-Funktion der Web-API für Entwickler, die einen **kleinen, kleinen Host** für ihre Dienste wünschen, als sehr attraktiv erwiesen. Es hat sich als attraktiv erwiesen, dass andere Frameworks diese Funktion ebenfalls benötigen, und dies stellte eine neue Herausforderung dar, da jedes Framework in einem eigenen Host Prozess auf der Basisadresse ausgeführt wurde und für die Verwaltung (gestartet, beendet usw.) unabhängig war. Eine moderne Webanwendung unterstützt im allgemeinen statische Datei Bereitstellung, dynamische Seitengenerierung, Web-API und neuere Echt Zeit-/Pushbenachrichtigungen. Es ist nicht realistisch, dass jeder dieser Dienste unabhängig voneinander ausgeführt und verwaltet werden sollte.

Was erforderlich war, war eine einzelne hostingabstraktion, die es Entwicklern ermöglicht, eine Anwendung aus einer Vielzahl verschiedener Komponenten und Frameworks zu verfassen und diese Anwendung dann auf einem unterstützenden Host auszuführen.

## <a name="the-open-web-interface-for-net-owin"></a>Die Open Web Interface für .net (owin)

 Inspiriert von den Vorteilen, die das [Gestell](http://rack.github.io/) in der Ruby-Community erzielt hat, haben mehrere Mitglieder der .net-Community festgelegt, dass eine Abstraktion zwischen den Webserver und den frameworkkomponenten erstellt wird. Zwei Entwurfs Ziele für die owin-Abstraktion waren, dass Sie einfach war und dass Sie die geringstmöglichen Abhängigkeiten von anderen Frameworktypen brauchte. Diese beiden Ziele helfen dabei, Folgendes sicherzustellen:

- Neue Komponenten könnten leichter entwickelt und genutzt werden.
- Anwendungen können leichter zwischen Hosts und potenziell ganzen Plattformen/Betriebssystemen portiert werden.

Die resultierende Abstraktion besteht aus zwei Kernelementen. Das erste ist das Umgebungs Wörterbuch. Diese Datenstruktur ist dafür verantwortlich, den gesamten Zustand zu speichern, der für die Verarbeitung einer HTTP-Anforderung und-Antwort sowie für jeden relevanten Server Zustand erforderlich ist. Das Umgebungs Wörterbuch ist wie folgt definiert:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Ein owin-kompatibler Webserver ist für das Auffüllen des Umgebungs Wörterbuchs mit Daten wie Text Ströme und Header Auflistungen für eine HTTP-Anforderung und-Antwort verantwortlich. Es ist dann die Aufgabe der Anwendungs-oder frameworkkomponenten, das Wörterbuch mit zusätzlichen Werten zu füllen oder zu aktualisieren und in den Antworttext-Stream zu schreiben.

Zusätzlich zur Angabe des Typs für das Umgebungs Wörterbuch definiert die owin-Spezifikation eine Liste von Schlüssel-Wert-Paaren für Schlüsselwörter. In der folgenden Tabelle werden z. b. die erforderlichen Wörterbuch Schlüssel für eine HTTP-Anforderung angezeigt:

| Schlüsselname | Wert Beschreibung |
| --- | --- |
| `"owin.RequestBody"` | Ein Stream mit dem Anforderungs Text, falls vorhanden. Stream. NULL kann als Platzhalter verwendet werden, wenn kein Anforderungs Text vorhanden ist. Siehe [Anforderungs Text](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Eine `IDictionary<string, string[]>` von Anforderungs Headern. Siehe [Header](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Eine `string`, die die HTTP-Anforderungs Methode der Anforderung enthält (z. b. `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Eine `string`, die den Anforderungs Pfad enthält. Der Pfad muss relativ zum "root" des Anwendungs Delegaten sein. siehe [Pfade](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Eine `string`, die den Teil des Anforderungs Pfads enthält, der dem "root" des Anwendungs Delegaten entspricht. siehe [Pfade](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Eine `string`, die den Protokollnamen und die Version enthält (z. b. `"HTTP/1.0"` oder `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Eine `string`, die die Abfrage Zeichenfolgen-Komponente des HTTP-Anforderungs-URI enthält, ohne das führende "?" (z. b. `"foo=bar&baz=quux"`). Der Wert kann eine leere Zeichenfolge sein. |
| `"owin.RequestScheme"` | Eine `string`, die das für die Anforderung verwendete URI-Schema enthält (z. b. `"http"`, `"https"`). siehe [URI-Schema](http://owin.org/html/owin.html#5-1-uri-scheme). |

Das zweite Schlüsselelement von owin ist der Anwendungs Delegat. Dabei handelt es sich um eine Funktions Signatur, die als primäre Schnittstelle zwischen allen Komponenten in einer owin-Anwendung fungiert. Die Definition für den Anwendungs Delegaten lautet wie folgt:

`Func<IDictionary<string, object>, Task>;`

Der Anwendungs Delegat ist dann einfach eine Implementierung des Func-Delegattyps, bei dem die Funktion das Umgebungs Wörterbuch als Eingabe akzeptiert und eine Aufgabe zurückgibt. Dieser Entwurf hat einige Auswirkungen auf Entwickler:

- Zum Schreiben von owin-Komponenten ist eine sehr geringe Anzahl von typabhängigkeiten erforderlich. Dadurch wird die Zugriffsmöglichkeit von owin für Entwickler erheblich erhöht.
- Der asynchrone Entwurf ermöglicht die effiziente Abstraktion mit der Verarbeitung von Computerressourcen, insbesondere bei mehr e/a-intensiven Vorgängen.
- Da der Anwendungs Delegat eine atomarische Ausführungs Einheit ist und das Umgebungs Wörterbuch als Parameter für den Delegaten übernommen wird, können owin-Komponenten problemlos miteinander verkettet werden, um komplexe http-Verarbeitungs Pipelines zu erstellen.

Aus Implementierungs Sicht ist owin eine Spezifikation ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Das Ziel ist nicht das nächste Webframework, sondern eine Spezifikation für die Interaktion von Webframe Works und Webservern.

Wenn Sie [owin](http://owin.org/) oder [Katana](https://github.com/aspnet/AspNetKatana/wiki)untersucht haben, haben Sie möglicherweise auch das [owin-nuget-Paket](http://nuget.org/packages/Owin) und die owin. dll bemerkt. Diese Bibliothek enthält eine einzige Schnittstelle, [iappbuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), mit der die in [Abschnitt 4](http://owin.org/html/owin.html#4-application-startup) der owin-Spezifikation beschriebene Startsequenz formatiert und comiert wird. Obwohl es nicht erforderlich ist, um owin-Server zu erstellen, stellt die [iappbuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) -Schnittstelle einen konkreten Bezugspunkt bereit und wird von den Projektkomponenten von Katana verwendet.

## <a name="project-katana"></a>Project Katana

Während sowohl die [owin](http://owin.org/html/owin.html) -Spezifikation als auch die *owin. dll* -Community im Besitz von Open Source-Aktivitäten sind, stellt das [Katana](https://github.com/aspnet/AspNetKatana/wiki) -Projekt den Satz von owin-Komponenten dar, die von Microsoft erstellt und freigegeben werden. Diese Komponenten umfassen beide Infrastrukturkomponenten, z. b. Hosts und Server, sowie funktionale Komponenten wie z. b. Authentifizierungs Komponenten und Bindungen an Frameworks wie [signalr](../../../signalr/index.md) und [ASP.net-Web-API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Das Projekt verfügt über die folgenden drei Ziele: 

- **Portable** –-Komponenten sollten in der Lage sein, neue Komponenten problemlos zu ersetzen, sobald Sie verfügbar werden. Dies schließt alle Arten von Komponenten ein, vom Framework zum Server und Host. Diese Zielsetzung besteht darin, dass Frameworks von Drittanbietern nahtlos auf Microsoft-Servern ausgeführt werden können, während Microsoft-Frameworks potenziell auf Servern und Hosts von Drittanbietern ausgeführt werden können.
- **Modular/flexibel**– im Gegensatz zu vielen Frameworks, die eine Vielzahl von Features enthalten, die standardmäßig aktiviert sind, sollten die Katana-Projektkomponenten klein und schwer fokussiert sein, sodass der Anwendungsentwickler die Kontrolle darüber erhält, welche Komponenten in Ihrer Anwendung verwendet werden sollen.
- **Lightweight/Performance/Scalable** – durch Aufteilen des herkömmlichen Konzepts eines Frameworks in eine Reihe kleiner, fokussierte Komponenten, die vom Anwendungsentwickler explizit hinzugefügt werden, kann eine sich ergebende Katana-Anwendung weniger Computerressourcen verbrauchen und somit eine höhere Auslastung als andere Typen von Servern und Frameworks verarbeiten. Da die Anforderungen der Anwendung mehr Features aus der zugrunde liegenden Infrastruktur erfordern, können diese der owin-Pipeline hinzugefügt werden. Dies sollte jedoch eine explizite Entscheidung für den Teil des Anwendungs Entwicklers sein. Außerdem bedeutet die Ersetzung von Komponenten auf niedrigerer Ebene, dass neue Hochleistungsserver nahtlos eingeführt werden können, um die Leistung von owin-Anwendungen zu verbessern, ohne diese Anwendungen zu unterbrechen.

## <a name="getting-started-with-katana-components"></a>Einstieg in Katana-Komponenten

Bei der ersten Einführung war ein Aspekt des [node. js](http://nodejs.org/) -Frameworks, das die Aufmerksamkeit der Menschen sofort aufzeichnete, die Einfachheit, mit der ein Webserver erstellt und ausgeführt werden konnte. Wenn die Katana-Ziele im Licht von [node. js](http://nodejs.org/)eingeschlossen wurden, könnte man Sie zusammenfassen, indem man sagt, dass Katana viele der Vorteile von [node. js](http://nodejs.org/) (und Frameworks wie Sie) bringt, ohne dass der Entwickler alles auswirft, was Sie über die Entwicklung von ASP.NET-Webanwendungen weiß. Damit diese Anweisung True ist, sollten die ersten Schritte mit dem Katana-Projekt für [node. js](http://nodejs.org/)genauso einfach sein.

## <a name="creating-hello-world"></a>"Hallo Welt!" wird erstellt.

Ein wichtiger Unterschied zwischen der JavaScript-und der .net-Entwicklung ist das vorhanden sein (oder fehlen) eines Compilers. Als Ausgangspunkt für einen einfachen Katana-Server dient das Visual Studio-Projekt. Wir können jedoch mit den geringsten Projekttypen beginnen: der leeren ASP.NET-Webanwendung.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Als nächstes installieren wir das nuget-Paket " [Microsoft. owin. Host. systemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) " im Projekt. Dieses Paket stellt einen owin-Server bereit, der in der ASP.net Request-Pipeline ausgeführt wird. Sie finden Sie im [nuget](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) -Katalog und können entweder mithilfe des Dialog Felds "Visual Studio-Paket-Manager" oder der Paket-Manager-Konsole mit dem folgenden Befehl installiert werden:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Wenn Sie das `Microsoft.Owin.Host.SystemWeb` Paket installieren, werden einige zusätzliche Pakete als Abhängigkeiten installiert. Eine dieser Abhängigkeiten ist `Microsoft.Owin`, eine Bibliothek, die mehrere Hilfstypen und Methoden zum Entwickeln von owin-Anwendungen bereitstellt. Mit diesen Typen können Sie schnell den folgenden "Hello World"-Server schreiben.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Dieser sehr einfache Webserver kann nun mit dem Befehl **F5** von Visual Studio ausgeführt werden und bietet vollständige Unterstützung für das Debuggen.

## <a name="switching-hosts"></a>Wechseln von Hosts

Standardmäßig wird das vorherige "Hello World"-Beispiel in der ASP.net Request-Pipeline ausgeführt, die System. Web im Kontext von IIS verwendet. Dies kann selbst zu einem enormen Nutzen beitragen, da wir die Flexibilität und Zusammenstellung einer owin-Pipeline mit den Verwaltungsfunktionen und der Gesamt Reife von IIS nutzen können. Es gibt jedoch Situationen, in denen die von IIS bereitgestellten Vorteile nicht erforderlich sind und der Wunsch für einen kleineren, leichteren Host ist. Was ist erforderlich, um unseren einfachen Webserver außerhalb von IIS und System. Web auszuführen?

Um das Portabilitäts Ziel zu veranschaulichen, erfordert das Verschieben eines Webserver Hosts auf einen Befehlszeilen Host lediglich das Hinzufügen der neuen Server-und Host Abhängigkeiten zum Ausgabeordner des Projekts und das anschließende Starten des Hosts. In diesem Beispiel hosten wir den Webserver auf einem Katana-Host mit dem Namen `OwinHost.exe` und verwenden den auf Katana HttpListener basierenden Server. Ebenso wie die anderen Katana-Komponenten werden diese von nuget mit dem folgenden Befehl abgerufen:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

In der Befehlszeile können wir dann zum Stamm Ordner des Projekts navigieren und einfach den `OwinHost.exe` ausführen (der im Ordner "Tools" des entsprechenden nuget-Pakets installiert wurde). Standardmäßig ist `OwinHost.exe` für die Suche nach dem HttpListener-basierten Server konfiguriert, sodass keine zusätzliche Konfiguration erforderlich ist. Wenn Sie in einem Webbrowser zu `http://localhost:5000/` navigieren, wird die Anwendung angezeigt, die jetzt über die Konsole ausgeführt wird.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana-Architektur

 Die Architektur der Katana-Komponente unterteilt eine Anwendung in vier logische Ebenen, wie unten dargestellt: *Host, Server, Middleware* und *Anwendung*. Die Komponentenarchitektur ist so factoriert, dass Implementierungen dieser Schichten in vielen Fällen problemlos ersetzt werden können, ohne dass die Anwendung neu kompiliert werden muss.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 Der Host ist für Folgendes zuständig:

- Verwalten des zugrunde liegenden Prozesses.
- Orchestrieren des Workflows, der zur Auswahl eines Servers und der Erstellung einer owin-Pipeline führt, über die Anforderungen verarbeitet werden.

  Derzeit gibt es drei primäre Hostingoptionen für Katana-basierte Anwendungen:  
  
**IIS/ASP. net**: bei Verwendung der Standardtypen "HttpModule" und "HttpHandler" können owin-Pipelines auf IIS als Teil eines ASP.net-Anforderungs Flusses ausgeführt werden. Die ASP.net-hostingunterstützung wird aktiviert, indem das nuget-Paket Microsoft. Aspnet. Host. systemWeb in einem Webanwendungs Projekt installiert wird. Da IIS sowohl als Host als auch als Server fungiert, wird die Unterscheidung von owin-Server/-Host in diesem nuget-Paket zusammengeführt. Dies bedeutet, dass ein Entwickler bei Verwendung des systemWeb-Hosts keine Alternative Server Implementierung ersetzen kann.  
  
**Benutzerdefinierter Host**: die Katana Component Suite bietet Entwicklern die Möglichkeit, Anwendungen in einem eigenen benutzerdefinierten Prozess zu hosten, egal ob es sich um eine Konsolenanwendung, einen Windows-Dienst usw. handelt. Diese Funktion ähnelt der von der Web-API bereitgestellten Self-Host-Funktion. Das folgende Beispiel zeigt einen benutzerdefinierten Host für Web-API-Code:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Das Self-Host-Setup für eine Katana-Anwendung ist ähnlich:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Ein wichtiger Unterschied zwischen den Self-Host-Beispielen für Web-API und Katana besteht darin, dass der Web-API-Konfigurations Code im Beispiel für das Katana-Self-Host-Beispiel fehlt. Um die Portabilität und die Zusammensetz barkeit zu ermöglichen, trennt Katana den Code, der den Server startet, von dem Code, der die Pipeline für die Anforderungs Verarbeitung konfiguriert. Der Code, der die Web-API konfiguriert, ist in der Klasse Startup enthalten, die zusätzlich als Typparameter in WebApplication. Start angegeben wird.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Die Startup-Klasse wird weiter unten in diesem Artikel ausführlicher erläutert. Der Code, der zum Starten eines Katana-Self-Host-Prozesses erforderlich ist, ähnelt dem Code, den Sie heute möglicherweise in ASP.net-Web-API Self-Host-Anwendungen verwenden.

**Owinhost. exe**: während einige einen benutzerdefinierten Prozess zum Ausführen von Katana-Webanwendungen schreiben möchten, bevorzugen viele einfach eine vorgefertigte ausführbare Datei, die einen Server starten und Ihre Anwendung ausführen kann. In diesem Szenario enthält die Katana-Komponentensuite `OwinHost.exe`. Wenn die ausführbare Datei innerhalb des Stamm Verzeichnisses eines Projekts ausgeführt wird, wird ein Server gestartet (Standardmäßig wird der HttpListener-Server verwendet), und Konventionen werden verwendet, um die Startklasse des Benutzers zu suchen und auszuführen. Für eine präzisetere Steuerung stellt die ausführbare Datei eine Reihe zusätzlicher Befehlszeilenparameter bereit.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Während der Host für das Starten und Verwalten des Prozesses zuständig ist, in dem die Anwendung ausgeführt wird, besteht die Verantwortung des Servers darin, einen Netzwerksocket zu öffnen, auf Anforderungen zu lauschen und diese über die Pipeline von owin-Komponenten zu senden, die vom Benutzer angegeben werden (wie Sie möglicherweise bereits bemerkt hat, dass diese Pipeline in der Startup-Klasse des Anwendungs Entwicklers angegeben ist. Zurzeit enthält das Katana-Projekt zwei Server Implementierungen: 

- **Microsoft. owin. Host. systemWeb**: wie bereits erwähnt, fungiert IIS in zusammen mit der ASP.NET-Pipeline sowohl als Host als auch als Server. Wenn Sie diese Hostingoption auswählen, werden daher von IIS beide Aspekte auf Hostebene verwaltet, z. b. die Prozess Aktivierung und die Überwachung von HTTP-Anforderungen Für ASP.NET-Webanwendungen sendet Sie die Anforderungen dann an die ASP.NET-Pipeline. Der Katana systemWeb-Host registriert ein ASP.net HttpModule und einen HttpHandler, um Anforderungen abzufangen, wenn Sie die HTTP-Pipeline durchlaufen, und sendet Sie über die benutzerdefinierte owin-Pipeline.
- **Microsoft. owin. Host. HttpListener**: wie der Name anzeigt, verwendet dieser Katana-Server die HttpListener-Klasse des .NET Framework, um einen Socket zu öffnen und Anforderungen an eine vom Entwickler angegebene owin-Pipeline zu senden. Dies ist derzeit die Standard Server Auswahl für die Katana-Self-Host-API und owinhost. exe.

## <a name="middlewareframework"></a>Middleware/Framework

 Wie bereits erwähnt, ist es für den Fall, dass der Server eine Anforderung von einem Client annimmt, dafür verantwortlich, ihn über eine Pipeline von owin-Komponenten zu übergeben, die durch den Startcode des Entwicklers angegeben werden. Diese Pipeline Komponenten werden als Middleware bezeichnet.  
 Auf der Basisebene muss eine owin-Middleware-Komponente einfach den owin-Anwendungs Delegaten implementieren, damit Sie aufgerufen werden kann.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Um die Entwicklung und Komposition von middlewarekomponenten zu vereinfachen, unterstützt Katana jedoch einige Konventionen und Hilfstypen für middlewarekomponenten. Am häufigsten handelt es sich um die `OwinMiddleware`-Klasse. Eine benutzerdefinierte Middleware-Komponente, die mit dieser Klasse erstellt wurde, würde etwa wie folgt aussehen: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Diese Klasse wird von `OwinMiddleware`abgeleitet, implementiert einen Konstruktor, der eine Instanz der nächsten Middleware in der Pipeline als eines der Argumente annimmt, und übergibt sie an den Basiskonstruktor. Zusätzliche Argumente, die zum Konfigurieren der Middleware verwendet werden, werden nach dem nächsten Middleware-Parameter auch als Konstruktorparameter deklariert.   
  
Zur Laufzeit wird die Middleware über die überschriebene `Invoke` Methode ausgeführt. Diese Methode nimmt ein einzelnes Argument vom Typ "`OwinContext`" an. Dieses Kontext Objekt wird durch das zuvor beschriebene `Microsoft.Owin` nuget-Paket bereitgestellt und ermöglicht einen stark typisierten Zugriff auf das Anforderungs-, Antwort-und Umgebungs Wörterbuch sowie einige zusätzliche Hilfstypen.   
  
Die Middleware-Klasse kann der owin-Pipeline im Anwendungs Startcode wie folgt problemlos hinzugefügt werden:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Da die Katana-Infrastruktur einfach eine Pipeline von owin-middlewarekomponenten erstellt und die Komponenten lediglich den Anwendungs Delegaten für die Teilnahme an der Pipeline unterstützen müssen, können middlewarekomponenten die Komplexität von einfachen Protokollierungen bis hin zu ganzen Frameworks wie ASP.net, Web-API oder [signalr](../../../signalr/index.md)unterstützen. Wenn Sie beispielsweise ASP.net-Web-API zur vorherigen owin-Pipeline hinzufügen möchten, müssen Sie den folgenden Startcode hinzufügen:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Die Katana-Infrastruktur erstellt die Pipeline der middlewarekomponenten basierend auf der Reihenfolge, in der Sie dem iappbuilder-Objekt in der Konfigurations Methode hinzugefügt wurden. In unserem Beispiel kann loggermiddleware alle Anforderungen verarbeiten, die durch die Pipeline geleitet werden, unabhängig davon, wie diese Anforderungen letztendlich verarbeitet werden. Dies ermöglicht leistungsstarke Szenarios, in denen eine Middlewarekomponente (z. b. eine Authentifizierungs Komponente) Anforderungen für eine Pipeline verarbeiten kann, die mehrere Komponenten und Frameworks (z. b. ASP.net-Web-API, signalr und einen statischen Dateiserver) enthält.
 
## <a name="applications"></a>Anwendungen

Wie in den vorherigen Beispielen veranschaulicht, sollten owin und das Katana-Projekt nicht als neues Anwendungs Programmiermodell betrachtet werden, sondern als Abstraktion, um Anwendungs Programmier Modelle und Frameworks von der Server-und der Hostinginfrastruktur zu entkoppeln. Wenn Sie z. b. Web-API-Anwendungen entwickeln, verwendet das Entwickler Framework weiterhin das ASP.net-Web-API Framework, unabhängig davon, ob die Anwendung in einer owin-Pipeline mithilfe von Komponenten aus dem Katana-Projekt ausgeführt wird. Die Stelle, an der owin-bezogener Code für den Anwendungsentwickler sichtbar ist, ist der Startcode der Anwendung, bei dem der Entwickler die owin-Pipeline verfasst. Im Startcode registriert der Entwickler eine Reihe von usexx-Anweisungen, die im Allgemeinen für jede Middlewarekomponente verwendet werden, die eingehende Anforderungen verarbeitet. Diese Vorgehensweise hat dieselbe Auswirkung wie das Registrieren von HTTP-Modulen in der aktuellen System. Web-Welt. In der Regel wird eine größere frameworkmiddleware, wie ASP.net-Web-API oder [signalr](../../../signalr/index.md) , am Ende der Pipeline registriert. Quer greifende middlewarekomponenten, z. b. die für die Authentifizierung oder Zwischenspeicherung, werden im Allgemeinen am Anfang der Pipeline registriert, damit Sie Anforderungen für alle Frameworks und Komponenten verarbeiten, die später in der Pipeline registriert werden. Diese Trennung der Middleware-Komponenten untereinander und von den zugrunde liegenden Infrastrukturkomponenten ermöglicht, dass sich die Komponenten in unterschiedlichen Geschwindigkeiten weiterentwickeln und gleichzeitig sicherstellen, dass das Gesamtsystem stabil bleibt.

## <a name="components--nuget-packages"></a>Komponenten – nuget-Pakete

Wie viele aktuelle Bibliotheken und Frameworks werden die Katana-Projektkomponenten als Satz von nuget-Paketen übermittelt. Für die bevorstehende Version 2,0 sieht das Katana-Paket-Abhängigkeits Diagramm wie folgt aus. (Klicken Sie auf das Bild, um es zu vergrößern.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Fast jedes Paket im Katana-Projekt hängt direkt oder indirekt vom owin-Paket ab. Möglicherweise denken Sie daran, dass es sich hierbei um das Paket mit der iappbuilder-Schnittstelle handelt, die eine konkrete Implementierung der Startsequenz der Anwendung bereitstellt, die in Abschnitt 4 der owin-Spezifikation beschrieben wird. Außerdem sind viele Pakete von Microsoft. owin abhängig, das eine Reihe von Hilfstypen zum Arbeiten mit HTTP-Anforderungen und-Antworten bereitstellt. Der Rest des Pakets kann entweder als Hosting-Infrastrukturpakete (Server oder Hosts) oder Middleware klassifiziert werden. Pakete und Abhängigkeiten, die für das Katana-Projekt extern sind, werden in Orange angezeigt.

Die Hostinginfrastruktur für Katana 2,0 umfasst sowohl den systemWeb-als auch den HttpListener-basierten Server, das owinhost-Paket zum Ausführen von owin-Anwendungen mithilfe von "owinhost. exe" und das Microsoft. owin. Hosting-Paket für Self-Hosting-owin-Anwendungen in einer benutzerdefinierter Host (z. b. Konsolenanwendung, Windows-Dienst usw.)

Bei Katana 2,0 konzentrieren sich die Middleware-Komponenten hauptsächlich auf die Bereitstellung unterschiedlicher Authentifizierungsmöglichkeiten. Es wird eine zusätzliche Middlewarekomponente für die Diagnose bereitgestellt, die die Unterstützung für eine Start-und Fehlerseite ermöglicht. Wenn owin in die de-facto-Hosting-Abstraktion wächst, wächst auch das Ökosystem der middlewarekomponenten, sowohl von Microsoft als auch von Drittanbietern.

## <a name="conclusion"></a>Zusammenfassung

 Von Anfang an war das Ziel des Katana-Projekts nicht, Entwickler zu erstellen und so zu erzwingen, dass Entwickler noch ein weiteres Webframework erlernen können. Vielmehr war das Ziel, eine Abstraktion zu erstellen, um Entwicklern von .NET-Webanwendungen mehr Auswahl zu ermöglichen, als zuvor möglich war. Durch die Aufteilung der logischen Ebenen eines typischen Webanwendungs Stapels in eine Reihe von ersetzbaren Komponenten ermöglicht das Katana-Projektkomponenten im gesamten Stapel, die für diese Komponenten sinnvollsten Raten zu verbessern. Durch das Erstellen aller Komponenten um die einfache owin-Abstraktion ermöglicht Katana, dass Frameworks und die darauf erstellten Anwendungen über eine Vielzahl von Servern und Hosts portierbar sind. Durch den Entwickler, der die Kontrolle über den Stapel erhält, stellt Katana sicher, dass der Entwickler die ultimative Auswahl treffen kann, wie einfach oder wie Funktionsumfang der webstapel sein sollte.  

## <a name="for-more-information-about-katana"></a>Weitere Informationen zu Katana

- Das Katana-Projekt auf GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Video: [das Katana-Projekt-owin für ASP.net](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)von Howard Dierking.

## <a name="acknowledgements"></a>Danksagung

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) Rick ist Senior Programming Writer für Microsoft, der sich auf Azure und MVC konzentriert.
- [Scott Hanselman](http://www.hanselman.com/blog/): (Twitter- [@shanselman](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (Twitter- [@jongalloway](https://twitter.com/jongalloway) )
