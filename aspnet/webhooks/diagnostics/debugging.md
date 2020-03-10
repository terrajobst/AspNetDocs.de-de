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
# <a name="aspnet-webhooks-debugging"></a>ASP.net webhooks Debuggen  

## <a name="debugging-in-azure"></a>Debuggen in Azure

Informationen zum Debuggen Ihrer Webanwendung bei der Ausführung in Azure finden Sie im Tutorial Problembehandlung für [eine Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debuggen mit Quelle und Symbolen

Neben dem Debuggen von eigenem Code ist es auch möglich, direkt in Microsoft ASP.net webhooks zu debuggen, und tatsächlich alle .net. Dies funktioniert unabhängig davon, ob Sie lokal oder Remote Debuggen. Konfigurieren Sie zunächst Visual Studio, um die Quelle und die Symbole zu suchen, indem Sie zu **Debuggen** und dann **Optionen und Einstellungen**navigieren. Legen Sie die Optionen wie folgt fest:

![Optionen und Einstellungen](_static/SourceSymbols.png)

Fügen Sie dann einen Link zu [symbolsource.org](http://symbolsource.org) hinzu, um die Quelle und die Symbole herunterzuladen. Wechseln Sie zur Registerkarte **Symbole** im obigen Menü, und fügen Sie Folgendes als Symbol Speicherort hinzu:

```
http://srv.symbolsource.org/pdb/Public
```

Stellen Sie außerdem sicher, dass das Cache Verzeichnis einen Kurznamen hat. Andernfalls können die Dateinamen zu lang werden, was dazu führt, dass die Symbole nicht geladen werden. Ein Beispiel Pfad ist:

```
C:\SymCache
```

Die Einstellungen sollten in etwa wie folgt aussehen:

![Beispiel für Optionen für Symbol Datei Speicherort](_static/SymSource.png)
