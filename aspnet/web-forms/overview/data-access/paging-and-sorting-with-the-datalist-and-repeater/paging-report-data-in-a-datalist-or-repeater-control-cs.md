---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: Paging von Berichtsdaten in einem DataList-oder RepeaterC#-Steuerelement () | Microsoft-Dokumentation
author: rick-anderson
description: Obwohl weder der DataList noch der Repeater Unterstützung für automatisches Paging oder Sortieren bietet, wird in diesem Tutorial veranschaulicht, wie der DataList oder der Repeater Paging-Unterstützung hinzugefügt wird,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 16686c7e41926698c0da9c60d3cf26e858f5daca
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617719"
---
# <a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Auslagern von Berichtsdaten in einem DataList- oder Wiederholungssteuerelement (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) oder [PDF herunterladen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> Obwohl weder der DataList noch der Repeater Unterstützung für automatisches Paging oder Sortieren bietet, wird in diesem Tutorial veranschaulicht, wie der DataList oder der Repeater Paging-Unterstützung hinzugefügt wird, was eine viel flexiblere Auslagerung und Datenanzeige Schnittstellen ermöglicht.

## <a name="introduction"></a>Einführung

Paging und Sortierung sind zwei sehr gängige Features, wenn Daten in einer Online Anwendung angezeigt werden. Wenn Sie z. b. in einem Online Buchhandel nach ASP.net Books suchen, gibt es möglicherweise Hunderte solcher Bücher, aber im Bericht mit den Suchergebnissen werden nur zehn Übereinstimmungen pro Seite aufgeführt. Darüber hinaus können die Ergebnisse nach Titel, Preis, Seitenanzahl, Autorenname usw. sortiert werden. Wie im Lernprogramm zum [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-cs.md) erläutert, bieten die Steuerelemente GridView, DetailsView und FormView alle integrierten Pagingunterstützung, die im Tick eines Kontrollkästchens aktiviert werden kann. Die GridView umfasst auch Sortierungs Unterstützung.

Leider bieten weder der DataList noch der Repeater Unterstützung für automatisches Paging oder sortieren. In diesem Tutorial wird erläutert, wie Sie dem DataList-oder Wiederholungs Dienst Paging-Unterstützung hinzufügen. Wir müssen manuell die Paging-Schnittstelle erstellen, die entsprechende Seite der Datensätze anzeigen und die Seite, die über Postbacks besucht wird, merken. Dies dauert zwar mehr Zeit und Code als bei GridView, DetailsView oder FormView, das DataList-und Repeater-Tool ermöglicht jedoch viel flexiblere Paginierung und Datenanzeige Schnittstellen.

> [!NOTE]
> Dieses Tutorial konzentriert sich ausschließlich auf das Paging. Im nächsten Tutorial widmen wir uns dem Hinzufügen von Sortierungs Funktionen.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten für das Paging und das Sortieren von Tutorials

