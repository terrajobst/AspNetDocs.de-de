---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Erstellen von Hilfeseiten für ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: In diesem Tutorial mit Code wird gezeigt, wie Sie Hilfeseiten für ASP.net-Web-API in ASP.NET 4. x erstellen.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448617"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a>Erstellen von Hilfeseiten für ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Tutorial mit Code wird gezeigt, wie Sie Hilfeseiten für ASP.net-Web-API in ASP.NET 4. x erstellen.

Wenn Sie eine Web-API erstellen, ist es oft hilfreich, eine Hilfeseite zu erstellen, damit andere Entwickler wissen, wie Sie Ihre API aufzurufen. Sie könnten die gesamte Dokumentation manuell erstellen, aber es ist besser, so weit wie möglich automatisch zu generieren. Um diese Aufgabe zu vereinfachen, stellt ASP.net-Web-API eine Bibliothek für die automatische Erstellung von Hilfeseiten zur Laufzeit bereit.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Erstellen von API-Hilfeseiten

Installieren Sie [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=282650). Mit diesem Update werden Hilfeseiten in die Web-API-Projektvorlage integriert.

Erstellen Sie als nächstes ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage aus. Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`. Die Vorlage erstellt außerdem die API-Hilfeseiten. Alle Code Dateien für die Hilfeseite werden im Ordner "Bereiche" des Projekts platziert.

![](creating-api-help-pages/_static/image2.png)

Wenn Sie die Anwendung ausführen, enthält die Startseite einen Link zur API-Hilfeseite. Auf der Startseite lautet der relative Pfad/Help.

![](creating-api-help-pages/_static/image3.png)

Über diesen Link gelangen Sie zu einer API-Zusammenfassungs Seite.

![](creating-api-help-pages/_static/image4.png)

Die MVC-Ansicht für diese Seite wird in Bereiche/helppage/views/Help/Index. cshtml definiert. Sie können diese Seite bearbeiten, um Layout, Einführung, Titel, Stile usw. zu ändern.

Der Hauptteil der Seite ist eine Tabelle mit APIs, gruppiert nach Controller. Die Tabelleneinträge werden mithilfe der **iapiexplorer** -Schnittstelle dynamisch generiert. (Ich werde später noch mehr über diese Schnittstelle sprechen.) Wenn Sie einen neuen API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert.

In der Spalte "API" sind die HTTP-Methode und der relative URI aufgeführt. Die Spalte "Beschreibung" enthält die Dokumentation für jede API. Zunächst ist die Dokumentation nur Platzhalter Text. Im nächsten Abschnitt zeige ich Ihnen, wie Sie Dokumentationen aus XML-Kommentaren hinzufügen.

Jede API verfügt über einen Link zu einer Seite mit detaillierteren Informationen, wie z. b. Anforderungs-und Antwort Texte.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Hinzufügen von Hilfeseiten zu einem vorhandenen Projekt

Mit dem nuget-Paket-Manager können Sie einem vorhandenen Web-API-Projekt Hilfeseiten hinzufügen. Diese Option ist nützlich, wenn Sie von einer anderen Projektvorlage als der Vorlage "Web-API" starten.

Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster der [Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) einen der folgenden Befehle ein:

Für eine **C#** -Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Für eine **Visual Basic** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Es gibt zwei Pakete, eine für C# und eine für Visual Basic. Stellen Sie sicher, dass Sie die Anwendung verwenden, die dem Projekt entspricht.

Mit diesem Befehl werden die erforderlichen Assemblys installiert, und die MVC-Ansichten für die Hilfeseiten (im Ordner "Bereiche/helppage") werden hinzugefügt. Sie müssen der Hilfeseite manuell einen Link hinzufügen. Der URI ist/Help. Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Stellen Sie außerdem sicher, dass Sie die Bereiche registrieren. Fügen Sie in der Datei "Global. asax" den folgenden Code in die **Anwendungs\_Start** -Methode ein, sofern diese nicht bereits vorhanden ist:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>API-Dokumentation hinzufügen

Standardmäßig verfügen die Hilfeseiten über Platzhalter Zeichenfolgen für die Dokumentation. Sie können [XML-Dokumentations Kommentare](https://msdn.microsoft.com/library/b2s063f7.aspx) verwenden, um die Dokumentation zu erstellen. Um dieses Feature zu aktivieren, öffnen Sie die Datei Bereiche/helppage/App\_Start/helppageconfig. cs, und heben Sie die Auskommentierung der folgenden Zeile auf:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Aktivieren Sie nun die XML-Dokumentation. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**aus. Wählen Sie die Seite **Erstellen** aus.

![](creating-api-help-pages/_static/image6.png)

Überprüfen Sie unter **Ausgabe**die **XML-Dokumentations Datei**. Geben Sie im Bearbeitungsfeld "App\_Data/XmlDocument. xml" ein.

![](creating-api-help-pages/_static/image7.png)

Öffnen Sie als nächstes den Code für den `ValuesController` API-Controller, der in/Controllers/ValuesController.cs. definiert ist. Fügen Sie den Controller Methoden einige Dokumentations Kommentare hinzu. Beispiel:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Tipp: Wenn Sie die Einfügemarke in der Zeile oberhalb der Methode positionieren und drei Schrägstriche eingeben, fügt Visual Studio automatisch die XML-Elemente ein. Anschließend können Sie die Leerzeichen ausfüllen.

Erstellen Sie jetzt die Anwendung, und führen Sie Sie erneut aus, und navigieren Sie zu den Hilfeseiten. Die Dokumentations Zeichenfolgen sollten in der API-Tabelle angezeigt werden.

![](creating-api-help-pages/_static/image8.png)

Die Hilfeseite liest die Zeichen folgen aus der XML-Datei zur Laufzeit. (Stellen Sie beim Bereitstellen der Anwendung sicher, dass Sie die XML-Datei bereitstellen.)

## <a name="under-the-hood"></a>Im Hintergrund

Die Hilfeseiten basieren auf der **apiexplorer** -Klasse, die Teil des Web-API-Frameworks ist. Die **apiexplorer** -Klasse stellt das Rohmaterial zum Erstellen einer Hilfeseite bereit. **Apiexplorer** enthält für jede API eine **apideabo-App** , die die API beschreibt. Zu diesem Zweck wird eine "API" als Kombination aus HTTP-Methode und relativer URI definiert. Hier sind z. b. einige unterschiedliche APIs:

- Get/API/Products
- Get/API/Products/{ID}
- Nach/API/Products

Wenn eine Controller Aktion mehrere HTTP-Methoden unterstützt, behandelt der **apiexplorer** jede Methode als eine eindeutige API.

Um eine API aus dem **apiexplorer**auszublenden, fügen Sie der Aktion das Attribut **apiexplorersettings** hinzu, und legen Sie *ignoreapi* auf true fest.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Sie können dieses Attribut auch dem Controller hinzufügen, um den gesamten Controller auszuschließen.

Die apiexplorer-Klasse ruft Dokumentations Zeichenfolgen von der **idocumentationprovider** -Schnittstelle ab. Wie Sie bereits gesehen haben, bietet die Hilfeseiten Bibliothek einen **idocumentationprovider** , der die Dokumentation aus XML-Dokumentations Zeichenfolgen abruft. Der Code befindet sich in/Areas/HelpPage/XmlDocumentationProvider.cs. Sie können die Dokumentation aus einer anderen Quelle abrufen, indem Sie Ihren eigenen **idocumentationprovider**schreiben. Um es zu verknüpfen, wenden Sie die in **helppageconfigurationextensions** definierte **setdocumentationprovider** -Erweiterungsmethode an.

**Apiexplorer** ruft automatisch die **idocumentationprovider** -Schnittstelle auf, um Dokumentations Zeichenfolgen für jede API zu erhalten. Sie speichert Sie in der- **Dokumentations** Eigenschaft der Objekte **apide-** und **apiparameterdescription** .

## <a name="next-steps"></a>Nächste Schritte

Sie sind nicht auf die hier gezeigten Hilfeseiten beschränkt. Tatsächlich ist **apiexplorer** nicht auf das Erstellen von Hilfeseiten beschränkt. Yao Huang Lin hat einige tolle Blogbeiträge verfasst, um Ihnen die folgenden Vorteile zu bringen:

- [Hinzufügen eines einfachen Test Clients zur ASP.net-Web-API Hilfeseite](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Erstellen ASP.net-Web-API Hilfeseite in selbstgeh osteten Diensten](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Entwurfszeit Generierung der Hilfeseite (oder des Clients) für ASP.net-Web-API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Erweiterte Anpassungen der Hilfeseite](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
