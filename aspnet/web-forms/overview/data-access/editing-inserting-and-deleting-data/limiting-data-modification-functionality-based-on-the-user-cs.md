---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Einschränken der Daten Änderungs Funktionalität basierend auf dem BenutzerC#() | Microsoft-Dokumentation
author: rick-anderson
description: In einer Webanwendung, die Benutzern das Bearbeiten von Daten ermöglicht, haben unterschiedliche Benutzerkonten möglicherweise unterschiedliche Berechtigungen für die Datenbearbeitung. In diesem Tutorial wird erläutert, wie t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: c3cacaddb7e9b493ba39718f41dcaab360d36fd9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78478425"
---
# <a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Benutzerabhängiges Beschränken von Datenänderungsfunktionen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) oder [PDF herunterladen](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> In einer Webanwendung, die Benutzern das Bearbeiten von Daten ermöglicht, haben unterschiedliche Benutzerkonten möglicherweise unterschiedliche Berechtigungen für die Datenbearbeitung. In diesem Tutorial wird erläutert, wie Sie die Daten Änderungs Funktionen auf der Grundlage des besuchenden Benutzers dynamisch anpassen.

## <a name="introduction"></a>Einführung

Eine Reihe von Webanwendungen unterstützen Benutzerkonten und bieten unterschiedliche Optionen, Berichte und Funktionen basierend auf dem angemeldeten Benutzer. Mit unseren Tutorials möchten wir beispielsweise Benutzern von den Lieferanten Unternehmen erlauben, sich an der Website anzumelden und allgemeine Informationen zu ihren Produkten zu aktualisieren, z. b. den Namen und die Menge pro Einheit, möglicherweise zusammen mit Lieferanteninformationen, wie z. b. den Firmennamen. Adresse, Kontaktperson Informationen usw. Außerdem möchten wir ggf. einige Benutzerkonten für Personen aus unserem Unternehmen einschließen, damit Sie sich anmelden und Produktinformationen wie z. b. Einheiten auf Lager-, neu anordnen-Ebene usw. aktualisieren können. Die Webanwendung gestattet möglicherweise auch anonymen Benutzern den Besuch (Personen, die sich nicht angemeldet haben), schränkt Sie jedoch auf die Anzeige von Daten ein. Wenn ein solches Benutzerkonto System vorhanden ist, möchten wir, dass die datenweb-Steuerelemente auf unseren ASP.NET Seiten die Funktionen zum Einfügen, bearbeiten und löschen anbieten, die für den aktuell angemeldeten Benutzer geeignet sind.

In diesem Tutorial wird erläutert, wie Sie die Daten Änderungs Funktionen auf der Grundlage des besuchenden Benutzers dynamisch anpassen. Insbesondere erstellen wir eine Seite, auf der die Lieferanten Informationen in einer bearbeitbaren DetailsView zusammen mit einer GridView angezeigt werden, in der die vom Lieferanten bereitgestellten Produkte aufgeführt sind. Wenn der Benutzer, der die Seite besucht, von unserem Unternehmen aus ist, kann er alle Lieferanteninformationen anzeigen. Bearbeiten Sie die Adresse. und bearbeiten Sie die Informationen für alle Produkte, die vom Lieferanten bereitgestellt werden. Wenn der Benutzer jedoch von einem bestimmten Unternehmen ist, kann er nur seine eigenen Adressinformationen anzeigen und bearbeiten und nur die Produkte bearbeiten, die nicht als eingestellt gekennzeichnet wurden.

[![ein Benutzer aus unserem Unternehmen kann alle Lieferanteninformationen bearbeiten.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Abbildung 1**: ein Benutzer aus unserem Unternehmen kann alle Informationen des Lieferanten s bearbeiten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))

[![Benutzer von einem bestimmten Lieferanten nur Ihre Informationen anzeigen und bearbeiten können](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Abbildung 2**: ein Benutzer eines bestimmten Lieferanten kann seine Informationen nur anzeigen und bearbeiten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))

Legen Sie Los!

