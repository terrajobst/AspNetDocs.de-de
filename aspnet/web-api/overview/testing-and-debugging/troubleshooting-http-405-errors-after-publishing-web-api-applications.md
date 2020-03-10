---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Problembehandlung bei Web api2-apps, die in Visual Studio funktionieren und auf einem Produktions-IIS-Server fehlschlagen
author: rmcmurray
description: Problembehandlung bei Web api2-apps, die in Visual Studio funktionieren und auf einem Produktions-IIS-Server fehlschlagen
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 1b47f1ade3619cfd010260352f6a96985ab3598b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447027"
---
# <a name="troubleshoot-web-api2-apps-that-work-in-visual-studio-and-fail-on-a-production-iis-server"></a>Problembehandlung bei Web api2-apps, die in Visual Studio funktionieren und auf einem Produktions-IIS-Server fehlschlagen

> Dieses Dokument erläutert die Problembehandlung für Web api2-apps, die auf einem Produktions-IIS-Server bereitgestellt werden. Sie behandelt häufige http 405-und 501-Fehler.
> 
> ## <a name="software-used-in-this-tutorial"></a>In diesem Tutorial verwendete Software
> 
> 
> - [Internetinformationsdienste (IIS)](https://www.iis.net/) (Version 7 oder höher)
> - [Web-API](../../index.md) 

Web-API-Apps verwenden in der Regel mehrere HTTP-Verben: Get, Post, Put, DELETE und manchmal Patch. Das heißt, Entwickler könnten in Situationen eintreten, in denen diese Verben von einem anderen IIS-Modul auf Ihrem IIS-Produktionsserver implementiert werden. Dies führt zu einer Situation, in der ein Web-API-Controller, der in Visual Studio oder auf einem Entwicklungs Server ordnungsgemäß funktioniert, Gibt einen HTTP 405-Fehler zurück, wenn er auf einem Produktions-IIS-Server bereitgestellt wird.

## <a name="what-causes-http-405-errors"></a>Was verursacht http 405-Fehler?

Der erste Schritt, um zu lernen, wie http 405-Fehler behoben werden, besteht darin, zu verstehen, was ein HTTP 405-Fehler eigentlich bedeutet. Das primäre Dokument für http ist [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), das den HTTP-Statuscode http 405 als ***Methode***definiert und den Statuscode in einer Situation beschreibt, in der &quot;der in der Anforderungs Zeile angegebenen Methode für die durch den Anforderungs-URI angegebene Ressource nicht zulässig ist.&quot; bedeutet, dass das HTTP-Verb für die spezifische URL, die ein HTTP-Client angefordert hat, nicht zulässig ist.

Im folgenden finden Sie einige der am häufigsten verwendeten HTTP-Methoden, die in RFC 2616, RFC 4918 und RFC 5789 definiert sind:

| HTTP-Methode | Beschreibung |
| --- | --- |
| **GET** | Diese Methode wird verwendet, um Daten aus einem URI abzurufen, und wahrscheinlich die am häufigsten verwendete HTTP-Methode. |
| **HEAD** | Diese Methode ähnelt der Get-Methode, mit dem Unterschied, dass Sie die Daten nicht aus dem Anforderungs-URI abruft, sondern lediglich den HTTP-Status abruft. |
| **POST** | Diese Methode wird normalerweise verwendet, um neue Daten an den URI zu senden. Post wird häufig zum Übermitteln von Formulardaten verwendet. |
| **PUT** | Diese Methode wird normalerweise zum Senden von Rohdaten an den URI verwendet. Put wird häufig verwendet, um JSON-oder XML-Daten an Web-API-Anwendungen zu senden. |
| **DELETE** | Diese Methode wird verwendet, um Daten aus einem URI zu entfernen. |
| **OPTIONS** | Diese Methode wird normalerweise verwendet, um die Liste der HTTP-Methoden abzurufen, die für einen URI unterstützt werden. |
| **Verschieben Kopieren** | Diese beiden Methoden werden mit WebDAV verwendet, und Ihr Zweck ist selbsterklärend. |
| **MKCOL** | Diese Methode wird mit WebDAV verwendet und wird verwendet, um eine Auflistung (z. b. ein Verzeichnis) am angegebenen URI zu erstellen. |
| **PROPFIND PROPPATCH** | Diese beiden Methoden werden mit WebDAV verwendet und zum Abfragen und Festlegen von Eigenschaften für einen URI verwendet. |
| **Sperre aufheben** | Diese beiden Methoden werden mit WebDAV verwendet und zum Sperren bzw. Entsperren der durch den Anforderungs-URI identifizierten Ressource bei der Erstellung verwendet. |
| **PATCH** | Diese Methode wird verwendet, um eine vorhandene HTTP-Ressource zu ändern. |

Wenn eine dieser HTTP-Methoden für die Verwendung auf dem Server konfiguriert ist, antwortet der Server mit dem HTTP-Status und anderen Daten, die für die Anforderung geeignet sind. (Eine Get-Methode kann z. b. eine HTTP 200 ***OK*** -Antwort erhalten, und eine Put-Methode kann eine ***http 201-*** Antwort erhalten.)

Wenn die HTTP-Methode nicht für die Verwendung auf dem Server konfiguriert ist, antwortet der Server mit einem ***nicht implementierten*** HTTP 501-Fehler.

Wenn eine HTTP-Methode jedoch für die Verwendung auf dem Server konfiguriert ist, aber für einen gegebenen Uri deaktiviert wurde, antwortet der Server mit einem Fehler vom Typ "http 405- ***Methode nicht zulässig*** ".

## <a name="example-http-405-error"></a>Beispiel für http 405-Fehler

Die folgenden HTTP-Anforderungs-und Antwort Beispiele veranschaulichen eine Situation, in der ein HTTP-Client versucht, einen Wert in einer Web-API-APP auf einem Webserver zu platzieren, und der Server gibt einen HTTP-Fehler zurück, der besagt, dass die Put-Methode nicht zulässig ist:

HTTP-Anforderung:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]

