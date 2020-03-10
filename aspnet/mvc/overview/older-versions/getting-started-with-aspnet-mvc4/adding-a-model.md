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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434415"
---
# <a name="adding-a-model"></a><span data-ttu-id="1725a-104">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="1725a-104">Adding a Model</span></span>

<span data-ttu-id="1725a-105">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1725a-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="1725a-106">Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="1725a-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="1725a-107">Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.</span><span class="sxs-lookup"><span data-stu-id="1725a-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="1725a-108">In diesem Abschnitt fügen Sie einige Klassen zum Verwalten von Filmen in einer-Datenbank hinzu.</span><span class="sxs-lookup"><span data-stu-id="1725a-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="1725a-109">Diese Klassen sind das &quot;Modell&quot; Teil der MVC-Anwendung ASP.net.</span><span class="sxs-lookup"><span data-stu-id="1725a-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="1725a-110">Sie verwenden eine .NET Framework Datenzugriffs Technologie, die als [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) bezeichnet wird, um diese Modellklassen zu definieren und mit Ihnen zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="1725a-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="1725a-111">Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklungsparadigma namens *Code First*.</span><span class="sxs-lookup"><span data-stu-id="1725a-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="1725a-112">Code First ermöglicht es Ihnen, Modell Objekte zu erstellen, indem einfache Klassen geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="1725a-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="1725a-113">(Diese werden auch als poco-Klassen bezeichnet, von &quot;Plain-Old CLR-Objekten.&quot;) Anschließend können Sie die Datenbank dynamisch von ihren Klassen erstellen lassen, wodurch ein sehr sauberer und schneller Entwicklungs Workflow ermöglicht wird.</span><span class="sxs-lookup"><span data-stu-id="1725a-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="1725a-114">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="1725a-114">Adding Model Classes</span></span>

<span data-ttu-id="1725a-115">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen**und dann **Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="1725a-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="1725a-116">Geben Sie den *Klassen* Namen &quot;Movie&quot;ein.</span><span class="sxs-lookup"><span data-stu-id="1725a-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="1725a-117">Fügen Sie der `Movie`-Klasse die folgenden fünf Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="1725a-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="1725a-118">Wir verwenden die `Movie`-Klasse, um Filme in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="1725a-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="1725a-119">Jede Instanz eines `Movie` Objekts entspricht einer Zeile in einer Datenbanktabelle, und jede Eigenschaft der `Movie` Klasse wird einer Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1725a-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="1725a-120">Fügen Sie in der gleichen Datei die folgende `MovieDBContext`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="1725a-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="1725a-121">Die `MovieDBContext`-Klasse stellt den Entity Framework Movie-Daten Bank Kontext dar, der das Abrufen, speichern und Aktualisieren von `Movie`-Klassen Instanzen in einer Datenbank übernimmt.</span><span class="sxs-lookup"><span data-stu-id="1725a-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="1725a-122">Die `MovieDBContext` wird von der `DbContext` Basisklasse abgeleitet, die vom Entity Framework bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="1725a-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="1725a-123">Um auf `DbContext` und `DbSet`verweisen zu können, müssen Sie am Anfang der Datei die folgende `using`-Anweisung hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="1725a-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="1725a-124">Die Datei Complete *Movie.cs* wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1725a-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="1725a-125">(Mehrere using-Anweisungen, die nicht benötigt werden, wurden entfernt.)</span><span class="sxs-lookup"><span data-stu-id="1725a-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="1725a-126">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="1725a-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="1725a-127">Die von Ihnen erstellte `MovieDBContext`-Klasse übernimmt die Aufgabe der Verbindung mit der Datenbank und die Zuordnung von `Movie` Objekten zu Datenbankdaten Sätzen.</span><span class="sxs-lookup"><span data-stu-id="1725a-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="1725a-128">Eine Frage ist jedoch, wie Sie angeben können, mit welcher Datenbank eine Verbindung hergestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1725a-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="1725a-129">Hierzu fügen Sie Verbindungsinformationen in der Datei " *Web. config* " der Anwendung hinzu.</span><span class="sxs-lookup"><span data-stu-id="1725a-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="1725a-130">Öffnen Sie die *Web. config* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1725a-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="1725a-131">(Nicht die Datei " *Web. config* " im Ordner " *views* ".) Öffnen Sie die Datei *Web. config* in rot.</span><span class="sxs-lookup"><span data-stu-id="1725a-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="1725a-132">Fügen Sie dem `<connectionStrings>`-Element in der Datei " *Web. config* " die folgende Verbindungs Zeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="1725a-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="1725a-133">Das folgende Beispiel zeigt einen Teil der Datei " *Web. config* ", in dem die neue Verbindungs Zeichenfolge hinzugefügt wurde:</span><span class="sxs-lookup"><span data-stu-id="1725a-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="1725a-134">Diese kleine Menge an Code und XML ist alles, was Sie schreiben müssen, um die Filmdaten in einer Datenbank darzustellen und zu speichern.</span><span class="sxs-lookup"><span data-stu-id="1725a-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="1725a-135">Als Nächstes erstellen Sie eine neue `MoviesController`-Klasse, die Sie verwenden können, um die Filmdaten anzuzeigen und Benutzern das Erstellen neuer Film Auflistungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="1725a-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1725a-136">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="1725a-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
