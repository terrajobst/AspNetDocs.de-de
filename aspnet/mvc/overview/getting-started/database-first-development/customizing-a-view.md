---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: Anpassen der Ansicht für EF-Database First mit ASP.NET MVC-App'
description: Dieses Tutorial konzentriert sich auf das Ändern der automatisch generierten Sichten, um die Darstellung zu verbessern.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471519"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: Anpassen der Ansicht für EF-Database First mit ASP.NET MVC-App

Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können. Der generierte Code entspricht den Spalten in der Datenbanktabelle.

Dieses Tutorial konzentriert sich auf das Ändern der automatisch generierten Sichten, um die Darstellung zu verbessern.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Hinzufügen von Kursen zur Seite "Student Detail"
> * Vergewissern Sie sich, dass die Kurse der Seite hinzugefügt werden.

## <a name="prerequisites"></a>Voraussetzungen

* [Ändern der Datenbank](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Hinzufügen von Kursen zu Studenten Details

Der generierte Code bietet einen guten Ausgangspunkt für Ihre Anwendung, bietet jedoch nicht unbedingt die gesamte Funktionalität, die Sie in Ihrer Anwendung benötigen. Sie können den Code anpassen, um die speziellen Anforderungen Ihrer Anwendung zu erfüllen. Derzeit werden in der Anwendung die registrierten Kurse für den ausgewählten Studenten nicht angezeigt. In diesem Abschnitt fügen Sie der **Detail** Ansicht für die Schüler/Studenten die registrierten Kurse für jeden Schüler/Studenten hinzu.

Öffnen Sie **views** > **Students** > *Details. cshtml*. Fügen Sie unter dem letzten &lt;/DL&gt; Tag, aber vor dem schließenden &lt;/div-&gt;-Tag den folgenden Code hinzu.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Mit diesem Code wird eine Tabelle erstellt, in der eine Zeile für jeden Datensatz in der Registrierungs Tabelle für den ausgewählten Studenten angezeigt wird. Die **Anzeige** Methode rendert HTML für das-Objekt (mudelitem), das den Ausdruck darstellt. Sie verwenden die Anzeige Methode (anstatt einfach den Eigenschafts Wert in den Code einzubetten), um sicherzustellen, dass der Wert basierend auf dem Typ und der Vorlage für diesen Typ ordnungsgemäß formatiert ist. In diesem Beispiel gibt jeder Ausdruck eine einzelne Eigenschaft aus dem aktuellen Datensatz in der Schleife zurück, und die Werte sind primitive Typen, die als Text gerendert werden.

## <a name="confirm-courses-are-added"></a>Bestätigen, dass Kurse hinzugefügt werden

Führen Sie die Lösung aus. Klicken Sie auf **Liste der Schüler/Studenten** , und wählen Sie **Details** für einen der Studenten aus. Sie werden sehen, dass die registrierten Kurse in der Ansicht enthalten sind.

![Student mit Registrierung](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Hinzufügen von Kursen zur Seite "Student Detail"
> * Es wurde bestätigt, dass die Kurse der Seite hinzugefügt werden.

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Daten Anmerkungen hinzufügen, um Validierungsanforderungen anzugeben und die Formatierung anzuzeigen.
> [!div class="nextstepaction"]
> [Datenvalidierung verbessern](enhancing-data-validation.md)
