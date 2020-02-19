---
uid: webhooks/diagnostics/logging
title: ASP.net webhooks-Protokollierung | Microsoft-Dokumentation
author: rick-anderson
description: 'Gewusst wie: Protokollieren in ASP.net webhooks'
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: a05b32c4a8f9577bcf6170bd19a9e440b1aeb75b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457582"
---
# <a name="aspnet-webhooks-logging"></a>ASP.net webhooks-Protokollierung

Microsoft ASP.net webhooks verwendet die Protokollierung, um Probleme und Probleme zu melden. Standardmäßig werden Protokolle mithilfe von [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) geschrieben, wo Sie mithilfe von Ablaufverfolgungslistenern [wie jeder](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) andere Protokolldaten Strom geändert werden können.

Wenn Sie Ihre Webanwendung als Azure-Web-App bereitstellen, werden die Protokolle automatisch übernommen und können mit jeder anderen [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) -Protokollierung verwaltet werden. Weitere Informationen finden Sie [unter Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Außerdem können Protokolle direkt aus Visual Studio abgerufen werden, wie unter Problembehandlung für [eine Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)beschrieben.

## <a name="redirecting-logs"></a>Umleiten von Protokollen

Anstatt Protokolle in [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)zu schreiben, ist es möglich, eine Alternative Protokollierungs Implementierung bereitzustellen, die sich direkt bei einem Protokoll-Manager wie [log4net](http://logging.apache.org/log4net/) und [nlog](http://nlog-project.org/)anmelden kann. Stellen Sie einfach eine Implementierung von [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) bereit, und registrieren Sie Sie mit einem Abhängigkeits Injection-Modul Ihrer Wahl, und es wird von Microsoft ASP.net webhooks übernommen. Weitere Informationen finden Sie [unter Abhängigkeitsinjektion in ASP.net-Web-API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) .
