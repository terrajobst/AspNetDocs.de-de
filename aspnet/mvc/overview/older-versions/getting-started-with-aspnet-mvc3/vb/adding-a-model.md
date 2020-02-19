---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Hinzufügen eines Modells (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457244"
---
# <a name="adding-a-model-vb"></a><span data-ttu-id="3e933-103">Hinzufügen eines Modells (VB)</span><span class="sxs-lookup"><span data-stu-id="3e933-103">Adding a Model (VB)</span></span>

<span data-ttu-id="3e933-104">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3e933-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="3e933-105">Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e933-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="3e933-106">Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="3e933-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="3e933-107">Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="3e933-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="3e933-108">Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:</span><span class="sxs-lookup"><span data-stu-id="3e933-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="3e933-109">Erforderliche Komponenten für Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="3e933-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="3e933-110">ASP.NET MVC 3-Tools aktualisieren</span><span class="sxs-lookup"><span data-stu-id="3e933-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="3e933-111">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)</span><span class="sxs-lookup"><span data-stu-id="3e933-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="3e933-112">Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="3e933-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="3e933-113">Ein Visual Web Developer-Projekt mit VB.NET-Quellcode ist für dieses Thema verfügbar.</span><span class="sxs-lookup"><span data-stu-id="3e933-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="3e933-114">[Laden Sie die VB.NET-Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="3e933-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="3e933-115">Wechseln Sie C#ggf. zur [ C# Version](../cs/adding-a-model.md) dieses Tutorials.</span><span class="sxs-lookup"><span data-stu-id="3e933-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="3e933-116">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="3e933-116">Adding a Model</span></span>

<span data-ttu-id="3e933-117">In diesem Abschnitt fügen Sie einige Klassen zum Verwalten von Filmen in einer-Datenbank hinzu.</span><span class="sxs-lookup"><span data-stu-id="3e933-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="3e933-118">Diese Klassen werden als "Modell"-Teil der ASP.NET MVC-Anwendung verwendet.</span><span class="sxs-lookup"><span data-stu-id="3e933-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="3e933-119">Sie verwenden eine .NET Framework Datenzugriffs Technologie, die als Entity Framework bezeichnet wird, um diese Modellklassen zu definieren und mit Ihnen zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="3e933-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="3e933-120">Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklungsparadigma namens *Code First*.</span><span class="sxs-lookup"><span data-stu-id="3e933-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="3e933-121">Code First ermöglicht es Ihnen, Modell Objekte zu erstellen, indem einfache Klassen geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="3e933-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="3e933-122">(Diese werden auch als poco-Klassen bezeichnet, von "Plain-Old CLR Objects".) Anschließend können Sie die Datenbank dynamisch von ihren Klassen erstellen lassen, wodurch ein sehr sauberer und schneller Entwicklungs Workflow ermöglicht wird.</span><span class="sxs-lookup"><span data-stu-id="3e933-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="3e933-123">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="3e933-123">Adding Model Classes</span></span>

<span data-ttu-id="3e933-124">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , und wählen Sie **Hinzufügen**und dann **Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="3e933-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="3e933-125">Benennen Sie die Klasse "Movie".</span><span class="sxs-lookup"><span data-stu-id="3e933-125">Name the class "Movie".</span></span>

<span data-ttu-id="3e933-126">Fügen Sie der `Movie`-Klasse die folgenden fünf Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="3e933-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="3e933-127">Wir verwenden die `Movie`-Klasse, um Filme in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="3e933-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="3e933-128">Jede Instanz eines `Movie` Objekts entspricht einer Zeile in einer Datenbanktabelle, und jede Eigenschaft der `Movie` Klasse wird einer Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3e933-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="3e933-129">Fügen Sie in der gleichen Datei die folgende `MovieDBContext`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="3e933-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="3e933-130">Die `MovieDBContext`-Klasse stellt den Entity Framework Movie-Daten Bank Kontext dar, der das Abrufen, speichern und Aktualisieren von `Movie`-Klassen Instanzen in einer Datenbank übernimmt.</span><span class="sxs-lookup"><span data-stu-id="3e933-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="3e933-131">Die `MovieDBContext` wird von der `DbContext` Basisklasse abgeleitet, die vom Entity Framework bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="3e933-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="3e933-132">Weitere Informationen zu `DbContext` und `DbSet`finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="3e933-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="3e933-133">Um auf `DbContext` und `DbSet`verweisen zu können, müssen Sie am Anfang der Datei die folgende `imports`-Anweisung hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="3e933-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="3e933-134">Die komplette Datei *Movie. vb* ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="3e933-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="3e933-135">Erstellen einer Verbindungs Zeichenfolge und arbeiten mit SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="3e933-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="3e933-136">Die von Ihnen erstellte `MovieDBContext`-Klasse übernimmt die Aufgabe der Verbindung mit der Datenbank und die Zuordnung von `Movie` Objekten zu Datenbankdaten Sätzen.</span><span class="sxs-lookup"><span data-stu-id="3e933-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="3e933-137">Eine Frage ist jedoch, wie Sie angeben können, mit welcher Datenbank eine Verbindung hergestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="3e933-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="3e933-138">Hierzu fügen Sie Verbindungsinformationen in der Datei " *Web. config* " der Anwendung hinzu.</span><span class="sxs-lookup"><span data-stu-id="3e933-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="3e933-139">Öffnen Sie die *Web. config* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3e933-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="3e933-140">(Nicht die Datei " *Web. config* " im Ordner " *views* ".) Die folgende Abbildung zeigt beide *Web. config* -Dateien. Öffnen Sie die Datei *Web. config* in rot.</span><span class="sxs-lookup"><span data-stu-id="3e933-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="3e933-141">Fügen Sie dem `<connectionStrings>`-Element in der Datei " *Web. config* " die folgende Verbindungs Zeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="3e933-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="3e933-142">Das folgende Beispiel zeigt einen Teil der Datei " *Web. config* ", in dem die neue Verbindungs Zeichenfolge hinzugefügt wurde:</span><span class="sxs-lookup"><span data-stu-id="3e933-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="3e933-143">Diese kleine Menge an Code und XML ist alles, was Sie schreiben müssen, um die Filmdaten in einer Datenbank darzustellen und zu speichern.</span><span class="sxs-lookup"><span data-stu-id="3e933-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="3e933-144">Als Nächstes erstellen Sie eine neue `MoviesController`-Klasse, die Sie verwenden können, um die Filmdaten anzuzeigen und Benutzern das Erstellen neuer Film Auflistungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="3e933-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e933-145">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="3e933-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
