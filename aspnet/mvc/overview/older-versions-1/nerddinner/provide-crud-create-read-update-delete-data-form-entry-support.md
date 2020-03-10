---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Bereitstellen von CRUD (Create, Read, Update, DELETE)-Unterstützung für Datenformular Einträge | Microsoft-Dokumentation
author: microsoft
description: In Schritt 5 wird gezeigt, wie Sie die dinnerscontroller-Klasse weiter nutzen können, indem Sie auch die Unterstützung für das Bearbeiten, erstellen und Löschen von Abendessen aktivieren.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: b3123af9a1477bc496a0d229d628510fc202b6d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468915"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Bereitstellen von CRUD-Unterstützung (Create, Read, Update, Delete) für Datenformulareinträge

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 5 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> In Schritt 5 wird gezeigt, wie Sie die dinnerscontroller-Klasse weiter nutzen können, indem Sie auch die Unterstützung für das Bearbeiten, erstellen und Löschen von Abendessen aktivieren.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>Nerddinner Step 5: erstellen, aktualisieren, Löschen von Formular Szenarios

Wir haben Controller und Ansichten eingeführt und erläutert, wie Sie Sie verwenden können, um eine Auflistung/Details-Erfahrung für Dinner on Site zu implementieren. Der nächste Schritt besteht darin, die dinnerscontroller-Klasse weiter zu machen und die Unterstützung für das Bearbeiten, erstellen und Löschen von Abendessen ebenfalls zu aktivieren.

### <a name="urls-handled-by-dinnerscontroller"></a>Von dinnerscontroller behandelte URLs

Wir haben zuvor dem dinnerscontroller Aktionsmethoden hinzugefügt, die Unterstützung für zwei URLs implementiert haben: */Dinners* und */Dinners/Details/[ID]* .

| **URL** | **Ben** | **Zweck** |
| --- | --- | --- |
| *Serviert* | GET | Zeigt eine HTML-Liste mit bevorstehenden Abendessen an. |
| */Dinners/Details/[ID]* | GET | Anzeigen von Details zu einem bestimmten Dinner. |

Nun fügen wir Aktionsmethoden hinzu, um drei zusätzliche URLs zu implementieren: */Dinners/Edit/[ID]* , */Dinners/Create*und */Dinners/delete/[ID]* . Diese URLs ermöglichen die Unterstützung für das Bearbeiten vorhandener Abendessen, das Erstellen neuer Abendessen und das Löschen von Dinner.

Wir unterstützen sowohl HTTP Get-als auch HTTP POST-Verb Interaktionen mit diesen neuen URLs. HTTP GET-Anforderungen für diese URLs zeigen die anfängliche HTML-Ansicht der Daten (ein Formular, das mit den Dinner-Daten aufgefüllt ist, im Fall von "Edit", ein leeres Formular im Fall von "Create" und einen Bestätigungsbildschirm zum Löschen im Fall von "Delete") an. HTTP POST-Anforderungen an diese URLs Speichern/aktualisieren/löschen die Dinner-Daten in unserem dinnerrepository (und von dort aus in der Datenbank).

| **URL** | **Ben** | **Zweck** |
| --- | --- | --- |
| */Dinners/Edit/[ID]* | GET | Anzeigen eines bearbeitbaren HTML-Formulars, das mit Dinner Data aufgefüllt ist. |
| POST | Speichern Sie die Formular Änderungen für ein bestimmtes Dinner in der Datenbank. |
| */Dinners/Create* | GET | Zeigt ein leeres HTML-Formular an, das Benutzern das Definieren neuer Abendessen ermöglicht. |
| POST | Erstellen Sie ein neues Dinner, und speichern Sie es in der Datenbank. |
| */Dinners/delete/[ID]* | GET | Bildschirm zum Löschen der Bestätigung anzeigen. |
| POST | Löscht das angegebene Dinner aus der Datenbank. |

### <a name="edit-support"></a>Unterstützung bearbeiten

Wir beginnen mit der Implementierung des Szenarios "Edit" (Bearbeiten).

#### <a name="the-http-get-edit-action-method"></a>Die HTTP-Get Edit Action-Methode

Wir beginnen damit, das http-"Get"-Verhalten der Edit Action-Methode zu implementieren. Diese Methode wird aufgerufen, wenn die URL */Dinners/Edit/[ID]* angefordert wird. Unsere Implementierung sieht wie folgt aus:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Der obige Code verwendet das dinnerrepository zum Abrufen eines Dinner-Objekts. Anschließend wird mit dem Dinner-Objekt eine Ansichts Vorlage gerendert. Da wir nicht explizit einen Vorlagen Namen an die *View ()* -Hilfsmethode übermittelt haben, wird der auf der Konvention basierende Standardpfad verwendet, um die Ansichts Vorlage aufzulösen:/views/Dinners/Edit.aspx.

