---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Hinzufügen einer neuen Kategorie zu "Dropdown List" mithilfe der jQuery-Benutzeroberfläche | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: cb9053593e2ea788638aec063c845cb91121861b
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075111"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Hinzufügen einer neuen Kategorie zu DropDownList mithilfe der jQuery-Benutzeroberfläche

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

Das HTML-`Select`-Tag eignet sich ideal für die Darstellung einer Liste von Daten fester Kategorie. häufig müssen Sie jedoch eine neue Kategorie hinzufügen. Angenommen, wir möchten das Genre "Opera" den Kategorien in unserer Datenbank hinzufügen? In diesem Abschnitt verwenden wir die jQuery-Benutzeroberfläche, um ein Dialogfeld hinzuzufügen, das zum Hinzufügen einer neuen Kategorie verwendet werden kann. In der folgenden Abbildung wird gezeigt, wie die Benutzeroberfläche im Browser vorhanden ist.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Wenn ein Benutzer den Link **neues Genre hinzufügen** auswählt, wird der Benutzer in einem Popup Dialogfeld zur Eingabe eines neuen Genre namens (und optional einer Beschreibung) aufgefordert. Die folgende Abbildung zeigt das Popup Dialogfeld **Genre hinzufügen** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Wenn ein neuer Genre Name eingegeben wird und die Schaltfläche " **Speichern** " gedrückt wird, geschieht Folgendes:

1. Ein AJAX-Befehl sendet die Daten an die Create-Methode des Genre Controllers, der das neue Genre in der Datenbank speichert und die neuen Genre Informationen (Genre Name und ID) als JSON zurückgibt.
2. JavaScript fügt die neuen Genre Daten der Auswahlliste hinzu.
3. JavaScript macht das neue Genre zum ausgewählten Element.

   In der folgenden Abbildung wurde **Opera** der Datenbank hinzugefügt und in der Dropdown Liste **Genre** ausgewählt. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Öffnen Sie die Datei *views\storemanager\kreate.cshtml* , und ersetzen Sie das Genre Markup durch den folgenden Code:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Die `_ChooseGenre` partielle Ansicht enthält die gesamte Logik zum Verbinden von JavaScript und jQuery, die zum Implementieren des Features neues Genre hinzufügen verwendet werden. Nachdem wir den Code abgeschlossen haben, ist es einfach, mit der Benutzeroberfläche des Künstlers gleich zu Vorgehen.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *views\storemanager* , und wählen Sie **Hinzufügen**und dann **anzeigen**aus. Geben Sie in der Eingabe für den **Ansichts Namen** `_ChooseGenre` klicken Sie dann auf **Hinzufügen**. Ersetzen Sie das Markup in der Datei *views\storemanager\\_ChooseGenre. cshtml* durch Folgendes:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Die erste Zeile deklariert, dass wir eine `Album` als Modell übergeben, genau die gleiche Modell Anweisung, die in der Create-Ansicht gefunden wurde. **Die nächsten Zeilen sind das Markup** -Hilfsobjekt. Die nächste Zeile ist der **Dropdown List** -Hilfsobjekt, genau das gleiche wie in der ursprünglichen CREATE-Sicht. Die nächste Zeile fügt einen Link mit dem Namen `Add New Genre`hinzu und formatiert ihn wie eine Schaltfläche. Die Zeile, die `ValidationMessageFor` enthält, wird direkt aus der Create-Ansicht kopiert. Die folgenden Zeilen:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

erstellt ein verborgenes div mit der ID `genreDialog`. Wir verwenden jQuery, um das Dialogfeld **Genre hinzufügen** mit der ID `genreDialog` in diesem DIV-Ereignis zu verbinden. Die letzten zwei Skript Tags enthalten Links zu den JavaScript-Dateien, die zum Implementieren des Features neues Genre hinzufügen verwendet werden. Die Datei */Scripts/chooseGenre.js* wird für Sie im Projekt bereitgestellt. Wir werden Sie später in diesem Tutorial untersuchen.

Führen Sie die Anwendung aus, und klicken Sie auf die Schaltfläche **neues Genre hinzufügen** . Geben Sie im Dialogfeld **Genre hinzufügen** im Feld **Name die Bezeichnung** **Opera** ein.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Klicken Sie auf die Schaltfläche **Save** . Ein AJAX-Befehl erstellt die Kategorie "Opera", füllt die Dropdown Liste mit "Opera" auf und legt Opera als ausgewähltes Genre fest.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Geben Sie einen Künstler, Titel und Preis ein, und wählen Sie dann die Schaltfläche **Erstellen** aus. Wenn Sie einen Preis unter $8,99 eingeben, wird das neue Album oben in der Index Sicht angezeigt. Vergewissern Sie sich, dass der neue Album Eintrag in der Datenbank gespeichert wurde.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Erstellen Sie ein neues Genre mit nur einem Buchstaben. Der folgende Code in der Datei *models\genre.cs* legt die minimale und maximale Länge des Genre namensfest.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Client seitige Validierungs Berichte Sie müssen eine Zeichenfolge zwischen 2 und 20 Zeichen eingeben.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Es wird geprüft, wie ein neues Genre zur Datenbank und zur Auswahlliste hinzugefügt wird.

