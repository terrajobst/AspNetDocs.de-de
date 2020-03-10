---
uid: signalr/overview/older-versions/hub-authorization
title: Authentifizierung und Autorisierung für signalr-Hubs (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450039"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="45a74-103">Authentifizierung und Autorisierung für SignalR-Hubs (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="45a74-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>

<span data-ttu-id="45a74-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="45a74-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="45a74-105">In diesem Thema wird beschrieben, wie Sie einschränken, welche Benutzer oder Rollen auf hubmethoden zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="45a74-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>

## <a name="overview"></a><span data-ttu-id="45a74-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="45a74-106">Overview</span></span>

<span data-ttu-id="45a74-107">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="45a74-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="45a74-108">Attribut autorisieren</span><span class="sxs-lookup"><span data-stu-id="45a74-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="45a74-109">Authentifizierung für alle Hubs erforderlich</span><span class="sxs-lookup"><span data-stu-id="45a74-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="45a74-110">Angepasste Autorisierung</span><span class="sxs-lookup"><span data-stu-id="45a74-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="45a74-111">Übergeben von Authentifizierungsinformationen an Clients</span><span class="sxs-lookup"><span data-stu-id="45a74-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="45a74-112">Authentifizierungs Optionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="45a74-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="45a74-113">Cookie mit Formular Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="45a74-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="45a74-114">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="45a74-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="45a74-115">Verbindungs Header</span><span class="sxs-lookup"><span data-stu-id="45a74-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="45a74-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="45a74-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="45a74-117">Attribut autorisieren</span><span class="sxs-lookup"><span data-stu-id="45a74-117">Authorize attribute</span></span>

<span data-ttu-id="45a74-118">Signalr stellt das [Autorisierungs](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut bereit, um anzugeben, welche Benutzer oder Rollen Zugriff auf einen Hub oder eine Methode haben.</span><span class="sxs-lookup"><span data-stu-id="45a74-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="45a74-119">Dieses Attribut befindet sich im `Microsoft.AspNet.SignalR`-Namespace.</span><span class="sxs-lookup"><span data-stu-id="45a74-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="45a74-120">Sie wenden das `Authorize`-Attribut entweder auf einen Hub oder bestimmte Methoden in einem Hub an.</span><span class="sxs-lookup"><span data-stu-id="45a74-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="45a74-121">Wenn Sie das `Authorize`-Attribut auf eine hubklasse anwenden, wird die angegebene Autorisierungs Anforderung auf alle Methoden im Hub angewendet.</span><span class="sxs-lookup"><span data-stu-id="45a74-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="45a74-122">Die verschiedenen Typen von Autorisierungs Anforderungen, die Sie anwenden können, sind unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="45a74-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="45a74-123">Ohne das `Authorize`-Attribut sind alle öffentlichen Methoden auf dem Hub für einen Client verfügbar, der mit dem Hub verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="45a74-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="45a74-124">Wenn Sie in Ihrer Webanwendung eine Rolle mit dem Namen "admin" definiert haben, können Sie angeben, dass nur Benutzer in dieser Rolle mit folgendem Code auf einen Hub zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="45a74-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="45a74-125">Oder Sie können angeben, dass ein Hub eine Methode enthält, die für alle Benutzer verfügbar ist, und eine zweite Methode, die nur authentifizierten Benutzern zur Verfügung steht, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="45a74-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="45a74-126">In den folgenden Beispielen werden verschiedene Autorisierungs Szenarien behandelt:</span><span class="sxs-lookup"><span data-stu-id="45a74-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="45a74-127">`[Authorize]` – nur authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="45a74-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="45a74-128">`[Authorize(Roles = "Admin,Manager")]` – nur authentifizierte Benutzer in den angegebenen Rollen</span><span class="sxs-lookup"><span data-stu-id="45a74-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="45a74-129">`[Authorize(Users = "user1,user2")]` – nur authentifizierte Benutzer mit den angegebenen Benutzernamen</span><span class="sxs-lookup"><span data-stu-id="45a74-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="45a74-130">`[Authorize(RequireOutgoing=false)]` – nur authentifizierte Benutzer können den Hub aufrufen, aber Aufrufe vom Server zurück an Clients werden nicht durch Autorisierung eingeschränkt, wie z. b., wenn nur bestimmte Benutzer eine Nachricht senden können, aber alle anderen Personen die Nachricht empfangen können.</span><span class="sxs-lookup"><span data-stu-id="45a74-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="45a74-131">Die Eigenschaft "Requirements Outgoing" kann nur auf den gesamten Hub angewendet werden, nicht auf die Einzelpersonen Methoden innerhalb des Hubs.</span><span class="sxs-lookup"><span data-stu-id="45a74-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="45a74-132">Wenn "Requirements Outgoing" nicht auf "false" festgelegt ist, werden nur Benutzer aufgerufen, die die Autorisierungs Anforderung erfüllen.</span><span class="sxs-lookup"><span data-stu-id="45a74-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="45a74-133">Authentifizierung für alle Hubs erforderlich</span><span class="sxs-lookup"><span data-stu-id="45a74-133">Require authentication for all hubs</span></span>

<span data-ttu-id="45a74-134">Sie können eine Authentifizierung für alle Hubs und hubmethoden in Ihrer Anwendung anfordern, indem Sie die Methode "Requirements [Authentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) " aufrufen, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="45a74-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="45a74-135">Sie können diese Methode verwenden, wenn Sie über mehrere Hubs verfügen und eine Authentifizierungsanforderung für alle erzwingen möchten.</span><span class="sxs-lookup"><span data-stu-id="45a74-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="45a74-136">Mit dieser Methode können Sie keine Rollen-, Benutzer-oder ausgehende Autorisierung angeben.</span><span class="sxs-lookup"><span data-stu-id="45a74-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="45a74-137">Sie können nur angeben, dass der Zugriff auf die hubmethoden auf authentifizierte Benutzer beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="45a74-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="45a74-138">Allerdings können Sie das Autorisierungs Attribut weiterhin auf Hubs oder Methoden anwenden, um zusätzliche Anforderungen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="45a74-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="45a74-139">Alle Anforderungen, die Sie in Attributen angeben, werden zusätzlich zu den grundlegenden Authentifizierungsanforderungen angewendet.</span><span class="sxs-lookup"><span data-stu-id="45a74-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="45a74-140">Das folgende Beispiel zeigt eine Global. asax-Datei, die alle hubmethoden auf authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="45a74-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="45a74-141">Wenn Sie die `RequireAuthentication()`-Methode nach der Verarbeitung einer signalr-Anforderung aufzurufen, löst signalr eine `InvalidOperationException`-Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="45a74-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="45a74-142">Diese Ausnahme wird ausgelöst, weil Sie der hubpipeline kein Modul hinzufügen können, nachdem die Pipeline aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="45a74-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="45a74-143">Das vorherige Beispiel zeigt, wie Sie die `RequireAuthentication`-Methode in der `Application_Start`-Methode aufrufen, die einmal vor der Verarbeitung der ersten Anforderung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="45a74-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="45a74-144">Angepasste Autorisierung</span><span class="sxs-lookup"><span data-stu-id="45a74-144">Customized authorization</span></span>

<span data-ttu-id="45a74-145">Wenn Sie die Art der Autorisierung anpassen müssen, können Sie eine Klasse erstellen, die von `AuthorizeAttribute` abgeleitet ist, und die [userauthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) -Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="45a74-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="45a74-146">Diese Methode wird für jede Anforderung aufgerufen, um zu bestimmen, ob der Benutzer autorisiert ist, die Anforderung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="45a74-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="45a74-147">In der überschriebenen Methode stellen Sie die erforderliche Logik für Ihr Autorisierungs Szenario bereit.</span><span class="sxs-lookup"><span data-stu-id="45a74-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="45a74-148">Im folgenden Beispiel wird gezeigt, wie Sie die Autorisierung über eine Anspruchs basierte Identität erzwingen.</span><span class="sxs-lookup"><span data-stu-id="45a74-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="45a74-149">Übergeben von Authentifizierungsinformationen an Clients</span><span class="sxs-lookup"><span data-stu-id="45a74-149">Pass authentication information to clients</span></span>

<span data-ttu-id="45a74-150">Möglicherweise müssen Sie Authentifizierungsinformationen in dem Code verwenden, der auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="45a74-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="45a74-151">Sie übergeben die erforderlichen Informationen, wenn Sie die Methoden auf dem Client aufrufen.</span><span class="sxs-lookup"><span data-stu-id="45a74-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="45a74-152">Eine Chat-Anwendungsmethode könnte z. b. den Benutzernamen der Person, die eine Meldung bereitstellt, als Parameter übergeben, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="45a74-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="45a74-153">Oder Sie können ein Objekt erstellen, um die Authentifizierungsinformationen darzustellen, und dieses Objekt als Parameter übergeben, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="45a74-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="45a74-154">Sie sollten die Verbindungs-ID eines Clients niemals an andere Clients übergeben, da Sie von einem böswilligen Benutzer verwendet werden kann, um eine Anforderung von diesem Client zu imitieren.</span><span class="sxs-lookup"><span data-stu-id="45a74-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="45a74-155">Authentifizierungs Optionen für .NET-Clients</span><span class="sxs-lookup"><span data-stu-id="45a74-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="45a74-156">Wenn Sie über einen .NET-Client verfügen, z. b. eine Konsolen-APP, die mit einem Hub interagiert, der auf authentifizierte Benutzer beschränkt ist, können Sie die Authentifizierungs Anmelde Informationen in einem Cookie, dem Verbindungs Header oder einem Zertifikat übergeben.</span><span class="sxs-lookup"><span data-stu-id="45a74-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="45a74-157">In den Beispielen in diesem Abschnitt wird gezeigt, wie diese verschiedenen Methoden zum Authentifizieren eines Benutzers verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="45a74-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="45a74-158">Dabei handelt es sich nicht um voll funktionsfähige signalr-apps.</span><span class="sxs-lookup"><span data-stu-id="45a74-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="45a74-159">Weitere Informationen zu .NET-Clients mit signalr finden Sie im Leitfaden für die [Hubs-API-.NET-Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="45a74-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="45a74-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="45a74-160">Cookie</span></span>

<span data-ttu-id="45a74-161">Wenn der .NET-Client mit einem Hub interagiert, der die ASP.NET-Formular Authentifizierung verwendet, müssen Sie das Authentifizierungs Cookie manuell auf der Verbindung festlegen.</span><span class="sxs-lookup"><span data-stu-id="45a74-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="45a74-162">Das Cookie wird der `CookieContainer`-Eigenschaft des [hubconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) -Objekts hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="45a74-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="45a74-163">Das folgende Beispiel zeigt eine Konsolen-APP, die ein Authentifizierungs Cookie von einer Webseite abruft und dieses Cookie der Verbindung hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="45a74-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="45a74-164">Die im Beispiel `https://www.contoso.com/RemoteLogin` URL verweist auf eine Webseite, die Sie erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="45a74-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="45a74-165">Auf der Seite werden der veröffentlichte Benutzername und das Kennwort abgerufen, und es wird versucht, den Benutzer mit den Anmelde Informationen anzumelden.</span><span class="sxs-lookup"><span data-stu-id="45a74-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="45a74-166">Die Konsolen-App stellt die Anmelde Informationen an www.contoso.com/RemoteLogin, die auf eine leere Seite verweisen können, die die folgende Code-Behind-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="45a74-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="45a74-167">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="45a74-167">Windows authentication</span></span>

<span data-ttu-id="45a74-168">Wenn Sie die Windows-Authentifizierung verwenden, können Sie die Anmelde Informationen des aktuellen Benutzers mithilfe der [default-Anmelde](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) Informationen-Eigenschaft übergeben.</span><span class="sxs-lookup"><span data-stu-id="45a74-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="45a74-169">Sie legen die Anmelde Informationen für die Verbindung mit dem Wert von Default-Anmelde Informationen fest.</span><span class="sxs-lookup"><span data-stu-id="45a74-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="45a74-170">Verbindungs Header</span><span class="sxs-lookup"><span data-stu-id="45a74-170">Connection header</span></span>

<span data-ttu-id="45a74-171">Wenn Ihre Anwendung keine Cookies verwendet, können Sie Benutzerinformationen im Verbindungs Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="45a74-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="45a74-172">Beispielsweise können Sie ein Token im Verbindungs Header übergeben.</span><span class="sxs-lookup"><span data-stu-id="45a74-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="45a74-173">Dann überprüfen Sie im Hub das Token des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="45a74-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="45a74-174">Zertifikat</span><span class="sxs-lookup"><span data-stu-id="45a74-174">Certificate</span></span>

<span data-ttu-id="45a74-175">Sie können ein Client Zertifikat übergeben, um den Benutzer zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="45a74-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="45a74-176">Sie fügen das Zertifikat hinzu, wenn Sie die Verbindung erstellen.</span><span class="sxs-lookup"><span data-stu-id="45a74-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="45a74-177">Das folgende Beispiel zeigt nur, wie Sie der Verbindung ein Client Zertifikat hinzufügen. die vollständige Konsolen-APP wird nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="45a74-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="45a74-178">Dabei wird die [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) -Klasse verwendet, die verschiedene Methoden zum Erstellen des Zertifikats bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="45a74-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