Nun erstellen wir diese Ansichts Vorlage. Hierzu klicken Sie mit der rechten Maustaste in die Bearbeitungsmethode und wählen den Kontextmenü Befehl "Ansicht hinzufügen" aus:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Im Dialogfeld "Ansicht hinzufügen" geben wir an, dass wir ein Dinner-Objekt an unsere Ansichts Vorlage als Modell übergeben, und wählen das automatische Gerüstbau der Vorlage "Bearbeiten" aus:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, fügt Visual Studio im Verzeichnis "\views\dinner" eine neue Ansichts Vorlagen Datei "Edit. aspx" für uns hinzu. Außerdem wird die neue Ansichts Vorlage "Edit. aspx" im Code-Editor geöffnet – mit einer anfänglichen "Bearbeitungs Gerüst"-Implementierung wie unten dargestellt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Nehmen wir einige Änderungen am standardmäßigen "Edit"-Gerüstbau vor, und aktualisieren Sie die Vorlage "Vorlage bearbeiten", damit Sie den folgenden Inhalt hat (wodurch einige der Eigenschaften entfernt werden, die wir nicht verfügbar machen möchten):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Wenn Sie die Anwendung ausführen und die URL *"/Dinners/Edit/1"* anfordern, wird die folgende Seite angezeigt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Das HTML-Markup, das von unserer Ansicht generiert wird, sieht wie folgt aus Dabei handelt es sich um ein standardmäßiges HTML-– mit einem &lt;Formular&gt; Element, das eine HTTP Post-Nachricht an die */Dinners/Edit/1* -URL ausführt, wenn die Schaltfläche "Save" &lt;Input Type = "Submit"/&gt; per Pushvorgang Ein HTML-&lt;Input Type = "Text"/&gt; Element wurde für jede bearbeitbare Eigenschaft ausgegeben:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>HTML. BeginForm ()-und HTML. TextBox () HTML-Hilfsmethoden

Unsere "Edit. aspx"-Ansichts Vorlage verwendet mehrere "HTML Helper"-Methoden: HTML. ValidationSummary (), HTML. BeginForm (), HTML. TextBox () und HTML. validationmessage (). Zusätzlich zum Erstellen von HTML-Markup für uns bieten diese Hilfsmethoden Integrierte Fehlerbehandlung und Validierungs Unterstützung.

##### <a name="htmlbeginform-helper-method"></a>HTML. BeginForm ()-Hilfsmethode

Mit der HTML. BeginForm ()-Hilfsmethode wird das HTML-&lt;Formular&gt; Element in unserem Markup ausgegeben. In der Ansichts Vorlage "Edit. aspx" werden Sie bemerken, dass C# bei Verwendung dieser Methode eine "using"-Anweisung angewendet wird. Die öffnende geschweifte Klammer gibt den Anfang des &lt;Formulars&gt; Inhalt an, und die schließende geschweifte Klammer zeigt das Ende des &lt;"/Form"&gt; Elements an:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Wenn Sie den Ansatz der "using"-Anweisung für ein Szenario wie diesen nicht verwenden können, können Sie alternativ eine HTML. BeginForm ()-und HTML. Endform ()-Kombination verwenden (was dasselbe tut):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Wenn Sie "HTML. BeginForm ()" ohne Parameter aufrufen, wird ein Formular Element ausgegeben, das eine HTTP-POST-Anforderung an die URL der aktuellen Anforderung sendet. Aus diesem Grund wird in der Bearbeitungs Ansicht ein *&lt;Form Action = "/Dinners/Edit/1" Method = "Post"&gt;* -Element generiert. Wir hätten auch explizite Parameter an HTML. BeginForm () übertragen können, wenn wir an eine andere URL Posten möchten.

##### <a name="htmltextbox-helper-method"></a>HTML. TextBox ()-Hilfsmethode

Unsere Ansicht "Edit. aspx" verwendet die HTML. TextBox ()-Hilfsmethode, um &lt;Input Type = "Text"/&gt; Elemente auszugeben:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Die obige HTML. Textfeld ()-Methode verwendet einen einzelnen Parameter –, der verwendet wird, um sowohl die ID-/namens-Attribute der &lt;Input Type = "Text"/&gt; Element anzugeben, die ausgegeben werden sollen, als auch die Model-Eigenschaft, mit der der Textfeld-Wert aufgefüllt wird. Beispielsweise hat das Dinner-Objekt, das wir an die Bearbeitungs Ansicht übermittelt haben, einen "Title"-Eigenschafts Wert von ".net Futures", und daher wird der HTML. TextBox ("Title")-Methodenaufrufe ausgegeben: *&lt;Input ID = "Title" Name = "Title" Type = "Text" Value = ". net Futures"/&gt;* .

Alternativ können wir den ersten HTML. TextBox ()-Parameter verwenden, um die ID bzw. den Namen des Elements anzugeben, und dann explizit den Wert übergeben, der als zweiter Parameter verwendet werden soll:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Häufig möchten wir eine benutzerdefinierte Formatierung für den Wert ausführen, der ausgegeben wird. Die in .NET integrierte statische String. Format ()-Methode ist für diese Szenarios nützlich. Mit der Ansichts Vorlage "Edit. aspx" wird der eventdate-Wert (der vom Typ "DateTime" ist) formatiert, sodass für die Zeit keine Sekunden angezeigt werden:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Ein dritter Parameter für HTML. TextBox () kann optional verwendet werden, um zusätzliche HTML-Attribute auszugeben. Der folgende Code Ausschnitt veranschaulicht, wie Sie ein zusätzliches size = "30"-Attribut und ein Class = "mycssclass"-Attribut für das &lt;Input Type = "Text"/&gt;-Element Rendering. Beachten Sie, dass wir den Namen des Class-Attributs mit einem "@" character because "-Klasse" als Schlüsselwort C#in einem reservierten Schlüsselwort versehen:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementieren der HTTP-Post-Bearbeitungs Aktionsmethode

