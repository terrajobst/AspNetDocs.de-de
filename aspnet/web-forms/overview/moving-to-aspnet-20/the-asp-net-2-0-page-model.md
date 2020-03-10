---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Das ASP.NET 2,0-Seiten Modell | Microsoft-Dokumentation
author: microsoft
description: In ASP.NET 1. x hatten Entwickler eine Auswahl zwischen einem Inline Code Modell und einem Code Behind-Code Modell. Code Behind kann entweder mithilfe von src attr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: bcb71b2b5a484e8756406867e08e8aa699a9024d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440709"
---
# <a name="the-aspnet-20-page-model"></a>Das ASP.NET 2,0-Seiten Modell

von [Microsoft](https://github.com/microsoft)

> In ASP.NET 1. x hatten Entwickler eine Auswahl zwischen einem Inline Code Modell und einem Code Behind-Code Modell. Code Behind kann entweder mithilfe des src-Attributs oder des Code Behind-Attributs der @Page-Direktive implementiert werden. In ASP.NET 2,0 haben Entwickler weiterhin die Wahl zwischen Inline Code und Code Behind, aber es wurden bedeutende Verbesserungen am Code Behind-Modell vorgenommen.

In ASP.NET 1. x hatten Entwickler eine Auswahl zwischen einem Inline Code Modell und einem Code Behind-Code Modell. Code Behind kann entweder mithilfe des src-Attributs oder des Code Behind-Attributs der @Page-Direktive implementiert werden. In ASP.NET 2,0 haben Entwickler weiterhin die Wahl zwischen Inline Code und Code Behind, aber es wurden bedeutende Verbesserungen am Code Behind-Modell vorgenommen.

## <a name="improvements-in-the-code-behind-model"></a>Verbesserungen im Code Behind-Modell

Um die Änderungen im Code Behind-Modell in ASP.NET 2,0 vollständig nachzuvollziehen, ist es am besten, das Modell schnell zu überprüfen, wie es in ASP.NET 1. x vorhanden war.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Das Code Behind-Modell in ASP.NET 1. x

In ASP.NET 1. x umfasste das Code Behind-Modell eine ASPX-Datei (Webform) und eine Code-Behind-Datei mit Programmiercode. Die beiden Dateien waren mithilfe der @Page-Direktive in der ASPX-Datei verbunden. Jedes Steuerelement auf der ASPX-Seite enthielt eine entsprechende Deklaration in der Code-Behind-Datei als Instanzvariable. Die Code-Behind-Datei enthielt auch Code für die Ereignis Bindung und den generierten Code, der für den Visual Studio-Designer erforderlich ist. Dieses Modell funktionierte recht gut, aber da jedes ASP.net-Element auf der ASPX-Seite entsprechenden Code in der Code Behind-Datei erforderte, gab es keine echte Trennung von Code und Inhalt. Wenn ein Designer z. b. ein neues Server Steuerelement zu einer ASPX-Datei außerhalb der Visual Studio-IDE hinzugefügt hat, würde die Anwendung aufgrund einer fehlenden Deklaration für dieses Steuerelement in der Code Behind-Datei unterbrechen.

## <a name="the-code-behind-model-in-aspnet-20"></a>Das Code Behind-Modell in ASP.NET 2,0

ASP.NET 2,0 verbessert dieses Modell erheblich. In ASP.NET 2,0 wird Code Behind mithilfe der neuen *partiellen Klassen* implementiert, die in ASP.NET 2,0 bereitgestellt werden. Die Code-Behind-Klasse in ASP.NET 2,0 ist als partielle Klasse definiert, die bedeutet, dass Sie nur einen Teil der Klassendefinition enthält. Der verbleibende Teil der Klassendefinition wird dynamisch von ASP.NET 2,0 mithilfe der ASPX-Seite zur Laufzeit oder bei vorkompilierter Website generiert. Der Link zwischen der Code-Behind-Datei und der ASPX-Seite wird weiterhin mithilfe der @ Page-Direktive festgelegt. Allerdings verwendet ASP.NET 2,0 anstelle eines Code Behind-oder src-Attributs nun das CodeFile-Attribut. Das erbt-Attribut wird auch verwendet, um den Klassennamen für die Seite anzugeben.

Eine typische @ Page-Direktive könnte wie folgt aussehen:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Eine typische Klassendefinition in einer ASP.NET 2,0-Code Behind-Datei könnte wie folgt aussehen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C#und Visual Basic sind die einzigen verwalteten Sprachen, die derzeit partielle Klassen unterstützen. Daher können Entwickler, die j# verwenden, das Code Behind-Modell nicht in ASP.NET 2,0 verwenden.

Das neue Modell erweitert das Code Behind-Modell, da Entwickler nun über Code Dateien verfügen, die nur den von Ihnen erstellten Code enthalten. Außerdem bietet es eine echte Trennung von Code und Inhalt, da keine Instanzen Variablen Deklarationen in der Code-Behind-Datei vorhanden sind.

> [!NOTE]
> Da die partielle Klasse für die ASPX-Seite die Ereignis Bindung findet, können Visual Basic Entwickler eine geringfügige Leistungssteigerung erkennen, indem Sie das Handles-Schlüsselwort in Code-Behind verwenden, um Ereignisse zu binden. C#hat kein entsprechendes Schlüsselwort.

## <a name="new--page-directive-attributes"></a>Attribute der neuen @ Page-Direktive

ASP.NET 2,0 fügt der @ Page-Direktive viele neue Attribute hinzu. Die folgenden Attribute sind neu in ASP.NET 2,0.

## <a name="async"></a>Async

Mit dem Async-Attribut können Sie die Seite so konfigurieren, dass Sie asynchron ausgeführt wird. Sie decken asynchrone Seiten später in diesem Modul ab.

## <a name="asynctimeout"></a>AsyncTimeout

Gibt das Timeout für asynchrone Seiten an. Der Standardwert ist 45 Sekunden.

## <a name="codefile"></a>CodeFile

Das CodeFile-Attribut ist der Ersatz für das Code Behind-Attribut in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Das CodeFileBaseClass-Attribut wird in Fällen verwendet, in denen mehrere Seiten von einer einzelnen Basisklasse abgeleitet werden sollen. Aufgrund der Implementierung von partiellen Klassen in ASP.net ohne dieses Attribut ist eine Basisklasse, die gemeinsame allgemeine Felder verwendet, um auf Steuerelemente zu verweisen, die auf einer ASPX-Seite deklariert sind, nicht ordnungsgemäß funktionsfähig, da das ASP. Nets-Kompilierungs Modul basierend auf den Steuerelementen auf der Seite automatisch neue Elemente Wenn Sie also eine allgemeine Basisklasse für zwei oder mehr Seiten in ASP.net wünschen, müssen Sie die Basisklasse im CodeFileBaseClass-Attribut angeben und dann jede Pages-Klasse von dieser Basisklasse ableiten. Das CodeFile-Attribut ist auch erforderlich, wenn dieses Attribut verwendet wird.

## <a name="compilationmode"></a>CompilationMode

Mit diesem Attribut können Sie die CompilationMode-Eigenschaft der ASPX-Seite festlegen. Die CompilationMode-Eigenschaft ist eine Enumeration, die die Werte **Always**, **Auto**und **Never**enthält. Der Standardwert ist **immer**. Durch die **Automatische** Einstellung wird verhindert, dass ASP.net die Seite nach Möglichkeit dynamisch kompiliert. Das Ausschließen von Seiten von dynamischer Kompilierung erhöht die Leistung. Wenn jedoch eine auszuschließende Seite den Code enthält, der kompiliert werden muss, wird beim Durchsuchen der Seite ein Fehler ausgelöst.

## <a name="enableeventvalidation"></a>EnableEventValidation

Dieses Attribut gibt an, ob Postback-und Rückruf Ereignisse überprüft werden. Wenn diese Option aktiviert ist, werden Argumente für Postback-oder Rückruf Ereignisse geprüft, um sicherzustellen, dass Sie von dem Server Steuerelement stammen, das Sie ursprünglich gerendert hat.

## <a name="enabletheming"></a>Aktivieren

Dieses Attribut gibt an, ob ASP.NET Designs auf einer Seite verwendet werden. Die Standardeinstellung ist **false**. ASP.net Themes werden in [Modul 10](profiles-themes-and-web-parts.md)behandelt.

## <a name="linepragmas"></a>LinePragmas

Dieses Attribut gibt an, ob Zeilenpragmas während der Kompilierung hinzugefügt werden sollen. Zeilenpragmas sind Optionen, die von Debuggern verwendet werden, um bestimmte Code Abschnitte zu markieren.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Dieses Attribut gibt an, ob JavaScript in die Seite eingefügt wird, um die Bild Lauf Position zwischen Postbacks aufrechtzuerhalten. Dieses Attribut ist standardmäßig auf " **false** " eingestellt.

Wenn dieses Attribut **true**ist, fügt ASP.net ein &lt;Skript&gt; Block für Postback hinzu, das wie folgt aussieht:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Beachten Sie, dass die src für diesen Skriptblock WebResource. axd ist. Diese Ressource ist kein physischer Pfad. Wenn dieses Skript angefordert wird, erstellt ASP.NET das Skript dynamisch.

### <a name="masterpagefile"></a>MasterPageFile

Dieses Attribut gibt die Masterseiten Datei für die aktuelle Seite an. Der Pfad kann relativ oder absolut sein. Master Seiten werden in [Modul 4](master-pages.md)behandelt.

## <a name="stylesheettheme"></a>StyleSheetTheme

Mit diesem Attribut können Sie Benutzeroberflächen-Darstellungs Eigenschaften überschreiben, die durch ein ASP.NET 2,0-Design definiert werden. Themen werden in [Modul 10](profiles-themes-and-web-parts.md)behandelt.

## <a name="theme"></a>Design

Gibt das Design für die Seite an. Wenn für das StyleSheetTheme-Attribut kein Wert angegeben wird, überschreibt das Design Attribut alle Stile, die auf die Steuerelemente auf der Seite angewendet werden.

## <a name="title"></a>Titel

Legt den Titel für die Seite fest. Der hier angegebene Wert wird im &lt;Titel&gt; Element der gerenderten Seite angezeigt.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Legt den Wert für die viewstateverschlüsseltionmode-Enumeration fest. Die verfügbaren Werte lauten **immer**, **Auto**und **nie**. Der Standardwert ist " **Auto**". Wenn dieses Attribut auf den Wert " **Auto**" festgelegt ist, wird "ViewState" verschlüsselt, wenn ein Steuerelement es durch Aufrufen der **registerrequirements sviewstateencryption** -Methode anfordert.

## <a name="setting-public-property-values-via-the--page-directive"></a>Festlegen von Werten für öffentliche Eigenschaften über die @ Page-Direktive

Eine weitere neue Funktion der @ Page-Direktive in ASP.NET 2,0 ist die Möglichkeit, den Anfangswert öffentlicher Eigenschaften einer Basisklasse festzulegen. Nehmen wir beispielsweise an, dass Sie in ihrer Basisklasse eine öffentliche Eigenschaft mit dem Namen " **sometext** " haben und d. h., dass Sie mit " **Hello** " initialisiert werden soll, wenn eine Seite geladen wird. Sie können dies erreichen, indem Sie einfach den Wert in der @ Page-Direktive wie folgt festlegen:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

Das **sometext** -Attribut der @ Page-Direktive legt den Anfangswert der SomeText-Eigenschaft in der Basisklasse auf *Hello!* fest. Das folgende Video ist eine exemplarische Vorgehensweise zum Festlegen des Anfangs Werts einer öffentlichen Eigenschaft in einer Basisklasse mithilfe der @ Page-Direktive.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Vollbildvideos öffnen](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Neue öffentliche Eigenschaften der Page-Klasse

Die folgenden öffentlichen Eigenschaften sind neu in ASP.NET 2,0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Gibt den Anwendungs relativen Pfad zur Seite oder zum Steuerelement zurück. Beispielsweise gibt die-Eigenschaft für eine Seite, die sich unter http://app/folder/page.aspxbefindet, ~/Folder/. zurück.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Gibt den relativen Pfad des virtuellen Verzeichnisses zur Seite oder zum Steuerelement zurück. Beispielsweise gibt die-Eigenschaft für eine Seite, die sich unter http://app/folder/page.aspxbefindet, ~/Folder/Page.aspx. zurück.

## <a name="asynctimeout"></a>AsyncTimeout

Ruft das für die asynchrone Seiten Behandlung verwendete Timeout ab oder legt es fest. (Asynchrone Seiten werden später in diesem Modul behandelt.)

## <a name="clientquerystring"></a>ClientQueryString

Eine schreibgeschützte Eigenschaft, die den Teil der Abfrage Zeichenfolge der angeforderten URL zurückgibt. Dieser Wert ist URL-codiert. Sie können die UrlDecode-Methode der HttpServerUtility-Klasse verwenden, um Sie zu decodieren.

## <a name="clientscript"></a>ClientScript

Diese Eigenschaft gibt ein ClientScriptManager-Objekt zurück, das zum Verwalten der ASP. Nets-Ausgabe des Client seitigen Skripts verwendet werden kann. (Die ClientScriptManager-Klasse wird später in diesem Modul behandelt.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Diese Eigenschaft steuert, ob die Ereignis Validierung für Postback-und Rückruf Ereignisse aktiviert ist. Wenn diese Option aktiviert ist, werden Argumente für Postback-oder Rückruf Ereignisse überprüft, um sicherzustellen, dass Sie vom Server Steuerelement stammen, das Sie ursprünglich gerendert

## <a name="enabletheming"></a>Aktivieren

Diese Eigenschaft ruft einen booleschen Wert ab, der angibt, ob ein ASP.NET 2,0-Design auf die Seite angewendet wird, oder legt diesen fest.

## <a name="form"></a>Formular

Diese Eigenschaft gibt das HTML-Formular auf der ASPX-Seite als HtmlForm-Objekt zurück.

## <a name="header"></a>Header

Diese Eigenschaft gibt einen Verweis auf ein HtmlHead-Objekt zurück, das den Seitenkopf enthält. Sie können das zurückgegebene HtmlHead-Objekt verwenden, um Stylesheets, Meta-Tags usw. zu erhalten bzw. festzulegen.

## <a name="idseparator"></a>IdSeparator

Diese schreibgeschützte Eigenschaft ruft das Zeichen ab, das zum Trennen von Steuerelement Bezeichnerzeichen verwendet wird, wenn ASP.net eine eindeutige ID für Steuerelemente auf einer Seite erbringt. Sie sollte nicht direkt im Code verwendet werden.

## <a name="isasync"></a>IsAsync

Diese Eigenschaft ermöglicht asynchrone Seiten. Asynchrone Seiten werden später in diesem Modul erläutert.

## <a name="iscallback"></a>IsCallback

Diese schreibgeschützte Eigenschaft gibt **true** zurück, wenn die Seite das Ergebnis eines Rückruf ist. Rückruf Rückrufe werden später in diesem Modul erläutert.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Diese schreibgeschützte Eigenschaft gibt **true** zurück, wenn die Seite Teil eines Seiten übergreifenden Postbacks ist. Seiten übergreifende Postbacks werden später in diesem Modul behandelt.

## <a name="items"></a>Items

Gibt einen Verweis auf eine IDictionary-Instanz zurück, die alle im Seiten Kontext gespeicherten-Objekte enthält. Sie können diesem IDictionary-Objekt Elemente hinzufügen und stehen Ihnen während der gesamten Lebensdauer des Kontexts zur Verfügung.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Diese Eigenschaft steuert, ob ASP.net JavaScript ausgibt, das die Seitenbild Lauf Position im Browser nach einem Postback beibehält. (Einzelheiten zu dieser Eigenschaft wurden bereits in diesem Modul erläutert.)

## <a name="master"></a>Master

Diese schreibgeschützte Eigenschaft gibt einen Verweis auf die MasterPage-Instanz für eine Seite zurück, auf die eine Master Seite angewendet wurde.

## <a name="masterpagefile"></a>MasterPageFile

Ruft den Dateinamen der Master Seite für die Seite ab oder legt diesen fest. Diese Eigenschaft kann nur in der PreInit-Methode festgelegt werden.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Diese Eigenschaft ruft die maximale Länge für den Seiten Status in Bytes ab oder legt diese fest. Wenn die-Eigenschaft auf eine positive Zahl festgelegt ist, wird der Seiten Ansichts Zustand in mehrere ausgeblendete Felder aufgeteilt, sodass die angegebene Anzahl von Bytes nicht überschritten wird. Wenn die Eigenschaft eine negative Zahl ist, wird der Ansichts Zustand nicht in Blöcke unterteilt.

## <a name="pageadapter"></a>PageAdapter

Gibt einen Verweis auf das PageAdapter-Objekt zurück, das die Seite für den anfordernden Browser ändert.

## <a name="previouspage"></a>PreviousPage

Gibt einen Verweis auf die vorherige Seite in Fällen von Server. Transfer oder einem Seiten übergreifenden Postback zurück.

## <a name="skinid"></a>SkinID

Gibt das ASP.NET 2,0-Skin an, das auf die Seite angewendet werden soll.

## <a name="stylesheettheme"></a>StyleSheetTheme

Diese Eigenschaft ruft das Stylesheet ab, das auf eine Seite angewendet wird, oder legt dieses fest.

## <a name="templatecontrol"></a>TemplateControl

Gibt einen Verweis auf das enthaltende Steuerelement für die Seite zurück.

## <a name="theme"></a>Design

Ruft den Namen des ASP.NET 2,0-Designs ab, das auf die Seite angewendet wird, oder legt diesen fest. Dieser Wert muss vor der PreInit-Methode festgelegt werden.

## <a name="title"></a>Titel

Diese Eigenschaft ruft den Titel für die Seite ab oder legt diesen fest, wie er von der Seiten Kopfzeile abgerufen wird.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ruft den viewstateverschlüsseltionmode der Seite ab oder legt ihn fest. Eine ausführliche Erläuterung dieser Eigenschaft finden Sie weiter oben in diesem Modul.

## <a name="new-protected-properties-of-the-page-class"></a>Neue geschützte Eigenschaften der Page-Klasse

Im folgenden finden Sie die neuen geschützten Eigenschaften der Page-Klasse in ASP.NET 2,0.

## <a name="adapter"></a>Adapter

Gibt einen Verweis auf den ControlAdapter zurück, der die Seite auf dem Gerät rendert, das Sie angefordert hat.

## <a name="asyncmode"></a>AsyncMode

Diese Eigenschaft gibt an, ob die Seite asynchron verarbeitet wird. Sie ist für die Verwendung durch die Laufzeit und nicht direkt im Code vorgesehen.

## <a name="clientidseparator"></a>ClientIDSeparator

Diese Eigenschaft gibt das Zeichen zurück, das beim Erstellen eindeutiger Client-IDs für Steuerelemente als Trennzeichen verwendet wird. Sie ist für die Verwendung durch die Laufzeit und nicht direkt im Code vorgesehen.

## <a name="pagestatepersister"></a>Pagestatepersiester

Diese Eigenschaft gibt das pagestatepersiester-Objekt für die Seite zurück. Diese Eigenschaft wird hauptsächlich von ASP.NET-Steuerelement Entwicklern verwendet.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Diese Eigenschaft gibt ein eindeutiges Suffix zurück, das an den Dateipfad zum Zwischenspeichern von Browsern angefügt wird. Der Standardwert ist \_\_UFPS = und eine 6-stellige Zahl.

## <a name="new-public-methods-for-the-page-class"></a>Neue öffentliche Methoden für die Page-Klasse

Die folgenden öffentlichen Methoden sind neu in der Page-Klasse in ASP.NET 2,0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Diese Methode registriert Ereignishandlerdelegaten für die asynchrone Seiten Ausführung. Asynchrone Seiten werden später in diesem Modul erläutert.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Wendet die Eigenschaften in einem Seiten Stylesheet auf die Seite an.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Diese Methode gibt eine asynchrone Aufgabe aus.

### <a name="getvalidators"></a>GetValidators

Gibt eine Auflistung von Validierungs Steuerelementen für die angegebene Validierungs Gruppe oder die Standard Validierungs Gruppe zurück, wenn kein Wert angegeben ist.

## <a name="registerasynctask"></a>RegisterAsyncTask

Diese Methode registriert eine neue Async-Aufgabe. Asynchrone Seiten werden später in diesem Modul behandelt.

## <a name="registerrequirescontrolstate"></a>Registerrequirements scontrolstate

Diese Methode teilt ASP.net mit, dass der Seiten Steuerelement Zustand beibehalten werden muss.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Diese Methode teilt ASP.net mit, dass die Seiten "ViewState" eine Verschlüsselung erfordern.

## <a name="resolveclienturl"></a>ResolveClientUrl

Gibt einen relative URL zurück, der für Client Anforderungen für Bilder usw. verwendet werden kann.

## <a name="setfocus"></a>SetFocus

Mit dieser Methode wird der Fokus auf das Steuerelement festgelegt, das beim anfänglichen Laden der Seite angegeben wird.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Mit dieser Methode wird die Registrierung des Steuer Elements, das an das Steuerelement weitergeleitet wird, aufgehoben.

## <a name="changes-to-the-page-lifecycle"></a>Änderungen am Seiten Lebenszyklus

Der Seiten Lebenszyklus in ASP.NET 2,0 hat sich nicht geändert, aber es gibt einige neue Methoden, die Sie kennen sollten. Der Lebenszyklus der ASP.NET 2,0-Seite ist unten dargestellt.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (neu in ASP.NET 2,0)

Das PreInit-Ereignis ist die früheste Phase im Lebenszyklus, auf die ein Entwickler zugreifen kann. Durch das Hinzufügen dieses Ereignisses können Sie ASP.NET 2,0 Designs, Masterseiten, Zugriffs Eigenschaften für ein ASP.NET 2,0-Profil usw. Programm gesteuert ändern. Wenn Sie sich in einem Post Back Zustand befinden, ist es wichtig zu erkennen, dass "ViewState" zu diesem Zeitpunkt im Lebenszyklus noch nicht auf Steuerelemente angewendet wurde. Wenn ein Entwickler daher eine Eigenschaft eines Steuer Elements in dieser Phase ändert, wird er wahrscheinlich später im Lebenszyklus der Seiten überschrieben.

## <a name="init"></a>Init

Das Init-Ereignis wurde nicht von ASP.NET 1. x geändert. Hier können Sie die Eigenschaften von Steuerelementen auf Ihrer Seite lesen oder initialisieren. In dieser Phase werden Masterseiten, Designs usw. bereits auf die Seite angewendet.

## <a name="initcomplete-new-in-20"></a>InitComplete (neu in 2,0)

Das Ereignis InitComplete wird am Ende der Initialisierungsphase der Seiten aufgerufen. An dieser Stelle im Lebenszyklus können Sie auf die Steuerelemente auf der Seite zugreifen, ihr Zustand wurde jedoch noch nicht aufgefüllt.

## <a name="preload-new-in-20"></a>Vorab laden (neu in 2,0)

Dieses Ereignis wird aufgerufen, nachdem alle Post Back Daten und unmittelbar vor dem Laden von Seiten\_angewendet wurden.

## <a name="load"></a>Laden

Das Lade Ereignis wurde von ASP.NET 1. x nicht geändert.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (neu in 2,0)

Das LoadComplete-Ereignis ist das letzte Ereignis in der Seiten Lade Phase. Zu diesem Zeitpunkt wurden alle Postback-und ViewState-Daten auf die Seite angewendet.

## <a name="prerender"></a>PreRender

Wenn Sie möchten, dass ViewState für Steuerelemente, die der Seite dynamisch hinzugefügt werden, ordnungsgemäß verwaltet wird, ist das PreRender-Ereignis die letzte Möglichkeit, Sie hinzuzufügen.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (neu in 2,0)

In der PreRenderComplete-Phase wurden der Seite alle Steuerelemente hinzugefügt, und die Seite kann gerendert werden. Das PreRenderComplete-Ereignis ist das letzte Ereignis, das ausgelöst wird, bevor die Seiten "ViewState" gespeichert werden.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (neu in 2,0)

Das SaveStateComplete-Ereignis wird sofort aufgerufen, nachdem alle Seiten Ansichts Zustand und der Steuerelement Zustand gespeichert wurden. Dies ist das letzte Ereignis, bevor die Seite tatsächlich im Browser gerendert wird.

## <a name="render"></a>Rendern

Die Rendering-Methode wurde seit ASP.NET 1. x nicht geändert. An dieser Stelle wird der HtmlTextWriter initialisiert, und die Seite wird im Browser gerendert.

## <a name="cross-page-postback-in-aspnet-20"></a>Seiten übergreifendes Postback in ASP.NET 2,0

In ASP.NET 1. x waren Postbacks zum Posten auf derselben Seite erforderlich. Seiten übergreifende Postbacks waren nicht zulässig. ASP.NET 2,0 bietet die Möglichkeit, auf eine andere Seite über die IButtonControl-Schnittstelle zurückzukehren. Jedes Steuerelement, das die neue IButtonControl-Schnittstelle implementiert (Schaltfläche, LinkButton und ImageButton zusätzlich zu benutzerdefinierten Steuerelementen von Drittanbietern), kann diese neue Funktionalität durch die Verwendung des PostBackUrl-Attributs nutzen. Der folgende Code zeigt ein Schaltflächen-Steuerelement, das zurück an eine zweite Seite sendet.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Wenn die Seite zurückgesendet wird, kann auf die Seite, auf der das Postback initiiert wird, über die Eigenschaft PreviousPage auf der zweiten Seite zugegriffen werden. Diese Funktion wird über das neue Webform-\_die Client seitige Funktion "dopostbackwithoptions" implementiert, die von ASP.NET 2,0 auf die Seite gerendert wird, wenn ein Steuerelement an eine andere Seite zurückgesendet wird. Diese JavaScript-Funktion wird vom neuen WebResource. axd-Handler bereitgestellt, der das Skript für den Client ausgibt.

Das folgende Video ist eine exemplarische Vorgehensweise für ein Seiten übergreifendes Postback.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Vollbildvideos öffnen](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Weitere Informationen zu Seiten übergreifenden Postbacks

### <a name="viewstate"></a>ViewState

Möglicherweise haben Sie sich bereits über das, was mit dem ViewState passiert, von der ersten Seite in einem Seiten übergreifenden Post Back Szenario gefragt. Alle Steuerelemente, die IPostBackDataHandler nicht implementieren, behalten schließlich ihren Zustand über ViewState bei, sodass Sie auf die Eigenschaften dieses Steuer Elements auf der zweiten Seite eines Seiten übergreifenden Postbacks zugreifen können, wenn Sie Zugriff auf den ViewState für die Seite haben. ASP.NET 2,0 übernimmt dieses Szenario mithilfe eines neuen ausgeblendeten Felds auf der zweiten Seite mit dem Namen \_\_PreviousPage. Das Formularfeld \_\_PreviousPage enthält den ViewState für die erste Seite, sodass Sie Zugriff auf die Eigenschaften aller Steuerelemente auf der zweiten Seite haben können.

### <a name="circumventing-findcontrol"></a>Umgehen von FindControl

In der Video exemplarischen Vorgehensweise eines Seiten übergreifenden Postbacks habe ich die FindControl-Methode verwendet, um einen Verweis auf das TextBox-Steuerelement auf der ersten Seite zu erhalten. Diese Methode funktioniert für diesen Zweck gut, aber FindControl ist aufwendig und erfordert das Schreiben von zusätzlichem Code. Glücklicherweise stellt ASP.NET 2,0 für diesen Zweck eine Alternative zu FindControl dar, die in vielen Szenarien funktioniert. Mit der PreviousPageType-Direktive können Sie einen stark typisierten Verweis auf die vorherige Seite verwenden, indem Sie entweder das Typname-Attribut oder das virtualPath-Attribut verwenden. Mit dem tykame-Attribut können Sie den Typ der vorherigen Seite angeben, während das virtualPath-Attribut es Ihnen ermöglicht, mithilfe eines virtuellen Pfads auf die vorherige Seite zu verweisen. Nachdem Sie die PreviousPageType-Direktive festgelegt haben, müssen Sie die Steuerelemente usw. verfügbar machen, mit denen Sie den Zugriff über öffentliche Eigenschaften zulassen möchten.

## <a name="lab-1-cross-page-postback"></a>Lab 1-Seiten übergreifendes Postback

In dieser Übungseinheit erstellen Sie eine Anwendung, die die neue Seiten übergreifende Post Back Funktion von ASP.NET 2,0 verwendet.

1. Öffnen Sie Visual Studio 2005, und erstellen Sie eine neue ASP.NET-Website.
2. Fügen Sie ein neues Webformular mit dem Namen Page2. aspx hinzu.
3. Öffnen Sie "default. aspx" in Designansicht, und fügen Sie ein Schaltflächen-und ein TextBox-Steuerelement hinzu. 

    1. Legen Sie dem Schaltflächen-Steuerelement eine ID von **SubmitButton** und dem Textfeld-Steuerelement eine ID des **Benutzernamens**zu.
    2. Legen Sie die Eigenschaft PostBackUrl der Schaltfläche auf Page2. aspx fest.
4. Öffnen Sie Page2. aspx in der Quell Ansicht.
5. Fügen Sie wie unten gezeigt eine @ PreviousPageType-Direktive hinzu:
6. Fügen Sie den folgenden Code auf der Seite\_Laden von Code Behind von Page2. aspx hinzu: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Erstellen Sie das Projekt, indem Sie im Menü Erstellen auf Erstellen klicken.
8. Fügen Sie den folgenden Code für "default. aspx" in das Code Behind ein: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Ändern Sie die Seite\_laden in Page2. aspx wie folgt: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Erstellen Sie das Projekt.
11. Führen Sie das Projekt aus.
12. Geben Sie Ihren Namen in das Textfeld ein, und klicken Sie auf die Schaltfläche.
13. Was ist das Ergebnis?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchrone Seiten in ASP.NET 2,0

Viele Konfliktprobleme in ASP.net werden durch die Latenz externer Aufrufe (z. b. Webdienst-oder Daten Bank Aufrufe), die Datei-e/a-Latenz usw. verursacht. Wenn eine Anforderung an eine ASP.NET-Anwendung gerichtet wird, verwendet ASP.net einen ihrer Arbeitsthreads, um diese Anforderung zu bedienen. Diese Anforderung besitzt diesen Thread, bis die Anforderung abgeschlossen ist und die Antwort gesendet wurde. ASP.NET 2,0 versucht, Latenzprobleme mit diesen Arten von Problemen aufzulösen, indem die Funktion zum asynchronen Ausführen von Seiten hinzugefügt wird. Dies bedeutet, dass ein Arbeits Thread die Anforderung starten und dann eine weitere Ausführung an einen anderen Thread übergeben kann, sodass er schnell zum verfügbaren Thread Pool zurückkehrt. Wenn die Datei-e/a, der Daten Bank Rückruf usw. abgeschlossen ist, wird ein neuer Thread aus dem Thread Pool abgerufen, um die Anforderung abzuschließen.

Der erste Schritt bei der asynchronen Ausführung einer Seite besteht darin, das **Async** -Attribut der Page-Direktive wie folgt festzulegen:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Dieses Attribut weist ASP.net an, den IHttpAsyncHandler für die Seite zu implementieren.

Der nächste Schritt besteht darin, die AddOnPreRenderCompleteAsync-Methode an einem Punkt im Lebenszyklus der Seite vor der Vorabversion aufzurufen. (Diese Methode wird in der Regel in Page\_Load aufgerufen.) Die AddOnPreRenderCompleteAsync-Methode nimmt zwei Parameter an: Ein BeginEventHandler und ein EndEventHandler. BeginEventHandler gibt ein iasynkresult zurück, das dann als Parameter an den EndEventHandler übergeben wird.

Das folgende Video ist eine exemplarische Vorgehensweise für eine asynchrone Seiten Anforderung.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Vollbildvideos öffnen](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Eine Async-Seite wird erst dann in den Browser gerengt, wenn der EndEventHandler abgeschlossen wurde. Zweifellos, aber einige Entwickler werden sich asynchrone Anforderungen als ähnlich wie Async-Rückrufe vorstellen. Es ist wichtig zu wissen, dass dies nicht der Fall ist. Der Vorteil von asynchronen Anforderungen besteht darin, dass der erste Arbeits Thread an den Thread Pool zurückgegeben werden kann, um neue Anforderungen zu bedienen, wodurch Konflikte aufgrund von e/a-gebundenen usw. reduziert werden.

## <a name="script-callbacks-in-aspnet-20"></a>Skript Rückrufe in ASP.NET 2,0

Webentwickler haben immer nach Möglichkeiten gesucht, um das Flimmern zu verhindern, das einem Rückruf zugeordnet ist. In ASP.NET 1. x war SmartNavigation die gängigste Methode, um das Flimmern zu vermeiden, aber die SmartNavigation verursachte Probleme für einige Entwickler aufgrund der Komplexität der Implementierung auf dem Client. ASP.NET 2,0 behandelt dieses Problem mit Skript Rückrufen. Skript Rückrufe verwenden XMLHTTP, um Anforderungen an den Webserver über JavaScript zu senden. Die XMLHTTP-Anforderung gibt XML-Daten zurück, die dann über das DOM des Browsers bearbeitet werden können. Der XMLHTTP-Code wird vom neuen WebResource. axd-Handler für den Benutzer ausgeblendet.

Es gibt mehrere Schritte, die erforderlich sind, um einen Skript Rückruf in ASP.NET 2,0 zu konfigurieren.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Schritt 1: Implementieren der ICallbackEventHandler-Schnittstelle

Damit ASP.net Ihre Seite als Teil eines Skript Rückrufs erkennen kann, müssen Sie die ICallbackEventHandler-Schnittstelle implementieren. Dies können Sie in der Code-Behind-Datei wie folgt durchführen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Sie können dies auch mit der @ implementierten Direktive tun, wie folgt:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalerweise verwenden Sie die @ implementierte Direktive, wenn Sie Inline-ASP.NET-Code verwenden.

## <a name="step-2--call-getcallbackeventreference"></a>Schritt 2: GetCallbackEventReference aufrufen

Wie bereits erwähnt, wird der XMLHTTP-Befehl im WebResource. axd-Handler gekapselt. Wenn die Seite gerendert wird, fügt ASP.net einen Webform-\_DoCallBack-Rückruf, ein Client Skript, das von WebResource. axd bereitgestellt wird, hinzu. Das Webform-\_DoCallBack-Funktion ersetzt die \_\_DoPostBack-Funktion für einen Rückruf. Beachten Sie, dass \_\_DoPostBack Programm gesteuert das Formular auf der Seite übermittelt. In einem Rückruf Szenario möchten Sie ein Postback verhindern, sodass \_\_DoPostBack nicht ausreicht.

> [!NOTE]
> \_\_DoPostBack wird in einem Client Skript-Rückruf Szenario weiterhin auf der Seite gerendert. Sie wird jedoch nicht für den Rückruf verwendet.

Die Argumente für den Webform-\_Client seitigen DoCallBack-Funktion werden über die serverseitige Funktion GetCallbackEventReference bereitgestellt, die normalerweise in Seiten\_Last aufgerufen wird. Ein typischer Aufruf von GetCallbackEventReference könnte wie folgt aussehen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> In diesem Fall ist cm eine Instanz von ClientScriptManager. Die ClientScriptManager-Klasse wird später in diesem Modul behandelt.

Es gibt mehrere überladene Versionen von GetCallbackEventReference. Die Argumente lauten in diesem Fall wie folgt:

`this`

Ein Verweis auf das Steuerelement, in dem GetCallbackEventReference aufgerufen wird. In diesem Fall ist es die Seite selbst.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Ein Zeichen folgen Argument, das vom Client seitigen Code an das serverseitige Ereignis übermittelt wird. In diesem Fall wird der Wert einer Dropdown Liste mit dem Namen "ddlcompany" übergeben.

`ShowCompanyName`

Der Name der Client seitigen Funktion, die den Rückgabewert (als Zeichenfolge) aus dem serverseitigen Rückruf Ereignis akzeptiert. Diese Funktion wird nur aufgerufen, wenn der serverseitige Rückruf erfolgreich ist. Daher empfiehlt es sich im Allgemeinen, die überladene Version von GetCallbackEventReference zu verwenden, die ein zusätzliches Zeichen folgen Argument annimmt, das den Namen einer Client seitigen Funktion angibt, die im Falle eines Fehlers ausgeführt werden soll.

`null`

Eine Zeichenfolge, die eine Client seitige Funktion darstellt, die Sie vor dem Rückruf an den Server initiiert hat. In diesem Fall gibt es kein solches Skript, sodass das Argument NULL ist.

`true`

Ein boolescher Wert, der angibt, ob der Rückruf asynchron durchgeführt werden soll.

Der Webform-Webformular\_DoCallBack auf dem Client übergibt diese Argumente. Wenn diese Seite auf dem Client gerendert wird, sieht der Code daher wie folgt aus:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Beachten Sie, dass die Signatur der Funktion auf dem Client etwas abweicht. Die Client seitige Funktion übergibt fünf Zeichen folgen und einen booleschen Wert. Die zusätzliche Zeichenfolge (die im obigen Beispiel NULL ist) enthält die Client seitige Funktion, die alle Fehler des serverseitigen Rückrufs behandelt.

## <a name="step-3--hook-the-client-side-control-event"></a>Schritt 3: Anschließen des Client seitigen Steuerungs Ereignisses

Beachten Sie, dass der Rückgabewert von GetCallbackEventReference oben einer Zeichen folgen Variablen zugewiesen wurde. Diese Zeichenfolge wird verwendet, um ein Client seitiges Ereignis für das Steuerelement zu verbinden, das den Rückruf initiiert. In diesem Beispiel wird der Rückruf durch eine Dropdown Liste auf der Seite initiiert, daher möchte ich das *OnChange* -Ereignis anschließen.

Fügen Sie dem Client seitigen Markup einfach wie folgt einen Handler hinzu, um das Client seitige Ereignis zu verbinden:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Denken Sie daran, dass *cbref* der Rückgabewert des Aufrufs von GetCallbackEventReference ist. Sie enthält den Webform-Aufrufs\_DoCallBack, der oben gezeigt wurde.

## <a name="step-4--register-the-client-side-script"></a>Schritt 4: Registrieren des Client seitigen Skripts

Beachten Sie, dass der Aufruf von GetCallbackEventReference festgelegt hat, dass ein Client seitiges Skript namens **showcompanyname** ausgeführt wird, wenn der serverseitige Rückruf erfolgreich ist. Dieses Skript muss mithilfe einer ClientScriptManager-Instanz der Seite hinzugefügt werden. (Die ClientScriptManager-Klasse wird weiter unten in diesem Modul erläutert.) Dies geschieht wie folgt:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Schritt 5: Aufrufen der Methoden der ICallbackEventHandler-Schnittstelle

Der ICallbackEventHandler enthält zwei Methoden, die Sie in Ihrem Code implementieren müssen. Sie sind **raisegcallbackevent** und **getcallbackevent**.

**Raisegcallbackevent** nimmt eine Zeichenfolge als Argument an und gibt nichts zurück. Das Zeichen folgen Argument wird vom Client seitigen Aufrufs an Webform\_DoCallBack übermittelt. In diesem Fall handelt es sich bei diesem Wert um das *value* -Attribut der Dropdown Liste ddlcompany. Der serverseitige Code sollte in der RaiseCallbackEvent-Methode platziert werden. Wenn Ihr Rückruf beispielsweise eine WebRequest für eine externe Ressource durchläuft, sollte dieser Code in RaiseCallbackEvent abgelegt werden.

**Getcallbackevent** ist für die Verarbeitung der Rückgabe des Rückrufs an den Client verantwortlich. Sie nimmt keine Argumente an und gibt eine Zeichenfolge zurück. Die zurückgegebene Zeichenfolge wird als Argument an die Client seitige Funktion (in diesem Fall " *showcompanyname*") weitergegeben.

Nachdem Sie die obigen Schritte ausgeführt haben, können Sie einen Skript Rückruf in ASP.NET 2,0 ausführen.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Vollbildvideos öffnen](the-asp-net-2-0-page-model/_static/callback1.wmv)

Skript Rückrufe in ASP.net werden in jedem Browser unterstützt, der XMLHTTP-Aufrufe unterstützt. Dies schließt alle modernen Browser ein, die heute verwendet werden. Internet Explorer verwendet das XMLHTTP-ActiveX-Objekt, während andere moderne Browser (einschließlich der bevorstehenden IE 7) ein System internes XMLHTTP-Objekt verwenden. Um Programm gesteuert zu ermitteln, ob ein Browser Rückrufe unterstützt, können Sie die **Request. Browser. supportcallback** -Eigenschaft verwenden. Diese Eigenschaft gibt **true** zurück, wenn der anfordernde Client Skript Rückrufe unterstützt.

## <a name="working-with-client-script-in-aspnet-20"></a>Arbeiten mit Client Skripts in ASP.NET 2,0

Client Skripts in ASP.NET 2,0 werden über die Verwendung der ClientScriptManager-Klasse verwaltet. Die ClientScriptManager-Klasse verfolgt Client Skripts, die einen Typ und einen Namen verwenden, nachverfolgt. Dadurch wird verhindert, dass das gleiche Skript Programm gesteuert auf einer Seite mehrmals eingefügt wird.

> [!NOTE]
> Nach erfolgreicher Registrierung eines Skripts auf einer Seite führt jeder nachfolgende Versuch, das Skript zu registrieren, dazu, dass das Skript nicht ein zweites Mal registriert wird. Doppelte Skripts werden nicht hinzugefügt, und es tritt keine Ausnahme auf. Um unnötige Berechnungen zu vermeiden, gibt es Methoden, mit denen Sie bestimmen können, ob ein Skript bereits registriert ist, sodass Sie nicht versuchen, es mehrmals zu registrieren.

Die Methoden von ClientScriptManager sollten allen aktuellen ASP.NET-Entwicklern vertraut sein:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Mit dieser Methode wird oben auf der gerenderten Seite ein Skript hinzugefügt. Dies ist nützlich für das Hinzufügen von Funktionen, die explizit auf dem Client aufgerufen werden.

Es gibt zwei überladene Versionen dieser Methode. Es gibt drei von vier Argumenten, die häufig vorkommen. Sie lauten wie folgt:

`type (string)`

Das ***Typargument*** identifiziert einen Typ für das Skript. Im Allgemeinen empfiehlt es sich, den Typ der Seite zu verwenden. GetType ()) für den Typ.

