---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Hinzufügen eines neuen Felds zum Film Modell und zur Tabelle | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498513"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a>Hinzufügen eines neuen Felds zum Modell und zur Tabelle eines Films

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.

In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen, um einige Änderungen an den Modellklassen zu migrieren, sodass die Änderung auf die Datenbank angewendet wird.

Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank zu erstellen, wie in diesem Tutorial bereits erwähnt, fügt Code First der Datenbank eine Tabelle hinzu, um zu verfolgen, ob das Schema der Datenbank mit den Modellklassen synchronisiert ist, von denen es generiert wurde. Wenn Sie nicht synchron sind, löst die Entity Framework einen Fehler aus. Dies vereinfacht das Auffinden von Problemen bei der Entwicklung, die Sie möglicherweise zur Laufzeit nur (aufgrund von unkomplizierter Fehlern) finden.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Einrichten von Code First-Migrationen für Modelländerungen

Wenn Sie Visual Studio 2012 verwenden, doppelklicken Sie auf die Datei *Movies. mdf* von Projektmappen-Explorer, um das Daten Bank Tool zu öffnen. Visual Studio Express für das Web Datenbank-Explorer angezeigt wird, wird in Visual Studio 2012 Server-Explorer angezeigt. Wenn Sie Visual Studio 2010 verwenden, verwenden Sie SQL Server-Objekt-Explorer.

Klicken Sie im Daten Bank Tool (Datenbank-Explorer, Server-Explorer oder SQL Server-Objekt-Explorer) mit der rechten Maustaste auf `MovieDBContext`, und wählen Sie löschen aus, um die Filme-Datenbank zu **Löschen** .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navigieren Sie zurück zu Projektmappen-Explorer. Klicken Sie mit der rechten Maustaste auf die Datei " *Movies. mdf* ", und wählen Sie **Löschen** aus, um die Datei

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Erstellen Sie die Anwendung, um sicherzustellen, dass keine Fehler vorliegen.

Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** und dann auf **Paket-Manager-Konsole**.

