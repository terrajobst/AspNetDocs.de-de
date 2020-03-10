---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Erstellen eines Team Projekts in TFS | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellt wird.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519561"
---
# <a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="94ea9-103">Erstellen eines Teamprojekts in TFS</span><span class="sxs-lookup"><span data-stu-id="94ea9-103">Creating a Team Project in TFS</span></span>

<span data-ttu-id="94ea9-104">von [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="94ea9-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="94ea9-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="94ea9-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="94ea9-106">In diesem Thema wird beschrieben, wie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="94ea9-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>

<span data-ttu-id="94ea9-107">Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.</span><span class="sxs-lookup"><span data-stu-id="94ea9-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="94ea9-108">Aufgaben Übersicht</span><span class="sxs-lookup"><span data-stu-id="94ea9-108">Task Overview</span></span>

<span data-ttu-id="94ea9-109">Zum Bereitstellen und Verwenden eines neuen Teamprojekts in TFS müssen Sie die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="94ea9-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="94ea9-110">Erteilen Sie dem Benutzer, der das neue Teamprojekt erstellt, Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="94ea9-111">Erstellen Sie das Teamprojekt.</span><span class="sxs-lookup"><span data-stu-id="94ea9-111">Create the team project.</span></span>
- <span data-ttu-id="94ea9-112">Erteilen von Berechtigungen für Teammitglieder, die an dem Projekt arbeiten werden.</span><span class="sxs-lookup"><span data-stu-id="94ea9-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="94ea9-113">Überprüfen Sie einige Inhalte.</span><span class="sxs-lookup"><span data-stu-id="94ea9-113">Check in some content.</span></span>

<span data-ttu-id="94ea9-114">In diesem Thema wird erläutert, wie Sie diese Prozeduren ausführen, und es werden die Benutzer und Auftrags Rollen identifiziert, die wahrscheinlich für die einzelnen Prozeduren verantwortlich sind.</span><span class="sxs-lookup"><span data-stu-id="94ea9-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="94ea9-115">Beachten Sie, dass in Abhängigkeit von der Struktur Ihrer Organisation jede dieser Aufgaben die Verantwortung einer anderen Person haben kann.</span><span class="sxs-lookup"><span data-stu-id="94ea9-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="94ea9-116">Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie TFS installiert und konfiguriert haben und dass Sie im Rahmen des Konfigurations Vorgangs eine Teamprojekt Sammlung erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="94ea9-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="94ea9-117">Weitere Informationen zu diesen Annahmen und allgemeine Hintergrundinformationen zu diesem Szenario finden Sie unter [Konfigurieren eines TFS-Buildservers für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="94ea9-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="94ea9-118">Erteilen von Berechtigungen für den Team Projekt Ersteller</span><span class="sxs-lookup"><span data-stu-id="94ea9-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="94ea9-119">Um ein neues Teamprojekt zu erstellen, benötigen Sie diese Berechtigungen:</span><span class="sxs-lookup"><span data-stu-id="94ea9-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="94ea9-120">Sie müssen über die Berechtigung " **neue Projekte erstellen** " für die TFS-Anwendungsebene verfügen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="94ea9-121">Diese Berechtigung erteilen Sie in der Regel, indem Sie der TFS-Gruppe der **Projekt Sammlungs Administratoren** Benutzer hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="94ea9-122">Die globale Gruppe **Team Foundation-Administratoren** enthält auch diese Berechtigung.</span><span class="sxs-lookup"><span data-stu-id="94ea9-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="94ea9-123">Sie müssen über die Berechtigung zum Erstellen neuer Team Sites innerhalb der SharePoint-Website Sammlung verfügen, die der TFS-Teamprojekt Sammlung entspricht.</span><span class="sxs-lookup"><span data-stu-id="94ea9-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="94ea9-124">Sie erteilen diese Berechtigung in der Regel, indem Sie den Benutzer einer SharePoint-Gruppe mit **voll** Zugriffsrechten für die SharePoint-Website Sammlung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="94ea9-125">Wenn Sie SQL Server Reporting Services Features verwenden, müssen Sie Mitglied der Rolle " **Team Foundation-Inhalts-Manager** " in Reporting Services sein.</span><span class="sxs-lookup"><span data-stu-id="94ea9-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="94ea9-126">Wer führt diese Prozeduren aus?</span><span class="sxs-lookup"><span data-stu-id="94ea9-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="94ea9-127">In der Regel führt die Person oder Gruppe, die die TFS-Bereitstellung verwaltet, auch diese Verfahren aus.</span><span class="sxs-lookup"><span data-stu-id="94ea9-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="94ea9-128">Da es sich hierbei um einen Berechtigungs Satz mit hohen Berechtigungen handelt, werden neue Team Projekte in der Regel von einer kleinen Teilmenge von Benutzern erstellt, die für die Verwaltung einer TFS-Bereitstellung zuständig sind.</span><span class="sxs-lookup"><span data-stu-id="94ea9-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="94ea9-129">Entwicklern werden normalerweise nicht die Berechtigungen erteilt, die zum Erstellen neuer Team Projekte erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="94ea9-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="94ea9-130">Erteilen von Berechtigungen in TFS</span><span class="sxs-lookup"><span data-stu-id="94ea9-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="94ea9-131">Wenn Sie einem Benutzer das Erstellen neuer Team Projekte ermöglichen möchten, besteht die erste allgemeine Aufgabe darin, den Benutzer der Gruppe Projekt Auflistungs **Administratoren** für die Teamprojekt Sammlung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="94ea9-132">**So fügen Sie der Gruppe "Projekt Auflistungs Administratoren" einen Benutzer hinzu**</span><span class="sxs-lookup"><span data-stu-id="94ea9-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="94ea9-133">Zeigen Sie auf dem TFS-Server im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft Team Foundation Server 2010**, und klicken Sie dann auf **Team Foundation-Verwaltungskonsole**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="94ea9-134">Erweitern Sie in der Navigationsstruktur Ansicht den Knoten **Anwendungsebene**, und klicken Sie dann auf **Team Projekt Sammlungen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="94ea9-135">Wählen Sie im Bereich **Teamprojekt Sammlungen** die Teamprojekt Sammlung aus, die Sie verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="94ea9-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="94ea9-136">Klicken Sie auf der Registerkarte **Allgemein** auf **Gruppenmitgliedschaft**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="94ea9-137">Wählen Sie im Dialogfeld **globale Gruppen** die Gruppe **Projekt** Auflistungs Administratoren aus, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="94ea9-138">Wählen Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** die Option **Windows-Benutzer oder-Gruppe**aus, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="94ea9-139">Geben Sie im Dialogfeld **Benutzer, Computer oder Gruppen auswählen** den Benutzernamen des Benutzers ein, in dem Sie neue Team Projekte erstellen möchten, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="94ea9-140">Klicken Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="94ea9-141">Klicken Sie im Dialogfeld **globale Gruppen** auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="94ea9-142">Erteilen von Berechtigungen in SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="94ea9-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="94ea9-143">Als nächstes müssen Sie dem Benutzer die Berechtigung zum Erstellen neuer Team Sites in der SharePoint-Website Sammlung erteilen, die der TFS-Teamprojekt Sammlung entspricht.</span><span class="sxs-lookup"><span data-stu-id="94ea9-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="94ea9-144">**So erteilen Sie voll Zugriffsberechtigungen für die SharePoint-Website Sammlung**</span><span class="sxs-lookup"><span data-stu-id="94ea9-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="94ea9-145">Wählen Sie in der Team Foundation Server-Verwaltungskonsole auf der Seite **Teamprojekt Sammlungen** die Teamprojekt Sammlung aus, die Sie verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="94ea9-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="94ea9-146">Notieren Sie sich auf der Registerkarte **SharePoint-Website** den Wert der **aktuellen Standard Website** -URL.</span><span class="sxs-lookup"><span data-stu-id="94ea9-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="94ea9-147">Öffnen Sie Internet Explorer, und navigieren Sie dann zu der URL, die Sie in Schritt 2 notiert haben.</span><span class="sxs-lookup"><span data-stu-id="94ea9-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94ea9-148">Wenn Sie als Benutzer, der die Teamprojekt Sammlung erstellt hat, nicht bei Windows angemeldet sind, müssen Sie sich bei SharePoint als dieser Benutzer anmelden, um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="94ea9-149">Klicken Sie im Menü **Websiteaktionen** auf **Siteeinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="94ea9-150">Klicken Sie auf der Seite **Site Einstellungen** unter **Benutzer und Berechtigungen**auf **Personen und Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="94ea9-151">Klicken Sie im linken Navigationsbereich auf **Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="94ea9-152">Klicken Sie auf der Seite **Personen und Gruppen: alle Gruppen** auf **Gruppen für diesen Standort einrichten**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="94ea9-153">Aufgrund eines doppelten http-Codierungs Fehlers erhalten Sie möglicherweise die Fehlermeldung " <strong>HTTP 404 nicht gefunden</strong> ".</span><span class="sxs-lookup"><span data-stu-id="94ea9-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="94ea9-154">Ersetzen Sie in diesem Fall die URL durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="94ea9-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="94ea9-155">Beispiel: `[site_collection_URL]/_layouts/permsetup.aspx`</span><span class="sxs-lookup"><span data-stu-id="94ea9-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="94ea9-156">Fügen Sie auf der Seite **Gruppen für diese Site einrichten** den Benutzer, der Team Projekte erstellt, der Gruppe **Besitzer** hinzu, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="94ea9-157">Weitere Informationen zum Aktivieren von Benutzern für das Erstellen neuer Team Projekte in einer Teamprojekt Auflistung finden Sie unter [Festlegen von Administrator Berechtigungen für Teamprojekt Sammlungen](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="94ea9-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="94ea9-158">Erstellen eines neuen Team Projekts und Hinzufügen von Benutzern</span><span class="sxs-lookup"><span data-stu-id="94ea9-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="94ea9-159">Sobald Sie über die erforderlichen Berechtigungen verfügen, können Sie das **Team Explorer** Fenster in Visual Studio 2010 verwenden, um ein neues Team Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="94ea9-160">Dieser Ansatz stellt einen Assistenten bereit, der alle erforderlichen Informationen sammelt und die erforderlichen Aufgaben in TFS, SharePoint und SQL Server Reporting Services ausführt.</span><span class="sxs-lookup"><span data-stu-id="94ea9-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="94ea9-161">Außerdem müssen Sie den Mitgliedern des Entwicklerteams Berechtigungen für das neue Teamprojekt erteilen, damit Sie Inhalte hinzufügen und ändern können.</span><span class="sxs-lookup"><span data-stu-id="94ea9-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="94ea9-162">Wer führt diese Prozeduren aus?</span><span class="sxs-lookup"><span data-stu-id="94ea9-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="94ea9-163">In der Regel führt entweder ein TFS-Administrator oder ein Entwicklerteam Leiter diese Verfahren aus.</span><span class="sxs-lookup"><span data-stu-id="94ea9-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="94ea9-164">Neues Team Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="94ea9-164">Create a New Team Project</span></span>

<span data-ttu-id="94ea9-165">Im nächsten Verfahren wird das Erstellen eines neuen Teamprojekts in TFS 2010 beschrieben.</span><span class="sxs-lookup"><span data-stu-id="94ea9-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="94ea9-166">**So erstellen Sie ein neues Teamprojekt**</span><span class="sxs-lookup"><span data-stu-id="94ea9-166">**To create a new team project**</span></span>

1. <span data-ttu-id="94ea9-167">Zeigen Sie im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, klicken Sie mit der rechten Maustaste auf **Microsoft Visual Studio 2010**, und klicken Sie dann auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94ea9-168">Wenn Sie Visual Studio 2010 nicht als Administrator ausführen, schlägt der Assistent für neue Team Projekte im letzten Schritt fehl.</span><span class="sxs-lookup"><span data-stu-id="94ea9-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="94ea9-169">Wenn das Dialogfeld **Benutzerkontensteuerung** angezeigt wird, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="94ea9-170">Klicken Sie in Visual Studio im Menü **Team** auf **Verbindung mit Team Foundation Server herstellen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94ea9-171">Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 4-7 weglassen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="94ea9-172">Klicken Sie im Dialogfeld **Verbindung mit Team Projekt** auf **Server**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="94ea9-173">Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="94ea9-174">Geben Sie im Dialogfeld **Team Foundation Server hinzufügen** die Details der TFS-Instanz an, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="94ea9-175">Klicken Sie im Dialogfeld **Team Foundation Server hinzufügen/entfernen** auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="94ea9-176">Wählen Sie im Dialogfeld **mit Team Projekt verbinden** die TFS-Instanz aus, mit der Sie eine Verbindung herstellen möchten, wählen Sie die Team Projekt Sammlung aus, die Sie hinzufügen möchten, und klicken Sie dann auf **verbinden**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="94ea9-177">Klicken Sie im **Team Explorer** Fenster mit der rechten Maustaste auf die Teamprojekt Sammlung, und klicken Sie dann auf **Neues Team Projekt**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="94ea9-178">Geben Sie im Dialogfeld **Neues Team Projekt** einen Namen und eine Beschreibung für das Teamprojekt ein, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94ea9-179">Wenn das Teamprojekt Leerzeichen enthält, treten möglicherweise Probleme auf, wenn Sie das Webbereitstellungs Tool (Web deploy) Internetinformationsdienste (IIS) verwenden, um Pakete über den Ausgabepfad bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="94ea9-180">Leerzeichen im Pfad können das Ausführen Web deploy Befehle viel schwieriger machen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="94ea9-181">Wählen Sie auf der Seite **Prozess Vorlage auswählen** die Prozess Vorlage aus, die Sie zum Verwalten des Entwicklungsprozesses verwenden möchten, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94ea9-182">Weitere Informationen zu Prozessvorlagen für TFS finden Sie unter [Prozessvorlagen und-Tools](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="94ea9-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="94ea9-183">Überlassen Sie auf der Seite " **Team Site Einstellungen** " die Standardeinstellungen unverändert, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="94ea9-184">Mit dieser Einstellung wird eine SharePoint-Team Website erstellt oder identifiziert, die dem TFS-Teamprojekt zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="94ea9-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="94ea9-185">Das Entwicklungsteam kann diese Website zum Verwalten der Dokumentation, zur Teilnahme an Diskussions Threads, zum Erstellen von Wiki-Seiten und zum Ausführen verschiedener anderer Aufgaben verwenden, die nicht mit Code verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="94ea9-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="94ea9-186">Weitere Informationen finden Sie unter [Interaktionen zwischen SharePoint-Produkten und Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="94ea9-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="94ea9-187">Überlassen Sie auf der Seite Quell Code Verwaltungs **Einstellungen angeben** die Standardeinstellungen unverändert, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="94ea9-188">Mit dieser Einstellung wird der Speicherort in der TFS-Ordnerhierarchie identifiziert oder erstellt, der als Stamm Ordner für Ihre Inhalte fungiert.</span><span class="sxs-lookup"><span data-stu-id="94ea9-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="94ea9-189">Klicken Sie auf der Seite **Team Projekteinstellungen bestätigen** auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="94ea9-190">Wenn das neue Teamprojekt erfolgreich erstellt wurde, klicken Sie auf der Seite **Teamprojekt erstellt** auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="94ea9-191">Hinzufügen von Benutzern zu einem Team Projekt</span><span class="sxs-lookup"><span data-stu-id="94ea9-191">Add Users to a Team Project</span></span>

<span data-ttu-id="94ea9-192">Nachdem Sie das neue Teamprojekt erstellt haben, können Sie Benutzern Berechtigungen erteilen, damit Sie mit dem Hinzufügen und zusammenarbeiten von Inhalten beginnen können.</span><span class="sxs-lookup"><span data-stu-id="94ea9-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="94ea9-193">**So fügen Sie einem Teamprojekt Benutzer hinzu**</span><span class="sxs-lookup"><span data-stu-id="94ea9-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="94ea9-194">Klicken Sie in Visual Studio 2010 im Fenster **Team Explorer** mit der rechten Maustaste auf das Teamprojekt, zeigen Sie auf **Teamprojekt Einstellungen**, und klicken Sie dann auf **Gruppenmitgliedschaft**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="94ea9-195">Um es Benutzern zu ermöglichen, Code unter der Quell Code Verwaltung hinzuzufügen, zu ändern und zu entfernen, müssen Sie Sie der Gruppe **mitwirk** Ende hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="94ea9-196">Wählen Sie im Dialogfeld **Projektgruppen** die Gruppe **mitwirk** Ende aus, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="94ea9-197">Wählen Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** die Option **Windows-Benutzer oder-Gruppe**aus, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="94ea9-198">Geben Sie im Dialogfeld **Benutzer, Computer oder Gruppen auswählen** den Benutzernamen des Benutzers ein, den Sie dem Teamprojekt hinzufügen möchten, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="94ea9-199">Klicken Sie im Dialogfeld **Eigenschaften von Team Foundation Server Gruppe** auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="94ea9-200">Klicken Sie im Dialogfeld **Projektgruppen** auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="94ea9-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="94ea9-201">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="94ea9-201">Conclusion</span></span>

<span data-ttu-id="94ea9-202">An diesem Punkt ist Ihr neues Teamprojekt einsatzbereit, und Ihr Entwicklerteam kann mit dem Hinzufügen von Inhalten und der Zusammenarbeit am Entwicklungsprozess beginnen.</span><span class="sxs-lookup"><span data-stu-id="94ea9-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="94ea9-203">Im nächsten Thema, [Hinzufügen von Inhalt zur Quell](adding-content-to-source-control.md)Code Verwaltung, wird beschrieben, wie Inhalt zur Quell Code Verwaltung hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="94ea9-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="94ea9-204">Weitere nützliche Informationen</span><span class="sxs-lookup"><span data-stu-id="94ea9-204">Further Reading</span></span>

<span data-ttu-id="94ea9-205">Eine umfassendere Anleitung zum Erstellen von Team Projekten in TFS finden Sie unter [Erstellen eines Teamprojekts](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="94ea9-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="94ea9-206">Weitere Informationen zum Aktivieren von Benutzern für das Erstellen neuer Team Projekte in einer Teamprojekt Auflistung finden Sie unter [Festlegen von Administrator Berechtigungen für Teamprojekt Sammlungen](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="94ea9-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="94ea9-207">Weitere Informationen zum Hinzufügen von Benutzern zu Team Projekten finden [Sie unter Hinzufügen von Benutzern zu Team Projekten](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="94ea9-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="94ea9-208">[Zurück](configuring-team-foundation-server-for-web-deployment.md)
> [Weiter](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="94ea9-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
