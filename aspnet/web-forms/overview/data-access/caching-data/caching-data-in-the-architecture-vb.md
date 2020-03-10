---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Zwischenspeichern von Daten in der Architektur (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Im vorherigen Tutorial haben Sie erfahren, wie Sie die Zwischenspeicherung auf der Präsentationsebene anwenden. In diesem Tutorial erfahren Sie, wie Sie unsere geschichtete Architektur nutzen...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: dc991a205fa7e61f604bc0f26e9b24b3faefd3d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78443589"
---
# <a name="caching-data-in-the-architecture-vb"></a>Zwischenspeichern von Daten in der Architektur (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) oder [PDF herunterladen](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> Im vorherigen Tutorial haben Sie erfahren, wie Sie die Zwischenspeicherung auf der Präsentationsebene anwenden. In diesem Tutorial erfahren Sie, wie Sie unsere geschichtete Architektur nutzen können, um Daten auf der Geschäftslogik Schicht zwischenzuspeichern. Dies erreichen Sie, indem Sie die Architektur so erweitern, dass Sie eine cachingschicht einschließt.

## <a name="introduction"></a>Einführung

Wie bereits im vorherigen Tutorial gezeigt, ist das Zwischenspeichern der ObjectDataSource s-Daten so einfach wie das Festlegen einiger Eigenschaften. Leider wendet ObjectDataSource das Caching auf der Präsentationsschicht an, die die zwischen Speicherungs Richtlinien eng mit der ASP.NET-Seite verbindet. Einer der Gründe für das Erstellen einer mehrschichtigen Architektur besteht darin, dass solche Verknüpfungen beschädigt werden können. Die Geschäftslogik Schicht entkoppelt beispielsweise die Geschäftslogik von den ASP.NET Seiten, während die Datenzugriffs Ebene die Datenzugriffs Details entkoppelt. Diese Entkopplung von Geschäftslogik-und Datenzugriffs Details wird teilweise bevorzugt, da das System besser lesbar, besser verwaltbar und flexibler zu ändern ist. Außerdem ist die Verwendung von Domänen Kenntnissen und Arbeitsabteilung möglich, da ein Entwickler, der auf der Präsentationsschicht arbeitet, nicht mit den Details der Datenbank vertraut sein muss, um Ihre Arbeit zu erledigen. Das Entkoppeln der Cachingrichtlinie von der Präsentationsebene bietet ähnliche Vorteile.

In diesem Tutorial erweitern wir unsere Architektur um eine zwischen Speicherungs *Schicht* (oder kurz cl), die unsere Cachingrichtlinie einsetzt. Die zwischen Speicherungs Ebene enthält eine `ProductsCL` Klasse, die den Zugriff auf Produktinformationen mit Methoden wie `GetProducts()`, `GetProductsByCategoryID(categoryID)`usw. ermöglicht, die beim Aufrufen zuerst versucht, die Daten aus dem Cache abzurufen. Wenn der Cache leer ist, rufen diese Methoden die entsprechende `ProductsBLL` Methode in der BLL auf, die wiederum die Daten aus der dal abruft. Die `ProductsCL`-Methoden Zwischenspeichern die aus der BLL abgerufenen Daten, bevor Sie zurückgegeben werden.

Wie in Abbildung 1 gezeigt, befindet sich die CL zwischen der Präsentations-und der Geschäftslogik Schicht.

![Die Cache Schicht (CL) ist eine andere Ebene in unserer Architektur.](caching-data-in-the-architecture-vb/_static/image1.png)

**Abbildung 1**: die Cache Schicht (CL) ist eine andere Ebene in unserer Architektur.

## <a name="step-1-creating-the-caching-layer-classes"></a>Schritt 1: Erstellen der cachingebenenklassen

In diesem Tutorial erstellen wir eine sehr einfache cl mit einer einzigen Klasse `ProductsCL`, die nur eine Handvoll Methoden aufweist. Das Erstellen einer vollständigen zwischen Speicherungs Schicht für die gesamte Anwendung erfordert das Erstellen von `CategoriesCL`-, `EmployeesCL`-und `SuppliersCL` Klassen und das Bereitstellen einer Methode in diesen cachingebenenklassen für jede Datenzugriffs-oder Änderungs Methode in der BLL. Wie bei BLL und Dal sollte die cachingschicht idealerweise als separates Klassen Bibliotheksprojekt implementiert werden. Wir implementieren es jedoch als Klasse im Ordner "`App_Code`".

Wenn Sie die CL-Klassen von den Klassen dal und BLL bereinigen möchten, erstellen Sie einen neuen Unterordner im Ordner `App_Code`. Klicken Sie mit der rechten Maustaste auf den Ordner `App_Code` im Projektmappen-Explorer, wählen Sie neuer Ordner aus, und benennen Sie den neuen Ordner `CL`. Nachdem Sie diesen Ordner erstellt haben, fügen Sie ihm eine neue Klasse mit dem Namen `ProductsCL.vb`hinzu.

![Fügen Sie einen neuen Ordner mit dem Namen cl und eine Klasse mit dem Namen productscl. vb hinzu.](caching-data-in-the-architecture-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen eines neuen Ordners mit dem Namen `CL` und einer Klasse mit dem Namen `ProductsCL.vb`

Die `ProductsCL`-Klasse sollte denselben Satz von Datenzugriffs-und Änderungs Methoden enthalten, die in der entsprechenden Business Logic Layer-Klasse (`ProductsBLL`) gefunden werden. Anstatt alle diese Methoden zu erstellen, können Sie hier nur ein paar erstellen, um ein Gefühl für die von der CL verwendeten Muster zu erhalten. Insbesondere fügen wir die Methoden "`GetProducts()`" und "`GetProductsByCategoryID(categoryID)`" in Schritt 3 und eine `UpdateProduct` Überladung in Schritt 4 hinzu. Sie können die verbleibenden `ProductsCL` Methoden und `CategoriesCL`-, `EmployeesCL`-und `SuppliersCL`-Klassen in ihrer Freizeit hinzufügen.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Schritt 2: Lesen und Schreiben in den Daten Cache

Die im vorherigen Tutorial untersuchte ObjectDataSource-Caching-Funktion verwendet intern den ASP.NET-Daten Cache, um die aus der BLL abgerufenen Daten zu speichern. Auf den Daten Cache kann auch Programm gesteuert über ASP.net Pages Code-Behind-Klassen oder über die Klassen in der Architektur der Webanwendung zugegriffen werden. Verwenden Sie das folgende Muster, um den Daten Cache aus einer Code Behind-Klasse der ASP.net page s zu lesen und in den Daten Cache zu schreiben:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

Die [`Cache` Class](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [`Insert`-Methode](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) verfügt über eine Reihe von über Ladungen. `Cache("key") = value` und `Cache.Insert(key, value)` sind Synonym, und beide fügen dem Cache ein Element mit dem angegebenen Schlüssel ohne einen definierten Ablauf hinzu. In der Regel soll ein Ablaufdatum angegeben werden, wenn ein Element zum Cache hinzugefügt wird, entweder als Abhängigkeit, als zeitbasierter Ablauf oder beides. Verwenden Sie eine der anderen `Insert`-Methoden Überladungen, um Abhängigkeits-oder zeitbasierte Ablauf Informationen bereitzustellen.

Die Methoden der Caching-Methode müssen zunächst überprüfen, ob die angeforderten Daten im Cache vorhanden sind. wenn dies der Fall ist, können Sie Sie von dort aus zurückgeben. Wenn sich die angeforderten Daten nicht im Cache befinden, muss die entsprechende BLL-Methode aufgerufen werden. Der Rückgabewert sollte zwischengespeichert und dann zurückgegeben werden, wie im folgenden Sequenzdiagramm veranschaulicht.

![Die s-Methoden der cachingschicht geben Daten aus dem Cache zurück, wenn Sie verfügbar sind.](caching-data-in-the-architecture-vb/_static/image3.png)

**Abbildung 3**: die Methoden zum Zwischenspeichern von Layer-Methoden geben Daten aus dem Cache zurück, wenn Sie verfügbar sind

Die in Abbildung 3 dargestellte Sequenz wird in den cl-Klassen mithilfe des folgenden Musters erreicht:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Hier ist *Type* der Typ der Daten, die im Cache `Northwind.ProductsDataTable`gespeichert werden, z. b., während *Key* der Schlüssel ist, der das Cache Element eindeutig identifiziert. Wenn sich das Element mit dem angegebenen *Schlüssel* nicht im Cache befindet, wird die *Instanz* `Nothing`, und die Daten werden aus der entsprechenden BLL-Methode abgerufen und dem Cache hinzugefügt. Wenn `Return instance` erreicht ist, enthält die *Instanz* einen Verweis auf die Daten, entweder aus dem Cache oder aus der BLL.

Stellen Sie sicher, dass Sie das obige Muster verwenden, wenn Sie auf Daten aus dem Cache zugreifen. Das folgende Muster, das auf den ersten Blick gleichwertig aussieht, enthält einen geringfügigen Unterschied, der eine Racebedingung einführt. Racebedingungen sind schwer zu debuggen, da Sie sich sporadisch offenlegen und schwer zu reproduzieren sind.

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

Der Unterschied in diesem zweiten, falschen Code Ausschnitt besteht darin, dass statt eines Verweises auf das zwischengespeicherte Element in einer lokalen Variablen direkt in der Bedingungs Anweisung *und* in der `Return`auf den Daten Cache zugegriffen wird. Angenommen, `Cache("key")` wird nicht `Nothing`, aber bevor die `Return`-Anweisung erreicht wird, entfernt das System den *Schlüssel* aus dem Cache. In diesem seltenen Fall gibt der Code `Nothing` anstelle eines Objekts vom erwarteten Typ zurück.

> [!NOTE]
> Der Daten Cache ist Thread sicher, sodass Sie den Thread Zugriff für einfache Lese-oder Schreibvorgänge nicht synchronisieren müssen. Wenn Sie jedoch mehrere Vorgänge für Daten im Cache durchführen müssen, die atomar sein müssen, sind Sie dafür verantwortlich, eine Sperre oder einen anderen Mechanismus zu implementieren, um die Thread Sicherheit sicherzustellen. Weitere Informationen finden [Sie unter Synchronisierung des Zugriffs auf den ASP.NET-Cache](http://www.ddj.com/184406369) .

Ein Element kann Programm gesteuert aus dem Daten Cache entfernt werden, indem die [`Remove`-Methode](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) wie folgt verwendet wird:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Schritt 3: Zurückgeben von Produktinformationen aus der`ProductsCL`-Klasse

In diesem Tutorial implementieren Sie zwei Methoden zum Zurückgeben von Produktinformationen aus der `ProductsCL`-Klasse: `GetProducts()` und `GetProductsByCategoryID(categoryID)`. Wie bei der `ProductsBL`-Klasse in der Geschäftslogik Schicht gibt die `GetProducts()`-Methode in der CL Informationen über alle Produkte als `Northwind.ProductsDataTable` Objekt zurück, während `GetProductsByCategoryID(categoryID)` alle Produkte aus einer angegebenen Kategorie zurückgibt.

Der folgende Code zeigt einen Teil der Methoden in der `ProductsCL`-Klasse:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Beachten Sie zunächst die `DataObject`-und `DataObjectMethodAttribute` Attribute, die auf die-Klasse und die-Methode angewendet werden. Diese Attribute stellen Informationen für den ObjectDataSource-Assistenten bereit, der angibt, welche Klassen und Methoden in den Schritten des Assistenten angezeigt werden sollen. Da auf die CL-Klassen und-Methoden von einer ObjectDataSource in der Präsentationsebene zugegriffen wird, habe ich diese Attribute hinzugefügt, um die Entwurfszeit Funktion zu verbessern. Eine ausführlichere Beschreibung dieser Attribute und ihrer Auswirkungen finden Sie im Tutorial zum [Erstellen einer Geschäftslogik Ebene](../introduction/creating-a-business-logic-layer-vb.md) .

In den Methoden `GetProducts()` und `GetProductsByCategoryID(categoryID)` werden die von der `GetCacheItem(key)`-Methode zurückgegebenen Daten einer lokalen Variablen zugewiesen. Die `GetCacheItem(key)`-Methode, die wir kurz untersuchen, gibt basierend auf dem angegebenen *Schlüssel*ein bestimmtes Element aus dem Cache zurück. Wenn keine solchen Daten im Cache gefunden werden, werden Sie aus der entsprechenden `ProductsBLL` Class-Methode abgerufen und dann mithilfe der `AddCacheItem(key, value)`-Methode dem Cache hinzugefügt.

Die `GetCacheItem(key)`-und `AddCacheItem(key, value)` Methoden-Schnittstelle mit dem Daten Cache, dem Lesen und Schreiben von Werten. Die `GetCacheItem(key)`-Methode ist die einfachere der beiden. Sie gibt einfach den Wert aus der Cache-Klasse zurück, indem Sie den folgenden *Schlüssel*verwendet:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` verwendet keinen *Schlüssel* Wert wie angegeben, sondern ruft stattdessen die `GetCacheKey(key)`-Methode auf, die den *Schlüssel* zurückgibt, der mit productscache vorangestellt ist. Der `MasterCacheKeyArray`, der die Zeichenfolge productscache enthält, wird auch von der `AddCacheItem(key, value)`-Methode verwendet, wie wir es vorübergehend sehen werden.

Aus einer Code Behind-Klasse der ASP.net page s kann auf den Daten Cache mithilfe der `Page`-Klasse [`Cache`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)zugegriffen werden, und es wird eine Syntax wie `Cache("key") = value`ermöglicht, wie in Schritt 2 beschrieben. Aus einer Klasse innerhalb der Architektur kann auf den Daten Cache mithilfe von `HttpRuntime.Cache` oder `HttpContext.Current.Cache`zugegriffen werden. Der Blogeintrag von [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx) [HttpRuntime. Cache im Vergleich zu HttpContext. Current. Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) notiert den geringfügigen Leistungsvorteil bei der Verwendung von `HttpRuntime` anstelle `HttpContext.Current`. Folglich verwendet `ProductsCL` `HttpRuntime`.

> [!NOTE]
> Wenn Ihre Architektur mithilfe von Klassen Bibliotheks Projekten implementiert wird, müssen Sie einen Verweis auf die `System.Web` Assembly hinzufügen, um die Klassen [`HttpRuntime`](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) und [`HttpContext`](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) zu verwenden.

Wenn das Element nicht im Cache gefunden wird, werden die Daten der `ProductsCL`-Klasse mithilfe der `AddCacheItem(key, value)`-Methode dem Cache hinzugefügt. Um dem Cache einen *Wert* hinzuzufügen, können wir den folgenden Code verwenden, der einen 60 Sekunden Ablauf verwendet:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` gibt den zeitbasierten Ablauf 60 Sekunden in der Zukunft an, während [`System.Web.Caching.Cache.NoSlidingExpiration`](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) angibt, dass es keinen gleitenden Ablauf gibt. Wenngleich diese `Insert`-Methoden Überladung Eingabeparameter für einen absoluten und gleitenden Ablauf hat, können Sie nur eine der beiden bereitstellen. Wenn Sie versuchen, sowohl eine absolute Zeit als auch eine Zeitspanne anzugeben, löst die `Insert` Methode eine `ArgumentException`-Ausnahme aus.

> [!NOTE]
> Diese Implementierung der `AddCacheItem(key, value)`-Methode hat derzeit einige Mängel. Diese Probleme werden in Schritt 4 behandelt und gelöst.

## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Schritt 4: invalidieren des Caches, wenn die Daten durch die Architektur geändert werden

Zusammen mit den Methoden zum Abrufen von Daten muss die zwischen Speicherungs Schicht die gleichen Methoden wie das BLL zum Einfügen, aktualisieren und Löschen von Daten bereitstellen. Die Daten Änderungs Methoden der CL-Daten ändern die zwischengespeicherten Daten nicht, sondern führen stattdessen die entsprechende Daten Änderungs Methode für BLL aus und erklären den Cache dann für ungültig. Wie bereits im vorherigen Tutorial gezeigt, ist dies das gleiche Verhalten, das ObjectDataSource anwendet, wenn die zwischen Speicherungs Funktionen aktiviert sind und die `Insert`-, `Update`-oder `Delete` Methoden aufgerufen werden.

Die folgende `UpdateProduct` Überladung veranschaulicht, wie die Daten Änderungs Methoden in der CL implementiert werden:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Die entsprechende Daten Änderungs Methode für die Geschäftslogik Schicht wird aufgerufen, aber bevor die Antwort zurückgegeben wird, müssen wir den Cache für ungültig erklären. Leider ist das invalidieren des Caches nicht unkompliziert, da die `ProductsCL`-Klasse `GetProducts()` und `GetProductsByCategoryID(categoryID)`-Methoden jeweils dem Cache Elemente mit unterschiedlichen Schlüsseln hinzufügen, und die `GetProductsByCategoryID(categoryID)`-Methode fügt ein anderes Cache Element für jede eindeutige *CategoryID*hinzu.

Wenn Sie den Cache für ungültig erklären, müssen wir *alle* Elemente entfernen, die möglicherweise von der `ProductsCL`-Klasse hinzugefügt wurden. Dies kann erreicht werden, indem eine *Cache Abhängigkeit* mit jedem Element verknüpft wird, das dem Cache in der `AddCacheItem(key, value)`-Methode hinzugefügt wird. Im Allgemeinen kann eine Cache Abhängigkeit ein anderes Element im Cache, eine Datei im Dateisystem oder Daten aus einer Microsoft SQL Server Datenbank sein. Wenn sich die Abhängigkeit ändert oder aus dem Cache entfernt wird, werden die zugeordneten Cache Elemente automatisch aus dem Cache entfernt. Für dieses Tutorial möchten wir ein zusätzliches Element im Cache erstellen, das als Cache Abhängigkeit für alle Elemente dient, die über die `ProductsCL`-Klasse hinzugefügt werden. Auf diese Weise können alle diese Elemente aus dem Cache entfernt werden, indem einfach die Cache Abhängigkeit entfernt wird.

Aktualisieren Sie die `AddCacheItem(key, value)`-Methode so, dass jedes Element, das über diese Methode dem Cache hinzugefügt wird, einer einzelnen Cache Abhängigkeit zugeordnet ist:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` ist ein Zeichen folgen Array, das einen einzelnen Wert (productscache) enthält. Zuerst wird dem Cache ein Cache Element hinzugefügt und das aktuelle Datum und die Uhrzeit zugewiesen. Wenn das Cache Element bereits vorhanden ist, wird es aktualisiert. Als nächstes wird eine Cache Abhängigkeit erstellt. Der [`CacheDependency` Klasse](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s-Konstruktor verfügt über eine Reihe von über Ladungen, aber der in diesem verwendete Wert erwartet zwei `String` Array Eingaben. Der erste gibt den Satz von Dateien an, die als Abhängigkeiten verwendet werden sollen. Da wir keine dateibasierten Abhängigkeiten verwenden möchten, wird der Wert `Nothing` für den ersten Eingabeparameter verwendet. Der zweite Eingabeparameter gibt den Satz von Cache Schlüsseln an, die als Abhängigkeiten verwendet werden sollen. Hier geben wir unsere einzige Abhängigkeit an, `MasterCacheKeyArray`. Der `CacheDependency` wird dann an die `Insert`-Methode weitergeleitet.

Mit dieser Änderung an `AddCacheItem(key, value)`ist das invalidieren des Caches so einfach wie das Entfernen der Abhängigkeit.

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Schritt 5: Aufrufen der zwischen Speicherungs Ebene von der Präsentationsebene

Die Klassen und Methoden der Cache Schicht können verwendet werden, um mithilfe der Techniken, die wir in diesen Tutorials untersucht haben, mit Daten zu arbeiten. Um die Arbeit mit zwischengespeicherten Daten zu veranschaulichen, speichern Sie die Änderungen in der `ProductsCL`-Klasse, und öffnen Sie dann die `FromTheArchitecture.aspx` Seite im Ordner `Caching`, und fügen Sie eine GridView hinzu. Erstellen Sie aus dem GridView s-Smarttag eine neue ObjectDataSource. Im ersten Schritt des Assistenten sollten Sie die `ProductsCL`-Klasse als eine der Optionen in der Dropdown Liste sehen.

[![die productscl-Klasse in der Dropdown Liste für das Geschäftsobjekt enthalten ist.](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Abbildung 4**: die `ProductsCL` Klasse ist in der Dropdown Liste für das Geschäftsobjekt enthalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-in-the-architecture-vb/_static/image6.png))

Nachdem Sie `ProductsCL`ausgewählt haben, klicken Sie auf Weiter. Die Dropdown Liste auf der Registerkarte auswählen enthält zwei Elemente: `GetProducts()` und `GetProductsByCategoryID(categoryID)` und die Registerkarte Update die einzige `UpdateProduct` Überladung. Wählen Sie die `GetProducts()`-Methode auf der Registerkarte auswählen und `UpdateProducts` auf der Registerkarte Aktualisieren aus, und klicken Sie dann auf Fertigstellen.

[![die Methoden der productscl Class s in den Dropdown Listen aufgeführt.](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Abbildung 5**: die Methoden der `ProductsCL`-Klasse werden in den Dropdown Listen aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](caching-data-in-the-architecture-vb/_static/image9.png))

Nachdem Sie den Assistenten abgeschlossen haben, wird die Eigenschaft ObjectDataSource s `OldValuesParameterFormatString` von Visual Studio auf `original_{0}` festgelegt, und der GridView werden die entsprechenden Felder hinzugefügt. Ändern Sie die `OldValuesParameterFormatString`-Eigenschaft auf ihren Standardwert zurück, `{0}`, und konfigurieren Sie die GridView, um das Paging, Sortieren und bearbeiten zu unterstützen. Da die von der CL verwendete `UploadProducts` Überladung nur den Namen und den Preis der bearbeiteten Produkte akzeptiert, schränken Sie die GridView ein, sodass nur diese Felder bearbeitet werden können.

Im vorherigen Tutorial haben wir eine GridView definiert, um Felder für die Felder `ProductName`, `CategoryName`und `UnitPrice` einzuschließen. Sie können diese Formatierung und Struktur auch replizieren. in diesem Fall sollten das deklarative Markup der GridView-und ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

An dieser Stelle haben wir eine Seite, die die zwischen Speicherungs Ebene verwendet. Um den Cache in Aktion zu sehen, legen Sie Breakpoints in der `ProductsCL`-Klasse `GetProducts()` und `UpdateProduct`-Methoden fest. Besuchen Sie die Seite in einem Browser, und durchlaufen Sie den Code beim Sortieren und Paging, um die aus dem Cache gezogenen Daten anzuzeigen. Aktualisieren Sie anschließend einen Datensatz, und beachten Sie, dass der Cache ungültig wird und daher aus der BLL abgerufen wird, wenn die Daten an die GridView zurückgegeben werden.

> [!NOTE]
> Die im Download bereitgestellte Cache Ebene, die diesen Artikel begleitet, ist nicht fertig. Sie enthält nur eine Klasse, `ProductsCL`, die nur eine Handvoll Methoden Sport hat. Darüber hinaus verwendet nur eine einzelne ASP.NET-Seite die CL (`~/Caching/FromTheArchitecture.aspx`) alle anderen Benutzer verweisen weiterhin direkt auf die BLL. Wenn Sie beabsichtigen, eine cl in Ihrer Anwendung zu verwenden, sollten alle Aufrufe von der Präsentationsschicht an die CL weitergeleitet werden. Dies erfordert, dass die cl s-Klassen und-Methoden diese Klassen und Methoden in der von der Präsentationsebene verwendeten BLL behandelt haben.

## <a name="summary"></a>Zusammenfassung

Während das Zwischenspeichern auf der Präsentationsebene mit ASP.NET 2,0 s SqlDataSource-und ObjectDataSource-Steuerelementen angewendet werden kann, werden idealerweise zwischen Speicherungs Zuständigkeiten an eine separate Ebene in der Architektur delegiert. In diesem Tutorial haben wir eine zwischen Speicherungs Schicht und die Geschäftslogik Schicht zwischen der Darstellungs Schicht und der Geschäftslogik Ebene erstellt. Die zwischen Speicherungs Schicht muss denselben Satz von Klassen und Methoden bereitstellen, die in der BLL vorhanden sind und von der Präsentationsebene aufgerufen werden.

Die Beispiele für die zwischen Speicherungs Schicht, die wir in diesemund in den vorherigen Tutorials untersucht haben, haben Mit reaktivem laden werden die Daten nur dann in den Cache geladen, wenn eine Anforderung für die Daten erfolgt und die Daten im Cache fehlen. Daten können auch proaktiv in den Cache *geladen* werden, ein Verfahren, das die Daten in den Cache lädt, bevor Sie tatsächlich benötigt werden. Im nächsten Tutorial wird ein Beispiel für das proaktive laden angezeigt, wenn wir uns ansehen, wie statische Werte beim Anwendungsstart im Cache gespeichert werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](caching-data-with-the-objectdatasource-vb.md)
> [Weiter](caching-data-at-application-startup-vb.md)
