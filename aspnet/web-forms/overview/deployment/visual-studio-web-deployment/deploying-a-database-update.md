---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636833"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="18d16-103">ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="18d16-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>

<span data-ttu-id="18d16-104">von [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="18d16-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="18d16-105">Starter Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="18d16-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="18d16-106">In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="18d16-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="18d16-107">Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="18d16-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="18d16-108">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="18d16-108">Overview</span></span>

<span data-ttu-id="18d16-109">In diesem Tutorial erstellen Sie eine Daten Bank Änderung und zugehörige Codeänderungen, testen die Änderungen in Visual Studio und stellen dann das Update für die Test-, Staging-und Produktionsumgebungen bereit.</span><span class="sxs-lookup"><span data-stu-id="18d16-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="18d16-110">Das Tutorial zeigt zunächst, wie eine Datenbank aktualisiert wird, die von Code First-Migrationen verwaltet wird, und anschließend wird gezeigt, wie eine Datenbank mithilfe des dbdacfx-Anbieters aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="18d16-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="18d16-111">Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](troubleshooting.md)Behandlung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="18d16-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="18d16-112">Bereitstellen eines Datenbankupdates mithilfe Code First-Migrationen</span><span class="sxs-lookup"><span data-stu-id="18d16-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="18d16-113">In diesem Abschnitt fügen Sie der `Person`-Basisklasse für die Entitäten `Student` und `Instructor` eine Geburtsdatums Spalte hinzu.</span><span class="sxs-lookup"><span data-stu-id="18d16-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="18d16-114">Anschließend aktualisieren Sie die Seite, auf der Dozenten Daten angezeigt werden, damit die neue Spalte angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="18d16-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="18d16-115">Zum Schluss stellen Sie die Änderungen in den Test-, Staging-und Produktionsumgebungen bereit.</span><span class="sxs-lookup"><span data-stu-id="18d16-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="18d16-116">Hinzufügen einer Spalte zu einer Tabelle in der Anwendungsdatenbank</span><span class="sxs-lookup"><span data-stu-id="18d16-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="18d16-117">Öffnen Sie im Projekt *contosouniversity. dal* *Person.cs* , und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse hinzu (danach müssen zwei schließende geschweifte Klammern vorhanden sein):</span><span class="sxs-lookup"><span data-stu-id="18d16-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="18d16-118">Aktualisieren Sie als nächstes die `Seed`-Methode, sodass Sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="18d16-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="18d16-119">Öffnen Sie *migrations\configuration.cs* , und ersetzen Sie den Code Block, der `var instructors = new List<Instructor>` beginnt, durch den folgenden Codeblock, der Geburtsdatum-Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="18d16-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="18d16-120">Erstellen Sie die Projekt Mappe, und öffnen Sie dann das Fenster **Paket-Manager-Konsole** .</span><span class="sxs-lookup"><span data-stu-id="18d16-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="18d16-121">Stellen Sie sicher, dass "conjesouniversity. dal" weiterhin als **Standard Projekt**ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="18d16-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="18d16-122">Wählen Sie im Fenster **Paket-Manager-Konsole** die Option **condesouniversity. dal** als **Standard Projekt**aus, und geben Sie dann den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="18d16-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="18d16-123">Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration`-Klasse definiert, und in der `Up`-Methode sehen Sie den Code, der die neue Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="18d16-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="18d16-124">Die `Up`-Methode erstellt die-Spalte, wenn Sie die Änderung implementieren, und die `Down`-Methode löscht die-Spalte, wenn Sie ein Rollback der Änderung durchführen.</span><span class="sxs-lookup"><span data-stu-id="18d16-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="18d16-126">Erstellen Sie die Projekt Mappe, und geben Sie dann den folgenden Befehl in das Fenster der **Paket-Manager-Konsole** ein (stellen Sie sicher, dass das Projekt condesouniversity. dal noch ausgewählt ist):</span><span class="sxs-lookup"><span data-stu-id="18d16-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="18d16-127">Der Entity Framework führt die `Up`-Methode aus und führt dann die `Seed`-Methode aus.</span><span class="sxs-lookup"><span data-stu-id="18d16-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="18d16-128">Anzeigen der neuen Spalte auf der Seite "Dozenten"</span><span class="sxs-lookup"><span data-stu-id="18d16-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="18d16-129">Öffnen Sie im Projekt contosouniversity die Datei " *Dozenten. aspx* ", und fügen Sie ein neues Vorlagen Feld hinzu, um das Geburtsdatum anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="18d16-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="18d16-130">Fügen Sie diese zwischen den Einstellungen für das Einstellungs Datum und die Büro Zuweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="18d16-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="18d16-131">(Wenn der Code Einzug nicht synchronisiert wird, können Sie STRG + K drücken und dann STRG + D drücken, um die Datei automatisch neu zu formatieren.)</span><span class="sxs-lookup"><span data-stu-id="18d16-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="18d16-132">Führen Sie die Anwendung aus, und klicken Sie auf den Link **Dozenten** .</span><span class="sxs-lookup"><span data-stu-id="18d16-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="18d16-133">Wenn die Seite geladen wird, sehen Sie, dass Sie das neue Feld Geburtsdatum enthält.</span><span class="sxs-lookup"><span data-stu-id="18d16-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Dozenten Seite mit BirthDate](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="18d16-135">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="18d16-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="18d16-136">Datenbankupdate bereitstellen</span><span class="sxs-lookup"><span data-stu-id="18d16-136">Deploy the database update</span></span>

1. <span data-ttu-id="18d16-137">Wählen Sie in **Projektmappen-Explorer** das Projekt conjesouniversity aus.</span><span class="sxs-lookup"><span data-stu-id="18d16-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="18d16-138">Klicken Sie im **Web** auf die Symbolleiste veröffentlichen, klicken Sie **auf das Profil** Veröffentlichungs Profil, und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="18d16-139">(Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt condesouniversity in **Projektmappen-Explorer**aus.)</span><span class="sxs-lookup"><span data-stu-id="18d16-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="18d16-140">Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser wird mit der Startseite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="18d16-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="18d16-141">Führen Sie die Seite **Dozenten** aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="18d16-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="18d16-142">Wenn die Anwendung versucht, auf die Datenbank für diese Seite zuzugreifen, aktualisiert Code First das Datenbankschema und führt die `Seed`-Methode aus.</span><span class="sxs-lookup"><span data-stu-id="18d16-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="18d16-143">Wenn die Seite angezeigt wird, wird die Spalte erwarteter **Geburtsdatum** mit Datumsangaben angezeigt.</span><span class="sxs-lookup"><span data-stu-id="18d16-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="18d16-144">Klicken Sie im **Web** auf die Symbolleiste veröffentlichen, klicken Sie **auf das** stagingveröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="18d16-145">Führen Sie die Seite **Dozenten** im Staging aus, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="18d16-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="18d16-146">Klicken Sie im Web auf die Symbolleiste **veröffentlichen** , klicken Sie auf das Veröffentlichungs Profil für die **Produktion** , und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="18d16-147">Führen Sie die Seite **Dozenten** in der Produktion aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="18d16-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="18d16-148">Bei einem realen Produktions Anwendungs Update, das eine Daten Bank Änderung umfasst, würden Sie die Anwendung normalerweise während der Bereitstellung offline schalten, indem Sie *App\_offline. htm*verwenden, wie im vorherigen Tutorial gezeigt.</span><span class="sxs-lookup"><span data-stu-id="18d16-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="18d16-149">Bereitstellen eines Datenbankupdates mithilfe des dbdacfx-Anbieters</span><span class="sxs-lookup"><span data-stu-id="18d16-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="18d16-150">In diesem Abschnitt fügen Sie der *Benutzer* Tabelle in der Mitgliedschafts Datenbank eine Spalte " *Kommentare* " hinzu und erstellen eine Seite, auf der Sie Kommentare für jeden Benutzer anzeigen und bearbeiten können.</span><span class="sxs-lookup"><span data-stu-id="18d16-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="18d16-151">Anschließend stellen Sie die Änderungen in den Test-, Staging-und Produktionsumgebungen bereit.</span><span class="sxs-lookup"><span data-stu-id="18d16-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="18d16-152">Hinzufügen einer Spalte zu einer Tabelle in der Mitgliedschafts Datenbank</span><span class="sxs-lookup"><span data-stu-id="18d16-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="18d16-153">Öffnen Sie in Visual Studio **SQL Server-Objekt-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="18d16-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="18d16-154">Erweitern Sie **(localdb) \v11.0**, erweitern Sie **Datenbanken**, erweitern Sie **ASPNET-contosouniversity** (nicht **ASPNET-contosouniversity-Prod**), und erweitern Sie dann **Tabellen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="18d16-155">Wenn **(localdb) \v11.0** unter dem Knoten **SQL Server** nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf den Knoten **SQL Server** , und klicken Sie dann auf **SQL Server hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="18d16-156">Geben Sie im Dialogfeld **Verbindung mit Server herstellen** *(localdb) \v11.0* als **Server Namen**ein, und klicken Sie dann auf **verbinden**.</span><span class="sxs-lookup"><span data-stu-id="18d16-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="18d16-157">Wenn **ASPNET-contosouniversity**nicht angezeigt wird, führen Sie das Projekt aus, und melden Sie sich mit den *Administrator* Anmelde Informationen (Kennwort ist *devpwd*) an, und aktualisieren Sie dann das Fenster **SQL Server-Objekt-Explorer** .</span><span class="sxs-lookup"><span data-stu-id="18d16-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="18d16-158">Klicken Sie mit der rechten Maustaste auf die Tabelle **Benutzer** , und klicken Sie dann auf **Designer anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Ssox-Sicht-Designer](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="18d16-160">Fügen Sie im Designer eine Spalte " *Kommentare* " hinzu, und legen Sie **Sie auf "** *nvarchar (128)* " und "Nullable" fest.</span><span class="sxs-lookup"><span data-stu-id="18d16-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Hinzufügen von Kommentarspalten](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="18d16-162">Klicken Sie im Feld **Vorschau der Daten Bank Updates** auf **Datenbank aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="18d16-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Vorschau der Daten Bank Updates](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="18d16-164">Erstellen einer Seite zum Anzeigen und Bearbeiten der neuen Spalte</span><span class="sxs-lookup"><span data-stu-id="18d16-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="18d16-165">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den **Konto** Ordner im Projekt contosouniversity, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="18d16-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="18d16-166">Erstellen Sie ein neues **Webformular mithilfe der Master Seite** , und nennen Sie es " *userinfo. aspx*".</span><span class="sxs-lookup"><span data-stu-id="18d16-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="18d16-167">Akzeptieren Sie die Standarddatei " *Site. Master* " als Master Seite.</span><span class="sxs-lookup"><span data-stu-id="18d16-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="18d16-168">Kopieren Sie das folgende Markup in das `MainContent` `Content`-Element (die letzten 3 `Content` Elemente):</span><span class="sxs-lookup"><span data-stu-id="18d16-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="18d16-169">Klicken Sie mit der rechten Maustaste auf die Seite *userinfo. aspx* , und klicken Sie auf **in Browser anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="18d16-170">Melden Sie sich mit Ihren *Administrator* Anmelde Informationen an (Kennwort ist *devpwd*), und fügen Sie einem Benutzer einige Kommentare hinzu, um zu überprüfen, ob die Seite ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="18d16-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Userinfo-Seite](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="18d16-172">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="18d16-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="18d16-173">Datenbankupdate bereitstellen</span><span class="sxs-lookup"><span data-stu-id="18d16-173">Deploy the database update</span></span>

<span data-ttu-id="18d16-174">Zum Bereitstellen von mithilfe des dbdacfx-Anbieters müssen Sie nur die Option **Datenbank aktualisieren** im Veröffentlichungs Profil auswählen.</span><span class="sxs-lookup"><span data-stu-id="18d16-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="18d16-175">Allerdings haben Sie für die erste Bereitstellung, bei der Sie diese Option verwendet haben, auch einige zusätzliche SQL-Skripts für die Ausführung konfiguriert: Diese befinden sich noch im Profil, und Sie müssen verhindern, dass Sie erneut ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="18d16-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="18d16-176">Öffnen Sie den Assistenten **Web veröffentlichen** , indem Sie mit der rechten Maustaste auf das Projekt contosouniversity und dann auf **veröffentlichen**klicken.</span><span class="sxs-lookup"><span data-stu-id="18d16-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="18d16-177">Wählen Sie das **Test** Profil aus.</span><span class="sxs-lookup"><span data-stu-id="18d16-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="18d16-178">Klicken Sie auf die Registerkarte **Einstellungen** .</span><span class="sxs-lookup"><span data-stu-id="18d16-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="18d16-179">Wählen Sie unter **DefaultConnection**die Option **Datenbank aktualisieren**aus.</span><span class="sxs-lookup"><span data-stu-id="18d16-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="18d16-180">Deaktivieren Sie die zusätzlichen Skripts, die Sie für die erste Bereitstellung konfiguriert haben:</span><span class="sxs-lookup"><span data-stu-id="18d16-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="18d16-181">Klicken Sie auf **Datenbankupdates konfigurieren**.</span><span class="sxs-lookup"><span data-stu-id="18d16-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="18d16-182">Deaktivieren Sie im Dialogfeld **Daten Bank Updates konfigurieren** die Kontrollkästchen neben *Grant. SQL* und *ASPNET-Data-dev. SQL*.</span><span class="sxs-lookup"><span data-stu-id="18d16-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="18d16-183">Klicken Sie auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-183">Click **Close**.</span></span>
6. <span data-ttu-id="18d16-184">Klicken Sie auf die Registerkarte **Vorschau** .</span><span class="sxs-lookup"><span data-stu-id="18d16-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="18d16-185">Klicken Sie unter **Datenbanken** und rechts neben **DefaultConnection**auf den Link **Datenbank anzeigen** .</span><span class="sxs-lookup"><span data-stu-id="18d16-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Datenbank-Vorschau](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="18d16-187">Im Vorschaufenster wird das Skript angezeigt, das in der Zieldatenbank ausgeführt wird, um zu erreichen, dass das Datenbankschema dem Schema der Quelldatenbank entspricht.</span><span class="sxs-lookup"><span data-stu-id="18d16-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="18d16-188">Das Skript enthält einen ALTER TABLE-Befehl, der die neue Spalte hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="18d16-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="18d16-189">Schließen Sie das Dialogfeld **Datenbank-Vorschau** , und klicken Sie dann auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="18d16-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="18d16-190">Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser wird mit der Startseite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="18d16-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="18d16-191">Führen Sie die userinfo-Seite ( *Konto/userinfo. aspx* zur URL der Startseite hinzufügen) aus, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="18d16-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="18d16-192">Sie müssen sich anmelden, indem Sie *Admin* und *devpwd*eingeben.</span><span class="sxs-lookup"><span data-stu-id="18d16-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="18d16-193">Daten in Tabellen werden nicht standardmäßig bereitgestellt, und Sie haben kein Skript für die Datenbereitstellung für die Durchführung konfiguriert, sodass Sie den Kommentar nicht finden, den Sie in der Entwicklung hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="18d16-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="18d16-194">Sie können jetzt einen neuen Kommentar in der Stagingumgebung hinzufügen, um sicherzustellen, dass die Änderung für die Datenbank bereitgestellt wurde und die Seite ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="18d16-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="18d16-195">Befolgen Sie das gleiche Verfahren für die Bereitstellung für Staging und Produktion.</span><span class="sxs-lookup"><span data-stu-id="18d16-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="18d16-196">Vergessen Sie nicht, die zusätzlichen Skripts zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="18d16-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="18d16-197">Der einzige Unterschied im Vergleich zum Test Profil besteht darin, dass Sie nur ein Skript in den Staging-und Produktions Profilen deaktivieren, da Sie so konfiguriert wurden, dass nur *ASPNET-Prod-Data. SQL*ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="18d16-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="18d16-198">Die Anmelde Informationen für Staging und Produktion lauten admin und prodpwd.</span><span class="sxs-lookup"><span data-stu-id="18d16-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="18d16-199">Bei einem realen Produktions Anwendungs Update, das eine Daten Bank Änderung umfasst, würden Sie die Anwendung normalerweise während der Bereitstellung offline schalten, indem Sie *App-\_offline. htm* hochladen, bevor Sie Sie veröffentlichen und anschließend löschen, wie im [vorherigen Tutorial](deploying-a-code-update.md)gezeigt.</span><span class="sxs-lookup"><span data-stu-id="18d16-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="18d16-200">Summary</span><span class="sxs-lookup"><span data-stu-id="18d16-200">Summary</span></span>

<span data-ttu-id="18d16-201">Sie haben nun ein Anwendungs Update bereitgestellt, das eine Daten Bank Änderung mithilfe Code First-Migrationen und des dbdacfx-Anbieters enthielt.</span><span class="sxs-lookup"><span data-stu-id="18d16-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Dozenten Seite mit BirthDate](deploying-a-database-update/_static/image8.png)

![Userinfo-Seite](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="18d16-204">Im nächsten Tutorial erfahren Sie, wie Sie bereit Stellungen über die Befehlszeile ausführen.</span><span class="sxs-lookup"><span data-stu-id="18d16-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="18d16-205">[Zurück](deploying-a-code-update.md)
> [Weiter](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="18d16-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
