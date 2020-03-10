---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Grundlegendes zu ASP.NET AJAX-UpdatePanel-Triggern | Microsoft-Dokumentation
author: scottcate
description: Wenn Sie im Markup-Editor in Visual Studio arbeiten, bemerken Sie möglicherweise (von IntelliSense), dass zwei untergeordnete Elemente eines Update Panel-Steuer Elements vorhanden sind. Eins von wh...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: b1cc869f373d4f8283b4d92af74707c3f11fef61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440541"
---
# <a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Grundlegendes zu UpdatePanel-Triggern in ASP.NET AJAX

von [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Wenn Sie im Markup-Editor in Visual Studio arbeiten, bemerken Sie möglicherweise (von IntelliSense), dass zwei untergeordnete Elemente eines Update Panel-Steuer Elements vorhanden sind. Eines davon ist das Triggers-Element, das die Steuerelemente auf der Seite angibt (oder das Benutzer Steuerelement, wenn Sie eines verwenden), das ein partielles Rendering des Update Panel-Steuer Elements auslöst, in dem sich das Element befindet.

## <a name="introduction"></a>Einführung

Die ASP.NET-Technologie von Microsoft sorgt für ein objektorientiertes und Ereignis gesteuertes Programmiermodell und vereinigt es mit den Vorteilen von kompiliertem Code. Das serverseitige Verarbeitungsmodell weist jedoch mehrere Nachteile in der Technologie auf, von denen viele durch die neuen Features in den Microsoft ASP.NET 3,5-AJAX-Erweiterungen adressiert werden können. Diese Erweiterungen ermöglichen viele neue Rich Client-Features, einschließlich partielles Rendering von Seiten, ohne dass eine vollständige Seiten Aktualisierung erforderlich ist, die Möglichkeit des Zugriffs auf Webdienste über ein Client Skript (einschließlich der ASP.net-Profilerstellungs-API) und eine umfangreiche Client seitige API entworfen, um viele der Steuerelement Schemas zu spiegeln, die im serverseitigen ASP.NET-Steuerelement Satz angezeigt werden.

Dieses Whitepaper untersucht die XML-Triggerfunktionen der ASP.NET AJAX-`UpdatePanel` Komponente. XML-Trigger sorgen für eine differenzierte Steuerung der Komponenten, die partielles Rendering für bestimmte Update Panel-Steuerelemente verursachen können.

Dieses Whitepaper basiert auf der Beta 2-Version von .NET Framework 3,5 und Visual Studio 2008. Die ASP.NET-AJAX-Erweiterungen, zuvor eine Add-on-Assembly, die auf ASP.NET 2,0 ausgerichtet ist, sind jetzt in die .NET Framework Basisklassen Bibliothek integriert. In diesem Whitepaper wird auch davon ausgegangen, dass Sie mit Visual Studio 2008, nicht mit Visual Web Developer Express arbeiten und Exemplarische Vorgehensweisen entsprechend der Benutzeroberfläche von Visual Studio bereitstellen (obwohl Code Auflistungen unabhängig von Entwicklungsumgebung).

## <a name="triggers"></a>*Trigger*

Trigger für ein bestimmtes Update Panel enthalten standardmäßig automatisch alle untergeordneten Steuerelemente, die ein Postback aufrufen, einschließlich (z. b.) Textfeld-Steuerelemente, deren `AutoPostBack`-Eigenschaft auf **true**festgelegt ist. Trigger können jedoch auch deklarativ mithilfe von Markup eingeschlossen werden. Dies erfolgt im Abschnitt `<triggers>` der Deklaration des UpdatePanel-Steuer Elements. Obwohl auf Trigger über die `Triggers` Auflistungs Eigenschaft zugegriffen werden kann, wird empfohlen, dass Sie alle partiellen rendertrigger zur Laufzeit registrieren (z. b. Wenn ein Steuerelement zur Entwurfszeit nicht verfügbar ist), indem Sie die `RegisterAsyncPostBackControl(Control)`-Methode des ScriptManager-Objekts für die Seite im `Page_Load` Ereignis verwenden. Denken Sie daran, dass die Seiten zustandslos sind. Sie sollten diese Steuerelemente daher bei jeder Erstellung erneut registrieren.

Die automatische Einfügung von untergeordneten Elementen kann ebenfalls deaktiviert werden (sodass untergeordnete Steuerelemente, die Postbacks erstellen, nicht automatisch partielle Renderfunktionen auslöst), indem Sie die `ChildrenAsTriggers`-Eigenschaft auf **false**festlegen Dies ermöglicht Ihnen die größte Flexibilität bei der Zuweisung, welche spezifischen Steuerelemente ein Seiten Rendering aufrufen können. es wird empfohlen, dass ein Entwickler auf ein Ereignis reagiert, anstatt Ereignisse zu behandeln, die möglicherweise auftreten.

Beachten Sie, dass beim Auslösen der Update Panel-Steuerelemente, wenn Update Mode auf **Conditional**festgelegt wird, wenn das untergeordnete Update Panel ausgelöst wird, das übergeordnete Element jedoch nicht ist, nur das untergeordnete Update Panel aktualisiert wird. Wenn jedoch das übergeordnete Update Panel aktualisiert wird, wird das untergeordnete Update Panel ebenfalls aktualisiert.

## <a name="the-lttriggersgt-element"></a>*Die &lt;&gt; Element Trigger*

Wenn Sie im Markup-Editor in Visual Studio arbeiten, bemerken Sie möglicherweise (von IntelliSense), dass zwei untergeordnete Elemente eines `UpdatePanel`-Steuer Elements vorhanden sind. Das am häufigsten gesehene Element ist das `<ContentTemplate>`-Element, das im Wesentlichen den Inhalt kapselt, der im Aktualisierungs Panel (dem Inhalt, für den wir partielles Rendering aktivieren) enthalten ist. Das andere Element ist das `<Triggers>`-Element, das die Steuerelemente auf der Seite angibt (oder das Benutzer Steuerelement, wenn Sie eines verwenden), das eine partielle Darstellung des Update Panel-Steuer Elements auslöst, in dem sich die &lt;Trigger&gt;-Element befindet.

Das `<Triggers>`-Element kann jede beliebige Anzahl von zwei untergeordneten Knoten enthalten: `<asp:AsyncPostBackTrigger>` und `<asp:PostBackTrigger>`. Beide akzeptieren beide Attribute, `ControlID` und `EventName`und können jedes beliebige Steuerelement innerhalb der aktuellen Kapselungs Einheit angeben (wenn sich z. b. das Update Panel-Steuerelement in einem Webbenutzer Steuerelement befindet), sollten Sie nicht versuchen, auf die Seite, auf der sich das Benutzer Steuerelement befindet, auf ein Steuerelement zu verweisen.

Das `<asp:AsyncPostBackTrigger>`-Element ist besonders nützlich, da es ein beliebiges Ereignis von einem Steuerelement, das als untergeordnetes *Element eines Update* Panel-Steuer Elements in der Kapselungs Einheit vorhanden ist, als untergeordnetes Element festlegen kann, nicht nur das Update Panel, unter dem dieser-Fehler ein untergeordnetes Element ist. Folglich kann jedes Steuerelement so erstellt werden, dass eine partielle Seiten Aktualisierung ausgelöst wird.

Ebenso kann das `<asp:PostBackTrigger>`-Element verwendet werden, um ein teilweises Seiten Rendering zu auslösen, aber eines, für das ein vollständiger Roundtrip zum Server erforderlich ist. Dieses Auslöserelement kann auch verwendet werden, um ein vollständiges Seiten Rendering zu erzwingen, wenn ein Steuerelement normalerweise ein partielles Seiten Rendering auslöst (z. b. Wenn ein `Button` Steuerelement im `<ContentTemplate>`-Element eines Update Panel-Steuer Elements vorhanden ist). Auch hier kann das Postback-Auslöserelement ein beliebiges Steuerelement angeben, das ein untergeordnetes Element eines Update Panel-Steuer Elements in der aktuellen Kapselungs Einheit ist.

## <a name="lttriggersgt-element-reference"></a>*&lt;Trigger&gt; Element Verweis*

*Markup Nachfolger:*

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;ASP: asyncpostbackauslöser&gt; | Gibt ein Steuerelement und ein Ereignis an, die eine Teil Seiten Aktualisierung für das Update Panel verursachen, das diesen auslöserverweis enthält. |
| &lt;ASP: postbackauslöser&gt; | Gibt ein Steuerelement und ein Ereignis an, die eine vollständige Seiten Aktualisierung bewirken (eine vollständige Seiten Aktualisierung). Dieses Tag kann verwendet werden, um eine vollständige Aktualisierung zu erzwingen, wenn ein Steuerelement andernfalls partielles Rendering auslöst. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Exemplarische Vorgehensweise: Kreuz Update Panel-Trigger*

1. Erstellen Sie eine neue ASP.NET-Seite mit einem ScriptManager-Objekt, das das partielle Rendering ermöglicht. Fügen Sie dieser Seite zwei Update Panel-Elemente hinzu: Fügen Sie in der ersten ein Label-Steuerelement (Label1) und zwei Schaltflächen-Steuerelemente ein (button1 und Button2). Button1 sollte zum Aktualisieren sowohl von als auch von Button2 zum Aktualisieren dieses Elements oder etwas in diesen Zeilen klicken. Fügen Sie im zweiten Update Panel nur ein Label-Steuerelement (Label2) ein, aber legen Sie dessen ForeColor-Eigenschaft auf einen anderen Wert als den Standardwert fest, um ihn zu unterscheiden.
2. Legen Sie die UpdateMode-Eigenschaft beider Update Panel-Tags auf **Conditional**fest.

**Codebeispiel 1: Markup für "default. aspx":** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Legen Sie im Click-Ereignishandler für Button1 Label1. Text und Label2. Text auf einen zeitabhängigen Wert fest (z. b. DateTime. now. ToLongTimeString ()). Legen Sie für den Click-Ereignishandler für Button2 nur Label1. Text auf den zeitabhängigen Wert fest.

**Codebeispiel 2: Code Behind (gekürzt) in Default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Drücken Sie F5, um das Projekt zu erstellen und auszuführen. Beachten Sie, dass beim Klicken auf beide Panels aktualisieren beide Bezeichnungen Text ändern. Wenn Sie jedoch auf dieses Panel aktualisieren klicken, wird nur Label1 aktualisiert.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))

