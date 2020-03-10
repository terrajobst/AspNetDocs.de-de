---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Wiederherstellen und Ändern vonC#Kenn Wörtern () | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET enthält zwei websteuer Elemente, die das Wiederherstellen und Ändern von Kenn Wörtern unterstützen. Das PasswordRecovery-Steuerelement ermöglicht einem Besucher, seinen verlorenen PA wiederherzustellen...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 8c07b8a3c36e4863c6d2d356b8483544ac4cafeb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457191"
---
# <a name="recovering-and-changing-passwords-c"></a>Wiederherstellen und Ändern von Kennwörtern (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET enthält zwei websteuer Elemente, die das Wiederherstellen und Ändern von Kenn Wörtern unterstützen. Das PasswordRecovery-Steuerelement ermöglicht einem Besucher, sein verlorenes Kennwort wiederherzustellen. Das ChangePassword-Steuerelement ermöglicht dem Benutzer das Aktualisieren seines Kennworts. Wie die anderen Anmelde bezogenen websteuer Elemente, die wir in dieser tutorialreihe gesehen haben, arbeiten die PasswordRecovery-und ChangePassword-Steuerelemente mit dem Mitgliedschafts Framework im Hintergrund, um Benutzer Kennwörter zurückzusetzen oder zu ändern.

## <a name="introduction"></a>Einführung

Zwischen den Websites für meine Bank, das Hilfsprogramm-Unternehmen, das Telefonunternehmen, e-Mail-Konten und personalisierte Webportale habe ich, wie bei den meisten Benutzern, Dutzende unterschiedlicher Kenn Wörter zu merken. Mit so vielen Anmelde Informationen, um diese Tage zu merken, ist es nicht ungewöhnlich, dass Personen das Kennwort vergessen. Um dies zu berücksichtigen, müssen Websites, die Benutzerkonten anbieten, eine Möglichkeit für einen Benutzer enthalten, sein Kennwort wiederherzustellen. Dieser Prozess umfasst in der Regel das Erstellen eines neuen, zufälligen Kennworts und das Senden per e-Mail an die e-Mail-Adresse des Benutzers in der Datei. Nachdem Sie das neue Kennwort erhalten haben, kehren die Benutzer an die Website zurück und ändern Ihr Kennwort von der zufällig generierten in eine besser einprägsame.

ASP.NET enthält zwei websteuer Elemente, die das Wiederherstellen und Ändern von Kenn Wörtern unterstützen. Das PasswordRecovery-Steuerelement ermöglicht einem Besucher, sein verlorenes Kennwort wiederherzustellen. Das ChangePassword-Steuerelement ermöglicht dem Benutzer das Aktualisieren seines Kennworts. Wie die anderen Anmelde bezogenen websteuer Elemente, die wir in dieser tutorialreihe gesehen haben, arbeiten die PasswordRecovery-und ChangePassword-Steuerelemente mit dem Mitgliedschafts Framework im Hintergrund, um Benutzer Kennwörter zurückzusetzen oder zu ändern.

In diesem Tutorial wird die Verwendung dieser beiden Steuerelemente untersucht. Außerdem erfahren Sie, wie Sie das Kennwort eines Benutzers Programm gesteuert ändern und zurücksetzen, indem Sie die `ChangePassword`-und `ResetPassword`-Methoden der `MembershipUser` Klasse durchsetzen.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Schritt 1: unterstützen der Wiederherstellung verlorener Kenn Wörter

Alle Websites, die Benutzerkonten unterstützen, müssen Benutzern einen Mechanismus zur Wiederherstellung Ihrer vergessenen Kenn Wörter bereitstellen. Die gute Nachricht ist, dass die Implementierung dieser Funktionalität in ASP.net dank des websteuer Elements PasswordRecovery eine Kinderspiel ist. Das PasswordRecovery-Steuerelement rendert eine Schnittstelle, die den Benutzer zur Eingabe des Benutzernamens auffordert und ggf. die Antwort auf seine Sicherheitsfrage. Anschließend sendet er eine e-Mail an das Kennwort des Benutzers.

> [!NOTE]
> Da e-Mail-Nachrichten über das Netzwerk im Klartext übertragen werden, besteht das Sicherheitsrisiko, dass das Kennwort eines Benutzers per e-Mail gesendet wird.

Das PasswordRecovery-Steuerelement besteht aus drei Ansichten:

- **Username** : fordert den Besucher zum Benutzernamen auf. Dies ist die anfängliche Ansicht.
- **Frage**: zeigt den Benutzernamen und die Sicherheitsfrage des Benutzers als Text an, zusammen mit einem Textfeld, in dem der Benutzer die Antwort auf seine Sicherheitsfrage eingeben kann.
- **Erfolg**: zeigt eine Meldung an, die den Benutzer darüber informiert, dass sein Kennwort per e-Mail gesendet wurde.

