---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Hinzufügen von ASP.net Identity zu einem leeren oder vorhandenen Web Forms Projekt ASP.NET 4. x
author: raquelsa
description: In diesem Tutorial wird gezeigt, wie Sie einer ASP.NET-Anwendung ASP.net Identity (das Mitgliedschaftssystem für ASP.net) hinzufügen. Wenn Sie einen neuen Web Forms oder MVC erstellen...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e82951d57f0b8052ee3f6530a7470be7d030206
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471975"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Hinzufügen von ASP.NET Identity zu einem leeren oder vorhandenen Web Forms-Projekt

> In diesem Tutorial wird gezeigt, wie Sie einer ASP.NET-Anwendung [ASP.net Identity](introduction-to-aspnet-identity.md) (das neue Mitgliedschaftssystem für ASP.net) hinzufügen.
> 
> Wenn Sie ein neues Web Forms-oder MVC-Projekt in Visual Studio 2017 RTM mit einzelnen Konten erstellen, werden alle erforderlichen Pakete von Visual Studio installiert, und alle erforderlichen Klassen werden für Sie hinzugefügt. In diesem Tutorial werden die Schritte zum Hinzufügen ASP.net Identity Unterstützung für Ihr vorhandenes Web Forms Projekt oder ein neues leeres Projekt veranschaulicht. Ich werde alle nuget-Pakete, die Sie installieren müssen, und die Klassen, die Sie hinzufügen müssen, erläutern. Ich übergebe Beispiel Web Forms zum Registrieren neuer Benutzer und zum Anmelden, während alle Haupteinstiegspunkt-APIs für die Benutzerverwaltung und-Authentifizierung hervorgehoben werden. In diesem Beispiel wird die ASP.net Identity Standard Implementierung für die SQL-Datenspeicherung verwendet, die auf Entity Framework basiert. In diesem Tutorial wird localdb für die SQL-Datenbank verwendet.
> 

## <a name="get-started-with-aspnet-identity"></a>Beginnen Sie mit ASP.net Identity

