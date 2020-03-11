---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Erstellen eines benutzerdefinierten AJAX Control Toolkit-SteuerC#Element-Extenders () | Microsoft-Dokumentation
author: microsoft
description: Benutzerdefinierte Extender ermöglichen es Ihnen, die Funktionen von ASP.NET-Steuerelementen anzupassen und zu erweitern, ohne neue Klassen erstellen zu müssen.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 7850e745f5985688c95fc7f649ccbb06b2f66e20
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430287"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Erstellen eines benutzerdefinierten AJAX Control Toolkit-Steuerelement-Extender (C#)

von [Microsoft](https://github.com/microsoft)

> Benutzerdefinierte Extender ermöglichen es Ihnen, die Funktionen von ASP.NET-Steuerelementen anzupassen und zu erweitern, ohne neue Klassen erstellen zu müssen.

In diesem Tutorial erfahren Sie, wie Sie ein benutzerdefiniertes AJAX Control Toolkit-Steuerelement-Extender erstellen. Wir erstellen einen einfachen, aber nützlichen neuen Extender, der den Zustand einer Schaltfläche von "deaktiviert" in "aktiviert" ändert, wenn Sie Text in ein Textfeld eingeben. Nach dem Lesen dieses Tutorials können Sie das ASP.NET AJAX-Toolkit mit ihren eigenen Steuerelement Erweiterungen erweitern.

Sie können benutzerdefinierte Steuerelement Erweiterungen mithilfe von Visual Studio oder Visual Web Developer erstellen (stellen Sie sicher, dass Sie über die neueste Version von Visual Web Developer verfügen).

## <a name="overview-of-the-disabledbutton-extender"></a>Übersicht über den disabledbutton-Extender

Unser neuer Steuerelement-Extender heißt der disabledbutton-Extender. Dieser Extender hat drei Eigenschaften:

- TargetControlID: das Textfeld, das vom Steuerelement erweitert wird.
- Targetbuttoniid: die Schaltfläche, die deaktiviert oder aktiviert ist.
- Disabledtext: der Text, der anfänglich in der Schaltfläche angezeigt wird. Wenn Sie mit der Eingabe beginnen, wird mit der Schaltfläche der Wert der Eigenschaft Text der Schaltfläche angezeigt.

Sie können den disabledbutton-Extender an ein TextBox-und Schaltflächen-Steuerelement anschließen. Bevor Sie Text eingeben, wird die Schaltfläche deaktiviert, und das Textfeld und die Schaltfläche sehen wie folgt aus:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))

Wenn Sie mit der Eingabe von Text beginnen, wird die Schaltfläche aktiviert, und das Textfeld und die Schaltfläche sehen wie folgt aus:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))

Zum Erstellen des Steuerelement-Extenders müssen die folgenden drei Dateien erstellt werden:

- DisabledButtonExtender.cs: Diese Datei ist die serverseitige Steuerelement Klasse, die das Erstellen des Extenders verwaltet und es Ihnen ermöglicht, die Eigenschaften zur Entwurfszeit festzulegen. Außerdem werden die Eigenschaften definiert, die für Ihren Extender festgelegt werden können. Sie können auf diese Eigenschaften über Code und zur Entwurfszeit zugreifen und die in der Datei "disablebuttonbehavior. js" definierten Eigenschaften abgleichen.
- Disabledbuttonbehavior. js: in dieser Datei fügen Sie Ihre gesamte Client Skript Logik hinzu.
- DisabledButtonDesigner.cs: Diese Klasse ermöglicht Entwurfszeit Funktionen. Sie benötigen diese Klasse, wenn Sie möchten, dass der Steuerelement-Extender mit dem Visual Studio-/Visual Web Developer-Designer ordnungsgemäß funktioniert.

Ein Steuerelement-Extender besteht also aus einem serverseitigen Steuerelement, einem Client seitigen Verhalten und einer serverseitigen Designer-Klasse. In den folgenden Abschnitten erfahren Sie, wie Sie alle drei Dateien erstellen.

## <a name="creating-the-custom-extender-website-and-project"></a>Erstellen der benutzerdefinierten extenderwebsite und des Projekts

Der erste Schritt besteht im Erstellen eines Klassen Bibliotheks Projekts und einer Website in Visual Studio/Visual Web Developer. Wir erstellen den benutzerdefinierten Extender im Klassen Bibliotheksprojekt und testen den benutzerdefinierten Extender auf der Website.

