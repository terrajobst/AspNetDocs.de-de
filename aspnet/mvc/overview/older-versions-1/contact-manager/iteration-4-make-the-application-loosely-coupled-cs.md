---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iterations #4 – Festlegen der Anwendung lose gekoppelt (C#) | Microsoft-Dokumentation'
author: microsoft
description: In dieser vierten Iterationen nutzen wir mehrere Software Entwurfsmuster, um die Verwaltung und Änderung der Contact Manager-Anwendung zu vereinfachen. Für ...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: ce8e3c4ff8a59be9f2f572813db599604216119d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437973"
---
# <a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iterations #4 – Festlegen der Anwendung lose gekoppelt (C#)

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> In dieser vierten Iterationen nutzen wir mehrere Software Entwurfsmuster, um die Verwaltung und Änderung der Contact Manager-Anwendung zu vereinfachen. Beispielsweise können wir unsere Anwendung so umgestalten, dass Sie das Repository-Muster und das Muster für die Abhängigkeitsinjektion verwendet.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Aufbauen einer Kontakt Verwaltung ASP.NET MVC-AnwendungC#()

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

In dieser vierten Iterations Anwendung der Kontakt-Manager-Anwendung wird die Anwendung umgestalten, um die Anwendung leichter gekoppelt zu gestalten. Wenn eine Anwendung lose gekoppelt ist, können Sie den Code in einem Teil der Anwendung ändern, ohne den Code in anderen Teilen der Anwendung ändern zu müssen. Lose gekoppelte Anwendungen sind stabiler zu ändern.

Derzeit sind die Datenzugriffs-und Validierungs Logik, die von der Contact Manager-Anwendung verwendet wird, in den Controller Klassen enthalten. Das ist eine gute Idee. Wenn Sie einen Teil Ihrer Anwendung ändern müssen, riskieren Sie, dass Sie Fehler in einen anderen Teil der Anwendung einführen. Wenn Sie z. b. die Validierungs Logik ändern, riskieren Sie, dass neue Fehler in den Datenzugriff oder die Controller Logik eingeführt werden.

> [!NOTE] 
> 
> (SRP), eine Klasse sollte nie mehr als einen Grund für die Änderung aufweisen. Das Kombinieren von Controller, Validierung und Daten Bank Logik stellt einen massiven Verstoß gegen das Prinzip der einzelnen Verantwortung dar.

Es gibt mehrere Gründe, die Sie möglicherweise benötigen, um Ihre Anwendung zu ändern. Möglicherweise müssen Sie der Anwendung ein neues Feature hinzufügen. möglicherweise müssen Sie einen Fehler in Ihrer Anwendung beheben, oder Sie müssen ggf. ändern, wie ein Feature der Anwendung implementiert wird. Anwendungen sind selten statisch. Sie werden tendenziell vergrößert und im Laufe der Zeit mutiert.

Stellen Sie sich beispielsweise vor, dass Sie sich entscheiden, wie Sie Ihre Datenzugriffs Ebene implementieren. Derzeit verwendet die Contact Manager-Anwendung den Microsoft-Entity Framework, um auf die Datenbank zuzugreifen. Sie können jedoch eine Migration zu einer neuen oder alternativen Datenzugriffs Technologie durchsetzen, z. b. ADO.NET Data Services oder NHibernate. Da der Datenzugriffs Code jedoch nicht vom Validierungs-und Controller Code isoliert ist, gibt es keine Möglichkeit, den Datenzugriffs Code in der Anwendung zu ändern, ohne anderen Code zu ändern, der nicht direkt mit dem Datenzugriff verknüpft ist.

Wenn eine Anwendung lose gekoppelt ist, können Sie auf der anderen Seite Änderungen an einem Teil der Anwendung vornehmen, ohne andere Teile einer Anwendung zu berühren. Beispielsweise können Sie Datenzugriffs Technologien wechseln, ohne die Validierungs-oder Controller Logik zu ändern.

In dieser Iterationen nutzen wir mehrere Software Entwurfsmuster, mit denen wir unsere Kontakt-Manager-Anwendung in eine lose gekoppelte Anwendung umgestalten können. Wenn Sie den Vorgang abgeschlossen haben, hat der Kontakt-Manager keine weiteren Aktionen ausgeführt, die er zuvor nicht ausgeführt hat. Die Anwendung kann jedoch in Zukunft einfacher geändert werden.

> [!NOTE] 
> 
> Refactoring ist der Prozess, bei dem eine Anwendung so umgeschrieben wird, dass Sie keinerlei vorhandene Funktionalität verliert.

