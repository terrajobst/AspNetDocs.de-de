---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Praktische Übung: Erstellen einer Single-Page-Anwendung (Spa) mit ASP.net-Web-API und Angular. js-ASP.NET 4. x'
author: rick-anderson
description: 'Schritt-für-Schritt-Code: Erstellen einer Single-Page-Anwendung (Spa) mit ASP.net-Web-API und Angular. js für ASP.NET 4. x.'
ms.author: riande
ms.date: 09/30/2015
ms.custom: seoapril2019
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 86833a890da759e489dd11dc9afb128a9b7a75e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448761"
---
# <a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Praktische Übung: Erstellen einer Single-Page-Anwendung (Spa) mit ASP.net-Web-API und Angular. js

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

Diese praktische Übung zeigt Ihnen, wie Sie eine Single-Page-Anwendung (Single Page Application, Spa) mit ASP.net-Web-API und Angular. js für ASP.NET 4. x erstellen.

In dieser praktischen Übungseinheit nutzen Sie diese Technologien zur Implementierung von Geek Quiz, einer Trivia-Website, die auf dem Spa-Konzept basiert. Zuerst implementieren Sie die Dienst Ebene mit ASP.net-Web-API, um die erforderlichen Endpunkte zum Abrufen der Quizfragen und zum Speichern der Antworten verfügbar zu machen. Anschließend erstellen Sie mithilfe von angularjs und CSS3-Transformations Effekten eine umfangreiche und reaktionsfähige Benutzeroberfläche.

In herkömmlichen Webanwendungen initiiert der Client (Browser) die Kommunikation mit dem Server, indem er eine Seite anfordert. Der Server verarbeitet dann die Anforderung und sendet den HTML-Code der Seite an den Client. Bei nachfolgenden Interaktionen mit der Seite – z. b. der Benutzer navigiert zu einem Link oder übermittelt ein Formular mit Daten – wird eine neue Anforderung an den Server gesendet, und der Flow wird erneut gestartet: der Server verarbeitet die Anforderung und sendet eine neue Seite als Reaktion auf die neue Aktions Anforderung an den Browser. vom Client.
> 
> In Single-Page-Anwendungen (Spas) wird die gesamte Seite nach der anfänglichen Anforderung im Browser geladen, aber nachfolgende Interaktionen erfolgen durch AJAX-Anforderungen. Dies bedeutet, dass der Browser nur den Teil der Seite aktualisieren muss, der geändert wurde. Es ist nicht erforderlich, die gesamte Seite neu zu laden. Der Spa-Ansatz reduziert die Zeit, die die Anwendung benötigt, um auf Benutzeraktionen zu reagieren. Dies führt zu einer weniger flüssigen Benutzer Leistung.
> 
> Die Architektur einer Spa umfasst bestimmte Herausforderungen, die nicht in herkömmlichen Webanwendungen vorhanden sind. Neue Technologien wie ASP.net-Web-API, JavaScript-Frameworks wie angularjs und neue Formatierungsfunktionen von CSS3 erleichtern es Ihnen jedoch, Spas zu entwerfen und zu erstellen.
> 
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit)verfügbar ist.

## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Erstellen eines ASP.net-Web-API Dienstanbieter zum Senden und empfangen von JSON-Daten
- Erstellen einer reaktionsfähigen Benutzeroberfläche mithilfe von angularjs
- Verbessern der Benutzeroberfläche mit CSS3-Transformationen

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zuerst Ihre Umgebung einrichten.

1. Öffnen Sie Windows-Explorer, und navigieren Sie zum **Quell** Ordner des Labs.
2. Klicken Sie mit der rechten Maustaste auf **Setup. cmd** , und wählen Sie **als Administrator ausführen** aus, um den Setup Vorgang zu starten, mit dem die Umgebung konfiguriert wird, und die Visual Studio-Code Ausschnitte für diese Übungseinheit
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion, um fortzufahren.

> [!NOTE]
> Vergewissern Sie sich, dass Sie vor dem Ausführen des Setups alle Abhängigkeiten für dieses Lab geprüft haben.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden der Code Ausschnitte

Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen. Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, auf die Sie in Visual Studio 2013 zugreifen können, um das manuelle Hinzufügen zu vermeiden.

