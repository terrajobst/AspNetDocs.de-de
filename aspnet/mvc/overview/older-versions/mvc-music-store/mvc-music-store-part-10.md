---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Teil 10: endgültige Updates für Navigation und Standort Entwurf, Schlussfolgerung | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 10 behandelt abschließende Updates für Navigation und S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433611"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="ea570-104">Teil 10: endgültige Updates für Navigation und Standort Entwurf, Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="ea570-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="ea570-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ea570-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ea570-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ea570-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ea570-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="ea570-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ea570-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="ea570-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ea570-109">Teil 10 behandelt abschließende Aktualisierungen der Navigation und des Website Entwurfs, der Schlussfolgerung.</span><span class="sxs-lookup"><span data-stu-id="ea570-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="ea570-110">Wir haben alle wichtigen Funktionen für unsere Website abgeschlossen, aber wir haben noch einige Features, die Sie der Website Navigation, der Startseite und der Store-Seite zum Durchsuchen hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="ea570-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="ea570-111">Erstellen der Teilansicht der Einkaufswagen Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ea570-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="ea570-112">Wir möchten die Anzahl der Elemente im Warenkorb des Benutzers auf der gesamten Website verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="ea570-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="ea570-113">Dies kann problemlos implementiert werden, indem eine Teilansicht erstellt wird, die zu "Site. Master" hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="ea570-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="ea570-114">Wie bereits gezeigt, enthält der ShoppingCart-Controller eine cartsummary-Aktionsmethode, die eine Teilansicht zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="ea570-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="ea570-115">Klicken Sie mit der rechten Maustaste auf den Ordner views/ShoppingCart, und wählen Sie Ansicht hinzufügen aus, um die Teilansicht von cartsummary zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea570-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="ea570-116">Benennen Sie die Ansicht cartsummary, und aktivieren Sie das Kontrollkästchen "partielle Ansicht erstellen", wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ea570-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="ea570-117">Die Teilansicht "cartsummary" ist wirklich einfach. es handelt sich lediglich um einen Link zur ShoppingCart-Index Sicht, in der die Anzahl der Elemente im Warenkorb angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ea570-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="ea570-118">Der vollständige Code für "cartsummary. cshtml" lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ea570-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="ea570-119">Mithilfe der HTML. renderaction-Methode können wir eine Teilansicht in eine beliebige Seite der Site einschließen, einschließlich des Standort Masters.</span><span class="sxs-lookup"><span data-stu-id="ea570-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="ea570-120">Renderaction erfordert, dass wir den Aktions Namen ("cartsummary") und den Controller Namen ("ShoppingCart") wie unten beschrieben angeben.</span><span class="sxs-lookup"><span data-stu-id="ea570-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="ea570-121">Vor dem Hinzufügen des Website Layouts wird auch das Genre Menü erstellt, damit alle Website-. Master-Updates gleichzeitig durchführen können.</span><span class="sxs-lookup"><span data-stu-id="ea570-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="ea570-122">Erstellen der Teilansicht für Genre Menü</span><span class="sxs-lookup"><span data-stu-id="ea570-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="ea570-123">Wir können es unseren Benutzern sehr viel leichter machen, durch den Store zu navigieren, indem Sie ein Genre Menü hinzufügen, in dem alle im Store verfügbaren Genres aufgelistet sind.</span><span class="sxs-lookup"><span data-stu-id="ea570-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="ea570-124">Wir führen die gleichen Schritte aus, um auch eine partielle genremenu-Ansicht zu erstellen. Anschließend können wir beide dem Website Master hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea570-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="ea570-125">Fügen Sie zunächst die folgende genremenu-Controller Aktion zum StoreController hinzu:</span><span class="sxs-lookup"><span data-stu-id="ea570-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="ea570-126">Mit dieser Aktion wird eine Liste der Genres zurückgegeben, die von der Teilansicht angezeigt werden, die wir als Nächstes erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea570-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="ea570-127">*Hinweis: Wir haben der Controller Aktion das Attribut [childaction only] hinzugefügt, das angibt, dass diese Aktion nur von einer Teilansicht aus verwendet werden soll. Dieses Attribut verhindert, dass die Controller Aktion durch Browsen zu/Store/GenreMenu. ausgeführt wird. Dies ist für Teilansichten nicht erforderlich. es ist jedoch eine bewährte Vorgehensweise, da wir sicherstellen möchten, dass unsere Controller Aktionen wie geplant verwendet werden. Wir geben auch "partialview" anstelle von "View" zurück, sodass das Ansichts Modul weiß, dass es das Layout für diese Ansicht nicht verwenden sollte, da es in andere Ansichten eingeschlossen wird.*</span><span class="sxs-lookup"><span data-stu-id="ea570-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="ea570-128">Klicken Sie mit der rechten Maustaste auf die Aktion genremenu Controller, und erstellen Sie eine partielle Ansicht mit dem Namen genremenu, die stark typisiert ist, indem Sie die Datenklasse Genre Ansicht verwenden</span><span class="sxs-lookup"><span data-stu-id="ea570-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="ea570-129">Aktualisieren Sie den Ansichts Code für die partielle genremenu-Ansicht, um die Elemente mithilfe einer ungeordneten Liste wie folgt anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ea570-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="ea570-130">Das Website Layout wird aktualisiert, um die Teilansichten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ea570-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="ea570-131">Wir können unsere Teilansichten dem Website Layout (/Views/Shared/\_Layout. cshtml) hinzufügen, indem Sie "HTML. renderaction ()" aufrufen.</span><span class="sxs-lookup"><span data-stu-id="ea570-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="ea570-132">Wir fügen Sie sowohl in als auch ein zusätzliches Markup hinzu, um Sie anzuzeigen, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="ea570-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="ea570-133">Wenn wir nun die Anwendung ausführen, sehen wir das Genre im linken Navigationsbereich und die Warenkorb-Übersicht oben.</span><span class="sxs-lookup"><span data-stu-id="ea570-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="ea570-134">Aktualisieren auf die Store-Suchseite</span><span class="sxs-lookup"><span data-stu-id="ea570-134">Update to the Store Browse page</span></span>

