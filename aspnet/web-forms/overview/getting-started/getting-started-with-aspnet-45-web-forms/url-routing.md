---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL-Routing | Microsoft-Dokumentation
author: Erikre
description: Diese tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 66b727b69ca4f9a3d35b67f492f9a554146e09ef
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590711"
---
# <a name="url-routing"></a>URL-Routing

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese tutorialreihe vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio Express 2013 für das Web. Für diese tutorialreihe steht ein Visual Studio 2013- [Projekt mit C# Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) zur Verfügung.

In diesem Tutorial ändern Sie die Wingtip Toys-Beispielanwendung, um das URL-Routing zu unterstützen. Das Routing ermöglicht Ihrer Webanwendung die Verwendung von URLs, die benutzerfreundlich, leichter zu merken und besser von Suchmaschinen unterstützt werden. Dieses Tutorial baut auf dem vorherigen Tutorial "Mitgliedschaft und Administration" auf und ist Teil der Wingtip Toys-tutorialreihe.

## <a name="what-youll-learn"></a>Lernen Sie Folgendes:

- Vorgehensweise beim Registrieren von Routen für eine ASP.net-Web Forms Anwendung
- Vorgehensweise beim Hinzufügen von Routen zu einer Webseite.
- Auswählen von Daten aus einer Datenbank zur Unterstützung von Routen.

## <a name="aspnet-routing-overview"></a>Übersicht über ASP.NET Routing

Über das URL-Routing können Sie eine Anwendung so konfigurieren, dass Sie Anforderungs-URLs akzeptiert, die nicht physischen Dateien zugeordnet sind. Eine Anforderungs-URL ist einfach die URL, die ein Benutzer in seinen Browser eingibt, um eine Seite auf Ihrer Website zu finden. Sie verwenden das Routing, um URLs zu definieren, die für Benutzer semantisch aussagekräftig sind und die bei der Suchmaschinenoptimierung (Search Engine Optimization, SEO) helfen können.

Standardmäßig enthält die Web Forms Vorlage [ASP.net friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Ein Großteil der grundlegenden Routing Aufgaben wird mithilfe von *freundlichen URLs*implementiert. In diesem Tutorial fügen Sie jedoch angepasste Routing Funktionen hinzu.

Vor dem Anpassen des URL-Routings kann die Wingtip Toys-Beispielanwendung mithilfe der folgenden URL eine Verknüpfung mit einem Produkt herstellen:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Durch Anpassen des URL-Routings verknüpft die Wingtip Toys-Beispielanwendung mit dem gleichen Produkt, indem eine einfachere URL verwendet wird:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Routen

Eine Route ist ein URL-Muster, das einem-Handler zugeordnet ist. Der Handler kann eine physische Datei sein, z. b. eine ASPX-Datei in einer Web Forms Anwendung. Ein Handler kann auch eine Klasse sein, die die Anforderung verarbeitet. Zum Definieren einer Route erstellen Sie eine Instanz der Routen Klasse, indem Sie das URL-Muster, den Handler und optional einen Namen für die Route angeben.

Fügen Sie der Anwendung die Route hinzu, indem Sie das `Route`-Objekt der Eigenschaft static `Routes` der `RouteTable`-Klasse hinzufügen. Die Routes-Eigenschaft ist ein `RouteCollection` Objekt, das alle Routen für die Anwendung speichert.

### <a name="url-patterns"></a>URL-Muster

Ein URL-Muster kann Literalwerte und Variablen Platzhalter (als URL-Parameter bezeichnet) enthalten. Die Literale und Platzhalter befinden sich in Segmenten der URL, die durch den Schrägstrich (`/`) getrennt sind.

Wenn eine Anforderung an die Webanwendung gestellt wird, wird die URL in Segmente und Platzhalter analysiert, und die Variablen Werte werden dem Anforderungs Handler bereitgestellt. Dieser Prozess ähnelt der Analyse der Daten in einer Abfrage Zeichenfolge und der Übergabe an den Anforderungs Handler. In beiden Fällen sind Variablen Informationen in der URL enthalten und in Form von Schlüssel-Wert-Paaren an den Handler übermittelt. Für Abfrage Zeichenfolgen befinden sich sowohl die Schlüssel als auch die Werte in der URL. Bei Routen sind die Schlüssel die im URL-Muster definierten Platzhalter Namen, und nur die Werte befinden sich in der URL.

In einem URL-Muster definieren Sie Platzhalter, indem Sie Sie in geschweifte Klammern (`{` und `}`) einschließen. Sie können mehr als einen Platzhalter in einem Segment definieren, die Platzhalter müssen jedoch durch einen Literalwert getrennt werden. Beispielsweise ist `{language}-{country}/{action}` ein gültiges Routen Muster. `{language}{country}/{action}` ist jedoch kein gültiges Muster, da zwischen den Platzhaltern kein Literalwert oder Trennzeichen vorhanden ist. Daher kann Routing nicht bestimmen, wo der Wert für den sprach Platzhalter von dem Wert für den Land Platzhalter getrennt werden soll.

### <a name="mapping-and-registering-routes"></a>Mapping und Registrieren von Routen

Bevor Sie Routen zu Seiten der Wingtip Toys-Beispielanwendung einbinden können, müssen Sie die Routen beim Starten der Anwendung registrieren. Um die Routen zu registrieren, ändern Sie den `Application_Start`-Ereignishandler.

1. Suchen Sie in **Projektmappen-Explorer**von Visual Studio die Datei *Global.asax.cs* , und öffnen Sie Sie.
2. Fügen Sie den in gelb markierten Code wie folgt der *Global.asax.cs* -Datei hinzu:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wenn die Wingtip Toys-Beispielanwendung gestartet wird, wird der `Application_Start`-Ereignishandler aufgerufen. Am Ende dieses Ereignis Handlers wird die `RegisterCustomRoutes`-Methode aufgerufen. Die `RegisterCustomRoutes`-Methode fügt jede Route hinzu, indem Sie die `MapPageRoute`-Methode des `RouteCollection`-Objekts aufrufen. Routen werden mithilfe eines Routen namens, einer Routen-URL und einer physischen URL definiert.

Der erste Parameter ("`ProductsByCategoryRoute`") ist der Routen Name. Sie wird verwendet, um die Route aufzurufen, wenn Sie benötigt wird. Der zweite Parameter ("`Category/{categoryName}`") definiert die benutzerfreundliche Ersetzungs-URL, die basierend auf dem Code dynamisch sein kann. Diese Route wird verwendet, wenn Sie ein Daten Steuerelement mit Links auffüllen, die auf der Grundlage von Daten generiert werden. Eine Route wird wie folgt angezeigt:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Der zweite Parameter der Route enthält einen dynamischen Wert, der durch geschweifte Klammern (`{ }`) angegeben wird. In diesem Fall ist der `categoryName` eine Variable, mit der der richtige Routing Pfad bestimmt wird.

> [!NOTE] 
> 
> **Optional**
> 
> Sie finden es möglicherweise einfacher, den Code zu verwalten, indem Sie die `RegisterCustomRoutes`-Methode in eine separate Klasse verschieben. Erstellen Sie im Ordner " *Logic* " eine separate `RouteActions`-Klasse. Verschieben Sie die obige `RegisterCustomRoutes`-Methode aus der *Global.asax.cs* -Datei in die neue `RoutesActions`-Klasse. Verwenden Sie die `RoleActions`-Klasse und die `createAdmin`-Methode als Beispiel dafür, wie Sie die `RegisterCustomRoutes`-Methode aus der *Global.asax.cs* -Datei aufzurufen.

Möglicherweise haben Sie auch den `RegisterRoutes` Methoden Aufrufes mithilfe des `RouteConfig`-Objekts am Anfang des `Application_Start` Ereignis Handlers bemerkt. Dieser aufgerufen wird, um das Standard Routing zu implementieren. Sie wurde als Standard Code eingeschlossen, als Sie die Anwendung mit der Web Forms-Vorlage von Visual Studio erstellt haben.

## <a name="retrieving-and-using-route-data"></a>Abrufen und Verwenden von Routendaten

Wie bereits erwähnt, können Routen definiert werden. Der Code, den Sie dem `Application_Start`-Ereignishandler in der *Global.asax.cs* -Datei hinzugefügt haben, lädt die definierbaren Routen.

### <a name="setting-routes"></a>Festlegen von Routen

Für Routen müssen Sie zusätzlichen Code hinzufügen. In diesem Tutorial verwenden Sie die Modell Bindung, um ein `RouteValueDictionary` Objekt abzurufen, das beim Erstellen der Routen mithilfe von Daten aus einem Daten Steuerelement verwendet wird. Das `RouteValueDictionary`-Objekt enthält eine Liste von Produktnamen, die zu einer bestimmten Produktkategorie gehören. Ein Link wird für jedes Produkt basierend auf den Daten und der Route erstellt.

#### <a name="enable-routes-for-categories-and-products"></a>Aktivieren von Routen für Kategorien und Produkte

Als nächstes aktualisieren Sie die Anwendung so, dass Sie die `ProductsByCategoryRoute` verwendet, um die richtige Route für die einzelnen Product Category-Links zu ermitteln. Außerdem aktualisieren Sie die Seite " *productList. aspx* " so, dass ein Routing Link für jedes Produkt enthalten ist. Die Links werden wie vor der Änderung angezeigt, aber die Links verwenden jetzt das URL-Routing.

1. Öffnen Sie in **Projektmappen-Explorer**die Seite *Site. Master* , wenn Sie nicht bereits geöffnet ist.
2. Aktualisieren Sie das **ListView** -Steuerelement mit dem Namen "`categoryList`" mit den in gelb markierten Änderungen, sodass das Markup wie folgt aussieht:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Öffnen Sie in **Projektmappen-Explorer**die Seite *productList. aspx* .
4. Aktualisieren Sie das `ItemTemplate`-Element der *productList. aspx* -Seite mit den in gelb markierten Updates, sodass das Markup wie folgt aussieht:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Öffnen Sie die Code Behind- *productList.aspx.cs* , und fügen Sie den folgenden Namespace hinzu, wie in gelb hervorgehoben:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Ersetzen Sie die `GetProducts`-Methode des Code-Behind (*productList.aspx.cs*) durch den folgenden Code:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Hinzufügen von Code für Produkt Details

Aktualisieren Sie nun den Code Behind (*ProductDetails.aspx.cs*) für die Seite *ProductDetails. aspx* , um Routendaten zu verwenden. Beachten Sie, dass die neue `GetProduct`-Methode auch einen Abfrage Zeichenfolgen-Wert für den Fall akzeptiert, in dem der Benutzer über ein Lesezeichen verfügt, das die ältere nicht-freundliche, nicht-weitergeleitete URL verwendet.

1. Ersetzen Sie die `GetProduct`-Methode des Code-Behind (*ProductDetails.aspx.cs*) durch den folgenden Code:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen, um die aktualisierten Routen anzuzeigen.

1. Drücken Sie **F5** , um die Beispielanwendung Wingtip Toys auszuführen.  
 Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.
2. Klicken Sie oben auf der Seite auf den Link **Produkte** .  
 Alle Produkte werden auf der Seite " *productList. aspx* " angezeigt. Die folgende URL (unter Verwendung ihrer Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/ProductList`
3. Klicken Sie dann im oberen **Bereich der Seite auf die** kategoriekategorie.  
 Nur Fahrzeuge werden auf der Seite " *productList. aspx* " angezeigt. Die folgende URL (unter Verwendung ihrer Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/Category/Cars`
4. Klicken Sie auf den Link mit dem Namen des ersten Fahrzeugs, das auf der Seite ("**konvertierbar**") aufgeführt ist, um die Produktdetails anzuzeigen.  
 Die folgende URL (unter Verwendung ihrer Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Geben Sie als nächstes die folgende nicht weitergeleitete URL (unter Verwendung ihrer Portnummer) in den Browser ein:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Der Code erkennt weiterhin eine URL, die eine Abfrage Zeichenfolge enthält, wenn ein Benutzer einen Link mit Lesezeichen aufweist.

## <a name="summary"></a>Summary

In diesem Tutorial haben Sie Routen für Kategorien und Produkte hinzugefügt. Sie haben gelernt, wie Routen in Daten Steuerelemente integriert werden können, die die Modell Bindung verwenden. Im nächsten Tutorial implementieren Sie die globale Fehlerbehandlung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.net friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Stellen Sie eine sichere ASP.net Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank bereit, um Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Kostenlose Testversion Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Zurück](membership-and-administration.md)
> [Weiter](aspnet-error-handling.md)
