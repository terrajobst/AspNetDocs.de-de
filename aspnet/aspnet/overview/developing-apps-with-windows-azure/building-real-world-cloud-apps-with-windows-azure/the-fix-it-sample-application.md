---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Anhang: die Anwendung zum Beheben von IT-Beispielen (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation'
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471393"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Anhang: die Anwendung zum Beheben von IT-Beispielen (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des Projekts zum Reparieren von IT](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

In diesem Anhang im e-book erstellen realer Cloud-apps mit Azure sind die folgenden Abschnitte enthalten, die zusätzliche Informationen zur Lösung zum Beheben von IT-Beispielen enthalten, die Sie herunterladen können:

- [Bekannte Probleme](#knownissues)
- [bewährten Methoden](#bestpractices)
- [Ausführen der APP aus Visual Studio auf dem lokalen Computer](#run-in-vs)
- [Bereitstellen der Basis-app zum Azure App Service von Web-Apps mithilfe der Windows PowerShell-Skripts](#deploybase)
- [Problembehandlung bei Windows PowerShell-Skripts](#troubleshooting)
- [Bereitstellen der APP mit Warteschlangen Verarbeitung zum Azure App Service von Web-Apps und einem Azure-clouddienst](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Bekannte Probleme

Die Korrektur der IT-App wurde ursprünglich entwickelt, um so einfach wie möglich einige der in diesem e-book dargestellten Muster zu veranschaulichen. Da es sich bei dem e-Book jedoch um das entwickeln realer apps handelt, haben wir den Korrektur-IT-Code einem Überprüfungs-und Testprozess unterzogen, ähnlich wie bei der veröffentlichten Software. Wir haben eine Reihe von Problemen erkannt, und wie bei jeder realen Anwendung wurden einige von Ihnen korrigiert, und einige davon haben wir auf eine spätere Version zurückgestellt.

In der folgenden Liste finden Sie Probleme, die in einer Produktionsanwendung behoben werden sollten. aus einem Grund oder einem anderen Grund haben wir uns entschieden, sich in der ersten Version der Anwendung zum Beheben von IT-Beispielen nicht zu beheben.

### <a name="security"></a>Sicherheit

- Stellen Sie sicher, dass eine Aufgabe nicht einem nicht vorhandenen Besitzer zugewiesen werden kann.
- Stellen Sie sicher, dass Sie nur Aufgaben anzeigen und ändern können, die Sie erstellt oder Ihnen zugewiesen haben.
- Verwenden Sie HTTPS für Anmelde Seiten und Authentifizierungs Cookies.
- Geben Sie ein Zeitlimit für Authentifizierungs Cookies an.

### <a name="input-validation"></a>Eingabeüberprüfung

Im Allgemeinen führt eine Produktions-app mehr Eingabevalidierung durch als die Korrektur der IT-app. Beispielsweise sollte die Größe der Image Größe/Bild Datei, die für den Upload zulässig ist, eingeschränkt sein.

### <a name="administrator-functionality"></a>Administrator Funktionalität

Ein Administrator sollte in der Lage sein, den Besitz vorhandener Aufgaben zu ändern. Beispielsweise könnte der Ersteller eines Tasks das Unternehmen verlassen und nicht mehr mit der Berechtigung, die Aufgabe aufrechtzuerhalten, wenn der administrative Zugriff aktiviert ist.

### <a name="queue-message-processing"></a>Verarbeitung von Warteschlangen Nachrichten

Die Verarbeitung von Warteschlangen Nachrichten in der Korrektur, dass Sie einfach ist, um das Warteschlangen zentrierte Arbeitsmuster mit einer minimalen Menge an Code zu veranschaulichen. Dieser einfache Code ist für eine tatsächliche Produktionsanwendung nicht ausreichend.

- Der Code garantiert nicht, dass jede Warteschlangen Nachricht höchstens einmal verarbeitet wird. Wenn Sie eine Nachricht aus der Warteschlange erhalten, gibt es einen Timeout Zeitraum, in dem die Nachricht für andere Warteschlangen Listener unsichtbar ist. Wenn das Timeout abläuft, bevor die Nachricht gelöscht wird, wird die Nachricht wieder sichtbar. Wenn eine workerrolleninstanz viel Zeit für die Verarbeitung einer Nachricht benötigt, ist es theoretisch möglich, dass dieselbe Nachricht zweimal verarbeitet wird, was zu einer doppelten Aufgabe in der Datenbank führt. Weitere Informationen zu diesem Problem finden Sie unter [Verwenden von Azure Storage Warteschlangen](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Die Warteschlangen-Abruf Logik könnte kostengünstiger sein, indem der Nachrichten Abruf für die Batch Verarbeitung durch die Batch Verarbeitung durch Jedes Mal, wenn Sie [cloudqueue. getmessageasync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)aufrufen, fallen Transaktionskosten an. Stattdessen können Sie [cloudqueue. getmessagesasync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) aufrufen (Beachten Sie den Plural '), mit dem mehrere Nachrichten in einer einzelnen Transaktion abgerufen werden. Die Transaktionskosten für Azure Storage Warteschlangen sind sehr gering, sodass die Auswirkung auf die Kosten in den meisten Szenarien nicht beträchtlich ist.
- Die enge Schleife im Nachrichten Verarbeitungs Code der Warteschlange bewirkt eine CPU-Affinität, bei der virtuelle Computer mit mehreren Kernen nicht effizient genutzt werden. Ein besseres Design wäre die Task Parallelität, um mehrere asynchrone Aufgaben parallel auszuführen.
- Warteschlangen Nachrichten-Verarbeitung weist nur die rudimentäre Ausnahmebehandlung auf. Der Code verarbeitet z. b. keine nicht verarbeitbaren [Nachrichten](https://msdn.microsoft.com/library/ms789028.aspx). (Wenn die Nachrichtenverarbeitung eine Ausnahme auslöst, müssen Sie den Fehler protokollieren und die Nachricht löschen, oder die workerrolle versucht, Sie erneut zu verarbeiten, und die Schleife wird unbegrenzt fortgesetzt.)

### <a name="sql-queries-are-unbounded"></a>SQL-Abfragen sind unbegrenzt

Aktueller Korrektur: der IT-Code gibt keine Beschränkung für die Anzahl der Zeilen an, die die Abfragen für Index Seiten zurückgeben können. Wenn eine große Anzahl von Tasks in die Datenbank eingegeben wird, kann dies zu Leistungsproblemen führen. Die Lösung besteht darin, Paging zu implementieren. Ein Beispiel finden Sie unter [Sortieren, Filtern und Paging mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Empfohlene Modelle anzeigen

Die Korrektur der IT-App verwendet die fixttask-Entitäts Klasse, um Informationen zwischen dem Controller und der Ansicht zu übergeben. Eine bewährte Vorgehensweise ist die Verwendung von Ansichts Modellen. Das Domänen Modell (z. b. die fixttask-Entitäts Klasse) ist um das, was für die Daten Persistenz erforderlich ist, entworfen, während ein Ansichts Modell für die Datendarstellung entworfen werden kann. Weitere Informationen finden Sie unter [12 ASP.net bewährte Methoden für MVC](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Sicheres Abbild-BLOB empfohlen

Die Korrektur der IT-App speichert hochgeladene Images als öffentlich, was bedeutet, dass jeder, der die URL findet, auf die Bilder zugreifen kann. Die Images können anstelle von Public gesichert werden.

### <a name="no-powershell-automation-scripts-for-queues"></a>Keine PowerShell-Automatisierungs Skripts für Warteschlangen

PowerShell-Beispiel Skripts wurden nur für die Basisversion von Problembehebung geschrieben, die vollständig in Azure App Service Web-Apps ausgeführt wird. Wir haben keine Skripts zum Einrichten und Bereitstellen der Web-App sowie der clouddienstumgebung bereitgestellt, die für die Warteschlangen Verarbeitung erforderlich ist

### <a name="special-handling-for-html-codes-in-user-input"></a>Spezielle Behandlung von HTML-Codes in Benutzereingaben

ASP.net verhindert automatisch eine Vielzahl von Methoden, mit denen böswillige Benutzer Website übergreifende Skript Angriffe durch Eingabe von Skripts in Benutzereingabe-Textfeldern durchführen können. Und das MVC-`DisplayFor` Hilfsprogramm, das zum Anzeigen von Aufgaben Titeln und Notizen verwendet wird, werden Werte, die an den Browser gesendet werden, automatisch codiert. In einer Produktions-APP möchten Sie jedoch möglicherweise zusätzliche Maßnahmen ergreifen. Weitere Informationen finden Sie unter [Anforderungs Validierung in ASP.net](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Bewährte Methoden

Im folgenden finden Sie einige Probleme, die behoben wurden, nachdem Sie in Code Review erkannt und die ursprüngliche Version der behobene IT-App getestet wurden. Einige sind darauf zurückzuführen, dass der ursprüngliche Programmierer eine bestimmte bewährte Vorgehensweise nicht beachtet hat, etwas einfach, weil der Code schnell geschrieben wurde und nicht für freigegebene Software gedacht war. Hier werden die Probleme aufgelistet, wenn es etwas gibt, das wir aus dieser Überprüfung und den Tests kennengelernt haben, die für andere Benutzer hilfreich sein können, die auch Web-Apps entwickeln.

### <a name="dispose-the-database-repository"></a>Löschen des Datenbankrepository

Die `FixItTaskRepository` Klasse muss die Entity Framework `DbContext` Instanz verwerfen. Hierzu haben Sie `IDisposable` in der `FixItTaskRepository`-Klasse implementiert:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Beachten Sie, dass die `FixItTaskRepository` Instanz automatisch von autofac verworfen wird, damit wir Sie nicht explizit verwerfen müssen.

Eine andere Möglichkeit besteht darin, die `DbContext` Member-Variable aus `FixItTaskRepository`zu entfernen und stattdessen eine lokale `DbContext` Variable innerhalb der einzelnen Repository-Methoden innerhalb einer `using`-Anweisung zu erstellen. Beispiel:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrieren von Singletons bei di

Da nur eine Instanz der `PhotoService` Klasse und `Logger` Klasse benötigt wird, sollten diese Klassen [als einzelne Instanzen für die Abhängigkeitsinjektion](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*registriert werden:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicherheit: Fehlerdetails für Benutzer nicht anzeigen

Die ursprüngliche Fehlerbehebung für die IT-App verfügte nicht über eine generische Fehlerseite, sodass alle Ausnahmen auf die Benutzeroberfläche hochskalieren können. einige Ausnahmen, wie z. b. Daten bankverbindungs Fehler, können dazu führen, dass eine vollständige Stapel Überwachung im Browser angezeigt wird Ausführliche Fehlerinformationen können manchmal Angriffe durch böswillige Benutzer ermöglichen. Die Lösung besteht darin, die Ausnahme Details zu protokollieren und dem Benutzer eine Fehlerseite anzuzeigen, die keine Fehlerdetails enthält. Die Korrektur, dass die IT-App bereits protokolliert wurde, und um eine Fehlerseite anzuzeigen, haben wir `<customErrors mode=On>` in der Datei "Web. config" hinzugefügt.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Standardmäßig bewirkt dies, dass " *views\shared\error.cshtml* " für Fehler angezeigt wird. Sie können *Error. cshtml* anpassen oder eine eigene Fehlerseiten Ansicht erstellen und ein `defaultRedirect` Attribut hinzufügen. Sie können auch unterschiedliche Fehlerseiten für bestimmte Fehler angeben.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sicherheit: erlauben Sie nur, dass eine Aufgabe vom Ersteller bearbeitet wird.

Auf der Seite Index Index werden nur die vom angemeldeten Benutzer erstellten Aufgaben angezeigt, aber ein böswilliger Benutzer könnte eine URL mit einer ID für die Aufgabe eines anderen Benutzers erstellen. Wir haben Code in *DashboardController.cs* hinzugefügt, um 404 in diesem Fall zurückzugeben:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Ausnahmen nicht schlucken

Die ursprüngliche Korrektur der IT-APP hat nach dem Protokollieren einer Ausnahme, die aus einer SQL-Abfrage resultiert, lediglich NULL zurückgegeben

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Dadurch wird der Benutzer so aussehen, als wäre die Abfrage erfolgreich, aber es wurden keine Zeilen zurückgegeben. Die Lösung besteht darin, die Ausnahme nach abfangen und Protokollierung erneut auszulösen:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Alle Ausnahmen in workerrollen abfangen

Alle nicht behandelten Ausnahmen in einer workerrolle bewirken, dass der virtuelle Computer wieder verwendet wird, sodass Sie alles, was Sie in einem try-catch-Block ausführen, umschließen und alle Ausnahmen behandeln möchten.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Angeben der Länge für Zeichen folgen Eigenschaften in Entitäts Klassen

Zum Anzeigen von einfachem Code wurde in der ursprünglichen Version der IT-App für die Korrektur keine Längenangabe für die Felder der fixttask-Entität angegeben. Daher wurden Sie als varchar (max) in der Datenbank definiert. Folglich würde die Benutzeroberfläche fast jede Menge an Eingaben akzeptieren. Durch Angeben von Längen werden Grenzwerte festgelegt, die sowohl für Benutzereingaben auf der Webseite als auch für die Spaltengröße in der Datenbank gelten:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Private Member als schreibgeschützt markieren, wenn die Änderungen nicht erwartet werden

Beispielsweise wird in der `DashboardController`-Klasse eine Instanz von `FixItTaskRepository` erstellt und erwartet nicht, dass Sie geändert [wird. daher](https://msdn.microsoft.com/library/acdd6hb7.aspx)haben wir Sie als schreibgeschützt definiert.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Verwenden Sie die Liste. Any () anstelle der Liste. Count ()-&gt; 0

Wenn Sie sich für eine oder mehrere Elemente in einer Liste mit den angegebenen Kriterien interessieren, verwenden Sie die [any](https://msdn.microsoft.com/library/bb534972.aspx) -Methode, da Sie zurückgibt, sobald ein Element gefunden wird, das die Kriterien erfüllt, während die `Count` Methode immer alle Elemente durchlaufen muss. Die Datei "Dashboard *Index. cshtml* " enthielt ursprünglich den folgenden Code:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Wir haben Folgendes geändert:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generieren von URLs in MVC-Ansichten mithilfe von MVC-Hilfsprogrammen

Für die Schaltfläche **Fix erstellen** auf der Startseite können Sie die Korrektur der IT-App als Anker Element hart codieren:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Für Ansichts-/Aktionslinks wie diese empfiehlt es sich, das HTML-Hilfsprogramm [URL. Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) zu verwenden, z. b.:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Verwenden Sie Task. Delay anstelle von Thread. Sleep in der workerrolle.

Die Vorlage "New-Project" fügt `Thread.Sleep` in den Beispielcode für eine workerrolle ein, aber der Thread kann dazu führen, dass der Thread Pool zusätzliche unnötige Threads erzeugt. Sie können dies vermeiden, indem Sie stattdessen " [Task. Delay](https://msdn.microsoft.com/library/hh139096.aspx) " verwenden.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Vermeiden Sie Async void.

Wenn eine asynchrone Methode keinen Wert zurückgeben muss, geben Sie einen `Task` Typ statt `void`zurück.

Dieses Beispiel ist aus der `FixItQueueManager`-Klasse:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Sie sollten `async void` nur für Ereignishandler der obersten Ebene verwenden. Wenn Sie eine Methode als `async void`definieren, kann der Aufrufer nicht auf die Methode **warten** oder Ausnahmen abfangen, die von der Methode ausgelöst werden. Weitere Informationen finden Sie unter [bewährte Methoden bei der asynchronen Programmierung](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Verwenden eines Abbruch Tokens, um die workerrollenschleife zu unterbrechen

In der Regel enthält die **Run** -Methode für eine workerrolle eine Endlosschleife. Wenn die workerrolle angehalten wird, wird die [RoleEntrypoint. OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) -Methode aufgerufen. Verwenden Sie diese Methode, um die Arbeit abzubrechen, die in der **Run** -Methode ausgeführt wird, und ordnungsgemäß zu beenden. Andernfalls kann der Prozess in der Mitte eines Vorgangs beendet werden.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Automatisches Ausführen von MIME-Sniffing-Prozeduren ablehnen

In einigen Fällen meldet Internet Explorer einen anderen MIME-Typ als den vom Webserver angegebenen Typ. Wenn Internet Explorer beispielsweise HTML-Inhalt in einer Datei findet, die mit dem HTTP-Antwortheader Content-Type: Text/Plain geliefert wird, legt Internet Explorer fest, dass der Inhalt als HTML gerendert werden soll. Leider kann diese "MIME-Sniffing" auch zu Sicherheitsproblemen für Server führen, die nicht vertrauenswürdigen Inhalt hosten. Um dieses Problem zu beheben, hat Internet Explorer 8 eine Reihe von Änderungen am Code der MIME-Typbestimmung vorgenommen und ermöglicht Anwendungsentwicklern, die [MIME-Sniffing](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)zu abonnieren. Der folgende Code wurde der Datei " *Web. config* " hinzugefügt.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Bündelung und Minimierung aktivieren

Wenn Visual Studio ein neues Webprojekt erstellt, ist das bündeln und minimieren von JavaScript-Dateien standardmäßig nicht aktiviert. In BundleConfig.cs wurde eine Codezeile hinzugefügt:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Festlegen eines Ablauf Timeouts für Authentifizierungs Cookies

Standardmäßig laufen Authentifizierungs Cookies in zwei Wochen ab. Eine kürzere Zeit ist sicherer. Sie können diese Einstellung in *StartupAuth.cs*ändern:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Ausführen der APP aus Visual Studio auf dem lokalen Computer

Es gibt zwei Möglichkeiten, die Korrektur der IT-App auszuführen:

- Führen Sie die Basisanwendung aus, die neue Tasks direkt in die SQL-Datenbank schreibt.
- Ausführen der Anwendung mithilfe einer Warteschlange und eines Back-End-Dienstanbieter zum Erstellen von Aufgaben Das Warteschlangen Muster wird im Kapitel [Queue-zentriertes Arbeitsmuster](queue-centric-work-pattern.md)beschrieben.

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Ausführen der Basisanwendung

1. Installieren Sie [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Installieren Sie das [Azure SDK für .net für Visual Studio](https://azure.microsoft.com/downloads/).
3. Laden Sie die ZIP-Datei aus der [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)herunter.
4. Klicken Sie im Datei-Explorer mit der rechten Maustaste auf die ZIP-Datei, und klicken Sie auf Eigenschaften, und klicken Sie dann im Eigenschaftenfenster auf Sperre entsperren.
5. Entzippen Sie die Datei.
6. Doppelklicken Sie auf die SLN-Datei, um Visual Studio zu starten.
7. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**und dann auf Paket-Manager- **Konsole**.
8. Klicken Sie in der Paket-Manager-Konsole (PMC) auf Wiederherstellen.
9. Beenden Sie Visual Studio.
10. Starten Sie den [Azure-Speicher Emulator](/azure/storage/common/storage-use-emulator).
11. Starten Sie Visual Studio neu, und öffnen Sie die Projektmappendatei, die Sie im vorherigen Schritt geschlossen haben
12. Stellen Sie sicher, dass das Projekt "Projektname" als Startprojekt festgelegt ist, und drücken Sie dann STRG + F5, um das Projekt auszuführen.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Ausführen der Anwendung mit Warteschlangen Verarbeitung

1. Befolgen Sie die Anweisungen zum [Ausführen der Basisanwendung](#runbase), und schließen Sie dann den Browser, und schließen Sie Visual Studio.
2. Starten Sie Visual Studio mit Administratorrechten. (Sie verwenden den Azure-Server Emulator, für den Administratorrechte erforderlich sind.)
3. Ändern Sie in der Datei " *Web. config* " der Anwendung im *mymexit* -Projekt (Webprojekt) den Wert von `appSettings/UseQueues` in "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Wenn der [Azure-Speicher Emulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) noch nicht ausgeführt wird, starten Sie ihn erneut.
5. Führen Sie das Webprojekt und das Projekt myfixitcloudservice gleichzeitig aus.

    Verwenden von Visual Studio:

   1. Drücken Sie **F5** , um das Projekt "Projekt" auszuführen.
   2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt myfixitcloudservice, und klicken Sie dann auf **Debuggen** > **neue Instanz starten**.

    Verwenden von Visual Studio 2013 Express für Web:

   3. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Eigenschaften**aus.
   4. Wählen Sie **mehrere Start Projekte**aus.
   5. Wählen Sie in der Dropdown Liste **Aktion** unter myfxit und myfixitcloudservice die Option **Start**aus.
   6. Klicken Sie auf **OK**.
   7. Drücken Sie **F5**, um beide Projekte auszuführen.

      Wenn Sie das Projekt myfixitcloudservice ausführen, startet Visual Studio den Azure-Server Emulator. Abhängig von ihrer Firewallkonfiguration müssen Sie den Emulator möglicherweise durch die Firewall zulassen.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Bereitstellen der Basis-app zum Azure App Service von Web-Apps mithilfe der Windows PowerShell-Skripts

Zur Veranschaulichung des Musters " [Alles automatisieren](automate-everything.md) " wird die Korrektur der IT-App mit Skripts bereitgestellt, die eine Umgebung in Azure einrichten und das Projekt in der neuen Umgebung bereitstellen. In den folgenden Anweisungen wird erläutert, wie die Skripts verwendet werden.

Wenn Sie in Azure ohne Verwendung von Warteschlangen ausführen möchten und die Änderungen für die lokale Ausführung mit Warteschlangen vorgenommen haben, stellen Sie sicher, dass Sie den Wert für die usequeues-appSetting auf false zurücksetzen, bevor Sie mit den folgenden Anweisungen fortfahren.

Bei diesen Anweisungen wird vorausgesetzt, dass Sie die Lösung zum Beheben von Lösungen bereits heruntergeladen und ausgeführt haben und dass Sie über ein Azure-Konto verfügen oder über ein Azure-Abonnement verfügen, das Sie zur Verwaltung autorisiert haben.

1. Installieren Sie die **Azure PowerShell** -Konsole. Anweisungen hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Diese angepasste Konsole ist für die Zusammenarbeit mit Ihrem Azure-Abonnement konfiguriert. Das Azure-Modul ist im Verzeichnis " *Programme* " installiert und wird bei jeder Verwendung der Azure PowerShell Konsole automatisch importiert.

    Wenn Sie lieber in einem anderen Host Programm arbeiten möchten, z. b. Windows PowerShell ISE, verwenden Sie das [Import-Module-](https://go.microsoft.com/fwlink/?LinkID=141553) Cmdlet, um das Azure-Modul zu importieren, oder verwenden Sie einen Befehl im Azure-Modul, um den automatischen Import des Moduls zu initiieren.
2. Starten Sie Azure PowerShell mit der Option **als Administrator ausführen** .
3. Führen Sie das [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) -Cmdlet aus, um die Azure PowerShell Ausführungs Richtlinie auf `RemoteSigned`festzulegen. Geben Sie **Y** (für ja) ein, um die Richtlinien Änderung abzuschließen.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Diese Einstellung ermöglicht es Ihnen, lokale Skripts auszuführen, die nicht digital signiert sind. (Sie können die Ausführungs Richtlinie auch auf "`Unrestricted`" festlegen, wodurch der Blockierung-Schritt später nicht mehr erforderlich ist. aus Sicherheitsgründen wird dies jedoch nicht empfohlen.)
4. Führen Sie das `Add-AzureAccount`-Cmdlet aus, um PowerShell mit Anmelde Informationen für Ihr Konto einzurichten.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Diese Anmelde Informationen laufen nach einem bestimmten Zeitraum ab, und Sie müssen das Cmdlet "`Add-AzureAccount`" erneut ausführen. Da dieses e-book geschrieben wird, beträgt das Zeitlimit vor Ablauf der Anmelde Informationen 12 Stunden.
5. Wenn Sie über mehrere Abonnements verfügen, verwenden Sie das Cmdlet Select-azureabonnement, um das Abonnement anzugeben, in dem Sie die Testumgebung erstellen möchten.
6. Importieren Sie ein Verwaltungs Zertifikat für das gleiche Azure-Abonnement, indem Sie die `Get-AzurePublishSettingsFile`-und `Import-AzurePublishSettingsFile`-Cmdlets verwenden. Das erste dieser Cmdlets lädt eine Zertifikatsdatei herunter, und in der zweiten Datei geben Sie den Speicherort dieser Datei an, um Sie zu importieren. > [!IMPORTANT]
   > Behalten Sie die heruntergeladene Datei an einem sicheren Speicherort bei, oder löschen Sie Sie, wenn Sie fertig sind, da Sie ein Zertifikat enthält, das zum Verwalten Ihrer Azure-Dienste verwendet werden kann.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Das Zertifikat wird für einen Rest-API-Rückruf verwendet, der die IP-Adresse des Entwicklungs Computers erkennt, um eine Firewallregel auf dem SQL-Datenbankserver festzulegen.
7. Führen Sie das Cmdlet " [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) " (Aliase sind `cd`, `chdir`und `sl`) aus, um zu dem Verzeichnis zu navigieren, das die Skripts enthält. (Sie befinden sich im Ordner *Automation* im Ordner Lösung für die IT-Lösung.) Fügen Sie den Pfad in Anführungszeichen ein, wenn einer der Verzeichnisnamen Leerzeichen enthält. Um z. b. zum `c:\Sample Apps\FixIt\Automation` Verzeichnis zu navigieren, können Sie den folgenden Befehl eingeben:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Damit Windows PowerShell diese Skripts ausführen kann, verwenden Sie das [Unblock-File-](https://go.microsoft.com/fwlink/p/?linkid=294021) Cmdlet. (Die Skripts werden blockiert, da Sie aus dem Internet heruntergeladen wurden.)

    > [!WARNING]
    > Sicherheit: bevor Sie `Unblock-File` für Skripts oder ausführbare Dateien ausführen, öffnen Sie die Datei im Editor, untersuchen Sie die Befehle, und vergewissern Sie sich, dass Sie keinen schädlichen Code enthalten.

    Der folgende Befehl führt z. b. das `Unblock-File`-Cmdlet für alle Skripts im aktuellen Verzeichnis aus.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Um die Web-App für die Basis (keine Warteschlangen Verarbeitung) zu erstellen, führen Sie das Skript zur Erstellung der Umgebung aus.

    Der erforderliche `Name`-Parameter gibt den Namen der Datenbank an und wird auch für das Speicherkonto verwendet, das vom Skript erstellt wird. Der Name muss innerhalb der azurewebsites.net-Domäne global eindeutig sein. Wenn Sie einen Namen angeben, der nicht eindeutig ist, wie z. b. "fxit" oder "Test" (oder auch wie im Beispiel "fixitdemo"), schlägt das Cmdlet "`New-AzureWebsite`" mit einem internen Fehler fehl, der einen Konflikt meldet. Das Skript konvertiert den Namen in Kleinbuchstaben, um die namens Anforderungen für Web-Apps, Speicher Konten und Datenbanken zu erfüllen.

    Der erforderliche `SqlDatabasePassword` Parameter gibt das Kennwort für das Administrator Konto an, das für die SQL-Datenbank erstellt wird. Fügen Sie keine speziellen XML-Zeichen in das Kennwort ein (&amp; &lt; &gt;;). Dies ist eine Einschränkung der Art und Weise, in der Skripts geschrieben wurden, nicht die Einschränkung von Azure.

    Wenn Sie z. b. eine Web-App mit dem Namen "fixitdemo" erstellen und ein SQL Server Administrator Kennwort "Passw0rd1" verwenden möchten, können Sie den folgenden Befehl eingeben:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Der Name muss in der azurewebsites.net-Domäne eindeutig sein, und das Kennwort muss den Anforderungen der SQL-Datenbank hinsichtlich der Kenn Wort Komplexität entsprechen. (Das Beispiel Passw0rd1 erfüllt die Anforderungen.)

    Beachten Sie, dass der-Befehl mit "beginnt.\". Um die böswillige Ausführung von Skripts zu verhindern, erfordert Windows PowerShell, dass Sie den voll qualifizierten Pfad zur Skriptdatei angeben, wenn Sie ein Skript ausführen. Mit einem Punkt können Sie das aktuelle Verzeichnis angeben (".\") oder den voll qualifizierten Pfad bereitstellen, z. b.:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Weitere Informationen zum Skript finden Sie unter dem-Cmdlet "`Get-Help`".

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Sie können die Parameter "`Detailed`", "`Full`", "`Parameters`" und "`Examples`" des Cmdlets "Get-Help" verwenden, um die zurückgegebene Hilfe zu filtern.

    Wenn das Skript fehlschlägt oder Fehler generiert, z. b. "New-azurewebsite: Set-azureabonnement und SELECT-azureabonnement First", ist die Konfiguration von Azure PowerShell möglicherweise nicht abgeschlossen.

    Nachdem das Skript abgeschlossen wurde, können Sie das Azure-Verwaltungsportal verwenden, um die erstellten Ressourcen anzuzeigen, wie im Kapitel [Automatisieren von allem](automate-everything.md) gezeigt.
10. Verwenden Sie das Skript *azurewebsite. ps1* , um das Projekt in der neuen Azure-Umgebung bereitzustellen. Beispiel:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Wenn die Bereitstellung abgeschlossen ist, wird der Browser geöffnet, und der Browser wird in Azure ausgeführt.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Problembehandlung bei Windows PowerShell-Skripts

Die häufigsten Fehler, die beim Ausführen dieser Skripts aufgetreten sind, beziehen sich auf Berechtigungen. Stellen Sie sicher, dass `Add-AzureAccount` und `Import-AzurePublishSettingsFile` erfolgreich waren und dass Sie Sie für dasselbe Azure-Abonnement verwendet haben. Auch wenn `Add-AzureAccount` erfolgreich war, müssen Sie ihn möglicherweise erneut ausführen. Die von `Add-AzureAccount` hinzugefügten Berechtigungen laufen in 12 Stunden ab.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Der Objekt Verweis ist nicht auf eine Instanz eines Objekts festgelegt.

Wenn das Skript Fehler zurückgibt, wie z. b. "Objekt Verweis ist nicht auf eine Instanz eines Objekts festgelegt", was bedeutet, dass Windows PowerShell kein zu verarbeitende Objekt finden kann (Dies ist eine NULL-Verweis Ausnahme), führen Sie das `Add-AzureAccount` Cmdlet aus, und versuchen Sie es erneut.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: auf dem Server ist ein interner Fehler aufgetreten.

Das `New-AzureWebsite`-Cmdlet gibt einen internen Fehler zurück, wenn der Name in der azurewebsites.net-Domäne nicht eindeutig ist. Um den Fehler zu beheben, verwenden Sie einen anderen Wert für den Namen, der im Name-Parameter von *New-AzureWebsiteEnv. ps1*vorliegt.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Neustarten des Skripts

Wenn Sie das Skript *New-AzureWebsiteEnv. ps1* neu starten müssen, weil es vor dem Drucken der Meldung "Skript ist fertiggestellt" fehlgeschlagen ist, können Sie Ressourcen löschen, die das Skript erstellt hat, bevor es angehalten wurde. Wenn das Skript z. b. die Web-App condesofixitdemo bereits erstellt hat und Sie das Skript erneut mit dem gleichen Namen ausführen, schlägt das Skript fehl, da der Name bereits verwendet wird.

Verwenden Sie die folgenden Cmdlets, um zu bestimmen, welche Ressourcen vom Skript erstellt wurden, bevor es angehalten wurde:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: um dieses Cmdlet auszuführen, übergeben Sie den Namen des Datenbankservers an `Get-AzureSqlDatabase`: `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Verwenden Sie die folgenden Befehle, um diese Ressourcen zu löschen. Beachten Sie, dass beim Löschen des Datenbankservers automatisch die dem Server zugeordneten Datenbanken gelöscht werden.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Bereitstellen der APP mit Warteschlangen Verarbeitung zum Azure App Service von Web-Apps und einem Azure-clouddienst

Nehmen Sie die folgende Änderung in der Datei "myfixit\web.config" vor, um Warteschlangen zu aktivieren. Ändern Sie unter `appSettings`den Wert `UseQueues` in "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Stellen Sie die MVC-Anwendung dann wie [zuvor](#deploybase)beschrieben für eine Web-App in Azure App Service bereit.

Erstellen Sie als nächstes einen neuen Azure-clouddienst. Die Skripts, die in der Fix-it-App enthalten sind, erstellen oder stellen den clouddienst nicht bereit, sodass Sie Azure-Portal hierfür verwenden müssen. Klicken Sie im Portal auf **neu** -- **Compute** – **clouddienst** -- **schneller**Fassung, und geben Sie dann eine URL und einen Speicherort für den Daten Center ein. Verwenden Sie das gleiche Rechenzentrum, in dem Sie die Web-App bereitgestellt haben.

![](the-fix-it-sample-application/_static/image1.png)

Bevor Sie den clouddienst bereitstellen können, müssen Sie einige der Konfigurationsdateien aktualisieren.

Ersetzen Sie in myfxit. workerrole\app.config unter `connectionStrings`den Wert der Verbindungs Zeichenfolge `appdb` durch die tatsächliche Verbindungs Zeichenfolge für die SQL-Datenbank. Sie können die Verbindungs Zeichenfolge aus dem Portal erhalten. Klicken Sie im-Portal auf **SQL-Datenbanken** - **appdb** , - **SQL-Datenbank-Verbindungs Zeichenfolgen für ADO .net, ODBC, PHP und JDBC anzuzeigen**. Kopieren Sie die ADO.NET-Verbindungs Zeichenfolge, und fügen Sie den Wert in die Datei app. config ein. Ersetzen Sie "{Your\_Password\_here}" durch ihr Daten Bank Kennwort. (Angenommen, Sie haben die Skripts verwendet, um die MVC-App bereitzustellen, haben Sie im `SqlDatabasePassword` script-Parameter das Daten Bank Kennwort angegeben.)

Das Ergebnis sollte wie folgt aussehen:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Ersetzen Sie in der gleichen Datei myfxit. workerrole\app.config unter `appSettings`die beiden Platzhalter Werte für das Azure-Speicherkonto.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Sie können den Zugriffsschlüssel aus dem Portal abrufen. Weitere Informationen finden [Sie unter Verwalten von Speicher Konten](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

Ersetzen Sie in MyFixItCloudService\ServiceConfiguration.Cloud.cscfg die gleichen beiden Platzhalter Werte für das Azure-Speicherkonto.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Nun können Sie den clouddienst bereitstellen. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt myfixitcloudservice, und wählen Sie **veröffentlichen** Weitere Informationen finden Sie unter "bereitstellen[der Anwendung in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)" in Teil 2 [dieses Tutorials](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Previous](more-patterns-and-guidance.md)
