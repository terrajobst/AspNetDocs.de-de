---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Erstellen von benutzerdefinierten HTMLC#-Hilfsprogrammen () | Microsoft-Dokumentation
author: microsoft
description: Dieses Tutorial soll Ihnen zeigen, wie Sie benutzerdefinierte HTML-Hilfsprogramme erstellen können, die Sie in ihren MVC-Ansichten verwenden können. Durch die Nutzung von HTML-Hilfsprogrammen...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594518"
---
# <a name="creating-custom-html-helpers-c"></a>Erstellen von benutzerdefinierten HTML-Hilfsprogrammen (C#)

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Dieses Tutorial soll Ihnen zeigen, wie Sie benutzerdefinierte HTML-Hilfsprogramme erstellen können, die Sie in ihren MVC-Ansichten verwenden können. Durch die Nutzung von HTML-Hilfsprogrammen können Sie die Menge an mühsamer Typisierung von HTML-Tags verringern, die Sie ausführen müssen, um eine HTML-Standardseite zu erstellen.

Dieses Tutorial soll Ihnen zeigen, wie Sie benutzerdefinierte HTML-Hilfsprogramme erstellen können, die Sie in ihren MVC-Ansichten verwenden können. Durch die Nutzung von HTML-Hilfsprogrammen können Sie die Menge an mühsamer Typisierung von HTML-Tags verringern, die Sie ausführen müssen, um eine HTML-Standardseite zu erstellen.

Im ersten Teil dieses Tutorials beschreibe ich einige der vorhandenen HTML-Hilfsprogramme, die im ASP.NET MVC-Framework enthalten sind. Als nächstes werden zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsprogrammen beschrieben: Ich erkläre, wie benutzerdefinierte HTML-Hilfsprogramme erstellt werden, indem eine statische Methode erstellt und eine Erweiterungsmethode erstellt wird.

## <a name="understanding-html-helpers"></a>Informationen zu HTML-Hilfsprogramme

Ein HTML-Hilfsprogramm ist nur eine Methode, die eine Zeichenfolge zurückgibt. Die Zeichenfolge kann jeden gewünschten Inhaltstyp darstellen. Sie können z. b. HTML-Hilfsprogramme verwenden, um Standard-HTML-Tags wie HTML-`<input>` und `<img>` Tags zu erzeugen. Sie können auch HTML-Hilfsprogramme verwenden, um komplexere Inhalte, z. b. eine Registerkarte oder eine HTML-Tabelle mit Datenbankdaten, zu Rendering

Das ASP.NET-MVC-Framework umfasst die folgenden Standard-HTML-Hilfsprogramme (Dies ist keine komplette Liste):

- HTML. Action Link ()
- HTML. BeginForm ()
- HTML. CheckBox ()
- HTML. Dropdown List ()
- HTML. Endform ()
- HTML. Hidden ()
- HTML. ListBox ()
- HTML. Password ()
- HTML. RadioButton ()
- HTML. Textarea ()
- HTML. TextBox ()

Nehmen Sie beispielsweise das Formular in der Liste 1. Dieses Formular wird mithilfe von zwei der Standard-HTML-Hilfsprogramme gerendert (siehe Abbildung 1). Dieses Formular verwendet die `Html.BeginForm()`-und `Html.TextBox()`-Hilfsmethoden, um ein einfaches HTML-Formular zu erzeugen.

[mit HTML-Hilfsprogrammen gerenderte ![Seite](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Abbildung 01**: Seite mit HTML-Hilfsprogrammen gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-custom-html-helpers-cs/_static/image3.png))

**Codebeispiel 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Die Hilfsmethode HTML. BeginForm () wird verwendet, um die öffnenden und schließenden HTML-`<form>` Tags zu erstellen. Beachten Sie, dass die `Html.BeginForm()`-Methode in einer using-Anweisung aufgerufen wird. Die using-Anweisung stellt sicher, dass das `<form>`-Tag am Ende des using-Blocks geschlossen wird.

Wenn Sie es vorziehen, anstatt einen Using-Block zu erstellen, können Sie die HTML. Endform ()-Hilfsmethode zum Schließen des `<form>`-Tags aufruft. Verwenden Sie den jeweiligen Ansatz, um eine öffnende und schließende `<form>` Kennung zu erstellen, die für Sie intuitiver erscheint.

Die `Html.TextBox()`-Hilfsmethoden werden in der Liste 1 zum Rendering von HTML-`<input>` Tags verwendet. Wenn Sie die Option Quelle anzeigen in Ihrem Browser auswählen, wird die HTML-Quelle in der Liste 2 angezeigt. Beachten Sie, dass die Quelle Standard-HTML-Tags enthält.

