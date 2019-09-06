---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Zwischenspeichern von Daten beim AnwendungsC#Start () | Microsoft-Dokumentation
author: rick-anderson
description: In jeder Webanwendung werden einige Daten häufig verwendet, und einige Daten werden selten verwendet. Wir können die Leistung unserer ASP.NET-Anwendung b verbessern...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b55b0df1b7843120de284891e16178df23fabe
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70386574"
---
# <a name="caching-data-at-application-startup-c"></a>Zwischenspeichern von Daten beim Anwendungsstart (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> In jeder Webanwendung werden einige Daten häufig verwendet, und einige Daten werden selten verwendet. Wir können die Leistung unserer ASP.NET-Anwendung verbessern, indem wir die häufig verwendeten Daten im Voraus laden. Dies ist eine Technik, die als Caching bezeichnet wird. In diesem Tutorial wird ein Ansatz für das proaktive laden veranschaulicht, bei dem beim Anwendungsstart Daten in den Cache geladen werden.

## <a name="introduction"></a>Einführung

In den beiden vorherigen Tutorials wurde das Zwischenspeichern von Daten in den Ebenen Darstellung und Caching erläutert. Beim zwischen [Speichern von Daten mit der ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)haben wir uns mit der Verwendung der Cachingfeatures von ObjectDataSource zum Zwischenspeichern von Daten in der Darstellungs Schicht beschäftigt. [Das Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-cs.md) untersuchte Zwischenspeicherung in einer neuen, separaten Cache Schicht. Für beide Tutorials wurde *reaktives laden* bei der Arbeit mit dem Daten Cache verwendet. Mit reaktivem laden überprüft das System jedes Mal, wenn die Daten angefordert werden, ob es sich im Cache befindet. Wenn dies nicht der Fall ist, werden die Daten aus der Ursprungsquelle, z. b. der Datenbank, abgerufen und im Cache gespeichert. Der Hauptvorteil von reaktivem laden ist die einfache Implementierung. Einer der Nachteile ist die ungleiche Leistung von Anforderungen. Stellen Sie sich eine Seite vor, die die zwischen Speicherungs Ebene aus dem vorherigen Tutorial verwendet, um Produktinformationen anzuzeigen. Wenn diese Seite zum ersten Mal besucht wird oder zum ersten Mal aufgerufen wird, nachdem die zwischengespeicherten Daten aufgrund von Arbeitsspeicher Einschränkungen entfernt wurden oder der angegebene Ablauf erreicht wurde, müssen die Daten aus der Datenbank abgerufen werden. Daher benötigen diese Benutzer Anforderungen länger, als von Benutzern angefordert werden, die vom Cache bedient werden können.

Das *proaktive laden* stellt eine Alternative Cache Verwaltungs Strategie dar, mit der die Leistung über Anforderungen hinweg durch Laden der zwischengespeicherten Daten vor dem Bedarf ausgecheckt wird. In der Regel verwendet das proaktive laden einen Prozess, der entweder regelmäßig überprüft oder benachrichtigt wird, wenn die zugrunde liegenden Daten aktualisiert wurden. Dieser Prozess aktualisiert dann den Cache, um ihn zu aktualisieren. Das proaktive laden ist besonders nützlich, wenn die zugrunde liegenden Daten von einer langsamen Datenbankverbindung, einem Webdienst oder einer anderen besonders langsamen Datenquelle stammen. Diese Methode für das proaktive laden ist jedoch schwieriger zu implementieren, da Sie die Erstellung, Verwaltung und Bereitstellung eines Prozesses zum Überprüfen auf Änderungen und zum Aktualisieren des Caches erfordert.

Eine weitere Art des proaktiven Ladens und der Typ, den wir in diesem Tutorial untersuchen, sind das Laden von Daten in den Cache beim Anwendungsstart. Diese Vorgehensweise ist besonders nützlich für das Zwischenspeichern statischer Daten, wie z. b. die Datensätze in Daten Bank Nachschlage Tabellen.

