---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
title: Entsperren und Genehmigen von Benutzerkonten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine Webseite für Administratoren zum Verwalten der gesperrten und genehmigten Status von Benutzern erstellen. Wir sehen uns auch an, wie neue Benutzer genehmigt werden...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 041854a5-ea8c-4de0-82f1-121ba6cb2893
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a7474676b8f502c583e226678de2b275e0ea3c7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590631"
---
# <a name="unlocking-and-approving-user-accounts-vb"></a>Entsperren und Genehmigen von Benutzerkonten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.14.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_vb.pdf)

> In diesem Tutorial wird gezeigt, wie Sie eine Webseite für Administratoren zum Verwalten der gesperrten und genehmigten Status von Benutzern erstellen. Außerdem wird erläutert, wie Sie neue Benutzer erst genehmigen, nachdem Sie Ihre e-Mail-Adresse überprüft haben.

## <a name="introduction"></a>Einführung

Zusammen mit einem Benutzernamen, Kennwort und e-Mail-Adresse verfügt jedes Benutzerkonto über zwei Status Felder, die vorgeben, ob sich der Benutzer bei der Website anmelden kann: gesperrt und genehmigt. Ein Benutzer wird automatisch gesperrt, wenn er ungültige Anmelde Informationen in der angegebenen Anzahl von Minuten festgelegt hat (die Standardeinstellungen Sperren einen Benutzer nach fünf ungültigen Anmelde versuchen innerhalb von 10 Minuten). Der genehmigte Status ist in Szenarios nützlich, in denen sich einige Aktionen erneut ausführen müssen, bevor sich ein neuer Benutzer an der Website anmelden kann. Beispielsweise muss ein Benutzer möglicherweise zuerst seine e-Mail-Adresse überprüfen oder von einem Administrator genehmigt werden, bevor er sich anmelden kann.

Da sich ein ausgesperrter oder nicht genehmigter Benutzer nicht anmelden kann, ist es nur natürlich, zu Fragen, wie diese Status zurückgesetzt werden können. ASP.NET enthält keine integrierten Funktionen oder websteuer Elemente zum Verwalten der gesperrten und genehmigten Status von Benutzern, da diese Entscheidungen auf Site-by-Site-Basis behandelt werden müssen. Einige Standorte genehmigen möglicherweise automatisch alle neuen Benutzerkonten (Standardverhalten). Andere Administratoren haben einen Administrator berechtigt, neue Konten zu genehmigen oder Benutzer erst zu genehmigen, wenn Sie einen Link besuchen, der an die e-Mail-Adresse gesendet wird, die bei der Registrierung Ebenso können einige Websites Benutzer sperren, bis ein Administrator Ihren Status zurücksetzt, während andere Standorte eine e-Mail an den gesperrten Benutzer mit einer URL senden, die er zum Entsperren seines Kontos besuchen kann.

In diesem Tutorial wird gezeigt, wie Sie eine Webseite für Administratoren zum Verwalten der gesperrten und genehmigten Status von Benutzern erstellen. Außerdem wird erläutert, wie Sie neue Benutzer erst genehmigen, nachdem Sie Ihre e-Mail-Adresse überprüft haben.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Schritt 1: Verwalten der gesperrten und genehmigten Status von Benutzern

<a id="Tutorial12"> </a>Im Tutorial [*zum Aufbauen einer Schnittstelle zum Auswählen eines Benutzerkontos aus vielen*](building-an-interface-to-select-one-user-account-from-many-vb.md) Lernprogrammen haben wir eine Seite erstellt, die jedes Benutzerkonto in einer auslagerten, gefilterten GridView aufführte. Das Raster listet den Namen und die e-Mail-Adresse der einzelnen Benutzer, deren genehmigte und ausgesperrte Status, ob Sie derzeit online sind, und alle Kommentare zum Benutzer auf. Zum Verwalten der genehmigten und gesperrten Status von Benutzern könnten wir dieses Raster bearbeitbar machen. Um den genehmigten Status eines Benutzers zu ändern, würde der Administrator zuerst das Benutzerkonto suchen und dann die zugehörige GridView-Zeile bearbeiten und das Kontrollkästchen genehmigt aktivieren bzw. deaktivieren. Alternativ können wir die genehmigten und gesperrten Status über eine separate ASP.NET-Seite verwalten.

