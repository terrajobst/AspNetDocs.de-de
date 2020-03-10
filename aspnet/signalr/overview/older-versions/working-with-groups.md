---
uid: signalr/overview/older-versions/working-with-groups
title: Arbeiten mit Gruppen in signalr 1. x | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird beschrieben, wie Sie Gruppen Mitgliedschafts Informationen mit der Hub-API beibehalten.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5f50dc162d6cdcfbf2261e6a751f5f99078d5c54
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467895"
---
# <a name="working-with-groups-in-signalr-1x"></a>Arbeiten mit Gruppen in SignalR 1.x

von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird beschrieben, wie Benutzer zu Gruppen hinzugefügt und Gruppen Mitgliedschafts Informationen persistent gespeichert werden.

## <a name="overview"></a>Übersicht

Gruppen in signalr bieten eine Methode zum Senden von Nachrichten an bestimmte Teilmengen verbundener Clients. Eine Gruppe kann über eine beliebige Anzahl von Clients verfügen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein. Sie müssen nicht explizit Gruppen erstellen. Eine Gruppe wird automatisch erstellt, wenn Sie Ihren Namen zum ersten Mal in einem Aufruf von Gruppen angeben. hinzufügen. Sie wird gelöscht, wenn Sie die letzte Verbindung aus der Mitgliedschaft entfernen. Eine Einführung in die Verwendung von Gruppen finden Sie unter [Verwalten der Gruppenmitgliedschaft von der Hub-Klasse](index.md) im Leitfaden "Hubs API-Server".

Es ist keine API zum erhalten einer Gruppen Mitgliedschafts Liste oder einer Liste von Gruppen vorhanden. Signalr sendet Nachrichten auf der Grundlage eines Pub/Sub-Modells an Clients und Gruppen, und der Server verwaltet keine Listen mit Gruppen oder Gruppenmitgliedschaften. Dadurch wird die Skalierbarkeit maximiert, denn wenn Sie einer Webfarm einen Knoten hinzufügen, muss jeder Status, den signalr beibehält, an den neuen Knoten weitergegeben werden.

Wenn Sie mit der `Groups.Add`-Methode einen Benutzer zu einer Gruppe hinzufügen, empfängt der Benutzer die Nachrichten, die für die Dauer der aktuellen Verbindung an diese Gruppe weitergeleitet werden. die Mitgliedschaft der Benutzer in dieser Gruppe wird jedoch nicht über die aktuelle Verbindung hinaus beibehalten. Wenn Sie Informationen zu Gruppen und Gruppenmitgliedschaften dauerhaft beibehalten möchten, müssen Sie diese Daten in einem Repository speichern, z. b. in einer Datenbank oder in einem Azure-Tabellen Speicher. Jedes Mal, wenn ein Benutzer eine Verbindung mit der Anwendung herstellt, rufen Sie aus dem Repository ab, zu dem der Benutzer gehört, und fügen diesen Benutzer manuell zu diesen Gruppen hinzu.

Wenn die Verbindung nach einer temporären Unterbrechung wieder hergestellt wird, wird der Benutzer automatisch erneut mit den zuvor zugewiesenen Gruppen verbunden. Das automatische erneute beitreten zu einer Gruppe gilt nur beim erneuten Herstellen der Verbindung, nicht beim Herstellen einer neuen Verbindung. Ein digital signiertes Token wird vom Client übermittelt, der die Liste der zuvor zugewiesenen Gruppen enthält. Wenn Sie überprüfen möchten, ob der Benutzer zu den angeforderten Gruppen gehört, können Sie das Standardverhalten überschreiben.

Dieses Thema enthält die folgenden Abschnitte:

