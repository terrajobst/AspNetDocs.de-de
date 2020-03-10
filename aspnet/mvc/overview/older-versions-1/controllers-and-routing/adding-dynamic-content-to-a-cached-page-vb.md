---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Hinzufügen dynamischer Inhalte zu einer zwischengespeicherten Seite (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie dynamischen und zwischengespeicherten Inhalt auf derselben Seite kombinieren. Durch die Ersetzung nach dem Cache können Sie dynamischen Inhalt anzeigen, z. b. Banner Ankündigungen o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f2f4372498e5a38bbfcb96d6e9f6338b0ef4df1f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486963"
---
# <a name="adding-dynamic-content-to-a-cached-page-vb"></a>Hinzufügen von dynamischen Inhalten zu einer zwischengespeicherten Seite (VB)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie dynamischen und zwischengespeicherten Inhalt auf derselben Seite kombinieren. Durch die Ersetzung nach dem Cache können Sie dynamischen Inhalt, z. b. Banner Ankündigungen oder Nachrichten Elemente, auf einer Seite anzeigen, die im Cache ausgegeben wurde.

Durch die Nutzung der Ausgabe Zwischenspeicherung können Sie die Leistung einer ASP.NET-MVC-Anwendung drastisch verbessern. Anstatt eine Seite jedes Mal neu zu generieren, wenn die Seite angefordert wird, kann die Seite einmal generiert und für mehrere Benutzer im Arbeitsspeicher zwischengespeichert werden.

Es ist jedoch ein Problem aufgetreten. Was geschieht, wenn Sie dynamischen Inhalt auf der Seite anzeigen müssen? Stellen Sie sich beispielsweise vor, dass Sie eine Banner Ankündigung auf der Seite anzeigen möchten. Sie möchten nicht, dass die Banner Ankündigung zwischengespeichert wird, sodass jeder Benutzer dieselbe Ankündigung erhält. Auf diese Weise würden Sie nichts Geld verdienen!

Glücklicherweise gibt es eine einfache Lösung. Sie können eine Funktion des ASP.NET-Frameworks, die *nach dem Cache ersetzt*wird, nutzen. Durch die Ersetzung nach dem Cache können Sie dynamischen Inhalt auf einer Seite ersetzen, die im Arbeitsspeicher zwischengespeichert wurde.

Wenn Sie im Cache eine Seite mithilfe des &lt;OutputCache-&gt; Attributs ausgeben, wird die Seite normalerweise sowohl auf dem Server als auch auf dem Client (dem Webbrowser) zwischengespeichert. Wenn Sie die Ersetzung nach dem Cache verwenden, wird eine Seite nur auf dem Server zwischengespeichert.

#### <a name="using-post-cache-substitution"></a>Verwenden der nach gespeicherten Cache Ersetzung

Die Verwendung der nach dem Cache ersetzenden Ersetzung erfordert zwei Schritte. Zunächst müssen Sie eine Methode definieren, die eine Zeichenfolge zurückgibt, die den dynamischen Inhalt darstellt, der auf der zwischengespeicherten Seite angezeigt werden soll. Als Nächstes rufen Sie die HttpResponse. Write tesubstitution ()-Methode auf, um den dynamischen Inhalt in die Seite einzufügen.

Stellen Sie sich beispielsweise vor, dass Sie nach dem Zufallsprinzip verschiedene Nachrichten Elemente auf einer zwischengespeicherten Seite anzeigen möchten. Die-Klasse in Auflistung 1 macht eine einzelne Methode mit dem Namen rendernews () verfügbar, die nach dem Zufallsprinzip ein Nachrichten Element aus einer Liste mit drei Nachrichten Elementen zurückgibt.

**Codebeispiel 1 – models\news.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Um die Ersetzung nach dem Cache zu nutzen, rufen Sie die HttpResponse. Write tesubstitution ()-Methode auf. Die Methode "Write tesubstitution ()" richtet den Code ein, um einen Bereich der zwischengespeicherten Seite durch dynamischen Inhalt zu ersetzen. Die "Write tesubstitution ()"-Methode wird verwendet, um das zufällige Nachrichten Element in der Ansicht in der Liste 2 anzuzeigen.

**Codebeispiel 2 – views\home\index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

Die rendernews-Methode wird an die Methode "Write tesubstitution ()" übermittelt. Beachten Sie, dass die rendernews-Methode nicht aufgerufen wird. Stattdessen wird ein Verweis auf die-Methode mithilfe des AddressOf-Operators an "Write tesubstitution ()" weitergegeben.

Die Index Sicht wird zwischengespeichert. Die Ansicht wird vom Controller in der Liste 3 zurückgegeben. Beachten Sie, dass die Index ()-Aktion mit einem &lt;OutputCache-&gt; Attribut versehen ist, das bewirkt, dass die Index Sicht 60 Sekunden lang zwischengespeichert wird.

**Codebeispiel 3 – controllers\homecontroller.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Obwohl die Index Ansicht zwischengespeichert wird, werden unterschiedliche Zufalls Nachrichten Elemente angezeigt, wenn Sie die Indexseite anfordern. Wenn Sie die Index Seite anfordern, wird die von der Seite angezeigte Zeit für 60 Sekunden nicht geändert (siehe Abbildung 1). Die Tatsache, dass sich die Zeit nicht ändert, beweist, dass die Seite zwischengespeichert wird. Der Inhalt, der von der Methode "Write tesubstitution ()" eingefügt wird, – das zufällige Nachrichten Element – ändert sich jedoch mit jeder Anforderung.

**Abbildung 1 – einfügen dynamischer Nachrichten Elemente auf einer zwischengespeicherten Seite**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Verwenden der Ersetzung nach dem Cache in Hilfsmethoden

Eine einfachere Möglichkeit, die Ersetzung nach dem Cache zu nutzen, besteht darin, den Aufrufen der Methode "Write tesubstitution ()" in einer benutzerdefinierten Hilfsmethode zu kapseln. Diese Vorgehensweise wird von der Hilfsmethode in der Liste 4 veranschaulicht.

**Codebeispiel 4 – helpers\adhelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

Die Auflistung 4 enthält ein Visual Basic Modul, das zwei Methoden verfügbar macht: renderbanner () und renderbannerinternal (). Die renderbanner ()-Methode stellt die tatsächliche Hilfsmethode dar. Diese Methode erweitert die standardmäßige ASP.NET MVC htmlhelper-Klasse, sodass Sie HTML. renderbanner () in einer Ansicht wie jede andere Hilfsmethode aufruft.

Die renderbanner ()-Methode ruft die HttpResponse. Write tesubstitution ()-Methode auf und übergibt die renderbannerinternal ()-Methode an die Methode "Write tesubstitution ()".

Die renderbannerinternal ()-Methode ist eine private Methode. Diese Methode wird nicht als Hilfsmethode verfügbar gemacht. Die renderbannerinternal ()-Methode gibt nach dem Zufallsprinzip ein Banner Ankündigungs Bild aus einer Liste mit drei Banner Ankündigungs Bildern zurück.

Die geänderte Index Sicht in der Liste 5 veranschaulicht, wie Sie die Methode renderbanner () Helper verwenden können. Beachten Sie, dass eine zusätzliche &lt;% @ Import%&gt;-Direktive am Anfang der Sicht enthalten ist, um den MvcApplication1. Hilfsprogramme-Namespace zu importieren. Wenn Sie den Import dieses Namespace vernachlässigen, wird die Methode renderbanner () nicht als Methode für die HTML-Eigenschaft angezeigt.

**Codebeispiel 5 – views\home\index.aspx (mit renderbanner ()-Methode)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Wenn Sie die Seite anfordern, die von der Ansicht in der Liste 5 gerendert wird, wird für jede Anforderung eine andere Banner Ankündigung angezeigt (siehe Abbildung 2). Die Seite wird zwischengespeichert, aber die Banner Ankündigung wird dynamisch von der hilfsprogrammmethode renderbanner () eingefügt.

**Abbildung 2 – die Index Ansicht, die eine zufällige Banner Ankündigung anzeigt**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie Sie Inhalte dynamisch auf einer zwischengespeicherten Seite aktualisieren können. Sie haben gelernt, wie Sie die HttpResponse. Write tesubstitution ()-Methode verwenden, um dynamischen Inhalt auf einer zwischengespeicherten Seite zu aktivieren. Sie haben auch gelernt, wie Sie den-Befehl der Methode "Write tesubstitution ()" innerhalb einer HTML-Hilfsmethode Kapseln.

Nutzen Sie nach Möglichkeit die Zwischenspeicherung – Dies kann eine erhebliche Auswirkung auf die Leistung Ihrer Webanwendungen haben. Wie in diesem Tutorial erläutert, können Sie das Caching auch dann nutzen, wenn Sie dynamische Inhalte auf Ihren Seiten anzeigen müssen.

> [!div class="step-by-step"]
> [Zurück](improving-performance-with-output-caching-vb.md)
> [Weiter](creating-a-controller-vb.md)
