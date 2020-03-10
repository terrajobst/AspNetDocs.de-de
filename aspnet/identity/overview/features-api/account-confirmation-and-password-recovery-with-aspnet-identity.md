---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Konto Bestätigung & Kenn Wort Wiederherstellung-C#ASP.net Identity ()-ASP.NET 4. x
author: HaoK
description: Vor der Durchführung dieses Tutorials sollten Sie zunächst Erstellen einer Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort Zurücksetzung Fertigstellen. Dieses Tutorial...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b2c88280df39aa81d60f9508910e8fe5d6db6b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499983"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Konto Bestätigung und Kenn Wort Wiederherstellung mitC#ASP.net Identity ()

> Vor der Durchführung dieses Tutorials sollten Sie zunächst [Erstellen einer Secure ASP.NET MVC 5-Web-App mit Anmeldung, e-Mail-Bestätigung und Kenn Wort](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)Zurücksetzung Fertigstellen. Dieses Tutorial enthält weitere Details und zeigt Ihnen, wie Sie e-Mail für die lokale Konto Bestätigung einrichten und Benutzern ermöglichen, Ihr vergessenes Kennwort in ASP.net Identity zurückzusetzen.

Ein lokales Benutzerkonto erfordert, dass der Benutzer ein Kennwort für das Konto erstellt, und dieses Kennwort wird (sicher) in der Web-App gespeichert. ASP.net Identity unterstützt auch Konten für soziale Netzwerke, die nicht erfordern, dass der Benutzer ein Kennwort für die App erstellt. [Konten für soziale](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Netzwerke verwenden einen Drittanbieter (z. b. Google, Twitter, Facebook oder Microsoft), um Benutzer zu authentifizieren. In diesem Thema werden folgende Themen behandelt:

- [Erstellen Sie eine ASP.NET MVC-App](#createMvc) , und entdecken Sie ASP.net Identity Features.
- [Erstellen des Identitäts Beispiels](#build)
- [E-Mail-Bestätigung einrichten](#email)

Neue Benutzer registrieren Ihren e-Mail-Alias, mit dem ein lokales Konto erstellt wird.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Wenn Sie die Schaltfläche registrieren auswählen, wird eine Bestätigungs-e-Mail mit einem Überprüfungs Token an Ihre e-Mail

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Dem Benutzer wird eine e-Mail mit einem Bestätigungs Token für sein Konto gesendet.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Wenn Sie den Link auswählen, wird das Konto bestätigt.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Kenn Wort Wiederherstellung/zurück Setzung

Lokale Benutzer, die Ihr Kennwort vergessen haben, können ein Sicherheits Token an Ihr e-Mail-Konto senden, sodass Sie Ihr Kennwort zurücksetzen können.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Der Benutzer erhält in Kürze eine e-Mail mit einem Link, über den die Benutzer sein Kennwort zurücksetzen können.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Wenn Sie den Link auswählen, gelangen Sie auf die Seite zurücksetzen.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Durch Auswählen der Schaltfläche **Zurücksetzen** wird bestätigt, dass das Kennwort zurückgesetzt wurde.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

Beginnen Sie mit der Installation und Ausführung von [Visual Studio 2017](https://visualstudio.microsoft.com/).

1. Erstellen Sie ein neues ASP.NET-Webprojekt, und wählen Sie die MVC-Vorlage aus. Web Forms auch ASP.net Identity unterstützen, sodass Sie ähnliche Schritte in einer Web Forms-app ausführen könnten.
2. Ändern Sie die Authentifizierung in **einzelne Benutzerkonten**.
3. Führen Sie die APP aus, wählen Sie den Link **registrieren** aus, und registrieren Sie einen Benutzer. An diesem Punkt wird nur die e-Mail-Überprüfung mit dem [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) -Attribut durchzuführen.
4. Navigieren Sie in Server-Explorer zu **Daten connections\defaultconnection\tables\aspnettusers**, klicken Sie mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**aus.

    Die folgende Abbildung zeigt das `AspNetUsers` Schema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Klicken Sie mit der rechten Maustaste auf die **aspnettusers** -Tabelle, und wählen Sie **Tabellendaten anzeigen**aus.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   An diesem Punkt wurde die e-Mail nicht bestätigt.

Der Standarddaten Speicher für ASP.net Identity ist Entity Framework, Sie können ihn jedoch so konfigurieren, dass er andere Datenspeicher verwendet und zusätzliche Felder hinzufügt. Weitere Informationen finden Sie im Abschnitt [Weitere Ressourcen](#addRes) am Ende dieses Tutorials.

Die [owin-Startklasse](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) wird aufgerufen, wenn die APP gestartet wird, und ruft die `ConfigureAuth`-Methode in *App\_Start\Startup.auth.cs*auf, die die owin-Pipeline konfiguriert und ASP.net Identity initialisiert. Untersuchen Sie die Methode `ConfigureAuth`. Jeder `CreatePerOwinContext` Aufruf registriert einen Rückruf (gespeichert im `OwinContext`), der einmal pro Anforderung aufgerufen wird, um eine Instanz des angegebenen Typs zu erstellen. Sie können einen Haltepunkt im Konstruktor und `Create` Methode jedes Typs (`ApplicationDbContext, ApplicationUserManager`) festlegen und überprüfen, ob Sie für jede Anforderung aufgerufen werden. Eine Instanz von `ApplicationDbContext` und `ApplicationUserManager` wird im owin-Kontext gespeichert, auf den in der gesamten Anwendung zugegriffen werden kann. ASP.net Identity in die owin-Pipeline über Cookie-Middleware. Weitere Informationen finden Sie unter [pro Verwaltung der Anforderungs Lebensdauer für die usermanager-Klasse in ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Wenn Sie Ihr Sicherheitsprofil ändern, wird ein neuer Sicherheits Stempel generiert und im Feld "`SecurityStamp`" der Tabelle " *aspnettusers* " gespeichert. Beachten Sie, dass sich das `SecurityStamp` Feld von dem Sicherheits Cookie unterscheidet. Das Sicherheits Cookie wird nicht in der `AspNetUsers` Tabelle (oder an einer anderen Stelle in der Identitätsdatenbank) gespeichert. Das sicherheitstokentoken wird mithilfe von [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) selbst signiert und mit den `UserId, SecurityStamp`-und Ablaufzeit Informationen erstellt.

Die Cookie-Middleware überprüft das Cookie für jede Anforderung. Die `SecurityStampValidator`-Methode in der `Startup`-Klasse trifft auf die Datenbank und überprüft den Sicherheits Stempel in regelmäßigen Abständen, wie im `validateInterval`angegeben. Dies geschieht nur alle 30 Minuten (in unserem Beispiel), es sei denn, Sie ändern das Sicherheitsprofil. Das Intervall von 30 Minuten wurde gewählt, um die Fahrten zur Datenbank zu minimieren. Weitere Informationen finden Sie im [Tutorial zur zweistufigen Authentifizierung](index.md) .

Gemäß den Kommentaren im Code unterstützt die `UseCookieAuthentication`-Methode die Cookie-Authentifizierung. Das Feld "`SecurityStamp`" und der zugehörige Code stellen eine zusätzliche Sicherheitsebene für Ihre APP bereit. Wenn Sie Ihr Kennwort ändern, werden Sie von dem Browser, mit dem Sie sich angemeldet haben, abgemeldet. Die `SecurityStampValidator.OnValidateIdentity`-Methode ermöglicht der APP, das Sicherheits Token zu validieren, wenn sich der Benutzer anmeldet. dieser wird verwendet, wenn Sie ein Kennwort ändern oder den externen Anmelde Namen verwenden. Dies ist erforderlich, um sicherzustellen, dass alle mit dem alten Kennwort generierten Token (Cookies) für ungültig erklärt werden. Wenn Sie im Beispiel Projekt das Benutzer Kennwort ändern, wird ein neues Token für den Benutzer generiert, vorherige Token werden für ungültig erklärt, und das `SecurityStamp` Feld wird aktualisiert.

Mit dem Identitätssystem können Sie Ihre APP so konfigurieren, dass sich das Sicherheitsprofil des Benutzers ändert (z. b. wenn der Benutzer sein Kennwort ändert oder die zugehörige Anmeldung ändert (z. b. von Facebook, Google, Microsoft-Konto usw.), dass der Benutzer von allen Benutzern abgemeldet wird. Browser Instanzen. Die folgende Abbildung zeigt beispielsweise die Beispiel-App für [einmaliges Anmelden](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) , mit der der Benutzer sich von allen Browser Instanzen (in diesem Fall IE, Firefox und Chrome) abmelden kann, indem er eine Schaltfläche auswählt. Alternativ können Sie mit dem Beispiel nur von einer bestimmten Browser Instanz abmelden.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

Die Beispiel-App für [einmaliges Anmelden](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) zeigt, wie Sie mit ASP.net Identity das Sicherheits Token erneut generieren können. Dies ist erforderlich, um sicherzustellen, dass alle mit dem alten Kennwort generierten Token (Cookies) für ungültig erklärt werden. Diese Funktion bietet eine zusätzliche Sicherheitsebene für Ihre Anwendung. Wenn Sie Ihr Kennwort ändern, werden Sie abgemeldet, wo Sie sich bei dieser Anwendung angemeldet haben.

Die *App\_start\identityconfig.cs* -Datei enthält die Klassen `ApplicationUserManager`, `EmailService` und `SmsService`. Die Klassen `EmailService` und `SmsService` implementieren jeweils die `IIdentityMessageService`-Schnittstelle, sodass Sie in jeder Klasse allgemeine Methoden zum Konfigurieren von e-Mail und SMS haben. Obwohl in diesem Tutorial nur das Hinzufügen von e-Mail-Benachrichtigungen über [sendgrid](http://sendgrid.com/)veranschaulicht wird, können Sie e-Mails mithilfe von SMTP und anderen Mechanismen senden.

Die `Startup`-Klasse enthält auch die kesselplatte zum Hinzufügen von Anmeldungen für soziale Netzwerke (Facebook, Twitter usw.). Weitere Informationen finden Sie unter My Tutorial [MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) .

Überprüfen Sie die `ApplicationUserManager`-Klasse, die die Benutzer Identitätsinformationen enthält, und konfigurieren Sie die folgenden Funktionen:

- Anforderungen an die Kenn Wort Stärke.
- Benutzer Sperre (Versuche und Uhrzeit).
- Zweistufige Authentifizierung (2FA). Ich werde 2FA und SMS in einem anderen Tutorial behandeln.
- Einbinden der e-Mail-und SMS-Dienste (Ich werde SMS in einem anderen Tutorial behandeln).

Die `ApplicationUserManager`-Klasse wird von der generischen `UserManager<ApplicationUser>`-Klasse abgeleitet. `ApplicationUser` von [identityuser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx)abgeleitet. `IdentityUser` abgeleitet von der generischen `IdentityUser` Klasse:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Die oben aufgeführten Eigenschaften stimmen mit den Eigenschaften in der `AspNetUsers` Tabelle überein.

Generische Argumente für `IUser` ermöglichen das Ableiten einer Klasse mit unterschiedlichen Typen für den Primärschlüssel. Weitere Informationen zum Ändern des Primärschlüssels von der Zeichenfolge in "int" oder "GUID" finden Sie im Beispiel " [changepk](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) ".

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) ist in " *models\identitymodels.cs* " wie folgt definiert:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Der oben markierte Code generiert eine " [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)". ASP.net Identity-und owin-Cookieauthentifizierung sind Anspruchs basiert. Daher erfordert das Framework, dass die APP eine `ClaimsIdentity` für den Benutzer generiert. `ClaimsIdentity` enthält Informationen über alle Ansprüche für den Benutzer, z. b. den Namen des Benutzers, das Alter und die Rollen, denen der Benutzer angehört. Sie können in dieser Phase auch weitere Ansprüche für den Benutzer hinzufügen.

Die owin-`AuthenticationManager.SignIn` Methode übergibt die `ClaimsIdentity` und signiert den Benutzer:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (Anmelden von MVC 5](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ) zeigt, wie Sie der `ApplicationUser`-Klasse zusätzliche Eigenschaften hinzufügen können.

## <a name="email-confirmation"></a>Bestätigung per e-Mail

Es empfiehlt sich, die e-Mail zu bestätigen, bei der ein neuer Benutzer registriert ist, um sicherzustellen, dass die Identität eines anderen Benutzers nicht angenommen wird (d. h., Sie haben sich nicht bei einer anderen e-Mail registriert). Angenommen, Sie hatten ein Diskussionsforum, Sie sollten verhindern, dass sich `"bob@example.com"` als `"joe@contoso.com"`registrieren. Ohne e-Mail-Bestätigung können `"joe@contoso.com"` unerwünschte e-Mails von Ihrer APP erhalten. Wenn Bob versehentlich als `"bib@example.com"` registriert ist und es nicht bemerkt hat, wäre er nicht in der Lage, die Kenn Wort Wiederherstellung zu verwenden, da die APP nicht über die richtige e-Mail verfügt. E-Mail-Bestätigung bietet nur eingeschränkten Schutz vor Bots und bietet keinen Schutz vor ermittelten Spammern. Sie verfügen über viele funktionierende e-Mail-Aliase, die Sie zum Registrieren verwenden können. Im folgenden Beispiel kann der Benutzer sein Kennwort erst ändern, wenn das zugehörige Konto bestätigt wurde (indem er einen Bestätigungslink für das e-Mail-Konto ausgewählt hat, bei dem er registriert wurde). Sie können diesen Workflow auf andere Szenarien anwenden, z. b. das Senden eines Links zum bestätigen und Zurücksetzen des Kennworts für neue Konten, die vom Administrator erstellt wurden, und dem Benutzer eine e-Mail senden, wenn er das Profil geändert hat. In der Regel möchten Sie, dass neue Benutzerdaten auf Ihrer Website veröffentlichen, bevor Sie per e-Mail, SMS-SMS oder einem anderen Mechanismus bestätigt wurden. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Erstellen Sie ein ausführlichere Beispiel

In diesem Abschnitt verwenden Sie nuget, um ein ausführeres Beispiel herunterzuladen, mit dem wir arbeiten werden.

1. Erstellen Sie ein neues ***leeres*** ASP.NET-Webprojekt.
2. Geben Sie in der Paket-Manager-Konsole die folgenden Befehle ein: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   In diesem Tutorial verwenden wir [sendgrid](http://sendgrid.com/) , um e-Mails zu senden. Das `Identity.Samples` Paket installiert den Code, mit dem wir arbeiten werden.
3. Legen Sie fest, dass das [Projekt SSL verwendet](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Testen Sie die Erstellung des lokalen Kontos, indem Sie die app ausführen, den Link **registrieren** auswählen und das Registrierungsformular veröffentlichen.
5. Wählen Sie den Link für eine e-Mail aus, der die e-Mail-Bestätigung simuliert.
6. Entfernen Sie den Bestätigungscode für den e-Mail-Link aus dem Beispiel (der `ViewBag.Link` Code im Konto Controller. Weitere Informationen finden Sie in den `DisplayEmail`-und `ForgotPasswordConfirmation` Aktionsmethoden und Razor-Ansichten).

> [!WARNING]
> Wenn Sie die Sicherheitseinstellungen in diesem Beispiel ändern, müssen die Produktions-apps eine Sicherheitsüberprüfung durchlaufen, die explizit die vorgenommenen Änderungen aufruft.

## <a name="examine-the-code-in-app_startidentityconfigcs"></a>Untersuchen Sie den Code in App\_start\identityconfig.cs

Das Beispiel zeigt, wie Sie ein Konto erstellen und es der *Administrator* Rolle hinzufügen. Sie sollten die e-Mail im Beispiel durch die e-Mail ersetzen, die Sie für das Administrator Konto verwenden. Die einfachste Möglichkeit zum Erstellen eines Administrator Kontos ist in der `Seed`-Methode Programm gesteuert. Wir hoffen, dass Sie in Zukunft über ein Tool verfügen, das Ihnen das Erstellen und Verwalten von Benutzern und Rollen ermöglicht. Mit dem Beispielcode können Sie Benutzer und Rollen erstellen und verwalten. Sie müssen jedoch zunächst über ein Administrator Konto verfügen, um die SeitenRollen und Benutzer Administrator ausführen zu können. In diesem Beispiel wird das Administrator Konto beim Seeding der Datenbank erstellt.

Ändern Sie das Kennwort, und ändern Sie den Namen in ein Konto, in dem Sie e-Mail-Benachrichtigungen erhalten

> [!WARNING]
> Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode.

Wie bereits erwähnt, fügt der `app.CreatePerOwinContext` Aufruf in der Startup-Klasse Rückrufe zur `Create`-Methode der Klassen "App DB Content", "User Manager" und "Role Manager" hinzu. Die owin-Pipeline Ruft die `Create`-Methode für diese Klassen für jede Anforderung auf und speichert den Kontext für jede Klasse. Der Konto Controller macht den Benutzer-Manager aus dem HTTP-Kontext (der den owin-Kontext enthält) verfügbar:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Wenn ein Benutzer ein lokales Konto registriert, wird die `HTTP Post Register`-Methode aufgerufen:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Der obige Code verwendet die Modelldaten zum Erstellen eines neuen Benutzerkontos mithilfe der eingegebenen e-Mail-Adresse und des eingegebenen Kennworts. Wenn sich der e-Mail-Alias im Datenspeicher befindet, schlägt die Kontoerstellung fehl, und das Formular wird erneut angezeigt. Die `GenerateEmailConfirmationTokenAsync`-Methode erstellt ein sicheres Bestätigungs Token und speichert es im ASP.net Identity Datenspeicher. Die [URL. Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) -Methode erstellt einen Link, der die `UserId` und das Bestätigungs Token enthält. Dieser Link wird dann per e-Mail an den Benutzer gesendet, der Benutzer kann den Link in der e-Mail-App auswählen, um sein Konto zu bestätigen.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>E-Mail-Bestätigung einrichten

Wechseln Sie zur [Azure sendgrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) -Registrierungsseite, und registrieren Sie sich für ein kostenloses Konto. Fügen Sie zum Konfigurieren von sendgrid Code ähnlich dem folgenden hinzu:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> E-Mail-Clients akzeptieren häufig nur Textnachrichten (kein HTML-Format). Sie sollten die Nachricht in Text und HTML bereitstellen. Im obigen sendgrid-Beispiel erfolgt dies mit dem oben gezeigten `myMessage.Text`-und `myMessage.Html` Code.

Der folgende Code zeigt, wie Sie e-Mails mithilfe der [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) -Klasse senden, wobei `message.Body` nur den Link zurückgibt.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Sicherheit: Speichern Sie vertrauliche Daten niemals in Ihrem Quellcode. Das Konto und die Anmelde Informationen werden in der appSetting gespeichert. In Azure können diese Werte auf der Registerkarte **[Konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** des Azure-Portal sicher gespeichert werden. Weitere Informationen finden [Sie unter Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)

Geben Sie Ihre sendgrid-Anmelde Informationen ein, führen Sie die APP aus, und registrieren Sie sich mit einem e-Mail-Alias Informationen dazu, wie Sie dies mit Ihrem [Outlook.com](http://outlook.com) -e-Mail-Konto durchführen, finden Sie in der SMTP-Konfiguration von John Tors [ C# für Outlook.com SMTP-Host](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) und im[ASP.net Identity 2,0: Einrichten der Konto Validierung und der zweistufigen Autorisierungs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) Beiträge.

Nachdem ein Benutzer die Schaltfläche **registrieren** ausgewählt hat, wird eine Bestätigungs-e-Mail mit einem Überprüfungs Token an seine e-Mail-Adresse gesendet

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Dem Benutzer wird eine e-Mail mit einem Bestätigungs Token für sein Konto gesendet.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Untersuchen des Codes

Der folgende Code veranschaulicht die `POST ForgotPassword`-Methode.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Die Methode schlägt im Hintergrund fehl, wenn die Benutzer-e-Mail nicht bestätigt wurde. Wenn ein Fehler für eine ungültige e-Mail-Adresse gepostet wurde, können böswillige Benutzer diese Informationen verwenden, um gültige UserID (e-Mail-Aliase) für Angriffe zu suchen.

Der folgende Code zeigt die `ConfirmEmail` Methode im Konto Controller, die aufgerufen wird, wenn der Benutzer den Bestätigungslink in der e-Mail-Adresse auswählt, die an ihn gesendet wird:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Sobald ein vergessenes Kenn Wort Token verwendet wurde, wird es ungültig. Die folgende Codeänderung in der `Create`-Methode (in der Datei *App\_start\identityconfig.cs* ) legt fest, dass die Token in drei Stunden ablaufen.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Mit dem obigen Code läuft das vergessene Kennwort und die e-Mail-Bestätigungs Token in drei Stunden ab. Der Standard `TokenLifespan` ist ein Tag.

Der folgende Code zeigt die e-Mail-Bestätigungs Methode:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 ASP.net Identity unterstützt die zweistufige Authentifizierung (2FA), um Ihre APP sicherer zu machen. Weitere Informationen finden [Sie unter ASP.net Identity 2,0: Einrichten der Konto Validierung und der zweistufigen Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) durch John Atten. Obwohl Sie die Kontosperrung bei Fehlern beim Versuch eines Anmelde Kennworts festlegen können, ist der Anmelde Name für die [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) -Sperrung anfällig. Es wird empfohlen, die Kontosperrung nur mit 2FA zu verwenden.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- Die [MVC 5-App mit Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) zeigt außerdem, wie Sie der Tabelle "Users" Profilinformationen hinzufügen.
- [ASP.NET MVC und Identity 2,0: Grundlegendes zu den Grundlagen](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) von John Atten.
- [Einführung zu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ankündigungs-RTM von ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) von Pranav Rastogi.
