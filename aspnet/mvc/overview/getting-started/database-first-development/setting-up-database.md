---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Tutorial: Einstieg in EF Database First mit MVC 5'
description: In diesem Tutorial wird gezeigt, wie Sie mit einer vorhandenen Datenbank beginnen und schnell eine Webanwendung erstellen, die es Benutzern ermöglicht, mit den Daten zu interagieren.
author: Rick-Anderson
ms.author: riande
ms.date: 01/15/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: a760767839a834a9c7e9fe358a3fd806a833261f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471459"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Tutorial: Einstieg in EF Database First mit MVC 5

Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können. Der generierte Code entspricht den Spalten in der Datenbanktabelle. Im letzten Teil der Reihe erfahren Sie, wie Sie Daten Anmerkungen zum Datenmodell hinzufügen, um Validierungsanforderungen anzugeben und die Formatierung anzuzeigen. Wenn Sie fertig sind, können Sie mit einem Azure-Artikel fortfahren, um zu erfahren, wie Sie eine .net-APP und eine SQL-Datenbank für Azure App Service bereitstellen.

In diesem Tutorial wird gezeigt, wie Sie mit einer vorhandenen Datenbank beginnen und schnell eine Webanwendung erstellen, die es Benutzern ermöglicht, mit den Daten zu interagieren. Er verwendet die Entity Framework 6 und MVC 5, um die Webanwendung zu erstellen. Mit der ASP.net Gerüstbau Funktion können Sie automatisch Code zum Anzeigen, aktualisieren, erstellen und Löschen von Daten generieren. Mithilfe der Veröffentlichungs Tools in Visual Studio können Sie die Website und die Datenbank problemlos in Azure bereitstellen.

Dieser Teil der Reihe konzentriert sich auf die Erstellung einer Datenbank und deren Auffüllen mit Daten.

Diese Serie wurde mit Beiträgen von Tom Dykstra und Rick Anderson verfasst. Es wurde auf der Grundlage von Feedback von Benutzern im Abschnitt "Kommentare" verbessert.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten der Datenbank

## <a name="prerequisites"></a>Voraussetzungen

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="set-up-the-database"></a>Einrichten der Datenbank

Um die Umgebung einer vorhandenen Datenbank zu imitieren, erstellen Sie zunächst eine Datenbank mit vorab gefüllten Daten, und erstellen Sie dann die Webanwendung, die eine Verbindung mit der Datenbank herstellt.

Dieses Tutorial wurde mit der Verwendung von localdb mit Visual Studio 2017 entwickelt. Sie können einen vorhandenen Datenbankserver anstelle von localdb verwenden, aber abhängig von Ihrer Version von Visual Studio und dem Typ der Datenbank werden alle Daten Tools in Visual Studio möglicherweise nicht unterstützt. Wenn die Tools für Ihre Datenbank nicht verfügbar sind, müssen Sie möglicherweise einige der datenbankspezifischen Schritte in der Management Suite für Ihre Datenbank ausführen.

