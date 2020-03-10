---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Verwenden von AJAX zum Implementieren von Mapping-Szenarios | Microsoft-Dokumentation
author: microsoft
description: In Schritt 11 wird gezeigt, wie Sie die Unterstützung von AJAX-Zugriffen in unsere "nerddinner"-Anwendung integrieren, sodass Benutzer, die Abendessen erstellen, bearbeiten oder anzeigen, die l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 7fc90f978b9f9eca511feca70a3c0d02ec69b940
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468537"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Implementieren von Zuordnungsszenarien mithilfe von AJAX

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 11 des kostenlosen Lernprogramms ["nerddinner"](introducing-the-nerddinner-tutorial.md) , in dem erläutert wird, wie Sie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellen.
> 
> In Schritt 11 wird gezeigt, wie Sie die Unterstützung von AJAX-Zugriffen in unsere "nerddinner"-Anwendung integrieren, sodass Benutzer, die Abendessen erstellen, bearbeiten oder anzeigen, den Speicherort des Dinner grafisch sehen
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>Nerddinner Step 11: Integrieren einer AJAX-Karte

Wir machen unsere Anwendung nun etwas visueller, da Sie die Unterstützung von AJAX-Mapping integrieren. Dies ermöglicht es Benutzern, die Abendessen erstellen, bearbeiten oder anzeigen, den Speicherort des Dinner grafisch anzuzeigen.

### <a name="creating-a-map-partial-view"></a>Erstellen einer Zuordnungs partiellen Ansicht

Wir verwenden die Mapping-Funktionalität an verschiedenen Stellen in unserer Anwendung. Damit der Code trocken bleibt, Kapseln wir die gemeinsame Karten Funktionalität in einer einzelnen partiellen Vorlage, die wir für mehrere Controller Aktionen und-Ansichten wieder verwenden können. Wir nennen diese Teilansicht "map. ascx" und erstellen Sie im Verzeichnis "\views\dinner".

Sie können map. ascx partiell erstellen, indem Sie mit der rechten Maustaste auf das Verzeichnis "\views\dinner" klicken und den Menübefehl Add-&gt;View auswählen. Wir nennen die Ansicht "map. ascx", überprüfen Sie als Teilansicht und geben an, dass wir ihr eine stark typisierte "Dinner"-Modell Klasse übergeben werden:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, wird unsere partielle Vorlage erstellt. Anschließend aktualisieren wir die Datei "map. ascx" mit folgendem Inhalt:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Das erste &lt;Skript&gt; Referenz verweist auf die Microsoft Virtual Earth 6,2-Mapping-Bibliothek. Die zweite &lt;Skript&gt; Referenz verweist auf die Datei "map. js", die wir in Kürze erstellen, die die allgemeine JavaScript-Zuordnungs Logik kapeihen wird. Das&gt; Element &lt;div id = "diemap" ist der HTML-Container, der von Virtual Earth zum Hosten der Zuordnung verwendet wird.

Anschließend verfügen wir über ein eingebettetes &lt;Skript&gt; Block, der zwei JavaScript-Funktionen enthält, die für diese Sicht spezifisch sind. Die erste Funktion verwendet jQuery, um eine Funktion zu verknüpfen, die ausgeführt wird, wenn die Seite für die Ausführung des Client seitigen Skripts bereit ist. Sie ruft eine loadMap ()-Hilfsfunktion auf, die wir in der "map. js"-Skriptdatei definieren, um das Virtual Earth-Karten Steuerelement zu laden. Die zweite Funktion ist ein Rückruf Ereignishandler, der der Zuordnung, die einen Speicherort identifiziert, eine PIN hinzufügt.

Beachten Sie, dass ein serverseitiger &lt;% =%&gt;-Block innerhalb des Client seitigen Skript Blocks verwendet wird, um den breiten-und Längengrad des Dinner einzubetten, das in JavaScript zugeordnet werden soll. Dies ist eine nützliche Methode, um dynamische Werte auszugeben, die vom Client seitigen Skript verwendet werden können (ohne dass ein separater AJAX-Rückruf an den Server erforderlich ist, um die Werte abzurufen – wodurch Sie schneller ist). Die &lt;% =%&gt; Blöcke werden ausgeführt, wenn die Sicht auf dem Server gerendert wird, sodass die Ausgabe der HTML-Datei mit eingebetteten JavaScript-Werten endet (z. b.: var Latitude = 47,64312;).

### <a name="creating-a-mapjs-utility-library"></a>Erstellen einer "map. js"-Hilfsprogrammbibliothek