Nun ist die HTTP-Get-Version der Edit Action-Methode implementiert. Wenn ein Benutzer die */Dinners/Edit/1* -URL anfordert, erhält er eine HTML-Seite wie die folgende:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Wenn Sie die Schaltfläche "Speichern" drücken, wird eine Formular Bereitstellung an die */Dinners/Edit/1* -URL gesendet, und die HTML-&lt;Eingabe&gt; Formular Werte werden mithilfe des HTTP POST-Verbs gesendet. Nun implementieren wir das HTTP Post-Verhalten der Edit Action-Methode –, die das Speichern des Dinner behandelt.

Wir beginnen mit dem Hinzufügen einer überladenen "Edit"-Aktionsmethode zu unserem dinnerscontroller, der über ein "Accept-Verbs"-Attribut verfügt, das angibt, dass es HTTP Post-Szenarien behandelt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Wenn das Attribut [akzeptverbs] auf überladene Aktionsmethoden angewendet wird, verarbeitet ASP.NET MVC automatisch die Verteilung von Anforderungen an die entsprechende Aktionsmethode, abhängig vom eingehenden HTTP-Verb. HTTP POST-Anforderungen an */Dinners/Edit/[ID]* -URLs werden an die obige Edit-Methode gesendet, während alle anderen HTTP-Verb Anforderungen an */Dinners/Edit/[ID]* -URLs an die erste von uns implementierte Bearbeitungsmethode (die nicht über ein `[AcceptVerbs]`-Attribut verfügte) gelangen.

| **Seitiges Thema: Gründe für die Unterscheidung über HTTP-Verben** |
| --- |
| Sie Fragen sich vielleicht – warum verwenden wir eine einzelne URL und unterscheiden Ihr Verhalten über das HTTP-Verb? Warum gibt es nicht nur zwei separate URLs, um das Laden und Speichern von Bearbeitungs Änderungen zu behandeln? Beispiel:/Dinners/Edit/[ID], um das Anfangs Formular und/Dinners/Save/[ID] anzuzeigen, um die Formular Bereitstellung zu speichern, um Sie zu speichern? Der Nachteil bei der Veröffentlichung von zwei separaten URLs besteht darin, dass in Fällen, in denen wir an/Dinners/Save/2 Posten und das HTML-Formular aufgrund eines Eingabe Fehlers erneut angezeigt werden muss, der Endbenutzer die/Dinners/Save/2-URL in der Adressleiste des Browsers erhält (da dies die URL ist, an die das Formular gesendet wurde). Wenn der Endbenutzer diese neu angezeigte Seite in der Browser Favoritenliste eingibt oder die URL kopiert, einfügt und an einen Freund sendet, speichert Sie am Ende eine URL, die in der Zukunft nicht mehr funktioniert (da diese URL von Post-Werten abhängt). Wenn Sie eine einzelne URL (z. b.:/Dinners/Edit/[ID]) verfügbar machen und die Verarbeitung durch das HTTP-Verb unterscheiden, ist es für Endbenutzer sicher, dass die Bearbeitungsseite als Lesezeichen versehen und/oder die URL an andere Personen gesendet wird. |

#### <a name="retrieving-form-post-values"></a>Abrufen von Formular Post-Werten

Es gibt eine Vielzahl von Möglichkeiten, auf bereitgestellte Formular Parameter in unserer HTTP Post-Methode "Edit" zuzugreifen. Ein einfacher Ansatz besteht darin, einfach die Request-Eigenschaft für die Controller-Basisklasse zu verwenden, um auf die Formular Auflistung zuzugreifen und die veröffentlichten Werte direkt abzurufen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Der obige Ansatz ist jedoch ein wenig ausführlich, vor allem dann, wenn wir Fehler Behandlungs Logik hinzugefügt haben.

Ein besserer Ansatz für dieses Szenario ist die Verwendung der integrierten *updatemodel ()* -Hilfsmethode für die Controller-Basisklasse. Es unterstützt das Aktualisieren der Eigenschaften eines Objekts, das wir mithilfe der eingehenden Formular Parameter übergeben. Sie verwendet Reflektion, um die Eigenschaftsnamen für das Objekt zu ermitteln, und konvertiert und ordnet Werte dann automatisch auf der Grundlage der vom Client gesendeten Eingabewerte zu.

Wir könnten die updatemodel ()-Methode verwenden, um die HTTP-Post-Bearbeitungsaktion mithilfe dieses Codes zu vereinfachen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Wir können nun die */Dinners/Edit/1* -URL aufrufen und den Titel unseres Dinner ändern:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Wenn Sie auf die Schaltfläche "Speichern" klicken, wird eine Formular Bereitstellung für die Aktion "Bearbeiten" durchgeführt, und die aktualisierten Werte werden in der Datenbank gespeichert. Wir werden dann zur Detail-URL für das Dinner umgeleitet (in dem die neu gespeicherten Werte angezeigt werden):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Behandeln von Bearbeitungs Fehlern

Die aktuelle HTTP-Post-Implementierung funktioniert problemlos – es sei denn, es liegen Fehler vor.

Wenn ein Benutzer eine Fehler Bearbeitung durchführt, müssen wir sicherstellen, dass das Formular erneut mit einer informativen Fehlermeldung angezeigt wird, die Sie zur Behebung von Fehlern führt. Dies gilt auch für Fälle, in denen ein Endbenutzer falsche Eingaben (z. b. eine falsch formatierte Datums Zeichenfolge) ausgibt, sowie Fälle, in denen das Eingabeformat gültig ist, aber eine Geschäftsregel Verletzung vorliegt. Wenn Fehler auftreten, sollte das Formular die Eingabedaten beibehalten, die der Benutzer ursprünglich eingegeben hat, damit Sie die Änderungen nicht manuell erneut ausfüllen müssen. Dieser Prozess sollte so oft wie nötig wiederholt werden, bis das Formular erfolgreich abgeschlossen wurde.