> [!NOTE]
> Das ASP.NET 2,0 s-Mitgliedschaftssystem bietet eine standardisierte, erweiterbare Plattform zum Erstellen, verwalten und Validieren von Benutzerkonten. Da die Untersuchung des Mitgliedschafts Systems über den Rahmen dieser Tutorials hinausgeht, wird in diesem Tutorial stattdessen die Mitgliedschaft "fakes" gewährt, indem anonyme Besucher entscheiden können, ob Sie von einem bestimmten Lieferanten oder von unserem Unternehmen stammen. Weitere Informationen zur Mitgliedschaft finden Sie unter meine unter [suchung ASP.NET 2,0 s Mitgliedschafts-, Rollen-und Profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) Artikel Reihe.

## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Schritt 1: zulassen, dass der Benutzer seine Zugriffsrechte angibt

In einer Webanwendung in der Praxis enthalten die Kontoinformationen eines Benutzers, ob Sie für unser Unternehmen oder für einen bestimmten Lieferanten gearbeitet haben. diese Informationen sind auf den ASP.NET-Seiten Programm gesteuert zugänglich, sobald sich der Benutzer bei der Website angemeldet hat. Diese Informationen können über das ASP.NET 2,0 s-Rollensystem, als Kontoinformationen auf Benutzerebene über das Profilsystem oder auf benutzerdefinierte Weise aufgezeichnet werden.

Da das Ziel dieses Tutorials darin besteht, die Anpassung der Daten Änderungs Funktionen basierend auf dem angemeldeten Benutzer zu veranschaulichen und nicht für die Veranschaulichung der ASP.NET 2,0 s-Mitgliedschaft,-Rollen und-Profilsysteme vorgesehen ist, verwenden wir einen sehr einfachen Mechanismus zum Ermitteln der Funktionen für den Benutzer, der die Seite besucht: eine Dropdown Liste, aus der der Benutzer angeben kann, ob er die Informationen zu den Lieferanten anzeigen und bearbeiten und alternativ die Informationen zu den einzelnen Lieferanten anzeigen und bearbeiten kann. Wenn der Benutzer anzeigt, dass er alle Lieferanteninformationen anzeigen und bearbeiten kann (Standardeinstellung), kann er alle Lieferanten durchlaufen, alle Adressinformationen eines Lieferanten bearbeiten und den Namen und die Menge pro Einheit für alle Produkte bearbeiten, die vom ausgewählten Lieferanten bereitgestellt werden. Wenn der Benutzer anzeigt, dass Sie nur einen bestimmten Lieferanten anzeigen und bearbeiten kann, kann er jedoch nur die Details und Produkte für diesen einen Lieferanten anzeigen und nur den Namen und die Menge pro Einheiten Informationen für die Produkte, die *nicht* eingestellt werden, nur aktualisieren.

Der erste Schritt in diesem Tutorial besteht darin, diese Dropdown Liste zu erstellen und mit den Lieferanten des Systems aufzufüllen. Öffnen Sie die Seite `UserLevelAccess.aspx` im Ordner `EditInsertDelete`, fügen Sie eine Dropdown List-Eigenschaft hinzu, deren `ID`-Eigenschaft auf `Suppliers`festgelegt ist, und binden Sie diese DropDownList an eine neue ObjectDataSource mit dem Namen `AllSuppliersDataSource`.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen allsuppliersdatasource.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Abbildung 3**: Erstellen einer neuen ObjectDataSource mit dem Namen "`AllSuppliersDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))

Da diese Dropdown Liste alle Lieferanten einschließen soll, konfigurieren Sie ObjectDataSource so, dass die `SuppliersBLL` Class s `GetSuppliers()`-Methode aufgerufen wird. Stellen Sie außerdem sicher, dass die ObjectDataSource s-`Update()`-Methode der `SuppliersBLL` Class s `UpdateSupplierAddress`-Methode zugeordnet ist, da diese ObjectDataSource auch von der DetailsView verwendet wird, die wir in Schritt 2 hinzufügen werden.

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, führen Sie die Schritte aus, indem Sie die `Suppliers` Dropdown List so konfigurieren, dass das `CompanyName` Datenfeld angezeigt wird, und das `SupplierID` Datenfeld als Wert für jede `ListItem`verwendet wird.

