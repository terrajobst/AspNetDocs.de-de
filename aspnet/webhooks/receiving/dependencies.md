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
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.net webhooks-Empfänger Abhängigkeiten

Microsoft ASP.net webhooks ist im Hinblick auf die Abhängigkeitsinjektion konzipiert. Die meisten Abhängigkeiten im System können mithilfe einer Abhängigkeitsinjektion-Engine durch alternative Implementierungen ersetzt werden.

Eine Liste der Empfänger Abhängigkeiten finden Sie unter [dependencyscopeextensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) . Wenn keine Abhängigkeit registriert wurde, wird eine Standard Implementierung verwendet. Eine Liste der Standard Implementierungen finden Sie unter [receiverservices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) .
