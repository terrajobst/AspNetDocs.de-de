---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Teil 1: Übersicht und Erstellen des Projekts | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600327"
---
# <a name="part-1-overview-and-creating-the-project"></a>Teil 1: Übersicht und Erstellen des Projekts

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework ist ein Objekt-/Relationales Mapping-Framework. Die Domänen Objekte in Ihrem Code werden Entitäten in einer relationalen Datenbank zugeordnet. Zum größten Teil müssen Sie sich keine Gedanken über die Datenbankschicht machen, da Sie Entity Framework für Sie kümmert. Der Code bearbeitet die Objekte, und die Änderungen werden in einer Datenbank gespeichert.

## <a name="about-the-tutorial"></a>Informationen zum Tutorial

In diesem Tutorial erstellen Sie eine einfache Store-Anwendung. Die Anwendung besteht aus zwei Hauptbereichen. Normale Benutzer können Produkte anzeigen und Aufträge erstellen:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Administratoren können Produkte erstellen, löschen oder bearbeiten:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erlernen

Hier lernen Sie Folgendes:

- Verwendung von Entity Framework mit ASP.net-Web-API.
- Verwenden von Knockout. js zum Erstellen einer dynamischen Client Benutzeroberfläche.
- Verwenden der Formular Authentifizierung mit der Web-API zum Authentifizieren von Benutzern

Obwohl dieses Tutorial eigenständig ist, sollten Sie zunächst die folgenden Tutorials lesen:

- [Ihre erste ASP.net-Web-API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Erstellen einer Web-API, die CRUD-Vorgänge unterstützt](../creating-a-web-api-that-supports-crud-operations.md)

Einige Kenntnisse von [ASP.NET MVC](../../../../mvc/index.md) sind ebenfalls hilfreich.

## <a name="overview"></a>Übersicht über

Im folgenden finden Sie die Architektur der Anwendung:

- ASP.NET MVC generiert die HTML-Seiten für den Client.
- ASP.net-Web-API macht CRUD-Vorgänge für die Daten (Produkte und Aufträge) verfügbar.
- Entity Framework übersetzt die C# von der Web-API verwendeten Modelle in Daten Bank Entitäten.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Das folgende Diagramm zeigt, wie die Domänen Objekte auf verschiedenen Ebenen der Anwendung dargestellt werden: die Datenbankebene, das Objektmodell und schließlich das Wire-Format, das zum Übertragen von Daten an den Client über HTTP verwendet wird.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

Sie können das tutorialprojekt entweder mithilfe von Visual Web Developer Express oder der Vollversion von Visual Studio erstellen.

Klicken Sie auf der **Start** Seite auf **Neues Projekt**.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Nennen Sie das Projekt productstore, und klicken Sie auf **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Internet Anwendung** aus, und klicken Sie auf **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Die Vorlage "Internet Anwendung" erstellt eine ASP.NET MVC-Anwendung, die die Formular Authentifizierung unterstützt. Wenn Sie die Anwendung jetzt ausführen, haben Sie bereits einige Features:

- Neue Benutzer können sich registrieren, indem Sie auf den Link "Register" in der oberen rechten Ecke klicken.
- Registrierte Benutzer können sich anmelden, indem Sie auf den Link "Anmelden" klicken.

Mitgliedschafts Informationen werden in einer Datenbank gespeichert, die automatisch erstellt wird. Weitere Informationen zur Formular Authentifizierung in ASP.NET MVC finden Sie unter Exemplarische Vorgehensweise [: Verwenden der Formular Authentifizierung in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualisieren der CSS-Datei

Dieser Schritt ist kosmetisch, aber die Seiten werden wie die vorherigen Screenshots dargestellt.

Erweitern Sie in Projektmappen-Explorer den Ordner Inhalt, und öffnen Sie die Datei mit dem Namen Site. CSS. Fügen Sie die folgenden CSS-Stile hinzu:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Nächste](using-web-api-with-entity-framework-part-2.md)
