---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Aktivieren automatisierter Unittests | Microsoft-Dokumentation
author: microsoft
description: In Schritt 12 wird gezeigt, wie Sie eine Reihe automatisierter Unittests entwickeln, mit denen unsere nerddinner-Funktionalität überprüft wird, und die uns die Gewissheit geben kann, dass Änderungen vorgenommen werden können...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 09a7aa186605a6cce48ee94028425ded957c00d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435591"
---
# <a name="enable-automated-unit-testing"></a>Aktivieren von automatisierten Komponententests

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 12 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> In Schritt 12 wird gezeigt, wie Sie eine Reihe automatisierter Unittests entwickeln, mit denen unsere nerddinner-Funktionalität überprüft wird. Dadurch erhalten wir die Gewissheit, dass Sie in Zukunft Änderungen und Verbesserungen an der Anwendung vornehmen können.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-12-unit-testing"></a>Nerddinner Step 12: Komponententests

Wir entwickeln eine Reihe automatisierter Komponententests, mit denen unsere nerddinner-Funktionalität überprüft wird, und die uns das Vertrauen bietet, dass die Anwendung in Zukunft geändert werden kann.

### <a name="why-unit-test"></a>Gründe für den Komponenten Test

Auf dem Laufwerk, in dem Sie arbeiten, haben Sie einen plötzlichen Blitz von Inspiration zu einer Anwendung, an der Sie arbeiten. Sie werden feststellen, dass es eine Änderung gibt, die Sie implementieren können, um die Anwendung erheblich zu verbessern. Dabei kann es sich um ein Refactoring handeln, das den Code bereinigt, eine neue Funktion hinzufügt oder einen Fehler behebt.

Die Frage, die Sie beim Eintreffen Ihres Computers trifft, ist – "wie sicher ist es, diese Verbesserung vorzunehmen". Was geschieht, wenn die Änderung eine Nebeneffekte hat oder etwas unterbricht? Die Änderung ist möglicherweise einfach und dauert nur wenige Minuten, aber was ist, wenn es Stunden dauert, alle Anwendungs Szenarios manuell zu testen? Was geschieht, wenn Sie vergessen, ein Szenario abzudecken, und eine unterbrochene Anwendung in die Produktion übergeht? Macht diese Verbesserung wirklich den gesamten Aufwand?

Automatisierte Komponententests können ein Sicherheitsnetz bereitstellen, mit dem Sie Ihre Anwendungen kontinuierlich verbessern können, und vermeiden, dass sich der Code, an dem Sie arbeiten, nicht sicher ist. Dank automatisierter Tests, mit denen die Funktionalität schnell überprüft werden kann, können Sie mit Sicherheit codieren – und Sie können Verbesserungen vornehmen. Außerdem helfen Sie bei der Erstellung von Lösungen, die besser verwaltierbar sind und eine längere Lebensdauer haben, was zu einer wesentlich höheren Rendite führt.

Mit dem ASP.NET-MVC-Framework können Sie Komponententests für Anwendungsfunktionen ganz einfach und natürlich ganz einfach machen. Außerdem wird ein Test gesteuertem Entwicklungs Workflow (Test-Based Development, TDD) ermöglicht, der die Test basierte Entwicklung ermöglicht.

### <a name="nerddinnertests-project"></a>Nerddinner. Tests-Projekt

Beim Erstellen unserer "nerddinner"-Anwendung zu Beginn dieses Tutorials wurde ein Dialogfeld mit der Frage angezeigt, ob ein Komponenten Testprojekt erstellt werden soll, das mit dem Anwendungsprojekt zusammengeführt werden soll:

![](enable-automated-unit-testing/_static/image1.png)

Wir haben das Optionsfeld "Ja, Erstellen eines Komponenten Testprojekts" ausgewählt – Dies führte dazu, dass der Projekt Mappe das Projekt "nerddinner. Tests" hinzugefügt wurde:

![](enable-automated-unit-testing/_static/image2.png)