ASP.NET MVC enthält einige nützliche integrierte Features, mit denen Fehlerbehandlung und Formular Neuanzeige problemlos durchführen können. Um diese Features in Aktion zu sehen, aktualisieren wir die Edit-Aktionsmethode mit folgendem Code:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Der obige Code ähnelt der vorherigen Implementierung – mit dem Unterschied, dass wir jetzt einen try/catch-Fehler Behandlungs Block um unsere Arbeit umschließen. Wenn beim Aufrufen von updatemodel () eine Ausnahme auftritt oder wenn wir versuchen, das dinnerrepository zu speichern (wodurch eine Ausnahme ausgelöst wird, wenn das Dinner-Objekt, das wir zu speichern versuchen, aufgrund einer Regelverletzung in unserem Modell ungültig ist), wird der catch-Fehler Behandlungs Block verwendet. auszuführen. Darin werden alle Regel Verletzungen durchlaufen, die im Dinner-Objekt vorhanden sind, und Sie einem modelstate-Objekt hinzufügen (das wir in Kürze erörtern werden). Anschließend zeigen wir die Ansicht erneut an.

Um diese Arbeit zu sehen, führen Sie die Anwendung erneut aus, bearbeiten ein Dinner und ändern Sie so, dass Sie einen leeren Titel, ein eventdate "Bogus" und eine UK-Telefonnummer mit einem Länder Wert von "USA" verwenden. Wenn Sie auf die Schaltfläche "Speichern" klicken, kann die HTTP Post-Bearbeitungsmethode das Dinner nicht speichern (weil es Fehler gibt), und das Formular wird erneut angezeigt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Unsere Anwendung hat eine angemessene Fehlermeldung. Die Textelemente mit der ungültigen Eingabe werden rot hervorgehoben, und dem Endbenutzer werden Validierungs Fehlermeldungen angezeigt. Das Formular bewahrt auch die Eingabedaten, die der Benutzer ursprünglich eingegeben hat – so, dass Sie nichts auffüllen müssen.

Wie können Sie Fragen, was passiert ist? Wie haben sich die Textfelder "Title", "eventdate" und "contactphone" in rot hervorgehoben und wissen, dass die ursprünglich eingegebenen Benutzer Werte ausgegeben werden? Und wie werden Fehlermeldungen oben in der Liste angezeigt? Die gute Nachricht ist, dass dies nicht durch Magic erfolgt ist, sondern da wir einige der integrierten ASP.NET-MVC-Features verwendet haben, die die Eingabevalidierung und Fehler Behandlungs Szenarios erleichtern.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Grundlegendes zu modelstate und den HTML-Hilfsmethoden für die Validierung

Controller Klassen verfügen über eine "modelstate"-Eigenschaften Auflistung, die eine Möglichkeit bietet, anzugeben, dass Fehler bei einem Modell Objekt vorliegen, das an eine Ansicht übermittelt wird. Fehler Einträge in der modelstate-Auflistung identifizieren den Namen der Modell Eigenschaft mit dem Problem (z. b. "Title", "eventdate" oder "contactphone") und ermöglichen das Angeben einer benutzerfreundlichen Fehlermeldung (z. b. "Title ist erforderlich").

Die " *updatemodel ()* "-Hilfsmethode füllt automatisch die modelstate-Auflistung auf, wenn beim Versuch, den Eigenschaften des Modell Objekts Formular Werte zuzuweisen, Fehler auftreten. Beispielsweise ist die eventdate-Eigenschaft des Dinner-Objekts vom Typ "DateTime". Wenn die updatemodel ()-Methode den Zeichen folgen Wert "Bogus" im obigen Szenario nicht zuweisen konnte, fügte die updatemodel ()-Methode der modelstate-Auflistung einen Eintrag hinzu, der angibt, dass bei dieser Eigenschaft ein Zuweisungs Fehler aufgetreten ist.

Entwickler können auch Code schreiben, um Fehler Einträge explizit in der modelstate-Auflistung hinzuzufügen, wie im folgenden Beispiel in unserem "Catch"-Fehler Behandlungs Block, der die modelstate-Auflistung mit Einträgen füllt, die auf den aktiven Regelverstößen in der Dinner-Objekt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integration von HTML-Hilfsprogrammen in modelstate

HTML-Hilfsmethoden, z. b. html. TextBox ()-überprüfen Sie die modelstate-Auflistung beim Rendern der Ausgabe Wenn ein Fehler für das Element vorhanden ist, wird der vom Benutzer eingegebene Wert und eine CSS-Fehler Klasse dargestellt.

Beispielsweise verwenden wir in unserer Ansicht "Bearbeiten" die Hilfsmethode HTML. TextBox (), um das eventdate-Objekt des Dinner-Objekts zu Rendering:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Wenn die Sicht im Fehlerszenario gerendert wurde, hat die HTML. TextBox ()-Methode die modelstate-Auflistung geprüft, um festzustellen, ob Fehler mit der Eigenschaft "eventdate" des Dinner-Objekts aufgetreten sind. Wenn festgestellt wurde, dass ein Fehler aufgetreten ist, wurde die gesendete Benutzereingabe ("Bogus") als Wert gerendert, und der &lt;Eingabe Type = "TextBox"/&gt; Markup, das generiert wurde, wurde eine CSS-Fehler Klasse hinzugefügt:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Sie können die Darstellung der CSS-Fehler Klasse so anpassen, dass Sie nach Belieben aussehen soll. Die Standard-CSS-Fehler Klasse – "Input-Validation-Error" – wird im Stylesheet " *\content\site.CSS* " definiert und sieht wie folgt aus:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Diese CSS-Regel hat bewirkt, dass unsere ungültigen Eingabeelemente wie unten gezeigt hervorgehoben werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>HTML. validationmessage ()-Hilfsmethode

