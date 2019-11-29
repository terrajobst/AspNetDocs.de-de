---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Benennen von Steuerelement-IDs auf Inhaltsseiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Veranschaulicht, wie contentplachalter-Steuerelemente als Benennungs Container fungieren und somit die programmgesteuerte Arbeit mit einem Steuerelement erschweren können (über FindControl)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 3cb8dec47040bc65f1a024325c91590729ffbdb7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586669"
---
# <a name="control-id-naming-in-content-pages-vb"></a>Benennung von Steuerelement-IDs auf Inhaltsseiten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Veranschaulicht, wie contentplachalter-Steuerelemente als Benennungs Container fungieren und somit die programmgesteuerte Arbeit mit einem Steuerelement erschweren (über FindControl). Untersucht dieses Problem und Problem Umgehungen. Erläutert auch, wie Programm gesteuert auf den resultierenden ClientID-Wert zugegriffen werden kann.

## <a name="introduction"></a>Einführung

Alle ASP.NET-Server Steuerelemente enthalten eine `ID`-Eigenschaft, mit der das Steuerelement eindeutig identifiziert wird. Dies ist die Methode, mit der das Steuerelement Programm gesteuert in der Code Behind-Klasse aufgerufen wird. Ebenso können die Elemente in einem HTML-Dokument ein `id` Attribut enthalten, das das Element eindeutig identifiziert. Diese `id` Werte werden häufig im Client seitigen Skript verwendet, um Programm gesteuert auf ein bestimmtes HTML-Element zu verweisen. Wenn ein ASP.NET-Server Steuerelement in HTML gerendert wird, wird möglicherweise angenommen, dass sein `ID` Wert als `id` Wert des gerenderten HTML-Elements verwendet wird. Dies ist nicht unbedingt der Fall, da unter bestimmten Umständen ein einzelnes Steuerelement mit einem einzelnen `ID` Wert mehrmals im gerenderten Markup angezeigt werden kann. Betrachten Sie ein GridView-Steuerelement, das ein TemplateField-Steuerelement mit einem Label-websteuer Element mit einem `ID` Wert `ProductName`enthält. Wenn die GridView zur Laufzeit an die zugehörige Datenquelle gebunden ist, wird diese Bezeichnung für jede Zeile der GridView einmal wiederholt. Jede gerenderte Bezeichnung benötigt einen eindeutigen `id` Wert.

Um solche Szenarien zu behandeln, ermöglicht ASP.net, dass bestimmte Steuerelemente als Benennungs Container bezeichnet werden. Ein Benennungs Container dient als neuer `ID` Namespace. Für alle Server Steuerelemente, die im Benennungs Container angezeigt werden, wird der gerenderte `id` Wert dem `ID` des Naming Container-Steuer Elements vorangestellt. Beispielsweise sind die Klassen `GridView` und `GridViewRow` beide Container. Folglich erhält ein Label-Steuerelement, das in einem GridView-TemplateField mit `ID` `ProductName` definiert ist, einen gerenderten `id` Wert `GridViewID_GridViewRowID_ProductName`. Da *gridviewrowid* für jede GridView-Zeile eindeutig ist, sind die resultierenden `id` Werte eindeutig.

> [!NOTE]
> Die [`INamingContainer`-Schnittstelle](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) wird verwendet, um anzugeben, dass ein bestimmtes ASP.NET-Server Steuerelement als Benennungs Container fungieren soll. Die `INamingContainer`-Schnittstelle gibt keine Methoden aus, die vom Server Steuerelement implementiert werden müssen. Stattdessen wird Sie als Marker verwendet. Wenn beim Erzeugen des gerenderten Markups ein Steuerelement diese Schnittstelle implementiert, wird der `ID` Wert von der ASP.net-Engine automatisch dem gerenderten `id` Attribut Wert der nachfolgenden Werte vorangestellt. Dieser Prozess wird in Schritt 2 ausführlicher erläutert.

Beim Benennen von Containern wird nicht nur der gerenderte `id` Attribut Wert geändert, sondern auch die programmgesteuerte referenzierte Verwendung des Steuer Elements aus der Code Behind-Klasse der ASP.NET-Klasse beeinflusst. Die `FindControl("controlID")`-Methode wird häufig verwendet, um Programm gesteuert auf ein websteuer Element zu verweisen. `FindControl` werden jedoch nicht durch das Benennen von Containern durchdringen. Folglich können Sie die `Page.FindControl`-Methode nicht direkt verwenden, um auf Steuerelemente in einer GridView oder einem anderen namens Container zu verweisen.