## <a name="under-the-hood"></a>*Einblick in die Hintergründe*

Im Beispiel, das wir soeben erstellt haben, sehen wir uns an, was ASP.NET AJAX tut und wie unsere Panel übergreifenden Update Panel-Trigger funktionieren. Zu diesem Zweck arbeiten wir mit dem generierten Seiten Quell-HTML und der Mozilla Firefox-Erweiterung namens Firebug, damit wir die AJAX-Postbacks leicht untersuchen können. Wir verwenden auch das .net-reflektortool von Lutz Roeder. Beide Tools sind kostenlos online verfügbar und können über eine Internetsuche gefunden werden.

Eine Untersuchung des Quellcodes der Seite zeigt fast nichts von der normalen. die Update Panel-Steuerelemente werden als `<div>` Container gerendert, und die vom `<asp:ScriptManager>`bereitgestellte Skript Ressource wird angezeigt. Es gibt auch einige neue AJAX-spezifische Aufrufe der PageRequestManager, die für die AJAX-Client Skript Bibliothek intern sind. Schließlich werden die beiden Update Panel-Container angezeigt: eine mit den gerenderten `<input>` Schaltflächen, wobei die beiden `<asp:Label>`-Steuerelemente als `<span>` Container gerendert werden. (Wenn Sie die DOM-Struktur in Firebug untersuchen, werden Sie feststellen, dass die Bezeichnungen abgeblendet sind, um anzugeben, dass Sie keinen sichtbaren Inhalt erzeugen).