1. Beginnen Sie mit der Installation und Ausführung von [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Wählen Sie auf der Start Seite die Option **Neues Projekt** aus, oder verwenden Sie das Menü, und wählen Sie **Datei**und dann **Neues Projekt**aus.
3. Erweitern Sie im linken Bereich **Visualisierung C#** , und wählen Sie dann **Web**und dann **ASP.NET Webanwendung (.NET Framework)** aus. Benennen Sie das Projekt "webformsidentity", und wählen Sie **OK**aus.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Beachten Sie, dass die Schaltfläche **Authentifizierung ändern** deaktiviert ist und in dieser Vorlage keine Authentifizierungs Unterstützung bereitgestellt wird. Mit den Web Forms-, MVC-und Web-API-Vorlagen können Sie die Authentifizierungsmethode auswählen.

## <a name="add-identity-packages-to-your-app"></a>Hinzufügen von Identitäts Paketen zu Ihrer APP

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Pakete verwalten**aus. Suchen Sie nach dem Paket **Microsoft. Aspnet. Identity. EntityFramework** , und installieren Sie es. 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Beachten Sie, dass mit diesem Paket die Abhängigkeits Pakete installiert werden: **EntityFramework** und **Microsoft ASP.net Identity Core**.

## <a name="add-a-web-form-to-register-users"></a>Hinzufügen eines Webformulars zum Registrieren von Benutzern

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**und dann **Web Form aus**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Benennen Sie im Dialogfeld **Name für Element angeben** das neue Webformular **Register**, und wählen Sie dann **OK** aus.
3. Ersetzen Sie das Markup in der generierten Datei " *Register. aspx* " durch den folgenden Code. Die Codeänderungen werden hervorgehoben. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Dies ist nur eine vereinfachte Version der Datei " *Register. aspx* ", die erstellt wird, wenn Sie ein neues ASP.net-Web Forms Projekt erstellen. Das obige Markup fügt Formularfelder und eine Schaltfläche hinzu, um einen neuen Benutzer zu registrieren.
4. Öffnen Sie die Datei *Register.aspx.cs* , und ersetzen Sie den Inhalt der Datei durch den folgenden Code:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Der obige Code ist eine vereinfachte Version der *Register.aspx.cs* -Datei, die erstellt wird, wenn Sie ein neues ASP.net-Web Forms Projekt erstellen.
    > 2. Die *identityuser* -Klasse ist die Standard Implementierung von EntityFramework der *iuser* -Schnittstelle. *Iuser* Interface ist die minimale Benutzeroberfläche für einen Benutzer auf ASP.net Identity Core.
    > 3. Die *userstore* -Klasse ist die Standard Implementierung von EntityFramework eines Benutzer Stores. Diese Klasse implementiert die minimalen Schnittstellen des ASP.net Identity-Kerns: *iuserstore*, *iuserloginstore*, *iuserclaimstore* und *iuserrolestore*.
    > 4. Die *usermanager* -Klasse macht Benutzer bezogene APIs verfügbar, die automatisch Änderungen am *userstore*speichern.
    > 5. Die *identityresult* -Klasse stellt das Ergebnis einer Identitäts Operation dar.
5. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**, **Ordner ASP.net** und dann **App\_Daten**aus.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Öffnen Sie die Datei *Web. config* , und fügen Sie einen Eintrag für die Verbindungs Zeichenfolge für die Datenbank hinzu, in der die Benutzerinformationen gespeichert werden. Die Datenbank wird zur Laufzeit von EntityFramework für die Identitäts Entitäten erstellt. Die Verbindungs Zeichenfolge ähnelt der für Sie erstellte Verbindungs Zeichenfolge, wenn Sie ein neues Web Forms Projekt erstellen. Der hervorgehobene Code zeigt das Markup, das Sie hinzufügen sollten:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Ersetzen Sie in Visual Studio 2015 oder höher `(localdb)\v11.0` durch `(localdb)\MSSQLLocalDB` in der Verbindungs Zeichenfolge.
    
7. Klicken Sie im Projekt mit der rechten Maustaste auf File *Register. aspx* , und wählen Sie **als Start Seite festlegen**aus. Drücken Sie STRG + F5, um die Webanwendung zu erstellen und auszuführen. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und wählen Sie dann **registrieren**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.net Identity bietet Unterstützung für die Validierung, und in diesem Beispiel können Sie das Standardverhalten für Benutzer-und Kenn Wort Validierungs Steuerelemente überprüfen, die aus dem Identitäts Kern Paket stammen. Das standardmäßige Validierungs Steuerelement für den Benutzer (`UserValidator`) verfügt über eine Eigenschaft `AllowOnlyAlphanumericUserNames`, für die der Standardwert `true`festgelegt ist. Das standardmäßige Validierungs Steuerelement für das Kennwort (`MinimumLengthValidator`) stellt sicher, dass das Kennwort mindestens 6 Zeichen umfasst. Diese Validierungs Steuerelemente sind Eigenschaften auf `UserManager`, die überschrieben werden können, wenn Sie über eine benutzerdefinierte Validierung verfügen möchten.

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Überprüfen der durch Entity Framework generierten localdb-Identitätsdatenbank und-Tabellen

1. Wählen Sie im Menü **Ansicht** die Option **Server-Explorer**aus.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Erweitern Sie **DefaultConnection (webformsidentity)** , erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **aspnettusers** , und wählen Sie dann **Tabellendaten anzeigen**aus.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Konfigurieren der Anwendung für die owin-Authentifizierung

An dieser Stelle haben wir nur die Unterstützung für das Erstellen von Benutzern hinzugefügt. Nun zeigen wir, wie wir eine Authentifizierung zum Anmelden eines Benutzers hinzufügen können. ASP.net Identity verwendet die Microsoft owin-Authentifizierungs Middleware für die Formular Authentifizierung. Die owin-Cookieauthentifizierung ist ein cookiebasierter und Anspruchs basierter Authentifizierungsmechanismus, der von allen unter [owin](https://msdn.microsoft.com/magazine/dn451439.aspx) oder IIS gehosteten Frameworks verwendet werden kann. Mit diesem Modell können die gleichen Authentifizierungs Pakete in mehreren Frameworks verwendet werden, einschließlich ASP.NET MVC und Web Forms. Weitere Informationen zu Project Katana und zum Ausführen von Middleware in einem Host agnostisch finden Sie unter [Getting Started with the Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Installieren von Authentifizierungs Paketen für Ihre Anwendung

1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Pakete verwalten**aus. Suchen Sie nach dem Paket ***Microsoft. Aspnet. Identity. owin*** , und installieren Sie es. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Suchen Sie nach dem Paket ***Microsoft. owin. Host. systemWeb*** , und installieren Sie es.

    > [!NOTE]
    > Das Paket " **Microsoft. Aspnet. Identity. owin** " enthält einen Satz von owin-Erweiterungs Klassen zum Verwalten und Konfigurieren der owin-Authentifizierungs Middleware, die von ASP.net Identity Core-Paketen verwendet werden soll.
    > Das **Microsoft. owin. Host. systemWeb** -Paket enthält einen owin-Server, der das Ausführen von owin-basierten Anwendungen auf IIS mithilfe der ASP.net Request-Pipeline ermöglicht. Weitere Informationen finden Sie [unter owin-Middleware in der integrierten IIS-Pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Hinzufügen von owin-Konfigurations Klassen für Start und Authentifizierung

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen**und dann **Neues Element hinzufügen**aus. Geben Sie im Dialogfeld Such Textfeld den Text "*owin*" ein. Benennen Sie die Klasse "*Startup*", und wählen Sie **Hinzufügen**aus. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Fügen Sie in der Datei Startup.cs den unten gezeigten hervorgehobenen Code hinzu, um die owin-Cookie-Authentifizierung zu konfigurieren.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Diese Klasse enthält das `OwinStartup` Attribut zum Angeben der owin-Startklasse. Jede owin-Anwendung verfügt über eine Startup-Klasse, in der Sie Komponenten für die Anwendungs Pipeline angeben. Weitere Informationen zu diesem Modell finden Sie unter [owin-Startklassen Erkennung](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) .

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Hinzufügen von Web Forms zum Registrieren und Anmelden von Benutzern

1. Öffnen Sie die Datei *Register.aspx.cs* , und fügen Sie folgenden Code hinzu, der den Benutzer anmeldet, wenn die Registrierung erfolgreich ist.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Da ASP.net Identity und die owin-Cookieauthentifizierung Anspruchs basiertes System sind, erfordert das Framework, dass der App-Entwickler eine " [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) " für den Benutzer generiert. ClaimsIdentity enthält Informationen über alle Ansprüche für den Benutzer, z. b. die Rollen, denen der Benutzer angehört. Sie können in dieser Phase auch weitere Ansprüche für den Benutzer hinzufügen.
    > - Sie können den Benutzer anmelden, indem Sie den AuthenticationManager von owin verwenden und `SignIn` aufrufen und die ClaimsIdentity übergeben, wie oben gezeigt. Mit diesem Code wird der Benutzer angemeldet und auch ein Cookie generiert. Dieser Befehl ist analog zu [formauthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) , das vom [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) -Modul verwendet wird.
2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen**und dann **Web Form aus**. Benennen Sie den Webformular- **Anmelde**Namen.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Ersetzen Sie den Inhalt der *Login. aspx* -Datei durch den folgenden Code:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Ersetzen Sie den Inhalt der *Login.aspx.cs* -Datei durch Folgendes:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Der `Page_Load` prüft jetzt den Status des aktuellen Benutzers und führt basierend auf seinem `Context.User.Identity.IsAuthenticated` Status Aktionen aus.
    >   **Angemeldeter Benutzer Name anzeigen** : das Microsoft ASP.net Identity Framework hat Erweiterungs Methoden für [System. Security. Principal. IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) hinzugefügt, die es Ihnen ermöglichen, die `UserName` und `UserId` für den angemeldeten Benutzer zu erhalten. Diese Erweiterungs Methoden werden in der `Microsoft.AspNet.Identity.Core`-Assembly definiert. Diese Erweiterungs Methoden sind die Ersetzung für [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - SignIn-Methode: `This`-Methode ersetzt die vorherige `CreateUser_Click`-Methode in diesem Beispiel und meldet den Benutzer ab, nachdem der Benutzer erfolgreich erstellt wurde.   
    >   Das Microsoft owin-Framework hat Erweiterungs Methoden auf `System.Web.HttpContext` hinzugefügt, mit denen Sie einen Verweis auf eine `IOwinContext`erhalten können. Diese Erweiterungs Methoden werden in `Microsoft.Owin.Host.SystemWeb` Assembly definiert. Die `OwinContext`-Klasse macht eine `IAuthenticationManager`-Eigenschaft verfügbar, die die für die aktuelle Anforderung Verfügbare Authentifizierungs Middleware-Funktionalität darstellt. Sie können den Benutzer mit dem `AuthenticationManager` von owin anmelden und `SignIn` aufrufen und die `ClaimsIdentity` wie oben gezeigt übergeben. Da ASP.net Identity-und owin-Cookieauthentifizierung Anspruchs basiertes System sind, erfordert das Framework, dass die APP eine `ClaimsIdentity` für den Benutzer generiert. Der `ClaimsIdentity` enthält Informationen über alle Ansprüche für den Benutzer, z. b. die Rollen, denen der Benutzer angehört. Sie können in dieser Phase auch weitere Ansprüche für den Benutzer hinzufügen. dieser Code meldet den Benutzer an und generiert auch ein Cookie. Dieser Befehl ist analog zu [formauthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) , das vom [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) -Modul verwendet wird.
    > - `SignOut`-Methode: Ruft einen Verweis auf die `AuthenticationManager` von owin ab und ruft `SignOut`auf. Dies entspricht der [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) -Methode, die vom [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) -Modul verwendet wird.
5. Drücken Sie **STRG + F5** , um die Webanwendung zu erstellen und auszuführen. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und wählen Sie dann **registrieren**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Hinweis: an diesem Punkt wird der neue Benutzer erstellt und angemeldet.
6. Wählen Sie die Schaltfläche **Abmelden aus** . Sie werden zum Anmeldeformular umgeleitet.
7. Geben Sie einen ungültigen Benutzernamen oder ein Kennwort ein, und wählen Sie die Schaltfläche **Anmelden** 
   Die `UserManager.Find`-Methode gibt NULL zurück, und die Fehlermeldung " *Ungültiger Benutzername oder Kennwort* " wird angezeigt.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
