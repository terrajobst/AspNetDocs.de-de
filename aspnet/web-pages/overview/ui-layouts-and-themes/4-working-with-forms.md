---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Arbeiten mit HTML-Formularen auf ASP.net Web Pages (Razor)-Websites | Microsoft-Dokumentation
author: Rick-Anderson
description: Ein Formular ist ein Abschnitt eines HTML-Dokuments, in dem Sie Benutzereingabe-Steuerelemente wie Textfelder, Kontrollkästchen, Options Felder und pulldownlisten platzieren. Sie verwenden die Formulare WH...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: c7d4802063c8610a246afe67bd15eea429f7304a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519603"
---
# <a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Arbeiten mit HTML-Formularen auf ASP.net Web Pages-Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie Sie ein HTML-Formular (mit Textfeldern und Schaltflächen) verarbeiten, wenn Sie auf einer ASP.net Web Pages-Website (Razor) arbeiten.
> 
> **Lernen Sie Folgendes:** 
> 
> - So erstellen Sie ein HTML-Formular
> - Gewusst wie: Lesen von Benutzereingaben aus dem Formular
> - Validieren von Benutzereingaben
> - Vorgehensweise beim Wiederherstellen von Formular Werten, nachdem die Seite gesendet wurde.
> 
> Dies sind die ASP.net-Programmier Konzepte, die im Artikel vorgestellt werden:
> 
> - Das `Request`-Objekt.
> - Eingabevalidierung.
> - HTML-Codierung.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

## <a name="creating-a-simple-html-form"></a>Erstellen eines einfachen HTML-Formulars

1. Erstellen Sie eine neue Website.
2. Erstellen Sie im Stamm Ordner eine Webseite mit dem Namen " *Form. cshtml* ", und geben Sie das folgende Markup ein:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Starten Sie die Seite in Ihrem Browser. (Klicken Sie in webmatrix im Arbeitsbereich " **Dateien** " mit der rechten Maustaste auf die Datei, und wählen Sie dann **im Browser starten aus**.) Ein einfaches Formular mit drei Eingabefeldern und einer Schaltfläche " **senden** " wird angezeigt.

    ![Screenshot eines Formulars mit drei Textfeldern.](4-working-with-forms/_static/image1.png)

    Wenn Sie an dieser Stelle auf die Schaltfläche " **senden** " klicken, geschieht nichts. Um das Formular zu nutzen, müssen Sie Code hinzufügen, der auf dem Server ausgeführt wird.

## <a name="reading-user-input-from-the-form"></a>Lesen von Benutzereingaben aus dem Formular

Um das Formular zu verarbeiten, fügen Sie Code hinzu, der die gesendeten Feldwerte liest und etwas mit Ihnen führt. In diesem Verfahren wird gezeigt, wie die Felder gelesen und die Benutzereingaben auf der Seite angezeigt werden. (In einer Produktionsanwendung führen Sie im allgemeinen weitere interessante Dinge mit Benutzereingaben aus. Dies wird im Artikel zum Arbeiten mit Datenbanken beschrieben.)

1. Geben Sie am Anfang der Datei " *Form. cshtml* " den folgenden Code ein:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Wenn der Benutzer die Seite zuerst anfordert, wird nur das leere Formular angezeigt. Der Benutzer (der Sie sein wird) füllt das Formular aus und klickt dann auf **senden**. Dadurch wird die Benutzereingabe an den Server übermittelt (Posts). Standardmäßig wird die Anforderung an dieselbe Seite (d.h. " *Form. cshtml*") weitergeleitet.

    Wenn Sie die Seite dieses Mal einreichen, werden die eingegebenen Werte direkt über dem Formular angezeigt:

    ![Screenshot, der die von Ihnen eingegebenen Werte anzeigt, die auf der Seite angezeigt werden.](4-working-with-forms/_static/image2.png)

    Sehen Sie sich den Code für die Seite an. Zuerst verwenden Sie die `IsPost`-Methode, um zu bestimmen, ob die &#8212; Seite gesendet wird, d. h., ob ein Benutzer auf die Schaltfläche **senden** geklickt hat. Wenn dies ein Post ist, gibt `IsPost` "true" zurück. Dies ist die Standardmethode in ASP.net Web Pages, um zu bestimmen, ob Sie mit einer ersten Anforderung (einer GET-Anforderung) oder einem Postback (Post-Anforderung) arbeiten. (Weitere Informationen zu Get und Post finden Sie in der Rand Leiste "http Get und Post und die ispost-Eigenschaft" in [Einführung in ASP.net Web Pages-Programmierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Als nächstes erhalten Sie die Werte, die der Benutzer aus dem `Request.Form` Objekt ausgefüllt hat, und Sie fügen Sie später in Variablen ein. Das `Request.Form`-Objekt enthält alle Werte, die mit der Seite übermittelt wurden, die jeweils durch einen Schlüssel identifiziert werden. Der Schlüssel entspricht dem `name`-Attribut des Formular Felds, das Sie lesen möchten. Um z. b. das `companyname` Feld (Textfeld) zu lesen, verwenden Sie `Request.Form["companyname"]`.

    Formular Werte werden im `Request.Form` Objekt als Zeichen folgen gespeichert. Wenn Sie also mit einem Wert als eine Zahl oder ein Datum oder einen anderen Typ arbeiten müssen, müssen Sie ihn von einer Zeichenfolge in diesen Typ konvertieren. Im Beispiel wird die `AsInt`-Methode des `Request.Form` verwendet, um den Wert des Felds Employees (das eine Anzahl von Mitarbeitern enthält) in eine ganze Zahl zu konvertieren.
