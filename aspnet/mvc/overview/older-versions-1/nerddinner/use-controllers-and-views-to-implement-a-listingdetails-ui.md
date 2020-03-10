---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Verwenden von Controllern und Ansichten zum Implementieren einer Auflistung/Details-Benutzeroberfläche | Microsoft-Dokumentation
author: microsoft
description: In Schritt 4 wird gezeigt, wie Sie der Anwendung einen Controller hinzufügen, der das Modell nutzt, um Benutzern eine Daten Auflistung/Details-Navigations Darstellung bereitzustellen...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 74319fe5ea4c79b50140834349e2fdf86420cfbb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486219"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Implementieren einer Auflistungs-/Detailbenutzeroberfläche mit Controllern und Ansichten

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 4 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie Sie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellen.
> 
> In Schritt 4 wird gezeigt, wie Sie der Anwendung einen Controller hinzufügen, der das Modell nutzt, um Benutzern eine Daten Auflistung/Details-Navigations Darstellung für Abendessen auf unserer "nerddinner"-Website bereitzustellen.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-4-controllers-and-views"></a>Nerddinner Step 4: Controller und Ansichten

Bei herkömmlichen Webframe Works (klassisches ASP, PHP, ASP.net Web Forms usw.) werden eingehende URLs normalerweise Dateien auf dem Datenträger zugeordnet. Beispiel: eine Anforderung für eine URL wie "/Products.aspx" oder "/Products.php" wird möglicherweise von der Datei "Products. aspx" oder "Products. php" verarbeitet.

Webbasierte MVC-Frameworks ordnen URLs auf etwas andere Weise dem Servercode zu. Anstatt eingehende URLs zu Dateien zuzuordnen, ordnen Sie stattdessen URLs Methoden für Klassen zu. Diese Klassen werden als "controller" bezeichnet und sind dafür verantwortlich, eingehende HTTP-Anforderungen zu verarbeiten, Benutzereingaben zu verarbeiten, Daten abzurufen und zu speichern und die Antwort zu bestimmen, die an den Client zurückgesendet werden soll (HTML anzeigen, Datei herunterladen, zu einer anderen umleiten). URL usw.).

Nachdem wir nun ein grundlegendes Modell für unsere "nerddinner"-Anwendung erstellt haben, ist der nächste Schritt das Hinzufügen eines Controllers zu einer Anwendung, die den Vorteil nutzt, um Benutzern eine Daten Auflistung/Details-Navigations Darstellung für Dinner auf unserer Website bereitzustellen.

### <a name="adding-a-dinnerscontroller-controller"></a>Hinzufügen eines dinnerscontroller-Controllers

