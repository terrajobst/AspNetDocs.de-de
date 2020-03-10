---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Einführung in das ASP.net Web Pages Löschen von Datenbankdaten | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie einen einzelnen Datenbankeintrag löschen. Es wird davon ausgegangen, dass Sie die Reihe durch Aktualisieren von Datenbankdaten in ASP.net Web PA abgeschlossen haben...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510459"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a>Einführung in ASP.net Web Pages Löschen von Datenbankdaten

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial wird gezeigt, wie Sie einen einzelnen Datenbankeintrag löschen. Dabei wird davon ausgegangen, dass Sie die Reihe durch Aktualisieren von Daten [Bank Daten in ASP.net Web Pages](updating-data.md)abgeschlossen haben.
> 
> Sie lernen Folgendes:
> 
> - Auswählen eines einzelnen Datensatzes aus einer Auflistung von Datensätzen.
> - Löschen eines einzelnen Datensatzes aus einer Datenbank.
> - Überprüfen, ob in einem Formular auf eine bestimmte Schaltfläche geklickt wurde.
>   
> 
> Erörterte Features und Technologien:
> 
> - Das `WebGrid`-Hilfsprogramm.
> - Der SQL `Delete`-Befehl.
> - Die `Database.Execute`-Methode, um einen SQL `Delete`-Befehl auszuführen.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie gelernt, wie ein vorhandener Daten Bank Datensatz aktualisiert wird. Dieses Lernprogramm ist ähnlich, mit der Ausnahme, dass Sie es löschen, anstatt den Datensatz zu aktualisieren. Die Prozesse sind sehr identisch, mit dem Unterschied, dass das Löschen einfacher ist, sodass dieses Tutorial kurz ist.

Auf der Seite " *Filme* " Aktualisieren Sie das `WebGrid` Hilfsprogramm, sodass neben den einzelnen Filmen ein **Lösch** Link angezeigt wird, der dem zuvor hinzugefügten **Bearbeitungs** Link entspricht.

![Seite "Filme", die einen Lösch Link für jeden Film anzeigt](deleting-data/_static/image1.png)

Beim Klicken auf den Link **Löschen** gelangen Sie wie bei der Bearbeitung zu einer anderen Seite, auf der sich die Filminformationen bereits in einem Formular befinden:

![Movie Page mit angezeigter Film löschen](deleting-data/_static/image2.png)

Sie können dann auf die Schaltfläche klicken, um den Datensatz dauerhaft zu löschen.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Hinzufügen eines Lösch Links zur Filmliste

Sie beginnen mit dem Hinzufügen eines **Lösch** Links zum `WebGrid`-Hilfsprogramm. Dieser Link ähnelt dem Link **Bearbeiten** , den Sie in einem vorherigen Tutorial hinzugefügt haben.

Öffnen Sie die Datei *Movies. cshtml* .

Ändern Sie das `WebGrid` Markup im Text der Seite, indem Sie eine Spalte hinzufügen. Hier ist das geänderte Markup:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Die neue Spalte lautet wie folgt:

[!code-html[Main](deleting-data/samples/sample2.html)]

Die Art und Weise, in der das Raster konfiguriert ist, ist die Spalte **Bearbeiten** im Raster ganz links, und die Spalte **Löschen** ist ganz rechts. (Es gibt jetzt ein Komma hinter der Spalte "`Year`", falls Sie dies nicht bemerkt haben.) Es gibt nichts besonderes, wo diese Link Spalten zu finden sind, und Sie können Sie einfach nebeneinander platzieren. In diesem Fall sind Sie getrennt, damit Sie nicht mehr gemischt werden können.

![Seite "Filme" mit den Links "Bearbeiten" und "Details" gekennzeichnet, um anzuzeigen, dass Sie nicht nebeneinander sind](deleting-data/_static/image3.png)

Die neue Spalte zeigt einen Link (`<a>`-Element) an, dessen Text "Delete" lautet. Das Ziel des Links (dessen `href`-Attribut) ist Code, der letztendlich in etwa diese URL aufgelöst wird, wobei der `id` Wert für jeden Film anders ist:

[!code-css[Main](deleting-data/samples/sample3.css)]

Mit diesem Link wird eine Seite mit dem Namen *deletemovie* aufgerufen und die ID des von Ihnen ausgewählten Films übergeben.

