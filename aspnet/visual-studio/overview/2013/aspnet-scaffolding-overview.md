---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: ASP.net Gerüstbau in Visual Studio 2013 | Microsoft-Dokumentation
author: Rick-Anderson
description: ASP.net Gerüstbau ist ein neues Feature, das in Visual Studio 2013 enthalten ist.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449565"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="5d3b2-103">ASP.NET-Gerüstbau in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5d3b2-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>

<span data-ttu-id="5d3b2-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5d3b2-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5d3b2-105">ASP.net Gerüstbau ist ein neues Feature, das in Visual Studio 2013 enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>

## <a name="overview"></a><span data-ttu-id="5d3b2-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5d3b2-106">Overview</span></span>

<span data-ttu-id="5d3b2-107">ASP.net Gerüstbau ist ein Code Generierungs Framework für ASP.NET-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="5d3b2-108">Visual Studio 2013 enthält vorinstallierte Code Generatoren für MVC-und Web-API-Projekte.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="5d3b2-109">Sie fügen dem Projekt Gerüstbau hinzu, wenn Sie schnell Code hinzufügen möchten, der mit Datenmodellen interagiert.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="5d3b2-110">Die Verwendung von Gerüstbau kann den Zeitaufwand für die Entwicklung von Standarddaten Vorgängen in Ihrem Projekt verkürzen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="5d3b2-111">Standardmäßig unterstützt Visual Studio 2013 das Erstellen von Code für ein Web Forms Projekt nicht, aber Sie können Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt entweder MVC-Abhängigkeiten hinzufügen oder eine Erweiterung installieren.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="5d3b2-112">Beide Ansätze sind unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-112">Both approaches are shown below.</span></span>

<span data-ttu-id="5d3b2-113">Visual Studio 2013 Update 2 (derzeit RC) bietet die Möglichkeit, ASP.net-Gerüstbau zu erweitern, um die Anforderungen Ihres Szenarios zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="5d3b2-114">Mit dieser Funktion können Sie eine angepasste Gerüst Vorlage erstellen und hinzufügen, um das Dialogfeld Neues Gerüst hinzufügen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="5d3b2-115">In der angepassten Vorlage geben Sie den Code an, der generiert wird, wenn Sie ein Gerüst Element hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="5d3b2-116">Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Gerüst für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="5d3b2-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d3b2-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5d3b2-117">Prerequisites</span></span>

