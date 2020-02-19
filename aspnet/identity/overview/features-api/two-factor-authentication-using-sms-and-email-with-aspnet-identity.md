---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Zweistufige Authentifizierung mit SMS und e-Mail mit ASP.net Identity-ASP.NET 4. x
author: HaoK
description: In diesem Tutorial wird gezeigt, wie Sie die zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail einrichten. Dieser Artikel wurde von Rick Anderson (@RickAndMSFT), PR...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 527b4392846e60dae0b216fdeabf21fd6618e4d7
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456737"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a>Zweistufige Authentifizierung mit SMS und e-Mail mit ASP.net Identity

von [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> In diesem Tutorial wird gezeigt, wie Sie die zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail einrichten.
> 
> Dieser Artikel wurde von Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung und Suhas Joshi verfasst. Das nuget-Beispiel wurde hauptsächlich von Hao Kung geschrieben.

In diesem Thema werden folgende Themen behandelt:

- [Beispiel zum Aufbau der Identität](#build)
- [Einrichten von SMS für die zweistufige Authentifizierung](#SMS)
- [Aktivieren der zweistufigen Authentifizierung](#enable2)
- [So registrieren Sie einen Anbieter für die zweistufige Authentifizierung](#reg)
- [Kombinieren von sozialen und lokalen Anmeldekonten](#combine)
- [Kontosperrung durch Brute-Force-Angriffe](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Beispiel zum Aufbau der Identität

In diesem Abschnitt verwenden Sie nuget, um ein Beispiel herunterzuladen, mit dem wir arbeiten werden. Beginnen Sie mit der Installation und Ausführung von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.

> [!NOTE]
> Warnung: Sie müssen Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) installieren, um dieses Tutorial abzuschließen.

1. Erstellen Sie ein neues ***leeres*** ASP.NET-Webprojekt.
2. Geben Sie in der Paket-Manager-Konsole die folgenden Befehle ein:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   In diesem Tutorial verwenden wir [sendgrid](http://sendgrid.com/) zum Senden von e-Mail und [twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) für SMS-Texting. Das `Identity.Samples` Paket installiert den Code, mit dem wir arbeiten werden.
3. Legen Sie fest, dass das [Projekt SSL verwendet](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Optional*: Befolgen Sie die Anweisungen in meinem [e-Mail-Bestätigungs-Tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) , um sendgrid zu verbinden und dann die APP auszuführen und ein e-Mail-Konto zu registrieren
5. *Optional:* Entfernen Sie den Bestätigungscode für den e-Mail-Link aus dem Beispiel (der `ViewBag.Link` Code im Konto Controller. Weitere Informationen finden Sie in den `DisplayEmail`-und `ForgotPasswordConfirmation` Aktionsmethoden und Razor-Ansichten).
6. *Optional:* Entfernen Sie den `ViewBag.Status` Code aus den Manage-und Account-Controllern und aus den Razor-Ansichten *views\account\verifycode.cshtml* und *views\manage\verifyphonenumber.cshtml* . Alternativ dazu können Sie den `ViewBag.Status` anzeigen lassen, um zu testen, wie diese APP lokal funktioniert, ohne e-Mails und SMS-Nachrichten anschließen und senden zu müssen.

> [!NOTE]
> Warnung: Wenn Sie eine der Sicherheitseinstellungen in diesem Beispiel ändern, müssen die Produktions-apps eine Sicherheitsüberprüfung durchlaufen, mit der die vorgenommenen Änderungen explizit aufgerufen werden.

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
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresse:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Ermitteln der Benutzer Anmelde Informationen für den SMS-Anbieter**  
  
   Twilio  
   Kopieren Sie die **Konto-SID** und das Authentifizierungs **Token**auf der Registerkarte **Dashboard** Ihres twilio-Kontos.  
  
   Aspsms:  
   Navigieren Sie in Ihren Kontoeinstellungen zu **UserKey** , und kopieren Sie Sie mit Ihrem selbst definierten **Kennwort**.  
  
   Diese Werte werden später in den Variablen `SMSAccountIdentification` und `SMSAccountPassword` gespeichert.
4. **Angeben von SenderID/Absender**  
  
   Twilio  
   Kopieren Sie Ihre twilio-Telefonnummer auf der Registerkarte **Zahlen** .  
  
   Aspsms:  
   Entsperren Sie einen oder mehrere Originatoren im Menü **Unlock** -Absender, oder wählen Sie einen alphanumerischen Absender aus (wird nicht von allen Netzwerken unterstützt).  
  
   Dieser Wert wird später in der Variablen `SMSAccountFrom` gespeichert.
5. **Übertragen der Anmelde Informationen für den SMS-Anbieter**  
  
   Stellen Sie die Anmelde Informationen und die Absender Telefonnummer für die APP zur Verfügung:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. Das Konto und die Anmelde Informationen werden dem obigen Code hinzugefügt, um das Beispiel einfach zu halten. Weitere Informationen finden Sie unter Jon Atten es [ASP.NET MVC: Private Settings out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementierung der Datenübertragung an den SMS-Anbieter**  
  
   Konfigurieren Sie die `SmsService`-Klasse in der *App\_start\identityconfig.cs* -Datei.  
  
   Aktivieren Sie je nach verwendetem SMS-Anbieter entweder den **twilio** -oder den **ASPSMS** -Abschnitt: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Führen Sie die APP aus, und melden Sie sich mit dem zuvor registrierten Konto an.
8. Klicken Sie auf Ihre Benutzer-ID, die die `Index` Aktionsmethode in `Manage` Controller aktiviert.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Klicken Sie auf Hinzufügen.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. In wenigen Sekunden erhalten Sie eine Textnachricht mit dem Überprüfungs Code. Geben Sie ihn ein, **und drücken Sie die Eingabe**  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. In der Ansicht verwalten wird angezeigt, dass Ihre Telefonnummer hinzugefügt wurde.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Untersuchen des Codes

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Mit der `Index` Aktionsmethode in `Manage` Controller wird die Statusmeldung basierend auf der vorherigen Aktion festgelegt, und es werden Links zum Ändern des lokalen Kennworts oder zum Hinzufügen eines lokalen Kontos bereitstellen. Die `Index`-Methode zeigt außerdem den Status oder Ihre 2FA-Telefonnummer, externe Anmeldungen, 2FA-aktiviert und die 2FA-Methode für diesen Browser an (später erläutert). Wenn Sie auf die Benutzer-ID (e-Mail) in der Titelleiste klicken, wird keine Nachricht übergeben. Wenn Sie auf den Link **Telefonnummer: Entfernen** klicken, wird `Message=RemovePhoneSuccess` als Abfrage Zeichenfolge weitergeleitet.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld an, in dem Sie eine Telefonnummer eingeben können, die SMS-Nachrichten empfangen kann.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Wenn Sie auf die Schaltfläche **Überprüfungs Code senden** klicken, wird die Telefonnummer an die HTTP Post-`AddPhoneNumber` Aktionsmethode gesendet.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Die `GenerateChangePhoneNumberTokenAsync`-Methode generiert das Sicherheits Token, das in der SMS-Nachricht festgelegt wird. Wenn der SMS-Dienst konfiguriert wurde, wird das Token als Zeichenfolge gesendet, &quot;Ihr Sicherheitscode &lt;Token&gt;&quot;ist. Die `SmsService.SendAsync`-Methode wird asynchron aufgerufen. Anschließend wird die APP an die `VerifyPhoneNumber` Aktionsmethode umgeleitet (in der das folgende Dialogfeld angezeigt wird), in der Sie den Überprüfungs Code eingeben können.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Nachdem Sie den Code eingegeben und auf "Senden" klicken, wird der Code an die HTTP Post-`VerifyPhoneNumber` Aktionsmethode gesendet.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Die `ChangePhoneNumberAsync`-Methode überprüft den veröffentlichten Sicherheitscode. Wenn der Code korrekt ist, wird die Telefonnummer dem `PhoneNumber`-Feld der `AspNetUsers` Tabelle hinzugefügt. Wenn dieser Aufruf erfolgreich ist, wird die `SignInAsync`-Methode aufgerufen:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Der `isPersistent`-Parameter legt fest, ob die Authentifizierungs Sitzung über mehrere Anforderungen hinweg beibehalten wird.

Wenn Sie Ihr Sicherheitsprofil ändern, wird ein neuer Sicherheits Stempel generiert und im Feld "`SecurityStamp`" der Tabelle " *aspnettusers* " gespeichert. Beachten Sie, dass sich das `SecurityStamp` Feld von dem Sicherheits Cookie unterscheidet. Das Sicherheits Cookie wird nicht in der `AspNetUsers` Tabelle (oder an einer anderen Stelle in der Identitätsdatenbank) gespeichert. Das sicherheitstokentoken wird mithilfe von [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) selbst signiert und mit den `UserId, SecurityStamp`-und Ablaufzeit Informationen erstellt.

Die Cookie-Middleware überprüft das Cookie für jede Anforderung. Die `SecurityStampValidator`-Methode in der `Startup`-Klasse trifft auf die Datenbank und überprüft den Sicherheits Stempel in regelmäßigen Abständen, wie im `validateInterval`angegeben. Dies geschieht nur alle 30 Minuten (in unserem Beispiel), es sei denn, Sie ändern das Sicherheitsprofil. Das Intervall von 30 Minuten wurde gewählt, um die Fahrten zur Datenbank zu minimieren.

Die `SignInAsync`-Methode muss aufgerufen werden, wenn Änderungen am Sicherheitsprofil vorgenommen werden. Wenn sich das Sicherheitsprofil ändert, aktualisiert die Datenbank das `SecurityStamp` Feld, und ohne die `SignInAsync`-Methode Aufrufs, werden Sie *nur* angemeldet, wenn die owin-Pipeline das nächste Mal auf die Datenbank (die `validateInterval`) trifft. Sie können dies testen, indem Sie die `SignInAsync`-Methode so ändern, dass Sie sofort zurückgegeben wird, und die Eigenschaft Cookie `validateInterval` von 30 Minuten auf 5 Sekunden festlegen:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Nachdem Sie die obigen Codeänderungen vorgenommen haben, können Sie Ihr Sicherheitsprofil ändern (z. b. durch Ändern des Status von **zwei aktiviertem Faktor**), und Sie werden in fünf Sekunden abgemeldet, wenn die `SecurityStampValidator.OnValidateIdentity`-Methode fehlschlägt. Entfernen Sie die Rückgabe Zeile in der `SignInAsync`-Methode, nehmen Sie eine andere Sicherheitsprofil Änderung vor, und Sie werden nicht abgemeldet. Die `SignInAsync`-Methode generiert ein neues Sicherheits Cookie.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Zwei-Faktor-Authentifizierung aktivieren

In der Beispiel-App müssen Sie die Benutzeroberfläche verwenden, um die zweistufige Authentifizierung (2FA) zu aktivieren. Um 2FA zu aktivieren, klicken Sie auf der Navigationsleiste auf Ihre Benutzer-ID (e-Mail-Alias).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Klicken Sie auf 2 Fa aktivieren.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Melden Sie sich ab, und melden Sie sich erneut an. Wenn Sie e-Mail aktiviert haben (siehe mein [Vorheriges Tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail für 2FA auswählen.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Die Seite Code überprüfen wird angezeigt, auf der Sie den Code (von SMS oder e-Mail) eingeben können.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Wenn Sie auf das Kontrollkästchen **diesen Browser speichern** klicken, werden Sie von der Verwendung von 2FA zum Anmelden mit diesem Computer und Browser ausgeschlossen. Durch das Aktivieren von 2FA und das Klicken auf das **Speichern dieses Browsers** erhalten Sie einen starken 2FA-Schutz vor böswilligen Benutzern, die versuchen, auf Ihr Konto zuzugreifen, solange Sie keinen Zugriff auf Ihren Computer haben. Sie können dies auf jedem beliebigen privaten Computer durchführen, den Sie regelmäßig verwenden. Wenn Sie **diesen Browser merken**, erhalten Sie die zusätzliche Sicherheit von 2FA von Computern, die Sie nicht regelmäßig verwenden, und Sie erhalten die Möglichkeit, die 2FA nicht auf Ihren eigenen Computern zu durchlaufen. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>So registrieren Sie einen Anbieter für die zweistufige Authentifizierung

Wenn Sie ein neues MVC-Projekt erstellen, enthält die Datei *IdentityConfig.cs* den folgenden Code, um einen Anbieter für die zweistufige Authentifizierung zu registrieren:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Hinzufügen einer Telefonnummer für 2FA

Die `AddPhoneNumber` Aktionsmethode im `Manage` Controller generiert ein Sicherheits Token und sendet es an die angegebene Telefonnummer.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Nachdem das Token gesendet wurde, wird es an die `VerifyPhoneNumber` Aktionsmethode umgeleitet. dort können Sie den Code zum Registrieren von SMS für 2FA eingeben. SMS 2FA wird erst verwendet, wenn Sie die Telefonnummer überprüft haben.

## <a name="enabling-2fa"></a>Aktivieren von 2FA

Die `EnableTFA` Aktionsmethode aktiviert 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Beachten Sie, dass die `SignInAsync` aufgerufen werden muss, da die Option "2FA aktivieren" eine Änderung am Sicherheitsprofil ist. Wenn 2FA aktiviert ist, muss sich der Benutzer für die Anmeldung mit 2FA anmelden, und zwar mit den von Ihnen registrierten 2FA-Ansätzen (SMS und e-Mail im Beispiel).

Sie können weitere 2FA-Anbieter hinzufügen, z. b. QR-Code-Generatoren, oder Sie können Sie als Besitzer schreiben (siehe [Verwenden von Google Authenticator mit ASP.net Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Die 2FA-Codes werden mithilfe des [zeitbasierten einmaligen Kenn Wort Algorithmus](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) generiert, und die Codes sind sechs Minuten gültig. Wenn Sie mehr als sechs Minuten für die Eingabe des Codes benötigen, erhalten Sie eine ungültige Code Fehlermeldung.

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Kombinieren von sozialen und lokalen Anmeldekonten

Sie können lokale und soziale Konten kombinieren, indem Sie auf Ihren e-Mail-Link klicken. In der folgenden Reihenfolge &quot;RickAndMSFT@gmail.com&quot; zuerst als lokale Anmeldung erstellt, aber Sie können zuerst das Konto als ein soziales Protokoll erstellen und dann einen lokalen Anmelde Namen hinzufügen.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Klicken Sie auf den Link **Verwalten** . Beachten Sie die 0 (null) externen (Anmeldungen), die diesem Konto zugeordnet sind.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Klicken Sie auf den Link zu einem anderen Dienst Protokoll, und akzeptieren Sie die APP-Anforderungen. Die beiden Konten wurden kombiniert, Sie können sich mit jedem Konto anmelden. Möglicherweise möchten Sie, dass Ihre Benutzer lokale Konten hinzufügen, falls Ihr soziales Protokoll im Authentifizierungsdienst ausgefallen ist, oder wahrscheinlich, dass Sie den Zugriff auf Ihr Social Media-Konto verloren haben.

In der folgenden Abbildung ist Tom ein soziales Protokoll in (das Sie über die **externen Anmeldungen sehen können: 1** auf der Seite angezeigt).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Wenn Sie auf " **Kennwort auswählen** " klicken, können Sie ein lokales Protokoll hinzufügen, das demselben Konto zugeordnet ist.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Kontosperrung durch Brute-Force-Angriffe

Sie können die Konten in Ihrer APP vor Wörterbuchangriffen schützen, indem Sie die Benutzer Sperrung aktivieren. Der folgende Code in der `ApplicationUserManager Create`-Methode konfiguriert Lock out:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Der obige Code ermöglicht nur eine Sperre für die zweistufige Authentifizierung. Obwohl Sie die Sperre für Anmeldungen aktivieren können, indem Sie `shouldLockout` in der `Login`-Methode des Konto Controllers in "true" ändern, empfiehlt es sich, die Sperre für Anmeldungen nicht zu aktivieren, da das Konto anfällig für [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) -Anmelde Angriffe ist. Im Beispielcode ist die Sperre für das in der `ApplicationDbInitializer Seed`-Methode erstellte Administrator Konto deaktiviert:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Erfordern eines überprüften e-Mail-Kontos für einen Benutzer

Der folgende Code erfordert, dass ein Benutzer über ein validiertes e-Mail-Konto verfügt, bevor er sich anmelden kann

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Überprüfen von signinmanager auf die 2FA-Anforderung

Sowohl das lokale Protokoll als auch das Protokoll für soziale Netzwerke werden überprüft, um festzustellen, ob 2FA aktiviert ist. Wenn 2FA aktiviert ist, gibt die `SignInManager` Logon-Methode `SignInStatus.RequiresVerification`zurück, und der Benutzer wird an die `SendCode` Aktionsmethode umgeleitet, wo Sie den Code eingeben müssen, um das Protokoll nacheinander abzuschließen. Wenn der Benutzer für das lokale Cookie "Users" eine Erinnerung festgelegt hat, gibt die `SignInManager` `SignInStatus.Success` zurück, und Sie müssen nicht die 2FA durchlaufen.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Der folgende Code zeigt die `SendCode` Aktionsmethode. Eine [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) wird mit allen für den Benutzer aktivierten 2FA-Methoden erstellt. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) wird an das Hilfsprogramm [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) übergeben, das es dem Benutzer ermöglicht, den 2FA-Ansatz (in der Regel e-Mail und SMS) auszuwählen.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Sobald der Benutzer den 2FA-Ansatz veröffentlicht hat, wird die `HTTP POST SendCode` Aktionsmethode aufgerufen, der `SignInManager` den 2FA-Code sendet, und der Benutzer wird an die `VerifyCode` Aktionsmethode umgeleitet, wo er den Code zum Abschließen der Anmeldung eingeben kann.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA-Sperre

Obwohl Sie die Kontosperrung bei Fehlern beim Versuch eines Anmelde Kennworts festlegen können, ist der Anmelde Name für die [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) -Sperrung anfällig. Es wird empfohlen, die Kontosperrung nur mit 2FA zu verwenden. Wenn die `ApplicationUserManager` erstellt wird, legt der Beispielcode die 2FA-Sperre fest und `MaxFailedAccessAttemptsBeforeLockout` auf fünf. Sobald sich ein Benutzer anmeldet (über ein lokales Konto oder ein Social Media-Konto), wird jeder fehlgeschlagene Versuch bei 2FA gespeichert. wenn die maximale Anzahl von versuchen erreicht ist, wird der Benutzer fünf Minuten lang gesperrt (Sie können die Sperr Zeit mit `DefaultAccountLockoutTimeSpan`festlegen).

<a id="addRes"></a>

## <a name="additional-resources"></a>Weitere Ressourcen

- [Empfohlene Ressourcen ASP.net Identity](../getting-started/aspnet-identity-recommended-resources.md) Eine umfassende Liste mit Identitäts Blogs, Videos, Tutorials und tollen Links.
- Die [MVC 5-App mit Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) zeigt außerdem, wie Sie der Tabelle "Users" Profilinformationen hinzufügen.
- [ASP.NET MVC und Identity 2,0: Grundlegendes zu den Grundlagen](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) von John Atten.
- [Konto Bestätigung und Kenn Wort Wiederherstellung mit ASP.net Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Einführung zu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ankündigungs-RTM von ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) von Pranav Rastogi.
- [ASP.net Identity 2,0: Einrichten der Konto Validierung und der zweistufigen Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) durch John Atten.
