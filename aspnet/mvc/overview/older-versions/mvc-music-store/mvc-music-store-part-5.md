---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Teil 5: Bearbeiten von Formularen und Vorlagen | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 5 umfasst Bearbeitungs Formulare und Vorlagen.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450909"
---
# <a name="part-5-edit-forms-and-templating"></a>Teil 5: Bearbeiten von Formularen und Vorlagen

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 5 umfasst Bearbeitungs Formulare und Vorlagen.

Im letzten Kapitel haben wir Daten aus der Datenbank geladen und angezeigt. In diesem Kapitel wird auch das Bearbeiten der Daten ermöglicht.

## <a name="creating-the-storemanagercontroller"></a>Erstellen von storemanagercontroller

Zunächst erstellen wir einen neuen Controller mit dem Namen " **storemanagercontroller**". Für diesen Controller werden die Gerüstbau Features genutzt, die im ASP.NET MVC 3 Tools Update verfügbar sind. Legen Sie die Optionen für das Dialogfeld Controller hinzufügen wie unten dargestellt fest.

![](mvc-music-store-part-5/_static/image1.png)

Wenn Sie auf die Schaltfläche Hinzufügen klicken, sehen Sie, dass der ASP.NET MVC 3-gerüstingmechanismus eine gute Menge an Arbeit für Sie erledigt:

- Er erstellt den neuen storemanagercontroller mit einer lokalen Entity Framework Variable.
- Dem Ordner "Views" des Projekts wird ein StoreManager-Ordner hinzugefügt.
- Sie fügt die Sicht "Create. cshtml", "Delete. cshtml", "Details. cshtml", "Edit. cshtml" und "index. cshtml" hinzu, die stark typisiert ist.

![](mvc-music-store-part-5/_static/image2.png)

Die neue StoreManager Controller-Klasse enthält CRUD (Create, Read, Update, DELETE) Controller-Aktionen, die wissen, wie Sie mit der Album Model-Klasse arbeiten und den Entity Framework Kontext für den Datenbankzugriff verwenden.

## <a name="modifying-a-scaffolded-view"></a>Ändern einer Gerüst Ansicht

Es ist wichtig zu beachten, dass dieser Code zwar für uns generiert wurde, aber auch der standardmäßige ASP.NET-MVC-Code ist, ebenso wie wir in diesem Tutorial geschrieben haben. Sie soll Ihnen den Zeitaufwand für das Schreiben von Code für Code Bausteine und das manuelle Erstellen der stark typisierten Ansichten ersparen, aber dies ist nicht die Art des generierten Codes, den Sie möglicherweise als Präfix mit düsteren Warnungen in Kommentaren über das Ändern der Ordnung. Dies ist Ihr Code, und Sie sollten ihn ändern.

Beginnen wir also mit einer schnellen Bearbeitung der Ansicht "StoreManager Index" (/Views/StoreManager/Index.cshtml). In dieser Ansicht wird eine Tabelle angezeigt, in der die im Store enthaltenen Alben mit den Links bearbeiten/Details/löschen aufgelistet werden und die öffentlichen Eigenschaften des Albums enthalten sind. Wir entfernen das Feld "albumarturl", da es in dieser Anzeige nicht sehr nützlich ist. Entfernen Sie in &lt;Tabelle&gt; Abschnitt des Ansichts Codes die &lt;Th-&gt; und &lt;TD-Elemente, die die "albumarturl"-Verweise umgeben, wie in den markierten Zeilen unten aufgeführt:&gt;

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Der geänderte Ansichts Code wird wie folgt angezeigt:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Ein erster Blick auf den Store-Manager

Führen Sie nun die Anwendung aus, und navigieren Sie zu/StoreManager/. Dadurch wird der soeben geänderte Speicher-Manager-Index angezeigt, und es wird eine Liste der Alben im Speicher mit Links zu bearbeiten, Details und löschen angezeigt.

