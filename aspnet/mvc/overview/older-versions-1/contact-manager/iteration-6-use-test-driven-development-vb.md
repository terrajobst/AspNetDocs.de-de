---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iterations #6 – Verwenden der Test gesteuerten Entwicklung (VB) | Microsoft-Dokumentation'
author: microsoft
description: In dieser sechsten Iterationen fügen wir der Anwendung neue Funktionen hinzu, indem wir zuerst Komponententests schreiben und Code für die Komponententests schreiben. In diesem Iterations,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: b166a1c6af29206d43558fa7de447c3f4da2ddfe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78492699"
---
# <a name="iteration-6--use-test-driven-development-vb"></a>Iterations #6 – Verwenden der Test gesteuerten Entwicklung (VB)

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> In dieser sechsten Iterationen fügen wir der Anwendung neue Funktionen hinzu, indem wir zuerst Komponententests schreiben und Code für die Komponententests schreiben. In dieser Iterationen fügen wir Kontaktgruppen hinzu.

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

In der vorherigen Iterationen der Kontakt-Manager-Anwendung wurden Komponententests erstellt, um ein Sicherheitsnetz für unseren Code bereitzustellen. Die Motivation für die Erstellung der Komponententests bestand darin, den Code stabiler zu ändern. Mit Komponententests können wir glücklicherweise Änderungen am Code vornehmen und sofort wissen, ob die vorhandene Funktionalität beschädigt ist.

In dieser Iterationen werden Komponententests für einen völlig anderen Zweck verwendet. In dieser Iterationen verwenden wir Komponententests als Teil einer Anwendungs Entwurfs Philosophie namens *Test gesteuerte Entwicklung*. Wenn Sie die Test gesteuerte Entwicklung testen, schreiben Sie zuerst Tests und schreiben dann Code für die Tests.

Genauer: bei der Test gesteuerten Entwicklung gibt es drei Schritte, die Sie beim Erstellen von Code (rot/grün/Umgestaltung) ausführen:

1. Schreiben eines fehlgeschlagenen Komponententests (rot)
2. Schreiben von Code, der den Komponenten Test übergibt (grün)
3. Umgestalten des Codes (Umgestaltung)

Zuerst schreiben Sie den Komponenten Test. Der Komponenten Test sollte ihre Absicht zum Verhalten Ihres Codes Ausdrücken. Wenn Sie den Komponenten Test erstmalig erstellen, sollte der Komponenten Test fehlschlagen. Der Test sollte fehlschlagen, da Sie noch keinen Anwendungscode geschrieben haben, der den Test erfüllt.

Als nächstes schreiben Sie genau genug Code, damit der Komponenten Test erfolgreich durchlaufen wird. Ziel ist es, den Code in der verzögensten, sloppsten und schnellsten Weise zu schreiben. Sie sollten keine Zeit in Bezug auf die Architektur Ihrer Anwendung verschwenden. Stattdessen sollten Sie sich darauf konzentrieren, den minimalen Code zu schreiben, der erforderlich ist, um die vom Komponenten Test ausgedrückte Absicht zu erfüllen.

Nachdem Sie ausreichend Code geschrieben haben, können Sie einen Schritt zurückgehen und die Gesamtarchitektur Ihrer Anwendung in Erwägung gezogen. In diesem Schritt können Sie Ihren Code umschreiben (umgestalten), indem Sie die Vorteile von Software Entwurfsmustern (z. b. das Repository-Muster) nutzen, sodass der Code besser verwaltbar ist. In diesem Schritt können Sie den Code in diesem Schritt mühelos neu schreiben, da der Code von Komponententests abgedeckt wird.

Es gibt viele Vorteile, die sich aus der Übung der Test gesteuerten Entwicklung ergeben. Zuerst zwingt die Test gesteuerte Entwicklung Sie, sich auf den Code zu konzentrieren, der eigentlich geschrieben werden muss. Da Sie sich ständig auf das Schreiben von ausreichendem Code zum Durchführen eines bestimmten Tests konzentrieren, werden Sie daran gehindert, in das Unkraut zu gehen und riesige Mengen an Code zu schreiben, die Sie niemals verwenden werden.

