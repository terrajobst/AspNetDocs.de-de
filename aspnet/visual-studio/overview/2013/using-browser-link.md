---
uid: visual-studio/overview/2013/using-browser-link
title: Verwenden von Browser Link in Visual Studio 2013 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505203"
---
# <a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="510b3-102">Verwenden von Browser Link in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="510b3-102">Using Browser Link in Visual Studio 2013</span></span>

<span data-ttu-id="510b3-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="510b3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="510b3-104">Browser Link ist ein neues Feature in Visual Studio 2013, das einen Kommunikationskanal zwischen der Entwicklungsumgebung und einem oder mehreren Webbrowsern erstellt.</span><span class="sxs-lookup"><span data-stu-id="510b3-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="510b3-105">Sie können die Browser Verknüpfung verwenden, um Ihre Webanwendung in mehreren Browsern gleichzeitig zu aktualisieren, was für Browser übergreifende Tests nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="510b3-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="510b3-106">Browser Aktualisierung</span><span class="sxs-lookup"><span data-stu-id="510b3-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="510b3-107">Anzeigen des Browser Link-Dashboards</span><span class="sxs-lookup"><span data-stu-id="510b3-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="510b3-108">Aktivieren von Browser Link für statische HTML-Dateien</span><span class="sxs-lookup"><span data-stu-id="510b3-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="510b3-109">Browser Verknüpfung wird deaktiviert</span><span class="sxs-lookup"><span data-stu-id="510b3-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="510b3-110">Wie funktioniert es?</span><span class="sxs-lookup"><span data-stu-id="510b3-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="510b3-111">Browser Aktualisierung</span><span class="sxs-lookup"><span data-stu-id="510b3-111">Browser Refresh</span></span>

<span data-ttu-id="510b3-112">Mit der Browser Aktualisierung können Sie mehrere Browser aktualisieren, die mit Visual Studio über den Browser Link verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="510b3-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="510b3-113">Um die Browser Aktualisierung zu verwenden, erstellen Sie zuerst eine ASP.NET-Anwendung, indem Sie eine der Projektvorlagen verwenden.</span><span class="sxs-lookup"><span data-stu-id="510b3-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="510b3-114">Debuggen Sie die Anwendung, indem Sie F5 drücken oder auf der Symbolleiste auf das Pfeilsymbol klicken:</span><span class="sxs-lookup"><span data-stu-id="510b3-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="510b3-115">Sie können auch die Dropdown Liste verwenden, um einen bestimmten Browser zum Debuggen auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="510b3-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="510b3-116">Wählen Sie zum Debuggen mit mehreren Browsern **Durchsuchen mit**aus.</span><span class="sxs-lookup"><span data-stu-id="510b3-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="510b3-117">Halten Sie im Dialogfeld " **Durchsuchen** " die STRG-Taste gedrückt, um mehr als einen Browser auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="510b3-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="510b3-118">Klicken Sie zum Debuggen mit den ausgewählten Browsern auf **Durchsuchen** .</span><span class="sxs-lookup"><span data-stu-id="510b3-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="510b3-119">Der Browser Link funktioniert auch, wenn Sie einen Browser von außerhalb von Visual Studio starten und zur Anwendungs-URL navigieren.</span><span class="sxs-lookup"><span data-stu-id="510b3-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="510b3-120">Die Browser Link-Steuerelemente befinden sich in der Dropdown Liste mit dem kreisförmigen Pfeilsymbol.</span><span class="sxs-lookup"><span data-stu-id="510b3-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="510b3-121">Das Pfeilsymbol ist die Schaltfläche " **Aktualisieren** ".</span><span class="sxs-lookup"><span data-stu-id="510b3-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="510b3-122">Um anzuzeigen, welche Browser verbunden sind, bewegen Sie den Mauszeiger über die Schaltfläche **Aktualisieren** während des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="510b3-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="510b3-123">Die verbundenen Browser werden in einem QuickInfo-Fenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="510b3-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="510b3-124">Um die verbundenen Browser zu aktualisieren, klicken Sie auf die Schaltfläche **Aktualisieren** , oder drücken Sie STRG + ALT + EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="510b3-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="510b3-125">Der folgende Screenshot zeigt z. b. ein ASP.net-Projekt, das ich mit der MVC 5-Projektvorlage erstellt habe.</span><span class="sxs-lookup"><span data-stu-id="510b3-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="510b3-126">Sie können sehen, dass die Anwendung in zwei Browsern am oberen Rand ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="510b3-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="510b3-127">Im unteren Bereich ist das Projekt in Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="510b3-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="510b3-128">In Visual Studio habe ich die &lt;H1-&gt; der Überschrift für die Startseite geändert:</span><span class="sxs-lookup"><span data-stu-id="510b3-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="510b3-129">Beim Klicken auf die Schaltfläche " **Aktualisieren** " trat die Änderung in beiden Browserfenstern auf:</span><span class="sxs-lookup"><span data-stu-id="510b3-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="510b3-130">**Notizen**</span><span class="sxs-lookup"><span data-stu-id="510b3-130">**Notes**</span></span>

