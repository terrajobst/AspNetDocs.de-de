---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Erstellen eines Website weiten Layouts mit Master Seiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden Grundlagen der Master Seite veranschaulicht. Dabei handelt es sich um Masterseiten, wie wird eine Master Seite erstellt, was sind Inhalts Platzhalter, wie wird eine CR...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: c0ee6ed9d944b9a8ff2b2996e93706b8416de905
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519225"
---
# <a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Erstellen eines websiteweiten Layouts mit Masterseiten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> In diesem Tutorial werden Grundlagen der Master Seite veranschaulicht. Dabei handelt es sich um Masterseiten, wie eine Master Seite erstellt wird, was Inhalts Platzhalter ist, wie eine ASP.NET Seite erstellt wird, die eine Master Seite verwendet, wie die Änderung der Master Seite automatisch auf den zugehörigen Inhaltsseiten reflektiert wird usw.

## <a name="introduction"></a>Einführung

Ein Attribut einer gut entworfenen Website ist ein konsistentes Seitenlayout für Websites. Nehmen Sie z. b. die www.ASP.NET-Website an. Zum Zeitpunkt der Erstellung dieses Artikels hat jede Seite denselben Inhalt am oberen und unteren Rand der Seite. Wie in Abbildung 1 zu sehen ist, wird am oberen Rand jeder Seite eine graue Leiste mit einer Liste von Microsoft-Communitys angezeigt. Darunter ist das Website Logo, die Liste der Sprachen, in die die Website übersetzt wurde, und die Kern Abschnitte: Startseite, Einstieg, erlernen, Downloads usw. Der untere Teil der Seite enthält auch Informationen über die Werbung auf www.ASP.net, eine Copyright Erklärung und einen Link zu den Datenschutzbestimmungen.

[![der www.ASP.NET-Website ein konsistentes Erscheinungsbild auf allen Seiten verwendet.](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Abbildung 01</strong>: die www.ASP.NET-Website verwendet ein konsistentes Erscheinungsbild auf allen Seiten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))

Ein anderes Attribut einer gut entworfenen Website ist die Einfachheit, mit der die Darstellung der Site geändert werden kann. In Abbildung 1 wird die www.ASP.net-Startseite ab dem 2008 angezeigt, aber zwischen jetzt und der Veröffentlichung dieses Tutorials hat sich das Aussehen und das Erscheinungsbild möglicherweise geändert. Möglicherweise werden die Menü Elemente am oberen Rand erweitert, sodass Sie einen neuen Abschnitt für das MVC-Framework enthalten. Oder vielleicht wird ein völlig neuer Entwurf mit unterschiedlichen Farben, Schriftarten und Layout präsentiert. Das Anwenden solcher Änderungen auf die gesamte Website sollte ein schneller und einfacher Prozess sein, der nicht das Ändern der Tausenden von Webseiten erfordert, die die Website bilden.

Das Erstellen einer Website-weiten Seitenvorlage in ASP.net ist durch die Verwendung von *Masterseiten*möglich. Kurz gesagt, ist eine Master Seite eine besondere Art von ASP.net, die das Markup definiert, das für alle *Inhaltsseiten* und für Bereiche, die auf Inhaltsseiten basieren, auf Seitenbasis definiert ist. (Eine Inhaltsseite ist eine ASP.NET Seite, die an die Master Seite gebunden ist.) Jedes Mal, wenn das Layout oder die Formatierung einer Master Seite geändert wird, wird die gesamte Ausgabe der Inhaltsseiten ebenfalls sofort aktualisiert. Dadurch wird die Anwendung der Website weiten Darstellung genauso einfach wie das Aktualisieren und Bereitstellen einer einzelnen Datei (nämlich der Master Seite).

Dies ist das erste Tutorial in einer Reihe von Tutorials, in denen die Verwendung von Masterseiten untersucht wird. Im Rahmen dieser tutorialreihe werden folgende Aktionen durchgeführt:

- Untersuchen der Erstellung von Masterseiten und der zugehörigen Inhaltsseiten
- Diskutieren Sie eine Vielzahl von Tipps, Tricks und Traps,
- Identifizieren Sie häufige Fehler bei der Master Seite, und untersuchen Sie die Problem Umgehungen.
- Siehe zugreifen auf die Master Seite von einer Inhaltsseite aus und umgekehrt,
- Erfahren Sie, wie Sie die Master Seite einer Inhaltsseite zur Laufzeit angeben.
- Weitere Themen zu erweiterten Masterseiten.

