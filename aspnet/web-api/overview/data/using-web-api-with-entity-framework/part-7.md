---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Erstellen der Sicht (UI) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448983"
---
# <a name="create-the-view-ui"></a><span data-ttu-id="2a0b0-102">Erstellen der Ansicht (Benutzeroberfläche)</span><span class="sxs-lookup"><span data-stu-id="2a0b0-102">Create the View (UI)</span></span>

<span data-ttu-id="2a0b0-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2a0b0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2a0b0-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="2a0b0-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="2a0b0-105">In diesem Abschnitt definieren Sie zunächst den HTML-Code für die APP und fügen die Datenbindung zwischen dem HTML-und dem Ansichts Modell hinzu.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="2a0b0-106">Öffnen Sie die Datei views/Home/Index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="2a0b0-107">Ersetzen Sie den gesamten Inhalt dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2a0b0-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="2a0b0-108">Die meisten `div` Elemente sind für die [Bootstrap](http://getbootstrap.com/) -Formatierung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="2a0b0-109">Die wichtigen Elemente sind diejenigen mit `data-bind` Attributen.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="2a0b0-110">Dieses Attribut verknüpft den HTML-Code mit dem Ansichts Modell.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="2a0b0-111">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2a0b0-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="2a0b0-112">In diesem Beispiel bewirkt die &quot;`text`&quot; Bindung, dass das `<p>`-Element den Wert der `error`-Eigenschaft aus dem Ansichts Modell anzeigt.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="2a0b0-113">Beachten Sie, dass `error` als `ko.observable`deklariert wurde:</span><span class="sxs-lookup"><span data-stu-id="2a0b0-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="2a0b0-114">Wenn `error`ein neuer Wert zugewiesen wird, aktualisiert Knockout den Text im `<p>` Element.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="2a0b0-115">Die `foreach` Bindung weist Knockout an, den Inhalt des `books` Arrays zu durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="2a0b0-116">Für jedes Element im Array erstellt Knockout ein neues &lt;Li&gt;-Element.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="2a0b0-117">Bindungen innerhalb des Kontexts des `foreach` verweisen auf Eigenschaften des Array Elements.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="2a0b0-118">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2a0b0-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="2a0b0-119">Hier liest die `text` Bindung die Author-Eigenschaft jedes Buchs.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="2a0b0-120">Wenn Sie die Anwendung jetzt ausführen, sollte Sie wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="2a0b0-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="2a0b0-121">Die Liste der Bücher wird asynchron geladen, nachdem die Seite geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="2a0b0-122">Derzeit sind die &quot;Details&quot; Verknüpfungen nicht funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="2a0b0-123">Diese Funktion wird im nächsten Abschnitt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="2a0b0-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a0b0-124">[Zurück](part-6.md)
> [Weiter](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="2a0b0-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
