---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Teil 9: Registrierung und Checkout | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 9 umfasst die Registrierung und das Auschecken.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450897"
---
# <a name="part-9-registration-and-checkout"></a>Teil 9: Registrierung und Checkout

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 9 umfasst die Registrierung und das Auschecken.

In diesem Abschnitt erstellen wir einen checkoutcontroller, der die Adresse des Kunden und die Zahlungsinformationen sammelt. Wir verlangen, dass sich Benutzer vor dem Auschecken bei unserer Website registrieren, sodass für diesen Controller eine Autorisierung erforderlich ist.

Benutzer navigieren in Ihrem Warenkorb zum Checkout-Prozess, indem Sie auf die Schaltfläche "Auschecken" klicken.

![](mvc-music-store-part-9/_static/image1.jpg)

Wenn der Benutzer nicht angemeldet ist, wird er dazu aufgefordert.

![](mvc-music-store-part-9/_static/image1.png)

Nach erfolgreicher Anmeldung wird dem Benutzer die Ansicht Adresse und Zahlung angezeigt.

![](mvc-music-store-part-9/_static/image2.png)

Nachdem Sie das Formular ausgefüllt und die Bestellung übermittelt haben, wird Ihnen der Bestätigungsbildschirm angezeigt.

![](mvc-music-store-part-9/_static/image3.png)

Wenn Sie versuchen, eine nicht vorhandene Bestellung oder eine Bestellung, die nicht zu gehört, anzuzeigen, wird die Fehler Ansicht angezeigt.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrieren des Warenkorbs

Der Einkaufsprozess ist anonym, wenn der Benutzer auf die Schaltfläche "Auschecken" klickt, muss er sich registrieren und anmelden. Benutzer erwarten, dass die Warenkorb-Informationen zwischen den besuchen aufbewahrt werden. Daher müssen wir die Warenkorb-Informationen einem Benutzer zuordnen, wenn Sie die Registrierung oder Anmeldung durchführen.

Dies ist sehr einfach, da die ShoppingCart-Klasse bereits über eine-Methode verfügt, die alle Elemente im aktuellen Warenkorb einem Benutzernamen zuordnet. Diese Methode muss nur aufgerufen werden, wenn ein Benutzer die Registrierung oder Anmeldung abschließt.

Öffnen Sie die **AccountController** -Klasse, die wir beim Einrichten der Mitgliedschaft und Autorisierung hinzugefügt haben. Fügen Sie eine using-Anweisung hinzu, die auf mvcmusicstore. Models verweist, und fügen Sie die folgende MigrateShoppingCart-Methode hinzu:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Ändern Sie als nächstes die Aktion Logon Post so, dass MigrateShoppingCart aufgerufen wird, nachdem der Benutzer überprüft wurde, wie unten dargestellt:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Nehmen Sie die gleiche Änderung an der Registrierungs Beitrags Aktion vor, unmittelbar nachdem das Benutzerkonto erfolgreich erstellt wurde:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Das ist nun der Fall: ein anonymer Warenkorb wird nach erfolgreicher Registrierung oder Anmeldung automatisch an ein Benutzerkonto übertragen.

## <a name="creating-the-checkoutcontroller"></a>Erstellen von checkoutcontroller

Klicken Sie mit der rechten Maustaste auf den Ordner Controllers, und fügen Sie dem Projekt mit dem Namen checkoutcontroller einen neuen Controller mit der leeren Controller Vorlage hinzu.

![](mvc-music-store-part-9/_static/image5.png)

Fügen Sie zunächst das Autorisierungs Attribut oberhalb der Controller Klassen Deklaration hinzu, damit Benutzer vor dem Auschecken registriert werden:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Hinweis: Dies ähnelt der Änderung, die wir zuvor an storemanagercontroller vorgenommen haben. in diesem Fall erforderte das Autorisierungs Attribut jedoch, dass der Benutzer in einer Administrator Rolle angemeldet ist. Im Checkout-Controller ist es erforderlich, dass der Benutzer angemeldet ist, aber nicht als Administrator angemeldet ist.*

Der Einfachheit halber beschäftigen wir uns in diesem Tutorial nicht mit Zahlungsinformationen. Stattdessen erlauben wir Benutzern das Auschecken mithilfe eines Aktionscodes. Wir speichern diesen Aktions Code mit einer Konstanten namens "Promocode".

Wie im StoreController deklarieren wir ein Feld für eine Instanz der Klasse "musicstoreentities" mit dem Namen "storedb". Damit die Klasse "musicstoreentities" verwendet werden kann, müssen wir eine using-Anweisung für den Namespace "mvcmusicstore. Models" hinzufügen. Der obere Rand des Checkout-Controllers wird unten angezeigt.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Der checkoutcontroller verfügt über die folgenden Controller Aktionen:

**Addressandpayment (Get-Methode)** zeigt ein Formular an, das es dem Benutzer ermöglicht, seine Informationen einzugeben.

**Addressandpayment (Post-Methode)** überprüft die Eingabe und verarbeitet die Bestellung.