![](mvc-music-store-part-5/_static/image3.png)

Wenn Sie auf den Link bearbeiten klicken, wird ein Bearbeitungs Formular mit Feldern für das Album angezeigt, einschließlich der Dropdown Liste für Genre und Künstlerin.

![](mvc-music-store-part-5/_static/image4.png)

Klicken Sie unten auf den Link "zurück zum Auflisten", und klicken Sie dann auf den Link Details für ein Album. Dadurch werden die Detailinformationen für ein einzelnes Album angezeigt.

![](mvc-music-store-part-5/_static/image5.png)

Klicken Sie erneut auf den Link zurück zu Liste, und klicken Sie dann auf den Link löschen. Dadurch wird ein Bestätigungs Dialogfeld angezeigt, in dem die Details des Albums angezeigt werden, und Sie werden gefragt, ob wir es löschen möchten.

![](mvc-music-store-part-5/_static/image6.png)

Wenn Sie im unteren Bereich auf die Schaltfläche Löschen klicken, wird das Album gelöscht, und Sie kehren zur Index Seite zurück, auf der das gelöschte Album angezeigt wird.

Wir arbeiten nicht mit dem Store Manager, aber wir verfügen über einen funktionierenden Controller und zeigen Code an, von dem aus die CRUD-Vorgänge gestartet werden.

## <a name="looking-at-the-store-manager-controller-code"></a>Sehen Sie sich den Store Manager-Controller Code an

Der Store Manager-Controller enthält eine gute Menge an Code. Wir gehen dies von oben nach unten durch. Der Controller enthält einige Standardnamespaces für einen MVC-Controller sowie einen Verweis auf den Namespace "Models". Der Controller verfügt über eine private Instanz von "musicstoreentities", die von den einzelnen Controller Aktionen für den Datenzugriff verwendet wird.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Speicher-Manager-Index-und Detail Aktionen

Die Index Ansicht Ruft eine Liste von Alben ab, einschließlich der referenzierten Genre-und Künstlerinformationen jedes Albums, wie zuvor bei der Arbeit an der Store-Methode zum Durchsuchen aufgetreten war. Die Index Sicht folgt den verweisen auf die verknüpften Objekte, sodass Sie den Genre Namen und den Namen der einzelnen Alben anzeigen kann, sodass der Controller effizient ist und diese Informationen in der ursprünglichen Anforderung abfragt.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Die Detail Controller Aktion des StoreManager-Controllers funktioniert genau wie die zuvor geschriebene Aktion "Store Controller-Details", die mit der Find ()-Methode nach dem Album abgefragt und dann an die Ansicht zurückgegeben wird.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Die Create Action-Methoden

Die Methoden zum Erstellen von Aktionen unterscheiden sich geringfügig von denen, die Sie bisher gesehen haben, da Sie Formular Eingaben verarbeiten. Wenn ein Benutzer das erste Mal besucht/StoreManager/Create/wird ein leeres Formular angezeigt. Diese HTML-Seite enthält ein &lt;Formular&gt; Element, das Dropdown-und Textfeld-Eingabeelemente enthält, in denen die Details des Albums eingegeben werden können.

Nachdem der Benutzer die Album-Formular Werte ausgefüllt hat, kann er die Schaltfläche "Speichern" drücken, um diese Änderungen an die Anwendung zurückzusenden, um Sie in der Datenbank zu speichern. Wenn der Benutzer die Schaltfläche "Speichern" drückt, wird das &lt;Formular&gt; einen HTTP-Post-Vorgang an die/StoreManager/Create/-URL durchführen und das &lt;Formular&gt; Werte als Teil von HTTP-Post senden.