<span data-ttu-id="ea570-135">Die Store-Seite zum Durchsuchen ist funktionsfähig, ist aber nicht sehr gut geeignet.</span><span class="sxs-lookup"><span data-stu-id="ea570-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="ea570-136">Wir können die Seite aktualisieren, um die Alben in einem besseren Layout anzuzeigen, indem Sie den Ansichts Code (in/views/Store/Browse.cshtml) wie folgt aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="ea570-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="ea570-137">Hier verwenden wir "URL. action" anstelle von "HTML. Action Link", damit wir eine spezielle Formatierung auf den Link anwenden können, um die Album-Grafik einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="ea570-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="ea570-138">*Hinweis: Es wird eine generische Album Abdeckung für diese Alben angezeigt. Diese Informationen werden in der-Datenbank gespeichert und sind über den Speicher-Manager bearbeitbar. Sie sind herzlich Willkommen beim Hinzufügen Ihrer eigenen Grafik.*</span><span class="sxs-lookup"><span data-stu-id="ea570-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="ea570-139">Wenn wir nun zu einem Genre navigieren, werden die in einem Raster gezeigten Alben mit der Album-Grafik angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ea570-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="ea570-140">Aktualisieren der Startseite zum Anzeigen der Top Selling-Alben</span><span class="sxs-lookup"><span data-stu-id="ea570-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="ea570-141">Wir möchten unsere Top Selling-Alben auf der Startseite präsentieren, um den Vertrieb zu steigern.</span><span class="sxs-lookup"><span data-stu-id="ea570-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="ea570-142">Wir nehmen einige Aktualisierungen an unserem HomeController vor, um dies zu bewältigen, und fügen auch einige zusätzliche Grafiken hinzu.</span><span class="sxs-lookup"><span data-stu-id="ea570-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="ea570-143">Zuerst fügen wir der Album-Klasse eine Navigations Eigenschaft hinzu, damit EntityFramework weiß, dass Sie verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="ea570-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="ea570-144">Die letzten Zeilen der **Album** -Klasse sollten nun wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="ea570-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="ea570-145">*Hinweis: dazu muss eine using-Anweisung hinzugefügt werden, um den System. Collections. Generic-Namespace zu verwenden.*</span><span class="sxs-lookup"><span data-stu-id="ea570-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="ea570-146">Zuerst fügen wir ein storedb-Feld und mvcmusicstore. Models mithilfe von-Anweisungen hinzu, wie in unseren anderen Controllern.</span><span class="sxs-lookup"><span data-stu-id="ea570-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="ea570-147">Als Nächstes fügen wir dem HomeController die folgende Methode hinzu, die die Datenbank abfragt, um die nachfolgend aufgeführten Alben nach OrderDetails zu suchen.</span><span class="sxs-lookup"><span data-stu-id="ea570-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="ea570-148">Dies ist eine private Methode, da wir Sie nicht als Controller Aktion zur Verfügung stellen möchten.</span><span class="sxs-lookup"><span data-stu-id="ea570-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="ea570-149">Wir fügen ihn aus Gründen der Einfachheit in den HomeController ein. es wird jedoch empfohlen, die Geschäftslogik nach Bedarf in separate Dienst Klassen zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="ea570-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="ea570-150">Damit können wir die Index Controller Aktion aktualisieren, um die fünf häufigsten verkauften Alben abzufragen und Sie an die Ansicht zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="ea570-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="ea570-151">Der gesamte Code für den aktualisierten HomeController ist wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ea570-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="ea570-152">Schließlich müssen wir unsere Start Index Ansicht aktualisieren, damit Sie eine Liste der Alben anzeigen kann, indem Sie den Modelltyp aktualisieren und die Liste der Alben unten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea570-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="ea570-153">Wir nutzen diese Gelegenheit, um der Seite auch eine Überschrift und einen promotionbereich hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea570-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="ea570-154">Wenn wir nun die Anwendung ausführen, sehen wir unsere aktualisierte Startseite mit Top Selling Alben und unsere Werbe Meldung.</span><span class="sxs-lookup"><span data-stu-id="ea570-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="ea570-155">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ea570-155">Conclusion</span></span>

<span data-ttu-id="ea570-156">Wir haben gesehen, dass ASP.NET MVC das Erstellen einer ausgereiften Website mit Datenbankzugriff, Mitgliedschaft, AJAX usw. erleichtert.</span><span class="sxs-lookup"><span data-stu-id="ea570-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="ea570-157">ziemlich schnell.</span><span class="sxs-lookup"><span data-stu-id="ea570-157">pretty quickly.</span></span> <span data-ttu-id="ea570-158">Hoffentlich haben Sie in diesem Tutorial die Tools erhalten, die Sie benötigen, um mit dem Aufbau eigener ASP.NET-MVC-Anwendungen zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="ea570-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ea570-159">Previous</span><span class="sxs-lookup"><span data-stu-id="ea570-159">Previous</span></span>](mvc-music-store-part-9.md)
