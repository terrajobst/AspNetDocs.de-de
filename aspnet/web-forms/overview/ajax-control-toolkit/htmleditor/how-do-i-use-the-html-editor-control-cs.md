---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Gewusst wie das HTML-Editor-Steuerelement verwenden? (C#) | Microsoft-Dokumentation
author: microsoft
description: HTMLEditor ist ein ASP.NET AJAX-Steuerelement, mit dem Sie HTML-Inhalt problemlos über Schaltflächen in einer Symbolleiste erstellen und bearbeiten können.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cb7d75b59b1361abeb6d3c38ad6e42e34d6e3f7b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78466605"
---
# <a name="how-do-i-use-the-html-editor-control-c"></a>Gewusst wie das HTML-Editor-Steuerelement verwenden? (C#)

von [Microsoft](https://github.com/microsoft)

> HTMLEditor ist ein ASP.NET AJAX-Steuerelement, mit dem Sie HTML-Inhalt problemlos über Schaltflächen in einer Symbolleiste erstellen und bearbeiten können.

Ziel dieses Tutorials ist es, Ihnen einen Überblick über das HTML-Editor-Steuerelement zu geben, das im AJAX Control Toolkit enthalten ist. Der HTML-Editor enthält Optionen zum Ändern des Schrift Grads, Auswählen einer Schriftart, Ändern der Hintergrundfarbe, Ändern der Vordergrundfarbe, Hinzufügen von Links, Hinzufügen von Bildern, Ändern der Textausrichtung und Ausführen von Ausschneiden, kopieren und Einfügen (siehe Abbildung 1).

[![des HTML-Editors](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Abbildung 01**: der HTML-Editor ([Klicken Sie, um das Bild in voller Größe anzuzeigen](how-do-i-use-the-html-editor-control-cs/_static/image2.png))

Der HTML-Editor ermöglicht es Ihnen, Inhalte mithilfe eines Entwurfs Modus einzugeben, oder Sie können HTML direkt eingeben. Sie erhalten auch die Möglichkeit, eine Vorschau Ihres HTML-Inhalts anzuzeigen (siehe Abbildung 2).

[![Entwurfs-, HTML-und Vorschau Schaltflächen](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Abbildung 02**: Entwurfs-, HTML-und Vorschau Schaltflächen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](how-do-i-use-the-html-editor-control-cs/_static/image4.png))

In diesem Tutorial erfahren Sie, wie Sie den HTML-Editor anzeigen, wie Sie die Symbolleisten Schaltflächen anpassen können, die im HTML-Editor angezeigt werden, und wie Sie Site übergreifende Skript Angriffe vermeiden.

## <a name="displaying-the-html-editor"></a>Anzeigen des HTML-Editors

Bevor Sie den HTML-Editor auf einer ASP.NET-Seite verwenden können, müssen Sie der Seite zunächst ein ScriptManager-Steuerelement hinzufügen. Das ScriptManager-Steuerelement befindet sich unterhalb der Registerkarte AJAX-Erweiterungen in der Visual Studio-/Visual Web Developer Express-Toolbox.

Sie sollten das ScriptManager-Steuerelement oben auf der Seite vor allen anderen Steuerelementen auf der Seite platzieren. Sie können Sie z. b. direkt unterhalb des öffnenden serverseitigen &lt;Formulars&gt; Tag platzieren.

Das HTML-Editor-Steuerelement befindet sich in der Toolbox mit dem Rest der AJAX Control Toolkit-Steuerelemente. Sie wird als Editor-Steuerelement bezeichnet (siehe Abbildung 3).

[![des HTML-Editor-Steuer Elements](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Abbildung 03**: das HTML-Editor-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](how-do-i-use-the-html-editor-control-cs/_static/image6.png))

Nachdem Sie den HTML-Editor auf eine Seite gezogen haben, können Sie seine Eigenschaften im Eigenschaften Blatt festlegen. Sie möchten z. b. normalerweise die Eigenschaften Width und Height festlegen. In der Liste 1 ist die Quelle für eine ASP.NET Seite enthalten, die einen HTML-Editor enthält.

**Codebeispiel 1: simpleeditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

Die Seite in der Liste 1 enthält ein HTML-Editor-Steuerelement, ein Schaltflächen-Steuerelement und ein Literalsteuerelement. Wenn Sie auf die Schaltfläche klicken, wird der Inhalt des HTML-Editors im Literalsteuerelement angezeigt (siehe Abbildung 4).

[![Senden eines Formulars mit einem HTML-Editor](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Abbildung 04**: Senden eines Formulars mit einem HTML-Editor ([Klicken Sie, um das Bild in voller Größe anzuzeigen](how-do-i-use-the-html-editor-control-cs/_static/image8.png))

Die Inhalts Eigenschaft HTML-Editor wird verwendet, um den HTML-Inhalt abzurufen, der in den HTML-Editor eingegeben wird. Beachten Sie, dass dieser HTML-Inhalt JavaScript enthalten kann. Im nächsten Abschnitt wird erläutert, wie Sie JavaScript Injection-Angriffe verhindern können.

## <a name="customizing-the-html-editor-toolbar"></a>Anpassen der Symbolleiste des HTML-Editors

Sie können genau anpassen, welche Schaltflächen im Editor angezeigt werden. Beispielsweise können Sie die Registerkarte "HTML" entfernen, um zu verhindern, dass Benutzer den HTML-Editor in den HTML-Modus wechseln. Oder Sie möchten die Dropdown Liste "Schriftart Größe" entfernen, um zu verhindern, dass Benutzer übermäßig großen Text in einem Forums Nachrichtenbeitrag erstellen (siehe Abbildung 5).

[![eines angepassten HTML-Editors](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Abbildung 05**: ein angepasster HTML-Editor ([Klicken Sie, um das Bild in voller Größe anzuzeigen](how-do-i-use-the-html-editor-control-cs/_static/image10.png))

Sie passen die Symbolleisten Schaltflächen durch Ableiten eines neuen HTML-Editors von der Basis-Editor-Klasse an. Der benutzerdefinierte Editor in der Liste 2 enthält z. b. nur die Symbolleisten Schaltflächen Fett und kursiv formatiert. Alle anderen Symbolleisten Schaltflächen wurden entfernt. Außerdem wurde die Registerkarte HTML aus dem unteren Bereich des Editors entfernt (die Registerkarten Entwurf und Vorschau sind jedoch noch vorhanden).

**Codebeispiel 2: App\_code\customeditor .cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Sie müssen die Klasse in der Liste 2 der APP\_Code Ordner hinzufügen, damit die Klasse automatisch kompiliert wird. Wenn die APP\_-Code Ordner nicht auf der Website vorhanden ist, können Sie einfach den Ordner hinzufügen.

Nachdem Sie einen benutzerdefinierten Editor erstellt haben, können Sie ihn einer ASP.NET-Seite auf die gleiche Weise hinzufügen, wie Sie den normalen HTML-Editor hinzufügen (siehe Codebeispiel 3).

**Codebeispiel 3: showcustomeditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Vermeiden von Cross-Site Scripting (XSS)-Angriffen

Wenn Sie Eingaben von einem Benutzer akzeptieren und diese Eingabe auf Ihrer Website erneut anzeigen, können Sie Ihre Website möglicherweise für Cross-Site Scripting (XSS)-Angriffe öffnen. Theoretisch könnte ein böswilliger Hacker JavaScript-Code übermitteln, der ausgeführt wird, wenn die Eingabe erneut angezeigt wird. Das JavaScript könnte verwendet werden, um Benutzer Kennwörter oder andere vertrauliche Informationen zu stehlen.

Normalerweise können Sie XSS-Angriffe durch HTML-Codierung von allen Eingaben, die Sie von einem Benutzer abrufen, übertragen, bevor Sie ihn auf einer Webseite anzeigen. Die HTML-Codierung der Ausgabe des HTML-Editors würde jedoch nicht nur &lt;Skript&gt; Tags codieren, sondern auch alle HTML-Tags codieren. Dies bedeutet, dass Sie die gesamte Formatierung verlieren würden, z. b. Schriftart, Schrift Grad und Hintergrundfarbe.

Wenn Sie vertrauliche Informationen von Benutzern erfassen, z. b. Kenn Wörter, Kreditkartennummern und Sozialversicherungsnummern, sollten Sie nicht codierten Inhalt nicht anzeigen, den Sie von einem Benutzer mit dem HTML-Editor abrufen. Der HTML-Editor sollte nur in Situationen verwendet werden, in denen der HTML-Inhalt nicht erneut angezeigt wird, oder der HTML-Inhalt wird von einer vertrauenswürdigen Partei an Ihre Website übermittelt.

Stellen Sie sich beispielsweise vor, dass Sie eine Blog Anwendung erstellen. In dieser Situation ist es sinnvoll, bei der Erstellung von Blogbeiträgen den HTML-Editor zu verwenden. Sie sind der einzige, der einen Blogbeitrag übermittelt, und Sie können sich vermutlich Selbstvertrauen, dass Sie böswillige JavaScript nicht übermitteln. Es ist jedoch nicht sinnvoll, den HTML-Editor zu verwenden, wenn anonyme Benutzerkommentare veröffentlichen können. In Situationen, in denen Benutzer vertrauliche Informationen wie Kenn Wörter übermitteln, sollten Sie besonders vorsichtig sein. Möglicherweise könnte ein böswilliger Benutzer einen Kommentar Posten, der das richtige JavaScript zum Stehlen eines Kennworts enthält.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie eine kurze Übersicht über das HTML-Editor-Steuerelement bereitgestellt, das im AJAX Control Toolkit enthalten ist. Sie haben gelernt, wie Sie den HTML-Editor verwenden, um umfangreiche Inhalte eines Benutzers zu akzeptieren und den Inhalt an den Server zu übermitteln. Außerdem wurde erläutert, wie Sie die Symbolleisten Schaltflächen anpassen können, die im HTML-Editor angezeigt werden. Schließlich haben Sie erfahren, wie Sie Cross-Site Scripting-Angriffe vermeiden, wenn Sie den HTML-Editor verwenden, um potenziell schädliche Eingaben zu akzeptieren.

> [!div class="step-by-step"]
> [Weiter](how-do-i-use-the-html-editor-control-vb.md)