Beginnen Sie mit der rechten Maustaste auf den Ordner "Controllers" in unserem Webprojekt, und wählen Sie dann den Menübefehl **Add-&gt;Controller** aus. (Sie können diesen Befehl auch ausführen, indem Sie STRG + M, STRG + C eingeben:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Dadurch wird das Dialogfeld "Controller hinzufügen" angezeigt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Wir nennen den neuen Controller "dinnerscontroller" und klicken auf die Schaltfläche "Add" (hinzufügen). In Visual Studio wird dann eine DinnersController.cs-Datei im Verzeichnis "\controllers" hinzugefügt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Außerdem wird die neue Klasse "dinnerscontroller" im Code-Editor geöffnet.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Hinzufügen von Index ()-und Details ()-Aktionsmethoden zur dinnerscontroller-Klasse

Wir möchten unseren Besuchern die Verwendung unserer Anwendung ermöglichen, um eine Liste der anstehenden Abendessen zu durchsuchen und Ihnen die Möglichkeit zu geben, auf ein beliebiges Dinner in der Liste zu klicken, um bestimmte Details anzuzeigen. Hierzu veröffentlichen wir die folgenden URLs aus der Anwendung:

| **URL** | **Zweck** |
| --- | --- |
| *Serviert* | Anzeigen einer HTML-Liste mit anstehenden Abendessen |
| */Dinners/Details/[ID]* | Hiermit werden Details zu einem bestimmten Dinner angezeigt, das durch einen in der URL eingebetteten "ID"-Parameter angegeben ist – der der dinnerid des Dinner in der Datenbank entspricht. Beispiel:/Dinners/Details/2 würde eine HTML-Seite mit Details zum Dinner anzeigen, dessen dinnerid-Wert 2 ist. |

Wir veröffentlichen die anfänglichen Implementierungen dieser URLs, indem wir zwei öffentliche Aktionsmethoden zur dinnerscontroller-Klasse wie unten hinzufügen:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Anschließend führen wir die Anwendung "nerddinner" aus und verwenden den Browser, um Sie aufzurufen. Wenn Sie die URL *"/Dinners/"* eingeben, wird unsere *Index ()* -Methode ausgeführt, und die folgende Antwort wird zurückgesendet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Wenn Sie die URL *"/Dinners/Details/2"* eingeben, wird die " *Details ()* "-Methode ausgeführt, und die folgende Antwort wird zurückgesendet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Vielleicht Fragen Sie sich: wie hat ASP.NET MVC gelernt, unsere dinnerscontroller-Klasse zu erstellen und diese Methoden aufzurufen? Um zu verstehen, wie das Routing funktioniert.

### <a name="understanding-aspnet-mvc-routing"></a>Grundlegendes zum ASP.NET MVC-Routing

ASP.NET MVC bietet eine leistungsstarke URL-Routing-Engine, die eine hohe Flexibilität bei der Steuerung der Zuordnung von URLs zu Controller Klassen bietet. Dadurch können wir vollständig anpassen, wie ASP.NET MVC die zu erstellende Controller Klasse bestimmt, welche Methode für Sie aufgerufen werden soll. Außerdem können Sie unterschiedliche Möglichkeiten konfigurieren, wie Variablen automatisch aus URL/QueryString analysiert und als Parameter Argumente an die Methode übergeben werden können. Es bietet die Flexibilität, eine Website für SEO vollständig zu optimieren (Suchmaschinenoptimierung) und jede gewünschte URL-Struktur aus einer Anwendung zu veröffentlichen.

Standardmäßig verfügen neue ASP.NET MVC-Projekte über einen vorkonfigurierten Satz von URL-Routing Regeln, die bereits registriert sind. Dies ermöglicht es uns, auf einfache Weise mit einer Anwendung zu beginnen, ohne explizit etwas zu konfigurieren. Die standardmäßigen Routing Regel Registrierungen finden Sie in der "Application"-Klasse unserer Projekte – die Sie öffnen können, indem Sie im Stammverzeichnis des Projekts auf die Datei "Global. asax" doppelklicken:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Die standardmäßigen ASP.NET-MVC-Routing Regeln werden in der RegisterRoutes-Methode dieser Klasse registriert:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Die "Routen. MapRoute ()-Methodenaufrufe oben registrieren eine Standard Routing Regel, die eingehende URLs den Controller Klassen unter Verwendung des URL-Formats zuordnet:/{Controller}/{Action}/{ID} "– Where" controller "ist der Name der Controller Klasse, die instanziiert werden soll," action "ist der Name einer öffentlichen Methode, die für ihn aufgerufen werden soll, und" ID "ist ein optionaler Parameter, der in die URL eingebettet ist, die als Argument an die Methode übergeben werden kann. Der dritte Parameter, der an den Methoden Aufruf"MapRoute ()" übergeben wird, ist ein Satz von Standardwerten, die für die Controller-/Aktions-/ID-Werte verwendet werden, wenn Sie nicht in der URL vorhanden sind (Controller = "Home", action = "index", ID = "").

Im folgenden finden Sie eine Tabelle, die veranschaulicht, wie eine Vielzahl von URLs mithilfe der Standardrouten Regel "<em>/{Controllers}/{Action}/{ID}"</em>zugeordnet werden:

| **URL** | **Controller Klasse** | **Aktionsmethode** | **Parameter wurden erfolgreich** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details (ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Bearbeiten (ID) | ID = 5 |
| */Dinners/Create* | DinnersController | Erstellen () | N/V |
| */Dinners* | DinnersController | Index () | N/V |
| */Home* | HomeController | Index () | N/V |
| */* | HomeController | Index () | N/V |

In den letzten drei Zeilen werden die verwendeten Standardwerte (Controller = Home, Action = Index, ID = "") angezeigt. Da die "index"-Methode als Standard Aktionsname registriert ist, wenn kein Name angegeben ist, bewirken die URLs "/Dinners" und "/Home", dass die Index ()-Aktionsmethode für Ihre Controller Klassen aufgerufen wird. Da der "Home"-Controller als Standard Controller registriert ist, wenn er nicht angegeben ist, bewirkt die URL "/", dass der HomeController erstellt und die Index ()-Aktionsmethode darauf aufgerufen wird.

Wenn Sie diese standardmäßigen URL-Routing Regeln nicht kennen, ist die gute Nachricht, dass Sie leicht zu ändern sind. Bearbeiten Sie Sie einfach in der oben aufgeführten Methode "RegisterRoutes". Für unsere "nerddinner"-Anwendung werden jedoch keine Standardregeln für das URL-Routing geändert – wir verwenden Sie stattdessen einfach unverändert.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Verwenden des dinnerrepository aus unserem dinnerscontroller

Nun ersetzen wir unsere aktuelle Implementierung der "Index ()"-und "Details ()"-Aktionsmethoden des dinnerscontrollers durch Implementierungen, die unser Modell verwenden.

Wir verwenden die zuvor erstellten dinnerrepository-Klasse, um das Verhalten zu implementieren. Wir fügen zunächst eine "using"-Anweisung hinzu, die auf den Namespace "nerddinner. Models" verweist, und deklarieren dann eine Instanz des dinnerrepository als Feld in unserer dinnercontroller-Klasse.

Weiter unten in diesem Kapitel wird das Konzept der "Abhängigkeitsinjektion" eingeführt, und es wird eine weitere Möglichkeit für unsere Controller zum Abrufen eines Verweises auf ein dinnerrepository angezeigt, das eine bessere Komponenten Überprüfung ermöglicht – aber zurzeit wird nur eine Instanz des dinnerrepository erstellt. Inline wie unten beschrieben.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Nun sind wir bereit, eine HTML-Antwort mit den abgerufenen Datenmodell Objekten zu generieren.

### <a name="using-views-with-our-controller"></a>Verwenden von Ansichten mit unserem Controller

Obwohl es möglich ist, Code in unsere Aktionsmethoden zu schreiben, um HTML zusammenzustellen, und dann die *Response. Write ()* -Hilfsmethode verwenden, um Sie an den Client zurückzusenden, wird diese Vorgehensweise schnell zu unhandlich. Ein wesentlich besserer Ansatz ist, dass wir nur Anwendungs-und Daten Logik innerhalb unserer dinnerscontroller-Aktionsmethoden ausführen und dann die zum Renderingvorgang der HTML-Antwort erforderlichen Daten an eine separate "View"-Vorlage übergeben, die für die Ausgabe der HTML-Darstellung zuständig ist. davon. Wie wir in Kürze sehen werden, ist eine "Ansichts Vorlage" eine Textdatei, die in der Regel eine Kombination aus HTML-Markup und eingebettetem Renderingcode enthält.

Die Trennung unserer Controller Logik von unserem Ansichts Rendering bringt einige große Vorteile mit sich. Insbesondere hilft es, eine klare "Trennung von Belangen" zwischen dem Anwendungscode und dem Formatierungs-/Renderingcode der Benutzeroberfläche zu erzwingen. Dadurch wird es wesentlich einfacher, Anwendungslogik isoliert von der Benutzeroberflächen-Renderinglogik zu testen. Es vereinfacht das spätere Ändern der Benutzeroberflächen-renderingvorlagen, ohne dass Änderungen am Anwendungscode vorgenommen werden müssen. Außerdem können Entwickler und Designer die Zusammenarbeit an Projekten vereinfachen.

Wir können unsere dinnerscontroller-Klasse aktualisieren, um anzugeben, dass wir eine Ansichts Vorlage verwenden möchten, um eine HTML-UI-Antwort zu senden, indem wir die Methoden Signaturen unserer zwei Aktionsmethoden von einem Rückgabetyp "void" ändern, sodass der Rückgabetyp "action result" lautet. Wir können dann die *View ()* -Hilfsmethode für die Controller-Basisklasse aufzurufen, um ein "ViewResult"-Objekt zurückzugeben, wie unten gezeigt:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Die Signatur der Hilfsmethode *View ()* , die wir oben verwenden, sieht wie folgt aus:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Der erste Parameter der Hilfsmethode *View ()* ist der Name der Ansichts Vorlagen Datei, die zum Rendering der HTML-Antwort verwendet werden soll. Der zweite Parameter ist ein Modell Objekt, das die Daten enthält, die die Ansichts Vorlage zum Rendering der HTML-Antwort benötigt.

Innerhalb unserer Index ()-Aktionsmethode rufen wir die *View ()* -Hilfsmethode auf und zeigen an, dass eine HTML-Auflistung von Abendessen mithilfe der Ansichts Vorlage "index" gerenstet werden soll. Wir übergeben der Ansichts Vorlage eine Sequenz von Dinner-Objekten, um die Liste zu generieren:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Innerhalb unserer Details ()-Aktionsmethode versuchen wir, ein Dinner-Objekt mit der in der URL bereitgestellten ID abzurufen. Wenn ein gültiges Dinner gefunden wird, wird die " *View ()* "-Hilfsmethode aufgerufen, die angibt, dass eine "Details"-Ansichts Vorlage zum Rendering des abgerufenen Dinner-Objekts verwendet werden soll. Wenn ein ungültiges Dinner angefordert wird, wird eine hilfreiche Fehlermeldung angezeigt, die besagt, dass das Dinner nicht vorhanden ist, indem eine "NotFound"-Ansichts Vorlage (und eine überladene Version der *View ()* -Hilfsmethode, die nur den Vorlagen Namen annimmt) verwendet wird:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Nun implementieren wir die Ansichts Vorlagen "NotFound", "Details" und "index".

### <a name="implementing-the-notfound-view-template"></a>Implementieren der "NotFound"-Ansichts Vorlage

Wir beginnen mit der Implementierung der "NotFound"-Ansichts Vorlage – diese zeigt eine benutzerfreundliche Fehlermeldung an, die anzeigt, dass das angeforderte Dinner nicht gefunden werden kann.

Wir erstellen eine neue Ansichts Vorlage durch Positionieren des Text Cursors innerhalb einer Controller Aktionsmethode. Klicken Sie dann mit der rechten Maustaste, und wählen Sie den Menübefehl "Ansicht hinzufügen" aus. (Sie können diesen Befehl auch ausführen, indem Sie STRG + M, STRG + V eingeben:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" wie unten angezeigt. Standardmäßig füllt das Dialogfeld den Namen der zu erstellenden Ansicht vorab mit dem Namen der Aktionsmethode auf, in der sich der Cursor beim Starten des Dialog Felds befand (in diesem Fall "Details"). Da wir zuerst die "NotFound"-Vorlage implementieren möchten, wird dieser Ansichts Name überschrieben und stattdessen auf "NotFound" festgelegt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, erstellt Visual Studio eine neue "NotFound. aspx"-Ansichts Vorlage für uns im Verzeichnis "\views\dinner" (das ebenfalls erstellt wird, wenn das Verzeichnis nicht bereits vorhanden ist):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Außerdem wird die neue "NotFound. aspx"-Ansichts Vorlage im Code-Editor geöffnet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Vorlagen anzeigen standardmäßig haben zwei "Inhaltsbereiche", in denen wir Inhalt und Code hinzufügen können. Mit dem ersten können wir den "Titel" der zurückgesendeten HTML-Seite anpassen. Mit dem zweiten können wir den "Hauptinhalt" der zurückgesendeten HTML-Seite anpassen.

Zum Implementieren unserer "NotFound"-Ansichts Vorlage fügen wir einige grundlegende Inhalte hinzu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Wir können es dann im Browser ausprobieren. Um dies zu erreichen, fordern wir die URL *"/Dinners/Details/9999"* an. Dies bezieht sich auf ein Dinner, das zurzeit nicht in der Datenbank vorhanden ist, und bewirkt, dass unsere "dinnerscontroller. Details ()"-Aktionsmethode die "NotFound"-Ansichts Vorlage gerenppt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Im obigen Screenshot bemerken Sie, dass unsere einfache Ansichts Vorlage eine Reihe von HTML geerbt hat, die den Hauptinhalt auf dem Bildschirm umgibt. Dies liegt daran, dass unsere View-Vorlage eine Vorlage "Master Seite" verwendet, mit der wir ein konsistentes Layout für alle Ansichten auf der Website anwenden können. In einem späteren Teil dieses Tutorials wird erläutert, wie Masterseiten mehr funktionieren.

### <a name="implementing-the-details-view-template"></a>Implementieren der Ansichts Vorlage "Details"

Nun implementieren wir die Ansichts Vorlage "Details" – die HTML für ein einzelnes Dinner-Modell generiert.

Dazu positionieren wir den Textcursor innerhalb der Detail Aktionsmethode, klicken mit der rechten Maustaste und wählen den Menübefehl "Add View" (Ansicht hinzufügen) aus (oder drücken STRG + M, STRG + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt. Wir behalten den Standard Ansichts Namen bei ("Details"). Im Dialogfeld Wählen Sie auch das Kontrollkästchen "eine stark typisierte Ansicht erstellen" aus, und wählen Sie (in der Dropdown Liste "ComboBox") den Namen des Modell Typs aus, den wir vom Controller an die Ansicht übergeben. Für diese Ansicht übergeben wir ein Dinner-Objekt (der voll qualifizierte Name für diesen Typ lautet: "nerddinner. Models. Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Anders als bei der vorherigen Vorlage, bei der wir uns entschieden haben, eine "leere Ansicht" zu erstellen, legen wir diesmal fest, dass die Ansicht mithilfe einer "Details"-Vorlage automatisch "Gerüstbau" wird. Dies können Sie angeben, indem Sie im obigen Dialogfeld "Inhalt anzeigen" ändern.

"Gerüstbau" generiert eine anfängliche Implementierung unserer Detail Ansichts Vorlage basierend auf dem Dinner-Objekt, das an Sie übergeben wird. Dies bietet eine einfache Möglichkeit, um schnell mit der Implementierung der Ansichts Vorlage zu beginnen.

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, erstellt Visual Studio eine neue "Details. aspx"-Ansichts Vorlagen Datei für uns im Verzeichnis "\views\dinner":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Außerdem wird die neue Ansichts Vorlage "Details. aspx" im Code-Editor geöffnet. Sie enthält eine anfängliche Gerüst Implementierung einer Detailansicht, die auf einem Dinner-Modell basiert. Die Gerüstbau-Engine verwendet die .NET-Reflektion, um die öffentlichen Eigenschaften, die in der Klasse verfügbar gemacht werden, zu untersuchen, und fügt entsprechend den gefundenen Typen passenden Inhalt hinzu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Wir können die URL *"/Dinners/Details/1"* anfordern, um zu sehen, wie diese "Details"-Implementierung im Browser aussieht. Wenn Sie diese URL verwenden, wird eines der Abendessen angezeigt, das wir bei der ersten Erstellung manuell zur Datenbank hinzugefügt haben:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Dies bringt uns schnell in Betrieb und bietet eine anfängliche Implementierung der Ansicht "Details. aspx". Dann können wir Sie optimieren, um die Benutzeroberfläche an unsere Zufriedenheit anzupassen.

Wenn wir uns die Vorlage "Details. aspx" genauer ansehen, werden wir feststellen, dass Sie statisches HTML und eingebetteten Renderingcode enthält. &lt;%%&gt; Code-Nuggets führen Code aus, wenn die Ansichts Vorlage gerendert wird, und &lt;% =%&gt; Code-Nuggets führen den darin enthaltenen Code aus und Rendern das Ergebnis im Ausgabestream der Vorlage.

Wir können Code in unserer Ansicht schreiben, der auf das "Dinner"-Modell Objekt zugreift, das mit einer stark typisierten "Model"-Eigenschaft vom Controller übermittelt wurde. Visual Studio bietet uns vollständigen Code-IntelliSense, wenn Sie im Editor auf diese Eigenschaft "Modell" zugreifen:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Wir nehmen einige Anpassungen vor, damit die Quelle für die endgültige Detail Ansichts Vorlage wie folgt aussieht:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Wenn Sie erneut auf die URL *"/Dinners/Details/1"* zugreifen, wird Sie nun wie folgt dargestellt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementieren der Ansichts Vorlage "index"

Nun implementieren Sie die "index"-Ansichts Vorlage – die eine Liste der anstehenden Abendessen generiert. Zu diesem Zweck positionieren wir den Textcursor innerhalb der Index Aktionsmethode. Klicken Sie dann mit der rechten Maustaste, und wählen Sie den Menübefehl "Ansicht hinzufügen" aus (oder drücken Sie STRG + M, STRG + V).

Im Dialogfeld "Ansicht hinzufügen" behalten Sie die Ansichts Vorlage mit dem Namen "index" bei und aktivieren das Kontrollkästchen "eine stark typisierte Ansicht erstellen". Diesmal wird automatisch eine "Listen"-Ansichts Vorlage generiert, und "nerddinner. Models. Dinner" wird als der an die Ansicht übergebenen Modelltyp ausgewählt (da wir angegeben haben, dass wir ein "List"-Gerüst erstellen, wird im Dialogfeld "Ansicht hinzufügen" angenommen, dass es sich um übergeben einer Sequenz von Dinner-Objekten von unserem Controller an die Ansicht):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, erstellt Visual Studio im Verzeichnis "\views\dinner" eine neue "index. aspx"-Ansichts Vorlagen Datei für uns. Es wird eine anfängliche Implementierung als "Gerüstbau" erstellt, die eine HTML-Tabelle mit den Abendessen bereitstellt, die an die Ansicht übergeben werden.

Wenn wir die Anwendung ausführen und auf die URL *"/Dinners/"* zugreifen, wird die Liste mit den Abendessen wie folgt dargestellt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Die obige Tabellen Lösung bietet uns ein Raster ähnliches Layout unserer Dinner Data – Dies ist nicht ganz alles, was wir für unsere kundenseitige Dinner-Auflistung wünschen. Wir können die Ansichts Vorlage "index. aspx" aktualisieren und ändern, um weniger Datenspalten aufzulisten, und ein &lt;UL&gt;-Element verwenden, um Sie anstelle einer Tabelle zu verwenden, indem Sie den folgenden Code verwenden:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Wir verwenden das Schlüsselwort "var" in der obigen foreach-Anweisung, während wir über jedes Dinner in unserem Modell eine Schleife durchlaufen. Wenn Sie mit C# 3,0 nicht vertraut sind, denken Sie möglicherweise, dass die Verwendung von "var" das Dinner-Objekt spät gebunden ist. Es bedeutet vielmehr, dass der Compiler den Typrückschluss für die stark typisierte "Model"-Eigenschaft verwendet (die den Typ "IEnumerable&lt;Dinner&gt;" hat) und die lokale "Dinner"-Variable als Dinner Type kompiliert – was bedeutet, dass wir in Code Blöcken vollständige IntelliSense-und Kompilierungszeit-Überprüfung dafür erhalten:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Wenn die */Dinners* -URL in unserem Browser auf "Aktualisieren" zeigt, sieht unsere aktualisierte Ansicht nun wie folgt aus:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Dies sieht besser aus – aber noch nicht ganz. Der letzte Schritt besteht darin, Endbenutzern die Möglichkeit zu geben, auf einzelne Dinner in der Liste zu klicken und Details zu Ihnen anzuzeigen. Dies wird durch das Rendern von HTML-Hyperlink-Elementen implementiert, die mit der Details-Aktionsmethode auf unserem dinnerscontroller verknüpft sind.

Wir können diese Hyperlinks in unserer Index Sicht auf eine von zwei Arten generieren. Der erste besteht darin, die HTML-&lt;ein&gt; Elemente wie unten beschrieben manuell zu erstellen. dabei werden &lt;%%&gt; Blöcke innerhalb der &lt;ein&gt; HTML-Element eingebettet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Ein alternativer Ansatz, den wir verwenden können, ist die Verwendung der integrierten "HTML. Action Link ()"-Hilfsmethode in ASP.NET MVC, die das programmgesteuerte Erstellen eines HTML-&lt;ein&gt; Element unterstützt, das mit einer anderen Aktionsmethode auf einem Controller verknüpft ist:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Der erste Parameter für den HTML-Code. die Hilfsmethode "action Link ()" ist der anzuzeigende Linktext (in diesem Fall der Titel des Dinner), der zweite Parameter ist der Controller Aktionsname, zu dem der Link generiert werden soll (in diesem Fall die Detail Methode), und der dritte Parameter ist ein Satz von Parametern, die an die Aktion gesendet werden sollen In diesem Fall geben wir den Parameter "ID" des Dinner an, zu dem Sie eine Verknüpfung herstellen möchten, und da die Standard-URL-Routing Regel in ASP.NET MVC "{Controller}/{Action}/{ID}" lautet, generiert die HTML. Action Link ()-Hilfsmethode die folgende Ausgabe:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Für unsere Index. aspx-Ansicht verwenden wir den Ansatz der Hilfsmethode "HTML. Action Link ()" und haben jedes Dinner in der Liste mit der entsprechenden Details-URL verknüpft:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Wenn wir nun die */Dinners* -URL erreichen, sieht die Dinner-Liste wie folgt aus:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Wenn wir auf eines der Abendessen in der Liste klicken, werden wir navigieren, um Details dazu anzuzeigen:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Konventionen-basierte Benennung und die Verzeichnisstruktur "\views"

ASP.NET MVC-Anwendungen verwenden beim Auflösen von Ansichts Vorlagen standardmäßig eine Konvention-basierte Verzeichnis Benennungs Struktur. Dies ermöglicht es Entwicklern, einen Location-Path nicht vollständig zu qualifizieren, wenn auf Sichten aus einer Controller Klasse verwiesen wird. Standardmäßig sucht ASP.NET MVC nach der Ansichts Vorlagen Datei innerhalb des Verzeichnisses * \views\[ControllerName]\* unterhalb der Anwendung.

Wir arbeiten z. b. an der dinnerscontroller-Klasse –, die explizit auf drei Ansichts Vorlagen verweist: "index", "Details" und "NotFound". ASP.NET MVC sucht standardmäßig im Verzeichnis " *\views\dinner* " unterhalb des Stamm Verzeichnisses der Anwendung nach diesen Ansichten:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Beachten Sie, dass im Projekt derzeit drei Controller Klassen vorhanden sind (dinnerscontroller, HomeController und AccountController – die letzten beiden wurden bei der Erstellung des Projekts standardmäßig hinzugefügt), und es gibt drei Unterverzeichnisse (jeweils eine für jede Controller) im Verzeichnis "\views".

Sichten, auf die von den Konten "Home" und "Accounts" verwiesen wird, lösen automatisch Ihre Ansichts Vorlagen aus den Verzeichnissen *\views\home* und *\views\account* Das Unterverzeichnis " *\views\shared* " bietet eine Möglichkeit zum Speichern von Ansichts Vorlagen, die auf mehreren Controllern innerhalb der Anwendung wieder verwendet werden. Wenn ASP.NET MVC versucht, eine Ansichts Vorlage aufzulösen, prüft Sie zuerst das Verzeichnis *\views\[Controller]* , und wenn die Ansichts Vorlage nicht gefunden werden kann, wird Sie im Verzeichnis " *\views\shared* " angezeigt.

Beim Benennen einzelner Ansichts Vorlagen wird empfohlen, dass die Ansichts Vorlage den gleichen Namen wie die Aktionsmethode hat, die das Rendering bewirkt hat. Über der "index"-Aktionsmethode wird z. b. die "index"-Sicht verwendet, um das Ansichts Ergebnis zu erzeugen, und die "Details"-Aktionsmethode verwendet die Ansicht "Details", um die Ergebnisse zu erzeugen. Dies erleichtert es, schnell zu sehen, welche Vorlage den einzelnen Aktionen zugeordnet ist.

Entwickler müssen den Namen der Ansichts Vorlage nicht explizit angeben, wenn die Ansichts Vorlage den gleichen Namen wie die Aktionsmethode hat, die auf dem Controller aufgerufen wird. Stattdessen können wir das Modell Objekt einfach an die "View ()"-Hilfsmethode übergeben (ohne den Ansichts Namen anzugeben), und ASP.NET MVC gibt automatisch an, dass die Ansichts Vorlage *\views\[ControllerName]\[Aktionsname]* auf dem Datenträger zum Renderingvorgang verwendet werden soll.

Dadurch können wir den Controller Code etwas bereinigen und vermeiden, dass der Name zweimal in unserem Code duplizieren wird:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Der obige Code ist alles, was erforderlich ist, um eine schöne Dinner-Auflistung/Details für die Website zu implementieren.

#### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt eine schöne Dinner Browsing-Darstellung erstellt.

Nun aktivieren wir CRUD (Create, Read, Update, DELETE) Data Form-Bearbeitungs Unterstützung.

> [!div class="step-by-step"]
> [Zurück](build-a-model-with-business-rule-validations.md)
> [Weiter](provide-crud-create-read-update-delete-data-form-entry-support.md)
