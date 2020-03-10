---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Erstellen einer MVC 3-Anwendung mit Razor und unaufdringlichem JavaScript | Microsoft-Dokumentation
author: microsoft
description: Die Webanwendung User List Sample zeigt, wie einfach es ist, ASP.NET MVC 3-Anwendungen mithilfe der Razor-Ansichts-Engine zu erstellen. Die Beispielanwendung s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434997"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Erstellen einer MVC 3-Anwendung mit Razor und unaufdringlichem JavaScript

von [Microsoft](https://github.com/microsoft)

> Die Webanwendung User List Sample zeigt, wie einfach es ist, ASP.NET MVC 3-Anwendungen mithilfe der Razor-Ansichts-Engine zu erstellen. Die Beispielanwendung zeigt, wie die neue Razor-Ansichts-Engine mit ASP.NET MVC Version 3 und Visual Studio 2010 verwendet wird, um eine fiktive Benutzer Listen-Website zu erstellen, die Funktionen wie das Erstellen, anzeigen, bearbeiten und Löschen von Benutzern umfasst.
> 
> In diesem Tutorial werden die Schritte beschrieben, die zum Erstellen des Benutzer Listen Beispiels ASP.NET MVC 3-Anwendung ausgeführt wurden. Ein Visual Studio-Projekt C# mit und VB-Quellcode ist für dieses Thema verfügbar: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Wenn Sie Fragen zu diesem Tutorial haben, veröffentlichen Sie Sie im [MVC-Forum](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Übersicht

Die Anwendung, die Sie entwickeln, ist eine einfache Benutzer Listen-Website. Benutzer können Benutzerinformationen eingeben, anzeigen und aktualisieren.

![Beispiel Website](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Sie können das VB-Projekt C# und das abgeschlossene Projekt [hier](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)herunterladen.

## <a name="creating-the-web-application"></a>Erstellen der Webanwendung

Um das Tutorial zu starten, öffnen Sie Visual Studio 2010, und erstellen Sie ein neues Projekt mithilfe der Vorlage *ASP.NET MVC 3-Webanwendung* . Benennen Sie die Anwendung &quot;Mvc3Razor&quot;.

[neues MVC 3-Projekt ![](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 3-Projekt** die Option **Internet Anwendung**aus, wählen Sie die Razor-Ansicht-Engine aus, und klicken Sie dann auf **OK**

![Neues ASP.NET MVC 3-Projekt Dialogfeld](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In diesem Tutorial verwenden Sie den ASP.net-Mitgliedschafts Anbieter nicht, sodass Sie alle Dateien löschen können, die mit der Anmeldung und der Mitgliedschaft verknüpft sind. Entfernen Sie in **Projektmappen-Explorer**die folgenden Dateien und Verzeichnisse:

- *Controllers\accountcontroller*
- *Models\accountmodels*
- *Views\shared\\_LogOnPartial*
- *Views\account* (und alle Dateien in diesem Verzeichnis)

![Soln-Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Bearbeiten Sie die Datei <em>\_Layout. cshtml</em> , und ersetzen Sie das Markup im `<div>` Element mit dem Namen `logindisplay` durch die Meldung <em>&quot;</em>Login&quot;deaktiviert. Das folgende Beispiel zeigt das neue Markup:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Hinzufügen des Modells

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , wählen Sie **Hinzufügen**aus, und klicken Sie auf **Klasse**.

![Neue Benutzer-MDL-Klasse](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Geben Sie der Klassen den Namen `UserModel`. Ersetzen Sie den Inhalt der *usermodel* -Datei durch den folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

Die `UserModel`-Klasse stellt Benutzer dar. Jeder Member der Klasse wird mit dem [erforderlichen](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) Attribut aus dem [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace kommentiert. Die Attribute im [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace bieten eine automatische Client-und serverseitige Validierung für Webanwendungen.

Öffnen Sie die `HomeController`-Klasse, und fügen Sie eine `using`-Direktive hinzu, damit Sie auf die Klassen `UserModel` und `Users` zugreifen können:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Fügen Sie direkt nach der `HomeController` Deklaration den folgenden Kommentar und den Verweis zu einer `Users` Klasse hinzu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

Bei der `Users`-Klasse handelt es sich um einen vereinfachten, in-Memory-Datenspeicher, den Sie in diesem Tutorial verwenden werden. In einer echten Anwendung würden Sie eine Datenbank zum Speichern von Benutzerinformationen verwenden. Die ersten Zeilen der `HomeController`-Datei werden im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Erstellen Sie die Anwendung so, dass das Benutzer Modell im nächsten Schritt für den Gerüstbau-Assistenten verfügbar ist.

## <a name="creating-the-default-view"></a>Erstellen der Standardansicht

Der nächste Schritt besteht darin, eine Aktionsmethode und Ansicht hinzuzufügen, um die Benutzer anzuzeigen.

Löschen Sie die vorhandene Datei " *views\home\index* ". Sie erstellen eine neue *Index* Datei, um die Benutzer anzuzeigen.

Ersetzen Sie in der `HomeController`-Klasse den Inhalt der `Index`-Methode durch den folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Klicken Sie mit der rechten Maustaste in die `Index` Methode, und klicken Sie dann auf **Ansicht hinzufügen**

![Ansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus. Wählen Sie für **Datenklasse anzeigen**die Option **Mvc3Razor. Models. usermodel**aus. (Wenn **Mvc3Razor. Models. usermodel** nicht im Feld **Datenklasse anzeigen** angezeigt wird, müssen Sie das Projekt erstellen.) Stellen Sie sicher, dass die Ansichts-Engine auf **Razor**festgelegt ist. Legen Sie **Inhalt anzeigen** auf **auflisten** fest, und klicken Sie auf **Hinzufügen**.

![Index Sicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Die neue Ansicht richtet automatisch die Benutzerdaten ein, die an die `Index` Ansicht übermittelt werden. Überprüfen Sie die neu generierte Datei *views\home\index* . Die Links " **Create New**", " **Edit**", " **Details**" und " **Delete** " funktionieren nicht, aber der Rest der Seite ist funktionsfähig. Führen Sie die Seite aus. Eine Liste der Benutzer wird angezeigt.

![Index Seite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Öffnen Sie die Datei " *Index. cshtml* ", und ersetzen Sie das `ActionLink` Markup durch den folgenden Code für **Bearbeiten**, **Details**und **Delete** :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Der Benutzername wird als ID verwendet, um den ausgewählten Datensatz in den Links " **Bearbeiten**", " **Details**" und " **Löschen** " zu suchen.

## <a name="creating-the-details-view"></a>Erstellen der Detailansicht

Der nächste Schritt besteht darin, eine `Details` Aktionsmethode und-Ansicht hinzuzufügen, um Benutzer Details anzuzeigen.

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Fügen Sie dem Home-Controller die folgende `Details`-Methode hinzu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Klicken Sie mit der rechten Maustaste in die `Details`-Methode, und wählen Sie dann <strong>Ansicht hinzufügen</strong> Vergewissern Sie sich, dass das Feld <strong>Datenklasse anzeigen</strong> das <strong>Mvc3Razor. Models. usermodel</strong>enthält<em>.</em> Legen Sie <strong>Inhalt anzeigen</strong> auf <strong>Details</strong> fest, und klicken Sie auf <strong>Hinzufügen</strong>.

![Detailansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Führen Sie die Anwendung aus, und wählen Sie einen Link Details aus. Das automatische Gerüst zeigt jede Eigenschaft im Modell an.

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Erstellen der Bearbeitungs Ansicht

Fügen Sie dem Home-Controller die folgende `Edit`-Methode hinzu.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Fügen Sie eine Ansicht wie in den vorherigen Schritten hinzu, aber legen Sie **Inhalt anzeigen** auf **Bearbeiten**fest.

![Bearbeitungs Ansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Führen Sie die Anwendung aus, und bearbeiten Sie den vor-und Nachnamen eines Benutzers. Wenn Sie gegen `DataAnnotation` Einschränkungen verstoßen, die auf die `UserModel` Klasse angewendet wurden, werden beim Senden des Formulars Validierungs Fehler angezeigt, die vom Servercode erzeugt werden. Wenn Sie z. b. den Vornamen &quot;Ann&quot; ändern, um eine&quot;zu &quot;, wird der folgende Fehler im Formular angezeigt, wenn Sie das Formular senden:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In diesem Tutorial behandeln Sie den Benutzernamen als Primärschlüssel. Aus diesem Grund kann die Eigenschaft Benutzername nicht geändert werden. Legen Sie in der Datei " *Edit. cshtml* " direkt nach der `Html.BeginForm`-Anweisung den Benutzernamen als ausgeblendetes Feld fest. Dies bewirkt, dass die-Eigenschaft im Modell übermittelt wird. Das folgende Code Fragment zeigt die Platzierung der `Hidden`-Anweisung:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Ersetzen Sie das `TextBoxFor`-und `ValidationMessageFor` Markup für den Benutzernamen durch einen `DisplayFor`-Befehl. Die `DisplayFor`-Methode zeigt die Eigenschaft als Schreib geschütztes Element an. Das folgende Beispiel zeigt das abgeschlossene Markup. Die ursprünglichen `TextBoxFor` und `ValidationMessageFor` Aufrufe werden mit den Razor-BEGIN-comment-und End-Comment-Zeichen (`@* *@`) auskommentiert.

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Aktivieren der Client seitigen Validierung

Um die Client seitige Validierung in ASP.NET MVC 3 zu aktivieren, müssen Sie zwei Flags festlegen, und Sie müssen drei JavaScript-Dateien einschließen.

Öffnen Sie die Datei " *Web. config* " der Anwendung. Überprüfen Sie, `that ClientValidationEnabled` und `UnobtrusiveJavaScriptEnabled` in den Anwendungseinstellungen auf true festgelegt sind. Das folgende Fragment aus der *Web. config* -Stammdatei zeigt die korrekten Einstellungen an:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Wenn Sie `UnobtrusiveJavaScriptEnabled` auf true festlegen, wird eine unaufdringliche und unaufdringliche Client Validierung ermöglicht. Wenn Sie die unaufdringliche Validierung verwenden, werden die Validierungsregeln in HTML5-Attribute umgewandelt. HTML5-Attributnamen dürfen nur aus Kleinbuchstaben, Ziffern und Bindestrichen bestehen.

Wenn Sie `ClientValidationEnabled` auf true festlegen, wird die Client seitige Validierung aktiviert. Wenn Sie diese Schlüssel in der *Web. config* -Datei der Anwendung festlegen, aktivieren Sie die Client Validierung und unaufdringliches JavaScript für die gesamte Anwendung. Sie können diese Einstellungen auch in einzelnen Ansichten oder in Controller Methoden aktivieren oder deaktivieren, indem Sie den folgenden Code verwenden:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Außerdem müssen Sie mehrere JavaScript-Dateien in der gerenderten Ansicht einschließen. Eine einfache Möglichkeit, JavaScript in alle Ansichten einzufügen, besteht darin, Sie der Datei " *views\shared\\_Layout. cshtml* " hinzuzufügen. Ersetzen Sie das `<head>`-Element der Datei *\_Layout. cshtml* durch den folgenden Code:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Die ersten beiden jQuery-Skripts werden vom Microsoft AJAX-Content Delivery Network (CDN) gehostet. Wenn Sie das Microsoft AJAX CDN nutzen, können Sie die Leistung Ihrer Anwendungen erheblich verbessern.

Führen Sie die Anwendung aus, und klicken Sie auf den Link bearbeiten. Anzeigen der Quelle der Seite im Browser. Die Browser Quelle zeigt viele Attribute der Form `data-val` (für die Datenvalidierung). Wenn die Client Validierung und das unaufdringliche Javascript aktiviert sind, enthalten Eingabefelder mit einer Client Validierungs Regel das `data-val="true"`-Attribut, um eine unaufdringliche Client Validierung zu initiieren. Beispielsweise wurde das `City`-Feld im Modell mit dem [erforderlichen](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut ergänzt, das im folgenden Beispiel den HTML-Code ergibt:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Für jede Client Validierungs Regel wird ein Attribut hinzugefügt, das die Form `data-val-rulename="message"`hat. Mithilfe des zuvor gezeigten `City` Field-Beispiels generiert die erforderliche Client Validierungs Regel das `data-val-required`-Attribut, und die Meldung &quot;das Feld "City" ist erforderlich&quot;. Führen Sie die Anwendung aus, bearbeiten Sie einen der Benutzer, und löschen Sie das Feld `City`. Wenn Sie das Feld aus dem Feld auslagern, wird eine Client seitige Validierungs Fehlermeldung angezeigt.

![Stadt erforderlich](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Ebenso wird für jeden Parameter in der Client Validierungs Regel ein Attribut hinzugefügt, das die Form `data-val-rulename-paramname=paramvalue`hat. Beispielsweise wird die `FirstName`-Eigenschaft mit dem [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut kommentiert und gibt eine minimale Länge von 3 und eine maximale Länge von 8 an. Die Daten Validierungs Regel namens `length` hat den Parameternamen `max` und den Parameterwert 8. Das folgende Beispiel zeigt den HTML-Code, der für das Feld `FirstName` generiert wird, wenn Sie einen der Benutzer bearbeiten:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Weitere Informationen zur unaufdringlichen Client Validierung finden Sie im Blogeintrag [unaufdringliche Client Validierung in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson es Blog.

> [!NOTE]
> In ASP.NET MVC 3 Beta müssen Sie manchmal das Formular übermitteln, um die Client seitige Validierung zu starten. Dies kann für die endgültige Version geändert werden.

## <a name="creating-the-create-view"></a>Erstellen der CREATE VIEW

Der nächste Schritt besteht darin, eine `Create` Aktionsmethode und-Ansicht hinzuzufügen, um dem Benutzer zu ermöglichen, einen neuen Benutzer zu erstellen. Fügen Sie dem Home-Controller die folgende `Create`-Methode hinzu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Fügen Sie eine Ansicht wie in den vorherigen Schritten hinzu, aber legen Sie **Inhalt anzeigen** auf **Erstellen**fest.

![Create View (Ansicht erstellen)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Führen Sie die Anwendung aus, wählen Sie den Link **Erstellen** aus, und fügen Sie einen neuen Benutzer hinzu. Die `Create`-Methode nutzt automatisch die Client seitige und serverseitige Validierung. Versuchen Sie, einen Benutzernamen einzugeben, der Leerzeichen enthält, z. b. &quot;Ben X&quot;. Wenn Sie im Feld Benutzername auf die Registerkarte klicken, wird ein Client seitiger Validierungs Fehler (`White space is not allowed`) angezeigt.

## <a name="add-the-delete-method"></a>Hinzufügen der Delete-Methode

Fügen Sie dem Home-Controller die folgende `Delete` Methode hinzu, um das Tutorial abzuschließen:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Fügen Sie wie in den vorherigen Schritten eine `Delete` Ansicht hinzu, indem Sie den **Inhalt** auf **Löschen**festlegen.

![Ansicht löschen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Sie verfügen jetzt über eine einfache, aber voll funktionsfähige ASP.NET MVC 3-Anwendung mit Validierung.