Klicken Sie auf die Schaltfläche dieses Panel aktualisieren, und beachten Sie, dass das oberste Update Panel mit der aktuellen Serverzeit aktualisiert wird. Wählen Sie in Firebug die Registerkarte Konsole aus, um die Anforderung zu überprüfen. Überprüfen Sie zuerst die Parameter für die Post-Anforderung:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))

Beachten Sie, dass das Update Panel dem serverseitigen AJAX-Code genau anzeigt, welche Steuerungsstruktur über den ScriptManager1-Parameter ausgelöst wurde: `Button1` des `UpdatePanel1` Steuer Elements. Klicken Sie jetzt auf die Schaltfläche beide Panels aktualisieren. Dann sehen wir, dass die Antwort eine durch Trennzeichen getrennte Reihe von Variablen ist, die in einer Zeichenfolge festgelegt sind. insbesondere sehen wir, dass das oberste Update Panel, `UpdatePanel1`, das gesamte HTML-Format hat, das an den Browser gesendet wird. Die AJAX-Client Skript Bibliothek ersetzt den ursprünglichen HTML-Inhalt von Update Panel durch die `.innerHTML`-Eigenschaft, sodass der geänderte Inhalt vom Server als HTML gesendet wird.

Klicken Sie nun auf die Schaltfläche beide Panels aktualisieren, und überprüfen Sie die Ergebnisse des Servers. Die Ergebnisse sind sehr ähnlich: beide Update Panel-Instanzen erhalten neue HTML-Daten vom Server. Wie beim vorherigen Rückruf wird der zusätzliche Seiten Status gesendet.