Nun erstellen wir die Datei "map. js", mit der wir die JavaScript-Funktionalität für unsere Karte Kapseln (und die oben genannten Methoden "loadMap" und "loadpin" implementieren können). Klicken Sie hierzu im Projekt mit der rechten Maustaste auf das Verzeichnis "\Scripts", und wählen Sie dann den Menübefehl "Add-&gt;New Item" aus. Wählen Sie das JScript-Element aus, und nennen Sie es "map. js".

Im folgenden finden Sie den JavaScript-Code, den wir der Datei "map. js" hinzufügen, die mit Virtual Earth interagieren wird, um unsere Karte anzuzeigen und den Speicherorte für unsere Abendessen hinzuzufügen:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrieren der Karte mit Formularen zum Erstellen und bearbeiten

Wir integrieren nun die Kartenunterstützung in unsere vorhandenen Erstellungs-und Bearbeitungs Szenarien. Die gute Nachricht ist, dass dies recht einfach ist, und es ist nicht erforderlich, dass wir unseren Controller Code ändern. Da unsere Ansichten "erstellen" und "Bearbeiten" eine gängige "dinnerform"-Teilansicht aufweisen, um die Dinner Form-Benutzeroberfläche zu implementieren, können wir die Karte an einem Ort hinzufügen und sowohl in den Erstellungs-als auch in den Bearbeitungs Szenarien

Wir müssen lediglich die Teilansicht "\views\dinners\dinnerform.ascx" öffnen und aktualisieren, um unsere neue Zuordnung einzubeziehen. Im folgenden sehen Sie, wie das aktualisierte dinnerform aussieht, sobald die Zuordnung hinzugefügt wird (Hinweis: die HTML-Formularelemente werden aus Gründen der Kürze im Code Ausschnitt ausgelassen):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Das partielle dinnerform-Objekt verwendet ein Objekt vom Typ "dinnerformviewmodel" als Modelltyp (da sowohl ein Dinner-Objekt als auch eine SelectList benötigt wird, um die Dropdown Liste der Länder aufzufüllen). Unsere Zuordnungs partielle benötigt lediglich ein Objekt vom Typ "Dinner" als Modelltyp, und wenn wir die Karte "Partial" Rendering, übergeben wir nur die Dinner Sub-Property von dinnerformviewmodel an diese:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Die JavaScript-Funktion, die der partiellen hinzugefügt wurde, verwendet jQuery, um ein "weich"-Ereignis an das HTML-Textfeld "Address" anzufügen. Sie haben wahrscheinlich von "Fokus"-Ereignissen gehört, die ausgelöst werden, wenn ein Benutzer auf ein Textfeld klickt. Das Gegenteil ist ein "weich"-Ereignis, das ausgelöst wird, wenn ein Benutzer ein Textfeld verlässt. Der obige Ereignishandler löscht die Text Feldwerte für breiten-und Längengrade, wenn dies geschieht, und gibt dann den neuen Adress Speicherort auf der Karte aus. Ein Rückruf Ereignishandler, den wir in der Datei "map. js" definiert haben, aktualisiert dann die Textfelder für die Längen-und Breitengrad in unserem Formular mithilfe von Werten, die von Virtual Earth basierend auf der von uns gegebenen Adresse zurückgegeben werden.

Wenn wir nun die Anwendung erneut ausführen und auf die Registerkarte "Host Dinner" klicken, sehen wir eine Standard Zuordnung, die zusammen mit den standardmäßigen Dinner Form-Elementen angezeigt wird:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Wenn wir eine Adresse eingeben und dann die Tab-Taste drücken, wird die Karte dynamisch aktualisiert, um den Speicherort anzuzeigen, und der Ereignishandler füllt die breiten-/Längen Grad-Textfelder mit den Speicherort Werten auf:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Wenn wir das neue Dinner speichern und es dann erneut zur Bearbeitung öffnen, werden Sie feststellen, dass der Zuordnungs Speicherort angezeigt wird, wenn die Seite geladen wird:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Jedes Mal, wenn das Adressfeld geändert wird, werden die Zuordnung und die Längen-/Längen Koordinaten aktualisiert.

Nun, da die Karte den Dinner-Speicherort anzeigt, können wir auch die Formularfelder für breiten-und Längengrade von sichtbaren Textfeldern in als ausgeblendete Elemente ändern (da die Zuordnung diese bei jeder eingegebenen Adresse automatisch aktualisiert). Zu diesem Zweck wechseln wir von der Verwendung des HTML. TextBox ()-HTML-Hilfsprogramms zur Verwendung der HTML. Hidden ()-Hilfsmethode:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Und jetzt sind unsere Formulare etwas benutzerfreundlicher und vermeiden es, den unformatierten breiten-/Längen Grad anzuzeigen (während Sie immer noch mit jedem Dinner in der Datenbank gespeichert werden):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrieren der Zuordnung in die Detailansicht

