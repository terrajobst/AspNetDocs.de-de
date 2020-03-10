---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Grundlegendes zu Teil Aktualisierungen von Seiten mit ASP.NET AJAX | Microsoft-Dokumentation
author: scottcate
description: Die vielleicht sichtbare Funktion der ASP.NET-AJAX-Erweiterungen ist die Möglichkeit, eine partielle oder inkrementelle Seiten Aktualisierung durchzuführen, ohne ein vollständiges Postback an "t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4b87cb8f58dbd7f27b16bcb0d488ff361770d4fe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439269"
---
# <a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Grundlegendes zu Teilupdates für Seiten mit ASP.NET AJAX

von [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Die am häufigsten sichtbare Funktion der ASP.NET-AJAX-Erweiterungen ist die Möglichkeit, partielle oder inkrementelle Seiten Aktualisierungen durchzuführen, ohne dass ein vollständiges Postback an den Server erfolgt, ohne Codeänderungen und minimale Markup Änderungen. Die Vorteile sind umfangreich – der Status Ihrer Multimedia (z. b. Adobe Flash oder Windows Media) ist unverändert, die Bandbreiten Kosten werden reduziert, und der Client hat nicht das Flimmern, das in der Regel mit einem Postback verknüpft ist.

## <a name="introduction"></a>Einführung

Die ASP.NET-Technologie von Microsoft sorgt für ein objektorientiertes und Ereignis gesteuertes Programmiermodell und vereinigt es mit den Vorteilen von kompiliertem Code. Das serverseitige Verarbeitungsmodell hat jedoch mehrere Nachteile in der Technologie:

- Für Seiten Aktualisierungen ist ein Roundtrip zum Server erforderlich, für den eine Seiten Aktualisierung erforderlich ist.
- Bei Roundtrips werden keine von JavaScript oder anderen Client seitigen Technologien (z. b. Adobe Flash) generierten Effekte persistent gespeichert.
- Während des Postbacks unterstützen andere Browser als Microsoft Internet Explorer das automatische Wiederherstellen der Scrollposition nicht. Auch in Internet Explorer gibt es immer noch ein Flimmern, wenn die Seite aktualisiert wird.
- Postbacks können eine hohe Bandbreite enthalten, da die \_\_ViewState-Formularfeld vergrößert werden kann, insbesondere beim Umgang mit Steuerelementen wie z. b. dem GridView-Steuerelement oder repeatzer.
- Es gibt kein einheitliches Modell für den Zugriff auf Webdienste über JavaScript oder andere Client seitige Technologien.

Geben Sie die ASP.NET AJAX-Erweiterungen von Microsoft ein. AJAX, das für **ein** synchrones **J** avaScript **A** ND **X** ml steht, ist ein integriertes Framework zum Bereitstellen von inkrementellen Seiten Aktualisierungen über plattformübergreifendes JavaScript, bestehend aus Server seitigem Code, der das Microsoft AJAX-Framework umfasst, und einer Skript Komponente, die als Microsoft AJAX-Skript Bibliothek bezeichnet wird. Die ASP.NET-AJAX-Erweiterungen bieten außerdem plattformübergreifende Unterstützung für den Zugriff auf ASP.NET-Webdienste über JavaScript.

In diesem Whitepaper werden die Funktionen zur teilweisen Seiten Aktualisierung der ASP.NET AJAX-Erweiterungen untersucht, die die ScriptManager-Komponente, das Update Panel-Steuerelement und das Update Progress-Steuerelement enthalten, und es werden Szenarien berücksichtigt, in denen Sie nicht eingesetzt.

Dieses Whitepaper basiert auf der Beta 2-Version von Visual Studio 2008 und der .NET Framework 3,5, die die ASP.NET AJAX-Erweiterungen in die Basisklassen Bibliothek integriert (wobei es sich zuvor um eine Add-on-Komponente handelt, die für ASP.NET 2,0 verfügbar ist). In diesem Whitepaper wird auch davon ausgegangen, dass Sie Visual Studio 2008 und nicht Visual Web Developer Express Edition verwenden. Einige Projektvorlagen, auf die verwiesen wird, sind für Visual Web Developer Express-Benutzer möglicherweise nicht verfügbar.

## <a name="partial-page-updates"></a>Teil Seiten Aktualisierungen

Die am häufigsten sichtbare Funktion der ASP.NET-AJAX-Erweiterungen ist die Möglichkeit, partielle oder inkrementelle Seiten Aktualisierungen durchzuführen, ohne dass ein vollständiges Postback an den Server erfolgt, ohne Codeänderungen und minimale Markup Änderungen. Die Vorteile sind umfangreich: der Status Ihrer Multimedia (z. b. Adobe Flash oder Windows Media) ist unverändert, die Bandbreiten Kosten werden reduziert, und der Client hat nicht das Flimmern, das in der Regel mit einem Postback verknüpft ist.

Die Möglichkeit zum Integrieren des partiellen Renderings von Seiten ist in ASP.net mit minimalen Änderungen in das Projekt integriert.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Exemplarische Vorgehensweise: Integrieren von partiellem Rendering in ein vorhandenes Projekt

1. Erstellen Sie in Microsoft Visual Studio 2008 ein neues ASP.NET-Website Projekt, indem Sie auf <em>Datei</em> <em>-&gt; neue</em> <em>-&gt; Website</em> klicken und im Dialogfeld ASP.NET-Website auswählen. Sie können es beliebig benennen, und Sie können es entweder im Dateisystem oder in Internetinformationsdienste (IIS) installieren.
2. Ihnen wird die leere Standardseite mit Basic ASP.NET Markup (ein serverseitiges Formular und eine `@Page`-Direktive) angezeigt. Legen Sie eine Bezeichnung mit dem Namen `Label1` und eine Schaltfläche mit dem Namen `Button1` auf der Seite im Formular Element ab. Sie können Ihre Texteigenschaften beliebig festlegen.
3. Doppelklicken Sie in Designansicht auf `Button1`, um einen Code Behind-Ereignishandler zu generieren. Legen Sie in diesem Ereignishandler `Label1.Text` auf die Schaltfläche geklickt haben. erforderlich.

**Codebeispiel 1: Markup für "default. aspx", bevor das partielle Rendering aktiviert ist**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Codebeispiel 2: Code Behind (gekürzt) in Default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Drücken Sie F5, um die Website zu starten. In Visual Studio werden Sie aufgefordert, eine Web. config-Datei hinzuzufügen, um das Debugging zu aktivieren. gehen Sie so vor. Wenn Sie auf die Schaltfläche klicken, wird die Seite aktualisiert, um den Text in der Bezeichnung zu ändern, und es gibt ein kurzes Flimmern, wenn die Seite neu gezeichnet wird.
2. Nachdem Sie das Browserfenster geschlossen haben, kehren Sie zu Visual Studio und zur Markup Seite zurück. Scrollen Sie in der Visual Studio-Toolbox nach unten, und suchen Sie die Registerkarte mit der Bezeichnung AJAX Extensions. (Wenn Sie nicht über diese Registerkarte verfügen, weil Sie eine ältere Version von AJAX-oder Atlas-Erweiterungen verwenden, lesen Sie die exemplarische Vorgehensweise zum Registrieren der Toolbox Elemente für AJAX-Erweiterungen weiter unten in diesem Whitepaper, oder installieren Sie die aktuelle Version mit dem herunterladbaren Windows Installer. von der-Website).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))

