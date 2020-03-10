---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Ablauf Verfolgung in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie die Ablauf Verfolgung in ASP.net-Web-API aktiviert wird.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484341"
---
# <a name="tracing-in-aspnet-web-api-2"></a>Ablauf Verfolgung in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

> Wenn Sie versuchen, eine webbasierte Anwendung zu debuggen, gibt es keinen Ersatz für einen guten Satz von Ablauf Verfolgungs Protokollen. In diesem Tutorial wird gezeigt, wie die Ablauf Verfolgung in ASP.net-Web-API aktiviert wird. Mit dieser Funktion können Sie verfolgen, was das Web-API-Framework vor und nach dem Aufrufen Ihres Controllers tut. Sie können Sie auch verwenden, um Ihren eigenen Code zu verfolgen.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (funktioniert auch mit Visual Studio 2015)
> - Web-API 2
> - [Microsoft. Aspnet. WebAPI. Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>System. Diagnostics-Ablauf Verfolgung in der Web-API aktivieren

Zunächst erstellen wir ein neues ASP.NET-Webanwendungs Projekt. Wählen Sie in Visual Studio im Menü **Datei** die Option **neu** > **Projekt**aus. Wählen Sie unter **Vorlagen**die Option **Web**, **ASP.NET Webanwendung**aus.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Wählen Sie die Vorlage Web-API-Projekt aus.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Wählen Sie **im Menü** Extras den Befehl **nuget-Paket-Manager**und dann **Package Manage Console**aus.

Geben Sie im Fenster der Paket-Manager-Konsole die folgenden Befehle ein.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Mit dem ersten Befehl wird das neueste Web-API-Ablauf Verfolgungs Paket installiert. Außerdem werden die Kern-Web-API-Pakete aktualisiert. Mit dem zweiten Befehl wird das WebAPI. Webhost-Paket auf die neueste Version aktualisiert.

> [!NOTE]
> Wenn Sie eine bestimmte Version der Web-API als Ziel verwenden möchten, verwenden Sie das Flag-Version, wenn Sie das Ablauf Verfolgungs Paket installieren.

Öffnen Sie die Datei WebApiConfig.cs im Ordner App\_Start. Fügen Sie der **Register** -Methode den folgenden Code hinzu.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Mit diesem Code wird die Klasse " [systemdiagnosticstracewriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) " der Web-API-Pipeline hinzugefügt. Die **systemdiagnosticstracewriter** -Klasse schreibt Ablauf Verfolgungen in [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Um die Ablauf Verfolgungen anzuzeigen, führen Sie die Anwendung im Debugger aus. Navigieren Sie im Browser zu `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Die Trace-Anweisungen werden in das Ausgabefenster in Visual Studio geschrieben. (Wählen Sie im Menü **Ansicht** die Option **Ausgabe**aus.)

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Da **systemdiagnosticstracewriter** Ablauf Verfolgungen in **System. Diagnostics. Trace**schreibt, können Sie zusätzliche Ablaufverfolgungslistener registrieren. beispielsweise, um Ablauf Verfolgungen in eine Protokolldatei zu schreiben. Weitere Informationen zu Ablaufverfolgungslistener finden Sie im Thema [Trace](https://msdn.microsoft.com/library/4y5y10s7.aspx) Listener auf der MSDN-Website.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurieren von systemdiagnosticstracewriter

Der folgende Code zeigt, wie der ablaufverfolgungswriter konfiguriert wird.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Es gibt zwei Einstellungen, die Sie steuern können:

- Isverbose: bei false enthält jede Ablauf Verfolgung minimale Informationen. True gibt an, dass Ablauf Verfolgungen Weitere Informationen enthalten.
- Minimumlevel: legt die minimale Ablauf Verfolgungs Ebene fest. Ablauf Verfolgungs Ebenen sind Debuggen, Info, Warnung, Fehler und schwerwiegend.

## <a name="adding-traces-to-your-web-api-application"></a>Hinzufügen von Ablauf Verfolgungen zu Ihrer Web-API-Anwendung

Durch das Hinzufügen eines Trace Writer erhalten Sie sofortigen Zugriff auf die von der Web-API-Pipeline erstellten Ablauf Verfolgungen. Sie können auch den ablaufverfolgungswriter verwenden, um den eigenen Code zu verfolgen:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Um den ablaufverfolgungswriter abzurufen, nennen Sie **httpconfiguration. Services. gettracewriter**. Von einem Controller aus kann auf diese Methode über die **apicontroller. Configuration** -Eigenschaft zugegriffen werden.

Wenn Sie eine Ablauf Verfolgung schreiben möchten, können Sie die Methode **itracewriter. Trace** direkt aufzurufen, aber die Klasse [itraceschreiterextensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) definiert einige Erweiterungs Methoden, die benutzerfreundlicher sind. Beispielsweise wird mit der oben gezeigten **Info** -Methode eine Ablauf Verfolgung mit **Informationen**auf Ablauf Verfolgungs Ebene erstellt.

## <a name="web-api-tracing-infrastructure"></a>Web-API-Ablauf Verfolgungs Infrastruktur

In diesem Abschnitt wird beschrieben, wie ein benutzerdefinierter ablaufverfolgungswriter für die Web-API

Das Paket Microsoft. Aspnet. WebAPI. Tracing basiert auf einer allgemeineren Ablauf Verfolgungs Infrastruktur in der Web-API. Anstatt Microsoft. Aspnet. WebAPI. Tracing zu verwenden, können Sie auch eine andere Ablauf Verfolgungs-/Protokollierungs Bibliothek einbinden, z. b. [nlog](http://nlog-project.org/) oder [log4net](http://logging.apache.org/log4net/).

Um Ablauf Verfolgungen zu erfassen, implementieren Sie die **itracewriter** -Schnittstelle. Hier sehen Sie ein einfaches Beispiel:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Die **itracewriter. Trace** -Methode erstellt eine Ablauf Verfolgung. Der Aufrufer gibt eine Kategorie und eine Ablauf Verfolgungs Ebene an. Die Kategorie kann eine beliebige benutzerdefinierte Zeichenfolge sein. Ihre Implementierung der Ablauf **Verfolgung** sollte folgende Aktionen ausführen:

1. Erstellen Sie einen neuen **TraceRecord**. Initialisieren Sie Sie wie gezeigt mit der Anforderungs-, der Kategorie-und der Ablauf Verfolgungs Ebene. Diese Werte werden vom Aufrufer bereitgestellt.
2. Rufen Sie den *traceaction* -Delegaten auf. Innerhalb dieses Delegaten wird erwartet, dass der Aufrufer den Rest des **TraceRecord**ausgefüllt.
3. Schreiben Sie den **TraceRecord**, indem Sie ein beliebiges Protokollierungs Verfahren verwenden, das Ihnen gefällt. Im hier gezeigten Beispiel wird einfach " **System. Diagnostics. Trace**" aufgerufen.

## <a name="setting-the-trace-writer"></a>Festlegen des Trace Writer

Um die Ablauf Verfolgung zu aktivieren, müssen Sie die Web-API für die Verwendung Ihrer **itracewriter** -Implementierung konfigurieren. Dies geschieht über das **httpconfiguration** -Objekt, wie im folgenden Code gezeigt:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Nur ein Ablauf Verfolgungs Schreiber kann aktiv sein. Standardmäßig legt die Web-API einen &quot;No-op-&quot;-Überwachungsdaten Satz fest, der keine Aktion ausführt. (Die &quot;No-op&quot; Überwachungs vorhanden ist, sodass der Ablauf Verfolgungs Code nicht prüfen muss, ob der ablaufverfolgungswriter **null** ist, bevor eine Ablauf Verfolgung geschrieben wird.)

## <a name="how-web-api-tracing-works"></a>Funktionsweise der Web API-Ablauf Verfolgung

Die Ablauf Verfolgung in der Web-API verwendet ein *Fassaden* Muster: Wenn die Ablauf Verfolgung aktiviert ist, umschließt die Web-API verschiedene Teile der Anforderungs Pipeline mit Klassen, die Ablauf Verfolgungs Aufrufe ausführen.

Wenn Sie z. b. einen Controller auswählen, verwendet die Pipeline die **ihttpcontrollerselector** -Schnittstelle. Bei aktivierter Ablauf Verfolgung fügt die Pipeline eine Klasse ein, die **ihttpcontrollerselector** implementiert, aber Aufrufe an die tatsächliche Implementierung durchführt:

![Die Web-API-Ablauf Verfolgung verwendet das Fassaden Muster.](tracing-in-aspnet-web-api/_static/image8.png)

Dieser Entwurf bietet folgende Vorteile:

- Wenn Sie keinen Trace Writer hinzufügen, werden die Ablauf Verfolgungs Komponenten nicht instanziiert und haben keine Auswirkungen auf die Leistung.
- Wenn Sie Standarddienste wie **ihttpcontrollerselector** durch ihre eigene benutzerdefinierte Implementierung ersetzen, wird die Ablauf Verfolgung nicht beeinträchtigt, da die Ablauf Verfolgung durch das Wrapper Objekt erfolgt.

Sie können auch das gesamte Web-API-Ablauf Verfolgungs Framework durch ihr eigenes benutzerdefiniertes Framework ersetzen, indem Sie den **itracemanager** -Standard Dienst ersetzen:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementieren Sie **itracemanager. Initialisieren** , um das Ablauf Verfolgungssystem zu initialisieren. Beachten Sie, dass dies das *gesamte* Ablauf Verfolgungs Framework ersetzt, einschließlich des gesamten Ablauf Verfolgungs Codes, der in die Web-API integriert ist.