Das Projekt "nerddinner. Tests" verweist auf die Projektassembly "nerddinner Application" und ermöglicht uns das einfache Hinzufügen automatisierter Tests, mit denen die Anwendungs Funktionalität überprüft wird.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Erstellen von Komponenten Tests für die Dinner Model-Klasse

Fügen wir dem Projekt "nerddinner. Tests" einige Tests hinzu, mit denen die Dinner-Klasse überprüft wird, die wir beim Erstellen unserer Modell Ebene erstellt haben.

Wir beginnen mit dem Erstellen eines neuen Ordners im Testprojekt mit dem Namen "Models", in dem wir unsere Modell bezogenen Tests platzieren. Klicken Sie dann mit der rechten Maustaste auf den Ordner, und wählen Sie den Menübefehl **Add-&gt;New Test** aus. Dadurch wird das Dialogfeld "neuen Test hinzufügen" angezeigt.

Wir erstellen einen "Komponenten Test" und nennen ihn "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Wenn Sie auf die Schaltfläche "OK" klicken, fügt Visual Studio dem Projekt eine DinnerTest.cs-Datei hinzu (und öffnet sie):

![](enable-automated-unit-testing/_static/image4.png)

Die Standardvorlage für Visual Studio-Komponententests enthält eine Reihe von Code für den Kessel, den ich ein wenig messy finde. Bereinigen Sie ihn, um nur den folgenden Code zu enthalten:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Das [TestClass]-Attribut der obigen dinnertest-Klasse identifiziert es als eine Klasse, die Tests enthalten wird, sowie einen optionalen Test Initialisierungs-und Beendigungs Code. Wir können darin Tests definieren, indem wir öffentliche Methoden hinzufügen, die über ein [TestMethod]-Attribut verfügen.

Im folgenden finden Sie die ersten der beiden Tests, die wir für unsere Dinner-Klasse hinzufügen werden. Der erste Test überprüft, ob unser Dinner ungültig ist, wenn ein neues Dinner erstellt wird, ohne dass alle Eigenschaften ordnungsgemäß festgelegt wurden. Der zweite Test überprüft, ob unser Dinner gültig ist, wenn für ein Dinner alle Eigenschaften mit gültigen Werten festgelegt sind:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Sie werden feststellen, dass unsere Testnamen sehr explizit (und etwas ausführlicher) sind. Dies ist der Fall, da wir möglicherweise Hunderte oder Tausende von kleinen Tests erstellen, und wir möchten, dass Sie schnell die Absicht und das Verhalten der einzelnen Tests ermitteln können (insbesondere, wenn wir eine Liste von Fehlern in einem Test Runner betrachten). Die Testnamen sollten nach der Funktionalität benannt werden, die Sie testen. Oben verwenden wir das Benennungs Muster "Nomen\_\_Verb".

Die Tests werden mit dem Testmuster "AAA" strukturiert – das für "Arrange, Act, Assert" steht:

- Anordnen: richten Sie die zu testende Einheit ein.
- Act: Testen der zu testenden und Erfassungs Ergebnisse
- Assert: Überprüfen Sie das Verhalten.

Beim Schreiben von Tests soll vermieden werden, dass die einzelnen Tests zu stark funktionieren. Stattdessen sollte jeder Test nur ein einzelnes Konzept überprüfen (was das Ermitteln der Fehlerursache erheblich vereinfacht). Eine gute Richtlinie besteht darin, zu versuchen, nur für jeden Test eine einzelne Assert-Anweisung zu verwenden. Wenn Sie mehr als eine Assert-Anweisung in einer Testmethode haben, stellen Sie sicher, dass Sie alle zum Testen des gleichen Konzepts verwendet werden. Nehmen Sie im Zweifelsfall einen weiteren Test vor.

### <a name="running-tests"></a>Tests werden ausgeführt

