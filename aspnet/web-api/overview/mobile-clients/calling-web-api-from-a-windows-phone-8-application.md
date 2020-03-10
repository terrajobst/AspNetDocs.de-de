---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Aufrufen der Web-API aus einer Windows Phone 8C#-Anwendung ()-ASP.NET 4. x
author: rmcmurray
description: 'Tutorial mit Code: Erstellen einer ASP.net-Web-API-Anwendung in ASP.NET 4. x, die einen Katalog von Büchern für eine Windows Phone 8-Anwendung bereitstellt.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498213"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="79112-103">Aufrufen des Web-API über eine Windows Phone 8-Anwendung (C#)</span><span class="sxs-lookup"><span data-stu-id="79112-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>

<span data-ttu-id="79112-104">von [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="79112-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="79112-105">In diesem Tutorial erfahren Sie, wie Sie ein umfassendes End-to-End-Szenario erstellen, das aus einer ASP.net-Web-API Anwendung besteht, die einen Katalog von Büchern für eine Windows Phone 8-Anwendung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="79112-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="79112-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="79112-106">Overview</span></span>

<span data-ttu-id="79112-107">Rest-Dienste wie ASP.net-Web-API vereinfachen die Erstellung von http-basierten Anwendungen für Entwickler durch die Abstraktion der Architektur für serverseitige und Client seitige Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="79112-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="79112-108">Anstatt ein proprietäres, socketbasiertes Protokoll für die Kommunikation zu erstellen, müssen Web-API-Entwickler einfach die erforderlichen HTTP-Methoden für Ihre Anwendung veröffentlichen (z. b. "Get", "Post", "Put", "Delete"), und Entwickler von Client Anwendungen müssen nur die HTTP-Methoden, die für Ihre Anwendung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="79112-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="79112-109">In diesem End-to-End-Tutorial erfahren Sie, wie Sie die Web-API verwenden, um die folgenden Projekte zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="79112-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="79112-110">Im [ersten Teil dieses Tutorials](#STEP1)erstellen Sie eine ASP.net-Web-API Anwendung, die alle Vorgänge zum Erstellen, lesen, aktualisieren und löschen (CRUD) zum Verwalten eines Buch Katalogs unterstützt.</span><span class="sxs-lookup"><span data-stu-id="79112-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="79112-111">Diese Anwendung verwendet die [XML-Beispieldatei (Books. Xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) von MSDN.</span><span class="sxs-lookup"><span data-stu-id="79112-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="79112-112">Im [zweiten Teil dieses Tutorials](#STEP2)erstellen Sie eine interaktive Windows Phone 8-Anwendung, mit der die Daten aus Ihrer Web-API-Anwendung abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="79112-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="79112-113">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="79112-113">Prerequisites</span></span>

- <span data-ttu-id="79112-114">Visual Studio 2013 mit installiertem Windows Phone 8 SDK</span><span class="sxs-lookup"><span data-stu-id="79112-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="79112-115">Windows 8 oder höher auf einem 64-Bit-System mit installiertem Hyper-V</span><span class="sxs-lookup"><span data-stu-id="79112-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="79112-116">Eine Liste der zusätzlichen Anforderungen finden Sie im Abschnitt *System Anforderungen* auf der [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) -Downloadseite.</span><span class="sxs-lookup"><span data-stu-id="79112-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="79112-117">Wenn Sie die Konnektivität zwischen Web-API und Windows Phone 8-Projekten auf Ihrem lokalen System testen möchten, müssen Sie die Anweisungen im Artikel *[Verbinden des Windows Phone 8-Emulators mit Web-API-Anwendungen auf einem lokalen Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* befolgen, um Ihre Testumgebung einzurichten.</span><span class="sxs-lookup"><span data-stu-id="79112-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="79112-118">Schritt 1: Erstellen des Web-API-buchbuch Projekts</span><span class="sxs-lookup"><span data-stu-id="79112-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="79112-119">Der erste Schritt in diesem End-to-End-Tutorial besteht darin, ein Web-API-Projekt zu erstellen, das alle CRUD-Vorgänge unterstützt. Beachten Sie, dass Sie der Projekt Mappe in [Schritt 2](#STEP2) dieses Tutorials das Windows Phone-Anwendungsprojekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="79112-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="79112-120">Öffnen Sie **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="79112-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="79112-121">Klicken Sie auf **Datei**, dann auf **neu**und dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="79112-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="79112-122">Wenn das Dialogfeld **Neues Projekt** angezeigt wird, erweitern Sie **installiert**, dann **Vorlagen**, dann **Visual C#** und dann **Web**.</span><span class="sxs-lookup"><span data-stu-id="79112-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="79112-123">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-123">Click image to expand</span></span>                                                                |

4. <span data-ttu-id="79112-124">Markieren Sie die **ASP.NET-Webanwendung**, geben Sie **Bookstore** als Projektnamen ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="79112-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="79112-125">Wenn das Dialogfeld **Neues ASP.net-Projekt** angezeigt wird, wählen Sie die Vorlage **Web-API** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="79112-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="79112-126">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-126">Click image to expand</span></span>                                                                |

6. <span data-ttu-id="79112-127">Wenn das Web-API-Projekt geöffnet wird, entfernen Sie den Beispiel Controller aus dem Projekt:</span><span class="sxs-lookup"><span data-stu-id="79112-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="79112-128">Erweitern Sie im Projektmappen-Explorer den Ordner **Controller** .</span><span class="sxs-lookup"><span data-stu-id="79112-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="79112-129">Klicken Sie mit der rechten Maustaste auf die Datei **ValuesController.cs** , und klicken Sie dann auf **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="79112-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="79112-130">Klicken Sie bei Aufforderung auf **OK** , um den Löschvorgang zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="79112-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="79112-131">Fügen Sie dem Web-API-Projekt eine XML-Datendatei hinzu. Diese Datei enthält den Inhalt des bookstore-Katalogs:</span><span class="sxs-lookup"><span data-stu-id="79112-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="79112-132">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **App\_Data** , und klicken Sie dann auf **Hinzufügen**und dann auf **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="79112-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="79112-133">Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, markieren Sie die Vorlage **XML-Datei** .</span><span class="sxs-lookup"><span data-stu-id="79112-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="79112-134">Benennen Sie die Datei **Books. XML**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="79112-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="79112-135">Wenn die Datei **Books. XML** geöffnet ist, ersetzen Sie den Code in der Datei durch den XML-Code aus der Datei "Sample **Books. XML** " auf MSDN:</span><span class="sxs-lookup"><span data-stu-id="79112-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="79112-136">Speichern und schließen Sie die XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="79112-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="79112-137">Fügen Sie dem Web-API-Projekt das bookstore-Modell hinzu. Dieses Modell enthält die CREATE-, Read-, Update-und DELETE-Logik (CRUD) für die Bookstore-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="79112-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="79112-138">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Modelle** , klicken Sie auf **Hinzufügen**und dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="79112-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="79112-139">Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="79112-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="79112-140">Wenn die Datei **BookDetails.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="79112-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="79112-141">Speichern und schließen Sie die Datei **BookDetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="79112-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="79112-142">Fügen Sie den Bookstore-Controller dem Web-API-Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="79112-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="79112-143">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Controller** , klicken Sie auf **Hinzufügen**und dann auf **Controller**.</span><span class="sxs-lookup"><span data-stu-id="79112-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="79112-144">Wenn das Dialogfeld **Gerüst hinzufügen** angezeigt wird, markieren Sie **Web API 2 Controller-Empty**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="79112-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="79112-145">Wenn das Dialogfeld **Controller hinzufügen** angezeigt wird, benennen Sie den Controller **bookscontroller**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="79112-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="79112-146">Wenn die Datei **BooksController.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="79112-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="79112-147">Speichern und schließen Sie die Datei **BooksController.cs** .</span><span class="sxs-lookup"><span data-stu-id="79112-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="79112-148">Erstellen Sie die Web-API-Anwendung, um nach Fehlern zu suchen.</span><span class="sxs-lookup"><span data-stu-id="79112-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="79112-149">Schritt 2: Hinzufügen des Windows Phone 8 Bookstore Catalog-Projekts</span><span class="sxs-lookup"><span data-stu-id="79112-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="79112-150">Der nächste Schritt dieses End-to-End-Szenarios besteht darin, die Katalog Anwendung für Windows Phone 8 zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="79112-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="79112-151">Diese Anwendung verwendet die Windows Phone Daten gebundene *App* -Vorlage für die Standardbenutzer Oberfläche und verwendet die Web-API-Anwendung, die Sie in [Schritt 1](#STEP1) dieses Tutorials als Datenquelle erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="79112-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="79112-152">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die **Bookstore** -Projekt Mappe, und klicken Sie dann auf **Hinzufügen**und **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="79112-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="79112-153">Wenn das Dialogfeld **Neues Projekt** angezeigt wird, erweitern Sie die Option **installiert**und dann **Visualisierung C#** , und klicken Sie dann auf **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="79112-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="79112-154">Markieren Sie **Windows Phone Daten gebundene App**, geben Sie **bookcatalog** als Name ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="79112-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="79112-155">Fügen Sie das nuget-Paket JSON.net dem **bookcatalog** -Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="79112-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="79112-156">Klicken Sie **im Projekt** Mappen-Explorer mit der rechten Maustaste auf **Verweise** , und klicken Sie dann auf **nuget-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="79112-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="79112-157">Wenn das Dialogfeld **nuget-Pakete verwalten** angezeigt wird, erweitern Sie den Abschnitt **Online** , und markieren Sie **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="79112-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="79112-158">Geben Sie **JSON.net** in das Suchfeld ein, und klicken Sie auf das Suchsymbol.</span><span class="sxs-lookup"><span data-stu-id="79112-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="79112-159">Markieren Sie **JSON.net** in den Suchergebnissen, und klicken Sie dann auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="79112-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="79112-160">Klicken Sie nach Abschluss der Installation auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="79112-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="79112-161">Fügen Sie dem **bookcatalog** -Projekt das **BookDetails** -Modell hinzu. Diese enthält ein generisches Modell der Bookstore-Klasse:</span><span class="sxs-lookup"><span data-stu-id="79112-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="79112-162">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **bookcatalog** , klicken Sie dann auf **Hinzufügen**und dann auf **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="79112-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="79112-163">Benennen Sie die neuen Ordner **Modelle**.</span><span class="sxs-lookup"><span data-stu-id="79112-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="79112-164">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Modelle** , klicken Sie auf **Hinzufügen**und dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="79112-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="79112-165">Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="79112-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="79112-166">Wenn die Datei **BookDetails.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="79112-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="79112-167">Speichern und schließen Sie die Datei **BookDetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="79112-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="79112-168">Aktualisieren Sie die **MainViewModel.cs** -Klasse, um die Funktionen für die Kommunikation mit der Web-API-Anwendung der Buchhandlung einzubeziehen</span><span class="sxs-lookup"><span data-stu-id="79112-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="79112-169">Erweitern Sie im Projektmappen-Explorer den Ordner **ViewModels** , und doppelklicken Sie dann auf die Datei **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="79112-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="79112-170">Wenn die Datei **MainViewModel.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes: Beachten Sie, dass Sie den Wert der `apiUrl`-Konstante mit der tatsächlichen URL Ihrer Web-API aktualisieren müssen:</span><span class="sxs-lookup"><span data-stu-id="79112-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="79112-171">Speichern und schließen Sie die Datei **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="79112-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="79112-172">Aktualisieren Sie die Datei " **MainPage. XAML** ", um den Anwendungsnamen anzupassen:</span><span class="sxs-lookup"><span data-stu-id="79112-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="79112-173">Doppelklicken Sie im Projektmappen-Explorer auf die Datei " **MainPage. XAML** ".</span><span class="sxs-lookup"><span data-stu-id="79112-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="79112-174">Wenn die Datei " **MainPage. XAML** " geöffnet ist, suchen Sie nach den folgenden Codezeilen:</span><span class="sxs-lookup"><span data-stu-id="79112-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="79112-175">Ersetzen Sie diese Zeilen durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="79112-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="79112-176">Speichern und schließen Sie die Datei " **MainPage. XAML** ".</span><span class="sxs-lookup"><span data-stu-id="79112-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="79112-177">Aktualisieren Sie die Datei " **detailspage. XAML** ", um die angezeigten Elemente anzupassen:</span><span class="sxs-lookup"><span data-stu-id="79112-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="79112-178">Doppelklicken Sie im Projektmappen-Explorer auf die Datei **detailspage. XAML** .</span><span class="sxs-lookup"><span data-stu-id="79112-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="79112-179">Wenn die Datei " **detailspage. XAML** " geöffnet ist, suchen Sie nach den folgenden Codezeilen:</span><span class="sxs-lookup"><span data-stu-id="79112-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="79112-180">Ersetzen Sie diese Zeilen durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="79112-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="79112-181">Speichern und schließen Sie die Datei **detailspage. XAML** .</span><span class="sxs-lookup"><span data-stu-id="79112-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="79112-182">Erstellen Sie die Windows Phone Anwendung, um nach Fehlern zu suchen.</span><span class="sxs-lookup"><span data-stu-id="79112-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="79112-183">Schritt 3: Testen der End-to-End-Lösung</span><span class="sxs-lookup"><span data-stu-id="79112-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="79112-184">Wie im Abschnitt " *Voraussetzungen* " dieses Tutorials erwähnt, müssen Sie beim Testen der Konnektivität zwischen Web-API und Windows Phone 8-Projekten auf Ihrem lokalen System die Anweisungen im Artikel *[Herstellen einer Verbindung zwischen dem Windows Phone 8-Emulator und Web-API-Anwendungen auf einem lokalen Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* befolgen, um Ihre Testumgebung einzurichten.</span><span class="sxs-lookup"><span data-stu-id="79112-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="79112-185">Nachdem Sie die Testumgebung konfiguriert haben, müssen Sie die Windows Phone Anwendung als Startprojekt festlegen.</span><span class="sxs-lookup"><span data-stu-id="79112-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="79112-186">Markieren Sie hierzu die **bookcatalog** -Anwendung im Projektmappen-Explorer, und klicken Sie dann auf **als Startprojekt festlegen**:</span><span class="sxs-lookup"><span data-stu-id="79112-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="79112-187">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-187">Click image to expand</span></span> |

<span data-ttu-id="79112-188">Wenn Sie F5 drücken, startet Visual Studio sowohl den Windows Phone Emulator, der eine &quot;die Sie&quot; Meldung anzeigen, während die Anwendungsdaten von Ihrer Web-API abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="79112-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="79112-189">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-189">Click image to expand</span></span> |

<span data-ttu-id="79112-190">Wenn alles erfolgreich ist, sollte der Katalog angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="79112-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="79112-191">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-191">Click image to expand</span></span> |

<span data-ttu-id="79112-192">Wenn Sie auf einen Buchtitel tippen, wird die Buchbeschreibung in der Anwendung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="79112-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="79112-193">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-193">Click image to expand</span></span> |

<span data-ttu-id="79112-194">Wenn die Anwendung nicht mit Ihrer Web-API kommunizieren kann, wird eine Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="79112-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="79112-195">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-195">Click image to expand</span></span> |

<span data-ttu-id="79112-196">Wenn Sie auf die Fehlermeldung tippen, werden weitere Details zum Fehler angezeigt:</span><span class="sxs-lookup"><span data-stu-id="79112-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="79112-197">Zum Erweitern auf das Bild klicken</span><span class="sxs-lookup"><span data-stu-id="79112-197">Click image to expand</span></span>                                                                 |
