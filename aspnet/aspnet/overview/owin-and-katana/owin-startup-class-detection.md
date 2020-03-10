---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Erkennung der owin-Startklasse | Microsoft-Dokumentation
author: Praburaj
description: In diesem Tutorial wird gezeigt, wie Sie konfigurieren, welche owin-Startklasse geladen wird. Weitere Informationen zu owin finden Sie unter Übersicht über Project Katana. Dieses Tutorial war...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500181"
---
# <a name="owin-startup-class-detection"></a>Erkennung der OWIN-Startup-Klasse

> In diesem Tutorial wird gezeigt, wie Sie konfigurieren, welche owin-Startklasse geladen wird. Weitere Informationen zu owin finden Sie unter [Übersicht über Project Katana](an-overview-of-project-katana.md). Dieses Tutorial wurde von Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), praburaj Thiagarajan und Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ) verfasst.
>
> ## <a name="prerequisites"></a>Voraussetzungen
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a>Erkennung der OWIN-Startup-Klasse

 Jede owin-Anwendung verfügt über eine Startup-Klasse, in der Sie Komponenten für die Anwendungs Pipeline angeben. Es gibt verschiedene Möglichkeiten, wie Sie Ihre Startup-Klasse mit der Laufzeit verbinden können, je nachdem, welches Hostingmodell Sie auswählen (owinhost, IIS und IIS-Express). Die in diesem Tutorial gezeigte Startup-Klasse kann in jeder Hostinganwendung verwendet werden. Sie verbinden die Startup-Klasse mit der Hostinglaufzeit, indem Sie einen der folgenden Ansätze verwenden:

1. **Benennungs Konvention**: Katana sucht nach einer Klasse mit dem Namen `Startup` im Namespace, der mit dem Assemblynamen oder dem globalen Namespace übereinstimmt.
2. **Owinstartup-Attribut**: Dies ist der Ansatz, den die meisten Entwickler ergreifen, um die Startup-Klasse anzugeben. Mit dem folgenden Attribut wird die Startup-Klasse auf die `TestStartup`-Klasse im `StartupDemo`-Namespace festgelegt.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   Das `OwinStartup`-Attribut überschreibt die Benennungs Konvention. Sie können einen anzeigen Amen auch mit diesem Attribut angeben. Wenn Sie jedoch einen anzeigen Amen verwenden, müssen Sie auch das `appSetting`-Element in der Konfigurationsdatei verwenden.
3. **Das appSetting-Element in der Konfigurationsdatei**: das `appSetting`-Element überschreibt das `OwinStartup` Attribut und die Benennungs Konvention. Sie können über mehrere Startklassen verfügen (jeweils mit einem `OwinStartup`-Attribut) und konfigurieren, welche Startklasse in eine Konfigurationsdatei geladen wird, indem Sie ein Markup ähnlich dem folgenden verwenden:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Der folgende Schlüssel, der explizit die Startup-Klasse und die Assembly angibt, kann ebenfalls verwendet werden:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Der folgende XML-Code in der Konfigurationsdatei gibt einen anzeigen Amen für die Startklasse `ProductionConfiguration`an.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Das obige Markup muss mit dem folgenden `OwinStartup` Attribute verwendet werden, das einen anzeigen Amen angibt und bewirkt, dass die `ProductionStartup2` Klasse ausgeführt wird.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Fügen Sie zum Deaktivieren der owin-Start Ermittlung die `appSetting owin:AutomaticAppStartup` mit dem Wert `"false"` in der Datei "Web. config" hinzu.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Erstellen einer ASP.net-Web-App mit owin-Start

1. Erstellen Sie eine leere ASP.NET-Webanwendung, und nennen Sie Sie **startupdemo**. -Installieren Sie `Microsoft.Owin.Host.SystemWeb` mit dem nuget-Paket-Manager. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**und dann auf Paket-Manager- **Konsole**. Geben Sie folgenden Befehl ein:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Fügen Sie eine owin-Startklasse hinzu. Klicken Sie in Visual Studio 2017 mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**aus. Geben Sie im Dialogfeld **Neues Element hinzufügen** *owin* in das Suchfeld ein, und ändern Sie den Namen in Startup.cs, und klicken Sie dann auf **Hinzufügen**.

     ![](owin-startup-class-detection/_static/image1.png)

   Wenn Sie das nächste Mal eine *owin-Startklasse*hinzufügen möchten, wird diese im Menü **Hinzufügen** verfügbar sein.

     ![](owin-startup-class-detection/_static/image2.png)

   Sie können auch mit der rechten Maustaste auf das Projekt klicken und **Hinzufügen**auswählen. Wählen Sie dann **Neues Element**aus, und wählen Sie dann die **owin-Startklasse aus**.

     ![](owin-startup-class-detection/_static/image3.png)

