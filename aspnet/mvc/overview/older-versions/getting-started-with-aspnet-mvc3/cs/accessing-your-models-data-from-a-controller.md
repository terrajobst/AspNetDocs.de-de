---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Zugreifen auf die Daten des Modells von einem ControllerC#() | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e9c7f24d426635760b613f570b85455ce71b3dc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506025"
---
# <a name="accessing-your-models-data-from-a-controller-c"></a>Zugreifen auf Modelldaten anhand eines Controllers (C#)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.
> 
> 
> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Für dieses Thema steht ein Visual C# Web Developer-Projekt mit Quellcode zur Verfügung. [Laden Sie C# die Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zur [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Tutorials.

In diesem Abschnitt erstellen Sie eine neue `MoviesController`-Klasse und schreiben Code, mit dem die Filmdaten abgerufen und im Browser mithilfe einer Ansichts Vorlage angezeigt werden. Stellen Sie sicher, dass Sie die Anwendung erstellen, bevor Sie fortfahren

Klicken Sie mit der rechten Maustaste auf den Ordner *Controllers* , und erstellen Sie einen neuen `MoviesController` Controller. Wählen Sie die folgenden Optionen:

- Controller Name: **moviescontroller**. (Dies ist die Standardeinstellung. )
- Vorlage: **Controller mit Lese-/Schreibaktionen und Ansichten unter Verwendung Entity Framework**.
- Modell Klasse: **Movie (mvcmovie. Models)** .
- Datenkontext Klasse: " **wviedbcontext" (mvcmovie. Models)** .
- Views: **Razor (cshtml)** . (Die Standardeinstellung.)