Visual Studio 2008 Professional (und höhere Editionen) umfasst einen integrierten Test Runner, der zum Ausführen von Visual Studio-Komponenten Testprojekten innerhalb der IDE verwendet werden kann. Wir können den Menübefehl **Test-&gt;Run-&gt;all Tests in Solution** (oder STRG R, A) auswählen, um alle Komponenten Tests auszuführen. Alternativ können wir den Cursor in einer bestimmten Testklasse oder Testmethode positionieren und den **Test-&gt;Run-&gt;Tests im aktuellen Kontext** Menübefehl (oder STRG R, t) verwenden, um eine Teilmenge der Komponenten Tests auszuführen.

Positionieren Sie den Cursor innerhalb der Klasse "dinnertest", und geben Sie "STRG R, T" ein, um die beiden soeben definierten Tests auszuführen. Wenn wir dies tun, wird in Visual Studio ein Fenster "Testergebnisse" angezeigt, und wir sehen, dass die Ergebnisse des Testlaufs darin aufgeführt werden:

![](enable-automated-unit-testing/_static/image5.png)

*Hinweis: das Visual Studio-Testergebnis Fenster zeigt die Klassennamen Spalte nicht standardmäßig an. Sie können dies hinzufügen, indem Sie mit der rechten Maustaste in das Fenster Testergebnisse klicken und den Menübefehl Spalten hinzufügen/entfernen verwenden.*

In den beiden Tests wurde nur ein Bruchteil einer Sekunde zum Ausführen von – benötigt, ebenso wie Sie sehen können, dass beide Tests bestanden wurden. Wir können Sie nun erweitern und erweitern, indem wir zusätzliche Tests erstellen, mit denen bestimmte Regel Überprüfungen überprüft werden. Außerdem werden die beiden Hilfsmethoden "isuserhost ()" und "isuserregistered () –" behandelt, die wir der Dinner-Klasse hinzugefügt haben. Wenn Sie alle diese Tests für die Dinner-Klasse durchführen, wird es viel einfacher und sicherer, in Zukunft neue Geschäftsregeln und Validierungen hinzuzufügen. Wir können unsere neue Regellogik zu Dinner hinzufügen und dann innerhalb von Sekunden überprüfen, ob die vorherige Logik Funktionalität nicht beschädigt wurde.

Beachten Sie, dass die Verwendung eines beschreibenden Testnamens Ihnen das schnelle Verständnis der Überprüfung der einzelnen Tests erleichtert. Ich empfehle Ihnen, den Menübefehl Extras **-&gt;Optionen** zu öffnen, den Bildschirm Test Tools-&gt;Test Ausführungs Konfiguration zu öffnen und das Kontrollkästchen "doppelklicken Sie auf ein fehlgeschlagener oder nicht eindeutiger Komponenten Test" zeigt den Fehlerpunkt im Feld "Test" an. Auf diese Weise können Sie im Fenster "Testergebnisse" auf einen Fehler doppelklicken und sofort zum Assert-Fehler springen.

### <a name="creating-dinnerscontroller-unit-tests"></a>Erstellen von dinnerscontroller-Komponenten Tests

Nun erstellen wir einige Komponententests, die unsere dinnerscontroller-Funktionalität überprüfen. Beginnen Sie mit der rechten Maustaste auf den Ordner "Controllers" in unserem Test Projekt, und wählen Sie dann den Menübefehl **Add-&gt;New Test** aus. Wir erstellen einen "Komponenten Test" und nennen ihn "DinnersControllerTest.cs".

Wir erstellen zwei Testmethoden, die die Details ()-Aktionsmethode auf dem dinnerscontroller überprüfen. Der erste überprüft, ob eine Ansicht zurückgegeben wird, wenn ein vorhandenes Dinner angefordert wird. Mit dem zweiten wird überprüft, ob eine "NotFound"-Ansicht zurückgegeben wird, wenn ein nicht vorhandenes Dinner angefordert wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Der obige Code kompiliert Clean. Beim Ausführen der Tests schlagen beide jedoch fehl:

![](enable-automated-unit-testing/_static/image6.png)

