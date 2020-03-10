---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Erkennung der owin-Startklasse | Microsoft-Dokumentation
author: Praburaj
description: In diesem Tutorial wird gezeigt, wie Sie konfigurieren, welche owin-Startklasse geladen wird. Weitere Informationen zu owin finden Sie unter Übersicht über Project Katana. Dieses Tutorial war...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500181"
---
# <a name="owin-startup-class-detection"></a><span data-ttu-id="7ca49-105">Erkennung der OWIN-Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="7ca49-105">OWIN Startup Class Detection</span></span>

> <span data-ttu-id="7ca49-106">In diesem Tutorial wird gezeigt, wie Sie konfigurieren, welche owin-Startklasse geladen wird.</span><span class="sxs-lookup"><span data-stu-id="7ca49-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="7ca49-107">Weitere Informationen zu owin finden Sie unter [Übersicht über Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="7ca49-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="7ca49-108">Dieses Tutorial wurde von Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), praburaj Thiagarajan und Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ) verfasst.</span><span class="sxs-lookup"><span data-stu-id="7ca49-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="7ca49-109">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="7ca49-109">Prerequisites</span></span>
>
> [<span data-ttu-id="7ca49-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7ca49-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a><span data-ttu-id="7ca49-111">Erkennung der OWIN-Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="7ca49-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="7ca49-112">Jede owin-Anwendung verfügt über eine Startup-Klasse, in der Sie Komponenten für die Anwendungs Pipeline angeben.</span><span class="sxs-lookup"><span data-stu-id="7ca49-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="7ca49-113">Es gibt verschiedene Möglichkeiten, wie Sie Ihre Startup-Klasse mit der Laufzeit verbinden können, je nachdem, welches Hostingmodell Sie auswählen (owinhost, IIS und IIS-Express).</span><span class="sxs-lookup"><span data-stu-id="7ca49-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="7ca49-114">Die in diesem Tutorial gezeigte Startup-Klasse kann in jeder Hostinganwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7ca49-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="7ca49-115">Sie verbinden die Startup-Klasse mit der Hostinglaufzeit, indem Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="7ca49-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="7ca49-116">**Benennungs Konvention**: Katana sucht nach einer Klasse mit dem Namen `Startup` im Namespace, der mit dem Assemblynamen oder dem globalen Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="7ca49-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="7ca49-117">**Owinstartup-Attribut**: Dies ist der Ansatz, den die meisten Entwickler ergreifen, um die Startup-Klasse anzugeben.</span><span class="sxs-lookup"><span data-stu-id="7ca49-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="7ca49-118">Mit dem folgenden Attribut wird die Startup-Klasse auf die `TestStartup`-Klasse im `StartupDemo`-Namespace festgelegt.</span><span class="sxs-lookup"><span data-stu-id="7ca49-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="7ca49-119">Das `OwinStartup`-Attribut überschreibt die Benennungs Konvention.</span><span class="sxs-lookup"><span data-stu-id="7ca49-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="7ca49-120">Sie können einen anzeigen Amen auch mit diesem Attribut angeben. Wenn Sie jedoch einen anzeigen Amen verwenden, müssen Sie auch das `appSetting`-Element in der Konfigurationsdatei verwenden.</span><span class="sxs-lookup"><span data-stu-id="7ca49-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="7ca49-121">**Das appSetting-Element in der Konfigurationsdatei**: das `appSetting`-Element überschreibt das `OwinStartup` Attribut und die Benennungs Konvention.</span><span class="sxs-lookup"><span data-stu-id="7ca49-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="7ca49-122">Sie können über mehrere Startklassen verfügen (jeweils mit einem `OwinStartup`-Attribut) und konfigurieren, welche Startklasse in eine Konfigurationsdatei geladen wird, indem Sie ein Markup ähnlich dem folgenden verwenden:</span><span class="sxs-lookup"><span data-stu-id="7ca49-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="7ca49-123">Der folgende Schlüssel, der explizit die Startup-Klasse und die Assembly angibt, kann ebenfalls verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="7ca49-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="7ca49-124">Der folgende XML-Code in der Konfigurationsdatei gibt einen anzeigen Amen für die Startklasse `ProductionConfiguration`an.</span><span class="sxs-lookup"><span data-stu-id="7ca49-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="7ca49-125">Das obige Markup muss mit dem folgenden `OwinStartup` Attribute verwendet werden, das einen anzeigen Amen angibt und bewirkt, dass die `ProductionStartup2` Klasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7ca49-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="7ca49-126">Fügen Sie zum Deaktivieren der owin-Start Ermittlung die `appSetting owin:AutomaticAppStartup` mit dem Wert `"false"` in der Datei "Web. config" hinzu.</span><span class="sxs-lookup"><span data-stu-id="7ca49-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="7ca49-127">Erstellen einer ASP.net-Web-App mit owin-Start</span><span class="sxs-lookup"><span data-stu-id="7ca49-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="7ca49-128">Erstellen Sie eine leere ASP.NET-Webanwendung, und nennen Sie Sie **startupdemo**.</span><span class="sxs-lookup"><span data-stu-id="7ca49-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="7ca49-129">-Installieren Sie `Microsoft.Owin.Host.SystemWeb` mit dem nuget-Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="7ca49-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="7ca49-130">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**und dann auf Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="7ca49-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="7ca49-131">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="7ca49-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="7ca49-132">Fügen Sie eine owin-Startklasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="7ca49-132">Add an OWIN startup class.</span></span> <span data-ttu-id="7ca49-133">Klicken Sie in Visual Studio 2017 mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**aus. Geben Sie im Dialogfeld **Neues Element hinzufügen** *owin* in das Suchfeld ein, und ändern Sie den Namen in Startup.cs, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="7ca49-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="7ca49-134">Wenn Sie das nächste Mal eine *owin-Startklasse*hinzufügen möchten, wird diese im Menü **Hinzufügen** verfügbar sein.</span><span class="sxs-lookup"><span data-stu-id="7ca49-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="7ca49-135">Sie können auch mit der rechten Maustaste auf das Projekt klicken und **Hinzufügen**auswählen. Wählen Sie dann **Neues Element**aus, und wählen Sie dann die **owin-Startklasse aus**.</span><span class="sxs-lookup"><span data-stu-id="7ca49-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="7ca49-136">Ersetzen Sie den generierten Code in der *Startup.cs* -Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7ca49-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="7ca49-137">Der `app.Use` Lambda-Ausdruck wird verwendet, um die angegebene Middlewarekomponente in der owin-Pipeline zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="7ca49-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="7ca49-138">In diesem Fall richten Sie die Protokollierung eingehender Anforderungen ein, bevor Sie auf die eingehende Anforderung Antworten.</span><span class="sxs-lookup"><span data-stu-id="7ca49-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="7ca49-139">Der `next`-Parameter ist der Delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) zur nächsten Komponente in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ca49-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="7ca49-140">Der `app.Run` Lambda-Ausdruck verknüpft die Pipeline mit eingehenden Anforderungen und stellt den Antwortmechanismus bereit.</span><span class="sxs-lookup"><span data-stu-id="7ca49-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="7ca49-141">Im obigen Code haben wir das `OwinStartup` Attribut auskommentiert, und wir verlassen uns auf die Konvention, die Klasse mit dem Namen `Startup` auszuführen. Drücken Sie ***F5*** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="7ca49-142">Die Treffer Aktualisierung ist mehrmals.</span><span class="sxs-lookup"><span data-stu-id="7ca49-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="7ca49-143">![](owin-startup-class-detection/_static/image4.png) Hinweis: die Zahl, die in den Bildern in diesem Tutorial angezeigt wird, stimmt nicht mit der angezeigten Anzahl ab.</span><span class="sxs-lookup"><span data-stu-id="7ca49-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="7ca49-144">Die millisekundenzeichenfolge wird verwendet, um eine neue Antwort anzuzeigen, wenn Sie die Seite aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="7ca49-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="7ca49-145">Die Ablauf Verfolgungs Informationen können im Fenster **Ausgabe** angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7ca49-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="7ca49-146">Weitere Startklassen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="7ca49-146">Add More Startup Classes</span></span>

<span data-ttu-id="7ca49-147">In diesem Abschnitt fügen wir eine weitere Startup-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="7ca49-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="7ca49-148">Sie können Ihrer Anwendung mehrere owin-Startklassen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="7ca49-149">Beispielsweise können Sie Startklassen für Entwicklung, Tests und Produktion erstellen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="7ca49-150">Erstellen Sie eine neue owin-Startklasse, und benennen Sie Sie `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="7ca49-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="7ca49-151">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="7ca49-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="7ca49-152">Drücken Sie die Taste F5, um die APP auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="7ca49-153">Das `OwinStartup`-Attribut gibt an, dass die Produktionsstart Klasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7ca49-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="7ca49-154">Erstellen Sie eine weitere owin-Startklasse, und benennen Sie Sie `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="7ca49-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="7ca49-155">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="7ca49-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="7ca49-156">Mit der obigen `OwinStartup` Attribut Überladung wird `TestingConfiguration` als *Anzeige Name der* Startklasse angegeben.</span><span class="sxs-lookup"><span data-stu-id="7ca49-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="7ca49-157">Öffnen Sie die Datei " *Web. config* ", und fügen Sie den owin-App-Start Schlüssel hinzu, der den anzeigen amen der Startklasse angibt:</span><span class="sxs-lookup"><span data-stu-id="7ca49-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="7ca49-158">Drücken Sie die Taste F5, um die APP auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="7ca49-159">Das App settings-Element übernimmt einen Präzedenzfall, und die Testkonfiguration wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7ca49-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="7ca49-160">Entfernen Sie den *anzeigen Amen aus dem `OwinStartup`* -Attribut in der `TestStartup`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="7ca49-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="7ca49-161">Ersetzen Sie den owin-App-Systemstart Schlüssel in der Datei " *Web. config* " durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7ca49-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="7ca49-162">Setzen Sie das `OwinStartup`-Attribut in jeder Klasse auf den von Visual Studio generierten Standard Attribut Code zurück:</span><span class="sxs-lookup"><span data-stu-id="7ca49-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="7ca49-163">Jeder der folgenden Start Schlüssel der owin-App führt dazu, dass die produktionsklasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7ca49-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="7ca49-164">Der letzte Start Schlüssel gibt die Start Konfigurations Methode an.</span><span class="sxs-lookup"><span data-stu-id="7ca49-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="7ca49-165">Mit dem folgenden owin-App-Start Schlüssel können Sie den Namen der Konfigurations Klasse in "`MyConfiguration`" ändern.</span><span class="sxs-lookup"><span data-stu-id="7ca49-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="7ca49-166">Verwenden von "owinhost. exe"</span><span class="sxs-lookup"><span data-stu-id="7ca49-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="7ca49-167">Ersetzen Sie die Datei "Web. config" durch das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="7ca49-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="7ca49-168">Der letzte Schlüssel gewinnt, daher wird in diesem Fall `TestStartup` angegeben.</span><span class="sxs-lookup"><span data-stu-id="7ca49-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="7ca49-169">Installieren Sie owinhost über die PMC:</span><span class="sxs-lookup"><span data-stu-id="7ca49-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="7ca49-170">Navigieren Sie zum Anwendungsordner (der Ordner mit der Datei " *Web. config* "), und geben Sie in einer Eingabeaufforderung Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="7ca49-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="7ca49-171">Im Befehlsfenster wird Folgendes angezeigt:</span><span class="sxs-lookup"><span data-stu-id="7ca49-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="7ca49-172">Starten Sie einen Browser mit der URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="7ca49-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="7ca49-173">Owinhost hat die oben aufgeführten Start Konventionen berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="7ca49-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="7ca49-174">Drücken Sie im Befehlsfenster die EINGABETASTE, um owinhost zu schließen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="7ca49-175">Fügen Sie in der `ProductionStartup`-Klasse das folgende owinstartup-Attribut hinzu, das den anzeigen amen der *productionconfiguration*angibt.</span><span class="sxs-lookup"><span data-stu-id="7ca49-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="7ca49-176">Geben Sie in der Eingabeaufforderung Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="7ca49-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="7ca49-177">Die Produktionsstart Klasse wird geladen.</span><span class="sxs-lookup"><span data-stu-id="7ca49-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="7ca49-178">Unsere Anwendung verfügt über mehrere Startklassen, und in diesem Beispiel haben wir die Startklasse zurückgestellt, die bis zur Laufzeit geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="7ca49-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="7ca49-179">Testen Sie die folgenden Startoptionen für die Laufzeit:</span><span class="sxs-lookup"><span data-stu-id="7ca49-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
