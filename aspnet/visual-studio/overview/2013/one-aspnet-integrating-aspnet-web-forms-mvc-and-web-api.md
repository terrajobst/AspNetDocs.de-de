---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Praktische Übungseinheit: One ASP.net: Integration ASP.net Web Forms, MVC und Web API | Microsoft-Dokumentation'
author: rick-anderson
description: ASP.net ist ein Framework zum Aufbauen von Websites, Apps und Diensten mit spezialisierten Technologien wie MVC, Web-API und anderen. Mit der Erweiterung ASP.net h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505455"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Praktische Übungseinheit: One ASP.net: Integration ASP.net Web Forms, MVC und Web API

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

> ASP.net ist ein Framework zum Aufbauen von Websites, Apps und Diensten mit spezialisierten Technologien wie MVC, Web-API und anderen. Mit der Erweiterung ASP.net hat sich seit der Erstellung und der ausdrücklichen Notwendigkeit, diese Technologien **integriert zu haben**, bewährt
> 
> Visual Studio 2013 führt ein neues einheitliches Projekt System ein, mit dem Sie eine Anwendung erstellen und alle ASP.NET-Technologien in einem Projekt verwenden können. Durch dieses Feature entfällt die Notwendigkeit, eine Technologie zu Beginn eines Projekts auszuwählen und sich daran zu halten. stattdessen wird die Verwendung mehrerer ASP.NET-Frameworks in einem Projekt angeregt.
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit)verfügbar ist.

