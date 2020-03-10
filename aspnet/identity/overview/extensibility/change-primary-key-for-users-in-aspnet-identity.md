---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Ändern des Primärschlüssels für Benutzer in ASP.net Identity-ASP.NET 4. x
author: Rick-Anderson
description: In Visual Studio 2013 verwendet die Standardweb Anwendung einen Zeichen folgen Wert für den Schlüssel für Benutzerkonten. Mit ASP.net Identity können Sie den Typ des...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472263"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="ae753-104">Ändern des Primärschlüssels für Benutzer in ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="ae753-104">Change Primary Key for Users in ASP.NET Identity</span></span>

<span data-ttu-id="ae753-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ae753-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ae753-106">In Visual Studio 2013 verwendet die Standardweb Anwendung einen Zeichen folgen Wert für den Schlüssel für Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="ae753-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="ae753-107">ASP.net Identity ermöglicht es Ihnen, den Typ des Schlüssels zu ändern, um Ihre Datenanforderungen zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="ae753-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="ae753-108">Beispielsweise können Sie den Typ des Schlüssels von einer Zeichenfolge in eine ganze Zahl ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="ae753-109">In diesem Thema wird gezeigt, wie Sie mit der Standardweb Anwendung beginnen und den Benutzerkonto Schlüssel in eine ganze Zahl ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="ae753-110">Sie können dieselben Änderungen verwenden, um beliebige Typen von Schlüsseln in Ihrem Projekt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ae753-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="ae753-111">Es wird gezeigt, wie diese Änderungen in der Standardweb Anwendung vorgenommen werden, aber Sie können ähnliche Änderungen auf eine angepasste Anwendung anwenden.</span><span class="sxs-lookup"><span data-stu-id="ae753-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="ae753-112">Es zeigt die Änderungen, die bei der Arbeit mit MVC oder Web Forms erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="ae753-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ae753-113">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="ae753-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ae753-114">Visual Studio 2013 mit Update 2 (oder höher)</span><span class="sxs-lookup"><span data-stu-id="ae753-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="ae753-115">ASP.net Identity 2,1 oder höher</span><span class="sxs-lookup"><span data-stu-id="ae753-115">ASP.NET Identity 2.1 or later</span></span>

