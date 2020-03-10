---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Teil 4: Modelle und Datenzugriff | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 4 werden Modelle und der Datenzugriff behandelt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451017"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="530d3-104">Teil 4: Modelle und Datenzugriff</span><span class="sxs-lookup"><span data-stu-id="530d3-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="530d3-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="530d3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="530d3-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="530d3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="530d3-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="530d3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="530d3-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="530d3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="530d3-109">In Teil 4 werden Modelle und der Datenzugriff behandelt.</span><span class="sxs-lookup"><span data-stu-id="530d3-109">Part 4 covers Models and Data Access.</span></span>

<span data-ttu-id="530d3-110">Bisher haben wir soeben "Dummy-Daten" von unseren Controllern an unsere Ansichts Vorlagen übergeben.</span><span class="sxs-lookup"><span data-stu-id="530d3-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="530d3-111">Jetzt sind wir bereit, eine echte Datenbank zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="530d3-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="530d3-112">In diesem Tutorial wird erläutert, wie Sie SQL Server Compact Edition (häufig als SQL CE bezeichnet) als Datenbank-Engine verwenden.</span><span class="sxs-lookup"><span data-stu-id="530d3-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="530d3-113">Bei SQL CE handelt es sich um eine kostenlose, eingebettete, dateibasierte Datenbank, für die keine Installation oder Konfiguration erforderlich ist, was für die lokale Entwicklung praktisch ist.</span><span class="sxs-lookup"><span data-stu-id="530d3-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="530d3-114">Datenbankzugriff mit Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="530d3-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="530d3-115">Wir verwenden die Unterstützung von Entity Framework (EF), die in ASP.NET MVC 3-Projekten enthalten ist, um die Datenbank abzufragen und zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="530d3-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="530d3-116">EF ist eine flexible ORM-Daten-API (Object Relational Mapping), mit der Entwickler in einer Datenbank gespeicherte Daten in objektorientierter Weise Abfragen und aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="530d3-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="530d3-117">Entity Framework Version 4 unterstützt ein Entwicklungsparadigma namens "Code First".</span><span class="sxs-lookup"><span data-stu-id="530d3-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="530d3-118">Code First ermöglicht das Erstellen eines Modell Objekts, indem einfache Klassen geschrieben werden (auch als poco aus "Plain-Old" CLR-Objekten bezeichnet), und die Datenbank kann sogar dynamisch von ihren Klassen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="530d3-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="530d3-119">Änderungen an unseren Modellklassen</span><span class="sxs-lookup"><span data-stu-id="530d3-119">Changes to our Model Classes</span></span>

<span data-ttu-id="530d3-120">Wir nutzen das Daten Bank Erstellungs Feature in Entity Framework in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="530d3-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="530d3-121">Bevor wir dies tun, nehmen wir jedoch einige geringfügige Änderungen an unseren Modellklassen vor, um einige Dinge hinzuzufügen, die wir später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="530d3-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="530d3-122">Hinzufügen der Klassen des Modell Modells</span><span class="sxs-lookup"><span data-stu-id="530d3-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="530d3-123">Unsere Alben werden Künstlern zugeordnet, daher fügen wir eine einfache Modell Klasse hinzu, um einen Künstler zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="530d3-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="530d3-124">Fügen Sie dem Ordner Models mithilfe des unten gezeigten Codes eine neue Klasse mit dem Namen Artist.cs hinzu.</span><span class="sxs-lookup"><span data-stu-id="530d3-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="530d3-125">Aktualisieren der Modellklassen</span><span class="sxs-lookup"><span data-stu-id="530d3-125">Updating our Model Classes</span></span>

<span data-ttu-id="530d3-126">Aktualisieren Sie die-Album Klasse wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="530d3-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="530d3-127">Nehmen Sie als nächstes die folgenden Aktualisierungen an der Genre-Klasse vor.</span><span class="sxs-lookup"><span data-stu-id="530d3-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a><span data-ttu-id="530d3-128">Hinzufügen der APP\_Datenordner</span><span class="sxs-lookup"><span data-stu-id="530d3-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="530d3-129">Wir fügen dem Projekt eine APP\_Datenverzeichnis hinzu, um unsere SQL Server Express Datenbankdateien zu speichern.</span><span class="sxs-lookup"><span data-stu-id="530d3-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="530d3-130">App-\_Daten sind ein spezielles Verzeichnis in ASP.net, das bereits über die erforderlichen Sicherheits Zugriffsberechtigungen für den Datenbankzugriff verfügt.</span><span class="sxs-lookup"><span data-stu-id="530d3-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="530d3-131">Wählen Sie im Menü Projekt die Option ASP.NET-Ordner hinzufügen und dann App-\_Daten aus.</span><span class="sxs-lookup"><span data-stu-id="530d3-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="530d3-132">Erstellen einer Verbindungs Zeichenfolge in der Datei "Web. config"</span><span class="sxs-lookup"><span data-stu-id="530d3-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="530d3-133">Wir fügen der Konfigurationsdatei der Website einige Zeilen hinzu, damit Entity Framework weiß, wie eine Verbindung mit der Datenbank hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="530d3-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="530d3-134">Doppelklicken Sie auf die Datei "Web. config", die sich im Stammverzeichnis des Projekts befindet.</span><span class="sxs-lookup"><span data-stu-id="530d3-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="530d3-135">Scrollen Sie zum Ende dieser Datei, und fügen Sie einen &lt;connectionStrings&gt; Abschnitt direkt oberhalb der letzten Zeile hinzu, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="530d3-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="530d3-136">Hinzufügen einer Kontext Klasse</span><span class="sxs-lookup"><span data-stu-id="530d3-136">Adding a Context Class</span></span>

