---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutorial: verbessern der Datenüberprüfung für EF-Database First mit ASP.NET MVC-App'
description: Dieses Tutorial konzentriert sich auf das Hinzufügen von Daten Anmerkungen zum Datenmodell, um Validierungsanforderungen anzugeben und die Formatierung anzuzeigen.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499533"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: verbessern der Datenüberprüfung für EF-Database First mit ASP.NET MVC-App

Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können. Der generierte Code entspricht den Spalten in der Datenbanktabelle.

Dieses Tutorial konzentriert sich auf das Hinzufügen von Daten Anmerkungen zum Datenmodell, um Validierungsanforderungen anzugeben und die Formatierung anzuzeigen. Es wurde auf der Grundlage von Feedback von Benutzern im Abschnitt "Kommentare" verbessert.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Hinzufügen von Daten Anmerkungen
> * Metadatenklassen hinzufügen

## <a name="prerequisites"></a>Voraussetzungen

* [Anpassen einer Ansicht](customizing-a-view.md)

## <a name="add-data-annotations"></a>Hinzufügen von Daten Anmerkungen

Wie Sie in einem früheren Thema gesehen haben, werden einige Daten Validierungsregeln automatisch auf die Benutzereingabe angewendet. Sie können z. b. nur eine Zahl für die Grade-Eigenschaft angeben. Wenn Sie weitere Daten Validierungsregeln angeben möchten, können Sie Ihrer Modell Klasse Daten Anmerkungen hinzufügen. Diese Anmerkungen werden in der gesamten Webanwendung für die angegebene Eigenschaft angewendet. Sie können auch Formatierungs Attribute anwenden, die die Anzeige der Eigenschaften ändern. beispielsweise, indem Sie den für Text Bezeichnungen verwendeten Wert ändern.

In diesem Tutorial fügen Sie Daten Anmerkungen hinzu, um die Länge der für die Eigenschaften FirstName, LastName und MiddleName angegebenen Werte einzuschränken. In der-Datenbank sind diese Werte auf 50 Zeichen beschränkt. in Ihrer Webanwendung wird diese Zeichen Beschränkung jedoch zurzeit nicht erzwungen. Wenn ein Benutzer mehr als 50 Zeichen für einen dieser Werte bereitstellt, stürzt die Seite ab, wenn versucht wird, den Wert in der Datenbank zu speichern. Außerdem wird Grade auf Werte zwischen 0 und 4 beschränkt.

Wählen Sie Modelle ** > "** ** > "** , und öffnen Sie die Datei *Student.cs* . Fügen Sie der-Klasse den folgenden markierten Code hinzu.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Öffnen Sie *Enrollment.cs* , und fügen Sie den folgenden hervorgehobenen Code hinzu.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Erstellen Sie die Projektmappe.

Klicken Sie auf **Liste von Schülern** und dann auf **Bearbeiten**. Wenn Sie versuchen, mehr als 50 Zeichen einzugeben, wird eine Fehlermeldung angezeigt.

![Fehlermeldung anzeigen](enhancing-data-validation/_static/image1.png)

Kehren Sie zur Startseite zurück. Klicken Sie auf **Liste der Registrierungen** , und wählen Sie **Bearbeiten**aus. Versuchen Sie, eine Stufe oberhalb von 4 bereitzustellen. Dieser Fehler wird *angezeigt: die Feld Qualität muss zwischen 0 und 4 liegen.*

## <a name="add-metadata-classes"></a>Metadatenklassen hinzufügen

Das direkte Hinzufügen der Validierungs Attribute zur Modell Klasse funktioniert, wenn Sie nicht erwarten, dass die Datenbank geändert wird. Wenn sich die Datenbank jedoch ändert und Sie die Modell Klasse neu generieren müssen, verlieren Sie alle Attribute, die Sie auf die Modell Klasse angewendet haben. Diese Vorgehensweise kann sehr ineffizient sein und den Verlust wichtiger Validierungsregeln anfällig machen.

