---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 3 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.net MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433215"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 3

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.NET MVC-Webanwendung.

## <a name="working-with-complex-types"></a>Arbeiten mit komplexen Typen

In diesem Abschnitt erstellen Sie eine Adress Klasse und erfahren, wie Sie eine Vorlage erstellen, um Sie anzuzeigen.

Erstellen Sie im Ordner *Models* eine neue Klassendatei mit dem Namen *Person.cs* , in der Sie zwei Typen einfügen: eine `Person`-Klasse und eine `Address`-Klasse. Die `Person`-Klasse enthält eine Eigenschaft, die als `Address`typisiert ist. Der `Address` Typ ist ein komplexer Typ. Dies bedeutet, dass es sich nicht um einen der integrierten Typen wie `int`, `string`oder `double`handelt. Stattdessen verfügt er über mehrere Eigenschaften. Der Code für die neuen Klassen sieht wie folgt aus:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

Fügen Sie im `Movie` Controller die folgende `PersonDetail` Aktion hinzu, um eine Person-Instanz anzuzeigen:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Fügen Sie dann dem `Movie` Controller den folgenden Code hinzu, um das Modell `Person` mit einigen Beispiel Daten aufzufüllen:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Öffnen Sie die Datei " *views\movies\persondetail.cshtml* ", und fügen Sie das folgende Markup für die `PersonDetail` Ansicht hinzu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Drücken Sie STRG + F5, um die Anwendung auszuführen, und navigieren Sie zu *Movies/persondetail*.

Die `PersonDetail` Sicht enthält nicht den `Address` komplexen Typ, wie Sie in diesem Screenshot sehen können. (Es wird keine Adresse angezeigt.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Die `Address` Modelldaten werden nicht angezeigt, weil es sich um einen komplexen Typ handelt. Öffnen Sie die Datei " *views\movies\persondetail.cshtml* " erneut, und fügen Sie das folgende Markup hinzu, um die Adressinformationen anzuzeigen.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Das gesamte Markup für die `PersonDetail` now-Ansicht sieht wie folgt aus:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Führen Sie die Anwendung erneut aus, und zeigen Sie die `PersonDetail` Ansicht an. Die Adressinformationen werden jetzt angezeigt:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Erstellen einer Vorlage für einen komplexen Typ

In diesem Abschnitt erstellen Sie eine Vorlage, die verwendet wird, um den `Address` komplexen Typs zu Rendering. Wenn Sie eine Vorlage für den `Address`-Typ erstellen, kann ASP.NET MVC diese automatisch verwenden, um ein Adress Modell an beliebiger Stelle in der Anwendung zu formatieren. Dadurch haben Sie die Möglichkeit, das Rendering des `Address` Typs von nur einer Stelle in der Anwendung zu steuern.

Erstellen Sie im Ordner *views\shared\displaytemplates* eine stark typisierte Teilansicht namens **Address**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Klicken Sie auf **Hinzufügen**, und öffnen Sie dann die neue Datei *views\shared\displaytemplates\address.cshtml* . Die neue Sicht enthält das folgende generierte Markup:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Führen Sie die Anwendung aus, und zeigen Sie die `PersonDetail` Ansicht an. Dieses Mal wird die soeben erstellte `Address` Vorlage zum Anzeigen des komplexen `Address` Typs verwendet, sodass die Anzeige wie folgt aussieht:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Zusammenfassung: Möglichkeiten zum Angeben des Modell Anzeige Formats und der Vorlage

Sie haben gesehen, dass Sie das Format oder die Vorlage für eine Modell Eigenschaft mit den folgenden Ansätzen angeben können:

- Anwenden des `DisplayFormat`-Attributs auf eine Eigenschaft im Modell. Der folgende Code bewirkt beispielsweise, dass das Datum ohne die Zeit angezeigt wird:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Anwenden eines [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attributs auf eine Eigenschaft im Modell und angeben des Datentyps. Der folgende Code bewirkt beispielsweise, dass das Datum ohne die Zeit angezeigt wird.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Wenn die Anwendung eine *Date. cshtml* -Vorlage im Ordner " *views\shared\displaytemplates* " oder im Ordner " *views\movies\displaytemplates* " enthält, wird diese Vorlage zum Rendering der `DateTime`-Eigenschaft verwendet. Andernfalls zeigt das integrierte ASP.net-Vorlagen System die Eigenschaft als Datum an.
- Erstellen einer Anzeige Vorlage im Ordner " *views\shared\displaytemplates* " oder im Ordner " *views\movies\displaytemplates* ", deren Name mit dem Datentyp übereinstimmt, den Sie formatieren möchten. Beispielsweise haben Sie gesehen, dass " *views\shared\displaytemplates\datetime.cshtml* " verwendet wurde, um `DateTime` Eigenschaften in einem Modell zu erzeugen, ohne dem Modell ein Attribut hinzuzufügen und den Sichten keine Markup hinzuzufügen.
- Verwenden des [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) -Attributs für das Modell, um die Vorlage zum Anzeigen der Modell Eigenschaft anzugeben.
- Explizites Hinzufügen des Namens der Anzeige Vorlage zum [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) -Befehl in einer Ansicht.

Welche Vorgehensweise Sie verwenden, hängt davon ab, was Sie in Ihrer Anwendung tun müssen. Es ist nicht ungewöhnlich, diese Ansätze zu mischen, um genau die Art der Formatierung zu erzielen, die Sie benötigen.

Im nächsten Abschnitt wechseln Sie in ein Bit und wechseln von der Anpassung, wie Daten angezeigt werden, um anzupassen, wie Sie eingegeben werden. Sie verbinden den jQuery-DatePicker mit den Bearbeitungs Sichten in der Anwendung, um eine schlanke Möglichkeit zum Angeben von Datumsangaben bereitzustellen.

> [!div class="step-by-step"]
> [Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
