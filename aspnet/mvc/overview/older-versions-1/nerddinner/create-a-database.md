---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Erstellen einer Datenbank | Microsoft-Dokumentation
author: microsoft
description: Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, in der alle Dinner-und RSVP-Daten für unsere "nerddinner"-Anwendung enthalten sind.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469299"
---
# <a name="create-a-database"></a><span data-ttu-id="1d79d-103">Erstellen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="1d79d-103">Create a Database</span></span>

<span data-ttu-id="1d79d-104">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1d79d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1d79d-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="1d79d-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="1d79d-106">Dies ist Schritt 2 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="1d79d-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="1d79d-107">Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, in der alle Dinner-und RSVP-Daten für unsere "nerddinner"-Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="1d79d-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="1d79d-108">Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.</span><span class="sxs-lookup"><span data-stu-id="1d79d-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="1d79d-109">Nerddinner Step 2: Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="1d79d-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="1d79d-110">Wir verwenden eine Datenbank, um alle Dinner-und RSVP-Daten für unsere "nerddinner"-Anwendung zu speichern.</span><span class="sxs-lookup"><span data-stu-id="1d79d-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="1d79d-111">Die folgenden Schritte zeigen, wie Sie die Datenbank mit der kostenlosen SQL Server Express Edition erstellen (die Sie mithilfe von V2 des [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)problemlos installieren können).</span><span class="sxs-lookup"><span data-stu-id="1d79d-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="1d79d-112">Der gesamte Code, den wir schreiben, funktioniert sowohl mit SQL Server Express als auch mit der vollständigen SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1d79d-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="1d79d-113">Erstellen einer neuen SQL Server Express Datenbank</span><span class="sxs-lookup"><span data-stu-id="1d79d-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="1d79d-114">Beginnen Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie dann den Menübefehl **Add-&gt;New Item** :</span><span class="sxs-lookup"><span data-stu-id="1d79d-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="1d79d-115">Dadurch wird das Dialogfeld "Neues Element hinzufügen" in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1d79d-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="1d79d-116">Wir Filtern nach der Kategorie "Data" (Daten) und wählen die Element Vorlage "SQL Server Datenbank" aus:</span><span class="sxs-lookup"><span data-stu-id="1d79d-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="1d79d-117">Benennen Sie die SQL Server Express Datenbank, die wir erstellen möchten, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="1d79d-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="1d79d-118">In Visual Studio werden Sie gefragt, ob Sie diese Datei dem Verzeichnis \app\_-Daten hinzufügen möchten (bei dem es sich um ein Verzeichnis handelt, das bereits mit Lese-und Schreib Sicherheits-ACLs eingerichtet wurde):</span><span class="sxs-lookup"><span data-stu-id="1d79d-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="1d79d-119">Wir klicken auf "Ja", und die neue Datenbank wird erstellt und dem Projektmappen-Explorer hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="1d79d-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="1d79d-120">Erstellen von Tabellen in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="1d79d-120">Creating Tables within our Database</span></span>

<span data-ttu-id="1d79d-121">Wir verfügen nun über eine neue leere Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1d79d-121">We now have a new empty database.</span></span> <span data-ttu-id="1d79d-122">Fügen wir einige Tabellen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1d79d-122">Let's add some tables to it.</span></span>

