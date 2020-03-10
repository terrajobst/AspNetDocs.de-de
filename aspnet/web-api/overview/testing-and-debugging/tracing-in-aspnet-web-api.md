---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Ablauf Verfolgung in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie die Ablauf Verfolgung in ASP.net-Web-API aktiviert wird.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484341"
---
# <a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="09f74-103">Ablauf Verfolgung in ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="09f74-103">Tracing in ASP.NET Web API 2</span></span>

<span data-ttu-id="09f74-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09f74-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="09f74-105">Wenn Sie versuchen, eine webbasierte Anwendung zu debuggen, gibt es keinen Ersatz für einen guten Satz von Ablauf Verfolgungs Protokollen.</span><span class="sxs-lookup"><span data-stu-id="09f74-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="09f74-106">In diesem Tutorial wird gezeigt, wie die Ablauf Verfolgung in ASP.net-Web-API aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="09f74-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="09f74-107">Mit dieser Funktion können Sie verfolgen, was das Web-API-Framework vor und nach dem Aufrufen Ihres Controllers tut.</span><span class="sxs-lookup"><span data-stu-id="09f74-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="09f74-108">Sie können Sie auch verwenden, um Ihren eigenen Code zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="09f74-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="09f74-109">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="09f74-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="09f74-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (funktioniert auch mit Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="09f74-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="09f74-111">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="09f74-111">Web API 2</span></span>
> - [<span data-ttu-id="09f74-112">Microsoft. Aspnet. WebAPI. Tracing</span><span class="sxs-lookup"><span data-stu-id="09f74-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="09f74-113">System. Diagnostics-Ablauf Verfolgung in der Web-API aktivieren</span><span class="sxs-lookup"><span data-stu-id="09f74-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="09f74-114">Zunächst erstellen wir ein neues ASP.NET-Webanwendungs Projekt.</span><span class="sxs-lookup"><span data-stu-id="09f74-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="09f74-115">Wählen Sie in Visual Studio im Menü **Datei** die Option **neu** > **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="09f74-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="09f74-116">Wählen Sie unter **Vorlagen**die Option **Web**, **ASP.NET Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="09f74-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="09f74-117">Wählen Sie die Vorlage Web-API-Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="09f74-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="09f74-118">Wählen Sie **im Menü** Extras den Befehl **nuget-Paket-Manager**und dann **Package Manage Console**aus.</span><span class="sxs-lookup"><span data-stu-id="09f74-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="09f74-119">Geben Sie im Fenster der Paket-Manager-Konsole die folgenden Befehle ein.</span><span class="sxs-lookup"><span data-stu-id="09f74-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="09f74-120">Mit dem ersten Befehl wird das neueste Web-API-Ablauf Verfolgungs Paket installiert.</span><span class="sxs-lookup"><span data-stu-id="09f74-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="09f74-121">Außerdem werden die Kern-Web-API-Pakete aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="09f74-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="09f74-122">Mit dem zweiten Befehl wird das WebAPI. Webhost-Paket auf die neueste Version aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="09f74-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="09f74-123">Wenn Sie eine bestimmte Version der Web-API als Ziel verwenden möchten, verwenden Sie das Flag-Version, wenn Sie das Ablauf Verfolgungs Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="09f74-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="09f74-124">Öffnen Sie die Datei WebApiConfig.cs im Ordner App\_Start.</span><span class="sxs-lookup"><span data-stu-id="09f74-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="09f74-125">Fügen Sie der **Register** -Methode den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="09f74-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="09f74-126">Mit diesem Code wird die Klasse " [systemdiagnosticstracewriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) " der Web-API-Pipeline hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="09f74-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="09f74-127">Die **systemdiagnosticstracewriter** -Klasse schreibt Ablauf Verfolgungen in [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="09f74-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="09f74-128">Um die Ablauf Verfolgungen anzuzeigen, führen Sie die Anwendung im Debugger aus.</span><span class="sxs-lookup"><span data-stu-id="09f74-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="09f74-129">Navigieren Sie im Browser zu `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="09f74-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="09f74-130">Die Trace-Anweisungen werden in das Ausgabefenster in Visual Studio geschrieben.</span><span class="sxs-lookup"><span data-stu-id="09f74-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="09f74-131">(Wählen Sie im Menü **Ansicht** die Option **Ausgabe**aus.)</span><span class="sxs-lookup"><span data-stu-id="09f74-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="09f74-132">Da **systemdiagnosticstracewriter** Ablauf Verfolgungen in **System. Diagnostics. Trace**schreibt, können Sie zusätzliche Ablaufverfolgungslistener registrieren. beispielsweise, um Ablauf Verfolgungen in eine Protokolldatei zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="09f74-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="09f74-133">Weitere Informationen zu Ablaufverfolgungslistener finden Sie im Thema [Trace](https://msdn.microsoft.com/library/4y5y10s7.aspx) Listener auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="09f74-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="09f74-134">Konfigurieren von systemdiagnosticstracewriter</span><span class="sxs-lookup"><span data-stu-id="09f74-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="09f74-135">Der folgende Code zeigt, wie der ablaufverfolgungswriter konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="09f74-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="09f74-136">Es gibt zwei Einstellungen, die Sie steuern können:</span><span class="sxs-lookup"><span data-stu-id="09f74-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="09f74-137">Isverbose: bei false enthält jede Ablauf Verfolgung minimale Informationen.</span><span class="sxs-lookup"><span data-stu-id="09f74-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="09f74-138">True gibt an, dass Ablauf Verfolgungen Weitere Informationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="09f74-138">If true, traces include more information.</span></span>
- <span data-ttu-id="09f74-139">Minimumlevel: legt die minimale Ablauf Verfolgungs Ebene fest.</span><span class="sxs-lookup"><span data-stu-id="09f74-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="09f74-140">Ablauf Verfolgungs Ebenen sind Debuggen, Info, Warnung, Fehler und schwerwiegend.</span><span class="sxs-lookup"><span data-stu-id="09f74-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="09f74-141">Hinzufügen von Ablauf Verfolgungen zu Ihrer Web-API-Anwendung</span><span class="sxs-lookup"><span data-stu-id="09f74-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="09f74-142">Durch das Hinzufügen eines Trace Writer erhalten Sie sofortigen Zugriff auf die von der Web-API-Pipeline erstellten Ablauf Verfolgungen.</span><span class="sxs-lookup"><span data-stu-id="09f74-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="09f74-143">Sie können auch den ablaufverfolgungswriter verwenden, um den eigenen Code zu verfolgen:</span><span class="sxs-lookup"><span data-stu-id="09f74-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="09f74-144">Um den ablaufverfolgungswriter abzurufen, nennen Sie **httpconfiguration. Services. gettracewriter**.</span><span class="sxs-lookup"><span data-stu-id="09f74-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="09f74-145">Von einem Controller aus kann auf diese Methode über die **apicontroller. Configuration** -Eigenschaft zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="09f74-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="09f74-146">Wenn Sie eine Ablauf Verfolgung schreiben möchten, können Sie die Methode **itracewriter. Trace** direkt aufzurufen, aber die Klasse [itraceschreiterextensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) definiert einige Erweiterungs Methoden, die benutzerfreundlicher sind.</span><span class="sxs-lookup"><span data-stu-id="09f74-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="09f74-147">Beispielsweise wird mit der oben gezeigten **Info** -Methode eine Ablauf Verfolgung mit **Informationen**auf Ablauf Verfolgungs Ebene erstellt.</span><span class="sxs-lookup"><span data-stu-id="09f74-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="09f74-148">Web-API-Ablauf Verfolgungs Infrastruktur</span><span class="sxs-lookup"><span data-stu-id="09f74-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="09f74-149">In diesem Abschnitt wird beschrieben, wie ein benutzerdefinierter ablaufverfolgungswriter für die Web-API</span><span class="sxs-lookup"><span data-stu-id="09f74-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="09f74-150">Das Paket Microsoft. Aspnet. WebAPI. Tracing basiert auf einer allgemeineren Ablauf Verfolgungs Infrastruktur in der Web-API.</span><span class="sxs-lookup"><span data-stu-id="09f74-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="09f74-151">Anstatt Microsoft. Aspnet. WebAPI. Tracing zu verwenden, können Sie auch eine andere Ablauf Verfolgungs-/Protokollierungs Bibliothek einbinden, z. b. [nlog](http://nlog-project.org/) oder [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="09f74-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/logging library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="09f74-152">Um Ablauf Verfolgungen zu erfassen, implementieren Sie die **itracewriter** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="09f74-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="09f74-153">Hier sehen Sie ein einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="09f74-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="09f74-154">Die **itracewriter. Trace** -Methode erstellt eine Ablauf Verfolgung.</span><span class="sxs-lookup"><span data-stu-id="09f74-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="09f74-155">Der Aufrufer gibt eine Kategorie und eine Ablauf Verfolgungs Ebene an.</span><span class="sxs-lookup"><span data-stu-id="09f74-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="09f74-156">Die Kategorie kann eine beliebige benutzerdefinierte Zeichenfolge sein.</span><span class="sxs-lookup"><span data-stu-id="09f74-156">The category can be any user-defined string.</span></span> <span data-ttu-id="09f74-157">Ihre Implementierung der Ablauf **Verfolgung** sollte folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="09f74-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="09f74-158">Erstellen Sie einen neuen **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="09f74-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="09f74-159">Initialisieren Sie Sie wie gezeigt mit der Anforderungs-, der Kategorie-und der Ablauf Verfolgungs Ebene.</span><span class="sxs-lookup"><span data-stu-id="09f74-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="09f74-160">Diese Werte werden vom Aufrufer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="09f74-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="09f74-161">Rufen Sie den *traceaction* -Delegaten auf.</span><span class="sxs-lookup"><span data-stu-id="09f74-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="09f74-162">Innerhalb dieses Delegaten wird erwartet, dass der Aufrufer den Rest des **TraceRecord**ausgefüllt.</span><span class="sxs-lookup"><span data-stu-id="09f74-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="09f74-163">Schreiben Sie den **TraceRecord**, indem Sie ein beliebiges Protokollierungs Verfahren verwenden, das Ihnen gefällt.</span><span class="sxs-lookup"><span data-stu-id="09f74-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="09f74-164">Im hier gezeigten Beispiel wird einfach " **System. Diagnostics. Trace**" aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="09f74-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="09f74-165">Festlegen des Trace Writer</span><span class="sxs-lookup"><span data-stu-id="09f74-165">Setting the Trace Writer</span></span>

<span data-ttu-id="09f74-166">Um die Ablauf Verfolgung zu aktivieren, müssen Sie die Web-API für die Verwendung Ihrer **itracewriter** -Implementierung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="09f74-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="09f74-167">Dies geschieht über das **httpconfiguration** -Objekt, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="09f74-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="09f74-168">Nur ein Ablauf Verfolgungs Schreiber kann aktiv sein.</span><span class="sxs-lookup"><span data-stu-id="09f74-168">Only one trace writer can be active.</span></span> <span data-ttu-id="09f74-169">Standardmäßig legt die Web-API einen &quot;No-op-&quot;-Überwachungsdaten Satz fest, der keine Aktion ausführt.</span><span class="sxs-lookup"><span data-stu-id="09f74-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="09f74-170">(Die &quot;No-op&quot; Überwachungs vorhanden ist, sodass der Ablauf Verfolgungs Code nicht prüfen muss, ob der ablaufverfolgungswriter **null** ist, bevor eine Ablauf Verfolgung geschrieben wird.)</span><span class="sxs-lookup"><span data-stu-id="09f74-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="09f74-171">Funktionsweise der Web API-Ablauf Verfolgung</span><span class="sxs-lookup"><span data-stu-id="09f74-171">How Web API Tracing Works</span></span>

<span data-ttu-id="09f74-172">Die Ablauf Verfolgung in der Web-API verwendet ein *Fassaden* Muster: Wenn die Ablauf Verfolgung aktiviert ist, umschließt die Web-API verschiedene Teile der Anforderungs Pipeline mit Klassen, die Ablauf Verfolgungs Aufrufe ausführen.</span><span class="sxs-lookup"><span data-stu-id="09f74-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="09f74-173">Wenn Sie z. b. einen Controller auswählen, verwendet die Pipeline die **ihttpcontrollerselector** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="09f74-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="09f74-174">Bei aktivierter Ablauf Verfolgung fügt die Pipeline eine Klasse ein, die **ihttpcontrollerselector** implementiert, aber Aufrufe an die tatsächliche Implementierung durchführt:</span><span class="sxs-lookup"><span data-stu-id="09f74-174">With tracing enabled, the pipeline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Die Web-API-Ablauf Verfolgung verwendet das Fassaden Muster.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="09f74-176">Dieser Entwurf bietet folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="09f74-176">The benefits of this design include:</span></span>

- <span data-ttu-id="09f74-177">Wenn Sie keinen Trace Writer hinzufügen, werden die Ablauf Verfolgungs Komponenten nicht instanziiert und haben keine Auswirkungen auf die Leistung.</span><span class="sxs-lookup"><span data-stu-id="09f74-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="09f74-178">Wenn Sie Standarddienste wie **ihttpcontrollerselector** durch ihre eigene benutzerdefinierte Implementierung ersetzen, wird die Ablauf Verfolgung nicht beeinträchtigt, da die Ablauf Verfolgung durch das Wrapper Objekt erfolgt.</span><span class="sxs-lookup"><span data-stu-id="09f74-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="09f74-179">Sie können auch das gesamte Web-API-Ablauf Verfolgungs Framework durch ihr eigenes benutzerdefiniertes Framework ersetzen, indem Sie den **itracemanager** -Standard Dienst ersetzen:</span><span class="sxs-lookup"><span data-stu-id="09f74-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="09f74-180">Implementieren Sie **itracemanager. Initialisieren** , um das Ablauf Verfolgungssystem zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="09f74-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="09f74-181">Beachten Sie, dass dies das *gesamte* Ablauf Verfolgungs Framework ersetzt, einschließlich des gesamten Ablauf Verfolgungs Codes, der in die Web-API integriert ist.</span><span class="sxs-lookup"><span data-stu-id="09f74-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