Wie Sie vielleicht schon einmal gemacht haben, werden Masterseiten und Inhalts Platzhalter sowohl als Benennungs Container implementiert. In diesem Tutorial untersuchen wir, wie sich Masterseiten auf das HTML-Element `id` Werte und Möglichkeiten zum programmgesteuerten Verweis auf websteuer Elemente innerhalb einer Inhaltsseite mithilfe `FindControl`auswirken.

## <a name="step-1-adding-a-new-aspnet-page"></a>Schritt 1: Hinzufügen einer neuen ASP.NET-Seite

Um die in diesem Tutorial beschriebenen Konzepte zu veranschaulichen, fügen wir unserer Website eine neue ASP.NET-Seite hinzu. Erstellen Sie im Stamm Ordner eine neue Inhaltsseite mit dem Namen `IDIssues.aspx`, die Sie an die `Site.master` Master Seite bindet.

![Fügen Sie die Inhaltsseite "iabschreckes. aspx" dem Stamm Ordner hinzu.](control-id-naming-in-content-pages-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen der Inhaltsseite `IDIssues.aspx` zum Stamm Ordner

Visual Studio erstellt automatisch ein Inhalts Steuerelement für jeden der vier Inhalts Platzhalter der Master Seite. Wenn ein Inhalts Steuerelement nicht vorhanden ist, wird der Standard Inhalt von contentplachalter der Master Seite stattdessen ausgegeben, wie im Lernprogramm zu [*mehreren contentplatzhaltern und Standard Inhalten*](multiple-contentplaceholders-and-default-content-vb.md) angegeben. Da die `QuickLoginUI`-und `LeftColumnContent`-contentplatzhalter geeignetes Standard Markup für diese Seite enthalten, entfernen Sie die entsprechenden Inhalts Steuerelemente aus `IDIssues.aspx`. An diesem Punkt sollte das deklarative Markup der Inhaltsseite wie folgt aussehen:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

Im Tutorial [*angeben des Titels, der Meta-Tags und anderer HTML-Header im Lernprogramm für die Master Seite*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) haben wir eine benutzerdefinierte Basis Seiten Klasse (`BasePage`) erstellt, die den Titel der Seite automatisch konfiguriert, wenn Sie nicht explizit festgelegt ist. Damit die `IDIssues.aspx` Seite diese Funktion anwendet, muss die Code-Behind-Klasse der Seite von der `BasePage` Klasse abgeleitet werden (anstelle von `System.Web.UI.Page`). Ändern Sie die Definition der Code Behind-Klasse so, dass Sie wie folgt aussieht:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Aktualisieren Sie abschließend die `Web.sitemap` Datei, sodass Sie einen Eintrag für diese neue Lektion enthält. Fügen Sie ein `<siteMapNode>` Element hinzu, und legen Sie dessen `title`-und `url` Attribute auf "Benennungs Probleme bei der Steuerelement-ID" und `~/IDIssues.aspx`fest. Nachdem Sie diese Ergänzung vorgenommen haben, sollte das Markup der `Web.sitemap` Datei in etwa wie folgt aussehen:

[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Wie Abbildung 2 zeigt, wird der neue Site Übersichts Eintrag in `Web.sitemap` sofort im Abschnitt Lektionen in der linken Spalte angezeigt.

![Der Abschnitt "Lektionen" enthält jetzt einen Link zu &quot;Probleme beim Benennen von Steuerelement-IDs&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Abbildung 02**: der Abschnitt "Lektionen" enthält jetzt einen Link zu "Probleme beim Benennen der Steuerelement-ID".

## <a name="step-2-examining-the-renderedidchanges"></a>Schritt 2: Untersuchen der gerenderten`ID`Änderungen

Um die Änderungen zu verstehen, die die ASP.net-Engine an den gerenderten `id` Werten der Server Steuerelemente vornimmt, fügen wir der `IDIssues.aspx` Seite einige websteuer Elemente hinzu und zeigen dann das gerenderte Markup an, das an den Browser gesendet wird. Geben Sie insbesondere den Text "Bitte geben Sie Ihr Alter ein:" gefolgt von einem TextBox-websteuer Element ein. Im weiteren Verlauf der Seite fügen Sie ein Schaltflächen-websteuer Element und ein Label-websteuer Element hinzu. Legen Sie die Eigenschaften `ID` und `Columns` des Textfelds auf `Age` bzw. 3 fest. Legen Sie die Eigenschaften `Text` und `ID` der Schaltfläche auf "Submit" und `SubmitButton`fest. Löschen Sie die `Text`-Eigenschaft der Bezeichnung, und legen Sie deren `ID` auf `Results`fest.

An diesem Punkt sollte das deklarative Markup des Inhalts Steuer Elements in etwa wie folgt aussehen:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

Abbildung 3 zeigt die Seite, wenn Sie im Visual Studio-Designer angezeigt wird.

[![die Seite drei websteuer Elemente enthält: ein Textfeld, eine Schaltfläche und eine Bezeichnung.](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Abbildung 03**: die Seite enthält drei websteuer Elemente: ein Textfeld, eine Schaltfläche und eine Bezeichnung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](control-id-naming-in-content-pages-vb/_static/image5.png)).

Besuchen Sie die Seite über einen Browser, und zeigen Sie dann die HTML-Quelle an. Wie das folgende Markup zeigt, sind die `id` Werte der HTML-Elemente für das Textfeld, die Schaltfläche und die Beschriftungs-websteuer Elemente eine Kombination aus den `ID` Werten der websteuer Elemente und den `ID` Werten der Benennungs Container auf der Seite.

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Wie bereits weiter oben in diesem Tutorial erwähnt, fungieren sowohl die Master Seite als auch die zugehörigen Inhalts Platzhalter als Benennungs Container. Folglich tragen beide die gerenderten `ID` Werte der zugehörigen Steuerelemente ein. Nehmen Sie das `id`-Attribut des Textfelds zum Beispiel: `ctl00_MainContent_Age`. Erinnern Sie sich daran, dass der `ID` Wert des TextBox-Steuer Elements `Age`wurde. Der `ID` Wert des contentplachalter-Steuer Elements wird `MainContent`vorangestellt. Außerdem wird diesem Wert der `ID` Wert der Master Seite (`ctl00`) vorangestellt. Der Nettoeffekt ist ein `id` Attribut Wert, der aus den `ID` Werten der Master Seite, dem contentplachalter-Steuerelement und dem TextBox-Element besteht.

In Abbildung 4 wird dieses Verhalten veranschaulicht. Um die gerenderte `id` des `Age` Textfelds zu ermitteln, beginnen Sie mit dem `ID` Wert des TextBox-Steuer Elements, `Age`. Arbeiten Sie anschließend in der Steuerelement Hierarchie nach oben. Stellen Sie bei jedem Benennungs Container (diese Knoten mit einer Peach-Farbe) der aktuellen gerenderten `id` den `id`des Benennungs Containers voran.

![Die gerenderten ID-Attribute basieren auf den ID-Werten der Benennungs Container.](control-id-naming-in-content-pages-vb/_static/image6.png)

**Abbildung 04**: die gerenderten `id` Attribute basieren auf den `ID` Werten der Benennungs Container

> [!NOTE]
> Wie bereits erläutert, bildet der `ctl00` Teil des gerenderten `id` Attributs den `ID` Wert der Master Seite, aber Sie Fragen sich vielleicht, wie dieser `ID` Wert entstanden ist. Wir haben Sie nicht an einer beliebigen Stelle auf unserer Master-oder Inhaltsseite angegeben. Die meisten Server Steuerelemente in einer ASP.NET-Seite werden explizit über das deklarative Markup der Seite hinzugefügt. Das `MainContent` contentplachalter-Steuerelement wurde explizit im Markup `Site.master`angegeben; das Textfeld "`Age`" wurde `IDIssues.aspx`Markup definiert. Wir können die `ID` Werte für diese Steuerelement Typen über die Eigenschaftenfenster oder die deklarative Syntax angeben. Andere Steuerelemente, wie z. b. die Master Seite, werden nicht im deklarativen Markup definiert. Folglich müssen die `ID` Werte automatisch für uns generiert werden. Die ASP.net-Engine legt die `ID` Werte zur Laufzeit für die Steuerelemente fest, deren IDs nicht explizit festgelegt wurden. Dabei wird das Benennungs Muster `ctlXX`verwendet, wobei *xx* ein sequenziierendes ganzzahliger Wert ist.

Da die Master Seite selbst als Benennungs Container fungiert, haben die auf der Master Seite definierten websteuer Elemente auch gerenderte `id` Attributwerte geändert. Beispielsweise verfügt die `DisplayDate` Bezeichnung, die wir der Master Seite im Tutorial [*Erstellen eines Website weiten Layouts mit Masterseiten*](creating-a-site-wide-layout-using-master-pages-vb.md) hinzugefügt haben, über das folgende gerenderte Markup:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Beachten Sie, dass das `id`-Attribut sowohl den `ID` Wert der Master Seite (`ctl00`) als auch den `ID` Wert des Beschriftungs-websteuer Elements (`DateDisplay`) enthält.

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Schritt 3: Programm gesteuertes verweisen auf websteuer Elemente über`FindControl`

Jedes ASP.NET-Server Steuerelement enthält eine `FindControl("controlID")`-Methode, die die Nachfolger Elemente des Steuer Elements nach einem Steuerelement namens *ControlID*durchsucht. Wenn ein solches Steuerelement gefunden wird, wird es zurückgegeben. Wenn kein übereinstimmendes Steuerelement gefunden wird, gibt `FindControl` `Nothing`zurück.

`FindControl` ist in Szenarien nützlich, in denen Sie auf ein Steuerelement zugreifen müssen, aber keinen direkten Verweis darauf haben. Beim Arbeiten mit datenweb Steuerelementen wie z. b. dem GridView-Steuerelement werden z. b. die Steuerelemente in den Feldern der GridView einmal in der deklarativen Syntax definiert, aber zur Laufzeit wird eine Instanz des Steuer Elements für jede GridView-Zeile erstellt. Folglich sind die zur Laufzeit generierten Steuerelemente vorhanden, aber es ist kein direkter Verweis aus der Code Behind-Klasse verfügbar. Daher müssen wir `FindControl` verwenden, um Programm gesteuert mit einem bestimmten Steuerelement in den Feldern der GridView zu arbeiten. (Weitere Informationen zum Verwenden von `FindControl` für den Zugriff auf die Steuerelemente in den Vorlagen eines Daten websteuer Elements finden Sie unter [benutzerdefinierte Formatierung basierend auf Daten](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md).) Das gleiche Szenario tritt auf, wenn websteuer Elemente dynamisch einem Webformular hinzugefügt werden, einem Thema, das unter [Erstellen von dynamische Daten Eintrag-Benutzeroberflächen](https://msdn.microsoft.com/library/aa479330.aspx)erläutert wird.

Um die Verwendung der `FindControl`-Methode für die Suche nach Steuerelementen innerhalb einer Inhaltsseite zu veranschaulichen, erstellen Sie einen Ereignishandler für das `Click`-Ereignis des `SubmitButton`. Fügen Sie im-Ereignishandler den folgenden Code hinzu, der Programm gesteuert mithilfe der `FindControl`-Methode auf das Textfeld `Age` und `Results` Bezeichnung verweist. Anschließend wird eine Meldung in `Results` basierend auf der Eingabe des Benutzers angezeigt.

> [!NOTE]
> Natürlich müssen wir `FindControl` nicht verwenden, um auf die Bezeichnung und die Textfeld-Steuerelemente für dieses Beispiel zu verweisen. Wir könnten direkt über die `ID` Eigenschaftswerte auf Sie verweisen. Ich verwende `FindControl` hier, um zu veranschaulichen, was geschieht, wenn `FindControl` von einer Inhaltsseite verwendet wird.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Die Syntax, mit der die `FindControl` Methode aufgerufen wird, unterscheidet sich in den ersten beiden Zeilen `SubmitButton_Click`jedoch geringfügig. Beachten Sie, dass alle ASP.NET-Server Steuerelemente eine `FindControl`-Methode enthalten. Dies schließt die `Page`-Klasse ein, von der alle ASP.NET-Code-Behind-Klassen abgeleitet werden müssen. Daher ist das Aufrufen von `FindControl("controlID")` Äquivalent zum Aufrufen von `Page.FindControl("controlID")`, vorausgesetzt, Sie haben die `FindControl`-Methode in der Code Behind-Klasse oder in einer benutzerdefinierten Basisklasse nicht überschrieben.

Nachdem Sie diesen Code eingegeben haben, besuchen Sie die Seite `IDIssues.aspx` über einen Browser, geben Sie Ihr Alter ein, und klicken Sie auf die Schaltfläche "Senden". Wenn Sie auf die Schaltfläche "Senden" klicken, wird eine `NullReferenceException` ausgelöst (siehe Abbildung 5).

[![eine NullReferenceException ausgelöst wird.](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Abbildung 05**: ein `NullReferenceException` wird ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](control-id-naming-in-content-pages-vb/_static/image9.png))

Wenn Sie im `SubmitButton_Click` Ereignishandler einen Haltepunkt festlegen, sehen Sie, dass beide Aufrufe von `FindControl` `Nothing`zurückgeben. Der `NullReferenceException` wird beim Versuch, auf die `Text`-Eigenschaft des `Age` Textfelds zuzugreifen, ausgelöst.

Das Problem besteht darin, dass `Control.FindControl` die Nachfolger Elemente des *Steuer*Elements, die sich im gleichen Namens Container befinden, nur durchsucht. Da die Master Seite einen neuen Namens Container darstellt, führt ein-`Page.FindControl("controlID")` nie das `ctl00`der Master Seite durch. (Siehe Abbildung 4, um die Steuerelement Hierarchie anzuzeigen, in der das `Page` Objekt als übergeordnetes Element des Masterseiten Objekts `ctl00`angezeigt wird.) Daher werden die `Results` Bezeichnung und das `Age` Textfeld nicht gefunden, und `ResultsLabel` und `AgeTextBox` werden Werte `Nothing`zugewiesen.

Es gibt zwei Problem Umgehungen für diese Aufgabe: Wir können einen Drilldown für einen Benennungs Container gleichzeitig auf das entsprechende Steuerelement ausführen. Wir können auch eine eigene `FindControl` Methode erstellen, die die namens Container durchläuft. Betrachten wir jede dieser Optionen.

### <a name="drilling-into-the-appropriate-naming-container"></a>Drilldown in den entsprechenden Benennungs Container

Um `FindControl` zu verwenden, um auf die `Results` Bezeichnung oder `Age` Textfeld zu verweisen, müssen wir `FindControl` von einem Vorgänger Steuerelement im gleichen Namens Container aus abrufen. Wie in Abbildung 4 gezeigt, ist das `MainContent` contentplachalter-Steuerelement der einzige Vorgänger von `Results` oder `Age`, das sich im gleichen Namens Container befindet. Anders ausgedrückt: Wenn Sie die `FindControl`-Methode aus dem `MainContent`-Steuerelement aufrufen, wie im folgenden Code Ausschnitt gezeigt, wird ein Verweis auf die `Results`-oder `Age`-Steuerelemente korrekt zurückgegeben.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Wir können jedoch nicht mit der `MainContent` contentplachalter aus der Code Behind-Klasse unserer Inhaltsseite mit der obigen Syntax arbeiten, da der contentplachalter auf der Master Seite definiert ist. Stattdessen muss `FindControl` verwendet werden, um einen Verweis auf `MainContent`zu erhalten. Ersetzen Sie den Code im `SubmitButton_Click` Ereignishandler durch die folgenden Änderungen:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Wenn Sie die Seite über einen Browser aufrufen, geben Sie Ihr Alter ein, und klicken Sie auf die Schaltfläche "Submit" (senden), wenn ein `NullReferenceException` ausgelöst wird. Wenn Sie im `SubmitButton_Click` Ereignishandler einen Haltepunkt festlegen, werden Sie feststellen, dass diese Ausnahme auftritt, wenn Sie versuchen, die `FindControl`-Methode des `MainContent`-Objekts aufzurufen. Das `MainContent` Objekt ist gleich `Nothing`, da die `FindControl`-Methode kein Objekt mit dem Namen "mainContent" finden kann. Der zugrunde liegende Grund ist derselbe wie bei den `Results` Bezeichnung und `Age` TextBox-Steuerelementen: `FindControl` startet die Suche von oben nach unten in der Steuerelement Hierarchie und führt keine namens Container ein. der `MainContent` contentplachalter befindet sich jedoch auf der Master Seite, die ein namens Container ist.

Bevor wir `FindControl` verwenden können, um einen Verweis auf `MainContent`zu erhalten, benötigen wir zuerst einen Verweis auf das Steuerelement der Master Seite. Nachdem wir einen Verweis auf die Master Seite haben, können wir über `FindControl` auf den `MainContent` contentplachalter und, von dort aus Verweise auf die `Results` Bezeichnung und `Age` Textfeld (wiederum über `FindControl`) erhalten. Aber wie erhalte ich einen Verweis auf die Master Seite? Wenn die `id` Attribute im gerenderten Markup überprüft werden, ist es offensichtlich, dass der `ID` Wert der Master Seite `ctl00`ist. Daher können wir mit `Page.FindControl("ctl00")` einen Verweis auf die Master Seite erhalten und dann mit diesem Objekt einen Verweis auf `MainContent`erhalten usw. Der folgende Code Ausschnitt veranschaulicht diese Logik:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Obwohl dieser Code sicherlich funktioniert, wird davon ausgegangen, dass die automatisch generierten `ID` der Master Seite immer `ctl00`werden. Es ist nie eine gute Idee, Annahmen über automatisch generierte Werte zu treffen.

Glücklicherweise ist ein Verweis auf die Master Seite über die `Master`-Eigenschaft der `Page` Klasse zugänglich. Daher ist es nicht erforderlich, `FindControl("ctl00")` zu verwenden, um einen Verweis auf die Master Seite zu erhalten, um auf den `MainContent` contentplachalter zuzugreifen. stattdessen können wir `Page.Master.FindControl("MainContent")`verwenden. Aktualisieren Sie den `SubmitButton_Click`-Ereignishandler mit folgendem Code:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Wenn Sie die Seite in einem Browser aufrufen, das Alter eingeben und auf die Schaltfläche "Senden" klicken, wird die Meldung wie erwartet in der `Results`-Bezeichnung angezeigt.

[![das Alter des Benutzers in der Bezeichnung angezeigt wird.](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Abbildung 06**: das Alter des Benutzers wird in der Bezeichnung angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](control-id-naming-in-content-pages-vb/_static/image12.png))

### <a name="recursively-searching-through-naming-containers"></a>Rekursiv Durchsuchen von Benennungs Containern

Der Grund, aus dem das vorherige Codebeispiel auf das `MainContent` contentplachalter-Steuerelement von der Master Seite verwiesen hat, und dann auf die `Results` Bezeichnung und `Age` Textfeld-Steuerelemente aus `MainContent`, liegt daran, dass die `Control.FindControl`-Methode nur den Benennungs Container des *Steuer*Elements Wenn `FindControl` im Benennungs Container bleiben, ist es in den meisten Szenarien sinnvoll, da zwei Steuerelemente in zwei verschiedenen Benennungs Containern die gleichen `ID` Werte aufweisen können. Betrachten Sie die Groß-/Kleinschreibung eines GridView-Objekts, das ein Bezeichnungs-websteuer Element namens `ProductName` innerhalb eines seiner templatefields definiert. Wenn die Daten zur Laufzeit an die GridView gebunden werden, wird für jede Zeile der GridView eine `ProductName` Bezeichnung erstellt. Wenn `FindControl`, die alle Benennungs Container durchsucht haben und `Page.FindControl("ProductName")`aufgerufen haben, sollte die Bezeichnungs Instanz die `FindControl` zurückgeben? Die `ProductName` Bezeichnung in der ersten GridView-Zeile? Der in der letzten Zeile?

Daher ist es in den meisten Fällen sinnvoll, den Benennungs Container für die `Control.FindControl` Suche zu *Steuern*. Es gibt aber auch andere Fälle, wie z. b. das, was wir tun, wenn wir eine eindeutige `ID` für alle Benennungs Container haben und vermeiden möchten, dass Sie auf jeden Benennungs Container in der Steuerelement Hierarchie genau verweisen, um auf ein Steuerelement zuzugreifen. Eine `FindControl` Variante, die rekursiv alle Benennungs Container durchsucht, ist ebenfalls sinnvoll. Leider enthält die .NET Framework keine solche Methode.

Die gute Nachricht ist, dass wir unsere eigene `FindControl`-Methode erstellen können, die rekursiv alle Benennungs Container durchsucht. Tatsächlich können Sie mithilfe von *Erweiterungs Methoden* eine `FindControlRecursive` Methode an die `Control` Klasse weiter verwenden, um die vorhandene `FindControl` Methode zu begleiten.

> [!NOTE]
> Erweiterungs Methoden sind ein neues Feature in C# 3,0 und Visual Basic 9, wobei es sich um die Sprachen handelt, die im Lieferumfang von .NET Framework Version 3,5 und Visual Studio 2008 enthalten sind. Kurz gesagt ermöglichen Erweiterungs Methoden einem Entwickler das Erstellen einer neuen Methode für einen vorhandenen Klassentyp über eine spezielle Syntax. Weitere Informationen zu dieser hilfreichen Funktion finden Sie im Artikel [Erweitern der Basistyps-Funktionalität mit Erweiterungs Methoden](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).

Fügen Sie zum Erstellen der Erweiterungsmethode dem Ordner "`App_Code`" eine neue Datei mit dem Namen `PageExtensionMethods.vb`hinzu. Fügen Sie eine Erweiterungsmethode mit dem Namen `FindControlRecursive` hinzu, die als Eingabe einen `String` Parameter mit dem Namen `controlID`annimmt. Damit Erweiterungs Methoden ordnungsgemäß funktionieren, ist es wichtig, dass die-Klasse als `Module` gekennzeichnet ist und dass den Erweiterungs Methoden das `<Extension()>`-Attribut vorangestellt wird. Darüber hinaus müssen alle Erweiterungs Methoden als ersten Parameter ein Objekt des Typs akzeptieren, auf den die Erweiterungsmethode angewendet wird.

Fügen Sie den folgenden Code in die `PageExtensionMethods.vb` Datei ein, um diese `Module` und die `FindControlRecursive` Erweiterungsmethode zu definieren:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Wenn dieser Code vorhanden ist, kehren Sie zur Code Behind-Klasse der `IDIssues.aspx` Seite zurück, und kommentieren Sie die aktuellen `FindControl` Methodenaufrufe aus. Ersetzen Sie Sie durch Aufrufe von `Page.FindControlRecursive("controlID")`. Die Erweiterungen der Erweiterungs Methoden sind, dass Sie direkt in den IntelliSense-Dropdown Listen angezeigt werden. Wie in Abbildung 7 gezeigt, ist die `FindControlRecursive`-Methode bei der Typ`Page` und der Treffer Zeit in der IntelliSense-Dropdown Liste zusammen mit den anderen `Control`-Klassen Methoden enthalten.

[![Erweiterungs Methoden sind in den IntelliSense-Dropdown-Vorgängen enthalten.](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Abbildung 07**: Erweiterungs Methoden sind in den IntelliSense-Dropdowns enthalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](control-id-naming-in-content-pages-vb/_static/image15.png))

Geben Sie den folgenden Code in den `SubmitButton_Click`-Ereignishandler ein, und testen Sie ihn, indem Sie die Seite aufrufen, Ihr Alter eingeben und auf die Schaltfläche "übermitteln" klicken. Wie bereits in Abbildung 6 gezeigt, ist die resultierende Ausgabe die Meldung "Sie sind älter als alt!".

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Da Erweiterungs Methoden neu in 3,0 C# und Visual Basic 9 sind, können Sie bei Verwendung von Visual Studio 2005 keine Erweiterungs Methoden verwenden. Stattdessen müssen Sie die `FindControlRecursive`-Methode in einer Hilfsklasse implementieren. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) hat ein Beispiel in seinem Blogbeitrag [ASP.net Maser Pages und `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx).

## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Schritt 4: Verwenden des korrekten`id`-Attribut Werts im Client seitigen Skript

Wie in der Einführung dieses Tutorials erwähnt, wird das gerenderte `id`-Attribut eines websteuer Elements oft im Client seitigen Skript verwendet, um Programm gesteuert auf ein bestimmtes HTML-Element zu verweisen. Das folgende JavaScript verweist z. b. auf ein HTML-Element, indem es `id` und dann seinen Wert in einem modalen Meldungs Feld anzeigt:

[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Beachten Sie, dass das `id`-Attribut des gerenderten HTML-Elements in ASP.NET Seiten, die keinen Benennungs Container enthalten, mit dem `ID`-Eigenschafts Wert des websteuer Elements identisch ist. Aus diesem Grund ist es verlockend, in `id` Attributwerten in JavaScript-Code zu schreiben. Das heißt, wenn Sie wissen, dass Sie über ein Client seitiges Skript auf das `Age` TextBox-websteuer Element zugreifen möchten, verwenden Sie dazu einen `document.getElementById("Age")`.

Das Problem bei diesem Ansatz besteht darin, dass bei Verwendung von Masterseiten (oder anderen Benennungs Container-Steuerelementen) der gerenderte HTML-`id` nicht mit der `ID`-Eigenschaft des websteuer Elements Synonym ist. Ihre erste Neigung besteht darin, die Seite über einen Browser zu besuchen und die Quelle anzuzeigen, um das tatsächliche `id` Attribut zu bestimmen. Wenn Sie den gerenderten `id` Wert kennen, können Sie ihn in den `getElementById` aufrufen, um auf das HTML-Element zuzugreifen, mit dem Sie über ein Client seitiges Skript arbeiten müssen. Diese Vorgehensweise ist kleiner als ideal, da bestimmte Änderungen an der Steuerelement Hierarchie der Seite oder Änderungen an den `ID` Eigenschaften der Benennungs Steuerelemente das resultierende `id` Attribut ändern. Dadurch wird der JavaScript-Code unterbrochen.

Die gute Nachricht ist, dass der `id` Attribut Wert, der gerendert wird, in Server seitigem Code über die [`ClientID`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)des websteuer Elements zugänglich ist. Sie sollten diese Eigenschaft verwenden, um den `id`-Attribut Wert zu bestimmen, der im Client seitigen Skript verwendet wird. Um z. b. der Seite eine JavaScript-Funktion hinzuzufügen, die den Wert des `Age` Textfelds in einem modalen Meldungs Feld anzeigt, fügen Sie den folgenden Code zum `Page_Load`-Ereignishandler hinzu:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

Der obige Code fügt den Wert der `ClientID`-Eigenschaft des `Age` TextBox in den JavaScript-Befehl `getElementById`ein. Wenn Sie diese Seite über einen Browser besuchen und die HTML-Quelle anzeigen, finden Sie den folgenden JavaScript-Code:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Beachten Sie, dass der richtige `id`-Attribut Wert, `ctl00_MainContent_Age`, innerhalb des Aufrufes `getElementById`angezeigt wird. Da dieser Wert zur Laufzeit berechnet wird, funktioniert er unabhängig von späteren Änderungen an der Seiten Steuerelement Hierarchie.

> [!NOTE]
> Dieses JavaScript-Beispiel zeigt lediglich, wie eine JavaScript-Funktion hinzugefügt wird, die korrekt auf das von einem Server Steuerelement gerenderte HTML-Element verweist Um diese Funktion verwenden zu können, müssen Sie zusätzliches JavaScript erstellen, um die Funktion aufzurufen, wenn das Dokument geladen wird oder wenn eine bestimmte Benutzeraktion auftritt. Weitere Informationen zu diesen und verwandten Themen finden Sie unter [Arbeiten mit Client seitigem Skript](https://msdn.microsoft.com/library/aa479302.aspx).

## <a name="summary"></a>Summary

Bestimmte ASP.NET-Server Steuerelemente fungieren als Benennungs Container. Dies wirkt sich auf die gerenderten `id` Attributwerte ihrer Nachfolger Steuerelemente sowie auf den Bereich von Steuerelementen aus, die von der `FindControl`-Methode verwendet werden. Im Hinblick auf Masterseiten werden von der Master Seite selbst und ihren contentplachalter-Steuerelementen Container benannt. Folglich müssen wir etwas mehr Arbeit zum programmgesteuerten Verweis auf Steuerelemente auf der Inhaltsseite mit `FindControl`platzieren. In diesem Tutorial haben wir zwei Techniken untersucht: ein Drilldown in das contentplachalter-Steuerelement und das Aufrufen seiner `FindControl` Methode. und ein Rollback der eigenen `FindControl` Implementierung, die rekursiv alle Benennungs Container durchsucht.

Zusätzlich zu den serverseitigen Problemen, die die Benennung von Containern in Bezug auf das verweisen auf websteuer Elemente einführen, gibt es auch Client seitige Probleme. Wenn keine namens Container vorhanden sind, sind der `ID` Eigenschafts Wert des websteuer Elements und der gerenderte `id` Attribut Wert gleich. Beim Hinzufügen von "Naming Container" enthält das gerenderte `id` Attribut jedoch sowohl die `ID` Werte des websteuer Elements als auch die namens Container in der Herkunft der Steuerelement Hierarchie. Diese Benennungs Probleme sind ein nicht Problem, solange Sie die `ClientID`-Eigenschaft des websteuer Elements verwenden, um den gerenderten `id` Attribut Wert in Ihrem Client seitigen Skript zu ermitteln.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net Master Seiten und `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Erstellen von Benutzeroberflächen für den dynamische Daten Eintrag](https://msdn.microsoft.com/library/aa479330.aspx)
- [Erweitern der Basistyp Funktionalität mit Erweiterungs Methoden](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Gewusst wie: verweisen auf ASP.net-Master Seiten Inhalt](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Mater Pages: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [Arbeiten mit Client seitigem Skript](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial waren Zack Jones und Suchi barnerjee. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](urls-in-master-pages-vb.md)
> [Weiter](interacting-with-the-master-page-from-the-content-page-vb.md)
