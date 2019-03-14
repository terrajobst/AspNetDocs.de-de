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
# <a name="razor-components-layouts"></a>Layouts für Razor-Komponenten

Durch [Rainer Stropek](https://www.timecockpit.com)

Apps enthalten in der Regel mehr als eine Seite. Layoutelemente, z. B. Menüs, copyright Nachrichten und Logos, müssen auf allen Seiten vorhanden sein. Kopieren den Code für diese Layoutelemente in allen Seiten einer App ist eine effiziente Lösung nicht. Solche Duplizierung ist schwer zu verwalten und wahrscheinlich führt zu einer inkonsistenten Inhalt im Laufe der Zeit. *Layouts* dieses Problem zu lösen.

Technisch gesehen ist ein Layout nur eine andere Komponente. Ein Layout wird definiert, in einer Razor-Vorlage oder in C# code und die Datenbindung, Abhängigkeitsinjektion und andere gewöhnlichen Funktionen der Komponenten enthalten. Aktivieren Sie zwei zusätzliche Aspekte einer *Komponente* in einer *Layout*:

* Die Layoutkomponente muss vom erben `BlazorLayoutComponent`. `BlazorLayoutComponent` definiert eine `Body` -Eigenschaft, die den Inhalt enthält, in das Layout gerendert werden.
* Die Layoutkomponente verwendet die `Body` Eigenschaft, um anzugeben, wo der Inhalt gerendert wird, mit der Razor-Syntax `@Body`. Während des Renderns `@Body` durch den Inhalt des Layouts ersetzt wird.

Das folgende Codebeispiel zeigt die Razor-Vorlage für eine Layoutkomponente. Beachten Sie die Verwendung von `BlazorLayoutComponent` und `@Body`:

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

## <a name="use-a-layout-in-a-component"></a>Verwenden Sie ein Layout in einer Komponente

Verwenden Sie die Razor-Direktive `@layout` ein Layout auf eine Komponente anwenden. Der Compiler konvertiert diese Richtlinie in eine `LayoutAttribute`, das auf die Component-Klasse angewendet wird.

Im folgenden Codebeispiel wird das Konzept veranschaulicht. Der Inhalt dieser Komponente eingefügt, in der *MasterLayout* an der Position des `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Zentralisierte Layoutauswahl.

Jeder Ordner, der eine app kann optional eine Vorlagendatei, die mit dem Namen enthalten *_ViewImports.cshtml*. Der Compiler schließt die Anweisungen in der Ansicht Imports-Datei in der Razor-Vorlagen im selben Ordner und rekursiv in allen Unterordnern angegeben. Aus diesem Grund eine *_ViewImports.cshtml* Datei mit `@layout MainLayout` wird sichergestellt, dass alle Komponenten in einem Ordner die *MainLayout* Layout. Besteht keine Notwendigkeit wiederholt hinzufügen `@layout` aller der  *\*.cshtml* Dateien.

Beachten Sie, die die Standardvorlage verwendet die *_ViewImports.cshtml* Mechanismus zur Layoutauswahl. Eine neu erstellte app enthält die *_ViewImports.cshtml* Datei die *Seiten* Ordner.

## <a name="nested-layouts"></a>Geschachtelte layouts

Apps können auf geschachtelte Layouts bestehen. Eine Komponente kann es sich um ein Layout verweisen, das wiederum ein anderes Layout verweist. Beispielsweise können schachteln Layouts verwendet werden, mit mehreren Ebenen mit eine Menüstruktur entsprechend.

Die folgenden Codebeispiele zeigen, wie geschachtelte Layouts verwendet wird. Die *CustomersComponent.cshtml* Datei ist die Komponente angezeigt. Beachten Sie, dass die Komponente verweist, das Layout auf `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

Die *MasterDataLayout.cshtml* Datei enthält die `MasterDataLayout`. Das Layout verweist auf ein anderes Layout `MainLayout`, wohin diese fließen, die eingebettet werden.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Zum Schluss `MainLayout` enthält die Layoutelemente der obersten Ebene, z. B. die Header, Footer und im Hauptmenü.

*MainLayout.cshtml*:

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