Beginnen Sie mit der Website. Gehen Sie folgendermaßen vor, um die Website zu erstellen:

1. Wählen Sie die Menü Options **Datei, neue Website**aus.
2. Wählen Sie die Vorlage **ASP.NET-Website** aus.
3. Nennen Sie die neue Website *website1*.
4. Klicken Sie auf die Schaltfläche **OK** .

Als nächstes müssen Sie das Klassen Bibliotheksprojekt erstellen, das den Code für den Steuerelement-Extender enthalten soll:

1. Wählen Sie die Menüoption **Datei, hinzufügen, neues Projekt**aus.
2. Wählen Sie die Vorlage **Klassenbibliothek** aus.
3. Benennen Sie die neue Klassenbibliothek mit dem Namen **customextenders**.
4. Klicken Sie auf die Schaltfläche **OK** .

Nachdem Sie diese Schritte ausgeführt haben, sollte das Projektmappen-Explorer Fenster wie in Abbildung 1 aussehen.

[![Lösung mit Website-und Klassen Bibliotheksprojekt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Abbildung 01**: Projekt Mappe mit Website-und Klassen Bibliotheksprojekt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))

Als nächstes müssen Sie alle erforderlichen Assemblyverweise auf das Klassen Bibliotheksprojekt hinzufügen:

1. Klicken Sie mit der rechten Maustaste auf das Projekt customextenders, und wählen Sie die Menüoption **Verweis hinzufügen**.
2. Wählen Sie die Registerkarte .net aus.
3. Fügen Sie Verweise auf die folgenden Assemblys hinzu:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Wählen Sie die Registerkarte durchsuchen.
5. Fügen Sie einen Verweis auf die Assembly "AjaxControlToolkit. dll" hinzu. Diese Assembly befindet sich in dem Ordner, in den Sie das AJAX Control Toolkit heruntergeladen haben.

Nachdem Sie diese Schritte ausgeführt haben, sollte der Ordner für die Klassen Bibliotheksprojekt Verweise wie in Abbildung 2 aussehen.

