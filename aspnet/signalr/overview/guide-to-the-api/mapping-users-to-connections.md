---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Zuordnung von signalr-Benutzern zu Verbindungen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden. Patrick Fletcher hat das Schreiben dieses Themas unterstützt. In diesem Thema verwendete Software Versionen...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431391"
---
# <a name="mapping-signalr-users-to-connections"></a>Zuordnen von SignalR-Benutzern zu Verbindungen

von [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden.
>
> Patrick Fletcher hat das Schreiben dieses Themas unterstützt.
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

## <a name="introduction"></a>Einführung

Jeder Client, der eine Verbindung mit einem Hub herstellt, übergibt eine eindeutige Verbindungs-ID. Sie können diesen Wert in der `Context.ConnectionId`-Eigenschaft des Hub-Kontexts abrufen. Wenn Ihre Anwendung einen Benutzer der Verbindungs-ID zuordnen und diese Zuordnung beibehalten muss, können Sie eine der folgenden Aktionen verwenden:

- [Der Benutzer-ID-Anbieter (signalr 2)](#IUserIdProvider)
- [In-Memory-Speicher](#inmemory), z. b. ein Wörterbuch
- [Signalr-Gruppe für jeden Benutzer](#groups)
- [Permanenter, externer Speicher](#database), z. b. eine Datenbanktabelle oder ein Azure-Tabellen Speicher

Jede dieser Implementierungen wird in diesem Thema gezeigt. Verwenden Sie die Methoden `OnConnected`, `OnDisconnected`und `OnReconnected` der `Hub`-Klasse, um den Benutzer Verbindungsstatus zu verfolgen.

Der beste Ansatz für Ihre Anwendung hängt von folgenden Informationen ab:

- Die Anzahl von Webservern, die Ihre Anwendung hosten.
- Gibt an, ob Sie eine Liste der derzeit verbundenen Benutzer erhalten müssen.
- Ob Sie Gruppen-und Benutzerinformationen beibehalten müssen, wenn die Anwendung oder der Server neu gestartet wird.
- Gibt an, ob die Latenz beim Aufrufen eines externen Servers ein Problem ist.

Die folgende Tabelle zeigt, welcher Ansatz für diese Überlegungen funktioniert.

|  | Mehr als ein Server | Liste der derzeit verbundenen Benutzer erhalten | Beibehalten von Informationen nach Neustarts | Optimale Leistung |
| --- | --- | --- | --- | --- |
| UserID-Anbieter | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| Im Arbeitsspeicher |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Einzelbenutzer Gruppen | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanent, extern | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Iuserid-Anbieter

Mit dieser Funktion können Benutzer angeben, welche Benutzer-ID auf einem irequest basiert, und zwar über eine neue Schnittstelle iuseridprovider.

**Der iuseridprovider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Standardmäßig wird eine Implementierung verwendet, die die `IPrincipal.Identity.Name` des Benutzers als Benutzernamen verwendet. Um dies zu ändern, registrieren Sie Ihre Implementierung von `IUserIdProvider` beim globalen Host beim Starten der Anwendung:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Innerhalb eines Hubs können Sie Nachrichten über die folgende API an diese Benutzer senden:

**Senden einer Nachricht an einen bestimmten Benutzer**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>In-Memory-Speicher

In den folgenden Beispielen wird veranschaulicht, wie die Verbindungs-und Benutzerinformationen in einem Wörterbuch beibehalten werden, das im Arbeitsspeicher gespeichert ist. Das Wörterbuch verwendet einen `HashSet` zum Speichern der Verbindungs-ID. Zu jedem Zeitpunkt kann ein Benutzer über mehr als eine Verbindung mit der signalr-Anwendung verfügen. Beispielsweise würde ein Benutzer, der über mehrere Geräte oder mehr als eine Browser Registerkarte verbunden ist, mehr als eine Verbindungs-ID aufweisen.

Wenn die Anwendung heruntergefahren wird, gehen alle Informationen verloren, aber Sie werden erneut aufgefüllt, wenn die Benutzer ihre Verbindungen wiederherstellen. In-Memory-Speicher funktioniert nicht, wenn Ihre Umgebung mehr als einen Webserver umfasst, da jeder Server über eine separate Auflistung von Verbindungen verfügt.

Im ersten Beispiel wird eine Klasse gezeigt, die die Zuordnung von Benutzern zu Verbindungen verwaltet. Der Schlüssel für das HashSet ist der Name des Benutzers.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Im nächsten Beispiel wird gezeigt, wie die Verbindungs Mapping-Klasse von einem Hub verwendet wird. Die Instanz der-Klasse wird in einem Variablennamen `_connections`gespeichert.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Einzelbenutzer Gruppen

Sie können für jeden Benutzer eine Gruppe erstellen und dann eine Nachricht an diese Gruppe senden, wenn Sie nur diesen Benutzer erreichen möchten. Der Name der einzelnen Gruppen ist der Name des Benutzers. Wenn ein Benutzer über mehrere Verbindungen verfügt, wird jede Verbindungs-ID der Gruppe des Benutzers hinzugefügt.

Sie sollten den Benutzer nicht manuell aus der Gruppe entfernen, wenn der Benutzer die Verbindung trennt. Diese Aktion wird automatisch vom signalr-Framework ausgeführt.

Im folgenden Beispiel wird gezeigt, wie Einzelbenutzer Gruppen implementiert werden.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Permanenter, externer Speicher

In diesem Thema wird gezeigt, wie Sie eine Datenbank oder einen Azure-Tabellen Speicher zum Speichern von Verbindungsinformationen verwenden. Diese Vorgehensweise funktioniert, wenn Sie über mehrere Webserver verfügen, da jeder Webserver mit demselben Datenrepository interagieren kann. Wenn die Webserver nicht mehr funktionieren oder die Anwendung neu gestartet wird, wird die `OnDisconnected`-Methode nicht aufgerufen. Daher ist es möglich, dass für Ihr Datenrepository Datensätze für Verbindungs-IDs vorhanden sind, die nicht mehr gültig sind. Zum Bereinigen dieser verwaisten Datensätze möchten Sie möglicherweise alle Verbindungen ungültig machen, die außerhalb eines Zeitraums erstellt wurden, der für Ihre Anwendung relevant ist. Die Beispiele in diesem Abschnitt enthalten einen Wert für die Nachverfolgung, wann die Verbindung erstellt wurde. Sie zeigen jedoch nicht, wie Sie alte Datensätze bereinigen, da dies möglicherweise als Hintergrundprozess gewünscht ist.

### <a name="database"></a>Datenbank

In den folgenden Beispielen wird gezeigt, wie die Verbindungs-und Benutzerinformationen in einer-Datenbank beibehalten werden. Sie können beliebige Datenzugriffs Technologien verwenden; Das folgende Beispiel zeigt jedoch, wie Modelle mithilfe von Entity Framework definiert werden. Diese Entitäts Modelle entsprechen Datenbanktabellen und-Feldern. Abhängig von den Anforderungen Ihrer Anwendung kann die Datenstruktur erheblich variieren.

Im ersten Beispiel wird gezeigt, wie eine Benutzer Entität definiert wird, die vielen Verbindungs Entitäten zugeordnet werden kann.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure Table Storage

Das folgende Beispiel für Azure Table Storage ähnelt dem Daten Bank Beispiel. Sie enthält nicht alle Informationen, die Sie für den Einstieg in Azure Table Storage Service benötigen. Weitere Informationen finden [Sie unter Verwenden des Tabellen Speichers mit .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Das folgende Beispiel zeigt eine Tabellen Entität zum Speichern von Verbindungsinformationen. Die Daten werden nach Benutzername partitioniert, und jede Entität wird durch die Verbindungs-ID identifiziert, sodass ein Benutzer jederzeit über mehrere Verbindungen verfügen kann.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Im Hub verfolgen Sie den Status der einzelnen Benutzer Verbindungen.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