![Pack-Mann hinzufügen](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Geben Sie im Fenster der **Paket-Manager-Konsole** an der `PM>` Eingabeaufforderung "Enable-Migrations-contexttyk-Name mvcmovie. Models. muviedbcontext" ein.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Mit dem Befehl **enable-Migrationen** (siehe oben) wird eine *Configuration.cs* -Datei in einem neuen *Migrations* Ordner erstellt.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio öffnet die Datei *Configuration.cs* . Ersetzen Sie die `Seed`-Methode in der Datei *Configuration.cs* durch den folgenden Code:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Klicken Sie unter **`Movie` mit der** rechten Maustaste auf die rote Wellenlinie, und wählen Sie dann **Auflösen** und dann **mvcmovie. Models aus.**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Dadurch wird die folgende using-Anweisung hinzugefügt:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First-Migrationen wird die `Seed`-Methode nach jeder Migration aufgerufen (d. h., **Update-Database** wird in der Paket-Manager-Konsole aufgerufen), und diese Methode aktualisiert Zeilen, die bereits eingefügt wurden, oder fügt Sie ein, wenn Sie noch nicht vorhanden sind.

**Drücken Sie STRG + UMSCHALT + B, um das Projekt zu erstellen.** (Die folgenden Schritte schlagen fehl, wenn Sie zu diesem Zeitpunkt nicht erstellen.)

Der nächste Schritt ist das Erstellen einer `DbMigration`-Klasse für die erste Migration. Bei dieser Migration zu wird eine neue Datenbank erstellt. aus diesem Grund haben Sie die Datei " *Movie. mdf* " in einem vorherigen Schritt gelöscht.

Geben Sie im Fenster der **Paket-Manager-Konsole** den Befehl "Add-Migration Initial" ein, um die anfängliche Migration zu erstellen. Der Name "Initial" ist willkürlich und wird zum Benennen der erstellten Migrations Datei verwendet.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First-Migrationen erstellt eine andere Klassendatei im *Migrations* Ordner (mit dem Namen *{DATESTAMP}\_Initial.cs* ), und diese Klasse enthält Code, der das Datenbankschema erstellt. Der Dateiname der Migration ist mit einem Zeitstempel versehen, um die Reihenfolge zu unterstützen. Untersuchen Sie die Datei " *{DATESTAMP}\_Initial.cs* ", die die Anweisungen zum Erstellen der Film Tabelle für die Movie DB enthält. Wenn Sie die Datenbank in den folgenden Anweisungen aktualisieren, wird die Datei " *{DATESTAMP}\_Initial.cs* " ausgeführt und das Datenbankschema erstellt. Anschließend wird die **Seed** -Methode ausgeführt, um die Datenbank mit Testdaten aufzufüllen.

Geben Sie in der **Paket-Manager-Konsole**den Befehl "Update-Database" ein, um die Datenbank zu erstellen, und führen Sie die **Seed** -Methode aus.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Wenn Sie eine Fehlermeldung erhalten, die anzeigt, dass eine Tabelle bereits vorhanden und nicht erstellt werden kann, liegt dies wahrscheinlich daran, dass Sie die Anwendung ausgeführt haben, nachdem Sie die Datenbank gelöscht haben und bevor Sie `update-database`ausgeführt haben. Löschen Sie in diesem Fall die Datei " *Movies. mdf* " erneut, und wiederholen Sie den `update-database` Befehl. Wenn weiterhin eine Fehlermeldung angezeigt wird, löschen Sie den Ordner "Migrationen" und den Inhalt, und beginnen Sie mit den Anweisungen am oberen Rand dieser Seite (Löschen Sie die Datei " *Movies. mdf* ", und fahren Sie dann mit enable-Migrationen fort).

Führen Sie die Anwendung aus, und navigieren Sie zur URL */Movies* . Die Seed-Daten werden angezeigt.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Beginnen Sie, indem Sie der vorhandenen `Movie` Klasse eine neue `Rating`-Eigenschaft hinzufügen. Öffnen Sie die Datei *models\muvie.cs* , und fügen Sie die `Rating`-Eigenschaft wie folgt hinzu:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Die gesamte `Movie`-Klasse sieht nun wie der folgende Code aus:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Erstellen Sie die Anwendung mit dem Menübefehl **Build** &gt;**Build Movie** oder durch Drücken von STRG + UMSCHALT + B.

Nachdem Sie die `Model`-Klasse aktualisiert haben, müssen Sie auch die Ansichts Vorlagen *\views\movies\index.cshtml* und *\views\movies\kreate.cshtml* aktualisieren, um die neue `Rating`-Eigenschaft in der Browseransicht anzuzeigen.

Öffnen Sie die Datei "<em>\views\movies\index.cshtml</em> ", und fügen Sie eine `<th>Rating</th>` Spaltenüberschrift direkt hinter der <strong>Preis</strong> Spalte ein. Fügen Sie dann am Ende der Vorlage eine `<td>` Spalte hinzu, um den `@item.Rating` Wert zu erzeugen. Im folgenden sehen Sie, wie die aktualisierte Vorlage " <em>Index. cshtml</em> " angezeigt wird:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Öffnen Sie als nächstes die Datei " *\views\movies\kreate.cshtml* ", und fügen Sie das folgende Markup am Ende des Formulars ein. Dadurch wird ein Textfeld gerendert, sodass Sie beim Erstellen eines neuen Films eine Bewertung angeben können.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Sie haben nun den Anwendungscode aktualisiert, um die neue `Rating`-Eigenschaft zu unterstützen.

Führen Sie nun die Anwendung aus, und navigieren Sie zur URL */Movies* . Wenn Sie dies jedoch tun, wird einer der folgenden Fehler angezeigt:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Dieser Fehler wird angezeigt, da die aktualisierte `Movie` Modell Klasse in der Anwendung sich nun von dem Schema der `Movie` Tabelle der vorhandenen Datenbank unterscheidet. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)

Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Diese Vorgehensweise ist sehr praktisch, wenn eine aktive Entwicklung für eine Testdatenbank ausgeführt wird. Sie ermöglicht es Ihnen, das Modell und das Datenbankschema schnell zu entwickeln. Der Nachteil besteht jedoch darin, dass Sie vorhandene Daten in der Datenbank verlieren – damit Sie diesen Ansatz *nicht* für eine Produktionsdatenbank verwenden möchten. Die Verwendung eines Initialisierers zum automatischen Seed-Seed einer Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer Anwendung. Weitere Informationen zu Entity Framework-datenbankinitialisierern finden Sie unter Tom Dykstra es [ASP.NET MVC/Entity Framework Tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.
3. Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.

Für dieses Tutorial verwenden wir Code First-Migrationen.

Aktualisieren Sie die Seed-Methode, sodass Sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie die Datei migrations\configuration.cs, und fügen Sie jedem Movie-Objekt ein Bewertungsfeld hinzu.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Erstellen Sie die Projekt Mappe, öffnen Sie das Konsolenfenster des **Paket-Managers** , und geben Sie den folgenden Befehl ein:

`add-migration AddRatingMig`

Der `add-migration` Befehl weist das Migrations Framework an, das aktuelle Movie-Modell mit dem aktuellen Movie DB-Schema zu untersuchen und den erforderlichen Code zum Migrieren der Datenbank zum neuen Modell zu erstellen. Addratingmig ist willkürlich und wird verwendet, um die Migrations Datei zu benennen. Es ist hilfreich, einen aussagekräftigen Namen für den Migrationsschritt zu verwenden.

Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration` abgeleitete Klasse definiert, und in der `Up`-Methode sehen Sie den Code, der die neue Spalte erstellt.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Erstellen Sie die Projekt Mappe, und geben Sie dann den Befehl "Update-Database" in das Fenster der **Paket-Manager-Konsole** ein.

Die folgende Abbildung zeigt die Ausgabe im Fenster der **Paket-Manager-Konsole** (der Datumsstempel "addratingmig" unterscheidet sich.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Führen Sie die Anwendung erneut aus, und navigieren Sie zur URL/Movies. Sie können das Feld neue Bewertung sehen.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Klicken Sie auf den Link **neu erstellen** , um einen neuen Film hinzuzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich der Bewertung, wird jetzt in der Liste der Filme angezeigt:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Sie sollten auch das Feld "`Rating`" den Vorlagen "Bearbeiten", "Details" und "SearchIndex" hinzufügen.

Sie können den Befehl "Update-Database" im Paket- **Manager-Konsolen** Fenster erneut eingeben, und es werden keine Änderungen vorgenommen, da das Schema mit dem Modell übereinstimmt.

In diesem Abschnitt haben Sie erfahren, wie Sie Modell Objekte ändern und die Datenbank mit den Änderungen synchron halten. Sie haben auch gelernt, wie Sie eine neu erstellte Datenbank mit Beispiel Daten auffüllen, um Szenarios ausprobieren zu können. Als nächstes sehen wir uns an, wie Sie den Modellklassen eine umfassendere Validierungs Logik hinzufügen und einige Geschäftsregeln erzwingen können.

> [!div class="step-by-step"]
> [Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)
