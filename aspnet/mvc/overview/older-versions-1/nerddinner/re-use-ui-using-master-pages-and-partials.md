---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Erneutes Verwenden der Benutzeroberfläche mithilfe von Master Seiten und partialen Microsoft-Dokumentation
author: microsoft
description: In Schritt 7 sehen Sie, wie wir das "trockene Prinzip" innerhalb unserer Ansichts Vorlagen anwenden können, um Code Duplikate mithilfe von partiellen Ansichts Vorlagen und Masterseiten zu vermeiden.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0b17cb6ac14b7f187bf1f175097a37907689d46e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468723"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Wiederverwenden der Benutzeroberfläche mit Masterseiten und Teilausführungen

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 7 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> Schritt 7 zeigt, wie wir das "trockene Prinzip" innerhalb unserer Ansichts Vorlagen anwenden können, um Code Duplikate mithilfe von partiellen Ansichts Vorlagen und Masterseiten zu vermeiden.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-7-partials-and-master-pages"></a>Nerddinner STEP 7: partiale und Master Seiten

Zu den Entwurfs Philosophien ASP.NET MVC gehört das Prinzip "keine Wiederholung selbst" (häufig als "trocken" bezeichnet). Ein trockener Entwurf hilft dabei, die Duplizierung von Code und Logik zu vermeiden, wodurch Anwendungen schneller erstellt und leichter zu verwalten sind.

Wir haben bereits gesehen, wie das Dry-Prinzip in einigen unserer nerddinner-Szenarios angewendet wurde. Einige Beispiele: unsere Validierungs Logik wird in unserer Modell Ebene implementiert, sodass Sie sowohl in Bearbeitungs-als auch in Erstellungs Szenarien in unserem Controller erzwungen werden kann. Wir verwenden die "NotFound"-Ansichts Vorlage in den Aktionsmethoden "Edit", "Details" und "Delete" erneut. Wir verwenden ein Konvention-Benennungs Muster mit unseren Ansichts Vorlagen, sodass der Name nicht explizit angegeben werden muss, wenn die View ()-Hilfsmethode aufgerufen wird. und wir verwenden die Klasse "dinnerformviewmodel" für Bearbeitungs-und Aktions Szenarien erneut.

Sehen wir uns nun an, wie wir das "trockene Prinzip" innerhalb unserer Ansichts Vorlagen anwenden können, um die Code Duplizierung ebenfalls zu vermeiden.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Erneutes Besuchen unserer Vorlagen zum Bearbeiten und Erstellen von Ansichten

Zurzeit verwenden wir zwei verschiedene Ansichts Vorlagen – "Edit. aspx" und "Create. aspx" –, um die Dinner Form-Benutzeroberfläche anzuzeigen. Bei einem schnellen visuellen Vergleich werden Sie hervorgehoben, wie ähnlich Sie sind. Im folgenden sehen Sie, wie das Formular erstellen aussieht:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Das Formular "Bearbeiten" sieht wie folgt aus:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Es gibt keinen großen Unterschied? Anders als der Titel und der Header Text sind das Formularlayout und die Eingabe Steuerelemente identisch.

Wenn wir die Ansichts Vorlagen "Edit. aspx" und "Create. aspx" öffnen, werden Sie feststellen, dass Sie ein identisches Formularlayout und Eingabe Steuerungs Code enthalten. Diese Duplizierung bedeutet, dass wir Änderungen am Ende jedes Mal zweimal vornehmen müssen, wenn wir eine neue Dinner-Eigenschaft einführen oder ändern, was nicht gut ist.

### <a name="using-partial-view-templates"></a>Verwenden von partiellen Ansichts Vorlagen

ASP.NET MVC unterstützt die Möglichkeit, partielle Ansichts Vorlagen zu definieren, die verwendet werden können, um die Ansichts-Renderinglogik für einen Teilbereich einer Seite zu kapseln. "Partiale" bieten eine nützliche Möglichkeit zum einmaligen Definieren der Ansichts-Renderinglogik und zur anschließenden erneuten Verwendung an mehreren Stellen in einer Anwendung.

Wir können eine Teil Ansichts Vorlage mit dem Namen "dinnerform. ascx" erstellen, die das Formularlayout und die Eingabeelemente kapselt, die beides gemeinsam sind, um die "Bereinigung" der Ansichts Vorlage "Edit. aspx" und "Create. aspx" zu unterstützen. Hierzu klicken Sie mit der rechten Maustaste auf das Verzeichnis "/Views/Dinners" und wählen den Menübefehl "Add-&gt;View" aus:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt. Wir benennen die neue Ansicht, die Sie als "dinnerform" erstellen möchten, aktivieren das Kontrollkästchen "partielle Ansicht erstellen" im Dialogfeld und geben an, dass wir ihr eine dinnerformviewmodel-Klasse übergeben werden:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, erstellt Visual Studio im Verzeichnis "\views\dinner" eine neue "dinnerform. ascx"-Ansichts Vorlage für uns.

