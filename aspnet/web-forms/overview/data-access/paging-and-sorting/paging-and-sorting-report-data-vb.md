---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Paging und Sortieren von Berichtsdaten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Paging und Sortierung sind zwei sehr gängige Features, wenn Daten in einer Online Anwendung angezeigt werden. In diesem Tutorial wird zunächst das Hinzufügen von Sortierung und...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6785b5cd2d4d3a2c2e7f2c2fea93f5cd5e2fdf24
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78502353"
---
# <a name="paging-and-sorting-report-data-vb"></a>Auslagern und Sortieren von Berichtsdaten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) oder [PDF herunterladen](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Paging und Sortierung sind zwei sehr gängige Features, wenn Daten in einer Online Anwendung angezeigt werden. In diesem Tutorial betrachten wir zuerst das Hinzufügen von Sortierung und Paginierung zu unseren Berichten, auf die wir dann in zukünftigen Tutorials aufbauen werden.

## <a name="introduction"></a>Einführung

Paging und Sortierung sind zwei sehr gängige Features, wenn Daten in einer Online Anwendung angezeigt werden. Wenn Sie z. b. in einem Online Buchhandel nach ASP.net Books suchen, gibt es möglicherweise Hunderte solcher Bücher, aber im Bericht mit den Suchergebnissen werden nur zehn Übereinstimmungen pro Seite aufgeführt. Darüber hinaus können die Ergebnisse nach Titel, Preis, Seitenanzahl, Autorenname usw. sortiert werden. Während in den letzten 23 Tutorials erläutert wurde, wie eine Vielzahl von Berichten erstellt werden kann, einschließlich Schnittstellen, die das Hinzufügen, bearbeiten und Löschen von Daten ermöglichen, haben wir uns nicht mit dem Sortieren von Daten beschäftigt, und die einzigen Auslagerungs Beispiele, die wir gesehen haben, sind in den DetailsView und FormView enthalten. Steuerelemente.

In diesem Tutorial erfahren Sie, wie Sie den Berichten Sortier-und Paginierung hinzufügen. hierzu können Sie einfach nur ein paar Kontrollkästchen aktivieren. Leider hat diese vereinfachte Implementierung Nachteile, dass die Sortier Schnittstelle ein wenig zu wünschen übrig lässt, und die pagingroutinen sind nicht für das effiziente Paging durch große Resultsets konzipiert. In zukünftigen Tutorials erfahren Sie, wie Sie die Einschränkungen der sofort Einsatz orientierten Paging-und Sortierungs Lösungen überwinden können.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten für das Paging und das Sortieren von Tutorials

