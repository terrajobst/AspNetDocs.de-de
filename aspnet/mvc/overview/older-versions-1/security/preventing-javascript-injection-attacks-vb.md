---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Verhindern von JavaScript Injection-Angriffen (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Verhindern Sie das Auftreten von JavaScript Injection-Angriffen und Site übergreifenden Skript Angriffen. In diesem Tutorial erläutert Stephen Walther, wie Sie ganz einfach
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: dfe09085f26c62c566649bc6f570aa25367a0f07
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594759"
---
# <a name="preventing-javascript-injection-attacks-vb"></a>Verhindern von Angriffen durch Einschleusung von JavaScript-Codes (VB)

von [Stephen Walther](https://github.com/StephenWalther)

[PDF herunterladen](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Verhindern Sie das Auftreten von JavaScript Injection-Angriffen und Site übergreifenden Skript Angriffen. In diesem Tutorial erläutert Stephen Walther, wie Sie diese Arten von Angriffen leicht durch HTML-Codierung Ihrer Inhalte besiegen können.

In diesem Tutorial wird erläutert, wie Sie JavaScript Injection-Angriffe in Ihren ASP.NET-MVC-Anwendungen verhindern können. In diesem Tutorial werden zwei Ansätze erläutert, um Ihre Website gegen einen JavaScript Injection-Angriff zu verteidigen. Sie erfahren, wie Sie JavaScript Injection-Angriffe vermeiden, indem Sie die Daten codieren, die Sie anzeigen. Außerdem erfahren Sie, wie Sie JavaScript Injection-Angriffe verhindern, indem Sie die von Ihnen akzeptierten Daten codieren.

## <a name="what-is-a-javascript-injection-attack"></a>Was ist ein JavaScript Injection-Angriff?

Wenn Sie Benutzereingaben akzeptieren und die Benutzereingaben erneut anzeigen, öffnen Sie Ihre Website für JavaScript Injection-Angriffe. Betrachten wir eine konkrete Anwendung, die für JavaScript Injection-Angriffe geöffnet ist.

Stellen Sie sich vor, Sie haben eine Kundenfeedback-Website erstellt (siehe Abbildung 1). Kunden können die Website besuchen und Feedback zur Nutzung ihrer Produkte geben. Wenn ein Kunde sein Feedback sendet, wird das Feedback erneut auf der Feedbackseite angezeigt.

[![Kunden Feedback-Website](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Abbildung 01**: Kunden Feedback-Website ([Klicken Sie, um das Bild in voller Größe anzuzeigen](preventing-javascript-injection-attacks-vb/_static/image3.png))

Die Kundenfeedback-Website verwendet die `controller` in der Liste 1. Diese `controller` enthält zwei Aktionen namens `Index()` und `Create()`.

**Codebeispiel 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

Die `Index()`-Methode zeigt die `Index` Ansicht an. Diese Methode übergibt das vorherige Kundenfeedback an die `Index` Ansicht, indem das Feedback aus der Datenbank abgerufen wird (mithilfe einer LINQ to SQL Abfrage).

Mit der `Create()`-Methode wird ein neues Feedback Element erstellt und der Datenbank hinzugefügt. Die Meldung, die der Kunde in das Formular eingibt, wird im Message-Parameter an die `Create()`-Methode übergeben. Es wird ein Feedback Element erstellt, und die Nachricht wird der `Message`-Eigenschaft des Feedback Elements zugewiesen. Das Feedback Element wird mit dem `DataContext.SubmitChanges()`-Methodenaufrufe an die Datenbank übermittelt. Schließlich wird der Besucher zurück zur `Index` Ansicht umgeleitet, in der das gesamte Feedback angezeigt wird.

Die `Index` Ansicht ist in der Liste 2 enthalten.

**Codebeispiel 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

Die `Index` Ansicht besteht aus zwei Abschnitten. Der obere Abschnitt enthält das tatsächliche Kundenfeedback Formular. Der untere Abschnitt enthält eine für. Jede Schleife durchläuft alle vorherigen Kundenfeedback Elemente und zeigt die Eigenschaften entrydate und Message für jedes Feedback Element an.

Die Kundenfeedback-Website ist eine einfache Website. Leider ist die Website für JavaScript Injection-Angriffe offen.

Stellen Sie sich vor, dass Sie den folgenden Text in das Kundenfeedback Formular eingeben:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Dieser Text stellt ein JavaScript-Skript dar, das ein Warn Meldungs Feld anzeigt. Nachdem jemand dieses Skript in das Feedback Formular übermittelt hat, wird die Nachricht <em>Boo!</em> wird immer dann angezeigt, wenn jemand in Zukunft auf die Kundenfeedback-Website zugreift (siehe Abbildung 2).

[![JavaScript Injection](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Abbildung 02**: JavaScript Injection ([Klicken Sie, um das Bild in voller Größe anzuzeigen](preventing-javascript-injection-attacks-vb/_static/image6.png))

Ihre erste Reaktion auf JavaScript Injection-Angriffe könnte eine Apathie sein. Vielleicht denken Sie daran, dass es sich bei JavaScript Injection-Angriffen einfach um einen Typ von *defakseangriffen* handelt Vielleicht glauben Sie, dass niemand etwas wirklich böse machen kann, indem er einen JavaScript-Injection-Angriff committet.

Leider kann ein Hacker etwas wirklich böse machen, indem er JavaScript in eine Website einfügt. Sie können einen JavaScript Injection-Angriff verwenden, um einen XSS-Angriff (Cross-Site Scripting) auszuführen. Bei einem Website übergreifenden Scripting-Angriff stehlen Sie vertrauliche Benutzerinformationen und senden die Informationen an eine andere Website.

Beispielsweise kann ein Hacker einen JavaScript Injection-Angriff verwenden, um die Werte von Browser Cookies von anderen Benutzern zu stehlen. Wenn vertrauliche Informationen wie Kenn Wörter, Kreditkartennummern oder Sozialversicherungsnummern – in den Browser Cookies gespeichert werden, kann ein Hacker einen JavaScript Injection-Angriff verwenden, um diese Informationen zu stehlen. Wenn ein Benutzer vertrauliche Informationen in ein Formularfeld eingibt, das in einer Seite enthalten ist, die mit einem JavaScript-Angriff kompromittiert wurde, kann der Hacker das eingefügte JavaScript verwenden, um die Formulardaten abzurufen und an eine andere Website zu senden.

*Seien Sie ängstlich*. Nehmen Sie JavaScript Injection-Angriffe Ernst, und schützen Sie vertrauliche Informationen Ihres Benutzers. In den nächsten beiden Abschnitten werden zwei Verfahren erläutert, die Sie verwenden können, um Ihre ASP.NET MVC-Anwendungen vor JavaScript Injection-Angriffen zu schützen.

## <a name="approach-1-html-encode-in-the-view"></a>Ansatz #1: HTML-Codierung in der Ansicht

Eine einfache Methode zum Verhindern von JavaScript Injection-Angriffen besteht darin, alle Daten, die von Website Benutzern eingegeben werden, in HTML zu codieren, wenn Sie die Daten in einer Ansicht erneut anzeigen. Die aktualisierte `Index` Ansicht in der Liste 3 folgt diesem Ansatz.

**Codebeispiel 3 – `Index.aspx` (HTML-codiert)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Beachten Sie, dass der Wert von `feedback.Message` HTML-codiert ist, bevor der Wert mit folgendem Code angezeigt wird:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Was bedeutet es, eine Zeichenfolge in HTML zu codieren? Wenn Sie eine Zeichenfolge in HTML codieren, werden gefährliche Zeichen wie `<` und `>` durch HTML-Entitäts Verweise ersetzt, z. b. `&lt;` und `&gt;`. Wenn die Zeichenfolge `<script>alert("Boo!")</script>` also HTML-codiert ist, wird Sie in `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`konvertiert. Die codierte Zeichenfolge wird nicht mehr als JavaScript-Skript ausgeführt, wenn Sie von einem Browser interpretiert wird. Stattdessen erhalten Sie die harmlose Seite in Abbildung 3.

[![besiegten JavaScript-Angriff](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Abbildung 03**: übersetzte JavaScript-Angriffe ([Klicken Sie, um das Bild in voller Größe anzuzeigen](preventing-javascript-injection-attacks-vb/_static/image9.png))

Beachten Sie, dass in der `Index` Ansicht in der Liste 3 nur der Wert `feedback.Message` codiert ist. Der Wert von `feedback.EntryDate` ist nicht codiert. Sie müssen nur die von einem Benutzer eingegebenen Daten codieren. Da der Wert von entrydate im Controller generiert wurde, müssen Sie diesen Wert nicht in HTML codieren.

## <a name="approach-2-html-encode-in-the-controller"></a>Ansatz #2: HTML-Codierung im Controller

Anstelle von HTML-Codierungs Daten, wenn Sie die Daten in einer Ansicht anzeigen, können Sie die Daten direkt vor dem Senden der Daten an die Datenbank in HTML codieren. Dieser zweite Ansatz wird im Fall der `controller` in der Liste 4 berücksichtigt.

**Codebeispiel 4 – `HomeController.cs` (HTML-codiert)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Beachten Sie, dass der Wert von Message HTML-codiert ist, bevor der Wert in der `Create()` Aktion an die Datenbank übermittelt wird. Wenn die Meldung erneut in der Ansicht angezeigt wird, ist die Nachricht HTML-codiert, und JavaScript-Code, der in die Nachricht eingefügt wird, wird nicht ausgeführt.

In der Regel sollten Sie den ersten Ansatz bevorzugen, der in diesem Tutorial in diesem zweiten Ansatz erläutert wird. Das Problem bei diesem zweiten Ansatz besteht darin, dass Sie in Ihrer Datenbank über HTML-codierte Daten verfügen. Mit anderen Worten, die Datenbankdaten werden mit lustigen Zeichen verknüpft.

Warum ist das nicht schlecht? Wenn Sie die Datenbankdaten auf einer anderen Seite als einer Webseite anzeigen müssen, treten Probleme auf. Beispielsweise können Sie die Daten nicht mehr problemlos in einer Windows Forms-Anwendung anzeigen.

## <a name="summary"></a>Summary

Der Zweck dieses Tutorials war, Sie vor der Aussicht eines JavaScript Injection-Angriffs zu erschrecken. In diesem Tutorial wurden zwei Ansätze zum verteidigen Ihrer ASP.NET MVC-Anwendungen gegen JavaScript Injection-Angriffe erläutert: Sie können entweder die von Benutzern gesendeten Daten in der Ansicht codieren oder die Benutzer übermittelten Daten im Controller codieren.

> [!div class="step-by-step"]
> [Vorheriges](authenticating-users-with-windows-authentication-vb.md)
