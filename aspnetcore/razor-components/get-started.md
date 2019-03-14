---
title: Erste Schritte mit Razor-Komponenten
author: guardrex
description: Erfahren Sie, wie Sie mit Razor-Komponenten beginnen, durch das Erstellen und ändern ein Projekt für die Razor-Komponenten.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036387"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="9292d-103">Erste Schritte mit Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="9292d-103">Get started with Razor Components</span></span>

<span data-ttu-id="9292d-104">Von [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9292d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9292d-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9292d-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9292d-106">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="9292d-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="9292d-107">So erstellen Ihr erste Projekt mit Razor-Komponenten in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9292d-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="9292d-108">Wählen Sie **Datei** > **neues Projekt** > **Web** > **ASP.NET Core-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="9292d-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="9292d-109">Stellen Sie sicher, dass **.NET Core** und **ASP.NET Core 3.0** oben ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="9292d-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="9292d-110">Wählen Sie die **Razor-Komponenten** Vorlage, und wählen **OK**.</span><span class="sxs-lookup"><span data-stu-id="9292d-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Dialogfeld "neue app"](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="9292d-112">Drücken Sie **F5**, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9292d-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="9292d-113">Herzlichen Glückwunsch!</span><span class="sxs-lookup"><span data-stu-id="9292d-113">Congratulations!</span></span> <span data-ttu-id="9292d-114">Sie haben soeben Ihre erste app mit Razor-Komponenten ausgeführt!</span><span class="sxs-lookup"><span data-stu-id="9292d-114">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9292d-115">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="9292d-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="9292d-116">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="9292d-116">Prerequisites:</span></span>

* [<span data-ttu-id="9292d-117">.NET Core SDK 3.0 (Vorschau)</span><span class="sxs-lookup"><span data-stu-id="9292d-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="9292d-118">So erstellen Sie Ihr erste Projekt mit Razor-Komponenten über eine Befehlsshell:</span><span class="sxs-lookup"><span data-stu-id="9292d-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="9292d-119">Navigieren Sie in einem Browser zu `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9292d-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="9292d-120">Herzlichen Glückwunsch!</span><span class="sxs-lookup"><span data-stu-id="9292d-120">Congratulations!</span></span> <span data-ttu-id="9292d-121">Sie haben soeben Ihre erste app mit Razor-Komponenten ausgeführt!</span><span class="sxs-lookup"><span data-stu-id="9292d-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="9292d-122">Razor-Komponenten-Projekt</span><span class="sxs-lookup"><span data-stu-id="9292d-122">Razor Components project</span></span>

<span data-ttu-id="9292d-123">Die von der Komponenten der Razor-Vorlage erstellte Projektmappe enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="9292d-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="9292d-124">*WebApplication1.Server* &ndash; das Server-Projekt ist ein ASP.NET Core-Projekt, das zum Hosten der app für Razor-Komponenten einrichten.</span><span class="sxs-lookup"><span data-stu-id="9292d-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="9292d-125">*WebApplication1.App* &ndash; das Client-Side-Web-UI-Projekt, die Razor-Komponenten verwendet.</span><span class="sxs-lookup"><span data-stu-id="9292d-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="9292d-126">Die UI-Logik in die *WebApplication1.App* Projekt wird getrennt vom Rest der app durch eine technische Einschränkung in ASP.NET Core 3.0 Preview 2.</span><span class="sxs-lookup"><span data-stu-id="9292d-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="9292d-127">Die Razor-Dateierweiterung (*.cshtml*) verwendet für Razor-Komponenten auch für Razor-Seiten und MVC-Ansichten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9292d-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="9292d-128">Derzeit verfügen über Razor-Komponenten und Razor-Seiten/MVC-Modelle für verschiedene Kompilierungsoptionen, damit die Razor-Komponenten-Razor-Dateien getrennt verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="9292d-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="9292d-129">In einer kommenden Vorschau verwenden, möchten wir eine neue Dateierweiterung für Razor-Komponenten einführen (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="9292d-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="9292d-130">Komponenten, Seiten und Ansichten gehostet werden *im selben Projekt*.</span><span class="sxs-lookup"><span data-stu-id="9292d-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="9292d-131">Wenn die app ausgeführt wird, stehen mehrere Seiten von Registerkarten in der Randleiste aus:</span><span class="sxs-lookup"><span data-stu-id="9292d-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="9292d-132">Startseite</span><span class="sxs-lookup"><span data-stu-id="9292d-132">Home</span></span>
* <span data-ttu-id="9292d-133">Zähler</span><span class="sxs-lookup"><span data-stu-id="9292d-133">Counter</span></span>
* <span data-ttu-id="9292d-134">Abrufen der Daten</span><span class="sxs-lookup"><span data-stu-id="9292d-134">Fetch data</span></span>

<span data-ttu-id="9292d-135">Wählen Sie auf der Seite „Counter“ die Schaltfläche **Hier klicken** aus, um den Zähler ohne Seitenaktualisierung heraufzusetzen.</span><span class="sxs-lookup"><span data-stu-id="9292d-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="9292d-136">Das Heraufsetzen eines Zählers auf einer Webseite erfordert normalerweise das Schreiben in JavaScript, aber Razor Components bietet einen besseren Ansatz mit C#.</span><span class="sxs-lookup"><span data-stu-id="9292d-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="9292d-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9292d-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="9292d-138">Eine Anforderung für `/counter` im Browser, laut der `@page` -Direktive am Anfang, bewirkt, dass die Leistungsindikator-Komponente, um seinen Inhalt zu rendern.</span><span class="sxs-lookup"><span data-stu-id="9292d-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="9292d-139">Rendern von Komponenten in eine in-Memory-Darstellung der Render-Struktur, die zur Aktualisierung der Benutzeroberfläche in eine flexible und effiziente Möglichkeit verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9292d-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="9292d-140">Jedes Mal die **hier klicken** ausgewählt ist:</span><span class="sxs-lookup"><span data-stu-id="9292d-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="9292d-141">Die `onclick` Ereignis wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="9292d-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="9292d-142">Die `IncrementCount` -Methode wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="9292d-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="9292d-143">Die `currentCount` wird erhöht.</span><span class="sxs-lookup"><span data-stu-id="9292d-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="9292d-144">Die Komponente wird erneut gerendert.</span><span class="sxs-lookup"><span data-stu-id="9292d-144">The component is rendered again.</span></span>

<span data-ttu-id="9292d-145">Die Runtime vergleicht den neuen Inhalt zum vorherigen Inhalt und den geänderten Inhalt gilt nur, die (DOKUMENTOBJEKTMODELL).</span><span class="sxs-lookup"><span data-stu-id="9292d-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="9292d-146">Fügen Sie eine Komponente an eine andere Komponente, die mithilfe einer HTML-ähnliche Syntax.</span><span class="sxs-lookup"><span data-stu-id="9292d-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="9292d-147">Parameter der Komponente werden mithilfe von Attributen oder untergeordneten Inhalt angegeben.</span><span class="sxs-lookup"><span data-stu-id="9292d-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="9292d-148">Eine Komponente eines Leistungsindikators kann z. B. an der app-Startseite hinzugefügt werden, durch das Hinzufügen einer `<Counter />` Element an die Index-Komponente.</span><span class="sxs-lookup"><span data-stu-id="9292d-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="9292d-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9292d-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="9292d-150">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="9292d-150">Run the app.</span></span> <span data-ttu-id="9292d-151">Die Startseite verfügt über eine eigene Leistungsindikator.</span><span class="sxs-lookup"><span data-stu-id="9292d-151">The homepage has its own counter.</span></span>

<span data-ttu-id="9292d-152">Um einen Parameter an die Leistungsindikator-Komponente hinzuzufügen, aktualisieren Sie der Komponente `@functions` blockieren:</span><span class="sxs-lookup"><span data-stu-id="9292d-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="9292d-153">Hinzufügen einer Eigenschaft für `IncrementAmount` ergänzt die `[Parameter]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="9292d-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="9292d-154">Ändern Sie die `IncrementCount`-Methode, um `IncrementAmount` beim Heraufsetzen des Werts von `currentCount` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9292d-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="9292d-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9292d-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="9292d-156">Geben Sie unter Verwendung eines Attributs einen `IncrementAmount`-Parameter in das `<Counter>`-Element der Home-Komponente ein.</span><span class="sxs-lookup"><span data-stu-id="9292d-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="9292d-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9292d-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="9292d-158">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="9292d-158">Run the app.</span></span> <span data-ttu-id="9292d-159">Die Startseite verfügt über eine eigene Zähler, der mit zehn jedes Mal erhöht die **hier klicken** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="9292d-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9292d-160">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="9292d-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
