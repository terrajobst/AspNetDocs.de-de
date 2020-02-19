---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Hinzufügen eines Modells | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457803"
---
# <a name="adding-a-model"></a>Hinzufügen eines Modells

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.

In diesem Abschnitt fügen Sie einige Klassen zum Verwalten von Filmen in einer-Datenbank hinzu. Diese Klassen sind das &quot;Modell&quot; Teil der MVC-Anwendung ASP.net.

Sie verwenden eine .NET Framework Datenzugriffs Technologie, die als [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) bezeichnet wird, um diese Modellklassen zu definieren und mit Ihnen zu arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklungsparadigma namens *Code First*. Code First ermöglicht es Ihnen, Modell Objekte zu erstellen, indem einfache Klassen geschrieben werden. (Diese werden auch als poco-Klassen bezeichnet, von &quot;Plain-Old CLR-Objekten.&quot;) Anschließend können Sie die Datenbank dynamisch von ihren Klassen erstellen lassen, wodurch ein sehr sauberer und schneller Entwicklungs Workflow ermöglicht wird.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen**und dann **Klasse**aus.

![](adding-a-model/_static/image1.png)

Geben Sie den *Klassen* Namen &quot;Movie&quot;ein.

Fügen Sie der `Movie`-Klasse die folgenden fünf Eigenschaften hinzu:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Wir verwenden die `Movie`-Klasse, um Filme in einer Datenbank darzustellen. Jede Instanz eines `Movie` Objekts entspricht einer Zeile in einer Datenbanktabelle, und jede Eigenschaft der `Movie` Klasse wird einer Spalte in der Tabelle zugeordnet.

Fügen Sie in der gleichen Datei die folgende `MovieDBContext`-Klasse hinzu:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Die `MovieDBContext`-Klasse stellt den Entity Framework Movie-Daten Bank Kontext dar, der das Abrufen, speichern und Aktualisieren von `Movie`-Klassen Instanzen in einer Datenbank übernimmt. Die `MovieDBContext` wird von der `DbContext` Basisklasse abgeleitet, die vom Entity Framework bereitgestellt wird.

Um auf `DbContext` und `DbSet`verweisen zu können, müssen Sie am Anfang der Datei die folgende `using`-Anweisung hinzufügen:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Die Datei Complete *Movie.cs* wird unten angezeigt. (Mehrere using-Anweisungen, die nicht benötigt werden, wurden entfernt.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB

Die von Ihnen erstellte `MovieDBContext`-Klasse übernimmt die Aufgabe der Verbindung mit der Datenbank und die Zuordnung von `Movie` Objekten zu Datenbankdaten Sätzen. Eine Frage ist jedoch, wie Sie angeben können, mit welcher Datenbank eine Verbindung hergestellt werden soll. Hierzu fügen Sie Verbindungsinformationen in der Datei " *Web. config* " der Anwendung hinzu.

Öffnen Sie die *Web. config* -Datei der Anwendung. (Nicht die Datei " *Web. config* " im Ordner " *views* ".) Öffnen Sie die Datei *Web. config* in rot.

![](adding-a-model/_static/image2.png)

Fügen Sie dem `<connectionStrings>`-Element in der Datei " *Web. config* " die folgende Verbindungs Zeichenfolge hinzu.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Das folgende Beispiel zeigt einen Teil der Datei " *Web. config* ", in dem die neue Verbindungs Zeichenfolge hinzugefügt wurde:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Diese kleine Menge an Code und XML ist alles, was Sie schreiben müssen, um die Filmdaten in einer Datenbank darzustellen und zu speichern.

Als Nächstes erstellen Sie eine neue `MoviesController`-Klasse, die Sie verwenden können, um die Filmdaten anzuzeigen und Benutzern das Erstellen neuer Film Auflistungen zu ermöglichen.

> [!div class="step-by-step"]
> [Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)
