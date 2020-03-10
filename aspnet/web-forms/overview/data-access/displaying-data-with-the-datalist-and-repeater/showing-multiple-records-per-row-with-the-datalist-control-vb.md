---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Anzeigen mehrerer Datensätze pro Zeile mit dem DataList-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem kurzen Tutorial erfahren Sie, wie Sie das Layout des DataList-Layouts mithilfe der Eigenschaften RepeatColumns und RepeatDirection anpassen.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 17283dae192896fbaa48f1d7fe49afdbaf4c9a02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78495303"
---
# <a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Anzeigen von mehreren Datensätzen pro Zeile mit dem DataList-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) oder [PDF herunterladen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> In diesem kurzen Tutorial erfahren Sie, wie Sie das Layout des DataList-Layouts mithilfe der Eigenschaften RepeatColumns und RepeatDirection anpassen.

## <a name="introduction"></a>Einführung

Die in den letzten beiden Tutorials dargestellten DataList-Beispiele haben jeden Datensatz aus seiner Datenquelle als Zeile in einer einspaltigen HTML-`<table>`gerendert. Obwohl es sich hierbei um das Standardverhalten von DataList handelt, ist es sehr einfach, die DataList-Anzeige so anzupassen, dass die Datenquellen Elemente auf eine mehrzeilige Tabelle mit mehreren Zeilen verteilt werden. Darüber hinaus ist es möglich, alle Datenquellen Elemente in einem einzeiligen, mehrspaltigen DataList anzuzeigen.

Wir können das DataList s-Layout über seine `RepeatColumns`-und `RepeatDirection` Eigenschaften anpassen, die angeben, wie viele Spalten gerendert werden und ob diese Elemente vertikal oder horizontal angeordnet werden. Abbildung 1 zeigt beispielsweise einen DataList, der Produktinformationen in einer Tabelle mit drei Spalten anzeigt.

[![dem DataList werden drei Produkte pro Zeile angezeigt.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Abbildung 1**: DataList zeigt drei Produkte pro Zeile an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))

Durch das Anzeigen mehrerer Datenquellen Elemente pro Zeile kann der DataList den horizontalen Bildschirmbereich effektiver nutzen. In diesem kurzen Tutorial untersuchen wir diese beiden DataList-Eigenschaften.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Schritt 1: Anzeigen von Produktinformationen in einem DataList

Bevor wir die Eigenschaften `RepeatColumns` und `RepeatDirection` überprüfen, können Sie zuerst auf unserer Seite einen DataList erstellen, der Produktinformationen mithilfe des standardmäßigen einspaltigen Tabellenlayouts mit mehreren Zeilen auflistet. In diesem Beispiel können Sie den Namen, die Kategorie und den Preis des Produkts mit dem folgenden Markup anzeigen:

