---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter-Hilfsprogramm mit ASP.net Web Pages | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Thema und dieser Anwendung wird gezeigt, wie Sie Ihrem webmatrix 3-Projekt ein Twitter-Hilfsprogramm hinzufügen. Sie enthält den Twitter-hilfsscode und zeigt, wie das Hilfsprogramm aufgerufen wird...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518619"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Twitter-Hilfsprogramm mit ASP.NET Web Pages

von [Tom fitzmacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Twitter-Hilfsprogramme sind veraltet. Informationen zu den neuesten Engagement-Tools für Twitter finden Sie unter [Übersicht über Twitter für Websites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> In diesem Thema und dieser Anwendung wird gezeigt, wie Sie Ihrem webmatrix 3-Projekt ein Twitter-Hilfsprogramm hinzufügen. Sie enthält den Twitter-hilfsscode und zeigt, wie die Hilfsmethoden aufgerufen werden.
> 
> Dieser Code für die Twitter. cshtml-Datei wurde von der **Tian Pan** von Microsoft entwickelt.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

## <a name="introduction"></a>Einführung

In diesem Thema wird veranschaulicht, wie Sie Ihrer Anwendung ein Twitter-Hilfsprogramm hinzufügen und Razor-Syntax verwenden, um die Hilfsmethoden aufzurufen. Mit dem Twitter-Hilfsprogramm können Sie ganz einfach Twitter-Schaltflächen und Widgets in Ihre Anwendung integrieren. Wenn Sie ein Twitter-Widget verwenden möchten, z. b. die Zeitachse eines Benutzers oder die Suchergebnisse für ein hashtag, müssen Sie zuerst das [Widget auf Twitter](https://twitter.com/settings/widgets)erstellen. Nachdem Sie Ihr Widget erstellt haben, erhalten Sie eine Widget-ID. Sie übergeben diese Widget-ID als Parameter, wenn Sie die Hilfsmethoden aufrufen, die das Widget anzeigen.

Dieses Thema wurde für Version 1,1 der Twitter-API geschrieben. Wenn Sie den Twitter-Hilfscode direkt zu Ihrem Projekt hinzufügen, können Sie den hilfsscode aktualisieren, wenn sich die Twitter-API ändert.

Weitere Informationen zum Installieren von webmatrix finden Sie unter [Introducing ASP.net Web Pages 2-Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Twitter-Hilfsprogramm zu Ihrem Projekt hinzufügen

Fügen Sie zum Hinzufügen des Twitter-Hilfsprogramms zunächst dem Projekt einen Ordner mit dem Namen **App\_Code** hinzu. Erstellen Sie dann eine Datei mit dem Namen **Twitter. cshtml**.

![App_Code Ordner](twitter-helper/_static/image1.png)

Ersetzen Sie den Standard Code in Twitter. cshtml durch den folgenden Code.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Twitter-Methoden von ihren Webseiten aus anrufen

Im folgenden Beispiel wird gezeigt, wie Sie die Twitter-Hilfsmethoden von einer Seite in Ihrem Projekt verwenden können. In Ihrem Projekt möchten Sie die Parameterwerte durch Werte ersetzen, die für Ihre Anforderungen relevant sind. Sie können die bereitgestellten Widget-IDs verwenden, um zu untersuchen, wie die Methoden funktionieren, aber Sie möchten Ihre eigenen Widgets für Ihr Projekt generieren.

Nicht alle unten gezeigten Parameter sind erforderlich. Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird. Beispielsweise muss auf die Schaltfläche "nachverfolgen" nur der Benutzername befolgt werden, aber im Beispiel wird gezeigt, wie Sie die Anzahl von Followern einschließen und wie Sie die Größe der Schaltfläche und der Sprache angeben.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Anzeigen der Ergebnisse

Der obige Code erzeugt die folgenden Schaltflächen und Widgets. Diese Schaltflächen und Widgets sind voll funktionsfähig, keine Screenshots. Die Schaltfläche Nachverfolgung wird auf Spanisch angezeigt, da der sprach Parameter auf **es**festgelegt wurde.

### <a name="follow-button"></a>Schaltfläche "folgen

[@aspnetfolgen)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Tweet-Schaltfläche

[Tweet](https://twitter.com/share) -`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Benutzer Zeitachse (Profil)

[Tweets nach @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoriten

Die [bevorzugten tweets nach @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Liste

[Tweets von @Microsoft/MS\_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Suche

[Tweets zu &quot;#ASP .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