> [!NOTE]
> Ausführlichere Informationen zu den Unterschieden zwischen proaktivem und reaktivem laden sowie Listen von vor-und Nachteile sowie Empfehlungen zur Implementierung finden Sie im Abschnitt [Verwalten des Inhalts eines Caches](https://msdn.microsoft.com/library/ms978503.aspx) des Themas [Caching Architecture Guide for .net. Framework-Anwendungen](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Schritt 1: Bestimmen, welche Daten beim Starten der Anwendung zwischengespeichert werden sollen

Die zwischen Speicherungs Beispiele mit reaktivem laden, die wir in den vorherigen beiden Tutorials untersucht haben, funktionieren gut für Daten, die sich in regelmäßigen Abständen ändern und die Generierung nicht übermäßig lange dauert. Wenn sich die zwischengespeicherten Daten jedoch nie ändern, ist der von reaktivem laden verwendete Ablauf überflüssig. Wenn die Daten, die zwischengespeichert werden, sehr viel Zeit in Anspruch nehmen, müssen die Benutzer, deren Anforderungen den Cache Leerstellen, lange warten, bis die zugrunde liegenden Daten abgerufen werden. Es empfiehlt sich, statische Daten und Daten zwischenzuspeichern, die beim Starten der Anwendung sehr viel Zeit in Erwägung gezogen werden.

Obwohl Datenbanken viele dynamische, häufig veränderliche Werte aufweisen, verfügen die meisten auch über eine große Menge statischer Daten. Beispielsweise verfügen praktisch alle Datenmodelle über eine oder mehrere Spalten, die einen bestimmten Wert aus einem festgelegten Satz von Optionen enthalten. Eine `Patients` Datenbanktabelle kann über `PrimaryLanguage` eine Spalte verfügen, deren Satz Werte Englisch, Spanisch, Französisch, Russisch, Japanisch usw. sein könnten. Oft werden diese Spaltentypen mithilfe von Nachschlage *Tabellen*implementiert. Anstatt die Zeichenfolge Englisch oder Französisch in der `Patients` Tabelle zu speichern, wird eine zweite Tabelle mit, häufig zwei Spalten, einem eindeutigen Bezeichner und einer Zeichen folgen Beschreibung, mit einem Datensatz für jeden möglichen Wert erstellt. Die `PrimaryLanguage` -Spalte in `Patients` der-Tabelle speichert den entsprechenden eindeutigen Bezeichner in der Such Tabelle. In Abbildung 1 ist die primäre Sprache von Patient John Doe Englisch, während Ed johnes Russisch ist.

![Die Sprachen Tabelle ist eine Nachschlage Tabelle, die von der Tabelle "Patienten" verwendet wird.](caching-data-at-application-startup-cs/_static/image1.png)

**Abbildung 1**: Die `Languages` Tabelle ist eine Such Tabelle, die von der `Patients` Tabelle verwendet wird.

Die Benutzeroberfläche zum Bearbeiten oder Erstellen eines neuen Patienten würde eine Dropdown Liste zulässiger Sprachen enthalten, die durch die Datensätze in `Languages` der Tabelle aufgefüllt werden. Ohne Caching muss jedes Mal, wenn diese Schnittstelle besucht wird, das `Languages` System die Tabelle Abfragen. Dies ist verschwenderisch und unnötig, da sich Such Tabellenwerte sehr selten ändern, falls immer.

Wir könnten die `Languages` Daten mit den gleichen reaktiven lade Techniken Zwischenspeichern, die in den vorherigen Tutorials untersucht wurden. Reaktives Laden verwendet jedoch einen zeitbasierten Ablauf, der nicht für statische Nachschlage Tabellendaten benötigt wird. Das Zwischenspeichern mit reaktivem Laden wäre zwar besser als das gesamte Caching, aber der beste Ansatz wäre, die Nachschlage Tabellendaten beim Starten der Anwendung proaktiv in den Cache zu laden.

In diesem Tutorial wird erläutert, wie Nachschlage Tabellendaten und andere statische Informationen zwischengespeichert werden.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Schritt 2: Untersuchen der verschiedenen Möglichkeiten zum Zwischenspeichern von Daten

Informationen können mithilfe verschiedener Ansätze Programm gesteuert in einer ASP.NET-Anwendung zwischengespeichert werden. Wir haben bereits gesehen, wie Sie den Daten Cache in vorherigen Tutorials verwenden. Alternativ können Objekte Programm gesteuert mithilfe *statischer* Member oder des *Anwendungs Zustands*zwischengespeichert werden.

Beim Arbeiten mit einer Klasse muss die Klasse in der Regel zuerst instanziiert werden, bevor auf die Member zugegriffen werden kann. Um z. b. eine Methode aus einer der Klassen in unserer Geschäftslogik Schicht aufzurufen, müssen wir zuerst eine Instanz der-Klasse erstellen:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Bevor wir *SomeMethod* aufrufen oder mit *SomeProperty*arbeiten können, müssen wir zuerst mithilfe des `new` Schlüssel Worts eine Instanz der Klasse erstellen. *SomeMethod* und *SomeProperty* sind einer bestimmten-Instanz zugeordnet. Die Lebensdauer dieser Member ist an die Lebensdauer des zugeordneten-Objekts gebunden. *Statische*Member hingegen sind Variablen, Eigenschaften und Methoden, die von *allen* Instanzen der Klasse gemeinsam genutzt werden und folglich eine Lebensdauer aufweisen, solange die Klasse ist. Statische Member werden durch das-Schlüsselwort `static`angegeben.

Zusätzlich zu statischen Membern können Daten mithilfe des Anwendungs Zustands zwischengespeichert werden. Jede ASP.NET-Anwendung verwaltet eine Name-Wert-Sammlung, die für alle Benutzer und Seiten der Anwendung freigegeben wird. Auf diese Auflistung kann mithilfe der- [ `Application` Eigenschaft](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)der [ `HttpContext` -Klasse](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)zugegriffen werden, und Sie wird aus der Code Behind-Klasse einer ASP.NET-Seite wie folgt verwendet:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

Der Daten Cache bietet eine viel umfassendere API zum Zwischenspeichern von Daten und bietet Mechanismen für Zeit-und Abhängigkeits basierte Abläufe, Cache Element Prioritäten usw. Mit statischen Membern und dem Anwendungs Zustand müssen diese Features manuell vom Seiten Entwickler hinzugefügt werden. Beim Zwischenspeichern von Daten beim Anwendungsstart für die Lebensdauer der Anwendung werden jedoch die Vorteile des Daten Caches in den Cache verschoben. In diesem Tutorial betrachten wir den Code, in dem alle drei Techniken zum Zwischenspeichern statischer Daten verwendet werden.

## <a name="step-3-caching-thesupplierstable-data"></a>Schritt 3: Zwischenspeichern`Suppliers`der Tabellendaten

Die Northwind-Datenbanktabellen, die wir für Date implementiert haben, enthalten keine herkömmlichen Nachschlage Tabellen. Die vier in unserer dal implementierten DataTables alle Modell Tabellen, deren Werte nicht statisch sind. Anstatt die Zeit für das Hinzufügen einer neuen Datentabelle zur dal und dann eine neue Klasse und Methoden in der BLL zu verbringen, wird in diesem Tutorial lediglich festgelegt `Suppliers` , dass die Daten der Tabelle statisch sind. Daher könnten wir diese Daten beim Anwendungsstart Zwischenspeichern.

Erstellen Sie zunächst im Ordner eine neue Klasse `StaticCache.cs` mit dem `CL` Namen.

![Erstellen der StaticCache.cs-Klasse im cl-Ordner](caching-data-at-application-startup-cs/_static/image2.png)

**Abbildung 2**: Erstellen Sie `StaticCache.cs` die-Klasse `CL` im Ordner.

Wir müssen eine Methode hinzufügen, die die Daten beim Start in den entsprechenden Cache Speicher lädt, sowie Methoden, die Daten aus diesem Cache zurückgeben.

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

Der obige Code verwendet die statische Member-Variable `suppliers`,, um die Ergebnisse der- `SuppliersBLL` Methode der `GetSuppliers()` Klasse zu speichern, die von der `LoadStaticCache()` -Methode aufgerufen wird. Die `LoadStaticCache()` -Methode soll während des Starts der Anwendung aufgerufen werden. Nachdem diese Daten beim Anwendungsstart geladen wurden, kann jede Seite, die mit Lieferantendaten arbeiten muss, die- `StaticCache` Methode der `GetSuppliers()` -Klasse aufzurufen. Daher wird der Daten bankaufrufzum Abrufen der Lieferanten nur einmal beim Anwendungsstart ausgeführt.

Anstatt eine statische Element Variable als Cache Speicher zu verwenden, könnten wir alternativ den Anwendungs Status oder den Daten Cache verwenden. Der folgende Code zeigt die-Klasse, die für die Verwendung des Anwendungs Zustands neu erstellt wurde:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

In `LoadStaticCache()`werden die Lieferanteninformationen im Anwendungsvariablen *Schlüssel*gespeichert. Sie wird als geeigneter Typ (`Northwind.SuppliersDataTable`) von `GetSuppliers()`zurückgegeben. Der Zugriff auf den Anwendungs Zustand ist in den Code-Behind-Klassen von ASP.net `Application["key"]`Seiten mithilfe von möglich. in der `HttpContext.Current.Application["key"]` Architektur müssen Sie jedoch verwenden, `HttpContext`um den aktuellen zu erhalten.

Ebenso kann der Daten Cache als Cache Speicher verwendet werden, wie im folgenden Code gezeigt:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Wenn Sie dem Daten Cache ohne zeitbasierten Ablauf ein Element hinzufügen möchten, verwenden Sie `System.Web.Caching.Cache.NoAbsoluteExpiration` den `System.Web.Caching.Cache.NoSlidingExpiration` -Wert und den-Wert als Eingabeparameter. Diese spezielle Überladung der- `Insert` Methode des Daten Caches wurde ausgewählt, sodass wir die *Priorität* des Cache Elements angeben konnten. Die Priorität wird verwendet, um zu bestimmen, welche Elemente aus dem Cache gelöscht werden sollen, wenn der verfügbare Arbeitsspeicher gering ist. Hier verwenden wir die Priorität `NotRemovable`, mit der sichergestellt wird, dass dieses Cache Element nicht bereinigt wird.

> [!NOTE]
> Der Download dieses Tutorials implementiert die `StaticCache` -Klasse mithilfe des Ansatzes für statische Element Variablen. Der Code für den Anwendungs Status und die Daten Cache Techniken ist in den Kommentaren in der Klassendatei verfügbar.

## <a name="step-4-executing-code-at-application-startup"></a>Schritt 4: Ausführen von Code beim Anwendungsstart

Um Code auszuführen, wenn eine Webanwendung zum ersten Mal gestartet wird, müssen wir eine `Global.asax`spezielle Datei namens erstellen. Diese Datei kann Ereignishandler für Ereignisse auf Anwendungs-, Sitzungs-und Anforderungs Ebene enthalten. hier können wir Code hinzufügen, der immer dann ausgeführt wird, wenn die Anwendung gestartet wird.

Fügen Sie `Global.asax` die Datei zum Stammverzeichnis Ihrer Webanwendung hinzu, indem Sie mit der rechten Maustaste auf den Namen des Website Projekts in der Projektmappen-Explorer von Visual Studio klicken und neues Element hinzufügen auswählen. Wählen Sie im Dialogfeld Neues Element hinzufügen den Elementtyp globale Anwendungsklasse aus, und klicken Sie dann auf die Schaltfläche hinzufügen.

> [!NOTE]
> Wenn Sie bereits über eine `Global.asax` -Datei in Ihrem Projekt verfügen, wird der Typ der globalen Anwendungsklasse nicht im Dialogfeld Neues Element hinzufügen aufgelistet.

[![Fügen Sie die Datei "Global. asax" dem Stammverzeichnis Ihrer Webanwendung hinzu.](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Abbildung 3**: Fügen Sie `Global.asax` die Datei dem Stammverzeichnis Ihrer Webanwendung hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-at-application-startup-cs/_static/image5.png)).

Die Standard `Global.asax` mäßige Datei Vorlage umfasst fünf Methoden innerhalb eines serverseitigen `<script>` Tags:

- **`Application_Start`** wird beim ersten Starten der Webanwendung ausgeführt.
- **`Application_End`** wird ausgeführt, wenn die Anwendung heruntergefahren wird.
- **`Application_Error`** wird ausgeführt, wenn eine nicht behandelte Ausnahme die Anwendung erreicht.
- **`Session_Start`** wird ausgeführt, wenn eine neue Sitzung erstellt wird.
- **`Session_End`** wird ausgeführt, wenn eine Sitzung abgelaufen oder abgebrochen wird.

Der `Application_Start` Ereignishandler wird während des Lebenszyklus einer Anwendung nur einmal aufgerufen. Die Anwendung wird gestartet, wenn eine ASP.NET-Ressource zum ersten Mal von der Anwendung angefordert wird und weiterhin ausgeführt wird, bis die Anwendung neu gestartet wird. Dies kann durch `/Bin` Ändern des Inhalts `Global.asax`des Ordners, ändern und Ändern des der Inhalt des `App_Code` Ordners oder das ändern `Web.config` der Datei. Eine ausführlichere Erläuterung des Anwendungslebenszyklus finden Sie unter [ASP.NET Application Lifecycle Overview (Übersicht über den Anwendungslebenszyklus](https://msdn.microsoft.com/library/ms178473.aspx) ).

Für diese Tutorials müssen wir nur der- `Application_Start` Methode Code hinzufügen. Sie können also die anderen entfernen. Aufrufen `Application_Start`Sie in einfach die `StaticCache` -Methode `LoadStaticCache()` der-Klasse, mit der die Lieferanteninformationen geladen und zwischengespeichert werden:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

Das ist schon alles! Beim Anwendungsstart nimmt die `LoadStaticCache()` -Methode die Lieferanteninformationen aus der BLL auf und speichert Sie in einer statischen Element Variablen (bzw. in einem beliebigen Cache Speicher, den Sie in `StaticCache` der-Klasse verwendet haben). Um dieses Verhalten zu überprüfen, legen Sie einen halte `Application_Start` Punkt in der Methode fest, und führen Sie die Anwendung aus. Beachten Sie, dass der Breakpoint beim Starten der Anwendung gedrückt wird. Nachfolgende Anforderungen bewirken jedoch nicht, dass die `Application_Start` -Methode ausgeführt wird.

[![Verwenden Sie einen Haltepunkt, um zu überprüfen, ob der Application_Start-Ereignis Handler ausgeführt wird.](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Abbildung 4**: Verwenden Sie einen Haltepunkt, um zu `Application_Start` überprüfen, ob der Ereignis Handler ausgeführt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-at-application-startup-cs/_static/image8.png)).

> [!NOTE]
> Wenn Sie den Haltepunkt beim `Application_Start` ersten Starten des Debuggens nicht erreichen, liegt dies daran, dass die Anwendung bereits gestartet wurde. Erzwingen Sie, dass die Anwendung neu gestartet `Global.asax` wird `Web.config` , indem Sie Ihre-oder-Dateien ändern, und wiederholen Sie können am Ende einer dieser Dateien einfach eine leere Zeile hinzufügen (oder entfernen), um die Anwendung schnell neu zu starten.

## <a name="step-5-displaying-the-cached-data"></a>Schritt 5: Anzeigen der zwischengespeicherten Daten

An diesem Punkt verfügt `StaticCache` die-Klasse über eine Version der Lieferantendaten, die beim Starten der Anwendung zwischengespeichert werden `GetSuppliers()` , auf die über die-Methode zugegriffen werden kann. Um mit diesen Daten von der Präsentationsebene zu arbeiten, können wir eine ObjectDataSource verwenden oder die- `StaticCache` `GetSuppliers()` Methode der Klasse Programm gesteuert aus der Code Behind-Klasse einer ASP.NET-Seite aufrufen. Sehen wir uns nun die Verwendung der ObjectDataSource-und GridView-Steuerelemente an, um die zwischengespeicherten Lieferanteninformationen anzuzeigen.

Öffnen Sie zunächst die `AtApplicationStartup.aspx` Seite `Caching` im Ordner. Ziehen Sie eine GridView aus der Toolbox auf den Designer, und `ID` legen Sie `Suppliers`die zugehörige-Eigenschaft auf fest. Wählen Sie als nächstes im Smarttagmenü der GridView eine neue ObjectDataSource mit `SuppliersCachedDataSource`dem Namen aus. Konfigurieren Sie ObjectDataSource so, dass `StaticCache` die- `GetSuppliers()` Methode der-Klasse verwendet wird.

[![Konfigurieren von ObjectDataSource für die Verwendung der staticcache-Klasse](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren von ObjectDataSource für die Verwendung `StaticCache` der-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-at-application-startup-cs/_static/image11.png))

[![Verwenden der getsuppliers ()-Methode zum Abrufen der zwischengespeicherten Lieferantendaten](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Abbildung 6**: Verwenden Sie `GetSuppliers()` die-Methode zum Abrufen der zwischengespeicherten Lieferantendaten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-at-application-startup-cs/_static/image14.png)).

Nachdem Sie den Assistenten abgeschlossen haben, fügt Visual Studio automatisch boundfields für jedes der Datenfelder in `SuppliersDataTable`hinzu. Das deklarative Markup von GridView und ObjectDataSource sollte in etwa wie folgt aussehen:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

Abbildung 7 zeigt die Seite, wenn Sie in einem Browser angezeigt wird. Die Ausgabe ist identisch, da wir die Daten aus der BLL-Klasse `SuppliersBLL` abgerufen haben. die Verwendung `StaticCache` der-Klasse gibt jedoch die Lieferantendaten zurück, die beim Starten der Anwendung zwischengespeichert wurden. Sie können Breakpoints in der- `StaticCache` Methode der `GetSuppliers()` -Klasse festlegen, um dieses Verhalten zu überprüfen.

[![Die zwischengespeicherten Lieferantendaten werden in einer GridView angezeigt.](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Abbildung 7**: Die zwischengespeicherten Lieferantendaten werden in einer GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-at-application-startup-cs/_static/image17.png)).

## <a name="summary"></a>Zusammenfassung

Die meisten Datenmodelle enthalten eine große Menge statischer Daten, die in der Regel in Form von Nachschlage Tabellen implementiert werden. Da diese Informationen statisch sind, gibt es keinen Grund, immer wieder auf die Datenbank zuzugreifen, wenn diese Informationen angezeigt werden müssen. Außerdem ist es aufgrund seiner statischen Natur, dass beim Zwischenspeichern der Daten keine Ablaufzeit erforderlich ist. In diesem Tutorial haben Sie erfahren, wie Sie solche Daten in den Daten Cache, den Anwendungs Status und über eine statische Element Variable Zwischenspeichern. Diese Informationen werden beim Starten der Anwendung zwischengespeichert und bleiben während der gesamten Lebensdauer der Anwendung im Cache.

In diesem Tutorial und in den letzten beiden Ausführungen haben wir uns mit dem Zwischenspeichern von Daten für die Dauer der Lebensdauer der Anwendung und der Verwendung zeitbasierter Abläufe beschäftigt. Beim Zwischenspeichern von Datenbankdaten kann ein zeitbasierter Ablauf jedoch kleiner als ideal sein. Anstatt den Cache regelmäßig zu leeren, wäre es optimal, das zwischengespeicherte Element nur zu entfernen, wenn die zugrunde liegenden Datenbankdaten geändert werden. Dieses ideal ist durch die Verwendung von SQL-Cache Abhängigkeiten möglich, die wir im nächsten Tutorial untersuchen werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann mit [ mitchell@4GuysFromRolla.comerreicht werden.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Teresa Murphy und Zack Jones. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie die Zeile in [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](caching-data-in-the-architecture-cs.md)
> [Weiter](using-sql-cache-dependencies-cs.md)