Zweitens zwingt eine "Test First"-Entwurfsmethodik, Code aus der Perspektive zu schreiben, wie der Code verwendet werden soll. Anders ausgedrückt: Wenn Sie die Test gesteuerte Entwicklung durchlaufen, schreiben Sie ständig die Tests aus der Perspektive des Benutzers. Aus diesem Grund kann die Test gesteuerte Entwicklung zu sauberen und verständlicheren APIs führen.

Zum Schluss zwingt die Test gesteuerte Entwicklung das Schreiben von Komponententests im Rahmen des normalen Prozesses zum Schreiben einer Anwendung. Als Projekt Stichtag ist das Testen in der Regel die erste Aufgabe, die das Fenster verlässt. Wenn Sie die Test gesteuerte Entwicklung betreiben, ist es jedoch wahrscheinlicher, dass Sie mit dem Schreiben von Komponententests in Bezug auf das Schreiben von Komponententests zu tun haben, da Komponententests durch die Test gesteuerte Entwicklung für den Prozess der Erstellung einer Anwendung zentral sind.

> [!NOTE] 
> 
> Um mehr über die Test gesteuerte Entwicklung zu erfahren, empfiehlt es sich, das Michael-Feathers-Buch zu lesen, das **effektiv mit Legacy Code arbeitet**.

In dieser Iterationen fügen wir unserer Contact Manager-Anwendung ein neues Feature hinzu. Wir fügen Unterstützung für Kontaktgruppen hinzu. Sie können Kontaktgruppen verwenden, um Ihre Kontakte in Kategorien wie z. b. Geschäfts-und Friend-Gruppen zu organisieren.

Wir fügen diese neue Funktionalität unserer Anwendung hinzu, indem wir einen Prozess der Test gesteuerten Entwicklung befolgen. Wir schreiben zuerst unsere Komponententests, und wir schreiben den gesamten Code für diese Tests.

## <a name="what-gets-tested"></a>Was wird getestet?

Wie bereits in der vorherigen Iterationen erläutert, schreiben Sie in der Regel keine Komponententests für Datenzugriffs Logik oder Sicht Logik. Sie schreiben keine Komponententests für die Datenzugriffs Logik, da der Zugriff auf eine Datenbank relativ langsam ist. Sie schreiben keine Komponententests für die Ansichts Logik, da der Zugriff auf eine Ansicht das Einrichten eines Webservers erfordert, bei dem es sich um einen relativ langsamen Vorgang handelt. Sie sollten keinen Komponenten Test schreiben, es sei denn, der Test kann immer wiederholt ausgeführt werden.

Da die Test gesteuerte Entwicklung von Komponententests gesteuert wird, konzentrieren wir uns zunächst auf das Schreiben von Controller und Geschäftslogik. Wir vermeiden das Berühren der Datenbank oder Sichten. Wir werden die Datenbank erst am Ende dieses Lernprogramms ändern oder unsere Ansichten erstellen. Wir beginnen mit dem, was getestet werden kann.

## <a name="creating-user-stories"></a>Erstellen von User Stories

