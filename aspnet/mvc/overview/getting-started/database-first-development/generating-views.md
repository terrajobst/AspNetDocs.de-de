---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: Generieren von Sichten für EF-Database First mit ASP.NET MVC-App'
description: Dieses Tutorial konzentriert sich auf die Verwendung von ASP.net-Gerüstbau zum Generieren der Controller und Sichten.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499473"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: Generieren von Sichten für EF-Database First mit ASP.NET MVC-App

Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können. Der generierte Code entspricht den Spalten in der Datenbanktabelle.

Dieses Tutorial konzentriert sich auf die Verwendung von ASP.net-Gerüstbau zum Generieren der Controller und Sichten.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Gerüst hinzufügen
> * Hinzufügen von Links zu neuen Ansichten
> * Anzeigen von Studenten Ansichten
> * Anzeigen von Registrierungs Sichten

## <a name="prerequisite"></a>Voraussetzung

* [Erstellen der Webanwendung und der Datenmodelle](creating-the-web-application.md)

## <a name="add-scaffold"></a>Gerüst hinzufügen

Sie sind bereit, Code zu generieren, der Standarddaten Vorgänge für die Modellklassen bereitstellt. Fügen Sie den Code hinzu, indem Sie ein Gerüst Element hinzufügen. Es gibt zahlreiche Optionen für die Art des Gerüsts, das Sie hinzufügen können. in diesem Tutorial schließt das Gerüst einen Controller und Ansichten ein, die den im vorherigen Abschnitt erstellten Studenten-und Registrierungs Modellen entsprechen.

Um die Konsistenz in Ihrem Projekt aufrechtzuerhalten, fügen Sie den neuen Controller dem Ordner vorhandene **Controller** hinzu. Klicken Sie mit der rechten Maustaste auf den Ordner **Controllers** , und wählen Sie > **Neues Gerüst Element** **Hinzufügen** aus.

Wählen Sie den **MVC 5-Controller mit Ansichten unter Verwendung Entity Framework** Option aus. Mit dieser Option werden Controller und Ansichten zum Aktualisieren, löschen, erstellen und Anzeigen der Daten in Ihrem Modell generiert.

![MVC-Controller hinzufügen](generating-views/_static/image2.png)

Wählen Sie **Student (condesosite. Models)** für die Modell Klasse aus, und wählen Sie **condesouniversitydataentities (condesosite. Models)** für die Kontext Klasse aus. Behalten Sie den Controller Namen als **studentscontroller**bei.

Klicken Sie auf **Hinzufügen**.

Wenn Sie einen Fehler erhalten, kann dies daran liegen, dass Sie das Projekt nicht im vorherigen Abschnitt erstellt haben. Wenn dies der Fall ist, versuchen Sie, das Projekt zu entwickeln, und fügen Sie es dann erneut hinzu.

Nachdem der Code Generierungsprozess fertiggestellt wurde, sehen Sie einen neuen Controller und Ansichten in den **Controllern** und **Ansichten** des Projekts > Ordner " **Students** ".

Führen Sie die gleichen Schritte erneut aus, aber fügen Sie ein Gerüst für die **Registrierungs Klasse hinzu** . Wenn Sie fertig sind, verfügen Sie über eine **EnrollmentsController.cs** -Datei und einen Ordner unter **Sichten** namens **Registrierungen** mit den Sichten Create, DELETE, Details, Edit und Index.

## <a name="add-links-to-new-views"></a>Hinzufügen von Links zu neuen Ansichten

Um Ihnen das Navigieren zu ihren neuen Ansichten zu erleichtern, können Sie den Index Sichten für Schüler und Studenten und Registrierungen einige Hyperlinks hinzufügen. Öffnen Sie die Datei in **views** > **Home** > *Index. cshtml*. Dies ist die Startseite für Ihre Website. Fügen Sie unter dem Jumbotron den folgenden Code hinzu.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Bei der Action Link-Methode ist der erste Parameter der Text, der im Link angezeigt werden soll. Der zweite Parameter ist die Aktion, und der dritte Parameter ist der Name des Controllers. Der erste Link zeigt z. b. auf die Index Aktion in studentscontroller. Der tatsächliche Hyperlink wird aus diesen Werten erstellt. Der erste Link nimmt letztendlich Benutzer an der Datei " **Index. cshtml** " im Ordner " **views/Students** " an.

