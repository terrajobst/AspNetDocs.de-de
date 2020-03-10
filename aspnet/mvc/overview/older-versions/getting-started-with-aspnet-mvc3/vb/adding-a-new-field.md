---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Hinzufügen eines neuen Felds zum Film Modell und zur Datenbanktabelle (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: b2b26b6009c55f02c8a4159bda839fe7aefea4c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434649"
---
# <a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Hinzufügen eines neuen Felds zum Modell und zur Datenbanktabelle eines Films (VB)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit VB.NET-Quellcode ist für dieses Thema verfügbar. [Laden Sie die VB.NET-Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wechseln Sie C#ggf. zur [ C# Version](../cs/adding-a-new-field.md) dieses Tutorials.

In diesem Abschnitt nehmen Sie einige Änderungen an den Modellklassen vor und erfahren, wie Sie das Datenbankschema aktualisieren können, sodass es den Modelländerungen entspricht.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Beginnen Sie, indem Sie der vorhandenen `Movie` Klasse eine neue `Rating`-Eigenschaft hinzufügen. Öffnen Sie die Datei *Movie.cs* , und fügen Sie die `Rating`-Eigenschaft wie folgt hinzu:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Die gesamte `Movie`-Klasse sieht nun wie der folgende Code aus:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Kompilieren Sie die Anwendung mithilfe des Menübefehls **Debug** &gt;**Build Movie** neu.

Nachdem Sie nun die `Model`-Klasse aktualisiert haben, müssen Sie auch die Ansichts Vorlagen *\views\movies\index.vbhtml* und *\views\movies\kreate.vbhtml* aktualisieren, um die neue `Rating`-Eigenschaft zu unterstützen.

Öffnen Sie die Datei "<em>\views\movies\index.vbhtml</em> ", und fügen Sie eine `<th>Rating</th>` Spaltenüberschrift direkt hinter der <strong>Preis</strong> Spalte ein. Fügen Sie dann am Ende der Vorlage eine `<td>` Spalte hinzu, um den `@item.Rating` Wert zu erzeugen. Im folgenden sehen Sie, wie die aktualisierte Vorlage " <em>Index. vbhtml</em> " aussieht:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Öffnen Sie als nächstes die Datei " *\views\movies\kreate.vbhtml* ", und fügen Sie das folgende Markup am Ende des Formulars ein. Dadurch wird ein Textfeld gerendert, sodass Sie beim Erstellen eines neuen Films eine Bewertung angeben können.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Verwalten von Modell-und Daten Bank Schema unterschieden

Sie haben nun den Anwendungscode aktualisiert, um die neue `Rating`-Eigenschaft zu unterstützen.

Führen Sie nun die Anwendung aus, und navigieren Sie zur URL */Movies* . Wenn Sie dies jedoch tun, wird der folgende Fehler angezeigt:

![](adding-a-new-field/_static/image1.png)

Dieser Fehler wird angezeigt, da die aktualisierte `Movie` Modell Klasse in der Anwendung sich nun von dem Schema der `Movie` Tabelle der vorhandenen Datenbank unterscheidet. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)

Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank zu erstellen, wie in diesem Tutorial bereits erwähnt, fügt Code First der Datenbank eine Tabelle hinzu, um zu verfolgen, ob das Schema der Datenbank mit den Modellklassen synchronisiert ist, von denen es generiert wurde. Wenn Sie nicht synchron sind, löst die Entity Framework einen Fehler aus. Dies vereinfacht das Auffinden von Problemen bei der Entwicklung, die Sie möglicherweise zur Laufzeit nur (aufgrund von unkomplizierter Fehlern) finden. Die Synchronisierungs Überprüfung bewirkt, dass die Fehlermeldung angezeigt wird, die Sie gerade gesehen haben.

Es gibt zwei Ansätze, um den Fehler zu beheben:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Diese Vorgehensweise ist sehr praktisch, wenn Sie eine aktive Entwicklung für eine Testdatenbank durchgeführt haben, da Sie das Modell und das Datenbankschema schnell weiterentwickeln können. Der Nachteil besteht jedoch darin, dass Sie vorhandene Daten in der Datenbank verlieren – damit Sie diesen Ansatz *nicht* für eine Produktionsdatenbank verwenden möchten.
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.

