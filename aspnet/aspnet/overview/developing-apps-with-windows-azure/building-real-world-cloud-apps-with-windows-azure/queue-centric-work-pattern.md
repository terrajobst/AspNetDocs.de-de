---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Warteschlangen zentriertes Arbeitsmuster (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Das e-Book zur Entwicklung realer Cloud-apps mit Azure basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Vorgehensweisen erläutert, für die er...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c73b070f11366e781bcea70ffc84fd49a47d469a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582768"
---
# <a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Warteschlangen zentriertes Arbeitsmuster (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Verfahren erläutert, die Ihnen bei der Entwicklung von Web-Apps für die Cloud helfen können. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Wir haben bereits gesehen, dass die Verwendung mehrerer Dienste zu einer "zusammengesetzten" SLA führen kann, bei der die effektive SLA der APP das *Produkt* der einzelnen SLAs ist. Beispielsweise verwendet die Korrektur der IT-App Websites, Storage und SQL-Datenbank. Wenn einer dieser Dienste ausfällt, gibt die APP einen Fehler an den Benutzer zurück.

Caching ist eine gute Möglichkeit, vorübergehende Fehler bei Schreib geschütztem Inhalt zu behandeln. Aber was ist, wenn Ihre Anwendung funktionieren muss? Wenn der Benutzer z. b. eine neue Korrektur der IT-Aufgabe übermittelt, kann die APP die Aufgabe nicht einfach im Cache ablegen. Die APP muss die Korrektur-IT-Aufgabe in einen persistenten Datenspeicher schreiben, damit Sie verarbeitet werden kann.

An dieser Stelle kommt das Warteschlangen zentrierte Arbeitsmuster ins Spiel. Dieses Muster ermöglicht eine lose Kopplung zwischen einer Webschicht und einem Back-End-Dienst.

Im folgenden wird erläutert, wie das Muster funktioniert. Wenn die Anwendung eine Anforderung erhält, wird eine Arbeitsaufgabe in eine Warteschlange eingefügt, und die Antwort wird sofort zurückgegeben. Anschließend werden von einem separaten Back-End-Prozess Arbeitsaufgaben aus der Warteschlange abgerufen und die Arbeit erledigt.

Das Warteschlangen zentrierte Arbeitsmuster ist hilfreich für:

- Arbeiten, die zeitaufwändig sind (hohe Latenzzeit).
- Arbeiten, die einen externen Dienst erfordern, der möglicherweise nicht immer verfügbar ist.
- Arbeiten Sie ressourcenintensiv (hohe CPU-Auslastung).
- Arbeiten, die von einem Raten Ausgleich profitieren würden (bei plötzlichen Lastspitzen).

## <a name="reduced-latency"></a>Reduzierte Latenz

Warteschlangen sind immer dann nützlich, wenn Sie zeitaufwändige Arbeiten erledigen. Wenn eine Aufgabe ein paar Sekunden oder länger dauert, fügen Sie das Arbeits Element in eine Warteschlange ein, anstatt den Endbenutzer zu blockieren. Teilen Sie dem Benutzer "Wir arbeiten an diesem", und verwenden Sie dann einen Warteschlangenlistener, um die Aufgabe im Hintergrund zu verarbeiten.

Wenn Sie z. b. etwas bei einem Onlinehändler kaufen, bestätigt die Website sofort die Bestellung. Dies bedeutet jedoch nicht, dass sich Ihre Inhalte bereits in einem LKW befinden, der übermittelt wird. Eine Aufgabe wird in eine Warteschlange gestellt, und im Hintergrund werden die Überprüfungen durchgeführt, die Elemente werden für den Versand vorbereitet, usw.

Bei Szenarios mit kurzer Latenzzeit kann die gesamte End-to-End-Zeit für die Verwendung einer Warteschlange länger sein, als wenn die Aufgabe synchron ausgeführt wird. Doch selbst dann können diese Nachteile durch die anderen Vorteile überwiegen.

## <a name="increased-reliability"></a>Höhere Zuverlässigkeit

In der Version von, die wir bereits gesehen haben, ist das Web-Front-End eng mit dem SQL-Datenbank-Back-End verbunden. Wenn der SQL-Datenbankdienst nicht verfügbar ist, erhält der Benutzer eine Fehlermeldung. Wenn Wiederholungen nicht funktionieren (d. h., der Fehler ist mehr als vorübergehend), können Sie nur einen Fehler anzeigen und den Benutzer bitten, den Vorgang später erneut auszuführen.

![Diagramm, das anzeigt, dass das Web-Front-End fehlschlägt, wenn das Back-End](queue-centric-work-pattern/_static/image1.png)

Mithilfe von Warteschlangen schreibt die APP eine Nachricht in die Warteschlange, wenn ein Benutzer eine Korrektur-IT-Aufgabe sendet. Die Nachrichten Nutzlast ist eine [JSON](http://json.org/) -Darstellung der Aufgabe. Sobald die Nachricht in die Warteschlange geschrieben wird, gibt die APP zurück und zeigt sofort eine Erfolgsmeldung an den Benutzer an.

Wenn einer der Back-End-Dienste – z. b. die SQL-Datenbank oder der Warteschlangen Listener, offline geschaltet wird, können Benutzer weiterhin neue Korrektur-IT-Aufgaben übermitteln. Die Nachrichten werden erst wieder in die Warteschlange eingereiht, bis die Back-End-Dienste An diesem Punkt werden die Back-End-Dienste im Rückstand auffangen.

![Diagramm, das anzeigt, dass das Web-Front-End weiterhin funktioniert, wenn ein SQL-Datenbankfehler auftritt](queue-centric-work-pattern/_static/image2.png)

Außerdem können Sie jetzt weitere Back-End-Logik hinzufügen, ohne sich Gedanken über die Resilienz des Front-Ends machen zu müssen. Angenommen, Sie möchten eine e-Mail oder SMS an den Besitzer senden, wenn eine neue Korrektur zugewiesen wird. Wenn die e-Mail oder der SMS-Dienst nicht mehr verfügbar ist, können Sie alles andere verarbeiten und dann eine Nachricht in eine separate Warteschlange zum Senden von e-Mails/SMS-Nachrichten einfügen.

Zuvor war unsere effektive SLA Web-Apps &times; Storage &times; SQL-Datenbank = 99,7%. (Siehe [Design, um Fehler zu überstehen](design-to-survive-failures.md).)

Wenn Sie die APP für die Verwendung einer Warteschlange ändern, hängt das Web-Front-End nur von Web-Apps und Speicher für eine zusammengesetzte SLA von 99,8% ab. (Beachten Sie, dass Warteschlangen zum Azure Storage-Dienst gehören, sodass Sie in derselben SLA wie BLOB Storage enthalten sind.)

Wenn Sie noch mehr als 99,8% benötigen, können Sie zwei Warteschlangen in zwei verschiedenen Regionen erstellen. Legen Sie einen als primären und den anderen als sekundären fest. Führen Sie in Ihrer APP ein Failover zur sekundären Warteschlange aus, wenn die primäre Warteschlange nicht verfügbar ist. Die Wahrscheinlichkeit, dass beide gleichzeitig verfügbar sind, ist sehr gering.

## <a name="rate-leveling-and-independent-scaling"></a>Raten Ausgleich und unabhängige Skalierung

Warteschlangen sind auch nützlich für die Bezeichnung *Raten* Ausgleich oder *Lasten*Ausgleich.

Web-Apps sind häufig anfällig für plötzliche Spitzen im Datenverkehr. Obwohl Sie die automatische Skalierung zum automatischen Hinzufügen von Webservern zur Verarbeitung von erhöhtem Webdatenverkehr verwenden können, kann die automatische Skalierung möglicherweise nicht schnell genug reagieren, um abrupte Spitzen beim Laden zu verarbeiten. Wenn die Webserver einen Teil der Arbeit Abladen können, indem Sie eine Nachricht in eine Warteschlange schreiben, können Sie mehr Datenverkehr verarbeiten. Ein Back-End-Dienst kann dann Nachrichten aus der Warteschlange lesen und verarbeiten. Die Tiefe der Warteschlange wird vergrößert oder verkleinert, wenn die eingehende Auslastung variiert.

Mit einem Großteil seiner zeitaufwändigen Arbeit an einem Back-End-Dienst kann die webeebene leichter auf plötzliche Spitzen im Datenverkehr reagieren. Und sparen Sie Geld, da eine bestimmte Menge an Datenverkehr von weniger Webservern verarbeitet werden kann.

Sie können die Web-und Back-End-Dienste unabhängig voneinander skalieren. Beispielsweise benötigen Sie möglicherweise drei Webserver, aber nur einen Server, der Warteschlangen Nachrichten verarbeitet. Wenn Sie im Hintergrund Eine rechenintensive Aufgabe ausführen, benötigen Sie möglicherweise weitere Back-End-Server.

![](queue-centric-work-pattern/_static/image3.png)

Die automatische Skalierung funktioniert sowohl mit Back-End-Diensten als auch mit der webeebene. Sie können die Anzahl der VMS, die die Aufgaben in der Warteschlange verarbeiten, basierend auf der CPU-Auslastung der Back-End-VMS zentral hoch-oder Herunterskalieren. Oder Sie können basierend auf der Anzahl der Elemente in einer Warteschlange automatisch skalieren. Beispielsweise können Sie die automatische Skalierung angeben, um zu versuchen, höchstens 10 Elemente in der Warteschlange zu speichern. Wenn die Warteschlange mehr als 10 Elemente umfasst, werden die virtuellen Computer durch die automatische Skalierung hinzugefügt. Wenn Sie das auffangen, werden die zusätzlichen VMS von der automatischen Skalierung herunterskaliert.

## <a name="adding-queues-to-the-fix-it-application"></a>Hinzufügen von Warteschlangen zur Korrektur der IT-Anwendung

Um das Warteschlangen Muster zu implementieren, müssen wir zwei Änderungen an der Korrektur-App vornehmen.

- Wenn ein Benutzer eine neue korrigierende Aufgabe übermittelt, platzieren Sie die Aufgabe in der Warteschlange, anstatt Sie in die Datenbank zu schreiben.
- Erstellen Sie einen Back-End-Dienst, der Nachrichten in der Warteschlange verarbeitet.

Für die Warteschlange verwenden wir den [Azure Queue Storage-Dienst](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Eine weitere Option ist die Verwendung [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Um zu entscheiden, welcher Warteschlangen Dienst verwendet werden soll, sollten Sie überlegen, wie Ihre APP die Nachrichten in der Warteschlange senden und empfangen muss:

- Wenn Sie Hersteller und konkurrierende Consumer zusammenarbeiten, sollten Sie die Verwendung von Azure Queue Storage Service in Erwägung gezogen. "Kooperierende Producer" bedeutet, dass mehrere Prozesse einer Warteschlange Nachrichten hinzufügen. "Konkurrierende Consumer" bedeutet, dass mehrere Prozesse Nachrichten aus der Warteschlange abrufen, um Sie zu verarbeiten, aber eine bestimmte Nachricht kann nur von einem "Consumer" verarbeitet werden. Wenn Sie mehr Durchsatz benötigen, als Sie mit einer einzelnen Warteschlange erreichen können, verwenden Sie zusätzliche Warteschlangen und/oder zusätzliche Speicher Konten.
- Wenn Sie ein [Veröffentlichungs-/Abonnementmodell](http://en.wikipedia.org/wiki/Publish/subscribe)benötigen, sollten Sie Azure Service Bus Warteschlangen verwenden.

Die Korrektur der IT-App eignet sich für das Modell der Zusammenarbeit und der konkurrierenden Kunden.

Ein weiterer Aspekt ist die Verfügbarkeit von Anwendungen. Der Queue Storage-Dienst ist Teil desselben Dienstanbieter, den wir für BLOB Storage verwenden, sodass die Verwendung der SLA nicht beeinträchtigt wird. Azure Service Bus ist ein separater Dienst mit einer eigenen SLA. Wenn wir Service Bus Warteschlangen verwenden, müssen wir einen zusätzlichen SLA-Prozentsatz berücksichtigen, und unser zusammengesetztes SLA wäre niedriger. Wenn Sie einen Warteschlangen Dienst auswählen, sollten Sie sicherstellen, dass Sie die Auswirkung ihrer Wahl auf die Verfügbarkeit der Anwendung verstehen. Weitere Informationen finden Sie im Abschnitt "Resources" ( [Ressourcen](#resources) ).

## <a name="creating-queue-messages"></a>Erstellen von Warteschlangen Nachrichten

Das Web-Front-End führt die folgenden Schritte aus, um einen Korrektur-IT-Task in der Warteschlange zu platzieren:

1. Erstellen Sie eine [cloudqueueclient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) -Instanz. Die `CloudQueueClient`-Instanz wird verwendet, um Anforderungen für den Warteschlangen Dienst auszuführen.
2. Erstellen Sie die Warteschlange, falls Sie noch nicht vorhanden ist.
3. Serialisieren Sie den Task "reparieren".
4. Aufrufen von [cloudqueue. addmessageasync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) , um die Nachricht in die Warteschlange zu stellen.

Dies wird im Konstruktor und in `SendMessageAsync`-Methode einer neuen `FixItQueueManager` Klasse durchführen.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Hier verwenden wir die [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) -Bibliothek, um das Format "atxit" in das JSON-Format zu serialisieren. Sie können den gewünschten serialisierungsansatz verwenden. JSON hat den Vorteil, dass er lesbarer ist, während er weniger ausführlich als XML ist.

Code in Produktionsqualität würde eine Fehler Behandlungs Logik hinzufügen, anhalten, wenn die Datenbank nicht mehr verfügbar ist, Wiederherstellung bereinitigerer behandeln, die Warteschlange beim Starten der Anwendung erstellen und "nicht verarbeitbare[" Nachrichten](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx)verwalten. (Eine nicht verarbeitbare Nachricht ist eine Nachricht, die aus irgendeinem Grund nicht verarbeitet werden kann. Sie möchten nicht, dass nicht verarbeitbare Nachrichten in der Warteschlange angeordnet werden, wo die workerrolle ständig versucht, Sie zu verarbeiten, fehlschlägt, den Versuch wiederholen, fehlschlägt usw.)

In der Front-End-MVC-Anwendung müssen wir den Code aktualisieren, mit dem eine neue Aufgabe erstellt wird. Anstatt die Aufgabe im Repository zu platzieren, müssen Sie die oben gezeigte `SendMessageAsync` Methode aufzurufen.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Verarbeiten von Warteschlangen Nachrichten

Zum Verarbeiten von Nachrichten in der Warteschlange erstellen wir einen Back-End-Dienst. Der Back-End-Dienst führt eine Endlosschleife aus, mit der die folgenden Schritte ausgeführt werden:

1. Die nächste Nachricht aus der Warteschlange erhalten.
2. Deserialisieren Sie die Nachricht in eine Korrektur-IT-Aufgabe.
3. Schreiben Sie den Task "korrigieren Sie" in die Datenbank.

Zum Hosten des Back-End-Diensts erstellen wir einen Azure-clouddienst, der eine *workerrolle*enthält. Eine workerrolle besteht aus mindestens einem virtuellen Computer, der die Back-End-Verarbeitung ausführen kann. Der Code, der auf diesen VMS ausgeführt wird, ruft Nachrichten aus der Warteschlange ab, sobald Sie verfügbar werden. Für jede Nachricht Deserialisieren wir die JSON-Nutzlast und schreiben eine Instanz der Entität "IT-Aufgabe korrigieren" in die Datenbank. dabei wird das gleiche Repository verwendet, das wir zuvor auf der webeebene verwendet haben.

Die folgenden Schritte zeigen, wie Sie ein workerrollenprojekt zu einer Projekt Mappe hinzufügen, die über ein Standardweb Projekt verfügt. Diese Schritte wurden bereits im Korrektur-IT-Projekt durchgeführt, das Sie herunterladen können.

Fügen Sie zunächst ein clouddienstprojekt zur Visual Studio-Projekt Mappe hinzu. Klicken Sie mit der rechten Maustaste auf die Lösung, und wählen Sie **Hinzufügen**und dann **Neues Projekt** Erweitern Sie im linken Bereich **Visualisierung C#**  , und wählen Sie **Cloud**aus.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Erweitern Sie im Dialogfeld **neuer Azure-Cloud-Dienst** im linken Bereich den Knoten **visuelle C#**  Knoten. Wählen Sie **workerrolle** aus, und klicken Sie auf das Pfeilsymbol.

![](queue-centric-work-pattern/_static/image6.png)

(Beachten Sie, dass Sie auch eine *webrolle*hinzufügen können. Wir könnten das Korrektur-IT-Front-End im selben clouddienst ausführen, anstatt es auf einer Azure-Website auszuführen. Dies hat einige Vorteile, wenn Verbindungen zwischen Front-End und Back-End leichter koordiniert werden. Um diese Demo aber einfach zu halten, behalten wir das Front-End in einer Azure App Service Web-App bei und führen nur das Back-End in einem clouddienst aus.)

Der workerrolle wird ein Standardname zugewiesen. Um den Namen zu ändern, zeigen Sie mit der Maus auf die workerrolle im rechten Bereich, und klicken Sie dann auf das Stift Symbol.

![](queue-centric-work-pattern/_static/image7.png)

Klicken Sie auf **OK** , um das Dialogfeld abzuschließen. Dadurch werden der Visual Studio-Projekt Mappe zwei Projekte hinzugefügt.

- ein Azure-Projekt, das den clouddienst definiert, einschließlich der Konfigurationsinformationen.
- Ein workerrollenprojekt, das die workerrolle definiert.

![](queue-centric-work-pattern/_static/image8.png)

Weitere Informationen finden Sie unter [Erstellen eines Azure-Projekts mit Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Innerhalb der workerrolle werden Nachrichten abgerufen, indem die `ProcessMessageAsync`-Methode der zuvor erkannten `FixItQueueManager`-Klasse aufgerufen wird.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Die `ProcessMessagesAsync`-Methode überprüft, ob eine Nachricht wartet. Wenn eine solche Nachricht vorhanden ist, wird die Nachricht in eine `FixItTask` Entität deserialisiert und die Entität in der Datenbank gespeichert. Sie wird durchlaufen, bis die Warteschlange leer ist.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Das Abrufen von Warteschlangen Nachrichten führt zu einer geringen Transaktionskosten. wenn keine Nachricht vorhanden ist, die auf die Verarbeitung wartet, wartet die `RunAsync`-Methode der workerrolle eine Sekunde vor dem Abruf, indem `Task.Delay(1000)`aufgerufen wird.

Durch das Hinzufügen von asynchronem Code in einem Webprojekt kann die Leistung automatisch verbessert werden, da IIS einen eingeschränkten Thread Pool verwaltet. Dies ist in einem workerrollenprojekt nicht der Fall. Um die Skalierbarkeit der workerrolle zu verbessern, können Sie Multithreadcode schreiben oder asynchronen Code verwenden, um die [parallele Programmierung](https://msdn.microsoft.com/library/ff963553.aspx)zu implementieren. Das Beispiel implementiert nicht die parallele Programmierung, sondern zeigt, wie Sie den Code asynchron machen, damit Sie die parallele Programmierung implementieren können.

## <a name="summary"></a>Summary

In diesem Kapitel haben Sie erfahren, wie Sie die Reaktionsfähigkeit, Zuverlässigkeit und Skalierbarkeit von Anwendungen verbessern, indem Sie das Warteschlangen zentrierte Arbeitsmuster implementieren.

Dies ist das letzte der 13 in diesem e-book behandelten Muster, aber es gibt noch viele andere Muster und Verfahren, die Sie beim Erstellen erfolgreicher Cloud-apps unterstützen können. Das [letzte Kapitel](more-patterns-and-guidance.md) enthält Links zu Ressourcen für Themen, die in diesen 13 Mustern nicht behandelt wurden.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen zu Warteschlangen finden Sie in den folgenden Ressourcen.

Dokumentation:

- [Microsoft Azure Storage in der Warteschlange Teil 1: Getting Started](https://www.red-gate.com/simple-talk/cloud/platform-as-a-service/microsoft-azure-storage-queues-part-1-getting-started/). Artikel von Roman schacherl.
- [Ausführen von Hintergrundaufgaben](https://msdn.microsoft.com/library/ff803365.aspx), Kapitel 5 von [Verschieben von Anwendungen in die Cloud, Dritt-Edition](https://msdn.microsoft.com/library/ff728592.aspx) von Microsoft Patterns and Practices. (Insbesondere der Abschnitt ["Verwenden von Azure Storage Warteschlangen"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Bewährte Methoden zum Maximieren der Skalierbarkeit und Wirtschaftlichkeit von Warteschlangen basierten Messaging Lösungen in Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Whitepaper von Valery mizonov.
- [Vergleichen von Azure-Warteschlangen und Service Bus Warteschlangen](https://msdn.microsoft.com/magazine/jj159884.aspx). Der MSDN Magazine-Artikel enthält zusätzliche Informationen, die Ihnen bei der Auswahl des zu verwendenden Warteschlangen Dienstanbieter helfen können. Im Artikel wird erwähnt, dass Service Bus von ACS für die Authentifizierung abhängig ist. Dies bedeutet, dass Ihre SB-Warteschlangen nicht verfügbar sind, wenn ACS nicht verfügbar ist. Da der Artikel aber geschrieben wurde, wurde SB so geändert, dass Sie SAS- [Token](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) als Alternative zu ACS verwenden können.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Weitere Informationen finden Sie unter Einführung in asynchrone Nachrichten, Pipes und Filter Muster, kompenerendes Transaktionsmuster, Muster für konkurrierende Consumer, cqrs-Muster.
- [Cqrs-Journey](https://msdn.microsoft.com/library/jj554200). E-Book zu cqrs von Microsoft Patterns and Practices.

Video:

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Videoreihe von Ulrich Homann, Marc Mercuri und Mark Simms. Stellt allgemeine Konzepte und architektonische Prinzipien in einer sehr zugänglichen und interessanten Weise vor, wobei Storys von der Benutzerfreundlichkeit von Microsoft Customer Advisory Team (CAT) mit tatsächlichen Kunden erstellt werden. Eine Einführung in die Azure Storage-Dienst und-Warteschlangen finden Sie in der Episode 5 ab 35:13.

> [!div class="step-by-step"]
> [Zurück](distributed-caching.md)
> [Weiter](more-patterns-and-guidance.md)
