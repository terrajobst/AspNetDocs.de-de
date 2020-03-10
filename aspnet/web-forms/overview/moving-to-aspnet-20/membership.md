---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Mitgliedschaft | Microsoft-Dokumentation
author: microsoft
description: Die ASP.NET-Mitgliedschaft basiert auf dem Erfolg des Formular Authentifizierungs Modells von ASP.NET 1. x. Die ASP.NET-Formular Authentifizierung bietet eine bequeme Möglichkeit zum incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: da6fc205bd852a818d65425586cec38fdb08d310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521703"
---
# <a name="membership"></a>Mitgliedschaft

von [Microsoft](https://github.com/microsoft)

> Die ASP.NET-Mitgliedschaft basiert auf dem Erfolg des Formular Authentifizierungs Modells von ASP.NET 1. x. Die ASP.NET-Formular Authentifizierung bietet eine bequeme Möglichkeit, ein Anmeldeformular in Ihre ASP.NET-Anwendung einzubinden und Benutzer anhand einer Datenbank oder eines anderen Datenspeicher zu überprüfen.

Die ASP.NET-Mitgliedschaft basiert auf dem Erfolg des Formular Authentifizierungs Modells von ASP.NET 1. x. Die ASP.NET-Formular Authentifizierung bietet eine bequeme Möglichkeit, ein Anmeldeformular in Ihre ASP.NET-Anwendung einzubinden und Benutzer anhand einer Datenbank oder eines anderen Datenspeicher zu überprüfen. Die Member der FormsAuthentication-Klasse ermöglichen das Behandeln von Cookies für die Authentifizierung, das Überprüfen auf eine gültige Anmeldung, das Protokollieren eines Benutzers usw. Das Implementieren der Formular Authentifizierung in einer ASP.NET 1. x-Anwendung kann jedoch eine ziemlich große Menge an Code erfordern.

Die Mitgliedschaft in ASP.NET 2,0 ist ein wichtiger Schritt bei der Verwendung der Formular Authentifizierung. (Die Mitgliedschaft ist am stärksten, wenn Sie mit der Formular Authentifizierung gekoppelt ist, aber die Verwendung der Formular Authentifizierung ist nicht zwingend erforderlich.) Wie Sie sehen werden, können Sie die ASP.NET-Mitgliedschaft und die Anmeldungs Steuerelemente in ASP.NET 2,0 verwenden, um ein leistungsfähiges Mitgliedschaftssystem zu implementieren, ohne viel Code schreiben zu müssen.

## <a name="implementing-membership-in-aspnet-20"></a>Implementieren der Mitgliedschaft in ASP.NET 2,0

Die Mitgliedschaft wird durch die folgenden vier Schritte implementiert. Beachten Sie, dass es viele untergeordnete Schritte und eine optionale Konfiguration gibt, die ebenfalls implementiert werden können. Diese Schritte sollen den großen Überblick über die Konfiguration der Mitgliedschaft veranschaulichen.

1. Erstellen Sie Ihre Mitgliedschafts Datenbank (wenn SQL Server als Mitgliedschafts Speicher verwendet wird.)
2. Geben Sie die Mitgliedschafts Optionen in ihren Anwendungs Konfigurationsdateien an. (Die Mitgliedschaft ist standardmäßig aktiviert.)
3. Bestimmen Sie den Typ des Mitgliedschafts Stores, den Sie verwenden möchten. Folgende Optionen sind verfügbar: 

    - Microsoft SQL Server (Version 7,0 oder höher)
    - Active Directory Speicher
    - Benutzerdefinierter Mitgliedschafts Anbieter
4. Konfigurieren Sie die Anwendung für die ASP.NET-Formular Authentifizierung. Auch hier ist die Mitgliedschaft so konzipiert, dass die Formular Authentifizierung genutzt wird, aber die Verwendung der Formular Authentifizierung ist keine Voraussetzung.
5. Definieren von Benutzerkonten für die Mitgliedschaft und Konfigurieren von Rollen, falls gewünscht.

## <a name="creating-the-membership-database"></a>Erstellen der Mitgliedschafts Datenbank

Wenn Sie SQL Server 7,0 oder höher als Mitgliedschafts Speicher verwenden, können Sie das ASPNET\_RegSql-Hilfsprogramm verwenden (das am einfachsten über die Eingabeaufforderung von Visual Studio .net 2005 verfügbar ist), um Ihre Datenbank zu konfigurieren. Das ASPNET\_RegSql-Hilfsprogramm kann als Eingabe Aufforderungs Tool oder über einen GUI-Assistenten verwendet werden. Die Assistenten-Methode ist die einfachste Möglichkeit zum Konfigurieren Ihrer Datenbank. Um auf den Assistenten zuzugreifen, führen Sie einfach den folgenden Befehl aus:

`aspnet_regsql W`

Nachdem Sie diesen Befehl ausgeführt haben, wird der Setup-Assistent für ASP.NET SQL Server wie unten dargestellt angezeigt.

![](membership/_static/image1.jpg)

**Abbildung 1**

Der Setup-Assistent für ASP.NET SQL Server erstellt die Website in der-Instanz, die Sie im Assistenten angeben. Allerdings verwendet ASP.net die Verbindungs Zeichenfolge in der Datei "Machine. config", um eine Verbindung mit Ihrer Datenbank herzustellen. Standardmäßig verweist diese Verbindungs Zeichenfolge auf eine SQL Server 2005-Instanz. Wenn Sie also eine SQL Server 2000-oder SQL Server 7,0-Instanz verwenden, müssen Sie die Verbindungs Zeichenfolge in der Datei Machine. config ändern. Diese Verbindungs Zeichenfolge kann hier angezeigt werden:

[!code-xml[Main](membership/samples/sample1.xml)]

Wenn Sie die Verbindungs Zeichenfolge nicht ändern, gibt ASP.net Ihnen keinen beschreibenden Fehler. Es wird nur noch die Meldung angezeigt, dass Sie die Datenbank noch nicht erstellt haben. Im obigen Fall habe ich die Verbindungs Zeichenfolge so geändert, dass Sie auf meine lokale SQL Server 2000-Instanz verweist.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Angeben der Konfiguration und Hinzufügen von Benutzern und Rollen

Der nächste Schritt bei der Konfiguration der Mitgliedschaft besteht darin, die erforderlichen Informationen der Datei "Web. config" der Anwendung hinzuzufügen. In ASP.NET 1. x war das Ändern der Datei "Web. config" aufgrund der Verwendung von "lowercamelcase" und dem Fehlen von IntelliSense manchmal schwierig. Visual Studio .net 2005 vereinfacht die Aufgabe mit IntelliSense für Konfigurationsdateien, aber ASP.NET 2,0 geht noch einen Schritt weiter, indem eine Webschnittstelle zum Bearbeiten von Konfigurationsdateien bereitgestellt wird.

Sie können die Weboberfläche starten, indem Sie auf der Projektmappen-Explorer Symbolleiste auf die Schaltfläche ASP.net Configuration klicken, wie unten gezeigt. Sie können die Weboberfläche auch über Popups starten, die angezeigt werden, wenn Anmelde Steuerelemente eingefügt werden.

![](membership/_static/image2.jpg)

**Abbildung 2**

Dadurch wird das unten gezeigte Websiteverwaltungs-Tool ASP.net gestartet. Bei der ASP.NET-Website Verwaltung handelt es sich um eine Schnittstelle mit vier Registerkarten, die die Verwaltung von Anwendungseinstellungen erleichtert. Die folgenden Registerkarten sind verfügbar:

- **Startseite**
- **Sicherheit** Benutzer, Rollen und Zugriff konfigurieren.
- **Anwendung** Konfigurieren Sie die Anwendungseinstellungen.
- **Anbieter** Konfigurieren und testen Sie Ihren Anwendungs Mitgliedschafts Anbieter.

Mit dem Websiteverwaltungs-Tool können Sie problemlos neue Benutzer erstellen, neue Rollen erstellen und Benutzer und Rollen verwalten. Diese Möglichkeit ist in der Windows-Benutzeroberfläche nicht verfügbar. Mithilfe der Windows-Benutzeroberfläche können Sie einfach Autorisierungs Einstellungen definieren und Anbieter, Funktionen, die nicht im Websiteverwaltungs-Tool sind, hinzufügen, löschen und verwalten.

Öffnen Sie zum Starten der Windows-Benutzeroberfläche das Snap-in "Internetinformationsdienste", klicken Sie mit der rechten Maustaste auf die Anwendung, und wählen Sie Eigenschaften aus. Klicken Sie auf die Registerkarte ASP.net und anschließend auf die Schaltfläche Konfiguration bearbeiten. (Die Anwendung muss unter ASP.NET 2,0 ausgeführt werden, damit die Schaltfläche "Konfiguration bearbeiten" aktiviert wird. Sie können auch die ASP.NET-Version im Dialogfeld ASP.net konfigurieren.) Das Dialogfeld ASP.NET-Konfigurationseinstellungen wird wie unten dargestellt angezeigt.

![](membership/_static/image3.jpg)

**Abbildung 3**

Auf der Registerkarte Allgemein werden Verbindungs Zeichenfolgen und Anwendungseinstellungen aufgelistet. Alle Einstellungen in kursiv Schrift werden in einer übergeordneten Konfigurationsdatei ("Machine. config" oder "Web. config" auf einer höheren Ebene) und Einstellungen, die nicht kursiv sind, in der Anwendungs Konfigurationsdatei definiert. Wenn eine Einstellung auf Anwendungsebene hinzugefügt, entfernt oder bearbeitet wird, wird die Einstellung von ASP.net auf Anwendungsebene "Web. config" hinzugefügt, entfernt oder geändert, anstatt die Einstellung aus der Konfigurationsdatei zu entfernen, von der Sie geerbt wurde.

Die Registerkarte Authentifizierung wird unten angezeigt. Hier konfigurieren Sie Ihre Mitgliedschafts Einstellungen. Hier können Formular Authentifizierungs Einstellungen, Mitgliedschafts Anbieter und Rollen Anbieter konfiguriert werden.

![](membership/_static/image4.jpg)

**Abbildung 4**

## <a name="implementing-membership-in-your-application"></a>Implementieren der Mitgliedschaft in der Anwendung

Die einfachste Möglichkeit, die ASP.NET 2,0-Mitgliedschaft in Ihre Anwendung zu implementieren, ist die Verwendung der bereitgestellten Logon-Steuerelemente. Mit dieser Methode können Sie die Grundlagen der ASP.NET 2,0-Mitgliedschaft implementieren, ohne Code schreiben zu müssen.

Die folgenden Anmelde Steuerelemente sind in ASP.NET 2,0 verfügbar:

## <a name="login-control"></a>Login-Steuerelement

Das-Anmelde Steuerelement stellt eine Benutzeroberfläche für die Anmeldung an Ihrem Mitgliedschaftssystem bereit. Sie erhalten ein Textfeld mit einem Benutzernamen und einem Kennwort sowie eine Anmelde Schaltfläche. Viele weitere allgemeine Features, wie z. b. ein Link zum Registrieren für Personen, die noch nicht ausgeführt wurden, ein Kontrollkästchen, mit dem sich der Benutzer automatisch bei nachfolgenden Besuchen anmelden kann, ein Link zur Kenn Wort Erinnerung usw. Alle Funktionen des Anmeldungs Steuer Elements können über die Eigenschaften des-Steuer Elements angepasst werden.

In ASP.NET 1. x mussten Entwickler beim Verwenden der Formular Authentifizierung eine große Menge an Code schreiben, um eine Suche durchzuführen. Mit der ASP.NET 2,0-Mitgliedschaft können Sie Benutzer überprüfen, ohne Code schreiben zu müssen. ASP.NET führt den Benutzer automatisch für Sie durch. (Wenn Sie das Login-Steuerelement ohne Verwendung der ASP.NET-Mitgliedschaft verwenden, können Sie die **OnAuthenticate** -Methode verwenden, um den Benutzer zu validieren.)

## <a name="loginview-control"></a>LoginView-Steuerelement

Das LoginView-Steuerelement ist ein auf Vorlagen basiertes Steuerelement, das standardmäßig zwei Vorlagen bereitstellt. AnonymousTemplate und LoggedInTemplate. Welche Vorlage angezeigt wird, hängt davon ab, ob der Benutzer beim Mitgliedschaftssystem angemeldet ist. Dieses Steuerelement wird normalerweise verwendet, um ein Login-Steuerelement anzuzeigen, wenn sich ein Benutzer noch nicht angemeldet hat, und ein LoginStatus-Steuerelement und/oder andere Anmelde Steuerelemente, wenn sich der Benutzer angemeldet hat. Wenn Sie die Rollen Verwaltung in Ihrer ASP.NET-Anwendung verwenden, kann das LoginView-Steuerelement eine bestimmte Vorlage basierend auf der Rolle "Benutzer" anzeigen. (Weitere Informationen zur ASP.NET-Rollen Verwaltung werden zu einem späteren Zeitpunkt behandelt.)

## <a name="passwordrecovery-control"></a>PasswordRecovery-Steuerelement

Das PasswordRecovery-Steuerelement ermöglicht es Benutzern, eine e-Mail mit Ihrem aktuellen Kennwort zu erhalten oder das Kennwort zurückzusetzen. Klartext und verschlüsselte Kenn Wörter können wieder hergestellt und per e-Mail an Benutzer gesendet werden. Wenn für das Kennwort ein Hashwert verwendet wird, kann es nicht wieder hergestellt werden. Stattdessen muss der Benutzer eine Kenn Wort Zurücksetzung durchführen.

## <a name="loginstatus-control"></a>LoginStatus-Steuerelement

Das LoginStatus-Steuerelement wird verwendet, um einen Anmelde Indikator für Benutzer anzuzeigen, die nicht angemeldet sind, und einen Abmelde Indikator für Benutzer, die zurzeit angemeldet sind. Die Eigenschaft "Request. IsAuthenticated" wird verwendet, um zu bestimmen, welcher Indikator angezeigt werden soll. Der vom LoginStatus-Steuerelement angezeigte Indikator kann Text (implementiert über die Eigenschaften " **LoginText** " und " **LogoutText** ") oder Bilder (implementiert über die Eigenschaften " **LoginImageUrl** " und " **LogoutImageUrl** ") sein.

Wenn sich ein Benutzer über das LoginStatus-Steuerelement anmeldet, wird er an die von der **LogoutPageUrl** -Eigenschaft angegebene URL umgeleitet. Wenn diese Eigenschaft nicht festgelegt ist, wird die aktuelle Seite aktualisiert. Da die Site wahrscheinlich durch die Formular Authentifizierung geschützt ist, leitet die Aktualisierung der aktuellen Seite den Benutzer zur Anmeldeseite für die Website um.

## <a name="loginname-control"></a>LoginName-Steuerelement

Das LoginName-Steuerelement zeigt den Benutzernamen des Benutzers an, der derzeit bei der Website angemeldet ist.

## <a name="createuserwizard-control"></a>Steuerelement "kreateuserwizard"

Das Steuerelement "kreateuserwizard" bietet Benutzern eine bequeme Möglichkeit, sich für Ihr Mitgliedschaftssystem zu registrieren. Mithilfe der unten gezeigten Schnittstelle können Sie Schritte hinzufügen (als Sammlung von WizardSteps implementiert).

![](membership/_static/image5.jpg)

**Abbildung 5**

Der "kreateuserwizard" ist ein auf Vorlagen basiertes Steuerelement, das von der Wizard-Klasse abgeleitet ist und die folgenden Vorlagen bereitstellt:

- **HeaderTemplate** Mit dieser Vorlage wird die Darstellung des Headers des Assistenten gesteuert.
- **SideBarTemplate** Mit dieser Vorlage wird die Darstellung der Rand Leiste des Assistenten gesteuert.
- **StartNavigationTemplate** Mit dieser Vorlage wird gesteuert, wie die Navigation im Assistenten beim Start Schritt ausgeführt wird.
- **StepNavigationTemplate** Diese Vorlage steuert die Darstellung des Navigationsbereichs, wenn dies nicht der Start-oder Abschluss Schritt ist.
- **FinishNavigationTemplate** Diese Vorlage steuert die Darstellung des Navigationsbereichs, wenn der Schritt abgeschlossen ist.

Außerdem erstellt ASP.net für jeden Schritt, den Sie dem Assistenten hinzufügen, eine benutzerdefinierte Vorlage, die für diesen Schritt sowohl eine ContentTemplate als auch eine CustomNavigationTemplate enthält. Ausführliche Informationen zum Anpassen von "anateuserwizard" finden Sie in der Dokumentation zu vs.net 2005:

## <a name="changepassword-control"></a>ChangePassword-Steuerelement

Das ChangePassword-Steuerelement ermöglicht es Benutzern, das Kennwort zu ändern. Wenn die Display Name-Eigenschaft den Wert true hat (standardmäßig false), kann der Benutzer sein Kennwort ändern, wenn er nicht angemeldet ist. Wenn der Benutzer bereits angemeldet *ist* und die Display Name-Eigenschaft auf true festgelegt ist, kann der Benutzer das Kennwort eines anderen Benutzers ändern, der nicht angemeldet ist und die Benutzer-ID dieses Benutzers kennt.

Beachten Sie Folgendes: Wenn Sie möchten, dass Benutzer Kenn Wörter ohne Anmeldung ändern können, müssen Sie sicherstellen, dass die Seite, auf der das ChangePassword-Steuerelement angezeigt wird, anonymen Zugriff zulässt. Natürlich müssen Benutzer Ihr altes Kennwort angeben, um Ihr Kennwort zu ändern.

## <a name="role-management"></a>Rollenverwaltung

Mithilfe der Rollen Verwaltung können Sie Benutzer einer bestimmten Rolle zuweisen und dann den Zugriff auf bestimmte Dateien oder Ordner basierend auf dieser Rolle einschränken. Die Rollen Verwaltung bietet auch eine API, sodass Sie Programm gesteuert eine Rolle bestimmen oder alle Benutzer in einer bestimmten Rolle ermitteln und entsprechend reagieren können.

Die Rollen Verwaltung ist keine Voraussetzung für die ASP.NET-Mitgliedschaft, und die Mitgliedschaft ist keine Voraussetzung für die Verwendung der Rollen Verwaltung. Die beiden ergänzen sich jedoch gut, und es ist wahrscheinlich, dass Sie von Entwicklern gemeinsam verwendet werden.

Nehmen Sie die folgende Änderung in der Datei "Web. config" vor, um die Rollen Verwaltung in Ihrer Anwendung zu aktivieren:

[!code-xml[Main](membership/samples/sample2.xml)]

Wenn das **CacheRolesInCookie** -Attribut auf "true" festgelegt ist, speichert ASP.net eine Benutzer Rollen Mitgliedschaft in einem Cookie auf dem Client. Dadurch können Rollen Suchvorgänge ohne Aufrufe an den RoleProvider erfolgen. Bei der Verwendung dieses Attributs wird Entwicklern empfohlen, sicherzustellen, dass das **cookieProtection** -Attribut auf all festgelegt ist. (Dies ist die Standardeinstellung.) Dadurch wird sichergestellt, dass die Cookiedaten verschlüsselt werden und sichergestellt werden, dass der Inhalt der Cookies nicht geändert wurde. Rollen können mithilfe des Websiteverwaltungs-Tools hinzugefügt werden. Sie ermöglicht Ihnen das einfache definieren von Rollen, das Konfigurieren des Zugriffs auf Teile der Website auf der Grundlage dieser Rollen und das Zuweisen von Benutzern zu Rollen.

![](membership/_static/image6.jpg)

**Abbildung 6**

Wie oben gezeigt, können neue Rollen hinzugefügt werden, indem Sie einfach den Namen der Rolle eingeben und dann auf Rolle hinzufügen klicken. Vorhandene Rollen können durch Klicken auf den entsprechenden Link in der Liste der vorhandenen Rollen verwaltet oder gelöscht werden.

Wenn Sie eine Rolle verwalten, können Sie Benutzer wie unten gezeigt hinzufügen oder entfernen.

![](membership/_static/image7.jpg)

**Abbildung 7**

Durch Aktivieren des Kontrollkästchens "Benutzer ist in Rolle" können Sie problemlos einen Benutzer zu einer bestimmten Rolle hinzufügen. ASP.NET aktualisiert Ihre Mitgliedschafts Datenbank automatisch mit den entsprechenden Einträgen. Außerdem sollten Sie Zugriffsregeln für Ihre Anwendung konfigurieren. ASP.NET 1. x-Entwickler sind mit dem &lt;Authorization&gt;-Element in der Datei "Web. config" vertraut, und diese Option ist weiterhin in ASP.NET 2,0 verfügbar. Es ist jedoch einfacher, Zugriffsregeln mithilfe des Websiteverwaltungs-Tools zu verwalten, wie unten gezeigt.

![](membership/_static/image8.jpg)

**Abbildung 8**

In diesem Fall wird der Verwaltungs Ordner hervorgehoben (er ist schwer zu erkennen, da das Tool ihn in hellgrau hervorgehoben hat), und der Administrator Rolle wurde der Zugriff gewährt. Alle anderen Benutzer werden verweigert. Sie können auf das Head-Symbol klicken, um eine Regel auszuwählen, und dann die Schaltflächen nach oben und nach unten verwenden, um die Regeln anzuordnen. Wie beim ASP.net-&lt;Authorization&gt;-Element werden Regeln in der Reihenfolge verarbeitet, in der Sie angezeigt werden. Anders ausgedrückt: Wenn die Reihenfolge der Regeln im obigen Screenshot umgekehrt wäre, hätte niemand Zugriff auf den Ordner "Verwaltung", da die erste Regel, die ASP.net finden würde, die Regel ist, die allen Benutzern den Ordner verweigert.

ASP.NET 2,0 fügt dem Ordner, für den Sie eine Zugriffs Regel angeben, eine Web. config-Datei hinzu. Zugriffsregeln können über die Konfigurationsdatei oder über das Websiteverwaltungs-Tool bearbeitet werden. Anders ausgedrückt: das Websiteverwaltungs-Tool ist einfach eine Schnittstelle, über die die Konfigurationsdatei in einer benutzerfreundlichen Umgebung bearbeitet werden kann.

## <a name="using-roles-in-code"></a>Verwenden von Rollen im Code

Die API für die Rollen Verwaltung wurde seit Version 1. x nicht geändert. Die **IsInRole** -Methode wird verwendet, um zu bestimmen, ob ein Benutzer einer bestimmten Rolle gehört.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET erstellt auch eine RolePrincipal-Instanz als Member des aktuellen Kontexts. Das RolePrincipal-Objekt kann verwendet werden, um alle Rollen zu erhalten, zu denen der Benutzer gehört:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Verwenden von RoleGroups mit dem LoginView-Steuerelement

Nachdem Sie nun mit der Rollen Verwaltung und-Mitgliedschaft vertraut sind, können Sie kurz darauf eingehen, wie das LoginView-Steuerelement diese Funktion in ASP.NET 2,0 nutzt. Wie bereits erwähnt, ist das LoginView-Steuerelement ein auf Vorlagen basiertes Steuerelement, das standardmäßig zwei Vorlagen enthält. AnonymousTemplate und LoggedInTemplate. Im Dialogfeld "LoginView-Aufgaben" befindet sich ein Link (unten dargestellt), mit dem Sie RoleGroups bearbeiten können.

![](membership/_static/image9.jpg)

**Abbildung 9**

Jedes RoleGroup-Objekt enthält ein Array von Zeichen folgen, das definiert, für welche Rollen RoleGroup gilt. Um dem LoginView-Steuerelement eine neue RoleGroup hinzuzufügen, klicken Sie auf den Link RoleGroups bearbeiten. In der obigen Abbildung können Sie sehen, dass ich eine neue RoleGroup für Administratoren hinzugefügt habe. Wenn Sie in der Dropdown Liste Sichten die Option RoleGroup (RoleGroup [0]) auswählen, können Sie eine Vorlage konfigurieren, die nur Mitgliedern der Rolle "Administratoren" angezeigt wird. In der folgenden Abbildung wurde eine neue RoleGroup hinzugefügt, die für Mitglieder der Rolle "Sales" und der Rolle "Distribution" gilt. Dadurch wird der Dropdown Liste "Ansichten" im Dialogfeld "LoginView Tasks" eine zweite RoleGroup hinzugefügt, und alle dieser Vorlage hinzugefügten Elemente werden von jedem Benutzer in der Rolle "Sales" oder "Distribution" angezeigt.

![](membership/_static/image10.jpg)

**Abbildung 10**

## <a name="overriding-the-existing-membership-provider"></a>Überschreiben des vorhandenen Mitgliedschafts Anbieters

Es gibt mehrere Möglichkeiten, die Funktionalität der ASP.NET-Mitgliedschaft zu erweitern. Erstens können Sie die vorhandene Funktionalität der sqlmembership shipprovider-Klasse offensichtlich ändern, indem Sie von ihr erben und deren Methoden überschreiben. Wenn Sie z. b. ihre eigene Funktionalität implementieren möchten, wenn Benutzer erstellt werden, können Sie eine eigene Klasse erstellen, die wie folgt von sqlmembership shipprovider erbt:

[!code-csharp[Main](membership/samples/sample5.cs)]

Wenn Sie andererseits einen eigenen Anbieter erstellen möchten (z. b. zum Speichern Ihrer Mitgliedschafts Informationen in einer Access-Datenbank), können Sie einen eigenen Anbieter erstellen.

## <a name="creating-your-own-membership-provider"></a>Erstellen eines eigenen Mitgliedschafts Anbieters

Um einen eigenen Mitgliedschafts Anbieter zu erstellen, müssen Sie zuerst eine Klasse erstellen, die von der Mitgliedschafts Anbieter Klasse erbt. Wenn Sie VB.NET verwenden, fügt Visual Studio 2005 die stubweise für alle Methoden hinzu, die Sie überschreiben müssen. Wenn Sie verwenden C#, können Sie die stusb hinzufügen.

Sie müssen Folgendes überschreiben:

- ApplicationName-Eigenschaft
- ChangePassword-Funktion
- Changepasswordfrag andanswer-Funktion
- Funktion "Funktion"
- DeleteUser-Funktion
- EnablePasswordReset (Eigenschaft)
- EnablePasswordRetrieval (Eigenschaft)
- FindUsersByEmail-Funktion
- FindUsersByName-Funktion
- GetAllUsers-Funktion
- Getnumofusersonline-Funktion
- GetPassword-Funktion
- GetUser-Funktion
- GetUserNameByEmail-Funktion
- MaxInvalidPasswordAttempts (Eigenschaft)
- Minrequirements dnonalpha anumschlag Character (Eigenschaft)
- Minrequirements dpasswordlength (Eigenschaft)
- PasswordAttemptWindow (Eigenschaft)
- PasswordFormat (Eigenschaft)
- Passwordfestiregularexpression (Eigenschaft)
- Requirements/Answer (Eigenschaft)
- Requirements suniqueemail (Eigenschaft)
- ResetPassword-Funktion
- Entsperren der Benutzerfunktion
- UpdateUser-Funktion
- ValidateUser-Funktion

Dies ist eine Liste, die als C# Entwickler implementiert werden muss. Möglicherweise ist es einfacher, die-Klasse in VB.net ohne Implementierung zu erstellen, und dann den .net-Reflektor oder ein ähnliches Tool zu C#verwenden, um den Code in zu konvertieren.

Die Verbindungs Zeichenfolge und andere Eigenschaften sollten in der Initialize-Methode auf ihre Standardwerte festgelegt werden. (Die Initialize-Methode wird ausgelöst, wenn der Anbieter zur Laufzeit geladen wird.) Der zweite Parameter für die Initialize-Methode ist vom Typ System. Collections. Specialized. NameValueCollection und ist ein Verweis auf das &lt;&gt; Element hinzufügen, das Ihrem benutzerdefinierten Anbieter in der Datei "Web. config" zugeordnet ist. Dieser Eintrag sieht wie folgt aus:

[!code-xml[Main](membership/samples/sample6.xml)]

Im folgenden finden Sie ein Beispiel für die Initialize-Methode.

[!code-csharp[Main](membership/samples/sample7.cs)]

Um den Benutzer zu validieren, wenn er Ihr Anmeldeformular sendet, müssen Sie die ValidateUser-Methode verwenden. Diese Methode wird ausgelöst, wenn der Benutzer im Anmelde Steuerelement auf die Anmelde Schaltfläche klickt. Sie platzieren den Code, der die Benutzersuche in dieser Methode durchführt.

Wie Sie sehen können, ist das Schreiben Ihres eigenen Mitgliedschafts Anbieters nicht schwierig und ermöglicht Ihnen das Erweitern dieser leistungsstarken Funktionalität von ASP.NET 2,0.