Wenn wir uns die Fehlermeldungen ansehen, sehen wir, dass der Test fehlgeschlagen ist, weil unsere dinnersrepository-Klasse keine Verbindung mit einer Datenbank herstellen konnte. Unsere "nerddinner"-Anwendung verwendet eine Verbindungs Zeichenfolge für eine lokale SQL Server Express Datei, die sich im Projekt "\app\_Data" des Projekts "nerddinner Application" befindet. Da unser Projekt "nerddinner. Tests" in einem anderen Verzeichnis als dem Anwendungsprojekt kompiliert und ausgeführt wird, ist der relative Pfad Speicherort der Verbindungs Zeichenfolge falsch.

Wir *könnten* dies beheben, indem wir die SQL Express-Datenbankdatei in das Testprojekt kopieren und dann in der app. config-Datei des Testprojekts eine entsprechende Test Verbindungs Zeichenfolge hinzufügen. Dies würde dazu führen, dass die oben aufgeführten Tests blockiert werden und ausgeführt werden.

Komponententests für Code, der eine echte Datenbank verwendet, bringt jedoch eine Reihe von Herausforderungen mit sich. Dies gilt insbesondere in folgenden Fällen:

- Sie verlangsamt die Ausführungszeit von Komponententests erheblich. Desto länger dauert es, bis Tests ausgeführt werden, umso wahrscheinlicher ist es, Sie häufig auszuführen. Idealerweise möchten Sie, dass die Komponententests innerhalb von Sekunden ausgeführt werden können – und Sie haben etwas, das Sie selbst tun, wenn Sie das Projekt kompilieren.
- Die Setup-und Bereinigungs Logik innerhalb der Tests wird komplizierter. Sie möchten, dass jeder Komponenten Test isoliert und unabhängig von anderen ist (ohne Nebeneffekte oder Abhängigkeiten). Beim Arbeiten mit einer echten Datenbank müssen Sie den Status berücksichtigen und zwischen Tests zurücksetzen.

Sehen wir uns das Entwurfsmuster "Abhängigkeitsinjektion" an, das uns dabei helfen kann, diese Probleme zu umgehen und die Notwendigkeit zu vermeiden, mit unseren Tests eine echte Datenbank zu verwenden.

### <a name="dependency-injection"></a>Dependency Injection

Derzeit ist "dinnerscontroller" eng mit der dinnerrepository-Klasse "gekoppelt". "Kopplung" bezieht sich auf eine Situation, in der eine Klasse explizit von einer anderen Klasse abhängig ist, um zu funktionieren:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Da die dinnerrepository-Klasse Zugriff auf eine Datenbank benötigt, benötigt die eng gekoppelte Abhängigkeit, die die dinnerscontroller-Klasse für das dinnerrepository besitzt, eine Datenbank, damit die dinnerscontroller-Aktionsmethoden getestet werden können.

Dies können Sie umgehen, indem Sie ein Entwurfsmuster namens "Abhängigkeitsinjektion" verwenden – ein Ansatz, bei dem Abhängigkeiten (z. b. Repository-Klassen, die Datenzugriff bereitstellen) nicht mehr implizit in Klassen erstellt werden, die Sie verwenden. Stattdessen können Abhängigkeiten explizit an die Klasse übergeben werden, die Sie mithilfe von Konstruktorargumenten verwendet. Wenn die Abhängigkeiten mithilfe von Schnittstellen definiert werden, haben wir die Flexibilität, "gefälschte" Abhängigkeits Implementierungen für Komponenten Testszenarien zu übergeben. Dies ermöglicht es uns, Test spezifische Abhängigkeits Implementierungen zu erstellen, die tatsächlich keinen Zugriff auf eine Datenbank erfordern.

Um dies in Aktion zu sehen, implementieren wir die Abhängigkeitsinjektion mit unserem dinnerscontroller.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extrahieren einer idinnerrepository-Schnittstelle

Der erste Schritt besteht darin, eine neue idinnerrepository-Schnittstelle zu erstellen, die den Repository-Vertrag kapselt, den unsere Controller zum Abrufen und Aktualisieren von Abendessen benötigen.

