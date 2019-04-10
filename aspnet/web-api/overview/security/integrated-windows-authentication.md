---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrierte Windows-Authentifizierung | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der integrierten Windows-Authentifizierung in ASP.NET Web-API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce845eb6c914321736d77e989f10344eb7596eba
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59416830"
---
# <a name="integrated-windows-authentication"></a><span data-ttu-id="50196-103">Integrierte Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="50196-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="50196-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="50196-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="50196-105">Integrierte Windows-Authentifizierung kann Benutzer melden Sie sich mit ihren Windows-Anmeldeinformationen, die mit Kerberos oder NTLM.</span><span class="sxs-lookup"><span data-stu-id="50196-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="50196-106">Der Client sendet die Anmeldeinformationen in der Authorization-Header.</span><span class="sxs-lookup"><span data-stu-id="50196-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="50196-107">Windows-Authentifizierung ist für Intranetumgebungen am besten geeignet.</span><span class="sxs-lookup"><span data-stu-id="50196-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="50196-108">Weitere Informationen finden Sie unter [Windows-Authentifizierung](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="50196-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="50196-109">Vorteile</span><span class="sxs-lookup"><span data-stu-id="50196-109">Advantages</span></span> | <span data-ttu-id="50196-110">Nachteile</span><span class="sxs-lookup"><span data-stu-id="50196-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="50196-111">– In IIS erstellt.</span><span class="sxs-lookup"><span data-stu-id="50196-111">- Built into IIS.</span></span> <span data-ttu-id="50196-112">– Die Anmeldeinformationen des Benutzers ist nicht in der Anforderung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="50196-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="50196-113">– Wenn der Client-Computer mit der Domäne (z. B. Intranetanwendung) gehört, muss der Benutzer nicht, Anmeldeinformationen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="50196-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="50196-114">-Nicht für die Internet-Anwendungen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="50196-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="50196-115">– Unterstützung für Kerberos oder NTLM in der Client erfordert.</span><span class="sxs-lookup"><span data-stu-id="50196-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="50196-116">-Client muss in Active Directory-Domäne sein.</span><span class="sxs-lookup"><span data-stu-id="50196-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="50196-117">Wenn Ihre Anwendung in Azure gehostet wird, und einer lokalen Active Directory-Domäne haben, sollten Sie Ihre lokalen AD mit Azure Active Directory in einem Verbund zusammenfassen.</span><span class="sxs-lookup"><span data-stu-id="50196-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="50196-118">Auf diese Weise können Benutzer sich mit ihren lokalen Anmeldeinformationen anmelden, aber die Authentifizierung von Azure AD erfolgt.</span><span class="sxs-lookup"><span data-stu-id="50196-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="50196-119">Weitere Informationen finden Sie unter [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="50196-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="50196-120">Zum Erstellen einer Anwendung, die integrierte Windows-Authentifizierung verwendet wird, wählen Sie die Vorlage "Intranet-Anwendung" im Assistenten für MVC 4.</span><span class="sxs-lookup"><span data-stu-id="50196-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="50196-121">Diese Projektvorlage fügt die folgende Einstellung in der Datei "Web.config":</span><span class="sxs-lookup"><span data-stu-id="50196-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="50196-122">Auf dem Client, der integrierten Windows-Authentifizierung funktioniert mit jedem Browser, die unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Authentifizierungsschema, das meisten gängigen Browser enthält.</span><span class="sxs-lookup"><span data-stu-id="50196-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="50196-123">Für .NET-Clientanwendungen die **"HttpClient"** Klasse unterstützt die Windows-Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="50196-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="50196-124">Windows-Authentifizierung ist anfällig für websiteübergreifende anforderungsfälschungen (CSRF).</span><span class="sxs-lookup"><span data-stu-id="50196-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="50196-125">Finden Sie unter [verhindern von Angriffen Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="50196-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
