---
uid: signalr/overview/security/hub-authorization
title: Authentifizierung und Autorisierung für signalr-Hubs | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können. Die in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr ve...
ms.author: bradyg
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 5006af5e623da6958a6d59949c6f2cf776c77fc3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467511"
---
# <a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="dab05-104">Authentifizierung und Autorisierung für SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="dab05-104">Authentication and Authorization for SignalR Hubs</span></span>

<span data-ttu-id="dab05-105">von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dab05-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="dab05-106">In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="dab05-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="dab05-107">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="dab05-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="dab05-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="dab05-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="dab05-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="dab05-109">.NET 4.5</span></span>
> - <span data-ttu-id="dab05-110">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="dab05-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="dab05-111">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="dab05-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="dab05-112">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="dab05-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="dab05-113">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="dab05-113">Questions and comments</span></span>
>
> <span data-ttu-id="dab05-114">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="dab05-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="dab05-115">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="dab05-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="dab05-116">Übersicht</span><span class="sxs-lookup"><span data-stu-id="dab05-116">Overview</span></span>

<span data-ttu-id="dab05-117">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="dab05-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="dab05-118">Attribut autorisieren</span><span class="sxs-lookup"><span data-stu-id="dab05-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="dab05-119">Authentifizierung für alle Hubs erforderlich</span><span class="sxs-lookup"><span data-stu-id="dab05-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="dab05-120">Angepasste Autorisierung</span><span class="sxs-lookup"><span data-stu-id="dab05-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="dab05-121">Übergeben von Authentifizierungsinformationen an Clients</span><span class="sxs-lookup"><span data-stu-id="dab05-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="dab05-122">Authentifizierungs Optionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="dab05-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="dab05-123">Cookie mit Formular Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="dab05-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="dab05-124">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="dab05-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="dab05-125">Verbindungs Header</span><span class="sxs-lookup"><span data-stu-id="dab05-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="dab05-126">Certificate</span><span class="sxs-lookup"><span data-stu-id="dab05-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="dab05-127">Attribut autorisieren</span><span class="sxs-lookup"><span data-stu-id="dab05-127">Authorize attribute</span></span>

