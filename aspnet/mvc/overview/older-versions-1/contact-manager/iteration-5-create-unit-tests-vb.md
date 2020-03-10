---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iterations #5 – Erstellen von Komponententests (VB) | Microsoft-Dokumentation'
author: microsoft
description: In der fünften Iterationen wird die Wartung und Änderung unserer Anwendung durch Hinzufügen von Komponententests vereinfacht. Wir simulieren unsere Datenmodell Klassen und erstellen Komponententests für o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ce1c6224a7e9203ff62f136f4f3a43e4561a904
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437835"
---
# <a name="iteration-5--create-unit-tests-vb"></a>Iterations #5 – Erstellen von Komponententests (VB)

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> In der fünften Iterationen wird die Wartung und Änderung unserer Anwendung durch Hinzufügen von Komponententests vereinfacht. Wir simulieren unsere Datenmodell Klassen und erstellen Komponententests für unsere Controller und Validierungs Logik.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Aufbauen einer Kontakt Verwaltung ASP.NET MVC-Anwendung (VB)

In dieser Reihe von Tutorials erstellen wir von Anfang bis Ende eine gesamte Kontakt Verwaltungs Anwendung. Mithilfe der Kontakt-Manager-Anwendung können Sie Kontaktinformationen (Namen, Telefonnummern und e-Mail-Adressen) für eine Liste von Personen speichern.

Die Anwendung wird über mehrere Iterationen erstellt. Bei jeder Iterationen verbessern wir die Anwendung allmählich. Das Ziel dieses mehrfaches Iterations Ansatzes besteht darin, Ihnen zu ermöglichen, den Grund für jede Änderung zu verstehen.

- Iterations #1: Erstellen Sie die Anwendung. In der ersten Iterationen erstellen wir den Kontakt-Manager auf einfachste Art und Weise. Wir fügen Unterstützung für grundlegende Daten Bank Vorgänge hinzu: Create, Read, Update und DELETE (CRUD).

- Iterations #2: machen Sie das Aussehen der Anwendung schön. In dieser Iterationen verbessern wir die Darstellung der Anwendung durch Ändern der standardmäßigen ASP.NET-MVC-Ansichts Master Seite und des Cascading Stylesheets.

- Iterations #3: Formular Validierung hinzufügen. In der dritten Iterationen fügen wir die grundlegende Formular Validierung hinzu. Wir hindern Personen daran, ein Formular zu senden, ohne die erforderlichen Formularfelder abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Iterations #4: Legen Sie die Anwendung lose gekoppelt. In dieser vierten Iterationen nutzen wir mehrere Software Entwurfsmuster, um die Verwaltung und Änderung der Contact Manager-Anwendung zu vereinfachen. Beispielsweise können wir unsere Anwendung so umgestalten, dass Sie das Repository-Muster und das Muster für die Abhängigkeitsinjektion verwendet.

- Iterations #5: Erstellen von Komponententests. In der fünften Iterationen wird die Wartung und Änderung unserer Anwendung durch Hinzufügen von Komponententests vereinfacht. Wir simulieren unsere Datenmodell Klassen und erstellen Komponententests für unsere Controller und Validierungs Logik.

- Iterations #6: Verwenden Sie die Test gesteuerte Entwicklung. In dieser sechsten Iterationen fügen wir der Anwendung neue Funktionen hinzu, indem wir zuerst Komponententests schreiben und Code für die Komponententests schreiben. In dieser Iterationen fügen wir Kontaktgruppen hinzu.

- Iterations #7: Hinzufügen von AJAX-Funktionen. In der siebten Iterationen verbessern wir die Reaktionsfähigkeit und Leistung unserer Anwendung durch Hinzufügen von Unterstützung für AJAX.

## <a name="this-iteration"></a>Diese Iterations

In der vorherigen Iterationen der Kontakt-Manager-Anwendung haben wir die Anwendung so umgestaltet, dass Sie lockerer gekoppelt ist. Die Anwendung wurde in verschiedene Controller-, Dienst-und Repository-Schichten aufgeteilt. Jede Ebene interagiert mit der darunter liegenden Ebene über Schnittstellen.

