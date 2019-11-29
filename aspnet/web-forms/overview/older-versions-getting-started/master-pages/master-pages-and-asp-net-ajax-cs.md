---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Master Seiten und ASP.NET AJAX (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Erläutert Optionen für die Verwendung von ASP.NET AJAX und Masterseiten. Sucht mithilfe der ScriptManagerProxy-Klasse. erläutert, wie die verschiedenen JS-Dateien in dependi geladen werden...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cd1d57b4d2aa01654da53ab2b1cc01f71ad8a87
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74639687"
---
# <a name="master-pages-and-aspnet-ajax-c"></a>Masterseiten und ASP.NET AJAX (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Erläutert Optionen für die Verwendung von ASP.NET AJAX und Masterseiten. Sucht mithilfe der ScriptManagerProxy-Klasse. erläutert, wie die verschiedenen JS-Dateien geladen werden, abhängig davon, ob der ScriptManager auf der Master Seite oder der Inhaltsseite verwendet wird.

## <a name="introduction"></a>Einführung

In den letzten Jahren haben immer mehr Entwickler [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-fähige Webanwendungen entwickelt. Eine AJAX-fähige Website verwendet eine Reihe verwandter Webtechnologien, um eine reaktionsfähigere Benutzer Leistung zu bieten. Das Erstellen von AJAX-fähigen ASP.NET-Anwendungen ist dank des [ASP.NET AJAX-Frameworks](../../../../ajax/index.md)von Microsoft erstaunlich einfach. ASP.NET AJAX ist in ASP.NET 3,5 und Visual Studio 2008 integriert. Sie steht auch als separater Download für ASP.NET 2,0-Anwendungen zur Verfügung.

Wenn Sie AJAX-aktivierte Webseiten mit dem ASP.NET AJAX-Framework entwickeln, müssen Sie jeder Seite, die das Framework verwendet, genau ein [ScriptManager-Steuer](https://msdn.microsoft.com/library/bb398863.aspx) Element hinzufügen. Wie der Name schon sagt, verwaltet der ScriptManager das Client seitige Skript, das in AJAX-aktivierten Webseiten verwendet wird. Der ScriptManager gibt mindestens HTML-Code aus, der den Browser anweist, die JavaScript-Dateien, die die ASP.NET AJAX-Client Bibliothek erstellt haben, herunterzuladen. Sie kann auch verwendet werden, um benutzerdefinierte JavaScript-Dateien, Skript aktivierte Webdienste und benutzerdefinierte Funktionen für Anwendungsdienste zu registrieren.

Wenn Ihre Website Masterseiten verwendet (wie dies sollte), müssen Sie nicht unbedingt jeder einzelnen Inhaltsseite ein ScriptManager-Steuerelement hinzufügen. Stattdessen können Sie der Master Seite ein ScriptManager-Steuerelement hinzufügen. In diesem Tutorial wird gezeigt, wie Sie das ScriptManager-Steuerelement der Master Seite hinzufügen. Außerdem wird erläutert, wie das ScriptManagerProxy-Steuerelement verwendet wird, um benutzerdefinierte Skripts und Skript Dienste auf einer bestimmten Inhaltsseite zu registrieren.

> [!NOTE]
> In diesem Tutorial wird das Entwerfen und entwickeln von AJAX-fähigen Webanwendungen mit dem ASP.NET AJAX-Framework nicht erläutert. Weitere Informationen zur Verwendung von AJAX finden Sie in den [ASP.NET AJAX-Videos](../../../videos/aspnet-ajax/index.md) und- [Tutorials](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)sowie in den Ressourcen, die im Abschnitt Weitere Lesevorgänge am Ende dieses Tutorials aufgeführt sind.

## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Untersuchen des vom ScriptManager-Steuerelement ausgegebenen Markups

Das ScriptManager-Steuerelement gibt Markup aus, das den Browser anweist, die JavaScript-Dateien herunterzuladen, die die ASP.NET AJAX-Client Bibliothek zusammen mit der Außerdem wird der Seite, die diese Bibliothek initialisiert, ein Bit von Inline-JavaScript hinzugefügt. Das folgende Markup zeigt den Inhalt, der der gerenderten Ausgabe einer Seite hinzugefügt wird, die ein ScriptManager-Steuerelement enthält:

[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Der `<script src="url"></script>`-Tags weist den Browser an, die JavaScript-Datei unter *URL*herunterzuladen und auszuführen. ScriptManager gibt drei solche Tags aus. eine verweist auf die Datei `WebResource.axd`, während die anderen beiden auf die Datei `ScriptResource.axd`verweisen. Diese Dateien sind nicht als Dateien auf Ihrer Website vorhanden. Wenn eine Anforderung für eine dieser Dateien beim Webserver eingeht, untersucht die ASP.net-Engine stattdessen die QueryString und gibt den entsprechenden JavaScript-Inhalt zurück. Das von diesen drei externen JavaScript-Dateien bereitgestellte Skript stellt die Client Bibliothek des ASP.NET AJAX-Frameworks dar. Die anderen `<script>` Tags, die vom ScriptManager ausgegeben werden, enthalten ein Inline Skript, mit dem diese Bibliothek initialisiert wird.

Die externen Skript Verweise und das Inline Skript, das von ScriptManager ausgegeben wird, sind für eine Seite, die das ASP.NET AJAX-Framework verwendet, unverzichtbar, sind aber nicht für Seiten erforderlich, die das Framework nicht verwenden. Daher kann es sinnvoll sein, den Seiten, die das ASP.NET AJAX-Framework verwenden, nur einen ScriptManager hinzuzufügen. Und das ist ausreichend, aber wenn Sie über viele Seiten verfügen, die das Framework verwenden, fügen Sie das ScriptManager-Steuerelement allen Seiten hinzu, eine sich wiederholende Aufgabe, um die geringste zu sagen. Alternativ können Sie der Master Seite einen ScriptManager hinzufügen, der das erforderliche Skript dann in alle Inhaltsseiten einfügt. Bei dieser Vorgehensweise ist es nicht erforderlich, eine ScriptManager-Datei zu einer neuen Seite hinzuzufügen, die das ASP.NET AJAX-Framework verwendet, da es bereits in der Master Seite enthalten ist. In Schritt 1 wird das Hinzufügen von ScriptManager zur Master Seite beschrieben.

> [!NOTE]
> Wenn Sie die AJAX-Funktionalität in die Benutzeroberfläche der Master Seite einschließen möchten, haben Sie keine Möglichkeit, den ScriptManager in die Master Seite einzubeziehen.

Ein Nachteil beim Hinzufügen von ScriptManager zur Master Seite besteht darin, dass das obige Skript auf *jeder* Seite ausgegeben wird, unabhängig davon, ob es erforderlich ist. Dies führt natürlich zu einer Verschwendung von Bandbreite für die Seiten, für die der ScriptManager (über die Master Seite) enthalten ist, aber keine Funktionen des ASP.NET AJAX-Frameworks verwenden. Aber genau wie viel Bandbreite ist verschwendet?

- Der tatsächliche Inhalt, der vom ScriptManager ausgegeben wird (siehe oben), ergibt etwas mehr als 1 KB.
- Die drei externen Skriptdateien, auf die vom `<script>`-Element verwiesen wird, enthalten jedoch ungefähr 450 KB an Daten, die nicht komprimiert sind. auf einer Website, die gzip-Komprimierung verwendet, kann diese Gesamtbandbreite nahezu 100 KB reduziert werden. Diese Skriptdateien werden jedoch für ein Jahr vom Browser zwischengespeichert. Dies bedeutet, dass Sie nur einmal heruntergeladen werden müssen und dann auf anderen Seiten auf der Website wieder verwendet werden können.

Im besten Fall beträgt die Gesamtkosten, wenn die Skriptdateien zwischengespeichert werden, 1 KB, was unerheblich ist. Im schlimmsten Fall ist dies jedoch der Fall, wenn die Skriptdateien noch nicht heruntergeladen wurden und der Webserver keine Komprimierungs Form verwendet. der Bandbreiten Treffer liegt bei etwa 450 KB. der Wert für eine Breitbandverbindung kann bis zu einer Minute für  Benutzer über Einwählmodems. Die gute Nachricht ist, dass die externen Skriptdateien nur selten auftreten, da die externen Skriptdateien vom Browser zwischengespeichert werden.

> [!NOTE]
> Wenn Sie das ScriptManager-Steuerelement weiterhin in der Master Seite platzieren möchten, sollten Sie sich das Web Form (das `<form runat="server">` Markup auf der Master Seite) ansehen. Jede ASP.NET-Seite, die das Post Back Modell verwendet, muss genau ein Web Form enthalten. Durch das Hinzufügen eines Webformulars werden zusätzliche Inhalte hinzugefügt: eine Reihe verborgener Formularfelder, das `<form>` Tag selbst und ggf. eine JavaScript-Funktion zum Initiieren eines Postbacks aus einem Skript. Dieses Markup ist für Seiten, die nicht zurückkehren, nicht erforderlich. Dieses überflüssige Markup könnte entfernt werden, indem Sie das Webformular aus der Master Seite entfernen und es manuell zu jeder Inhaltsseite hinzufügen, die eine benötigt. Allerdings wiegen die Vorteile, die das Webformular auf der Master Seite hat, die Nachteile davon ab, dass es zu bestimmten Inhaltsseiten unnötig hinzugefügt wurde.

## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Schritt 1: Hinzufügen eines ScriptManager-Steuer Elements zur Master Seite

Jede Webseite, die das ASP.NET AJAX-Framework verwendet, muss genau ein ScriptManager-Steuerelement enthalten. Aufgrund dieser Anforderung ist es in der Regel sinnvoll, ein einzelnes ScriptManager-Steuerelement auf der Master Seite zu platzieren, damit alle Inhaltsseiten das ScriptManager-Steuerelement automatisch enthalten. Außerdem muss der ScriptManager vor allen ASP.NET-AJAX-Server Steuerelementen, z. b. den Update Panel-und UpdateProgress-Steuerelementen, stehen. Daher ist es am besten, den ScriptManager vor allen contentplachalter-Steuerelementen innerhalb des Webformulars zu platzieren.

Öffnen Sie die `Site.master` Master Seite, und fügen Sie ein ScriptManager-Steuerelement auf der Seite innerhalb des Webformulars, jedoch vor dem `<div id="topContent">` Element hinzu (siehe Abbildung 1). Wenn Sie Visual Web Developer 2008 oder Visual Studio 2008 verwenden, befindet sich das ScriptManager-Steuerelement in der Toolbox auf der Registerkarte AJAX-Erweiterungen. Wenn Sie Visual Studio 2005 verwenden, müssen Sie zuerst das ASP.NET AJAX-Framework installieren und die Steuerelemente der Toolbox hinzufügen. Besuchen Sie das [ASP.NET AJAX-wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) , um das Framework für ASP.NET 2,0 zu erhalten.

Nachdem Sie den ScriptManager der Seite hinzugefügt haben, ändern Sie die `ID` von `ScriptManager1` in `MyManager`.

[![der Master Seite den ScriptManager hinzufügen](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen von ScriptManager zur Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image3.png))

## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Schritt 2: Verwenden des ASP.NET AJAX-Frameworks von einer Inhaltsseite

Wenn das ScriptManager-Steuerelement der Master Seite hinzugefügt wurde, können Sie einer beliebigen Inhaltsseite nun ASP.NET AJAX Framework-Funktionalität hinzufügen. Erstellen Sie eine neue ASP.NET-Seite, auf der ein zufällig ausgewähltes Produkt aus der Northwind-Datenbank angezeigt wird. Wir verwenden das Timer-Steuerelement des ASP.NET AJAX-Frameworks, um diese Anzeige alle 15 Sekunden zu aktualisieren und ein neues Produkt anzuzeigen.

Erstellen Sie zunächst im Stammverzeichnis eine neue Seite mit dem Namen `ShowRandomProduct.aspx`. Vergessen Sie nicht, diese neue Seite an die `Site.master` Master Seite zu binden.

[![der Website eine neue ASP.NET-Seite hinzufügen](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Abbildung 02**: Hinzufügen einer neuen ASP.NET-Seite zur Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image6.png))

Beachten Sie, dass im Tutorial zum [*angeben des Titels, der Meta-Tags und anderer HTML-Header in der Master Seite*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) eine benutzerdefinierte Basis Seiten Klasse mit dem Namen `BasePage` erstellt wurde, die den Titel der Seite generiert hat, wenn er nicht explizit festgelegt wurde. Wechseln Sie zur Code Behind-Klasse der `ShowRandomProduct.aspx` Seite, und lassen Sie Sie von `BasePage` (anstelle von `System.Web.UI.Page`) ableiten.

Aktualisieren Sie abschließend die `Web.sitemap` Datei, sodass Sie einen Eintrag für diese Lektion enthält. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Interaktions Lektion für die Master-zu-Inhalt-Seite hinzu:

[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

Das Hinzufügen dieses `<siteMapNode>` Elements wird in der Liste der Lektionen widergespiegelt (siehe Abbildung 5).

### <a name="displaying-a-randomly-selected-product"></a>Anzeigen eines zufällig ausgewählten Produkts

Kehren Sie zu `ShowRandomProduct.aspx`zurück. Ziehen Sie aus dem Designer ein Update Panel-Steuerelement aus der Toolbox in das `MainContent` Content-Steuerelement, und legen Sie dessen `ID`-Eigenschaft auf `ProductPanel`fest. UpdatePanel stellt einen Bereich auf dem Bildschirm dar, der asynchron durch ein partielles Seiten Postback aktualisiert werden kann.

Unsere erste Aufgabe besteht darin, Informationen über ein nach dem Zufallsprinzip ausgewähltes Produkt innerhalb von Update Panel anzuzeigen. Ziehen Sie zunächst ein DetailsView-Steuerelement in das Update Panel. Legen Sie die `ID`-Eigenschaft des DetailsView-Steuer Elements auf `ProductInfo` fest, und löschen Sie dessen Eigenschaften `Height` und `Width`. Erweitern Sie das Smarttag DetailsView, und wählen Sie aus der Dropdown Liste Datenquelle auswählen aus, dass die DetailsView an ein neues SqlDataSource-Steuerelement mit dem Namen `RandomProductDataSource`gebunden werden soll.

[![die DetailsView an ein neues SqlDataSource-Steuerelement binden.](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Abbildung 03**: Binden der DetailsView an ein neues SqlDataSource-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image9.png))

Konfigurieren Sie das SqlDataSource-Steuerelement für die Verbindung mit der Northwind-Datenbank über die `NorthwindConnectionString` (die im Tutorial [*Interaktion mit der Master Seite des Inhaltsseite*](interacting-with-the-content-page-from-the-master-page-cs.md) -Lernprogramm erstellt wurde). Wählen Sie beim Konfigurieren der SELECT-Anweisung eine benutzerdefinierte SQL-Anweisung aus, und geben Sie dann die folgende Abfrage ein:

[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

Das `TOP 1`-Schlüsselwort in der `SELECT`-Klausel gibt nur den ersten von der Abfrage zurückgegebenen Datensatz zurück. Die [`NEWID()`-Funktion](https://msdn.microsoft.com/library/ms190348.aspx) generiert einen neuen [Globally Unique Identifier Wert (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) und kann in einer `ORDER BY`-Klausel verwendet werden, um die Datensätze der Tabelle in zufälliger Reihenfolge zurückzugeben.

[![die SqlDataSource so konfigurieren, dass ein einzelner, zufällig ausgewählter Datensatz zurückgegeben wird.](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Abbildung 04**: Konfigurieren von SqlDataSource zum Zurückgeben eines einzelnen, zufällig ausgewählten Datensatzes ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image12.png))

Nachdem Sie den Assistenten abgeschlossen haben, erstellt Visual Studio ein BoundField-Element für die beiden Spalten, die von der obigen Abfrage zurückgegeben werden. An diesem Punkt sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

Abbildung 5 zeigt die `ShowRandomProduct.aspx` Seite, wenn Sie in einem Browser angezeigt wird. Klicken Sie auf die Schaltfläche Aktualisieren Ihres Browsers, um die Seite neu zu laden die `ProductName`-und `UnitPrice` Werte für einen neuen zufällig ausgewählten Datensatz sollten angezeigt werden.

[![der Name und der Preis eines zufälligen Produkts angezeigt werden.](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Abbildung 05**: der Name und der Preis eines zufälligen Produkts werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image15.png))

### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automatisches Anzeigen eines neuen Produkts alle 15 Sekunden

Das ASP.NET AJAX-Framework enthält ein Timer-Steuerelement, das ein Postback zu einem bestimmten Zeitpunkt ausführt. beim Postback wird das `Tick` Ereignis des Timers ausgelöst. Wenn das Timer-Steuerelement in einem Update Panel platziert wird, löst es ein partielles Seiten Postback aus, bei dem die Daten erneut an die DetailsView gebunden werden können, um ein neues zufällig ausgewähltes Produkt anzuzeigen.

Ziehen Sie hierzu einen Timer aus der Toolbox, und legen Sie ihn in der Update Panel-Datei ab. Ändern Sie den `ID` des Timers von `Timer1` in `ProductTimer` und dessen `Interval` Eigenschaft von 60000 in 15000. Die `Interval`-Eigenschaft gibt die Anzahl von Millisekunden zwischen Postbacks an; das Festlegen auf 15000 bewirkt, dass der Timer alle 15 Sekunden eine partielle Seiten Postback auslöst. An diesem Punkt sollte das deklarative Markup Ihres Timers in etwa wie folgt aussehen:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Erstellen Sie einen Ereignishandler für das `Tick` Ereignis des Timers. In diesem Ereignishandler müssen Sie die Daten erneut an die DetailsView binden, indem Sie die `DataBind`-Methode der DetailsView aufrufen. Dadurch wird die DetailsView angewiesen, die Daten erneut aus dem Datenquellen-Steuerelement abzurufen. Dadurch wird ein neuer zufällig ausgewählter Datensatz ausgewählt und angezeigt (wie beim erneuten Laden der Seite, indem Sie auf die Schaltfläche "Aktualisieren" klicken).

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Das ist schon alles! Besuchen Sie die Seite über einen Browser. Anfänglich werden die Informationen eines zufälligen Produkts angezeigt. Wenn Sie den Bildschirm geduldig ansehen, werden Sie feststellen, dass die vorhandene Anzeige nach 15 Sekunden durch die Informationen zu einem neuen Produkt magisch ersetzt wird.

Um besser zu sehen, was hier passiert, fügen wir dem Update Panel ein Label-Steuerelement hinzu, das die Uhrzeit anzeigt, zu der die Anzeige zuletzt aktualisiert wurde. Fügen Sie ein Label-websteuer Element innerhalb von Update Panel hinzu, legen Sie dessen `ID` auf `LastUpdateTime`fest, und löschen Sie dessen `Text`-Eigenschaft. Erstellen Sie als nächstes einen Ereignishandler für das `Load`-Ereignis von UpdatePanel, und zeigen Sie die aktuelle Uhrzeit in der Bezeichnung an. (Das `Load` Ereignis von UpdatePanel wird bei jedem vollständigen oder partiellen Postback der Seite ausgelöst.)

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Nachdem diese Änderung durchgeführt wurde, enthält die Seite die Zeit, zu der das aktuell angezeigte Produkt geladen wurde. Abbildung 6 zeigt die Seite beim ersten Besuch. In Abbildung 7 wird die Seite 15 Sekunden später angezeigt, nachdem das Timer-Steuerelement "ticked" und Update Panel aktualisiert wurde, um Informationen zu einem neuen Produkt anzuzeigen.

[![ein zufällig ausgewähltes Produkt beim Laden der Seite angezeigt wird](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Abbildung 06**: ein zufällig ausgewähltes Produkt wird beim Laden der Seite angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image18.png))

[![alle 15 Sekunden wird ein neues zufällig ausgewähltes Produkt angezeigt.](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Abbildung 07**: alle 15 Sekunden wird ein neues zufällig ausgewähltes Produkt angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image21.png))

## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Schritt 3: Verwenden des ScriptManagerProxy-Steuer Elements

Zusammen mit der Einbeziehung des erforderlichen Skripts für die ASP.NET AJAX Framework-Client Bibliothek kann der ScriptManager auch benutzerdefinierte JavaScript-Dateien, Verweise auf Skript aktivierte Webdienste sowie benutzerdefinierte Authentifizierungs-, Autorisierungs-und Profil Dienste registrieren. Diese Anpassungen sind normalerweise auf eine bestimmte Seite zugeschnitten. Wenn jedoch in ScriptManager auf der Master Seite auf die benutzerdefinierten Skriptdateien, Webdienst Verweise oder die Authentifizierungs-, Autorisierungs-oder Profil Dienste verwiesen wird, sind Sie auf *allen* Seiten der Website enthalten.

Verwenden Sie das ScriptManagerProxy-Steuerelement zum Hinzufügen von ScriptManager-bezogenen Anpassungen auf Seitenbasis. Sie können einen ScriptManagerProxy zu einer Inhaltsseite hinzufügen und dann die benutzerdefinierte JavaScript-Datei, den Webdienst Verweis oder die Authentifizierung, Autorisierung oder den Profil Dienst von ScriptManagerProxy; registrieren. Dies hat den Effekt, dass diese Dienste für die jeweilige Inhaltsseite registriert werden.

> [!NOTE]
> Eine ASP.NET-Seite darf nur ein ScriptManager-Steuerelement enthalten. Daher können Sie ein ScriptManager-Steuerelement nicht zu einer Inhaltsseite hinzufügen, wenn das ScriptManager-Steuerelement bereits auf der Master Seite definiert ist. Der einzige Zweck von ScriptManagerProxy besteht darin, Entwicklern die Möglichkeit zu geben, ScriptManager auf der Master Seite zu definieren, aber dennoch die Möglichkeit zu haben, ScriptManager-Anpassungen Seite für Seite hinzuzufügen.

Um das ScriptManagerProxy-Steuerelement in Aktion zu sehen, können Sie das Update Panel in `ShowRandomProduct.aspx` erweitern, um eine Schaltfläche einzufügen, die ein Client seitiges Skript verwendet, um das Timer-Steuerelement anzuhalten oder fortzusetzen. Das Timer-Steuerelement verfügt über drei Client seitige Methoden, die wir verwenden können, um diese gewünschte Funktionalität zu erreichen:

- `_startTimer()`: startet das Timer-Steuerelement.
- `_raiseTick()`: bewirkt, dass das Timer-Steuerelement "Tick" ist, wodurch zurück gepostet und das `Tick` Ereignis auf dem Server ausgelöst wird.
- `_stopTimer()`: stoppt das Timer-Steuerelement.

Erstellen wir eine JavaScript-Datei mit einer Variablen namens `timerEnabled` und einer Funktion mit dem Namen `ToggleTimer`. Die `timerEnabled` Variable gibt an, ob das Timer-Steuerelement derzeit aktiviert oder deaktiviert ist. der Standardwert ist "true". Die `ToggleTimer` Funktion akzeptiert zwei Eingabeparameter: einen Verweis auf die Schaltfläche Anhalten/Fortsetzen und den Client seitigen `id` Wert des Timer-Steuer Elements. Diese Funktion schaltet den Wert `timerEnabled`um, Ruft einen Verweis auf das Timer-Steuerelement ab, startet oder beendet den Timer (abhängig vom Wert `timerEnabled`) und aktualisiert den Anzeige Text der Schaltfläche in "anhalten" oder "fortsetzen". Diese Funktion wird immer dann aufgerufen, wenn auf die Schaltfläche Anhalten/Fortsetzen geklickt wird.

Erstellen Sie zunächst einen neuen Ordner auf der Website mit dem Namen `Scripts`. Fügen Sie als nächstes dem Ordner Scripts eine neue Datei mit dem Namen `TimerScript.js` vom Typ JScript File hinzu.

[![dem Ordner "Scripts" eine neue JavaScript-Datei hinzufügen](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Abbildung 08**: Hinzufügen einer neuen JavaScript-Datei zum `Scripts` Ordner ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image24.png))

[![eine neue JavaScript-Datei zur Website hinzugefügt wurde.](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Abbildung 09**: Es wurde eine neue JavaScript-Datei zur Website hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image27.png))

Fügen Sie dann der Datei "timerscript. js" den folgenden Skriptzugriff hinzu:

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Wir müssen nun diese benutzerdefinierte JavaScript-Datei in `ShowRandomProduct.aspx`registrieren. Kehren Sie zu `ShowRandomProduct.aspx` zurück, und fügen Sie der Seite ein ScriptManagerProxy-Steuerelement hinzu. Legen Sie den `ID` auf `MyManagerProxy`fest. Um eine benutzerdefinierte JavaScript-Datei zu registrieren, wählen Sie das ScriptManagerProxy-Steuerelement im Designer aus, und navigieren Sie dann zum Eigenschaftenfenster. Eine der Eigenschaften heißt Skripts. Wenn Sie diese Eigenschaft auswählen, wird der in Abbildung 10 dargestellte scriptreferenzierungseditor angezeigt. Klicken Sie auf die Schaltfläche hinzufügen, um einen neuen Skript Verweis einzuschließen, und geben Sie dann den Pfad zur Skriptdatei in der Path-Eigenschaft ein: `~/Scripts/TimerScript.js`.

[![Hinzufügen eines Skript Verweises zum ScriptManagerProxy-Steuerelement](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Abbildung 10**: Hinzufügen eines Skript Verweises zum ScriptManagerProxy-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image30.png))

Nach dem Hinzufügen des Skript Verweises wird das deklarative Markup des ScriptManagerProxy-Steuer Elements so aktualisiert, dass es eine `<Scripts>` Auflistung mit einem einzelnen `ScriptReference` Eintrag enthält, wie im folgenden Code Ausschnitt veranschaulicht:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

Der `ScriptReference` Eintrag weist ScriptManagerProxy an, einen Verweis auf die JavaScript-Datei im gerenderten Markup einzuschließen. Das heißt, indem das benutzerdefinierte Skript in ScriptManagerProxy registriert wird, enthält die gerenderte Ausgabe der `ShowRandomProduct.aspx` Seite nun ein weiteres `<script src="url"></script>` Tag: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Wir können nun die `ToggleTimer` Funktion, die in `TimerScript.js` definiert ist, aus dem Client Skript auf der Seite `ShowRandomProduct.aspx` abrufen. Fügen Sie in Update Panel den folgenden HTML-Code hinzu:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Dadurch wird eine Schaltfläche mit dem Text "Pause" angezeigt. Wenn Sie darauf klicken, wird die JavaScript-Funktion `ToggleTimer` aufgerufen, wobei ein Verweis auf die Schaltfläche und der ID-Wert des Timer-Steuer Elements (`ProductTimer`) übergeben werden. Beachten Sie die Syntax zum Abrufen des `id` Werts des Timer-Steuer Elements. `<%=ProductTimer.ClientID%>` den Wert der `ClientID`-Eigenschaft des `ProductTimer` Timer-Steuer Elements ausgibt. Im Tutorial "Benennen von Steuerelement- [*IDs in Inhaltsseiten*](control-id-naming-in-content-pages-cs.md) " wurden die Unterschiede zwischen dem serverseitigen `ID` Wert und dem resultierenden Client seitigen `id` Wert erläutert, und es wird erläutert, wie `ClientID` die Client seitige `id`zurückgibt.

Abbildung 11 zeigt diese Seite, wenn Sie zum ersten Mal über einen Browser besucht wird. Der Timer wird zurzeit ausgeführt und aktualisiert die angezeigten Produktinformationen alle 15 Sekunden. Abbildung 12 zeigt den Bildschirm nach dem Klicken auf die Schaltfläche Pause. Durch Klicken auf die Schaltfläche Anhalten wird der Timer angehalten und der Text der Schaltfläche in "Resume" aktualisiert. Die Produktinformationen werden aktualisiert (und alle 15 Sekunden aktualisiert), sobald der Benutzer auf "fortsetzen" klickt.

[![auf die Schaltfläche Anhalten, um das Timer-Steuerelement zu stoppen](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Abbildung 11**: Klicken auf die Schaltfläche Anhalten, um das Timer-Steuerelement anzuhalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-pages-and-asp-net-ajax-cs/_static/image33.png))

[Klicken Sie ![auf die Schaltfläche fortsetzen, um den Timer neu](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Abbildung 12**: Klicken Sie auf die Schaltfläche fortsetzen, um den Timer neu zu starten ([Klicken Sie, um das Bild in voller Größe](master-pages-and-asp-net-ajax-cs/_static/image36.png)

## <a name="summary"></a>Summary

Beim Entwickeln von AJAX-fähigen Webanwendungen mit dem ASP.NET AJAX-Framework ist es zwingend erforderlich, dass jede AJAX-aktivierte Webseite ein ScriptManager-Steuerelement enthält. Um diesen Prozess zu vereinfachen, können Sie der Master Seite einen ScriptManager hinzufügen, anstatt sich daran erinnern zu müssen, dass jeder und jeder Inhaltsseite ein ScriptManager hinzugefügt wird. In Schritt 1 wurde gezeigt, wie Sie den ScriptManager der Master Seite hinzufügen, während Schritt 2 die Implementierung von AJAX-Funktionen auf einer Inhaltsseite betrachtete.

Wenn Sie benutzerdefinierte Skripts, Verweise auf Skript aktivierte Webdienste oder angepasste Authentifizierungs-, Autorisierungs-oder Profil Dienste auf einer bestimmten Inhaltsseite hinzufügen müssen, fügen Sie der Inhaltsseite ein ScriptManagerProxy-Steuerelement hinzu, und konfigurieren Sie dann das Anpassungen. In Schritt 3 wurde die Verwendung von ScriptManagerProxy zum Registrieren einer benutzerdefinierten JavaScript-Datei auf einer bestimmten Inhaltsseite untersucht.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.NET AJAX-Framework](../../../../ajax/index.md)
- [ASP.NET AJAX-Tutorials](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX-Videos](../../../videos/aspnet-ajax/index.md)
- [Entwickeln interaktiver Benutzeroberflächen mit Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Verwenden von "netwid" zum Zufalls Sortieren von Datensätzen](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Verwenden des Timer-Steuer Elements](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Weiter](specifying-the-master-page-programmatically-cs.md)