Anschließend können Sie den doppelten Formularlayout-/eingabesteuerungscode aus den Ansichts Vorlagen "Edit. aspx/Create. aspx" in unsere neue Teil Ansichts Vorlage "dinnerform. ascx" Kopieren und einfügen:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Anschließend können wir unsere Vorlagen zum Bearbeiten und Erstellen von Ansichten aktualisieren, um die Teil Vorlage "dinnerform" aufzurufen und die Formular Duplizierung auszuschließen. Dies erreichen Sie, indem Sie "HTML. renderpartial" ("dinnerform") innerhalb unserer Ansichts Vorlagen aufrufen:

##### <a name="createaspx"></a>Create. aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit. aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Sie können den Pfad der partiellen Vorlage explizit qualifizieren, wenn Sie HTML. renderpartial aufrufen (z. b.: ~ views/Dinner/dinnerform. ascx "). In unserem obigen Code nutzen wir jedoch das auf Konventionen basierende Benennungs Muster innerhalb von ASP.NET MVC und legen einfach "dinnerform" als Namen der zu Rendering enden partiellen Angabe fest. Wenn dies der Fall ist ASP.net wird MVC zuerst im Verzeichnis für die Konventionen auf Konventionen angezeigt (für dinnerscontroller wäre dies/views/Dinners). Wenn die partielle Vorlage dort nicht gefunden wird, wird Sie im Verzeichnis "/Views/Shared" angezeigt.

Wenn HTML. renderpartial () nur mit dem Namen der partiellen Ansicht aufgerufen wird, übergibt ASP.NET MVC an die partielle Ansicht dieselben Model-und ViewData-Wörterbuch Objekte, die von der aufrufenden Ansichts Vorlage verwendet werden. Alternativ gibt es überladene Versionen von HTML. renderpartial (), die es Ihnen ermöglichen, ein alternatives Modell Objekt und/oder ViewData-Wörterbuch für die zu verwendende Teilansicht zu übergeben. Dies ist nützlich für Szenarien, in denen nur eine Teilmenge des vollständigen Modells/ViewModel übergeben werden soll.

