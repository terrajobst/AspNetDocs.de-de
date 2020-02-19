---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Hinzufügen der Validierung zum ModellC#() | Microsoft-Dokumentation
author: Rick-Anderson
description: Erstellen eines Controllers
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 19d86dc0df931a9d135e46209559892b77626cf6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457718"
---
# <a name="adding-validation-to-the-model-c"></a>Hinzufügen von Überprüfung zum Modell (C#)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.
> 
> 
> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Für dieses Thema steht ein Visual C# Web Developer-Projekt mit Quellcode zur Verfügung. [Laden Sie C# die Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zur [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Tutorials.

In diesem Abschnitt fügen Sie dem `Movie` Modell Validierungs Logik hinzu, und Sie stellen sicher, dass die Validierungsregeln immer dann erzwungen werden, wenn ein Benutzer versucht, einen Film mit der Anwendung zu erstellen oder zu bearbeiten.

## <a name="keeping-things-dry"></a>Halten Sie die Dinge trocken

Eines der Kern Entwurfs-Grundsätze von ASP.NET MVC ist "Dry" ("Don't repeat yourself"). ASP.NET MVC fordert Sie auf, die Funktionalität oder das Verhalten nur einmal anzugeben, und kann es dann überall in einer Anwendung widerspiegeln. Dies reduziert die Menge an Code, den Sie schreiben müssen, und macht den Code, den Sie schreiben, viel einfacher zu verwalten.

Die von ASP.NET MVC und Entity Framework Code First bereitgestellte Validierungs Unterstützung ist ein gutes Beispiel für das trockene Prinzip in Aktion. Sie können Validierungsregeln deklarativ an einem Ort angeben (in der Modell Klasse), und dann werden diese Regeln überall in der Anwendung erzwungen.

Sehen wir uns nun an, wie Sie diese Validierungs Unterstützung in der Movie-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln zum Movie-Modell

Fügen Sie zunächst der `Movie`-Klasse eine gewisse Validierungs Logik hinzu.