Wie Sie sehen können, kann die AJAX-Client Skript Bibliothek Formular Postbacks ohne zusätzlichen Code abfangen, da kein spezieller Code zum Ausführen eines AJAX-Postbacks verwendet wird. Server Steuerelemente verwenden automatisch JavaScript, damit Sie nicht automatisch das Formular senden ASP.NET fügt automatisch Code für die Formular Validierung und den Zustand ein, der in erster Linie durch die automatische Skript Ressourcen Einbindung, die PostBackOptions-Klasse, erreicht wird. und die ClientScriptManager-Klasse.

Stellen Sie sich beispielsweise ein CheckBox-Steuerelement vor. untersuchen Sie die Klassen disassemblierassembly in .net Stellen Sie zu diesem Zweck sicher, dass die System. Web-Assembly geöffnet ist, und navigieren Sie zur `System.Web.UI.WebControls.CheckBox`-Klasse, indem Sie die `RenderInputTag`-Methode öffnen. Suchen Sie nach einer bedingten Bedingung, die die `AutoPostBack`-Eigenschaft überprüft:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))

Wenn automatisches Postback für ein `CheckBox` Steuerelement aktiviert ist (über die AutoPostBack-Eigenschaft true), wird das resultierende `<input>` Tag daher mit einem Skript für die ASP.NET-Ereignis Behandlung im `onclick`-Attribut gerendert. Das Abfangen der Übermittlung des Formulars ermöglicht dann, dass ASP.NET AJAX in die Seite eingefügt werden kann, um alle möglichen wichtigen Änderungen zu vermeiden, die durch die Verwendung einer möglicherweise ungenauen Zeichen folgen Ersetzung auftreten können. Außerdem ermöglicht es *allen* benutzerdefinierten ASP.NET-Steuerelementen, die Leistungsfähigkeit von ASP.NET AJAX ohne zusätzlichen Code zu nutzen, um die Verwendung in einem Update Panel-Container zu unterstützen.

Die `<triggers>`-Funktionalität entspricht den Werten, die im PageRequestManager-Befehl \_UpdateControls initialisiert werden (Beachten Sie, dass die ASP.NET AJAX-Client Skript Bibliothek die Konvention verwendet, dass Methoden, Ereignisse und Feldnamen, die mit einem Unterstrich beginnen, als intern gekennzeichnet sind und nicht zur Verwendung außerhalb der Bibliothek selbst vorgesehen sind.) Damit können wir beobachten, welche Steuerelemente für AJAX-Postbacks vorgesehen sind.

Fügen wir der Seite z. b. zwei zusätzliche Steuerelemente hinzu, wobei ein Steuerelement nicht vollständig außerhalb der Update Panel-Elemente steht und ein Update Panel in einem Update Panel belassen wird. Wir fügen ein CheckBox-Steuerelement im oberen UpdatePanel hinzu und legen eine Dropdown List mit einer Reihe von Farben ab, die in der Liste definiert sind. Hier ist das neue Markup:

**Codebeispiel 3: neues Markup**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Und hier ist der neue Code Behind:

**Codebeispiel 4: Code Behind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Die Idee hinter dieser Seite besteht darin, dass in der Dropdown Liste eine von drei Farben zum Anzeigen der zweiten Bezeichnung ausgewählt wird, dass das Kontrollkästchen sowohl feststellt, ob es Fett ist, als auch, ob die Bezeichnungen das Datum und die Uhrzeit anzeigen. Das Kontrollkästchen sollte kein AJAX-Update verursachen, aber die Dropdown Liste sollte, auch wenn es nicht in einem Update Panel enthalten ist.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))

