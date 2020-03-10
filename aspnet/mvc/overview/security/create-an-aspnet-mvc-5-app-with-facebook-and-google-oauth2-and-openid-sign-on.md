---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Erstellen einer MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-C#on () | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Webanwendung erstellen, die es Benutzern ermöglicht, sich mithilfe von OAuth 2,0 mit Anmelde Informationen aus einem externen au anzumelden...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78456507"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Erstellen einer ASP.NET MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2-Anmeldung (C#)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Webanwendung erstellen, die es Benutzern ermöglicht, sich mithilfe von [OAuth 2,0](http://oauth.net/2/) mit Anmelde Informationen eines externen Authentifizierungs Anbieters wie Facebook, Twitter, LinkedIn, Microsoft oder Google anzumelden. Der Einfachheit halber konzentriert sich dieses Tutorial auf das Arbeiten mit Anmelde Informationen von Facebook und Google.
> 
> Die Aktivierung dieser Anmelde Informationen auf ihren Websites bietet einen erheblichen Vorteil, da Millionen von Benutzern bereits über Konten mit diesen externen Anbietern verfügen. Diese Benutzer sind möglicherweise eher geneigt, sich für Ihre Website zu registrieren, wenn Sie keinen neuen Satz von Anmelde Informationen erstellen und speichern müssen.
> 
> Siehe auch [ASP.NET MVC 5-App mit der zweistufigen Authentifizierung per SMS und e-Mail](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Das Tutorial zeigt außerdem, wie Profildaten für den Benutzer hinzugefügt werden und wie die Mitgliedschafts-API zum Hinzufügen von Rollen verwendet wird. Dieses Tutorial wurde von [Rick Anderson](https://blogs.msdn.com/rickAndy) verfasst (folgen Sie mir auf Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

<a id="start"></a>
## <a name="getting-started"></a>Erste Schritte

Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher. Hilfe zu Dropbox, GitHub, LinkedIn, Instagram, Buffer, Salesforce, Steam, Stack Exchange, "testpit", "Twitch", "Twitter", "Yahoo!" und mehr finden Sie in diesem [Beispiel Projekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Sie müssen Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher installieren, um Google OAuth 2 verwenden und lokal ohne SSL-Warnungen Debuggen zu können.

Klicken Sie auf der **Start** Seite auf **Neues Projekt** , oder verwenden Sie das Menü, und wählen Sie **Datei**und dann **Neues Projekt**aus.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Klicken Sie auf **Neues Projekt**, und wählen Sie dann auf der linken Seite die Option **Visualisierung C#**  und dann **Web** und dann **ASP.NET Webanwendung**aus. Nennen Sie das Projekt "mvcauth", und klicken Sie dann auf **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Klicken Sie im Dialogfeld **Neues Projekt ASP.net** auf **MVC**. Wenn die Authentifizierung keine **einzelnen Benutzerkonten**ist, klicken Sie auf die Schaltfläche **Authentifizierung ändern** , und wählen Sie **einzelne Benutzerkonten**aus. Durch das Überprüfen von **Host in der Cloud**kann die app in Azure sehr einfach gehostet werden.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Wenn Sie **Host in der Cloud**ausgewählt haben, vervollständigen Sie das Dialogfeld konfigurieren.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Verwenden von nuget zum Aktualisieren auf die neueste owin-Middleware

Verwenden Sie den nuget-Paket-Manager, um die [owin-Middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)zu aktualisieren. Wählen Sie im Menü auf der linken Seite die Option **Updates** . Sie können auf die Schaltfläche **Alle aktualisieren** klicken, oder Sie können nur nach owin-Paketen suchen (in der nächsten Abbildung gezeigt):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

In der folgenden Abbildung werden nur owin-Pakete angezeigt:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

In der Paket-Manager-Konsole (PMC) können Sie den `Update-Package`-Befehl eingeben, mit dem alle Pakete aktualisiert werden.

Drücken Sie **F5** oder **STRG + F5** , um die Anwendung auszuführen. In der folgenden Abbildung ist die Portnummer 1234. Wenn Sie die Anwendung ausführen, wird eine andere Portnummer angezeigt.

Abhängig von der Größe Ihres Browserfensters müssen Sie möglicherweise auf das Navigations Symbol klicken, um die Links " **Home**", " **about**", " **Contact**", " **Register** " und **"Anmelden"** anzuzeigen.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Einrichten von SSL im Projekt

Zum Herstellen einer Verbindung mit Authentifizierungs Anbietern wie Google und Facebook müssen Sie IIS-Express für die Verwendung von SSL einrichten. Es ist wichtig, dass Sie SSL nach der Anmeldung weiterhin verwenden und nicht auf http zurücksetzen. Ihr Anmelde Cookie ist genauso geheim wie Ihr Benutzername und Ihr Kennwort, und ohne SSL senden Sie es in Klartext über das Netzwerk. Außerdem haben Sie sich bereits die Zeit genommen, den Hand Shake auszuführen und den Kanal zu sichern (was den Großteil der Vorgänge ist, die HTTPS langsamer als http macht), bevor die MVC-Pipeline ausgeführt wird. Daher wird die aktuelle Anforderung oder Zukunft durch die Umleitung an http nach der Anmeldung an http zurückgeleitet. Anforderungen sind viel schneller.

1. Klicken Sie in **Projektmappen-Explorer**auf das **mvcauth** -Projekt.
2. Drücken Sie die F4-Taste, um die Projekteigenschaften anzuzeigen. Alternativ können Sie im Menü **Ansicht** die Option **Eigenschaften Fenster**auswählen.
3. Ändern Sie **SSL** in "true".  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Kopieren Sie die SSL-URL (die `https://localhost:44300/` wird, es sei denn, Sie haben andere SSL-Projekte erstellt).
5. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **mvcauth** , und wählen Sie **Eigenschaften**aus.
6. Wählen Sie die Registerkarte **Web** aus, und fügen Sie die SSL-URL in das Feld **Projekt-URL** ein. Speichern Sie die Datei (CTL + S). Sie benötigen diese URL, um Facebook-und Google-Authentifizierungs-apps zu konfigurieren.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Fügen Sie dem `Home` Controller das Attribut "Requirements [https](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) " hinzu, damit alle Anforderungen HTTPS verwenden müssen. Ein sichereren Ansatz besteht darin, der Anwendung den "Requirements [https](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) "-Filter hinzuzufügen. Weitere Informationen finden Sie im Abschnitt &quot;schützen der Anwendung mit SSL und des Attributs "autorisieren"&quot; in meinem Tutorial [Erstellen einer ASP.NET MVC-App mit Authentifizierung und SQL-Datenbank und Bereitstellen für Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein Teil des Home-Controllers wird unten angezeigt.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Drücken Sie STRG+F5, um die Anwendung auszuführen. Wenn Sie das Zertifikat in der Vergangenheit installiert haben, können Sie den restlichen Teil dieses Abschnitts überspringen und zum [Erstellen einer Google-App für OAuth 2 und zum Verbinden der APP mit dem Projekt](#goog)wechseln. andernfalls folgen Sie den Anweisungen, um dem von IIS Express generierten selbst signierten Zertifikat zu vertrauen.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lesen Sie das Dialogfeld **Sicherheitswarnung** , und klicken Sie dann auf **Ja** , wenn Sie das Zertifikat installieren möchten, das localhost darstellt.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Die Seite *Home* wird in IE angezeigt, und es gibt keine SSL-Warnungen.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome akzeptiert auch das Zertifikat und zeigt HTTPS-Inhalt ohne Warnung an. Firefox verwendet einen eigenen Zertifikat Speicher, sodass eine Warnung angezeigt wird. Für die Anwendung können Sie auf sichere Weise darauf klicken, dass **ich die Risiken verstanden habe**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Erstellen einer Google-App für OAuth 2 und Verbinden der APP mit dem Projekt

> [!WARNING]
> Aktuelle Google OAuth-Anweisungen finden Sie unter [Konfigurieren der Google-Authentifizierung in ASP.net Core](/aspnet/core/security/authentication/social/google-logins).

1. Navigieren Sie zur [Google Developers Console](https://console.developers.google.com/).
2. Wenn Sie noch kein Projekt erstellt haben, klicken Sie auf der linken Registerkarte auf **Anmelde** Informationen, und wählen Sie dann **Erstellen**aus.
3. Klicken Sie auf der linken Registerkarte auf **Anmelde**Informationen.
4. Klicken Sie auf **Anmelde Informationen erstellen** und dann auf **OAuth Client ID**. 

    1. Behalten Sie im Dialogfeld **Client-ID erstellen** die **Standardweb Anwendung** für den Anwendungstyp bei.
    2. Legen Sie die **autorisierten JavaScript** -Ursprünge auf die oben verwendete SSL-URL fest (`https://localhost:44300/`, es sei denn, Sie haben andere SSL-Projekte erstellt)
    3. Legen Sie den **autorisierten Umleitungs-URI** auf fest:  
         `https://localhost:44300/signin-google`
5. Klicken Sie auf das Menü Element OAuth-Zustimmungs Bildschirm und dann auf Ihre e-Mail-Adresse und den Produktnamen. Wenn Sie das Formular ausgefüllt haben, klicken Sie auf **Speichern**.
6. Klicken Sie auf das Menü Element Bibliothek, suchen Sie nach **Google + API**, klicken Sie darauf, und drücken Sie dann aktivieren.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Die folgende Abbildung zeigt die aktivierten APIs.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Rufen Sie über den Google API-API-Manager die Registerkarte **Anmelde** Informationen auf, um die **Client-ID**abzurufen. Herunterladen, um eine JSON-Datei mit geheimen Anwendungs Schlüsseln zu speichern. Kopieren Sie **ClientID** und **clientsecret** , und fügen Sie Sie in die `UseGoogleAuthentication`-Methode ein, die sich in der Datei *Startup.auth.cs* im Ordner *App_Start* befindet. Die unten gezeigten **ClientID** -und **clientsecret** -Werte sind Beispiele und funktionieren nicht.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. Das Konto und die Anmelde Informationen werden dem obigen Code hinzugefügt, um das Beispiel einfach zu halten. Weitere Informationen finden [Sie unter Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
8. Drücken Sie **STRG+F5** , um die Anwendung zu erstellen und auszuführen. Klicken Sie auf den Link **Log in** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Klicken Sie unter **anderen Dienst zum Anmelden verwenden auf** **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Wenn Sie einen der obigen Schritte übersehen, erhalten Sie einen HTTP 401-Fehler. Überprüfen Sie die obigen Schritte. Wenn Sie eine erforderliche Einstellung übersehen (z. b. **Produktname**), fügen Sie das fehlende Element hinzu, und speichern Sie es. Es kann einige Minuten dauern, bis die Authentifizierung funktioniert.
10. Sie werden auf die Google-Website umgeleitet, auf der Sie Ihre Anmelde Informationen eingeben.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert, Berechtigungen für die soeben erstellten Webanwendung zu erteilen:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Klicken Sie auf **Annehmen**. Sie werden nun wieder auf die **Register** Seite der mvcauth-Anwendung umgeleitet, auf der Sie Ihr Google-Konto registrieren können. Sie können zwar den lokalen E-Mail-Registrierungsnamen in Ihr Gmail-Konto ändern, im Allgemeinen sollten Sie jedoch den Standard-E-Mail-Aliasnamen beibehalten (den Sie für die Authentifizierung verwendet haben). Klicken Sie auf **Registrieren**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Erstellen der app in Facebook und Verbinden der APP mit dem Projekt

> [!WARNING]
> Aktuelle Facebook OAuth2-Authentifizierungs Anweisungen finden Sie unter [Konfigurieren der Facebook-Authentifizierung](/aspnet/core/security/authentication/social/facebook-logins) .

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Überprüfen der Mitgliedschafts Daten

Klicken Sie im Menü **Ansicht** auf **Server-Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Erweitern Sie **DefaultConnection (mvcauth)** , **Tabellen**, klicken Sie mit der rechten Maustaste auf **aspnettusers** , und klicken Sie dann auf **Tabellendaten anzeigen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnettusers-Tabellendaten](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Hinzufügen von Profildaten zur Benutzerklasse

In diesem Abschnitt fügen Sie den Benutzerdaten während der Registrierung Geburtsdatum und Privatstadt hinzu, wie in der folgenden Abbildung dargestellt.

![reg mit "Home Town" und "bday"](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Öffnen Sie die Datei " *models\identitymodels.cs* ", und fügen Sie die Eigenschaften "Geburtsdatum" und "Home"

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Öffnen Sie die Datei " *models\accountviewmodels.cs* " und die Eigenschaften "Geburtsdatum" und "Home" festlegen in `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Öffnen Sie die Datei " *controllers\accountcontroller.cs* ", und fügen Sie Code für Geburtsdatum und Home Town in der `ExternalLoginConfirmation` Aktionsmethode hinzu, wie hier gezeigt:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Fügen Sie der Datei " *views\account\externaddiginconfirmation.cshtml* " das Geburtsdatum und die Stadt "Home" hinzu:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Löschen Sie die Mitgliedschafts Datenbank, damit Sie Ihr Facebook-Konto erneut bei Ihrer Anwendung registrieren können, und überprüfen Sie, ob Sie das neue Geburtsdatum und die Heimat Profilinformationen hinzufügen können.

Klicken Sie in **Projektmappen-Explorer**auf das Symbol **alle Dateien anzeigen** , klicken Sie mit der rechten Maustaste auf *Add\_data\aspnet-mvcauth-&lt;DATESTAMP&gt;. mdf* , und klicken Sie dann auf **Löschen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Klicken Sie **im Menü** Extras auf **nuget-Paket**-Manager, und klicken Sie dann auf Paket-Manager- **Konsole** (PMC). Geben Sie in der PMC die folgenden Befehle ein.

1. Aktivieren-Migrationen
2. Add-Migration-init
3. Update-Database

Führen Sie die Anwendung aus, und verwenden Sie Facebook und Google, um sich anzumelden und einige Benutzer zu registrieren.

## <a name="examine-the-membership-data"></a>Überprüfen der Mitgliedschafts Daten

Klicken Sie im Menü **Ansicht** auf **Server-Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Klicken Sie mit der rechten Maustaste auf **aspnettusers** und dann auf **Tabellendaten anzeigen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Die Felder `HomeTown` und `BirthDate` sind unten dargestellt.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Abmelden ihrer APP und Anmelden mit einem anderen Konto

Wenn Sie sich bei Ihrer APP mit Facebook, anmelden und sich abmelden und dann erneut versuchen, sich mit einem anderen Facebook-Konto anzumelden (mit demselben Browser), werden Sie sofort beim vorherigen Facebook-Konto angemeldet, das Sie verwendet haben. Um ein anderes Konto zu verwenden, müssen Sie zu Facebook navigieren und sich bei Facebook abmelden. Die gleiche Regel gilt für alle anderen Authentifizierungs Anbieter von Drittanbietern. Alternativ können Sie sich mit einem anderen Konto anmelden, indem Sie einen anderen Browser verwenden.

## <a name="next-steps"></a>Nächste Schritte

Anweisungen für Yahoo und LinkedIn finden Sie unter [Einführung der Autorisierungs-und LinkedIn OAuth-Sicherheitsanbieter für owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) von Jerrie pelser. Weitere Informationen finden Sie unter die Recht sozialen Anmelde Schaltflächen von Jerrie für ASP.NET MVC 5, um Anmelde Schaltflächen für soziale

Befolgen Sie das Tutorial [Erstellen einer ASP.NET MVC-App mit Authentifizierung und SQL-](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)Datenbank, und stellen Sie eine Bereitstellung auf Azure App Service bereit, die dieses Tutorial fortsetzt und Folgendes zeigt:

1. Erfahren Sie, wie Sie Ihre APP in Azure bereitstellen.
2. So sichern Sie Ihre APP mit Rollen
3. So sichern Sie Ihre APP mit den "Requirements [https](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) "-und " [Autorisierungs](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtern".
4. Verwenden der Mitgliedschafts-API zum Hinzufügen von Benutzern und Rollen.

Informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir verbessern konnten. Sie können auch neue Themen unter Anzeigen der Art von [Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)anfordern. Sie können sogar neue Features anfordern und abstimmen, die zu ASP.net hinzugefügt werden sollen. Beispielsweise können Sie für ein Tool abstimmen, um [Benutzer und Rollen zu erstellen und zu verwalten.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Eine ausführliche Erläuterung der Funktionsweise externer Authentifizierungsdienste ASP.net finden Sie unter den [externen Authentifizierungsdiensten](https://asp.net/web-api/overview/security/external-authentication-services)von Robert McMurray. Der Artikel von Robert behandelt außerdem die Aktivierung der Microsoft-und Twitter-Authentifizierung. Das ausgezeichnete [EF/MVC-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) von Tom Dykstra zeigt, wie Sie mit dem Entity Framework arbeiten.
