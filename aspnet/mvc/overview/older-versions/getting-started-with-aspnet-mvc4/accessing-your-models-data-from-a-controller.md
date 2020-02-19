---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Zugreifen auf die Daten des Modells von einem Controller | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456165"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Zugreifen auf Modelldaten anhand eines Controllers

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.

In diesem Abschnitt erstellen Sie eine neue `MoviesController`-Klasse und schreiben Code, mit dem die Filmdaten abgerufen und im Browser mithilfe einer Ansichts Vorlage angezeigt werden.

**Erstellen Sie die Anwendung** , und fahren Sie mit dem nächsten Schritt fort.

Klicken Sie mit der rechten Maustaste auf den Ordner *Controllers* , und erstellen Sie einen neuen `MoviesController` Controller. Die folgenden Optionen werden erst angezeigt, wenn Sie die Anwendung erstellen. Wählen Sie die folgenden Optionen:

- Controller Name: **moviescontroller**. (Dies ist die Standardeinstellung. )
- Vorlage: **MVC-Controller mit Lese-/Schreibaktionen und Ansichten unter Verwendung Entity Framework**.
- Modell Klasse: **Movie (mvcmovie. Models)** .
- Datenkontext Klasse: " **wviedbcontext" (mvcmovie. Models)** .
- Views: **Razor (cshtml)** . (Die Standardeinstellung.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Klicken Sie auf **Hinzufügen**. Visual Studio Express erstellt die folgenden Dateien und Ordner:

- *Eine MoviesController.cs* -Datei im Ordner " *Controllers* " des Projekts.
- Einen Ordner " *Movies* " im Ordner " *views* " des Projekts.
- " *Create. cshtml", "Delete. cshtml", "Details. cshtml", "Edit. cshtml*" und " *Index. cshtml* " im neuen Ordner " *views\movies* ".

ASP.NET MVC 4 erstellte automatisch die CRUD-Aktionsmethoden und-Sichten (Create, Read, Update und DELETE) für Sie (die automatische Erstellung von CRUD-Aktionsmethoden und-Sichten wird als Gerüstbau bezeichnet). Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit der Sie Film Einträge erstellen, auflisten, bearbeiten und löschen können.

Führen Sie die Anwendung aus, und navigieren Sie zum `Movies` Controller, indem Sie */Movies* an die URL in der Adressleiste Ihres Browsers anhängen. Da sich die Anwendung auf das Standard Routing (definiert in der Datei *Global. asax* ) verlässt, wird der `http://localhost:xxxxx/Movies` Browser Anforderung an die standardmäßige `Index` Aktionsmethode des `Movies` Controllers weitergeleitet. Das heißt, die Browser Anforderung `http://localhost:xxxxx/Movies` ist praktisch identisch mit der Browser Anforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Erstellen eines Films

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details zu einem Film ein, und klicken Sie dann auf die Schaltfläche **Erstellen** .

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Wenn Sie auf die Schaltfläche **Erstellen** klicken, wird das Formular an den Server gesendet, auf dem die Filminformationen in der Datenbank gespeichert werden. Sie werden dann zur URL */Movies* umgeleitet, auf der Sie den neu erstellten Film in der Auflistung sehen können.

![Index/harrymet](accessing-your-models-data-from-a-controller/_static/image4.png "Index/harrymet")

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Untersuchen des generierten Codes

Öffnen Sie die Datei *controllers\moviescontroller.cs* , und untersuchen Sie die generierte `Index`-Methode. Unten wird ein Teil des Movie-Controllers mit der `Index`-Methode angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Die folgende Zeile aus der `MoviesController`-Klasse instanziiert einen Film Daten Bank Kontext, wie zuvor beschrieben. Sie können den Film Daten Bank Kontext verwenden, um Filme abzufragen, zu bearbeiten und zu löschen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Eine Anforderung an den `Movies` Controller gibt alle Einträge in der `Movies`-Tabelle der Movie-Datenbank zurück und übergibt die Ergebnisse dann an die `Index` Sicht.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und das @model-Schlüsselwort

An früherer Stelle in diesem Tutorial haben Sie erfahren, wie ein Controllerdaten oder Objekte mithilfe des `ViewBag` Objekts an eine Ansichts Vorlage übergeben kann. Der `ViewBag` ist ein dynamisches Objekt, das eine bequeme, spät gebundene Methode zum Übergeben von Informationen an eine Ansicht bereitstellt.

ASP.NET MVC bietet auch die Möglichkeit, stark typisierte Daten oder Objekte an eine Ansichts Vorlage zu übergeben. Dieser stark typisierte Ansatz ermöglicht eine bessere Kompilierzeit Überprüfung Ihres Codes und umfangreicheres IntelliSense im Visual Studio-Editor. Der Gerüstbau Mechanismus in Visual Studio nutzte diesen Ansatz mit der `MoviesController`-Klasse und Anzeige Vorlagen, als die Methoden und Sichten erstellt wurden.

Untersuchen Sie in der Datei " *controllers\moviescontroller.cs* " die generierte `Details`-Methode. Unten wird ein Teil des Movie-Controllers mit der `Details`-Methode angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Wenn eine `Movie` gefunden wird, wird eine Instanz des `Movie` Modells an die Detailansicht übermittelt. Überprüfen Sie den Inhalt der Datei " *views\movies\details.cshtml* ".

Durch Einschließen einer `@model`-Anweisung am Anfang der Ansichts Vorlagen Datei können Sie den Objekttyp angeben, den die Sicht erwartet. Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir. Beispielsweise übergibt der Code in der " *Details. cshtml* "-Vorlage jedes filmfeld an den `DisplayNameFor`-und [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) -HTML-Hilfsprogramm mit dem stark typisierten `Model`-Objekt. Die Methoden zum Erstellen und Bearbeiten von Methoden und Ansichten übergeben auch ein Movie Model-Objekt.

Überprüfen Sie die Ansichts Vorlage *Index. cshtml* und die `Index`-Methode in der Datei *MoviesController.cs* . Beachten Sie, wie der Code beim Aufrufen der `View` Helper-Methode in der `Index` Action-Methode ein [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) -Objekt erstellt. Der Code übergibt dann diese `Movies` Liste vom Controller an die Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Beim Erstellen des Movie Controllers haben Visual Studio Express automatisch die folgende `@model` Anweisung am Anfang der Datei " *Index. cshtml* " eingefügt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Diese `@model` Direktive ermöglicht Ihnen den Zugriff auf die Liste der Filme, die der Controller an die Ansicht übermittelt hat, indem er ein `Model` Objekt verwendet, das stark typisiert ist. In der Vorlage *Index. cshtml* durchläuft der Code z. b. die Filme durch Ausführen einer `foreach`-Anweisung über das stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Da das `Model` Objekt stark typisiert ist (als `IEnumerable<Movie>`-Objekt), wird jedes `item`-Objekt in der Schleife als `Movie`typisiert. Dies bedeutet neben anderen Vorteilen, dass Sie die Kompilierzeit Überprüfung des Codes und die vollständige IntelliSense-Unterstützung im Code-Editor erhalten:

![Modelintellisense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Arbeiten mit SQL Server LocalDB

Entity Framework Code First festgestellt, dass die angegebene Daten bankverbindungs Zeichenfolge auf eine `Movies` Datenbank verweist, die noch nicht vorhanden war, sodass Code First die Datenbank automatisch erstellt hat. Sie können überprüfen, ob er erstellt wurde, indem Sie im Ordner *App\_Data* Wenn die Datei *Movies. mdf* nicht angezeigt wird, klicken Sie auf der **Projektmappen-Explorer** Symbolleiste auf die Schaltfläche **alle Dateien anzeigen** , klicken Sie auf die Schaltfläche **Aktualisieren** , und erweitern Sie dann den Ordner *App\_Data* .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Doppelklicken Sie auf *Movies. mdf* , um den **Datenbank-Explorer**zu öffnen, und erweitern Sie dann den Ordner **Tabellen** , um die Tabelle Movies anzuzeigen.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Wenn der Datenbank-Explorer nicht angezeigt wird, wählen Sie **im Menü Extras die Option** **mit Datenbank verbinden**aus, und klicken Sie dann auf das Dialogfeld **Datenquelle auswählen** . Dadurch wird das Öffnen des Datenbank-Explorers erzwungen.

> [!NOTE]
> Wenn Sie VWD oder Visual Studio 2010 verwenden, erhalten Sie eine Fehlermeldung wie die folgende:
> 
> - Die Datenbank "c:\webs\mvc4\mvcmovie\mvcmovie\app\_data\movies. MDF ' kann nicht geöffnet werden, da es Version 706 ist. Dieser Server unterstützt Version 655 und früher. Ein Herabstufungspfad wird nicht unterstützt.
> - &quot;InvalidOperation-Ausnahme wurde vom Benutzercode nicht behandelt,&quot; die angegebene SqlConnection keinen Anfangs Katalog angibt.
> 
> Sie müssen die [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) und [localdb](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)installieren. Überprüfen Sie die auf der vorherigen Seite angegebene `MovieDBContext` Verbindungs Zeichenfolge.

Klicken Sie mit der rechten Maustaste auf die Tabelle `Movies`, und wählen Sie **Tabellendaten anzeigen** aus, um die erstellten Daten anzuzeigen.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle `Movies`, und wählen Sie **Tabellen Definition öffnen** aus, um die Tabellenstruktur anzuzeigen, die Entity Framework für Sie erstellt Code First.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Beachten Sie, dass das Schema der `Movies` Tabelle der `Movie` Klasse zugeordnet ist, die Sie zuvor erstellt haben. Entity Framework Code First dieses Schema basierend auf Ihrer `Movie`-Klasse automatisch für Sie erstellt.

Wenn Sie fertig sind, schließen Sie die Verbindung, indem Sie mit der rechten Maustaste auf " *muviedbcontext* " klicken und **Verbindung schließen**auswählen (Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie das Projekt das nächste Mal ausführen).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Sie verfügen jetzt über die Datenbank und eine einfache Auflistungs Seite, um Inhalt anzuzeigen. Im nächsten Tutorial untersuchen wir den restlichen Code und fügen eine `SearchIndex` Methode und eine `SearchIndex` Ansicht hinzu, mit der Sie in dieser Datenbank nach Filmen suchen können.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)
