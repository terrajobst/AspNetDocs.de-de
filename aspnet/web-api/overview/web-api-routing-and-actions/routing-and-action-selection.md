---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing-und Aktions Auswahl in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446913"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing-und Aktions Auswahl in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.net-Web-API eine HTTP-Anforderung an eine bestimmte Aktion auf einem Controller weiterleitet.

> [!NOTE]
> Eine allgemeine Übersicht über das Routing finden Sie unter [Routing in ASP.net-Web-API](routing-in-aspnet-web-api.md).

In diesem Artikel werden die Details des Routing Prozesses erläutert. Wenn Sie ein Web-API-Projekt erstellen und feststellen, dass einige Anforderungen nicht erwartungsgemäß weitergeleitet werden, wird Ihnen dieser Artikel hoffentlich helfen.

Das Routing umfasst drei Hauptphasen:

1. Übereinstimmung des URIs mit einer Routen Vorlage.
2. Auswählen eines Controllers.
3. Auswählen einer Aktion.

Sie können einige Teile des Prozesses durch ihre eigenen benutzerdefinierten Verhaltensweisen ersetzen. In diesem Artikel wird das Standardverhalten beschrieben. Am Ende notieren Sie die stellen, an denen Sie das Verhalten anpassen können.

## <a name="route-templates"></a>Routen Vorlagen

Eine Routen Vorlage ähnelt einem URI-Pfad, kann jedoch Platzhalter Werte enthalten, die mit geschweiften Klammern angegeben werden:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Wenn Sie eine Route erstellen, können Sie Standardwerte für einige oder alle Platzhalter angeben:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Sie können auch Einschränkungen bereitstellen, die einschränken, wie ein URI-Segment einem Platzhalter entsprechen kann:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Das Framework versucht, die Segmente im URI-Pfad mit der Vorlage abzugleichen. Literale in der Vorlage müssen exakt übereinstimmen. Ein Platzhalter entspricht einem beliebigen Wert, es sei denn, Sie geben Einschränkungen an. Das Framework stimmt nicht mit anderen Teilen des URI, wie z. b. dem Hostnamen oder den Abfrage Parametern, ab. Das Framework wählt die erste Route in der Routing Tabelle aus, die dem URI entspricht.

Es gibt zwei spezielle Platzhalter: "{controller}" und "{action}".

- "{controller}" gibt den Namen des Controllers an.
- "{action}" gibt den Namen der Aktion an. In der Web-API besteht die übliche Konvention darin, "{action}" auszulassen.

### <a name="defaults"></a>Standardeinstellungen

Wenn Sie Standardwerte angeben, entspricht die Route einem URI, der diese Segmente fehlt. Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Die URIs `http://localhost/api/products/all` und `http://localhost/api/products` der vorangehenden Route entsprechen. Im letzteren URI wird dem fehlenden `{category}` Segment der Standardwert `all`zugewiesen.

### <a name="route-dictionary"></a>Routen Wörterbuch

Wenn das Framework eine Entsprechung für einen URI findet, wird ein Wörterbuch erstellt, das den Wert für jeden Platzhalter enthält. Bei den Schlüsseln handelt es sich um die Platzhalter Namen, ohne die geschweiften Klammern. Die Werte werden aus dem URI-Pfad oder aus den Standardwerten entnommen. Das Wörterbuch wird im **ihttproutedata** -Objekt gespeichert.

Während dieser Weiterleitungs Übereinstimmungs Phase werden die besonderen Platzhalter "{controller}" und "{action}" genau wie die anderen Platzhalter behandelt. Sie werden einfach im Wörterbuch mit den anderen Werten gespeichert.

Ein Standardwert kann den besonderen Wert **RouteParameter. optional**aufweisen. Wenn ein Platzhalter diesem Wert zugewiesen wird, wird der Wert nicht dem Routen Wörterbuch hinzugefügt. Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Für den URI-Pfad "API/Products" enthält das Routen Wörterbuch Folgendes:

- Controller: "Products"
- Kategorie: "alle"