Dieser Schnittstellen Vertrag kann manuell definiert werden, indem Sie mit der rechten Maustaste auf den Ordner "\models" klicken und dann den Menübefehl **Add-&gt;New Item** auswählen und eine neue Schnittstelle mit dem Namen IDinnerRepository.cs erstellen.

Alternativ können wir die in Visual Studio Professional (und höheren Editionen) integrierten Refactoring-Tools verwenden, um automatisch eine Schnittstelle für uns aus unserer vorhandenen dinnerrepository-Klasse zu extrahieren und zu erstellen. Um diese Schnittstelle mit vs zu extrahieren, positionieren Sie einfach den Cursor im Text-Editor der dinnerrepository-Klasse, und klicken Sie dann mit der rechten Maustaste, und wählen Sie den Menübefehl **Refactor-&gt;Extract Interface** aus:

![](enable-automated-unit-testing/_static/image7.png)

Dadurch wird das Dialogfeld "Schnittstelle extrahieren" geöffnet, und Sie werden aufgefordert, den Namen der zu erstellenden Schnittstelle einzugeben. Der Standardwert ist idinnerrepository und wählt automatisch alle öffentlichen Methoden der vorhandenen dinnerrepository-Klasse aus, die der Schnittstelle hinzugefügt werden sollen:

![](enable-automated-unit-testing/_static/image8.png)

Wenn wir auf die Schaltfläche "OK" klicken, fügt Visual Studio der Anwendung eine neue idinnerrepository-Schnittstelle hinzu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Und die vorhandene dinnerrepository-Klasse wird so aktualisiert, dass Sie die-Schnittstelle implementiert:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualisieren von dinnerscontroller zur Unterstützung der Konstruktorinjektion

Wir aktualisieren nun die dinnerscontroller-Klasse, um die neue Schnittstelle zu verwenden.

Derzeit ist "dinnerscontroller" hart codiert, sodass das Feld "dinnerrepository" immer eine dinnerrepository-Klasse ist:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Wir ändern ihn so, dass das Feld "dinnerrepository" den Typ "idinnerrepository" anstelle von "dinnerrepository" hat. Anschließend fügen wir zwei öffentliche dinnerscontroller-Konstruktoren hinzu. Einer der Konstruktoren ermöglicht, dass ein idinnerrepository als Argument übergeben wird. Der andere ist ein Standardkonstruktor, der unsere vorhandene Implementierung von dinnerrepository verwendet:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Da ASP.NET MVC standardmäßig Controller Klassen mithilfe von Standardkonstruktoren erstellt, verwendet unser dinnerscontroller zur Laufzeit weiterhin die dinnerrepository-Klasse, um Daten Zugriffe auszuführen.

Wir können nun die Komponententests aktualisieren, um eine "gefälschte" Dinner-Repository-Implementierung mit dem parameterkonstruktor zu übergeben. Dieses "gefälschte" Dinner-Repository erfordert keinen Zugriff auf eine echte Datenbank und verwendet stattdessen in-Memory-Beispiel Daten.

#### <a name="creating-the-fakedinnerrepository-class"></a>Erstellen der fakedinnerrepository-Klasse

Erstellen wir eine fakedinnerrepository-Klasse.