Bevor wir mit diesem Tutorial beginnen, nehmen wir uns einen Moment Zeit, um die ASP.NET Seiten hinzuzufügen, die wir für dieses Tutorial benötigen, und die nächsten drei. Erstellen Sie zunächst einen neuen Ordner in dem Projekt mit dem Namen `PagingAndSorting`. Fügen Sie anschließend die folgenden fünf ASP.NET-Seiten zu diesem Ordner hinzu, wobei alle für die Verwendung der Master Seite `Site.master`konfiguriert sind:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`

![Erstellen Sie einen pagingandsortier-Ordner, und fügen Sie das Tutorial ASP.net Pages hinzu.](paging-and-sorting-report-data-vb/_static/image1.png)

**Abbildung 1**: Erstellen eines pagingandsortier-Ordners und Hinzufügen des Tutorials ASP.NET Seiten

Öffnen Sie als nächstes die Seite `Default.aspx`, und ziehen Sie das Steuerelement `SectionLevelTutorialListing.ascx` Benutzer aus dem Ordner `UserControls` auf die Entwurfs Oberfläche. Dieses Benutzer Steuerelement, das wir im Tutorial zu [Master Seiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-vb.md) erstellt haben, listet die Site Map auf und zeigt diese Tutorials im aktuellen Abschnitt in einer Aufzählung an.

![Fügen Sie das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzu.](paging-and-sorting-report-data-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen des Benutzer Steuer Elements "sectionleveltutoriallisting. ascx" zu "default. aspx"

Damit in der Auflistungs Liste die Lernprogramme zum Paging und Sortieren angezeigt werden, die wir erstellen, müssen wir Sie der Site Übersicht hinzufügen. Öffnen Sie die Datei `Web.sitemap`, und fügen Sie nach dem Bearbeiten, einfügen und Löschen von Site Map-Knoten Markup das folgende Markup hinzu:

[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]

![Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen](paging-and-sorting-report-data-vb/_static/image3.png)

**Abbildung 3**: Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen

## <a name="step-2-displaying-product-information-in-a-gridview"></a>Schritt 2: Anzeigen von Produktinformationen in einer GridView

Bevor wir die Paging-und Sortierungs Funktionen implementieren, sollten Sie zunächst eine standardmäßige, nicht sortierbare GridView erstellen, in der die Produktinformationen aufgelistet sind. Dies ist eine Aufgabe, die wir schon in dieser tutorialreihe mehrmals durchgeführt haben, damit diese Schritte vertraut sein sollten. Öffnen Sie zunächst die Seite `SimplePagingSorting.aspx`, und ziehen Sie ein GridView-Steuerelement aus der Toolbox auf den Designer, und legen Sie dessen `ID`-Eigenschaft auf `Products`fest. Erstellen Sie als nächstes eine neue ObjectDataSource, die die productbll-Klasse s `GetProducts()`-Methode verwendet, um alle Produktinformationen zurückzugeben.

![Abrufen von Informationen zu allen Produkten mithilfe der GetProducts ()-Methode](paging-and-sorting-report-data-vb/_static/image4.png)

**Abbildung 4**: Abrufen von Informationen über alle Produkte mithilfe der GetProducts ()-Methode

Da es sich bei diesem Bericht um einen schreibgeschützten Bericht handelt, ist es nicht erforderlich, die ObjectDataSource s-`Insert()`, `Update()`-oder `Delete()`-Methoden den entsprechenden `ProductsBLL` Methoden zuzuordnen. Wählen Sie daher in der Dropdown Liste für die Registerkarten aktualisieren, einfügen und löschen die Option (keine) aus.

![Wählen Sie in der Dropdown Liste auf den Registerkarten aktualisieren, einfügen und löschen die Option (keine) aus.](paging-and-sorting-report-data-vb/_static/image5.png)

**Abbildung 5**: Wählen Sie in der Dropdown Liste auf den Registerkarten aktualisieren, einfügen und löschen die Option (keine) aus.

Als nächstes passen Sie die GridView s-Felder so an, dass nur die Produktnamen, Lieferanten, Kategorien, Preise und nicht mehr unterstützten Status angezeigt werden. Darüber hinaus können Sie alle Formatierungs Änderungen auf Feldebene vornehmen, wie z. b. das Anpassen der `HeaderText` Eigenschaften oder das Formatieren des Preises als Währung. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der GridView s in etwa wie folgt aussehen:

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

In Abbildung 6 wird der Fortschritt angezeigt, der in einem Browser angezeigt wird. Beachten Sie, dass auf der Seite alle Produkte auf einem Bildschirm aufgeführt werden. dabei werden die Namen, die Kategorie, der Lieferant, der Preis und der Status des jeweiligen Produkts angezeigt.

[![werden alle Produkte aufgelistet.](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Abbildung 6**: alle Produkte werden aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image8.png))

## <a name="step-3-adding-paging-support"></a>Schritt 3: Hinzufügen von Paging-Unterstützung

Das Auflisten *aller* Produkte auf einem Bildschirm kann dazu führen, dass der Benutzer die Daten überlastet. Damit die Ergebnisse besser verwaltbar werden, können wir die Daten in kleinere Datenseiten unterbrechen und es dem Benutzer ermöglichen, die Datenseiten Weise zu durchlaufen. Um dies zu erreichen, aktivieren Sie einfach das Kontrollkästchen Paging aktivieren aus dem GridView s-Smarttag (Dadurch wird die GridView s [`AllowPaging`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) auf `true`) festgelegt.

[Aktivieren Sie das Kontrollkästchen Paging aktivieren, um Paging-Unterstützung hinzuzufügen. ![](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Abbildung 7**: Aktivieren des Kontrollkästchens Paging aktivieren zum Hinzufügen von Pagingunterstützung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image11.png))

Durch das Aktivieren von Paging wird die Anzahl der pro Seite angezeigten Datensätze begrenzt, und der GridView wird eine *pagingschnittstelle* hinzugefügt. Die standardmäßige Paging-Schnittstelle, wie in Abbildung 7 dargestellt, ist eine Reihe von Seitenzahlen, die es dem Benutzer ermöglichen, schnell von einer Datenseite zu einem anderen zu navigieren. Diese pagingschnittstelle sollte vertraut aussehen, wie wir Sie beim Hinzufügen von Paging-Unterstützung zu den DetailsView-und FormView-Steuerelementen in früheren Tutorials gesehen haben.

Sowohl das DetailsView-Steuerelement als auch das FormView-Steuerelement zeigen nur einen Datensatz pro Seite an. Die GridView nimmt jedoch die [`PageSize`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) an, um zu bestimmen, wie viele Datensätze pro Seite angezeigt werden sollen (diese Eigenschaft hat standardmäßig den Wert 10).

Diese Schnittstelle "GridView", "DetailsView" und "FormView s Paging" kann mit den folgenden Eigenschaften angepasst werden:

- `PagerStyle` die die Stil Informationen für die pagingschnittstelle angibt. kann Einstellungen wie `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`usw. angeben.
- `PagerSettings` enthält eine Reihe von Eigenschaften, die die Funktionalität der Paging-Schnittstelle anpassen können. `PageButtonCount` gibt die maximale Anzahl numerischer Seitenzahlen an, die in der Paging-Schnittstelle angezeigt werden (der Standardwert ist 10). die [`Mode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) gibt an, wie die Paging-Schnittstelle funktioniert und kann auf festgelegt werden: 

    - `NextPrevious` zeigt eine Schaltfläche "weiter" und "zurück" an, die es dem Benutzer ermöglicht, eine Seite einzeln vorwärts oder rückwärts zu verschieben.
    - `NextPreviousFirstLast` zusätzlich zu den Schaltflächen weiter und zurück sind auch die Schaltflächen First und Last enthalten, die es dem Benutzer ermöglichen, schnell zur ersten oder letzten Seite der Daten zu wechseln.
    - `Numeric` zeigt eine Reihe von Seitenzahlen an, sodass der Benutzer sofort zu jeder Seite springen kann.
    - `NumericFirstLast` zusätzlich zu den Seitenzahlen enthält die Schaltflächen "First" und "Last", die es dem Benutzer ermöglichen, schnell zur ersten oder letzten Seite der Daten zu wechseln. die ersten/letzten Schaltflächen werden nur angezeigt, wenn alle numerischen Seitenzahlen nicht passen.