| **Seitiges Thema: Warum &lt;%%&gt; anstelle von &lt;% =%&gt;?** |
| --- |
| Eine der kleinen Probleme, die Sie möglicherweise mit dem obigen Code bemerkt haben, besteht darin, dass wir einen &lt;%%&gt;-Block anstelle eines &lt;% =%&gt;-Blocks verwenden, wenn Sie HTML. renderpartial () aufrufen. &lt;% =%&gt; Blöcke in ASP.net geben an, dass ein Entwickler einen angegebenen Wert renderrenderz (z. b. &lt;% = "Hello"%&gt; würde "Hello" renderz renderz). &lt;%%&gt; blockiert stattdessen, dass der Entwickler Code ausführen möchte, und dass jede gerenderte Ausgabe darin explizit erfolgen muss (z. b.: &lt;% Response. Write ("Hello")%&gt;. Der Grund für die Verwendung eines &lt;%%&gt; Blocks mit unserem HTML. renderpartial-Code oben liegt daran, dass die HTML. renderpartial ()-Methode keine Zeichenfolge zurückgibt, sondern den Inhalt direkt in den Ausgabestream der aufrufenden Ansichts Vorlage ausgibt. Dies erfolgt aus Leistungsgründen, und dadurch entfällt die Notwendigkeit, ein (potenziell sehr großes) temporäres Zeichen folgen Objekt zu erstellen. Dies reduziert die Speicherauslastung und verbessert den gesamten Anwendungs Durchsatz. Ein häufiger Fehler bei der Verwendung von HTML. renderpartial () besteht darin, das Semikolon am Ende des Aufrufes zu vergessen, wenn es sich innerhalb eines &lt;%%&gt; Blocks befindet. Der folgende Code verursacht z. b. einen Compilerfehler: &lt;% HTML. renderpartial ("dinnerform")%&gt; Sie müssen stattdessen Folgendes schreiben: &lt;% HTML. renderpartial ("dinnerform"); %&gt; Dies liegt daran, dass &lt;%%&gt; Blöcke eigenständige Code Anweisungen sind und die Verwendung C# von Code Anweisungen mit einem Semikolon beendet werden muss. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Verdeutlichen von Code mithilfe von partiellen Ansichts Vorlagen

Wir haben die Teil Ansichts Vorlage "dinnerform" erstellt, um eine Duplizierung der Ansichts Renderinglogik an mehreren Stellen zu vermeiden Dies ist der häufigste Grund für das Erstellen von partiellen Ansichts Vorlagen.

Manchmal ist es auch sinnvoll, partielle Ansichten zu erstellen, auch wenn Sie nur an einer einzigen Stelle aufgerufen werden. Sehr komplizierte Ansichts Vorlagen können häufig viel leichter lesbar werden, wenn Ihre Ansichts-Renderinglogik in eine oder mehrere benannte partielle Vorlagen extrahiert und partitioniert wird.

Betrachten Sie beispielsweise den folgenden Code Ausschnitt aus der Datei "Site. Master" in unserem Projekt (in Kürze). Der Code ist relativ einfach zu lesen – teilweise, weil die Logik zum Anzeigen eines Anmelde-/Abmelde-Links oben rechts auf dem Bildschirm innerhalb des "logonusercontrol"-Elements gekapselt ist:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Wenn Sie versuchen, das HTML-/codemarkup innerhalb einer Ansichts Vorlage besser zu verstehen, sollten Sie sich darüber im klaren sein, ob es nicht klarer wäre, wenn einige davon extrahiert und in wohl benannte Teilansichten umgestaltet wurden.

### <a name="master-pages"></a>Masterseiten

Zusätzlich zur Unterstützung von Teilansichten unterstützt ASP.NET MVC auch die Möglichkeit zum Erstellen von Vorlagen für Masterseiten, mit denen das allgemeine Layout und das HTML-Format einer Website definiert werden können. Inhalts Platzhalter-Steuerelemente können der Master Seite hinzugefügt werden, um verteilbare Bereiche zu identifizieren, die überschrieben oder durch Sichten "ausgefüllt" werden können. Dies ermöglicht eine sehr effektive (und trockene) Methode, ein gängiges Layout auf eine Anwendung anzuwenden.

Standardmäßig wird neuen ASP.NET-MVC-Projekten automatisch eine Vorlage für Masterseiten hinzugefügt. Diese Master Seite hat den Namen "Site. Master" und befindet sich im Ordner "\views\shared\":

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Die Standarddatei "Site. Master" sieht wie folgt aus. Er definiert den äußeren HTML-Code der Site sowie ein Menü für die Navigation im oberen Bereich. Sie enthält zwei ersetzbare Inhalts Platzhalter-Steuerelemente – eine für den Titel und die andere für den Ort, an dem der primäre Inhalt einer Seite ersetzt werden soll:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Alle Ansichts Vorlagen, die wir für die "nerddinner"-Anwendung erstellt haben ("List", "Details", "Edit", "Create", "NotFound" usw.), basieren auf dieser Site. Master-Vorlage. Dies wird über das Attribut "MasterPageFile" angegeben, das standardmäßig der obersten &lt;% @ Page%&gt;-Direktive hinzugefügt wurde, als wir die Ansichten mithilfe des Dialog Felds "Ansicht hinzufügen" erstellt haben:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Dies bedeutet, dass wir den Inhalt von Site. Master ändern und die Änderungen automatisch anwenden und verwenden können, wenn wir unsere Ansichts Vorlagen Rendering.

Aktualisieren Sie den Header Abschnitt von "Site. Master", sodass der Header der Anwendung "nerddinner" anstelle von "meine MVC-Anwendung" lautet. Wir aktualisieren auch das Navigationsmenü, sodass die erste Registerkarte "Find a Dinner" (wird von der "Index ()"-Aktionsmethode von HomeController behandelt) ist, und wir fügen eine neue Registerkarte mit dem Namen "Host a Dinner" (behandelt von der Create ()-Aktionsmethode von dinnerscontroller) hinzu:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Wenn wir die Datei "Site. Master" Speichern und unseren Browser aktualisieren, werden die Header Änderungen in allen Ansichten innerhalb unserer Anwendung angezeigt. Beispiel:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Und mit der */Dinners/Edit/[ID]* -URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Nächster Schritt

Partiale und Masterseiten bieten sehr flexible Optionen, mit denen Sie Ansichten ordnungsgemäß organisieren können. Sie werden feststellen, dass Sie Sie dabei unterstützen, das Duplizieren von Inhalt und Code der Ansicht zu vermeiden und ihre Ansichts Vorlagen leichter zu lesen und zu verwalten.

Sehen wir uns jetzt das Auflistungs Szenario an, das wir zuvor erstellt haben

> [!div class="step-by-step"]
> [Zurück](use-viewdata-and-implement-viewmodel-classes.md)
> [Weiter](implement-efficient-data-paging.md)