Zunächst erstellen wir ein "fakes"-Verzeichnis in unserem Projekt "nerddinner. Tests" und fügen dann eine neue fakedinnerrepository-Klasse hinzu (Klicken Sie mit der rechten Maustaste auf den Ordner, und wählen Sie " **Add-&gt;New Class**)" aus:

![](enable-automated-unit-testing/_static/image9.png)

Wir aktualisieren den Code so, dass die fakedinnerrepository-Klasse die idinnerrepository-Schnittstelle implementiert. Klicken Sie dann mit der rechten Maustaste darauf, und wählen Sie den Kontextmenü Befehl "Schnittstelle implementieren idinnerrepository" aus:

![](enable-automated-unit-testing/_static/image10.png)

Dies bewirkt, dass Visual Studio automatisch alle Member der idinnerrepository-Schnittstelle der fakedinnerrepository-Klasse mit standardmäßigen "Stub out"-Implementierungen hinzufügt:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Anschließend können wir die fakedinnerrepository-Implementierung so aktualisieren, dass Sie aus einer in-Memory-Liste entfernt wird,&lt;Dinner&gt; Collection als Konstruktorargument übergeben wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Wir verfügen jetzt über eine gefälschte idinnerrepository-Implementierung, die keine Datenbank erfordert, und können stattdessen eine in-Memory-Liste von Dinner-Objekten bearbeiten.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Verwenden von fakedinnerrepository mit Komponenten Tests

Kehren wir zu den dinnerscontroller-Komponententests zurück, bei denen zuvor ein Fehler aufgetreten ist, weil die Datenbank nicht verfügbar war. Wir können die Testmethoden zur Verwendung eines fakedinnerrepository aktualisieren, das mit Sample-in-Memory-Dinner-Daten für den dinnerscontroller mit dem folgenden Code aufgefüllt wurde:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Und wenn wir nun diese Tests ausführen, bestehen beide:

![](enable-automated-unit-testing/_static/image11.png)

Das beste daran ist, dass Sie nur einen Bruchteil einer Sekunde zur Ausführung benötigen und keine komplizierte Setup-/Bereinigungs Logik benötigen. Wir können jetzt den gesamten Code der dinnerscontroller-Aktionsmethode (einschließlich Auflistungen, Paging, Details, erstellen, aktualisieren und löschen) testen, ohne jemals eine Verbindung mit einer echten Datenbank herstellen zu müssen.

| **Seitiges Thema: Frameworks für die Abhängigkeitsinjektion** |
| --- |
| Das Ausführen einer manuellen Abhängigkeitsinjektion (wie oben) funktioniert problemlos, aber es wird schwieriger, wenn die Anzahl der Abhängigkeiten und Komponenten in einer Anwendung zunimmt. Für .net gibt es mehrere Frameworks für die Abhängigkeitsinjektion, die eine noch größere Flexibilität der Abhängigkeits Verwaltung bieten. Diese Frameworks, auch manchmal als "Inversion of Control" (IOC)-Container bezeichnet, bieten Mechanismen, die ein zusätzliches Maß an Konfigurations Unterstützung für die Angabe und Übergabe von Abhängigkeiten an Objekte zur Laufzeit ermöglichen (meistens mithilfe von Konstruktorinjektion). ). Zu den beliebtesten OSS-Abhängigkeits Einschleusungen und IOC-Frameworks in .net gehören: autofac, Ninject, Spring.net, StructureMap und Windsor. ASP.NET MVC stellt Erweiterbarkeits-APIs zur Verfügung, mit denen Entwickler an der Auflösung und Instanziierung von Controllern teilnehmen können, und die es ermöglicht, dass Abhängigkeitsinjektion/IOC-Frameworks in diesem Prozess ordnungsgemäß integriert werden. Durch die Verwendung eines di/IOC-Frameworks können wir auch den Standardkonstruktor aus unserem dinnerscontroller-– entfernen, der die Kopplung zwischen ihm und dem dinnerrepository vollständig entfernt. Wir verwenden keine Abhängigkeitsinjektion/IOC-Framework mit unserer "nerddinner"-Anwendung. Es ist jedoch etwas, was wir für die Zukunft in Erwägung gezogen haben, wenn die "nerddinner Code-Base" und die Funktionen gewachsen sind |

### <a name="creating-edit-action-unit-tests"></a>Erstellen von Komponenten Tests zum Bearbeiten von Aktionen