Wie bereits im obigen Screenshot ersichtlich ist, ist die aktuellste Schaltfläche, auf die geklickt werden soll, die Rechte Schaltfläche, mit der Sie den Bereich aktualisieren, der die höchste Uhrzeit unabhängig von der untersten Zeit aktualisiert hat. Das Datum wurde auch zwischen den Mausklicks gewechselt, da das Datum in der untersten Bezeichnung sichtbar ist. Schließlich ist die Farbe der untersten Bezeichnung wichtig: Sie wurde vor kurzem aktualisiert als der Text der Bezeichnung, der zeigt, dass der Steuerelement Zustand wichtig ist, und Benutzer erwarten, dass Sie durch AJAX-Postbacks beibehalten werden. Die Zeit wurde *jedoch*nicht aktualisiert. Die Zeit wurde automatisch durch die Persistenz des \_\_ViewState-Felds der Seite aufgefüllt, die von der ASP.NET-Laufzeit interpretiert wird, als das Steuerelement auf dem Server erneut gerendert wurde. Der ASP.NET AJAX-Servercode erkennt nicht, in welchen Methoden die Steuerelemente den Zustand ändern; Er füllt einfach vom Ansichts Zustand erneut auf und führt dann die entsprechenden Ereignisse aus.

Es sollte jedoch darauf hingewiesen werden, dass die Zeit in der Seite\_Lade Ereignisses initialisiert wurde, die Zeit jedoch ordnungsgemäß erhöht worden wäre. Folglich sollten Entwickler darauf achten, dass der entsprechende Code bei den entsprechenden Ereignis Handlern ausgeführt wird, und die Verwendung von Seiten\_laden vermeiden, wenn ein Steuerelement-Ereignishandler geeignet wäre.

## <a name="summary"></a>Zusammenfassung

Das Update Panel-Steuerelement von ASP.NET AJAX Extensions ist vielseitiger und kann eine Reihe von Methoden zum Identifizieren von Steuerelement Ereignissen verwenden, die die Aktualisierung bewirken. Sie unterstützt das automatische Aktualisieren durch die untergeordneten Steuerelemente, kann aber auch auf Steuerungs Ereignisse an anderer Stelle auf der Seite reagieren.

Um das Potenzial der Server Verarbeitungs Last zu reduzieren, wird empfohlen, dass die `ChildrenAsTriggers`-Eigenschaft eines Update Panel-Objekts auf `false`festgelegt ist und dass Ereignisse in den standardmäßigen eingeschlossen werden. Dadurch wird auch verhindert, dass nicht benötigte Ereignisse potenziell unerwünschte Effekte, einschließlich Validierung und Änderungen an Eingabefeldern, auslösen. Diese Fehlertypen können schwer zu isolieren sein, da die Seite für den Benutzer transparent aktualisiert wird, und die Ursache ist möglicherweise nicht sofort ersichtlich.

Durch die Untersuchung der inneren Funktionsweise des ASP.NET AJAX-Formular postinterceptionmodells konnten wir feststellen, dass es das bereits von ASP.NET bereitgestellte Framework nutzt. Dabei behält Sie die maximale Kompatibilität mit Steuerelementen bei, die mit demselben Framework entworfen wurden, und greift minimal auf zusätzliches JavaScript hin, das für die Seite geschrieben wurde.

## <a name="bio"></a>Basierten

Rob pveza ist leitender .NET-Anwendungsentwickler bei terralever ([www.terralever.com](http://www.terralever.com)), einem führenden interaktiven Marketingunternehmen in Tempe, AZ. Er kann auf [robpaveza@gmail.com](mailto:robpaveza@gmail.com)erreicht werden, und sein Blog befindet sich unter [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate arbeitet seit 1997 mit Microsoft-Webtechnologien und ist der Präsident von myKB.com ([www.myKB.com](http://www.myKB.com)), wo er sich darauf spezialisiert hat, ASP.NET basierte Anwendungen zu schreiben, die sich auf die Software Lösungen der Wissensdatenbank konzentrieren. Scott kann über [scott.cate@myKB.com](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com) per e-Mail kontaktiert werden.

> [!div class="step-by-step"]
> [Zurück](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Weiter](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
