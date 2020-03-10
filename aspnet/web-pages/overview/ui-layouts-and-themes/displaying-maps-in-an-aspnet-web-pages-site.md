---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Anzeigen von Zuordnungen in einer ASP.net Web Pages (Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird erläutert, wie Sie interaktive Karten auf Seiten auf einer ASP.net Web Pages-Website (Razor-Website) auf der Grundlage von Zuordnungs Diensten anzeigen können
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518721"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Anzeigen von Zuordnungen in einer ASP.net Web Pages (Razor-) Site

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten auf einer ASP.net Web Pages-Website (Razor-Website) auf der Grundlage von Zuordnungs Diensten angezeigt werden, die von den von Ihnen, Google, Mapquest und
> 
> Sie lernen Folgendes:
> 
> - So generieren Sie eine Karte basierend auf einer Adresse
> - So generieren Sie eine Karte basierend auf breiten-und Längenkoordinaten.
> - Erfahren Sie, wie Sie ein Konto für die Verwendung von "wibmaps" registrieren und einen Schlüssel für die Verwendung mit "
> 
> Dies ist die im Artikel eingeführte ASP.net-Funktion:
> 
> - Das `Maps`-Hilfsprogramm.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Tutorial funktioniert auch mit webmatrix 3.

Auf Webseiten können Sie Maps auf einer Seite anzeigen, indem Sie `Maps`-Hilfsprogramm verwenden. Sie können Zuordnungen entweder auf einer Adresse oder in einem Satz von Längen-und breiten Koordinaten generieren. Mit der `Maps`-Klasse können Sie beliebte Karten Module wie z. a. z, Google, Mapquest und Yahoo aufzurufen.

Die Schritte zum Hinzufügen einer Zuordnung zu einer Seite sind identisch, unabhängig davon, welche der Zuordnungs-Engines Sie aufzurufen. Fügen Sie einfach einen JavaScript-Datei Verweis hinzu, der die verfügbaren Methoden zum Anzeigen der Zuordnung zur Verfügung stellt, und dann Methoden der `Maps`-Hilfsmethode aufrufen.

Sie wählen einen Kartendienst aus, der auf der von Ihnen verwendeten `Maps` Hilfsmethode basiert. Sie können Folgendes verwenden:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installieren der benötigten Teile

Um Maps anzuzeigen, benötigen Sie Folgendes:

- Das `Maps`-Hilfsprogramm. Dieses Hilfsprogramm ist in Version 2 der ASP.net-webhilfsprogramme-Bibliothek. Wenn Sie die Bibliothek nicht bereits hinzugefügt haben, können Sie Sie als nuget-Paket auf Ihrer Website installieren. Weitere Informationen finden Sie unter [Installieren von Hilfsprogrammen an einem ASP.net Web Pages-Standort](https://go.microsoft.com/fwlink/?LinkId=252372). (Suchen Sie im Katalog nach dem `microsoft-web-helpers` Paket.)
- Die jQuery-Bibliothek. Einige der webmatrix-Website Vorlagen enthalten bereits jQuery-Bibliotheken in Ihren *Skript* Ordnern. Wenn Sie nicht über diese Bibliotheken verfügen, können Sie die neueste jQuery-Bibliothek direkt von der [jQuery.org](http://jQuery.org) -Website herunterladen. Oder Sie können eine neue Website mit einer Vorlage erstellen (z. b. die Vorlage " **Starter Site** ") und dann die jQuery-Dateien von dieser Website auf die aktuelle Website kopieren.

Wenn Sie abschließend die Verwendung von "Get Maps" wünschen, müssen Sie zunächst ein (freies) Konto erstellen und einen Schlüssel erhalten. Um einen Schlüssel zu erhalten, führen Sie die folgenden Schritte aus:

1. Erstellen Sie ein Konto für das [Webkonto](https://www.microsoft.com/maps/developers/web.aspx)für die Entwicklung mit dem Konto " Sie müssen auch über eine Microsoft-Konto (Windows Live ID) verfügen.

    Sie können angeben, dass Sie den Schlüssel zum **Auswerten/testen**verwenden möchten. Wenn Sie die Zuordnungs Funktion auf Ihrem eigenen Computer mithilfe von webmatrix und IIS Express testen, wechseln Sie zum Arbeitsbereich **Website** , und notieren Sie sich die URL Ihrer Website (z. b. `http://localhost:50408`, auch wenn die Portnummer wahrscheinlich anders ist). Sie können diese *localhost* -Adresse als Website verwenden, wenn Sie registrieren.
2. Nachdem Sie sich für ein Konto registriert haben, navigieren Sie zum Konto Center für den Konto Center. Klicken Sie auf **Schlüssel erstellen oder anzeigen**:

    ![Zuordnung-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Notieren Sie den Schlüssel, den er erstellt.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Erstellen einer Karte basierend auf einer Adresse (mit Google)

Im folgenden Beispiel wird gezeigt, wie eine Seite erstellt wird, die eine Zuordnung auf der Grundlage einer Adresse rendert. Dieses Beispiel zeigt die Verwendung von Google Maps.

1. Erstellen Sie eine Datei namens " *mapaddress. cshtml* " im Stammverzeichnis der Website. Auf dieser Seite wird eine Karte basierend auf einer Adresse generiert, die Sie an Sie übergeben.
2. Kopieren Sie den folgenden Code in die Datei, und überschreiben Sie den vorhandenen Inhalt.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Beachten Sie die folgenden Funktionen der Seite:

    - Das `<script>` Element im `<head>` Element. Im Beispiel verweist das `<script>`-Element auf die *jQuery-1.6.4. min. js* -Datei. Dies ist eine minierte (komprimierte) Version der jQuery-Bibliothek, Version 1.6.4. Beachten Sie, dass bei der Referenz davon ausgegangen wird, dass sich die *js* -Datei im Ordner *Scripts* Ihrer Website befindet. 

        > [!NOTE]
        > Wenn Sie eine andere Version der jQuery-Bibliothek verwenden, stellen Sie sicher, dass Sie ordnungsgemäß auf diese Version verweisen.
    - Der aufzurufende `@Maps.GetGoogleHtml` im Text der Seite. Um eine Adresse zuzuordnen, müssen Sie eine Adress Zeichenfolge übergeben. Die Methoden für die anderen Zuordnungs-Engines funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml``@Maps.GetMapQuestHtml`).
3. Führen Sie die Seite aus und geben Sie eine Adresse ein. Auf der Seite wird eine auf Google Maps basierende Karte angezeigt, auf der der von Ihnen angegebene Speicherort angezeigt wird.

     ![Zuordnung-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Erstellen einer Karte basierend auf breiten-und Längenkoordinaten (mit der Verwendung von "")

In diesem Beispiel wird gezeigt, wie eine Zuordnung basierend auf Koordinaten erstellt wird. In diesem Beispiel wird gezeigt, wie Sie mit der Verwendung von "a. (Sie können eine Zuordnung auf der Grundlage von Koordinaten erstellen, indem Sie auch die anderen Zuordnungs-Engines verwenden, ohne eine-Taste zu verwenden.)

1. Erstellen Sie im Stammverzeichnis der Website eine Datei namens *mapkoordinaten. cshtml* , und ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und das Markup:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Ersetzen Sie `your-key-here` durch den zuvor generierten Schlüssel von "mit der Version".
3. Führen Sie die Seite *mapkoordinaten. cshtml* aus, geben Sie breiten-und Längenkoordinaten ein, und klicken Sie dann auf die **Karte** . . (Wenn Sie keine Koordinaten kennen, versuchen Sie Folgendes: Hierbei handelt es sich um einen Speicherort auf dem Microsoft Redmond-Campus.)

   - Breite: 47.6781005859375
   - Längengrad:-122.158317565918

     Die Seite wird mit den von Ihnen angegebenen Koordinaten angezeigt.

     ![Zuordnung-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Microsoft. Maps-API-Referenz](https://msdn.microsoft.com/library/gg427611.aspx)
