---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Verwenden von ViewData und Implementieren von ViewModel-Klassen | Microsoft-Dokumentation
author: microsoft
description: In Schritt 6 wird gezeigt, wie Sie die Unterstützung für umfangreichere Formular Bearbeitungs Szenarien aktivieren. Außerdem werden zwei Ansätze erläutert, die verwendet werden können, um Daten von Controllern an Sichten zu übergeben:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: ca9775417c2e25952511a73096fb76d5d4edaea2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435531"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Verwenden von ViewData und Implementieren von ViewModel-Klassen

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 6 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> In Schritt 6 wird gezeigt, wie Sie die Unterstützung für umfangreichere Formular Bearbeitungs Szenarien aktivieren. Außerdem werden zwei Ansätze erläutert, die zum Übergeben von Daten von Controllern an Sichten verwendet werden können: ViewData und ViewModel.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>Nerddinner Step 6: ViewData und ViewModel

Wir haben eine Reihe von Formular Bereitstellungs Szenarien behandelt und erläutert, wie die Unterstützung von CREATE, Update und DELETE (CRUD) implementiert wird. Wir werden jetzt die Implementierung von dinnerscontroller weiterleiten und Unterstützung für umfangreichere Formular Bearbeitungs Szenarien aktivieren. Dabei werden zwei Ansätze erläutert, die verwendet werden können, um Daten von Controllern an Sichten zu übergeben: ViewData und ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Übergeben von Daten von Controllern an View-Templates

Eine der definierenden Merkmale des MVC-Musters ist die strikte "Trennung von Belangen", die es ermöglicht, zwischen den verschiedenen Komponenten einer Anwendung zu erzwingen. Modelle, Controller und Ansichten verfügen jeweils über gut definierte Rollen und Verantwortlichkeiten und kommunizieren auf gut definierte Weise untereinander miteinander. Dies trägt zur herauf Stufung von Testability und Wiederverwendung von Code bei

Wenn eine Controller Klasse beschließt, eine HTML-Antwort zurück an einen Client zu übergeben, ist Sie dafür verantwortlich, explizit an die Sicht Vorlage alle Daten zu übergeben, die zum Rendering der Antwort erforderlich sind. Ansichts Vorlagen sollten niemals Datenabruf-oder Anwendungslogik – ausführen und sollten sich stattdessen auf den rendercode beschränken, der vom Controller an ihn geleitet wird.

Zurzeit sind die Modelldaten, die von unserer dinnerscontroller-Klasse an unsere Ansichts Vorlagen weitergeleitet werden, einfach und direkt – eine Liste von Dinner-Objekten im Fall von "Index ()" und ein einzelnes Dinner-Objekt im Fall von "Details ()", "Edit ()", "Create ()" und "Delete ()". Wenn wir unserer Anwendung weitere Benutzeroberflächen Funktionen hinzufügen, müssen wir häufig mehr als nur diese Daten übergeben, um HTML-Antworten innerhalb unserer Ansichts Vorlagen zu Rendering. Beispielsweise kann es sein, dass Sie das Feld "Country" in der Ansicht "Bearbeiten" und "erstellen" als HTML-Textfeld in eine Dropdown Liste ändern möchten. Anstatt die Dropdown Liste mit den Ländernamen in der Ansichts Vorlage hart zu codieren, können wir Sie aus einer Liste unterstützter Länder generieren, die wir dynamisch auffüllen. Wir benötigen eine Möglichkeit, das Dinner *-Objekt und* die Liste der unterstützten Länder von unserem Controller an unsere Ansichts Vorlagen zu übergeben.

Wir sehen uns zwei Möglichkeiten an, wie Sie dies erreichen können.

### <a name="using-the-viewdata-dictionary"></a>Verwenden des ViewData-Wörterbuchs

Die Controller-Basisklasse macht eine "ViewData"-Wörterbuch Eigenschaft verfügbar, die verwendet werden kann, um zusätzliche Datenelemente von Controllern an Sichten zu übergeben.