Die HTML. validationmessage ()-Hilfsmethode kann verwendet werden, um die modelstate-Fehlermeldung auszugeben, die einer bestimmten Modell Eigenschaft zugeordnet ist:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Der obige Code gibt Folgendes aus: *&lt;span class = "Field-Validation-Error"&gt; der Wert "Bogus" ist ungültig&lt;/Span&gt;*

Die Hilfsmethode HTML. validationmessage () unterstützt auch einen zweiten Parameter, mit dem Entwickler die angezeigte Fehlertext Meldung überschreiben können:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Der obige Code gibt Folgendes aus: *&lt;span class = "Field-Validation-Error"&gt;\*&lt;/Span&gt;* anstelle des Standard Fehlertexts, wenn ein Fehler für die eventdate-Eigenschaft vorliegt.

##### <a name="htmlvalidationsummary-helper-method"></a>HTML. ValidationSummary ()-Hilfsmethode

Die Hilfsmethode HTML. ValidationSummary () kann verwendet werden, um eine Zusammenfassungs Fehlermeldung zu erzeugen, begleitet von einer &lt;UL&gt;&lt;Li/&gt;&lt;/UL&gt; Liste aller ausführlichen Fehlermeldungen in der modelstate-Auflistung:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Die Hilfsmethode HTML. ValidationSummary () übernimmt einen optionalen Zeichen folgen Parameter – der eine Zusammenfassungs Fehlermeldung definiert, die über der Liste der detaillierten Fehler angezeigt wird:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Sie können optional CSS verwenden, um die Fehlerliste zu überschreiben.

#### <a name="using-a-addruleviolations-helper-method"></a>Verwenden einer addruleverletzungs-Hilfsmethode

Unsere anfängliche HTTP-Post-Bearbeitungs Implementierung verwendet eine foreach-Anweisung innerhalb des catch-Blocks, um die Regel Verletzungen des Dinner-Objekts zu durchlaufen und Sie der modelstate-Auflistung des Controllers hinzuzufügen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Wir können diesen Code etwas sauberer machen, indem wir dem Projekt "nerddinner" eine "controllerhelper"-Klasse hinzufügen und eine "addruleverletzungs"-Erweiterungsmethode implementieren, die der ASP.NET MVC modelanedictionary-Klasse eine Hilfsmethode hinzufügt. Diese Erweiterungsmethode kann die erforderliche Logik zum Auffüllen von modelstatuedictionary mit einer Liste von ruleverletzungs-Fehlern Kapseln:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Anschließend können wir unsere HTTP-Post Edit-Aktionsmethode aktualisieren, um diese Erweiterungsmethode zum Auffüllen der modelstate-Auflistung mit den Verstößen gegen die Dinner-Regel zu verwenden.

#### <a name="complete-edit-action-method-implementations"></a>Implementierungen der Edit-Aktionsmethode vervollständigen

Der folgende Code implementiert die gesamte Controller Logik, die für unser Bearbeitungs Szenario erforderlich ist:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Das schöne an unserer Edit-Implementierung ist, dass weder unsere Controller Klasse noch unsere Ansichts Vorlage etwas über die spezifische Validierung oder Geschäftsregeln wissen muss, die von unserem Dinner Model erzwungen werden. Wir können unserem Modell in Zukunft weitere Regeln hinzufügen und müssen keine Codeänderungen an unserem Controller oder in der Ansicht vornehmen, damit Sie unterstützt werden. Dies bietet uns die Flexibilität, unsere Anwendungsanforderungen in Zukunft mit mindestens einer Codeänderung problemlos zu entwickeln.

### <a name="create-support"></a>Unterstützung erstellen

Wir haben die Implementierung des Verhaltens "Edit" (Bearbeiten) der Klasse "dinnerscontroller" abgeschlossen. Wir fahren jetzt mit der Implementierung der "Create"-Unterstützung für Sie fort – dadurch können Benutzer neue Abendessen hinzufügen.

#### <a name="the-http-get-create-action-method"></a>Die HTTP-Get Create Action-Methode

Wir beginnen damit, das http "Get"-Verhalten der Create Action-Methode zu implementieren. Diese Methode wird aufgerufen, wenn jemand die */Dinners/Create* -URL besucht. Unsere Implementierung sieht wie folgt aus:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Der obige Code erstellt ein neues Dinner-Objekt und weist seine eventdate-Eigenschaft einer Woche in der Zukunft zu. Anschließend wird eine Ansicht gerendert, die auf dem neuen Dinner-Objekt basiert. Da wir nicht explizit einen Namen an die *View ()* -Hilfsmethode übermittelt haben, wird der auf der Konvention basierende Standardpfad verwendet, um die Ansichts Vorlage aufzulösen:/views/Dinners/Create.aspx.

