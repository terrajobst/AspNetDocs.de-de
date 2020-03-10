---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Owin-Middleware in der integrierten IIS-Pipeline | Microsoft-Dokumentation
author: Praburaj
description: In diesem Artikel erfahren Sie, wie Sie owin-middlewarekomponenten (OMCs) in der integrierten IIS-Pipeline ausführen und wie Sie das Pipeline Ereignis festlegen, auf dem eine OMC ausgeführt wird. Sie sollten...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 7d157fb6bd9e2ae9b55af41ef06c1eb5e6310ce1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500259"
---
# <a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Owin-Middleware in der integrierten IIS-Pipeline

von [praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://twitter.com/RickAndMSFT)

> In diesem Artikel erfahren Sie, wie Sie owin-middlewarekomponenten (OMCs) in der integrierten IIS-Pipeline ausführen und wie Sie das Pipeline Ereignis festlegen, auf dem eine OMC ausgeführt wird. Bevor Sie dieses Tutorial lesen, sollten Sie [eine Übersicht über die Project Katana](an-overview-of-project-katana.md) -und [owin-Startklassen Erkennung](owin-startup-class-detection.md) lesen. Dieses Tutorial wurde von Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, praburaj Thiagarajan und Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ) verfasst.

Obwohl [owin](an-overview-of-project-katana.md) -middlewarekomponenten (OMCs) in erster Linie für die Durchführung in einer Server unabhängigen Pipeline entwickelt wurden, ist es möglich, auch ein OMC in der integrierten IIS-Pipeline auszuführen (**klassischer Modus wird *nicht* unterstützt**). Zum Arbeiten mit der integrierten IIS-Pipeline kann ein OMC erstellt werden, indem das folgende Paket über die Paket-Manager-Konsole (PMC) installiert wird:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Dies bedeutet, dass alle Anwendungs Frameworks, auch solche, die noch nicht außerhalb von IIS und System. Web ausgeführt werden können, von vorhandenen owin-Middleware-Komponenten profitieren können. 

