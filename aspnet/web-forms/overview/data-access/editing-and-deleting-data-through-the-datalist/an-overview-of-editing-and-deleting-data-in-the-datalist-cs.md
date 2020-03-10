---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Eine Übersicht über das Bearbeiten und Löschen von Daten im DataListC#-Steuerelemente () | Microsoft-Dokumentation
author: rick-anderson
description: Während dem DataList die integrierten Funktionen zum Bearbeiten und löschen fehlen, sehen Sie in diesem Tutorial, wie Sie einen DataList erstellen, der das Bearbeiten und Löschen von o... unterstützt.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 481c9a14b1ebfe36ffcddd0237701bc04266e393
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78480915"
---
# <a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Eine Übersicht über das Bearbeiten und Löschen von Daten im DataListC#-Steuerelemente ()

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) oder [PDF herunterladen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Während dem DataList die integrierten Funktionen zum Bearbeiten und löschen fehlen, sehen Sie in diesem Tutorial, wie Sie einen DataList erstellen, der das Bearbeiten und löschen seiner zugrunde liegenden Daten unterstützt.

## <a name="introduction"></a>Einführung

In der [Übersicht über das Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) haben wir uns mit dem Einfügen, aktualisieren und Löschen von Daten mit der Anwendungsarchitektur, einem ObjectDataSource-Steuerelement und dem GridView-, DetailsView-und FormView-Steuerelement beschäftigt. Mit dem ObjectDataSource-Steuerelement und diesen drei datenweb-Steuerelementen war das Implementieren von Schnittstellen für die einfache Datenänderung ein Snap-in, das nur ein Kontrollkästchen von einem Smarttags Es muss kein Code geschrieben werden.

Leider fehlen dem DataList die integrierten Funktionen zum Bearbeiten und löschen, die im GridView-Steuerelement enthalten sind. Diese fehlende Funktion ist Teil der Tatsache, dass der DataList ein Relic aus der früheren Version von ASP.net ist, wenn deklarative Datenquellen-Steuerelemente und Code freie Daten Änderungs Seiten nicht verfügbar waren. Obwohl der DataList in ASP.NET 2,0 nicht die gleichen Funktionen für die Datenänderung bietet wie die GridView, können wir ASP.NET 1. x-Techniken verwenden, um solche Funktionen einzubeziehen. Diese Vorgehensweise erfordert etwas Code, aber wie in diesem Tutorial zu sehen ist, enthält der DataList einige Ereignisse und Eigenschaften, die zur Unterstützung dieses Prozesses vorhanden sind.

In diesem Lernprogramm erfahren Sie, wie Sie einen DataList erstellen, der das Bearbeiten und Löschen der zugrunde liegenden Daten unterstützt. In zukünftigen Tutorials werden erweiterte Bearbeitungs-und Lösch Szenarien, einschließlich der Überprüfung des Eingabe Felds, die ordnungsgemäße Behandlung von Ausnahmen, die von den Datenzugriffs-oder Geschäftslogik Schichten ausgelöst werden, und so weiter untersucht.

> [!NOTE]
> Wie das DataList-Steuerelement fehlen dem Repeater-Steuerelement die Standardfunktionen für das Einfügen, aktualisieren oder löschen. Obwohl diese Funktionalität hinzugefügt werden kann, enthält der DataList Eigenschaften und Ereignisse, die im Wiederholungs Modul nicht gefunden wurden und das Hinzufügen solcher Funktionen vereinfachen. Dieses Lernprogramm und zukünftige Elemente, die sich mit dem Bearbeiten und löschen befassen, konzentrieren sich daher ausschließlich auf den DataList.

## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Schritt 1: Erstellen der Webseiten zum Bearbeiten und Löschen von Tutorials

Bevor wir uns mit dem Aktualisieren und Löschen von Daten aus einem DataList beschäftigen, nehmen Sie sich zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen, und die nächsten. Fügen Sie zunächst einen neuen Ordner mit dem Namen `EditDeleteDataList`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Fügen Sie die ASP.NET-Seiten für die Tutorials hinzu.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen der ASP.NET-Seiten für die Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `EditDeleteDataList` die Lernprogramme in diesem Abschnitt auflistet. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))

Fügen Sie abschließend die Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere das folgende Markup nach den Master-/Detailberichten mit dem DataList-und Repeater-`<siteMapNode>`hinzu:

