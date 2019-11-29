---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Zwischenspeichern von Daten mit ObjectDataSourceC#() | Microsoft-Dokumentation
author: rick-anderson
description: Caching kann den Unterschied zwischen einer langsamen und einer schnellen Webanwendung bedeuten. Dieses Tutorial ist der erste von vier, bei dem Sie sich ausführlich mit dem Caching in ASP.net beschäftigen...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c9883314d6153b9816d9bad2a281ab3c0a816448
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74612621"
---
# <a name="caching-data-with-the-objectdatasource-c"></a>Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) oder [PDF herunterladen](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Caching kann den Unterschied zwischen einer langsamen und einer schnellen Webanwendung bedeuten. Dieses Tutorial ist der erste von vier, in dem Sie sich ausführlich mit dem Caching in ASP.net beschäftigen. Lernen Sie die wichtigsten Konzepte der Zwischenspeicherung und das Anwenden der Zwischenspeicherung auf die Darstellungs Schicht über das ObjectDataSource-Steuerelement.

## <a name="introduction"></a>Einführung

In Informatik ist das zwischen *Speichern* der Prozess, bei dem Daten oder Informationen, die teuer sind, zum Abrufen und Speichern einer Kopie der Daten an einem Speicherort, der schneller zugänglich ist, zu finden sind. Bei datengesteuerten Anwendungen verbrauchen große und komplexe Abfragen in der Regel den Großteil der Ausführungszeit der Anwendung. Die Leistung einer solchen Anwendung kann dann häufig verbessert werden, indem die Ergebnisse kostspieliger Datenbankabfragen im Arbeitsspeicher der Anwendung gespeichert werden.

