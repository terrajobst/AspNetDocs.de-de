---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Erstellen einer Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-BestätigungC#und Kenn Wort Zurücksetzung () | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit e-Mail-Bestätigung und Kenn Wort Zurücksetzung mithilfe des ASP.net Identity Mitgliedschafts Systems erstellen Ihre Zertifizierungsstelle...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899696"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Erstellen einer sicheren ASP.NET MVC 5-Web-App mit Anmeldung, E-Mail-Bestätigung und Kennwortzurücksetzung (C#)

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit e-Mail-Bestätigung und Kenn Wort Zurücksetzung mithilfe des ASP.net Identity Mitgliedschafts Systems erstellen

Eine aktualisierte Version dieses Tutorials, das .net Core verwendet, finden Sie unter [Konto Bestätigung und Kenn Wort Wiederherstellung in ASP.net Core [/ASPNET/Core/Security/Authentication/accconfirm].

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Erstellen einer ASP.NET-MVC-App

Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: Sie müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher installieren, um dieses Tutorial abzuschließen.

1. Erstellen Sie ein neues ASP.NET-Webprojekt, und wählen Sie die MVC-Vorlage aus. Web Forms auch ASP.net Identity unterstützt, können Sie ähnliche Schritte in einer Web Forms-app ausführen.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Überlassen Sie die Standard Authentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert. Später in diesem Tutorial wird die Bereitstellung in Azure durchzuführen. Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Legen Sie fest, dass das [Projekt SSL verwendet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Führen Sie die APP aus, klicken Sie auf den Link **registrieren** , und registrieren Sie einen Benutzer. An diesem Punkt wird nur die e-Mail-Überprüfung mit dem [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) -Attribut durchzuführen.
5. Navigieren Sie in Server-Explorer zu **Daten connections\defaultconnection\tables\aspnettusers**, klicken Sie mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**aus.

    Die folgende Abbildung zeigt das `AspNetUsers` Schema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Klicken Sie mit der rechten Maustaste auf die Tabelle **aspnettusers** , und wählen Sie **Tabellendaten anzeigen**aus.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 An diesem Punkt wurde die e-Mail nicht bestätigt.
7. Klicken Sie auf die Zeile, und wählen Sie löschen aus. Sie fügen diese e-Mail im nächsten Schritt erneut hinzu und senden eine Bestätigungs-e-Mail.

## <a name="email-confirmation"></a>Bestätigung per e-Mail

Es wird empfohlen, die e-Mail-Adresse einer neuen Benutzerregistrierung zu bestätigen, um sicherzustellen, dass Sie nicht von einer anderen Person angenommen werden (d. h., Sie haben sich nicht bei der e-Mail einer anderen Person registriert). Angenommen, Sie hatten ein Diskussionsforum, Sie sollten verhindern, dass sich `"bob@example.com"` als `"joe@contoso.com"`registrieren. Ohne e-Mail-Bestätigung können `"joe@contoso.com"` unerwünschte e-Mails von Ihrer APP erhalten. Wenn Bob versehentlich als `"bib@example.com"` registriert ist und es nicht bemerkt hat, wäre er nicht in der Lage, die Kenn Wort Wiederherstellung zu verwenden, da die APP nicht über die richtige e-Mail verfügt. E-Mail-Bestätigung bietet nur eingeschränkten Schutz vor Bots und bietet keinen Schutz vor ermittelten Spammern. Sie verfügen über viele funktionierende e-Mail-Aliase, die Sie zum Registrieren verwenden können.

In der Regel möchten Sie, dass neue Benutzerdaten auf Ihrer Website veröffentlichen, bevor Sie per e-Mail, SMS-SMS oder einem anderen Mechanismus bestätigt wurden. <a id="build"></a>In den folgenden Abschnitten aktivieren wir die e-Mail-Bestätigung und ändern den Code, um zu verhindern, dass sich neu registrierte Benutzer anmelden, bis Ihre e-Mail-Nachricht bestätigt wird.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Sendgrid anschließen

Die Anweisungen in diesem Abschnitt sind nicht aktuell. Aktualisierte Anweisungen finden Sie unter [Konfigurieren eines sendgrid-e-Mail-Anbieters](/aspnet/core/security/authentication/accconfirm#configure-email-provider)

Obwohl in diesem Tutorial nur das Hinzufügen von e-Mail-Benachrichtigungen über [sendgrid](http://sendgrid.com/)veranschaulicht wird, können Sie e-Mails mithilfe von SMTP und anderen Mechanismen (siehe [zusätzliche Ressourcen](#addRes)) senden.

1. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Wechseln Sie zur [Azure sendgrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) -Registrierungsseite, und registrieren Sie sich für ein kostenloses sendgrid-Konto. Konfigurieren Sie sendgrid, indem Sie Code wie den folgenden in *App_Start/identityconfig.cs*hinzufügen:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Sie müssen Folgendes hinzufügen:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Um dieses Beispiel einfach zu halten, speichern wir die App-Einstellungen in der Datei " *Web. config* ":

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. Das Konto und die Anmelde Informationen werden in der appSetting gespeichert. In Azure können diese Werte auf der Registerkarte **[Konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** des Azure-Portal sicher gespeichert werden. Weitere Informationen finden [Sie unter Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)

### <a name="enable-email-confirmation-in-the-account-controller"></a>E-Mail-Bestätigung im Konto Controller aktivieren

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Vergewissern Sie sich, dass die Datei " *views\account\confirmemail.cshtml* " eine korrekte Razor-Syntax aufweist. (Das @-Zeichen in der ersten Zeile ist möglicherweise nicht vorhanden. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Führen Sie die APP aus, und klicken Sie auf den Link registrieren. Nachdem Sie das Registrierungsformular eingereicht haben, sind Sie angemeldet.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Überprüfen Sie Ihr e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>E-Mail-Bestätigung vor der Anmeldung erforderlich

Wenn ein Benutzer das Registrierungsformular abschließt, werden Sie zurzeit angemeldet. Sie möchten Ihre e-Mail in der Regel bestätigen, bevor Sie Sie in protokollieren. Im folgenden Abschnitt ändern wir den Code so, dass neue Benutzer eine bestätigte e-Mail-Nachricht erhalten müssen, bevor Sie angemeldet (authentifiziert) werden. Aktualisieren Sie die `HttpPost Register`-Methode mit den folgenden hervorgehobenen Änderungen:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Wenn Sie die `SignInAsync`-Methode auskommentieren, wird der Benutzer nicht durch die Registrierung angemeldet. Die `TempData["ViewBagLink"] = callbackUrl;` Zeile kann verwendet werden, um [die APP zu Debuggen und die](#dbg) Registrierung ohne e-Mail zu senden. `ViewBag.Message` wird verwendet, um die Bestätigungs Anweisungen anzuzeigen. Das [Beispiel zum Herunterladen](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) enthält Code zum Testen der e-Mail-Bestätigung ohne e-Mail-Einrichtung und kann auch zum Debuggen der Anwendung verwendet werden.

Erstellen Sie eine `Views\Shared\Info.cshtml` Datei, und fügen Sie das folgende Razor-Markup hinzu:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Fügen Sie das [Attribut autorisieren](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) der `Contact` Aktionsmethode des Home-Controllers hinzu. Sie können auf den **Kontakt** Link klicken, um zu überprüfen, ob anonyme Benutzer keinen Zugriff haben und authentifizierte Benutzer Zugriff haben.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Sie müssen auch die `HttpPost Login` Aktionsmethode aktualisieren:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aktualisieren Sie die Ansicht *views\shared\error.cshtml* , um die Fehlermeldung anzuzeigen:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Löschen Sie alle Konten in der Tabelle " **aspnettusers** ", die den zu testenden e-Mail-Alias enthalten. Führen Sie die APP aus, und vergewissern Sie sich, dass Sie sich erst anmelden können, wenn Sie Ihre e-Mail Wenn Sie Ihre e-Mail-Adresse bestätigt haben, klicken Sie auf den Link **Kontakt** .

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Kenn Wort Wiederherstellung/zurück Setzung

Entfernen Sie die Kommentarzeichen aus der `HttpPost ForgotPassword` Aktionsmethode im Konto Controller:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Entfernen Sie die Kommentarzeichen aus dem `ForgotPassword` [Action Link](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in der Razor-Ansichts Datei " *views\account\login.cshtml* ":

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Auf der Anmeldeseite wird jetzt ein Link zum Zurücksetzen des Kennworts angezeigt.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Link zum erneuten Senden von e-Mail

Nachdem ein Benutzer ein neues lokales Konto erstellt hat, sendet er einen Bestätigungslink per e-Mail, bevor er sich anmelden kann. Wenn der Benutzer die Bestätigungs-e-Mail versehentlich löscht oder die e-Mail nie eintrifft, muss der Bestätigungslink erneut gesendet werden. Die folgenden Codeänderungen zeigen, wie Sie diese aktivieren.

Fügen Sie am Ende der Datei " *controllers\accountcontroller.cs* " die folgende Hilfsmethode hinzu:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aktualisieren Sie die Register-Methode, um das neue Hilfsprogramm zu verwenden:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aktualisieren Sie die Anmelde Methode, um das Kennwort erneut zu senden, wenn das Benutzerkonto nicht bestätigt wurde:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Kombinieren von sozialen und lokalen Anmeldekonten

Sie können lokale und soziale Konten kombinieren, indem Sie auf Ihren e-Mail-Link klicken. In der folgenden Reihenfolge **RickAndMSFT@gmail.com** zuerst als lokale Anmeldung erstellt, aber Sie können das Konto zuerst als ein soziales Protokoll erstellen und dann einen lokalen Anmelde Namen hinzufügen.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Klicken Sie auf den Link **Verwalten** . Notieren Sie sich die **externen Anmeldungen: 0** , die diesem Konto zugeordnet ist.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Klicken Sie auf den Link zu einem anderen Dienst Protokoll, und akzeptieren Sie die APP-Anforderungen. Die beiden Konten wurden kombiniert, Sie können sich mit jedem Konto anmelden. Möglicherweise möchten Sie, dass Ihre Benutzer lokale Konten hinzufügen, falls Ihr soziales Protokoll im Authentifizierungsdienst ausgefallen ist, oder wahrscheinlich, dass Sie den Zugriff auf Ihr Social Media-Konto verloren haben.

In der folgenden Abbildung ist Tom ein soziales Protokoll in (das Sie über die **externen Anmeldungen sehen können: 1** auf der Seite angezeigt).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Wenn Sie auf " **Kennwort auswählen** " klicken, können Sie ein lokales Protokoll hinzufügen, das demselben Konto zugeordnet ist.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>E-Mail-Bestätigung ausführlicher

Die [Bestätigung und Kenn Wort wiederASP.net Identity Herstellung](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) für das Tutorial in diesem Thema enthält weitere Details.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debuggen der APP

Wenn Sie keine e-Mail erhalten, die den Link enthält:

- Überprüfen Sie Ihren Junk-oder Spam Ordner.
- Melden Sie sich bei Ihrem sendgrid-Konto an, und klicken Sie auf den [Link Email Activity](https://sendgrid.com/logs/index).

Wenn Sie den Überprüfungs Link ohne e-Mail testen möchten, laden Sie das [vollständige Beispiel](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)herunter. Der Bestätigungslink und die Bestätigungs Codes werden auf der Seite angezeigt.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Links zu ASP.net Identity empfohlenen Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Weitere Informationen zur Kenn Wort Wiederherstellung und zur Konto Bestätigung.
- [MVC 5-App mit Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-App mit Facebook-und Google OAuth 2-Autorisierung schreiben. Außerdem wird gezeigt, wie Sie der Identitätsdatenbank zusätzliche Daten hinzufügen.
- Stellen [Sie eine sichere ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit. In diesem Tutorial wird die Azure-Bereitstellung hinzugefügt, die Sicherheit Ihrer APP mit Rollen, die Verwendung der Mitgliedschafts-API zum Hinzufügen von Benutzern und Rollen sowie zusätzliche Sicherheitsfeatures erläutert.
- [Erstellen einer Google-App für OAuth 2 und Verbinden der APP mit dem Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Erstellen der app in Facebook und Verbinden der APP mit dem Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Einrichten von SSL im Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
