---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: Erstellen der Webanwendung und der Datenmodelle für EF-Database First mit ASP.NET MVC'
description: Dieses Tutorial konzentriert sich auf das Erstellen der Webanwendung und das Erstellen der Datenmodelle basierend auf den Datenbanktabellen.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499527"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Tutorial: Erstellen der Webanwendung und der Datenmodelle für EF-Database First mit ASP.NET MVC

 Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können. Der generierte Code entspricht den Spalten in der Datenbanktabelle.

Dieses Tutorial konzentriert sich auf das Erstellen der Webanwendung und das Erstellen der Datenmodelle basierend auf den Datenbanktabellen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen einer ASP.NET-Web-App
> * Generieren der Modelle

## <a name="prerequisites"></a>Voraussetzungen

* [Ersten Schritte mit Entity Framework 6 Database First mithilfe von MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

Erstellen Sie in einer neuen Projekt Mappe oder in derselben Projekt Mappe wie das Datenbankprojekt in Visual Studio ein neues Projekt, und wählen Sie die Vorlage **ASP.NET-Webanwendung** aus. Nennen Sie das Projekt **conjesite**.

![Erstellen des Projekts](creating-the-web-application/_static/image1.png)

Klicken Sie auf **OK**.

Wählen Sie im Fenster Neues ASP.net-Projekt die **MVC** -Vorlage aus. Sie können den **Host in der Cloud** -Option vorerst löschen, da Sie die Anwendung später in der Cloud bereitstellen. Klicken Sie auf **OK** , um die Anwendung zu erstellen.

Das Projekt wird mit den Standard Dateien und-Ordnern erstellt.

In diesem Tutorial wird Entity Framework 6 verwendet. Im Fenster "nuget-Pakete verwalten" können Sie die Version der Entity Framework im Projekt überprüfen. Aktualisieren Sie ggf. Ihre Version von Entity Framework.

![Version anzeigen](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generieren der Modelle

Nun erstellen Sie Entity Framework Modelle aus den Datenbanktabellen. Diese Modelle sind Klassen, die Sie verwenden, um mit den Daten zu arbeiten. Jedes Modell spiegelt eine Tabelle in der Datenbank wider und enthält Eigenschaften, die den Spalten in der Tabelle entsprechen.

Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , und wählen Sie **Hinzufügen** und **Neues Element**.

Wählen Sie im Fenster Neues Element hinzufügen im linken Bereich **Daten** aus, und **ADO.NET Entity Data Model** aus den Optionen im mittleren Bereich. Benennen Sie die neue Modell **Datei mit**dem Namen "".

Klicken Sie auf **Hinzufügen**.

Wählen Sie im Assistenten für Entity Data Model den **EF-Designer aus der Datenbank aus**.

Klicken Sie auf **Weiter**.

Wenn Sie in Ihrer Entwicklungsumgebung Datenbankverbindungen definiert haben, wird möglicherweise eine dieser Verbindungen vorab ausgewählt. Sie möchten jedoch eine neue Verbindung mit der Datenbank erstellen, die Sie im ersten Teil dieses Tutorials erstellt haben. Klicken Sie auf die Schaltfläche **neue Verbindung** .

Geben Sie im Verbindungs Eigenschaftenfenster den Namen des lokalen Servers an, auf dem die Datenbank erstellt wurde (in diesem Fall **(localdb) \projectv13**). Nachdem Sie den Servernamen bereitgestellt haben, wählen Sie die Conto souniversitydata aus den verfügbaren Datenbanken aus.

![Verbindungs Eigenschaften festlegen](creating-the-web-application/_static/image8.png)

Klicken Sie auf **OK**.

Die richtigen Verbindungs Eigenschaften werden nun angezeigt. Sie können den Standardnamen für die Verbindung in der Datei "Web. config" verwenden.

Klicken Sie auf **Weiter**.

Wählen Sie die neueste Version von Entity Framework aus.

Klicken Sie auf **Weiter**.

Wählen Sie **Tabellen** aus, um Modelle für alle drei Tabellen zu generieren.

Klicken Sie auf **Fertig stellen**.

Wenn Sie eine Sicherheitswarnung erhalten, klicken Sie auf **OK** , um die Ausführung der Vorlage fortzusetzen.

Die Modelle werden aus den Datenbanktabellen generiert, und es wird ein Diagramm angezeigt, in dem die Eigenschaften und Beziehungen zwischen den Tabellen angezeigt werden.

![Diagramm des Modells](creating-the-web-application/_static/image11.png)

Der Ordner Models enthält jetzt viele neue Dateien im Zusammenhang mit den Modellen, die aus der Datenbank generiert wurden.

Die **ContosoModel.Context.cs** -Datei enthält eine Klasse, die von der **dbcontext** -Klasse abgeleitet ist, und stellt eine-Eigenschaft für jede Modell Klasse bereit, die einer Datenbanktabelle entspricht. Die **Course.cs**-, **Enrollment.cs**-und **Student.cs** -Dateien enthalten die Modellklassen, die die Datenbanktabellen darstellen. Beim Arbeiten mit Gerüstbau werden sowohl die Kontext Klasse als auch die Modellklassen verwendet.

Bevor Sie mit diesem Tutorial fortfahren, erstellen Sie das Projekt. Im nächsten Abschnitt generieren Sie Code auf der Grundlage der Datenmodelle, aber dieser Abschnitt funktioniert nicht, wenn das Projekt nicht erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen einer ASP.net-Web-App
> * Generierte Modelle

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Code generieren, der auf den Datenmodellen basiert.
> [!div class="nextstepaction"]
> [Erstellen von Sichten](generating-views.md)
