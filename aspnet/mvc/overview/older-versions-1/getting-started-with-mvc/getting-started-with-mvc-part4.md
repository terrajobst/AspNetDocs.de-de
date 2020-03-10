---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Erstellen einer Datenbank | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469671"
---
# <a name="creating-a-database"></a>Erstellen einer Datenbank

von [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

In diesem Abschnitt erstellen wir eine neue SQL Express-Datenbank, mit der wir die Filmdaten speichern und abrufen. Wählen Sie in der Visual Web Developer-IDE Ansicht aus. Server-Explorer. Klicken Sie mit der rechten Maustaste auf Datenverbindungen, und klicken Sie auf Verbindung hinzufügen.

![Addconnection](getting-started-with-mvc-part4/_static/image1.png)

Wählen Sie im Dialogfeld Datenquelle auswählen Microsoft SQL Server aus, und klicken Sie dann auf Weiter.

![](getting-started-with-mvc-part4/_static/image2.png)

Geben Sie im Dialogfeld Verbindung hinzufügen für Ihren Server Namen ".\sqlexpress" ein, und geben Sie "Movies" als Namen für die neue Datenbank ein.

[Dialog !["Verbindung hinzufügen"](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Klicken Sie auf OK, und Sie werden gefragt, ob Sie die Datenbank erstellen möchten. Wählen Sie ja aus.

[![Filme erstellen?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Nun haben Sie eine leere Datenbank in Server-Explorer.

![Neue Tabelle hinzufügen](getting-started-with-mvc-part4/_static/image7.png)

Klicken Sie mit der rechten Maustaste auf Tabellen und dann auf Tabelle hinzufügen Die Tabellen-Designer wird angezeigt. Fügen Sie Spalten für ID, Titel, ReleaseDate, Genre und Preis hinzu. Klicken Sie mit der rechten Maustaste auf die Spalte ID, und klicken Sie auf Primärschlüssel festlegen So sieht mein Entwurfs Bereich aus.

[Datenbanktabellen-Editor ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Wählen Sie außerdem die Spalte ID aus, und ändern Sie Unterspalten Eigenschaften unten "Identitäts Spezifikation" in "Ja".

[![IsIdentity-Column-Eigenschaften](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Wenn Sie den Vorgang abgeschlossen haben, klicken Sie auf der Symbolleiste auf das Symbol speichern, oder wählen Sie Datei aus. Speichern Sie aus dem Menü, und benennen Sie die Tabelle "**Movie**" (Singular). Wir haben eine Datenbank und eine Tabelle!

[![Namen auswählen](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Wechseln Sie zurück zu Server-Explorer, klicken Sie mit der rechten Maustaste auf die Tabelle Movie, und wählen Sie dann Tabellendaten anzeigen aus. Geben Sie einige Filme ein, damit unsere Datenbank über einige Daten verfügt.

[Bearbeitung der Datenbanktabelle ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Erstellen eines Modells

Wechseln Sie nun zurück zum Projektmappen-Explorer auf der rechten Seite der IDE, klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie hinzufügen aus. Neues Element.

[![addnewmudelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Wir werden ein Entitäts Modell aus unserer neuen Datenbank erstellen. Dadurch wird dem Projekt eine Reihe von Klassen hinzugefügt, die es uns erleichtern, die Daten in unserer Datenbank abzufragen und zu bearbeiten. Wählen Sie auf der linken Seite des Dialog Felds den Knoten Daten aus, und wählen Sie dann die Vorlage ADO.NET Entity Data Model Item aus. Nennen Sie die Datei Movies. edmx.

[![addnewdatamodel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Klicken Sie auf die Schaltfläche „Hinzufügen“. Daraufhin wird der Assistent "Entity Data Model" gestartet.

Wählen Sie im neu angezeigten Dialogfeld die Option aus Datenbank generieren aus. Da wir soeben eine Datenbank erstellt haben, müssen wir nur die Entity Framework über unsere neue Datenbank und Ihre Tabelle informieren. Klicken Sie auf Weiter, um die Datenbankverbindung in der Konfiguration unserer Webanwendung zu speichern. Aktivieren Sie nun das Kontrollkästchen Tabellen und Movie, und klicken Sie auf Fertigstellen.

[![Entity Data Model-Assistenten](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Nun können wir unsere neue Film Tabelle im Entity Framework Designer sehen und über Code darauf zugreifen.

[![Filme-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Auf der Entwurfs Oberfläche sehen Sie eine "Movie"-Klasse. Diese Klasse wird der "Movie"-Tabelle in der Datenbank zugeordnet, und jede darin enthaltenen Eigenschaften wird einer Spalte mit der Tabelle zugeordnet. Jede Instanz einer "Movie"-Klasse entspricht einer Zeile in der Tabelle "Movie".

Wenn Sie die vom Entity Framework verwendeten standardmäßigen Benennungs-und Mapping-Konventionen nicht kennen, können Sie den Entity Framework Designer verwenden, um Sie zu ändern oder anzupassen. Für diese Anwendung verwenden wir die Standardwerte und speichern die Datei einfach unverändert.

Nun arbeiten wir mit echten Daten.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part3.md)
> [Weiter](getting-started-with-mvc-part5.md)
