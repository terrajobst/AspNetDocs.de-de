---
uid: signalr/overview/security/persistent-connection-authorization
title: Authentifizierung und Autorisierung für permanente signalr-Verbindungen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen. Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467475"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="0b001-104">Authentifizierung und Autorisierung für permanente SignalR-Verbindungen</span><span class="sxs-lookup"><span data-stu-id="0b001-104">Authentication and Authorization for SignalR Persistent Connections</span></span>

<span data-ttu-id="0b001-105">von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0b001-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="0b001-106">In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen.</span><span class="sxs-lookup"><span data-stu-id="0b001-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="0b001-107">Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung finden [Sie unter Einführung in die Sicherheit](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="0b001-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0b001-108">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="0b001-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="0b001-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0b001-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0b001-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0b001-110">.NET 4.5</span></span>
> - <span data-ttu-id="0b001-111">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="0b001-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0b001-112">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="0b001-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="0b001-113">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0b001-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="0b001-114">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="0b001-114">Questions and comments</span></span>
>
> <span data-ttu-id="0b001-115">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="0b001-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0b001-116">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="0b001-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="0b001-117">Autorisierung erzwingen</span><span class="sxs-lookup"><span data-stu-id="0b001-117">Enforce authorization</span></span>

<span data-ttu-id="0b001-118">Zum Erzwingen von Autorisierungs Regeln bei Verwendung von [persistentconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie die `AuthorizeRequest`-Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="0b001-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="0b001-119">Das `Authorize`-Attribut kann nicht mit permanenten Verbindungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0b001-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="0b001-120">Die `AuthorizeRequest`-Methode wird vor jeder Anforderung vom signalr-Framework aufgerufen, um zu überprüfen, ob der Benutzer berechtigt ist, die angeforderte Aktion auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0b001-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="0b001-121">Die `AuthorizeRequest`-Methode wird nicht vom Client aufgerufen. Stattdessen authentifizieren Sie den Benutzer über den Standard Authentifizierungsmechanismus Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0b001-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="0b001-122">Im folgenden Beispiel wird gezeigt, wie Anforderungen auf authentifizierte Benutzer beschränkt werden.</span><span class="sxs-lookup"><span data-stu-id="0b001-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="0b001-123">Sie können eine beliebige angepasste Autorisierungs Logik in der Methode "autorizerequest" hinzufügen. Beispielsweise wird überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.</span><span class="sxs-lookup"><span data-stu-id="0b001-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
