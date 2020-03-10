---
uid: webhooks/receiving/dependencies
title: ASP.net webhooks-Empfänger Abhängigkeiten | Microsoft-Dokumentation
author: rick-anderson
description: Empfänger Abhängigkeiten und Abhängigkeitsinjektion in ASP.net-webhooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 477b8828209d0da1d485ef883b0f99b4e1b9b5bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517527"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="5b5b4-103">ASP.net webhooks-Empfänger Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="5b5b4-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="5b5b4-104">Microsoft ASP.net webhooks ist im Hinblick auf die Abhängigkeitsinjektion konzipiert.</span><span class="sxs-lookup"><span data-stu-id="5b5b4-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="5b5b4-105">Die meisten Abhängigkeiten im System können mithilfe einer Abhängigkeitsinjektion-Engine durch alternative Implementierungen ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="5b5b4-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="5b5b4-106">Eine Liste der Empfänger Abhängigkeiten finden Sie unter [dependencyscopeextensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) .</span><span class="sxs-lookup"><span data-stu-id="5b5b4-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="5b5b4-107">Wenn keine Abhängigkeit registriert wurde, wird eine Standard Implementierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="5b5b4-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="5b5b4-108">Eine Liste der Standard Implementierungen finden Sie unter [receiverservices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) .</span><span class="sxs-lookup"><span data-stu-id="5b5b4-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
