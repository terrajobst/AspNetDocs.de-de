---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Anzeigen von Zuordnungen in einer ASP.net Web Pages (Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird erläutert, wie Sie interaktive Karten auf Seiten auf einer ASP.net Web Pages-Website (Razor-Website) auf der Grundlage von Zuordnungs Diensten anzeigen können
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518721"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b4680-103">Anzeigen von Zuordnungen in einer ASP.net Web Pages (Razor-) Site</span><span class="sxs-lookup"><span data-stu-id="b4680-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="b4680-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b4680-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b4680-105">In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten auf einer ASP.net Web Pages-Website (Razor-Website) auf der Grundlage von Zuordnungs Diensten angezeigt werden, die von den von Ihnen, Google, Mapquest und</span><span class="sxs-lookup"><span data-stu-id="b4680-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="b4680-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b4680-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b4680-107">So generieren Sie eine Karte basierend auf einer Adresse</span><span class="sxs-lookup"><span data-stu-id="b4680-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="b4680-108">So generieren Sie eine Karte basierend auf breiten-und Längenkoordinaten.</span><span class="sxs-lookup"><span data-stu-id="b4680-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="b4680-109">Erfahren Sie, wie Sie ein Konto für die Verwendung von "wibmaps" registrieren und einen Schlüssel für die Verwendung mit "</span><span class="sxs-lookup"><span data-stu-id="b4680-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="b4680-110">Dies ist die im Artikel eingeführte ASP.net-Funktion:</span><span class="sxs-lookup"><span data-stu-id="b4680-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="b4680-111">Das `Maps`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="b4680-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b4680-112">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="b4680-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b4680-113">ASP.net Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="b4680-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="b4680-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="b4680-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="b4680-115">Dieses Tutorial funktioniert auch mit webmatrix 3.</span><span class="sxs-lookup"><span data-stu-id="b4680-115">This tutorial also works with WebMatrix 3.</span></span>

<span data-ttu-id="b4680-116">Auf Webseiten können Sie Maps auf einer Seite anzeigen, indem Sie `Maps`-Hilfsprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4680-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="b4680-117">Sie können Zuordnungen entweder auf einer Adresse oder in einem Satz von Längen-und breiten Koordinaten generieren.</span><span class="sxs-lookup"><span data-stu-id="b4680-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="b4680-118">Mit der `Maps`-Klasse können Sie beliebte Karten Module wie z. a. z, Google, Mapquest und Yahoo aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="b4680-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="b4680-119">Die Schritte zum Hinzufügen einer Zuordnung zu einer Seite sind identisch, unabhängig davon, welche der Zuordnungs-Engines Sie aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="b4680-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="b4680-120">Fügen Sie einfach einen JavaScript-Datei Verweis hinzu, der die verfügbaren Methoden zum Anzeigen der Zuordnung zur Verfügung stellt, und dann Methoden der `Maps`-Hilfsmethode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b4680-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="b4680-121">Sie wählen einen Kartendienst aus, der auf der von Ihnen verwendeten `Maps` Hilfsmethode basiert.</span><span class="sxs-lookup"><span data-stu-id="b4680-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="b4680-122">Sie können Folgendes verwenden:</span><span class="sxs-lookup"><span data-stu-id="b4680-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="b4680-123">Installieren der benötigten Teile</span><span class="sxs-lookup"><span data-stu-id="b4680-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="b4680-124">Um Maps anzuzeigen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b4680-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="b4680-125">Das `Maps`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="b4680-125">The `Maps` helper.</span></span> <span data-ttu-id="b4680-126">Dieses Hilfsprogramm ist in Version 2 der ASP.net-webhilfsprogramme-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="b4680-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="b4680-127">Wenn Sie die Bibliothek nicht bereits hinzugefügt haben, können Sie Sie als nuget-Paket auf Ihrer Website installieren.</span><span class="sxs-lookup"><span data-stu-id="b4680-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="b4680-128">Weitere Informationen finden Sie unter [Installieren von Hilfsprogrammen an einem ASP.net Web Pages-Standort](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="b4680-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="b4680-129">(Suchen Sie im Katalog nach dem `microsoft-web-helpers` Paket.)</span><span class="sxs-lookup"><span data-stu-id="b4680-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="b4680-130">Die jQuery-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="b4680-130">The jQuery library.</span></span> <span data-ttu-id="b4680-131">Einige der webmatrix-Website Vorlagen enthalten bereits jQuery-Bibliotheken in Ihren *Skript* Ordnern.</span><span class="sxs-lookup"><span data-stu-id="b4680-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="b4680-132">Wenn Sie nicht über diese Bibliotheken verfügen, können Sie die neueste jQuery-Bibliothek direkt von der [jQuery.org](http://jQuery.org) -Website herunterladen.</span><span class="sxs-lookup"><span data-stu-id="b4680-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="b4680-133">Oder Sie können eine neue Website mit einer Vorlage erstellen (z. b. die Vorlage " **Starter Site** ") und dann die jQuery-Dateien von dieser Website auf die aktuelle Website kopieren.</span><span class="sxs-lookup"><span data-stu-id="b4680-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="b4680-134">Wenn Sie abschließend die Verwendung von "Get Maps" wünschen, müssen Sie zunächst ein (freies) Konto erstellen und einen Schlüssel erhalten.</span><span class="sxs-lookup"><span data-stu-id="b4680-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="b4680-135">Um einen Schlüssel zu erhalten, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="b4680-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="b4680-136">Erstellen Sie ein Konto für das [Webkonto](https://www.microsoft.com/maps/developers/web.aspx)für die Entwicklung mit dem Konto "</span><span class="sxs-lookup"><span data-stu-id="b4680-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="b4680-137">Sie müssen auch über eine Microsoft-Konto (Windows Live ID) verfügen.</span><span class="sxs-lookup"><span data-stu-id="b4680-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="b4680-138">Sie können angeben, dass Sie den Schlüssel zum **Auswerten/testen**verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="b4680-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="b4680-139">Wenn Sie die Zuordnungs Funktion auf Ihrem eigenen Computer mithilfe von webmatrix und IIS Express testen, wechseln Sie zum Arbeitsbereich **Website** , und notieren Sie sich die URL Ihrer Website (z. b. `http://localhost:50408`, auch wenn die Portnummer wahrscheinlich anders ist).</span><span class="sxs-lookup"><span data-stu-id="b4680-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="b4680-140">Sie können diese *localhost* -Adresse als Website verwenden, wenn Sie registrieren.</span><span class="sxs-lookup"><span data-stu-id="b4680-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="b4680-141">Nachdem Sie sich für ein Konto registriert haben, navigieren Sie zum Konto Center für den Konto Center. Klicken Sie auf **Schlüssel erstellen oder anzeigen**:</span><span class="sxs-lookup"><span data-stu-id="b4680-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![Zuordnung-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="b4680-143">Notieren Sie den Schlüssel, den er erstellt.</span><span class="sxs-lookup"><span data-stu-id="b4680-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="b4680-144">Erstellen einer Karte basierend auf einer Adresse (mit Google)</span><span class="sxs-lookup"><span data-stu-id="b4680-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="b4680-145">Im folgenden Beispiel wird gezeigt, wie eine Seite erstellt wird, die eine Zuordnung auf der Grundlage einer Adresse rendert.</span><span class="sxs-lookup"><span data-stu-id="b4680-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="b4680-146">Dieses Beispiel zeigt die Verwendung von Google Maps.</span><span class="sxs-lookup"><span data-stu-id="b4680-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="b4680-147">Erstellen Sie eine Datei namens " *mapaddress. cshtml* " im Stammverzeichnis der Website.</span><span class="sxs-lookup"><span data-stu-id="b4680-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="b4680-148">Auf dieser Seite wird eine Karte basierend auf einer Adresse generiert, die Sie an Sie übergeben.</span><span class="sxs-lookup"><span data-stu-id="b4680-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="b4680-149">Kopieren Sie den folgenden Code in die Datei, und überschreiben Sie den vorhandenen Inhalt.</span><span class="sxs-lookup"><span data-stu-id="b4680-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="b4680-150">Beachten Sie die folgenden Funktionen der Seite:</span><span class="sxs-lookup"><span data-stu-id="b4680-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="b4680-151">Das `<script>` Element im `<head>` Element.</span><span class="sxs-lookup"><span data-stu-id="b4680-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="b4680-152">Im Beispiel verweist das `<script>`-Element auf die *jQuery-1.6.4. min. js* -Datei. Dies ist eine minierte (komprimierte) Version der jQuery-Bibliothek, Version 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="b4680-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="b4680-153">Beachten Sie, dass bei der Referenz davon ausgegangen wird, dass sich die *js* -Datei im Ordner *Scripts* Ihrer Website befindet.</span><span class="sxs-lookup"><span data-stu-id="b4680-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="b4680-154">Wenn Sie eine andere Version der jQuery-Bibliothek verwenden, stellen Sie sicher, dass Sie ordnungsgemäß auf diese Version verweisen.</span><span class="sxs-lookup"><span data-stu-id="b4680-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="b4680-155">Der aufzurufende `@Maps.GetGoogleHtml` im Text der Seite.</span><span class="sxs-lookup"><span data-stu-id="b4680-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="b4680-156">Um eine Adresse zuzuordnen, müssen Sie eine Adress Zeichenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="b4680-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="b4680-157">Die Methoden für die anderen Zuordnungs-Engines funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml``@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="b4680-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="b4680-158">Führen Sie die Seite aus und geben Sie eine Adresse ein.</span><span class="sxs-lookup"><span data-stu-id="b4680-158">Run the page and enter an address.</span></span> <span data-ttu-id="b4680-159">Auf der Seite wird eine auf Google Maps basierende Karte angezeigt, auf der der von Ihnen angegebene Speicherort angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b4680-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![Zuordnung-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="b4680-161">Erstellen einer Karte basierend auf breiten-und Längenkoordinaten (mit der Verwendung von "")</span><span class="sxs-lookup"><span data-stu-id="b4680-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="b4680-162">In diesem Beispiel wird gezeigt, wie eine Zuordnung basierend auf Koordinaten erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b4680-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="b4680-163">In diesem Beispiel wird gezeigt, wie Sie mit der Verwendung von "a.</span><span class="sxs-lookup"><span data-stu-id="b4680-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="b4680-164">(Sie können eine Zuordnung auf der Grundlage von Koordinaten erstellen, indem Sie auch die anderen Zuordnungs-Engines verwenden, ohne eine-Taste zu verwenden.)</span><span class="sxs-lookup"><span data-stu-id="b4680-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="b4680-165">Erstellen Sie im Stammverzeichnis der Website eine Datei namens *mapkoordinaten. cshtml* , und ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und das Markup:</span><span class="sxs-lookup"><span data-stu-id="b4680-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="b4680-166">Ersetzen Sie `your-key-here` durch den zuvor generierten Schlüssel von "mit der Version".</span><span class="sxs-lookup"><span data-stu-id="b4680-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="b4680-167">Führen Sie die Seite *mapkoordinaten. cshtml* aus, geben Sie breiten-und Längenkoordinaten ein, und klicken Sie dann auf die **Karte** .</span><span class="sxs-lookup"><span data-stu-id="b4680-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="b4680-168">.</span><span class="sxs-lookup"><span data-stu-id="b4680-168">button.</span></span> <span data-ttu-id="b4680-169">(Wenn Sie keine Koordinaten kennen, versuchen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b4680-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="b4680-170">Hierbei handelt es sich um einen Speicherort auf dem Microsoft Redmond-Campus.)</span><span class="sxs-lookup"><span data-stu-id="b4680-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="b4680-171">Breite: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="b4680-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="b4680-172">Längengrad:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="b4680-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="b4680-173">Die Seite wird mit den von Ihnen angegebenen Koordinaten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b4680-173">The page is displayed using the coordinates that you specified.</span></span>

     ![Zuordnung-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b4680-175">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b4680-175">Additional Resources</span></span>

[<span data-ttu-id="b4680-176">Microsoft. Maps-API-Referenz</span><span class="sxs-lookup"><span data-stu-id="b4680-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