Diese Lernprogramme sind so konzipiert, dass Sie präzise sind, und enthalten Schritt-für-Schritt-Anleitungen mit vielen Screenshots, um Sie durch den Prozess visuell zu führen. Jedes Tutorial ist in C# und Visual Basic Versionen verfügbar und enthält einen Download des gesamten verwendeten Codes.

Dieses erste Tutorial beginnt mit einem Blick auf die Grundlagen der Master Seite. Es wird erläutert, wie Masterseiten funktionieren. Sie erfahren, wie Sie eine Master Seite und zugehörige Inhaltsseiten mithilfe von Visual Web Developer erstellen, und wie sich Änderungen an einer Master Seite sofort auf den Inhaltsseiten widerspiegeln. Erste Schritte

## <a name="understanding-how-master-pages-work"></a>Grundlegendes zur Funktionsweise von Master Seiten

Das Erstellen einer Website mit einem konsistenten Website Layout erfordert, dass jede Webseite neben Ihrem benutzerdefinierten Inhalt auch ein gemeinsames Formatierungs Markup ausgibt. Während jedes Lernprogramm oder jeder Forumsbeitrag auf www.ASP.net über einen eigenen eindeutigen Inhalt verfügt, wird jede dieser Seiten jedoch auch eine Reihe allgemeiner `<div>` Elemente, die die Links auf der obersten Ebene des Abschnitts anzeigen: "Home", "erste Schritte", "erlernen" usw.

Es gibt eine Reihe von Techniken zum Erstellen von Webseiten mit einem konsistenten Erscheinungsbild. Ein naiver Ansatz besteht darin, das allgemeine layoutmarkup einfach zu kopieren und in alle Webseiten einzufügen, aber dieser Ansatz hat eine Reihe von downpages. Zunächst einmal müssen Sie bei jedem Erstellen einer neuen Seite daran denken, den freigegebenen Inhalt zu kopieren und in die Seite einzufügen. Solche Kopier-und Einfügevorgänge sind für einen Fehler reif, da Sie möglicherweise versehentlich nur eine Teilmenge des freigegebenen Markups in eine neue Seite kopieren. Diese Vorgehensweise ermöglicht es Ihnen, die vorhandene Website weite Darstellung durch ein neues zu ersetzen, da jede einzelne Seite der Site bearbeitet werden muss, um das neue Erscheinungsbild zu verwenden.