Beim Testen der Test gesteuerten Entwicklung beginnen Sie immer mit dem Schreiben eines Tests. Dadurch wird sofort die Frage gestellt: wie entscheiden Sie, welchen Test zuerst geschrieben werden soll? Um diese Frage zu beantworten, sollten Sie eine Reihe von [*User Stories*](http://en.wikipedia.org/wiki/User_stories)schreiben.

Eine User Story ist eine sehr kurze Beschreibung einer Software Anforderung (in der Regel ein Satz). Dies sollte eine nicht technische Beschreibung einer Anforderung sein, die aus der Benutzer Perspektive geschrieben wurde.

Hier finden Sie die Benutzer Textabschnitte, die die Features beschreiben, die für die neue Funktion der Kontaktgruppe erforderlich sind:

1. Der Benutzer kann eine Liste von Kontaktgruppen anzeigen.
2. Der Benutzer kann eine neue Kontaktgruppe erstellen.
3. Der Benutzer kann eine vorhandene Kontaktgruppe löschen.
4. Der Benutzer kann beim Erstellen eines neuen Kontakts eine Kontaktgruppe auswählen.
5. Der Benutzer kann eine Kontaktgruppe auswählen, wenn ein vorhandener Kontakt bearbeitet wird.
6. In der Index Ansicht wird eine Liste der Kontaktgruppen angezeigt.
7. Wenn ein Benutzer auf eine Kontaktgruppe klickt, wird eine Liste mit übereinstimmenden Kontakten angezeigt.

Beachten Sie, dass diese Liste von User Stories von einem Kunden vollständig verständlich ist. Technische Implementierungsdetails werden nicht erwähnt.

Während der Anwendung der Anwendung kann der Satz von User Storys besser verfeinert werden. Sie können eine User Story in mehrere Storys unterbrechen (Anforderungen). Beispielsweise können Sie entscheiden, dass das Erstellen einer neuen Kontaktgruppe eine Überprüfung beinhalten soll. Wenn Sie eine Kontaktgruppe ohne Namen einreichen, sollte ein Validierungs Fehler zurückgegeben werden.

Nachdem Sie eine Liste von User Stories erstellt haben, können Sie Ihren ersten Komponenten Test schreiben. Zunächst erstellen Sie einen Komponenten Test, um die Liste der Kontaktgruppen anzuzeigen.

## <a name="listing-contact-groups"></a>Auflisten von Kontaktgruppen

Der erste User Story ist, dass ein Benutzer in der Lage sein sollte, eine Liste der Kontaktgruppen anzuzeigen. Wir müssen diese Story mit einem Test Ausdrücken.

Erstellen Sie einen neuen Komponenten Test, indem Sie mit der rechten Maustaste auf den Ordner "Controllers" im Projekt "ContactManager. Tests" klicken, " **Hinzufügen", "neuer Test**" und "Komponenten **Test** Vorlage" auswählen (siehe Abbildung 1) Benennen Sie den neuen Komponenten Test groupcontrollertest. vb, und klicken Sie auf die Schaltfläche **OK** .

[![Hinzufügen des groupcontrollertest-Komponententests](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen des groupcontrollertest-Komponententests ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image2.png))

Der erste Komponenten Test ist in der Liste 1 enthalten. Mit diesem Test wird überprüft, ob die Index ()-Methode des Gruppen Controllers eine Gruppe von Gruppen zurückgibt. Der Test überprüft, ob in den Ansichts Daten eine Auflistung von Gruppen zurückgegeben wird.

**Codebeispiel 1-controllers\groupcontrollertest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Wenn Sie den Code zum ersten Mal in der Liste 1 in Visual Studio eingeben, werden viele rote Wellenlinien angezeigt. Die GroupController-oder Group-Klassen wurden nicht erstellt.

An diesem Punkt können wir die Anwendung selbst erstellen, damit wir den ersten Komponenten Test ausführen können. Das ist gut. Dies zählt als fehlgeschlagenen Test. Daher verfügen wir jetzt über die Berechtigung, mit dem Schreiben von Anwendungscode zu beginnen. Wir müssen genug Code schreiben, um den Test auszuführen.

Die Group Controller-Klasse in der Liste 2 enthält das erforderliche Minimum an Code, das zum Durchlaufen des Komponententests erforderlich ist. Die Index ()-Aktion gibt eine statisch codierte Liste von Gruppen zurück (die Group-Klasse ist in der Liste 3 definiert).

**Codebeispiel 2-controllers\groupcontroller.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Codebeispiel 3: models\group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Nachdem wir dem Projekt die GroupController-Klasse und die Group-Klasse hinzugefügt haben, wird der erste Komponenten Test erfolgreich abgeschlossen (siehe Abbildung 2). Wir haben die mindestens erforderliche Arbeit zum Durchlaufen des Tests durchgeführt. Es ist Zeit, zu feiern.

[![Erfolg!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Abbildung 02**: Erfolg! ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image4.png))

## <a name="creating-contact-groups"></a>Erstellen von Kontaktgruppen

Nun können wir mit dem zweiten User Story fortfahren. Wir müssen neue Kontaktgruppen erstellen können. Wir müssen diese Absicht mit einem Test Ausdrücken.

Der Test in der Liste 4 überprüft, ob der Aufruf der Create ()-Methode mit einer neuen Gruppe die Gruppe der Liste der Gruppen hinzufügt, die von der Index ()-Methode zurückgegeben werden. Anders ausgedrückt: Wenn ich eine neue Gruppe erstelle, sollte ich die neue Gruppe aus der Liste der von der Index ()-Methode zurückgegebenen Gruppen zurückholen können.

**Codebeispiel 4-controllers\groupcontrollertest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Der Test in der Liste 4 Ruft die Group Controller Create ()-Methode mit einer neuen Kontaktgruppe auf. Als nächstes überprüft der Test, ob beim Aufrufen der Group Controller Index ()-Methode die neue Gruppe in den Ansichts Daten zurückgegeben wird.

Der geänderte Gruppen Controller in der Liste 5 enthält die minimalen Änderungen, die erforderlich sind, um den neuen Test zu übergeben.

**Auflisten 5-controllers\groupcontroller.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Der Gruppen Controller in der Liste 5 verfügt über eine neue Create ()-Aktion. Durch diese Aktion wird einer Gruppe von Gruppen eine Gruppe hinzugefügt. Beachten Sie, dass die Index ()-Aktion geändert wurde, um den Inhalt der Auflistung von Gruppen zurückzugeben.

Noch einmal haben wir die erforderliche Mindestmenge an Arbeit durchgeführt, die zum Durchführen des Komponententests erforderlich ist. Nachdem wir diese Änderungen am Gruppen Controller vorgenommen haben, bestehen alle Komponententests.

## <a name="adding-validation"></a>Hinzufügen der Validierung

Diese Anforderung wurde im User Story nicht explizit angegeben. Es ist jedoch sinnvoll, dass eine Gruppe einen Namen hat. Andernfalls wäre das Organisieren von Kontakten in Gruppen nicht sehr nützlich.

Die Liste 6 enthält einen neuen Test, der diese Absicht ausdrückt. Mit diesem Test wird überprüft, ob beim Versuch, eine Gruppe ohne Angabe eines Namens zu erstellen, im Modell Status eine Validierungs Fehlermeldung angezeigt wird.

**Codebeispiel 6-controllers\groupcontrollertest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Um diesen Test zu erfüllen, müssen wir der Group-Klasse eine Name-Eigenschaft hinzufügen (siehe Codebeispiel 7). Außerdem müssen wir der Aktion "Create ()" für den Group Controller-Vorgang ein kleines wenig Validierungs Logik hinzufügen (siehe Codebeispiel 8).

**Codebeispiel 7: models\group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Auflisten von 8-controllers\groupcontroller.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Beachten Sie, dass die Group Controller Create ()-Aktion jetzt sowohl die Validierung als auch die Daten Bank Logik enthält. Derzeit besteht die vom Gruppen Controller verwendete Datenbank nur aus einer Sammlung im Arbeitsspeicher.

## <a name="time-to-refactor"></a>Umgestaltungs Zeit

Der dritte Schritt in rot/grün/Umgestaltung ist das Umgestaltungs Element. An diesem Punkt müssen wir aus dem Code zurückgehen und berücksichtigen, wie wir unsere Anwendung umgestalten können, um den Entwurf zu verbessern. Die Umgestaltungs Stufe ist die Phase, bei der wir uns die beste Methode zum Implementieren von Software Entwurfs Prinzipien und-Mustern vorstellen.

Wir können unseren Code beliebig ändern, um den Code Entwurf zu verbessern. Wir haben ein Sicherheitsnetz von Unittests, die verhindern, dass vorhandene Funktionen unterbunden werden.

Derzeit ist unser Gruppen Controller ein Chaos aus der Perspektive eines guten Softwaredesigns. Der Gruppen Controller enthält ein Durcheinander mit dem Validierungs-und Datenzugriffs Code. Um das Prinzip der einzelnen Verantwortung zu vermeiden, müssen wir diese Aspekte in verschiedene Klassen aufteilen.

Unsere umgestaltete Group Controller-Klasse ist in der Liste 9 enthalten. Der Controller wurde geändert, um die Dienst Ebene "ContactManager" zu verwenden. Dies ist die gleiche Dienst Schicht, die wir mit dem Contact Controller verwenden.

Codebeispiel 10 enthält die neuen Methoden, die der ContactManager-Dienst Ebene hinzugefügt wurden, um das validieren, auflisten und Erstellen von Gruppen zu unterstützen. Die icontactmanagerservice-Schnittstelle wurde so aktualisiert, dass Sie die neuen Methoden enthält.

In der Liste 11 ist eine neue fakecontactmanagerrepository-Klasse enthalten, die die icontactmanagerrepository-Schnittstelle implementiert. Anders als bei der entitycontactmanagerrepository-Klasse, die auch die icontactmanagerrepository-Schnittstelle implementiert, kommuniziert unsere neue fakecontactmanagerrepository-Klasse nicht mit der Datenbank. Die fakecontactmanagerrepository-Klasse verwendet eine Speicher interne Auflistung als Proxy für die Datenbank. Wir verwenden diese Klasse in den Komponententests als gefälschte Repository-Schicht.

**Codebeispiel 9-controllers\groupcontroller.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Auflisten von 10-controllers\contactmanagerservice.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Codebeispiel 11-controllers\fakecontactmanagerrepositorien.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]

