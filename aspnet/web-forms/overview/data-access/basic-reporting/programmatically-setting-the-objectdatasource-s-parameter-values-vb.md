---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Programm gesteuertes Festlegen der Parameter Werte von ObjectDataSource (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird das Hinzufügen einer Methode zu "Dal" und "BLL" untersucht, die einen einzelnen Eingabeparameter annimmt und Daten zurückgibt. Im Beispiel wird dieser Parameter festgelegt...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f1dd50f46528e8dd51f85e503604d3f0dbc21ad2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601872"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Programmgesteuertes Festlegen der Parameterwerte des ObjectDataSource-Steuerelements (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) oder [PDF herunterladen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> In diesem Tutorial wird das Hinzufügen einer Methode zu "Dal" und "BLL" untersucht, die einen einzelnen Eingabeparameter annimmt und Daten zurückgibt. Im Beispiel wird dieser Parameter Programm gesteuert festgelegt.

## <a name="introduction"></a>Einführung

Wie bereits im [vorherigen Tutorial](declarative-parameters-vb.md)gezeigt, stehen eine Reihe von Optionen für die deklarative Übergabe von Parameterwerten an die Methoden von ObjectDataSource zur Verfügung. Wenn der Parameterwert hart codiert ist, von einem websteuer Element auf der Seite stammt oder sich in einer anderen Quelle befindet, die von einer Datenquelle `Parameter` Objekt gelesen werden kann, kann dieser Wert z. b. an den Eingabeparameter gebunden werden, ohne dass eine Codezeile geschrieben werden muss.

Es kann jedoch vorkommen, dass der Parameterwert aus einer Quelle stammt, die nicht bereits von einer der integrierten Datenquellen `Parameter`-Objekten berücksichtigt wird. Wenn unsere Website Benutzerkonten unterstützt, möchten wir möglicherweise den Parameter basierend auf der Benutzer-ID des aktuell angemeldeten Besuchers festlegen. Möglicherweise müssen Sie auch den Parameterwert anpassen, bevor Sie ihn an die-Methode des zugrunde liegenden Objekts von ObjectDataSource senden.

Wenn die `Select`-Methode von ObjectDataSource aufgerufen wird, löst ObjectDataSource zuerst das [auswählende Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)aus. Die-Methode des zugrunde liegenden Objekts von ObjectDataSource wird dann aufgerufen. Nach Abschluss dieses Vorgangs wird das [ausgewählte Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) von ObjectDataSource ausgelöst (Abbildung 1 veranschaulicht diese Abfolge von Ereignissen). Die Parameterwerte, die an die-Methode des zugrunde liegenden Objekts von ObjectDataSource übergeben werden, können in einem Ereignishandler für das `Selecting`-Ereignis festgelegt oder angepasst werden.

[![das ausgewählte ObjectDataSource-Ereignis und das Auswählen von Ereignissen vor und nach dem Aufrufen der-Methode des zugrunde liegenden Objekts ausgelöst werden.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Abbildung 1**: die `Selected`-und `Selecting` Ereignisse von ObjectDataSource werden vor und nach dem Aufrufen der-Methode des zugrunde liegenden Objekts ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png)).

In diesem Tutorial wird das Hinzufügen einer Methode zu unserer dal und BLL beschrieben, die einen einzelnen Eingabeparameter `Month`vom Typ `Integer` akzeptiert und ein `EmployeesDataTable` Objekt zurückgibt, das mit den Mitarbeitern aufgefüllt wird, deren Einstellungs Jahrestag in der angegebenen `Month`ist. In unserem Beispiel wird dieser Parameter Programm gesteuert basierend auf dem aktuellen Monat festgelegt, und es wird eine Liste der "Mitarbeiter Jubiläen in diesem Monat" angezeigt.

Fangen wir an!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Schritt 1: Hinzufügen einer Methode zum`EmployeesTableAdapter`

Im ersten Beispiel müssen wir eine Möglichkeit zum Abrufen der Mitarbeiter hinzufügen, deren `HireDate` in einem bestimmten Monat aufgetreten sind. Um diese Funktionalität in Übereinstimmung mit unserer Architektur bereitzustellen, müssen wir zunächst eine Methode in `EmployeesTableAdapter` erstellen, die der entsprechenden SQL-Anweisung zugeordnet ist. Um dies zu erreichen, öffnen Sie zunächst das typisierte Northwind-DataSet. Klicken Sie mit der rechten Maustaste auf die `EmployeesTableAdapter` Bezeichnung, und wählen Sie Abfrage hinzufügen.

[![dem Mitarbeiter Adapter eine neue Abfrage hinzufügen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen Abfrage zum `EmployeesTableAdapter` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))

Fügen Sie eine SQL-Anweisung hinzu, die Zeilen zurückgibt. Wenn Sie den Bildschirm `SELECT` Anweisung angeben erreichen, wird die standardmäßige `SELECT` Anweisung für die `EmployeesTableAdapter` bereits geladen. Fügen Sie einfach in der `WHERE`-Klausel hinzu: `WHERE DATEPART(m, HireDate) = @Month`. [Datepart](https://msdn.microsoft.com/library/ms174420.aspx) ist eine T-SQL-Funktion, die einen bestimmten Datums Teil eines `datetime` Typs zurückgibt. in diesem Fall verwenden wir `DATEPART`, um den Monat der `HireDate` Spalte zurückzugeben.

[![nur die Zeilen zurückgeben, bei denen die HireDate-Spalte kleiner oder gleich dem Parameter @HiredBeforeDate ist.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Abbildung 3**: nur die Zeilen zurückgeben, bei denen die `HireDate` Spalte kleiner oder gleich dem Parameter "`@HiredBeforeDate`" ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))

Ändern Sie abschließend die `FillBy`-und `GetDataBy` Methodennamen in `FillByHiredDateMonth` bzw. `GetEmployeesByHiredDateMonth`.

[Wählen Sie ![geeignetere Methodennamen als FillBy und GetDataBy aus.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Abbildung 4**: Wählen Sie besser geeignete Methodennamen als `FillBy` und `GetDataBy` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))

Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen und zur Entwurfs Oberfläche des Datasets zurückzukehren. Der `EmployeesTableAdapter` sollte nun einen neuen Satz von Methoden für den Zugriff auf Mitarbeiter enthalten, die in einem bestimmten Monat eingestellt wurden.

[![die neuen Methoden in der Designoberfläche des Datasets angezeigt werden.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Abbildung 5**: die neuen Methoden werden im Designoberfläche des Datasets angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))

## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Schritt 2: Hinzufügen der`GetEmployeesByHiredDateMonth(month)`Methode zur Geschäftslogik Schicht

Da unsere Anwendungsarchitektur eine separate Ebene für die Geschäftslogik und die Datenzugriffs Logik verwendet, muss unserer BLL eine Methode hinzugefügt werden, die zum Abrufen von Mitarbeitern verwendet wird, die vor einem bestimmten Datum eingestellt wurden. Öffnen Sie die Datei `EmployeesBLL.vb`, und fügen Sie die folgende Methode hinzu:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Wie bei den anderen Methoden in dieser Klasse ruft `GetEmployeesByHiredDateMonth(month)` einfach die DAL auf und gibt die Ergebnisse zurück.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Schritt 3: Anzeigen von Mitarbeitern, deren Einstellungs Jahrestag dieser Monat ist

Der letzte Schritt in diesem Beispiel besteht darin, die Mitarbeiter anzuzeigen, deren Einstellungs Jahrestag in diesem Monat liegt. Fügen Sie zunächst der Seite `ProgrammaticParams.aspx` im `BasicReporting` Ordner eine GridView hinzu, und fügen Sie eine neue ObjectDataSource als Datenquelle hinzu. Konfigurieren Sie ObjectDataSource so, dass die `EmployeesBLL`-Klasse verwendet wird, wobei die `SelectMethod` auf `GetEmployeesByHiredDateMonth(month)`festgelegt ist.

[![die Klasse "Mitarbeiter-BLL" verwenden](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Abbildung 6**: Verwenden der `EmployeesBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))

[![aus der getemployeesbyhireddatemonth (month)-Methode auswählen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Abbildung 7**: auswählen aus der `GetEmployeesByHiredDateMonth(month)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))

Im letzten Bildschirm werden wir aufgefordert, die Quelle des `month` Parameter Werts anzugeben. Da dieser Wert Programm gesteuert festgelegt wird, lassen Sie die Parameter Quelle auf die Standardoption None fest, und klicken Sie auf Fertigstellen.

[![lassen Sie die Parameter Quelle auf keine fest.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Abbildung 8**: belassen der Parameter Quelle auf "None" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))

Dadurch wird ein `Parameter` Objekt in der `SelectParameters` Auflistung von ObjectDataSource erstellt, für das kein Wert angegeben wurde.

[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Um diesen Wert Programm gesteuert festzulegen, müssen wir einen Ereignishandler für das `Selecting`-Ereignis von ObjectDataSource erstellen. Um dies zu erreichen, wechseln Sie zum Designansicht, und doppelklicken Sie auf ObjectDataSource. Wählen Sie alternativ ObjectDataSource aus, wechseln Sie zum Eigenschaftenfenster, und klicken Sie dann auf das Blitz Symbol. Doppelklicken Sie anschließend entweder auf das Textfeld neben dem `Selecting`-Ereignis, oder geben Sie den Namen des Ereignis Handlers ein, den Sie verwenden möchten. Als dritte Möglichkeit können Sie den Ereignishandler erstellen, indem Sie die ObjectDataSource und das zugehörige `Selecting` Ereignis aus den beiden Dropdown Listen am oberen Rand der Code Behind-Klasse der Seite auswählen.

![Klicken Sie im Eigenschaften Fenster auf das Blitz Symbol, um die Ereignisse eines websteuer Elements aufzulisten.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Abbildung 9**: Klicken Sie im Eigenschaften Fenster auf das Blitz Symbol, um die Ereignisse eines websteuer Elements aufzulisten.

Bei allen drei Ansätzen wird der Code-Behind-Klasse der Seite ein neuer Ereignishandler für das `Selecting`-Ereignis der Seite hinzugefügt. In diesem Ereignishandler können wir mit `e.InputParameters(parameterName)`Lese-und Schreibvorgänge für die Parameterwerte durchführen, wobei *`parameterName`* der Wert des `Name`-Attributs im `<asp:Parameter>`-Tag ist (die `InputParameters` Auflistung kann auch ordnungsgemäß indiziert werden, wie in `e.InputParameters(index)`). Um den `month`-Parameter auf den aktuellen Monat festzulegen, fügen Sie dem `Selecting`-Ereignishandler Folgendes hinzu:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Wenn Sie diese Seite über einen Browser besuchen, sehen wir, dass nur ein Mitarbeiter diesen Monat (März) Laura Callahan eingestellt hat, der seit 1994 seit.

[![Sie die Mitarbeiter, deren Jubiläen in diesem Monat angezeigt werden.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Abbildung 10**: Mitarbeiter, deren Jubiläen in diesem Monat angezeigt werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))

## <a name="summary"></a>Summary

Obwohl die Parameterwerte der ObjectDataSource in der Regel deklarativ festgelegt werden können, ohne dass eine Codezeile erforderlich ist, ist es einfach, die Parameterwerte Programm gesteuert festzulegen. Wir müssen lediglich einen Ereignishandler für das `Selecting`-Ereignis von ObjectDataSource erstellen, das ausgelöst wird, bevor die-Methode des zugrunde liegenden Objekts aufgerufen wird, und die Werte für einen oder mehrere Parameter manuell über die `InputParameters` Auflistung festlegen.

Dieses Tutorial schließt den grundlegenden Bericht Erstellungs Abschnitt ab. Im [nächsten Tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) wird der Abschnitt Filter und Master-Details-Szenarios vorgestellt, in dem wir die Verfahren zum Filtern von Daten und Ausführen eines Drilldowns von einem Master Bericht in einen Detail Bericht untersuchen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Prüfer für dieses Tutorial war Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](declarative-parameters-vb.md)