[Ordner "![Verweise" mit erforderlichen verweisen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Abbildung 02**: Verweise auf Ordner mit erforderlichen verweisen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Erstellen des benutzerdefinierten Steuerelement-Extender

Nun, da wir unsere Klassenbibliothek haben, können wir mit dem entwickeln unserer Extendersteuerelement beginnen. Beginnen Sie mit den leeren Knochen einer benutzerdefinierten Extender-Steuerelement Klasse (siehe Codebeispiel 1).

**Codebeispiel 1-MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Es gibt mehrere Dinge, die Sie über die Steuerelement-Extender-Klasse in der Liste 1 bemerken. Beachten Sie zunächst, dass die Klasse von der Basisklasse "extendercontrolbase" erbt. Alle AJAX Control Toolkit-Extendersteuerelemente werden von dieser Basisklasse abgeleitet. Die-Basisklasse enthält z. b. die targetID-Eigenschaft, die eine erforderliche Eigenschaft für jeden Steuerelement-Extender ist.

Beachten Sie, dass die-Klasse die folgenden zwei Attribute enthält, die mit dem Client Skript verknüpft sind:

- WebResource: bewirkt, dass eine Datei als eingebettete Ressource in einer Assembly enthalten ist.
- Clientscriptresource: bewirkt, dass eine Skript Ressource aus einer Assembly abgerufen wird.

Das WebResource-Attribut wird verwendet, um die JavaScript-Datei mycontrolbehavior. js in die Assembly einzubetten, wenn der benutzerdefinierte Extender kompiliert wird. Das clientscriptresource-Attribut wird verwendet, um das Skript "mycontrolbehavior. js" aus der Assembly abzurufen, wenn der benutzerdefinierte Extender in einer Webseite verwendet wird.

Damit die Attribute WebResource und clientscriptresource funktionieren, muss die JavaScript-Datei als eingebettete Ressource kompiliert werden. Wählen Sie die Datei im Fenster Projektmappen-Explorer aus, öffnen Sie das Eigenschaften Blatt, und weisen Sie der Eigenschaft **Buildaktion** den Wert *Embedded Resource* zu.

Beachten Sie, dass der Steuerelement-Extender auch ein TargetControlType-Attribut enthält. Dieses Attribut wird verwendet, um den Typ des Steuer Elements anzugeben, das durch den Steuerelement-Extender erweitert wird. Im Fall von "Listing 1" wird der Steuerelement-Extender zum Erweitern eines Textfelds verwendet.

Beachten Sie schließlich, dass der benutzerdefinierte Extender eine Eigenschaft mit dem Namen MyProperty enthält. Die-Eigenschaft ist mit dem extendercontrolproperty-Attribut gekennzeichnet. Die Methoden GetPropertyValue () und SetPropertyValue () werden verwendet, um den Eigenschafts Wert vom serverseitigen Steuerelement-Extender an das Client seitige Verhalten zu übergeben.

Ich implementiere den Code für den disabledbutton-Extender. Den Code für diesen Extender finden Sie in der Liste 2.

**Codebeispiel 2: DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Der disabledbutton-Extender in der Liste 2 verfügt über zwei Eigenschaften namens targetbuttonid und disabledtext. Die auf die targetbuttonid-Eigenschaft angewendete IDReferenceProperty verhindert, dass Sie der Eigenschaft nichts anderes zuweisen, als die ID eines Schaltflächen-Steuer Elements.

Das WebResource-Attribut und das clientscriptresource-Attribut ordnen ein Client seitiges Verhalten in einer Datei mit dem Namen disabledbuttonbehavior. js mit diesem Extender zu. Diese JavaScript-Datei wird im nächsten Abschnitt erläutert.

## <a name="creating-the-custom-extender-behavior"></a>Erstellen des benutzerdefinierten extenderverhaltens

Die Client seitige Komponente eines Steuerelement-Extenders wird als Verhalten bezeichnet. Die tatsächliche Logik zum Deaktivieren und Aktivieren der Schaltfläche ist im Verhalten von disabledbutton enthalten. Der JavaScript-Code für das Verhalten ist in der Liste 3 enthalten.

**Codebeispiel 3: disabledbutton. js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Die JavaScript-Datei in der Liste 3 enthält eine Client seitige Klasse mit dem Namen disabledbuttonbehavior. Diese Klasse enthält wie der serverseitige Zwilling zwei Eigenschaften mit dem Namen "targetbuttonid" und "disabledtext", auf die Sie mithilfe von Get\_targetbuttonid/Set\_targetbuttonid zugreifen und\_disabledtext/Set\_disabledtext abrufen können.

Mit der Initialize ()-Methode wird ein KeyUp-Ereignishandler mit dem Target-Element für das Verhalten verknüpft. Jedes Mal, wenn Sie einen Buchstaben in das Textfeld eingeben, das diesem Verhalten zugeordnet ist, wird der KeyUp-Handler ausgeführt. Der KeyUp-Handler aktiviert oder deaktiviert die Schaltfläche abhängig davon, ob das Textfeld, das dem Verhalten zugeordnet ist, Text enthält.

Beachten Sie, dass Sie die JavaScript-Datei in der Liste 3 als eingebettete Ressource kompilieren müssen. Wählen Sie die Datei im Fenster Projektmappen-Explorer aus, öffnen Sie das Eigenschaften Blatt, und weisen Sie der Eigenschaft **Buildaktion** den Wert *Embedded-Ressource* zu (siehe Abbildung 3). Diese Option ist sowohl in Visual Studio als auch in Visual Web Developer verfügbar.

[![Hinzufügen einer JavaScript-Datei als eingebettete Ressource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Abbildung 03**: Hinzufügen einer JavaScript-Datei als eingebettete Ressource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Erstellen des benutzerdefinierten Extender-Designers

Es gibt eine letzte Klasse, die wir erstellen müssen, um unseren Extender abzuschließen. Wir müssen die Designer Klasse in der Liste 4 erstellen. Diese Klasse ist erforderlich, damit sich der Extender mit dem Visual Studio-/Visual Web Developer-Designer korrekt verhält.

**Codebeispiel 4-DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Sie ordnen den Designer in der Liste 4 dem disabledbutton-Extender mit dem Designer-Attribut zu. Sie müssen das Designer-Attribut wie folgt auf die disabledbuttonextender-Klasse anwenden:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Verwenden des benutzerdefinierten Extenders

Nachdem wir nun die Erstellung des disabledbutton-Steuerelement-Extenders abgeschlossen haben, ist es an der Zeit, ihn auf unserer ASP.NET-Website zu verwenden. Zuerst müssen wir den benutzerdefinierten Extender der Toolbox hinzufügen. Folgen Sie diesen Schritten:

1. Öffnen Sie eine ASP.NET-Seite, indem Sie im Projektmappen-Explorer Fenster auf die Seite doppelklicken.
2. Klicken Sie mit der rechten Maustaste auf die Toolbox, und wählen Sie die Menüoption **Elemente auswählen**.
3. Navigieren Sie im Dialogfeld Toolbox Elemente auswählen zur Assembly customextenders. dll.
4. Klicken Sie zum Schließen des Dialog Felds auf die Schaltfläche **OK** .

Nachdem Sie diese Schritte ausgeführt haben, sollte der Extender des disabledbutton-Steuer Elements in der Toolbox angezeigt werden (siehe Abbildung 4).

[![disabledbutton in der Toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Abbildung 04**: disabledbutton in der Toolbox ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))

Als nächstes müssen wir eine neue ASP.NET-Seite erstellen. Folgen Sie diesen Schritten:

1. Erstellen Sie eine neue ASP.NET-Seite mit dem Namen showdisabledbutton. aspx.
2. Ziehen Sie einen ScriptManager auf die Seite.
3. Ziehen Sie ein TextBox-Steuerelement auf die Seite.
4. Ziehen Sie ein Schaltflächen-Steuerelement auf die Seite.
5. Ändern Sie im Eigenschaftenfenster die Eigenschaft Schaltflächen-ID in den Wert <em>btnSave</em> und die Text-Eigenschaft in den Wert *Speichern\** .

Wir haben eine Seite mit einem standardmäßigen ASP.net-Textfeld und Schaltflächen-Steuerelement erstellt.

Als nächstes müssen wir das TextBox-Steuerelement mit dem disabledbutton-Extender erweitern:

1. Wählen Sie die Option Extender-Aufgabe **Hinzufügen** , um das Dialogfeld extenderassistent zu öffnen (siehe Abbildung 5). Beachten Sie, dass das Dialogfeld den benutzerdefinierten disabledbutton-Extender enthält.
2. Wählen Sie den Extender disabledbutton aus, und klicken Sie auf die Schaltfläche **OK** .

[![des extenderassistenten](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Abbildung 05**: Dialogfeld "extenderassistent" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))

Schließlich können wir die Eigenschaften des disabledbutton-Extenders festlegen. Sie können die Eigenschaften des "disabledbutton"-Extenders ändern, indem Sie die Eigenschaften des TextBox-Steuer Elements ändern:

1. Wählen Sie im Designer das Textfeld aus.
2. Erweitern Sie in der Eigenschaftenfenster den Knoten Extenders (siehe Abbildung 6).
3. Weisen Sie den Wert *Save* der disabledtext-Eigenschaft und den Wert *btnSave* der targetbuttonid-Eigenschaft zu.

[![Festlegen von Extendereigenschaften](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Abbildung 06**: Festlegen der Extendereigenschaften ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))

Wenn Sie die Seite ausführen (durch Drücken von F5), ist das Schaltflächen-Steuerelement anfänglich deaktiviert. Sobald Sie mit der Eingabe von Text in das Textfeld beginnen, ist das Schaltflächen-Steuerelement aktiviert (siehe Abbildung 7).

[![den disabledbutton-Extender in Aktion](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Abbildung 07**: der disabledbutton-Extender in Aktion ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wird erläutert, wie Sie das AJAX Control Toolkit mit benutzerdefinierten Extender-Steuerelementen erweitern können. In diesem Tutorial haben wir einen einfachen disabledbutton-Steuerelement-Extender erstellt. Wir haben diesen Extender implementiert, indem wir eine disabledbuttonextender-Klasse, ein disabledbuttonbehavior-JavaScript-Verhalten und eine disabledbuttondesigner-Klasse erstellt haben. Wenn Sie einen benutzerdefinierten Steuerelement-Extender erstellen, befolgen Sie einen ähnlichen Satz von Schritten.

> [!div class="step-by-step"]
> [Zurück](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Weiter](get-started-with-the-ajax-control-toolkit-vb.md)
