---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: URLs in Master Seiten (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Gibt an, wie URLs auf der Master Seite unterbrechen können, da sich die Masterseiten Datei in einem anderen relativen Verzeichnis als die Inhaltsseite befindet. Prüft die erneute Basis...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 2551a5361256234883bb37e46e794037284445a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473451"
---
# <a name="urls-in-master-pages-c"></a>URLs auf Masterseiten (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Gibt an, wie URLs auf der Master Seite unterbrechen können, da sich die Masterseiten Datei in einem anderen relativen Verzeichnis als die Inhaltsseite befindet. Untersucht URLs über ~ in der deklarativen Syntax und die programmgesteuerte Verwendung von ResolveUrl und ResolveClientUrl. (Siehe auch

## <a name="introduction"></a>Einführung

In allen bisher gefundenen Beispielen befinden sich die Master-und die Inhaltsseite im selben Ordner (Stamm Ordner der Website). Es gibt jedoch keinen Grund, warum sich die Master-und Inhaltsseiten im selben Ordner befinden müssen. Sie können Inhaltsseiten auf jeden Fall in Unterordnern erstellen. Auf ähnliche Weise können Sie einen `~/MasterPages/` Ordner erstellen, in dem Sie die Masterseiten Ihrer Site platzieren.

Ein potenzielles Problem beim Platzieren von Master-und Inhaltsseiten in verschiedenen Ordnern umfasst fehlerhafte URLs. Wenn die Master Seite relative URLs in Hyperlinks, Bildern oder anderen Elementen enthält, ist der Link für Inhaltsseiten, die sich in einem anderen Ordner befinden, ungültig. In diesem Tutorial untersuchen wir die Quelle dieses Problems sowie Problem Umgehungen.

## <a name="the-problem-with-relative-urls"></a>Das Problem mit relativen URLs

Eine URL auf einer Webseite wird als *relative URL* bezeichnet, wenn der Speicherort der Ressource, auf die Sie verweist, relativ zum Speicherort der Webseite in der Ordnerstruktur der Website ist. Jede URL, die nicht mit einem führenden Schrägstrich (`/`) oder einem Protokoll (z. b. `http://`) beginnt, ist relativ, da Sie durch den Browser auf der Grundlage des Speicher Orts der Webseite aufgelöst wird, die die URL enthält.

Unsere Website verfügt beispielsweise über einen `~/Images/` Ordner mit einer einzelnen Bilddatei, `PoweredByASPNET.gif`. Die Datei für die Masterseiten Datei `Site.master` über ein `<img>`-Element in der `footerContent` Region mit dem folgenden Markup verfügt:

[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

Der `src`-Attribut Wert im `<img>`-Element ist eine relative URL, weil er nicht mit `/` oder `http://`beginnt. Kurz gesagt: der `src` Attribut Wert weist den Browser an, im Unterordner `Images` nach einer Datei mit dem Namen `PoweredByASPNET.gif`zu suchen.

Beim Besuch einer Inhaltsseite wird das obige Markup direkt an den Browser gesendet. Nehmen Sie sich einen Moment Zeit, um `About.aspx` zu besuchen und die an den Browser gesendete HTML-Quelle anzuzeigen. Sie werden feststellen, dass genau das gleiche Markup auf der Master Seite an den Browser gesendet wurde.

[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Wenn sich die Inhaltsseite im Stamm Ordner befindet (wie `About.aspx`), funktioniert alles wie erwartet, da es einen `Images` Unterordner relativ zum Stamm Ordner gibt. Wenn sich die Inhaltsseite in einem anderen Ordner als die Master Seite befindet, werden die Dinge jedoch unterschiedlich angezeigt. Um dies zu veranschaulichen, erstellen Sie einen Unterordner mit dem Namen `Admin`. Fügen Sie als nächstes dem Ordner `Admin` eine Inhaltsseite mit dem Namen `Default.aspx` hinzu, und stellen Sie sicher, dass die neue Seite an die `Site.master` Master Seite gebunden wird.

> [!NOTE]
> Im Tutorial [*angeben des Titels, der Meta-Tags und anderer HTML-Header im Lernprogramm für die Master Seite*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) haben wir eine benutzerdefinierte Basis Seiten Klasse mit dem Namen `BasePage` erstellt, die den Titel der Inhaltsseite automatisch festlegt (sofern diese nicht explizit zugewiesen wurde). Vergessen Sie nicht, dass die Code Behind-Klasse der neu erstellten Seite von `BasePage` abgeleitet wird, damit Sie diese Funktionalität nutzen kann.

Nachdem Sie diese Inhaltsseite erstellt haben, sollte die Projektmappen-Explorer ähnlich wie in Abbildung 1 aussehen.

![Ein neuer Ordner und eine ASP.NET-Seite wurden dem Projekt hinzugefügt.](urls-in-master-pages-cs/_static/image1.png)

**Abbildung 01**: ein neuer Ordner und eine ASP.NET-Seite wurden dem Projekt hinzugefügt.

Aktualisieren Sie als nächstes die `Web.sitemap` Datei, sodass Sie einen neuen `<siteMapNode>` Eintrag für diese Lektion enthält. Der folgende XML-Code zeigt das gesamte `Web.sitemap` Markup, das nun das Hinzufügen eines dritten `<siteMapNode>` Elements einschließt.

[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Die neu erstellte `Default.aspx` Seite sollte vier Inhalts Steuerelemente aufweisen, die den vier Content-Platzhaltern in `Site.master`entsprechen. Fügen Sie dem Inhalts Steuerelement Text hinzu, der auf den `MainContent` contentplachalter verweist, und besuchen Sie dann die Seite über einen Browser. Wie in Abbildung 2 gezeigt, kann der Browser die `PoweredByASPNET.gif` Bilddatei nicht finden. Was geht da vor?

Der `~/Admin/Default.aspx` Inhaltsseite wird das gleiche HTML-Format für die `footerContent` Region gesendet wie die `About.aspx` Seite:

[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Da das `src`-Attribut des `<img>` Elements ein relative URL ist, versucht der Browser, nach einem `Images` Ordner in Relation zum Ordner Speicherort der Webseite zu suchen. Das heißt, der Browser sucht nach der Bilddatei `Admin/Images/PoweredByASPNET.gif`.

[![die poweredbyaspnet. gif-Bilddatei wurde nicht gefunden.](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Abbildung 02**: die `PoweredByASPNET.gif` Bilddatei wurde nicht gefunden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](urls-in-master-pages-cs/_static/image4.png))

### <a name="replacing-relative-urls-with-absolute-urls"></a>Ersetzen relativer URLs durch absolute URLs

Das Gegenteil einer relative URL ist ein absolute URL, bei dem es sich um einehandelt, bei der es sich um einen Schrägstrich (`/`) oder um ein Protokoll wie `http://`handelt. Da eine absolute URL den Speicherort einer Ressource von einem bekannten festgelegten Punkt aus angibt, ist dieselbe absolute URL auf jeder Webseite gültig, unabhängig von der Position der Webseite in der Ordnerstruktur der Website.

Um das in Abbildung 2 gezeigte beschädigte Bild zu beheben, müssen wir das `src` Attribut des `<img>` Elements aktualisieren, damit es eine absolute URL anstelle eines relativen Bilds verwendet. Um die richtige absolute URL zu ermitteln, besuchen Sie eine der Webseiten auf Ihrer Website, und überprüfen Sie die Adressleiste. Wie in der Adressleiste in Abbildung 2 gezeigt, wird der voll qualifizierte Pfad zur Webanwendung `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Daher können wir das `src`-Attribut des `<img>` Elements auf eine der beiden folgenden absoluten URLs aktualisieren:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Nehmen Sie sich einen Moment Zeit, um das `src`-Attribut des `<img>` Elements auf eine absolute URL zu aktualisieren, indem Sie eines der oben gezeigten Formulare verwenden, und besuchen Sie dann die `~/Admin/Default.aspx` Seite über einen Browser. Dieses Mal findet der Browser die `PoweredByASPNET.gif` Bilddatei ordnungsgemäß und zeigt Sie an (siehe Abbildung 3).

[![wird jetzt das Image "poweredbyaspnet. gif" angezeigt.](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Abbildung 03**: das `PoweredByASPNET.gif` Bild wird jetzt angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](urls-in-master-pages-cs/_static/image7.png))

Während die hart Codierung im absolute URL funktioniert, verbindet Sie Ihren HTML-Code eng mit dem Server-und Ordner Speicherort der Website, was sich ändern kann. Die Verwendung einer absolute URL der Form `http://localhost:3908/...` ist fehlerhaft, da die zuvor `localhost` Portnummer automatisch ausgewählt wird, wenn der integrierte ASP.NET Development-Webserver von Visual Studio gestartet wird. Ebenso gilt der `http://localhost` Teil nur, wenn er lokal getestet wird. Nachdem der Code auf einem Produktionsserver bereitgestellt wurde, ändert sich die URL-Basis in etwas anderes, wie z. b. `http://www.yourserver.com`. Die absolute URL in der Form `/ASPNET_MasterPages_Tutorial_04_CS/...` auch von derselben Brüchigkeit, da sich dieser Anwendungspfad häufig zwischen Entwicklungs-und Produktionsservern unterscheidet.

Die gute Nachricht ist, dass ASP.net eine Methode zum Erstellen eines gültigen relative URL zur Laufzeit bietet.

## <a name="usingandresolveclienturl"></a>Verwenden von`~`und`ResolveClientUrl`

Anstatt einen absolute URL hart zu codieren, ermöglicht ASP.NET Seiten Entwicklern die Verwendung der Tilde (`~`), um den Stamm der Webanwendung anzugeben. An früherer Stelle in diesem Tutorial haben Sie z. b. die Notation `~/Admin/Default.aspx` im Text verwendet, um auf die `Default.aspx` Seite im Ordner `Admin` zu verweisen. Der `~` gibt an, dass der Ordner `Admin` ein Unterordner des Stamms der Webanwendung ist.

Die [`ResolveClientUrl`-Methode](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) der `Control` Klasse nimmt eine URL an und ändert Sie in eine relative URL, die für die Webseite geeignet ist, auf der sich das Steuerelement befindet. Wenn Sie z. b. `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` von `About.aspx` aufrufen, wird `Images/PoweredByASPNET.gif`zurückgegeben Wenn Sie ihn aus `~/Admin/Default.aspx`aufrufen, wird jedoch `../Images/PoweredByASPNET.gif`zurückgegeben.

> [!NOTE]
> Da alle ASP.NET-Server Steuerelemente von der `Control`-Klasse abgeleitet werden, haben alle Server Steuerelemente Zugriff auf die `ResolveClientUrl`-Methode. Selbst die `Page` Klasse wird von der `Control`-Klasse abgeleitet. Dies bedeutet, dass Sie diese Methode direkt aus den Code Behind-Klassen Ihrer ASP.net Pages verwenden können.

### <a name="usingin-the-declarative-markup"></a>Verwenden von`~`im deklarativen Markup

Mehrere ASP.net-websteuer Elemente enthalten URL-bezogene Eigenschaften: das Hyperlink-Steuerelement verfügt über eine `NavigateUrl`-Eigenschaft. das Image-Steuerelement verfügt über eine `ImageUrl`-Eigenschaft. Und so weiter. Wenn Sie gerendert werden, übergeben diese Steuerelemente ihre URL-bezogenen Eigenschaftswerte an `ResolveClientUrl`. Wenn diese Eigenschaften also einen `~` enthalten, um den Stamm der Webanwendung anzugeben, wird die URL in einen gültigen relative URL geändert.

Beachten Sie, dass nur ASP.NET-Server Steuerelemente die `~` in Ihren URL-bezogenen Eigenschaften transformieren. Wenn eine `~` in statischem HTML-Markup (z. b. `<img src="~/Images/PoweredByASPNET.gif" />`) angezeigt wird, sendet die ASP.net-Engine die `~` zusammen mit dem restlichen HTML-Inhalt an den Browser. Der Browser geht davon aus, dass der `~` Teil der URL ist. Wenn der Browser z. b. die Markup `<img src="~/Images/PoweredByASPNET.gif" />` empfängt, wird davon ausgegangen, dass ein Unterordner mit dem Namen `~` mit einem Unterordner `Images` vorhanden ist, der die Bilddatei `PoweredByASPNET.gif`enthält.

Um das Bild Markup in `Site.master`zu korrigieren, ersetzen Sie das vorhandene `<img>`-Element durch ein ASP.net Image-websteuer Element. Legen Sie den `ID` des Abbilds des websteuer Elements auf `PoweredByImage`, seine `ImageUrl`-Eigenschaft auf `~/Images/PoweredByASPNET.gif`und seine `AlternateText`-Eigenschaft auf "unter ASP.net!" fest.

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Nachdem Sie diese Änderung an der Master Seite vorgenommen haben, besuchen Sie die `~/Admin/Default.aspx` Seite erneut. Dieses Mal wird die `PoweredByASPNET.gif` Bilddatei auf der Seite angezeigt (siehe Abbildung 3). Wenn das Image-websteuer Element gerendert wird, verwendet es die `ResolveClientUrl`-Methode, um seinen `ImageUrl` Eigenschafts Wert aufzulösen. In `~/Admin/Default.aspx` die `ImageUrl` in ein entsprechendes relative URL konvertiert, wie der folgende Code Ausschnitt der HTML-Quelle zeigt:

[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Zusätzlich zur Verwendung in URL-basierten websteuer Element Eigenschaften können die `~` auch beim Aufrufen der Methoden `Response.Redirect` und `Server.MapPath` verwendet werden. Außerdem kann die `ResolveClientUrl`-Methode bei Bedarf direkt aus dem deklarativen Markup einer ASP.net-oder Master Seite aufgerufen werden. Weitere Informationen finden Sie im Blogbeitrag von [Fritz Onion](https://www.pluralsight.com/blogs/fritz/) [using `ResolveClientUrl` in Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).

## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Die verbleibenden relativen URLs der Master Seite werden behoben.

Zusätzlich zum `<img>`-Element in der `footerContent`, das wir soeben korrigiert haben, enthält die Master Seite einen weiteren relative URL, der unsere Aufmerksamkeit erfordert. Die `topContent` Region enthält den Link "Master Pages-Tutorials", der auf `Default.aspx`verweist.

[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Da diese URL relativ ist, sendet Sie den Benutzer an die Seite `Default.aspx` im Ordner der Inhaltsseite, die Sie besuchen. Damit dieser Link immer auf `Default.aspx` im Stamm Ordner verweist, müssen wir das `<a>`-Element durch ein Hyperlink-websteuer Element ersetzen, damit die `~`-Notation verwendet werden kann.

Entfernen Sie das `<a>` Element Markup, und fügen Sie dort ein Hyperlink-Steuerelement hinzu. Legen Sie den `ID` des Links auf `lnkHome`, seine `NavigateUrl`-Eigenschaft auf `~/Default.aspx`und seine `Text`-Eigenschaft auf "Master Pages Tutorials" fest.

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

Das ist alles! An diesem Punkt basieren alle URLs auf unserer Master Seite ordnungsgemäß, wenn Sie von einer Inhaltsseite gerendert werden, unabhängig davon, in welchen Ordnern sich die Master Seite und die Inhaltsseite befinden.

### <a name="automatic-url-resolution-in-theheadsection"></a>Automatische URL-Auflösung im Abschnitt "`<head>`"

Im Tutorial [*Erstellen eines Website weiten Layouts mit Master Seiten*](creating-a-site-wide-layout-using-master-pages-cs.md) haben wir der `Styles.css`-Datei in der `<head>` Region eine `<link>` hinzugefügt:

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Während das `href`-Attribut des `<link>` Elements relativ ist, wird es zur Laufzeit automatisch in einen entsprechenden Pfad konvertiert. Wie im Tutorial zum [*angeben des Titels, der Meta-Tags und anderer HTML-Header in der Master Seite*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) erläutert, ist der `<head>` Bereich tatsächlich ein serverseitiges Steuerelement, mit dem der Inhalt der inneren Steuerelemente geändert werden kann, wenn er gerendert wird.

Um dies zu überprüfen, besuchen Sie die Seite `~/Admin/Default.aspx` und zeigen die an den Browser gesendete HTML-Quelle an. Wie der folgende Code Ausschnitt veranschaulicht, wurde das `href`-Attribut des `<link>` Elements automatisch in eine entsprechende relative URL geändert, `../Styles.css`.

[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Zusammenfassung

Master Seiten enthalten häufig Links, Bilder und andere externe Ressourcen, die über eine URL angegeben werden müssen. Da die Master Seite und Inhaltsseiten möglicherweise nicht im selben Ordner vorhanden sind, ist es wichtig, dass Sie keine relativen URLs verwenden. Obwohl es möglich ist, hart codierte absolute URLs zu verwenden, verbindet das absolute URL so eng mit der Webanwendung. Wenn das absolute URL geändert wird, wie es häufig beim Verschieben oder Bereitstellen einer Webanwendung der Fall ist, müssen Sie daran denken, die absoluten URLs zu aktualisieren.

Der ideale Ansatz besteht darin, die Tilde (`~`) zu verwenden, um den Anwendungs Stamm anzugeben. ASP.net-websteuer Elemente, die URL-bezogene Eigenschaften enthalten, ordnen das `~` dem Anwendungs Stamm zur Laufzeit zu. Intern verwenden die websteuer Elemente die `ResolveClientUrl`-Methode der `Control`-Klasse, um ein gültiges relative URL zu generieren. Diese Methode ist öffentlich und über jedes Server Steuerelement verfügbar (einschließlich der `Page`-Klasse), sodass Sie Sie bei Bedarf Programm gesteuert aus Ihren Code-Behind-Klassen verwenden können.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Master Seiten in ASP.net](http://www.odetocode.com/Articles/419.aspx)
- [URL-Neuzuordnung auf einer Master Seite](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Verwenden von `ResolveClientUrl` in Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [Weiter](control-id-naming-in-content-pages-cs.md)