Nun erstellen wir einige Komponententests, die die Bearbeitungsfunktionen von dinnerscontroller überprüfen. Wir beginnen damit, die HTTP-Get-Version unserer Bearbeitungsaktion zu testen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Wir erstellen einen Test, mit dem überprüft wird, ob eine Sicht, die von einem dinnerformviewmodel-Objekt gestützt wird, wieder gerendert wird, wenn ein gültiges Dinner angefordert wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Wenn wir den Test ausführen, werden wir jedoch feststellen, dass ein Fehler auftritt, da eine NULL-Verweis Ausnahme ausgelöst wird, wenn die Edit-Methode auf die User.Identity.Name-Eigenschaft zugreift, um die Dinner. ishostedby ()-Prüfung auszuführen.

Das User-Objekt für die Controller-Basisklasse kapselt Details zum angemeldeten Benutzer und wird durch ASP.NET MVC aufgefüllt, wenn der Controller zur Laufzeit erstellt wird. Da wir den dinnerscontroller außerhalb einer Webserver Umgebung testen, ist das User-Objekt nicht festgelegt (daher die NULL-Verweis Ausnahme).

### <a name="mocking-the-useridentityname-property"></a>Simulieren der User.Identity.Name-Eigenschaft

Durch die Durchführung von Frameworks können Sie die Tests vereinfachen, da wir dynamisch gefälschte Versionen abhängiger Objekte erstellen können, die unsere Tests unterstützen. Beispielsweise können wir in unserem Test zum Bearbeiten von Aktionen mit einem Simulations Framework dynamisch ein Benutzerobjekt erstellen, das von unserem dinnerscontroller zum Suchen eines simulierten Benutzernamens verwendet werden kann. Dadurch wird verhindert, dass beim Ausführen des Tests ein NULL-Verweis ausgelöst wird.