Außerdem bieten GridView, DetailsView und FormView alle die Eigenschaften `PageIndex` und `PageCount`, die angeben, welche aktuelle Seite angezeigt wird, und die Gesamtzahl der Datenseiten. Die `PageIndex`-Eigenschaft wird beginnend mit 0 indiziert. Dies bedeutet, dass beim Anzeigen der ersten Datenseite `PageIndex` gleich 0 ist. `PageCount`hingegen beginnt bei 1, was bedeutet, dass `PageIndex` auf die Werte zwischen 0 und `PageCount - 1`beschränkt ist.

Nehmen Sie sich einen Moment Zeit, um die Standarddarstellung der GridView-Paging-Schnittstelle zu verbessern. Lassen Sie die Paging-Schnittstelle insbesondere an einem hellgrauen Hintergrund ausrichten. Anstatt diese Eigenschaften direkt über die GridView s `PagerStyle`-Eigenschaft festzulegen, erstellen Sie eine CSS-Klasse in `Styles.css` mit dem Namen `PagerRowStyle` und weisen dann die `PagerStyle` s `CssClass`-Eigenschaft über das Design zu. Öffnen Sie zunächst `Styles.css`, und fügen Sie die folgende CSS-Klassendefinition hinzu:

[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Öffnen Sie als nächstes die Datei `GridView.skin` im Ordner `DataWebControls` im Ordner `App_Themes`. Wie im Lernprogramm zu *Master Seiten und der Website Navigation* erläutert, können mithilfe von Skin-Dateien die Standardeigenschaftswerte für ein websteuer Element angegeben werden. Erweitern Sie daher die vorhandenen Einstellungen so, dass Sie das Festlegen der `PagerStyle` s `CssClass`-Eigenschaft auf `PagerRowStyle`einschließen. Konfigurieren Sie außerdem die Paging-Schnittstelle so, dass höchstens fünf numerische Seiten Schaltflächen mithilfe der `NumericFirstLast` pagingschnittstelle angezeigt werden.

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Auslagerungs Benutzer

Abbildung 8 zeigt die Webseite, wenn Sie über einen Browser besucht werden, nachdem das Kontrollkästchen Aktivieren von GridView s aktiviert wurde und die `PagerStyle`-und `PagerSettings` Konfigurationen über die `GridView.skin`-Datei vorgenommen wurden. Beachten Sie, dass nur zehn Datensätze angezeigt werden und die Paging-Schnittstelle anzeigt, dass die erste Seite der Daten angezeigt wird.

[![bei aktiviertem Paging wird jeweils nur eine Teilmenge der Datensätze angezeigt.](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Abbildung 8**: bei aktiviertem Paging werden jeweils nur eine Teilmenge der Datensätze angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image14.png))

Wenn der Benutzer auf eine der Seitennummern in der pagingschnittstelle klickt, wird ein Postback durchführt, und die Seite wird erneut geladen, sodass die angeforderten Seiten Einträge angezeigt werden. Abbildung 9 zeigt die Ergebnisse nach der Entscheidung, die letzte Datenseite anzuzeigen. Beachten Sie, dass die letzte Seite nur einen Datensatz enthält. Dies liegt daran, dass insgesamt 81 Datensätze vorhanden sind, was zu acht Seiten mit 10 Datensätzen pro Seite und einer Seite mit einem einzelnen Datensatz führt.

[![klicken auf eine Seitenzahl verursacht ein Postback und zeigt die entsprechende Teilmenge der Datensätze an.](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Abbildung 9**: Klicken auf eine Seitenzahl verursacht ein Postback und zeigt die entsprechende Teilmenge der Datensätze[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image17.png))

## <a name="paging-s-server-side-workflow"></a>Server seitiger Paging-Workflow

Wenn der Endbenutzer auf eine Schaltfläche in der pagingschnittstelle klickt, folgt ein Postback, und der folgende serverseitige Workflow beginnt:

1. Die GridView s-(oder DetailsView-oder FormView)-`PageIndexChanging` Ereignis wird ausgelöst.
2. Die ObjectDataSource fordert *alle* Daten aus der BLL erneut an. die Eigenschaften Werte GridView s `PageIndex` und `PageSize` werden verwendet, um zu bestimmen, welche Datensätze von der BLL in der GridView angezeigt werden müssen.
3. Das GridView s `PageIndexChanged`-Ereignis wird ausgelöst.

In Schritt 2 fordert ObjectDataSource alle Daten aus der Datenquelle erneut an. Diese Art von Paginierung wird häufig als *standardpaginierung*bezeichnet, da es sich dabei um das Pagingverhalten handelt, das beim Festlegen der `AllowPaging`-Eigenschaft auf `true`standardmäßig verwendet wird. Beim standardaging Ruft das datenweb Steuerelement alle Datensätze für jede Datenseite ab, obwohl tatsächlich nur eine Teilmenge der Datensätze in das HTML-Format gerendert wird, das an den Browser gesendet wird. Wenn die Datenbankdaten nicht von der BLL-oder ObjectDataSource zwischengespeichert werden, ist das standardmäßige Paging für ausreichend große Resultsets oder für Webanwendungen mit vielen gleichzeitigen Benutzern nicht praktikabel.

Im nächsten Tutorial wird erläutert, wie *benutzerdefiniertes Paging*implementiert wird. Mit benutzerdefiniertem Paging können Sie ObjectDataSource ausdrücklich anweisen, nur den genauen Satz von Datensätzen abzurufen, der für die angeforderte Datenseite benötigt wird. Wie Sie sich vorstellen können, verbessert das benutzerdefinierte Paging die Effizienz beim Paging durch große Resultsets erheblich.

> [!NOTE]
> Beim Paging durch ausreichend große Resultsets oder für Standorte mit vielen gleichzeitigen Benutzern ist das Paging standardmäßig nicht geeignet. Beachten Sie jedoch, dass das benutzerdefinierte Paging mehr Änderungen und Aufwand für die Implementierung erfordert und nicht so einfach ist wie das Aktivieren eines Kontrollkästchens (Standardeinstellung). Paging). Aus diesem Grund ist das standardmäßige Paging möglicherweise die ideale Wahl für kleine Websites mit geringem Datenverkehr oder beim Paging durch relativ kleine Resultsets, da es wesentlich einfacher und schneller zu implementieren ist.

Wenn wir z. b. wissen, dass wir nie mehr als 100 Produkte in unserer Datenbank haben, ist die minimale Leistungssteigerung durch das benutzerdefinierte Paging wahrscheinlich durch den Aufwand ausgeglichen, der für die Implementierung erforderlich ist. Wenn es jedoch möglich ist, dass ein Tag Tausende oder Zehntausende von Produkten hat, wird die Skalierbarkeit unserer Anwendung durch *die Implementierung von* benutzerdefiniertem Paging erheblich beeinträchtigt.

## <a name="step-4-customizing-the-paging-experience"></a>Schritt 4: Anpassen der Paginierung

Die datenweb Steuerelemente bieten eine Reihe von Eigenschaften, die verwendet werden können, um das Auslagern von Benutzern zu verbessern. Die `PageCount`-Eigenschaft gibt beispielsweise an, wie viele Seiten insgesamt vorhanden sind, während die `PageIndex`-Eigenschaft die aktuelle Seite angibt, die besucht wird, und kann so festgelegt werden, dass ein Benutzer schnell zu einer bestimmten Seite verschoben wird. Um zu veranschaulichen, wie diese Eigenschaften verwendet werden, um die Benutzer-Paginierung zu verbessern, können Sie der Seite ein Label-websteuer Element hinzufügen, mit dem der Benutzer informiert wird, welche Seite Sie zurzeit besuchen, sowie ein Dropdown List-Steuerelement, mit dem Sie schnell zu einer bestimmten Seite springen können. .

Fügen Sie zunächst der Seite ein Label-websteuer Element hinzu, legen Sie die `ID`-Eigenschaft auf `PagingInformation`fest, und löschen Sie die `Text`-Eigenschaft. Erstellen Sie als nächstes einen Ereignishandler für das GridView s `DataBound`-Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Dieser Ereignishandler weist die `PagingInformation` Bezeichnung s `Text` Eigenschaft einer Nachricht zu, die den Benutzer darüber informiert, dass die Seite, auf der er gerade besucht wird, `Products.PageIndex + 1` aus der Anzahl der Gesamt Seiten `Products.PageIndex` `Products.PageCount`.`PageIndex` Im Gegensatz zum `PageIndexChanged`-Ereignishandler habe ich die `Text`-Eigenschaft "This-Bezeichnung zuweisen" im `DataBound`-Ereignishandler gewählt, da das `DataBound`-Ereignis jedes Mal ausgelöst wird, wenn Daten an die GridView gebunden werden, während der `PageIndexChanged`-Ereignishandler nur ausgelöst wird, wenn der Seitenindex geändert wird. Wenn das GridView-Ereignis anfänglich Daten gebunden ist, wird das `PageIndexChanging`-Ereignis nicht ausgelöst (während das `DataBound`-Ereignis).

Mit dieser Addition wird dem Benutzer nun eine Meldung angezeigt, die angibt, welche Seite Sie besuchen und wie viele Daten auf dem Datenblatt vorhanden sind.

[![die aktuelle Seitenzahl und die Gesamtzahl der Seiten angezeigt.](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Abbildung 10**: die aktuelle Seitenzahl und die Gesamtzahl der Seiten werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image20.png))

Zusätzlich zum Label-Steuerelement können Sie auch ein Dropdown List-Steuerelement hinzufügen, das die Seitenzahlen in der GridView mit der aktuell angezeigten Seite auflistet. Die Idee besteht darin, dass der Benutzer schnell von der aktuellen Seite zu einer anderen springen kann, indem er einfach den Index für die neue Seite aus der Dropdown Liste auswählt. Fügen Sie zunächst dem Designer eine Dropdown Liste hinzu, und legen Sie dessen `ID`-Eigenschaft auf `PageList` und die Option "AutoPostBack aktivieren" aus dem Smarttag fest.

Kehren Sie als nächstes zum `DataBound`-Ereignishandler zurück, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Dieser Code beginnt mit dem Löschen der Elemente in der Dropdown Liste `PageList`. Dies mag überflüssig erscheinen, da nicht erwartet wird, dass sich die Anzahl der Seiten ändert, andere Benutzer das System aber gleichzeitig verwenden, um Datensätze aus der `Products` Tabelle hinzuzufügen oder daraus zu entfernen. Solche Einfügungen oder Löschungen können die Anzahl der Datenseiten ändern.

Als nächstes müssen wir die Seitenzahlen erneut erstellen und die Zuordnung zur aktuellen GridView-`PageIndex`, die standardmäßig ausgewählt ist. Dies erreichen Sie mit einer Schleife zwischen 0 und `PageCount - 1`, Hinzufügen eines neuen `ListItem` in jeder Iterationen und Festlegen seiner `Selected`-Eigenschaft auf true, wenn der aktuelle Iterations Index der GridView s-`PageIndex` Eigenschaft entspricht.

Schließlich müssen wir einen Ereignishandler für das DropDownList s `SelectedIndexChanged`-Ereignis erstellen, das jedes Mal ausgelöst wird, wenn der Benutzer ein anderes Element aus der Liste auswählt. Wenn Sie diesen Ereignishandler erstellen möchten, doppelklicken Sie einfach auf die Dropdown Liste im Designer, und fügen Sie dann den folgenden Code hinzu:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Wie in Abbildung 11 gezeigt, bewirkt das Ändern der Eigenschaft "GridView s `PageIndex`", dass die Daten an die GridView-Ansicht zurückgesetzt werden. Im GridView s `DataBound`-Ereignishandler wird der entsprechende DropDownList-`ListItem` ausgewählt.

[![wird der Benutzer beim Auswählen des Dropdown-Listen Elements page 6 automatisch auf die sechste Seite gebracht.](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Abbildung 11**: der Benutzer wird beim Auswählen des Dropdown-Listen Elements der Seite 6 automatisch auf die sechste Seite gebracht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image23.png)).

## <a name="step-5-adding-bi-directional-sorting-support"></a>Schritt 5: Hinzufügen von Unterstützung für bidirektionale Sortierung

Das Hinzufügen von Unterstützung für bidirektionale Sortierungen ist so einfach wie das Hinzufügen von Paging-Unterstützung. Aktivieren Sie einfach die Option zum Aktivieren der Sortierung aus dem GridView s-Smarttag (mit dem die GridView s `true`[`AllowSorting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) Dadurch werden alle Header der GridView s-Felder als LinkButtons gerendert. Wenn Sie darauf klicken, wird ein Postback ausgelöst, und die Daten werden in aufsteigender Reihenfolge nach der angeklickten Spalte sortiert. Durch Klicken auf denselben Header Link Button werden die Daten erneut in absteigender Reihenfolge sortiert.

> [!NOTE]
> Wenn Sie anstelle eines typisierten Datasets eine benutzerdefinierte Datenzugriffs Schicht verwenden, verfügen Sie möglicherweise nicht über eine Option zum Aktivieren der Sortierung im GridView-Smarttag. Dieses Kontrollkästchen ist nur für GridViews verfügbar, die an Datenquellen gebunden sind, die das Sortieren unterstützen. Das typisierte DataSet bietet eine sofort einstufige Sortier Unterstützung, da ADO.net datdatababel eine `Sort` Methode bereitstellt, die beim Aufrufen die Datentyp DataRows mithilfe der angegebenen Kriterien sortiert.

Wenn die DAL keine Objekte zurückgibt, die das Sortieren unterstützen, müssen Sie ObjectDataSource so konfigurieren, dass Sortierungs Informationen an die Geschäftslogik Schicht übergeben werden, die entweder die Daten sortieren oder die Daten nach der dal Sortierungen lassen können. In einem zukünftigen Tutorial wird erläutert, wie Daten in der Geschäftslogik und in den Datenzugriffs Schichten sortiert werden.

Die Sortierungs Link Schaltflächen werden als HTML-Hyperlinks gerendert, deren aktuelle Farben (blau für einen nicht besuchten Link und ein dunkles roter für einen besuchten Link) mit der Hintergrundfarbe der Kopfzeile. Verwenden Sie stattdessen, dass alle Header Zeilen Links weiß angezeigt werden, unabhängig davon, ob Sie besucht wurden oder nicht. Dies kann erreicht werden, indem der `Styles.css`-Klasse Folgendes hinzugefügt wird:

[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Diese Syntax gibt an, dass beim Anzeigen dieser Hyperlinks in einem Element, das die Header Style-Klasse verwendet, weißer Text verwendet werden soll.

Wenn Sie die Seite über einen Browser besuchen, sollte der Bildschirm nach dieser CSS-Addition ähnlich wie in Abbildung 12 aussehen. In Abbildung 12 werden die Ergebnisse angezeigt, nachdem auf den Header Link "Price Field s" geklickt wurde.

[![werden die Ergebnisse nach UnitPrice in aufsteigender Reihenfolge sortiert.](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Abbildung 12**: die Ergebnisse wurden nach UnitPrice in aufsteigender Reihenfolge sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image26.png))

## <a name="examining-the-sorting-workflow"></a>Untersuchen des Sortier Workflows

Alle GridView-Felder "BoundField", "CheckBoxField", "TemplateField" usw. verfügen über eine `SortExpression`-Eigenschaft, die den Ausdruck angibt, der zum Sortieren der Daten verwendet werden soll, wenn auf den Link zum Sortieren des Felds "s" geklickt wird. Die GridView verfügt auch über eine `SortExpression`-Eigenschaft. Wenn auf einen LinkButton für Sortier Header geklickt wird, weist das GridView-Objekt der `SortExpression`-Eigenschaft `SortExpression` Wert zu. Anschließend werden die Daten aus der ObjectDataSource erneut abgerufen und entsprechend der GridView s-`SortExpression`-Eigenschaft sortiert. In der folgenden Liste sind die Schritte aufgeführt, die ausgeführt werden, wenn ein Endbenutzer die Daten in einer GridView-Ansicht sortiert:

1. Das GridView s- [Sortier Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) wird ausgelöst.
2. Die GridView s [`SortExpression`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) wird auf die `SortExpression` des Felds festgelegt, dessen Sortier Schaltfläche LinkButton angeklickt wurde.
3. Die ObjectDataSource Ruft alle Daten aus der BLL erneut ab und sortiert die Daten dann mithilfe der GridView s-`SortExpression`
4. Die GridView s `PageIndex`-Eigenschaft wird auf 0 zurückgesetzt. Dies bedeutet, dass beim Sortieren der Benutzer zur ersten Datenseite zurückgegeben wird (vorausgesetzt, dass die Unterstützung für die Paginierung implementiert wurde).
5. Das GridView s [`Sorted`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) wird ausgelöst.

Wie bei default Paging werden von der Standard Sortieroption *alle* Datensätze aus der BLL erneut abgerufen. Bei der Verwendung von Sortierung ohne Paging oder bei Verwendung der Sortierung mit Standardpaging gibt es keine Möglichkeit, diesen Leistungs Treffer (kurz vor dem Zwischenspeichern der Datenbankdaten) zu umgehen. Wie wir in einem zukünftigen Tutorial sehen werden, ist es jedoch möglich, Daten bei Verwendung von benutzerdefiniertem Paging effizient zu sortieren.

Beim Binden von ObjectDataSource an das GridView-Objekt in der Dropdown Liste im GridView s-Smarttag erhält jedes GridView-Feld automatisch seine `SortExpression`-Eigenschaft, die dem Namen des Datenfelds in der `ProductsRow`-Klasse zugewiesen ist. Beispielsweise wird die `ProductName` BoundField s-`SortExpression` auf `ProductName`festgelegt, wie im folgenden deklarativen Markup gezeigt:

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Ein Feld kann so konfiguriert werden, dass es nicht sortiert werden kann, indem seine `SortExpression`-Eigenschaft gelöscht wird (die Zuweisung zu einer leeren Zeichenfolge). Um dies zu veranschaulichen, stellen Sie sich vor, dass wir nicht möchten, dass unsere Kunden unsere Produkte nach Preis sortieren. Die Eigenschaft "`UnitPrice` BoundField s `SortExpression`" kann entweder aus dem deklarativen Markup oder über das Dialogfeld "Felder" entfernt werden (auf das Sie zugreifen können, indem Sie auf den Link Spalten bearbeiten im smarttaggridview s klicken).

![Die Ergebnisse wurden nach UnitPrice in aufsteigender Reihenfolge sortiert.](paging-and-sorting-report-data-vb/_static/image27.png)

**Abbildung 13**: die Ergebnisse wurden nach UnitPrice in aufsteigender Reihenfolge sortiert.

Nachdem die `SortExpression`-Eigenschaft für das `UnitPrice` BoundField entfernt wurde, wird der Header als Text und nicht als Link gerendert, sodass Benutzer die Daten nicht nach Preis sortieren können.

[![durch Entfernen der SortExpression-Eigenschaft können Benutzer die Produkte nicht mehr nach Preis sortieren.](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Abbildung 14**: indem die SortExpression-Eigenschaft entfernt wird, können Benutzer die Produkte nicht mehr nach Preis sortieren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image30.png)).

## <a name="programmatically-sorting-the-gridview"></a>Programm gesteuertes Sortieren der GridView

Sie können den Inhalt der GridView auch Programm gesteuert sortieren, indem Sie die GridView s- [`Sort`-Methode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)verwenden. Übergeben Sie einfach den `SortExpression` Wert, um den Wert zusammen mit dem [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` oder `Descending`) zu sortieren, und die GridView s-Daten werden neu sortiert.

Stellen Sie sich vor, dass der Grund, warum wir das Sortieren durch den `UnitPrice` deaktiviert haben, darauf zurückzuführen ist, dass unsere Kunden einfach nur die Produkte mit dem niedrigsten Preis kaufen würden. Wir möchten Sie jedoch ermutigen, die teuersten Produkte zu kaufen. Wir möchten also, dass Sie die Produkte nach Preis sortieren können, aber nur vom teuersten Preis bis zum geringsten Preis.

Um dies zu erreichen, fügen Sie der Seite ein Schaltflächen-websteuer Element hinzu, legen Sie dessen `ID`-Eigenschaft auf `SortPriceDescending`und die `Text`-Eigenschaft auf nach Preis sortieren fest. Erstellen Sie als nächstes einen Ereignishandler für die Schaltfläche s `Click` Ereignis, indem Sie im Designer auf das Schaltflächen-Steuerelement doppelklicken. Fügen Sie diesem Ereignishandler den folgenden Code hinzu:

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Wenn Sie auf diese Schaltfläche klicken, wird der Benutzer auf die erste Seite mit den nach Preis sortierten Produkten zurückgegeben (siehe Abbildung 15).

[Wenn Sie ![auf die Schaltfläche klicken, werden die Produkte von der teuersten zum geringsten](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Abbildung 15**: durch Klicken auf die Schaltfläche werden die Produkte von der teuersten zum geringsten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](paging-and-sorting-report-data-vb/_static/image33.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie standardmäßige Paging-und Sortierungs Funktionen implementieren, die beide so einfach sind wie das Aktivieren eines Kontrollkästchens. Wenn ein Benutzerdaten sortiert oder durchläuft, wird ein ähnlicher Workflow entwickelt:

1. Ein Postback folgt.
2. Das Data Web Control s-Ereignis vor der Ebene wird ausgelöst (`PageIndexChanging` oder `Sorting`).
3. Alle Daten werden von ObjectDataSource erneut abgerufen.
4. Das Data Web Control s-Ereignis nach der Ebene wird ausgelöst (`PageIndexChanged` oder `Sorted`)

Die Implementierung von basiertem Paging und Sortieren ist ein Kinderspiel, aber es muss mehr Aufwand unternommen werden, um das effizientere benutzerdefinierte Paging zu nutzen oder die Paging-oder Sortierungs Schnittstelle weiter zu verbessern. In zukünftigen Tutorials werden diese Themen behandelt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](creating-a-customized-sorting-user-interface-cs.md)
> [Weiter](efficiently-paging-through-large-amounts-of-data-vb.md)