1. <em>Bekanntes Problem:</em> Wenn Sie Visual Studio 2008 auf einem Computer installieren, auf dem Visual Studio 2005 bereits mit den ASP.NET 2,0-Erweiterungen installiert ist, importiert Visual Studio 2008 die Toolbox Elemente der AJAX-Erweiterungen. Sie können bestimmen, ob dies der Fall ist, indem Sie die QuickInfo der Komponenten untersuchen. Sie sollten die Version 5.0.0. Wenn Sie die Version 2.0.0.0 angeben, haben Sie Ihre alten Toolbox Elemente importiert und müssen Sie manuell mithilfe des Dialog Felds Toolbox Elemente auswählen in Visual Studio importieren. Sie können keine Steuerelemente der Version 2 über den Designer hinzufügen.

2. Bevor das `<asp:Label>`-Tag beginnt, erstellen Sie eine Zeile mit Leerzeichen, und doppelklicken Sie auf das Update Panel-Steuerelement in der Toolbox. Beachten Sie, dass am oberen Rand der Seite eine neue `@Register`-Direktive enthalten ist, die angibt, dass Steuerelemente im System. Web. UI-Namespace mit dem Präfix `asp:` importiert werden sollen.
3. Ziehen Sie das schließende `</asp:UpdatePanel>`-Tag nach dem Ende des Schaltflächen Elements, damit das Element wohl geformt ist und die Bezeichnung und Schaltflächen-Steuerelemente umschlossen sind.
4. Beginnen Sie nach dem öffnenden `<asp:UpdatePanel>`-Tag mit dem Öffnen eines neuen Tags. Beachten Sie, dass IntelliSense Sie durch zwei Optionen auffordert. Erstellen Sie in diesem Fall ein `<ContentTemplate>`-Tag. Stellen Sie sicher, dass Sie dieses Tag um die Bezeichnung und die Schaltfläche umschließen, damit das Markup wohl geformt ist.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))

1. Fügen Sie im `<form>`-Element ein ScriptManager-Steuerelement ein, indem Sie auf das `ScriptManager` Element in der Toolbox doppelklicken.
2. Bearbeiten Sie das `<asp:ScriptManager>`-Tag, sodass es das-Attribut `EnablePartialRendering= true`enthält.

**Codebeispiel 3: Markup für "default. aspx" mit aktiviertem partiellem Rendering**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Öffnen Sie die Datei "Web. config". Beachten Sie, dass Visual Studio automatisch einen Kompilierungs Verweis auf System. Web. Extensions. dll hinzugefügt hat.

