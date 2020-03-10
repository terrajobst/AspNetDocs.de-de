---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Teil 6: ASP.NET Mitgliedschaft | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 6 fügt die ASP.NET-Mitgliedschaft hinzu.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454881"
---
# <a name="part-6-aspnet-membership"></a><span data-ttu-id="0c551-104">Teil 6: ASP.NET Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="0c551-104">Part 6: ASP.NET Membership</span></span>

<span data-ttu-id="0c551-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0c551-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0c551-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0c551-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0c551-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="0c551-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0c551-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0c551-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0c551-109">Teil 6 fügt die ASP.NET-Mitgliedschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="0c551-109">Part 6 adds ASP.NET Membership.</span></span>

## <a id="_Toc260221672"></a><span data-ttu-id="0c551-110">Arbeiten mit der ASP.NET-Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="0c551-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="0c551-111">Klicken Sie auf Sicherheit</span><span class="sxs-lookup"><span data-stu-id="0c551-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="0c551-112">Stellen Sie sicher, dass die Formular Authentifizierung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0c551-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="0c551-113">Verwenden Sie den Link "Benutzer erstellen", um einige Benutzer zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0c551-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="0c551-114">Wenn Sie den Vorgang abgeschlossen haben, lesen Sie den Projektmappen-Explorer Fenster, und aktualisieren Sie die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="0c551-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="0c551-115">Beachten Sie, dass aspnetdb ist. Es wurde eine feine MDF-Erstellung erstellt.</span><span class="sxs-lookup"><span data-stu-id="0c551-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="0c551-116">Diese Datei enthält die Tabellen, die die ASP.net-Kerndienste wie die Mitgliedschaft unterstützen.</span><span class="sxs-lookup"><span data-stu-id="0c551-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="0c551-117">Nun können wir mit der Implementierung des Checkout Prozesses beginnen.</span><span class="sxs-lookup"><span data-stu-id="0c551-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="0c551-118">Erstellen Sie zunächst eine Seite "Checkout. aspx".</span><span class="sxs-lookup"><span data-stu-id="0c551-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="0c551-119">Die Seite "Checkout. aspx" sollte nur für angemeldete Benutzer verfügbar sein, sodass der Zugriff auf angemeldete Benutzer eingeschränkt wird und Benutzer, die nicht auf der Anmeldeseite angemeldet sind, umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="0c551-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="0c551-120">Zu diesem Zweck fügen wir dem Konfigurations Abschnitt der Datei "Web. config" Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="0c551-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="0c551-121">Die Vorlage für ASP.net Web Forms-Anwendungen hat der Datei "Web. config" automatisch einen Authentifizierungs Abschnitt hinzugefügt und die Standard Anmeldeseite eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="0c551-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="0c551-122">Wir müssen die Code Behind-Datei "Login. aspx" so ändern, dass ein anonymer Warenkorb migriert wird, wenn sich der Benutzer anmeldet.</span><span class="sxs-lookup"><span data-stu-id="0c551-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="0c551-123">Ändern Sie die Seite\_Lade Ereignis wie folgt.</span><span class="sxs-lookup"><span data-stu-id="0c551-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="0c551-124">Fügen Sie dann einen LoggedIn-Ereignishandler wie diesen hinzu, um den Sitzungs Namen auf den neu angemeldeten Benutzer festzulegen, und ändern Sie die temporäre Sitzungs-ID im Warenkorb in die des Benutzers, indem Sie die MigrateCart-Methode in der myshoppingcart-Klasse aufrufen.</span><span class="sxs-lookup"><span data-stu-id="0c551-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="0c551-125">(In der CS-Datei implementiert)</span><span class="sxs-lookup"><span data-stu-id="0c551-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="0c551-126">Implementieren Sie die MigrateCart ()-Methode wie folgt.</span><span class="sxs-lookup"><span data-stu-id="0c551-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="0c551-127">In "Checkout. aspx" verwenden wir eine EntityDataSource und eine GridView auf unserer Seite "Auschecken", wie auf unserer Warenkorb-Seite.</span><span class="sxs-lookup"><span data-stu-id="0c551-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="0c551-128">Beachten Sie, dass unser GridView-Steuerelement einen "ondat."-Ereignishandler mit dem Namen "myList"\_rowdatungebundene angibt. wir implementieren diesen Ereignishandler wie folgt.</span><span class="sxs-lookup"><span data-stu-id="0c551-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="0c551-129">Diese Methode behält einen laufenden Gesamtbetrag des Warenkorb bei, wenn jede Zeile gebunden ist, und aktualisiert die untere Zeile der GridView.</span><span class="sxs-lookup"><span data-stu-id="0c551-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="0c551-130">In dieser Phase haben wir eine "Review"-Präsentation der zu platzierenden Reihenfolge implementiert.</span><span class="sxs-lookup"><span data-stu-id="0c551-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="0c551-131">Wir behandeln ein leeres Wagen Szenario, indem wir der Seite\_Lade Ereignisses einige Codezeilen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="0c551-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="0c551-132">Wenn der Benutzer auf die Schaltfläche "Senden" klickt, wird der folgende Code im Click-Ereignishandler der Schaltfläche "Senden" ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="0c551-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="0c551-133">Das "Fleisch" des Auftrags Übermittlungs Vorgangs muss in der SubmitOrder ()-Methode der myshoppingcart-Klasse implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="0c551-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="0c551-134">SubmitOrder führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="0c551-134">SubmitOrder will:</span></span>

- <span data-ttu-id="0c551-135">Nehmen Sie alle Zeilen Elemente im Warenkorb, und verwenden Sie Sie, um einen neuen Bestelldaten Satz und die zugehörigen Order Details-Einträge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0c551-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="0c551-136">Versanddatum berechnen.</span><span class="sxs-lookup"><span data-stu-id="0c551-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="0c551-137">Löschen Sie den Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="0c551-137">Clear the shopping cart.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="0c551-138">Im Rahmen dieser Beispielanwendung berechnen wir ein Lieferdatum, indem wir einfach zwei Tage zum aktuellen Datum hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0c551-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="0c551-139">Wenn Sie die Anwendung jetzt ausführen, können wir den Einkaufsprozess von Anfang bis Ende testen.</span><span class="sxs-lookup"><span data-stu-id="0c551-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c551-140">[Zurück](tailspin-spyworks-part-5.md)
> [Weiter](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="0c551-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