In diesem Tutorial wird nicht ausführlich erläutert, wie dieser Link erstellt wird, da er fast identisch mit dem **Bearbeitungs** Link aus dem vorherigen Tutorial ist ([Aktualisieren von Datenbankdaten in ASP.net Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Erstellen der Seite "Löschen"

Nun können Sie die Seite erstellen, die als Ziel für den **Lösch** Link im Raster verwendet wird.

> [!NOTE] 
> 
> **Wichtig** Das Verfahren zum ersten auswählen eines zu löschenden Datensatzes und zum anschließenden Verwenden einer separaten Seite und Schaltfläche, um den Prozess zu bestätigen, ist für die Sicherheit äußerst wichtig. Wie Sie bereits in den vorherigen Tutorials gelesen haben *, sollten Sie* eine *beliebige* Änderung an Ihrer Website durchführen, indem Sie eine Form &mdash;, die einen HTTP Post-Vorgang verwendet. Wenn Sie den Standort durch Klicken auf einen Link (d. h. mithilfe eines Get-Vorgangs) ändern können, können die Benutzer einfache Anforderungen an Ihre Website senden und die Daten löschen. Auch ein Suchengine-Crawler, der Ihre Website indiziert, könnte versehentlich Daten durch die folgenden Links löschen.
> 
> Wenn Ihre APP Benutzern das Ändern eines Datensatzes ermöglicht, müssen Sie den Datensatz für die Bearbeitung auf jeden Fall an den Benutzer übernehmen. Möglicherweise ist es aber verlockend, diesen Schritt zum Löschen eines Datensatzes zu überspringen. Überspringen Sie diesen Schritt jedoch nicht. (Außerdem ist es für Benutzer hilfreich, den Datensatz anzuzeigen und zu bestätigen, dass der von Ihnen beabsichtigte Datensatz gelöscht wird.)
> 
> In einem nachfolgenden Tutorial finden Sie Informationen zum Hinzufügen von Anmelde Funktionen, damit sich ein Benutzer vor dem Löschen eines Datensatzes anmelden muss.

Erstellen Sie eine Seite mit dem Namen *deletemovie. cshtml* , und ersetzen Sie die Elemente in der Datei durch das folgende Markup:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Dieses Markup ähnelt den *editmovie* -Seiten, mit dem Unterschied, dass das Markup `<span>` Elemente enthält, anstatt Textfelder (`<input type="text">`) zu verwenden. Es gibt nichts, was bearbeitet werden muss. Sie müssen lediglich die Filmdetails anzeigen, damit Benutzer sicherstellen können, dass Sie den richtigen Film löschen.

Das Markup enthält bereits einen Link, mit dem der Benutzer zur filmauflistungs Seite zurückkehren kann.

Wie auf der Seite *editmovie* wird die ID des ausgewählten Films in einem ausgeblendeten Feld gespeichert. (Er wird an die Seite an erster Stelle als Abfrage Zeichenfolge-Wert an die Seite geleitet.) Es gibt einen `Html.ValidationSummary`-Befehl, mit dem Validierungs Fehler angezeigt werden. In diesem Fall könnte der Fehler darin bestehen, dass keine Film-ID an die Seite oder die Film-ID ungültig ist. Diese Situation kann eintreten, wenn jemand diese Seite ausgeführt hat, ohne zuvor einen Film auf der Seite " *Filme* " auszuwählen.

Die Schaltflächen Beschriftung lautet **Delete Movie**, und das Name-Attribut ist auf `buttonDelete`festgelegt. Das `name`-Attribut wird im Code verwendet, um die Schaltfläche zu identifizieren, die das Formular übermittelt hat.

Sie müssen Code in 1 schreiben) lesen Sie die Filmdetails, wenn die Seite zum ersten Mal angezeigt wird, und 2), und löschen Sie den Film, wenn der Benutzer auf die Schaltfläche klickt.

## <a name="adding-code-to-read-a-single-movie"></a>Hinzufügen von Code zum Lesen eines einzelnen Films

Fügen Sie am oberen Rand der Seite *deletemovie. cshtml* den folgenden Codeblock hinzu:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Dieses Markup ist mit dem entsprechenden Code auf der Seite *editmovie* identisch. Er ruft die Film-ID aus der Abfrage Zeichenfolge ab und verwendet die ID zum Lesen eines Datensatzes aus der Datenbank. Der Code enthält den Validierungstest (`IsInt()` und `row != null`), um sicherzustellen, dass die an die Seite übergebenen Movie-ID gültig ist.

Beachten Sie, dass dieser Code nur bei der erstmaligen Ausführung der Seite ausgeführt werden sollte. Sie möchten den Film Daten Satz nicht erneut aus der Datenbank lesen, wenn der Benutzer auf die Schaltfläche " **Film löschen** " klickt. Daher befindet sich der Code zum Lesen des Films in einem Test, der `if(!IsPost)` &mdash; heißt, *Wenn die Anforderung kein Post-Vorgang ist (Formular Übermittlung)* .