Öffnen Sie Datei *Movie.cs*. Fügen Sie am Anfang der Datei eine `using`-Anweisung hinzu, die auf den [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace verweist:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Der-Namespace ist Teil des .NET Framework. Er bietet einen integrierten Satz von Validierungs Attributen, die Sie deklarativ auf jede Klasse oder Eigenschaft anwenden können.

Aktualisieren Sie nun die `Movie`-Klasse, um die integrierten Attribute [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)und [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierung zu nutzen. Verwenden Sie den folgenden Code als Beispiel dafür, wo die Attribute angewendet werden sollen.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Das `Required`-Attribut gibt an, dass eine Eigenschaft einen Wert aufweisen muss. in diesem Beispiel muss ein Film Werte für die Eigenschaften `Title`, `ReleaseDate`, `Genre`und `Price` aufweisen, damit Sie gültig sind. Das Attribut `Range` schränkt einen Wert auf einen bestimmten Bereich ein. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen.

Code First stellt sicher, dass die Validierungsregeln, die Sie für eine Modell Klasse angeben, erzwungen werden, bevor die Anwendung Änderungen in der Datenbank speichert. Beispielsweise löst der folgende Code eine Ausnahme aus, wenn die `SaveChanges`-Methode aufgerufen wird, da mehrere erforderliche `Movie` Eigenschaftswerte fehlen und der Preis NULL (außerhalb des gültigen Bereichs) liegt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Das automatische Erzwingen von Validierungsregeln durch .NET Framework trägt dazu bei, die Anwendung stabiler zu machen. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

Im folgenden finden Sie eine komplette Code Auflistung für die aktualisierte Datei *Movie.cs* :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Validierungs Fehler-Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung erneut aus, und navigieren Sie zur URL */Movies* .

Klicken Sie auf den Link **Film erstellen** , um einen neuen Film hinzuzufügen. Füllen Sie das Formular mit einigen ungültigen Werten aus, und klicken Sie dann auf die Schaltfläche **Erstellen** .

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Beachten Sie, dass das Formular automatisch eine Hintergrundfarbe verwendet hat, um die Textfelder hervorzuheben, die ungültige Daten enthalten, und eine entsprechende Validierungs Fehlermeldung nebeneinander ausgegeben hat. Die Fehlermeldungen entsprechen den Fehler Zeichenfolgen, die Sie angegeben haben, als Sie die `Movie` Klasse kommentiert haben. Die Fehler werden sowohl auf Clientseite (mithilfe von JavaScript) als auch auf Serverseite erzwungen (für den Fall, dass ein Benutzer JavaScript deaktiviert hat).

Ein echter Vorteil besteht darin, dass Sie eine einzige Codezeile in der `MoviesController`-Klasse oder in der *Create. cshtml* -Sicht nicht ändern müssen, um diese Validierungs Benutzeroberfläche zu aktivieren. Der Controller und die Ansichten, die Sie zuvor in diesem Tutorial erstellt haben, haben automatisch die Validierungsregeln übernommen, die Sie mithilfe von Attributen für die `Movie` Modell Klasse angegeben haben.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>So erfolgt die Überprüfung in der CREATE VIEW-und Create Action-Methode

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. In der nächsten Liste wird gezeigt, wie die `Create` Methoden in der `MovieController`-Klasse aussehen. Sie sind unverändert, wie Sie Sie zuvor in diesem Tutorial erstellt haben.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Die erste Aktionsmethode zeigt das erste Create-Formular an. Die zweite verarbeitet den Formular Beitrag. Mit der zweiten `Create`-Methode wird `ModelState.IsValid` aufgerufen, um zu überprüfen, ob für den Film Validierungs Fehler vorliegen. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungs Fehler aufweist, wird das Formular von der `Create`-Methode erneut angezeigt. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank.

Im folgenden finden Sie die *Ansichts Vorlage Create. cshtml* , die Sie zuvor in diesem Tutorial erstellt haben. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Beachten Sie, dass der Code ein `Html.EditorFor`-Hilfsprogramm verwendet, um das `<input>`-Element für jede `Movie`-Eigenschaft auszugeben. Neben diesem Hilfsprogramm ist ein aufzurufende `Html.ValidationMessageFor` Hilfsmethode. Diese beiden Hilfsmethoden arbeiten mit dem Modell Objekt, das vom Controller an die Ansicht (in diesem Fall ein `Movie` Objekt) übermittelt wird. Sie suchen automatisch nach Validierungs Attributen, die im Modell angegeben sind, und zeigen Fehlermeldungen nach Bedarf an.

Der Vorteil dieses Ansatzes ist, dass weder der Controller noch die Vorlage "Create View" etwas über die tatsächlichen Validierungsregeln informiert, die erzwungen werden, oder über die angezeigten Fehlermeldungen. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben.

Wenn Sie die Validierungs Logik zu einem späteren Zeitpunkt ändern möchten, können Sie dies an genau einem Ort tun. Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Und dies bedeutet, dass Sie das DRY-Prinzip vollständig einhalten.

## <a name="adding-formatting-to-the-movie-model"></a>Hinzufügen von Formatierung zum Movie-Modell

Öffnen Sie Datei *Movie.cs*. Der [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt zusätzlich zum integrierten Satz von Validierungs Attributen Formatierungs Attribute bereit. Sie wenden das [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut und einen [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Enumerationswert auf das Veröffentlichungsdatum und die Preis Felder an. Der folgende Code zeigt die Eigenschaften `ReleaseDate` und `Price` mit dem entsprechenden [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut an.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Alternativ können Sie explizit einen [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) Wert festlegen. Der folgende Code zeigt die Eigenschaft "Release Date" mit einer Datumsformat Zeichenfolge (d. b. "d"). Mit dieser Option geben Sie an, dass Sie nicht als Teil des Veröffentlichungsdatums Zeit verwenden möchten.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Im folgenden Code wird die `Price`-Eigenschaft als Währung formatiert.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Die gesamte `Movie`-Klasse ist unten dargestellt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Führen Sie die Anwendung aus, und navigieren Sie zum `Movies` Controller.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Im nächsten Teil der Reihe überprüfen wir die Anwendung und nehmen einige Verbesserungen an den automatisch generierten Methoden `Details` und `Delete` vor.

> [!div class="step-by-step"]
> [Zurück](adding-a-new-field.md)
> [Weiter](improving-the-details-and-delete-methods.md)