In diesem Tutorial verwenden wir zwei ASP.NET Seiten: `ManageUsers.aspx` und `UserInformation.aspx`. Die Idee hierbei ist, dass `ManageUsers.aspx` die Benutzerkonten im System auflistet, während `UserInformation.aspx` es dem Administrator ermöglicht, die genehmigten und gesperrten Status für einen bestimmten Benutzer zu verwalten. Die erste Geschäftsordnung besteht darin, die GridView in `ManageUsers.aspx` zu erweitern, um ein HyperLinkField einzuschließen, das als Spalte mit links gerendert wird. Jeder Link soll auf `UserInformation.aspx?user=UserName`verweisen, wobei *username* der Name des zu bearbeitenden Benutzers ist.

> [!NOTE]
> Wenn Sie den Code für das <a id="Tutorial13"> </a>Tutorial zum wieder [*herstellen und ändern*](recovering-and-changing-passwords-vb.md) von Kenn Wörtern heruntergeladen haben, haben Sie möglicherweise bemerkt, dass die `ManageUsers.aspx` Seite bereits einen Satz von "verwalten"-Links enthält und die `UserInformation.aspx` Seite eine Schnittstelle zum Ändern des Kennworts des ausgewählten Benutzers bereitstellt. Ich entschied mich, diese Funktionalität nicht in dem Code zu replizieren, der diesem Tutorial zugeordnet ist, weil die Mitgliedschafts-API umgangen und direkt mit der SQL Server Datenbank betrieben wurde, um das Kennwort eines Benutzers zu ändern. Dieses Tutorial beginnt von Grund auf mit der Seite "`UserInformation.aspx`".

### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Hinzufügen von "Manage"-Links zum`UserAccounts`GridView

Öffnen Sie die Seite `ManageUsers.aspx`, und fügen Sie der `UserAccounts` GridView ein HyperLinkField hinzu. Legen Sie die `Text`-Eigenschaft des HyperLinkField auf "Manage" und seine `DataNavigateUrlFields`-und `DataNavigateUrlFormatString`-Eigenschaften auf `UserName` bzw. "UserInformation. aspx? User ={0}" fest. Mit diesen Einstellungen wird das HyperLinkField so konfiguriert, dass alle Hyperlinks den Text "Manage" anzeigen, aber jeder Link übergibt den entsprechenden Wert für den *Benutzernamen* an die QueryString.

Nehmen Sie nach dem Hinzufügen des HyperLinkField zur GridView einen Moment Zeit, um die `ManageUsers.aspx` Seite über einen Browser anzuzeigen. Wie in Abbildung 1 gezeigt, enthält jede GridView-Zeile nun den Link "verwalten". Der Link "verwalten" für Bruce verweist auf `UserInformation.aspx?user=Bruce`, während der Link "verwalten" für Dave auf `UserInformation.aspx?user=Dave`verweist.

[![das HyperLinkField eine](unlocking-and-approving-user-accounts-vb/_static/image2.png)](unlocking-and-approving-user-accounts-vb/_static/image1.png)

**Abbildung 1**: das HyperLinkField fügt für jedes Benutzerkonto den Link "verwalten" hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image3.png)).

Wir erstellen die Benutzeroberfläche und den Code für die `UserInformation.aspx` Seite in einem Moment, aber zunächst wird erläutert, wie Sie die gesperrten und genehmigten Status eines Benutzers Programm gesteuert ändern. Die [`MembershipUser`-Klasse](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) verfügt über [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) -und [`IsApproved`-Eigenschaften](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). Die `IsLockedOut`-Eigenschaft ist schreibgeschützt. Es gibt keinen Mechanismus zum programmgesteuerten Sperren eines Benutzers. um einen Benutzer zu entsperren, verwenden Sie die [`UnlockUser`-Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)der `MembershipUser`-Klasse. Die `IsApproved`-Eigenschaft ist lesbar und beschreibbar. Um Änderungen an dieser Eigenschaft zu speichern, muss die [`UpdateUser`-Methode](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)der `Membership` Klasse aufgerufen werden, wobei das geänderte `MembershipUser` Objekt übergeben wird.

