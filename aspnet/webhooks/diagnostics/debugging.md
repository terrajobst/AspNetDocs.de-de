---
uid: webhooks/diagnostics/debugging
title: ASP.net webhooks-Debuggen | Microsoft-Dokumentation
author: rick-anderson
description: Debuggen von ASP.net-webhooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520599"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="7a0e2-103">ASP.net webhooks Debuggen</span><span class="sxs-lookup"><span data-stu-id="7a0e2-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="7a0e2-104">Debuggen in Azure</span><span class="sxs-lookup"><span data-stu-id="7a0e2-104">Debugging in Azure</span></span>

<span data-ttu-id="7a0e2-105">Informationen zum Debuggen Ihrer Webanwendung bei der Ausführung in Azure finden Sie im Tutorial Problembehandlung für [eine Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="7a0e2-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="7a0e2-106">Debuggen mit Quelle und Symbolen</span><span class="sxs-lookup"><span data-stu-id="7a0e2-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="7a0e2-107">Neben dem Debuggen von eigenem Code ist es auch möglich, direkt in Microsoft ASP.net webhooks zu debuggen, und tatsächlich alle .net.</span><span class="sxs-lookup"><span data-stu-id="7a0e2-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="7a0e2-108">Dies funktioniert unabhängig davon, ob Sie lokal oder Remote Debuggen.</span><span class="sxs-lookup"><span data-stu-id="7a0e2-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="7a0e2-109">Konfigurieren Sie zunächst Visual Studio, um die Quelle und die Symbole zu suchen, indem Sie zu **Debuggen** und dann **Optionen und Einstellungen**navigieren.</span><span class="sxs-lookup"><span data-stu-id="7a0e2-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="7a0e2-110">Legen Sie die Optionen wie folgt fest:</span><span class="sxs-lookup"><span data-stu-id="7a0e2-110">Set the options like this:</span></span>

![Optionen und Einstellungen](_static/SourceSymbols.png)

<span data-ttu-id="7a0e2-112">Fügen Sie dann einen Link zu [symbolsource.org](http://symbolsource.org) hinzu, um die Quelle und die Symbole herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="7a0e2-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="7a0e2-113">Wechseln Sie zur Registerkarte **Symbole** im obigen Menü, und fügen Sie Folgendes als Symbol Speicherort hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a0e2-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="7a0e2-114">Stellen Sie außerdem sicher, dass das Cache Verzeichnis einen Kurznamen hat. Andernfalls können die Dateinamen zu lang werden, was dazu führt, dass die Symbole nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="7a0e2-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="7a0e2-115">Ein Beispiel Pfad ist:</span><span class="sxs-lookup"><span data-stu-id="7a0e2-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="7a0e2-116">Die Einstellungen sollten in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="7a0e2-116">The settings should look similar to this:</span></span>

![Beispiel für Optionen für Symbol Datei Speicherort](_static/SymSource.png)
