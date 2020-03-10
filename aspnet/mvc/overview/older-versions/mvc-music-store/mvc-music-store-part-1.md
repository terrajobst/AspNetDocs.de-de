---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Teil 1: Übersicht und Datei > Neues Projekt | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 1 umfasst Übersicht und Datei > Neues Projekt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451281"
---
# <a name="part-1-overview-and-file-new-project"></a>Teil 1: Übersicht und Datei > Neues Projekt

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 1 umfasst Übersicht und Datei&gt;neues Projekt.

## <a name="overview"></a>Übersicht

Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Web Developer für die Webentwicklung verwendet werden. Wir fangen langsam an, sodass die Webentwicklung für Einsteiger in Ordnung ist.

Die Anwendung, die wir aufbauen, ist ein einfacher Music Store. Es gibt drei Hauptkomponenten für die Anwendung: "Shopping", "Checkout" und "Administration".

![](mvc-music-store-part-1/_static/image1.jpg)

Besucher können Alben nach Genre durchsuchen:

![](mvc-music-store-part-1/_static/image2.jpg)

Sie können ein einzelnes Album anzeigen und dem Warenkorb hinzufügen:

![](mvc-music-store-part-1/_static/image3.jpg)

Sie können Ihren Warenkorb überprüfen und alle Elemente entfernen, die Sie nicht mehr benötigen:

![](mvc-music-store-part-1/_static/image4.jpg)

Wenn Sie das Auschecken fortsetzen, werden Sie zur Anmeldung oder Registrierung für ein Benutzerkonto aufgefordert.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Nachdem Sie ein Konto erstellt haben, können Sie die Bestellung vervollständigen, indem Sie Versand-und Zahlungsinformationen ausfüllen. Um dies zu gewährleisten, führen wir eine beeindruckende herauf Stufung durch: alles kostenlos, wenn Sie den Promotioncode "Free" eingeben!

![](mvc-music-store-part-1/_static/image5.jpg)

Nach der Bestellung wird ein einfacher Bestätigungsbildschirm angezeigt:

![](mvc-music-store-part-1/_static/image6.jpg)

Zusätzlich zu den kundenseitigen Seiten erstellen wir auch einen Administrator Abschnitt, der eine Liste der Alben anzeigt, von denen Administratoren Alben erstellen, bearbeiten und löschen können:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Datei&gt; neues Projekt

### <a name="installing-the-software"></a>Installieren der Software

In diesem Tutorial wird zunächst ein neues ASP.NET MVC 3-Projekt mit dem kostenlosen Visual Web Developer 2010 Express (kostenlos) erstellt. Anschließend fügen wir die Features inkrementell hinzu, um eine komplette funktionsfähige Anwendung zu erstellen. Auf diese Weise werden Datenbankzugriffe, Formular Bereitstellungs Szenarien, die Datenüberprüfung, die Verwendung von Masterseiten für konsistentes Seitenlayout, die Verwendung von AJAX für Seiten Aktualisierungen und Validierung, Benutzeranmeldung und mehr behandelt.

Sie können Schritt für Schritt ausführen, oder Sie können die abgeschlossene Anwendung aus [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store)herunterladen.

Sie können entweder Visual Studio 2010 SP1 oder Visual Web Developer 2010 Express SP1 (eine kostenlose Version von Visual Studio 2010) verwenden, um die Anwendung zu erstellen. Wir verwenden die SQL Server Compact (auch kostenlos) zum Hosten der Datenbank. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen.

- [Erforderliche Komponenten für Visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4,0]: einschließlich Laufzeit-und Tool Unterstützung

### <a name="creating-a-new-aspnet-mvc-3-project"></a>Erstellen eines neuen ASP.NET MVC 3-Projekts

Wir beginnen mit der Auswahl von "Neues Projekt" im Menü "Datei" in Visual Web Developer. Dadurch wird das Dialogfeld "Neues Projekt" geöffnet.

![](mvc-music-store-part-1/_static/image5.png)

Wählen Sie auf der linken C# Seite die Visual&gt;-Webvorlagen Gruppe aus, und wählen Sie dann die Vorlage "ASP.NET MVC 3-Webanwendung" in der mittleren Spalte aus. Nennen Sie Ihr Projekt mvcmusicstore, und klicken Sie auf die Schaltfläche OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Dadurch wird ein sekundäres Dialogfeld angezeigt, in dem wir einige MVC-spezifische Einstellungen für das Projekt erstellen können. Wählen Sie Folgendes aus:

Projektvorlage: Wählen Sie leer aus.

Engine anzeigen: Wählen Sie Razor aus.

HTML5-Semantik Markup aktivieren (aktiviert)

Stellen Sie sicher, dass Ihre Einstellungen wie unten dargestellt lauten, und klicken Sie dann auf die Schaltfläche OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Dadurch wird das Projekt erstellt. Werfen wir einen Blick auf die Ordner, die der Anwendung in der Projektmappen-Explorer auf der rechten Seite hinzugefügt wurden.

![](mvc-music-store-part-1/_static/image10.jpg)

Die leere MVC 3-Vorlage ist nicht vollständig leer – Sie fügt eine grundlegende Ordnerstruktur hinzu:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC nutzt einige grundlegende Benennungs Konventionen für Ordnernamen:

| **Ordner** | **Zweck** |
| --- | --- |
| **/Controllers** | Controller reagieren auf Eingaben aus dem Browser, entscheiden, was mit der Anwendung geschehen soll, und geben eine Antwort an den Benutzer zurück. |
| **/Views** | Ansichten enthalten unsere UI-Vorlagen |
| **/Models** | Modelle enthalten und bearbeiten Daten |
| **/Content** | Dieser Ordner enthält unsere Images, CSS und andere statische Inhalte. |
| **"/Scripts"** | Dieser Ordner enthält die JavaScript-Dateien. |

Diese Ordner sind auch in einer leeren ASP.NET MVC-Anwendung enthalten, da das ASP.NET-MVC-Framework standardmäßig eine Konvention für die Konfiguration verwendet und einige Standard Annahmen basierend auf den Benennungs Konventionen für Ordner vornimmt. Beispielsweise suchen Controller standardmäßig nach Ansichten im Ordner "Views", ohne dass Sie diese explizit in Ihrem Code angeben müssen. Wenn Sie mit den Standard Konventionen arbeiten, verringert sich der Code, den Sie schreiben müssen, und kann auch anderen Entwicklern das Verständnis ihres Projekts erleichtern. Diese Konventionen werden bei der Erstellung unserer Anwendung ausführlicher erläutert.

> [!div class="step-by-step"]
> [Weiter](mvc-music-store-part-2.md)