Nun erstellen wir diese Ansichts Vorlage. Klicken Sie hierzu mit der rechten Maustaste auf die Create Action-Methode, und wählen Sie den Kontextmenü Befehl "Ansicht hinzufügen" aus. Im Dialogfeld "Ansicht hinzufügen" geben wir an, dass wir ein Dinner-Objekt an die Ansichts Vorlage übergeben, und wählen das automatische Gerüstbau einer "Create"-Vorlage aus:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Wenn Sie auf die Schaltfläche "Add" (hinzufügen) klicken, speichert Visual Studio eine neue Gerüst basierte "Create. aspx"-Ansicht im Verzeichnis "\views\dinner" und öffnet Sie in der IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Nehmen wir einige Änderungen an der standardmäßigen "erstellen"-Gerüst Datei vor, die für uns generiert wurde, und ändern Sie Sie so, dass Sie wie folgt aussieht:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Wenn wir nun die Anwendung ausführen und auf die URL *"/Dinners/Create"* im Browser zugreifen, wird die Benutzeroberfläche wie unten aus unserer Create Action-Implementierung gerenkt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementieren der HTTP-Post-Aktionsmethode "Create"

Die HTTP-Get-Version unserer Create Action-Methode wurde implementiert. Wenn ein Benutzer auf die Schaltfläche "Speichern" klickt, führt er eine Formular Bereitstellung an der */Dinners/Create* -URL aus und übermittelt die HTML-&lt;Eingabe&gt; Formular Werte mithilfe des HTTP POST-Verbs.

Nun implementieren wir das HTTP-Post-Verhalten unserer Create Action-Methode. Wir beginnen mit dem Hinzufügen einer überladenen "Create"-Aktionsmethode zu unserem dinnerscontroller, der über ein "Accept-Verbs"-Attribut verfügt, das angibt, dass es HTTP Post-Szenarien behandelt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Es gibt eine Vielzahl von Möglichkeiten, auf die bereitgestellte Formular Parameter innerhalb unserer HTTP-Post-fähigen "Create"-Methode zuzugreifen.

Ein Ansatz besteht darin, ein neues Dinner-Objekt zu erstellen und dann die Methode " *updatemodel ()* " (wie bei der Aktion "Bearbeiten") zu verwenden, um es mit den veröffentlichten Formular Werten zu füllen. Anschließend können wir ihn unserem dinnerrepository hinzufügen, ihn in der Datenbank speichern und den Benutzer zu unserer Detail Aktion umleiten, um das neu erstellte Dinner mit dem folgenden Code anzuzeigen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternativ können wir einen Ansatz verwenden, bei dem unsere Create ()-Aktionsmethode ein Dinner-Objekt als Methoden Parameter annimmt. ASP.NET MVC instanziiert dann automatisch ein neues Dinner-Objekt für uns, füllt seine Eigenschaften mithilfe der Formular Eingaben auf und übergibt es an unsere Aktionsmethode:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Mit der obigen Aktionsmethode wird überprüft, ob das Dinner-Objekt durch Überprüfen der modelstate. IsValid-Eigenschaft erfolgreich mit den Formular Post-Werten aufgefüllt wurde. Dadurch wird "false" zurückgegeben, wenn Eingabe Konvertierungsprobleme vorliegen (z. b. eine Zeichenfolge "Bogus" für die eventdate-Eigenschaft). Wenn Probleme vorliegen, zeigt die Aktionsmethode das Formular erneut an.

Wenn die Eingabewerte gültig sind, versucht die Aktionsmethode, das neue Dinner im dinnerrepository hinzuzufügen und zu speichern. Diese Aufgabe wird in einem try/catch-Block umschlossen und das Formular erneut angezeigt, wenn Verstöße gegen Geschäftsregeln vorliegen (was dazu führen würde, dass die Methode "dinnerrepository. Save ()" eine Ausnahme auslöst).

Um dieses Fehler Behandlungs Verhalten in Aktion zu sehen, können wir die */Dinners/Create* -URL anfordern und Details zu einem neuen Dinner ausfüllen. Falsche Eingaben oder Werte bewirken, dass das Formular erstellen mit den unten markierten Fehlern erneut angezeigt wird:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Beachten Sie, dass unser Formular erstellen genau dieselbe Validierung und dieselben Geschäftsregeln wie das Bearbeitungs Formular berücksichtigt. Dies liegt daran, dass unsere Validierungs-und Geschäftsregeln im Modell definiert und nicht in die Benutzeroberfläche oder den Controller der Anwendung eingebettet wurden. Dies bedeutet, dass wir unsere Validierungs-oder Geschäftsregeln später an einem zentralen Ort ändern/weiterentwickeln können und dass Sie in der gesamten Anwendung angewendet werden können. Es ist nicht erforderlich, Code in den Bearbeitungs-oder Aktionsmethoden zu ändern, damit neue Regeln oder Änderungen an vorhandenen nicht automatisch beachtet werden.

Wenn wir die Eingabewerte korrigieren und erneut auf die Schaltfläche "Save" (speichern) klicken, wird das Hinzufügen des dinnerrepository erfolgreich abgeschlossen, und ein neues Dinner wird der Datenbank hinzugefügt. Wir werden dann an die URL */Dinners/Details/[ID]* umgeleitet –, wo Details zum neu erstellten Dinner angezeigt werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>DELETE-Unterstützung

Nun fügen wir unserem dinnerscontroller "Delete"-Unterstützung hinzu.

#### <a name="the-http-get-delete-action-method"></a>Die HTTP-Get DELETE-Aktionsmethode

