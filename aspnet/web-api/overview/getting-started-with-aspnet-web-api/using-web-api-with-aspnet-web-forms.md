---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Verwenden der Web-API mit ASP.net Web Forms-ASP.NET 4. x
author: MikeWasson
description: Tutorial mit Code Schritt für Schritt zum Hinzufügen einer Web-API zu einer ASP.net Forms-Anwendung für ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448533"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Verwenden der Web-API mit ASP.NET Web Forms

von [Mike Wasson](https://github.com/MikeWasson)

Dieses Tutorial führt Sie durch die Schritte zum Hinzufügen einer Web-API zu einer herkömmlichen ASP.net Web Forms-Anwendung in ASP.NET 4. x. 

## <a name="overview"></a>Übersicht

Obwohl ASP.net-Web-API mit ASP.NET MVC verpackt ist, ist es einfach, eine Web-API zu einer herkömmlichen ASP.net Web Forms-Anwendung hinzuzufügen.

Um die Web-API in einer Web Forms Anwendung zu verwenden, gibt es zwei Hauptschritte:

- Fügt einen Web-API-Controller hinzu, der von der **apicontroller** -Klasse abgeleitet wird.
- Fügen Sie der **Anwendungs\_Start** -Methode eine Routentabelle hinzu.

## <a name="create-a-web-forms-project"></a>Erstellen eines Web Forms Projekts

Starten Sie Visual Studio, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.net Web Forms Anwendung**aus. Geben Sie einen Namen für das Projekt ein, und klicken Sie auf **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Erstellen des Modells und des Controllers

In diesem Tutorial werden die gleichen Modell-und Controller Klassen wie im Tutorial " [Getting Started](tutorial-your-first-web-api.md) " verwendet.

Fügen Sie zunächst eine Modell Klasse hinzu. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**. Benennen Sie die Klasse Product, und fügen Sie die folgende Implementierung ein:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Als Nächstes fügen Sie dem Projekt einen Web-API-Controller hinzu. ein *Controller* ist das Objekt, das HTTP-Anforderungen für die Web-API verarbeitet.

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt. Wählen Sie **Neues Element hinzufügen**aus.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Erweitern Sie unter **installierte Vorlagen**die Option **Visualisierung C#**  , und wählen Sie **Web**aus. Wählen Sie dann in der Liste der Vorlagen die **Web-API-Controller Klasse**aus. Nennen Sie den Controller "ProductController", und klicken Sie auf **Hinzufügen**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

Mit dem Assistenten zum **Hinzufügen eines neuen Elements** wird eine Datei namens ProductsController.cs erstellt. Löschen Sie die Methoden, die der Assistent enthielt, und fügen Sie die folgenden Methoden hinzu:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Weitere Informationen zum Code in diesem Controller finden Sie im Tutorial [zu](tutorial-your-first-web-api.md) den ersten Schritten.

## <a name="add-routing-information"></a>Routing Informationen hinzufügen

Als Nächstes fügen wir eine URI-Route hinzu, sodass URIs der Form &quot;/API/Products/-&quot; an den Controller weitergeleitet werden.

Doppelklicken Sie in **Projektmappen-Explorer**auf Global. asax, um die Code Behind-Datei Global.asax.cs zu öffnen. Fügen Sie die folgende **using** -Anweisung hinzu.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Fügen Sie dann dem **Anwendungs\_Start** -Methode den folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Weitere Informationen zu Routing Tabellen finden Sie unter [Routing in ASP.net-Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Client seitiges AJAX hinzufügen

Das ist alles, was Sie benötigen, um eine Web-API zu erstellen, auf die Clients zugreifen können. Nun fügen wir eine HTML-Seite hinzu, die jQuery verwendet, um die API aufzurufen.

Stellen Sie sicher, dass Ihre Master Seite (z *. b. Site. Master*) eine `ContentPlaceHolder` mit `ID="HeadContent"`enthält:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Öffnen Sie die Datei default. aspx. Ersetzen Sie den Text, der sich im Hauptinhalts Abschnitt befindet, wie hier gezeigt:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Fügen Sie als nächstes im Abschnitt `HeaderContent` einen Verweis auf die jQuery-Quelldatei hinzu:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Hinweis: Sie können den Skript Verweis problemlos hinzufügen, indem Sie die Datei per Drag & Drop aus **Projektmappen-Explorer** in das Fenster "Code-Editor" verschieben.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Fügen Sie unterhalb des jQuery-Skripttags den folgenden Skriptblock hinzu:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Wenn das Dokument geladen wird, sendet dieses Skript eine AJAX-Anforderung an &quot;API/Produkte&quot;. Die Anforderung gibt eine Liste von Produkten im JSON-Format zurück. Das Skript fügt die Produktinformationen der HTML-Tabelle hinzu.

Wenn Sie die Anwendung ausführen, sollte Sie wie folgt aussehen:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