- <span data-ttu-id="510b3-131">Um die Browser Verknüpfung zu aktivieren, legen Sie `debug=true` im [&lt;-Kompilierungs&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) -Element in der Datei "Web. config" des Projekts fest.</span><span class="sxs-lookup"><span data-stu-id="510b3-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="510b3-132">Die Anwendung muss auf "localhost" ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="510b3-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="510b3-133">Die Anwendung muss auf .NET 4,0 oder höher ausgerichtet sein.</span><span class="sxs-lookup"><span data-stu-id="510b3-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="510b3-134">Anzeigen des Browser Link-Dashboards</span><span class="sxs-lookup"><span data-stu-id="510b3-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="510b3-135">Das Browser Link-Dashboard zeigt Informationen zu den Browser Link Verbindungen an.</span><span class="sxs-lookup"><span data-stu-id="510b3-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="510b3-136">Um das Dashboard anzuzeigen, klicken Sie auf das Dropdown Menü Browser Link (der kleine Pfeil neben der Schaltfläche **Aktualisieren** ).</span><span class="sxs-lookup"><span data-stu-id="510b3-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="510b3-137">Klicken Sie dann auf **Browser Link Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="510b3-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="510b3-138">Das Dashboard listet die verbundenen Browser und die URL auf, auf die die einzelnen Browser navigiert sind.</span><span class="sxs-lookup"><span data-stu-id="510b3-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="510b3-139">Im Abschnitt **Voraussetzungen** werden alle Schritte angezeigt, die zum Aktivieren des Browser Links für dieses Projekt erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="510b3-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="510b3-140">Der folgende Screenshot zeigt z. b. ein Projekt, in dem "Debug" in der Datei "Web. config" auf "false" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="510b3-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="510b3-141">Aktivieren von Browser Link für statische HTML-Dateien</span><span class="sxs-lookup"><span data-stu-id="510b3-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="510b3-142">Fügen Sie der Datei "Web. config" Folgendes hinzu, um den Browser Link für statische HTML-Dateien zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="510b3-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="510b3-143">Entfernen Sie diese Einstellung aus Leistungsgründen, wenn Sie Ihr Projekt veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="510b3-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="510b3-144">Browser Verknüpfung wird deaktiviert</span><span class="sxs-lookup"><span data-stu-id="510b3-144">Disabling Browser Link</span></span>

<span data-ttu-id="510b3-145">Browser Link ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="510b3-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="510b3-146">Es gibt mehrere Möglichkeiten, Sie zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="510b3-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="510b3-147">Deaktivieren Sie im Dropdown Menü Browser Link die Option **Browser Link aktivieren**.</span><span class="sxs-lookup"><span data-stu-id="510b3-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="510b3-148">Fügen Sie in der Datei "Web. config" im Abschnitt "appSettings" einen Schlüssel mit dem Namen "vs: enablebrowserlink" mit dem Wert "false" hinzu.</span><span class="sxs-lookup"><span data-stu-id="510b3-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="510b3-149">Legen Sie in der Datei Web. config Debuggen auf false fest.</span><span class="sxs-lookup"><span data-stu-id="510b3-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="510b3-150">Wie funktioniert es?</span><span class="sxs-lookup"><span data-stu-id="510b3-150">How Does It Work?</span></span>

<span data-ttu-id="510b3-151">Der Browser Link verwendet [signalr](../../../signalr/index.md) , um einen Kommunikationskanal zwischen Visual Studio und dem Browser zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="510b3-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="510b3-152">Wenn der Browser Link aktiviert ist, fungiert Visual Studio als signalr-Server, mit dem mehrere Clients (Browser) eine Verbindung herstellen können.</span><span class="sxs-lookup"><span data-stu-id="510b3-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="510b3-153">Mit dem Browser Link wird auch ein HTTP-Modul bei ASP.net registriert.</span><span class="sxs-lookup"><span data-stu-id="510b3-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="510b3-154">Dieses Modul fügt besondere &lt;Skripts&gt; Verweise auf jede Seiten Anforderung vom Server ein.</span><span class="sxs-lookup"><span data-stu-id="510b3-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="510b3-155">Die Skript Verweise können angezeigt werden, indem Sie im Browser "Quelle anzeigen" auswählen.</span><span class="sxs-lookup"><span data-stu-id="510b3-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="510b3-156">Die Quelldateien werden nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="510b3-156">Your source files are not modified.</span></span> <span data-ttu-id="510b3-157">Das HTTP-Modul fügt die Skript Verweise dynamisch ein.</span><span class="sxs-lookup"><span data-stu-id="510b3-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="510b3-158">Da es sich bei dem Browser seitigen Code um JavaScript handelt, funktioniert er für alle Browser, die von [signalr unterstützt](../../../signalr/overview/getting-started/supported-platforms.md)werden, ohne dass ein Browser-Plug-in erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="510b3-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