Wir beginnen mit der Implementierung des HTTP-Get-Verhaltens der DELETE-Aktionsmethode. Diese Methode wird aufgerufen, wenn jemand die */Dinners/delete/[ID]* -URL besucht. Im folgenden finden Sie die-Implementierung:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Die Aktionsmethode versucht, das Dinner abzurufen, das gelöscht werden soll. Wenn das Dinner vorhanden ist, rendert es eine Ansicht basierend auf dem Dinner-Objekt. Wenn das Objekt nicht vorhanden ist (oder bereits gelöscht wurde), wird eine Ansicht zurückgegeben, die die "NotFound"-Ansichts Vorlage rendert, die wir zuvor für unsere "Details"-Aktionsmethode erstellt haben.

Wir können die Vorlage "Delete" (Löschen) erstellen, indem Sie mit der rechten Maustaste auf die Delete-Aktionsmethode klicken und den Kontextmenü Befehl "Ansicht hinzufügen" auswählen. Im Dialogfeld "Ansicht hinzufügen" wird angegeben, dass wir ein Dinner-Objekt an unsere Ansichts Vorlage als Modell übergeben und eine leere Vorlage erstellen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, fügt Visual Studio im Verzeichnis "\views\dinner" eine neue "Delete. aspx"-Ansichts Vorlagen Datei für uns hinzu. Wir fügen der Vorlage HTML und Code hinzu, um einen Bestätigungsbildschirm zum Löschen wie unten beschrieben zu implementieren:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Der obige Code zeigt den Titel des zu löschenden Abend Buchs an und gibt ein &lt;Formular&gt; Elements aus, das einen Beitrag an die/Dinners/delete/[ID]-URL sendet, wenn der Endbenutzer auf die Schaltfläche "Löschen" klickt.

Wenn wir unsere Anwendung ausführen und auf die URL *"/Dinners/delete/[ID]"* für ein gültiges Dinner-Objekt zugreifen, wird die Benutzeroberfläche wie folgt gerendert:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Seitiges Thema: Warum wird ein Beitrag ausgeführt?** |
| --- |
| Vielleicht Fragen Sie sich – warum haben wir den Aufwand für das Erstellen einer &lt;Form&gt; auf dem Bestätigungsbildschirm löschen durchlaufen? Warum sollten Sie nicht einfach einen Standard Hyperlink verwenden, um eine Verknüpfung zu einer Aktionsmethode zu verwenden, die den eigentlichen Löschvorgang ausführt? Der Grund hierfür ist, dass wir sorgfältig vor Webcrawlern und Suchmaschinen suchen möchten, die unsere URLs ermitteln und versehentlich bewirken, dass Daten gelöscht werden, wenn Sie den Links folgen. HTTP-Get-basierte URLs werden als "sicher" angesehen, damit Sie auf Sie zugreifen bzw. durchforsten werden können, und Sie sollten nicht auf HTTP-Post-postzugriffe folgen. Eine gute Regel besteht darin sicherzustellen, dass Sie immer destruktive oder Daten ändernde Vorgänge hinter HTTP-POST-Anforderungen ablegen. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementieren der HTTP-Post-Aktionsmethode "Delete"

Nun wird die HTTP-Get-Version der DELETE-Aktionsmethode implementiert, die einen Bestätigungsbildschirm zum Löschen anzeigt. Wenn ein Endbenutzer auf die Schaltfläche "Löschen" klickt, wird eine Formular Bereitstellung für die URL */Dinners/Dinner/[ID]* durchgeführt.

Nun implementieren wir das http-"Post"-Verhalten der DELETE-Aktionsmethode mithilfe des folgenden Codes:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Die HTTP-Post-Version der DELETE-Aktionsmethode versucht, das Dinner-Objekt abzurufen, das gelöscht werden soll. Wenn Sie es nicht finden kann (da es bereits gelöscht wurde), wird die Vorlage "NotFound" gerendert. Wenn das Dinner gefunden wird, wird es aus dem dinnerrepository gelöscht. Anschließend wird eine "Deleted"-Vorlage gerendert.

Um die "gelöschte" Vorlage zu implementieren, klicken Sie mit der rechten Maustaste auf die Aktionsmethode, und wählen Sie das Kontextmenü "Ansicht hinzufügen" aus. Wir benennen die Ansicht "gelöscht" und haben eine leere Vorlage (und kein stark typisiertes Modell Objekt). Anschließend fügen wir einige HTML-Inhalte hinzu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Und wenn wir nun die Anwendung ausführen und auf die URL *"/Dinners/delete/[ID]"* für ein gültiges Dinner-Objekt zugreifen, wird der Bestätigungsbildschirm von Dinner DELETE wie unten dargestellt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Wenn Sie auf die Schaltfläche "Löschen" klicken, wird eine HTTP-Post-URL für die */Dinners/delete/[ID]* -URL durchgeführt, mit der das Dinner aus der Datenbank gelöscht wird und die "gelöschte" Ansichts Vorlage angezeigt wird:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Modell Bindungs Sicherheit

Wir haben zwei verschiedene Möglichkeiten erläutert, die integrierten Modell Bindungs Features von ASP.NET MVC zu verwenden. Der erste mit der updatemodel ()-Methode, um Eigenschaften für ein vorhandenes Modell Objekt zu aktualisieren, und der zweite, der die ASP.NET MVC-Unterstützung für das Übergeben von Modell Objekten in als Aktionsmethoden Parameter verwendet. Beide Techniken sind sehr leistungsstark und äußerst nützlich.

