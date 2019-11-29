---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Befehlszeilen Bereitstellung | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74634206"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="d8f80-103">ASP.net-Webbereitstellung mithilfe von Visual Studio: Befehlszeilen Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="d8f80-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>

<span data-ttu-id="d8f80-104">von [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d8f80-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d8f80-105">Starter Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="d8f80-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="d8f80-106">In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="d8f80-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="d8f80-107">Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d8f80-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="d8f80-108">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="d8f80-108">Overview</span></span>

<span data-ttu-id="d8f80-109">In diesem Tutorial wird gezeigt, wie Sie die Visual Studio-Webveröffentlichungs Pipeline von der Befehlszeile aus aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="d8f80-110">Dies ist nützlich für Szenarien, in denen Sie [den Bereitstellungs Prozess automatisieren](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) möchten, anstatt ihn manuell in Visual Studio zu verwenden, in der Regel mit einem [Quell Code Versionskontrollsystem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="d8f80-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="d8f80-111">Vornehmen einer Änderung an der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="d8f80-111">Make a change to deploy</span></span>

<span data-ttu-id="d8f80-112">Die Seite "Info" zeigt derzeit den Vorlagen Code an.</span><span class="sxs-lookup"><span data-stu-id="d8f80-112">Currently the About page displays the template code.</span></span>

![Info Seite mit Vorlagen Code](command-line-deployment/_static/image1.png)

<span data-ttu-id="d8f80-114">Ersetzen Sie dies durch Code, der eine Zusammenfassung der Schüler-/Studentenregistrierung anzeigt.</span><span class="sxs-lookup"><span data-stu-id="d8f80-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="d8f80-115">Öffnen Sie die Seite *about. aspx* , löschen Sie das gesamte Markup innerhalb des `MainContent` `Content`-Elements, und fügen Sie das folgende Markup an der Stelle ein:</span><span class="sxs-lookup"><span data-stu-id="d8f80-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="d8f80-116">Führen Sie das Projekt aus, und wählen Sie **die Seite Info** aus.</span><span class="sxs-lookup"><span data-stu-id="d8f80-116">Run the project and select the **About** page.</span></span>

![Seite „Info“](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="d8f80-118">Bereitstellen für Tests über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="d8f80-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="d8f80-119">Sie werden keine andere Daten Bank Änderung bereitstellen, daher deaktivieren Sie die dbdacfx-Daten Bank Bereitstellung für die ASPNET-contosouniversity-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d8f80-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="d8f80-120">Öffnen Sie den Assistenten **Web veröffentlichen** , und deaktivieren Sie in jedem der drei Veröffentlichungs Profile das Kontrollkästchen **Datenbank aktualisieren** auf der Registerkarte **Einstellungen** .</span><span class="sxs-lookup"><span data-stu-id="d8f80-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="d8f80-121">Suchen Sie auf der Start Seite von Windows 8 nach **Developer-Eingabeaufforderung für VS2012**.</span><span class="sxs-lookup"><span data-stu-id="d8f80-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="d8f80-122">Klicken Sie mit der rechten Maustaste auf das Symbol für **Developer-Eingabeaufforderung für VS2012** , und klicken Sie auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="d8f80-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="d8f80-123">Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und ersetzen Sie dabei den Pfad zur Projektmappendatei durch den Pfad zu ihrer Projektmappendatei:</span><span class="sxs-lookup"><span data-stu-id="d8f80-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="d8f80-124">MSBuild erstellt die Lösung und stellt Sie in der Testumgebung bereit.</span><span class="sxs-lookup"><span data-stu-id="d8f80-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Befehlszeilenausgabe](command-line-deployment/_static/image3.png)

<span data-ttu-id="d8f80-126">Öffnen Sie einen Browser, navigieren Sie zu `http://localhost/ContosoUniversity`, und klicken Sie dann **auf die Seite Info, um sicher** zustellen, dass die Bereitstellung erfolgreich war</span><span class="sxs-lookup"><span data-stu-id="d8f80-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="d8f80-127">Wenn Sie im Test keine Studenten erstellt haben, wird unter der Überschrift " **Student Body Statistics** " eine leere Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d8f80-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="d8f80-128">Wechseln Sie **zur Seite "** **Studenten** ", klicken Sie auf " **Student hinzufügen**", und fügen Sie einige Schüler und Studenten hinzu.</span><span class="sxs-lookup"><span data-stu-id="d8f80-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Info Seite in Test Umgebung](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="d8f80-130">Wichtige Befehlszeilenoptionen</span><span class="sxs-lookup"><span data-stu-id="d8f80-130">Key command line options</span></span>

<span data-ttu-id="d8f80-131">Der von Ihnen eingegebene Befehl hat den projektmappendateipfad und zwei Eigenschaften an MSBuild übermittelt:</span><span class="sxs-lookup"><span data-stu-id="d8f80-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="d8f80-132">Bereitstellen der Lösung im Vergleich zur Bereitstellung einzelner Projekte</span><span class="sxs-lookup"><span data-stu-id="d8f80-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="d8f80-133">Das Angeben der Projektmappendatei bewirkt, dass alle Projekte in der Projekt Mappe erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="d8f80-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="d8f80-134">Wenn Sie mehrere Webprojekte in der Projekt Mappe haben, gilt das folgende MSBuild-Verhalten:</span><span class="sxs-lookup"><span data-stu-id="d8f80-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="d8f80-135">Die Eigenschaften, die Sie in der Befehlszeile angeben, werden an jedes Projekt weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="d8f80-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="d8f80-136">Daher muss jedes Webprojekt über ein Veröffentlichungs Profil mit dem von Ihnen angegebenen Namen verfügen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="d8f80-137">Wenn Sie `/p:PublishProfile=Test`angeben, muss jedes Webprojekt über ein Veröffentlichungs Profil mit dem Namen " *Test*" verfügen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="d8f80-138">Sie können ein Projekt erfolgreich veröffentlichen, wenn ein anderes Projekt nicht gerade erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d8f80-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="d8f80-139">Weitere Informationen finden Sie unter Fehler bei MSBuild für den StackOverflow-Thread [mit zwei Paketen](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="d8f80-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="d8f80-140">Wenn Sie anstelle einer Projekt Mappe ein einzelnes Projekt angeben, müssen Sie einen Parameter hinzufügen, der die Visual Studio-Version angibt.</span><span class="sxs-lookup"><span data-stu-id="d8f80-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="d8f80-141">Wenn Sie Visual Studio 2012 verwenden, sieht die Befehlszeile in etwa wie im folgenden Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="d8f80-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="d8f80-142">Die Versionsnummer für Visual Studio 2010 ist 10,0.</span><span class="sxs-lookup"><span data-stu-id="d8f80-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="d8f80-143">Weitere Informationen finden Sie unter [Visual Studio-Projekt Kompatibilität und visualstudioversion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) für den Sayed Hashimi-Blog.</span><span class="sxs-lookup"><span data-stu-id="d8f80-143">For more information, see [Visual Studio project compatibility and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="d8f80-144">Angeben des Veröffentlichungs Profils</span><span class="sxs-lookup"><span data-stu-id="d8f80-144">Specifying the publish profile</span></span>

<span data-ttu-id="d8f80-145">Sie können das Veröffentlichungs Profil anhand des Namens oder des vollständigen Pfads der *pubxml* -Datei angeben, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="d8f80-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="d8f80-146">Webveröffentlichungs Methoden, die für die Befehlszeilen Veröffentlichung unterstützt werden</span><span class="sxs-lookup"><span data-stu-id="d8f80-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="d8f80-147">Drei Veröffentlichungs Methoden werden für die Veröffentlichung von Befehlszeilen unterstützt:</span><span class="sxs-lookup"><span data-stu-id="d8f80-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="d8f80-148">`MSDeploy` mit Web deploy veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="d8f80-149">`Package`-Veröffentlichung durch Erstellen eines Web deploy Pakets.</span><span class="sxs-lookup"><span data-stu-id="d8f80-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="d8f80-150">Sie müssen das Paket getrennt vom MSBuild-Befehl installieren, von dem es erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d8f80-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="d8f80-151">`FileSystem`-Veröffentlichung durch Kopieren von Dateien in einen angegebenen Ordner.</span><span class="sxs-lookup"><span data-stu-id="d8f80-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="d8f80-152">Angeben der Buildkonfiguration und-Plattform</span><span class="sxs-lookup"><span data-stu-id="d8f80-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="d8f80-153">Die Buildkonfiguration und-Plattform müssen in Visual Studio oder in der Befehlszeile festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="d8f80-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="d8f80-154">Die Veröffentlichungs Profile enthalten Eigenschaften mit dem Namen `LastUsedBuildConfiguration` und `LastUsedPlatform`. Sie können diese Eigenschaften jedoch nicht festlegen, um zu bestimmen, wie das Projekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d8f80-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="d8f80-155">Weitere Informationen finden Sie unter [MSBuild: So legen Sie die Konfigurations Eigenschaft für den](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Blog von Sayed Hashimi fest.</span><span class="sxs-lookup"><span data-stu-id="d8f80-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="d8f80-156">Bereitstellung für Staging</span><span class="sxs-lookup"><span data-stu-id="d8f80-156">Deploy to staging</span></span>

<span data-ttu-id="d8f80-157">Zum Bereitstellen in Azure müssen Sie das Kennwort der Befehlszeile hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="d8f80-158">Wenn Sie das Kennwort im Veröffentlichungs Profil in Visual Studio gespeichert haben, wurde es in verschlüsselter Form in der *pubxml. User* -Datei gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d8f80-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="d8f80-159">Der Zugriff auf diese Datei erfolgt nicht durch MSBuild, wenn Sie eine Befehlszeilen Bereitstellung ausführen, sodass Sie das Kennwort in einem Befehlszeilenparameter übergeben müssen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="d8f80-160">Kopieren Sie das benötigte Kennwort aus der *publishsettings* -Datei, die Sie zuvor für die Stagingwebsite heruntergeladen haben.</span><span class="sxs-lookup"><span data-stu-id="d8f80-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="d8f80-161">Das Kennwort ist der Wert des `userPWD`-Attributs für das Web deploy `publishProfile`-Element.</span><span class="sxs-lookup"><span data-stu-id="d8f80-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web deploy Kennwort](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="d8f80-163">Suchen Sie auf der Start Seite von Windows 8 nach **Developer-Eingabeaufforderung für VS2012**, und klicken Sie auf das Symbol, um die Eingabeaufforderung zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="d8f80-164">(Sie müssen diese Zeit nicht als Administrator öffnen, weil Sie nicht auf IIS auf dem lokalen Computer bereitstellen.)</span><span class="sxs-lookup"><span data-stu-id="d8f80-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="d8f80-165">Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und ersetzen Sie dabei den Pfad zur Projektmappendatei durch den Pfad zu ihrer Projektmappendatei und das Kennwort durch Ihr Kennwort:</span><span class="sxs-lookup"><span data-stu-id="d8f80-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="d8f80-166">Beachten Sie, dass diese Befehlszeile einen zusätzlichen Parameter enthält: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="d8f80-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="d8f80-167">Wenn dieses Lernprogramm geschrieben wird, muss die `AllowUntrustedCertificate`-Eigenschaft festgelegt werden, wenn Sie über die Befehlszeile in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="d8f80-168">Wenn die Behebung für diesen fehlerfrei gegeben wird, benötigen Sie diesen Parameter nicht.</span><span class="sxs-lookup"><span data-stu-id="d8f80-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="d8f80-169">Öffnen Sie einen Browser, navigieren Sie zur URL Ihrer Stagingwebsite, und klicken Sie dann auf die Seite Info, um **zu überprüfen** , ob die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="d8f80-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="d8f80-170">Wie Sie bereits gesehen haben, müssen Sie möglicherweise einige Studenten erstellen **, um Statistiken auf der Seite** "Info" anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d8f80-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="d8f80-171">In Produktionsumgebungen bereitstellen</span><span class="sxs-lookup"><span data-stu-id="d8f80-171">Deploy to production</span></span>

<span data-ttu-id="d8f80-172">Der Prozess für die Bereitstellung in der Produktion ähnelt dem Stagingprozess.</span><span class="sxs-lookup"><span data-stu-id="d8f80-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="d8f80-173">Kopieren Sie das benötigte Kennwort aus der Datei " *. publishsettings* ", die Sie zuvor für die Produktions Website heruntergeladen haben.</span><span class="sxs-lookup"><span data-stu-id="d8f80-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="d8f80-174">Öffnen Sie **Developer-Eingabeaufforderung für VS2012**.</span><span class="sxs-lookup"><span data-stu-id="d8f80-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="d8f80-175">Geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und ersetzen Sie dabei den Pfad zur Projektmappendatei durch den Pfad zu ihrer Projektmappendatei und das Kennwort durch Ihr Kennwort:</span><span class="sxs-lookup"><span data-stu-id="d8f80-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="d8f80-176">Bei einer realen Produktions Site kopieren Sie in der Regel vor der Bereitstellung die APP\_Datei " *Offline. htm* " auf die Website, und löschen Sie Sie nach erfolgreicher Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="d8f80-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="d8f80-177">Öffnen Sie einen Browser, navigieren Sie zur URL Ihrer Stagingwebsite, und klicken Sie dann auf die Seite Info, um **zu überprüfen** , ob die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="d8f80-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="d8f80-178">Summary</span><span class="sxs-lookup"><span data-stu-id="d8f80-178">Summary</span></span>

<span data-ttu-id="d8f80-179">Sie haben nun ein Anwendungs Update über die Befehlszeile bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="d8f80-179">You have now deployed an application update by using the command line.</span></span>

![Info Seite in Test Umgebung](command-line-deployment/_static/image6.png)

<span data-ttu-id="d8f80-181">Im nächsten Tutorial sehen Sie ein Beispiel zum Erweitern der Webveröffentlichungs Pipeline.</span><span class="sxs-lookup"><span data-stu-id="d8f80-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="d8f80-182">Im Beispiel wird gezeigt, wie Sie Dateien bereitstellen, die nicht im Projekt enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="d8f80-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8f80-183">[Zurück](deploying-a-database-update.md)
> [Weiter](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="d8f80-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
