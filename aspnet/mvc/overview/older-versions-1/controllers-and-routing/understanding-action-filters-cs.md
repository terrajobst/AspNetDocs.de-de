---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Grundlegendes zu AktionsC#Filtern () | Microsoft-Dokumentation
author: microsoft
description: Ziel dieses Tutorials ist es, Aktionsfilter zu erläutern. Ein Aktionsfilter ist ein Attribut, das Sie auf eine Controller Aktion anwenden können, oder auf einen vollständigen Controller...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: d1c72c2355c6122f851351a8c1e8f04fa63ae04e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470001"
---
# <a name="understanding-action-filters-c"></a>Grundlegendes zu Aktionsfiltern (C#)

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> Ziel dieses Tutorials ist es, Aktionsfilter zu erläutern. Ein Aktionsfilter ist ein Attribut, das Sie auf eine Controller Aktion anwenden können, oder ein ganzer Controller, der die Art und Weise ändert, in der die Aktion ausgeführt wird.

## <a name="understanding-action-filters"></a>Grundlegendes zu Aktions Filtern

Ziel dieses Tutorials ist es, Aktionsfilter zu erläutern. Ein Aktionsfilter ist ein Attribut, das Sie auf eine Controller Aktion anwenden können, oder ein ganzer Controller, der die Art und Weise ändert, in der die Aktion ausgeführt wird. Das ASP.NET-MVC-Framework umfasst mehrere Aktionsfilter:

- OutputCache – dieser Aktionsfilter speichert die Ausgabe einer Controller Aktion für einen angegebenen Zeitraum zwischen.
- Lenker Error – dieser Aktionsfilter behandelt Fehler, die ausgelöst werden, wenn eine Controller Aktion ausgeführt wird.
- Autorisieren – dieser Aktionsfilter ermöglicht Ihnen, den Zugriff auf einen bestimmten Benutzer oder eine Rolle einzuschränken.

Sie können auch eigene benutzerdefinierte Aktionsfilter erstellen. Beispielsweise können Sie einen benutzerdefinierten Aktionsfilter erstellen, um ein benutzerdefiniertes Authentifizierungssystem zu implementieren. Oder Sie möchten einen Aktionsfilter erstellen, der die von einer Controller Aktion zurückgegebenen Ansichts Daten ändert.

In diesem Tutorial erfahren Sie, wie Sie einen Aktionsfilter von Grund auf erstellen. Wir erstellen einen Protokoll Aktionsfilter, mit dem unterschiedliche Phasen der Verarbeitung einer Aktion im Visual Studio-Ausgabefenster protokolliert werden.

### <a name="using-an-action-filter"></a>Verwenden eines Aktions Filters

Ein Aktionsfilter ist ein Attribut. Sie können die meisten Aktionsfilter entweder auf eine einzelne Controller Aktion oder einen gesamten Controller anwenden.

Beispielsweise macht der Daten Controller in der Liste 1 eine Aktion mit dem Namen `Index()` verfügbar, die die aktuelle Zeit zurückgibt. Diese Aktion ist mit dem Filter für die `OutputCache` Aktion versehen. Dieser Filter bewirkt, dass der von der Aktion zurückgegebene Wert 10 Sekunden lang zwischengespeichert wird.

