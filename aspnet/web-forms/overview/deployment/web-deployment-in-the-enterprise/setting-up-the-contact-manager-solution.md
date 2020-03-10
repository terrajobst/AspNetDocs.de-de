---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Einrichten der Contact Manager-Lösung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie die Contact Manager-Lösung herunterladen und konfigurieren, um lokal auf einer Entwickler Arbeitsstation auszuführen.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511161"
---
# <a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="a3e32-103">Einrichten der Contact Manager-Lösung</span><span class="sxs-lookup"><span data-stu-id="a3e32-103">Setting Up the Contact Manager Solution</span></span>

<span data-ttu-id="a3e32-104">von [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a3e32-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a3e32-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="a3e32-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a3e32-106">In diesem Thema wird beschrieben, wie Sie die Contact Manager-Lösung herunterladen und konfigurieren, um lokal auf einer Entwickler Arbeitsstation auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>

## <a name="system-requirements"></a><span data-ttu-id="a3e32-107">Systemanforderungen</span><span class="sxs-lookup"><span data-stu-id="a3e32-107">System Requirements</span></span>

<span data-ttu-id="a3e32-108">Wenn Sie die Contact Manager-Lösung lokal ausführen und die anderen in diesem Tutorial beschriebenen Aufgaben ausführen möchten, müssen Sie diese Software auf Ihrer Entwickler Arbeitsstation installieren:</span><span class="sxs-lookup"><span data-stu-id="a3e32-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="a3e32-109">Visual Studio 2010 Service Pack 1, Premium oder Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="a3e32-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="a3e32-110">Internetinformationsdienste (IIS) 7,5 Express</span><span class="sxs-lookup"><span data-stu-id="a3e32-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="a3e32-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="a3e32-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="a3e32-112">IIS-Webbereitstellungs Tool (Web deploy) 2,1 oder höher</span><span class="sxs-lookup"><span data-stu-id="a3e32-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="a3e32-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="a3e32-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="a3e32-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="a3e32-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="a3e32-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="a3e32-115">.NET Framework 4</span></span>
- <span data-ttu-id="a3e32-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="a3e32-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="a3e32-117">Mit Ausnahme von Visual Studio 2010 können Sie die neuesten Versionen aller dieser Produkte und Komponenten über den [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118)herunterladen und installieren.</span><span class="sxs-lookup"><span data-stu-id="a3e32-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="a3e32-118">Herunterladen und Extrahieren der Lösung</span><span class="sxs-lookup"><span data-stu-id="a3e32-118">Download and Extract the Solution</span></span>

<span data-ttu-id="a3e32-119">Sie können die Beispielanwendung Contact Manager aus der MSDN Code Gallery [hier](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="a3e32-120">Konfigurieren und Ausführen der Lösung</span><span class="sxs-lookup"><span data-stu-id="a3e32-120">Configure and Run the Solution</span></span>

<span data-ttu-id="a3e32-121">Zum Konfigurieren und Ausführen der Contact Manager-Lösung auf dem lokalen Computer müssen Sie die folgenden Grundschritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="a3e32-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="a3e32-122">Wenn Sie noch nicht über eins verfügen, erstellen Sie eine lokale ASP.NET-Anwendungs Dienst Datenbank mit aktivierten Mitgliedschafts-und Rollen Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="a3e32-123">Bearbeiten Sie Verbindungs Zeichenfolgen in den *Web. config* -Dateien so, dass Sie auf Ihre lokale SQL Server Express Instanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="a3e32-124">Führen Sie die Projekt Mappe aus Visual Studio 2010 aus.</span><span class="sxs-lookup"><span data-stu-id="a3e32-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="a3e32-125">Im restlichen Teil dieses Abschnitts finden Sie weitere Anleitungen zum Ausführen dieser Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="a3e32-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="a3e32-126">**So erstellen Sie die Anwendungs Dienst Datenbank**</span><span class="sxs-lookup"><span data-stu-id="a3e32-126">**To create the application services database**</span></span>

1. <span data-ttu-id="a3e32-127">Öffnen Sie eine Visual Studio 2010-Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="a3e32-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="a3e32-128">Zeigen Sie dazu im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, klicken Sie auf **Visual Studio-Tools**, und klicken Sie dann auf **Visual Studio-Eingabeaufforderung (2010)** .</span><span class="sxs-lookup"><span data-stu-id="a3e32-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="a3e32-129">Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, und drücken Sie dann die EINGABETASTE:</span><span class="sxs-lookup"><span data-stu-id="a3e32-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="a3e32-130">Verwenden Sie den Schalter **– C** , um die Verbindungs Zeichenfolge für den Datenbankserver anzugeben.</span><span class="sxs-lookup"><span data-stu-id="a3e32-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="a3e32-131">Verwenden Sie den Schalter **– A** , um die Anwendungs Dienst Features anzugeben, die Sie der Datenbank hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="a3e32-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="a3e32-132">In diesem Fall gibt **m** an, dass Sie Unterstützung für den Mitgliedschafts Anbieter hinzufügen möchten, und **r** gibt an, dass Sie Unterstützung für den Rollen-Manager hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="a3e32-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="a3e32-133">Verwenden Sie den Schalter **– d** , um einen Namen für die Anwendungs Dienst Datenbank anzugeben.</span><span class="sxs-lookup"><span data-stu-id="a3e32-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="a3e32-134">Wenn Sie diesen Schalter weglassen, erstellt das Hilfsprogramm eine Datenbank mit dem Standardnamen **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="a3e32-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="a3e32-135">Wenn die Datenbank erfolgreich erstellt wurde, wird in der Eingabeaufforderung eine Bestätigung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3e32-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="a3e32-136">Weitere Informationen zum ASPNET\_RegSql-Hilfsprogramm finden Sie unter [ASP.NET SQL Server Registration Tool (ASPNET\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="a3e32-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>

<span data-ttu-id="a3e32-137">Im nächsten Schritt müssen Sie sicherstellen, dass die Verbindungs Zeichenfolgen in der Contact Manager-Lösung auf Ihre lokale Instanz von SQL Server Express verweisen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="a3e32-138">**So aktualisieren Sie die Verbindungs Zeichenfolgen**</span><span class="sxs-lookup"><span data-stu-id="a3e32-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="a3e32-139">Öffnen Sie die Contact Manager-Lösung in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a3e32-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="a3e32-140">Erweitern Sie im Fenster **Projektmappen-Explorer** das Projekt **ContactManager. MVC** , und doppelklicken Sie dann auf den Knoten **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="a3e32-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a3e32-141">Das ContactManager. MVC-Projekt enthält zwei *Web. config* -Dateien.</span><span class="sxs-lookup"><span data-stu-id="a3e32-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="a3e32-142">Sie müssen die Datei auf Projektebene bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="a3e32-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="a3e32-143">Überprüfen Sie im **connectionStrings** -Element, ob die Verbindungs Zeichenfolge mit dem Namen **ApplicationServices** auf Ihre lokale ASP.NET-Anwendungs Dienst Datenbank verweist.</span><span class="sxs-lookup"><span data-stu-id="a3e32-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="a3e32-144">Erweitern Sie im Fenster **Projektmappen-Explorer** das Projekt **ContactManager. Service** , und doppelklicken Sie dann auf den Knoten **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="a3e32-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="a3e32-145">Überprüfen Sie im **connectionStrings** -Element in der Verbindungs Zeichenfolge mit dem Namen **contactmanagercontext**, ob die **Datenquellen** Eigenschaft auf Ihre lokale Instanz von SQL Server Express festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="a3e32-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="a3e32-146">Sie müssen nichts anderes in der Verbindungs Zeichenfolge ändern.</span><span class="sxs-lookup"><span data-stu-id="a3e32-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="a3e32-147">Speichern Sie alle geöffneten Dateien.</span><span class="sxs-lookup"><span data-stu-id="a3e32-147">Save all open files.</span></span>

<span data-ttu-id="a3e32-148">Jetzt sollten Sie bereit sein, die Contact Manager-Lösung auf dem lokalen Computer auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="a3e32-149">Wenn Sie diese Schritte ausführen, ohne zuerst eine Anwendungs Dienst-Datenbank zu erstellen, erstellt ASP.net die Datenbank, wenn Sie zum ersten Mal versuchen, einen Benutzer zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3e32-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="a3e32-150">Wenn Sie die Datenbank manuell erstellen, haben Sie jedoch viel mehr Kontrolle über die Anwendungs Dienst-Funktionsgruppe, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="a3e32-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>

<span data-ttu-id="a3e32-151">**So führen Sie die Contact Manager-Lösung aus**</span><span class="sxs-lookup"><span data-stu-id="a3e32-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="a3e32-152">Drücken Sie in Visual Studio 2010 die Taste F5.</span><span class="sxs-lookup"><span data-stu-id="a3e32-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="a3e32-153">Internet Explorer wird gestartet und fordert die URL der Anwendung Contact Manager ASP.NET MVC 3 an.</span><span class="sxs-lookup"><span data-stu-id="a3e32-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="a3e32-154">Standardmäßig wird von der Anwendung die Seite **alle Kontakte** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3e32-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="a3e32-155">Fügen Sie einige Kontakte hinzu, und überprüfen Sie dann, ob die Anwendung erwartungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="a3e32-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="a3e32-156">Navigieren Sie zu `http://localhost:50114/Account/Register` (passen Sie die URL an, wenn Sie die Anwendung auf einem anderen Port verwenden).</span><span class="sxs-lookup"><span data-stu-id="a3e32-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="a3e32-157">Fügen Sie einen Benutzernamen, eine e-Mail-Adresse und ein Kennwort hinzu, und überprüfen Sie, ob ein Konto erfolgreich registriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a3e32-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="a3e32-158">Navigieren Sie zu `http://localhost:50114/Account/LogOn` (passen Sie die URL an, wenn Sie die Anwendung auf einem anderen Port verwenden).</span><span class="sxs-lookup"><span data-stu-id="a3e32-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="a3e32-159">Vergewissern Sie sich, dass Sie sich mit dem soeben erstellten Konto anmelden können.</span><span class="sxs-lookup"><span data-stu-id="a3e32-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="a3e32-160">Schließen Sie Internet Explorer, um das Debugging zu beenden.</span><span class="sxs-lookup"><span data-stu-id="a3e32-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a3e32-161">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a3e32-161">Conclusion</span></span>

<span data-ttu-id="a3e32-162">An diesem Punkt sollte die Contact Manager-Lösung vollständig so konfiguriert sein, dass Sie auf dem lokalen Computer ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3e32-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="a3e32-163">Sie können die Lösung als Referenz verwenden, wenn Sie die anderen Themen in diesem Tutorial durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="a3e32-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="a3e32-164">Im nächsten Thema, Grundlegendes [zur Projektdatei](understanding-the-project-file.md), wird erläutert, wie Sie die Projektdateien für benutzerdefinierte Microsoft-Build-Engine (MSBuild) in der Contact Manager-Lösung verwenden können, um den Bereitstellungs Prozess zu steuern.</span><span class="sxs-lookup"><span data-stu-id="a3e32-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3e32-165">[Zurück](the-contact-manager-solution.md)
> [Weiter](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="a3e32-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
