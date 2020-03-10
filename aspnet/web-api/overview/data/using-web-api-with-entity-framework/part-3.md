---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Verwenden Sie Code First-Migrationen, um die Datenbank zu Seed | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449115"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Verwenden Sie Code First-Migrationen, um die Datenbank zu verwenden.

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF, um die Datenbank mit Testdaten zu durchführen.

Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](part-3/samples/sample1.cmd)]

Dieser Befehl fügt Ihrem Projekt einen Ordner namens Migrationen sowie eine Codedatei mit dem Namen Configuration.cs im Migrations Ordner hinzu.

![](part-3/_static/image1.png)

Öffnen Sie die Datei Configuration.cs. Fügen Sie die folgende **using** -Anweisung hinzu.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Fügen Sie dann der **Configuration. Seed** -Methode den folgenden Code hinzu:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Geben Sie im Fenster der Paket-Manager-Konsole die folgenden Befehle ein:

[!code-console[Main](part-3/samples/sample4.cmd)]

Der erste Befehl generiert Code, mit dem die Datenbank erstellt wird, und der zweite Befehl führt den Code aus. Die Datenbank wird mithilfe von [localdb](https://msdn.microsoft.com/library/hh510202.aspx)lokal erstellt.

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Erkunden Sie die API (optional)

Drücken Sie F5, um die Anwendung im Debugmodus auszuführen. Visual Studio startet IIS Express und führt Ihre Web-App aus. Visual Studio öffnet dann einen Browser und öffnet die Startseite der app.

Wenn Visual Studio ein Webprojekt ausführt, wird eine Portnummer zugewiesen. In der folgenden Abbildung ist die Portnummer 50524. Wenn Sie die Anwendung ausführen, wird eine andere Portnummer angezeigt.

![](part-3/_static/image3.png)

Die Startseite wird mit ASP.NET MVC implementiert. Am oberen Rand der Seite befindet sich ein Link mit dem Text "API". Über diesen Link gelangen Sie zu einer automatisch generierten Hilfeseite für die Web-API. (Informationen dazu, wie diese Hilfeseite generiert wird und wie Sie der Seite Ihre eigene Dokumentation hinzufügen können, finden Sie unter [Erstellen von Hilfeseiten für ASP.net-Web-API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können auf die Links der Hilfeseite klicken, um Details zur API anzuzeigen, einschließlich des Anforderungs-und Antwort Formats.

![](part-3/_static/image4.png)

Die API ermöglicht CRUD-Vorgänge für die Datenbank. Im folgenden wird die-API zusammengefasst.

| Authors |  |
| --- | -- |
| GET api/authors | Alle Autoren erhalten. |
| GET api/authors/{id} | Erstellen Sie einen Autor anhand der ID. |
| POST /api/authors | Erstellen Sie einen neuen Autor. |
| PUT /api/authors/{id} | Aktualisiert einen vorhandenen Autor. |
| DELETE /api/authors/{id} | Löschen Sie einen Autor. |

| Bücher |  |
| --- | -- |
| GET /api/books | Alle Bücher erhalten. |
| GET /api/books/{id} | Sie erhalten ein Buch anhand der ID. |
| POST /api/books | Erstellen Sie ein neues Buch. |
| PUT /api/books/{id} | Aktualisieren eines vorhandenen Buchs. |
| DELETE /api/books/{id} | Löschen eines Buchs. |

## <a name="view-the-database-optional"></a>Anzeigen der Datenbank (optional)

Wenn Sie den Update-Database-Befehl ausgeführt haben, hat EF die Datenbank erstellt und die `Seed`-Methode aufgerufen. Wenn Sie die Anwendung lokal ausführen, verwendet EF [localdb](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Sie können die Datenbank in Visual Studio anzeigen. Wählen Sie im Menü **Ansicht** die Option **SQL Server-Objekt-Explorer**.

![](part-3/_static/image5.png)

Geben Sie im Dialogfeld **Verbindung mit Server herstellen** im Bearbeitungsfeld **Server Name den Namen** "(localdb) \v11.0" ein. Belassen Sie die **Authentifizierungs** Option als "Windows-Authentifizierung". Klicken Sie auf **Verbinden**.

![](part-3/_static/image6.png)

Visual Studio stellt eine Verbindung mit localdb her und zeigt die vorhandenen Datenbanken im SQL Server-Objekt-Explorer Fenster an. Sie können die Knoten erweitern, um die von EF erstellten Tabellen anzuzeigen.

![](part-3/_static/image7.png)

Um die Daten anzuzeigen, klicken Sie mit der rechten Maustaste auf eine Tabelle, und wählen Sie **Daten anzeigen**aus.

![](part-3/_static/image8.png)

Der folgende Screenshot zeigt die Ergebnisse für die Bücher Tabelle. Beachten Sie, dass EF die Datenbank mit den Seed-Daten aufgefüllt hat und die Tabelle den Fremdschlüssel für die Autoren Tabelle enthält.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Zurück](part-2.md)
> [Weiter](part-4.md)
