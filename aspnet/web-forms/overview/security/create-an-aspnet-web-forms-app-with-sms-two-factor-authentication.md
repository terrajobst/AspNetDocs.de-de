---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Erstellen einer ASP.net-Web Forms-App mit zweistufiger SMSC#-Authentifizierung () | Microsoft-Dokumentation
author: Erikre
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.net Web Forms-App mit zweistufiger Authentifizierung erstellen. Dieses Tutorial wurde entwickelt, um das Tutorial "CR..." zu ergänzen.
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c9558aca8a655071c0c94ed66433cf721f26c011
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78455979"
---
# <a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Erstellen einer ASP.NET Web Forms-Anwendung mit zweistufiger Authentifizierung per SMS (C#)

von [Erik Reitan](https://github.com/Erikre)

[Herunterladen der ASP.net Web Forms-App per e-Mail und die zweistufige Authentifizierung mit SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> In diesem Tutorial wird gezeigt, wie Sie eine ASP.net Web Forms-App mit zweistufiger Authentifizierung erstellen. Dieses Tutorial wurde entwickelt, um das Tutorial [Erstellen einer sicheren ASP.net Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kenn Wort](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)Zurücksetzung zu ergänzen. Außerdem basiert dieses Tutorial auf dem [MVC-Tutorial](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)von Rick Anderson.

## <a name="introduction"></a>Einführung

Dieses Tutorial führt Sie durch die erforderlichen Schritte zum Erstellen einer ASP.net-Web Forms Anwendung, die die zweistufige Authentifizierung mit Visual Studio unterstützt. Die zweistufige Authentifizierung ist ein zusätzlicher Benutzer Authentifizierungs Schritt. Durch diesen zusätzlichen Schritt wird während der Anmeldung eine eindeutige PIN (Personal Identification Number) generiert. Die PIN wird häufig als e-Mail oder SMS an den Benutzer gesendet. Der Benutzer der APP gibt dann die PIN als zusätzliches Authentifizierungs Measure ein, wenn er sich anmeldet.

### <a name="tutorial-tasks-and-information"></a>Lernprogramm Aufgaben und-Informationen:

- [Erstellen einer ASP.net-Web Forms-App](#createWebForms)
- [Einrichten von SMS und zweistufiger Authentifizierung](#SMS)
- [Aktivieren der zweistufigen Authentifizierung für registrierte Benutzer](#use2FA)
- [Weitere Ressourcen](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Erstellen einer ASP.net-Web Forms-App

Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie auch [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher. Außerdem müssen Sie ein [twilio](https://www.twilio.com/try-twilio) -Konto erstellen, wie unten erläutert.

> [!NOTE]
> Wichtig: Sie müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher installieren, um dieses Tutorial abzuschließen.

1. Erstellen Sie ein neues Projekt (**Datei** -&gt; **Neues Projekt**), und wählen Sie im Dialogfeld **Neues Projekt** die Vorlage **ASP.NET-Webanwendung** zusammen mit der .NET Framework Version aus.
2. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **Web Forms** aus. Überlassen Sie die Standard Authentifizierung als **einzelne Benutzerkonten**. Klicken Sie dann auf **OK** , um das neue Projekt zu erstellen.  
    ![Dialogfeld "Neues ASP.NET-Projekt"](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Aktivieren Sie Secure Sockets Layer (SSL) für das Projekt. Führen Sie die Schritte aus, die im Abschnitt **Aktivieren von SSL für den Projekt** Abschnitt der [Reihe "Getting Started with Web Forms Tutorial](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)" beschrieben sind.
4. Öffnen Sie in Visual Studio die **Paket-Manager-Konsole** (**Tools** -&gt; **nuget-Paket** -Manager -&gt; Paket- **Manager-Konsole**), und geben Sie den folgenden Befehl ein:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Einrichten von SMS und zweistufiger Authentifizierung

In diesem Tutorial wird twilio verwendet, Sie können jedoch einen beliebigen SMS-Anbieter verwenden.

1. Erstellen Sie ein [twilio](https://www.twilio.com/try-twilio) -Konto.
2. Kopieren Sie die **Konto-SID** und das Authentifizierungs Token auf der Registerkarte **Dashboard** Ihres twilio-Kontos **.** Sie werden Sie später zu Ihrer APP hinzufügen.
3. Kopieren Sie auf der Registerkarte **Zahlen** auch Ihre twilio- **Telefonnummer** .
4. Machen Sie die twilio- **Konto-SID**, das Authentifizierungs **Token** und die **Telefonnummer** für die app verfügbar. Um die Dinge einfach zu halten, speichern Sie diese Werte in der Datei " *Web. config* ". Wenn Sie in Azure bereitstellen, können Sie die Werte im **appSettings** -Abschnitt auf der Registerkarte "Website konfigurieren" sicher speichern. Verwenden Sie beim Hinzufügen der Telefonnummer auch nur Ziffern.   
   Beachten Sie, dass Sie auch sendgrid-Anmelde Informationen hinzufügen können. Sendgrid ist ein e-Mail-Benachrichtigungsdienst. Ausführliche Informationen zum Aktivieren von sendgrid finden Sie im Abschnitt "Einbinden von sendgrid" im Tutorial [Erstellen einer Secure ASP.net Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kenn Wort](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md) Zurücksetzung.

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. In diesem Beispiel werden das Konto und die Anmelde Informationen im **appSettings** -Abschnitt der Datei " *Web. config* " gespeichert. In Azure können diese Werte auf der Registerkarte **[Konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** des Azure-Portal sicher gespeichert werden. Weitere Informationen finden Sie im Thema zu den bewährten Methoden für die Bereitstellung von [Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
5. Konfigurieren Sie die `SmsService`-Klasse in der *App\_start\identityconfig.cs* -Datei, indem Sie die folgenden Änderungen gelb hervorheben: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Fügen Sie am Anfang der Datei *IdentityConfig.cs* die folgenden `using`-Anweisungen hinzu: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aktualisieren Sie die Datei *Account/Manage. aspx* , indem Sie die in gelb markierten Zeilen entfernen:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. Heben Sie im `Page_Load` Handler des *Manage.aspx.cs* -Code-Behind die Auskommentierung der in gelb markierten Codezeile auf, sodass Sie wie folgt aussieht: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Aktualisieren Sie im Code Behind von *Account*/*TwoFactorAuthenticationSignIn.aspx.cs*den `Page_Load` Handler, indem Sie den folgenden Code in gelb hervorheben: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Wenn Sie die obige Codeänderung vornehmen, wird die Dropdown Liste "Providers", die die Authentifizierungs Optionen enthält, nicht auf den ersten Wert zurückgesetzt. Dadurch kann der Benutzer bei der Authentifizierung alle Optionen auswählen, die verwendet werden sollen, und nicht nur den ersten.
10. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *default. aspx* , und wählen Sie **als Start Seite festlegen**aus.
11. Wenn Sie Ihre APP testen, erstellen Sie zunächst die APP (**STRG**+**UMSCHALT**+**B**) und führen dann die APP (**F5**) aus. Wählen Sie entweder **registrieren** aus, um ein neues Benutzerkonto zu erstellen, oder wählen Sie anmelden **aus** , wenn das Benutzerkonto bereits registriert wurde.
12. Nachdem Sie (als Benutzer) angemeldet sind, klicken Sie auf der Navigationsleiste auf die Benutzer-ID (e-Mail-Adresse), um die Seite **Konto verwalten** (Manage. aspx) anzuzeigen.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Klicken Sie auf der Seite **Konto verwalten** auf **Hinzufügen** neben **Telefonnummer** .  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Fügen Sie die Telefonnummer hinzu, an der Sie (als Benutzer) SMS-Nachrichten (Textnachrichten) empfangen möchten, und klicken Sie auf die Schaltfläche **senden** .   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    An diesem Punkt verwendet die APP die Anmelde Informationen aus der Datei " *Web. config* ", um sich mit twilio zu kontaktieren. Eine SMS-Nachricht (SMS) wird an das Telefon gesendet, das dem Benutzerkonto zugeordnet ist. Sie können überprüfen, ob die twilio-Nachricht gesendet wurde, indem Sie das twilio-Dashboard anzeigen.
15. In wenigen Sekunden erhält das dem Benutzerkonto zugeordnete Telefon eine Textnachricht mit dem Überprüfungs Code. Geben Sie den Überprüfungs Code ein, und drücken Sie **Submit**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Aktivieren der zweistufigen Authentifizierung für einen registrierten Benutzer

An diesem Punkt haben Sie die zweistufige Authentifizierung für Ihre App aktiviert. Damit ein Benutzer die zweistufige Authentifizierung verwenden kann, kann er einfach seine Einstellungen mithilfe der Benutzeroberfläche ändern. 

1. Als Benutzer Ihrer APP können Sie die zweistufige Authentifizierung für ihr bestimmtes Konto aktivieren, indem Sie auf der Navigationsleiste auf die Benutzer-ID (e-Mail-Alias) klicken, um die Seite **Konto verwalten** anzuzeigen. Klicken Sie dann auf den Link " **aktivieren** ", um die zweistufige Authentifizierung für das Konto zu aktivieren.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Melden Sie sich ab, und melden Sie sich erneut an. Wenn Sie e-Mail aktiviert haben, können Sie für die zweistufige Authentifizierung entweder SMS oder e-Mail auswählen. Wenn Sie keine e-Mail aktiviert haben, finden Sie weitere Informationen im Tutorial [Erstellen einer Secure ASP.net Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kenn Wort](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)Zurücksetzung.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Die Seite für die zweistufige Authentifizierung wird angezeigt, auf der Sie den Code (von SMS oder e-Mail) eingeben können.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Wenn Sie auf das Kontrollkästchen **diesen Browser speichern** klicken, ist es nicht erforderlich, dass Sie die zweistufige Authentifizierung für die Anmeldung verwenden, wenn Sie den Browser und das Gerät verwenden, auf dem Sie das Kontrollkästchen aktiviert haben. Solange böswillige Benutzer keinen Zugriff auf Ihr Gerät haben, können Sie durch Aktivieren der zweistufigen Authentifizierung und durch Klicken auf den Benutzer, den Sie **merken** , einen bequemen Kenn Wort Zugriff bereitstellen, während Sie weiterhin einen sicheren Schutz für die zweistufige Authentifizierung für den gesamten Zugriff von nicht vertrauenswürdigen Geräten erhalten. Sie können auf allen privaten Geräten dazu, die Sie regelmäßig verwenden.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Zweistufige Authentifizierung mithilfe von SMS und E-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Links zu ASP.net Identity empfohlenen Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Bereitstellen einer sicheren ASP.net-Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.net Web Forms-tutorialreihe: Hinzufügen eines OAuth 2,0-Anbieters](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.net Web Forms-tutorialreihe: Aktivieren von SSL für das Projekt](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Erstellen der app in Facebook und Verbinden der APP mit dem Projekt](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
