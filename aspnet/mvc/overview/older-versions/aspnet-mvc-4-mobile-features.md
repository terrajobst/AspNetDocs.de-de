---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: ASP.NET MVC 4 Mobile-Features | Microsoft-Dokumentation
author: Rick-Anderson
description: Es gibt jetzt eine MVC 5-Version dieses Tutorials mit Codebeispielen unter Bereitstellen einer ASP.NET MVC 5 Mobile-Webanwendung auf Azure-Websites.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 9716def069ca9f7115af32e16381f41bd4d13342
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468429"
---
# <a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4-Funktionen für mobile Geräte

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Es gibt jetzt eine MVC 5-Version dieses Tutorials mit Codebeispielen unter Bereitstellen [einer ASP.NET MVC 5 Mobile-Webanwendung auf Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)-Websites.

Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit mobilen Features in einer ASP.NET MVC 4-Webanwendung. In diesem Tutorial können Sie [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) oder Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer oder VWD&quot;) verwenden. Sie können die Professional-Version von Visual Studio verwenden, sofern diese bereits vorhanden ist.

Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (empfohlen) oder Visual Studio Web Developer Express SP1. Visual Studio 2012 enthält ASP.NET MVC 4. Wenn Sie Visual Web Developer 2010 verwenden, müssen Sie [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)installieren.

Sie werden auch einen Emulator für mobile Browser benötigen. Klicken Sie dazu auf einen der folgenden Links:

- [Windows 7 Phone-Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Dies ist der Emulator, der in den meisten Bildschirmfotos in diesem Tutorial verwendet wird.)
- Ändern Sie die Zeichenfolge des Benutzer-Agents zum Emulieren eines iPhones. Informationen finden Sie in [diesem](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) Blogeintrag.
- [Opera Mobile-Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) , wenn der Benutzer-Agent auf iPhone festgelegt ist. Anweisungen dazu, wie Sie den Benutzer-Agent in Safari auf "iPhone" festlegen, finden Sie unter So legen Sie [Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) im Blog von David.

Visual Studio-Projekte mit C#-Quellcode sind zur Ergänzung dieses Themas verfügbar:

