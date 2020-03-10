---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Aufrufen der Web-API aus einer Windows Phone 8C#-Anwendung ()-ASP.NET 4. x
author: rmcmurray
description: 'Tutorial mit Code: Erstellen einer ASP.net-Web-API-Anwendung in ASP.NET 4. x, die einen Katalog von Büchern für eine Windows Phone 8-Anwendung bereitstellt.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498213"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Aufrufen des Web-API über eine Windows Phone 8-Anwendung (C#)

von [Robert McMurray](https://github.com/rmcmurray)

In diesem Tutorial erfahren Sie, wie Sie ein umfassendes End-to-End-Szenario erstellen, das aus einer ASP.net-Web-API Anwendung besteht, die einen Katalog von Büchern für eine Windows Phone 8-Anwendung bereitstellt.

### <a name="overview"></a>Übersicht

Rest-Dienste wie ASP.net-Web-API vereinfachen die Erstellung von http-basierten Anwendungen für Entwickler durch die Abstraktion der Architektur für serverseitige und Client seitige Anwendungen. Anstatt ein proprietäres, socketbasiertes Protokoll für die Kommunikation zu erstellen, müssen Web-API-Entwickler einfach die erforderlichen HTTP-Methoden für Ihre Anwendung veröffentlichen (z. b. "Get", "Post", "Put", "Delete"), und Entwickler von Client Anwendungen müssen nur die HTTP-Methoden, die für Ihre Anwendung erforderlich sind.

In diesem End-to-End-Tutorial erfahren Sie, wie Sie die Web-API verwenden, um die folgenden Projekte zu erstellen:

- Im [ersten Teil dieses Tutorials](#STEP1)erstellen Sie eine ASP.net-Web-API Anwendung, die alle Vorgänge zum Erstellen, lesen, aktualisieren und löschen (CRUD) zum Verwalten eines Buch Katalogs unterstützt. Diese Anwendung verwendet die [XML-Beispieldatei (Books. Xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) von MSDN.
- Im [zweiten Teil dieses Tutorials](#STEP2)erstellen Sie eine interaktive Windows Phone 8-Anwendung, mit der die Daten aus Ihrer Web-API-Anwendung abgerufen werden.

#### <a name="prerequisites"></a>Voraussetzungen

- Visual Studio 2013 mit installiertem Windows Phone 8 SDK
- Windows 8 oder höher auf einem 64-Bit-System mit installiertem Hyper-V
- Eine Liste der zusätzlichen Anforderungen finden Sie im Abschnitt *System Anforderungen* auf der [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) -Downloadseite.

> [!NOTE]
> Wenn Sie die Konnektivität zwischen Web-API und Windows Phone 8-Projekten auf Ihrem lokalen System testen möchten, müssen Sie die Anweisungen im Artikel *[Verbinden des Windows Phone 8-Emulators mit Web-API-Anwendungen auf einem lokalen Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* befolgen, um Ihre Testumgebung einzurichten.

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Schritt 1: Erstellen des Web-API-buchbuch Projekts

Der erste Schritt in diesem End-to-End-Tutorial besteht darin, ein Web-API-Projekt zu erstellen, das alle CRUD-Vorgänge unterstützt. Beachten Sie, dass Sie der Projekt Mappe in [Schritt 2](#STEP2) dieses Tutorials das Windows Phone-Anwendungsprojekt hinzufügen.

1. Öffnen Sie **Visual Studio 2013**.
2. Klicken Sie auf **Datei**, dann auf **neu**und dann auf **Projekt**.
3. Wenn das Dialogfeld **Neues Projekt** angezeigt wird, erweitern Sie **installiert**, dann **Vorlagen**, dann **Visual C#** und dann **Web**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Zum Erweitern auf das Bild klicken                                                                |

4. Markieren Sie die **ASP.NET-Webanwendung**, geben Sie **Bookstore** als Projektnamen ein, und klicken Sie dann auf **OK**.
5. Wenn das Dialogfeld **Neues ASP.net-Projekt** angezeigt wird, wählen Sie die Vorlage **Web-API** aus, und klicken Sie dann auf **OK**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Zum Erweitern auf das Bild klicken                                                                |

6. Wenn das Web-API-Projekt geöffnet wird, entfernen Sie den Beispiel Controller aus dem Projekt:

    1. Erweitern Sie im Projektmappen-Explorer den Ordner **Controller** .
    2. Klicken Sie mit der rechten Maustaste auf die Datei **ValuesController.cs** , und klicken Sie dann auf **Löschen**.
    3. Klicken Sie bei Aufforderung auf **OK** , um den Löschvorgang zu bestätigen.
7. Fügen Sie dem Web-API-Projekt eine XML-Datendatei hinzu. Diese Datei enthält den Inhalt des bookstore-Katalogs:

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **App\_Data** , und klicken Sie dann auf **Hinzufügen**und dann auf **Neues Element**.
   2. Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, markieren Sie die Vorlage **XML-Datei** .
   3. Benennen Sie die Datei **Books. XML**, und klicken Sie dann auf **Hinzufügen**.
   4. Wenn die Datei **Books. XML** geöffnet ist, ersetzen Sie den Code in der Datei durch den XML-Code aus der Datei "Sample **Books. XML** " auf MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Speichern und schließen Sie die XML-Datei.

8. Fügen Sie dem Web-API-Projekt das bookstore-Modell hinzu. Dieses Modell enthält die CREATE-, Read-, Update-und DELETE-Logik (CRUD) für die Bookstore-Anwendung:

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Modelle** , klicken Sie auf **Hinzufügen**und dann auf **Klasse**.
   2. Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **Hinzufügen**.
   3. Wenn die Datei **BookDetails.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Speichern und schließen Sie die Datei **BookDetails.cs** .

9. Fügen Sie den Bookstore-Controller dem Web-API-Projekt hinzu:

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Controller** , klicken Sie auf **Hinzufügen**und dann auf **Controller**.
   2. Wenn das Dialogfeld **Gerüst hinzufügen** angezeigt wird, markieren Sie **Web API 2 Controller-Empty**, und klicken Sie dann auf **Hinzufügen**.
   3. Wenn das Dialogfeld **Controller hinzufügen** angezeigt wird, benennen Sie den Controller **bookscontroller**, und klicken Sie dann auf **Hinzufügen**.
   4. Wenn die Datei **BooksController.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Speichern und schließen Sie die Datei **BooksController.cs** .

10. Erstellen Sie die Web-API-Anwendung, um nach Fehlern zu suchen.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Schritt 2: Hinzufügen des Windows Phone 8 Bookstore Catalog-Projekts

Der nächste Schritt dieses End-to-End-Szenarios besteht darin, die Katalog Anwendung für Windows Phone 8 zu erstellen. Diese Anwendung verwendet die Windows Phone Daten gebundene *App* -Vorlage für die Standardbenutzer Oberfläche und verwendet die Web-API-Anwendung, die Sie in [Schritt 1](#STEP1) dieses Tutorials als Datenquelle erstellt haben.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die **Bookstore** -Projekt Mappe, und klicken Sie dann auf **Hinzufügen**und **Neues Projekt**.
2. Wenn das Dialogfeld **Neues Projekt** angezeigt wird, erweitern Sie die Option **installiert**und dann **Visualisierung C#** , und klicken Sie dann auf **Windows Phone**.
3. Markieren Sie **Windows Phone Daten gebundene App**, geben Sie **bookcatalog** als Name ein, und klicken Sie dann auf **OK**.
4. Fügen Sie das nuget-Paket JSON.net dem **bookcatalog** -Projekt hinzu:

    1. Klicken Sie **im Projekt** Mappen-Explorer mit der rechten Maustaste auf **Verweise** , und klicken Sie dann auf **nuget-Pakete verwalten**.
    2. Wenn das Dialogfeld **nuget-Pakete verwalten** angezeigt wird, erweitern Sie den Abschnitt **Online** , und markieren Sie **nuget.org**.
    3. Geben Sie **JSON.net** in das Suchfeld ein, und klicken Sie auf das Suchsymbol.
    4. Markieren Sie **JSON.net** in den Suchergebnissen, und klicken Sie dann auf **Installieren**.
    5. Klicken Sie nach Abschluss der Installation auf **Schließen**.
5. Fügen Sie dem **bookcatalog** -Projekt das **BookDetails** -Modell hinzu. Diese enthält ein generisches Modell der Bookstore-Klasse:

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **bookcatalog** , klicken Sie dann auf **Hinzufügen**und dann auf **neuer Ordner**.
   2. Benennen Sie die neuen Ordner **Modelle**.
   3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Modelle** , klicken Sie auf **Hinzufügen**und dann auf **Klasse**.
   4. Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **Hinzufügen**.
   5. Wenn die Datei **BookDetails.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Speichern und schließen Sie die Datei **BookDetails.cs** .

6. Aktualisieren Sie die **MainViewModel.cs** -Klasse, um die Funktionen für die Kommunikation mit der Web-API-Anwendung der Buchhandlung einzubeziehen

   1. Erweitern Sie im Projektmappen-Explorer den Ordner **ViewModels** , und doppelklicken Sie dann auf die Datei **MainViewModel.cs** .
   2. Wenn die Datei **MainViewModel.cs** geöffnet ist, ersetzen Sie den Code in der Datei durch Folgendes: Beachten Sie, dass Sie den Wert der `apiUrl`-Konstante mit der tatsächlichen URL Ihrer Web-API aktualisieren müssen: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Speichern und schließen Sie die Datei **MainViewModel.cs** .

7. Aktualisieren Sie die Datei " **MainPage. XAML** ", um den Anwendungsnamen anzupassen:

   1. Doppelklicken Sie im Projektmappen-Explorer auf die Datei " **MainPage. XAML** ".
   2. Wenn die Datei " **MainPage. XAML** " geöffnet ist, suchen Sie nach den folgenden Codezeilen: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Ersetzen Sie diese Zeilen durch Folgendes: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Speichern und schließen Sie die Datei " **MainPage. XAML** ".

8. Aktualisieren Sie die Datei " **detailspage. XAML** ", um die angezeigten Elemente anzupassen:

   1. Doppelklicken Sie im Projektmappen-Explorer auf die Datei **detailspage. XAML** .
   2. Wenn die Datei " **detailspage. XAML** " geöffnet ist, suchen Sie nach den folgenden Codezeilen: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Ersetzen Sie diese Zeilen durch Folgendes: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Speichern und schließen Sie die Datei **detailspage. XAML** .

9. Erstellen Sie die Windows Phone Anwendung, um nach Fehlern zu suchen.

### <a name="step-3-testing-the-end-to-end-solution"></a>Schritt 3: Testen der End-to-End-Lösung

Wie im Abschnitt " *Voraussetzungen* " dieses Tutorials erwähnt, müssen Sie beim Testen der Konnektivität zwischen Web-API und Windows Phone 8-Projekten auf Ihrem lokalen System die Anweisungen im Artikel *[Herstellen einer Verbindung zwischen dem Windows Phone 8-Emulator und Web-API-Anwendungen auf einem lokalen Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* befolgen, um Ihre Testumgebung einzurichten.

Nachdem Sie die Testumgebung konfiguriert haben, müssen Sie die Windows Phone Anwendung als Startprojekt festlegen. Markieren Sie hierzu die **bookcatalog** -Anwendung im Projektmappen-Explorer, und klicken Sie dann auf **als Startprojekt festlegen**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Zum Erweitern auf das Bild klicken |

Wenn Sie F5 drücken, startet Visual Studio sowohl den Windows Phone Emulator, der eine &quot;die Sie&quot; Meldung anzeigen, während die Anwendungsdaten von Ihrer Web-API abgerufen werden:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Zum Erweitern auf das Bild klicken |

Wenn alles erfolgreich ist, sollte der Katalog angezeigt werden:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Zum Erweitern auf das Bild klicken |

Wenn Sie auf einen Buchtitel tippen, wird die Buchbeschreibung in der Anwendung angezeigt:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Zum Erweitern auf das Bild klicken |

Wenn die Anwendung nicht mit Ihrer Web-API kommunizieren kann, wird eine Fehlermeldung angezeigt:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Zum Erweitern auf das Bild klicken |

Wenn Sie auf die Fehlermeldung tippen, werden weitere Details zum Fehler angezeigt:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Zum Erweitern auf das Bild klicken                                                                 |
