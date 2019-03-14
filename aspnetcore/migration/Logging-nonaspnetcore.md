---
title: Migrieren von "Microsoft.Extensions.Logging" 2.1 auf 2.2 oder 3.0
author: pakrym
description: Informationen Sie zum Migrieren einer ohne ASP.NET Core-Anwendung, die "Microsoft.Extensions.Logging" 2.1 auf 2.2 oder 3.0 verwendet.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063177"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="0e1e0-103">Migrieren von "Microsoft.Extensions.Logging" 2.1 auf 2.2 oder 3.0</span><span class="sxs-lookup"><span data-stu-id="0e1e0-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="0e1e0-104">Dieser Artikel beschreibt die allgemeinen Schritte zum Migrieren einer ohne ASP.NET Core-Anwendung, verwendet `Microsoft.Extensions.Logging` 2.1 auf 2.2 oder 3.0.</span><span class="sxs-lookup"><span data-stu-id="0e1e0-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="0e1e0-105">2.1 zu 2.2</span><span class="sxs-lookup"><span data-stu-id="0e1e0-105">2.1 to 2.2</span></span>

<span data-ttu-id="0e1e0-106">Erstellen Sie manuell `ServiceCollection` , und rufen Sie `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="0e1e0-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="0e1e0-107">2.1-Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0e1e0-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="0e1e0-108">2.2-Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0e1e0-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="0e1e0-109">2.1, 3.0</span><span class="sxs-lookup"><span data-stu-id="0e1e0-109">2.1 to 3.0</span></span>

<span data-ttu-id="0e1e0-110">In 3.0 verwenden `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="0e1e0-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="0e1e0-111">2.1-Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0e1e0-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="0e1e0-112">3.0-Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0e1e0-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="0e1e0-113">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0e1e0-113">Additional resources</span></span>

<xref:fundamentals/logging/index>