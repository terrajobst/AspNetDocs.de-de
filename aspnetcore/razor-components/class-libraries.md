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
# <a name="razor-components-class-libraries"></a>Klassenbibliotheken für Razor-Komponenten

Durch [Simon Timms](https://github.com/stimms)

> [!NOTE]
> Das .NET Core 3.0 Preview 2-SDK keine Projektvorlage für Klassenbibliotheken für Razor-Komponente enthalten. jedoch wird voraussichtlich eine Vorlage in einer kommenden Vorschau hinzugefügt. In der Zwischenzeit können Sie die Vorlage Blazor Komponente-Klassenbibliothek, die in diesem Thema erläutert.

Komponenten können in Komponentenbibliotheken projektübergreifend freigegeben werden. Komponenten können aus berücksichtigt werden:

* Ein anderes Projekt in der Projektmappe.
* Ein NuGet-Paket.
* Eine Bibliothek mit .NET auf die verwiesen wird.

Genau, wie Komponenten reguläre .NET Typen sind, sind Komponentenbibliotheken normale .NET Assemblys.

Verwenden Sie zum Erstellen einer neuen Komponentenbibliothek die `blazorlib` Vorlage mit der [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl. Die Vorlage ist Teil der installierten Vorlagen beim [Razor-Komponenten einrichten](xref:razor-components/get-started).

```console
dotnet new blazorlib -o MyComponentLib1
```

Um die Bibliothek zu einem vorhandenen Projekt hinzuzufügen, verwenden die [Dotnet-Sln](/dotnet/core/tools/dotnet-sln) Befehl:

```console
dotnet sln add .\MyComponentLib1
```

Komponentenbibliotheken können es sich um statische Dateien, z. B. Bilder, JavaScript und Stylesheets enthalten. Zur Buildzeit werden statische Dateien in die erstellte Assembly-Datei eingebettet (*DLL*), der Verbrauch der Komponenten ermöglicht, ohne dazu, wie Sie ihre Ressourcen enthalten. Alle Dateien in die `content` Verzeichnis als eingebettete Ressource gekennzeichnet sind. 

## <a name="consume-a-library-component"></a>Nutzen Sie eine Bibliothekskomponente

Um in einer Bibliothek in einem anderen Projekt definierten Komponenten nutzen den [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) Richtlinie muss verwendet werden. Einzelne Komponenten können anhand des Namens hinzugefügt werden. Die folgende Anweisung fügt beispielsweise `Component1` von `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Das allgemeine Format für die Direktive ist:

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

Allerdings ist es üblich, alle Komponenten aus einer Assembly mit einem Platzhalter enthalten:

```cshtml
@addTagHelper *, MyComponentLib1
```

Die `@addTagHelper` Richtlinie enthalten sein kann, im *_ViewImport.cshtml* die Komponenten verfügbar, die für ein gesamtes Projekt oder angewendet auf eine einzelne Seite oder eine Gruppe von Seiten in einem Ordner vornehmen. Mit der `@addTagHelper` Direktive vorhanden, die Bestandteile der Komponentenbibliothek genutzt werden können als wären sie in der gleichen Assembly wie die app. 

## <a name="build-pack-and-ship-to-nuget"></a>Build, Pack und Versand zu NuGet

Da Komponentenbibliotheken .NET Standardbibliotheken sind, unterscheidet Verpackung und Lieferung an den NuGet sich nicht von der Verpackung und Versand einer Bibliothek auf NuGet. Paketerstellung erfolgt unter Verwendung der [Dotnet-Pack](/dotnet/core/tools/dotnet-pack) Befehl:

```console
dotnet pack
```

Hochladen des Pakets mit NuGet die [Dotnet Nuget veröffentlichen](/dotnet/core/tools/dotnet-nuget-push) Befehl:

```console
dotnet nuget publish
```

Statische Ressourcen enthalten, werden im NuGet-Paket enthalten. Consumer erhalten automatisch Skripts und Stylesheets, damit der Consumer nicht erforderlich, um die Ressourcen manuell zu installieren sind.