<span data-ttu-id="ae753-116">Damit Sie die Schritte in diesem Tutorial ausführen können, müssen Sie über Visual Studio 2013 Update 2 (oder höher) und eine Webanwendung verfügen, die aus der ASP.net-Webanwendungsvorlage erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ae753-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="ae753-117">Die Vorlage wurde in Update 3 geändert.</span><span class="sxs-lookup"><span data-stu-id="ae753-117">The template changed in Update 3.</span></span> <span data-ttu-id="ae753-118">In diesem Thema wird gezeigt, wie Sie die Vorlage in Update 2 und Update 3 ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="ae753-119">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="ae753-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="ae753-120">Ändern Sie den Typ des Schlüssels in der Identity-Benutzerklasse.</span><span class="sxs-lookup"><span data-stu-id="ae753-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="ae753-121">Hinzufügen von angepassten Identitäts Klassen, die den Schlüsseltyp verwenden</span><span class="sxs-lookup"><span data-stu-id="ae753-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="ae753-122">Ändern der Kontext Klasse und des Benutzer-Managers, um den Schlüsseltyp zu verwenden</span><span class="sxs-lookup"><span data-stu-id="ae753-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="ae753-123">Ändern Sie die Startkonfiguration, sodass der Schlüsseltyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="ae753-124">Ändern Sie für MVC mit Update 2 den Kontotyp AccountController, um den Schlüsseltyp zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="ae753-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="ae753-125">Ändern Sie für MVC mit Update 3 AccountController und managecontroller so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="ae753-126">Ändern Sie für Web Forms mit Update 2 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="ae753-127">Ändern Sie für Web Forms mit Update 3 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="ae753-128">Anwendung ausführen</span><span class="sxs-lookup"><span data-stu-id="ae753-128">Run application</span></span>](#run)
- [<span data-ttu-id="ae753-129">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ae753-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="ae753-130">Ändern Sie den Typ des Schlüssels in der Identity-Benutzerklasse.</span><span class="sxs-lookup"><span data-stu-id="ae753-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="ae753-131">Geben Sie in Ihrem Projekt, das aus der Vorlage ASP.NET-Webanwendung erstellt wurde, an, dass die ApplicationUser-Klasse eine Ganzzahl für den Schlüssel für Benutzerkonten verwendet.</span><span class="sxs-lookup"><span data-stu-id="ae753-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="ae753-132">Ändern Sie in IdentityModels.cs die ApplicationUser-Klasse so, dass Sie von identityuser geerbt wird, der den Typ **int** für den generischen TKey-Parameter aufweist.</span><span class="sxs-lookup"><span data-stu-id="ae753-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="ae753-133">Außerdem übergeben Sie die Namen von drei angepassten Klassen, die Sie noch nicht implementiert haben.</span><span class="sxs-lookup"><span data-stu-id="ae753-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="ae753-134">Sie haben den Typ des Schlüssels geändert, aber standardmäßig geht der Rest der Anwendung davon aus, dass der Schlüssel eine Zeichenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="ae753-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="ae753-135">Sie müssen den Typ des Schlüssels in Code, der eine Zeichenfolge annimmt, explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="ae753-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="ae753-136">Ändern Sie in der **ApplicationUser** -Klasse die **generateuseridentityasync** -Methode in "int", wie im folgenden hervorgehobenen Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ae753-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="ae753-137">Diese Änderung ist für Web Forms Projekte mit der Vorlage Update 3 nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ae753-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="ae753-138">Hinzufügen von angepassten Identitäts Klassen, die den Schlüsseltyp verwenden</span><span class="sxs-lookup"><span data-stu-id="ae753-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="ae753-139">Die anderen Identitäts Klassen, z. b. identityuserrole, identityuserclaim, identityuserlogin, identityrole, userstore, rolestore, sind immer noch für die Verwendung eines Zeichen folgen Schlüssels festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ae753-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="ae753-140">Erstellen Sie neue Versionen dieser Klassen, die eine ganze Zahl für den Schlüssel angeben.</span><span class="sxs-lookup"><span data-stu-id="ae753-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="ae753-141">Sie müssen in diesen Klassen nicht viel Implementierungs Code bereitstellen, Sie legen primär einfach int als Schlüssel fest.</span><span class="sxs-lookup"><span data-stu-id="ae753-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="ae753-142">Fügen Sie der IdentityModels.cs-Datei die folgenden Klassen hinzu.</span><span class="sxs-lookup"><span data-stu-id="ae753-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="ae753-143">Ändern der Kontext Klasse und des Benutzer-Managers, um den Schlüsseltyp zu verwenden</span><span class="sxs-lookup"><span data-stu-id="ae753-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="ae753-144">Ändern Sie in IdentityModels.cs die Definition der **applicationdbcontext** -Klasse so, dass die neuen angepassten Klassen und ein **int** für den Schlüssel verwendet werden, wie im hervorgehobenen Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ae753-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="ae753-145">Der ThrowIfV1Schema-Parameter ist im Konstruktor nicht mehr gültig.</span><span class="sxs-lookup"><span data-stu-id="ae753-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="ae753-146">Ändern Sie den Konstruktor, sodass er keinen ThrowIfV1Schema-Wert übergibt.</span><span class="sxs-lookup"><span data-stu-id="ae753-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="ae753-147">Öffnen Sie IdentityConfig.cs, und ändern Sie die **applicationusermanager** -Klasse so, dass Ihre neue Benutzerspeicher Klasse zum Beibehalten von Daten und eine **int** -Taste für den Schlüssel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="ae753-148">In der Vorlage Update 3 müssen Sie die applicationsigninmanager-Klasse ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="ae753-149">Ändern Sie die Startkonfiguration, sodass der Schlüsseltyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="ae753-150">Ersetzen Sie in Startup.auth.cs den onvalidateidentity-Code, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ae753-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="ae753-151">Beachten Sie, dass die getuseridcallback-Definition den Zeichen folgen Wert in eine ganze Zahl analysiert.</span><span class="sxs-lookup"><span data-stu-id="ae753-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="ae753-152">Wenn das Projekt die generische Implementierung der **GetUserID** -Methode nicht erkennt, müssen Sie möglicherweise das ASP.net Identity nuget-Paket auf Version 2,1 aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ae753-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="ae753-153">Sie haben viele Änderungen an den von ASP.net Identity verwendeten Infrastruktur Klassen vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="ae753-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="ae753-154">Wenn Sie versuchen, das Projekt zu kompilieren, werden viele Fehler feststellen.</span><span class="sxs-lookup"><span data-stu-id="ae753-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="ae753-155">Glücklicherweise sind die restlichen Fehler alle ähnlich.</span><span class="sxs-lookup"><span data-stu-id="ae753-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="ae753-156">Die Identity-Klasse erwartet eine ganze Zahl für den Schlüssel, aber der Controller (oder das Webformular) übergibt einen Zeichen folgen Wert.</span><span class="sxs-lookup"><span data-stu-id="ae753-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="ae753-157">In jedem Fall müssen Sie eine Konvertierung von einer Zeichenfolge in eine ganze Zahl und eine ganze Zahl durch Aufrufen von **GetUserID&lt;int&gt;** durchsetzen.</span><span class="sxs-lookup"><span data-stu-id="ae753-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="ae753-158">Sie können entweder die Fehlerliste aus der Kompilierung bearbeiten oder die unten aufgeführten Änderungen befolgen.</span><span class="sxs-lookup"><span data-stu-id="ae753-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="ae753-159">Die restlichen Änderungen hängen vom Projekttyp ab, den Sie erstellen, und von dem Update, das Sie in Visual Studio installiert haben.</span><span class="sxs-lookup"><span data-stu-id="ae753-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="ae753-160">Sie können über die folgenden Links direkt zum entsprechenden Abschnitt wechseln.</span><span class="sxs-lookup"><span data-stu-id="ae753-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="ae753-161">Ändern Sie für MVC mit Update 2 den Kontotyp AccountController, um den Schlüsseltyp zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="ae753-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="ae753-162">Ändern Sie für MVC mit Update 3 AccountController und managecontroller so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="ae753-163">Ändern Sie für Web Forms mit Update 2 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="ae753-164">Ändern Sie für Web Forms mit Update 3 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="ae753-165">Ändern Sie für MVC mit Update 2 den Kontotyp AccountController, um den Schlüsseltyp zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="ae753-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="ae753-166">Öffnen Sie die Datei AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="ae753-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="ae753-167">Sie müssen die folgenden Methoden ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-167">You need to change the following methods.</span></span>

<span data-ttu-id="ae753-168">**Confirmemail** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="ae753-169">Methode **diszuordnen**</span><span class="sxs-lookup"><span data-stu-id="ae753-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="ae753-170">**Manage (manageuserviewmodel)** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="ae753-171">**Linklogincallback** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="ae753-172">**Removeaccountlist** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="ae753-173">**HasPassword** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="ae753-174">Sie können jetzt [die Anwendung ausführen](#run) und einen neuen Benutzer registrieren.</span><span class="sxs-lookup"><span data-stu-id="ae753-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="ae753-175">Ändern Sie für MVC mit Update 3 AccountController und managecontroller so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="ae753-176">Öffnen Sie die Datei AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="ae753-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="ae753-177">Sie müssen die folgende Methode ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-177">You need to change the following method.</span></span>

<span data-ttu-id="ae753-178">**Confirmemail** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="ae753-179">**Sendcode** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="ae753-180">Öffnen Sie die Datei ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="ae753-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="ae753-181">Sie müssen die folgenden Methoden ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-181">You need to change the following methods.</span></span>

<span data-ttu-id="ae753-182">**Index** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="ae753-183">**Removelogin** -Methoden</span><span class="sxs-lookup"><span data-stu-id="ae753-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="ae753-184">**Addphonenumber** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="ae753-185">**Enabletwofactorauthentication** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="ae753-186">**Disabletwofactorauthentication** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="ae753-187">**Verifyphonenumber** -Methoden</span><span class="sxs-lookup"><span data-stu-id="ae753-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="ae753-188">**Removephonenumber** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="ae753-189">**ChangePassword** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="ae753-190">**SetPassword** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="ae753-191">**Managelogins** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="ae753-192">**Linklogincallback** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="ae753-193">**HasPassword** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="ae753-194">**Hasphonenumber** -Methode</span><span class="sxs-lookup"><span data-stu-id="ae753-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="ae753-195">Sie können jetzt [die Anwendung ausführen](#run) und einen neuen Benutzer registrieren.</span><span class="sxs-lookup"><span data-stu-id="ae753-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="ae753-196">Ändern Sie für Web Forms mit Update 2 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="ae753-197">Für Web Forms mit Update 2 müssen Sie die folgenden Seiten ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="ae753-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="ae753-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="ae753-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="ae753-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="ae753-201">Sie können jetzt [die Anwendung ausführen](#run) und einen neuen Benutzer registrieren.</span><span class="sxs-lookup"><span data-stu-id="ae753-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="ae753-202">Ändern Sie für Web Forms mit Update 3 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ae753-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="ae753-203">Für Web Forms mit Update 3 müssen Sie die folgenden Seiten ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="ae753-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="ae753-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="ae753-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="ae753-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="ae753-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="ae753-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="ae753-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="ae753-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="ae753-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="ae753-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="ae753-212">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="ae753-212">Run application</span></span>

<span data-ttu-id="ae753-213">Sie haben alle erforderlichen Änderungen an der Standardweb Application-Vorlage abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="ae753-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="ae753-214">Führen Sie die Anwendung aus, und registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="ae753-214">Run the application and register a new user.</span></span> <span data-ttu-id="ae753-215">Nachdem Sie den Benutzer registriert haben, werden Sie bemerken, dass die aspnettusers-Tabelle eine ID-Spalte aufweist, die eine ganze Zahl ist.</span><span class="sxs-lookup"><span data-stu-id="ae753-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![neuer Primärschlüssel](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="ae753-217">Wenn Sie zuvor die ASP.net Identity Tabellen mit einem anderen Primärschlüssel erstellt haben, müssen Sie einige zusätzliche Änderungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="ae753-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="ae753-218">Löschen Sie die vorhandene Datenbank, wenn möglich.</span><span class="sxs-lookup"><span data-stu-id="ae753-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="ae753-219">Die Datenbank wird mit dem richtigen Entwurf neu erstellt, wenn Sie die Webanwendung ausführen und einen neuen Benutzer hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ae753-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="ae753-220">Wenn das Löschen nicht möglich ist, führen Sie Code First-Migrationen aus, um die Tabellen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ae753-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="ae753-221">Der neue ganzzahlige Primärschlüssel wird jedoch nicht als SQL-Identitäts Eigenschaft in der Datenbank eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="ae753-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="ae753-222">Sie müssen die ID-Spalte manuell als Identität festlegen.</span><span class="sxs-lookup"><span data-stu-id="ae753-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="ae753-223">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ae753-223">Other resources</span></span>

- [<span data-ttu-id="ae753-224">Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="ae753-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="ae753-225">Migrieren einer vorhandenen Website von einem SQL-Mitgliedschaftsanbieter nach ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="ae753-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="ae753-226">Migrieren von universellen Anbieter Daten für Mitgliedschafts-und Benutzerprofile zu ASP.net Identity</span><span class="sxs-lookup"><span data-stu-id="ae753-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="ae753-227">[Beispielanwendung](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) mit geändertem Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="ae753-227">[Sample application](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) with changed primary key</span></span>
