---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Hinzufügen von Inhalt zur Quell Code Verwaltung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird erläutert, wie Sie der Quell Code Verwaltung Inhalt in Team Foundation Server (TFS) 2010 hinzufügen. Es wird beschrieben, wie Projektmappen und Projekten einem Teamprofil hinzugefügt werden...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515109"
---
# <a name="adding-content-to-source-control"></a><span data-ttu-id="45389-104">Hinzufügen von Inhalten zur Quellcodeverwaltung</span><span class="sxs-lookup"><span data-stu-id="45389-104">Adding Content to Source Control</span></span>

<span data-ttu-id="45389-105">von [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="45389-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="45389-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="45389-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="45389-107">In diesem Thema wird erläutert, wie Sie der Quell Code Verwaltung Inhalt in Team Foundation Server (TFS) 2010 hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="45389-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="45389-108">Es wird beschrieben, wie Sie Projektmappen und Projekte zu einem Teamprojekt in TFS hinzufügen. Außerdem wird erläutert, wie Sie der Quell Code Verwaltung externe Abhängigkeiten wie Frameworks oder Assemblys hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="45389-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>

<span data-ttu-id="45389-109">Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.</span><span class="sxs-lookup"><span data-stu-id="45389-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="45389-110">Aufgaben Übersicht</span><span class="sxs-lookup"><span data-stu-id="45389-110">Task Overview</span></span>

<span data-ttu-id="45389-111">In den meisten Fällen sollte jedes Mitglied des Entwicklerteams in der Lage sein, der Quell Code Verwaltung Inhalt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="45389-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="45389-112">Zum Hinzufügen einer Projekt Mappe zur Quell Code Verwaltung in TFS müssen Sie diese Grundschritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="45389-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="45389-113">Stellen Sie eine Verbindung mit einem Teamprojekt her.</span><span class="sxs-lookup"><span data-stu-id="45389-113">Connect to a team project.</span></span>
- <span data-ttu-id="45389-114">Ordnen Sie die Teamprojekt-Ordnerstruktur auf dem-Server einer Ordnerstruktur auf dem lokalen Computer zu.</span><span class="sxs-lookup"><span data-stu-id="45389-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="45389-115">Fügen Sie die Projekt Mappe und ihren Inhalt der Quell Code Verwaltung hinzu.</span><span class="sxs-lookup"><span data-stu-id="45389-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="45389-116">Fügen Sie externe Abhängigkeiten zur Quell Code Verwaltung hinzu.</span><span class="sxs-lookup"><span data-stu-id="45389-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="45389-117">In diesem Thema wird gezeigt, wie Sie diese Prozeduren ausführen.</span><span class="sxs-lookup"><span data-stu-id="45389-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="45389-118">In den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits ein neues TFS-Teamprojekt erstellt haben, um Ihre Inhalte zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="45389-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="45389-119">Weitere Informationen zum Erstellen eines neuen Teamprojekts finden Sie unter [Erstellen eines Teamprojekts in TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="45389-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="45389-120">Wer führt diese Prozeduren aus?</span><span class="sxs-lookup"><span data-stu-id="45389-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="45389-121">In den meisten Fällen sollte jedes Mitglied des Entwicklerteams in der Lage sein, Inhalte innerhalb bestimmter Team Projekte hinzuzufügen und zu ändern.</span><span class="sxs-lookup"><span data-stu-id="45389-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="45389-122">Herstellen einer Verbindung mit einem Team Projekt und Erstellen einer Ordner Zuordnung</span><span class="sxs-lookup"><span data-stu-id="45389-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="45389-123">Bevor Sie der Quell Code Verwaltung Inhalte hinzufügen, müssen Sie eine Verbindung mit einem Teamprojekt herstellen und eine Zuordnung zwischen der Ordnerstruktur auf dem Server und dem Dateisystem auf dem lokalen Computer erstellen.</span><span class="sxs-lookup"><span data-stu-id="45389-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="45389-124">**So stellen Sie eine Verbindung mit einem Teamprojekt her und ordnen einen lokalen Pfad zu**</span><span class="sxs-lookup"><span data-stu-id="45389-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="45389-125">Öffnen Sie Visual Studio 2010 auf Ihrer Entwickler Arbeitsstation.</span><span class="sxs-lookup"><span data-stu-id="45389-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="45389-126">Klicken Sie in Visual Studio im Menü **Team** auf **Verbindung mit Team Foundation Server herstellen**.</span><span class="sxs-lookup"><span data-stu-id="45389-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45389-127">Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 3-6 weglassen.</span><span class="sxs-lookup"><span data-stu-id="45389-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="45389-128">Klicken Sie im Dialogfeld **Verbindung mit Team Projekt** auf **Server**.</span><span class="sxs-lookup"><span data-stu-id="45389-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="45389-129">Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="45389-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="45389-130">Geben Sie im Dialogfeld **Team Foundation Server hinzufügen** die Details der TFS-Instanz an, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="45389-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="45389-131">Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="45389-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="45389-132">Wählen Sie im Dialogfeld **mit Team Projekt verbinden** die TFS-Instanz aus, mit der Sie eine Verbindung herstellen möchten, wählen Sie die Teamprojekt Sammlung aus, wählen Sie das Teamprojekt aus, das Sie hinzufügen möchten, und klicken Sie dann auf **verbinden**.</span><span class="sxs-lookup"><span data-stu-id="45389-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="45389-133">Erweitern Sie im Fenster **Team Explorer** das Team Projekt, und doppelklicken Sie dann auf **Quell**Code Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="45389-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="45389-134">Klicken Sie auf der Registerkarte **Quellcodeverwaltungs-Explorer** auf **nicht zugeordnet**.</span><span class="sxs-lookup"><span data-stu-id="45389-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="45389-135">Navigieren Sie **im Dialogfeld** Zuordnung im Feld **lokaler Ordner** zu einem lokalen Ordner, der als Stamm Ordner für das Teamprojekt fungieren soll, **und klicken Sie**dann auf zuordnen.</span><span class="sxs-lookup"><span data-stu-id="45389-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="45389-136">Wenn Sie zum Herunterladen der Quelldateien aufgefordert werden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="45389-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="45389-137">An diesem Punkt haben Sie den serverseitigen Ordner für das Teamprojekt einem lokalen Ordner auf Ihrer Entwickler Arbeitsstation zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="45389-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="45389-138">Sie haben auch vorhandene Inhalte aus dem Teamprojekt in Ihre lokale Ordnerstruktur heruntergeladen.</span><span class="sxs-lookup"><span data-stu-id="45389-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="45389-139">Sie können jetzt mit dem Hinzufügen Ihres eigenen Inhalts zur Quell Code Verwaltung beginnen.</span><span class="sxs-lookup"><span data-stu-id="45389-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="45389-140">Hinzufügen von Projekten und Projektmappen zur Quell Code Verwaltung</span><span class="sxs-lookup"><span data-stu-id="45389-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="45389-141">Wenn Sie Projekte und Projektmappen zur Quell Code Verwaltung hinzufügen möchten, müssen Sie Sie zuerst in den zugeordneten Ordner für das Teamprojekt auf dem lokalen Computer verschieben.</span><span class="sxs-lookup"><span data-stu-id="45389-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="45389-142">Sie können dann den Inhalt einchecken, um die Ergänzungen mit dem Server zu synchronisieren.</span><span class="sxs-lookup"><span data-stu-id="45389-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="45389-143">**So fügen Sie Projekte zur Quell Code Verwaltung hinzu**</span><span class="sxs-lookup"><span data-stu-id="45389-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="45389-144">Verschieben Sie Ihre Projekte und Projektmappen auf der Arbeitsstation des Entwicklers an eine geeignete Position in der zugeordneten Ordnerstruktur für das Teamprojekt.</span><span class="sxs-lookup"><span data-stu-id="45389-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45389-145">Viele Organisationen haben einen bevorzugten Ansatz, um zu erfahren, wie Projekte und Projektmappen in der Quell Code Verwaltung organisiert werden sollten.</span><span class="sxs-lookup"><span data-stu-id="45389-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="45389-146">Anleitungen zum Strukturieren von Ordnern finden Sie unter Gewusst [wie: Strukturieren der Quell Code Verwaltungs Ordner in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="45389-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="45389-147">Öffnen Sie die Projekt Mappe in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="45389-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="45389-148">Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf die Projekt Mappe, und klicken Sie dann auf Projekt Mappe **zur Quell Code Verwaltung hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="45389-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="45389-149">In einigen Fällen müssen Sie, je nachdem, wie Ihre Organisation Inhalte in TFS strukturiert, möglicherweise Projekte zur Quell Code Verwaltung einzeln hinzufügen, um eine präzisere Kontrolle über die Organisation des Quellcodes zu bieten.</span><span class="sxs-lookup"><span data-stu-id="45389-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="45389-150">Vergewissern Sie sich, dass die Registerkarte **Quellcodeverwaltungs-Explorer** den Inhalt anzeigt, den Sie in der Server Ordnerstruktur für das Teamprojekt hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="45389-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="45389-151">Auf der Registerkarte **Quellcodeverwaltungs-Explorer** werden Ihre Inhalte ohne weitere Aufforderung angezeigt, weil Sie die Projekt Mappe einem zugeordneten Ordner im lokalen Dateisystem hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="45389-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="45389-152">Wenn sich Ihre Lösung an einem nicht zugeordneten Speicherort befand, werden Sie aufgefordert, Ordner Speicherorte sowohl in TFS als auch in Ihrem lokalen Dateisystem anzugeben.</span><span class="sxs-lookup"><span data-stu-id="45389-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="45389-153">Klicken Sie auf der Registerkarte **Quellcodeverwaltungs-Explorer** im **Ordner** Fenster mit der rechten Maustaste auf das Teamprojekt (z. **b. ContactManager**), und klicken Sie dann auf **Ausstehende Änderungen einchecken**.</span><span class="sxs-lookup"><span data-stu-id="45389-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="45389-154">Geben Sie im Dialogfeld **Einchecken – Quelldateien** einen Kommentar ein, und klicken Sie dann auf **Einchecken**.</span><span class="sxs-lookup"><span data-stu-id="45389-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="45389-155">An diesem Punkt haben Sie die Projekt Mappe der Quell Code Verwaltung in TFS hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="45389-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="45389-156">Externe Abhängigkeiten zur Quell Code Verwaltung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="45389-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="45389-157">Wenn Sie ein Projekt oder eine Projekt Mappe zur Quell Code Verwaltung hinzufügen, werden alle Dateien und Ordner in Ihrem Projekt oder ihrer Projekt Mappe ebenfalls hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="45389-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="45389-158">In vielen Fällen basieren Projekte und Projektmappen aber auch auf externen Abhängigkeiten, wie z. b. lokalen Assemblys, um ordnungsgemäß zu funktionieren.</span><span class="sxs-lookup"><span data-stu-id="45389-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="45389-159">Sie müssen diese Ressourcen der Quell Code Verwaltung hinzufügen, damit sowohl der Team Build als auch andere Mitglieder des Entwicklerteams den Code erfolgreich erstellen können.</span><span class="sxs-lookup"><span data-stu-id="45389-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="45389-160">Die Ordnerstruktur für die Beispiel Projekt Mappe Contact Manager enthält z. b. einen Ordner mit dem Namen Packages.</span><span class="sxs-lookup"><span data-stu-id="45389-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="45389-161">Diese enthält die Assembly und verschiedene unterstützende Ressourcen für den ADO.NET-Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="45389-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="45389-162">Der Ordner "Packages" ist nicht Teil der Contact Manager-Lösung, aber die Lösung wird ohne diese nicht erfolgreich erstellt.</span><span class="sxs-lookup"><span data-stu-id="45389-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="45389-163">Um Team Build zum Erstellen der Projekt Mappe zu aktivieren, müssen Sie den Paket Ordner der Quell Code Verwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="45389-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="45389-164">Die Einbindung eines Paket Ordners ist typisch für das, was geschieht, wenn Sie der Projekt Mappe mit der nuget-Erweiterung für Visual Studio 2010 die Entity Framework oder ähnliche Ressourcen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="45389-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>

<span data-ttu-id="45389-165">**So fügen Sie der Quell Code Verwaltung nicht-Projektinhalte hinzu**</span><span class="sxs-lookup"><span data-stu-id="45389-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="45389-166">Stellen Sie sicher, dass sich die Elemente, die Sie hinzufügen möchten (z. b. der Ordner Pakete), an einem geeigneten Speicherort innerhalb eines zugeordneten Ordners auf dem lokalen Dateisystem befinden</span><span class="sxs-lookup"><span data-stu-id="45389-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="45389-167">Erweitern Sie in Visual Studio 2010 im Fenster **Team Explorer** das Team Projekt, und doppelklicken Sie dann auf **Quell**Code Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="45389-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="45389-168">Wählen Sie auf der Registerkarte **Quellcodeverwaltungs-Explorer** im **Ordner Ordner** den Ordner aus, der die Elemente enthält, die Sie hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="45389-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="45389-169">Klicken Sie auf die Schaltfläche **Elemente zu Ordner hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="45389-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="45389-170">Wählen Sie im Dialogfeld **zur Quell Code Verwaltung hinzufügen** den Ordner oder die Elemente aus, die Sie hinzufügen möchten, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="45389-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="45389-171">Wählen Sie auf der Registerkarte **ausgeschlossene Elemente** alle erforderlichen Elemente aus, die automatisch ausgeschlossen wurden (z. b. Assemblys), und klicken Sie dann auf **Element (e) einschließen**.</span><span class="sxs-lookup"><span data-stu-id="45389-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="45389-172">Vergewissern Sie sich, dass auf der Registerkarte hinzu zufügende **Elemente** alle Dateien aufgelistet sind, die Sie einschließen möchten, und klicken Sie dann auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="45389-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="45389-173">Klicken Sie im **Quellcodeverwaltungs-Explorer** Fenster auf die Schaltfläche **Einchecken** .</span><span class="sxs-lookup"><span data-stu-id="45389-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="45389-174">Geben Sie im Dialogfeld **Einchecken – Quelldateien** einen Kommentar ein, und klicken Sie dann auf **Einchecken**.</span><span class="sxs-lookup"><span data-stu-id="45389-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="45389-175">An diesem Punkt haben Sie die externen Abhängigkeiten für die Projekt Mappe zur Quell Code Verwaltung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="45389-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="45389-176">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="45389-176">Conclusion</span></span>

<span data-ttu-id="45389-177">In diesem Thema wird beschrieben, wie Sie eine Verbindung mit einem Teamprojekt herstellen, eine Ordnerstruktur zuordnen und der Quell Code Verwaltung Inhalt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="45389-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="45389-178">Weitere Informationen zum Arbeiten mit Elementen unter Quell Code Verwaltung finden [Sie unter Verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="45389-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="45389-179">Im nächsten Thema [Konfigurieren eines TFS-Buildservers für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md)wird beschrieben, wie Sie einen TFS Team Build-Server zum Erstellen und Bereitstellen Ihrer Lösung vorbereiten.</span><span class="sxs-lookup"><span data-stu-id="45389-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="45389-180">Weitere nützliche Informationen</span><span class="sxs-lookup"><span data-stu-id="45389-180">Further Reading</span></span>

<span data-ttu-id="45389-181">Ausführlichere Informationen zum Arbeiten mit der Quell Code Verwaltung in TFS finden [Sie unter Verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="45389-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45389-182">[Zurück](creating-a-team-project-in-tfs.md)
> [Weiter](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="45389-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
