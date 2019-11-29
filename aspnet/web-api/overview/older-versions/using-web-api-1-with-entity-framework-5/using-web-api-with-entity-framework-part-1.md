---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Teil 1: Übersicht und Erstellen des Projekts | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600327"
---
# <a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="4c02e-102">Teil 1: Übersicht und Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="4c02e-102">Part 1: Overview and Creating the Project</span></span>

<span data-ttu-id="4c02e-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4c02e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4c02e-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="4c02e-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="4c02e-105">Entity Framework ist ein Objekt-/Relationales Mapping-Framework.</span><span class="sxs-lookup"><span data-stu-id="4c02e-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="4c02e-106">Die Domänen Objekte in Ihrem Code werden Entitäten in einer relationalen Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="4c02e-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="4c02e-107">Zum größten Teil müssen Sie sich keine Gedanken über die Datenbankschicht machen, da Sie Entity Framework für Sie kümmert.</span><span class="sxs-lookup"><span data-stu-id="4c02e-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="4c02e-108">Der Code bearbeitet die Objekte, und die Änderungen werden in einer Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4c02e-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="4c02e-109">Informationen zum Tutorial</span><span class="sxs-lookup"><span data-stu-id="4c02e-109">About the Tutorial</span></span>

<span data-ttu-id="4c02e-110">In diesem Tutorial erstellen Sie eine einfache Store-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4c02e-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="4c02e-111">Die Anwendung besteht aus zwei Hauptbereichen.</span><span class="sxs-lookup"><span data-stu-id="4c02e-111">There are two main parts to the application.</span></span> <span data-ttu-id="4c02e-112">Normale Benutzer können Produkte anzeigen und Aufträge erstellen:</span><span class="sxs-lookup"><span data-stu-id="4c02e-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="4c02e-113">Administratoren können Produkte erstellen, löschen oder bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="4c02e-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="4c02e-114">Fähigkeiten, die Sie erlernen</span><span class="sxs-lookup"><span data-stu-id="4c02e-114">Skills You'll Learn</span></span>

<span data-ttu-id="4c02e-115">Hier lernen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="4c02e-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="4c02e-116">Verwendung von Entity Framework mit ASP.net-Web-API.</span><span class="sxs-lookup"><span data-stu-id="4c02e-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="4c02e-117">Verwenden von Knockout. js zum Erstellen einer dynamischen Client Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="4c02e-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="4c02e-118">Verwenden der Formular Authentifizierung mit der Web-API zum Authentifizieren von Benutzern</span><span class="sxs-lookup"><span data-stu-id="4c02e-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="4c02e-119">Obwohl dieses Tutorial eigenständig ist, sollten Sie zunächst die folgenden Tutorials lesen:</span><span class="sxs-lookup"><span data-stu-id="4c02e-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="4c02e-120">Ihre erste ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="4c02e-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="4c02e-121">Erstellen einer Web-API, die CRUD-Vorgänge unterstützt</span><span class="sxs-lookup"><span data-stu-id="4c02e-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="4c02e-122">Einige Kenntnisse von [ASP.NET MVC](../../../../mvc/index.md) sind ebenfalls hilfreich.</span><span class="sxs-lookup"><span data-stu-id="4c02e-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="4c02e-123">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="4c02e-123">Overview</span></span>

<span data-ttu-id="4c02e-124">Im folgenden finden Sie die Architektur der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="4c02e-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="4c02e-125">ASP.NET MVC generiert die HTML-Seiten für den Client.</span><span class="sxs-lookup"><span data-stu-id="4c02e-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="4c02e-126">ASP.net-Web-API macht CRUD-Vorgänge für die Daten (Produkte und Aufträge) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4c02e-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="4c02e-127">Entity Framework übersetzt die C# von der Web-API verwendeten Modelle in Daten Bank Entitäten.</span><span class="sxs-lookup"><span data-stu-id="4c02e-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="4c02e-128">Das folgende Diagramm zeigt, wie die Domänen Objekte auf verschiedenen Ebenen der Anwendung dargestellt werden: die Datenbankebene, das Objektmodell und schließlich das Wire-Format, das zum Übertragen von Daten an den Client über HTTP verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4c02e-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="4c02e-129">Erstellen des Visual Studio-Projekts</span><span class="sxs-lookup"><span data-stu-id="4c02e-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="4c02e-130">Sie können das tutorialprojekt entweder mithilfe von Visual Web Developer Express oder der Vollversion von Visual Studio erstellen.</span><span class="sxs-lookup"><span data-stu-id="4c02e-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="4c02e-131">Klicken Sie auf der **Start** Seite auf **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4c02e-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="4c02e-132">Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten.</span><span class="sxs-lookup"><span data-stu-id="4c02e-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="4c02e-133">Wählen Sie unter **Visualisierung C#** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="4c02e-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="4c02e-134">Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="4c02e-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="4c02e-135">Nennen Sie das Projekt productstore, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c02e-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="4c02e-136">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Internet Anwendung** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c02e-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="4c02e-137">Die Vorlage "Internet Anwendung" erstellt eine ASP.NET MVC-Anwendung, die die Formular Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="4c02e-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="4c02e-138">Wenn Sie die Anwendung jetzt ausführen, haben Sie bereits einige Features:</span><span class="sxs-lookup"><span data-stu-id="4c02e-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="4c02e-139">Neue Benutzer können sich registrieren, indem Sie auf den Link "Register" in der oberen rechten Ecke klicken.</span><span class="sxs-lookup"><span data-stu-id="4c02e-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="4c02e-140">Registrierte Benutzer können sich anmelden, indem Sie auf den Link "Anmelden" klicken.</span><span class="sxs-lookup"><span data-stu-id="4c02e-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="4c02e-141">Mitgliedschafts Informationen werden in einer Datenbank gespeichert, die automatisch erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="4c02e-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="4c02e-142">Weitere Informationen zur Formular Authentifizierung in ASP.NET MVC finden Sie unter Exemplarische Vorgehensweise [: Verwenden der Formular Authentifizierung in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="4c02e-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="4c02e-143">Aktualisieren der CSS-Datei</span><span class="sxs-lookup"><span data-stu-id="4c02e-143">Update the CSS File</span></span>

<span data-ttu-id="4c02e-144">Dieser Schritt ist kosmetisch, aber die Seiten werden wie die vorherigen Screenshots dargestellt.</span><span class="sxs-lookup"><span data-stu-id="4c02e-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="4c02e-145">Erweitern Sie in Projektmappen-Explorer den Ordner Inhalt, und öffnen Sie die Datei mit dem Namen Site. CSS.</span><span class="sxs-lookup"><span data-stu-id="4c02e-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="4c02e-146">Fügen Sie die folgenden CSS-Stile hinzu:</span><span class="sxs-lookup"><span data-stu-id="4c02e-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="4c02e-147">Nächste</span><span class="sxs-lookup"><span data-stu-id="4c02e-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
