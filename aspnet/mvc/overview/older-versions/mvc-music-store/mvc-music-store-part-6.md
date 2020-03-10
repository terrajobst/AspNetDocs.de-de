---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Teil 6: Verwenden von Daten Anmerkungen für die Modell Validierung | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 6 umfasst die Verwendung von Daten Anmerkungen für Modell V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433533"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="c45ab-104">Teil 6: Verwenden von Daten Anmerkungen für die Modell Validierung</span><span class="sxs-lookup"><span data-stu-id="c45ab-104">Part 6: Using Data Annotations for Model Validation</span></span>

<span data-ttu-id="c45ab-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c45ab-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c45ab-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c45ab-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c45ab-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="c45ab-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c45ab-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="c45ab-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c45ab-109">In Teil 6 wird die Verwendung von Daten Anmerkungen für die Modell Validierung behandelt.</span><span class="sxs-lookup"><span data-stu-id="c45ab-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>

<span data-ttu-id="c45ab-110">Das Erstellungs-und Bearbeitungs Formular hat ein großes Problem: Sie führen keine Validierung durch.</span><span class="sxs-lookup"><span data-stu-id="c45ab-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="c45ab-111">Wir können z. b. die Pflichtfelder leer lassen oder Buchstaben im Feld Price eingeben. der erste Fehler, den wir sehen werden, ist die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c45ab-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="c45ab-112">Wir können unserer Anwendung problemlos eine Validierung hinzufügen, indem wir unseren Modellklassen Daten Anmerkungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c45ab-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="c45ab-113">Mit Daten Anmerkungen können wir die Regeln beschreiben, die auf unsere Modell Eigenschaften angewendet werden sollen, und ASP.NET MVC übernimmt die Erzwingung und zeigt die entsprechenden Nachrichten für unsere Benutzer an.</span><span class="sxs-lookup"><span data-stu-id="c45ab-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="c45ab-114">Hinzufügen von Validierungen zu unseren Album Formularen</span><span class="sxs-lookup"><span data-stu-id="c45ab-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="c45ab-115">Wir verwenden die folgenden Daten Anmerkungen-Attribute:</span><span class="sxs-lookup"><span data-stu-id="c45ab-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="c45ab-116">**Required** – gibt an, dass die Eigenschaft ein Pflichtfeld ist.</span><span class="sxs-lookup"><span data-stu-id="c45ab-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="c45ab-117">**Display Name** – definiert den Text, der für Formularfelder und Validierungs Nachrichten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="c45ab-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="c45ab-118">**StringLength** – definiert eine maximale Länge für ein Zeichen folgen Feld.</span><span class="sxs-lookup"><span data-stu-id="c45ab-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="c45ab-119">**Range** – gibt einen maximalen und minimalen Wert für ein numerisches Feld an.</span><span class="sxs-lookup"><span data-stu-id="c45ab-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="c45ab-120">**Bind** – listet Felder auf, die beim Binden von Parameter-oder Formular Werten an Modell Eigenschaften ausgeschlossen oder eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c45ab-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="c45ab-121">**Gerüst Column** – ermöglicht das Ausblenden von Feldern aus Editor Formularen</span><span class="sxs-lookup"><span data-stu-id="c45ab-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="c45ab-122">*Hinweis: Weitere Informationen zur Modell Validierung mithilfe von Attributen für die Daten Anmerkung finden Sie in der MSDN-Dokumentation unter* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="c45ab-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="c45ab-123">Öffnen Sie die Album-Klasse, und fügen Sie die folgenden *using* -Anweisungen am Anfang hinzu.</span><span class="sxs-lookup"><span data-stu-id="c45ab-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="c45ab-124">Aktualisieren Sie anschließend die Eigenschaften, um die Anzeige-und Validierungs Attribute hinzuzufügen, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="c45ab-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="c45ab-125">Während wir dort sind, haben wir auch das Genre und den Künstler in Virtual Properties geändert.</span><span class="sxs-lookup"><span data-stu-id="c45ab-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="c45ab-126">Dadurch können Entity Framework diese ggf. verzögert laden.</span><span class="sxs-lookup"><span data-stu-id="c45ab-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="c45ab-127">Nachdem diese Attribute dem Album Modell hinzugefügt wurden, beginnt der Bildschirm zum Erstellen und bearbeiten sofort mit der Überprüfung der Felder und der Verwendung der ausgewählten anzeigen Amen (z. b. "Album Kunst-URL" anstelle von "albumarturl").</span><span class="sxs-lookup"><span data-stu-id="c45ab-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="c45ab-128">Führen Sie die Anwendung aus, und navigieren Sie zu/StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="c45ab-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="c45ab-129">Als nächstes unterbrechen wir einige Validierungsregeln.</span><span class="sxs-lookup"><span data-stu-id="c45ab-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="c45ab-130">Geben Sie einen Preis von 0 ein, und lassen Sie den Titel leer.</span><span class="sxs-lookup"><span data-stu-id="c45ab-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="c45ab-131">Wenn Sie auf die Schaltfläche "erstellen" klicken, wird das Formular mit Validierungs Fehlermeldungen angezeigt, das anzeigt, welche Felder die von uns definierten Validierungsregeln nicht erfüllen.</span><span class="sxs-lookup"><span data-stu-id="c45ab-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="c45ab-132">Testen der Client seitigen Validierung</span><span class="sxs-lookup"><span data-stu-id="c45ab-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="c45ab-133">Die Server seitige Validierung ist aus Anwendungs Sicht sehr wichtig, da Benutzer die Client seitige Validierung umgehen können.</span><span class="sxs-lookup"><span data-stu-id="c45ab-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="c45ab-134">Webseiten Formulare, die nur serverseitige Validierung implementieren, weisen jedoch drei bedeutende Probleme auf.</span><span class="sxs-lookup"><span data-stu-id="c45ab-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="c45ab-135">Der Benutzer muss warten, bis das Formular gepostet, auf dem Server überprüft und die Antwort an den Browser gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="c45ab-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="c45ab-136">Der Benutzer erhält kein sofortiges Feedback, wenn er ein Feld korrigiert, sodass es nun die Validierungsregeln übergibt.</span><span class="sxs-lookup"><span data-stu-id="c45ab-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="c45ab-137">Wir verschwenden Server Ressourcen, um Validierungs Logik auszuführen, anstatt den Browser des Benutzers zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="c45ab-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="c45ab-138">Glücklicherweise ist für die ASP.NET MVC 3-Gerüst Vorlagen die Client seitige Validierung integriert, sodass keine zusätzliche Arbeit erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="c45ab-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="c45ab-139">Wenn Sie einen einzelnen Buchstaben im Feld "Title" eingeben, werden die Überprüfungsanforderungen erfüllt, sodass die Validierungs Meldung sofort entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="c45ab-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c45ab-140">[Zurück](mvc-music-store-part-5.md)
> [Weiter](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="c45ab-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