[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die DataList-Tutorials zum Bearbeiten und löschen.

![Die Site Übersicht enthält jetzt Einträge für die DataList-Tutorials zum Bearbeiten und löschen.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Abbildung 3**: die Site Übersicht enthält jetzt Einträge für die DataList-Tutorials zum Bearbeiten und löschen.

## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Schritt 2: Untersuchen von Techniken zum Aktualisieren und Löschen von Daten

Das Bearbeiten und Löschen von Daten mit der GridView ist so einfach, da das GridView-Objekt und das ObjectDataSource-Objekt unter dem Hintergrund in der Arbeit arbeiten. Wie im Tutorial unter [Suchen der Ereignisse im Zusammenhang mit dem Einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) erläutert, weist das GridView-Objekt automatisch seine Felder, die die bidirektionale Datenbindung verwendet haben, der `UpdateParameters`-Auflistung von ObjectDataSource zu und ruft dann diese ObjectDataSource s `Update()` Methode auf.

Leider stellt der DataList keine dieser integrierten Funktionen bereit. Sie müssen sicherstellen, dass die Werte des Benutzers den Parametern von ObjectDataSource zugewiesen werden und dass die `Update()`-Methode aufgerufen wird. Zur Unterstützung dieses Vorhabens bietet der DataList die folgenden Eigenschaften und Ereignisse:

- **Die [`DataKeyField`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  beim Aktualisieren oder löschen muss jedes Element im DataList eindeutig identifizieren können. Legen Sie diese Eigenschaft auf das Primärschlüssel Feld der angezeigten Daten fest. Dadurch wird die DataList s- [`DataKeys` Collection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) mit dem angegebenen `DataKeyField` Wert für jedes DataList-Element aufgefüllt.
- **Das [`EditCommand`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  wird ausgelöst, wenn auf eine Schaltfläche, einen LinkButton oder ein ImageButton geklickt wird, dessen `CommandName`-Eigenschaft auf Bearbeiten festgelegt ist.
- **Das [`CancelCommand`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  wird ausgelöst, wenn auf eine Schaltfläche, einen LinkButton oder ein ImageButton geklickt wird, dessen `CommandName`-Eigenschaft auf Abbrechen festgelegt ist.
- **Das [`UpdateCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  wird ausgelöst, wenn auf eine Schaltfläche, ein LinkButton oder ein ImageButton geklickt wird, dessen `CommandName`-Eigenschaft auf Update festgelegt ist.
- **Das [`DeleteCommand`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  wird ausgelöst, wenn auf eine Schaltfläche, einen LinkButton oder ein ImageButton geklickt wird, dessen `CommandName`-Eigenschaft auf Löschen festgelegt ist.

Mithilfe dieser Eigenschaften und Ereignisse gibt es vier Ansätze, mit denen Sie Daten aus dem DataList aktualisieren und löschen können:

1. **Mithilfe von ASP.NET 1. x-Techniken** war der DataList vor ASP.NET 2,0 und objectdatasources vorhanden und konnte Daten vollständig über programmgesteuerte Mittel aktualisieren und löschen. Diese Technik Ruft die ObjectDataSource vollständig ab und erfordert, dass die Daten direkt aus der Geschäftslogik Schicht an den DataList gebunden werden, und zwar sowohl beim Abrufen der anzuzeigenden Daten als auch beim Aktualisieren oder Löschen eines Datensatzes.
2. Die **Verwendung eines einzelnen ObjectDataSource-Steuer Elements auf der Seite zum auswählen, aktualisieren und löschen** , während dem DataList-Steuerelement keine Funktionen zum Bearbeiten und Löschen von GridView-Funktionen fehlen, gibt es keinen Grund, den wir nicht in sich selbst hinzufügen können. Bei dieser Vorgehensweise verwenden wir eine ObjectDataSource wie in den GridView-Beispielen, müssen jedoch einen Ereignishandler für das DataList s-`UpdateCommand` Ereignis erstellen, bei dem die ObjectDataSource s-Parameter festgelegt und die `Update()`-Methode aufgerufen wird.
3. Wenn **Sie ein ObjectDataSource-Steuerelement zum Auswählen von, aber aktualisieren und Löschen bei der** Verwendung von Option 2 verwenden, müssen wir ein wenig Code in das `UpdateCommand`-Ereignis schreiben und Parameterwerte zuweisen usw. Stattdessen können wir uns mit der Verwendung von ObjectDataSource für die Auswahl von "ObjectDataSource" (z. b. mit Option 1) angleichen. Meiner Meinung nach führt das Aktualisieren von Daten durch direkte Interaktion mit der BLL zu mehr lesbarem Code als das Zuweisen der ObjectDataSource s `UpdateParameters` und das Aufrufen der `Update()`-Methode.
4. Die **Verwendung deklarativer Mittel durch mehrere objectdatasources** erfordert, dass die vorherigen drei Ansätze etwas Code erfordern. Wenn Sie stattdessen möglichst viel deklarative Syntax verwenden, besteht die letzte Möglichkeit darin, mehrere objectdatasources auf der Seite einzubeziehen. Die erste ObjectDataSource Ruft die Daten aus der BLL ab und bindet Sie an den DataList. Zum Aktualisieren wird eine andere ObjectDataSource hinzugefügt, die jedoch direkt in der DataList-`EditItemTemplate`hinzugefügt wird. Um die Unterstützung für das Löschen einzuschließen, wäre eine andere ObjectDataSource im `ItemTemplate`erforderlich. Bei dieser Vorgehensweise verwenden diese eingebetteten ObjectDataSource-Objekte `ControlParameters`, um deklarativ die ObjectDataSource s-Parameter an die Benutzereingabe-Steuerelemente zu binden (anstatt Sie Programm gesteuert im DataList s `UpdateCommand`-Ereignishandler anzugeben). Dieser Ansatz erfordert weiterhin einen Teil des Codes, den Sie benötigen, um den eingebetteten ObjectDataSource s-`Update()` oder `Delete()`-Befehl aufzurufen, erfordert jedoch weitaus weniger als bei den anderen drei Ansätzen. Der Nachteil hierbei ist, dass mehrere objectdatasources die Seite überladen und dabei von der allgemeinen Lesbarkeit abgezogen werden.

Wenn die Anwendung nur einen dieser Ansätze verwendet, wähle ich Option 1 aus, da Sie die größtmögliche Flexibilität bietet und der DataList ursprünglich für dieses Muster konzipiert wurde. Obwohl das DataList-Steuerelement für die Arbeit mit den ASP.NET 2,0-Datenquellen-Steuerelementen erweitert wurde, verfügt es nicht über alle Erweiterungs Punkte oder Features der offiziellen ASP.NET 2,0-datenweb Steuerelemente (GridView, DetailsView und FormView). Die Optionen 2 bis 4 sind jedoch nicht ohne Verdienst.

In diesem und den zukünftigen Tutorials zum Bearbeiten und löschen wird eine ObjectDataSource zum Abrufen der anzuzeigenden Daten und zum Weiterleiten von Aufrufen der BLL zum Aktualisieren und Löschen von Daten verwendet (Option 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Schritt 3: Hinzufügen von DataList und Konfigurieren von ObjectDataSource

In diesem Tutorial erstellen wir einen DataList, der Produktinformationen auflistet, und für jedes Produkt bietet dem Benutzer die Möglichkeit, den Namen und den Preis zu bearbeiten und das Produkt vollständig zu löschen. Insbesondere rufen wir die Datensätze ab, die mithilfe von ObjectDataSource angezeigt werden sollen, führen jedoch die Aktualisierungs-und Lösch Aktionen aus, indem Sie direkt mit der BLL interagieren. Bevor wir uns mit der Implementierung der Bearbeitungs-und Löschfunktionen für den DataList beschäftigen, wird die Seite zuerst von der Seite zum Anzeigen der Produkte in einer schreibgeschützten Oberfläche angezeigt. Da wir diese Schritte in vorherigen Tutorials überprüft haben, werde ich Sie schnell durchgehen.

Öffnen Sie zunächst die Seite `Basics.aspx` im Ordner `EditDeleteDataList`, und fügen Sie der Seite aus dem Designansicht einen DataList hinzu. Erstellen Sie als nächstes ein neues ObjectDataSource-Objekt aus dem DataList s-Smarttag. Da wir mit Produktdaten arbeiten, konfigurieren Sie Sie so, dass Sie die `ProductsBLL`-Klasse verwenden. Um *alle* Produkte abzurufen, wählen Sie auf der Registerkarte auswählen die `GetProducts()` Methode aus.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbll-Klasse.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))

[![die Produktinformationen mithilfe der GetProducts ()-Methode zurückgeben](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Abbildung 5**: Zurückgeben der Produktinformationen mithilfe der `GetProducts()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))

Der DataList, wie z. b. GridView, ist nicht für das Einfügen neuer Daten konzipiert. Wählen Sie daher in der Dropdown Liste auf der Registerkarte Einfügen die Option (keine) aus. Wählen Sie für die Registerkarten aktualisieren und löschen außerdem (keine) aus, da Updates und Löschvorgänge Programm gesteuert über die BLL ausgeführt werden.

[![bestätigen, dass die Dropdown Listen in den Registerkarten "Einfügen", "Aktualisieren" und "Löschen" von ObjectDataSource auf (keine) festgelegt sind.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Abbildung 6**: überprüfen, ob die Dropdown Listen in den Registerkarten "Einfügen", "Aktualisieren" und "Löschen" von ObjectDataSource auf (keine) festgelegt sind ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))

Klicken Sie nach dem Konfigurieren von ObjectDataSource auf Fertigstellen, und kehren Sie zum Designer zurück. Wie bereits in den vergangenen Beispielen gezeigt, erstellt Visual Studio beim Abschließen der Konfiguration von ObjectDataSource automatisch eine `ItemTemplate` für die Dropdown Liste, die jedes der Datenfelder anzeigt. Ersetzen Sie diese `ItemTemplate` durch eine, in der nur der Name und der Preis des Produkts angezeigt werden. Legen Sie außerdem die `RepeatColumns`-Eigenschaft auf 2 fest.

> [!NOTE]
> Wie im Tutorial Übersicht über das *Einfügen, aktualisieren und Löschen von Daten* erläutert, muss beim Ändern von Daten mithilfe von ObjectDataSource die `OldValuesParameterFormatString`-Eigenschaft aus dem deklarativen Markup von ObjectDataSource (oder dem Standardwert `{0}`) entfernt werden. In diesem Tutorial verwenden wir jedoch ObjectDataSource nur zum Abrufen von Daten. Daher müssen wir den Eigenschafts Wert von ObjectDataSource s `OldValuesParameterFormatString` nicht ändern (obwohl er dies nicht tut).

Nachdem Sie die standarddatalist-`ItemTemplate` durch eine angepasste ersetzt haben, sollte das deklarative Markup auf der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Nehmen Sie sich einen Moment Zeit, um den Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 7 dargestellt, zeigt der DataList den Produktnamen und den Preis für jedes Produkt in zwei Spalten an.

[![die Namen und Preise der Produkte in einem DataList mit zwei Spalten angezeigt.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Abbildung 7**: die Produktnamen und-Preise werden in einem zwei spaltigen DataList angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))

> [!NOTE]
> Der DataList verfügt über eine Reihe von Eigenschaften, die für den Aktualisierungs-und Löschvorgang erforderlich sind, und diese Werte werden im Ansichts Zustand gespeichert. Daher ist es bei der Erstellung eines DataList-Datentyps, der das Bearbeiten oder Löschen von Daten unterstützt, erforderlich, dass der Ansichts Zustand der DataList-  
>   
> Der behebbare Reader kann sich erinnern, dass wir den Ansichts Zustand beim Erstellen bearbeitbarer GridViews, DetailsViews und formviews deaktivieren konnten. Dies liegt daran, dass ASP.NET 2,0- *websteuer*Elemente den Steuerelement Zustand einschließen können. Dies ist ein Zustand, der über Postbacks wie den Ansichts Zustand beibehalten wird

Durch das Deaktivieren des Ansichts Zustands in der GridView werden nur triviale Zustandsinformationen ausgelassen, aber der Steuerelement Zustand (einschließlich des zum Bearbeiten und Löschen erforderlichen Status) wird beibehalten. Der DataList, der im ASP.NET 1. x-Zeitrahmen erstellt wurde, verwendet nicht den Steuerelement Zustand und muss daher den Ansichts Zustand aktiviert haben. Weitere Informationen zum Zweck des Steuerungs Zustands und dessen Unterschiede zum Ansichts Zustand finden Sie unter [Control State or View State](https://msdn.microsoft.com/library/1whwt1k7.aspx) .

## <a name="step-4-adding-an-editing-user-interface"></a>Schritt 4: Hinzufügen einer Bearbeitungs Benutzeroberfläche

Das GridView-Steuerelement besteht aus einer Auflistung von Feldern (boundfields, checkboxfields, templatefields usw.). Diese Felder können Ihr gerendertes Markup abhängig von Ihrem Modus anpassen. Wenn z. b. im schreibgeschützten Modus ein BoundField den Wert des Daten Felds als Text anzeigt, im Bearbeitungsmodus wird ein TextBox-websteuer Element gerendert, dessen `Text`-Eigenschaft dem Daten Feldwert zugewiesen wird.

Der DataList hingegen rendert seine Elemente mithilfe von Vorlagen. Schreibgeschützte Elemente werden mithilfe der `ItemTemplate` gerendert, während Elemente im Bearbeitungsmodus über die `EditItemTemplate`gerendert werden. An diesem Punkt hat unser DataList nur eine `ItemTemplate`. Um Bearbeitungsfunktionen auf Element Ebene zu unterstützen, müssen wir eine `EditItemTemplate` hinzufügen, die das Markup enthält, das für das bearbeitbare Element angezeigt werden soll. In diesem Tutorial verwenden wir TextBox-websteuer Elemente zum Bearbeiten des Namens und des Einheits Preises des Produkts.

Der `EditItemTemplate` kann entweder deklarativ oder über den Designer erstellt werden (durch Auswählen der Option Vorlagen bearbeiten aus dem DataList s Smarttags). Wenn Sie die Option Vorlagen bearbeiten verwenden möchten, klicken Sie zuerst auf den Link Vorlagen bearbeiten im Smarttags, und wählen Sie dann in der Dropdown Liste das `EditItemTemplate` Element aus.

[![Sie sich für die Arbeit mit dem DataList-EditItemTemplate-Element entscheiden.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Abbildung 8**: Arbeiten mit dem DataList-`EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))

Geben Sie als nächstes Product Name: und Price: ein, und ziehen Sie dann zwei TextBox-Steuerelemente aus der Toolbox in die `EditItemTemplate`-Schnittstelle im Designer. Legen Sie die Textfelder `ID` Eigenschaften auf `ProductName` und `UnitPrice`fest.

[![ein Textfeld für den Namen und den Preis des Produkts hinzufügen.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Abbildung 9**: Hinzufügen eines Textfelds für den Namen und den Preis des Produkts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))

Wir müssen die entsprechenden Product Data-Feldwerte an die `Text` Eigenschaften der beiden Textfelder binden. Klicken Sie in den Textfeldern Smarttags auf den Link DataBindings bearbeiten, und ordnen Sie dann dem `Text`-Eigenschaft das entsprechende Datenfeld zu, wie in Abbildung 10 dargestellt.

> [!NOTE]
> Wenn Sie das `UnitPrice` Datenfeld an das Feld "Price TextBox s `Text`" binden, können Sie es als Währungswert (`{0:C}`), als allgemeine Zahl (`{0:N}`) oder als nicht formatiert formatieren.

![Binden Sie die Datenfelder ProductName und UnitPrice an die Text-Eigenschaften der Textfelder.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Abbildung 10**: Binden der Felder für `ProductName` und `UnitPrice` Daten an die `Text` Eigenschaften der Textfelder

Beachten Sie, dass das Dialogfeld DataBindings bearbeiten in Abbildung 10 das bidirektionale Datenbindung-Kontroll *Kästchen enthält,* das beim Bearbeiten eines TemplateField in der GridView-oder DetailsView oder einer Vorlage in der FormView vorhanden ist. Mit der bidirektionalen Datenbindung-Funktion konnte der Wert, der in das Eingabe-websteuer Element eingegeben wurde, beim Einfügen oder Aktualisieren von Daten automatisch den entsprechenden ObjectDataSource s `InsertParameters` oder `UpdateParameters` zugewiesen werden. Der DataList unterstützt die bidirektionale Datenbindung nicht, wie wir später in diesem Tutorial sehen werden, nachdem der Benutzer die Änderungen vorgenommen hat und bereit ist, die Daten zu aktualisieren, müssen wir Programm gesteuert auf diese Textfelder `Text` Eigenschaften zugreifen und deren Werte an die entsprechende `UpdateProduct` Methode in der `ProductsBLL`-Klasse übergeben.

Zum Schluss müssen die Schaltflächen aktualisieren und Abbrechen der `EditItemTemplate`hinzugefügt werden. Wie wir im [Master/Detail mithilfe einer aufgeschachtelten Liste von Master Datensätzen mit einem DataList-DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) -Lernprogramm festgelegt haben, wird beim Klicken auf eine Schaltfläche, eine LinkButton oder ein ImageButton, deren `CommandName`-Eigenschaft auf festgelegt ist, in einem Repeater oder DataList das Ereignis Repeater oder DataList s `ItemCommand` ausgelöst. Wenn für DataList die `CommandName`-Eigenschaft auf einen bestimmten Wert festgelegt ist, kann auch ein zusätzliches Ereignis ausgelöst werden. Zu den besonderen `CommandName` Eigenschafts Werten gehören u.a.:

- Abbrechen löst das `CancelCommand`-Ereignis aus.
- Bearbeiten löst das `EditCommand`-Ereignis aus.
- Update löst das `UpdateCommand`-Ereignis aus.

Beachten Sie, dass diese Ereignisse *zusätzlich zum* `ItemCommand` Ereignis ausgelöst werden.

Fügen Sie dem `EditItemTemplate` zwei Schaltflächen-websteuer Elemente hinzu, von denen eine `CommandName` auf "Aktualisieren" festgelegt ist, und die anderen s auf "Abbrechen" Nach dem Hinzufügen dieser zwei Schaltflächen-websteuer Elemente sollte der Designer in etwa wie folgt aussehen:

[![die Schaltflächen aktualisieren und Abbrechen zu EditItemTemplate hinzufügen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Abbildung 11**: Hinzufügen der Schaltflächen zum Aktualisieren und Abbrechen zum `EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))

Wenn Sie die `EditItemTemplate` vervollständigen, sollte das deklarative Markup des DataList s in etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Schritt 5: Hinzufügen der Grundlagen zum Wechseln in den Bearbeitungsmodus

An diesem Punkt hat unser DataList eine Bearbeitungs Schnittstelle definiert, die über seine `EditItemTemplate`definiert ist. Es gibt jedoch derzeit keine Möglichkeit für einen Benutzer, auf unserer Seite zu zeigen, dass er die Informationen eines Produkts bearbeiten möchte. Wir müssen jedem Produkt eine Bearbeitungs Schaltfläche hinzufügen, bei der das DataList-Element im Bearbeitungsmodus gerendert wird, wenn darauf geklickt wird. Fügen Sie zunächst dem `ItemTemplate`eine Bearbeitungs Schaltfläche hinzu, entweder über den Designer oder deklarativ. Stellen Sie sicher, dass die Bearbeitungs Schaltfläche s `CommandName` Eigenschaft auf Bearbeiten festgelegt ist.

Nachdem Sie diese Bearbeitungs Schaltfläche hinzugefügt haben, nehmen Sie sich einen Moment Zeit, um die Seite in einem Browser anzuzeigen. Mit dieser Addition sollte jede Produkt Auflistung eine Bearbeitungs Schaltfläche enthalten.

[![die Schaltflächen aktualisieren und Abbrechen zu EditItemTemplate hinzufügen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Abbildung 12**: Hinzufügen der Schaltflächen zum Aktualisieren und Abbrechen zum `EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))

Wenn Sie auf die Schaltfläche klicken, wird ein Postback ausgelöst, aber die Produkt Auflistung wird *nicht* in den Bearbeitungsmodus versetzt. Um das Produkt bearbeitbar zu machen, müssen Sie Folgendes durchführen:

1. Legen Sie die DataList s [`EditItemIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des `DataListItem` fest, dessen Bearbeitungs Schaltfläche soeben geklickt wurde.
2. Binden Sie die Daten erneut an den DataList. Wenn der DataList erneut gerendert wird, wird die `DataListItem`, deren `ItemIndex` dem DataList-`EditItemIndex` entspricht, mithilfe der `EditItemTemplate`gerendert.

Da das DataList s-`EditCommand` Ereignis beim Klicken auf die Schaltfläche Bearbeiten ausgelöst wird, erstellen Sie einen `EditCommand`-Ereignishandler mit folgendem Code:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

Der `EditCommand` Ereignishandler wird in ein Objekt des Typs `DataListCommandEventArgs` als zweiter Eingabeparameter übergeben, der einen Verweis auf die `DataListItem` enthält, auf deren Bearbeitungs Schaltfläche geklickt wurde (`e.Item`). Der Ereignishandler legt die DataList s-`EditItemIndex` zuerst auf die `ItemIndex` der bearbeitbaren `DataListItem` fest und bindet die Daten dann erneut an den DataList, indem die DataList s-`DataBind()`-Methode aufgerufen wird.

Nachdem Sie diesen Ereignishandler hinzugefügt haben, besuchen Sie die Seite in einem Browser. Wenn Sie auf die Schaltfläche "Bearbeiten" klicken, wird das angeklickte Produkt bearbeitet (siehe Abbildung 13).

[Wenn Sie ![auf die Schaltfläche Bearbeiten klicken, wird das Produkt bearbeitet.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Abbildung 13**: durch Klicken auf die Schaltfläche Bearbeiten wird das Produkt bearbeitbar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))

## <a name="step-6-saving-the-user-s-changes"></a>Schritt 6: Speichern der Benutzer-e-Änderungen

Wenn Sie auf die Schaltflächen Aktualisieren oder Abbrechen der bearbeiteten Produkte klicken, wird an dieser Stelle nichts unternommen. um diese Funktionalität hinzuzufügen, müssen wir Ereignishandler für die DataList s-`UpdateCommand` und `CancelCommand`-Ereignisse erstellen. Beginnen Sie mit dem Erstellen des `CancelCommand` Ereignis Handlers, der ausgeführt wird, wenn auf die Schaltfläche "Abbrechen" des bearbeiteten Produkts geklickt wird, und damit die Aufgabe, den DataList in den Zustand der vorabbearbeitung zurückzukehren.

Damit das DataList all seine Elemente im schreibgeschützten Modus Renderer, müssen folgende Schritte ausgeführt werden:

1. Legen Sie die DataList s [`EditItemIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index eines nicht vorhandenen `DataListItem` Indexes fest. `-1` ist eine sichere Wahl, da die `DataListItem` Indizes bei `0`beginnen.
2. Binden Sie die Daten erneut an den DataList. Da keine `DataListItem` `ItemIndex` es dem DataList-`EditItemIndex`entsprechen, wird der gesamte DataList in einem schreibgeschützten Modus gerendert.

Diese Schritte können mit folgendem Ereignishandlercode ausgeführt werden:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Durch Klicken auf die Schaltfläche Abbrechen wird der DataList in den Zustand der vorabbearbeitung zurückversetzt.

Der letzte Ereignishandler, den wir vervollständigen müssen, ist der `UpdateCommand` Ereignishandler. Dieser Ereignishandler muss folgende Aktionen ausführen:

1. Programm gesteuerter Zugriff auf den vom Benutzer eingegebenen Produktnamen und Preis sowie auf die `ProductID`bearbeiteten Produkte.
2. Initiieren Sie den Aktualisierungs Vorgang, indem Sie die entsprechende `UpdateProduct` Überladung in der `ProductsBLL`-Klasse aufrufen.
3. Legen Sie die DataList s [`EditItemIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index eines nicht vorhandenen `DataListItem` Indexes fest. `-1` ist eine sichere Wahl, da die `DataListItem` Indizes bei `0`beginnen.
4. Binden Sie die Daten erneut an den DataList. Da keine `DataListItem` `ItemIndex` es dem DataList-`EditItemIndex`entsprechen, wird der gesamte DataList in einem schreibgeschützten Modus gerendert.

Die Schritte 1 und 2 sind dafür verantwortlich, die Benutzer Änderungen zu speichern. die Schritte 3 und 4 geben den DataList in den Zustand der vorabbearbeitung zurück, nachdem die Änderungen gespeichert wurden, und sind mit den im `CancelCommand` Ereignishandler ausgeführten Schritten identisch.

Um den aktualisierten Produktnamen und Preis zu erhalten, müssen wir die `FindControl`-Methode verwenden, um Programm gesteuert auf die TextBox-websteuer Elemente innerhalb der `EditItemTemplate`zu verweisen. Wir müssen auch die bearbeiteten Produkte `ProductID` Wert erhalten. Bei der anfänglichen Bindung von ObjectDataSource an DataList hat Visual Studio dem Primärschlüssel Wert aus der Datenquelle (`ProductID`) die DataList s `DataKeyField`-Eigenschaft zugewiesen. Dieser Wert kann dann aus der DataList s-`DataKeys` Collection abgerufen werden. Nehmen Sie sich einen Moment Zeit, um sicherzustellen, dass die `DataKeyField` Eigenschaft tatsächlich auf `ProductID`festgelegt ist.

Der folgende Code implementiert die vier Schritte:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Der Ereignishandler beginnt mit dem Lesen der bearbeiteten Product s `ProductID` aus der `DataKeys` Auflistung. Im nächsten Schritt werden die beiden Textfelder in der `EditItemTemplate` referenziert, und ihre `Text` Eigenschaften werden in den lokalen Variablen `productNameValue` und `unitPriceValue`gespeichert. Verwenden Sie die `Decimal.Parse()`-Methode, um den Wert aus dem `UnitPrice` Textfeld zu lesen, sodass der Wert, wenn der eingegebene Wert ein Währungssymbol aufweist, trotzdem ordnungsgemäß in einen `Decimal` Wert konvertiert werden kann.

> [!NOTE]
> Die Werte aus den Textfeldern `ProductName` und `UnitPrice` werden nur den Variablen productnamevalue und unitpricevalue zugewiesen, wenn für die Textfelder Texteigenschaften ein Wert angegeben wurde. Andernfalls wird der Wert `Nothing` für die Variablen verwendet, wodurch die Daten mit einem Daten Bank `NULL` Wert aktualisiert werden. Das heißt, der Code behandelt leere Zeichen folgen in Daten Bank `NULL` Werte. Dies ist das Standardverhalten der Bearbeitungs Schnittstelle in den GridView-, DetailsView-und FormView-Steuerelementen.

Nachdem Sie die Werte gelesen haben, wird die `ProductsBLL` Class s `UpdateProduct`-Methode aufgerufen, und der Name, der Preis und die `ProductID`des Produkts werden übergeben. Der Ereignishandler wird beendet, indem der DataList mit exakt derselben Logik wie im `CancelCommand` Ereignishandler in den Zustand der vorabbearbeitung zurückgegeben wird.

Mit den `EditCommand`-, `CancelCommand`-und `UpdateCommand` Ereignis Handlern kann ein Besucher den Namen und den Preis eines Produkts bearbeiten. In Abbildung 14-16 wird dieser Bearbeitungs Workflow in Aktion angezeigt.

[![beim ersten Besuch der Seite befinden sich alle Produkte im schreibgeschützten Modus.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Abbildung 14**: beim ersten Besuch der Seite befinden sich alle Produkte im schreibgeschützten Modus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))

[Klicken Sie auf die Schaltfläche "Bearbeiten", ![Sie den Namen oder den Preis eines Produkts aktualisieren](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Abbildung 15**: Klicken Sie auf die Schaltfläche "Bearbeiten" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png)), um den Namen oder Preis eines Produkts zu aktualisieren

[Nachdem Sie den Wert geändert haben, klicken Sie auf aktualisieren, um zum schreibgeschützten Modus zurückzukehren. ![](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Abbildung 16**: Nachdem Sie den Wert geändert haben, klicken Sie auf aktualisieren, um zum schreibgeschützten Modus zurückzukehren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png)).

## <a name="step-7-adding-delete-capabilities"></a>Schritt 7: Hinzufügen von Löschfunktionen

Die Schritte zum Hinzufügen von Löschfunktionen zu einem DataList ähneln denen zum Hinzufügen von Bearbeitungsfunktionen. Kurz gesagt, wir müssen dem `ItemTemplate` eine Lösch Schaltfläche hinzufügen, die beim Klicken auf Folgendes angezeigt wird:

1. Liest die entsprechenden Product s `ProductID` über die `DataKeys` Auflistung.
2. Führt den Löschvorgang durch Aufrufen der `ProductsBLL` Class s `DeleteProduct`-Methode aus.
3. Bindet die Daten erneut an den DataList.

Beginnen Sie, indem Sie dem `ItemTemplate`eine Lösch Schaltfläche hinzufügen.

Beim Klicken auf eine Schaltfläche, deren `CommandName` bearbeiten, aktualisieren oder Abbrechen ist, wird das DataList s `ItemCommand`-Ereignis zusammen mit einem zusätzlichen Ereignis ausgelöst (z. b. bei Verwendung von Edit wird auch das `EditCommand` Ereignis ausgelöst). Entsprechend bewirkt jede Schaltfläche, jede Schaltfläche oder jedes ImageButton in der DataList, deren `CommandName`-Eigenschaft auf Delete festgelegt ist, das `DeleteCommand` Ereignis (zusammen mit `ItemCommand`).

Fügen Sie eine Lösch Schaltfläche neben der Bearbeitungs Schaltfläche im `ItemTemplate`hinzu, und legen Sie dessen `CommandName`-Eigenschaft auf Löschen fest. Nachdem Sie dieses Schaltflächen Steuerelement hinzugefügt haben, sollte die deklarative Syntax des DataList s `ItemTemplate` aussehen:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das DataList s `DeleteCommand`-Ereignis, indem Sie den folgenden Code verwenden:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Wenn Sie auf die Schaltfläche "Löschen" klicken, wird ein Postback ausgelöst und das Ereignis "DataList s `DeleteCommand` Im-Ereignishandler wird auf den `ProductID` Wert, auf den geklickt wurde, von der `DataKeys` Auflistung aus zugegriffen. Im nächsten Schritt wird das Produkt durch Aufrufen der `DeleteProduct` Methode der `ProductsBLL`-Klasse gelöscht.

Nach dem Löschen des Produkts ist es wichtig, dass die Daten erneut an den DataList (`DataList1.DataBind()`) gebunden werden, andernfalls zeigt der DataList weiterhin das Produkt an, das soeben gelöscht wurde.

## <a name="summary"></a>Zusammenfassung

Obwohl das DataList-Steuerelemente nicht den Punkt hat und auf Bearbeitung und Löschung der Unterstützung durch das GridView-Steuerelemente klickt, kann es mit einem kurzen Code verbessert werden, um diese Funktionen einzuschließen. In diesem Tutorial haben Sie erfahren, wie Sie eine zweispaltige Auflistung von Produkten erstellen, die gelöscht werden können und deren Name und Preis bearbeitet werden können. Das Hinzufügen von Unterstützung für Bearbeitung und Löschung besteht darin, die entsprechenden websteuer Elemente in den `ItemTemplate` und `EditItemTemplate`einzuschließen, die entsprechenden Ereignishandler zu erstellen, die vom Benutzer eingegebenen und die Primärschlüssel Werte zu lesen und mit der Geschäftslogik Schicht zu interagieren.

Obwohl wir dem DataList grundlegende Bearbeitungs-und Löschfunktionen hinzugefügt haben, fehlen ihm erweiterte Funktionen. Beispielsweise gibt es keine Überprüfung des Eingabe Felds. Wenn ein Benutzer einen Preis zu teuer eingibt, wird eine Ausnahme von `Decimal.Parse` ausgelöst, wenn versucht wird, zu teuer in eine `Decimal`zu konvertieren. Ebenso gilt: Wenn beim Aktualisieren der Daten in der Geschäftslogik oder bei Datenzugriffsebenen ein Problem auftritt, wird dem Benutzer der Standardfehler Bildschirm angezeigt. Ohne eine Bestätigung der Schaltfläche "Löschen" ist das versehentlich Löschen eines Produkts zu wahrscheinlich.

In zukünftigen Tutorials erfahren Sie, wie Sie die Benutzeroberflächen Bearbeitung verbessern.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial waren Zack Jones, Ken pespisa und Randy Schmidt. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Weiter](performing-batch-updates-cs.md)