> [!NOTE]
> Alle `Microsoft.Owin.Security.*` Pakete, die mit dem neuen Identitäts System in Visual Studio 2013 ausgeliefert werden (z. b. Cookies, Microsoft-Konto, Google, Facebook, Twitter, [bearertoken](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, autorisierungsserver, JWT, Azure Active Directory und Active Directory-Verbund Dienste), werden als OMCs erstellt und können in selbstgeh osteten und IIS-gehosteten Szenarios verwendet werden.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Ausführung von owin-Middleware in der integrierten IIS-Pipeline

Für owin-Konsolen Anwendungen wird die mithilfe der [Startkonfiguration](owin-startup-class-detection.md) erstellter Anwendungs Pipeline durch die Reihenfolge festgelegt, in der die Komponenten mithilfe der `IAppBuilder.Use`-Methode hinzugefügt werden. Das heißt, dass die owin-Pipeline in der [Katana](an-overview-of-project-katana.md) -Laufzeit OMCs in der Reihenfolge verarbeitet, in der Sie mit `IAppBuilder.Use`registriert wurden. In der integrierten IIS-Pipeline besteht die Anforderungs Pipeline aus [httpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) , die einen vordefinierten Satz von Pipeline Ereignissen abonniert haben, z. b. [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [autorizerequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)usw.

Wenn wir eine OMC mit der eines [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) in der ASP.net World vergleichen, muss ein OMC für das korrekte vordefinierte Pipeline Ereignis registriert werden. Beispielsweise wird die HttpModule-`MyModule` aufgerufen, wenn eine Anforderung an die [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) -Phase in der Pipeline gesendet wird:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Damit ein OMC an derselben ereignisbasierten Ausführungsreihenfolge teilnehmen kann, scannt der [Katana](an-overview-of-project-katana.md) -Lauf Zeit Code die [Startkonfiguration](owin-startup-class-detection.md) und abonniert jede Middlewarekomponente mit einem integrierten Pipeline Ereignis. Mit dem folgenden OMC-und Registrierungscode können Sie z. b. die Standard Ereignis Registrierung von Middleware-Komponenten anzeigen. (Ausführlichere Anweisungen zum Erstellen einer owin-Startklasse finden Sie unter [owin-Startklassen Erkennung](owin-startup-class-detection.md).)

1. Erstellen Sie ein leeres Webanwendungs Projekt, und nennen Sie es **owin2**.
2. Führen Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl aus: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Fügen Sie eine `OWIN Startup Class` hinzu, und benennen Sie Sie `Startup`. Ersetzen Sie den generierten Code durch Folgendes (die Änderungen werden hervorgehoben):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Drücken Sie F5, um die APP auszuführen.

Die Startkonfiguration richtet eine Pipeline mit drei middlewarekomponenten ein, die ersten beiden Diagnoseinformationen und das letzte, das auf Ereignisse reagiert (und auch Diagnoseinformationen angezeigt). Die `PrintCurrentIntegratedPipelineStage`-Methode zeigt das integrierte Pipeline Ereignis, für das diese Middleware aufgerufen wird, und eine Meldung an. In den Ausgabe Fenstern wird Folgendes angezeigt:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Die Katana-Laufzeit hat jede der owin-middlewarekomponenten standardmäßig dem [preexecuterequesthandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) zugeordnet, was dem IIS-Pipeline-Ereignis [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)entspricht.

## <a name="stage-markers"></a>Stufen Marker

Sie können OMCs so markieren, dass Sie in bestimmten Phasen der Pipeline ausgeführt werden, indem Sie die `IAppBuilder UseStageMarker()`-Erweiterungsmethode verwenden. Um einen Satz von middlewarekomponenten in einer bestimmten Phase auszuführen, fügen Sie einen Stufen Marker direkt nach der letzten Komponente ein, die während der Registrierung festgelegt ist. Es gibt Regeln, in denen Sie die Middleware ausführen können, und die Bestell Komponenten müssen ausgeführt werden (die Regeln werden später in diesem Tutorial erläutert). Fügen Sie dem `Configuration` Code wie unten gezeigt die `UseStageMarker`-Methode hinzu:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Der `app.UseStageMarker(PipelineStage.Authenticate)`-Befehl konfiguriert alle zuvor registrierten middlewarekomponenten (in diesem Fall unsere beiden Diagnose Komponenten), die in der Authentifizierungs Phase der Pipeline ausgeführt werden. Die letzte Middlewarekomponente (die Diagnose anzeigt und auf Anforderungen antwortet) wird in der `ResolveCache` Phase ausgeführt (das [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) -Ereignis).

Drücken Sie F5, um die APP auszuführen. Das Ausgabefenster zeigt Folgendes an:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Phasen markerregeln

Owin-middlewarekomponenten (OMC) können so konfiguriert werden, dass Sie mit den folgenden owin-Pipeline Stufen Ereignissen ausgeführt werden:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Standardmäßig wird OMCs beim letzten Ereignis (`PreHandlerExecute`) ausgeführt. Aus diesem Grund wird im ersten Beispielcode "preexecuterequesthandler" angezeigt.
2. Sie können die `app.UseStageMarker`-Methode verwenden, um eine OMC-Funktion zu registrieren, die Sie zuvor in jeder Phase der owin-Pipeline in der `PipelineStage`-Aufzählung ausgeführt haben.
3. Die owin-Pipeline und die IIS-Pipeline sind sortiert, daher müssen Aufrufe an `app.UseStageMarker` in der richtigen Reihenfolge erfolgen. Der Ereignishandler kann nicht auf ein Ereignis festgelegt werden, das dem zuletzt bei registrierten Ereignis vorangestellt ist, um `app.UseStageMarker`. Beispielsweise *nach* dem Aufrufen von:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   Aufrufe an `app.UseStageMarker`, die `Authenticate` oder `PostAuthenticate` übergeben werden, werden nicht berücksichtigt, und es wird keine Ausnahme ausgelöst. OMCs werden auf der neuesten Stufe ausgeführt, was standardmäßig `PreHandlerExecute`ist. Die Stufen Marker werden verwendet, um Sie früher auszuführen. Wenn Sie Phasen Marker nicht in der richtigen Reihenfolge angeben, wird auf den vorherigen Marker gerundet. Mit anderen Worten: das Hinzufügen eines Phasen Markers besagt, dass "Run no later than Stage X". OMC wird auf dem frühesten Phasen Marker ausgeführt, der in der owin-Pipeline nach dem Typ hinzugefügt wurde.
4. Die früheste Phase von Aufrufen von `app.UseStageMarker` WINS. Wenn Sie z. b. die Reihenfolge der `app.UseStageMarker` Aufrufe aus dem vorherigen Beispiel wechseln:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Im Ausgabefenster wird Folgendes angezeigt: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Die OMCs werden alle in der `AuthenticateRequest` Phase ausgeführt, weil das letzte bei der `Authenticate`-Ereignis registrierte OMC und das `Authenticate` Ereignis allen anderen Ereignissen vorangestellt sind.
