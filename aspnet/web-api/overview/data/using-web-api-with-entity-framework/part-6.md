---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Erstellen des JavaScript-Clients | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504723"
---
# <a name="create-the-javascript-client"></a><span data-ttu-id="11144-102">Erstellen des JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="11144-102">Create the JavaScript Client</span></span>

<span data-ttu-id="11144-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="11144-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="11144-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="11144-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="11144-105">In diesem Abschnitt erstellen Sie den Client für die Anwendung mithilfe von HTML, JavaScript und der [Knockout. js](http://knockoutjs.com/) -Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="11144-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="11144-106">Die Client-App wird in Phasen erstellt:</span><span class="sxs-lookup"><span data-stu-id="11144-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="11144-107">Eine Liste der Bücher wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="11144-107">Showing a list of books.</span></span>
- <span data-ttu-id="11144-108">Anzeigen eines Buch Details.</span><span class="sxs-lookup"><span data-stu-id="11144-108">Showing a book detail.</span></span>
- <span data-ttu-id="11144-109">Hinzufügen eines neuen Buchs.</span><span class="sxs-lookup"><span data-stu-id="11144-109">Adding a new book.</span></span>

<span data-ttu-id="11144-110">Die Knockout-Bibliothek verwendet das Model-View-ViewModel (MVVM)-Muster:</span><span class="sxs-lookup"><span data-stu-id="11144-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="11144-111">Das **Modell** ist die serverseitige Darstellung der Daten in der Geschäftsdomäne (in unserem Fall Bücher und Autoren).</span><span class="sxs-lookup"><span data-stu-id="11144-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="11144-112">Die **Ansicht** ist die Darstellungs Schicht (HTML).</span><span class="sxs-lookup"><span data-stu-id="11144-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="11144-113">Das **Ansichts Modell** ist ein JavaScript-Objekt, das die Modelle enthält.</span><span class="sxs-lookup"><span data-stu-id="11144-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="11144-114">Das Ansichts Modell ist eine Code Abstraktion der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="11144-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="11144-115">Die HTML-Darstellung ist nicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="11144-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="11144-116">Stattdessen stellt Sie abstrakte Features der Sicht dar, z. b. &quot;eine Liste der Bücher&quot;.</span><span class="sxs-lookup"><span data-stu-id="11144-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="11144-117">Die Sicht ist Daten gebunden an das Ansichts Modell.</span><span class="sxs-lookup"><span data-stu-id="11144-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="11144-118">Updates für das Ansichts Modell werden automatisch in der Ansicht widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="11144-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="11144-119">Das Ansichts Modell ruft auch Ereignisse aus der Ansicht ab, z. b. Schaltflächen Klicks.</span><span class="sxs-lookup"><span data-stu-id="11144-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="11144-120">Mit diesem Ansatz können Sie das Layout und die Benutzeroberfläche Ihrer APP leicht ändern, da Sie die Bindungen ändern können, ohne Code neu schreiben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="11144-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="11144-121">Beispielsweise können Sie eine Liste von Elementen als `<ul>`anzeigen und Sie später in eine Tabelle ändern.</span><span class="sxs-lookup"><span data-stu-id="11144-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="11144-122">Hinzufügen der Knockout-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="11144-122">Add the Knockout Library</span></span>

<span data-ttu-id="11144-123">Wählen Sie in Visual Studio im **Menü Extras** den Befehl **nuget-Paket-Manager**aus.</span><span class="sxs-lookup"><span data-stu-id="11144-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="11144-124">Klicken Sie anschließend auf **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="11144-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="11144-125">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="11144-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="11144-126">Mit diesem Befehl werden die Knockout-Dateien dem Ordner "Scripts" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="11144-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="11144-127">Erstellen des Ansichts Modells</span><span class="sxs-lookup"><span data-stu-id="11144-127">Create the View Model</span></span>

<span data-ttu-id="11144-128">Fügen Sie dem Ordner Scripts eine JavaScript-Datei mit dem Namen app. js hinzu.</span><span class="sxs-lookup"><span data-stu-id="11144-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="11144-129">(Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Skripts, wählen Sie **Hinzufügen**und dann **JavaScript-Datei**aus.) Fügen Sie den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="11144-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="11144-130">In Knockout ermöglicht die `observable`-Klasse die Datenbindung.</span><span class="sxs-lookup"><span data-stu-id="11144-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="11144-131">Wenn sich der Inhalt einer wahrnehmbaren Änderung ändert, benachrichtigt das Observable-Steuerelement alle Daten gebundenen Steuerelemente, sodass Sie sich selbst aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="11144-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="11144-132">(Die `observableArray` Klasse ist die Array Version von *Observable*.) Um mit zu beginnen, verfügt unser Ansichts Modell über zwei Observables:</span><span class="sxs-lookup"><span data-stu-id="11144-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="11144-133">`books` enthält die Liste der Bücher.</span><span class="sxs-lookup"><span data-stu-id="11144-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="11144-134">`error` enthält eine Fehlermeldung, wenn ein AJAX-Befehl fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="11144-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="11144-135">Die `getAllBooks`-Methode führt einen AJAX-Befehl aus, um die Liste der Bücher abzurufen.</span><span class="sxs-lookup"><span data-stu-id="11144-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="11144-136">Anschließend wird das Ergebnis auf das `books` Array gepusht.</span><span class="sxs-lookup"><span data-stu-id="11144-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="11144-137">Die `ko.applyBindings`-Methode ist Teil der Knockout-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="11144-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="11144-138">Er übernimmt das Ansichts Modell als Parameter und richtet die Datenbindung ein.</span><span class="sxs-lookup"><span data-stu-id="11144-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="11144-139">Skript Bündel hinzufügen</span><span class="sxs-lookup"><span data-stu-id="11144-139">Add a Script Bundle</span></span>

<span data-ttu-id="11144-140">Bündelung ist ein Feature in ASP.NET 4,5, das das kombinieren oder bündeln mehrerer Dateien in einer einzigen Datei vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="11144-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="11144-141">Die Bündelung reduziert die Anzahl der Anforderungen an den Server, wodurch die Seitenladezeit verbessert werden kann.</span><span class="sxs-lookup"><span data-stu-id="11144-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="11144-142">Öffnen Sie die Datei-App\_Start/bundleconfig. cs.</span><span class="sxs-lookup"><span data-stu-id="11144-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="11144-143">Fügen Sie der registerbundles-Methode den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="11144-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="11144-144">[Zurück](part-5.md)
> [Weiter](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="11144-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
