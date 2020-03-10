---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Anzeigen von Videos auf einer ASP.net Web Pages (Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Videos in einem ASP.net Web Pages mit Razor-Syntax Seite angezeigt werden.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510381"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Anzeigen von Videos auf einer ASP.net Web Pages-Website (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie einen Video Player (Media) auf einer ASP.net Web Pages-Website (Razor) verwenden, um Benutzern das Anzeigen von Videos zu gestatten, die auf der Website gespeichert sind. ASP.net Web Pages mit Razor-Syntax können Sie Flash-( *. SWF*), Media Player-( *. wmv*) und Silverlight-Videos ( *. xap*) abspielen.
> 
> Sie lernen Folgendes:
> 
> - Auswählen eines Video Players.
> - Gewusst wie: Hinzufügen von Videos zu einer Webseite
> - Festlegen von Video Player Attributen.
> 
> Dies sind die ASP.net Razor Pages-Funktionen, die im Artikel eingeführt wurden:
> 
> - Das `Video`-Hilfsprogramm.
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

## <a name="introduction"></a>Einführung

Möglicherweise möchten Sie ein Video auf Ihrer Website anzeigen. Eine Möglichkeit besteht darin, einen Link zu einer Website zu verwenden, die bereits über das Video verfügt, wie z. b. YouTube. Wenn Sie ein Video von diesen Websites direkt auf Ihren eigenen Seiten einbetten möchten, können Sie in der Regel ein HTML-Markup von der Website erhalten und es dann auf die Seite kopieren. Das folgende Beispiel zeigt beispielsweise, wie Sie ein YouTube-Video einbetten:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Wenn Sie ein Video abspielen möchten, das sich auf Ihrer eigenen Website befindet (nicht auf einer öffentlichen Video Freigabe Website), können Sie nicht direkt mit einem eingebetteten Markup wie diesem verknüpfen. Sie können jedoch Videos von Ihrer Website abspielen, indem Sie das `Video`-Hilfsprogramm verwenden, das einen Media Player direkt auf einer Seite rendert.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Auswählen eines Video Players

Es gibt viele Formate für Videodateien, und jedes Format erfordert in der Regel einen anderen Player und eine andere Möglichkeit zum Konfigurieren des Players. In ASP.net Razor Pages können Sie ein Video mit dem `Video`-Hilfsprogramm auf einer Webseite abspielen. Das `Video`-Hilfsprogramm vereinfacht das Einbetten von Videos in eine Webseite, da es automatisch die `object` und `embed` HTML-Elemente generiert, die normalerweise zum Hinzufügen von Videos zur Seite verwendet werden.

Das `Video`-Hilfsprogramm unterstützt die folgenden Medien Player:

- Adobe Flash
- Windows Media Player
- Microsoft Silverlight

### <a name="the-flash-player"></a>Der Flash Player

Der `Flash` Player des `Video`-Hilfsprogramms ermöglicht das Abspielen von Flash-Videos ( *. SWF* -Dateien) auf einer Webseite. Sie müssen mindestens einen Pfad zur Videodatei angeben. Wenn Sie nur den Pfad angeben, verwendet der Player Standardwerte, die von der aktuellen Version von Flash festgelegt werden. Typische Standardeinstellungen:

- Das Video wird unter Verwendung der Standardbreite und-Höhe und ohne Hintergrundfarbe angezeigt.
- Das Video wird automatisch abgespielt, wenn die Seite geladen wird.
- Das Video wird fortlaufend so lange Schleifen, bis es explizit beendet wird.
- Das Video wird so skaliert, dass es das gesamte Video anzeigt, anstatt das Video an eine bestimmte Größe anzupassen.
- Das Video wird in einem Fenster abgespielt.

### <a name="the-mediaplayer-player"></a>Der Media Player-Player

Mit dem `MediaPlayer` Player des `Video`-Hilfsprogramms können Sie Windows Media-Videos (*WMV* -Dateien), Windows Media-Audiodateien (*WMA* -Dateien) und MP3-Dateien (*MP3* -Dateien) auf einer Webseite abspielen. Sie müssen den Pfad der zu Wiedergabe enden Mediendatei einschließen. alle anderen Parameter sind optional. Wenn Sie nur einen Pfad angeben, verwendet der Spieler die Standardeinstellungen, die von der aktuellen Media Player-Version festgelegt werden, z. b.:

- Das Video wird unter Verwendung der Standardbreite und-Höhe angezeigt.
- Das Video wird automatisch abgespielt, wenn die Seite geladen wird.
- Das Video wird einmal abgespielt (keine Schleife).
- Der Player zeigt den vollständigen Satz von Steuerelementen in der Benutzeroberfläche an.
- Das Video wird in einem Fenster abgespielt.

### <a name="the-silverlight-player"></a>Der Silverlight-Player

Mit dem `Silverlight` Player des `Video`-Hilfsprogramms können Sie Windows Media Video (*WMV* -Dateien), Windows Media Audio ( *. WMA* -Dateien) und MP3-*Dateien (MP3-Dateien)* abspielen. Sie müssen den path-Parameter so festlegen, dass er auf ein Silverlight-basiertes Anwendungspaket ( *. xap* -Datei) verweist. Außerdem müssen Sie die Parameter "width" und "Height" festlegen. Alle anderen Parameter sind optional. Wenn Sie den Silverlight Player für Video verwenden und nur die erforderlichen Parameter festlegen, zeigt der Silverlight-Player das Video ohne Hintergrundfarbe an.

> [!NOTE]
> Für den Fall, dass Sie Silverlight nicht bereits kennen: die *XAP* -Datei ist eine komprimierte Datei, die Layoutanweisungen in einer *XAML* -Datei, verwalteten Code in Assemblys und optionale Ressourcen enthält. Sie können eine *XAP* -Datei in Visual Studio als Silverlight-Anwendungsprojekt erstellen.

Der `Silverlight` Video Player verwendet die Einstellungen, die Sie für den Player bereitstellen, und die Einstellungen, die in der *XAP* -Datei bereitgestellt werden.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME-Typen
> 
> Wenn ein Browser eine Datei herunterlädt, stellt der Browser sicher, dass der Dateityp mit dem MIME-Typ übereinstimmt, der für das gerenderte Dokument angegeben wird. Der MIME-Typ ist der Inhaltstyp oder Medientyp einer Datei. Das `Video`-Hilfsprogramm verwendet die folgenden MIME-Typen:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Abspielen von Flash-Videos (. swf)

In diesem Verfahren wird gezeigt, wie Sie ein Flash Video mit dem Namen *Sample. SWF*abspielen. Bei diesem Verfahren wird davon ausgegangen, dass Sie einen Ordner mit dem Namen " *Media* " auf Ihrer Site haben und sich die *. SWF* -Datei in diesem Ordner befindet.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.
2. Fügen Sie auf der Website eine Seite hinzu, und nennen Sie Sie " *Flash Video. cshtml*".
3. Fügen Sie der Seite das folgende Markup hinzu: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Führen Sie die Seite in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Die Seite wird angezeigt, und das Video wird automatisch abgespielt. 

    ![Klang](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")

Sie können den `quality`-Parameter für ein Flash Video auf `low`, `autolow`, `autohigh`, `medium`, `high`und `best`festlegen:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Mit dem `scale`-Parameter können Sie das Flash Video so ändern, dass es mit einer bestimmten Größe wiedergegeben wird, und Sie können Folgendes festlegen:

- [https://login.microsoftonline.com/consumers/](`showall`). Dadurch wird das gesamte Video sichtbar, während das ursprüngliche Seitenverhältnis beibehalten wird. Es kann jedoch sein, dass auf jeder Seite Grenzen angezeigt werden.
- [https://login.microsoftonline.com/consumers/](`noorder`). Dadurch wird das Video skaliert, während das ursprüngliche Seitenverhältnis beibehalten wird, es wird jedoch möglicherweise abgeschnitten.
- [https://login.microsoftonline.com/consumers/](`exactfit`). Dadurch wird das gesamte Video sichtbar, ohne dass das ursprüngliche Seitenverhältnis beibehalten wird, aber es kann zu einer Verzerrung kommen.

Wenn Sie keinen `scale` Parameter angeben, wird das gesamte Video angezeigt, und das ursprüngliche Seitenverhältnis wird ohne Zuschneiden beibehalten. Im folgenden Beispiel wird gezeigt, wie der `scale`-Parameter verwendet wird:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Der Flash Player unterstützt eine videomoduseinstellung mit dem Namen `windowMode`. Sie können dies auf `window`, `opaque`und `transparent`festlegen. Standardmäßig ist die `windowMode` auf `window`festgelegt, wodurch das Video in einem separaten Fenster auf der Webseite angezeigt wird. Die `opaque` Einstellung blendet alles hinter dem Video auf der Webseite aus. Mit der Einstellung "`transparent`" kann der Hintergrund der Webseite durch das Video angezeigt werden, wenn ein beliebiger Teil des Videos transparent ist.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Abspielen von Media Player-Videos ( *. wmv*)

Im folgenden Verfahren wird gezeigt, wie Sie ein Windows Media-Video namens *Sample. wmv* abspielen, das sich im Ordner " *Media* " befindet.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
2. Erstellen Sie eine neue Seite mit dem Namen *mediaplayervideo. cshtml*.
3. Fügen Sie der Seite das folgende Markup hinzu: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Führen Sie die Seite in einem Browser aus. Das Video wird automatisch geladen und abgespielt. 

    ![Klang](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")

Sie können `playCount` auf eine ganze Zahl festlegen, die angibt, wie oft das Video automatisch abgespielt werden soll:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Mit dem `uiMode`-Parameter können Sie angeben, welche Steuerelemente in der Benutzeroberfläche angezeigt werden. Sie können `uiMode` auf `invisible`, `none`, `mini`oder `full`festlegen. Wenn Sie keinen `uiMode` Parameter angeben, wird das Video zusätzlich zum Videofenster mit dem Statusfenster, der Suchleiste, den Steuerungs Schaltflächen und den volumesteuerelementen angezeigt. Diese Steuerelemente werden auch angezeigt, wenn Sie mit dem Player eine Audiodatei abspielen. Im folgenden finden Sie ein Beispiel für die Verwendung des `uiMode`-Parameters:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Standardmäßig ist die Audiodatei aktiviert, wenn das Video wiedergegeben wird. Sie können das audiostumm schalten, indem Sie den `mute`-Parameter auf "true" festlegen:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Sie können die Audioebene des Media Player-Videos steuern, indem Sie den `volume`-Parameter auf einen Wert zwischen 0 und 100 festlegen. Der Standardwert lautet "50". Im Folgenden ein Beispiel:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Abspielen von Silverlight-Videos

In diesem Verfahren wird gezeigt, wie Sie Videos abspielen, die auf der Seite "Silverlight *. xap* " in einem Ordner mit dem Namen " *Media*" enthalten sind.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
2. Erstellen Sie eine neue Seite mit dem Namen " *silverlightvideo. cshtml*".
3. Fügen Sie der Seite das folgende Markup hinzu: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Führen Sie die Seite in einem Browser aus. 

    ![Klang](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash-Objekt und Einbettungs Tagattribute](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK-Parameter Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