ASP.NET MVC ermöglicht das einfache Aufteilen der Logik dieser beiden URL-Aufruf Szenarien, indem es uns ermöglicht, zwei separate "Create"-Aktionsmethoden in unserer storemanagercontroller-Klasse zu implementieren – eine, um die erste HTTP-Get-Suche zur/StoreManager/Create/-URL zu verarbeiten, und die andere, um den HTTP-Post-Vorgang der übermittelten Änderungen zu verarbeiten.

### <a name="passing-information-to-a-view-using-viewbag"></a>Übergeben von Informationen an eine Ansicht mithilfe von viewbag

Wir haben den viewbag weiter oben in diesem Tutorial verwendet, aber noch nicht viel darüber gesprochen. Der viewbag ermöglicht uns, Informationen an die Ansicht zu übergeben, ohne ein stark typisiertes Modell Objekt zu verwenden. In diesem Fall muss unsere Aktion zum Bearbeiten des HTTP-Get-Controllers sowohl eine Liste der Genres als auch Künstler an das Formular übergeben, um die Dropdown Listen zu füllen. die einfachste Möglichkeit hierfür ist, Sie als viewbag-Elemente zurückzugeben.

Der viewbag ist ein dynamisches Objekt, d. h., Sie können "viewbag. foo" oder "viewbag. yournamehere" eingeben, ohne Code zu schreiben, um diese Eigenschaften zu definieren. In diesem Fall verwendet der Controller Code "viewbag. GenreID" und "viewbag. artistId", sodass die mit dem Formular übermittelten Dropdown Werte GenreID und artistId sind. Dies sind die von Ihnen fest zuweisenden Album Eigenschaften.

Diese Dropdown Werte werden an das Formular zurückgegeben, indem das SelectList-Objekt verwendet wird, das nur für diesen Zweck erstellt wird. Dies erfolgt mithilfe von Code wie dem folgenden:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Wie Sie im Code der Aktionsmethode sehen können, werden drei Parameter verwendet, um dieses Objekt zu erstellen:

- Die Liste der Elemente, die in der Dropdown Liste angezeigt werden. Beachten Sie, dass es sich nicht nur um eine Zeichenfolge handelt. Wir übergeben eine Liste von Genres.
- Der nächste Parameter, der an die SelectList übergeben wird, ist der ausgewählte Wert. Auf diese Weise weiß die SelectList, wie ein Element in der Liste vorab ausgewählt wird. Dies ist leichter zu verstehen, wenn wir uns das Bearbeitungs Formular ansehen, das ziemlich ähnlich ist.
- Der letzte Parameter ist die anzuzeigende Eigenschaft. In diesem Fall ist dies ein Hinweis darauf, dass die Genre.Name-Eigenschaft dem Benutzer angezeigt wird.

Daher ist die HTTP-Get create-Aktion ziemlich einfach. zwei selectlists werden dem viewbag hinzugefügt, und es wird kein Modell Objekt an das Formular weitergegeben (da es noch nicht erstellt wurde).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>HTML-Hilfsprogramme zum Anzeigen der Dropdown Felder in der Ansicht "erstellen"

Da wir darüber gesprochen haben, wie die Dropdown Werte an die Ansicht übergeben werden, sehen wir uns die Ansicht genauer an, um zu sehen, wie diese Werte angezeigt werden. Im Ansichts Code (/Views/StoreManager/Create.cshtml) sehen Sie, dass der folgende-Befehl zum Anzeigen der Genre-Dropdown-Datei ausgelöst wird.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Dies wird als HTML-Hilfsobjekt bezeichnet: eine hilfsprogrammmethode, die eine gängige Ansichts Aufgabe ausführt. HTML-Hilfsprogramme sind sehr nützlich, um unseren Ansichts Code kurz und lesbar zu halten. Das Hilfsprogramm HTML. Dropdown List wird von ASP.NET MVC bereitgestellt, aber wie wir später sehen werden, ist es möglich, unsere eigenen Hilfsprogramme zum Anzeigen von Code zu erstellen, den wir in unserer Anwendung wieder verwenden.

