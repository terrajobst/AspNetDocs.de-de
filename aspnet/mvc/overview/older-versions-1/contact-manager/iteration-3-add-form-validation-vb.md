---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iterations #3 – Formular Validierung hinzufügen (VB) | Microsoft-Dokumentation'
author: microsoft
description: In der dritten Iterationen fügen wir die grundlegende Formular Validierung hinzu. Wir hindern Personen daran, ein Formular zu senden, ohne die erforderlichen Formularfelder abzuschließen. Wir überprüfen auch EMAI...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 73b307f53875abe84b592c75b1ff614ffd9d8b82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437967"
---
# <a name="iteration-3--add-form-validation-vb"></a>Iterations #3 – Formular Validierung hinzufügen (VB)

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> In der dritten Iterationen fügen wir die grundlegende Formular Validierung hinzu. Wir hindern Personen daran, ein Formular zu senden, ohne die erforderlichen Formularfelder abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

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

In dieser zweiten Iterations Anwendung der Kontakt-Manager-Anwendung fügen wir die grundlegende Formular Validierung hinzu. Wir hindern Personen daran, einen Kontakt zu senden, ohne die Werte für die erforderlichen Formularfelder einzugeben. Wir überprüfen auch Telefonnummern und e-Mail-Adressen (siehe Abbildung 1).

[![des Dialog Felds "Neues Projekt"](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Abbildung 01**: ein Formular mit Validierung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-3-add-form-validation-vb/_static/image2.png))

