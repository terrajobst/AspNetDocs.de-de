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
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="f891c-103">Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 3</span><span class="sxs-lookup"><span data-stu-id="f891c-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>

<span data-ttu-id="f891c-104">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f891c-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="f891c-105">Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.NET MVC-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="f891c-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>

## <a name="working-with-complex-types"></a><span data-ttu-id="f891c-106">Arbeiten mit komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="f891c-106">Working with Complex Types</span></span>

<span data-ttu-id="f891c-107">In diesem Abschnitt erstellen Sie eine Adress Klasse und erfahren, wie Sie eine Vorlage erstellen, um Sie anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f891c-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="f891c-108">Erstellen Sie im Ordner *Models* eine neue Klassendatei mit dem Namen *Person.cs* , in der Sie zwei Typen einfügen: eine `Person`-Klasse und eine `Address`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f891c-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="f891c-109">Die `Person`-Klasse enthält eine Eigenschaft, die als `Address`typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="f891c-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="f891c-110">Der `Address` Typ ist ein komplexer Typ. Dies bedeutet, dass es sich nicht um einen der integrierten Typen wie `int`, `string`oder `double`handelt.</span><span class="sxs-lookup"><span data-stu-id="f891c-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="f891c-111">Stattdessen verfügt er über mehrere Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="f891c-111">Instead, it has several properties.</span></span> <span data-ttu-id="f891c-112">Der Code für die neuen Klassen sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f891c-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="f891c-113">Fügen Sie im `Movie` Controller die folgende `PersonDetail` Aktion hinzu, um eine Person-Instanz anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="f891c-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="f891c-114">Fügen Sie dann dem `Movie` Controller den folgenden Code hinzu, um das Modell `Person` mit einigen Beispiel Daten aufzufüllen:</span><span class="sxs-lookup"><span data-stu-id="f891c-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="f891c-115">Öffnen Sie die Datei " *views\movies\persondetail.cshtml* ", und fügen Sie das folgende Markup für die `PersonDetail` Ansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="f891c-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="f891c-116">Drücken Sie STRG + F5, um die Anwendung auszuführen, und navigieren Sie zu *Movies/persondetail*.</span><span class="sxs-lookup"><span data-stu-id="f891c-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="f891c-117">Die `PersonDetail` Sicht enthält nicht den `Address` komplexen Typ, wie Sie in diesem Screenshot sehen können.</span><span class="sxs-lookup"><span data-stu-id="f891c-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="f891c-118">(Es wird keine Adresse angezeigt.)</span><span class="sxs-lookup"><span data-stu-id="f891c-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="f891c-119">Die `Address` Modelldaten werden nicht angezeigt, weil es sich um einen komplexen Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="f891c-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="f891c-120">Öffnen Sie die Datei " *views\movies\persondetail.cshtml* " erneut, und fügen Sie das folgende Markup hinzu, um die Adressinformationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f891c-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="f891c-121">Das gesamte Markup für die `PersonDetail` now-Ansicht sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f891c-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="f891c-122">Führen Sie die Anwendung erneut aus, und zeigen Sie die `PersonDetail` Ansicht an.</span><span class="sxs-lookup"><span data-stu-id="f891c-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="f891c-123">Die Adressinformationen werden jetzt angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f891c-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="f891c-124">Erstellen einer Vorlage für einen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="f891c-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="f891c-125">In diesem Abschnitt erstellen Sie eine Vorlage, die verwendet wird, um den `Address` komplexen Typs zu Rendering.</span><span class="sxs-lookup"><span data-stu-id="f891c-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="f891c-126">Wenn Sie eine Vorlage für den `Address`-Typ erstellen, kann ASP.NET MVC diese automatisch verwenden, um ein Adress Modell an beliebiger Stelle in der Anwendung zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="f891c-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="f891c-127">Dadurch haben Sie die Möglichkeit, das Rendering des `Address` Typs von nur einer Stelle in der Anwendung zu steuern.</span><span class="sxs-lookup"><span data-stu-id="f891c-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="f891c-128">Erstellen Sie im Ordner *views\shared\displaytemplates* eine stark typisierte Teilansicht namens **Address**:</span><span class="sxs-lookup"><span data-stu-id="f891c-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="f891c-129">Klicken Sie auf **Hinzufügen**, und öffnen Sie dann die neue Datei *views\shared\displaytemplates\address.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f891c-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="f891c-130">Die neue Sicht enthält das folgende generierte Markup:</span><span class="sxs-lookup"><span data-stu-id="f891c-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="f891c-131">Führen Sie die Anwendung aus, und zeigen Sie die `PersonDetail` Ansicht an.</span><span class="sxs-lookup"><span data-stu-id="f891c-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="f891c-132">Dieses Mal wird die soeben erstellte `Address` Vorlage zum Anzeigen des komplexen `Address` Typs verwendet, sodass die Anzeige wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="f891c-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="f891c-133">Zusammenfassung: Möglichkeiten zum Angeben des Modell Anzeige Formats und der Vorlage</span><span class="sxs-lookup"><span data-stu-id="f891c-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="f891c-134">Sie haben gesehen, dass Sie das Format oder die Vorlage für eine Modell Eigenschaft mit den folgenden Ansätzen angeben können:</span><span class="sxs-lookup"><span data-stu-id="f891c-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="f891c-135">Anwenden des `DisplayFormat`-Attributs auf eine Eigenschaft im Modell.</span><span class="sxs-lookup"><span data-stu-id="f891c-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="f891c-136">Der folgende Code bewirkt beispielsweise, dass das Datum ohne die Zeit angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="f891c-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="f891c-137">Anwenden eines [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attributs auf eine Eigenschaft im Modell und angeben des Datentyps.</span><span class="sxs-lookup"><span data-stu-id="f891c-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="f891c-138">Der folgende Code bewirkt beispielsweise, dass das Datum ohne die Zeit angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="f891c-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="f891c-139">Wenn die Anwendung eine *Date. cshtml* -Vorlage im Ordner " *views\shared\displaytemplates* " oder im Ordner " *views\movies\displaytemplates* " enthält, wird diese Vorlage zum Rendering der `DateTime`-Eigenschaft verwendet.</span><span class="sxs-lookup"><span data-stu-id="f891c-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="f891c-140">Andernfalls zeigt das integrierte ASP.net-Vorlagen System die Eigenschaft als Datum an.</span><span class="sxs-lookup"><span data-stu-id="f891c-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="f891c-141">Erstellen einer Anzeige Vorlage im Ordner " *views\shared\displaytemplates* " oder im Ordner " *views\movies\displaytemplates* ", deren Name mit dem Datentyp übereinstimmt, den Sie formatieren möchten.</span><span class="sxs-lookup"><span data-stu-id="f891c-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="f891c-142">Beispielsweise haben Sie gesehen, dass " *views\shared\displaytemplates\datetime.cshtml* " verwendet wurde, um `DateTime` Eigenschaften in einem Modell zu erzeugen, ohne dem Modell ein Attribut hinzuzufügen und den Sichten keine Markup hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="f891c-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="f891c-143">Verwenden des [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) -Attributs für das Modell, um die Vorlage zum Anzeigen der Modell Eigenschaft anzugeben.</span><span class="sxs-lookup"><span data-stu-id="f891c-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="f891c-144">Explizites Hinzufügen des Namens der Anzeige Vorlage zum [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) -Befehl in einer Ansicht.</span><span class="sxs-lookup"><span data-stu-id="f891c-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="f891c-145">Welche Vorgehensweise Sie verwenden, hängt davon ab, was Sie in Ihrer Anwendung tun müssen.</span><span class="sxs-lookup"><span data-stu-id="f891c-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="f891c-146">Es ist nicht ungewöhnlich, diese Ansätze zu mischen, um genau die Art der Formatierung zu erzielen, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="f891c-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="f891c-147">Im nächsten Abschnitt wechseln Sie in ein Bit und wechseln von der Anpassung, wie Daten angezeigt werden, um anzupassen, wie Sie eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="f891c-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="f891c-148">Sie verbinden den jQuery-DatePicker mit den Bearbeitungs Sichten in der Anwendung, um eine schlanke Möglichkeit zum Angeben von Datumsangaben bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f891c-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f891c-149">[Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="f891c-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