- [Startprojekt Download](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Download des abgeschlossenen Projekts](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial fügen Sie der einfachen Konferenz zur Konferenz Auflistung, die im [Starter-Projekt](https://go.microsoft.com/fwlink/?LinkId=228307)bereitgestellt wird, Mobile Features hinzu. Der folgende Screenshot zeigt die Seite Tags der abgeschlossenen Anwendung, wie im [Windows 7 Phone-Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)zu sehen ist. Informationen zur Vereinfachung der Tastatureingabe finden Sie unter [Tastatur Zuordnung für Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) .

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Sie können Internet Explorer Version 9 oder 10, Firefox oder Chrome verwenden, um Ihre Mobile Anwendung zu entwickeln, indem Sie die [Benutzer-Agent-Zeichenfolge](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)festlegen. In der folgenden Abbildung wird das abgeschlossene Tutorial zum emuinieren eines iPhones mithilfe von Internet Explorer veranschaulicht. Sie können die Internet Explorer F-12-Entwicklertools und das [Tool "Tools](http://www.fiddler2.com/fiddler2/) " verwenden, um Ihre Anwendung zu debuggen.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Erlernte Fertigkeiten

Folgendes können Sie lernen:

- Wie die ASP.NET MVC 4-Vorlagen das HTML5-`viewport` Attribut und das adaptive Rendering verwenden, um die Anzeige auf mobilen Geräten zu verbessern.
- Erstellen von mobilen Ansichten.
- Erstellen eines Ansichts umschalchers, mit dem Benutzer zwischen einer mobilen Ansicht und einer Desktop Ansicht der Anwendung umschalten können.

### <a name="getting-started"></a>Erste Schritte

Laden Sie die Konferenz Auflistungs Anwendung für das Starter-Projekt mithilfe des folgenden Links herunter: [herunterladen](https://go.microsoft.com/fwlink/?LinkId=228307). Klicken Sie dann in Windows Explorer mit der rechten Maustaste auf die Datei *mvcmobile. zip* , und wählen Sie **Eigenschaften**aus. Wählen Sie im Dialogfeld **mvcmobile. zip-Eigenschaften** die Schaltfläche **Sperre entsperren** aus. (Durch diese Option wird eine Sicherheitswarnung verhindert, die angezeigt wird, wenn Sie versuchen, eine *.zip* -Datei zu verwenden, die Sie aus dem Internet heruntergeladen haben.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Klicken Sie mit der rechten Maustaste auf die Datei *mvcmobile. zip* , und wählen Sie **Alle extrahieren** , um die Datei zu entpacken. Öffnen Sie in Visual Studio die Datei " *mvcmobile. sln* ".

Drücken Sie STRG + F5, um die Anwendung auszuführen, die im Desktop Browser angezeigt wird. Starten Sie den Emulator für Mobile Browser, kopieren Sie die URL für die Konferenz Anwendung in den Emulator, und klicken Sie dann auf den Link **nach Tag durchsuchen** . Wenn Sie den Windows Phone Emulator verwenden, klicken Sie auf die URL-Leiste, und drücken Sie die Pause-Taste, um den Tastatur Zugriff zu erhalten. Die folgende Abbildung zeigt die Ansicht *AllTags* (aus Auswahl **von durchsuchen nach Tag**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Die Anzeige ist auf einem mobilen Gerät sehr gut lesbar. Wählen Sie den Link ASP.net aus.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Die ASP.net-tagansicht ist sehr überlastet. Beispielsweise ist die Spalte **Date** sehr schwer lesbar. Später in diesem Tutorial erstellen Sie eine Version der *AllTags* Ansicht, die speziell für Mobile Browser verwendet wird und die die Anzeige lesbar macht.

Hinweis: Zurzeit ist ein Fehler in der Mobile Caching-Engine vorhanden. Für Produktionsanwendungen müssen Sie das Fixed-Paket " [DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) " installieren. Ausführliche Informationen zur Behebung finden Sie unter [ASP.NET MVC 4 Mobile Caching Bug Fixed](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

## <a name="css-media-queries"></a>CSS-Medien Abfragen

[CSS-Medien Abfragen](http://www.w3.org/TR/css3-mediaqueries/) sind eine Erweiterung von CSS für Medientypen. Sie ermöglichen es Ihnen, Regeln zu erstellen, die die standardmäßigen CSS-Regeln für bestimmte Browser (Benutzer-Agents) überschreiben. Eine gängige Regel für CSS, die auf Mobile Browser abzielt, ist die Definition der maximalen Bildschirmgröße. Die Datei " *content\site.CSS* ", die beim Erstellen eines neuen ASP.NET MVC 4-Internet Projekts erstellt wird, enthält die folgende Medien Abfrage:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Wenn das Browserfenster 850 Pixel breit oder kleiner ist, werden die CSS-Regeln in diesem medienblock verwendet. Sie können CSS-Medien Abfragen wie diesen verwenden, um die Anzeige von HTML-Inhalten in kleinen Browsern (z. b. mobilen Browsern) besser zu ermöglichen als die standardmäßigen CSS-Regeln, die für die breiteren Anzeige von Desktop Browsern entwickelt wurden.

## <a name="the-viewport-meta-tag"></a>Das Viewport-Meta-Tag

Die meisten mobilen Browser definieren eine virtuelle Browserfenster Breite ( *Viewport*), die wesentlich größer ist als die tatsächliche Breite des mobilen Geräts. Auf diese Weise können Mobile Browser die gesamte Webseite innerhalb der virtuellen Anzeige anpassen. Benutzer können dann interessante Inhalte vergrößern. Wenn Sie jedoch die Breite des Viewports auf die tatsächliche Gerätebreite festlegen, ist kein Zoomen erforderlich, da der Inhalt in den mobilen Browser passt.

Das Viewport-`<meta>`-Tag in der ASP.NET MVC 4-Layoutdatei legt den Viewport auf die Gerätebreite fest. In der folgenden Zeile wird der Viewport `<meta>`-Tags in der ASP.NET MVC 4-Layoutdatei angezeigt.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Untersuchen der Auswirkung von CSS-Medien Abfragen und des Viewport-Meta-Tags

Öffnen Sie die Datei *views\shared\\_Layout. cshtml* im Editor, und kommentieren Sie den Viewport-`<meta>` Tag aus. Das folgende Markup zeigt die Auskommentierung der Zeile.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Öffnen Sie im Editor die Datei " *mvcmobile\content\site.CSS* ", und ändern Sie die maximale Breite in der Medien Abfrage in NULL Pixel. Dadurch wird verhindert, dass die CSS-Regeln in mobilen Browsern verwendet werden. In der folgenden Zeile wird die geänderte Medien Abfrage angezeigt:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Speichern Sie die Änderungen, und navigieren Sie in einem Emulator für Mobile Browser zur Konferenz Anwendung. Der kleine Text in der folgenden Abbildung ist das Ergebnis des Entfernens des Viewports `<meta>`-Tags. Wenn kein Viewport `<meta>` Kennung angezeigt wird, wird der Browser auf die Standardbreite des Viewports vergrößert (bei den meisten mobilen Browsern 850 Pixel oder breiter).

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Rückgängigmachen der Änderungen – heben Sie die Auskommentierung des Viewports `<meta>`-Tags in der Layoutdatei auf 850, und stellen Sie die Medien Abfrage in der Datei " *Site. CSS* " Speichern Sie die Änderungen, und aktualisieren Sie den mobilen Browser, um zu überprüfen, ob die Anzeige mit Mobilgeräten wieder hergestellt wurde.

Der Viewport `<meta>` Tag und die CSS-Medien Abfrage sind nicht spezifisch für ASP.NET MVC 4, und Sie können diese Features in jeder Webanwendung nutzen. Sie sind jetzt jedoch in die Dateien integriert, die generiert werden, wenn Sie ein neues ASP.NET MVC 4-Projekt erstellen.

Weitere Informationen zum Viewport `<meta>`-Tag finden Sie [in der Geschichte zweier Viewports – Teil 2](http://www.quirksmode.org/mobile/viewports2.html).

Im nächsten Abschnitt erfahren Sie, wie Sie Ansichten speziell für mobile Browser bereitstellen.

## <a name="overriding-views-layouts-and-partial-views"></a>Überschreiben von Sichten, Layouts und Teilansichten

Ein wichtiges neues Feature in ASP.NET MVC 4 ist ein einfacher Mechanismus, mit dem Sie jede beliebige Ansicht (einschließlich Layouts und Teilansichten) für Mobile Browser im Allgemeinen, für einen einzelnen mobilen Browser oder für einen beliebigen Browser außer Kraft setzen können. Um eine mobiltaugliche Ansicht zu erstellen, können Sie eine Ansichtsdatei kopieren und *.Mobile* zum Dateinamen hinzufügen. Wenn Sie z. b. eine Mobile *Index* Ansicht erstellen möchten, kopieren Sie *views\home\index.cshtml* in *Views\Home\Index.Mobile.cshtml*.

In diesem Abschnitt erstellen Sie eine Layoutdatei speziell für mobile Zwecke.

Kopieren Sie zunächst *views\shared\\_Layout. cshtml* in *views\shared\\_Layout. Mobile. cshtml*. Öffnen Sie *\_Layout. Mobile. cshtml* , und ändern Sie den Titel von **MVC4 Conference** in **Conference (Mobile)** .

Entfernen Sie in jedem `Html.ActionLink` aufrufen in jedem Link *Action Link*"Browse by". Der folgende Code zeigt den abgeschlossenen Textabschnitt der mobilen Layoutdatei.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopieren Sie die Datei *views\home\alltags.cshtml* nach *Views\Home\AllTags.Mobile.cshtml*. Öffnen Sie die neue Datei, und ändern Sie das `<h2>` -Element von "Tags" in "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Navigieren Sie in einem Desktopbrowser und über einen Emulator für mobile Browser zur Tags-Seite. Der Emulator für Mobile Browser zeigt die beiden von Ihnen vorgenommenen Änderungen an.

[![p2m_layoutTags. Mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Im Gegensatz dazu hat sich die Desktop Anzeige nicht geändert.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Browser spezifische Ansichten

Zusätzlich zu den Ansichten speziell für Mobiltelefone und den Desktop können Sie Ansichten für individuelle Browser erstellen. Beispielsweise können Sie Ansichten erstellen, die speziell für den iPhone-Browser gelten. In diesem Abschnitt erstellen Sie ein Layout für den iPhone-Browser und eine iPhone-Version der *AllTags* -Ansicht.

Öffnen Sie die Datei *Global. asax* , und fügen Sie der `Application_Start`-Methode den folgenden Code hinzu.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Durch diesen Code wird der neue Anzeigemodus "iPhone" definiert, der mit jeder eingehenden Anforderung abgeglichen wird. Wenn die eingehende Anforderung die definierte Bedingung erfüllt (der Benutzer-Agent also die Zeichenfolge "iPhone" enthält), sucht ASP.NET MVC nach Ansichten, die das Suffix "iPhone" im Namen enthalten.

Klicken Sie im Code mit der rechten Maustaste auf `DefaultDisplayMode`, und wählen Sie **Auflösen** und dann `using System.Web.WebPages;` aus. Dadurch wird ein Verweis auf den `System.Web.WebPages`-Namespace hinzugefügt, in dem die Typen `DisplayModes` und `DefaultDisplayMode` definiert sind.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternativ können Sie dem Abschnitt `using` der Datei die folgende Zeile manuell hinzufügen.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Der gesamte Inhalt der Datei " *Global. asax* " ist unten dargestellt.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Speichern Sie die Änderungen. Kopieren Sie die Datei *mvcmobile\views\shared\\_Layout. Mobile. cshtml* in *mvcmobile\views\shared\\_Layout. iPhone. cshtml*. Öffnen Sie die neue Datei, und ändern Sie dann die `h1` Überschrift von `Conference (Mobile)` in `Conference (iPhone)`.

Kopieren Sie die Datei *MvcMobile\Views\Home\AllTags.Mobile.cshtml* in *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Ändern Sie in der neuen Datei das `<h2>` -Element von "Tags (M)" in "Tags (iPhone)".

Führen Sie die Anwendung aus. Führen Sie einen Emulator für mobile Browser aus, stellen Sie sicher, dass für den zugehörigen Benutzer-Agent die Option "iPhone" festgelegt ist, und navigieren Sie zur Ansicht *AllTags* . Der folgende Screenshot zeigt die Ansicht *AllTags* , die im [Safari](http://www.apple.com/safari/download/) -Browser gerendert wird. Sie können Safari für Windows [hier](https://support.apple.com/kb/DL1531)herunterladen.

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

In diesem Abschnitt haben wir gesehen, wie man mobile Layouts und Ansichten erstellt sowie wie man das für spezifische Geräte wie das iPhone tut. Im nächsten Abschnitt erfahren Sie, wie Sie jQuery Mobile für überzeugendere Mobile Ansichten nutzen.

## <a name="using-jquery-mobile"></a>Verwenden von jQuery Mobile

Die [Mobile jQuery](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) -Bibliothek bietet ein Benutzeroberflächen Framework, das auf allen wichtigen mobilen Browsern funktioniert. jQuery Mobile wendet *Progressive Verb esse* rungen auf Mobile Browser an, die CSS und JavaScript unterstützen. Die Progressive Erweiterung ermöglicht allen Browsern, den grundlegenden Inhalt einer Webseite anzuzeigen, und ermöglicht gleichzeitig leistungsfähigere Browser und Geräte mit einer umfassenderen Anzeige. Die JavaScript-und CSS-Dateien, die im jQuery Mobile-Stil enthalten sind, sind viele Elemente für Mobile Browser, ohne Markup Änderungen vornehmen zu müssen.

In diesem Abschnitt Installieren Sie das nuget-Paket " *jQuery. Mobile. MVC* ", mit dem jQuery Mobile und ein Ansichts switcherwidget installiert werden.

Löschen Sie zunächst die frei *gegebenen\\_Layout. Mobile. cshtml* und *Shared\\_Layout. iPhone. cshtml* -Dateien, die Sie zuvor erstellt haben.

Benennen Sie *Views\Home\AllTags.Mobile.cshtml* -und *Views\Home\AllTags.iPhone.cshtml* -Dateien in *Views\Home\AllTags.iPhone.cshtml.Hide* und *Views\Home\AllTags.Mobile.cshtml.Hide*um. Da die Dateien nicht mehr über die Erweiterung " *. cshtml* " verfügen, werden Sie von der ASP.NET-MVC-Laufzeit nicht zum renderingder *AllTags* Ansicht verwendet.

Installieren Sie das nuget-Paket " *jQuery. Mobile. MVC* ", indem Sie Folgendes ausführen:

1. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. Geben Sie in der **Paket-Manager-Konsole**`Install-Package jQuery.Mobile.MVC -version 1.0.0`

Die folgende Abbildung zeigt die Dateien, die vom nuget-Paket jQuery. Mobile. MVC hinzugefügt und dem mvcmobile-Projekt hinzugefügt wurden. Dateien, die hinzugefügt werden, haben [Add], angehängt nach dem Dateinamen. Das Bild zeigt keine GIF-und PNG-Dateien an, die dem Ordner " *content\images* " hinzugefügt wurden.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Mit dem nuget-Paket "jQuery. Mobile. MVC" wird Folgendes installiert:

- Die *App\_start\bundlemobileconfig.cs* -Datei, die benötigt wird, um auf die hinzugefügten jQuery-JavaScript-und CSS-Dateien zu verweisen. Befolgen Sie die Anweisungen unten, und verweisen Sie auf das in dieser Datei definierte Mobile Paket.
- jQuery Mobile CSS-Dateien.
- Ein `ViewSwitcher` Controller-Widget (*controllers\viewswitchercontroller.cs*).
- jQuery Mobile JavaScript-Dateien.
- Eine jQuery-Layoutdatei im mobilen Stil (*views\shared\\_Layout. Mobile. cshtml*).
- Eine Ansichts Switcher-Teilansicht *(mvcmobile\views\shared\\_ViewSwitcher. cshtml*), die einen Link am oberen Rand jeder Seite bereitstellt, um von der Desktop Ansicht zur mobilen Ansicht zu wechseln und umgekehrt.
- Mehrere<em>PNG</em> -und <em>GIF</em> -Bilddateien im Ordner " <em>content\images</em> ".

Öffnen Sie die Datei *Global. asax* , und fügen Sie den folgenden Code in die letzte Zeile der `Application_Start`-Methode ein.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Der folgende Code zeigt die komplette Datei " *Global. asax* ".

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Wenn Sie Internet Explorer 9 verwenden und die `BundleMobileConfig` Zeile oberhalb der gelben Markierung nicht angezeigt wird, klicken Sie auf das Schaltflächen Bild [Kompatibilitäts Ansicht](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![der Schaltfläche Kompatibilitäts Ansicht (aus)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Bild der Schaltfläche "Kompatibilitäts Ansicht" (aus)") in IE, um das Symbol von einem ![Gliederungs Bild](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Bild der Schaltfläche "Kompatibilitäts Ansicht" (aus)") der Schaltfläche Kompatibilitäts Ansicht (aus) in ein voll Tonbild der Schaltfläche ![Kompatibilitäts Ansicht (ein)](https://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Bild der Schaltfläche "Kompatibilitäts Ansicht" (on)")zu ändern. Alternativ können Sie dieses Tutorial in Firefox oder Chrome anzeigen.

Öffnen Sie die Datei *mvcmobile\views\shared\\_Layout. Mobile. cshtml* , und fügen Sie das folgende Markup direkt nach dem `Html.Partial`-Befehl ein:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Die vollständige Datei " *mvcmobile\views\shared\\_Layout. Mobile. cshtml* " ist unten dargestellt:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Erstellen Sie die Anwendung, und navigieren Sie in Ihrem Emulator für Mobile Browser zur Ansicht *AllTags* . Sie sehen Folgendes:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Sie können den mobilen spezifischen Code Debuggen, indem Sie [die Benutzer-Agent-Zeichenfolge](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) für IE oder Chrome auf iPhone festlegen und dann die F-12-Entwicklertools verwenden. Wenn in Ihrem mobilen Browser die Links **Start**, **Speaker**, **Tag**und **Date** nicht als Schaltflächen angezeigt werden, sind die Verweise auf Mobile jQuery-Skripts und CSS-Dateien wahrscheinlich nicht richtig.

Zusätzlich zu den Formatänderungen finden Sie die **Anzeige mobiler Ansicht** und einen Link, über den Sie von der mobilen Ansicht zur Desktop Ansicht wechseln können. Wählen Sie den Link **Desktop Ansicht** aus, und die Desktop Ansicht wird angezeigt.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Die Desktop Ansicht bietet keine Möglichkeit, direkt zur mobilen Ansicht zurückzukehren. Korrigieren Sie dies jetzt. Öffnen Sie die Datei *views\shared\\_Layout. cshtml* . Fügen Sie direkt unterhalb der Seite `body` Element den folgenden Code hinzu, der das Ansichts switcherwidget rendert:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aktualisieren Sie die Ansicht *AllTags* im mobilen Browser. Sie können jetzt zwischen Desktop-und mobilen Ansichten navigieren.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Debughinweis: Sie können den folgenden Code am Ende der Datei views\shared\\_ViewSwitcher. cshtml hinzufügen, um das Debuggen von Ansichten zu unterstützen, wenn die Benutzer-Agent-Zeichenfolge auf ein mobiles Gerät festgelegt ist.
>
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
>
> und fügen Sie der Datei *views\shared\\_Layout. cshtml* die folgende Überschrift hinzu.
>
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]

Navigieren Sie in einem Desktop Browser zur Seite " *AllTags* ". Das Ansichts Switcher-Widget wird nicht in einem Desktop Browser angezeigt, da es nur zur Seite mobiler Layout hinzugefügt wird. Später in diesem Tutorial erfahren Sie, wie Sie das Ansichts-switcherwidget zur Desktop Ansicht hinzufügen können.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Verbessern der Speakers-Liste

Wählen Sie im mobilen Browser den Link **Speakers** aus. Da keine Mobile Ansicht (*allspeakers. Mobile. cshtml*) vorhanden ist, wird die Standard Anzeige (*allspeakers. cshtml*) mithilfe der mobilen Layoutansicht ( *\_Layout. Mobile. cshtml*) gerendert.

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Sie können eine standardmäßige (nicht Mobile) Ansicht in einem mobilen Layout global deaktivieren, indem Sie `RequireConsistentDisplayMode` auf `true` in den *Ansichten\\_ViewStart. cshtml* -Datei festlegen, wie im folgenden Beispiel:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Wenn `RequireConsistentDisplayMode` auf `true`festgelegt ist, wird das Mobile Layout (<em>\_Layout. Mobile. cshtml</em>) nur für mobile Ansichten verwendet. (Das heißt, die Ansichts Datei hat die Form <em>* * viewName</em><em>. Mobile. cshtml</em>.) Sie können `RequireConsistentDisplayMode` auf `true` festlegen, wenn Ihr mobiles Layout nicht mit ihren nicht mobilen Ansichten gut funktioniert. Der folgende Screenshot zeigt, wie die Seite <em>Referenten</em> gerendert wird, wenn `RequireConsistentDisplayMode` auf `true`festgelegt ist.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Sie können den konsistenten Anzeigemodus in einer Ansicht deaktivieren, indem Sie `RequireConsistentDisplayMode` auf `false` in der Ansichts Datei festlegen. Das folgende Markup in der Datei " *views\home\allspeakers.cshtml* " legt `RequireConsistentDisplayMode` auf `false`fest:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Erstellen einer Ansicht für Mobile Sprecher

Wie Sie gerade gesehen haben, ist die Ansicht *Speakers* gut lesbar, aber die Links sind klein und können auf mobilen Geräten schlecht angetippt werden. In diesem Abschnitt erstellen Sie eine Ansicht für Mobile-spezifische *Referenten* , die wie eine moderne Mobile Anwendung aussieht – Sie zeigt große, leicht zu entzurufenden Links an und enthält ein Suchfeld, in dem Sie schnell Referenten finden können.

Kopieren Sie *allspeakers. cshtml* in *allspeakers. Mobile. cshtml*. Öffnen Sie die Datei *allspeakers. Mobile. cshtml* , und entfernen Sie das Element `<h2>` Überschrift.

Fügen Sie im `<ul>`-Tag das `data-role`-Attribut hinzu, und legen Sie dessen Wert auf `listview`fest. Wie bei anderen [`data-*` Attributen](http://html5doctor.com/html5-custom-data-attributes/)ist `data-role="listview"` das Tippen der großen Listenelemente leichter. Das fertige Markup sieht wie folgt aus:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aktualisieren Sie den mobilen Browser. Die aktualisierte Ansicht sieht wie folgt aus:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Obwohl die mobile Ansicht verbessert wurde, ist es schwierig, in der langen Liste der Redner zu navigieren. Um dieses Problem zu beheben, fügen Sie im `<ul>`-Tag das `data-filter`-Attribut hinzu, und legen Sie es auf `true`fest. Der folgende Code zeigt das `ul` Markup.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

In der folgenden Abbildung wird das Feld Suchfilter oben auf der Seite angezeigt, das sich aus dem `data-filter`-Attribut ergibt.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Wenn Sie jeden Buchstaben in das Suchfeld eingeben, filtert jQuery Mobile die angezeigte Liste, wie in der folgenden Abbildung dargestellt.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Verbessern der Tags-Liste

Wie in der Standardansicht " *Sprecher* " ist die *tagansicht lesbar* , aber die Links sind klein und schwer zu tippen auf einem mobilen Gerät. In diesem Abschnitt korrigieren Sie die *Tags* -Ansicht auf die gleiche Weise, wie Sie die Ansicht " *Sprecher* " korrigiert haben.

Entfernen Sie das &quot;&quot; Suffix in der Datei *Views\Home\AllTags.Mobile.cshtml.Hide* , sodass der Name *Views\Home\AllTags.Mobile.cshtml*lautet. Öffnen Sie die umbenannte Datei, und entfernen Sie das `<h2>` Element.

Fügen Sie die Attribute "`data-role`" und "`data-filter`" dem `<ul>`-Tag hinzu, wie hier gezeigt:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Die folgende Abbildung zeigt die Seiten Filterung der Tags für den Buchstaben `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Verbessern der Datums Liste

Sie können die Ansicht " *dates* " so verbessern, wie Sie die Ansichten " *Sprecher* " und " *Tags* " verbessert haben, damit Sie auf einem mobilen Gerät einfacher zu verwenden sind.

Kopieren Sie die Datei *views\home\alldates.cshtml* nach *Views\Home\AllDates.Mobile.cshtml*. Öffnen Sie die neue Datei, und entfernen Sie das `<h2>` Element.

Fügen Sie dem `<ul>`-Tag `data-role="listview"` wie folgt hinzu:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

In der folgenden Abbildung wird gezeigt, wie die **Datums** Seite mit dem `data-role`-Attribut aussieht.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Ersetzen Sie den Inhalt der Datei *Views\Home\AllDates.Mobile.cshtml* durch den folgenden Code:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Dieser Code gruppiert alle Sitzungen nach Tagen. Er erstellt einen Listen Teiler für jeden neuen Tag und listet alle Sitzungen für jeden Tag unter einem unter Teiler auf. Dies sieht wie folgt aus, wenn dieser Code ausgeführt wird:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Verbessern der sessionstable-Ansicht

In diesem Abschnitt erstellen Sie eine Mobile-spezifische Ansicht von Sitzungen. Die Änderungen, die wir vornehmen, sind umfangreicher als in anderen von uns erstellten Ansichten.

Tippen Sie im mobilen Browser auf die Schaltfläche **Redner** , und geben Sie dann `Sc` in das Suchfeld ein.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Tippen Sie auf den Link **Scott Hanselman** .

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Wie Sie sehen können, ist es schwierig, die Anzeige in einem mobilen Browser zu lesen. Die Spalte Date ist schwer lesbar, und die Spalte Tags befindet sich außerhalb der Ansicht. Um dieses Problem zu beheben, kopieren Sie *views\home\sessionstable.cshtml* in *Views\Home\SessionsTable.Mobile.cshtml*, und ersetzen Sie dann den Inhalt der Datei durch den folgenden Code:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Der Code entfernt die Spalten "Room" und "Tags" und formatiert Titel, Sprecher und Datum vertikal, sodass alle diese Informationen in einem mobilen Browser lesbar sind. Das Bild unten zeigt die Codeänderungen.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Verbessern der sessionbycode-Ansicht

Schließlich erstellen Sie eine Mobile-spezifische Ansicht der *sessionbycode* -Sicht. Tippen Sie im mobilen Browser auf die Schaltfläche **Redner** , und geben Sie dann `Sc` in das Suchfeld ein.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Tippen Sie auf den Link **Scott Hanselman** . Die Sitzungen von Scott Hanselman werden angezeigt.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Wählen Sie **eine Übersicht über den Link "MS Web Stack of Love" aus** .

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Die Standard Desktop Ansicht ist in Ordnung, Sie können Sie jedoch verbessern.

Kopieren Sie die Datei *views\home\sessionbycode.cshtml* in *Views\Home\SessionByCode.Mobile.cshtml* , und ersetzen Sie den Inhalt der Datei *Views\Home\SessionByCode.Mobile.cshtml* durch das folgende Markup:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Das neue Markup verwendet das `data-role`-Attribut, um das Layout der Ansicht zu verbessern.

Aktualisieren Sie den mobilen Browser. Das folgende Bild zeigt die Codeänderungen, die Sie gerade vorgenommen haben:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup und Review

In diesem Tutorial wurden die neuen mobilen Features von ASP.NET MVC 4 Developer Preview vorgestellt. Zu den mobilen Features gehören:

- Die Möglichkeit, Layout, Ansichten und partielle Ansichten sowohl global als auch für eine einzelne Ansicht zu überschreiben.
- Kontrolle über Layout und partielle Überschreibungs Erzwingung mithilfe der `RequireConsistentDisplayMode`-Eigenschaft.
- Ein Ansichts switcherwidget für mobile Ansichten, das auch in Desktop Ansichten angezeigt werden kann.
- Unterstützung für bestimmte Browser, wie z. b. den iPhone-Browser.

## <a name="see-also"></a>Weitere Informationen

- [Mobile jQuery](http://jquerymobile.com) -Website.
- [Übersicht über jQuery Mobile](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Von W3C empfohlene bewährte Methoden für mobile Webanwendungen](http://www.w3.org/TR/mwabp/)
- [W3C-Empfehlungskandidaten für Medienabfragen](http://www.w3.org/TR/css3-mediaqueries/)
