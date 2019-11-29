---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Erstellen und Verwalten von RollenC#() | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden die erforderlichen Schritte zum Konfigurieren des Rollen-Frameworks erläutert. Danach erstellen wir Webseiten, mit denen Rollen erstellt und gelöscht werden.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a7883d0b05f2fa5a3fdac887f8c8b39d70418fb3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595773"
---
# <a name="creating-and-managing-roles-c"></a>Erstellen und Verwalten von Rollen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> In diesem Tutorial werden die erforderlichen Schritte zum Konfigurieren des Rollen-Frameworks erläutert. Danach erstellen wir Webseiten, mit denen Rollen erstellt und gelöscht werden.

## <a name="introduction"></a>Einführung

<a id="_msoanchor_1"> </a>Im Tutorial zur [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) haben wir uns mit der Verwendung der URL-Autorisierung beschäftigt, um bestimmte Benutzer von einer Gruppe von Seiten einzuschränken und deklarative und programmgesteuerte Techniken zum Anpassen der Funktionalität einer ASP.NET-Seite basierend auf dem Benutzer zu untersuchen. Das Erteilen von Berechtigungen für den Seiten Zugriff oder die Benutzer-zu-Benutzer-Basis kann jedoch zu einem Wartungs Albtraum in Szenarios werden, in denen viele Benutzerkonten vorhanden sind oder wenn sich die Benutzerberechtigungen häufig ändern. Jedes Mal, wenn ein Benutzer die Autorisierung zum Ausführen einer bestimmten Aufgabe erhält oder verliert, muss der Administrator die entsprechenden URL-Autorisierungs Regeln, das deklarative Markup und den Code aktualisieren.

In der Regel ist es hilfreich, Benutzer in Gruppen oder *Rollen* zu klassifizieren und dann Berechtigungen für Rollen Weise anzuwenden. Beispielsweise verfügen die meisten Webanwendungen über eine bestimmte Gruppe von Seiten oder Tasks, die nur für Administratoren reserviert sind. Mithilfe der im Tutorial zur *benutzerbasierten Autorisierung* gewonnenen Techniken fügen wir die entsprechenden URL-Autorisierungs Regeln, das deklarative Markup und den Code hinzu, um den angegebenen Benutzerkonten das Ausführen administrativer Aufgaben zu ermöglichen. Wenn jedoch ein neuer Administrator hinzugefügt wurde oder wenn ein vorhandener Administrator seine Administratorrechte widerrufen musste, müssten die Konfigurationsdateien und Webseiten zurückgegeben und aktualisiert werden. Mit Rollen können wir jedoch eine Rolle namens "Administratoren" erstellen und diese vertrauenswürdigen Benutzer der Administrator Rolle zuweisen. Als Nächstes fügen wir die entsprechenden URL-Autorisierungs Regeln, das deklarative Markup und den Code hinzu, damit die Administrator Rolle die verschiedenen administrativen Aufgaben ausführen kann. Wenn diese Infrastruktur vorhanden ist, ist das Hinzufügen neuer Administratoren zur Website oder das Entfernen vorhandener Administratoren so einfach wie das einschließen oder Entfernen des Benutzers aus der Administrator Rolle. Es sind keine Konfigurations-, deklarative Markup-oder Codeänderungen erforderlich.

ASP.net bietet ein Rollen Framework zum Definieren von Rollen und zum Zuordnen der Rollen zu Benutzerkonten. Mit dem Rollen Framework können wir Rollen erstellen und löschen, Benutzer zu einer Rolle hinzufügen oder aus einer Rolle entfernen, den Satz von Benutzern ermitteln, die zu einer bestimmten Rolle gehören, und feststellen, ob ein Benutzer zu einer bestimmten Rolle gehört. Nachdem das Rollen Framework konfiguriert wurde, können wir den Zugriff auf Seiten auf Rollen Basis über URL-Autorisierungs Regeln einschränken und zusätzliche Informationen oder Funktionen auf einer Seite auf der Grundlage der aktuell angemeldeten Benutzer Rollen anzeigen oder ausblenden.

