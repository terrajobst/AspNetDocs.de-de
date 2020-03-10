---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Teil 4: Hinzufügen einer admin-Ansicht | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447879"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="7161f-102">Teil 4: Hinzufügen einer Administrator Ansicht</span><span class="sxs-lookup"><span data-stu-id="7161f-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="7161f-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7161f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7161f-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="7161f-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="7161f-105">Administrator Ansicht hinzufügen</span><span class="sxs-lookup"><span data-stu-id="7161f-105">Add an Admin View</span></span>

<span data-ttu-id="7161f-106">Nun wird die Clientseite und eine Seite hinzugefügt, die Daten vom Administrator Controller verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="7161f-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="7161f-107">Die Seite ermöglicht Benutzern das Erstellen, bearbeiten oder Löschen von Produkten, indem AJAX-Anforderungen an den Controller gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="7161f-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="7161f-108">Erweitern Sie in Projektmappen-Explorer den Ordner Controllers, und öffnen Sie die Datei mit dem Namen HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="7161f-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="7161f-109">Diese Datei enthält einen MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="7161f-109">This file contains an MVC controller.</span></span> <span data-ttu-id="7161f-110">Fügen Sie eine Methode mit dem Namen `Admin`hinzu:</span><span class="sxs-lookup"><span data-stu-id="7161f-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="7161f-111">Die **httprouteurl** -Methode erstellt den URI für die Web-API, und wir speichern diese später in der Ansichts Tasche.</span><span class="sxs-lookup"><span data-stu-id="7161f-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="7161f-112">Positionieren Sie als nächstes den Textcursor innerhalb der `Admin` Aktionsmethode, klicken Sie mit der rechten Maustaste, und wählen Sie **Ansicht hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="7161f-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="7161f-113">Dadurch wird das Dialog **Feld Ansicht hinzufügen** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7161f-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="7161f-114">Benennen Sie im Dialog **Feld Ansicht hinzufügen** die Ansicht "admin".</span><span class="sxs-lookup"><span data-stu-id="7161f-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="7161f-115">Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7161f-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="7161f-116">Wählen Sie unter **Modell Klasse**die Option "Product (productstore. Models)" aus.</span><span class="sxs-lookup"><span data-stu-id="7161f-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="7161f-117">Belassen Sie alle anderen Optionen als Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="7161f-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="7161f-118">Wenn **Sie auf Hinzufügen** klicken, wird eine Datei namens admin. cshtml unter views/Home hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7161f-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="7161f-119">Öffnen Sie diese Datei, und fügen Sie folgenden HTML-Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="7161f-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="7161f-120">Dieser HTML-Code definiert die Struktur der Seite, aber noch keine Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="7161f-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="7161f-121">Erstellen eines Links zur Administrator Seite</span><span class="sxs-lookup"><span data-stu-id="7161f-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="7161f-122">Erweitern Sie in Projektmappen-Explorer den Ordner views, und erweitern Sie dann den freigegebenen Ordner.</span><span class="sxs-lookup"><span data-stu-id="7161f-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="7161f-123">Öffnen Sie die Datei mit dem Namen \_"Layout. cshtml".</span><span class="sxs-lookup"><span data-stu-id="7161f-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="7161f-124">Suchen Sie das **UL** -Element mit der ID = "Menu" und einen Aktions Link für die Administrator Ansicht:</span><span class="sxs-lookup"><span data-stu-id="7161f-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="7161f-125">Im Beispiel Projekt habe ich einige andere kosmetische Änderungen vorgenommen, z. b. die Zeichenfolge "Ihr Logo hier" zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="7161f-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="7161f-126">Diese wirken sich nicht auf die Funktionalität der Anwendung aus.</span><span class="sxs-lookup"><span data-stu-id="7161f-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="7161f-127">Sie können das Projekt herunterladen und die Dateien vergleichen.</span><span class="sxs-lookup"><span data-stu-id="7161f-127">You can download the project and compare the files.</span></span>

<span data-ttu-id="7161f-128">Führen Sie die Anwendung aus, und klicken Sie auf den Link "admin", der oben auf der Startseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="7161f-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="7161f-129">Die Seite admin sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="7161f-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="7161f-130">Momentan führt die Seite nichts aus.</span><span class="sxs-lookup"><span data-stu-id="7161f-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="7161f-131">Im nächsten Abschnitt verwenden wir Knockout. js, um eine dynamische Benutzeroberfläche zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7161f-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="7161f-132">Autorisierung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="7161f-132">Add Authorization</span></span>

<span data-ttu-id="7161f-133">Alle Benutzer, die die Website besuchen, können auf die Seite "admin" zugreifen.</span><span class="sxs-lookup"><span data-stu-id="7161f-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="7161f-134">Ändern wir dies, um die Berechtigung für Administratoren einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="7161f-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="7161f-135">Beginnen Sie, indem Sie eine Administrator Rolle und einen Administrator Benutzer hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7161f-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="7161f-136">Erweitern Sie in Projektmappen-Explorer den Ordner Filter, und öffnen Sie die Datei mit dem Namen InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="7161f-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="7161f-137">Suchen Sie den `SimpleMembershipInitializer`-Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="7161f-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="7161f-138">Fügen Sie nach dem Aufrufen von **WebSecurity. initializedatabaseconnetction**den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7161f-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="7161f-139">Dies ist eine schnelle und geänderte Möglichkeit, die Rolle "Administrator" hinzuzufügen und einen Benutzer für die Rolle zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7161f-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="7161f-140">Erweitern Sie in Projektmappen-Explorer den Ordner Controllers, und öffnen Sie die Datei HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="7161f-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="7161f-141">Fügen Sie das Attribut **autorisieren** der `Admin`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="7161f-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="7161f-142">Öffnen Sie die Datei AdminController.cs, und fügen Sie das Attribut **autorisieren** der gesamten `AdminController` Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="7161f-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="7161f-143">MVC und die Web-API definieren beide **Autorisierungs** Attribute in verschiedenen Namespaces.</span><span class="sxs-lookup"><span data-stu-id="7161f-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="7161f-144">MVC verwendet **System. Web. MVC. autorizeattribute**, während die Web-API **System. Web. http. autorizeattribute**verwendet.</span><span class="sxs-lookup"><span data-stu-id="7161f-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>

<span data-ttu-id="7161f-145">Jetzt können nur Administratoren die Administrator Seite anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7161f-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="7161f-146">Außerdem muss die Anforderung ein Authentifizierungs Cookie enthalten, wenn Sie eine HTTP-Anforderung an den Administrator Controller senden.</span><span class="sxs-lookup"><span data-stu-id="7161f-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="7161f-147">Wenn dies nicht der Fall ist, sendet der Server eine HTTP 401-Antwort (nicht autorisiert).</span><span class="sxs-lookup"><span data-stu-id="7161f-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="7161f-148">Sie können dies in "fddler" sehen, indem Sie eine GET-Anforderung an `http://localhost:*port*/api/admin`senden.</span><span class="sxs-lookup"><span data-stu-id="7161f-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7161f-149">[Zurück](using-web-api-with-entity-framework-part-3.md)
> [Weiter](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="7161f-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