1. Neues in Visual Studio 2008: die Web. config-Datei, die in den ASP.NET-Website-Projektvorlagen enthalten ist, enthält automatisch alle erforderlichen Verweise auf die ASP.NET AJAX-Erweiterungen und enthält kommentierte Abschnitte der Konfigurationsinformationen, die nicht kommentiert, um zusätzliche Funktionalität zu ermöglichen. Visual Studio 2005 enthielt ähnliche Vorlagen, als ASP.NET 2,0 AJAX-Erweiterungen installiert wurden. In Visual Studio 2008 sind die AJAX-Erweiterungen jedoch standardmäßig deaktiviert (d. h., Sie werden standardmäßig referenziert, können aber als Verweise entfernt werden).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))

1. Drücken Sie F5, um die Website zu starten. Beachten Sie, dass keine Quell Codeänderungen erforderlich waren, um das partielle Rendering zu unterstützen. das Markup wurde geändert.

Wenn Sie Ihre Website starten, sollten Sie sehen, dass das partielle Rendering jetzt aktiviert ist, denn wenn Sie auf die Schaltfläche klicken, wird kein Flimmern angezeigt, und es gibt auch keine Änderung an der Seitenbild Lauf Position (in diesem Beispiel wird dies nicht veranschaulicht). Wenn Sie die gerenderte Quelle der Seite nach dem Klicken auf die Schaltfläche betrachten, wird bestätigt, dass tatsächlich kein Postback stattgefunden hat. der ursprüngliche Bezeichnungs Text ist noch Teil des Quell Markups, und die Bezeichnung wurde über JavaScript geändert.

Visual Studio 2008 scheint keine vordefinierte Vorlage für eine ASP.NET AJAX-aktivierte Website zu erhalten. Eine solche Vorlage war jedoch in Visual Studio 2005 verfügbar, wenn die Visual Studio 2005-und ASP.NET 2,0-AJAX-Erweiterungen installiert wurden. Folglich ist das Konfigurieren einer Website und das Starten mit der AJAX-aktivierten Website Vorlage wahrscheinlich sogar noch einfacher, da die Vorlage eine vollständig konfigurierte Web. config-Datei enthalten muss (unterstützt alle ASP.NET AJAX-Erweiterungen, einschließlich Webdienst Zugriff). und JSON-Serialisierung-JavaScript Object Notation) und enthält standardmäßig ein Update Panel und ContentTemplate auf der Haupt Web Forms Seite. Das Aktivieren des partiellen Renderings mit dieser Standardseite ist so einfach wie das erneute besuchen dieser exemplarischen Vorgehensweise und das Ablegen von Steuerelementen auf der Seite.

## <a name="the-scriptmanager-control"></a>Das ScriptManager-Steuerelement

## <a name="scriptmanager-control-reference"></a>Referenz zum ScriptManager-Steuerelement

Markup aktivierte Eigenschaften:

| **Eigenschaftenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Allowcustomerrors-Umleitung | Bool | Gibt an, ob der benutzerdefinierte Fehler Abschnitt der Datei "Web. config" verwendet werden soll, um Fehler zu behandeln. |
| AsyncPostBackError-Message | Zeichenfolge | Ruft die an den Client gesendete Fehlermeldung ab, wenn ein Fehler ausgelöst wird, oder legt diese fest. |
| AsyncPostBack-Timeout | Int32 | Ruft die Standardzeit Spanne ab, die ein Client auf den Abschluss der asynchronen Anforderung warten soll, oder legt diesen fest. |
| EnableScript-Globalization | Bool | Ruft ab oder legt fest, ob die Skript Globalisierung aktiviert ist. |
| EnableScript-Localization | Bool | Ruft ab oder legt fest, ob die Skript Lokalisierung aktiviert ist. |
| ScriptLoadTimeout | Int32 | Bestimmt die Anzahl von Sekunden, die zum Laden von Skripts in den Client zulässig sind. |
| ScriptMode | Enumeration (Auto, Debug, Release, erben) | Ruft ab oder legt fest, ob Releaseversionen von Skripts dargestellt werden |
| scriptPath | Zeichenfolge | Ruft den Stammpfad zum Speicherort der Skriptdateien ab, die an den Client gesendet werden sollen, oder legt diesen fest. |

Nur-Code-Eigenschaften:

| **Eigenschaftenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Ruft Details zum ASP.NET-Authentifizierungsdienst Proxy ab, der an den Client gesendet wird. |
| IsDebug-gingenabled | Bool | Ruft ab, ob Skript-und Code Debuggen aktiviert ist. |
| IsInAsyncPostback | Bool | Ruft ab, ob sich die Seite derzeit in einer asynchronen Post Back Anforderung befindet. |
| Profile Service | ProfileService-Manager | Ruft Details zum Proxy des ASP.NET-Profil Erstellungs Dienstanbieter ab, der an den Client gesendet wird. |
| Skripts | Sammlungs&lt;Skript-Verweis&gt; | Ruft eine Auflistung von Skript verweisen ab, die an den Client gesendet werden. |
| Dienste | Sammlungs&lt;Dienst-Verweis&gt; | Ruft eine Auflistung von Webdienst-Proxy verweisen ab, die an den Client gesendet werden. |
| SupportsPartialRendering | Bool | Ruft ab, ob der aktuelle Client partielles Rendering unterstützt. Wenn diese Eigenschaft **false**zurückgibt, werden alle Seiten Anforderungen Standard Postbacks. |

Öffentliche Code Methoden:

| **Methodenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| SetFocus (Zeichenfolge) | Void | Legt den Fokus des Clients auf ein bestimmtes Steuerelement fest, wenn die Anforderung abgeschlossen wurde. |

Markup Nachfolger:

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;"AuthenticationService"&gt; | Stellt Details zum Proxy für den ASP.NET-Authentifizierungsdienst bereit. |
| &lt;ProfileService&gt; | Stellt Details zum Proxy für den ASP.NET-Profil Erstellungs Dienst bereit. |
| &lt;Skripts&gt; | Stellt zusätzliche Skript Verweise bereit. |
| &lt;ASP: scriptreferen&gt; | Bezeichnet einen bestimmten Skript Verweis. |
| &lt;Dienst&gt; | Bietet zusätzliche Webdienst Verweise, für die Proxy Klassen generiert werden. |
| &lt;ASP: servicereferenzierungs&gt; | Bezeichnet einen bestimmten Webdienst Verweis. |

Das ScriptManager-Steuerelement ist der wesentliche Kern für die ASP.NET AJAX-Erweiterungen. Sie ermöglicht den Zugriff auf die Skript Bibliothek (einschließlich des umfassenden Client seitigen skripttypsystems), unterstützt partielles Rendering und bietet umfassende Unterstützung für zusätzliche ASP.NET Services (z. b. Authentifizierung und Profilerstellung, aber auch andere Webdienste). Das ScriptManager-Steuerelement bietet auch Unterstützung für Globalisierung und Lokalisierung für die Client Skripts.

## <a name="providing-alternative-and-supplemental-scripts"></a>Bereitstellen von Alternativen und ergänzenden Skripts

Obwohl die Microsoft ASP.NET 2,0-AJAX-Erweiterungen den gesamten Skriptcode sowohl in der Debug-als auch in der releaseedition als Ressourcen enthalten, die in die referenzierten Assemblys eingebettet sind, können Entwickler den ScriptManager an angepasste Skriptdateien umleiten und registrieren. Weitere erforderliche Skripts.

Um die Standard Bindung für die in der Regel enthaltenen Skripts zu überschreiben (z. b. diejenigen, die den sys. WebForms-Namespace und das benutzerdefinierte Typisierungs System unterstützen), können Sie sich für das `ResolveScriptReference`-Ereignis der ScriptManager-Klasse registrieren. Wenn diese Methode aufgerufen wird, hat der Ereignishandler die Möglichkeit, den Pfad zur fraglichen Skriptdatei zu ändern. der Skript-Manager sendet dann eine andere oder angepasste Kopie der Skripts an den Client.

Darüber hinaus können Skript Verweise (dargestellt durch die `ScriptReference`-Klasse) Programm gesteuert oder über Markup eingefügt werden. Ändern Sie hierzu entweder Programm gesteuert die `ScriptManager.Scripts` Auflistung, oder fügen Sie `<asp:ScriptReference>` Tags unter dem `<Scripts>`-Tag ein, bei dem es sich um ein untergeordnetes Element der ersten Ebene des ScriptManager-Steuer Elements handelt.

## <a name="custom-error-handling-for-updatepanels"></a>Benutzerdefinierte Fehlerbehandlung für Update Panels

Obwohl Updates von Triggern behandelt werden, die von den Update Panel-Steuerelementen angegeben werden, wird die Unterstützung für die Fehlerbehandlung und benutzerdefinierte Fehlermeldungen von der ScriptManager-Steuerelement Instanz einer Seite behandelt. Dies erfolgt durch das verfügbar machen eines Ereignisses (`AsyncPostBackError`) auf der Seite, das dann eine benutzerdefinierte Ausnahme Behandlungs Logik bereitstellen kann.

Wenn Sie das AsyncPostBackError-Ereignis verarbeiten, können Sie die `AsyncPostBackErrorMessage`-Eigenschaft angeben, die dann bewirkt, dass nach Abschluss des Rückrufs ein Benachrichtigungs Feld ausgelöst wird.

Die Client seitige Anpassung ist auch möglich, anstatt das Standard Benachrichtigungs Feld zu verwenden. Beispielsweise kann es sein, dass Sie ein angepasstes `<div>`-Element anstelle des modalen Standard Dialogfelds des Browsers anzeigen möchten. In diesem Fall können Sie den Fehler in Client Skripts behandeln:

**Codebeispiel 5: Client seitiges Skript zum Anzeigen von benutzerdefinierten Fehlern**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Ganz einfach registriert das obige Skript einen Rückruf mit der Client seitigen AJAX-Laufzeit für den Zeitpunkt, zu dem die asynchrone Anforderung abgeschlossen wurde. Anschließend wird überprüft, ob ein Fehler gemeldet wurde. wenn dies der Fall ist, werden die Details verarbeitet, und schließlich wird der Laufzeit mitgeteilt, dass der Fehler in benutzerdefiniertem Skript behandelt wurde.

## <a name="globalization-and-localization-support"></a>Unterstützung für Globalisierung und Lokalisierung

Das ScriptManager-Steuerelement bietet umfassende Unterstützung für die Lokalisierung von Skript Zeichenfolgen und Komponenten der Benutzeroberfläche. Dieses Thema liegt jedoch außerhalb des Umfangs dieses Whitepaper. Weitere Informationen finden Sie im Whitepaper Globalization Support in ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>Das Update Panel-Steuerelement

## <a name="updatepanel-control-reference"></a>UpdatePanel-Steuerelement Verweis

Markup aktivierte Eigenschaften:

| **Eigenschaftenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Gibt an, ob untergeordnete Steuerelemente die Aktualisierung beim Postback automatisch aufrufen. |
| RenderMode | Aufzählung (Block, Inline) | Gibt an, wie der Inhalt visuell dargestellt werden soll. |
| Update Mode | Aufzählung (immer, bedingt) | Gibt an, ob Update Panel bei einem partiellen Rendering immer aktualisiert wird, oder, wenn es nur aktualisiert wird, wenn ein-ausgelöst wird. |

Nur-Code-Eigenschaften:

| **Eigenschaftenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| IsInPartialRendering | bool | Ruft ab, ob Update Panel das partielle Rendering für die aktuelle Anforderung unterstützt. |
| ContentTemplate | ITemplate | Ruft die Markup Vorlage für die Aktualisierungs Anforderung ab. |
| ContentTemplateContainer | Control | Ruft die programmgesteuerte Vorlage für die Aktualisierungs Anforderung ab. |
| Trigger | UpdatePanel-TriggerCollection | Ruft die Liste der dem aktuellen Update Panel zugeordneten Trigger ab. |

Öffentliche Code Methoden:

| **Methodenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Aktualisieren () | Void | Aktualisiert das angegebene Update Panel Programm gesteuert. Ermöglicht es einer Server Anforderung, ein partielles Rendering eines anderweitig nicht ausgelösten Update Panels auszulösen. |

Markup Nachfolger:

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;ContentTemplate-&gt; | Gibt das Markup an, das zum Rendern des partiellen Renderings verwendet werden soll. Untergeordnetes Element von &lt;ASP: UpdatePanel&gt;. |
| &lt;Trigger&gt; | Gibt eine Auflistung von *n* -Steuerelementen an, die mit dem Aktualisieren dieses Update Panel verknüpft sind. Untergeordnetes Element von &lt;ASP: UpdatePanel&gt;. |
| &lt;ASP: asyncpostbackauslöser&gt; | Gibt einen-Auslöser an, der ein partielles Seiten Rendering für das angegebene Update Panel aufruft. Dabei kann es sich um ein Steuerelement als Nachfolger des fraglichen UpdatePanel handeln. Granularität für den Ereignis Namen. Untergeordnetes Element von &lt;Triggern&gt;. |
| &lt;ASP: postbackauslöser&gt; | Gibt ein Steuerelement an, das bewirkt, dass die gesamte Seite aktualisiert wird. Dabei kann es sich um ein Steuerelement als Nachfolger des fraglichen UpdatePanel handeln. Granulär für das-Objekt. Untergeordnetes Element von &lt;Triggern&gt;. |

Das `UpdatePanel` Steuerelement ist das Steuerelement, das den serverseitigen Inhalt begrenzt, der an der partiellen Renderingfunktionalität der AJAX-Erweiterungen teilnimmt. Es gibt keine Beschränkung für die Anzahl der Update Panel-Steuerelemente, die sich auf einer Seite befinden können, und Sie können in die Liste eingefügt werden. Jedes Update Panel ist isoliert, sodass jede unabhängig funktionieren kann (es können jeweils zwei Update Panel-Elemente gleichzeitig ausgeführt werden, wodurch verschiedene Teile der Seite unabhängig vom Postback der Seite gerendert werden).

Das Update Panel-Steuerelement behandelt hauptsächlich Steuerelement Trigger. Standardmäßig wird jedes Steuerelement, das in einer Update Panel-`ContentTemplate` enthalten ist, das ein Postback erstellt, als Trigger für UpdatePanel registriert. Dies bedeutet, dass Update Panel mit den standardmäßigen Daten gebundenen Steuerelementen (z. b. der GridView), mit Benutzer Steuerelementen, funktionieren und im Skript programmiert werden kann.