In diesem Tutorial werden die erforderlichen Schritte zum Konfigurieren des Rollen-Frameworks erläutert. Danach erstellen wir Webseiten, mit denen Rollen erstellt und gelöscht werden. <a id="_msoanchor_2"> </a>Im Tutorial [*Zuweisen von Rollen zu Benutzern*](assigning-roles-to-users-cs.md) wird erläutert, wie Sie Benutzer zu Rollen hinzufügen und daraus entfernen. Im <a id="_msoanchor_3"> </a>Tutorial zur [*rollenbasierten Autorisierung*](role-based-authorization-cs.md) erfahren Sie, wie Sie den Zugriff auf Seiten auf Rollen Basis beschränken und wie Sie die Seiten Funktionalität abhängig von der Rolle des Besuchs Benutzers anpassen. Fangen wir an!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen neuer ASP.NET Seiten

In diesem Tutorial und den nächsten beiden werden verschiedene Rollen bezogene Funktionen und Funktionen untersucht. Wir benötigen eine Reihe von ASP.NET-Seiten, um die in diesen Tutorials untersuchten Themen zu implementieren. Erstellen Sie diese Seiten, und aktualisieren Sie die Site Übersicht.

Erstellen Sie zunächst einen neuen Ordner in dem Projekt mit dem Namen `Roles`. Fügen Sie als nächstes vier neue ASP.NET-Seiten zum Ordner "`Roles`" hinzu, wobei jede Seite mit der `Site.master` Master Seite verknüpft wird. Benennen Sie die Seiten:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

An diesem Punkt sollte die Projektmappen-Explorer des Projekts ähnlich wie der Screenshot in Abbildung 1 aussehen.

[dem Ordner "Rollen" wurden ![vier neue Seiten hinzugefügt.](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Abbildung 1**: vier neue Seiten wurden dem `Roles` Ordner hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image3.png))

Jede Seite sollte an diesem Punkt über die beiden Inhalts Steuerelemente verfügen, eine für jede der Inhalts Platzhalter der Master Seite: `MainContent` und `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Beachten Sie, dass das Standard Markup des `LoginContent` contentplachalter einen Link zum Anmelden oder Abmelden der Website anzeigt, je nachdem, ob der Benutzer authentifiziert ist. Wenn das `Content2` Content-Steuerelement auf der Seite ASP.net vorhanden ist, wird jedoch das Standard Markup der Master Seite überschrieben. Wie in <a id="_msoanchor_4"> </a> [*einer Übersicht über*](../introduction/an-overview-of-forms-authentication-cs.md) das Tutorial zur Formular Authentifizierung erläutert, ist das Überschreiben des Standard Markups in Seiten nützlich, bei denen wir keine Anmelde bezogenen Optionen in der linken Spalte anzeigen möchten.

Für diese vier Seiten möchten wir jedoch das Standard Markup der Master Seite für den `LoginContent` contentplachalter anzeigen. Entfernen Sie deshalb das deklarative Markup für das `Content2` Content-Steuerelement. Anschließend sollte jedes Markup der vier Seite nur ein Inhalts Steuerelement enthalten.

Abschließend aktualisieren wir die Site Map (`Web.sitemap`) so, dass Sie diese neuen Webseiten enthält. Fügen Sie den folgenden XML-Code nach `<siteMapNode>` dem hinzu, den Sie für die Mitgliedschafts Lernprogramme

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Wenn die Site Übersicht aktualisiert wurde, besuchen Sie die Website über einen Browser. Wie in Abbildung 2 gezeigt, enthält die Navigation auf der linken Seite die Elemente für die Lernprogramme "Rollen".

[dem Ordner "Rollen" wurden ![vier neue Seiten hinzugefügt.](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Abbildung 2**: vier neue Seiten wurden zum `Roles` Ordner hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image6.png))

## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Schritt 2: angeben und Konfigurieren des Rollen Framework-Anbieters

Wie das Mitgliedschafts Framework basiert das Rollen Framework auf dem Anbieter Modell. Wie im Lernprogramm <a id="_msoanchor_5"> </a>zu den [*Sicherheitsgrundlagen und ASP.NET-Unterstützung*](../introduction/security-basics-and-asp-net-support-cs.md) erläutert, werden die .NET Framework mit drei integrierten Rollen Anbietern ausgeliefert: [`AuthorizationStoreRoleProvider`](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx), [`WindowsTokenRoleProvider`](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)und [`SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Diese tutorialreihe konzentriert sich auf die `SqlRoleProvider`, die eine Microsoft SQL Server-Datenbank als Rollen Speicher verwendet.

