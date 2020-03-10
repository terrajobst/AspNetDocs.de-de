---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Anzeigen von Daten in einem Diagramm mit ASP.net Web Pages (Razor) | Microsoft-Dokumentation
author: microsoft
description: In diesem Kapitel wird erläutert, wie Daten in einem Diagramm angezeigt werden. In den vorherigen Kapiteln haben Sie gelernt, wie Daten manuell und in einem Raster angezeigt werden. In diesem Kapitel wird erläutert...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 6dad67d4e3d38d57a761c567d937d714a3184ea9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509133"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Anzeigen von Daten in einem Diagramm mit ASP.net Web Pages (Razor)

von [Microsoft](https://github.com/microsoft)

> In diesem Artikel wird erläutert, wie Sie ein Diagramm verwenden, um Daten auf einer ASP.net Web Pages-Website (Razor) mithilfe des `Chart`-Hilfsprogramms anzuzeigen.
> 
> **Lernen Sie**Folgendes:
> 
> - Anzeigen von Daten in einem Diagramm.
> - Gewusst wie: Formatieren von Diagrammen mithilfe integrierter Designs.
> - Das Speichern von Diagrammen und das Zwischenspeichern der Diagramme, um die Leistung zu verbessern.
> 
> Dies sind die ASP.net-Programmierfunktionen, die im Artikel eingeführt wurden:
> 
> - Das `Chart`-Hilfsprogramm.
> 
> > [!NOTE]
> > Die Informationen in diesem Artikel gelten für ASP.net Web Pages 1,0 und Web Pages 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Das Diagramm Hilfsprogramm

Wenn Sie Ihre Daten in grafischer Form anzeigen möchten, können Sie `Chart`-Hilfsprogramm verwenden. Das `Chart`-Hilfsprogramm kann ein Bild erzeugen, das Daten in einer Vielzahl von Diagrammtypen anzeigt. Es unterstützt viele Optionen zum Formatieren und bezeichnen. Das `Chart`-Hilfsprogramm kann mehr als 30 Typen von Diagrammen darstellen, einschließlich aller Diagrammtypen, mit denen Sie möglicherweise aus Microsoft Excel oder anderen Tool &#8212; Flächen Diagrammen, Balkendiagrammen, Säulen Diagrammen, Linien Diagrammen und Kreis Diagrammen zusammen mit spezielleren Diagrammen wie Aktien Diagrammen vertraut sind.

| **Flächen Diagramm** ![Beschreibung: Bild des Flächen Diagramm Typs](7-displaying-data-in-a-chart/_static/image1.jpg) | **Balkendiagramm** ![Beschreibung: Bild des Balkendiagramm Typs](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Säulendiagramm** ![Beschreibung: Bild des Säulendiagramm Typs](7-displaying-data-in-a-chart/_static/image3.jpg) | **Liniendiagramm** ![Beschreibung: Bild des Liniendiagramm Typs](7-displaying-data-in-a-chart/_static/image4.jpg) |
| Kreis **Diagramm** ![Beschreibung: Bild des Kreis Diagramm Typs](7-displaying-data-in-a-chart/_static/image5.jpg) | ![Beschreibung des **Aktien Diagramms** : Bild des Kursdiagramm Typs](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Diagrammelemente

In Diagrammen werden Daten und zusätzliche Elemente wie Legenden, Achsen, Reihen usw. angezeigt. Das folgende Bild zeigt viele der Diagramm Elemente, die Sie anpassen können, wenn Sie das `Chart`-Hilfsprogramm verwenden. In diesem Artikel wird gezeigt, wie Sie einige (nicht alle) dieser Elemente festlegen.

![Beschreibung: Abbildung der Diagramm Elemente](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Erstellen eines Diagramms aus Daten

Die Daten, die Sie in einem Diagramm anzeigen, können aus einem Array, aus den Ergebnissen, die von einer Datenbank zurückgegeben werden, oder aus Daten in einer XML-Datei bestehen.

### <a name="using-an-array"></a>Verwenden eines Arrays

Wie in [Einführung in ASP.net Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890)erläutert, können Sie mit einem Array eine Auflistung ähnlicher Elemente in einer einzelnen Variablen speichern. Sie können Arrays verwenden, um die Daten zu enthalten, die Sie in das Diagramm aufnehmen möchten.

In diesem Verfahren wird gezeigt, wie Sie ein Diagramm aus Daten in Arrays mithilfe des Standard Diagramm Typs erstellen können. Außerdem wird gezeigt, wie das Diagramm auf der Seite angezeigt wird.

1. Erstellen Sie eine neue Datei mit dem Namen *chartarraybasic. cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Der Code erstellt zunächst ein neues Diagramm und legt seine Breite und Höhe fest. Sie geben den Diagramm Titel an, indem Sie die `AddTitle`-Methode verwenden. Zum Hinzufügen von Daten verwenden Sie die `AddSeries`-Methode. In diesem Beispiel verwenden Sie die Parameter `name`, `xValue`und `yValues` der `AddSeries`-Methode. Der `name`-Parameter wird in der Diagramm Legende angezeigt. Der `xValue`-Parameter enthält ein Array von Daten, das entlang der horizontalen Achse des Diagramms angezeigt wird. Der `yValues`-Parameter enthält ein Array von Daten, das zum Zeichnen der vertikalen Punkte des Diagramms verwendet wird.

    Die `Write`-Methode rendert tatsächlich das Diagramm. Da Sie in diesem Fall keinen Diagrammtyp angegeben haben, rendert das `Chart`-Hilfsprogramm sein Standard Diagramm, bei dem es sich um ein Säulendiagramm handelt.
3. Führen Sie die Seite im Browser aus. Der Browser zeigt das Diagramm an. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Verwenden einer Datenbankabfrage für Diagramm Daten

Wenn sich die zu diagrammenden Informationen in einer Datenbank befinden, können Sie eine Datenbankabfrage ausführen und dann die Daten aus den Ergebnissen verwenden, um das Diagramm zu erstellen. In diesem Verfahren wird gezeigt, wie Sie die Daten aus der Datenbank lesen und anzeigen, die Sie im Artikel Einführung in die [Arbeit mit einer Datenbank an ASP.net Web Pages-Standorten](https://go.microsoft.com/fwlink/?LinkId=202893)erstellt haben.

1. Fügen Sie dem Stammverzeichnis der Website eine *App\_Daten* Ordner hinzu, wenn der Ordner nicht bereits vorhanden ist.
2. Fügen Sie im Ordner *App\_Data* die Datenbankdatei " *smallfolder. sdf* " hinzu, die unter Einführung in das [Arbeiten mit einer Datenbank an ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202893)beschrieben wird.
3. Erstellen Sie eine neue Datei mit dem Namen *chartdataquery. cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Der Code öffnet zunächst die Datenbank smallbakery und weist Sie einer Variablen mit dem Namen `db`zu. Diese Variable stellt ein `Database` Objekt dar, das verwendet werden kann, um aus der Datenbank zu lesen und in diese zu schreiben. Als Nächstes führt der Code eine SQL-Abfrage aus, um den Namen und den Preis der einzelnen Produkte zu erhalten. Der Code erstellt ein neues Diagramm und übergibt die Datenbankabfrage durch Aufrufen der `DataBindTable`-Methode des Diagramms. Diese Methode nimmt zwei Parameter an: der `dataSource`-Parameter ist für die Daten aus der Abfrage, und mit dem `xField`-Parameter können Sie festlegen, welche Datenspalte für die x-Achse des Diagramms verwendet wird.

    Als Alternative zur Verwendung der `DataBindTable`-Methode können Sie die `AddSeries`-Methode des `Chart`-Hilfsprogramms verwenden. Mit der `AddSeries`-Methode können Sie die `xValue`-und `yValues`-Parameter festlegen. Anstatt beispielsweise die `DataBindTable`-Methode wie folgt zu verwenden:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Sie können die `AddSeries`-Methode wie folgt verwenden:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Beide erzeugen die gleichen Ergebnisse. Die `AddSeries`-Methode ist flexibler, da Sie den Diagrammtyp und die Daten explizit angeben können, aber die `DataBindTable`-Methode ist einfacher zu verwenden, wenn Sie die zusätzliche Flexibilität nicht benötigen.
5. Führen Sie die Seite in einem Browser aus. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Verwenden von XML-Daten

Die dritte Option zum Diagramm ist die Verwendung einer XML-Datei als Daten für das Diagramm. Hierfür ist es erforderlich, dass die XML-Datei auch eine Schema Datei (*XSD* -Datei) enthält, die die XML-Struktur beschreibt. In diesem Verfahren wird gezeigt, wie Sie Daten aus einer XML-Datei lesen.

1. Erstellen Sie im Ordner *App\_Data* eine neue XML-Datei mit dem Namen " *Data. XML*".
2. Ersetzen Sie den vorhandenen XML-Code durch den folgenden Code. Dies sind einige XML-Daten zu Mitarbeitern in einem fiktiven Unternehmen. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Erstellen Sie im Ordner *App\_Data* eine neue XML-Datei mit dem Namen " *Data. xsd*". (Beachten Sie, dass diese Zeit die Erweiterung *. xsd*ist.)
4. Ersetzen Sie den vorhandenen XML-Code durch Folgendes: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Erstellen Sie im Stammverzeichnis der Website eine neue Datei mit dem Namen *chartdataxml. cshtml*.
6. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Der Code erstellt zunächst ein `DataSet`-Objekt. Dieses Objekt wird verwendet, um die aus der XML-Datei gelesenen Daten zu verwalten und entsprechend den Informationen in der Schema Datei zu organisieren. (Beachten Sie, dass der Anfang des Codes die-Anweisung `using SystemData`enthält. Dies ist erforderlich, um mit dem `DataSet` Objekt arbeiten zu können. Weitere Informationen finden Sie weiter unten in diesem Artikel unter [&quot;Verwenden von&quot;-Anweisungen und voll qualifizierten Namen](#SB_UsingStatements) .)

    Als Nächstes erstellt der Code auf Grundlage des Datasets ein `DataView`-Objekt. Die Datenansicht stellt ein Objekt bereit, an &#8212; das das Diagramm gebunden werden kann, d. h., es wird gelesen und gezeichnet. Das Diagramm bindet die Daten mithilfe der `AddSeries`-Methode, wie Sie zuvor beim darstellen der Array Daten gesehen haben, mit dem Unterschied, dass die Parameter `xValue` und `yValues` auf das `DataView` Objekt festgelegt sind.

    Außerdem wird in diesem Beispiel gezeigt, wie ein bestimmter Diagrammtyp angegeben wird. Wenn die Daten in der `AddSeries`-Methode hinzugefügt werden, wird der `chartType`-Parameter auch zum Anzeigen eines Kreis Diagramms festgelegt.
7. Führen Sie die Seite in einem Browser aus. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"-Anweisungen und voll qualifizierte Namen
> 
> Die .NET Framework, die ASP.net Web Pages mit Razor-Syntax auf basiert, besteht aus vielen Tausenden von Komponenten (Klassen). Um die Arbeit mit all diesen Klassen zu gewährleisten, sind Sie in *Namespaces*organisiert, die in gewisser Weise Bibliotheken ähneln. Der `System.Web`-Namespace enthält z. b. Klassen, die Browser-/Serverkommunikation unterstützen, der `System.Xml`-Namespace enthält Klassen, die zum Erstellen und Lesen von XML-Dateien verwendet werden, und der `System.Data`-Namespace enthält Klassen, mit denen Sie mit Daten arbeiten können.
> 
> Um auf eine bestimmte Klasse im .NET Framework zuzugreifen, muss Code nicht nur den Klassennamen, sondern auch den Namespace kennen, in dem sich die Klasse befindet. Um z. b. das `Chart`-Hilfsprogramm zu verwenden, muss der Code die `System.Web.Helpers.Chart` Klasse finden, die den Namespace (`System.Web.Helpers`) mit dem Klassennamen (`Chart`) kombiniert. Dies wird als *voll qualifizierter* Name &#8212; der Klasse bezeichnet und ist ein eindeutiger Speicherort, der sich innerhalb der vastness des .NET Framework befindet. Im Code sieht dies wie folgt aus:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Allerdings ist es mühsam (und fehleranfällig), diese langen, voll qualifizierten Namen jedes Mal zu verwenden, wenn Sie auf eine Klasse oder ein Hilfsprogramm verweisen möchten. Um die Verwendung von Klassennamen zu vereinfachen, können Sie daher die Namespaces *importieren* , an denen Sie interessiert sind. Dies ist in der Regel nur eine Handvoll von den vielen Namespaces im .NET Framework. Wenn Sie einen Namespace importiert haben, können Sie anstelle des voll qualifizierten Namens (`System.Web.Helpers.Chart`) nur einen Klassennamen (`Chart`) verwenden. Wenn der Code ausgeführt wird und auf einen Klassennamen stößt, kann er nur die Namespaces suchen, die Sie importiert haben, um diese Klasse zu finden.
> 
> Wenn Sie ASP.net Web Pages mit Razor-Syntax verwenden, um Webseiten zu erstellen, verwenden Sie in der Regel jedes Mal denselben Satz von Klassen, einschließlich der `WebPage`-Klasse, der verschiedenen Hilfsprogramme usw. Um Ihnen die Arbeit beim Importieren der relevanten Namespaces jedes Mal zu ersparen, wenn Sie eine Website erstellen, wird ASP.net so konfiguriert, dass automatisch ein Satz von Kernnamespaces für jede Website importiert wird. Aus diesem Grund mussten Sie sich nicht mit Namespaces befassen oder bis jetzt importieren. alle Klassen, mit denen Sie gearbeitet haben, befinden sich in Namespaces, die bereits für Sie importiert wurden.
> 
> Manchmal müssen Sie jedoch mit einer Klasse arbeiten, die sich nicht in einem Namespace befindet, der automatisch für Sie importiert wird. In diesem Fall können Sie entweder den voll qualifizierten Namen dieser Klasse verwenden, oder Sie können den Namespace, der die Klasse enthält, manuell importieren. Um einen Namespace zu importieren, verwenden Sie die `using`-Anweisung (`import` in Visual Basic), wie Sie in einem Beispiel weiter oben in diesem Artikel gesehen haben.
> 
> Beispielsweise befindet sich die `DataSet`-Klasse im `System.Data`-Namespace. Der `System.Data`-Namespace ist für ASP.net-Razor-Seiten nicht automatisch verfügbar. Damit Sie mit der `DataSet`-Klasse unter Verwendung des voll qualifizierten Namens arbeiten können, können Sie Code wie den folgenden verwenden:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Wenn Sie die `DataSet`-Klasse wiederholt verwenden müssen, können Sie einen Namespace wie diesen importieren und dann nur den Klassennamen im Code verwenden:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Sie können `using`-Anweisungen für alle anderen .NET Framework Namespaces hinzufügen, auf die Sie verweisen möchten. Wie bereits erwähnt, müssen Sie dies jedoch nicht oft tun, da sich die meisten Klassen, mit denen Sie arbeiten, in Namespaces befinden, die automatisch von ASP.net für die Verwendung auf *cshtml* -und *vbhtml* -Seiten importiert werden.

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Anzeigen von Diagrammen in einer Webseite

In den Beispielen, die Sie bisher gesehen haben, erstellen Sie ein Diagramm, und das Diagramm wird als Grafik direkt in den Browser gerendert. In vielen Fällen möchten Sie jedoch ein Diagramm als Teil einer Seite anzeigen, nicht nur im Browser. Hierfür ist ein zweistufiger Prozess erforderlich. Der erste Schritt besteht darin, eine Seite zu erstellen, die das Diagramm generiert, wie Sie dies bereits gesehen haben.

Der zweite Schritt besteht darin, das resultierende Bild auf einer anderen Seite anzuzeigen. Zum Anzeigen des Bilds verwenden Sie ein HTML-`<img>`-Element auf die gleiche Weise wie jedes beliebige Bild. Anstatt jedoch auf eine *JPG* -oder *PNG* -Datei zu verweisen, verweist das `<img>`-Element auf die *cshtml* -Datei, die das `Chart` Hilfsprogramm enthält, das das Diagramm erstellt. Wenn die Anzeige Seite ausgeführt wird, ruft das `<img>`-Element die Ausgabe des `Chart` Hilfsobjekts ab und rendert das Diagramm.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Erstellen Sie eine Datei mit dem Namen " *showChart. cshtml*".
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Der Code verwendet das `<img>`-Element, um das Diagramm anzuzeigen, das Sie zuvor in der Datei " *chartarraybasic. cshtml* " erstellt haben.
3. Führen Sie die Webseite in einem Browser aus. Die Datei *showChart. cshtml* zeigt das Diagramm Bild basierend auf dem Code in der Datei " *chartarraybasic. cshtml* " an.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Formatieren eines Diagramms

Das `Chart`-Hilfsprogramm unterstützt eine große Anzahl von Optionen, mit denen Sie die Darstellung des Diagramms anpassen können. Sie können Farben, Schriftarten, Rahmen usw. festlegen. Eine einfache Möglichkeit, die Darstellung eines Diagramms anzupassen, ist die Verwendung eines *Designs.* Designs sind Informationssammlungen, die angeben, wie ein Diagramm mithilfe von Schriftarten, Farben, Bezeichnungen, Paletten, Rahmen und Effekten dargestellt werden soll. (Beachten Sie, dass der Stil eines Diagramms nicht den Diagrammtyp angibt.)

In der folgenden Tabelle sind die integrierten Themen aufgeführt.

| Design | Beschreibung |
| --- | --- |
| `Vanilla` | Zeigt rote Spalten in einem weißen Hintergrund an. |
| `Blue` | Zeigt blaue Spalten in einem blauen Farbverlaufs Hintergrund an. |
| `Green` | Zeigt blaue Spalten in einem grünen Farbverlaufs Hintergrund an. |
| `Yellow` | Zeigt orangefarbene Spalten in einem gelben Farbverlaufs Hintergrund an. |
| `Vanilla3D` | Zeigt 3D-rote Spalten in einem weißen Hintergrund an. |

Sie können das Design angeben, das beim Erstellen eines neuen Diagramms verwendet werden soll.

1. Erstellen Sie eine neue Datei mit dem Namen *chartstylegreen. cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Dieser Code ist identisch mit dem vorherigen Beispiel, in dem die-Datenbank für Daten verwendet wird, beim Erstellen des `Chart`-Objekts wird jedoch der `theme`-Parameter hinzugefügt. Das folgende Beispiel zeigt den geänderten Code:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Führen Sie die Seite in einem Browser aus. Sie sehen die gleichen Daten wie zuvor, aber das Diagramm sieht genauer aus: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Speichern eines Diagramms

Wenn Sie das `Chart`-Hilfsprogramm wie bisher in diesem Artikel gesehen haben, erstellt das-Hilfsprogramm jedes Mal, wenn es aufgerufen wird, das Diagramm von Grund auf neu. Falls erforderlich, fragt der Code für das Diagramm auch die Datenbank erneut ab oder liest die XML-Datei erneut, um die Daten zu erhalten. In einigen Fällen kann dies ein komplexer Vorgang sein, z. b. wenn die Datenbank, die Sie Abfragen, groß ist, oder wenn die XML-Datei viele Daten enthält. Auch wenn das Diagramm nicht viele Daten umfasst, nimmt der Prozess der dynamischen Erstellung eines Images Server Ressourcen an, und wenn viele Personen die Seite oder Seiten anfordern, die das Diagramm anzeigen, kann sich dies auf die Leistung Ihrer Website auswirken.

Um Ihnen zu helfen, die potenziellen Auswirkungen der Erstellung eines Diagramms auf die Leistung zu reduzieren, können Sie ein Diagramm erstellen, wenn Sie es erstmalig benötigen, und dann speichern. Wenn das Diagramm erneut benötigt wird, anstatt es neu zu generieren, können Sie einfach die gespeicherte Version abrufen und diese wiederherstellen.

Sie können ein Diagramm auf folgende Weise speichern:

- Zwischenspeichern des Diagramms im Arbeitsspeicher des Computers (auf dem Server).
- Speichern Sie das Diagramm als Bilddatei.
- Speichern Sie das Diagramm als XML-Datei. Mit dieser Option können Sie das Diagramm ändern, bevor Sie es speichern.

### <a name="caching-a-chart"></a>Zwischenspeichern eines Diagramms

Nachdem Sie ein Diagramm erstellt haben, können Sie es Zwischenspeichern. Das Zwischenspeichern eines Diagramms bedeutet, dass es nicht neu erstellt werden muss, wenn es erneut angezeigt werden muss. Wenn Sie ein Diagramm im Cache speichern, geben Sie einen Schlüssel an, der für dieses Diagramm eindeutig sein muss.

Im Cache gespeicherte Diagramme können entfernt werden, wenn auf dem Server wenig Arbeitsspeicher verfügbar ist. Außerdem wird der Cache gelöscht, wenn die Anwendung aus irgendeinem Grund neu gestartet wird. Daher ist die Standardmethode zum Arbeiten mit einem zwischengespeicherten Diagramm, immer zuerst zu überprüfen, ob es im Cache verfügbar ist, und andernfalls, um das Diagramm zu erstellen oder neu zu erstellen.

1. Erstellen Sie im Stammverzeichnis Ihrer Website eine Datei mit dem Namen *showcachedchart. cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Das `<img>`-Tag enthält ein `src` Attribut, das auf die Datei " *chartsavetocache. cshtml* " verweist und einen Schlüssel als Abfrage Zeichenfolge an die Seite übergibt. Der Schlüssel enthält den Wert &quot;mychartkey&quot;. Die Datei *chartsavetocache. cshtml* enthält das `Chart` Hilfsprogramm, das das Diagramm erstellt. Diese Seite wird in einem Moment erstellt.

    Am Ende der Seite befindet sich ein Link zu einer Seite mit dem Namen *ClearCache. cshtml*. Das ist eine Seite, die Sie auch in Kürze erstellen werden. Sie benötigen " *ClearCache. cshtml* " nur, um das Zwischenspeichern für dieses Beispiel zu testen – es handelt sich nicht um einen Link oder eine Seite, den Sie normalerweise beim Arbeiten mit zwischengespeicherten Diagrammen einschließen.
3. Erstellen Sie im Stammverzeichnis Ihrer Website eine neue Datei mit dem Namen *chartsavetocache. cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Der Code überprüft zunächst, ob etwas als Schlüsselwert in der Abfrage Zeichenfolge übermittelt wurde. Wenn dies der Fall ist, versucht der Code, ein Diagramm aus dem Cache zu lesen, indem Sie die `GetFromCache`-Methode aufrufen und den Schlüssel übergibt. Wenn sich herausstellt, dass es im Cache unter diesem Schlüssel nichts gibt (was bei der ersten Anforderung des Diagramms der Fall ist), erstellt der Code das Diagramm wie üblich. Wenn das Diagramm abgeschlossen ist, speichert der Code es im Cache, indem `SaveToCache`aufgerufen wird. Diese Methode erfordert einen Schlüssel (damit das Diagramm später angefordert werden kann) und die Zeitspanne, in der das Diagramm im Cache gespeichert werden soll. (Die genaue Zeit, in der Sie ein Diagramm Zwischenspeichern, hängt davon ab, wie oft die von ihm dargestellten Daten sich ändern können.) Die `SaveToCache`-Methode erfordert auch einen `slidingExpiration` &#8212; -Parameter, wenn dieser auf true festgelegt ist, wird der Timeout-Wert jedes Mal zurückgesetzt, wenn auf das Diagramm zugegriffen wird. In diesem Fall bedeutet dies, dass der Cache Eintrag des Diagramms 2 Minuten nach dem letzten Zugriff auf das Diagramm abläuft. (Die Alternative zum gleitenden Ablauf ist die absolute Ablaufzeit, d. h., der Cache Eintrag läuft genau 2 Minuten ab, nachdem er in den Cache eingefügt wurde, unabhängig davon, wie oft auf ihn zugegriffen wurde.)

    Schließlich verwendet der Code die `WriteFromCache`-Methode zum Abrufen und Rendering des Diagramms aus dem Cache. Beachten Sie, dass sich diese Methode außerhalb des `if` Blocks befindet, der den Cache überprüft, da Sie das Diagramm aus dem Cache abfängt, um zu beginnen, oder dass Sie im Cache generiert und gespeichert werden musste.

    Beachten Sie, dass die `AddTitle`-Methode im Beispiel einen Zeitstempel enthält. (Das aktuelle Datum und die Uhrzeit &#8212; `DateTime.Now` &#8212; dem Titel hinzugefügt.)
5. Erstellen Sie eine neue Seite mit dem Namen *ClearCache. cshtml* , und ersetzen Sie deren Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Diese Seite verwendet das `WebCache`-Hilfsprogramm, um das Diagramm zu entfernen, das in " *chartsavetocache. cshtml*" zwischengespeichert wird. Wie bereits erwähnt, benötigen Sie normalerweise keine Seite wie diese. Sie erstellen es nur hier, um das Zwischenspeichern zu vereinfachen.
6. Führen Sie die " *showcachedchart. cshtml* "-Webseite in einem Browser aus. Die Seite zeigt das Diagramm Bild basierend auf dem Code an, der in der Datei " *chartsavetocache. cshtml* " enthalten ist. Notieren Sie sich den Zeitstempel im Diagramm Titel. 

    ![Beschreibung: Bild des Basis Diagramms mit Zeitstempel im Diagramm Titel](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Schließen Sie den Browser.
8. Führen Sie die *showcachedchart. cshtml* erneut aus. Beachten Sie, dass der Zeitstempel mit dem Wert vor identisch ist, was bedeutet, dass das Diagramm nicht erneut generiert, sondern stattdessen aus dem Cache gelesen wurde.
9. Klicken Sie in *showcachedchart. cshtml*auf den Link **Cache löschen** . Dadurch gelangen Sie zu *ClearCache. cshtml*, das meldet, dass der Cache gelöscht wurde.
10. Klicken Sie auf den Link **zu showcachedchart. cshtml zurückkehren** , oder führen Sie *showcachedchart. cshtml* in webmatrix erneut aus. Beachten Sie, dass sich der Zeitstempel geändert hat, da der Cache gelöscht wurde. Daher musste der Code das Diagramm neu generieren und wieder in den Cache einfügen.

### <a name="saving-a-chart-as-an-image-file"></a>Speichern eines Diagramms als Bilddatei

Sie können ein Diagramm auch als Bilddatei (z *. b. als jpg* -Datei) auf dem Server speichern. Anschließend können Sie die Bilddatei wie jedes beliebige Bild verwenden. Der Vorteil ist, dass die Datei gespeichert und nicht in einem temporären Cache gespeichert wird. Sie können ein neues Diagramm Bild zu unterschiedlichen Zeitpunkten speichern (z. b. stündlich) und dann einen permanenten Datensatz der Änderungen aufbewahren, die im Laufe der Zeit auftreten. Beachten Sie, dass Sie sicherstellen müssen, dass Ihre Webanwendung über die Berechtigung zum Speichern einer Datei in dem Ordner auf dem Server verfügt, auf dem Sie die Bilddatei ablegen möchten.

1. Erstellen Sie im Stammverzeichnis Ihrer Website einen Ordner mit dem Namen *\_chartfiles* , wenn er nicht bereits vorhanden ist.
2. Erstellen Sie im Stammverzeichnis Ihrer Website eine neue Datei mit dem Namen *chartsave. cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Der Code prüft zunächst, ob die *JPG* -Datei vorhanden ist, indem Sie die `File.Exists`-Methode aufrufen. Wenn die Datei nicht vorhanden ist, erstellt der Code eine neue `Chart` aus einem Array. Dieses Mal ruft der Code die `Save`-Methode auf und übergibt den `path`-Parameter, um den Dateipfad und den Dateinamen anzugeben, wo das Diagramm gespeichert werden soll. Im Text der Seite verwendet ein `<img>`-Element den Pfad, um auf die anzuzeigende *JPG* -Datei zu zeigen.
4. Führen Sie die Datei " *chartsave. cshtml* " aus.
5. Kehren Sie zu webmatrix zurück. Beachten Sie, dass eine Bilddatei mit dem Namen *chart01. jpg* im Ordner *\_chartfiles* gespeichert wurde.

### <a name="saving-a-chart-as-an-xml-file"></a>Speichern eines Diagramms als XML-Datei

Schließlich können Sie ein Diagramm als XML-Datei auf dem Server speichern. Ein Vorteil der Verwendung dieser Methode gegenüber dem Zwischenspeichern des Diagramms oder dem Speichern des Diagramms in einer Datei besteht darin, dass Sie den XML-Code vor dem Anzeigen des Diagramms ändern können, wenn Sie möchten. Die Anwendung muss über Lese-/Schreibberechtigungen für den Ordner auf dem Server verfügen, auf dem Sie die Bilddatei ablegen möchten.

1. Erstellen Sie im Stammverzeichnis Ihrer Website eine neue Datei mit dem Namen *chartsavexml. cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Dieser Code ähnelt dem Code, den Sie bereits gesehen haben, um ein Diagramm im Cache zu speichern, mit dem Unterschied, dass eine XML-Datei verwendet wird. Der Code prüft zunächst, ob die XML-Datei vorhanden ist, indem Sie die `File.Exists`-Methode aufrufen. Wenn die Datei vorhanden ist, erstellt der Code ein neues `Chart` Objekt und übergibt den Dateinamen als `themePath` Parameter. Dadurch wird das Diagramm basierend auf den in der XML-Datei vorhandenen Dateien erstellt. Wenn die XML-Datei nicht bereits vorhanden ist, erstellt der Code ein Diagramm wie normal und ruft dann `SaveXml` auf, um es zu speichern. Das Diagramm wird mit der `Write`-Methode gerendert, wie Sie dies bereits gesehen haben.

    Wie bei der Seite, die das Caching anzeigt, enthält dieser Code einen Zeitstempel im Diagramm Titel.
3. Erstellen Sie eine neue Seite mit dem Namen *chartdisplayxmlchart. cshtml* , und fügen Sie Ihr das folgende Markup hinzu: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Führen Sie die Seite *chartdisplayxmlchart. cshtml* aus. Das Diagramm wird angezeigt. Notieren Sie sich den Zeitstempel im Titel des Diagramms.
5. Schließen Sie den Browser.
6. Klicken Sie in webmatrix mit der rechten Maustaste auf den Ordner *\_chartfiles* , klicken Sie auf **Aktualisieren**, und öffnen Sie dann den Ordner. Die Datei *xmlchart. XML* in diesem Ordner wurde vom `Chart`-Hilfsprogramm erstellt. 

    ![Beschreibung: der _ChartFiles Ordner, der die vom Diagramm Hilfsprogramm erstellte Datei "xmlchart. xml" anzeigt.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Führen Sie die Seite *chartdisplayxmlchart. cshtml* erneut aus. Das Diagramm zeigt den gleichen Zeitstempel an, als Sie die Seite zum ersten Mal ausgeführt haben. Das liegt daran, dass das Diagramm aus dem XML-Code generiert wird, den Sie zuvor gespeichert haben.
8. Öffnen Sie in webmatrix den *\_chartfiles* -Ordner, und löschen Sie die Datei " *xmlchart. XML* ".
9. Führen Sie die Seite " *chartdisplayxmlchart. cshtml* " erneut aus. Dieses Mal wird der Zeitstempel aktualisiert, da der `Chart`-Hilfsprogramm die XML-Datei neu erstellen musste. Wenn Sie möchten, überprüfen Sie den *\_chartfiles* -Ordner, und beachten Sie, dass die XML-Datei zurück ist.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in das Arbeiten mit einer Datenbank an ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Verwenden der Zwischenspeicherung in ASP.net Web Pages Websites zum Verbessern der Leistung](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Chart-Klasse](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (ASP.net Web Pages-API-Referenz auf MSDN)