Das Ändern der icontactmanagerrepository-Schnittstelle erfordert, dass die Methoden "up Group ()" und "List Groups ()" in der entitycontactmanagerrepository-Klasse implementiert. Die verzögert und schnellste Methode hierfür ist das Hinzufügen von Stub-Methoden, die wie folgt aussehen:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]

Zum Schluss erfordern diese Änderungen am Entwurf unserer Anwendung, dass wir einige Änderungen an den Komponententests vornehmen müssen. Beim Ausführen der Komponententests muss nun das fakecontactmanagerrepository-Objekt verwendet werden. Die aktualisierte groupcontrollertest-Klasse ist in der Liste 12 enthalten.

**Auflisten 12-controllers\groupcontrollertest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Nachdem wir alle diese Änderungen vorgenommen haben, werden alle Komponententests bestanden. Wir haben den gesamten Kreis von rot/grün/Umgestaltung abgeschlossen. Wir haben die ersten zwei User Stories implementiert. Wir verfügen nun über unterstützende Komponententests für die in den User Storys ausgedrückten Anforderungen. Das Implementieren der restlichen Benutzer Textabschnitte umfasst das wiederholen desselben Zyklen von rot/grün/Umgestaltung.

## <a name="modifying-our-database"></a>Ändern der Datenbank

