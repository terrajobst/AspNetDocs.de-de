---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Zuweisen von Rollen zu Benutzern (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erstellen wir zwei ASP.NET-Seiten, um die Verwaltung der Benutzer zu unterstützen, die zu welchen Rollen gehören. Die erste Seite enthält Funktionen, um zu sehen, was...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: b53df4494eb0faef7c5e4547c2bf95e5fb071298
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577858"
---
# <a name="assigning-roles-to-users-vb"></a>Zuweisen von Rollen an Benutzer (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> In diesem Tutorial erstellen wir zwei ASP.NET-Seiten, um die Verwaltung der Benutzer zu unterstützen, die zu welchen Rollen gehören. Die erste Seite enthält Funktionen, um zu sehen, welche Benutzer zu einer bestimmten Rolle gehören, welche Rollen ein bestimmter Benutzer angehört, und die Möglichkeit, einen bestimmten Benutzer einer bestimmten Rolle zuzuweisen oder zu entfernen. Auf der zweiten Seite erweitern wir das Steuerelement "kreateuserwizard", sodass es einen Schritt zum Angeben der Rollen enthält, zu denen der neu erstellte Benutzer gehört. Dies ist in Szenarios nützlich, in denen ein Administrator neue Benutzerkonten erstellen kann.

## <a name="introduction"></a>Einführung

Im <a id="_msoanchor_1"> </a> [vorherigen Tutorial](creating-and-managing-roles-vb.md) wurden das Rollen Framework und die `SqlRoleProvider`untersucht. Wir haben gesehen, wie Sie die `Roles`-Klasse zum Erstellen, abrufen und Löschen von Rollen verwenden. Zusätzlich zum Erstellen und Löschen von Rollen müssen wir in der Lage sein, Benutzer einer Rolle zuzuweisen oder zu entfernen. Leider wird ASP.net nicht mit websteuer Elementen ausgeliefert, um zu verwalten, welche Benutzer zu welchen Rollen gehören. Stattdessen müssen wir eigene ASP.NET-Seiten erstellen, um diese Zuordnungen zu verwalten. Die gute Nachricht ist, dass das Hinzufügen und Entfernen von Benutzern zu Rollen recht einfach ist. Die `Roles`-Klasse enthält eine Reihe von Methoden zum Hinzufügen von Benutzern zu mindestens einer Rolle.

In diesem Tutorial erstellen wir zwei ASP.NET-Seiten, um die Verwaltung der Benutzer zu unterstützen, die zu welchen Rollen gehören. Die erste Seite enthält Funktionen, um zu sehen, welche Benutzer zu einer bestimmten Rolle gehören, welche Rollen ein bestimmter Benutzer angehört, und die Möglichkeit, einen bestimmten Benutzer einer bestimmten Rolle zuzuweisen oder zu entfernen. Auf der zweiten Seite erweitern wir das Steuerelement "kreateuserwizard", sodass es einen Schritt zum Angeben der Rollen enthält, zu denen der neu erstellte Benutzer gehört. Dies ist in Szenarios nützlich, in denen ein Administrator neue Benutzerkonten erstellen kann.

Fangen wir an!

## <a name="listing-what-users-belong-to-what-roles"></a>Auflisten, welche Benutzer zu welchen Rollen gehören

Die erste Geschäftsordnung für dieses Tutorial besteht darin, eine Webseite zu erstellen, von der Benutzer Rollen zugewiesen werden können. Bevor wir uns mit der Zuweisung von Benutzern zu Rollen befassen, konzentrieren wir uns zunächst darauf, wie Sie bestimmen können, welche Benutzer zu welchen Rollen gehören. Diese Informationen können auf zwei Arten angezeigt werden: "nach Rolle" oder "nach Benutzer". Wir könnten dem Besucher gestatten, eine Rolle auszuwählen und dann alle Benutzer anzuzeigen, die der Rolle angehören (die "nach Rolle"-Anzeige). Alternativ können wir den Besucher auffordern, einen Benutzer auszuwählen, und ihm dann die Rollen anzeigen, die diesem Benutzer zugewiesen sind (die "nach Benutzer"-Anzeige).

Die Ansicht "nach Rolle" ist hilfreich in Situationen, in denen der Besucher die Gruppe von Benutzern, die zu einer bestimmten Rolle gehören, kennen möchte. die Ansicht "nach Benutzer" ist ideal, wenn der Besucher die Rolle (n) eines bestimmten Benutzers kennen muss. Wir möchten, dass unsere Seite sowohl die Schnittstellen "nach Rolle" als auch "nach Benutzer" enthält.

Wir beginnen mit der Erstellung der "by User"-Schnittstelle. Diese Schnittstelle besteht aus einer Dropdown Liste und einer Liste von Kontrollkästchen. Die Dropdown Liste wird mit der Gruppe der Benutzer im System aufgefüllt. mit den Kontrollkästchen werden die Rollen aufgelistet. Wenn Sie einen Benutzer aus der Dropdown Liste auswählen, werden die Rollen überprüft, zu denen der Benutzer gehört. Die Person, die die Seite besucht, kann dann die Kontrollkästchen aktivieren bzw. deaktivieren, um den ausgewählten Benutzer den entsprechenden Rollen hinzuzufügen oder zu entfernen.

> [!NOTE]
> Das Verwenden einer Dropdown Liste zum Auflisten der Benutzerkonten ist nicht die ideale Wahl für Websites, auf denen möglicherweise Hunderte von Benutzerkonten vorhanden sind. Eine Dropdown Liste ist so konzipiert, dass Benutzer ein Element aus einer relativ kurzen Liste von Optionen auswählen können. Die Anzahl der Listenelemente wächst schnell, wenn die Anzahl der Listenelemente wächst. Wenn Sie eine Website mit einer potenziell großen Anzahl von Benutzerkonten entwickeln, empfiehlt es sich, eine alternative Benutzeroberfläche zu verwenden, z. b. eine ausführbare GridView oder eine filterbare Schnittstelle, die den Besucher auffordert, einen Buchstaben auszuwählen. zeigt die Benutzer an, deren Benutzername mit dem ausgewählten Buchstaben beginnt.

## <a name="step-1-building-the-by-user-user-interface"></a>Schritt 1: entwickeln der Benutzeroberfläche "by User"

Öffnen Sie die Seite `UsersAndRoles.aspx`. Fügen Sie am oberen Rand der Seite ein Label-websteuer Element mit dem Namen `ActionStatus` hinzu, und löschen Sie dessen `Text`-Eigenschaft. Wir verwenden diese Bezeichnung, um Feedback zu den durchgeführten Aktionen bereitzustellen, wie z. b. "Benutzer-Tito wurde der Rolle" Administratoren "hinzugefügt wurde, oder" User jisun wurde aus der Rolle "Supervisor" entfernt. Damit diese Nachrichten ausfallen, legen Sie die `CssClass`-Eigenschaft der Bezeichnung auf "wichtig" fest.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Fügen Sie anschließend der `Styles.css` Stylesheet die folgende CSS-Klassendefinition hinzu:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Diese CSS-Definition weist den Browser an, die Bezeichnung mit einer großen, roten Schriftart anzuzeigen. Abbildung 1 zeigt diesen Effekt durch den Visual Studio-Designer.

[![die CssClass-Eigenschaft der Bezeichnung eine große, rote Schriftart ergibt](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Abbildung 1**: die `CssClass`-Eigenschaft der Bezeichnung ergibt eine große rote Schriftart ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image3.png))

Fügen Sie als nächstes der Seite eine DropDownList hinzu, legen Sie die `ID`-Eigenschaft auf `UserList`fest, und legen Sie deren `AutoPostBack`-Eigenschaft auf true fest. Wir verwenden diese Dropdown Liste, um alle Benutzer im System aufzulisten. Diese Dropdown List wird an eine Auflistung von mitgliedfassungs Objekten gebunden. Da in der Dropdown Liste die UserName-Eigenschaft des Membership shipuser-Objekts angezeigt werden soll (und als Wert der Listenelemente verwendet werden soll), legen Sie die Eigenschaften `DataTextField` und `DataValueField` der Dropdown Liste auf "username" fest.

Fügen Sie unterhalb der Dropdown List einen Repeater mit dem Namen `UsersRoleList`hinzu. Mit diesem Wiederholungs Modul werden alle Rollen im System als eine Reihe von Kontrollkästchen aufgelistet. Definieren Sie die `ItemTemplate` des Repeater mit dem folgenden deklarativen Markup:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

Das `ItemTemplate` Markup enthält ein einzelnes CheckBox-websteuer Element mit dem Namen `RoleCheckBox`. Die `AutoPostBack`-Eigenschaft des Kontrollkästchens ist auf true festgelegt, und die Eigenschaft `Text` ist an `Container.DataItem`gebunden. Der Grund hierfür ist, dass die Datenbindung-Syntax einfach `Container.DataItem` ist, weil das Rollen Framework die Liste der Rollennamen als Zeichen folgen Array zurückgibt. dieses Zeichen folgen Array wird an den Wiederholungs Dienst gebunden. Eine ausführliche Beschreibung, warum diese Syntax verwendet wird, um den Inhalt eines Arrays anzuzeigen, das an ein datenweb-Steuerelement gebunden ist, geht über den Rahmen dieses Tutorials hinaus. Weitere Informationen zu diesem Thema finden Sie unter [Binden eines skalaren Arrays an ein datenweb-Steuer](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)Element.

An diesem Punkt sollte das deklarative Markup Ihrer Benutzeroberfläche in etwa wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Wir sind nun bereit, den Code zu schreiben, um die Gruppe der Benutzerkonten an die Dropdown Liste und den Satz von Rollen an den Repeater zu binden. Fügen Sie in der Code Behind-Klasse der Seite eine Methode mit dem Namen `BindUsersToUserList` und eine weitere mit dem Namen `BindRolesList`hinzu, indem Sie den folgenden Code verwenden:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

Die `BindUsersToUserList`-Methode ruft alle Benutzerkonten im System über die [`Membership.GetAllUsers`-Methode](https://msdn.microsoft.com/library/dy8swhya.aspx)ab. Dadurch wird ein [`MembershipUserCollection` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx)zurückgegeben, bei dem es sich um eine Auflistung von [`MembershipUser` Instanzen](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)handelt. Diese Sammlung wird dann an den `UserList` Dropdown List gebunden. Die `MembershipUser`-Instanzen, die die Auflistung zusammenstellen, enthalten eine Reihe von Eigenschaften wie `UserName`, `Email`, `CreationDate`und `IsOnline`. Um die Dropdown Liste anzuweisen, den Wert der `UserName`-Eigenschaft anzuzeigen, stellen Sie sicher, dass die Eigenschaften `DataTextField` und `DataValueField` der `UserList` DropDownList auf "username" festgelegt wurden.

> [!NOTE]
> Die `Membership.GetAllUsers`-Methode verfügt über zwei über Ladungen: eine, die keine Eingabeparameter akzeptiert und alle Benutzer zurückgibt, und eine, die ganzzahlige Werte für den Seitenindex und die Seitengröße annimmt und nur die angegebene Teilmenge der Benutzer zurückgibt. Wenn große Mengen von Benutzerkonten in einem Auslagerungs fähigen Benutzeroberflächen Element angezeigt werden, kann die zweite Überladung verwendet werden, um die Benutzer effizienter zu durchlaufen, da Sie nur die genaue Teilmenge der Benutzerkonten und nicht alle zurückgibt.

Die `BindRolesToList`-Methode beginnt mit dem Aufrufen der [`GetAllRoles`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)der `Roles` Klasse, die ein Zeichen folgen Array mit den Rollen im System zurückgibt. Dieses Zeichen folgen Array wird dann an den Repeater gebunden.

Zum Schluss müssen diese beiden Methoden aufgerufen werden, wenn die Seite zum ersten Mal geladen wird. Fügen Sie dem `Page_Load` -Ereignishandler folgenden Code hinzu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Wenn dieser Code vorhanden ist, nehmen Sie sich einen Moment Zeit, um die Seite über einen Browser zu besuchen. Ihr Bildschirm sollte in etwa wie in Abbildung 2 aussehen. Alle Benutzerkonten werden in der Dropdown Liste ausgefüllt, und darunter wird jede Rolle als Kontrollkästchen angezeigt. Da die `AutoPostBack` Eigenschaften der Dropdown List und der Kontrollkästchen auf true festgelegt werden, wird durch das Ändern des ausgewählten Benutzers oder das Überprüfen bzw. Deaktivieren einer Rolle ein Postback verursacht. Es wird jedoch keine Aktion ausgeführt, da wir noch Code zum Verarbeiten dieser Aktionen schreiben müssen. Diese Aufgaben werden in den nächsten beiden Abschnitten behandelt.

[![auf der Seite werden die Benutzer und Rollen angezeigt.](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Abbildung 2**: die Seite zeigt die Benutzer und Rollen[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image6.png))

### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Die Rollen, denen der ausgewählte Benutzer angehört, werden überprüft.

Wenn die Seite zum ersten Mal geladen wird oder wenn der Besucher in der Dropdown Liste einen neuen Benutzer auswählt, müssen die Kontrollkästchen der `UsersRoleList`aktualisiert werden, damit das Kontrollkästchen für eine bestimmte Rolle nur dann aktiviert wird, wenn der ausgewählte Benutzer zu dieser Rolle gehört. Um dies zu erreichen, erstellen Sie eine Methode mit dem Namen `CheckRolesForSelectedUser` mit folgendem Code:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Der obige Code beginnt mit der Ermittlung des ausgewählten Benutzers. Anschließend wird die [`GetRolesForUser(userName)`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) der Rollen Klasse verwendet, um den Satz von Rollen des angegebenen Benutzers als Zeichen folgen Array zurückzugeben. Im nächsten Schritt werden die Elemente des Repeater aufgelistet, und es wird Programm gesteuert auf das `RoleCheckBox` Kontrollkästchen für jedes Element verwiesen. Das Kontrollkästchen ist nur aktiviert, wenn die Rolle, der Sie entspricht, im `selectedUsersRoles` Zeichen folgen Array enthalten ist.

> [!NOTE]
> Die `Linq.Enumerable.Contains(Of String)(...)` Syntax wird nicht kompiliert, wenn Sie ASP.NET Version 2,0 verwenden. Die `Contains(Of String)`-Methode ist Teil der [LINQ-Bibliothek](http://en.wikipedia.org/wiki/Language_Integrated_Query), die neu in ASP.NET 3,5 ist. Wenn Sie weiterhin ASP.NET Version 2,0 verwenden, verwenden Sie stattdessen die [`Array.IndexOf(Of String)`-Methode](https://msdn.microsoft.com/library/eha9t187.aspx) .

Die `CheckRolesForSelectedUser`-Methode muss in zwei Fällen aufgerufen werden: Wenn die Seite zum ersten Mal geladen wird und wenn der ausgewählte Index `UserList` DropDownList geändert wird. Rufen Sie daher diese Methode aus dem `Page_Load`-Ereignishandler auf (nach den Aufrufen von `BindUsersToUserList` und `BindRolesToList`). Erstellen Sie außerdem einen Ereignishandler für das `SelectedIndexChanged` Ereignis DropDownList, und rufen Sie diese Methode von dort auf.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Wenn dieser Code vorhanden ist, können Sie die Seite über den Browser testen. Da die Seite "`UsersAndRoles.aspx`" jedoch derzeit nicht die Möglichkeit zum Zuweisen von Benutzern zu Rollen hat, haben keine Benutzer Rollen. Wir erstellen die Schnittstelle zum Zuweisen von Benutzern zu Rollen in einem Moment. Sie können also entweder das Wort verwenden, dass dieser Code funktioniert, und überprüfen, ob dies später der Fall ist, oder Sie können Benutzer manuell zu Rollen hinzufügen, indem Sie Datensätze in die `aspnet_UsersInRoles` Tabelle einfügen, um diese Funktionalität jetzt zu testen.

### <a name="assigning-and-removing-users-from-roles"></a>Zuweisen und Entfernen von Benutzern aus Rollen

Wenn der Besucher ein Kontrollkästchen im `UsersRoleList` Repeater prüft oder deaktiviert, müssen wir den ausgewählten Benutzer der entsprechenden Rolle hinzufügen oder entfernen. Die `AutoPostBack`-Eigenschaft des Kontrollkästchens ist derzeit auf true festgelegt, wodurch ein Postback ausgelöst wird, sobald ein Kontrollkästchen im Wiederholungs Modul aktiviert oder deaktiviert ist. Kurz gesagt, wir müssen einen Ereignishandler für das `CheckChanged` Ereignis des Kontrollkästchens erstellen. Da sich das Kontrollkästchen in einem Wiederholungs Steuerelement befindet, müssen wir die Ereignishandlerfunktion manuell hinzufügen. Fügen Sie zunächst den Ereignishandler der Code-Behind-Klasse als `Protected` Methode wie folgt hinzu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Wir werden zurückkehren, um den Code für diesen Ereignishandler in einem Moment zu schreiben. Zuerst wird die Behandlung von Ereignis Behandlung durchgeführt. Fügen Sie im `ItemTemplate`des Repeater-`OnCheckedChanged="RoleCheckBox_CheckChanged"`hinzu. Diese Syntax verdrahtet den `RoleCheckBox_CheckChanged` Ereignishandler mit dem `CheckedChanged` Ereignis des `RoleCheckBox`.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Die letzte Aufgabe besteht darin, den `RoleCheckBox_CheckChanged`-Ereignishandler abzuschließen. Wir müssen zunächst auf das CheckBox-Steuerelement verweisen, das das Ereignis ausgelöst hat, da diese Checkbox-Instanz anzeigt, welche Rolle über die Eigenschaften "`Text`" und "`Checked`" überprüft wurde. Wenn diese Informationen zusammen mit dem Benutzernamen des ausgewählten Benutzers verwendet werden, wird der Benutzer über die [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) -oder [`RemoveUserFromRole`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)der `Roles` Klasse hinzugefügt oder aus der Rolle entfernt.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Der obige Code beginnt mit der programmgesteuerten Referenzierung des Kontrollkästchens, das das Ereignis ausgelöst hat, das über den `sender` Eingabeparameter verfügbar ist. Wenn das Kontrollkästchen aktiviert ist, wird der ausgewählte Benutzer der angegebenen Rolle hinzugefügt, andernfalls werden Sie aus der Rolle entfernt. In beiden Fällen wird in der `ActionStatus` Bezeichnung eine Meldung angezeigt, in der die gerade ausgeführte Aktion zusammengefasst wird.

Nehmen Sie sich einen Moment Zeit, um diese Seite über einen Browser zu testen. Wählen Sie Benutzer Tito aus, und fügen Sie dann in der Rolle Administratoren und Supervisor die Rolle "Tito"

[![Tito wurde den Rollen "Administratoren" und "Supervisor" hinzugefügt.](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Abbildung 3**: Tito wurde den Administratoren-und Aufsichts Rollen hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image9.png))

Wählen Sie als nächstes in der Dropdown Liste die Option User Bruce aus. Es gibt ein Postback, und die Kontrollkästchen des Wiederholungs Moduls werden über die `CheckRolesForSelectedUser`aktualisiert. Da Bruce noch nicht zu Rollen gehört, sind die beiden Kontrollkästchen deaktiviert. Fügen Sie als nächstes Bruce der Rolle "Supervisor" hinzu.

[![Bruce wurde der Rolle "Supervisor" hinzugefügt.](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Abbildung 4**: Bruce wurde der Rolle "Supervisor" hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image12.png))

Um die Funktionalität der `CheckRolesForSelectedUser`-Methode weiter zu überprüfen, wählen Sie einen anderen Benutzer als Tito oder Bruce aus. Beachten Sie, dass die Kontrollkästchen automatisch deaktiviert werden. Dies bedeutet, dass Sie nicht zu Rollen gehören. Kehren Sie zu Tito zurück. Die Kontrollkästchen Administratoren und Supervisor sollten überprüft werden.

## <a name="step-2-building-the-by-roles-user-interface"></a>Schritt 2: entwickeln der Benutzeroberfläche "by Rollen"

An dieser Stelle haben wir die Benutzeroberfläche "von Benutzern" abgeschlossen und sind bereit, mit der Bewältigung der "by Rollen"-Schnittstelle zu beginnen. Die Schnittstelle "by Rollen" fordert den Benutzer auf, eine Rolle aus einer Dropdown Liste auszuwählen, und zeigt dann die Gruppe von Benutzern an, die zu dieser Rolle in einer GridView gehören.

Fügen Sie der `UsersAndRoles.aspx page`ein weiteres DropDownList-Steuerelement hinzu. Platzieren Sie diese unter dem Repeater-Steuerelement, benennen Sie Sie `RoleList`, und legen Sie die `AutoPostBack`-Eigenschaft auf true fest. Fügen Sie darunter eine GridView hinzu, und benennen Sie Sie `RolesUserList`. In dieser GridView werden die Benutzer aufgelistet, die zur ausgewählten Rolle gehören. Legen Sie die `AutoGenerateColumns`-Eigenschaft der GridView auf false fest, fügen Sie der `Columns` Auflistung des Rasters ein TemplateField-Element hinzu, und legen Sie dessen Eigenschaft `HeaderText` auf "Users" fest. Definieren Sie die `ItemTemplate` von TemplateField, damit der Wert des Datenbindung-Ausdrucks `Container.DataItem` in der `Text`-Eigenschaft einer Bezeichnung mit dem Namen `UserNameLabel`angezeigt wird.

Nach dem Hinzufügen und Konfigurieren von GridView sollte das deklarative Markup der "by Role"-Schnittstelle in etwa wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Wir müssen den `RoleList` Dropdown List mit dem Satz von Rollen im System auffüllen. Um dies zu erreichen, aktualisieren Sie die `BindRolesToList`-Methode, sodass das von der `Roles.GetAllRoles`-Methode zurückgegebene Zeichen folgen Array an die `RolesList` DropDownList (sowie an den `UsersRoleList` Repeater) bindet.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Die letzten zwei Zeilen in der `BindRolesToList`-Methode wurden hinzugefügt, um den Rollensatz an das `RoleList` DropDownList-Steuerelement zu binden. In Abbildung 5 wird das Endergebnis angezeigt, wenn es über einen Browser angezeigt wird – eine Dropdown Liste, die mit den Rollen des Systems gefüllt ist.

[![werden die Rollen in der Dropdown Liste "rolelist" angezeigt.](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Abbildung 5**: die Rollen werden in der Dropdown Liste `RoleList` angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image15.png))

### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Anzeigen der Benutzer, die zur ausgewählten Rolle gehören

Wenn die Seite zum ersten Mal geladen wird oder wenn eine neue Rolle aus der Dropdown Liste `RoleList` ausgewählt ist, müssen wir die Liste der Benutzer, die zu dieser Rolle gehören, in der GridView anzeigen. Erstellen Sie eine Methode mit dem Namen `DisplayUsersBelongingToRole`, indem Sie folgenden Code verwenden:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Diese Methode beginnt mit der Auswahl der ausgewählten Rolle aus der Dropdown Liste `RoleList`. Anschließend wird die [`Roles.GetUsersInRole(roleName)`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) verwendet, um ein Zeichen folgen Array mit den Benutzernamen der Benutzer abzurufen, die zu dieser Rolle gehören. Dieses Array wird dann an den `RolesUserList` GridView gebunden.

Diese Methode muss in zwei Fällen aufgerufen werden: Wenn die Seite zum ersten Mal geladen wird und die ausgewählte Rolle in der `RoleList` Dropdown list geändert wird. Aktualisieren Sie daher den `Page_Load`-Ereignishandler, sodass diese Methode nach dem Aufruf von `CheckRolesForSelectedUser`aufgerufen wird. Erstellen Sie als nächstes einen Ereignishandler für das `SelectedIndexChanged`-Ereignis der `RoleList`, und rufen Sie auch diese Methode von dort auf.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Wenn dieser Code vorhanden ist, sollte die `RolesUserList` GridView die Benutzer anzeigen, die zur ausgewählten Rolle gehören. Wie in Abbildung 6 gezeigt, besteht die Rolle "Supervisor" aus zwei Membern: Bruce und Tito.

[![der GridView listet die Benutzer auf, die zur ausgewählten Rolle gehören.](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Abbildung 6**: die GridView listet die Benutzer auf, die zur ausgewählten Rolle gehören ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image18.png))

### <a name="removing-users-from-the-selected-role"></a>Entfernen von Benutzern aus der ausgewählten Rolle

Wir erweitern die `RolesUserList` GridView, sodass Sie eine Spalte mit den Schaltflächen "Remove" (entfernen) enthält. Wenn Sie für einen bestimmten Benutzer auf die Schaltfläche "entfernen" klicken, werden diese aus der Rolle entfernt.

Beginnen Sie, indem Sie der GridView ein Feld zum Löschen einer Schaltfläche hinzufügen. Legen Sie dieses Feld als am weitesten links geändert ein, und ändern Sie dessen `DeleteText`-Eigenschaft von "Delete" (Standard) in "Remove".

[![hinzufügen](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Abbildung 7**: Hinzufügen der Schaltfläche "entfernen" zur GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image21.png))

Wenn Sie auf die Schaltfläche "Remove" (entfernen) klicken, wird ein Postback ausgelöst, und das `RowDeleting`-Ereignis der GridView wird ausgelöst. Wir müssen einen Ereignishandler für dieses Ereignis erstellen und Code schreiben, mit dem der Benutzer aus der ausgewählten Rolle entfernt wird. Erstellen Sie den Ereignishandler, und fügen Sie dann den folgenden Code hinzu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Der Code beginnt mit der Ermittlung des ausgewählten Rollen namens. Er verweist dann Programm gesteuert auf das `UserNameLabel` Steuerelement aus der Zeile, auf deren Schaltfläche "entfernen" geklickt wurde, um den Benutzernamen des zu entfernenden Benutzers zu bestimmen. Der Benutzer wird dann über einen aufzurufenden `Roles.RemoveUserFromRole` der-Methode aus der Rolle entfernt. Die `RolesUserList` GridView wird dann aktualisiert, und es wird eine Meldung über das Steuerelement `ActionStatus` Bezeichnung angezeigt.

> [!NOTE]
> Die Schaltfläche "entfernen" erfordert keine Bestätigung vom Benutzer, bevor der Benutzer aus der Rolle entfernt wird. Ich lade Sie ein, ein gewisses Maß an Benutzer Bestätigung hinzuzufügen. Eine der einfachsten Möglichkeiten, eine Aktion zu bestätigen, ist das Dialogfeld Client seitiges bestätigen. Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen der Client seitigen Bestätigung beim Löschen](https://asp.net/learn/data-access/tutorial-42-vb.aspx).

Abbildung 8 zeigt die Seite, nach der der Benutzer Tito aus der Gruppe "Supervisoren" entfernt wurde.

[![leider ist Tito nicht mehr Supervisor.](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Abbildung 8**: Leider ist Tito kein Supervisor mehr ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image24.png))

### <a name="adding-new-users-to-the-selected-role"></a>Der ausgewählten Rolle werden neue Benutzer hinzugefügt.

Neben dem Entfernen von Benutzern aus der ausgewählten Rolle sollte der Besucher dieser Seite auch in der Lage sein, der ausgewählten Rolle einen Benutzer hinzuzufügen. Die beste Oberfläche zum Hinzufügen eines Benutzers zur ausgewählten Rolle hängt von der Anzahl der erwarteten Benutzerkonten ab. Wenn Ihre Website nur ein paar Dutzend Benutzerkonten oder weniger enthält, können Sie hier eine Dropdown Liste verwenden. Wenn möglicherweise Tausende von Benutzerkonten vorhanden sind, sollten Sie eine Benutzeroberfläche einschließen, die es dem Besucher ermöglicht, die Konten zu durchsuchen, nach einem bestimmten Konto zu suchen oder die Benutzerkonten auf andere Weise zu filtern.

Verwenden Sie für diese Seite eine sehr einfache Schnittstelle, die unabhängig von der Anzahl der Benutzerkonten im System funktioniert. Wir verwenden also ein Textfeld, in dem der Besucher aufgefordert wird, den Benutzernamen des Benutzers einzugeben, den Sie der ausgewählten Rolle hinzufügen möchten. Wenn kein Benutzer mit diesem Namen vorhanden ist oder der Benutzer bereits Mitglied der Rolle ist, wird in `ActionStatus` Bezeichnung eine Meldung angezeigt. Wenn der Benutzer jedoch vorhanden und kein Mitglied der Rolle ist, fügen wir Sie der Rolle hinzu und aktualisieren das Raster.

Fügen Sie unterhalb der GridView ein Textfeld und eine Schaltfläche hinzu. Legen Sie den `ID` des Textfelds auf `UserNameToAddToRole` fest, und legen Sie die Eigenschaften `ID` und `Text` der Schaltfläche auf `AddUserToRoleButton` und "Benutzer zu Rolle hinzufügen" fest.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Erstellen Sie als nächstes einen `Click` Ereignishandler für die `AddUserToRoleButton`, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

Der Großteil des Codes im `Click` Ereignishandler führt verschiedene Validierungs Überprüfungen durch. Dadurch wird sichergestellt, dass der Besucher im Textfeld `UserNameToAddToRole` einen Benutzernamen eingegeben hat, dass der Benutzer im System vorhanden ist, und dass er nicht bereits zur ausgewählten Rolle gehört. Wenn eine dieser Überprüfungen fehlschlägt, wird eine entsprechende Meldung in `ActionStatus` angezeigt, und der Ereignishandler wird beendet. Wenn alle Überprüfungen bestanden werden, wird der Benutzer der Rolle über die `Roles.AddUserToRole`-Methode hinzugefügt. Danach wird die `Text`-Eigenschaft des Textfelds gelöscht, die GridView wird aktualisiert, und die `ActionStatus` Bezeichnung zeigt eine Meldung an, die angibt, dass der angegebene Benutzer der ausgewählten Rolle erfolgreich hinzugefügt wurde.

> [!NOTE]
> Um sicherzustellen, dass der angegebene Benutzer nicht bereits zur ausgewählten Rolle gehört, verwenden wir die [`Roles.IsUserInRole(userName, roleName)`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), die einen booleschen Wert zurückgibt, der angibt, ob *username* ein Member von *roleName*ist. Diese Methode wird im <a id="_msoanchor_2"> </a> [nächsten Tutorial](role-based-authorization-vb.md) erneut verwendet, wenn wir die rollenbasierte Autorisierung betrachten.

Besuchen Sie die Seite über einen Browser, und wählen Sie die Rolle "Supervisoren" aus der Dropdown Liste `RoleList` aus. Versuchen Sie, einen ungültigen Benutzernamen einzugeben – es wird eine Meldung mit dem Hinweis angezeigt, dass der Benutzer im System nicht vorhanden ist.

[![Sie einer Rolle keinen nicht vorhandenen Benutzer hinzufügen können.](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Abbildung 9**: Es ist nicht möglich, einer Rolle einen nicht vorhandenen Benutzer hinzuzufügen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image27.png))

Versuchen Sie jetzt, einen gültigen Benutzer hinzuzufügen. Fügen Sie Tito der Rolle "Supervisor" erneut hinzu.

[![Tito ist wieder ein Supervisor!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Abbildung 10**: Tito ist wieder ein Supervisor!  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image30.png))

## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Schritt 3: übergreifende Aktualisierung der Schnittstellen "durch Benutzer" und "nach Rolle"

Die Seite "`UsersAndRoles.aspx`" bietet zwei unterschiedliche Schnittstellen zum Verwalten von Benutzern und Rollen. Derzeit agieren diese beiden Schnittstellen unabhängig voneinander, daher ist es möglich, dass eine in einer Schnittstelle vorgenommene Änderung nicht sofort in der anderen angezeigt wird. Stellen Sie sich beispielsweise vor, dass der Besucher der Seite die Rolle "Supervisoren" aus der `RoleList` Dropdown Liste auswählt, die Bruce und Tito als Member auflistet. Als nächstes wählt der Besucher in der Dropdown Liste `UserList` den Wert Tito aus, der die Kontrollkästchen Administratoren und Supervisor im `UsersRoleList` Repeater prüft. Wenn der Besucher dann die Supervisor-Rolle vom Wiederholungs Modul abhebt, wird Tito aus der Rolle "Supervisor" entfernt. diese Änderung wird jedoch nicht in der "by Role"-Schnittstelle widergespiegelt. In der GridView wird Tito weiterhin als Mitglied der Rolle "Supervisor" angezeigt.

Um dieses Problem zu beheben, müssen Sie die GridView aktualisieren, wenn eine Rolle vom `UsersRoleList` Repeater überprüft oder deaktiviert wird. Ebenso müssen wir den Repeater aktualisieren, wenn ein Benutzer aus der Schnittstelle "by Role" entfernt oder einer Rolle hinzugefügt wird.

Der Repeater in der Benutzeroberfläche wird durch Aufrufen der `CheckRolesForSelectedUser`-Methode aktualisiert. Die "by Role"-Schnittstelle kann im `RowDeleting` Ereignishandler der `RolesUserList` GridView und im `Click` Ereignishandler der `AddUserToRoleButton` Schaltfläche geändert werden. Daher müssen wir die `CheckRolesForSelectedUser`-Methode aus jeder dieser Methoden abrufen.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Ebenso wird die GridView in der "by Role"-Schnittstelle durch Aufrufen der `DisplayUsersBelongingToRole`-Methode aktualisiert, und die Benutzeroberfläche "by User" wird durch den `RoleCheckBox_CheckChanged`-Ereignishandler geändert. Daher muss die `DisplayUsersBelongingToRole`-Methode von diesem Ereignishandler aufgerufen werden.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Mit diesen geringfügigen Codeänderungen werden die Schnittstellen "durch Benutzer" und "nach Rolle" jetzt ordnungsgemäß aktualisiert. Um dies zu überprüfen, besuchen Sie die Seite über einen Browser, und wählen Sie in den Dropdown Listen für "`UserList`" und "`RoleList`" die Option "Tito" Beachten Sie, dass Sie beim Deaktivieren der Aufsichtsrolle für Tito aus dem Repeater in der "by User"-Schnittstelle automatisch aus der GridView in der "by Role"-Schnittstelle entfernt werden. Durch das Hinzufügen von Tito zur Rolle "Supervisor" aus der "by Role"-Schnittstelle wird das Kontrollkästchen "Supervisor" in der Benutzeroberfläche "nach Benutzer" automatisch erneut überprüft.

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Schritt 4: Anpassen von "buildeuserwizard", um den Schritt "Rollen angeben" einzuschließen

<a id="_msoanchor_3"> </a>Im Tutorial [*Erstellen von Benutzerkonten*](../membership/creating-user-accounts-vb.md) wurde erläutert, wie Sie das websteuer Element "websteuer Element" von "Web" erstellen, um eine Schnittstelle zum Erstellen eines neuen Benutzerkontos bereitzustellen. Das Steuerelement "kreateuserwizard" kann auf zwei Arten verwendet werden:

- Dies bedeutet, dass Besucher auf der Website ein eigenes Benutzerkonto erstellen können.
- Als Mittel für Administratoren, neue Konten zu erstellen

Im ersten Anwendungsfall kommt ein Besucher an die Website und füllt den "feateuserwizard" aus und gibt seine Informationen ein, um sich auf der Website zu registrieren. Im zweiten Fall erstellt ein Administrator ein neues Konto für eine andere Person.

Wenn ein Konto von einem Administrator für eine andere Person erstellt wird, kann es hilfreich sein, dem Administrator zu gestatten, die Rollen anzugeben, zu denen das neue Benutzerkonto gehört. <a id="_msoanchor_4"> </a>Im Tutorial [ *Speichern* *zusätzlicher Benutzerinformationen* ](../membership/storing-additional-user-information-vb.md) wurde erläutert, wie Sie den "anateuserwizard" anpassen, indem Sie zusätzliche `WizardSteps`hinzufügen. Sehen wir uns nun an, wie Sie dem "feateuserwizard" einen weiteren Schritt hinzufügen, um die Rollen des neuen Benutzers anzugeben.

Öffnen Sie die Seite "`CreateUserWizardWithRoles.aspx`", und fügen Sie ein Steuerelement mit dem Namen "`RegisterUserWithRoles`" hinzu. Legen Sie die `ContinueDestinationPageUrl`-Eigenschaft des Steuer Elements auf "~/default.aspx" fest. Da es hier darum geht, dass ein Administrator ein neues Benutzerkonto mit diesem-Steuerelement von "kreateuserwizard" erstellt, legen Sie die `LoginCreatedUser`-Eigenschaft des Steuer Elements auf "false" fest. Diese `LoginCreatedUser` Eigenschaft gibt an, ob der Besucher automatisch als soeben erstellter Benutzer angemeldet ist. der Standardwert ist "true". Wir legen den Wert auf "false" fest, da ein Administrator ein neues Konto erstellt, wenn er ein neues Konto erstellt.

Wählen Sie als nächstes die Option "`WizardSteps`hinzufügen/entfernen" aus. im Smarttagmenü von "feateuserwizard", und fügen Sie ein neues `WizardStep`hinzu, indem Sie dessen `ID` auf "`SpecifyRolesStep`" festlegen. Verschieben Sie die `SpecifyRolesStep WizardStep` so, dass Sie nach dem Schritt "registrieren Sie sich für Ihr neues Konto", aber vor dem Schritt "abschließen" angezeigt wird. Legen Sie die `Title`-Eigenschaft des `WizardStep`auf "Rollen angeben", die `StepType`-Eigenschaft auf `Step`und die `AllowReturn`-Eigenschaft auf "false" fest.

[![hinzufügen](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Abbildung 11**: Hinzufügen der `WizardStep` "Rollen angeben" zum "feateuserwizard" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image33.png))

Nachdem Sie diese Änderung vorgenommen haben, sollte das deklarative Markup von "kreateuserwizard" wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

Fügen Sie im `WizardStep`"Rollen angeben" eine CheckBoxList mit dem Namen `RoleList.` in dieser CheckBoxList werden die verfügbaren Rollen aufgelistet, sodass die Person, die die Seite besucht, überprüfen kann, zu welchen Rollen der neu erstellte Benutzer gehört.

Wir haben zwei Codierungs Aufgaben: zuerst muss die `RoleList` CheckBoxList mit den Rollen im System aufgefüllt werden. Zweitens müssen wir den erstellten Benutzer den ausgewählten Rollen hinzufügen, wenn der Benutzer den Schritt "Rollen angeben" in den Schritt "Fertigstellen" verschiebt. Wir können die erste Aufgabe im Ereignishandler für `Page_Load` ausführen. Der folgende Code verweist beim ersten Besuch der Seite Programm gesteuert auf das Kontrollkästchen `RoleList` und bindet die Rollen im System an ihn.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Der obige Code sollte Ihnen vertraut erscheinen. <a id="_msoanchor_5"> </a>Im Lernprogramm zum [ *Speichern* *zusätzlicher Benutzerinformationen* ](../membership/storing-additional-user-information-vb.md) wurden zwei `FindControl` Anweisungen verwendet, um von einem benutzerdefinierten `WizardStep`aus auf ein websteuer Element zu verweisen. Und der Code, der die Rollen an die CheckBoxList bindet, wurde von einem früheren Zeitpunkt in diesem Tutorial entnommen.

Um die zweite Programmieraufgabe auszuführen, müssen wir wissen, wann der Schritt "Rollen angeben" abgeschlossen wurde. Beachten Sie, dass der "forateuserwizard" über ein `ActiveStepChanged` Ereignis verfügt, das jedes Mal ausgelöst wird, wenn der Besucher von einem Schritt zu einem anderen navigiert. Hier können Sie feststellen, ob der Benutzer den Schritt "Fertig" erreicht hat. Wenn dies der Fall ist, muss der Benutzer den ausgewählten Rollen hinzugefügt werden.

Erstellen Sie einen Ereignishandler für das `ActiveStepChanged`-Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Wenn der Benutzer soeben den Schritt "abgeschlossen" erreicht hat, listet der Ereignishandler die Elemente der `RoleList` CheckBoxList auf, und der soeben erstellte Benutzer wird den ausgewählten Rollen zugewiesen.

Besuchen Sie diese Seite über einen Browser. Der erste Schritt in der Datei "Setup" ist der Standard Schritt "registrieren Sie sich für Ihr neues Konto", in dem der Benutzername, das Kennwort, die e-Mail-Adresse und andere Schlüsselinformationen für den neuen Benutzer angefordert werden. Geben Sie die Informationen ein, um einen neuen Benutzernamens Wanda zu erstellen.

[![erstellen Sie einen neuen Benutzer mit dem Namen "Wanda"](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Abbildung 12**: Erstellen eines neuen Benutzers mit dem Namen "Wanda" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image36.png))

Klicken Sie auf die Schaltfläche "Benutzer erstellen". Der Skript Erstellungs-Assistent ruft intern die `Membership.CreateUser` Methode auf, erstellt das neue Benutzerkonto und wechselt dann zum nächsten Schritt, "Rollen angeben". Hier werden die System Rollen aufgelistet. Aktivieren Sie das Kontrollkästchen Vorgesetzten, und klicken Sie auf weiter

[![Wanda Mitglied der Rolle "Supervisor"](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Abbildung 13**: Erstellen eines Mitglieds der Rolle "Supervisor" ([Klicken Sie hier, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image39.png))

Wenn Sie auf Weiter klicken, wird ein Postback ausgelöst, und der `ActiveStep` wird auf den Schritt "Complete" aktualisiert. Im Ereignishandler `ActiveStepChanged` wird das kürzlich erstellte Benutzerkonto der Rolle Supervisor zugewiesen. Um dies zu überprüfen, kehren Sie zur Seite `UsersAndRoles.aspx` zurück, und wählen Sie in der Dropdown Liste `RoleList` die Option Supervisor aus. Wie in Abbildung 14 gezeigt, bestehen die Vorgesetzten nun aus drei Benutzern: Bruce, Tito und Wanda.

[![Bruce, Tito und Wanda sind alle Vorgesetzten.](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Abbildung 14**: Bruce, Tito und Wanda sind alle Vorgesetzten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](assigning-roles-to-users-vb/_static/image42.png))

## <a name="summary"></a>Summary

Das Rollen Framework bietet Methoden zum Abrufen von Informationen über die Rollen und Methoden eines bestimmten Benutzers, um zu bestimmen, welche Benutzer zu einer bestimmten Rolle gehören. Außerdem gibt es eine Reihe von Methoden zum Hinzufügen und Entfernen von Benutzern zu einer oder mehreren Rollen. In diesem Tutorial haben wir uns auf zwei der folgenden Methoden konzentriert: `AddUserToRole` und `RemoveUserFromRole`. Es gibt zusätzliche Varianten, die für das Hinzufügen mehrerer Benutzer zu einer einzelnen Rolle und das Zuweisen mehrerer Rollen zu einem einzelnen Benutzer entworfen wurden.

Dieses Tutorial enthält auch einen Blick auf das Erweitern des Steuer Elements "kreateuserwizard", um ein `WizardStep` zum Angeben der Rollen des neu erstellten Benutzers einzuschließen. Ein solcher Schritt kann einem Administrator dabei helfen, das Erstellen von Benutzerkonten für neue Benutzer zu vereinfachen.

An dieser Stelle haben wir gesehen, wie Sie Rollen erstellen und löschen und wie Sie Benutzer zu Rollen hinzufügen und daraus entfernen. Wir haben uns jedoch noch mit dem Anwenden der rollenbasierten Autorisierung befasst. <a id="_msoanchor_6"> </a>Im [folgenden Tutorial](role-based-authorization-vb.md) wird erläutert, wie Sie URL-Autorisierungs Regeln für Rollen Weise definieren und wie Sie auf Seitenebene basierend auf den Rollen des aktuell angemeldeten Benutzers eingeschränkt werden.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net Übersicht über das Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/ms228053.aspx)
- [Untersuchen von ASP. Netzwerk Mitgliedschaft, Rollen und Profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Ein eigenes Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-and-managing-roles-vb.md)
> [Weiter](role-based-authorization-vb.md)