`key (string)`

Das ***Schlüssel*** Argument ist ein benutzerdefinierter Schlüssel für das Skript. Dies sollte für jedes Skript eindeutig sein. Wenn Sie versuchen, ein Skript mit dem gleichen Schlüssel und Typ eines bereits hinzugefügten Skripts hinzuzufügen, wird es nicht hinzugefügt.

`script (string)`

Das ***Skript*** Argument ist eine Zeichenfolge, die das tatsächlich hinzu zufügende Skript enthält. Es wird empfohlen, ein StringBuilder-Skript zu verwenden, um das Skript zu erstellen, und dann die Methode "" mit der Methode "" in StringBuilder zum Zuweisen des ***Skript*** Arguments zu verwenden.

Wenn Sie den überladenen RegisterClientScriptBlock verwenden, der nur drei Argumente annimmt, müssen Sie Skript Elemente (&lt;Skript&gt; und &lt;/Script&gt;) in das Skript einschließen.

Sie können die Überladung von RegisterClientScriptBlock verwenden, die ein viertes Argument annimmt. Das vierte Argument ist ein boolescher Wert, der angibt, ob ASP.NET Skript Elemente für Sie hinzufügen soll. Wenn dieses Argument **true**ist, sollte das Skript die Skript Elemente nicht explizit enthalten.