Da die `IsApproved`-Eigenschaft lesbar und schreibbar ist, ist das CheckBox-Steuerelement wahrscheinlich das beste Benutzeroberflächen Element zum Konfigurieren dieser Eigenschaft. Ein Kontrollkästchen funktioniert jedoch nicht für die `IsLockedOut`-Eigenschaft, da ein Administrator einen Benutzer nicht sperren kann, sondern darf nur einen Benutzer entsperren. Eine geeignete Benutzeroberfläche für die `IsLockedOut`-Eigenschaft ist eine Schaltfläche, mit der das Benutzerkonto entsperrt wird, wenn darauf geklickt wird. Diese Schaltfläche sollte nur aktiviert werden, wenn der Benutzer gesperrt ist.

### <a name="creating-theuserinformationaspxpage"></a>Erstellen der Seite "`UserInformation.aspx`"

Nun können wir die Benutzeroberfläche in `UserInformation.aspx`implementieren. Öffnen Sie diese Seite, und fügen Sie die folgenden websteuer Elemente hinzu:

- Ein Hyperlink-Steuerelement, das den Administrator auf die `ManageUsers.aspx` Seite zurückgibt, wenn darauf geklickt wird.
- Ein Bezeichnungs-websteuer Element zum Anzeigen des Namens des ausgewählten Benutzers. Legen Sie den `ID` dieser Bezeichnung auf `UserNameLabel` und die Text-Eigenschaft fest.
- Ein CheckBox-Steuerelement mit dem Namen `IsApproved`. Legen Sie die `AutoPostBack`-Eigenschaft auf `True`fest.
- Ein Label-Steuerelement zum Anzeigen des letzten gesperrten Datums des Benutzers. Benennen Sie diese Bezeichnung `LastLockedOutDateLabel`, und löschen Sie die `Text`-Eigenschaft.
- Eine Schaltfläche zum Entsperren des Benutzers. Benennen Sie diese Schaltfläche `UnlockUserButton` und legen Sie die `Text`-Eigenschaft auf "Benutzer entsperren" fest.
- Ein Label-Steuerelement zum Anzeigen von Statusmeldungen, z. b. "der genehmigte Status des Benutzers wurde aktualisiert." Benennen Sie dieses Steuerelement `StatusMessage`, löschen Sie dessen `Text`-Eigenschaft, und legen Sie dessen Eigenschaft `CssClass` auf `Important`fest. (Die `Important` CSS-Klasse wird in der `Styles.css` Stylesheet-Datei definiert. Sie zeigt den entsprechenden Text in einer großen, roten Schriftart an.)

Nach dem Hinzufügen dieser Steuerelemente sollte das Designansicht in Visual Studio ähnlich dem Screenshot in Abbildung 2 aussehen.

[![erstellen Sie die Benutzeroberfläche für "UserInformation. aspx".](unlocking-and-approving-user-accounts-vb/_static/image5.png)](unlocking-and-approving-user-accounts-vb/_static/image4.png)