Leider ist unsere Arbeit nicht erledigt, obwohl wir alle Anforderungen erfüllt haben, die von unseren Komponententests ausgedrückt werden. Wir müssen weiterhin unsere Datenbank ändern.

Wir müssen eine neue Gruppendaten Bank Tabelle erstellen. Folgen Sie diesen Schritten:

1. Klicken Sie im Server-Explorer Fenster mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die Menüoption **neue Tabelle hinzufügen**aus.
2. Geben Sie die beiden unten beschriebenen Spalten in der Tabellen-Designer ein.
3. Markieren Sie die ID-Spalte als Primärschlüssel und Identitäts Spalte.
4. Speichern Sie die neue Tabelle mit den namens Gruppen, indem Sie auf das Symbol der Diskette klicken.

<a id="0.12_table01"></a>

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | int | False |
| Name | nvarchar(50) | False |

Als nächstes müssen wir alle Daten aus der Tabelle "Contacts" löschen (andernfalls können wir keine Beziehung zwischen den Tabellen "Contacts" und "Groups" erstellen). Folgen Sie diesen Schritten:

1. Klicken Sie mit der rechten Maustaste auf die Tabelle Kontakte, und wählen Sie die Menüoption **Tabellendaten anzeigen**aus.
2. Löschen Sie alle Zeilen.