Wir haben die Anwendung umgestaltet, um die Wartung und Änderung der Anwendung zu vereinfachen. Wenn wir z. b. eine neue Datenzugriffs Technologie verwenden müssen, können wir einfach die Repository-Ebene ändern, ohne den Controller oder die Dienst Schicht zu berühren. Indem der Kontakt-Manager locker gekoppelt wird, haben wir die Anwendung stabiler geändert, damit Sie geändert werden kann.

Was passiert jedoch, wenn wir der Kontakt-Manager-Anwendung ein neues Feature hinzufügen müssen? Oder was geschieht, wenn ein Fehler behoben wird? Eine traurige, aber bewährte Tatsache, dass Sie Code schreiben, besteht darin, dass Sie immer dann, wenn Sie Code berühren, das Risiko einer Einführung neuer Fehler erzeugen.

Beispielsweise kann Ihr Manager einen guten Tag zum Hinzufügen eines neuen Features zum Contact Manager auffordern. Sie möchte Unterstützung für Kontaktgruppen hinzufügen. Sie möchte, dass Benutzer Ihre Kontakte in Gruppen wie Friends, Business usw. organisieren können.

Um diese neue Funktion zu implementieren, müssen Sie alle drei Ebenen der Contact Manager-Anwendung ändern. Sie müssen den Controllern, der Dienst Ebene und dem Repository neue Funktionen hinzufügen. Sobald Sie mit dem Ändern von Code beginnen, riskieren Sie die Funktionen, die zuvor funktionieren.

Das Umgestalten der Anwendung in separate Ebenen, wie dies in der vorherigen Iterationen der Fall war, war eine gute Sache. Dies war eine gute Sache, da es uns ermöglicht, Änderungen an ganzen Ebenen vorzunehmen, ohne den Rest der Anwendung zu berühren. Wenn Sie jedoch den Code in einer Ebene leichter verwalten und ändern möchten, müssen Sie Komponententests für den Code erstellen.

Sie verwenden einen-Komponenten Test, um eine einzelne Code Einheit zu testen. Diese Code Einheiten sind kleiner als ganze Anwendungsebenen. In der Regel verwenden Sie einen Komponenten Test, um zu überprüfen, ob eine bestimmte Methode im Code erwartungsgemäß verhält. Beispielsweise erstellen Sie einen Komponenten Test für die Methode "Methode", die von der contactmanagerservice-Klasse verfügbar gemacht wird.

Die Komponententests für eine Anwendung funktionieren genau wie ein Sicherheitsnetz. Wenn Sie Code in einer Anwendung ändern, können Sie eine Reihe von Komponententests ausführen, um zu überprüfen, ob die Änderung vorhandene Funktionalität unterbricht. Mit Komponententests können Sie den Code sicher ändern. Mit Komponententests können Sie den gesamten Code in der Anwendung stabiler ändern.

In dieser Iterations Komponente fügen wir unserer Contact Manager-Anwendung Komponententests hinzu. Auf diese Weise können wir in der nächsten Iterationen der Anwendung Kontaktgruppen hinzufügen, ohne sich Gedanken über das Abbrechen vorhandener Funktionen machen zu müssen.

> [!NOTE] 
> 
> Es gibt eine Vielzahl von Frameworks für Unittests, einschließlich nunit, xUnit.net und MbUnit. In diesem Tutorial verwenden wir das Komponenten Test-Framework, das in Visual Studio enthalten ist. Sie könnten jedoch auch eines dieser alternativen Frameworks verwenden.

## <a name="what-gets-tested"></a>Was wird getestet?

In der perfekten Welt würde der gesamte Code durch Komponententests abgedeckt werden. In der perfekten Welt hätten Sie das perfekte Sicherheitsnetz. Sie können jede Codezeile in Ihrer Anwendung ändern und sofort wissen, indem Sie die Komponententests ausführen, und zwar unabhängig davon, ob die Änderung die vorhandene Funktionalität abgebrochen hat.