- [Hinzufügen und Entfernen von Benutzern](#add)
- [Aufrufen von Mitgliedern einer Gruppe](#call)
- [Speichern von Gruppenmitgliedschaften in einer Datenbank](#storedatabase)
- [Speichern der Gruppenmitgliedschaft in Azure Table Storage](#storeazuretable)
- [Überprüfen der Gruppenmitgliedschaft beim erneuten Herstellen der Verbindung](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Hinzufügen und Entfernen von Benutzern

Um Benutzer zu einer Gruppe hinzuzufügen oder daraus zu entfernen, müssen Sie die Methoden zum [Hinzufügen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) oder [Entfernen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) aufzurufen und die Verbindungs-ID und den Gruppennamen des Benutzers als Parameter übergeben. Sie müssen einen Benutzer nicht manuell aus einer Gruppe entfernen, wenn die Verbindung beendet wird.

Das folgende Beispiel zeigt die `Groups.Add`-und `Groups.Remove` Methoden, die in hubmethoden verwendet werden.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Die Methoden `Groups.Add` und `Groups.Remove` werden asynchron ausgeführt.

Wenn Sie einer Gruppe einen Client hinzufügen und sofort mithilfe der Gruppe eine Nachricht an den Client senden möchten, müssen Sie sicherstellen, dass die Methode "Groups. Add" zuerst abgeschlossen wird. In den folgenden Codebeispielen wird veranschaulicht, wie Sie dies tun, eine mit Code, der in .NET 4,5 funktioniert, und eine mit Code, der in .NET 4 funktioniert.

#### <a name="asynchronous-net-45-example"></a>Beispiel für asynchrones .NET 4,5

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asynchronous .NET 4-Beispiel

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Im Allgemeinen sollten Sie `await` nicht einschließen, wenn Sie die `Groups.Remove`-Methode aufrufen, weil die Verbindungs-ID, die Sie entfernen möchten, möglicherweise nicht mehr verfügbar ist. In diesem Fall wird `TaskCanceledException` nach einem Timeout der Anforderung ausgelöst. Wenn die Anwendung sicherstellen muss, dass der Benutzer vor dem Senden einer Nachricht aus der Gruppe entfernt wurde, können Sie `await` vor Gruppen hinzufügen. entfernen Sie, und fangen Sie dann die `TaskCanceledException` Ausnahme ab, die ausgelöst werden kann.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Aufrufen von Mitgliedern einer Gruppe

Sie können Nachrichten an alle Mitglieder einer Gruppe oder nur an bestimmte Mitglieder der Gruppe senden, wie in den folgenden Beispielen gezeigt.

- **Alle** verbundenen Clients in einer bestimmten Gruppe. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Alle verbundenen Clients in einer angegebenen Gruppe **mit Ausnahme der angegebenen Clients**, identifiziert durch die Verbindungs-ID. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Alle verbundenen Clients in einer angegebenen Gruppe **mit Ausnahme des aufrufenden Clients**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Speichern von Gruppenmitgliedschaften in einer Datenbank

In den folgenden Beispielen wird gezeigt, wie Gruppen-und Benutzerinformationen in einer-Datenbank beibehalten werden. Sie können beliebige Datenzugriffs Technologien verwenden; Das folgende Beispiel zeigt jedoch, wie Modelle mithilfe von Entity Framework definiert werden. Diese Entitäts Modelle entsprechen Datenbanktabellen und-Feldern. Abhängig von den Anforderungen Ihrer Anwendung kann die Datenstruktur erheblich variieren. Dieses Beispiel enthält eine Klasse mit dem Namen `ConversationRoom`, die für eine Anwendung eindeutig ist, die es Benutzern ermöglicht, Konversationen über verschiedene Themen wie Sport oder Gärtnerei beizutreten. Dieses Beispiel enthält auch eine-Klasse für die-Verbindungen. Die Verbindungs Klasse ist für die Überwachung der Gruppenmitgliedschaft nicht unbedingt erforderlich, ist jedoch häufig Teil der robusten Lösung für die Nachverfolgung von Benutzern.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Anschließend können Sie im Hub die Gruppen-und Benutzerinformationen aus der Datenbank abrufen und den Benutzer manuell zu den entsprechenden Gruppen hinzufügen. Das Beispiel enthält keinen Code zum Nachverfolgen der Benutzer Verbindungen. In diesem Beispiel wird das `await`-Schlüsselwort nicht vor `Groups.Add` angewendet, da eine Nachricht nicht sofort an Mitglieder der Gruppe gesendet wird. Wenn Sie sofort nach dem Hinzufügen des neuen Members eine Nachricht an alle Mitglieder der Gruppe senden möchten, sollten Sie das `await`-Schlüsselwort anwenden, um sicherzustellen, dass der asynchrone Vorgang abgeschlossen wurde.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Speichern der Gruppenmitgliedschaft in Azure Table Storage

Die Verwendung von Azure Table Storage zum Speichern von Gruppen-und Benutzerinformationen ähnelt der Verwendung einer Datenbank. Das folgende Beispiel zeigt eine Tabellen Entität, in der der Benutzername und der Gruppenname gespeichert werden.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

Im Hub rufen Sie die zugewiesenen Gruppen ab, wenn der Benutzer eine Verbindung herstellt.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Überprüfen der Gruppenmitgliedschaft beim erneuten Herstellen der Verbindung

Standardmäßig weist signalr einen Benutzer automatisch den entsprechenden Gruppen zu, wenn eine Verbindung mit einer vorübergehenden Unterbrechung wieder hergestellt wird, z. b. Wenn eine Verbindung gelöscht und wieder hergestellt wird, bevor das Timeout der Verbindung auftritt. Die Gruppeninformationen des Benutzers werden beim erneuten Herstellen einer Verbindung in ein Token übermittelt, und dieses Token wird auf dem Server überprüft. Informationen zum Überprüfungs Vorgang für das erneute Hinzufügen von Benutzern zu Gruppen finden Sie unter [Wiederherstellen von Gruppen beim erneuten Herstellen der Verbindung](index.md).

Im Allgemeinen sollten Sie das Standardverhalten verwenden, wenn Gruppen bei der erneuten Verbindungs Herstellung automatisch erneut beitreten. Signalr-Gruppen sind nicht als Sicherheitsmechanismus zum Einschränken des Zugriffs auf sensible Daten vorgesehen. Wenn Ihre Anwendung jedoch die Gruppenmitgliedschaft eines Benutzers überprüfen muss, wenn die Verbindung wieder hergestellt wird, können Sie das Standardverhalten überschreiben. Wenn Sie das Standardverhalten ändern, kann dies zu einer Belastung der Datenbank hinzugefügt werden, da die Gruppenmitgliedschaft eines Benutzers für jede erneute Verbindung abgerufen werden muss, und nicht nur, wenn der Benutzer eine Verbindung herstellt.

Wenn Sie die Gruppenmitgliedschaft bei der erneuten Verbindungs Herstellung überprüfen müssen, erstellen Sie ein neues Hub-Pipeline Modul, das eine Liste der zugewiesenen Gruppen zurückgibt, wie unten gezeigt.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Fügen Sie dieses Modul dann der Hub-Pipeline hinzu, wie unten gezeigt.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
