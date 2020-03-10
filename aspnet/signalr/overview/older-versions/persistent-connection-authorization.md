---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentifizierung und Autorisierung für permanente signalr-Verbindungen (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen. Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431187"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Authentifizierung und Autorisierung für permanente SignalR-Verbindungen (SignalR 1.x)

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen. Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung finden [Sie unter Einführung in die Sicherheit](index.md).

## <a name="enforce-authorization"></a>Autorisierung erzwingen

Zum Erzwingen von Autorisierungs Regeln bei Verwendung von [persistentconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie die `AuthorizeRequest`-Methode überschreiben. Das `Authorize`-Attribut kann nicht mit permanenten Verbindungen verwendet werden. Die `AuthorizeRequest`-Methode wird vor jeder Anforderung vom signalr-Framework aufgerufen, um zu überprüfen, ob der Benutzer berechtigt ist, die angeforderte Aktion auszuführen. Die `AuthorizeRequest`-Methode wird nicht vom Client aufgerufen. Stattdessen authentifizieren Sie den Benutzer über den Standard Authentifizierungsmechanismus Ihrer Anwendung.

Im folgenden Beispiel wird gezeigt, wie Anforderungen auf authentifizierte Benutzer beschränkt werden.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Sie können eine beliebige angepasste Autorisierungs Logik in der Methode "autorizerequest" hinzufügen. Beispielsweise wird überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.
