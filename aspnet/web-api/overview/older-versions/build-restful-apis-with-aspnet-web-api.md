---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Erstellen von Rest-APIs mit ASP.net-Web-API-ASP.NET 4. x
author: rick-anderson
description: 'Praktische Übungseinheit: Verwenden Sie die Web-API in ASP.NET 4. x, um eine einfache Rest-API für eine Contact Manager-Anwendung zu erstellen.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504267"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a>Erstellen von Rest-APIs mit ASP.net-Web-API

vom [Web Camps-Team](https://twitter.com/webcamps)

> Praktische Übungseinheit: Verwenden Sie die Web-API in ASP.NET 4. x, um eine einfache Rest-API für eine Contact Manager-Anwendung zu erstellen. Außerdem erstellen Sie einen Client, um die API zu nutzen.

In den letzten Jahren wurde deutlich, dass HTTP nicht nur für die Einrichtung von HTML-Seiten vorgesehen ist. Außerdem handelt es sich um eine leistungsfähige Plattform zum Entwickeln von Web-APIs, die eine Reihe von Verben (Get, Post usw.) sowie einige einfache Konzepte wie *URIs* und *Header*verwendet. ASP.net-Web-API ist ein Satz von Komponenten, die die HTTP-Programmierung vereinfachen. Da es auf der MVC-Laufzeit ASP.NET basiert, verarbeitet die Web-API automatisch die Transportdetails auf niedriger Ebene von http. Gleichzeitig stellt die Web-API natürlich das HTTP-Programmiermodell zur Verfügung. Ein Ziel der Web-API besteht darin, die Realität von http *nicht* zu abstrahieren. Folglich ist die Web-API sowohl flexibel als auch einfach zu erweitern.  Der Rest-Architekturstil ist als effektive Methode für die Verwendung von http erwiesen, obwohl dies sicherlich nicht der einzige gültige Ansatz für http ist. Der Kontakt-Manager macht unter anderem den Rest für das auflisten, hinzufügen und Entfernen von Kontakten verfügbar. 

Diese Übungseinheit erfordert grundlegende Kenntnisse von http und Rest und geht davon aus, dass Sie über grundlegende Kenntnisse in HTML, JavaScript und jQuery verfügen.
> 
> > [!NOTE]
> > Die ASP.NET-Website verfügt über einen Bereich, der für das ASP.net-Web-API Framework bei [https://asp.net/web-api](https://asp.net/web-api)reserviert ist. Diese Site liefert weiterhin aktuelle Informationen, Beispiele und Neuigkeiten im Zusammenhang mit der Web-API. Überprüfen Sie Sie häufig, wenn Sie die Art der Erstellung von benutzerdefinierten Web-APIs vertiefen möchten, die für praktisch alle Geräte oder Entwicklungs-Frameworks verfügbar sind.
> > 
> > ASP.net-Web-API, ähnlich wie ASP.NET MVC 4, bietet eine hohe Flexibilität in Bezug auf die Trennung der Dienst Ebene von den Controllern, sodass Sie mehrere der verfügbaren Frameworks für die Abhängigkeitsinjektion recht einfach verwenden können. Es gibt ein gutes Beispiel in MSDN, das zeigt, wie Sie Ninject für die Abhängigkeitsinjektion in einem ASP.net-Web-API Projekt verwenden, das Sie [hier](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)herunterladen können.
> 
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)verfügbar ist.

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Implementieren einer Rest-Web-API
- API von einem HTML-Client aus abrufen

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang B](#AppendixB) ).

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

**Installieren von Code Ausschnitten**

Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist. Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang A: Verwenden von Code Ausschnitten](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Schritte:

1. [Übung 1: Erstellen einer schreibgeschützten Web-API](#Exercise1)
2. [Übung 2: Erstellen einer Web-API mit Lese-/Schreibzugriff](#Exercise2)
3. [Übung 3: Verwenden der Web-API aus einem HTML-Client](#Exercise3)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Übung 1: Erstellen einer schreibgeschützten Web-API

In dieser Übung implementieren Sie die schreibgeschützten Get-Methoden für den Kontakt-Manager.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Aufgabe 1: Erstellen des API-Projekts

In dieser Aufgabe verwenden Sie die neuen ASP.NET-Webprojekt Vorlagen, um eine Web-API-Webanwendung zu erstellen.

1. Führen Sie **Visual Studio 2012 Express für das Web aus**. wechseln Sie zu **Start** , und geben Sie **vs Express für Web** und drücken Sie dann die **Eingabe**Taste.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt**aus. Wählen Sie **das C# visuelle Element aus |** Webprojekttyp aus der Strukturansicht Projekttyp, und wählen Sie dann den Projekttyp **ASP.NET MVC 4 Webanwendung** Legen Sie den **Namen** des Projekts auf *ContactManager* und den Projektmappennamen auf Start *fest, und*klicken Sie dann auf **OK**.

    ![Erstellen eines neuen ASP.NET MVC 4,0-Webanwendungs Projekts](build-restful-apis-with-aspnet-web-api/_static/image1.png "Erstellen eines neuen ASP.NET MVC 4,0-Webanwendungs Projekts")

    *Erstellen eines neuen ASP.NET MVC 4,0-Webanwendungs Projekts*
3. Wählen Sie im Dialogfeld ASP.NET MVC 4 Project Type den Typ **Web-API** -Projekt aus. Klicken Sie auf **OK**.

    ![Angeben des Web-API-Projekt Typs](build-restful-apis-with-aspnet-web-api/_static/image2.png "Angeben des Web-API-Projekt Typs")

    *Angeben des Web-API-Projekt Typs*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Aufgabe 2: Erstellen der Kontakt-Manager-API-Controller

In dieser Aufgabe erstellen Sie die Controller Klassen, in denen sich API-Methoden befinden.

1. Löschen Sie die Datei mit dem Namen **ValuesController.cs** im Ordner **Controllers** aus dem Projekt.
2. Klicken Sie im Projekt mit der rechten Maustaste auf den Ordner **Controller** , und wählen Sie **Hinzufügen | Controller** aus dem Kontextmenü.

    ![Dem Projekt wird ein neuer Controller hinzugefügt.](build-restful-apis-with-aspnet-web-api/_static/image3.png "Dem Projekt wird ein neuer Controller hinzugefügt.")

    *Dem Projekt wird ein neuer Controller hinzugefügt.*
3. Wählen Sie im angezeigten Dialogfeld **Controller hinzufügen** im Menüvorlage den Eintrag **leerer API-Controller** aus. Benennen Sie die Controller-Klasse **ContactController**. Klicken Sie dann auf **hinzufügen.**

    ![Verwenden des Dialog Felds "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image4.png "Verwenden des Dialog Felds "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers")

    *Verwenden des Dialog Felds "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers*
4. Fügen Sie den folgenden Code zu **ContactController**hinzu.

    (Code Ausschnitt- *Web-API Lab-Ex01-Get API-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Drücken Sie **F5**, um die Anwendung zu debuggen. Die Standard Startseite für ein Web-API-Projekt sollte angezeigt werden.

    ![Die Standard Startseite einer ASP.net-Web-API Anwendung](build-restful-apis-with-aspnet-web-api/_static/image5.png "Die Standard Startseite einer ASP.net-Web-API Anwendung")

    *Die Standard Startseite einer ASP.net-Web-API Anwendung*
6. Drücken Sie im Internet Explorer-Fenster die Taste **F12** , um das Fenster **Entwicklertools** zu öffnen. Klicken Sie auf die Registerkarte **Netzwerk** , und klicken Sie dann auf die Schaltfläche **Erfassung starten** , um den Netzwerk Datenverkehr im Fenster zu erfassen.

    ![Öffnen der Registerkarte "Netzwerk" und Initiieren der Netzwerk Erfassung](build-restful-apis-with-aspnet-web-api/_static/image6.png "Öffnen der Registerkarte "Netzwerk" und Initiieren der Netzwerk Erfassung")

    *Öffnen der Registerkarte "Netzwerk" und Initiieren der Netzwerk Erfassung*
7. Fügen Sie die URL in der Adressleiste des Browsers mit **/API/Contact** an, und drücken Sie die EINGABETASTE. Die Übertragungs Details werden im Fenster Netzwerk Erfassung angezeigt. Beachten Sie, dass der MIME-Typ der Antwort " **Application/JSON**" lautet. Dies zeigt, wie das Standardausgabeformat JSON ist.

    ![Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk Ansicht](build-restful-apis-with-aspnet-web-api/_static/image7.png "Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk Ansicht")

    *Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk Ansicht*

    > [!NOTE]
    > Das Standardverhalten von Internet Explorer 10 besteht darin, zu Fragen, ob der Benutzer den Stream, der sich aus dem Web-API-Befehl ergibt, speichern oder öffnen möchte. Bei der Ausgabe handelt es sich um eine Textdatei mit dem JSON-Ergebnis des Web-API-URL-Aufrufes. Brechen Sie das Dialogfeld nicht ab, um den Inhalt der Antwort über das Tool Fenster "Entwickler" anzeigen zu können.
8. Klicken Sie auf die Schaltfläche **Gehe zu ausführlicher Ansicht** , um weitere Details zur Antwort dieses API-Aufrufes anzuzeigen.

    ![Zur detaillierten Ansicht wechseln](build-restful-apis-with-aspnet-web-api/_static/image8.png "Zur Detailansicht wechseln")

    *Zur detaillierten Ansicht wechseln*
9. Klicken Sie auf die Registerkarte **Antwort** Text, um den tatsächlichen JSON-Antworttext anzuzeigen.

    ![Anzeigen des JSON-Ausgabe Texts im Netzwerkmonitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Anzeigen des JSON-Ausgabe Texts im Netzwerkmonitor")

    *Anzeigen des JSON-Ausgabe Texts im Netzwerkmonitor*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Aufgabe 3: Erstellen der Kontakt Modelle und Erweitern des Kontakt Controllers

In dieser Aufgabe erstellen Sie die Controller Klassen, in denen sich API-Methoden befinden.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , **und wählen Sie Klasse...** über das Kontextmenü.

    ![Hinzufügen eines neuen Modells zur Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image10.png "Hinzufügen eines neuen Modells zur Webanwendung")

    *Hinzufügen eines neuen Modells zur Webanwendung*
2. Benennen Sie im Dialogfeld **Neues Element hinzufügen** die neue Datei **Contact.cs** , und klicken Sie auf **hinzufügen.**

    ![Erstellen der neuen Contact-Klassendatei](build-restful-apis-with-aspnet-web-api/_static/image11.png "Erstellen der neuen Contact-Klassendatei")

    *Erstellen der neuen Contact-Klassendatei*
3. Fügen Sie der **Contact** -Klasse den folgenden markierten Code hinzu.

    (Code Ausschnitt- *Web-API Lab-Ex01-Contact-Klasse*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. Wählen Sie in der **ContactController** -Klasse die Wort **Zeichenfolge** in der Methoden Definition der **Get** -Methode aus, und geben Sie das Wort *Contact*ein. Nachdem das Wort eingegeben wurde, wird am Anfang des Word- **Kontakts**ein Indikator angezeigt. Halten Sie die **STRG** -Taste gedrückt, und drücken Sie die Taste (.), oder klicken Sie mit der Maus auf das Symbol, um das Dialogfeld "Hilfe" im Code-Editor zu öffnen, um die **using** -Direktive für den Namespace "Models" automatisch auszufüllen.

    ![Verwenden der IntelliSense-Unterstützung für Namespace Deklarationen](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Verwenden der IntelliSense-Unterstützung für Namespace Deklarationen*
5. Ändern Sie den Code für die **Get** -Methode, sodass Sie ein Array von Kontakt Modell Instanzen zurückgibt.

    (Code Ausschnitt- *Web-API Lab-Ex01-Rückgabe einer Liste von Kontakten*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Drücken Sie **F5** , um die Webanwendung im Browser zu debuggen. Führen Sie die folgenden Schritte aus, um die Änderungen an der Antwort Ausgabe der API anzuzeigen.

   1. Nachdem der Browser geöffnet wurde, drücken Sie **F12** , wenn die Entwicklertools noch nicht geöffnet sind.
   2. Klicken Sie auf die Registerkarte **Netzwerk** .
   3. Klicken Sie auf die Schaltfläche **Erfassung starten** .
   4. Fügen Sie das URL-Suffix **/API/Contact** der URL in der Adressleiste hinzu, und drücken **Sie die Eingabe** Taste.
   5. Klicken Sie auf die Schaltfläche **Gehe zu ausführlicher Ansicht** .
   6. Wählen Sie die Registerkarte **Antworttext** aus. Es sollte eine JSON-Zeichenfolge angezeigt werden, die die serialisierte Form eines Arrays von Kontakt Instanzen darstellt.

      ![JSON-serialisierte Ausgabe eines komplexen Web-API-Methoden Aufrufes](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON-serialisierte Ausgabe eines komplexen Web-API-Methoden Aufrufes")

      *JSON-serialisierte Ausgabe eines komplexen Web-API-Methoden Aufrufes*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Aufgabe 4: Extrahieren von Funktionen in eine Dienst Ebene

Diese Aufgabe veranschaulicht, wie Sie Funktionen in eine Dienst Ebene extrahieren, um Entwicklern das Trennen ihrer Dienst Funktionalität von der Controller Schicht zu erleichtern. so wird die Wiederverwendbarkeit der Dienste ermöglicht, die tatsächlich die Arbeit erledigen.

1. Erstellen Sie im Projektmappenstamm einen neuen Ordner, und nennen Sie ihn **Services**. Klicken Sie hierzu mit der rechten Maustaste auf das Projekt **ContactManager** , wählen Sie | **neuen Ordner** **Hinzufügen** aus, und nennen Sie ihn *Services*.

    ![Ordner zum Erstellen von Diensten](build-restful-apis-with-aspnet-web-api/_static/image14.png "Ordner zum Erstellen von Diensten")

    *Ordner zum Erstellen von Diensten*
2. Klicken Sie mit der rechten Maustaste auf den Ordner **Dienste** und wählen Sie **Hinzufügen Klasse...** über das Kontextmenü.

    ![Hinzufügen einer neuen Klasse zum Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image15.png "Hinzufügen einer neuen Klasse zum Ordner "Dienste"")

    *Hinzufügen einer neuen Klasse zum Ordner "Dienste"*
3. Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, nennen Sie die neue Klasse **contactrepository** , und klicken Sie auf **Hinzufügen**.

    ![Erstellen einer Klassendatei, die den Code für die Dienst Ebene des Contact-Repository enthält](build-restful-apis-with-aspnet-web-api/_static/image16.png "Erstellen einer Klassendatei, die den Code für die Dienst Ebene des Contact-Repository enthält")

    *Erstellen einer Klassendatei, die den Code für die Dienst Ebene des Contact-Repository enthält*
4. Fügen Sie der Datei **ContactRepository.cs** eine using-Direktive hinzu, um den Namespace "Models" einzuschließen.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Fügen Sie der Datei **ContactRepository.cs** den folgenden hervorgehobenen Code hinzu, um die getallcontacts-Methode zu implementieren.

    (Code Ausschnitt- *Web-API Lab-Ex01-Contact-Repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Öffnen Sie die Datei **ContactController.cs** , wenn Sie nicht bereits geöffnet ist.
7. Fügen Sie dem Namespace Deklarations Abschnitt der Datei die folgende using-Anweisung hinzu.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Fügen Sie der **ContactController.cs** -Klasse den folgenden hervorgehobenen Code hinzu, um ein privates Feld hinzuzufügen, das die Instanz des Repository darstellt, damit die übrigen Klassenmember die Dienst Implementierung verwenden können.

    (Code Ausschnitt- *Web-API Lab-Ex01-Contact Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Ändern Sie die **Get** -Methode so, dass Sie den Contact-Repository-Dienst nutzt.

    (Code Ausschnitt- *Web-API Lab-Ex01-Rückgabe einer Liste von Kontakten über das Repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Platzieren Sie einen Haltepunkt in der **Get** -Methoden Definition von **ContactController**.

   ![Hinzufügen von Breakpoints zum Kontakt Controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Hinzufügen von Breakpoints zum Kontakt Controller")

   *Hinzufügen von Breakpoints zum Kontakt Controller*
11. Drücken Sie **F5**, um die Anwendung auszuführen.
12. Wenn der Browser geöffnet wird, drücken Sie **F12** , um die Entwicklertools zu öffnen.
13. Klicken Sie auf die Registerkarte **Netzwerk** .
14. Klicken Sie auf die Schaltfläche **Erfassung starten** .
15. Fügen Sie die URL in der Adressleiste mit dem Suffix **/API/Contact** an, und drücken **Sie die Eingabe** Taste, um den API-Controller zu laden.
16. Visual Studio 2012 sollte abbrechen, sobald die Ausführung der **Get** -Methode beginnt.

   ![Unterbrechen innerhalb der Get-Methode](build-restful-apis-with-aspnet-web-api/_static/image18.png "Unterbrechen innerhalb der Get-Methode")

   *Unterbrechen innerhalb der Get-Methode*
17. Drücken Sie **F5**, um fortzufahren.
18. Wechseln Sie zurück zu Internet Explorer, wenn dieser noch nicht im Fokus ist. Notieren Sie sich das Fenster Netzwerk Erfassung.

    ![Netzwerk Ansicht in Internet Explorer, die die Ergebnisse des Web-API-Aufrufes anzeigt](build-restful-apis-with-aspnet-web-api/_static/image19.png "Netzwerk Ansicht in Internet Explorer, die die Ergebnisse des Web-API-Aufrufes anzeigt")

    *Netzwerk Ansicht in Internet Explorer, die die Ergebnisse des Web-API-Aufrufes anzeigt*
19. Klicken Sie auf die Schaltfläche **Gehe zu ausführlicher Ansicht** .
20. Klicken Sie auf die Registerkarte **Antworttext** . Beachten Sie die JSON-Ausgabe des API-Aufrufes und die Darstellung der beiden von der Dienst Ebene abgerufenen Kontakte.

    ![Anzeigen der JSON-Ausgabe der Web-API im Fenster "Entwicklertools"](build-restful-apis-with-aspnet-web-api/_static/image20.png "Anzeigen der JSON-Ausgabe der Web-API im Fenster "Entwicklertools"")

    *Anzeigen der JSON-Ausgabe der Web-API im Fenster "Entwicklertools"*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Übung 2: Erstellen einer Web-API mit Lese-/Schreibzugriff

In dieser Übung implementieren Sie Post-und Put-Methoden für den Contact Manager, um ihn mit Datenbearbeitungs Funktionen zu aktivieren.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Aufgabe 1: Öffnen des Web-API-Projekts

In dieser Aufgabe bereiten Sie sich darauf vor, das in Übung 1 erstellte Web-API-Projekt zu verbessern, sodass Benutzereingaben akzeptiert werden können.

1. Führen Sie **Visual Studio 2012 Express für das Web aus**. wechseln Sie zu **Start** , und geben Sie **vs Express für Web** und drücken Sie dann die **Eingabe**Taste.
2. Öffnen Sie die Projekt Mappe " **Begin** " unter **Source/Ex02-Read Write tewebapi/BEGIN/** Folder. Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Öffnen Sie die Datei **Services/contactrepository. cs** .

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Aufgabe 2: Hinzufügen von datendauerhaftigkeits Features zur kontaktrepository-Implementierung

In dieser Aufgabe erweitern Sie die contactrepository-Klasse des in Übung 1 erstellten Web-API-Projekts, sodass Benutzereingaben und neue Kontakt Instanzen persistent gespeichert und akzeptiert werden können.

1. Fügen Sie der **contactrepository** -Klasse die folgende Konstante hinzu, um den Namen des Schlüssel namens des Webserver-Cache Elements später in dieser Übung darzustellen.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Fügen Sie einen Konstruktor zum **contactrepository** hinzu, das den folgenden Code enthält.

    (Code Ausschnitt- *Web-API Lab-Ex02-Contact Repository-Konstruktor*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Ändern Sie den Code für die **getallcontacts** -Methode, wie unten gezeigt.

    (Code Ausschnitt- *Web-API Lab-Ex02-Get all Contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Dieses Beispiel dient zu Demonstrationszwecken und verwendet den Webserver Cache als Speichermedium, damit die Werte für mehrere Clients gleichzeitig verfügbar sind, anstatt einen Sitzungs Speichermechanismus oder eine Anforderungs Speicher Lebensdauer zu verwenden. Sie können Entity Framework, den XML-Speicher oder eine beliebige andere Vielfalt anstelle des Webserver Caches verwenden.
4. Implementieren Sie eine neue Methode mit dem Namen **savecontact** zur **contactrepository** -Klasse, um das Speichern eines Kontakts auszuführen. Die **savecontact** -Methode muss einen einzelnen **Contact** -Parameter verwenden und einen booleschen Wert zurückgeben, der den Erfolg oder Misserfolg angibt.

    (Code Ausschnitt- *Web-API Lab-Ex02-Implementieren der savecontact-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Übung 3: Verwenden der Web-API aus einem HTML-Client

In dieser Übung erstellen Sie einen HTML-Client, um die Web-API aufzurufen. Dieser Client vereinfacht den Datenaustausch mit der Web-API mithilfe von JavaScript und zeigt die Ergebnisse in einem Webbrowser mithilfe von HTML-Markup an.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Aufgabe 1: Ändern der Index Ansicht zur Bereitstellung einer GUI zum Anzeigen von Kontakten

In dieser Aufgabe ändern Sie die Standard Index Ansicht der Webanwendung, um die Anforderung der Anzeige der Liste vorhandener Kontakte in einem HTML-Browser zu unterstützen.

1. Öffnen Sie **Visual Studio 2012 Express für Web** , wenn es nicht bereits geöffnet ist.
2. Öffnen Sie die Projekt Mappe " **Begin** " unter **Quell/Ex03-consumingwebapi/BEGIN/** Folder. Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Öffnen Sie die Datei " **Index. cshtml** ", die sich im Ordner **views/Home** befindet.
4. Ersetzen Sie den HTML-Code im div-Element durch den ID- **Text** , sodass er wie der folgende Code aussieht.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Fügen Sie am Ende der Datei den folgenden JavaScript-Code hinzu, um die HTTP-Anforderung an die Web-API auszuführen.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Öffnen Sie die Datei **ContactController.cs** , wenn Sie nicht bereits geöffnet ist.
7. Platzieren Sie einen Haltepunkt in der **Get** -Methode der **ContactController** -Klasse.

    ![Platzieren eines Breakpoints in der Get-Methode des API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image21.png "Platzieren eines Breakpoints in der Get-Methode des API-Controllers")

    *Platzieren eines Breakpoints in der Get-Methode des API-Controllers*
8. Drücken Sie **F5**, um das Projekt auszuführen. Der Browser lädt das HTML-Dokument.

    > [!NOTE]
    > Stellen Sie sicher, dass Sie die Stamm-URL Ihrer Anwendung durchsuchen.
9. Nachdem die Seite geladen und das JavaScript ausgeführt wurde, wird der Breakpoint gedrückt, und die Codeausführung wird im Controller angehalten.

    ![Debuggen in Web-API-Aufrufe mithilfe von vs Express für Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debuggen in Web-API-Aufrufe mithilfe von vs Express für Web")

    *Debuggen in den Web-API-Aufrufe mithilfe von Visual Studio 2012 Express für Web*
10. Entfernen Sie den Haltepunkt, und drücken Sie **F5** oder die **Schaltfläche** zum Debuggen der Symbolleiste, um das Laden der Ansicht im Browser fortzusetzen. Sobald der Web-API-Befehl abgeschlossen ist, sollten Sie die vom Web-API-Befehl zurückgegebenen Kontakte sehen, die als Listenelemente im Browser angezeigt werden.

    ![Ergebnisse der API-Aufrufe, die im Browser als Listenelemente angezeigt werden](build-restful-apis-with-aspnet-web-api/_static/image23.png "Ergebnisse der API-Aufrufe, die im Browser als Listenelemente angezeigt werden")

    *Ergebnisse der API-Aufrufe, die im Browser als Listenelemente angezeigt werden*
11. Beenden des Debuggens.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Aufgabe 2: Ändern der Index Ansicht zum Bereitstellen einer GUI zum Erstellen von Kontakten

In dieser Aufgabe ändern Sie weiterhin die Index Ansicht der MVC-Anwendung. Der HTML-Seite wird ein Formular hinzugefügt, mit dem Benutzereingaben erfasst und an die Web-API gesendet werden, um einen neuen Kontakt zu erstellen. Außerdem wird eine neue Web-API-Controller Methode erstellt, um das Datum von der grafischen Benutzeroberfläche zu erfassen.

1. Öffnen Sie die Datei **ContactController.cs** .
2. Fügen Sie der Controller Klasse mit dem Namen **Post** eine neue Methode hinzu, wie im folgenden Code gezeigt.

    (Code Ausschnitt- *Web-API Lab-Ex03-Post-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Öffnen Sie die Datei **Index. cshtml** in Visual Studio, wenn Sie nicht bereits geöffnet ist.
4. Fügen Sie der Datei den folgenden HTML-Code direkt nach der ungeordneten Liste hinzu, die Sie in der vorherigen Aufgabe hinzugefügt haben.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Fügen Sie im Skript Element am Ende des Dokuments den folgenden markierten Code hinzu, um Click-Ereignisse zu behandeln, die die Daten mithilfe eines HTTP Post-Aufrufes an die Web-API senden.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. Platzieren Sie in **ContactController.cs**einen Haltepunkt in der **Post** -Methode.
7. Drücken Sie **F5** , um die Anwendung im Browser auszuführen.
8. Wenn die Seite im Browser geladen ist, geben Sie einen neuen Kontaktnamen und eine ID ein, und klicken Sie auf die Schaltfläche **Speichern** .

    ![Das HTML-Client Dokument, das im Browser geladen wird.](build-restful-apis-with-aspnet-web-api/_static/image24.png "Das HTML-Client Dokument, das im Browser geladen wird.")

    *Das HTML-Client Dokument, das im Browser geladen wird.*
9. Wenn das Debugger-Fenster in der **Post** -Methode unterbrochen wird, sehen Sie sich die Eigenschaften des **Contact** -Parameters an. Die Werte müssen mit den Daten verglichen werden, die Sie im Formular eingegeben haben.

    ![Das Kontakt Objekt, das vom Client an die Web-API gesendet wird.](build-restful-apis-with-aspnet-web-api/_static/image25.png "Das Kontakt Objekt, das vom Client an die Web-API gesendet wird.")

    *Das Kontakt Objekt, das vom Client an die Web-API gesendet wird.*
10. Schrittweises Durchlaufen der Methode im Debugger, bis die **Antwort** Variable erstellt wurde. Bei der Überprüfung **im Fenster Lokal** im Debugger sehen Sie, dass alle Eigenschaften festgelegt wurden.

   ![Die Antwort nach der Erstellung im Debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "Die Antwort nach der Erstellung im Debugger")

   *Die Antwort nach der Erstellung im Debugger*
11. Wenn Sie **F5** drücken oder im Debugger auf **weiter** klicken, wird die Anforderung beendet. Nachdem Sie wieder zum Browser gewechselt haben, wurde der neue Kontakt der Liste der Kontakte hinzugefügt, die von der **contactrepository** -Implementierung gespeichert wurden.

   ![Der Browser reflektiert die erfolgreiche Erstellung der neuen Kontakt Instanz.](build-restful-apis-with-aspnet-web-api/_static/image27.png "Der Browser reflektiert die erfolgreiche Erstellung der neuen Kontakt Instanz.")

   *Der Browser reflektiert die erfolgreiche Erstellung der neuen Kontakt Instanz.*

> [!NOTE]
> Außerdem können Sie diese Anwendung in Azure bereitstellen. Weitere Informationen hierzu finden Sie im [Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy](#AppendixC).

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser Übungseinheit haben Sie das neue ASP.net-Web-API Framework und die Implementierung von Rest-Web-APIs mit dem Framework eingeführt. Von hier aus können Sie ein neues Repository erstellen, das die Daten Persistenz mithilfe einer beliebigen Anzahl von Mechanismen vereinfacht und diesen Dienst anstelle der einfachen bereitstellt, die in dieser Übungseinheit als Beispiel bereitgestellt wird. Die Web-API unterstützt eine Reihe zusätzlicher Features, z. b. das Aktivieren der Kommunikation von nicht-HTML-Clients, die in einer beliebigen Sprache geschrieben wurden, die HTTP und JSON oder XML Die Möglichkeit, eine Web-API außerhalb einer typischen Webanwendung zu hosten, ist ebenfalls möglich, und Sie können auch eigene Serialisierungsformate erstellen.

Die ASP.NET-Website verfügt über einen Bereich, der für das ASP.net-Web-API Framework bei [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api)reserviert ist. Diese Site liefert weiterhin aktuelle Informationen, Beispiele und Neuigkeiten im Zusammenhang mit der Web-API. Überprüfen Sie Sie häufig, wenn Sie die Art der Erstellung von benutzerdefinierten Web-APIs vertiefen möchten, die für praktisch alle Geräte oder Entwicklungs-Frameworks verfügbar sind.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Anhang A: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](build-restful-apis-with-aspnet-web-api/_static/image28.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

    ![Beginnen Sie mit der Eingabe des Ausschnitt namens.](build-restful-apis-with-aspnet-web-api/_static/image29.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

    *Beginnen Sie mit der Eingabe des Ausschnitt namens.*

    ![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](build-restful-apis-with-aspnet-web-api/_static/image30.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

    *Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

    ![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](build-restful-apis-with-aspnet-web-api/_static/image31.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

    *Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>So fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)

1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.
2. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
3. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

    ![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](build-restful-apis-with-aspnet-web-api/_static/image32.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

    *Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

    ![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](build-restful-apis-with-aspnet-web-api/_static/image33.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

    *Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Anhang B: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express für Web-Kachel*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

In diesem Anhang wird erläutert, wie Sie eine neue Website aus dem Azure-Portal erstellen und die Anwendung, die Sie erhalten haben, mithilfe des Labs veröffentlichen. nutzen Sie dabei die von Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Azure-Portal

1. Wechseln Sie zum [Azure-Verwaltungsportal](https://manage.windowsazure.com/) , und melden Sie sich mit den mit Ihrem Abonnement verknüpften Microsoft-Anmelde Informationen an.

    > [!NOTE]
    > Mit Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt. Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.

    ![Anmelden bei Windows Azure-Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Anmelden bei Windows Azure-Portal")

    *Anmelden beim Portal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](build-restful-apis-with-aspnet-web-api/_static/image40.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Azure ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals in Azure bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](build-restful-apis-with-aspnet-web-api/_static/image41.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](build-restful-apis-with-aspnet-web-api/_static/image43.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](build-restful-apis-with-aspnet-web-api/_static/image44.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung in einer Azure für jede aktivierte Veröffentlichungs Methode erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen in Azure zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](build-restful-apis-with-aspnet-web-api/_static/image45.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung in Azure aus Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](build-restful-apis-with-aspnet-web-api/_static/image46.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](build-restful-apis-with-aspnet-web-api/_static/image48.png)

    ![Client-IP-Adresse](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Änderungen bestätigen*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](build-restful-apis-with-aspnet-web-api/_static/image51.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](build-restful-apis-with-aspnet-web-api/_static/image53.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein, z. b.: *MVC4SampleDB*.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](build-restful-apis-with-aspnet-web-api/_static/image56.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](build-restful-apis-with-aspnet-web-api/_static/image57.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image58.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

    ![In Windows Azure veröffentlichte Anwendung](build-restful-apis-with-aspnet-web-api/_static/image59.png "In Windows Azure veröffentlichte Anwendung")

    *In Azure veröffentlichte Anwendung*
