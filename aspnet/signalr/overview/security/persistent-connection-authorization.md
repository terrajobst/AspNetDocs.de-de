---
uid: signalr/overview/security/persistent-connection-authorization
title: Authentifizierung und Autorisierung für permanente signalr-Verbindungen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen. Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467475"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Authentifizierung und Autorisierung für permanente SignalR-Verbindungen

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird beschrieben, wie Sie die Autorisierung für eine permanente Verbindung erzwingen. Allgemeine Informationen zur Integration der Sicherheit in eine signalr-Anwendung finden [Sie unter Einführung in die Sicherheit](introduction-to-security.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr Version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Vorherige Versionen dieses Themas
>
> Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

## <a name="enforce-authorization"></a>Autorisierung erzwingen

Zum Erzwingen von Autorisierungs Regeln bei Verwendung von [persistentconnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie die `AuthorizeRequest`-Methode überschreiben. Das `Authorize`-Attribut kann nicht mit permanenten Verbindungen verwendet werden. Die `AuthorizeRequest`-Methode wird vor jeder Anforderung vom signalr-Framework aufgerufen, um zu überprüfen, ob der Benutzer berechtigt ist, die angeforderte Aktion auszuführen. Die `AuthorizeRequest`-Methode wird nicht vom Client aufgerufen. Stattdessen authentifizieren Sie den Benutzer über den Standard Authentifizierungsmechanismus Ihrer Anwendung.

Im folgenden Beispiel wird gezeigt, wie Anforderungen auf authentifizierte Benutzer beschränkt werden.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Sie können eine beliebige angepasste Autorisierungs Logik in der Methode "autorizerequest" hinzufügen. Beispielsweise wird überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.