Die angezeigten Sichten und Aktionen, die vom PasswordRecovery-Steuerelement ausgeführt werden, hängen von den folgenden Mitgliedschafts Konfigurationseinstellungen ab:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Die `RequiresQuestionAndAnswer` Einstellung des Mitgliedschafts Frameworks gibt an, ob Benutzer eine Sicherheitsfrage und-Antwort angeben müssen, wenn Sie sich für ein Konto registrieren. Wie im <a id="_msoanchor_1"> </a>Tutorial [*Erstellen von Benutzerkonten*](../membership/creating-user-accounts-cs.md) erläutert, enthält die Schnittstelle von "Benutzerkonten", wenn `RequiresQuestionAndAnswer` true (Standard) ist, Textfeld-Steuerelemente für die Sicherheitsfrage und-Antwort des neuen Benutzers. Wenn `RequiresQuestionAndAnswer` false ist, werden keine derartigen Informationen gesammelt. Wenn `RequiresQuestionAndAnswer` true ist, zeigt das PasswordRecovery-Steuerelement auch die Frage Ansicht an, nachdem der Benutzer seinen Benutzernamen eingegeben hat. das Kennwort wird nur wieder hergestellt, wenn der Benutzer die richtige Sicherheits Antwort eingibt. Wenn `RequiresQuestionAndAnswer` jedoch false ist, wechselt das PasswordRecovery-Steuerelement direkt von der Benutzernamen Ansicht zur Ansicht Erfolg.

Wenn der Benutzer seinen Benutzernamen oder seinen Benutzernamen und die Sicherheits Antwort angegeben hat, wenn `RequiresQuestionAndAnswer` den Wert true hat, sendet der PasswordRecovery-Benutzer sein Kennwort per e-Mail an den Benutzer. Wenn die `EnablePasswordRetrieval`-Option auf true festgelegt ist, wird dem Benutzer das aktuelle Kennwort per e-Mail zugestellt. Wenn der Wert auf false festgelegt ist und `EnablePasswordReset` auf true festgelegt ist, generiert das PasswordRecovery-Steuerelement ein neues, zufälliges Kennwort für den Benutzer und sendet dieses neue Kennwort per e-Mail an Sie. Wenn sowohl `EnablePasswordRetrieval` als auch `EnablePasswordReset` false sind, löst das PasswordRecovery-Steuerelement eine Ausnahme aus.

> [!NOTE]
> Beachten Sie, dass die `SqlMembershipProvider` die Kenn Wörter der Benutzer in einem von drei Formaten speichert: Clear, Hashed (Standard) oder verschlüsselt. Der verwendete Speichermechanismus hängt von den Mitgliedschafts Konfigurationseinstellungen ab. die Demoanwendung verwendet das Hashwert-Kennwort. Bei Verwendung des hashformats für den Hashwert muss die `EnablePasswordRetrieval` Option auf "false" festgelegt werden, da das System das tatsächliche Kennwort des Benutzers aus der in der Datenbank gespeicherten Hash Version nicht ermitteln kann.

In Abbildung 1 wird veranschaulicht, wie die Schnittstelle und das Verhalten von PasswordRecovery von der Mitgliedschafts Konfiguration beeinflusst werden.

[![die Elemente "Requirements andanswer", "EnablePasswordRetrieval" und "EnablePasswordReset" das Aussehen und Verhalten des PasswordRecovery-Steuer Elements beeinflussen.](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Abbildung 1**: die `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`und `EnablePasswordReset` beeinflussen die Darstellung und das Verhalten des PasswordRecovery-Steuer Elements ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image3.png))

> [!NOTE]
> <a id="_msoanchor_2"> </a>Im Tutorial [*Erstellen des Mitgliedschafts Schemas in SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) haben wir den Mitgliedschafts Anbieter konfiguriert, indem wir `RequiresQuestionAndAnswer` auf true festgelegt haben, `EnablePasswordRetrieval` auf false und `EnablePasswordReset` auf true.

### <a name="using-the-passwordrecovery-control"></a>Verwenden des PasswordRecovery-Steuer Elements

Betrachten wir nun die Verwendung des PasswordRecovery-Steuer Elements auf einer ASP.NET-Seite. Öffnen Sie `RecoverPassword.aspx`, und ziehen Sie das PasswordRecovery-Steuerelement per Drag & Drop aus der Toolbox auf den Designer. Legen Sie den `ID` auf `RecoverPwd`fest. Wie die websteuer Elemente Login und WebControl, werden die Ansichten des PasswordRecovery-Steuer Elements eine umfangreiche zusammengesetzte Schnittstelle, die Bezeichnungen, Textfelder, Schaltflächen und Validierungs Steuerelemente enthält. Sie können die Darstellung der Ansichten über die Stileigenschaften des Steuer Elements anpassen oder indem Sie die Ansichten in Vorlagen umwandelt. Ich lasse dies als Übung für den interessierten Reader aus.

