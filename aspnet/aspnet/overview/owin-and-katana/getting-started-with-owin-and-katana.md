---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Einstieg in owin und Katana | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472443"
---
# <a name="getting-started-with-owin-and-katana"></a>Erste Schritte mit OWIN und Katana

von [Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface for .net (owin)](http://owin.org/) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen. Durch die Entkopplung des Webservers von der Anwendung vereinfacht owin das Erstellen von Middleware für die .net-Webentwicklung. Außerdem vereinfacht owin das Portieren von Webanwendungen auf andere Hosts&#8212;, z. b. das Self-Hosting in einem Windows-Dienst oder einem anderen Prozess.

Owin ist eine in der Community befindliche Spezifikation, keine Implementierung. Das Katana-Projekt ist ein Satz von Open-Source-owin-Komponenten, die von Microsoft entwickelt wurden. Eine allgemeine Übersicht über owin und Katana finden Sie [in der Übersicht über Project Katana](an-overview-of-project-katana.md). In diesem Artikel gehe ich direkt in den Code ein, um zu beginnen.

In diesem Tutorial wird [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)verwendet, Sie können jedoch auch Visual Studio 2012 verwenden. Einige der Schritte unterscheiden sich in Visual Studio 2012, was ich unten beachten werde.

## <a name="host-owin-in-iis"></a>Hosten von owin in IIS

In diesem Abschnitt hosten wir owin in IIS. Mit dieser Option können Sie die Flexibilität und Zusammenfassung einer owin-Pipeline zusammen mit dem ausgereiften Funktions Satz von IIS kombinieren. Mit dieser Option wird die owin-Anwendung in der ASP.net Request-Pipeline ausgeführt.

Erstellen Sie zunächst ein neues ASP.NET-Webanwendungs Projekt. (Verwenden Sie in Visual Studio 2012 den Projekttyp ASP.net Empty Webanwendung.)

![](getting-started-with-owin-and-katana/_static/image1.png)

Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Hinzufügen von NuGet-Paketen

Fügen Sie als nächstes die erforderlichen nuget-Pakete hinzu. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Startklasse hinzufügen

Fügen Sie als nächstes eine owin-Startklasse hinzu. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**und dann **Neues Element**aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **owin-Startklasse**aus. Weitere Informationen zum Konfigurieren der Startup-Klasse finden Sie unter [owin-Startklassen Erkennung](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Fügen Sie der `Startup1.Configuration`-Methode folgenden Code hinzu:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Mit diesem Code wird der owin-Pipeline eine einfache Middleware hinzugefügt, die als Funktion implementiert wird, die eine **Microsoft. owin. iowincontext** -Instanz empfängt. Wenn der Server eine HTTP-Anforderung empfängt, ruft die owin-Pipeline die Middleware auf. Die Middleware legt den Inhaltstyp für die Antwort fest und schreibt den Antworttext.

> [!NOTE]
> Die owin-Startklassen Vorlage ist in Visual Studio 2013 verfügbar. Wenn Sie Visual Studio 2012 verwenden, fügen Sie einfach eine neue leere Klasse mit dem Namen "`Startup1`" hinzu, und fügen Sie den folgenden Code ein:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Ausführen der Anwendung

Drücken Sie F5, um das Debugging zu starten. Visual Studio öffnet ein Browserfenster zum `http://localhost:*port*/`. Die Seite sollte wie folgt aussehen:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Selbst Hosten von owin in einer Konsolenanwendung

Es ist einfach, diese Anwendung in einem benutzerdefinierten Prozess von IIS-Hosting in Self-Hosting zu konvertieren. Beim IIS-Hosting fungiert IIS sowohl als HTTP-Server als auch als Prozess, der den Dienst hostet. Beim Self-Hosting erstellt die Anwendung den Prozess und verwendet die **HttpListener** -Klasse als HTTP-Server.

Erstellen Sie in Visual Studio eine neue Konsolenanwendung. Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Fügen Sie dem Projekt aus Teil 1 dieses Tutorials eine `Startup1` Klasse hinzu. Sie müssen diese Klasse nicht ändern.

Implementieren Sie die `Main`-Methode der Anwendung wie folgt.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Wenn Sie die Konsolenanwendung ausführen, beginnt der Server mit dem lauschen an `http://localhost:9000`. Wenn Sie in einem Webbrowser zu dieser Adresse navigieren, wird die Seite "Hello World" angezeigt.

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Owin-Diagnose hinzufügen

Das Microsoft. owin. Diagnostics-Paket enthält Middleware, die nicht behandelte Ausnahmen abfängt und eine HTML-Seite mit Fehlerdetails anzeigt. Diese Seite funktioniert ähnlich wie die ASP.NET-Fehlerseite, die manchmal auch als "[gelber Bildschirm von Tod](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (Ysod) bezeichnet wird. Wie bei Ysod ist die Katana-Fehlerseite während der Entwicklung nützlich. es wird jedoch empfohlen, Sie im Produktionsmodus zu deaktivieren.

Um das Diagnosepaket in Ihrem Projekt zu installieren, geben Sie den folgenden Befehl im Fenster Paket-Manager-Konsole ein:

`install-package Microsoft.Owin.Diagnostics –Pre`

Ändern Sie den Code in der `Startup1.Configuration`-Methode wie folgt:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Verwenden Sie jetzt STRG + F5, um die Anwendung ohne Debuggen auszuführen, sodass Visual Studio bei der Ausnahme nicht unterbricht. Die Anwendung verhält sich wie zuvor, bis Sie zu `http://localhost/fail`navigieren. zu diesem Zeitpunkt löst die Anwendung die Ausnahme aus. Die Middleware der Fehlerseite fängt die Ausnahme ab und zeigt eine HTML-Seite mit Informationen über den Fehler an. Sie können auf die Registerkarten klicken, um die Umgebungsvariablen Stapel, Abfrage Zeichenfolge, Cookies, Anforderungs Header und owin anzuzeigen.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erkennung der OWIN-Startup-Klasse](owin-startup-class-detection.md)
- [Verwenden von owin für Self-Host-ASP.net-Web-API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Verwenden von owin zum selbst Hosten von signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
