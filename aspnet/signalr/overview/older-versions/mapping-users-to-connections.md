---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Zuordnung von signalr-Benutzern zu Verbindungen in signalr 1. x | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449997"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Zuordnen von SignalR-Benutzern zu Verbindungen in SignalR 1.x

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden.

## <a name="introduction"></a>Einführung

Jeder Client, der eine Verbindung mit einem Hub herstellt, übergibt eine eindeutige Verbindungs-ID. Sie können diesen Wert in der `Context.ConnectionId`-Eigenschaft des Hub-Kontexts abrufen. Wenn Ihre Anwendung einen Benutzer der Verbindungs-ID zuordnen und diese Zuordnung beibehalten muss, können Sie eine der folgenden Aktionen verwenden:

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
| Im Arbeitsspeicher |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Einzelbenutzer Gruppen | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Permanent, extern | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>In-Memory-Speicher

In den folgenden Beispielen wird veranschaulicht, wie die Verbindungs-und Benutzerinformationen in einem Wörterbuch beibehalten werden, das im Arbeitsspeicher gespeichert ist. Das Wörterbuch verwendet einen `HashSet` zum Speichern der Verbindungs-ID. Zu jedem Zeitpunkt kann ein Benutzer über mehr als eine Verbindung mit der signalr-Anwendung verfügen. Beispielsweise würde ein Benutzer, der über mehrere Geräte oder mehr als eine Browser Registerkarte verbunden ist, mehr als eine Verbindungs-ID aufweisen.

Wenn die Anwendung heruntergefahren wird, gehen alle Informationen verloren, aber Sie werden erneut aufgefüllt, wenn die Benutzer ihre Verbindungen wiederherstellen. In-Memory-Speicher funktioniert nicht, wenn Ihre Umgebung mehr als einen Webserver umfasst, da jeder Server über eine separate Auflistung von Verbindungen verfügt.

Im ersten Beispiel wird eine Klasse gezeigt, die die Zuordnung von Benutzern zu Verbindungen verwaltet. Der Schlüssel für das HashSet ist der Name des Benutzers.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Im nächsten Beispiel wird gezeigt, wie die Verbindungs Mapping-Klasse von einem Hub verwendet wird. Die Instanz der-Klasse wird in einem Variablennamen `_connections`gespeichert.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Einzelbenutzer Gruppen

Sie können für jeden Benutzer eine Gruppe erstellen und dann eine Nachricht an diese Gruppe senden, wenn Sie nur diesen Benutzer erreichen möchten. Der Name der einzelnen Gruppen ist der Name des Benutzers. Wenn ein Benutzer über mehrere Verbindungen verfügt, wird jede Verbindungs-ID der Gruppe des Benutzers hinzugefügt.

Sie sollten den Benutzer nicht manuell aus der Gruppe entfernen, wenn der Benutzer die Verbindung trennt. Diese Aktion wird automatisch vom signalr-Framework ausgeführt.

Im folgenden Beispiel wird gezeigt, wie Einzelbenutzer Gruppen implementiert werden.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Permanenter, externer Speicher

In diesem Thema wird gezeigt, wie Sie eine Datenbank oder einen Azure-Tabellen Speicher zum Speichern von Verbindungsinformationen verwenden. Diese Vorgehensweise funktioniert, wenn Sie über mehrere Webserver verfügen, da jeder Webserver mit demselben Datenrepository interagieren kann. Wenn die Webserver nicht mehr funktionieren oder die Anwendung neu gestartet wird, wird die `OnDisconnected`-Methode nicht aufgerufen. Daher ist es möglich, dass für Ihr Datenrepository Datensätze für Verbindungs-IDs vorhanden sind, die nicht mehr gültig sind. Zum Bereinigen dieser verwaisten Datensätze möchten Sie möglicherweise alle Verbindungen ungültig machen, die außerhalb eines Zeitraums erstellt wurden, der für Ihre Anwendung relevant ist. Die Beispiele in diesem Abschnitt enthalten einen Wert für die Nachverfolgung, wann die Verbindung erstellt wurde. Sie zeigen jedoch nicht, wie Sie alte Datensätze bereinigen, da dies möglicherweise als Hintergrundprozess gewünscht ist.

### <a name="database"></a>Datenbank

In den folgenden Beispielen wird gezeigt, wie die Verbindungs-und Benutzerinformationen in einer-Datenbank beibehalten werden. Sie können beliebige Datenzugriffs Technologien verwenden; Das folgende Beispiel zeigt jedoch, wie Modelle mithilfe von Entity Framework definiert werden. Diese Entitäts Modelle entsprechen Datenbanktabellen und-Feldern. Abhängig von den Anforderungen Ihrer Anwendung kann die Datenstruktur erheblich variieren.

Im ersten Beispiel wird gezeigt, wie eine Benutzer Entität definiert wird, die vielen Verbindungs Entitäten zugeordnet werden kann.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure Table Storage

Das folgende Beispiel für Azure Table Storage ähnelt dem Daten Bank Beispiel. Sie enthält nicht alle Informationen, die Sie für den Einstieg in Azure Table Storage Service benötigen. Weitere Informationen finden [Sie unter Verwenden des Tabellen Speichers mit .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Das folgende Beispiel zeigt eine Tabellen Entität zum Speichern von Verbindungsinformationen. Die Daten werden nach Benutzername partitioniert, und jede Entität wird durch die Verbindungs-ID identifiziert, sodass ein Benutzer jederzeit über mehrere Verbindungen verfügen kann.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

Im Hub verfolgen Sie den Status der einzelnen Benutzer Verbindungen.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