Diese Leistung sorgt auch für die IT-Verantwortung. Es ist wichtig, bei der Annahme von Benutzereingaben stets an der Sicherheit teilnehmen zu müssen. Dies gilt auch, wenn Objekte an die Formulareingabe gebunden werden. Sie sollten darauf achten, alle vom Benutzer eingegebenen Werte immer in HTML zu codieren, um HTML-und JavaScript Injection-Angriffe zu vermeiden. Sie sollten auch auf SQL Injection-Angriffe achten (Beachten Sie, dass wir LINQ to SQL für die Anwendung verwenden, die Parameter automatisch codiert, um dies zu verhindern. Arten von Angriffen). Sie sollten sich nie auf die Client seitige Validierung verlassen und stets die serverseitige Validierung verwenden, um sich gegen Hacker zu schützen, die versuchen, ihnen falsche Werte zu senden.

Ein zusätzliches Sicherheitselement, das Sie bei der Verwendung der Bindungs Features von ASP.NET MVC berücksichtigen sollten, ist der Bereich der Objekte, die Sie binden. Insbesondere sollten Sie sicherstellen, dass Sie die Sicherheitsauswirkungen der Eigenschaften verstehen, die Sie binden dürfen, und sicherstellen, dass Sie nur die Eigenschaften zulassen, die von einem zu aktualisierenden Endbenutzer aktualisiert werden können.

Standardmäßig versucht die updatemodel ()-Methode, alle Eigenschaften für das Modell Objekt zu aktualisieren, die den eingehenden Formular Parameterwerten entsprechen. Ebenso können als Aktionsmethoden Parameter standardmäßig alle-Objekte über Formular Parameter festgelegt werden.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Sperren der Bindung pro Nutzung

Sie können die Bindungs Richtlinie auf Nutzungs Basis sperren, indem Sie eine explizite "includeliste" der Eigenschaften angeben, die aktualisiert werden können. Dies kann durch Übergeben eines zusätzlichen Zeichen folgen Array-Parameters an die updatemodel ()-Methode wie unten dargestellt erfolgen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objekte, die als Aktionsmethoden Parameter weitergegeben werden, unterstützen auch ein [BIND]-Attribut, das eine "Include-Liste" zulässiger Eigenschaften wie unten beschrieben ermöglicht:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Sperren der Bindung auf typbasis

Sie können die Bindungs Regeln auch auf pro-Typ-Basis sperren. Auf diese Weise können Sie die Bindungs Regeln einmalig angeben und Sie dann in allen Szenarien (einschließlich updatemodel und Aktionsmethoden Parameter Szenarios) über alle Controller und Aktionsmethoden hinweg anwenden lassen.

Sie können die Bindungs Regeln pro Typ anpassen, indem Sie ein [BIND]-Attribut zu einem Typ hinzufügen, oder indem Sie es in der Datei "Global. asax" der Anwendung registrieren (nützlich für Szenarien, in denen Sie den Typ nicht besitzen). Anschließend können Sie mithilfe der include-und Exclude-Eigenschaften des Bindungs Attributs steuern, welche Eigenschaften für die bestimmte Klasse oder Schnittstelle gebunden werden können.

Wir verwenden diese Technik für die Dinner-Klasse in unserer "nerddinner"-Anwendung und fügen ihr ein [BIND]-Attribut hinzu, das die Liste der bindbaren Eigenschaften auf Folgendes einschränkt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Beachten Sie, dass wir nicht zulassen, dass die RSVPs-Sammlung über eine Bindung manipuliert wird, und dass die Eigenschaften "dinnerid" oder "hustedby" nicht über eine Bindung festgelegt werden. Aus Sicherheitsgründen werden diese speziellen Eigenschaften nur mithilfe von expliziten Code innerhalb unserer Aktionsmethoden geändert.

### <a name="crud-wrap-up"></a>CRUD-Wrap-up

ASP.NET MVC umfasst eine Reihe integrierter Features, die beim Implementieren von Formular Bereitstellungsszenarios hilfreich sind. Wir haben eine Vielzahl dieser Features verwendet, um die CRUD-Benutzeroberflächen Unterstützung zusätzlich zu unserem dinnerrepository bereitzustellen.

Wir verwenden einen modellorientierten Ansatz, um unsere Anwendung zu implementieren. Dies bedeutet, dass alle unsere Validierungs-und Geschäftsregel Logik innerhalb unserer Modell Ebene – und nicht innerhalb unserer Controller oder Ansichten definiert ist. Weder unsere Controller Klasse noch unsere Ansichts Vorlagen wissen über die spezifischen Geschäftsregeln, die von der Dinner Model-Klasse erzwungen werden.

Dadurch wird die Anwendungsarchitektur bereinigt, und Sie können Sie leichter testen. Wir können unserer Modell Ebene in Zukunft weitere Geschäftsregeln hinzufügen und *müssen keine Codeänderungen* an unserem Controller oder in der Ansicht vornehmen, damit Sie unterstützt werden. Dadurch wird eine große Menge an Agilität bereitgestellt, um die Anwendung in Zukunft zu entwickeln und zu ändern.

Der dinnerscontroller ermöglicht jetzt Dinner-Auflistungen und-Details sowie Unterstützung für das Erstellen, bearbeiten und löschen. Den gesamten Code für die-Klasse finden Sie unten:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Nächster Schritt

Wir verfügen jetzt über die grundlegende CRUD-Unterstützung (Create, Read, Update und DELETE) in unserer dinnerscontroller-Klasse.

Sehen wir uns nun an, wie wir die Klassen "ViewData" und "ViewModel" verwenden können, um eine umfassendere Benutzeroberfläche in unseren Formularen zu ermöglichen.

> [!div class="step-by-step"]
> [Zurück](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Weiter](use-viewdata-and-implement-viewmodel-classes.md)