Um beispielsweise das Szenario zu unterstützen, in dem das Textfeld "Country" in der Bearbeitungs Ansicht von einem HTML-Textfeld zu einer Dropdown list geändert werden soll, können wir unsere Edit ()-Aktionsmethode aktualisieren, um (zusätzlich zu einem Dinner-Objekt) ein SelectList-Objekt zu übergeben, das als m verwendet werden kann. eine Dropdown Liste für Länder.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Der Konstruktor der obigen SelectList-Auflistung akzeptiert eine Liste von Countys, um die Dropdown Liste mit und den aktuell ausgewählten Wert aufzufüllen.

Wir können dann die Ansichts Vorlage "Edit. aspx" aktualisieren, um die Hilfsmethode "HTML. DropDownList ()" anstelle der zuvor verwendeten Hilfsmethode "HTML. TextBox ()" zu verwenden:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Die oben genannte HTML. DropDownList ()-Hilfsmethode erfordert zwei Parameter. Der erste ist der Name des HTML-Formular Elements, das ausgegeben werden soll. Die zweite ist das "SelectList"-Modell, das über das ViewData-Wörterbuch übermittelt wurde. Wir verwenden das C# Schlüsselwort "as", um den Typ innerhalb des Wörterbuchs als SelectList umzuwandeln.

Wenn wir nun die Anwendung ausführen und auf die */Dinners/Edit/1* -URL in unserem Browser zugreifen, sehen wir, dass unsere Bearbeitungs Oberfläche aktualisiert wurde, um eine Dropdown Liste von Ländern anstelle eines Textfelds anzuzeigen:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Da wir die Vorlage zum Bearbeiten der Ansicht auch aus der HTTP-Post-Bearbeitungsmethode Rendern (in Szenarios, in denen Fehler auftreten), möchten wir sicherstellen, dass wir diese Methode auch aktualisieren, um "ViewData" "SelectList" hinzuzufügen, wenn die Ansichts Vorlage in Fehler Szenarien gerendert wird:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Und jetzt unterstützt das Szenario "dinnerscontroller-Bearbeitung" eine Dropdown Liste.

### <a name="using-a-viewmodel-pattern"></a>Verwenden eines ViewModel-Musters

Der ViewData-Wörterbuch Ansatz hat den Vorteil, dass er relativ schnell und einfach zu implementieren ist. Einige Entwickler verwenden jedoch keine Zeichen folgen basierten Wörterbücher, da Tippfehler zu Fehlern führen können, die zur Kompilierzeit nicht abgefangen werden. Das nicht typisierte ViewData-Wörterbuch erfordert außerdem die Verwendung des "as"-Operators oder der Umwandlung, wenn eine C# stark typisierte Sprache wie in einer Ansichts Vorlage verwendet wird.

Ein alternativer Ansatz, den wir verwenden könnten, ist eine, die häufig als "ViewModel"-Muster bezeichnet wird. Bei Verwendung dieses Musters erstellen wir stark typisierte Klassen, die für unsere spezifischen Ansichts Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte und Inhalte verfügbar machen, die von unseren Ansichts Vorlagen benötigt werden. Unsere Controller Klassen können diese Ansichts optimierten Klassen dann auffüllen und an unsere Ansichts Vorlage übergeben, um Sie zu verwenden. Dies ermöglicht Typsicherheit, Kompilierungszeit Überprüfung und Editor-IntelliSense innerhalb von Ansichts Vorlagen.

