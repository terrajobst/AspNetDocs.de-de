---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Hinzufügen von Sicherheit und Mitgliedschaft zu einer ASP.net Web Pages-(Razor-) Website | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie Ihre Website sichern, damit einige der Seiten nur für Personen verfügbar sind, die sich anmelden. (Außerdem wird erläutert, wie Seiten erstellt werden...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 0be3e767a42939a3c343f6d4a730eb1d9a6b367c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509901"
---
# <a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Hinzufügen von Sicherheit und Mitgliedschaft zu einer ASP.net Web Pages-(Razor-) Website

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie eine ASP.net Web Pages (Razor)-Website sichern, damit einige der Seiten nur für Personen verfügbar sind, die sich anmelden. (Sie erfahren auch, wie Sie Seiten erstellen, auf die jeder zugreifen kann.)
> 
> **Lernen Sie Folgendes:** 
> 
> - Es wird erläutert, wie Sie eine Website erstellen, die über eine Registrierungsseite und eine Anmeldeseite verfügt, damit Sie für einige Seiten den Zugriff auf Mitglieder einschränken können.
> - Erstellen von öffentlichen und Element basierten Seiten.
> - Definieren von Rollen, bei denen es sich um Gruppen mit unterschiedlichen Sicherheits Berechtigungen für Ihre Website handelt, sowie zum Zuweisen von Benutzern zu einer Rolle.
> - Verwenden von CAPTCHA, um zu verhindern, dass automatisierte Programme (Bots) Mitgliedskonten erstellen.
>   
> 
> Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:
> 
> - Die webmatrix- **Starter Site** -Vorlage.
> - Das `WebSecurity`-Hilfsprogramm und `Roles`-Klasse.
> - Das `ReCaptcha`-Hilfsprogramm.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - Webmatrix 3
> - ASP.net-webhilfsprogramme-Bibliothek

Sie können Ihre Website so einrichten, dass Benutzer sich bei ihr &#8212; anmelden können, sodass die Website die *Mitgliedschaft*unterstützt. Dies kann aus vielen Gründen nützlich sein. Beispielsweise könnte Ihre Website Seiten enthalten, die nur Mitgliedern zur Verfügung stehen sollen. In einigen Fällen ist es möglicherweise erforderlich, dass sich Benutzer anmelden, um Ihnen Feedback zu senden oder einen Kommentar zu hinterlassen.

Selbst wenn Ihre Website die Mitgliedschaft unterstützt, müssen sich Benutzer nicht unbedingt anmelden, bevor Sie einige der Seiten auf der Website verwenden. Benutzer, die nicht angemeldet sind, werden als *anonyme Benutzer*bezeichnet.

Ein Benutzer kann sich auf Ihrer Website registrieren und sich dann bei der Website anmelden. Für die Website sind ein Benutzername (eine e-Mail-Adresse) und ein Kennwort erforderlich, um zu bestätigen, dass die Benutzer den Anspruch darauf haben. Dieser Prozess der Anmeldung und Bestätigung der Identität eines Benutzers wird als *Authentifizierung*bezeichnet.

Sie können Sicherheit und Mitgliedschaft auf verschiedene Weise einrichten:

- Wenn Sie webmatrix verwenden, ist es eine einfache Möglichkeit, als neue Website basierend auf der Vorlage " **Starter Site** " zu erstellen. Diese Vorlage ist bereits für Sicherheit und Mitgliedschaft konfiguriert und verfügt bereits über eine Registrierungsseite, eine Anmeldeseite und so weiter.

    Die von der Vorlage erstellte Site verfügt auch über eine Option, mit der Benutzer sich über eine externe Website wie Facebook, Google oder Twitter anmelden können.
- Wenn Sie einer vorhandenen Website Sicherheit hinzufügen möchten oder wenn Sie die Vorlage " **Starter Site** " nicht verwenden möchten, können Sie eine eigene Registrierungsseite, Anmeldeseite usw. erstellen.

Dieser Artikel konzentriert sich auf die erste Option &mdash; Hinzufügen von Sicherheit mithilfe der Vorlage " **Starter Site** ". Außerdem finden Sie hier einige grundlegende Informationen zum Implementieren ihrer eigenen Sicherheit und Links zu weiteren Informationen zu diesem Thema. Es gibt auch Informationen zum Aktivieren externer Anmeldungen, die in einem separaten Artikel ausführlicher beschrieben werden.

## <a name="creating-website-security-using-the-starter-site-template"></a>Erstellen der Website Sicherheit mithilfe der Vorlage "Starter Site"

In webmatrix können Sie die Vorlage " **Starter Site** " verwenden, um eine Website zu erstellen, die Folgendes enthält:

- Eine Datenbank, die zum Speichern von Benutzernamen und Kenn Wörtern für ihre Mitglieder verwendet wird.
- Eine Registrierungsseite, auf der anonyme (neue) Benutzer registrieren können.
- Eine Anmelde-und Abmelde Seite.
- Eine Seite für die Kenn Wort Wiederherstellung und-zurück Setzung

Im folgenden Verfahren wird beschrieben, wie der Standort erstellt und konfiguriert wird.

1. Starten Sie webmatrix, und wählen Sie auf der Seite **Schnellstart** die Option **Site from Template aus**.
2. Wählen Sie die Vorlage **Starter Site** aus, und klicken Sie dann auf **OK**. Webmatrix erstellt eine neue Website.
3. Klicken Sie im linken Bereich auf die Arbeitsbereichs Auswahl für **Dateien** .
4. Öffnen Sie im Stamm Ordner der Website die Datei *\_AppStart. cshtml* , bei der es sich um eine spezielle Datei handelt, die für globale Einstellungen verwendet wird. Sie enthält einige Anweisungen, die mithilfe der `//` Zeichen auskommentiert werden:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Diese Anweisungen konfigurieren das `WebMail`-Hilfsprogramm, das zum Senden von e-Mails verwendet werden kann. Das Mitgliedschaftssystem kann e-Mails verwenden, um Bestätigungsnachrichten zu senden, wenn Benutzer sich registrieren oder wenn Sie Ihre Kenn Wörter ändern möchten. (Nach der Registrierung von Benutzern erhalten Sie z. b. eine e-Mail mit einem Link, auf den Sie klicken können, um den Registrierungsvorgang abzuschließen.)

    Das Senden von e-Mails erfordert Zugriff auf einen SMTP-Server, wie unter [Hinzufügen von e-Mail zu einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202899)beschrieben. Sie speichern die e-Mail-Einstellungen in diesem zentralen *\_AppStart. cshtml* -Datei, sodass Sie Sie nicht wiederholt in jede Seite codieren müssen, die e-Mails senden kann. (Sie müssen keine SMTP-Einstellungen konfigurieren, um eine Registrierungsdatenbank einzurichten. Sie benötigen nur SMTP-Einstellungen, wenn Sie Benutzer anhand ihres e-Mail-Alias überprüfen und Benutzer ein vergessenes Kennwort zurücksetzen möchten.)
5. Heben Sie die Auskommentierung der Anweisungen auf, indem Sie `//` von vor jeder Datei entfernen.

    Wenn Sie keine e-Mail-Bestätigung einrichten möchten, können Sie diesen Schritt und den nächsten Schritt überspringen. Wenn die SMTP-Werte nicht festgelegt sind, ist das neue Konto sofort ohne Bestätigungs-e-Mail verfügbar.
6. Ändern Sie die folgenden e-Mail-bezogenen Einstellungen im Code:

   - Legen Sie `WebMail.SmtpServer` auf den Namen des SMTP-Servers fest, auf den Sie Zugriff haben.
   - Lassen Sie `WebMail.EnableSsl` auf `true`festgelegt. Diese Einstellung sichert die Anmelde Informationen, die an den SMTP-Server gesendet werden, indem Sie verschlüsselt werden.
   - Legen Sie `WebMail.UserName` auf den Benutzernamen für Ihr SMTP-Server Konto fest.
   - Legen Sie `WebMail.Password` auf das Kennwort für Ihr SMTP-Server Konto fest.
   - Legen Sie `WebMail.From` auf Ihre e-Mail-Adresse fest. Dies ist die e-Mail-Adresse, von der die Nachricht gesendet wird.

     > [!NOTE] 
     > 
     > **Tipp** Weitere Informationen zu den Werten für diese Eigenschaften finden Sie unter [Konfigurieren von e-Mail-Einstellungen](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) in [Anpassen des Standort weiten Verhaltens für ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Speichern und schließen Sie *\_AppStart. cshtml*.
8. Führen Sie die " *default. cshtml* "-Seite in einem Browser aus.

    ![Sicherheits Mitgliedschaft-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Wenn eine Fehlermeldung angezeigt wird, die besagt, dass eine Eigenschaft eine Instanz von `ExtendedMembershipProvider`sein muss, ist der Standort möglicherweise nicht für die Verwendung des ASP.net Web Pages Mitgliedschafts Systems (simplemembership) konfiguriert. Dies kann vorkommen, wenn der Server eines hostinganbieters anders konfiguriert ist als der lokale Server. Um dieses Problem zu beheben, fügen Sie der Datei " *Web. config* " der Website das folgende-Element hinzu:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Fügen Sie dieses Element als untergeordnetes Element des `<configuration>`-Elements und als Peer des `<system.web>`-Elements hinzu.
9. Klicken Sie in der oberen rechten Ecke der Seite auf den Link **registrieren** . Die Seite *Register. cshtml* wird angezeigt.
10. Geben Sie einen Benutzernamen und ein Kennwort ein, und klicken Sie auf **registrieren**

    ![Sicherheit-Mitgliedschaft-3](16-adding-security-and-membership/_static/image2.png)

    Wenn Sie die Website aus der Vorlage " **Starter Site** " erstellt haben, wurde eine Datenbank mit dem Namen " *startersite. sdf* " im *App-\_Daten* Ordner der Site erstellt. Während der Registrierung werden die Benutzerinformationen der-Datenbank hinzugefügt. Wenn Sie die SMTP-Werte festlegen, wird eine Nachricht an die von Ihnen verwendete e-Mail-Adresse gesendet, damit Sie die Registrierung abschließen können.

    ![Sicherheit-Mitgliedschaft-4](16-adding-security-and-membership/_static/image3.png)
11. Wechseln Sie zu Ihrem e-Mail-Programm, und suchen Sie nach der Nachricht, die den Bestätigungscode und einen Link zur Website enthält.
12. Klicken Sie auf den Link, um Ihr Konto zu aktivieren. Über den Link Bestätigung wird eine Registrierungs Bestätigungsseite geöffnet.

    ![Sicherheits Mitgliedschaft-5](16-adding-security-and-membership/_static/image4.png)
13. Klicken Sie auf den Link **Anmelden** , und melden Sie sich mit dem Konto an, das Sie registriert haben.

      Nachdem Sie sich angemeldet haben, werden die **Anmelde** -und **Register** Links durch **einen** Abmeldelink ersetzt. Ihr Anmelde Name wird als Link angezeigt. (Der Link ermöglicht das Wechseln zu einer Seite, auf der Sie Ihr Kennwort ändern können.)

      ![Sicherheits Mitgliedschaft-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Standardmäßig senden ASP.NET-Webseiten Anmelde Informationen als Klartext an den Server (als Menschen lesbarer Text). Eine Produktions Site sollte Secure HTTP (https://, auch als *Secure Sockets Layer* oder SSL bezeichnet) zum Verschlüsseln vertraulicher Informationen verwenden, die mit dem Server ausgetauscht werden. Sie können festlegen, dass e-Mail-Nachrichten mithilfe von SSL gesendet werden sollen, indem Sie wie im vorherigen Beispiel `WebMail.EnableSsl=true` festlegen. Weitere Informationen zu SSL finden Sie unter [Sichern der Webkommunikation: Zertifikate, SSL und https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Zusätzliche Mitgliedschafts Funktionen am Standort

Ihre Website enthält weitere Funktionen, mit deren Hilfe Benutzer ihre Konten verwalten können. Benutzer können folgende Aktionen ausführen:

- Ändern Sie Ihre Kenn Wörter. Nachdem Sie sich angemeldet haben, können Sie auf den Benutzernamen (einen Link) klicken. Dadurch gelangen Sie zu einer Seite, auf der Sie ein neues Kennwort erstellen können (*Konto/ChangePassword. cshtml*).
- Wiederherstellen eines vergessenen Kennworts. Auf der Anmeldeseite befindet sich ein Link (**haben Sie Ihr Kennwort vergessen?** ), mit dem Benutzer zu einer Seite (*Account/forgotPassword. cshtml*) gelangen, in der Sie eine e-Mail-Adresse eingeben können. Die Website sendet Ihnen eine e-Mail-Nachricht mit einem Link, auf den Sie klicken können, um ein neues Kennwort (*Account/PasswordReset. cshtml*) festzulegen.

Außerdem können Sie es Benutzern ermöglichen, sich auch über eine externe Website anzumelden, wie weiter unten erläutert.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Erstellen einer reinen Members-Seite

Für den Zeitpunkt, an dem eine beliebige Seite auf der Website angezeigt werden kann, kann jeder beliebige Seite aufrufen. Vielleicht möchten Sie aber auch Seiten haben, die nur für Personen verfügbar sind, die sich angemeldet haben (d. h. für Mitglieder). ASP.NET ermöglicht das Erstellen von Seiten, auf die nur von angemeldeten Membern aus zugegriffen werden kann. Wenn anonyme Benutzer versuchen, auf eine nur Mitgliederseite zuzugreifen, werden Sie in der Regel an die Anmeldeseite umgeleitet.

In diesem Verfahren erstellen Sie einen Ordner, der Seiten enthält, die nur für angemeldete Benutzer verfügbar sind.

1. Erstellen Sie im Stammverzeichnis der Website einen neuen Ordner. (Klicken Sie im Menüband auf den Pfeil unterhalb **neu** , und wählen Sie dann **neuer Ordner**aus.)
2. Benennen Sie die neuen Ordner *Mitglieder*.
3. Erstellen Sie im Ordner *Members* eine neue Seite, und benennen Sie Sie mit der Bezeichnung " *Mitgliedsinformation. cshtml*".
4. Ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und das Markup:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Mit diesem Code wird die `IsAuthenticated`-Eigenschaft des `WebSecurity`-Objekts getestet, das `true` zurückgibt, wenn sich der Benutzer angemeldet hat. Wenn der Benutzer nicht angemeldet ist, ruft der Code `Response.Redirect` auf, um den Benutzer an die Seite *Login. cshtml* im *Konto* Ordner zu senden.

    Die URL der Umleitung enthält einen `returnUrl` Abfrage Zeichen folgen Wert, der `Request.Url.LocalPath` verwendet, um den Pfad der aktuellen Seite festzulegen. Wenn Sie den `returnUrl` Wert in der Abfrage Zeichenfolge wie folgt festlegen (und die Rückgabe-URL ein lokaler Pfad ist), werden die Benutzer von der Anmeldeseite nach der Anmeldung an diese Seite zurückgegeben.

    Der Code legt auch *\_Sitelayout. cshtml* -Seite als Layoutseite fest. (Weitere Informationen zu Layoutseiten finden Sie unter [Erstellen eines konsistenten Layouts in ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Führen Sie die Website aus. Wenn Sie noch angemeldet sind, klicken Sie auf die Schaltfläche **Abmelden** am oberen Rand der Seite.
6. Fordern Sie im Browser die Seite */Members/MembersInformation*an. Die URL könnte z. b. wie folgt aussehen:

    `http://localhost:38366/Members/MembersInformation`

    (Die Portnummer (38366) unterscheidet sich wahrscheinlich in Ihrer URL.)

    Sie werden zur Seite *Login. cshtml* umgeleitet, da Sie nicht angemeldet sind.
7. Melden Sie sich mit dem Konto an, das Sie zuvor erstellt haben. Sie werden zurück zur Seite " *mitgliedbildung* " umgeleitet. Da Sie angemeldet sind, wird Ihnen dieses Mal der Inhalt der Seite angezeigt.

Um den Zugriff auf mehrere Seiten zu sichern, können Sie Folgendes tun:

- Fügen Sie die Sicherheitsüberprüfung zu jeder Seite hinzu.
- Erstellen Sie eine *\_pagestart. cshtml* -Seite in dem Ordner, in dem Sie geschützte Seiten aufbewahren, und fügen Sie dort die Sicherheitsüberprüfung ein. Die Seite *\_pagestart. cshtml* fungiert als eine Art globaler Seite für alle Seiten im Ordner. Dieses Verfahren wird unter [Anpassen des Standort weiten Verhaltens für ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access)ausführlicher erläutert.

## <a name="creating-security-for-groups-of-users-roles"></a>Erstellen der Sicherheit für Gruppen von Benutzern (Rollen)

Wenn Ihre Website viele Mitglieder hat, ist es nicht effizient, die Berechtigung für jeden Benutzer einzeln zu überprüfen, bevor Sie eine Seite sehen können. Sie können stattdessen Gruppen oder *Rollen*erstellen, zu denen einzelne Mitglieder gehören. Sie können dann Berechtigungen basierend auf der Rolle überprüfen. In diesem Abschnitt erstellen Sie eine &quot;admin-&quot; Rolle und erstellen dann eine Seite, auf die Benutzer zugreifen können, die sich in der Rolle befinden.

Das ASP.NET-Mitgliedschaftssystem ist so eingerichtet, dass Rollen unterstützt werden. Anders als bei der Mitgliedschafts Registrierung und-Anmeldung enthält die **Starter Site** -Vorlage jedoch keine Seiten, die Sie beim Verwalten von Rollen unterstützen. (Das Verwalten von Rollen ist eine administrative Aufgabe und keine Benutzer Aufgabe.) Allerdings können Sie Gruppen direkt in der Mitgliedschafts Datenbank in webmatrix hinzufügen.

1. Klicken Sie in webmatrix auf die Arbeitsbereichs Auswahl für **Datenbanken** .
2. Öffnen Sie im linken Bereich den Knoten *startersite. sdf* , öffnen Sie den Knoten **Tabellen** , und doppelklicken Sie dann auf die Tabelle *Webseiten\_Rollen* .

    ![Sicherheits Mitgliedschaft-7](16-adding-security-and-membership/_static/image6.png)
3. Fügen Sie eine Rolle mit dem Namen &quot;admin&quot;hinzu. Das *RoleID* -Feld wird automatisch ausgefüllt. (Der Primärschlüssel ist der Primärschlüssel und wurde als identifizfeld festgelegt, wie unter [Einführung in das Arbeiten mit einer Datenbank auf ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202893)erläutert.)
4. Notieren Sie sich den Wert für das Feld *RoleID* . (Wenn es sich hierbei um die erste Rolle handelt, die Sie definieren, wird Sie auf 1 festgelegt.)

    ![Sicherheits Mitgliedschaft-8](16-adding-security-and-membership/_static/image7.png)
5. Schließen Sie die Tabelle *Webseiten\_Rollen* .
6. Öffnen Sie die Tabelle " *UserProfile* ".
7. Notieren Sie sich den *UserID* -Wert eines oder mehrerer Benutzer in der Tabelle, und schließen Sie dann die Tabelle.
8. Öffnen Sie die Tabelle *Webseiten\_userinllen* -Tabelle, und geben Sie eine *UserID* und einen *RoleID* -Wert in die Tabelle ein. Wenn Sie z. b. Benutzer 2 in die Rolle "&quot;admin&quot;" versetzen möchten, geben Sie die folgenden Werte ein:

    ![Sicherheits Mitgliedschaft-9](16-adding-security-and-membership/_static/image8.png)
9. Schließen Sie die *Webseiten\_usersinrollen* -Tabelle.

    Nachdem Sie Rollen definiert haben, können Sie eine Seite konfigurieren, auf die Benutzer zugreifen können, die sich in dieser Rolle befinden.
10. Erstellen Sie im Stamm Ordner der Website eine neue Seite mit dem Namen " *AdminError. cshtml* ", und ersetzen Sie den vorhandenen Inhalt durch den folgenden Code. Dies ist die Seite, an die Benutzer umgeleitet werden, wenn Ihnen der Zugriff auf eine Seite nicht gestattet wird.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Erstellen Sie im Stamm Ordner der Website eine neue Seite mit dem Namen " *adminonly. cshtml* ", und ersetzen Sie den vorhandenen Code durch den folgenden Code:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Die `Roles.IsUserInRole`-Methode gibt `true` zurück, wenn der aktuelle Benutzer ein Mitglied der angegebenen Rolle ist (in diesem Fall die Rolle "admin").
12. Führen Sie " *default. cshtml* " in einem Browser aus, melden Sie sich aber nicht an. (Melden Sie sich ab, wenn Sie bereits angemeldet sind.)
13. Fügen Sie in der Adressleiste des Browsers *adminonly* in der URL hinzu. (Mit anderen Worten: fordern Sie die Datei " *adminonly. cshtml* " an.) Sie werden zur *AdminError. cshtml* -Seite umgeleitet, da Sie zurzeit nicht als Benutzer in der &quot;admin-&quot; Rolle angemeldet sind.
14. Kehren Sie zu *default. cshtml* zurück, und melden Sie sich als der Benutzer an, den Sie der Rolle &quot;admin&quot; hinzugefügt haben.
15. Navigieren Sie zur Seite " *adminonly. cshtml* ". Dieses Mal wird die Seite angezeigt.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Verhindern, dass automatisierte Programme Ihrer Website beitreten

Die Anmeldeseite verhindert, dass automatisierte Programme (manchmal auch als *Webroboter* oder *Bots*bezeichnet) bei Ihrer Website registriert werden. In diesem Verfahren wird beschrieben, wie Sie einen "reCAPTCHA"-Test für die Registrierungsseite aktivieren.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrieren Sie Ihre Website unter reCAPTCHA.net ([http://recaptcha.net](http://recaptcha.net)). Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einen privaten Schlüssel.
2. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
3. Öffnen Sie im *Konto* Ordner die Datei " *Register. cshtml*".
4. Suchen Sie im Code am oberen Rand der Seite die folgenden Zeilen, und heben Sie die Auskommentierung auf, indem Sie die `//` Kommentarzeichen entfernen:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Ersetzen Sie `PRIVATE_KEY` durch ihren eigenen Schlüssel "reCAPTCHA private".
6. Entfernen Sie im Markup der Seite die `@*`, und `*@` Sie die Kommentarzeichen aus den folgenden Zeilen im Seiten Markup:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Ersetzen Sie `PUBLIC_KEY` durch Ihren Schlüssel.
8. Wenn Sie Sie nicht bereits entfernt haben, entfernen Sie das `<div>` Element, das Text enthält, der mit "So aktivieren Sie die CAPTCHA-Verifizierung..." beginnt. (Entfernen Sie das gesamte `<div>` Element und dessen Inhalt.)

9. Führen Sie " *default. cshtml* " in einem Browser aus. Wenn Sie bei der Website angemeldet sind, klicken Sie auf den Link **Abmelden** .
10. Klicken Sie auf den Link **registrieren** , und testen Sie die Registrierung mithilfe des CAPTCHA-Tests.

     ![Sicherheits Mitgliedschaft-10](16-adding-security-and-membership/_static/image9.png)

Weitere Informationen zum `ReCaptcha`-Hilfsprogramm finden Sie unter Verwenden von "", [um zu verhindern, dass automatisierte Programme (Bots) Ihre ASP.net](https://go.microsoft.com/fwlink/?LinkId=251967)-Website verwenden.

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Zulassen, dass sich Benutzer über eine externe Website anmelden

Die Vorlage " **Starter Site** " enthält Code und Markup, mit denen Benutzer sich mit Facebook, Windows Live, Twitter, Google oder Yahoo anmelden können. Diese Funktion ist standardmäßig nicht aktiviert. Das allgemeine Verfahren für die Verwendung von Benutzern, die sich mit diesen externen Anbietern anmelden, ist folgende:

- Entscheiden Sie, welche externen Sites Sie unterstützen möchten.
- Wechseln Sie ggf. zu dieser Site, und richten Sie eine Anmelde-App ein. (Dies ist z. b. erforderlich, um Facebook-Anmeldungen zuzulassen.)
- Konfigurieren Sie den Anbieter an Ihrer Site. In den meisten Fällen müssen Sie nur Code in der *\_AppStart. cshtml* -Datei auskommentieren.
- Fügen Sie der Registrierungsseite Markup hinzu, mit dem Benutzer für die Anmeldung eine Verknüpfung mit der externen Website herstellen können. In der Regel können Sie das benötigte Markup kopieren und den Text leicht ändern.

Schritt-für-Schritt-Anleitungen finden Sie im Thema [Aktivieren der Anmeldung von externen Standorten an einem ASP.net Web Pages-Standort](https://go.microsoft.com/fwlink/?LinkId=251969).

Nachdem sich ein Benutzer von einem anderen Standort aus angemeldet hat, kehrt der Benutzer zu Ihrer Website *zurück und ordnet* diese Anmeldung Ihrer Website zu. Dadurch wird auf Ihrer Website ein Mitgliedschafts Eintrag für die externe Anmeldung des Benutzers erstellt. Auf diese Weise können Sie die normalen Möglichkeiten der Mitgliedschaft (z. b. Rollen) mit der externen Anmeldung verwenden.

## <a name="adding-security-to-an-existing-website"></a>Hinzufügen von Sicherheit zu einer vorhandenen Website

Das zuvor in diesem Artikel beschriebene Verfahren basiert auf der Verwendung der **Starter Site** -Vorlage als Grundlage für die Website Sicherheit. Wenn es nicht praktikabel ist, von der **Starter Site** -Vorlage aus zu starten oder die relevanten Seiten von einer Website basierend auf dieser Vorlage zu kopieren, können Sie die gleiche Art von Sicherheit auf Ihrer eigenen Website implementieren, indem Sie Sie selbst codieren. Sie erstellen dieselben Seiten Typen – Registrierung, Anmeldung usw. – und verwenden dann Hilfsprogramme und Klassen, um die Mitgliedschaft einzurichten.

Der grundlegende Prozess wird im Blogbeitrag [der einfachsten Methode zum Implementieren von ASP.net Razor Security](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)beschrieben. Die meisten Aufgaben werden mithilfe der folgenden Methoden und Eigenschaften des `WebSecurity`-Hilfsprogramms ausgeführt:

- [Websecurty. Userist](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity. kreateuserandaccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Mit diesen Methoden können Sie feststellen, ob bereits eine Person registriert ist, und Sie registrieren.
- [Websecurty. IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Mit dieser Eigenschaft können Sie feststellen, ob der aktuelle Benutzer angemeldet ist. Dies ist nützlich, um Benutzer zu einer Anmeldeseite umzuleiten, wenn Sie noch nicht angemeldet sind.
- [WebSecurity. Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity. Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Diese Methoden protokollieren einen Benutzer ein oder aus.
- [WebSecurity. CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Diese Eigenschaft ist nützlich, wenn der angemeldete Name des aktuellen Benutzers angezeigt wird (wenn der Benutzer angemeldet ist).
- [WebSecurity. confirmaccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Diese Methode ist nützlich, wenn Sie eine e-Mail-Bestätigung für die Registrierung einrichten. (Details werden im Blogbeitrag [mithilfe der Bestätigungsfunktion für die ASP.net Web Pages Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)beschrieben.)

Zum Verwalten von Rollen können Sie die [Rollen](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) und [Mitgliedschafts](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) Klassen verwenden, wie im Blogeintrag beschrieben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Anpassen des Verhaltens von Websiteseiten](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sichern der Webkommunikation: Zertifikate, SSL und https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Die einfachste Methode zum Implementieren von ASP.net Razor Security](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) und [die Verwendung des Bestätigungs Features für die ASP.net Web Pages Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Dabei handelt es sich um Blogbeiträge, in denen beschrieben wird, wie Sie ASP.net-Mitgliedschafts Features ohne die Vorlage " **Starter Site** "
- [Aktivieren der Anmeldung über externe Websites einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity-Klassen-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [API-Referenz für die simpleroleprovider-Klasse](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Simplemembership shipprovider-Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
