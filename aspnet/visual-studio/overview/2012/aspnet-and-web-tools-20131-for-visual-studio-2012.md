---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Anmerkungen zu dieser Version von ASP.net and Web Tools 2013,1 für Visual Studio 2012 | Microsoft-Dokumentation
author: microsoft
description: In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools 2013,1 für Visual Studio 2012 beschrieben.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467103"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="b4b0e-103">Anmerkungen zu dieser Version – ASP.NET and Web Tools 2013.1 für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b4b0e-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="b4b0e-104">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b4b0e-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b4b0e-105">In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools 2013,1 für Visual Studio 2012 beschrieben.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="b4b0e-106">Inhalt</span><span class="sxs-lookup"><span data-stu-id="b4b0e-106">Contents</span></span>

- [<span data-ttu-id="b4b0e-107">Installationshinweise</span><span class="sxs-lookup"><span data-stu-id="b4b0e-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="b4b0e-108">Software Anforderungen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="b4b0e-109">Neue Features in ASP.net and Web Tools 2013,1 für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b4b0e-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="b4b0e-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="b4b0e-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="b4b0e-111">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="b4b0e-112">ASP.NET MVC 5-Vorlage</span><span class="sxs-lookup"><span data-stu-id="b4b0e-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="b4b0e-113">Vorlage für ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="b4b0e-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="b4b0e-114">Element Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="b4b0e-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b4b0e-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="b4b0e-116">ASP.net Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="b4b0e-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="b4b0e-117">Razor-Editor</span><span class="sxs-lookup"><span data-stu-id="b4b0e-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="b4b0e-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="b4b0e-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="b4b0e-119">Bekannte Probleme und wichtige Änderungen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="b4b0e-120">ASP.net Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="b4b0e-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="b4b0e-121">MVC-und Web-API-Gerüstbau: Fehler HTTP 404, nicht gefunden</span><span class="sxs-lookup"><span data-stu-id="b4b0e-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="b4b0e-122">Visual Studio Express 2012 für Web funktioniert nach dem Hinzufügen eines Gerüsts nicht mehr</span><span class="sxs-lookup"><span data-stu-id="b4b0e-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="b4b0e-123">ASP.net Razor 3</span><span class="sxs-lookup"><span data-stu-id="b4b0e-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="b4b0e-124">Das Anzeigen der cshtml-Datei mit "Durchsuchen" oder "F5" verursacht einen Server Fehler</span><span class="sxs-lookup"><span data-stu-id="b4b0e-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="b4b0e-125">URL-Rewrite und Tilde (~)</span><span class="sxs-lookup"><span data-stu-id="b4b0e-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="b4b0e-126">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="b4b0e-127">Installationshinweise</span><span class="sxs-lookup"><span data-stu-id="b4b0e-127">Installation Notes</span></span>

<span data-ttu-id="b4b0e-128">[Installieren](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) von ASP.net and Web Tools 2013,1 für Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="b4b0e-129">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-129">Software Requirements</span></span>

<span data-ttu-id="b4b0e-130">Sie müssen entweder über Visual Studio 2012 oder Visual Studio Express 2012 für das Web verfügen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="b4b0e-131">Neue Features in ASP.net and Web Tools 2013,1 für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b4b0e-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="b4b0e-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="b4b0e-132">Bootstrap</span></span>

<span data-ttu-id="b4b0e-133">Wenn Sie MVC 5-Controller und-Ansichten Gerüstbau, wird das Markup für die Ansichten [Bootstrap](http://getbootstrap.com/)verwendet.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="b4b0e-134">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="b4b0e-135">ASP.NET MVC 5-Vorlage</span><span class="sxs-lookup"><span data-stu-id="b4b0e-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="b4b0e-136">Wir haben eine neue MVC 5-Vorlage hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="b4b0e-137">Er verweist auf die neuesten MVC 5 nuget-Pakete, und Sie können das Gerüst zum Hinzufügen von Controllern und Ansichten verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="b4b0e-138">Vorlage für ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="b4b0e-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="b4b0e-139">Wir haben eine neue Web-API 2-Vorlage hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="b4b0e-140">Er verweist auf die neuesten nuget-Pakete der Web-API 2, und Sie können das Gerüst zum Hinzufügen von Controllern und Ansichten verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="b4b0e-141">Element Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-141">Item Templates</span></span>

<span data-ttu-id="b4b0e-142">Wir haben neue Element Vorlagen für MVC 5-Ansichten, Webseiten (Razor 3) und Web-API 2-Controller hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="b4b0e-143">Beim Hinzufügen neuer Elemente werden die entsprechenden nuget-Pakete für das Projekt installiert.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="b4b0e-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b4b0e-144">Entity Framework 6</span></span>

<span data-ttu-id="b4b0e-145">Wenn Sie einen MVC-oder Web-API-Controller mithilfe Entity Framework Gerüstbau, verwenden wir Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="b4b0e-146">Weitere Informationen zu Entity Framework finden Sie im [Versionsverlauf Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="b4b0e-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="b4b0e-147">Sie können auch die Entity Framework 6-Tools für Visual Studio 2012 herunterladen und installieren.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="b4b0e-148">Weitere Informationen finden [Sie unter Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="b4b0e-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="b4b0e-149">ASP.net Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="b4b0e-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="b4b0e-150">ASP.net Gerüstbau ist ein Code Generierungs Framework für ASP.NET-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="b4b0e-151">Es vereinfacht das Hinzufügen von Code Bausteinen zu Ihrem Projekt, das mit einem Datenmodell interagiert.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="b4b0e-152">In früheren Versionen von Visual Studio waren Gerüstbau auf ASP.NET MVC-Projekte beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="b4b0e-153">Mit diesem Update können Sie jetzt Gerüstbau für alle ASP.net-Projekte verwenden, einschließlich Web Forms.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="b4b0e-154">Dieses Update unterstützt das Erstellen von Seiten für ein Web Forms Projekt nicht, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt MVC-Abhängigkeiten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="b4b0e-155">Die Unterstützung für das Erstellen von Seiten für Web Forms wird in einem späteren Update hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="b4b0e-156">Wenn Sie Gerüstbau verwenden, stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="b4b0e-157">Wenn Sie z. b. mit einem ASP.net-Web Forms Projekt beginnen und dann mithilfe von Gerüstbau einen Web-API-Controller hinzufügen, werden die erforderlichen nuget-Pakete und-Verweise automatisch dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="b4b0e-158">Fügen Sie ein **Neues Gerüstbau Element** hinzu, und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster aus, um einem Web Forms Projekt MVC-Gerüstbau hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="b4b0e-159">Es gibt zwei Optionen für das Gerüstbau von MVC: Minimal und vollständig.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="b4b0e-160">Wenn Sie minimal auswählen, werden nur die nuget-Pakete und-Verweise für ASP.NET MVC dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="b4b0e-161">Wenn Sie die Option vollständig auswählen, werden die minimalen Abhängigkeiten sowie die erforderlichen Inhalts Dateien für ein MVC-Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="b4b0e-162">Die Unterstützung für Gerüstbau-asynchrone Controller verwendet die neuen Async-Features aus Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="b4b0e-163">Weitere Informationen und Tutorials finden Sie unter [Übersicht über ASP.net Gerüstbau](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4b0e-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="b4b0e-164">In diesen Tutorials wird Gerüstbau mit Visual Studio 2013 angezeigt, Sie sind jedoch auch auf ASP.net and Web Tools 2013,1 für Visual Studio 2012 anwendbar.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="b4b0e-165">Razor-Editor</span><span class="sxs-lookup"><span data-stu-id="b4b0e-165">Razor Editor</span></span>

<span data-ttu-id="b4b0e-166">Mit diesem Update unterstützt Visual Studio 2012 jetzt Razor 3-Tools/-Bearbeitung.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="b4b0e-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="b4b0e-167">NuGet 2.7</span></span>

<span data-ttu-id="b4b0e-168">Nuget 2,7 umfasst einen umfangreichen Satz neuer Features, die in den Anmerkungen zur [nuget-Version 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7)ausführlich beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="b4b0e-169">Mit dieser Version von nuget entfällt die Notwendigkeit, dass Benutzer explizit zulassen, dass Pakete fehlende Pakete wiederherstellen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="b4b0e-170">Bei der Installation von nuget 2,7 Stimmen die Benutzer implizit zu, dass fehlende Pakete automatisch wieder hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="b4b0e-171">Benutzer können die Paket Wiederherstellung über die nuget-Einstellungen in Visual Studio explizit ablehnen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="b4b0e-172">Diese Änderung vereinfacht die Funktionsweise der Paket Wiederherstellung.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="b4b0e-173">Bekannte Probleme und wichtige Änderungen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="b4b0e-174">ASP.net Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="b4b0e-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="b4b0e-175">MVC-und Web-API-Gerüstbau: Fehler HTTP 404, nicht gefunden</span><span class="sxs-lookup"><span data-stu-id="b4b0e-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="b4b0e-176">Wenn ein Fehler auftritt, wenn Sie einem Projekt ein Gerüst Element hinzufügen, ist es möglich, dass das Projekt in einem inkonsistenten Zustand verbleibt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="b4b0e-177">Für einige der Änderungen, die an Gerüstbau vorgenommen werden, wird ein Rollback ausgeführt, aber für andere Änderungen, wie z. b</span><span class="sxs-lookup"><span data-stu-id="b4b0e-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="b4b0e-178">Wenn für die Routing Konfigurationsänderungen ein Rollback ausgeführt wird, erhalten Benutzer einen HTTP 404-Fehler, wenn Sie zu Gerüst Elementen navigieren.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="b4b0e-179">Um diesen Fehler für MVC zu beheben, fügen Sie ein neues Gerüst Element hinzu, und wählen Sie MVC 5-Abhängigkeiten (Minimal oder vollständig) aus.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="b4b0e-180">Durch diesen Vorgang werden alle erforderlichen Änderungen in Ihrem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="b4b0e-181">So beheben Sie diesen Fehler für die Web-API:</span><span class="sxs-lookup"><span data-stu-id="b4b0e-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="b4b0e-182">Fügen Sie dem Projekt die folgende webapiconfig-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="b4b0e-183">Konfigurieren Sie webapiconfig. Register in der Anwendung\_Start-Methode in Global. asax wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b4b0e-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="b4b0e-184">Visual Studio Express 2012 für Web funktioniert nach dem Hinzufügen eines Gerüsts nicht mehr</span><span class="sxs-lookup"><span data-stu-id="b4b0e-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="b4b0e-185">Wenn Visual Studio Express 2012 für Web nicht mehr funktioniert, nachdem Sie ein Gerüst Element mit Entity Framework hinzugefügt haben (z. b. Web-API 2-Controller mit Aktionen, mithilfe Entity Framework), ist es möglich, dass Visual Studio Express das Native Image einer Assembly nicht laden konnte. abhängig von "System. Web. Extensions".</span><span class="sxs-lookup"><span data-stu-id="b4b0e-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="b4b0e-186">Um dieses Problem zu beheben, konfigurieren Sie Visual Studio Express, um mit dem MSIL-Image von System. Web. Extensions zu arbeiten:</span><span class="sxs-lookup"><span data-stu-id="b4b0e-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="b4b0e-187">Öffnen Sie die Eingabeaufforderung im Administrator Modus.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="b4b0e-188">Wechseln Sie zu%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE oder% Program Files (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (für 64-Bit-Fenster).</span><span class="sxs-lookup"><span data-stu-id="b4b0e-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="b4b0e-189">Öffnen Sie vwdebug. exe. config in einem Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="b4b0e-190">Fügen Sie die folgende Zeile unter dem &lt;Configuration&gt;/&lt;Runtime-&gt; Element hinzu:</span><span class="sxs-lookup"><span data-stu-id="b4b0e-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="b4b0e-191">Starten Sie Visual Studio Express 2012 für das Web neu.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="b4b0e-192">ASP.net Razor 3</span><span class="sxs-lookup"><span data-stu-id="b4b0e-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="b4b0e-193">Das Anzeigen der cshtml-Datei mit "Durchsuchen" oder "F5" verursacht einen Server Fehler</span><span class="sxs-lookup"><span data-stu-id="b4b0e-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="b4b0e-194">Wenn Sie ein MVC 5-Projekt in Visual Studio 2012 erstellen (oder in Visual Studio 2012 ein MVC 5-Projekt öffnen, das in Visual Studio 2013 erstellt wurde) und versuchen, eine cshtml-Datei mithilfe von "Durchsuchen" oder "F5" anzuzeigen, erhalten Sie einen Fehler, der besagt, dass der **Server Fehler in der Anwendung "/" angezeigt**wird.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="b4b0e-195">Der Server versucht, zu `http://localhost:XXXX/Views/../XXXX.cshtml` zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="b4b0e-196">Um dieses Problem zu beheben, ändern Sie die Einstellung für die **Start Aktion** in Ihrem Projekt auf eine **bestimmte Seite**.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="b4b0e-197">Es ist nicht erforderlich, einen Wert für die Seite anzugeben.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="b4b0e-198">Nachdem Sie diese Änderung vorgenommen haben, wechselt die Auswahl von F5 zum Stamm der Anwendung (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="b4b0e-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="b4b0e-199">Dieses Verhalten ist nicht identisch mit dem Verhalten für MVC 5-Projekte in Visual Studio 2013, in dem die **Aktuelle Seiten** Einstellung die geöffnete Seite öffnet.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="b4b0e-200">URL-Rewrite und Tilde (~)</span><span class="sxs-lookup"><span data-stu-id="b4b0e-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="b4b0e-201">Nach dem Upgrade auf ASP.net Razor 3 oder ASP.NET MVC 5 funktioniert die Tilde (~)-Notation möglicherweise nicht mehr ordnungsgemäß, wenn Sie URL-Neuschreibungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="b4b0e-202">Die URL-Umschreibung wirkt sich auf die Tilde (~)-Notation in HTML-Elementen aus, z. b. &lt;A/&gt;, &lt;Script/&gt;, &lt;Link/&gt;. Folglich wird die Tilde nicht mehr dem Stammverzeichnis zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="b4b0e-203">Wenn Sie z. b. Anforderungen für **ASP.NET/Content** zu **ASP.net**umschreiben, wird das href-Attribut in &lt;A href = "~/Content/"/&gt; in **/Content/Content/** anstatt **/** aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="b4b0e-204">Um diese Änderung zu unterdrücken, können Sie den **IIS-\_wasurlumgeschriebene** -Kontext auf jeder Webseite oder in **Application\_BeginRequest** in Global. asax auf false festlegen.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="b4b0e-205">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b4b0e-205">Templates</span></span>

<span data-ttu-id="b4b0e-206">Wenn Sie ASP.NET MVC-Projekte mit Visual Studio 2012 auf Windows 8.1 oder Windows Server 2012 R2 erstellen, zeigt Visual Studio eine Fehlermeldung an, die besagt, dass die Konfiguration von Web [URL] für ASP.NET 4,5 fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Konfigurationsfehler](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="b4b0e-208">Dieser Fehler wird angezeigt, da Visual Studio 2012 die Funktion ASP.NET 4,5 nicht aktiviert, wenn Sie auf diesen Versionen von Windows installiert ist.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="b4b0e-209">Um ASP.NET 4,5 zu aktivieren, führen Sie die unter Aktivieren [oder Deaktivieren von Windows-Features](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)beschriebenen Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Aktivieren oder Deaktivieren von Windows-Features](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="b4b0e-211">Wahlweise können Sie ASP.NET 4,5 auch über die Befehlszeile aktivieren.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="b4b0e-212">Öffnen Sie die Eingabeaufforderung im Administrator Modus.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="b4b0e-213">Führen Sie den folgenden Befehl aus, um ASP.NET 4,5 zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="b4b0e-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
