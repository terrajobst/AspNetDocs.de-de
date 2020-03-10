---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: Ändern der Datenbank für EF-Database First mit ASP.NET MVC-App'
description: Dieses Tutorial konzentriert sich auf die Aktualisierung der Datenbankstruktur und die Weitergabe dieser Änderung in der gesamten Webanwendung.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499521"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: Ändern der Datenbank für EF-Database First mit ASP.NET MVC-App

Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können. Der generierte Code entspricht den Spalten in der Datenbanktabelle.

Dieses Tutorial konzentriert sich auf die Aktualisierung der Datenbankstruktur und die Weitergabe dieser Änderung in der gesamten Webanwendung.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Hinzufügen eines Spaltennamens
> * Hinzufügen der Eigenschaft zu den Ansichten

## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen von Sichten](generating-views.md)

## <a name="add-a-column"></a>Hinzufügen eines Spaltennamens

Wenn Sie die Struktur einer Tabelle in der Datenbank aktualisieren, müssen Sie sicherstellen, dass die Änderung an das Datenmodell, die Ansichten und den Controller weitergegeben wird.

In diesem Tutorial fügen Sie der Tabelle "Student" eine neue Spalte hinzu, um den Vornamen des Studenten aufzuzeichnen. Um diese Spalte hinzuzufügen, öffnen Sie das Datenbankprojekt, und öffnen Sie die Datei Student. SQL. Fügen Sie entweder über den Designer oder den T-SQL-Code eine Spalte mit dem Namen **MiddleName** ein, die ein nvarchar (50) ist, und lässt NULL-Werte zu.

Stellen Sie diese Änderung in der lokalen Datenbank bereit, indem Sie das Datenbankprojekt (oder F5) starten. Das neue Feld wird der Tabelle hinzugefügt. Wenn Sie im SQL Server-Objekt-Explorer nicht angezeigt wird, klicken Sie im Bereich auf die Schaltfläche Aktualisieren.

![neue Spalte anzeigen](changing-the-database/_static/image2.png)

Die neue Spalte ist in der Datenbanktabelle vorhanden, Sie ist jedoch zurzeit nicht in der Datenmodell Klasse vorhanden. Sie müssen das Modell aktualisieren, um die neue Spalte hinzufügen zu können. Öffnen Sie im Ordner **Models** die Datei " **\tosomodel. edmx** ", um das Modell Diagramm anzuzeigen. Beachten Sie, dass das Student-Modell nicht die MiddleName-Eigenschaft enthält. Klicken Sie mit der rechten Maustaste auf eine beliebige Stelle auf der Entwurfs Oberfläche, und wählen Sie **Modell aus Datenbank aktualisieren aus**

Wählen Sie im Update-Assistenten die Registerkarte **Aktualisieren** aus, und wählen Sie dann **Tabellen** > **dbo** > **Student**aus. Klicken Sie auf **Fertig stellen**.

Nachdem der Update Vorgang abgeschlossen ist, enthält das Daten Bank Diagramm die neue **MiddleName** -Eigenschaft. Speichern Sie die Datei " **\desomodel. edmx** ". Sie müssen diese Datei speichern, damit die neue Eigenschaft an die **Student.cs** -Klasse weitergegeben wird. Sie haben nun die Datenbank und das Modell aktualisiert.

Erstellen Sie die Projektmappe.

## <a name="add-the-property-to-the-views"></a>Hinzufügen der Eigenschaft zu den Ansichten

Leider enthalten die Ansichten noch nicht die neue Eigenschaft. Zum Aktualisieren der Ansichten stehen Ihnen zwei Optionen zur Verfügung: Sie können die Ansichten entweder erneut generieren, indem Sie erneut Gerüstbau für die Klasse "Student" hinzufügen, oder Sie können die neue Eigenschaft manuell zu Ihren vorhandenen Ansichten hinzufügen. In diesem Tutorial fügen Sie das Gerüst erneut hinzu, da Sie keine angepassten Änderungen an den automatisch generierten Sichten vorgenommen haben. Sie sollten die-Eigenschaft manuell hinzufügen, wenn Sie Änderungen an den Sichten vorgenommen haben und diese Änderungen nicht verlieren möchten.

Um sicherzustellen, dass die Ansichten neu erstellt werden, löschen Sie den Ordner **Students** unter **views**, und löschen Sie den **studentscontroller**. Klicken Sie dann mit der rechten Maustaste auf den Ordner **Controllers** , und fügen Sie Gerüstbau für das **Student** Model Nennen Sie den Controller " **studentscontroller**". Wählen Sie **Hinzufügen** aus.

Erstellen Sie die Projektmappe erneut. Die Ansichten enthalten nun die MiddleName-Eigenschaft.

![mittleren Namen anzeigen](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Hinzugefügte Spalte
> * Die Eigenschaft wurde den Ansichten hinzugefügt.

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie die Ansicht zum Anzeigen von Details zu einem Student-Datensatz anpassen.
> [!div class="nextstepaction"]
> [Anpassen einer Ansicht](customizing-a-view.md)