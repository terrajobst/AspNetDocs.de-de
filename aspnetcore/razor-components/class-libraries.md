---
title: Klassenbibliotheken für Razor-Komponenten
author: guardrex
description: Ermitteln Sie, wie die Komponenten in Razor-Komponenten-apps aus einer externen Komponentenbibliothek aufgenommen werden können.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037407"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="c0934-103">Klassenbibliotheken für Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="c0934-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="c0934-104">Durch [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="c0934-104">By [Simon Timms](https://github.com/stimms)</span></span>

> [!NOTE]
> <span data-ttu-id="c0934-105">Das .NET Core 3.0 Preview 2-SDK keine Projektvorlage für Klassenbibliotheken für Razor-Komponente enthalten. jedoch wird voraussichtlich eine Vorlage in einer kommenden Vorschau hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c0934-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="c0934-106">In der Zwischenzeit können Sie die Vorlage Blazor Komponente-Klassenbibliothek, die in diesem Thema erläutert.</span><span class="sxs-lookup"><span data-stu-id="c0934-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="c0934-107">Komponenten können in Komponentenbibliotheken projektübergreifend freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c0934-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="c0934-108">Komponenten können aus berücksichtigt werden:</span><span class="sxs-lookup"><span data-stu-id="c0934-108">Components can be included from:</span></span>

* <span data-ttu-id="c0934-109">Ein anderes Projekt in der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="c0934-109">Another project in the solution.</span></span>
* <span data-ttu-id="c0934-110">Ein NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="c0934-110">A NuGet package.</span></span>
* <span data-ttu-id="c0934-111">Eine Bibliothek mit .NET auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="c0934-111">A referenced .NET library.</span></span>

<span data-ttu-id="c0934-112">Genau, wie Komponenten reguläre .NET Typen sind, sind Komponentenbibliotheken normale .NET Assemblys.</span><span class="sxs-lookup"><span data-stu-id="c0934-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="c0934-113">Verwenden Sie zum Erstellen einer neuen Komponentenbibliothek die `blazorlib` Vorlage mit der [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl.</span><span class="sxs-lookup"><span data-stu-id="c0934-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="c0934-114">Die Vorlage ist Teil der installierten Vorlagen beim [Razor-Komponenten einrichten](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="c0934-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="c0934-115">Um die Bibliothek zu einem vorhandenen Projekt hinzuzufügen, verwenden die [Dotnet-Sln](/dotnet/core/tools/dotnet-sln) Befehl:</span><span class="sxs-lookup"><span data-stu-id="c0934-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="c0934-116">Komponentenbibliotheken können es sich um statische Dateien, z. B. Bilder, JavaScript und Stylesheets enthalten.</span><span class="sxs-lookup"><span data-stu-id="c0934-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="c0934-117">Zur Buildzeit werden statische Dateien in die erstellte Assembly-Datei eingebettet (*DLL*), der Verbrauch der Komponenten ermöglicht, ohne dazu, wie Sie ihre Ressourcen enthalten.</span><span class="sxs-lookup"><span data-stu-id="c0934-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="c0934-118">Alle Dateien in die `content` Verzeichnis als eingebettete Ressource gekennzeichnet sind.</span><span class="sxs-lookup"><span data-stu-id="c0934-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="c0934-119">Nutzen Sie eine Bibliothekskomponente</span><span class="sxs-lookup"><span data-stu-id="c0934-119">Consume a library component</span></span>

<span data-ttu-id="c0934-120">Um in einer Bibliothek in einem anderen Projekt definierten Komponenten nutzen den [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) Richtlinie muss verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c0934-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="c0934-121">Einzelne Komponenten können anhand des Namens hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="c0934-121">Individual components may be added by name.</span></span> <span data-ttu-id="c0934-122">Die folgende Anweisung fügt beispielsweise `Component1` von `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="c0934-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="c0934-123">Das allgemeine Format für die Direktive ist:</span><span class="sxs-lookup"><span data-stu-id="c0934-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="c0934-124">Allerdings ist es üblich, alle Komponenten aus einer Assembly mit einem Platzhalter enthalten:</span><span class="sxs-lookup"><span data-stu-id="c0934-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="c0934-125">Die `@addTagHelper` Richtlinie enthalten sein kann, im *_ViewImport.cshtml* die Komponenten verfügbar, die für ein gesamtes Projekt oder angewendet auf eine einzelne Seite oder eine Gruppe von Seiten in einem Ordner vornehmen.</span><span class="sxs-lookup"><span data-stu-id="c0934-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="c0934-126">Mit der `@addTagHelper` Direktive vorhanden, die Bestandteile der Komponentenbibliothek genutzt werden können als wären sie in der gleichen Assembly wie die app.</span><span class="sxs-lookup"><span data-stu-id="c0934-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="c0934-127">Build, Pack und Versand zu NuGet</span><span class="sxs-lookup"><span data-stu-id="c0934-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="c0934-128">Da Komponentenbibliotheken .NET Standardbibliotheken sind, unterscheidet Verpackung und Lieferung an den NuGet sich nicht von der Verpackung und Versand einer Bibliothek auf NuGet.</span><span class="sxs-lookup"><span data-stu-id="c0934-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="c0934-129">Paketerstellung erfolgt unter Verwendung der [Dotnet-Pack](/dotnet/core/tools/dotnet-pack) Befehl:</span><span class="sxs-lookup"><span data-stu-id="c0934-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="c0934-130">Hochladen des Pakets mit NuGet die [Dotnet Nuget veröffentlichen](/dotnet/core/tools/dotnet-nuget-push) Befehl:</span><span class="sxs-lookup"><span data-stu-id="c0934-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="c0934-131">Statische Ressourcen enthalten, werden im NuGet-Paket enthalten.</span><span class="sxs-lookup"><span data-stu-id="c0934-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="c0934-132">Consumer erhalten automatisch Skripts und Stylesheets, damit der Consumer nicht erforderlich, um die Ressourcen manuell zu installieren sind.</span><span class="sxs-lookup"><span data-stu-id="c0934-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
