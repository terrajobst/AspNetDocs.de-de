---
uid: webhooks/source
title: ASP.NET WebHooks Quellcode und NuGet-Paketen | Microsoft-Dokumentation
author: rick-anderson
description: Links zu ASP.NET WebHooks Quellcode und NuGet-Pakete
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57027187"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET WebHooks Quellcode und NuGet-Pakete

Microsoft ASP.NET WebHooks ist Teil der Microsoft ASP.NET-Familie von Modulen und wird als gehosteten ein [Open Source-Projekt auf GitHub](https://github.com/aspnet/WebHooks). Dies bedeutet, dass wir Beiträge akzeptieren, aber betrachten Sie die [Beitragsrichtlinien](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) vor dem Senden eines Pull Request.

Dieser Onlinedokumentation, die Sie lesen, wird jetzt auch als gehostet [Open Source auf GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) und akzeptiert auch Beiträge.

## <a name="nuget-packages"></a>NuGet-Pakete

Die [NuGet-Pakete](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sind in drei Teile unterteilt:

* [Allgemeine](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Ein common-Paket, das Absender und Empfänger gemeinsam verwendet wird.

* [Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Eine Reihe von Paketen unterstützen, senden Ihre eigenen WebHooks an andere Benutzer. Die Funktionalität zum Senden von WebHooks wird ausführlicher beschrieben [WebHooks senden](sending/index.md).

* [Empfänger](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Eine Reihe von Paketen, die Unterstützung von WebHooks von anderen empfangen. Die Funktionalität für den Empfang von WebHooks wird ausführlicher beschrieben [empfangen WebHooks](receiving/index.md).