> [!IMPORTANT]
> Beachten Sie, dass das `Html.TextBox()`-HTML-Hilfsprogramm mit `<%= %>` Tags anstelle `<% %>` Tags gerendert wird. Wenn Sie das Gleichheitszeichen nicht einschließen, wird nichts im Browser gerendert.

Das ASP.NET-MVC-Framework enthält eine kleine Gruppe von Hilfsprogrammen. Höchstwahrscheinlich müssen Sie das MVC-Framework mit benutzerdefinierten HTML-Hilfsprogrammen erweitern. Im restlichen Teil dieses Tutorials erlernen Sie zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsprogrammen.

**Codebeispiel 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Erstellen von HTML-Hilfsprogrammen mit statischen Methoden

Die einfachste Möglichkeit, ein neues HTML-Hilfsprogramm zu erstellen, ist das Erstellen einer statischen Methode, die eine Zeichenfolge zurückgibt. Stellen Sie sich beispielsweise vor, dass Sie ein neues HTML-Hilfsprogramm erstellen, das einen HTML-`<label>` Tag rendert. Sie können die-Klasse in "Listing 2" verwenden, um eine `<label>` zu erzeugen.

**Codebeispiel 2 – `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

In der Liste 2 gibt es keine besonderen Informationen über die Klasse. Die `Label()`-Methode gibt einfach eine Zeichenfolge zurück.

Die geänderte Index Sicht in der Liste 3 verwendet die `LabelHelper`, um HTML-`<label>` Tags zu erzeugen. Beachten Sie, dass die Sicht eine `<%@ imports %>` Direktive enthält, die den `Application1.Helpers`-Namespace importiert.

**Codebeispiel 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Erstellen von HTML-Hilfsprogrammen mit Erweiterungs Methoden

Wenn Sie HTML-Hilfsprogramme erstellen möchten, die genau wie die standardmäßigen HTML-Hilfsprogramme im ASP.NET MVC-Framework funktionieren, müssen Sie Erweiterungs Methoden erstellen. Erweiterungs Methoden ermöglichen es Ihnen, einer vorhandenen Klasse neue Methoden hinzuzufügen. Beim Erstellen einer HTML-Hilfsmethode fügen Sie der htmlhelper-Klasse, die durch die HTML-Eigenschaft einer Ansicht dargestellt wird, neue Methoden hinzu.

Die-Klasse in der Liste 3 fügt der `HtmlHelper` Klasse mit dem Namen `Label()`eine Erweiterungsmethode hinzu. Es gibt einige Dinge, die Sie über diese Klasse bemerken sollten. Beachten Sie zunächst, dass die-Klasse eine statische Klasse ist. Sie müssen eine Erweiterungsmethode mit einer statischen Klasse definieren.

Beachten Sie, dass dem ersten Parameter der `Label()`-Methode das Schlüsselwort `this`vorangestellt wird. Der erste Parameter einer Erweiterungsmethode gibt die Klasse an, die von der Erweiterungsmethode erweitert wird.

**Codebeispiel 3 – `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Nachdem Sie eine Erweiterungsmethode erstellt und die Anwendung erfolgreich erstellt haben, wird die Erweiterungsmethode in Visual Studio IntelliSense wie alle anderen Methoden einer Klasse angezeigt (siehe Abbildung 2). Der einzige Unterschied besteht darin, dass Erweiterungs Methoden mit einem speziellen Symbol nebeneinander angezeigt werden (Symbol eines abwärts Pfeils).

[![mithilfe der HTML. Label ()-Erweiterungsmethode](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Abbildung 02**: Verwenden der HTML. Label ()-Erweiterungsmethode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-custom-html-helpers-cs/_static/image6.png))

Die geänderte Index Sicht in der Liste 4 verwendet die HTML. Label ()-Erweiterungsmethode, um alle `<label>` Tags zu rendern.

**Codebeispiel 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Summary

In diesem Tutorial haben Sie zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsprogrammen kennengelernt. Zuerst haben Sie gelernt, wie Sie ein Benutzer `Label()` definiertes HTML-Hilfsprogramm erstellen, indem Sie eine statische Methode erstellen, die eine Zeichenfolge zurückgibt Als nächstes haben Sie erfahren, wie Sie eine benutzerdefinierte `Label()` HTML-Hilfsmethode erstellen, indem Sie eine Erweiterungsmethode für die `HtmlHelper`-Klasse erstellen.

In diesem Tutorial habe ich mich auf das Entwickeln einer extrem einfachen HTML-Hilfsmethode konzentriert. Beachten Sie, dass ein HTML-Hilfsprogramm so kompliziert sein kann, wie Sie möchten. Sie können HTML-Hilfsprogramme erstellen, die umfangreichen Inhalt, z. b. Struktur Ansichten, Menüs oder Tabellen von Datenbankdaten, darstellen.

> [!div class="step-by-step"]
> [Zurück](asp-net-mvc-views-overview-cs.md)
> [Weiter](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
