---
title: Layouts für Razor-Komponenten
author: guardrex
description: Erfahren Sie, wie wiederverwendbares Layout-Komponenten für Blazor und Razor-Komponenten-apps zu erstellen.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039037"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="430ca-103">Layouts für Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="430ca-103">Razor Components layouts</span></span>

<span data-ttu-id="430ca-104">Durch [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="430ca-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="430ca-105">Apps enthalten in der Regel mehr als eine Seite.</span><span class="sxs-lookup"><span data-stu-id="430ca-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="430ca-106">Layoutelemente, z. B. Menüs, copyright Nachrichten und Logos, müssen auf allen Seiten vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="430ca-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="430ca-107">Kopieren den Code für diese Layoutelemente in allen Seiten einer App ist eine effiziente Lösung nicht.</span><span class="sxs-lookup"><span data-stu-id="430ca-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="430ca-108">Solche Duplizierung ist schwer zu verwalten und wahrscheinlich führt zu einer inkonsistenten Inhalt im Laufe der Zeit.</span><span class="sxs-lookup"><span data-stu-id="430ca-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="430ca-109">*Layouts* dieses Problem zu lösen.</span><span class="sxs-lookup"><span data-stu-id="430ca-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="430ca-110">Technisch gesehen ist ein Layout nur eine andere Komponente.</span><span class="sxs-lookup"><span data-stu-id="430ca-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="430ca-111">Ein Layout wird definiert, in einer Razor-Vorlage oder in C# code und die Datenbindung, Abhängigkeitsinjektion und andere gewöhnlichen Funktionen der Komponenten enthalten.</span><span class="sxs-lookup"><span data-stu-id="430ca-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="430ca-112">Aktivieren Sie zwei zusätzliche Aspekte einer *Komponente* in einer *Layout*:</span><span class="sxs-lookup"><span data-stu-id="430ca-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="430ca-113">Die Layoutkomponente muss vom erben `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="430ca-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="430ca-114">`BlazorLayoutComponent` definiert eine `Body` -Eigenschaft, die den Inhalt enthält, in das Layout gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="430ca-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="430ca-115">Die Layoutkomponente verwendet die `Body` Eigenschaft, um anzugeben, wo der Inhalt gerendert wird, mit der Razor-Syntax `@Body`.</span><span class="sxs-lookup"><span data-stu-id="430ca-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="430ca-116">Während des Renderns `@Body` durch den Inhalt des Layouts ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="430ca-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="430ca-117">Das folgende Codebeispiel zeigt die Razor-Vorlage für eine Layoutkomponente.</span><span class="sxs-lookup"><span data-stu-id="430ca-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="430ca-118">Beachten Sie die Verwendung von `BlazorLayoutComponent` und `@Body`:</span><span class="sxs-lookup"><span data-stu-id="430ca-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="430ca-119">Verwenden Sie ein Layout in einer Komponente</span><span class="sxs-lookup"><span data-stu-id="430ca-119">Use a layout in a component</span></span>

<span data-ttu-id="430ca-120">Verwenden Sie die Razor-Direktive `@layout` ein Layout auf eine Komponente anwenden.</span><span class="sxs-lookup"><span data-stu-id="430ca-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="430ca-121">Der Compiler konvertiert diese Richtlinie in eine `LayoutAttribute`, das auf die Component-Klasse angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="430ca-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="430ca-122">Im folgenden Codebeispiel wird das Konzept veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="430ca-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="430ca-123">Der Inhalt dieser Komponente eingefügt, in der *MasterLayout* an der Position des `@Body`:</span><span class="sxs-lookup"><span data-stu-id="430ca-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="430ca-124">Zentralisierte Layoutauswahl.</span><span class="sxs-lookup"><span data-stu-id="430ca-124">Centralized layout selection</span></span>

<span data-ttu-id="430ca-125">Jeder Ordner, der eine app kann optional eine Vorlagendatei, die mit dem Namen enthalten *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="430ca-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="430ca-126">Der Compiler schließt die Anweisungen in der Ansicht Imports-Datei in der Razor-Vorlagen im selben Ordner und rekursiv in allen Unterordnern angegeben.</span><span class="sxs-lookup"><span data-stu-id="430ca-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="430ca-127">Aus diesem Grund eine *_ViewImports.cshtml* Datei mit `@layout MainLayout` wird sichergestellt, dass alle Komponenten in einem Ordner die *MainLayout* Layout.</span><span class="sxs-lookup"><span data-stu-id="430ca-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="430ca-128">Besteht keine Notwendigkeit wiederholt hinzufügen `@layout` aller der  *\*.cshtml* Dateien.</span><span class="sxs-lookup"><span data-stu-id="430ca-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="430ca-129">Beachten Sie, die die Standardvorlage verwendet die *_ViewImports.cshtml* Mechanismus zur Layoutauswahl.</span><span class="sxs-lookup"><span data-stu-id="430ca-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="430ca-130">Eine neu erstellte app enthält die *_ViewImports.cshtml* Datei die *Seiten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="430ca-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="430ca-131">Geschachtelte layouts</span><span class="sxs-lookup"><span data-stu-id="430ca-131">Nested layouts</span></span>

<span data-ttu-id="430ca-132">Apps können auf geschachtelte Layouts bestehen.</span><span class="sxs-lookup"><span data-stu-id="430ca-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="430ca-133">Eine Komponente kann es sich um ein Layout verweisen, das wiederum ein anderes Layout verweist.</span><span class="sxs-lookup"><span data-stu-id="430ca-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="430ca-134">Beispielsweise können schachteln Layouts verwendet werden, mit mehreren Ebenen mit eine Menüstruktur entsprechend.</span><span class="sxs-lookup"><span data-stu-id="430ca-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="430ca-135">Die folgenden Codebeispiele zeigen, wie geschachtelte Layouts verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="430ca-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="430ca-136">Die *CustomersComponent.cshtml* Datei ist die Komponente angezeigt.</span><span class="sxs-lookup"><span data-stu-id="430ca-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="430ca-137">Beachten Sie, dass die Komponente verweist, das Layout auf `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="430ca-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="430ca-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="430ca-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="430ca-139">Die *MasterDataLayout.cshtml* Datei enthält die `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="430ca-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="430ca-140">Das Layout verweist auf ein anderes Layout `MainLayout`, wohin diese fließen, die eingebettet werden.</span><span class="sxs-lookup"><span data-stu-id="430ca-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="430ca-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="430ca-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="430ca-142">Zum Schluss `MainLayout` enthält die Layoutelemente der obersten Ebene, z. B. die Header, Footer und im Hauptmenü.</span><span class="sxs-lookup"><span data-stu-id="430ca-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="430ca-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="430ca-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