Unterhalb von werden das Rollen Framework und `SqlRoleProvider` wie das Mitgliedschafts Framework und `SqlMembershipProvider`funktionieren. Der .NET Framework enthält eine `Roles` Klasse, die als API für das Rollen Framework fungiert. Die `Roles`-Klasse verfügt über statische Methoden wie `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`und so weiter. Wenn eine dieser Methoden aufgerufen wird, delegiert die `Roles`-Klasse den Aufruf an den konfigurierten Anbieter. Der `SqlRoleProvider` funktioniert mit den Rollen spezifischen Tabellen (`aspnet_Roles` und `aspnet_UsersInRoles`) als Antwort.

Um den `SqlRoleProvider` Anbieter in unserer Anwendung verwenden zu können, müssen wir angeben, welche Datenbank als Speicher verwendet werden soll. Der `SqlRoleProvider` erwartet, dass der angegebene Rollen Speicher über bestimmte Datenbanktabellen, Sichten und gespeicherte Prozeduren verfügt. Diese erforderlichen Datenbankobjekte können mithilfe des [`aspnet_regsql.exe` Tools](https://msdn.microsoft.com/library/ms229862.aspx)hinzugefügt werden. An diesem Punkt verfügen wir bereits über eine Datenbank mit dem Schema, das für die `SqlRoleProvider`erforderlich ist. Zurück zum <a id="_msoanchor_6"> </a> [*Erstellen des Mitgliedschafts Schemas in SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) Tutorial haben wir eine Datenbank mit dem Namen `SecurityTutorials.mdf` erstellt und `aspnet_regsql.exe` zum Hinzufügen der Anwendungsdienste verwendet, die die für die `SqlRoleProvider`erforderlichen Datenbankobjekte enthalten. Daher müssen wir nur das Rollen Framework anweisen, die Rollen Unterstützung zu aktivieren und die `SqlRoleProvider` mit der `SecurityTutorials.mdf` Datenbank als Rollen Speicher zu verwenden.

Das Rollen Framework wird über das &lt;`roleManager`&gt;-Element in der `Web.config` Datei der Anwendung konfiguriert. Standardmäßig ist die Rollen Unterstützung deaktiviert. Um es zu aktivieren, müssen Sie das `enabled`-Attribut des [&lt;`roleManager`&gt;](https://msdn.microsoft.com/library/ms164660.aspx) Elements auf `true` wie folgt festlegen:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Standardmäßig verfügen alle Webanwendungen über einen Rollen Anbieter mit dem Namen `AspNetSqlRoleProvider` vom Typ `SqlRoleProvider`. Dieser Standardanbieter wird in `machine.config` (`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`) registriert:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Das `connectionStringName`-Attribut des Anbieters gibt den verwendeten Rollen Speicher an. Der `AspNetSqlRoleProvider`-Anbieter legt dieses Attribut auf `LocalSqlServer`fest, das auch in `machine.config` und Punkten standardmäßig auf eine SQL Server 2005 Express Edition Datenbank im `App_Data`-Ordner mit dem Namen `aspnet.mdf`festgelegt ist.

Wenn wir das Rollen Framework einfach aktivieren, ohne Anbieter Informationen in der `Web.config` Datei der Anwendung anzugeben, verwendet die Anwendung daher den Standardanbieter für registrierte Rollen `AspNetSqlRoleProvider`. Wenn die `~/App_Data/aspnet.mdf` Datenbank nicht vorhanden ist, wird Sie von der ASP.NET-Laufzeit automatisch erstellt und das Anwendungs Dienst Schema hinzugefügt. Wir möchten jedoch nicht die `aspnet.mdf` Datenbank verwenden. Stattdessen möchten wir die `SecurityTutorials.mdf` Datenbank verwenden, die wir bereits erstellt haben, und das Anwendungs Dienst Schema hinzugefügt haben. Diese Änderung kann auf zwei Arten durchgeführt werden:

- Geben Sie in`Web.config`<strong>einen Wert für den Namen der</strong> <strong>`LocalSqlServer`</strong> <strong>Verbindungs Zeichenfolge an</strong> <strong>.</strong> Wenn Sie den Wert `LocalSqlServer` Verbindungs Zeichenfolgen-Name in `Web.config`überschreiben, können wir den Standardanbieter für registrierte Rollen (`AspNetSqlRoleProvider`) verwenden und ihn ordnungsgemäß mit der `SecurityTutorials.mdf` Datenbank arbeiten lassen. Weitere Informationen zu dieser Technik finden Sie im Blogbeitrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/), [Konfigurieren von ASP.NET 2,0 Anwendungsdienste für die Verwendung von SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Fügen Sie einen neuen registrierten Anbieter vom Typ</strong> <strong>`SqlRoleProvider`</strong>hinzu, <strong>und konfigurieren Sie seine</strong> <strong>`connectionStringName`</strong> <strong>Einstellungen so, dass er auf die</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>Datenbank verweist.</strong> Dies ist der empfohlene Ansatz, der im <a id="_msoanchor_7"> </a>Tutorial [*Erstellen des Mitgliedschafts Schemas in SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) beschrieben und verwendet wird. Dies ist der Ansatz, den ich in diesem Tutorial verwenden werde.

Fügen Sie der `Web.config` Datei das folgende Rollen-Konfigurations Markup hinzu. Dieses Markup registriert einen neuen Anbieter mit dem Namen `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Das obige Markup definiert die `SecurityTutorialsSqlRoleProvider` als Standardanbieter (über das `defaultProvider`-Attribut im `<roleManager>`-Element). Außerdem wird die `applicationName` Einstellung des `SecurityTutorialsSqlRoleProvider`auf `SecurityTutorials`festgelegt, wobei es sich um dieselbe `applicationName` Einstellung handelt, die vom Mitgliedschafts Anbieter (`SecurityTutorialsSqlMembershipProvider`) verwendet wird. Obwohl es hier nicht gezeigt wird, kann das [`<add>`-Element](https://msdn.microsoft.com/library/ms164662.aspx) für die `SqlRoleProvider` auch ein `commandTimeout`-Attribut enthalten, um die Timeout Dauer der Datenbank in Sekunden anzugeben. Der Standardwert ist 30.

Wenn dieses Konfigurations Markup vorhanden ist, können wir mit der Verwendung von Rollen Funktionen in der Anwendung beginnen.

> [!NOTE]
> Das obige Konfigurations Markup veranschaulicht die Verwendung der `enabled` und `defaultProvider` Attribute &lt;des &gt; Elements `roleManager`Elements. Es gibt eine Reihe weiterer Attribute, die sich darauf auswirken, wie das Rollen Framework Rollen Informationen auf Benutzerbasis zuordnet. Diese Einstellungen werden im Tutorial zur <a id="_msoanchor_8"> </a> [*rollenbasierten Autorisierung*](role-based-authorization-cs.md) untersucht.

## <a name="step-3-examining-the-roles-api"></a>Schritt 3: Untersuchen der Rollen-API

Die Funktionalität des Rollen-Frameworks wird über die [`Roles`-Klasse](https://msdn.microsoft.com/library/system.web.security.roles.aspx)verfügbar gemacht, die 13 statische Methoden zum Ausführen von rollenbasierten Vorgängen enthält. Wenn wir uns das Erstellen und Löschen von Rollen in den Schritten 4 und 6 ansehen, werden die Methoden [`CreateRole`](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) und [`DeleteRole`](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) verwendet, mit denen eine Rolle zum System hinzugefügt oder daraus entfernt wird.

Um eine Liste aller Rollen im System zu erhalten, verwenden Sie die`GetAllRoles`- [Methode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (siehe Schritt 5). Die [`RoleExists`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) gibt einen booleschen Wert zurück, der angibt, ob eine angegebene Rolle vorhanden ist.

Im nächsten Tutorial untersuchen wir, wie Benutzer Rollen zugeordnet werden. Die Methoden [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [`AddUserToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [`AddUsersToRole`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)und [`AddUsersToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) der `Roles` Klasse fügen mindestens einer Rolle einen Benutzer hinzu. Wenn Sie Benutzer aus Rollen entfernen möchten, verwenden Sie die Methoden [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [`RemoveUserFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [`RemoveUsersFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)oder [`RemoveUsersFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) .

<a id="_msoanchor_9"> </a>Im Tutorial zur [*rollenbasierten Autorisierung*](role-based-authorization-cs.md) betrachten wir die Möglichkeiten, die Funktionalität basierend auf der aktuell angemeldeten Benutzerrolle Programm gesteuert anzuzeigen oder auszublenden. Um dies zu erreichen, können wir die Methoden [`FindUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [`GetRolesForUser`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [`GetUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)oder [`IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) der `Role` Klasse verwenden.

> [!NOTE]
> Beachten Sie, dass die `Roles`-Klasse den Aufruf an den konfigurierten Anbieter delegiert, wenn eine dieser Methoden aufgerufen wird. In unserem Fall bedeutet dies, dass der-Rückruf an den `SqlRoleProvider`gesendet wird. Der `SqlRoleProvider` führt dann den entsprechenden Daten Bank Vorgang basierend auf der aufgerufenen Methode aus. Beispielsweise führt der Code `Roles.CreateRole("Administrators")` dazu, dass die `SqlRoleProvider` die gespeicherte Prozedur `aspnet_Roles_CreateRole` ausführt, mit der ein neuer Datensatz in die `aspnet_Roles` Tabelle mit dem Namen "Administratoren" eingefügt wird.

Im restlichen Teil dieses Tutorials wird die Verwendung der Methoden `CreateRole`, `GetAllRoles`und `DeleteRole` der `Roles` Klasse zum Verwalten der Rollen im System untersucht.

## <a name="step-4-creating-new-roles"></a>Schritt 4: Erstellen neuer Rollen

Rollen bieten eine Möglichkeit zum willkürlichen Gruppieren von Benutzern, und in der Regel wird diese Gruppierung verwendet, um eine komfortablere Methode zum Anwenden von Autorisierungs Regeln zu verwenden. Um jedoch Rollen als Autorisierungs Mechanismus verwenden zu können, müssen Sie zunächst definieren, welche Rollen in der Anwendung vorhanden sind. Leider enthält ASP.net kein "kreaterolewizard"-Steuerelement. Um neue Rollen hinzuzufügen, müssen wir eine passende Benutzeroberfläche erstellen und die Rollen-API selbst aufrufen. Die gute Nachricht ist, dass dies sehr einfach zu erreichen ist.

> [!NOTE]
> Es gibt zwar kein websteuer Element " [ASP.net Web Site Administration](https://msdn.microsoft.com/library/ms228053.aspx)", aber es gibt eine lokale ASP.NET-Anwendung, die die Anzeige und Verwaltung der Konfiguration Ihrer Webanwendung unterstützt. Ich bin jedoch aus zwei Gründen kein großer Fan des ASP.NET-Websiteverwaltungs-Tools. Erstens ist es ein wenig fehlerhaft, und die Benutzer Darstellung lässt sich sehr viel wünschen. Zweitens ist das ASP.NET-Websiteverwaltungs-Tool nur für die lokale Ausführung konzipiert. Dies bedeutet, dass Sie eigene Rollen Verwaltungs Webseiten erstellen müssen, wenn Sie Rollen auf einer Live Website remote verwalten müssen. Aus diesen beiden Gründen konzentrieren sich dieses Lernprogramm und das nächste auf das Entwickeln der erforderlichen Rollen Verwaltungs Tools auf einer Webseite, anstatt sich auf das ASP.net Web Site Administration-Tool zu verlassen.

Öffnen Sie die Seite `ManageRoles.aspx` im Ordner `Roles`, und fügen Sie der Seite ein Textfeld und ein Schaltflächen-websteuer Element hinzu. Legen Sie die `ID`-Eigenschaft des TextBox-Steuer Elements auf `RoleName` und die Eigenschaften `ID` und `Text` der Schaltfläche auf `CreateRoleButton` bzw. Create Role fest. An diesem Punkt sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Doppelklicken Sie im Designer auf das Schaltflächen-Steuerelement `CreateRoleButton`, um einen `Click`-Ereignishandler zu erstellen, und fügen Sie dann den folgenden Code hinzu:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Der obige Code beginnt mit dem Zuweisen des im Textfeld `RoleName` eingegebenen gekürzten Rollen namens zur `newRoleName`-Variablen. Als nächstes wird die `RoleExists`-Methode der `Roles` Klasse aufgerufen, um zu bestimmen, ob die Rollen `newRoleName` bereits im System vorhanden ist. Wenn die Rolle nicht vorhanden ist, wird Sie über einen aufzurufenden `CreateRole` Methode erstellt. Wenn der `CreateRole`-Methode ein Rollenname übermittelt wird, der bereits im System vorhanden ist, wird eine `ProviderException` Ausnahme ausgelöst. Aus diesem Grund überprüft der Code zuerst, ob die Rolle noch nicht im System vorhanden ist, bevor `CreateRole`aufgerufen wird. Der `Click` Ereignishandler schließt das Löschen der `Text`-Eigenschaft des `RoleName` Textfelds ab.

> [!NOTE]
> Sie Fragen sich vielleicht, was passieren wird, wenn der Benutzer keinen Wert in das Textfeld "`RoleName`" eingibt. Wenn der an die `CreateRole` Methode eingegebene Wert `null` oder eine leere Zeichenfolge ist, wird eine Ausnahme ausgelöst. Ebenso wird eine Ausnahme ausgelöst, wenn der Rollenname ein Komma enthält. Folglich sollte die Seite Validierungs Steuerelemente enthalten, um sicherzustellen, dass der Benutzer eine Rolle eingibt und keine Kommas enthält. Ich lasse als Übung für den Reader aus.

Wir erstellen nun eine Rolle mit dem Namen "Administratoren". Besuchen Sie die Seite `ManageRoles.aspx` über einen Browser, geben Sie Administratoren in das Textfeld ein (siehe Abbildung 3), und klicken Sie dann auf die Schaltfläche Rolle erstellen.

[![Erstellen einer Administrator Rolle](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Abbildung 3**: Erstellen einer Administrator Rolle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image9.png))

Was ist los? Ein Postback tritt auf, aber es gibt keinen visuellen Hinweis darauf, dass die Rolle tatsächlich dem System hinzugefügt wurde. Wir werden diese Seite in Schritt 5 aktualisieren, um visuelles Feedback einzuschließen. Derzeit können Sie jedoch überprüfen, ob die Rolle erstellt wurde, indem Sie die `SecurityTutorials.mdf` Datenbank aufrufen und die Daten aus der `aspnet_Roles` Tabelle anzeigen. Wie in Abbildung 4 gezeigt, enthält die `aspnet_Roles` Tabelle einen Datensatz für die soeben hinzugefügten Administratoren Rollen.

[![die aspnet_Roles Tabelle eine Zeile für die Administratoren enthält.](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Abbildung 4**: die `aspnet_Roles` Tabelle enthält eine Zeile für die Administratoren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image12.png))

## <a name="step-5-displaying-the-roles-in-the-system"></a>Schritt 5: Anzeigen der Rollen im System

Erweitern Sie nun die Seite "`ManageRoles.aspx`", um eine Liste der aktuellen Rollen im System einzubeziehen. Fügen Sie hierzu der Seite ein GridView-Steuerelement hinzu, und legen Sie dessen `ID`-Eigenschaft auf `RoleList`fest. Fügen Sie als nächstes der Code Behind-Klasse der Seite mit dem Namen `DisplayRolesInGrid` eine Methode mit dem folgenden Code hinzu:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

Die `GetAllRoles`-Methode der `Roles` Klasse gibt alle Rollen im System als Array von Zeichen folgen zurück. Dieses Zeichen folgen Array wird dann an die GridView gebunden. Um die Liste der Rollen an die GridView zu binden, wenn die Seite zum ersten Mal geladen wird, müssen wir die `DisplayRolesInGrid`-Methode aus dem `Page_Load`-Ereignishandler der Seite aufzurufen. Mit dem folgenden Code wird diese Methode aufgerufen, wenn die Seite zum ersten Mal aufgerufen wird, jedoch nicht bei nachfolgenden Postbacks.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Wenn dieser Code vorhanden ist, besuchen Sie die Seite über einen Browser. Wie in Abbildung 5 gezeigt, sollte ein Raster mit einer einzelnen Spalte mit der Bezeichnung Item angezeigt werden. Das Raster enthält eine Zeile für die in Schritt 4 hinzugefügte Administrator Rolle.

[![in der GridView die Rollen in einer einzelnen Spalte angezeigt werden.](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Abbildung 5**: die GridView zeigt die Rollen in einer einzelnen Spalte an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image15.png))

In der GridView wird eine einzelne Spalte mit der Bezeichnung Item angezeigt, da die `AutoGenerateColumns`-Eigenschaft der GridView auf true festgelegt ist (Standardeinstellung). dadurch erstellt die GridView automatisch eine Spalte für jede Eigenschaft in der `DataSource`. Ein Array verfügt über eine einzelne Eigenschaft, die die Elemente im Array darstellt, daher die einzelne Spalte in der GridView.

Beim Anzeigen von Daten mit einer GridView-Sicht empfiehlt es sich, meine Spalten explizit zu definieren, anstatt Sie implizit von der GridView generieren zu lassen. Durch das explizite Definieren der Spalten ist es viel einfacher, die Daten zu formatieren, die Spalten neu anzuordnen und andere häufige Aufgaben auszuführen. Daher aktualisieren wir das deklarative Markup der GridView, damit seine Spalten explizit definiert werden.

Legen Sie zunächst die `AutoGenerateColumns`-Eigenschaft der GridView auf false fest. Fügen Sie als nächstes ein TemplateField zum Raster hinzu, legen Sie dessen `HeaderText`-Eigenschaft auf Rollen fest, und konfigurieren Sie die `ItemTemplate`, sodass der Inhalt des Arrays angezeigt wird. Fügen Sie zu diesem Zweck dem `ItemTemplate` ein Label-websteuer Element mit dem Namen `RoleNameLabel` hinzu, und binden Sie dessen `Text`-Eigenschaft an `Container.DataItem`.

Diese Eigenschaften und die Inhalte der `ItemTemplate`können deklarativ oder über das Dialogfeld Felder von GridView und Vorlagen bearbeiten festgelegt werden. Um das Dialogfeld Felder zu erreichen, klicken Sie auf den Link Spalten bearbeiten im Smarttags von GridView. Deaktivieren Sie als nächstes das Kontrollkästchen Felder automatisch generieren, um die `AutoGenerateColumns`-Eigenschaft auf false festzulegen, und fügen Sie der GridView ein TemplateField-Objekt hinzu, und legen Sie dessen `HeaderText`-Eigenschaft auf Role fest. Um den Inhalt der `ItemTemplate`zu definieren, wählen Sie die Option Vorlagen bearbeiten im Smarttags von GridView aus. Ziehen Sie ein Label-websteuer Element auf die `ItemTemplate`, legen Sie dessen `ID`-Eigenschaft auf `RoleNameLabel`fest, und konfigurieren Sie die zugehörigen Datenbindung-Einstellungen so, dass die `Text`-Eigenschaft an `Container.DataItem`gebunden ist.

Unabhängig davon, welchen Ansatz Sie verwenden, sollte das resultierende deklarative Markup der GridView in etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Der Inhalt des Arrays wird mithilfe der Datenbindung-Syntax `<%# Container.DataItem %>`angezeigt. Eine ausführliche Beschreibung, warum diese Syntax verwendet wird, wenn der Inhalt eines Arrays, das an die GridView gebunden ist, angezeigt wird, geht über den Rahmen dieses Tutorials hinaus. Weitere Informationen zu diesem Thema finden Sie unter [Binden eines skalaren Arrays an ein datenweb-Steuer](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)Element.

Derzeit wird das `RoleList` GridView nur an die Liste der Rollen gebunden, wenn die Seite zum ersten Mal besucht wird. Das Raster muss immer dann aktualisiert werden, wenn eine neue Rolle hinzugefügt wird. Um dies zu erreichen, aktualisieren Sie den `Click`-Ereignishandler der `CreateRoleButton` Schaltfläche, damit die `DisplayRolesInGrid`-Methode aufgerufen wird, wenn eine neue Rolle erstellt wird.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Wenn der Benutzer nun eine neue Rolle hinzufügt, zeigt der `RoleList` GridView die soeben hinzugefügte Rolle beim Postback an und bietet visuelles Feedback, dass die Rolle erfolgreich erstellt wurde. Um dies zu veranschaulichen, besuchen Sie die Seite `ManageRoles.aspx` über einen Browser, und fügen Sie eine Rolle mit dem Namen Supervisor hinzu. Wenn Sie auf die Schaltfläche Rolle erstellen klicken, wird ein Postback durchführt, und das Raster wird aktualisiert, um sowohl Administratoren als auch die neue Rolle, Supervisor, einzubeziehen.

[![die Rolle "Supervisoren" wurde hinzugefügt.](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Abbildung 6**: die Rolle "Supervisoren" wurde hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image18.png))

## <a name="step-6-deleting-roles"></a>Schritt 6: Löschen von Rollen

An diesem Punkt kann ein Benutzer eine neue Rolle erstellen und alle vorhandenen Rollen auf der Seite `ManageRoles.aspx` anzeigen. Gestatten Sie Benutzern, auch Rollen zu löschen. Die `Roles.DeleteRole`-Methode verfügt über zwei über Ladungen:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) : Löscht die Rolle *roleName*. Eine Ausnahme wird ausgelöst, wenn die Rolle mindestens ein Element enthält.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) : Löscht die Rolle *roleName*. Wenn *throwonpopulaterole* `true`ist, wird eine Ausnahme ausgelöst, wenn die Rolle mindestens ein Element enthält. Wenn *throwonpopulaterole* `false`ist, wird die Rolle gelöscht, unabhängig davon, ob Sie Elemente enthält oder nicht. Intern ruft die `DeleteRole(roleName)`-Methode `DeleteRole(roleName, true)`auf.

Die `DeleteRole`-Methode löst auch eine Ausnahme aus, wenn *roleName* `null` oder eine leere Zeichenfolge ist oder wenn *roleName* ein Komma enthält. Wenn *roleName* nicht im System vorhanden ist, wird `DeleteRole` im Hintergrund fehlschlagen, ohne dass eine Ausnahme ausgelöst wird.

Erweitern Sie nun die GridView in `ManageRoles.aspx`, um eine Schaltfläche "Löschen" einzuschließen, mit der die ausgewählte Rolle gelöscht wird, wenn darauf geklickt wird. Fügen Sie der GridView zunächst eine Lösch Schaltfläche hinzu, indem Sie zum Dialogfeld Felder navigieren und eine DELETE-Schaltfläche hinzufügen, die sich unter der CommandField-Option befindet. Erstellen Sie die Schaltfläche Löschen in der Spalte ganz links, und legen Sie die `DeleteText`-Eigenschaft auf Rolle löschen fest.

[![der rolelist-GridView eine Lösch Schaltfläche Hinzufügen](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Abbildung 7**: Hinzufügen einer Lösch Schaltfläche zur `RoleList` GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-and-managing-roles-cs/_static/image21.png))

Nach dem Hinzufügen der Schaltfläche Löschen sollte das deklarative Markup Ihrer GridView in etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das `RowDeleting` Ereignis der GridView. Dies ist das Ereignis, das beim Postback ausgelöst wird, wenn auf die Schaltfläche "Rolle löschen" geklickt wird. Fügen Sie dem Ereignishandler folgenden Code hinzu.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Der Code beginnt mit einem programmgesteuerten Verweis auf das `RoleNameLabel`-websteuer Element in der Zeile, auf deren Schaltfläche "Rolle löschen" geklickt wurde. Anschließend wird die `Roles.DeleteRole`-Methode aufgerufen, die die `Text` der `RoleNameLabel` und `false`übergibt und dadurch die Rolle löscht, unabhängig davon, ob Benutzer der Rolle zugeordnet sind. Zum Schluss wird die `RoleList` GridView aktualisiert, sodass die soeben gelöschte Rolle nicht mehr im Raster angezeigt wird.

> [!NOTE]
> Die Schaltfläche Rolle löschen erfordert keine Bestätigung vom Benutzer, bevor die Rolle gelöscht wird. Eine der einfachsten Möglichkeiten, eine Aktion zu bestätigen, ist das Dialogfeld Client seitiges bestätigen. Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen der Client seitigen Bestätigung beim Löschen](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

## <a name="summary"></a>Summary

Viele Webanwendungen verfügen über bestimmte Autorisierungs Regeln oder Funktionen auf Seitenebene, die nur für bestimmte Benutzer Klassen verfügbar sind. Beispielsweise kann eine Gruppe von Webseiten vorhanden sein, auf die nur Administratoren zugreifen können. Anstatt diese Autorisierungs Regeln auf Benutzerbasis zu definieren, empfiehlt es sich, die Regeln basierend auf einer Rolle zu definieren. Das heißt, dass Benutzer Scott und jisun nicht explizit auf die administrativen Webseiten zugreifen können. ein besser verwaltbarerer Ansatz besteht darin, Mitgliedern der Administrator Rolle den Zugriff auf diese Seiten zu gestatten und dann Scott und jisun als Benutzer anzugeben, die zum Administrator Rolle.

Das Rollen Framework erleichtert das Erstellen und Verwalten von Rollen. In diesem Tutorial wurde die Konfiguration des Rollen-Frameworks für die Verwendung der `SqlRoleProvider`erläutert, die eine Microsoft SQL Server-Datenbank als Rollen Speicher verwendet. Wir haben auch eine Webseite erstellt, auf der die vorhandenen Rollen im System aufgelistet sind und die das Erstellen neuer Rollen und das Löschen vorhandener Rollen ermöglicht. In den nachfolgenden Tutorials erfahren Sie, wie Sie Benutzern Rollen zuweisen und wie Sie die rollenbasierte Autorisierung anwenden.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Überprüfen der Mitgliedschaft, Rollen und Profile von ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Vorgehensweise: Verwenden des Rollen-Managers in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Rollen Anbieter](https://msdn.microsoft.com/library/aa478950.aspx)
- [Ein eigenes Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Technische Dokumentation für das `<roleManager>`-Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Verwenden der Mitgliedschafts-und Rollen-Manager-APIs](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Zu den führenden Reviewern für dieses Tutorial zählen Alicja Maziarz, Suchi Banerjee und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](assigning-roles-to-users-cs.md)