Öffnen Sie die Datei *scripung\choosegenre.js* , und untersuchen Sie den Code.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

In der zweiten Zeile wird die ID `genreDialog` verwendet, um ein Dialogfeld für das div-Tag in der Datei *views\storemanager\\_ChooseGenre. cshtml* zu erstellen. Die meisten benannten Parameter sind selbsterklärend. Der `autoOpen`-Parameter ist auf false festgelegt. Wenn Sie die Schaltfläche **Genre erstellen** auswählen, wird der Dialog explizit geöffnet (Dies wird in diesem Abschnitt beschrieben). Das Dialogfeld verfügt über zwei Schaltflächen, **Speichern** und **Abbrechen**. Die Schaltfläche **Abbrechen** schließt das Dialogfeld. Der folgende Code zeigt die Funktion zum **Speichern** von Schaltflächen.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Die `var createGenreForm` wird aus der `createGenreForm`-ID ausgewählt. Die `createGenreForm`-ID wurde im folgenden Code in der Datei *views\genre\\_CreateGenre. cshtml* festgelegt.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Die in der Datei *views\genre\\_CreateGenre. cshtml* verwendete hilfsüberladung [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) generiert HTML mit einem action-Attribut, das die URL zum Senden des Formulars enthält. Sie können dies sehen, indem Sie die Seite Album erstellen in einem Browser anzeigen und im Browser Quelle anzeigen auswählen. Das folgende Markup zeigt den generierten HTML-Code, der das Formular-Tag enthält.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Die jQuery-`$.post` Zeile führt einen AJAX-Rückruf für das action-Attribut (`/StoreManager/Create`) aus und übergibt die Daten aus dem Dialogfeld " **Genre erstellen** ". Die Daten bestehen aus dem Namen des neuen Genres und einer optionalen Beschreibung. Wenn der AJAX-Befehl erfolgreich ausgeführt wird, werden der neue Genre Name und-Wert dem SELECT-Markup hinzugefügt, und das neue Genre wird auf den ausgewählten Wert festgelegt. Da dieses dynamisch generierte Markup ist, wird die neue Auswahl Option nicht angezeigt, indem die Quelle im Browser angezeigt wird. Sie können den neuen HTML-Code mit den IE 9 F12-Entwicklertools anzeigen. Drücken Sie zum Anzeigen der neuen Select-Option in Internet Explorer 9 die Taste F12, um die F12-Entwicklertools zu starten. Navigieren Sie zur Seite erstellen, und fügen Sie ein neues Genre hinzu, damit das neue Genre in der Auswahlliste Genre ausgewählt wird. In den F12-Entwicklertools:

1. Wählen Sie die Registerkarte HTML aus.
2. Klicken Sie auf das Aktualisierungs Symbol.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Geben Sie im Suchfeld den Suchbegriff GenreID ein.
4. Mithilfe des nächsten Symbols   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Navigieren Sie zum folgenden Tag auswählen:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Erweitern Sie den Wert der letzten Option.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Der folgende Code in der Datei *scripung\choosegenre.js* zeigt, wie die Schaltfläche **neues Genre hinzufügen** mit dem Click-Ereignis verbunden wird und wie das Dialogfeld **neues Genre hinzufügen** erstellt wird.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

In der ersten Zeile wird eine Klickfunktion erstellt, die an die Schaltfläche **neues Genre hinzufügen** angeschlossen ist. Das folgende Markup aus der Datei views\storemanager\\_ChooseGenre. cshtml zeigt, wie die Schaltfläche **neues Genre hinzufügen** erstellt wird:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Mit der Load-Methode wird das Dialogfeld Genre hinzufügen erstellt und geöffnet, und die jQuery-`parse`-Methode wird aufgerufen, sodass die Client Validierung für Daten erfolgt, die im Dialog

In diesem Abschnitt haben Sie erfahren, wie Sie ein Dialogfeld erstellen, das verwendet werden kann, um einer Auswahlliste neue Kategoriedaten hinzuzufügen. Sie können das gleiche Verfahren zum Erstellen einer Benutzeroberfläche ausführen, um der Auswahlliste des Künstlers einen neuen Interpreten hinzuzufügen. Dieses Tutorial bietet einen Überblick über die Arbeit mit der **Dropdown Liste**ASP.NET MVC-HTML-Hilfsobjekt. Weitere Informationen zum Arbeiten mit der **Dropdown Liste**finden Sie im Abschnitt Additions Verweise weiter unten. Teilen Sie uns mit, ob dieses Tutorial hilfreich ist.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Zusätzliche Referenzen

- [ASP.NET MVC – Cascading Dropdown listet Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) von [Radu enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Ausgewählt](https://harvesthq.github.com/chosen/) Ein JavaScript-Plug-in, das die Mehrfachauswahl und Filterung unterstützt.

### <a name="contributors"></a>Mitwirkende

- [Radu-Anmeldung](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Prüfer

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Papst
- Tom Dykstra

> [!div class="step-by-step"]
> [Previous](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