## <a name="using-the-repository-software-design-pattern"></a>Verwenden des Repository-Software Entwurfs Musters

Unsere erste Änderung besteht darin, ein Software Entwurfsmuster zu nutzen, das als Repository-Muster bezeichnet wird. Wir verwenden das Repository-Muster, um den Datenzugriffs Code von der restlichen Anwendung zu isolieren.

Zum Implementieren des Repository-Musters müssen wir die folgenden beiden Schritte ausführen:

1. Erstellen einer Schnittstelle
2. Erstellen einer konkreten Klasse, die die-Schnittstelle implementiert

Zuerst müssen wir eine Schnittstelle erstellen, die alle Datenzugriffs Methoden beschreibt, die wir ausführen müssen. Die icontactmanagerrepository-Schnittstelle ist in der Liste 1 enthalten. Diese Schnittstelle beschreibt fünf Methoden: "kreatecontact ()", "DeleteContact ()", "EditContact ()", "GetContact" und "listcontacts ()".

**Codebeispiel 1: models\icontactmanagerrepositoriy.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Als nächstes müssen wir eine konkrete Klasse erstellen, die die icontactmanagerrepository-Schnittstelle implementiert. Da wir den Microsoft-Entity Framework für den Zugriff auf die Datenbank verwenden, erstellen wir eine neue Klasse mit dem Namen entitycontactmanagerrepository. Diese Klasse ist in der Liste 2 enthalten.

**Auflisten 2: models\entitycontactmanagerrepositor y.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Beachten Sie, dass die entitycontactmanagerrepository-Klasse die icontactmanagerrepository-Schnittstelle implementiert. Die-Klasse implementiert alle fünf Methoden, die von dieser Schnittstelle beschrieben werden.

Vielleicht Fragen Sie sich, warum wir uns mit einer Schnittstelle beschäftigen müssen. Warum müssen wir sowohl eine Schnittstelle als auch eine Klasse erstellen, die Sie implementiert?

Mit einer Ausnahme interagiert der Rest unserer Anwendung mit der-Schnittstelle und nicht mit der konkreten-Klasse. Anstatt die Methoden aufzurufen, die von der entitycontactmanagerrepository-Klasse verfügbar gemacht werden, werden die Methoden aufgerufen, die von der icontactmanagerrepository-Schnittstelle verfügbar gemacht werden.

Auf diese Weise können wir die Schnittstelle mit einer neuen Klasse implementieren, ohne den Rest der Anwendung ändern zu müssen. Beispielsweise möchten wir zu einem späteren Zeitpunkt möglicherweise eine dataservicescontactmanagerrepository-Klasse implementieren, die die icontactmanagerrepository-Schnittstelle implementiert. Die dataservicescontactmanagerrepository-Klasse kann ADO.NET-Data Services verwenden, um auf eine Datenbank anstatt auf die Microsoft-Entity Framework zuzugreifen.

Wenn der Anwendungscode für die icontactmanagerrepository-Schnittstelle und nicht für die konkrete entitycontactmanagerrepository-Klasse programmiert wird, können wir konkrete Klassen wechseln, ohne den restlichen Code zu ändern. Beispielsweise können wir von der entitycontactmanagerrepository-Klasse zur dataservicescontactmanagerrepository-Klasse wechseln, ohne den Datenzugriff oder die Validierungs Logik zu ändern.

Die Programmierung für Schnittstellen (Abstraktionen) anstelle von konkreten klassenmacht unsere Anwendung stabiler, um sich zu ändern.

> [!NOTE] 
> 
> Sie können schnell eine Schnittstelle aus einer konkreten Klasse in Visual Studio erstellen, indem Sie die Menüoption umgestalten, Schnittstelle extrahieren auswählen. Beispielsweise können Sie zuerst die entitycontactmanagerrepository-Klasse erstellen und dann mit Extract Interface die icontactmanagerrepository-Schnittstelle automatisch generieren.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Verwenden des Software Entwurfs Musters für die Abhängigkeitsinjektion

Nachdem wir den Datenzugriffs Code zu einer separaten Repository-Klasse migriert haben, müssen wir den Kontakt Controller ändern, um diese Klasse zu verwenden. Wir nutzen ein Software Entwurfsmuster namens "Abhängigkeitsinjektion", um die Repository-Klasse in unserem Controller zu verwenden.

Der geänderte Kontakt Controller ist in der Liste 3 enthalten.