## <a name="display-student-views"></a>Anzeigen von Studenten Ansichten

Sie werden sicherstellen, dass der Code, der dem Projekt hinzugefügt wurde, eine Liste der Schüler/Studenten anzeigt und Benutzern das Bearbeiten, erstellen oder Löschen der Studenten Datensätze in der Datenbank ermöglicht.

Klicken Sie mit der rechten Maustaste auf die **Ansichten** > **Startseite** > *Index. cshtml* -Datei, und wählen Sie **in Browser anzeigen aus**. Wählen Sie auf der Startseite der Anwendung die Option **Liste der Studenten aus**.

![](generating-views/_static/image6.png)

Beachten Sie auf der Seite **Index** die Liste der Schüler/Studenten und Links, um diese Daten zu ändern. Wählen Sie den Link **neu erstellen** aus, und geben Sie einige Werte für einen neuen Studenten an. Klicken Sie auf **Erstellen**, und beachten Sie, dass der neue Student Ihrer Liste hinzugefügt wird.

Wählen Sie auf der Seite **Index** den Link **Bearbeiten** aus, und ändern Sie einige der Werte für einen Schüler/Student. Klicken Sie auf **Speichern**, und beachten Sie, dass der Student-Datensatz geändert wurde.

Wählen Sie abschließend den Link **Löschen** aus, und bestätigen Sie, dass Sie den Datensatz löschen möchten, indem Sie auf die Schaltfläche **Löschen** klicken.

Wenn Sie keinen Code schreiben, haben Sie Ansichten hinzugefügt, die gängige Vorgänge für die Daten in der Tabelle "Student" ausführen.

Möglicherweise haben Sie bemerkt, dass die Text Bezeichnung für ein Feld auf der Daten Bank Eigenschaft (z. b. **LastName**) basiert, die nicht unbedingt auf der Webseite angezeigt werden soll. Beispielsweise können Sie bevorzugen, dass die Bezeichnung den **Nachnamen**hat. Sie beheben dieses Problem bei der Anzeige später in diesem Tutorial.

## <a name="display-enrollment-views"></a>Anzeigen von Registrierungs Sichten

Die Datenbank enthält eine 1: n-Beziehung zwischen den Tabellen "Student" und "Registrierung" sowie eine 1: n-Beziehung zwischen den Tabellen "Kurs" und "Registrierung". Diese Beziehungen werden von den Sichten für die Registrierung ordnungsgemäß verarbeitet. Navigieren Sie zur Startseite Ihrer Website, und wählen Sie die **Liste mit den Registrierungen** aus, und klicken Sie dann auf den Link **neu erstellen** .

In der Ansicht wird ein Formular zum Erstellen eines neuen Registrierungsdaten Satzes angezeigt. Beachten Sie insbesondere, dass das Formular eine " **CourseID** "-Dropdown Liste und eine " **StudentID** "-Dropdown Liste enthält. Beide werden mit Werten aus den verknüpften Tabellen aufgefüllt.

Außerdem wird die Überprüfung der angegebenen Werte automatisch auf Grundlage des Datentyps des Felds angewendet. Der **Wert erfordert eine** Zahl, sodass eine Fehlermeldung angezeigt wird, wenn Sie versuchen, einen nicht kompatiblen Wert anzugeben: *die Feld Qualität muss eine Zahl sein.*

Sie haben überprüft, ob die automatisch generierten Sichten es Benutzern ermöglichen, mit den Daten in der Datenbank zu arbeiten. Im nächsten Tutorial dieser Reihe aktualisieren Sie die Datenbank und nehmen die entsprechenden Änderungen in der-Webanwendung vor.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Gerüstbau hinzugefügt
> * Links zu neuen Ansichten hinzugefügt
> * Angezeigte Ansichten von Studenten
> * Angezeigte Registrierungs Sichten

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie die Datenbank ändern.
> [!div class="nextstepaction"]
> [Ändern der Datenbank](changing-the-database.md)