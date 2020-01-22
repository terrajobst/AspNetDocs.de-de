---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Erstellen einer Verbindungs Zeichenfolge und arbeiten mit SQL Server localdb | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d3c6e736c5dcf4a3615e3c72cfc033effc7cc8e6
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519309"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB

Die von Ihnen erstellte `MovieDBContext`-Klasse übernimmt die Aufgabe der Verbindung mit der Datenbank und die Zuordnung von `Movie` Objekten zu Datenbankdaten Sätzen. Eine Frage ist jedoch, wie Sie angeben können, mit welcher Datenbank eine Verbindung hergestellt werden soll. Sie müssen eigentlich nicht angeben, welche Datenbank verwendet werden soll, Entity Framework standardmäßig [localdb](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)verwendet. In diesem Abschnitt wird explizit eine Verbindungs Zeichenfolge in der Datei " *Web. config* " der Anwendung hinzugefügt.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[Localdb](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) ist eine vereinfachte Version der SQL Server Express Datenbank-Engine, die bei Bedarf gestartet wird und im Benutzermodus ausgeführt wird. Localdb wird in einem speziellen Ausführungs Modus von SQL Server Express ausgeführt, der es Ihnen ermöglicht, mit Datenbanken als *MDF* -Dateien zu arbeiten. Normalerweise werden localdb-Datenbankdateien im *App-\_Daten* Ordner eines Webprojekts gespeichert.

SQL Server Express wird nicht für die Verwendung in produktionsweb Anwendungen empfohlen. Localdb sollte insbesondere nicht für die Produktion mit einer Webanwendung verwendet werden, da es nicht für die Verwendung mit IIS konzipiert ist. Eine localdb-Datenbank kann jedoch problemlos zu SQL Server oder SQL Azure migriert werden.

In Visual Studio 2017 wird localdb standardmäßig mit Visual Studio installiert.

Standardmäßig sucht die Entity Framework nach einer Verbindungs Zeichenfolge mit dem Namen, die mit der Objekt Kontext Klasse übereinstimmen (`MovieDBContext` für dieses Projekt). Weitere Informationen finden Sie unter SQL Server Verbindungs Zeichenfolgen [für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Öffnen Sie die *Web. config* -Datei der Anwendung, die unten dargestellt ist. (Nicht die Datei " *Web. config* " im Ordner " *views* ".)

![](creating-a-connection-string/_static/image1.png)

Suchen Sie das `<connectionStrings>`-Element:

![](creating-a-connection-string/_static/image2.png)

Fügen Sie dem `<connectionStrings>`-Element in der Datei " *Web. config* " die folgende Verbindungs Zeichenfolge hinzu.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Das folgende Beispiel zeigt einen Teil der Datei " *Web. config* ", in dem die neue Verbindungs Zeichenfolge hinzugefügt wurde:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Die beiden Verbindungs Zeichenfolgen sind sehr ähnlich. Die erste Verbindungs Zeichenfolge wird `DefaultConnection` benannt und wird für die Mitgliedschafts Datenbank verwendet, um zu steuern, wer auf die Anwendung zugreifen kann. Die Verbindungs Zeichenfolge, die Sie hinzugefügt haben, gibt eine localdb-Datenbank mit dem Namen *Movie. mdf* im Ordner *App\_Data* an. Die Mitgliedschafts Datenbank wird in diesem Tutorial nicht verwendet. Weitere Informationen zu Mitgliedschaft, Authentifizierung und Sicherheit finden Sie in meinem Tutorial [Erstellen einer ASP.NET MVC-App mit Authentifizierung und SQL-Datenbank und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Der Name der Verbindungs Zeichenfolge muss mit dem Namen der [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) -Klasse identisch sein.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Sie müssen die `MovieDBContext` Verbindungs Zeichenfolge nicht hinzufügen. Wenn Sie keine Verbindungs Zeichenfolge angeben, erstellt Entity Framework eine localdb-Datenbank im Verzeichnis "Users" mit dem voll qualifizierten Namen der [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) -Klasse (in diesem Fall `MvcMovie.Models.MovieDBContext`). Sie können der Datenbank einen beliebigen Namen benennen, solange Sie über verfügt *. MDF* -Suffix. Wir könnten der Datenbank beispielsweise den Namen " *myfilms. mdf*" geben.

Als Nächstes erstellen Sie eine neue `MoviesController`-Klasse, die Sie verwenden können, um die Filmdaten anzuzeigen und Benutzern das Erstellen neuer Film Auflistungen zu ermöglichen.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)
