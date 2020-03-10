---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Iterations #2 – machen Sie die Anwendung gut aussehenC#() | Microsoft-Dokumentation'
author: microsoft
description: In dieser Iterationen verbessern wir die Darstellung der Anwendung durch Ändern der standardmäßigen ASP.NET-MVC-Ansichts Master Seite und des Cascading Stylesheets.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 246cb4b4668339cc4b7e4e03ea005102c6a2a5c3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487311"
---
# <a name="iteration-2--make-the-application-look-nice-c"></a>Iterations #2 – machen Sie das Aussehen der AnwendungC#schön ()

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> In dieser Iterationen verbessern wir die Darstellung der Anwendung durch Ändern der standardmäßigen ASP.NET-MVC-Ansichts Master Seite und des Cascading Stylesheets.

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

Das Ziel dieser Iterationen besteht darin, die Darstellung der Kontakt-Manager-Anwendung zu verbessern. Der Kontakt-Manager verwendet derzeit die standardmäßige ASP.NET-MVC-Ansichts-Master Seite und das Cascading-Stylesheet (siehe Abbildung 1). Diese Don 't sieht schlecht aus, aber ich möchte, dass der Kontakt-Manager nicht wie jede andere ASP.NET MVC-Website aussieht. Ich möchte diese Dateien durch benutzerdefinierte Dateien ersetzen.

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Abbildung 01**: die Standarddarstellung einer ASP.NET-MVC-Anwendung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image2.png))

In dieser Iterations Diskussion werden zwei Ansätze erläutert, um den visuellen Entwurf unserer Anwendung zu verbessern. Zuerst zeige ich Ihnen, wie Sie die ASP.NET MVC-Entwurfs Galerie nutzen können, um eine kostenlose ASP.NET MVC-Entwurfs Vorlage herunterzuladen. Der ASP.NET MVC-Entwurfs Katalog ermöglicht es Ihnen, eine professionelle Webanwendung zu erstellen, ohne eine Arbeit zu erledigen.

Ich entschied mich für die Verwendung einer Vorlage aus der ASP.NET MVC-Entwurfs Galerie für die Contact Manager-Anwendung. Stattdessen hatte ich einen benutzerdefinierten Entwurf, der von einem professionellen Entwurfs Unternehmen erstellt wurde. Im zweiten Teil dieses Tutorials erläutere ich, wie ich mit einem professionellen Entwurfs Unternehmen zum Erstellen des endgültigen ASP.NET MVC-Entwurfs gearbeitet habe.

## <a name="the-aspnet-mvc-design-gallery"></a>Der ASP.NET MVC-Entwurfs Katalog

Der ASP.NET MVC-Entwurfs Katalog ist eine von Microsoft bereitgestellte kostenlose Ressource. Der ASP.NET MVC-Katalog befindet sich unter folgender Adresse:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

Der ASP.NET MVC-Entwurfs Katalog hostet eine Sammlung von kostenlosen Website Entwürfen, die speziell für die Verwendung von in einem ASP.NET MVC-Projekt erstellt wurden. Entwürfe werden von Mitgliedern der Community hochgeladen. Besucher des Katalogs können für Ihre bevorzugten Entwürfe abstimmen (siehe Abbildung 2).

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Abbildung 02**: ASP.NET MVC-Entwurfs Galerie ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image4.png))

Während ich dieses Tutorial schreibe, ist der am häufigsten verwendete Entwurf im Katalog der Entwurf von David Hauser. Sie können dieses Design für ein ASP.NET-MVC-Projekt verwenden, indem Sie die folgenden Schritte ausführen:

1. Klicken Sie auf die Schaltfläche **herunterladen** , um die ZIP-Datei von Oktober auf Ihren Computer herunterzuladen.
2. Klicken Sie mit der rechten Maustaste auf die heruntergeladene ZIP-Datei, und klicken Sie auf die Schaltfläche zum **entsperren** (siehe Abbildung 3).
3. Entzippen Sie die Datei in einen Ordner mit dem Namen Oktober.
4. Wählen Sie alle Dateien aus dem Ordner "Designtemplate" aus, die im Oktober-Ordner enthalten sind, klicken Sie mit der rechten Maustaste auf die Dateien, und wählen Sie die Menüoption **Kopieren**.
5. Klicken Sie im Visual Studio-Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Projekt Knoten ContactManager, und wählen Sie die Menüoption **Einfügen** aus (siehe Abbildung 4).
6. Wählen Sie die Visual Studio-Menüoption **Bearbeiten, suchen und ersetzen, schneller** Setzung und ersetzen *[myprojectname]* durch *ContactManager* (siehe Abbildung 5).

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Abbildung 03**: Aufheben der Blockierung einer aus dem Internet heruntergeladenen Datei ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image6.png))

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Abbildung 04**: Überschreiben von Dateien im Projektmappen-Explorer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image8.png))

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Abbildung 05**: Ersetzen von [ProjectName] durch ContactManager ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image10.png))

Nachdem Sie diese Schritte ausgeführt haben, verwendet Ihre Webanwendung den neuen Entwurf. Die Seite in Abbildung 6 veranschaulicht das Aussehen der Contact Manager-Anwendung mit dem Oktober-Design.

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Abbildung 06**: ContactManager mit der Oktober-Vorlage ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Erstellen eines benutzerdefinierten ASP.NET MVC-Entwurfs

Der ASP.NET MVC-Entwurfs Katalog bietet eine gute Auswahl an verschiedenen Entwurfs Stilen. Der-Katalog bietet Ihnen die Möglichkeit, die Darstellung Ihrer ASP.NET-MVC-Anwendungen auf mühelose Weise anzupassen. Natürlich hat der Katalog den großen Vorteil, dass er vollständig kostenlos ist.

Allerdings müssen Sie möglicherweise einen vollständig eindeutigen Entwurf für Ihre Website erstellen. In diesem Fall ist es sinnvoll, mit einem Website Entwurfs Unternehmen zu arbeiten. Ich entschied mich, diesen Ansatz für den Entwurf der Contact Manager-Anwendung zu treffen.

Ich habe den Contact Manager aus der Iterations #1 gezippt und das Projekt an das Entwurfs Unternehmen gesendet. Sie waren nicht in der Besitz von Visual Studio (schade darauf!), aber das stellte kein Problem dar. Sie konnten Microsoft Visual Web Developer kostenlos von der [https://www.asp.net](https://www.asp.net) -Website herunterladen und die Contact Manager-Anwendung in Visual Web Developer öffnen. In einigen Tagen haben Sie den Entwurf in Abbildung 7 erzeugt.

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Abbildung 07**: der ASP.NET MVC Contact Manager-Entwurf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image14.png))

Der neue Entwurf umfasste zwei Haupt Dateien: eine neue Cascading Stylesheet-Datei und eine neue Ansichts-Masterseiten Datei. Eine Ansichts Master Seite enthält das Layout und den freigegebenen Inhalt für Sichten in einer ASP.NET MVC-Anwendung. Beispielsweise enthält die Seite Master anzeigen den Header, die Navigations Registerkarten und die Fußzeile, die in Abbildung 7 angezeigt werden. Ich habe die vorhandene Master Seite "Site. Master View" im Ordner "views\shared" mit der neuen Datei "Site. Master" aus dem Design Unternehmen überschrieben.

Das Design Unternehmen hat auch ein neues Cascading Stylesheet und einen Satz von Bildern erstellt. Diese neuen Dateien wurden im Inhalts Ordner abgelegt und die vorhandene Datei "Site. CSS" überschrieben. Sie sollten alle statischen Inhalte im Inhalts Ordner platzieren.

Beachten Sie, dass der neue Entwurf für den Contact Manager Bilder zum Bearbeiten und Löschen von Kontakten enthält. Ein Bild zum Bearbeiten und löschen wird neben den einzelnen Kontakten in der HTML-Tabelle der Kontakte angezeigt.