> [!NOTE]
> Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner " **Begin** " der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen. Beachten Sie, dass die während einer Übung hinzugefügten Code Ausschnitte in diesen Projektmappen fehlen und möglicherweise erst nach Abschluss der Übung funktionieren. Innerhalb des Quellcodes für eine Übung finden Sie auch einen **endordner** mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Erstellen einer Web-API](#Exercise1)
2. [Erstellen einer Spa-Schnittstelle](#Exercise2)

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen. Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen. In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind. Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Übung 1: Erstellen einer Web-API

Einer der wichtigsten Teile einer Spa ist die Dienst Schicht. Er ist verantwortlich für die Verarbeitung der AJAX-Aufrufe, die von der Benutzeroberfläche gesendet werden, und die Rückgabe von Daten als Reaktion auf diesen Aufruf Die abgerufenen Daten sollten in einem computerlesbaren Format dargestellt werden, damit Sie vom Client analysiert und verarbeitet werden können.

Das Web-API-Framework ist Teil des ASP.net-Stapels und wurde entwickelt, um die Implementierung von http-Diensten zu vereinfachen und in der Regel JSON-oder XML-formatierte Daten über eine Rest-API zu senden und zu empfangen. In dieser Übung erstellen Sie die Website zum Hosten der Anwendung "Geek Quiz" und implementieren dann den Back-End-Dienst, um die Quiz Daten mithilfe von ASP.net-Web-API verfügbar zu machen und beizubehalten.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Aufgabe 1 – Erstellen des ersten Projekts für das Geek-Quiz

In dieser Aufgabe erstellen Sie ein neues ASP.NET-MVC-Projekt mit Unterstützung für ASP.net-Web-API basierend auf dem **einen ASP.net** -Projekttyp, der in Visual Studio verfügbar ist. **Eine ASP.net** vereinigt alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit, Sie nach Belieben zu mischen und abzugleichen. Anschließend fügen Sie die Modellklassen des Entity Framework und den datenbankinitialisierer hinzu, um die Quizfragen einzufügen.

1. Öffnen Sie **Visual Studio Express 2013 für Web** , und wählen Sie Datei aus.  **Neues Projekt...** , um eine neue Projekt Mappe zu starten.

    ![Erstellen eines neuen Projekts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
2. Wählen Sie im Dialogfeld **Neues Projekt** unter der Visualisierung **C# ASP.NET Webanwendung aus.** Registerkarte "Web". Stellen Sie sicher, dass **.NET Framework 4,5** ausgewählt ist, nennen Sie es " *geekquiz*", **Wählen Sie einen** **Speicherort** aus,

    ![Erstellen eines neuen ASP.NET-Webanwendungs Projekts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Erstellen eines neuen ASP.NET-Webanwendungs Projekts")

    *Erstellen eines neuen ASP.NET-Webanwendungs Projekts*
3. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die **MVC** -Vorlage aus, und wählen Sie die Option **Web-API** aus. Stellen Sie außerdem sicher, dass die **Authentifizierungs** Option auf **einzelne Benutzerkonten**festgelegt ist. Klicken Sie zum Fortsetzen des Vorgangs auf **OK** .

    ![Erstellen eines neuen Projekts mit der MVC-Vorlage, einschließlich der Web-API-Komponenten](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Erstellen eines neuen Projekts mit der MVC-Vorlage, einschließlich der Web-API-Komponenten*
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Modelle** des **geekquiz** -Projekts, und wählen Sie **hinzufügen aus. Vorhandenes Element...**

    ![Hinzufügen eines vorhandenen Elements](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Hinzufügen eines vorhandenen Elements")

    *Hinzufügen eines vorhandenen Elements*
5. Navigieren Sie im Dialogfeld **Vorhandenes Element hinzufügen** zum Ordner **Source/Assets/Models** , und wählen Sie alle Dateien aus. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der modellassets](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Hinzufügen der modellassets")

    *Hinzufügen der modellassets*

    > [!NOTE]
    > Durch das Hinzufügen dieser Dateien fügen Sie das Datenmodell, den Daten Bank Kontext des Entity Framework und den datenbankinitialisierer für die Geek Quiz-Anwendung hinzu.
    > 
    > **Entity Framework (EF)** ist ein Objekt relationaler Mapper (ORM), mit dem Sie Datenzugriffs Anwendungen erstellen können, indem Sie mit einem konzeptionellen Anwendungsmodell programmieren, anstatt direkt mithilfe eines relationalen Speicher Schemas zu programmieren. Weitere Informationen zu Entity Framework finden Sie [hier](../../../entity-framework.md).
    > 
    > Im folgenden finden Sie eine Beschreibung der Klassen, die Sie soeben hinzugefügt haben:
    > 
    > - **Triviaoption:** stellt eine einzelne Option dar, die einer Quiz Frage zugeordnet ist.
    > - **Triviaquestion:** stellt eine Quiz Frage dar und macht die zugeordneten Optionen über die **options** -Eigenschaft verfügbar.
    > - **Triviaresponse:** stellt die vom Benutzer als Reaktion auf eine Quiz Frage ausgewählte Option dar.
    > - **Triviacontext:** stellt den Daten Bank Kontext des Entity Framework der Geek Quiz-Anwendung dar. Diese Klasse wird von **dcontext** abgeleitet und macht **dbset** -Eigenschaften verfügbar, die Auflistungen der oben beschriebenen Entitäten darstellen.
    > - **Triviadatabaseinitializer:** die Implementierung des Entity Framework Initialisierers für die **triviacontext** -Klasse, die von " **kreatedatabaseifnotexists**" erbt. Das Standardverhalten dieser Klasse besteht darin, die Datenbank nur dann zu erstellen, wenn Sie nicht vorhanden ist, wobei die in der **Seed** -Methode angegebenen Entitäten eingefügt werden.
6. Öffnen Sie die Datei **Global.asax.cs** , und fügen Sie die folgende using-Anweisung hinzu.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Fügen Sie am Anfang der **Anwendung\_Start** -Methode den folgenden Code hinzu, um den **triviadatabaseinitializer** als datenbankinitialisierer festzulegen.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Ändern Sie den **Home** -Controller, um den Zugriff auf authentifizierte Benutzer einzuschränken. Öffnen Sie hierzu die Datei **HomeController.cs** im Ordner **Controllers** , und fügen Sie das Attribut **autorisieren** der **HomeController** -Klassendefinition hinzu.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Der **Autorisierungs** Filter überprüft, ob der Benutzer authentifiziert ist. Wenn der Benutzer nicht authentifiziert ist, wird der HTTP-Statuscode 401 (nicht autorisiert) zurückgegeben, ohne dass die Aktion aufgerufen wird. Sie können den Filter Global, auf Controller Ebene oder auf der Ebene einzelner Aktionen anwenden.
9. Nun passen Sie das Layout der Webseiten und des Brandings an. Öffnen Sie hierzu die Datei **\_Layout. cshtml** in den **Ansichten | Frei gegebener Ordner,** und aktualisieren Sie den Inhalt des **&lt;Titels&gt;** Elements, indem Sie die *Anwendung "ASP.net* " durch " *Geek Quiz*" ersetzen.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Aktualisieren Sie in derselben *Datei die Navigations* Leiste, indem Sie die Links "Info" und " *Contact* " entfernen und den Link " *Home* " zur wieder *Gabe*umbenennen. Benennen Sie außerdem den *Anwendungsnamen* mit dem " *Geek Quiz*" um. Der HTML-Code für die Navigationsleiste sollte wie der folgende Code aussehen:

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aktualisieren Sie die Fußzeile der Layoutseite, indem Sie *meine ASP.NET-Anwendung* durch ein *Geek-Quiz*ersetzen. Ersetzen Sie dazu den Inhalt der **&lt;Fußzeile&gt;** -Elements durch den folgenden markierten Code.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Aufgabe 2 – Erstellen der Web-API von triviacontroller

In der vorherigen Aufgabe haben Sie die anfängliche Struktur der "Geek Quiz"-Webanwendung erstellt. Nun erstellen Sie einen einfachen Web-API-Dienst, der mit dem Quiz Datenmodell interagiert und die folgenden Aktionen verfügbar macht:

- **Get/API/Trivia**: Ruft die nächste Frage aus der Quiz Liste ab, die vom authentifizierten Benutzer beantwortet werden soll.
- **Post/API/Trivia**: speichert die vom authentifizierten Benutzer angegebene Quiz Antwort.

Verwenden Sie die von Visual Studio bereitgestellten ASP.net-gerüstingtools, um die Baseline für die Web-API-Controller Klasse zu erstellen.

1. Öffnen Sie die Datei **WebApiConfig.cs** im Ordner **App\_Start** . Diese Datei definiert die Konfiguration des Web-API-Diensts, z. b. wie Routen den Web-API-Controller Aktionen zugeordnet werden.
2. Fügen Sie am Anfang der Datei die folgende using-Anweisung hinzu.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Fügen Sie den folgenden markierten Code zur **Register** -Methode hinzu, um den Formatierer für die JSON-Daten, die von den Web-API-Aktionsmethoden abgerufen werden, Global zu konfigurieren

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Der **camelcasepropertynameskontosolver** konvertiert Eigenschaftsnamen automatisch in eine *Kamel* -Schreibweise. Dies ist die allgemeine Konvention für Eigenschaftsnamen in JavaScript.
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Controllers** des **geekquiz** -Projekts, und wählen Sie **hinzufügen aus. Neues Gerüst Element...**

    ![Erstellen eines neuen Gerüst Elements](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Erstellen eines neuen Gerüst Elements")

    *Erstellen eines neuen Gerüst Elements*
5. Vergewissern Sie sich, dass im Dialogfeld **Gerüst hinzufügen** im linken Bereich der Knoten **Allgemein** ausgewählt ist. Wählen Sie dann im mittleren Bereich die Vorlage **Web-API 2-Controller-leer** aus, und klicken Sie auf **Hinzufügen**.

    ![Auswählen der Vorlage "Web-API 2-Controller leer"](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Auswählen der Vorlage "Web-API 2-Controller leer"")

    *Auswählen der Vorlage "Web-API 2-Controller leer"*

    > [!NOTE]
    > **ASP.net Gerüstbau** ist ein Code Generierungs Framework für ASP.NET-Webanwendungen. Visual Studio 2013 enthält vorinstallierte Code Generatoren für MVC-und Web-API-Projekte. Sie sollten das Gerüst in Ihrem Projekt verwenden, wenn Sie schnell Code hinzufügen möchten, der mit Datenmodellen interagiert, um die für die Entwicklung von Standarddaten Vorgängen erforderliche Zeit zu reduzieren.
    > 
    > Der Gerüstbau Prozess stellt außerdem sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert werden. Wenn Sie z. b. mit einem leeren ASP.net-Projekt beginnen und dann mithilfe von Gerüstbau einen Web-API-Controller hinzufügen, werden die erforderlichen Web-API-nuget-Pakete und-Verweise automatisch dem Projekt hinzugefügt.
6. Geben Sie im Dialogfeld **Controller hinzufügen** im Textfeld **Controller Name den Namen** *triviacontroller* ein, und klicken Sie auf **Hinzufügen**.

    ![Hinzufügen des Trivia-Controllers](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Hinzufügen des Trivia-Controllers")

    *Hinzufügen des Trivia-Controllers*
7. Die Datei **TriviaController.cs** wird dann dem Ordner **Controllers** des **geekquiz** -Projekts hinzugefügt, das eine leere **triviacontroller** -Klasse enthält. Fügen Sie am Anfang der Datei die folgenden using-Anweisungen hinzu.

    (Code Ausschnitt- *aspnetwebapispa-EX1-triviacontrollerusings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Fügen Sie am Anfang der Klasse **triviacontroller** den folgenden Code hinzu, um die **triviacontext** -Instanz im Controller zu definieren, zu initialisieren und zu löschen.

    (Code Ausschnitt- *aspnetwebapispa-EX1-triviacontrollercontext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Die verwerfen-Methode von **triviacontroller** ruft **die verwerfen** **-Methode der** **triviacontext** -Instanz auf, mit der sichergestellt wird, dass alle vom Kontext Objekt verwendeten Ressourcen freigegeben werden, wenn die **triviacontext** -Instanz verworfen oder eine Garbage Collection durchgeführt wird. Dies umfasst das Schließen aller von Entity Framework geöffneten Datenbankverbindungen.
9. Fügen Sie am Ende der **triviacontroller** -Klasse die folgende Hilfsmethode hinzu. Diese Methode ruft die folgende Quiz Frage aus der-Datenbank ab, um vom angegebenen Benutzer beantwortet zu werden.

    (Code Ausschnitt- *aspnetwebapispa-EX1-triviacontrollernextquestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Fügen Sie der **triviacontroller** -Klasse die folgende **Get** -Aktionsmethode hinzu. Diese Aktionsmethode Ruft die im vorherigen Schritt definierte Hilfsmethode **nextfrag Async** auf, um die nächste Frage für den authentifizierten Benutzer abzurufen.

    (Code Ausschnitt- *aspnetwebapispa-EX1-triviacontrollergetaction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Fügen Sie am Ende der **triviacontroller** -Klasse die folgende Hilfsmethode hinzu. Diese Methode speichert die angegebene Antwort in der Datenbank und gibt einen booleschen Wert zurück, der angibt, ob die Antwort korrekt ist.

    (Code Ausschnitt- *aspnetwebapispa-EX1-triviacontrollerstoreasync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Fügen Sie der **triviacontroller** -Klasse die folgende **Post** -Aktionsmethode hinzu. Diese Aktionsmethode ordnet die Antwort dem authentifizierten Benutzer zu und ruft die **storeasync** -Hilfsmethode auf. Anschließend sendet er eine Antwort mit dem booleschen Wert, der von der-Hilfsmethode zurückgegeben wird.

    (Code Ausschnitt- *aspnetwebapispa-EX1-triviacontrollerpostaction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Ändern Sie den Web-API-Controller, um den Zugriff auf authentifizierte Benutzer einzuschränken, indem Sie das Attribut **autorisieren** der **triviacontroller** -Klassendefinition hinzufügen.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3 – Ausführen der Lösung

In dieser Aufgabe überprüfen Sie, ob der Web-API-Dienst, den Sie in der vorherigen Aufgabe erstellt haben, erwartungsgemäß funktioniert. Sie verwenden den Internet Explorer **F12-Entwicklertools** , um den Netzwerk Datenverkehr zu erfassen und die vollständige Antwort des Web-API-Diensts zu überprüfen.

> [!NOTE]
> Stellen Sie sicher, dass **Internet Explorer** in der Schaltfläche **Start** auf der Symbolleiste von Visual Studio ausgewählt ist.
> 
> ![Internet Explorer (Option)](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)

1. Drücken Sie **F5** , um die Projekt Mappe auszuführen. Die **Anmelde** Seite sollte im Browser angezeigt werden.

    > [!NOTE]
    > Wenn die Anwendung gestartet wird, wird die MVC-Standardroute ausgelöst, die standardmäßig der **Index** Aktion der **HomeController** -Klasse zugeordnet ist. Da **HomeController** auf authentifizierte Benutzer beschränkt ist (denken Sie daran, dass Sie diese Klasse mit dem Attribut **autorisieren** in Übung 1 versehen haben) und noch kein Benutzer authentifiziert ist, leitet die Anwendung die ursprüngliche Anforderung an die Anmeldeseite um.

    ![Ausführen der Lösung](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Ausführen der Lösung")

    *Ausführen der Lösung*
2. Klicken Sie auf **registrieren** , um einen neuen Benutzer zu erstellen.

    ![Registrieren eines neuen Benutzers](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Registrieren eines neuen Benutzers")

    *Registrieren eines neuen Benutzers*
3. Geben Sie auf der **Register** Seite einen **Benutzernamen** und ein **Kennwort**ein, und klicken Sie dann auf **registrieren**.

    ![Seite registrieren](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Seite registrieren")

    *Seite registrieren*
4. Die Anwendung registriert das neue Konto, und der Benutzer wird authentifiziert und zurück zur Startseite umgeleitet.

    ![Der Benutzer ist authentifiziert.](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "Benutzer authentifiziert")

    *Der Benutzer ist authentifiziert.*
5. Drücken Sie im Browser die Taste **F12** , um den **Entwicklertools** Bereich zu öffnen. Drücken Sie **STRG + 4** , oder klicken Sie auf das **Netzwerk** Symbol, und klicken Sie dann auf die grüne Pfeil Schaltfläche, um den Netzwerkverkehr zu erfassen

    ![Initiieren der Web-API-Netzwerk Erfassung](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Initiieren der Web-API-Netzwerk Erfassung")

    *Initiieren der Web-API-Netzwerk Erfassung*
6. Fügen Sie **API/Trivia** an die URL in der Adressleiste des Browsers an. Nun überprüfen Sie die Details der Antwort von der **Get** Action-Methode in **triviacontroller**.

    ![Abrufen der nächsten Frage Daten über die Web-API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Abrufen der nächsten Frage Daten über die Web-API")

    *Abrufen der nächsten Frage Daten über die Web-API*

    > [!NOTE]
    > Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei durchführen. Lassen Sie das Dialogfeld geöffnet, um den Antwort Inhalt über das Tool Fenster "Entwickler" anzeigen zu können.
7. Nun überprüfen Sie den Text der Antwort. Klicken Sie hierzu auf die Registerkarte **Details** , und klicken Sie dann auf **Antworttext**. Sie können überprüfen, ob es sich bei den heruntergeladenen Daten um ein Objekt mit den Eigenschaften **Optionen** (eine Liste von **triviaoption** -Objekten), **ID** und **Titel** handelt, die der Klasse **triviaquestion** entsprechen.

    ![Anzeigen des Web-API-Antwort Texts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Anzeigen des Web-API-Antwort Texts")

    *Anzeigen des Web-API-Antwort Texts*
8. Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Übung 2: Erstellen der Spa-Schnittstelle

In dieser Übung erstellen Sie zunächst den Web-Front-End-Teil des Geek Quiz, der sich auf die einseitige Anwendungs Interaktion mit **angularjs**konzentriert. Anschließend verbessern Sie die Benutzererfahrung mit CSS3, um umfangreiche Animationen auszuführen und einen visuellen Effekt des Kontext Wechsels beim Übergang von einer Frage zur nächsten zu bieten.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Aufgabe 1 – Erstellen der Spa-Schnittstelle mit angularjs

In dieser Aufgabe verwenden Sie **angularjs** , um die Clientseite der Geek Quiz-Anwendung zu implementieren. **Angularjs** ist ein Open-Source-JavaScript-Framework, das browserbasierte Anwendungen mit der Funktion " *Model-View-Controller* (MVC)" erweitert und sowohl die Entwicklung als auch das Testen ermöglicht.

Sie beginnen mit der Installation von angularjs über die Paket-Manager-Konsole von Visual Studio. Anschließend erstellen Sie den Controller, um das Verhalten der Geek-Quiz-APP und die Ansicht zum renderingfragen und Antworten mithilfe der angularjs-Vorlagen-Engine bereitzustellen.

> [!NOTE]
> Weitere Informationen zu angularjs finden Sie unter [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).

1. Öffnen Sie **Visual Studio Express 2013 für das Web** , und öffnen Sie die Projekt Mappe " **geekquiz. sln** ", die sich im Ordner " **Source/EX2-upatingaspinterface/BEGIN** " befindet. Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.
2. Öffnen Sie die **Paket-Manager-Konsole** über die **Tools** > **nuget-Paket-Manager**. Geben Sie den folgenden Befehl ein, um das nuget-Paket " **angularjs. Core** " zu installieren.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Scripts** des **geekquiz** -Projekts, und wählen Sie **hinzufügen aus. Neuer Ordner**. Benennen Sie die **App** , und drücken **Sie die Eingabe**Taste.
4. Klicken Sie mit der rechten Maustaste auf den soeben erstellten **App** -Ordner, und wählen Sie **Hinzufügen JavaScript-Datei**.

    ![Erstellen einer neuen JavaScript-Datei](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Erstellen einer neuen JavaScript-Datei*
5. Geben Sie im Dialogfeld **Name für Element angeben** *Quiz Controller* in das Textfeld **ElementName** ein, und klicken Sie auf **OK**.

    ![Benennen der neuen JavaScript-Datei](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Benennen der neuen JavaScript-Datei*
6. Fügen Sie in der Datei **Quiz-Controller. js** den folgenden Code hinzu, um den angularjs- **Quiz STRG** -Controller zu deklarieren und zu initialisieren.

    (Code Ausschnitt- *aspnetwebapispa-EX2-angularquiz Controller*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Die Konstruktorfunktion des **Quiz STRG** -Controllers erwartet einen injizierbaren Parameter mit dem Namen **$Scope**. Der anfängliche Zustand des Bereichs sollte in der Konstruktorfunktion eingerichtet werden, indem Eigenschaften an das **$Scope** Objekt angehängt werden. Die Eigenschaften enthalten das **Ansichts Modell**und sind für die Vorlage zugänglich, wenn der Controller registriert ist.
    > 
    > Der **Quiz STRG** -Controller wird in einem Modul mit dem Namen " **Quiz App**" definiert. Module sind Arbeitseinheiten, mit denen Sie Ihre Anwendung in separate Komponenten aufteilen können. Der Hauptvorteil der Verwendung von Modulen besteht darin, dass der Code leichter zu verstehen ist und Komponententests, Wiederverwendbarkeit und Wartbarkeit erleichtert.
7. Nun fügen Sie dem Bereich Verhalten hinzu, um auf Ereignisse zu reagieren, die in der Ansicht ausgelöst werden. Fügen Sie am Ende des **Quiz STRG** -Controllers den folgenden Code hinzu, um die **nextquestion** -Funktion im **$Scope** -Objekt zu definieren.

    (Code Ausschnitt- *aspnetwebapispa-EX2-angularquiz controllernextquestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Diese Funktion Ruft die nächste Frage aus der **Trivia** -Web-API ab, die in der vorherigen Übung erstellt wurde, und fügt die Frage Daten an das **$Scope** Objekt an.
8. Fügen Sie den folgenden Code am Ende des **Quiz STRG** -Controllers ein, um die **sendanswer** -Funktion im **$Scope** -Objekt zu definieren.

    (Code Ausschnitt- *aspnetwebapispa-EX2-angularquiz controllersendanswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Diese Funktion sendet die vom Benutzer ausgewählte Antwort an die **Trivia** -Web-API und speichert das Ergebnis – d. h., wenn die Antwort richtig oder nicht – im **$Scope** Objekt ist.
    > 
    > Die Funktionen **nextquestion** und **sendanswer** von oben verwenden das angularjs- **$http** Objekt, um die Kommunikation mit der Web-API über das XMLHttpRequest JavaScript-Objekt aus dem Browser zu abstrahieren. Angularjs unterstützt einen anderen Dienst, der eine höhere Abstraktions Ebene zur Durchführung von CRUD-Vorgängen für eine Ressource mithilfe von Rest-APIs bietet. Das angularjs- **$Resource** -Objekt verfügt über Aktionsmethoden, die Verhaltensweisen auf hoher Ebene bereitstellen, ohne mit dem **$http** -Objekt interagieren zu müssen. Verwenden Sie das **$Resource** -Objekt in Szenarien, die das CRUD-Modell benötigen (Informationen finden Sie in der [$Resource Dokumentation](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Der nächste Schritt besteht darin, die angularjs-Vorlage zu erstellen, die die Ansicht für das Quiz definiert. Öffnen Sie hierzu die Datei " **Index. cshtml** " in den **Ansichten | Basisordner,** und ersetzen Sie den Inhalt durch den folgenden Code.

    (Code Ausschnitt- *aspnetwebapispa-EX2-geekquiz View*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Die angularjs-Vorlage ist eine deklarative Spezifikation, bei der Informationen aus dem Modell und dem Controller verwendet werden, um ein statisches Markup in die dynamische Ansicht umzuwandeln, die der Benutzer im Browser sieht. Im folgenden finden Sie Beispiele für angularjs-Elemente und Element Attribute, die in einer Vorlage verwendet werden können:
    > 
    > - Die **ng-App-** Direktive weist angularjs das DOM-Element an, das das Stamm Element der Anwendung darstellt.
    > - Die **ng Controller-** Direktive fügt dem DOM einen Controller an dem Punkt an, an dem die Direktive deklariert ist.
    > - Die Schreibweise der geschweiften Klammer **{{}}** gibt Bindungen zu den im Controller definierten Bereichs Eigenschaften an.
    > - Die **ng Click-** Direktive wird verwendet, um die Funktionen aufzurufen, die als Reaktion auf Benutzer Klicks im Gültigkeitsbereich definiert sind.
10. Öffnen Sie die Datei " **Site. CSS** " im **Inhalts** Ordner, und fügen Sie am Ende der Datei die folgenden markierten Stile hinzu, um ein Aussehen und Gefühl für die Quiz Ansicht bereitzustellen.

    (Code Ausschnitt- *aspnetwebapispa-EX2-geekquiz Styles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2 – Ausführen der Lösung

In dieser Aufgabe führen Sie die Lösung mithilfe der neuen Benutzeroberfläche aus, die Sie mit angularjs erstellt haben, um einige der Quizfragen zu beantworten.

1. Drücken Sie **F5** , um die Projekt Mappe auszuführen.
2. Registrieren Sie ein neues Benutzerkonto. Führen Sie dazu die in Übung 1, Aufgabe 3 beschriebenen Registrierungsschritte aus.

    > [!NOTE]
    > Wenn Sie die Projekt Mappe aus der vorherigen Übung verwenden, können Sie sich mit dem Benutzerkonto anmelden, das Sie zuvor erstellt haben.
3. Die **Start** Seite sollte angezeigt werden, die die erste Frage des Quiz anzeigt. Beantworten Sie die Frage, indem Sie auf eine der Optionen klicken. Dadurch wird die zuvor definierte **sendanswer** -Funktion auslöst, die die ausgewählte Option an die **Trivia** -Web-API sendet.

    ![Beantworten einer Frage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Beantworten einer Frage")

    *Beantworten einer Frage*
4. Nachdem Sie auf eine der Schaltflächen geklickt haben, sollte die Antwort angezeigt werden. Klicken Sie auf **nächste Frage** , um die folgende Frage anzuzeigen. Dadurch wird die im Controller definierte **nextquestion** -Funktion auslöst.

    ![Nächste Frage wird angefordert.](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Nächste Frage wird angefordert.")

    *Nächste Frage wird angefordert.*
5. Die nächste Frage sollte angezeigt werden. Beantworten Sie weiterhin Fragen so oft wie Sie möchten. Nachdem Sie alle Fragen abgeschlossen haben, sollten Sie zur ersten Frage zurückkehren.

    ![Eine andere Frage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Eine andere Frage")

    *Nächste Frage*
6. Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Aufgabe 3 – Erstellen einer Flip-Animation mit CSS3

In dieser Aufgabe werden CSS3-Eigenschaften verwendet, um umfangreiche Animationen durchzuführen, wenn eine Frage beantwortet wird und die nächste Frage abgerufen wird.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den **Inhalts** Ordner des **geekquiz** -Projekts, und wählen Sie **hinzufügen aus. Vorhandenes Element...**

    ![Hinzufügen eines vorhandenen Elements zum Inhalts Ordner](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Hinzufügen eines vorhandenen Elements zum Inhalts Ordner")

    *Hinzufügen eines vorhandenen Elements zum Inhalts Ordner*
2. Navigieren Sie im Dialogfeld **Vorhandenes Element hinzufügen** zum Ordner **Source/Assets** , und wählen Sie **Flip. CSS**aus. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der Datei "Flip. CSS" aus Assets](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Hinzufügen der Datei "Flip. CSS" aus Assets")

    *Hinzufügen der Datei "Flip. CSS" aus Assets*
3. Öffnen Sie die soeben hinzugefügte Datei " **Flip. CSS** ", und überprüfen Sie Ihren Inhalt
4. Suchen Sie den Kommentar **Transformation für kippen** . In den Stilen unterhalb dieses Kommentars werden die CSS- **Perspektive** und **rotatey** -Transformationen verwendet, um einen &quot;Karten-&quot; Effekt zu generieren.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Suchen Sie das **Ausblenden des Bereichs im Flip** -Kommentar. Der folgende Stil blendet die Rückseite der Gesichter aus, wenn Sie vom Viewer entfernt werden, indem die CSS-Eigenschaft für die **Hintergrund Sichtbarkeit** auf *ausgeblendet*festgelegt wird.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Öffnen Sie die Datei **BundleConfig.cs** in der **App\_Ordner Start** , und fügen Sie den Verweis auf die Datei " **Flip. CSS** " im **&quot;~/Content/CSS&quot;** Style Bundle hinzu.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Drücken Sie **F5** , um die Projekt Mappe auszuführen und sich mit Ihren Anmelde Informationen anzumelden.
8. Beantworten Sie eine Frage, indem Sie auf eine der Optionen klicken. Beachten Sie beim Übergang zwischen Sichten den Flip-Effekt.

    ![Beantworten einer Frage mit dem Flip-Effekt](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Beantworten einer Frage mit dem Flip-Effekt")

    *Beantworten einer Frage mit dem Flip-Effekt*
9. Klicken Sie auf **nächste Frage** , um die folgende Frage abzurufen. Der Flip-Effekt sollte erneut angezeigt werden.

    ![Abrufen der folgenden Frage mit dem Flip-Effekt](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Abrufen der folgenden Frage mit dem Flip-Effekt")

    *Abrufen der folgenden Frage mit dem Flip-Effekt*

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch die Durchführung dieses praktischen Labs haben Sie Folgendes gelernt:

- Erstellen eines ASP.net-Web-API Controllers mithilfe von ASP.net-Gerüstbau
- Implementieren einer Web-API-Get-Aktion zum Abrufen der nächsten Quiz Frage
- Implementieren einer Web-API-Post-Aktion zum Speichern der Quiz Antworten
- Installieren von angularjs über die Visual Studio-Paket-Manager-Konsole
- Implementieren von angularjs-Vorlagen und-Controllern
- Verwenden von CSS3-Übergängen zum Durchführen von Animationseffekten
