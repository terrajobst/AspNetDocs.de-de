---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Eine Übersicht über das Einfügen, aktualisieren und Löschen von DatenC#() | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Lernprogramm erfahren Sie, wie Sie die Methoden Insert (), Update () und DELETE () von ObjectDataSource den Methoden der BLL-Klassen zuordnen und wie Sie mit "configu...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e26b8e841f86272a158b0c09b62ab2790d01d191
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74571418"
---
# <a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Eine Übersicht über das Einfügen, aktualisieren und Löschen von DatenC#()

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) oder [PDF herunterladen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> In diesem Tutorial erfahren Sie, wie Sie die Methoden Insert (), Update () und DELETE () von ObjectDataSource den Methoden der BLL-Klassen zuordnen und wie Sie die Steuerelemente GridView, DetailsView und FormView konfigurieren, um Daten Änderungs Funktionen bereitzustellen.

## <a name="introduction"></a>Einführung

In den letzten Tutorials haben wir untersucht, wie Daten auf einer ASP.NET-Seite mithilfe der GridView-, DetailsView-und FormView-Steuerelemente angezeigt werden. Diese Steuerelemente funktionieren einfach mit den Ihnen bereitgestellten Daten. Diese Steuerelemente greifen in der Regel über die Verwendung eines Datenquellen-Steuer Elements, wie z. b. ObjectDataSource, auf Daten zu. Wir haben gesehen, wie ObjectDataSource als Proxy zwischen der ASP.NET-Seite und den zugrunde liegenden Daten fungiert. Wenn eine GridView Daten anzeigen muss, ruft Sie die `Select()`-Methode von ObjectDataSource auf, die wiederum eine Methode von unserer Geschäftslogik Schicht (BLL) aufruft, die eine Methode im TableAdapter der entsprechenden Datenzugriffs Schicht aufruft, der wiederum eine `SELECT` Abfrage an die Datenbank Northwind sendet.

Beachten Sie, dass Visual Studio beim Erstellen der TableAdapters in der dal im [ersten Tutorial](../introduction/creating-a-data-access-layer-cs.md)automatisch Methoden zum Einfügen, aktualisieren und Löschen von Daten aus der zugrunde liegenden Datenbanktabelle hinzugefügt hat. Außerdem haben wir beim [Erstellen einer Geschäftslogik Schicht](../introduction/creating-a-business-logic-layer-cs.md) Methoden in der BLL entworfen, die diese Daten Änderungs Methoden aufgerufen haben.

Neben der `Select()`-Methode verfügt auch ObjectDataSource über `Insert()`-, `Update()`-und `Delete()`-Methoden. Wie die `Select()`-Methode können diese drei Methoden Methoden in einem zugrunde liegenden-Objekt zugeordnet werden. Bei der Konfiguration für das Einfügen, aktualisieren oder Löschen von Daten stellen die Steuerelemente GridView, DetailsView und FormView eine Benutzeroberfläche zum Ändern der zugrunde liegenden Daten bereit. Diese Benutzeroberfläche Ruft die Methoden `Insert()`, `Update()`und `Delete()` von ObjectDataSource auf, die dann die zugeordneten Methoden des zugrunde liegenden Objekts aufrufen (siehe Abbildung 1).

[![die Insert ()-, Update ()-und DELETE ()-Methoden von ObjectDataSource als Proxy für den BLL fungieren.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Abbildung 1**: die Methoden `Insert()`, `Update()`und `Delete()` von ObjectDataSource dienen als Proxy in der BLL ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))

In diesem Lernprogramm erfahren Sie, wie die Methoden `Insert()`, `Update()`und `Delete()` von ObjectDataSource den Methoden der Klassen in der BLL zugeordnet werden, und wie die Steuerelemente GridView, DetailsView und FormView konfiguriert werden, um Daten Änderungs Funktionen bereitzustellen.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Schritt 1: Erstellen der Tutorials zum Einfügen, aktualisieren und Löschen von Tutorials

Bevor wir uns mit dem Einfügen, aktualisieren und Löschen von Daten beschäftigen, nehmen wir uns zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen, und die nächsten. Fügen Sie zunächst einen neuen Ordner mit dem Namen `EditInsertDelete`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Fügen Sie die ASP.NET-Seiten für die Daten Änderungs bezogenen Tutorials hinzu.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen der ASP.NET-Seiten für die Daten Änderungs bezogenen Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `EditInsertDelete` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Designansicht der Seite ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))

Fügen Sie abschließend die Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem angepassten Formatierungs `<siteMapNode>`das folgende Markup hinzu:

[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials zum Bearbeiten, einfügen und löschen.

![Die Site Übersicht enthält nun Einträge für die Tutorials zum Bearbeiten, einfügen und löschen.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Abbildung 4**: die Site Übersicht enthält jetzt Einträge für die Tutorials zum Bearbeiten, einfügen und löschen.

## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Schritt 2: Hinzufügen und Konfigurieren des ObjectDataSource-Steuer Elements

Da sich GridView, DetailsView und FormView jeweils in den Funktionen und dem Layout der Datenänderung unterscheiden, betrachten wir jede einzelne. Anstatt jedes Steuerelement mit einem eigenen ObjectDataSource-Steuerelement zu verwenden, erstellen wir lediglich eine einzelne ObjectDataSource, die von allen drei Steuerelement Beispielen gemeinsam genutzt werden kann.

Öffnen Sie die Seite `Basics.aspx`, ziehen Sie eine ObjectDataSource aus der Toolbox auf den Designer, und klicken Sie auf den Link Datenquelle konfigurieren des Smarttags. Da der `ProductsBLL` die einzige BLL-Klasse ist, die das Bearbeiten, einfügen und Löschen von Methoden bereitstellt, konfigurieren Sie ObjectDataSource für die Verwendung dieser Klasse.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbll-Klasse.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))

Im nächsten Bildschirm können Sie angeben, welche Methoden der `ProductsBLL` Klasse der `Select()`, `Insert()`, `Update()`und `Delete()` von ObjectDataSource zugeordnet werden sollen. Wählen Sie hierzu die entsprechende Registerkarte aus, und wählen Sie in der Dropdown Liste die gewünschte Methode aus. In Abbildung 6, die jetzt vertraut sein sollte, wird die `Select()`-Methode von ObjectDataSource der `GetProducts()`-Methode der `ProductsBLL` Klasse zugeordnet. Sie können die Methoden `Insert()`, `Update()`und `Delete()` konfigurieren, indem Sie die entsprechende Registerkarte in der Liste oben auswählen.

