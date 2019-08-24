---
uid: webhooks/source
title: ASP.net webhooks-Quellcode und nuget-Pakete | Microsoft-Dokumentation
author: rick-anderson
description: Links zu ASP.net webhooks-Quellcode und nuget-Paketen
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000701"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.net webhooks-Quellcode und nuget-Pakete

Microsoft ASP.net webhooks ist Teil der Microsoft ASP.NET Familie von Modulen und wird als [Open-Source-Projekt auf GitHub](https://github.com/aspnet/WebHooks)gehostet. Dies bedeutet, dass wir Beiträge akzeptieren, aber beachten Sie die [Beitrags Richtlinien](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) , bevor Sie eine Pull Request senden.

Diese Online Dokumentation, die Sie jetzt lesen, wird auch als [Open Source auf GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) gehostet und akzeptiert auch Beiträge.

## <a name="nuget-packages"></a>NuGet-Pakete

Die [nuget-Pakete](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sind in drei Teile unterteilt:

* [Allgemein](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Ein gemeinsames Paket, das von Absendern und Empfängern gemeinsam genutzt wird.

* [Absender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Ein Satz von Paketen, die das Senden Ihrer eigenen webhooks an andere unterstützen. Die Funktionalität zum Senden von webhooks wird unter [Senden von webhooks](sending/senders.md)ausführlicher beschrieben.

* [Empfänger](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Ein Satz von Paketen, der den Empfang von webhooks von anderen unterstützt. Die Funktionen zum Empfangen von webhooks werden im Abschnitt zum [empfangen von webhooks](receiving/index.md)ausführlicher beschrieben.
