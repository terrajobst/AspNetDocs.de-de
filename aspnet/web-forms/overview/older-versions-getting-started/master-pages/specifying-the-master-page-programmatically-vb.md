---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Programm gesteuertes angeben der Master Seite (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Prüft, wie die Master Seite der Inhaltsseite Programm gesteuert über den PreInit-Ereignishandler festgelegt wird.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b039b22bef38ae6ebf80be070820dc1638f87f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457305"
---
# <a name="specifying-the-master-page-programmatically-vb"></a>Programmgesteuertes Festlegen der Masterseite (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Prüft, wie die Master Seite der Inhaltsseite Programm gesteuert über den PreInit-Ereignishandler festgelegt wird.

## <a name="introduction"></a>Einführung

Seit dem ersten Beispiel zum [*Erstellen eines Website weiten Layouts mit Master Seiten*](creating-a-site-wide-layout-using-master-pages-vb.md)haben alle Inhaltsseiten deklarativ über das `MasterPageFile`-Attribut in der `@Page`-Direktive auf Ihre Master Seite verwiesen. Beispielsweise wird die Inhaltsseite mit der folgenden `@Page`-Direktive mit der Master Seite `Site.master`verknüpft:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

Die [`Page`-Klasse](https://msdn.microsoft.com/library/system.web.ui.page.aspx) im `System.Web.UI`-Namespace enthält eine [`MasterPageFile`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) , die den Pfad zur Master Seite der Inhaltsseite zurückgibt. Es handelt sich um diese Eigenschaft, die von der `@Page`-Direktive festgelegt wird. Diese Eigenschaft kann auch verwendet werden, um die Master Seite der Inhaltsseite Programm gesteuert anzugeben. Diese Vorgehensweise ist nützlich, wenn Sie die Master Seite auf der Grundlage externer Faktoren dynamisch zuweisen möchten, z. b. durch den Benutzer, der die Seite besucht.

In diesem Tutorial fügen wir unserer Website eine zweite Master Seite hinzu und entscheiden dynamisch, welche Master Seite zur Laufzeit verwendet werden soll.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Schritt 1: sehen Sie sich den Seiten Lebenszyklus an

Wenn eine Anforderung auf dem Webserver für eine ASP.NET-Seite eingeht, bei der es sich um eine Inhaltsseite handelt, muss die ASP.net-Engine die Inhalts Steuerelemente der Seite in den entsprechenden contentplachalter-Steuerelementen der Master Seite vereinen. Diese Fusion erstellt eine einzelne Steuerelement Hierarchie, die dann den typischen Lebenszyklus der Seite durchlaufen kann.

In Abbildung 1 wird diese Fusion veranschaulicht. Schritt 1 in Abbildung 1 zeigt die anfänglichen Inhalts-und Masterseiten-Steuerelement Hierarchien. Am Ende der PreInit-Phase werden die Inhalts Steuerelemente auf der Seite den entsprechenden Content-Platzhaltern auf der Master Seite hinzugefügt (Schritt 2). Nach dieser Fusion fungiert die Master Seite als Stamm der Hierarchie mit der zugrunde zierten Steuerelement. Diese hierarchienhierarchie wird dann der Seite hinzugefügt, um die fertige Steuerelement Hierarchie zu schaffen (Schritt 3). Das Ergebnis ist, dass die Steuerelement Hierarchie der Seite die Hierarchie der geschmolzenen Steuerelemente enthält.

[![die Steuerungs Hierarchien der Master Seite und der Inhaltsseite während der PreInit-Phase miteinander verknüpft werden.](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Abbildung 01**: die Steuerungs Hierarchien der Master Seite und der Inhaltsseite werden während der PreInit-Phase miteinander verknüpft ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image3.png))

## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Schritt 2: Festlegen der`MasterPageFile`-Eigenschaft aus dem Code

Welche Master Seite in dieser Fusion ausgeführt wird, hängt vom Wert der `MasterPageFile`-Eigenschaft des `Page` Objekts ab. Das Festlegen des `MasterPageFile` Attributs in der `@Page` Direktive hat den Effekt, dass die `MasterPageFile` Eigenschaft des `Page`während der Initialisierungsphase zugewiesen wird. Dies ist die erste Phase des Lebenszyklus der Seite. Wir können diese Eigenschaft Alternativprogramm gesteuert festlegen. Es ist jedoch zwingend erforderlich, dass diese Eigenschaft festgelegt wird, bevor die Fusion in Abbildung 1 stattfindet.

Am Anfang der PreInit-Phase löst das `Page` Objekt das [`PreInit`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) aus und ruft seine [`OnPreInit`-Methode](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx)auf. Zum programmgesteuerten Festlegen der Master Seite können Sie entweder einen Ereignishandler für das `PreInit`-Ereignis erstellen oder die `OnPreInit`-Methode überschreiben. Sehen wir uns beide Ansätze an.

Öffnen Sie zunächst `Default.aspx.vb`, die Code Behind-Klassendatei für die Homepage unserer Website. Fügen Sie einen Ereignishandler für das `PreInit` Ereignis der Seite hinzu, indem Sie den folgenden Code eingeben:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Hier können Sie die `MasterPageFile`-Eigenschaft festlegen. Aktualisieren Sie den Code so, dass er der `MasterPageFile`-Eigenschaft den Wert "~/Site.Master" zuweist.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Wenn Sie einen Haltepunkt festlegen und mit dem Debuggen beginnen, sehen Sie, dass der `Page_PreInit`-Ereignishandler ausgeführt wird und die `MasterPageFile`-Eigenschaft "~/Site.Master" zugewiesen wird, wenn die `Default.aspx` Seite aufgerufen wird oder wenn ein Postback auf dieser Seite vorhanden ist.

Alternativ können Sie die `OnPreInit`-Methode der `Page` Klasse überschreiben und die `MasterPageFile`-Eigenschaft dort festlegen. In diesem Beispiel soll die Master Seite nicht auf einer bestimmten Seite, sondern nicht auf `BasePage`festgelegt werden. Erinnern Sie sich daran, dass wir eine benutzerdefinierte Basis Seiten Klasse (`BasePage`) zurück in erstellt haben, die [*den Titel, Meta-Tags und andere HTML-Header im Master Seiten-Tutorial angibt*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) . Derzeit überschreibt `BasePage` die `OnLoadComplete`-Methode der `Page` Klasse, wobei die `Title`-Eigenschaft der Seite auf der Grundlage der Site Übersichts Daten festgelegt wird. Aktualisieren Sie `BasePage`, um auch die `OnPreInit`-Methode zu überschreiben, um die Master Seite Programm gesteuert anzugeben.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Da alle Inhaltsseiten von `BasePage`abgeleitet sind, wird Ihre Master Seite nun von allen Inhalten Programm gesteuert zugewiesen. An diesem Punkt ist der `PreInit` Ereignishandler in `Default.aspx.vb` überflüssig. Sie können es einfach entfernen.

### <a name="what-about-thepagedirective"></a>Wie sieht es mit der`@Page`Direktive aus?

Was etwas verwirrend sein kann, ist, dass die `MasterPageFile` Eigenschaften der Inhaltsseiten nun an zwei Stellen angegeben werden: Programm gesteuert in der `OnPreInit`-Methode der `BasePage` Klasse sowie über das `MasterPageFile`-Attribut in den `@Page` Direktiven jeder Inhaltsseite.

Die erste Phase im Lebenszyklus der Seite ist die Initialisierungsphase. In dieser Phase wird der `MasterPageFile`-Eigenschaft des `Page` Objekts der Wert des `MasterPageFile`-Attributs in der `@Page` Direktive zugewiesen (sofern vorhanden). Die PreInit-Phase folgt der Initialisierungsphase, und hier wird die `MasterPageFile`-Eigenschaft des `Page` Objekts Programm gesteuert festgelegt, wodurch der Wert, der von der `@Page`-Direktive zugewiesen wird, überschrieben wird. Da die `MasterPageFile`-Eigenschaft des `Page` Objekts Programm gesteuert festgelegt wird, können wir das Attribut "`MasterPageFile`" aus der `@Page` Direktive entfernen, ohne dass die Benutzerumgebung beeinträchtigt wird. Um dies zu überzeugen, entfernen Sie das `MasterPageFile`-Attribut aus der `@Page`-Direktive in `Default.aspx` und besuchen dann die Seite über einen Browser. Wie Sie erwarten, ist die Ausgabe die gleiche wie vor dem Entfernen des Attributs.

Ob die `MasterPageFile`-Eigenschaft über die `@Page`-Direktive oder Programm gesteuert festgelegt wird, ist für die Benutzer Darstellung des Endbenutzers nicht von Belang. Das `MasterPageFile`-Attribut in der `@Page`-Direktive wird von Visual Studio jedoch während der Entwurfszeit verwendet, um die WYSIWYG-Ansicht im Designer zu erstellen. Wenn Sie in Visual Studio zu `Default.aspx` zurückkehren und zum Designer navigieren, wird die folgende Meldung angezeigt: "Fehler bei der Master Seite: die Seite verfügt über Steuerelemente, die einen Master Seiten Verweis erfordern, aber keine (siehe Abbildung 2).

Kurz gesagt, Sie müssen das `MasterPageFile`-Attribut in der `@Page`-Direktive belassen, um eine umfangreiche Entwurfszeit Funktion in Visual Studio zu nutzen.

[![Visual Studio das MasterPageFile-Attribut der @Page Direktive zum Rendering der Entwurfs Ansicht verwendet.](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Abbildung 02**: Visual Studio verwendet das `MasterPageFile` Attribute der `@Page` Direktive, um die Entwurfs Ansicht zu erzeugen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image6.png))

## <a name="step-3-creating-an-alternative-master-page"></a>Schritt 3: Erstellen einer alternativen Master Seite

Da die Master Seite einer Inhaltsseite Programm gesteuert zur Laufzeit festgelegt werden kann, ist es möglich, eine bestimmte Master Seite auf der Grundlage externer Kriterien dynamisch zu laden. Diese Funktionalität kann in Situationen nützlich sein, in denen das Layout der Website basierend auf dem Benutzer variieren muss. Beispielsweise kann eine Blog-Engine-Webanwendung es Benutzern ermöglichen, ein Layout für Ihren Blog auszuwählen, wobei jedes Layout einer anderen Master Seite zugeordnet ist. Wenn ein Besucher zur Laufzeit den Blog eines Benutzers anzeigen möchte, muss die Webanwendung das Layout des Blogs ermitteln und die entsprechende Master Seite dynamisch der Inhaltsseite zuordnen.

Im folgenden wird erläutert, wie Sie eine Master Seite auf der Grundlage externer Kriterien dynamisch laden können. Unsere Website enthält derzeit nur eine Master Seite (`Site.master`). Wir benötigen eine weitere Master Seite, um die Auswahl einer Master Seite zur Laufzeit zu veranschaulichen. Dieser Schritt konzentriert sich auf das Erstellen und Konfigurieren der neuen Master Seite. Schritt 4 prüft, welche Master Seite zur Laufzeit verwendet werden soll.

Erstellen Sie eine neue Master Seite im Stamm Ordner mit dem Namen `Alternate.master`. Fügen Sie auch ein neues Stylesheet zur Website namens `AlternateStyles.css`hinzu.

[![der Website eine weitere Master Seite und CSS-Datei hinzufügen](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Abbildung 03**: Hinzufügen einer weiteren Master Seite und CSS-Datei zur Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image9.png))

Ich habe die `Alternate.master` Master Seite so entworfen, dass der Titel am oberen Rand der Seite, zentriert und in einem Marine Hintergrund angezeigt wird. Ich habe die linke Spalte weggelassen und diesen Inhalt unterhalb des `MainContent` contentplachalter-Steuer Elements verschoben, das nun die gesamte Breite der Seite überspannt. Außerdem habe ich die Liste der ungeordneten Lektionen in die Liste der ungeordneten Lektionen übernommen und durch eine horizontale Liste oben `MainContent`ersetzt. Außerdem wurden die von der Master Seite verwendeten Schriftarten und Farben aktualisiert (und durch Erweiterung die Inhaltsseiten). Abbildung 4 zeigt `Default.aspx`, wenn die `Alternate.master` Master Seite verwendet wird.

> [!NOTE]
> ASP.net bietet die Möglichkeit, Designs *zu definieren.* Ein Design ist eine Auflistung von Bildern, CSS-Dateien und Stil bezogenen websteuer Element-Eigenschafts Einstellungen, die zur Laufzeit auf eine Seite angewendet werden können. Designs sind die Möglichkeit, wenn sich die Layouts Ihrer Website nur in den angezeigten Bildern und ihren CSS-Regeln unterscheiden. Wenn sich die Layouts erheblich unterscheiden, z. b. das Verwenden unterschiedlicher websteuer Elemente oder eines völlig anderen Layouts, müssen Sie separate Masterseiten verwenden. Weitere Informationen zu Designs finden Sie im Abschnitt weiter unten in diesem Tutorial.

[![unsere Inhaltsseiten nun ein neues Erscheinungsbild verwenden können](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Abbildung 04**: unsere Inhaltsseiten können nun ein neues Erscheinungsbild verwenden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image12.png))

Wenn das Markup der Master-und Inhaltsseiten miteinander verschmolzen ist, prüft die `MasterPage`-Klasse, ob jedes Inhalts Steuerelement auf der Seite Inhalt auf einen contentplachalter auf der Master Seite verweist. Eine-Ausnahme wird ausgelöst, wenn ein Inhalts Steuerelement gefunden wird, das auf einen nicht vorhandenen contentplac-Platzhalter verweist. Anders ausgedrückt: Es ist zwingend erforderlich, dass die Master Seite, die der Inhaltsseite zugewiesen wird, für jedes Inhalts Steuerelement auf der Inhaltsseite über einen contentplachalter verfügt.

Die `Site.master` Master Seite enthält vier contentplachalter-Steuerelemente:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Einige Inhaltsseiten in unserer Website enthalten nur ein oder zwei Inhalts Steuerelemente. andere enthalten ein Inhalts Steuerelement für jeden verfügbaren contentplatzhalter. Wenn unsere neue Master Seite (`Alternate.master`) ggf. den Inhaltsseiten zugewiesen ist, die Inhalts Steuerelemente für alle Content-Platzhalter in `Site.master` haben, ist es von entscheidender Bedeutung, dass `Alternate.master` auch dieselben contentplachalter-Steuerelemente wie `Site.master`enthält.

Um die `Alternate.master` Master Seite so zu sehen, dass Sie in etwa so aussieht wie meine (siehe Abbildung 4), definieren Sie zunächst die Stile der Master Seite im `AlternateStyles.css` Stylesheet. Fügen Sie die folgenden Regeln in `AlternateStyles.css`hinzu:

[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Fügen Sie als nächstes das folgende deklarative Markup zu `Alternate.master`hinzu. Wie Sie sehen können, enthält `Alternate.master` vier contentplachalter-Steuerelemente mit denselben `ID` Werten wie die contentplachalter-Steuerelemente in `Site.master`. Darüber hinaus enthält Sie ein ScriptManager-Steuerelement, das für die Seiten in unserer Website notwendig ist, die das ASP.NET AJAX-Framework verwenden.

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testen der neuen Master Seite

Um diese neue Master Seite zu testen, aktualisieren Sie die `OnPreInit`-Methode der `BasePage`-Klasse, sodass der `MasterPageFile`-Eigenschaft der Wert `"~/Alternate.maser"` zugewiesen ist, und besuchen Sie die Website. Jede Seite sollte ohne Fehler funktionieren, außer zwei: `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx`. Das Hinzufügen eines Produkts zu DetailsView in `~/Admin/AddProduct.aspx` führt zu einer `NullReferenceException` aus der Codezeile, die versucht, die `GridMessageText`-Eigenschaft der Master Seite festzulegen. Beim Besuch `~/Admin/Products.aspx` eine `InvalidCastException` beim Laden der Seite ausgelöst: "das Objekt vom Typ" ASP. Alternate\_Master "kann nicht in den Typ" ASP. Site\_Master "umgewandelt werden.

Diese Fehler treten auf, weil die `Site.master` Code Behind-Klasse öffentliche Ereignisse, Eigenschaften und Methoden enthält, die nicht in `Alternate.master`definiert sind. Der Markup Teil dieser beiden Seiten verfügt über eine `@MasterType`-Direktive, die auf die `Site.master` Master Seite verweist.

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Außerdem enthält der `ItemInserted` Ereignishandler der DetailsView in `~/Admin/AddProduct.aspx` Code, der die lose typisierte `Page.Master`-Eigenschaft in ein Objekt vom Typ `Site`umwandelt. Die `@MasterType`-Direktive (auf diese Weise verwendet), und die Umwandlung im `ItemInserted`-Ereignishandler verbindet die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten eng mit der `Site.master` Master Seite.

Um diese enge Kopplung zu unterbrechen, können wir `Site.master` und `Alternate.master` von einer gemeinsamen Basisklasse ableiten, die Definitionen für die öffentlichen Member enthält. Danach können wir die `@MasterType` Direktive aktualisieren, um auf diesen allgemeinen Basistyp zu verweisen.

### <a name="creating-a-custom-base-master-page-class"></a>Erstellen einer benutzerdefinierten Basis-Master Seiten Klasse

Fügen Sie dem `App_Code` Ordner mit dem Namen `BaseMasterPage.vb` eine neue Klassendatei hinzu, und lassen Sie Sie von `System.Web.UI.MasterPage`ableiten. Wir müssen die `RefreshRecentProductsGrid`-Methode und die `GridMessageText`-Eigenschaft in `BaseMasterPage`definieren, aber Sie können Sie nicht einfach aus `Site.master` verschieben, da diese Member mit websteuer Elementen arbeiten, die spezifisch für die `Site.master` Master Seite sind (die `RecentProducts` GridView und `GridMessage`-Bezeichnung).

Wir müssen `BaseMasterPage` so konfigurieren, dass diese Member hier definiert sind, aber tatsächlich von `BaseMasterPage`abgeleiteten Klassen (`Site.master` und `Alternate.master`) implementiert werden. Diese Vererbungs Vererbung ist möglich, indem die-Klasse als `MustInherit` und deren Member als `MustOverride`gekennzeichnet wird. Kurz gesagt, das Hinzufügen dieser Schlüsselwörter zur-Klasse und den beiden Membern gibt an, dass `BaseMasterPage` `RefreshRecentProductsGrid` und `GridMessageText`nicht implementiert hat, sondern dass die abgeleiteten Klassen dies tun.

Wir müssen auch das `PricesDoubled`-Ereignis in `BaseMasterPage` definieren und eine Möglichkeit von den abgeleiteten Klassen bereitstellen, um das Ereignis zu erhöhen. Das Muster, das in der .NET Framework verwendet wird, um dieses Verhalten zu vereinfachen, besteht darin, ein öffentliches Ereignis in der Basisklasse zu erstellen und eine geschützte, über schreibbare Methode namens `OnEventName`hinzuzufügen. Abgeleitete Klassen können dann diese Methode zum Ausführen des Ereignisses und zum Ausführen von Code direkt vor oder nach dem auslassen des Ereignisses überschreiben.

Aktualisieren Sie die `BaseMasterPage`-Klasse, sodass Sie den folgenden Code enthält:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Navigieren Sie als nächstes zur `Site.master` Code Behind-Klasse, und lassen Sie Sie von `BaseMasterPage`ableiten. Da `BaseMasterPage` Elemente enthält, die als gekennzeichnet sind `MustOverride` müssen wir diese Member hier in `Site.master`überschreiben. Fügen Sie das `Overrides`-Schlüsselwort den Methoden-und Eigenschafts Definitionen hinzu. Aktualisieren Sie außerdem den Code, der das `PricesDoubled`-Ereignis auslöst, im `Click`-Ereignishandler der `DoublePrice` Schaltfläche durch einen-Rückruf der `OnPricesDoubled`-Methode der Basisklasse.

Nach diesen Änderungen sollte die `Site.master` Code Behind-Klasse den folgenden Code enthalten:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Wir müssen auch die Code-Behind-Klasse von `Alternate.master`aktualisieren, um von `BaseMasterPage` abzuleiten und die beiden `MustOverride` Member zu überschreiben. Da `Alternate.master` jedoch keine GridView-Sicht enthält, die die neuesten Produkte auflistet, oder eine Bezeichnung, die nach dem Hinzufügen eines neuen Produkts zur Datenbank eine Meldung anzeigt, müssen diese Methoden nichts tun.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Verweisen auf die Basisklasse der Master Seite

Nachdem Sie die `BaseMasterPage`-Klasse abgeschlossen und die beiden Masterseiten erweitert haben, besteht der letzte Schritt darin, die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten so zu aktualisieren, dass Sie auf diesen gemeinsamen Typ verweisen. Ändern Sie zunächst die `@MasterType` Direktive auf beiden Seiten von:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

Nach:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Anstatt auf einen Dateipfad zu verweisen, verweist die `@MasterType`-Eigenschaft jetzt auf den Basistyp (`BaseMasterPage`). Folglich ist die stark typisierte `Master`-Eigenschaft, die in den Code-Behind-Klassen der beiden Seiten verwendet wird, jetzt vom Typ `BaseMasterPage` (anstelle des Typs `Site`). Mit dieser Änderung wird `~/Admin/Products.aspx`erneut ausgeführt. Bisher führte dies zu einem Umwandlungs Fehler, weil die Seite für die Verwendung der `Alternate.master` Master Seite konfiguriert ist, aber die `@MasterType` Direktive auf die `Site.master` Datei verwiesen hat. Die Seite wird jedoch ohne Fehler gerendert. Dies liegt daran, dass die `Alternate.master` Master Seite in ein Objekt vom Typ "`BaseMasterPage`" umgewandelt werden kann (da es erweitert wird).

In `~/Admin/AddProduct.aspx`muss eine kleine Änderung vorgenommen werden. Der `ItemInserted` Ereignishandler des DetailsView-Steuer Elements verwendet sowohl die stark typisierte `Master`-Eigenschaft als auch die lose typisierte `Page.Master`-Eigenschaft. Wir haben den stark typisierten Verweis korrigiert, als wir die `@MasterType` Direktive aktualisiert haben, aber wir müssen weiterhin den locker-typisierten Verweis aktualisieren. Ersetzen Sie die folgende Codezeile:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Mit folgendem können `Page.Master` in den Basistyp umgewandelt werden:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Schritt 4: bestimmen, welche Master Seite an die Inhaltsseiten gebunden werden soll

Unsere `BasePage`-Klasse legt derzeit alle `MasterPageFile` Eigenschaften der Inhaltsseiten auf einen hart codierten Wert in der PreInit-Phase des Lebenszyklus der Seite fest. Wir können diesen Code aktualisieren, um die Master Seite auf einem externen Faktor zu basieren. Möglicherweise hängt die zu ladende Master Seite von den Einstellungen des aktuell angemeldeten Benutzers ab. In diesem Fall müssten wir Code in der `OnPreInit`-Methode in `BasePage` schreiben, der die Masterseiten Einstellungen des aktuellen Benutzers nach sucht.

Erstellen Sie eine Webseite, die es dem Benutzer ermöglicht, die zu verwendende Master Seite auszuwählen-`Site.master` oder `Alternate.master`, und speichern Sie diese Auswahl in einer Sitzungsvariablen. Beginnen Sie mit dem Erstellen einer neuen Webseite im Stammverzeichnis mit dem Namen `ChooseMasterPage.aspx`. Wenn Sie diese Seite (oder andere Inhaltsseiten) erstellen, müssen Sie Sie nicht an eine Master Seite binden, da die Master Seite Programm gesteuert in `BasePage`festgelegt wird. Wenn Sie die neue Seite jedoch nicht an eine Master Seite binden, enthält das standardmäßige deklarative Markup der Seite ein Webformular und andere von der Master Seite bereitgestellte Inhalte. Sie müssen dieses Markup manuell durch die entsprechenden Inhalts Steuerelemente ersetzen. Aus diesem Grund ist es einfacher, die neue ASP.NET-Seite an eine Master Seite zu binden.

> [!NOTE]
> Da `Site.master` und `Alternate.master` denselben Satz von contentplachalter-Steuerelementen aufweisen, spielt es keine Rolle, welche Master Seite Sie beim Erstellen der neuen Inhaltsseite auswählen. Aus Gründen der Konsistenz empfiehlt es sich, `Site.master`zu verwenden.

[![der Website eine neue Inhaltsseite hinzufügen](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Abbildung 05**: Hinzufügen einer neuen Inhaltsseite zur Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image15.png))

Aktualisieren Sie die `Web.sitemap` Datei, sodass Sie einen Eintrag für diese Lektion enthält. Fügen Sie unterhalb der `<siteMapNode>` für die Master Pages-und ASP.NET AJAX-Lektion das folgende Markup hinzu:

[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Bevor Sie der `ChooseMasterPage.aspx` Seite Inhalte hinzufügen, nehmen Sie sich einen Moment Zeit, um die Code Behind-Klasse der Seite zu aktualisieren, sodass Sie von `BasePage` (anstatt von `System.Web.UI.Page`) abgeleitet wird. Fügen Sie als nächstes der Seite ein Dropdown List-Steuerelement hinzu, legen Sie dessen `ID`-Eigenschaft auf `MasterPageChoice`fest, und fügen Sie zwei ListItems mit den `Text`-Werten "~/Site.Master" und "~/Alternate.Master" hinzu.

Fügen Sie der Seite ein Schaltflächen-websteuer Element hinzu, und legen Sie dessen Eigenschaften für `ID` und `Text` auf `SaveLayout` bzw. "Layout-Auswahl speichern" fest. An diesem Punkt sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Beim ersten Besuch der Seite muss die aktuell ausgewählte Masterseiten Auswahl des Benutzers angezeigt werden. Erstellen Sie einen `Page_Load`-Ereignishandler, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Der obige Code wird nur auf dem ersten Seitenbesuch ausgeführt (und nicht auf nachfolgenden Postbacks). Zuerst wird überprüft, ob die Sitzungs Variable `MyMasterPage` vorhanden ist. Wenn dies der Fall ist, wird versucht, das passende ListItem-Element in der Dropdown Liste `MasterPageChoice` zu finden. Wenn eine übereinstimmende ListItem gefunden wird, wird die `Selected`-Eigenschaft auf `True`festgelegt.

Wir benötigen auch Code, der die Auswahl des Benutzers in der `MyMasterPage` Sitzungsvariablen speichert. Erstellen Sie einen Ereignishandler für das `Click` Ereignis der `SaveLayout` Schaltfläche, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Wenn der `Click`-Ereignishandler beim Postback ausgeführt wird, wurde die Master Seite bereits ausgewählt. Daher ist die Auswahl der Dropdown Liste des Benutzers bis zum nächsten Besuch der Seite nicht wirksam. Der `Response.Redirect` zwingt den Browser, `ChooseMasterPage.aspx`erneut anzufordern.

Wenn die `ChooseMasterPage.aspx` Seite abgeschlossen ist, besteht die letzte Aufgabe darin, `BasePage` die `MasterPageFile` Eigenschaft basierend auf dem Wert der `MyMasterPage` Sitzungsvariablen zuzuweisen. Wenn die Sitzungs Variable nicht festgelegt ist, wird `BasePage` standardmäßig `Site.master`.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Ich habe den Code verschoben, der die `MasterPageFile`-Eigenschaft des `Page` Objekts aus dem `OnPreInit` Ereignishandler und zwei separaten Methoden zuweist. Diese erste Methode, `SetMasterPageFile`, weist dem Wert, der von der zweiten Methode `GetMasterPageFileFromSession`zurückgegeben wird, die `MasterPageFile`-Eigenschaft zu. Ich habe die `SetMasterPageFile` Methode `Overridable` so gekennzeichnet, dass zukünftige Klassen, die `BasePage` erweitern, Sie optional überschreiben können, um benutzerdefinierte Logik zu implementieren, falls erforderlich. Im nächsten Tutorial wird ein Beispiel für das Überschreiben `BasePage``SetMasterPageFile`-Eigenschaft angezeigt.

Wenn dieser Code vorhanden ist, besuchen Sie die Seite `ChooseMasterPage.aspx`. Zunächst wird die Seite `Site.master` Master ausgewählt (siehe Abbildung 6), aber der Benutzer kann eine andere Master Seite aus der Dropdown Liste auswählen.

[![Inhaltsseiten werden mithilfe der Master Seite "Site. Master" angezeigt.](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Abbildung 06**: Inhaltsseiten werden mithilfe der `Site.master` Master Seite angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image18.png))

[![Inhaltsseiten werden jetzt auf der Seite "Alternative Master Master" angezeigt.](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Abbildung 07**: Inhaltsseiten werden nun über die Seite "`Alternate.master` Master" angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-master-page-programmatically-vb/_static/image21.png))

## <a name="summary"></a>Zusammenfassung

Wenn eine Inhaltsseite besucht wird, werden die Inhalts Steuerelemente mit den contentplachalter-Steuerelementen der Master Seite miteinander verschmolzen. Die Master Seite der Inhaltsseite wird durch die `MasterPageFile`-Eigenschaft der `Page` Klasse bezeichnet, die während der Initialisierungsphase dem `MasterPageFile`-Attribut der `@Page`-Direktive zugewiesen wird. Wie in diesem Tutorial gezeigt, können Sie der `MasterPageFile`-Eigenschaft einen Wert zuweisen, sofern dies vor dem Ende der PreInit-Phase geschieht. Wenn die Master Seite Programm gesteuert angegeben werden kann, wird die Tür für erweiterte Szenarien geöffnet, z. b. das dynamische Binden einer Inhaltsseite an eine Master Seite auf der Grundlage externer Faktoren.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.NET Seiten Lebenszyklus-Diagramm](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Übersicht über ASP.net page Lifecycle](https://msdn.microsoft.com/library/ms178472.aspx)
- [Übersicht über ASP.NET Designs und Skins](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Master Seiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [Designs in ASP.net](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Suchi Banerjee. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-pages-and-asp-net-ajax-vb.md)
> [Weiter](nested-master-pages-vb.md)
