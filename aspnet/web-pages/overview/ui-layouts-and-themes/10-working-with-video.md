---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Anzeigen von Videos auf einer ASP.net Web Pages (Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Videos in einem ASP.net Web Pages mit Razor-Syntax Seite angezeigt werden.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510381"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="aa8ec-103">Anzeigen von Videos auf einer ASP.net Web Pages-Website (Razor)</span><span class="sxs-lookup"><span data-stu-id="aa8ec-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="aa8ec-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa8ec-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa8ec-105">In diesem Artikel wird erläutert, wie Sie einen Video Player (Media) auf einer ASP.net Web Pages-Website (Razor) verwenden, um Benutzern das Anzeigen von Videos zu gestatten, die auf der Website gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="aa8ec-106">ASP.net Web Pages mit Razor-Syntax können Sie Flash-( *. SWF*), Media Player-( *. wmv*) und Silverlight-Videos ( *. xap*) abspielen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="aa8ec-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="aa8ec-108">Auswählen eines Video Players.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-108">How to choose a video player.</span></span>
> - <span data-ttu-id="aa8ec-109">Gewusst wie: Hinzufügen von Videos zu einer Webseite</span><span class="sxs-lookup"><span data-stu-id="aa8ec-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="aa8ec-110">Festlegen von Video Player Attributen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="aa8ec-111">Dies sind die ASP.net Razor Pages-Funktionen, die im Artikel eingeführt wurden:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="aa8ec-112">Das `Video`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aa8ec-113">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="aa8ec-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="aa8ec-114">ASP.net Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="aa8ec-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="aa8ec-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="aa8ec-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="aa8ec-116">Dieses Tutorial funktioniert auch mit webmatrix 3.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-116">This tutorial also works with WebMatrix 3.</span></span>

## <a name="introduction"></a><span data-ttu-id="aa8ec-117">Einführung</span><span class="sxs-lookup"><span data-stu-id="aa8ec-117">Introduction</span></span>

<span data-ttu-id="aa8ec-118">Möglicherweise möchten Sie ein Video auf Ihrer Website anzeigen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-118">You might want to display a video on your site.</span></span> <span data-ttu-id="aa8ec-119">Eine Möglichkeit besteht darin, einen Link zu einer Website zu verwenden, die bereits über das Video verfügt, wie z. b. YouTube.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="aa8ec-120">Wenn Sie ein Video von diesen Websites direkt auf Ihren eigenen Seiten einbetten möchten, können Sie in der Regel ein HTML-Markup von der Website erhalten und es dann auf die Seite kopieren.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="aa8ec-121">Das folgende Beispiel zeigt beispielsweise, wie Sie ein YouTube-Video einbetten:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="aa8ec-122">Wenn Sie ein Video abspielen möchten, das sich auf Ihrer eigenen Website befindet (nicht auf einer öffentlichen Video Freigabe Website), können Sie nicht direkt mit einem eingebetteten Markup wie diesem verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="aa8ec-123">Sie können jedoch Videos von Ihrer Website abspielen, indem Sie das `Video`-Hilfsprogramm verwenden, das einen Media Player direkt auf einer Seite rendert.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="aa8ec-124">Auswählen eines Video Players</span><span class="sxs-lookup"><span data-stu-id="aa8ec-124">Choosing a Video Player</span></span>

<span data-ttu-id="aa8ec-125">Es gibt viele Formate für Videodateien, und jedes Format erfordert in der Regel einen anderen Player und eine andere Möglichkeit zum Konfigurieren des Players.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="aa8ec-126">In ASP.net Razor Pages können Sie ein Video mit dem `Video`-Hilfsprogramm auf einer Webseite abspielen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="aa8ec-127">Das `Video`-Hilfsprogramm vereinfacht das Einbetten von Videos in eine Webseite, da es automatisch die `object` und `embed` HTML-Elemente generiert, die normalerweise zum Hinzufügen von Videos zur Seite verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="aa8ec-128">Das `Video`-Hilfsprogramm unterstützt die folgenden Medien Player:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="aa8ec-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="aa8ec-129">Adobe Flash</span></span>
- <span data-ttu-id="aa8ec-130">Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="aa8ec-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="aa8ec-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="aa8ec-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="aa8ec-132">Der Flash Player</span><span class="sxs-lookup"><span data-stu-id="aa8ec-132">The Flash Player</span></span>

<span data-ttu-id="aa8ec-133">Der `Flash` Player des `Video`-Hilfsprogramms ermöglicht das Abspielen von Flash-Videos ( *. SWF* -Dateien) auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="aa8ec-134">Sie müssen mindestens einen Pfad zur Videodatei angeben.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="aa8ec-135">Wenn Sie nur den Pfad angeben, verwendet der Player Standardwerte, die von der aktuellen Version von Flash festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="aa8ec-136">Typische Standardeinstellungen:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-136">Typical default settings are:</span></span>

- <span data-ttu-id="aa8ec-137">Das Video wird unter Verwendung der Standardbreite und-Höhe und ohne Hintergrundfarbe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="aa8ec-138">Das Video wird automatisch abgespielt, wenn die Seite geladen wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="aa8ec-139">Das Video wird fortlaufend so lange Schleifen, bis es explizit beendet wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="aa8ec-140">Das Video wird so skaliert, dass es das gesamte Video anzeigt, anstatt das Video an eine bestimmte Größe anzupassen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="aa8ec-141">Das Video wird in einem Fenster abgespielt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="aa8ec-142">Der Media Player-Player</span><span class="sxs-lookup"><span data-stu-id="aa8ec-142">The MediaPlayer Player</span></span>

<span data-ttu-id="aa8ec-143">Mit dem `MediaPlayer` Player des `Video`-Hilfsprogramms können Sie Windows Media-Videos (*WMV* -Dateien), Windows Media-Audiodateien (*WMA* -Dateien) und MP3-Dateien (*MP3* -Dateien) auf einer Webseite abspielen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="aa8ec-144">Sie müssen den Pfad der zu Wiedergabe enden Mediendatei einschließen. alle anderen Parameter sind optional.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="aa8ec-145">Wenn Sie nur einen Pfad angeben, verwendet der Spieler die Standardeinstellungen, die von der aktuellen Media Player-Version festgelegt werden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="aa8ec-146">Das Video wird unter Verwendung der Standardbreite und-Höhe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="aa8ec-147">Das Video wird automatisch abgespielt, wenn die Seite geladen wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="aa8ec-148">Das Video wird einmal abgespielt (keine Schleife).</span><span class="sxs-lookup"><span data-stu-id="aa8ec-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="aa8ec-149">Der Player zeigt den vollständigen Satz von Steuerelementen in der Benutzeroberfläche an.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="aa8ec-150">Das Video wird in einem Fenster abgespielt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="aa8ec-151">Der Silverlight-Player</span><span class="sxs-lookup"><span data-stu-id="aa8ec-151">The Silverlight Player</span></span>

<span data-ttu-id="aa8ec-152">Mit dem `Silverlight` Player des `Video`-Hilfsprogramms können Sie Windows Media Video (*WMV* -Dateien), Windows Media Audio ( *. WMA* -Dateien) und MP3-*Dateien (MP3-Dateien)* abspielen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="aa8ec-153">Sie müssen den path-Parameter so festlegen, dass er auf ein Silverlight-basiertes Anwendungspaket ( *. xap* -Datei) verweist.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="aa8ec-154">Außerdem müssen Sie die Parameter "width" und "Height" festlegen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="aa8ec-155">Alle anderen Parameter sind optional.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-155">All other parameters are optional.</span></span> <span data-ttu-id="aa8ec-156">Wenn Sie den Silverlight Player für Video verwenden und nur die erforderlichen Parameter festlegen, zeigt der Silverlight-Player das Video ohne Hintergrundfarbe an.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="aa8ec-157">Für den Fall, dass Sie Silverlight nicht bereits kennen: die *XAP* -Datei ist eine komprimierte Datei, die Layoutanweisungen in einer *XAML* -Datei, verwalteten Code in Assemblys und optionale Ressourcen enthält.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="aa8ec-158">Sie können eine *XAP* -Datei in Visual Studio als Silverlight-Anwendungsprojekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>

<span data-ttu-id="aa8ec-159">Der `Silverlight` Video Player verwendet die Einstellungen, die Sie für den Player bereitstellen, und die Einstellungen, die in der *XAP* -Datei bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="aa8ec-160">MIME-Typen</span><span class="sxs-lookup"><span data-stu-id="aa8ec-160">MIME Types</span></span>
> 
> <span data-ttu-id="aa8ec-161">Wenn ein Browser eine Datei herunterlädt, stellt der Browser sicher, dass der Dateityp mit dem MIME-Typ übereinstimmt, der für das gerenderte Dokument angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="aa8ec-162">Der MIME-Typ ist der Inhaltstyp oder Medientyp einer Datei.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="aa8ec-163">Das `Video`-Hilfsprogramm verwendet die folgenden MIME-Typen:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="aa8ec-164">Abspielen von Flash-Videos (. swf)</span><span class="sxs-lookup"><span data-stu-id="aa8ec-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="aa8ec-165">In diesem Verfahren wird gezeigt, wie Sie ein Flash Video mit dem Namen *Sample. SWF*abspielen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="aa8ec-166">Bei diesem Verfahren wird davon ausgegangen, dass Sie einen Ordner mit dem Namen " *Media* " auf Ihrer Site haben und sich die *. SWF* -Datei in diesem Ordner befindet.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="aa8ec-167">Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="aa8ec-168">Fügen Sie auf der Website eine Seite hinzu, und nennen Sie Sie " *Flash Video. cshtml*".</span><span class="sxs-lookup"><span data-stu-id="aa8ec-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="aa8ec-169">Fügen Sie der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="aa8ec-170">Führen Sie die Seite in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-170">Run the page in a browser.</span></span> <span data-ttu-id="aa8ec-171">(Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Die Seite wird angezeigt, und das Video wird automatisch abgespielt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="aa8ec-172">![Klang](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")</span><span class="sxs-lookup"><span data-stu-id="aa8ec-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="aa8ec-173">Sie können den `quality`-Parameter für ein Flash Video auf `low`, `autolow`, `autohigh`, `medium`, `high`und `best`festlegen:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="aa8ec-174">Mit dem `scale`-Parameter können Sie das Flash Video so ändern, dass es mit einer bestimmten Größe wiedergegeben wird, und Sie können Folgendes festlegen:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="aa8ec-175">[https://login.microsoftonline.com/consumers/](`showall`).</span><span class="sxs-lookup"><span data-stu-id="aa8ec-175">`showall`.</span></span> <span data-ttu-id="aa8ec-176">Dadurch wird das gesamte Video sichtbar, während das ursprüngliche Seitenverhältnis beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="aa8ec-177">Es kann jedoch sein, dass auf jeder Seite Grenzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="aa8ec-178">[https://login.microsoftonline.com/consumers/](`noorder`).</span><span class="sxs-lookup"><span data-stu-id="aa8ec-178">`noorder`.</span></span> <span data-ttu-id="aa8ec-179">Dadurch wird das Video skaliert, während das ursprüngliche Seitenverhältnis beibehalten wird, es wird jedoch möglicherweise abgeschnitten.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="aa8ec-180">[https://login.microsoftonline.com/consumers/](`exactfit`).</span><span class="sxs-lookup"><span data-stu-id="aa8ec-180">`exactfit`.</span></span> <span data-ttu-id="aa8ec-181">Dadurch wird das gesamte Video sichtbar, ohne dass das ursprüngliche Seitenverhältnis beibehalten wird, aber es kann zu einer Verzerrung kommen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="aa8ec-182">Wenn Sie keinen `scale` Parameter angeben, wird das gesamte Video angezeigt, und das ursprüngliche Seitenverhältnis wird ohne Zuschneiden beibehalten.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="aa8ec-183">Im folgenden Beispiel wird gezeigt, wie der `scale`-Parameter verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="aa8ec-184">Der Flash Player unterstützt eine videomoduseinstellung mit dem Namen `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="aa8ec-185">Sie können dies auf `window`, `opaque`und `transparent`festlegen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="aa8ec-186">Standardmäßig ist die `windowMode` auf `window`festgelegt, wodurch das Video in einem separaten Fenster auf der Webseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="aa8ec-187">Die `opaque` Einstellung blendet alles hinter dem Video auf der Webseite aus.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="aa8ec-188">Mit der Einstellung "`transparent`" kann der Hintergrund der Webseite durch das Video angezeigt werden, wenn ein beliebiger Teil des Videos transparent ist.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="aa8ec-189">Abspielen von Media Player-Videos ( *. wmv*)</span><span class="sxs-lookup"><span data-stu-id="aa8ec-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="aa8ec-190">Im folgenden Verfahren wird gezeigt, wie Sie ein Windows Media-Video namens *Sample. wmv* abspielen, das sich im Ordner " *Media* " befindet.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="aa8ec-191">Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="aa8ec-192">Erstellen Sie eine neue Seite mit dem Namen *mediaplayervideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="aa8ec-193">Fügen Sie der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="aa8ec-194">Führen Sie die Seite in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-194">Run the page in a browser.</span></span> <span data-ttu-id="aa8ec-195">Das Video wird automatisch geladen und abgespielt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="aa8ec-196">![Klang](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")</span><span class="sxs-lookup"><span data-stu-id="aa8ec-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="aa8ec-197">Sie können `playCount` auf eine ganze Zahl festlegen, die angibt, wie oft das Video automatisch abgespielt werden soll:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="aa8ec-198">Mit dem `uiMode`-Parameter können Sie angeben, welche Steuerelemente in der Benutzeroberfläche angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="aa8ec-199">Sie können `uiMode` auf `invisible`, `none`, `mini`oder `full`festlegen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="aa8ec-200">Wenn Sie keinen `uiMode` Parameter angeben, wird das Video zusätzlich zum Videofenster mit dem Statusfenster, der Suchleiste, den Steuerungs Schaltflächen und den volumesteuerelementen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="aa8ec-201">Diese Steuerelemente werden auch angezeigt, wenn Sie mit dem Player eine Audiodatei abspielen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="aa8ec-202">Im folgenden finden Sie ein Beispiel für die Verwendung des `uiMode`-Parameters:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="aa8ec-203">Standardmäßig ist die Audiodatei aktiviert, wenn das Video wiedergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="aa8ec-204">Sie können das audiostumm schalten, indem Sie den `mute`-Parameter auf "true" festlegen:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="aa8ec-205">Sie können die Audioebene des Media Player-Videos steuern, indem Sie den `volume`-Parameter auf einen Wert zwischen 0 und 100 festlegen.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="aa8ec-206">Der Standardwert lautet "50".</span><span class="sxs-lookup"><span data-stu-id="aa8ec-206">The default value is 50.</span></span> <span data-ttu-id="aa8ec-207">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="aa8ec-208">Abspielen von Silverlight-Videos</span><span class="sxs-lookup"><span data-stu-id="aa8ec-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="aa8ec-209">In diesem Verfahren wird gezeigt, wie Sie Videos abspielen, die auf der Seite "Silverlight *. xap* " in einem Ordner mit dem Namen " *Media*" enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="aa8ec-210">Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="aa8ec-211">Erstellen Sie eine neue Seite mit dem Namen " *silverlightvideo. cshtml*".</span><span class="sxs-lookup"><span data-stu-id="aa8ec-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="aa8ec-212">Fügen Sie der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="aa8ec-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="aa8ec-213">Führen Sie die Seite in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="aa8ec-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="aa8ec-214">![Klang](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")</span><span class="sxs-lookup"><span data-stu-id="aa8ec-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="aa8ec-215">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="aa8ec-215">Additional Resources</span></span>

<span data-ttu-id="aa8ec-216">[Übersicht über Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="aa8ec-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="aa8ec-217">Flash-Objekt und Einbettungs Tagattribute</span><span class="sxs-lookup"><span data-stu-id="aa8ec-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="aa8ec-218">[Windows Media Player 11 SDK-Parameter Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="aa8ec-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
