---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Erstellen einer sicheren ASP.net-Web Forms-App mit Benutzerregistrierung, e-Mail-C#Bestätigung und Kenn Wort Zurücksetzung () | Microsoft-Dokumentation
author: Erikre
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.net Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kenn Wort Zurücksetzung mit dem ASP.net Identity Mitglied erstellen...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: af3653bc164810126bc3bf8f1b1794d75642d807
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507129"
---
# <a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Erstellen einer sicheren ASP.NET Web Forms-App mit Benutzerregistrierung, E-Mail-Bestätigung und Kennwortzurücksetzung (C#)

von [Erik Reitan](https://github.com/Erikre)

> In diesem Tutorial wird gezeigt, wie Sie eine ASP.net Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kenn Wort Zurücksetzung mithilfe des ASP.net Identity Mitgliedschafts Systems erstellen. Dieses Tutorial basiert auf dem [MVC-Tutorial](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)von Rick Anderson.

## <a name="introduction"></a>Einführung

Dieses Tutorial führt Sie durch die erforderlichen Schritte zum Erstellen einer ASP.net-Web Forms Anwendung mithilfe von Visual Studio und ASP.NET 4,5 zum Erstellen einer sicheren Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kenn Wort Zurücksetzung.

### <a name="tutorial-tasks-and-information"></a>Lernprogramm Aufgaben und-Informationen:

- [Erstellen einer ASP.net-Web Forms-App](#createWebForms)
- [Sendgrid anschließen](#SG)
- [E-Mail-Bestätigung vor der Anmeldung erforderlich](#require)
- [Kenn Wort Wiederherstellung und Zurücksetzen](#reset)
- [Link zum erneuten Senden von e-Mail](#rsend)
- [Problembehandlung der APP](#dbg)
- [Weitere Ressourcen](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Erstellen einer ASP.net-Web Forms-App

Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie auch [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: Sie müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher installieren, um dieses Tutorial abzuschließen.

1. Erstellen Sie ein neues Projekt (**Datei** -&gt; **Neues Projekt**), und wählen Sie im Dialogfeld **Neues Projekt** die Vorlage **ASP.NET-Webanwendung** und die neueste .NET Framework Version aus.
2. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **Web Forms** aus. Überlassen Sie die Standard Authentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen **Host in der Cloud** aktiviert.   
 Klicken Sie dann auf **OK** , um das neue Projekt zu erstellen.  
    ![Dialogfeld "Neues ASP.NET-Projekt"](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Aktivieren Sie Secure Sockets Layer (SSL) für das Projekt. Führen Sie die Schritte aus, die im Abschnitt **Aktivieren von SSL für den Projekt** Abschnitt der [Reihe "Getting Started with Web Forms Tutorial](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)" beschrieben sind.
4. Führen Sie die APP aus, klicken Sie auf den Link **registrieren** , und registrieren Sie einen neuen Benutzer. An dieser Stelle basiert die einzige Überprüfung auf der e-Mail auf dem [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) -Attribut, um sicherzustellen, dass die e-Mail-Adresse wohl geformt ist. Sie ändern den Code, um eine e-Mail-Bestätigung hinzuzufügen. Schließen Sie das Browserfenster.
5. Navigieren Sie in **Server-Explorer** von Visual Studio (**anzeigen** -&gt; **Server-Explorer**) zu **Daten connections\defaultconnection\tables\aspnettusers**, klicken Sie mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**aus. 

    Die folgende Abbildung zeigt das `AspNetUsers` Tabellen Schema:

    ![Aspnettusers-Tabellen Schema](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. Klicken Sie in **Server-Explorer**mit der rechten Maustaste auf die **aspnettusers** -Tabelle, und wählen Sie **Tabellendaten anzeigen**aus.  
  
    ![Aspnettusers-Tabellendaten](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 An diesem Punkt wurde die e-Mail für den registrierten Benutzer nicht bestätigt.
7. Klicken Sie auf die Zeile, und wählen Sie löschen aus, um den Benutzer zu löschen. Sie fügen diese e-Mail im nächsten Schritt erneut hinzu und senden eine Bestätigungsmeldung an die e-Mail-Adresse.

## <a name="email-confirmation"></a>Bestätigung per e-Mail

Es empfiehlt sich, die e-Mail während der Registrierung eines neuen Benutzers zu bestätigen, um sicherzustellen, dass Sie nicht die Identität eines anderen Benutzers angenommen haben (d. h., Sie haben sich nicht bei der e-Mail einer anderen Person registriert). Angenommen, Sie hatten ein Diskussionsforum, Sie sollten verhindern, dass sich `"bob@cpandl.com"` als `"joe@contoso.com"`registrieren. Ohne e-Mail-Bestätigung können `"joe@contoso.com"` unerwünschte e-Mails von Ihrer APP erhalten. Wenn Bob versehentlich als `"bib@cpandl.com"` registriert ist und es nicht bemerkt hat, wäre er nicht in der Lage, die Kenn Wort Wiederherstellung zu verwenden, da die APP nicht über die richtige e-Mail verfügt. E-Mail-Bestätigung bietet nur eingeschränkten Schutz vor Bots und bietet keinen Schutz vor ermittelten Spammern.

Im Allgemeinen möchten Sie, dass neue Benutzer keine Daten an Ihre Website senden können, bevor Sie durch e-Mail, SMS, SMS oder einen anderen Mechanismus bestätigt wurden. In den folgenden Abschnitten aktivieren wir die e-Mail-Bestätigung und ändern den Code, um zu verhindern, dass sich neu registrierte Benutzer anmelden, bis Ihre e-Mail-Nachricht bestätigt wird. In diesem Tutorial verwenden Sie den e-Mail-Dienst sendgrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Sendgrid anschließen

Sendgrid hat seine API geändert, seit dieses Tutorial geschrieben wurde. Aktuelle sendgrid-Anweisungen finden Sie unter [sendgrid](http://sendgrid.com/) oder [Aktivieren der Konto Bestätigung und Kenn Wort Wiederherstellung](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Obwohl in diesem Tutorial nur das Hinzufügen von e-Mail-Benachrichtigungen über [sendgrid](http://sendgrid.com/)veranschaulicht wird, können Sie e-Mails mithilfe von SMTP und anderen Mechanismen (siehe [zusätzliche Ressourcen](#addRes)) senden.

1. Öffnen Sie in Visual Studio die **Paket-Manager-Konsole** (**Tools** -&gt; **nuget-Paket** -Manager -&gt; Paket- **Manager-Konsole**), und geben Sie den folgenden Befehl ein:  
    `Install-Package SendGrid`
2. Wechseln Sie zur [Azure sendgrid-Registrierungsseite](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) , und registrieren Sie sich für das kostenlose sendgrid-Konto. Sie können sich auch direkt auf [der sendgrid-Website](http://www.sendgrid.com)für ein kostenloses sendgrid-Konto registrieren.
3. Öffnen Sie in **Projektmappen-Explorer** die Datei *IdentityConfig.cs* im Ordner *App\_Start* , und fügen Sie den folgenden Code, der in gelb hervorgehoben ist, der `EmailService`-Klasse hinzu, um **sendgrid**zu konfigurieren:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Fügen Sie außerdem die folgenden `using`-Anweisungen am Anfang der *IdentityConfig.cs* -Datei hinzu: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Um dieses Beispiel einfach zu halten, speichern Sie die Werte für das e-Mail-Dienst Konto im `appSettings` Abschnitt der Datei " *Web. config* ". Fügen Sie der Datei " *Web. config* " im Stammverzeichnis des Projekts den folgenden XML-Code hinzu:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. In diesem Beispiel werden das Konto und die Anmelde Informationen im **appSetting** -Abschnitt der Datei " *Web. config* " gespeichert. In Azure können diese Werte auf der Registerkarte **[Konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** des Azure-Portal sicher gespeichert werden. Weitere Informationen finden Sie im Thema zu den bewährten Methoden für die Bereitstellung von [Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Fügen Sie die Werte für den e-Mail-Dienst der sendgrid-Authentifizierungs Werte (Benutzer Name und Kennwort) hinzu, damit Sie erfolgreich e-Mails von Ihrer APP senden können. Achten Sie darauf, dass Sie Ihren sendgrid-Kontonamen anstelle der von Ihnen angegebenen e-Mail-Adresse verwenden.

### <a name="enable-email-confirmation"></a>Bestätigung per e-Mail aktivieren

 Um die e-Mail-Bestätigung zu aktivieren, ändern Sie den Registrierungscode mithilfe der folgenden Schritte.  

1. Öffnen Sie im *Konto* Ordner den *Register.aspx.cs* -Code Behind, und aktualisieren Sie die `CreateUser_Click`-Methode, um die folgenden markierten Änderungen zu aktivieren: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *default. aspx* , und wählen Sie **als Start Seite festlegen**aus.
3. Drücken Sie F5, um die APP auszuführen **.** Nachdem die Seite angezeigt wird, klicken Sie auf den Link **registrieren** , um die Registerseite anzuzeigen.
4. Geben Sie Ihre e-Mail und Ihr Kennwort ein, und klicken Sie auf die Schaltfläche **registrieren** , um eine e-Mail über sendgrid zu senden.  
   Der aktuelle Zustand des Projekts und Codes ermöglicht dem Benutzer das anmelden, sobald er das Registrierungsformular abschließt, auch wenn Sie sein Konto nicht bestätigt haben.
5. Überprüfen Sie Ihr e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail  
   Nachdem Sie das Registrierungsformular eingereicht haben, werden Sie angemeldet.  
    ![Beispiel Website-angemeldet](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>E-Mail-Bestätigung vor der Anmeldung erforderlich

Obwohl Sie das e-Mail-Konto bestätigt haben, müssen Sie an diesem Punkt nicht auf den in der Bestätigungs-e-Mail enthaltenen Link klicken, um vollständig angemeldet zu sein. Im folgenden Abschnitt ändern Sie den Code, der erfordert, dass neue Benutzer eine bestätigte e-Mail-Nachricht erhalten, bevor Sie angemeldet werden (authentifiziert).

1. Aktualisieren Sie in **Projektmappen-Explorer** von Visual Studio das `CreateUser_Click`-Ereignis in der *Register.aspx.cs* -Code-Behind-Datei, die im *Konten* Ordner enthalten ist, mit den folgenden hervorgehobenen Änderungen: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aktualisieren Sie die `LogIn`-Methode im Code-Behind *Login.aspx.cs* mit den folgenden hervorgehobenen Änderungen: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Ausführen der Anwendung

 Nachdem Sie den Code implementiert haben, um zu überprüfen, ob die e-Mail-Adresse eines Benutzers bestätigt wurde, können Sie die Funktionalität sowohl auf den **Register** -als auch auf den **Anmelde** Seiten überprüfen. 

1. Löschen Sie alle Konten in der Tabelle " **aspnettusers** ", die den zu testenden e-Mail-Alias enthalten.
2. Führen Sie die APP (**F5**) aus, und vergewissern Sie sich, dass Sie sich erst dann als Benutzer registrieren können, wenn Sie Ihre e-Mail
3. Bevor Sie Ihr neues Konto über die soeben gesendete e-Mail bestätigen, versuchen Sie, sich mit dem neuen Konto anzumelden.  
 Sie werden feststellen, dass Sie sich nicht anmelden können und dass Sie über ein bestätigtes e-Mail-Konto verfügen müssen.
4. Wenn Sie Ihre e-Mail-Adresse bestätigt haben, melden Sie sich bei der APP an.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Kenn Wort Wiederherstellung und Zurücksetzen

1. Entfernen Sie in Visual Studio die Kommentarzeichen aus der `Forgot`-Methode im Code-Behind *Forgot.aspx.cs* , der im *Konto* Ordner enthalten ist, damit die-Methode wie folgt aussieht: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Öffnen Sie die Seite *Login. aspx* . Ersetzen Sie das Markup am Ende des **LoginForm** -Abschnitts wie unten gezeigt: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Öffnen Sie den *Login.aspx.cs* -Code-Behind, und heben Sie die Auskommentierung der folgenden Codezeile auf, die im `Page_Load` Ereignishandler gelb hervorgehoben ist: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Drücken Sie F5, um die APP auszuführen **.** Nachdem die Seite angezeigt wird, klicken Sie auf den Link **Anmelden** .
5. Klicken Sie auf den Link **Kennwort vergessen?** , um die Seite **Kennwort vergessen** anzuzeigen.
6. Geben Sie Ihre e-Mail-Adresse ein, und klicken Sie auf die Schaltfläche **senden** , um eine e-Mail an Ihre Adresse zu senden   
   Aktivieren Sie Ihr e-Mail-Konto, und klicken Sie auf den Link, um die Seite **Kennwort zurücksetzen**
7. Geben Sie auf der Seite **Kennwort zurücksetzen** Ihre e-Mail, Ihr Kennwort und Ihr bestätigtes Kennwort ein. Klicken Sie dann auf die Schaltfläche **Zurücksetzen** .  
   Wenn Sie Ihr Kennwort erfolgreich zurückgesetzt haben, wird die Seite **Kennwort geändert** angezeigt. Jetzt können Sie sich mit Ihrem neuen Kennwort anmelden.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Link zum erneuten Senden von e-Mail

Nachdem ein Benutzer ein neues lokales Konto erstellt hat, sendet er einen Bestätigungslink per e-Mail, bevor er sich anmelden kann. Wenn der Benutzer die Bestätigungs-e-Mail versehentlich löscht oder die e-Mail nie eintrifft, muss der Bestätigungslink erneut gesendet werden. Die folgenden Codeänderungen zeigen, wie Sie diese aktivieren.

1. Öffnen Sie in Visual Studio den **Login.aspx.cs** -Code Behind, und fügen Sie den folgenden Ereignishandler nach dem `LogIn`-Ereignishandler hinzu:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Ändern Sie den `LogIn`-Ereignishandler im Code Behind *Login.aspx.cs* , indem Sie den in gelb markierten Code wie folgt ändern: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aktualisieren Sie die Seite *Login. aspx* , indem Sie den in gelb markierten Code wie folgt hinzufügen: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Löschen Sie alle Konten in der Tabelle " **aspnettusers** ", die den zu testenden e-Mail-Alias enthalten.
5. Führen Sie die APP (**F5**) und Ihre e-Mail-Adresse aus.
6. Bevor Sie Ihr neues Konto über die soeben gesendete e-Mail bestätigen, versuchen Sie, sich mit dem neuen Konto anzumelden.  
   Sie werden feststellen, dass Sie sich nicht anmelden können und dass Sie über ein bestätigtes e-Mail-Konto verfügen müssen. Außerdem können Sie jetzt eine Bestätigungsnachricht erneut an Ihr e-Mail-Konto senden.
7. Geben Sie Ihre e-Mail-Adresse und Ihr Kennwort ein, und **Klicken Sie dann auf die Schalt** Fläche
8. Wenn Sie Ihre e-Mail-Adresse anhand der neu gesendeten e-Mail-Nachricht bestätigen, melden Sie sich bei der APP an.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Problembehandlung der APP

Wenn Sie keine e-Mail mit dem Link erhalten, um Ihre Anmelde Informationen zu überprüfen:

- Überprüfen Sie Ihren Junk-oder Spam Ordner.
- Melden Sie sich bei Ihrem sendgrid-Konto an, und klicken Sie auf den [Link Email Activity](https://sendgrid.com/logs/index).
- Stellen Sie sicher, dass Sie Ihren sendgrid-Benutzerkonto Namen als *Web. config* -Wert anstelle der e-Mail-Adresse Ihres sendgrid-Kontos verwendet haben.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Links zu ASP.net Identity empfohlenen Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.net Web Forms-tutorialreihe: Hinzufügen eines OAuth 2,0-Anbieters](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Stellen Sie eine sichere ASP.net Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank bereit, um Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.net Web Forms-tutorialreihe: Aktivieren von SSL für das Projekt](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