Bevor wir mit diesem Tutorial beginnen, nehmen wir uns einen Moment Zeit, um die ASP.NET Seiten hinzuzufügen, die wir für dieses Tutorial benötigen, und das nächste. Erstellen Sie zunächst einen neuen Ordner in dem Projekt mit dem Namen `PagingSortingDataListRepeater`. Fügen Sie anschließend die folgenden fünf ASP.NET-Seiten zu diesem Ordner hinzu, wobei alle für die Verwendung der Master Seite `Site.master`konfiguriert sind:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`

![Erstellen Sie einen pagingsortingdatalistrepeer-Ordner, und fügen Sie das Tutorial ASP.net Pages hinzu.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Abbildung 1**: Erstellen eines `PagingSortingDataListRepeater` Ordners und Hinzufügen des Tutorials ASP.NET Seiten

Öffnen Sie als nächstes die Seite `Default.aspx`, und ziehen Sie das Steuerelement `SectionLevelTutorialListing.ascx` Benutzer aus dem Ordner `UserControls` auf die Entwurfs Oberfläche. Dieses Benutzer Steuerelement, das wir im Tutorial zu [Master Seiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-cs.md) erstellt haben, listet die Site Map auf und zeigt diese Tutorials im aktuellen Abschnitt in einer Aufzählung an.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))

Damit in der Auflistungs Liste die Lernprogramme zum Paging und Sortieren angezeigt werden, die wir erstellen, müssen wir Sie der Site Übersicht hinzufügen. Öffnen Sie die Datei `Web.sitemap`, und fügen Sie nach dem Bearbeiten und Löschen mit dem DataList Site Map-Knoten Markup das folgende Markup hinzu:

[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]

![Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Abbildung 3**: Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen

## <a name="a-review-of-paging"></a>Eine Überprüfung des Paging

In den vorherigen Tutorials wurde erläutert, wie die Daten in den Steuerelementen GridView, DetailsView und FormView durchlaufen werden. Diese drei Steuerelemente bieten eine einfache Form der Paginierung , die als standardpaginierung bezeichnet wird, die durch einfaches Aktivieren der Option Paging aktivieren im Smarttags des Steuer Elements implementiert werden kann. Beim standardmäßigen Paging wird jedes Mal, wenn eine Seite mit Daten angefordert wird, entweder auf der ersten Seite, oder wenn der Benutzer zu einer anderen Seite der Daten navigiert, das GridView-, DetailsView-oder FormView-Steuerelement *alle* Daten von ObjectDataSource erneut anfordert. Anschließend wird eine bestimmte Gruppe von Datensätzen ausgegeben, die für den angeforderten Seitenindex und die Anzahl der pro Seite anzuzeigenden Datensätze angezeigt werden. Im Tutorial zum [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-cs.md) wurde das standardmäßige Paging ausführlich erläutert.

Da beim Paging standardmäßig alle Datensätze für jede Seite erneut angefordert werden, ist es beim Paging durch ausreichend große Datenmengen nicht praktikabel. Stellen Sie sich beispielsweise das Paging durch 50.000 Datensätze mit einer Seitengröße von 10 vor. Jedes Mal, wenn der Benutzer zu einer neuen Seite wechselt, müssen alle 50.000-Datensätze aus der Datenbank abgerufen werden, auch wenn nur zehn von Ihnen angezeigt werden.

Durch *benutzerdefiniertes Paging* werden die Leistungsaspekte der Standard Auslagerung gelöst, indem nur die genaue Teilmenge der Datensätze erfasst wird, die auf der angeforderten Seite angezeigt werden sollen. Beim Implementieren von benutzerdefiniertem Paging müssen wir die SQL-Abfrage schreiben, die effizient nur den richtigen Satz von Datensätzen zurückgibt. Wir haben gesehen, wie eine solche Abfrage mit SQL Server 2005 s New [`ROW_NUMBER()`-Schlüsselwort](http://www.4guysfromrolla.com/webtech/010406-1.shtml) zurück im Lernprogramm " [effizientes Paging durch große Datenmengen](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) " erstellt wird.

Um das Standardpaging in den DataList-oder Repeater-Steuerelementen zu implementieren, können Sie die [`PagedDataSource`-Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) als Wrapper für die `ProductsDataTable` verwenden, deren Inhalt ausgelagert wird. Die `PagedDataSource`-Klasse verfügt über eine `DataSource`-Eigenschaft, die jedem Aufzähl baren Objekt zugewiesen werden kann, und [`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) und [`CurrentPageIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) Eigenschaften, die angeben, wie viele Datensätze pro Seite und dem aktuellen Seitenindex angezeigt werden. Nachdem diese Eigenschaften festgelegt wurden, kann die `PagedDataSource` als Datenquelle für jedes datenweb Steuerelement verwendet werden. Der `PagedDataSource`gibt bei der Enumeration nur die entsprechende Teilmenge der Datensätze seines inneren `DataSource` basierend auf den Eigenschaften `PageSize` und `CurrentPageIndex` zurück. Abbildung 4 zeigt die Funktionalität der `PagedDataSource`-Klasse.

![Die "pgeddatasource" umschließt ein Enumerable-Objekt mit einer abrechenbaren Schnittstelle.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Abbildung 4**: die `PagedDataSource` umschließt ein Enumerable-Objekt mit einer abrechenbaren Schnittstelle.

Das `PagedDataSource`-Objekt kann direkt aus der Geschäftslogik Schicht erstellt und konfiguriert und mithilfe von ObjectDataSource an einen DataList-oder Wiederholungs Modul gebunden werden, oder es kann direkt in der Code Behind-Klasse der ASP.net page s erstellt und konfiguriert werden. Wenn die letztgenannte Vorgehensweise verwendet wird, müssen wir die ObjectDataSource verwenden und stattdessen die auslagerbaren Daten Programm gesteuert an das DataList-oder Wiederholungs Programm binden.

Das `PagedDataSource`-Objekt verfügt auch über Eigenschaften zur Unterstützung von benutzerdefiniertem Paging. Wir können jedoch die Verwendung eines `PagedDataSource` für das benutzerdefinierte Paging umgehen, da wir bereits über BLL-Methoden in der `ProductsBLL`-Klasse verfügen, die für benutzerdefiniertes Paging konzipiert sind und die genauen Datensätze zurückgeben.

In diesem Tutorial wird die Implementierung der Standard Paginierung in einem DataList-Objekt durch Hinzufügen einer neuen Methode zur `ProductsBLL`-Klasse, die ein entsprechend konfiguriertes `PagedDataSource`-Objekt zurückgibt, untersucht. Im nächsten Tutorial erfahren Sie, wie Sie benutzerdefiniertes Paging verwenden.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Schritt 2: Hinzufügen einer Standard-Paging-Methode in der Geschäftslogik Ebene

Die `ProductsBLL`-Klasse verfügt zurzeit über eine Methode zum Zurückgeben aller Produktinformationen `GetProducts()` und eine für die Rückgabe einer bestimmten Teilmenge von Produkten an einem Start Index `GetProductsPaged(startRowIndex, maximumRows)`. Beim standardmäßigen Paging verwenden die Steuerelemente GridView, DetailsView und FormView alle Produkte mithilfe der `GetProducts()`-Methode, verwenden jedoch intern eine `PagedDataSource`, um nur die richtige Teilmenge der Datensätze anzuzeigen. Um diese Funktionalität mit den DataList-und Repeater-Steuerelementen zu replizieren, können wir eine neue Methode in der BLL erstellen, die dieses Verhalten imitiert.

Fügen Sie der `ProductsBLL` Klasse mit dem Namen `GetProductsAsPagedDataSource` eine Methode hinzu, die zwei ganzzahlige Eingabeparameter annimmt:

- `pageIndex` den Index der anzuzeigenden Seite, indiziert bei Null und
- `pageSize` die Anzahl der Datensätze, die pro Seite angezeigt werden sollen.

`GetProductsAsPagedDataSource` beginnt mit dem Abrufen *aller* Datensätze aus `GetProducts()`. Anschließend wird ein `PagedDataSource` Objekt erstellt, dessen `CurrentPageIndex`-und `PageSize`-Eigenschaften auf die Werte der Parameter für die Übergabe `pageIndex` und `pageSize` festgelegt werden. Die Methode wird beendet, indem der konfigurierte `PagedDataSource`zurückgegeben wird:

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Schritt 3: Anzeigen von Produktinformationen in einem DataList mithilfe von Standard-Paging

Mit der `GetProductsAsPagedDataSource`-Methode, die der `ProductsBLL`-Klasse hinzugefügt wurde, können wir jetzt einen DataList-oder Repeater-Wert erstellen, der Standard Paginierung ermöglicht. Öffnen Sie zunächst die Seite `Paging.aspx` im Ordner `PagingSortingDataListRepeater`, und ziehen Sie einen DataList aus der Toolbox auf den Designer, und legen Sie die Eigenschaft DataList s `ID` auf `ProductsDefaultPaging`fest. Erstellen Sie aus dem DataList s-Smarttag eine neue ObjectDataSource mit dem Namen `ProductsDefaultPagingDataSource`, und konfigurieren Sie Sie so, dass Daten mithilfe der `GetProductsAsPagedDataSource`-Methode abgerufen werden.

[![erstellen Sie eine ObjectDataSource, und konfigurieren Sie Sie für die Verwendung der getproductaspgeddatasource ()-Methode.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Abbildung 5**: Erstellen einer ObjectDataSource und Konfigurieren der Methode für die Verwendung der `GetProductsAsPagedDataSource` `()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))

Legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest.

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Abbildung 6**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))

Da die `GetProductsAsPagedDataSource`-Methode zwei Eingabeparameter erwartet, fordert der Assistent Sie zur Angabe der Quelle dieser Parameterwerte auf.

Die Werte für den Seitenindex und die Seitengröße müssen über Postbacks hinweg gespeichert werden. Sie können im Ansichts Zustand gespeichert, persistent in der Abfrage Zeichenfolge gespeichert, in Sitzungsvariablen gespeichert oder mithilfe einer anderen Technik gespeichert werden. In diesem Tutorial verwenden wir die Abfrage Zeichenfolge, die den Vorteil hat, dass eine bestimmte Datenseite mit einem Lesezeichen versehen werden kann.

Verwenden Sie insbesondere die QueryString-Felder "pageIndex" und "PageSize" für die Parameter "`pageIndex`" und "`pageSize`" (siehe Abbildung 7). Nehmen Sie sich einen Moment Zeit, um die Standardwerte für diese Parameter festzulegen, da die QueryString-Werte nicht vorhanden sind, wenn ein Benutzer diese Seite zum ersten Mal besucht. Legen Sie für `pageIndex`den Standardwert auf 0 (die erste Seite der Daten wird angezeigt) und `pageSize` s den Standardwert 4 fest.

