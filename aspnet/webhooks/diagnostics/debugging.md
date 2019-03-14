---
uid: webhooks/diagnostics/debugging
title: Debuggen von ASP.NET-WebHooks | Microsoft-Dokumentation
author: rick-anderson
description: Wie Sie ASP.NET WebHooks zu debuggen.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062497"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="2b9e9-103">Debuggen von ASP.NET-WebHooks</span><span class="sxs-lookup"><span data-stu-id="2b9e9-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="2b9e9-104">Debuggen in Azure</span><span class="sxs-lookup"><span data-stu-id="2b9e9-104">Debugging in Azure</span></span>

<span data-ttu-id="2b9e9-105">Zum Debuggen der Webanwendung während der Ausführung in Azure finden Sie im Tutorial [Problembehandlung bei Web-Apps in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="2b9e9-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="2b9e9-106">Debuggen mit Symbolen und Quelle</span><span class="sxs-lookup"><span data-stu-id="2b9e9-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="2b9e9-107">Zusätzlich zu Ihren eigenen Code-Debuggen, ist es möglich, direkt in Microsoft ASP.NET WebHooks und in der Tat Debuggen aller .NET.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="2b9e9-108">Dies funktioniert, unabhängig davon, ob Sie lokal oder Remote zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="2b9e9-109">Konfigurieren Sie zunächst Visual Studio, um die Quelle und Symbole finden durch das Aufrufen **Debuggen** und dann **Optionen und Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="2b9e9-110">Legen Sie die Optionen wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2b9e9-110">Set the options like this:</span></span>

![Optionen und Einstellungen](_static/SourceSymbols.png)

<span data-ttu-id="2b9e9-112">Fügen Sie dann auf einen Link zu [symbolsource.org](http://symbolsource.org) für das Herunterladen der Quelle und Symbole.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="2b9e9-113">Wechseln Sie zu der **Symbole** im oberen Menü auf der Registerkarte, und fügen Sie Folgendes als Symbol Speicherort:</span><span class="sxs-lookup"><span data-stu-id="2b9e9-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="2b9e9-114">Stellen Sie außerdem sicher, dass das Cacheverzeichnis einen kurzen Namen verfügt; Andernfalls können die Namen der zu lang werden die bewirkt, das die Symbole nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="2b9e9-115">Ein Beispielpfad ist:</span><span class="sxs-lookup"><span data-stu-id="2b9e9-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="2b9e9-116">Die Einstellungen sollten wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="2b9e9-116">The settings should look similar to this:</span></span>

![Beispiel für Optionen Symbol Speicherort](_static/SymSource.png)