2. Starten Sie die Seite in Ihrem Browser, füllen Sie die Formularfelder aus, und klicken Sie auf **senden**. Auf der Seite werden die von Ihnen eingegebenen Werte angezeigt.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML-Codierung für Darstellung und Sicherheit
> 
> HTML hat spezielle Verwendungsmöglichkeiten für Zeichen wie `<`, `>`und `&`. Wenn diese Sonderzeichen angezeigt werden, wo Sie nicht erwartet werden, können Sie die Darstellung und Funktionalität Ihrer Webseite ruinieren. Der Browser interpretiert z. b. das `<` Zeichen (es sei denn, es folgt ein Leerzeichen) als Anfang eines HTML-Elements, wie z. b. `<b>` oder `<input ...>`. Wenn der Browser das Element nicht erkennt, verwirft es einfach die Zeichenfolge, die mit `<` beginnt, bis es etwas erreicht, das es erkennt. Dies kann natürlich zu einem merkwürdigen Rendering auf der Seite führen.
> 
> Die HTML-Codierung ersetzt diese reservierten Zeichen durch einen Code, der von Browsern als richtiges Symbol interpretiert wird. Beispielsweise wird das `<` Zeichen durch `&lt;` ersetzt, und das `>` Zeichen wird durch `&gt;`ersetzt. Der Browser rendert diese Ersetzungs Zeichenfolgen als die Zeichen, die Sie anzeigen möchten.
> 
> Es empfiehlt sich, die HTML-Codierung immer dann zu verwenden, wenn Sie Zeichen folgen (Eingabe) anzeigen, die Sie von einem Benutzer erhalten haben. Wenn Sie dies nicht tun, kann ein Benutzer versuchen, Ihre Webseite zum Ausführen eines bösartigen Skripts zu verwenden oder etwas anderes auszuführen, das Ihre Website Sicherheit beeinträchtigt, oder das ist nicht das, was Sie beabsichtigen. (Dies ist besonders wichtig, wenn Sie Benutzereingaben übernehmen, Sie an einem Ort speichern und später &#8212; anzeigen, z. b. als Blog Kommentar, Benutzer Überprüfung oder ähnliches.)
> 
> Um diese Probleme zu vermeiden, ASP.net Web Pages den Text Inhalt, den Sie aus Ihrem Code ausgeben, automatisch in HTML codiert. Wenn Sie z. b. den Inhalt einer Variablen oder eines Ausdrucks mithilfe von Code wie `@MyVar`anzeigen, codiert ASP.net Web Pages die Ausgabe automatisch.

## <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

Benutzer führen zu Fehlern. Sie bitten Sie, ein Feld auszufüllen, das Sie vergessen, oder Sie bitten Sie, die Anzahl der Mitarbeiter einzugeben und stattdessen einen Namen einzugeben. Um sicherzustellen, dass ein Formular vor der Verarbeitung ordnungsgemäß ausgefüllt wurde, überprüfen Sie die Eingabe des Benutzers.

In diesem Verfahren wird gezeigt, wie alle drei Formularfelder überprüft werden, um sicherzustellen, dass der Benutzer Sie nicht leer gelassen hat. Außerdem überprüfen Sie, ob der Wert für die Mitarbeiter Anzahl eine Zahl ist. Wenn Fehler auftreten, wird eine Fehlermeldung angezeigt, die den Benutzer darüber informiert, welche Werte die Überprüfung nicht bestanden haben.

1. Ersetzen Sie in der Datei " *Form. cshtml* " den ersten Codeblock durch den folgenden Code: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Zum Überprüfen von Benutzereingaben verwenden Sie das `Validation`-Hilfsprogramm. Sie registrieren erforderliche Felder, indem Sie `Validation.RequireField`aufrufen. Sie registrieren andere Validierungs Typen, indem Sie `Validation.Add` aufrufen und das zu überprüfende Feld sowie den Typ der Überprüfung angeben.

    Wenn die Seite ausgeführt wird, führt ASP.net alle Überprüfungen durch. Sie können die Ergebnisse überprüfen, indem Sie `Validation.IsValid`aufrufen, der true zurückgibt, wenn alles erfolgreich war, und false, wenn ein Feld nicht überprüft werden konnte In der Regel werden Sie `Validation.IsValid` aufgerufen, bevor Sie die Benutzereingaben verarbeiten.
2. Aktualisieren Sie das `<body>`-Element, indem Sie drei Aufrufe der `Html.ValidationMessage`-Methode wie folgt hinzufügen:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Wenn Sie Validierungs Fehlermeldungen anzeigen möchten, können Sie HTML.`ValidationMessage` aufrufen. und übergeben Sie den Namen des Felds, für das die Nachricht angezeigt werden soll.
3. Führen Sie die Seite aus. Lassen Sie die Felder leer, und klicken Sie auf **senden**. Es werden Fehlermeldungen angezeigt.

    ![Screenshot, der zeigt, dass Fehlermeldungen angezeigt werden, wenn die Überprüfung nicht erfolgreich ist.](4-working-with-forms/_static/image3.jpg)
4. Fügen Sie dem Feld **Employee count** eine Zeichenfolge (z. b. "ABC") hinzu, und klicken Sie erneut auf **senden** . Dieses Mal wird ein Fehler angezeigt, der darauf hinweist, dass die Zeichenfolge nicht das richtige Format hat, nämlich eine ganze Zahl.

    ![Screenshot mit Fehlermeldungen, die angezeigt werden, wenn Benutzer eine Zeichenfolge für das Feld "Employees" eingeben.](4-working-with-forms/_static/image4.jpg)

ASP.net Web Pages bietet mehr Optionen zum Überprüfen von Benutzereingaben, einschließlich der Möglichkeit, die Überprüfung mithilfe von Client Skripts automatisch auszuführen, damit Benutzer im Browser sofortiges Feedback erhalten. Weitere Informationen finden Sie weiter unten im Abschnitt " [Weitere Ressourcen](#Additional_Resources) ".

## <a name="restoring-form-values-after-postbacks"></a>Wiederherstellen von Formular Werten nach Postbacks

Wenn Sie die Seite im vorherigen Abschnitt getestet haben, haben Sie möglicherweise bemerkt, dass bei einem Überprüfungs Fehler alles, was Sie eingegeben haben (nicht nur die ungültigen Daten), abgelaufen ist, und Sie mussten die Werte für alle Felder erneut eingeben. Dies veranschaulicht einen wichtigen Punkt: Wenn Sie eine Seite übermitteln, Sie verarbeiten und die Seite dann erneut erstellen, wird die Seite neu erstellt. Wie Sie gesehen haben, bedeutet dies, dass alle Werte, die auf der Seite waren, als Sie übermittelt wurden, verloren gehen.

Dies kann jedoch problemlos behoben werden. Sie haben Zugriff auf die gesendeten Werte (im `Request.Form` Objekt, damit Sie diese Werte wieder in die Formularfelder eingeben können, wenn die Seite gerendert wird.

1. Ersetzen Sie in der Datei " *Form. cshtml* " die `value` Attribute der `<input>` Elemente mithilfe des `value`-Attributs. 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Das `value`-Attribut der `<input>` Elemente wurde so festgelegt, dass der Feldwert dynamisch aus dem `Request.Form` Objekt gelesen wird. Wenn die Seite das erste Mal angefordert wird, sind die Werte im `Request.Form`-Objekt leer. Das ist in Ordnung, da das Formular leer ist.
2. Starten Sie die Seite in Ihrem Browser, füllen Sie die Formularfelder aus, oder lassen Sie sie leer, und klicken Sie auf **senden**. Eine Seite, auf der die übermittelten Werte angezeigt werden, wird angezeigt.

    ![Formulare-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [1.001 Möglichkeiten zum erhalten von Eingaben von Webbenutzern](https://msdn.microsoft.com/library/ms971057.aspx)
- [Verwenden von Formularen und Verarbeiten von Benutzereingaben](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Überprüfen der Benutzereingabe in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Verwenden von AutoComplete in HTML-Formularen](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
