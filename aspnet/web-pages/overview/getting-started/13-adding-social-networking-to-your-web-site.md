---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Hinzufügen von sozialen Netzwerken zu ASP.net Web Pages (Razor) Websites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie Ihre Website mit Diensten für soziale Netzwerke integrieren. In diesem Kapitel erfahren Sie, wie Sie es Benutzern ermöglichen, Ihre Website zu markieren/zu verknüpfen...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 1637464b0473bba8133acbbf8918d92b4f552701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422955"
---
# <a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Hinzufügen von sozialen Netzwerken zu ASP.net Web Pages Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> Dieser Artikel erläutert das Hinzufügen von Links zu sozialen Netzwerken für Facebook, Twitter, Reddit und Digg zu Seiten auf einer ASP.net Web Pages-Website (Razor) und zum Einschließen von Twitter-Feeds, Xbox-gamingkarten und Gravatar-Bildern.
> 
> Sie lernen Folgendes:
> 
> - Gewusst wie: zulassen, dass Benutzer Ihre Website markieren/verknüpfen.
> - Hinzufügen eines Twitter-Feeds.
> - Gewusst wie: Hinzufügen einer Facebook **like** -Schaltfläche zu Seiten
> - Gewusst wie: Rendering von Gravatar.com-Bildern
> - Anzeigen einer Xbox-Gamer-Karte auf Ihrer Website.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - ASP.net Web Helper Library (nuget-Paket)
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 3, mit Ausnahme von Teilen, die die ASP.net-webhilfsbibliothek verwenden.

<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Verknüpfen Ihrer Website auf Websites für soziale Netzwerke

Wenn Sie etwas an Ihrer Site haben, möchten Sie es häufig für Freunde freigeben. Sie können dies leicht machen, indem Sie Symbole (Symbole) anzeigen, auf die Benutzer klicken können, um eine Seite auf Digg-, Reddit-, Facebook-, Twitter-oder ähnlichen Websites freizugeben.

Um diese Symbole anzuzeigen, fügen Sie die `LinkSharecode`-Hilfsprogramm zu einer Seite hinzu. Personen, die Ihre Seite besuchen, können auf ein einzelnes Symbol klicken. Wenn Sie über ein Konto für diese soziale Netzwerk Website verfügen, können Sie einen Link zu Ihrer Seite auf dieser Website veröffentlichen.