ASP.NET 2,0 bietet eine Vielzahl von Optionen für das Zwischenspeichern. Ein gesamtes gerendertes Markup einer Webseite oder eines Benutzer Steuer Elements kann über das *Ausgabe Caching*zwischengespeichert werden. Die ObjectDataSource-und SqlDataSource-Steuerelemente stellen ebenfalls zwischen Speicherungs Funktionen bereit und ermöglichen so das Zwischenspeichern von Daten auf der Steuerungsebene. Und ASP.net s *Data Cache* bietet eine umfassende Caching-API, mit der Seiten Entwickler Objekte Programm gesteuert Zwischenspeichern können. In diesem Tutorial und den nächsten drei untersuchen wir die Verwendung der ObjectDataSource s-Cachingfeatures und des Daten Caches. Außerdem wird erläutert, wie Sie Anwendungs weite Daten beim Start Zwischenspeichern und zwischengespeicherte Daten durch die Verwendung von SQL-Cache Abhängigkeiten neu speichern. In diesen Tutorials wird das Ausgabe Caching nicht untersucht. Ausführliche Informationen zum Ausgabe Caching finden Sie unterausgabe Zwischenspeicherung [in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Zwischenspeicherung kann an beliebiger Stelle in der Architektur, von der Datenzugriffs Ebene bis hin zur Präsentationsschicht, angewendet werden. In diesem Tutorial wird das Anwenden der Zwischenspeicherung auf die Darstellungs Ebene über das ObjectDataSource-Steuerelement untersucht. Im nächsten Tutorial untersuchen wir das Zwischenspeichern von Daten auf der Geschäftslogik Ebene.

## <a name="key-caching-concepts"></a>Schlüssel Caching-Konzepte

Das Caching kann die Gesamtleistung und Skalierbarkeit einer Anwendung erheblich verbessern, indem Daten, die teuer zu generieren und zu speichern sind, an einem Speicherort gespeichert werden, der effizienter auf Sie zugreifen kann. Da der Cache nur eine Kopie der tatsächlichen, zugrunde liegenden Daten enthält, kann er veraltet oder veraltet sein, *Wenn sich die*zugrunde liegenden Daten ändern. Um dies zu bekämpfen, kann ein Seiten Entwickler Kriterien angeben, mit denen das Cache Element *aus dem Cache entfernt wird,* indem Sie Folgendes verwenden:

- **Zeitbasierte Kriterien** für eine absolute oder gleitende Dauer können dem Cache ein Element hinzugefügt werden. Ein Seiten Entwickler kann z. b. eine Dauer von etwa 60 Sekunden angeben. Bei einer absoluten Dauer wird das zwischengespeicherte Element 60 Sekunden nach dem Hinzufügen zum Cache entfernt, unabhängig davon, wie häufig darauf zugegriffen wurde. Bei einer gleitenden Dauer wird das zwischengespeicherte Element 60 Sekunden nach dem letzten Zugriff entfernt.
- **Abhängigkeitsbasiertes Kriterium** : eine Abhängigkeit kann einem Element zugeordnet werden, wenn es dem Cache hinzugefügt wird. Wenn die Abhängigkeit des Elements geändert wird, wird Sie aus dem Cache entfernt. Bei der Abhängigkeit kann es sich um eine Datei, ein anderes Cache Element oder eine Kombination der beiden handelt. ASP.NET 2,0 lässt auch SQL-Cache Abhängigkeiten zu, die es Entwicklern ermöglichen, dem Cache ein Element hinzuzufügen und zu entfernen, wenn sich die zugrunde liegenden Datenbankdaten ändern. Die SQL-Cache Abhängigkeiten werden im nächsten Tutorial zum [Verwenden von SQL-Cache Abhängigkeiten](using-sql-cache-dependencies-cs.md) untersucht.

Unabhängig von den angegebenen Entfernungs Kriterien kann ein Element im *Cache vor dem* erreichen der zeitbasierten oder Abhängigkeits basierten Kriterien geleert werden. Wenn der Cache seine Kapazität erreicht hat, müssen vorhandene Elemente entfernt werden, bevor neue Werte hinzugefügt werden können. Folglich ist es beim programmgesteuerten Arbeiten mit zwischengespeicherten Daten wichtig, dass Sie immer davon ausgehen, dass die zwischengespeicherten Daten möglicherweise nicht vorhanden sind. Wir betrachten das Muster für den programmgesteuerten Zugriff auf Daten aus dem Cache im nächsten Tutorial, *das Zwischenspeichern von Daten in der Architektur*verwendet wird.

Caching bietet ein wirtschaftliches Mittel, um die Leistung einer Anwendung zu unterdrücken. Wie [Steven Smith](http://aspadvice.com/blogs/ssmith/) in seinem Artikel [ASP.NET Caching: Techniken und bewährte Methoden](https://msdn.microsoft.com/library/aa478965.aspx)erläutert:

Caching kann eine gute Möglichkeit sein, eine gute Leistung zu erzielen, ohne viel Zeit und Analyse zu benötigen. Der Arbeitsspeicher ist kostengünstig. Wenn Sie also die benötigte Leistung abrufen können, indem Sie die Ausgabe 30 Sekunden lang Zwischenspeichern, anstatt einen Tag oder eine Woche zu investieren, um den Code oder die Datenbank zu optimieren, führen Sie die cachinglösung aus (vorausgesetzt, dass die 30 Sekunden alten Daten in Ordnung sind) und weiter. Schließlich sollten Sie sich bei schlechten Entwürfen wahrscheinlich an Sie richten. Sie sollten also natürlich versuchen, Ihre Anwendungen richtig zu entwerfen. Wenn Sie jedoch noch heute eine gute Leistung erzielen müssen, kann das Caching ein hervorragender [Ansatz] sein, um Ihnen Zeit zu bieten, Ihre Anwendung zu einem späteren Zeitpunkt zu umgestalten, wenn Sie die Zeit dafür haben.

Während die Zwischenspeicherung spürbare Leistungsverbesserungen bieten kann, ist Sie nicht in allen Situationen anwendbar, wie z. b. bei Anwendungen, die Echtzeitdaten verwenden, häufig verwendete Daten oder auch in kurzlebigen veralteten Daten sind nicht akzeptabel. Für die meisten Anwendungen sollte jedoch Caching verwendet werden. Weitere Hintergrundinformationen zum Zwischenspeichern in ASP.NET 2,0 finden Sie im Abschnitt [Caching for Performance](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) der [Schnellstart-Tutorials zu ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Schritt 1: Erstellen der Caching-Webseiten

Bevor wir mit der Untersuchung der ObjectDataSource s-Cache Features beginnen, nehmen Sie sich zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen, und die nächsten drei. Fügen Sie zunächst einen neuen Ordner mit dem Namen `Caching`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`

![Fügen Sie die ASP.NET-Seiten für die Caching-bezogenen Tutorials hinzu.](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen der ASP.NET-Seiten für die Caching-bezogenen Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `Caching` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![Abbildung 2: Hinzufügen des Benutzer Steuer Elements "sectionleveltutoriallisting. ascx" zu "default. aspx"](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Abbildung 2**: Abbildung 2: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image4.png))

Fügen Sie diese Seiten schließlich als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem `<siteMapNode>`arbeiten mit binären Daten das folgende Markup hinzu:

[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Caching-Tutorials.

![Die Site Übersicht enthält jetzt Einträge für die Caching-Tutorials.](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Abbildung 3**: die Site Übersicht enthält jetzt Einträge für die Caching-Tutorials.

## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Schritt 2: Anzeigen einer Liste von Produkten in einer Webseite

In diesem Tutorial wird erläutert, wie Sie die integrierten Caching-Features von ObjectDataSource-Steuerelementen verwenden. Bevor wir uns diese Features ansehen können, benötigen wir jedoch zuerst eine Seite, aus der wir arbeiten können. Erstellen Sie eine Webseite, die eine GridView zum Auflisten von Produktinformationen verwendet, die von einer ObjectDataSource aus der `ProductsBLL`-Klasse abgerufen werden.

Öffnen Sie zunächst die Seite `ObjectDataSource.aspx` im Ordner `Caching`. Ziehen Sie eine GridView aus der Toolbox auf den Designer, legen Sie die `ID`-Eigenschaft auf `Products`fest, und wählen Sie aus dem smarttagtag aus, dass es an ein neues ObjectDataSource-Steuerelement mit dem Namen `ProductsDataSource`gebunden werden soll. Konfigurieren Sie die ObjectDataSource, um mit der `ProductsBLL`-Klasse zu arbeiten.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbll-Klasse.](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image8.png))

Erstellen Sie auf dieser Seite eine bearbeitbare GridView, damit wir überprüfen können, was geschieht, wenn die in ObjectDataSource zwischengespeicherten Daten über die GridView s-Schnittstelle geändert werden. Belassen Sie in der Dropdown Liste auf der Registerkarte auswählen den Standardwert `GetProducts()`, aber ändern Sie das ausgewählte Element auf der Registerkarte Update in die `UpdateProduct` Überladung, die `productName`, `unitPrice`und `productID` als Eingabeparameter akzeptiert.

[![die Dropdown Liste "Update Registerkarte" auf die entsprechende UpdateProduct-Überladung festlegen](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Abbildung 5**: Festlegen der Dropdown Liste der Registerkarte "Aktualisieren" auf die entsprechende `UpdateProduct` Überladung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image11.png))

Legen Sie abschließend die Dropdown Listen auf den Registerkarten einfügen und löschen auf (keine) fest, und klicken Sie auf Fertigstellen. Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen legt Visual Studio die Eigenschaft ObjectDataSource s `OldValuesParameterFormatString` auf `original_{0}`fest. Wie in der Übersicht über das [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) erläutert, muss diese Eigenschaft aus der deklarativen Syntax entfernt oder auf ihren Standardwert zurückgesetzt werden, `{0}`, damit der Update-Workflow ohne Fehler fortgesetzt werden kann.

Außerdem fügt Visual Studio nach Abschluss des Assistenten der GridView ein Feld für jedes der Product Data-Felder hinzu. Entfernen Sie alle außer die `ProductName`, `CategoryName`und `UnitPrice` boundfields. Aktualisieren Sie als nächstes die `HeaderText` Eigenschaften der einzelnen boundfields-Elemente auf "Product", "Category" bzw. "Price". Da das Feld "`ProductName`" erforderlich ist, konvertieren Sie das BoundField in ein TemplateField-Element, und fügen Sie dem `EditItemTemplate`ein "Requirements dfieldvalidator"-Element hinzu Außerdem konvertieren Sie den `UnitPrice` BoundField in ein TemplateField-Element und fügen ein CompareValidator hinzu, um sicherzustellen, dass der vom Benutzer eingegebene Wert ein gültiger Currency-Wert ist, der größer oder gleich 0 (null) ist. Zusätzlich zu diesen Änderungen können Sie beliebige ästhetische Änderungen durchführen, z. b. den `UnitPrice` Wert rechts ausrichten oder die Formatierung für den `UnitPrice` Text in seinen schreibgeschützten und Bearbeitungs Schnittstellen angeben.

Aktivieren Sie die GridView, indem Sie das Kontrollkästchen zum Aktivieren der Bearbeitung in der GridView s-Smarttag aktivieren. Aktivieren Sie auch die Kontrollkästchen Paging aktivieren und Sortierung aktivieren.

> [!NOTE]
> Benötigen Sie einen Überblick über die Anpassung der GridView s-Bearbeitungs Schnittstelle? Wenn dies der Fall ist, lesen Sie das Tutorial [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

[![Aktivieren der GridView-Unterstützung für das Bearbeiten, Sortieren und Paging](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Abbildung 6**: Aktivieren der GridView-Unterstützung für das Bearbeiten, Sortieren und Paging ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image14.png))

Nachdem Sie diese GridView-Änderungen vorgenommen haben, sollten das deklarative Markup von GridView und ObjectDataSource in etwa wie folgt aussehen:

[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Wie in Abbildung 7 gezeigt, listet die bearbeitbare GridView den Namen, die Kategorie und den Preis der einzelnen Produkte in der Datenbank auf. Nehmen Sie sich einen Moment Zeit, um die Seitenfunktionen zu testen und die Ergebnisse zu sortieren und einen Datensatz zu bearbeiten.

[![die Namen, Kategorien und Preise der einzelnen Produkte in einer sortierbaren, kostenpflichtigen, bearbeitbaren GridView aufgeführt.](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Abbildung 7**: die Namen, Kategorien und Preise der Produkte werden in einer sortierbaren, kostenpflichtigen, bearbeitbaren GridView aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image17.png)).

## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Schritt 3: untersuchen, wann die ObjectDataSource Daten anfordert

Die `Products` GridView ruft die anzuzeigenden Daten ab, indem Sie die `Select`-Methode der `ProductsDataSource` ObjectDataSource aufruft. Diese ObjectDataSource erstellt eine Instanz der Geschäftslogik Schicht s `ProductsBLL`-Klasse und ruft Ihre `GetProducts()`-Methode auf, die wiederum die Datenzugriffs Schicht s `ProductsTableAdapter` s `GetProducts()`-Methode aufruft. Die DAL-Methode stellt eine Verbindung mit der Northwind-Datenbank her und gibt die konfigurierte `SELECT` Abfrage aus. Diese Daten werden dann an die DAL zurückgegeben, die Sie in einem `NorthwindDataTable`verpackt. Das Datentabelle-Objekt wird an die BLL zurückgegeben, die es an die ObjectDataSource zurückgibt, die es an die GridView zurückgibt. Dann erstellt das GridView-Objekt für jede `DataRow` in der Datentabelle ein `GridViewRow` Objekt, und jedes `GridViewRow` wird schließlich in den HTML-Code gerendert, der an den Client zurückgegeben und im Browser des Besuchers angezeigt wird.

Diese Ereignis Sequenz findet jedes Mal statt, wenn die GridView an die zugrunde liegenden Daten gebunden werden muss. Dies geschieht beim ersten Besuch der Seite, bei der Umstellung von einer Datenseite auf eine andere, beim Sortieren der GridView oder beim Ändern der GridView-Daten mithilfe der integrierten Bearbeitungs-oder Lösch Schnittstellen. Wenn der GridView s-Ansichts Zustand deaktiviert ist, wird die GridView bei jedem und jedem Postback ebenfalls wieder gebunden. Die GridView kann auch durch Aufrufen der `DataBind()`-Methode explizit auf die Daten zurück gehandelt werden.

Wenn Sie die Häufigkeit, mit der die Daten aus der Datenbank abgerufen werden, vollständig einschätzen möchten, können Sie eine Meldung anzeigen, die anzeigt, wann die Daten erneut abgerufen werden. Fügen Sie ein Label-websteuer Element oberhalb der GridView mit dem Namen `ODSEvents`hinzu. Löschen Sie die `Text`-Eigenschaft, und legen Sie die `EnableViewState`-Eigenschaft auf `false`fest. Fügen Sie unterhalb der Bezeichnung ein Button-websteuer Element hinzu, und legen Sie dessen `Text`-Eigenschaft auf Postback fest.

[![der Seite oberhalb der GridView eine Bezeichnung und eine Schaltfläche Hinzufügen](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Abbildung 8**: Hinzufügen einer Bezeichnung und Schaltfläche zur Seite oberhalb der GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image20.png))

Während des Datenzugriffs Workflows wird das ObjectDataSource s-`Selecting` Ereignis ausgelöst, bevor das zugrunde liegende Objekt erstellt und die konfigurierte Methode aufgerufen wird. Erstellen Sie einen Ereignishandler für dieses Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Jedes Mal, wenn die ObjectDataSource eine Anforderung an die Architektur für Daten sendet, zeigt die Bezeichnung den Text an, in dem das Ereignis ausgelöst wird.

Besuchen Sie diese Seite in einem Browser. Beim ersten Besuch der Seite wird der Text für die Auslösung des Ereignisses angezeigt. Klicken Sie auf die Schaltfläche Postback, und beachten Sie, dass der Text nicht mehr angezeigt wird (vorausgesetzt, dass die GridView s `EnableViewState`-Eigenschaft auf `true`, die Standardeinstellung) Dies liegt daran, dass beim Postback das GridView-Objekt anhand seines Ansichts Zustands rekonstruiert wird und daher nicht für seine Daten die ObjectDataSource-Datenquelle verwendet. Das Sortieren, Paging oder Bearbeiten der Daten bewirkt jedoch, dass die GridView erneut an die zugehörige Datenquelle gebunden wird. aus diesem Grund wird der Text ausgelöste Ereignis ausgelöste Text erneut angezeigt.

[![immer dann, wenn die GridView auf die Datenquelle zurückgeht, wird die Auswahl von Ereignis ausgelöst angezeigt.](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Abbildung 9**: jedes Mal, wenn die GridView auf die Datenquelle zurückgeht, wird die Auswahl von Ereignis ausgelöst angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image23.png)).

[![klicken auf die Schaltfläche "Postback" bewirkt, dass die GridView aus dem Ansichts Zustand rekonstruiert wird.](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Abbildung 10**: durch Klicken auf die Schaltfläche "Postback" wird die GridView aus dem Ansichts Zustand rekonstruiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image26.png))

Es mag verschwenderisch sein, die Datenbankdaten jedes Mal abzurufen, wenn die Daten über oder sortiert werden. Da wir Standard-Paging wieder verwenden, hat ObjectDataSource alle Datensätze beim Anzeigen der ersten Seite abgerufen. Auch wenn die GridView keine Sortier-und Pagingunterstützung bereitstellt, müssen die Daten jedes Mal aus der Datenbank abgerufen werden, wenn die Seite zum ersten Mal von einem beliebigen Benutzer besucht wird (und bei jedem Postback, wenn der Ansichts Zustand deaktiviert ist). Wenn in der GridView jedoch die gleichen Daten für alle Benutzer angezeigt werden, sind diese zusätzlichen Datenbankanforderungen überflüssig. Warum werden die von der `GetProducts()`-Methode zurückgegebenen Ergebnisse nicht zwischengespeichert, und die GridView wird an diese zwischengespeicherten Ergebnisse gebunden?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Schritt 4: Zwischenspeichern der Daten mithilfe von ObjectDataSource

Durch einfaches Festlegen einiger Eigenschaften kann die ObjectDataSource so konfiguriert werden, dass die abgerufenen Daten automatisch in den ASP.NET-Daten Cache zwischengespeichert werden. In der folgenden Liste werden die Cache bezogenen Eigenschaften von ObjectDataSource zusammengefasst:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) muss auf `true` festgelegt werden, um das Zwischenspeichern zu aktivieren. Der Standardwert ist `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) die Zeitspanne (in Sekunden), in der die Daten zwischengespeichert werden. Die Standardeinstellung ist 0. Die ObjectDataSource speichert nur dann Daten zwischen, wenn `EnableCaching` `true` ist und `CacheDuration` auf einen Wert größer als 0 (null) festgelegt ist.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) kann auf `Absolute` oder `Sliding`festgelegt werden. Wenn `Absolute`, speichert die ObjectDataSource die abgerufenen Daten für `CacheDuration` Sekunden zwischen. Wenn `Sliding`, laufen die Daten erst ab, nachdem seit `CacheDuration` Sekunden auf Sie zugegriffen wurde. Der Standardwert ist `Absolute`.
- [Cachekeyabhängigkeit](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) verwenden Sie diese Eigenschaft, um die ObjectDataSource s-Cache Einträge einer vorhandenen Cache Abhängigkeit zuzuordnen. Die Dateneinträge von ObjectDataSource s können vorzeitig aus dem Cache entfernt werden, indem die zugehörigen `CacheKeyDependency`entfernt werden. Diese Eigenschaft wird am häufigsten verwendet, um eine SQL-Cache Abhängigkeit mit dem ObjectDataSource s-Cache zuzuordnen. Dies ist ein Thema, das wir in der Zukunft [mithilfe von SQL-Cache Abhängigkeiten](using-sql-cache-dependencies-cs.md) untersuchen.

Konfigurieren Sie den `ProductsDataSource` ObjectDataSource so, dass seine Daten 30 Sekunden lang in einer absoluten Skalierung zwischengespeichert werden. Legen Sie die Eigenschaft ObjectDataSource s `EnableCaching` auf `true` und deren `CacheDuration`-Eigenschaft auf 30 fest. Belassen Sie die `CacheExpirationPolicy`-Eigenschaft auf den Standardwert `Absolute`.

[![die ObjectDataSource so konfigurieren, dass Ihre Daten 30 Sekunden lang zwischengespeichert werden.](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Abbildung 11**: Konfigurieren von ObjectDataSource zum Zwischenspeichern der zugehörigen Daten für 30 Sekunden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-with-the-objectdatasource-cs/_static/image29.png))

Speichern Sie die Änderungen, und besuchen Sie diese Seite in einem Browser erneut. Der Text ausgelöste Ereignis Auslösung wird angezeigt, wenn Sie zum ersten Mal die Seite aufrufen, da sich die Daten nicht im Cache befinden. Nachfolgende Postbacks, die durch Klicken auf die Schaltfläche "Postback", sortieren, Paging oder klicken auf die Schaltflächen "Bearbeiten" oder "Abbrechen" ausgelöst werden, zeigen jedoch *nicht* den ausgelösten Ereignis Text Dies liegt daran, dass das `Selecting` Ereignis nur ausgelöst wird, wenn die ObjectDataSource die Daten aus dem zugrunde liegenden Objekt abruft. Das `Selecting` Ereignis wird nicht ausgelöst, wenn die Daten aus dem Daten Cache abgerufen werden.

Nach 30 Sekunden werden die Daten aus dem Cache entfernt. Die Daten werden auch aus dem Cache entfernt, wenn die Methoden `Insert`, `Update`oder `Delete` von ObjectDataSource aufgerufen werden. Folglich wird, nachdem 30 Sekunden vergangen sind oder auf die Schaltfläche Aktualisieren geklickt wurde, das Sortieren, Paging oder klicken auf die Schaltflächen Bearbeiten und Abbrechen bewirkt, dass ObjectDataSource seine Daten aus dem zugrunde liegenden Objekt ablegt und das ausgelöste Ereignis auswählt, wenn das `Selecting` Ereignis ausgelöst wird. Diese zurückgegebenen Ergebnisse werden wieder in den Daten Cache eingefügt.

> [!NOTE]
> Wenn das ausgelöste Ereignis Text häufig ausgelöst angezeigt wird, kann es sein, dass auch dann, wenn die ObjectDataSource mit zwischengespeicherten Daten verwendet werden soll, aufgrund von Speicher Einschränkungen. Wenn nicht genügend freier Arbeitsspeicher verfügbar ist, wurden die Daten, die von ObjectDataSource dem Cache hinzugefügt wurden, möglicherweise geleert. Wenn ObjectDataSource die Daten nicht ordnungsgemäß zwischenspeichert oder die Daten nur sporadisch zwischenspeichert, schließen Sie einige Anwendungen, um Arbeitsspeicher freizugeben, und versuchen Sie es noch mal.

Abbildung 12 zeigt den ObjectDataSource s Caching-Workflow. Wenn der Text ausgelöste Ereignis Auslösung auf dem Bildschirm angezeigt wird, liegt dies daran, dass die Daten nicht im Cache enthalten waren und aus dem zugrunde liegenden Objekt abgerufen werden mussten. Wenn dieser Text jedoch fehlt, ist er, weil die Daten aus dem Cache verfügbar waren. Wenn die Daten aus dem Cache zurückgegeben werden, gibt es keinen Aufrufen an das zugrunde liegende Objekt, sodass keine Datenbankabfrage ausgeführt wird.

![Der ObjectDataSource speichert seine Daten und ruft Sie aus dem Daten Cache ab.](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Abbildung 12**: die ObjectDataSource speichert Ihre Daten und ruft Sie aus dem Daten Cache ab.

Jede ASP.NET-Anwendung verfügt über eine eigene Daten Cache Instanz, die von allen Seiten und Besuchern gemeinsam genutzt wird. Dies bedeutet, dass die im Daten Cache von ObjectDataSource gespeicherten Daten gleichermaßen für alle Benutzer freigegeben werden, die die Seite besuchen. Um dies zu überprüfen, öffnen Sie die Seite `ObjectDataSource.aspx` in einem Browser. Beim ersten Besuch der Seite wird der Text ausgelöste Ereignis Auslösung angezeigt (vorausgesetzt, dass die Daten, die durch vorherige Tests dem Cache hinzugefügt wurden, jetzt entfernt wurden). Öffnen Sie eine zweite Browser Instanz, kopieren Sie die URL von der ersten Browser Instanz in die zweite, und fügen Sie Sie ein. In der zweiten Browser Instanz wird der Text ausgelöste Ereignis Auslösung nicht angezeigt, da er die gleichen zwischengespeicherten Daten wie der erste verwendet.

Beim Einfügen der abgerufenen Daten in den Cache verwendet ObjectDataSource einen Cache Schlüsselwert, der Folgendes enthält: die `CacheDuration`-und `CacheExpirationPolicy`-Eigenschaftswerte. der Typ des zugrunde liegenden Geschäftsobjekts, das von ObjectDataSource verwendet wird, das über die [`TypeName`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) angegeben wird (in diesem Beispiel`ProductsBLL`); der Wert der `SelectMethod`-Eigenschaft und der Name und die Werte der Parameter in der `SelectParameters` Auflistung. und die Werte der Eigenschaften `StartRowIndex` und `MaximumRows`, die beim Implementieren von [benutzerdefiniertem Paging](../paging-and-sorting/paging-and-sorting-report-data-cs.md) verwendet werden.

Wenn Sie den Cache Schlüsselwert als eine Kombination dieser Eigenschaften erstellen, wird ein eindeutiger Cache Eintrag sichergestellt, da sich diese Werte ändern. In den vorherigen Tutorials haben wir uns z. b. mit der Verwendung der `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`beschäftigt, die alle Produkte für eine bestimmte Kategorie zurückgibt. Ein Benutzer kann auf die Seite wechseln und Getränke anzeigen, die eine `CategoryID` von 1 haben. Wenn die ObjectDataSource ihre Ergebnisse ohne Berücksichtigung der `SelectParameters` Werte zwischengespeichert hat und ein anderer Benutzer auf die Seite kam, um die Bedingungen anzuzeigen, während sich die Getränkeprodukte im Cache befanden, sehen Sie die zwischengespeicherten Getränkeprodukte anstelle von Bedingungen. Durch die Variation des Cache Schlüssels durch diese Eigenschaften, die die Werte der `SelectParameters`enthalten, behält ObjectDataSource einen separaten Cache Eintrag für Getränke und Bedingungen bei.

## <a name="stale-data-concerns"></a>Probleme mit veralteten Daten

Die ObjectDataSource entfernt ihre Elemente automatisch aus dem Cache, wenn eine ihrer `Insert`-, `Update`-oder `Delete` Methoden aufgerufen wird. Dies trägt zum Schutz vor veralteten Daten bei, indem die Cache Einträge gelöscht werden, wenn die Daten auf der Seite geändert werden. Es ist jedoch möglich, dass eine ObjectDataSource, die die Zwischenspeicherung verwendet, weiterhin veraltete Daten anzeigt. Im einfachsten Fall kann dies darauf zurückzuführen sein, dass sich die Daten direkt innerhalb der Datenbank ändern. Möglicherweise hat ein Datenbankadministrator soeben ein Skript ausgeführt, mit dem einige der Datensätze in der Datenbank geändert werden.

Dieses Szenario kann sich auch auf eine detailliertere Weise entwickeln. Während die ObjectDataSource ihre Elemente aus dem Cache entfernt, wenn eine Ihrer Daten Änderungs Methoden aufgerufen wird, sind die zwischengespeicherten Elemente, die entfernt werden, für die ObjectDataSource-Eigenschaft eine bestimmte Kombination von Eigenschafts Werten (`CacheDuration`, `TypeName`, `SelectMethod`usw.). Wenn Sie über zwei objectdatasources verfügen, die unterschiedliche `SelectMethods` oder `SelectParameters`verwenden, aber trotzdem dieselben Daten aktualisieren können, kann ein ObjectDataSource-Objekt eine Zeile aktualisieren und seine eigenen Cache Einträge für ungültig erklären, aber die entsprechende Zeile für die zweite ObjectDataSource wird weiterhin vom zwischengespeicherten bereitgestellt. Ich empfehle Ihnen, Seiten zu erstellen, um diese Funktionalität zu präsentieren. Erstellen Sie eine Seite, auf der eine bearbeitbare GridView angezeigt wird, die Ihre Daten aus einer ObjectDataSource abruft, die Caching verwendet und so konfiguriert ist, dass Sie Daten aus der `ProductsBLL` Class s `GetProducts()`-Methode abrufen. Fügen Sie dieser Seite (oder einem anderen) weitere bearbeitbare GridView-und ObjectDataSource-Objekte hinzu, aber für diese zweite ObjectDataSource wird die `GetProductsByCategoryID(categoryID)`-Methode verwendet. Da sich die beiden Eigenschaften von objectdatasources `SelectMethod` unterscheiden, verfügen Sie jeweils über eigene zwischengespeicherte Werte. Wenn Sie ein Produkt in einem Raster bearbeiten, werden bei der nächsten Bindung der Daten an das andere Raster (durch auslagern, sortieren usw.) weiterhin die alten, zwischengespeicherten Daten verarbeitet und nicht die vom anderen Raster vorgenommene Änderung widerspiegeln.

Verwenden Sie kurz gesagt nur zeitbasierte Abläufe, wenn Sie bereit sind, das Potenzial veralteter Daten zu haben, und verwenden Sie kürzere Abläufe für Szenarien, in denen die Aktualität der Daten wichtig ist. Wenn veraltete Daten nicht zulässig sind, müssen Sie entweder das verzichten-Caching oder SQL-Cache Abhängigkeiten verwenden (vorausgesetzt, es handelt sich um Datenbankdaten, die Sie neu In einem zukünftigen Tutorial untersuchen wir SQL-Cache Abhängigkeiten.

## <a name="summary"></a>Summary

In diesem Tutorial haben wir die integrierten Caching-Funktionen von ObjectDataSource untersucht. Durch einfaches Festlegen einiger Eigenschaften können wir die ObjectDataSource anweisen, die vom angegebenen `SelectMethod` zurückgegebenen Ergebnisse in den ASP.NET-Daten Cache zwischenzuspeichern. Die Eigenschaften `CacheDuration` und `CacheExpirationPolicy` geben an, wie lange das Element zwischengespeichert wird und ob es sich um einen absoluten oder gleitenden Ablauf handelt. Die `CacheKeyDependency`-Eigenschaft ordnet alle ObjectDataSource s-Cache Einträge einer vorhandenen Cache Abhängigkeit zu. Dies kann verwendet werden, um die ObjectDataSource s-Einträge aus dem Cache zu entfernen, bevor der zeitbasierte Ablauf erreicht wird. Sie wird in der Regel mit SQL-Cache Abhängigkeiten verwendet.

Da die Werte von ObjectDataSource einfach in den Daten Cache zwischengespeichert werden, können Sie die integrierte ObjectDataSource-Funktion Programm gesteuert replizieren. Es ist nicht sinnvoll, dies auf der Präsentationsebene zu tun, da die ObjectDataSource diese Funktionalität standardmäßig bietet, aber wir können zwischen Speicherungs Funktionen in einer separaten Ebene der Architektur implementieren. Zu diesem Zweck müssen wir dieselbe von ObjectDataSource verwendete Logik wiederholen. In unserem nächsten Tutorial wird erläutert, wie Sie Programm gesteuert mit dem Daten Cache in der Architektur arbeiten.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.NET Caching: Techniken und bewährte Methoden](https://msdn.microsoft.com/library/aa478965.aspx)
- [Leitfaden zum Zwischenspeichern von Architekturen für .NET Framework Anwendungen](https://msdn.microsoft.com/library/ee817645.aspx)
- [Ausgabe Zwischenspeicherung in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](caching-data-in-the-architecture-cs.md)