Für "API/Products/Toys/123" enthält das Routen Wörterbuch jedoch Folgendes:

- Controller: "Products"
- Kategorie: "Toys"
- id: "123"

Die Standardwerte können auch einen Wert enthalten, der nicht an einer beliebigen Stelle in der Routen Vorlage angezeigt wird. Wenn die Route übereinstimmt, wird dieser Wert im Wörterbuch gespeichert. Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Wenn der URI-Pfad "API/root/8" lautet, enthält das Wörterbuch zwei Werte:

- Controller: "Customers"
- ID: "8"

## <a name="selecting-a-controller"></a>Auswählen eines Controllers

Die Controller Auswahl wird von der **ihttpcontrollerselector. selectcontroller** -Methode behandelt. Diese Methode nimmt eine **httprequestmessage** -Instanz an und gibt einen **httpcontrollerdescriptor**zurück. Die Standard Implementierung wird von der **defaulthttpcontrollerselector** -Klasse bereitgestellt. Diese Klasse verwendet einen einfachen Algorithmus:

1. Suchen Sie im Routen Wörterbuch nach dem Schlüssel "controller".
2. Nehmen Sie den Wert für diesen Schlüssel an, und fügen Sie die Zeichenfolge "controller" an, um den Namen des Controller Typs zu erhalten.
3. Suchen Sie nach einem Web-API-Controller mit diesem Typnamen.

Wenn das Routen Wörterbuch beispielsweise das Schlüssel-Wert-Paar "controller" = "Products" enthält, ist der Controllertyp "productscontroller". Wenn kein übereinstimmender Typ oder mehrere Übereinstimmungen vorhanden sind, gibt das Framework einen Fehler an den Client zurück.

In Schritt 3 verwendet **defaulthttpcontrollerselector** die **ihttpcontrollertyperesolver** -Schnittstelle, um die Liste der Web-API-Controller Typen abzurufen. Die Standard Implementierung von **ihttpcontrollertyperesolver** gibt alle öffentlichen Klassen zurück, die (a) **ihttpcontroller**implementieren, (b) nicht abstrakt sind und (c) einen Namen haben, der auf "controller" endet.

## <a name="action-selection"></a>Aktions Auswahl

Nachdem Sie den Controller ausgewählt haben, wählt das Framework die Aktion durch Aufrufen der **ihttpactionselector. SelectAction** -Methode aus. Diese Methode nimmt einen **httpcontrollercontext** an und gibt einen **httpactiondescriptor**zurück.

Die Standard Implementierung wird von der **apicontrolleraction Selector** -Klasse bereitgestellt. Zum Auswählen einer Aktion wird Folgendes untersucht:

- Die HTTP-Methode der Anforderung.
- Der Platzhalter "{action}" in der Routen Vorlage, falls vorhanden.
- Die Parameter der Aktionen auf dem Controller.

Bevor wir uns den Auswahl Algorithmus ansehen, müssen wir einige Dinge zu Controller Aktionen verstehen.

