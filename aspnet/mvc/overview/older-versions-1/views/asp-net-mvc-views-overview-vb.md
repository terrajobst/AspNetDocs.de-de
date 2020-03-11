---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Übersicht über ASP.NET MVC-Ansichten (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet Sie sich von einer HTML-Seite? In diesem Tutorial stellt Stephen Walther Sie in Ansichten vor und veranschaulicht, wie Sie es tun können...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435273"
---
# <a name="aspnet-mvc-views-overview-vb"></a>ASP.NET MVC-Ansichten – Übersicht (VB)

von [Stephen Walther](https://github.com/StephenWalther)

> Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet Sie sich von einer HTML-Seite? In diesem Tutorial stellt Stephen Walther Sie in Ansichten vor und veranschaulicht, wie Sie die Vorteile von Anzeigedaten und HTML-Hilfsprogrammen in einer Ansicht nutzen können.

Der Zweck dieses Tutorials besteht darin, Ihnen eine kurze Einführung in ASP.NET MVC-Ansichten, Daten anzeigen und HTML-Hilfsprogramme zu bieten. Am Ende dieses Tutorials sollten Sie wissen, wie neue Sichten erstellt, Daten von einem Controller an eine Ansicht übergeben werden und wie HTML-Hilfsprogramme verwendet werden, um Inhalte in einer Ansicht zu generieren.

## <a name="understanding-views"></a>Grundlegendes zu Sichten

Im Gegensatz zu ASP.net-oder Active Server Seiten schließt ASP.NET MVC nichts ein, das direkt einer Seite entspricht. In einer ASP.NET-MVC-Anwendung gibt es keine Seite auf dem Datenträger, die dem Pfad in der URL entspricht, die Sie in die Adressleiste Ihres Browsers eingeben. Die nächstgelegene Seite einer Seite in einer ASP.NET MVC-Anwendung wird als *Ansicht*bezeichnet.

In einer ASP.NET-MVC-Anwendung werden eingehende Browser Anforderungen Controller Aktionen zugeordnet. Eine Controller Aktion kann eine Ansicht zurückgeben. Eine Controller Aktion kann jedoch eine andere Art von Aktion ausführen, z. b. eine Umleitung an eine andere Controller Aktion.

In der Liste 1 ist ein einfacher Controller namens HomeController enthalten. Der HomeController macht zwei Controller Aktionen namens Index () und Details () verfügbar.

**Codebeispiel 1: HomeController. vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

Sie können die erste Aktion, die Index ()-Aktion, aufrufen, indem Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:

/Home/Index

Sie können die zweite Aktion, die Aktion "Details ()" aufrufen, indem Sie diese Adresse in Ihrem Browser eingeben:

/Home/Details

Die Index ()-Aktion gibt eine Ansicht zurück. Bei den meisten von Ihnen erstellten Aktionen werden Sichten zurückgegeben. Eine Aktion kann jedoch andere Arten von Aktions Ergebnissen zurückgeben. Beispielsweise gibt die Aktion Details () ein redirecttoaction-Ergebnis zurück, das die eingehende Anforderung an die Index ()-Aktion umleitet.

Die Index ()-Aktion enthält die folgende einzelne Codezeile:

Anzeigen ()

Diese Codezeile gibt eine Ansicht zurück, die sich unter folgendem Pfad auf dem Webserver befinden muss:

\Views\home\index.aspx

Der Pfad zur Ansicht wird vom Namen des Controllers und vom Namen der Controller Aktion abgeleitet.

Wenn Sie möchten, können Sie die Ansicht explizit angeben. In der folgenden Codezeile wird eine Ansicht mit dem Namen Fred zurückgegeben:

Ansicht (Fred)

Wenn diese Codezeile ausgeführt wird, wird eine Ansicht von folgendem Pfad zurückgegeben:

\Views\home\fred.aspx

> [!NOTE] 
> 
> Wenn Sie Vorhaben, Komponententests für Ihre ASP.NET MVC-Anwendung zu erstellen, empfiehlt es sich, explizit über Ansichts Namen zu sprechen. Auf diese Weise können Sie einen Komponenten Test erstellen, um zu überprüfen, ob die erwartete Ansicht von einer Controller Aktion zurückgegeben wurde.

## <a name="adding-content-to-a-view"></a>Hinzufügen von Inhalt zu einer Ansicht

Eine Sicht ist ein Standard-HTML-Dokument (X), das Skripts enthalten kann. Mithilfe von Skripts können Sie dynamische Inhalte zu einer Ansicht hinzufügen.

In der Ansicht in der Liste 2 werden z. b. das aktuelle Datum und die Uhrzeit angezeigt.

**Codebeispiel 2: \views\home\index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

Beachten Sie, dass der Text der HTML-Seite in der Liste 2 das folgende Skript enthält:

&lt;% Response. Write (DateTime. Now)%&gt;

Sie verwenden die Skript Trennzeichen &lt;% und%&gt;, um den Anfang und das Ende eines Skripts zu markieren. Dieses Skript ist in Visual Basic geschrieben. Es zeigt das aktuelle Datum und die aktuelle Uhrzeit durch Aufrufen der Response. Write ()-Methode zum Rendering von Inhalten im Browser an. Die Skript Trennzeichen &lt;% und%&gt; können verwendet werden, um eine oder mehrere-Anweisungen auszuführen.

Da Sie "Response. Write ()" so oft aufrufen, stellt Microsoft Ihnen eine Verknüpfung zum Aufrufen der Response. Write ()-Methode zur Verfügung. Die Ansicht in der Liste 3 verwendet die Trennzeichen &lt;% = und%&gt; als Verknüpfung zum Aufrufen von Response. Write ().

**Codebeispiel 3: views\home\index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

Sie können eine beliebige .NET-Sprache verwenden, um dynamischen Inhalt in einer Ansicht zu generieren. Normalerweise verwenden Sie entweder Visual Basic .net oder C# zum Schreiben von Controllern und Ansichten.

## <a name="using-html-helpers-to-generate-view-content"></a>Verwenden von HTML-Hilfsprogrammen zum Generieren von Ansichts Inhalten

Um das Hinzufügen von Inhalt zu einer Ansicht zu vereinfachen, können Sie die Vorteile eines *HTML*-Hilfsobjekts nutzen. Ein HTML-Hilfsprogramm ist in der Regel eine Methode, die eine Zeichenfolge generiert. Sie können HTML-Hilfsprogramme verwenden, um HTML-Standardelemente wie Textfelder, Links, Dropdown Listen und Listenfelder zu generieren.

Die Ansicht in der Liste 4 nutzt beispielsweise drei HTML-Hilfsprogramme: die BeginForm ()-, TextBox-und Password ()-Hilfsprogramme, um ein Anmeldeformular zu generieren (siehe Abbildung 1).

**Codebeispiel 4: \views\home\login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

[![des Dialog Felds "Neues Projekt"](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Abbildung 01**: ein Standard Anmeldeformular ([Klicken Sie, um das Bild in voller Größe anzuzeigen](asp-net-mvc-views-overview-vb/_static/image2.png))

Alle HTML-Hilfsmethoden werden in der HTML-Eigenschaft der Ansicht aufgerufen. Beispielsweise können Sie ein Textfeld durch Aufrufen der HTML. TextBox ()-Methode Rendering.

Beachten Sie, dass Sie die Skript Trennzeichen &lt;% = und%&gt; verwenden, wenn Sie sowohl die HTML. TextBox ()-als auch die HTML. Password ()-Hilfsprogramme aufrufen. Diese Hilfsprogramme geben einfach eine Zeichenfolge zurück. Sie müssen Response. Write () aufzurufen, um die Zeichenfolge im Browser zu erzeugen.

Die Verwendung von HTML-Hilfsmethoden ist optional. Dadurch wird das Leben vereinfacht, indem die Menge an HTML und Skript reduziert wird, die Sie schreiben müssen. Die Ansicht in Auflistung 5 rendert genau dasselbe Formular wie die Ansicht in der Ansicht 4, ohne HTML-Hilfsprogramme zu verwenden.

**Codebeispiel 5: \views\home\login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

Sie haben auch die Möglichkeit, eigene HTML-Hilfsprogramme zu erstellen. Sie können z. b. eine GridView ()-Hilfsmethode erstellen, die einen Satz von Datenbankdaten Sätzen in einer HTML-Tabelle automatisch anzeigt. Dieses Thema wird im Tutorial Erstellen von **benutzerdefinierten HTML**-Hilfsprogrammen erläutert.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Verwenden von Ansichts Daten zum Übergeben von Daten an eine Sicht

Sie verwenden Daten anzeigen, um Daten von einem Controller an eine Ansicht zu übergeben. Betrachten Sie Daten wie ein Paket, das Sie über die e-Mail senden. Alle Daten, die von einem Controller an eine Ansicht übermittelt werden, müssen mithilfe dieses Pakets gesendet werden. Der Controller in der Liste 6 fügt z. b. eine Meldung zum Anzeigen von Daten hinzu.

**Codebeispiel 6: ProductController. vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

Die Eigenschaft "ViewData" des Controllers stellt eine Auflistung von Name-Wert-Paaren dar. In der Liste 6 fügt die Index ()-Methode der Ansichts Datensammlung mit dem Wert Hallo Welt! ein Element hinzu. Wenn die Ansicht von der Index ()-Methode zurückgegeben wird, werden die Ansichts Daten automatisch an die Ansicht übermittelt.

Die Ansicht in der Liste 7 Ruft die Nachricht aus den Ansichts Daten ab und rendert die Nachricht im Browser.

**Codebeispiel 7: \views\product\index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Beachten Sie, dass die-Sicht beim Rendern der Nachricht die HTML. Encode ()-HTML-Hilfsmethode nutzt. Das HTML-Hilfsprogramm HTML. Encode () codiert Sonderzeichen, z. b. &lt; und &gt; in Zeichen, die auf einer Webseite sicher angezeigt werden können. Wenn Sie Inhalte, die von einem Benutzer an eine Website übermittelt werden, wiederzugeben, sollten Sie den Inhalt codieren, um JavaScript Injection-Angriffe zu verhindern.

(Da wir die Nachricht selbst in ProductController erstellt haben, müssen wir die Nachricht wirklich codieren. Es ist jedoch eine gute Gewohnheit, immer die HTML. Encode ()-Methode aufzurufen, wenn Inhalte angezeigt werden, die aus Sicht Daten innerhalb einer Ansicht abgerufen werden.)

In der Liste 7 haben wir die Anzeigedaten genutzt, um eine einfache Zeichen folgen Nachricht von einem Controller an eine Ansicht zu übergeben. Sie können auch Daten anzeigen verwenden, um andere Datentypen, z. b. eine Sammlung von Datenbankdaten Sätzen, von einem Controller an eine Ansicht zu übergeben. Wenn Sie z. b. den Inhalt der Datenbanktabelle "Products" in einer Ansicht anzeigen möchten, übergeben Sie die Sammlung der Datenbankdaten Sätze in "Daten anzeigen".

Sie haben auch die Möglichkeit, stark typisierte Ansichts Daten von einem Controller an eine Ansicht zu übergeben. Dieses Thema wird im Lernprogramm Grundlegendes zu **stark typisierten Ansichts Daten und Ansichten**beschrieben.

## <a name="summary"></a>Zusammenfassung

Dieses Tutorial bot eine kurze Einführung in ASP.NET MVC-Ansichten, Ansichts Daten und HTML-Hilfsprogramme. Im ersten Abschnitt haben Sie erfahren, wie Sie Ihrem Projekt neue Ansichten hinzufügen. Sie haben gelernt, dass Sie dem richtigen Ordner eine Ansicht hinzufügen müssen, um Sie von einem bestimmten Controller aus aufzurufen. Als nächstes haben wir das Thema der HTML-Hilfsprogramme erläutert. Sie haben gelernt, wie HTML-Hilfsprogramme Ihnen die einfache Generierung von HTML-Standard Inhalten ermöglichen. Schließlich haben Sie gelernt, wie Sie die Anzeige von Daten nutzen können, um Daten von einem Controller an eine Ansicht zu übergeben.

> [!div class="step-by-step"]
> [Zurück](passing-data-to-view-master-pages-cs.md)
> [Weiter](creating-custom-html-helpers-vb.md)