[![konfigurieren Sie die Dropdown Liste "Suppliers" für die Verwendung der Datenfelder "CompanyName" und "SupplierID"](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Abbildung 4**: Konfigurieren der `Suppliers` Dropdown Liste für die Verwendung der Felder `CompanyName` und `SupplierID` Daten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))

An diesem Punkt werden in der Dropdown Liste die Firmennamen der Lieferanten in der-Datenbank aufgelistet. Wir müssen jedoch auch die Option "alle Lieferanten anzeigen/bearbeiten" in die Dropdown Liste einschließen. Legen Sie hierzu die `Suppliers` DropDownList s `AppendDataBoundItems`-Eigenschaft auf `true` fest, und fügen Sie dann eine `ListItem` hinzu, deren `Text`-Eigenschaft "alle Lieferanten anzeigen/bearbeiten" und deren Wert `-1`ist. Dies kann direkt über das deklarative Markup oder über den Designer hinzugefügt werden, indem Sie zum Eigenschaftenfenster navigieren und auf die Ellipse in der DropDownList s `Items`-Eigenschaft klicken.

> [!NOTE]
> Eine ausführlichere Erläuterung zum Hinzufügen eines Elements "Alles auswählen" zu einer Dropdown Liste [*mit einem Dropdown List-Element finden Sie im Tutorial zur Master/Detail-Filterung mit einem Dropdown List-* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Tutorial.

Nachdem die `AppendDataBoundItems`-Eigenschaft festgelegt wurde und die `ListItem` hinzugefügt wurde, sollte das deklarative Markup der DropDownList s wie folgt aussehen:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Abbildung 5 zeigt einen Screenshot des aktuellen Fortschritts, wenn er über einen Browser angezeigt wird.

[![die Dropdown Liste "Suppliers" eine "alle anzeigen"-Liste und einen für jeden Lieferanten enthält.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Abbildung 5**: die Dropdown Liste "`Suppliers` anzeigen" enthält ein `ListItem`alle anzeigen sowie einen für jeden Lieferanten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))

Da wir die Benutzeroberfläche sofort nach dem Ändern der Auswahl des Benutzers aktualisieren möchten, legen Sie die `Suppliers` DropDownList s `AutoPostBack`-Eigenschaft auf `true`fest. In Schritt 2 erstellen wir ein DetailsView-Steuerelement, das die Informationen für die Lieferanten auf der Grundlage der Dropdown List-Auswahl anzeigt. Anschließend erstellen wir in Schritt 3 einen Ereignishandler für dieses DropDownList s-`SelectedIndexChanged` Ereignis, in dem wir Code hinzufügen, der die entsprechenden Lieferanteninformationen basierend auf dem ausgewählten Lieferanten an die DetailsView bindet.

## <a name="step-2-adding-a-detailsview-control"></a>Schritt 2: Hinzufügen eines DetailsView-Steuer Elements

Verwenden Sie eine DetailsView, um Lieferanteninformationen anzuzeigen. Für den Benutzer, der alle Lieferanten anzeigen und bearbeiten kann, unterstützt die DetailsView das Paging, sodass der Benutzer die Lieferanteninformationen einen Datensatz nacheinander durchlaufen kann. Wenn der Benutzer jedoch für einen bestimmten Lieferanten arbeitet, zeigt die DetailsView nur die Informationen des jeweiligen Lieferanten an und enthält keine Paging-Schnittstelle. In beiden Fällen muss die DetailsView es dem Benutzer ermöglichen, die Felder "Address", "City" und "Country" des Lieferanten zu bearbeiten.

Fügen Sie der Seite unterhalb der Dropdown Liste `Suppliers` eine DetailsView hinzu, legen Sie die `ID`-Eigenschaft auf `SupplierDetails`fest, und binden Sie Sie an die im vorherigen Schritt erstellte `AllSuppliersDataSource` ObjectDataSource. Aktivieren Sie als nächstes die Kontrollkästchen Paging aktivieren und Bearbeitung aktivieren im Smarttags DetailsView s.

> [!NOTE]
> Wenn Sie die Option zum Aktivieren der Bearbeitung in der DetailsView s Smarttags nicht sehen, haben Sie die ObjectDataSource s-`Update()` Methode nicht der `SuppliersBLL` Class s `UpdateSupplierAddress`-Methode zugeordnet. Nehmen Sie sich einen Moment Zeit, und nehmen Sie diese Konfigurationsänderung vor, nachdem die Option Bearbeitung aktivieren im Smarttags DetailsView s angezeigt werden soll.

Da die `UpdateSupplierAddress`-Methode der `SuppliersBLL`-Klasse nur vier Parameter akzeptiert: `supplierID`, `address`, `city`und `country`-ändern Sie die Details der DetailsView s boundfields, sodass die `CompanyName`-und `Phone` boundfields schreibgeschützt sind. Entfernen Sie außerdem den `SupplierID` BoundField vollständig. Zum Schluss hat der `AllSuppliersDataSource` ObjectDataSource derzeit die `OldValuesParameterFormatString`-Eigenschaft auf `original_{0}`festgelegt. Nehmen Sie sich einen Moment Zeit, um diese Eigenschafts Einstellung aus der deklarativen Syntax vollständig zu entfernen, oder legen Sie Sie auf den Standardwert `{0}`fest.

Nach dem Konfigurieren der `SupplierDetails` DetailsView und `AllSuppliersDataSource` ObjectDataSource verfügen wir über das folgende deklarative Markup:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

An diesem Punkt kann die DetailsView durchlaufen werden, und die ausgewählten Lieferinformationen des Lieferanten können unabhängig von der in der `Suppliers` Dropdown Liste getroffenen Auswahl aktualisiert werden (siehe Abbildung 6).

[![alle Lieferanteninformationen können angezeigt und die Adresse aktualisiert werden.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Abbildung 6**: alle Lieferanteninformationen können angezeigt werden, und Ihre Adresse wird aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))

## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Schritt 3: Anzeigen nur der Informationen des ausgewählten Lieferanten

Auf unserer Seite werden derzeit die Informationen zu allen Lieferanten angezeigt, unabhängig davon, ob ein bestimmter Lieferant aus der Dropdown Liste "`Suppliers`" ausgewählt wurde. Um nur die Lieferanteninformationen für den ausgewählten Lieferanten anzuzeigen, müssen wir der Seite eine weitere ObjectDataSource hinzufügen, die Informationen zu einem bestimmten Lieferanten abruft.

Fügen Sie der Seite ein neues ObjectDataSource-Objekt hinzu, benennen Sie es `SingleSupplierDataSource`. Klicken Sie im Smarttagmenü auf den Link Datenquelle konfigurieren, und verwenden Sie die `SuppliersBLL` Klasse s `GetSupplierBySupplierID(supplierID)` Methode. Wie bei der `AllSuppliersDataSource` ObjectDataSource muss die `SingleSupplierDataSource` ObjectDataSource s `Update()`-Methode der `SuppliersBLL` Class s `UpdateSupplierAddress`-Methode zugeordnet werden.

[![die Methode "singlesupplierdatasource ObjectDataSource" für die Verwendung der Methode getsupplierbysupplierid (SupplierID) konfigurieren.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Abbildung 7**: Konfigurieren der `SingleSupplierDataSource` ObjectDataSource für die Verwendung der `GetSupplierBySupplierID(supplierID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))

Als nächstes werden Sie aufgefordert, die Parameter Quelle für die `GetSupplierBySupplierID(supplierID)` Methode `supplierID` Eingabeparameter anzugeben. Da wir die Informationen für den von der Dropdown Liste ausgewählten Lieferanten anzeigen möchten, verwenden Sie die `Suppliers` DropDownList s `SelectedValue`-Eigenschaft als Parameter Quelle.

[![die Dropdown Liste "Suppliers" als SupplierID-Parameter Quelle verwenden](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Abbildung 8**: Verwenden der Dropdown List-`Suppliers` als `supplierID` Parameter Quelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))

Auch wenn diese zweite ObjectDataSource hinzugefügt wurde, ist das DetailsView-Steuerelement zurzeit so konfiguriert, dass immer die `AllSuppliersDataSource` ObjectDataSource verwendet wird. Wir müssen Logik hinzufügen, um die von der DetailsView verwendete Datenquelle je nach ausgewähltem `Suppliers` DropDownList-Element anzupassen. Um dies zu erreichen, erstellen Sie einen `SelectedIndexChanged` Ereignishandler für die Dropdown Liste "Suppliers". Dies kann am einfachsten durch Doppelklicken auf die Dropdown Liste im Designer erstellt werden. Dieser Ereignishandler muss ermitteln, welche Datenquelle verwendet werden soll, und muss die Daten erneut an die DetailsView binden. Dies wird mit folgendem Code erreicht:

[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Der Ereignishandler bestimmt, ob die Option "alle Lieferanten anzeigen/bearbeiten" ausgewählt wurde. Wenn dies der Fall ist, wird die `SupplierDetails` DetailsView s `DataSourceID` auf `AllSuppliersDataSource` festgelegt, und der Benutzer wird an den ersten Datensatz in der Gruppe von Lieferanten zurückgegeben, indem die `PageIndex`-Eigenschaft auf 0 festgelegt wird. Wenn der Benutzer jedoch einen bestimmten Lieferanten aus der Dropdown Liste ausgewählt hat, wird die DetailsView s-`DataSourceID` `SingleSuppliersDataSource`zugewiesen. Unabhängig davon, welche Datenquelle verwendet wird, wird der `SuppliersDetails` Modus wieder auf den schreibgeschützten Modus zurückgesetzt, und die Daten werden über einen aufzurufenden `SuppliersDetails` Steuerelement-`DataBind()`-Methode an die DetailsView zurückgesetzt.

Wenn dieser Ereignishandler vorhanden ist, zeigt das DetailsView-Steuerelement jetzt den ausgewählten Lieferanten an, es sei denn, die Option "alle Lieferanten anzeigen/bearbeiten" wurde ausgewählt. in diesem Fall können alle Lieferanten über die Paging-Schnittstelle angezeigt werden. Abbildung 9 zeigt die Seite mit ausgewählter Option "alle Lieferanten anzeigen/bearbeiten". Beachten Sie, dass die pagingschnittstelle vorhanden ist, die es dem Benutzer ermöglicht, jeden beliebigen Lieferanten zu besuchen und zu aktualisieren. Abbildung 10 zeigt die Seite mit ausgewähltem ma-maison-Lieferanten. In diesem Fall können nur MA-Maison s-Informationen angezeigt und bearbeitet werden.

[![alle Lieferanteninformationen können angezeigt und bearbeitet werden.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Abbildung 9**: alle Lieferanteninformationen können angezeigt und bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))

[![nur die Informationen des ausgewählten Lieferanten s angezeigt und bearbeitet werden können.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Abbildung 10**: nur die Informationen des ausgewählten Lieferanten s können angezeigt und bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))

> [!NOTE]
> Für dieses Tutorial müssen sowohl das DropDownList-Steuerelement als auch das DetailsView-Steuerelement s-`EnableViewState` auf `true` (Standardeinstellung) festgelegt werden, da die Änderungen DropDownList s `SelectedIndex` und DetailsView s `DataSourceID` Eigenschaften s über Postbacks hinweg gespeichert werden müssen.

## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Schritt 4: Auflisten der Lieferanten Produkte in einer bearbeitbaren GridView

Wenn die DetailsView fertiggestellt ist, besteht der nächste Schritt darin, eine bearbeitbare GridView-Struktur einzuschließen, in der die Produkte des ausgewählten Lieferanten aufgeführt sind. In dieser GridView sollten nur die Felder `ProductName` und `QuantityPerUnit` bearbeitet werden können. Wenn der Benutzer, der die Seite besucht, von einem bestimmten Lieferanten gehört, sollte er außerdem nur Updates für diese Produkte zulassen, die *nicht* eingestellt werden. Um dies zu erreichen, müssen wir zuerst eine Überladung der `ProductsBLL` Klasse s `UpdateProducts` Methode hinzufügen, die nur die Felder `ProductID`, `ProductName`und `QuantityPerUnit` als Eingaben annimmt. Wir haben diesen Prozess bereits in zahlreichen Tutorials schrittweise durchlaufen. betrachten Sie also den Code hier, der `ProductsBLL`hinzugefügt werden sollte:

[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Nachdem diese Überladung erstellt wurde, können wir das GridView-Steuerelement und dessen zugeordnete ObjectDataSource hinzufügen. Fügen Sie der Seite eine neue GridView hinzu, legen Sie die `ID`-Eigenschaft auf `ProductsBySupplier`fest, und konfigurieren Sie Sie für die Verwendung einer neuen ObjectDataSource mit dem Namen `ProductsBySupplierDataSource`. Da in dieser GridView diese Produkte vom ausgewählten Lieferanten aufgelistet werden sollen, verwenden Sie die `ProductsBLL` Class s `GetProductsBySupplierID(supplierID)`-Methode. Ordnen Sie außerdem die `Update()`-Methode der neuen `UpdateProduct` Überladung zu, die wir soeben erstellt haben.

[![Konfigurieren von ObjectDataSource für die Verwendung der soeben erstellten UpdateProduct-Überladung](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Abbildung 11**: Konfigurieren von ObjectDataSource für die Verwendung der soeben erstellten `UpdateProduct` Überladung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))

Wir haben erneut aufgefordert, die Parameter Quelle für die `GetProductsBySupplierID(supplierID)` Methode `supplierID` Eingabeparameter auszuwählen. Da wir die Produkte für den in der DetailsView ausgewählten Lieferanten anzeigen möchten, verwenden Sie die `SuppliersDetails` DetailsView-Steuerelement `SelectedValue`-Eigenschaft als Parameter Quelle.

[![die Eigenschaft "suppliersdetails DetailsView s SelectedValue" als Parameter Quelle verwenden.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Abbildung 12**: Verwenden der `SuppliersDetails` DetailsView s `SelectedValue`-Eigenschaft als Parameter Quelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))

Wenn Sie zur GridView zurückkehren, entfernen Sie alle GridView-Felder außer `ProductName`, `QuantityPerUnit`und `Discontinued`, wobei Sie das `Discontinued` CheckBoxField als schreibgeschützt markieren. Aktivieren Sie außerdem die Option zum Aktivieren der Bearbeitung im GridView s-Smarttag. Nachdem diese Änderungen vorgenommen wurden, sollte das deklarative Markup für GridView und ObjectDataSource in etwa wie folgt aussehen:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Wie bei unseren vorherigen objectdatasources ist diese Eigenschaft `OldValuesParameterFormatString` auf `original_{0}`festgelegt, was zu Problemen führt, wenn versucht wird, einen Produkt-e-Namen oder eine Menge pro Einheit zu aktualisieren. Entfernen Sie diese Eigenschaft vollständig aus der deklarativen Syntax, oder legen Sie Sie auf den Standardwert (`{0}`) fest.

Nach Abschluss dieser Konfiguration listet unsere Seite nun die Produkte auf, die vom in der GridView ausgewählten Lieferanten bereitgestellt werden (siehe Abbildung 13). *Zurzeit* kann der Name oder die Menge der Produkte pro Einheit aktualisiert werden. Wir müssen unsere Seiten Logik jedoch aktualisieren, damit diese Funktionalität für Benutzer, die einem bestimmten Lieferanten zugeordnet sind, nicht unterstützt wird. Dieser letzte Schritt wird in Schritt 5 behandelt.

[![werden die Produkte angezeigt, die vom ausgewählten Lieferanten bereitgestellt werden.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Abbildung 13**: die vom ausgewählten Lieferanten bereitgestellten Produkte werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))

> [!NOTE]
> Durch das Hinzufügen dieses bearbeitbaren GridView-Ereignisses sollte der `Suppliers` DropDownList s `SelectedIndexChanged`-Ereignishandler aktualisiert werden, um die GridView in einen schreibgeschützten Zustand zurückzusetzen. Wenn ein anderer Lieferant während der Bearbeitung von Produktinformationen ausgewählt wird, kann der entsprechende Index in der GridView für den neuen Lieferanten auch bearbeitet werden. Um dies zu verhindern, legen Sie einfach die GridView s `EditIndex`-Eigenschaft auf `-1` im `SelectedIndexChanged`-Ereignishandler fest.

Beachten Sie auch, dass es wichtig ist, dass der GridView s-Ansichts Zustand aktiviert ist (das Standardverhalten). Wenn Sie die Eigenschaft GridView s `EnableViewState` auf `false`festlegen, besteht das Risiko, dass gleichzeitige Benutzerdaten Sätze versehentlich löschen oder bearbeiten. Weitere Informationen finden Sie [unter Warnung: Parallelitäts Problem mit ASP.NET 2,0 GridViews/DetailsView/formviews, die das Bearbeiten und/oder löschen unterstützen und deren Ansichts Zustand deaktiviert ist](http://scottonwriting.net/sowblog/posts/10054.aspx) .

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Schritt 5: Deaktivieren der Bearbeitung für nicht mehr unterstützte Produkte, wenn alle Lieferanten anzeigen/bearbeiten nicht ausgewählt ist

Obwohl die `ProductsBySupplier` GridView voll funktionsfähig ist, gewährt Sie derzeit zu viel Zugriff auf die Benutzer eines bestimmten Lieferanten. Gemäß den Geschäftsregeln sollten solche Benutzer nicht mehr unterstützte Produkte aktualisieren können. Um dies zu erzwingen, können wir die Bearbeitungs Schaltfläche in den GridView-Zeilen mit nicht mehr unterstützten Produkten ausblenden (oder deaktivieren), wenn die Seite von einem Benutzer von einem Lieferanten besucht wird.

Erstellen Sie einen Ereignishandler für das GridView s `RowDataBound`-Ereignis. In diesem Ereignishandler muss festgestellt werden, ob der Benutzer einem bestimmten Lieferanten zugeordnet ist, der für dieses Tutorial durch Überprüfen der DropDownList s-`SelectedValue` Eigenschaft von Suppliers ermittelt werden kann, wenn er etwas anderes als-1 ist, dann wird der Benutzer einem bestimmten Lieferanten zugeordnet. Für solche Benutzer müssen wir dann bestimmen, ob das Produkt nicht mehr unterstützt wird. Wir können einen Verweis auf die tatsächliche `ProductRow`-Instanz, die an die GridView-Zeile gebunden ist, über die `e.Row.DataItem`-Eigenschaft erfassen, wie im Tutorial [*Anzeigen von Zusammenfassungs Informationen in der GridView-footer-footer*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) erläutert. Wenn das Produkt nicht mehr unterstützt wird, können Sie mithilfe der im vorherigen Tutorial beschriebenen Techniken einen programmgesteuerten Verweis auf die Schaltfläche "Bearbeiten" im GridView s-CommandField aufrufen, indem Sie eine [*Client seitige Bestätigung beim Löschen hinzufügen*](adding-client-side-confirmation-when-deleting-cs.md). Sobald ein Verweis vorhanden ist, können wir die Schaltfläche ausblenden oder deaktivieren.

[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Wenn dieser Ereignishandler vorhanden ist, wird beim Aufrufen dieser Seite als Benutzer von einem bestimmten Lieferanten nicht mehr bearbeitbare Produkte angezeigt, da die Bearbeitungs Schaltfläche für diese Produkte ausgeblendet ist. Beispielsweise ist Chef Anton s Gumbo Mix ein nicht mehr unterstütztes Produkt für den neuen Lieferanten von Orleans Cajun Delights. Wenn Sie die Seite für diesen bestimmten Lieferanten aufrufen, wird die Schaltfläche "Bearbeiten" für dieses Produkt in Sight ausgeblendet (siehe Abbildung 14). Wenn Sie jedoch mit "alle Lieferanten anzeigen/bearbeiten" aufrufen, ist die Schaltfläche "Bearbeiten" verfügbar (siehe Abbildung 15).

[![für Lieferanten spezifische Benutzer die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix ist ausgeblendet.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Abbildung 14**: für Lieferanten spezifische Benutzer wird die Bearbeitungs Schaltfläche für Chef Anton s Gumbo Mix ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))

[![für alle Lieferanten Benutzer anzeigen/bearbeiten wird die Schaltfläche Bearbeiten für Chef Anton s Gumbo Mix angezeigt.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Abbildung 15**: zum Anzeigen/Bearbeiten aller Lieferanten Benutzer wird die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))

## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Überprüfen der Zugriffsrechte in der Geschäftslogik Schicht

In diesem Tutorial verarbeitet die ASP.NET-Seite alle Logik in Bezug auf die Informationen, die der Benutzer sehen kann, und welche Produkte er aktualisieren kann. Im Idealfall wäre diese Logik auch auf der Geschäftslogik Schicht vorhanden. Beispielsweise kann die `SuppliersBLL` Class s `GetSuppliers()`-Methode (die alle Lieferanten zurückgibt) eine Prüfung einschließen, um sicherzustellen, dass der aktuell angemeldete Benutzer *keinem* bestimmten Lieferanten zugeordnet ist. Ebenso kann die `UpdateSupplierAddress` Methode eine Prüfung enthalten, um sicherzustellen, dass der aktuell angemeldete Benutzer entweder für unser Unternehmen gearbeitet hat (und daher alle Adressinformationen des Lieferanten aktualisieren kann) oder dem Lieferanten zugeordnet ist, dessen Daten aktualisiert werden.

Diese Überprüfungen der BLL-Schicht sind hier nicht enthalten, da die Rechte des Benutzers in unserem Tutorial durch eine Dropdown List auf der Seite bestimmt werden, auf die die BLL-Klassen nicht zugreifen können. Bei Verwendung des Mitgliedschafts Systems oder eines der Standard Authentifizierungs Schemas, die von ASP.NET bereitgestellt werden (z. b. Windows-Authentifizierung), kann der Zugriff auf die Informationen und Rollen Informationen des aktuell angemeldeten Benutzers über die BLL erfolgen, wodurch ein solcher Zugriff möglich ist. auf der Präsentations-und der BLL-Ebene können Rechte überprüft werden.

## <a name="summary"></a>Zusammenfassung

Die meisten Websites, die Benutzerkonten bereitstellen, müssen die Daten Änderungs Schnittstelle basierend auf dem angemeldeten Benutzer anpassen. Administratoren können möglicherweise jeden Datensatz löschen und bearbeiten, während Benutzer ohne Administrator Beschränkung darauf beschränkt sind, nur die von Ihnen erstellten Datensätze zu aktualisieren oder zu löschen. Je nach Szenario kann es sein, dass die Klassen Data Web Controls, ObjectDataSource und Business Logic Layer erweitert werden können, um bestimmte Funktionen auf der Grundlage des angemeldeten Benutzers hinzuzufügen oder zu verweigern. In diesem Tutorial wurde erläutert, wie Sie die sichtbaren und bearbeitbaren Daten begrenzen können, je nachdem, ob der Benutzer einem bestimmten Lieferanten zugeordnet wurde oder ob er für unser Unternehmen tätig war.

In diesem Tutorial wird die Untersuchung des Einfügens, Aktualisierens und Löschens von Daten mithilfe der Steuerelemente GridView, DetailsView und FormView abgeschlossen. Beginnend mit dem nächsten Tutorial wird das Hinzufügen von Paging-und Sortierungs Unterstützung berücksichtigt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](adding-client-side-confirmation-when-deleting-cs.md)
> [Weiter](an-overview-of-inserting-updating-and-deleting-data-vb.md)