HTTP-Antwort:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]

In diesem Beispiel hat der HTTP-Client eine gültige JSON-Anforderung an die URL für eine Web-API-Anwendung auf einem Webserver gesendet, aber der Server hat eine HTTP 405-Fehlermeldung zurückgegeben, die angibt, dass die Put-Methode für die URL nicht zulässig war. Wenn der Anforderungs-URI dagegen nicht mit einer Route für die Web-API-Anwendung identisch ist, gibt der Server einen Fehler vom Typ "http 404 ***nicht gefunden*** " zurück.

## <a name="resolve-http-405-errors"></a>Beheben von http 405-Fehlern

Es gibt mehrere Gründe, warum ein bestimmtes HTTP-Verb möglicherweise nicht zulässig ist, aber ein primäres Szenario ist die häufigste Ursache für diesen Fehler in IIS: für dasselbe Verb/dieselbe Methode sind mehrere Handler definiert, und einer der Handler blockiert die Verarbeitung der Anforderung durch den erwarteten Handler. Mithilfe der Erläuterung verarbeitet IIS die Handler von der ersten bis zum letzten, basierend auf den Bestell handlereinträgen in den Dateien " *ApplicationHost. config* " und " *Web. config* ", in denen die erste passende Kombination aus Pfad, Verb, Ressource usw. verwendet wird, um die Anforderung zu verarbeiten.

Das folgende Beispiel ist ein Auszug aus einer *ApplicationHost. config* -Datei für einen IIS-Server, der einen HTTP 405-Fehler zurückgegeben hat, wenn die Put-Methode verwendet wird, um Daten an eine Web-API-Anwendung zu senden. In diesem Auszug werden mehrere HTTP-Handler definiert, und jeder Handler verfügt über einen anderen Satz von HTTP-Methoden, für die er konfiguriert ist. der letzte Eintrag in der Liste ist der statische Inhalts Handler, der der Standard Handler ist, der verwendet wird, nachdem die anderen Handler ein chanc verwendet haben. e zum Untersuchen der Anforderung:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Im vorherigen Beispiel sind der WebDAV-Handler und der Erweiterungs lose URL-Handler für ASP.net (der für die Web-API verwendet wird) eindeutig für separate Listen von HTTP-Methoden definiert. Beachten Sie, dass der ISAPI-DLL-Handler für alle HTTP-Methoden konfiguriert ist, obwohl diese Konfiguration nicht zwangsläufig einen Fehler verursacht. Bei der Problembehandlung von http 405-Fehlern müssen jedoch Konfigurationseinstellungen wie diese berücksichtigt werden.

