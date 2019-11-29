---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Getting Started with ASP.NET 4,7 Web Forms und Visual Studio 2017 | Microsoft-Dokumentation
author: Erikre
description: Diese Schritt-für-Schritt-tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net-Web Forms Anwendung mit ASP.NET 4,7 und Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615454"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Getting Started with ASP.NET 4,5 Web Forms und Visual Studio 2017

[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

In dieser tutorialreihe wird gezeigt, wie Sie eine ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio 2017 erstellen. 

## <a name="introduction"></a>Einführung

Diese tutorialreihe führt Sie durch die Erstellung einer ASP.net Web Forms-Anwendung mit Visual Studio 2017 und ASP.NET 4,5. Sie erstellen eine Anwendung mit dem Namen **Wingtip Toys** -eine vereinfachte Store Front-Website, die Elemente online verkauft. Während der Reihe werden die neuen ASP.NET 4,5-Features hervorgehoben.

### <a name="target-audience"></a>Zielgruppe

Entwickler, die mit ASP.net Web Forms vertraut sind, sind die Zielgruppe für diese tutorialreihe.

Sie sollten über einige Kenntnisse in den folgenden Bereichen verfügen:

- Objektorientierte Programmierung (OOP) und Sprachen
- Webentwicklung (HTML, CSS, JavaScript)
- Relationale Datenbanken
- N-schichtige Architektur

Um diese Bereiche zu überprüfen, sollten Sie den folgenden Inhalt untersuchen:

- [Erste Schritte mit Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Webentwicklung](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, jQuery](http://w3schools.com/)
- [Relationale Datenbank](http://en.wikipedia.org/wiki/Relational_database)
- [Architektur mit mehreren Ebenen](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Anwendungs Features

Zu den in dieser Reihe dargestellten ASP.net Web Form-Features gehören:

- Das Webanwendungs Projekt (nicht das Website Projekt)
- Web Forms
- Master Seiten, Konfiguration
- Bootstrap
- Entity Framework Code First, localdb
- Anforderungs Validierung
- Stark typisierte Daten Steuerelemente
- Modellbindung
- Datenanmerkungen
- Wert Anbieter
- SSL und OAuth
- ASP.net Identity, Konfiguration und Autorisierung
- Unaufdringliche Validierung
- Routing
- ASP.NET – Fehlerbehandlung

### <a name="application-scenarios-and-tasks"></a>Anwendungsszenarien und-Aufgaben

Zu den Aufgaben der Lernprogramm Reihe gehören

- Erstellen, überprüfen und Ausführen eines neuen Projekts
- Erstellen einer Datenbankstruktur
- Initialisieren und Seeding einer Datenbank
- Anpassen der Benutzeroberfläche mit Stilen, Grafiken und einer Master Seite
- Hinzufügen von Seiten und Navigation
- Anzeigen von Menü Details und Produktdaten
- Erstellen eines Einkaufswagen
- Hinzufügen von SSL-und OAuth-Unterstützung
- Hinzufügen einer Zahlungsmethode
- Einschließen einer Administrator Rolle und eines Benutzers für die Anwendung
- Einschränken des Zugriffs auf bestimmte Seiten und Ordner
- Hochladen einer Datei in die Webanwendung
- Implementieren der Eingabevalidierung
- Routen für die Webanwendung werden registriert
- Implementieren der Fehlerbehandlung und der Fehler Protokollierung

## <a name="overview"></a>Übersicht über

Diese tutorialreihe richtet sich an Benutzer, die mit Programmier Konzepten vertraut sind, aber noch nicht in ASP.net Web Forms. Wenn Sie bereits mit ASP.net Web Forms vertraut sind, können Sie mit dieser Reihe noch mehr über neue ASP.NET 4,5-Features erfahren. Leser, die mit Programmier Konzepten und ASP.net-Web Forms nicht vertraut sind, finden Sie in den zusätzlichen Web Forms Tutorials im Abschnitt " [Getting Started](../../../index.md) " auf der ASP.NET-Website.

Die in dieser tutorialreihe bereitgestellte ASP.NET 4,5 umfasst die folgenden Features:

- Eine einfache Benutzeroberfläche zum Erstellen von Projekten, die [Unterstützung für viele ASP.NET-Frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC und die Web-API) bietet.
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), ein Layout, Design, Design und reaktionsfähiges Design Framework.
- [ASP.net Identity](../../../../identity/index.md)ein neues ASP.NET-Mitgliedschaftssystem, das in allen ASP.NET-Frameworks identisch funktioniert und mit anderen Webhosting-Software als IIS funktioniert.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Ein Update der-Entity Framework, die Folgendes ermöglicht:
  - Abrufen und Bearbeiten von Daten als stark typisierte Objekte
  - Asynchrones Zugreifen auf Daten
  - Behandeln vorübergehender Verbindungsfehler
  - SQL-Anweisungen protokollieren

Eine komplette Liste der ASP.NET 4,5-Features finden Sie unter [ASP.net and Web Tools für Visual Studio 2013 Anmerkungen](../../../../visual-studio/overview/2013/release-notes.md)zu dieser Version.

### <a name="the-wingtip-toys-sample-application"></a>Die Wingtip Toys-Beispielanwendung

Die folgenden Screenshots stammen aus der ASP.net Web Forms-Anwendung, die Sie in dieser tutorialreihe erstellen. Wenn Sie die Anwendung in Visual Studio ausführen, wird die folgende Web-Startseite angezeigt.

![Wingtip Toys-Standardseite](introduction-and-overview/_static/image1.png)

Sie können sich als neuer Benutzer registrieren oder sich als vorhandener Benutzer anmelden. Der obere Navigationsbereich enthält Links zu Produktkategorien und deren Produkten aus der Datenbank.

Wenn Sie **Produkte**auswählen, werden alle verfügbaren Produkte angezeigt. 

![Wingtip Toys-Produkte](introduction-and-overview/_static/image2.png)

Wenn Sie ein bestimmtes Produkt auswählen, werden Produktdetails angezeigt.

![Wingtip Toys-Produkt Details](introduction-and-overview/_static/image3.png)

Als Benutzer können Sie sich mit Web Forms Vorlagen Standardfunktionalität registrieren und anmelden. In diesem Tutorial wird auch erläutert, wie Sie sich mit einem vorhandenen Gmail-Konto anmelden. Außerdem können Sie sich als Administrator anmelden, um Produkte aus der Datenbank hinzuzufügen und daraus zu entfernen.

![Wingtip Toys-anmelden](introduction-and-overview/_static/image4.png)

Wenn Sie sich als Benutzer angemeldet haben, können Sie Produkte zum Warenkorb hinzufügen und mit PayPal Auschecken. Die Beispielanwendung ist so konzipiert, dass Sie in der Developer Sandbox von PayPal funktioniert. Es findet keine tatsächliche Money-Transaktion statt.

![Wingtip Toys: Einkaufswagen](introduction-and-overview/_static/image5.png)

PayPal bestätigt Ihre Konto-, Bestell-und Zahlungsinformationen.

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

Nach der Rückgabe von PayPal können Sie Ihre Bestellung überprüfen und vervollständigen.

![Wingtip Toys-Bestell Überprüfung](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Erforderliche Voraussetzungen

Vergewissern Sie sich, dass die folgende Software auf Ihrem Computer installiert ist, bevor Sie beginnen:

- [Microsoft Visual Studio 2017 oder Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

Der .NET Framework wird automatisch installiert.

Diese tutorialreihe verwendet Microsoft Visual Studio Community 2017. Diese tutorialreihe können Sie entweder mithilfe von oder Microsoft Visual Studio 2017 durchführen.

Beachten Sie die folgenden Informationen zu Visual Studio:

* Microsoft Visual Studio 2017 und Microsoft Visual Studio Community 2017 werden in dieser tutorialreihe als *Visual Studio* bezeichnet.

* Visual Studio 2017 wird neben allen älteren Versionen installiert, die bereits installiert sind. Websites, die in früheren Versionen erstellt wurden, können in Visual Studio 2017 geöffnet und weiterhin in früheren Versionen geöffnet werden.

* Wenn Sie Visual Studio zum ersten Mal gestartet haben, wird davon ausgegangen, dass Sie die *Webentwicklungs* Einstellungen ausgewählt haben. Weitere Informationen finden Sie unter Gewusst [wie: Auswählen von Einstellungen für die Webentwicklungs Umgebung](https://msdn.microsoft.com/library/ff521558.aspx).

Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie mit dem Erstellen des in dieser tutorialreihe dargestellten Webprojekts beginnen.

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

 Sie können die abgeschlossene Beispielanwendung jederzeit von der MSDN Samples-Website herunterladen:

[Getting Started with ASP.NET 4,5 Web Forms and Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Dieser Download umfasst die folgenden Elemente:

- Die Beispielanwendung im Ordner *wingtiptoys* .
- Die Ressourcen, die zum Erstellen der Beispielanwendung im Ordner *wingtiptoys-Assets* im Ordner *wingtiptoys* verwendet werden.

Der Download ist eine *ZIP* -Datei. Um das abgeschlossene Projekt anzuzeigen, das in dieser tutorialreihe erstellt wird *C#* , suchen Sie den Ordner in der ZIP-Datei, und wählen Sie ihn Speichern Sie C# den Ordner in dem Ordner, den Sie für die Arbeit mit Visual Studio-Projekten verwenden. Standardmäßig lautet der Ordner Visual Studio 2017-Projekte wie folgt:

<strong>C:\Benutzer&#92;</strong>  <strong><em>&lt;Benutzername&gt;</em></strong> <strong>\source\repos</strong>

Benennen Sie ***C#*** den Ordner in ***wingtiptoys***um.

> [!NOTE]
> Wenn Sie bereits über einen Ordner mit dem Namen *wingtiptoys* im Ordner "Projekte" verfügen, benennen Sie den vorhandenen *C#* Ordner vorübergehend um, bevor Sie den Ordner in *wingtiptoys*umbenennen.

Um das abgeschlossene Projekt auszuführen, öffnen Sie den Ordner *wingtiptoys* , und doppelklicken Sie auf die Datei *wingtiptoys. sln* . Visual Studio 2017 öffnet das Projekt. Klicken Sie anschließend in **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei *default. aspx* , und wählen Sie **in Browser anzeigen aus**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>ASP.net-Web Forms Quiz zum Überprüfen von Inhalten

Nachdem Sie die tutorialreihe abgeschlossen haben, erstellen Sie ein Quiz, um Ihr Wissen zu testen und wichtige Konzepte zu verstärken Jede Frage enthält eine Erläuterung und Links zu weiteren Anleitungen.

* [ASP.net Web Forms Quiz](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Lernprogramm Unterstützung und Kommentare

Für Fragen und Kommentare verwenden Sie den Q-und einen-Abschnitt, der auf der Beispielseite mit den ersten Schritten [mit ASP.NET 4,5 Web Forms und Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) enthalten ist.

Kommentare zu dieser tutorialreihe sind willkommen. Wenn diese tutorialreihe aktualisiert wird, wird jeder Versuch unternommen, Korrekturen oder Verbesserungsvorschläge in Erwägung zu gezogen.

Wenn ein Fehler auftritt, können die entsprechenden Fehlermeldungen verwirrend sein, und es gibt keine gute Erklärung, wie Sie Sie beheben können. Hilfe finden Sie in den ASP.net- [Foren](https://forums.asp.net/). Eine weitere gute Quelle ist der Q-und A-Abschnitt der Beispielseite " [Getting Started with ASP.NET 4,5 Web Forms and Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)". 

> [!div class="step-by-step"]
> [Nächste](create-the-project.md)
