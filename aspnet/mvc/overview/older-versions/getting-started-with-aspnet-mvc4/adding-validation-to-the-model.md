---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Hinzufügen der Validierung zum Modell | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: c9f6699c5d3500d4c1fcade9252aeb9dd92983da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498465"
---
# <a name="adding-validation-to-the-model"></a>Hinzufügen der Überprüfung zum Modell

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.

In diesem Abschnitt fügen Sie dem `Movie` Modell Validierungs Logik hinzu, und Sie stellen sicher, dass die Validierungsregeln immer dann erzwungen werden, wenn ein Benutzer versucht, einen Film mit der Anwendung zu erstellen oder zu bearbeiten.

## <a name="keeping-things-dry"></a>Halten Sie die Dinge trocken

Eines der Kern Entwurfs-Grundsätze von ASP.NET MVC ist "Dry" (&quot;Don't repeat yourself&quot;). ASP.NET MVC fordert Sie auf, die Funktionalität oder das Verhalten nur einmal anzugeben, und kann es dann überall in einer Anwendung widerspiegeln. Dadurch wird der Code, den Sie schreiben müssen, reduziert, und der Code, den Sie schreiben, ist weniger fehleranfällig und leichter zu verwalten.

Die von ASP.NET MVC und Entity Framework Code First bereitgestellte Validierungs Unterstützung ist ein gutes Beispiel für das trockene Prinzip in Aktion. Sie können Validierungsregeln deklarativ an einem Ort angeben (in der Modell Klasse), und die Regeln werden überall in der Anwendung erzwungen.

Sehen wir uns nun an, wie Sie diese Validierungs Unterstützung in der Movie-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln zum Movie-Modell

Fügen Sie zunächst der `Movie`-Klasse eine gewisse Validierungs Logik hinzu.

Öffnen Sie Datei *Movie.cs*. Fügen Sie am Anfang der Datei eine `using`-Anweisung hinzu, die auf den [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace verweist:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Beachten Sie, dass der Namespace keine `System.Web`enthält. DataAnnotations bietet eine integrierte Gruppe von Validierungs Attributen, die Sie deklarativ auf jede Klasse oder Eigenschaft anwenden können.

Aktualisieren Sie nun die `Movie`-Klasse, um die integrierten Attribute [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)und [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierung zu nutzen. Verwenden Sie den folgenden Code als Beispiel dafür, wo die Attribute angewendet werden sollen.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Führen Sie die Anwendung aus, und Sie erhalten erneut den folgenden Laufzeitfehler:

***Das Modell, das den Kontext "Kontext" unterstützt, hat sich seit der Erstellung der Datenbank geändert. Verwenden Sie Code First-Migrationen, um die Datenbank zu aktualisieren ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Wir werden Migrationen verwenden, um das Schema zu aktualisieren. Erstellen Sie die Projekt Mappe, öffnen Sie das Konsolenfenster des **Paket-Managers** , und geben Sie die folgenden Befehle ein:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration` abgeleitete Klasse mit dem angegebenen Namen (*adddataannotationsmig*) definiert, und in der `Up`-Methode sehen Sie den Code, durch den die Schema Einschränkungen aktualisiert werden. Die Felder `Title` und `Genre` können nicht mehr auf NULL festgelegt werden (d. h., Sie müssen einen Wert eingeben), und das Feld `Rating` hat eine maximale Länge von 5.

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Das `Required`-Attribut gibt an, dass eine Eigenschaft einen Wert aufweisen muss. in diesem Beispiel muss ein Film Werte für die Eigenschaften `Title`, `ReleaseDate`, `Genre`und `Price` aufweisen, damit Sie gültig sind. Das Attribut `Range` schränkt einen Wert auf einen bestimmten Bereich ein. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen. Systeminterne Typen (z. b. `decimal, int, float, DateTime`) sind standardmäßig erforderlich und benötigen das `Required`-Attribut nicht.

Code First stellt sicher, dass die Validierungsregeln, die Sie für eine Modell Klasse angeben, erzwungen werden, bevor die Anwendung Änderungen in der Datenbank speichert. Beispielsweise löst der folgende Code eine Ausnahme aus, wenn die `SaveChanges`-Methode aufgerufen wird, da mehrere erforderliche `Movie` Eigenschaftswerte fehlen und der Preis NULL (außerhalb des gültigen Bereichs) liegt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Das automatische Erzwingen von Validierungsregeln durch .NET Framework trägt dazu bei, die Anwendung stabiler zu machen. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

Im folgenden finden Sie eine komplette Code Auflistung für die aktualisierte Datei *Movie.cs* :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Validierungs Fehler-Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung erneut aus, und navigieren Sie zur URL */Movies* .

Klicken Sie auf den Link **neu erstellen** , um einen neuen Film hinzuzufügen. Füllen Sie das Formular mit einigen ungültigen Werten aus, und klicken Sie dann auf die Schaltfläche **Erstellen** .

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> um die jQuery-Validierung für nicht englische Gebiets Schemas zu unterstützen, die ein Komma (&quot;&quot;) für einen Dezimaltrennzeichen verwenden, müssen Sie *Globalize. js* und ihre spezifische *Kulturen/Globalize. Cultures. js* -Datei (von [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) und JavaScript einschließen, um `Globalize.parseFloat`zu verwenden. Der folgende Code zeigt die Änderungen an der Datei "views\movies\edit.cshtml", um mit der &quot;fr-FR-&quot; Kultur zu arbeiten:

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Beachten Sie, dass das Formular automatisch eine rote Rahmenfarbe verwendet hat, um die Textfelder hervorzuheben, die ungültige Daten enthalten, und eine entsprechende Validierungs Fehlermeldung nebeneinander ausgegeben hat. Die Fehlermeldungen werden sowohl auf Clientseite (mithilfe von JavaScript und jQuery) als auch auf Serverseite erzwungen (wenn ein Benutzer JavaScript deaktiviert hat).

Ein echter Vorteil besteht darin, dass Sie eine einzige Codezeile in der `MoviesController`-Klasse oder in der *Create. cshtml* -Sicht nicht ändern müssen, um diese Validierungs Benutzeroberfläche zu aktivieren. Die Controller und Ansichten, die Sie zuvor in diesem Tutorial erstellt haben, haben die angegebenen Validierungsregeln automatisch übernommen (mithilfe der Validierungsattribute für die Eigenschaften der Modellklasse `Movie`).

Möglicherweise haben Sie für die Eigenschaften `Title` und `Genre`bemerkt, das erforderliche Attribut wird erst erzwungen, wenn Sie das Formular übermitteln (Klicken Sie auf die Schaltfläche **Erstellen** ), oder geben Sie Text in das Eingabefeld ein, und entfernen Sie es. Für ein Feld, das anfänglich leer ist (z. b. die Felder in der CREATE VIEW) und das nur über das erforderliche Attribut und keine anderen Validierungs Attribute verfügt, können Sie Folgendes ausführen, um die Validierung zu initiieren:

1. In das Feld.
2. Geben Sie Text ein.
3. Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld.
4. Tab zurück in das Feld.
5. Entfernen Sie den Text.
6. Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld.

Die obige Sequenz löst die erforderliche Validierung aus, ohne auf die Schaltfläche "Senden" zu klicken. Wenn Sie einfach auf die Schaltfläche "Senden" klicken, ohne die Felder einzugeben, wird die Client seitige Validierung gestartet. Die Formulardaten werden erst an den Server gesendet, wenn auf Clientseite keine Validierungsfehler mehr auftreten. Sie können dies testen, indem Sie in der HTTP-Post-Methode einen Haltepunkt oder das [Tool "Tools](http://fiddler2.com/fiddler2/) " oder die IE 9 [F12-Entwicklertools](https://msdn.microsoft.com/ie/aa740478)verwenden.

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>So erfolgt die Überprüfung in der CREATE VIEW-und Create Action-Methode

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. In der nächsten Liste wird gezeigt, wie die `Create` Methoden in der `MovieController`-Klasse aussehen. Sie sind unverändert, wie Sie Sie zuvor in diesem Tutorial erstellt haben.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Die erste `Create`-Aktionsmethode (HTTP GET) zeigt das erste Formular „Create“ an. Die zweite Version (`[HttpPost]`) verarbeitet die Formularbereitstellung. Die zweite `Create`-Methode (die `HttpPost`-Version) ruft `ModelState.IsValid` auf, um zu überprüfen, ob der Film Validierungsfehler aufweist. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungsfehler enthält, zeigt die `Create`-Methode das Formular erneut an. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank. In unserem Movie-Beispiel **wird das Formular nicht an den Server gesendet, wenn Validierungs Fehler auf der Clientseite erkannt werden. die zweite** `Create`**Methode wird nie aufgerufen**. Wenn Sie JavaScript in Ihrem Browser deaktivieren, wird die Client Validierung deaktiviert, und die HTTP Post-`Create`-Methode ruft `ModelState.IsValid` auf, um zu überprüfen, ob für den Film Validierungs Fehler vorliegen.

Sie können einen Haltepunkt in der `HttpPost Create`-Methode festlegen und überprüfen, ob die Methode tatsächlich niemals aufgerufen wird und die clientseitige Validierung die Formulardaten nicht sendet, wenn Validierungsfehler gefunden werden. Wenn Sie JavaScript in Ihrem Browser deaktivieren und dann das Formular mit Fehlern senden, wird der Haltepunkt erreicht. Sie erhalten auch ohne JavaScript weiterhin eine vollständige Validierung. In der folgenden Abbildung wird gezeigt, wie Sie JavaScript in Internet Explorer deaktivieren.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Die folgende Abbildung zeigt, wie JavaScript im Firefox-Browser deaktiviert wird.

![](adding-validation-to-the-model/_static/image5.png)

In der folgenden Abbildung wird gezeigt, wie Sie JavaScript mit dem Chrome-Browser deaktivieren.

![](adding-validation-to-the-model/_static/image6.png)

Im folgenden finden Sie die *Ansichts Vorlage Create. cshtml* , die Sie zuvor in diesem Tutorial erstellt haben. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Beachten Sie, dass der Code ein `Html.EditorFor`-Hilfsprogramm verwendet, um das `<input>`-Element für jede `Movie`-Eigenschaft auszugeben. Neben diesem Hilfsprogramm ist ein aufzurufende `Html.ValidationMessageFor` Hilfsmethode. Diese beiden Hilfsmethoden arbeiten mit dem Modell Objekt, das vom Controller an die Ansicht (in diesem Fall ein `Movie` Objekt) übermittelt wird. Sie suchen automatisch nach Validierungs Attributen, die im Modell angegeben sind, und zeigen Fehlermeldungen nach Bedarf an.

Der Vorteil dieses Ansatzes ist, dass weder der Controller noch die Vorlage "Create View" etwas über die tatsächlichen Validierungsregeln informiert, die erzwungen werden, oder über die angezeigten Fehlermeldungen. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben. Diese Validierungsregeln werden automatisch auf die Bearbeitungs Ansicht und alle anderen Ansichten Vorlagen angewendet, die Sie erstellen können, um das Modell zu bearbeiten.

Wenn Sie die Validierungs Logik zu einem späteren Zeitpunkt ändern möchten, können Sie dies an genau einem Ort tun, indem Sie dem Modell Validierungs Attribute hinzufügen (in diesem Beispiel die `movie`-Klasse). Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Und dies bedeutet, dass Sie das DRY-Prinzip vollständig einhalten.

## <a name="adding-formatting-to-the-movie-model"></a>Hinzufügen von Formatierung zum Movie-Modell

Öffnen Sie die Datei *Movie.cs*, und überprüfen Sie die Klasse `Movie`. Der [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt zusätzlich zum integrierten Satz von Validierungs Attributen Formatierungs Attribute bereit. Wir haben bereits einen [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Enumerationswert auf das Veröffentlichungsdatum und die Preis Felder angewendet. Der folgende Code zeigt die Eigenschaften `ReleaseDate` und `Price` mit dem entsprechenden [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut an.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Die [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribute sind keine Validierungs Attribute, Sie werden verwendet, um der Ansichts-Engine mitzuteilen, wie der HTML-Code dargestellt werden soll. Im obigen Beispiel zeigt das `DataType.Date`-Attribut die Film Datumsangaben nur als Datumsangaben ohne Zeit an. Die folgenden [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribute überprüfen z. b. nicht das Format der Daten:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Die oben aufgeführten Attribute enthalten lediglich Hinweise für die Ansichts-Engine zum Formatieren der Daten (und zum Bereitstellen von Attributen wie &lt;einer&gt; für URLs und &lt;a href =&quot;mailto:EmailAddress. com&quot;&gt; e-Mail. Sie können das [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) -Attribut verwenden, um das Format der Daten zu validieren.

Ein alternativer Ansatz für die Verwendung der `DataType` Attribute ist, dass Sie explizit einen [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) Wert festlegen können. Der folgende Code zeigt die Eigenschaft "Release Date" mit einer Datumsformat Zeichenfolge (d. & # &quot;d&quot;). Mit dieser Option geben Sie an, dass Sie nicht als Teil des Veröffentlichungsdatums Zeit verwenden möchten.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Die gesamte `Movie`-Klasse ist unten dargestellt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Führen Sie die Anwendung aus, und navigieren Sie zum `Movies` Controller. Das Veröffentlichungsdatum und der Preis sind gut formatiert. Die Abbildung unten zeigt das Veröffentlichungsdatum und den Preis mithilfe &quot;fr-FR-&quot; als Kultur an.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Die Abbildung unten zeigt die gleichen Daten, die in der Standard Kultur (Englisch (USA)) angezeigt werden.

![](adding-validation-to-the-model/_static/image8.png)

Im nächsten Teil der Reihe überprüfen wir die Anwendung und nehmen einige Verbesserungen an den automatisch generierten Methoden `Details` und `Delete` vor.

> [!div class="step-by-step"]
> [Zurück](adding-a-new-field-to-the-movie-model-and-table.md)
> [Weiter](examining-the-details-and-delete-methods.md)