**Codebeispiel 3-controllers\contactcontroller.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Beachten Sie, dass der Contact Controller in der Liste 3 über zwei Konstruktoren verfügt. Der erste Konstruktor übergibt eine konkrete Instanz der icontactmanagerrepository-Schnittstelle an den zweiten Konstruktor. Die Contact Controller-Klasse verwendet die *konstruktorabhängigkeits Injektion*.

Die einzige Stelle, an der die entitycontactmanagerrepository-Klasse verwendet wird, ist der erste Konstruktor. Der Rest der Klasse verwendet die icontactmanagerrepository-Schnittstelle anstelle der konkreten entitycontactmanagerrepository-Klasse.

Dadurch können Implementierungen der icontactmanagerrepository-Klasse in Zukunft einfach gewechselt werden. Wenn Sie anstelle der entitycontactmanagerrepository-Klasse die dataservicescontactrepository-Klasse verwenden möchten, ändern Sie einfach den ersten Konstruktor.

Mit der konstruktorabhängigkeits Injektion wird die Contact Controller-Klasse ebenfalls sehr testfähig. In den Komponententests können Sie den Kontakt Controller instanziieren, indem Sie eine Mock-Implementierung der icontactmanagerrepository-Klasse übergeben. Diese Funktion der Abhängigkeitsinjektion ist für uns in der nächsten Iterationen sehr wichtig, wenn Komponententests für die Contact Manager-Anwendung erstellt werden.

> [!NOTE] 
> 
> Wenn Sie die Contact Controller-Klasse von einer bestimmten Implementierung der icontactmanagerrepository-Schnittstelle vollständig entkoppeln möchten, können Sie ein Framework nutzen, das die Abhängigkeitsinjektion unterstützt, wie z. b. StructureMap oder Microsoft Entity Framework (MEF). Wenn Sie ein Framework für die Abhängigkeitsinjektion nutzen, müssen Sie in Ihrem Code nie auf eine konkrete Klasse verweisen.

## <a name="creating-a-service-layer"></a>Erstellen einer Dienst Ebene

Sie haben vielleicht bemerkt, dass unsere Validierungs Logik weiterhin mit unserer Controller Logik in der geänderten Controller Klasse in der Liste 3 vermischt wird. Aus demselben Grund ist es eine gute Idee, unsere Datenzugriffs Logik zu isolieren. es ist eine gute Idee, unsere Validierungs Logik zu isolieren.