- Ersetzen Sie den generierten Code in der *Startup.cs* -Datei durch Folgendes:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  Der `app.Use` Lambda-Ausdruck wird verwendet, um die angegebene Middlewarekomponente in der owin-Pipeline zu registrieren. In diesem Fall richten Sie die Protokollierung eingehender Anforderungen ein, bevor Sie auf die eingehende Anforderung Antworten. Der `next`-Parameter ist der Delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) zur nächsten Komponente in der Pipeline. Der `app.Run` Lambda-Ausdruck verknüpft die Pipeline mit eingehenden Anforderungen und stellt den Antwortmechanismus bereit.
     > [!NOTE]
     > Im obigen Code haben wir das `OwinStartup` Attribut auskommentiert, und wir verlassen uns auf die Konvention, die Klasse mit dem Namen `Startup` auszuführen. Drücken Sie ***F5*** , um die Anwendung auszuführen. Die Treffer Aktualisierung ist mehrmals.

    ![](owin-startup-class-detection/_static/image4.png) Hinweis: die Zahl, die in den Bildern in diesem Tutorial angezeigt wird, stimmt nicht mit der angezeigten Anzahl ab. Die millisekundenzeichenfolge wird verwendet, um eine neue Antwort anzuzeigen, wenn Sie die Seite aktualisieren.
  Die Ablauf Verfolgungs Informationen können im Fenster **Ausgabe** angezeigt werden.

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Weitere Startklassen hinzufügen

In diesem Abschnitt fügen wir eine weitere Startup-Klasse hinzu. Sie können Ihrer Anwendung mehrere owin-Startklassen hinzufügen. Beispielsweise können Sie Startklassen für Entwicklung, Tests und Produktion erstellen.

1. Erstellen Sie eine neue owin-Startklasse, und benennen Sie Sie `ProductionStartup`.
2. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Drücken Sie die Taste F5, um die APP auszuführen. Das `OwinStartup`-Attribut gibt an, dass die Produktionsstart Klasse ausgeführt wird.

    ![](owin-startup-class-detection/_static/image6.png)
4. Erstellen Sie eine weitere owin-Startklasse, und benennen Sie Sie `TestStartup`.
5. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   Mit der obigen `OwinStartup` Attribut Überladung wird `TestingConfiguration` als *Anzeige Name der* Startklasse angegeben.
6. Öffnen Sie die Datei " *Web. config* ", und fügen Sie den owin-App-Start Schlüssel hinzu, der den anzeigen amen der Startklasse angibt:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Drücken Sie die Taste F5, um die APP auszuführen. Das App settings-Element übernimmt einen Präzedenzfall, und die Testkonfiguration wird ausgeführt.

    ![](owin-startup-class-detection/_static/image7.png)
8. Entfernen Sie den *anzeigen Amen aus dem `OwinStartup`* -Attribut in der `TestStartup`-Klasse.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Ersetzen Sie den owin-App-Systemstart Schlüssel in der Datei " *Web. config* " durch Folgendes:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Setzen Sie das `OwinStartup`-Attribut in jeder Klasse auf den von Visual Studio generierten Standard Attribut Code zurück:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Jeder der folgenden Start Schlüssel der owin-App führt dazu, dass die produktionsklasse ausgeführt wird.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Der letzte Start Schlüssel gibt die Start Konfigurations Methode an. Mit dem folgenden owin-App-Start Schlüssel können Sie den Namen der Konfigurations Klasse in "`MyConfiguration`" ändern.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Verwenden von "owinhost. exe"

1. Ersetzen Sie die Datei "Web. config" durch das folgende Markup:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Der letzte Schlüssel gewinnt, daher wird in diesem Fall `TestStartup` angegeben.
2. Installieren Sie owinhost über die PMC:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navigieren Sie zum Anwendungsordner (der Ordner mit der Datei " *Web. config* "), und geben Sie in einer Eingabeaufforderung Folgendes ein:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Im Befehlsfenster wird Folgendes angezeigt:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Starten Sie einen Browser mit der URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   Owinhost hat die oben aufgeführten Start Konventionen berücksichtigt.
5. Drücken Sie im Befehlsfenster die EINGABETASTE, um owinhost zu schließen.
6. Fügen Sie in der `ProductionStartup`-Klasse das folgende owinstartup-Attribut hinzu, das den anzeigen amen der *productionconfiguration*angibt.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Geben Sie in der Eingabeaufforderung Folgendes ein:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Die Produktionsstart Klasse wird geladen.
    ![](owin-startup-class-detection/_static/image9.png)

   Unsere Anwendung verfügt über mehrere Startklassen, und in diesem Beispiel haben wir die Startklasse zurückgestellt, die bis zur Laufzeit geladen werden soll.
8. Testen Sie die folgenden Startoptionen für die Laufzeit:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