Es gibt viele .NET-Frameworks, die mit ASP.NET MVC verwendet werden können (Sie können hier eine Liste sehen: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Zum Testen unserer "nerddinner"-Anwendung verwenden wir ein Open-Source-Tool mit dem Namen "muq", das kostenlos von [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)heruntergeladen werden kann.

Nach dem herunterladen fügen wir einen Verweis in das Projekt "nerddinner. Tests" der Assembly "" von "" "". dll "hinzu:

![](enable-automated-unit-testing/_static/image12.png)

Anschließend fügen wir der Testklasse, die einen Benutzernamen als Parameter annimmt und dann die User.Identity.Name-Eigenschaft für die dinnerscontroller-Instanz "verspottet", die "" "" "" "" "".

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Oben verwenden wir MOQ, um ein Mock-Objekt zu erstellen, das ein controllerContext-Objekt Fakes (was ASP.NET MVC an Controller Klassen übergibt, um Lauf Zeit Objekte wie Benutzer, Anforderung, Antwort und Sitzung verfügbar zu machen). Wir rufen die Methode "setupget" für das Mock auf, um anzugeben, dass die HttpContext.User.Identity.Name-Eigenschaft in controllerContext die Benutzernamen-Zeichenfolge zurückgeben soll, die an die Hilfsmethode übergeben wird.

Wir können eine beliebige Anzahl von controllerContext-Eigenschaften und-Methoden simulieren. Um dies zu veranschaulichen, habe ich auch einen setupget ()-Befehl für die "Request. IsAuthenticated"-Eigenschaft hinzugefügt (was für die Tests unten nicht erforderlich ist – aber damit können Sie veranschaulichen, wie Sie Anforderungs Eigenschaften simulieren können). Wenn wir den Vorgang abgeschlossen haben, wird dem dinnerscontroller, den unsere Hilfsmethode zurückgibt, eine Instanz des controllerContext-Mock zugewiesen.

Wir können nun Komponententests schreiben, die diese Hilfsmethode verwenden, um Bearbeitungs Szenarien mit unterschiedlichen Benutzern zu testen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Und jetzt, wenn wir die Tests ausführen, die Sie bestanden haben:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Testen von updatemodel ()-Szenarios

Wir haben Tests erstellt, die die HTTP-Get-Version der Bearbeitungsaktion abdecken. Nun erstellen wir einige Tests, die die HTTP-Post-Version der Bearbeitungsaktion überprüfen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Das interessante neue Testszenario, das wir mit dieser Aktionsmethode unterstützen, ist die Verwendung der "updatemodel ()"-Hilfsmethode für die Controller-Basisklasse. Wir verwenden diese Hilfsmethode, um Formular Post-Werte an die Dinner Object-Instanz zu binden.

Im folgenden finden Sie zwei Tests, in denen veranschaulicht wird, wie Sie von der zu verwendende Methode "updatemodel ()" bereitgestellte Werte bereitstellen können. Dies wird durch Erstellen und Auffüllen eines FormCollection-Objekts und anschließendes Zuweisen der Eigenschaft "valueprovider" auf dem Controller durchführen.

Der erste Test überprüft, ob bei erfolgreicher Speicherung der Browser an die Detail Aktion umgeleitet wird. Der zweite Test überprüft, ob bei der Bereitstellung einer ungültigen Eingabe die Bearbeitungs Ansicht erneut mit einer Fehlermeldung angezeigt wird.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testen von Wrap-up

Wir haben die Kernkonzepte behandelt, die an Komponententests von Controller Klassen beteiligt sind. Mit diesen Techniken können Sie problemlos Hunderte von einfachen Tests erstellen, mit denen das Verhalten der Anwendung überprüft wird.

Da unsere Controller-und Modell Tests keine echte Datenbank erfordern, sind Sie äußerst schnell und leicht auszuführen. Wir können Hunderte automatisierter Tests in Sekundenschnelle ausführen und sofort Feedback erhalten, ob eine Änderung, die wir vorgenommen haben, etwas abgebrochen hat. Dadurch erhalten wir das Vertrauen, um die Anwendung kontinuierlich zu verbessern, zu umgestalten und zu verfeinern.

Wir haben Tests als letztes Thema in diesem Kapitel behandelt – aber nicht, weil Tests etwas ist, das Sie am Ende eines Entwicklungsprozesses durchführen sollten! Im Gegensatz dazu sollten Sie automatisierte Tests so früh wie möglich in Ihren Entwicklungsprozess schreiben. Dies ermöglicht es Ihnen, bei der Entwicklung sofortiges Feedback zu erhalten, Sie dabei zu unterstützen, sich über die Anwendungsszenarien Ihrer Anwendung Gedanken zu machen, und Sie werden dazu geführt, dass Sie Ihre Anwendung mit sauberer Schicht und Kopplung entwerfen können.

Ein späteres Kapitel im Buch erläutert die Test gesteuerte Entwicklung (Test-gesteuerte Entwicklung) und die Verwendung mit ASP.NET MVC. TDD ist eine iterative Codierungs Übung, bei der Sie zunächst die Tests schreiben, die der resultierende Code erfüllen wird. Mit TDD beginnen Sie mit jedem Feature, indem Sie einen Test erstellen, der die Funktionalität überprüft, die Sie implementieren. Wenn Sie den Komponenten Test zuerst schreiben, können Sie sicherstellen, dass Sie das Feature und die Funktionsweise des Features eindeutig verstehen. Erst nachdem der Test geschrieben wurde (und Sie sich vergewissert haben, dass er fehlschlägt), implementieren Sie die tatsächliche Funktionalität, die der Test überprüft. Da Sie bereits Zeit für die Verwendung des Features aufgewendet haben, haben Sie einen besseren Einblick in die Anforderungen und die bestmögliche Implementierung. Wenn Sie die Implementierung durchgeführt haben, können Sie die Test – erneut ausführen und sofort Feedback erhalten, ob die Funktion ordnungsgemäß funktioniert. Wir behandeln Weitere Informationen in Kapitel 10.

### <a name="next-step"></a>Nächster Schritt

Einige abschließende Zusammenfassung der Kommentare.

> [!div class="step-by-step"]
> [Zurück](use-ajax-to-implement-mapping-scenarios.md)
> [Weiter](nerddinner-wrap-up.md)