Nun, da wir die Karte in unsere Erstellungs-und Bearbeitungs Szenarien integriert haben, können wir Sie auch in unser Detail Szenario integrieren. Wir müssen lediglich &lt;% HTML. renderpartial ("Map") aufzurufen. %&gt; in der Detailansicht.

Im folgenden sehen Sie, wie der Quellcode für die gesamte Detailansicht (mit Karten Integration) aussieht:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Wenn ein Benutzer nun zu einer/Dinners/Details/[ID]-URL navigiert, sehen Sie Details zum Dinner, den Speicherort des Abend Bilds auf der Karte (mit einer Push-Pin, bei der der Mauszeiger auf den Titel des Abend Bilds zeigt) und einen AJAX-Link zu RSVP für ihn:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementieren der Suche nach Orten in unserer Datenbank und im Repository

Zum Abschließen der AJAX-Implementierung fügen wir der Startseite der Anwendung eine Karte hinzu, mit der Benutzer grafisch nach Dinner-Dateien suchen können.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Wir beginnen damit, die Unterstützung in unserer Datenbank und in der Datenrepository-Ebene zu implementieren, um eine Speicherort basierte RADIUS-Suche nach Abendessen effizient durchzuführen Wir könnten die neuen [räumlichen Features von SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) verwenden, um dies zu implementieren, oder Alternativ können wir einen SQL-Funktions Ansatz verwenden, der von Gary dryder im folgenden Artikel erläutert wird: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) und Rob-Artikel über die Verwendung von mit LINQ to SQL hier: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Um dieses Verfahren zu implementieren, öffnen Sie das "Server-Explorer" in Visual Studio, wählen Sie "nerddinner Database" aus, und klicken Sie dann mit der rechten Maustaste auf den untergeordneten Knoten "Functions", und wählen Sie die Option zum Erstellen einer neuen Skalarwertfunktion aus:

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Anschließend fügen wir die folgende distancebetween-Funktion ein:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Anschließend erstellen wir in SQL Server eine neue Tabellenwert Funktion, die wir als "nearestdinner" bezeichnen:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Diese "nearestdinner"-Tabellen Funktion verwendet die distancebetween-Hilfsfunktion, um alle Abendessen innerhalb von 100 Meilen des breiten-und Längen Grads zurückzugeben, das wir bereitstellen:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Um diese Funktion aufzurufen, öffnen Sie zunächst den LINQ to SQL-Designer, indem Sie auf die Datei "nerddinner. dbml" im Verzeichnis "\models" doppelklicken:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Anschließend ziehen wir die Funktionen "nearestdinner" und "distancebetween" auf den LINQ to SQL-Designer, was dazu führt, dass Sie in unserer LINQ to SQL nerddinnerdatacontext-Klasse als Methoden hinzugefügt werden:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Anschließend können wir die Abfrage Methode "findbylocation" für unsere dinnerrepository-Klasse verfügbar machen, die die nearestdinner-Funktion verwendet, um anstehende Dinner-Werte zurückzugeben, die sich innerhalb von 100 Meilen des angegebenen Standorts befinden:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementieren einer JSON-basierten AJAX-Such Aktionsmethode

Nun implementieren wir eine Controller Aktionsmethode, die die neue findbylocation ()-Repository-Methode nutzt, um eine Liste der Dinner-Daten zurückzugeben, die zum Auffüllen einer Karte verwendet werden können. Diese Aktionsmethode gibt die Dinner-Daten in einem JSON-Format (JavaScript Object Notation) zurück, sodass es problemlos mithilfe von JavaScript auf dem Client manipuliert werden kann.

Um dies zu implementieren, erstellen Sie eine neue Klasse "searchcontroller", indem Sie mit der rechten Maustaste auf das Verzeichnis "\controllers" klicken und den Menübefehl Add-&gt;Controller auswählen. Anschließend implementieren wir eine "searchbylocation"-Aktionsmethode in der neuen searchcontroller-Klasse wie im folgenden Beispiel:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Die searchbylocation-Aktionsmethode von searchcontroller ruft intern die findbylocation-Methode in dinnerrepository auf, um eine Liste von nahe gelegenen Abendessen zu erhalten. Anstatt die Dinner-Objekte direkt an den Client zurückzugeben, gibt Sie stattdessen jsondinner Objects zurück. Die jsondinner-Klasse macht eine Teilmenge der Dinner-Eigenschaften verfügbar (z. b. aus Sicherheitsgründen werden die Namen der Personen, die RSVP für ein Dinner haben, nicht offengelegt). Außerdem enthält Sie eine rsvpcount-Eigenschaft, die nicht in Dinner – vorhanden ist. diese wird dynamisch berechnet, indem die Anzahl von RSVP-Objekten gezählt wird, die einem bestimmten Dinner zugeordnet sind.