Vor ASP.net, Version 2,0, haben Seiten Entwickler häufig allgemeine Markup in [Benutzer Steuerelemente](https://msdn.microsoft.com/library/y6wb1a0e.aspx) eingefügt und diese Benutzer Steuerelemente dann zu jeder Seite hinzugefügt. Dieser Ansatz erforderte, dass der Seiten Entwickler daran erinnert, die Benutzer Steuerelemente manuell zu jeder neuen Seite hinzuzufügen, aber für einfachere Website weite Änderungen zulässig sind, da bei der Aktualisierung des allgemeinen Markups nur die Benutzer Steuerelemente geändert werden mussten. Leider sind Visual Studio .NET 2002 und 2003-die Versionen von Visual Studio, die zum Erstellen von ASP.NET 1. x-Anwendungen verwendet wurden, die Benutzer Steuerelemente im Designansicht als graue Felder gerendert. Folglich haben Seiten Entwickler, die diesen Ansatz verwenden, keine WYSIWYG-Entwurfszeit Umgebung.

Die Mängel bei der Verwendung von Benutzer Steuerelementen wurden in ASP.NET Version 2,0 und Visual Studio 2005 mit der Einführung von *Masterseiten*behandelt. Eine Master Seite ist eine besondere Art von ASP.net, die sowohl das Site weite Markup als auch die *Regionen* definiert, in denen zugehörige *Inhaltsseiten* Ihr benutzerdefiniertes Markup definieren. Wie wir in Schritt 1 sehen werden, werden diese Regionen durch contentplachalter-Steuerelemente definiert. Das contentplachalter-Steuerelement bezeichnet einfach eine Position in der Steuerelement Hierarchie der Master Seite, in der benutzerdefinierter Inhalt von einer Inhaltsseite eingefügt werden kann.

> [!NOTE]
> Die grundlegenden Konzepte und Funktionen von Masterseiten wurden seit der ASP.NET-Version 2,0 nicht geändert. Visual Studio 2008 bietet jedoch Unterstützung für die Entwurfszeit für die Erstellung von in Visual Studio 2005 fehlenden Features. In einem zukünftigen Tutorial wird die Verwendung von in einer Zukunft verwendeten Masterseiten untersucht.

Abbildung 2 zeigt, wie die Master Seite für www.ASP.net aussehen könnte. Beachten Sie, dass die Master Seite das allgemeine standortweite Layout definiert: das Markup oben, unten und rechts auf jeder Seite sowie einen contentplachalter in der Mitte Links, wo sich der eindeutige Inhalt für jede einzelne Webseite befindet.

![Eine Master Seite definiert das Site weite Layout und die in einer Inhaltsseite auf Inhaltsseite bearbeitbaren Regionen.](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Abbildung 02**: eine Master Seite definiert das Site weite Layout und die in einer Inhaltsseite auf Inhaltsseite bearbeitbaren Regionen.

Nachdem eine Master Seite definiert wurde, kann Sie über den Tick eines Kontrollkästchens an neue ASP.NET Seiten gebunden werden. Diese ASP.net Pages, die als Inhaltsseiten bezeichnet werden, enthalten ein Inhalts Steuerelement für jede der contentplachalter-Steuerelemente der Master Seite. Wenn die Inhaltsseite über einen Browser besucht wird, erstellt die ASP.net-Engine die Steuerelement Hierarchie der Master Seite und fügt die Steuerelement Hierarchie der Inhaltsseite an die entsprechenden Stellen ein. Diese kombinierte Steuerelement Hierarchie wird gerendert, und der resultierende HTML-Code wird an den Browser des Endbenutzers zurückgegeben. Folglich gibt die Inhaltsseite sowohl das allgemeine Markup, das in der Master Seite definiert ist, außerhalb der contentplachalter-Steuerelemente als auch das Seiten spezifische Markup aus, das in den eigenen Inhalts Steuerelementen definiert ist. In Abbildung 3 wird dieses Konzept veranschaulicht.

[![das Markup der angeforderten Seite mit der Master Seite verschmolzen](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Abbildung 03**: das Markup der angeforderten Seite wird auf der Master Seite angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))

Nachdem wir nun erläutert haben, wie Masterseiten funktionieren, sehen wir uns nun die Erstellung einer Master Seite und zugehöriger Inhaltsseiten mithilfe von Visual Web Developer an.

> [!NOTE]
> Um die größtmögliche Zielgruppe zu erreichen, wird die ASP.NET-Website, die wir in dieser tutorialreihe erstellen, mit ASP.NET 3,5 mit der kostenlosen Version von Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)von Microsoft erstellt. Wenn Sie noch kein Upgrade auf ASP.NET 3,5 durchgeführt haben, machen Sie sich keine Sorgen: die in diesen Tutorials behandelten Konzepte funktionieren gleichermaßen gut mit ASP.NET 2,0 und Visual Studio 2005. Einige Demoanwendungen verwenden jedoch möglicherweise Features, die neu in der .NET Framework Version 3,5; Wenn 3,5-spezifische Features verwendet werden, wird ein Hinweis angezeigt, in dem erläutert wird, wie eine ähnliche Funktionalität in Version 2,0 implementiert wird. Beachten Sie, dass die Demoanwendungen, die in jedem Lernprogramm zum Herunterladen verfügbar sind, auf die .NET Framework Version 3,5 abzielen, die zu einer `Web.config` Datei führt, die 3,5-spezifische Konfigurationselemente enthält. Lange Geschichte kurz: Wenn Sie .NET 3,5 noch nicht auf Ihrem Computer installieren müssen, funktioniert die herunterladbare Webanwendung nicht, ohne zuvor das 3,5-spezifische Markup aus `Web.config`zu entfernen. Weitere Informationen zu diesem Thema finden Sie unter [Dissecting ASP.NET Version 3.5 `Web.config` Datei](http://www.4guysfromrolla.com/articles/121207-1.aspx) .

## <a name="step-1-creating-a-master-page"></a>Schritt 1: Erstellen einer Master Seite

Bevor wir die Erstellung und Verwendung von Master-und Inhaltsseiten erkunden können, benötigen wir zuerst eine ASP.NET-Website. Beginnen Sie mit dem Erstellen einer neuen Dateisystem basierten ASP.NET-Website. Starten Sie hierzu Visual Web Developer, und navigieren Sie dann zum Menü Datei, und wählen Sie neue Website aus, die das Dialogfeld neue Website anzeigt (siehe Abbildung 4). Wählen Sie die Vorlage ASP.NET-Website aus, legen Sie die Dropdown Liste Speicherort auf Datei System fest, wählen Sie einen Ordner zum Platzieren der Website aus, und legen Sie die Sprache auf Visual Basic fest. Dadurch wird eine neue Website mit einer `Default.aspx` ASP.NET Seite, einem Ordner `App_Data` und einer `Web.config`-Datei erstellt.

> [!NOTE]
> Visual Studio unterstützt zwei Modi der Projektverwaltung: Website Projekte und Webanwendungs Projekte. Bei Website Projekten fehlt eine Projektdatei, während Webanwendungs Projekte die Projekt Architektur in Visual Studio .NET 2002/2003 imitieren. Sie enthalten eine Projektdatei und kompilieren den Quellcode des Projekts in eine einzelne Assembly, die sich im Ordner "`/bin`" befindet. Visual Studio 2005 ursprünglich nur unterstützte Website Projekte, obwohl das Webanwendungs Projekt Modell mit Service Pack 1 neu eingeführt wurde. Visual Studio 2008 bietet beide Projekt Modelle. Die Editionen "Visual Web Developer 2005" und "2008" unterstützen jedoch nur Website Projekte. Ich verwende das Website Projekt Modell für meine Demos in dieser tutorialreihe. Wenn Sie eine nicht-Express-Edition verwenden und stattdessen das [Webanwendungs Projekt Modell](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) verwenden möchten, können Sie dies tun, aber beachten Sie, dass es möglicherweise einige Abweichungen zwischen den auf Ihrem Bildschirm angezeigten Schritten und den erforderlichen Schritten und den in diesen Tutorials dargestellten Bildschirmfotos gibt.

[![Erstellen einer neuen Datei System basierten Website](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Abbildung 04**: Erstellen einer neuen Datei System basierten Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))

Fügen Sie als nächstes der Website im Stammverzeichnis eine Master Seite hinzu, indem Sie mit der rechten Maustaste auf den Projektnamen klicken, neues Element hinzufügen auswählen und die Vorlage Master Seite auswählen. Beachten Sie, dass Masterseiten mit der Erweiterung `.master`enden. Benennen Sie diese neue Master Seite `Site.master` und klicken Sie dann auf hinzufügen.

[![der Website eine Master Seite mit dem Namen "Site. Master" hinzufügen](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Abbildung 05**: Hinzufügen einer Master Seite mit dem Namen `Site.master` zur Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))

Durch das Hinzufügen einer neuen Masterseiten Datei über Visual Web Developer wird eine Master Seite mit folgendem deklarativen Markup erstellt:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

Die erste Zeile im deklarativen Markup ist die [`@Master`-Direktive](https://msdn.microsoft.com/library/ms228176.aspx). Die `@Master`-Direktive ähnelt der [`@Page` Direktive](https://msdn.microsoft.com/library/ydy4x04a.aspx) , die auf ASP.NET Seiten angezeigt wird. Er definiert die serverseitige Sprache (VB) und Informationen über den Speicherort und die Vererbung der Code-Behind-Klasse der Master Seite.

Die `DOCTYPE` und das deklarative Markup der Seite werden unterhalb der `@Master`-Anweisung angezeigt. Die Seite enthält statisches HTML zusammen mit vier serverseitigen Steuerelementen:

- **Ein Webformular (das `<form runat="server">`)** : da alle ASP.NET Seiten normalerweise ein Webformular haben und die Master Seite websteuer Elemente enthalten kann, die in einem Web Form angezeigt werden müssen, müssen Sie das Webformular der Master Seite hinzufügen (anstatt jeder Inhaltsseite ein Webformular hinzuzufügen).
- **Ein contentplachalter-Steuerelement mit dem Namen `ContentPlaceHolder1`** : dieses contentplachalter-Steuerelement wird innerhalb des Webformulars angezeigt und dient als Region für die Benutzeroberfläche der Inhaltsseite.
- **Ein serverseitiges `<head>` Element** -das `<head>` Element verfügt über das `runat="server"`-Attribut, sodass es über serverseitigen Code zugänglich ist. Das `<head>`-Element wird auf diese Weise implementiert, sodass der Titel der Seite und andere `<head>`bezogenes Markup Programm gesteuert hinzugefügt oder angepasst werden können. Wenn Sie z. b. die `Title`-Eigenschaft einer ASP.NET-Seite festlegen, wird das `<title>` Element geändert, das vom `<head>`-Server Steuerelement
- **Ein contentplachalter-Steuerelement mit dem Namen `head`** -dieses contentplachalter-Steuerelement wird im `<head>` Server Steuerelement angezeigt und kann verwendet werden, um dem `<head>` Element deklarativ Inhalt hinzuzufügen.

Dieses deklarative Standard Markup für die Master Seite dient als Ausgangspunkt für das Entwerfen Ihrer eigenen Masterseiten. Sie können den HTML-Code bearbeiten oder der Master Seite Weitere websteuer Elemente oder contentplatzhalter hinzufügen.

> [!NOTE]
> Stellen Sie beim Entwerfen einer Master Seite sicher, dass die Master Seite ein Webformular enthält und dass mindestens ein contentplachalter-Steuerelement in diesem Webformular angezeigt wird.

### <a name="creating-a-simple-site-layout"></a>Erstellen eines einfachen Website Layouts

Erweitern Sie nun das standardmäßige deklarative Markup von `Site.master`, um ein Website Layout zu erstellen, in dem alle Seiten gemeinsam sind: ein allgemeiner Header. eine linke Spalte mit Navigation, Nachrichten und anderen Website weiten Inhalten und eine Fußzeile, in der das Symbol "unter Microsoft ASP.net" angezeigt wird. Abbildung 6 zeigt das Endergebnis der Master Seite, wenn eine ihrer Inhaltsseiten über einen Browser angezeigt wird. Der rote Kreisbereich in Abbildung 6 ist spezifisch für die besuchte Seite (`Default.aspx`). der andere Inhalt wird auf der Master Seite definiert und ist daher auf allen Inhaltsseiten konsistent.

[![die Master Seite definiert das Markup für die Bereiche oben, Links und unten.](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Abbildung 06**: die Master Seite definiert das Markup für die oberen, linken und unteren Teile ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))

Um das in Abbildung 6 gezeigte Standort Layout zu erreichen, müssen Sie zunächst die `Site.master` Master Seite aktualisieren, sodass Sie Folgendes deklaratives Markup enthält:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Das Layout der Master Seite wird mit einer Reihe von `<div>` HTML-Elementen definiert. Die `topContent` `<div>` enthält das Markup, das am oberen Rand jeder Seite angezeigt wird, während die `mainContent`, `leftContent`und `footerContent` `<div>` s verwendet werden, um den Inhalt der Seite, die linke Spalte und das Symbol "unter" mit dem Microsoft ASP.net ". Zusätzlich zum Hinzufügen dieser `<div>` Elemente habe ich auch die `ID`-Eigenschaft des primären contentplachalter-Steuer Elements von `ContentPlaceHolder1` in `MainContent`umbenannt.

Die Formatierungs-und Layoutregeln für diese sortierten `<div>` Elemente werden in der CSS-Datei `Styles.css`[(Cascading Stylesheet)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) geschrieben, die über ein `<link>`-Element im `<head>` Element der Master Seite angegeben wird. Diese verschiedenen Regeln definieren das Aussehen und das Gefühl der einzelnen `<div>` Elemente, die oben erwähnt werden. Beispielsweise werden mit dem `topContent` `<div>`-Element, das den Text und Link "Master Pages Tutorials" anzeigt, die Formatierungs Regeln in `Styles.css` wie folgt angegeben:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Wenn Sie auf Ihrem Computer auf dem Computer folgen, müssen Sie den dazugehörigen Code dieses Tutorials herunterladen und die `Styles.css`-Datei zu Ihrem Projekt hinzufügen. Ebenso müssen Sie einen Ordner mit dem Namen `Images` erstellen und das Symbol "unter Microsoft ASP.net" von der heruntergeladenen demowebsite in Ihr Projekt kopieren.

> [!NOTE]
> Eine Erörterung der CSS-und Webseiten Formatierung geht über den Rahmen dieses Artikels hinaus. Weitere Informationen zu CSS finden Sie in den [CSS-Tutorials](http://www.w3schools.com/css/default.asp) unter [W3Schools.com](http://www.w3schools.com/). Ich empfehle Ihnen außerdem, den begleitenden Code dieses Tutorials herunterzuladen und mit den CSS-Einstellungen in `Styles.css` zu experimentieren, um die Auswirkungen verschiedener Formatierungs Regeln anzuzeigen.

### <a name="creating-a-master-page-using-an-existing-design-template"></a>Erstellen einer Master Seite mithilfe einer vorhandenen Entwurfs Vorlage

Im Laufe der Jahre habe ich eine Reihe von ASP.NET-Webanwendungen für kleine bis mittelgroße Unternehmen erstellt. Einige meiner Clients verfügten über ein vorhandenes Site Layout, das Sie verwenden wollten. andere haben einen kompetenten Grafik-Designer eingestellt. Ein paar vertraute mich zum Entwerfen des Website Layouts an. Wie Sie in Abbildung 6 erkennen können, ist es in der Regel genauso klug, dass ein Programmierer das Layout einer Website entwerfen muss, wenn Sie den eigenen Arzt für Ihre Steuern benötigen.

Glücklicherweise gibt es unzählige Websites, die kostenlose HTML-Entwurfsvorlagen bieten. Google hat mehr als 6 Millionen Ergebnisse für den Suchbegriff "kostenlose Website Vorlagen" zurückgegeben. Einer meiner bevorzugten ist [OpenDesigns.org](http://opendesigns.org/). Nachdem Sie eine Website Vorlage gefunden haben, fügen Sie die CSS-Dateien und Images Ihrem Website Projekt hinzu, und integrieren Sie den HTML-Code der Vorlage in die Master Seite.

> [!NOTE]
> Außerdem bietet Microsoft eine Reihe von [kostenlosen ASP.net Design-Start-Kit-Vorlagen](https://msdn.microsoft.com/asp.net/aa336613.aspx) , die in Visual Studio in das Dialogfeld neue Website integriert werden können.

## <a name="step-2-creating-associated-content-pages"></a>Schritt 2: Erstellen zugehöriger Inhaltsseiten

Nachdem die Master Seite erstellt wurde, können Sie mit dem Erstellen von ASP.NET Seiten beginnen, die an die Master Seite gebunden sind. Diese Seiten werden als *Inhaltsseiten*bezeichnet.

Fügen Sie dem Projekt eine neue ASP.NET-Seite hinzu, und binden Sie Sie an die `Site.master` Master Seite. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie die Option Neues Element hinzufügen aus. Wählen Sie die Web Form-Vorlage aus, geben Sie den Namen `About.aspx`ein, und aktivieren Sie dann das Kontrollkästchen "Master Seite auswählen", wie in Abbildung 7 dargestellt. Dadurch wird das Dialogfeld Master Seite auswählen angezeigt (siehe Abbildung 8), in dem Sie die zu verwendende Master Seite auswählen können.

> [!NOTE]
> Wenn Sie Ihre ASP.NET-Website mit dem Webanwendungs Projekt Modell anstelle des Website Projekt Modells erstellt haben, wird im Dialogfeld Neues Element hinzufügen das Kontrollkästchen "Master Seite auswählen" nicht angezeigt. siehe Abbildung 7. Zum Erstellen einer Inhaltsseite bei Verwendung des Webanwendungs Projekt Modells müssen Sie anstelle der Web Form-Vorlage die Vorlage Webinhalts Formular auswählen. Nachdem Sie die Vorlage Webinhalts Formular ausgewählt und auf Hinzufügen geklickt haben, wird das gleiche in Abbildung 8 dargestellte Dialogfeld "Master Seite auswählen" angezeigt.

[![neue Inhaltsseite hinzufügen](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Abbildung 07**: Hinzufügen einer neuen Inhaltsseite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))

[![die Seite "Site. Master Master" auswählen](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Abbildung 08**: Auswählen der `Site.master` Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))

Wie das folgende deklarative Markup zeigt, enthält eine neue Inhaltsseite eine `@Page`-Direktive, die auf Ihre Master Seite zurück zeigt, und ein Inhalts Steuerelement für jede der contentplachalter-Steuerelemente der Master Seite.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> Im Abschnitt "Erstellen eines einfachen Website Layouts" in Schritt 1 wurde `ContentPlaceHolder1` in `MainContent`umbenannt. Wenn Sie die `ID` dieses contentplachalter-Steuer Elements nicht auf die gleiche Weise umbenennen, unterscheidet sich das deklarative Markup Ihrer Inhaltsseite geringfügig vom oben gezeigten Markup. Die `ContentPlaceHolderID` des zweiten Inhalts Steuer Elements gibt die `ID` des entsprechenden contentplachalter-Steuer Elements auf der Master Seite wieder.

Beim Rendern einer Inhaltsseite muss die ASP.net-Engine die Inhalts Steuerelemente der Seite mit den contentplachalter-Steuerelementen der Master Seite verbinden. Die ASP.net-Engine bestimmt die Master Seite der Inhaltsseite aus dem `MasterPageFile`-Attribut der `@Page` Direktive. Wie das obige Markup zeigt, wird diese Inhaltsseite an `~/Site.master`gebunden.

Die Master Seite verfügt über zwei contentplachalter-Steuerelemente: `head` und `MainContent`-Visual Web Developer hat zwei Inhalts Steuerelemente generiert. Jedes Inhalts Steuerelement verweist über seine `ContentPlaceHolderID`-Eigenschaft auf einen bestimmten contentplachalter.

Wenn Masterseiten gegenüber früheren Standort weiten Vorlagen Techniken glänzen, ist Ihre Entwurfszeit Unterstützung. Abbildung 9 zeigt die `About.aspx` Inhaltsseite, wenn Sie über die Designansicht von Visual Web Developer angezeigt werden. Beachten Sie, dass der Inhalt der Master Seite zwar sichtbar ist, aber ausgegraut ist und nicht geändert werden kann. Die Inhalts Steuerelemente, die den Content-Platzhaltern der Master Seite entsprechen, sind jedoch bearbeitbar. Ebenso wie bei jeder anderen ASP.NET-Seite können Sie die Schnittstelle der Inhaltsseite erstellen, indem Sie mithilfe der Quell-oder Entwurfs Ansicht websteuer Elemente hinzufügen.

[![der Entwurfs Ansicht der Inhaltsseite werden sowohl der Seiten spezifische Inhalt als auch der Master Seiten Inhalt angezeigt.](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Abbildung 09**: in der Entwurfs Ansicht der Inhaltsseite werden sowohl der Seiten spezifische Inhalt als auch der Master Seiten Inhalt angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))

### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Hinzufügen von Markup und websteuer Elementen zur Inhaltsseite

Nehmen Sie sich einen Moment Zeit, um einige Inhalte für die `About.aspx` Seite zu erstellen. Wie Sie in Abbildung 10 sehen können, habe ich eine "about the Author"-Überschrift und einige Absätze von Text eingegeben, aber auch die Möglichkeit, auch websteuer Elemente hinzuzufügen. Nachdem Sie diese Schnittstelle erstellt haben, besuchen Sie die Seite `About.aspx` über einen Browser.

[![besuchen Sie die Seite "about. aspx" über einen Browser.](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Abbildung 10**: Besuchen Sie die Seite "`About.aspx`" über einen Browser ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))

Es ist wichtig zu verstehen, dass die angeforderte Inhaltsseite und die zugehörige Master Seite vollständig auf dem Webserver zusammengefügt und als Ganzes gerendert werden. Der Browser des Endbenutzers sendet dann den resultierenden, zugezierten HTML-Code. Um dies zu überprüfen, zeigen Sie das vom Browser empfangene HTML an, indem Sie im Menü Ansicht auf Quelle klicken. Beachten Sie, dass es keine Frames oder andere spezielle Techniken zum Anzeigen zweier unterschiedlicher Webseiten in einem einzelnen Fenster gibt.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Binden einer Master Seite an eine vorhandene ASP.NET-Seite

Wie wir in diesem Schritt gesehen haben, ist das Hinzufügen einer neuen Inhaltsseite zu einer ASP.NET-Webanwendung genauso einfach wie das Aktivieren des Kontrollkästchens "Master Seite auswählen" und das Auswählen der Master Seite. Leider ist es nicht so einfach, eine vorhandene ASP.NET-Seite in eine Master Seite umzuwandeln.

Zum Binden einer Master Seite an eine vorhandene ASP.NET-Seite müssen Sie die folgenden Schritte ausführen:

1. Fügen Sie das `MasterPageFile`-Attribut zur `@Page` Direktive der ASP.NET-Seite hinzu, die auf die entsprechende Master Seite zeigt.
2. Fügen Sie Inhalts Steuerelemente für jeden der contentplatzhalter auf der Master Seite hinzu.
3. Fügen Sie den vorhandenen Inhalt der ASP.NET Seite selektiv aus, und fügen Sie ihn in die entsprechenden Inhalts Steuerelemente ein. Ich sage hier "selektiv", da die ASP.NET-Seite wahrscheinlich Markup enthält, das bereits von der Master Seite ausgedrückt wird, z. b. die `DOCTYPE`, das `<html>` Element und das Webformular.

Eine Schritt-für-Schritt-Anleitung zu diesem Vorgang und Bildschirmfotos finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/) [using Master Pages and Site Navigation](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) Tutorial. Diese Schritte werden im Abschnitt "Aktualisieren von `Default.aspx` und `DataSample.aspx` zur Verwendung der Master Seite" erläutert.

Da es viel einfacher ist, neue Inhaltsseiten zu erstellen, als vorhandene ASP.NET-Seiten in Inhaltsseiten zu konvertieren, empfiehlt es sich, bei der Erstellung einer neuen ASP.NET-Website der Website eine Master Seite hinzuzufügen. Binden Sie alle neuen ASP.NET-Seiten an diese Master Seite. Machen Sie sich keine Sorgen, wenn die anfängliche Master Seite sehr einfach oder einfach ist. Sie können die Master Seite zu einem späteren Zeitpunkt aktualisieren.

> [!NOTE]
> Beim Erstellen einer neuen ASP.NET-Anwendung fügt Visual Web Developer eine `Default.aspx` Seite hinzu, die nicht an eine Master Seite gebunden ist. Wenn Sie üben möchten, eine vorhandene ASP.NET-Seite in eine Inhaltsseite zu übernehmen, fahren Sie mit `Default.aspx`fort. Alternativ können Sie `Default.aspx` löschen und anschließend erneut hinzufügen. Aktivieren Sie dieses Mal jedoch das Kontrollkästchen "Master Seite auswählen".

## <a name="step-3-updating-the-master-pages-markup"></a>Schritt 3: Aktualisieren des Markups der Master Seite

Einer der Hauptvorteile von Masterseiten besteht darin, dass eine einzelne Master Seite verwendet werden kann, um das allgemeine Layout für zahlreiche Seiten auf der Website zu definieren. Daher ist für das Aktualisieren des Erscheinungsbild der Website das Aktualisieren einer einzelnen Datei erforderlich: die Master Seite.

Um dieses Verhalten zu veranschaulichen, aktualisieren wir unsere Master Seite so, dass Sie das aktuelle Datum in am oberen Rand der linken Spalte enthält. Fügen Sie der `leftContent` `<div>`eine Bezeichnung mit dem Namen `DateDisplay` hinzu.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Erstellen Sie als nächstes einen `Page_Load` Ereignishandler für die Master Seite, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Mit dem obigen Code wird die `Text`-Eigenschaft der Bezeichnung auf das aktuelle Datum und die aktuelle Uhrzeit festgelegt, die als Wochentag, den Namen des Monats und den zweistelligen Tag formatiert sind (siehe Abbildung 11). Nachdem Sie diese Änderung vorgenommen haben, besuchen Sie eine ihrer Inhaltsseiten. Wie in Abbildung 11 gezeigt, wird das resultierende Markup sofort aktualisiert, um die Änderung der Master Seite einzuschließen.

[![die Änderungen an der Master Seite beim Anzeigen der Seite "Inhalt" berücksichtigt werden.](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Abbildung 11**: die Änderungen an der Master Seite spiegeln sich beim Anzeigen der Seite "Inhalt" wider ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))

> [!NOTE]
> Wie in diesem Beispiel veranschaulicht, können Masterseiten serverseitige websteuer Elemente, Code und Ereignishandler enthalten.

## <a name="summary"></a>Zusammenfassung

Master Seiten ermöglichen ASP.NET-Entwicklern das Entwerfen eines konsistenten Website weiten Layouts, das leicht aktualisierbar ist. Das Erstellen von Masterseiten und den zugehörigen Inhaltsseiten ist so einfach wie das Erstellen von standardmäßigen ASP.NET-Seiten, da Visual Web Developer umfassende Entwurfszeit Unterstützung bietet.

Das Beispiel für eine Master Seite, das wir in diesem Tutorial erstellt haben, besaß zwei contentplachalter-Steuerelemente: Head und mainContent. Wir haben jedoch nur Markup für das mainContent contentplachalter-Steuerelement auf unserer Inhaltsseite angegeben. Im nächsten Tutorial betrachten wir die Verwendung mehrerer Inhalts Steuerelemente auf der Seite "Inhalt". Außerdem wird erläutert, wie Standard Markup für Inhalts Steuerelemente auf der Master Seite definiert wird, und wie Sie zwischen der Verwendung des Standard Markups, das in der Master Seite definiert ist, und der Bereitstellung eines benutzerdefinierten Markups über die Inhaltsseite umschalten können.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net for Designers: Kostenlose Entwurfsvorlagen und Anleitungen zum Erstellen von ASP.NET-Websites mithilfe von Webstandards](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Übersicht über ASP.net-Master Seiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutorials zu Cascading Stylesheets (CSS)](http://www.w3schools.com/css/default.asp)
- [Dynamisches Festlegen des Seitentitels](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Master Seiten in ASP.net](http://www.odetocode.com/articles/419.aspx)
- [Schnellstart-Tutorials für Master Seiten](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](nested-master-pages-cs.md)
> [Weiter](multiple-contentplaceholders-and-default-content-vb.md)