Ursprünglich wurden diese Verknüpfungen mit dem HTML-Code gerendert. Action Link ()-Hilfsprogramm wie folgt:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Die HTML. Action Link ()-Methode unterstützt keine Bilder (die Methode HTML codiert den Linktext aus Sicherheitsgründen). Daher habe ich die Aufrufe von HTML. Action Link () durch Aufrufe von URL. Action () wie folgt ersetzt:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Die HTML. Action Link ()-Methode rendert einen gesamten HTML-Hyperlink. Die URL. Action ()-Methode hingegen rendert nur die URL ohne den &lt;ein&gt;-Tag.

Beachten Sie außerdem, dass der neue Entwurf sowohl ausgewählte als auch nicht ausgewählte Registerkarten enthält. In Abbildung 8 wird z. b. die Registerkarte **neuen Kontakt erstellen** ausgewählt, und die Registerkarte **meine Kontakte** ist nicht ausgewählt.

[![des Dialog Felds "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Abbildung 08**: ausgewählte und nicht ausgewählte Registerkarten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-2-make-the-application-look-nice-cs/_static/image16.png))

Um das Rendern sowohl ausgewählter als auch nicht ausgewählter Registerkarten zu unterstützen, habe ich ein benutzerdefiniertes HTML-Hilfsprogramm namens menuitemhelper erstellt Diese Hilfsmethode rendert entweder ein &lt;Li&gt; Tag oder ein &lt;li class = "Selected"&gt; Tag, je nachdem, ob der aktuelle Controller und die Aktion dem Controller und dem Aktions Namen entsprechen, der an das Hilfsprogramm weitergegeben wurde. Der Code für das menuitemhelper-Element ist in der Liste 1 enthalten.

**Codebeispiel 1-helpers\menuitemhelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Der menuitemhelper verwendet intern die tagbuilder-Klasse, um das &lt;Li&gt; HTML-Tag zu erstellen. Die tagbuilder-Klasse ist eine sehr nützliche Hilfsprogrammklasse, die Sie verwenden können, wenn Sie ein neues HTML-Tag erstellen müssen. Es enthält Methoden zum Hinzufügen von Attributen, zum Hinzufügen von CSS-Klassen, zum Erstellen von IDs und zum Ändern des inneren HTML-Tags der

## <a name="summary"></a>Zusammenfassung

In dieser Iterationen haben wir den visuellen Entwurf unserer ASP.NET MVC-Anwendung verbessert. Zuerst wurden Sie mit dem ASP.NET MVC-Entwurfs Katalog eingeführt. Sie haben gelernt, wie Sie kostenlose Entwurfsvorlagen aus der ASP.NET MVC-Entwurfs Galerie herunterladen, die Sie in Ihren ASP.NET MVC-Anwendungen verwenden können.

Als nächstes haben wir erläutert, wie Sie einen benutzerdefinierten Entwurf erstellen können, indem Sie die standardmäßige Cascading Stylesheet-Datei und die Datei für die Master Ansicht ändern. Um den neuen Entwurf zu unterstützen, mussten wir einige geringfügige Änderungen an unserer Contact Manager-Anwendung vornehmen. Wir haben z. b. ein neues HTML-Hilfsprogramm namens "menuitemhelper" hinzugefügt, das ausgewählte und nicht ausgewählte Registerkarten anzeigt.

In der nächsten Iterationen befassen wir uns mit dem sehr wichtigen Betreff der Validierung. Wir fügen der Anwendung Validierungscode hinzu, damit ein Benutzer keinen neuen Kontakt erstellen kann, ohne die erforderlichen Werte anzugeben, z. b. die vor-und Nachnamen einer Person.

> [!div class="step-by-step"]
> [Zurück](iteration-1-create-the-application-cs.md)
> [Weiter](iteration-3-add-form-validation-cs.md)