<span data-ttu-id="530d3-137">Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine neue Klasse namens MusicStoreEntities.cs</span><span class="sxs-lookup"><span data-stu-id="530d3-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="530d3-138">Diese Klasse stellt den Daten Bank Kontext Entity Framework dar und übernimmt die Erstellungs-, Lese-, Aktualisierungs-und Löschvorgänge für uns.</span><span class="sxs-lookup"><span data-stu-id="530d3-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="530d3-139">Der Code für diese Klasse wird unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="530d3-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="530d3-140">Das ist das, es gibt keine andere Konfiguration, keine speziellen Schnittstellen usw. Durch die Erweiterung der dbcontext-Basisklasse kann unsere Klasse "musicstoreentities" unsere Daten Bank Vorgänge für uns verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="530d3-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="530d3-141">Nachdem wir nun damit verknüpft sind, fügen wir unseren Modellklassen einige weitere Eigenschaften hinzu, um einige der zusätzlichen Informationen in unserer Datenbank zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="530d3-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="530d3-142">Hinzufügen von Daten zum Store-Katalog</span><span class="sxs-lookup"><span data-stu-id="530d3-142">Adding our store catalog data</span></span>

<span data-ttu-id="530d3-143">Wir nutzen eine Funktion in Entity Framework, die einer neu erstellten Datenbank "Seed"-Daten hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="530d3-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="530d3-144">Dadurch wird der Store-Katalog mit einer Liste von Genres, Künstlern und Alben vorab aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="530d3-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="530d3-145">Der MvcMusicStore-Assets. zip-Download, der unsere zuvor in diesem Tutorial verwendeten Website Designdateien enthielt, verfügt über eine Klassendatei mit diesen Seed-Daten, die sich in einem Ordner mit dem Namen "Code" befindet.</span><span class="sxs-lookup"><span data-stu-id="530d3-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="530d3-146">Suchen Sie im Ordner Code/Models die Datei SampleData.cs, und legen Sie Sie im Ordner Models in unserem Projekt ab, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="530d3-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="530d3-147">Nun müssen wir eine Codezeile hinzufügen, um Entity Framework über die SampleData-Klasse zu informieren.</span><span class="sxs-lookup"><span data-stu-id="530d3-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="530d3-148">Doppelklicken Sie auf die Datei Global. asax im Stammverzeichnis des Projekts, um es zu öffnen, und fügen Sie die folgende Zeile am Anfang der Anwendung\_Start-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="530d3-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="530d3-149">An diesem Punkt haben wir die notwendigen Schritte zum Konfigurieren von Entity Framework für das Projekt abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="530d3-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="530d3-150">Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="530d3-150">Querying the Database</span></span>

