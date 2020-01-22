---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Untersuchen der Methoden "Details" und "Delete" | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 4ec8d239377d37d7e27fa23c0b1caef7420046ae
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519010"
---
# <a name="examining-the-details-and-delete-methods"></a>Untersuchen der Methoden „Details“ und „Delete“

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

In diesem Teil des Tutorials untersuchen Sie die automatisch generierten `Details` und `Delete` Methoden.

## <a name="examining-the-details-and-delete-methods"></a>Untersuchen der Methoden „Details“ und „Delete“

Öffnen Sie den `Movie` Controller, und untersuchen Sie die `Details`-Methode.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Die MVC-Gerüstbau-Engine, die diese Aktionsmethode erstellt hat, fügt einen Kommentar hinzu, der eine HTTP-Anforderung anzeigt, die In diesem Fall handelt es sich um eine `GET` Anforderung mit drei URL-Segmenten, dem `Movies` Controller, der `Details`-Methode und einem `ID` Wert.

Code First erleichtert die Suche nach Daten mithilfe der `Find`-Methode. Ein wichtiges Sicherheits Feature, das in die-Methode integriert ist, besteht darin, dass der Code überprüft, ob die `Find`-Methode einen Film gefunden hat, bevor der Code versucht, etwas zu tun. Ein Hacker könnte z. b. Fehler in die Website einfügen, indem er die von den Verknüpfungen erstellte URL von `http://localhost:xxxx/Movies/Details/1` in etwas wie `http://localhost:xxxx/Movies/Details/12345` (oder einen anderen Wert, der keinen eigentlichen Film repräsentiert) ändert. Wenn Sie nicht auf einen NULL-Film geprüft haben, führt ein NULL-Film zu einem Datenbankfehler.

Untersuchen Sie die Methoden `Delete` und `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Beachten Sie, dass die HTTP Get `Delete`-Methode den angegebenen Film nicht löscht, sondern eine Ansicht des Films zurückgibt, in der Sie den Löschvorgang übermitteln können (`HttpPost`). Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder eigentlich eines Bearbeitungs-, Erstellungs- oder sonstigen Vorgangs, der Daten ändern) stellt eine Sicherheitslücke dar. Weitere Informationen hierzu finden Sie im Blogeintrag von Stephen Walther [ASP.NET MVC Tip #46 – verwenden Sie keine Lösch Links, da Sie Sicherheitslücken erstellen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Die `HttpPost`-Methode, die die Daten löscht, heißt `DeleteConfirmed`, um der HTTP-POST-Methode eine eindeutige Signatur bzw. einen eindeutigen Namen zu geben. Die beiden Methodensignaturen werden nachstehend gezeigt:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Die Common Language Runtime (CLR) erfordert überladene Methoden, um eine eindeutige Parametersignatur zu erhalten (selber Methodenname, aber unterschiedliche Liste von Parametern). Hier benötigen Sie jedoch zwei Delete-Methoden: einen für "Get" und einen für "Post", die beide die gleiche Parameter Signatur aufweisen. (Beide müssen eine einzelne ganze Zahl als Parameter akzeptieren.)

Um dies zu sortieren, können Sie einige Dinge tun. Eine Möglichkeit besteht darin, den Methoden andere Namen zuzuweisen. Dafür das der Gerüstbaumechanismus im vorherigen Beispiel gesorgt. Dies bringt jedoch ein kleines Problem mit sich: ASP.NET ordnet Segmente einer URL anhand des Namens zu Aktionsmethoden zu. Wenn Sie die Methode umbenennen sollten, ist das Routing normalerweise nicht in der Lage, diese Methode zu finden. Die Lösung besteht (wie im Beispiel) im Hinzufügen des `ActionName("Delete")`-Attributs zur `DeleteConfirmed`-Methode. Dadurch wird die Zuordnung für das Routing System effektiv durchführt, sodass eine URL, die */Delete/* für eine Post-Anforderung enthält, die `DeleteConfirmed`-Methode findet.

Eine andere gängige Methode, um ein Problem mit Methoden zu vermeiden, die identische Namen und Signaturen aufweisen, besteht darin, die Signatur der Post-Methode künstlich zu ändern, sodass Sie einen nicht verwendeten Parameter enthält. Einige Entwickler fügen beispielsweise einen Parametertyp `FormCollection` hinzu, der an die Post-Methode übergeben wird, und verwenden dann einfach nicht den Parameter:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Summary

Sie verfügen jetzt über eine komplette ASP.NET MVC-Anwendung, die Daten in einer lokalen DatenbankDatenbank speichert. Sie können Filme erstellen, lesen, aktualisieren, löschen und nach Filmen suchen.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine Webanwendung erstellt und getestet haben, ist der nächste Schritt, Sie anderen Benutzern zur Verfügung zu stellen, die über das Internet verwendet werden. Zu diesem Zweck müssen Sie die Anwendung für einen Webhostinganbieter bereitstellen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem [kostenlosen Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)an. Ich empfehle Ihnen als nächstes das Tutorial bereitstellen [einer sicheren ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein ausgezeichnetes Tutorial ist die Zwischenebene von Tom Dykstra, das [ein Entity Framework Datenmodell für eine ASP.NET MVC-Anwendung erstellt](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) und die [ASP.NET-MVC-Foren](https://forums.asp.net/1146.aspx) sind ein hervorragend Ort, um Fragen zu stellen. Folgen Sie [mir](https://twitter.com/RickAndMSFT) auf Twitter, damit Sie Updates zu den neuesten Tutorials erhalten.

Feedback ist willkommen.

– [Rick Anderson](https://blogs.msdn.com/rickAndy) Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
– [Scott Hanselman](http://www.hanselman.com/blog/) Twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Vorherige](adding-validation.md)