**Codebeispiel 1 – `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Wenn Sie die `Index()` Aktion wiederholt aufrufen, indem Sie die URL/Data/Index in die Adressleiste Ihres Browsers eingeben und mehrmals auf die Schaltfläche "Aktualisieren" klicken, wird die gleiche Zeit für 10 Sekunden angezeigt. Die Ausgabe der `Index()` Aktion wird 10 Sekunden lang zwischengespeichert (siehe Abbildung 1).

[![zwischengespeicherte Zeit](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Abbildung 01**: zwischengespeicherte Zeit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-action-filters-cs/_static/image3.png))

In der Liste 1 wird ein einzelner Aktionsfilter – der `OutputCache` Aktionsfilter – auf die `Index()`-Methode angewendet. Wenn Sie benötigen, können Sie mehrere Aktionsfilter auf dieselbe Aktion anwenden. Beispielsweise können Sie die `OutputCache`-und `HandleError` Aktionsfilter auf dieselbe Aktion anwenden.

In der Liste 1 wird der `OutputCache` Aktionsfilter auf die `Index()` Aktion angewendet. Sie können dieses Attribut auch auf die `DataController` Klasse selbst anwenden. In diesem Fall würde das Ergebnis, das von einer Aktion zurückgegeben wird, die vom Controller verfügbar gemacht wird, 10 Sekunden lang zwischengespeichert werden.

### <a name="the-different-types-of-filters"></a>Die verschiedenen Filtertypen

Das ASP.NET-MVC-Framework unterstützt vier verschiedene Arten von Filtern:

1. Autorisierungs Filter – implementiert das `IAuthorizationFilter`-Attribut.
2. Aktionsfilter – implementiert das `IActionFilter`-Attribut.
3. Ergebnis Filter – implementiert das `IResultFilter`-Attribut.
4. Ausnahme Filter – implementiert das `IExceptionFilter`-Attribut.

Filter werden in der oben aufgeführten Reihenfolge ausgeführt. Autorisierungs Filter werden z. b. immer ausgeführt, bevor Aktionsfilter und Ausnahme Filter immer nach jedem anderen Filtertyp ausgeführt werden.

Autorisierungs Filter werden verwendet, um die Authentifizierung und Autorisierung für Controller Aktionen zu implementieren. Der Autorisierungs Filter ist z. b. ein Beispiel für einen Autorisierungs Filter.

Aktionsfilter enthalten Logik, die vor und nach der Ausführung einer Controller Aktion ausgeführt wird. Sie können beispielsweise einen Aktionsfilter verwenden, um die Ansichts Daten zu ändern, die von einer Controller Aktion zurückgegeben werden.

Ergebnis Filter enthalten Logik, die vor und nach dem Ausführen eines Ansichts Ergebnisses ausgeführt wird. Beispielsweise möchten Sie möglicherweise ein Ansichts Ergebnis ändern, bevor die Ansicht im Browser gerendert wird.

Ausnahme Filter sind der letzte Typ des zu testenden Filters. Sie können einen Ausnahme Filter verwenden, um Fehler zu behandeln, die entweder von den Controller Aktionen oder von Controller Aktions Ergebnissen ausgelöst werden. Sie können auch Ausnahme Filter verwenden, um Fehler zu protokollieren.

Jeder andere Filtertyp wird in einer bestimmten Reihenfolge ausgeführt. Wenn Sie die Reihenfolge steuern möchten, in der Filter desselben Typs ausgeführt werden, können Sie die Order-Eigenschaft eines Filters festlegen.

Die Basisklasse für alle Aktionsfilter ist die `System.Web.Mvc.FilterAttribute`-Klasse. Wenn Sie einen bestimmten Filtertyp implementieren möchten, müssen Sie eine Klasse erstellen, die von der Basis Filterklasse erbt und eine oder mehrere der Schnittstellen `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`oder `IExceptionFilter` implementiert.

### <a name="the-base-actionfilterattribute-class"></a>Die Base Action Filter Attribute-Klasse

Um Ihnen die Implementierung eines benutzerdefinierten Aktions Filters zu erleichtern, enthält das ASP.NET-MVC-Framework eine Basis `ActionFilterAttribute`-Klasse. Diese Klasse implementiert die `IActionFilter`-und `IResultFilter` Schnittstellen und erbt von der `Filter`-Klasse.

Die Terminologie ist hier nicht vollständig konsistent. Technisch gesehen ist eine Klasse, die von der Action Filter Attribute-Klasse erbt, sowohl ein Aktionsfilter als auch ein Ergebnis Filter. Allerdings wird der Word-Aktionsfilter verwendet, um auf einen beliebigen Filtertyp im ASP.NET MVC-Framework zu verweisen.

Die Basis `ActionFilterAttribute` Klasse verfügt über die folgenden Methoden, die Sie überschreiben können:

- OnAction-Ausführung – diese Methode wird aufgerufen, bevor eine Controller Aktion ausgeführt wird.
- OnAction-– diese Methode wird aufgerufen, nachdem eine Controller Aktion ausgeführt wurde.
- OnResultExecuting – diese Methode wird aufgerufen, bevor ein Controller Aktions Ergebnis ausgeführt wird.
- OnResultExecuted – diese Methode wird aufgerufen, nachdem ein Controller Aktions Ergebnis ausgeführt wurde.

Im nächsten Abschnitt erfahren Sie, wie Sie jede dieser unterschiedlichen Methoden implementieren können.

### <a name="creating-a-log-action-filter"></a>Erstellen eines Protokoll Aktions Filters

Um zu veranschaulichen, wie Sie einen benutzerdefinierten Aktionsfilter erstellen können, erstellen wir einen benutzerdefinierten Aktionsfilter, mit dem die Phasen der Verarbeitung einer Controller Aktion im Visual Studio-Ausgabefenster protokolliert werden. Unsere `LogActionFilter` ist in der Liste 2 enthalten.

**Codebeispiel 2 – `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

In der Liste 2 wird mit den Methoden `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`und `OnResultExecuted()` die `Log()`-Methode aufgerufen. Der Name der Methode und die aktuellen Routendaten werden an die `Log()` Methode weitergegeben. Die `Log()`-Methode schreibt eine Meldung in das Visual Studio-Ausgabefenster (siehe Abbildung 2).

[![schreiben in das Visual Studio-Ausgabefenster](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Abbildung 02**: Schreiben in das Visual Studio-Ausgabefenster ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-action-filters-cs/_static/image6.png))

Der Home-Controller in der Liste 3 veranschaulicht, wie Sie den Protokoll Aktionsfilter auf eine gesamte Controller Klasse anwenden können. Wenn eine der vom Home-Controller verfügbar gemachten Aktionen aufgerufen wird – entweder die `Index()`-Methode oder die `About()`-Methode – werden die Phasen der Verarbeitung der Aktion im Ausgabefenster von Visual Studio protokolliert.

**Codebeispiel 3 – `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie ASP.NET MVC Action Filters eingeführt. Sie haben die vier verschiedenen Arten von Filtern kennengelernt: Autorisierungs Filter, Aktionsfilter, Ergebnis Filter und Ausnahme Filter. Außerdem haben Sie mehr über die Basis `ActionFilterAttribute` Klasse erfahren.

Schließlich haben Sie erfahren, wie Sie einen einfachen Aktionsfilter implementieren. Wir haben einen Protokoll Aktionsfilter erstellt, der die Phasen der Verarbeitung einer Controller Aktion im Visual Studio-Ausgabefenster protokolliert.

> [!div class="step-by-step"]
> [Zurück](asp-net-mvc-routing-overview-cs.md)
> [Weiter](improving-performance-with-output-caching-cs.md)
