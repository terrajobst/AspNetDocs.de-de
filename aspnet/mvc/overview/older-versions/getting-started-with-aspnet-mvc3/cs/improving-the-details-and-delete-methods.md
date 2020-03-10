---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Verbessern der Details und LöschmethodenC#() | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 54d7be8fe1bff604ae9c9e9914d7c6426ab85c1c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498705"
---
# <a name="improving-the-details-and-delete-methods-c"></a>Optimieren der Methoden „Details“ und „Delete“ (C#)

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

In diesem Teil des Tutorials nehmen Sie einige Verbesserungen an den automatisch generierten `Details` und `Delete` Methoden vor. Diese Änderungen sind nicht erforderlich. Allerdings können Sie die Anwendung problemlos optimieren, wenn Sie nur wenige kleine Code Teile haben.

## <a name="improving-the-details-and-delete-methods"></a>Verbessern der Details und Löschmethoden

Als Sie das Gerüst für den `Movie` Controller erstellt haben, hat ASP.NET MVC Code generiert, der hervorragend funktionierte, aber mit nur wenigen kleinen Änderungen stabiler machen kann.

Öffnen Sie den `Movie` Controller, und ändern Sie die Methode `Details`, indem Sie `HttpNotFound` zurückgeben, wenn ein Film nicht gefunden wird Außerdem sollten Sie die `Details`-Methode ändern, um einen Standardwert für die ID festzulegen, die an Sie übermittelt wird. (In [Teil 6](examining-the-edit-methods-and-edit-view.md) dieses Tutorials haben Sie ähnliche Änderungen an der `Edit`-Methode vorgenommen.) Sie müssen jedoch den Rückgabetyp der `Details` Methode von `ViewResult` in `ActionResult`ändern, da die `HttpNotFound`-Methode kein `ViewResult` Objekt zurückgibt. Das folgende Beispiel zeigt die geänderte `Details`-Methode.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Code First erleichtert die Suche nach Daten mithilfe der `Find`-Methode. Ein wichtiges Sicherheits Feature, das in die-Methode integriert wurde, besteht darin, dass der Code überprüft, ob die `Find`-Methode einen Film gefunden hat, bevor der Code versucht, etwas zu tun. Ein Hacker könnte z. b. Fehler in die Website einfügen, indem er die von den Verknüpfungen erstellte URL von `http://localhost:xxxx/Movies/Details/1` in etwas wie `http://localhost:xxxx/Movies/Details/12345` (oder einen anderen Wert, der keinen eigentlichen Film repräsentiert) ändert. Wenn Sie nicht auf einen NULL-Film überprüfen, kann dies zu einem Datenbankfehler führen.