Verwenden Sie die IsClientScriptBlockRegistered-Methode, um zu bestimmen, ob bereits ein Skript registriert wurde. Dies ermöglicht es Ihnen, einen Versuch zu vermeiden, ein Skript erneut zu registrieren, das bereits registriert wurde.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (neu in 2,0)

Das RegisterClientScriptInclude-Tag erstellt einen Skriptblock, der mit einer externen Skriptdatei verknüpft ist. Es verfügt über zwei über Ladungen. Eine erfordert einen Schlüssel und eine URL. Die zweite fügt ein drittes Argument hinzu, das den Typ angibt.

Der folgende Code generiert z. b. einen Skriptblock, der im Stammverzeichnis des Skripts für die Anwendung mit "jsfunctions. js" verknüpft ist:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Mit diesem Code wird der folgende Code auf der gerenderten Seite erzeugt:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Der Skriptblock wird am unteren Rand der Seite gerendert.

Verwenden Sie die IsClientScriptIncludeRegistered-Methode, um zu bestimmen, ob bereits ein Skript registriert wurde. Dies ermöglicht es Ihnen, einen Versuch zu vermeiden, ein Skript erneut zu registrieren.

## <a name="registerstartupscript"></a>RegisterStartupScript

Die RegisterStartupScript-Methode verwendet die gleichen Argumente wie die RegisterClientScriptBlock-Methode. Ein Skript, das mit RegisterStartupScript registriert wird, wird ausgeführt, nachdem die Seite geladen wurde, jedoch vor dem Client seitigen OnLoad-Ereignis. In 1. X wurden Skripts, die mit RegisterStartupScript registriert wurden, direkt vor dem schließenden &lt;"/Form"&gt; Tag platziert, während Skripts, die mit RegisterClientScriptBlock registriert wurden, unmittelbar nach dem öffnenden &lt;Formular&gt; Tag platziert wurden. In ASP.NET 2,0 werden beide unmittelbar vor dem schließenden &lt;"/Form"&gt;-Tag platziert.