<span data-ttu-id="530d3-151">Nun aktualisieren wir den StoreController, sodass anstelle der Verwendung von "Dummy-Daten" stattdessen die Datenbank aufgerufen wird, um alle Informationen abzufragen.</span><span class="sxs-lookup"><span data-stu-id="530d3-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="530d3-152">Wir beginnen mit der Deklaration eines Felds auf dem **StoreController** , das eine Instanz der Klasse "musicstoreentities" mit dem Namen "storedb" enthält:</span><span class="sxs-lookup"><span data-stu-id="530d3-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="530d3-153">Aktualisieren des Speicher Indexes zum Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="530d3-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="530d3-154">Die Klasse "musicstoreentities" wird vom Entity Framework verwaltet und macht eine Auflistungs Eigenschaft für jede Tabelle in der Datenbank verfügbar.</span><span class="sxs-lookup"><span data-stu-id="530d3-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="530d3-155">Wir aktualisieren nun die Index Aktion von StoreController, um alle Genres in unserer Datenbank abzurufen.</span><span class="sxs-lookup"><span data-stu-id="530d3-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="530d3-156">Zuvor haben wir die Zeichen folgen Daten hart codiert.</span><span class="sxs-lookup"><span data-stu-id="530d3-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="530d3-157">Nun können Sie einfach die Entity Framework Kontext Generations Auflistung verwenden:</span><span class="sxs-lookup"><span data-stu-id="530d3-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="530d3-158">Es müssen keine Änderungen an der Ansichts Vorlage vorgenommen werden, da wir immer noch das gleiche storeindexviewmodel zurückgeben, das wir zuvor zurückgegeben haben. Wir geben jetzt nur noch Livedaten aus unserer Datenbank zurück.</span><span class="sxs-lookup"><span data-stu-id="530d3-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="530d3-159">Wenn wir das Projekt erneut ausführen und die URL "/Store" aufrufen, sehen wir nun eine Liste aller Genres in unserer Datenbank:</span><span class="sxs-lookup"><span data-stu-id="530d3-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="530d3-160">Aktualisieren von Store-durchsuchen und Details zur Verwendung von Livedaten</span><span class="sxs-lookup"><span data-stu-id="530d3-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="530d3-161">Mit der Action-Methode/Store/Browse? Genre = *[some-Genre]* suchen wir nach einem Genre anhand des Namens.</span><span class="sxs-lookup"><span data-stu-id="530d3-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="530d3-162">Wir erwarten nur ein Ergebnis, da wir niemals zwei Einträge für denselben Genre Namen haben sollten, und wir können also das verwenden. Single ()-Erweiterung in LINQ, um das entsprechende Genre Objekt wie folgt abzufragen (geben Sie dies noch nicht ein):</span><span class="sxs-lookup"><span data-stu-id="530d3-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="530d3-163">Die Single-Methode nimmt einen Lambda-Ausdruck als Parameter an, der angibt, dass ein einzelnes Genre Objekt so ist, dass sein Name mit dem definierten Wert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="530d3-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="530d3-164">Im obigen Fall laden wir ein einzelnes Genre Objekt mit einem namens Wert, der Disco entspricht.</span><span class="sxs-lookup"><span data-stu-id="530d3-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="530d3-165">Wir nutzen ein Entity Framework Feature, das es uns ermöglicht, andere Verwandte Entitäten anzugeben, die wir ebenfalls laden möchten, wenn das Genre Objekt abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="530d3-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="530d3-166">Diese Funktion wird als Abfrageergebnis Strukturierung bezeichnet und ermöglicht es uns, die Anzahl der Zugriffe auf die Datenbank zu reduzieren, um alle benötigten Informationen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="530d3-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="530d3-167">Wir möchten die Alben für das Genre, das wir abrufen, vorab abrufen. daher aktualisieren wir unsere Abfrage so, dass Sie aus Genres. include ("Alben") enthalten ist, um anzugeben, dass wir auch Verwandte Alben benötigen.</span><span class="sxs-lookup"><span data-stu-id="530d3-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="530d3-168">Dies ist effizienter, da sowohl unsere Genre-als auch die Album Daten in einer einzelnen Daten Bank Anforderung abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="530d3-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="530d3-169">Nachfolgend finden Sie eine Erläuterung dazu, wie unsere aktualisierte Aktion zum Durchsuchen von Controllern aussieht:</span><span class="sxs-lookup"><span data-stu-id="530d3-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="530d3-170">Wir können nun die Ansicht "Store-durchsuchen" aktualisieren, um die in den einzelnen Genres verfügbaren Alben anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="530d3-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="530d3-171">Öffnen Sie die Ansichts Vorlage (in/views/Store/Browse.cshtml), und fügen Sie eine Auflistungs Liste der Alben hinzu, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="530d3-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="530d3-172">Wenn wir unsere Anwendung ausführen und zu/Store/Browse? Genre = Jazz navigieren, sehen wir, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden, sodass alle Alben in unserem ausgewählten Genre angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="530d3-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="530d3-173">Wir nehmen dieselbe Änderung an der/Store/Details/-URL (ID]) vor und ersetzen unsere Dummydaten durch eine Datenbankabfrage, die ein Album lädt, dessen ID mit dem Parameterwert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="530d3-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="530d3-174">Wenn Sie die Anwendung ausführen und zu/Store/Details/1 navigieren, sehen Sie, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="530d3-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="530d3-175">Nachdem die Seite "Store-Details" so eingerichtet ist, dass Sie ein Album anhand der Album-ID anzeigt, aktualisieren wir nun die **browseinsicht** , um eine Verknüpfung mit der Detailansicht zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="530d3-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="530d3-176">Wir verwenden "HTML. Action Link" genau wie bei der Verknüpfung aus dem Store-Index zum Speichern von durchsuchen am Ende des vorherigen Abschnitts.</span><span class="sxs-lookup"><span data-stu-id="530d3-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="530d3-177">Die gesamte Quelle für die browseinsicht wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="530d3-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="530d3-178">Wir können nun von unserer Store-Seite zu einer Genre Seite navigieren, die die verfügbaren Alben auflistet, und durch Klicken auf ein Album können wir Details zu diesem Album anzeigen.</span><span class="sxs-lookup"><span data-stu-id="530d3-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="530d3-179">[Zurück](mvc-music-store-part-3.md)
> [Weiter](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="530d3-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