Wir leben jedoch nicht in einer idealen Welt. In der Praxis konzentrieren Sie sich beim Schreiben von Komponententests auf das Schreiben von Tests für Ihre Geschäftslogik (z. b. Validierungs Logik). Insbesondere schreiben Sie *keine* Komponententests für Ihre Datenzugriffs Logik oder Ihre Ansichts Logik.

Um nützlich zu sein, müssen Komponententests sehr schnell ausgeführt werden. Sie können mühelos Hunderte (oder sogar Tausende) von Komponententests für eine Anwendung sammeln. Wenn die Ausführung der Komponententests viel Zeit in Anspruch nimmt, wird die Ausführung vermieden. Mit anderen Worten, die Komponententests mit langer Ausführungszeit sind für tägliche Codierungs Zwecke unbrauchbar.

Aus diesem Grund schreiben Sie in der Regel keine Komponententests für Code, der mit einer-Datenbank interagiert. Das Ausführen von Hunderten von Komponententests für eine Live Datenbank wäre zu langsam. Stattdessen können Sie Ihre Datenbank simulieren und Code schreiben, der mit der Mock-Datenbank interagiert (es wird beschrieben, wie Sie eine Datenbank weiter unten durcharbeiten).

Aus einem ähnlichen Grund schreiben Sie in der Regel keine Komponententests für Sichten. Um eine Ansicht zu testen, müssen Sie einen Webserver einrichten. Da das Einrichten eines Webservers ein relativ langsamer Prozess ist, wird das Erstellen von Komponententests für Ihre Sichten nicht empfohlen.

Wenn Ihre Ansicht komplizierte Logik enthält, sollten Sie erwägen, die Logik in Hilfsmethoden zu verschieben. Sie können Komponententests für Hilfsmethoden schreiben, die ausgeführt werden, ohne einen Webserver aufzugliederung.

> [!NOTE] 
> 
> Obwohl das Schreiben von Tests für die Datenzugriffs Logik oder die Sicht Logik beim Schreiben von Komponententests keine gute Idee ist, können diese Tests bei der Erstellung funktionaler oder Integrationstests sehr wertvoll sein.

> [!NOTE] 
> 
> ASP.NET MVC ist die Web Forms Ansichts-Engine. Während das Web Forms Ansichts Modul von einem Webserver abhängt, sind andere Ansichts-Engines möglicherweise nicht.

## <a name="using-a-mock-object-framework"></a>Verwenden eines Mock-Objekt-Frameworks

Beim Erstellen von Komponententests müssen Sie fast immer ein Mock-Objekt Framework nutzen. Mit einem Mock-Objekt Framework können Sie Mock und stubweise für die Klassen in der Anwendung erstellen.

Beispielsweise können Sie ein Mock-Objekt Framework verwenden, um eine Pseudo Version der Repository-Klasse zu generieren. Auf diese Weise können Sie die mockrepository-Klasse anstelle der realen Repository-Klasse in ihren Komponententests verwenden. Mithilfe des mockrepository können Sie beim Ausführen eines Komponententests vermeiden, Datenbankcode auszuführen.

Visual Studio enthält kein Mock-Objekt Framework. Es gibt jedoch mehrere kommerzielle und Open Source-Pseudo Objekt-Frameworks, die für .NET Framework verfügbar sind:

1. Muq: Dieses Framework ist unter der Open Source-BSD-Lizenz verfügbar. Sie können "muq" aus [https://code.google.com/p/moq/](https://code.google.com/p/moq/)herunterladen.
2. Rhino-Mock: Dieses Framework ist unter der Open Source-BSD-Lizenz verfügbar. Sie können Rhino-Mock aus [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx)herunterladen.
3. Typemockisolator: Dies ist ein kommerzielles Framework. Sie können eine Testversion von [http://www.typemock.com/](http://www.typemock.com/)herunterladen.

In diesem Tutorial entschied ich mich für die Verwendung von "muq". Allerdings können Sie mit Rhino-Mock oder typemockisolator die Mockobjekte für die Contact Manager-Anwendung erstellen.

Bevor Sie die Verwendung von "muq" durchführen können, müssen Sie die folgenden Schritte ausführen:

1. erforderlich.
2. Stellen Sie vor dem Entzippen des Downloads sicher, dass Sie mit der rechten Maustaste auf die Datei klicken, und klicken Sie auf die Schaltfläche **Unblock** (siehe Abbildung 1).
3. Entpacken Sie den Download.
4. Fügen Sie dem Test Projekt einen Verweis auf die-Assembly hinzu, indem Sie die Menüoption **Projekt, Verweis hinzufügen auswählen,** um das Dialogfeld **Verweis hinzufügen** zu öffnen. Navigieren Sie auf der Registerkarte durchsuchen zu dem Ordner, in dem Sie die Datei ". dll" entzippt haben, und wählen Sie die Assembly ". Klicken Sie auf die Schaltfläche **OK** (siehe Abbildung 2).

[Aufheben der Blockierung von muq ![](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Abbildung 01**: Aufheben der Blockierung von muq ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-5-create-unit-tests-vb/_static/image2.png))

[Verweise ![nach dem Hinzufügen von muq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Abbildung 02**: Verweise nach dem Hinzufügen von muq ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-5-create-unit-tests-vb/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Erstellen von Komponenten Tests für die Dienst Ebene

Beginnen Sie, indem Sie eine Reihe von Komponententests für die Dienst Ebene der Kontakt-Manager-Anwendung erstellen. Wir verwenden diese Tests, um unsere Validierungs Logik zu überprüfen.

Erstellen Sie im Projekt "ContactManager. Tests" einen neuen Ordner mit dem Namen "Models". Klicken Sie dann mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie **hinzufügen, neuer Test**aus. Das in Abbildung 3 dargestellte Dialogfeld **neuen Test hinzufügen** wird angezeigt. Wählen Sie die Vorlage für Komponenten **Tests** aus, und benennen Sie den neuen Test contactmanagerservicetest. vb. Klicken Sie auf die Schaltfläche **OK** , um den neuen Test dem Testprojekt hinzuzufügen.

> [!NOTE] 
> 
> Im Allgemeinen möchten Sie, dass die Ordnerstruktur des Test Projekts der Ordnerstruktur Ihres ASP.NET MVC-Projekts entspricht. Beispielsweise platzieren Sie Controller Tests in einem Controller Ordner, modellieren Tests in einem Modell Ordner usw.

[![models\contactmanagerservicetest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Abbildung 03**: models\contactmanagerservicetest.cs ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-5-create-unit-tests-vb/_static/image6.png))

Zunächst möchten wir die Methode "Methode", die von der contactmanagerservice-Klasse verfügbar gemacht wird, testen. Wir erstellen die folgenden fünf Tests:

- "Kreatecontact ()": testet, ob "kreatecontact ()" den Wert "true" zurückgibt, wenn ein gültiger Kontakt an die Methode übergeben wird.
- "Kreatecontactrequiredfirstname ()": testet, ob dem Modell Status eine Fehlermeldung hinzugefügt wird, wenn ein Kontakt mit einem fehlenden Vornamen an die Methode "up contact ()" übergeben wird.
- "Kreatecontactrequiredlastname ()": testet, ob eine Fehlermeldung zum Modell Status hinzugefügt wird, wenn ein Kontakt mit einem fehlenden Nachnamen an die Methode "up contact ()" übergeben wird.
- "Kreatecontactinvalidphone ()": testet, ob dem Modell Status eine Fehlermeldung hinzugefügt wird, wenn ein Kontakt mit einer ungültigen Telefonnummer an die Methode "Methode" von "| atecontact ()" übergeben wird.
- "Kreatecontactinvalidemail ()": testet, ob dem Modell Status eine Fehlermeldung hinzugefügt wird, wenn ein Kontakt mit einer ungültigen e-Mail-Adresse an die Methode "Methode.

Der erste Test überprüft, ob ein gültiger Kontakt keinen Validierungs Fehler generiert. Die verbleibenden Tests überprüfen jede der Validierungsregeln.

Der Code für diese Tests ist in der Liste 1 enthalten.

**Codebeispiel 1: models\contactmanagerservicetest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]

Da wir die Contact-Klasse in der Liste 1 verwenden, müssen wir dem Test Projekt einen Verweis auf den Microsoft-Entity Framework hinzufügen. Fügen Sie einen Verweis auf die System. Data. Entity-Assembly hinzu.

In der Liste 1 ist eine Methode mit dem Namen Initialize () enthalten, die mit dem [TestInitialize]-Attribut ergänzt wird. Diese Methode wird automatisch vor jedem Ausführen der Komponententests aufgerufen (Sie wird fünf Mal direkt vor jedem der Komponententests aufgerufen). Die Initialize ()-Methode erstellt ein Mock-Repository mit der folgenden Codezeile:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Diese Codezeile verwendet das MOQ-Framework, um ein mockrepository aus der icontactmanagerrepository-Schnittstelle zu generieren. Das mockrepository wird anstelle des eigentlichen entitycontactmanagerrepository verwendet, um den Zugriff auf die Datenbank zu vermeiden, wenn jeder Komponenten Test ausgeführt wird. Das Mock-Repository implementiert die Methoden der icontactmanagerrepository-Schnittstelle, aber die Methoden Don 't machen tatsächlich etwas.

> [!NOTE] 
> 
> Bei der Verwendung des muq-Frameworks wird zwischen \_mockrepository und \_mockrepository. Object unterschieden. Der erste verweist auf die Mock-Klasse (of icontactmanagerrepository), die Methoden zum Angeben des Verhaltens des Mock-Repository enthält. Letztere bezieht sich auf das eigentliche Mock-Repository, das die icontactmanagerrepository-Schnittstelle implementiert.

Das mockrepository wird in der Initialize ()-Methode verwendet, wenn eine Instanz der contactmanagerservice-Klasse erstellt wird. Alle einzelnen Unittests verwenden diese Instanz der contactmanagerservice-Klasse.

In der Liste 1 sind fünf Methoden enthalten, die den einzelnen Komponententests entsprechen. Jede dieser Methoden wird mit dem [TestMethod]-Attribut versehen. Wenn Sie die Komponententests ausführen, wird jede Methode, die über dieses Attribut verfügt, aufgerufen. Anders ausgedrückt: jede Methode, die mit dem [TestMethod]-Attribut ergänzt wird, ist ein Komponenten Test.

Der erste Komponenten Test mit dem Namen CreateContact () überprüft, ob das Aufrufen von CreateContact () den Wert true zurückgibt, wenn eine gültige Instanz der Contact-Klasse an die-Methode übergeben wird. Der Test erstellt eine Instanz der Contact-Klasse, ruft die Methode "| atecontact ()" auf und überprüft, ob "kreatecontact ()" den Wert "true" zurückgibt.

Die verbleibenden Tests überprüfen, ob die Methode "false" zurückgibt, wenn die Methode "Methode" mit einem ungültigen Kontakt aufgerufen wird, und die erwartete Validierungs Fehlermeldung wird dem Modell Status hinzugefügt. Beispielsweise wird mit dem Test "kreatecontactrequiredfirstname ()" eine Instanz der Contact-Klasse mit einer leeren Zeichenfolge für die FirstName-Eigenschaft erstellt. Als nächstes wird die Methode "up contact ()" mit dem ungültigen Kontakt aufgerufen. Schließlich wird mit dem Test überprüft, ob "deatecontact ()" den Wert "false" zurückgibt und dass der Modell Status die erwartete Überprüfungs Fehlermeldung "Vorname ist erforderlich" enthält.

Sie können die Komponententests in der Liste 1 ausführen, indem Sie die Menüoption **Test, ausführen, alle Tests in Projekt Mappe (STRG + R, A)** auswählen. Die Ergebnisse der Tests werden im Fenster Testergebnisse angezeigt (siehe Abbildung 4).

[![Testergebnisse](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Abbildung 04**: Testergebnisse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-5-create-unit-tests-vb/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Erstellen von Komponenten Tests für Controller

ASP.NET MVC-Anwendung steuert den Fluss der Benutzerinteraktion. Beim Testen eines Controllers möchten Sie testen, ob der Controller das richtige Aktions Ergebnis zurückgibt, und Daten anzeigen. Möglicherweise möchten Sie auch testen, ob ein Controller in der erwarteten Weise mit Modellklassen interagiert.

Beispielsweise enthält die Liste 2 zwei Komponententests für die Methode Contact Controller Create (). Der erste Komponenten Test überprüft, ob die Create ()-Methode, wenn ein gültiger Kontakt an die Create ()-Methode übergeben wird, an die Index Aktion umgeleitet wird. Anders ausgedrückt: Wenn ein gültiger Kontakt übermittelt wird, sollte die Create ()-Methode eine redirecttorouteresult zurückgeben, die die Index Aktion darstellt.

Wir möchten die Dienst Ebene "ContactManager" nicht testen, wenn wir die Controller Schicht testen. Daher wird die Dienst Ebene mit dem folgenden Code in der Initialize-Methode verspottet:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

Im createvalidcontact ()-Komponenten Test wird das Verhalten beim Aufrufen der Methode CreateContact () der Dienst Schicht mit der folgenden Codezeile verspottet:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Diese Codezeile bewirkt, dass der Mock ContactManager-Dienst den Wert "true" zurückgibt, wenn die zugehörige Methode "| atecontact ()" aufgerufen wird. Durch die Durchführung der Dienst Ebene können wir das Verhalten des Controllers testen, ohne dass Code in der Dienst Ebene ausgeführt werden muss.

Der zweite Komponenten Test überprüft, ob die Create ()-Aktion die CREATE VIEW zurückgibt, wenn ein ungültiger Kontakt an die-Methode übermittelt wird. Wir führen dazu, dass die Methode "kreatecontact ()" der Dienst Schicht den Wert "false" mit der folgenden Codezeile zurückgibt:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Wenn sich die Create ()-Methode wie erwartet verhält, sollte Sie die CREATE VIEW zurückgeben, wenn die Dienst Ebene den Wert false zurückgibt. Auf diese Weise kann der Controller die Validierungs Fehlermeldungen in der Create-Ansicht anzeigen, und der Benutzer hat die Möglichkeit, die ungültigen Kontakt Eigenschaften zu korrigieren.

Wenn Sie Komponententests für Ihre Controller erstellen möchten, müssen Sie explizite Ansichts Namen aus den Controller Aktionen zurückgeben. Geben Sie z. b. keine Ansicht wie folgt zurück:

Rückgabe Ansicht ()

Geben Sie stattdessen die Ansicht wie folgt zurück:

Rückgabe Ansicht ("erstellen")

Wenn Sie beim Zurückgeben einer Sicht nicht explizit sind, gibt die ViewResult. ViewName-Eigenschaft eine leere Zeichenfolge zurück.

**Codebeispiel 2: controllers\contactcontrollertest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Zusammenfassung

In dieser Iterationen haben wir Komponententests für unsere Contact Manager-Anwendung erstellt. Wir können diese Komponententests jederzeit ausführen, um zu überprüfen, ob sich die Anwendung immer noch auf die erwartete Weise verhält. Die Komponententests fungieren als Sicherheitsnetz für unsere Anwendung, sodass wir unsere Anwendung in Zukunft sicher ändern können.

Wir haben zwei Sätze von Komponententests erstellt. Zunächst haben wir unsere Validierungs Logik getestet, indem wir Komponententests für unsere Dienst Ebene erstellt haben. Als nächstes haben wir unsere Fluss Steuerungslogik getestet, indem wir Komponententests für unsere Controller Schicht erstellen. Beim Testen unserer Dienst Schicht haben wir unsere Tests für unsere Dienst Ebene von unserer Repository-Schicht isoliert, indem wir unsere Repository-Ebene durch die. Beim Testen der Controller Schicht isolieren wir unsere Tests für unsere Controller Schicht durch das Durchsuchen der Dienst Ebene.

In der nächsten Iterationen wird die Contact Manager-Anwendung so geändert, dass Sie Kontaktgruppen unterstützt. Wir fügen diese neue Funktionalität unserer Anwendung mithilfe eines Software Entwurfsprozesses hinzu, der als Test gesteuerte Entwicklung bezeichnet wird.

> [!div class="step-by-step"]
> [Zurück](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Weiter](iteration-6-use-test-driven-development-vb.md)