Der HTML. DropDownList-Befehl muss lediglich zwei Dinge sagen können: wo soll die Liste angezeigt werden, und welcher Wert (falls vorhanden) sollte vorab ausgewählt werden. Der erste Parameter, GenreID, teilt der Dropdown Liste mit, dass im Modell oder in der viewbag nach einem Wert mit dem Namen "GenreID" gesucht werden soll. Der zweite Parameter wird verwendet, um den Wert anzugeben, der in der Dropdown Liste anfänglich ausgewählt angezeigt werden soll. Da dieses Formular ein Erstellungs Formular ist, gibt es keinen Wert, der vorab ausgewählt werden kann, und String. Empty wird übermittelt.

### <a name="handling-the-posted-form-values"></a>Verarbeiten der geposteten Formular Werte

Wie bereits erläutert, gibt es zwei Aktionsmethoden, die mit jedem Formular verknüpft sind. Der erste behandelt die HTTP-GET-Anforderung und zeigt das Formular an. Die zweite verarbeitet die HTTP-POST-Anforderung, die die übermittelten Formular Werte enthält. Beachten Sie, dass die Controller Aktion über ein [HttpPost]-Attribut verfügt, das ASP.NET MVC anweist, dass es nur auf HTTP-POST-Anforderungen reagieren soll.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Diese Aktion hat vier Zuständigkeiten:

- 1. Lesen der Formular Werte
- 2. Überprüfen Sie, ob die Formular Werte Validierungsregeln bestehen.
- 3. Wenn die Formular Übermittlung gültig ist, speichern Sie die Daten, und zeigen Sie die aktualisierte Liste an.
- 4. Wenn die Formular Übermittlung ungültig ist, wird das Formular mit Validierungs Fehlern erneut angezeigt.

#### <a name="reading-form-values-with-model-binding"></a>Lesen von Formular Werten mit Modell Bindung

Die Controller Aktion verarbeitet eine Formular Übermittlung, die Werte für GenreID und artistId (aus der Dropdown Liste) und Textfeld-Werte für Title, Price und albumarturl enthält. Obwohl es möglich ist, direkt auf Formular Werte zuzugreifen, ist es besser, die in ASP.NET MVC integrierten Funktionen für die Modell Bindung zu verwenden. Wenn eine Controller Aktion einen Modelltyp als Parameter annimmt, versucht ASP.NET MVC, ein Objekt dieses Typs mithilfe von Formular Eingaben (sowie Routen-und QueryString-Werte) aufzufüllen. Dies geschieht, indem nach Werten gesucht wird, deren Namen mit den Eigenschaften des Modell Objekts verglichen werden, z. b. beim Festlegen des GenreID-Werts des neuen Album Objekts, der nach einer Eingabe mit dem Namen "GenreID" sucht. Wenn Sie Ansichten mithilfe der Standardmethoden in ASP.NET MVC erstellen, werden die Formulare immer mithilfe von Eigenschaftsnamen als Eingabe Feldnamen gerendert, sodass die Feldnamen nur mit den Namen der Felder identisch werden.

#### <a name="validating-the-model"></a>Validieren des Modells

Das Modell wird mit einem einfachen Befehl von "modelstate. IsValid" überprüft. Wir haben noch keine Validierungsregeln zu unserer Album-Klasse hinzugefügt. Dies geschieht in gewisser Weise, da diese Prüfung momentan nicht viel zu tun hat. Wichtig ist, dass diese modelstat. IsValid-Überprüfung an die Validierungsregeln angepasst wird, die wir in unserem Modell ablegen, sodass zukünftige Änderungen an Validierungsregeln keine Aktualisierungen des Controller Aktionscodes erfordern.

#### <a name="saving-the-submitted-values"></a>Speichern der übermittelten Werte