![Abbildung 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.-Erstellen Sie eine Seite mit dem Namen *listlinkshare. cshtml* , und fügen Sie das folgende Markup hinzu:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    In diesem Beispiel wird der Seitentitel beim Ausführen des `LinkShare` Hilfsprogramms als Parameter übergeben, der wiederum den Seitentitel an die Social Networking-Website übergibt. Allerdings können Sie eine beliebige Zeichenfolge übergeben. In diesem Beispiel wird auch angegeben, welche Websites für soziale Netzwerke in der Liste enthalten sein sollen. Sie können die Websites für soziale Netzwerke angeben, die für Ihren Standort relevant sind.
2. Führen Sie die *listlinkshare. cshtml* -Seite in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.)
3. Klicken Sie auf ein Symbol für eine der Standorte, für die Sie sich registriert haben. Über den Link gelangen Sie zu der Seite des ausgewählten Standorts für soziale Netzwerke, auf der Sie einen Link freigeben können. Wenn Sie z. b. auf den Link Reddit klicken, gelangen Sie zur Seite `submit to reddit` auf der Reddit-Website.

     ![Abbildung 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Hinzufügen eines Twitter-Feeds

Informationen zur Verwendung eines Twitter-Hilfsprogramms, das mit der aktuellen Version der Twitter-API kompatibel ist, finden Sie unter [Twitter](../ui-layouts-and-themes/twitter-helper.md)-Hilfsprogramm. Dieses Beispiel zeigt, wie Sie Ihr eigenes Hilfsprogramm schreiben, damit Sie den Code aus vielen Seiten problemlos wieder verwenden können.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Anzeigen eines Facebook-&quot;wie&quot; Schaltfläche

In einigen Fällen besteht die beste Option darin, den Code direkt aus dem Social Network-Anbieter zu erhalten, anstatt sich auf ein Hilfsobjekt zu verlassen. Dies trifft vor allem dann zu, wenn der Anbieter des sozialen Netzwerks seine Optionen schneller aktualisiert, als das Hilfsprogramm aktualisiert wird.

Wenn Sie Ihrer Website Facebook-Features (z. b. die Schaltfläche like) hinzufügen möchten, können Sie Code Ausschnitte von der [Developers.Facebook.com](https://developers.facebook.com/) -Website abrufen. Auf der Facebook-Website verwenden Sie Ihre Tools, um einen Code Ausschnitt zu generieren, der für Ihre Website relevant ist.

Der folgende markierte Code ist der Code, der aus dem like-Schaltflächen Tool auf der Developers.Facebook.com-Website abgerufen wurde. Sie müssen ihre eigene APP-ID angeben.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Rendern eines Gravatar-Bilds

Ein *Gravatar* (ein &quot;Global erkannter Avatar&quot;) ist ein Image, das auf mehreren Websites als Ihr Avatar &#8212; verwendet werden kann. Dies ist ein Bild, das Sie darstellt. Beispielsweise kann ein Gravatar eine Person in einem Forumsbeitrag, in einem Blog Kommentar usw. identifizieren. (Sie können Ihr eigenes Gravatar auf der gravatar-Website unter [http://www.gravatar.com/](http://www.gravatar.com/)registrieren.) Wenn Sie Bilder neben Personennamen oder e-Mail-Adressen auf Ihrer Website anzeigen möchten, können Sie das Gravatar-Hilfsprogramm verwenden.

In diesem Beispiel verwenden Sie einen einzelnen Gravatar, der sich selbst repräsentiert. Eine andere Möglichkeit, einen Gravatar zu verwenden, besteht darin, dass die Benutzer ihre Gravatar-Adresse angeben können, wenn Sie sich auf Ihrer Website registrieren. (Sie erfahren, wie Sie es Benutzern ermöglichen können [, sich mit dem Hinzufügen von Sicherheit und Mitgliedschaft bei einer ASP.net Web Pages Site zu](https://go.microsoft.com/fwlink/?LinkId=202904)registrieren.) Wenn Sie dann Informationen für diesen Benutzer anzeigen, können Sie einfach den Gravatar hinzufügen, wo Sie den Benutzernamen anzeigen.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
2. Erstellen Sie eine neue Webseite namens *Gravatar. cshtml*.
3. Fügen Sie der Datei das folgende Markup hinzu: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Die `Gravatar.GetHtml`-Methode zeigt das Gravatar-Bild auf der Seite an. Um die Größe des Bilds zu ändern, können Sie eine Zahl als zweiten Parameter einschließen. Die Standardgröße ist 80. Zahlen kleiner als 80 machen das Bild kleiner. Zahlen größer als 80 machen das Bild größer.
4. Ersetzen Sie in den `Gravatar.GetHtml`-Methoden `<Your Gravatar account here>` durch die e-Mail-Adresse, die Sie für Ihr Gravatar-Konto verwenden. (Wenn Sie nicht über ein Gravatar-Konto verfügen, können Sie die e-Mail-Adresse eines Benutzers verwenden, der dies tut.)
5. Führen Sie die Seite in Ihrem Browser aus. Auf der Seite werden zwei Gravatar-Bilder für die angegebene e-Mail-Adresse angezeigt. Das zweite Bild ist kleiner als das erste. 

    ![Abbildung 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Anzeigen einer Xbox-Gamer-Karte

Wenn Benutzer Microsoft Xbox Games Online spielen, hat jeder Benutzer eine eindeutige ID. Statistiken werden für jeden Spieler in Form einer Spielerkarte aufbewahrt, die den Ruf, die Spieler Bewertung und die zuletzt gespielten Spiele anzeigt. Wenn Sie ein Xbox-Spieler sind, können Sie Ihre Spielerkarte auf Seiten auf Ihrer Website anzeigen, indem Sie das `GamerCard`-Hilfsprogramm verwenden.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
2. Erstellen Sie eine neue Seite mit dem Namen " *xboxgamer. cshtml* ", und fügen Sie das folgende Markup hinzu.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Mit der `GamerCard.GetHtml`-Eigenschaft geben Sie den Alias für die anzuzeigende Spielerkarte an.
3. Führen Sie die Seite in Ihrem Browser aus. Auf der Seite wird die von Ihnen angegebene Xbox Gamer-Karte angezeigt.

    ![Bild 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
