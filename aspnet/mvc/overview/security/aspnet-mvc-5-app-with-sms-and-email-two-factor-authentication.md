---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: ASP.NET MVC 5-App mit zweistufiger SMS-und e-Mail-Authentifizierung | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit zweistufiger Authentifizierung erstellen. Erstellen Sie eine sichere ASP.NET MVC 5-Web-App mit...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457595"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>ASP.NET MVC 5-App mit zweistufiger Authentifizierung per SMS und E-Mail

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-Web-App mit zweistufiger Authentifizierung erstellen. [Erstellen Sie eine sichere ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Zurücksetzung, bevor Sie den Vorgang fortsetzen. Sie können die abgeschlossene Anwendung [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)herunterladen. Der Download enthält debughilfsprogramme, mit denen Sie e-Mail-Bestätigung und SMS testen können, ohne eine e-Mail oder einen SMS-Anbieter einzurichten.
> 
> Dieses Tutorial wurde von [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ) verfasst.

- [Erstellen einer ASP.NET-MVC-App](#createMvc)
- [Einrichten von SMS für die zweistufige Authentifizierung](#SMS)
- [Aktivieren der zweistufigen Authentifizierung](#enable2)
- [Weitere Ressourcen](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Erstellen einer ASP.NET-MVC-App

Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: [Erstellen Sie eine Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Zurücksetzung, bevor Sie den Vorgang fortsetzen. Sie müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher installieren, um dieses Tutorial abzuschließen.

1. Erstellen Sie ein neues ASP.NET-Webprojekt, und wählen Sie die MVC-Vorlage aus. Web Forms auch ASP.net Identity unterstützt, können Sie ähnliche Schritte in einer Web Forms-app ausführen.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Überlassen Sie die Standard Authentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert. Später in diesem Tutorial wird die Bereitstellung in Azure durchzuführen. Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Legen Sie fest, dass das [Projekt SSL verwendet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Einrichten von SMS für die zweistufige Authentifizierung

Dieses Tutorial enthält Anweisungen zur Verwendung von twilio oder ASPSMS, aber Sie können auch einen beliebigen anderen SMS-Anbieter verwenden.

1. **Erstellen eines Benutzerkontos mit einem SMS-Anbieter**  
  
   Erstellen Sie ein [twilio](https://www.twilio.com/try-twilio) -oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) -Konto.
2. **Installieren zusätzlicher Pakete oder Hinzufügen von Dienst verweisen**  
  
   Twilio  
   Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:  
    `Install-Package Twilio`  
  
   Aspsms:  
   Der folgende Dienst Verweis muss hinzugefügt werden:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresse:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Ermitteln der Benutzer Anmelde Informationen für den SMS-Anbieter**  
  
   Twilio  
   Kopieren Sie die **Konto-SID** und das Authentifizierungs **Token**auf der Registerkarte **Dashboard** Ihres twilio-Kontos.  
  
   Aspsms:  
   Navigieren Sie in Ihren Kontoeinstellungen zu **UserKey** , und kopieren Sie Sie mit Ihrem selbst definierten **Kennwort**.  
  
   Diese Werte werden später in der Datei " *Web. config* " in den Schlüsseln `"SMSAccountIdentification"` und `"SMSAccountPassword"` gespeichert.
4. **Angeben von SenderID/Absender**  
  
   Twilio  
   Kopieren Sie Ihre twilio-Telefonnummer auf der Registerkarte **Zahlen** .  
  
   Aspsms:  
   Entsperren Sie einen oder mehrere Originatoren im Menü **Unlock** -Absender, oder wählen Sie einen alphanumerischen Absender aus (wird nicht von allen Netzwerken unterstützt).  
  
   Dieser Wert wird später in der Datei " *Web. config* " im Schlüssel `"SMSAccountFrom"` gespeichert.
5. **Übertragen der Anmelde Informationen für den SMS-Anbieter**  
  
   Stellen Sie die Anmelde Informationen und die Absender Telefonnummer für die APP zur Verfügung. Um dies zu gewährleisten, werden diese Werte in der Datei " *Web. config* " gespeichert. Wenn die Bereitstellung in Azure durchzuführen ist, können wir die Werte im Abschnitt " **App-Einstellungen** " auf der Registerkarte "Website konfigurieren" sicher speichern. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. Das Konto und die Anmelde Informationen werden dem obigen Code hinzugefügt, um das Beispiel einfach zu halten. Weitere Informationen finden [Sie unter Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
6. **Implementierung der Datenübertragung an den SMS-Anbieter**  
  
   Konfigurieren Sie die `SmsService`-Klasse in der *App\_start\identityconfig.cs* -Datei.  
  
   Aktivieren Sie je nach verwendetem SMS-Anbieter entweder den **twilio** -oder den **ASPSMS** -Abschnitt: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aktualisieren Sie die Razor-Ansicht *views\manage\index.cshtml* : (Hinweis: Entfernen Sie nicht nur die Kommentare im beendenden Code, und verwenden Sie den folgenden Code.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Überprüfen Sie, ob die `EnableTwoFactorAuthentication`-und `DisableTwoFactorAuthentication` Aktionsmethoden in der `ManageController` über das[[validateantiforgerytoken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) -Attribut verfügen:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Führen Sie die APP aus, und melden Sie sich mit dem zuvor registrierten Konto an.
10. Klicken Sie auf Ihre Benutzer-ID, die die `Index` Aktionsmethode in `Manage` Controller aktiviert.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Klicken Sie auf Hinzufügen.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld an, in dem Sie eine Telefonnummer eingeben können, die SMS-Nachrichten empfangen kann.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. In wenigen Sekunden erhalten Sie eine Textnachricht mit dem Überprüfungs Code. Geben Sie ihn ein, **und drücken Sie die Eingabe**  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. In der Ansicht verwalten wird angezeigt, dass Ihre Telefonnummer hinzugefügt wurde.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Zwei-Faktor-Authentifizierung aktivieren

In der von der Vorlage generierten App müssen Sie die Benutzeroberfläche verwenden, um die zweistufige Authentifizierung (2FA) zu aktivieren. Um 2FA zu aktivieren, klicken Sie auf der Navigationsleiste auf Ihre Benutzer-ID (e-Mail-Alias).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Klicken Sie auf 2 Fa aktivieren.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Melden Sie sich ab, und melden Sie sich erneut an. Wenn Sie e-Mail aktiviert haben (siehe mein [Vorheriges Tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail für 2FA auswählen.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Die Seite Code überprüfen wird angezeigt, auf der Sie den Code (von SMS oder e-Mail) eingeben können.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Wenn Sie auf das Kontrollkästchen **diesen Browser speichern** klicken, werden Sie von der Verwendung von 2FA für die Anmeldung aufgefordert, wenn Sie den Browser und das Gerät verwenden, auf dem Sie das Kontrollkästchen aktiviert haben. Solange böswillige Benutzer nicht auf Ihr Gerät zugreifen können, wird durch das Aktivieren von 2FA und das Klicken auf den Benutzer, der **sich in diesem Browser** befindet, ein bequemer Zugriff auf das Kennwort bereitgestellt, während gleichzeitig ein starker 2FA-Schutz für den gesamten Zugriff von nicht vertrauenswürdigen Geräten beibehalten wird Sie können auf allen privaten Geräten dazu, die Sie regelmäßig verwenden.

Dieses Tutorial enthält eine kurze Einführung in die Aktivierung von 2FA für eine neue ASP.NET MVC-app. In meinem Tutorial wird die [zweistufige Authentifizierung mit SMS und e-Mail mit ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) ausführlich zum Code hinter dem Beispiel behandelt.

<a id="addRes"></a>
## <a name="additional-resources"></a>Weitere Ressourcen

- [Zweistufige Authentifizierung mit SMS und e-Mail mit ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Detaillierte Informationen zur zweistufigen Authentifizierung
- [Links zu ASP.net Identity empfohlenen Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Weitere Informationen zur Kenn Wort Wiederherstellung und zur Konto Bestätigung.
- [MVC 5-App mit Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 5-App mit Facebook-und Google OAuth 2-Autorisierung schreiben. Außerdem wird gezeigt, wie Sie der Identitätsdatenbank zusätzliche Daten hinzufügen.
- Stellen [Sie eine sichere ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank im Azure-Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit. In diesem Tutorial wird die Azure-Bereitstellung hinzugefügt, die Sicherheit Ihrer APP mit Rollen, die Verwendung der Mitgliedschafts-API zum Hinzufügen von Benutzern und Rollen sowie zusätzliche Sicherheitsfeatures erläutert.
- [Erstellen einer Google-App für OAuth 2 und Verbinden der APP mit dem Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Erstellen der app in Facebook und Verbinden der APP mit dem Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Einrichten von SSL im Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Einrichten der C# Entwicklungsumgebung für und ASP.NET MVC](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