Wenn ein partielles Seiten Rendering ausgelöst wird, werden standardmäßig alle Update Panel-Steuerelemente auf der Seite aktualisiert, unabhängig davon, ob das Update Panel definierte Trigger für diese Aktion steuert. Wenn beispielsweise ein Update Panel ein Schaltflächen-Steuerelement definiert und auf das Schaltflächen-Steuerelement geklickt wird, werden alle Update Panel-Steuerelemente auf dieser Seite standardmäßig aktualisiert. Dies liegt daran, dass die `UpdateMode`-Eigenschaft von Update Panel standardmäßig auf `Always`festgelegt ist. Alternativ können Sie die UpdateMode-Eigenschaft auf `Conditional`festlegen. das bedeutet, dass Update Panel nur aktualisiert wird, wenn ein bestimmter-ausgelöst wird.

## <a name="custom-control-notes"></a>Benutzerdefinierte Steuerelement Notizen

Einem Benutzer Steuerelement oder einem benutzerdefinierten Steuerelement kann ein Update Panel hinzugefügt werden. die Seite, auf der diese Steuerelemente enthalten sind, muss jedoch auch ein ScriptManager-Steuerelement enthalten, bei dem die Eigenschaft EnablePartialRendering auf **true**festgelegt ist.

Eine Möglichkeit, die Sie bei der Verwendung von benutzerdefinierten websteuer Elementen berücksichtigen können, besteht darin, die geschützte `CreateChildControls()`-Methode der `CompositeControl`-Klasse zu überschreiben. Auf diese Weise können Sie ein Update Panel zwischen den untergeordneten Elementen des Steuer Elements und der Außenwelt einfügen, wenn Sie feststellen, dass die Seite partielles Rendering unterstützt. Andernfalls können Sie die untergeordneten Steuerelemente einfach in einen Container `Control`-Instanz Schichten.

## <a name="updatepanel-considerations"></a>Überlegungen zu UpdatePanel

UpdatePanel fungiert als ein schwarzes Feld, das ASP.NET-Postbacks innerhalb des Kontexts von JavaScript XMLHttpRequest umwickelt. Es gibt jedoch wichtige Überlegungen zur Leistung, die sich auf das Verhalten und die Geschwindigkeit im Hinblick auf Verhalten und Geschwindigkeit berücksichtigen. Um zu verstehen, wie Update Panel funktioniert, sollten Sie den AJAX-Austausch überprüfen, damit Sie am besten entscheiden können, wann die Anwendung geeignet ist. Im folgenden Beispiel werden eine vorhandene Website und Mozilla Firefox mit der Firebug-Erweiterung verwendet (Firebug erfasst XMLHttpRequest-Daten).

Stellen Sie sich ein Formular vor, das unter anderem über ein Postleitzahl-Textfeld verfügt, in dem ein Feld für die Stadt und das Bundesland in einem Formular oder Steuerelement aufgefüllt werden soll. In diesem Formular werden schließlich Mitgliedschafts Informationen erfasst, einschließlich des Namens, der Adresse und der Kontaktinformationen eines Benutzers. Es gibt viele Entwurfs Überlegungen, die auf der Grundlage der Anforderungen eines bestimmten Projekts zu berücksichtigen sind.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))

In der ursprünglichen Iterationen dieser Anwendung wurde ein Steuerelement erstellt, das die gesamte Benutzer Registrierungsdaten einschließlich Postleitzahl, Ort und Bundesland integriert. Das gesamte Steuerelement wurde in einem Update Panel umschließt und auf einem Webformular abgelegt. Wenn die Postleitzahl vom Benutzer eingegeben wird, erkennt Update Panel das Ereignis (das entsprechende TextChanged-Ereignis im Back-End, entweder durch Angeben von Triggern oder durch die Verwendung der Eigenschaft "Children enastriggers", die auf "true" festgelegt ist). AJAX stellt alle Felder innerhalb von Update Panel wie von Firebug aufgezeichnet dar (siehe das Diagramm auf der rechten Seite).

Wie die Bildschirmaufnahme anzeigt, werden die Werte aller Steuerelemente innerhalb von Update Panel übermittelt (in diesem Fall alle leer), ebenso wie das Feld "ViewState". Alle sagten, über 9 KB an Daten werden gesendet, wenn tatsächlich nur fünf Bytes an Daten benötigt werden, um diese spezielle Anforderung zu stellen. Die Antwort wird noch weiter in einem blobmodus aufgeführt: insgesamt werden 57KB an den Client gesendet, um einfach ein Textfeld und ein Dropdown Feld zu aktualisieren.