**Abbildung 2**: Erstellen der Benutzeroberfläche für `UserInformation.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image6.png))

Wenn die Benutzeroberfläche fertig ist, besteht die nächste Aufgabe darin, das Kontrollkästchen "`IsApproved`" und andere Steuerelemente basierend auf den Informationen des ausgewählten Benutzers festzulegen. Erstellen Sie einen Ereignishandler für das `Load` Ereignis der Seite, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample1.vb)]

Der obige Code beginnt mit dem sicherstellen, dass dies der erste Besuch der Seite und kein nachfolgendes Postback ist. Anschließend liest Sie den Benutzernamen, der durch das `user` QueryString-Feld weitergegeben wurde, und ruft Informationen zu diesem Benutzerkonto über die `Membership.GetUser(username)`-Methode ab. Wenn kein Benutzername über die QueryString angegeben wurde, oder wenn der angegebene Benutzer nicht gefunden wurde, wird der Administrator zurück an die `ManageUsers.aspx` Seite gesendet.

Der `UserName` Wert des `MembershipUser` Objekts wird dann im `UserNameLabel` angezeigt, und das Kontrollkästchen `IsApproved` wird anhand des `IsApproved`-Eigenschafts Werts geprüft.

Die [`LastLockoutDate`-Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) des `MembershipUser` Objekts gibt einen `DateTime` Wert zurück, der angibt, wann der Benutzer zuletzt gesperrt wurde. Wenn der Benutzer nie gesperrt wurde, hängt der zurückgegebene Wert vom Mitgliedschafts Anbieter ab. Wenn ein neues Konto erstellt wird, legt der `SqlMembershipProvider` das `LastLockoutDate` Feld der `aspnet_Membership` Tabelle auf `1754-01-01 12:00:00 AM`fest. Der obige Code zeigt eine leere Zeichenfolge in der `LastLockoutDateLabel` an, wenn die `LastLockoutDate`-Eigenschaft vor Jahr 2000. Andernfalls wird der Datums Teil der `LastLockoutDate`-Eigenschaft in der Bezeichnung angezeigt. Die `Enabled`-Eigenschaft des `UnlockUserButton`ist auf den gesperrten Status des Benutzers festgelegt, was bedeutet, dass diese Schaltfläche nur aktiviert wird, wenn der Benutzer gesperrt ist.

Nehmen Sie sich einen Moment Zeit, um die `UserInformation.aspx` Seite über einen Browser zu testen. Sie müssen natürlich bei `ManageUsers.aspx` beginnen und ein Benutzerkonto auswählen, das verwaltet werden soll. Beachten Sie, dass das Kontrollkästchen `IsApproved` bei der Ankunft bei `UserInformation.aspx`nur aktiviert ist, wenn der Benutzer genehmigt ist. Wenn der Benutzer jemals gesperrt wurde, wird das Datum der letzten Sperrung angezeigt. Die Schaltfläche Benutzer entsperren ist nur aktiviert, wenn der Benutzer zurzeit gesperrt ist. Wenn Sie das `IsApproved` Kontrollkästchen aktivieren oder deaktivieren, oder indem Sie auf die Schaltfläche Benutzer entsperren klicken, wird ein Postback ausgelöst, aber es werden keine Änderungen am Benutzerkonto vorgenommen, da wir noch keine Ereignishandler für diese Ereignisse erstellen müssen.

Kehren Sie zu Visual Studio zurück, und erstellen Sie Ereignishandler für das `CheckedChanged` Ereignis `IsApproved` und das `Click` Ereignis der `UnlockUser` Schaltfläche. Legen Sie im Ereignishandler für `CheckedChanged` die `IsApproved`-Eigenschaft des Benutzers auf die `Checked`-Eigenschaft des Kontrollkästchens fest, und speichern Sie die Änderungen dann über einen-Befehl `Membership.UpdateUser`. Im `Click`-Ereignishandler können Sie einfach die `UnlockUser`-Methode des `MembershipUser`-Objekts aufzurufen. Zeigen Sie in beiden Ereignis Handlern eine passende Meldung in der `StatusMessage` Bezeichnung an.

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample2.vb)]

### <a name="testing-theuserinformationaspxpage"></a>Testen der Seite "`UserInformation.aspx`"

Wenn diese Ereignishandler vorhanden sind, besuchen Sie die Seite erneut, und wiederholen Sie die Genehmigung für einen Benutzer. Wie in Abbildung 3 gezeigt, sollte auf der Seite eine kurze Meldung angezeigt werden, die anzeigt, dass die `IsApproved`-Eigenschaft des Benutzers erfolgreich geändert wurde.

[![Chris wurde nicht genehmigt](unlocking-and-approving-user-accounts-vb/_static/image8.png)](unlocking-and-approving-user-accounts-vb/_static/image7.png)

**Abbildung 3**: Chris ist nicht genehmigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image9.png))

Melden Sie sich als nächstes an, und melden Sie sich als der Benutzer an, dessen Konto soeben nicht genehmigt war. Da der Benutzer nicht genehmigt ist, kann er sich nicht anmelden. Standardmäßig zeigt das Login-Steuerelement die gleiche Meldung an, wenn sich der Benutzer unabhängig von der Ursache nicht anmelden kann. Im <a id="Tutorial6"> </a>Tutorial überprüfen von [*Benutzer Anmelde Informationen anhand des Mitgliedschafts Benutzer Stores*](../membership/validating-user-credentials-against-the-membership-user-store-vb.md) haben wir uns jedoch mit der Verbesserung des Anmeldungs Steuer Elements für die Anzeige einer geeigneteren Nachricht beschäftigt. Wie Abbildung 4 zeigt, wird Chris eine Meldung mit dem Hinweis angezeigt, dass er sich nicht anmelden kann, weil sein Konto noch nicht genehmigt wurde.

[![Chris kann sich nicht anmelden, da das Konto nicht genehmigt ist.](unlocking-and-approving-user-accounts-vb/_static/image11.png)](unlocking-and-approving-user-accounts-vb/_static/image10.png)

**Abbildung 4**: Chris kann sich nicht anmelden, da das Konto nicht genehmigt ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image12.png))

Um die ausgesperrte Funktionalität zu testen, versuchen Sie, sich als genehmigter Benutzer anzumelden, aber verwenden Sie ein falsches Kennwort. Wiederholen Sie diesen Vorgang so oft wie nötig, bis das Konto des Benutzers gesperrt wurde. Das Anmeldungs Steuerelement wurde ebenfalls aktualisiert und zeigt eine benutzerdefinierte Meldung an, wenn versucht wird, sich bei einem gesperrten Konto anzumelden. Sie wissen, dass ein Konto gesperrt wurde, nachdem Sie die folgende Meldung auf der Anmeldeseite angezeigt haben: "Ihr Konto wurde aufgrund von zu vielen ungültigen Anmelde versuchen gesperrt. Wenden Sie sich an den Administrator, damit das Konto entsperrt wird. "

Kehren Sie zur Seite "`ManageUsers.aspx`" zurück, und klicken Sie auf den Link "verwalten" für den gesperrten Benutzer. Wie in Abbildung 5 gezeigt, sollte ein Wert in der `LastLockedOutDateLabel` die Schaltfläche Unlock User aktiviert werden soll. Klicken Sie auf die Schaltfläche Benutzer entsperren, um das Benutzerkonto zu entsperren. Nachdem Sie den Benutzer entsperrt haben, kann er sich erneut anmelden.

[![Dave wurde aus dem System gesperrt.](unlocking-and-approving-user-accounts-vb/_static/image14.png)](unlocking-and-approving-user-accounts-vb/_static/image13.png)

**Abbildung 5**: Dave wurde aus dem System gesperrt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image15.png))

## <a name="step-2-specifying-new-users-approved-status"></a>Schritt 2: Angeben des genehmigten Status neuer Benutzer

Der genehmigte Status ist in Szenarios nützlich, in denen eine Aktion ausgeführt werden soll, bevor ein neuer Benutzer sich anmelden und auf die benutzerspezifischen Funktionen der Website zugreifen kann. Beispielsweise können Sie eine private Website ausführen, auf der nur authentifizierte Benutzer auf alle Seiten mit Ausnahme der Anmelde-und Registrierungsseiten zugreifen können. Was passiert jedoch, wenn ein unbekannter Ihre Website erreicht, die Registrierungsseite findet und ein Konto erstellt? Um dies zu verhindern, können Sie die Registrierungsseite in einen `Administration` Ordner verschieben und verlangen, dass ein Administrator jedes Konto manuell erstellt. Alternativ können Sie allen Benutzern gestatten, sich zu registrieren, aber den Zugriff auf die Website zu verbieten, bis ein Administrator das Benutzerkonto genehmigt.

Standardmäßig genehmigt das Steuerelement "kreateuserwizard" neue Konten. Sie können dieses Verhalten mithilfe der [`DisableCreatedUser`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)des Steuer Elements konfigurieren. Legen Sie diese Eigenschaft auf `True` fest, um neue Benutzerkonten nicht zu genehmigen.

> [!NOTE]
> Standardmäßig protokolliert das Steuerelement "" von "kreateuserwizard" automatisch das neue Benutzerkonto. Dieses Verhalten wird durch die [`LoginCreatedUser`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)des Steuer Elements vorgegeben. Da nicht genehmigte Benutzer sich nicht bei der Website anmelden können, wird `DisableCreatedUser` `True` das neue Benutzerkonto nicht bei der Website protokolliert, unabhängig vom Wert der `LoginCreatedUser`-Eigenschaft.

Wenn Sie Programm gesteuert neue Benutzerkonten über die `Membership.CreateUser`-Methode erstellen, verwenden Sie zum Erstellen eines nicht genehmigten Benutzerkontos eine der-über Ladungen, die den `IsApproved`-Eigenschafts Wert des neuen Benutzers als Eingabeparameter akzeptieren.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Schritt 3: Genehmigen von Benutzern durch Überprüfen Ihrer e-Mail-Adresse

Viele Websites, die Benutzerkonten unterstützen, genehmigen neue Benutzer erst, wenn Sie die bei der Registrierung angegebene e-Mail-Adresse überprüfen. Dieser Überprüfungs Vorgang wird häufig zum vereiteln von Bots, Spammern und anderen or-do-Wells verwendet, da hierfür eine eindeutige, verifizierte e-Mail-Adresse erforderlich ist und ein zusätzlicher Schritt im Registrierungsprozess hinzugefügt wird. Bei diesem Modell wird eine e-Mail-Nachricht gesendet, die einen Link zu einer Überprüfungs Seite enthält, wenn ein neuer Benutzer sich anmeldet. Durch den Besuch des Links hat der Benutzer bewiesen, dass er die e-Mail erhalten hat und dass die angegebene e-Mail-Adresse gültig ist. Die Seite "Überprüfung" ist für die Genehmigung des Benutzers verantwortlich. Dies kann automatisch erfolgen, wenn Sie alle Benutzer genehmigen, die diese Seite erreichen, oder wenn der Benutzer einige zusätzliche Informationen, z. b. eine [CAPTCHA](http://en.wikipedia.org/wiki/Captcha), bereitstellt.

Um diesem Workflow Rechnung zu tragen, müssen wir zuerst die Seite für die Kontoerstellung aktualisieren, damit neue Benutzer nicht genehmigt werden. Öffnen Sie die Seite `EnhancedCreateUserWizard.aspx` im Ordner `Membership`, und legen Sie die `DisableCreatedUser`-Eigenschaft des Steuer Elements für das-Steuerelement auf `True`fest.

Als nächstes müssen wir das Steuerelement "kreateuserwizard" so konfigurieren, dass eine e-Mail an den neuen Benutzer gesendet wird. dabei wird erläutert, wie das Konto überprüft wird. Insbesondere fügen wir einen Link in die e-Mail auf die `Verification.aspx` Seite ein (die wir noch erstellen müssen), indem wir die `UserId` des neuen Benutzers über die QueryString übergeben. Auf der Seite "`Verification.aspx`" wird der angegebene Benutzer gesucht und als "genehmigt" markiert.

### <a name="sending-a-verification-email-to-new-users"></a>Senden einer Überprüfungs-e-Mail an neue Benutzer

Um eine e-Mail aus dem Steuerelement "kreateuserwizard" zu senden, konfigurieren Sie die `MailDefinition`-Eigenschaft entsprechend. Wie bereits im <a id="Tutorial13"> </a> [vorherigen Tutorial](recovering-and-changing-passwords-vb.md)erläutert, enthalten die Steuerelemente ChangePassword und PasswordRecovery eine [`MailDefinition`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) , die auf die gleiche Weise funktioniert wie die des-Steuer Elements des-Steuer Elements des-Steuer Elements.

> [!NOTE]
> Um die `MailDefinition`-Eigenschaft verwenden zu können, müssen Sie in `Web.config`e-Mail-Übermittlungs Optionen angeben. Weitere Informationen finden Sie unter [Senden von e-Mails in ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

Erstellen Sie zunächst eine neue e-Mail-Vorlage mit dem Namen `CreateUserWizard.txt` im Ordner `EmailTemplates`. Verwenden Sie für die Vorlage den folgenden Text:

[!code-aspx[Main](unlocking-and-approving-user-accounts-vb/samples/sample3.aspx)]

Legen Sie die `BodyFileName`-Eigenschaft der `MailDefinition`auf "~/EmailTemplates/CreateUserWizard.txt" und deren `Subject`-Eigenschaft auf "Willkommen bei meiner Website! Aktivieren Sie Ihr Konto. "

Beachten Sie, dass die `CreateUserWizard.txt`-e-Mail-Vorlage einen `<%VerificationUrl%>` Platzhalter enthält. An dieser Stelle wird die URL für die `Verification.aspx` Seite platziert. Der Benutzername und das Kennwort des neuen Kontos werden automatisch vom Assistenten für die `<%UserName%>` und `<%Password%>` ersetzt, es ist jedoch kein integrierter `<%VerificationUrl%>` Platzhalter vorhanden. Wir müssen ihn manuell durch die entsprechende Überprüfungs-URL ersetzen.

Erstellen Sie hierzu einen Ereignishandler für das [`SendingMail`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) von "feateuserwizard", und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample4.vb)]

Das `SendingMail`-Ereignis wird nach dem `CreatedUser`-Ereignis ausgelöst. Dies bedeutet, dass das neue Benutzerkonto zu dem Zeitpunkt, zu dem der obige Ereignishandler ausgeführt wird, bereits erstellt wurde. Sie können auf den `UserId` Wert des neuen Benutzers zugreifen, indem Sie die `Membership.GetUser`-Methode aufrufen und die `UserName` übergeben, die in das CreateUserWizard-Steuerelement eingegeben wurden. Als nächstes wird die Überprüfungs-URL gebildet. Die-Anweisung `Request.Url.GetLeftPart(UriPartial.Authority)` gibt den `http://yourserver.com` Teil der URL zurück. `Request.ApplicationPath` gibt den Pfad zurück, in dem die Anwendung einen Stamm hat. Die Überprüfungs-URL wird dann als `Verification.aspx?ID=userId`definiert. Diese beiden Zeichen folgen werden dann verkettet, um die gesamte URL zu bilden. Zum Schluss hat der e-Mail-Nachrichtentext (`e.Message.Body`) alle Vorkommen von `<%VerificationUrl%>` durch die vollständige URL ersetzt.

