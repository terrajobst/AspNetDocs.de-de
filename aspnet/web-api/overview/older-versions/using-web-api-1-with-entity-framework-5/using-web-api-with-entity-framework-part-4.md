---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Teil 4: Hinzufügen einer admin-Ansicht | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600011"
---
# <a name="part-4-adding-an-admin-view"></a>Teil 4: Hinzufügen einer Administrator Ansicht

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Administrator Ansicht hinzufügen

Nun wird die Clientseite und eine Seite hinzugefügt, die Daten vom Administrator Controller verarbeiten kann. Die Seite ermöglicht Benutzern das Erstellen, bearbeiten oder Löschen von Produkten, indem AJAX-Anforderungen an den Controller gesendet werden.

Erweitern Sie in Projektmappen-Explorer den Ordner Controllers, und öffnen Sie die Datei mit dem Namen HomeController.cs. Diese Datei enthält einen MVC-Controller. Fügen Sie eine Methode mit dem Namen `Admin`hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Die **httprouteurl** -Methode erstellt den URI für die Web-API, und wir speichern diese später in der Ansichts Tasche.

Positionieren Sie als nächstes den Textcursor innerhalb der `Admin` Aktionsmethode, klicken Sie mit der rechten Maustaste, und wählen Sie **Ansicht hinzufügen**aus. Dadurch wird das Dialog **Feld Ansicht hinzufügen** angezeigt.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

Benennen Sie im Dialog **Feld Ansicht hinzufügen** die Ansicht "admin". Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie unter **Modell Klasse**die Option "Product (productstore. Models)" aus. Belassen Sie alle anderen Optionen als Standardwerte.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Wenn **Sie auf Hinzufügen** klicken, wird eine Datei namens admin. cshtml unter views/Home hinzugefügt. Öffnen Sie diese Datei, und fügen Sie folgenden HTML-Code hinzu. Dieser HTML-Code definiert die Struktur der Seite, aber noch keine Funktionalität.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Erstellen eines Links zur Administrator Seite

Erweitern Sie in Projektmappen-Explorer den Ordner views, und erweitern Sie dann den freigegebenen Ordner. Öffnen Sie die Datei mit dem Namen \_"Layout. cshtml". Suchen Sie das **UL** -Element mit der ID = "Menu" und einen Aktions Link für die Administrator Ansicht:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Im Beispiel Projekt habe ich einige andere kosmetische Änderungen vorgenommen, z. b. die Zeichenfolge "Ihr Logo hier" zu ersetzen. Diese wirken sich nicht auf die Funktionalität der Anwendung aus. Sie können das Projekt herunterladen und die Dateien vergleichen.

Führen Sie die Anwendung aus, und klicken Sie auf den Link "admin", der oben auf der Startseite angezeigt wird. Die Seite admin sollte wie folgt aussehen:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Momentan führt die Seite nichts aus. Im nächsten Abschnitt verwenden wir Knockout. js, um eine dynamische Benutzeroberfläche zu erstellen.

## <a name="add-authorization"></a>Autorisierung hinzufügen

Alle Benutzer, die die Website besuchen, können auf die Seite "admin" zugreifen. Ändern wir dies, um die Berechtigung für Administratoren einzuschränken.

Beginnen Sie, indem Sie eine Administrator Rolle und einen Administrator Benutzer hinzufügen. Erweitern Sie in Projektmappen-Explorer den Ordner Filter, und öffnen Sie die Datei mit dem Namen InitializeSimpleMembershipAttribute.cs. Suchen Sie den `SimpleMembershipInitializer`-Konstruktor. Fügen Sie nach dem Aufrufen von **WebSecurity. initializedatabaseconnetction**den folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Dies ist eine schnelle und geänderte Möglichkeit, die Rolle "Administrator" hinzuzufügen und einen Benutzer für die Rolle zu erstellen.

Erweitern Sie in Projektmappen-Explorer den Ordner Controllers, und öffnen Sie die Datei HomeController.cs. Fügen Sie das Attribut **autorisieren** der `Admin`-Methode hinzu.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Öffnen Sie die Datei AdminController.cs, und fügen Sie das Attribut **autorisieren** der gesamten `AdminController` Klasse hinzu.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC und die Web-API definieren beide **Autorisierungs** Attribute in verschiedenen Namespaces. MVC verwendet **System. Web. MVC. autorizeattribute**, während die Web-API **System. Web. http. autorizeattribute**verwendet.

Jetzt können nur Administratoren die Administrator Seite anzeigen. Außerdem muss die Anforderung ein Authentifizierungs Cookie enthalten, wenn Sie eine HTTP-Anforderung an den Administrator Controller senden. Wenn dies nicht der Fall ist, sendet der Server eine HTTP 401-Antwort (nicht autorisiert). Sie können dies in "fddler" sehen, indem Sie eine GET-Anforderung an `http://localhost:*port*/api/admin`senden.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-3.md)
> [Weiter](using-web-api-with-entity-framework-part-5.md)