Außerdem ist es möglicherweise von Interesse, zu sehen, wie ASP.NET AJAX die Präsentation aktualisiert. Der Antwort Teil der Aktualisierungs Anforderung von UpdatePanel wird in der Firebug-Konsolen Anzeige auf der linken Seite angezeigt. Dabei handelt es sich um eine speziell formulierte, durch Trennzeichen getrennte Zeichenfolge, die vom Client Skript untergliedert und dann auf der Seite neu zusammengesetzt wird. ASP.NET AJAX legt die *InnerHtml* -Eigenschaft des-HTML-Elements auf dem Client fest, der das Update Panel-Element darstellt. Wenn der Browser das DOM erneut generiert, kommt es zu einer geringfügigen Verzögerung, abhängig von der Menge der zu verarbeitenden Informationen.

Die Neugenerierung des DOM löst eine Reihe zusätzlicher Probleme aus:

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))

- Wenn sich das fokussierte HTML-Element innerhalb von Update Panel befindet, verliert es den Fokus. Damit Benutzer, die die Tab-Taste gedrückt haben, das Postleitzahl-Textfeld zu schließen, wäre das nächste Ziel das Textfeld "City". Nachdem das Update Panel die Anzeige aktualisiert hat, hatte das Formular jedoch keinen Fokus mehr, und durch Drücken der Tab-Taste hätten die Fokus Elemente hervorgehoben (z. b. Links).
- Wenn ein benutzerdefiniertes Client seitiges Skript verwendet wird, das auf DOM-Elemente zugreift, können Verweise, die von Funktionen persistent gespeichert werden, nach einem partiellen Postback außer Kraft gesetzt werden.

Update Panels sind nicht als Catch-All-Lösungen gedacht. Vielmehr stellen Sie eine schnelle Lösung für bestimmte Situationen bereit, einschließlich der Prototyperstellung von kleinen Updates und bieten eine vertraute Schnittstelle für ASP.NET-Entwickler, die möglicherweise mit dem .NET-Objektmodell vertraut sind, aber weniger mit dem Dom. Es gibt eine Reihe von Alternativen, die je nach Anwendungsszenario zu einer besseren Leistung führen können:

- Die Verwendung von PageMethods und JSON (JavaScript Object Notation) ermöglicht es dem Entwickler, statische Methoden auf einer Seite aufzurufen, als ob ein Webdienst Aufruf aufgerufen wurde. Da die Methoden statisch sind, ist kein Status erforderlich. der Skript Aufrufer liefert die Parameter, und das Ergebnis wird asynchron zurückgegeben.
- Verwenden Sie ggf. einen Webdienst und JSON, wenn ein einzelnes Steuerelement an verschiedenen Stellen in einer Anwendung verwendet werden muss. Dies erfordert wieder sehr wenig spezielle Aufgaben und wird asynchron ausgeführt.

Die Einbindung von Funktionen über Webdienste oder Seiten Methoden hat ebenfalls Nachteile. ASP.NET-Entwickler neigen in der Regel dazu, kleine Komponenten der Funktionalität in Benutzer Steuerelemente (ASCX-Dateien) zu erstellen. Seiten Methoden können nicht in diesen Dateien gehostet werden. Sie müssen innerhalb der eigentlichen ASPX-Seiten Klasse gehostet werden. Auf ähnliche Weise müssen Webdienste in der ASMX-Klasse gehostet werden. Abhängig von der Anwendung kann diese Architektur gegen das Prinzip der einzelnen Verantwortung verstoßen, da die Funktionalität für eine einzelne Komponente nun auf zwei oder mehr physische Komponenten verteilt ist, die nur wenige oder keine zusammenhängenden Bindungen aufweisen können.

Wenn eine Anwendung erfordert, dass UpdatePanels verwendet werden, sollten die folgenden Richtlinien bei der Problembehandlung und Wartung unterstützt werden.

- **Schachteln Sie Update Panel so wenig wie möglich, nicht nur innerhalb von Einheiten, sondern auch über Code Einheiten hinweg.** Beispielsweise ist ein Update Panel auf einer Seite, die ein Steuerelement umschließt, während dieses Steuerelement auch ein Update Panel enthält, das ein anderes Steuerelement enthält, das ein Update Panel enthält, eine bereichsübergreifende Schachtelung. Dadurch wird klar, welche Elemente aktualisiert werden sollten, und es wird verhindert, dass untergeordnete Update Panel-Elemente unerwartet aktualisiert werden.
- **Behalten Sie für die Eigenschaft *Children enastriggers* den Wert false bei, und legen Sie auslösende Ereignisse explizit fest.** Die Verwendung der `<Triggers>` Sammlung ist eine viel klarere Methode zum Behandeln von Ereignissen und verhindert möglicherweise unerwartetes Verhalten, hilft bei Wartungs Tasks und erzwingt einem Entwickler, sich für ein Ereignis zu entscheiden.
- **Verwenden Sie die kleinstmögliche Einheit, um Funktionalität zu erreichen.** Wie bereits in der Erörterung des Postal Code-diensdienstanzdienstanzdienstanzdienstanbieter erwähnt, wird durch das Umpacken von nur die Mindestzeit auf den Server, die Gesamt Verarbeitung und den Speicherbedarf des Client-

## <a name="the-updateprogress-control"></a>Das Update Progress-Steuerelement