[![Addgerüoldedmuviecontroller](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Klicken Sie auf **Hinzufügen**. Visual Web Developer erstellt die folgenden Dateien und Ordner:

- *Eine MoviesController.cs* -Datei im Ordner " *Controllers* " des Projekts.
- Einen Ordner " *Movies* " im Ordner " *views* " des Projekts.
- " *Create. cshtml", "Delete. cshtml", "Details. cshtml", "Edit. cshtml*" und " *Index. cshtml* " im neuen Ordner " *views\movies* ".

[![Newmuviecontrollerscreenshot](accessing-your-models-data-from-a-controller/_static/image4.png "Newmuviecontrollerscreenshot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Der ASP.NET MVC 3-gerüstingmechanismus erstellte automatisch die CRUD-Aktionsmethoden und-Sichten (Create, Read, Update und DELETE) für Sie. Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit der Sie Film Einträge erstellen, auflisten, bearbeiten und löschen können.

Führen Sie die Anwendung aus, und navigieren Sie zum `Movies` Controller, indem Sie */Movies* an die URL in der Adressleiste Ihres Browsers anhängen. Da sich die Anwendung auf das Standard Routing (definiert in der Datei *Global. asax* ) verlässt, wird der `http://localhost:xxxxx/Movies` Browser Anforderung an die standardmäßige `Index` Aktionsmethode des `Movies` Controllers weitergeleitet. Das heißt, die Browser Anforderung `http://localhost:xxxxx/Movies` ist praktisch identisch mit der Browser Anforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Erstellen eines Films

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details zu einem Film ein, und klicken Sie dann auf die Schaltfläche **Erstellen** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Wenn Sie auf die Schaltfläche **Erstellen** klicken, wird das Formular an den Server gesendet, auf dem die Filminformationen in der Datenbank gespeichert werden. Sie werden dann zur URL */Movies* umgeleitet, auf der Sie den neu erstellten Film in der Auflistung sehen können.

[![Index/harrymet](accessing-your-models-data-from-a-controller/_static/image8.png "Index/harrymet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Untersuchen des generierten Codes

Öffnen Sie die Datei *controllers\moviescontroller.cs* , und untersuchen Sie die generierte `Index`-Methode. Unten wird ein Teil des Movie-Controllers mit der `Index`-Methode angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Die folgende Zeile aus der `MoviesController`-Klasse instanziiert einen Film Daten Bank Kontext, wie zuvor beschrieben. Sie können den Film Daten Bank Kontext verwenden, um Filme abzufragen, zu bearbeiten und zu löschen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Eine Anforderung an den `Movies` Controller gibt alle Einträge in der `Movies`-Tabelle der Movie-Datenbank zurück und übergibt die Ergebnisse dann an die `Index` Sicht.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und das @model-Schlüsselwort

An früherer Stelle in diesem Tutorial haben Sie erfahren, wie ein Controllerdaten oder Objekte mithilfe des `ViewBag` Objekts an eine Ansichts Vorlage übergeben kann. Der `ViewBag` ist ein dynamisches Objekt, das eine bequeme, spät gebundene Methode zum Übergeben von Informationen an eine Ansicht bereitstellt.

ASP.NET MVC bietet auch die Möglichkeit, stark typisierte Daten oder Objekte an eine Ansichts Vorlage zu übergeben. Dieser stark typisierte Ansatz ermöglicht eine bessere Kompilierzeit Überprüfung Ihres Codes und umfassendere IntelliSense im Visual Web Developer-Editor. Wir verwenden diesen Ansatz mit der Ansichts Vorlage "`MoviesController`-Klasse" und " *Index. cshtml* ".

Beachten Sie, wie der Code beim Aufrufen der `View` Helper-Methode in der `Index` Action-Methode ein [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) -Objekt erstellt. Der Code übergibt dann diese `Movies` Liste vom Controller an die Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Durch Einschließen einer `@model`-Anweisung am Anfang der Ansichts Vorlagen Datei können Sie den Objekttyp angeben, den die Sicht erwartet. Beim Erstellen des Film Controllers enthielt Visual Web Developer automatisch die folgende `@model`-Anweisung am Anfang der Datei " *Index. cshtml* ":

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Diese `@model` Direktive ermöglicht Ihnen den Zugriff auf die Liste der Filme, die der Controller an die Ansicht übermittelt hat, indem er ein `Model` Objekt verwendet, das stark typisiert ist. In der Vorlage *Index. cshtml* durchläuft der Code z. b. die Filme durch Ausführen einer `foreach`-Anweisung über das stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Da das `Model` Objekt stark typisiert ist (als `IEnumerable<Movie>`-Objekt), wird jedes `item`-Objekt in der Schleife als `Movie`typisiert. Dies bedeutet neben anderen Vorteilen, dass Sie die Kompilierzeit Überprüfung des Codes und die vollständige IntelliSense-Unterstützung im Code-Editor erhalten:

[![Modelintellisense](accessing-your-models-data-from-a-controller/_static/image10.png "Modelintellisense")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Arbeiten mit SQL Server Compact

Entity Framework Code First festgestellt, dass die angegebene Daten bankverbindungs Zeichenfolge auf eine `Movies` Datenbank verweist, die noch nicht vorhanden war, sodass Code First die Datenbank automatisch erstellt hat. Sie können überprüfen, ob er erstellt wurde, indem Sie im Ordner *App\_Data* Wenn die Datei *Movies. sdf* nicht angezeigt wird, klicken Sie auf der **Projektmappen-Explorer** Symbolleiste auf die Schaltfläche **alle Dateien anzeigen** , klicken Sie auf die Schaltfläche **Aktualisieren** , und erweitern Sie dann den Ordner *App\_Data* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Doppelklicken Sie auf " *Movies. sdf* ", um **Server-Explorer**zu öffnen. Erweitern Sie dann den Ordner **Tabellen** , um die Tabellen anzuzeigen, die in der Datenbank erstellt wurden.

> [!NOTE]
> Wenn Sie beim Doppelklicken auf *Filme. sdf*eine Fehlermeldung erhalten, stellen Sie sicher, dass Sie [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Runtime + Tools-Unterstützung) installiert haben. (Links zur Software finden Sie in der Liste der Voraussetzungen in Teil 1 dieser tutorialreihe.) Wenn Sie jetzt das Release installieren, müssen Sie Visual Web Developer schließen und erneut öffnen.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Es gibt zwei Tabellen, eine für die `Movie` Entitätenmenge und dann die `EdmMetadata` Tabelle. Die `EdmMetadata` Tabelle wird vom Entity Framework verwendet, um zu bestimmen, wann das Modell und die Datenbank nicht synchronisiert sind.

Klicken Sie mit der rechten Maustaste auf die Tabelle `Movies`, und wählen Sie **Tabellendaten anzeigen** aus, um die erstellten Daten anzuzeigen.

[!["Moviestable"](accessing-your-models-data-from-a-controller/_static/image16.png ""Moviestable"")](accessing-your-models-data-from-a-controller/_static/image15.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle `Movies`, und wählen Sie **Tabellen Schema bearbeiten**.

[![Edittableschema](accessing-your-models-data-from-a-controller/_static/image18.png "Edittableschema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![Tableschemasm](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Beachten Sie, dass das Schema der `Movies` Tabelle der `Movie` Klasse zugeordnet ist, die Sie zuvor erstellt haben. Entity Framework Code First dieses Schema basierend auf Ihrer `Movie`-Klasse automatisch für Sie erstellt.

Wenn Sie fertig sind, schließen Sie die Verbindung. (Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie das Projekt das nächste Mal ausführen).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Sie verfügen jetzt über die Datenbank und eine einfache Auflistungs Seite, um Inhalt anzuzeigen. Im nächsten Tutorial untersuchen wir den restlichen Code und fügen eine `SearchIndex` Methode und eine `SearchIndex` Ansicht hinzu, mit der Sie in dieser Datenbank nach Filmen suchen können.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)