Der **Abschluss** wird angezeigt, nachdem ein Benutzer den Auscheck Vorgang erfolgreich abgeschlossen hat. In dieser Ansicht wird die Bestellnummer des Benutzers als Bestätigung angezeigt.

Benennen wir zuerst die Index Controller Aktion (die bei der Erstellung des Controllers generiert wurde) in addressandpayment um. Diese Controller Aktion zeigt nur das Checkout-Formular an, sodass keine Modellinformationen erforderlich sind.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Unsere addressandpayment-Post-Methode folgt demselben Muster, das wir im storemanagercontroller verwendet haben: Es wird versucht, die Übermittlung des Formulars zu akzeptieren und die Bestellung abzuschließen, und das Formular wird erneut angezeigt, wenn es fehlschlägt.

Nachdem die Überprüfung der Formulareingabe unsere Überprüfungsanforderungen für eine Bestellung erfüllt hat, wird der Promocode-Formular Wert direkt überprüft. Wenn alles richtig ist, speichern wir die aktualisierten Informationen in der Reihenfolge, geben das ShoppingCart-Objekt an, um den Bestellvorgang abzuschließen, und leiten zur vollständigen Aktion um.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Nach erfolgreichem Abschluss des Auscheck Vorgangs werden die Benutzer zur vollständigen Controller Aktion umgeleitet. Durch diese Aktion wird eine einfache Überprüfung ausgeführt, um zu überprüfen, ob die Bestellung tatsächlich dem angemeldeten Benutzer angehört, bevor die Bestellnummer als Bestätigung angezeigt wird.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Hinweis: die Fehler Ansicht wurde automatisch für uns im Ordner/views/Shared erstellt, als wir das Projekt gestartet haben.*

Der gesamte checkoutcontroller-Code lautet wie folgt:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Hinzufügen der addressandpayment-Ansicht

Nun erstellen wir die Ansicht "addressandpayment". Klicken Sie mit der rechten Maustaste auf eine der addressandpayment-Controller Aktionen, und fügen Sie eine Ansicht mit dem Namen addressandpayment hinzu, die als Bestellung stark typisiert ist und die Bearbeitungs Vorlage verwendet, wie unten gezeigt.

![](mvc-music-store-part-9/_static/image6.png)

In dieser Ansicht werden zwei der Techniken verwendet, die wir beim Erstellen der storemanageredit-Sicht betrachtet haben:

- Wir verwenden HTML. editorformodel (), um Formularfelder für das Bestell Modell anzuzeigen.
- Wir nutzen Validierungsregeln mithilfe einer Order-Klasse mit Validierungs Attributen.

Wir beginnen mit dem Aktualisieren des Formular Codes, um "HTML. Editor formodel ()" zu verwenden, gefolgt von einem zusätzlichen Textfeld für den Promo-Code. Der gesamte Code für die Ansicht addressandpayment ist unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definieren von Validierungsregeln für die Reihenfolge

Nachdem unsere Ansicht eingerichtet ist, richten wir die Validierungsregeln für das Bestell Modell wie zuvor für das Album Modell ein. Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine Klasse namens Order hinzu. Zusätzlich zu den Validierungs Attributen, die wir zuvor für das Album verwendet haben, wird auch ein regulärer Ausdruck verwendet, um die e-Mail-Adresse des Benutzers zu überprüfen.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Wenn Sie versuchen, das Formular mit fehlenden oder ungültigen Informationen zu senden, wird jetzt eine Fehlermeldung mit der Client seitigen Validierung angezeigt.

![](mvc-music-store-part-9/_static/image7.png)

Okay, wir haben den größten Teil der Arbeit für den Auscheck Prozess abgeschlossen. Wir haben nur ein paar Chancen und enden an der Fertigstellung. Wir müssen zwei einfache Ansichten hinzufügen, und wir müssen die Übergabe der Warenkorb-Informationen während des Anmeldevorgangs durchführen.

## <a name="adding-the-checkout-complete-view"></a>Hinzufügen der Ansicht "Checkout vervollständigen"

Die Ansicht "Auschecken vervollständigen" ist recht einfach, da Sie lediglich die Bestell-ID anzeigen muss. Klicken Sie mit der rechten Maustaste auf die Aktion Controller vervollständigen, und fügen Sie eine Ansicht mit dem Namen Complete hinzu, die stark typisiert ist.

![](mvc-music-store-part-9/_static/image8.png)

Nun aktualisieren wir den Ansichts Code so, dass die Bestell-ID angezeigt wird, wie unten gezeigt.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aktualisieren der Fehler Ansicht

Die Standardvorlage enthält eine Fehler Ansicht im Ordner "freigegebene Ansichten", sodass Sie an anderer Stelle der Site wieder verwendet werden kann. Diese Fehler Ansicht enthält einen sehr einfachen Fehler und verwendet nicht das Website Layout. daher aktualisieren wir Sie.

Da es sich hierbei um eine allgemeine Fehlerseite handelt, ist der Inhalt sehr einfach. Wir fügen eine Meldung und einen Link ein, um zur vorherigen Seite im Verlauf zu navigieren, wenn der Benutzer die Aktion wiederholen möchte.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-8.md)
> [Weiter](mvc-music-store-part-10.md)