Wenn Sie ein Problem mit den Datenbanktools in Ihrer Version von Visual Studio haben, stellen Sie sicher, dass Sie die neueste Version der Datenbanktools installiert haben. Informationen zum Aktualisieren oder Installieren der Datenbanktools finden Sie unter [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Starten Sie Visual Studio, und erstellen Sie ein **SQL Server-Datenbankprojekt**. Nennen Sie das Projekt **conjesouniversitydata**.

![Erstellen eines Datenbankprojekts](setting-up-database/_static/image1.png)

Sie verfügen jetzt über ein leeres Datenbankprojekt. Um sicherzustellen, dass Sie diese Datenbank in Azure bereitstellen können, legen Sie Azure SQL-Datenbank als Zielplattform für das Projekt fest. Wenn die Zielplattform festgelegt wird, wird die Datenbank nicht bereitgestellt. Es bedeutet nur, dass das Datenbankprojekt überprüft, ob der Daten bankentwurf mit der Zielplattform kompatibel ist. Um die Zielplattform festzulegen, öffnen Sie die **Eigenschaften** für das Projekt, und wählen Sie **Microsoft Azure SQL-Datenbank** für die Zielplattform aus.

Sie können die für dieses Tutorial benötigten Tabellen erstellen, indem Sie SQL-Skripts hinzufügen, die die Tabellen definieren. Klicken Sie mit der rechten Maustaste auf das Projekt, und fügen Sie es hinzu. Wählen Sie **Tabellen und Sichten** > **Tabelle** aus, und benennen Sie Sie als *Student*.

Ersetzen Sie in der Tabellen Datei den T-SQL-Befehl durch den folgenden Code, um die Tabelle zu erstellen.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Beachten Sie, dass das Entwurfs Fenster automatisch mit dem Code synchronisiert wird. Sie können entweder mit dem Code oder mit dem Designer arbeiten.

![Code und Entwurf anzeigen](setting-up-database/_static/image5.png)

Fügen Sie eine weitere Tabelle hinzu. Nennen Sie diese Zeit Course, und verwenden Sie den folgenden T-SQL-Befehl.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Wiederholen Sie den Vorgang, um eine Tabelle mit dem Namen "Registrierung" zu erstellen.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Sie können Ihre Datenbank mit Daten auffüllen, indem Sie ein Skript ausführen, das nach der Bereitstellung der Datenbank ausgeführt wird. Fügen Sie dem Projekt ein Skript nach der Bereitstellung hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt, und fügen Sie es hinzu. Wählen Sie **Benutzer** **Skripts > Skript nach der Bereitstellung**aus. Sie können den Standardnamen verwenden.

Fügen Sie dem Skript nach der Bereitstellung den folgenden T-SQL-Code hinzu. Dieses Skript fügt der Datenbank einfach Daten hinzu, wenn kein übereinstimmender Datensatz gefunden wird. Es werden keine Daten überschrieben oder gelöscht, die Sie möglicherweise in die Datenbank eingegeben haben.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Beachten Sie, dass das Skript nach der Bereitstellung jedes Mal ausgeführt wird, wenn Sie das Datenbankprojekt bereitstellen. Daher müssen Sie Ihre Anforderungen beim Schreiben dieses Skripts sorgfältig berücksichtigen. In einigen Fällen möchten Sie möglicherweise jedes Mal, wenn das Projekt bereitgestellt wird, von einem bekannten Datensatz ausgehen. In anderen Fällen möchten Sie die vorhandenen Daten möglicherweise in keiner Weise ändern. Basierend auf Ihren Anforderungen können Sie entscheiden, ob Sie ein Skript nach der Bereitstellung oder das, was Sie in das Skript einschließen müssen, benötigen. Weitere Informationen zum Auffüllen der Datenbank mit einem Skript nach der Bereitstellung finden Sie unter [einschließen von Daten in ein SQL Server Datenbankprojekt](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Sie verfügen jetzt über vier SQL-Skriptdateien, aber keine tatsächlichen Tabellen. Sie sind bereit, das Datenbankprojekt in localdb bereitzustellen. Klicken Sie in Visual Studio auf die Schaltfläche Start (oder F5), um das Datenbankprojekt zu erstellen und bereitzustellen. Überprüfen Sie auf der Registerkarte **Ausgabe** , ob Build und Bereitstellung erfolgreich war.

Um festzustellen, ob die neue Datenbank erstellt wurde, öffnen Sie **SQL Server-Objekt-Explorer** , und suchen Sie den Namen des Projekts auf dem richtigen lokalen Daten Bank Server (in diesem Fall **(localdb) \projectv13**).

Um anzuzeigen, dass die Tabellen mit Daten aufgefüllt sind, klicken Sie mit der rechten Maustaste auf eine Tabelle, und wählen Sie **Daten anzeigen**aus.

![Tabellendaten anzeigen](setting-up-database/_static/image9.png)

Eine bearbeitbare Ansicht der Tabellendaten wird angezeigt. Wenn Sie z. b. **Tabellen** > **dbo. Course** > **Daten anzeigen**auswählen, wird eine Tabelle mit drei Spalten (**Kurs**, **Titel**und **Guthaben**) und vier Zeilen angezeigt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Ein Einführ Endes Beispiel für Code First Entwicklung finden Sie unter [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md). Ein erweitertes Beispiel finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC 4-APP](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Anleitungen zum Auswählen des zu verwendenden Entity Framework Ansatzes finden Sie unter [Entity Framework Entwicklungsansätze](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten der Datenbank

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie die Webanwendung und die Datenmodelle erstellen.
> [!div class="nextstepaction"]
> [Erstellen der Webanwendung und der Datenmodelle](creating-the-web-application.md)