Der Nettoeffekt ist, dass neue Benutzer nicht genehmigt werden, was bedeutet, dass Sie sich nicht bei der Website anmelden können. Außerdem wird automatisch eine e-Mail mit einem Link zur Überprüfungs-URL gesendet (siehe Abbildung 6).

[![der neue Benutzer eine e-Mail mit einem Link zur Überprüfungs-URL erhält](unlocking-and-approving-user-accounts-vb/_static/image17.png)](unlocking-and-approving-user-accounts-vb/_static/image16.png)

**Abbildung 6**: der neue Benutzer erhält eine e-Mail mit einem Link zur Überprüfungs-URL ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image18.png))

> [!NOTE]
> Der standardmäßige Vorgang für den Vorgang von "kreateuserwizard" des dashateuserwizard-Steuer Elements zeigt eine Meldung an, in der der Benutzer darüber informiert wird, dass sein Konto erstellt wurde. Wenn Sie darauf klicken, wird der Benutzer zur URL, die von der `ContinueDestinationPageUrl`-Eigenschaft des Steuer Elements angegeben wird. Der "deeuserwizard" in `EnhancedCreateUserWizard.aspx` ist so konfiguriert, dass neue Benutzer an die `~/Membership/AdditionalUserInfo.aspx`gesendet werden, die den Benutzer zur Eingabe ihrer Heimat-, Homepage-URL und Signatur auffordert. Da diese Informationen nur von angemeldeten Benutzern hinzugefügt werden können, ist es sinnvoll, diese Eigenschaft zu aktualisieren, damit Benutzer an die Startseite der Website zurückgesendet werden (`~/Default.aspx`). Außerdem sollten die `EnhancedCreateUserWizard.aspx` Seite oder der Schritt "" von "kreateuserwizard" erweitert werden, um den Benutzer darüber zu informieren, dass ihm eine Verifizierungs-e-Mail gesendet wurde und sein Konto erst dann aktiviert wird, wenn die Anweisungen in dieser e-Mail befolgt werden. Ich lasse diese Änderungen als Übung für den Reader aus.

