---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056487"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a><span data-ttu-id="42003-101">Beispiel für ASP.NET Core-Hintergrundtasks (Generischer Host)</span><span class="sxs-lookup"><span data-stu-id="42003-101">ASP.NET Core Background Tasks Sample (Generic Host)</span></span>

<span data-ttu-id="42003-102">Dieses Beispiel veranschaulicht die Verwendung von [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="42003-102">This sample illustrates the use of [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span></span> <span data-ttu-id="42003-103">Dieses Beispiel veranschaulicht die Features, die im Artikel [Background tasks with hosted services in ASP.NET Core (Hintergrundtasks bei gehosteten Diensten in ASP.NET Core)](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="42003-103">This sample demonstrates the features described in the [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="42003-104">Beim Ausführen des Beispiels in [Visual Studio Code](https://code.visualstudio.com/) legen Sie den Wert **console** der Konsolenkonfiguration in *.vscode/launch.json* entweder auf `externalTerminal` oder auf `integratedTerminal` fest.</span><span class="sxs-lookup"><span data-stu-id="42003-104">When running the sample in [Visual Studio Code](https://code.visualstudio.com/), set the **console** value of the console configuration in *.vscode/launch.json* to either `externalTerminal` or `integratedTerminal`.</span></span> <span data-ttu-id="42003-105">Die Verwendung der `internalConsole` ist nicht kompatibel mit der Konsolentastatureingabe, die von der App für das Einreihen von Hintergrundarbeitselementen in die Warteschlange verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="42003-105">Use of the `internalConsole` is incompatible with console keystroke input that the app uses to enqueue background work items.</span></span>
