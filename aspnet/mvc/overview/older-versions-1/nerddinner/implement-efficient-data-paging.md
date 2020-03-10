---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementieren von effizientes Datenpaging | Microsoft-Dokumentation
author: microsoft
description: In Schritt 8 wird gezeigt, wie Sie der/Dinners-URL Paging-Unterstützung hinzufügen, sodass anstelle von 1000 Abendessen gleichzeitig 10 anstehende Dinner-Vorgänge angezeigt werden...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2d9a0dba381b71755ac626f76d52bc5bcb434447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486483"
---
# <a name="implement-efficient-data-paging"></a>Implementieren einer effizienten Datenauslagerung

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 8 für ein kostenloses ["nerddinner"-Anwendungs Lernprogramm](introducing-the-nerddinner-tutorial.md) , das die Erstellung einer kleinen, aber abgeschlossenen Webanwendung mithilfe von ASP.NET MVC 1 erläutert.
> 
> In Schritt 8 erfahren Sie, wie Sie der/Dinners-URL Paging-Unterstützung hinzufügen, sodass anstelle von 1000 Abendessen gleichzeitig 10 anstehende Dinner-Vorgänge angezeigt werden, und Endbenutzern die Möglichkeit geben, die gesamte Liste in einer SEO-freundlichen Weise zurück-und vorwärts zuleiten.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-8-paging-support"></a>Nerddinner Step 8: Paging-Unterstützung

Wenn unsere Website erfolgreich ist, erhalten Sie Tausende von bevorstehenden Abendessen. Wir müssen sicherstellen, dass unsere Benutzeroberfläche skaliert wird, um alle diese Dinner-Elemente zu verarbeiten, und Benutzern das Durchsuchen ermöglicht. Um dies zu ermöglichen, fügen wir der */Dinners* -URL Paging-Unterstützung hinzu, sodass anstelle von 1000 Abendessen gleichzeitig 10 anstehende Dinner-Vorgänge angezeigt werden und es Endbenutzern ermöglicht wird, die gesamte Liste in einer SEO-freundlichen Weise zurück-und vorwärts zuleiten.

### <a name="index-action-method-recap"></a>Index ()-Aktionsmethode (Recap)

Die "Index ()"-Aktionsmethode in der Klasse "dinnerscontroller" sieht zurzeit wie folgt aus:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Wenn eine Anforderung an die */Dinners* -URL gerichtet ist, wird eine Liste aller bevorstehenden Abendessen abgerufen, und anschließend wird eine Liste aller bevorstehenden Dinner-Anweisung gerendert:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Grundlegendes zu iquerable&lt;t&gt;

*Iquervable&lt;t&gt;* ist eine Schnittstelle, die als Teil von .NET 3,5 mit LINQ eingeführt wurde. Sie ermöglicht leistungsstarke "verzögerte Ausführungs Szenarien", die wir nutzen können, um Paging-Unterstützung zu implementieren.

In unserem dinnerrepository wird von der findupcomingdinners ()-Methode eine iquerable&lt;Dinner&gt;-Sequenz zurückgegeben:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Das von der findupcomingdinners ()-Methode zurückgegebene&lt;Dinner&gt;-Objekt kapselt eine Abfrage zum Abrufen von Dinner-Objekten aus der Datenbank mithilfe von LINQ to SQL. Wichtig ist, dass die Abfrage nicht für die Datenbank ausgeführt wird, bis wir versuchen, auf die Daten in der Abfrage zuzugreifen und Sie zu durchlaufen, oder bis wir die Methode "-Methode ()" aufrufen. Der Code, der die findupcomingdinner ()-Methode aufgerufen hat, kann optional zusätzliche "verkettete" Vorgänge/Filter zum iquerable&lt;Dinner&gt;-Objekt hinzufügen, bevor die Abfrage ausgeführt wird. LINQ to SQL ist dann intelligent genug, um die kombinierte Abfrage für die Datenbank auszuführen, wenn die Daten angefordert werden.

Um Paging-Logik zu implementieren, können wir die Index ()-Aktionsmethode des dinnerscontrollers aktualisieren, sodass Sie zusätzliche "Skip"-und "Take"-Operatoren für die zurückgegebene iquerable-&lt;Dinner&gt;-Sequenz anwendet, bevor ToList () darauf aufgerufen wird:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Der obige Code überspringt die ersten 10 bevorstehenden Dinner in der Datenbank und gibt dann 20 Abendessen zurück. LINQ to SQL ist intelligent genug, um eine optimierte SQL-Abfrage zu erstellen, die diese übersprungslogik in der SQL-Datenbank – und nicht auf dem Web-Server ausführt. Dies bedeutet, dass selbst dann, wenn wir Millionen von bevorstehenden Abendessen in der Datenbank haben, nur die 10, die wir wünschen, als Teil dieser Anforderung abgerufen werden (sodass Sie effizient und skalierbar ist).

### <a name="adding-a-page-value-to-the-url"></a>Hinzufügen eines "page"-Werts zur URL