> [!NOTE]
> Wenn Sie eine Funktion mit RegisterStartupScript registrieren, wird diese Funktion erst ausgeführt, wenn Sie Sie explizit im Client seitigen Code aufrufen.

Verwenden Sie die IsStartupScriptRegistered-Methode, um zu bestimmen, ob bereits ein Skript registriert wurde, und vermeiden Sie einen Versuch, ein Skript erneut zu registrieren.

## <a name="other-clientscriptmanager-methods"></a>Weitere ClientScriptManager-Methoden

Im folgenden finden Sie einige der anderen nützlichen Methoden der ClientScriptManager-Klasse.

|  <strong>GetCallbackEventReference</strong>   |                                                 Weitere Informationen finden Sie unter Skript Rückrufe weiter oben in diesem Modul.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ruft einen JavaScript-Verweis (JavaScript:&lt;-&gt;) ab, der zum Postback von einem Client seitigen Ereignis verwendet werden kann.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ruft eine Zeichenfolge ab, die zum Initiieren eines Postbacks vom Client verwendet werden kann.                                    |
|      <strong>GetWebResourceUrl</strong>       | Gibt eine URL zu einer Ressource zurück, die in eine Assembly eingebettet ist. Muss in Verbindung mit <strong>RegisterClientScriptResource</strong>verwendet werden. |
| <strong>RegisterClientScriptResource</strong> |     Registriert eine Webressource bei der Seite. Dabei handelt es sich um Ressourcen, die in eine Assembly eingebettet sind und vom neuen WebResource. axd-Handler verarbeitet werden.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registriert ein ausgeblendetes Formularfeld bei der Seite.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registriert Client seitigen Code, der ausgeführt wird, wenn das HTML-Formular übermittelt wird.                                   |
