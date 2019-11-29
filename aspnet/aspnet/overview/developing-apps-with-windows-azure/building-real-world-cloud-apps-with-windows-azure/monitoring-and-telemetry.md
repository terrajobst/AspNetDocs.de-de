---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Überwachung und Telemetrie (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Das e-Book zur Entwicklung realer Cloud-apps mit Azure basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Vorgehensweisen erläutert, für die er...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: 44941c9fd0dcd3223604fc4a4f2836f587578acb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585613"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Überwachung und Telemetrie (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Verfahren erläutert, die Ihnen bei der Entwicklung von Web-Apps für die Cloud helfen können. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Viele Benutzer verlassen sich auf Kunden, um Sie zu informieren, wenn Ihre Anwendung herunterläuft. Das ist nicht wirklich eine bewährte Vorgehensweise, insbesondere nicht in der Cloud. Es gibt keine Garantie für eine schnelle Benachrichtigung, und wenn Sie benachrichtigt werden, erhalten Sie häufig minimale oder irreführende Daten über das, was passiert ist. Mit guten Telemetrie-und Protokollierungs Systemen können Sie wissen, was mit der APP passiert, und wenn etwas schief geht, können Sie sofort sofort herausfinden und nützliche Informationen zur Problembehandlung nutzen.

## <a name="buy-or-rent-a-telemetry-solution"></a>Erwerben oder vermieten einer telemetrielösung

> [!NOTE]
> Dieser Artikel wurde vor der Veröffentlichung von [Application Insights](/azure/application-insights/app-insights-overview) geschrieben. Application Insights ist der bevorzugte Ansatz für Telemetrielösungen in Azure. Weitere Informationen finden [Sie unter Einrichten von Application Insights für Ihre ASP.NET-Website](/azure/application-insights/app-insights-asp-net) .

Eine der Vorteile der cloudumgebung ist, dass es wirklich einfach ist, ihren Weg zu gewinnen. Die Telemetrie ist ein Beispiel. Ohne großen Aufwand können Sie sehr kostengünstig ein sehr gutes Telemetriesystem einrichten und ausführen. Es gibt eine Reihe von tollen Partnern, die in Azure integriert werden können, und einige davon verfügen über kostenlose Tarife – so können Sie grundlegende Telemetriedaten erhalten. Hier sind nur einige der derzeit in Azure verfügbaren:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [DynaTrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) umfasst auch Überwachungs Features.

Wir werden uns schnell mit dem Einrichten von New Relic beschäftigen, um zu zeigen, wie einfach es ist, ein Telemetriesystem zu verwenden.

Melden Sie sich im Azure-Verwaltungs Portal für den Dienst an. Klicken Sie auf **neu**, und klicken Sie dann auf **Speichern**. Das Dialogfeld **Add-on auswählen** wird angezeigt. Scrollen Sie nach unten, und klicken Sie auf **New Relic**.

![Add-On auswählen](monitoring-and-telemetry/_static/image1.png)

Klicken Sie auf den Pfeil nach rechts, und wählen Sie die gewünschte Dienst Ebene aus. Für diese Demo verwenden wir den Free-Tarif.

![Add-on personalisieren](monitoring-and-telemetry/_static/image2.png)

Klicken Sie auf den Pfeil nach rechts, bestätigen Sie den "Einkauf", und New Relic wird jetzt im Portal als Add-on angezeigt.

![Kauf überprüfen](monitoring-and-telemetry/_static/image3.png)

![New Relic-Add-on im Verwaltungs Portal](monitoring-and-telemetry/_static/image4.png)

Klicken Sie auf **Verbindungsinformationen**, und kopieren Sie den Lizenzschlüssel.

![Verbindungsinformationen](monitoring-and-telemetry/_static/image5.png)

Wechseln Sie im Portal zur Registerkarte **Konfigurieren** für Ihre Web-App, legen Sie die **Leistungsüberwachung** auf **Add-on**fest, und legen Sie die Dropdown Liste **Add-on auswählen** auf **New Relic**fest. Klicken Sie dann auf **Speichern**.

![New Relic auf Registerkarte "Konfigurieren"](monitoring-and-telemetry/_static/image6.png)

Installieren Sie in Visual Studio das New Relic-nuget-Paket in Ihrer APP.

![Entwickler Analyse auf der Registerkarte Konfigurieren](monitoring-and-telemetry/_static/image7.png)

Stellen Sie die app in Azure bereit, und beginnen Sie mit der Verwendung. Erstellen Sie einige feste IT-Aufgaben, um einige Aktivitäten für das Überwachen von New Relic bereitzustellen.

Wechseln Sie dann zurück zur Seite **New Relic** auf der Registerkarte **Add-ons** des Portals, und klicken Sie auf **Verwalten**. Das Portal sendet Sie über Single Sign-on für die Authentifizierung an das New Relic-Verwaltungs Portal, sodass Sie Ihre Anmelde Informationen nicht erneut eingeben müssen. Die Übersichtsseite enthält eine Reihe von Leistungsstatistiken. (Klicken Sie auf das Bild, um die vollständige Übersichtsseite anzuzeigen.)

[Registerkarte "![New Relic Monitoring"](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Hier sind nur einige der Statistiken aufgeführt, die Sie sehen können:

- Die durchschnittliche Antwortzeit zu unterschiedlichen Tageszeiten.

    ![Antwortzeit](monitoring-and-telemetry/_static/image10.png)
- Durchsatzraten (in Anforderungen pro Minute) zu unterschiedlichen Tageszeiten.

    ![Durchsatz](monitoring-and-telemetry/_static/image11.png)
- Die CPU-Zeit des Servers für die Behandlung unterschiedlicher HTTP-Anforderungen

    ![Webtransaktions Zeiten](monitoring-and-telemetry/_static/image12.png)
- Die CPU-Zeit, die in verschiedenen Teilen des Anwendungs Codes aufgewendet wurde:

    ![Ablauf Verfolgungs Details](monitoring-and-telemetry/_static/image13.png)
- Historische Leistungsstatistik.

    ![Leistungs Verlaufs Daten](monitoring-and-telemetry/_static/image14.png)
- Aufrufe externer Dienste, z. b. des BLOB-Diensts, und Statistiken darüber, wie zuverlässig und reaktionsfähig der Dienst war.

    ![Externe Dienste](monitoring-and-telemetry/_static/image15.png)

    ![Externe Dienste](monitoring-and-telemetry/_static/image16.png)

    ![Externer Dienst](monitoring-and-telemetry/_static/image17.png)
- Informationen dazu, wo weltweit oder an welchem Ort im US-Web-App-Datenverkehr stammt.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Sie können auch Berichte und Ereignisse einrichten. Beispielsweise können Sie jedes Mal, wenn Sie Fehler sehen, eine e-Mail an das Problem senden.

![Berichte](monitoring-and-telemetry/_static/image19.png)

New Relic ist nur ein Beispiel für ein Telemetriesystem. Sie können all dies auch von anderen Diensten erhalten. Die Schönheit der Cloud besteht darin, dass Sie, ohne Code schreiben zu müssen, plötzlich viel mehr Informationen darüber erhalten können, wie Ihre Anwendung verwendet wird und was Ihre Kunden tatsächlich erleben.

<a id="log"></a>
## <a name="log-for-insight"></a>Für Einblicke protokollieren

Ein telemetriepaket ist ein guter erster Schritt, aber Sie müssen immer noch eigenen Code instrumentieren. Der telemetriedienst informiert Sie, wenn ein Problem vorliegt, und gibt Aufschluss darüber, was die Kunden haben. Sie erhalten jedoch möglicherweise keinen großen Einblick in das, was in Ihrem Code passiert.

Sie möchten nicht auf einen Produktionsserver über eine Remote Verbindung verfügen, um zu sehen, was Ihre APP tut. Das ist möglicherweise praktikabel, wenn Sie einen Server haben, aber was ist, wenn Sie auf Hunderte von Servern skaliert haben, und Sie wissen nicht, für welche Elemente Sie eine Remote Verbindung benötigen? Ihre Protokollierung sollte genügend Informationen bereitstellen, die Sie niemals zum Analysieren und Debuggen von Problemen auf Produktionsserver übermitteln müssen. Sie sollten genügend Informationen protokollieren, damit Sie Probleme nur über die Protokolle isolieren können.

### <a name="log-in-production"></a>In Produktion protokollieren

Viele Benutzer aktivieren die Ablauf Verfolgung nur dann in der Produktion, wenn ein Problem vorliegt und Sie debuggen möchten. Dies kann zu einer erheblichen Verzögerung zwischen dem Zeitpunkt, zu dem ein Problem auftritt, und dem Zeitpunkt, zu dem Sie nützliche Informationen zur Problembehandlung erhalten, führen. Die Informationen, die Sie erhalten, sind bei zeitweiligen Fehlern möglicherweise nicht hilfreich.

Wir empfehlen in der cloudumgebung, in der Speicherkosten günstig sind, dass Sie die Anmeldung immer in der Produktionsumgebung belassen. Auf diese Weise können Sie bei Fehlern bereits protokolliert werden, und Sie verfügen über Verlaufs Daten, die Ihnen helfen können, Probleme zu analysieren, die im Laufe der Zeit oder regelmäßig zu unterschiedlichen Zeiten auftreten. Sie könnten einen Löschvorgang automatisieren, um alte Protokolle zu löschen, aber Sie können feststellen, dass es kostengünstiger ist, einen solchen Prozess einzurichten, als die Protokolle beizubehalten.

Die zusätzlichen Kosten für die Protokollierung sind im Vergleich zum Zeitaufwand für die Problembehandlung trivial, und Sie können sparen, indem Sie alle Informationen, die Sie benötigen, wenn etwas schief geht, benötigen. Wenn jemand Ihnen mitteilt, dass ein zufälliger Fehler ungefähr etwa 8:00 in der letzten Nacht aufgetreten ist, Sie sich aber nicht an den Fehler erinnern, können Sie leicht feststellen, worum es sich bei dem Problem handelt.

Für weniger als $4 einen Monat können Sie 50 Gigabyte an Protokollen aufbewahren, und die Auswirkungen der Protokollierung auf die Leistung sind trivial, wenn Sie einen Blick behalten, um Leistungsengpässe zu vermeiden, stellen Sie sicher, dass die Protokollierungs Bibliothek asynchron ist.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Unterscheiden von Protokollen, die von Protokollen informieren, die eine Aktion erfordern

Protokolle sind dazu gedacht (ich möchte etwas wissen) oder Act (ich möchte etwas tun). Achten Sie darauf, Act-Protokolle nur für Probleme zu schreiben, die für eine Person oder einen automatisierten Prozess erforderlich sind, um Maßnahmen zu ergreifen. Zu viele Act-Protokolle erzeugen Rauschen, sodass zu viele Aufgaben durchlaufen werden müssen, um echte Probleme zu finden. Wenn Ihre Act-Protokolle automatisch Aktionen auslösen, wie z. b. das Senden von e-Mails an Supportmitarbeiter, sollten Sie vermeiden, dass Tausende solcher Aktionen durch ein einzelnes Problem ausgelöst werden.

In der .NET System. Diagnostics-Ablauf Verfolgung können den Protokollen Fehler, Warnung, Info und Debuggen/ausführliche Ebene zugewiesen werden. Sie können den Act von den Protokollen informieren, indem Sie die Fehlerstufe für Act-Protokolle reservieren und die niedrigeren Ebenen für das informieren der Protokolle verwenden.

![Protokolliergrade](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurieren von Protokollierungs Ebenen zur Laufzeit

Obwohl die Protokollierung immer in der Produktionsumgebung durchgeführt werden sollte, besteht eine bewährte Vorgehensweise in der Implementierung eines Protokollierungs Frameworks, mit dem Sie zur Laufzeit die Detailstufe anpassen können, die Sie protokollieren, ohne die Anwendung erneut bereitstellen oder neu starten zu müssen. Wenn Sie z. b. die Ablauf Verfolgungs Funktion in `System.Diagnostics` verwenden, können Sie Fehler-, Warnungs-, Info-und Debug-/Verbose-Protokolle erstellen. Es wird empfohlen, dass Sie immer Fehler-, Warnungs-und Informations Protokolle in der Produktionsumgebung protokollieren, und Sie sollten in der Lage sein, die Debug-/ausführungsprotokollierung für die Problembehandlung von Fall zu Fall dynamisch hinzuzufügen.

Web-Apps in Azure App Service verfügen über integrierte Unterstützung für das Schreiben von `System.Diagnostics` Protokollen in das Dateisystem, den Tabellen Speicher oder den BLOB-Speicher. Sie können für jedes Speicher Ziel verschiedene Protokollierungs Stufen auswählen, und Sie können den Protokolliergrad dynamisch ändern, ohne die Anwendung neu starten zu müssen. Die BLOB Storage-Unterstützung vereinfacht die Ausführung von [hdinsight](https://docs.microsoft.com/azure/hdinsight/) -Analyse Aufträgen in ihren Anwendungs Protokollen, da hdinsight weiß, wie BLOB Storage direkt verwendet werden kann.

### <a name="log-exceptions"></a>Protokollieren von Ausnahmen

Stellen Sie nicht nur eine *Ausnahme dar. "Destring ()* " im Protokollierungs Code. , Der Kontextinformationen verlässt. Im Fall von SQL-Fehlern wird die SQL-Fehlernummer weggelassen. Fügen Sie für alle Ausnahmen Kontextinformationen, die Ausnahme selbst und die inneren Ausnahmen ein, um sicherzustellen, dass Sie alles bereitstellen, was für die Problembehandlung erforderlich ist. Kontextinformationen können z. b. den Servernamen, einen Transaktions Bezeichner und einen Benutzernamen enthalten (jedoch nicht das Kennwort oder Geheimnisse).

Wenn Sie von jedem Entwickler abhängig sind, um mit der Ausnahme Protokollierung zurecht zu kommen, werden einige davon nicht mehr unterstützen. Um sicherzustellen, dass Sie jedes Mal ordnungsgemäß ausgeführt wird, erstellen Sie die Ausnahmebehandlung direkt in die Protokollierungs Schnittstelle: übergeben Sie das Ausnahme Objekt selbst an die Logger-Klasse, und protokollieren Sie die Ausnahme Daten ordnungsgemäß in der Logger-Klasse.

### <a name="log-calls-to-services"></a>Protokoll Aufrufe an Dienste

Es wird dringend empfohlen, jedes Mal, wenn Ihre APP einen Dienst aufruft, ein Protokoll zu schreiben, und zwar unabhängig davon, ob es sich um eine Datenbank, eine Rest-API oder einen externen Dienst handelt. Fügen Sie in Ihre Protokolle nicht nur einen Hinweis auf Erfolg oder Fehler, sondern die Dauer der einzelnen Anforderungen ein. In der cloudumgebung werden häufig Probleme im Zusammenhang mit langsamen Ausfällen und nicht den Ausfallzeiten angezeigt. Etwas, das normalerweise 10 Millisekunden benötigt, kann plötzlich eine Sekunde in Anspruch nehmen. Wenn jemand sagt, dass Ihre APP langsam ist, sollten Sie sich den neuen Relic oder den von Ihnen vorhandenen telemetriedienst ansehen und seine Erfahrungen überprüfen. Anschließend können Sie Ihre eigenen Protokolle anzeigen, um die Details zu den Gründen der langsamen Betrachtung anzuzeigen.

### <a name="use-an-ilogger-interface"></a>ILogger-Schnittstelle verwenden

Beim Erstellen einer Produktionsanwendung empfiehlt es sich, eine einfache *ILogger* -Schnittstelle zu erstellen und einige Methoden darin zu halten. Dies vereinfacht das spätere Ändern der Protokollierungs Implementierung und muss nicht den gesamten Code durchlaufen. Wir könnten die `System.Diagnostics.Trace`-Klasse in der gesamten Lösung für die IT-Lösung verwenden, aber stattdessen verwenden wir Sie in den Bereichen einer Protokollierungs Klasse, die *ILogger*implementiert, und wir machen *ILogger* -Methodenaufrufe in der gesamten app.

Auf diese Weise können Sie [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) durch den gewünschten Protokollierungs Mechanismus ersetzen, wenn Sie die Protokollierung noch reicher gestalten möchten. Wenn Ihre APP beispielsweise wächst, können Sie entscheiden, dass Sie ein umfassenderes Protokollierungs Paket verwenden möchten, z. b. [nlog](http://nlog-project.org/) oder den [Anwendungs Block für die Unternehmens Bibliotheks Protokollierung](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4net](http://logging.apache.org/log4net/) ist ein weiteres beliebtes Protokollierungs Framework, das jedoch keine asynchrone Protokollierung durchführt.)

Ein möglicher Grund für die Verwendung eines Frameworks, z. b. nlog, besteht darin, die Aufteilung der Protokollierungs Ausgabe in separate Datenspeicher mit hohem Volumen und hohem Wert zu unterteilen. So können Sie große Mengen von Informationsdaten, die Sie nicht benötigen, um schnelle Abfragen auszuführen, effizient speichern und gleichzeitig den schnellen Zugriff auf Act-Daten aufrechterhalten.

### <a name="semantic-logging"></a>Semantische Protokollierung

Eine relativ neue Methode zum Durchführen einer Protokollierung, die nützlichere Diagnoseinformationen verursachen kann, finden Sie [unter Enterprise Library Semantic Logging Application Block (Platte)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). In der-Schnittstelle werden die [Ereignis Ablauf Verfolgung für Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) und die [eventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) -Unterstützung in .NET 4,5 verwendet Sie definieren für jede Art von Ereignis, das Sie protokollieren, eine andere Methode, sodass Sie die von Ihnen geschriebenen Informationen anpassen können. Um beispielsweise einen SQL-Datenbankfehler zu protokollieren, können Sie eine `LogSQLDatabaseError`-Methode aufzurufen. Bei dieser Art von Ausnahme wissen Sie, dass es sich bei der Fehlernummer um ein wichtiges Element handelt, sodass Sie in der Methoden Signatur einen Parameter für die Fehlernummer einschließen und die Fehlernummer als separates Feld im Protokolldaten Satz aufzeichnen können, den Sie schreiben. Da sich die Zahl in einem separaten Feld befindet, können Sie auf einfachere und zuverlässigere Weise Berichte auf der Grundlage von SQL-Fehlernummern erhalten, als wenn Sie die Fehlernummer nur in einer Meldungs Zeichenfolge verkettet haben.

## <a name="logging-in-the-fix-it-app"></a>Protokollierung in der Fix-App

### <a name="the-ilogger-interface"></a>Die ILogger-Schnittstelle

Hier ist die *ILogger* -Schnittstelle in der Lösung zum Beheben von IT-apps.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Mit diesen Methoden können Sie Protokolle auf denselben vier Ebenen schreiben, die von *System. Diagnostics*unterstützt werden. Die traceapi-Methoden sind für die Protokollierung externer Dienst Aufrufe mit Informationen zur Latenz. Sie können auch einen Satz von Methoden für die Debug-/Verbose-Ebene hinzufügen.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Die Protokollierungs Implementierung der ILogger-Schnittstelle

Die Implementierung der-Schnittstelle ist sehr einfach. Im Grunde werden nur die standardmäßigen *System. Diagnostics* -Methoden aufgerufen. Der folgende Code Ausschnitt zeigt alle drei Informationsmethoden und eine der anderen.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Aufrufen der ILogger-Methoden

Jedes Mal, wenn der Code in der Korrektur-App eine Ausnahme abfängt, wird eine *ILogger* -Methode aufgerufen, um die Ausnahme Details zu protokollieren. Und jedes Mal, wenn ein Aufruf an die Datenbank, den BLOB-Dienst oder eine Rest-API erfolgt, wird vor dem Aufruf eine Stoppuhr gestartet, die Stoppuhr wird beendet, wenn der Dienst zurückkehrt, und die verstrichene Zeit wird zusammen mit Informationen über Erfolg oder Fehler protokolliert.

Beachten Sie, dass die Protokollmeldung den Klassennamen und den Methodennamen enthält. Es wird empfohlen, sicherzustellen, dass Protokollmeldungen ermitteln, welcher Teil des Anwendungs Codes Sie geschrieben hat.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Jetzt können Sie den Aufruf, die Methode, die ihn aufgerufen hat, und genau wie viel Zeit für die Korrektur der IT-App für SQL-Datenbank anzeigen.

![SQL-Datenbankabfrage in Protokollen](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Wenn Sie die Protokolle durchsuchen, sehen Sie, dass die Zeit, die Daten Bank Aufrufe annehmen, eine Variable ist. Diese Informationen können nützlich sein: da sich die APP all dies anmeldet, können Sie historische Trends in Bezug auf die Leistung des Daten Bank Dienstanbieter mit der Zeit analysieren. Ein Dienst kann beispielsweise in den meisten Fällen schnell ausfallen, aber Anforderungen können fehlschlagen, oder die Antworten können zu bestimmten Tageszeiten verlangsamt werden.

Sie können das gleiche für den BLOB-Dienst ausführen – für jedes Mal, wenn die APP eine neue Datei hochlädt, gibt es ein Protokoll, und Sie können genau sehen, wie lange es gedauert hat, bis die einzelnen Dateien hochgeladen wurden.

![BLOB-uploadprotokoll](monitoring-and-telemetry/_static/image23.png)

Es ist nur ein paar zusätzliche Codezeilen, die jedes Mal geschrieben werden müssen, wenn Sie einen Dienst anrufen. wenn jemand sagt, dass ein Problem aufgetreten ist, wissen Sie genau, ob es sich um einen Fehler handelt, oder ob es sich um einen Fehler handelt. Sie können die Ursache des Problems ermitteln, ohne eine Remote Verbindung zu einem Server herzustellen oder die Protokollierung zu aktivieren, nachdem der Fehler aufgetreten ist, und Sie darauf hoffen, ihn neu zu erstellen.

## <a name="dependency-injection-in-the-fix-it-app"></a>Abhängigkeitsinjektion in der Fix-it-App

Sie Fragen sich vielleicht, wie der Repository-Konstruktor im obigen Beispiel die Implementierung der Logger-Schnittstelle abruft:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Um die Schnittstelle mit der Implementierung zu verbinden, verwendet die APP die [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection)(di) mit [autofac](http://autofac.org/). DI ermöglicht die Verwendung eines Objekts, das auf einer Schnittstelle an vielen Stellen im gesamten Code basiert und nur an einer Stelle die Implementierung angeben muss, die bei der instanziierten Schnittstelle verwendet wird. Dies vereinfacht das Ändern der Implementierung: beispielsweise möchten Sie möglicherweise die System. Diagnostics-Protokollierung durch eine nlog-Protokollierung ersetzen. Sie können auch für automatisierte Tests eine simulierte Version der Protokollierung ersetzen.

Die Korrektur der IT-Anwendung verwendet di in allen Depots und allen Controllern. Die Konstruktoren der Controller Klassen erhalten eine *itaskrepository* -Schnittstelle auf die gleiche Weise wie das Repository eine Protokollierungs Schnittstelle erhält:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Die APP verwendet die autofac di-Bibliothek zum automatischen Bereitstellen von *taskrepository* -und *Logger* -Instanzen für diese Konstruktoren.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Dieser Code besagt im Grunde, dass ein Konstruktor eine *ILogger* -Schnittstelle benötigt, eine Instanz der *Logger* -Klasse übergibt und immer dann, wenn er eine *ifixttaskrepository* -Schnittstelle benötigt, eine Instanz der *fixttaskrepository* -Klasse übergibt.

[Autofac](http://autofac.org/) ist eines von vielen Framework für die Abhängigkeitsinjektion, das Sie verwenden können. Ein weiterer beliebter Wert ist [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), der von Microsoft Patterns and Practices empfohlen und unterstützt wird.

## <a name="built-in-logging-support-in-azure"></a>Integrierte Protokollierungs Unterstützung in Azure

Azure unterstützt die folgenden Arten der [Protokollierung für Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System. Diagnostics-Ablauf Verfolgung (Sie können die Ebenen dynamisch aktivieren und deaktivieren, ohne den Standort neu zu starten).
- Windows-Ereignisse.
- IIS-Protokolle (http/FREB).

Azure unterstützt die folgenden Arten der [Protokollierung in Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System. Diagnostics-Ablauf Verfolgung.
- Leistungsindikatoren.
- Windows-Ereignisse.
- IIS-Protokolle (http/FREB).
- Benutzerdefinierte Verzeichnis Überwachung.

Die Korrektur der IT-App verwendet die System. Diagnostics-Ablauf Verfolgung. Zum Aktivieren der System. Diagnostics-Protokollierung in einer Web-App müssen Sie lediglich einen Switch im Portal kippen oder die Rest-API aufzurufen. Klicken Sie im Portal auf die Registerkarte **Konfiguration** für Ihre Website, und Scrollen Sie nach unten, um den **Anwendungsdiagnose** Abschnitt anzuzeigen. Sie können die Protokollierung aktivieren oder deaktivieren und den gewünschten Protokolliergrad auswählen. Azure kann die Protokolle in das Dateisystem oder in ein Speicherkonto schreiben.

![Anwendungs Diagnose und Website Diagnose auf der Registerkarte "Konfigurieren"](monitoring-and-telemetry/_static/image24.png)

Nachdem Sie die Protokollierung in Azure aktiviert haben, können Sie Protokolle im Visual Studio-Ausgabefenster sehen, während diese erstellt werden.

![Menü für Streamingprotokolle](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menü für Streamingprotokolle](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Sie können auch Protokolle in Ihr Speicherkonto schreiben und Sie mit jedem Tool anzeigen, das auf den Azure Storage Tabellen Dienst zugreifen kann, z. b. **Server-Explorer** in Visual Studio oder [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Protokolle in Server-Explorer](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Summary

Es ist ganz einfach, ein vorkonfigurierte Telemetriesystem zu implementieren, die Protokollierung in Ihrem eigenen Code zu instrumentieren und die Protokollierung in Azure zu konfigurieren. Wenn Probleme mit der Produktion auftreten, hilft Ihnen die Kombination aus einem Telemetriesystem und benutzerdefinierten Protokollen dabei, Probleme schnell zu beheben, bevor Sie zu größeren Problemen für Ihre Kunden werden.

Im [nächsten Kapitel](transient-fault-handling.md) wird erläutert, wie vorübergehende Fehler behandelt werden, damit Sie nicht zu Produktionsproblemen werden, die Sie untersuchen müssen.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Dokumentation hauptsächlich zu Telemetriedaten:

- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Weitere Informationen finden Sie unter Instrumentation und Telemetrie-Leitfaden, Leitfaden zur Dienst Messung, Überwachungs Muster für den Integritäts Endpunkt und Lauf Zeit Neukonfiguration.
- [Penny Pinching in der Cloud: Aktivierung der New Relic-Leistungsüberwachung auf Azure-Websites](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael thomassy. Weitere Informationen finden Sie im Abschnitt Telemetrie und Diagnose.
- [Entwicklung der nächsten Generation mit Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). MSDN Magazine-Artikel.

Dokumentation hauptsächlich zur Protokollierung:

- [Anwendungs Block für semantische Protokollierung (Platte)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie zeigt den Fall für die semantische Protokollierung mit Platte.
- [Erstellen strukturierter und sinnvoller Protokolle mit semantischer Protokollierung](https://channel9.msdn.com/Events/Build/2013/3-336). Video Julianischer Dominguez zeigt den Fall für die semantische Protokollierung mit Platte.
- [EF6 SQL-Protokollierung – Teil 1: einfache Protokollierung](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers zeigt, wie Sie Abfragen protokollieren, die von Entity Framework in EF 6 ausgeführt werden.
- [Verbindungsresilienz und Befehls Abfang Funktion mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Fourth in einer neun teiligen tutorialreihe zeigt, wie Sie mit dem Feature "EF 6 Command intercepsting" SQL-Befehle protokollieren, die von Entity Framework an die Datenbank gesendet werden.
- [Verbessern der C# Protokollierung mit 5,0](http://www.dotnetcurry.com/showarticle.aspx?ID=972)-Aufrufer Es wird erläutert, wie Sie den Namen der aufrufenden Methode problemlos protokollieren können, ohne Sie in Literale hart codieren oder mithilfe von Reflektion manuell abrufen zu müssen.

Dokumentation hauptsächlich zur Problembehandlung:

- [Azure-Problembehandlung &amp; debuggingblog](https://blogs.msdn.com/b/kwill/)
- [Azuretools – das vom Azure Developer Support-Team verwendete Diagnose Hilfsprogramm](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Führt einen Download Link für ein Tool ein, das auf einem virtuellen Azure-Computer verwendet werden kann, um eine Vielzahl von Diagnose-und Überwachungstools herunterzuladen und auszuführen. Nützlich, wenn Sie ein Problem auf einem bestimmten virtuellen Computer diagnostizieren müssen.
- Behandeln von [Problemen mit einer Web-App in Azure App Service mithilfe von Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Ein schrittweises Tutorial für die ersten Schritte mit der System. Diagnostics-Ablauf Verfolgung und dem Remote Debuggen.

Videos:

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Serie von Ulrich Homann, Marc Mercuri und Mark Simms. Stellt allgemeine Konzepte und architektonische Prinzipien in einer sehr zugänglichen und interessanten Weise vor, wobei Storys von der Benutzerfreundlichkeit von Microsoft Customer Advisory Team (CAT) mit tatsächlichen Kunden erstellt werden. In den Vorfällen 4 und 9 geht es um Überwachung und Telemetrie. Episode 9 enthält eine Übersicht über die Überwachungsdienste "metricshub", "appdynamics", "New Relic" und "PagerDuty".
- [Building Big: Erkenntnisse von Azure-Kunden (Teil II](https://channel9.msdn.com/Events/Build/2012/3-030)). Mark Simms spricht über das Entwerfen von Fehlern und Instrumentieren von allem. Vergleichbar mit der Failsafe-Reihe, wird jedoch ausführlicher erläutert.

Code Beispiel:

- [Grundlagen des clouddiensts in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung, die vom Microsoft Azure Customer Advisory Team erstellt wurde. Veranschaulicht die Telemetrie-und Protokollierungs Verfahren, wie in den folgenden Artikeln erläutert. Im Beispiel wird die Anwendungs Protokollierung mithilfe von [nlog](http://nlog-project.org/)implementiert. Eine Verwandte Dokumentation finden Sie [in der Reihe mit vier TechNet wiki-Artikeln über Telemetrie und Protokollierung](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Zurück](design-to-survive-failures.md)
> [Weiter](transient-fault-handling.md)