[![von ObjectDataSource alle Produkte zurückgeben](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Abbildung 6**: lassen, dass die ObjectDataSource alle Produkte zurückgibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))

In Abbildung 7, 8 und 9 werden die Registerkarten Update, INSERT und DELETE von ObjectDataSource angezeigt. Konfigurieren Sie diese Registerkarten so, dass die Methoden `Insert()`, `Update()`und `Delete()` die Methoden `UpdateProduct`, `AddProduct`und `DeleteProduct` der `ProductsBLL` Klasse aufrufen.

[![ordnen Sie die Update ()-Methode von ObjectDataSource der "UpdateProduct"-Methode der productbll-Klasse zu.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Abbildung 7**: Zuordnen der `Update()` Methode von ObjectDataSource zur `UpdateProduct`-Methode der `ProductBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))

[![ordnen Sie die Insert ()-Methode von ObjectDataSource der addProduct-Methode der productbll-Klasse zu.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Abbildung 8**: Zuordnen der `Insert()` Methode der ObjectDataSource zur Add `Product`-Methode der `ProductBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))

[![ordnen Sie die Delete ()-Methode von ObjectDataSource der deleteProduct-Methode der productbll-Klasse zu.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Abbildung 9**: Zuordnen der `Delete()`-Methode der ObjectDataSource zur `DeleteProduct`-Methode der `ProductBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))

Möglicherweise haben Sie bemerkt, dass in den Dropdown Listen auf den Registerkarten aktualisieren, einfügen und Löschen bereits diese Methoden ausgewählt wurden. Dies liegt an der Verwendung der `DataObjectMethodAttribute`, die die Methoden von `ProductsBLL`ergänzt. Die deleteProduct-Methode hat z. b. die folgende Signatur:

[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

Das `DataObjectMethodAttribute` Attribut gibt den Zweck jeder Methode an, unabhängig davon, ob es sich um das auswählen, einfügen, aktualisieren oder löschen handelt und ob es sich um den Standardwert handelt. Wenn Sie diese Attribute beim Erstellen der BLL-Klassen ausgelassen haben, müssen Sie die Methoden auf den Registerkarten aktualisieren, einfügen und löschen manuell auswählen.

Nachdem Sie sichergestellt haben, dass die entsprechenden `ProductsBLL` Methoden den Methoden `Insert()`, `Update()`und `Delete()` von ObjectDataSource zugeordnet sind, klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

## <a name="examining-the-objectdatasources-markup"></a>Überprüfen des Markups von ObjectDataSource

Nachdem Sie die ObjectDataSource mithilfe des Assistenten konfiguriert haben, wechseln Sie zur Quell Ansicht, um das generierte deklarative Markup zu untersuchen. Das `<asp:ObjectDataSource>`-Tag gibt das zugrunde liegende-Objekt und die aufzurufenden Methoden an. Außerdem gibt es `DeleteParameters`, `UpdateParameters`und `InsertParameters`, die den Eingabe Parametern für die Methoden `AddProduct`, `UpdateProduct`und `DeleteProduct` der `ProductsBLL` Klasse zugeordnet sind:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

Die ObjectDataSource enthält einen Parameter für jeden Eingabeparameter für die zugehörigen Methoden, ebenso wie eine Liste von `SelectParameter` s, wenn die ObjectDataSource so konfiguriert ist, dass eine Select-Methode aufgerufen wird, die einen Eingabeparameter erwartet (z. b. `GetProductsByCategoryID(categoryID)`). Wie wir sehen werden, werden die Werte für diese `DeleteParameters`, `UpdateParameters`und `InsertParameters` automatisch von GridView, DetailsView und FormView festgelegt, bevor die `Insert()`-, `Update()`-oder `Delete()`-Methode von ObjectDataSource aufgerufen wird. Diese Werte können auch Programm gesteuert festgelegt werden, wie in einem zukünftigen Tutorial erläutert wird.

Ein Nebeneffekt der Verwendung des Assistenten für die Konfiguration von ObjectDataSource besteht darin, dass Visual Studio die [OldValuesParameterFormatString-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) auf `original_{0}`festlegt. Dieser Eigenschafts Wert wird verwendet, um die ursprünglichen Werte der bearbeiteten Daten einzuschließen, und ist in zwei Szenarien hilfreich:

- Wenn ein Datensatz bearbeitet wird, können Benutzer den Wert des Primärschlüssels ändern. In diesem Fall müssen sowohl der neue Primärschlüssel Wert als auch der ursprüngliche Primärschlüssel Wert bereitgestellt werden, damit der Datensatz mit dem ursprünglichen Primärschlüssel Wert gefunden werden kann und der Wert entsprechend aktualisiert wird.
- Bei Verwendung der optimistischen Parallelität. Optimistische Parallelität ist eine Technik, mit der sichergestellt wird, dass zwei gleichzeitige Benutzer die Änderungen nicht überschreiben, und ist das Thema für ein zukünftiges Tutorial.

Die `OldValuesParameterFormatString`-Eigenschaft gibt den Namen der Eingabeparameter in den Update-und Delete-Methoden des zugrunde liegenden Objekts für die ursprünglichen Werte an. Wir besprechen diese Eigenschaft und deren Zweck ausführlicher, wenn wir die optimistische Parallelität untersuchen. Ich gebe Sie jetzt ein, da unsere BLL-Methoden nicht die ursprünglichen Werte erwarten. Daher ist es wichtig, dass diese Eigenschaft entfernt wird. Wenn die `OldValuesParameterFormatString`-Eigenschaft auf einen anderen Wert als den Standardwert (`{0}`) festgelegt wird, wird ein Fehler ausgelöst, wenn ein datenweb Steuerelement versucht, die `Update()` oder `Delete()` Methoden von ObjectDataSource aufzurufen, da die ObjectDataSource versucht, sowohl den `UpdateParameters` als auch den ursprünglichen Wert Parameter zu übergeben.

Wenn dies an diesem Punkt nicht besonders klar ist, machen Sie sich keine Sorgen. Wir werden diese Eigenschaft und das zugehörige Hilfsprogramm in einem zukünftigen Tutorial untersuchen. Stellen Sie vorerst einfach sicher, dass Sie diese Eigenschaften Deklaration vollständig aus der deklarativen Syntax entfernen, oder legen Sie den Wert auf den Standardwert ({0}) fest.

> [!NOTE]
> Wenn Sie einfach den `OldValuesParameterFormatString`-Eigenschafts Wert aus der Eigenschaftenfenster im Designansicht löschen, ist die Eigenschaft weiterhin in der deklarativen Syntax vorhanden, wird jedoch auf eine leere Zeichenfolge festgelegt. Dies führt leider immer noch zu dem oben beschriebenen Problem. Entfernen Sie daher die-Eigenschaft vollständig aus der deklarativen Syntax, oder legen Sie aus dem Eigenschaftenfenster den Wert auf den Standardwert `{0}`fest.

## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Schritt 3: Hinzufügen eines datenweb-Steuer Elements und dessen Konfiguration für die Datenänderung

Nachdem die ObjectDataSource der Seite hinzugefügt und konfiguriert wurde, können Sie der Seite Daten-websteuer Elemente hinzufügen, um die Daten anzuzeigen und eine Möglichkeit für den Endbenutzer bereitzustellen, ihn zu ändern. Wir betrachten die GridView, DetailsView und FormView separat, da sich diese datenweb Steuerelemente in den Funktionen und der Konfiguration der Datenänderung unterscheiden.

Wie wir im restlichen Teil dieses Artikels sehen werden, ist das Hinzufügen sehr einfacher Bearbeitungs-, Einfüge-und Lösch Unterstützung über die Steuerelemente GridView, DetailsView und FormView wirklich einfach wie das Überprüfen von einigen Kontrollkästchen. In der Praxis gibt es zahlreiche Feinheiten und randfälle, die die Bereitstellung dieser Funktionalität erhöhen, als nur Punkte und klicken. Dieses Tutorial konzentriert sich jedoch ausschließlich auf die unter Beweis von vereinfachten Daten Änderungs Funktionen. In zukünftigen Tutorials werden Aspekte untersucht, die zweifellos in einer realen Umgebung auftreten werden.

## <a name="deleting-data-from-the-gridview"></a>Löschen von Daten aus der GridView

Beginnen Sie, indem Sie eine GridView aus der Toolbox auf den Designer ziehen. Binden Sie anschließend die ObjectDataSource an die GridView, indem Sie Sie in der Dropdown Liste im Smarttags von GridView auswählen. An diesem Punkt wird das deklarative Markup der GridView wie folgt lauten:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Das Binden der GridView an die ObjectDataSource über das Smarttag hat zwei Vorteile:

- Boundfields und checkboxfields werden für jedes der von ObjectDataSource zurückgegebenen Felder automatisch erstellt. Darüber hinaus werden die Eigenschaften von BoundField und CheckBoxField basierend auf den Metadaten des zugrunde liegenden Felds festgelegt. Beispielsweise werden die Felder `ProductID`, `CategoryName`und `SupplierName` im `ProductsDataTable` als schreibgeschützt gekennzeichnet und sollten daher beim Bearbeiten nicht aktualisierbar sein. Um dies zu ermöglichen, werden die schreibgeschützten [Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) von boundfields auf `true`festgelegt.
- Die [DataKeyNames-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) wird den Primärschlüssel Feldern des zugrunde liegenden Objekts zugewiesen. Dies ist erforderlich, wenn die GridView zum Bearbeiten oder Löschen von Daten verwendet wird, da diese Eigenschaft das Feld (oder den Satz von Feldern) angibt, das die einzelnen Datensätze eindeutig identifiziert. Weitere Informationen zur `DataKeyNames`-Eigenschaft finden Sie unter " [Master/Detail" mithilfe eines auswählbaren mastergridview-Objekts mit einem Detail DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) -Tutorial.

Die GridView kann zwar über die Eigenschaftenfenster oder deklarative Syntax an die ObjectDataSource gebunden werden. dabei müssen Sie jedoch das entsprechende BoundField-und `DataKeyNames` Markup manuell hinzufügen.

Das GridView-Steuerelement bietet integrierte Unterstützung für das Bearbeiten und löschen auf Zeilenebene. Das Konfigurieren einer GridView zum unterstützen von Löschvorgang fügt eine Spalte mit Schaltflächen Löschen hinzu. Wenn der Endbenutzer auf die Schaltfläche "Löschen" für eine bestimmte Zeile klickt, folgt ein Postback, und die GridView führt die folgenden Schritte aus:

1. Die `DeleteParameters` Werte der ObjectDataSource werden zugewiesen.
2. Die `Delete()`-Methode von ObjectDataSource wird aufgerufen, wobei der angegebene Datensatz gelöscht wird.
3. Die GridView bindet sich an die ObjectDataSource zurück, indem Sie Ihre `Select()`-Methode aufruft.

Die Werte, die dem `DeleteParameters` zugewiesen werden, sind die Werte der `DataKeyNames` Felder für die Zeile, auf deren Lösch Schaltfläche geklickt wurde. Daher ist es wichtig, dass die `DataKeyNames`-Eigenschaft einer GridView korrekt festgelegt ist. Wenn Sie nicht vorhanden ist, wird dem `DeleteParameters` in Schritt 1 ein `null` Wert zugewiesen, der in Schritt 2 wiederum zu gelöschten Datensätzen führt.

> [!NOTE]
> Die `DataKeys` Auflistung wird im GridView-Steuerelement Zustand gespeichert, was bedeutet, dass die `DataKeys` Werte über das Postback hinweg gespeichert werden, auch wenn der GridView s-Ansichts Zustand deaktiviert wurde. Es ist jedoch sehr wichtig, dass der Ansichts Zustand für GridViews aktiviert bleibt, die das Bearbeiten oder löschen unterstützen (das Standardverhalten). Wenn Sie die Eigenschaft GridView s `EnableViewState` auf `false`festlegen, funktioniert das Verhalten beim Bearbeiten und löschen problemlos für einen einzelnen Benutzer, aber wenn gleichzeitige Benutzerdaten löschen, besteht die Möglichkeit, dass diese gleichzeitigen Benutzerdaten Sätze versehentlich löschen oder bearbeiten können, die Sie nicht beabsichtigt haben. Weitere Informationen finden Sie in meinem Blogeintrag [Warnung: Parallelitäts Problem mit ASP.NET 2,0 GridViews/DetailsView/formviews, die das Bearbeiten und/oder löschen unterstützen und deren Ansichts Zustand deaktiviert ist](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx).

Dieselbe Warnung gilt auch für DetailsViews und formviews.

Zum Hinzufügen von Löschfunktionen zu einer GridView wechseln Sie einfach zum Smarttag, und aktivieren Sie das Kontrollkästchen "Löschen".

![Aktivieren Sie das Kontrollkästchen Löschen aktivieren.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Abbildung 10**: Aktivieren des Kontrollkästchens "Löschen"

Durch Aktivieren des Kontrollkästchens "löschen aktivieren" des Smarttags wird ein CommandField zur GridView hinzugefügt. Das CommandField rendert eine Spalte in der GridView mit Schaltflächen zum Ausführen einer oder mehrerer der folgenden Aufgaben: Auswählen eines Datensatzes, Bearbeiten eines Datensatzes und Löschen eines Datensatzes. Wir haben zuvor das CommandField in Aktion zum Auswählen von Datensätzen im [Master/Detail mithilfe eines auswählbaren Master GridView-Tutorials mit einem Detail DetailView-](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) Tutorial gesehen.

Das CommandField enthält eine Reihe von `ShowXButton` Eigenschaften, die angeben, welche Reihe von Schaltflächen im CommandField angezeigt werden. Durch Aktivieren des Kontrollkästchens zum Aktivieren der Löschung wird ein CommandField, dessen `ShowDeleteButton` Eigenschaft `true` ist, der Spalten Auflistung von GridView hinzugefügt.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

An dieser Stelle können Sie die Unterstützung für das GridView-Verfahren hinzufügen. Siehe Abbildung 11 zeigt, dass beim Aufrufen dieser Seite über einen Browser eine Spalte mit Schaltflächen Löschen vorhanden ist.

[![CommandField eine Spalte mit Lösch Schaltflächen hinzufügt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Abbildung 11**: das CommandField fügt eine Spalte mit Lösch Schaltflächen hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))

Wenn Sie dieses Lernprogramm von Grund auf selbst erstellt haben, wird beim Testen dieser Seite eine Ausnahme ausgelöst, wenn Sie auf die Schaltfläche "Löschen" klicken. Lesen Sie weiter, um zu erfahren, warum diese Ausnahmen ausgelöst wurden, und wie Sie Sie beheben.

> [!NOTE]
> Wenn Sie den Download im Rahmen dieses Tutorials durcharbeiten, wurden diese Probleme bereits berücksichtigt. Ich empfehle Ihnen jedoch, die unten aufgeführten Details kennenzulernen, um Probleme zu identifizieren, die auftreten können, und geeignete Problem Umgehungen.

Wenn beim Versuch, ein Produkt zu löschen, eine Ausnahme ausgegeben wird, die der Meldung "*ObjectDataSource ' ObjectDataSource1 '" konnte keine nicht generische Methode "deleteProduct" finden, die über die folgenden Parameter verfügt: "ProductID", "Original\_ProductID*", "Sie haben wahrscheinlich vergessen, die `OldValuesParameterFormatString` Eigenschaft aus der ObjectDataSource zu entfernen. Wenn die `OldValuesParameterFormatString`-Eigenschaft angegeben ist, versucht ObjectDataSource, sowohl `productID` als auch `original_ProductID` Eingabeparameter an die `DeleteProduct`-Methode zu übergeben. `DeleteProduct`akzeptiert jedoch nur einen einzelnen Eingabeparameter, daher die Ausnahme. Wenn Sie die `OldValuesParameterFormatString`-Eigenschaft entfernen (oder Sie auf `{0}`festlegen), weist ObjectDataSource an, nicht zu versuchen, den ursprünglichen Eingabeparameter zu übergeben.

[![stellen Sie sicher, dass die OldValuesParameterFormatString-Eigenschaft gelöscht wurde.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Abbildung 12**: sicherstellen, dass die `OldValuesParameterFormatString`-Eigenschaft gelöscht wurde ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))

Auch wenn Sie die `OldValuesParameterFormatString`-Eigenschaft entfernt haben, erhalten Sie immer noch eine Ausnahme, wenn Sie versuchen, ein Produkt mit der folgenden Meldung zu löschen: "*die DELETE-Anweisung steht in Konflikt mit der Verweis Einschränkung ' FK\_Order\_Details\_Produkte*". Die Northwind-Datenbank enthält eine FOREIGN KEY-Einschränkung zwischen der `Order Details` und `Products` Tabelle. Dies bedeutet, dass ein Produkt nicht aus dem System gelöscht werden kann, wenn es mindestens einen Daten Satz in der `Order Details` Tabelle gibt. Da jedes Produkt in der Northwind-Datenbank über mindestens einen Datensatz in `Order Details`verfügt, können wir keine Produkte löschen, bis wir zum ersten Mal die zugeordneten Bestelldetails des Produkts löschen.

[![eine FOREIGN KEY-Einschränkung das Löschen von Produkten untersagt](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Abbildung 13**: eine FOREIGN KEY-Einschränkung verbietet das Löschen von Produkten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))

In unserem Tutorial löschen wir einfach alle Datensätze aus der `Order Details` Tabelle. In einer realen Anwendung müssten wir Folgendes tun:

- Einen weiteren Bildschirm zur Verwaltung von Bestelldetails anzeigen
- Erweitern Sie die `DeleteProduct`-Methode, um Logik zum Löschen der Bestelldetails der angegebenen Produkte einzuschließen.
- Ändern Sie die vom TableAdapter verwendete SQL-Abfrage so, dass die Bestelldetails des angegebenen Produkts gelöscht werden.

Löschen Sie einfach alle Datensätze aus der `Order Details` Tabelle, um die FOREIGN KEY-Einschränkung zu umgehen. Wechseln Sie zum Server-Explorer in Visual Studio, klicken Sie mit der rechten Maustaste auf den Knoten `NORTHWND.MDF`, und wählen Sie neue Abfrage aus. Führen Sie anschließend im Abfragefenster die folgende SQL-Anweisung aus: `DELETE FROM [Order Details]`

[![alle Datensätze aus der Order Details-Tabelle löschen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Abbildung 14**: Löschen aller Datensätze aus der `Order Details` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))

Nachdem Sie die `Order Details` Tabelle durch Klicken auf die Schaltfläche "Löschen" gelöscht haben, wird das Produkt ohne Fehler gelöscht. Wenn das Produkt durch Klicken auf die Schaltfläche Löschen nicht gelöscht wird, stellen Sie sicher, dass die `DataKeyNames`-Eigenschaft der GridView auf das Feld Primärschlüssel (`ProductID`) festgelegt ist.

> [!NOTE]
> Beim Klicken auf die Schaltfläche Löschen folgt ein Postback, und der Datensatz wird gelöscht. Dies kann gefährlich sein, da es einfach ist, versehentlich auf die Lösch Schaltfläche der falschen Zeile zu klicken. In einem zukünftigen Tutorial erfahren Sie, wie Sie eine Client seitige Bestätigung beim Löschen eines Datensatzes hinzufügen.

## <a name="editing-data-with-the-gridview"></a>Bearbeiten von Daten mit der GridView

Neben dem Löschen bietet das GridView-Steuerelement auch eine integrierte Bearbeitungs Unterstützung auf Zeilenebene. Durch das Konfigurieren eines GridView zur Unterstützung der Bearbeitung wird eine Spalte mit Bearbeitungs Schaltflächen eingefügt Aus Sicht des Endbenutzers bewirkt das Klicken auf die Bearbeitungs Schaltfläche einer Zeile, dass diese Zeile bearbeitet werden kann. die Zellen werden in Textfelder umgewandelt, die die vorhandenen Werte enthalten, und die Bearbeitungs Schaltfläche durch die Schaltflächen aktualisieren und Abbrechen ersetzen. Nachdem die gewünschten Änderungen vorgenommen wurden, kann der Endbenutzer auf die Schaltfläche Aktualisieren klicken, um die Änderungen zu übernehmen, oder auf die Schaltfläche Abbrechen, um die Änderungen In beiden Fällen wird die GridView nach dem Klicken auf Aktualisieren oder Abbrechen in den Zustand der vorabbearbeitung zurückversetzt.

Wenn der Endbenutzer für eine bestimmte Zeile auf die Schaltfläche "Bearbeiten" klickt, wird aus unserer Perspektive als Seiten Entwickler ein Postback ausgeführt, und die GridView führt die folgenden Schritte aus:

1. Die `EditItemIndex`-Eigenschaft der GridView wird dem Index der Zeile zugewiesen, auf deren Bearbeitungs Schaltfläche geklickt wurde.
2. Die GridView bindet sich an die ObjectDataSource zurück, indem Sie Ihre `Select()`-Methode aufruft.
3. Der Zeilen Index, der dem `EditItemIndex` entspricht, wird im Bearbeitungsmodus gerendert. In diesem Modus wird die Bearbeitungs Schaltfläche durch die Schaltflächen aktualisieren und Abbrechen und boundfields ersetzt, deren `ReadOnly` Eigenschaften false (Standard) sind, als TextBox-websteuer Elemente gerendert werden, deren `Text` Eigenschaften den Werten der Datenfelder zugewiesen werden.

An diesem Punkt wird das Markup an den Browser zurückgegeben, sodass der Endbenutzer Änderungen an den Daten der Zeile vornehmen kann. Wenn der Benutzer auf die Schaltfläche Aktualisieren klickt, erfolgt ein Postback, und die GridView führt die folgenden Schritte aus:

1. Den `UpdateParameters` Werten der ObjectDataSource werden die vom Endbenutzer in der Bearbeitungs Schnittstelle von GridView eingegebenen Werte zugewiesen.
2. Die `Update()`-Methode von ObjectDataSource wird aufgerufen, wobei der angegebene Datensatz aktualisiert wird.
3. Die GridView bindet sich an die ObjectDataSource zurück, indem Sie Ihre `Select()`-Methode aufruft.

Die Primärschlüssel Werte, die dem `UpdateParameters` in Schritt 1 zugewiesen sind, stammen aus den Werten, die in der `DataKeyNames`-Eigenschaft angegeben sind, wohingegen die nicht-Primärschlüssel Werte aus dem Text in den TextBox-websteuer Elementen für die bearbeitete Zeile stammen. Wie beim Löschen ist es wichtig, dass die `DataKeyNames`-Eigenschaft einer GridView ordnungsgemäß festgelegt ist. Wenn Sie nicht vorhanden ist, wird dem `UpdateParameters` Primärschlüssel Wert in Schritt 1 ein `null` Wert zugewiesen, der wiederum keine aktualisierten Datensätze in Schritt 2 ergibt.

Sie können die Bearbeitungsfunktion aktivieren, indem Sie einfach das Kontrollkästchen Bearbeiten aktivieren im Smarttags von GridView aktivieren.

![Aktivieren Sie das Kontrollkästchen Bearbeitung aktivieren.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Abbildung 15**: Aktivieren Sie das Kontrollkästchen "Bearbeiten aktivieren"

Wenn Sie das Kontrollkästchen "Bearbeitung aktivieren" aktivieren, wird ein CommandField (falls erforderlich) hinzugefügt und dessen `ShowEditButton`-Eigenschaft auf `true`festgelegt Wie bereits erwähnt, enthält das CommandField eine Reihe von `ShowXButton` Eigenschaften, die angeben, welche Reihe von Schaltflächen im CommandField angezeigt werden. Durch Aktivieren des Kontrollkästchens "Bearbeiten aktivieren" wird dem vorhandenen CommandField die `ShowEditButton`-Eigenschaft hinzugefügt

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Das ist alles, was Sie tun müssen, um rudimentäre Bearbeitungs Unterstützung hinzuzufügen. Wie Figure16 zeigt, ist die Bearbeitungs Schnittstelle eher grob als jedes BoundField-Objekt, dessen `ReadOnly`-Eigenschaft auf `false` festgelegt ist (Standard), wird als Textfeld gerendert. Dies schließt Felder wie `CategoryID` und `SupplierID`ein, bei denen es sich um Schlüssel für andere Tabellen handelt.

[![klicken auf die Schaltfläche "Chai s Edit" zeigt die Zeile im Bearbeitungsmodus an](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Abbildung 16**: Klicken auf die Bearbeitungs Schaltfläche von Chai s zeigt die Zeile im Bearbeitungsmodus an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))

Zusätzlich dazu, dass Benutzer aufgefordert werden, Fremdschlüssel Werte direkt zu bearbeiten, fehlt die Schnittstelle der Bearbeitungs Schnittstelle auf folgende Weise:

- Wenn der Benutzer einen `CategoryID` oder `SupplierID` eingibt, der nicht in der Datenbank vorhanden ist, verstößt der `UPDATE` gegen eine FOREIGN KEY-Einschränkung, wodurch eine Ausnahme ausgelöst wird.
- Die Bearbeitungs Schnittstelle umfasst keine Validierung. Wenn Sie keinen erforderlichen Wert angeben (z. b. `ProductName`) oder einen Zeichen folgen Wert eingeben, in dem ein numerischer Wert erwartet wird (z. b. "zu viel!"). im Textfeld `UnitPrice`) wird eine Ausnahme ausgelöst. In einem zukünftigen Tutorial wird erläutert, wie Sie der Bearbeitungs Benutzeroberfläche Validierungs Steuerelemente hinzufügen.
- Derzeit müssen *alle* nicht schreibgeschützten Produktfelder in der GridView enthalten sein. Wenn Sie ein Feld aus der GridView entfernen, z. `UnitPrice`. Wenn Sie die Daten aktualisieren, würde die GridView den `UnitPrice` `UpdateParameters` Wert nicht festlegen, wodurch der `UnitPrice` des Datensatzes in einen `NULL` Wert geändert würde. Wenn ein erforderliches Feld, z. b. `ProductName`, aus der GridView entfernt wird, schlägt das Update ebenso fehl, dass die oben erwähnte Ausnahme "*Spalte ' ProductName ' keine NULL*-Werte zulässt".
- Die Formatierung der Bearbeitungs Schnittstelle lässt sich sehr viel wünschen. Die `UnitPrice` wird mit vier Dezimalstellen angezeigt. Im Idealfall enthalten die Werte für `CategoryID` und `SupplierID` Dropdown Listen, die die Kategorien und Lieferanten im System auflisten.

Dabei handelt es sich um alle Mängel, mit denen wir uns jetzt beschäftigen müssen, aber in zukünftigen Tutorials behandelt werden.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Einfügen, bearbeiten und Löschen von Daten mit der DetailsView

Wie in den vorherigen Tutorials gezeigt, zeigt das DetailsView-Steuerelement einen Datensatz gleichzeitig an und ermöglicht, wie z. b. GridView, das Bearbeiten und Löschen des aktuell angezeigten Datensatzes. Sowohl die Benutzeroberflächen des Endbenutzers beim Bearbeiten als auch das Löschen von Elementen aus einer DetailsView und der Workflow von der ASP.NET-Seite sind identisch mit denen der GridView. Die DetailsView unterscheidet sich von der GridView, dass Sie auch integrierte einfügeunterstützung bereitstellt.

Um die Daten Änderungs Funktionen der GridView zu veranschaulichen, fügen Sie zunächst der `Basics.aspx` Seite oberhalb der vorhandenen GridView eine DetailsView hinzu und binden diese mithilfe des Smarttags der DetailsView an die vorhandene ObjectDataSource. Löschen Sie als nächstes die Eigenschaften `Height` und `Width` der DetailsView, und aktivieren Sie die Option Paging aktivieren des Smarttags. Um das Bearbeiten, einfügen und Löschen der Unterstützung zu aktivieren, aktivieren Sie einfach die Kontrollkästchen Bearbeiten aktivieren, einfügen aktivieren und löschen im Smarttags aktivieren.

![Konfigurieren der DetailsView, um das Bearbeiten, einfügen und löschen zu unterstützen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Abbildung 17**: Konfigurieren der DetailsView für die Unterstützung von Bearbeitung, einfügen und löschen

Wie bei GridView wird durch das Hinzufügen von Unterstützung für das Bearbeiten, einfügen oder Löschen der DetailsView ein CommandField hinzugefügt, wie die folgende deklarative Syntax zeigt:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Beachten Sie, dass für die DetailsView das CommandField standardmäßig am Ende der Columns-Auflistung angezeigt wird. Da die Felder der DetailsView als Zeilen gerendert werden, wird das CommandField als Zeile mit den Schaltflächen einfügen, bearbeiten und löschen am unteren Rand der DetailsView angezeigt.

[![Konfigurieren der DetailsView, um das Bearbeiten, einfügen und löschen zu unterstützen.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Abbildung 18**: Konfigurieren der DetailsView für die Unterstützung von Bearbeitung, einfügen und löschen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))

Wenn Sie auf die Schaltfläche "Löschen" klicken, wird dieselbe Abfolge von Ereignissen wie in der GridView-Ansicht gestartet: ein Postback. gefolgt von der DetailsView, die die `DeleteParameters` der ObjectDataSource basierend auf den `DataKeyNames` Werten auffüllt. und wurde mit einem-`Delete()` der ObjectDataSource-Methode abgeschlossen, die das Produkt tatsächlich aus der Datenbank entfernt. Die Bearbeitung in DetailsView funktioniert auch identisch mit der der GridView.

Zum Einfügen wird dem Endbenutzer eine neue Schaltfläche angezeigt, mit der die DetailsView im Einfügemodus gerendert wird, wenn darauf geklickt wird. Mit dem "Einfügemodus" wird die Schaltfläche "neu" durch die Schaltflächen einfügen und Abbrechen ersetzt, und nur die boundfields-Eigenschaft, deren `InsertVisible`-Eigenschaft auf `true` festgelegt ist (Standardeinstellung) Für diese Datenfelder, die als automatische Inkrement-Felder (z. b. `ProductID`) identifiziert werden, wird die [InsertVisible-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) auf `false` festgelegt, wenn die DetailsView über das Smarttag an die Datenquelle gebunden wird.

Wenn eine Datenquelle über das Smarttag an eine DetailsView gebunden wird, legt Visual Studio die `InsertVisible`-Eigenschaft auf `false` nur für Felder mit automatischer Inkrement fest. Schreibgeschützte Felder, wie `CategoryName` und `SupplierName`, werden auf der Benutzeroberfläche des Einfügemodus angezeigt, es sei denn, ihre `InsertVisible`-Eigenschaft ist explizit auf `false`festgelegt. Nehmen Sie sich einen Moment Zeit, um die `InsertVisible` Eigenschaften der beiden Felder auf `false`festzulegen, entweder über die deklarative Syntax der DetailsView oder über den Link "Felder bearbeiten" im Smarttags. Abbildung 19 zeigt, wie Sie die `InsertVisible` Eigenschaften auf `false` festlegen, indem Sie auf den Link "Felder bearbeiten" klicken.

[![Northwind Traders bietet jetzt Acme-Tee an](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Abbildung 19**: Northwind Traders bietet jetzt Acme-Tee ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))

Nachdem Sie die `InsertVisible` Eigenschaften festgelegt haben, können Sie die `Basics.aspx` Seite in einem Browser anzeigen und auf die Schaltfläche Neu klicken. Abbildung 20 zeigt die DetailsView beim Hinzufügen eines neuen Getränks, eines Acme-Tees, zu unserer Produktlinie.

[![Northwind Traders bietet jetzt Acme-Tee an](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Abbildung 20**: Northwind Traders bietet jetzt Acme-Tee ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))

Nachdem Sie die Details für den Acme-Tee eingegeben und auf die Schaltfläche Einfügen geklickt haben, folgt ein Postback, und der neue Datensatz wird der `Products` Datenbanktabelle hinzugefügt. Da in dieser DetailsView die Produkte aufgeführt werden, in denen Sie in der Datenbanktabelle vorhanden sind, muss eine Seite zum letzten Produkt angezeigt werden, um das neue Produkt anzuzeigen.

[![Details für den Acme-Tee](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Abbildung 21**: Details zum Acme-Tee ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))

> [!NOTE]
> Die [CurrentMode-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) von DetailsView gibt an, dass die Schnittstelle angezeigt wird, und kann einen der folgenden Werte aufweisen: `Edit`, `Insert`oder `ReadOnly`. Die [DefaultMode-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) gibt den Modus an, zu dem die DetailsView zurückkehrt, nachdem ein Edit-oder INSERT-Vorgang abgeschlossen wurde, und eignet sich zum Anzeigen einer DetailsView, die dauerhaft im Bearbeitungs-oder Einfügemodus ist.

Das Einfügen und Bearbeiten von Funktionen in DetailsView weist die gleichen Einschränkungen wie die GridView auf: der Benutzer muss vorhandene `CategoryID` und `SupplierID` Werte über ein Textfeld eingeben. der Schnittstelle fehlt eine Validierungs Logik. Alle Produktfelder, die keine `NULL` Werte zulassen oder auf der Datenbankebene keinen Standardwert angeben, müssen in der einfügeschnittstelle enthalten sein usw.

Die Techniken, die wir für das erweitern und verbessern der Bearbeitungs Schnittstelle von GridView in zukünftigen Artikeln untersuchen werden, können auch auf die Schnittstellen zum Bearbeiten und Einfügen des DetailsView-Steuer Elements angewendet werden.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Verwenden von FormView für eine flexiblere Daten Änderungs Benutzeroberfläche

Die FormView bietet integrierte Unterstützung für das Einfügen, bearbeiten und Löschen von Daten, aber da Sie anstelle von Feldern Vorlagen verwendet, gibt es keinen Ort zum Hinzufügen der boundfields-oder CommandField-Steuerelemente, die von den Steuerelementen GridView und DetailsView zum Bereitstellen der Daten verwendet werden. Schnittstelle ändern. Stattdessen wird diese Schnittstelle die websteuer Elemente zum Erfassen von Benutzereingaben beim Hinzufügen eines neuen Elements oder zum Bearbeiten eines vorhandenen Elements zusammen mit den Schaltflächen neu, bearbeiten, löschen, einfügen, aktualisieren und Abbrechen manuell zu den entsprechenden Vorlagen hinzugefügt. Glücklicherweise erstellt Visual Studio automatisch die erforderliche Schnittstelle, wenn die Form Ansicht über die Dropdown Liste im Smarttags an eine Datenquelle gebunden wird.

Um diese Techniken zu veranschaulichen, fügen Sie zunächst der Seite `Basics.aspx` eine FormView hinzu, und binden Sie Sie aus dem Smarttag von FormView an die bereits erstellte ObjectDataSource. Dadurch wird eine `EditItemTemplate`, `InsertItemTemplate`und `ItemTemplate` für die FormView mit TextBox-websteuer Elementen generiert, die die Eingabe-und Schaltflächen-websteuer Elemente des Benutzers für die Schaltflächen neu, bearbeiten, löschen, einfügen, aktualisieren und Abbrechen erfassen. Außerdem wird die `DataKeyNames`-Eigenschaft von FormView auf das Primärschlüssel Feld (`ProductID`) des Objekts festgelegt, das von ObjectDataSource zurückgegeben wurde. Aktivieren Sie abschließend die Option Paging aktivieren im Smarttag von FormView.

Das folgende Beispiel zeigt das deklarative Markup für die `ItemTemplate` von FormView, nachdem FormView an die ObjectDataSource gebunden wurde. Standardmäßig ist jedes nicht-boolesche Wert-Produktfeld an die `Text`-Eigenschaft eines Bezeichnungs-websteuer Elements gebunden, während jedes boolesche Wertfeld (`Discontinued`) an die `Checked`-Eigenschaft eines deaktivierten CheckBox-websteuer Elements gebunden ist. Damit die Schaltflächen "neu", "Bearbeiten" und "Löschen" bei einem Klick ein bestimmtes FormView-Verhalten auslöst, ist es zwingend erforderlich, dass die `CommandName` Werte auf "`New`", "`Edit`" und "`Delete`" festgelegt werden.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Abbildung 22 zeigt die `ItemTemplate` von FormView, wenn Sie in einem Browser angezeigt wird. Jedes Produktfeld wird im unteren Bereich mit den Schaltflächen neu, bearbeiten und löschen aufgelistet.

[![der defaut FormView ItemTemplate werden die einzelnen Produktfelder zusammen mit den Schaltflächen neu, bearbeiten und löschen aufgelistet.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Abbildung 22**: im defaut FormView-`ItemTemplate` werden die einzelnen Produktfelder zusammen mit den Schaltflächen neu, bearbeiten und löschen aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))

Wie bei GridView und DetailsView führt das Klicken auf die Schaltfläche Löschen oder auf eine beliebige Schaltfläche, LinkButton oder ImageButton, deren `CommandName`-Eigenschaft auf Delete festgelegt ist, zu einem Postback, füllt die `DeleteParameters` von ObjectDataSource basierend auf dem `DataKeyNames` Wert von FormView auf und ruft die `Delete()`-Methode von ObjectDataSource auf.

Wenn auf die Schaltfläche Bearbeiten geklickt wird, wird ein Postback durchsucht, und die Daten werden an den `EditItemTemplate`zurückgebunden, der für das Rendern der Bearbeitungs Schnittstelle zuständig ist. Diese Schnittstelle enthält die websteuer Elemente zum Bearbeiten von Daten zusammen mit den Schaltflächen aktualisieren und Abbrechen. Der von Visual Studio generierte Standard `EditItemTemplate` enthält eine Bezeichnung für alle automatischen Inkrement-Felder (`ProductID`), ein Textfeld für jedes nicht-boolesche Wertfeld und ein Kontrollkästchen für jedes boolesche Wertfeld. Dieses Verhalten ähnelt dem automatisch generierten boundfields-Steuerelement in den GridView-und DetailsView-Steuerelementen.

> [!NOTE]
> Ein kleines Problem bei der automatischen Generierung der `EditItemTemplate` von FormView besteht darin, dass es TextBox-websteuer Elemente für Felder rendert, die schreibgeschützt sind, z. b. `CategoryName` und `SupplierName`. Dies wird in Kürze erläutert.

Die `Text`-Eigenschaft der TextBox-Steuerelemente in der `EditItemTemplate` *muss mithilfe der*bidirektionalen Datenbindung an den Wert des entsprechenden Daten Felds gebunden werden. Die bidirektionale Datenbindung, die durch `<%# Bind("dataField") %>`angegeben wird, führt die Datenbindung sowohl beim Binden von Daten an die Vorlage als auch beim Auffüllen der Parameter von ObjectDataSource zum Einfügen oder Bearbeiten von Datensätzen aus. Das heißt, wenn der Benutzer im `ItemTemplate`auf die Schaltfläche "Bearbeiten" klickt, gibt die `Bind()`-Methode den angegebenen Daten Feldwert zurück. Nachdem der Benutzer die Änderungen vorgenommen und auf Update geklickt hat, werden die zurückgegebenen Werte, die den mit `Bind()` angegebenen Datenfeldern entsprechen, auf die `UpdateParameters`von ObjectDataSource angewendet. Alternativ werden bei der unidirektionalen Datenbindung, die durch `<%# Eval("dataField") %>`gekennzeichnet ist, nur die Daten Feldwerte abgerufen, wenn Daten an die Vorlage gebunden werden, und die vom Benutzer eingegebenen Werte werden beim Postback *nicht* an die Parameter der Datenquelle zurückgegeben.

Das folgende deklarative Markup zeigt die `EditItemTemplate`von FormView. Beachten Sie, dass die `Bind()`-Methode in der Datenbindung-Syntax hier verwendet wird und dass für die websteuer Elemente Update und Abbrechen die `CommandName` Eigenschaften entsprechend festgelegt sind.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Unsere `EditItemTemplate`wird zu diesem Zeitpunkt eine Ausnahme auslösen, wenn Sie versuchen, Sie zu verwenden. Das Problem besteht darin, dass die Felder `CategoryName` und `SupplierName` als TextBox-websteuer Elemente im `EditItemTemplate`gerendert werden. Diese Textfelder müssen entweder in Bezeichnungen geändert oder entfernt werden. Wir löschen Sie einfach vollständig aus dem `EditItemTemplate`.

Abbildung 23 zeigt die Form View in einem Browser, nachdem auf die Schaltfläche Bearbeiten für Chai geklickt wurde. Beachten Sie, dass die `SupplierName`-und `CategoryName` Felder, die in der `ItemTemplate` angezeigt werden, nicht mehr vorhanden sind, da wir Sie soeben aus dem `EditItemTemplate`entfernt haben. Wenn auf die Schaltfläche Aktualisieren geklickt wird, führt FormView die gleiche Abfolge von Schritten durch wie das GridView-Steuerelement und das DetailsView-Steuerelement.

[![Standardmäßig zeigt EditItemTemplate jedes bearbeitbare Produktfeld als Textfeld oder Kontrollkästchen an.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Abbildung 23**: Standardmäßig zeigt das `EditItemTemplate` jedes bearbeitbare Produktfeld als Textfeld oder Kontrollkästchen[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png)).

Wenn auf die Schaltfläche Einfügen geklickt wird, `ItemTemplate` ein Postback. Es werden jedoch keine Daten an die FormView gebunden, da ein neuer Datensatz hinzugefügt wird. Die `InsertItemTemplate`-Schnittstelle enthält die websteuer Elemente zum Hinzufügen eines neuen Datensatzes zusammen mit den Schaltflächen einfügen und Abbrechen. Der Standard `InsertItemTemplate`, der von Visual Studio generiert wird, enthält ein Textfeld für jedes nicht-boolesche Wertfeld und ein Kontrollkästchen für jedes boolesche Wertfeld, ähnlich wie bei der Schnittstelle der automatisch generierten `EditItemTemplate`. Die `Text`-Eigenschaft der Textfeld-Steuerelemente wird mithilfe der bidirektionalen Datenbindung an den Wert des entsprechenden Daten Felds gebunden.

Das folgende deklarative Markup zeigt die `InsertItemTemplate`von FormView. Beachten Sie, dass die `Bind()`-Methode in der Datenbindung-Syntax hier verwendet wird und dass die `CommandName` Eigenschaften für die Schaltfläche "Einfügen" und "Abbrechen" entsprechend festgelegt sind.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Die automatische Generierung des `InsertItemTemplate`der FormView hat eine Feinheit. Insbesondere die TextBox-websteuer Elemente werden erstellt, auch für diejenigen Felder, die schreibgeschützt sind, z. b. `CategoryName` und `SupplierName`. Wie beim `EditItemTemplate`müssen diese Textfelder aus dem `InsertItemTemplate`entfernt werden.

Abbildung 24 zeigt die Form View in einem Browser, wenn Sie ein neues Produkt, Acme Coffee, hinzufügen. Beachten Sie, dass die `SupplierName`-und `CategoryName` Felder, die im `ItemTemplate` angezeigt werden, nicht mehr vorhanden sind, wie wir Sie gerade entfernt haben. Wenn auf die Schaltfläche Einfügen geklickt wird, durchläuft FormView die gleiche Abfolge von Schritten wie das DetailsView-Steuerelement und fügt der `Products` Tabelle einen neuen Datensatz hinzu. Abbildung 25 zeigt die Details des Acme Coffee-Produkts in der FormView, nachdem Sie eingefügt wurde.

[![die InsertItemTemplate die einfügeschnittstelle der FormView-Eigenschaft festlegt.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Abbildung 24**: die `InsertItemTemplate` die die einfügeschnittstelle von FormView festlegt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))

[![die Details für das neue Produkt, die Acme Coffee, werden in der FormView angezeigt.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Abbildung 25**: die Details für das neue Produkt, die Acme Coffee, werden in der FormView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))

Durch das Aufteilen der schreibgeschützten, Bearbeitungs-und einfügeschnittstellen in drei separate Vorlagen ermöglicht die FormView eine präzisere Kontrolle über diese Schnittstellen als die DetailsView und GridView.

> [!NOTE]
> Wie die DetailsView zeigt die `CurrentMode`-Eigenschaft von FormView die angezeigte Schnittstelle an, und ihre `DefaultMode`-Eigenschaft gibt den Modus an, zu dem die FormView zurückkehrt, nachdem eine Bearbeitung oder eine Einfügung abgeschlossen wurde.

## <a name="summary"></a>Summary

In diesem Tutorial haben wir die Grundlagen zum Einfügen, bearbeiten und Löschen von Daten mithilfe von GridView, DetailsView und FormView untersucht. Alle drei Steuerelemente bieten eine Reihe integrierter Funktionen für die Datenänderung, die verwendet werden können, ohne dass eine einzige Codezeile in der ASP.NET-Seite durch die datenweb Steuerelemente und die ObjectDataSource geschrieben werden kann. Die einfachen Punkt-und Klick Techniken erzeugen jedoch eine recht Frail-und naive Daten Änderungs Benutzeroberfläche. Um die Validierung zu ermöglichen, programmgesteuerte Werte einzufügen, Ausnahmen ordnungsgemäß zu behandeln, die Benutzeroberfläche anzupassen usw., müssen wir uns auf eine Reihe von Techniken stützen, die in den nächsten Tutorials erläutert werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
