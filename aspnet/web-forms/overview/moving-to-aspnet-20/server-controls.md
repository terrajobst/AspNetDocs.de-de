---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Server Steuerelemente | Microsoft-Dokumentation
author: microsoft
description: ASP.NET 2,0 verbessert Server Steuerelemente in vielerlei Hinsicht. In diesem Modul werden einige der Architektur Änderungen auf die Art und Weise behandelt ASP.NET 2,0 und Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: c02a633013f061c09141d4f98871848c011a799e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521091"
---
# <a name="server-controls"></a>Serversteuerelemente

von [Microsoft](https://github.com/microsoft)

> ASP.NET 2,0 verbessert Server Steuerelemente in vielerlei Hinsicht. In diesem Modul behandeln wir einige der Architektur Änderungen auf die Art und Weise, in der ASP.NET 2,0 und Visual Studio 2005 mit Server Steuerelementen umgeht.

ASP.NET 2,0 verbessert Server Steuerelemente in vielerlei Hinsicht. In diesem Modul behandeln wir einige der Architektur Änderungen auf die Art und Weise, in der ASP.NET 2,0 und Visual Studio 2005 mit Server Steuerelementen umgeht.

## <a name="view-state"></a>Ansichts Zustand

Die primäre Änderung im Ansichts Zustand in ASP.NET 2,0 ist eine drastische Verringerung der Größe. Stellen Sie sich eine Seite vor, für die nur ein Kalender Steuerelement gilt. Dies ist der Ansichts Zustand in ASP.NET 1,1.

[!code-css[Main](server-controls/samples/sample1.css)]

Hier sehen Sie den Ansichts Zustand auf einer identischen Seite in ASP.NET 2,0.

[!code-css[Main](server-controls/samples/sample2.css)]

Das ist eine ziemlich bedeutende Änderung, und es wird geprüft, ob der Ansichts Zustand über das Netzwerk hin-und hergeleitet wird. diese Änderung kann Entwicklern eine beträchtliche Leistungssteigerung geben. Die Verringerung der Größe des Ansichts Zustands ist größtenteils auf die Art und Weise, in der wir Sie intern behandeln. Beachten Sie, dass View State eine Base64-codierte Zeichenfolge ist. Um die Änderung des Ansichts Zustands in ASP.NET 2,0 besser zu verstehen, sehen Sie sich die decodierten Werte aus den obigen Beispielen an.

Hier ist der 1,1-Ansichts Zustand decodiert:

[!code-css[Main](server-controls/samples/sample3.css)]

Dies kann etwas wie giberish aussehen, aber hier ist ein Muster vorhanden. In ASP.NET 1. x haben wir einzelne Zeichen verwendet, um Datentypen und durch Trennzeichen getrennte Werte mithilfe der &lt;&gt; Zeichen zu identifizieren. Das "t" im obigen Ansichts Zustands Beispiel stellt ein Dreier dar. Das Triplet enthält ein paar von ArrayLists ("l" stellt eine ArrayList dar). Eine dieser ArrayLists enthält eine Int32 ("i") mit einem Wert von 1 und die andere eine andere. Das Triplet enthält ein paar von ArrayLists usw. Wichtig zu beachten ist, dass wir Triplets verwenden, die Paare enthalten, die Datentypen über einen Buchstaben identifiziert werden und die &lt; und &gt; Zeichen als Trennzeichen verwendet werden.

In ASP.NET 2,0 sieht der decodierte Ansichts Zustand etwas anders aus.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Sie sollten eine enorme Änderung der Darstellung des decodierten Ansichts Zustands feststellen. Diese Änderung hat mehrere Architektur-Grundlagen. Der Ansichts Zustand in ASP.NET 1. x verwendet den LosFormatter zum Serialisieren von Daten. In 2,0 verwenden wir die neue ObjectStateFormatter-Klasse. Diese Klasse wurde speziell entwickelt, um die Serialisierung und Deserialisierung des Ansichts Zustands und des Steuerungs Zustands zu unterstützen. (Der Steuerungs Zustand wird im nächsten Abschnitt behandelt.) Durch Ändern der Methode, mit der Serialisierung und Deserialisierung durchgeführt werden, werden viele Vorteile erzielt. Eine der dramatischsten ist die Tatsache, dass im Gegensatz zu dem losformatierer, der einen TextWriter verwendet, der ObjectStateFormatter einen BinaryWriter verwendet. Dadurch kann ASP.NET 2,0 den Ansichts Zustand einer Reihe von Bytes anstelle von Zeichen folgen speichern. Nehmen Sie z. b. eine ganze Zahl. In ASP.NET 1,1 benötigte eine ganze Zahl 4 Bytes des Ansichts Zustands. In ASP.NET 2,0 ist für dieselbe ganze Zahl nur 1 Byte erforderlich. Es wurden weitere Verbesserungen vorgenommen, um die Menge des gespeicherten Ansichts Zustands zu verringern. DateTime-Werte werden z. b. jetzt mit einem TickCount anstelle einer Zeichenfolge gespeichert.

Wenn all dies nicht ausreichend war, wurde die Tatsache berücksichtigt, dass einer der größten Consumer des Ansichts Zustands in 1. x das DataGrid und ähnliche Steuerelemente war. Ein wichtiger Nachteil von Steuerelementen, wie z. b. dem DataGrid, wo der Ansichts Zustand liegt, besteht darin, dass Sie häufig große Mengen an wiederholten Informationen In ASP.NET 1. x wurden diese wiederholten Informationen einfach immer wieder gespeichert, was zu einem sehr groß-Ansichts Zustand führte. In ASP.NET 2,0 wird die neue IndexedString-Klasse verwendet, um solche Daten zu speichern. Wenn eine Zeichenfolge wiederholt wird, speichern wir einfach das Token für IndexedString und den Index in einer laufenden Tabelle von IndexedString-Objekten.

## <a name="control-state"></a>Steuerelement Zustand

Einer der wichtigsten Grundlagen, den Entwickler mit dem Ansichts Zustand hatten, war die Größe, die der http-Nutzlast hinzugefügt wurde. Wie bereits erwähnt, ist einer der größten Consumer des Ansichts Zustands das DataGrid-Steuerelement. Um den großen Ansichts Zustand zu vermeiden, der von einem DataGrid generiert wird, haben viele Entwickler einfach den Ansichts Zustand für dieses Steuerelement deaktiviert. Leider war diese Lösung nicht immer eine gute Lösung. Der Ansichts Zustand in ASP.NET 1. x enthält nicht nur die Daten, die für die korrekte Funktion des Steuer Elements erforderlich sind. Es enthält auch Informationen zum Status der Benutzeroberfläche des Steuer Elements. Dies bedeutet Folgendes: Wenn Sie die Paginierung für ein DataGrid zulassen möchten, müssen Sie den Ansichts Zustand aktivieren, auch wenn Sie nicht alle Benutzeroberflächen Informationen benötigen, die der Ansichts Zustand enthält. Es handelt sich um ein all-oder Nothing-Szenario.

In ASP.NET 2,0 löst der Steuerungs Zustand dieses Problem durch die Einführung des Steuerelement Zustands hervorragend. Der Steuerelement Zustand enthält die Daten, die für die ordnungsgemäße Funktionalität eines Steuer Elements absolut notwendig sind. Im Gegensatz zum Ansichts Zustand kann der Steuerelement Zustand nicht deaktiviert werden. Daher ist es wichtig, dass die Daten, die im Steuerelement Zustand gespeichert werden, sorgfältig gesteuert werden.

> [!NOTE]
> Der Steuerelement Zustand wird zusammen mit dem Ansichts Zustand im \_\_Feld "ausgeblendetes Formular" angezeigt.

Dieses Video ist eine exemplarische Vorgehensweise des Ansichts Zustands und des Steuerungs Zustands.

![](server-controls/_static/image1.png)

[Vollbildvideos öffnen](server-controls/_static/state1.wmv)

Damit ein Server Steuerelement den Zustand des Steuer Elements lesen und schreiben kann, müssen Sie drei Schritte ausführen.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Schritt 1: Aufrufen der registerrequirements scontrolstate-Methode

Die registerrequirements scontrolstate-Methode informiert ASP.net, dass ein Steuerelement den Steuerelement Zustand beibehalten muss. Es wird ein Argument des Typs "Control" benötigt, das das Steuerelement ist, das registriert wird.

Beachten Sie, dass die Registrierung nicht von Anforderung zu Anforderung persistent gespeichert wird. Daher muss diese Methode für jede Anforderung aufgerufen werden, wenn ein Steuerelement den Steuerelement Zustand beibehalten soll. Es wird empfohlen, dass die-Methode in OnInit aufgerufen wird.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Schritt 2: Überschreiben von SaveControlState

Die SaveControlState-Methode speichert Änderungen am Steuerelement Zustand für ein Steuerelement seit dem letzten Postback. Es gibt ein Objekt zurück, das den Zustand des Steuer Elements darstellt.

## <a name="step-3-override-loadcontrolstate"></a>Schritt 3: Überschreiben von LoadControlState

Die LoadControlState-Methode lädt den gespeicherten Zustand in ein-Steuerelement. Die Methode nimmt ein Argument vom Typ "Object" an, das den gespeicherten Zustand für das Steuerelement enthält.

## <a name="full-xhtml-compliance"></a>Vollständige XHTML-Konformität

Jeder Webentwickler kennt die Wichtigkeit von Standards in Webanwendungen. Um eine Standard basierte Entwicklungsumgebung beizubehalten, ist ASP.NET 2,0 vollständig XHTML-kompatibel. Daher werden alle Tags gemäß XHTML-Standards in Browsern gerendert, die HTML 4,0 oder höher unterstützen.

Die DOCTYPE-Definition in ASP.NET 1,1 lautete wie folgt:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2,0 lautet die DOCTYPE-Standard Definition wie folgt:

[!code-html[Main](server-controls/samples/sample7.html)]

Wenn Sie auswählen, können Sie die Standardkonformität von XHTML über den Knoten xhtmlConformance in der Konfigurationsdatei ändern. Beispielsweise ändert der folgende Knoten in der Datei "Web. config" die XHTML-Konformität in XHTML 1,0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Wenn Sie auswählen, können Sie ASP.net auch so konfigurieren, dass die in ASP.NET 1. x verwendete Legacy Konfiguration wie folgt verwendet wird:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptive Rendering mithilfe von Adaptern

In ASP.NET 1. x enthielt die Konfigurationsdatei einen &lt;browserCaps-&gt; Abschnitt, der ein httpbrowserfunktionalitäten-Objekt auffüllt. Mit diesem Objekt kann ein Entwickler ermitteln, welches Gerät eine bestimmte Anforderung sendet und Code entsprechend Rendering. In ASP.NET 2,0 wurde das Modell verbessert und verwendet jetzt die neue ControlAdapter-Klasse. Die ControlAdapter-Klasse überschreibt Ereignisse im Lebenszyklus des-Steuer Elements und steuert das Rendering von Steuerelementen basierend auf den Funktionen des Benutzer-Agents. Die Funktionen eines bestimmten Benutzer-Agents werden durch eine Browser Definitionsdatei (eine Datei mit der Dateierweiterung ". Browser") definiert, die im Verzeichnis "c:\WINDOWS\Microsoft.NET\Framework\v2.0." gespeichert ist.\*\*\*\*Ordner \config\browser.

> [!NOTE]
> Die ControlAdapter-Klasse ist eine abstrakte Klasse.

Ähnlich wie der &lt;browserCaps&gt; Abschnitt in 1. x wird in der Browser Definitionsdatei ein regulärer Ausdruck verwendet, um die Benutzer-Agent-Zeichenfolge zu analysieren, um den anfordernden Browser zu identifizieren. Dabei werden bestimmte Funktionen für diesen Benutzer-Agent definiert. Der ControlAdapter rendert das Steuerelement über die Rendermethode. Wenn Sie also die "Rendering"-Methode überschreiben, sollten Sie "Rendering" nicht für die Basisklasse aufzurufen. Dies kann dazu führen, dass das Rendering zweimal, einmal für den Adapter und einmal für das Steuerelement selbst auftritt.

## <a name="developing-a-custom-adapter"></a>Entwickeln eines benutzerdefinierten Adapters

Sie können einen eigenen benutzerdefinierten Adapter entwickeln, indem Sie von ControlAdapter erben. Außerdem können Sie von der abstrakten Klasse PageAdapter erben, wenn ein Adapter für eine Seite benötigt wird. Die Zuordnung von Steuerelementen zu Ihrem benutzerdefinierten Adapter wird über das &lt;controlAdapters-&gt; Element in der Browser Definitionsdatei erreicht. Der folgende XML-Code aus einer Browser Definitionsdatei ordnet z. b. das Menü Steuerelement der MenuAdapter-Klasse zu:

[!code-html[Main](server-controls/samples/sample10.html)]

Mit diesem Modell ist es für einen Steuerungs Entwickler recht einfach, ein bestimmtes Gerät oder einen anderen Browser als Ziel zu verwenden. Es ist auch recht einfach für einen Entwickler, die gesamte Kontrolle darüber zu haben, wie Seiten auf jedem Gerät dargestellt werden.

## <a name="per-device-rendering"></a>Rendering pro Gerät

Server Steuerungseigenschaften in ASP.NET 2,0 können pro Gerät mithilfe eines browserspezifischen Präfix angegeben werden. Der folgende Code ändert z. b. den Text einer Bezeichnung, je nachdem, welches Gerät zum Durchsuchen der Seite verwendet wird.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Wenn die Seite, die diese Bezeichnung enthält, von Internet Explorer aus durchsucht wird, wird in der Bezeichnung der folgende Text angezeigt: "Sie durchsuchen Internet Explorer". Wenn die Seite von Firefox aus durchsucht wird, wird in der Bezeichnung der Text "Sie werden von Firefox aus" angezeigt. Wenn die Seite von einem anderen Gerät aus durchsucht wird, wird angezeigt, dass Sie auf einem unbekannten Gerät suchen. Jede Eigenschaft kann mit dieser speziellen Syntax angegeben werden.

## <a name="setting-focus"></a>Festlegen des Fokus

ASP.NET 1. x-Entwickler haben häufig gefragt, wie der anfängliche Fokus auf ein bestimmtes Steuerelement festgelegt wird. Auf einer Anmeldeseite ist es beispielsweise hilfreich, wenn das Textfeld Benutzer-ID den Fokus erhält, wenn die Seite zum ersten Mal geladen wird. In ASP.NET 1. x erforderte das Schreiben eines Client seitigen Skripts. Obwohl es sich bei einem solchen Skript um eine triviale Aufgabe handelt, ist es in ASP.NET 2,0 Dank der SetFocus-Methode nicht mehr erforderlich. Die SetFocus-Methode nimmt ein Argument an, das das Steuerelement angibt, das den Fokus erhalten soll. Dieses Argument kann entweder die Client-ID des Steuer Elements als Zeichenfolge oder der Name des Server Steuer Elements als Steuerelement Objekt sein. Um z. b. den anfänglichen Fokus auf ein TextBox-Steuerelement mit dem Namen "txtuserid" festzulegen, wenn die Seite zum ersten Mal geladen wird, fügen Sie den folgenden Code zu Seiten\_

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--oder

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2,0 verwendet den WebResource. axd-Handler (zuvor erläutert), um eine Client seitige Funktion zu erzeugen, die den Fokus festlegt. Der Name der Client seitigen Funktion ist Webform\_"Autofocus", wie hier gezeigt:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternativ können Sie die Focus-Methode für ein-Steuerelement verwenden, um den anfänglichen Fokus auf dieses Steuerelement festzulegen. Die Focus-Methode wird von der Control-Klasse abgeleitet und ist für alle ASP.NET 2,0-Steuerelemente verfügbar. Es ist auch möglich, den Fokus auf ein bestimmtes Steuerelement festzulegen, wenn ein Validierungs Fehler auftritt. Dies wird in einem späteren Modul behandelt.

## <a name="new-server-controls-in-aspnet-20"></a>Neue Server Steuerelemente in ASP.NET 2,0

Im folgenden finden Sie neue Server Steuerelemente in ASP.NET 2,0. Wir werden einige davon in späteren Modulen ausführlicher erläutern.

## <a name="imagemap-control"></a>ImageMap-Steuerelement

Das ImageMap-Steuerelement ermöglicht Ihnen das Hinzufügen von Hotspots zu einem Bild, das ein Postback initiieren oder zu einer URL navigieren kann. Es sind drei Arten von Hotspots verfügbar. CircleHotSpot, rechglehotspot und PolygonHotSpot. Hotspots werden über einen Auflistungs-Editor in Visual Studio oder Programm gesteuert im Code hinzugefügt. Es ist keine Benutzeroberfläche zum Zeichnen von Hotspots auf einem Image verfügbar. Die Koordinaten und die Größe oder der Radius des Hotspots müssen deklarativ angegeben werden. Es gibt auch keine visuelle Darstellung eines Hotspots im Designer. Wenn ein Hotspot für die Navigation zu einer URL konfiguriert ist, wird die URL über die NavigateUrl-Eigenschaft des Hotspots angegeben. Bei einem Post Back Hotspot ermöglicht Ihnen die PostBackValue-Eigenschaft, eine Zeichenfolge im Postback zu übergeben, die im serverseitigen Code abgerufen werden kann.

![Hotspot-Auflistungs-Editor in Visual Studio](server-controls/_static/image1.jpg)

**Abbildung 1**: Hotspot-Auflistungs-Editor in Visual Studio

## <a name="bulletedlist-control"></a>BulletedList-Steuerelement

Das Steuerelement "BulletedList" ist eine Auflistungs Liste, die problemlos Daten gebunden werden kann. Die Liste kann mithilfe der Eigenschaft "BulletStyle" (nummeriert) oder ungeordnet werden. Jedes Element in der Liste wird durch ein ListItem-Objekt dargestellt.

![Steuerelement "BulletedList" in Visual Studio](server-controls/_static/image1.gif)

**Abbildung 2**: Steuerelement "BulletedList" in Visual Studio

## <a name="hiddenfield-control"></a>HiddenField-Steuerelement

Das HiddenField-Steuerelement fügt der Seite ein ausgeblendetes Formularfeld hinzu, dessen Wert im serverseitigen Code verfügbar ist. Der Wert eines ausgeblendeten Formular Felds bleibt in der Regel zwischen den Postbacks unverändert. Es ist jedoch möglich, dass ein böswilliger Benutzer den Wert vor dem Postback ändern kann. Wenn dies der Fall ist, wird das "ValueChanged"-Ereignis vom HiddenField-Steuerelement ausgelöst. Wenn Sie vertrauliche Informationen im HiddenField-Steuerelement haben und sicherstellen möchten, dass es unverändert bleibt, sollten Sie das ValueChanged-Ereignis im Code behandeln.

## <a name="fileupload-control"></a>FileUpload-Steuerelement

Das FileUpload-Steuerelement in ASP.NET 2,0 ermöglicht das Hochladen von Dateien auf einen Webserver über eine ASP.NET-Seite. Dieses Steuerelement ähnelt der HtmlInputFile-Klasse ASP.NET 1. x mit wenigen Ausnahmen. In ASP.NET 1. x wurde empfohlen, die PostedFile-Eigenschaft auf NULL zu prüfen, um zu ermitteln, ob Sie eine gute Datei hatten. Das FileUpload-Steuerelement in ASP.NET 2,0 fügt eine neue HasFile-Eigenschaft hinzu, die Sie für denselben Zweck verwenden können, und es ist etwas effizienter.

Die PostedFile-Eigenschaft ist weiterhin für den Zugriff auf ein HttpPostedFile-Objekt verfügbar, aber ein Teil der Funktionalität von HttpPostedFile ist nun intrinsisch mit dem FileUpload-Steuerelement verfügbar. Um z. b. eine hochgeladene Datei in ASP.NET 1. x zu speichern, wird die SaveAs-Methode für das HttpPostedFile-Objekt aufgerufen. Wenn Sie das FileUpload-Steuerelement in ASP.NET 2,0 verwenden, würden Sie die SaveAs-Methode im FileUpload-Steuerelement selbst aufzurufen.

Eine weitere bedeutende Änderung im Verhalten von 2,0 (und wahrscheinlich die signifikanteste Änderung) besteht darin, dass es nicht mehr erforderlich ist, eine vollständige hochgeladene Datei in den Arbeitsspeicher zu laden, bevor Sie gespeichert wird. In 1. x wird jede hochgeladene Datei vollständig im Arbeitsspeicher gespeichert, bevor Sie auf den Datenträger geschrieben wird. Diese Architektur verhindert, dass große Dateien hochgeladen werden.

In ASP.NET 2,0 können Sie mit dem requestverländiskthreshold-Attribut des httpRuntime-Elements konfigurieren, wie viele Kilobytes in einem Puffer im Arbeitsspeicher aufbewahrt werden, bevor Sie auf den Datenträger geschrieben werden.

**Wichtig**: in der MSDN-Dokumentation (und in der Dokumentation an anderer Stelle) wird angegeben, dass dieser Wert in Bytes (nicht Kilobytes) liegt und der Standardwert 256 ist. Der Wert wird tatsächlich in Kilobyte angegeben, und der Standardwert ist 80. Wenn Sie den Standardwert 80K haben, stellen wir sicher, dass der Puffer nicht auf dem großen Objekt Heap endet.

## <a name="wizard-control"></a>Assistenten-Steuerelement

Es kommt häufig vor, dass ASP.NET-Entwickler versuchen, Informationen in einer Reihe von "Seiten" mithilfe von Bereichen oder über die Übertragung von einer Seite an eine Seite zu sammeln. Meistens ist das Bestreben eine frustrierende Angelegenheit und zeitaufwändig. Das neue Assistenten-Steuerelement löst die Probleme, indem es lineare und nichtlineare Schritte in einer Assistenten Schnittstelle zulässt, mit denen Benutzer vertraut sind. Das Assistenten-Steuerelement stellt Eingabeformulare in einer Reihe von Schritten dar. Jeder Schritt ist von einem bestimmten Typ, der von der StepType-Eigenschaft des-Steuer Elements angegeben wird. Folgende Schritt Typen sind verfügbar:

| **Schritttyp** | **Erklärung** |
| --- | --- |
| Auto | Der Assistent bestimmt automatisch den Typ des Schritts basierend auf seiner Position innerhalb der Schritt Hierarchie. |
| Start | Der erste Schritt, der häufig verwendet wird, um eine Einführungs Anweisung darzustellen. |
| Schritt | Ein normaler Schritt. |
| Finish | Der letzte Schritt, der normalerweise verwendet wird, um eine Schaltfläche zum Beenden des Assistenten zu präsentieren. |
| Abgeschlossen | Zeigt eine Nachricht an, die Erfolg oder Fehler kommuniziert. |

> [!NOTE]
> Das Assistenten-Steuerelement verfolgt seinen Zustand mithilfe des ASP.NET-Steuerelement Zustands. Daher kann die EnableViewState-Eigenschaft ohne Beeinträchtigung auf false festgelegt werden.

Dieses Video ist eine exemplarische Vorgehensweise des Assistenten-Steuer Elements.

![](server-controls/_static/image2.png)

[Vollbildvideos öffnen](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Lokalisieren von Steuerelementen

Das Localize-Steuerelement ähnelt einem Literalsteuerelement. Das Localize-Steuerelement verfügt jedoch über eine **Mode** -Eigenschaft, mit der gesteuert wird, wie das hinzugefügte Markup gerendert wird. Die Mode-Eigenschaft unterstützt die folgenden Werte:

| **Mode** | **Erklärung** |
| --- | --- |
| Transformieren | Markup wird gemäß dem Protokoll des Browsers transformiert, von dem die Anforderung stammt. |
| PassThrough | Markup wird unverändert gerendert. |
| Codieren | Markup, das dem Steuerelement hinzugefügt wird, wird mit HtmlEncode codiert. |

## <a name="multiview-and-view-controls"></a>MultiView-und View-Steuerelemente

Das MultiView-Steuerelement fungiert als Container für Sicht Steuerelemente, und das View-Steuerelement fungiert als Container (ähnlich wie ein Panel-Steuerelement) für andere Steuerelemente. Jede Ansicht in einem MultiView-Steuerelement wird durch ein einzelnes Ansichts Steuerelement dargestellt. Das erste Ansichts Steuerelement in der MultiView ist die Ansicht 0, die zweite Sicht 1 usw. Sie können Ansichten umschalten, indem Sie den ActiveViewIndex des MultiView-Steuer Elements angeben.

## <a name="substitution-control"></a>Ersetzung-Steuerelement

Das Steuerelement Ersetzung wird in Verbindung mit ASP.NET Caching verwendet. In Fällen, in denen Sie die Zwischenspeicherung nutzen möchten, aber Teile einer Seite haben, die bei jeder Anforderung aktualisiert werden müssen (anders ausgedrückt: Teile einer Seite, die von der Zwischenspeicherung ausgenommen sind), stellt die Ersetzungs Komponente eine gute Lösung dar. Das-Steuerelement rendert tatsächlich keine Ausgabe. Stattdessen wird Sie an eine Methode im serverseitigen Code gebunden. Wenn die Seite angefordert wird, wird die-Methode aufgerufen, und das zurückgegebene Markup wird anstelle des Ersetzung-Steuer Elements gerendert.

Die Methode, an die das Ersetzung-Steuerelement gebunden ist, wird über die **MethodName** -Eigenschaft angegeben. Diese Methode muss die folgenden Kriterien erfüllen:

- Dabei muss es sich um eine statische Methode (Shared in VB) handeln.
- Er akzeptiert einen Parameter vom Typ "HttpContext".
- Sie gibt eine Zeichenfolge zurück, die das Markup darstellt, das das Steuerelement auf der Seite ersetzen soll.

Das Ersetzungs Steuerelement ist nicht in der Lage, ein anderes Steuerelement auf der Seite zu ändern. es hat jedoch Zugriff auf den aktuellen HttpContext über den Parameter.

## <a name="gridview-control"></a>GridView-Steuerelement

Das GridView-Steuerelement ist der Ersatz für das DataGrid-Steuerelement. Dieses Steuerelement wird in einem späteren Modul ausführlicher behandelt.

## <a name="detailsview-control"></a>DetailsView-Steuerelement

Das DetailsView-Steuerelement ermöglicht es Ihnen, einen einzelnen Datensatz aus einer Datenquelle anzuzeigen und ihn zu bearbeiten oder zu löschen. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="formview-control"></a>FormView-Steuerelement

Das FormView-Steuerelement wird zum Anzeigen eines einzelnen Datensatzes aus einer Datenquelle in einer konfigurierbaren-Schnittstelle verwendet. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="accessdatasource-control"></a>AccessDataSource-Steuerelement

Das AccessDataSource-Steuerelement wird für die Datenbindung einer Access-Datenbank verwendet. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="objectdatasource-control"></a>ObjectDataSource-Steuerelement

Das ObjectDataSource-Steuerelement wird verwendet, um eine Architektur mit drei Ebenen zu unterstützen, sodass Steuerelemente an ein Geschäftsobjekt der mittleren Ebene gebunden werden können, im Gegensatz zu einem zweistufigen Modell, bei dem Steuerelemente direkt an die Datenquelle gebunden werden. Dies wird in einem späteren Modul ausführlicher erläutert.

## <a name="xmldatasource-control"></a>XmlDataSource-Steuerelement

Das XmlDataSource-Steuerelement wird für die Datenbindung an eine XML-Datenquelle verwendet. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource-Steuerelement

Das SiteMapDataSource-Steuerelement stellt Daten Bindungen für Site Navigations Steuerelemente basierend auf einer Site Übersicht bereit. Dies wird in einem späteren Modul ausführlicher erläutert.

## <a name="sitemappath-control"></a>SiteMapPath-Steuerelement

Das SiteMapPath-Steuerelement zeigt eine Reihe von Navigations Verknüpfungen an, die häufig als Brotkrümel bezeichnet werden. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="menu-control"></a>Menu-Steuerelement

Das Menü Steuerelement zeigt dynamische Menüs mithilfe von DHTML an. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="treeview-control"></a>TreeView-Steuerelement

Das TreeView-Steuerelement wird verwendet, um eine hierarchische Strukturansicht der Daten anzuzeigen. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="login-control"></a>Login-Steuerelement

Das-Anmelde Steuerelement stellt einen Mechanismus zum Anmelden bei einer Website bereit. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="loginview-control"></a>LoginView-Steuerelement

Das LoginView-Steuerelement ermöglicht die Anzeige verschiedener Vorlagen basierend auf dem Anmeldestatus eines Benutzers. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="passwordrecovery-control"></a>PasswordRecovery-Steuerelement

Das PasswordRecovery-Steuerelement wird zum Abrufen von vergessenen Kenn Wörtern durch Benutzer einer ASP.NET-Anwendung verwendet. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="loginstatus"></a>LoginStatus

Das LoginStatus-Steuerelement zeigt den Anmeldestatus eines Benutzers an. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="loginname"></a>LoginName

Das LoginName-Steuerelement zeigt den Benutzernamen eines Benutzers nach der Anmeldung bei einer ASP.NET-Anwendung an. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="createuserwizard"></a>CreateUserWizard

Der-Assistent ist ein konfigurierbarer Assistent, mit dem Benutzer ein ASP.net-Mitgliedschafts Konto für die Verwendung in einer ASP.NET-Anwendung erstellen können. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="changepassword"></a>ChangePassword

Das ChangePassword-Steuerelement ermöglicht es Benutzern, Ihr Kennwort für eine ASP.NET-Anwendung zu ändern. Sie wird in einem späteren Modul ausführlicher behandelt.

## <a name="various-webparts"></a>Verschiedene Webparts

ASP.NET 2,0 wird mit verschiedenen Webparts ausgeliefert. Diese werden in einem späteren Modul ausführlich behandelt.