Als nächstes müssen wir eine Beziehung zwischen der Datenbanktabelle "Groups" und der vorhandenen Datenbanktabelle "Contacts" definieren. Folgen Sie diesen Schritten:

1. Doppelklicken Sie im Fenster Server-Explorer auf die Tabelle Kontakte, um die Tabellen-Designer zu öffnen.
2. Fügen Sie der Tabelle Contacts eine neue ganzzahlige Spalte mit dem Namen GroupID hinzu.
3. Klicken Sie auf die Schaltfläche Beziehung, um das Dialogfeld Fremdschlüssel Beziehungen zu öffnen (siehe Abbildung 3).
4. Klicken Sie auf die Schaltfläche Add.
5. Klicken Sie auf die Schaltfläche mit den Auslassungs Punkten neben der Schaltfläche Tabellen-und Spaltenspezifikation.
6. Wählen Sie im Dialogfeld Tabellen und Spalten die Option Gruppen als Primärschlüssel Tabelle und ID als Primärschlüssel Spalte aus. Wählen Sie Kontakte als Fremdschlüssel Tabelle und GroupID als Fremdschlüssel Spalte aus (siehe Abbildung 4). Klicken Sie auf die Schaltfläche "OK".
7. Wählen Sie unter **INSERT-und Update-Spezifikation**den Wert **Cascade** for **Delete Rule**aus.
8. Klicken Sie auf die Schaltfläche Schließen, um das Dialogfeld Fremdschlüssel Beziehungen zu schließen.
9. Klicken Sie auf die Schaltfläche speichern, um die Änderungen in der Tabelle Kontakte zu speichern.

[![Erstellen einer Datenbanktabellen Beziehung](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Abbildung 03**: Erstellen einer Datenbanktabellen Beziehung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image6.png))

[![angeben von Tabellen Beziehungen](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Abbildung 04**: Angeben von Tabellen Beziehungen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image8.png))

### <a name="updating-our-data-model"></a>Aktualisieren des Datenmodells

Als nächstes müssen wir das Datenmodell aktualisieren, um die neue Datenbanktabelle darzustellen. Folgen Sie diesen Schritten:

1. Doppelklicken Sie im Ordner Models auf die Datei contactmanagermodel. edmx, um die Entity Designer zu öffnen.
2. Klicken Sie mit der rechten Maustaste auf die Designer Oberfläche, und wählen Sie die Menüoption **Modell aus Datenbank aktualisieren aus**.
3. Wählen Sie im Update-Assistenten die Tabelle Gruppen aus, und klicken Sie auf die Schaltfläche Fertigstellen (siehe Abbildung 5).
4. Klicken Sie mit der rechten Maustaste auf die Entität Gruppen, und wählen Sie die Option **Umbenennen** Ändern Sie den Namen der *Groups* -Entität in *Group* (Singular).
5. Klicken Sie mit der rechten Maustaste auf die Gruppen Navigations Eigenschaft, die unten in der Entität "Contact" angezeigt wird. Ändern Sie den Namen der *Gruppen* Navigations Eigenschaft in *Group* (Singular).

[![Aktualisieren eines Entity Framework Modells aus der Datenbank](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Abbildung 05**: Aktualisieren eines Entity Framework Modells aus der Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image10.png))

Nachdem Sie diese Schritte ausgeführt haben, stellt das Datenmodell die Tabellen "Contacts" und "Groups" dar. Die Entity Designer sollte beide Entitäten anzeigen (siehe Abbildung 6).

