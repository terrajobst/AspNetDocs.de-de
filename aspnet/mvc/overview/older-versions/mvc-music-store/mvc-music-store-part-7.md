---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Teil 7: Mitgliedschaft und Autorisierung | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 7 werden die Mitgliedschaft und die Autorisierung behandelt.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433467"
---
# <a name="part-7-membership-and-authorization"></a>Teil 7: Mitgliedschaft und Autorisierung

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 7 werden die Mitgliedschaft und die Autorisierung behandelt.

Der Store Manager-Controller ist zurzeit für alle Benutzer zugänglich, die unsere Website besuchen. Ändern wir dies, um die Berechtigung für Website Administratoren einzuschränken.

## <a name="adding-the-accountcontroller-and-views"></a>Hinzufügen der AccountController-und-Sichten

Ein Unterschied zwischen der vollständigen ASP.NET MVC 3-Webanwendungs Vorlage und der Vorlage für eine leere ASP.NET MVC 3-Webanwendung besteht darin, dass die leere Vorlage keinen Konto Controller enthält. Wir fügen einen Konto Controller hinzu, indem wir einige Dateien aus einer neuen ASP.NET MVC-Anwendung kopieren, die aus der Vorlage Full ASP.NET MVC 3-Webanwendung erstellt wurde.

Erstellen Sie eine neue ASP.NET MVC-Anwendung mithilfe der Vorlage Full ASP.NET MVC 3-Webanwendung, und kopieren Sie die folgenden Dateien in die gleichen Verzeichnisse in unserem Projekt:

1. Kopieren Sie AccountController.cs in das Verzeichnis Controller.
2. Accountmodels in das Modell Verzeichnis kopieren
3. Erstellen Sie im Verzeichnis "Views" ein Konto Verzeichnis, und kopieren Sie alle vier Sichten in

Ändern Sie den Namespace für die Controller-und Modellklassen, damit Sie mit dem mvcmusicstore beginnen. Die AccountController-Klasse sollte den mvcmusicstore. Controllers-Namespace verwenden, und die accountmodels-Klasse sollte den mvcmusicstore. Models-Namespace verwenden.

*Hinweis: diese Dateien sind auch im Download MvcMusicStore-Assets. zip verfügbar, von dem aus wir unsere Website Entwurfs Dateien zu Beginn des Tutorials kopiert haben. Die Mitgliedschafts Dateien befinden sich im Code Verzeichnis.*

Die aktualisierte Lösung sollte wie folgt aussehen:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Hinzufügen eines Administrators mit der ASP.NET-Konfigurations Website

Bevor wir auf unserer Website eine Autorisierung benötigen, müssen wir einen Benutzer mit Zugriff erstellen. Die einfachste Möglichkeit zum Erstellen eines Benutzers besteht darin, die integrierte ASP.NET-Konfigurations Website zu verwenden.

Starten Sie die ASP.NET-Konfigurations Website, indem Sie auf das Symbol im Projektmappen-Explorer klicken.

![](mvc-music-store-part-7/_static/image2.png)

Dadurch wird eine Konfigurations Website gestartet. Klicken Sie im Startbildschirm auf die Registerkarte Sicherheit, und klicken Sie dann auf den Link "Rollen aktivieren" in der Mitte des Bildschirms.

![](mvc-music-store-part-7/_static/image3.png)

Klicken Sie auf den Link "Rollen erstellen oder verwalten".

![](mvc-music-store-part-7/_static/image4.png)

Geben Sie "Administrator" als Rollenname ein, und klicken Sie auf die Schaltfläche Rolle hinzufügen.

![](mvc-music-store-part-7/_static/image5.png)

Klicken Sie auf die Schaltfläche zurück, und klicken Sie auf der linken Seite auf den Link Benutzer erstellen.

![](mvc-music-store-part-7/_static/image6.png)

Füllen Sie die Felder für die Benutzerinformationen auf der linken Seite mit den folgenden Informationen aus:

| **Feld** | **Wert** |
| --- | --- |
| **Benutzername** | Administrator |
| **Kennwort** | password123! |
| **Kennwort bestätigen** | password123! |
| **E-Mail** | (eine beliebige e-Mail-Adresse funktioniert) |
| **Sicherheitsfrage** | (je nachdem, was Sie möchten) |
| **Sicherheitsantwort** | (je nachdem, was Sie möchten) |

*Hinweis: Sie können natürlich auch ein beliebiges Kennwort verwenden. Die Standardeinstellungen für die Kenn Wort Sicherheit erfordern ein Kennwort mit einer Länge von 7 Zeichen und ein nicht alphanumerisches Zeichen.*

Wählen Sie die Administrator Rolle für diesen Benutzer aus, und klicken Sie auf die Schaltfläche Benutzer erstellen.

![](mvc-music-store-part-7/_static/image7.png)

An diesem Punkt sollte eine Meldung angezeigt werden, die besagt, dass der Benutzer erfolgreich erstellt wurde.

![](mvc-music-store-part-7/_static/image8.png)

Sie können das Browserfenster jetzt schließen.

## <a name="role-based-authorization"></a>Rollenbasierte Autorisierung

Nun können wir den Zugriff auf den storemanagercontroller mithilfe des Attributs [autorisieren] einschränken und angeben, dass der Benutzer in der Administrator Rolle sein muss, um auf eine beliebige Controller Aktion in der Klasse zugreifen zu können.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Hinweis: das Attribut [autorisieren] kann auf bestimmte Aktionsmethoden und auf Controller Klassenebene platziert werden.*

Wenn Sie jetzt zu/StoreManager navigieren, wird ein Dialogfeld für die Anmeldung angezeigt:

![](mvc-music-store-part-7/_static/image9.png)

Nachdem Sie sich mit unserem neuen Administrator Konto angemeldet haben, können wir wie zuvor zum Bildschirm zum Bearbeiten des Albums wechseln.

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-6.md)
> [Weiter](mvc-music-store-part-8.md)
