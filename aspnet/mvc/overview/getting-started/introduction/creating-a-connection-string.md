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
ms.openlocfilehash: 20781ad760d3a0e4559ec4c7e18528f3686dcc02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499011"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="bcfde-102">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="bcfde-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="bcfde-103">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bcfde-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="bcfde-104">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="bcfde-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="bcfde-105">Die von Ihnen erstellte `MovieDBContext`-Klasse übernimmt die Aufgabe der Verbindung mit der Datenbank und die Zuordnung von `Movie` Objekten zu Datenbankdaten Sätzen.</span><span class="sxs-lookup"><span data-stu-id="bcfde-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bcfde-106">Eine Frage ist jedoch, wie Sie angeben können, mit welcher Datenbank eine Verbindung hergestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="bcfde-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="bcfde-107">Sie müssen eigentlich nicht angeben, welche Datenbank verwendet werden soll, Entity Framework standardmäßig [localdb](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)verwendet.</span><span class="sxs-lookup"><span data-stu-id="bcfde-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="bcfde-108">In diesem Abschnitt wird explizit eine Verbindungs Zeichenfolge in der Datei " *Web. config* " der Anwendung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bcfde-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="bcfde-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="bcfde-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="bcfde-110">[Localdb](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) ist eine vereinfachte Version der SQL Server Express Datenbank-Engine, die bei Bedarf gestartet wird und im Benutzermodus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bcfde-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="bcfde-111">Localdb wird in einem speziellen Ausführungs Modus von SQL Server Express ausgeführt, der es Ihnen ermöglicht, mit Datenbanken als *MDF* -Dateien zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="bcfde-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="bcfde-112">Normalerweise werden localdb-Datenbankdateien im *App-\_Daten* Ordner eines Webprojekts gespeichert.</span><span class="sxs-lookup"><span data-stu-id="bcfde-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="bcfde-113">SQL Server Express wird nicht für die Verwendung in produktionsweb Anwendungen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="bcfde-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="bcfde-114">Localdb sollte insbesondere nicht für die Produktion mit einer Webanwendung verwendet werden, da es nicht für die Verwendung mit IIS konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="bcfde-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="bcfde-115">Eine localdb-Datenbank kann jedoch problemlos zu SQL Server oder SQL Azure migriert werden.</span><span class="sxs-lookup"><span data-stu-id="bcfde-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="bcfde-116">In Visual Studio 2017 wird localdb standardmäßig mit Visual Studio installiert.</span><span class="sxs-lookup"><span data-stu-id="bcfde-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="bcfde-117">Standardmäßig sucht die Entity Framework nach einer Verbindungs Zeichenfolge mit dem Namen, die mit der Objekt Kontext Klasse übereinstimmen (`MovieDBContext` für dieses Projekt).</span><span class="sxs-lookup"><span data-stu-id="bcfde-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="bcfde-118">Weitere Informationen finden Sie unter SQL Server Verbindungs Zeichenfolgen [für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcfde-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="bcfde-119">Öffnen Sie die *Web. config* -Datei der Anwendung, die unten dargestellt ist.</span><span class="sxs-lookup"><span data-stu-id="bcfde-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="bcfde-120">(Nicht die Datei " *Web. config* " im Ordner " *views* ".)</span><span class="sxs-lookup"><span data-stu-id="bcfde-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="bcfde-121">Suchen Sie das `<connectionStrings>`-Element:</span><span class="sxs-lookup"><span data-stu-id="bcfde-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="bcfde-122">Fügen Sie dem `<connectionStrings>`-Element in der Datei " *Web. config* " die folgende Verbindungs Zeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="bcfde-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="bcfde-123">Das folgende Beispiel zeigt einen Teil der Datei " *Web. config* ", in dem die neue Verbindungs Zeichenfolge hinzugefügt wurde:</span><span class="sxs-lookup"><span data-stu-id="bcfde-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="bcfde-124">Die beiden Verbindungs Zeichenfolgen sind sehr ähnlich.</span><span class="sxs-lookup"><span data-stu-id="bcfde-124">The two connection strings are very similar.</span></span> <span data-ttu-id="bcfde-125">Die erste Verbindungs Zeichenfolge wird `DefaultConnection` benannt und wird für die Mitgliedschafts Datenbank verwendet, um zu steuern, wer auf die Anwendung zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="bcfde-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="bcfde-126">Die Verbindungs Zeichenfolge, die Sie hinzugefügt haben, gibt eine localdb-Datenbank mit dem Namen *Movie. mdf* im Ordner *App\_Data* an.</span><span class="sxs-lookup"><span data-stu-id="bcfde-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="bcfde-127">Die Mitgliedschafts Datenbank wird in diesem Tutorial nicht verwendet. Weitere Informationen zu Mitgliedschaft, Authentifizierung und Sicherheit finden Sie in meinem Tutorial [Erstellen einer ASP.NET MVC-App mit Authentifizierung und SQL-Datenbank und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="bcfde-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="bcfde-128">Der Name der Verbindungs Zeichenfolge muss mit dem Namen der [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) -Klasse identisch sein.</span><span class="sxs-lookup"><span data-stu-id="bcfde-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="bcfde-129">Sie müssen die `MovieDBContext` Verbindungs Zeichenfolge nicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bcfde-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="bcfde-130">Wenn Sie keine Verbindungs Zeichenfolge angeben, erstellt Entity Framework eine localdb-Datenbank im Verzeichnis "Users" mit dem voll qualifizierten Namen der [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) -Klasse (in diesem Fall `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="bcfde-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="bcfde-131">Sie können der Datenbank einen beliebigen Namen benennen, solange Sie über verfügt *. MDF* -Suffix.</span><span class="sxs-lookup"><span data-stu-id="bcfde-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="bcfde-132">Wir könnten der Datenbank beispielsweise den Namen " *myfilms. mdf*" geben.</span><span class="sxs-lookup"><span data-stu-id="bcfde-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="bcfde-133">Als Nächstes erstellen Sie eine neue `MoviesController`-Klasse, die Sie verwenden können, um die Filmdaten anzuzeigen und Benutzern das Erstellen neuer Film Auflistungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="bcfde-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bcfde-134">[Zurück](adding-a-model.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="bcfde-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