<span data-ttu-id="dab05-128">Signalr stellt das [Autorisierungs](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut bereit, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder eine Methode haben.</span><span class="sxs-lookup"><span data-stu-id="dab05-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="dab05-129">Dieses Attribut befindet sich im `Microsoft.AspNet.SignalR`-Namespace.</span><span class="sxs-lookup"><span data-stu-id="dab05-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="dab05-130">Sie wenden das `Authorize`-Attribut entweder auf einen Hub oder bestimmte Methoden in einem Hub an.</span><span class="sxs-lookup"><span data-stu-id="dab05-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="dab05-131">Wenn Sie das `Authorize`-Attribut auf eine hubklasse anwenden, wird die angegebene Autorisierungs Anforderung auf alle Methoden im Hub angewendet.</span><span class="sxs-lookup"><span data-stu-id="dab05-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="dab05-132">Dieses Thema enthält Beispiele für die verschiedenen Typen von Autorisierungs Anforderungen, die Sie anwenden können.</span><span class="sxs-lookup"><span data-stu-id="dab05-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="dab05-133">Ohne das `Authorize`-Attribut kann ein verbundener Client auf jede öffentliche Methode auf dem Hub zugreifen.</span><span class="sxs-lookup"><span data-stu-id="dab05-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="dab05-134">Wenn Sie in Ihrer Webanwendung eine Rolle mit dem Namen "admin" definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle mit folgendem Code auf einen Hub zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="dab05-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="dab05-135">Oder Sie können angeben, dass ein Hub eine Methode enthält, die für alle Benutzer verfügbar ist, und eine zweite Methode, die nur authentifizierten Benutzern zur Verfügung steht, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="dab05-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="dab05-136">In den folgenden Beispielen werden verschiedene Autorisierungs Szenarien behandelt:</span><span class="sxs-lookup"><span data-stu-id="dab05-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="dab05-137">`[Authorize]` – nur authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="dab05-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="dab05-138">`[Authorize(Roles = "Admin,Manager")]` – nur authentifizierte Benutzer in den angegebenen Rollen</span><span class="sxs-lookup"><span data-stu-id="dab05-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="dab05-139">`[Authorize(Users = "user1,user2")]` – nur authentifizierte Benutzer mit den angegebenen Benutzernamen</span><span class="sxs-lookup"><span data-stu-id="dab05-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="dab05-140">`[Authorize(RequireOutgoing=false)]` – nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server zurück an Clients werden nicht durch Autorisierung eingeschränkt, wie z. b., wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen Personen die Nachricht empfangen können.</span><span class="sxs-lookup"><span data-stu-id="dab05-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="dab05-141">Die Eigenschaft "Requirements Outgoing" kann nur auf den gesamten Hub angewendet werden, nicht auf die Einzelpersonen Methoden innerhalb des Hubs.</span><span class="sxs-lookup"><span data-stu-id="dab05-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="dab05-142">Wenn "Requirements Outgoing" nicht auf "false" festgelegt ist, werden nur Benutzer aufgerufen, die die Autorisierungs Anforderung erfüllen.</span><span class="sxs-lookup"><span data-stu-id="dab05-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="dab05-143">Authentifizierung für alle Hubs erforderlich</span><span class="sxs-lookup"><span data-stu-id="dab05-143">Require authentication for all hubs</span></span>

<span data-ttu-id="dab05-144">Sie können eine Authentifizierung für alle Hubs und hubmethoden in Ihrer Anwendung anfordern, indem Sie die Methode "Requirements [Authentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) " aufrufen, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="dab05-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="dab05-145">Sie können diese Methode verwenden, wenn Sie über mehrere Hubs verfügen und eine Authentifizierungsanforderung für alle erzwingen möchten.</span><span class="sxs-lookup"><span data-stu-id="dab05-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="dab05-146">Mit dieser Methode können Sie keine Anforderungen für die Rollen-, Benutzer-oder ausgehende Autorisierung angeben.</span><span class="sxs-lookup"><span data-stu-id="dab05-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="dab05-147">Sie können nur angeben, dass der Zugriff auf die hubmethoden auf authentifizierte Benutzer beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="dab05-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="dab05-148">Allerdings können Sie das Autorisierungs Attribut weiterhin auf Hubs oder Methoden anwenden, um zusätzliche Anforderungen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="dab05-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="dab05-149">Jede Anforderung, die Sie in einem Attribut angeben, wird der grundlegenden Anforderung der Authentifizierung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="dab05-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="dab05-150">Das folgende Beispiel zeigt eine Startdatei, die alle hubmethoden auf authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="dab05-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="dab05-151">Wenn Sie die `RequireAuthentication()`-Methode nach der Verarbeitung einer signalr-Anforderung aufzurufen, löst signalr eine `InvalidOperationException`-Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="dab05-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="dab05-152">Signalr löst diese Ausnahme aus, da Sie der hubpipeline kein Modul hinzufügen können, nachdem die Pipeline aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="dab05-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="dab05-153">Das vorherige Beispiel zeigt, wie Sie die `RequireAuthentication`-Methode in der `Configuration`-Methode aufrufen, die einmal vor der Verarbeitung der ersten Anforderung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="dab05-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="dab05-154">Angepasste Autorisierung</span><span class="sxs-lookup"><span data-stu-id="dab05-154">Customized authorization</span></span>

<span data-ttu-id="dab05-155">Wenn Sie die Art der Autorisierung anpassen müssen, können Sie eine Klasse erstellen, die von `AuthorizeAttribute` abgeleitet ist, und die [userauthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) -Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="dab05-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="dab05-156">Für jede Anforderung ruft signalr diese Methode auf, um zu bestimmen, ob der Benutzer autorisiert ist, die Anforderung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="dab05-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="dab05-157">In der überschriebenen Methode stellen Sie die erforderliche Logik für Ihr Autorisierungs Szenario bereit.</span><span class="sxs-lookup"><span data-stu-id="dab05-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="dab05-158">Im folgenden Beispiel wird gezeigt, wie Sie die Autorisierung über eine Anspruchs basierte Identität erzwingen.</span><span class="sxs-lookup"><span data-stu-id="dab05-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="dab05-159">Übergeben von Authentifizierungsinformationen an Clients</span><span class="sxs-lookup"><span data-stu-id="dab05-159">Pass authentication information to clients</span></span>

<span data-ttu-id="dab05-160">Möglicherweise müssen Sie Authentifizierungsinformationen in dem Code verwenden, der auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="dab05-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="dab05-161">Sie übergeben die erforderlichen Informationen, wenn Sie die Methoden auf dem Client aufrufen.</span><span class="sxs-lookup"><span data-stu-id="dab05-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="dab05-162">Eine Chat-Anwendungsmethode könnte z. b. den Benutzernamen der Person, die eine Meldung bereitstellt, als Parameter übergeben, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="dab05-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="dab05-163">Oder Sie können ein Objekt erstellen, um die Authentifizierungsinformationen darzustellen, und dieses Objekt als Parameter übergeben, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="dab05-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="dab05-164">Sie sollten die Verbindungs-ID eines Clients niemals an andere Clients übergeben, da Sie von einem böswilligen Benutzer verwendet werden kann, um eine Anforderung von diesem Client zu imitieren.</span><span class="sxs-lookup"><span data-stu-id="dab05-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="dab05-165">Authentifizierungs Optionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="dab05-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="dab05-166">Wenn Sie über einen .NET-Client verfügen, z. b. eine Konsolen-APP, die mit einem Hub interagiert, der auf authentifizierte Benutzer beschränkt ist, können Sie die Authentifizierungs Anmelde Informationen in einem Cookie, dem Verbindungs Header oder einem Zertifikat übergeben.</span><span class="sxs-lookup"><span data-stu-id="dab05-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="dab05-167">In den Beispielen in diesem Abschnitt wird gezeigt, wie diese verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="dab05-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="dab05-168">Dabei handelt es sich nicht um voll funktionsfähige signalr-apps.</span><span class="sxs-lookup"><span data-stu-id="dab05-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="dab05-169">Weitere Informationen zu .NET-Clients mit signalr finden Sie im Leitfaden für die [Hubs-API-.NET-Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="dab05-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="dab05-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="dab05-170">Cookie</span></span>

<span data-ttu-id="dab05-171">Wenn der .NET-Client mit einem Hub interagiert, der die ASP.NET-Formular Authentifizierung verwendet, müssen Sie das Authentifizierungs Cookie manuell auf der Verbindung festlegen.</span><span class="sxs-lookup"><span data-stu-id="dab05-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="dab05-172">Das Cookie wird der `CookieContainer`-Eigenschaft des [hubconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) -Objekts hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="dab05-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="dab05-173">Das folgende Beispiel zeigt eine Konsolen-APP, die ein Authentifizierungs Cookie von einer Webseite abruft und dieses Cookie der Verbindung hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="dab05-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="dab05-174">Die Konsolen-App stellt die Anmelde Informationen an <strong>www.contoso.com/RemoteLogin</strong> , die auf eine leere Seite verweisen können, die die folgende Code-Behind-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="dab05-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="dab05-175">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="dab05-175">Windows authentication</span></span>

<span data-ttu-id="dab05-176">Wenn Sie die Windows-Authentifizierung verwenden, können Sie die Anmelde Informationen des aktuellen Benutzers mithilfe der [default-Anmelde](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Informationen-Eigenschaft übergeben.</span><span class="sxs-lookup"><span data-stu-id="dab05-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="dab05-177">Sie legen die Anmelde Informationen für die Verbindung mit dem Wert von Default-Anmelde Informationen fest.</span><span class="sxs-lookup"><span data-stu-id="dab05-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="dab05-178">Verbindungs Header</span><span class="sxs-lookup"><span data-stu-id="dab05-178">Connection header</span></span>

<span data-ttu-id="dab05-179">Wenn Ihre Anwendung keine Cookies verwendet, können Sie Benutzerinformationen im Verbindungs Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="dab05-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="dab05-180">Beispielsweise können Sie ein Token im Verbindungs Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="dab05-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="dab05-181">Dann überprüfen Sie im Hub das Token des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="dab05-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="dab05-182">Zertifikat</span><span class="sxs-lookup"><span data-stu-id="dab05-182">Certificate</span></span>

<span data-ttu-id="dab05-183">Sie können ein Client Zertifikat übergeben, um den Benutzer zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="dab05-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="dab05-184">Sie fügen das Zertifikat hinzu, wenn Sie die Verbindung erstellen.</span><span class="sxs-lookup"><span data-stu-id="dab05-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="dab05-185">Das folgende Beispiel zeigt nur, wie Sie der Verbindung ein Client Zertifikat hinzufügen. die vollständige Konsolen-APP wird nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dab05-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="dab05-186">Dabei wird die [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) -Klasse verwendet, die verschiedene Methoden zum Erstellen des Zertifikats bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="dab05-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