### <a name="creating-the-verification-page"></a>Erstellen der Überprüfungs Seite

Die letzte Aufgabe besteht darin, die `Verification.aspx` Seite zu erstellen. Fügen Sie diese Seite zum Stamm Ordner hinzu, und ordnen Sie Sie der `Site.master` Master Seite zu. Entfernen Sie das Inhalts Steuerelement, das auf die `LoginContent` contentplachalter verweist, damit die Inhaltsseite den Standard Inhalt der Master Seite verwendet, wie wir mit den meisten der zuvor hinzugefügten Inhaltsseiten durchgeführt haben.

Fügen Sie ein Label-websteuer Element zur Seite `Verification.aspx` hinzu, legen Sie dessen `ID` auf `StatusMessage`, und löschen Sie die Text-Eigenschaft. Erstellen Sie als nächstes den `Page_Load`-Ereignishandler, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample5.vb)]

Der größte Teil des obigen Codes überprüft, ob die über die QueryString angegebene UserID vorhanden ist, ob es sich um einen gültigen `Guid` Wert handelt und ob er auf ein vorhandenes Benutzerkonto verweist. Wenn alle diese Prüfungen bestanden werden, wird das Benutzerkonto genehmigt. Andernfalls wird eine entsprechende Statusmeldung angezeigt.

