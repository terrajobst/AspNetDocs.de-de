---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: Anpassen der Ansicht für EF-Database First mit ASP.NET MVC-App'
description: Dieses Tutorial konzentriert sich auf das Ändern der automatisch generierten Sichten, um die Darstellung zu verbessern.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471519"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="b397c-103">Tutorial: Anpassen der Ansicht für EF-Database First mit ASP.NET MVC-App</span><span class="sxs-lookup"><span data-stu-id="b397c-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="b397c-104">Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b397c-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b397c-105">In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können.</span><span class="sxs-lookup"><span data-stu-id="b397c-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b397c-106">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="b397c-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="b397c-107">Dieses Tutorial konzentriert sich auf das Ändern der automatisch generierten Sichten, um die Darstellung zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="b397c-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="b397c-108">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="b397c-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b397c-109">Hinzufügen von Kursen zur Seite "Student Detail"</span><span class="sxs-lookup"><span data-stu-id="b397c-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="b397c-110">Vergewissern Sie sich, dass die Kurse der Seite hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b397c-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b397c-111">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b397c-111">Prerequisites</span></span>

* [<span data-ttu-id="b397c-112">Ändern der Datenbank</span><span class="sxs-lookup"><span data-stu-id="b397c-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="b397c-113">Hinzufügen von Kursen zu Studenten Details</span><span class="sxs-lookup"><span data-stu-id="b397c-113">Add courses to student detail</span></span>

<span data-ttu-id="b397c-114">Der generierte Code bietet einen guten Ausgangspunkt für Ihre Anwendung, bietet jedoch nicht unbedingt die gesamte Funktionalität, die Sie in Ihrer Anwendung benötigen.</span><span class="sxs-lookup"><span data-stu-id="b397c-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="b397c-115">Sie können den Code anpassen, um die speziellen Anforderungen Ihrer Anwendung zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="b397c-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="b397c-116">Derzeit werden in der Anwendung die registrierten Kurse für den ausgewählten Studenten nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b397c-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="b397c-117">In diesem Abschnitt fügen Sie der **Detail** Ansicht für die Schüler/Studenten die registrierten Kurse für jeden Schüler/Studenten hinzu.</span><span class="sxs-lookup"><span data-stu-id="b397c-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="b397c-118">Öffnen Sie **views** > **Students** > *Details. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b397c-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="b397c-119">Fügen Sie unter dem letzten &lt;/DL&gt; Tag, aber vor dem schließenden &lt;/div-&gt;-Tag den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="b397c-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="b397c-120">Mit diesem Code wird eine Tabelle erstellt, in der eine Zeile für jeden Datensatz in der Registrierungs Tabelle für den ausgewählten Studenten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b397c-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="b397c-121">Die **Anzeige** Methode rendert HTML für das-Objekt (mudelitem), das den Ausdruck darstellt.</span><span class="sxs-lookup"><span data-stu-id="b397c-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="b397c-122">Sie verwenden die Anzeige Methode (anstatt einfach den Eigenschafts Wert in den Code einzubetten), um sicherzustellen, dass der Wert basierend auf dem Typ und der Vorlage für diesen Typ ordnungsgemäß formatiert ist.</span><span class="sxs-lookup"><span data-stu-id="b397c-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="b397c-123">In diesem Beispiel gibt jeder Ausdruck eine einzelne Eigenschaft aus dem aktuellen Datensatz in der Schleife zurück, und die Werte sind primitive Typen, die als Text gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="b397c-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="b397c-124">Bestätigen, dass Kurse hinzugefügt werden</span><span class="sxs-lookup"><span data-stu-id="b397c-124">Confirm courses are added</span></span>

<span data-ttu-id="b397c-125">Führen Sie die Lösung aus.</span><span class="sxs-lookup"><span data-stu-id="b397c-125">Run the solution.</span></span> <span data-ttu-id="b397c-126">Klicken Sie auf **Liste der Schüler/Studenten** , und wählen Sie **Details** für einen der Studenten aus.</span><span class="sxs-lookup"><span data-stu-id="b397c-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="b397c-127">Sie werden sehen, dass die registrierten Kurse in der Ansicht enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="b397c-127">You will see the enrolled courses have been included in the view.</span></span>

![Student mit Registrierung](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="b397c-129">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b397c-129">Next steps</span></span>
<span data-ttu-id="b397c-130">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="b397c-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b397c-131">Hinzufügen von Kursen zur Seite "Student Detail"</span><span class="sxs-lookup"><span data-stu-id="b397c-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="b397c-132">Es wurde bestätigt, dass die Kurse der Seite hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b397c-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="b397c-133">Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Daten Anmerkungen hinzufügen, um Validierungsanforderungen anzugeben und die Formatierung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b397c-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b397c-134">Datenvalidierung verbessern</span><span class="sxs-lookup"><span data-stu-id="b397c-134">Enhance data validation</span></span>](enhancing-data-validation.md)