## <a name="adding-code-to-delete-the-selected-movie"></a>Hinzufügen von Code zum Löschen des ausgewählten Films

Wenn Sie den Film löschen möchten, wenn der Benutzer auf die Schaltfläche klickt, fügen Sie den folgenden Code direkt innerhalb der schließenden geschweiften Klammer des `@`-Blocks ein:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Dieser Code ähnelt dem Code zum Aktualisieren eines vorhandenen Datensatzes, aber einfacher. Der Code führt im Grunde eine SQL `Delete`-Anweisung aus.

 Wie auf der *editmovie* -Seite befindet sich der Code in einem `if(IsPost)`-Block. Dieses Mal ist die `if()` Bedingung etwas komplizierter: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Hier gibt es zwei Bedingungen. Der erste besteht darin, dass die Seite übermittelt wird, wie Sie vor &mdash; `if(IsPost)`gesehen haben.

Die zweite Bedingung ist `!Request["buttonDelete"].IsEmpty()`. Dies bedeutet, dass die Anforderung über ein Objekt mit dem Namen `buttonDelete`verfügt. Zugegeben, es ist eine indirekte Methode, um zu testen, welche Schaltfläche das Formular übermittelt hat. Wenn ein Formular mehrere Sende Schaltflächen enthält, wird nur der Name der Schaltfläche, auf die geklickt wurde, in der Anforderung angezeigt. Wenn der Name einer bestimmten Schaltfläche in der Anforderung &mdash; oder wie im Code angegeben erscheint, ist diese Schaltfläche logisch, wenn diese Schaltfläche nicht leer ist &mdash; das ist die Schaltfläche, die das Formular übermittelt hat.

Der `&&`-Operator bedeutet "and" (logisches and). Daher ist die gesamte `if` Bedingung...

*Diese Anforderung ist ein Post (keine erstmalige Anforderung).*  
  
 AND  
  
*Die Schaltfläche `buttonDelete`* *war die Schaltfläche, die das Formular übermittelt hat.*

Dieses Formular (auf dieser Seite) enthält nur eine Schaltfläche, sodass der zusätzliche Test für `buttonDelete` technisch nicht erforderlich ist. Dennoch sind Sie im Begriff, einen Vorgang durchzuführen, mit dem Daten dauerhaft entfernt werden. Daher sollten Sie so sicher wie möglich sein, dass Sie den Vorgang nur ausführen, wenn der Benutzer ihn explizit angefordert hat. Nehmen Sie beispielsweise an, dass Sie diese Seite später erweitert und ihr weitere Schaltflächen hinzugefügt haben. Selbst dann wird der Code, der den Film löscht, nur dann ausgeführt, wenn auf die Schaltfläche "`buttonDelete`" geklickt wurde.

Wie auf der Seite *editmovie* erhalten Sie die ID aus dem ausgeblendeten Feld und führen dann den SQL-Befehl aus. Die Syntax für die `Delete`-Anweisung lautet wie folgt:

`DELETE FROM table WHERE ID = value`

Es ist wichtig, dass Sie die `WHERE`-Klausel und die ID einschließen. Wenn Sie die WHERE-Klausel weglassen, *werden alle Datensätze in der Tabelle gelöscht*. Wie Sie gesehen haben, übergeben Sie den ID-Wert an den SQL-Befehl, indem Sie einen Platzhalter verwenden.

## <a name="testing-the-movie-delete-process"></a>Testen des Film Löschvorgangs

Jetzt können Sie testen. Führen Sie die Seite *Filme* aus, und klicken Sie auf **Löschen** neben einem Film. Wenn die Seite *deletemovie* angezeigt wird, klicken Sie auf **Film löschen**.

![Löschen der Filmseite mit hervorgehobener Schaltfläche "Movie](deleting-data/_static/image4.png)

Wenn Sie auf die Schaltfläche klicken, löscht der Code die Filme und kehrt zur Filmliste zurück. Dort können Sie nach dem gelöschten Film suchen und sich vergewissern, dass er gelöscht wurde.

## <a name="coming-up-next"></a>Nächste nächste

Das nächste Tutorial zeigt, wie Sie allen Seiten auf Ihrer Website ein gängiges Aussehen und Layout zuordnen.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Vervollständigen der Auflistung für Movie Page (aktualisiert mit Links zum Löschen)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Vervollständigen der Liste für die deletemovie-Seite

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax](../introducing-razor-syntax-c.md)
- [SQL DELETE-Anweisung](http://www.w3schools.com/sql/sql_delete.asp) auf der W3Schools-Website

> [!div class="step-by-step"]
> [Zurück](updating-data.md)
> [Weiter](layouts.md)