In diesem Tutorial verwenden wir den ersten Ansatz – Sie haben die Entity Framework Code First die Datenbank automatisch neu erstellen, wenn das Modell geändert wird.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Automatisches Neuerstellen der Datenbank bei Modelländerungen

Aktualisieren Sie die Anwendung, damit Code First die Datenbank jederzeit automatisch löscht und neu erstellt, wenn Sie das Modell für die Anwendung ändern.

> [!NOTE] 
> 
> **Warnung** Sie sollten diesen Ansatz zum automatischen Löschen und erneuten Erstellen der Datenbank nur dann aktivieren, wenn Sie eine Entwicklungs-oder Testdatenbank verwenden und *nie* in einer Produktionsdatenbank, die echte Daten enthält. Die Verwendung auf einem Produktionsserver kann zu Datenverlusten führen.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen**und dann **Klasse**aus.

![](adding-a-new-field/_static/image2.png)

Benennen Sie die Klasse &quot;"movieinitializer"&quot;. Aktualisieren Sie die `MovieInitializer`-Klasse so, dass Sie den folgenden Code enthält:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

Die `MovieInitializer`-Klasse gibt an, dass die vom Modell verwendete Datenbank gelöscht und automatisch neu erstellt werden soll, wenn sich die Modellklassen jemals ändern. Der Code enthält eine `Seed` Methode zum Angeben einiger Standarddaten, die automatisch der Datenbank hinzugefügt werden, wenn Sie erstellt (oder neu erstellt) werden. Dies bietet eine hilfreiche Möglichkeit, um die Datenbank mit einigen Beispiel Daten aufzufüllen, ohne dass Sie Sie jedes Mal manuell auffüllen müssen, wenn Sie eine Modell Änderung vornehmen.

Nachdem Sie nun die `MovieInitializer` Klasse definiert haben, sollten Sie Sie verknüpfen, damit jedes Mal, wenn die Anwendung ausgeführt wird, überprüft wird, ob sich die Modellklassen von dem Schema in der Datenbank unterscheiden. Wenn dies der Fall ist, können Sie den Initialisierer ausführen, um die Datenbank neu zu erstellen, damit Sie dem Modell entspricht, und dann die Datenbank mit den Beispiel Daten auffüllen.

Öffnen Sie die Datei " *Global. asax* ", die sich im Stammverzeichnis des `MvcMovies` Projekts befindet:

Die Datei " *Global. asax* " enthält die-Klasse, die die gesamte Anwendung für das Projekt definiert, und enthält einen `Application_Start` Ereignishandler, der beim ersten Start der Anwendung ausgeführt wird.

Suchen Sie nach der `Application_Start`-Methode, und fügen Sie am Anfang der-Methode einen `Database.SetInitializer`-aufrufsbefehl hinzu, wie unten dargestellt:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

Die soeben hinzugefügte `Database.SetInitializer` Anweisung gibt an, dass die Datenbank, die von der `MovieDBContext` Instanz verwendet wird, automatisch gelöscht und neu erstellt werden soll, wenn das Schema und die Datenbank nicht stimmen. Wie Sie gesehen haben, füllt sie auch die Datenbank mit den Beispiel Daten auf, die in der `MovieInitializer`-Klasse angegeben sind.

Schließen Sie die Datei " *Global. asax* ".

Führen Sie die Anwendung erneut aus, und navigieren Sie zur URL */Movies* . Wenn die Anwendung gestartet wird, erkennt Sie, dass die Modellstruktur nicht mehr mit dem Datenbankschema übereinstimmt. Die Datenbank wird automatisch neu erstellt, um der neuen Modellstruktur zu entsprechen, und die Datenbank wird mit den Beispiel Filmen aufgefüllt:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Klicken Sie auf den Link **neu erstellen** , um einen neuen Film hinzuzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich der Bewertung, wird jetzt in der Liste der Filme angezeigt:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

In diesem Abschnitt haben Sie erfahren, wie Sie Modell Objekte ändern und die Datenbank mit den Änderungen synchron halten. Sie haben auch gelernt, wie Sie eine neu erstellte Datenbank mit Beispiel Daten auffüllen, um Szenarios ausprobieren zu können. Als nächstes sehen wir uns an, wie Sie den Modellklassen eine umfassendere Validierungs Logik hinzufügen und einige Geschäftsregeln erzwingen können.

> [!div class="step-by-step"]
> [Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)
