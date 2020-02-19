---
uid: mvc/overview/performance/bundling-and-minification
title: Bündelung und Minimierung | Microsoft-Dokumentation
author: Rick-Anderson
description: Bündelung und Minimierung sind zwei Verfahren, die Sie in ASP.NET 4,5 zur Verbesserung der Anforderungs Ladezeit verwenden können. Bündelung und Minimierung verbessern die Ladezeit durch reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 61bfe5dbac04b57e1461183b66ead2f01fe0734c
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457764"
---
# <a name="bundling-and-minification"></a>Bundling und Minimierung

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Bündelung und Minimierung sind zwei Verfahren, die Sie in ASP.NET 4,5 zur Verbesserung der Anforderungs Ladezeit verwenden können. Bündelung und Minimierung verbessern die Ladezeit, indem die Anzahl der Anforderungen an den Server reduziert und die Größe der angeforderten Assets (z. b. CSS und JavaScript) reduziert wird.

Die meisten der aktuellen Haupt Browser beschränken die Anzahl [gleichzeitiger Verbindungen](http://www.browserscope.org/?category=network) pro Hostname auf sechs. Dies bedeutet, dass während der Verarbeitung von sechs Anforderungen zusätzliche Anforderungen für Assets auf einem Host vom Browser in die Warteschlange eingereiht werden. In der folgenden Abbildung zeigt die Registerkarten Netzwerk der Internet Explorer F12-Tools die zeitliche Steuerung für Assets, die für die Info-Ansicht einer Beispielanwendung erforderlich sind.

![B/M](bundling-and-minification/_static/image1.png)

Die grauen Balken zeigen die Zeit an, zu der die Anforderung vom Browser in die Warteschlange eingereiht wird Der gelbe Balken ist die Anforderungs Zeit bis zum ersten Byte, d. h. die Zeit, die benötigt wird, um die Anforderung zu senden und die erste Antwort vom Server zu empfangen. Die blauen Balken zeigen die Zeit an, die zum Empfangen der Antwortdaten vom Server benötigt wird. Sie können auf ein Medienobjekt doppelklicken, um ausführliche Informationen zur zeitlichen Steuerung zu erhalten. Die folgende Abbildung zeigt z. b. die zeitlichen Details zum Laden der Datei */Scripts/myscripts/JavaScript6.js* .

![](bundling-and-minification/_static/image2.png)

Die vorherige Abbildung zeigt das **Start** Ereignis, das den Zeitpunkt der Warteschlange der Anforderung anzeigt, da der Browser die Anzahl gleichzeitiger Verbindungen einschränkt. In diesem Fall wurde die Anforderung für 46 Millisekunden in die Warteschlange eingereiht, um auf den Abschluss einer anderen Anforderung zu warten.

## <a name="bundling"></a>Anbietet

Bündelung ist ein neues Feature in ASP.NET 4,5, das das kombinieren oder bündeln mehrerer Dateien in einer einzigen Datei vereinfacht. Sie können CSS, JavaScript und andere Bündel erstellen. Weniger Dateien bedeuten weniger HTTP-Anforderungen und können die Leistung der ersten Seiten Auslastung verbessern.

In der folgenden Abbildung ist die gleiche Zeit Steuerungs Ansicht der oben gezeigten Ansicht "Ansicht" dargestellt, aber dieses Mal mit aktivierter Bündelung und Minimierung.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimierung

Bei der Minimierung werden verschiedene Code Optimierungen für Skripts oder CSS durchführt, z. b. das Entfernen unnötiger Leerzeichen und Kommentare und das verkürzen von Variablennamen auf ein Zeichen. Beachten Sie die folgende JavaScript-Funktion.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Nach der Minimierung wird die Funktion auf Folgendes reduziert:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Zusätzlich zum Entfernen von Kommentaren und unnötigen Leerraum wurden die folgenden Parameter und Variablennamen wie folgt umbenannt (verkürzt):

| **Original** | **Ann** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Auswirkungen von Bündelung und Minimierung

In der folgenden Tabelle werden verschiedene wichtige Unterschiede zwischen dem individuellen Auflisten aller Assets und der Verwendung von Bündelung und Minimierung (B/M) im Beispielprogramm veranschaulicht.

|  | **Verwenden von B/M** | **Ohne B/M** | **Änderung** |
| --- | --- | --- | --- |
| **Datei Anforderungen** | 9 | 34 | 256% |
| **Gesendete KB** | 3,26 | 11.92 | 266% |
| **KB empfangen** | 388.51 | 530 | 36% |
| **Ladezeit** | 510 MS | 780 MS | 53% |

Die gesendeten Bytes Wiesen bei der Bündelung eine erhebliche Reduzierung auf, da Browser recht ausführlich mit den HTTP-Headern sind, die Sie für Anforderungen verwenden. Die empfangene Verringerung der Bytes ist nicht so groß, weil die größten Dateien (*Skripts\\jQuery-UI-1.8.11. min. js* und *Skripts\\jQuery-1.7.1. min. js*) bereits minimiert wurden. Hinweis: die Zeitangaben für das Beispielprogramm verwendeten das [Tool "](http://www.fiddler2.com/fiddler2/) Tools", um ein langsames Netzwerk zu simulieren. (Wählen Sie im Menü Fiddler- **Regeln** die Option **Leistung** und dann **Modem Geschwindigkeiten simulieren**aus.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debuggen von gebündelten und miniertem JavaScript

Das Debuggen von JavaScript in einer Entwicklungsumgebung (in der das [Kompilierungs Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der Datei " *Web. config* " auf "`debug="true"`" festgelegt ist) ist einfach, da die JavaScript-Dateien nicht gebündelt oder minimiert werden. Sie können auch einen Releasebuild Debuggen, bei dem Ihre JavaScript-Dateien gebündelt und minimiert werden. Mithilfe der Tools für die Verwendung von IE F12 können Sie mithilfe des folgenden Ansatzes eine JavaScript-Funktion Debuggen, die in einem minierten Bundle enthalten ist:

1. Wählen Sie die Registerkarte **Skript** , und wählen Sie dann die Schaltfläche **Debugging starten** aus.
2. Wählen Sie das Paket mit der JavaScript-Funktion aus, die Sie mithilfe der Schaltfläche Objekte debuggen möchten.  
    ![](bundling-and-minification/_static/image4.png)
3. Formatieren Sie die minierte JavaScript-Datei, indem Sie die **Konfigurations Schaltfläche** ![](bundling-and-minification/_static/image5.png)und dann **JavaScript formatieren**auswählen.
4. Wählen Sie im Eingabefeld **Skript suchen** den Namen der Funktion aus, die Sie debuggen möchten. In der folgenden Abbildung wurde **addaltdeimg** in das Eingabefeld für das **Suchskript** eingegeben.  
    ![](bundling-and-minification/_static/image6.png)

Weitere Informationen zum Debuggen mit den F12-Entwicklertools finden Sie im MSDN-Artikel [Verwenden des F12-Entwicklertools zum Debuggen von JavaScript-Fehlern](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Steuern der Bündelung und Minimierung

Die Bündelung und Minimierung wird aktiviert oder deaktiviert, indem der Wert des Debug-Attributs im [compilation-Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der Datei " *Web. config* " festgelegt wird. Im folgenden XML-Code wird `debug` auf true festgelegt, sodass die Bündelung und Minimierung deaktiviert ist.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Legen Sie zum Aktivieren von Bündelung und Minimierung den `debug` Wert auf "false" fest. Sie können die *Web. config* -Einstellung mit der `EnableOptimizations`-Eigenschaft für die `BundleTable`-Klasse überschreiben. Der folgende Code ermöglicht das bündeln und minimieren und überschreibt jede Einstellung in der Datei " *Web. config* ".

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Wenn `EnableOptimizations` nicht `true` oder das debug-Attribut im [Kompilierungs Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der Datei *Web. config* auf `false`festgelegt ist, werden Dateien nicht gebündelt oder minimiert. Außerdem wird die min-Version der Dateien nicht verwendet, sondern die vollständigen Debugversionen werden ausgewählt. `EnableOptimizations` überschreibt das debug-Attribut im [compilation-Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der Datei " *Web. config".*

## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Verwenden von Bündelung und Minimierung mit ASP.net Web Forms und Webseiten

- Informationen zu Webseiten finden Sie im Blogeintrag [Hinzufügen der Weboptimierung zu einer Webseiten-Website](https://docs.microsoft.com/archive/blogs/rickandy/adding-web-optimization-to-a-web-pages-site).
- Web Forms finden Sie im Blogeintrag [Hinzufügen von Bündelung und Minimierung zu Web Forms](https://docs.microsoft.com/archive/blogs/rickandy/adding-bundling-and-minification-to-web-forms).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Verwenden von Bündelung und Minimierung mit ASP.NET MVC

In diesem Abschnitt wird ein ASP.NET MVC-Projekt erstellt, um die Bündelung und Minimierung zu untersuchen. Erstellen Sie zunächst ein neues ASP.NET MVC-Internetprojekt mit dem Namen **mvcbm** , ohne die Standardeinstellungen zu ändern.

Öffnen Sie die *App-\\\_starten Sie\\BundleConfig.cs* -Datei, und überprüfen Sie die `RegisterBundles`-Methode, die zum Erstellen, registrieren und Konfigurieren von Paketen verwendet wird Der folgende Code zeigt einen Teil der `RegisterBundles`-Methode.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Der vorangehende Code erstellt ein neues JavaScript-Paket mit dem Namen *~/Bundles/jQuery* , das alle geeigneten (Debuggen oder minimierte) enthält. *vsdoc*) Dateien im Ordner " *Scripts* ", die mit der Platzhalter Zeichenfolge "~/Scripts/jQuery-{Version}.js" identisch sind. Bei ASP.NET MVC 4 bedeutet dies bei einer Debugkonfiguration, dass die Datei *jQuery-1.7.1. js* dem Bundle hinzugefügt wird. In einer Releasekonfiguration wird *jQuery-1.7.1. min. js* hinzugefügt. Das Bundle-Framework befolgt einige gängige Konventionen, wie z. b.:

- Die ". min"-Datei für die Freigabe wird ausgewählt, wenn " *Flex. min. js* " und " *Flex. js* " vorliegen
- Auswählen der nicht-". min"-Version für das Debuggen.
- Ignorieren von "-vsdoc"-Dateien (z. b. *jQuery-1.7.1-vsdoc. js*), die nur von IntelliSense verwendet werden.

Der oben gezeigte `{version}` Platzhalter Vergleich wird zum automatischen Erstellen eines jQuery-Pakets mit der entsprechenden Version von jQuery im Skript *Ordner verwendet* . In diesem Beispiel bietet die Verwendung einer Platzhalter Karte die folgenden Vorteile:

- Ermöglicht die Verwendung von nuget, um ein Update auf eine neuere Version von jQuery durchführen zu können, ohne den vorangehenden codiercode oder jQuery-Verweise in den Ansichts Seiten
- Wählt automatisch die Vollversion für Debugkonfigurationen und die ". min"-Version für Releasebuilds aus.

## <a name="using-a-cdn"></a>Verwenden eines CDN

 Der folgende Code ersetzt das lokale jQuery-Bundle durch ein CDN-jQuery-Bundle.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Im obigen Code wird jQuery vom CDN im Releasemodus angefordert, und die Debugversion von jQuery wird lokal im Debugmodus abgerufen. Wenn Sie ein CDN verwenden, sollten Sie für den Fall, dass die CDN-Anforderung fehlschlägt, über einen Fall Back Mechanismus verfügen. Das folgende Markup Fragment am Ende der Layoutdatei zeigt das Skript, das zum Anfordern der jQuery-Anforderung hinzugefügt wird, wenn das CDN fehlschlägt.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Erstellen eines Pakets

Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) -Klasse `Include`-Methode nimmt ein Array von Zeichen folgen an, wobei jede Zeichenfolge ein virtueller Pfad zur Ressource ist. Der folgende Code aus der `RegisterBundles`-Methode in der *App\\\_Start\\BundleConfig.cs* -Datei zeigt, wie mehrere Dateien zu einem Paket hinzugefügt werden:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) -Klasse `IncludeDirectory`-Methode wird bereitgestellt, um alle Dateien in einem Verzeichnis (und optional alle Unterverzeichnisse) hinzuzufügen, die einem Suchmuster entsprechen. Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) -Klasse `IncludeDirectory`-API wird unten dargestellt:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Auf Pakete wird in Sichten mithilfe der Rendering-Methode verwiesen (`Styles.Render` für CSS und `Scripts.Render` für JavaScript). Das folgende Markup aus den *Sichten\\Shared\\\_Layout. cshtml* -Datei zeigt, wie die standardmäßigen ASP.net-Internetprojekt Sichten auf CSS-und JavaScript-Bündel verweisen.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Beachten Sie, dass die Rendering-Methode ein Array von Zeichen folgen annimmt, sodass Sie mehrere Bündel in einer Codezeile hinzufügen können. Sie sollten in der Regel die Rendering-Methoden verwenden, die den erforderlichen HTML-Code für den Verweis auf das Medienobjekt erstellen. Sie können die `Url`-Methode verwenden, um die URL für das Medienobjekt zu generieren, ohne dass das für den Verweis auf das Medienobjekt erforderliche Markup Angenommen, Sie möchten das neue HTML5-Attribut " [Async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) " verwenden. Der folgende Code zeigt, wie mit der `Url`-Methode auf modernizr verwiesen wird.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Verwenden des Platzhalter Zeichens "\*" zum Auswählen von Dateien

Der in der `Include`-Methode angegebene virtuelle Pfad und das Suchmuster in der `IncludeDirectory`-Methode können im letzten Pfad Segment ein Platzhalter Zeichen (\*) als Präfix oder Suffix akzeptieren. Bei der Such Zeichenfolge wird Groß-/Kleinschreibung Die `IncludeDirectory`-Methode hat die Möglichkeit, Unterverzeichnisse zu durchsuchen.

Stellen Sie sich ein Projekt mit den folgenden JavaScript-Dateien vor:

- *Skripts\\häufig\\"addaltdeimg. js"*
- *Skripts\\gängige\\"" "" "*
- *Skripts\\gemeinsame\\"" "" "" ".*
- *Skripts\\Common\\Sub1\\"degglelinjs. js"*

![dir IMAG](bundling-and-minification/_static/image7.png)

In der folgenden Tabelle werden die einem Paket hinzugefügten Dateien wie gezeigt mit dem Platzhalter angezeigt:

| **Call** | **Hinzugefügte Dateien oder ausgelöste Ausnahme** |
| --- | --- |
| Include ("~/Scripts/Common/\*. js") | *Addaltdeimg. js*, "", " *", "* ", "", "", " *", "* ". |
| Include ("~/Scripts/Common/T\*. js") | Ungültige Muster Ausnahme. Das Platzhalter Zeichen ist nur für das Präfix oder Suffix zulässig. |
| Include ("~/Scripts/Common/\*og.\*") | Ungültige Muster Ausnahme. Nur ein Platzhalter Zeichen ist zulässig. |
| Include ("~/Scripts/Common/T\*") | " *Degglediv. js*", " *deggleimg. js* " |
| Include ("~/Scripts/Common/\*") | Ungültige Muster Ausnahme. Ein reines Platzhalter Segment ist ungültig. |
| Include Directory ("~/Scripts/Common", "T\*") | " *Degglediv. js*", " *deggleimg. js* " |
| Include Directory ("~/Scripts/Common", "T\*", true) | " *Degglediv. js* *", "* *deggleimg. js*", "" "" " |

Das explizite hinzufügen jeder Datei zu einem Bündel ist aus den folgenden Gründen im Allgemeinen das über das Platzhalter Laden von Dateien vorzuziehen:

- Das Hinzufügen von Skripts nach Platzhaltern wird standardmäßig in alphabetischer Reihenfolge geladen, was in der Regel nicht der gewünschte ist. CSS-und JavaScript-Dateien müssen häufig in einer bestimmten (nicht alphabetischen) Reihenfolge hinzugefügt werden. Sie können dieses Risiko verringern, indem Sie eine benutzerdefinierte [ibundleorderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) -Implementierung hinzufügen, aber das explizite hinzufügen der einzelnen Dateien ist weniger fehleranfällig. Beispielsweise können Sie in Zukunft neue Assets zu einem Ordner hinzufügen, die möglicherweise eine Änderung Ihrer [ibundleorderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) -Implementierung erforderlich machen.
- Anzeigen bestimmter Dateien, die einem Verzeichnis mithilfe von Platzhalter laden hinzugefügt werden, kann in alle Sichten eingeschlossen werden, die auf dieses Bündel verweisen. Wenn das Ansichts spezifische Skript zu einem Paket hinzugefügt wird, erhalten Sie möglicherweise einen JavaScript-Fehler in anderen Sichten, die auf das Paket verweisen.
- CSS-Dateien, die andere Dateien importieren, führen dazu, dass die importierten Dateien zweimal geladen werden. Der folgende Code erstellt z. b. ein Bündel mit den meisten CSS-Dateien des jQuery UI-Designs, die zweimal geladen werden. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Die Platzhalter Auswahl "\*. CSS" führt jede CSS-Datei im Ordner ein, einschließlich der *Inhalte\\Designs\\Basis\\jQuery. UI. all. CSS* -Datei. Die *jQuery. UI. all. CSS* -Datei importiert andere CSS-Dateien.

## <a name="bundle-caching"></a>Bündel Caching

Bündel legen den HTTP-Ablauf Header ein Jahr ab, wenn das Paket erstellt wird. Wenn Sie zu einer zuvor angezeigten Seite navigieren, zeigt der "fddler" an, dass der Internet Explorer keine bedingte Anforderung für das Bündel stellt, d. h. es sind keine HTTP GET-Anforderungen von IE für die Pakete und keine http 304-Antworten vom Server vorhanden. Sie können eine bedingte Anforderung für jedes Bündel mit der Taste F5 erzwingen (was zu einer HTTP 304-Antwort für jedes Bündel führt). Sie können eine vollständige Aktualisierung erzwingen, indem Sie ^ F5 verwenden (was zu einer HTTP 200-Antwort für jedes Bündel führt).

In der folgenden Abbildung ist die Registerkarte **Caching** des Antwort Bereichs "fddler" dargestellt:

![Bild zum Zwischenspeichern von "Image"](bundling-and-minification/_static/image8.png)

Die Anforderung.   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 ist für das Bundle **allmyscripts** und enthält ein Abfrage Zeichenfolgen-Paar **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Die Abfrage Zeichenfolge **v** verfügt über ein Wert Token, das ein eindeutiger Bezeichner für die Zwischenspeicherung ist. Solange das Bündel nicht geändert wird, fordert die ASP.NET-Anwendung das **allmyscripts** -Bundle mithilfe dieses Tokens an. Wenn eine Datei im Bündel geändert wird, generiert das ASP.net Optimization Framework ein neues Token, das sicherstellt, dass Browser Anforderungen für das Paket das neueste Paket erhalten.

Wenn Sie die IE9 F12-Entwicklertools ausführen und zu einer zuvor geladenen Seite navigieren, zeigt IE fälschlicherweise bedingte GET-Anforderungen an, die an die einzelnen Bündel und der Server, die http 304 zurückgeben. Sie können herausfinden, warum IE9 Probleme mit dem bestimmen, ob eine bedingte Anforderung im Blogeintrag [mithilfe von CDNs eingegangen ist und abläuft, um die Website Leistung zu verbessern](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>Weniger, coffeescript, scss, Sass-Bündelung.

Das Bundle-und Minimierung-Framework bietet einen Mechanismus zum Verarbeiten von zwischen Sprachen wie [scss](http://sass-lang.com/), [Sass](http://sass-lang.com/), [less](http://www.dotlesscss.org/) oder [coffeescript](http://coffeescript.org/)und zum Anwenden von Transformationen, wie z. b. Minimierung auf das resultierende Bündel. Fügen Sie z. b [.](http://www.dotlesscss.org/) dem MVC 4-Projekt weniger Dateien hinzu:

1. Erstellen Sie einen Ordner für Ihren weniger Inhalt. Im folgenden Beispiel wird der *Inhalt\\Ordner myless* verwendet.
2. Fügen Sie dem Projekt das nuget **-Paket "** [. Less](http://www.dotlesscss.org/) " hinzu.  
    ![nuget-Installation mit DOTLESS-](bundling-and-minification/_static/image9.png)
3. Fügen Sie eine Klasse hinzu, die die [ibundletransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) -Schnittstelle implementiert. Fügen Sie dem Projekt den folgenden Code für die. Less-Transformation hinzu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Erstellen Sie ein Paket mit weniger Dateien mit dem `LessTransform` und der [cssminify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) -Transformation. Fügen Sie der `RegisterBundles`-Methode in der *App\\_start\\BundleConfig.cs* -Datei den folgenden Code hinzu.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Fügen Sie den folgenden Code zu allen Sichten hinzu, die auf das weniger Bündel verweisen.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Bündel Überlegungen

Eine gute Konvention zum Erstellen von Paketen ist das Einschließen von "Bundle" als Präfix in den Paketnamen. Dadurch wird ein möglicher [Routing Konflikt](https://forums.asp.net/post/5012037.aspx)verhindert.

Sobald Sie eine Datei in einem Bündel aktualisieren, wird ein neues Token für den Parameter für die Paket Abfrage Zeichenfolge generiert, und das vollständige Paket muss heruntergeladen werden, wenn ein Client das nächste Mal eine Seite mit dem Paket anfordert. In herkömmlichem Markup, bei dem jedes Asset einzeln aufgelistet wird, wird nur die geänderte Datei heruntergeladen. Ressourcen, die sich häufig ändern, sind möglicherweise keine guten Kandidaten für das bündeln.

Bündelung und Minimierung verbessern hauptsächlich die Ladezeit für die erste Seiten Anforderung. Nachdem eine Webseite angefordert wurde, speichert der Browser die Assets (JavaScript, CSS und Images) zwischen, sodass das bündeln und minimieren keine Leistungssteigerung bietet, wenn dieselbe Seite angefordert wird, oder Seiten auf derselben Website, die dieselben Ressourcen anfordern. Wenn Sie den-Header nicht ordnungsgemäß für Ihre Assets festlegen, und Sie keine Bündelung und Minimierung verwenden, markieren die Heuristik der Browser Aktualität die Objekte nach einigen Tagen, und der Browser benötigt eine Überprüfungs Anforderung für jedes Asset. In diesem Fall bieten Bündelung und Minimierung eine Leistungssteigerung nach der ersten Seiten Anforderung. Weitere Informationen finden Sie im Blog [mit CDNs und läuft ab, um die Website Leistung zu verbessern](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Die Browser Beschränkung von sechs gleichzeitigen Verbindungen pro Hostname kann mithilfe eines [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)verringert werden. Da das CDN einen anderen Hostnamen als die Hostingsite hat, werden die Asset-Anforderungen vom CDN nicht in Bezug auf das Limit von sechs gleichzeitigen Verbindungen für Ihre Hostingumgebung gezählt. Ein CDN kann auch allgemeine Vorteile für das Zwischenspeichern von Paketen und Kanten Zwischenspeicherung bereitstellen.

Bündel sollten auf Seiten partitioniert werden, die Sie benötigen. Die MVC-Standardvorlage für eine Internetanwendung erstellt beispielsweise ein jQuery-Validierungs Bündel, das von jQuery getrennt ist. Da die erstellten Standard Sichten keine Eingaben aufweisen und keine Werte bereitstellen, enthalten Sie das Validierungs Bündel nicht.

Der `System.Web.Optimization`-Namespace wird in *System. Web. Optimization. dll*implementiert. Es nutzt die webgrease-Bibliothek (*webgrease. dll*) für Minimierung-Funktionen, die wiederum *Antlr3. Runtime. dll*verwendet.

*Ich verwende Twitter, um Quick Posts und Freigabe Links zu erstellen. Mein Twitter-Handle ist*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Video:[bündeln und optimieren](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) durch [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Hinzufügen der Weboptimierung zu einer Web Pages-Website](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Hinzufügen von Bündelung und Minimierung zu Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Leistungseinbußen bei der Bündelung und Minimierung von Webbrowsen](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) von [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Die Verwendung von CDNs und läuft ab, um die Website Leistung von Rick Anderson zu verbessern](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimieren von RTT (Roundtrip-Zeiten)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Mitwirkende

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana Larose