[!["QueryString" als Quelle für die Parameter "pageIndex" und "PageSize" verwenden](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Abbildung 7**: Verwenden von "QueryString" als Quelle für die Parameter "`pageIndex`" und "`pageSize`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))

Nach dem Konfigurieren von ObjectDataSource erstellt Visual Studio automatisch eine `ItemTemplate` für den DataList. Passen Sie die `ItemTemplate` so an, dass nur der Name, die Kategorie und der Lieferant des Produkts angezeigt werden. Legen Sie außerdem die DataList s `RepeatColumns`-Eigenschaft auf 2 fest, deren `Width` auf 100% und deren `ItemStyle` e auf 50% `Width`. Diese breiten Einstellungen geben den gleichen Abstand für die beiden Spalten an.

Nachdem Sie diese Änderungen vorgenommen haben, sollten das DataList-und ObjectDataSource s-Markup in etwa wie folgt aussehen:

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Da wir in diesem Tutorial keine Aktualisierungs-oder Löschfunktionen ausführen, können Sie den Ansichts Zustand des DataList-s deaktivieren, um die gerenderte Seitengröße zu verringern.

Wenn Sie diese Seite anfänglich über einen Browser aufrufen, werden weder der `pageIndex` noch `pageSize` QueryString-Parameter bereitgestellt. Daher werden die Standardwerte 0 und 4 verwendet. Wie in Abbildung 8 gezeigt, führt dies zu einem DataList, der die ersten vier Produkte anzeigt.

[![werden die ersten vier Produkte aufgelistet.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Abbildung 8**: die ersten vier Produkte werden aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))

Ohne eine pagingschnittstelle gibt es derzeit keine einfache Möglichkeit für einen Benutzer, zur zweiten Seite der Daten zu navigieren. In Schritt 4 erstellen wir eine Paging-Schnittstelle. Vorerst kann das Paging jedoch nur durch direktes angeben der pagingkriterien in "QueryString" durchgeführt werden. Wenn Sie z. b. die zweite Seite anzeigen möchten, ändern Sie die URL in der Adressleiste des Browsers von `Paging.aspx` in `Paging.aspx?pageIndex=2`, und drücken Sie die EINGABETASTE. Dies bewirkt, dass die zweite Seite der Daten angezeigt wird (siehe Abbildung 9).

[![die zweite Seite der Daten angezeigt wird.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Abbildung 9**: die zweite Seite der Daten wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))

## <a name="step-4-creating-the-paging-interface"></a>Schritt 4: Erstellen der Paging-Schnittstelle

Es gibt eine Vielzahl von unterschiedlichen Paging-Schnittstellen, die implementiert werden können. Die Steuerelemente GridView, DetailsView und FormView stellen vier verschiedene Schnittstellen zur Auswahl:

- Im nächsten Schritt können **vorherige** Benutzer jeweils eine Seite auf die nächste oder vorherige Seite verschieben.
- **Next, Previous, First, Last** neben Next und Previous Buttons, enthält diese Schnittstelle die erste und die letzte Schaltfläche, um zur ersten oder letzten Seite zu wechseln.
- **Numeric** listet die Seitenzahlen in der Paging-Schnittstelle auf, sodass ein Benutzer schnell zu einer bestimmten Seite springen kann.
- **Numeric, First, Last** zusätzlich zu den numerischen Seitenzahlen, enthält Schaltflächen zum Wechseln zur ersten oder letzten Seite.

Für DataList und Repeater sind wir für die Entscheidung für eine pagingschnittstelle und deren Implementierung verantwortlich. Dies umfasst das Erstellen der erforderlichen websteuer Elemente auf der Seite und das Anzeigen der angeforderten Seite, wenn auf eine bestimmte Paging-Schnittstellen Schaltfläche geklickt wird. Darüber hinaus müssen bestimmte Paging-Schnittstellen Steuerelemente möglicherweise deaktiviert werden. Wenn Sie z. b. die erste Seite der Daten mit der nächsten, vorherigen, ersten, letzten Oberfläche anzeigen, sind die Schaltflächen "First" und "Previous" deaktiviert.

Verwenden Sie für dieses Tutorial die nächste, vorherige, erste und letzte Schnittstelle. Fügen Sie der Seite vier Schaltflächen-websteuer Elemente hinzu, und legen Sie deren `ID` s auf `FirstPage`, `PrevPage`, `NextPage`und `LastPage`fest. Legen Sie die `Text` Eigenschaften auf &lt;&lt; First, &lt; Prev, Next &gt;und Last &gt;&gt; fest.

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

Erstellen Sie als nächstes einen `Click`-Ereignishandler für jede dieser Schaltflächen. In Kürze fügen wir den Code hinzu, der zum Anzeigen der angeforderten Seite erforderlich ist.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Merken der Gesamtzahl der Datensätze, die durchlaufen werden

Unabhängig davon, welche pagingschnittstelle ausgewählt ist, müssen wir die Gesamtzahl der Datensätze berechnen und merken, die durchlaufen werden. Die Gesamtanzahl der Zeilen (in Verbindung mit der Seitengröße) bestimmt, wie viele Datenseiten durchlaufen werden, wodurch bestimmt wird, welche pagingschnittstellensteuerelemente hinzugefügt oder aktiviert werden. In der nächsten, vorherigen, ersten, letzten Schnittstelle, die wir aufbauen, wird die Seitenzahl auf zweierlei Weise verwendet:

- , Um zu bestimmen, ob die letzte Seite angezeigt wird. in diesem Fall sind die Schaltflächen "Next" und "Last" deaktiviert.
- Wenn der Benutzer auf die letzte Schaltfläche klickt, müssen wir Sie auf die letzte Seite setzen, deren Index eine kleiner ist als die Seitenanzahl.

Die Seitenanzahl wird als die Obergrenze der Gesamt Zeilen Anzahl dividiert durch die Seitengröße berechnet. Wenn wir beispielsweise ein Paging durch 79 Datensätze mit vier Datensätzen pro Seite durchlaufen, ist die Seitenanzahl 20 (die Obergrenze von 79/4). Bei Verwendung der numerischen Paging-Schnittstelle informiert uns diese Informationen darüber, wie viele numerische Seiten Schaltflächen angezeigt werden sollen. Wenn unsere pagingschnittstelle die Schaltflächen "Next" oder "Last" enthält, wird die Seitenanzahl verwendet, um zu bestimmen, wann die Schaltflächen für die nächste oder letzte

Wenn die pagingschnittstelle eine letzte Schaltfläche enthält, ist es zwingend erforderlich, dass die Gesamtanzahl der Datensätze, die durchlaufen werden, über Postbacks hinweg gespeichert werden, damit wir den letzten Seitenindex ermitteln können, wenn auf die letzte Schaltfläche geklickt wird. Um dies zu vereinfachen, erstellen Sie in der Code Behind-Klasse der ASP.net page s eine `TotalRowCount`-Eigenschaft, die den Wert des Ansichts Zustands beibehält:

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Zusätzlich zu den `TotalRowCount`nehmen Sie sich eine Minute Zeit, um schreibgeschützte Eigenschaften auf Seitenebene für den einfachen Zugriff auf den Seitenindex, die Seitengröße und die Seitenanzahl zu erstellen:

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Ermitteln der Gesamtanzahl von Datensätzen, die durchlaufen werden

Das `PagedDataSource` Objekt, das von der `Select()`-Methode von ObjectDataSource zurückgegeben wurde, enthält *alle* Produktdaten Sätze, auch wenn nur eine Teilmenge der Elemente in der DataList angezeigt wird. Die `PagedDataSource` s [`Count`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) gibt nur die Anzahl der Elemente zurück, die in DataList angezeigt werden. die [`DataSourceCount`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) gibt die Gesamtanzahl der Elemente innerhalb der `PagedDataSource`zurück. Daher müssen wir die ASP.net page s-Eigenschaft `TotalRowCount`-Eigenschaft den Wert der `PagedDataSource` s-`DataSourceCount`-Eigenschaft zuweisen.

Um dies zu erreichen, erstellen Sie einen Ereignishandler für das Ereignis "ObjectDataSource s `Selected`". Im `Selected`-Ereignishandler haben wir Zugriff auf den Rückgabewert der ObjectDataSource s `Select()`-Methode, in diesem Fall auf die `PagedDataSource`.

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>Anzeigen der angeforderten Datenseite

Wenn der Benutzer auf eine der Schaltflächen in der Paging-Schnittstelle klickt, muss die angeforderte Seite der Daten angezeigt werden. Da die Paging-Parameter über "QueryString" angegeben werden, um die angeforderte Datenseite anzuzeigen, verwenden Sie `Response.Redirect(url)`, damit der Benutzer die `Paging.aspx` Seite mit den entsprechenden Paging-Parametern erneut anfordert. Um beispielsweise die zweite Seite der Daten anzuzeigen, leiten wir den Benutzer an `Paging.aspx?pageIndex=1`um.

Um dies zu vereinfachen, erstellen Sie eine `RedirectUser(sendUserToPageIndex)`-Methode, die den Benutzer an `Paging.aspx?pageIndex=sendUserToPageIndex`umleitet. Anschließend wird diese Methode von der vier Schaltfläche `Click` Ereignis Handlern aufgerufen. Wenden Sie im `FirstPage` `Click`-Ereignishandler `RedirectUser(0)`an, um Sie an die erste Seite zu senden. Verwenden Sie im `PrevPage` `Click`-Ereignishandler `PageIndex - 1` als Seitenindex. Und so weiter.

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

Mit den `Click` Ereignis Handlern können die DataList s-Datensätze durch Klicken auf die Schaltflächen ausgelagert werden. Nehmen Sie sich einen Moment Zeit, um es auszuprobieren!

## <a name="disabling-paging-interface-controls"></a>Deaktivieren von Paging-Schnittstellen Steuerelementen

Derzeit werden alle vier Schaltflächen unabhängig von der angezeigten Seite aktiviert. Wir möchten jedoch die Schaltflächen "First" und "Previous" deaktivieren, wenn die erste Seite der Daten angezeigt wird, und die Schaltflächen "weiter" und "zurück" beim zeigen der letzten Seite. Das `PagedDataSource` Objekt, das von der ObjectDataSource s `Select()`-Methode zurückgegeben wird, verfügt über Eigenschaften [`IsFirstPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) und [`IsLastPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , die wir untersuchen können, um zu bestimmen, ob die erste oder letzte Seite der Daten angezeigt wird.

Fügen Sie dem Ereignishandler ObjectDataSource s `Selected` Folgendes hinzu:

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Mit dieser Addition werden die Schaltflächen "First" und "Previous" deaktiviert, wenn die erste Seite angezeigt wird, während die Schaltflächen "Next" und "Last" beim Anzeigen der letzten Seite deaktiviert werden.

Schließen Sie die Paging-Schnittstelle ab, indem Sie den Benutzer darüber informieren, welche Seite aktuell angezeigt wird und wie viele Seiten insgesamt vorhanden sind. Fügen Sie der Seite ein Label-websteuer Element hinzu, und legen Sie dessen `ID`-Eigenschaft auf `CurrentPageNumber`fest. Legen Sie die `Text`-Eigenschaft im Ereignishandler von ObjectDataSource s so fest, dass die aktuell angezeigte aktuelle Seite (`PageIndex + 1`) und die Gesamtzahl der Seiten (`PageCount`) enthalten sind.

[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

Abbildung 10 zeigt `Paging.aspx` beim ersten Besuch. Da der QueryString-Wert leer ist, zeigt der DataList standardmäßig die ersten vier Produkte an. die Schaltflächen First und Previous sind deaktiviert. Wenn Sie auf Weiter klicken, werden die nächsten vier Datensätze angezeigt (siehe Abbildung 11); die Schaltflächen "First" und "Previous" sind jetzt aktiviert.

[![die erste Seite der Daten angezeigt wird.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Abbildung 10**: die erste Seite der Daten wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))

[![die zweite Seite der Daten angezeigt wird.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Abbildung 11**: die zweite Seite der Daten wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))

> [!NOTE]
> Die Paging-Schnittstelle kann weiter verbessert werden, indem der Benutzer die Anzahl der Seiten angibt, die pro Seite angezeigt werden sollen. Beispielsweise könnte eine Dropdown List hinzugefügt werden, die Seitengrößen Optionen wie 5, 10, 25, 50 und alle auflistet. Wenn Sie eine Seitengröße auswählen, muss der Benutzer zurück an `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`umgeleitet werden. Ich lasse diese Erweiterung nicht als Übung für den Reader umsetzen.

## <a name="using-custom-paging"></a>Verwenden von benutzerdefiniertem Paging

Die Daten des DataList-Datentyps werden durch die ineffiziente Standard pagingtechnik durchlaufen. Beim Paging durch ausreichend große Datenmengen ist es zwingend erforderlich, dass benutzerdefinierte Paginierung verwendet werden. Obwohl sich die Implementierungsdetails geringfügig unterscheiden, entsprechen die Konzepte hinter der Implementierung von benutzerdefiniertem Paging in einem DataList dem Standard-Paging. Verwenden Sie bei benutzerdefiniertem Paging die `ProductBLL` Klassen-`GetProductsPaged` Methode (anstelle von `GetProductsAsPagedDataSource`). Wie im Lernprogramm [effizientes Paging durch große Datenmengen](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) erläutert, müssen `GetProductsPaged` den Start Zeilen Index und die maximale Anzahl von Zeilen übergeben, die zurückgegeben werden sollen. Diese Parameter können in der Abfrage Zeichenfolge genau wie die `pageIndex` und `pageSize` Parameter beibehalten werden, die bei der Standard Auslagerung verwendet werden.

Da es keine `PagedDataSource` mit benutzerdefiniertem Paging gibt, müssen alternative Techniken verwendet werden, um die Gesamtanzahl der Datensätze zu ermitteln, die durchlaufen werden, und ob die erste oder letzte Seite der Daten erneut angezeigt wird. Die `TotalNumberOfProducts()`-Methode in der `ProductsBLL`-Klasse gibt die Gesamtzahl der Produkte zurück, die durchlaufen werden. Wenn Sie ermitteln möchten, ob die erste Seite der Daten angezeigt wird, untersuchen Sie den Index für die Start Zeile, wenn der Wert 0 (null) ist, dann wird die erste Seite angezeigt. Die letzte Seite wird angezeigt, wenn der Start Zeilen Index plus die maximal zurück zugebende Zeile größer oder gleich der Gesamtzahl der Datensätze ist, die durchlaufen werden.

Im nächsten Tutorial erfahren Sie mehr über die Implementierung von benutzerdefiniertem Paging.

## <a name="summary"></a>Summary

Obwohl weder der DataList noch der Repeater die in den Steuerelementen GridView, DetailsView und FormView gefundene Standard-Pagingunterstützung bietet, können diese Funktionen mit minimalem Aufwand hinzugefügt werden. Die einfachste Möglichkeit zum Implementieren des Standardpaging besteht darin, den gesamten Satz von Produkten in einem `PagedDataSource` zu umschließen und dann die `PagedDataSource` an den DataList-oder Repeater zu binden. In diesem Tutorial haben wir der `ProductsBLL`-Klasse die `GetProductsAsPagedDataSource`-Methode hinzugefügt, um die `PagedDataSource`zurückzugeben. Die `ProductsBLL`-Klasse enthält bereits die Methoden, die für das benutzerdefinierte Paging `GetProductsPaged` und `TotalNumberOfProducts`erforderlich sind.

Zusammen mit dem Abrufen der exakten Gruppe von Datensätzen, die für das benutzerdefinierte Paging oder alle Datensätze in einer `PagedDataSource` für das standardmäßige Paging angezeigt werden sollen, muss auch die Paging-Schnittstelle manuell hinzugefügt werden. Für dieses Tutorial haben wir eine nächste, vorherige, erste und letzte Schnittstelle mit vier Schaltflächen-websteuer Elementen erstellt. Außerdem wurde ein Label-Steuerelement angezeigt, das die aktuelle Seitenzahl und die Gesamtzahl der Seiten anzeigt.

Im nächsten Tutorial erfahren Sie, wie Sie dem DataList und Repeater Sortier Unterstützung hinzufügen. Wir sehen uns auch an, wie Sie einen DataList erstellen können, der sowohl per Pager als auch sortiert werden kann (mit Beispielen, die die standardmäßige und benutzerdefinierte Auslagerung verwenden).

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Liz shulok, Ken pespisa und Bernadette Leigh. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](sorting-data-in-a-datalist-or-repeater-control-cs.md)
