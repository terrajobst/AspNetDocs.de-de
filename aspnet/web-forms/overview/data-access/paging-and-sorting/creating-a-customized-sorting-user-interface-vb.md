---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Erstellen einer benutzerdefinierten Sortierungs Benutzeroberfläche (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Beim Anzeigen einer langen Liste sortierter Daten kann es sehr hilfreich sein, verwandte Daten durch die Einführung von Trennzeichen zu gruppieren. In diesem Tutorial erfahren Sie, wie Sie CRE...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 66127630560141cd795beb15f525a7fba85f3993
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607368"
---
# <a name="creating-a-customized-sorting-user-interface-vb"></a>Erstellen einer angepassten Benutzeroberfläche zum Sortieren (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) oder [PDF herunterladen](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Beim Anzeigen einer langen Liste sortierter Daten kann es sehr hilfreich sein, verwandte Daten durch die Einführung von Trennzeichen zu gruppieren. In diesem Tutorial erfahren Sie, wie Sie eine solche sortierende Benutzeroberfläche erstellen.

## <a name="introduction"></a>Einführung

Beim Anzeigen einer langen Liste von sortierten Daten, bei denen es nur wenige unterschiedliche Werte in der sortierten Spalte gibt, kann ein Endbenutzer feststellen, wo genau die Differenz Grenzen eintreten. Beispielsweise gibt es 81 Produkte in der-Datenbank, aber nur neun verschiedene Kategorieoptionen (acht eindeutige Kategorien plus die `NULL`-Option). Betrachten Sie den Fall eines Benutzers, der an der Untersuchung der Produkte interessiert ist, die in der Kategorie "Seafood" liegen. Auf einer Seite, auf der *alle* Produkte in einer einzelnen GridView aufgelistet sind, kann der Benutzer entscheiden, ob die Ergebnisse nach Kategorie sortiert werden sollen, in der alle Produkte der Benutzer Produkte gruppiert werden. Nach der Sortierung nach Kategorie muss der Benutzer dann die Liste durchsuchen, um zu suchen, wo die in der Benutzerliste gruppierten Produkte beginnen und enden. Da die Ergebnisse alphabetisch nach dem Kategorienamen sortiert werden, ist die Suche nach den Produkten der-Produkte nicht schwierig, erfordert aber trotzdem eine genaue Überprüfung der Liste der Elemente im Raster.

Um die Grenzen zwischen sortierten Gruppen hervorzuheben, verwenden viele Websites eine Benutzeroberfläche, die ein Trennzeichen zwischen diesen Gruppen hinzufügt. Mithilfe von Trennzeichen wie den in Abbildung 1 gezeigten können Benutzer eine bestimmte Gruppe schneller finden und ihre Grenzen identifizieren und ermitteln, welche unterschiedlichen Gruppen in den Daten vorhanden sind.

[![werden alle Kategoriegruppen eindeutig identifiziert.](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Abbildung 1**: jede Kategoriegruppe ist eindeutig identifiziert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-customized-sorting-user-interface-vb/_static/image3.png))

In diesem Tutorial erfahren Sie, wie Sie eine solche sortierende Benutzeroberfläche erstellen.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Schritt 1: Erstellen eines Standard mäßigen, sortierbaren GridView-Sicht

Bevor wir untersuchen, wie die GridView erweitert wird, um die erweiterte Sortier Schnittstelle bereitzustellen, können Sie zuerst eine standardmäßige, sortierbare GridView erstellen, die die Produkte auflistet. Öffnen Sie zunächst die Seite `CustomSortingUI.aspx` im Ordner `PagingAndSorting`. Fügen Sie der Seite eine GridView hinzu, legen Sie die `ID`-Eigenschaft auf `ProductList`fest, und binden Sie Sie an eine neue ObjectDataSource. Konfigurieren Sie ObjectDataSource so, dass die `ProductsBLL` Klasse s `GetProducts()` Methode zum Auswählen von Datensätzen verwendet wird.

Als Nächstes konfigurieren Sie die GridView so, dass Sie nur die `ProductName`, `CategoryName`, `SupplierName`und `UnitPrice` boundfields und das nicht mehr unterstützte CheckBoxField enthält. Konfigurieren Sie abschließend die GridView, um die Sortierung zu unterstützen, indem Sie das Kontrollkästchen Sortierung aktivieren im GridView-Smarttag aktivieren (oder indem Sie die `AllowSorting`-Eigenschaft auf `true`festlegen). Nachdem Sie diese Ergänzungen auf der Seite `CustomSortingUI.aspx` vorgenommen haben, sollte das deklarative Markup in etwa wie folgt aussehen:

[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um den Fortschritt in einem Browser anzuzeigen. Abbildung 2 zeigt die sortierbare GridView, wenn Ihre Daten nach Kategorie in alphabetischer Reihenfolge sortiert werden.

[![die sortierbaren GridView s-Daten nach Kategorie sortiert](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Abbildung 2**: die sortierbaren GridView s-Daten werden nach Kategorie sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-customized-sorting-user-interface-vb/_static/image6.png))

## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Schritt 2: Untersuchen von Verfahren zum Hinzufügen der Trenn Zeilen

Wenn die generische, sortierbare GridView vollständig ist, muss nur noch die Trenn Zeilen in der GridView vor jeder eindeutigen sortierten Gruppe hinzugefügt werden können. Aber wie können solche Zeilen in die GridView-Struktur eingefügt werden? Im Wesentlichen müssen die GridView s-Zeilen durchlaufen werden, und es wird festgelegt, wo die Unterschiede zwischen den Werten in der sortierten Spalte auftreten, und anschließend wird die entsprechende Trennlinie hinzugefügt. Wenn Sie dieses Problem betrachten, scheint es natürlich, dass die Projekt Mappe irgendwo im `RowDataBound` Ereignishandler von GridView s liegt. Wie im Tutorial " [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) " erläutert, wird dieser Ereignishandler häufig verwendet, wenn die Formatierung auf Zeilenebene auf Grundlage der Daten der Zeile angewendet wird. Der `RowDataBound` Ereignishandler ist hier jedoch nicht die Projekt Mappe, da Zeilen von diesem Ereignishandler nicht Programm gesteuert zur GridView hinzugefügt werden können. Die GridView s-`Rows` Sammlung ist tatsächlich schreibgeschützt.

Wenn Sie der GridView zusätzliche Zeilen hinzufügen möchten, stehen Ihnen drei Optionen zur Auswahl:

- Fügen Sie diese metadatentrennzeilen den eigentlichen Daten hinzu, die an die GridView gebunden sind.
- Nachdem die GridView an die Daten gebunden wurde, fügen Sie der GridView s-Steuerelement Sammlung zusätzliche `TableRow` Instanzen hinzu.
- Erstellen Sie ein benutzerdefiniertes Server Steuerelement, das das GridView-Steuerelement erweitert und die Methoden überschreibt, die für das Erstellen der GridView-Struktur

Das Erstellen eines benutzerdefinierten Server Steuer Elements ist der beste Ansatz, wenn diese Funktionalität auf vielen Webseiten oder über mehrere Websites benötigt wird. Es würde jedoch etwas Code und eine gründliche Untersuchung in die Tiefe der internen GridView s-Funktionen umfassen. Daher wird diese Option für dieses Tutorial nicht berücksichtigt.

Die anderen beiden Optionen fügen Trenn Zeilen zu den eigentlichen Daten hinzu, die an die GridView gebunden werden, und zum Bearbeiten der GridView s-Steuerelement Auflistung nach dem gebundenen Angriff auf das Problem und eine Erörterung.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Hinzufügen von Zeilen zu den Daten, die an die GridView gebunden sind

Wenn die GridView an eine Datenquelle gebunden ist, erstellt Sie eine `GridViewRow` für jeden Datensatz, der von der Datenquelle zurückgegeben wird. Aus diesem Grund können wir die erforderlichen Trenn Zeilen einfügen, indem Sie der Datenquelle Trennzeichen Sätze hinzufügen, bevor Sie an die GridView gebunden werden. In Abbildung 3 wird dieses Konzept veranschaulicht.

![Eine Technik umfasst das Hinzufügen von Trenn Zeilen zur Datenquelle.](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Abbildung 3**: eine Technik umfasst das Hinzufügen von Trenn Zeilen zur Datenquelle.

Ich verwende den Begriff Trennzeichen Sätze in Anführungszeichen, weil kein spezieller Trennzeichen Daten Satz vorhanden ist. Stattdessen müssen wir auf irgendeine Weise darauf achten, dass ein bestimmter Datensatz in der Datenquelle als Trennzeichen und nicht als normale Daten Zeile fungiert. In unseren Beispielen wird eine `ProductsDataTable` Instanz erneut an die GridView-Instanz gebunden, die aus `ProductRows`besteht. Wir könnten einen Datensatz als Trennlinie markieren, indem wir dessen `CategoryID`-Eigenschaft auf `-1` festlegen (da ein solcher Wert nicht normal vorhanden sein konnte).

Um dieses Verfahren zu verwenden, müssen wir die folgenden Schritte ausführen:

1. Programm gesteuertes Abrufen der Daten für die Bindung an die GridView-Instanz (eine `ProductsDataTable` Instanz)
2. Sortieren der Daten auf der Grundlage der Eigenschaften von GridView s `SortExpression` und `SortDirection`
3. Durchlaufen Sie die `ProductsRows` im `ProductsDataTable`, und suchen Sie nach dem Ort, an dem die Unterschiede in der sortierten Spalte liegen.
4. Fügen Sie an jeder Gruppen Grenze einen Trennzeichen Daten Satz `ProductsRow`-Instanz in die Datentabelle ein, bei der es sich um einen `CategoryID` auf `-1` festgelegt hat
5. Binden Sie die Daten nach dem Einfügen der Trenn Zeilen Programm gesteuert an die GridView.

Zusätzlich zu diesen fünf Schritten muss auch ein Ereignishandler für das GridView s `RowDataBound`-Ereignis bereitgestellt werden. An dieser Stelle werden alle `DataRow` überprüft und ermittelt, ob es sich um eine Trennlinie handelt, deren `CategoryID` Einstellung `-1`wurde. Wenn dies der Fall ist, möchten wir wahrscheinlich die Formatierung oder den Text anpassen, der in den Zellen angezeigt wird.

Die Verwendung dieser Technik zum Einfügen der Sortierungs Gruppen Grenzen erfordert etwas mehr Arbeit als oben beschrieben, da Sie auch einen Ereignishandler für das GridView s `Sorting`-Ereignis bereitstellen und die `SortExpression`-und `SortDirection`-Werte nachverfolgen müssen.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Bearbeiten der GridView s-Steuerelement Sammlung, nachdem Sie Daten gebunden war

Anstatt die Daten vor der Bindung an die GridView zu überprüfen, können wir die Trennzeichen Zeilen hinzufügen, *nachdem* die Daten an die GridView gebunden wurden. Der Prozess der Datenbindung erstellt die GridView s-Steuerelement Hierarchie, bei der es sich in Wirklichkeit einfach um eine `Table` Instanz handelt, die aus einer Auflistung von Zeilen besteht, von denen jede aus einer Auflistung von Zellen besteht. Die GridView s-Steuerelement Auflistung enthält insbesondere ein `Table` Objekt im Stammverzeichnis, ein `GridViewRow` (das von der `TableRow`-Klasse abgeleitet wird) für jeden Datensatz in der `DataSource`, der an die GridView gebunden ist, und ein `TableCell` Objekt in jeder `GridViewRow` Instanz für jedes Datenfeld in der `DataSource`.

Um zwischen den einzelnen Sortiergruppen Trenn Zeilen hinzuzufügen, können wir diese Steuerelement Hierarchie nach der Erstellung direkt bearbeiten. Wir können sicher sein, dass die GridView s-Steuerelement Hierarchie zum letzten Mal erstellt wurde, bis die Seite gerendert wird. Daher überschreibt dieser Ansatz die `Page` Class s `Render`-Methode. zu diesem Zeitpunkt wird die endgültige Steuerelement Hierarchie der GridView s so aktualisiert, dass die erforderlichen Trenn Zeilen enthalten sind. Dieses Verfahren wird in Abbildung 4 veranschaulicht.

[![eine Alternative Technik die GridView s-Steuerelement Hierarchie manipuliert](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Abbildung 4**: eine Alternative Technik bearbeitet die GridView s-Steuerelement Hierarchie ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-customized-sorting-user-interface-vb/_static/image10.png))

In diesem Tutorial verwenden wir diesen letzteren Ansatz, um die Sortierfunktion für Benutzer anzupassen.

> [!NOTE]
> Der Code, den Sie in diesem Tutorial vorstellen, basiert auf dem Beispiel im Blogeintrag [Teemu-Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s, das [etwas mit der GridView-Sortier Gruppierung spielt](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).

## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Schritt 3: Hinzufügen der Trenn Zeilen zur GridView s-Steuerelement Hierarchie

Da wir nur die Trenn Zeilen der GridView s-Steuerelement Hierarchie hinzufügen möchten, nachdem die Steuerelement Hierarchie erstellt und zum letzten Mal auf dieser Seite erstellt wurde, möchten wir diese Addition am Ende des Lebenszyklus der Seite, aber vor der tatsächlichen GridView c- die Benutzerkontensteuerung-Hierarchie wurde in HTML gerendert. Der letzte mögliche Punkt, an dem dies erreicht werden kann, ist das `Page` Class-`Render` Ereignis, das wir in unserer Code-Behind-Klasse mithilfe der folgenden Methoden Signatur überschreiben können:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Wenn die ursprüngliche `Render`-Methode der `Page` Klasse aufgerufen wird `base.Render(writer)` werden alle Steuerelemente auf der Seite gerendert, wodurch das Markup basierend auf Ihrer Steuerelement Hierarchie erzeugt wird. Daher ist es zwingend erforderlich, dass wir `base.Render(writer)`aufrufen, sodass die Seite gerendert wird, und dass wir die Hierarchie der GridView s-Steuerelemente vor dem Aufrufen von `base.Render(writer)`bearbeiten, sodass die Trennzeichen Zeilen der GridView s-Steuerelement Hierarchie hinzugefügt wurden, bevor Sie gerendert wurden.

Um die Sortierungs Gruppen Header einzufügen, müssen Sie zuerst sicherstellen, dass der Benutzer die Sortierung der Daten angefordert hat. Standardmäßig sind die Inhalte der GridView-Inhalte nicht sortiert, daher müssen wir keine Gruppen Sortierungs Header eingeben.

> [!NOTE]
> Wenn die GridView nach einer bestimmten Spalte sortiert werden soll, wenn die Seite zum ersten Mal geladen wird, rufen Sie die GridView s `Sort`-Methode auf dem ersten Seitenbesuch auf (aber nicht bei nachfolgenden Postbacks). Fügen Sie zu diesem Zweck diesen-Befehl im `Page_Load`-Ereignishandler innerhalb einer `if (!Page.IsPostBack)` bedingten ein. Weitere Informationen zur `Sort`-Methode finden Sie im Tutorial zum [Paging und zum Sortieren von Berichtsdaten](paging-and-sorting-report-data-vb.md) .

Wenn Sie davon ausgehen, dass die Daten sortiert wurden, besteht die nächste Aufgabe darin, zu bestimmen, nach welcher Spalte die Daten sortiert wurden, und dann die Zeilen zu scannen, die auf Unterschiede in diesen Spaltenwerten suchen. Der folgende Code stellt sicher, dass die Daten sortiert wurden, und sucht die Spalte, nach der die Daten sortiert wurden:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Wenn das GridView-Objekt noch sortiert werden kann, wurde die GridView s-`SortExpression`-Eigenschaft nicht festgelegt. Daher möchten wir nur die Trennzeichen Zeilen hinzufügen, wenn diese Eigenschaft einen Wert aufweist. Wenn dies der Fall ist, müssen wir den Index der Spalte festlegen, nach der die Daten sortiert wurden. Dies wird erreicht, indem die GridView s-`Columns` Auflistung durchlaufen wird. dabei wird nach der Spalte gesucht, deren `SortExpression`-Eigenschaft der `SortExpression` Eigenschaft GridView s entspricht. Zusätzlich zum Spalten Index wird auch die `HeaderText`-Eigenschaft verwendet, die beim Anzeigen der Trennzeichen Zeilen verwendet wird.

Mit dem Index der Spalte, nach der die Daten sortiert werden, besteht der letzte Schritt darin, die Zeilen der GridView aufzulisten. Für jede Zeile muss bestimmt werden, ob sich der Wert der sortierten Spalte von dem Wert der Spalte in der vorherigen Zeile in der Spalte unterscheidet. Wenn dies der Fall ist, müssen wir eine neue `GridViewRow` Instanz in die Steuerelement Hierarchie einfügen. Dies wird mit folgendem Code erreicht:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Dieser Code beginnt mit einem programmgesteuerten Verweis auf das `Table` Objekt, das sich im Stammverzeichnis der GridView s-Steuerelement Hierarchie befindet, und erstellt eine Zeichen folgen Variable mit dem Namen `lastValue`. `lastValue` wird verwendet, um den Wert der aktuellen Zeilen-sortierten Spalte mit dem Wert der vorherigen Zeile zu vergleichen. Als nächstes wird die GridView s-`Rows` Collection aufgelistet, und für jede Zeile wird der Wert der sortierten Spalte in der `currentValue`-Variablen gespeichert.

> [!NOTE]
> Um den Wert der sortierten Spalte der Spalte zu bestimmen, verwende ich die Cell s `Text`-Eigenschaft. Dies funktioniert gut für boundfields, funktioniert jedoch nicht wie gewünscht für templatefields, checkboxfields usw. Wir sehen uns an, wie Sie die alternativen GridView-Felder in Kürze berücksichtigen.

Anschließend werden die Variablen `currentValue` und `lastValue` verglichen. Wenn Sie sich unterscheiden, müssen wir der Steuerelement Hierarchie eine neue Trenn Zeile hinzufügen. Dies wird erreicht, indem der Index der `GridViewRow` in der `Table`-Objekt-`Rows`-Auflistung ermittelt wird, neue `GridViewRow`-und `TableCell` Instanzen erstellt werden und dann die `TableCell` und `GridViewRow` der Steuerelement Hierarchie hinzugefügt werden.

Beachten Sie, dass die einzeilige `TableCell` der Zeilen Trennlinie so formatiert ist, dass Sie die gesamte Breite der GridView umfasst, mit der `SortHeaderRowStyle` CSS-Klasse formatiert wird und die `Text`-Eigenschaft aufweist, sodass Sie sowohl den Namen der Sortier Gruppe (z. b. Kategorie) als auch den Wert der Gruppe (z. b. Getränke) anzeigt. Schließlich wird `lastValue` auf den Wert `currentValue`aktualisiert.

Die CSS-Klasse, die zum Formatieren der Kopfzeilen `SortHeaderRowStyle` der Sortier Gruppe verwendet wird, muss in der `Styles.css` Datei angegeben werden. Sie können jederzeit beliebige Stileinstellungen verwenden, Ich habe Folgendes verwendet:

[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Mit dem aktuellen Code fügt die Sortierungs Schnittstelle Sortiergruppen Kopfzeilen hinzu, wenn Sie nach beliebigen BoundField sortiert werden (siehe Abbildung 5, das beim Sortieren nach Lieferanten einen Screenshot anzeigt). Beim Sortieren nach einem beliebigen anderen Feldtyp (z. b. CheckBoxField oder TemplateField) werden die Header der Sortier Gruppe jedoch nicht gefunden (siehe Abbildung 6).

[![die Sortierungs Schnittstelle Sortiergruppen Kopfzeilen beim Sortieren nach boundfields enthält.](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Abbildung 5**: die Sortierungs Schnittstelle enthält Sortiergruppen Kopfzeilen beim Sortieren nach boundfields ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-customized-sorting-user-interface-vb/_static/image13.png))

[beim Sortieren eines CheckBoxField fehlen ![die Sortiergruppen Header.](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Abbildung 6**: die Header der Sortier Gruppe fehlen beim Sortieren eines CheckBoxField ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-customized-sorting-user-interface-vb/_static/image16.png))

Der Grund für das Sortieren von Sortierungs Gruppen Headern in einem CheckBoxField-Objekt liegt darin, dass der Code derzeit nur die `TableCell` s `Text`-Eigenschaft verwendet, um den Wert der sortierten Spalte für jede Zeile zu bestimmen. Bei checkboxfields ist die `TableCell` s `Text`-Eigenschaft eine leere Zeichenfolge. Stattdessen ist der Wert über ein CheckBox-websteuer Element verfügbar, das sich innerhalb der `TableCell` s `Controls` Auflistung befindet.

Um andere Feldtypen als boundfields zu verarbeiten, müssen wir den Code erweitern, in dem die `currentValue` Variable zum Überprüfen des Vorhandenseins eines Kontrollkästchens in der `Controls` Auflistung der `TableCell` s zugewiesen wird. Anstatt `currentValue = gvr.Cells(sortColumnIndex).Text`zu verwenden, ersetzen Sie diesen Code durch Folgendes:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Dieser Code untersucht die sortierte Spalte `TableCell` für die aktuelle Zeile, um zu bestimmen, ob in der `Controls` Auflistung Steuerelemente vorhanden sind. Wenn dies der Fall ist und das erste Steuerelement ein Kontrollkästchen ist, wird die `currentValue` Variable abhängig von der Eigenschaft `Checked` Eigenschaft auf Ja oder Nein festgelegt. Andernfalls wird der Wert aus der `TableCell` s-`Text` Eigenschaft entnommen. Diese Logik kann repliziert werden, um die Sortierung für alle templatefields-Felder zu verarbeiten, die in der GridView vorhanden sein können.

Mit dem obigen Code Additions sind die Sortierungs Gruppen Header nun beim Sortieren nach dem nicht mehr unterstützten CheckBoxField vorhanden (siehe Abbildung 7).

[![die Sortierungs Gruppen Header jetzt beim Sortieren eines CheckBoxField vorhanden sind.](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Abbildung 7**: die Header der Sortier Gruppe sind nun beim Sortieren eines CheckBoxField vorhanden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-customized-sorting-user-interface-vb/_static/image19.png))

> [!NOTE]
> Wenn Sie über Produkte mit `NULL` Daten bankwerten für die Felder "`CategoryID`", "`SupplierID`" oder "`UnitPrice`" verfügen, werden diese Werte standardmäßig als leere Zeichen folgen in der GridView angezeigt. Dies bedeutet, dass der Text der Trenn Zeile für diese Produkte mit `NULL` Werten wie die Kategorie lautet: (das heißt, es gibt keinen Namen nach Kategorie: wie bei Kategorie: Getränke). Wenn Sie hier einen Wert anzeigen möchten, können Sie entweder die Eigenschaft boundfields [`NullDisplayText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) auf den Text festlegen, der angezeigt werden soll, oder Sie können eine Bedingungs Anweisung in der Methode "Rendering" hinzufügen, wenn Sie die `currentValue` der Eigenschaft "`Text` der Trenn zeichenzeile" zuweisen.

## <a name="summary"></a>Summary

In der GridView sind nicht viele integrierte Optionen zum Anpassen der Sortier Schnittstelle enthalten. Allerdings ist es mit einem wenig Code auf niedriger Ebene möglich, die GridView s-Steuerelement Hierarchie zu optimieren, um eine benutzerdefinierte Schnittstelle zu erstellen. In diesem Tutorial haben Sie erfahren, wie Sie eine Sortierungs Gruppe für Sortiergruppen für eine sortierbare GridView-Struktur hinzufügen, mit der die verschiedenen Gruppen und Gruppen Grenzen leichter identifiziert werden. Weitere Beispiele für angepasste Sortier Schnittstellen finden Sie im Blogeintrag [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [A few ASP.NET 2,0 GridView Sortierungs Tipps und Tricks](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) .

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Vorheriges](sorting-custom-paged-data-vb.md)
