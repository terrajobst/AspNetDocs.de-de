---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Verbessern der Leistung mit AusgabeC#Zwischenspeicherung () | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie die Leistung Ihrer ASP.NET-MVC-Webanwendungen drastisch verbessern können, indem Sie die Ausgabe Zwischenspeicherung nutzen. ...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 548c5bea2e9cf26e0574e72d2c0ea204dbd90f9c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486669"
---
# <a name="improving-performance-with-output-caching-c"></a>Verbessern der Leistung mit Ausgabezwischenspeicherung (C#)

von [Microsoft](https://github.com/microsoft)

> In diesem Tutorial erfahren Sie, wie Sie die Leistung Ihrer ASP.NET-MVC-Webanwendungen drastisch verbessern können, indem Sie die Ausgabe Zwischenspeicherung nutzen. Sie erfahren, wie Sie das von einer Controller Aktion zurückgegebene Ergebnis Zwischenspeichern, sodass derselbe Inhalt nicht jedes Mal erstellt werden muss, wenn ein neuer Benutzer die Aktion aufruft.

In diesem Tutorial wird erläutert, wie Sie die Leistung einer ASP.NET MVC-Anwendung erheblich verbessern können, indem Sie den Ausgabe Cache nutzen. Mit dem Ausgabe Cache können Sie den von einer Controller Aktion zurückgegebenen Inhalt zwischenspeichern. Auf diese Weise muss derselbe Inhalt nicht jedes Mal generiert werden, wenn dieselbe Controller Aktion aufgerufen wird.

Stellen Sie sich beispielsweise vor, dass Ihre ASP.NET MVC-Anwendung eine Liste von Datenbankdaten Sätzen in einer Sicht namens "index" anzeigt. Jedes Mal, wenn ein Benutzer die Controller Aktion aufruft, die die Index Ansicht zurückgibt, muss der Satz von Datenbankdaten Sätzen aus der Datenbank abgerufen werden, indem eine Datenbankabfrage ausgeführt wird.

Wenn Sie auf der anderen Seite den Ausgabe Cache nutzen, können Sie die Ausführung einer Datenbankabfrage vermeiden, wenn ein Benutzer dieselbe Controller Aktion aufruft. Die Sicht kann aus dem Cache abgerufen werden, anstatt erneut von der Controller Aktion generiert zu werden. Mithilfe von Caching können Sie keine redundante Arbeit auf dem Server ausführen.

## <a name="enabling-output-caching"></a>Aktivieren der Ausgabe Zwischenspeicherung

Sie aktivieren das Zwischenspeichern von Ausgaben, indem Sie ein [OutputCache]-Attribut entweder einer einzelnen Controller Aktion oder einer gesamten Controller Klasse hinzufügen. Der Controller in der Liste 1 macht beispielsweise eine Aktion mit dem Namen Index () verfügbar. Die Ausgabe der Index ()-Aktion wird 10 Sekunden lang zwischengespeichert.

**Codebeispiel 1 – controllers\homecontroller.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

In den Beta Versionen von ASP.NET MVC funktioniert die Ausgabe Zwischenspeicherung für eine URL wie [http://www.MySite.com/](http://www.mysite.com/)nicht. Stattdessen müssen Sie eine URL wie [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index)eingeben. 

In der Liste 1 wird die Ausgabe der Index ()-Aktion 10 Sekunden lang zwischengespeichert. Wenn Sie möchten, können Sie eine wesentlich längere Cache Dauer angeben. Wenn Sie z. b. die Ausgabe einer Controller Aktion für einen Tag zwischenspeichern möchten, können Sie eine Cache Dauer von 86400 Sekunden angeben (60 Sekunden \* 60 Minuten \* 24 Stunden).

Es gibt keine Garantie, dass der Inhalt für die von Ihnen angegebene Zeitspanne zwischengespeichert wird. Wenn die Arbeitsspeicher Ressourcen gering sind, startet der Cache die automatische Überprüfung von Inhalt.

Der Home-Controller in der Liste 1 gibt die Index Ansicht in der Liste 2 zurück. Es gibt keine besonderen Informationen zu dieser Ansicht. Die Index Sicht zeigt einfach die aktuelle Uhrzeit an (siehe Abbildung 1).

**Codebeispiel 2 – views\home\index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Abbildung 1 – zwischengespeicherte Index Sicht**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Wenn Sie die Index ()-Aktion mehrmals aufrufen, indem Sie die URL/Home/Index in der Adressleiste des Browsers eingeben und die Schaltfläche Aktualisieren/erneut laden in Ihrem Browser wiederholt drücken, wird die von der Index Ansicht angezeigte Zeit 10 Sekunden lang nicht geändert. Die gleiche Zeit wird angezeigt, da die Ansicht zwischengespeichert wird.

Es ist wichtig zu verstehen, dass für alle Benutzer, die Ihre Anwendung besuchen, dieselbe Ansicht zwischengespeichert wird. Jeder Benutzer, der die Index ()-Aktion aufruft, erhält dieselbe zwischengespeicherte Version der Index Sicht. Dies bedeutet, dass der Arbeitsaufwand, den der Webserver ausführen muss, um die Index Ansicht zu erfüllen, drastisch reduziert wird.

Die Ansicht in der Liste 2 bewirkt, dass etwas ganz einfach ist. Die Ansicht zeigt nur die aktuelle Zeit an. Allerdings können Sie eine Sicht, die eine Reihe von Datenbankdaten Sätzen anzeigt, genauso einfach Zwischenspeichern. In diesem Fall muss der Satz von Datenbankdaten Sätzen nicht jedes Mal aus der Datenbank abgerufen werden, und jedes Mal, wenn die Controller Aktion, die die Sicht zurückgibt, aufgerufen wird. Durch die Zwischenspeicherung kann der Arbeitsaufwand reduziert werden, der von Ihrem Webserver und Datenbankserver ausgeführt werden muss.

Verwenden Sie die Seite &lt;% @ OutputCache%&gt;-Direktive in einer MVC-Ansicht nicht. Diese Direktive wird von der Web Forms Welt überströmt und sollte nicht in einer ASP.NET MVC-Anwendung verwendet werden.

## <a name="where-content-is-cached"></a>Inhalt zwischengespeichert

Wenn Sie das Attribut [OutputCache] verwenden, wird der Inhalt standardmäßig an dreispeicher Orten zwischengespeichert: Webserver, beliebige Proxy Server und Webbrowser. Sie können genau steuern, wo der Inhalt zwischengespeichert wird, indem Sie die Location-Eigenschaft des [OutputCache]-Attributs ändern.

Sie können die Location-Eigenschaft auf einen der folgenden Werte festlegen:

> · Irgendeiner
> 
> · Ent
> 
> · Tete
> 
> · Servers
> 
> · Gar
> 
> · ServerAndClient

Standardmäßig hat die Location-Eigenschaft den Wert any. Es gibt jedoch Situationen, in denen Sie möglicherweise nur im Browser oder nur auf dem Server zwischenspeichern möchten. Wenn Sie z. b. Informationen zwischenspeichern, die für jeden Benutzer personalisiert werden, sollten Sie die Informationen auf dem Server nicht zwischenspeichern. Wenn Sie für verschiedene Benutzer unterschiedliche Informationen anzeigen, sollten Sie die Informationen nur auf dem Client zwischenspeichern.

Beispielsweise macht der Controller in der Liste 3 eine Aktion mit dem Namen GetName () verfügbar, die den aktuellen Benutzernamen zurückgibt. Wenn sich Jack bei der Website anmeldet und die GetName ()-Aktion aufruft, gibt die Aktion die Zeichenfolge "Hi Jack" zurück. Wenn sich dann Jill bei der Website anmeldet und die GetName ()-Aktion aufruft, erhält Sie auch die Zeichenfolge "Hi Jack". Die Zeichenfolge wird auf dem Webserver für alle Benutzer zwischengespeichert, nachdem Jack die Controller Aktion anfänglich aufgerufen hat.

**Codebeispiel 3 – controllers\badusercontroller.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Höchstwahrscheinlich funktioniert der Controller in der Liste 3 nicht wie gewünscht. Sie möchten die Meldung "Hi Jack" nicht an Jill anzeigen.

Sie sollten niemals personalisierte Inhalte im Server Cache Zwischenspeichern. Möglicherweise möchten Sie jedoch die personalisierten Inhalte im Browser Cache Zwischenspeichern, um die Leistung zu verbessern. Wenn Sie Inhalte im Browser Zwischenspeichern und ein Benutzer mehrmals dieselbe Controller Aktion aufruft, kann der Inhalt nicht auf dem Server, sondern aus dem Browser Cache abgerufen werden.

Der geänderte Controller in der Liste 4 speichert die Ausgabe der GetName ()-Aktion zwischen. Der Inhalt wird jedoch nur im Browser und nicht auf dem Server zwischengespeichert. Wenn mehrere Benutzer die Methode GetName () aufrufen, erhält jede Person ihren eigenen Benutzernamen und keinen Benutzernamen einer anderen Person.

**Codebeispiel 4 – controllers\usercontroller.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Beachten Sie, dass das [OutputCache]-Attribut in der Liste 4 eine Location-Eigenschaft enthält, die auf den Wert outputcacheloation. Client festgelegt ist. Das [OutputCache]-Attribut enthält auch eine NoStore-Eigenschaft. Die NoStore-Eigenschaft wird verwendet, um Proxy Server und Browser darüber zu informieren, dass Sie keine permanente Kopie des zwischengespeicherten Inhalts speichern sollten.

## <a name="varying-the-output-cache"></a>Variieren des Ausgabe Caches

In einigen Fällen möchten Sie möglicherweise verschiedene zwischengespeicherte Versionen des gleichen Inhalts. Stellen Sie sich beispielsweise vor, dass Sie eine Master-/Detailseite erstellen. Die Master Seite zeigt eine Liste mit Filmtiteln an. Wenn Sie auf einen Titel klicken, erhalten Sie Details für den ausgewählten Film.

Wenn Sie die Detailseite Zwischenspeichern, werden die Details für denselben Film unabhängig von dem Film angezeigt, auf den Sie klicken. Der erste Film, der vom ersten Benutzer ausgewählt wird, wird allen zukünftigen Benutzern angezeigt.

Sie können dieses Problem beheben, indem Sie die VaryByParam-Eigenschaft des [OutputCache]-Attributs nutzen. Diese Eigenschaft ermöglicht es Ihnen, verschiedene zwischengespeicherte Versionen desselben Inhalts zu erstellen, wenn ein Formular Parameter oder ein Parameter für die Abfrage Zeichenfolge variiert.

Der Controller in der Liste 5 stellt z. b. zwei Aktionen namens Master () und Details () zur Verfügung. Die Master ()-Aktion gibt eine Liste von Filmtiteln zurück, und die Details ()-Aktion gibt die Details für den ausgewählten Film zurück.

**Codebeispiel 5 – controllers\moviescontroller.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Die Master ()-Aktion enthält eine VaryByParam-Eigenschaft mit dem Wert "None". Wenn die Master ()-Aktion aufgerufen wird, wird dieselbe zwischengespeicherte Version der Master Ansicht zurückgegeben. Alle Formular Parameter oder Abfrage Zeichenfolgen-Parameter werden ignoriert (siehe Abbildung 2).

**Abbildung 2 – die/Movies/Master-Sicht**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Abbildung 3 – die/Movies/Details-Sicht**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Die Aktion Details () enthält eine VaryByParam-Eigenschaft mit dem Wert "ID". Wenn verschiedene Werte des ID-Parameters an die Controller Aktion übergeben werden, werden unterschiedliche zwischengespeicherte Versionen der Detailansicht generiert.

Es ist wichtig zu verstehen, dass die Verwendung der VaryByParam-Eigenschaft zu einer größeren Zwischenspeicherung und nicht zu einer geringeren Anzahl führt. Eine andere zwischengespeicherte Version der Detailansicht wird für jede andere Version des ID-Parameters erstellt.

Sie können die VaryByParam-Eigenschaft auf die folgenden Werte festlegen:

> \* = eine andere zwischengespeicherte Version erstellen, wenn ein Formular-oder Abfrage Zeichenfolgen-Parameter variiert.
> 
> None = nie unterschiedliche zwischengespeicherte Versionen erstellen
> 
> Semikolons Liste von Parametern = unterschiedliche zwischengespeicherte Versionen erstellen, wenn eines der Formulare oder Abfrage Zeichenfolgen-Parameter in der Liste variiert

## <a name="creating-a-cache-profile"></a>Erstellen eines Cache Profils

Als Alternative zum Konfigurieren von Ausgabe Cache Eigenschaften durch Ändern der Eigenschaften des [OutputCache]-Attributs können Sie ein Cache Profil in der Webkonfigurationsdatei (Web. config) erstellen. Das Erstellen eines Cache Profils in der Webkonfigurationsdatei bietet einige wichtige Vorteile.

Zunächst können Sie durch Konfigurieren der Ausgabe Zwischenspeicherung in der Webkonfigurationsdatei steuern, wie Controller Aktionen Inhalte an einem zentralen Ort Zwischenspeichern. Sie können ein Cache Profil erstellen und es auf mehrere Controller oder Controller Aktionen anwenden.

Zweitens können Sie die Webkonfigurationsdatei ändern, ohne die Anwendung neu kompilieren zu müssen. Wenn Sie das Zwischenspeichern für eine Anwendung deaktivieren müssen, die bereits in der Produktionsumgebung bereitgestellt wurde, können Sie einfach die in der Webkonfigurationsdatei definierten Cache Profile ändern. Alle Änderungen an der Webkonfigurationsdatei werden automatisch erkannt und angewendet.

Beispielsweise wird im Abschnitt &lt;Caching&gt; Webkonfiguration in der Liste 6 ein Cache Profil mit dem Namen Cache1Hour definiert. Der Abschnitt &lt;Caching&gt; muss innerhalb des Abschnitts &lt;System. Web&gt; einer Webkonfigurationsdatei angezeigt werden.

**Codebeispiel 6 – Abschnitt zum Zwischenspeichern für "Web. config"**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Der Controller in der Liste 7 veranschaulicht, wie Sie das Cache1Hour-Profil auf eine Controller Aktion mit dem [OutputCache]-Attribut anwenden können.

**Codebeispiel 7 – controllers\profilecontroller.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Wenn Sie die vom Controller verfügbar gemachte Index ()-Aktion in der Liste 7 aufrufen, wird für eine Stunde dieselbe Zeit zurückgegeben.

## <a name="summary"></a>Zusammenfassung

Die Ausgabe Zwischenspeicherung bietet eine sehr einfache Methode, um die Leistung Ihrer ASP.NET-MVC-Anwendungen drastisch zu verbessern. In diesem Tutorial haben Sie gelernt, wie Sie das [OutputCache]-Attribut verwenden, um die Ausgabe von Controller Aktionen zwischenzuspeichern. Außerdem haben Sie erfahren, wie Sie die Eigenschaften des [OutputCache]-Attributs ändern, wie z. b. die Eigenschaften Duration und VaryByParam, um die Zwischenspeicherung von Inhalt zu ändern. Schließlich haben Sie erfahren, wie Sie Cache Profile in der Webkonfigurationsdatei definieren.

> [!div class="step-by-step"]
> [Zurück](understanding-action-filters-cs.md)
> [Weiter](adding-dynamic-content-to-a-cached-page-cs.md)