Um z. b. Dinner Form-Bearbeitungs Szenarios zu aktivieren, können wir eine "dinnerformviewmodel"-Klasse wie unten beschrieben erstellen, die zwei stark typisierte Eigenschaften verfügbar macht: ein Dinner-Objekt und das SelectList-Modell, das zum Auffüllen der Länder-Dropdown Liste erforderlich ist:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Anschließend können wir unsere Edit ()-Aktionsmethode aktualisieren, um das dinnerformviewmodel mit dem Dinner-Objekt zu erstellen, das wir aus unserem Repository abrufen, und es dann an unsere View-Vorlage übergeben:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Anschließend aktualisieren wir die Ansichts Vorlage, sodass Sie ein "dinnerformviewmodel" anstelle eines "Dinner"-Objekts erwartet, indem Sie das Attribut "erbt" oben auf der Seite "Edit. aspx" ändern:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Sobald dies erfolgt ist, wird das IntelliSense der Eigenschaft "Model" in der Ansichts Vorlage aktualisiert, um das Objektmodell des Typs "dinnerformviewmodel" widerzuspiegeln, den wir übergeben:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Wir können dann den Ansichts Code aktualisieren, um ihn zu bearbeiten. Beachten Sie, dass die Namen der von uns erstellten Eingabeelemente nicht geändert werden (die Formularelemente werden weiterhin "Title", "Country" genannt) – aber wir aktualisieren die HTML-Hilfsmethoden, um die Werte mithilfe der Klasse "dinnerformviewmodel" abzurufen:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Außerdem aktualisieren wir unsere Methode zum Bearbeiten von Post, um beim Rendern von Fehlern die Klasse "dinnerformviewmodel" zu verwenden:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Wir können unsere Create ()-Aktionsmethoden auch aktualisieren, um genau dieselbe *dinnerformviewmodel* -Klasse zu verwenden, um auch die Dropdown List-Länder in diesen zu aktivieren. Im folgenden finden Sie die HTTP-Get-Implementierung:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Im folgenden finden Sie die Implementierung der HTTP-Post-Methode "Create":

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Und jetzt unterstützen unsere Bildschirme "Bearbeiten" und "erstellen" Dropdown Listen zum Auswählen des Landes.

### <a name="custom-shaped-viewmodel-classes"></a>Benutzerdefinierte ViewModel-Klassen

Im obigen Szenario macht unsere dinnerformviewmodel-Klasse das Dinner Model-Objekt direkt als Eigenschaft verfügbar, zusammen mit einer unterstützenden SelectList-Modell Eigenschaft. Diese Vorgehensweise eignet sich gut für Szenarien, in denen die HTML-Benutzeroberfläche, die wir in unserer Ansichts Vorlage erstellen möchten, relativ genau den Domänen Modell Objekten entspricht.

In Szenarios, in denen dies nicht der Fall ist, können Sie eine benutzerdefinierte ViewModel-Klasse erstellen, deren Objektmodell für die Verwendung durch die Sicht optimiert ist – und die sich möglicherweise völlig von dem zugrunde liegenden Domänen Modell Objekt unterscheiden. Beispielsweise könnten andere Eigenschaftsnamen und/oder Aggregat Eigenschaften, die von mehreren Modell Objekten gesammelt werden, verfügbar gemacht werden.

Benutzerdefinierte ViewModel-Klassen können verwendet werden, um Daten von Controllern an Sichten zu übergeben und um das Zurücksenden von Formulardaten an die Aktionsmethode eines Controllers zu unterstützen. Für dieses spätere Szenario kann die Aktionsmethode ein ViewModel-Objekt mit den vom Formular übermittelten Daten aktualisieren und dann die ViewModel-Instanz verwenden, um ein tatsächliches Domänen Modell Objekt zuzuordnen oder abzurufen.

Benutzerdefinierte ViewModel-Klassen können viel Flexibilität bereitstellen und können jederzeit untersucht werden, wenn Sie den Renderingcode in ihren Ansichts Vorlagen oder den Formular Bereitstellungs Code innerhalb ihrer Aktionsmethoden finden, der zu kompliziert ist. Dies ist oft ein Vorzeichen, dass Ihre Domänen Modelle nicht ordnungsgemäß mit der von Ihnen generierten Benutzeroberfläche übereinstimmen und dass eine benutzerdefinierte zwischen benutzerdefinierte ViewModel-Klasse hilfreich sein kann.

### <a name="next-step"></a>Nächster Schritt

Sehen wir uns nun an, wie wir partiale und Masterseiten verwenden können, um die Benutzeroberfläche in unserer Anwendung wiederzuverwenden und freizugeben.

> [!div class="step-by-step"]
> [Zurück](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Weiter](re-use-ui-using-master-pages-and-partials.md)
