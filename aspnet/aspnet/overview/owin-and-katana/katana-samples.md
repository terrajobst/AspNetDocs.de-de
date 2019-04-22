---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana-Beispiele | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59379070"
---
# <a name="katana-samples"></a>Katana-Beispiele

by [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana-Beispiele

**ASP.NET leitet Beispiel** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
In einigen Anwendungen möchten Sie die OWIN-Komponenten in der Routentabelle für Asp.Net parallel mit nicht-OWIN-Komponenten einbinden. Dieses Beispiel zeigt, wie Sie mit die RouteCollection-Erweiterungsmethoden MapOwinPath und MapOwinRoute von "Microsoft.owin.Host.systemweb" bereitgestellt wird.

**Beispiel-Pipelines Verzweigungen** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
OWIN-Anforderung Verarbeitungspipelines müssen nicht linear sein, diese zum Verarbeiten von Anforderungen auf unterschiedliche Weise gebrancht werden können. Dieses Beispiel zeigt, wie Sie eine verzweigte Pipeline basierend auf anforderungspfade oder andere Anforderungsdaten wie Header zu erstellen. Diese Komponenten sind im Microsoft.Owin.Mapping Nuget-Paket verfügbar.

**Benutzerdefinierte Server-Beispiel** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Zeigt, wie Sie einen benutzerdefinierten OWIN-Server verwenden, wenn Sie Selbsthosting OWIN.

**Eingebettete Beispiel** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Einige OWIN-Server in Ihrem eigenen Prozess ausgeführt werden können (&quot;selbstgehostet&quot;). Dieses Beispiel zeigt, wie Sie zum Starten einer OWIN-Anwendung, die mit den Tools, die von der Microsoft.Owin.Hosting-Nuget-Paket bereitgestellt wird.

**HelloWorld-Beispiels** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN ist ein HTTP-Server-API-Abstraktion, das auf verschiedenen Servern Anwendungsportabilität ermöglicht. Dieses Beispiel veranschaulicht das Schreiben eine Hello World-Anwendung mit einigen **einfache Wrapper** rund um den unformatierten OWIN-Abstraktion, und führen sie auf einem Webserver, z.B. ASP.NET.

**Hello World-Rohdaten OWIN-Beispiel** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
In diesem Beispiel wird veranschaulicht, wie zum Schreiben einer Hello World-Anwendung mithilfe der **unformatierten** OWIN-Abstraktion, und führen Sie es auf einem Webserver wie Asp.Net.

**SignalR-Beispiel** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Zeigt, wie selfhosten von SignalR mit OWIN / Katana. Weitere Informationen zum Self-hosting SignalR finden Sie unter [Lernprogramm: Selfhosten von SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Beispiel für statische Dateien** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Zeigt, wie zur Unterstützung von HTTP-Anforderungen für statische Dateien, die mithilfe von OWIN / Katana.

**Web-API** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
Dieses Beispiel zeigt, wie OWIN in IIS zu hosten und Web-API für die OWIN-Pipeline hinzufügen.

**Web-Socket-Beispiel** | [Quellcode:](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Zeigt, wie Web Sockets in OWIN mithilfe von unterstützen die [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) Klasse.