Um dieses Problem zu vermeiden, können Sie eine Metadatenklasse hinzufügen, die die Attribute enthält. Wenn Sie die Modell Klasse der Metadatenklasse zuordnen, werden diese Attribute auf das Modell angewendet. Bei diesem Ansatz kann die Modell Klasse neu generiert werden, ohne alle Attribute zu verlieren, die auf die Metadatenklasse angewendet wurden.

Fügen Sie im Ordner **Models** eine Klasse mit dem Namen *Metadata.cs*hinzu.

Ersetzen Sie den Code in *Metadata.cs* durch den folgenden Code.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Diese Metadatenklassen enthalten alle Validierungs Attribute, die Sie zuvor auf die Modellklassen angewendet haben. Das **Anzeige** Attribut wird verwendet, um den für Text Bezeichnungen verwendeten Wert zu ändern.

Nun müssen Sie die Modellklassen den Metadatenklassen zuordnen.

Fügen Sie im Ordner **Models** eine Klasse mit dem Namen *PartialClasses.cs*hinzu.

Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Beachten Sie, dass jede Klasse als `partial` Klasse gekennzeichnet ist und jeweils mit dem Namen und Namespace der Klasse übereinstimmt, die automatisch generiert wird. Durch Anwenden des metadatenattributs auf die partielle Klasse stellen Sie sicher, dass die Daten Validierungs Attribute auf die automatisch generierte Klasse angewendet werden. Diese Attribute gehen nicht verloren, wenn Sie die Modellklassen neu generieren, da das Metadatenattribut in partiellen Klassen angewendet wird, die nicht erneut generiert werden.

Zum erneuten Generieren der automatisch generierten Klassen öffnen Sie die Datei " *\tosomodel. edmx* ". Klicken Sie erneut mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Modell aus Datenbank aktualisieren aus**. Obwohl Sie die Datenbank nicht geändert haben, werden die Klassen durch diesen Prozess neu generiert. Wählen Sie auf der Registerkarte **Aktualisieren** die Option **Tabellen** und **Fertig**stellen aus.

Speichern Sie *die Datei "* %% amp; quot;", um die Änderungen zu übernehmen.

Öffnen Sie die Datei *Student.cs* oder *Enrollment.cs* , und beachten Sie, dass sich die zuvor angewendeten Daten Validierungs Attribute nicht mehr in der Datei befinden. Führen Sie die Anwendung jedoch aus, und beachten Sie, dass die Validierungsregeln weiterhin angewendet werden, wenn Sie Daten eingeben.

## <a name="conclusion"></a>Zusammenfassung

Diese Reihe bietet ein einfaches Beispiel für das Generieren von Code aus einer vorhandenen Datenbank, die es Benutzern ermöglicht, Daten zu bearbeiten, zu aktualisieren, zu erstellen und zu löschen. Zum Erstellen des Projekts wurden ASP.NET MVC 5, Entity Framework und ASP.net Gerüstbau verwendet. 

Ein Einführ Endes Beispiel für Code First Entwicklung finden Sie unter [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md). 

Ein erweitertes Beispiel finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC 4-APP](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Beachten Sie, dass die dbcontext-API, die Sie für die Arbeit mit Daten in Database First verwenden, mit der API identisch ist, die Sie zum Arbeiten mit Daten in Code First verwenden. Auch wenn Sie Database First verwenden möchten, können Sie erfahren, wie komplexere Szenarien behandelt werden, z. b. das Lesen und aktualisieren verwandter Daten, das Behandeln von Parallelitäts Konflikten usw. aus einem Code First Tutorial. Der einzige Unterschied besteht darin, wie die Datenbank-, Kontext Klassen-und Entitäts Klassen erstellt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Eine vollständige Liste der Daten Validierungs Anmerkungen, die Sie auf Eigenschaften und Klassen anwenden können, finden Sie unter [System. ComponentModel. DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Hinzugefügte Daten Anmerkungen
> * Metadatenklassen hinzugefügt

Informationen zum Bereitstellen einer Web-App und einer SQL-Datenbank für Azure App Service finden Sie in diesem Tutorial:
> [!div class="nextstepaction"]
> [Bereitstellen einer .net-App für Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
