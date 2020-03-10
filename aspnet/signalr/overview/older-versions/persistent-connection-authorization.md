---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentifizierung und Autorisierung für permanente signalr-Verbindungen (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen. Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431187"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="dbc2c-104">Authentifizierung und Autorisierung für permanente SignalR-Verbindungen (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="dbc2c-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="dbc2c-105">von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dbc2c-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="dbc2c-106">In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="dbc2c-107">Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung finden [Sie unter Einführung in die Sicherheit](index.md).</span><span class="sxs-lookup"><span data-stu-id="dbc2c-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="dbc2c-108">Autorisierung erzwingen</span><span class="sxs-lookup"><span data-stu-id="dbc2c-108">Enforce authorization</span></span>

<span data-ttu-id="dbc2c-109">Zum Erzwingen von Autorisierungs Regeln bei Verwendung von [persistentconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie die `AuthorizeRequest`-Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="dbc2c-110">Das `Authorize`-Attribut kann nicht mit permanenten Verbindungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="dbc2c-111">Die `AuthorizeRequest`-Methode wird vor jeder Anforderung vom signalr-Framework aufgerufen, um zu überprüfen, ob der Benutzer berechtigt ist, die angeforderte Aktion auszuführen.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="dbc2c-112">Die `AuthorizeRequest`-Methode wird nicht vom Client aufgerufen. Stattdessen authentifizieren Sie den Benutzer über den Standard Authentifizierungsmechanismus Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="dbc2c-113">Im folgenden Beispiel wird gezeigt, wie Anforderungen auf authentifizierte Benutzer beschränkt werden.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="dbc2c-114">Sie können eine beliebige angepasste Autorisierungs Logik in der Methode "autorizerequest" hinzufügen. Beispielsweise wird überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.</span><span class="sxs-lookup"><span data-stu-id="dbc2c-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
