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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472347"
---
# <a name="katana-samples"></a>Katana-Beispiele

von [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana-Beispiele

**ASP.net Routes-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
In einigen Anwendungen möchten Sie owin-Komponenten in der ASP.NET-Routing Tabelle nebeneinander mit nicht-owin-Komponenten verbinden. In diesem Beispiel wird gezeigt, wie die RouteCollection-Erweiterungs Methoden mapowinpath und mapowinroute verwendet werden, die von Microsoft. owin. Host. systemWeb bereitgestellt werden.

**Beispiel für Verzweigungs Pipelines** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Owin-Anforderungs Verarbeitungs Pipelines müssen nicht linear sein, Sie können so verzweigt werden, dass Anforderungen auf unterschiedliche Weise verarbeitet werden. In diesem Beispiel wird gezeigt, wie eine Verzweigungs Pipeline basierend auf Anforderungs Pfaden oder anderen Anforderungs Daten, z. b. Headern, erstellt wird. Diese Komponenten sind im nuget-Paket Microsoft. owin. Mapping verfügbar.

**Beispiel für benutzerdefinierte Server** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Zeigt, wie ein benutzerdefinierter owin-Server verwendet wird, wenn das selbst Hosting von owin verwendet wird.

**Eingebetteter Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Einige owin-Server können in Ihrem eigenen Prozess ausgeführt werden (&quot;selbstgeh ostete&quot;). In diesem Beispiel wird gezeigt, wie eine owin-Anwendung mit den Tools gestartet wird, die vom Microsoft. owin. Hosting-nuget-Paket bereitgestellt werden.

**HelloWorld-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
Owin ist eine HTTP-Server-API-Abstraktion, die die Portabilität von Anwendungen auf verschiedenen Servern ermöglicht. In diesem Beispiel wird veranschaulicht, wie Sie eine Hallo Welt Anwendung mit einigen **einfachen Wrappern** für die unformatierte owin-Abstraktion schreiben und Sie auf einem Webserver wie ASP.net ausführen.

Hallo Welt unformatierte **owin-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
In diesem Beispiel wird veranschaulicht, wie Sie eine Hallo Welt Anwendung **mit der** unformatierten owin-Abstraktion schreiben und Sie auf einem Webserver wie ASP.net ausführen.

**Signalr-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Zeigt, wie Sie signalr mithilfe von owin/Katana selbst hosten können. Weitere Informationen zum Selbsthosting von signalr finden Sie unter [Tutorial: signalr Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Beispiel für statische Dateien** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Zeigt, wie HTTP-Anforderungen für statische Dateien mit owin/Katana unterstützt werden.

**Web-API** - | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
Dieses Beispiel zeigt, wie Sie owin in IIS hosten und der owin-Pipeline eine Web-API hinzufügen.

**WebSocket-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Zeigt, wie websockets in owin mithilfe der [System .net. websockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) -Klasse unterstützt werden.