Um dieses Problem zu beheben, können wir eine separate [*Dienst Schicht*](http://martinfowler.com/eaaCatalog/serviceLayer.html)erstellen. Die Dienst Ebene ist eine separate Ebene, die zwischen den Controller-und Repository-Klassen eingefügt werden kann. Die Dienst Ebene enthält unsere Geschäftslogik, einschließlich der gesamten Validierungs Logik.

Contactmanagerservice ist in der Liste 4 enthalten. Sie enthält die Validierungs Logik der Contact Controller-Klasse.

**Codebeispiel 4: models\contactmanagerservice.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Beachten Sie, dass der Konstruktor für den contactmanagerservice ein validationdictionary erfordert. Die Dienst Ebene kommuniziert über dieses validationdictionary mit der Controller Schicht. Im folgenden Abschnitt wird das validationdictionary ausführlich erläutert, wenn wir das Decorator-Muster erörtern.

Beachten Sie, dass der contactmanagerservice die Schnittstelle icontactmanagerservice implementiert. Sie sollten sich immer darauf konzentrieren, anstelle von konkreten Klassen für Schnittstellen zu programmieren. Andere Klassen in der Contact Manager-Anwendung interagieren nicht direkt mit der contactmanagerservice-Klasse. Stattdessen wird der Rest der Kontakt-Manager-Anwendung mit der icontactmanagerservice-Schnittstelle programmiert, mit einer Ausnahme.

Die icontactmanagerservice-Schnittstelle ist in der Liste 5 enthalten.

**Auflisten 5: models\icontactmanagerservice.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Die geänderte Contact Controller-Klasse ist in der Liste 6 enthalten. Beachten Sie, dass der Contact Controller nicht mehr mit dem ContactManager-Repository interagiert. Stattdessen interagiert der Kontakt Controller mit dem ContactManager-Dienst. Jede Ebene wird so weit wie möglich von anderen Ebenen isoliert.

**Auflisten 6-controllers\contactcontroller.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Unsere Anwendung wird nicht mehr mit dem Prinzip der einzelnen Zuständigkeit (SRP) ausgeführt. Der Kontakt-Controller in der Liste 6 wurde für alle Verantwortlichkeiten außer der Steuerung des Anwendungs Ausführungs Flusses entfernt. Die gesamte Validierungs Logik wurde aus dem Contact Controller entfernt und in die Dienst Schicht übermittelt. Die gesamte Daten Bank Logik wurde in die Repository-Schicht übermittelt.

## <a name="using-the-decorator-pattern"></a>Verwenden des Decorator-Musters

Wir möchten unsere Service Schicht vollständig von unserer Controller Schicht entkoppeln. Im Prinzip sollten wir unsere Dienst Schicht in einer separaten Assembly von unserer Controller Schicht kompilieren können, ohne einen Verweis auf unsere MVC-Anwendung hinzufügen zu müssen.

Unsere Dienst Schicht muss jedoch in der Lage sein, Validierungs Fehlermeldungen zurück an die Controller Schicht zu übergeben. Wie können wir es der Dienst Ebene ermöglichen, Validierungs Fehlermeldungen zu übermitteln, ohne den Controller und die Dienst Schicht zu koppeln? Wir können ein Software Entwurfsmuster mit dem Namen [Decorator-Muster](http://en.wikipedia.org/wiki/Decorator_pattern)nutzen.

Ein Controller verwendet einen modelstatedictionary namens modelstate, um Validierungs Fehler darzustellen. Daher ist es möglicherweise verlockend, modelstate von der Controller Schicht an die Dienst Schicht zu übergeben. Die Verwendung von modelstate in der Dienst Ebene würde jedoch dazu führen, dass die Dienst Ebene von einer Funktion des ASP.NET-MVC-Frameworks abhängig ist. Dies wäre schlecht, weil Sie einen Tag lang die Dienst Ebene mit einer WPF-Anwendung anstelle einer ASP.NET MVC-Anwendung verwenden möchten. In diesem Fall möchten Sie nicht auf das ASP.NET MVC-Framework verweisen, um die modelstatuedictionary-Klasse zu verwenden.

Das Decorator-Muster ermöglicht es Ihnen, eine vorhandene Klasse in einer neuen Klasse zu umschließen, um eine Schnittstelle zu implementieren. Unser Contact Manager-Projekt enthält die modelstatewrapper-Klasse, die in der Liste 7 enthalten ist. Die modelstatewrapper-Klasse implementiert die-Schnittstelle in der Auflistung 8.

**Codebeispiel 7: models\validation\modelstatewrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Auflisten von 8-models\validation\ivalidationditionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Wenn Sie sich die Liste 5 genauer ansehen, sehen Sie, dass die Dienst Ebene "ContactManager" die ivalidationdictionary-Schnittstelle exklusiv verwendet. Der ContactManager-Dienst ist nicht von der modelstatuedictionary-Klasse abhängig. Wenn der Contact Controller den ContactManager-Dienst erstellt, bindet der Controller seinen modelstate wie folgt ein:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Zusammenfassung

In dieser Iterations Funktion haben wir der Contact Manager-Anwendung keine neuen Funktionen hinzugefügt. Das Ziel dieser Iterationen bestand darin, die Kontakt-Manager-Anwendung so zu gestalten, dass Sie einfacher zu verwalten und zu ändern ist.

Zuerst haben wir das Repository-Software Entwurfsmuster implementiert. Wir haben den gesamten Datenzugriffs Code zu einer separaten ContactManager-Repository-Klasse migriert.

Wir haben auch unsere Validierungs Logik von unserer Controller Logik isoliert. Wir haben eine separate Dienst Ebene erstellt, die den gesamten Validierungscode enthält. Die Controller Schicht interagiert mit der Dienst Ebene, und die Dienst Schicht interagiert mit der Repository-Ebene.

Als wir die Dienst Ebene erstellt haben, nutzten wir das Decorator-Muster, um modelstate von unserer Dienst Schicht zu isolieren. In unserer Dienst Schicht haben wir anstelle von modelstate die ivalidationdictionary-Schnittstelle programmiert.

Schließlich haben wir ein Software Entwurfsmuster mit dem Namen "Abhängigkeitsinjektion" genutzt. Mit diesem Muster können wir anstelle von konkreten Klassen für Schnittstellen (Abstraktionen) programmieren. Das Implementieren des Entwurfs Musters für die Abhängigkeitsinjektion sorgt auch für einen testbaren Code. In der nächsten Iterationen fügen wir dem Projektkomponenten Tests hinzu.

> [!div class="step-by-step"]
> [Zurück](iteration-3-add-form-validation-cs.md)
> [Weiter](iteration-5-create-unit-tests-cs.md)
