---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Signalr-Verbindungs Dichte Tests mit Kurbel | Microsoft-Dokumentation
author: bradygaster
description: Testen der Verbindungsdichte in SignalR mit Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 901e039fbb81651ed18d560c99745b7e7f716e01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449871"
---
# <a name="signalr-connection-density-testing-with-crank"></a>Testen der Verbindungsdichte in SignalR mit Crank

von [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Artikel wird beschrieben, wie Sie mit dem Tool "Kurbel" eine Anwendung mit mehreren simulierten Clients testen.

Sobald Ihre Anwendung in ihrer Hostingumgebung (entweder eine Azure-webrolle, IIS oder selbstgeh ostet mithilfe von owin) ausgeführt wird, können Sie die Antwort der Anwendung auf ein hohes Maß an Verbindungs Dichte mit dem Kurbel Tool testen. Bei der Hostingumgebung kann es sich um einen Internetinformationsdienste (IIS)-Server, einen owin-Host oder eine Azure-webrolle handeln. (Hinweis: Leistungsindikatoren sind für Azure App Service-Web-Apps nicht verfügbar, sodass Sie keine Leistungsdaten aus einem Verbindungs Dichte Test erhalten können.)

Die Verbindungs Dichte bezieht sich auf die Anzahl der gleichzeitigen TCP-Verbindungen, die auf einem Server eingerichtet werden können. Jede TCP-Verbindung verursacht einen eigenen Aufwand, und das Öffnen einer großen Anzahl von Verbindungen im Leerlauf erzeugt letztendlich einen Speicher Engpass.

[Die signalr-Codebasis](https://github.com/signalr/signalr) enthält ein Auslastungs Test Tool mit dem Namen " **Kurbel**". Die neueste Version der Kurbel finden Sie in [der Verzweigung dev](https://github.com/SignalR/signalr/tree/dev) auf GitHub. Sie können [hier](https://github.com/SignalR/SignalR/archive/dev.zip)ein ZIP-Archiv der dev-Verzweigung der signalr-Codebasis herunterladen.

Die Kurbel kann verwendet werden, um den Arbeitsspeicher des Servers vollständig zu verzieren und so die Gesamtzahl der Verbindungen im Leerlauf auf der Server Hardware zu berechnen. Alternativ dazu können Sie auch die Kurbel zum Auslastungs Test des Servers mit einer bestimmten Menge an Speicherauslastung verwenden, indem Sie Verbindungen bis zu einer bestimmten Anzahl oder einen bestimmten Arbeitsspeicher Schwellenwert erreichen.

Beim Testen ist es wichtig, dass Sie Remote Clients verwenden, um den Wettbewerb von Ressourcen (d.h. TCP-Verbindungen und Arbeitsspeicher) zu vermeiden. Überwachen Sie die Clients, um sicherzustellen, dass keine Engpässe auftreten, die möglicherweise verhindern, dass der Server seine volle Kapazität erreicht (Arbeitsspeicher oder CPU). Möglicherweise müssen Sie die Anzahl der Clients erhöhen, um den Server vollständig zu laden.

### <a name="running-a-connection-density-test"></a>Ausführen eines Verbindungs Dichte Tests

In diesem Abschnitt werden die Schritte beschrieben, die zum Ausführen eines Verbindungs Dichte Tests für eine signalr-Anwendung erforderlich sind.

1. Laden Sie den [dev-Branch der signalr](https://github.com/SignalR/SignalR/archive/dev.zip)-Codebasis herunter, und erstellen Sie ihn. Navigieren Sie in einer Eingabeaufforderung zu &lt;Projektverzeichnis&gt;\src\Microsoft.Aspnet.SignalR.Crank\bin\debug.
2. Stellen Sie Ihre Anwendung in der vorgesehenen Hostingumgebung bereit. Notieren Sie sich den Endpunkt, den die Anwendung verwendet. Beispielsweise wird in der im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md)" erstellten Anwendung der Endpunkt `http://<yourhost>:8080/signalr`.
3. Installieren Sie [signalr-Leistungsindikatoren](signalr-performance.md#perfcounters) auf dem Server. Wenn Ihre Anwendung in Azure ausgeführt wird, finden Sie unter [Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle](using-signalr-performance-counters-in-an-azure-web-role.md)Weitere Informationen.

Nachdem Sie die CodeBase heruntergeladen und erstellt und Leistungsindikatoren auf Ihrem Host installiert haben, befindet sich das Befehlszeilen Tool "Kurbel" im Ordner "`src\Microsoft.AspNet.SignalR.Crank\bin\Debug`".

Folgende Optionen sind für das Tool "Kurbel" verfügbar:

- **/?** : Zeigt den Hilfe Bildschirm an. Die verfügbaren Optionen werden auch angezeigt, wenn der **URL** -Parameter ausgelassen wird.
- **/URL**: die URL für signalr-Verbindungen. Dieser Parameter ist erforderlich. Für eine signalr-Anwendung, die die Standard Zuordnung verwendet, endet der Pfad mit "/signalr".
- **/Transport**: der Name des verwendeten Transports. Der Standardwert ist `auto`, bei dem das am besten verfügbare Protokoll ausgewählt wird. Zu den Optionen gehören `WebSockets`, `ServerSentEvents`und `LongPolling` (`ForeverFrame` ist keine Option für die Kurbel, da der .NET-Client anstelle von Internet Explorer verwendet wird). Weitere Informationen dazu, wie signalr Transporte auswählt, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: die Anzahl der Clients, die in jedem Batch hinzugefügt werden. Der Standardwert ist 50.
- **/ConnectInterval**: das Intervall (in Millisekunden) zwischen dem Hinzufügen von Verbindungen. Der Standard ist 500.
- **/Connections**: die Anzahl von Verbindungen, die zum Laden der Anwendung verwendet werden. Der Standardwert ist 100.000.
- **/ConnectTimeout**: das Timeout in Sekunden, bevor der Test abgebrochen wird. Der Standardwert ist 300.
- **Minservermb**: die minimale Anzahl von Server Megabyte, die erreicht werden müssen. Der Standard ist 500.
- **SendBytes**: die Größe der Nutzlast, die an den Server gesendet wird (in Bytes). Die Standardeinstellung ist 0.
- **Sendinterval**: die Verzögerung in Millisekunden zwischen Nachrichten an den Server. Der Standard ist 500.
- **SendTimeout**: das Timeout in Millisekunden für Nachrichten an den Server. Der Standardwert ist 300.
- **Controllerurl**: die URL, unter der ein Client einen Controller-Hub hostet. Der Standardwert ist NULL (kein Controller-Hub). Der controllerhub wird beim Start der Kurbel Sitzung gestartet. Es wurde kein weiterer Kontakt zwischen dem Controller-Hub und der Kurbel ausgelöst.
- **Numclients**: die Anzahl der simulierten Clients, die eine Verbindung mit der Anwendung herstellen. Der Standardwert ist 1.
- **Protokolldatei**: der Dateiname für die Protokolldatei für den Testlauf. Der Standardwert lautet `crank.csv`.
- **Sampleingeterval**: die Zeit in Millisekunden zwischen Stichproben von Leistungs Zählern. Der Standardwert lautet 1000.
- **Signalrinstance**: der Instanzname für die Leistungsindikatoren auf dem Server. Standardmäßig wird der Client Verbindungsstatus verwendet.

### <a name="example"></a>Beispiel

Mit dem folgenden Befehl wird eine Website mit dem Namen `pfsignalr` in Azure getestet, die eine Anwendung auf Port 8080 mit einem Hub mit dem Namen "controllerhub" unter Verwendung von 100 Verbindungen hostet.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