In dieser Iterationen fügen wir die Validierungs Logik den Controller Aktionen direkt hinzu. Im Allgemeinen ist dies nicht die empfohlene Vorgehensweise zum Hinzufügen von Validierungen zu einer ASP.NET MVC-Anwendung. Ein besserer Ansatz besteht darin, die Validierungs Logik einer Anwendung in einer separaten [Dienst Schicht](http://martinfowler.com/eaaCatalog/serviceLayer.html)zu platzieren. In der nächsten Iterationen wird die Kontakt-Manager-Anwendung umgestalten, um die Wartung der Anwendung zu vereinfachen.

In dieser Iterationen wird der gesamte Validierungscode von Hand geschrieben, um die Dinge einfach zu halten. Anstatt den Validierungscode selbst zu schreiben, könnten wir ein Validierungs Framework nutzen. Beispielsweise können Sie den Microsoft Enterprise Library Validation Application Block (VAB) verwenden, um die Validierungs Logik für Ihre ASP.NET MVC-Anwendung zu implementieren. Weitere Informationen zum Validierungs Anwendungs Block finden Sie unter:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Hinzufügen der Validierung zur CREATE VIEW

Beginnen Sie, indem Sie der CREATE VIEW eine Validierungs Logik hinzufügen. Da wir nun die CREATE VIEW mit Visual Studio generiert haben, enthält die CREATE VIEW bereits die erforderliche Benutzeroberflächen Logik, um Validierungs Meldungen anzuzeigen. Die CREATE-Sicht ist in der Liste 1 enthalten.

**Codebeispiel 1: \views\contact\kreate.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Die Field-Validation-Error-Klasse wird verwendet, um die Ausgabe zu formatieren, die vom HTML. validationmessage ()-Hilfsprogramm gerendert wird Die Input-Validation-Error-Klasse wird verwendet, um das Textfeld (Eingabe) zu formatieren, das vom HTML. Textfeld ()-Hilfsprogramm gerendert wird. Die Validation-Summary-Errors-Klasse wird verwendet, um die ungeordnete Liste zu formatieren, die vom HTML. ValidationSummary ()-Hilfsprogramm gerendert wird.

> [!NOTE] 
> 
> Sie können die in diesem Abschnitt beschriebenen Stylesheet-Klassen ändern, um die Darstellung von Validierungs Fehlermeldungen anzupassen.

## <a name="adding-validation-logic-to-the-create-action"></a>Hinzufügen von Validierungs Logik zur CREATE-Aktion

Zurzeit zeigt die CREATE-Sicht keine Validierungs Fehlermeldungen an, da wir die Logik zum Generieren von Nachrichten nicht geschrieben haben. Um Validierungs Fehlermeldungen anzuzeigen, müssen Sie die Fehlermeldungen modelstate hinzufügen.

> [!NOTE] 
> 
> Die updatemodel ()-Methode fügt automatisch Fehlermeldungen zu modelstate hinzu, wenn ein Fehler beim Zuweisen des Werts eines Formular Felds zu einer Eigenschaft vorliegt. Wenn Sie z. b. versuchen, die Zeichenfolge "Apple" einer BirthDate-Eigenschaft zuzuweisen, die DateTime-Werte akzeptiert, fügt die updatemodel ()-Methode "modelstate" einen Fehler hinzu.

Die geänderte Create ()-Methode in der Liste 2 enthält einen neuen Abschnitt, in dem die Eigenschaften der Contact-Klasse überprüft werden, bevor der neue Kontakt in die Datenbank eingefügt wird.

**Codebeispiel 2: controllers\contactcontroller.vb (Create with Validation)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

Der Abschnitt Validate erzwingt vier verschiedene Validierungsregeln:

- Die FirstName-Eigenschaft muss eine Länge größer als 0 (null) aufweisen (und darf nicht ausschließlich aus Leerzeichen bestehen).
- Die LastName-Eigenschaft muss eine Länge größer als 0 (null) aufweisen (und darf nicht ausschließlich aus Leerzeichen bestehen).
- Wenn die Phone-Eigenschaft einen Wert hat (hat eine Länge größer als 0), muss die Phone-Eigenschaft einem regulären Ausdruck entsprechen.
- Wenn die e-Mail-Eigenschaft einen Wert hat (hat eine Länge größer als 0), muss die e-Mail-Eigenschaft einem regulären Ausdruck entsprechen.

Bei einem Verstoß gegen die Validierungs Regel wird modelstate mithilfe der addmodelerror ()-Methode eine Fehlermeldung hinzugefügt. Wenn Sie modelstate eine Nachricht hinzufügen, geben Sie den Namen einer Eigenschaft und den Text einer Validierungs Fehlermeldung an. Diese Fehlermeldung wird in der Ansicht von den Hilfsmethoden HTML. ValidationSummary () und HTML. validationmessage () angezeigt.

Nachdem die Validierungsregeln ausgeführt wurden, wird die IsValid-Eigenschaft von modelstate überprüft. Die IsValid-Eigenschaft gibt false zurück, wenn dem modelstate-Objekt Validierungs Fehlermeldungen hinzugefügt wurden. Wenn die Validierung fehlschlägt, wird das Formular erstellen mit den Fehlermeldungen erneut angezeigt.

> [!NOTE] 
> 
> Ich habe die regulären Ausdrücke zum Überprüfen der Telefonnummer und e-Mail-Adresse aus dem Repository für reguläre Ausdrücke unter [ *http://regexlib.com* ](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Hinzufügen von Validierungs Logik zur Bearbeitungsaktion

Die Aktion bearbeiten () aktualisiert einen Kontakt. Die Edit ()-Aktion muss genau dieselbe Validierung wie die Create ()-Aktion ausführen. Anstatt denselben Validierungscode zu duplizieren, sollten wir den Kontakt Controller so umgestalten, dass sowohl die Create ()-als auch die Edit ()-Aktion dieselbe Validierungsmethode aufruft.

Die geänderte Contact Controller-Klasse ist in der Liste 3 enthalten. Diese Klasse verfügt über eine neue validatecontact ()-Methode, die in den Aktionen Create () und Edit () aufgerufen wird.

**Codebeispiel 3-controllers\contactcontroller.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Zusammenfassung

In dieser Iterationen haben wir der Contact Manager-Anwendung eine grundlegende Formular Validierung hinzugefügt. Mit unserer Validierungs Logik wird verhindert, dass Benutzer einen neuen Kontakt einreichen oder einen vorhandenen Kontakt bearbeiten, ohne die Werte für die Eigenschaften FirstName und LastName anzugeben. Außerdem müssen Benutzer gültige Telefonnummern und e-Mail-Adressen angeben.

In dieser Iterationen haben wir die Validierungs Logik unserer Contact Manager-Anwendung so einfach wie möglich hinzugefügt. Das Mischen unserer Validierungs Logik in unsere Controller Logik führt jedoch langfristig zu Problemen. Unsere Anwendung wird im Laufe der Zeit schwieriger zu warten und zu ändern.

In der nächsten Iterationen werden unsere Validierungs Logik und Datenbankzugriffs Logik von unseren Controllern umgestalten. Wir nutzen mehrere Software Entwurfs Prinzipien, damit wir eine lose gekoppelte und besser verwaltbare Anwendung erstellen können.

> [!div class="step-by-step"]
> [Zurück](iteration-2-make-the-application-look-nice-vb.md)
> [Weiter](iteration-4-make-the-application-loosely-coupled-vb.md)