<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Erstellen einer Website basierend auf dem **One ASP.net** -Projekttyp
- Verwenden Sie unterschiedliche **ASP.net** -Frameworks wie **MVC** und die **Web-API** im gleichen Projekt.
- Identifizieren der Hauptkomponenten einer **ASP.net** -Anwendung
- Nutzen Sie das **ASP.net Gerüstbau** -Framework, um automatisch Controller und Sichten zum Ausführen von CRUD-Vorgängen basierend auf Ihren Modellklassen zu erstellen.
- Machen Sie denselben Satz von Informationen in maschinellen und lesbaren Formaten verfügbar, indem Sie das richtige Tool für jeden Auftrag verwenden.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [Erstellen eines neuen Web Forms Projekts](#Exercise1)
2. [Erstellen eines MVC-Controllers mithilfe von Gerüstbau](#Exercise2)
3. [Erstellen eines Web-API-Controllers mithilfe von Gerüstbau](#Exercise3)

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen. Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen. In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind. Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Übung 1: Erstellen eines neuen Web Forms Projekts

In dieser Übung erstellen Sie eine neue Web Forms Website in Visual Studio 2013 mithilfe der **ASP.net** Unified Project-Funktion, die es Ihnen ermöglicht, Web Forms-, MVC-und Web-API-Komponenten problemlos in dieselbe Anwendung zu integrieren. Sie werden dann die generierte Lösung durchsuchen und deren Teile identifizieren, und schließlich wird die Website in Aktion angezeigt.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Aufgabe 1 – Erstellen einer neuen Website mit einer ASP.net-Funktion

In dieser Aufgabe beginnen Sie mit dem Erstellen einer neuen Website in Visual Studio basierend auf dem **One ASP.net** -Projekttyp. **Eine ASP.net** vereinigt alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit, Sie nach Belieben zu mischen und abzugleichen. Anschließend erkennen Sie die verschiedenen Komponenten von Web Forms, MVC und der Web-API, die nebeneinander innerhalb Ihrer Anwendung nebeneinander stehen.

1. Öffnen Sie **Visual Studio Express 2013 für Web** , und wählen Sie Datei aus.  **Neues Projekt...** , um eine neue Projekt Mappe zu starten.

    ![Erstellen eines neuen Projekts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Erstellen eines neuen Projekts*
2. Wählen Sie im Dialogfeld **Neues Projekt** unter der Visualisierung **C# ASP.NET Webanwendung aus.** Registerkarte Web, und stellen Sie sicher, dass **.NET Framework 4,5** ausgewählt ist. Nennen Sie das Projekt *myhybridsite*, wählen Sie einen **Speicherort** aus, und klicken Sie auf **OK**.

    ![Neues ASP.NET-Webanwendungs Projekt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Erstellen eines neuen ASP.NET-Webanwendungs Projekts*
3. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die **Vorlage Web Forms** aus, und wählen Sie die Optionen **MVC** und **Web-API** aus. Stellen Sie außerdem sicher, dass die **Authentifizierungs** Option auf **einzelne Benutzerkonten**festgelegt ist. Klicken Sie zum Fortsetzen des Vorgangs auf **OK** .

    ![Erstellen eines neuen Projekts mit der Web Forms Vorlage, einschließlich Web-API und MVC-Komponenten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Erstellen eines neuen Projekts mit der Web Forms Vorlage, einschließlich Web-API und MVC-Komponenten*
4. Nun können Sie die Struktur der generierten Lösung untersuchen.

    ![Untersuchen der generierten Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Untersuchen der generierten Lösung*

    1. **Konto:** Dieser Ordner enthält die Web Form-Seiten für die Registrierung, die Anmeldung bei und die Verwaltung der Benutzerkonten der Anwendung. Dieser Ordner wird hinzugefügt, wenn die Authentifizierungs Option **einzelner Benutzerkonten** während der Konfiguration der Web Forms-Projektvorlage ausgewählt wird.
    2. **Modelle:** Dieser Ordner enthält die Klassen, die Ihre Anwendungsdaten darstellen.
    3. **Controller** und **Ansichten**: diese Ordner sind für die **ASP.NET MVC** -und **ASP.net-Web-API** -Komponenten erforderlich. In den nächsten Übungen erfahren Sie mehr über die MVC-und Web-API-Technologien.
    4. Die Dateien " **default. aspx**", " **Contact. aspx** " und " **about. aspx** " sind vordefinierte Web Form-Seiten, die Sie als Ausgangspunkt verwenden können, um die für die Anwendung spezifischen Seiten zu erstellen. Die Programmierlogik dieser Dateien befindet sich in einer separaten Datei, die als &quot;Code-Behind-&quot; Datei bezeichnet wird, die über eine &quot;. aspx. vb&quot;-oder &quot;. aspx.cs&quot;-Erweiterung (abhängig von der verwendeten Sprache) verfügt. Die Code-Behind-Logik wird auf dem Server ausgeführt und erzeugt dynamisch die HTML-Ausgabe für die Seite.
    5. Auf den Seiten " **Site. Master** " und " **Site. Mobile. Master** " werden das Aussehen und Verhalten und das Standardverhalten aller Seiten in der Anwendung definiert.
5. Doppelklicken Sie auf die Datei " **default. aspx** ", um den Inhalt der Seite zu durchsuchen.

    ![Untersuchen der Seite "default. aspx"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Untersuchen der Seite "default. aspx"*

    > [!NOTE]
    > Mit der **Page** -Direktive am Anfang der Datei werden die Attribute der Web Forms Seite definiert. Beispielsweise gibt das **MasterPageFile** -Attribut den Pfad zur Master Seite an, in diesem Fall die *Site. Master* -Seite und das erbt-Attribut, das die Code-Behind-Klasse für die zu **erbende** Seite definiert. Diese Klasse befindet sich in der Datei, die durch das **Code Behind** -Attribut bestimmt wird.
    > 
    > Das **ASP: Content** -Steuerelement enthält den eigentlichen Inhalt der Seite (Text, Markup und Steuerelemente) und wird einem **ASP: contentplachalter** -Steuerelement auf der Master Seite zugeordnet. In diesem Fall wird der Seiten Inhalt innerhalb des *mainContent* -Steuer Elements gerendert, das auf der Seite *Site. Master* definiert ist.
6. Erweitern Sie den Ordner **App\_Start** , und beachten Sie die Datei **WebApiConfig.cs** . Visual Studio enthielt diese Datei in die generierte Projekt Mappe, da Sie beim Konfigurieren Ihres Projekts mit der One ASP.net-Vorlage eine Web-API eingeschlossen haben.
7. Öffnen Sie die Datei **WebApiConfig.cs** . In der *webapiconfig* -Klasse finden Sie die Konfiguration, die der Web-API zugeordnet ist, die http-Routen zu **Web-API-Controllern**zuordnet.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Öffnen Sie die Datei **RouteConfig.cs** . In der *RegisterRoutes* -Methode finden Sie die Konfiguration, die MVC zugeordnet ist, mit der http-Routen zu **MVC-Controllern**zugeordnet werden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2 – Ausführen der Lösung

In dieser Aufgabe führen Sie die generierte Lösung aus, untersuchen die APP und einige ihrer Features, wie das Umschreiben von URLs und die integrierte Authentifizierung.

1. Drücken Sie **F5** , oder klicken Sie auf die Schaltfläche **Start** auf der Symbolleiste, um die Projekt Mappe auszuführen. Die Startseite der Anwendung sollte im Browser geöffnet werden.

    ![Ausführen der Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Überprüfen Sie, ob die Web Forms Seiten aufgerufen werden. Fügen Sie zu diesem Zweck **/Contact.aspx** an die URL in der Adressleiste an, und drücken **Sie die Eingabe**Taste.

    ![Friendly URLs](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Friendly URLs*

    > [!NOTE]
    > Wie Sie sehen können, wird die URL in **/Contact**geändert. Ab **ASP.NET 4**wurden die URL-Routing Funktionen Web Forms hinzugefügt, sodass Sie URLs wie *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* anstelle von *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* schreiben können. Weitere Informationen finden Sie unter [URL-Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Sie werden nun den in die Anwendung integrierten Authentifizierungs Fluss untersuchen. Klicken Sie hierzu in der oberen rechten Ecke der Seite auf **registrieren** .

    ![Registrieren eines neuen Benutzers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrieren eines neuen Benutzers*
4. Geben Sie auf der **Register** Seite einen **Benutzernamen** und ein **Kennwort**ein, und klicken Sie dann auf **registrieren**.

    ![Seite registrieren](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Seite registrieren*
5. Die Anwendung registriert das neue Konto, und der Benutzer wird authentifiziert.

    ![Benutzer authentifiziert](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Benutzer authentifiziert*
6. Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Übung 2: Erstellen eines MVC-Controllers mithilfe eines Gerüsts

In dieser Übung nutzen Sie das ASP.net Gerüstbau-Framework, das von Visual Studio bereitgestellt wird, um einen ASP.NET MVC 5-Controller mit Aktionen und Razor-Ansichten zum Durchführen von CRUD-Vorgängen zu erstellen, ohne eine einzige Codezeile zu schreiben. Der Gerüstbau Prozess verwendet Entity Framework Code First, um den Datenkontext und das Datenbankschema in der SQL-Datenbank zu generieren.

**Informationen zu Entity Framework Code First**

Entity Framework (EF) ist ein Objekt relationaler Mapper (ORM), mit dem Sie Datenzugriffs Anwendungen erstellen können, indem Sie mit einem konzeptionellen Anwendungsmodell programmieren, anstatt direkt mithilfe eines relationalen Speicher Schemas zu programmieren.

Der Workflow für die Entity Framework Code First Modellierung ermöglicht Ihnen die Verwendung ihrer eigenen Domänen Klassen, um das Modell darzustellen, das EF beim Ausführen von Abfragen, Änderungs Nachverfolgung und Aktualisierungs Funktionen verwendet. Wenn Sie den Code First Entwicklungs Workflow verwenden, müssen Sie die Anwendung nicht starten, indem Sie eine Datenbank erstellen oder ein Schema angeben. Stattdessen können Sie .net-Standardklassen schreiben, die die am besten geeigneten Domänen Modell Objekte für Ihre Anwendung definieren, und Entity Framework die Datenbank für Sie erstellen.

> [!NOTE]
> Weitere Informationen zu Entity Framework finden Sie [hier](../../../entity-framework.md).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Aufgabe 1 – Erstellen eines neuen Modells

Nun definieren Sie eine **Person** -Klasse, bei der es sich um das Modell handelt, das vom Gerüstbau Prozess zum Erstellen des MVC-Controllers und der Ansichten verwendet wird. Zunächst erstellen Sie eine **Person** -Modell Klasse, und die CRUD-Vorgänge im Controller werden automatisch mithilfe von Gerüstbau Features erstellt.

1. Öffnen Sie **Visual Studio Express 2013 für das Web** und die Projekt Mappe " **myhybridsite. sln** ", die sich im Ordner " **Source/EX2-mvcgerüst/BEGIN** " befindet. Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.
2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Models** des Projekts **myhybridsite** , und wählen Sie **hinzufügen aus. Klasse...**

    ![Hinzufügen der Person-Modell Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Hinzufügen der Person-Modell Klasse*
3. Benennen Sie im Dialogfeld **Neues Element hinzufügen** die Datei *Person.cs* , und klicken Sie auf **Hinzufügen**.

    ![Erstellen der Person-Modell Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Erstellen der Person-Modell Klasse*
4. Ersetzen Sie den Inhalt der **Person.cs** -Datei durch den folgenden Code. Drücken Sie **STRG + S** , um die Änderungen zu speichern.

    (Code Ausschnitt- *bringingtogetheroneaspnet-EX2-Personclass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **myhybridsite** , und wählen Sie **Erstellen**aus, oder drücken Sie **STRG + UMSCHALT + B** , um das Projekt zu erstellen.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Aufgabe 2 – Erstellen eines MVC-Controllers

Nachdem das **Person** -Modell erstellt wurde, verwenden Sie ASP.NET MVC-Gerüstbau mit Entity Framework, um die CRUD-Controller Aktionen und-Ansichten für **Person**zu erstellen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Controllers** des Projekts **myhybridsite** , und wählen Sie **Hinzufügen | Neues Gerüst Element...**

    ![Erstellen eines neuen Gerüst Controllers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Erstellen eines neuen Gerüst Controllers*
2. Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **MVC 5-Controller mit Ansichten unter Verwendung Entity Framework** aus, und klicken Sie dann auf **hinzufügen.**

    ![Auswählen von MVC 5-Controller mit Ansichten und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Auswählen von MVC 5-Controller mit Ansichten und Entity Framework*
3. Legen Sie *mvcpersoncontroller* als **Controller Namen**fest, wählen Sie die Option **Async Controller Actions verwenden** aus, und wählen Sie **Person (myhybridsite. Models)** als **Modell Klasse**aus.

    ![Hinzufügen eines MVC-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Hinzufügen eines MVC-Controllers mit Gerüstbau*
4. Klicken Sie unter **Datenkontext Klasse**auf **neuer Datenkontext...** .

    ![Erstellen eines neuen Daten Kontexts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Erstellen eines neuen Daten Kontexts*
5. Benennen Sie im Dialogfeld **neuer Datenkontext** den neuen Datenkontext *personcontext* , und klicken Sie auf **Hinzufügen**.

    ![Erstellen des neuen personcontext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Erstellen des neuen personcontext-Typs*
6. Klicken Sie auf **Hinzufügen** , um den neuen Controller für **Person** mit Gerüst zu erstellen. Visual Studio generiert dann die Controller Aktionen, den Datenkontext der Person und die Razor-Ansichten.

    ![Nach dem Erstellen des MVC-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Nach dem Erstellen des MVC-Controllers mit Gerüstbau*
7. Öffnen Sie die Datei **MvcPersonController.cs** im Ordner **Controllers** . Beachten Sie, dass die CRUD-Aktionsmethoden automatisch generiert wurden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Durch Aktivieren des Kontrollkästchens **Async-Controller Aktionen** aus den Gerüstbau Optionen in den vorherigen Schritten generiert Visual Studio asynchrone Aktionsmethoden für alle Aktionen, die Zugriff auf den Person-Datenkontext einschließen. Es wird empfohlen, asynchrone Aktionsmethoden für lang ausgeführte, nicht CPU-gebundene Anforderungen zu verwenden, um zu verhindern, dass der Webserver während der Verarbeitung der Anforderung funktioniert.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3 – Ausführen der Lösung

In dieser Aufgabe führen Sie die Projekt Mappe erneut aus, um zu überprüfen, ob die Ansichten für die **Person** erwartungsgemäß funktionieren. Sie werden eine neue Person hinzufügen, um zu überprüfen, ob Sie erfolgreich in der Datenbank gespeichert wurde.

1. Drücken Sie **F5** , um die Projekt Mappe auszuführen.
2. Navigieren Sie zu **/MvcPerson**. Die Gerüst Ansicht, die die Liste der Personen anzeigt, sollte angezeigt werden.
3. Klicken Sie auf **neu erstellen** , um eine neue Person hinzuzufügen.

    ![Navigieren zu den dashboarded-MVC-Ansichten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navigieren zu den dashboarded-MVC-Ansichten*
4. Geben Sie in der Ansicht **Erstellen** einen **Namen** und ein **Alter** für die Person an, und klicken Sie auf **Erstellen**.

    ![Hinzufügen einer neuen Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Hinzufügen einer neuen Person*
5. Die neue Person wird der Liste hinzugefügt. Klicken Sie in der Liste Element auf **Details** , um die Detailansicht der Person anzuzeigen. Klicken Sie dann in der **Detail** Ansicht auf **zurück zur Liste** , um zur Listenansicht zurückzukehren.

    ![Detailansicht der Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Detailansicht der Person*
6. Klicken Sie auf den Link **Löschen** , um die Person zu löschen. Klicken Sie in der Ansicht **Löschen** auf **Löschen** , um den Vorgang zu bestätigen.

    ![Löschen einer Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Löschen einer Person*
7. Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Übung 3: Erstellen eines Web-API-Controllers mithilfe von Gerüstbau

Das Web-API-Framework ist Teil des ASP.net-Stapels und wurde entwickelt, um die Implementierung von http-Diensten zu vereinfachen und im allgemeinen JSON-oder XML-formatierte Daten über eine restliche API zu senden und zu empfangen

In dieser Übung verwenden Sie ASP.net Gerüstbau erneut, um einen Web-API-Controller zu generieren. Sie verwenden dieselben **Person** -und **personcontext** -Klassen aus der vorherigen Übung, um dieselben Personendaten im JSON-Format bereitzustellen. Sie werden sehen, wie Sie dieselben Ressourcen auf verschiedene Weise in derselben ASP.NET-Anwendung verfügbar machen können.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Aufgabe 1 – Erstellen eines Web-API-Controllers

In dieser Aufgabe erstellen Sie einen neuen **Web-API-Controller** , der die Personendaten in einem Computer nutzbaren Format wie JSON verfügbar macht.

1. Wenn Sie nicht bereits geöffnet ist, öffnen Sie **Visual Studio Express 2013 für das Web** , und öffnen Sie die Projekt Mappe " **myhybridsite. sln** ", die sich im Ordner **Quell/EX3-WebAPI/Anfang** befindet. Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.

    > [!NOTE]
    > Wenn Sie mit der BEGIN-Projekt Mappe aus Übung 3 beginnen, drücken Sie **STRG + UMSCHALT + B** , um die Projekt Mappe zu erstellen.
2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Controllers** des Projekts **myhybridsite** , und wählen Sie **Hinzufügen | Neues Gerüst Element...**

    ![Erstellen eines neuen Gerüst Controllers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Erstellen eines neuen Gerüst Controllers*
3. Wählen Sie im Dialogfeld **Gerüst hinzufügen** im linken Bereich **Web-API** und dann **Web-API 2-Controller mit Aktionen Entity Framework** im mittleren Bereich aus, und klicken Sie dann auf **hinzufügen.**

    ![Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework")

    *Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework*
4. Legen Sie *apipersoncontroller* als **Controller Namen**fest, wählen Sie die Option **Async Controller Actions verwenden** aus, und wählen Sie **Person (myhybridsite. Models)** und **personcontext (myhybridsite. Models)** als **Modell** -bzw. **Datenkontext** Klassen aus. Klicken Sie anschließend auf **Hinzufügen**.

    ![Hinzufügen eines Web-API-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Hinzufügen eines Web-API-Controllers mit Gerüstbau")

    *Hinzufügen eines Web-API-Controllers mit Gerüstbau*
5. Visual Studio generiert dann die **apipersoncontroller** -Klasse mit den vier CRUD-Aktionen, um mit Ihren Daten zu arbeiten.

    ![Nach dem Erstellen des Web-API-Controllers mit Gerüst](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Nach dem Erstellen des Web-API-Controllers mit Gerüst")

    *Nach dem Erstellen des Web-API-Controllers mit Gerüst*
6. Öffnen Sie die Datei **ApiPersonController.cs** , und überprüfen Sie die *GetPeople* -Aktionsmethode. Diese Methode fragt das Datenbankfeld des **personcontext** -Typs ab, um die Personendaten zu erhalten.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Beachten Sie nun den Kommentar oberhalb der Methoden Definition. Sie stellt den URI bereit, der diese Aktion verfügbar macht, die Sie in der nächsten Aufgabe verwenden werden.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Die Web-API ist standardmäßig so konfiguriert, dass die Abfragen im */API* -Pfad abgefangen werden, um Konflikte mit MVC-Controllern zu vermeiden. Wenn Sie diese Einstellung ändern müssen, finden Sie weitere Informationen unter [Routing in ASP.net-Web-API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2 – Ausführen der Lösung

In dieser Aufgabe verwenden Sie die Internet Explorer **F12-Entwicklertools** , um die vollständige Antwort vom Web-API-Controller zu überprüfen. Sie werden erfahren, wie Sie Netzwerk Datenverkehr erfassen, um einen besseren Einblick in Ihre Anwendungsdaten zu erhalten.

> [!NOTE]
> Stellen Sie sicher, dass **Internet Explorer** in der Schaltfläche **Start** auf der Symbolleiste von Visual Studio ausgewählt ist.
> 
> ![Internet Explorer (Option)](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Die **F12-Entwicklertools** verfügen über eine Vielzahl von Funktionen, die in dieser praktischen Übungseinheit nicht behandelt werden. Weitere Informationen hierzu finden Sie unter [Verwenden der F12-Entwicklertools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).

1. Drücken Sie **F5** , um die Projekt Mappe auszuführen.

    > [!NOTE]
    > Damit diese Aufgabe ordnungsgemäß ausgeführt werden kann, muss Ihre Anwendung über Daten verfügen. Wenn die Datenbank leer ist, können Sie zu Aufgabe 3 in Übung 2 zurückkehren und die Schritte zum Erstellen einer neuen Person mithilfe der MVC-Ansichten ausführen.
2. Drücken Sie im Browser die Taste **F12** , um den **Entwicklertools** Bereich zu öffnen. Drücken Sie **STRG** + **4** , oder klicken Sie auf das **Netzwerk** Symbol, und klicken Sie dann auf die grüne Pfeiltaste, um den Netzwerk Datenverkehr zu erfassen.

    ![Initiieren der Web-API-Netzwerk Erfassung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiieren der Web-API-Netzwerk Erfassung")

    *Initiieren der Web-API-Netzwerk Erfassung*
3. Fügen Sie **API/apiperson** an die URL in der Adressleiste des Browsers an. Nun überprüfen Sie die Details der Antwort von **apipersoncontroller**.

    ![Abrufen von Personendaten über die Web-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Abrufen von Personendaten über die Web-API")

    *Abrufen von Personendaten über die Web-API*

    > [!NOTE]
    > Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei durchführen. Lassen Sie das Dialogfeld geöffnet, um den Antwort Inhalt über das Tool Fenster "Entwickler" anzeigen zu können.
4. Nun überprüfen Sie den Text der Antwort. Klicken Sie hierzu auf die Registerkarte **Details** , und klicken Sie dann auf **Antworttext**. Sie können überprüfen, ob die heruntergeladenen Daten eine Liste von Objekten mit der Eigenschaften- **ID**, dem **Namen** und dem **Alter** der **Person** -Klasse sind.

    ![Anzeigen des Web-API-Antwort Texts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Anzeigen des Web-API-Antwort Texts")

    *Anzeigen des Web-API-Antwort Texts*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Aufgabe 3 – Hinzufügen von Web-API-Hilfeseiten

Wenn Sie eine Web-API erstellen, ist es hilfreich, eine Hilfeseite zu erstellen, damit andere Entwickler wissen, wie Sie Ihre API aufzurufen. Sie könnten die Dokumentationsseiten manuell erstellen und aktualisieren, aber es ist besser, Sie automatisch zu generieren, um eine Wartung zu vermeiden. In dieser Aufgabe verwenden Sie ein nuget-Paket zum automatischen Generieren von Web-API-Hilfeseiten für die Lösung.

1. Wählen Sie **im Menü Extras** in Visual Studio den **nuget-Paket-Manager**aus, und klicken Sie dann auf Paket-Manager- **Konsole**.
2. Führen Sie im Fenster der **Paket-Manager-Konsole** den folgenden Befehl aus:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Das Paket **Microsoft. Aspnet. WebAPI. helppage** installiert die erforderlichen Assemblys und fügt MVC-Ansichten für die Hilfeseiten im Ordner **Bereiche/helppage** hinzu.

    ![Bereich "helppage"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Bereich "helppage"")

    *Bereich "helppage"*
3. Standardmäßig verfügen die Hilfeseiten über Platzhalter Zeichenfolgen für die Dokumentation. Sie können XML-Dokumentations Kommentare verwenden, um die Dokumentation zu erstellen. Um dieses Feature zu aktivieren, öffnen Sie die Datei **HelpPageConfig.cs** , die sich im Ordner **Bereiche/helppage/App\_Start** befindet, und heben Sie die Auskommentierung der folgenden Zeile auf:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **myhybridsite**, wählen Sie **Eigenschaften** , und klicken Sie auf die Registerkarte **Erstellen** .

    ![Registerkarte Erstellen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Abschnitt "Build"")

    *Registerkarte Erstellen*
5. Wählen Sie unter **Ausgabe**die Option **XML-Dokumentations Datei**aus. Geben Sie im Bearbeitungsfeld **App\_Data/XmlDocument. XML**ein.

    ![Ausgabe Abschnitt der Registerkarte "Build"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Ausgabe Abschnitt der Registerkarte "Build"")

    *Ausgabe Abschnitt der Registerkarte "Build"*
6. Drücken Sie **STRG** + **S** , um die Änderungen zu speichern.
7. Öffnen Sie die Datei **ApiPersonController.cs** aus dem Ordner **Controllers** .
8. Geben Sie eine neue Zeile zwischen der *GetPeople* -Methoden Signatur und dem Kommentar *//Get API/apiperson* ein, und geben Sie dann drei Schrägstriche ein.

    > [!NOTE]
    > Visual Studio fügt automatisch die XML-Elemente ein, die die Methoden Dokumentation definieren.
9. Fügen Sie einen Übersichts Text und den Rückgabewert für die *GetPeople* -Methode hinzu. Es sollte wie folgt aussehen.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Drücken Sie **F5** , um die Projekt Mappe auszuführen.
11. Fügen Sie **/Help** an die URL in der Adressleiste an, um zur Hilfeseite zu navigieren.

    ![ASP.net-Web-API Hilfeseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.net-Web-API Hilfeseite")

    *ASP.net-Web-API Hilfeseite*

    > [!NOTE]
    > Der Hauptinhalt der Seite ist eine Tabelle mit APIs, gruppiert nach Controller. Die Tabelleneinträge werden mithilfe der **iapiexplorer** -Schnittstelle dynamisch generiert. Wenn Sie einen API-Controller hinzufügen oder aktualisieren, wird die Tabelle automatisch aktualisiert, wenn Sie das nächste Mal die Anwendung erstellen.
    > 
    > In der Spalte **API** werden die HTTP-Methode und der relative URI aufgelistet. Die Spalte **Beschreibung** enthält Informationen, die aus der Dokumentation der Methode extrahiert wurden.
12. Beachten Sie, dass die Beschreibung, die Sie oberhalb der Methoden Definition hinzugefügt haben, in der Spalte Beschreibung angezeigt wird.

    ![API-Methoden Beschreibung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API-Methoden Beschreibung")

    *API-Methoden Beschreibung*
13. Klicken Sie auf eine API-Methode, um zu einer Seite mit detaillierteren Informationen zu navigieren, einschließlich Beispiel Antworttext.

    ![Detail Informationen (Seite)](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Informationen (Seite)")

    *Seite mit detaillierten Informationen*

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch die Durchführung dieses praktischen Labs haben Sie Folgendes gelernt:

- Erstellen Sie eine neue Webanwendung mit der ASP.net-Funktion in Visual Studio 2013
- Integrieren mehrerer ASP.NET-Technologien in ein einzelnes Projekt
- Generieren von MVC-Controllern und-Sichten aus ihren Modellklassen mithilfe von ASP.net
- Generieren von Web-API-Controllern, die Funktionen wie die asynchrone Programmierung und den Datenzugriff über Entity Framework verwenden
- Automatisches Generieren von Web-API-Hilfeseiten für Ihre Controller
