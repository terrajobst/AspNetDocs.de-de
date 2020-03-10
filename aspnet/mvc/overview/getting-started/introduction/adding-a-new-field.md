---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Hinzufügen eines neuen Felds | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5974e53e4610dccc7812df261dc97a9b0327de85
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470931"
---
# <a name="adding-a-new-field"></a>Hinzufügen eines neuen Felds

von [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen, um einige Änderungen an den Modellklassen zu migrieren, sodass die Änderung auf die Datenbank angewendet wird.

Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank zu erstellen, wie in diesem Tutorial bereits erwähnt, fügt Code First der Datenbank eine Tabelle hinzu, um zu verfolgen, ob das Schema der Datenbank mit den Modellklassen synchronisiert ist, von denen es generiert wurde. Wenn Sie nicht synchron sind, löst die Entity Framework einen Fehler aus. Dies vereinfacht das Auffinden von Problemen bei der Entwicklung, die Sie möglicherweise zur Laufzeit nur (aufgrund von unkomplizierter Fehlern) finden.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Einrichten von Code First-Migrationen für Modelländerungen

Navigieren Sie zu Projektmappen-Explorer. Klicken Sie mit der rechten Maustaste auf die Datei " *Movies. mdf* ", und wählen Sie **Löschen** aus, um die Datei Wenn die Datei *Movies. mdf* nicht angezeigt wird, klicken Sie auf das Symbol **alle Dateien anzeigen** , das unten in der roten Gliederung angezeigt wird.

![](adding-a-new-field/_static/image1.png)

Erstellen Sie die Anwendung, um sicherzustellen, dass keine Fehler vorliegen.

Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** und dann auf **Paket-Manager-Konsole**.

![Pack-Mann hinzufügen](adding-a-new-field/_static/image2.png)

Geben Sie im Fenster **Paket-Manager-Konsole** an der `PM>` Eingabeaufforderung ein.

Enable-Migrationen-contexttykname mvcmovie. Models. muviedbcontext

![](adding-a-new-field/_static/image3.png)

Mit dem Befehl **enable-Migrationen** (siehe oben) wird eine *Configuration.cs* -Datei in einem neuen *Migrations* Ordner erstellt.

![](adding-a-new-field/_static/image4.png)

Visual Studio öffnet die Datei *Configuration.cs* . Ersetzen Sie die `Seed`-Methode in der Datei *Configuration.cs* durch den folgenden Code:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Bewegen Sie den Mauszeiger über die rote Wellenlinie unter `Movie`, und klicken Sie auf `Show Potential Fixes` und **dann auf** **mvcmovie. Models.**

![](adding-a-new-field/_static/image5.png)

Dadurch wird die folgende using-Anweisung hinzugefügt:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First-Migrationen wird die `Seed`-Methode nach jeder Migration aufgerufen (d. h., **Update-Database** wird in der Paket-Manager-Konsole aufgerufen), und diese Methode aktualisiert Zeilen, die bereits eingefügt wurden, oder fügt Sie ein, wenn Sie noch nicht vorhanden sind.
> 
> Die [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode im folgenden Code führt einen Upsert-Vorgang aus:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Da die [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode bei jeder Migration ausgeführt wird, können Sie keine Daten einfügen, da die Zeilen, die Sie hinzufügen möchten, nach der ersten Migration, mit der die Datenbank erstellt wird, bereits vorhanden sind. Der[Upsert](http://en.wikipedia.org/wiki/Upsert)-Vorgang verhindert Fehler, die auftreten würden, wenn Sie versuchen, eine Zeile einzufügen, die bereits vorhanden ist, aber alle Änderungen an Daten überschreibt, die Sie beim Testen der Anwendung möglicherweise vorgenommen haben. Mit Testdaten in einigen Tabellen sollten Sie dies möglicherweise nicht tun: in einigen Fällen, in denen Sie Daten während des Tests ändern, möchten Sie, dass Ihre Änderungen nach Datenbankaktualisierungen verbleiben. In diesem Fall möchten Sie einen bedingten Einfügevorgang durchführen: Fügen Sie eine Zeile nur ein, wenn Sie nicht bereits vorhanden ist.   
> 
> Der erste Parameter, der an die [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode übergeben wird, gibt die Eigenschaft an, mit der überprüft wird, ob eine Zeile bereits vorhanden ist. Für die von Ihnen bereitgestellten testfilmdaten kann die `Title`-Eigenschaft für diesen Zweck verwendet werden, da jeder Titel in der Liste eindeutig ist:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Bei diesem Code wird davon ausgegangen, dass Titel eindeutig sind. Wenn Sie einen doppelten Titel manuell hinzufügen, erhalten Sie die folgende Ausnahme, wenn Sie das nächste Mal eine Migration ausführen.   
> 
> *Die Sequenz enthält mehr als ein Element.*  
> 
> Weitere Informationen zur [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode finden Sie unter [Übernehmen der addorupdate-Methode von EF 4,3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).

**Drücken Sie STRG + UMSCHALT + B, um das Projekt zu erstellen.** (Die folgenden Schritte schlagen fehl, wenn Sie zu diesem Zeitpunkt nicht erstellt werden.)

Der nächste Schritt ist das Erstellen einer `DbMigration`-Klasse für die erste Migration. Bei dieser Migration wird eine neue Datenbank erstellt. aus diesem Grund haben Sie die Datei " *Movie. mdf* " in einem vorherigen Schritt gelöscht.

Geben Sie im Fenster **Paket-Manager-Konsole** den Befehl `add-migration Initial` ein, um die anfängliche Migration zu erstellen. Der Name "Initial" ist willkürlich und wird zum Benennen der erstellten Migrations Datei verwendet.

![](adding-a-new-field/_static/image6.png)

Code First-Migrationen erstellt eine andere Klassendatei im *Migrations* Ordner (mit dem Namen *{DATESTAMP}\_Initial.cs* ), und diese Klasse enthält Code, der das Datenbankschema erstellt. Der Dateiname der Migration ist mit einem Zeitstempel versehen, um die Reihenfolge zu unterstützen. Untersuchen Sie die Datei " *{DATESTAMP}\_Initial.cs* ", die die Anweisungen zum Erstellen der `Movies` Tabelle für die Movie DB enthält. Wenn Sie die Datenbank in den folgenden Anweisungen aktualisieren, wird die Datei " *{DATESTAMP}\_Initial.cs* " ausgeführt und das Datenbankschema erstellt. Anschließend wird die **Seed** -Methode ausgeführt, um die Datenbank mit Testdaten aufzufüllen.

Geben Sie in der **Paket-Manager-Konsole**den Befehl `update-database`, um die Datenbank zu erstellen, und führen Sie die `Seed`-Methode aus.

![](adding-a-new-field/_static/image7.png)

Wenn Sie eine Fehlermeldung erhalten, die anzeigt, dass eine Tabelle bereits vorhanden und nicht erstellt werden kann, liegt dies wahrscheinlich daran, dass Sie die Anwendung ausgeführt haben, nachdem Sie die Datenbank gelöscht haben und bevor Sie `update-database`ausgeführt haben. Löschen Sie in diesem Fall die Datei " *Movies. mdf* " erneut, und wiederholen Sie den `update-database` Befehl. Wenn weiterhin eine Fehlermeldung angezeigt wird, löschen Sie den Ordner "Migrationen" und den Inhalt, und beginnen Sie mit den Anweisungen am oberen Rand dieser Seite (Löschen Sie die Datei " *Movies. mdf* ", und fahren Sie dann mit enable-Migrationen fort). Wenn Sie weiterhin einen Fehler erhalten, öffnen Sie SQL Server-Objekt-Explorer, und entfernen Sie die Datenbank aus der Liste.

Führen Sie die Anwendung aus, und navigieren Sie zur URL */Movies* . Die Seed-Daten werden angezeigt.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Beginnen Sie, indem Sie der vorhandenen `Movie` Klasse eine neue `Rating`-Eigenschaft hinzufügen. Öffnen Sie die Datei *models\muvie.cs* , und fügen Sie die `Rating`-Eigenschaft wie folgt hinzu:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Die gesamte `Movie`-Klasse sieht nun wie der folgende Code aus:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Erstellen Sie die Anwendung (STRG + UMSCHALT + B).

Da Sie der `Movie`-Klasse ein neues Feld hinzugefügt haben, müssen Sie auch die Bindungs- *weißen Liste* aktualisieren, damit diese neue Eigenschaft eingeschlossen wird. Aktualisieren Sie das `bind`-Attribut für `Create` und `Edit` Aktionsmethoden, um die `Rating`-Eigenschaft einzuschließen:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Sie müssen auch die Ansichtsvorlagen aktualisieren, um die neue `Rating`-Eigenschaft in der Browseransicht anzuzeigen, zu erstellen und zu bearbeiten.

Öffnen Sie die Datei " *\views\movies\index.cshtml* ", und fügen Sie eine `<th>Rating</th>` Spaltenüberschrift direkt hinter der **Preis** Spalte ein. Fügen Sie dann am Ende der Vorlage eine `<td>` Spalte hinzu, um den `@item.Rating` Wert zu erzeugen. Im folgenden sehen Sie, wie die aktualisierte Vorlage " *Index. cshtml* " angezeigt wird:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Öffnen Sie als nächstes die Datei " *\views\movies\kreate.cshtml* ", und fügen Sie das Feld "`Rating`" mit dem folgenden markierten Markup hinzu. Dadurch wird ein Textfeld gerendert, sodass Sie beim Erstellen eines neuen Films eine Bewertung angeben können.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Sie haben nun den Anwendungscode aktualisiert, um die neue `Rating`-Eigenschaft zu unterstützen.

Führen Sie die Anwendung aus, und navigieren Sie zur URL */Movies* . Wenn Sie dies jedoch tun, wird einer der folgenden Fehler angezeigt:

![](adding-a-new-field/_static/image9.png)  
  
Das Modell, das den Kontext "Kontext" unterstützt, hat sich seit der Erstellung der Datenbank geändert. Verwenden Sie Code First-Migrationen, um die Datenbank zu aktualisieren (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Dieser Fehler wird angezeigt, da die aktualisierte `Movie` Modell Klasse in der Anwendung sich nun von dem Schema der `Movie` Tabelle der vorhandenen Datenbank unterscheidet. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)

Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Dieser Ansatz ist früh im Entwicklungszyklus sehr praktisch, wenn die aktive Entwicklung in einer Testdatenbank erfolgt. Er ermöglicht Ihnen, das Modell und das Datenbankschema schnell weiterzuentwickeln. Der Nachteil besteht jedoch darin, dass Sie vorhandene Daten in der Datenbank verlieren – damit Sie diesen Ansatz *nicht* für eine Produktionsdatenbank verwenden möchten. Das Verwenden eines Initialisierers zum automatischen Ausführen eines Seedings für eine Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer Anwendung. Weitere Informationen zu Entity Framework-datenbankinitialisierern finden Sie unter [ASP.NET MVC/Entity Framework Tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.
3. Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.

Für dieses Tutorial verwenden wir Code First-Migrationen.

Aktualisieren Sie die Seed-Methode, sodass Sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie die Datei migrations\configuration.cs, und fügen Sie jedem Movie-Objekt ein Bewertungsfeld hinzu.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Erstellen Sie die Projekt Mappe, öffnen Sie das Konsolenfenster des **Paket-Managers** , und geben Sie den folgenden Befehl ein:

`add-migration Rating`

Der `add-migration` Befehl weist das Migrations Framework an, das aktuelle Movie-Modell mit dem aktuellen Movie DB-Schema zu untersuchen und den erforderlichen Code zum Migrieren der Datenbank zum neuen Modell zu erstellen. Die namens *Bewertung* ist willkürlich und wird verwendet, um die Migrations Datei zu benennen. Es ist hilfreich, einen aussagekräftigen Namen für den Migrationsschritt zu verwenden.

Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration` abgeleitete Klasse definiert, und in der `Up`-Methode sehen Sie den Code, der die neue Spalte erstellt.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Erstellen Sie die Projekt Mappe, und geben Sie dann den Befehl `update-database` im Fenster **Paket-Manager-Konsole** ein.

Die folgende Abbildung zeigt die Ausgabe im Fenster der **Paket-Manager-Konsole** (die *Bewertung* der vorab ausstehenden Datumsstempel ist anders.)

![](adding-a-new-field/_static/image11.png)

Führen Sie die Anwendung erneut aus, und navigieren Sie zur URL/Movies. Sie können das Feld neue Bewertung sehen.

![](adding-a-new-field/_static/image12.png)

Klicken Sie auf den Link **neu erstellen** , um einen neuen Film hinzuzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich der Bewertung, wird jetzt in der Liste der Filme angezeigt:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Nachdem das Projekt Migrationen verwendet hat, müssen Sie die Datenbank nicht löschen, wenn Sie ein neues Feld hinzufügen oder das Schema anderweitig aktualisieren. Im nächsten Abschnitt nehmen wir weitere Schema Änderungen vor und verwenden Migrationen, um die Datenbank zu aktualisieren.

Sie sollten auch das Feld `Rating` den Vorlagen bearbeiten, Details und Löschen Anzeigen hinzufügen.

Sie können den Befehl "Update-Database" erneut im Fenster " **Paket-Manager-Konsole** " eingeben, und es würde kein Migrations Code ausgeführt werden, da das Schema mit dem Modell übereinstimmt. Wenn Sie jedoch "Update-Database" ausführen, wird die `Seed`-Methode erneut ausgeführt, und wenn Sie einen der Seed-Daten geändert haben, gehen die Änderungen verloren, da die `Seed`-Methode Daten aktualisiert. Weitere Informationen zur `Seed`-Methode finden Sie im beliebten [ASP.NET MVC/Entity Framework-Tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)von Tom Dykstra.

In diesem Abschnitt haben Sie erfahren, wie Sie Modell Objekte ändern und die Datenbank mit den Änderungen synchron halten. Sie haben auch gelernt, wie Sie eine neu erstellte Datenbank mit Beispiel Daten auffüllen, um Szenarios ausprobieren zu können. Dies war nur eine kurze Einführung in Code First. eine ausführlichere Anleitung zum Thema finden Sie unter [Erstellen eines Entity Framework Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) . Als nächstes sehen wir uns an, wie Sie den Modellklassen eine umfassendere Validierungs Logik hinzufügen und einige Geschäftsregeln erzwingen können.

> [!div class="step-by-step"]
> [Zurück](adding-search.md)
> [Weiter](adding-validation.md)