Wenn ein Benutzer auf diese Seite wechselt, gibt er seinen Benutzernamen ein und klickt auf die Schaltfläche "Senden". Da wir in unseren Konfigurationseinstellungen für die Mitgliedschaft die `RequiresQuestionAndAnswer`-Eigenschaft auf true festgelegt haben, zeigt das PasswordRecovery-Steuerelement die Frage Ansicht an. Nachdem der Benutzer die richtige Sicherheits Antwort eingegeben und auf "Senden" geklickt hat, aktualisiert das PasswordRecovery-Steuerelement das Kennwort des Benutzers auf ein zufällig generiertes Kennwort und sendet dieses Kennwort an die e-Mail-Adresse in der Datei. All dies war möglich, ohne dass wir eine einzige Codezeile schreiben mussten.

Vor dem Testen dieser Seite gibt es eine letzte Konfiguration, die in der Regel gilt: Wir müssen die Einstellungen für die e-Mail-Übermittlung in `Web.config`angeben. Das PasswordRecovery-Steuerelement ist für das Senden der e-Mail auf diese Einstellungen angewiesen.

Die Konfiguration der e-Mail-Übermittlung wird über das [`<mailSettings>`-Element](https://msdn.microsoft.com/library/w355a94k.aspx)des [`<system.net>` Elements](https://msdn.microsoft.com/library/6484zdc1.aspx)angegeben. Verwenden Sie das [`<smtp>`-Element](https://msdn.microsoft.com/library/ms164240.aspx) , um die Übermittlungs Methode und die standardmäßige from-Adresse anzugeben. Das folgende Markup konfiguriert e-Mail-Einstellungen für die Verwendung eines SMTP-Netzwerkservers mit dem Namen `smtp.example.com` an Port 25 und mit Benutzernamen-/Kennwort-Anmelde Informationen für username und Password

> [!NOTE]
> `<system.net>` ist ein untergeordnetes Element des root `<configuration>`-Elements und ein gleich geordnetes Element von `<system.web>`. Legen Sie daher das `<system.net>`-Element nicht innerhalb des `<system.web>`-Elements ab. Legen Sie Sie stattdessen auf derselben Ebene ab.

[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Zusätzlich zur Verwendung eines SMTP-Servers im Netzwerk können Sie alternativ auch ein Pickup-Verzeichnis angeben, in dem die zu sendenden e-Mail-Nachrichten abgelegt werden sollen.

Nachdem Sie die SMTP-Einstellungen konfiguriert haben, besuchen Sie die Seite `RecoverPassword.aspx` über einen Browser. Versuchen Sie zunächst, einen Benutzernamen einzugeben, der nicht im Benutzerspeicher vorhanden ist. Wie Abbildung 2 zeigt, zeigt das PasswordRecovery-Steuerelement eine Meldung mit dem Hinweis an, dass auf die Benutzerinformationen nicht zugegriffen werden konnte. Der Text der Nachricht kann durch die [`UserNameFailureText`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)des Steuer Elements angepasst werden.

[![eine Fehlermeldung angezeigt wird, wenn ein ungültiger Benutzername eingegeben wird.](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Abbildung 2**: eine Fehlermeldung wird angezeigt, wenn ein ungültiger Benutzername eingegeben wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image6.png))

Geben Sie jetzt einen Benutzernamen ein. Verwenden Sie den Benutzernamen eines Kontos im System mit einer e-Mail-Adresse, auf die Sie zugreifen können, und deren Sicherheits Antwort Ihnen bekannt ist. Nach dem Eingeben des Benutzernamens und dem Klicken auf "Senden" zeigt das PasswordRecovery-Steuerelement seine Frage Ansicht an. Wie bei der Benutzernamen Ansicht zeigt das PasswordRecovery-Steuerelement, wenn Sie eine falsche Antwort eingeben, eine Fehlermeldung an (siehe Abbildung 3). Verwenden Sie die [`QuestionFailureText`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) , um diese Fehlermeldung anzupassen.

[![eine Fehlermeldung angezeigt wird, wenn der Benutzer eine ungültige Sicherheits Antwort eingibt](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Abbildung 3**: eine Fehlermeldung wird angezeigt, wenn der Benutzer eine ungültige Sicherheits Antwort eingibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image9.png))

Geben Sie abschließend die richtige Sicherheits Antwort ein, und klicken Sie auf senden. Im Hintergrund generiert das PasswordRecovery-Steuerelement ein zufälliges Kennwort, weist es dem Benutzerkonto zu, sendet eine e-Mail, in der der Benutzer über sein neues Kennwort informiert wird (siehe Abbildung 4) und zeigt dann die Erfolgs Ansicht an.

[![dem Benutzer eine e-Mail mit seinem neuen Kennwort gesendet](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Abbildung 4**: dem Benutzer wird eine e-Mail mit seinem neuen Kennwort gesendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image12.png))

### <a name="customizing-the-email"></a>Anpassen der e-Mail

Die vom PasswordRecovery-Steuerelement gesendete Standard-e-Mail ist recht langweilig (siehe Abbildung 4). Die Nachricht wird von dem Konto gesendet, das im `from`-Attribut des `<smtp>` Elements mit dem Antragsteller Kennwort und dem nur-Text-Text angegeben ist:

Kehren Sie zur Website zurück, und melden Sie sich mit den folgenden Informationen an.

Benutzername: *Benutzer* Name

Kennwort: *Kennwort*

Diese Nachricht kann Programm gesteuert über einen Ereignishandler für das [`SendingMail` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)des PasswordRecovery-Steuer Elements oder deklarativ über die [`MailDefinition`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx)angepasst werden. Sehen wir uns diese beiden Optionen an.

Das `SendingMail` Ereignis wird vor dem Senden der e-Mail-Nachricht ausgelöst und ist die letzte Möglichkeit, die e-Mail-Nachricht Programm gesteuert anzupassen. Wenn dieses Ereignis ausgelöst wird, wird dem Ereignishandler ein Objekt vom Typ [`MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), dessen `Message`-Eigenschaft einen Verweis auf die zu sendende e-Mail-Adresse enthält.

Erstellen Sie einen Ereignishandler für das `SendingMail`-Ereignis, und fügen Sie den folgenden Code hinzu, der Programm gesteuert `webmaster@example.com` der CC-Liste hinzufügt.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

Die e-Mail-Nachricht kann auch über deklarative Mittel konfiguriert werden. Die `MailDefinition`-Eigenschaft von PasswordRecovery ist ein Objekt vom Typ [`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). Die `MailDefinition`-Klasse bietet eine Reihe von e-Mail-bezogenen Eigenschaften, einschließlich `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`und anderen. Legen Sie zunächst die [`Subject`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) auf einen aussagekräftigeren Wert fest als den standardmäßig verwendeten (Password), z. b. Wenn Ihr Kennwort zurückgesetzt wurde...

Um den Text der e-Mail-Nachricht anzupassen, müssen wir eine separate e-Mail-Vorlagen Datei erstellen, die den Inhalt des Texts enthält. Erstellen Sie zunächst einen neuen Ordner auf der Website mit dem Namen `EmailTemplates`. Fügen Sie als nächstes dem Ordner `PasswordRecovery.txt` eine neue Textdatei mit dem Namen hinzu, und fügen Sie den folgenden Inhalt hinzu:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Beachten Sie die Verwendung der Platzhalter `<%UserName%>` und `<%Password%>`. Das PasswordRecovery-Steuerelement ersetzt diese beiden Platzhalter automatisch durch den Benutzernamen des Benutzers und das wiederhergestellte Kennwort vor dem Senden der e-Mail.

Zeigen Sie schließlich die`BodyFileName`- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) des `MailDefinition`auf die soeben erstellte e-Mail-Vorlage (`~/EmailTemplates/PasswordRecovery.txt`).

Nachdem Sie diese Änderungen vorgenommen haben, besuchen Sie die `RecoverPassword.aspx` Seite, und geben Sie Ihren Benutzernamen und Ihre Sicherheits Antwort Sie erhalten eine e-Mail, die wie in Abbildung 5 aussieht. Beachten Sie, dass `webmaster@example.com` CC 'd war und dass der Betreff und der Text aktualisiert wurden.

[![Betreff, Text und CC-Liste aktualisiert](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Abbildung 5**: Betreff, Text und CC-Liste wurden aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image15.png))

Um eine HTML-formatierte e-Mail-Adresse [`IsBodyHtml`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) zu senden, geben Sie "true" an (der Standardwert ist "false"), und aktualisieren Sie die

Die `MailDefinition`-Eigenschaft ist nicht eindeutig für die PasswordRecovery-Klasse. Wie wir in Schritt 2 sehen werden, bietet das ChangePassword-Steuerelement auch eine `MailDefinition`-Eigenschaft. Außerdem enthält das Steuerelement "kreateuserwizard" eine solche Eigenschaft, die Sie so konfigurieren können, dass automatisch eine Willkommens-e-Mail-Nachricht an neue Benutzer gesendet wird.

> [!NOTE]
> Zurzeit sind keine Links im linken Navigationsbereich zum Erreichen der `RecoverPassword.aspx` Seite vorhanden. Ein Benutzer möchte nur diese Seite besuchen, wenn er sich nicht erfolgreich an der Website anmelden konnte. Aktualisieren Sie daher die `Login.aspx` Seite so, dass Sie einen Link zur `RecoverPassword.aspx` Seite enthält.

### <a name="programmatically-resetting-a-users-password"></a>Programm gesteuertes Zurücksetzen des Kennworts eines Benutzers

Beim Zurücksetzen des Kennworts eines Benutzers Ruft das PasswordRecovery-Steuerelement die [`ResetPassword`-Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)des `MembershipUser` Objekts auf. Diese Methode verfügt über zwei Überladungen:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** : setzt das Kennwort eines Benutzers zurück. Verwenden Sie diese Überladung, wenn `RequiresQuestionAndAnswer` false ist.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** : setzt das Kennwort eines Benutzers nur zurück, wenn die angegebene *securityanswer* korrekt ist. Verwenden Sie diese Überladung, wenn `RequiresQuestionAndAnswer` true ist.

Beide über Ladungen geben das neue, zufällig generierte Kennwort zurück.

Wie bei den anderen Methoden im Mitgliedschafts Framework delegiert die `ResetPassword`-Methode an den konfigurierten Anbieter. Der `SqlMembershipProvider` Ruft die gespeicherte Prozedur `aspnet_Membership_ResetPassword` auf und übergibt den Benutzernamen, das neue Kennwort und die angegebene Kenn Wort Antwort in anderen Feldern. Die gespeicherte Prozedur stellt sicher, dass die Kenn Wort Antwort übereinstimmt, und aktualisiert dann das Kennwort des Benutzers.

Einige der Low-Level-Implementierungs Hinweise:

- Ein gesperrter Benutzer kann sein Kennwort nicht zurücksetzen. Ein nicht genehmigter Benutzer darf jedoch nicht. Im Tutorial zum <a id="_msoanchor_3"> </a> [*entsperren und Genehmigen von Benutzer*](unlocking-and-approving-user-accounts-cs.md) Konten werden die Status "gesperrt" und "genehmigt" ausführlicher erläutert.
- Wenn die Kenn Wort Antwort falsch ist, wird die Anzahl der Fehler bei der Kenn Wort Antwort des Benutzers inkrementiert. Wenn innerhalb eines bestimmten Zeitfensters eine angegebene Anzahl ungültiger Sicherheits Antwort Versuche auftritt, wird der Benutzer gesperrt.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Ein Wort zum Generieren der Zufalls Kennwörter

Die zufällig generierten Kenn Wörter, die in den e-Mail-Nachrichten in Abbildung 4 und 5 angezeigt werden, werden durch die [`GeneratePassword`-Methode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)der Mitgliedschafts Klasse erstellt Diese Methode akzeptiert zwei ganzzahlige Eingabeparameter: *length* und *numofnonalpha anenumericcharacters* -und gibt eine Zeichenfolge mit einer Länge von mindestens *Längen* Zeichen mit mindestens *Nummerierungen nicht alpha* numerischen Zeichen zurück. Wenn diese Methode innerhalb der Mitgliedschafts Klassen oder der Anmelde bezogenen websteuer Elemente aufgerufen wird, werden die Werte für diese beiden Parameter durch die `MinRequiredPasswordLength`-und `MinRequiredNonalphanumericCharacters` Eigenschaften der Mitgliedschafts Konfiguration bestimmt, die wir auf 7 bzw. 1 festlegen.

Die `GeneratePassword`-Methode verwendet einen kryptografisch starken Zufallszahlengenerator, um sicherzustellen, dass es keine Abweichung gibt, welche Zufallszeichen ausgewählt werden. Darüber hinaus wird `GeneratePassword` `public`. Dies bedeutet, dass Sie es direkt aus der ASP.NET-Anwendung verwenden können, wenn Sie zufällige Zeichen folgen oder Kenn Wörter generieren müssen.

> [!NOTE]
> Die `SqlMembershipProvider`-Klasse generiert immer ein zufälliges Kennwort mit einer Länge von mindestens 14 Zeichen. wenn `MinRequiredPasswordLength` also kleiner als 14 ist, wird der Wert ignoriert.

## <a name="step-2-changing-passwords"></a>Schritt 2: Ändern von Kenn Wörtern

Die nach dem Zufallsprinzip generierten Kenn Wörter sind schwer zu merken. Sehen Sie sich das in Abbildung 4: `WWGUZv(f2yM:Bd`gezeigte Kennwort an. Versuchen Sie, diese an den Speicher zu übergeben! Wenn ein Benutzer ein zufällig generiertes Kennwort dieser Art erhält, empfiehlt es sich, das Kennwort zu ändern.

Verwenden Sie das Steuerelement ChangePassword, um eine Schnittstelle für einen Benutzer zu erstellen, um Ihr Kennwort zu ändern Ähnlich wie das PasswordRecovery-Steuerelement besteht das ChangePassword-Steuerelement aus zwei Ansichten: Ändern von Kennwort und Erfolg. In der Ansicht Kennwort ändern wird der Benutzer aufgefordert, seine alten und neuen Kenn Wörter einzugeben. Wenn Sie das richtige alte Kennwort und ein neues Kennwort angeben, das die Mindestanforderungen für die Länge und die nicht-alphanumerische Zeichen erfüllt, aktualisiert das ChangePassword-Steuerelement das Kennwort des Benutzers und zeigt die Ansicht Erfolg an.

> [!NOTE]
> Das ChangePassword-Steuerelement ändert das Kennwort des Benutzers, indem die [`ChangePassword`-Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)des `MembershipUser` Objekts aufgerufen wird. Die ChangePassword-Methode akzeptiert zwei `string` Eingabe Parametern ( *oldPassword* und *newPassword*) und aktualisiert das Konto des Benutzers mit dem *newPassword*, vorausgesetzt, dass das angegebene *oldPassword* korrekt ist.

Öffnen Sie die Seite `ChangePassword.aspx`, und fügen Sie der Seite ein ChangePassword-Steuerelement hinzu, und benennen Sie es `ChangePwd`. An diesem Punkt sollte die Designansicht die Ansicht "Kennwort ändern" anzeigen (siehe Abbildung 6). Wie beim PasswordRecovery-Steuerelement können Sie zwischen den Sichten über das Smarttag des-Steuer Elements umschalten. Darüber hinaus können die Darstellungen dieser Sichten durch die Eigenschaften des Sortierungs Stils oder durch die Umsetzung in eine Vorlage anpassbar sein.

[![der Seite ein ChangePassword-Steuerelement hinzufügen](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Abbildung 6**: Hinzufügen eines ChangePassword-Steuer Elements zur Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image18.png))

Mit dem ChangePassword-Steuerelement kann das Kennwort des aktuell angemeldeten Benutzers *oder* das Kennwort eines anderen, angegebenen Benutzers aktualisiert werden. Wie in Abbildung 6 gezeigt, rendert die Standardansicht Kennwort ändern nur drei Textfeld-Steuerelemente: einen für das alte Kennwort und zwei für das neue Kennwort. Diese Standardschnittstelle wird verwendet, um das Kennwort des aktuell angemeldeten Benutzers zu aktualisieren.

Wenn Sie das ChangePassword-Steuerelement verwenden möchten, um das Kennwort eines anderen Benutzers zu aktualisieren, legen Sie die [`DisplayUserName`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) des Steuer Elements Dadurch wird der Seite ein viertes Textfeld hinzugefügt, in dem Sie zur Eingabe des Benutzernamens des Benutzers aufgefordert werden, dessen Kennwort geändert werden soll.

Wenn Sie `DisplayUserName` auf "true" festlegen, können Sie zulassen, dass ein abangemeldeter Benutzer sein Kennwort ändert, ohne sich anmelden zu müssen. Ich denke, es gibt keinen Fehler, wenn sich ein Benutzer anmelden muss, bevor er Ihr Kennwort ändern kann. Lassen Sie daher `DisplayUserName` auf false festgelegt (Standardeinstellung). Bei dieser Entscheidung wird jedoch im Grunde genommen, dass anonyme Benutzer nicht auf diese Seite zugreifen können. Aktualisieren Sie die URL-Autorisierungs Regeln der Site, um anonymen Benutzern den Besuch `ChangePassword.aspx`zu verweigern. Wenn Sie den Arbeitsspeicher über die Syntax der URL-Autorisierungs Regel aktualisieren müssen, finden <a id="_msoanchor_4"> </a>Sie weitere Informationen im Tutorial zur [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) .

> [!NOTE]
> Es scheint so, als ob die `DisplayUserName`-Eigenschaft nützlich ist, damit Administratoren die Kenn Wörter anderer Benutzer ändern können. Auch wenn `DisplayUserName` auf "true" festgelegt ist, muss das richtige alte Kennwort bekannt und eingegeben werden. Wir besprechen Verfahren, mit denen Administratoren in Schritt 3 Benutzer Kennwörter ändern können.

Besuchen Sie die Seite `ChangePassword.aspx` über einen Browser, und ändern Sie Ihr Kennwort. Beachten Sie, dass eine Fehlermeldung angezeigt wird, wenn Sie ein neues Kennwort eingeben, das die Anforderungen an die Kenn Wort Länge und nicht-alphanumerische Zeichen nicht erfüllt (siehe Abbildung 7).

[![der Seite ein ChangePassword-Steuerelement hinzufügen](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Abbildung 7**: Hinzufügen eines ChangePassword-Steuer Elements zur Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image21.png))

Nachdem Sie das richtige alte Kennwort eingegeben und ein gültiges neues Kennwort eingegeben haben, wird das Kennwort des angemeldeten Benutzers geändert, und die Ansicht Erfolg wird angezeigt.

### <a name="sending-a-confirmation-email"></a>Senden einer Bestätigungs-e-Mail

Standardmäßig sendet das Steuerelement ChangePassword keine e-Mail-Nachricht an den Benutzer, dessen Kennwort soeben aktualisiert wurde. Wenn Sie eine e-Mail senden möchten, konfigurieren Sie einfach die `MailDefinition`-Eigenschaft des Steuer Elements. Wir konfigurieren das ChangePassword-Steuerelement so, dass dem Benutzer eine HTML-formatierte e-Mail-Nachricht mit dem neuen Kennwort gesendet wird.

Erstellen Sie zunächst eine neue Datei im Ordner "`EmailTemplates`" mit dem Namen `ChangePassword.htm`. Fügen Sie das folgende Markup hinzu:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Legen Sie als nächstes die Eigenschaften `BodyFileName`, `IsBodyHtml`und `Subject` der Change`MailDefinition` Password-Eigenschaft auf ~/EmailTemplates/ChangePassword.htm, true und Ihr Kennwort geändert.

Nachdem Sie diese Änderungen vorgenommen haben, besuchen Sie die Seite, und ändern Sie Ihr Kennwort erneut. Dieses Mal sendet das Steuerelement ChangePassword eine angepasste, HTML-formatierte e-Mail an die e-Mail-Adresse des Benutzers in der Datei (siehe Abbildung 8).

[![eine e-Mail-Nachricht informiert den Benutzer darüber, dass sein Kennwort geändert wurde](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Abbildung 8**: eine e-Mail-Nachricht informiert den Benutzer darüber, dass sein Kennwort geändert wurde ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image24.png))

## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Schritt 3: Zulassen von Administratoren zum Ändern von Benutzer Kennwörtern

Ein gängiges Feature in Anwendungen, die Benutzerkonten unterstützen, ist die Möglichkeit eines Administrators, andere Benutzer Kennwörter zu ändern. Diese Funktionalität ist manchmal erforderlich, da das System nicht in der Lage ist, Benutzer ihre eigenen Kenn Wörter zu ändern. In einem solchen Fall kann ein Benutzer, der sein vergessenes Kennwort wiederherstellen möchte, dem Administrator ein neues Kennwort zuweisen. Mit den Steuerelementen "PasswordRecovery" und "ChangePassword" müssen sich Administratoren jedoch nicht selbst mit den Kenn Wörtern der Benutzer in der Lage sein, diese zu ändern, da die Benutzer dies selbst tun können.

Was passiert jedoch, wenn Ihr Client darauf beharrt, dass Administratoren in der Lage sein sollen, die Kenn Wörter anderer Benutzer zu ändern? Leider kann das Hinzufügen dieser Funktionalität etwas Arbeit sein. Um das Kennwort eines Benutzers zu ändern, müssen sowohl das alte als auch das neue Kennwort für die `ChangePassword`-Methode des `MembershipUser` Objekts bereitgestellt werden, aber ein Administrator sollte das Kennwort eines Benutzers nicht kennen, um es zu ändern.

Eine Problem Umgehung besteht darin, zuerst das Kennwort des Benutzers zurückzusetzen und dann mithilfe von Code wie dem folgenden in das neue Kennwort zu ändern:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Dieser Code beginnt mit dem Abrufen von Informationen zum *Benutzernamen*, d. h. dem Benutzer, dessen Kennwort der Administrator ändern möchte. Als nächstes wird die `ResetPassword`-Methode aufgerufen, die dem Benutzer ein neues zufälliges Kennwort zuweist. Dieses zufällig generierte Kennwort wird von der-Methode zurückgegeben und in der Variablen `resetPwd`gespeichert. Nun, da wir das Kennwort des Benutzers kennen, können wir es über einen `ChangePassword`ändern.

Das Problem besteht darin, dass dieser Code nur funktioniert, wenn die Konfiguration des Mitgliedschafts Systems so festgelegt ist, dass `RequiresQuestionAndAnswer` false ist. Wenn `RequiresQuestionAndAnswer` true ist, wie es bei der Anwendung der Fall ist, muss der `ResetPassword` Methode die Sicherheits Antwort übergeben werden. andernfalls wird eine Ausnahme ausgelöst.

Wenn das Mitgliedschafts Framework so konfiguriert ist, dass eine Sicherheitsfrage und-Antwort erforderlich ist, und Ihr Client darauf wartet, dass Administratoren Benutzer Kennwörter ändern können, haben Sie drei Möglichkeiten:

- Lösen Sie die Hände in der Luft aus, und teilen Sie dem Client mit, dass dies nur eine Sache ist, die nicht erledigt werden kann.
- Legen Sie `RequiresQuestionAndAnswer` auf false fest. Dies führt zu einer weniger sicheren Anwendung. Stellen Sie sich vor, dass ein Benutzer, der der Benutzer besitzt, Zugriff auf den e-Mail-Posteingang eines Vielleicht hat der gefährdete Benutzer den Schreibtisch verlassen, um zum Mittagessen zu wechseln, und die Arbeitsstation konnte nicht gesperrt werden. In beiden Fällen kann der Benutzer, der die Seite `RecoverPassword.aspx`, besuchen und den Benutzernamen des Benutzers eingeben. Das System wird dann das wiederhergestellte Kennwort per e-Mail senden, ohne zur Sicherheits Antwort aufzufordern.
- Umgehen Sie die Abstraktions Ebene, die vom Mitgliedschafts Framework erstellt wurde, und arbeiten Sie direkt mit der SQL Server Datenbank. Das Mitgliedschafts Schema enthält eine gespeicherte Prozedur mit dem Namen `aspnet_Membership_SetPassword`, mit der das Kennwort eines Benutzers festgelegt wird und die Sicherheits Antwort oder das alte Kennwort nicht erforderlich ist, um die Aufgabe zu erledigen.

Keine dieser Optionen ist besonders ansprechend, aber das ist die Lebensdauer eines Entwicklers.

Ich habe den dritten Ansatz implementiert und Code geschrieben, der die `Membership`-und `MembershipUser` Klassen umgeht und direkt mit der `SecurityTutorials` Datenbank arbeitet.

> [!NOTE]
> Wenn Sie direkt mit der Datenbank arbeiten, wird die Kapselung, die vom Mitgliedschafts Framework bereitgestellt wird, zerstört. Diese Entscheidung verbindet uns mit dem `SqlMembershipProvider`, sodass der Code weniger portabel ist. Außerdem funktioniert dieser Code in zukünftigen Versionen von möglicherweise nicht erwartungsgemäß ASP.net Wenn das Mitgliedschafts Schema geändert wird. Diese Vorgehensweise ist eine Problem Umgehung, und wie bei den meisten Problem Umgehungen ist es kein Beispiel für bewährte Methoden.

Der Code weist einige unschöne Bits auf und ist recht lang. Daher möchte ich dieses Tutorial nicht mit einer detaillierten Untersuchung überladen. Wenn Sie mehr erfahren möchten, laden Sie den Code für dieses Tutorial herunter, und besuchen Sie die Seite `~/Administration/ManageUsers.aspx`. Auf dieser Seite, die wir im <a id="_msoanchor_5"> </a> [vorherigen Tutorial](building-an-interface-to-select-one-user-account-from-many-cs.md)erstellt haben, werden die einzelnen Benutzer aufgelistet. Ich habe die GridView so aktualisiert, dass Sie einen Link zur `UserInformation.aspx` Seite enthält, wobei der Benutzername des ausgewählten Benutzers über die QueryString übergeben wird. Auf der Seite `UserInformation.aspx` werden Informationen zum ausgewählten Benutzer und zu den Textfeldern zum Ändern des Kennworts angezeigt (siehe Abbildung 9).

Nachdem Sie das neue Kennwort eingegeben, im zweiten Textfeld bestätigt und auf die Schaltfläche "Benutzer aktualisieren" geklickt haben, wird ein Postback durchsucht, und die gespeicherte Prozedur `aspnet_Membership_SetPassword` wird aufgerufen, um das Kennwort des Benutzers zu aktualisieren. Ich empfehle den Lesern, die an dieser Funktionalität interessiert sind, mit dem Code vertraut zu machen und die Funktionalität zu erweitern, um das Senden einer e-Mail an den Benutzer zu unterstützen, dessen Kennwort geändert wurde.

[![ein Administrator das Kennwort eines Benutzers ändern darf](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Abbildung 9**: ein Administrator kann das Kennwort eines Benutzers ändern ([Klicken Sie, um das Bild in voller Größe anzuzeigen](recovering-and-changing-passwords-cs/_static/image27.png))

> [!NOTE]
> Die `UserInformation.aspx` Seite funktioniert zurzeit nur, wenn das Mitgliedschafts Framework zum Speichern von Kenn Wörtern im Clear-oder Hashformat konfiguriert ist. Der Code zum Verschlüsseln des neuen Kennworts fehlt, obwohl Sie dazu eingeladen sind, diese Funktionalität hinzuzufügen. Ich empfehle Ihnen, den erforderlichen Code hinzuzufügen, indem Sie einen Decompiler wie [Reflektor](http://www.aisto.com/roeder/dotnet/) zum Untersuchen des Quellcodes für Methoden in der .NET Framework verwenden. beginnen Sie, indem Sie die `ChangePassword`-Methode der `SqlMembershipProvider` Klasse untersuchen. Dies ist das Verfahren, das ich zum Schreiben des Codes zum Erstellen eines Hashwerts für das Kennwort verwendet habe.

## <a name="summary"></a>Zusammenfassung

ASP.net bietet zwei Steuerelemente, die Benutzern helfen, Ihr Kennwort zu verwalten. Das PasswordRecovery-Steuerelement ist hilfreich für Benutzer, die ihre Kenn Wörter vergessen haben. Abhängig von der Konfiguration des Mitgliedschafts-Frameworks wird der Benutzer entweder sein vorhandenes Kennwort oder ein neues, nach dem Zufallsprinzip generiertes Kennwort per e-Mail gesendet. Mit dem ChangePassword-Steuerelement kann ein Benutzer sein Kennwort aktualisieren.

Wie die-Steuerelemente Login und anateuserwizard können die PasswordRecovery-und ChangePassword-Steuerelemente eine umfangreiche Benutzeroberfläche erzeugen, ohne dass Sie einen Lick mit deklarativem Markup oder Codezeile schreiben müssen. Wenn die Standardbenutzer Oberfläche Ihren Anforderungen nicht entspricht, können Sie Sie über verschiedene Stileigenschaften anpassen. Alternativ können die Schnittstellen der Steuerelemente in Vorlagen konvertiert werden, um ein noch feineres Maß an Kontrolle zu erhalten. Im Hintergrund verwenden diese Steuerelemente die Mitgliedschafts-API, indem Sie die `ResetPassword`-und `ChangePassword` Methoden des `MembershipUser` Objekts aufrufen.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ChangePassword-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Schnellstarts für PasswordRecovery-Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Senden von e-Mails in ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` FAQs](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Zu den leitenden Reviewern für dieses Tutorial zählen Michael emmings und Suchi Banerjee. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [Weiter](unlocking-and-approving-user-accounts-cs.md)