**Welche Methoden auf dem Controller werden als "Aktionen" betrachtet?** Wenn eine Aktion ausgewählt wird, untersucht das Framework nur öffentliche Instanzmethoden auf dem Controller. Außerdem werden ["spezielle namens Methoden"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (Konstruktoren, Ereignisse, Operator Überladungen usw.) und Methoden, die von der **apicontroller** -Klasse geerbt werden, ausgeschlossen.

**HTTP-Methoden.** Das Framework wählt nur Aktionen aus, die der HTTP-Methode der Anforderung entsprechen, wie folgt bestimmt:

1. Sie können die HTTP-Methode mit einem Attribut angeben: **akzeptverbs**, **httpdelete**, **HttpGet**, **httphead**, **httpoptions**, **httppatch**, **HttpPost**oder **httpput**.
2. Andernfalls, wenn der Name der Controller Methode mit "Get", "Post", "Put", "Delete", "Head", "Options" oder "Patch" beginnt, unterstützt die Aktion die HTTP-Methode gemäß der Konvention.
3. Wenn keine der oben genannten Punkte angegeben ist, unterstützt die Methode Post.

**Parameter Bindungen.** Eine Parameter Bindung besteht darin, wie die Web-API einen Wert für einen Parameter erstellt. Dies ist die Standardregel für die Parameter Bindung:

- Einfache Typen werden aus dem URI entnommen.
- Komplexe Typen werden aus dem Anforderungs Text entnommen.

Einfache Typen umfassen alle [.NET Framework primitiven Typen](https://msdn.microsoft.com/library/system.type.isprimitive)sowie **DateTime**, **Decimal**, **GUID**, **String**und **TimeSpan**. Für jede Aktion kann höchstens ein Parameter den Anforderungs Text lesen.

> [!NOTE]
> Es ist möglich, die Standard Bindungs Regeln zu überschreiben. Weitere Informationen finden Sie [unter WebAPI-Parameter Bindung im](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)Hintergrund.

Mit diesem Hintergrund ist hier der Aktions Auswahl Algorithmus.

1. Erstellen Sie eine Liste aller Aktionen auf dem Controller, die der HTTP-Anforderungs Methode entsprechen.
2. Wenn das Routen Wörterbuch über einen "action"-Eintrag verfügt, entfernen Sie Aktionen, deren Name diesem Wert nicht entspricht.
3. Versuchen Sie, die Aktionsparameter wie folgt mit dem URI zu vergleichen: 

    1. Für jede Aktion wird eine Liste der Parameter abgerufen, bei denen es sich um einen einfachen Typ handelt, bei dem die Bindung den Parameter aus dem URI abruft. Optionale Parameter ausschließen.
    2. Versuchen Sie aus dieser Liste, eine Entsprechung für jeden Parameternamen zu finden, entweder im Routen Wörterbuch oder in der URI-Abfrage Zeichenfolge. Bei Übereinstimmungen wird die Groß-/Kleinschreibung nicht beachtet, und die Reihenfolge der Parameter
    3. Wählen Sie eine Aktion aus, bei der jeder Parameter in der Liste eine Entsprechung im URI aufweist.
    4. Wenn mehr als eine Aktion diese Kriterien erfüllt, wählen Sie die mit den meisten Parameter Übereinstimmungen aus.
4. Ignorieren von Aktionen mit dem **[NonAction]** -Attribut.

Schritt #3 ist wahrscheinlich die verwirrend. Die grundlegende Idee ist, dass ein Parameter seinen Wert entweder aus dem URI, aus dem Anforderungs Text oder aus einer benutzerdefinierten Bindung erhalten kann. Für Parameter, die aus dem URI stammen, möchten wir sicherstellen, dass der URI tatsächlich einen Wert für diesen Parameter enthält, entweder im Pfad (über das Routen Wörterbuch) oder in der Abfrage Zeichenfolge.

Sehen Sie sich beispielsweise die folgende Aktion an:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Der *ID* -Parameter wird an den URI gebunden. Daher kann diese Aktion nur einem URI entsprechen, der einen Wert für "ID" enthält, entweder im Routen Wörterbuch oder in der Abfrage Zeichenfolge.

Optionale Parameter stellen eine Ausnahme dar, da Sie optional sind. Für einen optionalen Parameter ist es in Ordnung, wenn die Bindung den Wert aus dem URI nicht erhalten kann.

Komplexe Typen sind aus einem anderen Grund eine Ausnahme. Ein komplexer Typ kann nur über eine benutzerdefinierte Bindung an den URI gebunden werden. Aber in diesem Fall kann das Framework nicht im Voraus wissen, ob der Parameter an einen bestimmten URI gebunden würde. Zum ermitteln muss die Bindung aufgerufen werden. Das Ziel des Auswahl Algorithmus besteht darin, vor dem Aufrufen von Bindungen eine Aktion aus der statischen Beschreibung auszuwählen. Daher werden komplexe Typen aus dem übereinstimmenden Algorithmus ausgeschlossen.

Nachdem die Aktion ausgewählt wurde, werden alle Parameter Bindungen aufgerufen.

Zusammenfassung:

- Die Aktion muss mit der HTTP-Methode der Anforderung identisch sein.
- Der Aktionsname muss mit dem Eintrag "action" im Routen Wörterbuch, falls vorhanden, identisch sein.
- Wenn der Parameter für jeden Parameter der Aktion aus dem URI entnommen wird, muss der Parameter Name entweder im Routen Wörterbuch oder in der URI-Abfrage Zeichenfolge gefunden werden. (Optionale Parameter und Parameter mit komplexen Typen werden ausgeschlossen.)
- Versuchen Sie, die meisten Parameter zu vergleichen. Die beste Entsprechung kann eine Methode ohne Parameter sein.

## <a name="extended-example"></a>Erweitertes Beispiel

Radwegen

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controller:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP-Anforderung:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Routen Abgleich

Der URI entspricht der Route mit dem Namen "defaultapi". Das Routen Wörterbuch enthält die folgenden Einträge:

- Controller: "Products"
- id: "1"

Das Routen Wörterbuch enthält nicht die Abfrage Zeichenfolgen-Parameter "Version" und "Details", aber diese werden während der Aktions Auswahl weiterhin berücksichtigt.

### <a name="controller-selection"></a>Controller Auswahl

Aus dem Eintrag "controller" im Routen Wörterbuch ist der Controllertyp `ProductsController`.

### <a name="action-selection"></a>Aktions Auswahl

Die HTTP-Anforderung ist eine GET-Anforderung. Die Controller Aktionen, die Get unterstützen, sind `GetAll`, `GetById`und `FindProductsByName`. Das Routen Wörterbuch enthält keinen Eintrag für "action", daher müssen wir nicht mit dem Aktions Namen vergleichen.

Als nächstes versuchen wir, die Parameternamen für die Aktionen abzugleichen, indem wir nur die get-Aktionen betrachten.

| Aktion | Parameter für die Abgleich |
| --- | --- |
| `GetAll` | none |
| `GetById` | "id" |
| `FindProductsByName` | Benennen |

Beachten Sie, dass der *Versions* Parameter von `GetById` nicht berücksichtigt wird, da es sich um einen optionalen Parameter handelt.

Die `GetAll`-Methode entspricht trivial. Die `GetById`-Methode stimmt ebenfalls überein, da das Routen Wörterbuch "ID" enthält. Die `FindProductsByName`-Methode stimmt nicht mit ab.

Die `GetById`-Methode gewinnt, da Sie mit einem Parameter übereinstimmt, im Gegensatz zu den Parametern für `GetAll`. Die-Methode wird mit den folgenden Parameterwerten aufgerufen:

- *ID* = 1
- *Version* = 1,5

Beachten Sie, dass der Wert des-Parameters aus der URI-Abfrage Zeichenfolge stammt, obwohl die *Version* im Auswahl Algorithmus nicht verwendet wurde.

## <a name="extension-points"></a>Erweiterungspunkte

Die Web-API stellt Erweiterungs Punkte für einige Teile des Routing Prozesses bereit.

| Schnittstelle | Beschreibung |
| --- | --- |
| **Ihttpcontrollerselector** | Wählt den Controller aus. |
| **Ihttpcontrollertyperesolver** | Ruft die Liste der Controller Typen ab. Der " **defaulthttpcontrollerselector" wählt den Controllertyp** aus der Liste aus. |
| **Iassembliesresolver** | Ruft die Liste der projektenassemblys ab. Die **ihttpcontrollertyperesolver** -Schnittstelle verwendet diese Liste, um die Controller Typen zu suchen. |
| **Ihttpcontrolleractivator** | Erstellt neue Controller Instanzen. |
| **Ihttpactionselector** | Wählt die Aktion aus. |
| **Ihttpactioninvoker** | Ruft die Aktion auf. |

Verwenden Sie die **Services** -Auflistung für das **httpconfiguration** -Objekt, um eine eigene Implementierung für eine dieser Schnittstellen bereitzustellen:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