Wenn die Formular Übermittlung die Überprüfung übergibt, ist es an der Zeit, die Werte in der Datenbank zu speichern. Bei Entity Framework muss das Modell nur der Alben-Auflistung hinzugefügt und SaveChanges aufgerufen werden.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework generiert die entsprechenden SQL-Befehle, um den Wert beizubehalten. Nachdem Sie die Daten gespeichert haben, werden wir zurück zur Liste der Alben umgeleitet, damit wir das Update sehen können. Dies erfolgt durch die Rückgabe von redirectdeaction mit dem Namen der Controller Aktion, die angezeigt werden soll. In diesem Fall ist dies die Index Methode.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Anzeigen Ungültiger Formular Übermittlungen mit Validierungs Fehlern

Im Fall einer ungültigen Formulareingabe werden die Dropdown Werte der viewbag (wie im Fall von HTTP-Get) hinzugefügt, und die gebundenen Modell Werte werden zur Anzeige an die Ansicht zurückgegeben. Validierungs Fehler werden automatisch mithilfe der @Html.ValidationMessageFor HTML-Hilfsobjekts angezeigt.

#### <a name="testing-the-create-form"></a>Testen des Formulars "erstellen"

Um dies zu testen, führen Sie die Anwendung aus, und navigieren Sie zu/StoreManager/Create/. Dadurch wird das leere Formular angezeigt, das von der StoreController Create HTTP-Get-Methode zurückgegeben wurde.

Geben Sie einige Werte ein, und klicken Sie auf die Schaltfläche erstellen, um das Formular zu senden

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Behandeln von Änderungen

Das Bearbeitungs Aktions Paar (http-Get und HTTP-Post) ähnelt den Methoden zum Erstellen von Aktionen, die wir gerade gesehen haben. Da das Bearbeitungs Szenario das Arbeiten mit einem vorhandenen Album umfasst, lädt die Edit HTTP-Get-Methode das Album basierend auf dem Parameter "ID", der über die Route übergeben wird. Dieser Code zum Abrufen eines Albums von albumId ist das gleiche, das wir zuvor in der Aktion "Details Controller" untersucht haben. Wie bei der Create/HTTP-Get-Methode werden die Dropdown Werte über den viewbag zurückgegeben. Dadurch können wir ein Album als Modell Objekt an die Ansicht (die stark in die Album-Klasse eingegeben wird) zurückgeben und gleichzeitig zusätzliche Daten (z. b. eine Liste von Genres) über den viewbag übergeben.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Die Aktion "http-Post bearbeiten" ähnelt sehr der Aktion "http-Post erstellen". Der einzige Unterschied besteht darin, dass kein neues Album zur Datenbank hinzugefügt wird. Alben-Auflistung. die aktuelle Instanz des Albums wird mithilfe von DB gefunden. Eintrag (Album), und legen Sie seinen Status auf geändert fest. Dies weist Entity Framework an, dass wir ein vorhandenes Album ändern, anstatt ein neues zu erstellen.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Wir können dies testen, indem Sie die Anwendung ausführen und zu/StoreManger/navigieren und dann auf den Link bearbeiten für ein Album klicken.

![](mvc-music-store-part-5/_static/image9.png)

Dadurch wird das Bearbeitungs Formular angezeigt, das von der Edit HTTP-Get-Methode angezeigt wird. Geben Sie einige Werte ein, und klicken Sie auf die Schaltfläche speichern.

![](mvc-music-store-part-5/_static/image10.png)

Dadurch wird das Formular gepostet, die Werte werden gespeichert, und die Liste wird zurückgegeben, und es wird angezeigt, dass die Werte aktualisiert wurden.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Behandeln der Löschung

Das Löschen folgt dem gleichen Muster wie "Bearbeiten und erstellen", wobei eine Controller Aktion zum Anzeigen des Bestätigungs Formulars verwendet wird, und eine weitere Controller Aktion zum Verarbeiten der Formular Übermittlung.