<span data-ttu-id="5d3b2-118">Um ASP.net Gerüstbau verwenden zu können, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5d3b2-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="5d3b2-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5d3b2-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="5d3b2-120">WebEntwicklertools (Teil der standardmäßigen Visual Studio 2013 Installation)</span><span class="sxs-lookup"><span data-stu-id="5d3b2-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="5d3b2-121">ASP.net Web Frameworks und Tools 2013 (Bestandteil der Standard Visual Studio 2013 Installation)</span><span class="sxs-lookup"><span data-stu-id="5d3b2-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="5d3b2-122">Hinzufügen eines Gerüsts zu MVC oder Web-API</span><span class="sxs-lookup"><span data-stu-id="5d3b2-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="5d3b2-123">Wenn Sie ein Gerüst hinzufügen möchten, klicken Sie mit der rechten Maustaste entweder auf das Projekt oder einen Ordner innerhalb des Projekts, und wählen Sie **Hinzufügen** – **Neues Gerüstbau Element**aus, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Gerüst Element hinzufügen](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="5d3b2-125">Wählen Sie im Fenster **Gerüst hinzufügen** den Objekttyp aus, den Sie hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Typ des Gerüsts auswählen](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="5d3b2-127">Das Fenster **Controller hinzufügen** bietet Ihnen die Möglichkeit, Optionen zum Erstellen des Controllers auszuwählen. Dies umfasst auch, ob Sie die neuen Async-Funktionen von Entity Framework 6 verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Controller hinzufügen](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="5d3b2-129">Die relevanten Klassen und Seiten werden für Ihr Szenario erstellt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="5d3b2-130">Die folgende Abbildung zeigt z. b. den MVC-Controller und Sichten, die über Gerüstbau für eine Modell Klasse mit dem Namen Filme erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Die erstellten Dateien](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="5d3b2-132">Fügen Sie ein Gerüst Element Web Forms</span><span class="sxs-lookup"><span data-stu-id="5d3b2-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="5d3b2-133">Zum Hinzufügen von Gerüstbau, der Web Forms Code generiert, müssen Sie entweder eine Erweiterung in Visual Studio installieren oder MVC-Abhängigkeiten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="5d3b2-134">Beide Ansätze sind unten dargestellt, aber Sie müssen nur einen dieser Ansätze durchführen.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="5d3b2-135">Erweiterung des Web Forms Gerüsts</span><span class="sxs-lookup"><span data-stu-id="5d3b2-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="5d3b2-136">Sie können eine Visual Studio-Erweiterung installieren, die es Ihnen ermöglicht, Gerüstbau mit einem Web Forms Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="5d3b2-137">**Wählen Sie** in Visual Studio Extras und dann **Erweiterungen und Updates**aus.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="5d3b2-138">In diesem Dialogfeld Suchen Sie in der Visual Studio Gallery nach **Web Forms Gerüstbau**.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![Web Forms-Gerüst installieren](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="5d3b2-140">Weitere Informationen finden Sie unter [Web Forms Gerüstbau](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="5d3b2-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="5d3b2-141">MVC-Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="5d3b2-141">MVC Dependencies</span></span>

<span data-ttu-id="5d3b2-142">Wenn Sie MVC-Abhängigkeiten hinzufügen möchten, wählen Sie - **Neues Gerüst Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="5d3b2-143">Wählen Sie im Fenster Gerüst hinzufügen die Option **MVC-Abhängigkeiten**aus, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![MVC-Abhängigkeiten hinzufügen](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="5d3b2-145">Es gibt zwei Optionen für das Gerüstbau von MVC: Minimal und vollständig.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="5d3b2-146">Wenn Sie minimal auswählen, werden nur die nuget-Pakete und-Verweise für ASP.NET MVC dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="5d3b2-147">Wenn Sie die Option vollständig auswählen, werden die minimalen Abhängigkeiten sowie die erforderlichen Inhalts Dateien für ein MVC-Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="5d3b2-148">Um das Gerüst problemlos zu verwenden, wählen Sie vollständige Abhängigkeiten aus.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-148">To easily use scaffolding, select Full dependencies.</span></span>

![vollständige Abhängigkeiten auswählen](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="5d3b2-150">Nachdem Sie die Abhängigkeiten hinzugefügt haben, wird die Datei "Infodatei **. txt** " angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="5d3b2-151">Befolgen Sie die Anweisungen in dieser Datei, um sicherzustellen, dass das Projekt ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="5d3b2-152">Wenn Sie die Schritte in der Datei "Infodatei. txt" ausgeführt haben, können Sie ein neues Gerüsts hinzufügen, wie im vorherigen Abschnitt über MVC und die Web-API gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="5d3b2-153">Die automatisch generierten Sichten und der Controller funktionieren in Ihrem Projekt ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="5d3b2-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="5d3b2-154">Lernprogramme</span><span class="sxs-lookup"><span data-stu-id="5d3b2-154">Tutorials</span></span>

<span data-ttu-id="5d3b2-155">Informationen zum Erstellen eines angepassten Gerüstes finden Sie unter [Erstellen eines benutzerdefinierten Gerüstes für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="5d3b2-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="5d3b2-156">Informationen zum Anpassen der generierten Dateien finden Sie unter [Anpassen der generierten Dateien aus dem Dialogfeld "neues Gerüst Element](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)".</span><span class="sxs-lookup"><span data-stu-id="5d3b2-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="5d3b2-157">Ein Beispiel für die Verwendung von Gerüstbau mit **Database First Entwicklung**finden Sie unter [EF Database First mit ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="5d3b2-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="5d3b2-158">Ein Beispiel für die Verwendung von Gerüstbau in einem **MVC** -Projekt finden Sie unter [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="5d3b2-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="5d3b2-159">Ein Beispiel für die Verwendung von Gerüstbau in einem **Web-API** -Projekt finden Sie unter [Erstellen einer Rest-API mit Attribut Routing in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="5d3b2-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