## <a name="updateprogress-control-reference"></a>UpdateProgress-Steuerungs Referenz

Markup aktivierte Eigenschaften:

| **Eigenschaftenname** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Zeichenfolge | Gibt die ID des Update Panel-Vorgangs an, über den dieser UpdateProgress berichten soll. |
| Display after | Int | Gibt das Timeout in Millisekunden an, bevor dieses Steuerelement angezeigt wird, nachdem die asynchrone Anforderung beginnt. |
| DynamicLayout | bool | Gibt an, ob der Status dynamisch gerendert wird. |

Markup Nachfolger:

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Enthält die Steuerelement Vorlage, die für den Inhalt festgelegt ist, der mit diesem Steuerelement angezeigt wird. |

Mit dem Update Progress-Steuerelement erhalten Sie ein Maß an Feedback, mit dem Sie die Interessen ihrer Benutzer berücksichtigen können, während Sie die erforderliche Arbeit an den Server übermitteln. Dadurch können Ihre Benutzer wissen, dass Sie etwas tun, auch wenn es nicht offensichtlich ist, vor allem, da die meisten Benutzer für die Aktualisierung der Seite verwendet werden und die Symbolleiste der Statusleiste angezeigt wird.

Beachten Sie, dass UpdateProgress-Steuerelemente an beliebiger Stelle in einer Seiten Hierarchie vorkommen können. In Fällen, in denen ein partielles Postback von einem untergeordneten Update Panel initiiert wird (wobei ein Update Panel in einem anderen Update Panel geschachtelt ist), bewirken Postbacks, die das untergeordnete Update Panel auslösen,, dass UpdateProgress-Vorlagen für das untergeordnete Element angezeigt werden. UpdatePanel und das übergeordnete Update Panel. Wenn es sich beim Auslösen jedoch um ein direktes untergeordnetes Element des übergeordneten Update Panel handelt, werden nur die UpdateProgress-Vorlagen angezeigt, die dem übergeordneten Element zugeordnet sind.

## <a name="summary"></a>Zusammenfassung

Bei den Microsoft ASP.NET AJAX-Erweiterungen handelt es sich um ausgereifte Produkte, die dazu dienen, den Zugriff auf Webinhalte zu erleichtern und eine umfassendere Benutzer Darstellung für Ihre Webanwendungen bereitzustellen. Als Teil der ASP.NET-AJAX-Erweiterungen sind die partiellen Seiten Rendering-Steuerelemente, einschließlich der ScriptManager-, Update Panel-und UpdateProgress-Steuerelemente, einige der am häufigsten sichtbaren Komponenten des Toolkits.

Die ScriptManager-Komponente integriert die Bereitstellung von Client-JavaScript für die Erweiterungen und ermöglicht die Zusammenarbeit der verschiedenen Server-und Client seitigen Komponenten mit minimalem Entwicklungsaufwand.

Das Update Panel-Steuerelement ist das offensichtliche magische Feld-Markup innerhalb von Update Panel kann serverseitiges Code Behind aufweisen und keine Seiten Aktualisierung auslösen. UpdatePanel-Steuerelemente können in einem Text eingefügt werden und können von Steuerelementen in anderen Update Panel-Elementen abhängen. Standardmäßig behandeln Update Panel alle Postbacks, die von den Nachfolger Steuerelementen aufgerufen werden, obwohl diese Funktionalität entweder deklarativ oder Programm gesteuert fein abgestimmt werden kann.

Wenn Sie das Update Panel-Steuerelement verwenden, sollten Entwickler die Auswirkungen auf die Leistung beachten, die potenziell auftreten können. Mögliche Alternativen sind Webdienste und Seiten Methoden, obwohl der Entwurf der Anwendung berücksichtigt werden sollte.

Mit dem Update Progress-Steuerelement kann der Benutzer wissen, dass er nicht ignoriert wird, und dass die Behind-the-Szenen-Anforderung ausgeführt wird, während die Seite andernfalls nichts tut, um auf die Benutzereingaben zu reagieren. Außerdem bietet Sie die Möglichkeit, partielle renderingergebnisse abzubrechen.

Zusammen bieten diese Tools Unterstützung bei der Erstellung umfassender und nahtloser Benutzer Funktionen, indem die Server Arbeit für den Benutzer weniger offensichtlich und der Workflow weniger unterbricht.

## <a name="bio"></a>Basierten

Scott Cate arbeitet seit 1997 mit Microsoft-Webtechnologien und ist der Präsident von myKB.com ([www.myKB.com](http://www.myKB.com)), wo er sich darauf spezialisiert hat, ASP.NET basierte Anwendungen zu schreiben, die sich auf die Software Lösungen der Wissensdatenbank konzentrieren. Scott kann über [scott.cate@myKB.com](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com) per e-Mail kontaktiert werden.

> [!div class="step-by-step"]
> [Weiter](understanding-asp-net-ajax-updatepanel-triggers.md)