Die Aktion zum Löschen des HTTP-Get-Controllers ist exakt identisch mit der vorherigen Aktion für den Store Manager-Detail Controller.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Mit der Vorlage "Inhalt der Inhalts Vorlage löschen" wird ein Formular angezeigt, das stark typisiert ist.

![](mvc-music-store-part-5/_static/image12.png)

Die Vorlage "Löschen" zeigt alle Felder für das Modell an, aber wir können das Recht etwas vereinfachen. Ändern Sie den Ansichts Code in/views/StoreManager/DELETE.cshtml in den folgenden Code.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Dadurch wird eine vereinfachte Lösch Bestätigung angezeigt.

![](mvc-music-store-part-5/_static/image13.png)

Wenn Sie auf die Schaltfläche "Löschen" klicken, wird das Formular zurück an den Server gesendet, auf dem die deleteconfirmed-Aktion ausgeführt wird.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Unsere HTTP-Post-Aktion zum Löschen des Controllers führt die folgenden Aktionen aus:

- 1. Lädt das Album nach ID.
- 2. Löscht das Album und speichert die Änderungen.
- 3. Leitet an den Index weiter und zeigt an, dass das Album aus der Liste entfernt wurde.

Um dies zu testen, führen Sie die Anwendung aus, und navigieren Sie zu/StoreManager. Wählen Sie ein Album aus der Liste aus, und klicken Sie auf den Link löschen.

![](mvc-music-store-part-5/_static/image14.png)

Dadurch wird der Bestätigungsbildschirm für den Löschvorgang angezeigt.

![](mvc-music-store-part-5/_static/image15.png)

Wenn Sie auf die Schaltfläche "Löschen" klicken, wird das Album entfernt und die Seite "Store Manager Index" zurückgegeben, die anzeigt, dass das Album gelöscht wurde.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Verwenden eines benutzerdefinierten HTML-Hilfsprogramms zum Abschneiden von Text

Wir haben ein mögliches Problem mit unserer Store Manager-Index Seite. Die Eigenschaften "Album Titel" und "Artist Name" können so lange ausreichen, dass Sie die Tabellen Formatierung auslösen könnten. Wir erstellen ein benutzerdefiniertes HTML-Hilfsprogramm, mit dem wir diese und andere Eigenschaften in unseren Ansichten problemlos abschneiden können.

![](mvc-music-store-part-5/_static/image17.png)

Die @helper Syntax von Razor vereinfacht das Erstellen eigener Hilfsfunktionen für die Verwendung in ihren Ansichten. Öffnen Sie die/Views/StoreManager/Index.cshtml-Ansicht, und fügen Sie den folgenden Code direkt nach der @model Zeile ein.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Diese Hilfsmethode nimmt eine Zeichenfolge und eine maximale Länge an, um dies zuzulassen. Wenn der angegebene Text kürzer als die angegebene Länge ist, gibt das Hilfsprogramm ihn unverändert aus. Wenn er länger ist, wird der Text abgeschnitten und "..." gerendert. für den Rest.

Nun können wir unsere TRUNCATE-Hilfsmethode verwenden, um sicherzustellen, dass die Eigenschaften "Album Titel" und "Name des Künstlers" weniger als 25 Zeichen umfassen. Der gesamte Ansichts Code, der das neue TRUNCATE Helper verwendet, wird unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Wenn wir nun die/StoreManager/-URL durchsuchen, werden die Alben und Titel unter unsere maximale Länge beibehalten.

![](mvc-music-store-part-5/_static/image18.png)

Hinweis: Dies zeigt die einfache Groß-/Kleinschreibung beim Erstellen und Verwenden eines-Hilfsprogramms in einer Ansicht. Weitere Informationen zum Erstellen von Hilfsprogrammen, die Sie in Ihrer Website verwenden können, finden Sie in meinem Blogbeitrag: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-4.md)
> [Weiter](mvc-music-store-part-6.md)