[![Entity Designer anzeigen von Gruppe und Kontakt](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Abbildung 06**: Entity Designer anzeigen von Gruppe und Kontakt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Erstellen der Repository-Klassen

Als nächstes müssen wir unsere Repository-Klasse implementieren. Im Verlauf dieser Iterationen haben wir der icontactmanagerrepository-Schnittstelle mehrere neue Methoden hinzugefügt, während wir Code schreiben, um die Komponententests zu erfüllen. Die endgültige Version der icontactmanagerrepository-Schnittstelle ist in der Liste 14 enthalten.

**Codebeispiel 14: models\icontactmanagerrepositoriy.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Wir haben in unserer echten entitycontactmanagerrepository-Klasse keine der Methoden zum Arbeiten mit Kontaktgruppen implementiert. Derzeit verfügt die entitycontactmanagerrepository-Klasse über Stubmethoden für jede der Kontaktgruppen Methoden, die in der icontactmanagerrepository-Schnittstelle aufgelistet sind. Beispielsweise sieht die ListGroups ()-Methode zurzeit wie folgt aus:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Mit den Stub-Methoden konnten wir unsere Anwendung kompilieren und die Komponententests durchlaufen. Jetzt ist es aber an der Zeit, diese Methoden tatsächlich zu implementieren. Die endgültige Version der entitycontactmanagerrepository-Klasse ist in der Liste 13 enthalten.

**Auflisten 13-models\entitycontactmanagerrepositorien.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Erstellen der Sichten

ASP.NET MVC-Anwendung, wenn Sie die standardmäßige ASP.net-Ansichts-Engine verwenden. Daher erstellen Sie keine Sichten als Reaktion auf einen bestimmten Komponenten Test. Da eine Anwendung jedoch ohne Ansichten nutzlos wäre, können wir diese Iterations Iterationen nicht durchführen, ohne die in der Contact Manager-Anwendung enthaltenen Sichten zu erstellen und zu ändern.

Zum Verwalten von Kontaktgruppen müssen die folgenden neuen Ansichten erstellt werden (siehe Abbildung 7):

- Views\group\index.aspx-zeigt die Liste der Kontaktgruppen an
- Views\group\delete.aspx-zeigt das Bestätigungsformular zum Löschen einer Kontaktgruppe an

[![der Gruppen Index Ansicht](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Abbildung 07**: Ansicht "Gruppen Index" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image14.png))

Wir müssen die folgenden vorhandenen Ansichten so ändern, dass Sie Kontaktgruppen enthalten:

- Views\home\kreate.aspx
- Views\home\edit.aspx
- Views\home\index.aspx

Die geänderten Ansichten sehen Sie sich die Visual Studio-Anwendung an, die dieses Tutorial begleitet. Abbildung 8 veranschaulicht z. b. die Kontakt Index Sicht.

[![der Ansicht "Kontakt index"](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Abbildung 08**: Ansicht "Kontakt index" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-6-use-test-driven-development-vb/_static/image16.png))

## <a name="summary"></a>Zusammenfassung

In dieser Iterationen haben wir unserer Contact Manager-Anwendung neue Funktionen hinzugefügt, indem wir eine Test gesteuerte Entwicklungsmethode für Entwicklungsanwendungen befolgen. Wir haben mit der Erstellung eines Satzes von User Stories begonnen. Wir haben eine Reihe von Komponententests erstellt, die den Anforderungen entsprechen, die von den User Storys ausgedrückt werden. Schließlich haben wir gerade genug Code geschrieben, um die Anforderungen zu erfüllen, die von den Komponententests ausgedrückt werden.

Nachdem das Schreiben von ausreichendem Code abgeschlossen wurde, um die von den Komponententests ausgedrückten Anforderungen zu erfüllen, haben wir unsere Datenbank und Sichten aktualisiert. Wir haben unserer Datenbank eine neue Gruppentabelle hinzugefügt und unser Entity Framework Datenmodell aktualisiert. Außerdem haben wir einen Satz von Sichten erstellt und geändert.

In der nächsten Iterations--der abschließenden Iterations-wird die Anwendung neu geschrieben, um AJAX zu nutzen. Durch die Nutzung von AJAX verbessern wir die Reaktionsfähigkeit und Leistung der Kontakt-Manager-Anwendung.

> [!div class="step-by-step"]
> [Zurück](iteration-5-create-unit-tests-vb.md)
> [Weiter](iteration-7-add-ajax-functionality-vb.md)
