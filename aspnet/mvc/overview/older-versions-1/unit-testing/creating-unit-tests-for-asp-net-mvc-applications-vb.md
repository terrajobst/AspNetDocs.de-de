---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Erstellen von Komponenten Tests für ASP.NET-MVC-Anwendungen (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie Sie Komponententests für Controller Aktionen erstellen. In diesem Tutorial demonstriert Stephen Walther, wie Sie testen können, ob eine Controller Aktion eine Gruppe zurückgibt...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 643faddaf6f9cd075131e8e5a9cccb303e355ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435381"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Erstellen von Komponententests für ASP.NET MVC-Anwendungen (VB)

von [Stephen Walther](https://github.com/StephenWalther)

[PDF herunterladen](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Erfahren Sie, wie Sie Komponententests für Controller Aktionen erstellen. In diesem Tutorial demonstriert Stephen Walther, wie Sie testen können, ob eine Controller Aktion eine bestimmte Ansicht zurückgibt, einen bestimmten Satz von Daten zurückgibt oder einen anderen Typ von Aktions Ergebnis zurückgibt.

In diesem Tutorial wird veranschaulicht, wie Sie Komponententests für die Controller in Ihren ASP.NET-MVC-Anwendungen schreiben können. Wir besprechen, wie drei verschiedene Arten von Komponententests erstellt werden. Sie erfahren, wie Sie die von einer Controller Aktion zurückgegebene Sicht testen, die von einer Controller Aktion zurückgegebenen Ansichts Daten testen und überprüfen, ob eine Controller Aktion Sie zu einer zweiten Controller Aktion umleitet.

## <a name="creating-the-controller-under-test"></a>Erstellen des zu testenden Controllers

Zunächst erstellen wir den Controller, den wir testen möchten. Der Controller namens der `ProductController`ist in der Liste 1 enthalten.

**Codebeispiel 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

Der `ProductController` enthält zwei Aktionsmethoden mit dem Namen `Index()` und `Details()`. Beide Aktionsmethoden geben eine Ansicht zurück. Beachten Sie, dass die `Details()` Aktion einen Parameter mit dem Namen ID akzeptiert.

## <a name="testing-the-view-returned-by-a-controller"></a>Testen der von einem Controller zurückgegebenen Ansicht

Stellen Sie sich vor, dass wir testen möchten, ob die `ProductController` die richtige Ansicht zurückgibt. Wir möchten sicherstellen, dass die Detailansicht zurückgegeben wird, wenn die `ProductController.Details()` Aktion aufgerufen wird. Die Testklasse in der Liste 2 enthält einen Komponenten Test zum Testen der Ansicht, die von der `ProductController.Details()` Aktion zurückgegeben wird.

**Codebeispiel 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

Die Klasse in der Liste 2 enthält eine Testmethode mit dem Namen `TestDetailsView()`. Diese Methode enthält drei Codezeilen. Die erste Codezeile erstellt eine neue Instanz der `ProductController`-Klasse. Die zweite Codezeile Ruft die `Details()` Aktionsmethode des Controllers auf. Schließlich überprüft die letzte Codezeile, ob die Ansicht, die von der `Details()` Aktion zurückgegeben wurde, die Detailansicht ist.

Die `ViewResult.ViewName`-Eigenschaft stellt den Namen der von einem Controller zurückgegebenen Ansicht dar. Eine große Warnung beim Testen dieser Eigenschaft. Es gibt zwei Möglichkeiten, wie ein Controller eine Ansicht zurückgeben kann. Ein Controller kann eine Ansicht wie die folgende explizit zurückgeben:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Alternativ kann der Name der Sicht wie folgt aus dem Namen der Controller Aktion abgeleitet werden:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Diese Controller Aktion gibt auch eine Ansicht mit dem Namen `Details`zurück. Der Name der Ansicht wird jedoch aus dem Aktions Namen abgeleitet. Wenn Sie den Ansichts Namen testen möchten, müssen Sie den Ansichts Namen explizit aus der Controller Aktion zurückgeben.

Sie können den Komponenten Test in der Liste 2 ausführen, indem Sie die Tastenkombination **STRG + R eingeben,** oder indem Sie auf die Schaltfläche **alle Tests in** der Projekt Mappe ausführen klicken (siehe Abbildung 1). Wenn der Test durchlaufen wird, wird das Testergebnisse Fenster in Abbildung 2 angezeigt.

[![alle Tests in der Projekt Mappe ausführen](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Abbildung 01**: Ausführen aller Tests in der Projekt Mappe ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))

[![Erfolg!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Abbildung 02**: Erfolg! ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))

## <a name="testing-the-view-data-returned-by-a-controller"></a>Testen der von einem Controller zurückgegebenen Ansichts Daten

Ein MVC-Controller übergibt Daten an eine Ansicht, indem er einen sogenannten *`View Data`* verwendet. Angenommen, Sie möchten die Details für ein bestimmtes Produktanzeigen, wenn Sie die `ProductController Details()` Aktion aufrufen. In diesem Fall können Sie eine Instanz einer `Product`-Klasse (definiert in Ihrem Modell) erstellen und die Instanz an die `Details` Ansicht übergeben, indem Sie `View Data`nutzen.

Der geänderte `ProductController` in der Liste 3 enthält eine aktualisierte `Details()` Aktion, die ein Produkt zurückgibt.

**Codebeispiel 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Zuerst wird durch die `Details()`-Aktion eine neue Instanz der `Product`-Klasse erstellt, die einen Laptop Computer darstellt. Als nächstes wird die Instanz der `Product`-Klasse als zweiter Parameter an die `View()`-Methode übergeben.

Sie können Komponententests schreiben, um zu testen, ob die erwarteten Daten in den Ansichts Daten enthalten sind. Der Komponenten Test in der Liste 4 testet, ob ein Produkt, das einen Laptop darstellt, beim Abrufen der `ProductController Details()` Aktionsmethode zurückgegeben wird.

**Codebeispiel 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

In der Liste 4 testet die `TestDetailsView()`-Methode die durch Aufrufen der `Details()`-Methode zurückgegebenen Ansichts Daten. Der `ViewData` wird als Eigenschaft für den `ViewResult` verfügbar gemacht, der durch Aufrufen der `Details()`-Methode zurückgegeben wird. Die `ViewData.Model`-Eigenschaft enthält das an die Ansicht über gegebene Produkt. Der Test überprüft lediglich, ob das in den Ansichts Daten enthaltene Produkt den Namen Laptop hat.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testen des von einem Controller zurückgegebenen Aktions Ergebnisses

Eine komplexere Controller Aktion kann abhängig von den Werten der Parameter, die an die Controller Aktion weitergeleitet werden, unterschiedliche Aktions Ergebnistypen zurückgeben. Eine Controller Aktion kann verschiedene Arten von Aktions Ergebnissen zurückgeben, einschließlich einer `ViewResult`, `RedirectToRouteResult`oder `JsonResult`.

Beispielsweise gibt die geänderte `Details()` Aktion in Auflistung 5 die `Details` Ansicht zurück, wenn Sie eine gültige Produkt-ID an die Aktion übergeben. Wenn Sie eine ungültige Produkt-ID übergeben--eine ID mit einem Wert kleiner als 1--, werden Sie zur `Index()` Aktion umgeleitet.

**Codebeispiel 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Sie können das Verhalten der `Details()` Aktion mit dem Komponenten Test in der Liste 6 testen. Der Komponenten Test in der Liste 6 überprüft, ob Sie zur `Index` Ansicht umgeleitet werden, wenn eine ID mit dem Wert-1 an die `Details()`-Methode weitergegeben wird.

**Codebeispiel 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Wenn Sie die `RedirectToAction()`-Methode in einer Controller Aktion aufzurufen, gibt die Controller Aktion eine `RedirectToRouteResult`zurück. Der Test überprüft, ob der `RedirectToRouteResult` den Benutzer an eine Controller Aktion mit dem Namen `Index`weiterleitet.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie Komponententests für MVC-Controller Aktionen erstellen. Zuerst haben Sie erfahren, wie Sie überprüfen, ob die Rechte Ansicht von einer Controller Aktion zurückgegeben wird. Sie haben gelernt, wie Sie die `ViewResult.ViewName`-Eigenschaft verwenden, um den Namen einer Ansicht zu überprüfen.

Als nächstes haben wir untersucht, wie Sie den Inhalt von `View Data`testen können. Nachdem Sie eine Controller Aktion aufgerufen haben, haben Sie gelernt, ob das richtige Produkt in `View Data` zurückgegeben wurde.

Schließlich haben wir erläutert, wie Sie überprüfen können, ob verschiedene Arten von Aktions Ergebnissen von einer Controller Aktion zurückgegeben werden. Sie haben gelernt, ob ein Controller eine `ViewResult` oder eine `RedirectToRouteResult`zurückgibt.

> [!div class="step-by-step"]
> [Previous](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
