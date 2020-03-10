---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET Datenzugriff-Empfohlene Ressourcen | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Thema enthält Links zu Dokumentations Ressourcen zum Zugreifen auf Daten in ASP.NET-Webanwendungen, primär mithilfe der Entity Framework und SQL SE...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513969"
---
# <a name="aspnet-data-access---recommended-resources"></a>ASP.NET: Datenzugriff – Empfohlene Ressourcen

> Dieses Thema enthält Links zu Dokumentations Ressourcen zum Zugreifen auf Daten in ASP.NET-Webanwendungen, primär mithilfe der Entity Framework und SQL Server.
> 
> Wenn Sie einen tollen Blogbeitrag, [StackOverflow](http://stackoverflow.com) -Thread oder einen anderen Link, der nützlich wäre, kennen, [Senden Sie uns eine e-Mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) mit dem Link.
> 
> Letzte Aktualisierung 4/3/2014

Dieses Thema enthält folgende Abschnitte:

- [Einstieg in den Datenzugriff in ASP.net](#gettingstarted)
- [Verwenden des Entity Framework](#ef)

    - [Verwenden von Entity Framework Code First](#cf)
    - [Verwenden von Entity Framework Code First-Migrationen](#efcfmigrations)
    - [Verwenden von Entity Framework Database First oder Model First (EF-Designer)](#efdbf)
    - [Laden verwandter Daten in Entity Framework (Lazy Load, unverzügliches Laden und Explizites Laden)](#efrelateddata)
    - [Optimieren der Entity Framework Leistung](#optimizingef)
    - [Behandeln von Parallelität in einer Entity Framework Anwendung](#efconcurrency)
    - [Bücher zum Entity Framework](#efbooks)
    - [Zusätzliche Entity Framework Ressourcen](#otherefresources)
- [Datenbindung in ASP.net-Web Forms Anwendungen](#wfdatabinding)

    - [Verwenden Web Forms Modell Bindung](#wfmodelbinding)
    - [Verwenden von Web Forms Datenquellen-Steuerelementen](#wfdsc)
    - [Verwenden von Web Forms Daten gebundenen Steuerelementen und Daten Bindungs Ausdrücken](#wfdbc)
- [Arbeiten mit SQL Server-Datenbanken](#sqlserver)

    - [Arbeiten mit SQL Server Express localdb-Datenbanken](#sslocaldb)
    - [Arbeiten mit SQL Server Express-Datenbanken](#sse)
    - [Arbeiten mit der Windows Azure SQL-Datenbank](#ssdb)
    - [Auswählen zwischen SQL Server und Windows Azure SQL-Datenbank](#ssdbchoosing)
- [Arbeiten mit nosql-Datenbankverwaltungssystemen](#nosql)
- [Verwenden von LINQ-Abfragen in ASP.NET-Anwendungen](#linq)
- [Verwenden dynamische Daten Gerüstbau](#dd)
- [Sichern des Datenzugriffs](#securing)
- [Optimieren der Datenzugriffs Leistung](#optimizingdataaccess)
- [Bereitstellen einer Datenbank](#deploying)
- [Zugreifen auf Daten über einen Webdienst](#webservice)
- [Weitere Ressourcen](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Einstieg in den Datenzugriff in ASP.net

- [Daten Speicherungs Optionen (entwickeln realer Cloud-apps mit Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Kapitel eines e-Books zum entwickeln für die Cloud. Führt nosql-Datenbanken als Alternative ein, die viele Entwickler, die mit relationalen Datenbanken vertraut sind, häufig übersehen Hier finden Sie Richtlinien dazu, was Sie bei der Auswahl von relationalem oder nosql berücksichtigen oder eine bestimmte Plattform auswählen können.
- [ASP.NET Data Access Options](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Eine Einführung in die Datenzugriffs Optionen für relationale Datenbanken für ASP.net und Anleitungen zum Auswählen von Plattformen und zum Zugriff auf Methoden, die für Ihr Szenario geeignet sind.
- [Relationale Datenbank](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Wenn Sie nicht mit relationalen Datenbanken gearbeitet haben, finden Sie auf dieser Seite eine Einführung in die Terminologie und Konzepte von relationalen Datenbanken. Eine Einführung in SQL Server finden Sie weiter unten in diesem Thema unter [Arbeiten mit SQL Server Datenbanken](#sqlserver) .

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Verwenden des Entity Framework

- [Entity Framework Entwicklungsansätze](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Anleitungen zum Auswählen eines Entity Framework Entwicklungsansatzes Database First, Model First oder Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Verwenden von Entity Framework Code First

Die folgenden Tutorials bieten herunterladbare Beispielanwendungen:

- Die ersten Schritte [mit EF 6 unter Verwendung von MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Umfasst eine Vielzahl von Entity Framework Code First Szenarien, einschließlich Migrationen und EF 6-Features wie verbindungsresilienz, Befehls Abfang Funktion und Async. Dies ist eine aktualisierte Version der [EF 5/MVC 4-Serie](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Die vorherige Reihe enthält ein Tutorial zu den Repository-und Arbeitseinheiten Mustern, die nicht in der neuen Reihe enthalten sind.
- [Einführung in ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Deckt einen engeren Bereich von Entity Framework Code First Szenarien ab, führt aber eine umfassendere Aufgabe der Einführung von MVC-Features aus.
- [Modell Bindung und Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Verwendet Code First in einer Web Forms Anwendung.
- Der Einstieg [in ASP.NET 4,5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Eine Einführung in die Web Forms mit einer gewissen Abdeckung Code First. Verwendet Modell Bindung.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Verwendet Code First in einer e-Commerce-MVC 3-Anwendung, die auch die Mitgliedschaft und Autorisierung implementiert. Die hier verwendete MVC-Version und das ASP.NET-Mitgliedschaftssystem (Authentifizierung und Autorisierung) sind veraltet. Weitere aktuelle Informationen zur ASP.NET-Mitgliedschaft finden Sie unter [https://asp.net/identity](https://asp.net/identity).

Weitere Ressourcen:

- [Entity Framework-Code First zu einer vorhandenen Datenbank](https://msdn.microsoft.com/data/jj200620). MSDN. Video und Exemplarische Vorgehensweise, in der gezeigt wird, wie Code First mit einer vorhandenen Datenbank verwendet wird.
- [Data Developer Center-Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Eine Anleitung für Entity Framework Dokumentation, die vom Entity Framework-Team erstellt und verwaltet wurde, finden Sie im Link " [Get Started](https://msdn.microsoft.com/data/ee712907) ".

Weitere [Informationen zu den Entity Framework](#efbooks) und [zusätzlichen Entity Framework Ressourcen](#otherefresources) finden Sie weiter unten in diesem Thema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Verwenden von Entity Framework Code First-Migrationen

In den meisten der oben aufgeführten Code First Tutorials werden Migrationen behandelt. Weitere Informationen finden Sie auch in den folgenden Ressourcen.

- [ASP.NET-Webbereitstellung mithilfe von Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)Dabei handelt es sich um eine Lernprogrammserie mit zwölf Teilen, in der die Bereitstellungsaufgaben vollständig vorgestellt werden. 2-teilige tutorialreihe, in der gezeigt wird, wie Code First-Migrationen zum Bereitstellen einer Datenbank verwendet wird.
- Stellen [Sie eine sichere ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit. Microsoft Azure). Verwenden von Migrationen zum Bereitstellen von Mitgliedschafts-und Anwendungsdaten in Azure.
- [Übersicht über die Webbereitstellung für Visual Studio und ASP.net](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Im Abschnitt **Konfigurieren der Daten Bank Bereitstellung in Visual Studio** finden Sie eine Erläuterung dazu, wie Code First-Migrationen in Funktionen der Webbereitstellung in Visual Studio integriert ist.
- [Data Developer Center-Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) (MSDN). Die Migrations Dokumentation des Entity Framework Teams.
- [Migrations Screencast-Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF-Blog). Drei Videos zu erweiterten Themen in Code First-Migrationen.
- [Code First-Migrationen mit ASP.net Web Pages Websites](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnettts-Blog). Zeigt, wie Code First Migrationen mit einer ASP.net Web Pages Site verwendet werden, indem der Datenkontext in einem Visual Studio-Klassen Bibliotheksprojekt bereitgestellt wird.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Verwenden von Entity Framework Database First oder Model First (EF-Designer)

- Die ersten Schritte [mit Entity Framework 6 Database First mit MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Führen Sie ein Skript in Server-Explorer aus, um eine Datenbank zu erstellen, und verwenden Sie dann den Entity Framework Designer, um das Datenmodell zu erstellen. Zeigt, wie einfache CRUD-Webseiten erstellt werden. für andere Funktionen zur Datenverarbeitung können Sie einem der Code First Tutorials folgen, da alle EF-Workflows dieselbe dbcontext-API verwenden.

Die folgenden Ressourcen sind älter. Sie sind nützlich, wenn Sie die Version 4,0 des Entity Framework verwenden und ein Datenquellen-Steuerelement für die Datenbindung in einer Web Forms Anwendung verwenden möchten.

- [Beginnen Sie mit der Entity Framework 4,0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Zeigt, wie das **EntityDataSource** -Steuerelement verwendet wird.
- [Fortfahren mit dem Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(zeigt, wie Sie das **ObjectDataSource** -Steuerelement verwenden. Enthält ein Tutorial zur Parallelitäts Behandlung, ein Tutorial zur Leistung von EF und ein Tutorial zu den Neuerungen in EF 4,0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Behandeln von zugehörigen Daten in Entity Framework (Lazy Load, unverzügliches Laden und Explizites Laden)

- [Lesen verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First MVC-Beispielanwendung. Die angezeigten Methoden gelten auch für Web Forms Modell Bindung und den Database First Workflow.
- [Data Developer Center: Laden verwandter Entitäten](https://msdn.microsoft.com/data/jj574232) (MSDN). Die Dokumentation des Entity Framework Teams zum Laden verwandter Daten.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimieren der Entity Framework Leistung

- [Erweiterte Entity Framework Szenarien für eine ASP.NET-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Zeigt, wie Sie eigene SQL-Anweisungen ausführen oder eigene gespeicherte Prozeduren aufzurufen, die Änderungs Erkennung deaktivieren und die Validierung beim Speichern von Änderungen deaktivieren.
- [Leistungs Überlegungen zu Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Überlegungen zur Leistung (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximieren der Leistung mit dem Entity Framework in einer ASP.NET-Webanwendung](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Gilt für Entity Framework 4,0.
- Siehe auch [Optimieren des ASP.NET-Datenzugriffs](#optimizingdataaccess) weiter unten in diesem Thema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Behandeln von Parallelität in einer Entity Framework Anwendung

- [Behandeln von Parallelität mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, dbcontext-API, mithilfe einer MVC-Beispielanwendung.
- [Data Developer Center – optimistische](https://msdn.microsoft.com/data/jj592904) Parallelitäts Muster (MSDN). Die Parallelitäts Dokumentation des Entity Framework Teams.
- [Behandeln von Parallelität mit dem Entity Framework in einer ASP.NET-Webanwendung](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Gilt für Entity Framework 4,0. Database First, ObjectContext-API, mithilfe einer Web Forms Beispielanwendung.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Bücher zum Entity Framework

- [Programmieren Entity Framework: dbcontext](http://shop.oreilly.com/product/0636920022237.do) von Julie Lerman und Rowan Miller.
- [Programmier Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

Beide Bücher sind mit den aktuellen empfohlenen Techniken auf dem neuesten Stand. Sie bieten eine umfassendere, aber einfach zu befolgende Einführung in den Entity Framework als alle im Internet verfügbaren. Ein weiteres Buch, das [Programmieren von Entity Framework](http://shop.oreilly.com/product/9780596807252.do) von Julie Lerman, ist größer und umfassender, aber es ist älter, und viele der Techniken, die es behandelt, sind nicht mehr die empfohlene Vorgehensweise für die Verwendung des Entity Framework. Weitere Informationen finden Sie auch in der Liste der Bücher, die vom Entity Framework Team im [Data Developer Center (Bücher](https://msdn.microsoft.com/data/aa937716) zur MSDN-Website) empfohlen werden.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Weitere Entity Framework Ressourcen

- [Entity Framework (ADO.net)-Teamblog](https://blogs.msdn.com/b/adonet/). Eine der besten Ressourcen für die aktuellsten Informationen und Ankündigungen neuer Erweiterungen. Weitere EF-bezogene Blogs finden Sie unter "Blogroll" unter " [Get Started with Entity Framework](https://msdn.microsoft.com/data/ee712907)".
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Weitere Informationen zu Themen im Zusammenhang mit dem Entity Framework finden Sie in der Spalte **Datenpunkte** .

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Datenbindung in ASP.net-Web Forms Anwendungen

- [ASP.net Web Forms Datenzugriffs Optionen](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Verwenden Web Forms Modell Bindung

- [Modell Bindung und Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Tutorialreihe mit EF-Code First.
- [Web Forms Modell Bindung Teil 1: Auswählen von Daten](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Blog von Scott Guthrie). In diesen älteren Blogbeiträgen hat die Eigenschaft, die derzeit den Namen ItemType hat, den Namen modeltype, aber andernfalls sind die darin enthaltenen Informationen gültig.
- [Web Forms Modell Bindung Teil 2: Filtern von Daten](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Blog von Scott Guthrie).
- [Web Forms Modell Bindung Teil 3: Aktualisieren und validieren](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Blog von Scott Guthrie).
- [ASP.NET 4,5 Web Forms Modell Bindung](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (Video).
- [Modell Bindung Teil 1: Auswählen von Daten](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (Video).
- [Modell Bindung, Teil 2: Filtern](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (Video).
- [In den ersten Schritten mit ASP.NET 4,5-Web Forms-Datenelemente und-Details anzeigen](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Verwenden von Web Forms Datenquellen-Steuerelementen

- [Datenquellen-Webserver Steuerelemente](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Ankündigung der Veröffentlichung von dynamische Daten Provider-und EntityDataSource-Steuerelement für Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (Microsoft Web Development-Blog).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Verwenden von Web Forms Daten gebundenen Steuerelementen und Daten Bindungs Ausdrücken

- [Modell Bindung und Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Tutorialreihe, in der EF Code First verwendet werden.
- [In den ersten Schritten mit ASP.NET 4,5-Web Forms-Datenelemente und-Details anzeigen](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Stark typisierte Daten Steuerelemente](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Blog von Scott Guthrie).
- [Stark typisierte Daten Steuerelemente](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (Video).
- [ASP.NET 4,5 Web Forms stark typisierte Daten Steuerelemente](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (Video).
- [Daten gebundene Webserver Steuerelemente](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Übersicht über Daten Bindungs Ausdrücke](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Auf dieser Seite werden nur **eval** und **Bind**behandelt. Es wurde nicht aktualisiert, um **Item** und **binditem**einzuschließen.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Arbeiten mit SQL Server-Datenbanken

- [SQL Server-Datenbankfunktionen](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Eine allgemeine Einführung in eine Vielzahl von SQL Server Themen finden Sie in den Einträgen im Inhaltsverzeichnis.
- [SQL Server Editionen](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Eine Zusammenfassung der verfügbaren SQL Server Editionen mit Links zu weiteren Informationen zu den einzelnen Editionen.)
- [SQL Server Verbindungs Zeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Verwenden von SQL Server Compact für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Beispiele für Daten Bankprodukte](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). AdventureWorks-Beispiel Datenbanken.
- [Installieren von Beispiel Datenbanken](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Zusätzlich zu den hier gezeigten Methoden können Sie auch eine der MDF-Beispieldateien in den App-\_Datenordner eines Webprojekts herunterladen, die Datenbank in localdb konvertieren und eine localdb-Verbindungs Zeichenfolge erstellen. Weitere Informationen hierzu finden Sie unter Gewusst [wie: Upgrade auf localdb](https://msdn.microsoft.com/library/hh873188.aspx).

Weitere Informationen finden Sie auch in den folgenden Abschnitten zum Arbeiten mit SQL Server Express und localdb und zum Auswählen zwischen SQL Server und SQL-Datenbank.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Arbeiten mit SQL Server Express localdb-Datenbanken

- [SQL Server Express 2012 localdb](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Die offizielle MSDN-Einführung in localdb.
- [SQL Server Verbindungs Zeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- Vorgehens [Weise: Upgrade auf localdb](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Vorgehensweise beim Migrieren einer MDF-Datei aus einer früheren Version von SQL Server Express zu localdb. Sie müssen diesen Vorgang auch durchlaufen, wenn Sie eine der [SQL Server Beispiel Datenbanken "2012](https://go.microsoft.com/fwlink/?linkid=117483)" herunterladen.
- [Einführung in localdb, ein verbessertes SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express Blog). Bietet mehr Hintergrundinformationen dazu, warum localdb erstellt wurde als in MSDN enthalten.
- [Localdb: wo befindet sich meine Datenbank?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express-Blog). Informationen dazu, wo localdb-Datenbankdateien erstellt werden.
- [Verwenden von localdb mit vollständigem IIS, Teil 1: Benutzerprofil](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express Blog). Localdb ist nicht für die Verwendung mit IIS konzipiert. Diese Reihe von Blogbeiträgen erläutert die Probleme und einige Problem Umgehungen.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Arbeiten mit SQL Server Express-Datenbanken

- [SQL Server Verbindungs Zeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Wenn Sie die Verbindungs Zeichenfolgen-Einstellung "AttachDBFilename" mit SQL Server Express verwenden, finden Sie weitere Informationen im Abschnitt Benutzer Instanz dieser Seite.
- [Übernehmen des Besitzes Ihrer lokalen SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express Blog). Ein häufiges Problem ist nicht in der Lage, mit SQL Server Express Datenbanken zu arbeiten, da Sie kein Administrator für die SQL Server Express-Instanz sind. Standardmäßig ist nur die Person, die SQL Server Express installiert hat, ein Administrator. In diesem Blog wird erläutert, wie Sie sich als Administrator auf dem Computer SQL Server Express, wenn Sie ein Administrator sind.
- [Kann meine ASP.NET-Webanwendung eine SQL Server Express Datenbank in der Produktionsumgebung verwenden?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Arbeiten mit der Windows Azure SQL-Datenbank

- Stellen [Sie eine sichere ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) -Website (Microsoft Azure-Website) bereit.
- [SQL-Datenbanken](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure-Website). Lernprogramme und Anleitungen zu den ersten Schritten.
- [Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Der Knoten der obersten Ebene des Inhaltsverzeichnisses für die SQL-Datenbank in MSDN.
- [TechNet wiki-Artikel Index für Windows Azure SQL-Datenbank](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (Microsoft TechNet-Website).
- [Anwendungs Block zur Behandlung vorübergehender Fehler](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Ein Framework, das es Ihnen ermöglicht, vorübergehende Netzwerkfehler und Verbindungsfehler zu behandeln, die durch eine Drosselung verursacht werden. Verfügbar in einem nuget-Paket: [Enterprise Library 5,0-Anwendungs Block zur Behandlung vorübergehender Fehler](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- Die ersten Schritte [mit SQL-Datenbank und Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure-Trainingskit](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft Download Center). Umfasst praktische Übungseinheiten für die SQL-Datenbank.
- [Communityforum für Windows Azure SQL-Datenbank](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Umstellung auf Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Ein Kapitel eines umfassenden End-to-End-Szenarios vom Microsoft Patterns and Practices-Team. Erläutert, warum Sie möglicherweise migrieren möchten und wie Sie von SQL Server zu einer SQL-Datenbank migrieren.
- [Migrieren von SQL Server-Datenbanken zu Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL-Datenbankmigrations-Assistent](http://sqlazuremw.codeplex.com/). Ein Open Source-Tool zum Migrieren von Datenbanken zu und aus der SQL-Datenbank.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Auswählen zwischen SQL Server und Windows Azure SQL-Datenbank

- [Vergleichen Sie SQL Server mit der Windows Azure SQL-Datenbank](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (Microsoft TechNet-Website).
- [Daten Migration zu Windows Azure SQL-Datenbank: Tools und Techniken](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Enthält Abschnitte, in denen die SQL Server mit der SQL-Datenbank verglichen werden und Anleitungen für die Migration von SQL Server zu SQL-Datenbank bereitgestellt werden.
- [Leitfaden zur Windows Azure SQL-Daten Bank Bereitstellung](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (Microsoft TechNet-Website).
- [Einschränkungen für SQL Server Features (Windows Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage und Windows Azure SQL-Datenbank: Vergleich und Gegenüberstellung](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Für eine Anwendung, die Sie in Windows Azure bereitstellen, kann der Windows Azure-Tabellen Speicher eine Alternative zu Windows Azure SQL-Datenbank sein. Dieses Thema hilft Ihnen bei der Entscheidung zwischen diesen Alternativen.
- [Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Richtlinien und Einschränkungen (Windows Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Arbeiten mit nosql-Datenbankverwaltungssystemen

- [Windows Azure-Data Services](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure-Website). Weitere Informationen finden Sie im [Featurehandbuch für Tabellen Dienste](https://docs.microsoft.com/azure/) und im Abschnitt **Big Data** auf der Seite.
- [ASP.NET-Anwendung mit mehreren Ebenen mithilfe von Speicher Tabellen, Warteschlangen und BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure Site). End-to-End-Tutorial mit herunterladbaren Beispielanwendungen, die Windows Azure Storage nosql-Tabellen verwenden.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Verwenden von LINQ-Abfragen in ASP.NET-Anwendungen

- [ASP.NET Data Access Options](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Enthält eine Einführung in LINQ.
- [LINQ-Schulungs Videos](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (Blog von Joe Stagner).
- [ASP.NET Forums Thread mit Links zu dynamischen LINQ-Ressourcen](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Verwenden dynamische Daten Gerüstbau

- [Dynamische Daten Projektvorlagen](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Leitfaden zur Verwendung von dynamische Daten Projekten.
- [ASP.net dynamische Daten](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Sichern des Datenzugriffs

- [Sichern des Datenzugriffs in ASP.net](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Sicherheitsüberlegungen (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- Gewusst [wie: Sichern von Verbindungs Zeichenfolgen bei der Verwendung von Datenquellen Steuerelementen](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimieren der Datenzugriffs Leistung

- [ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET Caching](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Verbessern der Leistung von ASP.net](https://msdn.microsoft.com/library/ff647787) (MSDN). Oben auf dieser Seite befindet sich die Warnung "deaktivierte Inhalte", aber die meisten Informationen sind immer noch relevant, und es ist keine vergleichbare aktualisierte Ressource vorhanden.
- [Verbessern der SQL Server Leistung](https://msdn.microsoft.com/library/ff647793) (MSDN). Derselbe Kommentar wie der vorherige Link.

Siehe auch [Optimieren der Entity Framework Leistung](#optimizingef) weiter oben in diesem Thema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Bereitstellen einer Datenbank

- [ASP.net Web Deployment: Empfohlene Ressourcen](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Zugreifen auf Daten über einen Webdienst

- [Zugreifen auf Daten über einen Webdienst](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Leitfaden für die Verwendung der Web-API im Vergleich zu WCF.
- Der Einstieg [in ASP.net-Web-API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET Data Access FAQ](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.net Web Forms Tutorials-Data](../web-forms/overview/data-access/index.md). Die meisten dieser Tutorials sind relativ alt. Stellen Sie sicher, dass Sie zuerst [ASP.NET Datenzugriffs Optionen](https://msdn.microsoft.com/library/ms178359.aspx) und [Datenspeicher Optionen (erstellen realer Cloud-apps mit Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) lesen, damit Sie nicht zu weit in eine Datenzugriffs Methode gelangen, die für Ihr Szenario nicht geeignet ist.
- [ASP.NET MVC-Inhalts](../mvc/overview/getting-started/recommended-resources-for-mvc.md)Zuordnung.
- [ASP.net Web Pages Tutorials-Daten](../web-pages/overview/data/index.md).
- [Zugreifen auf Daten in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Bietet eine Liste von Links, die dieser Inhalts Zuordnung ähneln, aber mit dem Fokus auf Visual Studio anstatt ASP.net.