Abbildung 7 zeigt die `Verification.aspx` Seite, wenn Sie über einen Browser besucht werden.

[![das Konto des neuen Benutzers jetzt genehmigt ist.](unlocking-and-approving-user-accounts-vb/_static/image20.png)](unlocking-and-approving-user-accounts-vb/_static/image19.png)

**Abbildung 7**: das neue Benutzerkonto ist nun genehmigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](unlocking-and-approving-user-accounts-vb/_static/image21.png))

## <a name="summary"></a>Summary

Alle Mitgliedschafts Benutzerkonten haben zwei Statuswerte, die bestimmen, ob sich der Benutzer bei der Website anmelden kann: `IsLockedOut` und `IsApproved`. Beide Eigenschaften müssen für den Benutzer `True` werden, damit Sie sich anmelden können.

Der Status "gesperrt" des Benutzers wird als Sicherheitsmaßnahme verwendet, um die Wahrscheinlichkeit zu verringern, dass ein Hacker durch Brute-Force-Methoden in eine Site verwandelt wird. Insbesondere wird ein Benutzer gesperrt, wenn in einem bestimmten Zeitfenster eine bestimmte Anzahl von ungültigen Anmelde versuchen vorliegt. Diese Begrenzungen können mithilfe der Mitgliedschafts Anbieter Einstellungen in `Web.config`konfiguriert werden.

Der genehmigte Status wird häufig als Mittel verwendet, um zu verhindern, dass sich neue Benutzer anmelden, bis eine Aktion aufgetreten ist. Möglicherweise erfordert die Website, dass neue Konten zuerst vom Administrator oder, wie in Schritt 3 gezeigt, durch die Überprüfung Ihrer e-Mail-Adresse genehmigt werden.

Fröhliche Programmierung!

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](recovering-and-changing-passwords-vb.md)