Im vorherigen Beispiel war der ISAPI-DLL-Handler nicht das Problem. Tatsächlich wurde das Problem nicht in der Datei " *ApplicationHost. config* " für den IIS-Server definiert. das Problem wurde durch einen Eintrag verursacht, der in der Datei " *Web. config* " durchgeführt wurde, als die Web-API-Anwendung in Visual Studio erstellt wurde. Der folgende Auszug aus der Datei " *Web. config* " der Anwendung zeigt den Speicherort des Problems an:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

In diesem Auszug wird der URL-Handler für die Erweiterungs lose Erweiterung für ASP.net neu definiert, um zusätzliche HTTP-Methoden zu enthalten, die mit der Web-API-Anwendung verwendet werden. Da jedoch ein ähnlicher Satz von HTTP-Methoden für den WebDAV-Handler definiert ist, tritt ein Konflikt auf. In diesem speziellen Fall wird der WebDAV-Handler definiert und von IIS geladen, obwohl WebDAV für die Website deaktiviert ist, die die Web-API-Anwendung enthält. Bei der Verarbeitung einer HTTP PUT-Anforderung ruft IIS das WebDAV-Modul auf, da es für das Put-Verb definiert ist. Wenn das WebDAV-Modul aufgerufen wird, überprüft es seine Konfiguration und sieht, dass es deaktiviert ist. Daher gibt es für jede Anforderung, die einer WebDAV-Anforderung ähnelt, einen Fehler vom Typ "http 405- ***Methode nicht zulässig*** " zurück. Um dieses Problem zu beheben, sollten Sie WebDAV aus der Liste der HTTP-Module für die Website entfernen, auf der die Web-API-Anwendung definiert ist. Im folgenden Beispiel wird gezeigt, wie das aussehen könnte:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Dieses Szenario tritt häufig auf, nachdem eine Anwendung aus einer Entwicklungsumgebung in einer IIS-Produktionsumgebung veröffentlicht wurde. Dies liegt daran, dass sich die Liste der Handler/Module zwischen ihren Entwicklungs-und Produktionsumgebungen unterscheidet. Wenn Sie z. b. Visual Studio 2012 oder höher zum Entwickeln einer Web-API-Anwendung verwenden, ist IIS Express der Standardweb Server für Tests. Dieser entwicklungsweb Server ist eine herunter skalierte Version der vollständigen IIS-Funktionalität, die in einem Server Produkt enthalten ist. dieser entwicklungsweb Server enthält einige Änderungen, die für Entwicklungsszenarien hinzugefügt wurden. Beispielsweise wird das WebDAV-Modul häufig auf einem produktionsweb Server installiert, auf dem die Vollversion von IIS ausgeführt wird, obwohl er möglicherweise nicht verwendet wird. Mit der Entwicklungsversion von IIS (IIS Express) wird das WebDAV-Modul installiert, aber die Einträge für das WebDAV-Modul werden absichtlich auskommentiert, sodass das WebDAV-Modul nie auf IIS Express geladen wird, es sei denn, Sie ändern die IIS Express-Konfiguration. Einstellungen zum Hinzufügen von WebDAV-Funktionen zu Ihrer IIS Express Installation. Folglich funktioniert Ihre Webanwendung möglicherweise auf dem Entwicklungs Computer ordnungsgemäß, aber es treten möglicherweise http 405-Fehler auf, wenn Sie Ihre Web-API-Anwendung auf dem IIS-Webserver der Produktion veröffentlichen.

## <a name="http-501-errors"></a>HTTP 501-Fehler

* Gibt an, dass die spezifische Funktionalität auf dem Server nicht implementiert wurde.
* In der Regel bedeutet dies, dass in Ihren IIS-Einstellungen kein Handler definiert ist, der der HTTP-Anforderung entspricht:
  * Gibt wahrscheinlich an, dass etwas nicht ordnungsgemäß auf IIS installiert wurde.
  * Die IIS-Einstellungen wurden geändert, sodass keine Handler definiert sind, die die jeweilige HTTP-Methode unterstützen.

Um dieses Problem zu beheben, müssen Sie jede Anwendung neu installieren, die versucht, eine HTTP-Methode zu verwenden, für die Sie keine entsprechenden Modul-oder handlerdefinitionen hat.

## <a name="summary"></a>Zusammenfassung

Http 405-Fehler werden ausgelöst, wenn eine HTTP-Methode von einem Webserver für eine angeforderte URL nicht zulässig ist. Diese Bedingung tritt häufig auf, wenn ein bestimmter Handler für ein bestimmtes Verb definiert wurde und dieser Handler den Handler überschreibt, den Sie für die Verarbeitung der Anforderung erwarten.