Anschließend verwenden wir die Hilfsmethode JSON () für die Controller-Basisklasse, um die Reihenfolge der Abendessen mit einem JSON-basierten Wire-Format zurückzugeben. JSON ist ein Standardtext Format, das einfache Datenstrukturen darstellt. Im folgenden finden Sie ein Beispiel dafür, wie eine JSON-formatierte Liste mit zwei jsondinner-Objekten aussieht, wenn Sie von der Aktionsmethode zurückgegeben wird:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Aufrufen der JSON-basierten AJAX-Methode mithilfe von jQuery

Wir können nun die Startseite der "nerddinner"-Anwendung aktualisieren, um die searchbylocation-Aktionsmethode von searchcontroller zu verwenden. Zu diesem Zweck öffnen Sie die/Views/Home/Index.aspx-Ansichts Vorlage und aktualisieren Sie, sodass Sie über ein Textfeld, eine Such Schaltfläche, eine Karte und ein &lt;div&gt;-Element mit dem Namen "dinnerlist" verfügt:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Wir können der Seite dann zwei JavaScript-Funktionen hinzufügen:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Die erste JavaScript-Funktion lädt die Karte, wenn die Seite erstmalig geladen wird. Die zweite JavaScript-Funktion verbindet einen JavaScript-Click-Ereignishandler auf der Such Schaltfläche. Wenn die Schaltfläche gedrückt wird, wird die Funktion finddinnersgivenlocation () JavaScript aufgerufen, die wir der Datei "map. js" hinzufügen:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Diese finddinnersgivenlocation ()-Funktion ruft Map auf. Suchen Sie auf dem Virtual Earth-Steuerelement, um es auf dem eingegebenen Speicherort zu zentrieren. Wenn der Virtual Earth-Kartendienst zurückgegeben wird, wird die Karte angezeigt. Die Find ()-Methode ruft die callbackupdatemapdinner-Rückruf Methode auf, die wir als letztes Argument angegeben haben.

Die callbackupdatemapdinner ()-Methode ist der Ort, an dem die eigentliche Arbeit erfolgt. Er verwendet die $. Post ()-Hilfsmethode von jQuery, um einen AJAX-Befehl an die searchbylocation ()-Aktionsmethode von searchcontroller auszuführen, – übergibt er die breiten-und Längengrade der neu zentrierten Karte. Es definiert eine Inline Funktion, die aufgerufen wird, wenn die $. Post ()-Hilfsmethode abgeschlossen wird, und die JSON-formatierten Dinner-Ergebnisse, die von der searchbylocation ()-Aktionsmethode zurückgegeben werden, werden mit einer Variablen namens "Dinner" an Sie übermittelt. Anschließend führt er einen Foreach-Vorgang für jedes zurückgegebene Dinner aus und verwendet den breiten-und Längengrad und andere Eigenschaften des Essens, um eine neue PIN in der Karte hinzuzufügen. Außerdem wird der HTML-Liste der Abendessen auf der rechten Seite der Karte ein Dinner-Eintrag hinzugefügt. Anschließend wird ein Hover-Ereignis für die Pushpins und die HTML-Liste eingeblendet, sodass Details zum Dinner angezeigt werden, wenn ein Benutzer darauf zeigt:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Und wenn wir nun die Anwendung ausführen und die Startseite besuchen, wird eine Karte angezeigt. Wenn wir den Namen einer Stadt eingeben, werden in der Karte die bevorstehenden Abendessen in der Nähe angezeigt:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Wenn Sie den Mauszeiger über ein Dinner bewegen, werden Details dazu angezeigt.

Wenn Sie entweder in der Blase oder auf der rechten Seite in der HTML-Liste auf den Dinner Title klicken, gelangen Sie zum Dinner –, für das wir optional eine RSVP ausführen können:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt die gesamte Anwendungs Funktionalität unserer "nerddinner"-Anwendung implementiert. Sehen wir uns nun an, wie wir automatisierte Unittests aktivieren können.

> [!div class="step-by-step"]
> [Zurück](use-ajax-to-deliver-dynamic-updates.md)
> [Weiter](enable-automated-unit-testing.md)
