---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-Host ASP.net-Web-API 1 (C#)-ASP.NET 4. x
author: MikeWasson
description: Tutorial mit Code zeigt, wie eine Web-API in einer Konsolenanwendung gehostet wird.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421371"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="bf072-103">Self-Host-ASP.net-Web-API 1C#()</span><span class="sxs-lookup"><span data-stu-id="bf072-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="bf072-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf072-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bf072-105">In diesem Tutorial wird gezeigt, wie Sie eine Web-API in einer Konsolenanwendung hosten.</span><span class="sxs-lookup"><span data-stu-id="bf072-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="bf072-106">ASP.net-Web-API ist IIS nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bf072-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="bf072-107">Sie können eine Web-API in Ihrem eigenen Host Prozess selbst hosten.</span><span class="sxs-lookup"><span data-stu-id="bf072-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="bf072-108">**Neue Anwendungen sollten owin für die Self-Host-Web-API verwenden.**</span><span class="sxs-lookup"><span data-stu-id="bf072-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="bf072-109">Weitere Informationen finden [Sie unter Verwenden von owin für Self-Host ASP.net-Web-API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bf072-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bf072-110">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="bf072-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bf072-111">Web-API 1</span><span class="sxs-lookup"><span data-stu-id="bf072-111">Web API 1</span></span>
> - <span data-ttu-id="bf072-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bf072-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="bf072-113">Erstellen des Konsolen Anwendungs Projekts</span><span class="sxs-lookup"><span data-stu-id="bf072-113">Create the Console Application Project</span></span>

<span data-ttu-id="bf072-114">Starten Sie Visual Studio, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="bf072-115">Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="bf072-116">Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten.</span><span class="sxs-lookup"><span data-stu-id="bf072-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="bf072-117">Wählen Sie unter **Visualisierung C#** die Option **Windows**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="bf072-118">Wählen Sie in der Liste der Projektvorlagen die Option **Konsolenanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="bf072-119">Nennen Sie das Projekt &quot;SelfHost-&quot;, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf072-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="bf072-120">Festlegen des Ziel Frameworks (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="bf072-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="bf072-121">Wenn Sie Visual Studio 2010 verwenden, ändern Sie das Ziel Framework in .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="bf072-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="bf072-122">(Standardmäßig ist die Projektvorlage auf das [.NET Framework-Client Profil](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)ausgerichtet.)</span><span class="sxs-lookup"><span data-stu-id="bf072-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="bf072-123">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="bf072-124">Ändern Sie in der Dropdown Liste **Ziel Framework** das Ziel Framework in .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="bf072-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="bf072-125">Wenn Sie aufgefordert werden, die Änderung anzuwenden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="bf072-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="bf072-126">Installieren des nuget-Paket-Managers</span><span class="sxs-lookup"><span data-stu-id="bf072-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="bf072-127">Der nuget-Paket-Manager ist die einfachste Möglichkeit, die Web-API-Assemblys zu einem Non-ASP.net-Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="bf072-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="bf072-128">Um zu prüfen, **ob der** nuget-Paket-Manager installiert ist, klicken Sie in Visual Studio auf das Menü Extras.</span><span class="sxs-lookup"><span data-stu-id="bf072-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="bf072-129">Wenn Sie ein Menü Element namens **nuget-Paket-Manager**sehen, haben Sie den nuget-Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="bf072-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="bf072-130">So installieren Sie den nuget-Paket-Manager:</span><span class="sxs-lookup"><span data-stu-id="bf072-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="bf072-131">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf072-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="bf072-132">Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="bf072-133">Wählen Sie im Dialogfeld **Erweiterungen und Updates** die Option **Online**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="bf072-134">Wenn "nuget-Paket-Manager" nicht angezeigt wird, geben Sie "nuget-Paket-Manager" in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="bf072-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="bf072-135">Wählen Sie den nuget-Paket-Manager aus, **und klicken Sie**auf</span><span class="sxs-lookup"><span data-stu-id="bf072-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="bf072-136">Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, zu installieren.</span><span class="sxs-lookup"><span data-stu-id="bf072-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="bf072-137">Nachdem die Installation abgeschlossen ist, werden Sie möglicherweise aufgefordert, Visual Studio neu zu starten.</span><span class="sxs-lookup"><span data-stu-id="bf072-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="bf072-138">Hinzufügen des Web-API-nuget-Pakets</span><span class="sxs-lookup"><span data-stu-id="bf072-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="bf072-139">Fügen Sie nach der Installation des nuget-Paket-Managers das Web-API-Self-Host-Paket zu Ihrem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="bf072-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="bf072-140">Wählen Sie **im Menü** Extras den Befehl **nuget-Paket-Manager**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="bf072-141">*Hinweis*: Wenn dieses Menü Element nicht angezeigt wird, stellen Sie sicher, dass der nuget-Paket-Manager ordnungsgemäß installiert ist.</span><span class="sxs-lookup"><span data-stu-id="bf072-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="bf072-142">Wählen Sie **nuget-Pakete für Projekt Mappe verwalten aus** .</span><span class="sxs-lookup"><span data-stu-id="bf072-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="bf072-143">Klicken Sie im Dialogfeld " **nuget-Pakete verwalten** " auf **Online**.</span><span class="sxs-lookup"><span data-stu-id="bf072-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="bf072-144">Geben Sie im Suchfeld &quot;Microsoft. Aspnet. WebAPI. SelfHost&quot;ein.</span><span class="sxs-lookup"><span data-stu-id="bf072-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="bf072-145">Wählen Sie das ASP.net-Web-API selbsthostpaket aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="bf072-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="bf072-146">Nachdem das Paket installiert wurde, klicken Sie auf **Schließen** , um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="bf072-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="bf072-147">Stellen Sie sicher, dass Sie das Paket mit dem Namen "Microsoft. Aspnet. WebAPI. SelfHost" installieren, nicht "aspnetwebapi. SelfHost".</span><span class="sxs-lookup"><span data-stu-id="bf072-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="bf072-148">Erstellen des Modells und des Controllers</span><span class="sxs-lookup"><span data-stu-id="bf072-148">Create the Model and Controller</span></span>

<span data-ttu-id="bf072-149">In diesem Tutorial werden die gleichen Modell-und Controller Klassen wie im Tutorial " [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) " verwendet.</span><span class="sxs-lookup"><span data-stu-id="bf072-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="bf072-150">Fügen Sie eine öffentliche Klasse mit dem Namen `Product`hinzu.</span><span class="sxs-lookup"><span data-stu-id="bf072-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="bf072-151">Fügen Sie eine öffentliche Klasse mit dem Namen `ProductsController`hinzu.</span><span class="sxs-lookup"><span data-stu-id="bf072-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="bf072-152">Leiten Sie diese Klasse von **System. Web. http. apicontroller**ab.</span><span class="sxs-lookup"><span data-stu-id="bf072-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="bf072-153">Weitere Informationen zum Code in diesem Controller finden Sie im Tutorial [zu](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) den ersten Schritten.</span><span class="sxs-lookup"><span data-stu-id="bf072-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="bf072-154">Dieser Controller definiert drei Get-Aktionen:</span><span class="sxs-lookup"><span data-stu-id="bf072-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="bf072-155">URI</span><span class="sxs-lookup"><span data-stu-id="bf072-155">URI</span></span> | <span data-ttu-id="bf072-156">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="bf072-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bf072-157">/api/products</span><span class="sxs-lookup"><span data-stu-id="bf072-157">/api/products</span></span> | <span data-ttu-id="bf072-158">Eine Liste aller Produkte erhalten.</span><span class="sxs-lookup"><span data-stu-id="bf072-158">Get a list of all products.</span></span> |
| <span data-ttu-id="bf072-159">/API/Products/-*ID*</span><span class="sxs-lookup"><span data-stu-id="bf072-159">/api/products/*id*</span></span> | <span data-ttu-id="bf072-160">Erhalten Sie ein Produkt nach ID.</span><span class="sxs-lookup"><span data-stu-id="bf072-160">Get a product by ID.</span></span> |
| <span data-ttu-id="bf072-161">/API/Products/? Category =*Kategorie*</span><span class="sxs-lookup"><span data-stu-id="bf072-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="bf072-162">Eine Liste der Produkte nach Kategorie erhalten.</span><span class="sxs-lookup"><span data-stu-id="bf072-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="bf072-163">Hosten der Web-API</span><span class="sxs-lookup"><span data-stu-id="bf072-163">Host the Web API</span></span>

<span data-ttu-id="bf072-164">Öffnen Sie die Datei Program.cs, und fügen Sie die folgenden using-Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="bf072-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="bf072-165">Fügen Sie der **Program** -Klasse den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="bf072-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="bf072-166">Optionale Hinzufügen einer HTTP-URL-Namespace Reservierung</span><span class="sxs-lookup"><span data-stu-id="bf072-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="bf072-167">Diese Anwendung lauscht auf `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="bf072-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="bf072-168">Standardmäßig sind für das lauschen an einer bestimmten http-Adresse Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bf072-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="bf072-169">Wenn Sie das Tutorial ausführen, erhalten Sie möglicherweise den folgenden Fehler: "http konnte die URL http://+:8080/nicht registrieren". es gibt zwei Möglichkeiten, diesen Fehler zu vermeiden:</span><span class="sxs-lookup"><span data-stu-id="bf072-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="bf072-170">Führen Sie Visual Studio mit erweiterten Administrator Berechtigungen aus, oder</span><span class="sxs-lookup"><span data-stu-id="bf072-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="bf072-171">Verwenden Sie netsh. exe, um Ihrem Konto Berechtigungen zum Reservieren der URL zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="bf072-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="bf072-172">Um "Netsh. exe" zu verwenden, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und geben Sie den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="bf072-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="bf072-173">Dabei ist " *" Computer\Benutzername "* " Ihr Benutzerkonto.</span><span class="sxs-lookup"><span data-stu-id="bf072-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="bf072-174">Wenn Sie das selbst Hosting abgeschlossen haben, stellen Sie sicher, dass Sie die Reservierung löschen:</span><span class="sxs-lookup"><span data-stu-id="bf072-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="bf072-175">Abrufen der Web-API aus einer Client AnwendungC#()</span><span class="sxs-lookup"><span data-stu-id="bf072-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="bf072-176">Wir schreiben eine einfache Konsolenanwendung, die die Web-API aufruft.</span><span class="sxs-lookup"><span data-stu-id="bf072-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="bf072-177">Fügen Sie der Projekt Mappe ein neues Konsolen Anwendungsprojekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="bf072-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="bf072-178">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Neues Projekt hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="bf072-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="bf072-179">Erstellen Sie eine neue Konsolenanwendung mit dem Namen &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="bf072-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="bf072-180">Fügen Sie mit dem nuget-Paket-Manager das ASP.net-Web-API Core-Bibliothekspaket hinzu:</span><span class="sxs-lookup"><span data-stu-id="bf072-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="bf072-181">Wählen Sie im Menü Extras den Befehl **nuget-Paket-Manager**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="bf072-182">Wählen Sie **nuget-Pakete für Projekt Mappe verwalten aus** .</span><span class="sxs-lookup"><span data-stu-id="bf072-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="bf072-183">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Online**.</span><span class="sxs-lookup"><span data-stu-id="bf072-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="bf072-184">Geben Sie im Suchfeld &quot;Microsoft. Aspnet. WebAPI. Client&quot;ein.</span><span class="sxs-lookup"><span data-stu-id="bf072-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="bf072-185">Wählen Sie das Paket Microsoft ASP.net Web-API Client Libraries aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="bf072-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="bf072-186">Fügen Sie dem Projekt "SelfHost" in "ClientApp" einen Verweis hinzu:</span><span class="sxs-lookup"><span data-stu-id="bf072-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="bf072-187">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das ClientApp-Projekt.</span><span class="sxs-lookup"><span data-stu-id="bf072-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="bf072-188">Klicken Sie auf **Verweis hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="bf072-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="bf072-189">Wählen **Sie im Dialog**Feld **Verweis-Manager** Unterprojekt Mappe die Option **Projekte**aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="bf072-190">Wählen Sie das Projekt SelfHost aus.</span><span class="sxs-lookup"><span data-stu-id="bf072-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="bf072-191">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf072-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="bf072-192">Öffnen Sie die Datei "Client/Program. cs".</span><span class="sxs-lookup"><span data-stu-id="bf072-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="bf072-193">Fügen Sie die folgende **using** -Anweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="bf072-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="bf072-194">Fügen Sie eine statische **HttpClient** -Instanz hinzu:</span><span class="sxs-lookup"><span data-stu-id="bf072-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="bf072-195">Fügen Sie die folgenden Methoden hinzu, um alle Produkte aufzulisten, ein Produkt nach ID aufzulisten und Produkte nach Kategorie aufzulisten.</span><span class="sxs-lookup"><span data-stu-id="bf072-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="bf072-196">Jede dieser Methoden folgt dem gleichen Muster:</span><span class="sxs-lookup"><span data-stu-id="bf072-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="bf072-197">Aufrufen von **HttpClient. getasync** , um eine GET-Anforderung an den entsprechenden URI zu senden.</span><span class="sxs-lookup"><span data-stu-id="bf072-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="bf072-198">Rufen Sie **HttpResponseMessage. ensuresuccess Statuscode**auf.</span><span class="sxs-lookup"><span data-stu-id="bf072-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="bf072-199">Diese Methode löst eine Ausnahme aus, wenn der HTTP-Antwortstatus ein Fehlercode ist.</span><span class="sxs-lookup"><span data-stu-id="bf072-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="bf072-200">Aufrufen von "read **asasync&lt;t&gt;** ", um einen CLR-Typ aus der HTTP-Antwort zu deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="bf072-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="bf072-201">Diese Methode ist eine Erweiterungsmethode, die in **System .net. http. httpcontentextensions**definiert ist.</span><span class="sxs-lookup"><span data-stu-id="bf072-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="bf072-202">Die Methoden **getasync** und Read **asasync** sind beide asynchron.</span><span class="sxs-lookup"><span data-stu-id="bf072-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="bf072-203">Sie geben **Aufgaben** Objekte zurück, die den asynchronen Vorgang darstellen.</span><span class="sxs-lookup"><span data-stu-id="bf072-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="bf072-204">Durch Abrufen der **Ergebnis** Eigenschaft wird der Thread blockiert, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="bf072-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="bf072-205">Weitere Informationen zur Verwendung von HttpClient, einschließlich der Vorgehensweise zum Erstellen von nicht blockierenden aufrufen, finden Sie unter [Aufrufen einer Web-API von einem .NET-Client](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="bf072-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="bf072-206">Bevor Sie diese Methoden aufrufen, legen Sie die BaseAddress-Eigenschaft der httpclient-Instanz auf "`http://localhost:8080`" fest.</span><span class="sxs-lookup"><span data-stu-id="bf072-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="bf072-207">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bf072-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="bf072-208">Dadurch sollte Folgendes ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="bf072-208">This should output the following.</span></span> <span data-ttu-id="bf072-209">(Denken Sie daran, die SelfHost-Anwendung zuerst auszuführen.)</span><span class="sxs-lookup"><span data-stu-id="bf072-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
