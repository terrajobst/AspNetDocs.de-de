---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Angeben des Titels, der Meta-Tags und anderer HTML-Header auf der Master Seite (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Prüft verschiedene Techniken zum Definieren von sortierten &lt;Head-&gt; Elementen auf der Master Seite auf der Seite Inhalt.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 160af664cdf27f9ede1273aaf915da749a39ad48
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637729"
---
# <a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Angeben des Titels, der META-Tags und anderer HTML-Header auf der Masterseite (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Prüft verschiedene Techniken zum Definieren von sortierten &lt;Head-&gt; Elementen auf der Master Seite auf der Seite Inhalt.

## <a name="introduction"></a>Einführung

Neue Masterseiten, die in Visual Studio 2008 erstellt wurden, verfügen standardmäßig über zwei contentplachalter-Steuerelemente: eine mit dem Namen "`head`", die sich im `<head>` Element befindet. und eine mit dem Namen `ContentPlaceHolder1`, die innerhalb des Webformulars platziert wird. Der Zweck `ContentPlaceHolder1` besteht darin, einen Bereich im Webformular zu definieren, der seitenweise angepasst werden kann. Der `head` contentplachalter ermöglicht es Seiten, dem Abschnitt `<head>` benutzerdefinierten Inhalt hinzuzufügen. (Selbstverständlich können diese beiden contentplatzhalter geändert oder entfernt werden, und der Master Seite kann zusätzlicher contentplachalter hinzugefügt werden. Unsere Master Seite, `Site.master`, verfügt zurzeit über vier contentplachalter-Steuerelemente.)

Das HTML-`<head>`-Element dient als Repository für Informationen über das Webseiten Dokument, das nicht Teil des Dokuments selbst ist. Dies umfasst Informationen wie den Titel der Webseite, Meta-Informationen, die von Suchmaschinen oder internen Crawlern verwendet werden, sowie Links zu externen Ressourcen wie RSS-Feeds, JavaScript und CSS-Dateien. Einige dieser Informationen sind möglicherweise für alle Seiten auf der Website relevant. Beispielsweise möchten Sie möglicherweise die gleichen CSS-Regeln und JavaScript-Dateien für jede ASP.NET-Seite Global importieren. Es gibt jedoch Teile des `<head>` Elements, die Seiten spezifisch sind. Der Seitentitel ist ein gutes Beispiel.

In diesem Tutorial wird erläutert, wie Sie globale und Seiten spezifische `<head>` Abschnitts Markup auf der Master Seite und in den zugehörigen Inhaltsseiten definieren.

## <a name="examining-the-master-pagesheadsection"></a>Überprüfen des`<head>`Abschnitts der Master Seite

Die von Visual Studio 2008 erstellte standardmäßige Masterseiten Datei enthält im `<head>` Abschnitt das folgende Markup:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Beachten Sie, dass das `<head>`-Element ein `runat="server"`-Attribut enthält, das angibt, dass es sich um ein Server Steuerelement (anstelle von statischem HTML) handelt. Alle ASP.NET-Seiten werden von der [`Page`-Klasse](https://msdn.microsoft.com/library/system.web.ui.page.aspx)abgeleitet, die sich im `System.Web.UI`-Namespace befindet. Diese Klasse enthält eine [`Header`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) , die Zugriff auf den `<head>` Bereich der Seite bietet. Mithilfe der `Header`-Eigenschaft können wir den Titel einer ASP.NET-Seite festlegen oder dem gerenderten `<head>` Abschnitt zusätzliches Markup hinzufügen. Anschließend können Sie das `<head>` Element einer Inhaltsseite anpassen, indem Sie im `Page_Load`-Ereignishandler der Seite etwas Code schreiben. Wir untersuchen, wie der Titel der Seite in Schritt 1 Programm gesteuert festgelegt wird.

Das im obigen `<head>`-Element gezeigte Markup enthält auch ein contentplachalter-Steuerelement mit dem Namen `head`. Dieses contentplachalter-Steuerelement ist nicht erforderlich, da Inhaltsseiten dem `<head>` Element Programm gesteuert benutzerdefinierten Inhalt hinzufügen können. Dies ist jedoch in Situationen nützlich, in denen eine Inhaltsseite dem `<head>`-Element statisches Markup hinzufügen muss, da das statische Markup deklarativ dem entsprechenden Inhalts Steuerelement und nicht Programm gesteuert hinzugefügt werden kann.

Zusätzlich zum `<title>`-Element und `head` contentplachalter sollte das `<head>`-Element der Master Seite alle Markup auf `<head>`Ebene enthalten, das allen Seiten gemeinsam ist. Auf unserer Website verwenden alle Seiten die CSS-Regeln, die in der `Styles.css`-Datei definiert sind. Folglich haben wir das `<head>`-Element im Tutorial [*Erstellen eines Website weiten Layouts mit Master Seiten*](creating-a-site-wide-layout-using-master-pages-vb.md) aktualisiert, um ein entsprechendes `<link>`-Element einzuschließen. Das aktuelle `<head>` Markup der `Site.master` Master Seite ist unten dargestellt.

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Schritt 1: Festlegen des Titels einer Inhaltsseite

Der Titel der Webseite wird über das `<title>`-Element angegeben. Es ist wichtig, den Titel jeder Seite auf einen passenden Wert festzulegen. Beim Besuch einer Seite wird der Titel in der Titelleiste des Browsers angezeigt. Außerdem verwenden Browser beim Lesezeichen einer Seite den Titel der Seite als empfohlenen Namen für das Lesezeichen. Außerdem zeigen viele Suchmaschinen beim Anzeigen von Suchergebnissen den Titel der Seite an.

> [!NOTE]
> Standardmäßig legt Visual Studio das `<title>`-Element auf der Master Seite auf "unbenannte Seite" fest. Auf ähnliche Weise haben auch neue ASP.NET-Seiten den `<title>` auf "unbenannte Seite" festgelegt. Da es leicht vergessen werden kann, den Titel der Seite auf einen passenden Wert festzulegen, gibt es viele Seiten im Internet mit dem Titel "unbenannte Seite". Durchsuchen von Google nach Webseiten mit diesem Titel werden ungefähr 2.460.000 Ergebnisse zurückgegeben. Auch Microsoft ist anfällig für das Veröffentlichen von Webseiten mit dem Titel "unbenannte Seite". Zum Zeitpunkt der Erstellung dieses Artikels hat eine Google Search 236 solche Webseiten in der Microsoft.com-Domäne gemeldet.

Eine ASP.NET-Seite kann Ihren Titel auf eine der folgenden Arten angeben:

- Indem der Wert direkt innerhalb des `<title>` Elements platziert wird.
- Verwenden des `Title`-Attributs in der `<%@ Page %>`-Direktive
- Programm gesteuertes Festlegen der `Title`-Eigenschaft der Seite mithilfe von Code wie `Page.Title="title"` oder `Page.Header.Title="title"`.

Inhaltsseiten haben kein `<title>`-Element, da es auf der Master Seite definiert ist. Daher können Sie zum Festlegen des Titels einer Inhaltsseite entweder das `Title`-Attribut der `<%@ Page %>` Direktive verwenden oder es Programm gesteuert festlegen.

### <a name="setting-the-pages-title-declaratively"></a>Deklaratives Festlegen des Titels der Seite

Der Titel einer Inhaltsseite kann mithilfe des `Title`-Attributs der [`<%@ Page %>` Direktive](https://msdn.microsoft.com/library/ydy4x04a.aspx)deklarativ festgelegt werden. Diese Eigenschaft kann festgelegt werden, indem Sie die `<%@ Page %>`-Direktive oder die Eigenschaftenfenster direkt ändern. Sehen wir uns beide Ansätze an.

Suchen Sie in der Quell Ansicht nach der `<%@ Page %>`-Direktive, die sich am oberen Rand des deklarativen Markups der Seite befindet. Die `<%@ Page %>`-Direktive für `Default.aspx` folgt:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

Die `<%@ Page %>`-Direktive gibt Seiten spezifische Attribute an, die von der ASP.net-Engine beim Auswerten und Kompilieren der Seite verwendet werden. Dies schließt neben anderen Informationen auch die Masterseiten Datei, den Speicherort der zugehörigen Codedatei und ihren Titel ein.

Standardmäßig legt Visual Studio beim Erstellen einer neuen Inhaltsseite das `Title`-Attribut auf "unbenannte Seite" fest. Ändern Sie das `Title` Attribut `Default.aspx`von "unbenannte Seite" in "Master Seiten-Tutorials", und zeigen Sie die Seite dann über einen Browser an. Abbildung 1 zeigt die Titelleiste des Browsers, die den Titel der neuen Seite widerspiegelt.

![Die Titelleiste des Browsers zeigt nun &quot;Master Seiten-Lernprogramme&quot; anstelle &quot;unbenannten Seite&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Abbildung 01**: die Titelleiste des Browsers zeigt nun "Master Seiten-Lernprogramme" anstelle von "unbenannte Seite" an

Der Titel der Seite kann auch aus dem Eigenschaftenfenster festgelegt werden. Wählen Sie aus der Eigenschaftenfenster in der Dropdown Liste die Option Dokument aus, um die Eigenschaften auf Seitenebene zu laden. dazu gehört auch die Eigenschaft `Title`. Abbildung 2 zeigt die Eigenschaftenfenster, nachdem `Title` auf "Lernprogramme für Master Seiten" festgelegt wurde.

![Sie können den Titel auch über das Eigenschaften Fenster konfigurieren.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Abbildung 02**: Sie können den Titel auch über das Eigenschaften Fenster konfigurieren.

### <a name="setting-the-pages-title-programmatically"></a>Programm gesteuertes Festlegen des Seitentitels

Das `<head runat="server">` Markup der Master Seite wird in eine [`HtmlHead` Klassen](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) Instanz übersetzt, wenn die Seite von der ASP.net-Engine gerendert wird. Die `HtmlHead`-Klasse verfügt über eine [`Title`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) , deren Wert im gerenderten `<title>`-Element widergespiegelt wird. Auf diese Eigenschaft kann über `Page.Header.Title`aus der Code Behind-Klasse einer ASP.net page zugegriffen werden. auf dieselbe Eigenschaft kann auch über `Page.Title`zugegriffen werden.

Wenn Sie den Titel der Seite Programm gesteuert festlegen möchten, navigieren Sie zur Code Behind-Klasse der `About.aspx` Seite, und erstellen Sie einen Ereignishandler für das `Load` Ereignis der Seite. Legen Sie als nächstes den Titel der Seite auf "Master Page Tutorials:: about:: *Date*" fest, wobei *Datum* das aktuelle Datum ist. Nachdem Sie diesen Code hinzugefügt haben, sollte der `Page_Load` Ereignishandler in etwa wie folgt aussehen:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Abbildung 3 zeigt die Titelleiste des Browsers beim Besuch der Seite "`About.aspx`".

![Der Titel der Seite wird Programm gesteuert festgelegt und enthält das aktuelle Datum.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Abbildung 03**: der Titel der Seite wird Programm gesteuert festgelegt und enthält das aktuelle Datum.

## <a name="step-2-automatically-assigning-a-page-title"></a>Schritt 2: Automatisches Zuweisen eines Seitentitels

Wie in Schritt 1 gesehen, kann der Titel einer Seite deklarativ oder Programm gesteuert festgelegt werden. Wenn Sie vergessen, den Titel explizit in einen aussagekräftigeren Namen zu ändern, erhält die Seite den Standard Titel "unbenannte Seite". Im Idealfall wird der Titel der Seite automatisch festgelegt, wenn der Wert nicht explizit angegeben wird. Wenn z. b. zur Laufzeit der Titel der Seite "unbenannte Seite" ist, kann es vorkommen, dass der Titel automatisch aktualisiert wird, damit er mit dem Dateinamen der ASP.NET-Seite identisch ist. Die gute Nachricht ist, dass der Titel mit etwas vorgelagertem Arbeitsaufwand automatisch zugewiesen werden kann.

Alle ASP.NET-Webseiten werden von der `Page`-Klasse im System. Web. UI-Namespace abgeleitet. Die `Page`-Klasse definiert die minimale Funktionalität, die für eine ASP.NET-Seite benötigt wird, und macht neben vielen anderen wesentliche Eigenschaften wie `IsPostBack`, `IsValid`, `Request`und `Response`verfügbar. Oft erfordert jede Seite in einer Webanwendung zusätzliche Features oder Funktionen. Eine gängige Methode zum Bereitstellen dieser Methode ist das Erstellen einer benutzerdefinierten Basis Seiten Klasse. Eine benutzerdefinierte Basis Seiten Klasse ist eine Klasse, die Sie erstellen, die von der `Page`-Klasse abgeleitet ist und zusätzliche Funktionen umfasst. Nachdem Sie diese Basisklasse erstellt haben, können Sie die ASP.NET-Seiten davon ableiten (anstatt die `Page`-Klasse), sodass Sie den ASP.NET Seiten die erweiterte Funktionalität bieten.

In diesem Schritt erstellen Sie eine Basis Seite, die den Titel der Seite automatisch auf den Dateinamen der ASP.NET-Seite festlegt, wenn der Titel nicht anderweitig explizit festgelegt wurde. In Schritt 3 wird das Festlegen des Seitentitels basierend auf der Site Übersicht untersucht.

> [!NOTE]
> Eine gründliche Untersuchung der Erstellung und Verwendung von benutzerdefinierten Basis Seiten Klassen geht über den Rahmen dieser tutorialreihe hinaus. Weitere Informationen finden Sie unter [Verwenden einer benutzerdefinierten Basisklasse für Code Behind-Klassen Ihrer ASP.net Pages](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).

### <a name="creating-the-base-page-class"></a>Erstellen der Basis Seiten Klasse

Unsere erste Aufgabe besteht darin, eine Basis Seiten Klasse zu erstellen, bei der es sich um eine Klasse handelt, die die `Page` Klasse erweitert. Fügen Sie zunächst dem Projekt einen `App_Code` Ordner hinzu, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen klicken, ASP.NET Ordner hinzufügen auswählen und dann `App_Code`auswählen. Klicken Sie dann mit der rechten Maustaste auf den Ordner `App_Code`, und fügen Sie eine neue Klasse mit dem Namen `BasePage.vb`hinzu. Abbildung 4 zeigt die Projektmappen-Explorer, nachdem der Ordner `App_Code` und `BasePage.vb` Klasse hinzugefügt wurden.

![Fügen Sie einen App_Code Ordner und eine Klasse mit dem Namen BasePage hinzu.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Abbildung 04**: Hinzufügen eines `App_Code` Ordners und einer Klasse mit dem Namen `BasePage`

> [!NOTE]
> Visual Studio unterstützt zwei Modi der Projektverwaltung: Website Projekte und Webanwendungs Projekte. Der Ordner "`App_Code`" ist für die Verwendung mit dem Website Projekt Modell vorgesehen. Wenn Sie das Webanwendungs Projekt Modell verwenden, platzieren Sie die `BasePage.vb`-Klasse in einem anderen Ordner als `App_Code`, z. b. `Classes`. Weitere Informationen zu diesem Thema finden Sie unter [Migrieren eines Website Projekts zu einem Webanwendungs Projekt](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).

Da die benutzerdefinierte Basis Seite als Basisklasse für Code Behind-Klassen der ASP.net Pages fungiert, muss Sie die `Page`-Klasse erweitern.

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Jedes Mal, wenn eine ASP.NET-Seite angefordert wird, wird eine Reihe von Phasen durchlaufen, wobei die angeforderte Seite in HTML gerendert wird. Wir können auf eine Phase tippen, indem wir die `OnEvent` Methode der `Page` Klasse überschreiben. Legen Sie für unsere Basis Seite den Titel automatisch fest, wenn er nicht explizit durch die `LoadComplete`-Phase angegeben wurde (was möglicherweise nach der `Load` Phase auftritt).

Überschreiben Sie dazu die `OnLoadComplete`-Methode, und geben Sie den folgenden Code ein:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

Die `OnLoadComplete`-Methode beginnt mit dem bestimmen, ob die `Title` Eigenschaft noch nicht explizit festgelegt wurde. Wenn die `Title`-Eigenschaft `Nothing`, eine leere Zeichenfolge oder der Wert "unbenannte Seite" ist, wird Sie dem Dateinamen der angeforderten ASP.NET-Seite zugewiesen. Der physische Pfad zum angeforderten ASP.net page-`C:\MySites\Tutorial03\Login.aspx`, z. b., kann über die `Request.PhysicalPath`-Eigenschaft aufgerufen werden. Die `Path.GetFileNameWithoutExtension`-Methode wird verwendet, um nur den filename-Teil abzurufen, und dieser Dateiname wird dann der `Page.Title`-Eigenschaft zugewiesen.

> [!NOTE]
> Ich lade Sie ein, diese Logik zu verbessern, um das Format des Titels zu verbessern. Wenn z. b. der Dateiname der Seite `Company-Products.aspx`ist, erstellt der obige Code den Titel "Company-Products", aber im Idealfall würde der Bindestrich durch ein Leerzeichen ersetzt werden, wie z. b. "Unternehmens Produkte". Außerdem sollten Sie ggf. ein Leerzeichen hinzufügen, wenn es eine Fall Änderung gibt. Das heißt, Sie können Code hinzufügen, der den Dateinamen `OurBusinessHours.aspx` in den Titel "unsere Geschäftszeiten" umwandelt.

### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Die Inhaltsseiten erben die Basis Seiten Klasse

Wir müssen nun die ASP.NET-Seiten auf unserer Website aktualisieren, sodass Sie von der benutzerdefinierten Basis Seite (`BasePage`) anstatt von der `Page`-Klasse abgeleitet werden. Um dies zu erreichen, gehen Sie zu jeder Code Behind-Klasse, und ändern Sie die Klassen Deklaration von:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Bis:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Besuchen Sie anschließend die Website über einen Browser. Wenn Sie eine Seite besuchen, deren Titel explizit festgelegt ist, z. b. `Default.aspx` oder `About.aspx`, wird der explizit angegebene Titel verwendet. Wenn Sie jedoch eine Seite besuchen, deren Titel nicht von der Standardeinstellung ("unbenannte Seite") geändert wurde, legt die Basis Seiten Klasse den Titel auf den Dateinamen der Seite fest.

Abbildung 5 zeigt die `MultipleContentPlaceHolders.aspx` Seite, wenn Sie in einem Browser angezeigt wird. Beachten Sie, dass der Titel genau der Dateiname der Seite (abzüglich der Erweiterung), "multiplecontentplatzhalter", ist.

[![wenn ein Titel nicht explizit angegeben wird, wird der Dateiname der Seite automatisch verwendet.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Abbildung 05**: Wenn ein Titel nicht explizit angegeben wird, wird der Dateiname der Seite automatisch verwendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))

## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Schritt 3: basieren des Seitentitels auf der Site Übersicht

ASP.net bietet ein robustes Site Übersichts Framework, mit dem Seiten Entwickler eine hierarchische Site Zuordnung in einer externen Ressource (z. b. einer XML-Datei oder einer Datenbanktabelle) zusammen mit websteuer Elementen definieren können, um Informationen über die Site Übersicht anzuzeigen (z. b. SiteMapPath). Menü-und TreeView-Steuerelemente).

Auf die Site Map-Struktur kann auch Programm gesteuert über die Code Behind-Klasse einer ASP.NET-Seite zugegriffen werden. Auf diese Weise können wir den Titel einer Seite automatisch auf den Titel des entsprechenden Knotens in der Site Übersicht festlegen. Wir erweitern die in Schritt 2 erstellte `BasePage`-Klasse, sodass Sie diese Funktionalität bietet. Zuerst muss jedoch eine Site Übersicht für unsere Website erstellt werden.

> [!NOTE]
> In diesem Tutorial wird davon ausgegangen, dass der Reader bereits mit ASP vertraut ist. Die Site Übersichts Features von net. Weitere Informationen zum Verwenden der Site Übersicht finden Sie in der mehrteiligen Artikel Reihe unter unter [Suchen von ASP. Netzwerk-Website Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).

### <a name="creating-the-site-map"></a>Erstellen der Site Übersicht

Das Site Übersichts System basiert auf dem [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), das die Site Map-API von der Logik entkoppelt, die Site Übersichts Informationen zwischen Arbeitsspeicher und einem permanenten Speicher serialisiert. Die .NET Framework wird mit der [`XmlSiteMapProvider`-Klasse](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)ausgeliefert, die der Standard-Site Übersichts Anbieter ist. Wie der Name schon sagt, verwendet `XmlSiteMapProvider` eine XML-Datei als Site Übersichts Speicher. Wir verwenden diesen Anbieter zum Definieren der Site Übersicht.

Erstellen Sie zunächst eine Site Übersichts Datei im Stamm Ordner der Website namens `Web.sitemap`. Klicken Sie hierzu in Projektmappen-Explorer mit der rechten Maustaste auf den Namen der Website, wählen Sie neues Element hinzufügen aus, und wählen Sie die Vorlage Site Map aus. Stellen Sie sicher, dass die Datei `Web.sitemap` benannt ist, und klicken Sie dann auf

[![eine Datei namens "Web. Sitemap" dem Stamm Ordner der Website hinzufügen.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Abbildung 06**: Hinzufügen einer Datei mit dem Namen "`Web.sitemap`" zum Stamm Ordner der Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))

Fügen Sie der `Web.sitemap` Datei den folgenden XML-Code hinzu:

[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Dieser XML-Code definiert die hierarchische Site Übersichts Struktur, wie in Abbildung 7 dargestellt.

![Die Site Map besteht derzeit aus drei Site Übersichts Knoten.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Abbildung 07**: die Site Map besteht derzeit aus drei Site Übersichts Knoten.

Wir aktualisieren die Struktur der Site Map in zukünftigen Tutorials, wenn wir neue Beispiele hinzufügen.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualisieren der Master Seite, sodass Navigations-websteuer Elemente einbezogen werden

Nachdem wir nun eine Site Übersicht definiert haben, aktualisieren wir die Master Seite so, dass Sie Navigations-websteuer Elemente einschließt. Fügen wir der linken Spalte im Abschnitt "Lektionen" ein ListView-Steuerelement hinzu, das eine ungeordnete Liste mit einem Listenelement für jeden in der Site Map definierten Knoten rendert.

> [!NOTE]
> Das ListView-Steuerelement ist neu in ASP.net, Version 3,5. Wenn Sie eine frühere Version von ASP.NET verwenden, verwenden Sie stattdessen das Repeater-Steuerelement. Weitere Informationen zum ListView-Steuerelement finden [Sie unter Verwenden der ListView-und DataPager-Steuerelemente von ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Entfernen Sie zunächst das vorhandene ungeordnete Listen Markup aus dem Abschnitt Lektionen. Ziehen Sie als nächstes ein ListView-Steuerelement aus der Toolbox, und legen Sie es unterhalb der Überschrift Lektionen ab. Die ListView befindet sich im Daten Abschnitt der Toolbox neben den anderen Ansicht-Steuerelementen: GridView, DetailsView und FormView. Legen Sie die `ID`-Eigenschaft von ListView auf `LessonsList`fest.

Wählen Sie im Assistenten zum Konfigurieren von Datenquellen aus, dass ListView an ein neues SiteMapDataSource-Steuerelement mit dem Namen `LessonsDataSource`gebunden werden soll. Das SiteMapDataSource-Steuerelement gibt die hierarchische Struktur aus dem Site Map-System zurück.

[![Binden eines SiteMapDataSource-Steuer Elements an das lessonslist-ListView-Steuerelement](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Abbildung 08**: Binden eines SiteMapDataSource-Steuer Elements an das lessonslist-ListView-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))

Nach dem Erstellen des SiteMapDataSource-Steuer Elements müssen die ListView-Vorlagen definiert werden, damit eine ungeordnete Liste mit einem Listenelement für jeden Knoten gerendert wird, der vom SiteMapDataSource-Steuerelement zurückgegeben wird. Dies kann mithilfe des folgenden Vorlagen Markups erreicht werden:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

Das `LayoutTemplate` generiert das Markup für eine ungeordnete Liste (`<ul>...</ul>`), während die `ItemTemplate` jedes von SiteMapDataSource zurückgegebene Element als Listenelement (`<li>`) rendert, das einen Link zur jeweiligen Lektion enthält.

Nachdem Sie die ListView-Vorlagen konfiguriert haben, besuchen Sie die Website. Wie in Abbildung 9 gezeigt, enthält der Abschnitt "Lektionen" ein einzelnes Aufgabenelement, "Home". Wo sind die Lektionen "about" und "using multiple contentplachalter Controls"? SiteMapDataSource ist so konzipiert, dass ein hierarchischer Satz von Daten zurückgegeben wird, aber das ListView-Steuerelement kann nur eine einzige Ebene der Hierarchie anzeigen. Folglich wird nur die erste Ebene der Site Übersichts Knoten angezeigt, die von SiteMapDataSource zurückgegeben wird.

[![Abschnitt "Lektionen" enthält ein einzelnes Listenelement](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Abbildung 09**: der Abschnitt "Lektionen" enthält ein einzelnes Listenelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))

Wenn Sie mehrere Ebenen anzeigen möchten, können Sie mehrere ListViews innerhalb des `ItemTemplate`Schachteln. Diese Vorgehensweise wurde im Tutorial " [Arbeiten mit Daten](../../data-access/index.md)" unter " [ *Master Seiten und Website Navigation* ](../../data-access/introduction/master-pages-and-site-navigation-vb.md) " untersucht. Für diese tutorialreihe enthält die Site Übersicht jedoch nur zwei Ebenen: "Home" (die oberste Ebene); und jede Lektion als untergeordnetes Element von "Home". Anstatt eine geclusterte ListView zu erstellen, können Sie stattdessen die SiteMapDataSource anweisen, den Startknoten nicht zurückzugeben, indem Sie die [`ShowStartingNode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) auf `False`festlegen. Der Endeffekt ist, dass SiteMapDataSource zuerst die zweite Ebene der Site Übersichts Knoten zurückgibt.

Mit dieser Änderung zeigt ListView Aufzählungs Elemente für die Lektionen "about" und "using multiple contentplachalter Controls" an, lässt jedoch ein Aufzählungs Zeichen aus. Um dieses Problem zu beheben, können wir im `LayoutTemplate`explizit ein Aufzählungs Zeichen hinzufügen:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Wenn Sie SiteMapDataSource so konfigurieren, dass der Startknoten ausgelassen und explizit ein Start Aufzählungs Element hinzugefügt wird, wird im Abschnitt Lektionen nun die gewünschte Ausgabe angezeigt.

[![der Abschnitt Lektionen enthält ein Aufzählungs Zeichen für Home und jeden untergeordneten Knoten.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Abbildung 10**: der Abschnitt "Lektionen" enthält ein Aufzählungs Zeichen für Home und jeden untergeordneten Knoten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))

### <a name="setting-the-title-based-on-the-site-map"></a>Festlegen des Titels basierend auf der Site Übersicht

Wenn die Site Map vorhanden ist, können wir unsere `BasePage`-Klasse aktualisieren, um den in der Site Map angegebenen Titel zu verwenden. Wie in Schritt 2, möchten wir nur den Titel des Site Übersichts Knotens verwenden, wenn der Seitentitel nicht explizit vom Seiten Entwickler festgelegt wurde. Wenn die angeforderte Seite keinen explizit festgelegten Seitentitel hat und nicht in der Site Übersicht gefunden wird, verwenden wir den Dateinamen der angeforderten Seite (abzüglich der Erweiterung), wie in Schritt 2. In Abbildung 11 wird dieser Entscheidungsprozess veranschaulicht.

![Wenn kein explizit festgelegter Seitentitel vorhanden ist, wird der zugehörige Titel des Site Übersichts Knotens verwendet.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Abbildung 11**: Wenn kein explizit festgelegter Seitentitel vorhanden ist, wird der zugehörige Titel des Site Übersichts Knotens verwendet.

Aktualisieren Sie die `OnLoadComplete`-Methode der `BasePage` Klasse, sodass Sie den folgenden Code enthält:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Wie zuvor wird durch die `OnLoadComplete`-Methode bestimmt, ob der Titel der Seite explizit festgelegt wurde. Wenn `Page.Title` `Nothing`, eine leere Zeichenfolge oder der Wert "unbenannte Seite" zugewiesen ist, weist der Code `Page.Title`automatisch einen Wert zu.

Um den zu verwendenden Titel zu ermitteln, beginnt der Code mit dem Verweis auf die [`CurrentNode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)der [`SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx). `CurrentNode` gibt die [`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) Instanz in der Site Übersicht zurück, die der aktuell angeforderten Seite entspricht. Wenn die aktuell angeforderte Seite innerhalb der Site Übersicht gefunden wird, wird die `Title`-Eigenschaft des `SiteMapNode`dem Titel der Seite zugewiesen. Wenn die aktuell angeforderte Seite nicht in der Site Übersicht angezeigt wird, gibt `CurrentNode` `Nothing` zurück, und der Dateiname der angeforderten Seite wird als Titel verwendet (wie in Schritt 2 ausgeführt).

In Abbildung 12 wird die `MultipleContentPlaceHolders.aspx` Seite angezeigt, wenn Sie in einem Browser angezeigt wird. Da der Titel dieser Seite nicht explizit festgelegt wird, wird stattdessen der zugehörige Titel des Site Übersichts Knotens verwendet.

![Der Titel der multiplecontentplatz Halter. aspx-Seite wird aus der Site Übersicht abgerufen.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Abbildung 12**: der Titel der multiplecontentplatz Halter. aspx-Seite wird aus der Site Übersicht abgerufen

## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Schritt 4: Hinzufügen anderer Seiten spezifischer Markup zum`<head>`Abschnitt

In den Schritten 1, 2 und 3 wurde das Anpassen des `<title>`-Elements auf Seitenbasis betrachtet. Zusätzlich zu `<title>`kann der `<head>` Abschnitt `<meta>` Elemente und `<link>` Elemente enthalten. Wie bereits weiter oben in diesem Tutorial erwähnt, enthält `Site.master``<head>` Abschnitt ein `<link>` Element, das `Styles.css`werden kann. Da dieses `<link>` Element innerhalb der Master Seite definiert ist, ist es im `<head>` Abschnitt für alle Inhaltsseiten enthalten. Aber wie geht es um das Hinzufügen von `<meta>` und `<link>` Elementen auf Seite-für-Seite-Basis?

Die einfachste Möglichkeit zum Hinzufügen von Seiten spezifischem Inhalt zum `<head>` Abschnitt besteht darin, ein contentplachalter-Steuerelement auf der Master Seite zu erstellen. Wir haben bereits einen derartigen contentplachalter (mit dem Namen `head`). Um benutzerdefinierte `<head>` Markup hinzuzufügen, erstellen Sie daher ein entsprechendes Inhalts Steuerelement auf der Seite, und platzieren Sie das Markup dort.

Um das Hinzufügen von benutzerdefiniertem `<head>` Markup zu einer Seite zu veranschaulichen, fügen wir ein `<meta>` Description-Element in den aktuellen Satz von Inhaltsseiten ein. Das `<meta>` Description-Element enthält eine kurze Beschreibung der Webseite. die meisten Suchmaschinen enthalten diese Informationen in irgendeiner Form, wenn Suchergebnisse angezeigt werden.

Ein `<meta>` Description-Element weist die folgende Form auf:

[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Um dieses Markup zu einer Inhaltsseite hinzuzufügen, fügen Sie den obigen Text dem Content-Steuerelement hinzu, das dem `head` contentplachalter der Master Seite zugeordnet ist. Wenn Sie z. b. ein `<meta>` Description-Element für `Default.aspx`definieren möchten, fügen Sie das folgende Markup hinzu:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Da der `head` contentplachalter nicht im Text der HTML-Seite liegt, wird das dem Inhalts Steuerelement hinzugefügte Markup nicht in der Designansicht angezeigt. Um das `<meta>` Description-Element anzuzeigen, besuchen Sie `Default.aspx` über einen Browser. Nachdem die Seite geladen wurde, sehen Sie sich die Quelle an, und beachten Sie, dass der `<head>` Abschnitt das im Inhalts Steuerelement angegebene Markup enthält.

Nehmen Sie sich einen Moment Zeit, um `About.aspx`, `MultipleContentPlaceHolders.aspx`und `Login.aspx``<meta>` Beschreibungs Elemente hinzuzufügen.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programm gesteuertes Hinzufügen von Markup zum`<head>`Bereich

Der `head` contentplachalter ermöglicht es uns, dem `<head>` Bereich der Master Seite deklarativ benutzerdefiniertes Markup hinzuzufügen. Benutzerdefiniertes Markup kann auch Programm gesteuert hinzugefügt werden. Beachten Sie, dass die `Header`-Eigenschaft der `Page` Klasse die `HtmlHead` Instanz zurückgibt, die auf der Master Seite (dem `<head runat="server">`) definiert ist.

Das programmgesteuerte Hinzufügen von Inhalt zum `<head>` Bereich ist hilfreich, wenn der hinzu zufügende Inhalt dynamisch ist. Vielleicht basiert Sie auf dem Benutzer, der die Seite besucht. Vielleicht wird Sie aus einer Datenbank abgerufen. Unabhängig von der Ursache können Sie dem `HtmlHead` Inhalte hinzufügen, indem Sie der `Controls` Auflistung wie folgt Steuerelemente hinzufügen:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Der obige Code fügt der `<head>` Region das `<meta>` Keywords-Element hinzu, das eine durch Trennzeichen getrennte Liste von Schlüsselwörtern bereitstellt, die die Seite beschreiben. Beachten Sie, dass Sie zum Hinzufügen eines `<meta>`-Tags eine [`HtmlMeta`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) Instanz erstellen, deren Eigenschaften `Name` und `Content` festlegen und Sie dann der `Header`Auflistung `Controls` hinzufügen. Wenn Sie ein `<link>` Element Programm gesteuert hinzufügen möchten, erstellen Sie ein [`HtmlLink`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) Objekt, legen Sie seine Eigenschaften fest, und fügen Sie es dann der `Controls` Auflistung des `Header`hinzu.

> [!NOTE]
> Um beliebiges Markup hinzuzufügen, erstellen Sie eine [`LiteralControl`](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) Instanz, legen Sie die `Text`-Eigenschaft fest, und fügen Sie Sie dann der `Controls` Auflistung des `Header`hinzu.

## <a name="summary"></a>Summary

In diesem Tutorial haben wir eine Vielzahl von Möglichkeiten zum Hinzufügen von `<head>` Regions Markup auf seitenweise erläutert. Eine Master Seite sollte eine `HtmlHead` Instanz (`<head runat="server">`) mit einem contentplachalter enthalten. Die `HtmlHead`-Instanz ermöglicht Inhaltsseiten den programmgesteuerten Zugriff auf den `<head>` Bereich und die deklarative und programmgesteuerte Festlegung des Seitentitels. Das contentplachalter-Steuerelement ermöglicht, dass benutzerdefiniertes Markup dem `<head>` Abschnitt deklarativ über ein Inhalts Steuerelement hinzugefügt werden kann.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Dynamisches Festlegen des Seitentitels in ASP.net](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Untersuchen von ASP. NET-Site Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Verwenden von HTML-Meta-Tags](http://searchenginewatch.com/showPage.html?page=2167931)
- [Master Seiten in ASP.net](http://www.odetocode.com/articles/419.aspx)
- [Verwenden der ListView-und DataPager-Steuerelemente von ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Verwenden einer benutzerdefinierten Basisklasse für Code Behind-Klassen Ihrer ASP.net Pages](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial waren Zack Jones und Suchi Banerjee. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](multiple-contentplaceholders-and-default-content-vb.md)
> [Weiter](urls-in-master-pages-vb.md)
