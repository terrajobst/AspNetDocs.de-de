---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Anmelden mithilfe externer Websites auf einer ASP.net Web Pages-(Razor-) Website | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird erläutert, wie Sie sich mit Facebook, Google, Twitter, Yahoo und anderen Websites bei Ihrer ASP.net Web Pages (Razor-Website) anmelden, –...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518787"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Anmelden mithilfe externer Websites auf einer ASP.net Web Pages-(Razor-) Website

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie sich mit Facebook, Google, Twitter, Yahoo und anderen Websites bei Ihrer ASP.net Web Pages (Razor-Website) anmelden, d. –., wie OAuth und OpenID auf Ihrer Website unterstützt werden.
> 
> Sie lernen Folgendes:
> 
> - Aktivieren der Anmeldung von anderen Websites, wenn Sie die webmatrix-Starter Site-Vorlage verwenden.
> 
> Dies ist die im Artikel eingeführte ASP.net-Funktion:
> 
> - Das `OAuthWebSecurity`-Hilfsprogramm.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - Webmatrix 3

ASP.net Web Pages bietet Unterstützung für [OAuth](http://oauth.net/) -und [OpenID](http://openid.net/) -Anbieter. Mithilfe dieser Anbieter können Sie es Benutzern ermöglichen, sich mit Ihren vorhandenen Anmelde Informationen von Facebook, Twitter, Microsoft und Google bei Ihrer Website anzumelden. Wenn Sie sich beispielsweise mit einem Facebook-Konto anmelden möchten, können Benutzer einfach ein Facebook-Symbol auswählen, das Sie zur Facebook-Anmeldeseite umleitet, auf der Sie Ihre Benutzerinformationen eingeben. Anschließend können Sie die Facebook-Anmeldung mit Ihrem Konto auf Ihrer Website verknüpfen. Eine verwandte Erweiterung der Webseiten-Mitgliedschafts Features besteht darin, dass Benutzer mehrere Anmeldungen (einschließlich Anmeldungen von Websites für soziale Netzwerke) mit einem einzigen Konto auf Ihrer Website verknüpfen können.

Diese Abbildung zeigt die Anmeldeseite aus der **Starter Site** -Vorlage, in der ein Benutzer ein Facebook-, Twitter-, Google-oder Microsoft-Symbol auswählen kann, um die Anmeldung mit einem externen Konto zu aktivieren:

![externe Anbieter](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Sie können die OAuth-und OpenID-Mitgliedschaft aktivieren, indem Sie einige Codezeilen in der Vorlage " **Starter Site** " auskommentieren. Die Methoden und Eigenschaften, die Sie für die Arbeit mit den OAuth-und OpenID-Anbietern verwenden, befinden sich in der `WebMatrix.Security.OAuthWebSecurity`-Klasse. Die **Starter Site** -Vorlage enthält eine vollständige Mitgliedschafts Infrastruktur, vollständig mit einer Anmeldeseite, einer Mitgliedschafts Datenbank und dem gesamten Code, den Sie zum Anmelden von Benutzern bei Ihrer Website mithilfe lokaler Anmelde Informationen oder eines anderen Standorts benötigen.

In diesem Abschnitt wird erläutert, wie Sie es Benutzern ermöglichen können, sich von externen Websites an eine Website anzumelden, die auf der Vorlage **Starter Site** basiert. Nachdem Sie eine Starter Site erstellt haben, gehen Sie wie folgt vor (Details finden Sie hier):

- Für Standorte, die einen OAuth-Anbieter verwenden (Facebook, Twitter und Microsoft), erstellen Sie eine Anwendung auf der externen Website. Dadurch erhalten Sie Anwendungs Schlüssel, die Sie benötigen, um die Anmelde Funktion für diese Sites aufzurufen.
- Für Websites, die einen OpenID-Anbieter (Google) verwenden, ist es nicht erforderlich, eine Anwendung zu erstellen. Für alle diese Websites müssen Sie über ein Konto verfügen, um sich anzumelden und Entwickler Anwendungen zu erstellen.

    > [!NOTE]
    > Microsoft-Anwendungen akzeptieren nur eine Live-URL für eine funktionierende Website, sodass Sie keine lokale Website-URL zum Testen von Anmeldungen verwenden können.
- Bearbeiten Sie einige Dateien auf Ihrer Website, um den entsprechenden Authentifizierungs Anbieter anzugeben und einen Anmelde Namen an die Website zu übermitteln, die Sie verwenden möchten.

Dieser Artikel enthält separate Anweisungen für die folgenden Aufgaben:

- [Aktivieren von Google-Anmeldungen](#To_enable_Google_logins)
- [Aktivieren von Facebook-Anmeldungen](#To_enable_Facebook_logins)
- [Aktivieren von Twitter-Anmeldungen](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Aktivieren von Google-Anmeldungen

1. Erstellen oder öffnen Sie eine ASP.net Web Pages Site, die auf der webmatrix-Starter Site-Vorlage basiert.
2. Öffnen Sie die Seite *\_AppStart. cshtml* , und heben Sie die Auskommentierung der folgenden Codezeile auf. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testen der Google-Anmeldung

1. Führen Sie die Seite *default. cshtml* Ihrer Website aus, und klicken Sie auf die Schaltfläche **Anmelden** .
2. Wählen Sie auf der *Anmelde* Seite im Abschnitt **Verwenden eines anderen Dienstanbieter für** die Anmeldung entweder die **Google** -oder **Yahoo** -Schaltfläche "Senden" aus. In diesem Beispiel wird die Google-Anmeldung verwendet. 

    Die Webseite leitet die Anforderung an die Google-Anmeldeseite um.

    ![Google-Anmeldung](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Anmelde Informationen für ein vorhandenes Google-Konto eingeben.
4. Wenn Sie gefragt werden, ob Sie *localhost* die Verwendung von Informationen aus dem Konto gestatten möchten, klicken Sie auf **zulassen**.

    Der Code verwendet das Google-Token zum Authentifizieren des Benutzers und kehrt dann zu dieser Seite auf Ihrer Website zurück. Auf dieser Seite können Benutzer Ihre Google-Anmeldung mit einem vorhandenen Konto auf Ihrer Website verknüpfen, oder Sie können ein neues Konto auf Ihrer Website registrieren, um die externe Anmeldung zuzuordnen.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Wählen Sie die Schaltfläche **zuordnen** aus. Der Browser kehrt zur Startseite Ihrer Anwendung zurück.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Aktivieren von Facebook-Anmeldungen

1. Wechseln Sie zur [Facebook-Entwickler Website](https://developers.facebook.com/apps) (melden Sie sich an, wenn Sie noch nicht angemeldet sind).
2. Wählen Sie die Schaltfläche **neue APP erstellen** aus, und befolgen Sie dann die Anweisungen, um die neue Anwendung zu benennen und zu erstellen.
3. Wählen Sie im Abschnitt **Wählen Sie aus, wie Ihre APP in Facebook integriert werden**soll den Abschnitt **Website** aus.
4. Geben Sie im Feld **Website-URL** die URL Ihrer Website ein (z. b. `http://www.example.com`). Das Feld **Domäne** ist optional. Sie können dies verwenden, um die Authentifizierung für eine gesamte Domäne (z. b. *example.com*) bereitzustellen. 

    > [!NOTE]
    > Wenn Sie eine Website auf dem lokalen Computer mit einer URL wie `http://localhost:12345` ausführen (wobei es sich um eine lokale Portnummer handelt), können Sie diesen Wert zum Website- **URL** -Feld hinzufügen, um den Standort zu testen. Allerdings müssen Sie jedes Mal, wenn die Portnummer des lokalen Standorts geändert wird, das Feld " **Website-URL** " der Anwendung aktualisieren.
5. Wählen Sie die Schaltfläche **Änderungen speichern** aus.
6. Klicken Sie erneut auf die Registerkarte **apps** , und zeigen Sie dann die Startseite für Ihre Anwendung an.
7. Kopieren Sie die Werte für **App-ID** und **App-Geheimnis** für die Anwendung, und fügen Sie Sie in eine temporäre Textdatei ein. Diese Werte werden an den Facebook-Anbieter im Website Code übergeben.
8. Beenden Sie die Facebook-Entwickler Website.

Nun nehmen Sie Änderungen an zwei Seiten auf Ihrer Website vor, damit Benutzer sich mit ihren Facebook-Konten bei der Website anmelden können.

1. Erstellen oder öffnen Sie eine ASP.net Web Pages Site, die auf der webmatrix-Starter Site-Vorlage basiert.
2. Öffnen Sie die Seite *\_AppStart. cshtml* , und heben Sie die Auskommentierung des Codes für den Facebook OAuth-Anbieter auf. Der unkommentierte Codeblock sieht wie folgt aus: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopieren Sie den Wert der **App-ID** aus der Facebook-Anwendung als Wert des Parameters `appId` (innerhalb der Anführungszeichen).
4. Kopieren Sie den Wert der **App-Geheimnis** aus der Facebook-Anwendung als `appSecret` Parameterwert.
5. Speichern und schließen Sie die Datei.

### <a name="testing-facebook-login"></a>Testen der Facebook-Anmeldung

1. Führen Sie die Seite *default. cshtml* der Site aus, und klicken Sie auf die Schaltfläche **Anmelden** .
2. Wählen Sie auf der *Anmelde* Seite im Abschnitt **Verwenden eines anderen Dienstanbieter zum Anmelden** das **Facebook** -Symbol aus. 

    Die Webseite leitet die Anforderung an die Facebook-Anmeldeseite um.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Melden Sie sich bei einem Facebook-Konto an. 

    Der Code verwendet das Facebook-Token, um Sie zu authentifizieren, und kehrt dann zu einer Seite zurück, auf der Sie Ihre Facebook-Anmeldung mit dem Anmelde Namen Ihrer Website verknüpfen können. Der Benutzername oder die e-Mail-Adresse wird in das **e-Mail-** Feld im Formular eingegeben.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Wählen Sie die Schaltfläche **zuordnen** aus. 

    Der Browser kehrt zur Startseite zurück, und Sie sind angemeldet.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Aktivieren von Twitter-Anmeldungen

1. Navigieren Sie zur [Twitter-Entwickler Website](https://dev.twitter.com/).
2. Wählen Sie den Link **app erstellen** aus, und melden Sie sich dann bei der Site an.
3. Füllen Sie im Formular " **Anwendung erstellen** " die Felder " **Name** " und " **Beschreibung** " aus.
4. Geben Sie im Feld **Website** die URL Ihrer Website ein (z. b. `http://www.example.com`). 

    > [!NOTE]
    > Wenn Sie Ihre Website lokal testen (mit einer URL wie `http://localhost:12345`), akzeptiert Twitter die URL möglicherweise nicht. Sie können jedoch möglicherweise die lokale Loopback-IP-Adresse verwenden (z. b. `http://127.0.0.1:12345`). Dies vereinfacht das lokale Testen Ihrer Anwendung. Jedes Mal, wenn die Portnummer Ihres lokalen Standorts geändert wird, müssen Sie jedoch das **Website** -Feld Ihrer Anwendung aktualisieren.
5. Geben Sie im Feld **Rückruf-URL** in der Website eine URL für die Seite ein, zu der Benutzer nach der Anmeldung bei Twitter zurückkehren sollen. Geben Sie beispielsweise die URL ein, die Sie im Feld **Website** eingegeben haben, um Benutzer an die Startseite der Startseite zu senden (deren Anmeldestatus erkannt wird).
6. Akzeptieren Sie die Bedingungen, und wählen Sie die Schaltfläche **Ihre Twitter-Anwendung erstellen** aus.
7. Wählen Sie auf der Startseite **eigene Anwendungen** die Anwendung aus, die Sie erstellt haben.
8. Scrollen Sie auf der Registerkarte **Details** zum unteren Rand, und klicken Sie auf die Schaltfläche **mein Zugriffs Token erstellen** .
9. Kopieren Sie auf der Registerkarte **Details** die Werte **Consumer Key** und **Consumer Secret** für Ihre Anwendung, und fügen Sie Sie in eine temporäre Textdatei ein. Diese Werte werden an den Twitter-Anbieter im Website Code übergeben.
10. Beenden Sie die Twitter-Website.

Nun nehmen Sie Änderungen an zwei Seiten auf Ihrer Website vor, damit Benutzer sich mit Ihren Twitter-Konten bei der Website anmelden können.

1. Erstellen oder öffnen Sie eine ASP.net Web Pages Site, die auf der webmatrix-Starter Site-Vorlage basiert.
2. Öffnen Sie die Seite *\_AppStart. cshtml* , und heben Sie die Auskommentierung des Codes für den OAuth-Anbieter von Twitter auf. Der unkommentierte Codeblock sieht wie folgt aus: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopieren Sie den Wert für **Consumer Key** aus der Twitter-Anwendung als Wert des Parameters `consumerKey` (innerhalb der Anführungszeichen).
4. Kopieren Sie den Wert für **Consumer Secret** aus der Twitter-Anwendung als Wert für den `consumerSecret`-Parameter.
5. Speichern und schließen Sie die Datei.

### <a name="testing-twitter-login"></a>Testen der Twitter-Anmeldung

1. Führen Sie die Seite *default. cshtml* Ihrer Website aus, und klicken Sie auf die Schaltfläche **Anmelden** .
2. Wählen Sie auf der *Anmelde* Seite im Abschnitt **Verwenden eines anderen Dienstanbieter zum Anmelden** das **Twitter** -Symbol aus. 

    Die Webseite leitet die Anforderung an eine Twitter-Anmeldeseite für die von Ihnen erstellte Anwendung um.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Melden Sie sich bei einem Twitter-Konto an.
4. Der Code verwendet das Twitter-Token zum Authentifizieren des Benutzers und kehrt dann zu einer Seite zurück, auf der Sie Ihren Anmelde Namen Ihrem Website Konto zuordnen können. Der Name oder die e-Mail-Adresse wird in das **e-Mail-** Feld im Formular eingegeben.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Wählen Sie die Schaltfläche **zuordnen** aus. 

    Der Browser kehrt zur Startseite zurück, und Sie sind angemeldet.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Anpassen des Verhaltens von Websiteseiten](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Hinzufügen von Sicherheit und Mitgliedschaft zu einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkID=202904)