Ändern Sie die Methoden `Delete` und `DeleteConfirmed` entsprechend, um einen Standardwert für den ID-Parameter anzugeben und `HttpNotFound` zurückzugeben, wenn ein Film nicht gefunden wird. Die aktualisierten `Delete` Methoden im `Movie` Controller sind unten dargestellt.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Beachten Sie, dass die `Delete`-Methode die Daten nicht löscht. Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder eigentlich eines Bearbeitungs-, Erstellungs- oder sonstigen Vorgangs, der Daten ändern) stellt eine Sicherheitslücke dar. Weitere Informationen hierzu finden Sie im Blogeintrag von Stephen Walther [ASP.NET MVC Tip #46 – verwenden Sie keine Lösch Links, da Sie Sicherheitslücken erstellen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Die `HttpPost`-Methode, die die Daten löscht, heißt `DeleteConfirmed`, um der HTTP-POST-Methode eine eindeutige Signatur bzw. einen eindeutigen Namen zu geben. Die beiden Methodensignaturen werden nachstehend gezeigt:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Der Common Language Runtime (CLR) erfordert, dass überladene Methoden über eine eindeutige Signatur verfügen (gleicher Name, andere Liste von Parametern). Hier benötigen Sie jedoch zwei Delete-Methoden: einen für "Get" und einen für "Post", bei dem beide die gleiche Signatur benötigen. (Beide müssen eine einzelne ganze Zahl als Parameter akzeptieren.)

Um dies zu sortieren, können Sie einige Dinge tun. Eine Möglichkeit besteht darin, den Methoden andere Namen zuzuweisen. Das haben wir im vorherigen Beispiel getan. Dies bringt jedoch ein kleines Problem mit sich: ASP.NET ordnet Segmente einer URL anhand des Namens zu Aktionsmethoden zu. Wenn Sie die Methode umbenennen sollten, ist das Routing normalerweise nicht in der Lage, diese Methode zu finden. Die Lösung besteht (wie im Beispiel) im Hinzufügen des `ActionName("Delete")`-Attributs zur `DeleteConfirmed`-Methode. Dadurch wird die Zuordnung für das Routing System effektiv durchführt, sodass eine URL, die <em>/Delete/</em>für eine Post-Anforderung enthält, die `DeleteConfirmed`-Methode findet.

Eine andere Möglichkeit, ein Problem mit Methoden zu vermeiden, die identische Namen und Signaturen aufweisen, besteht darin, die Signatur der Post-Methode künstlich so zu ändern, dass Sie einen nicht verwendeten Parameter enthält. Einige Entwickler fügen beispielsweise einen Parametertyp `FormCollection` hinzu, der an die Post-Methode übergeben wird, und verwenden dann einfach nicht den Parameter:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Umbilden

Sie verfügen jetzt über eine komplette ASP.NET MVC-Anwendung, die Daten in einer SQL Server Compact-Datenbank speichert. Sie können Filme erstellen, lesen, aktualisieren, löschen und nach Filmen suchen.

![](improving-the-details-and-delete-methods/_static/image1.png)

In diesem grundlegenden Tutorial haben Sie den Einstieg in die Controller, das Zuordnen von Ansichten und das Übergeben von hart codierten Daten begonnen. Anschließend haben Sie ein Datenmodell erstellt und entworfen. Entity Framework Code First eine Datenbank im laufenden Betrieb aus dem Datenmodell erstellt haben, und das ASP.NET MVC-Gerüstsystem hat automatisch die Aktionsmethoden und-Sichten für grundlegende CRUD-Vorgänge generiert. Anschließend haben Sie ein Suchformular hinzugefügt, mit dem Benutzer die Datenbank durchsuchen können. Sie haben die Datenbank so geändert, dass Sie eine neue Datenspalte enthält. Anschließend haben Sie zwei Seiten aktualisiert, um diese neuen Daten zu erstellen und anzuzeigen. Sie haben die Validierung hinzugefügt, indem Sie das Datenmodell mit Attributen aus dem `DataAnnotations`-Namespace markieren. Die resultierende Validierung wird auf dem Client und auf dem Server ausgeführt.

Wenn Sie Ihre Anwendung bereitstellen möchten, ist es hilfreich, zuerst die Anwendung auf dem lokalen IIS 7-Server zu testen. Sie können diesen Link für den [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) verwenden, um IIS-Einstellungen für ASP.NET-Anwendungen zu aktivieren. Sehen Sie sich die folgenden Bereitstellungs Links an:

- [ASP.net-Bereitstellungs Inhalts Zuordnung](https://msdn.microsoft.com/library/dd394698.aspx)
- [Aktivieren von IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Webanwendungs Projekte](https://msdn.microsoft.com/library/dd394698.aspx)

Ich empfehle Ihnen nun, unsere zwischengeschaltete [Erstellung eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) und [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) -Tutorials kennenzulernen, die [ASP.net-Artikel auf MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)kennenzulernen und die vielen Videos und Ressourcen in [https://asp.net/mvc](https://asp.net/mvc) kennenzulernen, um noch mehr über ASP.NET MVC zu erfahren. Die [ASP.NET-MVC-Foren](https://forums.asp.net/1146.aspx) sind ein guter Ort, um Fragen zu stellen.

Viel Spaß!

– Scott Hanselman ([http://hanselman.com](http://hanselman.com) und [@shanselman](http://twitter.com/shanselman) auf Twitter) und Rick Anderson [Blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Previous](adding-validation-to-the-model.md)