Anstatt einen bestimmten Seitenbereich hart zu codieren, möchten wir, dass unsere URLs einen "page"-Parameter enthalten, der angibt, welches Dinner Range ein Benutzer anfordert.

#### <a name="using-a-querystring-value"></a>Verwenden eines QueryString-Werts

Der folgende Code veranschaulicht, wie wir unsere Index ()-Aktionsmethode aktualisieren können, um einen QueryString-Parameter zu unterstützen, und URLs wie */Dinners aktivieren? Seite = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Die obige Index ()-Aktionsmethode hat einen Parameter mit dem Namen "page". Der-Parameter wird als eine Ganzzahl angegeben, die NULL-Werte zulässt (was int? anzeigt). Dies bedeutet, dass die URL " */Dinners? Page = 2* " bewirkt, dass der Wert "2" als Parameterwert übergeben wird. Die */Dinners* -URL (ohne QueryString-Wert) führt zu einer Übergabe eines NULL-Werts.

Wir multiplizieren den Seiten Wert mit der Seitengröße (in diesem Fall 10 Zeilen), um zu bestimmen, wie viele Abendessen übersprungen werden soll. Wir verwenden den [ C# NULL-Operator "Coalescing" (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) , der beim Umgang mit Typen, die NULL-Werte zulassen, nützlich ist. Der obige Code weist Seite den Wert 0 zu, wenn der Seiten Parameter NULL ist.

#### <a name="using-embedded-url-values"></a>Verwenden von eingebetteten URL-Werten

Eine Alternative zur Verwendung eines QueryString-Werts besteht darin, den Seiten Parameter in die tatsächliche URL selbst einzubetten. Beispiel: */Dinners/Page/2* oder */Dinners/2*. ASP.NET MVC enthält eine leistungsfähige URL-Routing-Engine, die das unterstützen von Szenarien wie diesem erleichtert.

Wir können benutzerdefinierte Routing Regeln registrieren, die beliebige eingehende URL oder URL-Format jeder beliebigen Controller Klasse oder Aktionsmethode zuordnen. Wir müssen lediglich die Datei "Global. asax" in unserem Projekt öffnen:

![](implement-efficient-data-paging/_static/image2.png)

Und registrieren Sie dann eine neue Zuordnungsregel mithilfe der MapRoute ()-Hilfsmethode, z. b. dem ersten Routen aufzurufen. MapRoute () unten:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Oben registrieren wir eine neue Routing Regel mit dem Namen "upcomingdinner". Wir weisen darauf hin, dass es das URL-Format "Dinner/Page/{Page}" hat – wobei "{page}" ein Parameterwert ist, der in die URL eingebettet ist. Der dritte Parameter der MapRoute ()-Methode gibt an, dass der Index ()-Aktionsmethode für die dinnerscontroller-Klasse URLs zugeordnet werden sollen, die diesem Format entsprechen.

Wir können genau denselben Index ()-Code verwenden, den wir zuvor mit dem QueryString-Szenario verwendet haben – mit dem Unterschied, dass der Parameter "page" aus der URL und nicht aus "QueryString" stammt:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Und wenn wir nun die Anwendung ausführen und eingeben */Dinners* werden die ersten 10 bevorstehenden Abendessen angezeigt:

![](implement-efficient-data-paging/_static/image3.png)

Und wenn wir */Dinners/page/1* eingeben, wird die nächste Seite des Abendessen angezeigt:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Benutzeroberfläche für die Seitennavigation

Der letzte Schritt zum Durchführen des Pagingszenarios ist die Implementierung der "Next"-und "Previous"-Navigations Benutzeroberfläche in unserer Ansichts Vorlage, damit Benutzer die Dinner-Daten problemlos überspringen können.

Um dies ordnungsgemäß zu implementieren, müssen wir die Gesamtzahl der Dinner-Vorgänge in der Datenbank sowie die Anzahl der Datenseiten, in die dies übersetzt wird, kennen. Anschließend müssen wir berechnen, ob der aktuell angeforderte "page"-Wert am Anfang oder am Ende der Daten liegt, und die Benutzeroberfläche "Previous" und "Next" entsprechend anzeigen oder ausblenden. Wir könnten diese Logik in unserer Index ()-Aktionsmethode implementieren. Alternativ können wir dem Projekt eine Hilfsklasse hinzufügen, die diese Logik in einer wiederverwendbaren Weise kapselt.

Im folgenden finden Sie eine einfache "PaginatedList"-Hilfsklasse, die aus der Liste&lt;t&gt; Collection-Klasse abgeleitet ist, die in den .NET Framework integriert ist. Es implementiert eine wiederverwendbare Auflistungs Klasse, die verwendet werden kann, um eine beliebige Sequenz von iquerable-Daten zu paginieren. In unserer "nerddinner"-Anwendung werden wir über iquerable-&lt;Dinner&gt;-Ergebnisse verfügen, aber es könnte genauso leicht für iquerable&lt;Product&gt; oder iquervable verwendet werden&lt;Customer&gt; Ergebnisse in anderen Anwendungsszenarien:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Beachten Sie, wie die Berechnung von Eigenschaften wie "pageIndex", "PageSize", "totalCount" und "TotalPages" berechnet und dann verfügbar gemacht wird. Außerdem werden zwei Hilfseigenschaften "haspreviouspage" und "hasnextpage" verfügbar gemacht, die angeben, ob die Datenseite in der Auflistung am Anfang oder Ende der ursprünglichen Sequenz liegt. Der obige Code führt dazu, dass zwei SQL-Abfragen ausgeführt werden: der erste zum Abrufen der Gesamtzahl der Dinner-Objekte (Dadurch werden die Objekte nicht zurückgegeben – stattdessen wird eine "SELECT count"-Anweisung ausgeführt, die eine ganze Zahl zurückgibt), und die zweite, um nur die Zeilen von abzurufen. Daten, die wir in der Datenbank für die aktuelle Datenseite benötigen.

Anschließend können Sie die Hilfsmethode dinnerscontroller. Index () aktualisieren, um eine PaginatedList-&lt;Dinner&gt; aus dem Ergebnis "dinnerrepository. findupcomingdinner ()" zu erstellen und Sie an unsere Ansichts Vorlage zu übergeben:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Anschließend können Sie die Ansichts Vorlage \views\dinners\index.aspx so aktualisieren, dass Sie von ViewPage&lt;nerddinner geerbt wird. Hilfsprogramme. PaginatedList&lt;Dinner&gt;&gt; anstelle von ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;. Fügen Sie dann den folgenden Code am Ende der Ansicht-Vorlage ein, um die nächste und vorherige Navigations-UI anzuzeigen oder auszublenden:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Beachten Sie, wie die Hilfsmethode HTML. routelink () verwendet wird, um unsere Hyperlinks zu generieren. Diese Methode ähnelt der zuvor verwendeten Hilfsmethode HTML. Action Link (). Der Unterschied besteht darin, dass wir die URL mit der Routing Regel "upcomingdinners" erstellen, die wir in unserer Global. asax-Datei einrichten. Dadurch wird sichergestellt, dass wir URLs für unsere Index ()-Aktionsmethode generieren, die folgendes Format aufweisen: */Dinners/Page/{Page}* – wobei der {Page}-Wert eine Variable ist, die wir oben basierend auf dem aktuellen pageIndex-Wert bereitstellen.

Und wenn wir unsere Anwendung erneut ausführen, sehen wir in unserem Browser jeweils 10 Abendessen:

![](implement-efficient-data-paging/_static/image5.png)

Wir haben auch &lt;&lt;&lt; und &gt;&gt;&gt; Navigations Benutzeroberfläche unten auf der Seite, mit der wir die Daten mithilfe von verfügbaren URLs für Suchmaschinen weiterleiten und rückwärts überspringen können:

![](implement-efficient-data-paging/_static/image6.png)

| **Seitiges Thema: Grundlegendes zu den Auswirkungen von iquerable&lt;t&gt;** |
| --- |
| Iquerable&lt;t&gt; ist ein sehr leistungsfähiges Feature, das eine Vielzahl interessanter verzögerter Ausführungs Szenarien ermöglicht (z. b. Paging-und Kompositions basierte Abfragen). Wie bei allen leistungsstarken Features sollten Sie mit der Verwendung der Anwendung vorsichtig vorgehen und sicherstellen, dass Sie nicht missbraucht wird. Es ist wichtig zu erkennen, dass das Zurückgeben eines iquerable&lt;t&gt; Ergebnis aus Ihrem Repository es dem aufrufenden Code ermöglicht, auf verkettete Operator Methoden an ihn anzufügen und somit an der endgültigen Abfrage Ausführung teilzunehmen. Wenn Sie diese Fähigkeit nicht durch aufrufenden Code bereitstellen möchten, sollten Sie die zurückgegebenen&lt;t&gt; oder IEnumerable&lt;t&gt; Ergebnisse zurückgeben, die die Ergebnisse einer Abfrage enthalten, die bereits ausgeführt wurde. In Paginierungs Szenarien erfordert dies, dass Sie die tatsächliche datenpaginierungs-Logik per Push in die aufgerufene Repository-Methode übersetzen. In diesem Szenario können wir unsere findupcomingdinner ()-Finder-Methode so aktualisieren, dass Sie eine Signatur hat, die entweder eine PaginatedList: PaginatedList&lt; Dinner&gt; findupcomingdinner (int pageIndex) zurückgibt. int PageSize) {}, oder kehren Sie einen IList&lt;Dinner&gt;zurück, und verwenden Sie einen "totalCount"-out-Parameter, um die Gesamtzahl der Abendessen zurückzugeben: IList&lt;Dinner&gt; findupcomingdinners (int pageIndex, int PageSize, out int totalCount) {} |

### <a name="next-step"></a>Nächster Schritt

Sehen wir uns nun an, wie wir der Anwendung Authentifizierungs-und Autorisierungs Unterstützung hinzufügen können.

> [!div class="step-by-step"]
> [Zurück](re-use-ui-using-master-pages-and-partials.md)
> [Weiter](secure-applications-using-authentication-and-authorization.md)