<span data-ttu-id="1d79d-123">Zu diesem Zweck navigieren wir in Visual Studio zum Registerkarten Fenster "Server-Explorer", das es uns ermöglicht, Datenbanken und Server zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="1d79d-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="1d79d-124">SQL Server Express Datenbanken, die im Ordner \app\_Data der Anwendung gespeichert sind, werden automatisch innerhalb der Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1d79d-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="1d79d-125">Optional können Sie das Symbol "Verbindung mit Datenbank herstellen" oben im Fenster "Server-Explorer" verwenden, um der Liste zusätzliche SQL Server Datenbanken (sowohl lokal als auch Remote) hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="1d79d-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="1d79d-126">Wir fügen unserer "nerddinner Database" zwei Tabellen hinzu – eines zum Speichern unserer Abendessen und das andere, um die RSVP-Akzeptanz zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="1d79d-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="1d79d-127">Wir können neue Tabellen erstellen, indem Sie mit der rechten Maustaste auf den Ordner "Tables" in der Datenbank klicken und den Menübefehl "neue Tabelle hinzufügen" auswählen:</span><span class="sxs-lookup"><span data-stu-id="1d79d-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="1d79d-128">Dadurch wird ein Tabellen-Designer geöffnet, der es uns ermöglicht, das Schema der Tabelle zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1d79d-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="1d79d-129">In der Tabelle "Dinner" (Dinner) werden 10 Datenspalten hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="1d79d-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="1d79d-130">Wir möchten, dass die Spalte "dinnerid" ein eindeutiger Primärschlüssel für die Tabelle ist.</span><span class="sxs-lookup"><span data-stu-id="1d79d-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="1d79d-131">Wir können dies konfigurieren, indem Sie mit der rechten Maustaste auf die Spalte "dinnerid" klicken und das Menü Element "Primärschlüssel festlegen" auswählen:</span><span class="sxs-lookup"><span data-stu-id="1d79d-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="1d79d-132">Zusätzlich zum Erstellen von dinnerid als Primärschlüssel soll die Spalte auch als Identitäts Spalte konfiguriert werden, deren Wert automatisch erhöht wird, wenn der Tabelle neue Daten Zeilen hinzugefügt werden (was bedeutet, dass die erste eingefügte Dinner-Zeile eine dinnerid von 1, die zweite eingefügte Zeile hat). hat eine dinnerid von 2 usw.).</span><span class="sxs-lookup"><span data-stu-id="1d79d-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="1d79d-133">Wählen Sie hierzu die Spalte "dinnerid" aus, und legen Sie dann mit dem Editor "Spalten Eigenschaften" die Eigenschaft "(ist Identity)" für die Spalte auf "yes" fest.</span><span class="sxs-lookup"><span data-stu-id="1d79d-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="1d79d-134">Wir verwenden die Standardeinstellungen der Identität (Start bei 1 und Inkrement 1 für jede neue Dinner-Zeile):</span><span class="sxs-lookup"><span data-stu-id="1d79d-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="1d79d-135">Anschließend speichern wir die Tabelle durch Eingabe von STRG + S oder mithilfe des Menübefehls **File-&gt;Save** .</span><span class="sxs-lookup"><span data-stu-id="1d79d-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="1d79d-136">Dadurch werden wir aufgefordert, die Tabelle zu benennen.</span><span class="sxs-lookup"><span data-stu-id="1d79d-136">This will prompt us to name the table.</span></span> <span data-ttu-id="1d79d-137">Wir nennen es "Dinner":</span><span class="sxs-lookup"><span data-stu-id="1d79d-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="1d79d-138">Unsere neue Dinner-Tabelle wird dann im Server-Explorer in unserer Datenbank angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1d79d-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="1d79d-139">Anschließend wiederholen Sie die obigen Schritte und erstellen eine "RSVP"-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="1d79d-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="1d79d-140">Diese Tabelle verfügt über drei Spalten.</span><span class="sxs-lookup"><span data-stu-id="1d79d-140">This table with have 3 columns.</span></span> <span data-ttu-id="1d79d-141">Wir richten die rsvpid-Spalte als Primärschlüssel ein und machen Sie auch zu einer Identitäts Spalte:</span><span class="sxs-lookup"><span data-stu-id="1d79d-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="1d79d-142">Wir speichern Sie und nennen Sie den Namen "RSVP".</span><span class="sxs-lookup"><span data-stu-id="1d79d-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="1d79d-143">Einrichten einer Fremdschlüssel Beziehung zwischen Tabellen</span><span class="sxs-lookup"><span data-stu-id="1d79d-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="1d79d-144">Wir verfügen jetzt über zwei Tabellen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1d79d-144">We now have two tables within our database.</span></span> <span data-ttu-id="1d79d-145">Der letzte Schema Entwurfs Schritt ist das Einrichten einer 1: n-Beziehung zwischen diesen beiden Tabellen – damit wir jede Dinner-Zeile mit 0 (null) oder mehr RSVP-Zeilen verknüpfen können, die für Sie gelten.</span><span class="sxs-lookup"><span data-stu-id="1d79d-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="1d79d-146">Hierzu konfigurieren Sie die Spalte "dinnerid" der RSVP-Tabelle, um eine Fremdschlüssel Beziehung mit der Spalte "dinnerid" in der Tabelle "Dinner" zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="1d79d-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="1d79d-147">Zu diesem Zweck öffnen Sie die RSVP-Tabelle im Tabellen-Designer, indem Sie im Server-Explorer darauf doppelklicken.</span><span class="sxs-lookup"><span data-stu-id="1d79d-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="1d79d-148">Wählen Sie dann die Spalte "dinnerid" darin aus, klicken Sie mit der rechten Maustaste, und wählen Sie "Beziehungen..." aus. Kontextmenü Befehl:</span><span class="sxs-lookup"><span data-stu-id="1d79d-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="1d79d-149">Dadurch wird ein Dialogfeld angezeigt, das zum Einrichten von Beziehungen zwischen Tabellen verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="1d79d-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="1d79d-150">Wir klicken auf die Schaltfläche "hinzufügen", um eine neue Beziehung zum Dialogfeld hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1d79d-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="1d79d-151">Nachdem eine Beziehung hinzugefügt wurde, erweitern wir den Strukturansicht-Knoten "Tabellen-und Spaltenspezifikation" innerhalb des Eigenschaften Rasters rechts neben dem Dialogfeld, und klicken Sie dann auf "..." rechts neben der Schaltfläche:</span><span class="sxs-lookup"><span data-stu-id="1d79d-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="1d79d-152">Klicken auf die Schaltfläche "..." Schaltfläche öffnet ein weiteres Dialogfeld, in dem Sie angeben können, welche Tabellen und Spalten an der Beziehung beteiligt sind, und dass wir die Beziehung benennen können.</span><span class="sxs-lookup"><span data-stu-id="1d79d-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="1d79d-153">Die Primärschlüssel Tabelle wird in "Dinner" geändert, und die Spalte "dinnerid" in der Tabelle "Dinner" wird als Primärschlüssel ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="1d79d-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="1d79d-154">Unsere RSVP-Tabelle ist die Fremdschlüssel Tabelle und die RSVP. Die dinnerid-Spalte wird als Fremdschlüssel zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="1d79d-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="1d79d-155">Nun wird jede Zeile in der RSVP-Tabelle einer Zeile in der Dinner-Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1d79d-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="1d79d-156">SQL Server behält die referenzielle Integrität für uns – und hindert uns daran, eine neue RSVP-Zeile hinzuzufügen, wenn Sie nicht auf eine gültige Dinner-Zeile verweist.</span><span class="sxs-lookup"><span data-stu-id="1d79d-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="1d79d-157">Außerdem wird verhindert, dass wir eine Dinner-Zeile löschen, wenn noch RSVP-Zeilen darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="1d79d-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="1d79d-158">Hinzufügen von Daten zu den Tabellen</span><span class="sxs-lookup"><span data-stu-id="1d79d-158">Adding Data to our Tables</span></span>

<span data-ttu-id="1d79d-159">Abschließend fügen wir einige Beispiel Daten zu unserer Dinner-Tabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="1d79d-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="1d79d-160">Wir können einer Tabelle Daten hinzufügen, indem Sie im Server-Explorer mit der rechten Maustaste darauf klicken und den Befehl "Tabellendaten anzeigen" auswählen:</span><span class="sxs-lookup"><span data-stu-id="1d79d-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="1d79d-161">Wir fügen einige Zeilen mit Dinner Data hinzu, die Sie später verwenden können, wenn wir mit der Implementierung der Anwendung beginnen:</span><span class="sxs-lookup"><span data-stu-id="1d79d-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="1d79d-162">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="1d79d-162">Next Step</span></span>

<span data-ttu-id="1d79d-163">Die Erstellung unserer Datenbank ist abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="1d79d-163">We've finished creating our database.</span></span> <span data-ttu-id="1d79d-164">Nun erstellen wir Modellklassen, die Sie verwenden können, um Sie abzufragen und zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="1d79d-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1d79d-165">[Zurück](create-a-new-aspnet-mvc-project.md)
> [Weiter](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="1d79d-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