[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Wir haben gesehen, wie Daten in vorherigen Beispielen an einen DataList gebunden werden. Daher werde ich diese Schritte schnell durchgehen. Öffnen Sie zunächst die Seite `RepeatColumnAndDirection.aspx` im Ordner `DataListRepeaterBasics`, und ziehen Sie einen DataList aus der Toolbox auf den Designer. Wählen Sie aus dem DataList s-Smarttag eine neue ObjectDataSource aus, und konfigurieren Sie Sie so, dass Sie die Daten aus der `ProductsBLL` Klasse `GetProducts`-Methode per Pull abruft, und wählen Sie auf den Registerkarten einfügen, aktualisieren und Löschen des Assistenten die Option (keine) aus.

Nach dem Erstellen und Binden der neuen ObjectDataSource an den DataList erstellt Visual Studio automatisch eine `ItemTemplate`, die den Namen und den Wert für jedes der Product Data-Felder anzeigt. Passen Sie die `ItemTemplate` entweder direkt über das deklarative Markup oder die Option Vorlagen bearbeiten im smarttagtag DataList an, sodass Sie das oben gezeigte Markup verwendet. ersetzen Sie dabei den *Produktnamen*, den *Kategorienamen*und den *Preis* Text durch Label-Steuerelemente, die die entsprechende Datenbindung-Syntax verwenden, um Ihren `Text` Eigenschaften Werte zuzuweisen. Nachdem Sie die `ItemTemplate`aktualisiert haben, sollte Ihr deklaratives Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Beachten Sie, dass ein Format Bezeichner in die `Eval` Datenbindung-Syntax für die `UnitPrice`eingeschlossen wurde, wobei der zurückgegebene Wert als Währungs `Eval("UnitPrice", "{0:C}").` formatiert wird.

Nehmen Sie sich einen Moment Zeit, um Ihre Seite in einem Browser zu besuchen. Wie in Abbildung 2 gezeigt, wird der DataList als einspaltige Tabelle mit mehreren Zeilen gerendert.

[![Standardmäßig rendert DataList als einspaltige Tabelle mit mehreren Zeilen.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Abbildung 2**: Standardmäßig wird der DataList als einspaltige Tabelle mit mehreren Zeilen gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))

## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Schritt 2: Ändern der Layoutrichtung des DataList-s

Das Standardverhalten für den DataList besteht darin, seine Elemente vertikal in einer einspaltigen Tabelle mit mehreren Zeilen anzuordnen. dieses Verhalten kann jedoch problemlos über die DataList s- [`RepeatDirection`-Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)geändert werden. Die `RepeatDirection`-Eigenschaft kann einen von zwei möglichen Werten annehmen: `Horizontal` oder `Vertical` (Standardeinstellung).

Wenn die `RepeatDirection`-Eigenschaft von `Vertical` in `Horizontal`geändert wird, rendert der DataList seine Datensätze in einer einzelnen Zeile und erstellt eine Spalte pro Datenquellen Element. Um diesen Effekt zu veranschaulichen, klicken Sie im Designer auf den DataList und ändern dann aus dem Eigenschaftenfenster die `RepeatDirection`-Eigenschaft von `Vertical` in `Horizontal`. Unmittelbar danach passt der Designer das Layout des DataList-s an und erstellt eine einzeilige, mehrspaltige Schnittstelle (siehe Abbildung 3).

[![die RepeatDirection-Eigenschaft bestimmt, wie die Richtung der DataList-Elemente festgelegt wird.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Abbildung 3**: die `RepeatDirection`-Eigenschaft legt fest, wie die Richtung der DataList-Elemente bestimmt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png)).

Wenn Sie kleine Datenmengen anzeigen, ist eine einzeilige, mehrspaltige Tabelle möglicherweise eine ideale Möglichkeit, um die Bildschirmfläche zu maximieren. Bei größeren Datenmengen erfordert eine einzelne Zeile jedoch zahlreiche Spalten, mit denen die Elemente, die auf dem Bildschirm nach rechts passen, nach rechts übertragen werden. In Abbildung 4 werden die Produkte angezeigt, wenn Sie in einem DataList mit einer Zeile gerendert werden. Da es zahlreiche Produkte gibt (über 80), muss der Benutzer einen Bildlauf nach rechts durchführen, um Informationen zu den einzelnen Produkten anzuzeigen.

[![für ausreichend große Datenquellen ist für einen DataList mit einer einzelnen Spalte ein horizontaler Bildlauf erforderlich.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Abbildung 4**: für ausreichend große Datenquellen ist für einen DataList mit einer einzelnen Spalte ein horizontaler[Bildlauf erforderlich (Klicken Sie, um das Bild in voller Größe anzuzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))

## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Schritt 3: Anzeigen von Daten in einer mehrzeiligen Tabelle mit mehreren Zeilen

Um einen mehrzeiligen DataList mit mehreren Zeilen zu erstellen, müssen wir die`RepeatColumns`- [Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) auf die Anzahl der anzuzeigenden Spalten festlegen. Standardmäßig wird die `RepeatColumns`-Eigenschaft auf 0 festgelegt, sodass der DataList alle Elemente in einer einzelnen Zeile oder Spalte anzeigt (abhängig vom Wert der `RepeatDirection`-Eigenschaft).

In unserem Beispiel werden drei Produkte pro Tabellenzeile angezeigt. Legen Sie daher die `RepeatColumns`-Eigenschaft auf 3 fest. Nachdem Sie diese Änderung vorgenommen haben, nehmen Sie sich einen Moment Zeit, um die Ergebnisse in einem Browser anzuzeigen. Wie in Abbildung 5 gezeigt, werden die Produkte nun in einer drei spaltigen Tabelle mit mehreren Zeilen aufgelistet.

[![drei Produkte pro Zeile angezeigt werden.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Abbildung 5**: drei Produkte werden pro Zeile angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))

Die `RepeatDirection`-Eigenschaft wirkt sich darauf aus, wie die Elemente im DataList angelegt werden. Abbildung 5 zeigt die Ergebnisse, bei denen die `RepeatDirection`-Eigenschaft auf `Horizontal`festgelegt ist. Beachten Sie, dass die ersten drei Produkte Chai, Chang und Aniseed-Sirup von links nach rechts und von oben nach unten angeordnet werden. Die nächsten drei Produkte (beginnend mit Chef Anton s Cajun Seasoning) werden in einer Zeile unterhalb der ersten drei Produkte angezeigt. Wenn Sie die `RepeatDirection`-Eigenschaft wieder in `Vertical`ändern, werden diese Produkte von oben nach unten, von links nach rechts, in Abbildung 6 dargestellt.

[![hier werden die Produkte vertikal angelegt.](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Abbildung 6**: Hier werden die Produkte vertikal angeordnet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))

Die Anzahl der Zeilen, die in der resultierenden Tabelle angezeigt werden, hängt von der Anzahl der Datensätze ab, die an den DataList gebunden sind. Genau, ist die Obergrenze der Gesamtzahl der Datenquellen Elemente dividiert durch den `RepeatColumns`-Eigenschafts Wert. Da die `Products` Tabelle derzeit 84 Produkte enthält, die durch 3 teilbar sind, gibt es 28 Zeilen. Wenn die Anzahl der Elemente in der Datenquelle und der `RepeatColumns`-Eigenschafts Wert nicht teilbar sind, enthält die letzte Zeile oder Spalte leere Zellen. Wenn die `RepeatDirection` auf `Vertical`festgelegt ist, enthält die letzte Spalte leere Zellen. Wenn `RepeatDirection` `Horizontal`ist, enthält die letzte Zeile die leeren Zellen.

## <a name="summary"></a>Zusammenfassung

Der DataList listet standardmäßig seine Elemente in einer einspaltigen Tabelle mit mehreren Zeilen auf, die das Layout eines GridView-Objekts mit einem einzelnen TemplateField imitiert. Obwohl dieses Standardlayout akzeptabel ist, können wir die Bildschirmfläche maximieren, indem wir mehrere Datenquellen Elemente pro Zeile anzeigen. Dies ist einfach das Festlegen der DataList s `RepeatColumns`-Eigenschaft auf die Anzahl der Spalten, die pro Zeile angezeigt werden. Darüber hinaus kann die DataList s-`RepeatDirection` Eigenschaft verwendet werden, um anzugeben, ob der Inhalt der mehrzeiligen Tabelle mit mehreren Zeilen horizontal von links nach rechts, von oben nach unten oder vertikal von oben nach unten, von links nach rechts angeordnet werden soll.

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war John suru. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Weiter](nested-data-web-controls-vb.md)
