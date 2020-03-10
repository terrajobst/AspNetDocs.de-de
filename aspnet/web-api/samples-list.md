---
uid: web-api/samples-list
title: Web-API-Beispielliste-ASP.NET 4. x
author: rick-anderson
description: ASP.net-Web-API Liste mit Beispielen für ASP.NET 4. x
ms.author: riande
ms.date: 09/18/2012
ms.custom: seoapril2019
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 24368fc80e7fc3c840f1f1f9accf13fa15a06c1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484263"
---
# <a name="web-api-samples-list"></a>Liste der Web-API-Beispiele

## <a name="httpclient-samples"></a>HttpClient-Beispiele

**Beispiel** | [vs 2012-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Zeigt, wie der [Microsoft Translator-Dienst](https://msdn.microsoft.com/library/ff512419.aspx) mit der **HttpClient** -Klasse aufgerufen wird. Die Microsoft Translator-Dienst-API erfordert ein OAuth-Token, das von der Anwendung abgerufen wird, indem für jede Anforderung an den Konvertierungs Dienst eine Anforderung an den Azure-tokenserver gesendet wird. Das Ergebnis des tokenservers wird in die an den Übersetzungsdienst gesendete Anforderung eingefügt. Bevor Sie dieses Beispiel ausführen, müssen Sie einen [Anwendungs Schlüssel aus Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) abrufen und die Informationen in der accesstokenmessagehandler-Beispiel Klasse eingeben.

**Google Maps-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

Verwendet **HttpClient** zum Herunterladen einer Karte von Redmond, WA aus der [Google Maps-API](https://developers.google.com/maps/), speichert Sie als lokale Datei und öffnet die Standardbild Anzeige.

**Twitter-Client Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Zeigt, wie Sie einen einfachen Twitter-Client mithilfe von **HttpClient**schreiben. Das Beispiel verwendet einen **httpmessagehandler** , um OAuth-Authentifizierungsinformationen in die ausgehende **httprequestmessage**einzufügen. Das Ergebnis von Twitter wird mithilfe von JSON.net gelesen. Bevor Sie dieses Beispiel ausführen, müssen Sie einen [Anwendungs Schlüssel von Twitter](https://dev.twitter.com/)abrufen und die Informationen in der Beispiel Klasse oauthmessagehandler eingeben.

**World Bank Sample** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [vs 2010 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [vs 2012 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Erläutert das Abrufen von Daten von der World Bank Daten Site mithilfe von JSON.net, um das Ergebnis zu analysieren.

## <a name="web-api-samples"></a>Web-API-Beispiele

Ersten Schritte **mit ASP.net-Web-API** | [vs 2012-Quelle](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Zeigt, wie eine einfache Web-API erstellt wird, die HTTP-GET-Anforderungen unterstützt. Enthält den Quellcode für das [erste ASP.net-Web-API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.net-Web-API JavaScript-Szenarien – Kommentare** | [vs 2012-Quelle](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Zeigt, wie Sie mit ASP.net-Web-API Web-APIs erstellen, die Browser Clients unterstützen und problemlos mithilfe von jQuery aufgerufen werden können.

**Kontakt-Manager** - | [vs 2010-Quelle](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

In diesem Beispiel wird ASP.net-Web-API verwendet, um eine einfache Kontakt-Manager-Anwendung zu erstellen. Die Anwendung besteht aus einer Contact Manager-Web-API, die von einer ASP.NET MVC-Anwendung verwendet wird, und einer Windows Phone Anwendung, um eine Liste von Kontakten anzuzeigen und zu verwalten.

**Beispiel für Batch** Verarbeitung | [ausführliche Beschreibung](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [vs 2012-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Zeigt, wie die http-Batch Verarbeitung in ASP.NET implementiert wird. Die Batch Verarbeitung besteht darin, mehrere HTTP-Anforderungen innerhalb eines einzelnen mehrteiligen MIME-Entitäts Texts zu platzieren, der dann als HTTP Post an den Server gesendet wird. Die Anforderungen werden einzeln verarbeitet, und die Antworten werden in einen anderen MIME-mehrteiligen Entitäts Text eingefügt, der an den Client zurückgegeben wird.

**Inhalts Controller Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [vs 2010 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | [vs 2012 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

Zeigt, wie Anforderungs-und Antwort Entitäten asynchron mithilfe von Streams gelesen und geschrieben werden. Der Beispiel Controller hat zwei Aktionen: eine Put-Aktion, die den Anforderungs Entitäts Text asynchron liest und in einer lokalen Datei speichert, sowie eine Get-Aktion, die den Inhalt der lokalen Datei zurückgibt.

**Beispiel für benutzerdefinierte Assemblyresolver** | [vs 2012-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Zeigt, wie ASP.net-Web-API geändert wird, um die Ermittlung von Controllern aus einer dynamisch geladenen Bibliotheksassembly zu unterstützen. Im Beispiel wird ein benutzerdefinierter **iassembliesresolver** implementiert, der die Standard Implementierung aufruft und dann die Bibliotheksassembly den Standard Ergebnissen hinzufügt.

**Beispiel für einen benutzerdefinierten Medientyp-Formatierer** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [vs 2010-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Zeigt, wie ein benutzerdefiniertes Medientyp-Formatierer mithilfe der **bufferedmediatypeer Formatierungs Programm** -Basisklasse erstellt wird. Diese Basisklasse ist für Formatierer gedacht, in denen hauptsächlich synchrone Lese-und Schreibvorgänge verwendet werden. Das Beispiel zeigt den Medientyp Formatierer und zeigt, wie Sie ihn einbinden können, indem Sie ihn als Teil der **httpconfiguration** für Ihre Anwendung registrieren. Beachten Sie, dass es auch möglich ist, die **Media** Type-Basisklasse direkt für Formatierer zu verwenden, die in erster Linie asynchrone Lese-und Schreibvorgänge verwenden.

**Beispiel für eine benutzerdefinierte Parameter Bindung** | [ausführliche Beschreibung](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [vs 2010-Quelle](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Zeigt, wie der Parameter Bindungsprozess angepasst wird. dabei handelt es sich um den Prozess, der bestimmt, wie Informationen aus einer Anforderung an Aktionsparameter gebunden werden. In diesem Beispiel hat der Home-Controller vier Aktionen:

1. Bindprincipal zeigt, wie ein IPrincipal-Parameter von einem benutzerdefinierten generischen Prinzipal, nicht aus einer HTTP GET-Nachricht gebunden wird.
2. Bindcustomcomplextypefromuriorbody zeigt, wie ein komplexer Typparameter gebunden wird, der entweder aus dem Nachrichtentext oder aus dem Anforderungs-URI einer HTTP Post-Nachricht stammt.
3. BindCustomComplexTypeFromUriWithRenamedProperty shows how to bind a complex-type parameter with a renamed property which comes from the request URI of an HTTP POST message;
4. Postmultipleparametersfrombody zeigt, wie mehrere Parameter aus dem Text für eine Post-Nachricht gebunden werden.

**Beispiel für Datei Upload** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Veranschaulicht das Hochladen von Dateien in einen **apicontroller** mithilfe von MIME-Multipart-Datei Upload und das Einrichten von Status Benachrichtigungen mit **HttpClient** mithilfe von **progressnotificationhandler**. Der Controller liest den Inhalt eines HTML-Datei Uploads asynchron und schreibt mindestens einen Textteil in eine lokale Datei. Die Antwort enthält Informationen über die hochgeladene Datei (oder Dateien).

**Beispiel für das Hochladen von Dateien in einen Azure-BLOB-Speicher** | [ausführliche Beschreibung](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Dieses Beispiel ähnelt dem Beispiel zum Hochladen von Dateien, aber anstatt die hochgeladenen Dateien auf dem lokalen Datenträger zu speichern, lädt es die Dateien asynchron mithilfe des [Windows Azure SDK für .net](https://www.windowsazure.com/develop/net/)in den [Azure-BLOB-Speicher](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) hoch. Außerdem bietet es einen Mechanismus, mit dem die bloblisten aufgelistet werden, die derzeit in einem [Azure BLOB Storage Container](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)vorhanden sind. Sie können das Beispiel ausprobieren, das für **Azure Storage Emulator** ausgeführt wird, der im Azure SDK verfügbar ist. Wenn Sie über ein [Azure Storage Konto](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)verfügen, können Sie auch mit dem echten Speicherdienst ausführen.

**Beispiel für http-nachrichtenhandlerpipeline** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [vs 2010-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Zeigt, wie Sie **httpmessagehandler** -Instanzen sowohl auf dem Client (**HttpClient**) als auch auf dem Server (ASP.net-Web-API) verknüpfen. Im Beispiel wird derselbe Handler sowohl auf dem Client als auch auf dem Server verwendet. Obwohl es selten vorkommt, dass derselbe Handler an beiden Stellen ausgeführt würde, ist das Objektmodell auf Client-und Serverseite identisch.

**Beispiel für JSON-Upload** | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Veranschaulicht das Hochladen und Herunterladen von JSON in und von einem **apicontroller**. Das Beispiel verwendet einen minimalen **apicontroller** und greift mithilfe von **HttpClient**darauf zu.

**Mashup-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Zeigt, wie Sie in einer **apicontroller** -Aktion asynchron auf mehrere Remote Standorte zugreifen können. Jedes Mal, wenn die Aktion ausgeführt wird, werden die Anforderungen asynchron ausgeführt, sodass keine Threads blockiert werden.

**Beispiel** für die Speicher Ablauf Verfolgung | [ausführliche Beschreibung](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [vs 2010-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

Dieses Beispiel Projekt erstellt ein nuget-Paket, mit dem ein benutzerdefinierter in-Memory-Ablaufverfolgungs-Writer in ASP.net-Web-API Anwendungen installiert wird.

**MongoDB-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Zeigt, wie Sie MongoDB als permanenten Speicher für einen **apicontroller**verwenden, indem Sie ein Repository-Muster verwenden.

**Beispiel für Antworttext Prozessor** | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Zeigt, wie eine Antwort Entität (d. h. ein HTTP-Antworttext) in eine lokale Datei kopiert wird, bevor Sie an den Client übertragen wird. Außerdem wird die asynchrone Verarbeitung dieser Datei durchgeführt. Das Beispiel implementiert einen **httpmessagehandler** , der die Antwort Entität mit einer umschließt, die sich in der Ausgabe als normal und in eine lokale Datei schreibt.

**XDocument-Beispiel hochladen** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [vs 2012-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Zeigt, wie Sie ein XDocument mithilfe von **pushstreamcontent** und **HttpClient**in einen **apicontroller** hochladen.

**Validierungs Beispiel** | [vs 2010-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Zeigt, wie Sie Validierungs Attribute für Ihre Modelle in ASP.net WebAPI verwenden können, um den Inhalt der HTTP-Anforderung zu validieren. Veranschaulicht, wie Eigenschaften nach Bedarf gekennzeichnet werden, wie sowohl frameworkdefinierte als auch benutzerdefinierte Validierungs Attribute verwendet werden, um das Modell mit Anmerkungen zu versehen, und wie Fehler Antworten für ungültige Modell Zustände zurückgegeben werden.

**Web Form-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [vs 2010-Quelle](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Zeigt einen apicontroller an, der einem Web Forms Projekt hinzugefügt wurde.

**[Restbugs-Beispiel](https://github.com/howarddierking/RestBugs)**

Restbugs ist eine einfache Anwendung zur Fehler Nachverfolgung, die zeigt, wie ASP.net-Web-API und die neue HTTP-Client Bibliothek verwendet werden, um ein hypermediengesteuerte System zu erstellen. Das Beispiel umfasst sowohl Client-als auch Server Implementierungen mithilfe von ASP.net-Web-API. Der Server verwendet einen benutzerdefinierten Razor-Formatierer zum Generieren von Ressourcen Darstellungen. Das Beispiel bietet auch einen Node. js-Server, der die Vorteile der Verwendung eines hypermedienentwurfs zum Entkoppeln von Clients und Servern veranschaulicht.
