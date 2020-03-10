---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 benutzerdefinierte Aktionsfilter | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET MVC stellt Aktionsfilter zum Ausführen der Filter Logik bereit, bevor oder nachdem eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468177"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="d561c-104">ASP.NET MVC 4 – Benutzerdefinierte Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="d561c-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="d561c-105">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d561c-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d561c-106">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="d561c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="d561c-107">ASP.NET MVC stellt Aktionsfilter zum Ausführen der Filter Logik bereit, bevor oder nachdem eine Aktionsmethode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="d561c-108">Aktionsfilter sind benutzerdefinierte Attribute, die deklarative Mittel zum Hinzufügen von vor-und nach-Aktion-Verhalten zu den Aktionsmethoden des Controllers bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="d561c-109">In dieser praktischen Übungseinheit erstellen Sie ein benutzerdefiniertes Aktionsfilter Attribut in der mvcmusicstore-Projekt Mappe, um die Anforderungen des Controllers abzufangen und die Aktivität einer Website in einer Datenbanktabelle zu protokollieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="d561c-110">Sie können den Protokollierungs Filter per Injection zu einem beliebigen Controller oder einer Aktion hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d561c-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="d561c-111">Schließlich wird die Protokoll Ansicht angezeigt, in der die Liste der Besucher angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="d561c-112">Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse von **ASP.NET MVC**verfügen.</span><span class="sxs-lookup"><span data-stu-id="d561c-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="d561c-113">Wenn Sie **ASP.NET MVC** noch nicht verwendet haben, empfehlen wir Ihnen, **ASP.NET MVC 4-Grundlagen** praktische Übungseinheiten zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="d561c-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="d561c-114">Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das in den [Releases Microsoft-Web/webcamptrainingkit](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="d561c-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="d561c-115">Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d561c-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d561c-116">Ziele</span><span class="sxs-lookup"><span data-stu-id="d561c-116">Objectives</span></span>

<span data-ttu-id="d561c-117">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d561c-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="d561c-118">Erstellen eines benutzerdefinierten Aktionsfilter Attributs zum Erweitern der Filterfunktionen</span><span class="sxs-lookup"><span data-stu-id="d561c-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="d561c-119">Anwenden eines benutzerdefinierten Filter Attributs per injection auf eine bestimmte Ebene</span><span class="sxs-lookup"><span data-stu-id="d561c-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="d561c-120">Globales Registrieren eines benutzerdefinierten Aktions Filters</span><span class="sxs-lookup"><span data-stu-id="d561c-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d561c-121">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="d561c-121">Prerequisites</span></span>

<span data-ttu-id="d561c-122">Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:</span><span class="sxs-lookup"><span data-stu-id="d561c-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="d561c-123">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).</span><span class="sxs-lookup"><span data-stu-id="d561c-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d561c-124">Einrichten</span><span class="sxs-lookup"><span data-stu-id="d561c-124">Setup</span></span>

<span data-ttu-id="d561c-125">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="d561c-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="d561c-126">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="d561c-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="d561c-127">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="d561c-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="d561c-128">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.</span><span class="sxs-lookup"><span data-stu-id="d561c-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d561c-129">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="d561c-129">Exercises</span></span>

<span data-ttu-id="d561c-130">Diese praktische Übungseinheit besteht aus den folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="d561c-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="d561c-131">Übung 1: Protokollierungs Aktionen</span><span class="sxs-lookup"><span data-stu-id="d561c-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="d561c-132">Übung 2: Verwalten mehrerer Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="d561c-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="d561c-133">Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="d561c-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="d561c-134">Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="d561c-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d561c-135">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="d561c-136">Übung 1: Protokollierungs Aktionen</span><span class="sxs-lookup"><span data-stu-id="d561c-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="d561c-137">In dieser Übung erfahren Sie, wie Sie einen benutzerdefinierten Aktionsprotokoll Filter mithilfe von ASP.NET MVC 4-Filter Anbietern erstellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="d561c-138">Zu diesem Zweck wenden Sie einen Protokollierungs Filter auf die Musicstore-Website an, mit der alle Aktivitäten in den ausgewählten Controllern aufgezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="d561c-139">Der Filter erweitert **Action Filter attributeclass** und setzt die **OnAction** -Methode außer Kraft, um die einzelnen Anforderungen abzufangen und dann die Protokollierungs Aktionen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d561c-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="d561c-140">Die Kontextinformationen zu HTTP-Anforderungen, zum Ausführen von Methoden, Ergebnissen und Parametern werden von der ASP.NET MVC-Klasse " **aktionsexecutingcontext** " bereitgestellt **.**</span><span class="sxs-lookup"><span data-stu-id="d561c-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="d561c-141">ASP.NET MVC 4 verfügt auch über Standardfilter Anbieter, die Sie verwenden können, ohne einen benutzerdefinierten Filter zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="d561c-142">ASP.NET MVC 4 bietet die folgenden Typen von Filtern:</span><span class="sxs-lookup"><span data-stu-id="d561c-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="d561c-143">**Autorisierungs** Filter, der Sicherheitsentscheidungen darüber trifft, ob eine Aktionsmethode ausgeführt werden soll, z. b. das Durchführen der Authentifizierung oder das Überprüfen von Eigenschaften der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="d561c-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="d561c-144">**Aktions** Filter, der die Ausführung der Aktionsmethode umschließt.</span><span class="sxs-lookup"><span data-stu-id="d561c-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="d561c-145">Mit diesem Filter können zusätzliche Verarbeitungsschritte ausgeführt werden, z. b. das Bereitstellen zusätzlicher Daten für die Aktionsmethode, das Überprüfen des Rückgabewerts oder das Abbrechen der Ausführung der Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="d561c-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="d561c-146">Der **Ergebnis** Filter, der die Ausführung des Aktions result-Objekts umschließt.</span><span class="sxs-lookup"><span data-stu-id="d561c-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="d561c-147">Dieser Filter kann zusätzliche Verarbeitungsschritte durchführen, z. b. das Ändern der HTTP-Antwort.</span><span class="sxs-lookup"><span data-stu-id="d561c-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="d561c-148">**Ausnahme** Filter, der ausgeführt wird, wenn eine nicht behandelte Ausnahme in der Aktionsmethode ausgelöst wird, beginnend mit den Autorisierungs Filtern und mit der Ausführung des Ergebnisses.</span><span class="sxs-lookup"><span data-stu-id="d561c-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="d561c-149">Ausnahme Filter können für Aufgaben wie z. b. die Protokollierung oder die Anzeige einer Fehlerseite verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="d561c-150">Weitere Informationen zu Filter Anbietern finden Sie in diesem MSDN-Link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="d561c-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="d561c-151">Informationen zum MVC Music Store-Anwendungs Protokollierungs Feature</span><span class="sxs-lookup"><span data-stu-id="d561c-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="d561c-152">Diese Music Store-Projekt Mappe verfügt über eine neue Datenmodell Tabelle für die Website Protokollierung " **Action Log**" mit den folgenden Feldern: Name des Controllers, der eine Anforderung empfangen hat, so genannte Aktion, Client-IP und Zeitstempel.</span><span class="sxs-lookup"><span data-stu-id="d561c-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="d561c-153">![Datenmodell. Aktionsprotokoll Tabelle.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Datenmodell. Aktionsprotokoll Tabelle.")</span><span class="sxs-lookup"><span data-stu-id="d561c-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="d561c-154">*Datenmodell-Aktionsprotokoll Tabelle*</span><span class="sxs-lookup"><span data-stu-id="d561c-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="d561c-155">Die Lösung bietet eine ASP.NET MVC-Ansicht für das Aktionsprotokoll, das Sie unter **mvcmusicstores/views/Action Log**finden:</span><span class="sxs-lookup"><span data-stu-id="d561c-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="d561c-156">![Aktionsprotokoll Ansicht](aspnet-mvc-4-custom-action-filters/_static/image2.png "Aktionsprotokoll Ansicht")</span><span class="sxs-lookup"><span data-stu-id="d561c-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="d561c-157">*Aktionsprotokoll Ansicht*</span><span class="sxs-lookup"><span data-stu-id="d561c-157">*Action Log view*</span></span>

<span data-ttu-id="d561c-158">Bei dieser gegebenen Struktur konzentrieren sich alle Aufgaben auf das Unterbrechen der Anforderung des Controllers und das Ausführen der Protokollierung mithilfe der benutzerdefinierten Filterung.</span><span class="sxs-lookup"><span data-stu-id="d561c-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="d561c-159">Aufgabe 1: Erstellen eines benutzerdefinierten Filters zum Abfangen der Anforderung eines Controllers</span><span class="sxs-lookup"><span data-stu-id="d561c-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="d561c-160">In dieser Aufgabe erstellen Sie eine benutzerdefinierte Filter Attribut Klasse, die die Protokollierungs Logik enthält.</span><span class="sxs-lookup"><span data-stu-id="d561c-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="d561c-161">Zu diesem Zweck erweitern Sie die ASP.NET MVC **Action Filter Attribute** -Klasse und implementieren die **IAction Filter**-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="d561c-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="d561c-162">Das **Action Filter Attribute-Attribut** ist die Basisklasse für alle Attribut Filter.</span><span class="sxs-lookup"><span data-stu-id="d561c-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="d561c-163">Es stellt die folgenden Methoden bereit, um eine bestimmte Logik nach und vor der Ausführung der Controller Aktion auszuführen:</span><span class="sxs-lookup"><span data-stu-id="d561c-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="d561c-164">**OnAction-Ausführung**(Action executingcontext Filter context): unmittelbar vor dem Aufruf der Action-Methode.</span><span class="sxs-lookup"><span data-stu-id="d561c-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="d561c-165">**OnAction**-Anweisung (Action executedcontext Filter context): nach dem Aufrufen der Aktionsmethode und vor dem Ausführen des Ergebnisses (vor dem Anzeigen des Rendering).</span><span class="sxs-lookup"><span data-stu-id="d561c-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="d561c-166">**OnResultExecuting**(resultexecutingcontext Filter context): unmittelbar vor dem Ausführen des Ergebnisses (vor dem Anzeigen des Rendering).</span><span class="sxs-lookup"><span data-stu-id="d561c-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="d561c-167">**OnResultExecuted**(resultexecutedcontext Filter context): nach dem Ausführen des Ergebnisses (nachdem die Ansicht gerendert wurde).</span><span class="sxs-lookup"><span data-stu-id="d561c-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="d561c-168">Wenn Sie eine dieser Methoden in einer abgeleiteten Klasse überschreiben, können Sie Ihren eigenen Filter Code ausführen.</span><span class="sxs-lookup"><span data-stu-id="d561c-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>

1. <span data-ttu-id="d561c-169">Öffnen Sie die Projekt Mappe " **Begin** " im Ordner " **\source\ex01-loggingaktions\begin** ".</span><span class="sxs-lookup"><span data-stu-id="d561c-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="d561c-170">Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="d561c-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="d561c-171">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="d561c-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d561c-172">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="d561c-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d561c-173">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="d561c-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d561c-174">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="d561c-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d561c-175">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="d561c-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d561c-176">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="d561c-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="d561c-177">Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="d561c-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="d561c-178">Fügen Sie dem C# Ordner **Filters** eine neue Klasse hinzu, und nennen Sie Sie *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="d561c-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="d561c-179">In diesem Ordner werden alle benutzerdefinierten Filter gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d561c-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="d561c-180">Öffnen Sie **CustomActionFilter.cs** , und fügen Sie einen Verweis auf **System. Web. MVC** -und **mvcmusicstore. Models** -Namespaces hinzu:</span><span class="sxs-lookup"><span data-stu-id="d561c-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="d561c-181">(Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX1-customaktionfilternamespaces*)</span><span class="sxs-lookup"><span data-stu-id="d561c-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="d561c-182">Erben Sie die **CustomAction Filter** -Klasse von **Action Filter Attribute** , und legen Sie dann die **CustomAction Filter** -Klasse die **IAction Filter** -Schnittstelle an.</span><span class="sxs-lookup"><span data-stu-id="d561c-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="d561c-183">Legen Sie die **customaktionfilter** -Klasse überschreiben Sie die **onaktionexecution** -Methode, und fügen Sie die erforderliche Logik zum Protokollieren der Filter Ausführung hinzu.</span><span class="sxs-lookup"><span data-stu-id="d561c-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="d561c-184">Fügen Sie zu diesem Zweck den folgenden markierten Code in der **customaktionfilter** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="d561c-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="d561c-185">(Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX1-loggingactions*)</span><span class="sxs-lookup"><span data-stu-id="d561c-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="d561c-186">Die **onaktionsmethode** verwendet **Entity Framework** , um ein neues Aktionsprotokoll hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="d561c-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="d561c-187">Er erstellt und füllt eine neue Entitäts Instanz mit den Kontextinformationen aus **FilterContext**.</span><span class="sxs-lookup"><span data-stu-id="d561c-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="d561c-188">Weitere Informationen zur **controllerContext** -Klasse finden Sie auf der [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)-Website.</span><span class="sxs-lookup"><span data-stu-id="d561c-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="d561c-189">Aufgabe 2: Einfügen eines codeinterceptors in die Speicher Controller Klasse</span><span class="sxs-lookup"><span data-stu-id="d561c-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="d561c-190">In dieser Aufgabe fügen Sie den benutzerdefinierten Filter hinzu, indem Sie ihn in alle Controller Klassen und Controller Aktionen einfügen, die protokolliert werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="d561c-191">Für diese Übung verfügt die Store Controller-Klasse über ein Protokoll.</span><span class="sxs-lookup"><span data-stu-id="d561c-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="d561c-192">Die **OnAction** -Methode der Methode aus dem benutzerdefinierten **Action logfilterattribute** -Filter wird ausgeführt, wenn ein injiziertes Element aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="d561c-193">Es ist auch möglich, eine bestimmte Controller Methode abzufangen.</span><span class="sxs-lookup"><span data-stu-id="d561c-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="d561c-194">Öffnen Sie **StoreController** unter **mvcmusicstore\controllers** , und fügen Sie einen Verweis auf den **Filter** -Namespace hinzu:</span><span class="sxs-lookup"><span data-stu-id="d561c-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="d561c-195">Fügen Sie den benutzerdefinierten Filter **customaktionfilter** in die **StoreController** -Klasse ein, indem Sie das Attribut **[customaktionfilter]** vor der Klassen Deklaration hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d561c-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="d561c-196">Wenn ein Filter in eine Controller Klasse eingefügt wird, werden alle zugehörigen Aktionen ebenfalls eingefügt.</span><span class="sxs-lookup"><span data-stu-id="d561c-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="d561c-197">Wenn Sie den Filter nur für eine Reihe von Aktionen anwenden möchten, müssten Sie **[customaktionfilter]** an jeden der folgenden Elemente einfügen:</span><span class="sxs-lookup"><span data-stu-id="d561c-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="d561c-198">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="d561c-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="d561c-199">In dieser Aufgabe testen Sie, ob der Protokollierungs Filter funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d561c-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="d561c-200">Sie starten die Anwendung und besuchen den Store, und dann überprüfen Sie die protokollierten Aktivitäten.</span><span class="sxs-lookup"><span data-stu-id="d561c-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="d561c-201">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d561c-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="d561c-202">Navigieren Sie zu **/ActionLog** , um den Anfangszustand der Protokoll Ansicht anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="d561c-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="d561c-203">![Protokoll verfolgungsstatus vor Seiten Aktivität](aspnet-mvc-4-custom-action-filters/_static/image3.png "Protokoll verfolgungsstatus vor Seiten Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="d561c-204">*Protokoll verfolgungsstatus vor Seiten Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="d561c-205">Standardmäßig wird immer ein Element angezeigt, das beim Abrufen der vorhandenen Genres für das Menü generiert wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="d561c-206">Aus Gründen der Einfachheit wird die Tabelle " **aktionlog** " bereinigt, wenn die Anwendung ausgeführt wird, sodass nur die Protokolle der Überprüfung der jeweiligen Aufgabe angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="d561c-207">Möglicherweise müssen Sie den folgenden Code aus der **Sitzungs\_Start** -Methode (in der **Global. asax** -Klasse) entfernen, um ein Verlaufs Protokoll für alle innerhalb des Speicher Controllers ausgeführten Aktionen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d561c-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="d561c-208">Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.</span><span class="sxs-lookup"><span data-stu-id="d561c-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="d561c-209">Navigieren Sie zu **/ActionLog** , und wenn das Protokoll leer ist, drücken Sie **F5** , um die Seite zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="d561c-210">Überprüfen Sie, ob Ihre Besuche nachverfolgt wurden:</span><span class="sxs-lookup"><span data-stu-id="d561c-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="d561c-211">![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image4.png "Aktionsprotokoll mit protokollierter Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="d561c-212">*Aktionsprotokoll mit protokollierter Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="d561c-213">Übung 2: Verwalten mehrerer Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="d561c-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="d561c-214">In dieser Übung fügen Sie der StoreController-Klasse einen zweiten benutzerdefinierten Aktions Filter hinzu und definieren die jeweilige Reihenfolge, in der beide Filter ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="d561c-215">Anschließend aktualisieren Sie den Code, um den Filter Global zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="d561c-216">Beim Definieren der Ausführungsreihenfolge der Filter sind unterschiedliche Optionen zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="d561c-217">Zum Beispiel die Order-Eigenschaft und der Filterbereich:</span><span class="sxs-lookup"><span data-stu-id="d561c-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="d561c-218">Sie können einen **Bereich** für jeden Filter definieren. Sie können z. b. alle Aktionsfilter so festlegen, dass Sie innerhalb des **Controller Bereichs**ausgeführt werden, und alle Autorisierungs Filter, die im globalen Gültigkeits **Bereich**ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="d561c-219">Die Bereiche haben eine definierte Ausführungsreihenfolge.</span><span class="sxs-lookup"><span data-stu-id="d561c-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="d561c-220">Außerdem verfügt jeder Aktionsfilter über eine Order-Eigenschaft, die verwendet wird, um die Ausführungsreihenfolge im Filterbereich zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="d561c-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="d561c-221">Weitere Informationen zur Ausführung von benutzerdefinierten Aktions Filtern finden Sie in diesem MSDN-Artikel: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="d561c-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="d561c-222">Aufgabe 1: Erstellen eines neuen benutzerdefinierten Aktions Filters</span><span class="sxs-lookup"><span data-stu-id="d561c-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="d561c-223">In dieser Aufgabe erstellen Sie einen neuen benutzerdefinierten Aktions Filter, der in die StoreController-Klasse eingefügt werden soll, und erfahren, wie Sie die Ausführungsreihenfolge der Filter verwalten.</span><span class="sxs-lookup"><span data-stu-id="d561c-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="d561c-224">Öffnen Sie die Projekt Mappe " **Begin** " im Ordner " **\source\ex02-managingmultipleaktionfilters\begin** ".</span><span class="sxs-lookup"><span data-stu-id="d561c-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="d561c-225">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="d561c-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d561c-226">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="d561c-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d561c-227">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="d561c-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d561c-228">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="d561c-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d561c-229">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="d561c-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="d561c-230">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="d561c-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d561c-231">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="d561c-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d561c-232">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="d561c-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="d561c-233">Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="d561c-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="d561c-234">Fügen Sie dem C# Ordner **Filters** eine neue Klasse hinzu, und nennen Sie Sie *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="d561c-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="d561c-235">Öffnen Sie **MyNewCustomActionFilter.cs** , und fügen Sie einen Verweis auf **System. Web. MVC** und den **mvcmusicstore. Models** -Namespace hinzu:</span><span class="sxs-lookup"><span data-stu-id="d561c-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="d561c-236">(Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX2-mynewcustomaktionfilternamespaces*)</span><span class="sxs-lookup"><span data-stu-id="d561c-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="d561c-237">Ersetzen Sie die Standardklassen Deklaration durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="d561c-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="d561c-238">(Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX2-mynewcustomaction filterClass*)</span><span class="sxs-lookup"><span data-stu-id="d561c-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="d561c-239">Dieser benutzerdefinierte Aktions Filter ist beinahe identisch mit dem Filter, den Sie in der vorherigen Übung erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="d561c-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="d561c-240">Der Hauptunterschied besteht darin, dass die *&quot;von&quot;* Attribut mit diesem neuen Klassennamen protokolliert wurde, um zu ermitteln, welcher Filter das Protokoll registriert hat.</span><span class="sxs-lookup"><span data-stu-id="d561c-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify which filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="d561c-241">Aufgabe 2: Einfügen eines neuen Code Interceptors in die StoreController-Klasse</span><span class="sxs-lookup"><span data-stu-id="d561c-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="d561c-242">In dieser Aufgabe fügen Sie einen neuen benutzerdefinierten Filter in die StoreController-Klasse ein und führen die Projekt Mappe aus, um zu überprüfen, wie beide Filter zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="d561c-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="d561c-243">Öffnen Sie die **StoreController** -Klasse unter **mvcmusicstore\controllers** , und fügen Sie den neuen benutzerdefinierten Filter **mynewcustomaktionfilter** in die **StoreController** -Klasse ein, wie im folgenden Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d561c-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="d561c-244">Führen Sie nun die Anwendung aus, um zu sehen, wie diese beiden benutzerdefinierten Aktionsfilter funktionieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="d561c-245">Drücken Sie dazu **F5** , und warten Sie, bis die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="d561c-246">Navigieren Sie zu **/ActionLog** , um den Anfangszustand der Protokoll Ansicht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="d561c-247">![Protokoll verfolgungsstatus vor Seiten Aktivität](aspnet-mvc-4-custom-action-filters/_static/image5.png "Protokoll verfolgungsstatus vor Seiten Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="d561c-248">*Protokoll verfolgungsstatus vor Seiten Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="d561c-249">Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.</span><span class="sxs-lookup"><span data-stu-id="d561c-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="d561c-250">Überprüfen Sie dieses Mal. Ihre Besuche wurden zweimal nachverfolgt: einmal für jeden benutzerdefinierten Aktionsfilter, den Sie in der **storagecontroller** -Klasse hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="d561c-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="d561c-251">![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image6.png "Aktionsprotokoll mit protokollierter Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="d561c-252">*Aktionsprotokoll mit protokollierter Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="d561c-253">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="d561c-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="d561c-254">Aufgabe 3: Verwalten der Filter Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="d561c-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="d561c-255">In dieser Aufgabe erfahren Sie, wie Sie die Ausführungsreihenfolge der Filter mithilfe der Eigenschaft Order verwalten.</span><span class="sxs-lookup"><span data-stu-id="d561c-255">In this task, you will learn how to manage the filters' execution order by using the Order property.</span></span>

1. <span data-ttu-id="d561c-256">Öffnen Sie die **StoreController** -Klasse unter **mvcmusicstore\controllers** , und geben Sie die **Order** -Eigenschaft in beiden Filtern an, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d561c-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="d561c-257">Überprüfen Sie nun, wie die Filter ausgeführt werden, abhängig vom Wert der Order-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d561c-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="d561c-258">Sie werden feststellen, dass der Filter mit dem kleinsten Bestellwert (**customaktionfilter**) der erste ist, der ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="d561c-259">Drücken Sie **F5** , und warten Sie, bis die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="d561c-260">Navigieren Sie zu **/ActionLog** , um den Anfangszustand der Protokoll Ansicht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="d561c-261">![Protokoll verfolgungsstatus vor Seiten Aktivität](aspnet-mvc-4-custom-action-filters/_static/image7.png "Protokoll verfolgungsstatus vor Seiten Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="d561c-262">*Protokoll verfolgungsstatus vor Seiten Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="d561c-263">Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.</span><span class="sxs-lookup"><span data-stu-id="d561c-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="d561c-264">Überprüfen Sie, ob die Besuche dieses Zeitraums nachverfolgt wurden, geordnet nach dem Reihenfolge Wert der Filter: **customaktionfilter** Logs.</span><span class="sxs-lookup"><span data-stu-id="d561c-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="d561c-265">![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image8.png "Aktionsprotokoll mit protokollierter Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="d561c-266">*Aktionsprotokoll mit protokollierter Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="d561c-267">Nun aktualisieren Sie den Reihenfolge Wert der Filter und überprüfen, wie sich die Protokollierungs Reihenfolge ändert.</span><span class="sxs-lookup"><span data-stu-id="d561c-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="d561c-268">Aktualisieren Sie in der **StoreController** -Klasse den Wert für die Reihenfolge der Filter, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d561c-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="d561c-269">Führen Sie die Anwendung erneut aus, indem Sie **F5**drücken.</span><span class="sxs-lookup"><span data-stu-id="d561c-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="d561c-270">Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.</span><span class="sxs-lookup"><span data-stu-id="d561c-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="d561c-271">Überprüfen Sie, ob die vom **mynewcustomaktionfilter** -Filter erstellten Protokolle zuerst angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="d561c-272">![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image9.png "Aktionsprotokoll mit protokollierter Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="d561c-273">*Aktionsprotokoll mit protokollierter Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="d561c-274">Aufgabe 4: globales Registrieren von Filtern</span><span class="sxs-lookup"><span data-stu-id="d561c-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="d561c-275">In dieser Aufgabe aktualisieren Sie die Projekt Mappe so, dass der neue Filter (**mynewcustomaktionfilter**) als globaler Filter registriert wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="d561c-276">Auf diese Weise wird Sie von allen Aktionen ausgelöst, die in der Anwendung ausgeführt werden, und nicht nur in den StoreController-Anwendungen, wie Sie in der vorherigen Aufgabe ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="d561c-276">By doing this, it will be triggered by all the actions performed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="d561c-277">Entfernen Sie in der **StoreController** -Klasse das Attribut **[mynewcustomaktionfilter]** und die Order-Eigenschaft von **[customaktionfilter]** .</span><span class="sxs-lookup"><span data-stu-id="d561c-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="d561c-278">Dieser sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="d561c-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="d561c-279">Öffnen Sie die Datei **Global. asax** , und suchen Sie die **Anwendung\_Start** -Methode.</span><span class="sxs-lookup"><span data-stu-id="d561c-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="d561c-280">Beachten Sie, dass jedes Mal, wenn die Anwendung gestartet wird, die globalen Filter durch Aufrufen der **registerglobalfilters** -Methode innerhalb der Klasse **FilterConfig** registriert werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="d561c-281">![Registrieren globaler Filter in "Global. asax"](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrieren globaler Filter in "Global. asax"")</span><span class="sxs-lookup"><span data-stu-id="d561c-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="d561c-282">*Registrieren globaler Filter in "Global. asax"*</span><span class="sxs-lookup"><span data-stu-id="d561c-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="d561c-283">Öffnen Sie die Datei **FilterConfig.cs** im **App-\_Ordner Start** .</span><span class="sxs-lookup"><span data-stu-id="d561c-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="d561c-284">Fügen Sie mithilfe von System. Web. MVC; einen Verweis auf hinzu. Verwenden von mvcmusicstore. Filters; Namespace.</span><span class="sxs-lookup"><span data-stu-id="d561c-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="d561c-285">Aktualisieren der **registerglobalfilters** -Methode hinzufügen des benutzerdefinierten Filters.</span><span class="sxs-lookup"><span data-stu-id="d561c-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="d561c-286">Fügen Sie zu diesem Zweck den hervorgehobenen Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d561c-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="d561c-287">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d561c-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="d561c-288">Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.</span><span class="sxs-lookup"><span data-stu-id="d561c-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="d561c-289">Überprüfen Sie, ob jetzt **[mynewcustomaktionfilter]** in HomeController und aktionlogcontroller eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="d561c-290">![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image11.png "Aktionsprotokoll mit protokollierter Aktivität")</span><span class="sxs-lookup"><span data-stu-id="d561c-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="d561c-291">*Aktionsprotokoll mit protokollierter globaler Aktivität*</span><span class="sxs-lookup"><span data-stu-id="d561c-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="d561c-292">Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="d561c-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d561c-293">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d561c-293">Summary</span></span>

<span data-ttu-id="d561c-294">Durch die Durchführung dieses praktischen Labs haben Sie gelernt, wie Sie einen Aktionsfilter zum Ausführen von benutzerdefinierten Aktionen erweitern.</span><span class="sxs-lookup"><span data-stu-id="d561c-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="d561c-295">Sie haben auch gelernt, wie Sie einen beliebigen Filter an Ihre Seiten Controller einfügen.</span><span class="sxs-lookup"><span data-stu-id="d561c-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="d561c-296">Die folgenden Konzepte wurden verwendet:</span><span class="sxs-lookup"><span data-stu-id="d561c-296">The following concepts were used:</span></span>

- <span data-ttu-id="d561c-297">Erstellen von benutzerdefinierten Aktions Filtern mit der ASP.NET MVC Action Filter Attribute-Klasse</span><span class="sxs-lookup"><span data-stu-id="d561c-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="d561c-298">Einfügen von Filtern in ASP.NET-MVC-Controller</span><span class="sxs-lookup"><span data-stu-id="d561c-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="d561c-299">Verwalten der Filter Reihenfolge mit der Order-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="d561c-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="d561c-300">Globales Registrieren von Filtern</span><span class="sxs-lookup"><span data-stu-id="d561c-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d561c-301">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="d561c-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d561c-302">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d561c-303">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="d561c-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d561c-304">Wechseln Sie zu [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d561c-304">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d561c-305">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="d561c-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="d561c-306">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="d561c-306">Click on **Install Now**.</span></span> <span data-ttu-id="d561c-307">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d561c-308">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="d561c-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d561c-309">![Installieren von Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="d561c-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d561c-310">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d561c-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d561c-311">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="d561c-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="d561c-313">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="d561c-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d561c-314">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="d561c-314">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="d561c-316">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="d561c-316">*Installation progress*</span></span>
6. <span data-ttu-id="d561c-317">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-317">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="d561c-319">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="d561c-319">*Installation completed*</span></span>
7. <span data-ttu-id="d561c-320">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="d561c-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d561c-321">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="d561c-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="d561c-323">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="d561c-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d561c-324">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="d561c-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="d561c-325">In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="d561c-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="d561c-326">Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="d561c-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="d561c-327">Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="d561c-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d561c-328">Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="d561c-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="d561c-329">Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-329">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="d561c-330">![Anmelden bei Windows Azure-Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Anmelden bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="d561c-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="d561c-331">*Anmelden bei Windows Azure Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="d561c-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="d561c-332">Klicken Sie in der Befehlsleiste auf **neu** .</span><span class="sxs-lookup"><span data-stu-id="d561c-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="d561c-333">![Erstellen einer neuen Website](aspnet-mvc-4-custom-action-filters/_static/image18.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="d561c-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="d561c-334">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="d561c-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="d561c-335">Klicken Sie auf **Compute** - | **Website**.</span><span class="sxs-lookup"><span data-stu-id="d561c-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="d561c-336">Wählen Sie dann **schneller** Fassung aus.</span><span class="sxs-lookup"><span data-stu-id="d561c-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="d561c-337">Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="d561c-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d561c-338">Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="d561c-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="d561c-339">Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="d561c-340">Sie enthält keine Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d561c-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="d561c-341">![Erstellen einer neuen Website mithilfe der schneller Fassung](aspnet-mvc-4-custom-action-filters/_static/image19.png "Erstellen einer neuen Website mithilfe der schneller Fassung")</span><span class="sxs-lookup"><span data-stu-id="d561c-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="d561c-342">*Erstellen einer neuen Website mithilfe der schneller Fassung*</span><span class="sxs-lookup"><span data-stu-id="d561c-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="d561c-343">Warten Sie, bis die neue **Website** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d561c-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="d561c-344">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte.</span><span class="sxs-lookup"><span data-stu-id="d561c-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="d561c-345">Überprüfen Sie, ob die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d561c-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="d561c-346">![Navigieren zur neuen Website](aspnet-mvc-4-custom-action-filters/_static/image20.png "Navigieren zur neuen Website")</span><span class="sxs-lookup"><span data-stu-id="d561c-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="d561c-347">*Navigieren zur neuen Website*</span><span class="sxs-lookup"><span data-stu-id="d561c-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="d561c-348">![Website wird ausgeführt](aspnet-mvc-4-custom-action-filters/_static/image21.png "Website wird ausgeführt")</span><span class="sxs-lookup"><span data-stu-id="d561c-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="d561c-349">*Website wird ausgeführt*</span><span class="sxs-lookup"><span data-stu-id="d561c-349">*Web site running*</span></span>
6. <span data-ttu-id="d561c-350">Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="d561c-351">![Öffnen der Website-Verwaltungs Seiten](aspnet-mvc-4-custom-action-filters/_static/image22.png "Öffnen der Website-Verwaltungs Seiten")</span><span class="sxs-lookup"><span data-stu-id="d561c-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="d561c-352">*Öffnen der Website-Verwaltungs Seiten*</span><span class="sxs-lookup"><span data-stu-id="d561c-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="d561c-353">Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .</span><span class="sxs-lookup"><span data-stu-id="d561c-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d561c-354">Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="d561c-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="d561c-355">Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="d561c-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="d561c-356">**Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="d561c-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="d561c-357">![Das Website-Veröffentlichungs Profil wird heruntergeladen.](aspnet-mvc-4-custom-action-filters/_static/image23.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")</span><span class="sxs-lookup"><span data-stu-id="d561c-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="d561c-358">*Das Website-Veröffentlichungs Profil wird heruntergeladen.*</span><span class="sxs-lookup"><span data-stu-id="d561c-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="d561c-359">Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="d561c-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="d561c-360">In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="d561c-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="d561c-361">![Die Veröffentlichungs Profil Datei wird gespeichert.](aspnet-mvc-4-custom-action-filters/_static/image24.png "Das Veröffentlichungs Profil wird gespeichert.")</span><span class="sxs-lookup"><span data-stu-id="d561c-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="d561c-362">*Die Veröffentlichungs Profil Datei wird gespeichert.*</span><span class="sxs-lookup"><span data-stu-id="d561c-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="d561c-363">Aufgabe 2: Konfigurieren des Datenbankservers</span><span class="sxs-lookup"><span data-stu-id="d561c-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="d561c-364">Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="d561c-365">Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="d561c-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="d561c-366">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="d561c-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="d561c-367">Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="d561c-368">Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen.</span><span class="sxs-lookup"><span data-stu-id="d561c-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="d561c-369">Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="d561c-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="d561c-370">Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="d561c-371">![Dashboard des SQL-Datenbankservers](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard des SQL-Datenbankservers")</span><span class="sxs-lookup"><span data-stu-id="d561c-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="d561c-372">*Dashboard des SQL-Datenbankservers*</span><span class="sxs-lookup"><span data-stu-id="d561c-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="d561c-373">In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen.</span><span class="sxs-lookup"><span data-stu-id="d561c-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="d561c-374">Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](aspnet-mvc-4-custom-action-filters/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="d561c-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Client-IP-Adresse](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="d561c-376">*Client-IP-Adresse*</span><span class="sxs-lookup"><span data-stu-id="d561c-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="d561c-377">Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Änderungen bestätigen](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="d561c-379">*Änderungen bestätigen*</span><span class="sxs-lookup"><span data-stu-id="d561c-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d561c-380">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="d561c-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="d561c-381">Kehren Sie zur ASP.NET MVC 4-Lösung zurück.</span><span class="sxs-lookup"><span data-stu-id="d561c-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="d561c-382">Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="d561c-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="d561c-383">![Veröffentlichen der Anwendung](aspnet-mvc-4-custom-action-filters/_static/image29.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="d561c-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="d561c-384">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="d561c-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="d561c-385">Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.</span><span class="sxs-lookup"><span data-stu-id="d561c-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="d561c-386">![Importieren des Veröffentlichungs Profils](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importieren des Veröffentlichungs Profils")</span><span class="sxs-lookup"><span data-stu-id="d561c-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="d561c-387">*Veröffentlichungs Profil wird importiert.*</span><span class="sxs-lookup"><span data-stu-id="d561c-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="d561c-388">Klicken Sie auf **Verbindung**überprüfen.</span><span class="sxs-lookup"><span data-stu-id="d561c-388">Click **Validate Connection**.</span></span> <span data-ttu-id="d561c-389">Klicken Sie nach Abschluss der Überprüfung auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="d561c-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d561c-390">Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d561c-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="d561c-391">![Die Verbindung wird überprüft.](aspnet-mvc-4-custom-action-filters/_static/image31.png "Die Verbindung wird überprüft.")</span><span class="sxs-lookup"><span data-stu-id="d561c-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="d561c-392">*Die Verbindung wird überprüft.*</span><span class="sxs-lookup"><span data-stu-id="d561c-392">*Validating connection*</span></span>
4. <span data-ttu-id="d561c-393">Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="d561c-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="d561c-394">![Webbereitstellungs Konfiguration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Webbereitstellungs Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="d561c-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="d561c-395">*Webbereitstellungs Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="d561c-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="d561c-396">Konfigurieren Sie die Datenbankverbindung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d561c-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="d561c-397">Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.</span><span class="sxs-lookup"><span data-stu-id="d561c-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="d561c-398">Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.</span><span class="sxs-lookup"><span data-stu-id="d561c-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="d561c-399">Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.</span><span class="sxs-lookup"><span data-stu-id="d561c-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="d561c-400">Geben Sie einen neuen Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="d561c-400">Type a new database name.</span></span>

     <span data-ttu-id="d561c-401">![Konfigurieren der Ziel Verbindungs Zeichenfolge](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="d561c-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="d561c-402">*Konfigurieren der Ziel Verbindungs Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="d561c-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="d561c-403">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d561c-403">Then click **OK**.</span></span> <span data-ttu-id="d561c-404">Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d561c-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="d561c-405">![Erstellen der Datenbank](aspnet-mvc-4-custom-action-filters/_static/image34.png "Erstellen der Daten Bank Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="d561c-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="d561c-406">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="d561c-406">*Creating the database*</span></span>
7. <span data-ttu-id="d561c-407">Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d561c-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="d561c-408">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d561c-408">Then click **Next**.</span></span>

    <span data-ttu-id="d561c-409">![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](aspnet-mvc-4-custom-action-filters/_static/image35.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")</span><span class="sxs-lookup"><span data-stu-id="d561c-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="d561c-410">*Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="d561c-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="d561c-411">Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="d561c-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="d561c-412">![Veröffentlichen der Webanwendung](aspnet-mvc-4-custom-action-filters/_static/image36.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="d561c-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="d561c-413">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="d561c-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="d561c-414">Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d561c-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="d561c-415">Anhang C: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="d561c-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="d561c-416">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="d561c-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d561c-417">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d561c-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d561c-418">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-custom-action-filters/_static/image37.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="d561c-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d561c-419">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="d561c-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="d561c-420">***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***</span><span class="sxs-lookup"><span data-stu-id="d561c-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="d561c-421">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="d561c-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d561c-422">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="d561c-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d561c-423">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="d561c-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d561c-424">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="d561c-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d561c-425">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="d561c-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="d561c-426">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-custom-action-filters/_static/image38.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="d561c-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="d561c-427">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="d561c-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="d561c-428">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-custom-action-filters/_static/image39.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="d561c-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="d561c-429">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="d561c-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="d561c-430">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-custom-action-filters/_static/image40.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="d561c-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="d561c-431">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="d561c-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="d561c-432">So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="d561c-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="d561c-433">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="d561c-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="d561c-434">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="d561c-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="d561c-435">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="d561c-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="d561c-436">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-custom-action-filters/_static/image41.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="d561c-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="d561c-437">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="d561c-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="d561c-438">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-custom-action-filters/_static/image42.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="d561c-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="d561c-439">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="d561c-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
